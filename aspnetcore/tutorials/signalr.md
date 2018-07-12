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
# <a name="get-started-with-signalr-on-aspnet-core"></a>SignalR on ASP.NET Core 使用者入門

作者：[Rachel Appel](https://twitter.com/rachelappel)

本教學課程將教導您使用 SignalR for ASP.NET Core 建置即時應用程式的基本概念。

   ![方案](signalr/_static/signalr-get-started-finished.png)

本教學課程示範下列 SignalR 開發工作：

> [!div class="checklist"]
> * 建立 SignalR on ASP.NET Core Web 應用程式。
> * 建立 SignalR 中樞將內容推送至用戶端。
> * 修改 `Startup` 類別，並設定應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

安裝下列軟體：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 版或更新版本，包含 **ASP.NET 及網頁程式開發**工作負載
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.1 或更新版本](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>建立裝載 SignalR 用戶端和伺服器的 ASP.NET Core 專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 使用 [檔案] > [新增專案] 功能表選項，然後選擇 [ASP.NET Core Web 應用程式]。 將專案命名為 *SignalRChat*。

   ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-dialog.png)

2. 選取 [Web 應用程式] 使用 Razor Pages 建立專案。 然後選取 [確定]。 確定從架構選取器選取 [ASP.NET Core 2.1]，雖然 SignalR 在較舊版本的 .NET 上執行。

   ![Visual Studio 的 [新增專案] 對話方塊](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio 包含 `Microsoft.AspNetCore.SignalR` 套件，它的 **ASP.NET Core Web 應用程式**範本當中包含其伺服器程式庫。 不過，必須使用 *npm* 安裝適用於 SignalR 的 JavaScript 用戶端程式庫。

3. 在 [套件管理員主控台] 視窗中，從專案根目錄中執行下列命令：

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. 在專案的 *lib* 資料夾中，建立名為"signalr" 的新資料夾。 將 *signalr.js* 檔案從 *node_modules\\@aspnet\signalr\dist\browser* 複製到這個資料夾。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. 從 [整合式終端機] 執行下列命令：

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. 使用 *npm* 安裝 JavaScript 用戶端程式庫。

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. 在專案的 *lib* 資料夾中，建立名為"signalr" 的新資料夾。 將 *signalr.js* 檔案從 *node_modules\\@aspnet\signalr\dist\browser* 複製到這個資料夾。

---

## <a name="create-the-signalr-hub"></a>建立 SignalR 中樞

中樞是作為高階管線的類別，可讓用戶端與伺服器對彼此呼叫方法。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 將類別新增至專案中，方法是選擇 [檔案] > [新增] > [檔案]，然後選取 [Visual C# 類別]。 將類別命名為 `ChatHub`，並將檔案命名為 *ChatHub.cs*。

2. 繼承自 `Microsoft.AspNetCore.SignalR.Hub`。 `Hub` 類別包含屬性和事件以便管理連線和群組，以及傳送和接收資料。

3. 建立 `SendMessage` 方法，以傳送訊息給所有已連線的交談用戶端。 請注意，它會傳回 [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx) (工作)，因為 SignalR 為非同步。 非同步程式碼較易調整大小。

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. 在 Visual Studio Code 中開啟 *SignalRChat* 資料夾。

2. 將類別新增至專案中，方法是從功能表選取 [檔案] > [新增檔案]。 將類別命名為 `ChatHub`，並將檔案命名為 *ChatHub.cs*。

3. 繼承自 `Microsoft.AspNetCore.SignalR.Hub`。 `Hub` 類別包含屬性和事件以便管理連線和群組，以及傳送和接收資料至用戶端。

4. 將 `SendMessage` 方法加入類別中。 `SendMessage` 方法會傳送訊息給所有已連線的交談用戶端。 請注意，它會傳回 [Task](/dotnet/api/system.threading.tasks.task) (工作)，因為 SignalR 為非同步。 非同步程式碼較易調整大小。

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>設定專案以使用 SignalR

SignalR 伺服器必須設定，它才知道要傳遞要求給 SignalR。

1. 若要設定 SignalR 專案，請修改專案的 `Startup.ConfigureServices` 方法。

   `services.AddSignalR` 可讓[相依性插入](xref:fundamentals/dependency-injection)系統使用 SignalR 服務。

1. 在 `Configure` 方法中使用 `UseSignalR` 來設定中樞的路由。 `app.UseSignalR` 將 SignalR 新增到[中介軟體](xref:fundamentals/middleware/index)管線。

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>建立 SignalR 用戶端程式碼

1. 將名為 *chat.js* 的 JavaScript 檔案新增至 *wwwroot\js* 資料夾。 將下列程式碼加入該檔案：

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. 以下列程式碼取代 *Pages\Index.cshtml* 中的程式碼：

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   前面的 HTML 會顯示名稱和訊息欄位，以及一個提交按鈕。 請注意底部的指令碼參考：SignalR 和 *chat.js* 的參考。

## <a name="run-the-app"></a>執行應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 選取 [偵錯] > [啟動但不偵錯] 啟動瀏覽器，並載入本機網站。 從網址列複製 URL。

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)，並在網址列中貼上 URL。

1. 選擇任一個瀏覽器、輸入名稱和訊息，然後按一下 [傳送] 按鈕。 名稱和訊息會立即顯示在兩個頁面上。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 按 [偵錯] (F5) 以建置並執行程式。 執行程式會開啟瀏覽器視窗。

1. 開啟另一個瀏覽器視窗，並在其中載入本機網站。

1. 選擇任一個瀏覽器、輸入名稱和訊息，然後按一下 [傳送] 按鈕。 名稱和訊息會立即顯示在兩個頁面上。

---

  ![方案](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>相關資源

[ASP.NET Core SignalR 簡介](xref:signalr/introduction)
