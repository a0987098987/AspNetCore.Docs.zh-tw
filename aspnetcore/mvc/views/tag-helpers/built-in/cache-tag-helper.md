---
title: "快取中 ASP.NET Core MVC 標記協助程式"
author: pkellner
description: "示範如何使用快取標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="846d5-103">快取中 ASP.NET Core MVC 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="846d5-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="846d5-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="846d5-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="846d5-105">快取標記協助程式提供了可大幅改善其內部的 ASP.NET Core 快取提供者的內容快取的 ASP.NET Core 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="846d5-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="846d5-106">Razor 檢視引擎設定的預設`expires-after`為 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="846d5-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="846d5-107">下列的 Razor 標記會快取的日期/時間：</span><span class="sxs-lookup"><span data-stu-id="846d5-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="846d5-108">包含頁面的第一個要求`CacheTagHelper`會顯示目前的日期/時間。</span><span class="sxs-lookup"><span data-stu-id="846d5-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="846d5-109">其他要求會顯示快取的值，直到快取到期 （預設值 20 分鐘），或者由記憶體不足的壓力移出記憶體。</span><span class="sxs-lookup"><span data-stu-id="846d5-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="846d5-110">您可以設定快取期間具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="846d5-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="846d5-111">快取標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="846d5-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="846d5-112">enabled</span><span class="sxs-lookup"><span data-stu-id="846d5-112">enabled</span></span>    


| <span data-ttu-id="846d5-113">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-113">Attribute Type</span></span>    | <span data-ttu-id="846d5-114">有效值</span><span class="sxs-lookup"><span data-stu-id="846d5-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="846d5-115">boolean</span><span class="sxs-lookup"><span data-stu-id="846d5-115">boolean</span></span>           | <span data-ttu-id="846d5-116">"true"（預設值）</span><span class="sxs-lookup"><span data-stu-id="846d5-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="846d5-117">"false"</span><span class="sxs-lookup"><span data-stu-id="846d5-117">"false"</span></span>   |


<span data-ttu-id="846d5-118">決定是否要快取內容快取標記協助程式 」 所括住。</span><span class="sxs-lookup"><span data-stu-id="846d5-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="846d5-119">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="846d5-119">The default is `true`.</span></span>  <span data-ttu-id="846d5-120">如果設定為`false`此快取標記協助程式不會影響快取上轉譯的輸出。</span><span class="sxs-lookup"><span data-stu-id="846d5-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="846d5-121">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="846d5-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="846d5-122">expires-on</span></span> 

| <span data-ttu-id="846d5-123">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-123">Attribute Type</span></span>    | <span data-ttu-id="846d5-124">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="846d5-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="846d5-125">DateTimeOffset</span></span>    | <span data-ttu-id="846d5-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="846d5-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="846d5-127">設定絕對到期日。</span><span class="sxs-lookup"><span data-stu-id="846d5-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="846d5-128">下列範例將快取的快取標記協助程式內容，直到 2025 年 1 月 29，下午 5:02。</span><span class="sxs-lookup"><span data-stu-id="846d5-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="846d5-129">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="846d5-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="846d5-130">expires-after</span></span>

| <span data-ttu-id="846d5-131">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-131">Attribute Type</span></span>    | <span data-ttu-id="846d5-132">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="846d5-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="846d5-133">TimeSpan</span></span>    | <span data-ttu-id="846d5-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="846d5-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="846d5-135">從快取內容的第一個要求時間設定時間的長度。</span><span class="sxs-lookup"><span data-stu-id="846d5-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="846d5-136">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="846d5-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="846d5-137">expires-sliding</span></span>

| <span data-ttu-id="846d5-138">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-138">Attribute Type</span></span>    | <span data-ttu-id="846d5-139">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="846d5-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="846d5-140">TimeSpan</span></span>    | <span data-ttu-id="846d5-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="846d5-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="846d5-142">設定應該收回快取項目，如果它尚未被存取的時間。</span><span class="sxs-lookup"><span data-stu-id="846d5-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="846d5-143">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="846d5-144">改變-依標頭</span><span class="sxs-lookup"><span data-stu-id="846d5-144">vary-by-header</span></span>

| <span data-ttu-id="846d5-145">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-145">Attribute Type</span></span>    | <span data-ttu-id="846d5-146">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-147">String</span><span class="sxs-lookup"><span data-stu-id="846d5-147">String</span></span>            | <span data-ttu-id="846d5-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="846d5-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="846d5-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="846d5-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="846d5-150">接受單一標頭值或其變更時觸發快取重新整理的標頭值的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="846d5-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="846d5-151">下列範例會監視的標頭值`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="846d5-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="846d5-152">此範例會將快取內容的每個不同`User-Agent`呈現給 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="846d5-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="846d5-153">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="846d5-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="846d5-154">vary-by-query</span></span>

| <span data-ttu-id="846d5-155">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-155">Attribute Type</span></span>    | <span data-ttu-id="846d5-156">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-157">String</span><span class="sxs-lookup"><span data-stu-id="846d5-157">String</span></span>            | <span data-ttu-id="846d5-158">「 讓 」</span><span class="sxs-lookup"><span data-stu-id="846d5-158">"Make"</span></span>                |
|                   | <span data-ttu-id="846d5-159">「 請模型 」</span><span class="sxs-lookup"><span data-stu-id="846d5-159">"Make,Model"</span></span> |

<span data-ttu-id="846d5-160">接受單一標頭值或以逗號分隔清單的標頭值變更時觸發快取重新整理的標頭值。</span><span class="sxs-lookup"><span data-stu-id="846d5-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="846d5-161">下列範例會查看值`Make`和`Model`。</span><span class="sxs-lookup"><span data-stu-id="846d5-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="846d5-162">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="846d5-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="846d5-163">vary-by-route</span></span>

| <span data-ttu-id="846d5-164">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-164">Attribute Type</span></span>    | <span data-ttu-id="846d5-165">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-166">String</span><span class="sxs-lookup"><span data-stu-id="846d5-166">String</span></span>            | <span data-ttu-id="846d5-167">「 讓 」</span><span class="sxs-lookup"><span data-stu-id="846d5-167">"Make"</span></span>                |
|                   | <span data-ttu-id="846d5-168">「 請模型 」</span><span class="sxs-lookup"><span data-stu-id="846d5-168">"Make,Model"</span></span> |

<span data-ttu-id="846d5-169">接受單一標頭值或路由資料的參數值變更時觸發快取重新整理的標頭值的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="846d5-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="846d5-170">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-170">Example:</span></span>

<span data-ttu-id="846d5-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="846d5-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="846d5-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="846d5-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="846d5-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="846d5-173">vary-by-cookie</span></span>

| <span data-ttu-id="846d5-174">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-174">Attribute Type</span></span>    | <span data-ttu-id="846d5-175">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-176">String</span><span class="sxs-lookup"><span data-stu-id="846d5-176">String</span></span>            | <span data-ttu-id="846d5-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="846d5-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="846d5-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="846d5-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="846d5-179">接受單一標頭值或標頭值 (s) 變更時觸發快取重新整理的標頭值的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="846d5-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="846d5-180">下列範例會查看與 ASP.NET 識別相關聯的 cookie。</span><span class="sxs-lookup"><span data-stu-id="846d5-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="846d5-181">當使用者通過驗證設定要求 cookie 觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="846d5-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="846d5-182">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="846d5-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="846d5-183">vary-by-user</span></span>

| <span data-ttu-id="846d5-184">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-184">Attribute Type</span></span>    | <span data-ttu-id="846d5-185">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="846d5-186">Boolean</span></span>             | <span data-ttu-id="846d5-187">"true"</span><span class="sxs-lookup"><span data-stu-id="846d5-187">"true"</span></span>                  |
|                     | <span data-ttu-id="846d5-188">"false"（預設值）</span><span class="sxs-lookup"><span data-stu-id="846d5-188">"false" (default)</span></span> |

<span data-ttu-id="846d5-189">指定已登入使用者 （或內容主體） 變更時，應該重設快取。</span><span class="sxs-lookup"><span data-stu-id="846d5-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="846d5-190">目前的使用者就是所謂的要求內容主體，而且可以藉由參考檢視 Razor 檢視`@User.Identity.Name`。</span><span class="sxs-lookup"><span data-stu-id="846d5-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="846d5-191">下列範例會查看目前的登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="846d5-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="846d5-192">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="846d5-193">使用這個屬性會維護透過在登入和登出循環的快取中的內容。</span><span class="sxs-lookup"><span data-stu-id="846d5-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="846d5-194">當使用`vary-by-user="true"`，登入和登出的動作會導致無效的快取驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="846d5-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="846d5-195">快取無效，因為登入，產生新的唯一的 cookie 值。</span><span class="sxs-lookup"><span data-stu-id="846d5-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="846d5-196">快取保留匿名狀態時沒有任何 cookie，或已過期。</span><span class="sxs-lookup"><span data-stu-id="846d5-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="846d5-197">這表示如果沒有使用者登入，仍會維護快取。</span><span class="sxs-lookup"><span data-stu-id="846d5-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="846d5-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="846d5-198">vary-by</span></span>

| <span data-ttu-id="846d5-199">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-199">Attribute Type</span></span>    | <span data-ttu-id="846d5-200">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-201">String</span><span class="sxs-lookup"><span data-stu-id="846d5-201">String</span></span>             | <span data-ttu-id="846d5-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="846d5-202">"@Model"</span></span>                 |


<span data-ttu-id="846d5-203">可讓您自訂的哪些資料取得快取。</span><span class="sxs-lookup"><span data-stu-id="846d5-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="846d5-204">更新屬性的字串值的變更，快取標記協助程式的內容所參考的物件時。</span><span class="sxs-lookup"><span data-stu-id="846d5-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="846d5-205">通常模型值的字串串連會指派給這個屬性。</span><span class="sxs-lookup"><span data-stu-id="846d5-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="846d5-206">實際上，這表示，串連的值的任何更新的快取失效。</span><span class="sxs-lookup"><span data-stu-id="846d5-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="846d5-207">下列範例假設控制器方法轉譯檢視加總兩個路由參數的整數值`myParam1`和`myParam2`，並傳回，當做單一模型屬性。</span><span class="sxs-lookup"><span data-stu-id="846d5-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="846d5-208">當這個總和變更時，快取標記協助程式的內容轉譯，而且一次快取。</span><span class="sxs-lookup"><span data-stu-id="846d5-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="846d5-209">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-209">Example:</span></span>

<span data-ttu-id="846d5-210">動作：</span><span class="sxs-lookup"><span data-stu-id="846d5-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="846d5-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="846d5-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="846d5-212">priority</span><span class="sxs-lookup"><span data-stu-id="846d5-212">priority</span></span>

| <span data-ttu-id="846d5-213">屬性類型</span><span class="sxs-lookup"><span data-stu-id="846d5-213">Attribute Type</span></span>    | <span data-ttu-id="846d5-214">範例值</span><span class="sxs-lookup"><span data-stu-id="846d5-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="846d5-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="846d5-215">CacheItemPriority</span></span>  | <span data-ttu-id="846d5-216">「 高 」</span><span class="sxs-lookup"><span data-stu-id="846d5-216">"High"</span></span>                   |
|                    | <span data-ttu-id="846d5-217">「 低 」</span><span class="sxs-lookup"><span data-stu-id="846d5-217">"Low"</span></span> |
|                    | <span data-ttu-id="846d5-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="846d5-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="846d5-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="846d5-219">"Normal"</span></span> |

<span data-ttu-id="846d5-220">提供了內建的快取提供者快取收回的指引。</span><span class="sxs-lookup"><span data-stu-id="846d5-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="846d5-221">網頁伺服器將會收回`Low`時更新快取項目第一次記憶體不足壓力下。</span><span class="sxs-lookup"><span data-stu-id="846d5-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="846d5-222">範例：</span><span class="sxs-lookup"><span data-stu-id="846d5-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="846d5-223">`priority`屬性並不保證快取保留特定層級。</span><span class="sxs-lookup"><span data-stu-id="846d5-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="846d5-224">`CacheItemPriority`是只是建議。</span><span class="sxs-lookup"><span data-stu-id="846d5-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="846d5-225">將此屬性設定為`NeverRemove`並不保證一定會保留在快取。</span><span class="sxs-lookup"><span data-stu-id="846d5-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="846d5-226">請參閱[其他資源](#additional-resources)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="846d5-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="846d5-227">快取標記協助程式是依賴[記憶體快取服務](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="846d5-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="846d5-228">如果尚未加入快取標記協助程式就會加入服務。</span><span class="sxs-lookup"><span data-stu-id="846d5-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="846d5-229">其他資源</span><span class="sxs-lookup"><span data-stu-id="846d5-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
