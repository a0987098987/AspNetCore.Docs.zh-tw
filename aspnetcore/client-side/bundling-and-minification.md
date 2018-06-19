---
title: 在 ASP.NET Core 組合和 minifiy 靜態資產
author: scottaddie
description: 了解如何最佳化 ASP.NET Core web 應用程式中的靜態資源套用統合及縮製的技術。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: a3d49315fbb62eb1a42eb1b30885dc19a81c0a91
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094641"
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a>在 ASP.NET Core 組合和 minifiy 靜態資產

作者：[Scott Addie](https://twitter.com/Scott_Addie)

這篇文章會說明套用統合及縮製，包括如何使用這些功能，與 ASP.NET Core web 應用程式的優點。

## <a name="what-is-bundling-and-minification"></a>統合及縮製為何？

組合和縮製都可套用 web 應用程式中的兩個不同的效能最佳化。 一起使用，統合及縮製改善效能降低伺服器的要求數目以及減少要求的靜態資產的大小。

統合及縮製主要改善第一個頁面要求載入時間。 一旦要求的網頁上，瀏覽器快取靜態資產 （JavaScript、 CSS 和映像）。 因此，統合及縮製不改善效能時要求相同的頁面或頁面，要求相同的資產在相同網站。 如果到期標頭未正確設定資產並不使用統合及縮製，如果瀏覽器的有效期限啟發學習法標示資產過時幾天之後。 此外，瀏覽器會需要為每個資產的驗證要求。 在此情況下，統合及縮製提供改進效能，即使第一個頁面要求。

### <a name="bundling"></a>結合在一起

結合在一起將多個檔案合併成單一檔案。 結合在一起，減少所需呈現 web 資產，例如網頁伺服器要求的數目。 您可以建立任意數目的個別組合，專為 CSS、 JavaScript 等。較少的檔案表示更少的 HTTP 要求從瀏覽器至伺服器或提供您的應用程式的服務。 這會導致更佳的第一個頁面負載效能。

### <a name="minification"></a>縮小

縮製從程式碼移除不必要的字元，而不需變更功能。 結果會是重要的大小降低要求資產 （例如 CSS、 影像和 JavaScript 檔案）。 常見的縮製的副作用包括縮短成一個字元的變數名稱，以及移除註解和不必要的空白字元。

請考慮下列的 JavaScript 函式：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

縮製減少函式所示：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

移除註解和不必要的空白字元，除了下列的參數和變數名稱已命名，如下所示：

原始 | 已重新命名
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>統合及縮製的影響

下表摘要列出個別資產的載入和使用統合及縮製之間的差異：

動作 | 使用 B/M | 沒有 B/M | 變更
--- | :---: | :---: | :---:
檔案要求  | 7   | 18     | 157%
傳送的 KB | 156 | 264.68 | 70%
載入時間 （毫秒） | 885 | 2360   | 167%

瀏覽器會相當詳細資訊，關於 HTTP 要求標頭。 所傳送的總位元組度量所看到的大幅降低結合在一起。 載入時間顯示有明顯的改進，不過此範例會在本機執行。 統合及縮製使用資產透過網路傳輸時，會實現更高的效能提升。

## <a name="choose-a-bundling-and-minification-strategy"></a>選擇統合及縮製的策略

MVC 和 Razor 頁面的專案範本提供的方塊外方案統合及縮製的 JSON 組態檔所組成。 協力廠商工具，例如[Gulp](xref:client-side/using-gulp)和[Grunt](xref:client-side/using-grunt)工作的說明，完成更多的複雜性與相同的工作。 協力廠商工具的絕佳符合時是您的開發工作流程需要處理超出統合及縮製&mdash;例如 linting 和映像最佳化。 使用設計階段組合和縮製，應用程式的部署之前，先建立這些縮短的檔案。 統合及縮小部署之前提供的優點減少的伺服器負載。 不過，務必辨識該設計階段組合和縮製增加建置複雜，而且只適用於靜態檔案。

## <a name="configure-bundling-and-minification"></a>統合及縮製的設定

MVC 和 Razor 頁面 專案範本提供*bundleconfig.json*組態檔會定義每個組合的選項。 依預設，單一組合組態定義的自訂 javascript (*wwwroot/js/site.js*) 和樣式表 (*wwwroot/css/site.css*) 檔案：

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

設定選項包括：

* `outputFileName`: 要輸出的組合檔案的名稱。 可包含相對路徑*bundleconfig.json*檔案。 **所需**
* `inputFiles`： 要配套起來的檔案陣列。 這些是在組態檔的相對路徑。 **選擇性**，* 空值會導致空的輸出檔案。 [通用慣例](http://www.tldp.org/LDP/abs/html/globbingref.html)支援的模式。
* `minify`： 輸出型別縮製選項。 **選擇性**，*預設值- `minify: { enabled: true }`*
  * 每個輸出檔案類型有組態選項。
    * [CSS 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`： 旗標，指出是否要將產生的檔案加入至專案檔。 **選擇性**，*預設為 false*
* `sourceMap`： 指出是否要產生將配套的檔案的來源對應的旗標。 **選擇性**，*預設為 false*
* `sourceMapRootPath`： 用於儲存產生的來源對應檔的根路徑。

## <a name="build-time-execution-of-bundling-and-minification"></a>建置時間執行的統合及縮製

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet 封裝啟用的結合在一起執行，以及在建置階段縮製。 封裝會插入[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)在建置和清除的時間執行的。 *Bundleconfig.json*建置程序來產生輸出檔案，根據定義的組態分析檔案。

> [!NOTE]
> BuildBundlerMinifier 所屬的社群導向的專案，Microsoft 不提供任何支援的 GitHub 上。 問題應必填欄位[這裡](https://github.com/madskristensen/BundlerMinifier/issues)。

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

出現下列輸出：

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>統合及縮製的臨機操作執行

您可根據臨機操作，執行統合及縮製的工作，而不需建置專案。 新增[BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet 封裝加入您的專案：

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core 所屬的社群導向的專案，Microsoft 不提供任何支援的 GitHub 上。 問題應必填欄位[這裡](https://github.com/madskristensen/BundlerMinifier/issues)。

此套件會擴充以包含.NET Core CLI *dotnet 配套*工具。 在 封裝管理員主控台 (PMC) 視窗或在命令殼層中，可以執行下列命令：

```console
dotnet bundle
```

> [!IMPORTANT]
> NuGet 套件管理員將相依性加入至 *.csproj 檔案做為`<PackageReference />`節點。 `dotnet bundle`命令已向.NET Core CLI 時，才`<DotNetCliToolReference />` 節點可用。 適當地修改 *.csproj 檔案。

## <a name="add-files-to-workflow"></a>將檔案加入至工作流程

請考慮使用範例中的額外*custom.css*檔案會加入與下列類似下列：

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

要縮短*custom.css*和組合使用*site.css*到*site.min.css*檔案中，加入相對路徑*bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> 或者，您可以使用下列通用慣例模式：
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> 此通用慣例模式比對所有 CSS 檔案，但不包括縮短的檔案模式。

建置應用程式。 開啟*site.min.css*注意到的內容和*custom.css*附加至檔案結尾。

## <a name="environment-based-bundling-and-minification"></a>環境架構統合及縮製

最佳做法，您的應用程式的配套和縮短檔應該用於實際執行環境。 在開發期間，原始的檔案，請為方便偵錯應用程式。

指定要包含在您的網頁中使用哪些檔案[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)在檢視中。 環境標記協助程式只會呈現其內容，在特定中執行時[環境](xref:fundamentals/environments)。

下列`environment`標記會呈現未處理的 CSS 檔案中執行時`Development`環境：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

下列`environment`以外的環境中執行時，標記會轉譯配套並縮短的 CSS 檔案`Development`。 例如，在執行`Production`或`Staging`呈現這些樣式表的觸發程序：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>使用從 Gulp bundleconfig.json

一些情況下，應用程式的統合及縮製的工作流程需要額外的處理。 範例包括映像最佳化、 快取 busting，及 CDN 資產處理。 為了滿足這些需求，您可以將轉換使用 Gulp 統合及縮製工作流程。

### <a name="use-the-bundler--minifier-extension"></a>使用搭配程式 （& s） 縮短程式延伸模組

Visual Studio[搭配程式 （& s) 縮短程式](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)延伸模組會處理 Gulp 的轉換。

> [!NOTE]
> 搭配程式 （& s） 縮短程式副檔名所屬的社群導向的專案，Microsoft 不提供任何支援的 GitHub 上。 問題應必填欄位[這裡](https://github.com/madskristensen/BundlerMinifier/issues)。

以滑鼠右鍵按一下*bundleconfig.json*檔案在 [方案總管]，然後選取**搭配程式 （& s) 縮短程式** > **轉換至 Gulp...**:

![轉換至 Gulp 的操作功能表項目](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js*和*package.json*檔案會新增至專案。 支援[npm](https://www.npmjs.com/)封裝中所列*package.json*檔案的`devDependencies`安裝 > 一節。

若要安裝的 Gulp CLI 為全域的相依性 PMC 視窗執行下列命令：

```console
npm i -g gulp-cli
```

*Gulpfile.js*檔案讀取*bundleconfig.json*輸入、 輸出和設定檔。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>手動轉換

如果 Visual Studio 和 （或） 搭配程式 （& s） 縮短程式擴充功能無法使用，手動轉換。

新增*package.json*檔案，以下列`devDependencies`，專案根目錄：

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

在相同的層級執行下列命令來安裝相依性*package.json*:

```console
npm i
```

做為全域的相依性安裝的 Gulp CLI:

```console
npm i -g gulp-cli
```

複製*gulpfile.js*專案根目錄底下的檔案：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>執行 Gulp 工作

若要觸發 Gulp 縮製工作，在專案之前建置 Visual Studio 中，加入下列[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)*.csproj 檔案：

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

在此範例中，任何工作定義內`MyPreCompileTarget`執行之前的預先定義的目標`Build`目標。 Visual Studio 的 [輸出] 視窗中會出現類似下面的輸出：

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

或者，Visual Studio 工作執行器總管可能用來將 Gulp 工作繫結至特定的 Visual Studio 事件。 請參閱[執行預設工作](xref:client-side/using-gulp#running-default-tasks)如需這麼做可指示。

## <a name="additional-resources"></a>其他資源

* [使用 Gulp](xref:client-side/using-gulp)
* [使用 Grunt](xref:client-side/using-grunt)
* [使用多重環境](xref:fundamentals/environments)
* [標記協助程式](xref:mvc/views/tag-helpers/intro)
