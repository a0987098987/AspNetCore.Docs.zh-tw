---
title: 開始使用 ASP.NET Core SignalR
author: tdykstra
description: 在本教學課程中，您會建立使用 ASP.NET Core SignalR 的聊天應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: c52041b34d6c9d1d8f06f980c900b805a0933293
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861976"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="6ab6a-103">教學課程：開始使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6ab6a-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="6ab6a-104">本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="6ab6a-105">您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6ab6a-106">建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-106">Create a web project.</span></span>
> * <span data-ttu-id="6ab6a-107">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="6ab6a-108">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="6ab6a-109">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="6ab6a-110">新增程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="6ab6a-111">最後，您會有一個運作正常的聊天應用程式：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-111">At the end, you'll have a working chat app:</span></span>

![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="6ab6a-113">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="6ab6a-114">我們正為規劃中的 ASP.NET Core 目錄新結構測試其可用性。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="6ab6a-115">如果您有幾分鐘的時間可以嘗試在目前或建議的目錄中尋找 7 個不同主題，請[按一下這裡參加研究](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>


[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="6ab6a-116">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="6ab6a-116">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6ab6a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ab6a-117">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6ab6a-118">從功能表中選取 [檔案] > [新增專案] 。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-118">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="6ab6a-119">在 [新增專案] 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-119">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6ab6a-120">將專案命名為 *SignalRChat*。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-120">Name the project *SignalRChat*.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="6ab6a-122">選取 [Web 應用程式] 建立使用 Razor Pages 的專案。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-122">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="6ab6a-123">選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.2]，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-123">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6ab6a-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6ab6a-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6ab6a-126">對要建立新專案資料夾所在的資料夾，開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="6ab6a-127">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6ab6a-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6ab6a-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6ab6a-129">從功能表中選取 [檔案] > [新增方案] 。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="6ab6a-130">選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式] (不要選取 [ASP.NET Core Web 應用程式 (MVC)])。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-130">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="6ab6a-131">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-131">Select **Next**.</span></span>

* <span data-ttu-id="6ab6a-132">將專案命名為 *SignalRChat*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="6ab6a-133">新增 SignalR 用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="6ab6a-133">Add the SignalR client library</span></span>

<span data-ttu-id="6ab6a-134">SignalR 伺服器程式庫包含在 `Microsoft.AspNetCore.App` 中繼套件內。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-134">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="6ab6a-135">JavaScript 用戶端程式庫不會自動包括在專案中。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="6ab6a-136">針對此教學課程，您會使用程式庫管理員 (LibMan) 從 *unpkg* 取得用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="6ab6a-137">unpkg 是一個內容傳遞網路 (CDN)，可以傳遞在 Node.js 套件管理員 (npm) 中找到的任何項目。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6ab6a-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ab6a-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6ab6a-139">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [新增] > [用戶端程式庫]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="6ab6a-140">在 [新增用戶端程式庫] 對話方塊中，針對 [提供者] 選取 [unpkg]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="6ab6a-141">針對 [程式庫]，輸入 `@aspnet/signalr@1`，然後選取非預覽版的最新版本。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-141">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取程式庫](signalr/_static/libman1.png)

* <span data-ttu-id="6ab6a-143">選取 [選擇特定檔案]、展開 [散發者/瀏覽器] 資料夾，然後選取 *signalr.js* 與 *signalr.min.js*。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-143">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="6ab6a-144">將 [目標位置] 設定為 *wwwroot/lib/signalr/*，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-144">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![[新增用戶端程式庫] 對話方塊 - 選取檔案與目的地](signalr/_static/libman2.png)

  <span data-ttu-id="6ab6a-146">LibMan 會建立 *wwwroot/lib/signalr* 資料夾，並將選取的檔案複製到其中。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-146">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6ab6a-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6ab6a-147">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6ab6a-148">在整合式終端機中，執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-148">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="6ab6a-149">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-149">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="6ab6a-150">您可能必須等幾秒鐘，才會看到輸出。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-150">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="6ab6a-151">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-151">The parameters specify the following options:</span></span>
  * <span data-ttu-id="6ab6a-152">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-152">Use the unpkg provider.</span></span>
  * <span data-ttu-id="6ab6a-153">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-153">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="6ab6a-154">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-154">Copy only the specified files.</span></span>

  <span data-ttu-id="6ab6a-155">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-155">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6ab6a-156">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6ab6a-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6ab6a-157">在 [終端機] 中執行下列命令以安裝 LibMan。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-157">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="6ab6a-158">瀏覽到專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="6ab6a-159">執行下列命令以透過使用 LibMan 來取得 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-159">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="6ab6a-160">參數指定下列選項：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="6ab6a-161">使用 unpkg 提供者。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="6ab6a-162">將檔案複製到 *wwwroot/lib/signalr* 目的地。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="6ab6a-163">只複製指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-163">Copy only the specified files.</span></span>

  <span data-ttu-id="6ab6a-164">輸出看起來會像下列範例這樣：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="6ab6a-165">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="6ab6a-165">Create a SignalR hub</span></span>

<span data-ttu-id="6ab6a-166">*中樞*類別可提供作為高階管線，用來處理用戶端/伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-166">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="6ab6a-167">在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-167">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="6ab6a-168">在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-168">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="6ab6a-169">`ChatHub` 類別繼承自 SignalR `Hub` 類別。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-169">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="6ab6a-170">`Hub` 類別管理連線、群組和傳訊。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-170">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="6ab6a-171">任何連線的用戶端都可以呼叫 `SendMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-171">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="6ab6a-172">它會將接收的訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-172">It sends the received message to all clients.</span></span> <span data-ttu-id="6ab6a-173">SignalR 程式碼是以非同步方式來提供最大的延展性。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-173">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="6ab6a-174">設定 SignalR</span><span class="sxs-lookup"><span data-stu-id="6ab6a-174">Configure SignalR</span></span>

<span data-ttu-id="6ab6a-175">SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-175">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="6ab6a-176">將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-176">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="6ab6a-177">這些變更會將 SignalR 新增至 ASP.NET Core 相依性插入系統和中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-177">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="6ab6a-178">新增 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="6ab6a-178">Add SignalR client code</span></span>

* <span data-ttu-id="6ab6a-179">以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-179">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="6ab6a-180">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-180">The preceding code:</span></span>

  * <span data-ttu-id="6ab6a-181">建立名稱和訊息文字的文字方塊，以及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-181">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="6ab6a-182">使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-182">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="6ab6a-183">包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-183">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="6ab6a-184">在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-184">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="6ab6a-185">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-185">The preceding code:</span></span>

  * <span data-ttu-id="6ab6a-186">建立並啟動連線。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-186">Creates and starts a connection.</span></span>
  * <span data-ttu-id="6ab6a-187">新增處理常式至提交按鈕，以將訊息傳送至中樞。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-187">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="6ab6a-188">新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-188">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6ab6a-189">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="6ab6a-189">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6ab6a-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ab6a-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6ab6a-191">按 **CTRL+F5** 即可執行應用程式而不偵錯。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-191">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6ab6a-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6ab6a-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6ab6a-193">在整合式終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-193">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6ab6a-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6ab6a-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6ab6a-195">從功能表中選取 [執行] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-195">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="6ab6a-196">從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-196">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="6ab6a-197">選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送訊息] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-197">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="6ab6a-198">名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-198">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="6ab6a-200">如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-200">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="6ab6a-201">您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-201">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="6ab6a-202">例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-202">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="6ab6a-203">在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-203">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="6ab6a-204">![signalr.js 找不到錯誤](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="6ab6a-204">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ab6a-205">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6ab6a-205">Next steps</span></span>

<span data-ttu-id="6ab6a-206">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-206">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6ab6a-207">建立 Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-207">Create a web app project.</span></span>
> * <span data-ttu-id="6ab6a-208">新增 SignalR 用戶端程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-208">Add the SignalR client library.</span></span>
> * <span data-ttu-id="6ab6a-209">建立 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-209">Create a SignalR hub.</span></span>
> * <span data-ttu-id="6ab6a-210">設定專案以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-210">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="6ab6a-211">新增使用中樞的程式碼，以將訊息從任何用戶端傳送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6ab6a-211">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="6ab6a-212">若要深入了解 SignalR，請參閱簡介：</span><span class="sxs-lookup"><span data-stu-id="6ab6a-212">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6ab6a-213">ASP.NET Core SignalR 簡介</span><span class="sxs-lookup"><span data-stu-id="6ab6a-213">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
