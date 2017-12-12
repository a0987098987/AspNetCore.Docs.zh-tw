---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: "監控與遙測 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: dfb0158ec05c890ecf80571d95b22d8c791ba7fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>監控與遙測 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


許多人都依賴客戶，讓他們知道他們的應用程式關閉時。 這並非最佳做法是任何位置，以及特別是在雲端中找不到。 不保證的快速通知，並執行取得通知時，您通常可以取得最少，或使人產生誤解的資料發生了什麼事。 具有良好的遙測及記錄系統，您能夠知道如何運作的應用程式，以及項目時未出錯您立即找出，且有很有幫助的疑難排解資訊，才能使用。

## <a name="buy-or-rent-a-telemetry-solution"></a>購買或租用遙測方案

> [!NOTE]
> 本文撰寫之前[Application Insights](https://azure.microsoft.com/services/application-insights/)發行。 Application Insights 是在 Azure 上遙測解決方案慣用的方法。 請參閱[為您的 ASP.NET 網站設定 Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net)如需詳細資訊。


其中一項操作的絕佳雲端環境的相關它是簡單來購買或租用勝利好。 遙測是範例。 不費吹灰之力就可以真正良好的遙測系統最多並執行，很效率。 有一大堆與 Azure 中，整合的好夥伴而且其中部分可用層 – 讓您可以取得基本遙測執行任何動作。 以下是幾個項目目前可用在 Azure 上：

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

自 2015 年 3 月[Microsoft Application Insights for Visual Studio Online](https://azure.microsoft.com/en-us/documentation/articles/app-insights-get-started/)尚未發行，但也可以利用 嘗試預覽。[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#)也包含監視功能。

我們將快速逐步 New Relic 設定來顯示如何輕鬆它可以是使用遙測系統。

在 Azure 管理入口網站中註冊服務。 按一下**新增**，然後按一下 **存放區**。 **選擇附加元件** 對話方塊隨即出現。 向下捲動並按一下**New Relic**。

![選擇 附加元件](monitoring-and-telemetry/_static/image1.png)

按一下向右箭號，然後選擇您想要的服務層。 這個示範中，我們將使用免費層。

![個人化附加元件](monitoring-and-telemetry/_static/image2.png)

按一下向右箭號並確認 「 採購 」，New Relic 現在顯示為入口網站中的附加元件。

![檢閱 採購單](monitoring-and-telemetry/_static/image3.png)

![在管理入口網站中的新 Relic 附加元件](monitoring-and-telemetry/_static/image4.png)

按一下**連接資訊**，並將複製的授權金鑰。

![連接資訊](monitoring-and-telemetry/_static/image5.png)

移至**設定**索引標籤上，在入口網站中，web 應用程式設定**效能監視**至**附加元件**，並設定**選擇附加元件**下拉式清單來**New Relic**。 然後按一下 **儲存**。

![在 [設定] 索引標籤中的新 Relic](monitoring-and-telemetry/_static/image6.png)

在 Visual Studio 中，安裝應用程式中的新的 Relic NuGet 套件。

![在 [設定] 索引標籤中的開發人員分析](monitoring-and-telemetry/_static/image7.png)

將應用程式部署至 Azure，並開始使用它。 建立幾個修正它的工作提供一些 New relic，若要監視的活動。

然後返回**New Relic**頁面**附加元件** 索引標籤的入口網站，然後按一下**管理**。 在入口網站會傳送至 New Relic 管理入口網站中，因此您不需要再次輸入認證，使用單一登入進行驗證。 [概觀] 頁面會顯示各種效能統計資料。 （按一下以查看概觀頁面的完整大小的影像）。

[![新的 Relic 監視 索引標籤](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

以下是幾個您所見的統計資料：

- 在每天不同時間的平均回應時間。

    ![回應時間](monitoring-and-telemetry/_static/image10.png)
- 輸送量速率 （以每分鐘的要求） 在每天不同時間。

    ![輸送量](monitoring-and-telemetry/_static/image11.png)
- 處理不同的 HTTP 要求所花費的伺服器 CPU 時間。

    ![網頁交易時間](monitoring-and-telemetry/_static/image12.png)
- CPU 花費在應用程式程式碼的不同部分：

    ![追蹤的詳細資訊](monitoring-and-telemetry/_static/image13.png)
- 記錄效能統計資料。

    ![歷程記錄的效能](monitoring-and-telemetry/_static/image14.png)
- 服務已呼叫外部服務，例如 Blob 服務和如何可靠且回應迅速的相關統計資料。

    ![外部服務](monitoring-and-telemetry/_static/image15.png)

    ![外部服務](monitoring-and-telemetry/_static/image16.png)

    ![外部服務](monitoring-and-telemetry/_static/image17.png)
- 有關在哪裡世界，或在美國的 web 應用程式流量來自何處。

    ![地理位置](monitoring-and-telemetry/_static/image18.png)

您也可以設定報告和事件。 比方說，您可以說出任何時間，您會開始看到錯誤，傳送電子郵件問題的警示的支援人員。

![報告](monitoring-and-telemetry/_static/image19.png)

New Relic 只是一個範例的遙測系統。您可以取得這些全部都從其他服務。 雲端的好處是，而不需要撰寫任何程式碼，以及最少或沒有費用，您突然可以取得更多有關如何使用您的應用程式和實際發生什麼您的客戶資訊。

<a id="log"></a>
## <a name="log-for-insight"></a>記錄檔以取得深入資訊

遙測封裝是理想的第一個步驟中，但您仍然必須檢測您自己的程式碼。 遙測服務會告訴您發生問題時，並告訴您什麼客戶遇到，但它可能不會產生大量的深入了解您的程式碼中。

您不想要到實際執行伺服器，以查看您的應用程式正在執行遠端必須。 在您有一部伺服器，但要如何得知您已經調整到數百部伺服器，而且您不知道您需要遠端到哪些可能實際嗎？ 您的記錄應該提供足夠的資訊，您不再需要遠端到實際執行伺服器，來分析和偵錯問題。 您應該記錄足夠的資訊，讓您可以找出問題等作業只透過記錄檔。

### <a name="log-in-production"></a>在生產環境中的記錄

許多人沒有問題，而且他們想要偵錯時，才開啟在生產環境中的追蹤。 這樣可能會導致您注意的問題及取得其相關的有用疑難排解資訊的時間之間的經過長時間延遲。 且可能無法如間歇性的錯誤很有幫助您取得的資訊。

建議在其中儲存體是便宜的雲端環境中是永遠讓您在生產環境中登入。 這樣一來，當錯誤發生，您已經有這些記錄，且您有歷程記錄資料，可協助您分析經過一段時間開發，或定期在不同的時間，就可能發生的問題。 您無法自動清除處理序來刪除舊的記錄檔，但您可能會發現它是保留記錄檔超出設定這類處理程序更耗費資源。

記錄的額外的費用是資訊的一般相較於數量疑難排解時間和金錢，您可以儲存所有當發生錯誤時，您必須已提供。 然後當有人告訴您所具有的隨機錯誤一段時間約 8:00 前一晚，但它們不記得錯誤，您可以輕易地找出問題為何。

小於 $4 美元的月份，您就可以相反地，保持 50 gb 的記錄檔和記錄的效能影響是一般只要保留記住-若要避免效能瓶頸，請確定您記錄程式庫是非同步的一件事。

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>區別需要採取行動的記錄檔通知的記錄檔

記錄檔，旨在的 INFORM （我想要知道項目） 或 （我想要執行某個動作） 的動作。 請小心只寫入真的需要採取動作的人或自動化程序問題的動作記錄檔。 太多的動作記錄將會建立雜訊，需要太多工作来詳查它所有以找出真正的問題。 如果您的動作記錄檔會自動觸發某些動作，例如傳送電子郵件給支援人員，避免讓數千個這類動作會觸發單一問題。

在.NET System.Diagnostics 追蹤記錄檔可以指派錯誤、 警告、 資訊及偵錯/詳細資訊層級。 您可以從 INFORM 記錄檔區分 ACT，保留錯誤層級的動作記錄檔，並使用較低層級 INFORM 記錄檔。

![記錄層級](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>在執行階段設定的記錄層級

雖然需要有記錄會一律生產環境中，另一個的最佳作法是實作可讓您在執行階段調整的登入您的詳細資料，而不需重新部署或重新啟動您的應用程式層級的記錄架構。 例如當您使用中的和追蹤功能`System.Diagnostics`錯誤、 警告、 資訊，您可以建立及偵錯/詳細資訊記錄。 我們建議您一律記錄錯誤、 警告和資訊記錄檔實際執行環境，並需要能夠以動態方式將偵錯/詳細資訊記錄，以進行疑難排解的案例為基礎。

Azure App Service 中的 web 應用程式已內建的書寫支援`System.Diagnostics`記錄至檔案系統中，資料表儲存體或 Blob 儲存體。 您可以選取不同的記錄層級上每個儲存體目的地，而且您可以變更即時的記錄層級不需要重新啟動您的應用程式。 Blob 儲存體支援可讓您更輕鬆地執行[HDInsight](https://docs.microsoft.com/azure/hdinsight/)上您應用程式記錄檔，因為 HDInsight 知道如何直接使用 Blob 儲存體作業的分析。

### <a name="log-exceptions"></a>記錄例外狀況

不要只放置*例外狀況。Tostring （)*記錄程式碼中。 還剩下出的內容資訊。 在 SQL 錯誤的情況下它省去了 SQL 錯誤號碼。 所有例外狀況，包括內容資訊、 例外狀況本身和內部例外狀況以確定您提供的所有項目所需進行疑難排解。 例如，內容資訊可能包括伺服器名稱、 交易識別碼和使用者名稱 （但不是密碼或任何機密 ！）。

如果您依賴每一個開發人員做正確的事，記錄的例外狀況，其中部分將不會。 若要確定它取得完成操作右邊每次，建置 例外狀況處理，請直接將記錄器介面： 傳遞例外狀況物件記錄器類別，然後適當地登入記錄器類別的例外狀況資料。

### <a name="log-calls-to-services"></a>記錄檔呼叫服務

我們強烈建議您將資料寫入記錄每次您的應用程式呼叫服務時，不論它是資料庫或 REST API 或任何外部服務。 在您記錄中包含不僅成功或失敗，但每個要求所花費的指示。 在雲端環境中，您通常會看到與速度，而不是完整的潛在性中斷問題相關的問題。 通常要花費 10 毫秒的項目可能會突然開始進行第二個的動作。 當有人告訴您應用程式變慢時，您想要能夠查看 New Relic 或任何遙測服務，也可以驗證員工的體驗，然後想要尋找您自己的記錄檔，以深入了解為什麼速度慢的詳細資料。

### <a name="use-an-ilogger-interface"></a>使用 ILogger 介面

我們建議您建立生產應用程式時進行的工作是要建立簡單*ILogger*介面，並在其中堅持某些方法。 這可讓您輕鬆地變更記錄實作之後，不必瀏覽執行的所有程式碼。 我們無法使用`System.Diagnostics.Trace`類別整個修正它應用程式，但我們會改為使用它在幕後記錄類別實作中*ILogger*，我們進行*ILogger*整個呼叫方法應用程式。

這樣一來，如果您想要讓您記錄更豐富的您可以取代[ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs)想與任何其他記錄機制。 比方說，您的應用程式茁壯您可能會決定您想要更廣泛的記錄封裝使用如[NLog](http://nlog-project.org/)或[企業程式庫記錄應用程式區塊](https://msdn.microsoft.com/en-us/library/dn440731(v=pandp.60).aspx)。 ([Log4Net](http://logging.apache.org/log4net/)是另一個熱門的記錄架構，但它不會執行非同步的記錄。)

使用的架構，例如 NLog 一個可能的原因是有助於分割成個別的高容量和高價值資料存放區記錄輸出。 這將有助於您有效率地儲存大型 INFORM 資料，您不需要快速針對執行查詢，同時維持快速存取 ACT 資料磁碟區。

### <a name="semantic-logging"></a>語意的記錄

一個相對新的方法可產生更實用的診斷資訊的記錄，請參閱[企業程式庫語意記錄應用程式區塊 (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)。 平台會使用[Windows 事件追蹤](https://msdn.microsoft.com/en-us/library/windows/desktop/bb968803.aspx)(ETW) 和[EventSource](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracing.eventsource.aspx)支援在.NET 4.5，您可以建立多個結構化和可查詢的記錄檔中。 您定義每種類型的事件記錄時，可讓您自訂的資訊，您撰寫不同的方法。 例如，若要記錄的 SQL 資料庫錯誤，您可能會呼叫`LogSQLDatabaseError`方法。 針對該類型的例外狀況中，您知道金鑰的資訊是錯誤號碼，讓您無法在方法簽章中包含錯誤數字參數並做為個別的欄位，您撰寫的記錄檔記錄中記錄的錯誤號碼。 在個別的欄位數目，所以您可以更輕鬆並穩定地取得根據 SQL 錯誤號碼，如果您只成訊息的字串串連的錯誤號碼可以報告。

## <a name="logging-in-the-fix-it-app"></a>其記錄在修正應用程式

### <a name="the-ilogger-interface"></a>ILogger 介面

以下是*ILogger*修正它應用程式中的介面。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

這些方法可讓您撰寫在相同的四個層級所支援的記錄檔*System.Diagnostics*。 TraceApi 方法都是外部的服務呼叫記錄延遲的相關資訊。 您也可以新增一組方法，以偵錯/詳細資訊層級。

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>記錄器實作 ILogger 介面

介面的實作是非常簡單。 它基本上只會呼叫納入標準*System.Diagnostics*方法。 下列程式碼片段示範這三個資訊的方法，而另一個每個其他。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>呼叫 ILogger 方法

每次它修正應用程式中的程式碼攔截例外狀況，它會呼叫*ILogger*方法，以記錄例外狀況詳細資料。 每次會在資料庫、 Blob 服務或 REST API 呼叫，它會啟動馬錶呼叫之前，停止馬錶服務傳回時，並和記錄檔，以及成功或失敗的相關資訊的時間。

請注意記錄檔訊息中包含的類別名稱和方法名稱。 它是最好的作法是確定記錄檔訊息，識別應用程式程式碼的哪個部分撰寫它們。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

現在針對每次它修正應用程式發出 SQL 資料庫呼叫，呼叫的方法呼叫它，請參閱 < 以及完全多少時間所需。

![記錄檔中的 SQL 資料庫查詢](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

如果您移瀏覽記錄檔，您可以看到資料庫呼叫所花的時間是變數。 資訊可能很實用： 因為應用程式記錄這所有您可以分析中的資料庫服務如何執行一段時間的歷史趨勢。 比方說，服務可能會快速大部分的情況下，但可能會失敗的要求或回應可能會在每天特定時間變慢。

您可以執行 Blob 服務 – 相同的動作，每次應用程式上傳新的檔案，沒有記錄，且您可以看到每個檔案上傳所花費的時間完全。

![Blob 上傳記錄檔](monitoring-and-telemetry/_static/image23.png)

是幾個額外的程式碼來撰寫每次呼叫服務，而且現在只要有人說它們碰到問題，您知道完全問題為何，或是否發生錯誤時，即使在只執行速度很慢。 您可以找出問題的來源，而不需要遠端伺服器，或開啟記錄之後發生的錯誤，並且希望能夠重新建立它。

## <a name="dependency-injection-in-the-fix-it-app"></a>相依性插入修正它的應用程式

您可能想知道如何在上面顯示的範例中的儲存機制建構函式會取得記錄器介面實作：

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

若要將介面實作應用程式的連接會使用[相依性插入](http://en.wikipedia.org/wiki/Dependency_injection)(DI) 與[AutoFac](http://autofac.org/)。 DI 可讓您使用許多不同程式碼中的介面為基礎的物件，而且只需要指定在一處取得介面具現化時使用的實作。 這可讓您更輕鬆地變更實作： 例如，您可能想要取代 NLog 記錄器 System.Diagnostics 記錄器。 或者，您可能要取代的記錄器模擬版本進行自動化測試。

修正它的應用程式使用 DI 中所有儲存機制和所有控制器。 控制器類別的建構函式取得*ITaskRepository*介面相同的方式儲存機制取得記錄器介面：

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

應用程式使用 AutoFac DI 程式庫會自動提供*TaskRepository*和*記錄器*這些建構函式的執行個體。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

這段程式碼基本上表示的任何位置建構函式需要*ILogger*介面，請執行個體中傳遞*記錄器*類別，以及每當需要*IFixItTaskRepository*介面，請執行個體中傳遞*FixItTaskRepository*類別。

[AutoFac](http://autofac.org/)是您可以使用許多相依性插入架構的其中一個。 另一個熱門的一種是[Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx)，是建議，由 Microsoft Patterns and Practices 支援。

## <a name="built-in-logging-support-in-azure"></a>在 Azure 中的內建的記錄支援

Azure 支援下列種類的[Azure App Service 中的 Web 應用程式記錄](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics 追蹤 （您可以開啟和關閉和即時設定層級，不需要重新啟動站台）。
- Windows 事件。
- IIS 記錄檔 (HTTP/FREB)。

Azure 支援下列種類的[登入雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics 追蹤。
- 效能計數器。
- Windows 事件。
- IIS 記錄檔 (HTTP/FREB)。
- 監視的自訂目錄。

修正它應用程式使用 System.Diagnostics 追蹤。 您需要如何做以讓登入 web 應用程式的 System.Diagnostics 的就是在入口網站中翻轉交換器，或呼叫 REST API。 在入口網站中，按一下**組態**索引標籤上，為您的網站，並向下捲動以查看**Application Diagnostics** > 一節。 您可以開啟或關閉記錄，並選取您想要的記錄層級。 您可以將記錄檔寫入檔案系統或儲存體帳戶的 Azure。

![應用程式診斷和站台設定 索引標籤中的診斷](monitoring-and-telemetry/_static/image24.png)

啟用登入 Azure 之後，您可以在建立時看到 Visual Studio 輸出 視窗中的記錄檔。

![資料流處理的記錄檔 功能表](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![資料流處理的記錄檔 功能表](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

您也可以將記錄寫入儲存體帳戶，並檢視任何工具，可以存取 Azure 儲存體資料表服務，例如**伺服器總管**Visual Studio 中或[Azure 儲存體總管](https://azure.microsoft.com/features/storage-explorer/)。

![在伺服器總管 中的記錄檔](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>總結

它是真正簡易實作的方塊外遙測系統、 檢測您自己的程式碼中的記錄和設定 Azure 中的記錄。 而且，當生產問題，遙測系統與自訂記錄檔的組合可協助您快速地解決問題，它們會成為主要問題為您的客戶之前。

在[下一章](transient-fault-handling.md)我們將探討如何處理暫時性錯誤，因此它們不會變得生產問題，您必須調查。

## <a name="resources"></a>資源

如需詳細資訊，請參閱下列資源。

主要相關遙測的文件：

- [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/en-us/library/dn568099.aspx)。 請參閱檢測和遙測指引、 服務計量指引、 健康情況端點監控模式和執行階段重新設定模式。
- [就在雲端中的貨幣值： 啟用 New Relic 效能監視在 Azure 網站上](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx)。
- [Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。 請參閱遙測和診斷 > 一節。
- [使用 Application Insights 的下一代開發](https://msdn.microsoft.com/en-us/magazine/dn683794.aspx)。 MSDN Magazine 文件。

記錄有關的主要文件集：

- [語意記錄應用程式區塊 (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)。 Neil Mackenzie 呈現 SLAB 語意記錄的情況。
- [建立結構化和有意義的記錄檔與語意記錄](https://channel9.msdn.com/Events/Build/2013/3-336)。 （影片）凱撒 Dominguez 呈現 SLAB 語意記錄的情況。
- [EF6 SQL 記錄 – 第 1 部： 簡單的記錄](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/)。 Arthur Vickers 示範如何將記錄在 EF 6 的 Entity Framework 所執行的查詢。
- [連接恢復功能和 Entity framework，ASP.NET MVC 應用程式中的命令攔截](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)。 第四個九部分的教學課程系列，示範如何使用 EF 6 命令攔截功能來記錄由 Entity Framework 傳送至資料庫的 SQL 命令。
- [改善使用 C# 5.0 呼叫端資訊屬性的記錄](http://www.dotnetcurry.com/showarticle.aspx?ID=972)。 如何在不含硬式編碼它到常值或使用反映來取得它以手動方式，輕鬆記錄呼叫方法的名稱。

主要是有關疑難排解文件集：

- [Azure 的疑難排解&amp;偵錯部落格](https://blogs.msdn.com/b/kwill/)。
- [AzureTools – 診斷公用程式使用 Azure 開發人員支援小組](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)。 介紹並提供一種工具，可以在 Azure VM 上用來下載並執行各種不同的診斷和監視工具的下載連結。 當您需要以診斷特定 VM 上的問題時很有用。
- [疑難排解 web 應用程式中使用 Visual Studio 的 Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)。 開始使用 System.Diagnostics 追蹤和遠端偵錯的逐步教學課程。

影片：

- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 九部分 Ulrich Homann、 Marc Mercuri 和 Mark Simms 的系列。 高層級概念與架構原則非常可存取且有趣的方式，呈現劇本取自與實際客戶的 Microsoft 客戶諮詢團隊 (CAT) 體驗。 劇集 4，9 個有關監控與遙測。 時段 9 包括監視服務 MetricsHub、 AppDynamics、 New Relic 和 PagerDuty 的概觀。
- [建置大型： 學到來自 Azure 客戶-II](https://channel9.msdn.com/Events/Build/2012/3-030)。 Mark Simms 交談有關失敗的設計和檢測的所有項目。 類似於保全數列但進入詳細的使用說明。

程式碼範例：

- [雲端服務在 Azure 中的基本概念](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)。 Microsoft Azure 客戶諮詢小組所建立的範例應用程式。 示範下列文件中所述的遙測及記錄作法。 此範例會實作應用程式記錄使用[NLog](http://nlog-project.org/)。 如需相關的文件，請參閱[四個有關遙測和記錄的 TechNet wiki 文章系列](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon)。

>[!div class="step-by-step"]
[上一頁](design-to-survive-failures.md)
[下一頁](transient-fault-handling.md)
