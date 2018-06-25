---
title: ASP.NET Core MVC 中的快取標籤協助程式
author: pkellner
description: 示範如何使用快取標籤協助程式
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 969716e21211513053f52049368a0a7190ffba47
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276548"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="b501a-103">ASP.NET Core MVC 中的快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="b501a-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="b501a-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="b501a-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="b501a-105">快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至內部 ASP.NET Core 快取提供者，因此大幅提升應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="b501a-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="b501a-106">Razor 檢視引擎將預設的 `expires-after` 設定為 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="b501a-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="b501a-107">下列 Razor 標記會快取日期/時間：</span><span class="sxs-lookup"><span data-stu-id="b501a-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="b501a-108">第一個對包含 `CacheTagHelper` 之頁面的要求會顯示目前的日期/時間。</span><span class="sxs-lookup"><span data-stu-id="b501a-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="b501a-109">其他要求則會顯示在快取過期 (預設為 20 分鐘) 或因記憶體不足而撤銷之前的快取值。</span><span class="sxs-lookup"><span data-stu-id="b501a-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="b501a-110">您可以使用下列屬性來設定快取期間：</span><span class="sxs-lookup"><span data-stu-id="b501a-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="b501a-111">快取標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="b501a-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="b501a-112">enabled</span><span class="sxs-lookup"><span data-stu-id="b501a-112">enabled</span></span>    


| <span data-ttu-id="b501a-113">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-113">Attribute Type</span></span>    | <span data-ttu-id="b501a-114">有效值</span><span class="sxs-lookup"><span data-stu-id="b501a-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="b501a-115">boolean</span><span class="sxs-lookup"><span data-stu-id="b501a-115">boolean</span></span>           | <span data-ttu-id="b501a-116">"true" (預設)</span><span class="sxs-lookup"><span data-stu-id="b501a-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="b501a-117">"false"</span><span class="sxs-lookup"><span data-stu-id="b501a-117">"false"</span></span>   |


<span data-ttu-id="b501a-118">判斷是否快取以快取標籤協助程式括住的內容。</span><span class="sxs-lookup"><span data-stu-id="b501a-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="b501a-119">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="b501a-119">The default is `true`.</span></span>  <span data-ttu-id="b501a-120">如果設定為 `false`，此快取標籤協助程式將不會對轉譯輸出有任何快取影響。</span><span class="sxs-lookup"><span data-stu-id="b501a-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="b501a-121">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="b501a-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="b501a-122">expires-on</span></span> 

| <span data-ttu-id="b501a-123">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-123">Attribute Type</span></span> |           <span data-ttu-id="b501a-124">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="b501a-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b501a-125">DateTimeOffset</span></span> | <span data-ttu-id="b501a-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="b501a-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="b501a-127">設定絕對到期日。</span><span class="sxs-lookup"><span data-stu-id="b501a-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="b501a-128">下列範例會快取 2025 年 1 月 29 日下午 5:02 以前的快取標籤協助程式內容。</span><span class="sxs-lookup"><span data-stu-id="b501a-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="b501a-129">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="b501a-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="b501a-130">expires-after</span></span>

| <span data-ttu-id="b501a-131">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-131">Attribute Type</span></span> |        <span data-ttu-id="b501a-132">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="b501a-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b501a-133">TimeSpan</span></span>    | <span data-ttu-id="b501a-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="b501a-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="b501a-135">設定從第一個要求時間到快取內容的時間長度。</span><span class="sxs-lookup"><span data-stu-id="b501a-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="b501a-136">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="b501a-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="b501a-137">expires-sliding</span></span>

| <span data-ttu-id="b501a-138">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-138">Attribute Type</span></span> |        <span data-ttu-id="b501a-139">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="b501a-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b501a-140">TimeSpan</span></span>    | <span data-ttu-id="b501a-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="b501a-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="b501a-142">設定快取項目多久未被存取時應該撤銷。</span><span class="sxs-lookup"><span data-stu-id="b501a-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="b501a-143">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="b501a-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="b501a-144">vary-by-header</span></span>

| <span data-ttu-id="b501a-145">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-145">Attribute Type</span></span>    | <span data-ttu-id="b501a-146">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="b501a-147">String</span><span class="sxs-lookup"><span data-stu-id="b501a-147">String</span></span>            | <span data-ttu-id="b501a-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="b501a-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="b501a-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="b501a-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="b501a-150">接受單一標頭值或標頭值的逗號分隔清單在變更時，觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="b501a-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="b501a-151">下列範例會監視標頭值 `User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="b501a-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="b501a-152">此範例會快取展示給網頁伺服器之每個不同 `User-Agent` 的內容。</span><span class="sxs-lookup"><span data-stu-id="b501a-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="b501a-153">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="b501a-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="b501a-154">vary-by-query</span></span>

| <span data-ttu-id="b501a-155">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-155">Attribute Type</span></span>    | <span data-ttu-id="b501a-156">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="b501a-157">String</span><span class="sxs-lookup"><span data-stu-id="b501a-157">String</span></span>            | <span data-ttu-id="b501a-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="b501a-158">"Make"</span></span>                |
|                   | <span data-ttu-id="b501a-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="b501a-159">"Make,Model"</span></span> |

<span data-ttu-id="b501a-160">接受單一標頭值或標頭值的逗號分隔清單在標頭值變更時，觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="b501a-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="b501a-161">下列範例會查看 `Make` 和 `Model` 的值。</span><span class="sxs-lookup"><span data-stu-id="b501a-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="b501a-162">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="b501a-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="b501a-163">vary-by-route</span></span>

| <span data-ttu-id="b501a-164">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-164">Attribute Type</span></span>    | <span data-ttu-id="b501a-165">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="b501a-166">String</span><span class="sxs-lookup"><span data-stu-id="b501a-166">String</span></span>            | <span data-ttu-id="b501a-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="b501a-167">"Make"</span></span>                |
|                   | <span data-ttu-id="b501a-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="b501a-168">"Make,Model"</span></span> |

<span data-ttu-id="b501a-169">接受單一標頭值或標頭值的逗號分隔清單在路由資料參數值變更時，觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="b501a-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="b501a-170">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-170">Example:</span></span>

<span data-ttu-id="b501a-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="b501a-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="b501a-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b501a-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="b501a-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="b501a-173">vary-by-cookie</span></span>

| <span data-ttu-id="b501a-174">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-174">Attribute Type</span></span>    | <span data-ttu-id="b501a-175">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="b501a-176">String</span><span class="sxs-lookup"><span data-stu-id="b501a-176">String</span></span>            | <span data-ttu-id="b501a-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="b501a-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="b501a-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="b501a-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="b501a-179">接受單一標頭值或標頭值的逗號分隔清單在標頭值變更時，觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="b501a-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="b501a-180">下列範例會查看與 ASP.NET 識別建立關聯的 Cookie。</span><span class="sxs-lookup"><span data-stu-id="b501a-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="b501a-181">當使用者經過驗證時，待設定的要求 Cookie 會觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="b501a-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="b501a-182">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="b501a-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="b501a-183">vary-by-user</span></span>

| <span data-ttu-id="b501a-184">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-184">Attribute Type</span></span>    | <span data-ttu-id="b501a-185">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="b501a-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="b501a-186">Boolean</span></span>             | <span data-ttu-id="b501a-187">"true"</span><span class="sxs-lookup"><span data-stu-id="b501a-187">"true"</span></span>                  |
|                     | <span data-ttu-id="b501a-188">"false" (預設)</span><span class="sxs-lookup"><span data-stu-id="b501a-188">"false" (default)</span></span> |

<span data-ttu-id="b501a-189">指定當登入的使用者 (或內容主體) 變更時，是否應該重設快取。</span><span class="sxs-lookup"><span data-stu-id="b501a-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="b501a-190">目前的使用者也稱為要求內容主體，可在 Razor 檢視中藉由參考 `@User.Identity.Name` 進行檢視。</span><span class="sxs-lookup"><span data-stu-id="b501a-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="b501a-191">下列範例會查看目前登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="b501a-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="b501a-192">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="b501a-193">使用此屬性可保留登入再登出週期內的快取內容。</span><span class="sxs-lookup"><span data-stu-id="b501a-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="b501a-194">使用 `vary-by-user="true"` 時，經過驗證之使用者的登入再登出動作會使快取失效。</span><span class="sxs-lookup"><span data-stu-id="b501a-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="b501a-195">快取失效是由於登入時會產生新的唯一 Cookie 值所致。</span><span class="sxs-lookup"><span data-stu-id="b501a-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="b501a-196">如果沒有任何 Cookie 或 Cookie 已過期，則會以匿名狀態保留快取。</span><span class="sxs-lookup"><span data-stu-id="b501a-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="b501a-197">這表示如果沒有任何使用者登入，則會保留快取。</span><span class="sxs-lookup"><span data-stu-id="b501a-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="b501a-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="b501a-198">vary-by</span></span>

| <span data-ttu-id="b501a-199">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-199">Attribute Type</span></span> | <span data-ttu-id="b501a-200">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="b501a-201">String</span><span class="sxs-lookup"><span data-stu-id="b501a-201">String</span></span>     |    <span data-ttu-id="b501a-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="b501a-202">"@Model"</span></span>    |

<span data-ttu-id="b501a-203">可自訂要快取哪些資料。</span><span class="sxs-lookup"><span data-stu-id="b501a-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="b501a-204">當屬性字串值所參考的物件變更時，就會更新快取標籤協助程式的內容。</span><span class="sxs-lookup"><span data-stu-id="b501a-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="b501a-205">通常會對此屬性指派模型值的字串串連。</span><span class="sxs-lookup"><span data-stu-id="b501a-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="b501a-206">實際上，這表示更新任何串連值都會使快取失效。</span><span class="sxs-lookup"><span data-stu-id="b501a-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="b501a-207">下列範例假設轉譯檢視的控制器方法加總兩個路由參數 `myParam1` 和 `myParam2` 的整數值，並傳回一個模型屬性。</span><span class="sxs-lookup"><span data-stu-id="b501a-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="b501a-208">當此總和變更時，快取標籤協助程式的內容會重新轉譯和快取。</span><span class="sxs-lookup"><span data-stu-id="b501a-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="b501a-209">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-209">Example:</span></span>

<span data-ttu-id="b501a-210">動作：</span><span class="sxs-lookup"><span data-stu-id="b501a-210">Action:</span></span>

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

<span data-ttu-id="b501a-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b501a-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="b501a-212">priority</span><span class="sxs-lookup"><span data-stu-id="b501a-212">priority</span></span>

| <span data-ttu-id="b501a-213">屬性類型</span><span class="sxs-lookup"><span data-stu-id="b501a-213">Attribute Type</span></span>    | <span data-ttu-id="b501a-214">範例值</span><span class="sxs-lookup"><span data-stu-id="b501a-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="b501a-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="b501a-215">CacheItemPriority</span></span>  | <span data-ttu-id="b501a-216">"High"</span><span class="sxs-lookup"><span data-stu-id="b501a-216">"High"</span></span>                   |
|                    | <span data-ttu-id="b501a-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="b501a-217">"Low"</span></span> |
|                    | <span data-ttu-id="b501a-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="b501a-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="b501a-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="b501a-219">"Normal"</span></span> |

<span data-ttu-id="b501a-220">提供快取撤銷指引給內建快取提供者。</span><span class="sxs-lookup"><span data-stu-id="b501a-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="b501a-221">當記憶體不足時，網頁伺服器會先撤銷 `Low` 快取項目。</span><span class="sxs-lookup"><span data-stu-id="b501a-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="b501a-222">範例：</span><span class="sxs-lookup"><span data-stu-id="b501a-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="b501a-223">`priority` 屬性並不保證特定快取保留層級。</span><span class="sxs-lookup"><span data-stu-id="b501a-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="b501a-224">`CacheItemPriority` 僅供建議。</span><span class="sxs-lookup"><span data-stu-id="b501a-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="b501a-225">將此屬性設定為 `NeverRemove` 並不保證一定會保留快取。</span><span class="sxs-lookup"><span data-stu-id="b501a-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="b501a-226">如需詳細資訊，請參閱[其他資源](#additional-resources)。</span><span class="sxs-lookup"><span data-stu-id="b501a-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="b501a-227">快取標籤協助程式相依於[記憶體快取服務](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="b501a-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="b501a-228">如果尚未新增此服務，快取標籤協助程式會予以新增。</span><span class="sxs-lookup"><span data-stu-id="b501a-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b501a-229">其他資源</span><span class="sxs-lookup"><span data-stu-id="b501a-229">Additional resources</span></span>

* [<span data-ttu-id="b501a-230">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="b501a-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="b501a-231">身分識別簡介</span><span class="sxs-lookup"><span data-stu-id="b501a-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
