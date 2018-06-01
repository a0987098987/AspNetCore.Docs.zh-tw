---
title: 開始使用 ASP.NET Core 的 SignalR
author: rachelappel
description: 在本教學課程中，您可以建立使用 SignalR 的 ASP.NET Core 應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: eb14fbf42f5c18ccdc3ca42af8fd8bcfaa15c623
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688584"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="bbf04-103">開始使用 ASP.NET Core 的 SignalR</span><span class="sxs-lookup"><span data-stu-id="bbf04-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="bbf04-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="bbf04-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="bbf04-105">本教學課程將教導您建置適用於 ASP.NET Core 使用 SignalR 的即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="bbf04-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![方案](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="bbf04-107">本教學課程將示範下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="bbf04-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbf04-108">建立 SignalR ASP.NET Core web 應用程式上。</span><span class="sxs-lookup"><span data-stu-id="bbf04-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="bbf04-109">建立 SignalR 中樞內容推播至用戶端。</span><span class="sxs-lookup"><span data-stu-id="bbf04-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="bbf04-110">修改`Startup`類別，並設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="bbf04-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="bbf04-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bbf04-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="bbf04-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="bbf04-112">Prerequisites</span></span>

<span data-ttu-id="bbf04-113">安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="bbf04-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbf04-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbf04-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="bbf04-115">.NET core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="bbf04-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="bbf04-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 或更新版本的版本**ASP.NET 及 web 開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="bbf04-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="bbf04-117">npm</span><span class="sxs-lookup"><span data-stu-id="bbf04-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bbf04-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbf04-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="bbf04-119">.NET core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="bbf04-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="bbf04-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbf04-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="bbf04-121">Visual Studio 程式碼的 C#</span><span class="sxs-lookup"><span data-stu-id="bbf04-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="bbf04-122">npm</span><span class="sxs-lookup"><span data-stu-id="bbf04-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="bbf04-123">建立 SignalR 用戶端和伺服器所裝載的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="bbf04-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbf04-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbf04-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="bbf04-125">使用**檔案** > **新專案**功能表選項，然後選擇**ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="bbf04-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="bbf04-126">將專案命名*SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="bbf04-126">Name the project *SignalRChat*.</span></span>

   ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="bbf04-128">選取**Web 應用程式**建立使用 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="bbf04-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="bbf04-129">然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="bbf04-129">Then select **OK**.</span></span> <span data-ttu-id="bbf04-130">確定**ASP.NET Core 2.1**雖然 SignalR 執行較舊版本的.NET framework 選取器中選取。</span><span class="sxs-lookup"><span data-stu-id="bbf04-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="bbf04-132">Visual Studio 包含`Microsoft.AspNetCore.SignalR`封裝包含其伺服器程式庫的一部分其**ASP.NET Core Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="bbf04-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="bbf04-133">不過，適用於 SignalR JavaScript 用戶端程式庫必須使用安裝*npm*。</span><span class="sxs-lookup"><span data-stu-id="bbf04-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="bbf04-134">在中執行下列命令**Package Manager Console**視窗中的，從專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="bbf04-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="bbf04-135">複製*signalr.js*檔案從*node_modules\\ @aspnet\signalr\dist\browser* 至*lib*專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="bbf04-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bbf04-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbf04-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="bbf04-137">從**整合的終端機**，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bbf04-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

2. <span data-ttu-id="bbf04-138">JavaScript 用戶端程式庫使用安裝*npm*。</span><span class="sxs-lookup"><span data-stu-id="bbf04-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="bbf04-139">複製*signalr.js*檔案從*node_modules\\ @aspnet\signalr\dist\browser* 至*lib*專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="bbf04-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="bbf04-140">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="bbf04-140">Create the SignalR Hub</span></span>

<span data-ttu-id="bbf04-141">集線器是做為概要管線可讓用戶端與伺服器彼此呼叫方法的類別。</span><span class="sxs-lookup"><span data-stu-id="bbf04-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbf04-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbf04-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="bbf04-143">將類別加入專案中，選擇**檔案** > **新增** > **檔案**，然後選取**Visual C# 類別**。</span><span class="sxs-lookup"><span data-stu-id="bbf04-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="bbf04-144">繼承自`Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="bbf04-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="bbf04-145">`Hub`類別包含屬性和事件管理連接和群組，以及傳送和接收資料。</span><span class="sxs-lookup"><span data-stu-id="bbf04-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="bbf04-146">建立`SendMessage`連接的交談的所有用戶端傳送訊息的方法。</span><span class="sxs-lookup"><span data-stu-id="bbf04-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="bbf04-147">請注意，它會傳回[工作](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)，因為 SignalR 是非同步。</span><span class="sxs-lookup"><span data-stu-id="bbf04-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="bbf04-148">非同步程式碼較易調整大小。</span><span class="sxs-lookup"><span data-stu-id="bbf04-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bbf04-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbf04-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="bbf04-150">開啟*SignalRChat* Visual Studio 程式碼中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="bbf04-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="bbf04-151">將類別加入專案中，選取**檔案** > **新檔案**從功能表。</span><span class="sxs-lookup"><span data-stu-id="bbf04-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="bbf04-152">繼承自`Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="bbf04-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="bbf04-153">`Hub`類別包含屬性和事件管理連接和群組，以及傳送和接收資料給用戶端。</span><span class="sxs-lookup"><span data-stu-id="bbf04-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="bbf04-154">將 `SendMessage` 方法加入類別中。</span><span class="sxs-lookup"><span data-stu-id="bbf04-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="bbf04-155">`SendMessage`方法連接的交談的所有用戶端會傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="bbf04-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="bbf04-156">請注意，它會傳回[工作](/dotnet/api/system.threading.tasks.task)，因為 SignalR 是非同步。</span><span class="sxs-lookup"><span data-stu-id="bbf04-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="bbf04-157">非同步程式碼較易調整大小。</span><span class="sxs-lookup"><span data-stu-id="bbf04-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="bbf04-158">設定專案以使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="bbf04-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="bbf04-159">SignalR 伺服器必須設定，讓它知道要傳遞到 SignalR 要求。</span><span class="sxs-lookup"><span data-stu-id="bbf04-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="bbf04-160">若要設定 SignalR 專案，請修改專案的`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="bbf04-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="bbf04-161">`services.AddSignalR` 將 SignalR 的一部分[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="bbf04-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="bbf04-162">設定路由，以便您使用的中樞`UseSignalR`。</span><span class="sxs-lookup"><span data-stu-id="bbf04-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="bbf04-163">建立 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="bbf04-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="bbf04-164">取代中的內容*Pages\Index.cshtml*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="bbf04-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="bbf04-165">前面的 HTML 顯示名稱和訊息欄位及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbf04-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="bbf04-166">請注意底部的指令碼參考： SignalR 的參考和*chat.js*。</span><span class="sxs-lookup"><span data-stu-id="bbf04-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="bbf04-167">加入名為的 JavaScript 檔案*chat.js*至*wwwroot\js*資料夾。</span><span class="sxs-lookup"><span data-stu-id="bbf04-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="bbf04-168">將下列程式碼加入該檔案：</span><span class="sxs-lookup"><span data-stu-id="bbf04-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="bbf04-169">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="bbf04-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbf04-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbf04-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="bbf04-171">選取**偵錯** > **啟動但不偵錯**啟動瀏覽器，並載入本機網站。</span><span class="sxs-lookup"><span data-stu-id="bbf04-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="bbf04-172">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="bbf04-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="bbf04-173">開啟另一個瀏覽器執行個體 （任何瀏覽器），並在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="bbf04-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="bbf04-174">選擇任一個瀏覽器，輸入名稱和訊息，然後按一下**傳送** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbf04-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="bbf04-175">名稱和訊息會顯示兩個頁面上立即。</span><span class="sxs-lookup"><span data-stu-id="bbf04-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bbf04-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbf04-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="bbf04-177">按 [偵錯] (F5) 以建置並執行程式。</span><span class="sxs-lookup"><span data-stu-id="bbf04-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="bbf04-178">執行程式，開啟瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="bbf04-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="bbf04-179">開啟另一個瀏覽器視窗，並載入本機的網站。</span><span class="sxs-lookup"><span data-stu-id="bbf04-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="bbf04-180">選擇任一個瀏覽器，輸入名稱和訊息，然後按一下**傳送** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bbf04-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="bbf04-181">名稱和訊息會顯示兩個頁面上立即。</span><span class="sxs-lookup"><span data-stu-id="bbf04-181">The name and message are displayed on both pages instantly.</span></span>

---

  ![方案](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="bbf04-183">相關資源</span><span class="sxs-lookup"><span data-stu-id="bbf04-183">Related resources</span></span>

[<span data-ttu-id="bbf04-184">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="bbf04-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
