---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 附錄︰ 修正範例應用程式 （建置使用 Azure 的真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 6f4fa7cf3746da0a6cdd4bd037fea509d488a59d
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578012"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>附錄︰ 修正範例應用程式 （建置使用 Azure 的真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Tom Dykstra](https://github.com/tdykstra)

[下載該專案的修正程式](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


建置真實世界的雲端應用程式與 Azure 的電子書本附錄包含下列各節提供它修正範例應用程式，您可以下載的其他資訊：

- [已知的問題](#knownissues)
- [最佳做法](#bestpractices)
- [如何在您的本機電腦上，從 Visual Studio 執行應用程式](#run-in-vs)
- [如何使用 Windows PowerShell 指令碼，將基底的應用程式部署至 Azure App Service Web Apps](#deploybase)
- [疑難排解 Windows PowerShell 指令碼](#troubleshooting)
- [如何使用佇列處理至 Azure App Service Web Apps 和 Azure 雲端服務部署應用程式](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>已知問題

修正其應用程式的原始開發以說明以盡可能簡單地在本電子書中的某些模式呈現。 不過，由於電子書，是關於建置真實世界應用程式，我們會受限於它修正程式碼檢閱和測試程序類似，我們會針對發行的軟體中執行的動作。 我們發現一些問題，並如同任何真實世界應用程式中，我們已修正的其中一些其中一些我們延後到更新版。

下列清單包含應該加以解決的問題，在生產環境應用程式，但其中一個原因，或另一個我們決定不修正它範例應用程式的初始版本中的位址。

### <a name="security"></a>安全性

- 請確定您無法將工作指派給不存在的擁有者。
- 請確定您只可以檢視及修改您建立或指派給您的工作。
- 使用 HTTPS 登入頁面和驗證 cookie。
- 指定驗證 cookie 的時間限制。

### <a name="input-validation"></a>輸入的驗證

生產應用程式一樣在一般情況下，多個比它修正應用程式輸入驗證。 例如，影像大小 / 映像上傳應該限制允許的檔案大小。

### <a name="administrator-functionality"></a>系統管理員功能

系統管理員應該能夠變更現有工作的擁有權。 例如，工作的建立者可能會離開公司，沒有人離開維護工作，除非已啟用系統管理存取權的授權單位。

### <a name="queue-message-processing"></a>佇列訊息處理

修正其應用程式中處理的佇列訊息被設計為簡化，以便說明佇列為中心的工作模式與最少程式碼。 這個簡單的程式碼不是適用於實際生產應用程式。

- 程式碼並不保證會處理最多一次，每個佇列訊息。 當您從佇列收到訊息時，沒有逾時期限，看不到其他佇列接聽程式訊息的期間。 如果在逾時到期之前刪除訊息，再次顯示訊息。 因此，如果背景工作角色執行個體花費很長的時間處理訊息，它是理論上，相同的訊息才會處理兩次，導致資料庫中的重複工作。 如需有關此問題的詳細資訊，請參閱 <<c0> [ 使用 Azure 儲存體佇列](https://msdn.microsoft.com/library/ff803365.aspx#sec7)。
- 佇列輪詢邏輯可能是更符合成本效益，批次擷取訊息。 每次呼叫[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)，沒有交易成本。 相反地，您可以呼叫[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (請注意的複數 ')，其中在單一交易中取得多個訊息。 Azure 儲存體佇列的交易成本是非常低的因此不是在大部分情況下大量成本的影響。
- 緊密迴圈中的佇列訊息處理程式碼會導致 CPU 親和性，不會有效率地使用多核心 Vm。 更好的設計會使用工作平行處理原則，來平行執行數個非同步工作。
- 佇列的訊息處理具有僅基本的例外狀況處理。 比方說，程式碼不會處理[有害訊息](https://msdn.microsoft.com/library/ms789028.aspx)。 （時處理訊息導致例外狀況，您必須將錯誤記錄，並刪除訊息，或背景工作角色將會嘗試處理它一次，而且迴圈將繼續無限期地執行）。

### <a name="sql-queries-are-unbounded"></a>SQL 查詢都不受限制

目前修正它的程式碼會用於索引頁的查詢可能傳回的資料列數目沒有限制。 如果大量的工作輸入至資料庫時，收到的結果清單的大小可能會導致效能問題。 解決方法是實作分頁。 如需範例，請參閱[排序、 篩選和分頁與 ASP.NET MVC 應用程式中的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)。

### <a name="view-models-recommended"></a>建議的檢視模型

修正其應用程式會使用 FixItTask 實體類別的控制器和檢視之間傳遞資訊。 最佳做法是使用檢視模型。 網域模型 （例如 FixItTask 實體類別） 被專為所需的資料持續性，而檢視模型可設計以呈現資料。 如需詳細資訊，請參閱 < [12 ASP.NET MVC 最佳作法](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)。

### <a name="secure-image-blob-recommended"></a>建議的安全映像 blob

修正它的應用程式市集上傳映像為公用，亦即找到 URL 的任何人可以存取映像。 映像無法受到保護，而非公用。

### <a name="no-powershell-automation-scripts-for-queues"></a>佇列的任何 PowerShell 自動化指令碼

僅為修正它完全在 Azure App Service Web Apps 中執行的基底版本撰寫的範例 PowerShell 自動化指令碼。 我們尚未提供指令碼來設定並部署至 web 應用程式和佇列處理所需的雲端服務環境。

### <a name="special-handling-for-html-codes-in-user-input"></a>在使用者輸入中的 HTML 程式碼的特殊處理

ASP.NET 會自動防止的多種資訊，請在其中惡意使用者可能會在使用者輸入的文字方塊中輸入指令碼嘗試跨網站指令碼攻擊。 和 MVC`DisplayFor`協助程式用來顯示工作項目和資訊自動傳送至瀏覽器的 HTML 編碼值。 但在生產應用程式中，您可能想要採取額外的量值。 如需詳細資訊，請參閱 <<c0> [ 要求的驗證，在 ASP.NET 中](https://msdn.microsoft.com/library/hh882339.aspx)。

<a id="bestpractices"></a>
## <a name="best-practices"></a>最佳作法

以下是一些探索到的程式碼檢閱和測試修正其應用程式的原始版本之後修正的問題。 某些所造成的原始對於不留意特定的最佳作法，有些只因為程式碼快速寫入，而不適用於發行的軟體。 我們要列出的問題，萬一有一點我們發現，這個檢閱和測試，可能會很有幫助其他人也正在開發 web 應用程式。

### <a name="dispose-the-database-repository"></a>處置資料庫存放庫

`FixItTaskRepository`類別必須處置 Entity Framework`DbContext`執行個體。 我們採用的方法來實作`IDisposable`在`FixItTaskRepository`類別：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

請注意，將會自動處置 AutoFac`FixItTaskRepository`執行個體，所以我們不需要明確處置它。

另一個選項是移除`DbContext`成員變數，從`FixItTaskRepository`，而改為建立本機`DbContext`變數中每個存放庫方法，在`using`陳述式。 例如: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>使用 DI，因此註冊單次個體

因為只有一個執行個體`PhotoService`類別以及`Logger`需要類別，這些類別都應該[註冊為相依性插入的單一執行個體](https://code.google.com/p/autofac/wiki/InstanceScope)在*DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>安全性： 不要向使用者顯示錯誤詳細資料

原始修正它的應用程式未有一般錯誤頁面，而讓所有例外狀況反昇到 UI 中，讓某些例外狀況，例如資料庫連線錯誤可能會導致顯示在瀏覽器的完整堆疊追蹤。 詳細的錯誤資訊有時可以促進遭到惡意使用者攻擊的。 解決方案是記錄例外狀況詳細資料，並不包含錯誤詳細資料對使用者顯示錯誤頁面。 已經記錄它修正應用程式，並以顯示錯誤頁面，我們已新增`<customErrors mode=On>`Web.config 檔案中。

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

依預設這會導致*Views\Shared\Error.cshtml*要顯示的錯誤。 您可以自訂*Error.cshtml*或建立您自己的錯誤頁面檢視，並新增`defaultRedirect`屬性。 您也可以指定不同的錯誤網頁的特定錯誤。

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>安全性： 只允許其建立者可以編輯工作

儀表板索引頁面只會顯示登入的使用者，所建立的工作，但惡意使用者可以建立另一位使用者的工作識別碼的 URL。 我們加入程式碼中的*DashboardController.cs*傳回 404，在此情況下：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>不要抑制例外狀況

原始修正它的應用程式只傳回了 null 之後記錄例外狀況所產生的 SQL 查詢：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

這會讓它看起來給使用者時，如果查詢成功，但只要未傳回任何資料列。 解決方案是重新擲回的例外狀況之後攔截和記錄：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>在 背景工作角色中攔截所有例外狀況

任何未處理的例外狀況，以背景工作角色將導致回收 VM，因此您想要所有項目包裝在 try / catch 區塊中執行並處理所有例外狀況。

### <a name="specify-length-for-string-properties-in-entity-classes"></a>實體類別中指定的字串內容長度

若要顯示簡單的程式碼，修正其應用程式的原始版本未指定 FixItTask，實體的欄位長度，因此它們會定義為 varchar （max），在資料庫中。 如此一來，UI 會接受任何數量的輸入。 同時套用到使用者的指定長度集限制輸入中的網頁和資料庫中的資料行大小：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>將私用成員標示為 readonly，當他們不應該變更

例如，在`DashboardController`類別的執行個體`FixItTaskRepository`建立，且不應該變更，我們把它做為定義[readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx)。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>使用清單。而不是清單 any （)。Count （) &gt; 0

如果您所有您想知道在清單中的一或多個項目是否符合指定的條件，請使用[任何](https://msdn.microsoft.com/library/bb534972.aspx)方法，因為它會傳回當找到符合準則的項目，而`Count`方法一律必須逐一查看透過每個項目。 儀表板*Index.cshtml*檔案原本這段程式碼：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

我們將它變更為這個：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>使用 MVC 協助程式的 MVC 檢視中產生 Url

針對**建立修正**按鈕在首頁上，修正其應用程式硬式編碼的錨定項目：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

如需這類的檢視/動作連結最好是使用[其中使用了 Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML 協助程式，例如：

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>而非 Thread.Sleep Task.Delay 用於背景工作角色

新專案範本將`Thread.Sleep`在此範例程式碼背景工作角色，而造成執行緒進入睡眠狀態可能會導致執行緒集區繁衍 （spawn） 額外不必要的執行緒。 您可以使用，避免[Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx)改。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>避免非同步 void

如果非同步方法不需要傳回值，傳回`Task`型別而非`void`。

這個範例取自`FixItQueueManager`類別： 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

您應該使用`async void`只針對最上層的事件處理常式。 如果您定義方法，以作為`async void`，呼叫端無法**await**方法或攔截方法擲回任何例外狀況。 如需詳細資訊，請參閱 <<c0> [ 中非同步程式設計的最佳做法](https://msdn.microsoft.com/magazine/jj991977.aspx)。 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>若要中斷背景工作角色迴圈使用的取消語彙基元

通常**執行**工作者角色上的方法包含無限迴圈。 正在停止背景工作角色，當[RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)呼叫方法。 您應該使用這個方法來取消內進行的工作**執行**方法，並結束依正常程序。 否則，程序可能會終止正在進行。

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>退出自動 MIME 探查程序

在某些情況下，Internet Explorer 會報告不同的網頁伺服器所指定之類型的 MIME 類型。 比方說，如果 Internet Explorer HTML 內容中尋找檔案，傳送 HTTP 回應標頭內容類型： text/plain Internet Explorer 決定內容應該會轉譯為 HTML。 不幸的是，此 「 MIME 探查 」 也可能導致裝載未受信任的內容伺服器的安全性問題。 若要解決這個問題，Internet Explorer 8 有對 MIME 類型判斷程式碼進行一些變更，並允許應用程式開發人員[退出 MIME 探查](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)。 下列程式碼已加入至*Web.config*檔案。

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>啟用統合和縮製

當 Visual Studio 建立新的 web 專案時，統合和縮製的 JavaScript 檔案是預設不啟用。 我們在 BundleConfig.cs 新增一行程式碼：

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>設定驗證 cookie 的到期逾時值

根據預設，驗證 cookie 會在兩週內到期。 較短的時間會更加安全。 您可以變更此設定並*StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>如何在您的本機電腦上，從 Visual Studio 執行應用程式

有兩種方式可以執行它修正應用程式：

- 執行新的工作直接寫入 SQL 資料庫的基底應用程式。
- 執行應用程式使用佇列加上後端服務建立工作。 佇列模式一章中所述[佇列為中心的工作模式](queue-centric-work-pattern.md)。

<a id="runbase"></a>
### <a name="run-the-base-application"></a>執行基本的應用程式

1. 安裝[Visual Studio 2013 或 Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads)。
2. 安裝[Azure SDK for.NET for Visual Studio 2013。](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. 下載的.zip 檔案[MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)。
4. 在 檔案總管 中，以滑鼠右鍵按一下.zip 檔案並按一下 內容，然後在 屬性 視窗中按一下 解除封鎖。
5. 將檔案解壓縮。
6. 按兩下.sln 檔案，即可啟動 Visual Studio。
7. 從 工具 功能表中，按一下 程式庫套件管理員 中，則套件管理員主控台。
8. 在 套件管理員主控台 (PMC)，按一下 還原。
9. 結束 Visual Studio。
10. 開始[Azure 儲存體模擬器](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)。
11. 重新啟動 Visual Studio，您關閉上一個步驟中開啟方案檔。
12. 請確定將 FixIt 專案設定為啟始專案，，然後按 CTRL + F5 執行專案。

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>使用佇列處理執行的應用程式

1. 請依照下列指示，說明如何[執行應用程式基底](#runbase)，然後關閉瀏覽器，然後關閉 Visual Studio。
2. 啟動 Visual Studio 系統管理員權限。 （您將使用 Azure 計算模擬器中，而且需要系統管理員權限）。
3. 應用程式中*Web.config*中的檔案*MyFixIt*專案 （web 專案） 中，變更值`appSettings/UseQueues`為"true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. 如果[Azure 儲存體模擬器](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)不是仍在執行中，重新啟動它。
5. 同時執行的 FixIt web 專案和 MyFixItCloudService 專案。

    使用 Visual Studio 2013:

   1. 按 f5 鍵執行 FixIt 專案。
   2. 在 **方案總管**MyFixItCloudService 專案中，以滑鼠右鍵按一下，然後按一下 **偵錯** -- **開始新執行個體**。

      使用 Visual Studio 2013 Express for Web 中：

   3. 在 [方案總管] 中，以滑鼠右鍵按一下 FixIt 方案，然後選取**屬性**。
   4. 選取 **多個啟始專案**...
   5. 在 **動作**MyFixIt 和 MyFixItCloudService，底下的下拉式清單中，選取**開始**。
   6. 按一下 [確定 **Deploying Office Solutions**]。
   7. 按 f5 鍵執行兩個專案。

      當您執行 MyFixItCloudService 專案時，Visual Studio 會啟動 Azure 計算模擬器。 根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>如何使用 Windows PowerShell 指令碼，將基底的應用程式部署至 Azure App Service Web Apps

為了說明[自動執行的所有項目](automate-everything.md)模式，它修正應用程式隨附在 Azure 中的環境設定，並將專案部署到新環境的指令碼。 下列指示說明如何使用指令碼。

如果您想要在 Azure 中執行，而不需使用佇列，以及您所做的變更，使用佇列在本機執行，請確定您將 UseQueues appSetting 值設回 false，下列指示進行之前，先。

這些指示假設您已經下載，並在本機執行 Fix It 解決方案，而且您有 Azure 帳戶，或是有 Azure 訂用帳戶，您有權管理。

1. 安裝**Azure PowerShell**主控台。 如需相關指示，請參閱 <<c0> [ 如何安裝和設定 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。

    這個自訂的主控台會設定為使用您 Azure 訂用帳戶。 Azure 模組會安裝在*Program Files*目錄和自動匯入每個使用 Azure PowerShell 主控台上。

    如果您想要工作在不同的主機程式中，例如 Windows PowerShell ISE 中，務必使用[Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)匯入 Azure 模組，或使用 Azure 模組中的命令，來觸發自動匯入模組的 cmdlet。
2. 啟動 Azure PowerShell**系統管理員身分執行**選項。
3. 執行[Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet 來設定 Azure PowerShell 執行原則為`RemoteSigned`。 請輸入**Y** （適用於 [是]) 來完成原則變更。

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    此設定可讓您執行本機未經過數位簽署的指令碼。 (您也可以設定執行原則`Unrestricted`，這就不需要解除封鎖步驟之後，但基於安全性理由不建議這。)
4. 執行`Add-AzureAccount`cmdlet 來設定 PowerShell，使用您的帳戶的認證。

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    這些認證會在一段時間之後過期，而且您必須重新執行`Add-AzureAccount`cmdlet。 本電子書正在寫入時，在認證到期前的時間限制為 12 小時。
5. 如果您有多個訂用帳戶，請指定您想要建立測試環境中的訂用帳戶使用 Select-azuresubscription cmdlet。
6. 使用匯入相同的 Azure 訂用帳戶的管理憑證`Get-AzurePublishSettingsFile`和`Import-AzurePublishSettingsFile`cmdlet。 這些 cmdlet 的第一個下載的憑證檔案，並在第二個您必須指定該檔案的位置以將它匯入。 > [!IMPORTANT]
   > 將下載的檔案保留在安全的位置，或刪除當您完成時，因為它包含可用來管理您的 Azure 服務的憑證。

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    會使用的憑證偵測到開發電腦的 IP 位址，以便設定 SQL Database 伺服器上的防火牆規則的 REST API 呼叫。
7. 執行[Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (別名`cd`， `chdir`，和`sl`) 來瀏覽至包含的指令碼的目錄。 (它們位於*自動化*修正它的方案資料夾中的資料夾。)如果任一目錄名稱包含空格，請將路徑放在引號中。 例如，若要瀏覽至`c:\Sample Apps\FixIt\Automation`目錄，您可以輸入下列命令：

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. 若要讓 Windows PowerShell 執行這些指令碼，使用[Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet。 （因為它們已從網際網路下載會封鎖指令碼）。

    > [!WARNING]
    > 執行前的安全性-`Unblock-File`上任何指令碼或可執行檔，在記事本中開啟檔案，檢查命令，並確認它們不包含任何惡意程式碼。

    例如，下列命令會執行`Unblock-File`cmdlet 目前目錄中的所有指令碼。

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. 若要建立基底 （未處理的佇列） 的 web 應用程式修正此問題的應用程式，執行環境建立指令碼。

    所需`Name`參數指定的資料庫名稱，並也會用於指令碼會建立儲存體帳戶。 名稱必須是全域唯一的 azurewebsites.net 網域中。 如果您指定的名稱，不是唯一的例如 Fixit 或測試 （或甚至是如同此範例中，fixitdemo），`New-AzureWebsite`指令程式會失敗並報告衝突發生內部錯誤。 指令碼會將名稱轉換成全部小寫，以符合 web 應用程式、 儲存體帳戶和資料庫的名稱需求。

    所需`SqlDatabasePassword`參數會指定將建立 SQL Database 的系統管理員帳戶的密碼。 不要在密碼中包含的特殊 XML 字元 (&amp; &lt; &gt; ;)。 這是方式的一項限制的指令碼所撰寫的 Azure 限制。

    比方說，如果您想要建立名為"fixitdemo 「 web 應用程式，並使用 「 Passw0rd1"的 SQL Server 系統管理員密碼，您可以輸入下列命令：

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    在 azurewebsites.net 網域中，名稱必須是唯一，而且密碼必須符合密碼複雜性需求的 SQL 資料庫。 （範例 Passw0rd1 沒有符合的需求）。

    請注意，命令便會開始使用 」".\"。 為了協助防止惡意指令碼執行，Windows PowerShell 會要求您提供的指令碼檔案的完整的路徑，當您執行指令碼。 您可以使用點來表示目前的目錄 (「。\")或者，提供完整的路徑，例如：

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    如需有關此指令碼的詳細資訊，請使用`Get-Help`cmdlet。

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    您可以使用`Detailed`， `Full`， `Parameters`，和`Examples`來篩選傳回的說明 Get-help cmdlet 的參數。

    如果指令碼失敗或產生錯誤，例如"New-azurewebsite:: 呼叫 Set-azuresubscription 和 Select-azuresubscription 第一個，「 您可能未完成 Azure PowerShell 的設定。

    在指令碼完成之後，您可以使用 Azure 管理入口網站，查看所建立的資源，如中所示[自動執行的所有項目](automate-everything.md)一章。
10. 若要將 FixIt 專案部署到新的 Azure 環境中，使用*AzureWebsite.ps1*指令碼。 例如: 

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    部署完成時，瀏覽器會開啟與修正它在 Azure 中執行。

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>疑難排解 Windows PowerShell 指令碼

執行下列指令碼時遇到最常見的錯誤相關的權限。 請確定`Add-AzureAccount`和`Import-AzurePublishSettingsFile`成功，而您用於相同的 Azure 訂用帳戶。 即使`Add-AzureAccount`已成功您可能必須再執行一次。 所加入的使用權限`Add-AzureAccount`在 12 小時後到期。

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>並未將物件參考設定為物件的執行個體。

如果指令碼會傳回錯誤，例如 「 物件參考未設定為物件的執行個體 」 （表示 Windows PowerShell 找不到處理程序 （這會是 null 參考例外狀況） 的物件），執行`Add-AzureAccount`cmdlet 並再試一次指令碼。

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError： 伺服器發生內部錯誤。

`New-AzureWebsite`名稱不是唯一在 azurewebsites.net 網域中時，cmdlet 會傳回發生內部錯誤。 若要解決此錯誤，請使用不同的值做為名稱，也就是在 Name 參數*新增 AzureWebsiteEnv.ps1*。

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>重新啟動指令碼

如果您需要重新啟動*新增 AzureWebsiteEnv.ps1*指令碼因為它無法它列印 「 指令碼完成 」 訊息之前，您可能想要刪除資源之前所建立的指令碼停止。 例如，如果指令碼建立 ContosoFixItDemo web 應用程式，並具有相同名稱重新執行指令碼，指令碼會失敗，因為名稱正在使用中。

若要判斷哪些資源停止之前所建立的指令碼，請使用下列 cmdlet:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`： 若要執行這個指令程式，使用管線傳送至資料庫伺服器名稱`Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

若要刪除這些資源，請使用下列命令。 請注意，如果您刪除的資料庫伺服器時，您會自動刪除與伺服器相關聯的資料庫。

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>如何使用佇列處理至 Azure App Service Web Apps 和 Azure 雲端服務部署應用程式

若要啟用佇列，請在 MyFixIt\Web.config 檔案中進行下列變更。 底下`appSettings`，變更值`UseQueues`為"true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

然後部署至 Azure App Service 的 web 應用程式的 MVC 應用程式，如所述[早](#deploybase)。

接下來，建立新的 Azure 雲端服務。 修正其應用程式隨附的指令碼不要建立或部署雲端服務，因此您必須為此使用 Azure 入口網站。 在入口網站中，按一下**的新** -- **計算**–**雲端服務** -- **快速建立**，然後輸入 URL和資料中心位置。 使用您用來部署 web 應用程式相同的資料中心。

![](the-fix-it-sample-application/_static/image1.png)

您可以部署雲端服務之前，您需要更新一些組態檔。

在 MyFixIt.WorkerRoler\app.config，底下`connectionStrings`的值取代`appdb`與實際的連接字串的 SQL database 的連接字串。 您可以從入口網站取得連接字串。 在入口網站中，按一下**SQL Database** - **appdb** - **檢視 SQL Database 連接字串，如 Ado.net、 ODBC、 PHP 和 JDBC**。 複製 ADO.NET 連接字串，並將值貼到 app.config 檔案。 取代"{您\_密碼\_此處}"與您的資料庫密碼。 (假設您使用指令碼來部署 MVC 應用程式，您指定的資料庫密碼`SqlDatabasePassword`指令碼參數。)

結果看起來應該如下所示：

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

在相同的 MyFixIt.WorkerRoler\app.config 檔案中，在`appSettings`，Azure 儲存體帳戶的兩個預留位置值取代。

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

您可以從入口網站取得的存取金鑰。 請參閱[如何管理儲存體帳戶](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)。

在 MyFixItCloudService\ServiceConfiguration.Cloud.cscfg，將 Azure 儲存體帳戶相同的兩個預留位置值。

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

現在，您已準備好要部署雲端服務。 在方案總管中以滑鼠右鍵按一下 MyFixItCloudService 專案，然後選取**發佈**。 如需詳細資訊，請參閱 「[部署至 Azure 應用程式](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"，這是在第 2 部分[本教學課程](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)。

> [!div class="step-by-step"]
> [上一步](more-patterns-and-guidance.md)
