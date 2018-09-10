---
title: 教學課程：SignalR on ASP.NET Core 使用者入門
author: tdykstra
description: 在本教學課程中，您會建立使用 SignalR for ASP.NET Core 的聊天應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055828"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>教學課程：SignalR on ASP.NET Core 使用者入門

本教學課程將教授使用 SignalR 建置即時應用程式的基本概念。 您將學習如何：

> [!div class="checklist"]
> * 建立使用 SignalR on ASP.NET Core 的 Web 應用程式。
> * 在伺服器上建立 SignalR 中樞。
> * 從 JavaScript 用戶端連線到 SignalR 中樞。
> * 使用中樞，將訊息從任何用戶端傳送至所有連線的用戶端。

最後，您會有一個運作正常的聊天應用程式：

![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 15.7.3 版或更新版本](https://www.visualstudio.com/downloads/)，包含 **ASP.NET 及網頁程式開發**工作負載
* [.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (Node.js 的套件管理員，用於 SignalR JavaScript 用戶端程式庫。)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all)
* [C# for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (Node.js 的套件管理員，用於 SignalR JavaScript 用戶端程式庫。)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Visual Studio for Mac 7.5.4 版或更新版本](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all) (隨附於 Visual Studio 安裝)
* [npm](https://www.npmjs.com/get-npm) (Node.js 的套件管理員，用於 SignalR JavaScript 用戶端程式庫。)

---

## <a name="create-the-project"></a>建立專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* 從功能表中選取 [檔案] > [新增專案] 。

* 在 [新增專案] 對話方塊中，選取 [已安裝] > [Visual C++] > [Web] > [ASP.NET Core Web 應用程式]。 將專案命名為 *SignalRChat*。

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-dialog.png)

* 選取 [Web 應用程式] 建立使用 Razor Pages 的專案。

* 選取 **.NET Core** 作為目標 Framework、選取 [ASP.NET Core 2.1]，然後按一下 [確定]。

  ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* 開啟您可以用於新專案的資料夾。

* 在 [整合式終端機] 中執行下列命令：

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 從功能表中選取 [檔案] > [新增方案] 。

* 選取 [.NET Core] > [應用程式] > [ASP.NET Core Web 應用程式] (不要選取 [ASP.NET Core Web 應用程式 (MVC)])。

* 選取 [下一步]。

* 將專案命名為 *SignalRChat*，然後選取 [建立]。

---

## <a name="add-the-signalr-client-library"></a>新增 SignalR 用戶端程式庫

SignalR 伺服器程式庫包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)內。 但是，您必須從 [npm 這個 Node.js 套件管理員](https://www.npmjs.com/get-npm)中取得 JavaScript 用戶端程式庫。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* 在 [套件管理員主控台] (PMC) 中，切換至專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. 切換至新的專案資料夾。

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在 [終端機] 中，巡覽至專案資料夾 (包含 *SignalRChat.csproj* 檔案的資料夾)。

---

* 執行 npm 初始設定式來建立 *package.json* 檔案：

  ```console
  npm init -y
  ```

  該命令會建立類似下列範例的輸出：

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

* 安裝用戶端程式庫套件：

  ```console
  npm install @aspnet/signalr
  ```

  該命令會建立類似下列範例的輸出：

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

`npm install` 命令已將 JavaScript 用戶端程式庫下載到 *node_modules* 下的子資料夾。 將其從該處複製到 *wwwroot* 下您可以從聊天應用程式網頁參考的資料夾。

* 在 *wwwroot/lib* 中建立 *signalr* 資料夾。

* 將 *signalr.js* 檔案從 *node_modules/@aspnet/signalr/dist/browser* 複製到新的 *wwwroot/lib/signalr* 資料夾。

## <a name="create-the-signalr-hub"></a>建立 SignalR 中樞

[中樞](xref:signalr/hubs)類別可提供作為高階管線，用來處理用戶端/伺服器通訊。

* 在 SignalRChat 專案資料夾中，建立 *Hubs* 資料夾。

* 在 *Hubs* 資料夾中，建立含有下列程式碼的 *ChatHub.cs* 檔案：

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  `ChatHub` 類別繼承自 SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) 類別。 `Hub` 類別管理連線、群組和傳訊。

  任何連線的用戶端都可以呼叫 `SendMessage` 方法。 它會將接收的訊息傳送至所有用戶端。 SignalR 程式碼是以非同步方式來提供最大的延展性。

## <a name="configure-the-project-to-use-signalr"></a>設定專案以使用 SignalR

SignalR 伺服器必須設定為將 SignalR 要求傳遞給 SignalR。

* 將下列醒目提示的程式碼新增至 *Startup.cs* 檔案。

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  這些變更會將 SignalR 新增至[相依性插入](xref:fundamentals/dependency-injection)系統和[中介軟體](xref:fundamentals/middleware/index)管線。

## <a name="create-the-signalr-client-code"></a>建立 SignalR 用戶端程式碼

* 以下列程式碼取代 *Pages\Index.cshtml* 中的內容：

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  上述程式碼：

  * 建立名稱和訊息文字的文字方塊，以及提交按鈕。
  * 使用 `id="messagesList"` 建立清單，用於顯示接收自 SignalR 中樞的訊息。
  * 包含 SignalR 的指令碼參考和您在下一個步驟中建立的 *chat.js* 應用程式程式碼。

* 在 *wwwroot/js* 資料夾中，建立含有下列程式碼的 *chat.js* 檔案：

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  上述程式碼：

  * 建立並啟動連線。
  * 新增處理常式至提交按鈕，以將訊息傳送至中樞。
  * 新增處理常式至連線物件，以從中樞接收訊息，並將它們新增至清單。

## <a name="run-the-app"></a>執行應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 按 **CTRL+F5** 即可執行應用程式而不偵錯。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 按 **CTRL+F5** 即可執行應用程式而不偵錯。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 從功能表中選取 [執行] > [啟動但不偵錯]。

---

* 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

* 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送] 按鈕。

  名稱和訊息會立即顯示在兩個頁面上。

  ![SignalR 範例應用程式](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> 如果應用程式無法運作，請開啟您的瀏覽器開發人員工具 (F12)，然後移至主控台。 您可能會看到與 HTML 和 JavaScript 程式碼相關的錯誤。 例如，假設您將 *signalr.js* 放置在與指示不同的資料夾中。 在此情況下，該檔案的參考無法運作，您會在主控台中看到 404 錯誤。
> ![signalr.js 找不到錯誤](signalr/_static/f12-console.png)

## <a name="next-steps"></a>後續步驟

如果您想要用戶端連線到不同網域中的 SignalR 應用程式，您必須啟用跨原始資源共用 (CORS)。 如需詳細資訊，請參閱[跨原始資源共用](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing)。

若要深入了解 SignalR、中樞及 JavaScript 用戶端，請參閱下列資源：

* [SignalR for ASP.NET Core 簡介](xref:signalr/introduction)
* [使用 SignalR for ASP.NET Core 的中樞](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript 用戶端](xref:signalr/javascript-client)
