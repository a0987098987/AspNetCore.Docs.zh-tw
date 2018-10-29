---
title: ASP.NET Core 的 URL 重寫中介軟體
author: guardrex
description: 了解如何使用 ASP.NET Core 的 URL 重寫中介軟體，進行 URL 重寫與重新導向作業。
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326065"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="777d7-103">ASP.NET Core 的 URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="777d7-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="777d7-104">由 [Luke Latham](https://github.com/guardrex) 和 [Mikael Mengistu](https://github.com/mikaelm12) 提供</span><span class="sxs-lookup"><span data-stu-id="777d7-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="777d7-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="777d7-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="777d7-106">URL 重寫是指根據一或多個預先定義的規則來修改要求 URL 的動作。</span><span class="sxs-lookup"><span data-stu-id="777d7-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="777d7-107">URL 重寫會在資源位置和位址之間建立一個抽象層，讓位置和位址不那麼緊密連結。</span><span class="sxs-lookup"><span data-stu-id="777d7-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="777d7-108">下列幾種情況非常適合使用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="777d7-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="777d7-109">暫時或永久性移動/取代伺服器資源時，同時為這些資源保持穩定的定位器。</span><span class="sxs-lookup"><span data-stu-id="777d7-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="777d7-110">跨不同應用程式或跨單一應用程式各區域來分割要求處理作業。</span><span class="sxs-lookup"><span data-stu-id="777d7-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="777d7-111">移除、新增或重組傳入要求的 URL 區段。</span><span class="sxs-lookup"><span data-stu-id="777d7-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="777d7-112">針對搜尋引擎最佳化 (SEO) 來最佳化公用 URL。</span><span class="sxs-lookup"><span data-stu-id="777d7-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="777d7-113">允許使用易記的公用 URL，以協助使用者預測他們在前往連結後能找到哪些內容。</span><span class="sxs-lookup"><span data-stu-id="777d7-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="777d7-114">將不安全的要求重新導向至安全的端點。</span><span class="sxs-lookup"><span data-stu-id="777d7-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="777d7-115">防止圖片盜連。</span><span class="sxs-lookup"><span data-stu-id="777d7-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="777d7-116">您可以透過數種方式來定義變更 URL 的規則，包括 Regex、Apache mod_rewrite 模組規則、IIS Rewrite Module 規則，以及使用自訂規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="777d7-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="777d7-117">本文件介紹 URL 重寫，並提供如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。</span><span class="sxs-lookup"><span data-stu-id="777d7-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="777d7-118">URL 重寫可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="777d7-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="777d7-119">如果可行的話，您應該限制規則的數目與複雜程度。</span><span class="sxs-lookup"><span data-stu-id="777d7-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="777d7-120">URL 重新導向和 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="777d7-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="777d7-121">乍看之下，「URL 重新導向」和「URL 重寫」在用語上的差異很微妙，但卻對提供資源給用戶端方面有重要的影響。</span><span class="sxs-lookup"><span data-stu-id="777d7-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="777d7-122">ASP.NET Core 的 URL 重寫中介軟體可以同時滿足這兩種需求。</span><span class="sxs-lookup"><span data-stu-id="777d7-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="777d7-123">「URL 重新導向」是指針對用戶端的作業，其會指示用戶端到另一個位址存取資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="777d7-124">這項作業需要往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="777d7-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="777d7-125">當用戶端提出新的資源要求時，系統會將重新導向 URL 傳回給用戶端，並顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="777d7-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="777d7-126">如果 `/resource` 是「重新導向」至 `/different-resource`，即表示用戶端要求的是 `/resource`。</span><span class="sxs-lookup"><span data-stu-id="777d7-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="777d7-127">伺服器會回應用戶端應前往 `/different-resource` 取得資源，並提供狀態碼，指出重新導向為暫時性或永久性。</span><span class="sxs-lookup"><span data-stu-id="777d7-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="777d7-128">用戶端即會在重新導向 URL 處執行新的資源要求。</span><span class="sxs-lookup"><span data-stu-id="777d7-128">The client executes a new request for the resource at the redirect URL.</span></span>

![伺服器上的 WebAPI 服務端點已暫時從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="777d7-134">當您將要求重新導向至不同的 URL 時，可以指出該重新導向為永久性或暫時性。</span><span class="sxs-lookup"><span data-stu-id="777d7-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="777d7-135">如果資源具有新的永久 URL，且您希望告知用戶端所有未來的資源要求皆應使用這個新 URL，請使用 301 (已永久移動) 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="777d7-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="777d7-136">*用戶端收到 301 狀態碼時，可能會快取回應。*</span><span class="sxs-lookup"><span data-stu-id="777d7-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="777d7-137">如果重新導向是暫時的且可能會變更，因此用戶端不應儲存亦不應於日後重複使用這個重新導向 URL，請使用 302 (已找到) 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="777d7-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="777d7-138">如需詳細資訊，請參閱 [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616：狀態碼定義)。</span><span class="sxs-lookup"><span data-stu-id="777d7-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="777d7-139">「URL 重寫」是針對伺服器端的作業，以從不同的資源位址提供資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="777d7-140">重寫 URL 時並不需要往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="777d7-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="777d7-141">系統不會將重寫的 URL 傳回給用戶端，也不會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="777d7-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="777d7-142">將 `/resource`「重寫」至 `/different-resource` 時，用戶端要求的是 `/resource`，且伺服器會於 `/different-resource`「內部」擷取資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="777d7-143">雖然用戶端或許可以在重寫的 URL 處擷取資源，但用戶端並不會在提出要求與接收回應時得知資源位於重寫的 URL 處。</span><span class="sxs-lookup"><span data-stu-id="777d7-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![伺服器上的 WebAPI 服務端點已從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="777d7-148">URL 重寫範例應用程式</span><span class="sxs-lookup"><span data-stu-id="777d7-148">URL rewriting sample app</span></span>

<span data-ttu-id="777d7-149">您可以使用 [URL 重寫範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/)，來探索 URL 重寫中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="777d7-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="777d7-150">應用程式會套用重新寫入和重新導向規則，並顯示重寫的 URL 或重新導向的 URL。</span><span class="sxs-lookup"><span data-stu-id="777d7-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="777d7-151">URL 重寫中介軟體的使用時機</span><span class="sxs-lookup"><span data-stu-id="777d7-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="777d7-152">當您無法使用 [IIS URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) (Windows Server)、[Apache mod_rewrite 模組](https://httpd.apache.org/docs/2.4/rewrite/) (Apache Server)、[Nginx上的 URL 重寫](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)，或者您的應用程式裝載於 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 之上時，請使用 URL 重寫中介軟體。</span><span class="sxs-lookup"><span data-stu-id="777d7-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="777d7-153">建議您使用 IIS、Apache 或 Nginx 中的伺服器端 URL 重寫技術，主要是因為中介軟體不支援這些模組的完整功能，而且中介軟體的效能可能與模組效能不符。</span><span class="sxs-lookup"><span data-stu-id="777d7-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="777d7-154">不過，有一些伺服器模組的功能不適用於 ASP.NET Core 專案，例如 IIS Rewrite Module 規則的 `IsFile` 和 `IsDirectory` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="777d7-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="777d7-155">在這些情況下，請改為使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="777d7-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="777d7-156">Package</span><span class="sxs-lookup"><span data-stu-id="777d7-156">Package</span></span>

<span data-ttu-id="777d7-157">若要在專案中包含中介軟體，請新增 [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) 套件的參考。</span><span class="sxs-lookup"><span data-stu-id="777d7-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="777d7-158">此功能適用於以 ASP.NET Core 1.1 或更新版本為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="777d7-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="777d7-159">延伸模組和選項</span><span class="sxs-lookup"><span data-stu-id="777d7-159">Extension and options</span></span>

<span data-ttu-id="777d7-160">若要建立 URL 重寫和重新導向規則，您可以針對每個規則的擴充方法建立 [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="777d7-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="777d7-161">依據所需的處理順序來鏈結多個規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="777d7-162">系統會使用 `app.UseRewriter(options);` 將 `RewriteOptions` 新增至要求管線，並將其傳入 URL 重寫中介軟體。</span><span class="sxs-lookup"><span data-stu-id="777d7-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="777d7-163">將非 www 重新導向 www</span><span class="sxs-lookup"><span data-stu-id="777d7-163">Redirect non-www to www</span></span>

<span data-ttu-id="777d7-164">有三個選項可讓應用程式將非 `www` 要求重新導向 `www`：</span><span class="sxs-lookup"><span data-stu-id="777d7-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="777d7-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; 會將要求永久重新導向 `www` 子網域 (如果要求為非 `www`)。</span><span class="sxs-lookup"><span data-stu-id="777d7-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="777d7-166">使用 [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="777d7-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="777d7-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; 會將要求重新導向 `www` 子網域 (如果連入的要求為非 `www`)。</span><span class="sxs-lookup"><span data-stu-id="777d7-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="777d7-168">使用 [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="777d7-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="777d7-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; 會將要求重新導向 `www` 子網域 (如果連入的要求為非 `www`)。</span><span class="sxs-lookup"><span data-stu-id="777d7-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="777d7-170">允許您提供回應的狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="777d7-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="777d7-171">使用 [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) 類別的欄位進行 `AddRedirectToWww` 指派。</span><span class="sxs-lookup"><span data-stu-id="777d7-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="777d7-172">URL 重新導向</span><span class="sxs-lookup"><span data-stu-id="777d7-172">URL redirect</span></span>

<span data-ttu-id="777d7-173">使用 `AddRedirect` 將要求重新導向。</span><span class="sxs-lookup"><span data-stu-id="777d7-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="777d7-174">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="777d7-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="777d7-175">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="777d7-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="777d7-176">第三個參數 (如果有的話) 會指定狀態碼。</span><span class="sxs-lookup"><span data-stu-id="777d7-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="777d7-177">如果您未指定狀態碼，則預設為 302 (已找到)，表示已暫時移動或取代資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="777d7-178">在啟用開發人員工具的瀏覽器中，使用 `/redirect-rule/1234/5678` 路徑提出範例應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="777d7-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="777d7-179">Regex 會比對 `redirect-rule/(.*)` 上的要求路徑，並會將路徑取代為 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="777d7-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="777d7-180">系統會將重新導向 URL 與 302 (已找到) 狀態碼傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="777d7-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="777d7-181">瀏覽器在重新導向 URL 處提出新要求，該 URL 也會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="777d7-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="777d7-182">由於範例應用程式中的任何規則都不符合重新導向 URL，因此第二個要求會收到應用程式的 200 (確定) 回應，且回應主體會顯示重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="777d7-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="777d7-183">「重新導向」URL 時，即需往返伺服器。</span><span class="sxs-lookup"><span data-stu-id="777d7-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="777d7-184">建立重新導向規則時，請務必謹慎。</span><span class="sxs-lookup"><span data-stu-id="777d7-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="777d7-185">每次向應用程式提出要求時 (包括重新導向後)，請評估您的重新導向規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="777d7-186">因為您有可能會不小心建立無限重新導向迴圈。</span><span class="sxs-lookup"><span data-stu-id="777d7-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="777d7-187">原始要求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="777d7-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="777d7-189">括弧內所含的運算式部分稱為「擷取群組」。</span><span class="sxs-lookup"><span data-stu-id="777d7-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="777d7-190">運算式的點 (`.`) 表示「比對任何字元」。</span><span class="sxs-lookup"><span data-stu-id="777d7-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="777d7-191">星號 (`*`) 表示「比對前置字元零或多次」。</span><span class="sxs-lookup"><span data-stu-id="777d7-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="777d7-192">因此，擷取群組 `(.*)` 會擷取 URL 的 `1234/5678` 最後這兩個路徑區段。</span><span class="sxs-lookup"><span data-stu-id="777d7-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="777d7-193">這個單一擷取群組會擷取您在要求 URL 中的 `redirect-rule/` 之後所提供的任何值。</span><span class="sxs-lookup"><span data-stu-id="777d7-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="777d7-194">系統會將擷取的群組以貨幣符號 (`$`) 後接擷取序號的形式，插入取代字串中。</span><span class="sxs-lookup"><span data-stu-id="777d7-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="777d7-195">使用 `$1` 可取得第一個擷取群組值、使用 `$2` 則會取得第二個，依您的 Regex 擷取群組順序以此類推。</span><span class="sxs-lookup"><span data-stu-id="777d7-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="777d7-196">在範例應用程式中，重新導向規則 Regex 只有一個擷取的群組，因此在取代字串中只有 `$1` 這一個插入的群組。</span><span class="sxs-lookup"><span data-stu-id="777d7-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="777d7-197">套用規則時，URL 會變成 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="777d7-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="777d7-198">將 URL 重新導向至安全端點</span><span class="sxs-lookup"><span data-stu-id="777d7-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="777d7-199">使用 `AddRedirectToHttps` 將 HTTP 要求重新導向至使用 HTTPS (`https://`) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="777d7-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="777d7-200">如果未提供狀態碼，中介軟體會預設為 302 (已找到)。</span><span class="sxs-lookup"><span data-stu-id="777d7-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="777d7-201">如果未提供連接埠，中介軟體會預設為 `null`；這表示通訊協定變更為 `https://` 且用戶端會存取連接埠 443 上的資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="777d7-202">此範例示範如何將狀態碼設為 301 (已永久移動)，並將連接埠變更為 5001。</span><span class="sxs-lookup"><span data-stu-id="777d7-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="777d7-203">使用 `AddRedirectToHttpsPermanent` 將不安全的要求重新導向至使用安全 HTTPS 通訊協定 (連接埠 443 上的 `https://`) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="777d7-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="777d7-204">中介軟體會將狀態碼設定為 301 (已永久移動)。</span><span class="sxs-lookup"><span data-stu-id="777d7-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="777d7-205">在不要求額外重新導向規則的情況下重新導向至 HTTPS 時，建議使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="777d7-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="777d7-206">如需詳細資訊，請參閱[強制執行 HTTPS](xref:security/enforcing-ssl#require-https) 主題。</span><span class="sxs-lookup"><span data-stu-id="777d7-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="777d7-207">範例應用程式可以示範如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="777d7-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="777d7-208">將擴充方法新增至 `RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="777d7-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="777d7-209">在任何 URL 位置，向應用程式提出不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="777d7-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="777d7-210">關閉瀏覽器自我簽署憑證不受信任的安全性警告，或建立信任憑證的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="777d7-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="777d7-211">使用 `AddRedirectToHttps(301, 5001)` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="777d7-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="777d7-213">使用 `AddRedirectToHttpsPermanent` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="777d7-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="777d7-215">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="777d7-215">URL rewrite</span></span>

<span data-ttu-id="777d7-216">使用 `AddRewrite` 來建立 URL 重寫的規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="777d7-217">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="777d7-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="777d7-218">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="777d7-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="777d7-219">第三個參數 `skipRemainingRules: {true|false}` 指出中介軟體是否要在套用目前規則時略過其他重寫規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="777d7-220">原始要求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="777d7-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="777d7-222">在 Regex 中，最引人注意的是運算式開頭的 `^`。</span><span class="sxs-lookup"><span data-stu-id="777d7-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="777d7-223">這表示，比對會從 URL 路徑的開頭開始。</span><span class="sxs-lookup"><span data-stu-id="777d7-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="777d7-224">在使用重新導向規則 `redirect-rule/(.*)` 的稍早範例中，Regex 的開頭沒有任何 ^；因此，成功比對的任何字元可能位於路徑中的 `redirect-rule/` 之前。</span><span class="sxs-lookup"><span data-stu-id="777d7-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="777d7-225">路徑</span><span class="sxs-lookup"><span data-stu-id="777d7-225">Path</span></span>                               | <span data-ttu-id="777d7-226">比對</span><span class="sxs-lookup"><span data-stu-id="777d7-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="777d7-227">[是]</span><span class="sxs-lookup"><span data-stu-id="777d7-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="777d7-228">[是]</span><span class="sxs-lookup"><span data-stu-id="777d7-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="777d7-229">[是]</span><span class="sxs-lookup"><span data-stu-id="777d7-229">Yes</span></span>   |

<span data-ttu-id="777d7-230">`^rewrite-rule/(\d+)/(\d+)` 重寫規則只會比對開頭為 `rewrite-rule/` 的路徑。</span><span class="sxs-lookup"><span data-stu-id="777d7-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="777d7-231">請注意，下列重寫規則與上述重新導向規則之間的比對差異。</span><span class="sxs-lookup"><span data-stu-id="777d7-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="777d7-232">路徑</span><span class="sxs-lookup"><span data-stu-id="777d7-232">Path</span></span>                              | <span data-ttu-id="777d7-233">比對</span><span class="sxs-lookup"><span data-stu-id="777d7-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="777d7-234">[是]</span><span class="sxs-lookup"><span data-stu-id="777d7-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="777d7-235">否</span><span class="sxs-lookup"><span data-stu-id="777d7-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="777d7-236">否</span><span class="sxs-lookup"><span data-stu-id="777d7-236">No</span></span>    |

<span data-ttu-id="777d7-237">運算式的 `^rewrite-rule/` 部分之後，有 `(\d+)/(\d+)` 這兩個擷取群組。</span><span class="sxs-lookup"><span data-stu-id="777d7-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="777d7-238">`\d` 表示「比對數字」。</span><span class="sxs-lookup"><span data-stu-id="777d7-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="777d7-239">加號 (`+`) 表示「比對一或多個前置字元」。</span><span class="sxs-lookup"><span data-stu-id="777d7-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="777d7-240">因此，URL 必須包含某個數字，後接斜線與另一個數字。</span><span class="sxs-lookup"><span data-stu-id="777d7-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="777d7-241">這些擷取群組會以 `$1` 和 `$2` 形式插入重寫的 URL。</span><span class="sxs-lookup"><span data-stu-id="777d7-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="777d7-242">重寫規則的取代字串會將擷取的群組放入查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="777d7-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="777d7-243">系統會重寫 `/rewrite-rule/1234/5678` 的要求路徑，以取得位於 `/rewritten?var1=1234&var2=5678` 的資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="777d7-244">如果原始要求上有查詢字串，則會在重寫 URL 時予以保留。</span><span class="sxs-lookup"><span data-stu-id="777d7-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="777d7-245">這麼做不需往返伺服器，即可取得資源。</span><span class="sxs-lookup"><span data-stu-id="777d7-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="777d7-246">如果資源存在，就會擷取該資源，並傳回 200 (確定) 狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="777d7-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="777d7-247">因為系統並未重新導向用戶端，因此瀏覽器網址列中的 URL 不會變更。</span><span class="sxs-lookup"><span data-stu-id="777d7-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="777d7-248">對用戶端而言，URL 重寫作業並未發生。</span><span class="sxs-lookup"><span data-stu-id="777d7-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="777d7-249">請盡可能使用 `skipRemainingRules: true`，因為比對規則是非常耗費資源的程序，且會降低應用程式的回應時間。</span><span class="sxs-lookup"><span data-stu-id="777d7-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="777d7-250">如需最快速的應用程式回應：</span><span class="sxs-lookup"><span data-stu-id="777d7-250">For the fastest app response:</span></span>
> * <span data-ttu-id="777d7-251">排序重寫規則；從最常比對的規則排到最不常比對的規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="777d7-252">當出現符合項目且不需要任何額外的規則處理時，即略過剩下的規則處理。</span><span class="sxs-lookup"><span data-stu-id="777d7-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="777d7-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="777d7-253">Apache mod_rewrite</span></span>

<span data-ttu-id="777d7-254">使用 `AddApacheModRewrite` 來套用 Apache mod_rewrite 規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="777d7-255">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="777d7-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="777d7-256">如需 mod_rewrite 規則的詳細資訊和範例，請參閱 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="777d7-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="777d7-257">系統會使用 `StreamReader` 來讀取 *ApacheModRewrite.txt* 規則檔案的規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="777d7-258">第一個參數會採用透過[相依性插入](dependency-injection.md)所提供的 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="777d7-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="777d7-259">系統會插入 `IHostingEnvironment` 以提供 `ContentRootFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="777d7-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="777d7-260">第二個參數是規則檔案的路徑，也就是範例應用程式中的 *ApacheModRewrite.txt*。</span><span class="sxs-lookup"><span data-stu-id="777d7-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="777d7-261">範例應用程式會將要求從 `/apache-mod-rules-redirect/(.\*)` 重新導向至 `/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="777d7-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="777d7-262">回應狀態碼為 302 (已找到)。</span><span class="sxs-lookup"><span data-stu-id="777d7-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="777d7-263">原始要求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="777d7-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="777d7-265">中介軟體可支援下列 Apache mod_rewrite 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="777d7-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="777d7-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="777d7-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="777d7-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="777d7-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="777d7-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="777d7-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="777d7-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="777d7-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="777d7-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="777d7-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="777d7-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="777d7-271">HTTP_HOST</span></span>
* <span data-ttu-id="777d7-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="777d7-272">HTTP_REFERER</span></span>
* <span data-ttu-id="777d7-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="777d7-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="777d7-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="777d7-274">HTTPS</span></span>
* <span data-ttu-id="777d7-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="777d7-275">IPV6</span></span>
* <span data-ttu-id="777d7-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="777d7-276">QUERY_STRING</span></span>
* <span data-ttu-id="777d7-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="777d7-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="777d7-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="777d7-278">REMOTE_PORT</span></span>
* <span data-ttu-id="777d7-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="777d7-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="777d7-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="777d7-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="777d7-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="777d7-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="777d7-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="777d7-282">REQUEST_URI</span></span>
* <span data-ttu-id="777d7-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="777d7-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="777d7-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="777d7-284">SERVER_ADDR</span></span>
* <span data-ttu-id="777d7-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="777d7-285">SERVER_PORT</span></span>
* <span data-ttu-id="777d7-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="777d7-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="777d7-287">TIME</span><span class="sxs-lookup"><span data-stu-id="777d7-287">TIME</span></span>
* <span data-ttu-id="777d7-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="777d7-288">TIME_DAY</span></span>
* <span data-ttu-id="777d7-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="777d7-289">TIME_HOUR</span></span>
* <span data-ttu-id="777d7-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="777d7-290">TIME_MIN</span></span>
* <span data-ttu-id="777d7-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="777d7-291">TIME_MON</span></span>
* <span data-ttu-id="777d7-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="777d7-292">TIME_SEC</span></span>
* <span data-ttu-id="777d7-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="777d7-293">TIME_WDAY</span></span>
* <span data-ttu-id="777d7-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="777d7-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="777d7-295">IIS URL Rewrite Module 規則</span><span class="sxs-lookup"><span data-stu-id="777d7-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="777d7-296">若要使用適用於 IIS URL Rewrite Module 的規則，請使用 `AddIISUrlRewrite`。</span><span class="sxs-lookup"><span data-stu-id="777d7-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="777d7-297">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="777d7-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="777d7-298">在 Windows Server IIS 上執行時，請不要引導中介軟體使用您的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="777d7-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="777d7-299">使用 IIS 時，這些規則應該儲存在 *web.config* 外部，以免與 IIS Rewrite Module 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="777d7-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="777d7-300">如需 IIS URL Rewrite Module 規則的詳細資訊和範例，請參閱 [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0) 和 [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)。</span><span class="sxs-lookup"><span data-stu-id="777d7-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="777d7-301">系統會使用 `StreamReader` 來讀取 *IISUrlRewrite.xml* 規則檔案的規則。</span><span class="sxs-lookup"><span data-stu-id="777d7-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="777d7-302">第一個參數會採用 `IFileProvider`，而第二個參數則是 XML 規則檔案的路徑，也就是範例應用程式中的 *IISUrlRewrite.xml*。</span><span class="sxs-lookup"><span data-stu-id="777d7-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="777d7-303">範例應用程式會將要求從 `/iis-rules-rewrite/(.*)` 重寫至 `/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="777d7-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="777d7-304">系統會將回應與 200 (確定) 狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="777d7-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="777d7-305">原始要求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="777d7-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="777d7-307">如果您具備使用中且已設定伺服器層級規則的 IIS Rewrite Module，但其會以非預期的方式影響應用程式，則您可以停用應用程式的 IIS Rewrite Module 。</span><span class="sxs-lookup"><span data-stu-id="777d7-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="777d7-308">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="777d7-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="777d7-309">不支援的功能</span><span class="sxs-lookup"><span data-stu-id="777d7-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="777d7-310">與 ASP.NET Core 2.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="777d7-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="777d7-311">輸出規則</span><span class="sxs-lookup"><span data-stu-id="777d7-311">Outbound Rules</span></span>
* <span data-ttu-id="777d7-312">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="777d7-312">Custom Server Variables</span></span>
* <span data-ttu-id="777d7-313">萬用字元</span><span class="sxs-lookup"><span data-stu-id="777d7-313">Wildcards</span></span>
* <span data-ttu-id="777d7-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="777d7-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="777d7-315">與 ASP.NET Core 1.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="777d7-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="777d7-316">全域規則</span><span class="sxs-lookup"><span data-stu-id="777d7-316">Global Rules</span></span>
* <span data-ttu-id="777d7-317">輸出規則</span><span class="sxs-lookup"><span data-stu-id="777d7-317">Outbound Rules</span></span>
* <span data-ttu-id="777d7-318">重寫對應</span><span class="sxs-lookup"><span data-stu-id="777d7-318">Rewrite Maps</span></span>
* <span data-ttu-id="777d7-319">CustomResponse 動作</span><span class="sxs-lookup"><span data-stu-id="777d7-319">CustomResponse Action</span></span>
* <span data-ttu-id="777d7-320">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="777d7-320">Custom Server Variables</span></span>
* <span data-ttu-id="777d7-321">萬用字元</span><span class="sxs-lookup"><span data-stu-id="777d7-321">Wildcards</span></span>
* <span data-ttu-id="777d7-322">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="777d7-322">Action:CustomResponse</span></span>
* <span data-ttu-id="777d7-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="777d7-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="777d7-324">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="777d7-324">Supported server variables</span></span>

<span data-ttu-id="777d7-325">中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="777d7-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="777d7-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="777d7-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="777d7-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="777d7-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="777d7-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="777d7-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="777d7-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="777d7-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="777d7-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="777d7-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="777d7-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="777d7-331">HTTP_HOST</span></span>
* <span data-ttu-id="777d7-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="777d7-332">HTTP_REFERER</span></span>
* <span data-ttu-id="777d7-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="777d7-333">HTTP_URL</span></span>
* <span data-ttu-id="777d7-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="777d7-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="777d7-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="777d7-335">HTTPS</span></span>
* <span data-ttu-id="777d7-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="777d7-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="777d7-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="777d7-337">QUERY_STRING</span></span>
* <span data-ttu-id="777d7-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="777d7-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="777d7-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="777d7-339">REMOTE_PORT</span></span>
* <span data-ttu-id="777d7-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="777d7-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="777d7-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="777d7-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="777d7-342">您也可以透過 `PhysicalFileProvider` 取得 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="777d7-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="777d7-343">這種方法可讓重寫規則檔案的位置更有彈性。</span><span class="sxs-lookup"><span data-stu-id="777d7-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="777d7-344">請確認重寫規則檔案已部署到您所提供之路徑的伺服器。</span><span class="sxs-lookup"><span data-stu-id="777d7-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="777d7-345">以方法為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="777d7-345">Method-based rule</span></span>

<span data-ttu-id="777d7-346">使用 `Add(Action<RewriteContext> applyRule)` 以在方法中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="777d7-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="777d7-347">`RewriteContext` 會公開 `HttpContext` 以便用於您的方法。</span><span class="sxs-lookup"><span data-stu-id="777d7-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="777d7-348">`context.Result` 可判斷其他管線處理的執行方式。</span><span class="sxs-lookup"><span data-stu-id="777d7-348">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="777d7-349">context.Result</span><span class="sxs-lookup"><span data-stu-id="777d7-349">context.Result</span></span>                       | <span data-ttu-id="777d7-350">動作</span><span class="sxs-lookup"><span data-stu-id="777d7-350">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="777d7-351">`RuleResult.ContinueRules` (預設值)</span><span class="sxs-lookup"><span data-stu-id="777d7-351">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="777d7-352">繼續套用規則</span><span class="sxs-lookup"><span data-stu-id="777d7-352">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="777d7-353">停止套用規則，並傳送回應</span><span class="sxs-lookup"><span data-stu-id="777d7-353">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="777d7-354">停止套用規則，並將內容傳送至下一個中介軟體</span><span class="sxs-lookup"><span data-stu-id="777d7-354">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="777d7-355">範例應用程式會示範一個方法，以重新導向對 *.xml* 結尾之路徑的要求。</span><span class="sxs-lookup"><span data-stu-id="777d7-355">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="777d7-356">如果您要求 `/file.xml`，該要求會重新導向至 `/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="777d7-356">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="777d7-357">狀態碼會設定為 301 (已永久移動)。</span><span class="sxs-lookup"><span data-stu-id="777d7-357">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="777d7-358">若要重新導向，您必須明確設定回應的狀態碼；否則，會傳回 200 (確定) 狀態碼，且用戶端不會發生重新導向。</span><span class="sxs-lookup"><span data-stu-id="777d7-358">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="777d7-359">原始要求：`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="777d7-359">Original Request: `/file.xml`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 file.xml 的要求和回應](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="777d7-361">以 IRule 為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="777d7-361">IRule-based rule</span></span>

<span data-ttu-id="777d7-362">使用 `Add(IRule)`，以在衍生自 `IRule` 的類別中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="777d7-362">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="777d7-363">相較於使用以方法為基礎的規則方法，使用 `IRule` 更有彈性。</span><span class="sxs-lookup"><span data-stu-id="777d7-363">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="777d7-364">您的衍生類別可包括建構函式，以在其中傳入 `ApplyRule` 方法的參數。</span><span class="sxs-lookup"><span data-stu-id="777d7-364">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="777d7-365">為了滿足若干條件，系統會檢查範例應用程式中的 `extension` 和 `newPath` 參數值。</span><span class="sxs-lookup"><span data-stu-id="777d7-365">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="777d7-366">`extension` 必須包含值，而且值必須是 *.png*、*.jpg* 或 *.gif*。</span><span class="sxs-lookup"><span data-stu-id="777d7-366">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="777d7-367">如果 `newPath` 無效，就會擲回 `ArgumentException`。</span><span class="sxs-lookup"><span data-stu-id="777d7-367">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="777d7-368">如果您要求 *image.png*，該要求會重新導向至 `/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="777d7-368">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="777d7-369">如果您要求 *image.jpg*，該要求會重新導向至 `/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="777d7-369">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="777d7-370">狀態碼會設定為 301 (已永久移動)，而 `context.Result` 會設定為停止處理規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="777d7-370">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="777d7-371">原始要求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="777d7-371">Original Request: `/image.png`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.png 的要求和回應](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="777d7-373">原始要求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="777d7-373">Original Request: `/image.jpg`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.jpg 的要求和回應](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="777d7-375">Regex 範例</span><span class="sxs-lookup"><span data-stu-id="777d7-375">Regex examples</span></span>

| <span data-ttu-id="777d7-376">Goal</span><span class="sxs-lookup"><span data-stu-id="777d7-376">Goal</span></span> | <span data-ttu-id="777d7-377">Regex 字串及</span><span class="sxs-lookup"><span data-stu-id="777d7-377">Regex String &</span></span><br><span data-ttu-id="777d7-378">比對範例</span><span class="sxs-lookup"><span data-stu-id="777d7-378">Match Example</span></span> | <span data-ttu-id="777d7-379">取代字串及</span><span class="sxs-lookup"><span data-stu-id="777d7-379">Replacement String &</span></span><br><span data-ttu-id="777d7-380">輸出範例</span><span class="sxs-lookup"><span data-stu-id="777d7-380">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="777d7-381">將路徑重寫成查詢字串</span><span class="sxs-lookup"><span data-stu-id="777d7-381">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="777d7-382">移除斜線</span><span class="sxs-lookup"><span data-stu-id="777d7-382">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="777d7-383">強制使用斜線</span><span class="sxs-lookup"><span data-stu-id="777d7-383">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="777d7-384">避免重寫特定的要求</span><span class="sxs-lookup"><span data-stu-id="777d7-384">Avoid rewriting specific requests</span></span> | <span data-ttu-id="777d7-385">`^(.*)(?<!\.axd)$` 或 `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="777d7-385">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="777d7-386">是：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="777d7-386">Yes: `/resource.htm`</span></span><br><span data-ttu-id="777d7-387">否：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="777d7-387">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="777d7-388">重新排列 URL 區段</span><span class="sxs-lookup"><span data-stu-id="777d7-388">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="777d7-389">取代 URL 區段</span><span class="sxs-lookup"><span data-stu-id="777d7-389">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="777d7-390">其他資源</span><span class="sxs-lookup"><span data-stu-id="777d7-390">Additional resources</span></span>

* [<span data-ttu-id="777d7-391">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="777d7-391">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="777d7-392">中介軟體</span><span class="sxs-lookup"><span data-stu-id="777d7-392">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="777d7-393">.NET 中的規則運算式</span><span class="sxs-lookup"><span data-stu-id="777d7-393">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="777d7-394">規則運算式語言 - 快速參考</span><span class="sxs-lookup"><span data-stu-id="777d7-394">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="777d7-395">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="777d7-395">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* <span data-ttu-id="777d7-396">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0 (適用於 IIS))</span><span class="sxs-lookup"><span data-stu-id="777d7-396">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="777d7-397">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)</span><span class="sxs-lookup"><span data-stu-id="777d7-397">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* [<span data-ttu-id="777d7-398">IIS URL Rewrite Module 論壇</span><span class="sxs-lookup"><span data-stu-id="777d7-398">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* <span data-ttu-id="777d7-399">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (保持精簡的 URL 結構)</span><span class="sxs-lookup"><span data-stu-id="777d7-399">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en)</span></span>
* <span data-ttu-id="777d7-400">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 個重寫 URL 的祕訣與技巧)</span><span class="sxs-lookup"><span data-stu-id="777d7-400">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="777d7-401">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (是否要使用斜線)</span><span class="sxs-lookup"><span data-stu-id="777d7-401">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
