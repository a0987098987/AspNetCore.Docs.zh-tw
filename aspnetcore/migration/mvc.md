---
title: 從 ASP.NET MVC 移轉至 ASP.NET Core MVC
author: ardalis
description: 了解如何將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC 開始使用。
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a85b9f15be8ad9ca66b20ef1f4422fe67806a797
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468536"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>從 ASP.NET MVC 移轉至 ASP.NET Core MVC

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Daniel Roth](https://github.com/danroth27)， [Steve Smith](https://ardalis.com/)，和[Scott Addie](https://scottaddie.com)

本文說明如何開始移轉至 ASP.NET MVC 專案[ASP.NET Core MVC](../mvc/overview.md)。 在過程中，它會反白顯示已從 ASP.NET MVC 的事項。 從 ASP.NET MVC 移轉為多個步驟的程序，這篇文章涵蓋初始設定，基本的控制器和檢視、 靜態內容，與用戶端相依性。 其他文章涵蓋移轉設定和許多的 ASP.NET MVC 專案中找到的身分識別程式碼。

> [!NOTE]
> 在範例中的版本號碼可能不是最新資訊。 您可能需要據此更新您的專案。

## <a name="create-the-starter-aspnet-mvc-project"></a>建立 ASP.NET MVC 專案的入門

若要示範升級，我們先建立 ASP.NET MVC 應用程式。 建立具有名稱*WebApp1*使命名空間符合我們在下一個步驟中建立 ASP.NET Core 專案。

![Visual Studio 新增專案對話方塊](mvc/_static/new-project.png)

![新的 Web 應用程式 對話方塊中：ASP.NET 範本 面板中選取的 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

*選擇性：* 從方案的名稱變更*WebApp1*要*Mvc5*。 Visual Studio 會顯示新的方案名稱 (*Mvc5*)，讓您更方便區分此專案從下一步 的專案。

## <a name="create-the-aspnet-core-project"></a>建立 ASP.NET Core 專案

建立新*空*與前一個專案同名的 ASP.NET Core web 應用程式 (*WebApp1*) 使兩個專案中的命名空間相符。 具有相同的命名空間可讓您更輕鬆地複製兩個專案之間的程式碼。 您必須在不同於使用相同名稱的前一個專案目錄中建立此專案。

![[新增專案] 對話](mvc/_static/new_core.png)

![新的 ASP.NET Web 應用程式 對話方塊中：ASP.NET Core 範本 面板中選取的空白專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *選擇性：* 建立新的 ASP.NET Core 應用程式使用*Web 應用程式*專案範本。 將專案命名為*WebApp1*，然後選取驗證選項**個別使用者帳戶**。 重新命名此應用程式*FullAspNetCore*。 轉換中建立此專案可節省時間。 您可以查看範本產生的程式碼以查看最後的結果，或將程式碼複製到轉換專案。 它也很有用時停滯的轉換步驟，以比較與範本產生專案。

## <a name="configure-the-site-to-use-mvc"></a>設定站台使用 MVC

::: moniker range=">= aspnetcore-2.1"

* 當以.NET Core 為目標[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)預設參考。 此套件會包含封裝常用套件的 MVC 應用程式。 如果目標.NET Framework，套件參考都必須列出個別專案檔中。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* 當以.NET Core 為目標[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)預設參考。 此套件會包含封裝常用套件的 MVC 應用程式。 如果目標.NET Framework，套件參考都必須列出個別專案檔中。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* 當目標為.NET Core 或.NET Framework，常用套件的 MVC 應用程式列出封裝個別專案檔中。

::: moniker-end

`Microsoft.AspNetCore.Mvc` 是 ASP.NET Core MVC 架構。 `Microsoft.AspNetCore.StaticFiles` 是靜態檔案處理常式。 ASP.NET Core 執行階段已模組化的您必須明確選擇中提供靜態檔案 (請參閱[靜態檔案](xref:fundamentals/static-files))。

* 開啟*Startup.cs*檔案，並變更程式碼以符合下列各項：

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles`延伸模組方法會將靜態檔案處理常式。 如先前所述，ASP.NET 執行階段模組化，而且您必須明確地選擇要提供靜態檔案。 `UseMvc`擴充方法新增路由。 如需詳細資訊，請參閱 <<c0> [ 應用程式啟動](xref:fundamentals/startup)並[路由](xref:fundamentals/routing)。

## <a name="add-a-controller-and-view"></a>新增控制器和檢視

在本節中，您將新增的最小的控制器和檢視，以做為預留位置的 ASP.NET MVC 控制器和檢視下一節中，您會移轉。

* 新增*控制器*資料夾。

* 新增**控制器類別**名為*HomeController.cs*來*控制器*資料夾。

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* 新增*檢視*資料夾。

* 新增*Views/Home*資料夾。

* 新增**Razor 檢視**名為*Index.cshtml*來*Views/Home*資料夾。

![[新增項目] 對話方塊](mvc/_static/view.png)

專案結構如下所示：

![顯示檔案和資料夾的 WebApp1 的方案總管](mvc/_static/project-structure-controller-view.png)

內容取代*Views/Home/Index.cshtml*以下列檔案：

```html
<h1>Hello world!</h1>
```

執行應用程式。

![在 Microsoft Edge 中開啟的 web 應用程式](mvc/_static/hello-world.png)

請參閱[控制器](xref:mvc/controllers/actions)並[檢視](xref:mvc/views/overview)如需詳細資訊。

既然我們已最小的使用 ASP.NET Core 專案，我們可以開始移轉功能從 ASP.NET MVC 專案。 我們需要將下列：

* 用戶端內容 （CSS、 字型和指令碼）

* 控制器

* 檢視

* 模型

* 統合

* 篩選條件

* 輸入/輸出的記錄檔、 身分識別 （這會在下一個教學課程中完成）。

## <a name="controllers-and-views"></a>控制器和檢視

* 複製每一個方法從 ASP.NET MVC`HomeController`新`HomeController`。 請注意，在 ASP.NET MVC 中內建範本的控制器動作方法傳回型別[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); 在 ASP.NET Core MVC 動作方法會傳回`IActionResult`改。 `ActionResult` 實作`IActionResult`，這樣就不需要變更您的動作方法的傳回型別。

* 複製*About.cshtml*， *Contact.cshtml*，並*Index.cshtml*從 ASP.NET MVC 專案 」 至 ASP.NET Core 專案中的 Razor 檢視檔案。

* 執行 ASP.NET Core 應用程式並測試每個方法。 我們還沒有樣式的配置檔案或尚未移轉，讓呈現的檢視只會包含在檢視檔案內容。 您不需要的版面配置檔案產生連結`About`並`Contact`檢視，因此您必須從瀏覽器叫用它們 (取代**4492**專案中使用的連接埠號碼)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

請注意缺乏樣式和功能表項目。 我們將在下節修正該問題。

## <a name="static-content"></a>靜態內容

在舊版的 ASP.NET MVC 中，靜態內容裝載的 web 專案的根目錄，並已混合使用伺服器端檔案。 ASP.NET Core 中的靜態內容裝載於*wwwroot*資料夾。 您會想要複製您舊的 ASP.NET MVC 應用程式，以從靜態內容*wwwroot* ASP.NET Core 專案中的資料夾。 在此範例轉換：

* 複製 */favicon.ico*至舊的 MVC 專案中的檔案*wwwroot* ASP.NET Core 專案中的資料夾。

舊的 ASP.NET MVC 專案會使用[Bootstrap](https://getbootstrap.com/)啟動安裝程式中的檔案及其樣式和存放區*內容*並*指令碼*資料夾。 範本，用來產生舊的 ASP.NET MVC 專案，參考在配置檔案中的啟動程序 (*Views/Shared/_Layout.cshtml*)。 您可以複製*bootstrap.js*並*bootstrap.css*檔案從 ASP.NET MVC 專案加入*wwwroot*在新的專案資料夾。 相反地，我們將新增的啟動程序支援 （和其他用戶端程式庫） 使用 Cdn 下, 一節。

## <a name="migrate-the-layout-file"></a>移轉的配置檔案

* 複製 *_ViewStart.cshtml*舊的 ASP.NET MVC 專案中的檔案*檢視*到 ASP.NET Core 專案的資料夾*檢視*資料夾。 *_ViewStart.cshtml*尚未變更的檔案，這是 ASP.NET Core MVC 中。

* 建立*Views/Shared*資料夾。

* *選擇性：* 複製 *_ViewImports.cshtml*從*FullAspNetCore* MVC 專案*檢視*到 ASP.NET Core 專案的資料夾*檢視*資料夾。 中的任何命名空間宣告中移除 *_ViewImports.cshtml*檔案。 *_ViewImports.cshtml*檔案提供的所有檢視檔案的命名空間和帶入[標籤協助程式](xref:mvc/views/tag-helpers/intro)。 標籤協助程式會在新的版面配置檔案。 *_ViewImports.cshtml*檔案是 ASP.NET Core 的新功能。

* 複製 *_Layout.cshtml*舊的 ASP.NET MVC 專案中的檔案*Views/Shared*到 ASP.NET Core 專案的資料夾*Views/Shared*資料夾。

開啟 *_Layout.cshtml*檔案，並進行下列變更 （已完成的程式碼如下所示）：

* 取代`@Styles.Render("~/Content/css")`具有`<link>`載入的項目*bootstrap.css* （如下所示）。

* 移除 `@Scripts.Render("~/bundles/modernizr")`。

* 標記為註解`@Html.Partial("_LoginPartial")`列 (括住的那行`@*...*@`)。 如需詳細資訊，請參閱[移轉驗證和身分識別至 ASP.NET Core](xref:migration/identity)

* 取代`@Scripts.Render("~/bundles/jquery")`與`<script>`項目 （如下所示）。

* 取代`@Scripts.Render("~/bundles/bootstrap")`與`<script>`項目 （如下所示）。

Bootstrap CSS 包含取代標記：

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

JQuery 和 Bootstrap JavaScript 包含的取代標記：

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

已更新 *_Layout.cshtml*檔案如下所示：

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

在瀏覽器中檢視站台。 它應現在可正確載入，以就地的預期樣式。

* *選擇性：* 您可能會想嘗試使用新的版面配置檔案。 此專案的版面配置檔案可以複製*FullAspNetCore*專案。 新的版面配置檔會使用[標籤協助程式](xref:mvc/views/tag-helpers/intro)還有其他改進功能。

## <a name="configure-bundling-and-minification"></a>設定統合和縮製

如需如何設定統合和縮製的詳細資訊，請參閱[統合和縮製](../client-side/bundling-and-minification.md)。

## <a name="solve-http-500-errors"></a>解決 HTTP 500 錯誤

有許多問題，可能會造成 HTTP 500 錯誤訊息包含問題的來源上的任何資訊。 例如，如果*views/_viewimports.cshtml*檔案包含在您的專案不存在的命名空間，您會收到 HTTP 500 錯誤。 根據預設，ASP.NET Core 應用程式，在`UseDeveloperExceptionPage`延伸模組新增至`IApplicationBuilder`並執行設定時*開發*。 下列程式碼中有詳細說明：

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core 會將 web 應用程式中的未處理例外狀況轉換成 HTTP 500 錯誤回應。 一般來說，這些回應以避免洩露可能含有機密伺服器的相關資訊不包含錯誤詳細資料。 請參閱**使用開發人員例外狀況頁面**中[處理錯誤](../fundamentals/error-handling.md)如需詳細資訊。

## <a name="additional-resources"></a>其他資源

* <xref:razor-components/index>
* <xref:mvc/views/tag-helpers/intro>
