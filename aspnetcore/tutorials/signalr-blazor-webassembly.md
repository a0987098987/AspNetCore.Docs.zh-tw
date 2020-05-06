---
title: 搭配Blazor WebAssembly SignalR使用 ASP.NET Core
author: guardrex
description: 建立使用 ASP.NET Core SignalR搭配Blazor WebAssembly 的聊天應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/30/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: 1579b92dbc9db08bfdc5572e5d4245bd18d50590
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82773784"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a>搭配使用 ASP.NET Core SignalR 與 Blazor WebAssembly

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

本教學課程將教您使用 SignalR 搭配 Blazor WebAssembly 建立即時應用程式的基本概念。 您會了解如何：

> [!div class="checklist"]
> * 建立 Blazor WebAssembly 託管應用程式專案
> * 新增 SignalR 用戶端程式庫
> * 新增 SignalR 中樞
> * 新增 SignalR 服務和 SignalR 中樞的端點
> * 新增 Razor 元件程式碼以進行聊天

在本教學課程結尾，您將會有一個可運作的聊天應用程式。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a>建立 hosted Blazor WebAssembly 應用程式專案

當不使用 Visual Studio 16.6 版 Preview 2 或更新版本時，請安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。 Blazor WebAssembly 處於預覽階段時， [WebAssembly](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/)套件會有預覽版本。 在命令 shell 中，執行下列命令：

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-rc1.20223.4
```

遵循您選擇的工具的指導方針：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新專案。

1. 選取 [ **Blazor 應用程式**] 並選取 **[下一步]**。

1. 在 [**專案名稱**] 欄位中輸入 "BlazorSignalRApp"。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]  。

1. 選擇 [ **Blazor WebAssembly 應用程式**] 範本。

1. 在 [ **Advanced**] 底下，選取 [ **ASP.NET Core**裝載] 核取方塊。

1. 選取 [建立]  。

> [!NOTE]
> 如果您已升級或安裝新版 Visual Studio，且 VS UI 中未出現 Blazor WebAssembly 範本，請使用先前所示的`dotnet new`命令重新安裝範本。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在命令 shell 中，執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. 在 Visual Studio Code 中，開啟應用程式的專案資料夾。

1. 當對話方塊出現以新增資產以建立和偵錯工具時，請選取 **[是**]。 Visual Studio Code 會自動新增*vscode*資料夾，其中包含產生的*啟動 json*和工作 *. json*檔案。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在命令 shell 中，執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. 在 Visual Studio for Mac 中，流覽至專案資料夾，然後開啟專案的方案檔（*.sln*），以開啟專案。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

在命令 shell 中，執行下列命令：

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a>新增 SignalR 用戶端程式庫

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 在**方案總管**中，以滑鼠右鍵按一下 [ **BlazorSignalRApp** ] 專案，然後選取 [**管理 NuGet 套件**]。

1. 在 [**管理 NuGet 封裝**] 對話方塊中，確認 [**套件來源**] 已設定為 [ *nuget.org*]。

1. 在選取 **[流覽]** 的情況下，于搜尋方塊中輸入 "AspNetCore. SignalR. Client"。

1. 在搜尋結果中選取套件， `Microsoft.AspNetCore.SignalR.Client`然後選取 [**安裝**]。

1. 如果出現 [**預覽變更**] 對話方塊，請選取 **[確定]**。

1. 如果 [**接受授權**] 對話方塊出現，如果您同意授權條款，請選取 [**我接受**]。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

在**整合式終端**機（從工具列**觀看** > **終端**機）中，執行下列命令：

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在 [**解決方案**] 提要欄位中，以滑鼠右鍵按一下 [ **BlazorSignalRApp** ] 專案，然後選取 [**管理 NuGet 套件**]。

1. 在 [**管理 NuGet 封裝**] 對話方塊中，確認 [來源] 下拉式設定為 [ *nuget.org*]。

1. 在選取 **[流覽]** 的情況下，于搜尋方塊中輸入 "AspNetCore. SignalR. Client"。

1. 在搜尋結果中，選取`Microsoft.AspNetCore.SignalR.Client`封裝旁的核取方塊，然後選取 [**新增套件**]。

1. 如果 [**接受授權**] 對話方塊出現，請選取 [**接受**] （如果您同意授權條款）。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

在命令 shell 中，執行下列命令：

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a>新增 SignalR 中樞

在**BlazorSignalRApp**專案中，建立*中樞*（複數）資料夾，並新增下列`ChatHub`類別（*hub/ChatHub .cs*）：

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-services-and-an-endpoint-for-the-signalr-hub"></a>新增服務和 SignalR 中樞的端點

1. 在**BlazorSignalRApp**專案中，開啟*Startup.cs*檔案。

1. 將`ChatHub`類別的命名空間新增至檔案頂端：

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. 將 SignalR 和回應壓縮中介軟體服務`Startup.ConfigureServices`新增至：

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_ConfigureServices&highlight=3,5-9)]

1. 在`Startup.Configure`控制器的端點和用戶端回溯之間，為中樞新增端點：

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet_UseEndpoints&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a>新增 Razor 元件程式碼以進行聊天

1. 在 [ **BlazorSignalRApp** ] 專案中，開啟*Pages/Index. razor*檔案。

1. 將標記取代為下列程式碼：

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a>執行應用程式

1. 遵循您工具的指導方針：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 在 [**方案總管**中，選取 [ **BlazorSignalRApp** ] 專案。 按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd>+<kbd>F5</kbd> ，以執行應用程式而不進行偵測。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 Trek VI：未發現的國家/地區* &copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 當 VS Code 提供來建立伺服器應用程式的啟動設定檔（*. vscode/啟動 json*）時， `program`專案會顯示如下，以指向應用程式的元件（`{APPLICATION NAME}.Server.dll`）：

   ```json
   "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/{APPLICATION NAME}.Server.dll"
   ```

1. 按<kbd>f5</kbd>鍵以執行應用程式的偵測或<kbd>Ctrl</kbd>+<kbd>F5</kbd> ，以執行應用程式而不進行偵測。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 Trek VI：未發現的國家/地區* &copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在 [**方案**] 提要欄位中，選取 [ **BlazorSignalRApp** ] 專案。 按<kbd>則是⌘</kbd>+<kbd>↩</kbd>* * 以執行應用程式，並使用偵錯工具或<kbd>⌥</kbd>+<kbd>則是⌘</kbd>+<kbd>↩</kbd>執行應用程式而不進行偵測。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 Trek VI：未發現的國家/地區* &copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 在命令 shell 中，執行下列命令：

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息會立即顯示在兩個頁面上：

   ![SignalR Blazor WebAssembly 範例應用程式會在兩個顯示交換訊息的瀏覽器視窗中開啟。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   引號：*星星 Trek VI：未發現的國家/地區* &copy;1991[重要](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

---

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立Blazor WebAssembly 託管應用程式專案
> * 新增SignalR用戶端程式庫
> * 新增SignalR中樞
> * 新增SignalR服務和SignalR中樞的端點
> * 新增Razor聊天的元件程式碼

若要深入瞭解如何Blazor建立應用程式， Blazor請參閱檔：

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a>其他資源

* <xref:signalr/introduction>
