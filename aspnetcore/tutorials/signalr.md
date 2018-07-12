---
title: SignalR on ASP.NET Core 使用者入門
author: rachelappel
description: 在本教學課程中，您會使用 SignalR for ASP.NET Core 建立應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830551"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="a8f82-103">SignalR on ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="a8f82-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="a8f82-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="a8f82-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="a8f82-105">本教學課程將教導您使用 SignalR for ASP.NET Core 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a8f82-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![方案](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="a8f82-107">本教學課程示範下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="a8f82-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8f82-108">建立 SignalR on ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8f82-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="a8f82-109">建立 SignalR 中樞將內容推送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8f82-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="a8f82-110">修改 `Startup` 類別，並設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8f82-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="a8f82-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8f82-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8f82-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="a8f82-112">Prerequisites</span></span>

<span data-ttu-id="a8f82-113">安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="a8f82-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f82-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f82-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="a8f82-115">.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8f82-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a8f82-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 版或更新版本，包含 **ASP.NET 及網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="a8f82-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a8f82-117">npm</span><span class="sxs-lookup"><span data-stu-id="a8f82-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f82-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8f82-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a8f82-119">.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a8f82-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a8f82-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8f82-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a8f82-121">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8f82-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="a8f82-122">npm</span><span class="sxs-lookup"><span data-stu-id="a8f82-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="a8f82-123">建立裝載 SignalR 用戶端和伺服器的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="a8f82-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f82-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f82-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a8f82-125">使用 [檔案] > [新增專案] 功能表選項，然後選擇 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="a8f82-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a8f82-126">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="a8f82-126">Name the project *SignalRChat*.</span></span>

   ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="a8f82-128">選取 [Web 應用程式] 使用 Razor Pages 建立專案。</span><span class="sxs-lookup"><span data-stu-id="a8f82-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="a8f82-129">然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a8f82-129">Then select **OK**.</span></span> <span data-ttu-id="a8f82-130">確定從架構選取器選取 [ASP.NET Core 2.1]，雖然 SignalR 在較舊版本的 .NET 上執行。</span><span class="sxs-lookup"><span data-stu-id="a8f82-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="a8f82-132">Visual Studio 包含 `Microsoft.AspNetCore.SignalR` 套件，它的 **ASP.NET Core Web 應用程式**範本當中包含其伺服器程式庫。</span><span class="sxs-lookup"><span data-stu-id="a8f82-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a8f82-133">不過，必須使用 *npm* 安裝適用於 SignalR 的 JavaScript 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="a8f82-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="a8f82-134">在 [套件管理員主控台] 視窗中，從專案根目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a8f82-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="a8f82-135">在專案的 *lib* 資料夾中，建立名為"signalr" 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8f82-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="a8f82-136">將 *signalr.js* 檔案從 *node_modules\\@aspnet\signalr\dist\browser* 複製到這個資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8f82-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f82-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8f82-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a8f82-138">從 [整合式終端機] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a8f82-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="a8f82-139">使用 *npm* 安裝 JavaScript 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="a8f82-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="a8f82-140">在專案的 *lib* 資料夾中，建立名為"signalr" 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8f82-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="a8f82-141">將 *signalr.js* 檔案從 *node_modules\\@aspnet\signalr\dist\browser* 複製到這個資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8f82-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="a8f82-142">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="a8f82-142">Create the SignalR Hub</span></span>

<span data-ttu-id="a8f82-143">中樞是作為高階管線的類別，可讓用戶端與伺服器對彼此呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="a8f82-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f82-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f82-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a8f82-145">將類別新增至專案中，方法是選擇 [檔案] > [新增] > [檔案]，然後選取 [Visual C# 類別]。</span><span class="sxs-lookup"><span data-stu-id="a8f82-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="a8f82-146">將類別命名為 `ChatHub`，並將檔案命名為 *ChatHub.cs*。</span><span class="sxs-lookup"><span data-stu-id="a8f82-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="a8f82-147">繼承自 `Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="a8f82-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a8f82-148">`Hub` 類別包含屬性和事件以便管理連線和群組，以及傳送和接收資料。</span><span class="sxs-lookup"><span data-stu-id="a8f82-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="a8f82-149">建立 `SendMessage` 方法，以傳送訊息給所有已連線的交談用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8f82-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="a8f82-150">請注意，它會傳回 [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx) (工作)，因為 SignalR 為非同步。</span><span class="sxs-lookup"><span data-stu-id="a8f82-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="a8f82-151">非同步程式碼較易調整大小。</span><span class="sxs-lookup"><span data-stu-id="a8f82-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f82-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8f82-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a8f82-153">在 Visual Studio Code 中開啟 *SignalRChat* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8f82-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="a8f82-154">將類別新增至專案中，方法是從功能表選取 [檔案] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="a8f82-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="a8f82-155">將類別命名為 `ChatHub`，並將檔案命名為 *ChatHub.cs*。</span><span class="sxs-lookup"><span data-stu-id="a8f82-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="a8f82-156">繼承自 `Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="a8f82-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a8f82-157">`Hub` 類別包含屬性和事件以便管理連線和群組，以及傳送和接收資料至用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8f82-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="a8f82-158">將 `SendMessage` 方法加入類別中。</span><span class="sxs-lookup"><span data-stu-id="a8f82-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="a8f82-159">`SendMessage` 方法會傳送訊息給所有已連線的交談用戶端。</span><span class="sxs-lookup"><span data-stu-id="a8f82-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="a8f82-160">請注意，它會傳回 [Task](/dotnet/api/system.threading.tasks.task) (工作)，因為 SignalR 為非同步。</span><span class="sxs-lookup"><span data-stu-id="a8f82-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="a8f82-161">非同步程式碼較易調整大小。</span><span class="sxs-lookup"><span data-stu-id="a8f82-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="a8f82-162">設定專案以使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="a8f82-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="a8f82-163">SignalR 伺服器必須設定，它才知道要傳遞要求給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="a8f82-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="a8f82-164">若要設定 SignalR 專案，請修改專案的 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="a8f82-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="a8f82-165">`services.AddSignalR` 可讓[相依性插入](xref:fundamentals/dependency-injection)系統使用 SignalR 服務。</span><span class="sxs-lookup"><span data-stu-id="a8f82-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="a8f82-166">在 `Configure` 方法中使用 `UseSignalR` 來設定中樞的路由。</span><span class="sxs-lookup"><span data-stu-id="a8f82-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="a8f82-167">`app.UseSignalR` 將 SignalR 新增到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="a8f82-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="a8f82-168">建立 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="a8f82-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="a8f82-169">將名為 *chat.js* 的 JavaScript 檔案新增至 *wwwroot\js* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8f82-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="a8f82-170">將下列程式碼加入該檔案：</span><span class="sxs-lookup"><span data-stu-id="a8f82-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="a8f82-171">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a8f82-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="a8f82-172">前面的 HTML 會顯示名稱和訊息欄位，以及一個提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="a8f82-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="a8f82-173">請注意底部的指令碼參考：SignalR 和 *chat.js* 的參考。</span><span class="sxs-lookup"><span data-stu-id="a8f82-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a8f82-174">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a8f82-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a8f82-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8f82-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a8f82-176">選取 [偵錯] > [啟動但不偵錯] 啟動瀏覽器，並載入本機網站。</span><span class="sxs-lookup"><span data-stu-id="a8f82-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="a8f82-177">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="a8f82-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a8f82-178">開啟另一個瀏覽器執行個體 (任何瀏覽器)，並在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="a8f82-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a8f82-179">選擇任一個瀏覽器、輸入名稱和訊息，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a8f82-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a8f82-180">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="a8f82-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a8f82-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8f82-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a8f82-182">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="a8f82-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a8f82-183">執行程式會開啟瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="a8f82-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="a8f82-184">開啟另一個瀏覽器視窗，並在其中載入本機網站。</span><span class="sxs-lookup"><span data-stu-id="a8f82-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="a8f82-185">選擇任一個瀏覽器、輸入名稱和訊息，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a8f82-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a8f82-186">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="a8f82-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![方案](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="a8f82-188">相關資源</span><span class="sxs-lookup"><span data-stu-id="a8f82-188">Related resources</span></span>

[<span data-ttu-id="a8f82-189">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="a8f82-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
