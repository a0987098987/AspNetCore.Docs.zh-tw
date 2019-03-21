---
title: 啟用 ASP.NET Core 中的跨源要求 (CORS)
author: rick-anderson
description: 了解如何為標準，以允許或拒絕在 ASP.NET Core 應用程式的跨原始要求的 CORS。
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 2cad26d0f61519f63888a2bc399bb7e8a0f1ee04
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210128"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="49676-103">啟用 ASP.NET Core 中的跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="49676-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="49676-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49676-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="49676-105">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="49676-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="49676-106">瀏覽器安全性可防止網頁對不同的網域服務之 web 網頁提出要求。</span><span class="sxs-lookup"><span data-stu-id="49676-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="49676-107">這項限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="49676-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="49676-108">同源原則會防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="49676-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="49676-109">有時候，您可能要允許其他站台會將跨原始來源要求對您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="49676-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="49676-110">如需詳細資訊，請參閱 < [Mozilla CORS 文章](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="49676-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="49676-111">[跨原始資源共用](https://www.w3.org/TR/cors/)(CORS):</span><span class="sxs-lookup"><span data-stu-id="49676-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="49676-112">是 W3C 標準，可讓伺服器放寬同源原則。</span><span class="sxs-lookup"><span data-stu-id="49676-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="49676-113">已**不**一項安全性功能，CORS 會放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="49676-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="49676-114">API 不允許 CORS 較為安全。</span><span class="sxs-lookup"><span data-stu-id="49676-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="49676-115">如需詳細資訊，請參閱 <<c0> [ 運作方式的 CORS](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="49676-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="49676-116">可讓以明確允許某些跨源要求，並拒絕其他的伺服器。</span><span class="sxs-lookup"><span data-stu-id="49676-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="49676-117">是更安全和更有彈性，比早期的技術，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="49676-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="49676-118">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="49676-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="49676-119">相同原始來源</span><span class="sxs-lookup"><span data-stu-id="49676-119">Same origin</span></span>

<span data-ttu-id="49676-120">兩個 Url 有相同的原點，如果它們有相同的配置、 主機和連接埠 ([RFC 6454](https://tools.ietf.org/html/rfc6454))。</span><span class="sxs-lookup"><span data-stu-id="49676-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="49676-121">這些兩個 Url 有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="49676-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="49676-122">這些 Url 會有不同的來源，比先前的兩個 Url:</span><span class="sxs-lookup"><span data-stu-id="49676-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="49676-123">`https://example.net` &ndash; 不同的網域</span><span class="sxs-lookup"><span data-stu-id="49676-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="49676-124">`https://www.example.com/foo.html` &ndash; 不同的子網域</span><span class="sxs-lookup"><span data-stu-id="49676-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="49676-125">`http://example.com/foo.html` &ndash; 不同的配置</span><span class="sxs-lookup"><span data-stu-id="49676-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="49676-126">`https://example.com:9000/foo.html` &ndash; 不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="49676-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="49676-127">比較原始來源時，Internet Explorer 不會視為連接埠。</span><span class="sxs-lookup"><span data-stu-id="49676-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="49676-128">使用具名的原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="49676-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="49676-129">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="49676-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="49676-130">下列程式碼會為指定的 origin 整個應用程式啟用 CORS:</span><span class="sxs-lookup"><span data-stu-id="49676-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="49676-131">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="49676-131">The preceding code:</span></span>

* <span data-ttu-id="49676-132">若要設定的原則名稱"\_myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="49676-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="49676-133">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="49676-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="49676-134">呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>延伸模組方法，可讓核心。</span><span class="sxs-lookup"><span data-stu-id="49676-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables cores.</span></span>
* <span data-ttu-id="49676-135">呼叫<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>具有[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="49676-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="49676-136">Lambda 會採用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>物件。</span><span class="sxs-lookup"><span data-stu-id="49676-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="49676-137">[組態選項](#cors-policy-options)，例如`WithOrigins`，本文稍後所述。</span><span class="sxs-lookup"><span data-stu-id="49676-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="49676-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將應用程式的服務容器中的 CORS 服務：</span><span class="sxs-lookup"><span data-stu-id="49676-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="49676-139">如需詳細資訊，請參閱 < [CORS 原則選項](#cpo)本文件中。</span><span class="sxs-lookup"><span data-stu-id="49676-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="49676-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以鏈結方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="49676-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="49676-141">下列醒目提示的程式碼會套用至透過 CORS 中介軟體的所有應用程式端點的 CORS 原則：</span><span class="sxs-lookup"><span data-stu-id="49676-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

<span data-ttu-id="49676-142">請參閱[Razor 頁面、 控制器及動作方法中的 啟用 CORS](#ecors)套用頁面/控制站/動作層級的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="49676-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="49676-143">注意:</span><span class="sxs-lookup"><span data-stu-id="49676-143">Note:</span></span>

* <span data-ttu-id="49676-144">`UseCors` 必須在 `UseMvc` 之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="49676-144">`UseCors` must be called before `UseMvc`.</span></span>
* <span data-ttu-id="49676-145">URL 必須**未**包含斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="49676-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="49676-146">如果 URL 終止`/`，比較傳回`false`並傳回不含標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="49676-147">請參閱[測試 CORS](#test)如需有關測試上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="49676-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="49676-148">使用屬性中啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="49676-148">Enable CORS with attributes</span></span>

<span data-ttu-id="49676-149">[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="49676-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="49676-150">`[EnableCors]`屬性能讓選取的結束點，而不是所有的結束點的 CORS。</span><span class="sxs-lookup"><span data-stu-id="49676-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="49676-151">使用`[EnableCors]`指定的預設原則和`[EnableCors("{Policy String}")]`指定原則。</span><span class="sxs-lookup"><span data-stu-id="49676-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="49676-152">`[EnableCors]`屬性可以套用至：</span><span class="sxs-lookup"><span data-stu-id="49676-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="49676-153">Razor 頁面 `PageModel`</span><span class="sxs-lookup"><span data-stu-id="49676-153">Razor Page `PageModel`</span></span>
* <span data-ttu-id="49676-154">控制器</span><span class="sxs-lookup"><span data-stu-id="49676-154">Controller</span></span>
* <span data-ttu-id="49676-155">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="49676-155">Controller action method</span></span>

<span data-ttu-id="49676-156">您可以將不同原則套用至控制器/頁面模型/動作與`[EnableCors]`屬性。</span><span class="sxs-lookup"><span data-stu-id="49676-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="49676-157">當`[EnableCors]`屬性會套用至控制器/頁面模型/動作方法，並啟用 CORS 中介軟體中，這兩項原則會套用。</span><span class="sxs-lookup"><span data-stu-id="49676-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="49676-158">我們建議您不要組合原則。</span><span class="sxs-lookup"><span data-stu-id="49676-158">We recommend against combining policies.</span></span> <span data-ttu-id="49676-159">使用`[EnableCors]`屬性或中介軟體，不能同時在相同的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="49676-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="49676-160">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="49676-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="49676-161">下列程式碼會建立 CORS 預設原則和原則，名為`"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="49676-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="49676-162">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="49676-162">Disable CORS</span></span>

<span data-ttu-id="49676-163">[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)控制器/頁面模型/動作屬性停用 CORS。</span><span class="sxs-lookup"><span data-stu-id="49676-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="49676-164">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="49676-164">CORS policy options</span></span>

<span data-ttu-id="49676-165">本章節描述各種選項，可在 CORS 原則設定：</span><span class="sxs-lookup"><span data-stu-id="49676-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="49676-166">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="49676-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="49676-167">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="49676-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="49676-168">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="49676-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="49676-169">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="49676-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="49676-170">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="49676-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="49676-171">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="49676-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="49676-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> 在中，稱為`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="49676-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="49676-173">如需一些選項，可能會很有幫助讀取[運作方式的 CORS](#how-cors)區段第一次。</span><span class="sxs-lookup"><span data-stu-id="49676-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="49676-174">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="49676-174">Set the allowed origins</span></span>

<span data-ttu-id="49676-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許來自任何配置的所有原始網域的 CORS 要求 (`http`或`https`)。</span><span class="sxs-lookup"><span data-stu-id="49676-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="49676-176">`AllowAnyOrigin` 不安全因為*任何網站*可以將跨原始來源要求對應用程式。</span><span class="sxs-lookup"><span data-stu-id="49676-176">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="49676-177">指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="49676-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="49676-178">這兩種方法以設定應用程式時，CORS 服務就會傳回 CORS 回應無效。</span><span class="sxs-lookup"><span data-stu-id="49676-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="49676-179">指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="49676-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="49676-180">安全的應用程式中，指定確切的原始來源清單，如果用戶端，必須獲得授權存取伺服器資源本身。</span><span class="sxs-lookup"><span data-stu-id="49676-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

<span data-ttu-id="49676-181">`AllowAnyOrigin` 會影響預檢要求，`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-181">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="49676-182">如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="49676-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="49676-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 設定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>屬性是可讓正在評估是否允許來源時，符合設定的萬用字元網域的原始來源的函式的原則。</span><span class="sxs-lookup"><span data-stu-id="49676-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="49676-184">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="49676-184">Set the allowed HTTP methods</span></span>

<span data-ttu-id="49676-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>：</span><span class="sxs-lookup"><span data-stu-id="49676-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="49676-186">可讓任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="49676-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="49676-187">會影響預檢要求，`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="49676-188">如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="49676-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="49676-189">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="49676-189">Set the allowed request headers</span></span>

<span data-ttu-id="49676-190">若要允許特定的標頭傳送在 CORS 要求中，呼叫*撰寫要求標頭*，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="49676-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="49676-191">若要允許所有 author 要求標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="49676-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="49676-192">此設定會影響預檢要求，`Access-Control-Request-Headers`標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="49676-193">如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="49676-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="49676-194">CORS 中介軟體原則相符項目所指定的特定標頭`WithHeaders`傳送的標頭時才有可能`Access-Control-Request-Headers`完全符合的標頭中所述`WithHeaders`。</span><span class="sxs-lookup"><span data-stu-id="49676-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="49676-195">比方說，假設應用程式設定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="49676-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="49676-196">CORS 中介軟體會拒絕具有以下要求標頭的預檢要求，因為`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 中未列出`WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="49676-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="49676-197">應用程式會傳回*200 確定*回應但不會傳送回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="49676-198">因此，在瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="49676-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="49676-199">CORS 中介軟體一律允許四個中的標頭`Access-Control-Request-Headers`傳送不論 CorsPolicy.Headers 中設定的值。</span><span class="sxs-lookup"><span data-stu-id="49676-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="49676-200">此標頭的清單包括：</span><span class="sxs-lookup"><span data-stu-id="49676-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="49676-201">比方說，假設應用程式設定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="49676-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="49676-202">CORS 中介軟體成功回應下列要求標頭的預檢要求因為`Content-Language`總是列入允許清單：</span><span class="sxs-lookup"><span data-stu-id="49676-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="49676-203">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="49676-203">Set the exposed response headers</span></span>

<span data-ttu-id="49676-204">根據預設，瀏覽器不會將所有應用程式的回應標頭公開。</span><span class="sxs-lookup"><span data-stu-id="49676-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="49676-205">如需詳細資訊，請參閱[W3C 跨原始資源共用 （術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="49676-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="49676-206">是預設可用的回應標頭如下：</span><span class="sxs-lookup"><span data-stu-id="49676-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="49676-207">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="49676-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="49676-208">若要讓其他標頭使用應用程式，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="49676-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="49676-209">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="49676-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="49676-210">認證需要在 CORS 要求的特殊處理。</span><span class="sxs-lookup"><span data-stu-id="49676-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="49676-211">根據預設，瀏覽器不會傳送具有跨原始要求的認證。</span><span class="sxs-lookup"><span data-stu-id="49676-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="49676-212">認證包含 cookie 與 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="49676-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="49676-213">若要傳送與跨原始要求的認證，用戶端必須將`XMLHttpRequest.withCredentials`至`true`。</span><span class="sxs-lookup"><span data-stu-id="49676-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="49676-214">使用`XMLHttpRequest`直接：</span><span class="sxs-lookup"><span data-stu-id="49676-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="49676-215">使用 jQuery:</span><span class="sxs-lookup"><span data-stu-id="49676-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="49676-216">使用[擷取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="49676-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="49676-217">伺服器必須允許的認證。</span><span class="sxs-lookup"><span data-stu-id="49676-217">The server must allow the credentials.</span></span> <span data-ttu-id="49676-218">若要允許跨原始來源的認證，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="49676-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="49676-219">HTTP 回應包含`Access-Control-Allow-Credentials`標頭，它會告訴瀏覽器伺服器允許跨原始來源要求認證。</span><span class="sxs-lookup"><span data-stu-id="49676-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="49676-220">如果瀏覽器傳送認證，但是回應沒有包含有效`Access-Control-Allow-Credentials`標頭，瀏覽器不會公開應用程式，回應及跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="49676-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="49676-221">允許跨原始來源的認證會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="49676-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="49676-222">網站，以在另一個網域可以傳送給使用者不知情的情況下代表的使用者上的應用程式的登入使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="49676-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="49676-223">CORS 規格也會指出該設定來源，則`"*"`（所有原始網域） 是無效的如果`Access-Control-Allow-Credentials`標頭已存在。</span><span class="sxs-lookup"><span data-stu-id="49676-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="49676-224">預檢要求</span><span class="sxs-lookup"><span data-stu-id="49676-224">Preflight requests</span></span>

<span data-ttu-id="49676-225">對於某些 CORS 要求，瀏覽器會傳送其他要求，再進行實際的要求。</span><span class="sxs-lookup"><span data-stu-id="49676-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="49676-226">此要求會呼叫*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="49676-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="49676-227">如果下列條件成立，則瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="49676-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="49676-228">要求方法是 GET、 HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="49676-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="49676-229">應用程式不會設定要求標頭以外`Accept`， `Accept-Language`， `Content-Language`， `Content-Type`，或`Last-Event-ID`。</span><span class="sxs-lookup"><span data-stu-id="49676-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="49676-230">`Content-Type`標頭，如果設定，有下列值之一：</span><span class="sxs-lookup"><span data-stu-id="49676-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="49676-231">在要求標頭的規則集套用至應用程式設定所呼叫的標頭的用戶端要求`setRequestHeader`上`XMLHttpRequest`物件。</span><span class="sxs-lookup"><span data-stu-id="49676-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="49676-232">CORS 規格會呼叫這些標頭*撰寫要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="49676-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="49676-233">規則不適用於瀏覽器可以設定，例如標頭`User-Agent`， `Host`，或`Content-Length`。</span><span class="sxs-lookup"><span data-stu-id="49676-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="49676-234">預檢要求的範例如下：</span><span class="sxs-lookup"><span data-stu-id="49676-234">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="49676-235">事前要求使用 HTTP OPTIONS; 方法。</span><span class="sxs-lookup"><span data-stu-id="49676-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="49676-236">它包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="49676-236">It includes two special headers:</span></span>

* <span data-ttu-id="49676-237">`Access-Control-Request-Method`：將會用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="49676-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="49676-238">`Access-Control-Request-Headers`：在實際的要求設定的應用程式的要求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="49676-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="49676-239">如稍早所述，這不包含標頭，以瀏覽器設定，例如`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="49676-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="49676-240">CORS 預檢要求可能包括`Access-Control-Request-Headers`標頭，會向伺服器指出傳送實際要求的標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="49676-241">若要允許特定的標頭，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="49676-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="49676-242">若要允許所有 author 要求標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="49676-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="49676-243">瀏覽器未在設定的方式完全一致`Access-Control-Request-Headers`。</span><span class="sxs-lookup"><span data-stu-id="49676-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="49676-244">如果您將設定標頭的任何項目以外`"*"`(或使用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)，您應該至少包含`Accept`， `Content-Type`，和`Origin`，再加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="49676-245">以下是範例回應預檢要求 （假設伺服器允許的要求）：</span><span class="sxs-lookup"><span data-stu-id="49676-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="49676-246">回應包括`Access-Control-Allow-Methods`標頭，其中列出允許的方法和 （選擇性）`Access-Control-Allow-Headers`標頭，它會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="49676-247">如果預檢要求成功，則瀏覽器會傳送實際要求。</span><span class="sxs-lookup"><span data-stu-id="49676-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="49676-248">如果預檢要求遭到拒絕，應用程式會傳回*200 確定*回應但不會傳送回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="49676-249">因此，在瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="49676-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="49676-250">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="49676-250">Set the preflight expiration time</span></span>

<span data-ttu-id="49676-251">`Access-Control-Max-Age`標頭會指定多久可以快取預檢要求的回應。</span><span class="sxs-lookup"><span data-stu-id="49676-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="49676-252">若要設定此標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="49676-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="49676-253">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="49676-253">How CORS works</span></span>

<span data-ttu-id="49676-254">本章節描述中所發生情況[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求的 HTTP 訊息層級。</span><span class="sxs-lookup"><span data-stu-id="49676-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="49676-255">CORS 是**不**的安全性功能。</span><span class="sxs-lookup"><span data-stu-id="49676-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="49676-256">CORS 是 W3C 標準，可讓伺服器放寬同源原則。</span><span class="sxs-lookup"><span data-stu-id="49676-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="49676-257">比方說，無法使用惡意執行者[防止跨網站指令碼 (XSS)](xref:security/cross-site-scripting)對您的網站並執行其已啟用 CORS 的站台的跨網站要求竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="49676-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="49676-258">您的 API 不允許 CORS 較為安全。</span><span class="sxs-lookup"><span data-stu-id="49676-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="49676-259">它由用戶端 （瀏覽器） 強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="49676-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="49676-260">伺服器會執行要求，並傳回回應，它是發生錯誤，區塊會將回應傳回用戶端。</span><span class="sxs-lookup"><span data-stu-id="49676-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="49676-261">例如，任何下列工具會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="49676-261">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="49676-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="49676-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="49676-263">Postman</span><span class="sxs-lookup"><span data-stu-id="49676-263">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="49676-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="49676-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="49676-265">網頁瀏覽器的網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="49676-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="49676-266">它可讓伺服器，以允許瀏覽器執行跨原始來源[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或是[擷取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)否則會禁止的要求。</span><span class="sxs-lookup"><span data-stu-id="49676-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="49676-267">（不含 CORS) 的瀏覽器無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="49676-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="49676-268">CORS，再[JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)用來規避這項限制。</span><span class="sxs-lookup"><span data-stu-id="49676-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="49676-269">JSONP 不會使用 XHR，它會使用`<script>`接收回應的標記。</span><span class="sxs-lookup"><span data-stu-id="49676-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="49676-270">要載入的跨原始來源系統允許指令碼。</span><span class="sxs-lookup"><span data-stu-id="49676-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="49676-271">[CORS 規格](https://www.w3.org/TR/cors/)引進了數個新的 HTTP 標頭啟用跨源要求。</span><span class="sxs-lookup"><span data-stu-id="49676-271">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="49676-272">如果瀏覽器支援 CORS，它會設定自動跨原始來源要求這些標頭。</span><span class="sxs-lookup"><span data-stu-id="49676-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="49676-273">若要啟用 CORS，不需要自訂 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="49676-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="49676-274">以下是跨原始要求的範例。</span><span class="sxs-lookup"><span data-stu-id="49676-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="49676-275">`Origin`標頭提供網站提出要求的網域：</span><span class="sxs-lookup"><span data-stu-id="49676-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="49676-276">如果伺服器允許的要求，它會設定`Access-Control-Allow-Origin`標頭回應中的。</span><span class="sxs-lookup"><span data-stu-id="49676-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="49676-277">此標頭的值可能是符合`Origin`要求標頭或萬用字元值`"*"`，允許任何來源的意義：</span><span class="sxs-lookup"><span data-stu-id="49676-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="49676-278">如果回應不包含`Access-Control-Allow-Origin`標頭，跨原始來源要求就會失敗。</span><span class="sxs-lookup"><span data-stu-id="49676-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="49676-279">具體而言，瀏覽器不允許要求。</span><span class="sxs-lookup"><span data-stu-id="49676-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="49676-280">即使伺服器會傳回成功的回應，瀏覽器不提供回應給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="49676-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="49676-281">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="49676-281">Test CORS</span></span>

<span data-ttu-id="49676-282">若要測試 CORS:</span><span class="sxs-lookup"><span data-stu-id="49676-282">To test CORS:</span></span>

1. <span data-ttu-id="49676-283">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="49676-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="49676-284">或者，您可以[下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="49676-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="49676-285">啟用 CORS，本文件中使用其中一種方法。</span><span class="sxs-lookup"><span data-stu-id="49676-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="49676-286">例如: </span><span class="sxs-lookup"><span data-stu-id="49676-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="49676-287">`WithOrigins("https://localhost:<port>");` 應該只用於測試的範例應用程式類似於[下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="49676-287">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="49676-288">建立 web 應用程式專案 （Razor 頁面或 MVC）。</span><span class="sxs-lookup"><span data-stu-id="49676-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="49676-289">此範例會使用 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="49676-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="49676-290">您可以在相同的方案，為 API 專案中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="49676-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="49676-291">將下列反白顯示的程式碼，加入*Index.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="49676-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="49676-292">在上述程式碼，取代`url: 'https://<web app>.azurewebsites.net/api/values/1',`以部署的應用程式的 url。</span><span class="sxs-lookup"><span data-stu-id="49676-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="49676-293">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="49676-293">Deploy the API project.</span></span> <span data-ttu-id="49676-294">例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="49676-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="49676-295">從桌面執行的 Razor 頁面或 MVC 應用程式，然後按一下**測試** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="49676-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="49676-296">您可以使用 F12 工具來檢閱錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="49676-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="49676-297">移除從 localhost 原點`WithOrigins`部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="49676-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="49676-298">或者，執行用戶端應用程式使用不同的連接埠。</span><span class="sxs-lookup"><span data-stu-id="49676-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="49676-299">例如，從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="49676-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="49676-300">使用用戶端應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="49676-300">Test with the client app.</span></span> <span data-ttu-id="49676-301">CORS 錯誤會傳回錯誤，但無法使用 JavaScript 的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="49676-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="49676-302">使用 F12 工具中的 [主控台] 索引標籤，來查看錯誤。</span><span class="sxs-lookup"><span data-stu-id="49676-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="49676-303">根據瀏覽器中，您收到錯誤 （在 [F12 工具] 主控台中） 如下所示：</span><span class="sxs-lookup"><span data-stu-id="49676-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="49676-304">使用 Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="49676-304">Using Microsoft Edge:</span></span>

     <span data-ttu-id="49676-305">**SEC7120: [CORS] 原點`https://localhost:44375`找不到`https://localhost:44375`中跨原始來源資源的存取控制-允許-原始回應標頭 `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="49676-305">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="49676-306">使用 Chrome:</span><span class="sxs-lookup"><span data-stu-id="49676-306">Using Chrome:</span></span>

     <span data-ttu-id="49676-307">**存取在 XMLHttpRequest`https://webapi.azurewebsites.net/api/values/1`從原點`https://localhost:44375`已封鎖的 CORS 原則：要求的資源上有沒有 '存取控制-允許-原始' 標頭。**</span><span class="sxs-lookup"><span data-stu-id="49676-307">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49676-308">其他資源</span><span class="sxs-lookup"><span data-stu-id="49676-308">Additional resources</span></span>

* [<span data-ttu-id="49676-309">跨原始資源共用 (CORS)</span><span class="sxs-lookup"><span data-stu-id="49676-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
