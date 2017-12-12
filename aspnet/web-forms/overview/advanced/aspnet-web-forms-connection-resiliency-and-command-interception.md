---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: "ASP.NET Web Form 連接恢復功能和命令攔截 |Microsoft 文件"
author: Erikre
description: "本教學課程說明如何修改範例應用程式支援連接恢復功能和命令攔截。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 1c24ccd220bf6df09a958d07b13077f004da0a03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Form 連接恢復功能和命令攔截
====================
由[Erik Reitan](https://github.com/Erikre)

在本教學課程中，您將修改 Wingtip Toys 範例應用程式支援連接恢復功能和命令被攔截。 藉由啟用連接恢復功能，Wingtip Toys 範例應用程式會自動重試資料呼叫典型的雲端環境的暫時性錯誤發生時。 此外，藉由實作命令攔截，Wingtip Toys 範例應用程式將會攔截所有傳送至才能記錄，或變更資料庫的 SQL 查詢。

> [!NOTE] 
> 
> 此 Web Form 教學課程根據 Tom Dykstra 下列 MVC 教學課程：  
> [連接恢復功能和命令攔截與 Entity Framework 中的 ASP.NET MVC 應用程式](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>您將學習：

- 如何提供連接恢復功能。
- 如何實作命令攔截。

## <a name="prerequisites"></a>必要條件

開始之前，請確定您已在電腦上安裝下列軟體：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web)。 會自動安裝.NET Framework。
- Wingtip Toys 範例專案，以便您可以實作在本教學課程，Wingtip Toys 專案中所述的功能。 下列連結提供下載詳細資料：

    - [開始使用 ASP.NET 4.5.1 Web Form-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- 之前完成本教學課程，請考慮檢閱相關的教學課程系列，[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 教學課程將協助您熟悉**WingtipToys**專案和程式碼。

## <a name="connection-resiliency"></a>連接恢復功能

當您考慮應用程式部署到 Windows Azure 時，要考慮的其中一個選項部署至資料庫**Windows** **Azure SQL Database**，雲端資料庫服務。 當您連線到比當您的 web 伺服器和資料庫伺服器直接連接在一起在相同的資料中心的雲端資料庫服務，暫時性連接錯誤是通常會更頻繁。 即使雲端 web 伺服器和雲端資料庫服務會裝載在相同的資料中心，有更多的網路連線之間可以有問題，例如負載平衡器。

也雲端服務是通常由其他使用者共用，這表示其回應可能會受到這些。 和您資料庫的存取權可能會受到節流。 節流表示資料庫服務擲回例外狀況時您嘗試存取它比允許的還頻繁，您*服務等級協定*(SLA)。

許多或大部分您正在存取雲端服務時，會發生連接問題是暫時性，也就是他們自行解決在短時間內。 因此再試一次資料庫作業取得的是通常屬於暫時性的錯誤類型時，您無法再次嘗試操作之後短的等候，而且作業可能會成功。 如果您藉由自動嘗試一次，處理暫時性錯誤，您可以為您的使用者提供更好的經驗給客戶，進行大部分是不可見。 在 Entity Framework 6 連接恢復功能會自動執行程序的重試失敗的 SQL 查詢。

針對特定資料庫服務必須適當地設定連接恢復功能：

1. 編譯器必須知道哪一個例外狀況有可能是暫時性的。 您想要重試因暫時失去網路連線錯誤，例如程式 bug 所造成的錯誤。
2. 必須等候適當的失敗作業的重試之間的時間量。 您可以等候時間適用於批次處理的重試之間比線上 web 網頁使用者等待回應的位置。
3. 它有適當數目的次數之前放棄重試一次。 您可以在線上的應用程式中的批次程序中重試次數。

您可以設定這些手動設定 Entity Framework 提供者支援的任何資料庫環境。

您必須啟用連接恢復功能執行的是衍生自組件中建立的類別`DbConfiguration`類別，並在該類別中設定 SQL Database 的執行策略，即 Entity Framework 中的另一個術語，會重試原則。

### <a name="implementing-connection-resiliency"></a>實作連接恢復功能

1. 下載並開啟[WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409)範例 Visual Studio 中的 Web Form 應用程式。
2. 在*邏輯*資料夾**WingtipToys**應用程式中，將類別檔案命名為*WingtipToysConfiguration.cs*。
3. 將現有的程式碼更換成下列程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework 自動執行衍生自的類別中找到的程式碼`DbConfiguration`。 您可以使用`DbConfiguration`類別來設定工作，否則您會在中執行的程式碼中*Web.config*檔案。 如需詳細資訊，請參閱[EntityFramework 程式碼為基礎的組態](https://msdn.microsoft.com/data/jj680699)。

1. 在*邏輯*資料夾中，開啟*AddProducts.cs*檔案。
2. 新增`using`陳述式`System.Data.Entity.Infrastructure`以黃色反白顯示：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. 新增`catch`封鎖`AddProduct`方法以便`RetryLimitExceededException`會記錄為中反白黃色：   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

藉由新增`RetryLimitExceededException`例外狀況，您可以提供更好的記錄或顯示錯誤訊息給使用者，他們可以選擇要再試一次程序。 藉由捕捉`RetryLimitExceededException`例外狀況，可能是暫時性的唯一錯誤將已有已嘗試且失敗數次。 傳回實際的例外狀況會包裝在`RetryLimitExceededException`例外狀況。 此外，您也會新增一般 catch 區塊。 如需有關`RetryLimitExceededException`例外狀況，請參閱[Entity Framework 連接恢復功能 / 重試邏輯](https://msdn.microsoft.com/en-us/data/dn456835)。

## <a name="command-interception"></a>命令攔截

既然您已開啟重試原則，您要如何測試以確認它的運作正常？ 不是很容易就能強制暫時性錯誤發生，特別是您要在本機執行，且很難整合自動化的單元測試實際的暫時性錯誤。 若要測試連接恢復功能，您需要能夠攔截 Entity Framework 會將傳送至 SQL Server 的查詢，並取代為通常屬於暫時性的例外狀況類型的 SQL Server 回應。

您也可以使用查詢攔截，以便實作雲端應用程式的最佳作法： 記錄檔的延遲和成功或失敗的所有呼叫到外部服務，例如資料庫服務。

在本教學課程的這一節中，您將使用 Entity Framework [*攔截功能*](https://msdn.microsoft.com/data/dn469464)來記錄和模擬暫時性錯誤。

### <a name="create-a-logging-interface-and-class"></a>建立記錄介面和類別

記錄的最佳作法是針對執行使用[ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx)而不是硬式編碼呼叫`System.Diagnostics.Trace`或記錄類別。 可讓您更輕鬆地變更其他記錄機制之後，如果您需要執行此作業。 因此在此區段中，您將建立記錄介面和類別來實作它。

根據上述的程序，您已下載並開啟**WingtipToys**範例 Visual Studio 中的應用程式。

1. 建立資料夾中的**WingtipToys**專案，並命名*記錄*。
2. 在*記錄*資料夾中，建立名為的類別檔案*ILogger.cs* ，並以下列程式碼取代預設程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

 介面會提供三個追蹤層級，以指出記錄檔的相對重要性，設計來提供外部服務呼叫，例如資料庫查詢的延遲資訊的另一個。 記錄方法具有多載，可讓您傳遞例外狀況。 這是使類別實作的介面，而不是依賴在整個應用程式的每一個記錄方法呼叫完成後可靠地記錄包括堆疊追蹤和內部例外狀況的例外狀況資訊。  
  
 `TraceApi`方法可讓您追蹤每次呼叫外部的服務，例如 SQL Database 的延遲。
3. 在*記錄*資料夾中，建立名為的類別檔案*Logger.cs* ，並以下列程式碼取代預設程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

實作會使用`System.Diagnostics`進行追蹤。 這是.NET 可輕鬆地產生和使用追蹤資訊的內建功能。 有許多&quot;接聽程式&quot;您可透過`System.Diagnostics`將記錄寫入檔案，比方說，或將它們寫入 Windows Azure blob 儲存體的追蹤。 在中看到的一些選項和其他資源的詳細資訊，連結[Visual Studio 中疑難排解 Windows Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 此教學課程中，您將只探討 Visual Studio 中的記錄檔**輸出**視窗。

在實際執行的應用程式可能會想要考量以外使用追蹤架構`System.Diagnostics`，而`ILogger`介面可切換至不同的追蹤機制，如果您決定要執行此作業相當簡單。

### <a name="create-interceptor-classes"></a>建立攔截器的類別

接下來，您會建立每次它即將傳送資料庫，一個可模擬暫時性錯誤而另一個要記錄的查詢，Entity Framework 將呼叫的類別。 這些攔截器的類別必須衍生自`DbCommandInterceptor`類別。 中，您可以撰寫執行查詢時自動呼叫的方法覆寫。 這些方法中，您可以檢查或記錄傳送至資料庫，此查詢，您可以變更查詢傳送至資料庫之前，或傳回某些資訊至 Entity Framework 自行而不需甚至傳遞至資料庫的查詢。

1. 若要建立傳送至資料庫之前，將會記錄每個 SQL 查詢攔截器類別，建立名為的類別檔案*InterceptorLogging.cs*中*邏輯*資料夾和取代預設程式碼下列程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

 對於成功的查詢或命令，此程式碼會寫入的資訊記錄檔與延遲資訊。 對於例外狀況，它會建立錯誤記錄檔。
2. 若要建立將會產生空的暫時性錯誤，當您輸入的攔截器類別&quot;擲回&quot;中**名稱**此頁面上的文字方塊中*AdminPage.aspx*，建立類別檔名為*InterceptorTransientErrors.cs*中*邏輯*資料夾並取代預設程式碼取代下列程式碼：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    此程式碼只覆寫`ReaderExecuting`方法，也就所謂的查詢可傳回多個資料列。 如果您想要檢查連接恢復功能的其他類型的查詢，您可能也覆寫`NonQueryExecuting`和`ScalarExecuting`方法記錄攔截器一樣。  
  
 稍後，您將"Admin"身分登入，並且選取**Admin**上方導覽列上的連結。 然後，在*AdminPage.aspx*頁面，您會加入名為 product&quot;擲回&quot;。 程式碼會建立空的 SQL 資料庫例外狀況，錯誤號碼 20，通常是暫時性的已知型別。 其他目前辨識為暫時性的錯誤號碼 64、 233、 10053、 10054、 10060、 10928、 10929、 40197、 40501、 和 40613，但這些是 SQL Database 的新版本中可能會變更。 將會重新命名為"TransientErrorExample 」，您可以遵循的程式碼中的產品*InterceptorTransientErrors.cs*檔案。  
  
 程式碼傳回 Entity Framework，而不是執行查詢，並將結果傳遞例外狀況。 會傳回暫時性例外狀況*四個*時間，然後將程式碼會還原為資料庫傳遞查詢的正常程序。

    因為記錄的所有項目時，您可以請參閱 Entity Framework 會嘗試在最終成功前, 四次執行查詢，和應用程式中的唯一差異是，花費更長時間才能呈現含有查詢結果的頁面。  
  
 Entity Framework 將會重試次數是可設定;程式碼會指定四次，因為這是 SQL Database 執行原則的預設值。 如果您變更執行原則時，您必須也變更的程式碼這裡指定所產生的暫時性錯誤的次數。 您也可以變更程式碼以產生更多的例外狀況，因此 Entity Framework 將會擲回`RetryLimitExceededException`例外狀況。
3. 在*Global.asax*，加入下列 using 陳述式：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. 然後，加入要反白顯示的行`Application_Start`方法：  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

下列幾行程式碼的是什麼情況會讓您的攔截器程式碼，Entity Framework 會將查詢傳送至資料庫時執行。 請注意，因為您所建立的暫時性錯誤模擬不同的攔截器類別，並記錄，您可以個別啟用和停用它們。   
  
 您可以新增使用攔截器`DbInterception.Add`方法中任何位置的程式碼; 它不一定要在`Application_Start`方法。 另一個選項，如果您未在攔截器`Application_Start`方法，會以更新或加入名為的類別*WingtipToysConfiguration.cs*並將上述程式碼中的建構函式的結尾`WingtipToysbConfiguration`類別。

無論您將這個程式碼，是請小心不要執行`DbInterception.Add`相同的攔截器超過一次，或您會取得額外的攔截器執行個體。 例如，如果您新增記錄攔截器兩次，您會看到兩個記錄檔，針對每個 SQL 查詢。

攔截器執行順序註冊 (順序`DbInterception.Add`方法呼叫)。 順序可能會根據您所做的攔截器中重要的。 例如，攔截器可能會變更，它會取得以 SQL 命令`CommandText`屬性。 如果它未變更的 SQL 命令下, 一步攔截器會取得已變更的 SQL 命令未原始的 SQL 命令。

您已的暫時性錯誤模擬程式碼撰寫方式，可讓您在 UI 中輸入不同的值而造成暫時性的錯誤。 或者，您可以撰寫一律產生的暫時性例外狀況序列，而不會檢查特定的參數值的攔截器程式碼。 然後，只有在您想要產生暫時性錯誤時，您可以加入攔截器。 如果您這樣做，不過，不加入的攔截器，直到資料庫初始化完成之後。 前開始產生暫時性錯誤，亦即，執行至少一個資料庫上作業，例如查詢實體集的其中一個動作。 Entity Framework 資料庫初始化期間執行數個查詢，而且不在交易中中, 執行，因此初始化期間發生的錯誤可能會導致的內容，讓不一致的狀態。

## <a name="test-logging-and-connection-resiliency"></a>測試記錄和連接恢復功能

1. 在 Visual Studio 中，按**F5**偵錯模式，然後以"Admin"使用"Pa$ $word"做為密碼登入執行應用程式。
2. 選取**Admin**從上方導覽列。
3. 輸入新的產品，名為 「 擲回 」 與適當的描述、 價格和映像檔案。
4. 按**新增產品** 按鈕。  
 您會注意到瀏覽器似乎 Entity Framework 正在重試查詢多次時的幾秒鐘的時間停止回應。 第一個重試會立即完成，然後增加每個額外的重試前的等候。 這個程序的呼叫每次重試之前，請等候再*指數型輪詢*。
5. 請等候頁面不再 atttempting 載入。
6. 停止專案，並查看 Visual Studio**輸出**視窗來查看追蹤輸出。 您可以找到**輸出**視窗選取**偵錯** - &gt; **Windows**  - &gt; **輸出**。 您可能必須捲動完您記錄器所撰寫的其他數個記錄檔。  
  
 請注意，您可以查看實際傳送至資料庫的 SQL 查詢。 您會看到一些初始查詢和命令的 Entity Framework 處理開始之前，檢查資料庫版本和移轉歷程記錄資料表。   
    ![輸出視窗](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
 請注意，除非您停止應用程式，並重新啟動它不能重複這項測試。 如果您想要能夠在應用程式的單一執行多次測試連接恢復功能，您可以撰寫程式碼中的錯誤計數器重設`InterceptorTransientErrors`。
7. 若要查看差異的執行策略 （重試原則） 標示，註解`SetExecutionStrategy`中一行*WingtipToysConfiguration.cs*檔案*邏輯*資料夾中，執行**系統管理員**頁面偵錯模式中上一次，並新增名為 「 產品&quot;擲回&quot;一次。  
  
 這次嘗試第一次執行查詢時，立即上第一個產生的例外狀況, 所停止偵錯工具。  
    ![偵錯-檢視詳細資料](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. 請取消註解`SetExecutionStrategy`中一行*WingtipToysConfiguration.cs*檔案。

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何修改 Web Form 範例應用程式支援連接恢復功能和命令攔截。

## <a name="next-steps"></a>後續步驟

您已檢閱連線恢復功能和 ASP.NET Web Form 中的命令攔截後，檢閱 ASP.NET Web Form 主題[ASP.NET 4.5 中的非同步方法](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md)。 本主題將告訴您建置非同步的 ASP.NET Web Form 應用程式使用 Visual Studio 的基本概念。
