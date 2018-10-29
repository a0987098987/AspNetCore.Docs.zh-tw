---
title: ASP.NET Core 中的錨點標籤協助程式
author: pkellner
description: 探索 ASP.NET Core 錨點標籤協助程式屬性，以及各屬性在 HTML 錨點標籤的延伸行為中所扮演的角色。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 01c5833210b73dafb763602d363afcf9e7bc0122
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206272"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="30755-103">ASP.NET Core 中的錨點標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="30755-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="30755-104">由 [Peter Kellner](http://peterkellner.net) 與 [Scott Addie](https://github.com/scottaddie) 撰寫</span><span class="sxs-lookup"><span data-stu-id="30755-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="30755-105">[錨點標籤協助程式](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper)藉由新增新的屬性，來增強標準 HTML 錨點 (`<a ... ></a>`) 標籤。</span><span class="sxs-lookup"><span data-stu-id="30755-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="30755-106">依照慣例，屬性名稱的開頭會加上 `asp-`。</span><span class="sxs-lookup"><span data-stu-id="30755-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="30755-107">所轉譯錨點元素的 `href` 屬性值取決於 `asp-` 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="30755-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="30755-108">如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。</span><span class="sxs-lookup"><span data-stu-id="30755-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="30755-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="30755-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="30755-110">這整份文件的範例皆使用 *SpeakerController*：</span><span class="sxs-lookup"><span data-stu-id="30755-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="30755-111">`asp-` 屬性如下所列。</span><span class="sxs-lookup"><span data-stu-id="30755-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="30755-112">asp-controller</span><span class="sxs-lookup"><span data-stu-id="30755-112">asp-controller</span></span>

<span data-ttu-id="30755-113">[asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) 屬性指派用於產生 URL 的控制器。</span><span class="sxs-lookup"><span data-stu-id="30755-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="30755-114">下列標記列出所有喇叭：</span><span class="sxs-lookup"><span data-stu-id="30755-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="30755-115">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="30755-116">若僅指定 `asp-controller` 屬性而未指定 `asp-action`，則預設的 `asp-action` 值即為與目前所執行之檢視相關的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="30755-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="30755-117">若在上述的標記中省略 `asp-action`，且在 *HomeController*'s *Index* 檢視 (*/Home*) 使用錨點標籤協助程式，則產生的 HTML 即為：</span><span class="sxs-lookup"><span data-stu-id="30755-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="30755-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="30755-118">asp-action</span></span>

<span data-ttu-id="30755-119">[asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) 屬性值代表所產生 `href` 屬性中包含的控制器動作名稱。</span><span class="sxs-lookup"><span data-stu-id="30755-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="30755-120">下列標記會將產生的 `href` 屬性值設為喇叭評估頁面：</span><span class="sxs-lookup"><span data-stu-id="30755-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="30755-121">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="30755-122">若未指定任何 `asp-controller` 屬性，則會使用呼叫執行目前檢視之檢視的預設控制器。</span><span class="sxs-lookup"><span data-stu-id="30755-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="30755-123">若 `asp-action` 屬性值為 `Index`，就不會將任何動作附加至 URL，而導致引動預設的 `Index` 動作。</span><span class="sxs-lookup"><span data-stu-id="30755-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="30755-124">指定的動作 (或預設的動作) 必須存在於 `asp-controller` 所參考的控制器中。</span><span class="sxs-lookup"><span data-stu-id="30755-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="30755-125">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="30755-125">asp-route-{value}</span></span>

<span data-ttu-id="30755-126">[asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 屬性可使用萬用字元路由首碼。</span><span class="sxs-lookup"><span data-stu-id="30755-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="30755-127">任何佔用 `{value}` 預留位置的值都會解譯為潛在的路由參數。</span><span class="sxs-lookup"><span data-stu-id="30755-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="30755-128">如果找不到預設路由，則會將此路由首碼附加至產生的 `href` 屬性，作為要求參數與值。</span><span class="sxs-lookup"><span data-stu-id="30755-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="30755-129">否則會在路由範本中加以取代。</span><span class="sxs-lookup"><span data-stu-id="30755-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="30755-130">請考慮下列控制器動作：</span><span class="sxs-lookup"><span data-stu-id="30755-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="30755-131">使用 *Startup.Configure* 中定義的預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="30755-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="30755-132">MVC 檢視會使用動作提供的模型，如下所示：</span><span class="sxs-lookup"><span data-stu-id="30755-132">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="30755-133">預設路由的 `{id?}` 預留位置相符。</span><span class="sxs-lookup"><span data-stu-id="30755-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="30755-134">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="30755-135">假設路由首碼不屬於相符路由範本的一部份，如同下列 MVC 檢視：</span><span class="sxs-lookup"><span data-stu-id="30755-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="30755-136">因為在相符路由中找不到 `speakerid`，所以產生了下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="30755-137">如果未指定 `asp-controller` 或 `asp-action`，則會遵循與 `asp-route` 屬性中相同的預設處理。</span><span class="sxs-lookup"><span data-stu-id="30755-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="30755-138">asp-route</span><span class="sxs-lookup"><span data-stu-id="30755-138">asp-route</span></span>

<span data-ttu-id="30755-139">[asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) 屬性用於建立直接連結至具名路由的 URL。</span><span class="sxs-lookup"><span data-stu-id="30755-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="30755-140">使用[路由屬性](xref:mvc/controllers/routing#attribute-routing)，路由可以如 `SpeakerController` 中所示命名並用於其 `Evaluations` 動作：</span><span class="sxs-lookup"><span data-stu-id="30755-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="30755-141">在下列標記中，`asp-route` 屬性參考了具名路由：</span><span class="sxs-lookup"><span data-stu-id="30755-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="30755-142">錨點標記協助程式會使用 URL */Speaker/Evaluations*，直接對該控制器動作產生路由。</span><span class="sxs-lookup"><span data-stu-id="30755-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="30755-143">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="30755-144">如果除了 `asp-route`，還指定了 `asp-controller` 或 `asp-action`，產生的路由可能不如預期。</span><span class="sxs-lookup"><span data-stu-id="30755-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="30755-145">為避免路由衝突，請勿將 `asp-route` 與 `asp-controller` 及 `asp-action` 屬性搭配使用。</span><span class="sxs-lookup"><span data-stu-id="30755-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="30755-146">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="30755-146">asp-all-route-data</span></span>

<span data-ttu-id="30755-147">[asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 屬性支援建立機碼值組的字典。</span><span class="sxs-lookup"><span data-stu-id="30755-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="30755-148">機碼為參數名稱，值則為參數值。</span><span class="sxs-lookup"><span data-stu-id="30755-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="30755-149">在下列範例中，字典會經過初始化並傳遞至 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="30755-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="30755-150">或者，您也可以使用自己的模型來傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="30755-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="30755-151">上述程式碼會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="30755-152">`asp-all-route-data` 字典已經過扁平化，以產生符合多載 `Evaluations` 動作之需求的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="30755-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="30755-153">若字典中有任何機碼符合路由參數，這些值在路由中會受到適當地取代。</span><span class="sxs-lookup"><span data-stu-id="30755-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="30755-154">其他不相符的值則產生作為要求參數。</span><span class="sxs-lookup"><span data-stu-id="30755-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="30755-155">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="30755-155">asp-fragment</span></span>

<span data-ttu-id="30755-156">[asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) 屬性定義要附加至 URL 的 URL 片段。</span><span class="sxs-lookup"><span data-stu-id="30755-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="30755-157">錨點標籤協助程式會新增雜湊字元 (#)。</span><span class="sxs-lookup"><span data-stu-id="30755-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="30755-158">請考慮下列標記：</span><span class="sxs-lookup"><span data-stu-id="30755-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="30755-159">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="30755-160">雜湊標籤在建置用戶端應用程式時很實用。</span><span class="sxs-lookup"><span data-stu-id="30755-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="30755-161">例如，您可以使用這些標籤在 JavaScript 中輕鬆標記和搜尋。</span><span class="sxs-lookup"><span data-stu-id="30755-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="30755-162">asp-area</span><span class="sxs-lookup"><span data-stu-id="30755-162">asp-area</span></span>

<span data-ttu-id="30755-163">[asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) 屬性設定區域名稱，用以設定合適的路由。</span><span class="sxs-lookup"><span data-stu-id="30755-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="30755-164">下列範例描述了區域屬性如何造成路由重新對應。</span><span class="sxs-lookup"><span data-stu-id="30755-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="30755-165">將 `asp-area` 設定為 "Blogs" 會在 此錨點標籤之相關控制器和檢視的路由前面加上目錄 *Areas/Blogs*。</span><span class="sxs-lookup"><span data-stu-id="30755-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="30755-166">**{專案名稱}**</span><span class="sxs-lookup"><span data-stu-id="30755-166">**{Project name}**</span></span>
  * <span data-ttu-id="30755-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="30755-167">**wwwroot**</span></span>
  * <span data-ttu-id="30755-168">**區域**</span><span class="sxs-lookup"><span data-stu-id="30755-168">**Areas**</span></span>
    * <span data-ttu-id="30755-169">**部落格**</span><span class="sxs-lookup"><span data-stu-id="30755-169">**Blogs**</span></span>
      * <span data-ttu-id="30755-170">**控制器**</span><span class="sxs-lookup"><span data-stu-id="30755-170">**Controllers**</span></span>
        * <span data-ttu-id="30755-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="30755-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="30755-172">**檢視**</span><span class="sxs-lookup"><span data-stu-id="30755-172">**Views**</span></span>
        * <span data-ttu-id="30755-173">**Home**</span><span class="sxs-lookup"><span data-stu-id="30755-173">**Home**</span></span>
          * <span data-ttu-id="30755-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="30755-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="30755-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="30755-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="30755-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="30755-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="30755-177">**控制器**</span><span class="sxs-lookup"><span data-stu-id="30755-177">**Controllers**</span></span>

<span data-ttu-id="30755-178">以上述目錄階層為例，要參考 *AboutBlog.cshtml* 檔案的標記即為：</span><span class="sxs-lookup"><span data-stu-id="30755-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="30755-179">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="30755-180">若要讓區域在 MVC 應用程式中正常運作，路由範本必須包含區域參考 (若其存在)。</span><span class="sxs-lookup"><span data-stu-id="30755-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="30755-181">該範本將以 *Startup.Configure* 中 `routes.MapRoute` 方法呼叫的第二個參數表示：</span><span class="sxs-lookup"><span data-stu-id="30755-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="30755-182">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="30755-182">asp-protocol</span></span>

<span data-ttu-id="30755-183">[asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) 屬性用於在您的 URL 中指定通訊協定 (例如 `https`)。</span><span class="sxs-lookup"><span data-stu-id="30755-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="30755-184">例如: </span><span class="sxs-lookup"><span data-stu-id="30755-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="30755-185">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="30755-186">此範例中的主機名稱為 localhost，但錨點標籤協助程式在產生 URL 時使用網站的公用網域。</span><span class="sxs-lookup"><span data-stu-id="30755-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="30755-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="30755-187">asp-host</span></span>

<span data-ttu-id="30755-188">[asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) 屬性用於在您的 URL 中指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="30755-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="30755-189">例如: </span><span class="sxs-lookup"><span data-stu-id="30755-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="30755-190">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="30755-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="30755-191">asp-page</span></span>

<span data-ttu-id="30755-192">[asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) 屬性與 Razor 頁面搭配使用。</span><span class="sxs-lookup"><span data-stu-id="30755-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="30755-193">您可用其將錨點標籤的 `href` 屬性值設定為特定頁面。</span><span class="sxs-lookup"><span data-stu-id="30755-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="30755-194">在頁面名稱的開頭加上正斜線 ("/") 即可建立 URL。</span><span class="sxs-lookup"><span data-stu-id="30755-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="30755-195">下列範例指向出席者 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="30755-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="30755-196">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="30755-197">`asp-page` 屬性與 `asp-route`、`asp-controller` 以及 `asp-action` 屬性互斥。</span><span class="sxs-lookup"><span data-stu-id="30755-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="30755-198">但是，`asp-page` 可以搭配 `asp-route-{value}` 使用來控制路由，如下列標記所示：</span><span class="sxs-lookup"><span data-stu-id="30755-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="30755-199">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="30755-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="30755-200">asp-page-handler</span></span>

<span data-ttu-id="30755-201">[asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) 屬性與 Razor 頁面搭配使用。</span><span class="sxs-lookup"><span data-stu-id="30755-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="30755-202">其用途為建立特定頁面處理常式的連結。</span><span class="sxs-lookup"><span data-stu-id="30755-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="30755-203">請考慮下列頁面處理常式：</span><span class="sxs-lookup"><span data-stu-id="30755-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="30755-204">頁面模型的相關標記會連結到 `OnGetProfile` 頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="30755-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="30755-205">請注意，`asp-page-handler` 屬性值中會省略頁面處理常式方法名稱的 `On<Verb>` 首碼。</span><span class="sxs-lookup"><span data-stu-id="30755-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="30755-206">如果這是非同步方法，則 `Async` 尾碼也會一併省略。</span><span class="sxs-lookup"><span data-stu-id="30755-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="30755-207">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="30755-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="30755-208">其他資源</span><span class="sxs-lookup"><span data-stu-id="30755-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
