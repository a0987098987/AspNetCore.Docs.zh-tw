---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "附錄︰ 修正它範例應用程式 （建置真實世界雲端應用程式與 Azure） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 470b8a5f4a004c85f603c9c5d0766e5826c96e38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="254c8-104">附錄︰ 修正它範例應用程式 （建置真實世界雲端應用程式與 Azure）</span><span class="sxs-lookup"><span data-stu-id="254c8-104">Appendix: The Fix It Sample Application (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="254c8-105">由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="254c8-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="254c8-106">下載該專案的修正</span><span class="sxs-lookup"><span data-stu-id="254c8-106">Download The Fix It Project</span></span>](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> <span data-ttu-id="254c8-107">**建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。</span><span class="sxs-lookup"><span data-stu-id="254c8-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="254c8-108">它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="254c8-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="254c8-109">E 書籍的相關資訊，請參閱[第一章](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="254c8-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="254c8-110">建立真實世界雲端應用程式與 Azure 的電子書本附錄包含下列各節提供有關修正它範例應用程式，您可以下載的其他資訊：</span><span class="sxs-lookup"><span data-stu-id="254c8-110">This appendix to the Building Real World Cloud Apps with Azure e-book contains the following sections that provide additional information about the Fix It sample application that you can download:</span></span>

- [<span data-ttu-id="254c8-111">已知的問題</span><span class="sxs-lookup"><span data-stu-id="254c8-111">Known issues</span></span>](#knownissues)
- [<span data-ttu-id="254c8-112">最佳作法</span><span class="sxs-lookup"><span data-stu-id="254c8-112">Best practices</span></span>](#bestpractices)
- [<span data-ttu-id="254c8-113">如何在本機電腦上從 Visual Studio 執行應用程式</span><span class="sxs-lookup"><span data-stu-id="254c8-113">How to run the app from Visual Studio on your local computer</span></span>](#run-in-vs)
- [<span data-ttu-id="254c8-114">如何使用 Windows PowerShell 指令碼，將基底應用程式部署至 Azure App Service Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="254c8-114">How to deploy the base app to Azure App Service Web Apps by using the Windows PowerShell scripts</span></span>](#deploybase)
- [<span data-ttu-id="254c8-115">疑難排解 Windows PowerShell 指令碼</span><span class="sxs-lookup"><span data-stu-id="254c8-115">Troubleshooting the Windows PowerShell scripts</span></span>](#troubleshooting)
- [<span data-ttu-id="254c8-116">如何部署應用程式與處理 Azure App Service Web 應用程式和 Azure 雲端服務的佇列</span><span class="sxs-lookup"><span data-stu-id="254c8-116">How to deploy the app with queue processing to Azure App Service Web Apps and an Azure Cloud Service</span></span>](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a><span data-ttu-id="254c8-117">已知問題</span><span class="sxs-lookup"><span data-stu-id="254c8-117">Known issues</span></span>

<span data-ttu-id="254c8-118">修正它應用程式原本是為了說明以盡可能簡單地這個 e 活頁簿中的某些模式呈現所開發。</span><span class="sxs-lookup"><span data-stu-id="254c8-118">The Fix It app was originally developed in order to illustrate as simply as possible some of the patterns presented in this e-book.</span></span> <span data-ttu-id="254c8-119">不過，由於建置真實世界應用程式的相關電子書，我們必須修正它的程式碼檢閱和測試程序類似於我們還會做為發行的軟體。</span><span class="sxs-lookup"><span data-stu-id="254c8-119">However, since the e-book is about building real-world apps, we subjected the Fix It code to a review and testing process similar to what we'd do for released software.</span></span> <span data-ttu-id="254c8-120">我們發現許多問題，並如同任何真實世界應用程式中，我們已修正其中部分且其中部分我們延後至較新版本。</span><span class="sxs-lookup"><span data-stu-id="254c8-120">We found a number of issues, and as with any real-world application, some of them we fixed and some of them we deferred to a later release.</span></span>

<span data-ttu-id="254c8-121">下列清單包含問題，應該解決實際執行應用程式中，但在其中一個原因或另一個我們決定不修正它範例應用程式的初始版本中的位址。</span><span class="sxs-lookup"><span data-stu-id="254c8-121">The following list includes issues that should be addressed in a production application, but for one reason or another we decided not to address in the initial release of the Fix It sample application.</span></span>

### <a name="security"></a><span data-ttu-id="254c8-122">安全性</span><span class="sxs-lookup"><span data-stu-id="254c8-122">Security</span></span>

- <span data-ttu-id="254c8-123">請確定您無法將工作指派給不存在的擁有者。</span><span class="sxs-lookup"><span data-stu-id="254c8-123">Ensure that you can't assign a task to a non-existent owner.</span></span>
- <span data-ttu-id="254c8-124">確定您只能檢視和修改您建立或指派給您的工作。</span><span class="sxs-lookup"><span data-stu-id="254c8-124">Ensure that you can only view and modify tasks that you created or are assigned to you.</span></span>
- <span data-ttu-id="254c8-125">使用 HTTPS 登入頁面和驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="254c8-125">Use HTTPS for sign-in pages and authentication cookies.</span></span>
- <span data-ttu-id="254c8-126">指定的驗證 cookie 的時間限制。</span><span class="sxs-lookup"><span data-stu-id="254c8-126">Specify a time limit for authentication cookies.</span></span>

### <a name="input-validation"></a><span data-ttu-id="254c8-127">輸入的驗證</span><span class="sxs-lookup"><span data-stu-id="254c8-127">Input validation</span></span>

<span data-ttu-id="254c8-128">一般情況下，生產環境應用程式會執行輸入驗證比多修正它應用程式。</span><span class="sxs-lookup"><span data-stu-id="254c8-128">In general, a production app would do more input validation than the Fix It app.</span></span> <span data-ttu-id="254c8-129">例如，影像大小 / 映像上傳應該限制允許的檔案大小。</span><span class="sxs-lookup"><span data-stu-id="254c8-129">For example, the image size / image file size allowed for upload should be limited.</span></span>

### <a name="administrator-functionality"></a><span data-ttu-id="254c8-130">系統管理員功能</span><span class="sxs-lookup"><span data-stu-id="254c8-130">Administrator functionality</span></span>

<span data-ttu-id="254c8-131">系統管理員應該能夠變更現有的工作上的擁有權。</span><span class="sxs-lookup"><span data-stu-id="254c8-131">An administrator should be able to change ownership on existing tasks.</span></span> <span data-ttu-id="254c8-132">例如，工作的建立者可能會離開公司，沒有人離開維護工作，除非已啟用系統管理存取權的授權單位。</span><span class="sxs-lookup"><span data-stu-id="254c8-132">For example, the creator of a task might leave the company, leaving no one with authority to maintain the task unless administrative access is enabled.</span></span>

### <a name="queue-message-processing"></a><span data-ttu-id="254c8-133">佇列的訊息處理</span><span class="sxs-lookup"><span data-stu-id="254c8-133">Queue message processing</span></span>

<span data-ttu-id="254c8-134">佇列中的訊息處理它修正應用程式被設計為簡化，以便說明最小量的程式碼以佇列為主的工作模式。</span><span class="sxs-lookup"><span data-stu-id="254c8-134">Queue message processing in the Fix It app was designed to be simple in order to illustrate the queue-centric work pattern with a minimum amount of code.</span></span> <span data-ttu-id="254c8-135">這個簡單的程式碼不是足夠用於實際執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="254c8-135">This simple code would not be adequate for an actual production application.</span></span>

- <span data-ttu-id="254c8-136">程式碼並不保證會處理一次的每個佇列的訊息。</span><span class="sxs-lookup"><span data-stu-id="254c8-136">The code does not guarantee that each queue message will be processed at most once.</span></span> <span data-ttu-id="254c8-137">當您從佇列取得訊息時，但沒有逾時期限，在訊息是看不到其他佇列接聽程式。</span><span class="sxs-lookup"><span data-stu-id="254c8-137">When you get a message from the queue, there is a timeout period, during which the message is invisible to other queue listeners.</span></span> <span data-ttu-id="254c8-138">如果在逾時過期之前刪除訊息，再次顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="254c8-138">If the timeout expires before the message is deleted, the message becomes visible again.</span></span> <span data-ttu-id="254c8-139">因此，如果背景工作角色執行個體花費很長的時間處理訊息，它是相同的訊息處理兩次，理論上可能導致資料庫中的重複工作。</span><span class="sxs-lookup"><span data-stu-id="254c8-139">Therefore, if a worker role instance spends a long time processing a message, it is theoretically possible for the same message to get processed twice, resulting in a duplicate task in the database.</span></span> <span data-ttu-id="254c8-140">如需有關此問題的詳細資訊，請參閱[使用 Azure 儲存體佇列](https://msdn.microsoft.com/en-us/library/ff803365.aspx#sec7)。</span><span class="sxs-lookup"><span data-stu-id="254c8-140">For more information about this issue, see [Using Azure Storage Queues](https://msdn.microsoft.com/en-us/library/ff803365.aspx#sec7).</span></span>
- <span data-ttu-id="254c8-141">藉由批次處理訊息擷取佇列輪詢邏輯可能更符合成本效益。</span><span class="sxs-lookup"><span data-stu-id="254c8-141">The queue polling logic could be more cost-effective, by batching message retrieval.</span></span> <span data-ttu-id="254c8-142">每次呼叫[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)，會有交易成本。</span><span class="sxs-lookup"><span data-stu-id="254c8-142">Every time you call [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), there is a transaction cost.</span></span> <span data-ttu-id="254c8-143">相反地，您可以呼叫[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (請注意複數形式的 ')，此 cmdlet 會取得在單一交易中的多個訊息。</span><span class="sxs-lookup"><span data-stu-id="254c8-143">Instead, you can call [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (note the plural 's'), which gets multiple messages in a single transaction.</span></span> <span data-ttu-id="254c8-144">Azure 儲存體佇列的交易成本是很低的因此不在大部分情況下大量成本的影響。</span><span class="sxs-lookup"><span data-stu-id="254c8-144">The transaction costs for Azure Storage Queues are very low, so the impact on costs is not substantial in most scenarios.</span></span>
- <span data-ttu-id="254c8-145">緊密迴圈中的佇列訊息處理程式碼會使 CPU 親和性，不會有效地使用多核心的 Vm。</span><span class="sxs-lookup"><span data-stu-id="254c8-145">The tight loop in the queue message-processing code causes CPU affinity, which does not utilize multi-core VMs efficiently.</span></span> <span data-ttu-id="254c8-146">更好的設計會使用工作平行處理原則，以平行方式執行幾項非同步工作。</span><span class="sxs-lookup"><span data-stu-id="254c8-146">A better design would use task parallelism to run several async tasks in parallel.</span></span>
- <span data-ttu-id="254c8-147">佇列的訊息處理有只初步的例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="254c8-147">Queue message-processing has only rudimentary exception handling.</span></span> <span data-ttu-id="254c8-148">例如，程式碼不會處理[有害訊息](https://msdn.microsoft.com/en-us/library/ms789028.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-148">For example, the code doesn't handle [poison messages](https://msdn.microsoft.com/en-us/library/ms789028.aspx).</span></span> <span data-ttu-id="254c8-149">（訊息處理就會導致例外狀況，您必須將錯誤記錄，並刪除郵件，或背景工作角色將會嘗試處理它，而迴圈會繼續執行無限期。）</span><span class="sxs-lookup"><span data-stu-id="254c8-149">(When message processing causes an exception, you have to log the error and delete the message, or the worker role will try to process it again, and the loop will continue indefinitely.)</span></span>

### <a name="sql-queries-are-unbounded"></a><span data-ttu-id="254c8-150">SQL 查詢都不受限制</span><span class="sxs-lookup"><span data-stu-id="254c8-150">SQL queries are unbounded</span></span>

<span data-ttu-id="254c8-151">目前修正它的程式碼會用於索引頁的查詢可能傳回的資料列數目沒有限制。</span><span class="sxs-lookup"><span data-stu-id="254c8-151">Current Fix It code places no limit on how many rows the queries for Index pages might return.</span></span> <span data-ttu-id="254c8-152">如果大量的工作輸入到資料庫時，收到的結果清單的大小可能會導致效能問題。</span><span class="sxs-lookup"><span data-stu-id="254c8-152">If a large volume of tasks is entered into the database, the size of the resulting lists received could cause performance issues.</span></span> <span data-ttu-id="254c8-153">解決方法是實作分頁。</span><span class="sxs-lookup"><span data-stu-id="254c8-153">The solution is to implement paging.</span></span> <span data-ttu-id="254c8-154">如需範例，請參閱[排序、 篩選和分頁與 ASP.NET MVC 應用程式的 Entity Framework](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="254c8-154">For an example, see [Sorting, Filtering, and Paging with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).</span></span>

### <a name="view-models-recommended"></a><span data-ttu-id="254c8-155">建議您檢視模型</span><span class="sxs-lookup"><span data-stu-id="254c8-155">View models recommended</span></span>

<span data-ttu-id="254c8-156">修正它應用程式使用 FixItTask 實體類別的控制器和檢視之間傳遞資訊。</span><span class="sxs-lookup"><span data-stu-id="254c8-156">The Fix It app uses the FixItTask entity class to pass information between the controller and the view.</span></span> <span data-ttu-id="254c8-157">最佳作法是使用檢視模型。</span><span class="sxs-lookup"><span data-stu-id="254c8-157">A best practice is to use view models.</span></span> <span data-ttu-id="254c8-158">網域模型 （例如 FixItTask 實體類別） 是針對設計所需的資料持續性，而檢視模型可以針對資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="254c8-158">The domain model (e.g., the FixItTask entity class) is designed around what is needed for data persistence, while a view model can be designed for data presentation.</span></span> <span data-ttu-id="254c8-159">如需詳細資訊，請參閱[12 ASP.NET MVC 最佳作法](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-159">For more information, see [12 ASP.NET MVC Best Practices](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).</span></span>

### <a name="secure-image-blob-recommended"></a><span data-ttu-id="254c8-160">建議使用的安全映像 blob</span><span class="sxs-lookup"><span data-stu-id="254c8-160">Secure image blob recommended</span></span>

<span data-ttu-id="254c8-161">修正它應用程式市集上傳映像為公用，這表示 URL 會尋找任何人可以存取的映像。</span><span class="sxs-lookup"><span data-stu-id="254c8-161">The Fix It app stores uploaded images as public, meaning that anyone who finds the URL can access the images.</span></span> <span data-ttu-id="254c8-162">無法保護映像，而不是公用。</span><span class="sxs-lookup"><span data-stu-id="254c8-162">The images could be secured instead of public.</span></span>

### <a name="no-powershell-automation-scripts-for-queues"></a><span data-ttu-id="254c8-163">佇列沒有 PowerShell 自動化指令碼</span><span class="sxs-lookup"><span data-stu-id="254c8-163">No PowerShell automation scripts for queues</span></span>

<span data-ttu-id="254c8-164">只有修正它完全在 Azure App Service Web 應用程式中執行的基本版本所撰寫的範例 PowerShell 自動化指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-164">Sample PowerShell automation scripts were written only for the base version of Fix It that runs entirely in Azure App Service Web Apps.</span></span> <span data-ttu-id="254c8-165">我們尚未提供指令碼以設定和部署 web 應用程式，以及所需的佇列處理的雲端服務環境。</span><span class="sxs-lookup"><span data-stu-id="254c8-165">We haven't provided scripts for setting up and deploying to the web app plus Cloud Service environment required for queue processing.</span></span>

### <a name="special-handling-for-html-codes-in-user-input"></a><span data-ttu-id="254c8-166">使用者輸入中的 HTML 程式碼的特殊處理</span><span class="sxs-lookup"><span data-stu-id="254c8-166">Special handling for HTML codes in user input</span></span>

<span data-ttu-id="254c8-167">ASP.NET 會自動防止許多方式在其中惡意使用者可能會在使用者輸入的文字方塊中輸入指令碼嘗試跨網站指令碼的攻擊。</span><span class="sxs-lookup"><span data-stu-id="254c8-167">ASP.NET automatically prevents many ways in which malicious users might attempt cross-site scripting attacks by entering script in user input text boxes.</span></span> <span data-ttu-id="254c8-168">和 MVC`DisplayFor`協助程式用來顯示工作項目和資訊會自動針對它傳送至瀏覽器進行 HTML 編碼的值。</span><span class="sxs-lookup"><span data-stu-id="254c8-168">And the MVC `DisplayFor` helper used to display task titles and notes automatically HTML-encodes values that it sends to the browser.</span></span> <span data-ttu-id="254c8-169">但在生產環境應用程式可能會想要採取其他措施。</span><span class="sxs-lookup"><span data-stu-id="254c8-169">But in a production app you might want to take additional measures.</span></span> <span data-ttu-id="254c8-170">如需詳細資訊，請參閱[中 ASP.NET 的要求驗證](https://msdn.microsoft.com/en-us/library/hh882339.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-170">For more information, see [Request Validation in ASP.NET](https://msdn.microsoft.com/en-us/library/hh882339.aspx).</span></span>

<a id="bestpractices"></a>
## <a name="best-practices"></a><span data-ttu-id="254c8-171">最佳作法</span><span class="sxs-lookup"><span data-stu-id="254c8-171">Best practices</span></span>

<span data-ttu-id="254c8-172">以下是在程式碼檢閱中發現及測試它修正應用程式的原始版本已修正後一些問題。</span><span class="sxs-lookup"><span data-stu-id="254c8-172">Following are some issues that were fixed after being discovered in code review and testing of the original version of the Fix It app.</span></span> <span data-ttu-id="254c8-173">部分所引起原始的編碼器不留意特定的最佳作法，一些只因為此程式碼以快速地撰寫並不適合發行的軟體。</span><span class="sxs-lookup"><span data-stu-id="254c8-173">Some were caused by the original coder not being aware of a particular best practice, some simply because the code was written quickly and wasn't intended for released software.</span></span> <span data-ttu-id="254c8-174">我們正在列出的問題，以防萬一發生我們所了解從這個檢閱的項目和測試，可能會很有幫助其他人也開發 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="254c8-174">We're listing the issues here in case there's something we learned from this review and testing that might be helpful to others who are also developing web apps.</span></span>

### <a name="dispose-the-database-repository"></a><span data-ttu-id="254c8-175">處置資料庫儲存機制</span><span class="sxs-lookup"><span data-stu-id="254c8-175">Dispose the database repository</span></span>

<span data-ttu-id="254c8-176">`FixItTaskRepository`類別必須處置 Entity Framework`DbContext`執行個體。</span><span class="sxs-lookup"><span data-stu-id="254c8-176">The `FixItTaskRepository` class must dispose the Entity Framework `DbContext` instance.</span></span> <span data-ttu-id="254c8-177">我們採用的方法來實作`IDisposable`中`FixItTaskRepository`類別：</span><span class="sxs-lookup"><span data-stu-id="254c8-177">We did this by implementing `IDisposable` in the `FixItTaskRepository` class:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

<span data-ttu-id="254c8-178">請注意，自動將處置 AutoFac`FixItTaskRepository`執行個體，因此我們不需要明確處置它。</span><span class="sxs-lookup"><span data-stu-id="254c8-178">Note that AutoFac will automatically dispose the `FixItTaskRepository` instance, so we don't need to explicitly dispose it.</span></span>

<span data-ttu-id="254c8-179">另一個選項是移除`DbContext`成員變數，從`FixItTaskRepository`，而是建立本機`DbContext`變數在每個儲存機制方法中，內部`using`陳述式。</span><span class="sxs-lookup"><span data-stu-id="254c8-179">Another option is to remove the `DbContext` member variable from `FixItTaskRepository`, and instead create a local `DbContext` variable within each repository method, inside a `using` statement.</span></span> <span data-ttu-id="254c8-180">例如: </span><span class="sxs-lookup"><span data-stu-id="254c8-180">For example:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a><span data-ttu-id="254c8-181">在這種情況將 singleton 註冊 DI 與</span><span class="sxs-lookup"><span data-stu-id="254c8-181">Register singletons as such with DI</span></span>

<span data-ttu-id="254c8-182">因為只有一個執行個體`PhotoService`類別和`Logger`需要類別，這些類別應[註冊相依性插入的單一執行個體為](https://code.google.com/p/autofac/wiki/InstanceScope)中*DependenciesConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="254c8-182">Since only one instance of the `PhotoService` class and `Logger` class is needed, these classes should be [registered as single instances for dependency injection](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a><span data-ttu-id="254c8-183">安全性： 不向使用者顯示錯誤詳細資料</span><span class="sxs-lookup"><span data-stu-id="254c8-183">Security: Don't show error details to users</span></span>

<span data-ttu-id="254c8-184">原始修正它的應用程式未有一般錯誤頁面，並就讓所有的例外狀況泡泡，最多 UI，讓某些例外狀況，例如資料庫連線錯誤可能會導致瀏覽器顯示完整的堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="254c8-184">The original Fix It app didn't have a generic error page and just let all exceptions bubble up to the UI, so some exceptions such as database connection errors could result in a full stack trace being displayed to the browser.</span></span> <span data-ttu-id="254c8-185">詳細的錯誤資訊，有時也有助於惡意使用者攻擊。</span><span class="sxs-lookup"><span data-stu-id="254c8-185">Detailed error information can sometimes facilitate attacks by malicious users.</span></span> <span data-ttu-id="254c8-186">方案是記錄例外狀況詳細資料，並顯示錯誤頁面，使用者不會包含錯誤詳細資料。</span><span class="sxs-lookup"><span data-stu-id="254c8-186">The solution is to log the exception details and display an error page to the user that doesn't include error details.</span></span> <span data-ttu-id="254c8-187">修正它應用程式已記錄，以及為了顯示錯誤頁面，我們加入了`<customErrors mode=On>`Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="254c8-187">The Fix It app was already logging, and in order to display an error page, we added `<customErrors mode=On>` in the Web.config file.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

<span data-ttu-id="254c8-188">依預設這會導致*Views\Shared\Error.cshtml*来顯示的錯誤。</span><span class="sxs-lookup"><span data-stu-id="254c8-188">By default this causes *Views\Shared\Error.cshtml* to be displayed for errors.</span></span> <span data-ttu-id="254c8-189">您可以自訂*Error.cshtml*或建立您自己的錯誤頁面檢視並新增`defaultRedirect`屬性。</span><span class="sxs-lookup"><span data-stu-id="254c8-189">You can customize *Error.cshtml* or create your own error page view and add a `defaultRedirect` attribute.</span></span> <span data-ttu-id="254c8-190">您也可以指定不同的錯誤頁面，針對特定錯誤。</span><span class="sxs-lookup"><span data-stu-id="254c8-190">You can also specify different error pages for specific errors.</span></span>

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a><span data-ttu-id="254c8-191">安全性： 只允許由其建立者編輯工作</span><span class="sxs-lookup"><span data-stu-id="254c8-191">Security: only allow a task to be edited by its creator</span></span>

<span data-ttu-id="254c8-192">儀表板索引頁面只會顯示登入的使用者所建立的工作，但惡意使用者可以建立 URL 給其他使用者的工作識別碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-192">The Dashboard Index page only shows tasks created by the logged-on user, but a malicious user could create a URL with an ID to another user's task.</span></span> <span data-ttu-id="254c8-193">我們加入程式碼中的*DashboardController.cs*在此情況下傳回 404:</span><span class="sxs-lookup"><span data-stu-id="254c8-193">We added code in *DashboardController.cs* to return a 404 in that case:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a><span data-ttu-id="254c8-194">不要忍受例外狀況</span><span class="sxs-lookup"><span data-stu-id="254c8-194">Don't swallow exceptions</span></span>

<span data-ttu-id="254c8-195">原始修正它的應用程式只傳回了 null 登入的 SQL 查詢所造成的例外狀況之後：</span><span class="sxs-lookup"><span data-stu-id="254c8-195">The original Fix It app just returned null after logging an exception that resulted from a SQL query:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

<span data-ttu-id="254c8-196">這會使它看起來向使用者查詢成功，但只要未傳回任何資料列。</span><span class="sxs-lookup"><span data-stu-id="254c8-196">This would make it look to the user as if the query succeeded but just didn't return any rows.</span></span> <span data-ttu-id="254c8-197">解決方案是重新擲回的例外狀況之後攔截及記錄：</span><span class="sxs-lookup"><span data-stu-id="254c8-197">Solution is to re-throw the exception after catching and logging:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a><span data-ttu-id="254c8-198">在背景工作角色中攔截所有例外狀況</span><span class="sxs-lookup"><span data-stu-id="254c8-198">Catch all exceptions in worker roles</span></span>

<span data-ttu-id="254c8-199">任何未處理的例外狀況，以背景工作角色將導致回收 VM，所以您想要將所有項目包裝在 try catch 區塊中執行並處理所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="254c8-199">Any unhandled exceptions in a worker role will cause the VM to be recycled, so you want to wrap everything you do in a try-catch block and handle all exceptions.</span></span>

### <a name="specify-length-for-string-properties-in-entity-classes"></a><span data-ttu-id="254c8-200">在實體類別中指定的字串屬性的長度</span><span class="sxs-lookup"><span data-stu-id="254c8-200">Specify length for string properties in entity classes</span></span>

<span data-ttu-id="254c8-201">為了顯示簡單的程式碼，修正它應用程式的原始版本未指定，在 FixItTask 實體的欄位長度，因此它們被定義為 varchar （max） 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="254c8-201">In order to display simple code, the original version of the Fix It app didn't specify lengths for the fields of the FixItTask entity, and as a result they were defined as varchar(max) in the database.</span></span> <span data-ttu-id="254c8-202">因此，UI 會接受幾乎任何數量的輸入。</span><span class="sxs-lookup"><span data-stu-id="254c8-202">As a result, the UI would accept almost any amount of input.</span></span> <span data-ttu-id="254c8-203">指定的長度設定限制，同時套用至使用者輸入中的網頁和資料庫中的資料行大小：</span><span class="sxs-lookup"><span data-stu-id="254c8-203">Specifying lengths sets limits that apply both to user input in the web page and column size in the database:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a><span data-ttu-id="254c8-204">將私用成員標記為 readonly，當它們不應該變更</span><span class="sxs-lookup"><span data-stu-id="254c8-204">Mark private members as readonly when they aren't expected to change</span></span>

<span data-ttu-id="254c8-205">例如，在`DashboardController`類別的執行個體`FixItTaskRepository`建立，且不應該變更，因此我們定義成[readonly](https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-205">For example, in the `DashboardController` class an instance of `FixItTaskRepository` is created and isn't expected to change, so we defined it as [readonly](https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx).</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a><span data-ttu-id="254c8-206">使用清單。Any （) 而不是清單。Count （) &gt; 0</span><span class="sxs-lookup"><span data-stu-id="254c8-206">Use list.Any() instead of list.Count() &gt; 0</span></span>

<span data-ttu-id="254c8-207">如果所有的您很重視您的清單中的一個或多個項目是否符合指定的準則，請使用[任何](https://msdn.microsoft.com/en-us/library/bb534972.aspx)方法，它就會傳回找到符合準則的項目，而因為`Count`方法一律必須逐一查看透過每個項目。</span><span class="sxs-lookup"><span data-stu-id="254c8-207">If you all you care about is whether one or more items in a list fit the specified criteria, use the [Any](https://msdn.microsoft.com/en-us/library/bb534972.aspx) method, because it returns as soon as an item fitting the criteria is found, whereas the `Count` method always has to iterate through every item.</span></span> <span data-ttu-id="254c8-208">儀表板*Index.cshtml*檔案最初的這段程式碼：</span><span class="sxs-lookup"><span data-stu-id="254c8-208">The Dashboard *Index.cshtml* file originally had this code:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

<span data-ttu-id="254c8-209">我們變更此：</span><span class="sxs-lookup"><span data-stu-id="254c8-209">We changed it to this:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a><span data-ttu-id="254c8-210">產生 Url 中使用 MVC helper 的 MVC 檢視</span><span class="sxs-lookup"><span data-stu-id="254c8-210">Generate URLs in MVC views using MVC helpers</span></span>

<span data-ttu-id="254c8-211">如**建立修正**按鈕在首頁上，修正它應用程式硬式編碼的錨定項目：</span><span class="sxs-lookup"><span data-stu-id="254c8-211">For the **Create a Fix It** button on the home page, the Fix It app hard coded an anchor element:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

<span data-ttu-id="254c8-212">檢視/動作連結，就像這樣的最好使用[Url.Action](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.action.aspx) HTML helper，例如：</span><span class="sxs-lookup"><span data-stu-id="254c8-212">For View/Action links like this it's better to use the [Url.Action](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.action.aspx) HTML helper, for example:</span></span>

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a><span data-ttu-id="254c8-213">背景工作角色，而非 Thread.Sleep 使用 Task.Delay</span><span class="sxs-lookup"><span data-stu-id="254c8-213">Use Task.Delay instead of Thread.Sleep in worker role</span></span>

<span data-ttu-id="254c8-214">將新專案範本`Thread.Sleep`在範例程式碼背景工作角色，而造成執行緒睡眠可能會導致執行緒集區繁衍 （spawn） 其他不必要的執行緒。</span><span class="sxs-lookup"><span data-stu-id="254c8-214">The new-project template puts `Thread.Sleep` in the sample code for a worker role, but causing the thread to sleep can cause the thread pool to spawn additional unnecessary threads.</span></span> <span data-ttu-id="254c8-215">您可以藉由避免[Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096.aspx)改為。</span><span class="sxs-lookup"><span data-stu-id="254c8-215">You can avoid that by using [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096.aspx) instead.</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a><span data-ttu-id="254c8-216">避免 async void</span><span class="sxs-lookup"><span data-stu-id="254c8-216">Avoid async void</span></span>

<span data-ttu-id="254c8-217">如果非同步方法不需要傳回值，傳回`Task`型別而非`void`。</span><span class="sxs-lookup"><span data-stu-id="254c8-217">If an async method doesn't need to return a value, return a `Task` type rather than `void`.</span></span>

<span data-ttu-id="254c8-218">這個範例取自`FixItQueueManager`類別：</span><span class="sxs-lookup"><span data-stu-id="254c8-218">This example is from the `FixItQueueManager` class:</span></span> 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

<span data-ttu-id="254c8-219">您應該使用`async void`僅適用於最上層的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="254c8-219">You should use `async void` only for top-level event handlers.</span></span> <span data-ttu-id="254c8-220">如果您定義將方法當做`async void`，呼叫端無法**await**方法或攔截方法擲回任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="254c8-220">If you define a method as `async void`, the caller cannot **await** the method or catch any exceptions the method throws.</span></span> <span data-ttu-id="254c8-221">如需詳細資訊，請參閱[中非同步程式設計的最佳作法](https://msdn.microsoft.com/en-us/magazine/jj991977.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-221">For more information, see [Best Practices in Asynchronous Programming](https://msdn.microsoft.com/en-us/magazine/jj991977.aspx).</span></span> 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a><span data-ttu-id="254c8-222">中斷從背景工作角色迴圈中使用取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="254c8-222">Use a cancellation token to break from worker role loop</span></span>

<span data-ttu-id="254c8-223">一般而言，**執行**工作者角色上的方法包含無限迴圈。</span><span class="sxs-lookup"><span data-stu-id="254c8-223">Typically, the **Run** method on a worker role contains an infinite loop.</span></span> <span data-ttu-id="254c8-224">背景工作角色停止時，請[Application_end](https://msdn.microsoft.com/en-us/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="254c8-224">When the worker role is stopping, the [RoleEntryPoint.OnStop](https://msdn.microsoft.com/en-us/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) method is called.</span></span> <span data-ttu-id="254c8-225">您應該使用這個方法來取消工作內所進行**執行**方法並結束依正常程序。</span><span class="sxs-lookup"><span data-stu-id="254c8-225">You should use this method to cancel the work that is being done inside the **Run** method and exit gracefully.</span></span> <span data-ttu-id="254c8-226">否則，處理序可能會終止作業當中。</span><span class="sxs-lookup"><span data-stu-id="254c8-226">Otherwise, the process might be terminated in the middle of an operation.</span></span>

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a><span data-ttu-id="254c8-227">選擇退出自動 MIME 探查程序</span><span class="sxs-lookup"><span data-stu-id="254c8-227">Opt out of Automatic MIME Sniffing Procedure</span></span>

<span data-ttu-id="254c8-228">在某些情況下，Internet Explorer 會報告不同的網頁伺服器所指定之類型的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="254c8-228">In some cases, Internet Explorer reports a MIME type different than the type specified by the web server.</span></span> <span data-ttu-id="254c8-229">比方說，如果 Internet Explorer HTML 內容中找到檔案，傳送 HTTP 回應標頭內容類型： text/plain Internet Explorer 會決定內容應該會轉譯為 HTML。</span><span class="sxs-lookup"><span data-stu-id="254c8-229">For instance, if Internet Explorer finds HTML content in a file delivered with the HTTP response header Content-Type: text/plain, Internet Explorer determines that the content should be rendered as HTML.</span></span> <span data-ttu-id="254c8-230">不幸的是，此 「 MIME 探查 」 也會導致裝載未受信任的內容伺服器的安全性問題。</span><span class="sxs-lookup"><span data-stu-id="254c8-230">Unfortunately, this "MIME-sniffing" can also lead to security problems for servers hosting untrusted content.</span></span> <span data-ttu-id="254c8-231">為了打擊這個問題，Internet Explorer 8 MIME 類型判斷程式碼所做的變更數目和可讓應用程式開發人員[退出 MIME 探查](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-231">To combat this problem, Internet Explorer 8 has made a number of changes to MIME-type determination code and allows application developers to [opt out of MIME-sniffing](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx).</span></span> <span data-ttu-id="254c8-232">下列程式碼已加入*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="254c8-232">The following code was added to the *Web.config* file.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a><span data-ttu-id="254c8-233">啟用統合及縮製</span><span class="sxs-lookup"><span data-stu-id="254c8-233">Enable bundling and minification</span></span>

<span data-ttu-id="254c8-234">當 Visual Studio 建立新的 web 專案時，統合及縮製的 JavaScript 檔是預設不啟用。</span><span class="sxs-lookup"><span data-stu-id="254c8-234">When Visual Studio creates a new web project, bundling and minification of JavaScript files is not enabled by default.</span></span> <span data-ttu-id="254c8-235">我們在 BundleConfig.cs 加入一行程式碼：</span><span class="sxs-lookup"><span data-stu-id="254c8-235">We added a line of code in BundleConfig.cs:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a><span data-ttu-id="254c8-236">設定的驗證 cookie 的到期逾時</span><span class="sxs-lookup"><span data-stu-id="254c8-236">Set an expiration time-out for authentication cookies</span></span>

<span data-ttu-id="254c8-237">根據預設，驗證 cookie 會在兩週內到期。</span><span class="sxs-lookup"><span data-stu-id="254c8-237">By default, authentication cookies expire in two weeks.</span></span> <span data-ttu-id="254c8-238">較短的時間會比較安全。</span><span class="sxs-lookup"><span data-stu-id="254c8-238">A shorter time is more secure.</span></span> <span data-ttu-id="254c8-239">您可以變更此設定在*StartupAuth.cs*:</span><span class="sxs-lookup"><span data-stu-id="254c8-239">You can change this setting in *StartupAuth.cs*:</span></span>

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a><span data-ttu-id="254c8-240">如何在本機電腦上從 Visual Studio 執行應用程式</span><span class="sxs-lookup"><span data-stu-id="254c8-240">How to run the app from Visual Studio on your local computer</span></span>

<span data-ttu-id="254c8-241">有兩種方式可以執行它修正應用程式：</span><span class="sxs-lookup"><span data-stu-id="254c8-241">There are two ways to run the Fix It app:</span></span>

- <span data-ttu-id="254c8-242">執行新工作將直接寫入至 SQL 資料庫的基底應用程式。</span><span class="sxs-lookup"><span data-stu-id="254c8-242">Run the base application that writes new tasks directly to the SQL database.</span></span>
- <span data-ttu-id="254c8-243">執行應用程式使用佇列加上後端服務建立的工作。</span><span class="sxs-lookup"><span data-stu-id="254c8-243">Run the application using a queue plus a backend service to create tasks.</span></span> <span data-ttu-id="254c8-244">本章說明佇列模式[佇列為主的工作模式](queue-centric-work-pattern.md)。</span><span class="sxs-lookup"><span data-stu-id="254c8-244">The queue pattern is described in the chapter [Queue-Centric Work Pattern](queue-centric-work-pattern.md).</span></span>

<a id="runbase"></a>
### <a name="run-the-base-application"></a><span data-ttu-id="254c8-245">執行基本的應用程式</span><span class="sxs-lookup"><span data-stu-id="254c8-245">Run the base application</span></span>

1. <span data-ttu-id="254c8-246">安裝[Visual Studio 2013 或 Visual Studio 2013 Express for Web](https://www.visualstudio.com/en-us/downloads)。</span><span class="sxs-lookup"><span data-stu-id="254c8-246">Install [Visual Studio 2013 or Visual Studio 2013 Express for Web](https://www.visualstudio.com/en-us/downloads).</span></span>
2. <span data-ttu-id="254c8-247">安裝[Azure SDK for.NET for Visual Studio 2013。](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="254c8-247">Install the [Azure SDK for .NET for Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)</span></span>
3. <span data-ttu-id="254c8-248">下載.zip 檔案從[MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)。</span><span class="sxs-lookup"><span data-stu-id="254c8-248">Download the .zip file from the [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).</span></span>
4. <span data-ttu-id="254c8-249">在檔案總管] 中，以滑鼠右鍵按一下.zip 檔案並按一下 [內容]，然後在 [屬性] 視窗中按一下 [解除封鎖。</span><span class="sxs-lookup"><span data-stu-id="254c8-249">In File Explorer, right-click the .zip file and click Properties, then in the Properties window click Unblock.</span></span>
5. <span data-ttu-id="254c8-250">解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="254c8-250">Unzip the file.</span></span>
6. <span data-ttu-id="254c8-251">按兩下.sln 檔案，即可啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="254c8-251">Double-click the .sln file to launch Visual Studio.</span></span>
7. <span data-ttu-id="254c8-252">從 [工具] 功能表中，按一下 [程式庫封裝管理員，則封裝管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="254c8-252">From the Tools menu, click Library Package Manager, then Package Manager Console.</span></span>
8. <span data-ttu-id="254c8-253">在 封裝管理員主控台 (PMC)，按一下 還原。</span><span class="sxs-lookup"><span data-stu-id="254c8-253">In the Package Manager Console (PMC), click Restore.</span></span>
9. <span data-ttu-id="254c8-254">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="254c8-254">Exit Visual Studio.</span></span>
10. <span data-ttu-id="254c8-255">啟動[Azure 儲存體模擬器](https://msdn.microsoft.com/en-us/library/windowsazure/hh403989.aspx)。</span><span class="sxs-lookup"><span data-stu-id="254c8-255">Start the [Azure storage emulator](https://msdn.microsoft.com/en-us/library/windowsazure/hh403989.aspx).</span></span>
11. <span data-ttu-id="254c8-256">重新啟動 Visual Studio 中，開啟上一個步驟中您關閉方案檔案。</span><span class="sxs-lookup"><span data-stu-id="254c8-256">Restart Visual Studio, opening the solution file you closed in the previous step.</span></span>
12. <span data-ttu-id="254c8-257">請確定 FixIt 專案已設定為啟始專案，，然後按 CTRL + F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="254c8-257">Make sure the FixIt project is set as the startup project, and then press CTRL+F5 to run the project.</span></span>

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a><span data-ttu-id="254c8-258">執行應用程式佇列的處理</span><span class="sxs-lookup"><span data-stu-id="254c8-258">Run the application with queue processing</span></span>

1. <span data-ttu-id="254c8-259">依照指示[執行基底應用程式](#runbase)，然後關閉瀏覽器，然後關閉 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="254c8-259">Follow the directions for [Run the base application](#runbase), and then close the browser and close Visual Studio.</span></span>
2. <span data-ttu-id="254c8-260">啟動 Visual Studio 系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="254c8-260">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="254c8-261">（您將使用 Azure 計算模擬器中，而且這需要系統管理員權限。）</span><span class="sxs-lookup"><span data-stu-id="254c8-261">(You'll be using the Azure compute emulator, and that requires administrator privileges.)</span></span>
3. <span data-ttu-id="254c8-262">應用程式中*Web.config*檔案*MyFixIt*專案 （web 專案） 中，變更的值`appSettings/UseQueues`為"true":</span><span class="sxs-lookup"><span data-stu-id="254c8-262">In the application *Web.config* file in the *MyFixIt* project (the web project), change the value of `appSettings/UseQueues` to "true":</span></span> 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. <span data-ttu-id="254c8-263">如果[Azure 儲存體模擬器](https://msdn.microsoft.com/en-us/library/windowsazure/hh403989.aspx)不是仍在執行中，再次啟動它。</span><span class="sxs-lookup"><span data-stu-id="254c8-263">If the [Azure storage emulator](https://msdn.microsoft.com/en-us/library/windowsazure/hh403989.aspx) isn't still running, start it again.</span></span>
5. <span data-ttu-id="254c8-264">同時執行 FixIt web 專案與 MyFixItCloudService 專案。</span><span class="sxs-lookup"><span data-stu-id="254c8-264">Run the FixIt web project and the MyFixItCloudService project simultaneously.</span></span>

    <span data-ttu-id="254c8-265">使用 Visual Studio 2013:</span><span class="sxs-lookup"><span data-stu-id="254c8-265">Using Visual Studio 2013:</span></span>

    1. <span data-ttu-id="254c8-266">按 F5 執行 FixIt 專案。</span><span class="sxs-lookup"><span data-stu-id="254c8-266">Press F5 to run the FixIt project.</span></span>
    2. <span data-ttu-id="254c8-267">在**方案總管 中**MyFixItCloudService 專案中，按一下滑鼠右鍵，然後按**偵錯** -- **開始新執行個體**。</span><span class="sxs-lookup"><span data-stu-id="254c8-267">In **Solution Explorer**, right-click the MyFixItCloudService project, and then click **Debug** -- **Start New Instance**.</span></span>

    <span data-ttu-id="254c8-268">使用 Visual Studio 2013 Express for Web:</span><span class="sxs-lookup"><span data-stu-id="254c8-268">Using Visual Studio 2013 Express for Web:</span></span>

    1. <span data-ttu-id="254c8-269">在 方案總管 FixIt 方案上按一下滑鼠右鍵，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="254c8-269">In Solution Explorer, right-click the FixIt solution and select **Properties**.</span></span>
    2. <span data-ttu-id="254c8-270">選取**多個啟始專案**...</span><span class="sxs-lookup"><span data-stu-id="254c8-270">Select **Multiple Startup Projects**..</span></span>
    3. <span data-ttu-id="254c8-271">在**動作**MyFixIt 和 MyFixItCloudService，底下的下拉式清單選取**啟動**。</span><span class="sxs-lookup"><span data-stu-id="254c8-271">In the **Action** dropdown list under MyFixIt and MyFixItCloudService, select **Start**.</span></span>
    4. <span data-ttu-id="254c8-272">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="254c8-272">Click **OK**.</span></span>
    5. <span data-ttu-id="254c8-273">按 F5 執行這兩個專案。</span><span class="sxs-lookup"><span data-stu-id="254c8-273">Press F5 to run both projects.</span></span>

    <span data-ttu-id="254c8-274">當您執行 MyFixItCloudService 專案時，Visual Studio 會啟動 Azure 計算模擬器。</span><span class="sxs-lookup"><span data-stu-id="254c8-274">When you run the MyFixItCloudService project, Visual Studio starts the Azure compute emulator.</span></span> <span data-ttu-id="254c8-275">根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。</span><span class="sxs-lookup"><span data-stu-id="254c8-275">Depending on your firewall configuration, you might need to allow the emulator through the firewall.</span></span>

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a><span data-ttu-id="254c8-276">如何使用 Windows PowerShell 指令碼，將基底應用程式部署至 Azure App Service Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="254c8-276">How to deploy the base app to Azure App Service Web Apps by using the Windows PowerShell scripts</span></span>

<span data-ttu-id="254c8-277">為了說明[一切自動化](automate-everything.md)模式中，修正它應用程式提供給指令碼會設定在 Azure 中的環境，然後將專案部署到新環境。</span><span class="sxs-lookup"><span data-stu-id="254c8-277">To illustrate the [Automate Everything](automate-everything.md) pattern, the Fix It app is supplied with scripts that set up an environment in Azure and deploy the project to the new environment.</span></span> <span data-ttu-id="254c8-278">下列指示說明如何使用指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-278">The following instructions explain how to use the scripts.</span></span>

<span data-ttu-id="254c8-279">如果您想要在 Azure 中執行，而不使用佇列，並進行變更，以執行本機佇列，請確定您回到 false 後再繼續使用下列指示設定 UseQueues appsetting owin： 值。</span><span class="sxs-lookup"><span data-stu-id="254c8-279">If you want to run in Azure without using queues, and you made the changes to run locally with queues, make sure you set the UseQueues appSetting value back to false before proceeding with the following instructions.</span></span>

<span data-ttu-id="254c8-280">這些指示假設您已經下載並執行修正它方案在本機，而且您有 Azure 帳戶或具有 Azure 訂用帳戶已獲授權管理。</span><span class="sxs-lookup"><span data-stu-id="254c8-280">These instructions assume you have already downloaded and run the Fix It solution locally, and that you have an Azure account or have an Azure subscription that you are authorized to manage.</span></span>

1. <span data-ttu-id="254c8-281">安裝**Azure PowerShell**主控台。</span><span class="sxs-lookup"><span data-stu-id="254c8-281">Install the **Azure PowerShell** console.</span></span> <span data-ttu-id="254c8-282">如需指示，請參閱[如何安裝及設定 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。</span><span class="sxs-lookup"><span data-stu-id="254c8-282">For instructions, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span>

    <span data-ttu-id="254c8-283">這個自訂的主控台設定為使用您的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="254c8-283">This customized console is configured to work with your Azure subscription.</span></span> <span data-ttu-id="254c8-284">Azure 模組安裝在*Program Files*目錄以及自動匯入每次使用 Azure PowerShell 主控台。</span><span class="sxs-lookup"><span data-stu-id="254c8-284">The Azure module is installed in the *Program Files* directory and is automatically imported on every use of the Azure PowerShell console.</span></span>

    <span data-ttu-id="254c8-285">如果您想要使用不同的主機程式，例如 Windows PowerShell ISE，請務必使用[Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)匯入 Azure 模組，或使用 Azure 模組中的命令，來觸發自動匯入模組的 cmdlet。</span><span class="sxs-lookup"><span data-stu-id="254c8-285">If you prefer to work in a different host program, such as Windows PowerShell ISE, be sure to use the [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet to import the Azure module or use a command in the Azure module to trigger automatic importing of the module.</span></span>
2. <span data-ttu-id="254c8-286">啟動 Azure PowerShell**系統管理員身分執行**選項。</span><span class="sxs-lookup"><span data-stu-id="254c8-286">Start Azure PowerShell with the **Run as administrator** option.</span></span>
3. <span data-ttu-id="254c8-287">執行[Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet 來設定 Azure PowerShell 執行原則為`RemoteSigned`。</span><span class="sxs-lookup"><span data-stu-id="254c8-287">Run the [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet to set the Azure PowerShell execution policy to `RemoteSigned`.</span></span> <span data-ttu-id="254c8-288">輸入**Y** （適用於 [是]) 來完成原則變更。</span><span class="sxs-lookup"><span data-stu-id="254c8-288">Enter **Y** (for Yes) to complete the policy change.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    <span data-ttu-id="254c8-289">此設定可讓您執行不經過數位簽署的本機指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-289">This setting enables you to run local scripts that aren't digitally signed.</span></span> <span data-ttu-id="254c8-290">(您也可以將執行原則設`Unrestricted`，這就不需要解除封鎖步驟更新版本中，但基於安全性理由不建議這樣。)</span><span class="sxs-lookup"><span data-stu-id="254c8-290">(You can also set the execution policy to `Unrestricted`, which would eliminate the need for the unblock step later, but this is not recommended for security reasons.)</span></span>
4. <span data-ttu-id="254c8-291">執行`Add-AzureAccount`使用您的帳戶認證設定 PowerShell cmdlet。</span><span class="sxs-lookup"><span data-stu-id="254c8-291">Run the `Add-AzureAccount` cmdlet to set up PowerShell with credentials for your account.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    <span data-ttu-id="254c8-292">這些認證過期一段時間之後，您必須重新執行`Add-AzureAccount`cmdlet。</span><span class="sxs-lookup"><span data-stu-id="254c8-292">These credentials expire after a period of time and you have to re-run the `Add-AzureAccount` cmdlet.</span></span> <span data-ttu-id="254c8-293">正在寫入此的電子書，則在認證到期前的時間限制為 12 小時。</span><span class="sxs-lookup"><span data-stu-id="254c8-293">As this e-book is being written, the time limit before credentials expire is 12 hours.</span></span>
5. <span data-ttu-id="254c8-294">如果您有多個訂閱，可用於選取 AzureSubscription cmdlet 指定您想要建立測試環境中的訂用的帳戶。</span><span class="sxs-lookup"><span data-stu-id="254c8-294">If you have multiple subscriptions, use the Select-AzureSubscription cmdlet to specify the subscription you want to create the test environment in.</span></span>
6. <span data-ttu-id="254c8-295">使用匯入相同的 Azure 訂用帳戶的管理憑證`Get-AzurePublishSettingsFile`和`Import-AzurePublishSettingsFile`cmdlet。</span><span class="sxs-lookup"><span data-stu-id="254c8-295">Import a management certificate for the same Azure subscription by using the `Get-AzurePublishSettingsFile` and `Import-AzurePublishSettingsFile` cmdlets.</span></span> <span data-ttu-id="254c8-296">這些 cmdlet 的第一個下載的憑證檔案，並在第二個您指定才能將它匯入該檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="254c8-296">The first of these cmdlets downloads a certificate file, and in the second one you specify the location of that file in order to import it.</span></span> > [!IMPORTANT]
 > <span data-ttu-id="254c8-297">將下載的檔案保留在安全的位置，或刪除當您完成時，因為它包含可以用來管理您的 Azure 服務的憑證。</span><span class="sxs-lookup"><span data-stu-id="254c8-297">Keep the downloaded file in a safe location or delete it when you're done with it, because it contains a certificate that can be used to manage your Azure services.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    <span data-ttu-id="254c8-298">使用憑證的 REST API 呼叫，以便設定 SQL Database 伺服器上的防火牆規則偵測到開發電腦的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="254c8-298">The certificate is used for a REST API call that detects the development machine's IP address in order to set a firewall rule on the SQL Database server.</span></span>
7. <span data-ttu-id="254c8-299">執行[Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (別名`cd`， `chdir`，和`sl`) 來瀏覽至包含的指令碼的目錄。</span><span class="sxs-lookup"><span data-stu-id="254c8-299">Run the [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (aliases are `cd`, `chdir`, and `sl`) to navigate to the directory that contains the scripts.</span></span> <span data-ttu-id="254c8-300">(它們位於*自動化*修正它的方案資料夾中的資料夾。)如果任何目錄名稱包含空格，請將以引號括住的路徑。</span><span class="sxs-lookup"><span data-stu-id="254c8-300">(They're located in the *Automation* folder in the Fix It solution folder.) Put the path in quotes if any of the directory names contain spaces.</span></span> <span data-ttu-id="254c8-301">例如，若要瀏覽至`c:\Sample Apps\FixIt\Automation`目錄，您可以輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="254c8-301">For example, to navigate to the `c:\Sample Apps\FixIt\Automation` directory you could enter the following command:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. <span data-ttu-id="254c8-302">若要讓 Windows PowerShell 來執行這些指令碼，使用[Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet。</span><span class="sxs-lookup"><span data-stu-id="254c8-302">To allow Windows PowerShell to run these scripts, use the [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet.</span></span> <span data-ttu-id="254c8-303">（因為它們已從網際網路下載會封鎖指令碼）。</span><span class="sxs-lookup"><span data-stu-id="254c8-303">(The scripts are blocked because they were downloaded from the Internet.)</span></span>

    > [!WARNING]
    > <span data-ttu-id="254c8-304">安全性-再執行`Unblock-File`上任何指令碼或可執行檔中，開啟 [記事本] 中的檔案、 檢查命令，並確認它們不包含任何惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-304">Security - Before running `Unblock-File` on any script or executable file, open the file in Notepad, examine the commands, and verify that they do not contain any malicious code.</span></span>

    <span data-ttu-id="254c8-305">例如，下列命令執行`Unblock-File`cmdlet 上目前的目錄中的所有指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-305">For example, the following command runs the `Unblock-File` cmdlet on all scripts in the current directory.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. <span data-ttu-id="254c8-306">若要建立 web 應用程式基底 （沒有處理佇列） 修正此問題的應用程式，執行環境建立指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-306">To create the web app for the base (no queues processing) Fix It app, run the environment creation script.</span></span>

    <span data-ttu-id="254c8-307">所需`Name`參數指定的資料庫名稱，也會用於指令碼建立的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="254c8-307">The required `Name` parameter specifies the name of the database and is also used for the storage account that the script creates.</span></span> <span data-ttu-id="254c8-308">名稱必須是 azurewebsites.net 網域內是唯一的。</span><span class="sxs-lookup"><span data-stu-id="254c8-308">The name must be globally unique within the azurewebsites.net domain.</span></span> <span data-ttu-id="254c8-309">如果您指定的名稱不是唯一的例如 Fixit 或測試 （或甚至是如 fixitdemo 範例），`New-AzureWebsite`指令程式會失敗並報告衝突發生內部錯誤。</span><span class="sxs-lookup"><span data-stu-id="254c8-309">If you specify a name that is not unique, like Fixit or Test (or even as in the example, fixitdemo), the `New-AzureWebsite` cmdlet fails with an Internal Error that reports a conflict.</span></span> <span data-ttu-id="254c8-310">指令碼會將名稱轉換為全部小寫，以符合用於 web 應用程式、 儲存體帳戶和資料庫的名稱需求。</span><span class="sxs-lookup"><span data-stu-id="254c8-310">The script converts the name to all lower-case to comply with name requirements for web apps, storage accounts, and databases.</span></span>

    <span data-ttu-id="254c8-311">所需`SqlDatabasePassword`參數會指定將建立 SQL 資料庫系統管理員帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-311">The required `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="254c8-312">不要在密碼中包含的特殊 XML 字元 (&amp; &lt; &gt; ;)。</span><span class="sxs-lookup"><span data-stu-id="254c8-312">Don't include special XML characters in the password (&amp; &lt; &gt; ;).</span></span> <span data-ttu-id="254c8-313">這是方式的指令碼所撰寫的 Azure 限制的限制。</span><span class="sxs-lookup"><span data-stu-id="254c8-313">This is a limitation of the way the scripts were written, not a limitation of Azure.</span></span>

    <span data-ttu-id="254c8-314">例如，如果您想要建立名為"fixitdemo 「 web 應用程式，並使用 「 Passw0rd1"的 SQL Server 系統管理員密碼，您可以輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="254c8-314">For example, if you want to create a web app named "fixitdemo" and use a SQL Server administrator password of "Passw0rd1", you could enter the following command:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    <span data-ttu-id="254c8-315">在 azurewebsites.net 網域中，名稱必須是唯一且密碼必須符合 SQL 資料庫密碼複雜性需求。</span><span class="sxs-lookup"><span data-stu-id="254c8-315">The name must be unique in the azurewebsites.net domain, and the password must meet SQL Database requirements for password complexity.</span></span> <span data-ttu-id="254c8-316">（範例 Passw0rd1 沒有符合的需求）。</span><span class="sxs-lookup"><span data-stu-id="254c8-316">(The example Passw0rd1 does meet the requirements.)</span></span>

    <span data-ttu-id="254c8-317">請注意，命令開始使用 」。\".</span><span class="sxs-lookup"><span data-stu-id="254c8-317">Note that the command begins with ".\".</span></span> <span data-ttu-id="254c8-318">以協助防止惡意的指令碼執行 Windows PowerShell 會要求您提供的指令碼檔案的完整的路徑，當您執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-318">To help prevent malicious execution of scripts, Windows PowerShell requires that you provide the fully qualified path to the script file when you run a script.</span></span> <span data-ttu-id="254c8-319">您可以使用點來指出目前的目錄 (「。\")或者，提供完整的路徑，例如：</span><span class="sxs-lookup"><span data-stu-id="254c8-319">You can use a dot to indicate the current directory (".\") or provide the fully qualified path, such as:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    <span data-ttu-id="254c8-320">如需有關此指令碼的詳細資訊，請使用`Get-Help`cmdlet。</span><span class="sxs-lookup"><span data-stu-id="254c8-320">For more information about the script, use the `Get-Help` cmdlet.</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    <span data-ttu-id="254c8-321">您可以使用`Detailed`， `Full`， `Parameters`，和`Examples`來篩選傳回的說明 Get-help cmdlet 的參數。</span><span class="sxs-lookup"><span data-stu-id="254c8-321">You can use the `Detailed`, `Full`, `Parameters`, and `Examples` parameters of the Get-Help cmdlet to filter the help that is returned.</span></span>

    <span data-ttu-id="254c8-322">如果指令碼失敗或產生錯誤，例如"New-azurewebsite:: 呼叫組 AzureSubscription 和 Select-azuresubscription 首先，「 您可能未完成 Azure PowerShell 的組態。</span><span class="sxs-lookup"><span data-stu-id="254c8-322">If the script fails or generates errors, such as "New-AzureWebsite : Call Set-AzureSubscription and Select-AzureSubscription first," you might not have completed the configuration of Azure PowerShell.</span></span>

    <span data-ttu-id="254c8-323">在指令碼完成之後，您可以使用 Azure 管理入口網站，以查看所建立的資源中所示[一切自動化](automate-everything.md)章節。</span><span class="sxs-lookup"><span data-stu-id="254c8-323">After the script finishes, you can use the Azure Management Portal to see the resources that were created, as shown in the [Automate Everything](automate-everything.md) chapter.</span></span>
10. <span data-ttu-id="254c8-324">若要將 FixIt 專案部署到新的 Azure 環境中，使用*AzureWebsite.ps1*指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-324">To deploy the FixIt project to the new Azure environment, use the *AzureWebsite.ps1* script.</span></span> <span data-ttu-id="254c8-325">例如: </span><span class="sxs-lookup"><span data-stu-id="254c8-325">For example:</span></span>

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    <span data-ttu-id="254c8-326">當部署完成時，修正它在 Azure 中執行瀏覽器會開啟。</span><span class="sxs-lookup"><span data-stu-id="254c8-326">When deployment is done, the browser opens with Fix It running in Azure.</span></span>

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a><span data-ttu-id="254c8-327">疑難排解 Windows PowerShell 指令碼</span><span class="sxs-lookup"><span data-stu-id="254c8-327">Troubleshooting the Windows PowerShell scripts</span></span>

<span data-ttu-id="254c8-328">執行下列指令碼時遇到最常見的錯誤相關的權限。</span><span class="sxs-lookup"><span data-stu-id="254c8-328">The most common errors encountered when running these scripts are related to permissions.</span></span> <span data-ttu-id="254c8-329">請確定`Add-AzureAccount`和`Import-AzurePublishSettingsFile`成功，而您用於相同的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="254c8-329">Make sure that `Add-AzureAccount` and `Import-AzurePublishSettingsFile` were successful and that you used them for the same Azure subscription.</span></span> <span data-ttu-id="254c8-330">即使`Add-AzureAccount`已成功您可能必須執行一次。</span><span class="sxs-lookup"><span data-stu-id="254c8-330">Even if `Add-AzureAccount` was successful you might have to run it again.</span></span> <span data-ttu-id="254c8-331">所加入的使用權限`Add-AzureAccount`在 12 小時後到期。</span><span class="sxs-lookup"><span data-stu-id="254c8-331">The permissions added by `Add-AzureAccount` expire in 12 hours.</span></span>

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a><span data-ttu-id="254c8-332">並未將物件參考設定為物件的執行個體。</span><span class="sxs-lookup"><span data-stu-id="254c8-332">Object reference not set to an instance of an object.</span></span>

<span data-ttu-id="254c8-333">如果指令碼會傳回錯誤，例如 「 物件參考未設定為物件的執行個體 」 （表示 Windows PowerShell 找不到處理程序 （這是 null 參考例外狀況） 的物件），執行`Add-AzureAccount`指令程式，然後再試一次指令碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-333">If the script returns errors, such as "Object reference not set to an instance of an object," which means that Windows PowerShell can't find an object to process (this is a null reference exception), run the `Add-AzureAccount` cmdlet and try the script again.</span></span>

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a><span data-ttu-id="254c8-334">InternalError： 伺服器發生內部錯誤。</span><span class="sxs-lookup"><span data-stu-id="254c8-334">InternalError: The server encountered an internal error.</span></span>

<span data-ttu-id="254c8-335">`New-AzureWebsite`名稱不是唯一 azurewebsites.net 網域中時，cmdlet 會傳回發生內部錯誤。</span><span class="sxs-lookup"><span data-stu-id="254c8-335">The `New-AzureWebsite` cmdlet returns an internal error when the name is not unique in the azurewebsites.net domain.</span></span> <span data-ttu-id="254c8-336">若要解決錯誤，請使用不同的值做為名稱，也就是 Name 參數中*新增 AzureWebsiteEnv.ps1*。</span><span class="sxs-lookup"><span data-stu-id="254c8-336">To resolve the error, use a different value for the name, which is in the Name parameter of *New-AzureWebsiteEnv.ps1*.</span></span>

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a><span data-ttu-id="254c8-337">重新啟動指令碼</span><span class="sxs-lookup"><span data-stu-id="254c8-337">Restarting the script</span></span>

<span data-ttu-id="254c8-338">如果您需要重新啟動*新增 AzureWebsiteEnv.ps1*指令碼因為它無法在列印 「 指令碼完成 」 訊息之前，您可能想要刪除之前建立的指令碼停止的資源。</span><span class="sxs-lookup"><span data-stu-id="254c8-338">If you need to restart the *New-AzureWebsiteEnv.ps1* script because it failed before it printed the "Script is complete" message, you might want to delete resources that the script created before it stopped.</span></span> <span data-ttu-id="254c8-339">比方說，如果指令碼已經建立 ContosoFixItDemo web 應用程式，並且具有相同名稱重新執行指令碼，指令碼將會失敗，因為名稱正在使用中。</span><span class="sxs-lookup"><span data-stu-id="254c8-339">For example, if the script already created the ContosoFixItDemo web app and you run the script again with the same name, the script will fail because the name is in use.</span></span>

<span data-ttu-id="254c8-340">若要判斷哪些資源停止之前所建立的指令碼，請使用下列 cmdlet:</span><span class="sxs-lookup"><span data-stu-id="254c8-340">To determine which resources the script created before it stopped, use the following cmdlets:</span></span>

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- <span data-ttu-id="254c8-341">`Get-AzureSqlDatabase`： 若要執行這個指令程式，使用管線傳送至資料庫伺服器名稱`Get-AzureSqlDatabase`:</span><span class="sxs-lookup"><span data-stu-id="254c8-341">`Get-AzureSqlDatabase`: To run this cmdlet, pipe the database server name to `Get-AzureSqlDatabase`:</span></span>  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

<span data-ttu-id="254c8-342">若要刪除這些資源，請使用下列命令。</span><span class="sxs-lookup"><span data-stu-id="254c8-342">To delete these resources, use the following commands.</span></span> <span data-ttu-id="254c8-343">請注意，如果您刪除的資料庫伺服器，您會自動刪除與伺服器相關聯的資料庫。</span><span class="sxs-lookup"><span data-stu-id="254c8-343">Note that if you delete the database server, you automatically delete the databases associated with the server.</span></span>

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a><span data-ttu-id="254c8-344">如何部署應用程式與處理 Azure App Service Web 應用程式和 Azure 雲端服務的佇列</span><span class="sxs-lookup"><span data-stu-id="254c8-344">How to deploy the app with queue processing to Azure App Service Web Apps and an Azure Cloud Service</span></span>

<span data-ttu-id="254c8-345">若要啟用佇列，進行下列變更 MyFixIt\Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="254c8-345">To enable queues, make the following change in the MyFixIt\Web.config file.</span></span> <span data-ttu-id="254c8-346">在下`appSettings`，變更的值`UseQueues`為"true":</span><span class="sxs-lookup"><span data-stu-id="254c8-346">Under `appSettings`, change the value of `UseQueues` to "true":</span></span> 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

<span data-ttu-id="254c8-347">然後將 web 應用程式在 Azure 應用程式服務中，在 MVC 應用程式部署所述[早](#deploybase)。</span><span class="sxs-lookup"><span data-stu-id="254c8-347">Then deploy the MVC application to an web app in Azure App Service, as described [earlier](#deploybase).</span></span>

<span data-ttu-id="254c8-348">接下來，建立新的 Azure 雲端服務。</span><span class="sxs-lookup"><span data-stu-id="254c8-348">Next, create a new Azure cloud service.</span></span> <span data-ttu-id="254c8-349">修正它應用程式隨附的指令碼不要建立或部署雲端服務，因此您必須使用 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="254c8-349">The scripts included with the Fix It app do not create or deploy the cloud service, so you must use Azure portal for this.</span></span> <span data-ttu-id="254c8-350">在入口網站中，按一下**新增** -- **計算**–**雲端服務** -- **快速建立**，然後輸入 URL和資料中心位置。</span><span class="sxs-lookup"><span data-stu-id="254c8-350">In the portal, click **New** -- **Compute** – **Cloud Service** -- **Quick Create**, and then enter a URL and a data center location.</span></span> <span data-ttu-id="254c8-351">使用相同的資料中心部署 web 應用程式的位置。</span><span class="sxs-lookup"><span data-stu-id="254c8-351">Use the same data center where you deployed the web app.</span></span>

![](the-fix-it-sample-application/_static/image1.png)

<span data-ttu-id="254c8-352">您可以部署雲端服務之前，您需要更新部分組態檔案。</span><span class="sxs-lookup"><span data-stu-id="254c8-352">Before you can deploy the cloud service, you need to update some of the configuration files.</span></span>

<span data-ttu-id="254c8-353">MyFixIt.WorkerRoler\app.config，在底下`connectionStrings`，取代的值`appdb`與實際的連接字串，SQL database 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="254c8-353">In MyFixIt.WorkerRoler\app.config, under `connectionStrings`, replace the value of the `appdb` connection string with the actual connection string for the SQL Database.</span></span> <span data-ttu-id="254c8-354">您可以從入口網站取得的連接字串。</span><span class="sxs-lookup"><span data-stu-id="254c8-354">You can get the connection string from the portal.</span></span> <span data-ttu-id="254c8-355">在入口網站中，按一下**SQL 資料庫** - **appdb** - **檢視 SQL Database 連接字串，如 Ado.net、 ODBC、 PHP 和 JDBC**。</span><span class="sxs-lookup"><span data-stu-id="254c8-355">In the portal, click **SQL Databases** - **appdb** - **View SQL Database connection strings for ADO .Net, ODBC, PHP, and JDBC**.</span></span> <span data-ttu-id="254c8-356">複製 ADO.NET 連接字串，並將值貼到 app.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="254c8-356">Copy the ADO.NET connection string and paste the value into the app.config file.</span></span> <span data-ttu-id="254c8-357">取代"{您\_密碼\_這裡}"與您的資料庫密碼。</span><span class="sxs-lookup"><span data-stu-id="254c8-357">Replace "{your\_password\_here}" with your database password.</span></span> <span data-ttu-id="254c8-358">(假設您使用指令碼來部署 MVC 應用程式，您指定的資料庫密碼`SqlDatabasePassword`指令碼參數。)</span><span class="sxs-lookup"><span data-stu-id="254c8-358">(Assuming you used the scripts to deploy the MVC app, you specified the database password in the `SqlDatabasePassword` script parameter.)</span></span>

<span data-ttu-id="254c8-359">結果應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="254c8-359">The result should look like the following:</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

<span data-ttu-id="254c8-360">在相同的 MyFixIt.WorkerRoler\app.config 檔案中，在`appSettings`，取代兩個 Azure 儲存體帳戶的預留位置值。</span><span class="sxs-lookup"><span data-stu-id="254c8-360">In the same MyFixIt.WorkerRoler\app.config file, under `appSettings`, replace the two placeholder values for the Azure storage account.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

<span data-ttu-id="254c8-361">您可以從入口網站取得的存取金鑰。</span><span class="sxs-lookup"><span data-stu-id="254c8-361">You can get the access key from the portal.</span></span> <span data-ttu-id="254c8-362">請參閱[如何管理儲存體帳戶](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)。</span><span class="sxs-lookup"><span data-stu-id="254c8-362">See [How To Manage Storage Accounts](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).</span></span>

<span data-ttu-id="254c8-363">在 MyFixItCloudService\ServiceConfiguration.Cloud.cscfg，取代 Azure 儲存體帳戶相同的兩個預留位置值。</span><span class="sxs-lookup"><span data-stu-id="254c8-363">In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, replace the same two placeholders values for the Azure storage account.</span></span>

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

<span data-ttu-id="254c8-364">現在您已準備好要部署雲端服務。</span><span class="sxs-lookup"><span data-stu-id="254c8-364">Now you are ready to deploy the cloud service.</span></span> <span data-ttu-id="254c8-365">在方案總管 中以滑鼠右鍵按一下 MyFixItCloudService 專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="254c8-365">In Solution Explore, right-click the MyFixItCloudService project and select **Publish**.</span></span> <span data-ttu-id="254c8-366">如需詳細資訊，請參閱 「[部署至 Azure 應用程式](https://www.windowsazure.com/en-us/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"，這是在第 2 部分[本教學課程](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)。</span><span class="sxs-lookup"><span data-stu-id="254c8-366">For more information, see "[Deploy the Application to Azure](https://www.windowsazure.com/en-us/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", which is in part 2 of [this tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="254c8-367">上一步</span><span class="sxs-lookup"><span data-stu-id="254c8-367">Previous</span></span>](more-patterns-and-guidance.md)
