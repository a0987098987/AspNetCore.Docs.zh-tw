---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: "建立真實世界雲端應用程式與 Azure |Microsoft 文件"
author: MikeWasson
description: "這個電子書引領您完成模式為基礎的方法來建置實際雲端解決方案。 在開發程序以及與模式適用於..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 4de0b52e0b4ae7ce00e7b07bce2decfc5068964a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="25262-104">建立真實世界雲端應用程式與 Azure</span><span class="sxs-lookup"><span data-stu-id="25262-104">Building Real-World Cloud Apps with Azure</span></span>
====================
<span data-ttu-id="25262-105">由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="25262-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="25262-106">[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="25262-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="25262-107">這個電子書引領您完成模式為基礎的方法來建置實際雲端解決方案。</span><span class="sxs-lookup"><span data-stu-id="25262-107">This e-book walks you through a patterns-based approach to building real-world cloud solutions.</span></span> <span data-ttu-id="25262-108">模式適用於開發程序以及架構和編碼方式。</span><span class="sxs-lookup"><span data-stu-id="25262-108">The patterns apply to the development process as well as to architecture and coding practices.</span></span>
> 
> <span data-ttu-id="25262-109">內容為基礎的簡報 Scott Guthrie 所開發並由他在挪威開發人員會議 (NDC) 中傳遞的 2013 年 6 月 ([第 1 部分](http://vimeo.com/68215538)，[第 2 部分](http://vimeo.com/68215602))，而是在 Microsoft 技術 Ed 澳洲中2013 年 9 月日 ([第 1 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)，[第 2 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325))。</span><span class="sxs-lookup"><span data-stu-id="25262-109">The content is based on a presentation developed by Scott Guthrie and delivered by him at the Norwegian Developers Conference (NDC) in June of 2013 ([part 1](http://vimeo.com/68215538), [part 2](http://vimeo.com/68215602)), and at Microsoft Tech Ed Australia in September, 2013 ([part 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [part 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)).</span></span> <span data-ttu-id="25262-110">[許多其他](more-patterns-and-guidance.md#acknowledgments)更新和增強內容時從視訊來書寫形式轉換它。</span><span class="sxs-lookup"><span data-stu-id="25262-110">[Many others](more-patterns-and-guidance.md#acknowledgments) updated and augmented the content while transitioning it from video to written form.</span></span>


## <a name="intended-audience"></a><span data-ttu-id="25262-111">適用對象</span><span class="sxs-lookup"><span data-stu-id="25262-111">Intended Audience</span></span>

<span data-ttu-id="25262-112">對於雲端開發感到好奇考慮移至雲端，或為新雲端開發人員會發現以下的最重要的概念和做法，他們需要知道的簡要概觀。</span><span class="sxs-lookup"><span data-stu-id="25262-112">Developers who are curious about developing for the cloud, considering a move to the cloud, or are new to cloud development will find here a concise overview of the most important concepts and practices they need to know.</span></span> <span data-ttu-id="25262-113">概念說明具體範例，與其他資源，如需進一步資訊每個章節連結。</span><span class="sxs-lookup"><span data-stu-id="25262-113">The concepts are illustrated with concrete examples, and each chapter links to other resources for more in-depth information.</span></span> <span data-ttu-id="25262-114">範例和其他資源連結適用於 Microsoft 架構及服務，但是所闡述的原則套用至其他 web 開發架構和雲端部署環境。</span><span class="sxs-lookup"><span data-stu-id="25262-114">The examples and the links to additional resources are for Microsoft frameworks and services, but the principles illustrated apply to other web development frameworks and cloud environments as well.</span></span>

<span data-ttu-id="25262-115">已經雲端開發人員可能會發現想法此處可協助使其更順利。</span><span class="sxs-lookup"><span data-stu-id="25262-115">Developers who are already developing for the cloud may find ideas here that will help make them more successful.</span></span> <span data-ttu-id="25262-116">可以獨立地讀取數列中的每一章，讓您可以挑選並選擇您感興趣的主題。</span><span class="sxs-lookup"><span data-stu-id="25262-116">Each chapter in the series can be read independently, so you can pick and choose topics that you're interested in.</span></span>

<span data-ttu-id="25262-117">監看 Scott Guthrie 的任何人*建置真實世界雲端應用程式與 Azure*展示和要有更多詳細資料和更新的資訊會發現這裡。</span><span class="sxs-lookup"><span data-stu-id="25262-117">Anyone who watched Scott Guthrie's *Building Real World Cloud Apps with Azure* presentation and wants more details and updated information will find that here.</span></span>

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a><span data-ttu-id="25262-118">雲端開發模式</span><span class="sxs-lookup"><span data-stu-id="25262-118">Cloud development patterns</span></span>

<span data-ttu-id="25262-119">這個電子書說明十三個建議的雲端開發模式。</span><span class="sxs-lookup"><span data-stu-id="25262-119">This e-book explains thirteen recommended patterns for cloud development.</span></span> <span data-ttu-id="25262-120">「 」 中使用模式這裡的廣泛的意義上表示執行動作的建議的方式： 如何將進行開發、 設計和撰寫程式碼的雲端應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-120">"Pattern" is used here in a broad sense to mean a recommended way to do things: how best to go about developing, designing, and coding cloud apps.</span></span> <span data-ttu-id="25262-121">這些是可協助您 」 都會歸類為成功的 pit 「 如果您遵循這些重要模式。</span><span class="sxs-lookup"><span data-stu-id="25262-121">These are key patterns which will help you "fall into the pit of success" if you follow them.</span></span>

- <span data-ttu-id="25262-122">[一切自動化](automate-everything.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-122">[Automate everything](automate-everything.md).</span></span>

    - <span data-ttu-id="25262-123">使用指令碼效率最大化，並減少重複性的程序中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="25262-123">Use scripts to maximize efficiency and minimize errors in repetitive processes.</span></span>
    - <span data-ttu-id="25262-124">示範： Azure 管理指令碼。</span><span class="sxs-lookup"><span data-stu-id="25262-124">Demo: Azure management scripts.</span></span>
- <span data-ttu-id="25262-125">[原始檔控制](source-control.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-125">[Source control](source-control.md).</span></span> 

    - <span data-ttu-id="25262-126">設定原始檔控制中的分支結構，以促進 DevOps 工作流程。</span><span class="sxs-lookup"><span data-stu-id="25262-126">Set up branching structure in source control to facilitate DevOps workflow.</span></span>
    - <span data-ttu-id="25262-127">示範： 在原始檔控制中加入指令碼。</span><span class="sxs-lookup"><span data-stu-id="25262-127">Demo: add scripts to source control.</span></span>
    - <span data-ttu-id="25262-128">示範： 保留出原始檔控制的敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="25262-128">Demo: keep sensitive data out of source control.</span></span>
    - <span data-ttu-id="25262-129">示範： 使用 Visual Studio 中的 Git。</span><span class="sxs-lookup"><span data-stu-id="25262-129">Demo: use Git in Visual Studio.</span></span>
- <span data-ttu-id="25262-130">[持續整合與傳遞](continuous-integration-and-continuous-delivery.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-130">[Continuous integration and delivery](continuous-integration-and-continuous-delivery.md).</span></span> 

    - <span data-ttu-id="25262-131">自動化建置和部署與每個原始檔控制簽入。</span><span class="sxs-lookup"><span data-stu-id="25262-131">Automate build and deployment with each source control check-in.</span></span>
- <span data-ttu-id="25262-132">[Web 開發最佳作法](web-development-best-practices.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-132">[Web development best practices](web-development-best-practices.md).</span></span> 

    - <span data-ttu-id="25262-133">Web 層保持無狀態的。</span><span class="sxs-lookup"><span data-stu-id="25262-133">Keep web tier stateless.</span></span>
    - <span data-ttu-id="25262-134">示範： 調整和 Azure App Service Web 應用程式中的自動調整。</span><span class="sxs-lookup"><span data-stu-id="25262-134">Demo: scaling and auto-scaling in Web Apps in Azure App Service.</span></span>
    - <span data-ttu-id="25262-135">避免工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="25262-135">Avoid session state.</span></span>
    - <span data-ttu-id="25262-136">使用 CDN。</span><span class="sxs-lookup"><span data-stu-id="25262-136">Use a CDN.</span></span>
    - <span data-ttu-id="25262-137">使用非同步程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="25262-137">Use asynchronous programming model.</span></span>
    - <span data-ttu-id="25262-138">示範： 在 ASP.NET MVC 和 Entity Framework 中的非同步。</span><span class="sxs-lookup"><span data-stu-id="25262-138">Demo: async in ASP.NET MVC and Entity Framework.</span></span>
- <span data-ttu-id="25262-139">[單一登入](single-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-139">[Single sign-on](single-sign-on.md).</span></span> 

    - <span data-ttu-id="25262-140">Azure Active Directory 的簡介。</span><span class="sxs-lookup"><span data-stu-id="25262-140">Introduction to Azure Active Directory.</span></span>
    - <span data-ttu-id="25262-141">示範： 建立使用 Azure Active Directory 的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-141">Demo: create an ASP.NET app that uses Azure Active Directory.</span></span>
- <span data-ttu-id="25262-142">[資料儲存體選項](data-storage-options.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-142">[Data storage options](data-storage-options.md).</span></span> 

    - <span data-ttu-id="25262-143">類型的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="25262-143">Types of data stores.</span></span>
    - <span data-ttu-id="25262-144">如何選擇正確的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="25262-144">How to choose the right data store.</span></span>
    - <span data-ttu-id="25262-145">示範： Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="25262-145">Demo: Azure SQL Database.</span></span>
- <span data-ttu-id="25262-146">[資料分割策略](data-partitioning-strategies.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-146">[Data partitioning strategies](data-partitioning-strategies.md).</span></span> 

    - <span data-ttu-id="25262-147">垂直、 水平資料分割或兩者來協助調整關聯式資料庫。</span><span class="sxs-lookup"><span data-stu-id="25262-147">Partition data vertically, horizontally, or both to facilitate scaling a relational database.</span></span>
- <span data-ttu-id="25262-148">[非結構化的 blob 儲存體](unstructured-blob-storage.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-148">[Unstructured blob storage](unstructured-blob-storage.md).</span></span> 

    - <span data-ttu-id="25262-149">藉由使用 blob 服務將檔案儲存在雲端中。</span><span class="sxs-lookup"><span data-stu-id="25262-149">Store files in the cloud by using the blob service.</span></span>
    - <span data-ttu-id="25262-150">示範： 使用 blob 儲存體中修正該應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-150">Demo: using blob storage in the Fix It app.</span></span>
- <span data-ttu-id="25262-151">[設計存留失敗](design-to-survive-failures.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-151">[Design to survive failures](design-to-survive-failures.md).</span></span> 

    - <span data-ttu-id="25262-152">類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="25262-152">Types of failures.</span></span>
    - <span data-ttu-id="25262-153">失敗的範圍。</span><span class="sxs-lookup"><span data-stu-id="25262-153">Failure Scope.</span></span>
    - <span data-ttu-id="25262-154">了解 Sla。</span><span class="sxs-lookup"><span data-stu-id="25262-154">Understanding SLAs.</span></span>
- <span data-ttu-id="25262-155">[監控與遙測](monitoring-and-telemetry.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-155">[Monitoring and telemetry](monitoring-and-telemetry.md).</span></span> 

    - <span data-ttu-id="25262-156">為什麼您應該同時購買的遙測應用程式並撰寫您自己的程式碼來檢測您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-156">Why you should both buy a telemetry app and write your own code to instrument your app.</span></span>
    - <span data-ttu-id="25262-157">示範： New Relic for Azure</span><span class="sxs-lookup"><span data-stu-id="25262-157">Demo: New Relic for Azure</span></span>
    - <span data-ttu-id="25262-158">示範： 記錄程式碼中修正該應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-158">Demo: logging code in the Fix It app.</span></span>
    - <span data-ttu-id="25262-159">示範： 相依性插入它修正應用程式中。</span><span class="sxs-lookup"><span data-stu-id="25262-159">Demo: dependency injection in the Fix It app.</span></span>
    - <span data-ttu-id="25262-160">示範： 在 Azure 中的內建的記錄支援。</span><span class="sxs-lookup"><span data-stu-id="25262-160">Demo: built-in logging support in Azure.</span></span>
- <span data-ttu-id="25262-161">[暫時性錯誤處理](transient-fault-handling.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-161">[Transient fault handling](transient-fault-handling.md).</span></span> 

    - <span data-ttu-id="25262-162">使用智慧的重試/撤退邏輯來減少暫時性失敗的影響。</span><span class="sxs-lookup"><span data-stu-id="25262-162">Use smart retry/back-off logic to mitigate the effect of transient failures.</span></span>
    - <span data-ttu-id="25262-163">示範： 重試/撤退 Entity Framework 6 中。</span><span class="sxs-lookup"><span data-stu-id="25262-163">Demo: retry/back-off in Entity Framework 6.</span></span>
- <span data-ttu-id="25262-164">[分散式快取](distributed-caching.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-164">[Distributed caching](distributed-caching.md).</span></span> 

    - <span data-ttu-id="25262-165">改善延展性，並使用分散式快取可減少資料庫的交易成本。</span><span class="sxs-lookup"><span data-stu-id="25262-165">Improve scalability and reduce database transaction costs by using distributed caching.</span></span>
- <span data-ttu-id="25262-166">[佇列為主的工作模式](queue-centric-work-pattern.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-166">[Queue-centric work pattern](queue-centric-work-pattern.md).</span></span> 

    - <span data-ttu-id="25262-167">啟用高可用性，並透過鬆散結合的 web 和背景工作層提升可調適性。</span><span class="sxs-lookup"><span data-stu-id="25262-167">Enable high availability and improve scalability by loosely coupling web and worker tiers.</span></span>
    - <span data-ttu-id="25262-168">示範： 修正它應用程式中的 Azure 儲存體佇列。</span><span class="sxs-lookup"><span data-stu-id="25262-168">Demo: Azure storage queues in the Fix It app.</span></span>
- <span data-ttu-id="25262-169">[多個雲端應用程式模式和指引](more-patterns-and-guidance.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-169">[More cloud app patterns and guidance](more-patterns-and-guidance.md).</span></span>
- [<span data-ttu-id="25262-170">附錄︰修正範例應用程式</span><span class="sxs-lookup"><span data-stu-id="25262-170">Appendix: The Fix It Sample Application</span></span>](the-fix-it-sample-application.md)

    - <span data-ttu-id="25262-171">已知問題</span><span class="sxs-lookup"><span data-stu-id="25262-171">Known Issues</span></span>
    - <span data-ttu-id="25262-172">最佳作法</span><span class="sxs-lookup"><span data-stu-id="25262-172">Best Practices</span></span>
    - <span data-ttu-id="25262-173">如何下載、 建置、 執行及部署。</span><span class="sxs-lookup"><span data-stu-id="25262-173">How to download, build, run, and deploy.</span></span>

<span data-ttu-id="25262-174">這些模式套用到所有的雲端環境中，但我們將說明這些點使用的 Microsoft 技術和服務，例如 Visual Studio、 Team Foundation Service、 ASP.NET 和 Azure 為基礎的範例。</span><span class="sxs-lookup"><span data-stu-id="25262-174">These patterns apply to all cloud environments, but we'll illustrate them by using examples based on Microsoft technologies and services, such as Visual Studio, Team Foundation Service, ASP.NET, and Azure.</span></span>

<span data-ttu-id="25262-175">本指南的這個餘數導入了修正它範例應用程式和 Web 應用程式中執行它修正應用程式的 Azure App Service 雲端環境中。</span><span class="sxs-lookup"><span data-stu-id="25262-175">This remainder of this chapter introduces the Fix It sample application and the Web Apps in Azure App Service cloud environment that the Fix It app runs in.</span></span>

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a><span data-ttu-id="25262-176">修正它範例應用程式</span><span class="sxs-lookup"><span data-stu-id="25262-176">The Fix it sample application</span></span>

<span data-ttu-id="25262-177">大部分的螢幕擷取畫面和此的電子書中所顯示的程式碼範例根據它修正應用程式原本是由開發[Scott Guthrie](https://weblogs.asp.net/scottgu/)示範建議的雲端應用程式開發模式和作法。</span><span class="sxs-lookup"><span data-stu-id="25262-177">Most of the screen shots and code examples shown in this e-book are based on the Fix It app originally developed by [Scott Guthrie](https://weblogs.asp.net/scottgu/) to demonstrate recommended cloud app development patterns and practices.</span></span>

![修正應用程式首頁](introduction/_static/image1.png)

<span data-ttu-id="25262-179">範例應用程式是票證系統簡單的工作項目。</span><span class="sxs-lookup"><span data-stu-id="25262-179">The sample app is a simple work item ticketing system.</span></span> <span data-ttu-id="25262-180">當您需要修復的功能時，您建立票證，並指派它某人，和其他人可以登入，並查看票證指派給它們，並將票證標示為已完成時完成的工作。</span><span class="sxs-lookup"><span data-stu-id="25262-180">When you need something fixed, you create a ticket and assign it to someone, and others can log in and see the tickets assigned to them and mark tickets as completed when the work is done.</span></span>

<span data-ttu-id="25262-181">它是標準的 Visual Studio web 專案。</span><span class="sxs-lookup"><span data-stu-id="25262-181">It's a standard Visual Studio web project.</span></span> <span data-ttu-id="25262-182">它建置在 ASP.NET MVC，並使用 SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="25262-182">It is built on ASP.NET MVC and uses a SQL Server database.</span></span> <span data-ttu-id="25262-183">它可以在 IIS Express 在本機執行，而且可以部署至 Azure 網站雲端中執行。</span><span class="sxs-lookup"><span data-stu-id="25262-183">It can run locally in IIS Express and can be deployed to a Azure Web Site to run in the cloud.</span></span> <span data-ttu-id="25262-184">您可以記錄中使用表單驗證和本機資料庫，或使用例如 Google 社交提供者。</span><span class="sxs-lookup"><span data-stu-id="25262-184">You can log in using forms authentication and a local database or by using a social provider such as Google.</span></span> <span data-ttu-id="25262-185">（稍後我們將也示範如何在 Active Directory 組織帳戶登入。）</span><span class="sxs-lookup"><span data-stu-id="25262-185">(Later we'll also show how to log in with an Active Directory organizational account.)</span></span>

![登入頁面](introduction/_static/image2.png)

<span data-ttu-id="25262-187">一旦您登入中建立票證、 將它指派給某人，和上傳您想要獲得修正的圖片。</span><span class="sxs-lookup"><span data-stu-id="25262-187">Once you're logged in you can create a ticket, assign it to someone, and upload a picture of what you want to get fixed.</span></span>

![建立修正它的工作](introduction/_static/image3.png)

![修正此問題建立工作](introduction/_static/image4.png)

<span data-ttu-id="25262-190">您可以追蹤您所建立的工作項目的進度，查看指派給您、 檢視票證的詳細資訊，以及標記項目，為已完成的票證。</span><span class="sxs-lookup"><span data-stu-id="25262-190">You can track the progress of work items you created, see tickets assigned to you, view ticket details, and mark items as completed.</span></span>

<span data-ttu-id="25262-191">這是非常簡單的應用程式功能的觀點而言，但您會看到如何建置它，使其可以擴充至數百萬名使用者，然後將彈性資料庫失敗與連接終止數目等事項。</span><span class="sxs-lookup"><span data-stu-id="25262-191">This is a very simple app from a feature perspective, but you'll see how to build it so that it can scale to millions of users and will be resilient to things like database failures and connection terminations.</span></span> <span data-ttu-id="25262-192">您也會看到如何建立自動化和敏捷式軟體開發的開發工作流程，可讓您能夠啟動簡單和更佳及更好讓應用程式有效率地快速地逐一查看的開發週期。</span><span class="sxs-lookup"><span data-stu-id="25262-192">You'll also see how to create an automated and agile development workflow, which enables you to start simple and make the app better and better by iterating the development cycle efficiently and quickly.</span></span>

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a><span data-ttu-id="25262-193">Azure App Service 中的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="25262-193">Web Apps in Azure App Service</span></span>

<span data-ttu-id="25262-194">修正它應用程式使用的雲端環境是 azure 的一種，我們會呼叫 Web Sites 服務。</span><span class="sxs-lookup"><span data-stu-id="25262-194">The cloud environment used for the Fix It application is a service of Azure that we call Web Sites.</span></span> <span data-ttu-id="25262-195">此服務是方法，您可以裝載自己在 Azure 中的 web 應用程式，而不必建立 Vm，再進行更新、 安裝和設定 IIS，依此類推。我們在我們的 Vm 上裝載您的網站，自動提供備份和復原和其他服務。</span><span class="sxs-lookup"><span data-stu-id="25262-195">This service is a way that you can host your own web app in Azure without having to create VMs and keep them updated, install and configure IIS, etc. We host your site on our VMs and automatically provide backup and recovery and other services for you.</span></span> <span data-ttu-id="25262-196">Web Sites 服務使用 ASP.NET、 Node.js、 PHP 和 Python。</span><span class="sxs-lookup"><span data-stu-id="25262-196">The Web Sites service works with ASP.NET, Node.js, PHP, and Python.</span></span> <span data-ttu-id="25262-197">它可讓您非常快速地使用 Visual Studio、 Web Deploy、 FTP、 Git 或 TFS 部署。</span><span class="sxs-lookup"><span data-stu-id="25262-197">It enables you to deploy very quickly using Visual Studio, Web Deploy, FTP, Git, or TFS.</span></span> <span data-ttu-id="25262-198">它通常是在幾秒內啟動部署的時間更新可在網際網路上的時間之間。</span><span class="sxs-lookup"><span data-stu-id="25262-198">It's usually just a few seconds between the time you start a deployment and the time your update is available over the Internet.</span></span> <span data-ttu-id="25262-199">若要開始，所有完全免費，您可以在您的流量增加而向上延展。</span><span class="sxs-lookup"><span data-stu-id="25262-199">It's all free to get started, and you can scale up as your traffic grows.</span></span>

<span data-ttu-id="25262-200">在幕後，Azure App Service 中的 Web 應用程式會提供許多架構的元件和功能，您必須自行建置，如果您要裝載您自己的 Vm 上使用 IIS 的網站。</span><span class="sxs-lookup"><span data-stu-id="25262-200">Behind the scenes, Web Apps in Azure App Service provides a lot of architectural components and features that you'd have to build yourself if you were going to host a web site using IIS on your own VMs.</span></span> <span data-ttu-id="25262-201">一個元件是部署結束點，會自動設定 IIS，並且您想要執行您的網站的許多 Vm 上安裝應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-201">One component is a deployment end point that automatically configures IIS and installs your application on as many VMs as you want to run your site on.</span></span>

![部署服務](introduction/_static/image5.png)

<span data-ttu-id="25262-203">當使用者叫用的網站時，他們不直接叫用 IIS Vm、 會經歷[Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing)負載平衡器。</span><span class="sxs-lookup"><span data-stu-id="25262-203">When a user hits the web site, they don't hit the IIS VMs directly, they go through [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) load balancers.</span></span> <span data-ttu-id="25262-204">您可以使用這些與您自己的伺服器，但此處的優點是，它們為您自動設定。</span><span class="sxs-lookup"><span data-stu-id="25262-204">You can use these with your own servers, but the advantage here is that they're set up for you automatically.</span></span> <span data-ttu-id="25262-205">使用智慧的啟發學習法會將帳戶因素，例如工作階段親和性，在 IIS 中的佇列深度，以及 CPU 使用量，在每個機器 Vm 的流量引導至裝載您的網站。</span><span class="sxs-lookup"><span data-stu-id="25262-205">They use a smart heuristic that takes into account factors such as session affinity, queue depth in IIS, and CPU usage on each machine to direct traffic to the VMs that host your web site.</span></span>

![ARR 負載平衡器](introduction/_static/image6.png)

<span data-ttu-id="25262-207">如果電腦關閉時，Azure 會自動提取從循環，產生新的 VM 執行個體，而且會開始將新的執行個體--而不需要停機應用程式的所有流量導向。</span><span class="sxs-lookup"><span data-stu-id="25262-207">If a machine goes down, Azure automatically pulls it from the rotation, spins up a new VM instance, and starts directing traffic to the new instance -- all with no down time for your application.</span></span>

![從機器失敗的自動復原](introduction/_static/image7.png)

<span data-ttu-id="25262-209">這會自動發生。</span><span class="sxs-lookup"><span data-stu-id="25262-209">All of this takes place automatically.</span></span> <span data-ttu-id="25262-210">您只需要是建立網站和部署應用程式，使用 Windows PowerShell、 Visual Studio 中或在 Azure 管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="25262-210">All you need to do is create a web site and deploy your application to it, using Windows PowerShell, Visual Studio, or the Azure management portal.</span></span>

<span data-ttu-id="25262-211">如需快速且輕鬆逐步教學課程示範如何在 Visual Studio 中建立 web 應用程式，並將它部署至 Azure 網站，請參閱[開始使用 Azure 和 ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="25262-211">For a quick and easy step-by-step tutorial that shows how to create a web application in Visual Studio and deploy it to a Azure Web Site, see [Get started with Azure and ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<a id="summary"></a>
## <a name="summary"></a><span data-ttu-id="25262-212">總結</span><span class="sxs-lookup"><span data-stu-id="25262-212">Summary</span></span>

<span data-ttu-id="25262-213">本簡介提供一份活頁簿將涵蓋的主題、 螢幕擷取畫面的範例應用程式，並在雲端環境中 Azure App Service Web 應用程式的簡短概觀。</span><span class="sxs-lookup"><span data-stu-id="25262-213">This introduction has provided a list of topics the book will cover, screenshots of the sample application, and a brief overview of the Web Apps in Azure App Service cloud environment.</span></span> <span data-ttu-id="25262-214">開發應用程式中，以及雲端的最大的優點之一是，所以可以輕鬆地自動化重複的開發工作，例如建立測試環境，並將您的程式碼部署至它。</span><span class="sxs-lookup"><span data-stu-id="25262-214">One of the great advantages of developing apps in and for the cloud is that it's easy to automate repetitive development tasks such as creating a test environment and deploying your code to it.</span></span> <span data-ttu-id="25262-215">如何執行這個動作的主旨[下一章](automate-everything.md)。</span><span class="sxs-lookup"><span data-stu-id="25262-215">How to do that is the subject of the [next chapter](automate-everything.md).</span></span>

## <a name="resources"></a><span data-ttu-id="25262-216">資源</span><span class="sxs-lookup"><span data-stu-id="25262-216">Resources</span></span>

<span data-ttu-id="25262-217">如需有關本章節涵蓋之主題的詳細資訊，請參閱下列資源。</span><span class="sxs-lookup"><span data-stu-id="25262-217">For more information about the topics covered in this chapter, see the following resources.</span></span>

<span data-ttu-id="25262-218">文件集：</span><span class="sxs-lookup"><span data-stu-id="25262-218">Documentation:</span></span>

- <span data-ttu-id="25262-219">[Web 應用程式在 Azure App Service 中的](https://azure.microsoft.com/services/app-service/web/)。</span><span class="sxs-lookup"><span data-stu-id="25262-219">[Web Apps in Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="25262-220">如需 Web 應用程式的 Azure 文件入口網站頁面。</span><span class="sxs-lookup"><span data-stu-id="25262-220">Portal page for Azure documentation about Web Apps.</span></span>
- [<span data-ttu-id="25262-221">Web 應用程式、 雲端服務和 Vm： 使用時機？</span><span class="sxs-lookup"><span data-stu-id="25262-221">Web Apps, Cloud Services, and VMs: When to use which?</span></span>](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) <span data-ttu-id="25262-222">如本章所示的 WAWS-DEV.JSON 只是三種方式，您可以在 Azure 中執行 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25262-222">WAWS as shown in this chapter is just one of three ways you can run web apps in Azure.</span></span> <span data-ttu-id="25262-223">本文說明的三種方式之間的差異，並提供有關如何選擇哪一種最適合您案例的指引。</span><span class="sxs-lookup"><span data-stu-id="25262-223">This article explains the differences between the three ways and gives guidance on how to choose which one is right for your scenario.</span></span> <span data-ttu-id="25262-224">Web Sites 中，例如雲端服務是 Azure PaaS 功能。</span><span class="sxs-lookup"><span data-stu-id="25262-224">Like Web Sites, Cloud Services is a PaaS feature of Azure.</span></span> <span data-ttu-id="25262-225">Vm 是 IaaS 功能。</span><span class="sxs-lookup"><span data-stu-id="25262-225">VMs are an IaaS feature.</span></span> <span data-ttu-id="25262-226">如需 PaaS 和 IaaS 的說明，請參閱[資料選項](data-storage-options.md#paasiaas)章節。</span><span class="sxs-lookup"><span data-stu-id="25262-226">For an explanation of PaaS versus IaaS, see the [Data Options](data-storage-options.md#paasiaas) chapter.</span></span>

<span data-ttu-id="25262-227">影片：</span><span class="sxs-lookup"><span data-stu-id="25262-227">Videos:</span></span>

- [<span data-ttu-id="25262-228">Scott Guthrie 開始步驟 0-什麼是 Azure 雲端作業系統？</span><span class="sxs-lookup"><span data-stu-id="25262-228">Scott Guthrie starts at Step 0 - What is the Azure Cloud OS?</span></span>](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- <span data-ttu-id="25262-229">[Web Sites 架構-與 Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)。</span><span class="sxs-lookup"><span data-stu-id="25262-229">[Web Sites Architecture - with Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).</span></span>
- <span data-ttu-id="25262-230">[Azure Web Sites 內部項目與 Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski)。</span><span class="sxs-lookup"><span data-stu-id="25262-230">[Azure Web Sites Internals with Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="25262-231">下一步</span><span class="sxs-lookup"><span data-stu-id="25262-231">Next</span></span>](automate-everything.md)
