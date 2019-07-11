---
title: 配套並縮短在 ASP.NET Core 中的靜態資產
author: scottaddie
description: 了解如何藉由套用統合和縮製技術最佳化的 ASP.NET Core web 應用程式中的靜態資源。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: 6254a74fd0a11669706a2a89b156a3223e300d1c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813501"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>配套並縮短在 ASP.NET Core 中的靜態資產

藉由[Scott Addie](https://twitter.com/Scott_Addie)和[David 松](https://twitter.com/davidpine7)

這篇文章會說明套用統合和縮製，包括如何使用這些功能，ASP.NET Core web 應用程式的優點。

## <a name="what-is-bundling-and-minification"></a>統合和縮製是什麼

統合和縮製是您可以套用 web 應用程式中的兩種獨特的效能最佳化。 搭配使用，統合和縮製改善效能降低的伺服器要求數目以及減少要求的靜態資產的大小。

統合和縮製主要改善的第一個頁面要求載入時間。 一旦要求的網頁上，瀏覽器快取靜態資產 （JavaScript、 CSS 和映像）。 因此，統合和縮製不改善效能時要求相同的頁面上或要求相同的資產位於相同站台上的頁面。 如果過期標頭未正確設定資產並不使用統合和縮製，如果瀏覽器的有效期限啟發學習法標記資產過時幾天後。 此外，瀏覽器會針對每個資產需要將驗證要求。 在此情況下，統合和縮製提供效能改進，即使第一個網頁要求。

### <a name="bundling"></a>統合

統合將多個檔案結合成單一檔案。 統合可減少所需呈現的 web 資產，例如網頁伺服器要求的數目。 您可以建立任意數目的個別的套件組合，專為 CSS、 JavaScript 等。較少的檔案表示更少的 HTTP 要求從瀏覽器到伺服器或服務，提供您的應用程式。 這會導致改善第一次頁面載入效能。

### <a name="minification"></a>縮製

縮製從程式碼移除不必要的字元，而不需要變更的功能。 結果會是重大的大小減少要求的資產 （例如 CSS、 影像和 JavaScript 檔案）。 常見的縮製的副作用包括縮短至某一字元的變數名稱，並移除註解和不必要的空白字元。

請考慮下列的 JavaScript 函式：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

縮製可減少函式，如下：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

除了移除不必要的空白字元的註解，下列的參數和變數名稱已重新命名為，如下所示：

原始 | 已重新命名
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>統合和縮製的影響

下表概述個別載入資產和使用統合和縮製之間的差異：

動作 | B/m | Without B/M | 變更
--- | :---: | :---: | :---:
提出要求  | 7   | 18     | 157%
傳送的 KB | 156 | 264.68 | 70%
載入時間 （毫秒） | 885 | 2360   | 167%

瀏覽器會相當詳細資訊，關於 HTTP 要求標頭。 所傳送的總位元組時將大幅降低所看到的計量。 載入時間會顯示有明顯的改進，不過此範例會在本機執行。 使用統合和縮製與資產透過網路傳輸時，會實現更高的效能提升。

## <a name="choose-a-bundling-and-minification-strategy"></a>選擇統合和縮製的策略

MVC 和 Razor 頁面專案範本提供統合和縮製 JSON 組態檔所組成的立即可用的解決方案。 第三方工具，例如[Grunt](xref:client-side/using-grunt)工作執行器中，完成更多的複雜度與相同的工作。 第三方工具時，非常適合您的開發工作流程需要處理超過統合和縮製&mdash;linting 和映像最佳化等。 藉由使用的設計階段統合和縮製，縮短的檔案會在應用程式的部署之前建立。 統合及縮小部署之前提供的優點是減少的伺服器負載。 不過，務必要辨識該設計階段統合和縮製增組建的複雜度，並僅適用於靜態檔案。

## <a name="configure-bundling-and-minification"></a>設定統合和縮製

::: moniker range="<= aspnetcore-2.0"

在 ASP.NET Core 2.0 或更早版本，MVC 和 Razor 頁面專案範本會提供*bundleconfig.json*定義每一個套件組合的選項的組態檔：

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

在 ASP.NET Core 2.1 或更新版本中，加入新的 JSON 檔案，名為*bundleconfig.json*、 MVC 或 Razor 頁面專案根目錄。 做為起點，該檔案中包含下列 JSON:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

*Bundleconfig.json*檔案會定義每一個套件組合的選項。 在上述範例中，單一組合設定定義適用於自訂的 JavaScript (*wwwroot/js/site.js*) 和樣式表 (*wwwroot/css/site.css*) 檔案。

設定選項包括：

* `outputFileName`：組合檔案輸出名稱。 可以包含相對路徑*bundleconfig.json*檔案。 **required**
* `inputFiles`：要組合在一起的檔案陣列。 這些是在組態檔的相對路徑。 **選擇性**，* 空的輸出檔案中的值是空的結果。 [萬用字元](https://www.tldp.org/LDP/abs/html/globbingref.html)支援模式。
* `minify`：輸出類型縮製的選項。 **選擇性**，*預設值- `minify: { enabled: true }`*
  * 每個輸出檔案類型有組態選項。
    * [CSS 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`：旗標表示是否要將產生的檔案新增至專案檔。 **選擇性**，*預設為 false*
* `sourceMap`：旗標，指出是否要產生搭售檔案的來源對應。 **選擇性**，*預設為 false*
* `sourceMapRootPath`：用於儲存產生的來源對應檔的根路徑。

## <a name="build-time-execution-of-bundling-and-minification"></a>統合和縮製的建置時間執行

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/)執行統合和縮製在建置階段，可讓 NuGet 套件。 封裝會插入[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)執行在建置和清除的時間。 *Bundleconfig.json*建置程序，以產生根據定義的組態輸出檔案的分析檔案。

> [!NOTE]
> BuildBundlerMinifier 所屬的社群導向的 GitHub 上的專案，Microsoft 不提供支援。 問題應該提報[此處](https://github.com/madskristensen/BundlerMinifier/issues)。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

新增*BuildBundlerMinifier*封裝至您的專案。

建置專案。 在 [輸出] 視窗中顯示下列訊息：

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

清除專案。 在 [輸出] 視窗中顯示下列訊息：

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

新增*BuildBundlerMinifier*封裝至您的專案：

```console
dotnet add package BuildBundlerMinifier
```

如果使用 ASP.NET Core 1.x，還原新加入的封裝：

```console
dotnet restore
```

建置專案：

```console
dotnet build
```

顯示下列內容：

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

清除專案：

```console
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

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>統合和縮製的臨機操作執行

可以執行統合和縮製工作臨機操作，而不需建置專案。 新增[BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet 封裝加入您的專案：

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core 所屬的社群導向的 GitHub 上的專案，Microsoft 不提供支援。 問題應該提報[此處](https://github.com/madskristensen/BundlerMinifier/issues)。

此套件會擴充以包含.NET Core CLI *dotnet 套件組合*工具。 在 套件管理員主控台 (PMC) 視窗中，或在命令殼層中，可以執行下列命令：

```console
dotnet bundle
```

> [!IMPORTANT]
> NuGet 套件管理員將相依性新增至 *.csproj 檔，做為`<PackageReference />`節點。 `dotnet bundle`命令會向.NET Core CLI 時，才`<DotNetCliToolReference />`節點可用。 據此修改 *.csproj 檔案。

## <a name="add-files-to-workflow"></a>將檔案新增至工作流程

請考慮範例，其中額外*custom.css*檔案會加入類似於下列：

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

要縮短*custom.css*及組合與其*site.css*成*site.min.css*檔案中，新增的相對路徑*bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> 或者，您可以使用下列通用慣例模式：
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> 這個萬用字元模式比對所有 CSS 檔案，並排除縮短的檔案模式。

建置應用程式。 開啟*site.min.css* ，並注意內容*custom.css*附加至檔案結尾。

## <a name="environment-based-bundling-and-minification"></a>環境式統合和縮製

最佳做法，您的應用程式的統合和縮製檔案應在生產環境中。 在開發期間，原始的檔案，請以利偵錯應用程式。

指定要使用包含在您的網頁中的檔案[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)檢視中。 環境標籤協助程式只會呈現其內容，當執行在特定[環境](xref:fundamentals/environments)。

下列`environment`中執行時，標籤會呈現未處理的 CSS 檔案`Development`環境：

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

下列`environment`以外的其他環境中執行時，標籤會呈現統合和縮製的 CSS 檔案`Development`。 例如，在執行`Production`或`Staging`觸發這些樣式表呈現：

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>使用 Gulp 從 bundleconfig.json

有一些的情況中的應用程式統合和縮製工作流程需要額外的處理。 範例包括映像最佳化、 快取 busting，及 CDN 資產處理。 為了滿足這些需求，您可以將轉換使用 Gulp 的統合和縮製工作流程。

### <a name="use-the-bundler--minifier-extension"></a>使用 Bundler & Minifier 擴充功能

Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)擴充功能會處理轉換成 Gulp。

> [!NOTE]
> Microsoft 提供不支援的 GitHub 上的社群導向專案所屬 Bundler & Minifier 延伸模組。 問題應該提報[此處](https://github.com/madskristensen/BundlerMinifier/issues)。

以滑鼠右鍵按一下*bundleconfig.json*方案總管 中的檔案，然後選取**Bundler & Minifier** > **轉換至 Gulp...** :

![轉換至 Gulp 的操作功能表項目](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js*並*package.json*檔案會新增至專案。 支援[npm](https://www.npmjs.com/)中所列封裝*package.json*檔案的`devDependencies`安裝一節。

若要安裝 Gulp CLI 做為全域的相依性的 PMC 視窗中執行下列命令：

```console
npm i -g gulp-cli
```

*Gulpfile.js*檔案讀取*bundleconfig.json*輸入、 輸出和設定檔。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>手動轉換

如果 Visual Studio 和/或 Bundler & Minifier 擴充功能無法使用，以手動方式將轉換。

新增*package.json*檔案，以下列`devDependencies`，至專案根目錄：

> [!WARNING]
> `gulp-uglify`模組不支援 ECMAScript (ES) 2015年 / ES6 和更新版本。 安裝[gulp terser](https://www.npmjs.com/package/gulp-terser)而不是`gulp-uglify`使用 ES2015 / ES6 或更新版本。

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

位於相同層級中執行下列命令來安裝相依性*package.json*:

```console
npm i
```

安裝 Gulp CLI 做為全域的相依性：

```console
npm i -g gulp-cli
```

複製*gulpfile.js*專案根目錄的檔案如下：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>執行 Gulp 工作

若要觸發 Gulp 縮製工作，才能在 Visual Studio 中建置專案，新增下列[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)*.csproj 檔案：

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

在此範例中，任何工作定義內`MyPreCompileTarget`目標之前預先定義執行`Build`目標。 如下所示的輸出會顯示在 Visual Studio 的 [輸出] 視窗中：

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
* [標記協助程式](xref:mvc/views/tag-helpers/intro)
