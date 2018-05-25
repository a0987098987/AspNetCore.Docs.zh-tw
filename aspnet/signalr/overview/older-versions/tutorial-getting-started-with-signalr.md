---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 教學課程： 開始使用 SignalR 1.x |Microsoft 文件
author: pfletcher
description: 使用 ASP.NET SignalR 建立 HTML 網頁中的即時聊天應用程式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="53a4c-103">教學課程： 開始使用 SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="53a4c-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="53a4c-104">由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="53a4c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="53a4c-105">此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="53a4c-106">將 SignalR 加入空的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="53a4c-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="53a4c-107">總覽</span><span class="sxs-lookup"><span data-stu-id="53a4c-107">Overview</span></span>

<span data-ttu-id="53a4c-108">此教學課程介紹 SignalR 開發，以顯示如何建立簡單的瀏覽器為基礎的交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="53a4c-109">您會加入空的 ASP.NET web 應用程式中的 SignalR 程式庫、 建立中樞類別將訊息傳送至用戶端，並建立 HTML 網頁，可讓使用者傳送及接收交談的訊息。</span><span class="sxs-lookup"><span data-stu-id="53a4c-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="53a4c-110">示範如何在 MVC 4 中建立交談應用程式使用 MVC 檢視的類似教學課程，請參閱[SignalR 和 MVC 4 入門](index.md)。</span><span class="sxs-lookup"><span data-stu-id="53a4c-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="53a4c-111">本教學課程使用 SignalR 的發行 (1.x) 版本。</span><span class="sxs-lookup"><span data-stu-id="53a4c-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="53a4c-112">SignalR 之間變更的詳細資訊的 1.x 和 2.0，請參閱[升級 SignalR 1.x 專案](../releases/upgrading-signalr-1x-projects-to-20.md)。</span><span class="sxs-lookup"><span data-stu-id="53a4c-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="53a4c-113">SignalR 是開放原始碼.NET 程式庫建置 web 應用程式需要即時使用者互動或即時資料更新。</span><span class="sxs-lookup"><span data-stu-id="53a4c-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="53a4c-114">範例包括社交應用程式、 多使用者遊戲、 企業共同作業及新聞、 天氣或財務更新應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="53a4c-115">這些通常稱為即時應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-115">These are often called real-time applications.</span></span>

<span data-ttu-id="53a4c-116">SignalR 簡化建立即時應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="53a4c-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="53a4c-117">它包含 ASP.NET server 程式庫和 JavaScript 用戶端程式庫，讓您更輕鬆地管理用戶端伺服器連線，並將內容更新推送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="53a4c-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="53a4c-118">您可以將 SignalR 程式庫加入現有的 ASP.NET 應用程式即可取得即時的功能。</span><span class="sxs-lookup"><span data-stu-id="53a4c-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="53a4c-119">本教學課程將示範下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="53a4c-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="53a4c-120">將 SignalR 程式庫加入至 ASP.NET web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="53a4c-121">建立內容推播至用戶端中樞類別。</span><span class="sxs-lookup"><span data-stu-id="53a4c-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="53a4c-122">使用 SignalR jQuery 程式庫，在網頁中的傳送訊息，並顯示從中樞的更新。</span><span class="sxs-lookup"><span data-stu-id="53a4c-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="53a4c-123">下列螢幕擷取畫面顯示瀏覽器中執行之交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="53a4c-124">每個新的使用者可以張貼評論，並查看使用者加入這個聊天室之後新增的註解。</span><span class="sxs-lookup"><span data-stu-id="53a4c-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![交談的執行個體](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="53a4c-126">章節：</span><span class="sxs-lookup"><span data-stu-id="53a4c-126">Sections:</span></span>

- [<span data-ttu-id="53a4c-127">設定專案</span><span class="sxs-lookup"><span data-stu-id="53a4c-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="53a4c-128">執行範例</span><span class="sxs-lookup"><span data-stu-id="53a4c-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="53a4c-129">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="53a4c-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="53a4c-130">後續步驟</span><span class="sxs-lookup"><span data-stu-id="53a4c-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="53a4c-131">設定專案</span><span class="sxs-lookup"><span data-stu-id="53a4c-131">Set up the Project</span></span>

<span data-ttu-id="53a4c-132">本節顯示如何建立空的 ASP.NET web 應用程式，新增 SignalR，和建立交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="53a4c-133">必要條件：</span><span class="sxs-lookup"><span data-stu-id="53a4c-133">Prerequisites:</span></span>

- <span data-ttu-id="53a4c-134">Visual Studio 2010 SP1 或 2012年。</span><span class="sxs-lookup"><span data-stu-id="53a4c-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="53a4c-135">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發的工具。</span><span class="sxs-lookup"><span data-stu-id="53a4c-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="53a4c-136">[Microsoft ASP.NET 及 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)。</span><span class="sxs-lookup"><span data-stu-id="53a4c-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="53a4c-137">針對 Visual Studio 2012 中，此安裝程式會加入新的 ASP.NET 功能包括 SignalR 範本加入 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="53a4c-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="53a4c-138">Visual Studio 2010 SP1 中，安裝程式不提供，但您可以完成本教學課程中的設定步驟所述安裝 SignalR NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="53a4c-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="53a4c-139">下列步驟使用 Visual Studio 2012 建立空白 ASP.NET Web 應用程式，並新增 SignalR library:</span><span class="sxs-lookup"><span data-stu-id="53a4c-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="53a4c-140">在 Visual Studio 中建立空白 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![建立空的網站](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="53a4c-142">開啟**Package Manager Console**選取**工具 |程式庫套件管理員 |Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="53a4c-143">到主控台視窗輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="53a4c-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="53a4c-144">此命令會安裝最新版的 SignalR 1.x。</span><span class="sxs-lookup"><span data-stu-id="53a4c-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="53a4c-145">在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |類別**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="53a4c-146">將新類別**ChatHub**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="53a4c-147">在**方案總管 中**展開指令碼 節點。</span><span class="sxs-lookup"><span data-stu-id="53a4c-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="53a4c-148">JQuery 和 SignalR 的指令碼程式庫會顯示專案中。</span><span class="sxs-lookup"><span data-stu-id="53a4c-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![程式庫參考](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="53a4c-150">中的程式碼取代**ChatHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="53a4c-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="53a4c-151">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |新項目**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="53a4c-152">在**加入新項目**對話方塊中，選取**全域應用程式類別**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![加入全域](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="53a4c-154">加入下列`using`陳述式之後提供`using`Global.asax.cs 類別中的陳述式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="53a4c-155">加入下列行中的程式碼`Application_Start`全域註冊 SignalR 中樞的預設路由類別的方法。</span><span class="sxs-lookup"><span data-stu-id="53a4c-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="53a4c-156">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |新項目**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="53a4c-157">在**加入新項目**對話方塊中，選取 Html 網頁並按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="53a4c-158">在**方案總管 中**，以滑鼠右鍵按一下您剛建立的 HTML 網頁，然後按一下**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="53a4c-159">HTML 網頁中的預設程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="53a4c-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="53a4c-160">**儲存所有**專案。</span><span class="sxs-lookup"><span data-stu-id="53a4c-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="53a4c-161">執行範例</span><span class="sxs-lookup"><span data-stu-id="53a4c-161">Run the Sample</span></span>

1. <span data-ttu-id="53a4c-162">按 F5 以偵錯模式執行專案。</span><span class="sxs-lookup"><span data-stu-id="53a4c-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="53a4c-163">HTML 頁面載入瀏覽器執行個體，並提示輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="53a4c-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="53a4c-165">輸入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="53a4c-165">Enter a user name.</span></span>
3. <span data-ttu-id="53a4c-166">從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="53a4c-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="53a4c-167">在每個瀏覽器執行個體中，輸入唯一的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="53a4c-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="53a4c-168">每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="53a4c-169">這些註解應該顯示在所有瀏覽器執行個體。</span><span class="sxs-lookup"><span data-stu-id="53a4c-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53a4c-170">這個簡單的聊天應用程式不會維持在伺服器上的討論內容。</span><span class="sxs-lookup"><span data-stu-id="53a4c-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="53a4c-171">集線器會廣播到所有目前使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="53a4c-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="53a4c-172">稍後加入交談的使用者會看到訊息的時間加入它們加入。</span><span class="sxs-lookup"><span data-stu-id="53a4c-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="53a4c-173">下列螢幕擷取畫面顯示三個瀏覽器執行個體，其中一個執行個體傳送訊息時就會更新中執行之交談應用程式：</span><span class="sxs-lookup"><span data-stu-id="53a4c-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![對談瀏覽器](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="53a4c-175">在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="53a4c-176">沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。</span><span class="sxs-lookup"><span data-stu-id="53a4c-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="53a4c-177">這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="53a4c-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="53a4c-179">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="53a4c-179">Examine the Code</span></span>

<span data-ttu-id="53a4c-180">SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="53a4c-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="53a4c-181">SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="53a4c-181">SignalR Hubs</span></span>

<span data-ttu-id="53a4c-182">下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。</span><span class="sxs-lookup"><span data-stu-id="53a4c-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="53a4c-183">衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="53a4c-184">您可以建立中樞類別上的公用方法，然後呼叫在網頁中的 jQuery 指令碼的方式來存取這些方法。</span><span class="sxs-lookup"><span data-stu-id="53a4c-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="53a4c-185">在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。</span><span class="sxs-lookup"><span data-stu-id="53a4c-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="53a4c-186">中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.broadcastMessage**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="53a4c-187">**傳送**方法將示範中樞的幾個概念：</span><span class="sxs-lookup"><span data-stu-id="53a4c-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="53a4c-188">中樞上宣告的公用方法，可讓用戶端呼叫。</span><span class="sxs-lookup"><span data-stu-id="53a4c-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="53a4c-189">使用**Microsoft.AspNet.SignalR.Hub.Clients**動態屬性來存取所有用戶端連線到此中樞。</span><span class="sxs-lookup"><span data-stu-id="53a4c-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="53a4c-190">在用戶端上呼叫 jQuery 函式 (例如`broadcastMessage`函式) 來更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="53a4c-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="53a4c-191">SignalR 和 jQuery</span><span class="sxs-lookup"><span data-stu-id="53a4c-191">SignalR and jQuery</span></span>

<span data-ttu-id="53a4c-192">HTML 網頁中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。</span><span class="sxs-lookup"><span data-stu-id="53a4c-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="53a4c-193">在程式碼中的重要工作宣告參考宣告的函式，伺服器可以呼叫用戶端推入內容和啟動將訊息傳送至中樞連線的中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="53a4c-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="53a4c-194">下列程式碼會宣告中樞 proxy。</span><span class="sxs-lookup"><span data-stu-id="53a4c-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="53a4c-195">在 jQuery 伺服器類別和其成員的參考是 camel 命名法的大小寫。</span><span class="sxs-lookup"><span data-stu-id="53a4c-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="53a4c-196">程式碼範例會參考 C# **ChatHub**類別中當做 jQuery **chatHub**。</span><span class="sxs-lookup"><span data-stu-id="53a4c-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="53a4c-197">下列程式碼是如何在指令碼中建立的回呼函式。</span><span class="sxs-lookup"><span data-stu-id="53a4c-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="53a4c-198">在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。</span><span class="sxs-lookup"><span data-stu-id="53a4c-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="53a4c-199">HTML 編碼內容，再顯示它兩行是選擇性的並顯示簡單的方式，以避免指令碼資料隱碼。</span><span class="sxs-lookup"><span data-stu-id="53a4c-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="53a4c-200">下列程式碼會示範如何開啟與中樞的連線。</span><span class="sxs-lookup"><span data-stu-id="53a4c-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="53a4c-201">此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**的 HTML 網頁上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="53a4c-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="53a4c-202">這個方法會確保此事件處理常式執行之前，已建立連線。</span><span class="sxs-lookup"><span data-stu-id="53a4c-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="53a4c-203">後續步驟</span><span class="sxs-lookup"><span data-stu-id="53a4c-203">Next Steps</span></span>

<span data-ttu-id="53a4c-204">您已學習 SignalR 是建置即時 web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="53a4c-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="53a4c-205">您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="53a4c-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="53a4c-206">您可以提供範例應用程式在本教學課程或其他的 SignalR 應用程式在網際網路上透過將它們部署到主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="53a4c-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="53a4c-207">Microsoft 提供的免費 web 裝載中可用的最多 10 個 web sites [Windows Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="53a4c-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="53a4c-208">如需如何部署範例 SignalR 應用程式的逐步解說，請參閱[發行 SignalR 快速入門範例做為 Windows Azure 網站](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)。</span><span class="sxs-lookup"><span data-stu-id="53a4c-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="53a4c-209">如需如何部署 Visual Studio web 專案至 Windows Azure 網站的詳細資訊，請參閱[部署 ASP.NET 應用程式至 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="53a4c-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="53a4c-210">(注意： WebSocket 傳輸目前不支援的 Windows Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="53a4c-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="53a4c-211">當 WebSocket 傳輸無法使用，SignalR 的 [傳輸] 區段中所述，使用其他可用的傳輸[SignalR 主題的介紹](index.md)。)</span><span class="sxs-lookup"><span data-stu-id="53a4c-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="53a4c-212">深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="53a4c-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="53a4c-213">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="53a4c-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="53a4c-214">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="53a4c-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="53a4c-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="53a4c-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
