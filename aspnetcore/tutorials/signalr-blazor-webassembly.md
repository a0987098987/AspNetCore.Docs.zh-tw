---
title: SignalR使用 ASP.NET Core 搭配Blazor WebAssembly
author: guardrex
description: 建立使用 ASP.NET Core 搭配的聊天應用 SignalR 程式 Blazor WebAssembly 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: 5a58e7ae28842e2e8a0f3bae8f47e252839903fe
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408873"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a>SignalR使用 ASP.NET Core 搭配Blazor WebAssembly

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本教學課程將教您使用與建立即時應用程式的基本概念 SignalR Blazor WebAssembly 。 您會了解如何：

> [!div class="checklist"]
> * 建立 Blazor WebAssembly 託管應用程式專案
> * 新增 SignalR 用戶端程式庫
> * 新增 SignalR 中樞
> * 新增 SignalR 服務和中樞的端點 SignalR
> * 新增 Razor 聊天的元件程式碼

在本教學課程結尾，您將會有一個可運作的聊天應用程式。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 使用**ASP.NET 和 網頁程式開發**工作負載[Visual Studio 2019 16.6 或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Visual Studio for Mac 8.6 版或更新版本](https://visualstudio.microsoft.com/vs/mac/)
* [!INCLUDE [.NET Core 3.1 SDK](~/includes/3.1-SDK.md)]

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a>建立託管 Blazor WebAssembly 應用程式專案

遵循您選擇的工具的指導方針：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!NOTE]
> Visual Studio 16.6 或更新版本，而且需要 .NET Core SDK 3.1.300 或更新版本。

1. 建立新專案。

1. 選取** Blazor 應用程式**，然後選取 **[下一步]**。

1. `BlazorSignalRApp`在 [**專案名稱**] 欄位中輸入。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]****。

1. 選擇 [ ** Blazor WebAssembly 應用程式**] 範本。

1. 在 [ **Advanced**] 底下，選取 [ **ASP.NET Core**裝載] 核取方塊。

1. 選取 [建立]****。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在命令 shell 中，執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. 在 Visual Studio Code 中，開啟應用程式的專案資料夾。

1. 當對話方塊出現以新增資產以建立和偵錯工具時，請選取 **[是**]。 Visual Studio Code 會自動新增 `.vscode` 具有所產生檔案和檔案的資料夾 `launch.json` `tasks.json` 。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 安裝最新版本的[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) ，並執行下列步驟：

1. **File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。

1. 在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。

1. 選擇 [ ** Blazor WebAssembly 應用程式**] 範本。 選取 [下一步] 。

   確認下列設定：

   * 設定為 **.Net Core 3.1**的**目標 Framework** 。
   * **驗證**設為 [**無驗證**]。

   選取 [**主控 ASP.NET Core** ] 核取方塊。

   選取 [下一步] 。

1. 在 [**專案名稱**] 欄位中，將應用程式命名為 `BlazorSignalRApp` 。 選取 [建立]****。

   如果出現會信任開發憑證的提示，請信任憑證並繼續。 需要使用者和 keychain 密碼，才能信任憑證。

1. 流覽至專案資料夾，然後開啟專案的方案檔（），以開啟專案 `.sln` 。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

在命令 shell 中，執行下列命令：

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a>新增 SignalR 用戶端程式庫

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 在**方案總管**中，以滑鼠右鍵按一下 `BlazorSignalRApp.Client` 專案，然後選取 [**管理 NuGet 套件**]。

1. 在 [**管理 NuGet 封裝**] 對話方塊中，確認 [**套件來源**] 設定為 `nuget.org` 。

1. 在選取 **[流覽]** 的情況下，于 `Microsoft.AspNetCore.SignalR.Client` 搜尋方塊中輸入。

1. 在搜尋結果中選取套件， [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) 然後選取 [**安裝**]。

1. 如果出現 [**預覽變更**] 對話方塊，請選取 **[確定]**。

1. 如果 [**接受授權**] 對話方塊出現，如果您同意授權條款，請選取 [**我接受**]。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

在**整合式終端**機（從工具列**觀看**  >  **終端**機）中，執行下列命令：

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在 [**解決方案**] 提要欄位中，以滑鼠右鍵按一下 `BlazorSignalRApp.Client` 專案，然後選取 [**管理 NuGet 套件**]。

1. 在 [**管理 NuGet 封裝**] 對話方塊中，確認 [來源] 下拉式設定為 `nuget.org` 。

1. 在選取 **[流覽]** 的情況下，于 `Microsoft.AspNetCore.SignalR.Client` 搜尋方塊中輸入。

1. 在搜尋結果中，選取封裝旁的核取方塊 [`Microsoft.AspNetCore.SignalR.Client`](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) ，然後選取 [**新增套件**]。

1. 如果 [**接受授權**] 對話方塊出現，請選取 [**接受**] （如果您同意授權條款）。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

在命令 shell 中，執行下列命令：

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a>新增 SignalR 中樞

在 `BlazorSignalRApp.Server` 專案中，建立 `Hubs` （複數）資料夾，並新增下列 `ChatHub` 類別（ `Hubs/ChatHub.cs` ）：

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-services-and-an-endpoint-for-the-signalr-hub"></a>新增服務和中樞的端點 SignalR

1. 在 `BlazorSignalRApp.Server` 專案中，開啟 `Startup.cs` 檔案。

1. 將類別的命名空間新增 `ChatHub` 至檔案頂端：

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. 將 SignalR 和回應壓縮中介軟體服務新增至 `Startup.ConfigureServices` ：

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_ConfigureServices&highlight=3,5-9)]

1. 在 `Startup.Configure` 中：

   * 使用處理管線設定頂端的回應壓縮中介軟體。
   * 在控制器的端點和用戶端的回退之間，新增中樞的端點。

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_Configure&highlight=3,25)]

## <a name="add-razor-component-code-for-chat"></a>新增 Razor 聊天的元件程式碼

1. 在 `BlazorSignalRApp.Client` 專案中，開啟 `Pages/Index.razor` 檔案。

1. 將標記取代為下列程式碼：

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a>執行應用程式

1. 遵循您工具的指導方針：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 在 [**方案總管**中，選取 `BlazorSignalRApp.Server` 專案。 按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd> + <kbd>F5</kbd> ，以執行應用程式而不進行偵測。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalRBlazor WebAssembly範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 當 VS Code 提供來建立伺服器應用程式（）的啟動設定檔時 `.vscode/launch.json` ， `program` 專案會顯示如下，以指向應用程式的元件（ `{APPLICATION NAME}.Server.dll` ）：

   ```json
   "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/{APPLICATION NAME}.Server.dll"
   ```

1. 按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd> + <kbd>F5</kbd> ，以執行應用程式而不進行偵測。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalRBlazor WebAssembly範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在 [**方案**] 提要欄位中，選取 `BlazorSignalRApp.Server` 專案。 按<kbd>則是⌘</kbd> + <kbd>↩</kbd>以執行應用程式的偵錯工具，或<kbd>⌥</kbd> + <kbd>則是⌘</kbd> + <kbd>↩</kbd>以執行應用程式而不進行偵測。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalRBlazor WebAssembly範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 在命令 shell 中，執行下列命令：

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器、輸入名稱和訊息，然後選取按鈕來傳送訊息。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalRBlazor WebAssembly範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 TREK VI：未發現的國家/地區* &copy; 1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

---

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立 Blazor WebAssembly 託管應用程式專案
> * 新增 SignalR 用戶端程式庫
> * 新增 SignalR 中樞
> * 新增 SignalR 服務和中樞的端點 SignalR
> * 新增 Razor 聊天的元件程式碼

若要深入瞭解如何建立 Blazor 應用程式，請參閱 Blazor 檔：

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a>其他資源

* <xref:signalr/introduction>
* [SignalR用於驗證的跨原始來源協調](xref:blazor/fundamentals/additional-scenarios#signalr-cross-origin-negotiation-for-authentication)
