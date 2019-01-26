---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：ASP.NET MVC 應用程式中使用 EF 的連線恢復功能和命令攔截
author: tdykstra
description: 在本教學課程中，您將了解如何使用連線恢復功能和命令攔截。 也就是 Entity Framework 6 的兩個重要的功能。
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889817"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>教學課程：在 ASP.NET MVC 應用程式中使用 Entity Framework 連接恢復功能和命令攔截

到目前為止應用程式具有已在本機執行 IIS Express 中的開發電腦上。 若要讓實際的應用程式供其他人透過網際網路使用，您必須將它部署至 web 主控提供者，，您必須將資料庫部署到資料庫伺服器。

在本教學課程中，您將了解如何使用連線恢復功能和命令攔截。 它們是兩個重要功能的 Entity Framework 6，您要部署到雲端環境時，會特別有用： 連線恢復功能 （自動重試的暫時性錯誤） 和命令攔截 （捕捉所有的 SQL 查詢傳送至資料庫若要記錄或加以變更。）

此連接恢復功能和命令攔截教學課程是選擇性的。 如果您略過本教學課程中，必須要在後續的教學課程中進行一些微幅調整。

在本教學課程中，您已：

> [!div class="checklist"]
> * 啟用連線恢復功能
> * 啟用命令攔截
> * 測試新的組態

## <a name="prerequisites"></a>必要條件

* [排序、篩選與分頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>啟用連線恢復功能

當您部署至 Windows Azure 應用程式時，您會將資料庫部署到 Windows Azure SQL Database，雲端資料庫服務。 當您連接到與您的 web 伺服器和資料庫伺服器會直接連接一起在相同的資料中心的雲端資料庫服務，暫時性連接錯誤是通常會更頻繁。 即使在相同的資料中心裝載雲端 web 伺服器和雲端資料庫服務，有更多的網路連線，它們可能會發生問題，例如負載平衡器之間。

也雲端服務通常由其他使用者共用，這表示回應速度可能會受到這些。 與您資料庫的存取權可能受到節流。 當您嘗試存取多個經常超過所允許的服務等級協定 (SLA)，節流方式的資料庫服務將擲回例外狀況。

許多或大部分連線問題時您正在存取雲端服務是暫時性的也就是他們的解析本身一段時間。 因此當您嘗試在資料庫作業，並取得一種是通常是暫時性的錯誤，您可以再次嘗試操作之後不久和作業可能會成功。 如果您藉由自動嘗試一次，處理暫時性錯誤，您可以為您的使用者提供更好的體驗對客戶進行大部分都不可見。 連接恢復功能，在 Entity Framework 6 中，會自動執行程序的重試失敗的 SQL 查詢。

連接恢復功能必須適當地設定特定的資料庫服務：

- 編譯器必須知道哪一個例外狀況有可能是暫時性的。 您想要重試因暫時遺失網路連線能力錯誤，例如程式 bug 所造成的錯誤。
- 它必須等候適當的失敗作業的重試之間的時間量。 您可以再重試之間等待的批次程序比使用者等待回應的位置線上 web 網頁。
- 它具有適當數目的次數之前放棄重試一次。 您可以在批次程序，就像是在線上應用程式重試一次。

您可以設定這些設定，以手動方式針對 Entity Framework 提供者支援的任何資料庫環境，但通常很適合使用 Windows Azure SQL Database 的線上應用程式的預設值已設定，以及這些都是您將實作的 Contoso 大學應用程式的設定。

您只需要進行連接復原是衍生自組件中建立的類別[DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx)類別，並在該類別中設定 SQL Database*執行策略*，在 EF 中是另一種說法*重試原則*。

1. 在 DAL 資料夾中，新增名為的類別檔案*SchoolConfiguration.cs*。
2. 使用下列程式碼取代範本程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework 會自動執行衍生自的類別中找到的程式碼`DbConfiguration`。 您可以使用`DbConfiguration`執行組態工作，否則您會在中執行的程式碼中的類別*Web.config*檔案。 如需詳細資訊，請參閱 < [EntityFramework 的程式碼為基礎的組態](https://msdn.microsoft.com/data/jj680699)。
3. 在  *StudentController.cs*，新增`using`陳述式`System.Data.Entity.Infrastructure`。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. 變更的所有`catch`封鎖該 catch`DataException`例外狀況，讓它們攔截`RetryLimitExceededException`例外狀況而。 例如: 

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    您已使用`DataException`嘗試識別可能是暫時性，才能提供好記的 「 再試一次 」 訊息的錯誤。 但既然您已開啟重試原則，可能是暫時性的唯一錯誤將已有已嘗試且失敗數次，並傳回實際的例外狀況會包裝在`RetryLimitExceededException`例外狀況。

如需詳細資訊，請參閱 < [Entity Framework 連接恢復 / 重試邏輯](https://msdn.microsoft.com/data/dn456835)。

## <a name="enable-command-interception"></a>啟用命令攔截

既然您已開啟重試原則，您要如何測試以確認，它會如預期般運作？ 它不是很容易就能強制暫時性錯誤發生，特別是您在本機執行，也會特別難以將實際的暫時性錯誤整合到自動化的單元測試。 若要測試連接恢復功能，您需要攔截 Entity Framework 會將傳送至 SQL Server 的查詢和 SQL Server 回應取代是通常是暫時性的例外狀況類型的方法。

您也可以使用查詢攔截，以便實作雲端應用程式的最佳作法：[記錄的延遲和成功或失敗的外部服務的所有呼叫](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)例如資料庫服務。 提供 EF6[專用的記錄 API](https://msdn.microsoft.com/data/dn469464) ，可以輕鬆地進行記錄，但在本教學課程的這一節中，您將學習如何使用 Entity Framework[攔截功能](https://msdn.microsoft.com/data/dn469464)直接，同時用於記錄和模擬暫時性錯誤。

### <a name="create-a-logging-interface-and-class"></a>建立記錄介面和類別

A[記錄的最佳做法](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)是要使用介面而不是硬式編碼將 System.Diagnostics.Trace 或記錄類別的呼叫。 可讓您更輕鬆地變更您的記錄機制稍後，如果您需要這麼做。 讓您將在本節中建立記錄介面和類別來實作其/p >

1. 在專案中建立資料夾並將它命名*記錄*。
2. 在 *記錄*資料夾中，建立名為的類別檔案*ILogger.cs*，並以下列程式碼取代範本程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    介面提供三種追蹤層級，來指出記錄檔的相對重要性，旨在提供外部服務呼叫，例如資料庫查詢的延遲資訊的另一個。 記錄方法具有多載，可讓您傳入例外狀況。 這是使類別實作的介面，而不是依賴在整個應用程式的每個記錄方法呼叫中完成，可靠地記錄包括堆疊追蹤和內部例外狀況的例外狀況資訊。

    TraceApi 方法可讓您追蹤每次呼叫外部的服務，例如 SQL Database 的延遲。
3. 在 *記錄*資料夾中，建立名為的類別檔案*Logger.cs*，並以下列程式碼取代範本程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    此實作會使用 System.Diagnostics 進行追蹤。 這是.NET 讓您輕鬆地產生及使用追蹤資訊的內建功能。 有許多 「 接聽程式 」 記錄檔寫入檔案，比方說，或將它們寫入至 Azure blob 儲存體，您可以使用與 System.Diagnostics 追蹤。 在中看到的一些選項和其他資源，如需詳細資訊，連結[在 Visual Studio 中疑難排解 Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 本教學課程中您只會查看記錄檔在 Visual Studio**輸出**視窗。

    您可能要考慮追蹤套件以外 System.Diagnostics，在實際執行的應用程式，ILogger 介面輕鬆切換至不同的追蹤機制，如果您決定要這麼做。

### <a name="create-interceptor-classes"></a>建立攔截器類別

接下來，您將建立 Entity Framework 將會呼叫每一次，它會將查詢傳送至資料庫，以模擬暫時性錯誤，另一個要記錄的類別。 這些攔截器的類別必須衍生自`DbCommandInterceptor`類別。 在其中您可以撰寫會自動執行查詢時呼叫的方法覆寫。 這些方法中，您可以檢查或記錄傳送至資料庫，查詢，您可以傳送至資料庫之前，請變更查詢或傳回某些資訊至 Entity Framework 自己沒有甚至將查詢傳遞至資料庫。

1. 若要建立將會記錄每個傳送至資料庫的 SQL 查詢攔截器類別，建立名為的類別檔案*SchoolInterceptorLogging.cs*中*DAL*資料夾，然後取代範本程式碼下列程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    對於成功的查詢或命令，此程式碼會寫入延遲資訊的資訊記錄的檔。 針對例外狀況，它會建立錯誤記錄檔。
2. 若要建立會產生空的暫時性錯誤，當您輸入 「 擲回 」 的攔截器類別中**搜尋**方塊中，建立名為的類別檔案*SchoolInterceptorTransientErrors.cs* 中*DAL*資料夾，並取代為下列程式碼的範本程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    這段程式碼只會覆寫`ReaderExecuting`方法，也就所謂的查詢可傳回多個資料列。 如果您想要檢查查詢的其他類型的連線恢復功能，您可能也會覆寫`NonQueryExecuting`和`ScalarExecuting`方法，為記錄攔截器沒有。

    當您執行 [Student] 頁面，並輸入 「 擲回 」 做為搜尋字串時，此程式碼會建立空的 SQL Database 例外狀況錯誤號碼 20，已知為通常是暫時性的類型。 其他目前辨識為暫時性的錯誤號碼 64、 233、 10053、 10054、 10060、 10928、 10929、 40197、 40501、 和 40613，但這些是在 SQL Database 的新版本中有所變更。

    程式碼會傳回例外狀況，而不是執行查詢，並傳遞後的查詢結果的 Entity framework。 暫時性例外狀況會傳回四次，然後將程式碼會還原為一般的程序，將查詢傳遞至資料庫。

    因為所有項目記錄時，您可以請參閱 Entity Framework 會嘗試將在最終成功前, 四次執行查詢，並在應用程式中唯一的差別是，需要花費較長的時間呈現含有查詢結果的頁面。

    Entity Framework 會重試的次數與可設定;程式碼會指定四次，因為這是 SQL Database 執行原則的預設值。 如果您變更執行原則時，您必須也變更此處的程式碼，指定產生暫時性錯誤的次數。 您也可以變更程式碼以產生更多的例外狀況，讓 Entity Framework 將會擲回`RetryLimitExceededException`例外狀況。

    您在 [搜尋] 方塊中輸入的值將會在`command.Parameters[0]`和`command.Parameters[1]`（一個用於名字和姓氏的其中一個）。 找到的值"%throw%"時，「 擲回 」 所取代的參數"an"，因此將會找到一些學生，而且傳回。

    這是只是便利的方式來測試根據變更的應用程式 UI 的一些輸入的連接恢復功能。 您也可以撰寫程式碼所產生的所有查詢或更新的暫時性錯誤，因為相關的註解中稍後所述*DbInterception.Add*方法。
3. 在  *Global.asax*，新增下列`using`陳述式：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. 新增至反白顯示的行`Application_Start`方法：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    這行程式碼會造成您的攔截器程式碼，Entity Framework 會將查詢傳送至資料庫時要執行的原因。 請注意，因為您所建立的暫時性錯誤模擬不同的攔截器類別，並記錄，您可以獨立啟用和停用它們。

    您可以將使用的攔截器`DbInterception.Add`方法中任何位置的程式碼; 它不一定要在`Application_Start`方法。 另一個選項是將此程式碼放在您稍早建立用來設定執行原則的 DbConfiguration 類別。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    只要您將此程式碼，是請小心不要執行`DbInterception.Add`相同的攔截器超過一次，或者您會收到額外的攔截器執行個體。 比方說，如果您新增記錄攔截器兩次，您會看到兩個記錄檔，針對每個 SQL 查詢。

    攔截器執行順序註冊 (順序`DbInterception.Add`方法呼叫)。 順序可能會有任何影響，取決於您所要做的攔截器。 比方說，攔截器可能會變更 SQL 命令，它會取得以`CommandText`屬性。 如果它未變更的 SQL 命令下, 一步 的攔截器會取得已變更的 SQL 命令未原始的 SQL 命令。

    您已撰寫的暫時性錯誤模擬程式碼可讓您在 UI 中輸入不同的值而造成暫時性錯誤的方式。 或者，您可以撰寫一律產生暫時性例外狀況的序列，而不檢查特定的參數值的攔截器程式碼。 只有在您想要產生暫時性錯誤時，您接著可以增加攔截器。 如果您這麼做，不過，不加入的攔截器，直到資料庫初始化完成之後。 換句話說，才能開始產生暫時性錯誤時，才能執行其中一個實體集上的至少一個資料庫作業等查詢。 Entity Framework 資料庫初始化期間執行數個查詢，並不在交易中中, 執行，因此在初始化期間的錯誤可能會導致要進入不一致狀態的內容。

## <a name="test-the-new-configuration"></a>測試新的組態

1. 按下**F5**以執行應用程式在偵錯模式中，然後按一下**學生** 索引標籤。
2. 看看 Visual Studio**輸出**視窗來查看追蹤輸出。 您可能必須向上捲動過去以前往您記錄器所寫入的記錄某些 JavaScript 錯誤。

    請注意，您可以看到傳送至資料庫的實際 SQL 查詢。 您會看到一些初始查詢和 Entity Framework 會開始之前，先檢查資料庫版本的命令和移轉歷程記錄資料表 （您將了解移轉中下一個教學課程）。 您會看到查詢，以供分頁、 以找出多少學生，和最後您會看到取得學生資料的查詢。

    ![一般查詢記錄](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 在 **學生**頁面上，輸入 「 擲回 」 作為搜尋字串，然後按一下**搜尋**。

    ![擲回的搜尋字串](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    您會發現瀏覽器似乎 Entity Framework 正在重試查詢多次時的幾秒鐘的時間停止回應。 第一個重試狀況，卻非常快速，則會增加每個額外的重試之前等候。 這個程序的每次重試會在呼叫之前，請等候再*指數型輪詢*。

    當此頁面會顯示，可顯示學生"的 「 在其名稱中，查看 [輸出] 視窗中，，您會看到相同的查詢嘗試五次，前四次傳回暫時性例外狀況。 針對每個暫時性的錯誤，您會看到您撰寫產生中的暫時性錯誤時的記錄檔`SchoolInterceptorTransientErrors`類別 （「 傳回暫時性錯誤命令...」），您會看到時所寫入的記錄檔`SchoolInterceptorLogging`取得例外狀況。

    ![顯示重試次數的記錄輸出](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    因為您輸入搜尋字串，傳回學生資料的查詢已參數化：

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    您不登參數的值，但也可以執行。 如果您想要看到的參數值，您可以撰寫記錄程式碼，以取得參數值從`Parameters`屬性`DbCommand`得到的攔截器方法中的物件。

    請注意，您就無法重複這項測試，除非您停止應用程式，並將它重新啟動。 如果您想要能夠在應用程式的單一執行多次測試連接恢復功能，您可以撰寫程式碼來重設錯誤計數器中的`SchoolInterceptorTransientErrors`。
4. 若要查看差異執行策略 （重試原則） 提出，註解`SetExecutionStrategy`一行*SchoolConfiguration.cs*、 學生頁面偵錯模式中再次執行，並再次搜尋 」 擲回 」。

    這次嘗試第一次執行查詢時，立即，第一個產生的例外狀況所停止偵錯工具。

    ![空的例外狀況](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. 取消註解*SetExecutionStrategy*一行*SchoolConfiguration.cs*。

## <a name="get-the-code"></a>取得程式碼

[下載已完成的專案](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>其他資源

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 已啟用的連線恢復功能
> * 啟用的命令攔截
> * 測試新的組態

請前往下一篇文章，以了解 Code First 移轉和 Azure 部署。
> [!div class="nextstepaction"]
> [Code First 移轉和 Azure 部署](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
