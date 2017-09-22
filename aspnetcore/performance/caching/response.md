---
title: "回應快取"
author: rick-anderson
description: "說明如何使用快取以降低頻寬，並提升效能的回應。"
keywords: "ASP.NET Core，快取 HTTP 標頭的回應"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: af114401d2f6f183291caba3c015359afb737d93
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="response-caching"></a><span data-ttu-id="b9f1d-104">回應快取</span><span class="sxs-lookup"><span data-stu-id="b9f1d-104">Response Caching</span></span>

<span data-ttu-id="b9f1d-105">由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b9f1d-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="b9f1d-106">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="b9f1d-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a><span data-ttu-id="b9f1d-107">什麼是快取回應</span><span class="sxs-lookup"><span data-stu-id="b9f1d-107">What is Response Caching</span></span>

<span data-ttu-id="b9f1d-108">*快取回應*將快取相關的標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-108">*Response caching* adds cache-related headers to responses.</span></span> <span data-ttu-id="b9f1d-109">這些標頭，指定您想用戶端、 proxy 和中的介軟體來快取回應。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-109">These headers specify how you want client, proxy and middleware to cache responses.</span></span> <span data-ttu-id="b9f1d-110">回應快取可以減少用戶端或 proxy 可讓 web 伺服器的要求數目。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-110">Response caching can reduce the number of requests a client or proxy makes to the web server.</span></span> <span data-ttu-id="b9f1d-111">回應快取也可以減少量網頁伺服器執行以產生回應的工作。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-111">Response caching can also reduce the amount of work the web server performs to generate the response.</span></span> 

<span data-ttu-id="b9f1d-112">主要用於快取的 HTTP 標頭是`Cache-Control`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-112">The primary HTTP header used for caching is `Cache-Control`.</span></span> <span data-ttu-id="b9f1d-113">請參閱[HTTP 1.1 快取](https://tools.ietf.org/html/rfc7234#section-5.2)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-113">See the [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) for more information.</span></span> <span data-ttu-id="b9f1d-114">一般快取指示詞：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-114">Common cache directives:</span></span>

* [<span data-ttu-id="b9f1d-115">public</span><span class="sxs-lookup"><span data-stu-id="b9f1d-115">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [<span data-ttu-id="b9f1d-116">private</span><span class="sxs-lookup"><span data-stu-id="b9f1d-116">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [<span data-ttu-id="b9f1d-117">無快取</span><span class="sxs-lookup"><span data-stu-id="b9f1d-117">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [<span data-ttu-id="b9f1d-118">Pragma</span><span class="sxs-lookup"><span data-stu-id="b9f1d-118">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)
* [<span data-ttu-id="b9f1d-119">而有所不同</span><span class="sxs-lookup"><span data-stu-id="b9f1d-119">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)

<span data-ttu-id="b9f1d-120">Web 伺服器可以快取回應，藉由新增快取中介軟體的回應。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-120">The web server can cache responses by adding the response caching middleware.</span></span> <span data-ttu-id="b9f1d-121">請參閱[快取中介軟體的回應](middleware.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-121">See [Response caching middleware](middleware.md) for more information.</span></span>

## <a name="distributed-cache-tag-helper"></a><span data-ttu-id="b9f1d-122">分散式快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="b9f1d-122">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="b9f1d-123">[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)啟用分散式快取。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-123">The [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) enables distributed caching.</span></span>


## <a name="responsecache-attribute"></a><span data-ttu-id="b9f1d-124">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="b9f1d-124">ResponseCache Attribute</span></span>

<span data-ttu-id="b9f1d-125">`ResponseCacheAttribute`指定在回應快取中設定適當的標頭的必要參數。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-125">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="b9f1d-126">請參閱[ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)參數的描述。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-126">See [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)  for a description of the parameters.</span></span>

>[!WARNING]
> <span data-ttu-id="b9f1d-127">停用快取內容，其中包含已驗證的用戶端的資訊。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-127">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="b9f1d-128">快取，才應該啟用不會變更的內容會根據使用者的身分識別，或者是否使用者登入。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-128">Caching should only be enabled for content that does not change based on a user's identity, or whether a user is logged in.</span></span>

<span data-ttu-id="b9f1d-129">`VaryByQueryKeys string[]`(需要 ASP.NET Core 1.1.0 和更新版本): 設定時，快取中介軟體的回應會將預存的回應因指定的查詢索引鍵清單的值。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-129">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1.0 and higher): When set, the response caching middleware will vary the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="b9f1d-130">必須啟用快取中介軟體的回應，才能設定`VaryByQueryKeys`屬性，否則執行階段例外狀況將會擲回。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-130">The response caching middleware must be enabled to set the `VaryByQueryKeys` property, otherwise a runtime exception will be thrown.</span></span> <span data-ttu-id="b9f1d-131">沒有對應的 HTTP 標頭`VaryByQueryKeys`屬性。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-131">There is no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="b9f1d-132">這個屬性是由快取中介軟體的回應的 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-132">This property is an HTTP feature handled by the response caching middleware.</span></span> <span data-ttu-id="b9f1d-133">中介軟體提供快取回的應，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-133">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="b9f1d-134">例如，請考慮下列順序：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-134">For example, consider the following sequence:</span></span>

| <span data-ttu-id="b9f1d-135">要求</span><span class="sxs-lookup"><span data-stu-id="b9f1d-135">Request</span></span>          | <span data-ttu-id="b9f1d-136">結果</span><span class="sxs-lookup"><span data-stu-id="b9f1d-136">Result</span></span> |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | <span data-ttu-id="b9f1d-137">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="b9f1d-137">returned from server</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="b9f1d-138">所傳回的中介軟體</span><span class="sxs-lookup"><span data-stu-id="b9f1d-138">returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="b9f1d-139">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="b9f1d-139">returned from server</span></span> |

<span data-ttu-id="b9f1d-140">第一個要求是由伺服器傳回，而且快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-140">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="b9f1d-141">第二個要求會傳回由中介軟體，因為查詢字串比對前一個要求。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-141">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="b9f1d-142">第三項要求不在中介軟體快取因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-142">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="b9f1d-143">`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) `ResponseCacheFilter`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-143">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="b9f1d-144">`ResponseCacheFilter`執行的工作更新適當的 HTTP 標頭和回應的功能。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-144">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="b9f1d-145">篩選器：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-145">The filter:</span></span>

* <span data-ttu-id="b9f1d-146">移除任何現有的標頭的`Vary`， `Cache-Control`，和`Pragma`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-146">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="b9f1d-147">寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-147">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="b9f1d-148">更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-148">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="b9f1d-149">而有所不同</span><span class="sxs-lookup"><span data-stu-id="b9f1d-149">Vary</span></span>

<span data-ttu-id="b9f1d-150">此標頭僅寫入時`VaryByHeader`屬性設定。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-150">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="b9f1d-151">設定為`Vary`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-151">It is set to the `Vary` property's value.</span></span> <span data-ttu-id="b9f1d-152">下列範例會使用`VaryByHeader`屬性。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-152">The following sample uses the `VaryByHeader` property.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="b9f1d-153">您的瀏覽器網路工具，您可以檢視回應標頭。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-153">You can view the response headers with your browsers network tools.</span></span> <span data-ttu-id="b9f1d-154">下圖顯示輸出的邊緣 F12**網路**索引標籤時`About2`動作方法會重新整理。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-154">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed.</span></span> 

![在上邊緣 F12 輸出 * * 網路 * * 索引標籤時呼叫 'About2' 動作方法](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="b9f1d-156">NoStore 和 Location.None</span><span class="sxs-lookup"><span data-stu-id="b9f1d-156">NoStore and Location.None</span></span>

<span data-ttu-id="b9f1d-157">`NoStore`大部分的其他屬性會覆寫。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-157">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="b9f1d-158">當這個屬性設定為`true`、`Cache-Control`標頭會設定為 「 無存放區 」。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-158">When this property is set to `true`, the `Cache-Control` header will be set to "no-store".</span></span> <span data-ttu-id="b9f1d-159">如果`Location`設`None`:</span><span class="sxs-lookup"><span data-stu-id="b9f1d-159">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="b9f1d-160">`Cache-Control` 設定為 `"no-store, no-cache"`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-160">`Cache-Control` is set to `"no-store, no-cache"`.</span></span> 
* <span data-ttu-id="b9f1d-161">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-161">`Pragma` is set to `no-cache`.</span></span> 

<span data-ttu-id="b9f1d-162">如果`NoStore`是`false`和`Location`是`None`，`Cache-Control`和`Pragma`會設定為`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-162">If `NoStore` is `false` and `Location` is `None`,  `Cache-Control` and `Pragma` will be set to `no-cache`.</span></span>

<span data-ttu-id="b9f1d-163">您通常將`NoStore`至`true`錯誤頁面上。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-163">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="b9f1d-164">例如: </span><span class="sxs-lookup"><span data-stu-id="b9f1d-164">For example:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="b9f1d-165">這會導致下列標頭：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-165">This will result in the following headers:</span></span>

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="b9f1d-166">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="b9f1d-166">Location and Duration</span></span>

<span data-ttu-id="b9f1d-167">若要啟用快取，`Duration`必須設為正值和`Location`都必須在`Any`（預設值） 或`Client`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-167">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="b9f1d-168">在此情況下，`Cache-Control`標頭會設定為後面接著"的保留時間上限 」 的回應的位置值。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-168">In this case, the `Cache-Control` header will be set to the location value followed by the "max-age" of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="b9f1d-169">`Location`選項`Any`和`Client`轉譯`Cache-Control`標頭值的`public`和`private`分別。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-169">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="b9f1d-170">如先前所述，設定`Location`至`None`同時設定`Cache-Control`和`Pragma`標頭`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-170">As noted previously, setting `Location` to `None` will set both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="b9f1d-171">以下範例，示範標頭所產生的設定`Duration`並讓預設`Location`值。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-171">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="b9f1d-172">會產生下列標頭：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-172">Produces the following headers:</span></span>

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a><span data-ttu-id="b9f1d-173">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="b9f1d-173">Cache Profiles</span></span>

<span data-ttu-id="b9f1d-174">而不必重複`ResponseCache`上許多的控制器動作屬性，快取設定檔的設定可以設定為選項，設定在 MVC 時`ConfigureServices`方法中的`Startup`。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-174">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="b9f1d-175">參考的快取設定檔中的值將使用的預設值為`ResponseCache`屬性，然後將會覆寫屬性指定任何屬性。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-175">Values found in a referenced cache profile will be used as the defaults by the `ResponseCache` attribute, and will be overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="b9f1d-176">設定快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-176">Setting up a cache profile:</span></span>

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="b9f1d-177">參考快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-177">Referencing a cache profile:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="b9f1d-178">`ResponseCache`屬性可以套用到動作 （方法），以及控制站 （類別）。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-178">The `ResponseCache` attribute can be applied both to actions (methods) as well as controllers (classes).</span></span> <span data-ttu-id="b9f1d-179">方法層級屬性會覆寫類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-179">Method-level attributes will override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="b9f1d-180">在上述範例中，類別層級屬性會指定持續時間為 30 秒，而方法層級屬性設定為 60 秒持續時間的參考快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="b9f1d-180">In the above example, a class-level attribute specifies a duration of 30 seconds while a method-level attributes references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="b9f1d-181">產生的標頭：</span><span class="sxs-lookup"><span data-stu-id="b9f1d-181">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a><span data-ttu-id="b9f1d-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="b9f1d-182">Additional Resources</span></span>

* [<span data-ttu-id="b9f1d-183">快取 http 規格</span><span class="sxs-lookup"><span data-stu-id="b9f1d-183">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="b9f1d-184">快取控制</span><span class="sxs-lookup"><span data-stu-id="b9f1d-184">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
