---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 教學課程： 高頻率即時與 SignalR 2 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。 高頻率訊息處理中...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 008cbc8ec7ca0af4b20e5e3974cf3862efa7677e
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794876"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a><span data-ttu-id="7c5f8-104">教學課程： 高頻率即時與 SignalR 2</span><span class="sxs-lookup"><span data-stu-id="7c5f8-104">Tutorial: High-Frequency Realtime with SignalR 2</span></span>
====================
<span data-ttu-id="7c5f8-105">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7c5f8-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="7c5f8-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="7c5f8-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> <span data-ttu-id="7c5f8-107">本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 2 提供高頻率的傳訊功能。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-107">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="7c5f8-108">在此情況下頻繁訊息表示會以固定費率; 的更新在此應用程式，最多 10 個訊息，第二個。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-108">High-frequency messaging in this case means updates that are sent at a fixed rate; in the case of this application, up to 10 messages a second.</span></span>
> 
> <span data-ttu-id="7c5f8-109">您會在本教學課程中建立應用程式會顯示使用者可以拖曳圖形。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-109">The application you'll create in this tutorial displays a shape that users can drag.</span></span> <span data-ttu-id="7c5f8-110">圖形的位置，所有其他已連線的瀏覽器中就會更新以符合使用定時的更新拖曳圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-110">The position of the shape in all other connected browsers will then be updated to match the position of the dragged shape using timed updates.</span></span>
> 
> <span data-ttu-id="7c5f8-111">本教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-111">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7c5f8-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="7c5f8-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="7c5f8-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7c5f8-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="7c5f8-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7c5f8-114">.NET 4.5</span></span>
> - <span data-ttu-id="7c5f8-115">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="7c5f8-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="7c5f8-116">本教學課程中使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7c5f8-116">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="7c5f8-117">若要使用 Visual Studio 2012，本教學課程中，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-117">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="7c5f8-118">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-118">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="7c5f8-119">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-119">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="7c5f8-120">在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-120">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="7c5f8-121">這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-121">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="7c5f8-122">有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-122">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="7c5f8-123">教學課程的版本</span><span class="sxs-lookup"><span data-stu-id="7c5f8-123">Tutorial Versions</span></span>
> 
> <span data-ttu-id="7c5f8-124">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-124">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="7c5f8-125">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="7c5f8-125">Questions and comments</span></span>
> 
> <span data-ttu-id="7c5f8-126">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-126">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7c5f8-127">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-127">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="7c5f8-128">總覽</span><span class="sxs-lookup"><span data-stu-id="7c5f8-128">Overview</span></span>

<span data-ttu-id="7c5f8-129">本教學課程會示範如何建立與其他瀏覽器即時分享物件狀態的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-129">This tutorial demonstrates how to create an application that shares the state of an object with other browsers in real time.</span></span> <span data-ttu-id="7c5f8-130">我們將建立的應用程式稱為 MoveShape。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-130">The application we'll create is called MoveShape.</span></span> <span data-ttu-id="7c5f8-131">MoveShape 頁面會顯示使用者可以拖曳; HTML Div 項目當使用者拖曳 Div，則其新位置將傳送到伺服器，然後就可以知道所有其他連線的用戶端更新以符合圖形的位置。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-131">The MoveShape page will display an HTML Div element that the user can drag; when the user drags the Div, its new position will be sent to the server, which will then tell all other connected clients to update the shape's position to match.</span></span>

![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

<span data-ttu-id="7c5f8-133">在本教學課程所建立的應用程式根據示範中，由 Damian Edwards 主講。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-133">The application created in this tutorial is based on a demo by Damian Edwards.</span></span> <span data-ttu-id="7c5f8-134">可以看到包含這段示範影片的影片[此處](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-134">A video containing this demo can be seen [here](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).</span></span>

<span data-ttu-id="7c5f8-135">本教學課程會示範如何從每個引發拖曳圖形的事件傳送 SignalR 訊息啟動。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-135">The tutorial will start by demonstrating how to send SignalR messages from each event that fires as the shape is dragged.</span></span> <span data-ttu-id="7c5f8-136">每個連線的用戶端會更新圖形的本機版本的位置收到訊息的每一次。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-136">Each connected client will then update the position of the local version of the shape each time a message is received.</span></span>

<span data-ttu-id="7c5f8-137">儘管應用程式將使用此方法來運作，這不是建議的程式設計模型，因為會開始傳送，因此用戶端和伺服器無法取得應付訊息，並會降低效能的訊息數目沒有上限.</span><span class="sxs-lookup"><span data-stu-id="7c5f8-137">While the application will function using this method, this is not a recommended programming model, since there would be no upper limit to the number of messages getting sent, so the clients and server could get overwhelmed with messages and performance would degrade.</span></span> <span data-ttu-id="7c5f8-138">脫離，圖形會立即移動每個方法，由，而不是移動順暢到每個新的位置，也會是在用戶端上顯示的動畫。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-138">The displayed animation on the client would also be disjointed, as the shape would be moved instantly by each method, rather than moving smoothly to each new location.</span></span> <span data-ttu-id="7c5f8-139">本教學課程後面幾節將示範如何建立計時器函式，將由用戶端或伺服器，傳送的訊息最大速率限制以及如何將圖案移動位置之間的順暢。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-139">Later sections of the tutorial will demonstrate how to create a timer function that restricts the maximum rate at which messages are sent by either the client or server, and how to move the shape smoothly between locations.</span></span> <span data-ttu-id="7c5f8-140">在本教學課程所建立的應用程式的最終版本可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-140">The final version of the application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="7c5f8-141">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-141">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="7c5f8-142">必要條件</span><span class="sxs-lookup"><span data-stu-id="7c5f8-142">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="7c5f8-143">建立專案並加入 SignalR 和 JQuery.UI NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="7c5f8-143">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>](#createtheproject2013)
- [<span data-ttu-id="7c5f8-144">建立基底的應用程式</span><span class="sxs-lookup"><span data-stu-id="7c5f8-144">Create the base application</span></span>](#baseapp)
- [<span data-ttu-id="7c5f8-145">應用程式啟動時啟動的中樞</span><span class="sxs-lookup"><span data-stu-id="7c5f8-145">Starting the hub when the application starts</span></span>](#startup2013)
- [<span data-ttu-id="7c5f8-146">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="7c5f8-146">Add the client loop</span></span>](#clientloop)
- [<span data-ttu-id="7c5f8-147">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="7c5f8-147">Add the server loop</span></span>](#serverloop)
- [<span data-ttu-id="7c5f8-148">用戶端上新增動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="7c5f8-148">Add smooth animation on the client</span></span>](#animation)
- [<span data-ttu-id="7c5f8-149">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7c5f8-149">Further Steps</span></span>](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="7c5f8-150">必要條件</span><span class="sxs-lookup"><span data-stu-id="7c5f8-150">Prerequisites</span></span>

<span data-ttu-id="7c5f8-151">本教學課程需要 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-151">This tutorial requires Visual Studio 2013.</span></span>

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a><span data-ttu-id="7c5f8-152">建立專案並加入 SignalR 和 JQuery.UI NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="7c5f8-152">Create the project and add the SignalR and JQuery.UI NuGet package</span></span>

<span data-ttu-id="7c5f8-153">在本節中，我們將建立 Visual Studio 2013 中的專案。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-153">In this section, we'll create the project in Visual Studio 2013.</span></span>

<span data-ttu-id="7c5f8-154">下列步驟使用 Visual Studio 2013 建立 ASP.NET 空白 Web 應用程式並將新增之 SignalR 和 jQuery.UI 程式庫：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-154">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR and jQuery.UI libraries:</span></span>

1. <span data-ttu-id="7c5f8-155">在 Visual Studio 建立 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-155">In Visual Studio create an ASP.NET Web Application.</span></span>

    ![建立 web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. <span data-ttu-id="7c5f8-157">在 **新增 ASP.NET 專案** 視窗中，保持**空白**選取，然後按一下 **建立專案**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-157">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![建立空白網站](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. <span data-ttu-id="7c5f8-159">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |SignalR Hub 類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-159">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="7c5f8-160">將類別命名為**MoveShapeHub.cs**並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-160">Name the class **MoveShapeHub.cs** and add it to the project.</span></span> <span data-ttu-id="7c5f8-161">這個步驟會建立**MoveShapeHub**類別，並將一組指令碼檔案和支援 SignalR 的組件參考加入至專案。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-161">This step creates the **MoveShapeHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c5f8-162">您也可以新增至專案 SignalR，依序按一下**工具 |程式庫套件管理員 |套件管理員主控台**並執行命令：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-162">You can also add SignalR to a project by clicking **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    <span data-ttu-id="7c5f8-163">`install-package Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="7c5f8-163">`install-package Microsoft.AspNet.SignalR`.</span></span> 

    <span data-ttu-id="7c5f8-164">如果您可以使用主控台來新增 SignalR，建立 SignalR hub 類別做為個別的步驟之後新增 SignalR。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>
4. <span data-ttu-id="7c5f8-165">按一下 **工具 |程式庫套件管理員 |套件管理員主控台**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-165">Click **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="7c5f8-166">在 [套件管理員] 視窗中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-166">In the package manager window, run the following command:</span></span>

    `Install-Package jQuery.UI.Combined`

    <span data-ttu-id="7c5f8-167">這會安裝 jQuery UI 程式庫，您將用來製作圖案動畫。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-167">This installs the jQuery UI library, which you'll use to animate the shape.</span></span>
5. <span data-ttu-id="7c5f8-168">在 **方案總管 中**展開指令碼 節點。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-168">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="7c5f8-169">適用於 jQuery、 jQueryUI 及 SignalR 的指令碼程式庫會顯示在專案中。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-169">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

    ![指令碼程式庫參考](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a><span data-ttu-id="7c5f8-171">建立基底的應用程式</span><span class="sxs-lookup"><span data-stu-id="7c5f8-171">Create the base application</span></span>

<span data-ttu-id="7c5f8-172">在本節中，我們將建立每個滑鼠移動事件期間，將形狀的位置傳送至伺服器的瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-172">In this section, we'll create a browser application that sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="7c5f8-173">伺服器然後廣播給所有其他連接的用戶端的這項資訊是在接收。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-173">The server then broadcasts this information to all other connected clients as it is received.</span></span> <span data-ttu-id="7c5f8-174">我們將探討在稍後的章節中的此應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-174">We'll expand on this application in later sections.</span></span>

1. <span data-ttu-id="7c5f8-175">如果您尚未建立 MoveShapeHub.cs 類別中**方案總管**，以滑鼠右鍵按一下專案，然後選取**新增**，**類別...**.將類別命名為**MoveShapeHub**然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-175">If you haven't already created the MoveShapeHub.cs class, in **Solution Explorer**, right-click on the project and select **Add**, **Class...**. Name the class **MoveShapeHub** and click **Add**.</span></span>
2. <span data-ttu-id="7c5f8-176">在新程式碼取代**MoveShapeHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-176">Replace the code in the new **MoveShapeHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="7c5f8-177">`MoveShapeHub`上述類別會實作 SignalR hub。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-177">The `MoveShapeHub` class above is an implementation of a SignalR hub.</span></span> <span data-ttu-id="7c5f8-178">依照[開始使用 SignalR](tutorial-getting-started-with-signalr.md)教學課程中，中樞都有在用戶端會直接呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-178">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients will call directly.</span></span> <span data-ttu-id="7c5f8-179">在此情況下，用戶端會傳送物件，其中包含新的伺服器，然後取得傳播到所有其他連線的用戶端 圖形的 X 和 Y 座標。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-179">In this case, the client will send an object containing the new X and Y coordinates of the shape to the server, which then gets broadcasted to all other connected clients.</span></span> <span data-ttu-id="7c5f8-180">SignalR 將自動序列化此物件，使用 JSON。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-180">SignalR will automatically serialize this object using JSON.</span></span>

    <span data-ttu-id="7c5f8-181">物件，將會傳送至用戶端 (`ShapeModel`) 包含要儲存圖形的位置的成員。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-181">The object that will be sent to the client (`ShapeModel`) contains members to store the position of the shape.</span></span> <span data-ttu-id="7c5f8-182">在伺服器上物件的版本也包含成員，才能追蹤用戶端的資料儲存，以便指定用戶端將不會傳送自己的資料。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-182">The version of the object on the server also contains a member to track which client's data is being stored, so that a given client won't be sent their own data.</span></span> <span data-ttu-id="7c5f8-183">這個成員會使用`JsonIgnore`以防止它被序列化並傳送至用戶端的屬性。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-183">This member uses the `JsonIgnore` attribute to keep it from being serialized and sent to the client.</span></span>

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a><span data-ttu-id="7c5f8-184">應用程式啟動時啟動的中樞</span><span class="sxs-lookup"><span data-stu-id="7c5f8-184">Starting the hub when the application starts</span></span>

1. <span data-ttu-id="7c5f8-185">接下來，我們會設定對應至中樞應用程式啟動時。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-185">Next, we'll set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="7c5f8-186">在 SignalR 2 中，這是藉由新增 OWIN 啟動類別，稱之為`MapSignalR`時的啟動類別`Configuration`OWIN 啟動時，執行方法。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-186">In SignalR 2, this is done by adding an OWIN startup class, which will call `MapSignalR` when the startup class' `Configuration` method is executed when OWIN starts.</span></span> <span data-ttu-id="7c5f8-187">OWIN 的啟動類別加入使用`OwinStartup`組件屬性。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-187">The class is added to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

    <span data-ttu-id="7c5f8-188">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |OWIN 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-188">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="7c5f8-189">將類別命名為*啟始*，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-189">Name the class *Startup* and click **OK**.</span></span>
2. <span data-ttu-id="7c5f8-190">變更 Startup.cs 的內容所示：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-190">Change the contents of Startup.cs to the following:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a><span data-ttu-id="7c5f8-191">新增用戶端</span><span class="sxs-lookup"><span data-stu-id="7c5f8-191">Adding the client</span></span>

1. <span data-ttu-id="7c5f8-192">接下來，我們將新增用戶端。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-192">Next, we'll add the client.</span></span> <span data-ttu-id="7c5f8-193">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |新的項目**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-193">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="7c5f8-194">在 **加入新項目**對話方塊中，選取**Html 網頁**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-194">In the **Add New Item** dialog, select **Html Page**.</span></span> <span data-ttu-id="7c5f8-195">將頁面命名**Default.html**然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-195">Name the page **Default.html** and click **Add**.</span></span>
2. <span data-ttu-id="7c5f8-196">在 **方案總管**，以滑鼠右鍵按一下您剛建立的網頁，然後按一下**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-196">In **Solution Explorer**, right-click the page you just created and click **Set as Start Page**.</span></span>
3. <span data-ttu-id="7c5f8-197">HTML 網頁中的預設程式碼取代為下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-197">Replace the default code in the HTML page with the following code snippet.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c5f8-198">確認指令碼參照以下相符項目新增至您的專案，在指令碼 資料夾中的封裝。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-198">Verify that the script references below match the packages added to your project in the Scripts folder.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    <span data-ttu-id="7c5f8-199">上述的 HTML 和 JavaScript 程式碼會建立紅色的 Div 稱為圖形、 啟用圖形的拖曳行為使用 jQuery 程式庫，並使用圖形的`drag`圖形的位置傳送至伺服器的事件。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-199">The above HTML and JavaScript code creates a red Div called Shape, enables the shape's dragging behavior using the jQuery library, and uses the shape's `drag` event to send the shape's position to the server.</span></span>
4. <span data-ttu-id="7c5f8-200">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-200">Start the application by pressing F5.</span></span> <span data-ttu-id="7c5f8-201">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-201">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="7c5f8-202">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-202">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a><span data-ttu-id="7c5f8-204">新增用戶端迴圈</span><span class="sxs-lookup"><span data-stu-id="7c5f8-204">Add the client loop</span></span>

<span data-ttu-id="7c5f8-205">由於傳送每一個滑鼠移動事件形狀的位置，會建立網路流量不需要數量，將訊息從用戶端就必須進行節流處理。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-205">Since sending the location of the shape on every mouse move event will create an unneccesary amount of network traffic, the messages from the client need to be throttled.</span></span> <span data-ttu-id="7c5f8-206">我們將使用 javascript`setInterval`函式來設定迴圈，將新的位置資訊傳送到伺服器以固定費率。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-206">We'll use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="7c5f8-207">這個迴圈是功能的 「 遊戲迴圈 」，重複呼叫的函式的磁碟機的所有遊戲或其他模擬非常基本表示法。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-207">This loop is a very basic representation of a "game loop", a repeatedly called function that drives all of the functionality of a game or other simulation.</span></span>

1. <span data-ttu-id="7c5f8-208">更新用戶端中的程式碼的 HTML 網頁，以符合下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-208">Update the client code in the HTML page to match the following code snippet.</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    <span data-ttu-id="7c5f8-209">上述的更新新增了`updateServerModel`函式，都會呼叫固定的頻率。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-209">The above update adds the `updateServerModel` function, which gets called on a fixed frequency.</span></span> <span data-ttu-id="7c5f8-210">此函式會將位置資料傳送到伺服器時`moved`旗標指出是要傳送的新位置資料。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-210">This function sends the position data to the server whenever the `moved` flag indicates that there is new position data to send.</span></span>
2. <span data-ttu-id="7c5f8-211">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-211">Start the application by pressing F5.</span></span> <span data-ttu-id="7c5f8-212">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-212">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="7c5f8-213">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-213">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="7c5f8-214">取得傳送到伺服器的訊息數目會受到節流控制，因為動畫不會顯示為 smooth 上一節。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-214">Since the number of messages that get sent to the server will be throttled, the animation will not appear as smooth as in the previous section.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a><span data-ttu-id="7c5f8-216">新增伺服器迴圈</span><span class="sxs-lookup"><span data-stu-id="7c5f8-216">Add the server loop</span></span>

<span data-ttu-id="7c5f8-217">在目前的應用程式，從伺服器傳送至用戶端的訊息移通常會將收到。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-217">In the current application, messages sent from the server to the client go out as often as they are received.</span></span> <span data-ttu-id="7c5f8-218">這會呈現類似的問題，因為出現在用戶端中;訊息可以傳送頻率比所需之連接如此一來可能會淹沒。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-218">This presents a similar problem as was seen on the client; messages can be sent more often than they are needed, and the connection could become flooded as a result.</span></span> <span data-ttu-id="7c5f8-219">本節說明如何更新伺服器實作的計時器，針對外寄訊息的速率進行節流。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-219">This section describes how to update the server to implement a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="7c5f8-220">內容取代`MoveShapeHub.cs`取代為下列程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-220">Replace the contents of `MoveShapeHub.cs` with the following code snippet.</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    <span data-ttu-id="7c5f8-221">上述程式碼會展開要新增的用戶端`Broadcaster`類別，節流處理外寄訊息使用`Timer`從.NET framework 類別。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-221">The above code expands the client to add the `Broadcaster` class, which throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

    <span data-ttu-id="7c5f8-222">由於中樞本身是暫時性 （它會建立每次需要它），`Broadcaster`會建立為單一值。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-222">Since the hub itself is transitory (it is created every time it is needed), the `Broadcaster` will be created as a singleton.</span></span> <span data-ttu-id="7c5f8-223">延遲初始設定 （.NET 4 中引入） 用來延遲其建立，直到有需要確保計時器啟動之前，完全建立第一個 「 中樞 」 執行個體。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-223">Lazy initialization (introduced in .NET 4) is used to defer its creation until it is needed, ensuring that the first hub instance is completely created before the timer is started.</span></span>

    <span data-ttu-id="7c5f8-224">用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法，所以它不再接收內送訊息時，立即呼叫。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-224">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method, so that it is no longer called immediately whenever incoming messages are received.</span></span> <span data-ttu-id="7c5f8-225">相反地，您會將用戶端訊息傳送速率為每秒 25 呼叫受`_broadcastLoop`從計時器`Broadcaster`類別。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-225">Instead, the messages to the clients will be sent at a rate of 25 calls per second, managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

    <span data-ttu-id="7c5f8-226">最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的中樞的參考 (`_hubContext`) 使用`GlobalHost`。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-226">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to obtain a reference to the currently operating hub (`_hubContext`) using the `GlobalHost`.</span></span>
2. <span data-ttu-id="7c5f8-227">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-227">Start the application by pressing F5.</span></span> <span data-ttu-id="7c5f8-228">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-228">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="7c5f8-229">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-229">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="7c5f8-230">不會從上一節中，瀏覽器中的可見差異，但會進行節流處理至用戶端傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-230">There will not be a visible difference in the browser from the previous section, but the number of messages that get sent to the client will be throttled.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a><span data-ttu-id="7c5f8-232">用戶端上新增動畫更為順暢</span><span class="sxs-lookup"><span data-stu-id="7c5f8-232">Add smooth animation on the client</span></span>

<span data-ttu-id="7c5f8-233">應用程式幾乎已完成，但我們無法讓一個更多改進，圖形用戶端上的移動中移動以回應伺服器的訊息時。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-233">The application is almost complete, but we could make one more improvement, in the motion of the shape on the client as it is moved in response to server messages.</span></span> <span data-ttu-id="7c5f8-234">而不是給定伺服器的新位置來設定圖案的位置，我們會使用 JQuery UI 程式庫的`animate`順暢移動圖形，其目前的和新的位置之間的函式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-234">Rather than setting the position of the shape to the new location given by the server, we'll use the JQuery UI library's `animate` function to move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="7c5f8-235">更新用戶端`updateShape`方法看起來像下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-235">Update the client's `updateShape` method to look like the highlighted code below:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    <span data-ttu-id="7c5f8-236">上述程式碼會將圖形從舊位置移到新伺服器所指定的期間 （在本例中為 100 毫秒） 的動畫間隔。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-236">The above code moves the shape from the old location to the new one given by the server over the course of the animation interval (in this case, 100 milliseconds).</span></span> <span data-ttu-id="7c5f8-237">新動畫開始之前，會清除任何在圖形上執行的上一個動畫。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-237">Any previous animation running on the shape is cleared before the new animation starts.</span></span>
2. <span data-ttu-id="7c5f8-238">按 f5 鍵啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-238">Start the application by pressing F5.</span></span> <span data-ttu-id="7c5f8-239">複製頁面的 URL，並將它貼到第二個瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-239">Copy the page's URL, and paste it into a second browser window.</span></span> <span data-ttu-id="7c5f8-240">將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-240">Drag the shape in one of the browser windows; the shape in the other browser window should move.</span></span> <span data-ttu-id="7c5f8-241">移動的另一個視窗中的圖形應該會出現較不為移轉的時間，而不是一次設定每個內送訊息會以內插值取代其移動不穩定。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-241">The movement of the shape in the other window should appear less jerky as its movement is interpolated over time rather than being set once per incoming message.</span></span>

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a><span data-ttu-id="7c5f8-243">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7c5f8-243">Further Steps</span></span>

<span data-ttu-id="7c5f8-244">在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將用戶端與伺服器之間的頻率高的訊息傳送方式。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-244">In this tutorial, you've learned how to program a SignalR application that sends high-frequency messages between clients and servers.</span></span> <span data-ttu-id="7c5f8-245">此通訊架構可用於開發線上遊戲，以及其他的模擬，例如[ShootR 遊戲建立與 SignalR](https://shootr.azurewebsites.net/)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-245">This communication paradigm is useful for developing online games and other simulations, such as [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="7c5f8-246">在本教學課程中建立完整的應用程式可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-246">The complete application created in this tutorial can be downloaded from [Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).</span></span>

<span data-ttu-id="7c5f8-247">若要深入了解 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="7c5f8-247">To learn more about SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="7c5f8-248">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="7c5f8-248">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="7c5f8-249">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="7c5f8-249">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="7c5f8-250">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7c5f8-250">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="7c5f8-251">如需如何部署至 Azure 的 SignalR 應用程式的逐步解說，請參閱 <<c0> [ 使用 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-251">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="7c5f8-252">如需如何將 Visual Studio web 專案部署至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="7c5f8-252">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
