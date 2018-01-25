---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: "連接恢復功能和命令攔截與 Entity Framework 中的 ASP.NET MVC 應用程式 |Microsoft 文件"
author: tdykstra
description: "Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1a28284e203904cc943e5e46b369e8a58ea5c820
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>連接恢復功能和命令攔截與 Entity Framework 中的 ASP.NET MVC 應用程式
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


到目前為止已被本機執行應用程式在 IIS Express 在開發電腦上。 若要讓實際的應用程式供其他人透過網際網路使用，您必須將它部署至 web 主控提供者，並可將資料庫部署到資料庫伺服器。

在本教學課程中，您將學習如何使用 Entity Framework 6 會特別有用，當您部署到雲端環境的兩個功能： 連接恢復功能 （自動重試的暫時性錯誤） 和命令攔截 （攔截所有的 SQL 查詢傳送到資料庫，才能記錄或加以變更）。

此連接恢復功能和命令攔截教學課程是選擇性的。 如果您略過此教學課程中，修正一些次要的問題必須要在後續的教學課程中進行。

## <a name="enable-connection-resiliency"></a>啟用連接恢復功能

當您部署至 Windows Azure 應用程式時，您將資料庫部署到 Windows Azure SQL Database，雲端資料庫服務。 當您連線到比當您的 web 伺服器和資料庫伺服器直接連接在一起在相同的資料中心的雲端資料庫服務，暫時性連接錯誤是通常會更頻繁。 即使雲端 web 伺服器和雲端資料庫服務會裝載在相同的資料中心，有更多的網路連線之間可以有問題，例如負載平衡器。

也雲端服務是通常由其他使用者共用，這表示其回應可能會受到這些。 和您資料庫的存取權可能會受到節流。 當您嘗試經常存取多個它，超過所允許的服務等級協定 (SLA) 時，節流表示資料庫服務將擲回例外狀況。

許多或大部分連線問題時您正在存取雲端服務是暫時性，也就是他們自行解決在短時間內。 因此再試一次資料庫作業取得的是通常屬於暫時性的錯誤類型時，您無法再次嘗試操作之後短的等候，而且作業可能會成功。 如果您藉由自動嘗試一次，處理暫時性錯誤，您可以為您的使用者提供更好的經驗給客戶，進行大部分是不可見。 在 Entity Framework 6 連接恢復功能會自動執行程序的重試失敗的 SQL 查詢。

針對特定資料庫服務必須適當地設定連接恢復功能：

- 編譯器必須知道哪一個例外狀況有可能是暫時性的。 您想要重試因暫時失去網路連線錯誤，例如程式 bug 所造成的錯誤。
- 必須等候適當的失敗作業的重試之間的時間量。 您可以等候時間適用於批次處理的重試之間比線上 web 網頁使用者等待回應的位置。
- 它有適當數目的次數之前放棄重試一次。 您可以在線上的應用程式中的批次程序中重試次數。

您可以設定這些手動設定 Entity Framework 提供者支援的任何資料庫環境，但通常適用於使用 Windows Azure SQL Database 的線上應用程式的預設值已設定，並這些是您將實作 Contoso 大學應用程式的設定。

您只需要啟用連接恢復功能是衍生自組件中建立類別[DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx)類別，並在該類別中設定 SQL Database*執行策略*，在 EF 是另一種說法*重試原則*。

1. 在 DAL 資料夾中，加入名為的類別檔案*SchoolConfiguration.cs*。
2. 取代為下列程式碼中的範本程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework 自動執行衍生自的類別中找到的程式碼`DbConfiguration`。 您可以使用`DbConfiguration`類別來設定工作，否則您會在中執行的程式碼中*Web.config*檔案。 如需詳細資訊，請參閱[EntityFramework 程式碼為基礎的組態](https://msdn.microsoft.com/data/jj680699)。
3. 在*StudentController.cs*，新增`using`陳述式`System.Data.Entity.Infrastructure`。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. 變更所有`catch`封鎖該 catch`DataException`例外狀況，讓它們攔截`RetryLimitExceededException`例外狀況而。 例如: 

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    您已使用`DataException`嘗試識別可能是暫時性，以便將提供好記的 「 重試 」 訊息的錯誤。 但是，現在您已開啟重試原則，可能是暫時性的唯一錯誤將已經已經嘗試過且失敗數次並傳回實際的例外狀況會包裝在`RetryLimitExceededException`例外狀況。

如需詳細資訊，請參閱[Entity Framework 連接恢復功能 / 重試邏輯](https://msdn.microsoft.com/data/dn456835)。

## <a name="enable-command-interception"></a>啟用命令攔截

既然您已開啟重試原則，您要如何測試以確認它的運作正常？ 不是很容易就能強制暫時性錯誤發生，特別是您要在本機執行，且很難整合自動化的單元測試實際的暫時性錯誤。 若要測試連接恢復功能，您需要能夠攔截 Entity Framework 會將傳送至 SQL Server 的查詢，並取代為通常屬於暫時性的例外狀況類型的 SQL Server 回應。

您也可以使用查詢攔截，以便實作雲端應用程式的最佳作法：[記錄延遲和成功或失敗的所有外部服務呼叫](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)等資料庫服務。 提供 EF6[專用記錄 API](https://msdn.microsoft.com/data/dn469464) ，可以讓您更輕鬆地進行記錄，但在本教學課程的這一節中，您將學習如何使用 Entity Framework[攔截功能](https://msdn.microsoft.com/data/dn469464)直接，同時用於記錄和模擬暫時性錯誤。

### <a name="create-a-logging-interface-and-class"></a>建立記錄介面和類別

A[最佳作法進行記錄](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)是針對執行使用介面而不是硬式編碼呼叫 System.Diagnostics.Trace 或記錄的類別。 可讓您更輕鬆地變更其他記錄機制之後，如果您需要執行此作業。 讓您在這一節中建立記錄介面和類別來實作它/p > 

1. 專案中建立資料夾，並將其命名*記錄*。
2. 在*記錄*資料夾中，建立名為的類別檔案*ILogger.cs*，並將範本程式碼取代下列程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    介面會提供三個追蹤層級，以指出記錄檔的相對重要性，設計來提供外部服務呼叫，例如資料庫查詢的延遲資訊的另一個。 記錄方法具有多載，可讓您傳遞例外狀況。 這是使類別實作的介面，而不是依賴在整個應用程式的每一個記錄方法呼叫完成後可靠地記錄包括堆疊追蹤和內部例外狀況的例外狀況資訊。

    TraceApi 方法可讓您追蹤每次呼叫外部的服務，例如 SQL Database 的延遲。
3. 在*記錄*資料夾中，建立名為的類別檔案*Logger.cs*，並將範本程式碼取代下列程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    實作會使用 System.Diagnostics 執行追蹤。 這是.NET 可輕鬆地產生和使用追蹤資訊的內建功能。 有許多 「 接聽程式 」 記錄檔寫入檔案，比方說，或將它們寫入至 blob 儲存體，在 Azure 中，您可以使用 System.Diagnostics 追蹤。 在中看到的一些選項和其他資源的詳細資訊，連結[Visual Studio 中疑難排解 Azure Web Sites](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 本教學課程，您只會看到記錄在 Visual Studio**輸出**視窗。

    在實際執行的應用程式可能會想要考量以外 System.Diagnostics，追蹤封裝和 ILogger 介面可切換至不同的追蹤機制，如果您決定要執行此作業相當簡單。

### <a name="create-interceptor-classes"></a>建立攔截器的類別

接下來，您會建立每次它即將傳送資料庫，一個可模擬暫時性錯誤而另一個要記錄的查詢，Entity Framework 將呼叫的類別。 這些攔截器的類別必須衍生自`DbCommandInterceptor`類別。 在您撰寫執行查詢時自動呼叫的方法覆寫。 這些方法中，您可以檢查或記錄傳送至資料庫，此查詢，您可以變更查詢傳送至資料庫之前，或傳回某些資訊至 Entity Framework 自行而不需甚至傳遞至資料庫的查詢。

1. 若要建立將會記錄傳送至資料庫的每一個 SQL 查詢攔截器類別，建立名為的類別檔案*SchoolInterceptorLogging.cs*中*DAL*資料夾和範本程式碼的取代下列程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    對於成功的查詢或命令，此程式碼會寫入的資訊記錄檔與延遲資訊。 對於例外狀況，它會建立錯誤記錄檔。
2. 在中建立的攔截器類別，將會產生空的暫時性錯誤，當您輸入 「 擲回 」**搜尋**方塊中，建立名為的類別檔案*SchoolInterceptorTransientErrors.cs*中*DAL*資料夾，並取代為下列程式碼的範本程式碼：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    此程式碼只覆寫`ReaderExecuting`方法，也就所謂的查詢可傳回多個資料列。 如果您想要檢查連接恢復功能的其他類型的查詢，您可能也覆寫`NonQueryExecuting`和`ScalarExecuting`方法記錄攔截器一樣。

    當您執行學生頁面，並輸入搜尋字串"Throw"時，此程式碼會建立空的 SQL 資料庫例外狀況，錯誤號碼 20，通常是暫時性的已知型別。 其他目前辨識為暫時性的錯誤號碼 64、 233、 10053、 10054、 10060、 10928、 10929、 40197、 40501、 和 40613，但這些是 SQL Database 的新版本中可能會變更。

    程式碼會傳回例外狀況，而不是執行查詢，並傳遞後的查詢結果的 Entity framework。 暫時性例外狀況就會傳回四次，然後程式碼會還原為資料庫傳遞查詢的正常程序。

    因為記錄的所有項目時，您可以請參閱 Entity Framework 會嘗試在最終成功前, 四次執行查詢，和應用程式中的唯一差異是，花費更長時間才能呈現含有查詢結果的頁面。

    Entity Framework 將會重試次數是可設定;程式碼會指定四次，因為這是 SQL Database 執行原則的預設值。 如果您變更執行原則時，您必須也變更的程式碼這裡指定所產生的暫時性錯誤的次數。 您也可以變更程式碼以產生更多的例外狀況，因此 Entity Framework 將會擲回`RetryLimitExceededException`例外狀況。

    您在 [搜尋] 方塊中輸入的值會在`command.Parameters[0]`和`command.Parameters[1]`（使用其中一種是名字和姓氏的其中一個）。 找到的值"%throw%"時，「 擲回 」 所取代的參數"an"如此將會找到一些學生，而且傳回。

    這是方式正適合測試根據變更的應用程式 UI 的某些輸入的連接恢復功能。 您也可以撰寫程式碼所產生的所有查詢或更新暫時性錯誤，關於註解中稍後所述*DbInterception.Add*方法。
3. 在*Global.asax*，加入下列`using`陳述式：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. 新增要反白顯示的行`Application_Start`方法：

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    下列幾行程式碼的是什麼情況會讓您的攔截器程式碼，Entity Framework 會將查詢傳送至資料庫時執行。 請注意，因為您所建立的暫時性錯誤模擬不同的攔截器類別，並記錄，您可以個別啟用和停用它們。

    您可以新增使用攔截器`DbInterception.Add`方法中任何位置的程式碼; 它不一定要在`Application_Start`方法。 另一個選項是將此程式碼置於 DbConfiguration 建立的類別，您先前設定執行原則。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    無論您將這個程式碼，是請小心不要執行`DbInterception.Add`相同的攔截器超過一次，或您會取得額外的攔截器執行個體。 例如，如果您新增記錄攔截器兩次，您會看到兩個記錄檔，針對每個 SQL 查詢。

    攔截器執行順序註冊 (順序`DbInterception.Add`方法呼叫)。 順序可能會根據您所做的攔截器中重要的。 例如，攔截器可能會變更，它會取得以 SQL 命令`CommandText`屬性。 如果它未變更的 SQL 命令下, 一步攔截器會取得已變更的 SQL 命令未原始的 SQL 命令。

    您已的暫時性錯誤模擬程式碼撰寫方式，可讓您在 UI 中輸入不同的值而造成暫時性的錯誤。 或者，您可以撰寫一律產生的暫時性例外狀況序列，而不會檢查特定的參數值的攔截器程式碼。 然後，只有在您想要產生暫時性錯誤時，您可以加入攔截器。 如果您這樣做，不過，不加入的攔截器，直到資料庫初始化完成之後。 前開始產生暫時性錯誤，亦即，執行至少一個資料庫上作業，例如查詢實體集的其中一個動作。 Entity Framework 資料庫初始化期間執行數個查詢，而且不在交易中中, 執行，因此初始化期間發生的錯誤可能會導致的內容，讓不一致的狀態。

## <a name="test-logging-and-connection-resiliency"></a>測試記錄和連接恢復功能

1. 按 F5 以偵錯模式中執行應用程式，然後按一下**學生** 索引標籤。
2. 查看 Visual Studio**輸出**視窗來查看追蹤輸出。 您可能必須向上捲動超過前往您記錄器所寫入的記錄某些 JavaScript 錯誤。

    請注意，您可以查看實際傳送至資料庫的 SQL 查詢。 您會看到一些初始查詢和 Entity Framework 將會執行開始使用，請檢查資料庫版本的命令和移轉歷程記錄資料表 （您將了解移轉中下一個教學課程）。 您會看到查詢，以供分頁、 了解如何收授多位學生，和最後看見取得學生資料的查詢。

    ![一般查詢記錄](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 在**學生**頁面上，輸入"Throw"作為搜尋字串，然後按一下**搜尋**。

    ![擲回的搜尋字串](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    您會注意到瀏覽器似乎 Entity Framework 正在重試查詢多次時的幾秒鐘的時間停止回應。 第一個重試就會發生非常快速，然後再增加每個額外的重試之前等待。 這個程序的呼叫每次重試之前，請等候再*指數型輪詢*。

    當頁面顯示時，有顯示學生 」 的 「 在其名稱，查看 [輸出] 視窗中，而且您會看到相同的查詢嘗試 5 次，前四次傳回暫時性例外狀況。 針對每個暫時性錯誤，您會看到所產生的暫時性錯誤時，您撰寫的記錄檔`SchoolInterceptorTransientErrors`類別 （「 傳回暫時性錯誤命令...」），而且您會看到記錄檔時，要寫入`SchoolInterceptorLogging`取得例外狀況。

    ![顯示 重試次數的記錄輸出](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    因為您輸入搜尋字串，參數化查詢，以傳回學生資料：

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    您無法登入參數的值，但也可以執行。 如果您想要看到的參數值，您可以寫入記錄程式碼來取得參數值從`Parameters`屬性`DbCommand`，您會得到在攔截器方法物件。

    請注意，除非您停止應用程式，並重新啟動它不能重複這項測試。 如果您想要能夠在應用程式的單一執行多次測試連接恢復功能，您可以撰寫程式碼中的錯誤計數器重設`SchoolInterceptorTransientErrors`。
4. 若要查看差異的執行策略 （重試原則） 標示，註解`SetExecutionStrategy`中一行*SchoolConfiguration.cs*、 學生頁面偵錯模式中再次執行，並再次搜尋 」 擲回 」。

    這次嘗試第一次執行查詢時，立即上第一個產生的例外狀況, 所停止偵錯工具。

    ![空的例外狀況](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. 請取消註解*SetExecutionStrategy*中一行*SchoolConfiguration.cs*。

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何啟用連接恢復功能和記錄 Entity Framework 所撰寫並傳送至資料庫的 SQL 命令。 在下一個教學課程中，您會部署到網際網路，將資料庫部署中使用 Code First 移轉應用程式。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[上一頁](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[下一頁](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
