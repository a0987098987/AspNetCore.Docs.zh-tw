---
title: 將ASP.NET核心SignalR與BlazorWeb 組裝一起使用
author: guardrex
description: 創建使用 ASP.NETSignalR核心Blazor與 Web 組裝的聊天應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: c4843dc282e1978b39738e206ecc79ded87fcff9
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80306577"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a>將ASP.NET核心信號R與布拉佐爾網路組裝一起使用

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

本教學將介紹使用 SignalR 使用 Blazor WebAssembly 構建即時應用程式的基礎知識。 您會了解如何：

> [!div class="checklist"]
> * 建立 Blazor WebAssembly 託管應用專案
> * 新增 SignalR 用戶端程式庫
> * 新增訊號R中心
> * SignalR 中心加入 SignalR 服務和終結點
> * 新增用於聊天的 Razor 元件碼

在本教程結束時,您將有一個工作聊天應用程式。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/)([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a>建立託管的 Blazor WebAssembly 應用專案

不使用 Visual Studio 版本 16.6 預覽版或更高版本時,請安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)範本。 [微軟.AspNetCore.元件.WebAssembly.範本](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/)包有預覽版本,而Blazor WebAssembly處於預覽狀態。 在命令 shell 中,執行以下指令:

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
```

遵循您選擇工具的指南:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新專案。

1. 選擇**Blazor 應用程式**並選擇 **「下一步**」。

1. 在**專案名稱**欄位中鍵入"BlazorSignalRApp" 確認 **「位置**」條目正確或為專案提供位置。 選取 [建立]  。

1. 選擇**Blazor Web 組裝應用**範本。

1. 在 **「進階」** 下,選擇 **「ASP.NET核心託管**」複選框。

1. 選取 [建立]  。

> [!NOTE]
> 如果升級或安裝了新版本的 Visual Studio,並且 Blazor WebAssembly 範本未顯示在 VS`dotnet new`UI 中,請使用前面顯示 的命令重新安裝範本。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在命令 shell 中,執行以下指令:

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. 在可視化工作室代碼中,打開應用的項目資料夾。

1. 當對話框顯示為添加要生成和調試應用的資產時,選擇 **"是**"。 可視化工作室代碼自動添加 *.vscode*資料夾與生成的*啟動.json*和*任務.json*檔。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在命令 shell 中,執行以下指令:

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. 在 Mac 的 Visual Studio 中,透過導航到專案資料夾並打開專案的解決方案檔 *(.sln*) 來打開專案。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

在命令 shell 中,執行以下指令:

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a>新增 SignalR 用戶端程式庫

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. 在**解決方案資源管理器**中,右鍵單擊**BlazorSignalRApp.Client**專案,然後選擇 **「管理 NuGet 包**」。

1. 在 **'管理 NuGet 套件'** 對話框中,確認**套件來源**設定為*nuget.org*。

1. 選中 **「瀏覽**」後,在搜尋框中鍵入「Microsoft.AspNetCore.SignalR.用戶端」。。

1. 在搜尋結果中,選擇`Microsoft.AspNetCore.SignalR.Client`包並選擇 **「安裝**」。

1. 如果出現 **「預覽更改**」對話框,請選擇 **「確定**」。

1. 如果出現 **「許可證接受」** 對話方塊,請選擇 **「我同意」** 許可條款。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

在**整合式終端**機 (從**工具列檢視** > **終端**)中,執行以下命令:

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在 **「解決方案**」側邊欄中,右鍵單擊**BlazorSignalRApp.用戶端**專案,然後選擇 **「管理 NuGet 包**」。

1. 在 **'管理 NuGet 套件'** 對話框中,確認來源下拉清單設定為*nuget.org*。

1. 選中 **「瀏覽**」後,在搜尋框中鍵入「Microsoft.AspNetCore.SignalR.用戶端」。。

1. 在搜尋結果中,選擇`Microsoft.AspNetCore.SignalR.Client`包旁邊的複選框,然後選擇"**添加包**"。

1. 如果出現 **「許可證接受」** 對話方塊,請選擇「如果同意許可條款,**則接受**」。。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

在命令外殼中,執行以下命令:

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a>新增訊號R中心

在**BlazorSignalRApp.Server**專案中,建立*一個集線器*(`ChatHub`複數) 資料夾並新增以下類(*集線器/ ChatHub.cs):*

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a>SignalR 中心加入 SignalR 服務和終結點

1. 在**BlazorSignalRApp.Server**專案中,打開*Startup.cs*檔。

1. 將類別的`ChatHub`命名空間加入檔案的頂部:

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. 新增 SignalR`Startup.ConfigureServices`服務

   ```csharp
   services.AddSignalR();
   ```

1. 在`Startup.Configure`預設控制器路由的終結點和用戶端回退之間,為中心添加終結點:

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a>新增用於聊天的 Razor 元件碼

1. 在**BlazorSignalRApp.Client**專案中,打開*頁面/Index.razor*檔。

1. 將標記取代為以下代碼:

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a>執行應用程式

1. 依工具指南操作:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 在**解決方案資源管理器**中,選擇**BlazorSignalRApp.Server**專案。 按**Ctrl+F5**可執行應用程式而不進行調試。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息立即顯示在兩個頁面上:

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 從**Debug** > 工具列中選擇 **「調試運行而不調試**」。。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息立即顯示在兩個頁面上:

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 在 **「解決方案**」側邊欄中,選擇**BlazorSignalRApp.Server**專案。 從選單中,選擇「 > **不調試即可運行啟動** **Run** 」。

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息立即顯示在兩個頁面上:

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 在命令外殼中,執行以下命令:

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. 從網址列複製 URL，開啟另一個瀏覽器執行個體或索引標籤，然後將 URL 貼入網址列。

1. 選擇任一個瀏覽器，輸入名稱和訊息，然後選取 [傳送]**** 按鈕。 名稱和訊息立即顯示在兩個頁面上:

   ![SignalR Blazor WebAssembly 範例應用程式在兩個瀏覽器視窗中打開,顯示交換的消息。](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   行情 *:《星際迷航》六世:未被發現的國家*&copy;1991[年派拉蒙](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)

---

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立BlazorWeb 群組載託管應用專案
> * 新增SignalR用戶端函式庫
> * 新增SignalR中心
> * 為SignalRSignalR中心新增服務和終結點
> * 新增用於聊天的 Razor 元件碼

要瞭解有關建Blazor置 應用程式的資訊Blazor, 請參考以下文件:

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a>其他資源

* <xref:signalr/introduction>
