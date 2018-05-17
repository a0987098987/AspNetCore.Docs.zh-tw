---
title: ASP.NET Core 的 URL 重寫中介軟體
author: guardrex
description: 了解如何使用 ASP.NET Core 的 URL 重寫中介軟體，進行 URL 重寫與重新導向作業。
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: 336a097c2186bc195854bd54211d4554a577ed14
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="f7a91-103">ASP.NET Core 的 URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="f7a91-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="f7a91-104">由 [Luke Latham](https://github.com/guardrex) 和 [Mikael Mengistu](https://github.com/mikaelm12) 提供</span><span class="sxs-lookup"><span data-stu-id="f7a91-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="f7a91-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f7a91-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f7a91-106">URL 重寫是指根據一或多個預先定義的規則來修改要求 URL 的動作。</span><span class="sxs-lookup"><span data-stu-id="f7a91-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="f7a91-107">URL 重寫會在資源位置和位址之間建立一個抽象層，讓位置和位址不那麼緊密連結。</span><span class="sxs-lookup"><span data-stu-id="f7a91-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="f7a91-108">下列幾種情況非常適合使用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="f7a91-108">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="f7a91-109">暫時或永久性移動/取代伺服器資源時，同時為這些資源保持穩定的定位器</span><span class="sxs-lookup"><span data-stu-id="f7a91-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="f7a91-110">跨不同應用程式或跨單一應用程式各區域來分割要求處理作業</span><span class="sxs-lookup"><span data-stu-id="f7a91-110">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="f7a91-111">移除、新增或重組傳入要求的 URL 區段</span><span class="sxs-lookup"><span data-stu-id="f7a91-111">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="f7a91-112">針對搜尋引擎最佳化 (SEO) 來最佳化公用 URL</span><span class="sxs-lookup"><span data-stu-id="f7a91-112">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="f7a91-113">允許使用易記的公用 URL，以協助使用者預測他們在前往連結後能找到哪些內容</span><span class="sxs-lookup"><span data-stu-id="f7a91-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="f7a91-114">將不安全的要求重新導向至安全的端點</span><span class="sxs-lookup"><span data-stu-id="f7a91-114">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="f7a91-115">防止圖片盜連</span><span class="sxs-lookup"><span data-stu-id="f7a91-115">Preventing image hotlinking</span></span>

<span data-ttu-id="f7a91-116">您可以透過數種方式來定義變更 URL 的規則，包括 Regex、Apache mod_rewrite 模組規則、IIS Rewrite Module 規則，以及使用自訂規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f7a91-116">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="f7a91-117">本文件介紹 URL 重寫，並提供如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。</span><span class="sxs-lookup"><span data-stu-id="f7a91-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="f7a91-118">URL 重寫可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="f7a91-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="f7a91-119">如果可行的話，您應該限制規則的數目與複雜程度。</span><span class="sxs-lookup"><span data-stu-id="f7a91-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="f7a91-120">URL 重新導向和 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f7a91-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="f7a91-121">乍看之下，「URL 重新導向」和「URL 重寫」在用語上的差異很微妙，但卻對提供資源給用戶端方面有重要的影響。</span><span class="sxs-lookup"><span data-stu-id="f7a91-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="f7a91-122">ASP.NET Core 的 URL 重寫中介軟體可以同時滿足這兩種需求。</span><span class="sxs-lookup"><span data-stu-id="f7a91-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="f7a91-123">「URL 重新導向」是指針對用戶端的作業，其會指示用戶端到另一個位址存取資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="f7a91-124">這項作業需要往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="f7a91-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="f7a91-125">當用戶端提出新的資源要求時，系統會將重新導向 URL 傳回給用戶端，並顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f7a91-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="f7a91-126">如果 `/resource` 是「重新導向」至 `/different-resource`，即表示用戶端要求的是 `/resource`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="f7a91-127">伺服器會回應用戶端應前往 `/different-resource` 取得資源，並提供狀態碼，指出重新導向為暫時性或永久性。</span><span class="sxs-lookup"><span data-stu-id="f7a91-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="f7a91-128">用戶端即會在重新導向 URL 處執行新的資源要求。</span><span class="sxs-lookup"><span data-stu-id="f7a91-128">The client executes a new request for the resource at the redirect URL.</span></span>

![伺服器上的 WebAPI 服務端點已暫時從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="f7a91-134">當您將要求重新導向至不同的 URL 時，可以指出該重新導向為永久性或暫時性。</span><span class="sxs-lookup"><span data-stu-id="f7a91-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="f7a91-135">如果資源具有新的永久 URL，且您希望告知用戶端所有未來的資源要求皆應使用這個新 URL，請使用 301 (已永久移動) 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f7a91-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="f7a91-136">*用戶端收到 301 狀態碼時，可能會快取回應。*</span><span class="sxs-lookup"><span data-stu-id="f7a91-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="f7a91-137">如果重新導向是暫時的且可能會變更，因此用戶端不應儲存亦不應於日後重複使用這個重新導向 URL，請使用 302 (已找到) 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f7a91-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="f7a91-138">如需詳細資訊，請參閱 [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616：狀態碼定義)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="f7a91-139">「URL 重寫」是針對伺服器端的作業，以從不同的資源位址提供資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="f7a91-140">重寫 URL 時並不需要往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="f7a91-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="f7a91-141">系統不會將重寫的 URL 傳回給用戶端，也不會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f7a91-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="f7a91-142">將 `/resource`「重寫」至 `/different-resource` 時，用戶端要求的是 `/resource`，且伺服器會於 `/different-resource`「內部」擷取資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="f7a91-143">雖然用戶端或許可以在重寫的 URL 處擷取資源，但用戶端並不會在提出要求與接收回應時得知資源位於重寫的 URL 處。</span><span class="sxs-lookup"><span data-stu-id="f7a91-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![伺服器上的 WebAPI 服務端點已從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="f7a91-148">URL 重寫範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f7a91-148">URL rewriting sample app</span></span>

<span data-ttu-id="f7a91-149">您可以使用 [URL 重寫範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/)，來探索 URL 重寫中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="f7a91-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="f7a91-150">應用程式會套用重新寫入和重新導向規則，並顯示重寫的 URL 或重新導向的 URL。</span><span class="sxs-lookup"><span data-stu-id="f7a91-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="f7a91-151">URL 重寫中介軟體的使用時機</span><span class="sxs-lookup"><span data-stu-id="f7a91-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="f7a91-152">當您無法使用 [IIS URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) (Windows Server)、[Apache mod_rewrite 模組](https://httpd.apache.org/docs/2.4/rewrite/) (Apache Server)、[Nginx上的 URL 重寫](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)，或者您的應用程式裝載於 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 之上時，請使用 URL 重寫中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f7a91-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="f7a91-153">建議您使用 IIS、Apache 或 Nginx 中的伺服器端 URL 重寫技術，主要是因為中介軟體不支援這些模組的完整功能，而且中介軟體的效能可能與模組效能不符。</span><span class="sxs-lookup"><span data-stu-id="f7a91-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="f7a91-154">不過，有一些伺服器模組的功能不適用於 ASP.NET Core 專案，例如 IIS Rewrite Module 規則的 `IsFile` 和 `IsDirectory` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="f7a91-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="f7a91-155">在這些情況下，請改為使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f7a91-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="f7a91-156">Package</span><span class="sxs-lookup"><span data-stu-id="f7a91-156">Package</span></span>

<span data-ttu-id="f7a91-157">若要在專案中包含中介軟體，請新增 [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) 套件的參考。</span><span class="sxs-lookup"><span data-stu-id="f7a91-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="f7a91-158">此功能適用於以 ASP.NET Core 1.1 或更新版本為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f7a91-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="f7a91-159">延伸模組和選項</span><span class="sxs-lookup"><span data-stu-id="f7a91-159">Extension and options</span></span>

<span data-ttu-id="f7a91-160">若要建立 URL 重寫和重新導向規則，您可以針對每個規則的擴充方法建立 `RewriteOptions` 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f7a91-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="f7a91-161">依據所需的處理順序來鏈結多個規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="f7a91-162">系統會使用 `app.UseRewriter(options);` 將 `RewriteOptions` 新增至要求管線，並將其傳入 URL 重寫中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f7a91-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

### <a name="url-redirect"></a><span data-ttu-id="f7a91-165">URL 重新導向</span><span class="sxs-lookup"><span data-stu-id="f7a91-165">URL redirect</span></span>

<span data-ttu-id="f7a91-166">使用 `AddRedirect` 將要求重新導向。</span><span class="sxs-lookup"><span data-stu-id="f7a91-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="f7a91-167">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f7a91-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="f7a91-168">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="f7a91-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="f7a91-169">第三個參數 (如果有的話) 會指定狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f7a91-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="f7a91-170">如果您未指定狀態碼，則預設為 302 (已找到)，表示已暫時移動或取代資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="f7a91-173">在啟用開發人員工具的瀏覽器中，使用 `/redirect-rule/1234/5678` 路徑提出範例應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="f7a91-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="f7a91-174">Regex 會比對 `redirect-rule/(.*)` 上的要求路徑，並會將路徑取代為 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="f7a91-175">系統會將重新導向 URL 與 302 (已找到) 狀態碼傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f7a91-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="f7a91-176">瀏覽器在重新導向 URL 處提出新要求，該 URL 也會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f7a91-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="f7a91-177">由於範例應用程式中的任何規則都不符合重新導向 URL，因此第二個要求會收到應用程式的 200 (確定) 回應，且回應主體會顯示重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="f7a91-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="f7a91-178">「重新導向」URL 時，即需往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="f7a91-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="f7a91-179">建立重新導向規則時，請務必謹慎。</span><span class="sxs-lookup"><span data-stu-id="f7a91-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="f7a91-180">每次向應用程式提出要求時 (包括重新導向後)，請評估您的重新導向規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="f7a91-181">因為您有可能會不小心建立無限重新導向迴圈。</span><span class="sxs-lookup"><span data-stu-id="f7a91-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="f7a91-182">原始要求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="f7a91-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="f7a91-184">括弧內所含的運算式部分稱為「擷取群組」。</span><span class="sxs-lookup"><span data-stu-id="f7a91-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="f7a91-185">運算式的點 (`.`) 表示「比對任何字元」。</span><span class="sxs-lookup"><span data-stu-id="f7a91-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="f7a91-186">星號 (`*`) 表示「比對前置字元零或多次」。</span><span class="sxs-lookup"><span data-stu-id="f7a91-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="f7a91-187">因此，擷取群組 `(.*)` 會擷取 URL 的 `1234/5678` 最後這兩個路徑區段。</span><span class="sxs-lookup"><span data-stu-id="f7a91-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="f7a91-188">這個單一擷取群組會擷取您在要求 URL 中的 `redirect-rule/` 之後所提供的任何值。</span><span class="sxs-lookup"><span data-stu-id="f7a91-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="f7a91-189">系統會將擷取的群組以貨幣符號 (`$`) 後接擷取序號的形式，插入取代字串中。</span><span class="sxs-lookup"><span data-stu-id="f7a91-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="f7a91-190">使用 `$1` 可取得第一個擷取群組值、使用 `$2` 則會取得第二個，依您的 Regex 擷取群組順序以此類推。</span><span class="sxs-lookup"><span data-stu-id="f7a91-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="f7a91-191">在範例應用程式中，重新導向規則 Regex 只有一個擷取的群組，因此在取代字串中只有 `$1` 這一個插入的群組。</span><span class="sxs-lookup"><span data-stu-id="f7a91-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="f7a91-192">套用規則時，URL 會變成 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="f7a91-193">將 URL 重新導向至安全端點</span><span class="sxs-lookup"><span data-stu-id="f7a91-193">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="f7a91-194">使用 `AddRedirectToHttps` 將 HTTP 要求重新導向至使用 HTTPS (`https://`) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="f7a91-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="f7a91-195">如果未提供狀態碼，中介軟體會預設為 302 (已找到)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="f7a91-196">如果未提供連接埠，中介軟體會預設為 `null`；這表示通訊協定變更為 `https://` 且用戶端會存取連接埠 443 上的資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="f7a91-197">此範例示範如何將狀態碼設為 301 (已永久移動)，並將連接埠變更為 5001。</span><span class="sxs-lookup"><span data-stu-id="f7a91-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="f7a91-198">使用 `AddRedirectToHttpsPermanent` 將不安全的要求重新導向至使用安全 HTTPS 通訊協定 (連接埠 443 上的 `https://`) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="f7a91-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="f7a91-199">中介軟體會將狀態碼設定為 301 (已永久移動)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

<span data-ttu-id="f7a91-200">範例應用程式可以示範如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-200">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="f7a91-201">將擴充方法新增至 `RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-201">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="f7a91-202">在任何 URL 位置，向應用程式提出不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="f7a91-202">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="f7a91-203">關閉瀏覽器自我簽署憑證不受信任的安全性警告。</span><span class="sxs-lookup"><span data-stu-id="f7a91-203">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="f7a91-204">使用 `AddRedirectToHttps(301, 5001)` 的原始要求：`/secure`</span><span class="sxs-lookup"><span data-stu-id="f7a91-204">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="f7a91-206">使用 `AddRedirectToHttpsPermanent` 的原始要求：`/secure`</span><span class="sxs-lookup"><span data-stu-id="f7a91-206">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="f7a91-208">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f7a91-208">URL rewrite</span></span>

<span data-ttu-id="f7a91-209">使用 `AddRewrite` 來建立 URL 重寫的規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-209">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="f7a91-210">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f7a91-210">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="f7a91-211">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="f7a91-211">The second parameter is the replacement string.</span></span> <span data-ttu-id="f7a91-212">第三個參數 `skipRemainingRules: {true|false}` 指出中介軟體是否要在套用目前規則時略過其他重寫規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-212">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-213">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="f7a91-215">原始要求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="f7a91-215">Original Request: `/rewrite-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="f7a91-217">在 Regex 中，最引人注意的是運算式開頭的 `^`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-217">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="f7a91-218">這表示，比對會從 URL 路徑的開頭開始。</span><span class="sxs-lookup"><span data-stu-id="f7a91-218">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="f7a91-219">在使用重新導向規則 `redirect-rule/(.*)` 的稍早範例中，Regex 的開頭沒有任何 ^；因此，成功比對的任何字元可能位於路徑中的 `redirect-rule/` 之前。</span><span class="sxs-lookup"><span data-stu-id="f7a91-219">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="f7a91-220">路徑</span><span class="sxs-lookup"><span data-stu-id="f7a91-220">Path</span></span>                               | <span data-ttu-id="f7a91-221">比對</span><span class="sxs-lookup"><span data-stu-id="f7a91-221">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="f7a91-222">[是]</span><span class="sxs-lookup"><span data-stu-id="f7a91-222">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="f7a91-223">[是]</span><span class="sxs-lookup"><span data-stu-id="f7a91-223">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="f7a91-224">[是]</span><span class="sxs-lookup"><span data-stu-id="f7a91-224">Yes</span></span>   |

<span data-ttu-id="f7a91-225">`^rewrite-rule/(\d+)/(\d+)` 重寫規則只會比對開頭為 `rewrite-rule/` 的路徑。</span><span class="sxs-lookup"><span data-stu-id="f7a91-225">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="f7a91-226">請注意，下列重寫規則與上述重新導向規則之間的比對差異。</span><span class="sxs-lookup"><span data-stu-id="f7a91-226">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="f7a91-227">路徑</span><span class="sxs-lookup"><span data-stu-id="f7a91-227">Path</span></span>                              | <span data-ttu-id="f7a91-228">比對</span><span class="sxs-lookup"><span data-stu-id="f7a91-228">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="f7a91-229">[是]</span><span class="sxs-lookup"><span data-stu-id="f7a91-229">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="f7a91-230">否</span><span class="sxs-lookup"><span data-stu-id="f7a91-230">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="f7a91-231">否</span><span class="sxs-lookup"><span data-stu-id="f7a91-231">No</span></span>    |

<span data-ttu-id="f7a91-232">運算式的 `^rewrite-rule/` 部分之後，有 `(\d+)/(\d+)` 這兩個擷取群組。</span><span class="sxs-lookup"><span data-stu-id="f7a91-232">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="f7a91-233">`\d` 表示「比對數字」。</span><span class="sxs-lookup"><span data-stu-id="f7a91-233">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="f7a91-234">加號 (`+`) 表示「比對一或多個前置字元」。</span><span class="sxs-lookup"><span data-stu-id="f7a91-234">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="f7a91-235">因此，URL 必須包含某個數字，後接斜線與另一個數字。</span><span class="sxs-lookup"><span data-stu-id="f7a91-235">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="f7a91-236">這些擷取群組會以 `$1` 和 `$2` 形式插入重寫的 URL。</span><span class="sxs-lookup"><span data-stu-id="f7a91-236">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="f7a91-237">重寫規則的取代字串會將擷取的群組放入查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="f7a91-237">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="f7a91-238">系統會重寫 `/rewrite-rule/1234/5678` 的要求路徑，以取得位於 `/rewritten?var1=1234&var2=5678` 的資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-238">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="f7a91-239">如果原始要求上有查詢字串，則會在重寫 URL 時予以保留。</span><span class="sxs-lookup"><span data-stu-id="f7a91-239">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="f7a91-240">這麼做不需往返伺服器，即可取得資源。</span><span class="sxs-lookup"><span data-stu-id="f7a91-240">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="f7a91-241">如果資源存在，就會擷取該資源，並傳回 200 (確定) 狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f7a91-241">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="f7a91-242">因為系統並未重新導向用戶端，因此瀏覽器網址列中的 URL 不會變更。</span><span class="sxs-lookup"><span data-stu-id="f7a91-242">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="f7a91-243">對用戶端而言，URL 重寫作業並未發生。</span><span class="sxs-lookup"><span data-stu-id="f7a91-243">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="f7a91-244">請盡可能使用 `skipRemainingRules: true`，因為比對規則是非常耗費資源的程序，且會降低應用程式的回應時間。</span><span class="sxs-lookup"><span data-stu-id="f7a91-244">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="f7a91-245">如需最快速的應用程式回應：</span><span class="sxs-lookup"><span data-stu-id="f7a91-245">For the fastest app response:</span></span>
> * <span data-ttu-id="f7a91-246">排序重寫規則；從最常比對的規則排到最不常比對的規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-246">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="f7a91-247">當出現符合項目且不需要任何額外的規則處理時，即略過剩下的規則處理。</span><span class="sxs-lookup"><span data-stu-id="f7a91-247">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="f7a91-248">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="f7a91-248">Apache mod_rewrite</span></span>

<span data-ttu-id="f7a91-249">使用 `AddApacheModRewrite` 來套用 Apache mod_rewrite 規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-249">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="f7a91-250">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="f7a91-250">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="f7a91-251">如需 mod_rewrite 規則的詳細資訊和範例，請參閱 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-251">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f7a91-253">系統會使用 `StreamReader` 來讀取 *ApacheModRewrite.txt* 規則檔案的規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-253">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f7a91-255">第一個參數會採用透過[相依性插入](dependency-injection.md)所提供的 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-255">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="f7a91-256">系統會插入 `IHostingEnvironment` 以提供 `ContentRootFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-256">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="f7a91-257">第二個參數是規則檔案的路徑，也就是範例應用程式中的 *ApacheModRewrite.txt*。</span><span class="sxs-lookup"><span data-stu-id="f7a91-257">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="f7a91-258">範例應用程式會將要求從 `/apache-mod-rules-redirect/(.\*)` 重新導向至 `/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-258">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="f7a91-259">回應狀態碼為 302 (已找到)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-259">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="f7a91-260">原始要求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="f7a91-260">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="f7a91-262">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f7a91-262">Supported server variables</span></span>

<span data-ttu-id="f7a91-263">中介軟體可支援下列 Apache mod_rewrite 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="f7a91-263">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="f7a91-264">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f7a91-264">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="f7a91-265">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="f7a91-265">HTTP_ACCEPT</span></span>
* <span data-ttu-id="f7a91-266">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="f7a91-266">HTTP_CONNECTION</span></span>
* <span data-ttu-id="f7a91-267">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="f7a91-267">HTTP_COOKIE</span></span>
* <span data-ttu-id="f7a91-268">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="f7a91-268">HTTP_FORWARDED</span></span>
* <span data-ttu-id="f7a91-269">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="f7a91-269">HTTP_HOST</span></span>
* <span data-ttu-id="f7a91-270">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="f7a91-270">HTTP_REFERER</span></span>
* <span data-ttu-id="f7a91-271">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="f7a91-271">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="f7a91-272">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f7a91-272">HTTPS</span></span>
* <span data-ttu-id="f7a91-273">IPV6</span><span class="sxs-lookup"><span data-stu-id="f7a91-273">IPV6</span></span>
* <span data-ttu-id="f7a91-274">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="f7a91-274">QUERY_STRING</span></span>
* <span data-ttu-id="f7a91-275">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f7a91-275">REMOTE_ADDR</span></span>
* <span data-ttu-id="f7a91-276">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="f7a91-276">REMOTE_PORT</span></span>
* <span data-ttu-id="f7a91-277">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f7a91-277">REQUEST_FILENAME</span></span>
* <span data-ttu-id="f7a91-278">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="f7a91-278">REQUEST_METHOD</span></span>
* <span data-ttu-id="f7a91-279">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="f7a91-279">REQUEST_SCHEME</span></span>
* <span data-ttu-id="f7a91-280">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="f7a91-280">REQUEST_URI</span></span>
* <span data-ttu-id="f7a91-281">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f7a91-281">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="f7a91-282">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="f7a91-282">SERVER_ADDR</span></span>
* <span data-ttu-id="f7a91-283">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="f7a91-283">SERVER_PORT</span></span>
* <span data-ttu-id="f7a91-284">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="f7a91-284">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="f7a91-285">TIME</span><span class="sxs-lookup"><span data-stu-id="f7a91-285">TIME</span></span>
* <span data-ttu-id="f7a91-286">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="f7a91-286">TIME_DAY</span></span>
* <span data-ttu-id="f7a91-287">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="f7a91-287">TIME_HOUR</span></span>
* <span data-ttu-id="f7a91-288">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="f7a91-288">TIME_MIN</span></span>
* <span data-ttu-id="f7a91-289">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="f7a91-289">TIME_MON</span></span>
* <span data-ttu-id="f7a91-290">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="f7a91-290">TIME_SEC</span></span>
* <span data-ttu-id="f7a91-291">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="f7a91-291">TIME_WDAY</span></span>
* <span data-ttu-id="f7a91-292">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="f7a91-292">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="f7a91-293">IIS URL Rewrite Module 規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-293">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="f7a91-294">若要使用適用於 IIS URL Rewrite Module 的規則，請使用 `AddIISUrlRewrite`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-294">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="f7a91-295">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="f7a91-295">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="f7a91-296">在 Windows Server IIS 上執行時，請不要引導中介軟體使用您的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f7a91-296">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="f7a91-297">使用 IIS 時，這些規則應該儲存在 *web.config* 外部，以免與 IIS Rewrite Module 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="f7a91-297">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="f7a91-298">如需 IIS URL Rewrite Module 規則的詳細資訊和範例，請參閱 [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0) 和 [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-298">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-299">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-299">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f7a91-300">系統會使用 `StreamReader` 來讀取 *IISUrlRewrite.xml* 規則檔案的規則。</span><span class="sxs-lookup"><span data-stu-id="f7a91-300">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f7a91-302">第一個參數會採用 `IFileProvider`，而第二個參數則是 XML 規則檔案的路徑，也就是範例應用程式中的 *IISUrlRewrite.xml*。</span><span class="sxs-lookup"><span data-stu-id="f7a91-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="f7a91-303">範例應用程式會將要求從 `/iis-rules-rewrite/(.*)` 重寫至 `/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="f7a91-304">系統會將回應與 200 (確定) 狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f7a91-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="f7a91-305">原始要求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="f7a91-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="f7a91-307">如果您具備使用中且已設定伺服器層級規則的 IIS Rewrite Module，但其會以非預期的方式影響應用程式，則您可以停用應用程式的 IIS Rewrite Module 。</span><span class="sxs-lookup"><span data-stu-id="f7a91-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="f7a91-308">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="f7a91-309">不支援的功能</span><span class="sxs-lookup"><span data-stu-id="f7a91-309">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7a91-311">與 ASP.NET Core 2.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="f7a91-311">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="f7a91-312">輸出規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-312">Outbound Rules</span></span>
* <span data-ttu-id="f7a91-313">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f7a91-313">Custom Server Variables</span></span>
* <span data-ttu-id="f7a91-314">萬用字元</span><span class="sxs-lookup"><span data-stu-id="f7a91-314">Wildcards</span></span>
* <span data-ttu-id="f7a91-315">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="f7a91-315">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f7a91-317">與 ASP.NET Core 1.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="f7a91-317">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="f7a91-318">全域規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-318">Global Rules</span></span>
* <span data-ttu-id="f7a91-319">輸出規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-319">Outbound Rules</span></span>
* <span data-ttu-id="f7a91-320">重寫對應</span><span class="sxs-lookup"><span data-stu-id="f7a91-320">Rewrite Maps</span></span>
* <span data-ttu-id="f7a91-321">CustomResponse 動作</span><span class="sxs-lookup"><span data-stu-id="f7a91-321">CustomResponse Action</span></span>
* <span data-ttu-id="f7a91-322">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f7a91-322">Custom Server Variables</span></span>
* <span data-ttu-id="f7a91-323">萬用字元</span><span class="sxs-lookup"><span data-stu-id="f7a91-323">Wildcards</span></span>
* <span data-ttu-id="f7a91-324">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="f7a91-324">Action:CustomResponse</span></span>
* <span data-ttu-id="f7a91-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="f7a91-325">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="f7a91-326">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f7a91-326">Supported server variables</span></span>

<span data-ttu-id="f7a91-327">中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="f7a91-327">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="f7a91-328">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="f7a91-328">CONTENT_LENGTH</span></span>
* <span data-ttu-id="f7a91-329">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="f7a91-329">CONTENT_TYPE</span></span>
* <span data-ttu-id="f7a91-330">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="f7a91-330">HTTP_ACCEPT</span></span>
* <span data-ttu-id="f7a91-331">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="f7a91-331">HTTP_CONNECTION</span></span>
* <span data-ttu-id="f7a91-332">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="f7a91-332">HTTP_COOKIE</span></span>
* <span data-ttu-id="f7a91-333">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="f7a91-333">HTTP_HOST</span></span>
* <span data-ttu-id="f7a91-334">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="f7a91-334">HTTP_REFERER</span></span>
* <span data-ttu-id="f7a91-335">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="f7a91-335">HTTP_URL</span></span>
* <span data-ttu-id="f7a91-336">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="f7a91-336">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="f7a91-337">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f7a91-337">HTTPS</span></span>
* <span data-ttu-id="f7a91-338">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="f7a91-338">LOCAL_ADDR</span></span>
* <span data-ttu-id="f7a91-339">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="f7a91-339">QUERY_STRING</span></span>
* <span data-ttu-id="f7a91-340">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f7a91-340">REMOTE_ADDR</span></span>
* <span data-ttu-id="f7a91-341">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="f7a91-341">REMOTE_PORT</span></span>
* <span data-ttu-id="f7a91-342">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f7a91-342">REQUEST_FILENAME</span></span>
* <span data-ttu-id="f7a91-343">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="f7a91-343">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="f7a91-344">您也可以透過 `PhysicalFileProvider` 取得 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-344">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="f7a91-345">這種方法可讓重寫規則檔案的位置更有彈性。</span><span class="sxs-lookup"><span data-stu-id="f7a91-345">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="f7a91-346">請確認重寫規則檔案已部署到您所提供之路徑的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f7a91-346">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="f7a91-347">以方法為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-347">Method-based rule</span></span>

<span data-ttu-id="f7a91-348">使用 `Add(Action<RewriteContext> applyRule)` 以在方法中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f7a91-348">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="f7a91-349">`RewriteContext` 會公開 `HttpContext` 以便用於您的方法。</span><span class="sxs-lookup"><span data-stu-id="f7a91-349">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="f7a91-350">`context.Result` 可判斷其他管線處理的執行方式。</span><span class="sxs-lookup"><span data-stu-id="f7a91-350">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="f7a91-351">context.Result</span><span class="sxs-lookup"><span data-stu-id="f7a91-351">context.Result</span></span>                       | <span data-ttu-id="f7a91-352">動作</span><span class="sxs-lookup"><span data-stu-id="f7a91-352">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="f7a91-353">`RuleResult.ContinueRules` (預設值)</span><span class="sxs-lookup"><span data-stu-id="f7a91-353">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="f7a91-354">繼續套用規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-354">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="f7a91-355">停止套用規則，並傳送回應</span><span class="sxs-lookup"><span data-stu-id="f7a91-355">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="f7a91-356">停止套用規則，並將內容傳送至下一個中介軟體</span><span class="sxs-lookup"><span data-stu-id="f7a91-356">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-357">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-357">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-358">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-358">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="f7a91-359">範例應用程式會示範一個方法，以重新導向對 *.xml* 結尾之路徑的要求。</span><span class="sxs-lookup"><span data-stu-id="f7a91-359">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="f7a91-360">如果您要求 `/file.xml`，該要求會重新導向至 `/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-360">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="f7a91-361">狀態碼會設定為 301 (已永久移動)。</span><span class="sxs-lookup"><span data-stu-id="f7a91-361">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="f7a91-362">若要重新導向，您必須明確設定回應的狀態碼；否則，會傳回 200 (確定) 狀態碼，且用戶端不會發生重新導向。</span><span class="sxs-lookup"><span data-stu-id="f7a91-362">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="f7a91-363">原始要求：`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="f7a91-363">Original Request: `/file.xml`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 file.xml 的要求和回應](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="f7a91-365">以 IRule 為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="f7a91-365">IRule-based rule</span></span>

<span data-ttu-id="f7a91-366">使用 `Add(IRule)`，以在衍生自 `IRule` 的類別中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f7a91-366">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="f7a91-367">相較於使用以方法為基礎的規則方法，使用 `IRule` 更有彈性。</span><span class="sxs-lookup"><span data-stu-id="f7a91-367">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="f7a91-368">您的衍生類別可包括建構函式，以在其中傳入 `ApplyRule` 方法的參數。</span><span class="sxs-lookup"><span data-stu-id="f7a91-368">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7a91-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7a91-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7a91-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="f7a91-371">為了滿足若干條件，系統會檢查範例應用程式中的 `extension` 和 `newPath` 參數值。</span><span class="sxs-lookup"><span data-stu-id="f7a91-371">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="f7a91-372">`extension` 必須包含值，而且值必須是 *.png*、*.jpg* 或 *.gif*。</span><span class="sxs-lookup"><span data-stu-id="f7a91-372">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="f7a91-373">如果 `newPath` 無效，就會擲回 `ArgumentException`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-373">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="f7a91-374">如果您要求 *image.png*，該要求會重新導向至 `/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-374">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="f7a91-375">如果您要求 *image.jpg*，該要求會重新導向至 `/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="f7a91-375">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="f7a91-376">狀態碼會設定為 301 (已永久移動)，而 `context.Result` 會設定為停止處理規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="f7a91-376">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="f7a91-377">原始要求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="f7a91-377">Original Request: `/image.png`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.png 的要求和回應](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="f7a91-379">原始要求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="f7a91-379">Original Request: `/image.jpg`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.jpg 的要求和回應](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="f7a91-381">Regex 範例</span><span class="sxs-lookup"><span data-stu-id="f7a91-381">Regex examples</span></span>

| <span data-ttu-id="f7a91-382">Goal</span><span class="sxs-lookup"><span data-stu-id="f7a91-382">Goal</span></span> | <span data-ttu-id="f7a91-383">Regex 字串及</span><span class="sxs-lookup"><span data-stu-id="f7a91-383">Regex String &</span></span><br><span data-ttu-id="f7a91-384">比對範例</span><span class="sxs-lookup"><span data-stu-id="f7a91-384">Match Example</span></span> | <span data-ttu-id="f7a91-385">取代字串及</span><span class="sxs-lookup"><span data-stu-id="f7a91-385">Replacement String &</span></span><br><span data-ttu-id="f7a91-386">輸出範例</span><span class="sxs-lookup"><span data-stu-id="f7a91-386">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="f7a91-387">將路徑重寫成查詢字串</span><span class="sxs-lookup"><span data-stu-id="f7a91-387">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="f7a91-388">移除斜線</span><span class="sxs-lookup"><span data-stu-id="f7a91-388">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="f7a91-389">強制使用斜線</span><span class="sxs-lookup"><span data-stu-id="f7a91-389">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="f7a91-390">避免重寫特定的要求</span><span class="sxs-lookup"><span data-stu-id="f7a91-390">Avoid rewriting specific requests</span></span> | <span data-ttu-id="f7a91-391">`^(.*)(?<!\.axd)$` 或 `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="f7a91-391">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="f7a91-392">是：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="f7a91-392">Yes: `/resource.htm`</span></span><br><span data-ttu-id="f7a91-393">否：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="f7a91-393">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="f7a91-394">重新排列 URL 區段</span><span class="sxs-lookup"><span data-stu-id="f7a91-394">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="f7a91-395">取代 URL 區段</span><span class="sxs-lookup"><span data-stu-id="f7a91-395">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="f7a91-396">其他資源</span><span class="sxs-lookup"><span data-stu-id="f7a91-396">Additional resources</span></span>

* [<span data-ttu-id="f7a91-397">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="f7a91-397">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="f7a91-398">中介軟體</span><span class="sxs-lookup"><span data-stu-id="f7a91-398">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="f7a91-399">.NET 中的規則運算式</span><span class="sxs-lookup"><span data-stu-id="f7a91-399">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="f7a91-400">規則運算式語言 - 快速參考</span><span class="sxs-lookup"><span data-stu-id="f7a91-400">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="f7a91-401">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="f7a91-401">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* <span data-ttu-id="f7a91-402">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0 (適用於 IIS))</span><span class="sxs-lookup"><span data-stu-id="f7a91-402">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="f7a91-403">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)</span><span class="sxs-lookup"><span data-stu-id="f7a91-403">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* [<span data-ttu-id="f7a91-404">IIS URL Rewrite Module 論壇</span><span class="sxs-lookup"><span data-stu-id="f7a91-404">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* <span data-ttu-id="f7a91-405">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (保持精簡的 URL 結構)</span><span class="sxs-lookup"><span data-stu-id="f7a91-405">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en)</span></span>
* <span data-ttu-id="f7a91-406">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 個重寫 URL 的祕訣與技巧)</span><span class="sxs-lookup"><span data-stu-id="f7a91-406">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="f7a91-407">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (是否要使用斜線)</span><span class="sxs-lookup"><span data-stu-id="f7a91-407">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
