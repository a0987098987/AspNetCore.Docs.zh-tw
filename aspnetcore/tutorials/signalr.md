---
title: 開始使用 ASP.NET Core SignalR
author: bradygaster
description: 在本教學課程中，您會建立使用 ASP.NET Core SignalR的聊天應用程式。
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: tutorials/signalr
ms.openlocfilehash: ac727ed0517a8b30fd8194c010576fdd74a5950a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2019
ms.locfileid: "74052861"
---
# <a name="tutorial-get-started-with-aspnet-core-opno-locsignalr"></a><span data-ttu-id="f5a80-103">教學課程：開始使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f5a80-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f5a80-104">本教學課程將教您使用 SignalR建立即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f5a80-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="f5a80-105">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="f5a80-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f5a80-106">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-106">Create a web project.</span></span>
> * <span data-ttu-id="f5a80-107">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="f5a80-108">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="f5a80-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="f5a80-109">將專案設定為使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f5a80-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="f5a80-110">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f5a80-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="f5a80-111">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="f5a80-111">At the end, you'll have a working chat app:</span></span>

![[!OP.無-LOC （SignalR）]<span data-ttu-id="f5a80-112"> 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f5a80-112"> sample app</span></span>](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="f5a80-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="f5a80-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="f5a80-117">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="f5a80-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="f5a80-119">從功能表中選取 [檔案] > [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="f5a80-120">在 [建立新專案] 對話方塊中，選取 [ASP.NET Core Web 應用程式]，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="f5a80-121">在 [設定新專案] 對話方塊中，將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="f5a80-122">在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中，選取 [ **.net Core** ] 和 [ **ASP.NET Core 3.0**]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="f5a80-123">選取 [Web 應用程式] 建立使用 Razor Pages 的專案，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="f5a80-126">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f5a80-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="f5a80-127">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f5a80-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f5a80-129">從功能表中選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="f5a80-130">選取 [.NET Core] > [應用程式] > [Web 應用程式] (不選取 [Web 應用程式 (模型-檢視-控制器)])，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="f5a80-131">請確認 [目標 Framework] 設為 **.NET Core 3.0**，然後選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="f5a80-132">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="f5a80-133">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="f5a80-133">Add the SignalR client library</span></span>

<span data-ttu-id="f5a80-134">SignalR 伺服器程式庫包含在 ASP.NET Core 3.0 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="f5a80-135">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="f5a80-136">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="f5a80-137">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="f5a80-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="f5a80-139">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [新增] > [用戶端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="f5a80-140">在 [新增用戶端程式庫] 對話方塊中，針對 [提供者] 選取 [unpkg]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="f5a80-141">針對 [程式庫] 輸入 `@microsoft/signalr@latest`。</span><span class="sxs-lookup"><span data-stu-id="f5a80-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="f5a80-142">選取 [選擇特定檔案]、展開 [散發者/瀏覽器] 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="f5a80-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="f5a80-143">將 [**目標位置**] 設定為*wwwroot/js/signalr/* ，然後選取 [**安裝**]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="f5a80-145">LibMan 會建立*wwwroot/js/signalr*資料夾，並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="f5a80-147">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="f5a80-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="f5a80-148">執行下列命令，使用 LibMan 取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="f5a80-149">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="f5a80-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="f5a80-150">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="f5a80-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="f5a80-151">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="f5a80-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="f5a80-152">將檔案複製到*wwwroot/js/signalr*目的地。</span><span class="sxs-lookup"><span data-stu-id="f5a80-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="f5a80-153">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-153">Copy only the specified files.</span></span>

  <span data-ttu-id="f5a80-154">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="f5a80-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f5a80-156">在 [終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="f5a80-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="f5a80-157">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="f5a80-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="f5a80-158">執行下列命令，使用 LibMan 取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="f5a80-159">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="f5a80-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="f5a80-160">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="f5a80-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="f5a80-161">將檔案複製到*wwwroot/js/signalr*目的地。</span><span class="sxs-lookup"><span data-stu-id="f5a80-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="f5a80-162">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-162">Copy only the specified files.</span></span>

  <span data-ttu-id="f5a80-163">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="f5a80-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="f5a80-164">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="f5a80-164">Create a SignalR hub</span></span>

<span data-ttu-id="f5a80-165">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="f5a80-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="f5a80-166">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f5a80-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="f5a80-167">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f5a80-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="f5a80-168">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="f5a80-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="f5a80-169">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="f5a80-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="f5a80-170">連線的用戶端可以呼叫 `SendMessage` 方法將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="f5a80-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="f5a80-171">本教學課程稍後將示範呼叫該方法的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f5a80-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> SignalR<span data-ttu-id="f5a80-172"> 程式碼是非同步，可提供最大的擴充性。</span><span class="sxs-lookup"><span data-stu-id="f5a80-172"> code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="f5a80-173">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="f5a80-173">Configure SignalR</span></span>

<span data-ttu-id="f5a80-174">SignalR 伺服器必須設定為將 SignalR 要求傳遞至 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f5a80-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="f5a80-175">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="f5a80-176">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入和路由系統。</span><span class="sxs-lookup"><span data-stu-id="f5a80-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="f5a80-177">新增 SignalR 用戶端程式代碼</span><span class="sxs-lookup"><span data-stu-id="f5a80-177">Add SignalR client code</span></span>

* <span data-ttu-id="f5a80-178">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5a80-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="f5a80-179">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5a80-179">The preceding code:</span></span>

  * <span data-ttu-id="f5a80-180">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5a80-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="f5a80-181">建立包含 `id="messagesList"` 的清單，以顯示從 SignalR 中樞接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="f5a80-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="f5a80-182">包含 SignalR 的腳本參考，以及您在下一個步驟中建立的*聊天 .js*應用程式代碼。</span><span class="sxs-lookup"><span data-stu-id="f5a80-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="f5a80-183">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f5a80-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="f5a80-184">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5a80-184">The preceding code:</span></span>

  * <span data-ttu-id="f5a80-185">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="f5a80-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="f5a80-186">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="f5a80-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="f5a80-187">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="f5a80-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="f5a80-188">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f5a80-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f5a80-190">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="f5a80-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f5a80-192">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f5a80-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-193">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f5a80-194">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="f5a80-195">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="f5a80-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="f5a80-196">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5a80-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="f5a80-197">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="f5a80-197">The name and message are displayed on both pages instantly.</span></span>

  ![[!OP.無-LOC （SignalR）]<span data-ttu-id="f5a80-198"> 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f5a80-198"> sample app</span></span>](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="f5a80-199">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="f5a80-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="f5a80-200">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5a80-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="f5a80-201">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="f5a80-202">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5a80-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="f5a80-203">![signalr.js 找不到錯誤](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="f5a80-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="f5a80-204">如果您在 Chrome 中收到錯誤 ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY，請執行下列命令來更新您的開發憑證：</span><span class="sxs-lookup"><span data-stu-id="f5a80-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f5a80-205">本教學課程將教您使用 SignalR建立即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f5a80-205">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="f5a80-206">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="f5a80-206">You learn how to:</span></span> 

> [!div class="checklist"]  
> * <span data-ttu-id="f5a80-207">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-207">Create a web project.</span></span>   
> * <span data-ttu-id="f5a80-208">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-208">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="f5a80-209">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="f5a80-209">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="f5a80-210">將專案設定為使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f5a80-210">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="f5a80-211">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f5a80-211">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="f5a80-212">最後，您將會有一個可運作的聊天應用程式： ![[！OP.無-LOC （SignalR）] 範例應用程式](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="f5a80-212">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="f5a80-213">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="f5a80-213">Prerequisites</span></span>    

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-214">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-214">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-215">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-215">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-216">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-216">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="f5a80-217">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="f5a80-217">Create a web project</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-218">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="f5a80-219">從功能表中選取 [檔案] > [新增專案]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-219">From the menu, select **File > New Project**.</span></span> 

* <span data-ttu-id="f5a80-220">在 [新增專案] 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-220">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f5a80-221">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="f5a80-221">Name the project *SignalRChat*.</span></span> 

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="f5a80-223">選取 [Web 應用程式] 建立使用 Razor Pages 的專案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-223">Select **Web Application** to create a project that uses Razor Pages.</span></span> 

* <span data-ttu-id="f5a80-224">選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.2]，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-224">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>    

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-226">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-226">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="f5a80-227">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f5a80-227">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="f5a80-228">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f5a80-228">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-229">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-229">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="f5a80-230">從功能表中選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-230">From the menu, select **File > New Solution**.</span></span>    

* <span data-ttu-id="f5a80-231">選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式] (不要選取 [ASP.NET Core Web 應用程式 (MVC)])。</span><span class="sxs-lookup"><span data-stu-id="f5a80-231">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>  

* <span data-ttu-id="f5a80-232">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-232">Select **Next**.</span></span>  

* <span data-ttu-id="f5a80-233">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-233">Name the project *SignalRChat*, and then select **Create**.</span></span>   

--- 

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="f5a80-234">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="f5a80-234">Add the SignalR client library</span></span> 

<span data-ttu-id="f5a80-235">SignalR 伺服器程式庫包含在 `Microsoft.AspNetCore.App` 中繼套件中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-235">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="f5a80-236">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-236">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="f5a80-237">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-237">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="f5a80-238">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="f5a80-238">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-239">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-239">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="f5a80-240">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [新增] > [用戶端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-240">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>  

* <span data-ttu-id="f5a80-241">在 [新增用戶端程式庫] 對話方塊中，針對 [提供者] 選取 [unpkg]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-241">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="f5a80-242">針對 [程式庫]，輸入 `@aspnet/signalr@1`，然後選取非預覽版的最新版本。</span><span class="sxs-lookup"><span data-stu-id="f5a80-242">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span> 

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="f5a80-244">選取 [選擇特定檔案]、展開 [散發者/瀏覽器] 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="f5a80-244">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span> 

* <span data-ttu-id="f5a80-245">將 [目標位置] 設定為 *wwwroot/lib/signalr/* ，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-245">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>    

  ![[新增用戶端程式庫] 對話方塊 - 選取檔案與目的地](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="f5a80-247">LibMan 會建立 *wwwroot/lib/signalr* 資料夾，並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-247">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-248">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-248">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="f5a80-249">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="f5a80-249">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="f5a80-250">執行下列命令，使用 LibMan 取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-250">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="f5a80-251">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="f5a80-251">You might have to wait a few seconds before seeing output.</span></span> 

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="f5a80-252">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="f5a80-252">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="f5a80-253">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="f5a80-253">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="f5a80-254">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="f5a80-254">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="f5a80-255">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-255">Copy only the specified files.</span></span>  

  <span data-ttu-id="f5a80-256">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="f5a80-256">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-257">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="f5a80-258">在 [終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="f5a80-258">In the **Terminal**, run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="f5a80-259">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="f5a80-259">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span> 

* <span data-ttu-id="f5a80-260">執行下列命令，使用 LibMan 取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="f5a80-260">Run the following command to get the SignalR client library by using LibMan.</span></span>    

  ```console    
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js    
  ```   

  <span data-ttu-id="f5a80-261">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="f5a80-261">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="f5a80-262">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="f5a80-262">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="f5a80-263">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="f5a80-263">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="f5a80-264">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-264">Copy only the specified files.</span></span>  

  <span data-ttu-id="f5a80-265">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="f5a80-265">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"    
  ```   

--- 

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="f5a80-266">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="f5a80-266">Create a SignalR hub</span></span>   

<span data-ttu-id="f5a80-267">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="f5a80-267">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="f5a80-268">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f5a80-268">In the SignalRChat project folder, create a *Hubs* folder.</span></span>    

* <span data-ttu-id="f5a80-269">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f5a80-269">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="f5a80-270">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="f5a80-270">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="f5a80-271">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="f5a80-271">The `Hub` class manages connections, groups, and messaging.</span></span>  

  <span data-ttu-id="f5a80-272">連線的用戶端可以呼叫 `SendMessage` 方法將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="f5a80-272">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="f5a80-273">本教學課程稍後將示範呼叫該方法的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f5a80-273">JavaScript client code that calls the method is shown later in the tutorial.</span></span> SignalR<span data-ttu-id="f5a80-274"> 程式碼是非同步，可提供最大的擴充性。</span><span class="sxs-lookup"><span data-stu-id="f5a80-274"> code is asynchronous to provide maximum scalability.</span></span>    

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="f5a80-275">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="f5a80-275">Configure SignalR</span></span>  

<span data-ttu-id="f5a80-276">SignalR 伺服器必須設定為將 SignalR 要求傳遞至 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f5a80-276">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>    

* <span data-ttu-id="f5a80-277">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f5a80-277">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="f5a80-278">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入系統和中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="f5a80-278">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>  

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="f5a80-279">新增 SignalR 用戶端程式代碼</span><span class="sxs-lookup"><span data-stu-id="f5a80-279">Add SignalR client code</span></span>    

* <span data-ttu-id="f5a80-280">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5a80-280">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="f5a80-281">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5a80-281">The preceding code:</span></span>   

  * <span data-ttu-id="f5a80-282">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5a80-282">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="f5a80-283">建立包含 `id="messagesList"` 的清單，以顯示從 SignalR 中樞接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="f5a80-283">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>   
  * <span data-ttu-id="f5a80-284">包含 SignalR 的腳本參考，以及您在下一個步驟中建立的*聊天 .js*應用程式代碼。</span><span class="sxs-lookup"><span data-stu-id="f5a80-284">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>    

* <span data-ttu-id="f5a80-285">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f5a80-285">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="f5a80-286">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5a80-286">The preceding code:</span></span>   

  * <span data-ttu-id="f5a80-287">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="f5a80-287">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="f5a80-288">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="f5a80-288">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="f5a80-289">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="f5a80-289">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="f5a80-290">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f5a80-290">Run the app</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f5a80-291">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5a80-291">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="f5a80-292">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="f5a80-292">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f5a80-293">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f5a80-293">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="f5a80-294">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f5a80-294">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli  
  dotnet run -p SignalRChat.csproj  
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f5a80-295">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f5a80-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="f5a80-296">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="f5a80-296">From the menu, select **Run > Start Without Debugging**.</span></span>  

--- 

* <span data-ttu-id="f5a80-297">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="f5a80-297">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>    

* <span data-ttu-id="f5a80-298">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5a80-298">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="f5a80-299">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="f5a80-299">The name and message are displayed on both pages instantly.</span></span>   

  ![[!OP.無-LOC （SignalR）]<span data-ttu-id="f5a80-300"> 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f5a80-300"> sample app</span></span>](signalr/_static/2.x/signalr-get-started-finished.png) 

> [!TIP]    
> <span data-ttu-id="f5a80-301">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="f5a80-301">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="f5a80-302">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5a80-302">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="f5a80-303">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="f5a80-303">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="f5a80-304">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f5a80-304">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="f5a80-305">![signalr.js 找不到錯誤](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="f5a80-305">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="f5a80-306">其他資源</span><span class="sxs-lookup"><span data-stu-id="f5a80-306">Additional resources</span></span> 
* [<span data-ttu-id="f5a80-307">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="f5a80-307">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

::: moniker-end
