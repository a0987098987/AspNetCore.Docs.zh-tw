---
title: 捆綁和整理 ASP.NET 核心中的靜態資產
author: scottaddie
description: 瞭解如何透過應用捆綁和最小化技術來優化ASP.NET核心 Web 應用程式中的靜態資源。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658267"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>捆綁和整理 ASP.NET 核心中的靜態資產

由[斯科特·阿迪](https://twitter.com/Scott_Addie)[和大衛·派恩](https://twitter.com/davidpine7)

本文介紹了應用捆綁和小化的好處,包括這些功能如何與ASP.NET核心 Web 應用一起使用。

## <a name="what-is-bundling-and-minification"></a>什麼是捆綁和小化

捆綁和分小是兩種不同的性能優化,您可以應用於 Web 應用。 捆綁和縮小相結合,通過減少伺服器請求的數量和減少請求的靜態資產的大小來提高性能。

捆綁和小化主要改善第一頁請求載入時間。 請求網頁後,瀏覽器將緩存靜態資產(JAVAScript、CSS 和圖像)。 因此,在同一網站上請求同一頁面或頁面時,捆綁和小化不會提高性能。 如果過期標頭未正確設置在資產上,並且不使用捆綁和小化,瀏覽器的新鮮感啟發式將標記幾天后資產過時。 此外,瀏覽器需要每個資產的驗證請求。 在這種情況下,捆綁和分明即使在第一頁請求后也能提高性能。

### <a name="bundling"></a>捆綁

統合可將多個檔案合併成單一檔案。 捆綁減少了呈現 Web 資產(如網頁)所需的伺服器請求數。 您可以為 CSS、JavaScript 等創建任意數量的單個捆綁包。檔越少,從瀏覽器到伺服器或提供應用程式的服務的 HTTP 請求更少。 這提高了第一頁的載入性能。

### <a name="minification"></a>縮製

敏化可在不更改功能的情況下從代碼中刪除不必要的字元。 結果是請求的資產(如 CSS、映射和 JAVAScript 檔)的大小顯著縮減。 最小化的常見副作用包括將變數名稱縮短為一個字元,並刪除註釋和不必要的空格。

請考慮以下 JavaScript 函數:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

最小化將功能減少到以下內容:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

除了刪除註解和不必要的空格外,以下參數和變數名稱也重命名如下:

原始 | 已重新命名
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>捆綁和小化的影響

下表概述了單獨載入資產和使用捆綁和最小化之間的區別:

動作 | 帶 B/M | 無 B/M | 變更
--- | :---: | :---: | :---:
檔案要求  | 7   | 18     | 157%
KB 已傳輸 | 156 | 264.68 | 70%
載入時間 (毫秒) | 885 | 2360   | 167%

瀏覽器在 HTTP 請求標頭方面相當詳細。 捆綁時發送的位元組總數指標顯著減少。 載入時間顯示顯著改進,但此範例在本機運行。 使用捆綁和計量與通過網路傳輸的資產進行捆綁和計量時,可實現更大的性能提升。

## <a name="choose-a-bundling-and-minification-strategy"></a>選擇捆綁與最小化原則

MVC 和 Razor Pages 專案範本提供了一個開箱即用的解決方案,用於捆綁和分明,包括 JSON 配置檔。 第三方工具(如[Grunt](xref:client-side/using-grunt)任務運行者)以稍微複雜一點完成相同的任務。 當您的開發工作流需要超越捆綁和縮編&mdash;(如鑲合和圖像優化)的處理時,第三方工具非常合適。 通過使用設計時捆綁和小化,在應用部署之前創建已創建已化的檔。 部署前捆綁和簡化可提供減少伺服器負載的優勢。 但是,請務必認識到,設計時捆綁和小化會增加構建複雜性,並且僅適用於靜態檔。

## <a name="configure-bundling-and-minification"></a>設定捆繫與最小化

::: moniker range="<= aspnetcore-2.0"

在ASP.NET Core 2.0 或更早版本中,MVC 和 Razor Pages 專案範本提供一個*bundleconfig.json*設定檔,用於定義每個捆綁套件的選項:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

在ASP.NET Core 2.1 或更高版本中,向MVC或Razor Pages專案根添加新的 JSON 檔,名為*bundleconfig.json。* 將以下 JSON 作為起點在該檔中:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

*bundleconfig.json*檔定義了每個捆綁包的選項。 在前面的範例中,為自定義 JavaScript *(wwwroot/js/site.js)* 和樣式表 (*wwwroot/css/site.css)* 檔定義了單個捆綁包配置。

設定選項包括：

* `outputFileName`:要輸出的捆綁檔的名稱。 可以包含*來自 bundleconfig.json*檔中的相對路徑。 **必填**
* `inputFiles`:要捆綁在一起的文件陣組。 這些是配置檔的相對路徑。 **可選**,\一個空值會導致輸出檔為空。 支援[globing](https://www.tldp.org/LDP/abs/html/globbingref.html)模式。
* `minify`:輸出類型的最小化選項。 **選擇 ,***預設`minify: { enabled: true }`-*
  * 配置選項每個輸出檔類型都可用。
    * [CSS 最小值器](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript 分明器](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML 最小值器](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`:指示是否將生成的檔添加到專案檔的標誌。 **選擇**,*預設 - 假*
* `sourceMap`:指示是否為捆綁檔生成源映射的標誌。 **選擇**,*預設 - 假*
* `sourceMapRootPath`:用於存儲生成的源映射檔的根路徑。

## <a name="build-time-execution-of-bundling-and-minification"></a>捆繫和最小化的建置時間

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet 包允許在生成時執行捆綁和分化。 該套件注入在產生和清潔時間執行的[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)。 生成過程分析*bundleconfig.json*檔,以便根據定義的配置生成輸出檔。

> [!NOTE]
> BuildBundlerMinifier 屬於 GitHub 上的社區驅動專案,Microsoft 不為其提供任何支援。 問題應[在此處](https://github.com/madskristensen/BundlerMinifier/issues)提交。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

將*BuildBundlerMinifier*包添加到專案中。

建置專案。 輸出視窗中會顯示以下內容:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

清理專案。 輸出視窗中會顯示以下內容:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

將*BuildBundlerMinifier*套件加入您的專案:

```dotnetcli
dotnet add package BuildBundlerMinifier
```

如果使用 ASP.NET 酷 1.x,請還原新添加的套件:

```dotnetcli
dotnet restore
```

建置專案：

```dotnetcli
dotnet build
```

出現以下內容:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

清除專案:

```dotnetcli
dotnet clean
```

即會出現下列輸出：

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>臨時執行捆綁和小化

無需構建專案,即可臨時運行捆綁和分化任務。 將[捆綁器 Minifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet 套件加入您的專案:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core 屬於 GitHub 上的社區驅動專案,Microsoft 不為其提供任何支援。 問題應[在此處](https://github.com/madskristensen/BundlerMinifier/issues)提交。

此包擴展 .NET 核心 CLI 以包括*點網捆綁*工具。 以下命令可以在套件管理員主控台 (PMC) 視窗或命令 shell 中執行:

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> NuGet 套件管理員將依賴項`<PackageReference />`作為節點添加到 *.csproj 檔。 僅當`dotnet bundle``<DotNetCliToolReference />`使用節點時,該命令才會註冊到 .NET Core CLI。 相應地修改 @.csproj 檔。

## <a name="add-files-to-workflow"></a>新增檔案到工作流

考慮新增類似以下內容的其他*自訂.css*檔案的範例:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

要將*自訂.css*進行小化並將其與*site.css*捆綁到*site.min.css*檔中,將相對路徑新增到*bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> 或者,可以使用以下 globing 模式:
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> 此 globing 模式匹配所有 CSS 檔,並排除已小化檔模式。

建置應用程式。 打開*網站.min.css*並注意到*自訂.css*的內容追加到文件的末尾。

## <a name="environment-based-bundling-and-minification"></a>基於環境的捆綁和小化

最佳做法是,應用的捆綁和微化檔應在生產環境中使用。 在開發過程中,原始檔使應用程式的調試更加容易。

在檢視中使用[環境標記説明程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)指定要包含在頁面中的檔。 環境標記説明程式僅在特定[環境中](xref:fundamentals/environments)運行時呈現其內容。

在環境中`environment`執行時,以下標記呈現未處理`Development`的 CSS 檔:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

以下`environment`標記在`Development`非環境中運行時呈現捆綁和已掃描的 CSS 檔。 例如,在`Production`中`Staging`運行 或觸發這些樣式表的呈現:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>使用來自 Gulp 的捆綁配置.json

在某些情況下,應用的捆綁和小化工作流需要額外的處理。 示例包括圖像優化、緩存破壞和CDN資產處理。 為了滿足這些要求,您可以將捆綁和最小化工作流轉換為使用 Gulp。

### <a name="use-the-bundler--minifier-extension"></a>使用捆綁器&最小值延伸器

可視化工作室[捆綁器&最小值擴展](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)處理轉換為 Gulp。

> [!NOTE]
> 捆綁器& Minifier 擴展屬於 GitHub 上的社區驅動專案,Microsoft 不支援該專案。 問題應[在此處](https://github.com/madskristensen/BundlerMinifier/issues)提交。

右鍵按一下解決方案資源管理程式中的*bundleconfig.json*檔,然後選擇**捆綁器&最小值** > **轉換為 Gulp...**

![轉換為 Gulp 上下文選單項目](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*gulpfile.js*和*包.json*檔將添加到專案中。 將安裝*包.json*檔`devDependencies`部分中列出的支援[npm](https://www.npmjs.com/)包。

在 PMC 視窗執行以下指令,以將 Gulp CLI 安裝為全域相依項:

```console
npm i -g gulp-cli
```

*gulpfile.js*檔讀取輸入、輸出和設置的*bundleconfig.json*檔。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>手動轉換

如果 Visual Studio 和/或捆綁器&最小值擴展不可用,則手動轉換。

將包含以下內容`devDependencies`的*套件.json*檔加入到專案根:

> [!WARNING]
> 該`gulp-uglify`模組不支援 ECMAScript (ES) 2015 / ES6 及更高版本。 安裝[口槽](https://www.npmjs.com/package/gulp-terser),`gulp-uglify`而不是使用 ES2015 / ES6 或更高版本。

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

透過*在 與套件*相同的等級執行以下指令來安裝依賴項:

```console
npm i
```

將 Gulp CLI 安裝為全域相依項:

```console
npm i -g gulp-cli
```

將下面的*gulpfile.js*複製到專案根目錄:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>執行 Gulp 工作

在 Visual Studio 中產生專案之前觸發 Gulp 小化任務,將以下[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)新增到 @.csproj 檔中:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

在此範例中,在`MyPreCompileTarget`目標內定義的任何任務都運行在預定義`Build`目標之前。 類似於以下內容的輸出將顯示在視覺化工作室的輸出視窗中:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

## <a name="additional-resources"></a>其他資源

* [使用 Grunt](xref:client-side/using-grunt)
* [使用多重環境](xref:fundamentals/environments)
* [標記說明器](xref:mvc/views/tag-helpers/intro)
