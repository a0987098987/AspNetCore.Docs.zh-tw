---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: 將 SignalR 1.x 專案升級至第 2 版 |Microsoft Docs
author: bradygaster
description: 本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x 中，以及如何針對升級的程序期間可能發生的問題進行疑難排解...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: a1cb4903f3cdeef70ffd0f624a3a2170f641a395
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837334"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="e8ec3-103">將 SignalR 1.x 專案升級至第 2 版</span><span class="sxs-lookup"><span data-stu-id="e8ec3-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="e8ec3-104">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e8ec3-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="e8ec3-105">本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x 中，以及如何進行疑難排解，升級程序期間可能發生問題。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e8ec3-106">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="e8ec3-106">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="e8ec3-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e8ec3-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="e8ec3-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e8ec3-108">.NET 4.5</span></span>
> - <span data-ttu-id="e8ec3-109">SignalR 版本 1 和 2</span><span class="sxs-lookup"><span data-stu-id="e8ec3-109">SignalR versions 1 and 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="e8ec3-110">本教學課程中使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e8ec3-110">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="e8ec3-111">若要使用 Visual Studio 2012，本教學課程中，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e8ec3-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="e8ec3-112">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="e8ec3-113">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="e8ec3-114">在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="e8ec3-115">這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="e8ec3-116">有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="e8ec3-117">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="e8ec3-117">Questions and comments</span></span>
>
> <span data-ttu-id="e8ec3-118">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e8ec3-119">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="e8ec3-120">SignalR 2 使用的伺服器平台提供具有一致開發體驗[OWIN](http://owin.org)。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="e8ec3-121">本文說明更新到版本 2 的 SignalR 1.x 應用程式所需的幾個步驟。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="e8ec3-122">雖然建議您升級至 SignalR 2 的應用程式，SignalR 1.x 仍受支援。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="e8ec3-123">本教學課程說明如何升級 web 裝載的應用程式，SignalR 2。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="e8ec3-124">SignalR 2 現在支援自我裝載的應用程式 （其會裝載在主控台應用程式、 Windows 服務或其他處理序伺服器）。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="e8ec3-125">如需如何開始使用 SignalR 2 建立的自我裝載的應用程式的資訊，請參閱[教學課程：SignalR 自我裝載](../deployment/tutorial-signalr-self-host.md)。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="e8ec3-126">內容</span><span class="sxs-lookup"><span data-stu-id="e8ec3-126">Contents</span></span>

<span data-ttu-id="e8ec3-127">下列各節描述升級 SignalR 專案，以及如何針對可能發生的問題進行疑難排解所涉及的工作。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="e8ec3-128">例如：升級至 SignalR 2 的快速入門教學課程</span><span class="sxs-lookup"><span data-stu-id="e8ec3-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="e8ec3-129">在升級期間發生的錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="e8ec3-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="e8ec3-130">範例：開始使用教學課程應用程式升級為 SignalR 2</span><span class="sxs-lookup"><span data-stu-id="e8ec3-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="e8ec3-131">在本節中，您將更新應用程式中建立[入門教學課程的 SignalR 1.x 版](../older-versions/index.md)使用 SignalR 2。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="e8ec3-132">一旦您完成快速入門教學課程，請以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="e8ec3-133">確認**目標 framework**設定為 **.NET Framework 4.5。**</span><span class="sxs-lookup"><span data-stu-id="e8ec3-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="e8ec3-134">開啟 [Package Manager Console]。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-134">Open the Package Manager Console.</span></span> <span data-ttu-id="e8ec3-135">移除 SignalR 1.x 專案使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="e8ec3-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="e8ec3-136">安裝 SignalR 2 使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="e8ec3-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="e8ec3-137">在 HTML 頁面中，更新以符合現在包含在專案中的指令碼版本的 SignalR 的指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="e8ec3-138">在全域應用程式類別中，移除 MapHubs 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="e8ec3-139">以滑鼠右鍵按一下方案，然後選取**新增**，**新項目...**.在對話方塊中，選取**Owin 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="e8ec3-140">新類別命名**Startup.cs**。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="e8ec3-141">Startup.cs 的內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e8ec3-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="e8ec3-142">組件屬性將類別加入至 Owin 的啟動程序，它會執行`Configuration`Owin 啟動時的方法。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="e8ec3-143">這會進而呼叫`MapSignalR`方法會建立應用程式中的所有 SignalR 中樞的路由。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="e8ec3-144">執行專案，並將主頁面的 URL 複製到另一個瀏覽器或瀏覽器窗格中的，與之前。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="e8ec3-145">每一頁會詢問使用者名稱，並從每個頁面傳送的訊息應該在這兩個瀏覽器窗格中顯示。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="e8ec3-146">在升級期間發生的錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="e8ec3-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="e8ec3-147">本節說明在升級期間可能發生的問題。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="e8ec3-148">如需更完整的 SignalR 應用程式，可能會發生的錯誤和問題清單，請參閱 < [SignalR 疑難排解](../testing-and-debugging/troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="e8ec3-149">' 的呼叫是下列的方法或屬性之間模稜兩可的 '</span><span class="sxs-lookup"><span data-stu-id="e8ec3-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="e8ec3-150">如果的參考，會發生此錯誤`Microsoft.AspNet.SignalR.Owin`不會移除。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="e8ec3-151">此套件已被取代;必須先移除參考，並自行封裝的 1.x 版本必須先解除安裝。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="e8ec3-152">中樞的方法以無訊息方式失敗</span><span class="sxs-lookup"><span data-stu-id="e8ec3-152">Hub methods fail silently</span></span>

<span data-ttu-id="e8ec3-153">確認您的用戶端中的指令碼參考最多的日期，而且`OwinStartup`屬性 (attribute) 您的啟動類別具有正確的類別和專案的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="e8ec3-154">此外，請嘗試開啟中樞位址 （signalr/中樞），以在瀏覽器中;會出現任何錯誤會提供錯誤情形的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e8ec3-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
