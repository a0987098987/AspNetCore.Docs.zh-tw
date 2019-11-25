---
title: 搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR
author: ssougnez
description: 在本教學課程中，您會將 Webpack 設定為配套，並建立用戶端以 TypeScript 撰寫的 ASP.NET Core SignalR web 應用程式。
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317478"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="2b94a-103">搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2b94a-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="2b94a-104">作者：[Sébastien Sougnez](https://twitter.com/ssougnez) 和 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="2b94a-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2b94a-105">[Webpack](https://webpack.js.org/) 可讓開發人員組合並建置 Web 應用程式的用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="2b94a-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="2b94a-106">本教學課程示範如何在用戶端以[TypeScript](https://www.typescriptlang.org/)撰寫的 ASP.NET Core SignalR web 應用程式中使用 Webpack。</span><span class="sxs-lookup"><span data-stu-id="2b94a-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="2b94a-107">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="2b94a-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2b94a-108">Scaffold 入門 ASP.NET Core SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="2b94a-109">設定 SignalR TypeScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="2b94a-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="2b94a-110">使用 Webpack 設定組建管線</span><span class="sxs-lookup"><span data-stu-id="2b94a-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="2b94a-111">設定 SignalR 伺服器</span><span class="sxs-lookup"><span data-stu-id="2b94a-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="2b94a-112">啟用用戶端與伺服器之間的通訊</span><span class="sxs-lookup"><span data-stu-id="2b94a-112">Enable communication between client and server</span></span>

<span data-ttu-id="2b94a-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b94a-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="2b94a-114">先決條件</span><span class="sxs-lookup"><span data-stu-id="2b94a-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b94a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b94a-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2b94a-116">隨附 [ASP.NET 和網頁程式開發](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)工作負載的 **Visual Studio 2019**</span><span class="sxs-lookup"><span data-stu-id="2b94a-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2b94a-117">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="2b94a-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="2b94a-118">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="2b94a-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b94a-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2b94a-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2b94a-121">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="2b94a-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2b94a-122">適用於 Visual Studio Code 1.17.1 版或更新版本的 C#</span><span class="sxs-lookup"><span data-stu-id="2b94a-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="2b94a-123">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="2b94a-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="2b94a-124">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b94a-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b94a-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2b94a-126">設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。</span><span class="sxs-lookup"><span data-stu-id="2b94a-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="2b94a-127">根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="2b94a-128">請遵循 Visual Studio 中的下列指示：</span><span class="sxs-lookup"><span data-stu-id="2b94a-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="2b94a-129">流覽至 [**工具**] >**選項**> [**專案和方案**] > **Web 套件管理**> [**外部 web 工具**]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="2b94a-130">從清單中選取 *$(PATH)* 項目。</span><span class="sxs-lookup"><span data-stu-id="2b94a-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="2b94a-131">按一下向上箭號，將此項目移至清單中的第二個位置。</span><span class="sxs-lookup"><span data-stu-id="2b94a-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio 組態](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="2b94a-133">Visual Studio 組態已完成。</span><span class="sxs-lookup"><span data-stu-id="2b94a-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="2b94a-134">現在即可開始建立專案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-134">It's time to create the project.</span></span>

1. <span data-ttu-id="2b94a-135">使用 [檔案] > [**新增**>**專案**] 功能表**選項，然後**選擇 [ **ASP.NET Core Web 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="2b94a-136">將專案命名為*命名為 signalrwebpack*，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="2b94a-137">從 [目標 framework] 下拉式選單中選取 [ *.Net Core* ]，然後從 [framework 選取器] 下拉式選單中選取 [ *ASP.NET Core 3.0* ]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="2b94a-138">選取 [**空白**] 範本，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b94a-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2b94a-140">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b94a-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="2b94a-141">隨即會在 *SignalRWebPack* 目錄中建立以 .NET Core 為目標的空白 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="2b94a-142">設定 Webpack 和 TypeScript</span><span class="sxs-lookup"><span data-stu-id="2b94a-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="2b94a-143">下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。</span><span class="sxs-lookup"><span data-stu-id="2b94a-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="2b94a-144">在專案根目錄中執行下列命令來建立 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="2b94a-145">將醒目提示的屬性新增至 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="2b94a-146">將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。</span><span class="sxs-lookup"><span data-stu-id="2b94a-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="2b94a-147">安裝必要的 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-147">Install the required npm packages.</span></span> <span data-ttu-id="2b94a-148">從專案的根目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b94a-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="2b94a-149">要注意的一些命令詳細資料：</span><span class="sxs-lookup"><span data-stu-id="2b94a-149">Some command details to note:</span></span>

    * <span data-ttu-id="2b94a-150">版本號碼接在每一個套件名稱的 `@` 符號之後。</span><span class="sxs-lookup"><span data-stu-id="2b94a-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="2b94a-151">npm 會安裝這些特定的套件版本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="2b94a-152">`-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。</span><span class="sxs-lookup"><span data-stu-id="2b94a-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="2b94a-153">例如，會使用 `"webpack": "4.29.3"`，而不是 `"webpack": "^4.29.3"`。</span><span class="sxs-lookup"><span data-stu-id="2b94a-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="2b94a-154">此選項可防止意外升級至較新的套件版本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="2b94a-155">如需詳細資料，請參閱官方的 [npm-install](https://docs.npmjs.com/cli/install) 文件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="2b94a-156">以下列程式碼片段取代 `scripts`package.json*檔案的* 屬性：</span><span class="sxs-lookup"><span data-stu-id="2b94a-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="2b94a-157">指令碼的一些說明：</span><span class="sxs-lookup"><span data-stu-id="2b94a-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="2b94a-158">`build`：在開發模式下組合您的用戶端資源，並監看檔案變更。</span><span class="sxs-lookup"><span data-stu-id="2b94a-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="2b94a-159">檔案監看員會導致套件組合在每次專案檔變更時重新產生。</span><span class="sxs-lookup"><span data-stu-id="2b94a-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="2b94a-160">`mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。</span><span class="sxs-lookup"><span data-stu-id="2b94a-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="2b94a-161">`build` 僅用於開發。</span><span class="sxs-lookup"><span data-stu-id="2b94a-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="2b94a-162">`release`：在生產模式下組合您的用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="2b94a-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="2b94a-163">`publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="2b94a-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="2b94a-164">它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="2b94a-165">使用下列內容，在專案根目錄中建立名為 *webpack.config.js* 的檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="2b94a-166">上述檔案會設定 Webpack 編譯。</span><span class="sxs-lookup"><span data-stu-id="2b94a-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="2b94a-167">要注意的一些組態詳細資料：</span><span class="sxs-lookup"><span data-stu-id="2b94a-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="2b94a-168">`output` 屬性會覆寫 *dist* 的預設值。</span><span class="sxs-lookup"><span data-stu-id="2b94a-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="2b94a-169">因而在 *wwwroot* 目錄中發出套件組合。</span><span class="sxs-lookup"><span data-stu-id="2b94a-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="2b94a-170">`resolve.extensions` 陣列會包含 *.js*以匯入 SignalR 用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="2b94a-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="2b94a-171">在專案根目錄中建立新的 *src* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2b94a-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="2b94a-172">其目的是要儲存專案的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="2b94a-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="2b94a-173">使用下列內容建立 *src/index.html*。</span><span class="sxs-lookup"><span data-stu-id="2b94a-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="2b94a-174">上述的 HTML 會定義首頁的樣板標記。</span><span class="sxs-lookup"><span data-stu-id="2b94a-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="2b94a-175">建立新的 *src/css* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2b94a-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="2b94a-176">其目的是要儲存專案的 *.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="2b94a-177">使用下列內容建立 *rc/css/main.css*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="2b94a-178">上述的 *main.css* 檔案會設定應用程式的樣式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="2b94a-179">使用下列內容建立 *src/tsconfig.json*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="2b94a-180">上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="2b94a-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="2b94a-181">使用下列內容建立 *src/index.ts*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="2b94a-182">上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="2b94a-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="2b94a-183">`keyup`：當使用者在識別為 `tbMessage` 的文字方塊中鍵入某些內容時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="2b94a-184">當使用者按下 `send`Enter**鍵時，即會呼叫** 函式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="2b94a-185">`click`：當使用者按一下 [傳送] 按鈕時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="2b94a-186">系統會呼叫 `send` 函式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="2b94a-187">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="2b94a-188">在 `Startup.Configure` 方法中，將呼叫新增至[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。</span><span class="sxs-lookup"><span data-stu-id="2b94a-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="2b94a-189">上述程式碼可讓伺服器找出並提供 *index.html* 檔案，而不論使用者輸入的是其完整 URL 還是 Web 應用程式的根目錄 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="2b94a-190">在 `Startup.Configure` 方法的結尾，將 */hub*路由對應至 `ChatHub` 中樞。</span><span class="sxs-lookup"><span data-stu-id="2b94a-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="2b94a-191">取代顯示 Hello World 的程式碼 *！*</span><span class="sxs-lookup"><span data-stu-id="2b94a-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="2b94a-192">具有下列這一行：</span><span class="sxs-lookup"><span data-stu-id="2b94a-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="2b94a-193">在 `Startup.ConfigureServices` 方法中，呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)。</span><span class="sxs-lookup"><span data-stu-id="2b94a-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="2b94a-194">它會將 SignalR 服務新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="2b94a-195">在專案根目錄中建立名為 *Hubs* 的新目錄。</span><span class="sxs-lookup"><span data-stu-id="2b94a-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="2b94a-196">其目的是要儲存在下一個步驟中建立的 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="2b94a-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="2b94a-197">使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="2b94a-198">在 *Startup.cs* 檔案的頂端新增下列程式碼，以解析 `ChatHub` 參考：</span><span class="sxs-lookup"><span data-stu-id="2b94a-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="2b94a-199">啟用用戶端與伺服器的通訊</span><span class="sxs-lookup"><span data-stu-id="2b94a-199">Enable client and server communication</span></span>

<span data-ttu-id="2b94a-200">應用程式目前會顯示一個簡單的表單來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="2b94a-201">當您嘗試這樣做時，不會執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="2b94a-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="2b94a-202">伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。</span><span class="sxs-lookup"><span data-stu-id="2b94a-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="2b94a-203">在專案的根目錄下執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b94a-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="2b94a-204">上述命令會安裝[SignalR TypeScript 用戶端](https://www.npmjs.com/package/@microsoft/signalr)，以允許用戶端將訊息傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="2b94a-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="2b94a-205">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="2b94a-206">上述程式碼支援從伺服器接收訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="2b94a-207">`HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。</span><span class="sxs-lookup"><span data-stu-id="2b94a-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="2b94a-208">`withUrl` 函式則會設定中樞 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="2b94a-209"> 可在用戶端與伺服器之間交換訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="2b94a-210">每個訊息都有特定的名稱。</span><span class="sxs-lookup"><span data-stu-id="2b94a-210">Each message has a specific name.</span></span> <span data-ttu-id="2b94a-211">例如，您可以包含名稱為 `messageReceived` 的訊息，用來執行負責在訊息區域中顯示新訊息的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2b94a-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="2b94a-212">接聽特定的訊息可透過 `on` 函式完成。</span><span class="sxs-lookup"><span data-stu-id="2b94a-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="2b94a-213">您可以接聽任意數目的訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="2b94a-213">You can listen to any number of message names.</span></span> <span data-ttu-id="2b94a-214">也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。</span><span class="sxs-lookup"><span data-stu-id="2b94a-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="2b94a-215">用戶端收到訊息之後，就會在其 `div` 屬性中使用作者的名稱和訊息內容建立新的 `innerHTML` 元素。</span><span class="sxs-lookup"><span data-stu-id="2b94a-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="2b94a-216">它會新增至顯示訊息的主要 `div` 元素。</span><span class="sxs-lookup"><span data-stu-id="2b94a-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="2b94a-217">現在用戶端可以接收訊息，請設定它來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="2b94a-218">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="2b94a-219">透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。</span><span class="sxs-lookup"><span data-stu-id="2b94a-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="2b94a-220">方法的第一個參數是訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="2b94a-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="2b94a-221">訊息資料則佔用其他參數。</span><span class="sxs-lookup"><span data-stu-id="2b94a-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="2b94a-222">在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="2b94a-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="2b94a-223">訊息是由使用者名稱和文字方塊中的使用者輸入所組成。</span><span class="sxs-lookup"><span data-stu-id="2b94a-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="2b94a-224">如果傳送可正常運作，則會清除文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="2b94a-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="2b94a-225">將醒目提示的方法新增至 `ChatHub` 類別：</span><span class="sxs-lookup"><span data-stu-id="2b94a-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="2b94a-226">上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="2b94a-227">不需要讓泛型 `on` 方法接收所有訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="2b94a-228">以訊息名稱命名方法就已足夠。</span><span class="sxs-lookup"><span data-stu-id="2b94a-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="2b94a-229">在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="2b94a-230">C# `NewMessage` 方法預期有用戶端所傳送的資料。</span><span class="sxs-lookup"><span data-stu-id="2b94a-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="2b94a-231">將會在 [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 上呼叫 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) 方法。</span><span class="sxs-lookup"><span data-stu-id="2b94a-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="2b94a-232">接收的訊息就會傳送到連線至中樞的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="2b94a-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="2b94a-233">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-233">Test the app</span></span>

<span data-ttu-id="2b94a-234">使用下列步驟確認應用程式可正常運作。</span><span class="sxs-lookup"><span data-stu-id="2b94a-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b94a-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b94a-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2b94a-236">在 *release* 模式下執行 Webpack。</span><span class="sxs-lookup"><span data-stu-id="2b94a-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="2b94a-237">使用 [套件管理員主控台] 視窗，在專案根目錄中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="2b94a-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="2b94a-238">如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。</span><span class="sxs-lookup"><span data-stu-id="2b94a-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="2b94a-239">選取 [偵錯] > [啟動但不偵錯]，啟動瀏覽器中的應用程式，但不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="2b94a-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="2b94a-240">隨即會在 *處提供*wwwroot/index.html`http://localhost:<port_number>` 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="2b94a-241">如果您遇到編譯錯誤，請嘗試關閉並重新開啟方案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="2b94a-242">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="2b94a-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="2b94a-243">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2b94a-244">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2b94a-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="2b94a-245">唯一使用者名稱和訊息會立即顯示在這兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="2b94a-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b94a-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2b94a-247">在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：</span><span class="sxs-lookup"><span data-stu-id="2b94a-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="2b94a-248">在專案根目錄中執行下列命令，藉以建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="2b94a-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="2b94a-249">Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。</span><span class="sxs-lookup"><span data-stu-id="2b94a-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="2b94a-250">開啟瀏覽器並前往 `http://localhost:<port_number>`。</span><span class="sxs-lookup"><span data-stu-id="2b94a-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="2b94a-251">隨即會提供 *wwwroot/index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="2b94a-252">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="2b94a-253">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="2b94a-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="2b94a-254">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2b94a-255">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2b94a-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="2b94a-256">唯一使用者名稱和訊息會立即顯示在這兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="2b94a-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b94a-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b94a-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2b94a-259">隨附 [ASP.NET 和網頁程式開發](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)工作負載的 **Visual Studio 2019**</span><span class="sxs-lookup"><span data-stu-id="2b94a-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2b94a-260">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="2b94a-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="2b94a-261">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="2b94a-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b94a-262">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2b94a-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2b94a-264">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="2b94a-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2b94a-265">適用於 Visual Studio Code 1.17.1 版或更新版本的 C#</span><span class="sxs-lookup"><span data-stu-id="2b94a-265">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="2b94a-266">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="2b94a-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="2b94a-267">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b94a-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b94a-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2b94a-269">設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。</span><span class="sxs-lookup"><span data-stu-id="2b94a-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="2b94a-270">根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="2b94a-271">請遵循 Visual Studio 中的下列指示：</span><span class="sxs-lookup"><span data-stu-id="2b94a-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="2b94a-272">流覽至 [**工具**] >**選項**> [**專案和方案**] > **Web 套件管理**> [**外部 web 工具**]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="2b94a-273">從清單中選取 *$(PATH)* 項目。</span><span class="sxs-lookup"><span data-stu-id="2b94a-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="2b94a-274">按一下向上箭號，將此項目移至清單中的第二個位置。</span><span class="sxs-lookup"><span data-stu-id="2b94a-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio 組態](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="2b94a-276">Visual Studio 組態已完成。</span><span class="sxs-lookup"><span data-stu-id="2b94a-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="2b94a-277">現在即可開始建立專案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-277">It's time to create the project.</span></span>

1. <span data-ttu-id="2b94a-278">使用 [檔案] > [**新增**>**專案**] 功能表**選項，然後**選擇 [ **ASP.NET Core Web 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="2b94a-279">將專案命名為*命名為 signalrwebpack*，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="2b94a-280">從目標 Framework 下拉式清單中選取 [.NET Core]，然後從 Framework 選取器下拉式清單中選取 [ASP.NET Core 2.2]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="2b94a-281">選取 [**空白**] 範本，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="2b94a-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b94a-282">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2b94a-283">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b94a-283">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="2b94a-284">隨即會在 *SignalRWebPack* 目錄中建立以 .NET Core 為目標的空白 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="2b94a-285">設定 Webpack 和 TypeScript</span><span class="sxs-lookup"><span data-stu-id="2b94a-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="2b94a-286">下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。</span><span class="sxs-lookup"><span data-stu-id="2b94a-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="2b94a-287">在專案根目錄中執行下列命令來建立 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="2b94a-288">將醒目提示的屬性新增至 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="2b94a-289">將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。</span><span class="sxs-lookup"><span data-stu-id="2b94a-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="2b94a-290">安裝必要的 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-290">Install the required npm packages.</span></span> <span data-ttu-id="2b94a-291">從專案的根目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b94a-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="2b94a-292">要注意的一些命令詳細資料：</span><span class="sxs-lookup"><span data-stu-id="2b94a-292">Some command details to note:</span></span>

    * <span data-ttu-id="2b94a-293">版本號碼接在每一個套件名稱的 `@` 符號之後。</span><span class="sxs-lookup"><span data-stu-id="2b94a-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="2b94a-294">npm 會安裝這些特定的套件版本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="2b94a-295">`-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。</span><span class="sxs-lookup"><span data-stu-id="2b94a-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="2b94a-296">例如，會使用 `"webpack": "4.29.3"`，而不是 `"webpack": "^4.29.3"`。</span><span class="sxs-lookup"><span data-stu-id="2b94a-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="2b94a-297">此選項可防止意外升級至較新的套件版本。</span><span class="sxs-lookup"><span data-stu-id="2b94a-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="2b94a-298">如需詳細資料，請參閱官方的 [npm-install](https://docs.npmjs.com/cli/install) 文件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="2b94a-299">以下列程式碼片段取代 `scripts`package.json*檔案的* 屬性：</span><span class="sxs-lookup"><span data-stu-id="2b94a-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="2b94a-300">指令碼的一些說明：</span><span class="sxs-lookup"><span data-stu-id="2b94a-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="2b94a-301">`build`：在開發模式下組合您的用戶端資源，並監看檔案變更。</span><span class="sxs-lookup"><span data-stu-id="2b94a-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="2b94a-302">檔案監看員會導致套件組合在每次專案檔變更時重新產生。</span><span class="sxs-lookup"><span data-stu-id="2b94a-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="2b94a-303">`mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。</span><span class="sxs-lookup"><span data-stu-id="2b94a-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="2b94a-304">`build` 僅用於開發。</span><span class="sxs-lookup"><span data-stu-id="2b94a-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="2b94a-305">`release`：在生產模式下組合您的用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="2b94a-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="2b94a-306">`publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="2b94a-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="2b94a-307">它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="2b94a-308">使用下列內容，在專案根目錄中建立名為 *webpack.config.js* 的檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="2b94a-309">上述檔案會設定 Webpack 編譯。</span><span class="sxs-lookup"><span data-stu-id="2b94a-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="2b94a-310">要注意的一些組態詳細資料：</span><span class="sxs-lookup"><span data-stu-id="2b94a-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="2b94a-311">`output` 屬性會覆寫 *dist* 的預設值。</span><span class="sxs-lookup"><span data-stu-id="2b94a-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="2b94a-312">因而在 *wwwroot* 目錄中發出套件組合。</span><span class="sxs-lookup"><span data-stu-id="2b94a-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="2b94a-313">`resolve.extensions` 陣列會包含 *.js*以匯入 SignalR 用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="2b94a-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="2b94a-314">在專案根目錄中建立新的 *src* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2b94a-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="2b94a-315">其目的是要儲存專案的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="2b94a-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="2b94a-316">使用下列內容建立 *src/index.html*。</span><span class="sxs-lookup"><span data-stu-id="2b94a-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="2b94a-317">上述的 HTML 會定義首頁的樣板標記。</span><span class="sxs-lookup"><span data-stu-id="2b94a-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="2b94a-318">建立新的 *src/css* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2b94a-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="2b94a-319">其目的是要儲存專案的 *.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="2b94a-320">使用下列內容建立 *rc/css/main.css*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="2b94a-321">上述的 *main.css* 檔案會設定應用程式的樣式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="2b94a-322">使用下列內容建立 *src/tsconfig.json*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="2b94a-323">上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="2b94a-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="2b94a-324">使用下列內容建立 *src/index.ts*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="2b94a-325">上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="2b94a-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="2b94a-326">`keyup`：當使用者在識別為 `tbMessage` 的文字方塊中鍵入某些內容時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="2b94a-327">當使用者按下 `send`Enter**鍵時，即會呼叫** 函式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="2b94a-328">`click`：當使用者按一下 [傳送] 按鈕時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="2b94a-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="2b94a-329">系統會呼叫 `send` 函式。</span><span class="sxs-lookup"><span data-stu-id="2b94a-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="2b94a-330">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="2b94a-331">`Startup.Configure` 方法中提供的程式碼會顯示 *Hello World!* 。</span><span class="sxs-lookup"><span data-stu-id="2b94a-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="2b94a-332">請以 `app.Run`UseDefaultFiles[ 和 ](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)UseStaticFiles[ 的呼叫取代 ](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="2b94a-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="2b94a-333">上述程式碼可讓伺服器找出並提供 *index.html* 檔案，而不論使用者輸入的是其完整 URL 還是 Web 應用程式的根目錄 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="2b94a-334">在 [ 方法中呼叫 ](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)AddSignalR`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="2b94a-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="2b94a-335">它會將 SignalR 服務新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="2b94a-336">將 */hub* 路由對應至 `ChatHub` 中樞。</span><span class="sxs-lookup"><span data-stu-id="2b94a-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="2b94a-337">在 `Startup.Configure` 方法的結尾，新增下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="2b94a-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="2b94a-338">在專案根目錄中建立名為 *Hubs* 的新目錄。</span><span class="sxs-lookup"><span data-stu-id="2b94a-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="2b94a-339">其目的是要儲存在下一個步驟中建立的 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="2b94a-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="2b94a-340">使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：</span><span class="sxs-lookup"><span data-stu-id="2b94a-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="2b94a-341">在 *Startup.cs* 檔案的頂端新增下列程式碼，以解析 `ChatHub` 參考：</span><span class="sxs-lookup"><span data-stu-id="2b94a-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="2b94a-342">啟用用戶端與伺服器的通訊</span><span class="sxs-lookup"><span data-stu-id="2b94a-342">Enable client and server communication</span></span>

<span data-ttu-id="2b94a-343">應用程式目前會顯示一個簡單的表單來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="2b94a-344">當您嘗試這樣做時，不會執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="2b94a-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="2b94a-345">伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。</span><span class="sxs-lookup"><span data-stu-id="2b94a-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="2b94a-346">在專案的根目錄下執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2b94a-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="2b94a-347">上述命令會安裝[SignalR TypeScript 用戶端](https://www.npmjs.com/package/@microsoft/signalr)，以允許用戶端將訊息傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="2b94a-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="2b94a-348">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="2b94a-349">上述程式碼支援從伺服器接收訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="2b94a-350">`HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。</span><span class="sxs-lookup"><span data-stu-id="2b94a-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="2b94a-351">`withUrl` 函式則會設定中樞 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-351">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="2b94a-352"> 可在用戶端與伺服器之間交換訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-352"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="2b94a-353">每個訊息都有特定的名稱。</span><span class="sxs-lookup"><span data-stu-id="2b94a-353">Each message has a specific name.</span></span> <span data-ttu-id="2b94a-354">例如，您可以包含名稱為 `messageReceived` 的訊息，用來執行負責在訊息區域中顯示新訊息的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2b94a-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="2b94a-355">接聽特定的訊息可透過 `on` 函式完成。</span><span class="sxs-lookup"><span data-stu-id="2b94a-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="2b94a-356">您可以接聽任意數目的訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="2b94a-356">You can listen to any number of message names.</span></span> <span data-ttu-id="2b94a-357">也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。</span><span class="sxs-lookup"><span data-stu-id="2b94a-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="2b94a-358">用戶端收到訊息之後，就會在其 `div` 屬性中使用作者的名稱和訊息內容建立新的 `innerHTML` 元素。</span><span class="sxs-lookup"><span data-stu-id="2b94a-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="2b94a-359">它會新增至顯示訊息的主要 `div` 元素。</span><span class="sxs-lookup"><span data-stu-id="2b94a-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="2b94a-360">現在用戶端可以接收訊息，請設定它來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="2b94a-361">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="2b94a-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="2b94a-362">透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。</span><span class="sxs-lookup"><span data-stu-id="2b94a-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="2b94a-363">方法的第一個參數是訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="2b94a-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="2b94a-364">訊息資料則佔用其他參數。</span><span class="sxs-lookup"><span data-stu-id="2b94a-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="2b94a-365">在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="2b94a-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="2b94a-366">訊息是由使用者名稱和文字方塊中的使用者輸入所組成。</span><span class="sxs-lookup"><span data-stu-id="2b94a-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="2b94a-367">如果傳送可正常運作，則會清除文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="2b94a-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="2b94a-368">將醒目提示的方法新增至 `ChatHub` 類別：</span><span class="sxs-lookup"><span data-stu-id="2b94a-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="2b94a-369">上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="2b94a-370">不需要讓泛型 `on` 方法接收所有訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="2b94a-371">以訊息名稱命名方法就已足夠。</span><span class="sxs-lookup"><span data-stu-id="2b94a-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="2b94a-372">在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="2b94a-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="2b94a-373">C# `NewMessage` 方法預期有用戶端所傳送的資料。</span><span class="sxs-lookup"><span data-stu-id="2b94a-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="2b94a-374">將會在 [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 上呼叫 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) 方法。</span><span class="sxs-lookup"><span data-stu-id="2b94a-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="2b94a-375">接收的訊息就會傳送到連線至中樞的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="2b94a-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="2b94a-376">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="2b94a-376">Test the app</span></span>

<span data-ttu-id="2b94a-377">使用下列步驟確認應用程式可正常運作。</span><span class="sxs-lookup"><span data-stu-id="2b94a-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b94a-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b94a-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2b94a-379">在 *release* 模式下執行 Webpack。</span><span class="sxs-lookup"><span data-stu-id="2b94a-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="2b94a-380">使用 [套件管理員主控台] 視窗，在專案根目錄中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="2b94a-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="2b94a-381">如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。</span><span class="sxs-lookup"><span data-stu-id="2b94a-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="2b94a-382">選取 [偵錯] > [啟動但不偵錯]，啟動瀏覽器中的應用程式，但不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="2b94a-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="2b94a-383">隨即會在 *處提供*wwwroot/index.html`http://localhost:<port_number>` 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="2b94a-384">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="2b94a-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="2b94a-385">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2b94a-386">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2b94a-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="2b94a-387">唯一使用者名稱和訊息會立即顯示在這兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="2b94a-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2b94a-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b94a-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2b94a-389">在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：</span><span class="sxs-lookup"><span data-stu-id="2b94a-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="2b94a-390">在專案根目錄中執行下列命令，藉以建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="2b94a-390">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="2b94a-391">Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。</span><span class="sxs-lookup"><span data-stu-id="2b94a-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="2b94a-392">開啟瀏覽器並前往 `http://localhost:<port_number>`。</span><span class="sxs-lookup"><span data-stu-id="2b94a-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="2b94a-393">隨即會提供 *wwwroot/index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b94a-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="2b94a-394">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="2b94a-395">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="2b94a-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="2b94a-396">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="2b94a-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2b94a-397">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2b94a-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="2b94a-398">唯一使用者名稱和訊息會立即顯示在這兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="2b94a-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2b94a-400">其他資源</span><span class="sxs-lookup"><span data-stu-id="2b94a-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
