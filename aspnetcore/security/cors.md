---
title: 啟用 ASP.NET Core 中的跨源要求 (CORS)
author: rick-anderson
description: 了解如何為標準，以允許或拒絕在 ASP.NET Core 應用程式的跨原始要求的 CORS。
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2018
ms.locfileid: "41834714"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="4ba07-103">啟用 ASP.NET Core 中的跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="4ba07-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="4ba07-104">藉由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4ba07-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4ba07-105">瀏覽器安全性可防止網頁對另一個網域提出 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="4ba07-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="4ba07-106">這項限制稱為*同源原則*，可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="4ba07-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="4ba07-107">不過，有時候您可能想要讓跨原始來源要求對您的 web API 的其他站台。</span><span class="sxs-lookup"><span data-stu-id="4ba07-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="4ba07-108">[跨原始資源共用](http://www.w3.org/TR/cors/)(CORS) 是 W3C 標準，可讓伺服器放寬同源原則。</span><span class="sxs-lookup"><span data-stu-id="4ba07-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="4ba07-109">使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。</span><span class="sxs-lookup"><span data-stu-id="4ba07-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="4ba07-110">CORS 可較為安全且更有彈性，比早期技術這類[JSONP](https://wikipedia.org/wiki/JSONP)。</span><span class="sxs-lookup"><span data-stu-id="4ba07-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="4ba07-111">本主題說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="4ba07-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="4ba07-112">什麼是 「 相同原始 」？</span><span class="sxs-lookup"><span data-stu-id="4ba07-112">What is "same origin"?</span></span>

<span data-ttu-id="4ba07-113">如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原點。</span><span class="sxs-lookup"><span data-stu-id="4ba07-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="4ba07-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="4ba07-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="4ba07-115">這些兩個 Url 有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="4ba07-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="4ba07-116">這些 Url 會有兩個不同的來源，比上一個：</span><span class="sxs-lookup"><span data-stu-id="4ba07-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="4ba07-117">`http://example.net` -不同的網域</span><span class="sxs-lookup"><span data-stu-id="4ba07-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="4ba07-118">`http://www.example.com/foo.html` -不同的子網域</span><span class="sxs-lookup"><span data-stu-id="4ba07-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="4ba07-119">`https://example.com/foo.html` -不同的配置</span><span class="sxs-lookup"><span data-stu-id="4ba07-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="4ba07-120">`http://example.com:9000/foo.html` -不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="4ba07-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="4ba07-121">比較原始來源時，Internet Explorer 不會視為連接埠。</span><span class="sxs-lookup"><span data-stu-id="4ba07-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="4ba07-122">啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="4ba07-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="4ba07-123">若要設定您的應用程式的 CORS 新增`Microsoft.AspNetCore.Cors`封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="4ba07-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="4ba07-124">呼叫[AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)在`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4ba07-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="4ba07-125">與中介軟體啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="4ba07-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="4ba07-126">若要啟用 CORS，請將 CORS 中介軟體新增至要求管線使用`UseCors`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="4ba07-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="4ba07-127">CORS 中介軟體必須在前面定義的端點在您的應用程式中要支援跨原始來源要求 (例如，任何呼叫之前`UseMvc`)。</span><span class="sxs-lookup"><span data-stu-id="4ba07-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="4ba07-128">新增 CORS 中介軟體使用時，就可以指定跨源原則[CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)類別。</span><span class="sxs-lookup"><span data-stu-id="4ba07-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="4ba07-129">執行這項作業的方法有兩種。</span><span class="sxs-lookup"><span data-stu-id="4ba07-129">There are two ways to do this.</span></span> <span data-ttu-id="4ba07-130">第一個是呼叫`UseCors`使用 lambda:</span><span class="sxs-lookup"><span data-stu-id="4ba07-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="4ba07-131">**注意︰** 必須指定不含尾端斜線的 URL (`/`)。</span><span class="sxs-lookup"><span data-stu-id="4ba07-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="4ba07-132">如果 URL 終止`/`，比較會傳回`false`且會傳回不含標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="4ba07-133">Lambda 會採用`CorsPolicyBuilder`物件。</span><span class="sxs-lookup"><span data-stu-id="4ba07-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="4ba07-134">您會發現一份[組態選項](#cors-policy-options)本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="4ba07-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="4ba07-135">在此範例中，該原則可讓跨源要求，從`http://example.com`和其他來源。</span><span class="sxs-lookup"><span data-stu-id="4ba07-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="4ba07-136">CorsPolicyBuilder 有 fluent API，因此您可以在方法呼叫鏈結：</span><span class="sxs-lookup"><span data-stu-id="4ba07-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="4ba07-137">第二種方法是定義一或多個具名的 CORS 原則，並再依名稱選取的原則，在執行階段。</span><span class="sxs-lookup"><span data-stu-id="4ba07-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="4ba07-138">此範例會新增一個名為"AllowSpecificOrigin 」 的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="4ba07-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="4ba07-139">若要選取原則，將名稱傳遞給`UseCors`。</span><span class="sxs-lookup"><span data-stu-id="4ba07-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="4ba07-140">在 MVC 中啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="4ba07-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="4ba07-141">您也可以使用 MVC 套用每個動作，每個控制站，或適用於所有控制站的特定 CORS。</span><span class="sxs-lookup"><span data-stu-id="4ba07-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="4ba07-142">使用 MVC 啟用 CORS 時，會使用相同的 CORS 服務，但不 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="4ba07-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="4ba07-143">每個動作</span><span class="sxs-lookup"><span data-stu-id="4ba07-143">Per action</span></span>

<span data-ttu-id="4ba07-144">若要指定特定動作的 CORS 原則新增`[EnableCors]`屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="4ba07-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="4ba07-145">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="4ba07-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="4ba07-146">每個控制站</span><span class="sxs-lookup"><span data-stu-id="4ba07-146">Per controller</span></span>

<span data-ttu-id="4ba07-147">若要指定特定的控制站的 CORS 原則新增`[EnableCors]`屬性至控制器類別。</span><span class="sxs-lookup"><span data-stu-id="4ba07-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="4ba07-148">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="4ba07-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="4ba07-149">全域</span><span class="sxs-lookup"><span data-stu-id="4ba07-149">Globally</span></span>

<span data-ttu-id="4ba07-150">您可以啟用 CORS 全域所有控制站新增`CorsAuthorizationFilterFactory`至全域篩選集合的篩選器：</span><span class="sxs-lookup"><span data-stu-id="4ba07-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="4ba07-151">優先順序是： 全域控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="4ba07-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="4ba07-152">動作層級原則優先於控制器層級原則，以及控制器層級原則的優先順序高於全域原則。</span><span class="sxs-lookup"><span data-stu-id="4ba07-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="4ba07-153">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="4ba07-153">Disable CORS</span></span>

<span data-ttu-id="4ba07-154">若要停用控制器或動作的 CORS，請使用`[DisableCors]`屬性。</span><span class="sxs-lookup"><span data-stu-id="4ba07-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="4ba07-155">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="4ba07-155">CORS policy options</span></span>

<span data-ttu-id="4ba07-156">本章節描述您可以設定的 CORS 原則中的各種選項。</span><span class="sxs-lookup"><span data-stu-id="4ba07-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="4ba07-157">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="4ba07-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="4ba07-158">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="4ba07-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="4ba07-159">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="4ba07-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="4ba07-160">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="4ba07-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="4ba07-161">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="4ba07-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="4ba07-162">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="4ba07-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="4ba07-163">如需一些選項，可能會很有幫助讀取[如何 CORS 運作](#how-cors-works)第一次。</span><span class="sxs-lookup"><span data-stu-id="4ba07-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="4ba07-164">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="4ba07-164">Set the allowed origins</span></span>

<span data-ttu-id="4ba07-165">若要允許一或多個特定的來源：</span><span class="sxs-lookup"><span data-stu-id="4ba07-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="4ba07-166">若要允許所有來源：</span><span class="sxs-lookup"><span data-stu-id="4ba07-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="4ba07-167">請仔細考慮，才能允許來自任何來源的要求。</span><span class="sxs-lookup"><span data-stu-id="4ba07-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="4ba07-168">這表示，幾乎任何網站可以對進行 AJAX 呼叫您的 API。</span><span class="sxs-lookup"><span data-stu-id="4ba07-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="4ba07-169">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="4ba07-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="4ba07-170">若要允許所有的 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="4ba07-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="4ba07-171">這會影響事前要求和存取控制-允許-方法標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="4ba07-172">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="4ba07-172">Set the allowed request headers</span></span>

<span data-ttu-id="4ba07-173">CORS 預檢要求可能會包含存取控制-access-control-request-headers 標標頭，列出應用程式設定的 HTTP 標頭 (所謂 「 撰寫要求標頭 」)。</span><span class="sxs-lookup"><span data-stu-id="4ba07-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="4ba07-174">列入允許清單特定的標頭：</span><span class="sxs-lookup"><span data-stu-id="4ba07-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="4ba07-175">若要允許所有撰寫要求標頭：</span><span class="sxs-lookup"><span data-stu-id="4ba07-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="4ba07-176">瀏覽器不會在它們如何設定存取控制-access-control-request-headers 標完全一致的。</span><span class="sxs-lookup"><span data-stu-id="4ba07-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="4ba07-177">如果您將設定標頭的任何項目以外的其他 「 \* 」，您應該至少包含在 「 accept 」，"content-type"、"origin"，以及您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="4ba07-178">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="4ba07-178">Set the exposed response headers</span></span>

<span data-ttu-id="4ba07-179">根據預設，瀏覽器不會將所有應用程式的回應標頭公開。</span><span class="sxs-lookup"><span data-stu-id="4ba07-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="4ba07-180">(請參閱[ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header)。)是預設可用的回應標頭如下：</span><span class="sxs-lookup"><span data-stu-id="4ba07-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="4ba07-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="4ba07-181">Cache-Control</span></span>

* <span data-ttu-id="4ba07-182">內容語言</span><span class="sxs-lookup"><span data-stu-id="4ba07-182">Content-Language</span></span>

* <span data-ttu-id="4ba07-183">Content-Type</span><span class="sxs-lookup"><span data-stu-id="4ba07-183">Content-Type</span></span>

* <span data-ttu-id="4ba07-184">到期</span><span class="sxs-lookup"><span data-stu-id="4ba07-184">Expires</span></span>

* <span data-ttu-id="4ba07-185">上次修改</span><span class="sxs-lookup"><span data-stu-id="4ba07-185">Last-Modified</span></span>

* <span data-ttu-id="4ba07-186">Pragma</span><span class="sxs-lookup"><span data-stu-id="4ba07-186">Pragma</span></span>

<span data-ttu-id="4ba07-187">CORS 規格會呼叫這些*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="4ba07-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="4ba07-188">若要讓其他標頭可供應用程式：</span><span class="sxs-lookup"><span data-stu-id="4ba07-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="4ba07-189">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="4ba07-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="4ba07-190">認證需要在 CORS 要求的特殊處理。</span><span class="sxs-lookup"><span data-stu-id="4ba07-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="4ba07-191">根據預設，瀏覽器不會傳送任何與跨原始要求的認證。</span><span class="sxs-lookup"><span data-stu-id="4ba07-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="4ba07-192">認證包含 cookie，以及 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="4ba07-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="4ba07-193">若要傳送與跨原始要求的認證，用戶端必須 XMLHttpRequest.withCredentials 設為 true。</span><span class="sxs-lookup"><span data-stu-id="4ba07-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="4ba07-194">直接使用 XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="4ba07-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="4ba07-195">在 jQuery 中：</span><span class="sxs-lookup"><span data-stu-id="4ba07-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="4ba07-196">此外，伺服器必須允許的認證。</span><span class="sxs-lookup"><span data-stu-id="4ba07-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="4ba07-197">若要允許跨原始來源的認證：</span><span class="sxs-lookup"><span data-stu-id="4ba07-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="4ba07-198">現在的 HTTP 回應將包含存取控制-允許-認證標頭，它會告訴瀏覽器伺服器允許跨原始來源要求認證。</span><span class="sxs-lookup"><span data-stu-id="4ba07-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="4ba07-199">如果瀏覽器傳送認證，但是回應沒有包含有效的存取控制-允許-認證標頭，瀏覽器不會公開 （expose） 的應用程式的回應，而且 AJAX 要求失敗。</span><span class="sxs-lookup"><span data-stu-id="4ba07-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="4ba07-200">允許跨原始來源的認證時要小心。</span><span class="sxs-lookup"><span data-stu-id="4ba07-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="4ba07-201">網站，以在另一個網域可以傳送給使用者不知情的情況下代表的使用者上的應用程式的登入的使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="4ba07-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="4ba07-202">CORS 規格也會指出該設定來源，則`"*"`（所有原始網域） 是無效的如果`Access-Control-Allow-Credentials`標頭已存在。</span><span class="sxs-lookup"><span data-stu-id="4ba07-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="4ba07-203">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="4ba07-203">Set the preflight expiration time</span></span>

<span data-ttu-id="4ba07-204">存取控制-最大壽命標頭會指定多久可以快取預檢要求的回應。</span><span class="sxs-lookup"><span data-stu-id="4ba07-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="4ba07-205">若要設定此標頭：</span><span class="sxs-lookup"><span data-stu-id="4ba07-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="4ba07-206">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="4ba07-206">How CORS works</span></span>

<span data-ttu-id="4ba07-207">本章節描述 CORS 要求的 HTTP 訊息層級中發生的動作。</span><span class="sxs-lookup"><span data-stu-id="4ba07-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="4ba07-208">請務必了解，這樣可以正確地設定和非預期的行為發生時偵錯的 CORS 原則，CORS 的運作方式。</span><span class="sxs-lookup"><span data-stu-id="4ba07-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="4ba07-209">CORS 規格引進了數個新的 HTTP 標頭啟用跨源要求。</span><span class="sxs-lookup"><span data-stu-id="4ba07-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="4ba07-210">如果瀏覽器支援 CORS，它會設定自動跨原始來源要求這些標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="4ba07-211">若要啟用 CORS，不需要自訂 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="4ba07-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="4ba07-212">以下是跨原始要求的範例。</span><span class="sxs-lookup"><span data-stu-id="4ba07-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="4ba07-213">`Origin`標頭提供網站提出要求的網域：</span><span class="sxs-lookup"><span data-stu-id="4ba07-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="4ba07-214">如果伺服器允許的要求，它會在回應中設定的存取控制-允許-原始標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="4ba07-215">此標頭的值符合 Origin 標頭從要求中，或者是萬用字元值"\*"，表示允許任何來源：</span><span class="sxs-lookup"><span data-stu-id="4ba07-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="4ba07-216">如果回應不包含存取控制-允許-原始標頭，AJAX 要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="4ba07-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="4ba07-217">具體而言，瀏覽器不允許要求。</span><span class="sxs-lookup"><span data-stu-id="4ba07-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="4ba07-218">即使伺服器會傳回成功的回應，瀏覽器不提供回應給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ba07-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="4ba07-219">預檢要求</span><span class="sxs-lookup"><span data-stu-id="4ba07-219">Preflight Requests</span></span>

<span data-ttu-id="4ba07-220">對於某些 CORS 要求，瀏覽器會傳送其他要求，稱為 「 預檢要求，」 後才會傳送實際要求的資源。</span><span class="sxs-lookup"><span data-stu-id="4ba07-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="4ba07-221">如果下列條件成立，則瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="4ba07-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="4ba07-222">要求方法是 GET、 HEAD 或 POST、 和</span><span class="sxs-lookup"><span data-stu-id="4ba07-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="4ba07-223">應用程式不會設定 Accept、 Accept-language、 內容語言、 以外任何要求標頭內容類型或最後一個事件識別碼，以及</span><span class="sxs-lookup"><span data-stu-id="4ba07-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="4ba07-224">Content-type 標頭 (如果設定) 是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="4ba07-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="4ba07-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="4ba07-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="4ba07-226">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="4ba07-226">multipart/form-data</span></span>

  * <span data-ttu-id="4ba07-227">text/plain</span><span class="sxs-lookup"><span data-stu-id="4ba07-227">text/plain</span></span>

<span data-ttu-id="4ba07-228">要求標頭的相關規則適用於應用程式會藉由呼叫 setRequestHeader XMLHttpRequest 物件上設定的標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="4ba07-229">（CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於設定瀏覽器，例如使用者代理程式、 主機或 Content-length 標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="4ba07-230">預檢要求的範例如下：</span><span class="sxs-lookup"><span data-stu-id="4ba07-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="4ba07-231">事前要求使用 HTTP OPTIONS; 方法。</span><span class="sxs-lookup"><span data-stu-id="4ba07-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="4ba07-232">它包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="4ba07-232">It includes two special headers:</span></span>

* <span data-ttu-id="4ba07-233">存取控制-要求的方法： 將實際的要求使用 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="4ba07-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="4ba07-234">存取控制-access-control-request-headers 標： 實際的要求設定的應用程式的要求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="4ba07-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="4ba07-235">（同樣地，這不包括瀏覽器設定的標頭。）</span><span class="sxs-lookup"><span data-stu-id="4ba07-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="4ba07-236">以下是範例回應，假設伺服器允許的要求：</span><span class="sxs-lookup"><span data-stu-id="4ba07-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="4ba07-237">此回應包含存取控制-允許-方法標頭，其中列出允許的方法，並選擇性地存取控制-允許-標頭的標頭，它會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="4ba07-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="4ba07-238">如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="4ba07-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
