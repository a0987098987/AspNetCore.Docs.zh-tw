---
title: 在 ASP.NET Core 中組合及縮小靜態資產
author: scottaddie
description: 瞭解如何藉由套用捆綁和縮制技術，將 ASP.NET Core web 應用程式中的靜態資源優化。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2020
no-loc:
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
uid: client-side/bundling-and-minification
ms.openlocfilehash: 5db6ab3d790257c677c0a4ed7e605eb39c2982ed
ms.sourcegitcommit: cc845634a490c49ff869c89b6e422b6d65d0e886
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87159715"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>在 ASP.NET Core 中組合及縮小靜態資產

由[Scott Addie](https://twitter.com/Scott_Addie)和[David 松樹](https://twitter.com/davidpine7)

本文說明套用配套和縮制的優點，包括如何將這些功能與 ASP.NET Core web 應用程式搭配使用。

## <a name="what-is-bundling-and-minification"></a>什麼是捆綁和縮制

捆綁和縮制是您可以在 web 應用程式中套用的兩個不同的效能優化。 搭配使用、結合和縮制可減少伺服器要求的數目，以及減少所要求靜態資產的大小，進而提升效能。

組合和縮制主要會改善第一頁的要求載入時間。 一旦要求網頁之後，瀏覽器就會快取靜態資產（JavaScript、CSS 和影像）。 因此，在要求相同資產的相同網站上要求相同頁面（或頁面）時，配套和縮制並不會改善效能。 如果資產上的 expires 標頭未正確設定，且未使用配套和縮制，則瀏覽器的有效性啟發式會將幾天後的資產過時。 此外，瀏覽器需要每個資產的驗證要求。 在此情況下，即使在第一頁要求之後，配套和縮制也可提供效能改進。

### <a name="bundling"></a>統合

統合可將多個檔案合併成單一檔案。 「組合」可減少呈現 web 資產（例如網頁）所需的伺服器要求數目。 您可以針對 CSS、JavaScript 等建立任意數目的個別組合。較少的檔案表示從瀏覽器到伺服器的 HTTP 要求較少，或提供您應用程式的服務。 這會導致第一頁載入效能改進。

### <a name="minification"></a>縮製

縮制會從程式碼移除不必要的字元，而不會改變功能。 結果會大幅減少要求的資產（例如 CSS、影像和 JavaScript 檔案）的大小。 縮制的常見副作用包括縮短變數名稱為一個字元，以及移除批註和不必要的空白字元。

請考慮下列 JavaScript 函數：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

縮制會將函數縮減為下列內容：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

除了移除批註和不必要的空白字元以外，下列參數和變數名稱的重新命名方式如下：

原始 | 已重新命名
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>捆綁與縮制的影響

下表概述個別載入資產與使用配套和縮制之間的差異：

動作 | 使用 B/M | 不含 B/M 的 | 變更
--- | :---: | :---: | :---:
檔案要求  | 7   | 18     | 157%
已傳輸 KB | 156 | 264.68 | 70%
載入時間（毫秒） | 885 | 2360   | 167%

瀏覽器對於 HTTP 要求標頭而言相當冗長。 [已傳送的位元組總數] 計量在進行捆綁時明顯降低。 載入時間顯示顯著的改進，但此範例會在本機執行。 搭配透過網路傳輸的資產使用配套和縮制時，就可以獲得更佳的效能提升。

## <a name="choose-a-bundling-and-minification-strategy"></a>選擇捆綁和縮制策略

MVC 和 :::no-loc(Razor)::: Pages 專案範本提供了一種解決方案，可供包含 JSON 設定檔的組合和縮制使用。 協力廠商工具（例如[Grunt](xref:client-side/using-grunt)工作執行器）會以更複雜的方式來完成相同的工作。 當您的開發工作流程需要處理超出配套和縮制 &mdash; （例如 linting 和影像優化）時，協力廠商工具是很好的組合。 藉由使用設計階段組合和縮制，縮減檔案會在應用程式部署之前建立。 部署之前的捆綁與縮小提供伺服器負載降低的優點。 不過，請務必辨識設計階段的組合和縮制會增加組建複雜度，而且只適用于靜態檔案。

## <a name="configure-bundling-and-minification"></a>設定捆綁和縮制

::: moniker range="<= aspnetcore-2.0"

在 ASP.NET Core 2.0 或更早版本中，MVC 和 :::no-loc(Razor)::: Pages 專案範本會提供設定檔案的*bundleconfig.js* ，以定義每個組合的選項：

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

在 ASP.NET Core 2.1 或更新版本中，將名為*bundleconfig.js的*新 JSON 檔案加入至 MVC 或 :::no-loc(Razor)::: Pages 專案根目錄。 在該檔案中包含下列 JSON 做為起點：

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

檔案*上的bundleconfig.js*會定義每個配套的選項。 在上述範例中，會針對自訂 JavaScript （*wwwroot/js/site.js*）和樣式表單（*wwwroot/css/.css*）檔案定義單一組合設定。

設定選項包括：

* `outputFileName`：要輸出的配套檔案名。 可以包含檔案中*bundleconfig.js*的相對路徑。 **必填**
* `inputFiles`：要組合在一起的檔案陣列。 這些是設定檔的相對路徑。 （**選擇性**） * 空白值會產生空的輸出檔。 支援[萬用字元模式。](https://www.tldp.org/LDP/abs/html/globbingref.html)
* `minify`：輸出類型的縮制選項。 **選擇性**，*預設值 `minify: { enabled: true }` -*
  * 每個輸出檔案類型都有可用的設定選項。
    * [CSS Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML Minifier](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`：表示是否要將產生的檔案加入至專案檔的旗標。 **選擇性**，*預設值-false*
* `sourceMap`：表示是否要產生配套檔案之來源對應的旗標。 **選擇性**，*預設值-false*
* `sourceMapRootPath`：用來儲存所產生之來源對應檔的根路徑。

## <a name="add-files-to-workflow"></a>將檔案新增至工作流程

假設有一個範例，其中加入了類似下列的其他*自訂 .css*檔案：

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

若要縮小*自訂的 .css* ，並將*它與 .css 結合在**網站. 最小的 .css*檔案中，請將相對路徑新增至*bundleconfig.js*：

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> 或者，也可以使用下列萬用字元模式：
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> 這個萬用字元模式會比對所有 CSS 檔案，並排除縮減檔案模式。

建置應用程式。 開啟 [*網站*]，並注意 [*自訂 .css* ] 的內容會附加至檔案結尾。

## <a name="environment-based-bundling-and-minification"></a>以環境為基礎的捆綁和縮制

最佳做法是，應用程式的配套和縮減檔案應該在生產環境中使用。 在開發期間，原始檔案可讓您更輕鬆地對應用程式進行偵錯工具。

使用您的瀏覽器中的[環境](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)標籤協助程式，指定要包含在頁面中的檔案。 環境標籤協助程式只會在特定[環境](xref:fundamentals/environments)中執行時呈現其內容。

下列標記會在 `environment` 環境中執行時呈現未處理的 CSS 檔案 `Development` ：

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

在以外的環境中執行時，下列標記會轉譯 `environment` 配套和縮減的 CSS 檔案 `Development` 。 例如，在或中執行時，會 `Production` `Staging` 觸發這些樣式表單的呈現：

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>從 Gulp 取用 bundleconfig.js

在某些情況下，應用程式的捆綁與縮制工作流程需要額外的處理。 範例包括影像優化、快取破壞和 CDN 資產處理。 若要滿足這些需求，您可以將包裝和縮制工作流程轉換為使用 Gulp。

### <a name="manually-convert-the-bundling-and-minification-workflow-to-use-gulp"></a>手動將配套與縮制工作流程轉換為使用 Gulp

使用下列內容將*package.js*新增 `devDependencies` 至專案根目錄：

> [!WARNING]
> `gulp-uglify`模組不支援 ECMAScript （ES） 2015/ES6 和更新版本。 安裝[gulp-terser](https://www.npmjs.com/package/gulp-terser) ，而不是 `gulp-uglify` 使用 ES2015/ES6 或更新版本。

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

在與package.js的相同層級*上*執行下列命令，以安裝相依性：

```bash
npm i
```

將 Gulp CLI 安裝為全域相依性：

```bash
npm i -g gulp-cli
```

將下列*gulpfile.js*檔案複製到專案根目錄：

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>執行 Gulp 工作

若要在專案建立 Visual Studio 之前觸發 Gulp 縮制工作：

1. 安裝[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier) NuGet 套件。
1. 將下列[MSBuild 目標](/visualstudio/msbuild/msbuild-targets)加入至專案檔：

    [!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

在此範例中，目標內定義的任何工作都會在 `MyPreCompileTarget` 預先定義的目標之前執行 `Build` 。 與下列類似的輸出會出現在 Visual Studio 的 [輸出] 視窗中：

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
* [標籤協助程式](xref:mvc/views/tag-helpers/intro)
