---
title: 開始使用ASP.NET核心SignalR
author: bradygaster
description: 在本教學中,您將創建一個使用ASP.NET酷睿的SignalR聊天應用。
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr
ms.openlocfilehash: 55ebdbfa4556deca74a6cdf0638307425cd1a01a
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78666744"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="b8aa5-103">教學課程：開始使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b8aa5-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b8aa5-104">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="b8aa5-105">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b8aa5-106">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-106">Create a web project.</span></span>
> * <span data-ttu-id="b8aa5-107">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="b8aa5-108">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="b8aa5-109">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="b8aa5-110">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="b8aa5-111">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-111">At the end, you'll have a working chat app:</span></span>

![SignalR 範例應用程式](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="b8aa5-113">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="b8aa5-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="b8aa5-117">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="b8aa5-117">Create a web app project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="b8aa5-119">從功能表中選取 [檔案] > [新增專案] \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="b8aa5-120">在 [建立新專案]\*\*\*\* 對話方塊中，選取 [ASP.NET Core Web 應用程式]\*\*\*\*，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="b8aa5-121">在 [設定新專案]\*\*\*\* 對話方塊中，將專案命名為 *SignalRChat*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="b8aa5-122">在 **"建立新ASP.NET核心 Web 應用程式**' 對話框中,選擇 **.NET 核心\*\*\*\*和ASP.NET核心 3.0**。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="b8aa5-123">選取 [Web 應用程式]\*\*\*\* 建立使用 Razor Pages 的專案，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="b8aa5-126">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="b8aa5-127">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b8aa5-129">從功能表中選取 [檔案] > [新增方案] \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="b8aa5-130">選取 [.NET Core] > [應用程式] > [Web 應用程式]\*\*\*\* (不選取 [Web 應用程式 (模型-檢視-控制器)]\*\*\*\*)，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="b8aa5-131">請確認 [目標 Framework]\*\*\*\* 設為 **.NET Core 3.0**，然後選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="b8aa5-132">將專案命名為 *SignalRChat*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="b8aa5-133">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="b8aa5-133">Add the SignalR client library</span></span>

<span data-ttu-id="b8aa5-134">SignalR 伺服器程式庫包含在 ASP.NET Core 3.0 共用架構內。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="b8aa5-135">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="b8aa5-136">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="b8aa5-137">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="b8aa5-139">在 [方案總管]\*\*\*\* 中，以滑鼠右鍵按一下專案，然後選取 [新增]**[用戶端程式庫]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="b8aa5-140">在 [新增用戶端程式庫]\*\*\*\* 對話方塊中，針對 [提供者]\*\*\*\* 選取 [unpkg]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="b8aa5-141">針對 [程式庫]\*\*\*\* 輸入 `@microsoft/signalr@latest`。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="b8aa5-142">選取 [選擇特定檔案]\*\*\*\*、展開 [散發者/瀏覽器]\*\* 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="b8aa5-143">將**目標位置**設定為*wwwroot/js/信號器/,* 然後選擇 **「安裝**」 。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="b8aa5-145">LibMan 創建*wwwroot/js/訊號資料夾*,並將選取檔案複製到該資料夾。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="b8aa5-147">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="b8aa5-148">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="b8aa5-149">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="b8aa5-150">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="b8aa5-151">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="b8aa5-152">將檔案複製到*wwwroot/js/信號器*目標。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="b8aa5-153">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-153">Copy only the specified files.</span></span>

  <span data-ttu-id="b8aa5-154">輸出看起來會像下列範例：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-155">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b8aa5-156">在 [終端機]\*\*\*\* 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="b8aa5-157">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="b8aa5-158">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="b8aa5-159">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="b8aa5-160">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="b8aa5-161">將檔案複製到*wwwroot/js/信號器*目標。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="b8aa5-162">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-162">Copy only the specified files.</span></span>

  <span data-ttu-id="b8aa5-163">輸出看起來會像下列範例：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="b8aa5-164">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="b8aa5-164">Create a SignalR hub</span></span>

<span data-ttu-id="b8aa5-165">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="b8aa5-166">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="b8aa5-167">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="b8aa5-168">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="b8aa5-169">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="b8aa5-170">連線的用戶端可以呼叫 `SendMessage` 方法將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="b8aa5-171">本教學課程稍後將示範呼叫該方法的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="b8aa5-172">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="b8aa5-173">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="b8aa5-173">Configure SignalR</span></span>

<span data-ttu-id="b8aa5-174">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="b8aa5-175">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="b8aa5-176">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入和路由系統。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="b8aa5-177">新增 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="b8aa5-177">Add SignalR client code</span></span>

* <span data-ttu-id="b8aa5-178">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="b8aa5-179">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-179">The preceding code:</span></span>

  * <span data-ttu-id="b8aa5-180">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="b8aa5-181">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="b8aa5-182">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="b8aa5-183">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="b8aa5-184">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-184">The preceding code:</span></span>

  * <span data-ttu-id="b8aa5-185">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="b8aa5-186">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="b8aa5-187">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b8aa5-188">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b8aa5-188">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b8aa5-190">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b8aa5-192">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-193">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b8aa5-194">從功能表中選取 [執行] > [啟動但不偵錯]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="b8aa5-195">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="b8aa5-196">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="b8aa5-197">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-197">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR 範例應用程式](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="b8aa5-199">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="b8aa5-200">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="b8aa5-201">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="b8aa5-202">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="b8aa5-203">![signalr.js 找不到錯誤](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="b8aa5-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="b8aa5-204">如果您在 Chrome 中收到錯誤ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY,請執行這些指令以更新您的開發證書:</span><span class="sxs-lookup"><span data-stu-id="b8aa5-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b8aa5-205">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-205">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="b8aa5-206">您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-206">You learn how to:</span></span>   

> [!div class="checklist"]  
> * <span data-ttu-id="b8aa5-207">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-207">Create a web project.</span></span>   
> * <span data-ttu-id="b8aa5-208">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-208">Add the SignalR client library.</span></span> 
> * <span data-ttu-id="b8aa5-209">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-209">Create a SignalR hub.</span></span>   
> * <span data-ttu-id="b8aa5-210">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-210">Configure the project to use SignalR.</span></span>   
> * <span data-ttu-id="b8aa5-211">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-211">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="b8aa5-212">最後,您將有工作聊天應用程式:SignalR![範例應用程式](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="b8aa5-212">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b8aa5-213">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="b8aa5-213">Prerequisites</span></span>    

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-214">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-214">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-215">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-215">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-216">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-216">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="b8aa5-217">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="b8aa5-217">Create a web project</span></span> 

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-218">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="b8aa5-219">從功能表中選取 [檔案] > [新增專案] \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-219">From the menu, select **File > New Project**.</span></span> 

* <span data-ttu-id="b8aa5-220">在 [新增專案]\*\*\*\* 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-220">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b8aa5-221">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-221">Name the project *SignalRChat*.</span></span> 

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="b8aa5-223">選取 [Web 應用程式]\*\*\*\* 建立使用 Razor Pages 的專案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-223">Select **Web Application** to create a project that uses Razor Pages.</span></span> 

* <span data-ttu-id="b8aa5-224">選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.2]\*\*\*\*，然後按一下 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-224">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>    

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-226">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-226">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="b8aa5-227">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-227">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="b8aa5-228">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-228">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-229">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-229">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="b8aa5-230">從功能表中選取 [檔案] > [新增方案] \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-230">From the menu, select **File > New Solution**.</span></span>    

* <span data-ttu-id="b8aa5-231">選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式]\*\*\*\* (不要選取 [ASP.NET Core Web 應用程式 (MVC)]\*\*\*\*)。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-231">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>  

* <span data-ttu-id="b8aa5-232">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-232">Select **Next**.</span></span>  

* <span data-ttu-id="b8aa5-233">將專案命名為 *SignalRChat*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-233">Name the project *SignalRChat*, and then select **Create**.</span></span>   

--- 

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="b8aa5-234">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="b8aa5-234">Add the SignalR client library</span></span>   

<span data-ttu-id="b8aa5-235">SignalR 伺服器程式庫包含在 `Microsoft.AspNetCore.App` 中繼套件內。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-235">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="b8aa5-236">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-236">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="b8aa5-237">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-237">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="b8aa5-238">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-238">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>    

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-239">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-239">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="b8aa5-240">在 [方案總管]\*\*\*\* 中，以滑鼠右鍵按一下專案，然後選取 [新增]**[用戶端程式庫]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-240">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>  

* <span data-ttu-id="b8aa5-241">在 [新增用戶端程式庫]\*\*\*\* 對話方塊中，針對 [提供者]\*\*\*\* 選取 [unpkg]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-241">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="b8aa5-242">針對 [程式庫]\*\*\*\*，輸入 `@microsoft/signalr@3`，然後選取不是預覽版的最新版本。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-242">For **Library**, enter `@microsoft/signalr@3`, and select the latest version that isn't preview.</span></span>  

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="b8aa5-244">選取 [選擇特定檔案]\*\*\*\*、展開 [散發者/瀏覽器]\*\* 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-244">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span> 

* <span data-ttu-id="b8aa5-245">將 [目標位置]\*\*\*\* 設定為 *wwwroot/lib/signalr/*，然後選取 [安裝]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-245">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>    

  ![[新增用戶端程式庫] 對話方塊 - 選取檔案與目的地](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="b8aa5-247">LibMan 會建立 *wwwroot/lib/signalr* 資料夾並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-247">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-248">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-248">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="b8aa5-249">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-249">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="b8aa5-250">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-250">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="b8aa5-251">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-251">You might have to wait a few seconds before seeing output.</span></span>   

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  <span data-ttu-id="b8aa5-252">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-252">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="b8aa5-253">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-253">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="b8aa5-254">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-254">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="b8aa5-255">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-255">Copy only the specified files.</span></span>  

  <span data-ttu-id="b8aa5-256">輸出看起來會像下列範例：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-256">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.0.1" to "wwwroot/lib/signalr" 
  ```   

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-257">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="b8aa5-258">在 [終端機]\*\*\*\* 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-258">In the **Terminal**, run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="b8aa5-259">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-259">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span> 

* <span data-ttu-id="b8aa5-260">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-260">Run the following command to get the SignalR client library by using LibMan.</span></span>  

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  <span data-ttu-id="b8aa5-261">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-261">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="b8aa5-262">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-262">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="b8aa5-263">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-263">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="b8aa5-264">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-264">Copy only the specified files.</span></span>  

  <span data-ttu-id="b8aa5-265">輸出看起來會像下列範例：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-265">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.x.x" to "wwwroot/lib/signalr" 
  ```   

--- 

## <a name="create-a-signalr-hub"></a><span data-ttu-id="b8aa5-266">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="b8aa5-266">Create a SignalR hub</span></span> 

<span data-ttu-id="b8aa5-267">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-267">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="b8aa5-268">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-268">In the SignalRChat project folder, create a *Hubs* folder.</span></span>    

* <span data-ttu-id="b8aa5-269">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-269">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="b8aa5-270">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-270">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="b8aa5-271">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-271">The `Hub` class manages connections, groups, and messaging.</span></span>    

  <span data-ttu-id="b8aa5-272">連線的用戶端可以呼叫 `SendMessage` 方法將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-272">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="b8aa5-273">本教學課程稍後將示範呼叫該方法的 JavaScript 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-273">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="b8aa5-274">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-274">SignalR code is asynchronous to provide maximum scalability.</span></span>  

## <a name="configure-signalr"></a><span data-ttu-id="b8aa5-275">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="b8aa5-275">Configure SignalR</span></span>    

<span data-ttu-id="b8aa5-276">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-276">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>  

* <span data-ttu-id="b8aa5-277">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-277">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="b8aa5-278">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入系統和中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-278">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>    

## <a name="add-signalr-client-code"></a><span data-ttu-id="b8aa5-279">新增 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="b8aa5-279">Add SignalR client code</span></span>  

* <span data-ttu-id="b8aa5-280">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-280">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="b8aa5-281">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-281">The preceding code:</span></span>   

  * <span data-ttu-id="b8aa5-282">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-282">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="b8aa5-283">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-283">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span> 
  * <span data-ttu-id="b8aa5-284">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-284">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>  

* <span data-ttu-id="b8aa5-285">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-285">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="b8aa5-286">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-286">The preceding code:</span></span>   

  * <span data-ttu-id="b8aa5-287">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-287">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="b8aa5-288">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-288">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="b8aa5-289">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-289">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="b8aa5-290">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b8aa5-290">Run the app</span></span>  

# <a name="visual-studio"></a>[<span data-ttu-id="b8aa5-291">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8aa5-291">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="b8aa5-292">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-292">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-code"></a>[<span data-ttu-id="b8aa5-293">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8aa5-293">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="b8aa5-294">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b8aa5-294">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b8aa5-295">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b8aa5-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b8aa5-296">從功能表中選取 [執行] > [啟動但不偵錯]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-296">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="b8aa5-297">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-297">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="b8aa5-298">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-298">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="b8aa5-299">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-299">The name and message are displayed on both pages instantly.</span></span>   

  <span data-ttu-id="b8aa5-300">![SignalR範例應用程式](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="b8aa5-300">![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span> 

> [!TIP]    
> <span data-ttu-id="b8aa5-301">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-301">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="b8aa5-302">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-302">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="b8aa5-303">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-303">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="b8aa5-304">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8aa5-304">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="b8aa5-305">![signalr.js 找不到錯誤](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="b8aa5-305">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="b8aa5-306">其他資源</span><span class="sxs-lookup"><span data-stu-id="b8aa5-306">Additional resources</span></span> 
* [<span data-ttu-id="b8aa5-307">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="b8aa5-307">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

::: moniker-end
