---
title: 搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR
author: ssougnez
description: 在本教學課程中，您可以設定 Webpack 來組合並建置其用戶端以 TypeScript 撰寫的 ASP.NET Core SignalR Web 應用程式。
ms.author: bradyg
ms.custom: mvc
ms.date: 04/23/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 8bebdd9102f93d2b2a8bf142db1053def9d001a0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64884603"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR

作者：[Sébastien Sougnez](https://twitter.com/ssougnez) 和 [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) 可讓開發人員組合並建置 Web 應用程式的用戶端資源。 本教學課程示範如何在其以 [TypeScript](https://www.typescriptlang.org/) 撰寫的 ASP.NET Core SignalR Web 應用程式中使用 Webpack。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * Scaffold 入門 ASP.NET Core SignalR 應用程式
> * 設定 SignalR TypeScript 用戶端
> * 使用 Webpack 設定組建管線
> * 設定 SignalR 伺服器
> * 啟用用戶端與伺服器之間的通訊

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 15.9 版或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)，包含 **ASP.NET 與網頁程式開發**工作負載
* [.NET Core SDK 2.2 或更新版本](https://www.microsoft.com/net/download/all)
* 具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.2 或更新版本](https://www.microsoft.com/net/download/all)
* [適用於 Visual Studio Code 1.17.1 版或更新版本的 C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* 具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)

---

## <a name="create-the-aspnet-core-web-app"></a>建立 ASP.NET Core Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。 根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。 請遵循 Visual Studio 中的下列指示：

1. 巡覽至 [工具] > [選項] > [專案和方案] > [網頁套件管理] > [外部 Web 工具]。
1. 從清單中選取 *$(PATH)* 項目。 按一下向上箭號，將此項目移至清單中的第二個位置。

    ![Visual Studio 組態](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio 組態已完成。 現在即可開始建立專案。

1. 使用 [檔案] > [新增] > [專案] 功能表選項，然後選擇 [ASP.NET Core Web 應用程式] 範本。
1. 將專案命名為 *SignalRWebPack*，然後選取 [確定]。
1. 從目標 Framework 下拉式清單中選取 [.NET Core]，然後從 Framework 選取器下拉式清單中選取 [ASP.NET Core 2.2]。 選取 [空白] 範本，然後選取 [確定]。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在 [整合式終端機] 中執行下列命令：

```console
dotnet new web -o SignalRWebPack
```

隨即會在 *SignalRWebPack* 目錄中建立以 .NET Core 為目標的空白 ASP.NET Core Web 應用程式。

---

## <a name="configure-webpack-and-typescript"></a>設定 Webpack 和 TypeScript

下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。

1. 在專案根目錄中執行下列命令來建立 *package.json* 檔案：

    ```console
    npm init -y
    ```

1. 將醒目提示的屬性新增至 *package.json* 檔案：

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。

1. 安裝必要的 npm 套件。 從專案的根目錄中執行下列命令：

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    要注意的一些命令詳細資料：

    * 版本號碼接在每一個套件名稱的 `@` 符號之後。 npm 會安裝這些特定的套件版本。
    * `-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。 例如，會使用 `"webpack": "4.29.3"`，而不是 `"webpack": "^4.29.3"`。 此選項可防止意外升級至較新的套件版本。

    如需詳細資料，請參閱官方的 [npm-install](https://docs.npmjs.com/cli/install) 文件。

1. 以下列程式碼片段取代 *package.json* 檔案的 `scripts` 屬性：

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    指令碼的一些說明：

    * `build`：在開發模式下組合您的用戶端資源，並監看檔案變更。 檔案監看員會導致套件組合在每次專案檔變更時重新產生。 `mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。 `build` 僅用於開發。
    * `release`：在生產模式下組合您的用戶端資源。
    * `publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。 它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。

1. 使用下列內容，在專案根目錄中建立名為 *webpack.config.js* 的檔案：

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    上述檔案會設定 Webpack 編譯。 要注意的一些組態詳細資料：

    * `output` 屬性會覆寫 *dist* 的預設值。 因而在 *wwwroot* 目錄中發出套件組合。
    * `resolve.extensions` 陣列包含 *.js* 來匯入 SignalR 用戶端 JavaScript。

1. 在專案根目錄中建立新的 *src* 目錄。 其目的是要儲存專案的用戶端資產。

1. 使用下列內容建立 *src/index.html*。

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    上述的 HTML 會定義首頁的樣板標記。

1. 建立新的 *src/css* 目錄。 其目的是要儲存專案的 *.css* 檔案。

1. 使用下列內容建立 *rc/css/main.css*：

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    上述的 *main.css* 檔案會設定應用程式的樣式。

1. 使用下列內容建立 *src/tsconfig.json*：

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。

1. 使用下列內容建立 *src/index.ts*：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：

    * `keyup`：當使用者在識別為 `tbMessage` 的文字方塊中鍵入某些內容時，就會引發此事件。 當使用者按下 **Enter** 鍵時，即會呼叫 `send` 函式。
    * `click`：當使用者按一下 [傳送] 按鈕時，就會引發此事件。 系統會呼叫 `send` 函式。

## <a name="configure-the-aspnet-core-app"></a>設定 ASP.NET Core 應用程式

1. `Startup.Configure` 方法中提供的程式碼會顯示 *Hello World!*。 請以 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 的呼叫取代 `app.Run` 方法呼叫。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    上述程式碼可讓伺服器找出並提供 *index.html* 檔案，而不論使用者輸入的是其完整 URL 還是 Web 應用程式的根目錄 URL。

1. 在 `Startup.ConfigureServices` 方法中呼叫 [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)。 它會將 SignalR 服務新增至您的專案。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. 將 */hub* 路由對應至 `ChatHub` 中樞。 在 `Startup.Configure` 方法的結尾，新增下列程式碼行：

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. 在專案根目錄中建立名為 *Hubs* 的新目錄。 其目的是要儲存下一個步驟所建立的 SignalR 中樞。

1. 使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. 在 *Startup.cs* 檔案的頂端新增下列程式碼，以解析 `ChatHub` 參考：

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>啟用用戶端與伺服器的通訊

應用程式目前會顯示一個簡單的表單來傳送訊息。 當您嘗試這樣做時，不會執行任何動作。 伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。

1. 在專案的根目錄下執行下列命令：

    ```console
    npm install @aspnet/signalr
    ```

    上述命令會安裝 [SignalR TypeScript 用戶端](https://www.npmjs.com/package/@aspnet/signalr)，這可讓用戶端將訊息傳送至伺服器。

1. 將醒目提示的程式碼新增至 *src/index.ts* 檔案：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    上述程式碼支援從伺服器接收訊息。 `HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。 `withUrl` 函式則會設定中樞 URL。

    SignalR 可讓用戶端與伺服器之間進行訊息交換。 每個訊息都有特定的名稱。 例如，您可以包含名稱為 `messageReceived` 的訊息，用來執行負責在訊息區域中顯示新訊息的邏輯。 接聽特定的訊息可透過 `on` 函式完成。 您可以接聽任意數目的訊息名稱。 也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。 用戶端收到訊息之後，就會在其 `innerHTML` 屬性中使用作者的名稱和訊息內容建立新的 `div` 元素。 它會新增至顯示訊息的主要 `div` 元素。

1. 現在用戶端可以接收訊息，請設定它來傳送訊息。 將醒目提示的程式碼新增至 *src/index.ts* 檔案：

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。 方法的第一個參數是訊息名稱。 訊息資料則佔用其他參數。 在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。 訊息是由使用者名稱和文字方塊中的使用者輸入所組成。 如果傳送可正常運作，則會清除文字方塊的值。

1. 將醒目提示的方法新增至 `ChatHub` 類別：

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。 不需要讓泛型 `on` 方法接收所有訊息。 以訊息名稱命名方法就已足夠。

    在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。 C# `NewMessage` 方法預期有用戶端所傳送的資料。 將會在 [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) 上呼叫 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法。 接收的訊息就會傳送到連線至中樞的所有用戶端。

## <a name="test-the-app"></a>測試應用程式

使用下列步驟確認應用程式可正常運作。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 在 *release* 模式下執行 Webpack。 使用 [套件管理員主控台] 視窗，在專案根目錄中執行下列命令。 如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. 選取 [偵錯] > [啟動但不偵錯]，啟動瀏覽器中的應用程式，但不附加偵錯工具。 隨即會在 `http://localhost:<port_number>` 處提供 *wwwroot/index.html* 檔案。

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)。 在網址列中貼上 URL。

1. 選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。 唯一使用者名稱和訊息會立即顯示在兩個頁面上。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. 在專案根目錄中執行下列命令，藉以建置並執行應用程式：

    ```console
    dotnet run
    ```

    Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。

1. 開啟瀏覽器並前往 `http://localhost:<port_number>`。 隨即會提供 *wwwroot/index.html* 檔案。 從網址列複製 URL。

1. 開啟另一個瀏覽器執行個體 (任何瀏覽器)。 在網址列中貼上 URL。

1. 選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。 唯一使用者名稱和訊息會立即顯示在兩個頁面上。

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>其他資源

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
