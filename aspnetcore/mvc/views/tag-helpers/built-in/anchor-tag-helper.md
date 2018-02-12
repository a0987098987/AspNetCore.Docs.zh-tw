---
title: "ASP.NET Core 中的錨點標籤協助程式"
author: pkellner
description: "示範如何使用錨點標籤協助程式"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="bf103-103">錨點標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="bf103-103">Anchor Tag Helper</span></span>

<span data-ttu-id="bf103-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="bf103-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="bf103-105">錨點標籤協助程式藉由新增新的屬性，來增強 HTML 錨點 (`<a ... ></a>`) 標籤。</span><span class="sxs-lookup"><span data-stu-id="bf103-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="bf103-106">產生的連結 (在 `href` 標籤上) 是使用新的屬性所建立。</span><span class="sxs-lookup"><span data-stu-id="bf103-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="bf103-107">該 URL 可包含選擇性通訊協定，例如 https。</span><span class="sxs-lookup"><span data-stu-id="bf103-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="bf103-108">本文件的所有範例中都會使用下列喇叭控制器。</span><span class="sxs-lookup"><span data-stu-id="bf103-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="bf103-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="bf103-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="bf103-110">錨點標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="bf103-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="bf103-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="bf103-111">asp-controller</span></span>

<span data-ttu-id="bf103-112">`asp-controller` 可用來與用於產生 URL 的控制器建立關聯。</span><span class="sxs-lookup"><span data-stu-id="bf103-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="bf103-113">指定的控制器必須存在於目前的專案中。</span><span class="sxs-lookup"><span data-stu-id="bf103-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="bf103-114">下列程式碼列出所有喇叭：</span><span class="sxs-lookup"><span data-stu-id="bf103-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="bf103-115">產生的標記會是：</span><span class="sxs-lookup"><span data-stu-id="bf103-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="bf103-116">如果指定了 `asp-controller` 但未指定 `asp-action`，預設的 `asp-action` 會是目前執行檢視的預設控制器方法。</span><span class="sxs-lookup"><span data-stu-id="bf103-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="bf103-117">換句話說，在上述範例中，如果省略 `asp-action`，而且此錨點標籤協助程式是從 *HomeController* 的 `Index` 檢視 (**/Home**) 所產生，則產生的標記會是：</span><span class="sxs-lookup"><span data-stu-id="bf103-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="bf103-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="bf103-118">asp-action</span></span>

<span data-ttu-id="bf103-119">`asp-action` 是控制器中的動作方法名稱，將會包含在產生的 `href` 中。</span><span class="sxs-lookup"><span data-stu-id="bf103-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="bf103-120">例如，下列程式碼會設定產生的 `href` 以指向喇叭詳細資料頁面：</span><span class="sxs-lookup"><span data-stu-id="bf103-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="bf103-121">產生的標記會是：</span><span class="sxs-lookup"><span data-stu-id="bf103-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="bf103-122">如果未指定任何 `asp-controller` 屬性，則會使用呼叫檢視並執行目前檢視的預設控制器。</span><span class="sxs-lookup"><span data-stu-id="bf103-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="bf103-123">如果屬性 `asp-action` 為 `Index`，則不會將任何動作附加至 URL，而導致呼叫預設的 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="bf103-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="bf103-124">指定的動作 (或預設的動作) 必須存在於 `asp-controller` 所參考的控制器中。</span><span class="sxs-lookup"><span data-stu-id="bf103-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="bf103-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="bf103-125">asp-page</span></span>

<span data-ttu-id="bf103-126">在錨點標籤中使用 `asp-page` 屬性可設定其 URL 以指向特定頁面。</span><span class="sxs-lookup"><span data-stu-id="bf103-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="bf103-127">在頁面名稱前面加上正斜線 "/" 以建立 URL。</span><span class="sxs-lookup"><span data-stu-id="bf103-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="bf103-128">下面範例中的 URL 會指向目前目錄中的「喇叭」頁面。</span><span class="sxs-lookup"><span data-stu-id="bf103-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="bf103-129">上述程式碼範例中的 `asp-page` 屬性會在檢視中轉譯 HTML 輸出，如下列程式碼片段所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="bf103-130">`asp-page` 屬性與 `asp-route`、`asp-controller` 以及 `asp-action` 屬性互斥。</span><span class="sxs-lookup"><span data-stu-id="bf103-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="bf103-131">不過，`asp-page` 可以搭配 `asp-route-id` 來控制路由，如下列程式碼範例所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="bf103-132">`asp-route-id` 會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="bf103-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="bf103-133">若要在 Razor 頁面中使用 `asp-page` 屬性，URL 必須是相對路徑，例如 `"./Speaker"`。</span><span class="sxs-lookup"><span data-stu-id="bf103-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="bf103-134">`asp-page` 屬性中的相對路徑無法在 MVC 檢視中使用。</span><span class="sxs-lookup"><span data-stu-id="bf103-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="bf103-135">針對 MVC 檢視，請改用 "/" 語法。</span><span class="sxs-lookup"><span data-stu-id="bf103-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="bf103-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="bf103-136">asp-route-{value}</span></span>

<span data-ttu-id="bf103-137">`asp-route-` 是路由前置萬用字元。</span><span class="sxs-lookup"><span data-stu-id="bf103-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="bf103-138">您放在行尾破折號後面的任何值都會解譯為潛在路由參數。</span><span class="sxs-lookup"><span data-stu-id="bf103-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="bf103-139">如果找不到預設路由，則會在產生的 href 附加此路由前置字元作為要求參數和值。</span><span class="sxs-lookup"><span data-stu-id="bf103-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="bf103-140">否則會在路由範本中加以取代。</span><span class="sxs-lookup"><span data-stu-id="bf103-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="bf103-141">假設您有定義如下的控制器方法：</span><span class="sxs-lookup"><span data-stu-id="bf103-141">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="bf103-142">並在 *Startup.cs* 中定義了預設路由範本，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="bf103-143">下列 **cshtml** 檔案包含使用從控制器傳入檢視的 **speaker** 模型參數所需的錨點標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="bf103-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="bf103-144">由於預設路由中找不到 **id**，因此產生的 HTML 會如下所示。</span><span class="sxs-lookup"><span data-stu-id="bf103-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="bf103-145">如果路由前置字元不屬於所找到的路由範本，在本例中為下列 **cshtml** 檔案：</span><span class="sxs-lookup"><span data-stu-id="bf103-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="bf103-146">由於符合的路由中找不到 **speakerid**，因此產生的 HTML 會如下所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="bf103-147">如果未指定 `asp-controller` 或 `asp-action`，則會遵循與 `asp-route` 屬性相同的預設處理。</span><span class="sxs-lookup"><span data-stu-id="bf103-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="bf103-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="bf103-148">asp-route</span></span>

<span data-ttu-id="bf103-149">`asp-route` 可讓您建立直接連結至具名路由的 URL。</span><span class="sxs-lookup"><span data-stu-id="bf103-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="bf103-150">使用路由屬性，路由可以如 `SpeakerController` 中所示命名並用於其 `Evaluations` 方法。</span><span class="sxs-lookup"><span data-stu-id="bf103-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="bf103-151">`Name = "speakerevals"` 會指示錨點標籤協助程式使用 URL `/Speaker/Evaluations`，來產生該控制器方法的直接路由。</span><span class="sxs-lookup"><span data-stu-id="bf103-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="bf103-152">如果除了 `asp-route`，還指定了 `asp-controller` 或 `asp-action`，產生的路由可能不如預期。</span><span class="sxs-lookup"><span data-stu-id="bf103-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="bf103-153">`asp-route` 不應該搭配屬性 `asp-controller` 或 `asp-action` 使用，以免發生路由衝突。</span><span class="sxs-lookup"><span data-stu-id="bf103-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="bf103-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="bf103-154">asp-all-route-data</span></span>

<span data-ttu-id="bf103-155">`asp-all-route-data` 可建立索引鍵/值組的字典，其中索引鍵是參數名稱，而值是與該索引鍵建立關聯的值。</span><span class="sxs-lookup"><span data-stu-id="bf103-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="bf103-156">如下範例所示，會建立內嵌字典並將資料傳遞至 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="bf103-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="bf103-157">您也可以使用模型來傳入資料。</span><span class="sxs-lookup"><span data-stu-id="bf103-157">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="bf103-158">上述程式碼會產生下列 URL：http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="bf103-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="bf103-159">按一下連結時，會呼叫控制器方法 `EvaluationsCurrent`。</span><span class="sxs-lookup"><span data-stu-id="bf103-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="bf103-160">因為該控制器有兩個字串參數符合 `asp-all-route-data` 字典中已建立的字串參數，所以會呼叫此方法。</span><span class="sxs-lookup"><span data-stu-id="bf103-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="bf103-161">如果字典中有任何索引鍵符合路由參數，這些值會在路由中被適當取代，並會產生其他不相符的值作為要求參數。</span><span class="sxs-lookup"><span data-stu-id="bf103-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="bf103-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="bf103-162">asp-fragment</span></span>

<span data-ttu-id="bf103-163">`asp-fragment` 定義要附加至 URL 的 URL 片段。</span><span class="sxs-lookup"><span data-stu-id="bf103-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="bf103-164">錨點標籤協助程式會新增雜湊字元 (#)。</span><span class="sxs-lookup"><span data-stu-id="bf103-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="bf103-165">如果您建立一個標籤：</span><span class="sxs-lookup"><span data-stu-id="bf103-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="bf103-166">產生的 URL 會是：http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="bf103-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="bf103-167"># 標籤在建置用戶端應用程式時會很有用。</span><span class="sxs-lookup"><span data-stu-id="bf103-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="bf103-168">例如，您可以使用這些標籤在 JavaScript 中輕鬆標記和搜尋。</span><span class="sxs-lookup"><span data-stu-id="bf103-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="bf103-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="bf103-169">asp-area</span></span>

<span data-ttu-id="bf103-170">`asp-area` 會設定 ASP.NET Core 用來設定適當路由的區域名稱。</span><span class="sxs-lookup"><span data-stu-id="bf103-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="bf103-171">以下是區域屬性如何造成路由重新對應的範例。</span><span class="sxs-lookup"><span data-stu-id="bf103-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="bf103-172">將 `asp-area` 設定為 Blogs 會在 此錨點標籤的相關聯控制器和檢視路由前面加上字典 `Areas/Blogs`。</span><span class="sxs-lookup"><span data-stu-id="bf103-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="bf103-173">專案名稱</span><span class="sxs-lookup"><span data-stu-id="bf103-173">Project name</span></span>
  * <span data-ttu-id="bf103-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="bf103-174">wwwroot</span></span>
  * <span data-ttu-id="bf103-175">區域</span><span class="sxs-lookup"><span data-stu-id="bf103-175">Areas</span></span>
    * <span data-ttu-id="bf103-176">部落格</span><span class="sxs-lookup"><span data-stu-id="bf103-176">Blogs</span></span>
      * <span data-ttu-id="bf103-177">控制器</span><span class="sxs-lookup"><span data-stu-id="bf103-177">Controllers</span></span>
        * <span data-ttu-id="bf103-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="bf103-178">HomeController.cs</span></span>
      * <span data-ttu-id="bf103-179">檢視</span><span class="sxs-lookup"><span data-stu-id="bf103-179">Views</span></span>
        * <span data-ttu-id="bf103-180">首頁</span><span class="sxs-lookup"><span data-stu-id="bf103-180">Home</span></span>
          * <span data-ttu-id="bf103-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf103-181">Index.cshtml</span></span>
          * <span data-ttu-id="bf103-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="bf103-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="bf103-183">控制器</span><span class="sxs-lookup"><span data-stu-id="bf103-183">Controllers</span></span>

<span data-ttu-id="bf103-184">參考 ```AboutBlog.cshtml``` 檔案時指定有效的區域標籤 (例如 ```area="Blogs"```) 會類似使用下列的錨點標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="bf103-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="bf103-185">產生的 HTML 會包含區域區段，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="bf103-186">若要讓 MVC 區域在 Web 應用程式中正常運作，路由範本必須包含區域的參考 (若存在)。</span><span class="sxs-lookup"><span data-stu-id="bf103-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="bf103-187">該範本 (也就是 `routes.MapRoute` 方法呼叫的第二個參數) 會顯示為：`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="bf103-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="bf103-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="bf103-188">asp-protocol</span></span>

<span data-ttu-id="bf103-189">`asp-protocol` 適用於在您的 URL 中指定通訊協定 (例如 `https`)。</span><span class="sxs-lookup"><span data-stu-id="bf103-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="bf103-190">包含通訊協定的範例錨點標籤協助程式如下所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="bf103-191">並將產生 HTML，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bf103-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="bf103-192">此範例中的網域為 localhost，但錨點標籤協助程式使用網站的公用網域來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="bf103-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf103-193">其他資源</span><span class="sxs-lookup"><span data-stu-id="bf103-193">Additional resources</span></span>

* [<span data-ttu-id="bf103-194">區域</span><span class="sxs-lookup"><span data-stu-id="bf103-194">Areas</span></span>](xref:mvc/controllers/areas)
