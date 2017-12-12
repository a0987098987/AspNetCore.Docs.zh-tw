---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: "教學課程： 開始使用 SignalR 1.x 和 MVC 4 |Microsoft 文件"
author: pfletcher
description: "使用 ASP.NET SignalR 和 ASP.NET MVC 4 建置即時聊天的應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: e678c85520613fea2a8d00de60aca04d895d6307
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="7878a-103">教學課程： 開始使用 SignalR 1.x 和 MVC 4</span><span class="sxs-lookup"><span data-stu-id="7878a-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="7878a-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="7878a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="7878a-105">本教學課程會示範如何使用 ASP.NET SignalR 建立即時聊天的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="7878a-106">將 SignalR 加入至 MVC 4 應用程式，並建立聊天檢視來傳送，並顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="7878a-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="7878a-107">概觀</span><span class="sxs-lookup"><span data-stu-id="7878a-107">Overview</span></span>

<span data-ttu-id="7878a-108">本教學課程為您介紹 ASP.NET SignalR 和 ASP.NET MVC 4 的即時 web 應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="7878a-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="7878a-109">教學課程使用相同的交談應用程式程式碼做為[SignalR 快速入門教學課程](tutorial-getting-started-with-signalr.md)，但示範如何將它加入至 MVC 4 應用程式以網際網路為範本。</span><span class="sxs-lookup"><span data-stu-id="7878a-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="7878a-110">本主題中，您將學習下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="7878a-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="7878a-111">將 SignalR 程式庫加入至 MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="7878a-112">建立內容推播至用戶端中樞類別。</span><span class="sxs-lookup"><span data-stu-id="7878a-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="7878a-113">使用 SignalR jQuery 程式庫，在網頁中的傳送訊息，並顯示從中樞的更新。</span><span class="sxs-lookup"><span data-stu-id="7878a-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="7878a-114">下列螢幕擷取畫面顯示瀏覽器中執行之已完成的交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![交談的執行個體](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="7878a-116">章節：</span><span class="sxs-lookup"><span data-stu-id="7878a-116">Sections:</span></span>

- [<span data-ttu-id="7878a-117">設定專案</span><span class="sxs-lookup"><span data-stu-id="7878a-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="7878a-118">執行範例</span><span class="sxs-lookup"><span data-stu-id="7878a-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="7878a-119">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="7878a-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="7878a-120">接下來的步驟</span><span class="sxs-lookup"><span data-stu-id="7878a-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="7878a-121">設定專案</span><span class="sxs-lookup"><span data-stu-id="7878a-121">Set up the Project</span></span>

<span data-ttu-id="7878a-122">必要條件：</span><span class="sxs-lookup"><span data-stu-id="7878a-122">Prerequisites:</span></span>

- <span data-ttu-id="7878a-123">Visual Studio 2010 SP1、 Visual Studio 2012 或 Visual Studio 2012 Express。</span><span class="sxs-lookup"><span data-stu-id="7878a-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="7878a-124">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發的工具。</span><span class="sxs-lookup"><span data-stu-id="7878a-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="7878a-125">針對 Visual Studio 2010、 安裝[ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683)。</span><span class="sxs-lookup"><span data-stu-id="7878a-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="7878a-126">本節說明如何建立 ASP.NET MVC 4 應用程式、 新增 SignalR 程式庫和建立交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="7878a-127">Visual Studio 中建立 ASP.NET MVC 4 應用程式、 其命名為 SignalRChat，並按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="7878a-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="7878a-128">在 VS 2010 中，選取**.NET Framework 4** Framework 版本下拉式控制項中。</span><span class="sxs-lookup"><span data-stu-id="7878a-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="7878a-129">在.NET Framework 4 和 4.5 版上執行 SignalR 程式碼。</span><span class="sxs-lookup"><span data-stu-id="7878a-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![建立 mvc web](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
    2. <span data-ttu-id="7878a-131">選取 網際網路應用程式範本，請清除選項以**建立單元測試專案**，按一下 確定。</span><span class="sxs-lookup"><span data-stu-id="7878a-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

        ![建立 mvc 網際網路站台](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
    3. <span data-ttu-id="7878a-133">開啟**工具 |程式庫套件管理員 |Package Manager Console** ，然後執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="7878a-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="7878a-134">這個步驟可將一組指令碼檔案和啟用 SignalR 功能的組件參考加入至專案。</span><span class="sxs-lookup"><span data-stu-id="7878a-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

        `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
    4. <span data-ttu-id="7878a-135">在**方案總管 中**展開指令碼 資料夾。</span><span class="sxs-lookup"><span data-stu-id="7878a-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="7878a-136">請注意 SignalR 的指令碼程式庫已加入至專案。</span><span class="sxs-lookup"><span data-stu-id="7878a-136">Note that script libraries for SignalR have been added to the project.</span></span>

        ![程式庫參考](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
    5. <span data-ttu-id="7878a-138">在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |新的資料夾**，並加入新的資料夾，名為**集線器**。</span><span class="sxs-lookup"><span data-stu-id="7878a-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
    6. <span data-ttu-id="7878a-139">以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |類別**，並建立新的 C# 類別名稱為**ChatHub.cs**。</span><span class="sxs-lookup"><span data-stu-id="7878a-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="7878a-140">您將使用此類別將訊息傳送至所有用戶端的 SignalR 伺服器集線器。</span><span class="sxs-lookup"><span data-stu-id="7878a-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="7878a-141">如果您使用 Visual Studio 2012，而且已安裝[ASP.NET 和 Web 工具 2012.2 更新](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)，您可以使用新的 SignalR 項目範本建立中樞類別。</span><span class="sxs-lookup"><span data-stu-id="7878a-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="7878a-142">若要這樣做，請以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |新項目**，選取**SignalR 中樞類別 (v1)**，並加以**ChatHub.cs**。</span><span class="sxs-lookup"><span data-stu-id="7878a-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="7878a-143">中的程式碼取代**ChatHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="7878a-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="7878a-144">開啟**Global.asax**檔的專案，並加入至方法的呼叫`RouteTable.Routes.MapHubs();`做為第一行中的程式碼`Application_Start`方法。</span><span class="sxs-lookup"><span data-stu-id="7878a-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="7878a-145">此程式碼會註冊 SignalR 中樞的預設路由，並必須在呼叫之前註冊的任何其他路由。</span><span class="sxs-lookup"><span data-stu-id="7878a-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="7878a-146">已完成`Application_Start`方法看起來類似下列的範例。</span><span class="sxs-lookup"><span data-stu-id="7878a-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="7878a-147">編輯`HomeController`類別中找到**Controllers/HomeController.cs**並將下列方法加入至類別。</span><span class="sxs-lookup"><span data-stu-id="7878a-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="7878a-148">這個方法會傳回**聊天**您將在稍後步驟中建立的檢視。</span><span class="sxs-lookup"><span data-stu-id="7878a-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="7878a-149">以滑鼠右鍵按一下內`Chat`方法剛剛建立，並按一下**加入檢視**建立新的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="7878a-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="7878a-150">在**加入檢視**] 對話方塊中，請確定已選取核取方塊**使用版面配置頁或主版頁面**（清除其他核取方塊），然後按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="7878a-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![新增檢視](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="7878a-152">編輯名為新的檢視檔案**Chat.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="7878a-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="7878a-153">之後&lt;h2&gt;標記中，貼上下列&lt;div&gt;區段和`@section scripts`加入頁面中的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="7878a-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="7878a-154">此指令碼可讓網頁來傳送訊息，並顯示從伺服器的訊息。</span><span class="sxs-lookup"><span data-stu-id="7878a-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="7878a-155">[聊天室] 檢視的完整程式碼會出現在下列程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="7878a-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7878a-156">當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，封裝管理員可能會安裝版本比本主題所顯示的版本還新的指令碼。</span><span class="sxs-lookup"><span data-stu-id="7878a-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="7878a-157">請確定您的程式碼中的指令碼參考符合安裝在您的專案中的指令碼程式庫的版本。</span><span class="sxs-lookup"><span data-stu-id="7878a-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="7878a-158">**儲存所有**專案。</span><span class="sxs-lookup"><span data-stu-id="7878a-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="7878a-159">執行範例</span><span class="sxs-lookup"><span data-stu-id="7878a-159">Run the Sample</span></span>

1. <span data-ttu-id="7878a-160">按 F5 以偵錯模式執行專案。</span><span class="sxs-lookup"><span data-stu-id="7878a-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="7878a-161">在瀏覽器位址列中，附加**/首頁/聊天**專案的預設網頁的 url。</span><span class="sxs-lookup"><span data-stu-id="7878a-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="7878a-162">聊天室頁面載入瀏覽器執行個體，並提示輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7878a-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="7878a-164">輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7878a-164">Enter a user name.</span></span>
4. <span data-ttu-id="7878a-165">從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="7878a-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="7878a-166">在每個瀏覽器執行個體中，輸入唯一的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="7878a-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="7878a-167">每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。</span><span class="sxs-lookup"><span data-stu-id="7878a-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="7878a-168">這些註解應該顯示在所有瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="7878a-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7878a-169">這個簡單的聊天應用程式不會維持在伺服器上的討論內容。</span><span class="sxs-lookup"><span data-stu-id="7878a-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="7878a-170">集線器會廣播到所有目前使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="7878a-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="7878a-171">稍後加入交談的使用者會看到訊息的時間加入它們加入。</span><span class="sxs-lookup"><span data-stu-id="7878a-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="7878a-172">下列螢幕擷取畫面顯示瀏覽器中執行之交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![對談瀏覽器](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="7878a-174">在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="7878a-175">如果您使用 Internet Explorer 瀏覽器，此節點會顯示在偵錯模式。</span><span class="sxs-lookup"><span data-stu-id="7878a-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="7878a-176">沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。</span><span class="sxs-lookup"><span data-stu-id="7878a-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="7878a-177">這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="7878a-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="7878a-178">如果您使用非 Internet Explorer 的瀏覽器，您也可以存取動態**集線器**瀏覽至它直接管理，例如 http://mywebsite/signalr/hubs 檔案。</span><span class="sxs-lookup"><span data-stu-id="7878a-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="7878a-180">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="7878a-180">Examine the Code</span></span>

<span data-ttu-id="7878a-181">SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="7878a-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="7878a-182">SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="7878a-182">SignalR Hubs</span></span>

<span data-ttu-id="7878a-183">下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。</span><span class="sxs-lookup"><span data-stu-id="7878a-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="7878a-184">衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7878a-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="7878a-185">您可以建立中樞類別上的公用方法，然後呼叫在網頁中的 jQuery 指令碼的方式來存取這些方法。</span><span class="sxs-lookup"><span data-stu-id="7878a-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="7878a-186">在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。</span><span class="sxs-lookup"><span data-stu-id="7878a-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="7878a-187">中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.addNewMessageToPage**。</span><span class="sxs-lookup"><span data-stu-id="7878a-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="7878a-188">**傳送**方法將示範中樞的幾個概念：</span><span class="sxs-lookup"><span data-stu-id="7878a-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="7878a-189">中樞上宣告的公用方法，可讓用戶端呼叫。</span><span class="sxs-lookup"><span data-stu-id="7878a-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="7878a-190">使用**Microsoft.AspNet.SignalR.Hub.Clients**屬性來存取所有用戶端連線到此中樞。</span><span class="sxs-lookup"><span data-stu-id="7878a-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="7878a-191">在用戶端上呼叫 jQuery 函式 (例如`addNewMessageToPage`函式) 來更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="7878a-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="7878a-192">SignalR 和 jQuery</span><span class="sxs-lookup"><span data-stu-id="7878a-192">SignalR and jQuery</span></span>

<span data-ttu-id="7878a-193">**Chat.cshtml**檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。</span><span class="sxs-lookup"><span data-stu-id="7878a-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="7878a-194">在程式碼中的重要工作建立的自動產生 proxy 集線器，宣告的函式推入內容給用戶端，可以呼叫的伺服器和啟動連線將訊息傳送至中樞的參考。</span><span class="sxs-lookup"><span data-stu-id="7878a-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="7878a-195">下列程式碼會宣告中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="7878a-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="7878a-196">在 jQuery 伺服器類別和其成員的參考是 camel 命名法的大小寫。</span><span class="sxs-lookup"><span data-stu-id="7878a-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="7878a-197">程式碼範例會參考 C# **ChatHub**類別中當做 jQuery **chatHub**。</span><span class="sxs-lookup"><span data-stu-id="7878a-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="7878a-198">如果您想要參考`ChatHub`類別在 jQuery 與傳統 Pascal 大小寫在 C# 中一樣編輯 ChatHub.cs 類別檔案。</span><span class="sxs-lookup"><span data-stu-id="7878a-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="7878a-199">新增`using`陳述式來參考`Microsoft.AspNet.SignalR.Hubs`命名空間。</span><span class="sxs-lookup"><span data-stu-id="7878a-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="7878a-200">然後加入`HubName`屬性`ChatHub`類別，例如`[HubName("ChatHub")]`。</span><span class="sxs-lookup"><span data-stu-id="7878a-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="7878a-201">最後，更新您 jQuery 參考`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="7878a-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="7878a-202">下列程式碼會示範如何在指令碼中建立的回呼函式。</span><span class="sxs-lookup"><span data-stu-id="7878a-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="7878a-203">在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。</span><span class="sxs-lookup"><span data-stu-id="7878a-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="7878a-204">選擇性呼叫`htmlEncode`函式便會顯示為 HTML 的方式之後，顯示在頁面中，以避免指令碼資料隱碼的方式編碼的訊息內容。</span><span class="sxs-lookup"><span data-stu-id="7878a-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="7878a-205">下列程式碼會示範如何開啟與中樞的連線。</span><span class="sxs-lookup"><span data-stu-id="7878a-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="7878a-206">此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**聊天室網頁中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="7878a-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="7878a-207">這個方法可確保此事件處理常式執行之前，已建立連線。</span><span class="sxs-lookup"><span data-stu-id="7878a-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="7878a-208">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7878a-208">Next Steps</span></span>

<span data-ttu-id="7878a-209">您已學習 SignalR 是建置即時 web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="7878a-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="7878a-210">您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="7878a-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="7878a-211">深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="7878a-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="7878a-212">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="7878a-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="7878a-213">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="7878a-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="7878a-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7878a-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
