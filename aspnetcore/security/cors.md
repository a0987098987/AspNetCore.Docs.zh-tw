---
title: "啟用跨原始要求 (CORS)"
author: rick-anderson
description: "本文件介紹的標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始要求的 CORS。"
keywords: "ASP.NET Core，CORS，跨來源"
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 5398b6ad6531710de2b8000cb368e5fa607ae7ff
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="50f2a-104">啟用跨原始要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="50f2a-104">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="50f2a-105">由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="50f2a-105">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="50f2a-106">瀏覽器安全性防止網頁 AJAX 要求到另一個網域。</span><span class="sxs-lookup"><span data-stu-id="50f2a-106">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="50f2a-107">這項限制稱為*相同來源原則*，並可防止惡意網站從另一個站台讀取的機密資料。</span><span class="sxs-lookup"><span data-stu-id="50f2a-107">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="50f2a-108">不過，有時您可能想要讓其他對您的 web API 進行跨原始要求的站台。</span><span class="sxs-lookup"><span data-stu-id="50f2a-108">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="50f2a-109">[跨原始資源共用](http://www.w3.org/TR/cors/)」 (CORS) 是一種 W3C 標準，可讓放寬的相同來源原則的伺服器。</span><span class="sxs-lookup"><span data-stu-id="50f2a-109">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="50f2a-110">使用 CORS，伺服器可以明確地允許某些跨原始要求時拒絕其他人。</span><span class="sxs-lookup"><span data-stu-id="50f2a-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="50f2a-111">CORS 是更安全且更彈性與較早的技術如[JSONP](https://wikipedia.org/wiki/JSONP)。</span><span class="sxs-lookup"><span data-stu-id="50f2a-111">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="50f2a-112">本主題示範如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="50f2a-112">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="50f2a-113">什麼是 「 相同來源 」？</span><span class="sxs-lookup"><span data-stu-id="50f2a-113">What is "same origin"?</span></span>

<span data-ttu-id="50f2a-114">如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原始主機。</span><span class="sxs-lookup"><span data-stu-id="50f2a-114">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="50f2a-115">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="50f2a-115">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="50f2a-116">這些兩個 Url 有相同的原始主機：</span><span class="sxs-lookup"><span data-stu-id="50f2a-116">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="50f2a-117">這些 Url 會有兩個不同來源比上一個：</span><span class="sxs-lookup"><span data-stu-id="50f2a-117">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="50f2a-118">`http://example.net`為不同的網域</span><span class="sxs-lookup"><span data-stu-id="50f2a-118">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="50f2a-119">`http://www.example.com/foo.html`為不同的子網域</span><span class="sxs-lookup"><span data-stu-id="50f2a-119">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="50f2a-120">`https://example.com/foo.html`為不同的配置</span><span class="sxs-lookup"><span data-stu-id="50f2a-120">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="50f2a-121">`http://example.com:9000/foo.html`為不同的通訊埠</span><span class="sxs-lookup"><span data-stu-id="50f2a-121">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="50f2a-122">比較來源時，Internet Explorer 不會將連接埠。</span><span class="sxs-lookup"><span data-stu-id="50f2a-122">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="50f2a-123">CORS 設定</span><span class="sxs-lookup"><span data-stu-id="50f2a-123">Setting up CORS</span></span>

<span data-ttu-id="50f2a-124">若要設定加入您的應用程式的 CORS`Microsoft.AspNetCore.Cors`封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="50f2a-124">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="50f2a-125">將 CORS 服務加入 Startup.cs 中：</span><span class="sxs-lookup"><span data-stu-id="50f2a-125">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="50f2a-126">啟用 CORS 中介軟體</span><span class="sxs-lookup"><span data-stu-id="50f2a-126">Enabling CORS with middleware</span></span>

<span data-ttu-id="50f2a-127">若要啟用整個應用程式的 CORS 將 CORS 中介軟體新增至您要求管線使用`UseCors`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="50f2a-127">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="50f2a-128">請注意，CORS 中介軟體必須在任何已定義的端點之前在您想要支援跨原始要求 （例如應用程式中。</span><span class="sxs-lookup"><span data-stu-id="50f2a-128">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="50f2a-129">任何呼叫之前`UseMvc`)。</span><span class="sxs-lookup"><span data-stu-id="50f2a-129">before any call to `UseMvc`).</span></span>

<span data-ttu-id="50f2a-130">加入 CORS 中介軟體使用時，您可以指定跨原始原則`CorsPolicyBuilder`類別。</span><span class="sxs-lookup"><span data-stu-id="50f2a-130">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="50f2a-131">執行這項作業的方法有兩種。</span><span class="sxs-lookup"><span data-stu-id="50f2a-131">There are two ways to do this.</span></span> <span data-ttu-id="50f2a-132">第一個方法是呼叫 UseCors lambda:</span><span class="sxs-lookup"><span data-stu-id="50f2a-132">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="50f2a-133">**注意：**沒有尾端斜線，必須指定的 URL (`/`)。</span><span class="sxs-lookup"><span data-stu-id="50f2a-133">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="50f2a-134">如果 URL 將會終止並`/`，比較會傳回`false`而且不會傳回不含標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-134">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="50f2a-135">Lambda 會採用`CorsPolicyBuilder`物件。</span><span class="sxs-lookup"><span data-stu-id="50f2a-135">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="50f2a-136">您可以找到一份[組態選項](#cors-policy-options)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="50f2a-136">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="50f2a-137">在此範例中的原則，允許跨原始要求從`http://example.com`和其他來源。</span><span class="sxs-lookup"><span data-stu-id="50f2a-137">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="50f2a-138">請注意，CorsPolicyBuilder fluent API，因此您可以在方法呼叫鏈結：</span><span class="sxs-lookup"><span data-stu-id="50f2a-138">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="50f2a-139">第二種方法是定義一個或多個具名的 CORS 原則，然後依名稱選取的原則，在執行階段。</span><span class="sxs-lookup"><span data-stu-id="50f2a-139">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="50f2a-140">這個範例會將名為"AllowSpecificOrigin 」 的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="50f2a-140">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="50f2a-141">若要選取的原則，將傳遞至名稱`UseCors`。</span><span class="sxs-lookup"><span data-stu-id="50f2a-141">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="50f2a-142">啟用 CORS MVC 中</span><span class="sxs-lookup"><span data-stu-id="50f2a-142">Enabling CORS in MVC</span></span>

<span data-ttu-id="50f2a-143">MVC 或者可用來套用特定的 CORS，每個動作，每個控制站，或全域的所有控制站。</span><span class="sxs-lookup"><span data-stu-id="50f2a-143">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="50f2a-144">使用 MVC 啟用 CORS 時使用相同的 CORS 服務，但 CORS 中介軟體不是。</span><span class="sxs-lookup"><span data-stu-id="50f2a-144">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="50f2a-145">每個動作</span><span class="sxs-lookup"><span data-stu-id="50f2a-145">Per action</span></span>

<span data-ttu-id="50f2a-146">若要指定特定動作的 CORS 原則將`[EnableCors]`動作屬性。</span><span class="sxs-lookup"><span data-stu-id="50f2a-146">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="50f2a-147">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="50f2a-147">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="50f2a-148">每個控制站</span><span class="sxs-lookup"><span data-stu-id="50f2a-148">Per controller</span></span>

<span data-ttu-id="50f2a-149">若要指定將特定控制器的 CORS 原則`[EnableCors]`屬性加入控制器類別。</span><span class="sxs-lookup"><span data-stu-id="50f2a-149">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="50f2a-150">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="50f2a-150">Specify the policy name.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="50f2a-151">全域</span><span class="sxs-lookup"><span data-stu-id="50f2a-151">Globally</span></span>

<span data-ttu-id="50f2a-152">您可以啟用 CORS 全域所有控制器加入`CorsAuthorizationFilterFactory`全域篩選集合的篩選：</span><span class="sxs-lookup"><span data-stu-id="50f2a-152">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="50f2a-153">優先順序是： 全域控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="50f2a-153">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="50f2a-154">動作層級原則的優先順序高於控制器層級原則和控制器層級原則會優先於全域原則。</span><span class="sxs-lookup"><span data-stu-id="50f2a-154">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="50f2a-155">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="50f2a-155">Disable CORS</span></span>

<span data-ttu-id="50f2a-156">若要停用 CORS，控制器或動作，使用`[DisableCors]`屬性。</span><span class="sxs-lookup"><span data-stu-id="50f2a-156">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="50f2a-157">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="50f2a-157">CORS policy options</span></span>

<span data-ttu-id="50f2a-158">本章節描述您可以設定 CORS 原則中的各種選項。</span><span class="sxs-lookup"><span data-stu-id="50f2a-158">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="50f2a-159">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="50f2a-159">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="50f2a-160">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="50f2a-160">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="50f2a-161">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="50f2a-161">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="50f2a-162">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="50f2a-162">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="50f2a-163">跨原始要求中的認證</span><span class="sxs-lookup"><span data-stu-id="50f2a-163">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="50f2a-164">預檢到期時間設定</span><span class="sxs-lookup"><span data-stu-id="50f2a-164">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="50f2a-165">某些選項可能會很有幫助讀取[如何 CORS 運作](#how-cors-works)第一次。</span><span class="sxs-lookup"><span data-stu-id="50f2a-165">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="50f2a-166">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="50f2a-166">Set the allowed origins</span></span>

<span data-ttu-id="50f2a-167">若要允許一或多個特定的來源：</span><span class="sxs-lookup"><span data-stu-id="50f2a-167">To allow one or more specific origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="50f2a-168">若要允許所有來源：</span><span class="sxs-lookup"><span data-stu-id="50f2a-168">To allow all origins:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="50f2a-169">請仔細考慮，才能允許來自任何來源的要求。</span><span class="sxs-lookup"><span data-stu-id="50f2a-169">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="50f2a-170">這表示幾乎任何網站，可以進行 AJAX 呼叫您的 api。</span><span class="sxs-lookup"><span data-stu-id="50f2a-170">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="50f2a-171">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="50f2a-171">Set the allowed HTTP methods</span></span>

<span data-ttu-id="50f2a-172">若要允許所有的 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="50f2a-172">To allow all HTTP methods:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="50f2a-173">這會影響事前要求和存取控制-允許-方法標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-173">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="50f2a-174">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="50f2a-174">Set the allowed request headers</span></span>

<span data-ttu-id="50f2a-175">CORS 預檢要求可能會包含存取控制-頭 access-control-request-headers 標頭，列出應用程式所設定的 HTTP 標頭 (所謂的 「 撰寫要求標頭 」)。</span><span class="sxs-lookup"><span data-stu-id="50f2a-175">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="50f2a-176">白名單特定的標頭：</span><span class="sxs-lookup"><span data-stu-id="50f2a-176">To whitelist specific headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="50f2a-177">若要允許所有撰寫要求標頭：</span><span class="sxs-lookup"><span data-stu-id="50f2a-177">To allow all author request headers:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="50f2a-178">瀏覽器不會在它們如何設定存取控制-access-control-request-headers 標完全一致的。</span><span class="sxs-lookup"><span data-stu-id="50f2a-178">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="50f2a-179">如果您將設定標頭的任何項目不是"*"，您至少應該包含 [接受]，「 內容型別 」 和 「 原始 」，再加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-179">If you set headers to anything other than "*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="50f2a-180">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="50f2a-180">Set the exposed response headers</span></span>

<span data-ttu-id="50f2a-181">根據預設，在瀏覽器不會公開所有應用程式的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-181">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="50f2a-182">(請參閱[http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header)。)預設可用的回應標頭如下：</span><span class="sxs-lookup"><span data-stu-id="50f2a-182">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="50f2a-183">快取控制</span><span class="sxs-lookup"><span data-stu-id="50f2a-183">Cache-Control</span></span>

* <span data-ttu-id="50f2a-184">內容語言</span><span class="sxs-lookup"><span data-stu-id="50f2a-184">Content-Language</span></span>

* <span data-ttu-id="50f2a-185">內容類型</span><span class="sxs-lookup"><span data-stu-id="50f2a-185">Content-Type</span></span>

* <span data-ttu-id="50f2a-186">到期</span><span class="sxs-lookup"><span data-stu-id="50f2a-186">Expires</span></span>

* <span data-ttu-id="50f2a-187">上次修改</span><span class="sxs-lookup"><span data-stu-id="50f2a-187">Last-Modified</span></span>

* <span data-ttu-id="50f2a-188">Pragma</span><span class="sxs-lookup"><span data-stu-id="50f2a-188">Pragma</span></span>

<span data-ttu-id="50f2a-189">CORS 規格會呼叫這些*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="50f2a-189">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="50f2a-190">若要讓其他標頭可用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="50f2a-190">To make other headers available to the application:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="50f2a-191">跨原始要求中的認證</span><span class="sxs-lookup"><span data-stu-id="50f2a-191">Credentials in cross-origin requests</span></span>

<span data-ttu-id="50f2a-192">認證需要特殊處理 CORS 要求中。</span><span class="sxs-lookup"><span data-stu-id="50f2a-192">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="50f2a-193">根據預設，在瀏覽器不會傳送跨原始要求的任何認證。</span><span class="sxs-lookup"><span data-stu-id="50f2a-193">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="50f2a-194">認證會包括 cookie，以及 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="50f2a-194">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="50f2a-195">若要傳送與跨原始要求的認證，用戶端必須 XMLHttpRequest.withCredentials 設定為 true。</span><span class="sxs-lookup"><span data-stu-id="50f2a-195">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="50f2a-196">直接使用 XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="50f2a-196">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="50f2a-197">JQuery： 中</span><span class="sxs-lookup"><span data-stu-id="50f2a-197">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="50f2a-198">此外，伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="50f2a-198">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="50f2a-199">若要允許跨原始認證：</span><span class="sxs-lookup"><span data-stu-id="50f2a-199">To allow cross-origin credentials:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="50f2a-200">現在 HTTP 回應將包含存取控制-允許-認證標頭，告知瀏覽器伺服器允許跨原始要求的認證。</span><span class="sxs-lookup"><span data-stu-id="50f2a-200">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="50f2a-201">如果瀏覽器傳送認證，但回應不包含有效的存取控制-允許-認證標頭，瀏覽器不會公開至應用程式中，回應和 AJAX 要求失敗。</span><span class="sxs-lookup"><span data-stu-id="50f2a-201">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="50f2a-202">要非常小心有關允許跨原始認證，因為這表示網站，位於另一個網域可以傳送給您的應用程式使用者的身分登入之使用者的認證不會察覺使用者。</span><span class="sxs-lookup"><span data-stu-id="50f2a-202">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="50f2a-203">CORS 規格也狀態該設定來源可"*"（所有原始網域） 不正確的存取控制-允許-認證標頭是否存在。</span><span class="sxs-lookup"><span data-stu-id="50f2a-203">The CORS spec also states that setting origins to "*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="50f2a-204">預檢到期時間設定</span><span class="sxs-lookup"><span data-stu-id="50f2a-204">Set the preflight expiration time</span></span>

<span data-ttu-id="50f2a-205">存取控制的最大年齡標頭指定可以快取預檢要求的回應的時間長度。</span><span class="sxs-lookup"><span data-stu-id="50f2a-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="50f2a-206">若要設定此標頭：</span><span class="sxs-lookup"><span data-stu-id="50f2a-206">To set this header:</span></span>

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="50f2a-207">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="50f2a-207">How CORS works</span></span>

<span data-ttu-id="50f2a-208">本章節描述 CORS 要求，在 HTTP 訊息的層級中發生的事。</span><span class="sxs-lookup"><span data-stu-id="50f2a-208">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="50f2a-209">請務必了解 CORS 運作方式，以便您可以正確地設定您的 CORS 原則和疑難排解如果項目不在您預期方式運作。</span><span class="sxs-lookup"><span data-stu-id="50f2a-209">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="50f2a-210">CORS 規格導入了幾個新的 HTTP 標頭啟用跨原始要求。</span><span class="sxs-lookup"><span data-stu-id="50f2a-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="50f2a-211">如果瀏覽器支援 CORS，它會設定這些標頭會自動針對跨原始要求。您不需要執行任何 JavaScript 程式碼中的特殊的動作。</span><span class="sxs-lookup"><span data-stu-id="50f2a-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="50f2a-212">以下是跨原始要求的範例。</span><span class="sxs-lookup"><span data-stu-id="50f2a-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="50f2a-213">「 原始 」 標頭提供提出要求的站台的網域：</span><span class="sxs-lookup"><span data-stu-id="50f2a-213">The "Origin" header gives the domain of the site that is making the request:</span></span>

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

<span data-ttu-id="50f2a-214">如果伺服器允許的要求，它會在回應中設定的存取控制-允許的原始標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="50f2a-215">此標頭的值符合要求，從 Origin 標頭或萬用字元值"*"，允許任何來源的意義：</span><span class="sxs-lookup"><span data-stu-id="50f2a-215">The value of this header either matches the Origin header from the request, or is the wildcard value "*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="50f2a-216">如果回應不包含存取控制-允許的原始標頭，要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="50f2a-216">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="50f2a-217">具體來說，瀏覽器不允許要求。</span><span class="sxs-lookup"><span data-stu-id="50f2a-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="50f2a-218">即使伺服器會傳回成功的回應，瀏覽器不會回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="50f2a-218">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="50f2a-219">預檢要求</span><span class="sxs-lookup"><span data-stu-id="50f2a-219">Preflight Requests</span></span>

<span data-ttu-id="50f2a-220">對於某些 CORS 要求，瀏覽器會傳送其他要求，傳送實際要求的資源之前稱為 「 預檢要求，」。</span><span class="sxs-lookup"><span data-stu-id="50f2a-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="50f2a-221">如果下列條件成立，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="50f2a-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="50f2a-222">要求方法為 GET、 HEAD 或 post 要求和</span><span class="sxs-lookup"><span data-stu-id="50f2a-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="50f2a-223">應用程式不會設定 Accept，接受語言內容語言，以外的任何要求標頭的內容類型或最後一個事件識別碼，並</span><span class="sxs-lookup"><span data-stu-id="50f2a-223">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="50f2a-224">Content-type 標頭 (如果設定) 是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="50f2a-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="50f2a-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="50f2a-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="50f2a-226">multipart/表單資料</span><span class="sxs-lookup"><span data-stu-id="50f2a-226">multipart/form-data</span></span>

  * <span data-ttu-id="50f2a-227">文字/純文字</span><span class="sxs-lookup"><span data-stu-id="50f2a-227">text/plain</span></span>

<span data-ttu-id="50f2a-228">要求標頭有關的規則適用於應用程式會藉由呼叫 setRequestHeader XMLHttpRequest 物件上設定的標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="50f2a-229">（CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於可設定的瀏覽器，例如使用者代理程式、 主機、 或 Content-length 標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-229">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="50f2a-230">預檢要求的範例如下：</span><span class="sxs-lookup"><span data-stu-id="50f2a-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="50f2a-231">事前要求會使用 OPTIONS HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="50f2a-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="50f2a-232">它包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="50f2a-232">It includes two special headers:</span></span>

* <span data-ttu-id="50f2a-233">存取控制-要求的方法： 將實際的要求使用 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="50f2a-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="50f2a-234">存取控制-access-control-request-headers 標： 實際要求設定的應用程式的要求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="50f2a-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="50f2a-235">（同樣地，這不包括瀏覽器設定的標頭。）</span><span class="sxs-lookup"><span data-stu-id="50f2a-235">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="50f2a-236">以下是範例回應，假設伺服器允許要求：</span><span class="sxs-lookup"><span data-stu-id="50f2a-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="50f2a-237">回應包括列出允許的方法，存取控制-允許-方法標頭和選擇性地存取控制-允許的標頭標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="50f2a-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="50f2a-238">如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="50f2a-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
