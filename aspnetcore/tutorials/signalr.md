---
title: 開始使用 ASP.NET Core SignalR
author: bradygaster
description: 在本教學課程中，您會建立使用 ASP.NET Core SignalR 的聊天應用程式。
ms.author: bradyg
ms.custom: mvc
ms.date: 09/24/2019
uid: tutorials/signalr
ms.openlocfilehash: bec01adc2682f83b0225df66e221bd2e4ea9feb4
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925327"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="add59-103">教學課程：開始使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="add59-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="add59-104">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="add59-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="add59-105">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="add59-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="add59-106">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="add59-106">Create a web project.</span></span>
> * <span data-ttu-id="add59-107">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="add59-108">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="add59-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="add59-109">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="add59-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="add59-110">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="add59-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="add59-111">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="add59-111">At the end, you'll have a working chat app:</span></span>

![SignalR 範例應用程式](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="add59-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="add59-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="add59-117">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="add59-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="add59-119">從功能表中選取 [檔案] > [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="add59-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="add59-120">在 [建立新專案] 對話方塊中，選取 [ASP.NET Core Web 應用程式]，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="add59-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="add59-121">在 [設定新專案] 對話方塊中，將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="add59-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="add59-122">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="add59-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="add59-123">選取 [Web 應用程式] 建立使用 Razor Pages 的專案，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="add59-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="add59-126">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="add59-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="add59-127">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="add59-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="add59-129">從功能表中選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="add59-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="add59-130">選取 [.NET Core] > [應用程式] > [Web 應用程式] (不選取 [Web 應用程式 (模型-檢視-控制器)])，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="add59-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="add59-131">請確認 [目標 Framework] 設為 **.NET Core 3.0**，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="add59-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="add59-132">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="add59-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="add59-133">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="add59-133">Add the SignalR client library</span></span>

<span data-ttu-id="add59-134">SignalR 伺服器程式庫包含在 ASP.NET Core 3.0 共用架構內。</span><span class="sxs-lookup"><span data-stu-id="add59-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="add59-135">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="add59-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="add59-136">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="add59-137">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="add59-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="add59-139">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [新增] > [用戶端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="add59-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="add59-140">在 [新增用戶端程式庫] 對話方塊中，針對 [提供者] 選取 [unpkg]。</span><span class="sxs-lookup"><span data-stu-id="add59-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="add59-141">針對 [程式庫] 輸入 `@microsoft/signalr@latest`。</span><span class="sxs-lookup"><span data-stu-id="add59-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="add59-142">選取 [選擇特定檔案]、展開 [散發者/瀏覽器] 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="add59-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="add59-143">將 [**目標位置**] 設定為*wwwroot/js/signalr/* ，然後選取 [**安裝**]。</span><span class="sxs-lookup"><span data-stu-id="add59-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="add59-145">LibMan 會建立*wwwroot/js/signalr*資料夾，並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="add59-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="add59-147">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="add59-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="add59-148">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="add59-149">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="add59-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="add59-150">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="add59-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="add59-151">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="add59-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="add59-152">將檔案複製到*wwwroot/js/signalr*目的地。</span><span class="sxs-lookup"><span data-stu-id="add59-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="add59-153">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="add59-153">Copy only the specified files.</span></span>

  <span data-ttu-id="add59-154">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="add59-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="add59-156">在 [終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="add59-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="add59-157">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="add59-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="add59-158">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="add59-159">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="add59-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="add59-160">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="add59-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="add59-161">將檔案複製到*wwwroot/js/signalr*目的地。</span><span class="sxs-lookup"><span data-stu-id="add59-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="add59-162">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="add59-162">Copy only the specified files.</span></span>

  <span data-ttu-id="add59-163">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="add59-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="add59-164">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="add59-164">Create a SignalR hub</span></span>

<span data-ttu-id="add59-165">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="add59-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="add59-166">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="add59-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="add59-167">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="add59-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="add59-168">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="add59-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="add59-169">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="add59-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="add59-170">連線的用戶端可以呼叫 `SendMessage` 方法將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="add59-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="add59-171">本教學課程稍後將示範呼叫該方法的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="add59-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="add59-172">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="add59-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="add59-173">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="add59-173">Configure SignalR</span></span>

<span data-ttu-id="add59-174">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="add59-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="add59-175">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="add59-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="add59-176">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入和路由系統。</span><span class="sxs-lookup"><span data-stu-id="add59-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="add59-177">新增 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="add59-177">Add SignalR client code</span></span>

* <span data-ttu-id="add59-178">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="add59-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="add59-179">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="add59-179">The preceding code:</span></span>

  * <span data-ttu-id="add59-180">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="add59-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="add59-181">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="add59-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="add59-182">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="add59-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="add59-183">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="add59-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="add59-184">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="add59-184">The preceding code:</span></span>

  * <span data-ttu-id="add59-185">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="add59-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="add59-186">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="add59-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="add59-187">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="add59-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="add59-188">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="add59-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="add59-190">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="add59-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="add59-192">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="add59-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-193">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="add59-194">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="add59-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="add59-195">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="add59-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="add59-196">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="add59-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="add59-197">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="add59-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR 範例應用程式](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="add59-199">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="add59-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="add59-200">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="add59-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="add59-201">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="add59-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="add59-202">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="add59-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="add59-203">![signalr.js 找不到錯誤](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="add59-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="add59-204">如果您在 Chrome 中收到錯誤 ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY，請執行下列命令來更新您的開發憑證：</span><span class="sxs-lookup"><span data-stu-id="add59-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="add59-205">後續步驟</span><span class="sxs-lookup"><span data-stu-id="add59-205">Next steps</span></span>

<span data-ttu-id="add59-206">若要深入了解 SignalR，請參閱簡介：</span><span class="sxs-lookup"><span data-stu-id="add59-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="add59-207">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="add59-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="add59-208">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="add59-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="add59-209">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="add59-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="add59-210">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="add59-210">Create a web project.</span></span>
> * <span data-ttu-id="add59-211">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="add59-212">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="add59-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="add59-213">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="add59-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="add59-214">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="add59-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="add59-215">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="add59-215">At the end, you'll have a working chat app:</span></span>

![SignalR 範例應用程式](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="add59-217">必要條件</span><span class="sxs-lookup"><span data-stu-id="add59-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-220">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="add59-221">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="add59-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="add59-223">從功能表中選取 [檔案] > [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="add59-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="add59-224">在 [新增專案] 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="add59-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="add59-225">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="add59-225">Name the project *SignalRChat*.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="add59-227">選取 [Web 應用程式] 建立使用 Razor Pages 的專案。</span><span class="sxs-lookup"><span data-stu-id="add59-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="add59-228">選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.2]，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="add59-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="add59-231">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="add59-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="add59-232">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="add59-232">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-233">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="add59-234">從功能表中選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="add59-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="add59-235">選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式] (不要選取 [ASP.NET Core Web 應用程式 (MVC)])。</span><span class="sxs-lookup"><span data-stu-id="add59-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="add59-236">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="add59-236">Select **Next**.</span></span>

* <span data-ttu-id="add59-237">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="add59-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="add59-238">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="add59-238">Add the SignalR client library</span></span>

<span data-ttu-id="add59-239">SignalR 伺服器程式庫包含在 `Microsoft.AspNetCore.App` 中繼套件內。</span><span class="sxs-lookup"><span data-stu-id="add59-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="add59-240">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="add59-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="add59-241">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="add59-242">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="add59-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="add59-244">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [新增] > [用戶端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="add59-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="add59-245">在 [新增用戶端程式庫] 對話方塊中，針對 [提供者] 選取 [unpkg]。</span><span class="sxs-lookup"><span data-stu-id="add59-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="add59-246">針對 [程式庫]，輸入 `@aspnet/signalr@1`，然後選取非預覽版的最新版本。</span><span class="sxs-lookup"><span data-stu-id="add59-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="add59-248">選取 [選擇特定檔案]、展開 [散發者/瀏覽器] 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="add59-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="add59-249">將 [目標位置] 設定為 *wwwroot/lib/signalr/* ，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="add59-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取檔案與目的地](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="add59-251">LibMan 會建立 *wwwroot/lib/signalr* 資料夾，並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="add59-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="add59-253">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="add59-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="add59-254">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="add59-255">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="add59-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="add59-256">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="add59-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="add59-257">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="add59-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="add59-258">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="add59-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="add59-259">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="add59-259">Copy only the specified files.</span></span>

  <span data-ttu-id="add59-260">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="add59-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-261">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="add59-262">在 [終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="add59-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="add59-263">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="add59-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="add59-264">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="add59-265">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="add59-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="add59-266">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="add59-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="add59-267">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="add59-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="add59-268">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="add59-268">Copy only the specified files.</span></span>

  <span data-ttu-id="add59-269">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="add59-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="add59-270">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="add59-270">Create a SignalR hub</span></span>

<span data-ttu-id="add59-271">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="add59-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="add59-272">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="add59-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="add59-273">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="add59-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="add59-274">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="add59-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="add59-275">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="add59-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="add59-276">連線的用戶端可以呼叫 `SendMessage` 方法將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="add59-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="add59-277">本教學課程稍後將示範呼叫該方法的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="add59-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="add59-278">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="add59-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="add59-279">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="add59-279">Configure SignalR</span></span>

<span data-ttu-id="add59-280">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="add59-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="add59-281">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="add59-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="add59-282">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入系統和中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="add59-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="add59-283">新增 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="add59-283">Add SignalR client code</span></span>

* <span data-ttu-id="add59-284">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="add59-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="add59-285">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="add59-285">The preceding code:</span></span>

  * <span data-ttu-id="add59-286">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="add59-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="add59-287">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="add59-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="add59-288">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="add59-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="add59-289">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="add59-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="add59-290">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="add59-290">The preceding code:</span></span>

  * <span data-ttu-id="add59-291">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="add59-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="add59-292">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="add59-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="add59-293">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="add59-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="add59-294">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="add59-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="add59-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add59-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="add59-296">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="add59-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="add59-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="add59-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="add59-298">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="add59-298">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="add59-299">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="add59-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="add59-300">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="add59-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="add59-301">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="add59-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="add59-302">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="add59-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="add59-303">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="add59-303">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR 範例應用程式](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="add59-305">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="add59-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="add59-306">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="add59-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="add59-307">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="add59-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="add59-308">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="add59-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="add59-309">![signalr.js 找不到錯誤](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="add59-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="add59-310">其他資源</span><span class="sxs-lookup"><span data-stu-id="add59-310">Additional resources</span></span>

* [<span data-ttu-id="add59-311">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="add59-311">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)

## <a name="next-steps"></a><span data-ttu-id="add59-312">後續步驟</span><span class="sxs-lookup"><span data-stu-id="add59-312">Next steps</span></span>

<span data-ttu-id="add59-313">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="add59-313">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="add59-314">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="add59-314">Create a web app project.</span></span>
> * <span data-ttu-id="add59-315">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="add59-315">Add the SignalR client library.</span></span>
> * <span data-ttu-id="add59-316">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="add59-316">Create a SignalR hub.</span></span>
> * <span data-ttu-id="add59-317">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="add59-317">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="add59-318">新增使用中樞的程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="add59-318">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="add59-319">若要深入了解 SignalR，請參閱簡介：</span><span class="sxs-lookup"><span data-stu-id="add59-319">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="add59-320">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="add59-320">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
