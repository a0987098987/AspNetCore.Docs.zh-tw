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
ms.openlocfilehash: fb448f22db5bb624083f07cefaad29d6d58331ef
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>開始使用 ASP.NET Core 的 SignalR

作者：[Rachel Appel](https://twitter.com/rachelappel)

本教學課程將教導您建置適用於 ASP.NET Core 使用 SignalR 的即時應用程式的基本概念。

   ![方案](get-started/_static/signalr-get-started-finished.png)

本教學課程將示範下列 SignalR 開發工作：

> [!div class="checklist"]
> * 建立 SignalR ASP.NET Core web 應用程式上。
> * 建立 SignalR 中樞內容推播至用戶端。
> * 修改`Startup`類別，並設定應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>必要條件

安裝下列軟體：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1)或更新版本
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 或更新版本的版本**ASP.NET 及 web 開發**工作負載
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1)或更新版本
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio 程式碼的 C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>建立 SignalR 用戶端和伺服器所裝載的 ASP.NET Core 專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 使用**檔案** > **新專案**功能表選項，然後選擇**ASP.NET Core Web 應用程式**。 將專案命名*SignalRChat*。

   ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started/_static/signalr-new-project-dialog.png)

2. 選取**Web 應用程式**建立使用 Razor 頁面專案。 然後選取**確定**。 確定**ASP.NET Core 2.1**雖然 SignalR 執行較舊版本的.NET framework 選取器中選取。

   ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started/_static/signalr-new-project-choose-type.png)

Visual Studio 包含`Microsoft.AspNetCore.SignalR`封裝包含其伺服器程式庫的一部分其**ASP.NET Core Web 應用程式**範本。 不過，適用於 SignalR JavaScript 用戶端程式庫必須使用安裝*npm*。

3. 在中執行下列命令**Package Manager Console**視窗中的，從專案根目錄：

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. 複製*signalr.js*檔案從*node_modules\\ @aspnet\signalr\dist\browser* 至*lib*專案資料夾中的。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. 從**整合的終端機**，執行下列命令：

    ```console
    dotnet new razor -o SignalRChat
    ```

2. JavaScript 用戶端程式庫使用安裝*npm*。

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. 複製*signalr.js*檔案從*node_modules\\ @aspnet\signalr\dist\browser* 至*lib*專案資料夾中的。

-----

## <a name="create-the-signalr-hub"></a>建立 SignalR 中樞

集線器是做為概要管線可讓用戶端與伺服器彼此呼叫方法的類別。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 將類別加入專案中，選擇**檔案** > **新增** > **檔案**，然後選取**Visual C# 類別**。

2. 繼承自`Microsoft.AspNetCore.SignalR.Hub`。 `Hub`類別包含屬性和事件管理連接和群組，以及傳送和接收資料。

3. 建立`SendMessage`連接的交談的所有用戶端傳送訊息的方法。 請注意，它會傳回[工作](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)，因為 SignalR 是非同步。 非同步程式碼較易調整大小。

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. 開啟*SignalRChat* Visual Studio 程式碼中的資料夾。

2. 將類別加入專案中，選取**檔案** > **新檔案**從功能表。

3. 繼承自`Microsoft.AspNetCore.SignalR.Hub`。 `Hub`類別包含屬性和事件管理連接和群組，以及傳送和接收資料給用戶端。

4. 將 `SendMessage` 方法加入類別中。 `SendMessage`方法連接的交談的所有用戶端會傳送訊息。 請注意，它會傳回[工作](/dotnet/api/system.threading.tasks.task)，因為 SignalR 是非同步。 非同步程式碼較易調整大小。

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a>設定專案以使用 SignalR

SignalR 伺服器必須設定，讓它知道要傳遞到 SignalR 要求。

1. 若要設定 SignalR 專案，請修改專案的`Startup.ConfigureServices`方法。

   `services.AddSignalR` 將 SignalR 的一部分[中介軟體](xref:fundamentals/middleware/index)管線。

2. 設定路由，以便您使用的中樞`UseSignalR`。


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a>建立 SignalR 用戶端程式碼

1. 取代中的內容*Pages\Index.cshtml*為下列程式碼：

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   前面的 HTML 顯示名稱和訊息欄位及提交按鈕。 請注意底部的指令碼參考： SignalR 的參考和*chat.js*。

2. 加入名為的 JavaScript 檔案*chat.js*至*wwwroot\js*資料夾。 將下列程式碼加入該檔案：

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>執行應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 選取**偵錯** > **啟動但不偵錯**啟動瀏覽器，並載入本機網站。 從網址列複製 URL。

1. 開啟另一個瀏覽器執行個體 （任何瀏覽器），並在網址列中貼上 URL。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後按一下**傳送** 按鈕。 名稱和訊息會顯示兩個頁面上立即。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 按 [偵錯] (F5) 以建置並執行程式。 執行程式，開啟瀏覽器視窗。

1. 開啟另一個瀏覽器視窗，並載入本機的網站。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後按一下**傳送** 按鈕。 名稱和訊息會顯示兩個頁面上立即。

---

  ![方案](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>相關資源

[ASP.NET Core SignalR 簡介](introduction.md)
