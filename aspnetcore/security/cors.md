---
title: 啟用跨原始要求 (CORS) 中 ASP.NET Core
author: rick-anderson
description: 深入了解如何以標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始要求的 CORS。
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="52f26-103">啟用跨原始要求 (CORS) 中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52f26-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="52f26-104">由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="52f26-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="52f26-105">瀏覽器安全性防止網頁 AJAX 要求到另一個網域。</span><span class="sxs-lookup"><span data-stu-id="52f26-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="52f26-106">這項限制稱為*相同來源原則*，並可防止惡意網站從另一個站台讀取的機密資料。</span><span class="sxs-lookup"><span data-stu-id="52f26-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="52f26-107">不過，有時您可能想要讓其他對您的 web API 進行跨原始要求的站台。</span><span class="sxs-lookup"><span data-stu-id="52f26-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="52f26-108">[跨原始資源共用](http://www.w3.org/TR/cors/)」 (CORS) 是一種 W3C 標準，可讓放寬的相同來源原則的伺服器。</span><span class="sxs-lookup"><span data-stu-id="52f26-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="52f26-109">使用 CORS，伺服器可以明確地允許某些跨原始要求時拒絕其他人。</span><span class="sxs-lookup"><span data-stu-id="52f26-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="52f26-110">CORS 是更安全且更彈性與較早的技術如[JSONP](https://wikipedia.org/wiki/JSONP)。</span><span class="sxs-lookup"><span data-stu-id="52f26-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="52f26-111">本主題示範如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="52f26-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="52f26-112">什麼是 「 相同來源 」？</span><span class="sxs-lookup"><span data-stu-id="52f26-112">What is "same origin"?</span></span>

<span data-ttu-id="52f26-113">如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原始主機。</span><span class="sxs-lookup"><span data-stu-id="52f26-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="52f26-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="52f26-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="52f26-115">這些兩個 Url 有相同的原始主機：</span><span class="sxs-lookup"><span data-stu-id="52f26-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="52f26-116">這些 Url 會有兩個不同來源比上一個：</span><span class="sxs-lookup"><span data-stu-id="52f26-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="52f26-117">`http://example.net` 為不同的網域</span><span class="sxs-lookup"><span data-stu-id="52f26-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="52f26-118">`http://www.example.com/foo.html` 為不同的子網域</span><span class="sxs-lookup"><span data-stu-id="52f26-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="52f26-119">`https://example.com/foo.html` 為不同的配置</span><span class="sxs-lookup"><span data-stu-id="52f26-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="52f26-120">`http://example.com:9000/foo.html` 為不同的通訊埠</span><span class="sxs-lookup"><span data-stu-id="52f26-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="52f26-121">比較來源時，Internet Explorer 不會視為連接埠。</span><span class="sxs-lookup"><span data-stu-id="52f26-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="52f26-122">CORS 設定</span><span class="sxs-lookup"><span data-stu-id="52f26-122">Setting up CORS</span></span>

<span data-ttu-id="52f26-123">若要設定加入您的應用程式的 CORS`Microsoft.AspNetCore.Cors`封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="52f26-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="52f26-124">將 CORS 服務加入 Startup.cs 中：</span><span class="sxs-lookup"><span data-stu-id="52f26-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="52f26-125">啟用 CORS 中介軟體</span><span class="sxs-lookup"><span data-stu-id="52f26-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="52f26-126">若要啟用整個應用程式的 CORS 將 CORS 中介軟體新增至您要求管線使用`UseCors`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="52f26-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="52f26-127">請注意，CORS 中介軟體必須在任何已定義的端點之前在您想要支援跨原始要求 （例如應用程式中。</span><span class="sxs-lookup"><span data-stu-id="52f26-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="52f26-128">任何呼叫之前`UseMvc`)。</span><span class="sxs-lookup"><span data-stu-id="52f26-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="52f26-129">加入 CORS 中介軟體使用時，您可以指定跨原始原則`CorsPolicyBuilder`類別。</span><span class="sxs-lookup"><span data-stu-id="52f26-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="52f26-130">執行這項作業的方法有兩種。</span><span class="sxs-lookup"><span data-stu-id="52f26-130">There are two ways to do this.</span></span> <span data-ttu-id="52f26-131">第一個方法是呼叫 UseCors lambda:</span><span class="sxs-lookup"><span data-stu-id="52f26-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="52f26-132">**注意：**沒有尾端斜線，必須指定的 URL (`/`)。</span><span class="sxs-lookup"><span data-stu-id="52f26-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="52f26-133">如果 URL 將會終止並`/`，比較會傳回`false`而且不會傳回不含標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="52f26-134">Lambda 會採用`CorsPolicyBuilder`物件。</span><span class="sxs-lookup"><span data-stu-id="52f26-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="52f26-135">您可以找到一份[組態選項](#cors-policy-options)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="52f26-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="52f26-136">在此範例中的原則，允許跨原始要求從`http://example.com`和其他來源。</span><span class="sxs-lookup"><span data-stu-id="52f26-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="52f26-137">請注意，CorsPolicyBuilder fluent API，因此您可以在方法呼叫鏈結：</span><span class="sxs-lookup"><span data-stu-id="52f26-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="52f26-138">第二種方法是定義一個或多個具名的 CORS 原則，然後依名稱選取的原則，在執行階段。</span><span class="sxs-lookup"><span data-stu-id="52f26-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="52f26-139">這個範例會將名為"AllowSpecificOrigin 」 的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="52f26-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="52f26-140">若要選取的原則，將傳遞至名稱`UseCors`。</span><span class="sxs-lookup"><span data-stu-id="52f26-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="52f26-141">啟用 CORS MVC 中</span><span class="sxs-lookup"><span data-stu-id="52f26-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="52f26-142">MVC 或者可用來套用特定的 CORS，每個動作，每個控制站，或全域的所有控制站。</span><span class="sxs-lookup"><span data-stu-id="52f26-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="52f26-143">使用 MVC 啟用 CORS 時使用相同的 CORS 服務，但 CORS 中介軟體不是。</span><span class="sxs-lookup"><span data-stu-id="52f26-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="52f26-144">每個動作</span><span class="sxs-lookup"><span data-stu-id="52f26-144">Per action</span></span>

<span data-ttu-id="52f26-145">若要指定特定動作的 CORS 原則將`[EnableCors]`動作屬性。</span><span class="sxs-lookup"><span data-stu-id="52f26-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="52f26-146">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="52f26-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="52f26-147">每個控制站</span><span class="sxs-lookup"><span data-stu-id="52f26-147">Per controller</span></span>

<span data-ttu-id="52f26-148">若要指定將特定控制器的 CORS 原則`[EnableCors]`屬性加入控制器類別。</span><span class="sxs-lookup"><span data-stu-id="52f26-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="52f26-149">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="52f26-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="52f26-150">全域</span><span class="sxs-lookup"><span data-stu-id="52f26-150">Globally</span></span>

<span data-ttu-id="52f26-151">您可以啟用 CORS 全域所有控制器加入`CorsAuthorizationFilterFactory`全域篩選集合的篩選：</span><span class="sxs-lookup"><span data-stu-id="52f26-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="52f26-152">優先順序是： 全域控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="52f26-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="52f26-153">動作層級原則的優先順序高於控制器層級原則和控制器層級原則會優先於全域原則。</span><span class="sxs-lookup"><span data-stu-id="52f26-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="52f26-154">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="52f26-154">Disable CORS</span></span>

<span data-ttu-id="52f26-155">若要停用 CORS，控制器或動作，使用`[DisableCors]`屬性。</span><span class="sxs-lookup"><span data-stu-id="52f26-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="52f26-156">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="52f26-156">CORS policy options</span></span>

<span data-ttu-id="52f26-157">本章節描述您可以設定 CORS 原則中的各種選項。</span><span class="sxs-lookup"><span data-stu-id="52f26-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="52f26-158">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="52f26-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="52f26-159">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="52f26-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="52f26-160">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="52f26-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="52f26-161">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="52f26-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="52f26-162">跨原始要求中的認證</span><span class="sxs-lookup"><span data-stu-id="52f26-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="52f26-163">預檢到期時間設定</span><span class="sxs-lookup"><span data-stu-id="52f26-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="52f26-164">某些選項可能會很有幫助讀取[如何 CORS 運作](#how-cors-works)第一次。</span><span class="sxs-lookup"><span data-stu-id="52f26-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="52f26-165">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="52f26-165">Set the allowed origins</span></span>

<span data-ttu-id="52f26-166">若要允許一或多個特定的來源：</span><span class="sxs-lookup"><span data-stu-id="52f26-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="52f26-167">若要允許所有來源：</span><span class="sxs-lookup"><span data-stu-id="52f26-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="52f26-168">請仔細考慮，才能允許來自任何來源的要求。</span><span class="sxs-lookup"><span data-stu-id="52f26-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="52f26-169">這表示幾乎任何網站，可以進行 AJAX 呼叫您的 api。</span><span class="sxs-lookup"><span data-stu-id="52f26-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="52f26-170">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="52f26-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="52f26-171">若要允許所有的 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="52f26-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="52f26-172">這會影響事前要求和存取控制-允許-方法標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="52f26-173">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="52f26-173">Set the allowed request headers</span></span>

<span data-ttu-id="52f26-174">CORS 預檢要求可能會包含存取控制-頭 access-control-request-headers 標頭，列出應用程式所設定的 HTTP 標頭 (所謂的 「 撰寫要求標頭 」)。</span><span class="sxs-lookup"><span data-stu-id="52f26-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="52f26-175">允許清單特定的標頭：</span><span class="sxs-lookup"><span data-stu-id="52f26-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="52f26-176">若要允許所有撰寫要求標頭：</span><span class="sxs-lookup"><span data-stu-id="52f26-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="52f26-177">瀏覽器不會在它們如何設定存取控制-access-control-request-headers 標完全一致的。</span><span class="sxs-lookup"><span data-stu-id="52f26-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="52f26-178">如果您將設定標頭的任何項目不是"\*"，您至少應該包含 [接受]，「 內容型別 」 和 「 原始 」，再加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="52f26-179">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="52f26-179">Set the exposed response headers</span></span>

<span data-ttu-id="52f26-180">根據預設，瀏覽器不會公開所有的應用程式的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="52f26-181">(請參閱[ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header)。)預設可用的回應標頭如下：</span><span class="sxs-lookup"><span data-stu-id="52f26-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="52f26-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="52f26-182">Cache-Control</span></span>

* <span data-ttu-id="52f26-183">Content-Language</span><span class="sxs-lookup"><span data-stu-id="52f26-183">Content-Language</span></span>

* <span data-ttu-id="52f26-184">Content-Type</span><span class="sxs-lookup"><span data-stu-id="52f26-184">Content-Type</span></span>

* <span data-ttu-id="52f26-185">到期</span><span class="sxs-lookup"><span data-stu-id="52f26-185">Expires</span></span>

* <span data-ttu-id="52f26-186">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="52f26-186">Last-Modified</span></span>

* <span data-ttu-id="52f26-187">Pragma</span><span class="sxs-lookup"><span data-stu-id="52f26-187">Pragma</span></span>

<span data-ttu-id="52f26-188">CORS 規格會呼叫這些*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="52f26-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="52f26-189">若要讓其他標頭可用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="52f26-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="52f26-190">跨原始要求中的認證</span><span class="sxs-lookup"><span data-stu-id="52f26-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="52f26-191">認證需要特殊處理 CORS 要求中。</span><span class="sxs-lookup"><span data-stu-id="52f26-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="52f26-192">根據預設，瀏覽器不會傳送跨原始要求的任何認證。</span><span class="sxs-lookup"><span data-stu-id="52f26-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="52f26-193">認證會包括 cookie，以及 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="52f26-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="52f26-194">若要傳送與跨原始要求的認證，用戶端必須 XMLHttpRequest.withCredentials 設定為 true。</span><span class="sxs-lookup"><span data-stu-id="52f26-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="52f26-195">直接使用 XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="52f26-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="52f26-196">JQuery： 中</span><span class="sxs-lookup"><span data-stu-id="52f26-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="52f26-197">此外，伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="52f26-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="52f26-198">若要允許跨原始認證：</span><span class="sxs-lookup"><span data-stu-id="52f26-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="52f26-199">現在 HTTP 回應將包含存取控制-允許-認證標頭，告知瀏覽器伺服器允許跨原始要求的認證。</span><span class="sxs-lookup"><span data-stu-id="52f26-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="52f26-200">如果瀏覽器傳送認證，但回應未包含有效的存取控制-允許-認證標頭，瀏覽器將不會公開至應用程式中，回應和 AJAX 要求失敗。</span><span class="sxs-lookup"><span data-stu-id="52f26-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="52f26-201">允許跨原始憑證時要小心。</span><span class="sxs-lookup"><span data-stu-id="52f26-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="52f26-202">網站，位於另一個網域可以代表使用者的不知情的情況下的使用者上的應用程式傳送登入之使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="52f26-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="52f26-203">CORS 規格也會指出該設定來源可"\*"（所有原始網域） 是無效的如果`Access-Control-Allow-Credentials`標頭已存在。</span><span class="sxs-lookup"><span data-stu-id="52f26-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="52f26-204">預檢到期時間設定</span><span class="sxs-lookup"><span data-stu-id="52f26-204">Set the preflight expiration time</span></span>

<span data-ttu-id="52f26-205">存取控制的最大年齡標頭指定可以快取預檢要求的回應的時間長度。</span><span class="sxs-lookup"><span data-stu-id="52f26-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="52f26-206">若要設定此標頭：</span><span class="sxs-lookup"><span data-stu-id="52f26-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="52f26-207">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="52f26-207">How CORS works</span></span>

<span data-ttu-id="52f26-208">本章節描述在 CORS 要求的 HTTP 訊息層級中發生的事。</span><span class="sxs-lookup"><span data-stu-id="52f26-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="52f26-209">請務必了解 CORS 搭配運作，如此才能正確設定的 CORS 原則和 troubleshooted 發生非預期的行為。</span><span class="sxs-lookup"><span data-stu-id="52f26-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="52f26-210">CORS 規格導入了幾個新的 HTTP 標頭啟用跨原始要求。</span><span class="sxs-lookup"><span data-stu-id="52f26-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="52f26-211">如果瀏覽器支援 CORS，它會設定這些標頭會自動針對跨原始要求。</span><span class="sxs-lookup"><span data-stu-id="52f26-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="52f26-212">若要啟用 CORS，無須自訂 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="52f26-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="52f26-213">以下是跨原始要求的範例。</span><span class="sxs-lookup"><span data-stu-id="52f26-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="52f26-214">`Origin`標頭提供提出要求的站台的網域：</span><span class="sxs-lookup"><span data-stu-id="52f26-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="52f26-215">如果伺服器允許的要求，它會在回應中設定的存取控制-允許的原始標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="52f26-216">此標頭的值符合要求，從 Origin 標頭或萬用字元值"\*"，允許任何來源的意義：</span><span class="sxs-lookup"><span data-stu-id="52f26-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="52f26-217">如果回應不包含存取控制-允許的原始標頭，要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="52f26-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="52f26-218">具體來說，瀏覽器不允許要求。</span><span class="sxs-lookup"><span data-stu-id="52f26-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="52f26-219">即使伺服器會傳回成功的回應，瀏覽器不會提供回應給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="52f26-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="52f26-220">預檢要求</span><span class="sxs-lookup"><span data-stu-id="52f26-220">Preflight Requests</span></span>

<span data-ttu-id="52f26-221">對於某些 CORS 要求，瀏覽器會傳送其他要求，傳送實際要求的資源之前稱為 「 預檢要求，」。</span><span class="sxs-lookup"><span data-stu-id="52f26-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="52f26-222">如果下列條件成立，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="52f26-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="52f26-223">要求方法為 GET、 HEAD 或 post 要求和</span><span class="sxs-lookup"><span data-stu-id="52f26-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="52f26-224">應用程式不會設定 Accept，接受語言內容語言，以外的任何要求標頭的內容類型或最後一個事件識別碼，並</span><span class="sxs-lookup"><span data-stu-id="52f26-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="52f26-225">Content-type 標頭 (如果設定) 是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="52f26-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="52f26-226">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="52f26-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="52f26-227">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="52f26-227">multipart/form-data</span></span>

  * <span data-ttu-id="52f26-228">文字/純文字</span><span class="sxs-lookup"><span data-stu-id="52f26-228">text/plain</span></span>

<span data-ttu-id="52f26-229">要求標頭有關的規則適用於應用程式會藉由呼叫 setRequestHeader XMLHttpRequest 物件上設定的標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="52f26-230">（CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於可設定的瀏覽器，例如使用者代理程式、 主機、 或 Content-length 標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="52f26-231">預檢要求的範例如下：</span><span class="sxs-lookup"><span data-stu-id="52f26-231">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="52f26-232">事前要求會使用 OPTIONS HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="52f26-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="52f26-233">它包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="52f26-233">It includes two special headers:</span></span>

* <span data-ttu-id="52f26-234">存取控制-要求的方法： 將實際的要求使用 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="52f26-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="52f26-235">存取控制-access-control-request-headers 標： 實際要求設定的應用程式的要求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="52f26-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="52f26-236">（同樣地，這不包括瀏覽器設定的標頭）。</span><span class="sxs-lookup"><span data-stu-id="52f26-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="52f26-237">以下是範例回應，假設伺服器允許要求：</span><span class="sxs-lookup"><span data-stu-id="52f26-237">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="52f26-238">回應包括列出允許的方法，存取控制-允許-方法標頭和選擇性地存取控制-允許的標頭標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="52f26-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="52f26-239">如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="52f26-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
