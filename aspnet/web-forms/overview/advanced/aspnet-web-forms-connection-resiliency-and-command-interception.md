---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Form 連線恢復功能和命令攔截 |Microsoft Docs
author: Erikre
description: 本教學課程說明如何修改範例應用程式支援連接恢復功能和命令攔截。
ms.author: aspnetcontent
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 9a66b297608a5a8cd536b9af2a9ae4bb600a6bbb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807085"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Form 連線恢復功能和命令攔截
====================
藉由[Erik Reitan](https://github.com/Erikre)

在本教學課程中，您將修改 「 Wingtip Toys 範例應用程式支援連接恢復功能和命令攔截。 藉由啟用連接恢復功能，Wingtip Toys 範例應用程式會自動重試的資料呼叫一般的雲端環境的暫時性錯誤發生時。 此外，藉由實作命令攔截，Wingtip Toys 範例應用程式會攔截所有傳送至資料庫，以記錄或進行變更的 SQL 查詢。

> [!NOTE] 
> 
> 此 Web Form 教學課程已根據 Tom Dykstra 下列 MVC 教學課程：  
> [連線恢復功能和命令攔截與 Entity Framework 中的 ASP.NET MVC 應用程式](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>您將學到什麼：

- 如何提供連接恢復功能。
- 如何實作命令攔截。

## <a name="prerequisites"></a>必要條件

在開始之前，請確定您已在電腦上安裝下列軟體：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或是[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 .NET Framework 會自動安裝。
- Wingtip Toys 的範例專案，以便您可以實作在本教學課程，Wingtip Toys 專案中所述的功能。 下列連結提供下載詳細資料：

    - [Getting Started with ASP.NET 4.5.1 Web Form-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- 之前完成本教學課程，請考慮檢閱相關的教學課程系列[Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 教學課程系列可協助您熟悉**WingtipToys**專案和程式碼。

## <a name="connection-resiliency"></a>連線恢復功能

當您考慮應用程式部署到 Windows Azure 時，要考慮的其中一個選項部署的資料庫**Windows** **Azure SQL Database**，雲端資料庫服務。 當您連接到與您的 web 伺服器和資料庫伺服器會直接連接一起在相同的資料中心的雲端資料庫服務，暫時性連接錯誤是通常會更頻繁。 即使在相同的資料中心裝載雲端 web 伺服器和雲端資料庫服務，有更多的網路連線，它們可能會發生問題，例如負載平衡器之間。

也雲端服務通常由其他使用者共用，這表示回應速度可能會受到這些。 與您資料庫的存取權可能受到節流。 節流方式的資料庫服務擲回例外狀況時您嘗試存取它更頻繁地超過所允許在您*服務等級協定*(SLA)。

您正在存取雲端服務時，會發生的最大或最多連線問題是暫時性，也就是他們的解析本身一段時間。 因此當您嘗試在資料庫作業，並取得一種是通常是暫時性的錯誤，您可以再次嘗試操作之後不久和作業可能會成功。 如果您藉由自動嘗試一次，處理暫時性錯誤，您可以為您的使用者提供更好的體驗對客戶進行大部分都不可見。 連接恢復功能，在 Entity Framework 6 中，會自動執行程序的重試失敗的 SQL 查詢。

連接恢復功能必須適當地設定特定的資料庫服務：

1. 編譯器必須知道哪一個例外狀況有可能是暫時性的。 您想要重試因暫時遺失網路連線能力錯誤，例如程式 bug 所造成的錯誤。
2. 它必須等候適當的失敗作業的重試之間的時間量。 您可以再重試之間等待的批次程序比使用者等待回應的位置線上 web 網頁。
3. 它具有適當數目的次數之前放棄重試一次。 您可以在批次程序，就像是在線上應用程式重試一次。

您可以設定這些設定，以手動方式針對 Entity Framework 提供者支援的任何資料庫環境。

您只需要啟用連接恢復功能是建立衍生自組件中的類別`DbConfiguration`類別，並在該類別中設定 SQL Database 的執行策略，這在 Entity Framework 是重試原則的另一種說法。

### <a name="implementing-connection-resiliency"></a>實作連接恢復功能

1. 下載並開啟[WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409)範例 Visual Studio 中的 Web Form 應用程式。
2. 在 *邏輯*資料夾**WingtipToys**應用程式中，將類別檔案命名為*WingtipToysConfiguration.cs*。
3. 將現有的程式碼更換成下列程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework 會自動執行衍生自的類別中找到的程式碼`DbConfiguration`。 您可以使用`DbConfiguration`執行組態工作，否則您會在中執行的程式碼中的類別*Web.config*檔案。 如需詳細資訊，請參閱 < [EntityFramework 的程式碼為基礎的組態](https://msdn.microsoft.com/data/jj680699)。

1. 在 *邏輯*資料夾中，開啟*AddProducts.cs*檔案。
2. 新增`using`陳述式`System.Data.Entity.Infrastructure`以黃色反白顯示：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. 新增`catch`封鎖`AddProduct`方法，讓`RetryLimitExceededException`記錄為中反白顯示黃色：   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

藉由新增`RetryLimitExceededException`例外狀況，您可以提供更好的記錄或他們可以選擇要再試一次程序對使用者顯示錯誤訊息。 藉由捕捉`RetryLimitExceededException`例外狀況，可能是暫時性的唯一錯誤將會已具有已嘗試且失敗數次。 傳回的實際例外狀況會包裝在`RetryLimitExceededException`例外狀況。 此外，您也會新增一般 catch 區塊。 如需詳細資訊`RetryLimitExceededException`例外狀況，請參閱 < [Entity Framework 連接恢復 / 重試邏輯](https://msdn.microsoft.com/data/dn456835)。

## <a name="command-interception"></a>命令攔截

既然您已開啟重試原則，您要如何測試以確認，它會如預期般運作？ 它不是很容易就能強制暫時性錯誤發生，特別是您在本機執行，也會特別難以將實際的暫時性錯誤整合到自動化的單元測試。 若要測試連接恢復功能，您需要攔截 Entity Framework 會將傳送至 SQL Server 的查詢和 SQL Server 回應取代是通常是暫時性的例外狀況類型的方法。

您也可以使用查詢攔截，以便實作雲端應用程式的最佳作法： 記錄的延遲和成功或失敗的所有呼叫對外部服務，例如資料庫服務。

在本教學課程的這一節中，您將使用 Entity Framework [*攔截功能*](https://msdn.microsoft.com/data/dn469464)來記錄和模擬暫時性錯誤。

### <a name="create-a-logging-interface-and-class"></a>建立記錄介面和類別

記錄的最佳作法是使用執行[ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx)而不是硬式編碼呼叫`System.Diagnostics.Trace`或記錄類別。 可讓您更輕鬆地變更您的記錄機制稍後，如果您需要這麼做。 因此在此區段中，您將建立的記錄介面和類別來實作它。

根據上述的程序，您已下載並開啟**WingtipToys**範例 Visual Studio 中的應用程式。

1. 建立資料夾**WingtipToys**專案，並命名*記錄*。
2. 在 *記錄*資料夾中，建立名為的類別檔案*ILogger.cs*並以下列程式碼取代預設的程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   介面提供三種追蹤層級，來指出記錄檔的相對重要性，旨在提供外部服務呼叫，例如資料庫查詢的延遲資訊的另一個。 記錄方法具有多載，可讓您傳入例外狀況。 這是使類別實作的介面，而不是依賴在整個應用程式的每個記錄方法呼叫中完成，可靠地記錄包括堆疊追蹤和內部例外狀況的例外狀況資訊。  
  
   `TraceApi`方法可讓您追蹤每次呼叫外部的服務，例如 SQL Database 的延遲。
3. 在 *記錄*資料夾中，建立名為的類別檔案*Logger.cs*並以下列程式碼取代預設的程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

此實作會使用`System.Diagnostics`進行追蹤。 這是.NET 讓您輕鬆地產生及使用追蹤資訊的內建功能。 有許多&quot;接聽程式&quot;您可以搭配使用`System.Diagnostics`將記錄檔寫入檔案，比方說，或將它們寫入 Windows Azure blob 儲存體的追蹤。 在中看到的一些選項和其他資源，如需詳細資訊，連結[在 Visual Studio 中疑難排解 Windows Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 本教學課程中，您將只探討 Visual Studio 中的記錄檔**輸出**視窗。

在生產環境應用程式中您可能要考慮使用追蹤架構以外`System.Diagnostics`，而`ILogger`介面輕鬆切換至不同的追蹤機制，如果您決定要這麼做。

### <a name="create-interceptor-classes"></a>建立攔截器類別

接下來，您將建立 Entity Framework 將會呼叫每一次，它會將查詢傳送至資料庫，以模擬暫時性錯誤，另一個要記錄的類別。 這些攔截器的類別必須衍生自`DbCommandInterceptor`類別。 中，您可以撰寫會自動執行查詢時呼叫的方法覆寫。 這些方法中，您可以檢查或記錄傳送至資料庫，查詢，您可以傳送至資料庫之前，請變更查詢或傳回某些資訊至 Entity Framework 自己沒有甚至將查詢傳遞至資料庫。

1. 若要建立傳送至資料庫之前，會記錄每個 SQL 查詢攔截器類別，建立名為的類別檔案*InterceptorLogging.cs*中*邏輯*資料夾，並將預設程式碼下列程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   對於成功的查詢或命令，此程式碼會寫入延遲資訊的資訊記錄的檔。 針對例外狀況，它會建立錯誤記錄檔。
2. 若要建立攔截器類別，當您輸入時，會產生空的暫時性錯誤&quot;擲回&quot;中**名稱**名為頁面上的文字方塊*AdminPage.aspx*，建立類別檔案名*InterceptorTransientErrors.cs*中*邏輯*資料夾，並將預設程式碼使用下列程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    這段程式碼只會覆寫`ReaderExecuting`方法，也就所謂的查詢可傳回多個資料列。 如果您想要檢查查詢的其他類型的連線恢復功能，您可能也會覆寫`NonQueryExecuting`和`ScalarExecuting`方法，為記錄攔截器沒有。  
  
   稍後，您將為 「 管理員 」 身分登入，並選取**Admin**在上方導覽列上的連結。 然後，在*AdminPage.aspx*頁面上，您將新增名為產品&quot;擲回&quot;。 程式碼會建立空的 SQL Database 例外狀況錯誤號碼 20，已知為通常是暫時性的類型。 其他目前辨識為暫時性的錯誤號碼 64、 233、 10053、 10054、 10060、 10928、 10929、 40197、 40501、 和 40613，但這些是在 SQL Database 的新版本中有所變更。 產品將會重新命名為"TransientErrorExample"，其中的程式碼中，您可以依照*InterceptorTransientErrors.cs*檔案。  
  
   程式碼會傳回例外狀況，Entity framework，而不是執行查詢，並傳遞後的結果。 會傳回暫時性例外狀況*四個*時間，然後將程式碼會還原為一般的程序的查詢傳遞至資料庫。

    因為所有項目記錄時，您可以請參閱 Entity Framework 會嘗試將在最終成功前, 四次執行查詢，並在應用程式中唯一的差別是，需要花費較長的時間呈現含有查詢結果的頁面。  
  
   Entity Framework 會重試的次數與可設定;程式碼會指定四次，因為這是 SQL Database 執行原則的預設值。 如果您變更執行原則時，您必須也變更此處的程式碼，指定產生暫時性錯誤的次數。 您也可以變更程式碼以產生更多的例外狀況，讓 Entity Framework 將會擲回`RetryLimitExceededException`例外狀況。
3. 在  *Global.asax*，新增下列 using 陳述式：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. 然後，新增要反白顯示的行`Application_Start`方法：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

這行程式碼會造成您的攔截器程式碼，Entity Framework 會將查詢傳送至資料庫時要執行的原因。 請注意，因為您所建立的暫時性錯誤模擬不同的攔截器類別，並記錄，您可以獨立啟用和停用它們。   
  
 您可以將使用的攔截器`DbInterception.Add`方法中任何位置的程式碼; 它不一定要在`Application_Start`方法。 另一個選項，如果您未新增中的攔截器`Application_Start`方法，就是更新或新增名為的類別*WingtipToysConfiguration.cs* ，並將上述程式碼中的建構函式結尾處放`WingtipToysbConfiguration`類別。

只要您將此程式碼，是請小心不要執行`DbInterception.Add`相同的攔截器超過一次，或者您會收到額外的攔截器執行個體。 比方說，如果您新增記錄攔截器兩次，您會看到兩個記錄檔，針對每個 SQL 查詢。

攔截器執行順序註冊 (順序`DbInterception.Add`方法呼叫)。 順序可能會有任何影響，取決於您所要做的攔截器。 比方說，攔截器可能會變更 SQL 命令，它會取得以`CommandText`屬性。 如果它未變更的 SQL 命令下, 一步 的攔截器會取得已變更的 SQL 命令未原始的 SQL 命令。

您已撰寫的暫時性錯誤模擬程式碼可讓您在 UI 中輸入不同的值而造成暫時性錯誤的方式。 或者，您可以撰寫一律產生暫時性例外狀況的序列，而不檢查特定的參數值的攔截器程式碼。 只有在您想要產生暫時性錯誤時，您接著可以增加攔截器。 如果您這麼做，不過，不加入的攔截器，直到資料庫初始化完成之後。 換句話說，才能開始產生暫時性錯誤時，才能執行其中一個實體集上的至少一個資料庫作業等查詢。 Entity Framework 資料庫初始化期間執行數個查詢，並不在交易中中, 執行，因此在初始化期間的錯誤可能會導致要進入不一致狀態的內容。

## <a name="test-logging-and-connection-resiliency"></a>測試記錄和連線恢復功能

1. 在 Visual Studio 中按**F5**執行應用程式偵錯模式，然後按一下 登入為 「 管理員 」 使用 「 Pa$ $word 」 作為密碼。
2. 選取  **Admin**從頂端導覽列。
3. 輸入新的產品名為 「 擲回 」 使用適當的描述、 價格和映像檔案。
4. 按下**新增產品** 按鈕。  
   您會發現瀏覽器似乎 Entity Framework 正在重試查詢多次時的幾秒鐘的時間停止回應。 第一個重試會立即完成，則等候會增加每個額外的重試之前。 這個程序的每次重試會在呼叫之前，請等候再*指數型輪詢*。
5. 頁面已無法載入 atttempting 等候。
6. 停止專案，並看看 Visual Studio**輸出**視窗來查看追蹤輸出。 您可以找到**輸出**視窗中的選取**偵錯** - &gt; **Windows**  - &gt; **輸出**。 您可能必須捲動過去您記錄器所撰寫的其他數個記錄檔。  
  
   請注意，您可以看到傳送至資料庫的實際 SQL 查詢。 您會看到一些初始查詢和命令的 Entity Framework 會更新，以開始使用，請檢查資料庫版本和移轉歷程記錄資料表。   
    ![輸出視窗](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   請注意，您就無法重複這項測試，除非您停止應用程式，並將它重新啟動。 如果您想要能夠在應用程式的單一執行多次測試連接恢復功能，您可以撰寫程式碼來重設錯誤計數器中的`InterceptorTransientErrors`。
7. 若要查看差異執行策略 （重試原則） 提出，註解`SetExecutionStrategy`一行*WingtipToysConfiguration.cs*中的檔案*邏輯*資料夾中執行**系統管理員**頁面中偵錯模式，然後新增名為產品&quot;擲回&quot;一次。  
  
   這次嘗試第一次執行查詢時，立即，第一個產生的例外狀況所停止偵錯工具。  
    ![偵錯-檢視詳細資料](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. 取消註解`SetExecutionStrategy`一行*WingtipToysConfiguration.cs*檔案。

## <a name="summary"></a>總結

在本教學課程中，您已了解如何修改 Web Form 範例應用程式支援連接恢復功能和命令攔截。

## <a name="next-steps"></a>後續步驟

連線恢復功能和命令攔截在 ASP.NET Web Form 中的，您已檢閱之後，檢閱 ASP.NET Web Form 主題[ASP.NET 4.5 中的非同步方法](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md)。 本主題將教導您建置非同步 ASP.NET Web Forms 應用程式使用 Visual Studio 的基本概念。
