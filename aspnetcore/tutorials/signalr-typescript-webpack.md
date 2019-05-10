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
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="1edf5-103">搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="1edf5-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="1edf5-104">作者：[Sébastien Sougnez](https://twitter.com/ssougnez) 和 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1edf5-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1edf5-105">[Webpack](https://webpack.js.org/) 可讓開發人員組合並建置 Web 應用程式的用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="1edf5-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="1edf5-106">本教學課程示範如何在其以 [TypeScript](https://www.typescriptlang.org/) 撰寫的 ASP.NET Core SignalR Web 應用程式中使用 Webpack。</span><span class="sxs-lookup"><span data-stu-id="1edf5-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="1edf5-107">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="1edf5-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1edf5-108">Scaffold 入門 ASP.NET Core SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="1edf5-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="1edf5-109">設定 SignalR TypeScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="1edf5-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="1edf5-110">使用 Webpack 設定組建管線</span><span class="sxs-lookup"><span data-stu-id="1edf5-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="1edf5-111">設定 SignalR 伺服器</span><span class="sxs-lookup"><span data-stu-id="1edf5-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="1edf5-112">啟用用戶端與伺服器之間的通訊</span><span class="sxs-lookup"><span data-stu-id="1edf5-112">Enable communication between client and server</span></span>

<span data-ttu-id="1edf5-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1edf5-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1edf5-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="1edf5-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1edf5-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1edf5-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1edf5-116">[Visual Studio 2017 15.9 版或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)，包含 **ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="1edf5-116">[Visual Studio 2017 version 15.9 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="1edf5-117">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1edf5-117">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="1edf5-118">具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="1edf5-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1edf5-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1edf5-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="1edf5-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1edf5-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="1edf5-121">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1edf5-121">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1edf5-122">適用於 Visual Studio Code 1.17.1 版或更新版本的 C#</span><span class="sxs-lookup"><span data-stu-id="1edf5-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="1edf5-123">具有 [npm](https://www.npmjs.com/) 的 [Node.js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="1edf5-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="1edf5-124">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="1edf5-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1edf5-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1edf5-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1edf5-126">設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。</span><span class="sxs-lookup"><span data-stu-id="1edf5-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="1edf5-127">根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。</span><span class="sxs-lookup"><span data-stu-id="1edf5-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="1edf5-128">請遵循 Visual Studio 中的下列指示：</span><span class="sxs-lookup"><span data-stu-id="1edf5-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="1edf5-129">巡覽至 [工具] > [選項] > [專案和方案] > [網頁套件管理] > [外部 Web 工具]。</span><span class="sxs-lookup"><span data-stu-id="1edf5-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="1edf5-130">從清單中選取 *$(PATH)* 項目。</span><span class="sxs-lookup"><span data-stu-id="1edf5-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="1edf5-131">按一下向上箭號，將此項目移至清單中的第二個位置。</span><span class="sxs-lookup"><span data-stu-id="1edf5-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio 組態](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="1edf5-133">Visual Studio 組態已完成。</span><span class="sxs-lookup"><span data-stu-id="1edf5-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="1edf5-134">現在即可開始建立專案。</span><span class="sxs-lookup"><span data-stu-id="1edf5-134">It's time to create the project.</span></span>

1. <span data-ttu-id="1edf5-135">使用 [檔案] > [新增] > [專案] 功能表選項，然後選擇 [ASP.NET Core Web 應用程式] 範本。</span><span class="sxs-lookup"><span data-stu-id="1edf5-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="1edf5-136">將專案命名為 *SignalRWebPack*，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="1edf5-136">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="1edf5-137">從目標 Framework 下拉式清單中選取 [.NET Core]，然後從 Framework 選取器下拉式清單中選取 [ASP.NET Core 2.2]。</span><span class="sxs-lookup"><span data-stu-id="1edf5-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="1edf5-138">選取 [空白] 範本，然後選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="1edf5-138">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1edf5-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1edf5-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1edf5-140">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1edf5-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="1edf5-141">隨即會在 *SignalRWebPack* 目錄中建立以 .NET Core 為目標的空白 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1edf5-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="1edf5-142">設定 Webpack 和 TypeScript</span><span class="sxs-lookup"><span data-stu-id="1edf5-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="1edf5-143">下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。</span><span class="sxs-lookup"><span data-stu-id="1edf5-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="1edf5-144">在專案根目錄中執行下列命令來建立 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1edf5-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="1edf5-145">將醒目提示的屬性新增至 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1edf5-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="1edf5-146">將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。</span><span class="sxs-lookup"><span data-stu-id="1edf5-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="1edf5-147">安裝必要的 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="1edf5-147">Install the required npm packages.</span></span> <span data-ttu-id="1edf5-148">從專案的根目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1edf5-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="1edf5-149">要注意的一些命令詳細資料：</span><span class="sxs-lookup"><span data-stu-id="1edf5-149">Some command details to note:</span></span>

    * <span data-ttu-id="1edf5-150">版本號碼接在每一個套件名稱的 `@` 符號之後。</span><span class="sxs-lookup"><span data-stu-id="1edf5-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="1edf5-151">npm 會安裝這些特定的套件版本。</span><span class="sxs-lookup"><span data-stu-id="1edf5-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="1edf5-152">`-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。</span><span class="sxs-lookup"><span data-stu-id="1edf5-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="1edf5-153">例如，會使用 `"webpack": "4.29.3"`，而不是 `"webpack": "^4.29.3"`。</span><span class="sxs-lookup"><span data-stu-id="1edf5-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="1edf5-154">此選項可防止意外升級至較新的套件版本。</span><span class="sxs-lookup"><span data-stu-id="1edf5-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="1edf5-155">如需詳細資料，請參閱官方的 [npm-install](https://docs.npmjs.com/cli/install) 文件。</span><span class="sxs-lookup"><span data-stu-id="1edf5-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="1edf5-156">以下列程式碼片段取代 *package.json* 檔案的 `scripts` 屬性：</span><span class="sxs-lookup"><span data-stu-id="1edf5-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="1edf5-157">指令碼的一些說明：</span><span class="sxs-lookup"><span data-stu-id="1edf5-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="1edf5-158">`build`：在開發模式下組合您的用戶端資源，並監看檔案變更。</span><span class="sxs-lookup"><span data-stu-id="1edf5-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="1edf5-159">檔案監看員會導致套件組合在每次專案檔變更時重新產生。</span><span class="sxs-lookup"><span data-stu-id="1edf5-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="1edf5-160">`mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。</span><span class="sxs-lookup"><span data-stu-id="1edf5-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="1edf5-161">`build` 僅用於開發。</span><span class="sxs-lookup"><span data-stu-id="1edf5-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="1edf5-162">`release`：在生產模式下組合您的用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="1edf5-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="1edf5-163">`publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="1edf5-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="1edf5-164">它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="1edf5-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="1edf5-165">使用下列內容，在專案根目錄中建立名為 *webpack.config.js* 的檔案：</span><span class="sxs-lookup"><span data-stu-id="1edf5-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="1edf5-166">上述檔案會設定 Webpack 編譯。</span><span class="sxs-lookup"><span data-stu-id="1edf5-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="1edf5-167">要注意的一些組態詳細資料：</span><span class="sxs-lookup"><span data-stu-id="1edf5-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="1edf5-168">`output` 屬性會覆寫 *dist* 的預設值。</span><span class="sxs-lookup"><span data-stu-id="1edf5-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="1edf5-169">因而在 *wwwroot* 目錄中發出套件組合。</span><span class="sxs-lookup"><span data-stu-id="1edf5-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="1edf5-170">`resolve.extensions` 陣列包含 *.js* 來匯入 SignalR 用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="1edf5-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="1edf5-171">在專案根目錄中建立新的 *src* 目錄。</span><span class="sxs-lookup"><span data-stu-id="1edf5-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="1edf5-172">其目的是要儲存專案的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="1edf5-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="1edf5-173">使用下列內容建立 *src/index.html*。</span><span class="sxs-lookup"><span data-stu-id="1edf5-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="1edf5-174">上述的 HTML 會定義首頁的樣板標記。</span><span class="sxs-lookup"><span data-stu-id="1edf5-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="1edf5-175">建立新的 *src/css* 目錄。</span><span class="sxs-lookup"><span data-stu-id="1edf5-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="1edf5-176">其目的是要儲存專案的 *.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1edf5-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="1edf5-177">使用下列內容建立 *rc/css/main.css*：</span><span class="sxs-lookup"><span data-stu-id="1edf5-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="1edf5-178">上述的 *main.css* 檔案會設定應用程式的樣式。</span><span class="sxs-lookup"><span data-stu-id="1edf5-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="1edf5-179">使用下列內容建立 *src/tsconfig.json*：</span><span class="sxs-lookup"><span data-stu-id="1edf5-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="1edf5-180">上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="1edf5-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="1edf5-181">使用下列內容建立 *src/index.ts*：</span><span class="sxs-lookup"><span data-stu-id="1edf5-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="1edf5-182">上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="1edf5-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="1edf5-183">`keyup`：當使用者在識別為 `tbMessage` 的文字方塊中鍵入某些內容時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="1edf5-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="1edf5-184">當使用者按下 **Enter** 鍵時，即會呼叫 `send` 函式。</span><span class="sxs-lookup"><span data-stu-id="1edf5-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="1edf5-185">`click`：當使用者按一下 [傳送] 按鈕時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="1edf5-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="1edf5-186">系統會呼叫 `send` 函式。</span><span class="sxs-lookup"><span data-stu-id="1edf5-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="1edf5-187">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="1edf5-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="1edf5-188">`Startup.Configure` 方法中提供的程式碼會顯示 *Hello World!*。</span><span class="sxs-lookup"><span data-stu-id="1edf5-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="1edf5-189">請以 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 的呼叫取代 `app.Run` 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="1edf5-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="1edf5-190">上述程式碼可讓伺服器找出並提供 *index.html* 檔案，而不論使用者輸入的是其完整 URL 還是 Web 應用程式的根目錄 URL。</span><span class="sxs-lookup"><span data-stu-id="1edf5-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="1edf5-191">在 `Startup.ConfigureServices` 方法中呼叫 [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)。</span><span class="sxs-lookup"><span data-stu-id="1edf5-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="1edf5-192">它會將 SignalR 服務新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="1edf5-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="1edf5-193">將 */hub* 路由對應至 `ChatHub` 中樞。</span><span class="sxs-lookup"><span data-stu-id="1edf5-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="1edf5-194">在 `Startup.Configure` 方法的結尾，新增下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="1edf5-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="1edf5-195">在專案根目錄中建立名為 *Hubs* 的新目錄。</span><span class="sxs-lookup"><span data-stu-id="1edf5-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="1edf5-196">其目的是要儲存下一個步驟所建立的 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="1edf5-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="1edf5-197">使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：</span><span class="sxs-lookup"><span data-stu-id="1edf5-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="1edf5-198">在 *Startup.cs* 檔案的頂端新增下列程式碼，以解析 `ChatHub` 參考：</span><span class="sxs-lookup"><span data-stu-id="1edf5-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="1edf5-199">啟用用戶端與伺服器的通訊</span><span class="sxs-lookup"><span data-stu-id="1edf5-199">Enable client and server communication</span></span>

<span data-ttu-id="1edf5-200">應用程式目前會顯示一個簡單的表單來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="1edf5-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="1edf5-201">當您嘗試這樣做時，不會執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="1edf5-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="1edf5-202">伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。</span><span class="sxs-lookup"><span data-stu-id="1edf5-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="1edf5-203">在專案的根目錄下執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1edf5-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="1edf5-204">上述命令會安裝 [SignalR TypeScript 用戶端](https://www.npmjs.com/package/@aspnet/signalr)，這可讓用戶端將訊息傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="1edf5-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="1edf5-205">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1edf5-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="1edf5-206">上述程式碼支援從伺服器接收訊息。</span><span class="sxs-lookup"><span data-stu-id="1edf5-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="1edf5-207">`HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。</span><span class="sxs-lookup"><span data-stu-id="1edf5-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="1edf5-208">`withUrl` 函式則會設定中樞 URL。</span><span class="sxs-lookup"><span data-stu-id="1edf5-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="1edf5-209">SignalR 可讓用戶端與伺服器之間進行訊息交換。</span><span class="sxs-lookup"><span data-stu-id="1edf5-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="1edf5-210">每個訊息都有特定的名稱。</span><span class="sxs-lookup"><span data-stu-id="1edf5-210">Each message has a specific name.</span></span> <span data-ttu-id="1edf5-211">例如，您可以包含名稱為 `messageReceived` 的訊息，用來執行負責在訊息區域中顯示新訊息的邏輯。</span><span class="sxs-lookup"><span data-stu-id="1edf5-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="1edf5-212">接聽特定的訊息可透過 `on` 函式完成。</span><span class="sxs-lookup"><span data-stu-id="1edf5-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="1edf5-213">您可以接聽任意數目的訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="1edf5-213">You can listen to any number of message names.</span></span> <span data-ttu-id="1edf5-214">也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。</span><span class="sxs-lookup"><span data-stu-id="1edf5-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="1edf5-215">用戶端收到訊息之後，就會在其 `innerHTML` 屬性中使用作者的名稱和訊息內容建立新的 `div` 元素。</span><span class="sxs-lookup"><span data-stu-id="1edf5-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="1edf5-216">它會新增至顯示訊息的主要 `div` 元素。</span><span class="sxs-lookup"><span data-stu-id="1edf5-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="1edf5-217">現在用戶端可以接收訊息，請設定它來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="1edf5-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="1edf5-218">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="1edf5-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="1edf5-219">透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。</span><span class="sxs-lookup"><span data-stu-id="1edf5-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="1edf5-220">方法的第一個參數是訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="1edf5-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="1edf5-221">訊息資料則佔用其他參數。</span><span class="sxs-lookup"><span data-stu-id="1edf5-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="1edf5-222">在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="1edf5-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="1edf5-223">訊息是由使用者名稱和文字方塊中的使用者輸入所組成。</span><span class="sxs-lookup"><span data-stu-id="1edf5-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="1edf5-224">如果傳送可正常運作，則會清除文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="1edf5-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="1edf5-225">將醒目提示的方法新增至 `ChatHub` 類別：</span><span class="sxs-lookup"><span data-stu-id="1edf5-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="1edf5-226">上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="1edf5-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="1edf5-227">不需要讓泛型 `on` 方法接收所有訊息。</span><span class="sxs-lookup"><span data-stu-id="1edf5-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="1edf5-228">以訊息名稱命名方法就已足夠。</span><span class="sxs-lookup"><span data-stu-id="1edf5-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="1edf5-229">在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="1edf5-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="1edf5-230">C# `NewMessage` 方法預期有用戶端所傳送的資料。</span><span class="sxs-lookup"><span data-stu-id="1edf5-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="1edf5-231">將會在 [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) 上呼叫 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 方法。</span><span class="sxs-lookup"><span data-stu-id="1edf5-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="1edf5-232">接收的訊息就會傳送到連線至中樞的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="1edf5-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="1edf5-233">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="1edf5-233">Test the app</span></span>

<span data-ttu-id="1edf5-234">使用下列步驟確認應用程式可正常運作。</span><span class="sxs-lookup"><span data-stu-id="1edf5-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1edf5-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1edf5-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1edf5-236">在 *release* 模式下執行 Webpack。</span><span class="sxs-lookup"><span data-stu-id="1edf5-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="1edf5-237">使用 [套件管理員主控台] 視窗，在專案根目錄中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="1edf5-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="1edf5-238">如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。</span><span class="sxs-lookup"><span data-stu-id="1edf5-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="1edf5-239">選取 [偵錯] > [啟動但不偵錯]，啟動瀏覽器中的應用程式，但不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="1edf5-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="1edf5-240">隨即會在 `http://localhost:<port_number>` 處提供 *wwwroot/index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1edf5-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="1edf5-241">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="1edf5-241">Open another browser instance (any browser).</span></span> <span data-ttu-id="1edf5-242">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="1edf5-242">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="1edf5-243">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1edf5-243">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="1edf5-244">唯一使用者名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="1edf5-244">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1edf5-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1edf5-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1edf5-246">在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：</span><span class="sxs-lookup"><span data-stu-id="1edf5-246">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="1edf5-247">在專案根目錄中執行下列命令，藉以建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="1edf5-247">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="1edf5-248">Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。</span><span class="sxs-lookup"><span data-stu-id="1edf5-248">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="1edf5-249">開啟瀏覽器並前往 `http://localhost:<port_number>`。</span><span class="sxs-lookup"><span data-stu-id="1edf5-249">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="1edf5-250">隨即會提供 *wwwroot/index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1edf5-250">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="1edf5-251">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="1edf5-251">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="1edf5-252">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="1edf5-252">Open another browser instance (any browser).</span></span> <span data-ttu-id="1edf5-253">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="1edf5-253">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="1edf5-254">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1edf5-254">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="1edf5-255">唯一使用者名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="1edf5-255">The unique user name and message are displayed on both pages instantly.</span></span>

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="1edf5-257">其他資源</span><span class="sxs-lookup"><span data-stu-id="1edf5-257">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
