---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: "教學課程： 開始使用 SignalR 2 |Microsoft 文件"
author: pfletcher
description: "此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 您會加入空白的 ASP.NET web 應用程式中的 SignalR 並建立 HTML pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3bec32b9b21325cde461541d7a313f401a0cfce7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="5d3eb-104">教學課程： 開始使用 SignalR 2</span><span class="sxs-lookup"><span data-stu-id="5d3eb-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="5d3eb-105">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5d3eb-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="5d3eb-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="5d3eb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="5d3eb-107">此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="5d3eb-108">將 SignalR 加入空的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5d3eb-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="5d3eb-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="5d3eb-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5d3eb-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5d3eb-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5d3eb-111">.NET 4.5</span></span>
> - <span data-ttu-id="5d3eb-112">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="5d3eb-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="5d3eb-113">使用 Visual Studio 2012 進行這個教學課程</span><span class="sxs-lookup"><span data-stu-id="5d3eb-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="5d3eb-114">透過本教學課程中使用 Visual Studio 2012，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="5d3eb-115">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="5d3eb-116">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="5d3eb-117">在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="5d3eb-118">這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="5d3eb-119">某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="5d3eb-120">教學課程版本</span><span class="sxs-lookup"><span data-stu-id="5d3eb-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="5d3eb-121">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5d3eb-122">問題和註解</span><span class="sxs-lookup"><span data-stu-id="5d3eb-122">Questions and comments</span></span>
> 
> <span data-ttu-id="5d3eb-123">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5d3eb-124">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="5d3eb-125">概觀</span><span class="sxs-lookup"><span data-stu-id="5d3eb-125">Overview</span></span>

<span data-ttu-id="5d3eb-126">此教學課程介紹 SignalR 開發，以顯示如何建立簡單的瀏覽器為基礎的交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="5d3eb-127">您會加入空的 ASP.NET web 應用程式中的 SignalR 程式庫、 建立中樞類別將訊息傳送至用戶端，並建立 HTML 網頁，可讓使用者傳送及接收交談的訊息。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="5d3eb-128">示範如何在 MVC 5 中建立交談應用程式使用 MVC 檢視的類似教學課程，請參閱[SignalR 2 和 MVC 5 入門](tutorial-getting-started-with-signalr-and-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5d3eb-129">本教學課程會示範如何建立在第 2 版中的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="5d3eb-130">SignalR 之間變更的詳細資訊的 1.x 和 2，請參閱[升級 SignalR 1.x 專案](../releases/upgrading-signalr-1x-projects-to-20.md)和[Visual Studio 2013 版本資訊](../../../visual-studio/overview/2013/release-notes.md#TOC13)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="5d3eb-131">SignalR 是開放原始碼.NET 程式庫建置 web 應用程式需要即時使用者互動或即時資料更新。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="5d3eb-132">範例包括社交應用程式、 多使用者遊戲、 企業共同作業及新聞、 天氣或財務更新應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="5d3eb-133">這些通常稱為即時應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-133">These are often called real-time applications.</span></span>

<span data-ttu-id="5d3eb-134">SignalR 簡化建立即時應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="5d3eb-135">它包含 ASP.NET server 程式庫和 JavaScript 用戶端程式庫，讓您更輕鬆地管理用戶端伺服器連線，並將內容更新推送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="5d3eb-136">您可以將 SignalR 程式庫加入現有的 ASP.NET 應用程式即可取得即時的功能。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="5d3eb-137">本教學課程將示範下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="5d3eb-138">將 SignalR 程式庫加入至 ASP.NET web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="5d3eb-139">建立內容推播至用戶端中樞類別。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="5d3eb-140">建立 OWIN 啟動類別將應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="5d3eb-141">使用 SignalR jQuery 程式庫，在網頁中的傳送訊息，並顯示從中樞的更新。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="5d3eb-142">下列螢幕擷取畫面顯示瀏覽器中執行之交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="5d3eb-143">每個新的使用者可以張貼評論，並查看使用者加入這個聊天室之後新增的註解。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![交談的執行個體](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="5d3eb-145">章節：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-145">Sections:</span></span>

- [<span data-ttu-id="5d3eb-146">設定專案</span><span class="sxs-lookup"><span data-stu-id="5d3eb-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="5d3eb-147">執行範例</span><span class="sxs-lookup"><span data-stu-id="5d3eb-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="5d3eb-148">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="5d3eb-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="5d3eb-149">接下來的步驟</span><span class="sxs-lookup"><span data-stu-id="5d3eb-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="5d3eb-150">設定專案</span><span class="sxs-lookup"><span data-stu-id="5d3eb-150">Set up the Project</span></span>

<span data-ttu-id="5d3eb-151">本節顯示如何建立空的 ASP.NET web 應用程式，使用 Visual Studio 2013 和 SignalR 第 2 版新增 SignalR，和建立交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="5d3eb-152">必要條件：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-152">Prerequisites:</span></span>

- <span data-ttu-id="5d3eb-153">Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-153">Visual Studio 2013.</span></span> <span data-ttu-id="5d3eb-154">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2013 Express 開發的工具。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="5d3eb-155">下列步驟使用 Visual Studio 2013 建立的 ASP.NET 空 Web 應用程式，並新增 SignalR library:</span><span class="sxs-lookup"><span data-stu-id="5d3eb-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="5d3eb-156">在 Visual Studio 中建立 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![建立 web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="5d3eb-158">在**新增 ASP.NET 專案** 視窗中，保留**空**選取並按**建立專案**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![建立空的網站](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="5d3eb-160">在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |SignalR 中樞類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="5d3eb-161">將類別**ChatHub.cs**並將它加入至專案。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="5d3eb-162">這個步驟會建立**ChatHub**類別，並將一組指令碼檔案和支援 SignalR 的組件參考加入至專案。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d3eb-163">您也可以加入至專案 SignalR，藉由開啟**工具 |程式庫套件管理員 |Package Manager Console**並執行命令：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="5d3eb-164">如果您要加入 SignalR 使用主控台，建立 SignalR 中樞類別當做個別的步驟之後新增 SignalR。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d3eb-165">如果您使用 Visual Studio 2012 **SignalR 中樞類別 (v2)**範本將無法使用。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="5d3eb-166">您可以加入純**類別**呼叫`ChatHub`改為。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="5d3eb-167">在**方案總管 中**，展開 指令碼 節點。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="5d3eb-168">JQuery 和 SignalR 的指令碼程式庫會顯示專案中。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="5d3eb-169">在新的程式碼取代**ChatHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="5d3eb-170">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |OWIN 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="5d3eb-171">將新類別`Startup`按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d3eb-172">如果您使用 Visual Studio 2012 **OWIN 啟動類別**範本將無法使用。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="5d3eb-173">您可以加入純**類別**呼叫`Startup`改為。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="5d3eb-174">下列變更新啟動類別的內容。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="5d3eb-175">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |HTML 網頁**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="5d3eb-176">將新頁面`index.html`。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="5d3eb-177">您可能需要變更版本號碼的 JQuery 和 SignalR 的程式庫參考</span><span class="sxs-lookup"><span data-stu-id="5d3eb-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="5d3eb-178">在**方案總管 中**，以滑鼠右鍵按一下您剛建立的 HTML 網頁，然後按一下**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="5d3eb-179">HTML 網頁中的預設程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d3eb-180">封裝管理員可能會安裝較新版的 SignalR 指令碼。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="5d3eb-181">請確認下面指令碼參考對應至版本的指令碼專案中的檔案 （它們將位於不同，如果您加入 SignalR 使用 NuGet 而非新增集線器。）</span><span class="sxs-lookup"><span data-stu-id="5d3eb-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="5d3eb-182">**儲存所有**專案。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="5d3eb-183">執行範例</span><span class="sxs-lookup"><span data-stu-id="5d3eb-183">Run the Sample</span></span>

1. <span data-ttu-id="5d3eb-184">按 F5 以偵錯模式執行專案。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="5d3eb-185">HTML 頁面載入瀏覽器執行個體，並提示輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="5d3eb-187">輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-187">Enter a user name.</span></span>
3. <span data-ttu-id="5d3eb-188">從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="5d3eb-189">在每個瀏覽器執行個體中，輸入唯一的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="5d3eb-190">每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="5d3eb-191">這些註解應該顯示在所有瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d3eb-192">這個簡單的聊天應用程式不會維持在伺服器上的討論內容。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="5d3eb-193">集線器會廣播到所有目前使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="5d3eb-194">稍後加入交談的使用者會看到訊息的時間加入它們加入。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="5d3eb-195">下列螢幕擷取畫面顯示三個瀏覽器執行個體，其中一個執行個體傳送訊息時就會更新中執行之交談應用程式：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![對談瀏覽器](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="5d3eb-197">在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="5d3eb-198">沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="5d3eb-199">這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="5d3eb-200">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="5d3eb-200">Examine the Code</span></span>

<span data-ttu-id="5d3eb-201">SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="5d3eb-202">SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="5d3eb-202">SignalR Hubs</span></span>

<span data-ttu-id="5d3eb-203">下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="5d3eb-204">衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="5d3eb-205">您可以建立中樞類別上的公用方法，然後呼叫在網頁中的指令碼的方式來存取這些方法。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="5d3eb-206">在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="5d3eb-207">中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.broadcastMessage**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="5d3eb-208">**傳送**方法將示範中樞的幾個概念：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="5d3eb-209">中樞上宣告的公用方法，可讓用戶端呼叫。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="5d3eb-210">使用**Microsoft.AspNet.SignalR.Hub.Clients**動態屬性來存取所有用戶端連線到此中樞。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="5d3eb-211">在用戶端上呼叫的函式 (例如`broadcastMessage`函式) 來更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="5d3eb-212">SignalR 和 jQuery</span><span class="sxs-lookup"><span data-stu-id="5d3eb-212">SignalR and jQuery</span></span>

<span data-ttu-id="5d3eb-213">HTML 網頁中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="5d3eb-214">在程式碼中的重要工作宣告參考宣告的函式，伺服器可以呼叫用戶端推入內容和啟動將訊息傳送至中樞連線的中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="5d3eb-215">下列程式碼會宣告中樞 proxy 的參考。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="5d3eb-216">在 JavaScript 中在 camel 命名法的情況下會伺服器類別和其成員的參考。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="5d3eb-217">程式碼範例會參考 C# **ChatHub**類別在 JavaScript 中為**chatHub**。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="5d3eb-218">下列程式碼是如何在指令碼中建立的回呼函式。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="5d3eb-219">在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="5d3eb-220">HTML 編碼內容，再顯示它兩行是選擇性的並顯示簡單的方式，以避免指令碼資料隱碼。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="5d3eb-221">下列程式碼會示範如何開啟與中樞的連線。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="5d3eb-222">此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**的 HTML 網頁上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="5d3eb-223">這個方法可確保此事件處理常式執行之前，已建立連線。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="5d3eb-224">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5d3eb-224">Next Steps</span></span>

<span data-ttu-id="5d3eb-225">您已學習 SignalR 是建置即時 web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="5d3eb-226">您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="5d3eb-227">如需如何部署範例 SignalR 應用程式至 Azure 的逐步解說，請參閱[與 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="5d3eb-228">如需如何部署 Visual Studio web 專案至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="5d3eb-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="5d3eb-229">深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="5d3eb-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="5d3eb-230">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="5d3eb-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="5d3eb-231">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="5d3eb-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="5d3eb-232">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="5d3eb-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
