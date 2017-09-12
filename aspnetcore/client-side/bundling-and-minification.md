---
title: "統合及縮製中 ASP.NET Core"
author: spboyer
description: 
keywords: "ASP.NET Core 組合和縮製、 CSS、 JavaScript、 縮短，BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: d8512bdd49b61019f22a49900bdd65086d821a6b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>統合及縮製中 ASP.NET Core

統合及縮製是兩項技術可用於 ASP.NET 網頁載入效能改善 web 應用程式。 結合在一起將多個檔案合併成單一檔案。 縮製執行各種不同的程式碼最佳化，以指令碼和 CSS，這會產生較小的裝載。 一起使用，統合及縮製載入時間效能透過減少伺服器的要求數目，以及改進降低要求資產 （例如 CSS 和 JavaScript 檔案） 的大小。

本文件說明使用統合及縮製，包括如何使用這些功能，與 ASP.NET Core 應用程式的優點。

## <a name="overview"></a>概觀

在 ASP.NET Core 應用程式，有多個統合及縮小用戶端資源選項。 MVC 的核心範本提供的方塊外解決方案使用組態檔和 BuildBundlerMinifier NuGet 封裝。 協力廠商工具，例如[Gulp](using-gulp.md)和[Grunt](using-grunt.md)也可用來完成相同的工作，應該您的程序需要額外的工作流程或變得複雜。 使用設計階段組合和縮製，應用程式的部署之前，先建立這些縮短的檔案。 統合及縮小部署之前提供的優點減少的伺服器負載。 不過，務必辨識該設計階段組合和縮製增加建置複雜，而且只適用於靜態檔案。

統合及縮製主要改善第一個頁面要求載入時間。 一旦要求的網頁上，瀏覽器快取資產 （JavaScript、 CSS 和圖像） 因此統合及縮製將不提供任何提升效能，當要求相同的頁面上，或在相同的網頁站台要求相同的資產。 如果您不需要設定到期標頭，在您的資產上正確和不使用統合及縮製、 瀏覽器的有效期限啟發學習法會將標示為資產過時幾天之後和瀏覽器將會需要為每個資產的驗證要求。 在此情況下，統合及縮製即使在第一個頁面要求後提供的效能提升。

### <a name="bundling"></a>結合在一起

結合在一起是功能可讓您輕鬆地結合或多個檔案配套成單一檔案。 結合在一起時，會將多個檔案結合成單一檔案，因為它可減少擷取及顯示 web 資產，例如網頁所需的伺服器的要求數目。 您可以建立 CSS、 JavaScript 和其他組合。 較少的檔案表示更少的 HTTP 要求從瀏覽器，以在伺服器或提供您的應用程式的服務。 這會導致更佳的第一個頁面負載效能。

### <a name="minification"></a>縮小

縮製執行各種不同的程式碼最佳化，以降低要求資產 （例如 CSS、 影像、 JavaScript 檔案） 的大小。 常見的縮製的結果包含移除不必要的空白字元和註解，並縮短成一個字元的變數名稱。

請考慮下列的 JavaScript 函式：

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

之後縮製，函式會減少所示：

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

移除註解和不必要的空白字元，除了下列參數和變數名稱已重新命名 （縮短），如下所示：

原始 | 已重新命名
--- | :---:
imageTagAndImageID | t
imageContext | 一個
imageElement | r

## <a name="impact-of-bundling-and-minification"></a>統合及縮製的影響

下表顯示個別列出所有資產，並使用簡單的 web 網頁上的統合及縮製之間的數個重要差異：

動作 | 使用 B/M | 沒有 B/M | 變更
--- | :---: | :---: | :---:
檔案要求 |7 | 18 | 157%
傳送的 KB | 156 | 264.68 | 70%
載入時間 （毫秒） | 885 | 2360 | 167%

傳送的位元組有大幅降低與結合在一起的瀏覽器相當詳細資訊，以套用要求的 HTTP 標頭。 載入時間顯示大改進，但此範例已在本機執行。 統合及縮製使用資產透過網路傳輸時，您會得到更提升效能。

## <a name="using-bundling-and-minification-in-a-project"></a>在專案中使用統合及縮製

MVC 專案範本提供`bundleconfig.json`組態檔會定義每個組合的選項。 依預設，單一組合組態定義的自訂 javascript (`wwwroot/js/site.js`) 和樣式表 (`wwwroot/css/site.css`) 檔案。

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

組合的選項包括：

* outputFileName-要輸出的組合檔案的名稱。 可包含相對路徑`bundleconfig.json`檔案。 **所需**
* inputFiles-要配套起來的檔案陣列。 這些是在組態檔的相對路徑。 **選擇性**，* 空值會導致空的輸出檔案。 [通用慣例](http://www.tldp.org/LDP/abs/html/globbingref.html)支援的模式。
* 縮短-縮小選項的輸出類型。 **選擇性**，*預設值-`minify: { enabled: true }`*
  * 每個輸出檔案類型有組態選項。
    * [CSS 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [HTML 縮短程式](https://github.com/madskristensen/BundlerMinifier/wiki)
* includeInProject-將產生的檔案加入至專案檔。 **選擇性**，*預設為 false*
* Sourcemap-產生將配套的檔案的來源對應。 **選擇性**，*預設為 false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017年

開啟`bundleconfig.json`在 Visual Studio 中，如果您的環境沒有安裝; 此擴充提示會建議是否都有一個可以協助進行這種檔案類型。

![BuildBundlerMinifier 擴充功能的建議](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

選取檢視擴充功能，並安裝**搭配程式 （& s) 縮短程式**擴充功能 （需要 Visual Studio 重新啟動）。

![BuildBundlerMinifier 擴充功能的建議](../client-side/bundling-and-minification/_static/view-extension.png)

重新啟動完成時，您需要設定要執行的處理程序縮短和結合在一起的用戶端資產的組建。 以滑鼠右鍵按一下`bundleconfig.json`檔案，然後選取*啟用組合建置...*.

建置專案，而`bundleconfig.json`包含在建置程序，以產生根據組態的輸出檔。

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Visual Studio 程式碼或命令列

Visual Studio 的擴充功能結合在一起的磁碟機和縮製程序使用 GUI 手勢。不過，相同的功能可與`dotnet`CLI 和 BuildBundlerMinifier NuGet 封裝。

將 NuGet 封裝加入至您的專案：

```console
dotnet add package BuildBundlerMinifier
```

還原的相依性：

```console
dotnet restore
```

建置應用程式：

```console
dotnet build
```

建置命令的輸出顯示縮製及/或根據設定結合在一起的結果。

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>新增檔案

在此範例中，其他的 CSS 檔案會加入稱為`custom.css`而且設定為統合及縮製與`site.css`，並產生單一`site.min.css`。

custom.css

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

新增的相對路徑`bundleconfig.json`。

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> 或者，您無法使用通用慣例模式-`"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]`此 cmdlet 會取得所有 CSS 檔，並排除縮短的檔案模式。

建置應用程式，如果您開啟`site.min.css`，您會立即注意的內容`custom.css`已附加至檔案結尾。

## <a name="controlling-bundling-and-minification"></a>控制統合及縮製

一般情況下，您要使用您的應用程式只會在實際執行環境的配套和縮短檔。 在開發期間，您要使用原始的檔案，因此您的應用程式偵錯更容易。

您可以指定哪些指令碼和 CSS 檔案，以包含在您使用環境標記協助程式在版面配置頁面的頁面 (請參閱[標記協助程式](../mvc/views/tag-helpers/index.md))。 在特定的環境中執行時，環境標記協助程式只會呈現其內容。 請參閱[使用多個環境](../fundamentals/environments.md)如指定目前環境的詳細資訊。

下列環境標記將會呈現未處理的 CSS 檔案中執行時`Development`環境：

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

只有在執行時，此環境標記會轉譯配套並縮短的 CSS 檔案`Production`或`Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>使用從 Gulp bundleconfig.json

如果您的應用程式統合及縮製工作流程需要額外的處理序，例如映像處理、 快取 busting、 CDN assest 處理 」 等，您可以將組合和 Minify 程序轉換成 Gulp。

> [!NOTE]
> 僅適用於 Visual Studio 2015 和 2017年 [轉換] 選項。

以滑鼠右鍵按一下`bundleconfig.json`選取**Gulp 轉換成...**.這會產生`gulpfile.js`並安裝必要的 npm 封裝。

![將轉換成 Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

`gulpfile.js`產生讀取`bundleconfig.json`檔案設定，因此它可以繼續使用輸入/輸出和設定。

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

若要在 Visual Studio 2017 建置專案時，請啟用 Gulp，請先 *.csproj 檔案中加入下列：

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

若要啟用 Gulp，Visual Studio 2015 中建置專案時，將下列內容加入`project.json`檔案：

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>其他資源

* [使用 Gulp](using-gulp.md)
* [使用 Grunt](using-grunt.md)
* [使用多個環境](../fundamentals/environments.md)
* [標記協助程式](../mvc/views/tag-helpers/index.md)
