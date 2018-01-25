---
title: "區域"
author: rick-anderson
description: "示範如何使用區域。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 87bf2eaad1c13d21412051be769992411f685e2e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="areas"></a><span data-ttu-id="647c1-103">區域</span><span class="sxs-lookup"><span data-stu-id="647c1-103">Areas</span></span>

<span data-ttu-id="647c1-104">由[Dhananjay Kumar](https://twitter.com/debug_mode)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="647c1-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="647c1-105">區域是用來組織到群組的相關的功能，為不同的命名空間 （適用於路由） 和資料夾結構 （如檢視） 的 ASP.NET MVC 功能。</span><span class="sxs-lookup"><span data-stu-id="647c1-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="647c1-106">使用區域建立階層，以新增另一個路由參數，路由`area`至`controller`和`action`。</span><span class="sxs-lookup"><span data-stu-id="647c1-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="647c1-107">區域則提供資料的大型 ASP.NET Core MVC Web 應用程式分割成較小功能群組的方式。</span><span class="sxs-lookup"><span data-stu-id="647c1-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="647c1-108">一個區域就是應用程式中的一個 MVC 結構。</span><span class="sxs-lookup"><span data-stu-id="647c1-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="647c1-109">在 MVC 專案中，邏輯元件，例如模型、 控制器和檢視保留在不同的資料夾，而且若要建立這些元件之間的關聯性則 MVC 會使用命名慣例。</span><span class="sxs-lookup"><span data-stu-id="647c1-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="647c1-110">大型應用程式中，它可能比較有利分割成個別高功能層級區域的 應用程式。</span><span class="sxs-lookup"><span data-stu-id="647c1-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="647c1-111">例如，電子商務應用程式與多個業務單位，例如簽出、 帳單和搜尋等。每個這些單位具有自己的邏輯元件檢視、 控制器、 和模型。</span><span class="sxs-lookup"><span data-stu-id="647c1-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="647c1-112">在此案例中，您可以使用實體分割的相同專案中的商務元件的區域。</span><span class="sxs-lookup"><span data-stu-id="647c1-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="647c1-113">區域可以定義為較小的功能單位 ASP.NET Core MVC 專案中使用它自己組的控制站、 檢視和模型。</span><span class="sxs-lookup"><span data-stu-id="647c1-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="647c1-114">請考慮使用區域中的 MVC 專案時：</span><span class="sxs-lookup"><span data-stu-id="647c1-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="647c1-115">您的應用程式是由多個高層級的功能性元件應該以邏輯方式分隔</span><span class="sxs-lookup"><span data-stu-id="647c1-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="647c1-116">您想要分割您的 MVC 專案，使每個功能區域即可獨立</span><span class="sxs-lookup"><span data-stu-id="647c1-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="647c1-117">區域的功能：</span><span class="sxs-lookup"><span data-stu-id="647c1-117">Area features:</span></span>

* <span data-ttu-id="647c1-118">ASP.NET Core MVC 應用程式可以有任意數目的區域</span><span class="sxs-lookup"><span data-stu-id="647c1-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="647c1-119">每個區域有它自己的控制器、 模型和檢視</span><span class="sxs-lookup"><span data-stu-id="647c1-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="647c1-120">可讓您將大型 MVC 專案組織成多個可以獨立處理的高階元件</span><span class="sxs-lookup"><span data-stu-id="647c1-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="647c1-121">支援具有相同名稱的多個控制站，只要它們具有不同*區域*</span><span class="sxs-lookup"><span data-stu-id="647c1-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="647c1-122">讓我們看一下範例來說明如何建立和使用的區域。</span><span class="sxs-lookup"><span data-stu-id="647c1-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="647c1-123">假設您有兩個不同的群組，控制器和檢視表的市集應用程式： 產品和服務。</span><span class="sxs-lookup"><span data-stu-id="647c1-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="647c1-124">一般資料夾結構，使用 MVC 區域看起來如下：</span><span class="sxs-lookup"><span data-stu-id="647c1-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="647c1-125">專案名稱</span><span class="sxs-lookup"><span data-stu-id="647c1-125">Project name</span></span>

  * <span data-ttu-id="647c1-126">區域</span><span class="sxs-lookup"><span data-stu-id="647c1-126">Areas</span></span>

    * <span data-ttu-id="647c1-127">產品</span><span class="sxs-lookup"><span data-stu-id="647c1-127">Products</span></span>

      * <span data-ttu-id="647c1-128">控制站</span><span class="sxs-lookup"><span data-stu-id="647c1-128">Controllers</span></span>

        * <span data-ttu-id="647c1-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="647c1-129">HomeController.cs</span></span>

        * <span data-ttu-id="647c1-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="647c1-130">ManageController.cs</span></span>

      * <span data-ttu-id="647c1-131">檢視</span><span class="sxs-lookup"><span data-stu-id="647c1-131">Views</span></span>

        * <span data-ttu-id="647c1-132">首頁</span><span class="sxs-lookup"><span data-stu-id="647c1-132">Home</span></span>

          * <span data-ttu-id="647c1-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="647c1-133">Index.cshtml</span></span>

        * <span data-ttu-id="647c1-134">管理</span><span class="sxs-lookup"><span data-stu-id="647c1-134">Manage</span></span>

          * <span data-ttu-id="647c1-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="647c1-135">Index.cshtml</span></span>

    * <span data-ttu-id="647c1-136">服務</span><span class="sxs-lookup"><span data-stu-id="647c1-136">Services</span></span>

      * <span data-ttu-id="647c1-137">控制站</span><span class="sxs-lookup"><span data-stu-id="647c1-137">Controllers</span></span>

        * <span data-ttu-id="647c1-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="647c1-138">HomeController.cs</span></span>

      * <span data-ttu-id="647c1-139">檢視</span><span class="sxs-lookup"><span data-stu-id="647c1-139">Views</span></span>

        * <span data-ttu-id="647c1-140">首頁</span><span class="sxs-lookup"><span data-stu-id="647c1-140">Home</span></span>

          * <span data-ttu-id="647c1-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="647c1-141">Index.cshtml</span></span>

<span data-ttu-id="647c1-142">當 MVC 嘗試呈現在區域中，檢視依預設時，它會嘗試尋找在下列位置：</span><span class="sxs-lookup"><span data-stu-id="647c1-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="647c1-143">這些是可以透過變更預設位置`AreaViewLocationFormats`上`Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`。</span><span class="sxs-lookup"><span data-stu-id="647c1-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="647c1-144">例如，在下列程式碼，而不需要的資料夾名稱為' '，它已變更為 '類別'。</span><span class="sxs-lookup"><span data-stu-id="647c1-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="647c1-145">請注意一件事是，結構*檢視*資料夾就會被視為重要此處，而其餘資料夾的內容要*控制器*和*模型*沒有**不**重要。</span><span class="sxs-lookup"><span data-stu-id="647c1-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="647c1-146">例如，您不需要*控制器*和*模型*所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="647c1-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="647c1-147">因為內容的*控制器*和*模型*是只是程式碼取得編譯成 dll 的內容為*檢視*為止的要求已進行檢視。</span><span class="sxs-lookup"><span data-stu-id="647c1-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="647c1-148">一旦您定義了資料夾階層，您必須告訴 MVC 每個控制站是與區域相關聯。</span><span class="sxs-lookup"><span data-stu-id="647c1-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="647c1-149">您執行此作業，而將控制器名稱與`[Area]`屬性。</span><span class="sxs-lookup"><span data-stu-id="647c1-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="647c1-150">設定路由定義可搭配您新建立的區域。</span><span class="sxs-lookup"><span data-stu-id="647c1-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="647c1-151">[路由至控制器動作](routing.md)文章進入詳細資料，以建立路由定義，包括使用傳統與屬性路由的路由。</span><span class="sxs-lookup"><span data-stu-id="647c1-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="647c1-152">在此範例中，我們會使用傳統的路由。</span><span class="sxs-lookup"><span data-stu-id="647c1-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="647c1-153">若要這樣做，請開啟*Startup.cs*檔案，並修改加`areaRoute`名為路由定義下方。</span><span class="sxs-lookup"><span data-stu-id="647c1-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="647c1-154">瀏覽至`http://<yourApp>/products`、`Index`動作方法的`HomeController`中`Products`區域將會叫用。</span><span class="sxs-lookup"><span data-stu-id="647c1-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="647c1-155">連結產生</span><span class="sxs-lookup"><span data-stu-id="647c1-155">Link Generation</span></span>

* <span data-ttu-id="647c1-156">產生連結從某個區域中的動作，以控制站相同的控制器內的另一個動作。</span><span class="sxs-lookup"><span data-stu-id="647c1-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="647c1-157">假設目前的要求路徑就像是`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="647c1-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="647c1-158">HtmlHelper 語法：`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="647c1-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="647c1-159">TagHelper 語法：`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="647c1-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="647c1-160">請注意，我們不需要提供 '區域' 和 '控制器 」 值此處，因為它們已經就可在目前要求的內容。</span><span class="sxs-lookup"><span data-stu-id="647c1-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="647c1-161">這些類型的值稱為`ambient`值。</span><span class="sxs-lookup"><span data-stu-id="647c1-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="647c1-162">產生連結從某個區域中的動作基礎控制站，以在不同的控制站上的另一個動作</span><span class="sxs-lookup"><span data-stu-id="647c1-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="647c1-163">假設目前的要求路徑就像是`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="647c1-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="647c1-164">HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="647c1-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="647c1-165">TagHelper 語法：`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="647c1-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="647c1-166">請注意，此處會使用 '區域' 環境值上明確地指定控制器 」 值。</span><span class="sxs-lookup"><span data-stu-id="647c1-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="647c1-167">產生連結從某個區域中的動作為基礎控制站，另一個動作以不同的控制器和不同的區域。</span><span class="sxs-lookup"><span data-stu-id="647c1-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="647c1-168">假設目前的要求路徑就像是`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="647c1-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="647c1-169">HtmlHelper 語法：`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="647c1-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="647c1-170">TagHelper 語法：`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="647c1-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="647c1-171">請注意，此處使用任何環境的值。</span><span class="sxs-lookup"><span data-stu-id="647c1-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="647c1-172">從區域控制器內的動作產生連結至不同的控制站上的另一個動作和**不**區域中。</span><span class="sxs-lookup"><span data-stu-id="647c1-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="647c1-173">HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="647c1-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="647c1-174">TagHelper 語法：`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="647c1-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="647c1-175">因為我們想要產生非區域的連結以控制器的動作，我們空白 '區域' 在此環境的值。</span><span class="sxs-lookup"><span data-stu-id="647c1-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="647c1-176">發行的區域</span><span class="sxs-lookup"><span data-stu-id="647c1-176">Publishing Areas</span></span>

<span data-ttu-id="647c1-177">所有`*.cshtml`和`wwwroot/**`檔案會發行至輸出時`<Project Sdk="Microsoft.NET.Sdk.Web">`隨附於*.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="647c1-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
