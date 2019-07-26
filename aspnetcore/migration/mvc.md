---
title: 從 ASP.NET MVC 遷移至 ASP.NET Core MVC
author: ardalis
description: 瞭解如何開始將 ASP.NET MVC 專案遷移至 ASP.NET Core MVC。
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: 6c9449fb43960d05db8aa6dcba64d3d830834cdb
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371882"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>從 ASP.NET MVC 遷移至 ASP.NET Core MVC

作者: [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)

本文說明如何開始將 ASP.NET MVC 專案遷移至[ASP.NET CORE mvc](../mvc/overview.md)。 在此過程中, 它強調了許多已從 ASP.NET MVC 變更的專案。 從 ASP.NET MVC 遷移是一個多步驟程式, 本文涵蓋初始設定、基本控制器和視圖、靜態內容和用戶端相依性。 其他文章涵蓋了在許多 ASP.NET MVC 專案中找到的遷移設定和識別程式碼。

> [!NOTE]
> 範例中的版本號碼可能不是最新的。 您可能需要據以更新您的專案。

## <a name="create-the-starter-aspnet-mvc-project"></a>建立 starter ASP.NET MVC 專案

為了示範升級, 我們將從建立 ASP.NET MVC 應用程式開始。 使用名稱*WebApp1*建立它, 讓命名空間符合我們在下一個步驟中建立的 ASP.NET Core 專案。

![Visual Studio 新增專案 對話方塊](mvc/_static/new-project.png)

![[新增 Web 應用程式] 對話方塊:在 [ASP.NET 範本] 面板中選取 MVC 專案範本](mvc/_static/new-project-select-mvc-template.png)

*選擇性*將解決方案的名稱從*WebApp1*變更為*Mvc5*。 Visual Studio 會顯示新的方案名稱 (*Mvc5*), 讓您更輕鬆地從下一個專案告訴此專案。

## <a name="create-the-aspnet-core-project"></a>建立 ASP.NET Core 專案

使用與前一個專案相同的名稱建立新的*空白*ASP.NET Core web 應用程式 (*WebApp1*), 讓這兩個專案中的命名空間相符。 擁有相同的命名空間可讓您更輕鬆地在這兩個專案之間複製程式碼。 您必須在與前一個專案不同的目錄中建立此專案, 以使用相同的名稱。

![[新增專案] 對話](mvc/_static/new_core.png)

![[新增 ASP.NET Web 應用程式] 對話方塊:在 [ASP.NET Core 範本] 面板中選取了空的專案範本](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *選擇性*使用*Web 應用程式*專案範本建立新的 ASP.NET Core 應用程式。 將專案命名為*WebApp1*, 然後選取**個別使用者帳戶**的驗證選項。 將此應用程式重新命名為*FullAspNetCore*。 建立這個專案可讓您省下轉換的時間。 您可以查看範本產生的程式碼, 以查看最終結果, 或將程式碼複製到轉換專案。 當您停滯在轉換步驟, 與範本產生的專案進行比較時, 這也很有説明。

## <a name="configure-the-site-to-use-mvc"></a>將網站設定為使用 MVC

::: moniker range=">= aspnetcore-2.1"

* 以 .NET Core 為目標時, 預設會參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。 此套件包含 MVC 應用程式經常使用的套件。 如果目標 .NET Framework, 則套件參考必須個別列在專案檔中。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* 以 .NET Core 為目標時, 預設會參考[AspNetCore。](xref:fundamentals/metapackage) 此套件包含 MVC 應用程式常用的套件套件。 如果目標 .NET Framework, 則套件參考必須個別列在專案檔中。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* 以 .NET Core 或 .NET Framework 為目標時, MVC 應用程式的套件常用封裝會分別列在專案檔中。

::: moniker-end

`Microsoft.AspNetCore.Mvc`是 ASP.NET Core MVC 架構。 `Microsoft.AspNetCore.StaticFiles`這是靜態檔案處理常式。 ASP.NET Core 執行時間是模組化的, 而且您必須明確加入宣告以提供靜態檔案 (請參閱[靜態](xref:fundamentals/static-files)檔案)。

* 開啟*Startup.cs*檔案, 並變更程式碼以符合下列內容:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles`擴充方法會加入靜態檔案處理常式。 如先前所述, ASP.NET 執行時間是模組化的, 您必須明確加入宣告靜態檔案。 `UseMvc`擴充方法會新增路由。 如需詳細資訊, 請參閱[應用程式啟動](xref:fundamentals/startup)和[路由](xref:fundamentals/routing)。

## <a name="add-a-controller-and-view"></a>新增控制器和視圖

在本節中, 您將新增最少的控制器和視圖, 做為 ASP.NET MVC 控制器的預留位置, 以及您將在下一節中遷移的視圖。

* 新增 [*控制器*] 資料夾。

* 將名為*HomeController.cs*的**控制器類別**新增至 [*控制器*] 資料夾。

![[新增項目] 對話方塊](mvc/_static/add_mvc_ctl.png)

* 加入*Views*資料夾。

* 新增*Views/Home*資料夾。

* 將名為*Index*的**Razor View**新增至*Views/Home*資料夾。

![[新增項目] 對話方塊](mvc/_static/view.png)

專案結構如下所示:

![方案總管顯示 WebApp1 的檔案和資料夾](mvc/_static/project-structure-controller-view.png)

以下列內容取代*Views/Home/Index. cshtml*檔案的內容:

```html
<h1>Hello world!</h1>
```

執行應用程式。

![在 Microsoft Edge 中開啟 Web 應用程式](mvc/_static/hello-world.png)

如需詳細資訊, 請參閱[控制器](xref:mvc/controllers/actions)和[Views](xref:mvc/views/overview) 。

既然我們已有最小的工作 ASP.NET Core 專案, 我們就可以開始從 ASP.NET MVC 專案遷移功能。 我們需要移動下列各項:

* 用戶端內容 (CSS、字型和腳本)

* 控制器

* 檢視

* 模型

* 統合

* 篩選條件

* 登入/登出、身分識別 (這會在下一個教學課程中完成)。

## <a name="controllers-and-views"></a>控制器和視圖

* 將 ASP.NET MVC `HomeController`中的每個方法複製到新`HomeController`的。 請注意, 在 ASP.NET MVC 中, 內建範本的控制器動作方法傳回類型為[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx);在 ASP.NET Core MVC 中, 動作方法`IActionResult`會改為傳回。 `ActionResult`會`IActionResult`執行, 因此不需要變更動作方法的傳回型別。

* 將*About. cshtml*、 *Contact*和*Index. cshtml* Razor view 檔案從 ASP.NET MVC 專案複製到 ASP.NET Core 專案。

* 執行 ASP.NET Core 應用程式, 並測試每個方法。 我們尚未遷移版面配置檔案或樣式, 因此轉譯的視圖只會包含視圖檔案中的內容。 您不會有`About`和`Contact` views 的配置檔案產生連結, 因此您必須從瀏覽器叫用 (以專案中使用的埠號碼取代**4492** )。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡人頁面](mvc/_static/contact-page.png)

請注意, 缺少樣式和功能表項目。 我們將在下節修正該問題。

## <a name="static-content"></a>靜態內容

在舊版的 ASP.NET MVC 中, 靜態內容是由 Web 專案的根目錄所裝載, 並且與伺服器端檔案混合使用。 在 ASP.NET Core 中, 靜態內容會裝載在*wwwroot*資料夾中。 您會想要將靜態內容從舊的 ASP.NET MVC 應用程式複製到 ASP.NET Core 專案中的*wwwroot*資料夾。 在此範例轉換中:

* 將*favicon*檔案從舊的 MVC 專案複製到 ASP.NET Core 專案中的*wwwroot*資料夾。

舊的 ASP.NET MVC 專案會使用[啟動](https://getbootstrap.com/)程式來進行其樣式設定, 並將啟動載入器檔案儲存在*內容*和*腳本*資料夾中。 產生舊 ASP.NET MVC 專案的範本會參考版面配置檔案中的啟動程式 (*Views/Shared/_Layout*)。 您可以將 ASP.NET MVC 專案中的*啟動*載入器和*啟動程式 .css*檔案複製到新專案中的*wwwroot*資料夾。 相反地, 我們會在下一節中使用 Cdn 來新增對啟動程式 (和其他用戶端程式庫) 的支援。

## <a name="migrate-the-layout-file"></a>遷移版面配置檔案

* 將舊 ASP.NET MVC 專案的  *views*  資料夾中的 *_ViewStart*複製到 ASP.NET Core 專案的  *views*  資料夾。 ASP.NET Core MVC 中的 *_ViewStart*不會變更。

* 建立*Views/Shared*資料夾。

* *選擇性*將*FullAspNetCore* MVC 專案 [ *views* ] 資料夾中的 *_ViewImports*複製到 ASP.NET Core 專案的 [ *views* ] 資料夾。 移除 *_ViewImports*中的任何命名空間宣告。 *_ViewImports*會提供所有視圖檔案的命名空間, 並引進[標記](xref:mvc/views/tag-helpers/intro)協助程式。 標籤協助程式會用於新的版面配置檔案。 *_ViewImports*是 ASP.NET Core 的新檔案。

* 將舊 ASP.NET MVC 專案的*views/shared*資料夾中的 *_Layout* , 複製到 ASP.NET Core 專案的*views/shared*資料夾中。

開啟 *_Layout* , 並進行下列變更 (完成的程式碼如下所示):

* 取代`@Styles.Render("~/Content/css")`為要載入*啟動*程式的元素(請參閱下文)。`<link>`

* 移除 `@Scripts.Render("~/bundles/modernizr")`。

* 將`@Html.Partial("_LoginPartial")`行標記為批註 (以`@*...*@`括住行)。 如需詳細資訊, 請參閱[將驗證和身分識別遷移至 ASP.NET Core](xref:migration/identity)

* 取代`@Scripts.Render("~/bundles/jquery")`為元素(請參閱下文)。`<script>`

* 取代`@Scripts.Render("~/bundles/bootstrap")`為元素(請參閱下文)。`<script>`

啟動載入 CSS 的取代標記包含:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

JQuery 和啟動程式 JavaScript 包含的取代標記:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

更新的 *_Layout*檔如下所示:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

在瀏覽器中觀看網站。 現在應該會正確地載入, 且應具備預期的樣式。

* *選擇性*您可能想要嘗試使用新的版面配置檔案。 針對此專案, 您可以從*FullAspNetCore*專案複製版面配置檔案。 新的設定檔案[](xref:mvc/views/tag-helpers/intro)會使用標籤協助程式, 並具有其他改良功能。

## <a name="configure-bundling-and-minification"></a>設定捆綁和縮制

如需有關如何設定配套和縮制的詳細資訊, 請參閱[捆綁和縮制](../client-side/bundling-and-minification.md)。

## <a name="solve-http-500-errors"></a>解決 HTTP 500 錯誤

有許多問題可能會導致 HTTP 500 錯誤訊息, 其中不包含問題來源的相關資訊。 例如, 如果*Views/_ViewImports. cshtml*檔案包含的命名空間不存在於您的專案中, 您將會收到 HTTP 500 錯誤。 根據預設, 在 ASP.NET Core 應用程式`UseDeveloperExceptionPage`中, 延伸模組會`IApplicationBuilder`新增至, 並在設定為*開發*時執行。 下列程式碼將詳細說明這一點:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core 會將 web 應用程式中未處理的例外狀況轉換成 HTTP 500 錯誤回應。 一般來說, 錯誤詳細資料不會包含在這些回應中, 以防止洩漏伺服器的潛在敏感性資訊。 如需詳細資訊, 請參閱在[處理錯誤](../fundamentals/error-handling.md)中**使用開發人員例外狀況頁面**。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
