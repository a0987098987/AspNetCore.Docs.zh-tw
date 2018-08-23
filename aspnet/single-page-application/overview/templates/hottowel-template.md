---
uid: single-page-application/overview/templates/hottowel-template
title: 熱 Towel 範本 |Microsoft Docs
author: madskristensen
description: HotTowel 範本
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835061"
---
<a name="hot-towel-template"></a>Hot Towel 範本
====================
藉由[Mads Kristensen](https://github.com/madskristensen)

> 最忙碌的毛巾 MVC 範本作者 John Papa
> 
> 選擇要下載的版本：
> 
> [適用於 Visual Studio 2012 的最忙碌的毛巾 MVC 範本](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Visual Studio 2013 的最忙碌的毛巾 MVC 範本](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> 熱毛巾： 因為您不想要移至其中一個情況下 SPA ！


要建置 SPA，但無法決定要從何處開始？ 使用熱門的毛巾，並以秒為單位，您將有 SPA 並在其上建置所需的所有工具 ！

最忙碌的毛巾建立好的切入點，來建置使用 ASP.NET 單一頁面應用程式 (SPA)。 根據預設，您它提供模組化結構程式碼、 檢視瀏覽、 資料繫結、 豐富的資料管理和簡單但簡潔樣式。 最忙碌的毛巾提供您要建置 SPA，讓您可以專注於您的應用程式未連結的所有項目。

> 了解如何建置從 SPA [John Papa 的影片、 教學課程和 Pluralsight 課程](http://johnpapa.net/spa?vsix)。


## <a name="application-structure"></a>應用程式結構

最忙碌的毛巾 SPA 提供應用程式資料夾，其中包含定義您的應用程式的 JavaScript 和 HTML 檔案。

在 [應用程式] 資料夾中：

- durandal
- 服務
- viewmodel
- 檢視

應用程式資料夾中包含模組的集合。 這些模組會封裝功能，並宣告對其他模組的相依性。 [Views] 資料夾包含您的應用程式的 HTML 和 viewmodel 資料夾包含檢視 （一般 MVVM 模式） 的展示邏輯。 [服務] 資料夾是適合用來裝載您的應用程式可能需要 HTTP 資料擷取或本機儲存體互動等任何通用服務。 很常見的多個 viewmodel 重複使用從服務模組的程式碼。

## <a name="aspnet-mvc-server-side-application-structure"></a>ASP.NET MVC 伺服器端應用程式結構

最忙碌的毛巾熟悉且功能強大的 ASP.NET MVC 結構為基礎。

- 應用程式\_開始
- 內容
- Controllers
- 模型
- 指令碼
- 檢視

## <a name="featured-libraries"></a>精選文件庫

- ASP.NET MVC
- ASP.NET Web API
- ASP.NET Web 最佳化-統合和縮製
- [Breeze.js](http://Breezejs.com) -豐富的資料管理
- [Durandal.js](http://Durandaljs.com) -瀏覽和檢視構成要素
- [Knockout.js](http://Knockoutjs.com) -資料繫結
- [Require.js](http://requirejs.org) -使用 AMD 和最佳化的模組化
- [Toastr.js](http://jpapa.me/c7toastr) -快顯訊息
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) -強大的 CSS 樣式

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>透過 Visual Studio 2012 的最忙碌的毛巾 SPA 範本安裝

最忙碌的毛巾可以安裝為 Visual Studio 2012 的範本。 只要按一下`File`  |  `New Project` ，然後選擇  `ASP.NET MVC 4 Web Application`。 然後選取 ' 熱毛巾單一頁面應用程式 」 範本，並執行 ！

## <a name="installing-via-the-nuget-package"></a>透過 NuGet 套件安裝

最忙碌的毛巾也是加強現有的空白 ASP.NET MVC 專案的 NuGet 套件。 只使用 Nuget 來安裝，然後再執行 ！

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>如何建置在熱門的毛巾？

只要開始加入程式碼 ！

1. 新增您自己伺服器端程式碼，最好是 Entity Framework 和 WebAPI （這會真正顯現 Breeze.js 與）
2. 加入檢視，以`App/views`資料夾
3. 新增到 viewmodel`App/viewmodels`資料夾
4. 將 HTML 和 Knockout 資料繫結新增至您新的檢視
5. 更新中的瀏覽路由 `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>逐步解說中的 HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml 是開始的路由和檢視的 MVC 應用程式。 它包含所有標準的中繼標記、 css 連結及您所預期的 JavaScript 參考。 本文包含單一`<div>`這是所有內容 （檢視） 將放置的位置要求時。 `@Scripts.Render`用以執行應用程式的程式碼中，包含此功能的進入點 Require.js`main.js`檔案。 示範如何建立啟動顯示畫面，您的應用程式載入時提供啟動顯示畫面。

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

`main.js`檔案包含已載入您的應用程式時，立即將執行的程式碼。 這是您想要用來定義您的瀏覽路由、 設定檢視，您啟動及執行任何安裝/啟動程序等預備您的應用程式資料。

`main.js`檔案會定義數個 durandal 的模組，以幫助啟動應用程式執行。 定義陳述式可協助解決模組相依性，讓它們可供函式。 第一次會啟用偵錯訊息，其中傳送應用程式的主控台視窗中執行何種事件的相關訊息。 App.start 程式碼會告訴 durandal 架構，來啟動應用程式。 使 durandal 知道所有檢視和 viewmodels 會分別包含在相同的已命名資料夾中，設定慣例。 最後，`app.setRoot`一開始會載入`shell`使用預先定義`entrance`動畫。

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>檢視

檢視位於`App/views`資料夾。

### <a name="shellhtml"></a>shell.html

`shell.html`包含 HTML 的主要配置。 所有其他程式檢視端將某處包含您`shell`檢視。 最忙碌的毛巾提供`shell`與三個這類區域： 標頭、 內容區域和頁尾。 每一個地區已載入內容形成要求時的其他檢視。

`compose`頁首和頁尾的繫結是硬式編碼中指向的熱毛巾`nav`和`footer`分別檢視。 Compose 繫結區段`#content`繫結至`router`模組的作用中的項目。 換句話說，當您按一下 瀏覽連結，它是對應的檢視會載入此區域中。

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html`包含為了讓 SPA 的導覽連結。 這是功能表結構可放置的位置，例如。 通常這是資料繫結 （使用 Knockout） 為了`router`模組，以顯示在中所定義的導覽`shell.js`。 油墨廓清會尋找資料繫結屬性，並將它們加入繫結`shell`viewmodel 顯示瀏覽路線，並顯示進度列 （使用 Twitter Bootstrap） 如果`router`模組正在進行瀏覽不同的檢視，另一個 （請參閱`router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>home.html 和 details.html

這些檢視包含自訂檢視的 HTML。 當`home`連結`nav`按一下檢視的功能表時，`home`檢視將會放置在內容區域的`shell`檢視。 這些檢視可以增強或取代您自己的自訂檢視。

### <a name="footerhtml"></a>footer.html

`footer.html`包含出現在頁尾中，在底部的 HTML`shell`檢視。

## <a name="viewmodels"></a>ViewModels

Viewmodel 位於`App/viewmodels`資料夾。

### <a name="shelljs"></a>shell.js

`shell` Viewmodel 包含屬性和繫結至的函式`shell`檢視。 通常這是位於功能表瀏覽繫結 (請參閱`router.mapNav`邏輯)。

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>home.js 並 details.js

這些 viewmodel 包含的屬性和繫結至的函式`home`檢視。 它也包含展示邏輯檢視，並且資料和檢視之間的連結。

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>服務

服務應用程式/服務資料夾中找到。 在理想情況下您未來的服務，例如 dataservice 模組，也就是負責取得和張貼遠端資料，無法放置。

### <a name="loggerjs"></a>logger.js

最忙碌的毛巾提供`logger`模組服務 資料夾中的。 `logger`模組適合用來記錄訊息至主控台，以及在快顯通知快顯中的使用者。
