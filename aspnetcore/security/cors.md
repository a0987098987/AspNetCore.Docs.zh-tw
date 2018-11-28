---
title: 啟用 ASP.NET Core 中的跨源要求 (CORS)
author: rick-anderson
description: 了解如何為標準，以允許或拒絕在 ASP.NET Core 應用程式的跨原始要求的 CORS。
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458539"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="ed85e-103">啟用 ASP.NET Core 中的跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="ed85e-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="ed85e-104">藉由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ed85e-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ed85e-105">瀏覽器安全性可防止網頁對不同的網域服務之 web 網頁提出要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="ed85e-106">這項限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="ed85e-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="ed85e-107">同源原則會防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="ed85e-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ed85e-108">有時候，您可能要允許其他站台會將跨原始來源要求對您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed85e-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="ed85e-109">[跨原始資源共用](https://www.w3.org/TR/cors/)(CORS) 是 W3C 標準，可讓伺服器放寬同源原則。</span><span class="sxs-lookup"><span data-stu-id="ed85e-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="ed85e-110">使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。</span><span class="sxs-lookup"><span data-stu-id="ed85e-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="ed85e-111">CORS 可較為安全且更有彈性，比早期的技術，例如[JSONP](https://wikipedia.org/wiki/JSONP)。</span><span class="sxs-lookup"><span data-stu-id="ed85e-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="ed85e-112">本主題說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="ed85e-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="ed85e-113">婞鎏磭懘</span><span class="sxs-lookup"><span data-stu-id="ed85e-113">Same origin</span></span>

<span data-ttu-id="ed85e-114">兩個 Url 有相同的原點，如果它們有相同的配置、 主機和連接埠 ([RFC 6454](https://tools.ietf.org/html/rfc6454))。</span><span class="sxs-lookup"><span data-stu-id="ed85e-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="ed85e-115">這些兩個 Url 有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="ed85e-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="ed85e-116">這些 Url 會有不同的來源，比先前的兩個 Url:</span><span class="sxs-lookup"><span data-stu-id="ed85e-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="ed85e-117">`https://example.net` &ndash; 不同的網域</span><span class="sxs-lookup"><span data-stu-id="ed85e-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="ed85e-118">`https://www.example.com/foo.html` &ndash; 不同的子網域</span><span class="sxs-lookup"><span data-stu-id="ed85e-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="ed85e-119">`http://example.com/foo.html` &ndash; 不同的配置</span><span class="sxs-lookup"><span data-stu-id="ed85e-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="ed85e-120">`https://example.com:9000/foo.html` &ndash; 不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="ed85e-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="ed85e-121">比較原始來源時，Internet Explorer 不會視為連接埠。</span><span class="sxs-lookup"><span data-stu-id="ed85e-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="ed85e-122">註冊的 CORS 服務</span><span class="sxs-lookup"><span data-stu-id="ed85e-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ed85e-123">參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ed85e-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ed85e-124">參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或新增的套件參考[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ed85e-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ed85e-125">新增的套件參考[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ed85e-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="ed85e-126">呼叫<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>在`Startup.ConfigureServices`將 CORS 服務加入至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="ed85e-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="ed85e-127">啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="ed85e-127">Enable CORS</span></span>

<span data-ttu-id="ed85e-128">註冊之後的 CORS 服務，使用下列其中一個方法來啟用 CORS 的 ASP.NET Core 應用程式中：</span><span class="sxs-lookup"><span data-stu-id="ed85e-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="ed85e-129">[CORS 中介軟體](#enable-cors-with-cors-middleware)&ndash;套用 CORS 原則全域透過中介軟體應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed85e-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="ed85e-130">[在 MVC 中的 CORS](#enable-cors-in-mvc) &ndash;套用 CORS 原則，每個動作，或每個控制站。</span><span class="sxs-lookup"><span data-stu-id="ed85e-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="ed85e-131">不使用 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ed85e-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="ed85e-132">啟用 CORS 與 CORS 中介軟體</span><span class="sxs-lookup"><span data-stu-id="ed85e-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="ed85e-133">CORS 中介軟體會處理應用程式的跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="ed85e-134">若要啟用 CORS 中介軟體在要求處理管線中，呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>擴充方法`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="ed85e-135">CORS 中介軟體必須在前面定義的端點在您的應用程式中要支援跨原始來源要求 (例如，再呼叫`UseMvc`MVC/Razor 頁面中介軟體)。</span><span class="sxs-lookup"><span data-stu-id="ed85e-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="ed85e-136">A*跨源原則*新增 CORS 中介軟體使用時，可以指定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>類別。</span><span class="sxs-lookup"><span data-stu-id="ed85e-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="ed85e-137">有兩個方法定義的 CORS 原則：</span><span class="sxs-lookup"><span data-stu-id="ed85e-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="ed85e-138">呼叫`UseCors`使用 lambda:</span><span class="sxs-lookup"><span data-stu-id="ed85e-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="ed85e-139">Lambda 會採用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>物件。</span><span class="sxs-lookup"><span data-stu-id="ed85e-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="ed85e-140">[組態選項](#cors-policy-options)，例如`WithOrigins`，本主題稍後所述。</span><span class="sxs-lookup"><span data-stu-id="ed85e-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="ed85e-141">在上述範例中，該原則可讓跨源要求，從`https://example.com`和其他來源。</span><span class="sxs-lookup"><span data-stu-id="ed85e-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="ed85e-142">必須指定不含尾端斜線的 URL (`/`)。</span><span class="sxs-lookup"><span data-stu-id="ed85e-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="ed85e-143">如果 URL 終止`/`，比較傳回`false`並傳回不含標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="ed85e-144">`CorsPolicyBuilder` 有 fluent API，因此您可以在方法呼叫鏈結：</span><span class="sxs-lookup"><span data-stu-id="ed85e-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="ed85e-145">定義一或多個具名的 CORS 原則，並在執行階段的名稱以選取原則。</span><span class="sxs-lookup"><span data-stu-id="ed85e-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="ed85e-146">下列範例會將名為使用者定義的 CORS 原則*AllowSpecificOrigin*。</span><span class="sxs-lookup"><span data-stu-id="ed85e-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="ed85e-147">若要選取原則，將名稱傳遞給`UseCors`:</span><span class="sxs-lookup"><span data-stu-id="ed85e-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="ed85e-148">啟用在 MVC 中的 CORS</span><span class="sxs-lookup"><span data-stu-id="ed85e-148">Enable CORS in MVC</span></span>

<span data-ttu-id="ed85e-149">您也可以使用 MVC 套用特定 CORS 原則，每個動作，或每個控制器。</span><span class="sxs-lookup"><span data-stu-id="ed85e-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="ed85e-150">您可以使用 MVC 啟用 CORS，會使用已註冊的 CORS 服務。</span><span class="sxs-lookup"><span data-stu-id="ed85e-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="ed85e-151">不使用 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ed85e-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="ed85e-152">每個動作</span><span class="sxs-lookup"><span data-stu-id="ed85e-152">Per action</span></span>

<span data-ttu-id="ed85e-153">若要指定特定動作的 CORS 原則，請新增[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="ed85e-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="ed85e-154">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="ed85e-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="ed85e-155">每個控制站</span><span class="sxs-lookup"><span data-stu-id="ed85e-155">Per controller</span></span>

<span data-ttu-id="ed85e-156">若要指定特定的控制站的 CORS 原則，請新增[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性至控制器類別。</span><span class="sxs-lookup"><span data-stu-id="ed85e-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="ed85e-157">指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="ed85e-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="ed85e-158">優先順序是：</span><span class="sxs-lookup"><span data-stu-id="ed85e-158">The precedence order is:</span></span>

1. <span data-ttu-id="ed85e-159">action</span><span class="sxs-lookup"><span data-stu-id="ed85e-159">action</span></span>
1. <span data-ttu-id="ed85e-160">控制器</span><span class="sxs-lookup"><span data-stu-id="ed85e-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="ed85e-161">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="ed85e-161">Disable CORS</span></span>

<span data-ttu-id="ed85e-162">若要停用控制器或動作的 CORS，請使用[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性：</span><span class="sxs-lookup"><span data-stu-id="ed85e-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="ed85e-163">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="ed85e-163">CORS policy options</span></span>

<span data-ttu-id="ed85e-164">本章節描述您可以設定的 CORS 原則中的各種選項。</span><span class="sxs-lookup"><span data-stu-id="ed85e-164">This section describes the various options that you can set in a CORS policy.</span></span> <span data-ttu-id="ed85e-165"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>方法呼叫`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-165">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> method is called in `Startup.ConfigureServices`.</span></span>

* [<span data-ttu-id="ed85e-166">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="ed85e-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="ed85e-167">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="ed85e-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="ed85e-168">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="ed85e-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="ed85e-169">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="ed85e-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="ed85e-170">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="ed85e-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="ed85e-171">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="ed85e-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="ed85e-172">如需一些選項，可能會很有幫助讀取[運作方式的 CORS](#how-cors-works)區段第一次。</span><span class="sxs-lookup"><span data-stu-id="ed85e-172">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="ed85e-173">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="ed85e-173">Set the allowed origins</span></span>

<span data-ttu-id="ed85e-174">ASP.NET Core MVC 中的 CORS 中介軟體有幾種方式可以指定允許的來源：</span><span class="sxs-lookup"><span data-stu-id="ed85e-174">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="ed85e-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; 可讓您指定一個或多個 Url。</span><span class="sxs-lookup"><span data-stu-id="ed85e-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Allows specifying one or more URLs.</span></span> <span data-ttu-id="ed85e-176">URL 可能包含配置、 主機名稱和連接埠，但不含任何路徑資訊。</span><span class="sxs-lookup"><span data-stu-id="ed85e-176">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="ed85e-177">例如， `https://example.com` 。</span><span class="sxs-lookup"><span data-stu-id="ed85e-177">For example, `https://example.com`.</span></span> <span data-ttu-id="ed85e-178">必須指定不含尾端斜線的 URL (`/`)。</span><span class="sxs-lookup"><span data-stu-id="ed85e-178">The URL must be specified without a trailing slash (`/`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <span data-ttu-id="ed85e-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許來自任何配置的所有原始網域的 CORS 要求 (`http`或`https`)。</span><span class="sxs-lookup"><span data-stu-id="ed85e-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  <span data-ttu-id="ed85e-180">請仔細考慮，才能允許來自任何來源的要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-180">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="ed85e-181">允許來自任何來源的要求表示*任何網站*可以將跨原始來源要求對您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed85e-181">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="ed85e-182">指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="ed85e-183">這兩種方法以設定應用程式時，CORS 服務就會傳回 CORS 回應無效。</span><span class="sxs-lookup"><span data-stu-id="ed85e-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="ed85e-184">指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="ed85e-185">請考慮指定確切的原始來源清單，如果用戶端，必須獲得授權存取伺服器資源本身。</span><span class="sxs-lookup"><span data-stu-id="ed85e-185">Consider specifying an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

  <span data-ttu-id="ed85e-186">此設定會影響預檢要求，`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-186">This setting affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="ed85e-187">如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="ed85e-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="ed85e-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 設定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>原則以允許符合設定含萬用字元網域，如果原始來源允許在評估時的原始來源的函式的屬性。</span><span class="sxs-lookup"><span data-stu-id="ed85e-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcarded domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="ed85e-189">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="ed85e-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="ed85e-190">若要允許所有的 HTTP 方法，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-190">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

<span data-ttu-id="ed85e-191">此設定會影響預檢要求，`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-191">This setting affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="ed85e-192">如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="ed85e-192">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="ed85e-193">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="ed85e-193">Set the allowed request headers</span></span>

<span data-ttu-id="ed85e-194">若要允許特定的標頭傳送在 CORS 要求中，呼叫*撰寫要求標頭*，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="ed85e-194">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="ed85e-195">若要允許所有 author 要求標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-195">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="ed85e-196">此設定會影響預檢要求，`Access-Control-Request-Headers`標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-196">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="ed85e-197">如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="ed85e-197">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ed85e-198">CORS 中介軟體原則相符項目所指定的特定標頭`WithHeaders`傳送的標頭時才有可能`Access-Control-Request-Headers`完全符合的標頭中所述`WithHeaders`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-198">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="ed85e-199">比方說，假設應用程式設定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed85e-199">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="ed85e-200">CORS 中介軟體會拒絕具有以下要求標頭的預檢要求，因為`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 中未列出`WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="ed85e-200">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="ed85e-201">應用程式會傳回*200 確定*回應但不會傳送回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-201">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="ed85e-202">因此，在瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-202">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ed85e-203">CORS 中介軟體一律允許四個中的標頭`Access-Control-Request-Headers`傳送不論 CorsPolicy.Headers 中設定的值。</span><span class="sxs-lookup"><span data-stu-id="ed85e-203">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="ed85e-204">此標頭的清單包括：</span><span class="sxs-lookup"><span data-stu-id="ed85e-204">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="ed85e-205">比方說，假設應用程式設定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed85e-205">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="ed85e-206">CORS 中介軟體成功回應下列要求標頭的預檢要求因為`Content-Language`總是列入允許清單：</span><span class="sxs-lookup"><span data-stu-id="ed85e-206">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="ed85e-207">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="ed85e-207">Set the exposed response headers</span></span>

<span data-ttu-id="ed85e-208">根據預設，瀏覽器不會將所有應用程式的回應標頭公開。</span><span class="sxs-lookup"><span data-stu-id="ed85e-208">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="ed85e-209">如需詳細資訊，請參閱 < [W3C 跨原始資源共用 （術語）： 簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="ed85e-209">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="ed85e-210">是預設可用的回應標頭如下：</span><span class="sxs-lookup"><span data-stu-id="ed85e-210">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="ed85e-211">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="ed85e-211">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="ed85e-212">若要讓其他標頭使用應用程式，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-212">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="ed85e-213">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="ed85e-213">Credentials in cross-origin requests</span></span>

<span data-ttu-id="ed85e-214">認證需要在 CORS 要求的特殊處理。</span><span class="sxs-lookup"><span data-stu-id="ed85e-214">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="ed85e-215">根據預設，瀏覽器不會傳送具有跨原始要求的認證。</span><span class="sxs-lookup"><span data-stu-id="ed85e-215">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="ed85e-216">認證包含 cookie 與 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="ed85e-216">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="ed85e-217">若要傳送與跨原始要求的認證，用戶端必須將`XMLHttpRequest.withCredentials`至`true`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-217">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="ed85e-218">使用`XMLHttpRequest`直接：</span><span class="sxs-lookup"><span data-stu-id="ed85e-218">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="ed85e-219">在 jQuery 中：</span><span class="sxs-lookup"><span data-stu-id="ed85e-219">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="ed85e-220">此外，伺服器必須允許的認證。</span><span class="sxs-lookup"><span data-stu-id="ed85e-220">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="ed85e-221">若要允許跨原始來源的認證，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-221">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="ed85e-222">HTTP 回應包含`Access-Control-Allow-Credentials`標頭，它會告訴瀏覽器伺服器允許跨原始來源要求認證。</span><span class="sxs-lookup"><span data-stu-id="ed85e-222">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="ed85e-223">如果瀏覽器傳送認證，但是回應沒有包含有效`Access-Control-Allow-Credentials`標頭，瀏覽器不會公開應用程式，回應及跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="ed85e-223">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="ed85e-224">允許跨原始來源的認證時要小心。</span><span class="sxs-lookup"><span data-stu-id="ed85e-224">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="ed85e-225">網站，以在另一個網域可以傳送給使用者不知情的情況下代表的使用者上的應用程式的登入使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="ed85e-225">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="ed85e-226">CORS 規格也會指出該設定來源，則`"*"`（所有原始網域） 是無效的如果`Access-Control-Allow-Credentials`標頭已存在。</span><span class="sxs-lookup"><span data-stu-id="ed85e-226">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="ed85e-227">預檢要求</span><span class="sxs-lookup"><span data-stu-id="ed85e-227">Preflight requests</span></span>

<span data-ttu-id="ed85e-228">對於某些 CORS 要求，瀏覽器會傳送其他要求，再進行實際的要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-228">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="ed85e-229">此要求會呼叫*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="ed85e-229">This request is called a *preflight request*.</span></span> <span data-ttu-id="ed85e-230">如果下列條件成立，則瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="ed85e-230">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="ed85e-231">要求方法是 GET、 HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="ed85e-231">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="ed85e-232">應用程式不會設定要求標頭以外`Accept`， `Accept-Language`， `Content-Language`， `Content-Type`，或`Last-Event-ID`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-232">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="ed85e-233">`Content-Type`標頭，如果設定，有下列值之一：</span><span class="sxs-lookup"><span data-stu-id="ed85e-233">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="ed85e-234">在要求標頭的規則集套用至應用程式設定所呼叫的標頭的用戶端要求`setRequestHeader`上`XMLHttpRequest`物件。</span><span class="sxs-lookup"><span data-stu-id="ed85e-234">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="ed85e-235">CORS 規格會呼叫這些標頭*撰寫要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="ed85e-235">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="ed85e-236">規則不適用於瀏覽器可以設定，例如標頭`User-Agent`， `Host`，或`Content-Length`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-236">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="ed85e-237">預檢要求的範例如下：</span><span class="sxs-lookup"><span data-stu-id="ed85e-237">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="ed85e-238">事前要求使用 HTTP OPTIONS; 方法。</span><span class="sxs-lookup"><span data-stu-id="ed85e-238">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="ed85e-239">它包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="ed85e-239">It includes two special headers:</span></span>

* <span data-ttu-id="ed85e-240">`Access-Control-Request-Method`: 將會用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="ed85e-240">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="ed85e-241">`Access-Control-Request-Headers`： 一份應用程式設定實際要求的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-241">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="ed85e-242">如稍早所述，這不包含標頭，以瀏覽器設定，例如`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-242">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="ed85e-243">CORS 預檢要求可能包括`Access-Control-Request-Headers`標頭，會向伺服器指出傳送實際要求的標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-243">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="ed85e-244">若要允許特定的標頭，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-244">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="ed85e-245">若要允許所有 author 要求標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-245">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="ed85e-246">瀏覽器未在設定的方式完全一致`Access-Control-Request-Headers`。</span><span class="sxs-lookup"><span data-stu-id="ed85e-246">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="ed85e-247">如果您將設定標頭的任何項目以外`"*"`(或使用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)，您應該至少包含`Accept`， `Content-Type`，和`Origin`，再加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-247">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="ed85e-248">以下是範例回應預檢要求 （假設伺服器允許的要求）：</span><span class="sxs-lookup"><span data-stu-id="ed85e-248">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="ed85e-249">回應包括`Access-Control-Allow-Methods`標頭，其中列出允許的方法和 （選擇性）`Access-Control-Allow-Headers`標頭，它會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-249">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="ed85e-250">如果預檢要求成功，則瀏覽器會傳送實際要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-250">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="ed85e-251">如果預檢要求遭到拒絕，應用程式會傳回*200 確定*回應但不會傳送回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-251">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="ed85e-252">因此，在瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-252">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="ed85e-253">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="ed85e-253">Set the preflight expiration time</span></span>

<span data-ttu-id="ed85e-254">`Access-Control-Max-Age`標頭會指定多久可以快取預檢要求的回應。</span><span class="sxs-lookup"><span data-stu-id="ed85e-254">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="ed85e-255">若要設定此標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="ed85e-255">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="ed85e-256">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="ed85e-256">How CORS works</span></span>

<span data-ttu-id="ed85e-257">本章節描述 CORS 要求的 HTTP 訊息層級中發生的動作。</span><span class="sxs-lookup"><span data-stu-id="ed85e-257">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="ed85e-258">請務必了解，這樣可以正確地設定和非預期的行為發生時偵錯的 CORS 原則，CORS 的運作方式。</span><span class="sxs-lookup"><span data-stu-id="ed85e-258">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="ed85e-259">CORS 規格引進了數個新的 HTTP 標頭啟用跨源要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-259">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="ed85e-260">如果瀏覽器支援 CORS，它會設定自動跨原始來源要求這些標頭。</span><span class="sxs-lookup"><span data-stu-id="ed85e-260">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="ed85e-261">若要啟用 CORS，不需要自訂 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="ed85e-261">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="ed85e-262">以下是跨原始要求的範例。</span><span class="sxs-lookup"><span data-stu-id="ed85e-262">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="ed85e-263">`Origin`標頭提供網站提出要求的網域：</span><span class="sxs-lookup"><span data-stu-id="ed85e-263">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="ed85e-264">如果伺服器允許的要求，它會設定`Access-Control-Allow-Origin`標頭回應中的。</span><span class="sxs-lookup"><span data-stu-id="ed85e-264">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="ed85e-265">此標頭的值可能是符合`Origin`要求標頭或萬用字元值`"*"`，允許任何來源的意義：</span><span class="sxs-lookup"><span data-stu-id="ed85e-265">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="ed85e-266">如果回應不包含`Access-Control-Allow-Origin`標頭，跨原始來源要求就會失敗。</span><span class="sxs-lookup"><span data-stu-id="ed85e-266">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="ed85e-267">具體而言，瀏覽器不允許要求。</span><span class="sxs-lookup"><span data-stu-id="ed85e-267">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="ed85e-268">即使伺服器會傳回成功的回應，瀏覽器不提供回應給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed85e-268">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed85e-269">其他資源</span><span class="sxs-lookup"><span data-stu-id="ed85e-269">Additional resources</span></span>

* [<span data-ttu-id="ed85e-270">跨原始資源共用 (CORS)</span><span class="sxs-lookup"><span data-stu-id="ed85e-270">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
