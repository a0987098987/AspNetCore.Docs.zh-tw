---
title: "開始使用 ASP.NET Core 的 SignalR"
author: rachelappel
description: "了解建置適用於 ASP.NET Core 使用 SignalR 的即時應用程式的基本概念。"
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>教學課程： 開始使用 SignalR for ASP.NET Core

作者：[Rachel Appel](https://twitter.com/rachelappel)

本教學課程將教導您建置適用於 ASP.NET Core 使用 SignalR 的即時應用程式的基本概念。

   ![方案](get-started-signalr-core/_static/signalr-get-started-finished.png)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

本教學課程將示範下列 SignalR 開發工作：

> [!div class="checklist"]
> * 建立 ASP.NET Core web 應用程式。
> * 建立 SignalR 中樞內容推播至用戶端。
> * 您可以使用 SignalR JavaScript 程式庫來傳送訊息，並顯示從中樞的更新。

## <a name="prerequisites"></a>必要條件

安裝下列軟體：

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)或更新版本
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 或更新與 ASP.NET 及 web 程式開發工作負載版本
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>建立 SignalR 用戶端和伺服器所裝載的 ASP.NET Core 專案

1. 使用**檔案** > **新專案**功能表選項，然後選擇**ASP.NET Core Web 應用程式**。 將專案命名為 `SignalRChat`。

  ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. 選取**Web 應用程式**建立使用 Razor 頁面專案。 然後選取**確定**。 確定**ASP.NET Core 2.1**雖然 SignalR 執行較舊版本的.NET framework 選取器中選取。

  ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  裝載 SignalR 的伺服器端程式碼的程式庫會包含在專案範本。 安裝個別的用戶端 JavaScript [npm](https://www.npmjs.com/)。

  ```console
   npm install @aspnet/signalr
  ```

3. 複製*signalr.js*從*node_modules\\ @aspnet\signalr\dist\browser* 至*wwwroot\lib*專案資料夾中的。

## <a name="create-the-signalr-hub"></a>建立 SignalR 中樞

集線器是做為概要管線可讓用戶端與伺服器彼此呼叫方法的類別。

1. 將類別加入專案中，選擇**檔案** > **新增** > **檔案**，然後選取**Visual C# 類別**。 

1. 繼承自`Microsoft.AspNetCore.SignalR.Hub`。 `Hub`類別包含屬性和事件管理連接和群組，以及傳送和接收資料。

1. 建立`Send`連接的交談的所有用戶端傳送訊息的方法。 請注意，它會傳回`Task`，因為 SignalR 是非同步。 非同步程式碼較易調整大小。

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>設定專案以使用 SignalR

SignalR 伺服器必須設定，讓它知道要傳遞到 SignalR 要求。

1. 若要設定 SignalR 專案，請修改`ConfigureServices`應用程式的方法`Startup`類別藉由將呼叫`services.AddSignalR`。

  `services.AddSignalR` 將 SignalR 的一部分[ASP.NET Core 中介軟體](xref:fundamentals/middleware/index)管線。

1. 設定路由，以便您使用的中樞`UseSignalR`。

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>建立 SignalR 用戶端程式碼

1. 取代中的內容*Pages\Index.cshtml*為下列程式碼：

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  前面的 HTML 顯示名稱和訊息欄位及提交按鈕。 請注意底部的指令碼參考： SignalR 的參考和*chat.js*。

1. 加入 JavaScript 檔案*wwwroot\js*資料夾名為*chat.js*並加入下列程式碼：

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>執行應用程式

1. 選取**偵錯** > **啟動但不偵錯**啟動瀏覽器，並載入本機網站。 從網址列複製 URL。

1. 開啟另一個瀏覽器執行個體 （任何瀏覽器），並在網址列中貼上 URL。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後按一下**傳送** 按鈕。 名稱和訊息會顯示兩個頁面上立即。

  ![方案](get-started-signalr-core/_static/signalr-get-started-finished.png)
