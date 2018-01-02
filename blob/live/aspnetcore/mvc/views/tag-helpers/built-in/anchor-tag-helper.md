---
title: "錨定標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用錨定標記協助程式"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 503ad7c4ce8c4f08b2a06dbe9f985566f54d3ca2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2017
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="e7e75-104">錨定標記協助程式</span><span class="sxs-lookup"><span data-stu-id="e7e75-104">Anchor Tag Helper</span></span>

<span data-ttu-id="e7e75-105">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="e7e75-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="e7e75-106">錨定標記協助程式增強 HTML 錨定 (`<a ... ></a>`) 藉由加入新屬性的標記。</span><span class="sxs-lookup"><span data-stu-id="e7e75-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="e7e75-107">所產生的連結 (在`href`標記) 會建立使用新的屬性。</span><span class="sxs-lookup"><span data-stu-id="e7e75-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="e7e75-108">該 URL 可以包含選擇性通訊協定，例如 https。</span><span class="sxs-lookup"><span data-stu-id="e7e75-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="e7e75-109">在這份文件中的範例中用於下列的喇叭控制器。</span><span class="sxs-lookup"><span data-stu-id="e7e75-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="e7e75-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="e7e75-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="e7e75-111">錨定標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="e7e75-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="e7e75-112">asp 控制器</span><span class="sxs-lookup"><span data-stu-id="e7e75-112">asp-controller</span></span>

<span data-ttu-id="e7e75-113">`asp-controller`用來建立關聯的控制器將會用來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="e7e75-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="e7e75-114">指定的控制器必須存在於目前的專案。</span><span class="sxs-lookup"><span data-stu-id="e7e75-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="e7e75-115">下列程式碼列出所有的喇叭：</span><span class="sxs-lookup"><span data-stu-id="e7e75-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="e7e75-116">將產生的標記：</span><span class="sxs-lookup"><span data-stu-id="e7e75-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="e7e75-117">如果`asp-controller`指定和`asp-action`不是，預設值`asp-action`將目前正在執行檢視的預設控制器方法。</span><span class="sxs-lookup"><span data-stu-id="e7e75-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="e7e75-118">是，在上述範例中，如果`asp-action`被省略，而且此錨定標記協助程式從產生的*HomeController*的`Index`檢視 (**/首頁**)，將會產生的標記：</span><span class="sxs-lookup"><span data-stu-id="e7e75-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="e7e75-119">asp 動作</span><span class="sxs-lookup"><span data-stu-id="e7e75-119">asp-action</span></span>

<span data-ttu-id="e7e75-120">`asp-action`將會包含控制器動作方法的名稱在產生`href`。</span><span class="sxs-lookup"><span data-stu-id="e7e75-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="e7e75-121">例如，下列程式碼產生設定`href`指向喇叭詳細資料頁面：</span><span class="sxs-lookup"><span data-stu-id="e7e75-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="e7e75-122">將產生的標記：</span><span class="sxs-lookup"><span data-stu-id="e7e75-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="e7e75-123">如果沒有`asp-controller`屬性已指定，會使用預設的控制站呼叫執行目前檢視的檢視。</span><span class="sxs-lookup"><span data-stu-id="e7e75-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="e7e75-124">如果屬性`asp-action`是`Index`，然後採取任何動作會不附加至的 URL，導致預設`Index`所呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="e7e75-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="e7e75-125">動作指定 （或預設），必須在控制器中參考`asp-controller`。</span><span class="sxs-lookup"><span data-stu-id="e7e75-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="e7e75-126">asp 網頁</span><span class="sxs-lookup"><span data-stu-id="e7e75-126">asp-page</span></span>

<span data-ttu-id="e7e75-127">使用`asp-page`屬性中設定其 URL，以指向特定頁面的錨點標籤。</span><span class="sxs-lookup"><span data-stu-id="e7e75-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="e7e75-128">以正斜線的頁面名稱前面加上"/"來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="e7e75-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="e7e75-129">下面範例中的 URL 會指向目前的目錄中的"喇叭 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="e7e75-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="e7e75-130">`asp-page`屬性在上述程式碼範例中的呈現 HTML 輸出中的檢視，類似於下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="e7e75-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```
<span data-ttu-id="e7e75-131">cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">檢視喇叭</a></span><span class="sxs-lookup"><span data-stu-id="e7e75-131">cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a></span></span>
```

The `asp-route-id` produces the following output:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```


### <a name="asp-route-value"></a><span data-ttu-id="e7e75-132">asp-路由的 {value}</span><span class="sxs-lookup"><span data-stu-id="e7e75-132">asp-route-{value}</span></span>

<span data-ttu-id="e7e75-133">`asp-route-`是萬用字元路由前置詞。</span><span class="sxs-lookup"><span data-stu-id="e7e75-133">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="e7e75-134">尾端的虛線會被解譯為潛在的路由參數之後，您將任何值。</span><span class="sxs-lookup"><span data-stu-id="e7e75-134">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="e7e75-135">如果找不到預設路由，此路由前置字元會附加至要求參數和值為產生的 href。</span><span class="sxs-lookup"><span data-stu-id="e7e75-135">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="e7e75-136">否則將會取代中的路由範本。</span><span class="sxs-lookup"><span data-stu-id="e7e75-136">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="e7e75-137">假設您有控制器方法定義，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e7e75-137">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="e7e75-138">而且有預設路由範本中定義您*Startup.cs* ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e7e75-138">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="e7e75-139">**Cshtml**檔案，其中包含錨定標記協助程式需要使用**喇叭**傳入從控制器到檢視的模型參數如下所示：</span><span class="sxs-lookup"><span data-stu-id="e7e75-139">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="e7e75-140">產生的 HTML 會如下所示因為**識別碼**預設路由中找不到。</span><span class="sxs-lookup"><span data-stu-id="e7e75-140">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="e7e75-141">如果路由前置字元不是找到的路由範本的一部分，也就是以下列**cshtml**檔案：</span><span class="sxs-lookup"><span data-stu-id="e7e75-141">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="e7e75-142">產生的 HTML 會如下所示因為**speakerid**找不到相符的路由中：</span><span class="sxs-lookup"><span data-stu-id="e7e75-142">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="e7e75-143">如果有任一個`asp-controller`或`asp-action`未指定，則相同的預設處理後，因為處於`asp-route`屬性。</span><span class="sxs-lookup"><span data-stu-id="e7e75-143">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="e7e75-144">asp 路由</span><span class="sxs-lookup"><span data-stu-id="e7e75-144">asp-route</span></span>

<span data-ttu-id="e7e75-145">`asp-route`可用來建立直接連結到具名路由的 URL。</span><span class="sxs-lookup"><span data-stu-id="e7e75-145">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="e7e75-146">使用路由的屬性，路由可以具名中所示`SpeakerController`和用於其`Evaluations`方法。</span><span class="sxs-lookup"><span data-stu-id="e7e75-146">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="e7e75-147">`Name = "speakerevals"`會告知錨定標記協助程式，來產生該控制器方法，使用 URL 直接路由`/Speaker/Evaluations`。</span><span class="sxs-lookup"><span data-stu-id="e7e75-147">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="e7e75-148">如果`asp-controller`或`asp-action`指定除了`asp-route`，產生的路由可能不如預期。</span><span class="sxs-lookup"><span data-stu-id="e7e75-148">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="e7e75-149">`asp-route`不應該使用屬性的其中一種`asp-controller`或`asp-action`為避免發生路由衝突。</span><span class="sxs-lookup"><span data-stu-id="e7e75-149">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="e7e75-150">asp all-路由資料</span><span class="sxs-lookup"><span data-stu-id="e7e75-150">asp-all-route-data</span></span>

<span data-ttu-id="e7e75-151">`asp-all-route-data`允許建立所在的索引鍵是參數名稱和值是該索引鍵相關聯的值的機碼值組的字典。</span><span class="sxs-lookup"><span data-stu-id="e7e75-151">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="e7e75-152">如下範例所示，內嵌字典會建立並將資料傳遞到 razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="e7e75-152">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="e7e75-153">或者，資料無法傳遞入您的模型。</span><span class="sxs-lookup"><span data-stu-id="e7e75-153">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="e7e75-154">上述程式碼會產生下列 URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="e7e75-154">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="e7e75-155">按一下連結時，控制器方法`EvaluationsCurrent`呼叫。</span><span class="sxs-lookup"><span data-stu-id="e7e75-155">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="e7e75-156">因為該控制器有兩個字串參數相符項目已從建立呼叫`asp-all-route-data`字典。</span><span class="sxs-lookup"><span data-stu-id="e7e75-156">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="e7e75-157">如果任何索引鍵字典比對路由的參數，這些值將會由適當地路由和其他不相符的值將會產生做為要求參數。</span><span class="sxs-lookup"><span data-stu-id="e7e75-157">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="e7e75-158">asp 片段</span><span class="sxs-lookup"><span data-stu-id="e7e75-158">asp-fragment</span></span>

<span data-ttu-id="e7e75-159">`asp-fragment`定義要附加至 URL 的 URL 片段。</span><span class="sxs-lookup"><span data-stu-id="e7e75-159">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="e7e75-160">錨定標記協助程式，將雜湊字元 （#）。</span><span class="sxs-lookup"><span data-stu-id="e7e75-160">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="e7e75-161">如果您建立的標記：</span><span class="sxs-lookup"><span data-stu-id="e7e75-161">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="e7e75-162">將產生的 URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="e7e75-162">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="e7e75-163">建置用戶端應用程式時，適合使用雜湊標記。</span><span class="sxs-lookup"><span data-stu-id="e7e75-163">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="e7e75-164">它們可以用於簡單的標記，並在 JavaScript 中，例如搜尋。</span><span class="sxs-lookup"><span data-stu-id="e7e75-164">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="e7e75-165">asp 區域</span><span class="sxs-lookup"><span data-stu-id="e7e75-165">asp-area</span></span>

<span data-ttu-id="e7e75-166">`asp-area`設定 ASP.NET Core 使用來設定適當的路由的區域名稱。</span><span class="sxs-lookup"><span data-stu-id="e7e75-166">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="e7e75-167">以下是如何區域屬性會造成重新對應路由的範例。</span><span class="sxs-lookup"><span data-stu-id="e7e75-167">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="e7e75-168">設定`asp-area`部落格的前置詞目錄`Areas/Blogs`路由相關聯的控制器與檢視此錨定標記。</span><span class="sxs-lookup"><span data-stu-id="e7e75-168">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="e7e75-169">專案名稱</span><span class="sxs-lookup"><span data-stu-id="e7e75-169">Project name</span></span>
  * <span data-ttu-id="e7e75-170">wwwroot</span><span class="sxs-lookup"><span data-stu-id="e7e75-170">wwwroot</span></span>
  * <span data-ttu-id="e7e75-171">區域</span><span class="sxs-lookup"><span data-stu-id="e7e75-171">Areas</span></span>
    * <span data-ttu-id="e7e75-172">部落格</span><span class="sxs-lookup"><span data-stu-id="e7e75-172">Blogs</span></span>
      * <span data-ttu-id="e7e75-173">控制站</span><span class="sxs-lookup"><span data-stu-id="e7e75-173">Controllers</span></span>
        * <span data-ttu-id="e7e75-174">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="e7e75-174">HomeController.cs</span></span>
      * <span data-ttu-id="e7e75-175">檢視</span><span class="sxs-lookup"><span data-stu-id="e7e75-175">Views</span></span>
        * <span data-ttu-id="e7e75-176">首頁</span><span class="sxs-lookup"><span data-stu-id="e7e75-176">Home</span></span>
          * <span data-ttu-id="e7e75-177">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e7e75-177">Index.cshtml</span></span>
          * <span data-ttu-id="e7e75-178">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="e7e75-178">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="e7e75-179">控制站</span><span class="sxs-lookup"><span data-stu-id="e7e75-179">Controllers</span></span>

<span data-ttu-id="e7e75-180">指定有效的例如區域標記```area="Blogs"```參考時```AboutBlog.cshtml```檔案將會如下所示使用錨定標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="e7e75-180">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="e7e75-181">產生的 HTML 將包含區域區段，並會顯示如下：</span><span class="sxs-lookup"><span data-stu-id="e7e75-181">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="e7e75-182">MVC web 應用程式中工作的區域，如果它存在路由範本時，必須包含區域的參考。</span><span class="sxs-lookup"><span data-stu-id="e7e75-182">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="e7e75-183">該範本，也就是第二個參數的`routes.MapRoute`呼叫方法時，將會顯示為：`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="e7e75-183">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="e7e75-184">asp 通訊協定</span><span class="sxs-lookup"><span data-stu-id="e7e75-184">asp-protocol</span></span>

<span data-ttu-id="e7e75-185">`asp-protocol`可用於指定通訊協定 (例如`https`) 在您的 URL。</span><span class="sxs-lookup"><span data-stu-id="e7e75-185">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="e7e75-186">範例包含通訊協定的錨定標記協助程式看起來如下：</span><span class="sxs-lookup"><span data-stu-id="e7e75-186">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="e7e75-187">而且，將會產生 HTML，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e7e75-187">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="e7e75-188">此範例中的網域為 localhost，但產生 URL 時，錨定標記協助程式會使用網站的公用網域。</span><span class="sxs-lookup"><span data-stu-id="e7e75-188">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7e75-189">其他資源</span><span class="sxs-lookup"><span data-stu-id="e7e75-189">Additional resources</span></span>

* [<span data-ttu-id="e7e75-190">區域</span><span class="sxs-lookup"><span data-stu-id="e7e75-190">Areas</span></span>](xref:mvc/controllers/areas)
