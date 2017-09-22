---
title: "區域"
author: rick-anderson
description: "示範如何使用區域。"
keywords: "ASP.NET Core，區域、 路由、 檢視表"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 0f388ba090ada11a0ac7937606cbcd5a89d6263e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="areas"></a>區域

由[Dhananjay Kumar](https://twitter.com/debug_mode)和[Rick Anderson](https://twitter.com/RickAndMSFT)

區域是用來組織到群組的相關的功能，為不同的命名空間 （適用於路由） 和資料夾結構 （如檢視） 的 ASP.NET MVC 功能。 使用區域建立階層，以新增另一個路由參數，路由`area`至`controller`和`action`。

區域則提供資料的大型 ASP.NET Core MVC Web 應用程式分割成較小功能群組的方式。 一個區域就是應用程式中的一個 MVC 結構。 在 MVC 專案中，邏輯元件，例如模型、 控制器和檢視保留在不同的資料夾，而且若要建立這些元件之間的關聯性則 MVC 會使用命名慣例。 大型應用程式中，它可能比較有利分割成個別高功能層級區域的 應用程式。 例如，電子商務應用程式與多個業務單位，例如簽出、 帳單和搜尋等。每個這些單位具有自己的邏輯元件檢視、 控制器、 和模型。 在此案例中，您可以使用實體分割的相同專案中的商務元件的區域。

區域可以定義為較小的功能單位 ASP.NET Core MVC 專案中使用它自己組的控制站、 檢視和模型。

請考慮使用區域中的 MVC 專案時：

* 您的應用程式是由多個高層級的功能性元件應該以邏輯方式分隔

* 您想要分割您的 MVC 專案，使每個功能區域即可獨立

區域的功能：

* ASP.NET Core MVC 應用程式可以有任意數目的區域

* 每個區域有它自己的控制器、 模型和檢視

* 可讓您將大型 MVC 專案組織成多個可以獨立處理的高階元件

* 支援具有相同名稱的多個控制站，只要它們具有不同*區域*

讓我們看一下範例來說明如何建立和使用的區域。 假設您有兩個不同的群組，控制器和檢視表的市集應用程式： 產品和服務。 一般資料夾結構，使用 MVC 區域看起來如下：

* 專案名稱

  * 區域

    * 產品

      * 控制站

        * HomeController.cs

        * ManageController.cs

      * 檢視

        * 首頁

          * Index.cshtml

        * 管理

          * Index.cshtml

    * 服務

      * 控制站

        * HomeController.cs

      * 檢視

        * 首頁

          * Index.cshtml

當 MVC 嘗試呈現在區域中，檢視依預設時，它會嘗試尋找在下列位置：

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

這些是可以透過變更預設位置`AreaViewLocationFormats`上`Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`。

例如，在下列程式碼，而不需要的資料夾名稱為' '，它已變更為 '類別'。

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

請注意一件事是，結構*檢視*資料夾就會被視為重要此處，而其餘資料夾的內容要*控制器*和*模型*沒有**不**重要。 例如，您不需要*控制器*和*模型*所有資料夾。 因為內容*控制器*和*模型*是只是程式碼取得編譯成 dll 的內容為*檢視*不要求，直到已進行檢視。

一旦您定義了資料夾階層，您必須告訴 MVC 每個控制站是與區域相關聯。 您執行此作業，而將控制器名稱與`[Area]`屬性。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4]}} -->

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

設定路由定義可搭配您新建立的區域。 [路由至控制器動作](routing.md)文章進入詳細資料，以建立路由定義，包括使用傳統與屬性路由的路由。 在此範例中，我們會使用傳統的路由。 若要這樣做，請開啟*Startup.cs*檔案，並修改加`areaRoute`名為路由定義下方。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 6]}} -->

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(name: "areaRoute",
       template: "{area:exists}/{controller=Home}/{action=Index}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

瀏覽至`http://<yourApp>/products`、`Index`動作方法的`HomeController`中`Products`區域將會叫用。

## <a name="link-generation"></a>連結產生

* 產生連結從某個區域中的動作，以控制站相同的控制器內的另一個動作。

  假設目前的要求路徑就像是`/Products/Home/Create`

  HtmlHelper 語法：`@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper 語法：`<a asp-action="Index">Go to Product's Home Page</a>`

  請注意，我們不需要提供 '區域' 和 '控制器 」 值此處，因為它們已在目前要求的內容中使用。 這些類型的值稱為`ambient`值。

* 產生連結從某個區域中的動作基礎控制站，以在不同的控制站上的另一個動作

  假設目前的要求路徑就像是`/Products/Home/Create`

  HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`

  TagHelper 語法：`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  請注意，此處會使用 '區域' 環境值上明確地指定控制器 」 值。

* 產生連結從某個區域中的動作為基礎控制站，另一個動作以不同的控制器和不同的區域。

  假設目前的要求路徑就像是`/Products/Home/Create`

  HtmlHelper 語法：`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper 語法：`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`

  請注意，此處使用任何環境的值。

* 從區域控制器內的動作產生連結至不同的控制站上的另一個動作和**不**區域中。

  HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`

  TagHelper 語法：`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  因為我們想要產生非區域的連結以控制器的動作，我們空白 '區域' 在此環境的值。

## <a name="publishing-areas"></a>發行的區域

所有`*.cshtml`和`wwwroot/**`檔案會發行至輸出時`<Project Sdk="Microsoft.NET.Sdk.Web">`隨附於*.csproj*檔案。
