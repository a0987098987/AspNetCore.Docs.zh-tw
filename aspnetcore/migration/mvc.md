---
title: "ASP.NET MVC 從移轉至 ASP.NET Core MVC"
author: ardalis
description: 
keywords: "ASP.NET Core, MVC, 移轉"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 7a4357da4cc97d7c60cc7e309add7583ef096597
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET MVC 從移轉至 ASP.NET Core MVC

由[Rick Anderson](https://twitter.com/RickAndMSFT)，[奧 Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)

本文將說明如何開始移轉至 ASP.NET MVC 專案[ASP.NET Core MVC](../mvc/overview.md)。 在過程中，它會反白顯示已從 ASP.NET MVC 的事項。 ASP.NET MVC 從移轉是多重步驟的程序，本文章涵蓋初始設定，基本的控制器和檢視、 靜態內容，與用戶端相依性。 其他文章涵蓋移轉設定和許多的 ASP.NET MVC 專案中的身分識別程式碼。

> [!NOTE]
> 樣本中的版本號碼可能不是最新資訊。 您可能要據以更新您的專案。

## <a name="create-the-starter-aspnet-mvc-project"></a>建立入門 ASP.NET MVC 專案

若要示範升級，我們會先建立 ASP.NET MVC 應用程式。 建立具有名稱*WebApp1*讓命名空間會比對我們在下一個步驟中建立的 ASP.NET Core 專案。

![Visual Studio 新專案 對話方塊](mvc/_static/new-project.png)

![新的 Web 應用程式 對話方塊： ASP.NET 範本 面板中選取的 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

*選擇性：*變更解決方案的名稱*WebApp1*至*Mvc5*。 Visual Studio 會顯示新的方案名稱 (*Mvc5*)，都將您更方便區分此專案，從下一個專案。

## <a name="create-the-aspnet-core-project"></a>建立 ASP.NET Core 專案

建立新*空*ASP.NET Core web 應用程式與先前的專案名稱相同 (*WebApp1*) 使兩個專案中的命名空間相符。 具有相同的命名空間，可以更輕鬆地將程式碼的兩個專案之間複製。 您必須在比先前的專案以使用相同的名稱不同的目錄中建立此專案。

![[新增專案] 對話](mvc/_static/new_core.png)

![新的 ASP.NET Web 應用程式 對話方塊： 選取 ASP.NET Core 範本面板中的空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *選擇性：*建立新的 ASP.NET Core 應用程式使用*Web 應用程式*專案範本。 將專案命名*WebApp1*，以及選取的驗證選項**個別使用者帳戶**。 重新命名此應用程式， *FullAspNetCore*。 建立您的時間儲存此專案將會在轉換中。 您可以查看範本產生的程式碼以查看結果，或將程式碼複製到轉換專案。 它也是很有幫助時堵塞在要與範本產生的專案比較轉換步驟。

## <a name="configure-the-site-to-use-mvc"></a>將網站設定為使用 MVC

* 安裝`Microsoft.AspNetCore.Mvc`和`Microsoft.AspNetCore.StaticFiles`NuGet 封裝。

  `Microsoft.AspNetCore.Mvc`是 ASP.NET Core MVC 架構。 `Microsoft.AspNetCore.StaticFiles`是靜態檔案處理常式。 ASP.NET 執行階段是一個模組，以及您必須明確地選擇提供靜態檔案 (請參閱[靜態檔案處理](../fundamentals/static-files.md))。

* 開啟*.csproj*檔案 (在專案上按一下滑鼠右鍵**方案總管 中**選取**編輯 WebApp1.csproj**) 並加入`PrepareForPublish`目標：

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  `PrepareForPublish`目標所需的取得透過 Bower 用戶端程式庫。 我們將討論的更新版本。

* 開啟*Startup.cs*檔案，並且變更以符合下列程式碼：

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  `UseStaticFiles`擴充方法新增靜態檔案處理常式。 如先前所述，ASP.NET 執行階段模組化，而且您必須明確地選擇提供靜態檔案。 `UseMvc`擴充方法會將路由。 如需詳細資訊，請參閱[應用程式啟動](../fundamentals/startup.md)和[路由](../fundamentals/routing.md)。

## <a name="add-a-controller-and-view"></a>加入控制器和檢視

在本節中，您會加入最少的控制器和檢視，以做為預留位置的 ASP.NET MVC 控制器和檢視表會在下一節中移轉。

* 新增*控制器*資料夾。

* 新增**MVC 控制器類別**名稱*HomeController.cs*至*控制器*資料夾。

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* 新增*檢視*資料夾。

* 新增*Views/Home*資料夾。

* 新增*Index.cshtml* MVC 檢視頁面，即可*Views/Home*資料夾。

![[新增項目] 對話方塊](mvc/_static/view.png)

專案結構如下所示：

![方案總管 中顯示檔案和資料夾的 WebApp1](mvc/_static/project-structure-controller-view.png)

取代內容*Views/Home/Index.cshtml*以下列檔案：

```html
<h1>Hello world!</h1>
```

執行應用程式。

![在 Microsoft Edge 中開啟的 web 應用程式](mvc/_static/hello-world.png)

請參閱[控制器](../mvc/controllers/index.md)和[檢視](../mvc/views/index.md)如需詳細資訊。

現在我們有最小的工作 ASP.NET Core 專案時，我們可以開始從 ASP.NET MVC 專案移轉功能。 我們需要將下列：

* 用戶端內容 （CSS、 字型和指令碼）

* 控制器

* 檢視

* 模型

* 結合在一起

* 篩選條件

* 記錄檔 in/out、 身分識別 （這會執行下一個教學課程中。）

## <a name="controllers-and-views"></a>控制器和檢視

* 每個方法複製 ASP.NET MVC`HomeController`新`HomeController`。 請注意，ASP.NET MVC 中內建的範本控制器動作方法傳回型別是[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET Core MVC 動作方法傳回`IActionResult`改為。 `ActionResult`實作`IActionResult`，因此不需要變更您的動作方法的傳回型別。

* 複製*About.cshtml*， *Contact.cshtml*，和*Index.cshtml* Razor 檢視檔案，從 ASP.NET MVC 專案加入 ASP.NET Core 專案。

* 執行 ASP.NET Core 應用程式和測試每一種方法。 我們尚未樣式的配置檔案尚未移轉，讓呈現的檢視表只會包含在檢視檔案內容。 您不需要的配置檔案產生連結`About`和`Contact`檢視時，所以您必須先從瀏覽器叫用 (取代**4492**專案中使用的連接埠號碼)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

請注意缺乏樣式和功能表項目。 我們將修正的下一節。

## <a name="static-content"></a>靜態內容

在舊版 ASP.NET MVC 中，靜態內容裝載 web 專案的根目錄和伺服器端檔案已混合。 在 ASP.NET Core 靜態內容裝載於*wwwroot*資料夾。 您要複製舊 ASP.NET MVC 應用程式中的靜態內容*wwwroot* ASP.NET Core 專案資料夾中的。 在這個範例轉換：

* 複製*favicon.ico*檔案從舊的 MVC 專案至*wwwroot* ASP.NET Core 專案資料夾中的。

舊的 ASP.NET MVC 專案會使用[Bootstrap](http://getbootstrap.com/)啟動安裝程式中的檔案其設定樣式和存放區的*內容*和*指令碼*資料夾。 產生舊的 ASP.NET MVC 專案範本會參考配置檔案中的啟動程序 (*Views/Shared/_Layout.cshtml*)。 您無法複製*bootstrap.js*和*bootstrap.css*檔案從 ASP.NET MVC 專案加入*wwwroot*不會使用新的專案，而該方法中的資料夾改良的機制讓您管理 ASP.NET Core 中的用戶端相依性。

在新的專案中，我們會將啟動程序 （及其他用戶端程式庫） 使用支援新增[Bower](https://bower.io/):

* 新增[Bower](https://bower.io/)名為組態檔*bower.json*專案根目錄 (以滑鼠右鍵按一下專案，然後**新增 > 新的項目 > Bower 的組態檔**)。 新增[Bootstrap](http://getbootstrap.com/)和[jQuery](https://jquery.com/)檔案 （請參閱下列反白顯示的程式行）。

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

在儲存檔案，Bower 將會自動下載相依性， *wwwroot/lib*資料夾。 您可以使用**搜尋方案總管**方塊，即可尋找資產的路徑：

![方案總管 中搜尋結果中顯示的 jquery 資產](mvc/_static/search.png)

請參閱[Bower 與管理用戶端封裝](../client-side/bower.md)如需詳細資訊。

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>移轉配置檔案

* 複製*_viewstart.vbhtml*檔案從舊的 ASP.NET MVC 專案*檢視*ASP.NET Core 專案的資料夾*檢視*資料夾。 *_Viewstart.vbhtml* ASP.NET Core MVC 中未變更檔案。

* 建立*Views/Shared*資料夾。

* *選擇性：*複製*_ViewImports.cshtml*從*FullAspNetCore* MVC 專案*檢視*資料夾到 ASP.NET Core 專案*檢視*資料夾。 中的任何命名空間宣告中移除*_ViewImports.cshtml*檔案。 *_ViewImports.cshtml*檔案命名空間提供檢視的所有檔案，並使[標記協助程式](../mvc/views/tag-helpers/index.md)。 新的版面配置檔中使用標記協助程式。 *_ViewImports.cshtml*檔案是新的 ASP.NET core。

* 複製*_Layout.cshtml*檔案從舊的 ASP.NET MVC 專案*Views/Shared* ASP.NET Core 專案的資料夾*Views/Shared*資料夾。

開啟*_Layout.cshtml*檔案，然後進行下列變更 （已完成的程式碼如下所示）：

   * 取代`@Styles.Render("~/Content/css")`與`<link>`載入的項目*bootstrap.css* （請參閱下文）。

   * 移除 `@Scripts.Render("~/bundles/modernizr")`。

   * 標記為註解`@Html.Partial("_LoginPartial")`列 (周圍的線條`@*...*@`)。 我們將在未來的教學課程中傳回它。

   * 取代`@Scripts.Render("~/bundles/jquery")`與`<script>`項目 （請參閱下文）。

   * 取代`@Scripts.Render("~/bundles/bootstrap")`與`<script>`項目 （請參閱下文）...

取代 CSS 連結：

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

取代指令碼標記中：

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

已更新*_Layout.cshtml*檔案如下所示：

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

在瀏覽器中檢視站台。 它應該現在正確載入，以就地預期的樣式。

* *選擇性：*要再試一次使用新的版面配置檔案。 這個專案的版面配置檔案可以複製*FullAspNetCore*專案。 新的版面配置檔案使用[標記協助程式](../mvc/views/tag-helpers/index.md)且具有其他改進功能。

## <a name="configure-bundling--minification"></a>設定結合在一起 （& s) 縮製

如需如何設定統合及縮製的資訊，請參閱[組合和縮製](../client-side/bundling-and-minification.md)。

## <a name="solving-http-500-errors"></a>解決 HTTP 500 錯誤

有許多問題可能會導致 HTTP 500 錯誤訊息，其中不包含問題的來源上的任何資訊。 例如，如果*Views/_ViewImports.cshtml*檔案包含不存在於您的專案命名空間，您會收到 HTTP 500 錯誤。 若要取得詳細的錯誤訊息，請加入下列程式碼：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

請參閱**使用開發人員例外狀況頁面**中[錯誤處理](../fundamentals/error-handling.md)如需詳細資訊。

## <a name="additional-resources"></a>其他資源

* [用戶端開發](../client-side/index.md)

* [標記協助程式](../mvc/views/tag-helpers/index.md)
