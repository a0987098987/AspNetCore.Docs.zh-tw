---
title: ASP.NET MVC 從移轉至 ASP.NET Core MVC
author: ardalis
description: 了解如何開始將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC。
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET MVC 從移轉至 ASP.NET Core MVC

由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)

本文將說明如何開始移轉至 ASP.NET MVC 專案[ASP.NET Core MVC](../mvc/overview.md)。 在過程中，它會反白顯示已從 ASP.NET MVC 的事項。 ASP.NET MVC 從移轉是多重步驟的程序，本文章涵蓋初始設定，基本的控制器和檢視、 靜態內容，與用戶端相依性。 其他文章涵蓋移轉設定和許多的 ASP.NET MVC 專案中的身分識別程式碼。

> [!NOTE]
> 樣本中的版本號碼可能不是最新資訊。 您可能要據以更新您的專案。

## <a name="create-the-starter-aspnet-mvc-project"></a>建立入門 ASP.NET MVC 專案

若要示範升級，我們會先建立 ASP.NET MVC 應用程式。 建立具有名稱*WebApp1*使命名空間符合我們在下一個步驟中建立的 ASP.NET Core 專案。

![Visual Studio 新專案 對話方塊](mvc/_static/new-project.png)

![新的 Web 應用程式 對話方塊： ASP.NET 範本 面板中選取的 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

*選擇性：*變更解決方案的名稱*WebApp1*至*Mvc5*。 Visual Studio 會顯示新的方案名稱 (*Mvc5*)，讓您更方便區分此專案，從下一個專案。

## <a name="create-the-aspnet-core-project"></a>建立 ASP.NET Core 專案

建立新*空*ASP.NET Core web 應用程式與先前的專案名稱相同 (*WebApp1*) 使兩個專案中的命名空間相符。 具有相同的命名空間，可以更輕鬆地將程式碼的兩個專案之間複製。 您必須在比先前的專案以使用相同的名稱不同的目錄中建立此專案。

![[新增專案] 對話](mvc/_static/new_core.png)

![新的 ASP.NET Web 應用程式 對話方塊： 選取 ASP.NET Core 範本面板中的空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *選擇性：*建立新的 ASP.NET Core 應用程式使用*Web 應用程式*專案範本。 將專案命名*WebApp1*，以及選取的驗證選項**個別使用者帳戶**。 重新命名此應用程式， *FullAspNetCore*。 轉換中建立此專案節省您的時間。 您可以查看範本產生的程式碼以查看結果，或將程式碼複製到轉換專案。 它也是很有幫助時堵塞在要與範本產生的專案比較轉換步驟。

## <a name="configure-the-site-to-use-mvc"></a>將網站設定為使用 MVC

* 當目標為.NET Core，ASP.NET Core metapackage 會加入至專案，稱為`Microsoft.AspNetCore.All`預設。 此套件包含像是`Microsoft.AspNetCore.Mvc`和`Microsoft.AspNetCore.StaticFiles`。 如果目標.NET Framework，封裝會參照需要個別列出 *.csproj 檔案中。

`Microsoft.AspNetCore.Mvc` 是 ASP.NET Core MVC 架構。 `Microsoft.AspNetCore.StaticFiles` 是靜態檔案處理常式。 ASP.NET Core 執行階段是一個模組，以及您必須明確地選擇提供靜態檔案 (請參閱[靜態檔案](xref:fundamentals/static-files))。

* 開啟*Startup.cs*檔案，並且變更以符合下列程式碼：

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles`擴充方法新增靜態檔案處理常式。 如先前所述，ASP.NET 執行階段模組化，而且您必須明確地選擇提供靜態檔案。 `UseMvc`擴充方法會將路由。 如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)和[路由](xref:fundamentals/routing)。

## <a name="add-a-controller-and-view"></a>加入控制器和檢視

在本節中，您會加入最少的控制器和檢視，以做為預留位置的 ASP.NET MVC 控制器和檢視表會在下一節中移轉。

* 新增*控制器*資料夾。

* 新增**控制器類別**名為*HomeController.cs*至*控制器*資料夾。

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* 新增*檢視*資料夾。

* 新增*Views/Home*資料夾。

* 新增**Razor 檢視**名為*Index.cshtml*至*Views/Home*資料夾。

![[新增項目] 對話方塊](mvc/_static/view.png)

專案結構如下所示：

![方案總管 中顯示檔案和資料夾的 WebApp1](mvc/_static/project-structure-controller-view.png)

取代內容*Views/Home/Index.cshtml*以下列檔案：

```html
<h1>Hello world!</h1>
```

執行應用程式。

![在 Microsoft Edge 中開啟的 web 應用程式](mvc/_static/hello-world.png)

請參閱[控制器](xref:mvc/controllers/actions)和[檢視](xref:mvc/views/overview)如需詳細資訊。

現在我們有最小的工作 ASP.NET Core 專案時，我們可以開始從 ASP.NET MVC 專案移轉功能。 我們需要移動下列：

* 用戶端內容 （CSS、 字型和指令碼）

* 控制器

* 檢視

* 模型

* 結合在一起

* 篩選條件

* 記錄檔 in/out、 身分識別 （這在下一個教學課程中完成）。

## <a name="controllers-and-views"></a>控制器和檢視

* 每個方法複製 ASP.NET MVC`HomeController`新`HomeController`。 請注意，ASP.NET MVC 中內建的範本控制器動作方法傳回型別是[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET Core MVC 動作方法傳回`IActionResult`改為。 `ActionResult` 實作`IActionResult`，因此不需要變更您的動作方法的傳回型別。

* 複製*About.cshtml*， *Contact.cshtml*，和*Index.cshtml* Razor 檢視檔案，從 ASP.NET MVC 專案加入 ASP.NET Core 專案。

* 執行 ASP.NET Core 應用程式和測試每一種方法。 我們尚未樣式的配置檔案尚未移轉，因此只能包含呈現的檢視的檢視檔案中的內容。 您不需要的配置檔案產生連結`About`和`Contact`檢視時，所以您必須先從瀏覽器叫用 (取代**4492**專案中使用的連接埠號碼)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

請注意缺乏樣式和功能表項目。 我們將在下節修正該問題。

## <a name="static-content"></a>靜態內容

在舊版 ASP.NET MVC 中，靜態內容裝載 web 專案的根目錄和伺服器端檔案已混合。 在 ASP.NET Core 靜態內容裝載於*wwwroot*資料夾。 您要複製舊 ASP.NET MVC 應用程式中的靜態內容*wwwroot* ASP.NET Core 專案資料夾中的。 在這個範例轉換：

* 複製*favicon.ico*檔案從舊的 MVC 專案至*wwwroot* ASP.NET Core 專案資料夾中的。

舊的 ASP.NET MVC 專案會使用[Bootstrap](https://getbootstrap.com/)啟動安裝程式中的檔案其設定樣式和存放區的*內容*和*指令碼*資料夾。 產生舊的 ASP.NET MVC 專案範本會參考配置檔案中的啟動程序 (*Views/Shared/_Layout.cshtml*)。 您無法複製*bootstrap.js*和*bootstrap.css*檔案從 ASP.NET MVC 專案加入*wwwroot*新專案中的資料夾。 相反地，我們會將新增的啟動程序支援 （和其他用戶端程式庫） 使用 Cdn 下一節。

## <a name="migrate-the-layout-file"></a>移轉配置檔案

* 複製 *_viewstart.vbhtml*檔案從舊的 ASP.NET MVC 專案*檢視*ASP.NET Core 專案的資料夾*檢視*資料夾。 *_Viewstart.vbhtml* ASP.NET Core MVC 中未變更檔案。

* 建立*Views/Shared*資料夾。

* *選擇性：*複製 *_ViewImports.cshtml*從*FullAspNetCore* MVC 專案*檢視*ASP.NET Core 專案的資料夾*檢視*資料夾。 中的任何命名空間宣告中移除 *_ViewImports.cshtml*檔案。 *_ViewImports.cshtml*檔案命名空間提供檢視的所有檔案，並使[標記協助程式](xref:mvc/views/tag-helpers/intro)。 新的版面配置檔中使用標記協助程式。 *_ViewImports.cshtml*檔案是新的 ASP.NET core。

* 複製 *_Layout.cshtml*檔案從舊的 ASP.NET MVC 專案*Views/Shared* ASP.NET Core 專案的資料夾*Views/Shared*資料夾。

開啟 *_Layout.cshtml*檔案，然後進行下列變更 （已完成的程式碼如下所示）：

* 取代`@Styles.Render("~/Content/css")`與`<link>`載入的項目*bootstrap.css* （請參閱下文）。

* 移除 `@Scripts.Render("~/bundles/modernizr")`。

* 標記為註解`@Html.Partial("_LoginPartial")`列 (周圍的線條`@*...*@`)。 我們將在未來的教學課程中傳回它。

* 取代`@Scripts.Render("~/bundles/jquery")`與`<script>`項目 （請參閱下文）。

* 取代`@Scripts.Render("~/bundles/bootstrap")`與`<script>`項目 （請參閱下文）。

包含啟動載入器的 CSS 取代標記：

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

JQuery 和啟動程序 JavaScript 內含的取代標記：

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

已更新 *_Layout.cshtml*檔案如下所示：

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

在瀏覽器中檢視站台。 它應該現在正確載入，以就地預期的樣式。

* *選擇性：*要再試一次使用新的版面配置檔案。 這個專案的版面配置檔案可以複製*FullAspNetCore*專案。 新的版面配置檔案使用[標記協助程式](xref:mvc/views/tag-helpers/intro)且具有其他改進功能。

## <a name="configure-bundling-and-minification"></a>統合及縮製的設定

如需如何設定統合及縮製的資訊，請參閱[組合和縮製](../client-side/bundling-and-minification.md)。

## <a name="solve-http-500-errors"></a>解決 HTTP 500 錯誤

有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題的來源上的任何資訊。 例如，如果*Views/_ViewImports.cshtml*檔案包含不存在於您的專案命名空間，您會收到 HTTP 500 錯誤。 根據預設，ASP.NET Core 應用程式，在`UseDeveloperExceptionPage`延伸會加入至`IApplicationBuilder`而設定時，執行*開發*。 下列程式碼中有詳細說明：

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core 會將 web 應用程式中的未處理例外狀況轉換成 HTTP 500 錯誤回應。 一般來說，若要防止潛在的敏感資訊伺服器公開這些回應不包含錯誤詳細資料。 請參閱**使用開發人員例外狀況頁面**中[處理錯誤](../fundamentals/error-handling.md)如需詳細資訊。

## <a name="additional-resources"></a>其他資源

* [用戶端開發](xref:client-side/index)
* [標記協助程式](xref:mvc/views/tag-helpers/intro)
