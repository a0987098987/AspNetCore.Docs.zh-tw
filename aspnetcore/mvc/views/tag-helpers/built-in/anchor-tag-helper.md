---
title: "錨定標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用錨定標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 7923876c792544ac4d559eb8de29475d8a4b37e0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="ce343-103">錨定標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ce343-103">Anchor Tag Helper</span></span>

<span data-ttu-id="ce343-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="ce343-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ce343-105">錨定標記協助程式增強 HTML 錨定 (`<a ... ></a>`) 藉由加入新屬性的標記。</span><span class="sxs-lookup"><span data-stu-id="ce343-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="ce343-106">所產生的連結 (在`href`標記) 會建立使用新的屬性。</span><span class="sxs-lookup"><span data-stu-id="ce343-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="ce343-107">該 URL 可以包含選擇性通訊協定，例如 https。</span><span class="sxs-lookup"><span data-stu-id="ce343-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="ce343-108">在這份文件中的範例中用於下列的喇叭控制器。</span><span class="sxs-lookup"><span data-stu-id="ce343-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="ce343-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="ce343-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="ce343-110">錨定標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="ce343-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="ce343-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="ce343-111">asp-controller</span></span>

<span data-ttu-id="ce343-112">`asp-controller`用來建立關聯的控制器將會用來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ce343-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="ce343-113">指定的控制器必須存在於目前的專案。</span><span class="sxs-lookup"><span data-stu-id="ce343-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="ce343-114">下列程式碼列出所有的喇叭：</span><span class="sxs-lookup"><span data-stu-id="ce343-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="ce343-115">將產生的標記：</span><span class="sxs-lookup"><span data-stu-id="ce343-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="ce343-116">如果`asp-controller`指定和`asp-action`不是，預設值`asp-action`將目前正在執行檢視的預設控制器方法。</span><span class="sxs-lookup"><span data-stu-id="ce343-116">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="ce343-117">是，在上述範例中，如果`asp-action`被省略，而且此錨定標記協助程式從產生的*HomeController*的`Index`檢視 (**/首頁**)，將會產生的標記：</span><span class="sxs-lookup"><span data-stu-id="ce343-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="ce343-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="ce343-118">asp-action</span></span>

<span data-ttu-id="ce343-119">`asp-action`將會包含控制器動作方法的名稱在產生`href`。</span><span class="sxs-lookup"><span data-stu-id="ce343-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="ce343-120">例如，下列程式碼產生設定`href`指向喇叭詳細資料頁面：</span><span class="sxs-lookup"><span data-stu-id="ce343-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="ce343-121">將產生的標記：</span><span class="sxs-lookup"><span data-stu-id="ce343-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="ce343-122">如果沒有`asp-controller`屬性已指定，會使用預設的控制站呼叫執行目前檢視的檢視。</span><span class="sxs-lookup"><span data-stu-id="ce343-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="ce343-123">如果屬性`asp-action`是`Index`，然後採取任何動作會不附加至的 URL，導致預設`Index`所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="ce343-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="ce343-124">動作指定 （或預設），必須在控制器中參考`asp-controller`。</span><span class="sxs-lookup"><span data-stu-id="ce343-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="ce343-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="ce343-125">asp-page</span></span>

<span data-ttu-id="ce343-126">使用`asp-page`屬性中設定其 URL，以指向特定頁面的錨點標籤。</span><span class="sxs-lookup"><span data-stu-id="ce343-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="ce343-127">以正斜線的頁面名稱前面加上"/"來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ce343-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="ce343-128">下面範例中的 URL 會指向目前的目錄中的"喇叭 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="ce343-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="ce343-129">`asp-page`屬性在上述程式碼範例中的呈現 HTML 輸出中的檢視，類似於下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="ce343-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="ce343-130">`asp-page`屬性是互斥的`asp-route`， `asp-controller`，和`asp-action`屬性。</span><span class="sxs-lookup"><span data-stu-id="ce343-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="ce343-131">不過，`asp-page`可以搭配`asp-route-id`來控制路由，如下列程式碼範例所示：</span><span class="sxs-lookup"><span data-stu-id="ce343-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="ce343-132">`asp-route-id`會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="ce343-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="ce343-133">若要使用`asp-page`Razor 頁面中，Url 屬性必須是相對路徑，例如`"./Speaker"`。</span><span class="sxs-lookup"><span data-stu-id="ce343-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="ce343-134">中的相對路徑`asp-page`屬性不適用於 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="ce343-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="ce343-135">請改用 MVC 檢視的"/"語法。</span><span class="sxs-lookup"><span data-stu-id="ce343-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="ce343-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="ce343-136">asp-route-{value}</span></span>

<span data-ttu-id="ce343-137">`asp-route-`是萬用字元路由前置詞。</span><span class="sxs-lookup"><span data-stu-id="ce343-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="ce343-138">尾端的虛線會被解譯為潛在的路由參數之後，您將任何值。</span><span class="sxs-lookup"><span data-stu-id="ce343-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="ce343-139">如果找不到預設路由，此路由前置字元會附加至要求參數和值為產生的 href。</span><span class="sxs-lookup"><span data-stu-id="ce343-139">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="ce343-140">否則將會取代中的路由範本。</span><span class="sxs-lookup"><span data-stu-id="ce343-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="ce343-141">假設您有控制器方法定義，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ce343-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="ce343-142">而且有預設路由範本中定義您*Startup.cs* ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ce343-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="ce343-143">**Cshtml**檔案，其中包含錨定標記協助程式需要使用**喇叭**傳入從控制器到檢視的模型參數如下所示：</span><span class="sxs-lookup"><span data-stu-id="ce343-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="ce343-144">產生的 HTML 會如下所示因為**識別碼**預設路由中找不到。</span><span class="sxs-lookup"><span data-stu-id="ce343-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="ce343-145">如果路由前置字元不是找到的路由範本的一部分，也就是以下列**cshtml**檔案：</span><span class="sxs-lookup"><span data-stu-id="ce343-145">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="ce343-146">產生的 HTML 會如下所示因為**speakerid**找不到相符的路由中：</span><span class="sxs-lookup"><span data-stu-id="ce343-146">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="ce343-147">如果有任一個`asp-controller`或`asp-action`未指定，則相同的預設處理後，因為處於`asp-route`屬性。</span><span class="sxs-lookup"><span data-stu-id="ce343-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="ce343-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="ce343-148">asp-route</span></span>

<span data-ttu-id="ce343-149">`asp-route`可用來建立直接連結到具名路由的 URL。</span><span class="sxs-lookup"><span data-stu-id="ce343-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="ce343-150">使用路由的屬性，路由可以具名中所示`SpeakerController`和用於其`Evaluations`方法。</span><span class="sxs-lookup"><span data-stu-id="ce343-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="ce343-151">`Name = "speakerevals"`會告知錨定標記協助程式，來產生該控制器方法，使用 URL 直接路由`/Speaker/Evaluations`。</span><span class="sxs-lookup"><span data-stu-id="ce343-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="ce343-152">如果`asp-controller`或`asp-action`指定除了`asp-route`，產生的路由可能不如預期。</span><span class="sxs-lookup"><span data-stu-id="ce343-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="ce343-153">`asp-route`不應該使用屬性的其中一種`asp-controller`或`asp-action`為避免發生路由衝突。</span><span class="sxs-lookup"><span data-stu-id="ce343-153">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="ce343-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="ce343-154">asp-all-route-data</span></span>

<span data-ttu-id="ce343-155">`asp-all-route-data`允許建立所在的索引鍵是參數名稱和值是該索引鍵相關聯的值的機碼值組的字典。</span><span class="sxs-lookup"><span data-stu-id="ce343-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="ce343-156">如下範例所示，內嵌字典會建立並將資料傳遞到 razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="ce343-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="ce343-157">或者，資料無法傳遞入您的模型。</span><span class="sxs-lookup"><span data-stu-id="ce343-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="ce343-158">上述程式碼會產生下列 URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="ce343-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="ce343-159">按一下連結時，控制器方法`EvaluationsCurrent`呼叫。</span><span class="sxs-lookup"><span data-stu-id="ce343-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="ce343-160">因為該控制器有兩個字串參數相符項目已從建立呼叫`asp-all-route-data`字典。</span><span class="sxs-lookup"><span data-stu-id="ce343-160">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="ce343-161">如果任何索引鍵字典比對路由的參數，這些值將會由適當地路由和其他不相符的值將會產生做為要求參數。</span><span class="sxs-lookup"><span data-stu-id="ce343-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="ce343-162">asp 片段</span><span class="sxs-lookup"><span data-stu-id="ce343-162">asp-fragment</span></span>

<span data-ttu-id="ce343-163">`asp-fragment`定義要附加至 URL 的 URL 片段。</span><span class="sxs-lookup"><span data-stu-id="ce343-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="ce343-164">錨定標記協助程式，將雜湊字元 （#）。</span><span class="sxs-lookup"><span data-stu-id="ce343-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="ce343-165">如果您建立的標記：</span><span class="sxs-lookup"><span data-stu-id="ce343-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="ce343-166">將產生的 URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="ce343-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="ce343-167">建置用戶端應用程式時，適合使用雜湊標記。</span><span class="sxs-lookup"><span data-stu-id="ce343-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="ce343-168">它們可以用於簡單的標記，並在 JavaScript 中，例如搜尋。</span><span class="sxs-lookup"><span data-stu-id="ce343-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="ce343-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="ce343-169">asp-area</span></span>

<span data-ttu-id="ce343-170">`asp-area`設定 ASP.NET Core 使用來設定適當的路由的區域名稱。</span><span class="sxs-lookup"><span data-stu-id="ce343-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="ce343-171">以下是如何區域屬性會造成重新對應路由的範例。</span><span class="sxs-lookup"><span data-stu-id="ce343-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="ce343-172">設定`asp-area`部落格的前置詞目錄`Areas/Blogs`路由相關聯的控制器與檢視此錨定標記。</span><span class="sxs-lookup"><span data-stu-id="ce343-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="ce343-173">專案名稱</span><span class="sxs-lookup"><span data-stu-id="ce343-173">Project name</span></span>
  * <span data-ttu-id="ce343-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="ce343-174">wwwroot</span></span>
  * <span data-ttu-id="ce343-175">區域</span><span class="sxs-lookup"><span data-stu-id="ce343-175">Areas</span></span>
    * <span data-ttu-id="ce343-176">部落格</span><span class="sxs-lookup"><span data-stu-id="ce343-176">Blogs</span></span>
      * <span data-ttu-id="ce343-177">控制站</span><span class="sxs-lookup"><span data-stu-id="ce343-177">Controllers</span></span>
        * <span data-ttu-id="ce343-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="ce343-178">HomeController.cs</span></span>
      * <span data-ttu-id="ce343-179">檢視</span><span class="sxs-lookup"><span data-stu-id="ce343-179">Views</span></span>
        * <span data-ttu-id="ce343-180">首頁</span><span class="sxs-lookup"><span data-stu-id="ce343-180">Home</span></span>
          * <span data-ttu-id="ce343-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ce343-181">Index.cshtml</span></span>
          * <span data-ttu-id="ce343-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="ce343-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="ce343-183">控制站</span><span class="sxs-lookup"><span data-stu-id="ce343-183">Controllers</span></span>

<span data-ttu-id="ce343-184">指定有效的例如區域標記```area="Blogs"```參考時```AboutBlog.cshtml```檔案將會如下所示使用錨定標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ce343-184">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="ce343-185">產生的 HTML 將包含區域區段，並會顯示如下：</span><span class="sxs-lookup"><span data-stu-id="ce343-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="ce343-186">MVC web 應用程式中工作的區域，如果它存在路由範本時，必須包含區域的參考。</span><span class="sxs-lookup"><span data-stu-id="ce343-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="ce343-187">該範本，也就是第二個參數的`routes.MapRoute`呼叫方法時，將會顯示為：`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="ce343-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="ce343-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="ce343-188">asp-protocol</span></span>

<span data-ttu-id="ce343-189">`asp-protocol`可用於指定通訊協定 (例如`https`) 在您的 URL。</span><span class="sxs-lookup"><span data-stu-id="ce343-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="ce343-190">範例包含通訊協定的錨定標記協助程式看起來如下：</span><span class="sxs-lookup"><span data-stu-id="ce343-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="ce343-191">而且，將會產生 HTML，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ce343-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="ce343-192">此範例中的網域為 localhost，但產生 URL 時，錨定標記協助程式會使用網站的公用網域。</span><span class="sxs-lookup"><span data-stu-id="ce343-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce343-193">其他資源</span><span class="sxs-lookup"><span data-stu-id="ce343-193">Additional resources</span></span>

* [<span data-ttu-id="ce343-194">區域</span><span class="sxs-lookup"><span data-stu-id="ce343-194">Areas</span></span>](xref:mvc/controllers/areas)
