---
uid: single-page-application/overview/templates/hottowel-template
title: 熱毛巾範本 |Microsoft 文件
author: madskristensen
description: HotTowel 範本
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875015"
---
<a name="hot-towel-template"></a>熱毛巾範本
====================
由[Mads Kristensen](https://github.com/madskristensen)

> John Papa 撰寫熱毛巾 MVC 範本
> 
> 選擇要下載哪個版本：
> 
> [Visual Studio 2012 的熱毛巾 MVC 範本](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 的熱毛巾 MVC 範本](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> 熱毛巾： 因為您不想要移至其中一個情況下 SPA ！


要建置 SPA，但無法決定要從何處開始嗎？ 使用熱毛巾和以秒為單位，您會有 SPA 和您需要在其上建置的所有工具 ！

熱毛巾建立用於建置 asp.net 單一頁面應用程式 (SPA) 的最佳起始點。 現成您它提供的模組化結構程式碼、 檢視瀏覽、 資料繫結、 豐富的資料管理和優雅地簡單卻又樣式。 熱毛巾提供所需的所有組建 SPA，因此您可以專注於您的應用程式不配管。

> 了解如何建置從 SPA [John Papa 視訊、 教學課程和 Pluralsight 課程](http://johnpapa.net/spa?vsix)。


## <a name="application-structure"></a>應用程式結構

熱毛巾 SPA 提供應用程式的資料夾含有定義您的應用程式的 JavaScript 和 HTML 檔案。

內部應用程式資料夾中：

- Durandal
- 服務
- viewmodels
- 檢視

應用程式資料夾中包含模組的集合。 這些模組封裝功能，並宣告對其他模組相依性。 [檢視] 資料夾包含您的應用程式的 HTML 和 viewmodels 資料夾包含的檢視 （一般 MVVM 模式） 的呈現邏輯。 [服務] 資料夾是適合用來儲存任何應用程式可能需要 HTTP 資料擷取或本機存放區互動之類的通用服務。 它是很常見的多個 viewmodels 重複使用來自服務模組的程式碼。

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC 伺服器端應用程式結構

熱毛巾熟悉且功能強大的 ASP.NET MVC 結構為基礎。

- 應用程式\_開始
- 內容
- Controllers
- 模型
- 指令碼
- 檢視

## <a name="featured-libraries"></a>精選文件庫

- ASP.NET MVC
- ASP.NET Web API
- ASP.NET Web 最佳化-統合及縮製
- [Breeze.js](http://Breezejs.com) -豐富的資料管理
- [Durandal.js](http://Durandaljs.com) -瀏覽和檢視構成要素
- [解 Knockout.js](http://Knockoutjs.com) -資料繫結
- [Require.js](http://requirejs.org) -使用 AMD 和最佳化，在模組化
- [Toastr.js](http://jpapa.me/c7toastr) -快顯訊息
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) -強固的 CSS 樣式

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>透過 Visual Studio 2012 熱毛巾 SPA 範本安裝

熱毛巾可以安裝為 Visual Studio 2012 範本。 只要按一下`File`  |  `New Project`選擇`ASP.NET MVC 4 Web Application`。 然後選取 ' 熱毛巾單一網頁應用程式 」 範本，並執行 ！

## <a name="installing-via-the-nuget-package"></a>透過 NuGet 封裝安裝

熱毛巾也是加強現有的空白 ASP.NET MVC 專案的 NuGet 封裝。 只使用 Nuget 來安裝，然後執行 ！

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>如何建立熱毛巾上呢？

只需啟動加入程式碼 ！

1. 加入您自己伺服器端程式碼，最好是 Entity Framework 和 WebAPI （其中分季 Breeze.js 與）
2. 加入檢視，以`App/views`資料夾
3. 加入至 viewmodels`App/viewmodels`資料夾
4. 將 HTML 和 Knockout 資料繫結加入至新的檢視
5. 更新中的瀏覽路由 `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>HTML/JavaScript 的逐步解說

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml 是開始的路由和 MVC 應用程式的檢視。 它包含所有標準 meta 標記、 css 連結，以及您希望 JavaScript 參考。 本文包含單一`<div>`也就是所有的內容 (views) 放置要求時。 `@Scripts.Render`用來執行包含此應用程式的程式碼的進入點的 Require.js`main.js`檔案。 示範如何建立啟動顯示畫面，您的應用程式載入時提供的啟動顯示畫面。

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js`檔案包含已載入您的應用程式時，立即將執行的程式碼。 這是您要定義巡覽路由、 設定您的檢視，啟動並執行任何設定/啟動載入例如 priming 應用程式的資料。

`main.js`檔案會定義數個 durandal 的模組，協助啟動應用程式開始。 定義陳述式可協助解決模組相依性，以供函式。 第一次會啟用偵錯訊息，再傳送至主控台視窗執行何種事件的相關訊息。 App.start 程式碼會告知 durandal 架構，以啟動應用程式。 使 durandal 知道所有檢視和 viewmodels 分別包含在相同的具名資料夾中，設定慣例。 最後，`app.setRoot`一開始載入`shell`使用預先定義`entrance`動畫。

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>檢視

檢視存在於`App/views`資料夾。

### <a name="shellhtml"></a>shell.html

`shell.html`主版面配置包含您的 HTML。 所有其他檢視中 」 的一端必須某處組合您`shell`檢視。 熱毛巾提供`shell`這類的三個區域： 標頭、 內容區域和頁尾。 每一個地區載入內容形成要求時的其他檢視。

`compose`頁首和頁尾的繫結是硬式編碼中指向熱毛巾`nav`和`footer`分別檢視。 撰寫繫結區段`#content`繫結至`router`模組的使用中的項目。 換句話說，當您按一下 瀏覽連結，它是對應的檢視中載入此區域。

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` SPA 包含瀏覽連結。 這是功能表結構可以放在哪裡，例如。 通常這是資料繫結 （使用 Knockout） 至`router`模組顯示中所定義的導覽`shell.js`。 Knockout 會尋找資料繫結屬性和繫結至`shell`viewmodel 以顯示 瀏覽路由，並顯示進度列 （使用 Twitter 的啟動程序） 如果`router`模組正在進行瀏覽不同的檢視，至另一個 （請參閱`router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>home.html 和 details.html

這些檢視包含 HTML 之自訂檢視。 當`home`中連結`nav`按一下檢視的功能表時，`home`檢視將會放在內容區域的`shell`檢視。 這些檢視可以擴充或取代您自己的自訂檢視。

### <a name="footerhtml"></a>footer.html

`footer.html`包含出現在頁尾中，在底部的 HTML`shell`檢視。

## <a name="viewmodels"></a>ViewModels

在中找到 ViewModels`App/viewmodels`資料夾。

### <a name="shelljs"></a>shell.js

`shell` Viewmodel 包含屬性和函式繫結至`shell`檢視。 通常這是位於功能表巡覽繫結 (請參閱`router.mapNav`邏輯)。

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>home.js 並 details.js

這些 viewmodels 包含的屬性和函式繫結至`home`檢視。 它也包含檢視的呈現邏輯，資料和檢視之間黏附。

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>服務

服務應用程式/服務資料夾中找到。 在理想情況下無法放置 dataservice 模組，負責取得和張貼遠端資料，例如您未來的服務。

### <a name="loggerjs"></a>logger.js

熱毛巾提供`logger`模組服務 資料夾中的。 `logger`模組非常適合用來記錄訊息至主控台，以及在快顯通知的顯中的使用者。
