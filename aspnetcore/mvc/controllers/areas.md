---
title: ASP.NET Core 中的區域
author: rick-anderson
description: 了解其為 ASP.NET MVC 功能的區域，如何用來將相關功能組織成群組，作為個別命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。
ms.author: riande
ms.date: 08/16/2019
uid: mvc/controllers/areas
ms.openlocfilehash: d0af3092776ee09469c879fffd3047c50b1a59b4
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545801"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="21045-103">ASP.NET Core 中的區域</span><span class="sxs-lookup"><span data-stu-id="21045-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="21045-104">作者：[Dhananjay Kumar](https://twitter.com/debug_mode) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21045-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="21045-105">區域是 ASP.NET 功能，可用來將相關功能組織為群組，以作為個別的命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。</span><span class="sxs-lookup"><span data-stu-id="21045-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="21045-106">使用區域可基於路由的目的，藉由將另一個路由參數 `area` 新增至 `controller` 和 `action` 或 Razor 頁面 `page` 來建立階層。</span><span class="sxs-lookup"><span data-stu-id="21045-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="21045-107">區域可提供一種方式來將 ASP.NET Core Web 應用程式分割成較小的功能群組，每個都有一組自己的 Razor Pages、控制器、檢視和模型。</span><span class="sxs-lookup"><span data-stu-id="21045-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="21045-108">一個區域基本上是應用程式內的一個結構。</span><span class="sxs-lookup"><span data-stu-id="21045-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="21045-109">在 ASP.NET Core Web 專案中，Pages、模型、控制器和檢視等邏輯元件都會保留在不同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="21045-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="21045-110">ASP.NET Core 執行階段會使用命名慣例來建立這些元件之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="21045-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="21045-111">針對大型應用程式，將應用程式分割成個別高功能層級區域可能較有利。</span><span class="sxs-lookup"><span data-stu-id="21045-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="21045-112">舉例來說，一個電子商務應用程式可具有多個業務單位，例如結帳、計費和搜尋。</span><span class="sxs-lookup"><span data-stu-id="21045-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="21045-113">這其中的每個單位都有自己的區域，以包含檢視、控制器、Razor Pages 和模型。</span><span class="sxs-lookup"><span data-stu-id="21045-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="21045-114">處於下列情況時，請考慮在專案中使用區域：</span><span class="sxs-lookup"><span data-stu-id="21045-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="21045-115">應用程式是由可以邏輯方式區隔的多個高階功能性元件所組成的。</span><span class="sxs-lookup"><span data-stu-id="21045-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="21045-116">您想要分割應用程式，讓每個功能區域都可獨立運作。</span><span class="sxs-lookup"><span data-stu-id="21045-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="21045-117">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="21045-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="21045-118">下載範例提供基本的應用程式來測試區域。</span><span class="sxs-lookup"><span data-stu-id="21045-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="21045-119">如果您使用 Razor Pages，請參閱此文件中的[使用 Razor Pages 的區域](#areas-with-razor-pages)。</span><span class="sxs-lookup"><span data-stu-id="21045-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="21045-120">適用於控制器與檢視的區域</span><span class="sxs-lookup"><span data-stu-id="21045-120">Areas for controllers with views</span></span>

<span data-ttu-id="21045-121">使用區域、控制器及檢視的典型 ASP.NET Core Web 應用程式包含下列項目：</span><span class="sxs-lookup"><span data-stu-id="21045-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="21045-122">一個[區域資料夾結構](#area-folder-structure)。</span><span class="sxs-lookup"><span data-stu-id="21045-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="21045-123">使用 [&lbrack;Area&rbrack;](#attribute) 屬性裝飾的控制器可使控制器與區域產生關聯：</span><span class="sxs-lookup"><span data-stu-id="21045-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="21045-124">[已新增至啟動的區域路由](#add-area-route)：</span><span class="sxs-lookup"><span data-stu-id="21045-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="21045-125">區域資料夾結構</span><span class="sxs-lookup"><span data-stu-id="21045-125">Area folder structure</span></span>

<span data-ttu-id="21045-126">假設應用程式具有兩個邏輯群組：「產品」  和「服務」  。</span><span class="sxs-lookup"><span data-stu-id="21045-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="21045-127">使用區域，資料夾結構應該如下：</span><span class="sxs-lookup"><span data-stu-id="21045-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="21045-128">Project name</span><span class="sxs-lookup"><span data-stu-id="21045-128">Project name</span></span>
  * <span data-ttu-id="21045-129">區域</span><span class="sxs-lookup"><span data-stu-id="21045-129">Areas</span></span>
    * <span data-ttu-id="21045-130">產品</span><span class="sxs-lookup"><span data-stu-id="21045-130">Products</span></span>
      * <span data-ttu-id="21045-131">Controllers</span><span class="sxs-lookup"><span data-stu-id="21045-131">Controllers</span></span>
        * <span data-ttu-id="21045-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="21045-132">HomeController.cs</span></span>
        * <span data-ttu-id="21045-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="21045-133">ManageController.cs</span></span>
      * <span data-ttu-id="21045-134">檢視</span><span class="sxs-lookup"><span data-stu-id="21045-134">Views</span></span>
        * <span data-ttu-id="21045-135">首頁</span><span class="sxs-lookup"><span data-stu-id="21045-135">Home</span></span>
          * <span data-ttu-id="21045-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="21045-136">Index.cshtml</span></span>
        * <span data-ttu-id="21045-137">管理</span><span class="sxs-lookup"><span data-stu-id="21045-137">Manage</span></span>
          * <span data-ttu-id="21045-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="21045-138">Index.cshtml</span></span>
          * <span data-ttu-id="21045-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="21045-139">About.cshtml</span></span>
    * <span data-ttu-id="21045-140">服務</span><span class="sxs-lookup"><span data-stu-id="21045-140">Services</span></span>
      * <span data-ttu-id="21045-141">Controllers</span><span class="sxs-lookup"><span data-stu-id="21045-141">Controllers</span></span>
        * <span data-ttu-id="21045-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="21045-142">HomeController.cs</span></span>
      * <span data-ttu-id="21045-143">檢視</span><span class="sxs-lookup"><span data-stu-id="21045-143">Views</span></span>
        * <span data-ttu-id="21045-144">首頁</span><span class="sxs-lookup"><span data-stu-id="21045-144">Home</span></span>
          * <span data-ttu-id="21045-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="21045-145">Index.cshtml</span></span>

<span data-ttu-id="21045-146">儘管上述配置通常會在使用區域時使用，但是只需有檢視檔案，就能使用此資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="21045-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="21045-147">檢視探索會依下列順序搜尋相符的區域檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="21045-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="21045-148">非檢視資料夾的位置 (例如「控制站」  和「模型」  ) 並**不**重要。</span><span class="sxs-lookup"><span data-stu-id="21045-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="21045-149">例如，「控制站」  和「模型」  資料夾並非必要項。</span><span class="sxs-lookup"><span data-stu-id="21045-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="21045-150">「控制器」  和「模型」  的內容都是要編譯為 .dll 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="21045-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="21045-151">「檢視」  的內容要在向該檢視發出要求之後才會編譯。</span><span class="sxs-lookup"><span data-stu-id="21045-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="21045-152">使控制器與區域產生關聯</span><span class="sxs-lookup"><span data-stu-id="21045-152">Associate the controller with an Area</span></span>

<span data-ttu-id="21045-153">區域控制器會透過 [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) 屬性來指定：</span><span class="sxs-lookup"><span data-stu-id="21045-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="21045-154">新增區域路由</span><span class="sxs-lookup"><span data-stu-id="21045-154">Add Area route</span></span>

<span data-ttu-id="21045-155">區域路由通常會使用慣例路由，而非屬性路由。</span><span class="sxs-lookup"><span data-stu-id="21045-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="21045-156">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="21045-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="21045-157">一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。</span><span class="sxs-lookup"><span data-stu-id="21045-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="21045-158">如果 URL 空間在所有區域中都是統一的，則可使用 `{area:...}` 作為路由範本中的語彙基元：</span><span class="sxs-lookup"><span data-stu-id="21045-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="21045-159">在上述程式碼中，`exists` 會套用路由必須與區域相符的條件約束。</span><span class="sxs-lookup"><span data-stu-id="21045-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="21045-160">若要將路由新增至區域，使用 `{area:...}` 是最簡單的機制。</span><span class="sxs-lookup"><span data-stu-id="21045-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="21045-161">下列程式碼會使用 <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> 來建立兩個具名的區域路由：</span><span class="sxs-lookup"><span data-stu-id="21045-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="21045-162">搭配 ASP.NET Core 2.2 使用 `MapAreaRoute` 時，請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/7772) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="21045-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="21045-163">如需詳細資訊，請參閱[區域路由](xref:mvc/controllers/routing#areas)。</span><span class="sxs-lookup"><span data-stu-id="21045-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="21045-164">使用 MVC 區域產生連結</span><span class="sxs-lookup"><span data-stu-id="21045-164">Link generation with MVC areas</span></span>

<span data-ttu-id="21045-165">以下來自[範例下載](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)的程式碼會顯示使用指定的區域來產生連結：</span><span class="sxs-lookup"><span data-stu-id="21045-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="21045-166">使用上述程式碼產生的連結，在應用程式中的任何位置都是有效的。</span><span class="sxs-lookup"><span data-stu-id="21045-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="21045-167">範例下載包括[部分檢視](xref:mvc/views/partial)，其中可在未指定區域的情況下包含上述連結和相同連結。</span><span class="sxs-lookup"><span data-stu-id="21045-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="21045-168">部分檢視會在[配置檔案](xref:mvc/views/layout)中進行參考，因此，應用程式中的每個頁面都會顯示產生的連結。</span><span class="sxs-lookup"><span data-stu-id="21045-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="21045-169">在未指定區域的情況下產生的連結，只有在從相同區域與控制器中的頁面進行參考時才有效。</span><span class="sxs-lookup"><span data-stu-id="21045-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="21045-170">未指定區域或控制站時，路由即會取決於「環境」  值。</span><span class="sxs-lookup"><span data-stu-id="21045-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="21045-171">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="21045-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="21045-172">在許多適用於範例應用程式的案例中，使用環境值會產生不正確的連結。</span><span class="sxs-lookup"><span data-stu-id="21045-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="21045-173">如需詳細資訊，請參閱[路由至控制器動作](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="21045-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="21045-174">使用 _ViewStart.cshtml 檔案共用區域的配置</span><span class="sxs-lookup"><span data-stu-id="21045-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="21045-175">若要針對整個應用程式共用通用的配置，請將 *_ViewStart.cshtml* 移至應用程式根資料夾。</span><span class="sxs-lookup"><span data-stu-id="21045-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="21045-176">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="21045-176">_ViewImports.cshtml</span></span>

<span data-ttu-id="21045-177">在其標準位置中， */Views/_ViewImports.cshtml* 不適用於區域。</span><span class="sxs-lookup"><span data-stu-id="21045-177">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="21045-178">若要在您的區域中使用一般[標記協助程式](xref:mvc/views/tag-helpers/intro)、`@using` 或 `@inject`，請確定適當的 *_ViewImports* 檔案[適用於您的區域檢視](xref:mvc/views/layout#importing-shared-directives)。</span><span class="sxs-lookup"><span data-stu-id="21045-178">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="21045-179">如果您希望在所有檢視中都有相同的行為，請將 */Views/_ViewImports.cshtml* 移至應用程式根目錄。</span><span class="sxs-lookup"><span data-stu-id="21045-179">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="21045-180">變更檢視儲存所在的預設區域資料夾</span><span class="sxs-lookup"><span data-stu-id="21045-180">Change default area folder where views are stored</span></span>

<span data-ttu-id="21045-181">下列程式碼會將預設的區域資料夾從 `"Areas"` 變更為 `"MyAreas"`：</span><span class="sxs-lookup"><span data-stu-id="21045-181">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="21045-182">使用 Razor Pages 的區域</span><span class="sxs-lookup"><span data-stu-id="21045-182">Areas with Razor Pages</span></span>

<span data-ttu-id="21045-183">使用 Razor Pages 的區域，在應用程式的根目錄中需要有 *Areas/<area name>/Pages* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="21045-183">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="21045-184">下列資料夾結構搭配[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)一起使用：</span><span class="sxs-lookup"><span data-stu-id="21045-184">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="21045-185">Project name</span><span class="sxs-lookup"><span data-stu-id="21045-185">Project name</span></span>
  * <span data-ttu-id="21045-186">區域</span><span class="sxs-lookup"><span data-stu-id="21045-186">Areas</span></span>
    * <span data-ttu-id="21045-187">產品</span><span class="sxs-lookup"><span data-stu-id="21045-187">Products</span></span>
      * <span data-ttu-id="21045-188">頁面</span><span class="sxs-lookup"><span data-stu-id="21045-188">Pages</span></span>
        * <span data-ttu-id="21045-189">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="21045-189">_ViewImports</span></span>
        * <span data-ttu-id="21045-190">關於</span><span class="sxs-lookup"><span data-stu-id="21045-190">About</span></span>
        * <span data-ttu-id="21045-191">索引</span><span class="sxs-lookup"><span data-stu-id="21045-191">Index</span></span>
    * <span data-ttu-id="21045-192">服務</span><span class="sxs-lookup"><span data-stu-id="21045-192">Services</span></span>
      * <span data-ttu-id="21045-193">頁面</span><span class="sxs-lookup"><span data-stu-id="21045-193">Pages</span></span>
        * <span data-ttu-id="21045-194">管理</span><span class="sxs-lookup"><span data-stu-id="21045-194">Manage</span></span>
          * <span data-ttu-id="21045-195">關於</span><span class="sxs-lookup"><span data-stu-id="21045-195">About</span></span>
          * <span data-ttu-id="21045-196">索引</span><span class="sxs-lookup"><span data-stu-id="21045-196">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="21045-197">使用 Razor Pages 和區域產生連結</span><span class="sxs-lookup"><span data-stu-id="21045-197">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="21045-198">以下來自[範例下載](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas)的程式碼會顯示使用指定的區域 (例如 `asp-area="Products"`) 來產生連結：</span><span class="sxs-lookup"><span data-stu-id="21045-198">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="21045-199">使用上述程式碼產生的連結，在應用程式中的任何位置都是有效的。</span><span class="sxs-lookup"><span data-stu-id="21045-199">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="21045-200">範例下載包括[部分檢視](xref:mvc/views/partial)，其中可在未指定區域的情況下包含上述連結和相同連結。</span><span class="sxs-lookup"><span data-stu-id="21045-200">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="21045-201">部分檢視會在[配置檔案](xref:mvc/views/layout)中進行參考，因此，應用程式中的每個頁面都會顯示產生的連結。</span><span class="sxs-lookup"><span data-stu-id="21045-201">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="21045-202">在未指定區域的情況下產生的連結，只有在從相同區域中的頁面進行參考時才有效。</span><span class="sxs-lookup"><span data-stu-id="21045-202">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="21045-203">未指定區域時，路由即會取決於「環境」  值。</span><span class="sxs-lookup"><span data-stu-id="21045-203">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="21045-204">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="21045-204">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="21045-205">在許多適用於範例應用程式的案例中，使用環境值會產生不正確的連結。</span><span class="sxs-lookup"><span data-stu-id="21045-205">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="21045-206">例如，試想從下列程式碼產生的連結：</span><span class="sxs-lookup"><span data-stu-id="21045-206">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="21045-207">針對上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="21045-207">For the preceding code:</span></span>

* <span data-ttu-id="21045-208">僅當最後一個要求是針對 `Services` 區域中的頁面時，從 `<a asp-page="/Manage/About">` 產生的連結才是正確的。</span><span class="sxs-lookup"><span data-stu-id="21045-208">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="21045-209">例如 `/Services/Manage/`、`/Services/Manage/Index` 或 `/Services/Manage/About`。</span><span class="sxs-lookup"><span data-stu-id="21045-209">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="21045-210">僅當最後一個要求是針對 `/Home` 中的頁面時，從 `<a asp-page="/About">` 產生的連結才是正確的。</span><span class="sxs-lookup"><span data-stu-id="21045-210">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="21045-211">程式碼來自[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas)。</span><span class="sxs-lookup"><span data-stu-id="21045-211">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="21045-212">使用 _ViewImports 檔案匯入命名空間和標記協助程式</span><span class="sxs-lookup"><span data-stu-id="21045-212">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="21045-213">可以將 *_ViewImports.cshtml* 檔案新增至每個區域 *Pages* 資料夾，以將命名空間和標記協助程式匯入到資料夾中的每個 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="21045-213">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="21045-214">請考慮範例程式碼的 *Services* 區域，該區域不包含 *_ViewImports.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="21045-214">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="21045-215">下列標記示範 */Services/Manage/About* Razor頁面：</span><span class="sxs-lookup"><span data-stu-id="21045-215">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="21045-216">在上述標記中：</span><span class="sxs-lookup"><span data-stu-id="21045-216">In the preceding markup:</span></span>

* <span data-ttu-id="21045-217">必須使用完整的網域名稱來指定此模型 (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`)。</span><span class="sxs-lookup"><span data-stu-id="21045-217">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="21045-218">[標記協助程式](xref:mvc/views/tag-helpers/intro)由 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 啟用</span><span class="sxs-lookup"><span data-stu-id="21045-218">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="21045-219">在範例下載中，Products 區域包含下列 *_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="21045-219">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="21045-220">下列標記示範 */Products/About* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="21045-220">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="21045-221">在上述檔案中，命名空間和 `@addTagHelper` 指示詞會由 *Areas/Products/Pages/_ViewImports.cshtml* 檔案匯入到檔案。</span><span class="sxs-lookup"><span data-stu-id="21045-221">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="21045-222">如需詳細資訊，請參閱[管理標籤協助程式範圍](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope)和[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)。</span><span class="sxs-lookup"><span data-stu-id="21045-222">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="21045-223">Razor Pages 區域的共用版面配置</span><span class="sxs-lookup"><span data-stu-id="21045-223">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="21045-224">若要針對整個應用程式共用通用的配置，請將 *_ViewStart.cshtml* 移至應用程式根資料夾。</span><span class="sxs-lookup"><span data-stu-id="21045-224">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="21045-225">發行區域</span><span class="sxs-lookup"><span data-stu-id="21045-225">Publishing Areas</span></span>

<span data-ttu-id="21045-226">當 \*.csproj 檔案中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">` 時，會將所有 \*.cshtml 檔案及 *wwwroot* 目錄內的檔案發佈至輸出。</span><span class="sxs-lookup"><span data-stu-id="21045-226">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
