---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 附錄︰ 修正它範例應用程式 （建置真實世界雲端應用程式與 Azure） |Microsoft 文件
author: MikeWasson
description: Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876471"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>附錄︰ 修正它範例應用程式 （建置真實世界雲端應用程式與 Azure）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載該專案的修正](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


建立真實世界雲端應用程式與 Azure 的電子書本附錄包含下列各節提供有關修正它範例應用程式，您可以下載的其他資訊：

- [已知的問題](#knownissues)
- [最佳做法](#bestpractices)
- [如何在本機電腦上從 Visual Studio 執行應用程式](#run-in-vs)
- [如何使用 Windows PowerShell 指令碼，將基底應用程式部署至 Azure App Service Web 應用程式](#deploybase)
- [疑難排解 Windows PowerShell 指令碼](#troubleshooting)
- [如何部署應用程式與處理 Azure App Service Web 應用程式和 Azure 雲端服務的佇列](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>已知問題

修正它應用程式原本是為了說明以盡可能簡單地這個 e 活頁簿中的某些模式呈現所開發。 不過，由於建置真實世界應用程式的相關電子書，我們必須修正它的程式碼檢閱和測試程序類似於我們還會做為發行的軟體。 我們發現許多問題，並如同任何真實世界應用程式中，我們已修正其中部分且其中部分我們延後至較新版本。

下列清單包含問題，應該解決實際執行應用程式中，但在其中一個原因或另一個我們決定不修正它範例應用程式的初始版本中的位址。

### <a name="security"></a>安全性

- 請確定您無法將工作指派給不存在的擁有者。
- 確定您只能檢視和修改您建立或指派給您的工作。
- 使用 HTTPS 登入頁面和驗證 cookie。
- 指定的驗證 cookie 的時間限制。

### <a name="input-validation"></a>輸入的驗證

一般情況下，生產環境應用程式會執行輸入驗證比多修正它應用程式。 例如，影像大小 / 映像上傳應該限制允許的檔案大小。

### <a name="administrator-functionality"></a>系統管理員功能

系統管理員應該能夠變更現有的工作上的擁有權。 例如，工作的建立者可能會離開公司，沒有人離開維護工作，除非已啟用系統管理存取權的授權單位。

### <a name="queue-message-processing"></a>佇列的訊息處理

佇列中的訊息處理它修正應用程式被設計為簡化，以便說明最小量的程式碼以佇列為主的工作模式。 這個簡單的程式碼不是足夠用於實際執行應用程式。

- 程式碼並不保證會處理一次的每個佇列的訊息。 當您從佇列取得訊息時，但沒有逾時期限，在訊息是看不到其他佇列接聽程式。 如果在逾時過期之前刪除訊息，再次顯示訊息。 因此，如果背景工作角色執行個體花費很長的時間處理訊息，它是相同的訊息處理兩次，理論上可能導致資料庫中的重複工作。 如需有關此問題的詳細資訊，請參閱[使用 Azure 儲存體佇列](https://msdn.microsoft.com/library/ff803365.aspx#sec7)。
- 藉由批次處理訊息擷取佇列輪詢邏輯可能更符合成本效益。 每次呼叫[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)，會有交易成本。 相反地，您可以呼叫[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (請注意複數形式的 ')，此 cmdlet 會取得在單一交易中的多個訊息。 Azure 儲存體佇列的交易成本是很低的因此不在大部分情況下大量成本的影響。
- 緊密迴圈中的佇列訊息處理程式碼會使 CPU 親和性，不會有效地使用多核心的 Vm。 更好的設計會使用工作平行處理原則，以平行方式執行幾項非同步工作。
- 佇列的訊息處理有只初步的例外狀況處理。 例如，程式碼不會處理[有害訊息](https://msdn.microsoft.com/library/ms789028.aspx)。 （訊息處理就會導致例外狀況，您必須將錯誤記錄，並刪除郵件，或背景工作角色將會嘗試處理它，而迴圈會繼續執行無限期。）

### <a name="sql-queries-are-unbounded"></a>SQL 查詢都不受限制

目前修正它的程式碼會用於索引頁的查詢可能傳回的資料列數目沒有限制。 如果大量的工作輸入到資料庫時，收到的結果清單的大小可能會導致效能問題。 解決方法是實作分頁。 如需範例，請參閱[排序、 篩選和分頁與 ASP.NET MVC 應用程式的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)。

### <a name="view-models-recommended"></a>建議您檢視模型

修正它應用程式使用 FixItTask 實體類別的控制器和檢視之間傳遞資訊。 最佳作法是使用檢視模型。 網域模型 （例如 FixItTask 實體類別） 是針對設計所需的資料持續性，而檢視模型可以針對資料的呈現。 如需詳細資訊，請參閱[12 ASP.NET MVC 最佳作法](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)。

### <a name="secure-image-blob-recommended"></a>建議使用的安全映像 blob

修正它應用程式市集上傳映像為公用，這表示 URL 會尋找任何人可以存取的映像。 無法保護映像，而不是公用。

### <a name="no-powershell-automation-scripts-for-queues"></a>佇列沒有 PowerShell 自動化指令碼

只有修正它完全在 Azure App Service Web 應用程式中執行的基本版本所撰寫的範例 PowerShell 自動化指令碼。 我們尚未提供指令碼以設定和部署 web 應用程式，以及所需的佇列處理的雲端服務環境。

### <a name="special-handling-for-html-codes-in-user-input"></a>使用者輸入中的 HTML 程式碼的特殊處理

ASP.NET 會自動防止許多方式在其中惡意使用者可能會在使用者輸入的文字方塊中輸入指令碼嘗試跨網站指令碼的攻擊。 和 MVC`DisplayFor`協助程式用來顯示工作項目和資訊會自動針對它傳送至瀏覽器進行 HTML 編碼的值。 但在生產環境應用程式可能會想要採取其他措施。 如需詳細資訊，請參閱[中 ASP.NET 的要求驗證](https://msdn.microsoft.com/library/hh882339.aspx)。

<a id="bestpractices"></a>
## <a name="best-practices"></a>最佳作法

以下是在程式碼檢閱中發現及測試它修正應用程式的原始版本已修正後一些問題。 部分所引起原始的編碼器不留意特定的最佳作法，一些只因為此程式碼以快速地撰寫並不適合發行的軟體。 我們正在列出的問題，以防萬一發生我們所了解從這個檢閱的項目和測試，可能會很有幫助其他人也開發 web 應用程式。

### <a name="dispose-the-database-repository"></a>處置資料庫儲存機制

`FixItTaskRepository`類別必須處置 Entity Framework`DbContext`執行個體。 我們採用的方法來實作`IDisposable`中`FixItTaskRepository`類別：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

請注意，自動將處置 AutoFac`FixItTaskRepository`執行個體，因此我們不需要明確處置它。

另一個選項是移除`DbContext`成員變數，從`FixItTaskRepository`，而是建立本機`DbContext`變數在每個儲存機制方法中，內部`using`陳述式。 例如: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>在這種情況將 singleton 註冊 DI 與

因為只有一個執行個體`PhotoService`類別和`Logger`需要類別，這些類別應[註冊相依性插入的單一執行個體為](https://code.google.com/p/autofac/wiki/InstanceScope)中*DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>安全性： 不向使用者顯示錯誤詳細資料

原始修正它的應用程式未有一般錯誤頁面，並就讓所有的例外狀況泡泡，最多 UI，讓某些例外狀況，例如資料庫連線錯誤可能會導致瀏覽器顯示完整的堆疊追蹤。 詳細的錯誤資訊，有時也有助於惡意使用者攻擊。 方案是記錄例外狀況詳細資料，並顯示錯誤頁面，使用者不會包含錯誤詳細資料。 修正它應用程式已記錄，以及為了顯示錯誤頁面，我們加入了`<customErrors mode=On>`Web.config 檔案中。

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

依預設這會導致*Views\Shared\Error.cshtml*来顯示的錯誤。 您可以自訂*Error.cshtml*或建立您自己的錯誤頁面檢視並新增`defaultRedirect`屬性。 您也可以指定不同的錯誤頁面，針對特定錯誤。

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>安全性： 只允許由其建立者編輯工作

儀表板索引頁面只會顯示登入的使用者所建立的工作，但惡意使用者可以建立 URL 給其他使用者的工作識別碼。 我們加入程式碼中的*DashboardController.cs*在此情況下傳回 404:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>不要忍受例外狀況

原始修正它的應用程式只傳回了 null 登入的 SQL 查詢所造成的例外狀況之後：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

這會使它看起來向使用者查詢成功，但只要未傳回任何資料列。 解決方案是重新擲回的例外狀況之後攔截及記錄：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>在背景工作角色中攔截所有例外狀況

任何未處理的例外狀況，以背景工作角色將導致回收 VM，所以您想要將所有項目包裝在 try catch 區塊中執行並處理所有例外狀況。

### <a name="specify-length-for-string-properties-in-entity-classes"></a>在實體類別中指定的字串屬性的長度

為了顯示簡單的程式碼，修正它應用程式的原始版本未指定，在 FixItTask 實體的欄位長度，因此它們被定義為 varchar （max） 資料庫中。 因此，UI 會接受幾乎任何數量的輸入。 指定的長度設定限制，同時套用至使用者輸入中的網頁和資料庫中的資料行大小：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>將私用成員標記為 readonly，當它們不應該變更

例如，在`DashboardController`類別的執行個體`FixItTaskRepository`建立，且不應該變更，因此我們定義成[readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx)。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>使用清單。Any （) 而不是清單。Count （) &gt; 0

如果所有的您很重視您的清單中的一個或多個項目是否符合指定的準則，請使用[任何](https://msdn.microsoft.com/library/bb534972.aspx)方法，它就會傳回找到符合準則的項目，而因為`Count`方法一律必須逐一查看透過每個項目。 儀表板*Index.cshtml*檔案最初的這段程式碼：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

我們變更此：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>產生 Url 中使用 MVC helper 的 MVC 檢視

如**建立修正**按鈕在首頁上，修正它應用程式硬式編碼的錨定項目：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

檢視/動作連結，就像這樣的最好使用[Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML helper，例如：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>背景工作角色，而非 Thread.Sleep 使用 Task.Delay

將新專案範本`Thread.Sleep`在範例程式碼背景工作角色，而造成執行緒睡眠可能會導致執行緒集區繁衍 （spawn） 其他不必要的執行緒。 您可以藉由避免[Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx)改為。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>避免 async void

如果非同步方法不需要傳回值，傳回`Task`型別而非`void`。

這個範例取自`FixItQueueManager`類別： 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

您應該使用`async void`僅適用於最上層的事件處理常式。 如果您定義將方法當做`async void`，呼叫端無法**await**方法或攔截方法擲回任何例外狀況。 如需詳細資訊，請參閱[中非同步程式設計的最佳作法](https://msdn.microsoft.com/magazine/jj991977.aspx)。 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>中斷從背景工作角色迴圈中使用取消語彙基元

一般而言，**執行**工作者角色上的方法包含無限迴圈。 背景工作角色停止時，請[Application_end](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)方法呼叫。 您應該使用這個方法來取消工作內所進行**執行**方法並結束依正常程序。 否則，處理序可能會終止作業當中。

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>選擇退出自動 MIME 探查程序

在某些情況下，Internet Explorer 會報告不同的網頁伺服器所指定之類型的 MIME 類型。 比方說，如果 Internet Explorer HTML 內容中找到檔案，傳送 HTTP 回應標頭內容類型： text/plain Internet Explorer 會決定內容應該會轉譯為 HTML。 不幸的是，此 「 MIME 探查 」 也會導致裝載未受信任的內容伺服器的安全性問題。 為了打擊這個問題，Internet Explorer 8 MIME 類型判斷程式碼所做的變更數目和可讓應用程式開發人員[退出 MIME 探查](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)。 下列程式碼已加入*Web.config*檔案。

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>啟用統合及縮製

當 Visual Studio 建立新的 web 專案時，統合及縮製的 JavaScript 檔是預設不啟用。 我們在 BundleConfig.cs 加入一行程式碼：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>設定的驗證 cookie 的到期逾時

根據預設，驗證 cookie 會在兩週內到期。 較短的時間會比較安全。 您可以變更此設定在*StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>如何在本機電腦上從 Visual Studio 執行應用程式

有兩種方式可以執行它修正應用程式：

- 執行新工作將直接寫入至 SQL 資料庫的基底應用程式。
- 執行應用程式使用佇列加上後端服務建立的工作。 本章說明佇列模式[佇列為主的工作模式](queue-centric-work-pattern.md)。

<a id="runbase"></a>
### <a name="run-the-base-application"></a>執行基本的應用程式

1. 安裝[Visual Studio 2013 或 Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads)。
2. 安裝[Azure SDK for.NET for Visual Studio 2013。](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. 下載.zip 檔案從[MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)。
4. 在檔案總管] 中，以滑鼠右鍵按一下.zip 檔案並按一下 [內容]，然後在 [屬性] 視窗中按一下 [解除封鎖。
5. 解壓縮檔案。
6. 按兩下.sln 檔案，即可啟動 Visual Studio。
7. 從 [工具] 功能表中，按一下 [程式庫封裝管理員，則封裝管理員主控台]。
8. 在 封裝管理員主控台 (PMC)，按一下 還原。
9. 結束 Visual Studio。
10. 啟動[Azure 儲存體模擬器](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)。
11. 重新啟動 Visual Studio 中，開啟上一個步驟中您關閉方案檔案。
12. 請確定 FixIt 專案已設定為啟始專案，，然後按 CTRL + F5 執行專案。

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>執行應用程式佇列的處理

1. 依照指示[執行基底應用程式](#runbase)，然後關閉瀏覽器，然後關閉 Visual Studio。
2. 啟動 Visual Studio 系統管理員權限。 （您將使用 Azure 計算模擬器中，而且這需要系統管理員權限。）
3. 應用程式中*Web.config*檔案*MyFixIt*專案 （web 專案） 中，變更的值`appSettings/UseQueues`為"true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. 如果[Azure 儲存體模擬器](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)不是仍在執行中，再次啟動它。
5. 同時執行 FixIt web 專案與 MyFixItCloudService 專案。

    使用 Visual Studio 2013:

   1. 按 F5 執行 FixIt 專案。
   2. 在**方案總管 中**MyFixItCloudService 專案中，按一下滑鼠右鍵，然後按**偵錯** -- **開始新執行個體**。

      使用 Visual Studio 2013 Express for Web:

   3. 在 方案總管 FixIt 方案上按一下滑鼠右鍵，然後選取**屬性**。
   4. 選取**多個啟始專案**...
   5. 在**動作**MyFixIt 和 MyFixItCloudService，底下的下拉式清單選取**啟動**。
   6. 按一下 [確定 **Deploying Office Solutions**]。
   7. 按 F5 執行這兩個專案。

      當您執行 MyFixItCloudService 專案時，Visual Studio 會啟動 Azure 計算模擬器。 根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>如何使用 Windows PowerShell 指令碼，將基底應用程式部署至 Azure App Service Web 應用程式

為了說明[一切自動化](automate-everything.md)模式中，修正它應用程式提供給指令碼會設定在 Azure 中的環境，然後將專案部署到新環境。 下列指示說明如何使用指令碼。

如果您想要在 Azure 中執行，而不使用佇列，並進行變更，以執行本機佇列，請確定您回到 false 後再繼續使用下列指示設定 UseQueues appsetting owin： 值。

這些指示假設您已經下載並執行修正它方案在本機，而且您有 Azure 帳戶或具有 Azure 訂用帳戶已獲授權管理。

1. 安裝**Azure PowerShell**主控台。 如需指示，請參閱[如何安裝及設定 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。

    這個自訂的主控台設定為使用您的 Azure 訂用帳戶。 Azure 模組安裝在*Program Files*目錄以及自動匯入每次使用 Azure PowerShell 主控台。

    如果您想要使用不同的主機程式，例如 Windows PowerShell ISE，請務必使用[Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)匯入 Azure 模組，或使用 Azure 模組中的命令，來觸發自動匯入模組的 cmdlet。
2. 啟動 Azure PowerShell**系統管理員身分執行**選項。
3. 執行[Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet 來設定 Azure PowerShell 執行原則為`RemoteSigned`。 輸入**Y** （適用於 [是]) 來完成原則變更。

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    此設定可讓您執行不經過數位簽署的本機指令碼。 (您也可以將執行原則設`Unrestricted`，這就不需要解除封鎖步驟更新版本中，但基於安全性理由不建議這樣。)
4. 執行`Add-AzureAccount`使用您的帳戶認證設定 PowerShell cmdlet。

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    這些認證過期一段時間之後，您必須重新執行`Add-AzureAccount`cmdlet。 正在寫入此的電子書，則在認證到期前的時間限制為 12 小時。
5. 如果您有多個訂閱，可用於選取 AzureSubscription cmdlet 指定您想要建立測試環境中的訂用的帳戶。
6. 使用匯入相同的 Azure 訂用帳戶的管理憑證`Get-AzurePublishSettingsFile`和`Import-AzurePublishSettingsFile`cmdlet。 這些 cmdlet 的第一個下載的憑證檔案，並在第二個您指定才能將它匯入該檔案的位置。 > [!IMPORTANT]
   > 將下載的檔案保留在安全的位置，或刪除當您完成時，因為它包含可以用來管理您的 Azure 服務的憑證。

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    使用憑證的 REST API 呼叫，以便設定 SQL Database 伺服器上的防火牆規則偵測到開發電腦的 IP 位址。
7. 執行[Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (別名`cd`， `chdir`，和`sl`) 來瀏覽至包含的指令碼的目錄。 (它們位於*自動化*修正它的方案資料夾中的資料夾。)如果任何目錄名稱包含空格，請將以引號括住的路徑。 例如，若要瀏覽至`c:\Sample Apps\FixIt\Automation`目錄，您可以輸入下列命令：

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. 若要讓 Windows PowerShell 來執行這些指令碼，使用[Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet。 （因為它們已從網際網路下載會封鎖指令碼）。

    > [!WARNING]
    > 安全性-再執行`Unblock-File`上任何指令碼或可執行檔中，開啟 [記事本] 中的檔案、 檢查命令，並確認它們不包含任何惡意程式碼。

    例如，下列命令執行`Unblock-File`cmdlet 上目前的目錄中的所有指令碼。

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. 若要建立 web 應用程式基底 （沒有處理佇列） 修正此問題的應用程式，執行環境建立指令碼。

    所需`Name`參數指定的資料庫名稱，也會用於指令碼建立的儲存體帳戶。 名稱必須是 azurewebsites.net 網域內是唯一的。 如果您指定的名稱不是唯一的例如 Fixit 或測試 （或甚至是如 fixitdemo 範例），`New-AzureWebsite`指令程式會失敗並報告衝突發生內部錯誤。 指令碼會將名稱轉換為全部小寫，以符合用於 web 應用程式、 儲存體帳戶和資料庫的名稱需求。

    所需`SqlDatabasePassword`參數會指定將建立 SQL 資料庫系統管理員帳戶的密碼。 不要在密碼中包含的特殊 XML 字元 (&amp; &lt; &gt; ;)。 這是方式的指令碼所撰寫的 Azure 限制的限制。

    例如，如果您想要建立名為"fixitdemo 「 web 應用程式，並使用 「 Passw0rd1"的 SQL Server 系統管理員密碼，您可以輸入下列命令：

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    在 azurewebsites.net 網域中，名稱必須是唯一且密碼必須符合 SQL 資料庫密碼複雜性需求。 （範例 Passw0rd1 沒有符合的需求）。

    請注意，命令開始使用 」。\". 以協助防止惡意的指令碼執行 Windows PowerShell 會要求您提供的指令碼檔案的完整的路徑，當您執行指令碼。 您可以使用點來指出目前的目錄 (「。\")或者，提供完整的路徑，例如：

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    如需有關此指令碼的詳細資訊，請使用`Get-Help`cmdlet。

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    您可以使用`Detailed`， `Full`， `Parameters`，和`Examples`來篩選傳回的說明 Get-help cmdlet 的參數。

    如果指令碼失敗或產生錯誤，例如"New-azurewebsite:: 呼叫組 AzureSubscription 和 Select-azuresubscription 首先，「 您可能未完成 Azure PowerShell 的組態。

    在指令碼完成之後，您可以使用 Azure 管理入口網站，以查看所建立的資源中所示[一切自動化](automate-everything.md)章節。
10. 若要將 FixIt 專案部署到新的 Azure 環境中，使用*AzureWebsite.ps1*指令碼。 例如: 

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    當部署完成時，修正它在 Azure 中執行瀏覽器會開啟。

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>疑難排解 Windows PowerShell 指令碼

執行下列指令碼時遇到最常見的錯誤相關的權限。 請確定`Add-AzureAccount`和`Import-AzurePublishSettingsFile`成功，而您用於相同的 Azure 訂用帳戶。 即使`Add-AzureAccount`已成功您可能必須執行一次。 所加入的使用權限`Add-AzureAccount`在 12 小時後到期。

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>並未將物件參考設定為物件的執行個體。

如果指令碼會傳回錯誤，例如 「 物件參考未設定為物件的執行個體 」 （表示 Windows PowerShell 找不到處理程序 （這是 null 參考例外狀況） 的物件），執行`Add-AzureAccount`指令程式，然後再試一次指令碼。

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError： 伺服器發生內部錯誤。

`New-AzureWebsite`名稱不是唯一 azurewebsites.net 網域中時，cmdlet 會傳回發生內部錯誤。 若要解決錯誤，請使用不同的值做為名稱，也就是 Name 參數中*新增 AzureWebsiteEnv.ps1*。

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>重新啟動指令碼

如果您需要重新啟動*新增 AzureWebsiteEnv.ps1*指令碼因為它無法在列印 「 指令碼完成 」 訊息之前，您可能想要刪除之前建立的指令碼停止的資源。 比方說，如果指令碼已經建立 ContosoFixItDemo web 應用程式，並且具有相同名稱重新執行指令碼，指令碼將會失敗，因為名稱正在使用中。

若要判斷哪些資源停止之前所建立的指令碼，請使用下列 cmdlet:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`： 若要執行這個指令程式，使用管線傳送至資料庫伺服器名稱`Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

若要刪除這些資源，請使用下列命令。 請注意，如果您刪除的資料庫伺服器，您會自動刪除與伺服器相關聯的資料庫。

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>如何部署應用程式與處理 Azure App Service Web 應用程式和 Azure 雲端服務的佇列

若要啟用佇列，進行下列變更 MyFixIt\Web.config 檔案中。 在下`appSettings`，變更的值`UseQueues`為"true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

然後將 web 應用程式在 Azure 應用程式服務中，在 MVC 應用程式部署所述[早](#deploybase)。

接下來，建立新的 Azure 雲端服務。 修正它應用程式隨附的指令碼不要建立或部署雲端服務，因此您必須使用 Azure 入口網站。 在入口網站中，按一下**新增** -- **計算**–**雲端服務** -- **快速建立**，然後輸入 URL和資料中心位置。 使用相同的資料中心部署 web 應用程式的位置。

![](the-fix-it-sample-application/_static/image1.png)

您可以部署雲端服務之前，您需要更新部分組態檔案。

MyFixIt.WorkerRoler\app.config，在底下`connectionStrings`，取代的值`appdb`與實際的連接字串，SQL database 的連接字串。 您可以從入口網站取得的連接字串。 在入口網站中，按一下**SQL 資料庫** - **appdb** - **檢視 SQL Database 連接字串，如 Ado.net、 ODBC、 PHP 和 JDBC**。 複製 ADO.NET 連接字串，並將值貼到 app.config 檔案。 取代"{您\_密碼\_這裡}"與您的資料庫密碼。 (假設您使用指令碼來部署 MVC 應用程式，您指定的資料庫密碼`SqlDatabasePassword`指令碼參數。)

結果應該如下所示：

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

在相同的 MyFixIt.WorkerRoler\app.config 檔案中，在`appSettings`，取代兩個 Azure 儲存體帳戶的預留位置值。

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

您可以從入口網站取得的存取金鑰。 請參閱[如何管理儲存體帳戶](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)。

在 MyFixItCloudService\ServiceConfiguration.Cloud.cscfg，取代 Azure 儲存體帳戶相同的兩個預留位置值。

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

現在您已準備好要部署雲端服務。 在方案總管 中以滑鼠右鍵按一下 MyFixItCloudService 專案，然後選取**發行**。 如需詳細資訊，請參閱 「[部署至 Azure 應用程式](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"，這是在第 2 部分[本教學課程](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)。

> [!div class="step-by-step"]
> [上一步](more-patterns-and-guidance.md)
