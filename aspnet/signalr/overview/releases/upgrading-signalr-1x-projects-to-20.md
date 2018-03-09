---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "將 SignalR 1.x 專案升級到版本 2 |Microsoft 文件"
author: pfletcher
description: "本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x，以及如何疑難排解，升級程序期間可能發生問題..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="68a0c-103">將 SignalR 1.x 專案升級到版本 2</span><span class="sxs-lookup"><span data-stu-id="68a0c-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="68a0c-104">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="68a0c-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="68a0c-105">本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x，以及如何升級程序期間可能發生的問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="68a0c-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="68a0c-106">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="68a0c-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="68a0c-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="68a0c-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="68a0c-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="68a0c-108">.NET 4.5</span></span>
> - <span data-ttu-id="68a0c-109">SignalR 版本 1 和 2</span><span class="sxs-lookup"><span data-stu-id="68a0c-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="68a0c-110">使用 Visual Studio 2012 進行這個教學課程</span><span class="sxs-lookup"><span data-stu-id="68a0c-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="68a0c-111">透過本教學課程中使用 Visual Studio 2012，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="68a0c-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="68a0c-112">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。</span><span class="sxs-lookup"><span data-stu-id="68a0c-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="68a0c-113">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="68a0c-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="68a0c-114">在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="68a0c-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="68a0c-115">這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="68a0c-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="68a0c-116">某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="68a0c-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="68a0c-117">問題和註解</span><span class="sxs-lookup"><span data-stu-id="68a0c-117">Questions and comments</span></span>
> 
> <span data-ttu-id="68a0c-118">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="68a0c-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="68a0c-119">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="68a0c-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="68a0c-120">SignalR 2 提供一致的開發經驗跨伺服器平台使用[OWIN](http://owin.org)。</span><span class="sxs-lookup"><span data-stu-id="68a0c-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="68a0c-121">這篇文章描述更新為版本 2 的 SignalR 1.x 應用程式所需的幾個步驟。</span><span class="sxs-lookup"><span data-stu-id="68a0c-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="68a0c-122">它鼓勵升級至 SignalR 2 應用程式，而 SignalR 1.x 仍會支援。</span><span class="sxs-lookup"><span data-stu-id="68a0c-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="68a0c-123">本教學課程說明如何升級至 SignalR 2 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="68a0c-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="68a0c-124">在 SignalR 2 現在支援自我裝載的應用程式 （其裝載的主控台應用程式、 Windows 服務或其他處理程序中的伺服器）。</span><span class="sxs-lookup"><span data-stu-id="68a0c-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="68a0c-125">如需如何開始使用 SignalR 2 建立的自我裝載應用程式資訊，請參閱[教學課程： 自我裝載的 SignalR](../deployment/tutorial-signalr-self-host.md)。</span><span class="sxs-lookup"><span data-stu-id="68a0c-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="68a0c-126">內容</span><span class="sxs-lookup"><span data-stu-id="68a0c-126">Contents</span></span>

<span data-ttu-id="68a0c-127">下列章節說明升級 SignalR 專案，以及如何疑難排解可能遇到的問題的相關工作。</span><span class="sxs-lookup"><span data-stu-id="68a0c-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="68a0c-128">範例： 升級至 SignalR 2 的快速入門教學課程</span><span class="sxs-lookup"><span data-stu-id="68a0c-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="68a0c-129">在升級期間發生錯誤的疑難排解</span><span class="sxs-lookup"><span data-stu-id="68a0c-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="68a0c-130">範例： 快速入門教學課程應用程式升級為 SignalR 2</span><span class="sxs-lookup"><span data-stu-id="68a0c-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="68a0c-131">在本節中，您要更新應用程式中建立[SignalR 1.x 版快速入門教學課程](../older-versions/index.md)使用 SignalR 2。</span><span class="sxs-lookup"><span data-stu-id="68a0c-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="68a0c-132">一旦您完成快速入門教學課程，請以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="68a0c-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="68a0c-133">確認**目標 framework**設**.NET Framework 4.5。**</span><span class="sxs-lookup"><span data-stu-id="68a0c-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="68a0c-134">開啟 封裝管理員主控台。</span><span class="sxs-lookup"><span data-stu-id="68a0c-134">Open the Package Manager Console.</span></span> <span data-ttu-id="68a0c-135">移除 SignalR 1.x 從專案中使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="68a0c-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="68a0c-136">安裝 SignalR 2 使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="68a0c-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="68a0c-137">在 HTML 頁面中，更新的指令碼參考適用於 SignalR 符合現在包含在專案中的指令碼的版本。</span><span class="sxs-lookup"><span data-stu-id="68a0c-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="68a0c-138">在全域應用程式類別中，移除 MapHubs 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="68a0c-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="68a0c-139">以滑鼠右鍵按一下方案，然後選取**新增**，**新項目...**.在對話方塊中，選取**Owin 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="68a0c-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="68a0c-140">將新類別**Startup.cs**。</span><span class="sxs-lookup"><span data-stu-id="68a0c-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="68a0c-141">Startup.cs 中的內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="68a0c-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="68a0c-142">組件屬性將類別加入至 Owin 的啟動程序，它會執行`Configuration`Owin 啟動時的方法。</span><span class="sxs-lookup"><span data-stu-id="68a0c-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="68a0c-143">這會依次呼叫`MapSignalR`方法，可在應用程式中建立所有 SignalR 中樞的路由。</span><span class="sxs-lookup"><span data-stu-id="68a0c-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="68a0c-144">執行專案，並將主頁面的 URL 複製到另一個瀏覽器或瀏覽器 窗格之前。</span><span class="sxs-lookup"><span data-stu-id="68a0c-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="68a0c-145">每個頁面會要求使用者名稱，並從每一頁所傳送的訊息應該在這兩個瀏覽器窗格中顯示。</span><span class="sxs-lookup"><span data-stu-id="68a0c-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="68a0c-146">在升級期間發生錯誤的疑難排解</span><span class="sxs-lookup"><span data-stu-id="68a0c-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="68a0c-147">本節說明在升級期間可能遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="68a0c-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="68a0c-148">如需更完整的 SignalR 應用程式，可能會發生的錯誤和問題清單，請參閱[SignalR 疑難排解](../testing-and-debugging/troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="68a0c-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="68a0c-149">' 的呼叫是下列的方法或屬性之間模稜兩可的 '</span><span class="sxs-lookup"><span data-stu-id="68a0c-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="68a0c-150">如果的參考，會發生這個錯誤`Microsoft.AspNet.SignalR.Owin`不會移除。</span><span class="sxs-lookup"><span data-stu-id="68a0c-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="68a0c-151">此套件已被取代。必須移除參照，必須先解除安裝 SelfHost 封裝 1.x 版本。</span><span class="sxs-lookup"><span data-stu-id="68a0c-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="68a0c-152">中樞的方法以無訊息模式失敗</span><span class="sxs-lookup"><span data-stu-id="68a0c-152">Hub methods fail silently</span></span>

<span data-ttu-id="68a0c-153">確認您的用戶端指令碼參考最多的日期，且`OwinStartup`啟動類別具有正確的類別和專案的組件名稱屬性。</span><span class="sxs-lookup"><span data-stu-id="68a0c-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="68a0c-154">此外，再試一次開啟中樞中的位址 （/signalr/集線器） 您的瀏覽器;會出現任何錯誤會提供有關發生什麼事錯誤的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="68a0c-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
