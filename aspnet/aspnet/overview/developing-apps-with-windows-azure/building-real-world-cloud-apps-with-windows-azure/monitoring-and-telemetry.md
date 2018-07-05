---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: 監視和遙測 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: b2c91b26fddc21986bf90957f7e4ef0a493d5837
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396756"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>監視和遙測 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


很多人依賴客戶，讓他們知道他們的應用程式關閉時。 這並非最佳做法是任何位置，以及特別是在雲端中找不到。 快速通知，不保證，當您執行取得通知，但通常您會發生了什麼事的最小或使人產生誤解的資料。 良好的遙測與記錄系統，您可以很清楚發生什麼情況的應用程式，並有差錯您立即找出並且擁有很有幫助的疑難排解資訊，才能使用。

## <a name="buy-or-rent-a-telemetry-solution"></a>購買或租用的遙測解決方案

> [!NOTE]
> 之前所撰寫的這篇文章[Application Insights](https://azure.microsoft.com/services/application-insights/)發行。 Application Insights 是針對遙測解決方案在 Azure 上慣用的方法。 請參閱[設定為您的 ASP.NET 網站的 Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net)如需詳細資訊。


絕佳雲端環境的相關的事情之一是，它真的很容易購買，或永久出租踏上的勝利。 遙測是範例。 而不需要大費周章中，您可以取得絕佳的遙測系統啟動和執行、 非常符合成本效益。 有一大堆很棒的合作夥伴整合有了 Azure，而其中部分有免費層 – 因此您可以取得基本的遙測資料的 「 百無。 以下是幾個項目目前可用在 Azure 上：

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

自 2015 年 3 月起[Microsoft Application Insights for Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)尚未發行，但可供試用的預覽。[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#)也包含監視功能。

我們將快速逐步設定 New Relic，以示範如何輕鬆使用遙測系統。

在 Azure 管理入口網站中，註冊服務。 按一下 **的新**，然後按一下**存放區**。 **選擇附加元件** 對話方塊隨即出現。 向下捲動並按一下**New Relic**。

![選擇附加元件](monitoring-and-telemetry/_static/image1.png)

按一下向右箭號，然後選擇您想要的服務層。 這段示範影片，我們將使用免費層。

![個人化附加元件](monitoring-and-telemetry/_static/image2.png)

按一下向右箭號，確認 [購買]，和 New Relic 現在出現在入口網站中的附加元件形式。

![檢閱購買](monitoring-and-telemetry/_static/image3.png)

![在管理入口網站中新的 Relic 附加元件](monitoring-and-telemetry/_static/image4.png)

按一下 **連接資訊**，並將複製的授權金鑰。

![連接資訊](monitoring-and-telemetry/_static/image5.png)

移至**設定**索引標籤上，在入口網站中，web 應用程式設定**效能監控**來**附加元件**，並設定**選擇附加元件**下拉式清單**New Relic**。 然後按一下**儲存**。

![設定 索引標籤中的 new Relic](monitoring-and-telemetry/_static/image6.png)

在 Visual Studio 中，安裝在您的應用程式中的 New Relic NuGet 封裝。

![在 [設定] 索引標籤中的開發人員分析](monitoring-and-telemetry/_static/image7.png)

將應用程式部署至 Azure，並開始使用它。 建立一些其修正工作提供一些 New relic 監視的活動。

然後返回**New Relic**頁面**附加元件** 索引標籤的入口網站，然後按一下**管理**。 在入口網站傳送給您 New Relic 管理入口網站中，因此您不需要再次輸入您的認證，使用單一登入進行驗證。 [概觀] 頁面會顯示各種不同的效能統計資料。 （按一下以查看 [概觀] 頁面的完整大小的影像）。

[![新的 Relic 監視索引標籤](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

以下是幾個您所見的統計資料：

- 在一天的不同時間的平均回應時間。

    ![回應時間](monitoring-and-telemetry/_static/image10.png)
- 輸送量速率 （以每分鐘的要求） 在不同一天的時間。

    ![輸送量](monitoring-and-telemetry/_static/image11.png)
- 處理不同的 HTTP 要求所花費的伺服器 CPU 時間。

    ![網頁交易時間](monitoring-and-telemetry/_static/image12.png)
- 應用程式程式碼的不同部分中所花費的 CPU 時間：

    ![追蹤詳細資料](monitoring-and-telemetry/_static/image13.png)
- 記錄效能統計資料。

    ![歷程記錄的效能](monitoring-and-telemetry/_static/image14.png)
- 在服務已呼叫外部服務，例如 Blob 服務及如何可靠且回應迅速的相關統計資料。

    ![外部服務](monitoring-and-telemetry/_static/image15.png)

    ![外部服務](monitoring-and-telemetry/_static/image16.png)

    ![外部服務](monitoring-and-telemetry/_static/image17.png)
- 有關在哪裡世界或在美國的 web 應用程式流量來自何處中的資訊。

    ![地理位置](monitoring-and-telemetry/_static/image18.png)

您也可以設定報告和事件。 比方說，您可以假設任何時間，您會開始看到錯誤，傳送電子郵件問題的警示的支援人員。

![報告](monitoring-and-telemetry/_static/image19.png)

New Relic 是只是一個範例遙測系統;您可以取得所有這些以及其他服務。 雲端的優點是不必撰寫任何程式碼，以及最少或沒有費用，您突然可以取得更多您的應用程式的使用方式和資訊實際發生什麼您的客戶。

<a id="log"></a>
## <a name="log-for-insight"></a>記錄檔以取得深入解析

遙測套件是良好的第一個步驟中，但您仍必須實作您自己的程式碼。 遙測服務會告訴您問題時，並告訴您哪些客戶遇到，不過可能不讓您深入了解發生什麼情況在程式碼中很多。

您不想要有遠端連線至實際執行伺服器，以查看您的應用程式正在做什麼。 當您有一部伺服器，但要如何得知您已調整為數百個伺服器，而且您不知道您需要遠端連線至哪些，這可能是實用嗎？ 您的記錄應該提供足夠的資訊，您永遠不必遠端登入實際執行伺服器，來分析和偵錯問題。 您應該會記錄足夠的資訊，讓您可以找出只會透過記錄檔的問題。

### <a name="log-in-production"></a>登入生產環境

沒有問題，而且他們想要偵錯時，才在生產環境中的追蹤開啟很多人。 這會造成您了解問題的時間和取得其相關的有用疑難排解資訊的時間之間的經過長時間延遲。 且可能無法如間歇性錯誤很有幫助您取得的資訊。

我們在其中儲存體十分便宜的雲端環境中的建議是一律讓您在生產環境中登入。 當您已經有這些記錄，而且您有可以幫助的歷程記錄資料，會發生錯誤時，這樣一來您分析經過一段時間進行開發，或定期在不同時間發生的問題。 您無法自動清除程序，以刪除舊的記錄檔，但您可能會發現它是更難設定這類程序，它是保留記錄檔。

記錄的額外的費用是資訊的一般規則相較於疑難排解的時間和金錢，您可以藉由將所有發生錯誤時，您必須已可使用將儲存的量。 然後當有人命令它們隨機錯誤一段時間約 8:00 昨晚，但它們不記得錯誤，您可以輕易地找出問題是什麼。

對於小於 $4 月，您可以手上，讓 50 gb 的記錄檔和記錄的效能影響是 trivial，只要請記住，為了避免發生效能瓶頸，請確定您的記錄程式庫是非同步的一件事。

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>區別需要動作的記錄檔中的通知的記錄檔

記錄檔是用來通知 （我想要知道） 或是 ACT （我想要執行一些動作）。 請小心只寫入 ACT 記錄檔中真的需要採取動作的人員或自動化的程序的問題。 太多的 ACT 記錄檔會造成干擾，需要太多的工作，仔細檢查一切，找出真正的問題。 如果您的動作記錄檔會自動觸發某些動作，例如傳送電子郵件給支援人員，避免讓觸發單一問題的數千個這類動作。

在.NET System.Diagnostics 追蹤記錄檔可以指派錯誤、 警告、 資訊與偵錯/詳細資訊層級。 您可以從通知記錄區分 ACT，保留 ACT 記錄檔的錯誤層級，並通知記錄檔中使用較低層級。

![記錄層級](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>在執行階段設定的記錄層級

雖然很值得擁有記錄一律在生產環境中，另一個最佳做法是實作可讓您在執行階段調整要記錄的詳細資料，而無須重新部署或重新啟動您的應用程式層級的記錄架構。 例如當您使用追蹤工具，在`System.Diagnostics`錯誤、 警告、 資訊，，您可以建立並偵錯/詳細資訊記錄。 我們建議您一律會記錄錯誤、 警告、 和資訊記錄在生產環境中，您會想要能夠以動態方式加入 偵錯/詳細資訊的記錄以疑難排解案例的基礎。

在 Azure App Service 中的 web 應用程式已內建支援進行寫入`System.Diagnostics`檔案系統、 資料表儲存體或 Blob 儲存體的記錄檔。 您可以選取不同的記錄層級，每個儲存體目的地，而且您可以變更即時的記錄層級不需要重新啟動您的應用程式。 Blob 儲存體支援可讓您更輕鬆地執行[HDInsight](https://docs.microsoft.com/azure/hdinsight/)上您的應用程式記錄檔，因為 HDInsight 知道如何直接使用 Blob 儲存體作業的分析。

### <a name="log-exceptions"></a>記錄例外狀況

不要只放置*例外狀況。Tostring （)* 您記錄程式碼中。 省去了內容資訊。 在 SQL 錯誤的情況下，它省去了 SQL 錯誤號碼。 對於所有的例外狀況，包括內容資訊、 例外狀況本身和內部例外狀況以確定您提供的所有項目所需進行疑難排解。 例如，內容資訊可能包括伺服器名稱、 交易識別碼和使用者名稱 （但不是密碼或任何祕密 ！）。

如果您依賴每位開發人員如何正確處理例外狀況記錄，其中部分不會。 若要確定它取得的權限方式每次，建置至記錄器介面處理的例外狀況： 傳遞例外狀況物件 logger 類別，並正確登入記錄器類別的例外狀況資料。

### <a name="log-calls-to-services"></a>對服務的記錄呼叫

我們強烈建議您將資料寫入記錄檔每次您的應用程式向外呼叫服務，無論是資料庫或 REST API 或任何外部服務。 在您記錄中包含不只表示成功或失敗，但每個要求花費多少時間。 在雲端環境中，您通常會看到相關的速度變慢，而不是完整的中斷問題。 通常要花費 10 毫秒的項目可能會突然開始進行第二個的動作。 當有人告訴您您的應用程式變慢時，您想要能夠查看 New Relic 或任何遙測服務，也可以驗證使用者體驗，然後想要尋找您自己的記錄檔，若要深入了解為什麼很慢的詳細資料。

### <a name="use-an-ilogger-interface"></a>使用 ILogger 介面

我們建議您建立生產環境應用程式時執行的動作是建立簡單*ILogger*介面，並情景其中的一些方法。 這可讓您輕鬆地變更記錄實作更新版本，而不需要瀏覽所有您要的程式碼。 我們可以使用`System.Diagnostics.Trace`類別在整個 It 應用程式，但我們會改為使用它在實作的記錄類別，涵蓋*ILogger*，，賺得*ILogger*方法會在整個呼叫應用程式。

如此一來，如果您想要讓您記錄的內容加豐富，您可以取代[ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs)在您想使用任何記錄機制。 比方說，您的應用程式隨著您可能會決定您想要使用的更完整的記錄封裝，例如[NLog](http://nlog-project.org/)或是[企業程式庫 Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx)。 ([Log4Net](http://logging.apache.org/log4net/)是另一個常見的記錄架構，但它不會執行非同步記錄。)

使用例如 NLog 架構的可能原因之一是促進分割成個別高容量、 高價值的資料存放區記錄輸出。 可協助您有效率地儲存大量通知資料，您不需要執行快速查詢，同時維持資料的快速存取。

### <a name="semantic-logging"></a>語意記錄

如有較新的作法可能會產生更有用的診斷資訊的記錄，請參閱 <<c0> [ 企業程式庫語意記錄應用程式區塊 (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)。 使用的 SLAB[的 Windows 事件追蹤](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)(ETW) 和[EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)支援在.NET 4.5 中，您可以建立多個結構化和可查詢的記錄檔。 您定義不同的方法，每種類型的事件記錄時，可讓您自訂您撰寫的資訊。 例如，若要記錄的 SQL 資料庫錯誤，您可能會呼叫`LogSQLDatabaseError`方法。 針對該類型的例外狀況中，您知道關鍵的資訊是錯誤號碼，因此您可以包含方法簽章中的錯誤數字參數，並且做為個別的欄位，在您撰寫的記錄檔記錄中記錄的錯誤號碼。 因為數目為個別的欄位中您可以更輕鬆且可靠地取得根據 SQL 錯誤號碼，如果您只串連的錯誤號碼轉換為訊息字串比的報表。

## <a name="logging-in-the-fix-it-app"></a>登入此修正其應用程式

### <a name="the-ilogger-interface"></a>ILogger 介面

以下是*ILogger*修正其應用程式中的介面。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

這些方法可讓您撰寫在相同的四個層級所支援的記錄檔*System.Diagnostics*。 TraceApi 方法適用於記錄的外部服務呼叫的延遲資訊。 您也可以加入一組偵錯/詳細資訊層級的方法。

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>記錄器實作 ILogger 介面

介面的實作是很簡單。 它基本上只會呼叫納入標準*System.Diagnostics*方法。 下列程式碼片段會示範這三個資訊的方法，其中每個其他。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>呼叫 ILogger 方法

每次它修正應用程式中的程式碼攔截到例外狀況，它會呼叫*ILogger*方法來記錄例外狀況詳細資料。 與每一次，它會呼叫資料庫、 Blob 服務或 REST API 時，它啟動之前，還有 stopwatch，請停止馬錶時，服務會傳回，並記錄經過的時間，以及成功或失敗的相關資訊。

請注意記錄訊息包含的類別名稱和方法名稱。 它是個不錯的做法，請確定記錄檔訊息識別哪一部分的應用程式程式碼撰寫它們。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

現在每當，修正其應用程式發出呼叫至 SQL Database，您所見的呼叫，呼叫的方法，完全多少時間花了。

![記錄檔中的 SQL 資料庫查詢](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

如果您移瀏覽記錄檔，您可以看到資料庫呼叫所花費的時間是變數。 資訊可能會很有用： 因為應用程式會記錄所有這您可以分析的資料庫服務經過一段時間的執行方式的歷史趨勢。 比方說，服務可能會快速大部分的情況，但要求可能會失敗或回應可能會在特定一天的時間變慢。

您可以進行相同的動作，Blob 服務-每次應用程式上傳新的檔案，沒有記錄檔，而且您可以看到每個檔案上傳所花費的時間完全。

![Blob 上傳記錄檔](monitoring-and-telemetry/_static/image23.png)

它是只要幾個額外的程式碼撰寫每次呼叫服務時，每當有人說他們遇到問題，您現在知道確切的問題是什麼，或它是否發生錯誤，即使它只執行速度很慢。 您可以找出問題的來源，而不需要遠端連線至伺服器，或開啟記錄之後發生的錯誤，並且希望能夠重新建立它。

## <a name="dependency-injection-in-the-fix-it-app"></a>修正程式中的相依性插入它的應用程式

您或許想知道如何在上述範例中的存放庫建構函式會取得記錄器的介面實作：

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

若要連接介面的實作應用程式會使用[相依性插入](http://en.wikipedia.org/wiki/Dependency_injection)(DI) 與[AutoFac](http://autofac.org/)。 DI 可讓您使用多個位置，在整個程式碼中的介面為基礎的物件，並只需要指定在一處取得介面具現化時使用的實作。 這可讓您更輕鬆地變更實作： 例如，您可能想要取代 NLog 記錄器的 System.Diagnostics 記錄器。 或者，進行自動化測試中，您可能想要取代記錄器的模擬版本。

修正其應用程式會使用 DI，在所有存放庫及所有控制站。 控制器類別的建構函式取得*ITaskRepository*介面相同的方式儲存機制取得記錄器的介面：

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

應用程式使用 AutoFac DI 程式庫會自動提供*TaskRepository*並*記錄器*這些建構函式的執行個體。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

這段程式碼基本上，任何地方的建構函式需要*ILogger*介面，則會執行個體中傳遞*記錄器*類別，以及每當需要*IFixItTaskRepository*介面中，執行個體中傳遞*FixItTaskRepository*類別。

[AutoFac](http://autofac.org/)是其中一個您可以使用的許多相依性插入架構。 另一個熱門的是[Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx)，這是建議，而且受到 Microsoft Patterns and Practices。

## <a name="built-in-logging-support-in-azure"></a>在 Azure 中的內建的記錄支援

Azure 支援下列類型的[Azure App Service 中的 Web Apps 記錄](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- （您可以開啟和關閉且立即設定層級，不需要重新啟動站台） 的 System.Diagnostics 追蹤。
- Windows 事件。
- IIS 記錄檔 (HTTP/FREB)。

Azure 支援下列類型的[登入雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics 追蹤。
- 效能計數器。
- Windows 事件。
- IIS 記錄檔 (HTTP/FREB)。
- 監視的自訂目錄。

修正其應用程式使用 System.Diagnostics 追蹤。 您只需要啟用登入的 web 應用程式的 System.Diagnostics 執行是在入口網站中翻轉交換器，或呼叫 REST API。 在入口網站中，按一下**組態**索引標籤上，為您的網站，並向下捲動以查看**Application Diagnostics**一節。 您可以開啟或關閉記錄，並選取您想要的記錄層級。 您可以將記錄檔寫入檔案系統或儲存體帳戶的 Azure。

![應用程式診斷和站台設定 索引標籤中的診斷](monitoring-and-telemetry/_static/image24.png)

啟用 登入 Azure 之後，您就可以看到 Visual Studio 輸出 視窗中的記錄檔，在建立。

![串流記錄檔 功能表](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![串流記錄檔 功能表](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

您也可以寫入至儲存體帳戶的記錄檔和任何工具，檢視可以存取 Azure 儲存體表格服務，例如**伺服器總管**Visual Studio 中或[Azure 儲存體總管](https://azure.microsoft.com/features/storage-explorer/)。

![在 [伺服器總管] 中的記錄檔](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>總結

它是非常簡單了，實作的立即可用的遙測系統、 檢測登入您的程式碼，並在 Azure 中設定記錄。 當您遇到生產環境問題，遙測系統和自訂記錄檔的組合會協助您快速解決問題，才能為您的客戶成為主要的問題。

在 [[下一步] 一章](transient-fault-handling.md)我們將探討如何處理暫時性錯誤，因此它們不會變得生產問題的調查。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源。

主要的相關遙測的文件：

- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱檢測和遙測指引、 服務計量指導方針、 健康情況端點監視模式和執行階段重新設定模式。
- [舉凡在雲端中的貨幣值： 啟用 Azure 網站上監視新的 Relic 效能](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx)。
- [Azure 雲端服務上大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。 請參閱 [遙測和診斷] 區段。
- [使用 Application Insights 的新一代開發](https://msdn.microsoft.com/magazine/dn683794.aspx)。 MSDN Magazine 文章。

主要記錄的相關文件：

- [語意記錄應用程式區塊 (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)。 Neil Mackenzie 呈現 SLAB 使用語意記錄的情況。
- [使用語意記錄建立結構化且有意義的記錄檔](https://channel9.msdn.com/Events/Build/2013/3-336)。 （影片）凱撒 Dominguez 呈現 SLAB 使用語意記錄的情況。
- [EF6 SQL 記錄 – 第 1 部： 簡單記錄](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/)。 Arthur Vickers 示範如何將記錄 Entity Framework 中 EF 6 所執行的查詢。
- [連線恢復功能和命令攔截與 ASP.NET MVC 應用程式中的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 第四個九段式教學課程系列中，示範如何使用 EF 6 命令攔截功能來記錄傳送至資料庫中，Entity framework 的 SQL 命令。
- [改善使用 C# 5.0 呼叫端資訊屬性的記錄](http://www.dotnetcurry.com/showarticle.aspx?ID=972)。 如何不需要硬式編碼到常值，或使用反映來取得它以手動方式，輕鬆記錄呼叫方法的名稱。

主要是有關疑難排解的文件：

- [Azure 的疑難排解&amp;偵錯部落格](https://blogs.msdn.com/b/kwill/)。
- [AzureTools – 診斷公用程式使用 Azure 開發人員支援小組](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)。 介紹並提供工具，可用於 Azure VM 上下載並執行各種不同的診斷和監視工具的下載連結。 當您需要以診斷特定 VM 上的問題時很有用。
- [疑難排解使用 Visual Studio 的 Azure App Service 中的 web 應用程式](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 開始使用 System.Diagnostics 追蹤和遠端偵錯的逐步教學課程。

影片：

- [FailSafe︰ 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms、 包含九部組件系列。 提供高階的概念和架構原則非常可存取且有趣的方式，取自 Microsoft 客戶諮詢團隊 (CAT) 體驗，與實際客戶的劇本。 4 到 9 的影片是關於監視和遙測。 9 包含監視服務 MetricsHub、 AppDynamics、 New Relic 中，與 PagerDuty 的概觀。
- [建置大型： 從 Azure 的客戶-第 ii 部分 Lessons learned](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 將討論如何針對失敗設計及檢測所有項目。 類似於保全數列但作法的詳細說明。

程式碼範例：

- [雲端服務基本概念，在 Azure 中的](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 Microsoft Azure 客戶諮詢團隊所建立的範例應用程式。 將遙測和記錄做法的示範，如下列文章所述。 此範例會實作所使用的應用程式記錄[NLog](http://nlog-project.org/)。 如需相關的文件，請參閱[系列的四個有關遙測和記錄的 TechNet wiki 文章](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon)。

> [!div class="step-by-step"]
> [上一頁](design-to-survive-failures.md)
> [下一頁](transient-fault-handling.md)
