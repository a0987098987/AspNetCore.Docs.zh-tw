---
title: 搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR
author: ssougnez
description: 在本教學課程中，您會將 Webpack 設定為配套，並建立用戶端以 TypeScript 撰寫的 ASP.NET Core SignalR web 應用程式。
ms.author: bradyg
ms.custom: mvc
ms.date: 02/10/2020
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: f8bbd9ed2e9c792197eb29be459f7e5ee499bfd1
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171992"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="34e47-103">搭配 TypeScript 和 Webpack 使用 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="34e47-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="34e47-104">作者：[Sébastien Sougnez](https://twitter.com/ssougnez) 和 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="34e47-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="34e47-105">[Webpack](https://webpack.js.org/) 可讓開發人員組合並建置 Web 應用程式的用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="34e47-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="34e47-106">本教學課程示範如何在其以 [TypeScript](https://www.typescriptlang.org/) 撰寫的 ASP.NET Core SignalR Web 應用程式中使用 Webpack。</span><span class="sxs-lookup"><span data-stu-id="34e47-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="34e47-107">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="34e47-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="34e47-108">Scaffold 入門 ASP.NET Core SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="34e47-109">設定 SignalR TypeScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="34e47-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="34e47-110">使用 Webpack 設定組建管線</span><span class="sxs-lookup"><span data-stu-id="34e47-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="34e47-111">設定 SignalR 伺服器</span><span class="sxs-lookup"><span data-stu-id="34e47-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="34e47-112">啟用用戶端與伺服器之間的通訊</span><span class="sxs-lookup"><span data-stu-id="34e47-112">Enable communication between client and server</span></span>

<span data-ttu-id="34e47-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="34e47-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="34e47-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="34e47-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e47-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e47-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="34e47-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)，其中包含 **ASP.NET 和 Web 部署**工作負載</span><span class="sxs-lookup"><span data-stu-id="34e47-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="34e47-117">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="34e47-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="34e47-118">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="34e47-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e47-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="34e47-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="34e47-121">.NET Core SDK 3.0 或更新版本</span><span class="sxs-lookup"><span data-stu-id="34e47-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="34e47-122">適用於 Visual Studio Code 1.17.1 版或更新版本的 C#</span><span class="sxs-lookup"><span data-stu-id="34e47-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="34e47-123">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="34e47-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="34e47-124">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e47-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e47-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e47-126">設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。</span><span class="sxs-lookup"><span data-stu-id="34e47-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="34e47-127">根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。</span><span class="sxs-lookup"><span data-stu-id="34e47-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="34e47-128">請遵循 Visual Studio 中的下列指示：</span><span class="sxs-lookup"><span data-stu-id="34e47-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="34e47-129">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="34e47-129">Launch Visual Studio.</span></span> <span data-ttu-id="34e47-130">在 [開始] 視窗中，選取 [**不需要程式碼即可繼續**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-130">At the start window, select **Continue without code**.</span></span>
1. <span data-ttu-id="34e47-131">流覽至 [**工具**] >**選項**> [**專案和方案**] > **Web 套件管理**> [**外部 web 工具**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-131">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="34e47-132">從清單中選取 *$(PATH)* 項目。</span><span class="sxs-lookup"><span data-stu-id="34e47-132">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="34e47-133">按一下向上箭號，將專案移至清單中的第二個位置，然後選取 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="34e47-133">Click the up arrow to move the entry to the second position in the list, and select **OK**.</span></span>

    ![Visual Studio 設定](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="34e47-135">Visual Studio 設定已完成。</span><span class="sxs-lookup"><span data-stu-id="34e47-135">Visual Studio configuration is complete.</span></span>

1. <span data-ttu-id="34e47-136">使用 [檔案] > [新增] > [專案] 功能表選項，然後選擇 [ASP.NET Core Web 應用程式] 範本。</span><span class="sxs-lookup"><span data-stu-id="34e47-136">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="34e47-137">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="34e47-137">Select **Next**.</span></span>
1. <span data-ttu-id="34e47-138">將專案命名為*命名為 signalrwebpack*，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-138">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="34e47-139">從 [目標 framework] 下拉式選單中選取 [ *.Net Core* ]，然後從 [framework 選取器] 下拉式選單中選取 [ *ASP.NET Core 3.1* ]。</span><span class="sxs-lookup"><span data-stu-id="34e47-139">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.1* from the framework selector drop-down.</span></span> <span data-ttu-id="34e47-140">選取 [**空白**] 範本，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-140">Select the **Empty** template, and select **Create**.</span></span>

<span data-ttu-id="34e47-141">將 `Microsoft.TypeScript.MSBuild` 封裝新增至專案：</span><span class="sxs-lookup"><span data-stu-id="34e47-141">Add the `Microsoft.TypeScript.MSBuild` package to the project:</span></span>

1. <span data-ttu-id="34e47-142">在**方案總管**（右窗格）中，以滑鼠右鍵按一下專案節點，然後選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-142">In **Solution Explorer** (right pane), right-click the project node and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="34e47-143">在 [**流覽**] 索引標籤中，搜尋 `Microsoft.TypeScript.MSBuild`，然後按一下右側的 [**安裝**] 來安裝套件。</span><span class="sxs-lookup"><span data-stu-id="34e47-143">In the **Browse** tab, search for `Microsoft.TypeScript.MSBuild`, and then click **Install** on the right to install the package.</span></span>

<span data-ttu-id="34e47-144">Visual Studio 會在**方案總管**的 [相依性 **]** 節點底下新增 NuGet 套件，並在專案中啟用 TypeScript 編譯。</span><span class="sxs-lookup"><span data-stu-id="34e47-144">Visual Studio adds the NuGet package under the **Dependencies** node in **Solution Explorer**, enabling TypeScript compilation in the project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e47-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-145">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="34e47-146">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-146">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
code -r SignalRWebPack
```

* <span data-ttu-id="34e47-147">`dotnet new` 命令會在*命名為 signalrwebpack*目錄中建立空白的 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e47-147">The `dotnet new` command creates an empty ASP.NET Core web app in a *SignalRWebPack* directory.</span></span>
* <span data-ttu-id="34e47-148">`code` 命令會開啟目前 Visual Studio Code 實例中的*命名為 signalrwebpack*資料夾。</span><span class="sxs-lookup"><span data-stu-id="34e47-148">The `code` command opens the *SignalRWebPack* folder in the current instance of Visual Studio Code.</span></span>

<span data-ttu-id="34e47-149">在**整合式終端**機中執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-149">Run the following .NET Core CLI command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add package Microsoft.TypeScript.MSBuild
```

<span data-ttu-id="34e47-150">上述命令會加入（Microsoft TypeScript） [https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/] 封裝，並在專案中啟用 TypeScript 編譯。</span><span class="sxs-lookup"><span data-stu-id="34e47-150">The preceding command adds the (Microsoft.TypeScript.MSBuild)[https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/] package, enabling TypeScript compilation in the project.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="34e47-151">設定 Webpack 和 TypeScript</span><span class="sxs-lookup"><span data-stu-id="34e47-151">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="34e47-152">下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。</span><span class="sxs-lookup"><span data-stu-id="34e47-152">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="34e47-153">在專案根目錄中執行下列命令，以建立*封裝 json*檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-153">Run the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="34e47-154">將反白顯示的屬性新增至*封裝 json*檔案，並儲存檔案變更：</span><span class="sxs-lookup"><span data-stu-id="34e47-154">Add the highlighted property to the *package.json* file and save the file changes:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="34e47-155">將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。</span><span class="sxs-lookup"><span data-stu-id="34e47-155">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="34e47-156">安裝必要的 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="34e47-156">Install the required npm packages.</span></span> <span data-ttu-id="34e47-157">從專案根目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-157">Run the following command from the project root:</span></span>

    ```console
    npm i -D -E clean-webpack-plugin@3.0.0 css-loader@3.4.2 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.9.0 ts-loader@6.2.1 typescript@3.7.5 webpack@4.41.5 webpack-cli@3.3.10
    ```

    <span data-ttu-id="34e47-158">要注意的一些命令詳細資料：</span><span class="sxs-lookup"><span data-stu-id="34e47-158">Some command details to note:</span></span>

    * <span data-ttu-id="34e47-159">版本號碼接在每一個套件名稱的 `@` 符號之後。</span><span class="sxs-lookup"><span data-stu-id="34e47-159">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="34e47-160">npm 會安裝這些特定的套件版本。</span><span class="sxs-lookup"><span data-stu-id="34e47-160">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="34e47-161">`-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。</span><span class="sxs-lookup"><span data-stu-id="34e47-161">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="34e47-162">例如，會使用 `"webpack": "4.41.5"`，而不是 `"webpack": "^4.41.5"`。</span><span class="sxs-lookup"><span data-stu-id="34e47-162">For example, `"webpack": "4.41.5"` is used instead of `"webpack": "^4.41.5"`.</span></span> <span data-ttu-id="34e47-163">此選項可防止意外升級至較新的套件版本。</span><span class="sxs-lookup"><span data-stu-id="34e47-163">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="34e47-164">如需詳細資訊，請參閱[npm-安裝](https://docs.npmjs.com/cli/install)檔。</span><span class="sxs-lookup"><span data-stu-id="34e47-164">See the [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="34e47-165">將*package. json*檔案的 `scripts` 屬性取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="34e47-165">Replace the `scripts` property of the *package.json* file with the following code:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="34e47-166">指令碼的一些說明：</span><span class="sxs-lookup"><span data-stu-id="34e47-166">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="34e47-167">`build`：在開發模式下組合用戶端資源，並監看檔案變更。</span><span class="sxs-lookup"><span data-stu-id="34e47-167">`build`: Bundles the client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="34e47-168">檔案監看員會導致套件組合在每次專案檔變更時重新產生。</span><span class="sxs-lookup"><span data-stu-id="34e47-168">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="34e47-169">`mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。</span><span class="sxs-lookup"><span data-stu-id="34e47-169">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="34e47-170">`build` 僅用於開發。</span><span class="sxs-lookup"><span data-stu-id="34e47-170">Only use `build` in development.</span></span>
    * <span data-ttu-id="34e47-171">`release`：在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="34e47-171">`release`: Bundles the client-side resources in production mode.</span></span>
    * <span data-ttu-id="34e47-172">`publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="34e47-172">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="34e47-173">它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e47-173">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="34e47-174">使用下列程式碼，在專案根目錄中建立名為*webpack*的檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-174">Create a file named *webpack.config.js*, in the project root, with the following code:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="34e47-175">上述檔案會設定 Webpack 編譯。</span><span class="sxs-lookup"><span data-stu-id="34e47-175">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="34e47-176">要注意的一些組態詳細資料：</span><span class="sxs-lookup"><span data-stu-id="34e47-176">Some configuration details to note:</span></span>

    * <span data-ttu-id="34e47-177">`output` 屬性會覆寫 *dist* 的預設值。</span><span class="sxs-lookup"><span data-stu-id="34e47-177">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="34e47-178">因而在 *wwwroot* 目錄中發出套件組合。</span><span class="sxs-lookup"><span data-stu-id="34e47-178">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="34e47-179">`resolve.extensions` 陣列包含 *.js* 來匯入 SignalR 用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="34e47-179">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="34e47-180">在專案根目錄中建立新的*src*目錄，以儲存專案的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="34e47-180">Create a new *src* directory in the project root to store the project's client-side assets.</span></span>

1. <span data-ttu-id="34e47-181">使用下列標記建立*src/index.html* 。</span><span class="sxs-lookup"><span data-stu-id="34e47-181">Create *src/index.html* with the following markup.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="34e47-182">上述的 HTML 會定義首頁的樣板標記。</span><span class="sxs-lookup"><span data-stu-id="34e47-182">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="34e47-183">建立新的 *src/css* 目錄。</span><span class="sxs-lookup"><span data-stu-id="34e47-183">Create a new *src/css* directory.</span></span> <span data-ttu-id="34e47-184">其目的是要儲存專案的 *.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-184">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="34e47-185">使用下列 CSS 建立*src/css/main. css* ：</span><span class="sxs-lookup"><span data-stu-id="34e47-185">Create *src/css/main.css* with the following CSS:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="34e47-186">上述的 *main.css* 檔案會設定應用程式的樣式。</span><span class="sxs-lookup"><span data-stu-id="34e47-186">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="34e47-187">使用下列 JSON 建立*src/tsconfig* ：</span><span class="sxs-lookup"><span data-stu-id="34e47-187">Create *src/tsconfig.json* with the following JSON:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="34e47-188">上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="34e47-188">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="34e47-189">使用下列程式碼建立*src/index. ts* ：</span><span class="sxs-lookup"><span data-stu-id="34e47-189">Create *src/index.ts* with the following code:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="34e47-190">上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="34e47-190">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="34e47-191">`keyup`：當使用者在 [`tbMessage`] 文字方塊中輸入時，就會引發這個事件。</span><span class="sxs-lookup"><span data-stu-id="34e47-191">`keyup`: This event fires when the user types in the `tbMessage`textbox.</span></span> <span data-ttu-id="34e47-192">當使用者按下 `send`Enter**鍵時，即會呼叫** 函式。</span><span class="sxs-lookup"><span data-stu-id="34e47-192">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="34e47-193">`click`：當使用者按一下 [傳送] 按鈕時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="34e47-193">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="34e47-194">系統會呼叫 `send` 函式。</span><span class="sxs-lookup"><span data-stu-id="34e47-194">The `send` function is called.</span></span>

## <a name="configure-the-app"></a><span data-ttu-id="34e47-195">設定應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-195">Configure the app</span></span>

1. <span data-ttu-id="34e47-196">在 `Startup.Configure`中，將呼叫新增至[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。</span><span class="sxs-lookup"><span data-stu-id="34e47-196">In `Startup.Configure`, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=9-10)]

   <span data-ttu-id="34e47-197">上述程式碼可讓伺服器找出並提供*索引 .html*檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-197">The preceding code allows the server to locate and serve the *index.html* file.</span></span>  <span data-ttu-id="34e47-198">該檔案會提供給使用者是否輸入其完整 URL 或 web 應用程式的根 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-198">The file is served whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="34e47-199">在 `Startup.Configure`的結尾，將 */hub*路由對應至 `ChatHub` 中樞。</span><span class="sxs-lookup"><span data-stu-id="34e47-199">At the end of `Startup.Configure`, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="34e47-200">取代顯示 Hello World 的程式碼 *！*</span><span class="sxs-lookup"><span data-stu-id="34e47-200">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="34e47-201">成為下列這行：</span><span class="sxs-lookup"><span data-stu-id="34e47-201">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="34e47-202">在 `Startup.ConfigureServices`中，呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)。</span><span class="sxs-lookup"><span data-stu-id="34e47-202">In `Startup.ConfigureServices`, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="34e47-203">在專案根*命名為 signalrwebpack/* 中建立名為*hub*的新目錄，以儲存 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="34e47-203">Create a new directory named *Hubs* in the project root *SignalRWebPack/* to store the SignalR hub.</span></span>

1. <span data-ttu-id="34e47-204">使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：</span><span class="sxs-lookup"><span data-stu-id="34e47-204">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="34e47-205">在*Startup.cs*檔案的頂端新增下列 `using` 語句，以解析 `ChatHub` 參考：</span><span class="sxs-lookup"><span data-stu-id="34e47-205">Add the following `using` statement at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="34e47-206">啟用用戶端與伺服器的通訊</span><span class="sxs-lookup"><span data-stu-id="34e47-206">Enable client and server communication</span></span>

<span data-ttu-id="34e47-207">應用程式目前顯示傳送訊息的基本表單，但尚未正常運作。</span><span class="sxs-lookup"><span data-stu-id="34e47-207">The app currently displays a basic form to send messages, but is not yet functional.</span></span> <span data-ttu-id="34e47-208">伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。</span><span class="sxs-lookup"><span data-stu-id="34e47-208">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="34e47-209">在專案根目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-209">Run the following command at the project root:</span></span>

    ```console
    npm i @microsoft/signalr @types/node
    ```

    <span data-ttu-id="34e47-210">上述命令會安裝：</span><span class="sxs-lookup"><span data-stu-id="34e47-210">The preceding command installs:</span></span>

     * <span data-ttu-id="34e47-211">[SignalR TypeScript 用戶端](https://www.npmjs.com/package/@microsoft/signalr)，可讓用戶端將訊息傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="34e47-211">The [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>
     * <span data-ttu-id="34e47-212">適用于 node.js 的 TypeScript 類型定義，可啟用 node.js 類型的編譯時間檢查。</span><span class="sxs-lookup"><span data-stu-id="34e47-212">The TypeScript type definitions for Node.js, which enables compile-time checking of Node.js types.</span></span>

1. <span data-ttu-id="34e47-213">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-213">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="34e47-214">上述程式碼支援從伺服器接收訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-214">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="34e47-215">`HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。</span><span class="sxs-lookup"><span data-stu-id="34e47-215">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="34e47-216">`withUrl` 函式則會設定中樞 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-216">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="34e47-217">SignalR 可讓用戶端與伺服器之間進行訊息交換。</span><span class="sxs-lookup"><span data-stu-id="34e47-217">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="34e47-218">每個訊息都有特定的名稱。</span><span class="sxs-lookup"><span data-stu-id="34e47-218">Each message has a specific name.</span></span> <span data-ttu-id="34e47-219">例如，名稱為 `messageReceived` 的訊息可以執行負責在訊息區域中顯示新訊息的邏輯。</span><span class="sxs-lookup"><span data-stu-id="34e47-219">For example, messages with the name `messageReceived` can run the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="34e47-220">接聽特定的訊息可透過 `on` 函式完成。</span><span class="sxs-lookup"><span data-stu-id="34e47-220">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="34e47-221">可以接聽任意數目的訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="34e47-221">Any number of message names can be listened to.</span></span> <span data-ttu-id="34e47-222">也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。</span><span class="sxs-lookup"><span data-stu-id="34e47-222">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="34e47-223">用戶端收到訊息之後，就會在其 `div` 屬性中使用作者的名稱和訊息內容建立新的 `innerHTML` 元素。</span><span class="sxs-lookup"><span data-stu-id="34e47-223">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="34e47-224">它會新增至顯示訊息的主要 `div` 元素。</span><span class="sxs-lookup"><span data-stu-id="34e47-224">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="34e47-225">現在用戶端可以接收訊息，請設定它來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-225">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="34e47-226">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-226">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="34e47-227">透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。</span><span class="sxs-lookup"><span data-stu-id="34e47-227">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="34e47-228">方法的第一個參數是訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="34e47-228">The method's first parameter is the message name.</span></span> <span data-ttu-id="34e47-229">訊息資料則佔用其他參數。</span><span class="sxs-lookup"><span data-stu-id="34e47-229">The message data inhabits the other parameters.</span></span> <span data-ttu-id="34e47-230">在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="34e47-230">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="34e47-231">訊息是由使用者名稱和文字方塊中的使用者輸入所組成。</span><span class="sxs-lookup"><span data-stu-id="34e47-231">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="34e47-232">如果傳送可正常運作，則會清除文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="34e47-232">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="34e47-233">將 `NewMessage` 方法新增至 `ChatHub` 類別：</span><span class="sxs-lookup"><span data-stu-id="34e47-233">Add the `NewMessage` method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="34e47-234">上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-234">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="34e47-235">不需要讓泛型 `on` 方法接收所有訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-235">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="34e47-236">以訊息名稱命名方法就已足夠。</span><span class="sxs-lookup"><span data-stu-id="34e47-236">A method named after the message name suffices.</span></span>

    <span data-ttu-id="34e47-237">在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-237">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="34e47-238">C# `NewMessage` 方法預期有用戶端所傳送的資料。</span><span class="sxs-lookup"><span data-stu-id="34e47-238">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="34e47-239">在[用戶端](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all)上進行[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)的呼叫。</span><span class="sxs-lookup"><span data-stu-id="34e47-239">A call is made to [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="34e47-240">接收的訊息就會傳送到連線至中樞的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="34e47-240">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="34e47-241">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-241">Test the app</span></span>

<span data-ttu-id="34e47-242">使用下列步驟確認應用程式可正常運作。</span><span class="sxs-lookup"><span data-stu-id="34e47-242">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e47-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e47-243">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="34e47-244">在 *release* 模式下執行 Webpack。</span><span class="sxs-lookup"><span data-stu-id="34e47-244">Run Webpack in *release* mode.</span></span> <span data-ttu-id="34e47-245">使用 [**套件管理員主控台**] 視窗，在專案根目錄中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="34e47-245">Using the **Package Manager Console** window, run the following command in the project root.</span></span> <span data-ttu-id="34e47-246">如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。</span><span class="sxs-lookup"><span data-stu-id="34e47-246">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="34e47-247">選取 [偵錯] > [啟動但不偵錯]，啟動瀏覽器中的應用程式，但不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="34e47-247">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="34e47-248">隨即會在 *處提供*wwwroot/index.html`http://localhost:<port_number>` 檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-248">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="34e47-249">如果您遇到編譯錯誤，請嘗試關閉並重新開啟方案。</span><span class="sxs-lookup"><span data-stu-id="34e47-249">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="34e47-250">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="34e47-250">Open another browser instance (any browser).</span></span> <span data-ttu-id="34e47-251">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-251">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="34e47-252">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e47-252">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="34e47-253">唯一使用者名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="34e47-253">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e47-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="34e47-255">在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：</span><span class="sxs-lookup"><span data-stu-id="34e47-255">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="34e47-256">在專案根目錄中執行下列命令，藉以建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="34e47-256">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="34e47-257">Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。</span><span class="sxs-lookup"><span data-stu-id="34e47-257">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="34e47-258">開啟瀏覽器並前往 `http://localhost:<port_number>`。</span><span class="sxs-lookup"><span data-stu-id="34e47-258">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="34e47-259">隨即會提供 *wwwroot/index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-259">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="34e47-260">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-260">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="34e47-261">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="34e47-261">Open another browser instance (any browser).</span></span> <span data-ttu-id="34e47-262">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-262">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="34e47-263">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e47-263">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="34e47-264">唯一使用者名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="34e47-264">The unique user name and message are displayed on both pages instantly.</span></span>

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="34e47-266">必要條件</span><span class="sxs-lookup"><span data-stu-id="34e47-266">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e47-267">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e47-267">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="34e47-268">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)，其中包含 **ASP.NET 和 Web 部署**工作負載</span><span class="sxs-lookup"><span data-stu-id="34e47-268">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="34e47-269">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="34e47-269">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="34e47-270">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="34e47-270">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e47-271">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-271">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="34e47-272">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-272">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="34e47-273">.NET Core SDK 2.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="34e47-273">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="34e47-274">適用於 Visual Studio Code 1.17.1 版或更新版本的 C#</span><span class="sxs-lookup"><span data-stu-id="34e47-274">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="34e47-275">具有 [npm](https://nodejs.org/) 的 [Node.js](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="34e47-275">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="34e47-276">建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-276">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e47-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e47-277">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="34e47-278">設定 Visual Studio 以在 *PATH* 環境變數中尋找 npm。</span><span class="sxs-lookup"><span data-stu-id="34e47-278">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="34e47-279">根據預設，Visual Studio 會使用在其安裝目錄中找到的 npm 版本。</span><span class="sxs-lookup"><span data-stu-id="34e47-279">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="34e47-280">請遵循 Visual Studio 中的下列指示：</span><span class="sxs-lookup"><span data-stu-id="34e47-280">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="34e47-281">流覽至 [**工具**] >**選項**> [**專案和方案**] > **Web 套件管理**> [**外部 web 工具**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-281">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="34e47-282">從清單中選取 *$(PATH)* 項目。</span><span class="sxs-lookup"><span data-stu-id="34e47-282">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="34e47-283">按一下向上箭號，將此項目移至清單中的第二個位置。</span><span class="sxs-lookup"><span data-stu-id="34e47-283">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Visual Studio 設定](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="34e47-285">Visual Studio 組態已完成。</span><span class="sxs-lookup"><span data-stu-id="34e47-285">Visual Studio configuration is completed.</span></span> <span data-ttu-id="34e47-286">現在即可開始建立專案。</span><span class="sxs-lookup"><span data-stu-id="34e47-286">It's time to create the project.</span></span>

1. <span data-ttu-id="34e47-287">使用 [檔案] > [**新增**>**專案**] 功能表**選項，然後**選擇 [ **ASP.NET Core Web 應用程式**] 範本。</span><span class="sxs-lookup"><span data-stu-id="34e47-287">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="34e47-288">將專案命名為*命名為 signalrwebpack*，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-288">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="34e47-289">從目標 Framework 下拉式清單中選取 [.NET Core]，然後從 Framework 選取器下拉式清單中選取 [ASP.NET Core 2.2]。</span><span class="sxs-lookup"><span data-stu-id="34e47-289">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="34e47-290">選取 [**空白**] 範本，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="34e47-290">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e47-291">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-291">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="34e47-292">在 [整合式終端機] 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-292">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="34e47-293">隨即會在 *SignalRWebPack* 目錄中建立以 .NET Core 為目標的空白 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e47-293">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="34e47-294">設定 Webpack 和 TypeScript</span><span class="sxs-lookup"><span data-stu-id="34e47-294">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="34e47-295">下列步驟可設定 TypeScript 至 JavaScript 的轉換和用戶端資源的組合。</span><span class="sxs-lookup"><span data-stu-id="34e47-295">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="34e47-296">在專案根目錄中執行下列命令，以建立*封裝 json*檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-296">Run the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="34e47-297">將醒目提示的屬性新增至 *package.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-297">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="34e47-298">將 `private` 屬性設定為 `true` 可避免在下一個步驟中出現套件安裝警告。</span><span class="sxs-lookup"><span data-stu-id="34e47-298">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="34e47-299">安裝必要的 npm 套件。</span><span class="sxs-lookup"><span data-stu-id="34e47-299">Install the required npm packages.</span></span> <span data-ttu-id="34e47-300">從專案根目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-300">Run the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="34e47-301">要注意的一些命令詳細資料：</span><span class="sxs-lookup"><span data-stu-id="34e47-301">Some command details to note:</span></span>

    * <span data-ttu-id="34e47-302">版本號碼接在每一個套件名稱的 `@` 符號之後。</span><span class="sxs-lookup"><span data-stu-id="34e47-302">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="34e47-303">npm 會安裝這些特定的套件版本。</span><span class="sxs-lookup"><span data-stu-id="34e47-303">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="34e47-304">`-E` 選項會停用 npm 將[語意版本控制](https://semver.org/)範圍運算子寫入 *package.json* 的預設行為。</span><span class="sxs-lookup"><span data-stu-id="34e47-304">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="34e47-305">例如，會使用 `"webpack": "4.29.3"`，而不是 `"webpack": "^4.29.3"`。</span><span class="sxs-lookup"><span data-stu-id="34e47-305">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="34e47-306">此選項可防止意外升級至較新的套件版本。</span><span class="sxs-lookup"><span data-stu-id="34e47-306">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="34e47-307">如需詳細資訊，請參閱[npm-安裝](https://docs.npmjs.com/cli/install)檔。</span><span class="sxs-lookup"><span data-stu-id="34e47-307">See the [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="34e47-308">將*package. json*檔案的 `scripts` 屬性取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="34e47-308">Replace the `scripts` property of the *package.json* file with the following code:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="34e47-309">指令碼的一些說明：</span><span class="sxs-lookup"><span data-stu-id="34e47-309">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="34e47-310">`build`：在開發模式下組合用戶端資源，並監看檔案變更。</span><span class="sxs-lookup"><span data-stu-id="34e47-310">`build`: Bundles the client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="34e47-311">檔案監看員會導致套件組合在每次專案檔變更時重新產生。</span><span class="sxs-lookup"><span data-stu-id="34e47-311">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="34e47-312">`mode` 選項會停用生產環境最佳化，例如樹狀結構搖晃和縮製。</span><span class="sxs-lookup"><span data-stu-id="34e47-312">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="34e47-313">`build` 僅用於開發。</span><span class="sxs-lookup"><span data-stu-id="34e47-313">Only use `build` in development.</span></span>
    * <span data-ttu-id="34e47-314">`release`：在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="34e47-314">`release`: Bundles the client-side resources in production mode.</span></span>
    * <span data-ttu-id="34e47-315">`publish`：執行 `release` 指令碼，以在生產模式下組合用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="34e47-315">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="34e47-316">它會呼叫 .NET Core CLI 的 [publish](/dotnet/core/tools/dotnet-publish) 命令來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="34e47-316">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="34e47-317">使用下列程式碼，在專案根目錄中建立名為*webpack*的檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-317">Create a file named *webpack.config.js* in the project root, with the following code:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="34e47-318">上述檔案會設定 Webpack 編譯。</span><span class="sxs-lookup"><span data-stu-id="34e47-318">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="34e47-319">要注意的一些組態詳細資料：</span><span class="sxs-lookup"><span data-stu-id="34e47-319">Some configuration details to note:</span></span>

    * <span data-ttu-id="34e47-320">`output` 屬性會覆寫 *dist* 的預設值。</span><span class="sxs-lookup"><span data-stu-id="34e47-320">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="34e47-321">因而在 *wwwroot* 目錄中發出套件組合。</span><span class="sxs-lookup"><span data-stu-id="34e47-321">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="34e47-322">`resolve.extensions` 陣列包含 *.js* 來匯入 SignalR 用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="34e47-322">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="34e47-323">在專案根目錄中建立新的*src*目錄，以儲存專案的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="34e47-323">Create a new *src* directory in the project root to store the project's client-side assets.</span></span>

1. <span data-ttu-id="34e47-324">使用下列標記建立*src/index.html* 。</span><span class="sxs-lookup"><span data-stu-id="34e47-324">Create *src/index.html* with the following markup.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="34e47-325">上述的 HTML 會定義首頁的樣板標記。</span><span class="sxs-lookup"><span data-stu-id="34e47-325">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="34e47-326">建立新的 *src/css* 目錄。</span><span class="sxs-lookup"><span data-stu-id="34e47-326">Create a new *src/css* directory.</span></span> <span data-ttu-id="34e47-327">其目的是要儲存專案的 *.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-327">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="34e47-328">使用下列標記建立*src/css/main. css* ：</span><span class="sxs-lookup"><span data-stu-id="34e47-328">Create *src/css/main.css* with the following markup:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="34e47-329">上述的 *main.css* 檔案會設定應用程式的樣式。</span><span class="sxs-lookup"><span data-stu-id="34e47-329">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="34e47-330">使用下列 JSON 建立*src/tsconfig* ：</span><span class="sxs-lookup"><span data-stu-id="34e47-330">Create *src/tsconfig.json* with the following JSON:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="34e47-331">上述程式碼會設定 TypeScript 編譯器來產生 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 相容的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="34e47-331">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="34e47-332">使用下列程式碼建立*src/index. ts* ：</span><span class="sxs-lookup"><span data-stu-id="34e47-332">Create *src/index.ts* with the following code:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="34e47-333">上述的 TypeScript 會擷取 DOM 項目的參考，並將附加兩個事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="34e47-333">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="34e47-334">`keyup`：當使用者在 [`tbMessage`] 文字方塊中輸入時，就會引發這個事件。</span><span class="sxs-lookup"><span data-stu-id="34e47-334">`keyup`: This event fires when the user types in the `tbMessage` textbox.</span></span> <span data-ttu-id="34e47-335">當使用者按下 `send`Enter**鍵時，即會呼叫** 函式。</span><span class="sxs-lookup"><span data-stu-id="34e47-335">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="34e47-336">`click`：當使用者按一下 [傳送] 按鈕時，就會引發此事件。</span><span class="sxs-lookup"><span data-stu-id="34e47-336">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="34e47-337">系統會呼叫 `send` 函式。</span><span class="sxs-lookup"><span data-stu-id="34e47-337">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="34e47-338">設定 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-338">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="34e47-339">`Startup.Configure` 方法中提供的程式碼會顯示 *Hello World!* 。</span><span class="sxs-lookup"><span data-stu-id="34e47-339">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="34e47-340">請以 `app.Run`UseDefaultFiles[ 和 ](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)UseStaticFiles[ 的呼叫取代 ](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="34e47-340">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="34e47-341">上述程式碼可讓伺服器找出並提供 *index.html* 檔案，而不論使用者輸入的是其完整 URL 還是 Web 應用程式的根目錄 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-341">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="34e47-342">在 `Startup.ConfigureServices`中呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 。</span><span class="sxs-lookup"><span data-stu-id="34e47-342">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="34e47-343">它會將 SignalR 服務新增至專案。</span><span class="sxs-lookup"><span data-stu-id="34e47-343">It adds the SignalR services to the project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="34e47-344">將 */hub* 路由對應至 `ChatHub` 中樞。</span><span class="sxs-lookup"><span data-stu-id="34e47-344">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="34e47-345">在 `Startup.Configure`結尾新增下列幾行：</span><span class="sxs-lookup"><span data-stu-id="34e47-345">Add the following lines at the end of `Startup.Configure`:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="34e47-346">在專案根目錄中建立名為 *Hubs* 的新目錄。</span><span class="sxs-lookup"><span data-stu-id="34e47-346">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="34e47-347">其目的是要儲存下一個步驟所建立的 SignalR 中樞。</span><span class="sxs-lookup"><span data-stu-id="34e47-347">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="34e47-348">使用下列程式碼建立中樞 *Hubs/ChatHub.cs*：</span><span class="sxs-lookup"><span data-stu-id="34e47-348">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="34e47-349">在 *Startup.cs* 檔案的頂端新增下列程式碼，以解析 `ChatHub` 參考：</span><span class="sxs-lookup"><span data-stu-id="34e47-349">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="34e47-350">啟用用戶端與伺服器的通訊</span><span class="sxs-lookup"><span data-stu-id="34e47-350">Enable client and server communication</span></span>

<span data-ttu-id="34e47-351">應用程式目前會顯示一個簡單的表單來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-351">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="34e47-352">當您嘗試這樣做時，不會執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="34e47-352">Nothing happens when you try to do so.</span></span> <span data-ttu-id="34e47-353">伺服器正在接聽特定的路由，但對於已傳送的訊息不會進行任何處理。</span><span class="sxs-lookup"><span data-stu-id="34e47-353">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="34e47-354">在專案根目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="34e47-354">Run the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="34e47-355">上述命令會安裝 [SignalR TypeScript 用戶端](https://www.npmjs.com/package/@microsoft/signalr)，這可讓用戶端將訊息傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="34e47-355">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="34e47-356">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-356">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="34e47-357">上述程式碼支援從伺服器接收訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-357">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="34e47-358">`HubConnectionBuilder` 類別會建立用於設定伺服器連線的新產生器。</span><span class="sxs-lookup"><span data-stu-id="34e47-358">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="34e47-359">`withUrl` 函式則會設定中樞 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-359">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="34e47-360">SignalR 可讓用戶端與伺服器之間進行訊息交換。</span><span class="sxs-lookup"><span data-stu-id="34e47-360">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="34e47-361">每個訊息都有特定的名稱。</span><span class="sxs-lookup"><span data-stu-id="34e47-361">Each message has a specific name.</span></span> <span data-ttu-id="34e47-362">例如，名稱為 `messageReceived` 的訊息可以執行負責在訊息區域中顯示新訊息的邏輯。</span><span class="sxs-lookup"><span data-stu-id="34e47-362">For example, messages with the name `messageReceived` can run the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="34e47-363">接聽特定的訊息可透過 `on` 函式完成。</span><span class="sxs-lookup"><span data-stu-id="34e47-363">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="34e47-364">您可以接聽任意數目的訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="34e47-364">You can listen to any number of message names.</span></span> <span data-ttu-id="34e47-365">也可將參數傳遞給訊息，例如作者的名稱和已接收訊息的內容。</span><span class="sxs-lookup"><span data-stu-id="34e47-365">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="34e47-366">用戶端收到訊息之後，就會在其 `div` 屬性中使用作者的名稱和訊息內容建立新的 `innerHTML` 元素。</span><span class="sxs-lookup"><span data-stu-id="34e47-366">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="34e47-367">新的訊息會新增至顯示訊息的主要 `div` 專案。</span><span class="sxs-lookup"><span data-stu-id="34e47-367">The new message is added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="34e47-368">現在用戶端可以接收訊息，請設定它來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-368">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="34e47-369">將醒目提示的程式碼新增至 *src/index.ts* 檔案：</span><span class="sxs-lookup"><span data-stu-id="34e47-369">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="34e47-370">透過 Websocket 連線傳送訊息需要呼叫 `send` 方法。</span><span class="sxs-lookup"><span data-stu-id="34e47-370">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="34e47-371">方法的第一個參數是訊息名稱。</span><span class="sxs-lookup"><span data-stu-id="34e47-371">The method's first parameter is the message name.</span></span> <span data-ttu-id="34e47-372">訊息資料則佔用其他參數。</span><span class="sxs-lookup"><span data-stu-id="34e47-372">The message data inhabits the other parameters.</span></span> <span data-ttu-id="34e47-373">在此範例中，識別為 `newMessage` 的訊息會傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="34e47-373">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="34e47-374">訊息是由使用者名稱和文字方塊中的使用者輸入所組成。</span><span class="sxs-lookup"><span data-stu-id="34e47-374">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="34e47-375">如果傳送可正常運作，則會清除文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="34e47-375">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="34e47-376">將 `NewMessage` 方法新增至 `ChatHub` 類別：</span><span class="sxs-lookup"><span data-stu-id="34e47-376">Add the `NewMessage` method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="34e47-377">上述程式碼會在伺服器收到訊息之後，向所有連線的使用者廣播已接收的訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-377">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="34e47-378">不需要讓泛型 `on` 方法接收所有訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-378">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="34e47-379">以訊息名稱命名方法就已足夠。</span><span class="sxs-lookup"><span data-stu-id="34e47-379">A method named after the message name suffices.</span></span>

    <span data-ttu-id="34e47-380">在此範例中，TypeScript 用戶端會傳送一則識別為 `newMessage` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="34e47-380">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="34e47-381">C# `NewMessage` 方法預期有用戶端所傳送的資料。</span><span class="sxs-lookup"><span data-stu-id="34e47-381">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="34e47-382">在[用戶端](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all)上進行[SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)的呼叫。</span><span class="sxs-lookup"><span data-stu-id="34e47-382">A call is made to [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="34e47-383">接收的訊息就會傳送到連線至中樞的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="34e47-383">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="34e47-384">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="34e47-384">Test the app</span></span>

<span data-ttu-id="34e47-385">使用下列步驟確認應用程式可正常運作。</span><span class="sxs-lookup"><span data-stu-id="34e47-385">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="34e47-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34e47-386">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="34e47-387">在 *release* 模式下執行 Webpack。</span><span class="sxs-lookup"><span data-stu-id="34e47-387">Run Webpack in *release* mode.</span></span> <span data-ttu-id="34e47-388">使用 [**套件管理員主控台**] 視窗，在專案根目錄中執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="34e47-388">Using the **Package Manager Console** window, run the following command in the project root.</span></span> <span data-ttu-id="34e47-389">如果您不在專案根目錄中，請先輸入 `cd SignalRWebPack`，再輸入命令。</span><span class="sxs-lookup"><span data-stu-id="34e47-389">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="34e47-390">選取 [偵錯] > [啟動但不偵錯]，啟動瀏覽器中的應用程式，但不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="34e47-390">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="34e47-391">隨即會在 *處提供*wwwroot/index.html`http://localhost:<port_number>` 檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-391">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="34e47-392">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="34e47-392">Open another browser instance (any browser).</span></span> <span data-ttu-id="34e47-393">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-393">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="34e47-394">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e47-394">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="34e47-395">唯一使用者名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="34e47-395">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="34e47-396">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34e47-396">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="34e47-397">在專案根目錄中執行下列命令，藉以在 *release* 模式下執行 Webpack：</span><span class="sxs-lookup"><span data-stu-id="34e47-397">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="34e47-398">在專案根目錄中執行下列命令，藉以建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="34e47-398">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="34e47-399">Web 伺服器會啟動應用程式，並使其可在 localhost 上使用。</span><span class="sxs-lookup"><span data-stu-id="34e47-399">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="34e47-400">開啟瀏覽器並前往 `http://localhost:<port_number>`。</span><span class="sxs-lookup"><span data-stu-id="34e47-400">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="34e47-401">隨即會提供 *wwwroot/index.html* 檔案。</span><span class="sxs-lookup"><span data-stu-id="34e47-401">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="34e47-402">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-402">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="34e47-403">開啟另一個瀏覽器執行個體 (任何瀏覽器)。</span><span class="sxs-lookup"><span data-stu-id="34e47-403">Open another browser instance (any browser).</span></span> <span data-ttu-id="34e47-404">在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="34e47-404">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="34e47-405">選擇其中一個瀏覽器，在 [訊息] 文字方塊中鍵入某些內容，然後按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="34e47-405">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="34e47-406">唯一使用者名稱和訊息會立即顯示在兩個頁面上。</span><span class="sxs-lookup"><span data-stu-id="34e47-406">The unique user name and message are displayed on both pages instantly.</span></span>

---

![兩個瀏覽器視窗中顯示的訊息](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="34e47-408">其他資源</span><span class="sxs-lookup"><span data-stu-id="34e47-408">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
