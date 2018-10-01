---
title: 教學課程：SignalR on ASP.NET Core 使用者入門
author: tdykstra
description: 在本教學課程中，您會建立使用 SignalR for ASP.NET Core 的聊天應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6f93d6dc664f68425ef0fa0d02f9011e4875bc33
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402129"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="a4e6a-103">教學課程：SignalR on ASP.NET Core 使用者入門</span><span class="sxs-lookup"><span data-stu-id="a4e6a-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="a4e6a-104">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="a4e6a-105">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a4e6a-106">建立使用 SignalR on ASP.NET Core 的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="a4e6a-107">在伺服器上建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="a4e6a-108">從 JavaScript 用戶端連線到 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="a4e6a-109">使用中樞，將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="a4e6a-110">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-110">At the end, you'll have a working chat app:</span></span>

![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="a4e6a-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4e6a-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="a4e6a-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4e6a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4e6a-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4e6a-115">[Visual Studio 2017 15.8 版或更新版本](https://www.visualstudio.com/downloads/)，包含 **ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="a4e6a-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a4e6a-116">.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a4e6a-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4e6a-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4e6a-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a4e6a-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4e6a-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a4e6a-119">.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="a4e6a-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a4e6a-120">C# for Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4e6a-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4e6a-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a4e6a-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="a4e6a-122">Visual Studio for Mac 7.5.4 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="a4e6a-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="a4e6a-123">[.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all) (隨附於 Visual Studio 安裝)</span><span class="sxs-lookup"><span data-stu-id="a4e6a-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="a4e6a-124">建立專案</span><span class="sxs-lookup"><span data-stu-id="a4e6a-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4e6a-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4e6a-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="a4e6a-126">從功能表中選取 [檔案] > [新增專案] 。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="a4e6a-127">在 [新增專案] 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a4e6a-128">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-128">Name the project *SignalRChat*.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="a4e6a-130">選取 [Web 應用程式] 建立使用 Razor Pages 的專案。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="a4e6a-131">選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.1]，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4e6a-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4e6a-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="a4e6a-134">開啟您可以用於新專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="a4e6a-135">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4e6a-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a4e6a-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a4e6a-137">從功能表中選取 [檔案] > [新增方案] 。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="a4e6a-138">選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式] (不要選取 [ASP.NET Core Web 應用程式 (MVC)])。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="a4e6a-139">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-139">Select **Next**.</span></span>

* <span data-ttu-id="a4e6a-140">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="a4e6a-141">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="a4e6a-141">Add the SignalR client library</span></span>

<span data-ttu-id="a4e6a-142">SignalR 伺服器程式庫包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="a4e6a-143">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="a4e6a-144">針對此教學課程，您會使用[程式庫管理員 (LibMan)](xref:client-side/libman/index) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="a4e6a-145">[unpkg](https://unpkg.com/#/) 是一個[內容傳遞網路](https://wikipedia.org/wiki/Content_delivery_network)，可以傳遞在 [npm、Node.js 套件管理員](https://www.npmjs.com/get-npm)中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4e6a-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4e6a-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="a4e6a-147">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [新增] > [用戶端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="a4e6a-148">在 [新增用戶端程式庫] 對話方塊中，針對 [提供者] 選取 [unpkg]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="a4e6a-149">針對 [程式庫]，輸入 _@aspnet/signalr@1_，然後選取不是預覽版的最新版本。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/libman1.png)

* <span data-ttu-id="a4e6a-151">選取 [選擇特定檔案]、展開 [散發者/瀏覽器] 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="a4e6a-152">將 [目標位置] 設定為 *wwwroot/lib/signalr/*，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取檔案與目的地](signalr/_static/libman2.png)

  <span data-ttu-id="a4e6a-154">[LibMan](xref:client-side/libman/index) 會建立 *wwwroot/lib/signalr* 資料夾並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4e6a-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4e6a-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="a4e6a-156">在 [整合式終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="a4e6a-157">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="a4e6a-158">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="a4e6a-159">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="a4e6a-160">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="a4e6a-161">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="a4e6a-162">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="a4e6a-163">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-163">Copy only the specified files.</span></span>

  <span data-ttu-id="a4e6a-164">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4e6a-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a4e6a-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a4e6a-166">在 [終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="a4e6a-167">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="a4e6a-168">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="a4e6a-169">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="a4e6a-170">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="a4e6a-171">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="a4e6a-172">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-172">Copy only the specified files.</span></span>

  <span data-ttu-id="a4e6a-173">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="a4e6a-174">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="a4e6a-174">Create the SignalR hub</span></span>

<span data-ttu-id="a4e6a-175">[中樞](xref:signalr/hubs)類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="a4e6a-176">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="a4e6a-177">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="a4e6a-178">`ChatHub` 類別繼承自 SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) 類別。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="a4e6a-179">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="a4e6a-180">任何連線的用戶端都可以呼叫 `SendMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="a4e6a-181">它會將接收的訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-181">It sends the received message to all clients.</span></span> <span data-ttu-id="a4e6a-182">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="a4e6a-183">設定專案以使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="a4e6a-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="a4e6a-184">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="a4e6a-185">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="a4e6a-186">這些變更會將 SignalR 新增至[相依性插入](xref:fundamentals/dependency-injection)系統和[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="a4e6a-187">建立 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="a4e6a-187">Create the SignalR client code</span></span>

* <span data-ttu-id="a4e6a-188">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="a4e6a-189">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-189">The preceding code:</span></span>

  * <span data-ttu-id="a4e6a-190">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="a4e6a-191">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="a4e6a-192">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="a4e6a-193">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="a4e6a-194">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-194">The preceding code:</span></span>

  * <span data-ttu-id="a4e6a-195">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="a4e6a-196">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="a4e6a-197">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a4e6a-198">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a4e6a-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4e6a-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4e6a-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4e6a-200">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4e6a-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4e6a-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a4e6a-202">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4e6a-203">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a4e6a-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a4e6a-204">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="a4e6a-205">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="a4e6a-206">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="a4e6a-207">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="a4e6a-209">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="a4e6a-210">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="a4e6a-211">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="a4e6a-212">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="a4e6a-213">![signalr.js 找不到錯誤](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="a4e6a-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4e6a-214">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a4e6a-214">Next steps</span></span>

<span data-ttu-id="a4e6a-215">如果您想要用戶端連線到不同網域中的 SignalR 應用程式，您必須啟用跨原始資源共用 (CORS)。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="a4e6a-216">如需詳細資訊，請參閱[跨原始資源共用](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing)。</span><span class="sxs-lookup"><span data-stu-id="a4e6a-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="a4e6a-217">若要深入了解 SignalR、中樞及 JavaScript 用戶端，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="a4e6a-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="a4e6a-218">SignalR for ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="a4e6a-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="a4e6a-219">使用 SignalR for ASP.NET Core 的中樞</span><span class="sxs-lookup"><span data-stu-id="a4e6a-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a4e6a-220">ASP.NET Core SignalR JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="a4e6a-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
