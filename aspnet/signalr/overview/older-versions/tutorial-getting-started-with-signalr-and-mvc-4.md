---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 教學課程：開始使用 SignalR 1.x 及 MVC 4 |Microsoft Docs
author: pfletcher
description: 使用 ASP.NET SignalR 及 ASP.NET MVC 4 建置即時聊天應用程式。
ms.author: riande
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: a67d05288252c17d84b1d7df5f7bcddde3c887f5
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287706"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="d06e4-103">教學課程：開始使用 SignalR 1.x 及 MVC 4</span><span class="sxs-lookup"><span data-stu-id="d06e4-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="d06e4-104">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="d06e4-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d06e4-105">本教學課程會示範如何使用 ASP.NET SignalR 建立即時聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="d06e4-106">您會將 SignalR 加入至 MVC 4 應用程式，並建立交談檢視，以傳送和顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="d06e4-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="d06e4-107">總覽</span><span class="sxs-lookup"><span data-stu-id="d06e4-107">Overview</span></span>

<span data-ttu-id="d06e4-108">本教學課程會向您介紹使用 ASP.NET SignalR 及 ASP.NET MVC 4 的即時 web 應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="d06e4-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="d06e4-109">本教學課程會使用相同的交談應用程式程式碼做為[SignalR 開始使用教學課程](tutorial-getting-started-with-signalr.md)，但會顯示如何將它新增至以網際網路範本為基礎的 MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="d06e4-110">在本主題中，您將學習下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="d06e4-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="d06e4-111">將 SignalR 程式庫新增至 MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="d06e4-112">建立中樞類別以將內容推送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="d06e4-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="d06e4-113">使用 SignalR jQuery 程式庫在網頁上傳送訊息，並顯示從中樞的更新。</span><span class="sxs-lookup"><span data-stu-id="d06e4-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="d06e4-114">下列螢幕擷取畫面顯示在瀏覽器中執行之已完成的交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![交談執行個體](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="d06e4-116">章節：</span><span class="sxs-lookup"><span data-stu-id="d06e4-116">Sections:</span></span>

- [<span data-ttu-id="d06e4-117">設定專案</span><span class="sxs-lookup"><span data-stu-id="d06e4-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="d06e4-118">執行範例</span><span class="sxs-lookup"><span data-stu-id="d06e4-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="d06e4-119">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="d06e4-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="d06e4-120">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d06e4-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="d06e4-121">設定專案</span><span class="sxs-lookup"><span data-stu-id="d06e4-121">Set up the Project</span></span>

<span data-ttu-id="d06e4-122">必要條件：</span><span class="sxs-lookup"><span data-stu-id="d06e4-122">Prerequisites:</span></span>

- <span data-ttu-id="d06e4-123">Visual Studio 2010 SP1、 Visual Studio 2012 或 Visual Studio 2012 Express。</span><span class="sxs-lookup"><span data-stu-id="d06e4-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="d06e4-124">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發工具。</span><span class="sxs-lookup"><span data-stu-id="d06e4-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="d06e4-125">針對 Visual Studio 2010 中，安裝[ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)。</span><span class="sxs-lookup"><span data-stu-id="d06e4-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="d06e4-126">本節說明如何建立 ASP.NET MVC 4 應用程式、 新增 SignalR 程式庫和建立交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="d06e4-127">在 Visual Studio 建立 ASP.NET MVC 4 應用程式、 SignalRChat，將它命名，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d06e4-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="d06e4-128">在 VS 2010 中，選取 **.NET Framework 4** Framework 版本下拉式清單控制項中。</span><span class="sxs-lookup"><span data-stu-id="d06e4-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="d06e4-129">SignalR 的程式碼會執行在.NET Framework 4 和 4.5 版。</span><span class="sxs-lookup"><span data-stu-id="d06e4-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![建立 mvc web](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="d06e4-131">選取 [網際網路應用程式] 範本，請清除的選項**建立單元測試專案**，按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d06e4-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![建立 mvc 網際網路網站](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="d06e4-133">開啟**工具 > NuGet 套件管理員 > Package Manager Console**並執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="d06e4-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="d06e4-134">此步驟可將一組指令碼檔案和啟用 SignalR 功能的組件參考加入至專案。</span><span class="sxs-lookup"><span data-stu-id="d06e4-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="d06e4-135">在 **方案總管 中**展開指令碼 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d06e4-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="d06e4-136">請注意 SignalR 的指令碼程式庫已加入至專案。</span><span class="sxs-lookup"><span data-stu-id="d06e4-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![程式庫參考](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="d06e4-138">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |新的資料夾**，並新增名為的新資料夾**中樞**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="d06e4-139">以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |類別**，並建立新 C# 類別名為**ChatHub.cs**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="d06e4-140">您將使用這個類別做為 SignalR 伺服器中樞將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="d06e4-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="d06e4-141">如果您使用 Visual Studio 2012，並且已安裝[ASP.NET 和 Web 工具 2012.2 更新](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)，您可以使用新的 SignalR 項目範本建立中樞類別。</span><span class="sxs-lookup"><span data-stu-id="d06e4-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="d06e4-142">若要這樣做，請以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |新的項目**，選取**SignalR Hub 類別 (v1)**，並加以命名**ChatHub.cs**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="d06e4-143">中的程式碼取代**ChatHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="d06e4-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="d06e4-144">開啟**Global.asax**專案的檔案，並新增至方法的呼叫`RouteTable.Routes.MapHubs();`中的程式碼的第一行`Application_Start`方法。</span><span class="sxs-lookup"><span data-stu-id="d06e4-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="d06e4-145">此程式碼會註冊 SignalR 中樞的預設路由，並註冊任何其他路由之前，必須呼叫。</span><span class="sxs-lookup"><span data-stu-id="d06e4-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="d06e4-146">已完成`Application_Start`方法如以下範例所示。</span><span class="sxs-lookup"><span data-stu-id="d06e4-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="d06e4-147">編輯`HomeController`類別中找到**controllers/Homecontroller.cs**並將下列方法新增至類別。</span><span class="sxs-lookup"><span data-stu-id="d06e4-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="d06e4-148">這個方法會傳回**聊天**您將在稍後的步驟建立的檢視。</span><span class="sxs-lookup"><span data-stu-id="d06e4-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="d06e4-149">以滑鼠右鍵按一下`Chat`方法剛剛建立，並按一下 **加入檢視**以建立新的檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="d06e4-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="d06e4-150">在**加入檢視** 對話方塊中，請確定已選取核取方塊**使用版面配置頁或主版頁面**（清除其他核取方塊），然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![新增檢視](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="d06e4-152">編輯新的檢視檔案命名為**Chat.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="d06e4-153">在後&lt;h2&gt;標記中，貼上下列&lt;div&gt;一節和`@section scripts`到頁面的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="d06e4-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="d06e4-154">此指令碼可讓頁面，即可將交談訊息傳送，並顯示從伺服器的訊息。</span><span class="sxs-lookup"><span data-stu-id="d06e4-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="d06e4-155">[聊天室] 檢視的完整程式碼會出現在下列程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="d06e4-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d06e4-156">當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，套件管理員可能會安裝比本主題中顯示的版本還新的指令碼的版本。</span><span class="sxs-lookup"><span data-stu-id="d06e4-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="d06e4-157">請確定您的程式碼中的指令碼參考符合安裝在您專案的指令碼程式庫的版本。</span><span class="sxs-lookup"><span data-stu-id="d06e4-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="d06e4-158">**全部儲存**專案。</span><span class="sxs-lookup"><span data-stu-id="d06e4-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="d06e4-159">執行範例</span><span class="sxs-lookup"><span data-stu-id="d06e4-159">Run the Sample</span></span>

1. <span data-ttu-id="d06e4-160">按 F5 以偵錯模式中執行專案。</span><span class="sxs-lookup"><span data-stu-id="d06e4-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="d06e4-161">在瀏覽器網址列中，附加 **/home/聊天**專案的預設頁面的 url。</span><span class="sxs-lookup"><span data-stu-id="d06e4-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="d06e4-162">聊天室頁面載入瀏覽器執行個體，並提示輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="d06e4-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="d06e4-164">輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="d06e4-164">Enter a user name.</span></span>
4. <span data-ttu-id="d06e4-165">從瀏覽器的網址列複製 URL，並使用它來開啟兩個更多的瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="d06e4-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="d06e4-166">在每個瀏覽器執行個體中，輸入唯一的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="d06e4-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="d06e4-167">在每個瀏覽器執行個體中，新增註解，然後按一下**傳送**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="d06e4-168">註解應該會顯示在瀏覽器的所有執行個體。</span><span class="sxs-lookup"><span data-stu-id="d06e4-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d06e4-169">這個簡單的聊天應用程式不會維護伺服器上的討論內容。</span><span class="sxs-lookup"><span data-stu-id="d06e4-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="d06e4-170">中樞會廣播到所有目前使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="d06e4-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="d06e4-171">稍後加入聊天室使用者會看到訊息的時間加入它們加入。</span><span class="sxs-lookup"><span data-stu-id="d06e4-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="d06e4-172">下列螢幕擷取畫面顯示在瀏覽器中執行的聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![對談的瀏覽器](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="d06e4-174">在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="d06e4-175">如果您使用 Internet Explorer 作為您的瀏覽器，此節點會顯示在 偵錯模式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="d06e4-176">沒有名為的指令碼檔案**中樞**SignalR 程式庫以動態方式產生在執行階段。</span><span class="sxs-lookup"><span data-stu-id="d06e4-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="d06e4-177">此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="d06e4-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="d06e4-178">如果您使用非 Internet Explorer 的瀏覽器，您也可以存取動態**集線器**瀏覽至它直接，例如檔案 http://mywebsite/signalr/hubs。</span><span class="sxs-lookup"><span data-stu-id="d06e4-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="d06e4-180">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="d06e4-180">Examine the Code</span></span>

<span data-ttu-id="d06e4-181">SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 建立中樞做為主要協調物件在伺服器上，並使用 SignalR jQuery 程式庫來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="d06e4-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="d06e4-182">SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="d06e4-182">SignalR Hubs</span></span>

<span data-ttu-id="d06e4-183">在程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。</span><span class="sxs-lookup"><span data-stu-id="d06e4-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="d06e4-184">衍生自**中樞**類別是實用的方式，來建置 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="d06e4-185">您可以建立中樞類別上的公用方法，並在網頁上的 jQuery 指令碼的方式呼叫它們，以存取這些方法。</span><span class="sxs-lookup"><span data-stu-id="d06e4-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="d06e4-186">在對談程式碼中，用戶端呼叫**ChatHub.Send**方法以傳送新訊息。</span><span class="sxs-lookup"><span data-stu-id="d06e4-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="d06e4-187">中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.addNewMessageToPage**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="d06e4-188">**傳送**方法將示範數個中樞概念：</span><span class="sxs-lookup"><span data-stu-id="d06e4-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="d06e4-189">可讓用戶端呼叫，則請在中樞中宣告的公用方法。</span><span class="sxs-lookup"><span data-stu-id="d06e4-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="d06e4-190">使用**Microsoft.AspNet.SignalR.Hub.Clients**屬性來存取所有用戶端連線到此集線器。</span><span class="sxs-lookup"><span data-stu-id="d06e4-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="d06e4-191">在用戶端上呼叫 jQuery 函式 (例如`addNewMessageToPage`函式) 來更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="d06e4-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="d06e4-192">SignalR 和 jQuery</span><span class="sxs-lookup"><span data-stu-id="d06e4-192">SignalR and jQuery</span></span>

<span data-ttu-id="d06e4-193">**Chat.cshtml**檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d06e4-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="d06e4-194">在程式碼中的重要工作建立自動產生之 proxy 的中樞中，宣告的函式推播內容給用戶端，可以呼叫的伺服器和啟動連線將訊息傳送至中樞的參考。</span><span class="sxs-lookup"><span data-stu-id="d06e4-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="d06e4-195">下列程式碼會宣告中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="d06e4-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="d06e4-196">在 jQuery 中的伺服器類別和其成員的參考會處於駝峰式大小寫。</span><span class="sxs-lookup"><span data-stu-id="d06e4-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="d06e4-197">程式碼範例會參考 C# **ChatHub**在 jQuery 中做為類別**chatHub**。</span><span class="sxs-lookup"><span data-stu-id="d06e4-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="d06e4-198">如果您想要參考`ChatHub`類別在 jQuery 中使用傳統的 pascal 命名法大小寫，如同在 C# 中，編輯 ChatHub.cs 類別檔案。</span><span class="sxs-lookup"><span data-stu-id="d06e4-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="d06e4-199">新增`using`陳述式來參考`Microsoft.AspNet.SignalR.Hubs`命名空間。</span><span class="sxs-lookup"><span data-stu-id="d06e4-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="d06e4-200">然後新增`HubName`屬性設定為`ChatHub`類別，例如`[HubName("ChatHub")]`。</span><span class="sxs-lookup"><span data-stu-id="d06e4-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="d06e4-201">最後，更新您的 jQuery 參考`ChatHub`類別。</span><span class="sxs-lookup"><span data-stu-id="d06e4-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="d06e4-202">下列程式碼示範如何建立指令碼中的回呼函式。</span><span class="sxs-lookup"><span data-stu-id="d06e4-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="d06e4-203">在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。</span><span class="sxs-lookup"><span data-stu-id="d06e4-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="d06e4-204">選擇性呼叫`htmlEncode`函式會顯示為 HTML 的辦法之前先顯示在頁面中，做為防止指令碼資料隱碼攻擊的方式編碼的訊息內容。</span><span class="sxs-lookup"><span data-stu-id="d06e4-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="d06e4-205">下列程式碼示範如何使用中樞開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="d06e4-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="d06e4-206">程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**[聊天室] 頁面中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="d06e4-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="d06e4-207">這個方法可確保事件處理常式執行之前，會建立連接。</span><span class="sxs-lookup"><span data-stu-id="d06e4-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="d06e4-208">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d06e4-208">Next Steps</span></span>

<span data-ttu-id="d06e4-209">您已了解 SignalR 是用來建置即時 web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="d06e4-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="d06e4-210">您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="d06e4-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="d06e4-211">若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="d06e4-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="d06e4-212">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="d06e4-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="d06e4-213">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="d06e4-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="d06e4-214">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="d06e4-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
