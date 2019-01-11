---
title: ASP.NET Core 的 URL 重寫中介軟體
author: guardrex
description: 了解如何使用 ASP.NET Core 的 URL 重寫中介軟體，進行 URL 重寫與重新導向作業。
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/url-rewriting
ms.openlocfilehash: d2dd5e9b7f196bcbd1940f7ef58331dabd2367a1
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637803"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="71972-103">ASP.NET Core 的 URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="71972-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="71972-104">由 [Luke Latham](https://github.com/guardrex) 和 [Mikael Mengistu](https://github.com/mikaelm12) 提供</span><span class="sxs-lookup"><span data-stu-id="71972-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="71972-105">如需本主題的 1.1 版，請下載 [ASP.NET Core 中的 URL 重寫中介軟體 (1.1 版，PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf)。</span><span class="sxs-lookup"><span data-stu-id="71972-105">For the 1.1 version of this topic, download [URL Rewriting Middleware in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="71972-106">本文件介紹 URL 重寫，並提供如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。</span><span class="sxs-lookup"><span data-stu-id="71972-106">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

<span data-ttu-id="71972-107">URL 重寫是指根據一或多個預先定義的規則來修改要求 URL 的動作。</span><span class="sxs-lookup"><span data-stu-id="71972-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="71972-108">URL 重寫會在資源位置和位址之間建立一個抽象層，讓位置和位址不那麼緊密連結。</span><span class="sxs-lookup"><span data-stu-id="71972-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses aren't tightly linked.</span></span> <span data-ttu-id="71972-109">下列幾種情況非常適合使用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="71972-109">URL rewriting is valuable in several scenarios to:</span></span>

* <span data-ttu-id="71972-110">暫時或永久性移動/取代伺服器資源，並為這些資源保持穩定的定位器時。</span><span class="sxs-lookup"><span data-stu-id="71972-110">Move or replace server resources temporarily or permanently and maintain stable locators for those resources.</span></span>
* <span data-ttu-id="71972-111">跨不同應用程式或跨單一應用程式各區域來分割要求處理作業。</span><span class="sxs-lookup"><span data-stu-id="71972-111">Split request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="71972-112">移除、新增或重組傳入要求的 URL 區段。</span><span class="sxs-lookup"><span data-stu-id="71972-112">Remove, add, or reorganize URL segments on incoming requests.</span></span>
* <span data-ttu-id="71972-113">針對搜尋引擎最佳化 (SEO) 來將公開 URL 最佳化。</span><span class="sxs-lookup"><span data-stu-id="71972-113">Optimize public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="71972-114">允許使用易懂的公開 URL，來協助訪客預測要求資源所傳回的內容。</span><span class="sxs-lookup"><span data-stu-id="71972-114">Permit the use of friendly public URLs to help visitors predict the content returned by requesting a resource.</span></span>
* <span data-ttu-id="71972-115">將不安全的要求重新導向至安全的端點。</span><span class="sxs-lookup"><span data-stu-id="71972-115">Redirect insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="71972-116">防止直接連結，即外部網站透過將另一個網站的資產連結至本身的內容，來盜用該網站擁有的靜態資產。</span><span class="sxs-lookup"><span data-stu-id="71972-116">Prevent hotlinking, where an external site uses a hosted static asset on another site by linking the asset into its own content.</span></span>

> [!NOTE]
> <span data-ttu-id="71972-117">URL 重寫可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="71972-117">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="71972-118">如果可行的話，請限制規則的數目與複雜程度。</span><span class="sxs-lookup"><span data-stu-id="71972-118">Where feasible, limit the number and complexity of rules.</span></span>

<span data-ttu-id="71972-119">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71972-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="71972-120">URL 重新導向和 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="71972-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="71972-121">乍看之下，「URL 重新導向」和「URL 重寫」在用語上的差異不太明顯，但卻對提供資源給用戶端方面有重要的影響。</span><span class="sxs-lookup"><span data-stu-id="71972-121">The difference in wording between *URL redirect* and *URL rewrite* is subtle but has important implications for providing resources to clients.</span></span> <span data-ttu-id="71972-122">ASP.NET Core 的 URL 重寫中介軟體可以同時滿足這兩種需求。</span><span class="sxs-lookup"><span data-stu-id="71972-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="71972-123">「URL 重新導向」牽涉有關用戶端的作業，其會指示用戶端到其他位址存取資源，而非用戶端原本要求的位址。</span><span class="sxs-lookup"><span data-stu-id="71972-123">A *URL redirect* involves a client-side operation, where the client is instructed to access a resource at a different address than the client originally requested.</span></span> <span data-ttu-id="71972-124">這項作業需要伺服器的來回行程。</span><span class="sxs-lookup"><span data-stu-id="71972-124">This requires a round trip to the server.</span></span> <span data-ttu-id="71972-125">當用戶端提出新的資源要求時，系統會將重新導向 URL 傳回給用戶端，並顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="71972-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span>

<span data-ttu-id="71972-126">若 `/resource` 重新導向 至 `/different-resource`，則伺服器會回應用戶端應前往 `/different-resource` 取得資源，並提供狀態碼，指出重新導向為暫時性或永久性。</span><span class="sxs-lookup"><span data-stu-id="71972-126">If `/resource` is *redirected* to `/different-resource`, the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span>

![伺服器上的 WebAPI 服務端點已暫時從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="71972-132">將要求重新導向至不同的 URL 時，可以透過指定具狀態的回應，來指出該重新導向為永久性或暫時性：</span><span class="sxs-lookup"><span data-stu-id="71972-132">When redirecting requests to a different URL, indicate whether the redirect is permanent or temporary by specifying the status code with the response:</span></span>

* <span data-ttu-id="71972-133">如果資源具有新的永久 URL，且您希望告知用戶端所有未來的資源要求皆應使用這個新 URL，請使用 301 (已永久移動) 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="71972-133">The *301 - Moved Permanently* status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="71972-134">用戶端收到 301 狀態碼時，可能會快取並重複使用回應。</span><span class="sxs-lookup"><span data-stu-id="71972-134">*The client may cache and reuse the response when a 301 status code is received.*</span></span>

* <span data-ttu-id="71972-135">若重新導向為暫時性或很有可能變更時，請使用 302 (已找到) 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="71972-135">The *302 - Found* status code is used where the redirection is temporary or generally subject to change.</span></span> <span data-ttu-id="71972-136">302 狀態碼會指示用戶端不要儲存 URL 並在之後使用。</span><span class="sxs-lookup"><span data-stu-id="71972-136">The 302 status code indicates to the client not to store the URL and use it in the future.</span></span>

<span data-ttu-id="71972-137">如需狀態碼的詳細資訊，請參閱 [RFC 2616：Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (狀態碼定義)。</span><span class="sxs-lookup"><span data-stu-id="71972-137">For more information on status codes, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="71972-138">「URL 重寫」伺服器端作業，會從用戶端要求以外的其他資源位址提供資源。</span><span class="sxs-lookup"><span data-stu-id="71972-138">A *URL rewrite* is a server-side operation that provides a resource from a different resource address than the client requested.</span></span> <span data-ttu-id="71972-139">重寫 URL 並不需要伺服器的來回行程。</span><span class="sxs-lookup"><span data-stu-id="71972-139">Rewriting a URL doesn't require a round trip to the server.</span></span> <span data-ttu-id="71972-140">系統不會將重寫的 URL 傳回用戶端，也不會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="71972-140">The rewritten URL isn't returned to the client and doesn't appear in the browser's address bar.</span></span>

<span data-ttu-id="71972-141">若 `/resource`「重寫」至 `/different-resource`，則伺服器會於「內部」擷取資源，並在 `/different-resource` 傳回資源。</span><span class="sxs-lookup"><span data-stu-id="71972-141">If `/resource` is *rewritten* to `/different-resource`, the server *internally* fetches and returns the resource at `/different-resource`.</span></span>

<span data-ttu-id="71972-142">雖然用戶端或許可以在重寫的 URL 處擷取資源，但用戶端並不會在提出要求與接收回應時得知資源位於重寫的 URL 處。</span><span class="sxs-lookup"><span data-stu-id="71972-142">Although the client might be able to retrieve the resource at the rewritten URL, the client isn't informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![伺服器上的 WebAPI 服務端點已從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="71972-147">URL 重寫範例應用程式</span><span class="sxs-lookup"><span data-stu-id="71972-147">URL rewriting sample app</span></span>

<span data-ttu-id="71972-148">您可以利用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)，來探索 URL 重寫中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="71972-148">You can explore the features of the URL Rewriting Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="71972-149">範例應用程式會套用重新導向與重寫規則，還會示範多種情況下的重新導向或重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="71972-149">The app applies redirect and rewrite rules and shows the redirected or rewritten URL for several scenarios.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="71972-150">URL 重寫中介軟體的使用時機</span><span class="sxs-lookup"><span data-stu-id="71972-150">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="71972-151">當您無法使用以下方法時，請使用重寫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="71972-151">Use URL Rewriting Middleware when you're unable to use the following approaches:</span></span>

* [<span data-ttu-id="71972-152">Windows Server 上的 IIS URL Rewrite module</span><span class="sxs-lookup"><span data-stu-id="71972-152">URL Rewrite module with IIS on Windows Server</span></span>](https://www.iis.net/downloads/microsoft/url-rewrite)
* [<span data-ttu-id="71972-153">Apache Server 上的 Apache mod_rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="71972-153">Apache mod_rewrite module on Apache Server</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="71972-154">Nginx 上的 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="71972-154">URL rewriting on Nginx</span></span>](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<span data-ttu-id="71972-155">此外也請在應用程式裝載於 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (之前稱為 WebListener) 時使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71972-155">Also, use the middleware when the app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener).</span></span>

<span data-ttu-id="71972-156">使用 IIS、Apache 和 Nginx 所提供伺服器型 URL 重寫技術的主要原因包括：</span><span class="sxs-lookup"><span data-stu-id="71972-156">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, and Nginx are:</span></span>

* <span data-ttu-id="71972-157">中介軟體不支援這些模組的完整功能。</span><span class="sxs-lookup"><span data-stu-id="71972-157">The middleware doesn't support the full features of these modules.</span></span>

  <span data-ttu-id="71972-158">有部分伺服器模組的功能不適用於 ASP.NET Core 專案，例如 IIS Rewrite Module 規則的 `IsFile` 和 `IsDirectory` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="71972-158">Some of the features of the server modules don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="71972-159">在這些情況下，請改為使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71972-159">In these scenarios, use the middleware instead.</span></span>
* <span data-ttu-id="71972-160">中介軟體的效能可能比不上模組的效能時。</span><span class="sxs-lookup"><span data-stu-id="71972-160">The performance of the middleware probably doesn't match that of the modules.</span></span>

  <span data-ttu-id="71972-161">如果要確實得知哪種方法會降低最多效能，或是降低的效能可以忽略的話，進行效能評定是唯一方法。</span><span class="sxs-lookup"><span data-stu-id="71972-161">Benchmarking is the only way to know for sure which approach degrades performance the most or if degraded performance is negligible.</span></span>

## <a name="package"></a><span data-ttu-id="71972-162">Package</span><span class="sxs-lookup"><span data-stu-id="71972-162">Package</span></span>

<span data-ttu-id="71972-163">若要在您的專案中包含中介軟體，請在包含 [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) 套件的專案檔中，將套件參考新增至 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="71972-163">To include the middleware in your project, add a package reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the project file, which contains the [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) package.</span></span>

<span data-ttu-id="71972-164">不使用 `Microsoft.AspNetCore.App` 中繼套件時，請將專案參考新增至 `Microsoft.AspNetCore.Rewrite` 套件。</span><span class="sxs-lookup"><span data-stu-id="71972-164">When not using the `Microsoft.AspNetCore.App` metapackage, add a project reference to the `Microsoft.AspNetCore.Rewrite` package.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="71972-165">延伸模組和選項</span><span class="sxs-lookup"><span data-stu-id="71972-165">Extension and options</span></span>

<span data-ttu-id="71972-166">若要建立 URL 重寫和重新導向規則，您可以針對每個重寫規則的擴充方法建立 [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="71972-166">Establish URL rewrite and redirect rules by creating an instance of the [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) class with extension methods for each of your rewrite rules.</span></span> <span data-ttu-id="71972-167">依據所需的處理順序來鏈結多個規則。</span><span class="sxs-lookup"><span data-stu-id="71972-167">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="71972-168">系統會使用 <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*> 將 `RewriteOptions` 新增至要求管線，並將其傳入 URL 重寫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="71972-168">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="71972-169">將非 www 重新導向 www</span><span class="sxs-lookup"><span data-stu-id="71972-169">Redirect non-www to www</span></span>

<span data-ttu-id="71972-170">有三個選項可讓應用程式將非 `www` 要求重新導向 `www`：</span><span class="sxs-lookup"><span data-stu-id="71972-170">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="71972-171"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; 將要求永久重新導向 `www` 子網域 (如果要求為非 `www`)。</span><span class="sxs-lookup"><span data-stu-id="71972-171"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="71972-172">使用 [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="71972-172">Redirects with a [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) status code.</span></span>

* <span data-ttu-id="71972-173"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; 將要求重新導向 `www` 子網域 (如果要求為非 `www`)。</span><span class="sxs-lookup"><span data-stu-id="71972-173"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="71972-174">使用 [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="71972-174">Redirects with a [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) status code.</span></span> <span data-ttu-id="71972-175">多載可讓您提供回應的狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="71972-175">An overload permits you to provide the status code for the response.</span></span> <span data-ttu-id="71972-176">請使用 <xref:Microsoft.AspNetCore.Http.StatusCodes> 類別的欄位來進行狀態碼指派。</span><span class="sxs-lookup"><span data-stu-id="71972-176">Use a field of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for a status code assignment.</span></span>

### <a name="url-redirect"></a><span data-ttu-id="71972-177">URL 重新導向</span><span class="sxs-lookup"><span data-stu-id="71972-177">URL redirect</span></span>

<span data-ttu-id="71972-178">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> 將要求重新導向。</span><span class="sxs-lookup"><span data-stu-id="71972-178">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> to redirect requests.</span></span> <span data-ttu-id="71972-179">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="71972-179">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="71972-180">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="71972-180">The second parameter is the replacement string.</span></span> <span data-ttu-id="71972-181">第三個參數 (如果有的話) 會指定狀態碼。</span><span class="sxs-lookup"><span data-stu-id="71972-181">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="71972-182">如果您未指定狀態碼，則狀態碼會預設為 302 (已找到)，表示已暫時移動或取代資源。</span><span class="sxs-lookup"><span data-stu-id="71972-182">If you don't specify the status code, the status code defaults to *302 - Found*, which indicates that the resource is temporarily moved or replaced.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="71972-183">在啟用開發人員工具的瀏覽器中，使用 `/redirect-rule/1234/5678` 路徑提出範例應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="71972-183">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="71972-184">Regex 會比對 `redirect-rule/(.*)` 上的要求路徑，並會將路徑取代為 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="71972-184">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="71972-185">系統會將重新導向 URL 與 302 (已找到) 狀態碼傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="71972-185">The redirect URL is sent back to the client with a *302 - Found* status code.</span></span> <span data-ttu-id="71972-186">瀏覽器在重新導向 URL 處提出新要求，該 URL 也會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="71972-186">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="71972-187">因為範例應用程式與重新導向 URL 沒有任何相符規則：</span><span class="sxs-lookup"><span data-stu-id="71972-187">Since no rules in the sample app match on the redirect URL:</span></span>

* <span data-ttu-id="71972-188">第二個要求會從應用程式收到 200 (確定) 回應。</span><span class="sxs-lookup"><span data-stu-id="71972-188">The second request receives a *200 - OK* response from the app.</span></span>
* <span data-ttu-id="71972-189">回應的本文會顯示重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="71972-189">The body of the response shows the redirect URL.</span></span>

<span data-ttu-id="71972-190">「重新導向」URL 時，會對伺服器進行來回行程。</span><span class="sxs-lookup"><span data-stu-id="71972-190">A round trip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="71972-191">建立重新導向規則時，請務必謹慎。</span><span class="sxs-lookup"><span data-stu-id="71972-191">Be cautious when establishing redirect rules.</span></span> <span data-ttu-id="71972-192">每向應用程式提出要求時 (包括重新導向後)，請評估您的重新導向規則。</span><span class="sxs-lookup"><span data-stu-id="71972-192">Redirect rules are evaluated on every request to the app, including after a redirect.</span></span> <span data-ttu-id="71972-193">因為您有可能會不小心建立無限重新導向迴圈。</span><span class="sxs-lookup"><span data-stu-id="71972-193">It's easy to accidentally create a *loop of infinite redirects*.</span></span>

<span data-ttu-id="71972-194">原始要求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="71972-194">Original Request: `/redirect-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="71972-196">括弧內所含的運算式部分稱為「擷取群組」。</span><span class="sxs-lookup"><span data-stu-id="71972-196">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="71972-197">運算式的點 (`.`) 表示「比對任何字元」。</span><span class="sxs-lookup"><span data-stu-id="71972-197">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="71972-198">星號 (`*`) 表示「比對前置字元零或多次」。</span><span class="sxs-lookup"><span data-stu-id="71972-198">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="71972-199">因此，擷取群組 `(.*)` 會擷取 URL 的 `1234/5678` 最後這兩個路徑區段。</span><span class="sxs-lookup"><span data-stu-id="71972-199">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="71972-200">這個單一擷取群組會擷取您在要求 URL 中的 `redirect-rule/` 之後所提供的任何值。</span><span class="sxs-lookup"><span data-stu-id="71972-200">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="71972-201">系統會將擷取的群組以貨幣符號 (`$`) 後接擷取序號的形式，插入取代字串中。</span><span class="sxs-lookup"><span data-stu-id="71972-201">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="71972-202">使用 `$1` 可取得第一個擷取群組值、使用 `$2` 則會取得第二個，依您的 Regex 擷取群組順序以此類推。</span><span class="sxs-lookup"><span data-stu-id="71972-202">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="71972-203">在範例應用程式中，重新導向規則 Regex 只有一個擷取的群組，因此在取代字串中只有 `$1` 這一個插入的群組。</span><span class="sxs-lookup"><span data-stu-id="71972-203">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="71972-204">套用規則時，URL 會變成 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="71972-204">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="71972-205">將 URL 重新導向至安全端點</span><span class="sxs-lookup"><span data-stu-id="71972-205">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="71972-206">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> 將 HTTP 要求重新導向至使用 HTTPS 通訊協定的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="71972-206">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> to redirect HTTP requests to the same host and path using the HTTPS protocol.</span></span> <span data-ttu-id="71972-207">如果未提供狀態碼，中介軟體會預設為 302 (已找到)。</span><span class="sxs-lookup"><span data-stu-id="71972-207">If the status code isn't supplied, the middleware defaults to *302 - Found*.</span></span> <span data-ttu-id="71972-208">若未提供連接埠：</span><span class="sxs-lookup"><span data-stu-id="71972-208">If the port isn't supplied:</span></span>

* <span data-ttu-id="71972-209">中介軟體會預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="71972-209">The middleware defaults to `null`.</span></span>
* <span data-ttu-id="71972-210">配置會變更為 `https` (HTTPS 通訊協定)，且用戶端會在連接埠 443 上存取資源。</span><span class="sxs-lookup"><span data-stu-id="71972-210">The scheme changes to `https` (HTTPS protocol), and the client accesses the resource on port 443.</span></span>

<span data-ttu-id="71972-211">下列範例示範了如何將狀態碼設為 301 (已永久移動)，並將連接埠變更為 5001。</span><span class="sxs-lookup"><span data-stu-id="71972-211">The following example shows how to set the status code to *301 - Moved Permanently* and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="71972-212">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> 將不安全的要求重新導向至使用安全 HTTPS 通訊協定 (在連接埠 443 上) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="71972-212">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> to redirect insecure requests to the same host and path with secure HTTPS protocol on port 443.</span></span> <span data-ttu-id="71972-213">中介軟體會將狀態碼設定為 301 (已永久移動)。</span><span class="sxs-lookup"><span data-stu-id="71972-213">The middleware sets the status code to *301 - Moved Permanently*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="71972-214">在不要求額外重新導向規則的情況下重新導向至安全的端點時，建議使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71972-214">When redirecting to a secure endpoint without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="71972-215">如需詳細資訊，請參閱[強制執行 HTTPS](xref:security/enforcing-ssl#require-https) 主題。</span><span class="sxs-lookup"><span data-stu-id="71972-215">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="71972-216">範例應用程式可以示範如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="71972-216">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="71972-217">將擴充方法新增至 `RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="71972-217">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="71972-218">在任何 URL 位置，向應用程式提出不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="71972-218">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="71972-219">關閉瀏覽器自我簽署憑證不受信任的安全性警告，或建立信任憑證的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="71972-219">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="71972-220">使用 `AddRedirectToHttps(301, 5001)` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="71972-220">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="71972-222">使用 `AddRedirectToHttpsPermanent` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="71972-222">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="71972-224">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="71972-224">URL rewrite</span></span>

<span data-ttu-id="71972-225">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> 來建立 URL 重寫的規則。</span><span class="sxs-lookup"><span data-stu-id="71972-225">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> to create a rule for rewriting URLs.</span></span> <span data-ttu-id="71972-226">第一個參數會包含 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="71972-226">The first parameter contains the regex for matching on the incoming URL path.</span></span> <span data-ttu-id="71972-227">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="71972-227">The second parameter is the replacement string.</span></span> <span data-ttu-id="71972-228">第三個參數 `skipRemainingRules: {true|false}` 指出中介軟體是否要在套用目前規則時略過其他重寫規則。</span><span class="sxs-lookup"><span data-stu-id="71972-228">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

<span data-ttu-id="71972-229">原始要求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="71972-229">Original Request: `/rewrite-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="71972-231">運算式開頭的插入號 (`^`) 表示，比對會從 URL 路徑的開頭開始。</span><span class="sxs-lookup"><span data-stu-id="71972-231">The carat (`^`) at the beginning of the expression means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="71972-232">在先前的重新導向規則範例 `redirect-rule/(.*)` 中，Regex 的開頭沒有插入號 (`^`)。</span><span class="sxs-lookup"><span data-stu-id="71972-232">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat (`^`) at the start of the regex.</span></span> <span data-ttu-id="71972-233">因此，就算 `redirect-rule/` 前有任何字元也能成功比對。</span><span class="sxs-lookup"><span data-stu-id="71972-233">Therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="71972-234">路徑</span><span class="sxs-lookup"><span data-stu-id="71972-234">Path</span></span>                               | <span data-ttu-id="71972-235">比對</span><span class="sxs-lookup"><span data-stu-id="71972-235">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="71972-236">是</span><span class="sxs-lookup"><span data-stu-id="71972-236">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="71972-237">[是]</span><span class="sxs-lookup"><span data-stu-id="71972-237">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="71972-238">是</span><span class="sxs-lookup"><span data-stu-id="71972-238">Yes</span></span>   |

<span data-ttu-id="71972-239">`^rewrite-rule/(\d+)/(\d+)` 重寫規則只會比對開頭為 `rewrite-rule/` 的路徑。</span><span class="sxs-lookup"><span data-stu-id="71972-239">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="71972-240">請注意下表中的比對差異。</span><span class="sxs-lookup"><span data-stu-id="71972-240">In the following table, note the difference in matching.</span></span>

| <span data-ttu-id="71972-241">路徑</span><span class="sxs-lookup"><span data-stu-id="71972-241">Path</span></span>                              | <span data-ttu-id="71972-242">比對</span><span class="sxs-lookup"><span data-stu-id="71972-242">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="71972-243">是</span><span class="sxs-lookup"><span data-stu-id="71972-243">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="71972-244">否</span><span class="sxs-lookup"><span data-stu-id="71972-244">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="71972-245">否</span><span class="sxs-lookup"><span data-stu-id="71972-245">No</span></span>    |

<span data-ttu-id="71972-246">運算式的 `^rewrite-rule/` 部分之後，有 `(\d+)/(\d+)` 這兩個擷取群組。</span><span class="sxs-lookup"><span data-stu-id="71972-246">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="71972-247">`\d` 表示「比對數字」。</span><span class="sxs-lookup"><span data-stu-id="71972-247">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="71972-248">加號 (`+`) 表示「比對一或多個前置字元」。</span><span class="sxs-lookup"><span data-stu-id="71972-248">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="71972-249">因此，URL 必須包含某個數字，後接斜線與另一個數字。</span><span class="sxs-lookup"><span data-stu-id="71972-249">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="71972-250">這些擷取群組會以 `$1` 和 `$2` 形式插入重寫的 URL。</span><span class="sxs-lookup"><span data-stu-id="71972-250">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="71972-251">重寫規則的取代字串會將擷取的群組放入查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="71972-251">The rewrite rule replacement string places the captured groups into the query string.</span></span> <span data-ttu-id="71972-252">系統會重寫 `/rewrite-rule/1234/5678` 的要求路徑，以取得位於 `/rewritten?var1=1234&var2=5678` 的資源。</span><span class="sxs-lookup"><span data-stu-id="71972-252">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="71972-253">如果原始要求上有查詢字串，則會在重寫 URL 時予以保留。</span><span class="sxs-lookup"><span data-stu-id="71972-253">If a query string is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="71972-254">這麼做就不需要伺服器的來回行程，即可取得資源。</span><span class="sxs-lookup"><span data-stu-id="71972-254">There's no round trip to the server to obtain the resource.</span></span> <span data-ttu-id="71972-255">如果資源存在，就會擷取該資源，並傳回 200 (確定) 狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="71972-255">If the resource exists, it's fetched and returned to the client with a *200 - OK* status code.</span></span> <span data-ttu-id="71972-256">因為系統並未重新導向用戶端，因此瀏覽器網址列中的 URL 不會變更。</span><span class="sxs-lookup"><span data-stu-id="71972-256">Because the client isn't redirected, the URL in the browser's address bar doesn't change.</span></span> <span data-ttu-id="71972-257">用戶端也無法偵測到伺服器上發生 URL 重寫作業。</span><span class="sxs-lookup"><span data-stu-id="71972-257">Clients can't detect that a URL rewrite operation occurred on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="71972-258">因為比對規則是計算繁複的程序，且會增加應用程式的回應時間，所以請盡可能使用 `skipRemainingRules: true`。</span><span class="sxs-lookup"><span data-stu-id="71972-258">Use `skipRemainingRules: true` whenever possible because matching rules is computationally expensive and increases app response time.</span></span> <span data-ttu-id="71972-259">如需最快速的應用程式回應：</span><span class="sxs-lookup"><span data-stu-id="71972-259">For the fastest app response:</span></span>
>
> * <span data-ttu-id="71972-260">排序重寫規則；從最常比對的規則排到最不常比對的規則。</span><span class="sxs-lookup"><span data-stu-id="71972-260">Order rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="71972-261">當出現符合項目且不需要任何額外的規則處理時，即略過剩下的規則處理。</span><span class="sxs-lookup"><span data-stu-id="71972-261">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="71972-262">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="71972-262">Apache mod_rewrite</span></span>

<span data-ttu-id="71972-263">使用 <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*> 來套用 Apache mod_rewrite 規則。</span><span class="sxs-lookup"><span data-stu-id="71972-263">Apply Apache mod_rewrite rules with <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span></span> <span data-ttu-id="71972-264">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="71972-264">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="71972-265">如需 mod_rewrite 規則的詳細資訊和範例，請參閱 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="71972-265">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

<span data-ttu-id="71972-266">系統會使用 <xref:System.IO.StreamReader> 來讀取 *ApacheModRewrite.txt* 規則檔案的規則：</span><span class="sxs-lookup"><span data-stu-id="71972-266">A <xref:System.IO.StreamReader> is used to read the rules from the *ApacheModRewrite.txt* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

<span data-ttu-id="71972-267">範例應用程式會將要求從 `/apache-mod-rules-redirect/(.\*)` 重新導向至 `/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="71972-267">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="71972-268">回應狀態碼為 302 (已找到)。</span><span class="sxs-lookup"><span data-stu-id="71972-268">The response status code is *302 - Found*.</span></span>

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

<span data-ttu-id="71972-269">原始要求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="71972-269">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="71972-271">中介軟體可支援下列 Apache mod_rewrite 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="71972-271">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="71972-272">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="71972-272">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="71972-273">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="71972-273">HTTP_ACCEPT</span></span>
* <span data-ttu-id="71972-274">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="71972-274">HTTP_CONNECTION</span></span>
* <span data-ttu-id="71972-275">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="71972-275">HTTP_COOKIE</span></span>
* <span data-ttu-id="71972-276">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="71972-276">HTTP_FORWARDED</span></span>
* <span data-ttu-id="71972-277">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="71972-277">HTTP_HOST</span></span>
* <span data-ttu-id="71972-278">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="71972-278">HTTP_REFERER</span></span>
* <span data-ttu-id="71972-279">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="71972-279">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="71972-280">HTTPS</span><span class="sxs-lookup"><span data-stu-id="71972-280">HTTPS</span></span>
* <span data-ttu-id="71972-281">IPV6</span><span class="sxs-lookup"><span data-stu-id="71972-281">IPV6</span></span>
* <span data-ttu-id="71972-282">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="71972-282">QUERY_STRING</span></span>
* <span data-ttu-id="71972-283">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="71972-283">REMOTE_ADDR</span></span>
* <span data-ttu-id="71972-284">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="71972-284">REMOTE_PORT</span></span>
* <span data-ttu-id="71972-285">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="71972-285">REQUEST_FILENAME</span></span>
* <span data-ttu-id="71972-286">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="71972-286">REQUEST_METHOD</span></span>
* <span data-ttu-id="71972-287">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="71972-287">REQUEST_SCHEME</span></span>
* <span data-ttu-id="71972-288">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="71972-288">REQUEST_URI</span></span>
* <span data-ttu-id="71972-289">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="71972-289">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="71972-290">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="71972-290">SERVER_ADDR</span></span>
* <span data-ttu-id="71972-291">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="71972-291">SERVER_PORT</span></span>
* <span data-ttu-id="71972-292">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="71972-292">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="71972-293">TIME</span><span class="sxs-lookup"><span data-stu-id="71972-293">TIME</span></span>
* <span data-ttu-id="71972-294">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="71972-294">TIME_DAY</span></span>
* <span data-ttu-id="71972-295">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="71972-295">TIME_HOUR</span></span>
* <span data-ttu-id="71972-296">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="71972-296">TIME_MIN</span></span>
* <span data-ttu-id="71972-297">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="71972-297">TIME_MON</span></span>
* <span data-ttu-id="71972-298">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="71972-298">TIME_SEC</span></span>
* <span data-ttu-id="71972-299">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="71972-299">TIME_WDAY</span></span>
* <span data-ttu-id="71972-300">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="71972-300">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="71972-301">IIS URL Rewrite Module 規則</span><span class="sxs-lookup"><span data-stu-id="71972-301">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="71972-302">若要使用與套用至 IIS URL Rewrite Module 一樣的規則集，請使用 <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>。</span><span class="sxs-lookup"><span data-stu-id="71972-302">To use the same rule set that applies to the IIS URL Rewrite Module, use <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span></span> <span data-ttu-id="71972-303">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="71972-303">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="71972-304">在 Windows Server IIS 上執行時，請不要引導中介軟體使用應用程式的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71972-304">Don't direct the middleware to use the app's *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="71972-305">使用 IIS 時，這些規則應該儲存在應用程式的 *web.config* 檔案外部，以免與 IIS Rewrite Module 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="71972-305">With IIS, these rules should be stored outside of the app's *web.config* file in order to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="71972-306">如需 IIS URL Rewrite Module 規則的詳細資訊和範例，請參閱 [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0) 和 [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)。</span><span class="sxs-lookup"><span data-stu-id="71972-306">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

<span data-ttu-id="71972-307">系統會使用 <xref:System.IO.StreamReader> 來讀取 *IISUrlRewrite.xml* 規則檔案的規則：</span><span class="sxs-lookup"><span data-stu-id="71972-307">A <xref:System.IO.StreamReader> is used to read the rules from the *IISUrlRewrite.xml* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

<span data-ttu-id="71972-308">範例應用程式會將要求從 `/iis-rules-rewrite/(.*)` 重寫至 `/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="71972-308">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="71972-309">系統會將回應與 200 (確定) 狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="71972-309">The response is sent to the client with a *200 - OK* status code.</span></span>

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

<span data-ttu-id="71972-310">原始要求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="71972-310">Original Request: `/iis-rules-rewrite/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="71972-312">如果您具備使用中且已設定伺服器層級規則的 IIS Rewrite Module，但其會以非預期的方式影響應用程式，則您可以停用應用程式的 IIS Rewrite Module 。</span><span class="sxs-lookup"><span data-stu-id="71972-312">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="71972-313">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="71972-313">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="71972-314">不支援的功能</span><span class="sxs-lookup"><span data-stu-id="71972-314">Unsupported features</span></span>

<span data-ttu-id="71972-315">與 ASP.NET Core 2.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="71972-315">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="71972-316">輸出規則</span><span class="sxs-lookup"><span data-stu-id="71972-316">Outbound Rules</span></span>
* <span data-ttu-id="71972-317">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="71972-317">Custom Server Variables</span></span>
* <span data-ttu-id="71972-318">萬用字元</span><span class="sxs-lookup"><span data-stu-id="71972-318">Wildcards</span></span>
* <span data-ttu-id="71972-319">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="71972-319">LogRewrittenUrl</span></span>

#### <a name="supported-server-variables"></a><span data-ttu-id="71972-320">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="71972-320">Supported server variables</span></span>

<span data-ttu-id="71972-321">中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="71972-321">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="71972-322">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="71972-322">CONTENT_LENGTH</span></span>
* <span data-ttu-id="71972-323">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="71972-323">CONTENT_TYPE</span></span>
* <span data-ttu-id="71972-324">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="71972-324">HTTP_ACCEPT</span></span>
* <span data-ttu-id="71972-325">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="71972-325">HTTP_CONNECTION</span></span>
* <span data-ttu-id="71972-326">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="71972-326">HTTP_COOKIE</span></span>
* <span data-ttu-id="71972-327">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="71972-327">HTTP_HOST</span></span>
* <span data-ttu-id="71972-328">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="71972-328">HTTP_REFERER</span></span>
* <span data-ttu-id="71972-329">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="71972-329">HTTP_URL</span></span>
* <span data-ttu-id="71972-330">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="71972-330">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="71972-331">HTTPS</span><span class="sxs-lookup"><span data-stu-id="71972-331">HTTPS</span></span>
* <span data-ttu-id="71972-332">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="71972-332">LOCAL_ADDR</span></span>
* <span data-ttu-id="71972-333">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="71972-333">QUERY_STRING</span></span>
* <span data-ttu-id="71972-334">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="71972-334">REMOTE_ADDR</span></span>
* <span data-ttu-id="71972-335">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="71972-335">REMOTE_PORT</span></span>
* <span data-ttu-id="71972-336">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="71972-336">REQUEST_FILENAME</span></span>
* <span data-ttu-id="71972-337">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="71972-337">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="71972-338">您也可以透過 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 取得 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="71972-338">You can also obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> via a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span></span> <span data-ttu-id="71972-339">這種方法可讓重寫規則檔案的位置更有彈性。</span><span class="sxs-lookup"><span data-stu-id="71972-339">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="71972-340">請確認重寫規則檔案已部署到您所提供之路徑的伺服器。</span><span class="sxs-lookup"><span data-stu-id="71972-340">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="71972-341">以方法為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="71972-341">Method-based rule</span></span>

<span data-ttu-id="71972-342">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 以在方法中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="71972-342">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to implement your own rule logic in a method.</span></span> <span data-ttu-id="71972-343">`Add` 會公開 <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>，使 <xref:Microsoft.AspNetCore.Http.HttpContext> 可用於您的方法中。</span><span class="sxs-lookup"><span data-stu-id="71972-343">`Add` exposes the <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, which makes available the <xref:Microsoft.AspNetCore.Http.HttpContext> for use in your method.</span></span> <span data-ttu-id="71972-344">[RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*)可判斷其他管線處理的執行方式。</span><span class="sxs-lookup"><span data-stu-id="71972-344">The [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) determines how additional pipeline processing is handled.</span></span> <span data-ttu-id="71972-345">請將值設定為下表中描述的其中一個 <xref:Microsoft.AspNetCore.Rewrite.RuleResult> 欄位。</span><span class="sxs-lookup"><span data-stu-id="71972-345">Set the value to one of the <xref:Microsoft.AspNetCore.Rewrite.RuleResult> fields described in the following table.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="71972-346">動作</span><span class="sxs-lookup"><span data-stu-id="71972-346">Action</span></span>                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| <span data-ttu-id="71972-347">`RuleResult.ContinueRules` (預設值)</span><span class="sxs-lookup"><span data-stu-id="71972-347">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="71972-348">繼續套用規則。</span><span class="sxs-lookup"><span data-stu-id="71972-348">Continue applying rules.</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="71972-349">停止套用規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="71972-349">Stop applying rules and send the response.</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="71972-350">停止套用規則，並將內容傳送至下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="71972-350">Stop applying rules and send the context to the next middleware.</span></span> |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

<span data-ttu-id="71972-351">範例應用程式會示範一個方法，以重新導向對 *.xml* 結尾之路徑的要求。</span><span class="sxs-lookup"><span data-stu-id="71972-351">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="71972-352">若要求是針對 `/file.xml` 發出，則要求會重新導向至 `/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="71972-352">If a request is made for `/file.xml`, the request is redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="71972-353">狀態碼會設定為 301 (已永久移動)。</span><span class="sxs-lookup"><span data-stu-id="71972-353">The status code is set to *301 - Moved Permanently*.</span></span> <span data-ttu-id="71972-354">當瀏覽器針對 */xmlfiles/file.xml*發出新的要求時，靜態檔案中介軟體會從 *wwwroot/xmlfiles* 資料夾將檔案提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="71972-354">When the browser makes a new request for */xmlfiles/file.xml*, Static File Middleware serves the file to the client from the *wwwroot/xmlfiles* folder.</span></span> <span data-ttu-id="71972-355">若要重新導向，請明確設定回應的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="71972-355">For a redirect, explicitly set the status code of the response.</span></span> <span data-ttu-id="71972-356">否則會傳回 200 (確定) 狀態碼，用戶端上也不會發生重新導向。</span><span class="sxs-lookup"><span data-stu-id="71972-356">Otherwise, a *200 - OK* status code is returned, and the redirect doesn't occur on the client.</span></span>

<span data-ttu-id="71972-357">*RewriteRules.cs*：</span><span class="sxs-lookup"><span data-stu-id="71972-357">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

<span data-ttu-id="71972-358">這種方法也能重寫要求。</span><span class="sxs-lookup"><span data-stu-id="71972-358">This approach can also rewrite requests.</span></span> <span data-ttu-id="71972-359">範例應用程式示範了如何重寫任何文字檔要求的路徑，以從 *wwwroot* 資料夾提供 *file.txt* 文字檔。</span><span class="sxs-lookup"><span data-stu-id="71972-359">The sample app demonstrates rewriting the path for any text file request to serve the *file.txt* text file from the *wwwroot* folder.</span></span> <span data-ttu-id="71972-360">靜態檔案中介軟體會根據更新的要求路徑來提供檔案：</span><span class="sxs-lookup"><span data-stu-id="71972-360">Static File Middleware serves the file based on the updated request path:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

<span data-ttu-id="71972-361">*RewriteRules.cs*：</span><span class="sxs-lookup"><span data-stu-id="71972-361">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a><span data-ttu-id="71972-362">以 IRule 為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="71972-362">IRule-based rule</span></span>

<span data-ttu-id="71972-363">利用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 來使用類別中實作 <xref:Microsoft.AspNetCore.Rewrite.IRule> 介面的專屬規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="71972-363">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to use rule logic in a class that implements the <xref:Microsoft.AspNetCore.Rewrite.IRule> interface.</span></span> <span data-ttu-id="71972-364">相較於使用以方法為基礎的規則方法，`IRule` 提供了更高彈性。</span><span class="sxs-lookup"><span data-stu-id="71972-364">`IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="71972-365">您的實作類別可以包含建構函式，以便您在其中傳入 <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> 方法的參數。</span><span class="sxs-lookup"><span data-stu-id="71972-365">Your implementation class may include a constructor that allows you can pass in parameters for the <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> method.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="71972-366">為了滿足若干條件，系統會檢查範例應用程式中的 `extension` 和 `newPath` 參數值。</span><span class="sxs-lookup"><span data-stu-id="71972-366">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="71972-367">`extension` 必須包含值，而且值必須是 *.png*、*.jpg* 或 *.gif*。</span><span class="sxs-lookup"><span data-stu-id="71972-367">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="71972-368">如果 `newPath` 無效，就會擲回 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="71972-368">If the `newPath` isn't valid, an <xref:System.ArgumentException> is thrown.</span></span> <span data-ttu-id="71972-369">若要求是針對 *image.png* 發出，則要求會重新導向至 `/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="71972-369">If a request is made for *image.png*, the request is redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="71972-370">若要求是針對 *image.jpg* 發出，則要求會重新導向至 `/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="71972-370">If a request is made for *image.jpg*, the request is redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="71972-371">狀態碼會設定為 301 (已永久移動)，而 `context.Result` 會設定為停止處理規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="71972-371">The status code is set to *301 - Moved Permanently*, and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

<span data-ttu-id="71972-372">原始要求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="71972-372">Original Request: `/image.png`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.png 的要求和回應](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="71972-374">原始要求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="71972-374">Original Request: `/image.jpg`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.jpg 的要求和回應](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="71972-376">Regex 範例</span><span class="sxs-lookup"><span data-stu-id="71972-376">Regex examples</span></span>

| <span data-ttu-id="71972-377">Goal</span><span class="sxs-lookup"><span data-stu-id="71972-377">Goal</span></span> | <span data-ttu-id="71972-378">Regex 字串及</span><span class="sxs-lookup"><span data-stu-id="71972-378">Regex String &</span></span><br><span data-ttu-id="71972-379">比對範例</span><span class="sxs-lookup"><span data-stu-id="71972-379">Match Example</span></span> | <span data-ttu-id="71972-380">取代字串及</span><span class="sxs-lookup"><span data-stu-id="71972-380">Replacement String &</span></span><br><span data-ttu-id="71972-381">輸出範例</span><span class="sxs-lookup"><span data-stu-id="71972-381">Output Example</span></span> |
| ---- | ------------------------------- | -------------------------------------- |
| <span data-ttu-id="71972-382">將路徑重寫成查詢字串</span><span class="sxs-lookup"><span data-stu-id="71972-382">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="71972-383">移除斜線</span><span class="sxs-lookup"><span data-stu-id="71972-383">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="71972-384">強制使用斜線</span><span class="sxs-lookup"><span data-stu-id="71972-384">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="71972-385">避免重寫特定的要求</span><span class="sxs-lookup"><span data-stu-id="71972-385">Avoid rewriting specific requests</span></span> | <span data-ttu-id="71972-386">`^(.*)(?<!\.axd)$` 或 `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="71972-386">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="71972-387">是：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="71972-387">Yes: `/resource.htm`</span></span><br><span data-ttu-id="71972-388">否：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="71972-388">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="71972-389">重新排列 URL 區段</span><span class="sxs-lookup"><span data-stu-id="71972-389">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="71972-390">取代 URL 區段</span><span class="sxs-lookup"><span data-stu-id="71972-390">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="71972-391">其他資源</span><span class="sxs-lookup"><span data-stu-id="71972-391">Additional resources</span></span>

* [<span data-ttu-id="71972-392">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="71972-392">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="71972-393">中介軟體</span><span class="sxs-lookup"><span data-stu-id="71972-393">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="71972-394">.NET 中的規則運算式</span><span class="sxs-lookup"><span data-stu-id="71972-394">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="71972-395">規則運算式語言 - 快速參考</span><span class="sxs-lookup"><span data-stu-id="71972-395">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="71972-396">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="71972-396">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* <span data-ttu-id="71972-397">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0 (適用於 IIS))</span><span class="sxs-lookup"><span data-stu-id="71972-397">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="71972-398">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)</span><span class="sxs-lookup"><span data-stu-id="71972-398">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* [<span data-ttu-id="71972-399">IIS URL Rewrite Module 論壇</span><span class="sxs-lookup"><span data-stu-id="71972-399">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* <span data-ttu-id="71972-400">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (保持精簡的 URL 結構)</span><span class="sxs-lookup"><span data-stu-id="71972-400">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en)</span></span>
* <span data-ttu-id="71972-401">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 個重寫 URL 的祕訣與技巧)</span><span class="sxs-lookup"><span data-stu-id="71972-401">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="71972-402">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (是否要使用斜線)</span><span class="sxs-lookup"><span data-stu-id="71972-402">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
