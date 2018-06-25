---
title: ASP.NET Core 中的區域
author: rick-anderson
description: 了解其為 ASP.NET MVC 功能的區域，如何用來將相關功能組織成群組，作為個別命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: 3e998af42cd6209271495dd8dd97a8aed35717a4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274823"
---
# <a name="areas-in-aspnet-core"></a>ASP.NET Core 中的區域

作者：[Dhananjay Kumar](https://twitter.com/debug_mode) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

區域是 ASP.NET MVC 功能，用來將相關功能組織成群組，作為個別命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。 使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。

區域可讓您將大型 ASP.NET Core MVC Web 應用程式分割成較小的功能群組。 一個區域基本上是應用程式內的一個 MVC 結構。 在 MVC 專案中，模型、控制器和檢視等邏輯元件會保留在不同的資料夾中，而且 MVC 會使用命名慣例來建立這些元件之間的關聯性。 針對大型應用程式，將應用程式分割成個別高階區域可能較有利。 例如，一個電子商務應用程式可分成多個業務單位，例如結帳、計費和搜尋等。所有這些單位都有自己的邏輯元件檢視、控制器和模型。 在此案例中，您可以使用區域來實體分割相同專案中的商務元件。

在 ASP.NET Core MVC 專案中，區域可以定義為較小的功能單位，並且具有一組自己的控制器、檢視和模型。

在下列情況時，請考慮在 MVC 專案中使用區域：

* 您的應用程式是由應該透過邏輯方式區隔的多個高階功能性元件所構成

* 您想要分割 MVC 專案，讓每個功能區域都可以獨立運作

區域功能：

* ASP.NET Core MVC 應用程式可以有任意數目的區域

* 每個區域都有自己的控制器、模型和檢視

* 可讓您將大型 MVC 專案組織成多個可以獨立處理的高階元件

* 支援多個同名的控制器 (只要這些控制器有不同的「區域」即可)

請查看範例來說明如何建立和使用區域。 假設您的市集應用程式具有兩個不同的控制器和檢視分組：產品和服務。 使用 MVC 區域的一般資料夾結構如下：

* 專案名稱

  * 區域

    * 產品

      * Controllers

        * HomeController.cs

        * ManageController.cs

      * 檢視

        * 首頁

          * Index.cshtml

        * 管理

          * Index.cshtml

    * 服務

      * Controllers

        * HomeController.cs

      * 檢視

        * 首頁

          * Index.cshtml

MVC 嘗試轉譯區域中的檢視時，預設會嘗試在下列位置中尋找：

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

這些是 `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` 上可透過 `AreaViewLocationFormats` 變更的預設位置。

例如，在下列程式碼中，它已變更為 'Categories'，而不是讓資料夾名稱成為 'Areas'。

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

有一件事需要注意：*Views* 資料夾結構是這裡唯一視為重要的結構，而其餘資料夾 (例如 *Controllers* 和 *Models*) 的內容並**不**重要。 例如，您根本不需要 *Controllers* 和 *Models* 資料夾。 原因是 *Controllers* 和 *Models* 的內容就只是要編譯為 .dll 的程式碼；但除非已對該檢視提出要求，否則 *Views* 的內容不是程式碼。

定義資料夾階層之後，需要告訴 MVC：每個控制器都會與某個區域建立關聯。 做法是以 `[Area]` 屬性裝飾控制器名稱。

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

設定可與新建立區域搭配使用的路由定義。 [路由至控制器動作](routing.md)一文會詳述如何建立路由定義，包括使用傳統路由與屬性路由的比較。 在此範例中，我們將使用傳統路由。 若要這樣做，請開啟 *Startup.cs* 檔案，然後新增下面的 `areaRoute` 具名路由定義以進行修改。

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

瀏覽至 `http://<yourApp>/products`，以叫用 `Products` 區域中 `HomeController` 的 `Index` 動作方法。

## <a name="link-generation"></a>連結產生

* 產生下列兩者之間的連結：從區域控制器內的某個動作到相同控制器內的另一個動作。

  假設目前的要求路徑如下：`/Products/Home/Create`

  HtmlHelper 語法：`@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper 語法：`<a asp-action="Index">Go to Product's Home Page</a>`

  請注意，我們在這裡不需要提供 'area' 和 'controller' 值，因為目前要求的內容中已經有它們。 這些類型的值稱為 `ambient` 值。

* 產生下列兩者之間的連結：從區域控制器內的某個動作到不同控制器上的另一個動作

  假設目前的要求路徑如下：`/Products/Home/Create`

  HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  TagHelper 語法：`<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  請注意，這裡會使用 'area' 的環境值，但要在上方明確指定 'controller' 值。

* 產生下列兩者之間的連結：從區域控制器內的某個動作到不同控制器和不同區域上的另一個動作。

  假設目前的要求路徑如下：`/Products/Home/Create`

  HtmlHelper 語法：`@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper 語法：`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  請注意，這裡未使用環境值。

* 產生下列兩者之間的連結：從區域控制器內的某個動作到不同控制器上但**不**在區域中的另一個動作。

  HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  TagHelper 語法：`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  因為我們想要產生非區域控制器動作的連結，所以我們在這裡清空 'area' 的環境值。

## <a name="publishing-areas"></a>發行區域

*.csproj* 檔案中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">` 時，所有 `*.cshtml` 和 `wwwroot/**` 檔案都會發行至輸出。
