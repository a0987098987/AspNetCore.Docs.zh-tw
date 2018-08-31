---
title: 教學課程：SignalR on ASP.NET Core 使用者入門
author: tdykstra
description: 在本教學課程中，您會建立使用 SignalR for ASP.NET Core 的聊天應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751658"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="91813-103">教學課程：SignalR on ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="91813-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="91813-104">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="91813-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="91813-105">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="91813-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="91813-106">建立使用 SignalR on ASP.NET Core 的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="91813-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="91813-107">在伺服器上建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="91813-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="91813-108">從 JavaScript 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="91813-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="91813-109">使用中樞，將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="91813-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="91813-110">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="91813-110">At the end, you'll have a working chat app:</span></span>

![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="91813-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="91813-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91813-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="91813-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91813-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91813-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="91813-115">[Visual Studio 2017 15.7.3 版或更新版本](https://www.visualstudio.com/downloads/)，包含 **ASP.NET 及網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="91813-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="91813-116">.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="91813-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="91813-117">[npm](https://www.npmjs.com/get-npm) (Node.js 的套件管理員，用於 SignalR JavaScript 用戶端程式庫。)</span><span class="sxs-lookup"><span data-stu-id="91813-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="91813-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91813-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="91813-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91813-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="91813-120">.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="91813-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="91813-121">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91813-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="91813-122">[npm](https://www.npmjs.com/get-npm) (Node.js 的套件管理員，用於 SignalR JavaScript 用戶端程式庫。)</span><span class="sxs-lookup"><span data-stu-id="91813-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="91813-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="91813-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="91813-124">Visual Studio for Mac 7.5.4 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="91813-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="91813-125">[.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all) (隨附於 Visual Studio 安裝)</span><span class="sxs-lookup"><span data-stu-id="91813-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="91813-126">[npm](https://www.npmjs.com/get-npm) (Node.js 的套件管理員，用於 SignalR JavaScript 用戶端程式庫。)</span><span class="sxs-lookup"><span data-stu-id="91813-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="91813-127">建立專案</span><span class="sxs-lookup"><span data-stu-id="91813-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91813-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91813-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="91813-129">從功能表中選取 [檔案] > [新增專案] 。</span><span class="sxs-lookup"><span data-stu-id="91813-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="91813-130">在 [新增專案] 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="91813-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="91813-131">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="91813-131">Name the project *SignalRChat*.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="91813-133">選取 [Web 應用程式] 建立使用 Razor Pages 的專案。</span><span class="sxs-lookup"><span data-stu-id="91813-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="91813-134">選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.1]，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="91813-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="91813-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91813-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="91813-137">開啟您可以用於新專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="91813-138">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="91813-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="91813-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="91813-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="91813-140">從功能表中選取 [檔案] > [新增方案] 。</span><span class="sxs-lookup"><span data-stu-id="91813-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="91813-141">選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式] (不要選取 [ASP.NET Core Web 應用程式 (MVC)])。</span><span class="sxs-lookup"><span data-stu-id="91813-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="91813-142">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="91813-142">Select **Next**.</span></span>

* <span data-ttu-id="91813-143">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="91813-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="91813-144">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="91813-144">Add the SignalR client library</span></span>

<span data-ttu-id="91813-145">SignalR 伺服器程式庫包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="91813-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="91813-146">但是，您必須從 npm 這個 Node.js 套件管理員中取得 JavaScript 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="91813-146">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91813-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91813-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="91813-148">在 [套件管理員主控台] (PMC) 中，切換至專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="91813-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="91813-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91813-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="91813-150">切換至新的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="91813-151">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="91813-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="91813-152">在 [終端機] 中，巡覽至專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="91813-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="91813-153">執行 npm 初始設定式來建立 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="91813-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="91813-154">該命令會建立類似下列範例的輸出：</span><span class="sxs-lookup"><span data-stu-id="91813-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="91813-155">安裝用戶端程式庫套件：</span><span class="sxs-lookup"><span data-stu-id="91813-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="91813-156">該命令會建立類似下列範例的輸出：</span><span class="sxs-lookup"><span data-stu-id="91813-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="91813-157">`npm install` 命令已將 JavaScript 用戶端程式庫下載到 *node_modules* 下的子資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="91813-158">將其從該處複製到 *wwwroot* 下您可以從聊天應用程式網頁參考的資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="91813-159">在 *wwwroot/lib* 中建立 *signalr* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="91813-160">將 *signalr.js* 檔案從 *node_modules/@aspnet/signalr/dist/browser* 複製到新的 *wwwroot/lib/signalr* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="91813-161">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="91813-161">Create the SignalR hub</span></span>

<span data-ttu-id="91813-162">[中樞](xref:signalr/hubs)類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="91813-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="91813-163">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="91813-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="91813-164">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="91813-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="91813-165">`ChatHub` 類別繼承自 SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) 類別。</span><span class="sxs-lookup"><span data-stu-id="91813-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="91813-166">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="91813-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="91813-167">任何連線的用戶端都可以呼叫 `SendMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="91813-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="91813-168">它會將接收的訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="91813-168">It sends the received message to all clients.</span></span> <span data-ttu-id="91813-169">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="91813-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="91813-170">設定專案以使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="91813-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="91813-171">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="91813-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="91813-172">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="91813-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="91813-173">這些變更會將 SignalR 新增至[相依性插入](xref:fundamentals/dependency-injection)系統和[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="91813-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="91813-174">建立 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="91813-174">Create the SignalR client code</span></span>

* <span data-ttu-id="91813-175">以下列程式碼取代 *Pages\Index.cshtml* 中的內容：</span><span class="sxs-lookup"><span data-stu-id="91813-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="91813-176">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="91813-176">The preceding code:</span></span>

  * <span data-ttu-id="91813-177">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="91813-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="91813-178">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="91813-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="91813-179">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="91813-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="91813-180">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="91813-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="91813-181">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="91813-181">The preceding code:</span></span>

  * <span data-ttu-id="91813-182">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="91813-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="91813-183">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="91813-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="91813-184">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="91813-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="91813-185">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="91813-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91813-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91813-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="91813-187">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="91813-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="91813-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91813-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="91813-189">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="91813-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="91813-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="91813-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="91813-191">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="91813-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="91813-192">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="91813-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="91813-193">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="91813-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="91813-194">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="91813-194">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="91813-196">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="91813-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="91813-197">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="91813-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="91813-198">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="91813-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="91813-199">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="91813-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="91813-200">![signalr.js 找不到錯誤](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="91813-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="91813-201">後續步驟</span><span class="sxs-lookup"><span data-stu-id="91813-201">Next steps</span></span>

<span data-ttu-id="91813-202">如果您想要用戶端連線到不同網域中的 SignalR 應用程式，您必須啟用跨原始資源共用 (CORS)。</span><span class="sxs-lookup"><span data-stu-id="91813-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="91813-203">如需詳細資訊，請參閱[跨原始資源共用](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing)。</span><span class="sxs-lookup"><span data-stu-id="91813-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="91813-204">若要深入了解 SignalR、中樞及 JavaScript 用戶端，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="91813-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="91813-205">SignalR for ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="91813-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="91813-206">使用 SignalR for ASP.NET Core 的中樞</span><span class="sxs-lookup"><span data-stu-id="91813-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="91813-207">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="91813-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
