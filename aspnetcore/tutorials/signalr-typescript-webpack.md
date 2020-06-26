---
title: 搭配 SignalR TypeScript 和 Webpack 使用 ASP.NET Core
author: ssougnez
description: 在本教學課程中，您會將 Webpack 設定為配套，並建立 SignalR 用戶端以 TypeScript 撰寫的 ASP.NET Core web 應用程式。
ms.author: bradyg
ms.custom: mvc
ms.date: 02/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 49d185ce941d5f8e841224e7de3b72b8350a1c47
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407898"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>搭配 SignalR TypeScript 和 Webpack 使用 ASP.NET Core

作者：[Sébastien Sougnez](https://twitter.com/ssougnez) 和 [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) 可讓開發人員組合並建置 Web 應用程式的用戶端資源。 本教學課程示範如何在 SignalR 用戶端以[TypeScript](https://www.typescriptlang.org/)撰寫的 ASP.NET Core web 應用程式中使用 Webpack。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
> * Scaffold 入門 ASP.NET Core SignalR 應用程式
> * 設定 SignalR TypeScript 用戶端
> * 使用 Webpack 設定組建管線
> * 設定 SignalR 伺服器
> * 啟用用戶端與伺服器之間的通訊

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample)（[如何下載](xref:index#how-to-download-a-sample)）

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ASP.NET 和 網頁程式開發**工作負載的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [.NET Core SDK 3.0 或更新版本](https://dotnet.microsoft.com/download/dotnet-core)
* 具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 3.0 或更新版本](https://dotnet.microsoft.com/download/dotnet-core)
* [適用於 Visual Studio Code 1.17.1 版或更新版本的 C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* 具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)

---

## <a name="create-the-aspnet-core-web-app"></a>建立 ASP.NET Core Web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。 根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。 請遵循 Visual Studio 中的下列指示：

1. 啟動 Visual Studio。 在 [開始] 視窗中，選取 [**不需要程式碼即可繼續**]。
1. 流覽至 [**工具**] [選項] [ > **Options** > **專案和方案**] [ > **web 套件管理** > **外部 web 工具**]。
1. 從清單中選取 *$(PATH)* 項目。 按一下向上箭號，將專案移至清單中的第二個位置，然後選取 **[確定]**。

    ![Visual Studio 設定](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio 設定已完成。

1. 使用 [**檔案**] [  >  **新增**  >  **專案**] 功能表選項，然後選擇 [ **ASP.NET Core Web 應用程式**] 範本。 選取 [下一步] 。
1. 將專案命名為*命名為 signalrwebpack*，然後選取 [**建立**]。
1. 從 [目標 framework] 下拉式選單中選取 [ *.Net Core* ]，然後從 [framework 選取器] 下拉式選單中選取 [ *ASP.NET Core 3.1* ]。 選取 [**空白**] 範本，然後選取 [**建立**]。

將 `Microsoft.TypeScript.MSBuild` 套件新增至專案：

1. 在**方案總管**（右窗格）中，以滑鼠右鍵按一下專案節點，然後選取 [**管理 NuGet 套件**]。 在 [**流覽**] 索引標籤中，搜尋 `Microsoft.TypeScript.MSBuild` ，然後按一下右側的 [**安裝**] 來安裝套件。

Visual Studio 會在**方案總管**的 [相依性 **]** 節點底下新增 NuGet 套件，並在專案中啟用 TypeScript 編譯。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在 [整合式終端機]**** 中執行下列命令：

```dotnetcli
dotnet new web -o SignalRWebPack
code -r SignalRWebPack
```

* `dotnet new`命令會在*命名為 signalrwebpack*目錄中建立空的 ASP.NET Core web 應用程式。
* `code`命令會在 Visual Studio Code 的目前實例中開啟*命名為 signalrwebpack*資料夾。

在**整合式終端**機中執行下列 .NET Core CLI 命令：

```dotnetcli
dotnet add package Microsoft.TypeScript.MSBuild
```

上述命令會新增[Microsoft typescript](https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/)封裝，並在專案中啟用 TypeScript 編譯。

---

## <a name="configure-webpack-and-typescript"></a>設定 Webpack 和 TypeScript

下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。

1. 在專案根目錄中執行下列命令，以在檔案*上建立package.js* ：

    ```console
    npm init -y
    ```

1. 將反白顯示的屬性新增至檔案中的*package.js* ，並儲存檔案變更：

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。

1. 安裝必要的 npm 套件。 從專案根目錄執行下列命令︰

    ```console
    npm i -D -E clean-webpack-plugin@3.0.0 css-loader@3.4.2 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.9.0 ts-loader@6.2.1 typescript@3.7.5 webpack@4.41.5 webpack-cli@3.3.10
    ```

    要注意的一些命令詳細資料：

    * 版本號碼接在每一個套件名稱的 `@` 符號之後。 npm 會安裝這些特定的套件版本。
    * `-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。 例如，會使用 `"webpack": "4.41.5"`，而不是 `"webpack": "^4.41.5"`。 此選項可防止意外升級至較新的套件版本。

    如需詳細資訊，請參閱[npm-安裝](https://docs.npmjs.com/cli/install)檔。

1. 將檔案 `scripts` *上package.js*的屬性取代為下列程式碼：

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    指令碼的一些說明：

    * `build`：在開發模式下組合用戶端資源，並監看檔案變更。 檔案監看員會導致套件組合在每次專案檔變更時重新產生。 `mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。 `build` 僅用於開發。
    * `release`：在生產模式下組合用戶端資源。
    * `publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。 它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。

1. 使用下列程式碼，在專案根目錄中建立名為*webpack.config.js*的檔案：

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    上述檔案會設定 Webpack 編譯。 要注意的一些組態詳細資料：

    * `output` 屬性會覆寫 *dist* 的預設值。 因而在 *wwwroot* 目錄中發出套件組合。
    * `resolve.extensions`陣列包含匯 *.js*入 SignalR 用戶端 JavaScript 的 .js。

1. 在專案根目錄中建立新的*src*目錄，以儲存專案的用戶端資產。

1. 使用下列標記建立*src/index.html* 。

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    上述的 HTML 會定義首頁的樣板標記。

1. 建立新的 *src/css* 目錄。 其目的是要儲存專案的 *.css* 檔案。

1. 使用下列 CSS 建立*src/css/main. css* ：

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    上述的 *main.css* 檔案會設定應用程式的樣式。

1. 使用下列 JSON 建立*src/tsconfig.js* ：

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。

1. 使用下列程式碼建立*src/index. ts* ：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：

    * `keyup`：當使用者在文字方塊中輸入時，就會引發這個事件 `tbMessage` 。 當使用者按下 **Enter** 鍵時，即會呼叫 `send` 函式。
    * `click`：當使用者按一下 [傳送]**** 按鈕時，就會引發此事件。 系統會呼叫 `send` 函式。

## <a name="configure-the-app"></a>設定應用程式

1. 在中 `Startup.Configure` ，將呼叫新增至[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=9-10)]

   上述程式碼可讓伺服器找出並提供*index.html*檔案。  該檔案會提供給使用者是否輸入其完整 URL 或 web 應用程式的根 URL。

1. 在結尾，將 `Startup.Configure` */hub*路由對應至 `ChatHub` 中樞。 取代顯示 Hello World 的程式碼 *！* 成為下列這行： 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. 在中 `Startup.ConfigureServices` ，呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)。

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. 在專案根*命名為 signalrwebpack/* 中建立名為*hub*的新目錄，以儲存 SignalR 中樞。

1. 使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. `using`在*Startup.cs*檔案頂端新增下列語句，以解析 `ChatHub` 參考：

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>啟用用戶端與伺服器的通訊

應用程式目前顯示傳送訊息的基本表單，但尚未正常運作。 伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。

1. 在專案根目錄中執行下列命令：

    ```console
    npm i @microsoft/signalr @types/node
    ```

    上述命令會安裝：

     * [ SignalR TypeScript 用戶端](https://www.npmjs.com/package/@microsoft/signalr)，可讓用戶端將訊息傳送至伺服器。
     * Node.js 的 TypeScript 類型定義，可啟用 Node.js 類型的編譯時間檢查。

1. 將醒目提示的程式碼新增至 *src/index.ts* 檔案：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    上述程式碼支援從伺服器接收訊息。 `HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。 `withUrl` 函式則會設定中樞 URL。

    SignalR啟用用戶端與伺服器之間的訊息交換。 每個訊息都有特定的名稱。 例如，具有名稱的訊息 `messageReceived` 可以執行負責在 [訊息] 區域中顯示新訊息的邏輯。 接聽特定的訊息可透過 `on` 函式完成。 可以接聽任意數目的訊息名稱。 也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。 用戶端收到訊息之後，就會在其 `innerHTML` 屬性中使用作者的名稱和訊息內容建立新的 `div` 元素。 它會新增至顯示訊息的主要 `div` 元素。

1. 現在用戶端可以接收訊息，請設定它來傳送訊息。 將醒目提示的程式碼新增至 *src/index.ts* 檔案：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。 方法的第一個參數是訊息名稱。 訊息資料則佔用其他參數。 在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。 訊息是由使用者名稱和文字方塊中的使用者輸入所組成。 如果傳送可正常運作，則會清除文字方塊的值。

1. 將 `NewMessage` 方法新增至 `ChatHub` 類別：

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。 不需要讓泛型 `on` 方法接收所有訊息。 以訊息名稱命名方法就已足夠。

    在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。 C# `NewMessage` 方法預期有用戶端所傳送的資料。 在[用戶端](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all)上進行[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)的呼叫。 接收的訊息就會傳送到連線至中樞的所有用戶端。

## <a name="test-the-app"></a>測試應用程式

使用下列步驟確認應用程式可正常運作。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 在 *release* 模式下執行 Webpack。 使用 [**套件管理員主控台**] 視窗，在專案根目錄中執行下列命令。 如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. 選取**Debug**  >  [**不進行調試**程式的 Debug Start]，在瀏覽器中啟動應用程式，而不附加偵錯工具。 隨即會在 `http://localhost:<port_number>` 處提供 *wwwroot/index.html* 檔案。

   如果您遇到編譯錯誤，請嘗試關閉並重新開啟方案。 

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)。 在網址列中貼上 URL。

1. 選擇其中一個瀏覽器，在 [訊息]**** 文字方塊中鍵入某些內容，然後按一下 [傳送]**** 按鈕。 唯一使用者名稱和訊息會立即顯示在這兩個頁面上。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. 在專案根目錄中執行下列命令，藉以建置並執行應用程式：

    ```dotnetcli
    dotnet run
    ```

    Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。

1. 開啟瀏覽器並前往 `http://localhost:<port_number>`。 隨即會提供 *wwwroot/index.html* 檔案。 從網址列複製 URL。

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)。 在網址列中貼上 URL。

1. 選擇其中一個瀏覽器，在 [訊息]**** 文字方塊中鍵入某些內容，然後按一下 [傳送]**** 按鈕。 唯一使用者名稱和訊息會立即顯示在這兩個頁面上。

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ASP.NET 和 網頁程式開發**工作負載的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [.NET Core SDK 2.2 或更新版本](https://dotnet.microsoft.com/download/dotnet-core)
* 具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.2 或更新版本](https://dotnet.microsoft.com/download/dotnet-core)
* [適用於 Visual Studio Code 1.17.1 版或更新版本的 C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* 具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)

---

## <a name="create-the-aspnet-core-web-app"></a>建立 ASP.NET Core Web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。 根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。 請遵循 Visual Studio 中的下列指示：

1. 流覽至 [**工具**] [選項] [ > **Options** > **專案和方案**] [ > **web 套件管理** > **外部 web 工具**]。
1. 從清單中選取 *$(PATH)* 項目。 按一下向上箭號，將此項目移至清單中的第二個位置。

    ![Visual Studio 設定](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio 組態已完成。 現在即可開始建立專案。

1. 使用 [檔案]**[新增]** > **[專案]** > **** 功能表選項，然後選擇 [ASP.NET Core Web 應用程式]**** 範本。
1. 將專案命名為*命名為 signalrwebpack*，然後選取 [**建立**]。
1. 從目標 Framework 下拉式清單中選取 [.NET Core]**，然後從 Framework 選取器下拉式清單中選取 [ASP.NET Core 2.2]**。 選取 [**空白**] 範本，然後選取 [**建立**]。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在 [整合式終端機]**** 中執行下列命令：

```dotnetcli
dotnet new web -o SignalRWebPack
```

隨即會在 *SignalRWebPack* 目錄中建立以 .NET Core 為目標的空白 ASP.NET Core Web 應用程式。

---

## <a name="configure-webpack-and-typescript"></a>設定 Webpack 和 TypeScript

下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。

1. 在專案根目錄中執行下列命令，以在檔案*上建立package.js* ：

    ```console
    npm init -y
    ```

1. 將醒目提示的屬性新增至 *package.json* 檔案：

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。

1. 安裝必要的 npm 套件。 從專案根目錄執行下列命令︰

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    要注意的一些命令詳細資料：

    * 版本號碼接在每一個套件名稱的 `@` 符號之後。 npm 會安裝這些特定的套件版本。
    * `-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。 例如，會使用 `"webpack": "4.29.3"`，而不是 `"webpack": "^4.29.3"`。 此選項可防止意外升級至較新的套件版本。

    如需詳細資訊，請參閱[npm-安裝](https://docs.npmjs.com/cli/install)檔。

1. 將檔案 `scripts` *上package.js*的屬性取代為下列程式碼：

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    指令碼的一些說明：

    * `build`：在開發模式下組合用戶端資源，並監看檔案變更。 檔案監看員會導致套件組合在每次專案檔變更時重新產生。 `mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。 `build` 僅用於開發。
    * `release`：在生產模式下組合用戶端資源。
    * `publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。 它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。

1. 使用下列程式碼，在專案根目錄中建立名為*webpack.config.js*的檔案：

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    上述檔案會設定 Webpack 編譯。 要注意的一些組態詳細資料：

    * `output` 屬性會覆寫 *dist* 的預設值。 因而在 *wwwroot* 目錄中發出套件組合。
    * `resolve.extensions`陣列包含匯 *.js*入 SignalR 用戶端 JavaScript 的 .js。

1. 在專案根目錄中建立新的*src*目錄，以儲存專案的用戶端資產。

1. 使用下列標記建立*src/index.html* 。

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    上述的 HTML 會定義首頁的樣板標記。

1. 建立新的 *src/css* 目錄。 其目的是要儲存專案的 *.css* 檔案。

1. 使用下列標記建立*src/css/main. css* ：

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    上述的 *main.css* 檔案會設定應用程式的樣式。

1. 使用下列 JSON 建立*src/tsconfig.js* ：

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。

1. 使用下列程式碼建立*src/index. ts* ：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：

    * `keyup`：當使用者在文字方塊中輸入時，就會引發這個事件 `tbMessage` 。 當使用者按下 **Enter** 鍵時，即會呼叫 `send` 函式。
    * `click`：當使用者按一下 [傳送]**** 按鈕時，就會引發此事件。 系統會呼叫 `send` 函式。

## <a name="configure-the-aspnet-core-app"></a>設定 ASP.NET Core 應用程式

1. `Startup.Configure` 方法中提供的程式碼會顯示 *Hello World!*。 請以 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 的呼叫取代 `app.Run` 方法呼叫。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    上述程式碼可讓伺服器找出並提供 *index.html* 檔案，而不論使用者輸入的是其完整 URL 還是 Web 應用程式的根目錄 URL。

1. 在中呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) `Startup.ConfigureServices` 。 它會將 SignalR 服務新增至專案。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. 將 */hub* 路由對應至 `ChatHub` 中樞。 在結尾新增下列幾行 `Startup.Configure` ：

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. 在專案根目錄中建立名為 *Hubs* 的新目錄。 其目的是要儲存 SignalR 中樞，這是在下一個步驟中建立的。

1. 使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. 在 *Startup.cs* 檔案的頂端新增下列程式碼，以解析 `ChatHub` 參考：

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>啟用用戶端與伺服器的通訊

應用程式目前會顯示一個簡單的表單來傳送訊息。 當您嘗試這樣做時，不會執行任何動作。 伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。

1. 在專案根目錄中執行下列命令：

    ```console
    npm install @aspnet/signalr
    ```

    上述命令會安裝[ SignalR TypeScript 用戶端](https://www.npmjs.com/package/@microsoft/signalr)，讓用戶端可以將訊息傳送至伺服器。

1. 將醒目提示的程式碼新增至 *src/index.ts* 檔案：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    上述程式碼支援從伺服器接收訊息。 `HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。 `withUrl` 函式則會設定中樞 URL。

    SignalR啟用用戶端與伺服器之間的訊息交換。 每個訊息都有特定的名稱。 例如，具有名稱的訊息 `messageReceived` 可以執行負責在 [訊息] 區域中顯示新訊息的邏輯。 接聽特定的訊息可透過 `on` 函式完成。 您可以接聽任意數目的訊息名稱。 也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。 用戶端收到訊息之後，就會在其 `innerHTML` 屬性中使用作者的名稱和訊息內容建立新的 `div` 元素。 新的訊息會新增至 `div` 顯示訊息的主要元素。

1. 現在用戶端可以接收訊息，請設定它來傳送訊息。 將醒目提示的程式碼新增至 *src/index.ts* 檔案：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。 方法的第一個參數是訊息名稱。 訊息資料則佔用其他參數。 在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。 訊息是由使用者名稱和文字方塊中的使用者輸入所組成。 如果傳送可正常運作，則會清除文字方塊的值。

1. 將 `NewMessage` 方法新增至 `ChatHub` 類別：

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。 不需要讓泛型 `on` 方法接收所有訊息。 以訊息名稱命名方法就已足夠。

    在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。 C# `NewMessage` 方法預期有用戶端所傳送的資料。 在[用戶端](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all)上進行[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)的呼叫。 接收的訊息就會傳送到連線至中樞的所有用戶端。

## <a name="test-the-app"></a>測試應用程式

使用下列步驟確認應用程式可正常運作。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 在 *release* 模式下執行 Webpack。 使用 [**套件管理員主控台**] 視窗，在專案根目錄中執行下列命令。 如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. 選取**Debug**  >  [**不進行調試**程式的 Debug Start]，在瀏覽器中啟動應用程式，而不附加偵錯工具。 隨即會在 `http://localhost:<port_number>` 處提供 *wwwroot/index.html* 檔案。

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)。 在網址列中貼上 URL。

1. 選擇其中一個瀏覽器，在 [訊息]**** 文字方塊中鍵入某些內容，然後按一下 [傳送]**** 按鈕。 唯一使用者名稱和訊息會立即顯示在這兩個頁面上。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. 在專案根目錄中執行下列命令，藉以建置並執行應用程式：

    ```dotnetcli
    dotnet run
    ```

    Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。

1. 開啟瀏覽器並前往 `http://localhost:<port_number>`。 隨即會提供 *wwwroot/index.html* 檔案。 從網址列複製 URL。

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)。 在網址列中貼上 URL。

1. 選擇其中一個瀏覽器，在 [訊息]**** 文字方塊中鍵入某些內容，然後按一下 [傳送]**** 按鈕。 唯一使用者名稱和訊息會立即顯示在這兩個頁面上。

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
