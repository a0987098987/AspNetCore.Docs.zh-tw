---
title: ASP.NET Core 中的區域
author: rick-anderson
description: 了解其為 ASP.NET MVC 功能的區域，如何用來將相關功能組織成群組，作為個別命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 61527eb350b5aba9cb37b1de5acdeae1287bf073
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072717"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="27e47-103">ASP.NET Core 中的區域</span><span class="sxs-lookup"><span data-stu-id="27e47-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="27e47-104">作者：[Dhananjay Kumar](https://twitter.com/debug_mode) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="27e47-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="27e47-105">區域是 ASP.NET MVC 功能，用來將相關功能組織成群組，作為個別命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。</span><span class="sxs-lookup"><span data-stu-id="27e47-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="27e47-106">使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。</span><span class="sxs-lookup"><span data-stu-id="27e47-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="27e47-107">區域可讓您將大型 ASP.NET Core MVC Web 應用程式分割成較小的功能群組。</span><span class="sxs-lookup"><span data-stu-id="27e47-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="27e47-108">一個區域基本上是應用程式內的一個 MVC 結構。</span><span class="sxs-lookup"><span data-stu-id="27e47-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="27e47-109">在 MVC 專案中，模型、控制器和檢視等邏輯元件會保留在不同的資料夾中，而且 MVC 會使用命名慣例來建立這些元件之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="27e47-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="27e47-110">針對大型應用程式，將應用程式分割成個別高階區域可能較有利。</span><span class="sxs-lookup"><span data-stu-id="27e47-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="27e47-111">例如，一個電子商務應用程式可分成多個業務單位，例如結帳、計費和搜尋等。所有這些單位都有自己的邏輯元件檢視、控制器和模型。</span><span class="sxs-lookup"><span data-stu-id="27e47-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="27e47-112">在此案例中，您可以使用區域來實體分割相同專案中的商務元件。</span><span class="sxs-lookup"><span data-stu-id="27e47-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="27e47-113">在 ASP.NET Core MVC 專案中，區域可以定義為較小的功能單位，並且具有一組自己的控制器、檢視和模型。</span><span class="sxs-lookup"><span data-stu-id="27e47-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="27e47-114">在下列情況時，請考慮在 MVC 專案中使用區域：</span><span class="sxs-lookup"><span data-stu-id="27e47-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="27e47-115">您的應用程式是由應該透過邏輯方式區隔的多個高階功能性元件所構成</span><span class="sxs-lookup"><span data-stu-id="27e47-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="27e47-116">您想要分割 MVC 專案，讓每個功能區域都可以獨立運作</span><span class="sxs-lookup"><span data-stu-id="27e47-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="27e47-117">區域功能：</span><span class="sxs-lookup"><span data-stu-id="27e47-117">Area features:</span></span>

* <span data-ttu-id="27e47-118">ASP.NET Core MVC 應用程式可以有任意數目的區域</span><span class="sxs-lookup"><span data-stu-id="27e47-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="27e47-119">每個區域都有自己的控制器、模型和檢視</span><span class="sxs-lookup"><span data-stu-id="27e47-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="27e47-120">可讓您將大型 MVC 專案組織成多個可以獨立處理的高階元件</span><span class="sxs-lookup"><span data-stu-id="27e47-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="27e47-121">支援多個同名的控制器 (只要這些控制器有不同的「區域」即可)</span><span class="sxs-lookup"><span data-stu-id="27e47-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="27e47-122">請查看範例來說明如何建立和使用區域。</span><span class="sxs-lookup"><span data-stu-id="27e47-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="27e47-123">假設您的市集應用程式具有兩個不同的控制器和檢視分組：產品和服務。</span><span class="sxs-lookup"><span data-stu-id="27e47-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="27e47-124">使用 MVC 區域的一般資料夾結構如下：</span><span class="sxs-lookup"><span data-stu-id="27e47-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="27e47-125">專案名稱</span><span class="sxs-lookup"><span data-stu-id="27e47-125">Project name</span></span>

  * <span data-ttu-id="27e47-126">區域</span><span class="sxs-lookup"><span data-stu-id="27e47-126">Areas</span></span>

    * <span data-ttu-id="27e47-127">產品</span><span class="sxs-lookup"><span data-stu-id="27e47-127">Products</span></span>

      * <span data-ttu-id="27e47-128">Controllers</span><span class="sxs-lookup"><span data-stu-id="27e47-128">Controllers</span></span>

        * <span data-ttu-id="27e47-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="27e47-129">HomeController.cs</span></span>

        * <span data-ttu-id="27e47-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="27e47-130">ManageController.cs</span></span>

      * <span data-ttu-id="27e47-131">檢視</span><span class="sxs-lookup"><span data-stu-id="27e47-131">Views</span></span>

        * <span data-ttu-id="27e47-132">首頁</span><span class="sxs-lookup"><span data-stu-id="27e47-132">Home</span></span>

          * <span data-ttu-id="27e47-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="27e47-133">Index.cshtml</span></span>

        * <span data-ttu-id="27e47-134">管理</span><span class="sxs-lookup"><span data-stu-id="27e47-134">Manage</span></span>

          * <span data-ttu-id="27e47-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="27e47-135">Index.cshtml</span></span>

    * <span data-ttu-id="27e47-136">服務</span><span class="sxs-lookup"><span data-stu-id="27e47-136">Services</span></span>

      * <span data-ttu-id="27e47-137">Controllers</span><span class="sxs-lookup"><span data-stu-id="27e47-137">Controllers</span></span>

        * <span data-ttu-id="27e47-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="27e47-138">HomeController.cs</span></span>

      * <span data-ttu-id="27e47-139">檢視</span><span class="sxs-lookup"><span data-stu-id="27e47-139">Views</span></span>

        * <span data-ttu-id="27e47-140">首頁</span><span class="sxs-lookup"><span data-stu-id="27e47-140">Home</span></span>

          * <span data-ttu-id="27e47-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="27e47-141">Index.cshtml</span></span>

<span data-ttu-id="27e47-142">MVC 嘗試轉譯區域中的檢視時，預設會嘗試在下列位置中尋找：</span><span class="sxs-lookup"><span data-stu-id="27e47-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="27e47-143">這些是 `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` 上可透過 `AreaViewLocationFormats` 變更的預設位置。</span><span class="sxs-lookup"><span data-stu-id="27e47-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="27e47-144">例如，在下列程式碼中，它已變更為 'Categories'，而不是讓資料夾名稱成為 'Areas'。</span><span class="sxs-lookup"><span data-stu-id="27e47-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="27e47-145">有一件事需要注意：*Views* 資料夾結構是這裡唯一視為重要的結構，而其餘資料夾 (例如 *Controllers* 和 *Models*) 的內容並**不**重要。</span><span class="sxs-lookup"><span data-stu-id="27e47-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="27e47-146">例如，您根本不需要 *Controllers* 和 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="27e47-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="27e47-147">原因是 *Controllers* 和 *Models* 的內容就只是要編譯為 .dll 的程式碼；但除非已對該檢視提出要求，否則 *Views* 的內容不是程式碼。</span><span class="sxs-lookup"><span data-stu-id="27e47-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="27e47-148">定義資料夾階層之後，需要告訴 MVC：每個控制器都會與某個區域建立關聯。</span><span class="sxs-lookup"><span data-stu-id="27e47-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="27e47-149">做法是以 `[Area]` 屬性裝飾控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="27e47-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="27e47-150">設定可與新建立區域搭配使用的路由定義。</span><span class="sxs-lookup"><span data-stu-id="27e47-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="27e47-151">[路由至控制器動作](routing.md)一文會詳述如何建立路由定義，包括使用傳統路由與屬性路由的比較。</span><span class="sxs-lookup"><span data-stu-id="27e47-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="27e47-152">在此範例中，我們將使用傳統路由。</span><span class="sxs-lookup"><span data-stu-id="27e47-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="27e47-153">若要這樣做，請開啟 *Startup.cs* 檔案，然後新增下面的 `areaRoute` 具名路由定義以進行修改。</span><span class="sxs-lookup"><span data-stu-id="27e47-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="27e47-154">瀏覽至 `http://<yourApp>/products`，以叫用 `Products` 區域中 `HomeController` 的 `Index` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="27e47-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="27e47-155">連結產生</span><span class="sxs-lookup"><span data-stu-id="27e47-155">Link Generation</span></span>

* <span data-ttu-id="27e47-156">產生下列兩者之間的連結：從區域控制器內的某個動作到相同控制器內的另一個動作。</span><span class="sxs-lookup"><span data-stu-id="27e47-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="27e47-157">假設目前的要求路徑如下：`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="27e47-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="27e47-158">HtmlHelper 語法：`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="27e47-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="27e47-159">TagHelper 語法：`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="27e47-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="27e47-160">請注意，我們在這裡不需要提供 'area' 和 'controller' 值，因為目前要求的內容中已經有它們。</span><span class="sxs-lookup"><span data-stu-id="27e47-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="27e47-161">這些類型的值稱為 `ambient` 值。</span><span class="sxs-lookup"><span data-stu-id="27e47-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="27e47-162">產生下列兩者之間的連結：從區域控制器內的某個動作到不同控制器上的另一個動作</span><span class="sxs-lookup"><span data-stu-id="27e47-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="27e47-163">假設目前的要求路徑如下：`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="27e47-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="27e47-164">HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="27e47-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="27e47-165">TagHelper 語法：`<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="27e47-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="27e47-166">請注意，這裡會使用 'area' 的環境值，但要在上方明確指定 'controller' 值。</span><span class="sxs-lookup"><span data-stu-id="27e47-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="27e47-167">產生下列兩者之間的連結：從區域控制器內的某個動作到不同控制器和不同區域上的另一個動作。</span><span class="sxs-lookup"><span data-stu-id="27e47-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="27e47-168">假設目前的要求路徑如下：`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="27e47-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="27e47-169">HtmlHelper 語法：`@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="27e47-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="27e47-170">TagHelper 語法：`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="27e47-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="27e47-171">請注意，這裡未使用環境值。</span><span class="sxs-lookup"><span data-stu-id="27e47-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="27e47-172">產生下列兩者之間的連結：從區域控制器內的某個動作到不同控制器上但**不**在區域中的另一個動作。</span><span class="sxs-lookup"><span data-stu-id="27e47-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="27e47-173">HtmlHelper 語法：`@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="27e47-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="27e47-174">TagHelper 語法：`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="27e47-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="27e47-175">因為我們想要產生非區域控制器動作的連結，所以我們在這裡清空 'area' 的環境值。</span><span class="sxs-lookup"><span data-stu-id="27e47-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="27e47-176">發行區域</span><span class="sxs-lookup"><span data-stu-id="27e47-176">Publishing Areas</span></span>

<span data-ttu-id="27e47-177">*.csproj* 檔案中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">` 時，所有 `*.cshtml` 和 `wwwroot/**` 檔案都會發行至輸出。</span><span class="sxs-lookup"><span data-stu-id="27e47-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
