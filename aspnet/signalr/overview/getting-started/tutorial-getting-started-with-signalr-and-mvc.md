---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 教學課程： 開始使用 SignalR 2 和 MVC 5 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天應用程式。 您將加入 MVC 5 應用程式中的 SignalR 和建立交談檢視...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 903319040c9ac938cea5dce2e6579d88e0d80bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384047"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="7a5f6-104">教學課程： 開始使用 SignalR 2 和 MVC 5</span><span class="sxs-lookup"><span data-stu-id="7a5f6-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="7a5f6-105">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="7a5f6-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="7a5f6-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="7a5f6-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="7a5f6-107">本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="7a5f6-108">您會加入 MVC 5 應用程式中的 SignalR 和建立交談檢視，以傳送，並顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7a5f6-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="7a5f6-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="7a5f6-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7a5f6-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="7a5f6-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7a5f6-111">.NET 4.5</span></span>
> - <span data-ttu-id="7a5f6-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="7a5f6-112">MVC 5</span></span>
> - <span data-ttu-id="7a5f6-113">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="7a5f6-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="7a5f6-114">本教學課程中使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7a5f6-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="7a5f6-115">若要使用 Visual Studio 2012，本教學課程中，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="7a5f6-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="7a5f6-116">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="7a5f6-117">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="7a5f6-118">在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="7a5f6-119">這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="7a5f6-120">有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="7a5f6-121">教學課程的版本</span><span class="sxs-lookup"><span data-stu-id="7a5f6-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="7a5f6-122">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="7a5f6-123">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="7a5f6-123">Questions and comments</span></span>
> 
> <span data-ttu-id="7a5f6-124">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7a5f6-125">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="7a5f6-126">總覽</span><span class="sxs-lookup"><span data-stu-id="7a5f6-126">Overview</span></span>

<span data-ttu-id="7a5f6-127">本教學課程會向您介紹 ASP.NET SignalR 2 和 ASP.NET MVC 5 的即時 web 應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="7a5f6-128">本教學課程會使用相同的交談應用程式程式碼作為[SignalR 開始使用教學課程](tutorial-getting-started-with-signalr.md)，但會顯示如何將它新增至 MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="7a5f6-129">在本主題中，您將學習下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="7a5f6-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="7a5f6-130">加入 MVC 5 應用程式中的 SignalR 程式庫。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="7a5f6-131">建立中樞和 OWIN 啟動類別，以將內容推送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="7a5f6-132">使用 SignalR jQuery 程式庫在網頁上傳送訊息，並顯示從中樞的更新。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="7a5f6-133">下列螢幕擷取畫面顯示在瀏覽器中執行之已完成的交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![交談執行個體](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="7a5f6-135">章節：</span><span class="sxs-lookup"><span data-stu-id="7a5f6-135">Sections:</span></span>

- [<span data-ttu-id="7a5f6-136">設定專案</span><span class="sxs-lookup"><span data-stu-id="7a5f6-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="7a5f6-137">執行範例</span><span class="sxs-lookup"><span data-stu-id="7a5f6-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="7a5f6-138">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="7a5f6-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="7a5f6-139">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7a5f6-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="7a5f6-140">設定專案</span><span class="sxs-lookup"><span data-stu-id="7a5f6-140">Set up the Project</span></span>

<span data-ttu-id="7a5f6-141">必要條件：</span><span class="sxs-lookup"><span data-stu-id="7a5f6-141">Prerequisites:</span></span>

- <span data-ttu-id="7a5f6-142">Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-142">Visual Studio 2013.</span></span> <span data-ttu-id="7a5f6-143">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2013 Express 開發工具。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="7a5f6-144">本節說明如何建立 ASP.NET MVC 5 應用程式、 新增 SignalR 程式庫和建立交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="7a5f6-145">在 Visual Studio 中建立.NET Framework 4.5 為目標的 C# ASP.NET 應用程式、 SignalRChat，將它命名，並按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![建立 web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="7a5f6-147">在 [ `New ASP.NET Project` ] 對話方塊中，然後選取**MVC**，然後按一下**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![建立 web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="7a5f6-149">選取 **不需要驗證**中**變更驗證**對話方塊，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![選取 不驗證](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="7a5f6-151">如果您為您的應用程式中，選取不同的驗證提供者`Startup.cs`類別會為您建立; 您不需要建立您自己`Startup.cs`步驟 10 以下的類別。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="7a5f6-152">按一下  **確定**中**新增 ASP.NET 專案**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="7a5f6-153">開啟**工具 |程式庫套件管理員 |套件管理員主控台**並執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="7a5f6-154">此步驟可將一組指令碼檔案和啟用 SignalR 功能的組件參考加入至專案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="7a5f6-155">在 [**方案總管] 中**，展開 [指令碼] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="7a5f6-156">請注意 SignalR 的指令碼程式庫已加入至專案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![指令碼 資料夾](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="7a5f6-158">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |新的資料夾**，並新增名為的新資料夾**中樞**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="7a5f6-159">以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |新的項目**，選取**Visual C# |Web |SignalR**中的節點**已安裝**窗格中，選取**SignalR Hub 類別 (v2)** 從中間窗格中，並建立名為的新中樞**ChatHub.cs**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="7a5f6-160">您將使用這個類別做為 SignalR 伺服器中樞將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![建立新的中樞](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="7a5f6-162">中的程式碼取代**ChatHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="7a5f6-163">建立新的類別，稱為 Startup.cs。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="7a5f6-164">變更檔案的內容所示。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="7a5f6-165">編輯`HomeController`類別中找到**controllers/Homecontroller.cs**並將下列方法新增至類別。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="7a5f6-166">這個方法會傳回**聊天**您將在稍後的步驟建立的檢視。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="7a5f6-167">以滑鼠右鍵按一下**Views/Home**資料夾，然後選取**新增。...|檢視**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="7a5f6-168">在 **加入檢視**對話方塊中，命名新的檢視**聊天**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![新增檢視](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="7a5f6-170">內容取代**Chat.cshtml**為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7a5f6-171">當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，套件管理員可能會安裝的版本比本主題中所顯示的版本還新的 SignalR 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="7a5f6-172">請確定您的程式碼中的指令碼參考符合安裝在您專案的指令碼程式庫的版本。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="7a5f6-173">**全部儲存**專案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="7a5f6-174">執行範例</span><span class="sxs-lookup"><span data-stu-id="7a5f6-174">Run the Sample</span></span>

1. <span data-ttu-id="7a5f6-175">按 F5 以偵錯模式中執行專案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="7a5f6-176">在瀏覽器網址列中，附加 **/home/聊天**專案的預設頁面的 url。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="7a5f6-177">聊天室頁面載入瀏覽器執行個體，並提示輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="7a5f6-179">輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-179">Enter a user name.</span></span>
4. <span data-ttu-id="7a5f6-180">從瀏覽器的網址列複製 URL，並使用它來開啟兩個更多的瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="7a5f6-181">在每個瀏覽器執行個體中，輸入唯一的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="7a5f6-182">在每個瀏覽器執行個體中，新增註解，然後按一下**傳送**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="7a5f6-183">註解應該會顯示在瀏覽器的所有執行個體。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7a5f6-184">這個簡單的聊天應用程式不會維護伺服器上的討論內容。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="7a5f6-185">中樞會廣播到所有目前使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="7a5f6-186">稍後加入聊天室使用者會看到訊息的時間加入它們加入。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="7a5f6-187">下列螢幕擷取畫面顯示在瀏覽器中執行的聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![對談的瀏覽器](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="7a5f6-189">在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="7a5f6-190">如果您使用 Internet Explorer 作為您的瀏覽器，此節點會顯示在 偵錯模式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="7a5f6-191">沒有名為的指令碼檔案**中樞**SignalR 程式庫以動態方式產生在執行階段。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="7a5f6-192">此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="7a5f6-193">如果您使用非 Internet Explorer 的瀏覽器，您也可以存取動態**集線器**瀏覽至它直接，例如檔案http://mywebsite/signalr/hubs。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="7a5f6-194">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="7a5f6-194">Examine the Code</span></span>

<span data-ttu-id="7a5f6-195">SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 建立中樞做為主要協調物件在伺服器上，並使用 SignalR jQuery 程式庫來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="7a5f6-196">SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="7a5f6-196">SignalR Hubs</span></span>

<span data-ttu-id="7a5f6-197">在程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="7a5f6-198">衍生自**中樞**類別是實用的方式，來建置 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="7a5f6-199">您可以建立中樞類別上的公用方法，並從網頁中的指令碼的方式呼叫它們，以存取這些方法。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="7a5f6-200">在對談程式碼中，用戶端呼叫**ChatHub.Send**方法以傳送新訊息。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="7a5f6-201">中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.addNewMessageToPage**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="7a5f6-202">**傳送**方法將示範數個中樞概念：</span><span class="sxs-lookup"><span data-stu-id="7a5f6-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="7a5f6-203">可讓用戶端呼叫，則請在中樞中宣告的公用方法。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="7a5f6-204">使用**Microsoft.AspNet.SignalR.Hub.Clients**屬性來存取所有用戶端連線到此集線器。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="7a5f6-205">在用戶端上呼叫的函式 (例如`addNewMessageToPage`函式) 來更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="7a5f6-206">SignalR 和 jQuery</span><span class="sxs-lookup"><span data-stu-id="7a5f6-206">SignalR and jQuery</span></span>

<span data-ttu-id="7a5f6-207">**Chat.cshtml**檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="7a5f6-208">在程式碼中的重要工作建立自動產生之 proxy 的中樞中，宣告的函式推播內容給用戶端，可以呼叫的伺服器和啟動連線將訊息傳送至中樞的參考。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="7a5f6-209">下列程式碼會宣告將中樞 proxy 的參考。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="7a5f6-210">在 JavaScript 伺服器類別和其成員的參考是駝峰式大小寫。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="7a5f6-211">程式碼範例會參考 C# **ChatHub**類別，以做為 JavaScript **chatHub**。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="7a5f6-212">如果您想要參考`ChatHub`類別在 jQuery 中使用傳統的 pascal 命名法大小寫，如同在 C# 中，編輯 ChatHub.cs 類別檔案。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="7a5f6-213">新增`using`陳述式來參考`Microsoft.AspNet.SignalR.Hubs`命名空間。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="7a5f6-214">然後新增`HubName`屬性設定為`ChatHub`類別，例如`[HubName("ChatHub")]`。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="7a5f6-215">最後，更新您的 jQuery 參考`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="7a5f6-216">下列程式碼示範如何建立指令碼中的回呼函式。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="7a5f6-217">在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="7a5f6-218">選擇性呼叫`htmlEncode`函式會顯示為 HTML 的辦法之前先顯示在頁面中，做為防止指令碼資料隱碼攻擊的方式編碼的訊息內容。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="7a5f6-219">下列程式碼示範如何使用中樞開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="7a5f6-220">程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**[聊天室] 頁面中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="7a5f6-221">這個方法可確保事件處理常式執行之前，會建立連接。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="7a5f6-222">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7a5f6-222">Next Steps</span></span>

<span data-ttu-id="7a5f6-223">您已了解 SignalR 是用來建置即時 web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="7a5f6-224">您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="7a5f6-225">如需如何部署 Azure 範例 SignalR 應用程式的逐步解說，請參閱 <<c0> [ 使用 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="7a5f6-226">如需如何將 Visual Studio web 專案部署至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="7a5f6-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="7a5f6-227">若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="7a5f6-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="7a5f6-228">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="7a5f6-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="7a5f6-229">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="7a5f6-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="7a5f6-230">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7a5f6-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
