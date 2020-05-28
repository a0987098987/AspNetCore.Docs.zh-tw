---
<span data-ttu-id="f8608-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-102">'Blazor'</span></span>
- <span data-ttu-id="f8608-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-103">'Identity'</span></span>
- <span data-ttu-id="f8608-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-105">'Razor'</span></span>
- <span data-ttu-id="f8608-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-106">'SignalR' uid:</span></span> 

---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="f8608-107">ASP.NET Core 的 URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="f8608-107">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="f8608-108">依[Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="f8608-108">By [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f8608-109">本文件介紹 URL 重寫，並提供如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。</span><span class="sxs-lookup"><span data-stu-id="f8608-109">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

<span data-ttu-id="f8608-110">URL 重寫是指根據一或多個預先定義的規則來修改要求 URL 的動作。</span><span class="sxs-lookup"><span data-stu-id="f8608-110">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="f8608-111">URL 重寫會在資源位置和位址之間建立一個抽象層，讓位置和位址不那麼緊密連結。</span><span class="sxs-lookup"><span data-stu-id="f8608-111">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses aren't tightly linked.</span></span> <span data-ttu-id="f8608-112">下列幾種情況非常適合使用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="f8608-112">URL rewriting is valuable in several scenarios to:</span></span>

* <span data-ttu-id="f8608-113">暫時或永久性移動/取代伺服器資源，並為這些資源保持穩定的定位器時。</span><span class="sxs-lookup"><span data-stu-id="f8608-113">Move or replace server resources temporarily or permanently and maintain stable locators for those resources.</span></span>
* <span data-ttu-id="f8608-114">跨不同應用程式或跨單一應用程式各區域來分割要求處理作業。</span><span class="sxs-lookup"><span data-stu-id="f8608-114">Split request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="f8608-115">移除、新增或重組傳入要求的 URL 區段。</span><span class="sxs-lookup"><span data-stu-id="f8608-115">Remove, add, or reorganize URL segments on incoming requests.</span></span>
* <span data-ttu-id="f8608-116">針對搜尋引擎最佳化 (SEO) 來將公開 URL 最佳化。</span><span class="sxs-lookup"><span data-stu-id="f8608-116">Optimize public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="f8608-117">允許使用易懂的公開 URL，來協助訪客預測要求資源所傳回的內容。</span><span class="sxs-lookup"><span data-stu-id="f8608-117">Permit the use of friendly public URLs to help visitors predict the content returned by requesting a resource.</span></span>
* <span data-ttu-id="f8608-118">將不安全的要求重新導向至安全的端點。</span><span class="sxs-lookup"><span data-stu-id="f8608-118">Redirect insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="f8608-119">防止直接連結，即外部網站透過將另一個網站的資產連結至本身的內容，來盜用該網站擁有的靜態資產。</span><span class="sxs-lookup"><span data-stu-id="f8608-119">Prevent hotlinking, where an external site uses a hosted static asset on another site by linking the asset into its own content.</span></span>

> [!NOTE]
> <span data-ttu-id="f8608-120">URL 重寫可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="f8608-120">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="f8608-121">如果可行的話，請限制規則的數目與複雜程度。</span><span class="sxs-lookup"><span data-stu-id="f8608-121">Where feasible, limit the number and complexity of rules.</span></span>

<span data-ttu-id="f8608-122">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="f8608-122">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="f8608-123">URL 重新導向和 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f8608-123">URL redirect and URL rewrite</span></span>

<span data-ttu-id="f8608-124">乍看之下，「URL 重新導向」\*\* 和「URL 重寫」\*\* 在用語上的差異不太明顯，但卻對提供資源給用戶端方面有重要的影響。</span><span class="sxs-lookup"><span data-stu-id="f8608-124">The difference in wording between *URL redirect* and *URL rewrite* is subtle but has important implications for providing resources to clients.</span></span> <span data-ttu-id="f8608-125">ASP.NET Core 的 URL 重寫中介軟體可以同時滿足這兩種需求。</span><span class="sxs-lookup"><span data-stu-id="f8608-125">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="f8608-126">「URL 重新導向」\*\* 牽涉有關用戶端的作業，其會指示用戶端到其他位址存取資源，而非用戶端原本要求的位址。</span><span class="sxs-lookup"><span data-stu-id="f8608-126">A *URL redirect* involves a client-side operation, where the client is instructed to access a resource at a different address than the client originally requested.</span></span> <span data-ttu-id="f8608-127">這項作業需要伺服器的來回行程。</span><span class="sxs-lookup"><span data-stu-id="f8608-127">This requires a round trip to the server.</span></span> <span data-ttu-id="f8608-128">當用戶端提出新的資源要求時，系統會將重新導向 URL 傳回給用戶端，並顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f8608-128">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span>

<span data-ttu-id="f8608-129">若 `/resource` 重新導向\*\* 至 `/different-resource`，則伺服器會回應用戶端應前往 `/different-resource` 取得資源，並提供狀態碼，指出重新導向為暫時性或永久性。</span><span class="sxs-lookup"><span data-stu-id="f8608-129">If `/resource` is *redirected* to `/different-resource`, the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span>

![伺服器上的 WebAPI 服務端點已暫時從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="f8608-135">將要求重新導向至不同的 URL 時，可以透過指定具狀態的回應，來指出該重新導向為永久性或暫時性：</span><span class="sxs-lookup"><span data-stu-id="f8608-135">When redirecting requests to a different URL, indicate whether the redirect is permanent or temporary by specifying the status code with the response:</span></span>

* <span data-ttu-id="f8608-136">如果資源具有新的永久 URL，且您希望告知用戶端所有未來的資源要求皆應使用這個新 URL，請使用 301 (已永久移動)\*\* 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-136">The *301 - Moved Permanently* status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="f8608-137">用戶端收到 301 狀態碼時，可能會快取並重複使用回應。\*\*</span><span class="sxs-lookup"><span data-stu-id="f8608-137">*The client may cache and reuse the response when a 301 status code is received.*</span></span>

* <span data-ttu-id="f8608-138">若重新導向為暫時性或很有可能變更時，請使用 302 (已找到)\*\* 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-138">The *302 - Found* status code is used where the redirection is temporary or generally subject to change.</span></span> <span data-ttu-id="f8608-139">302 狀態碼會指示用戶端不要儲存 URL 並在之後使用。</span><span class="sxs-lookup"><span data-stu-id="f8608-139">The 302 status code indicates to the client not to store the URL and use it in the future.</span></span>

<span data-ttu-id="f8608-140">如需狀態碼的詳細資訊，請參閱 [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616：狀態碼定義)。</span><span class="sxs-lookup"><span data-stu-id="f8608-140">For more information on status codes, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="f8608-141">「URL 重寫」\*\* 伺服器端作業，會從用戶端要求以外的其他資源位址提供資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-141">A *URL rewrite* is a server-side operation that provides a resource from a different resource address than the client requested.</span></span> <span data-ttu-id="f8608-142">重寫 URL 並不需要伺服器的來回行程。</span><span class="sxs-lookup"><span data-stu-id="f8608-142">Rewriting a URL doesn't require a round trip to the server.</span></span> <span data-ttu-id="f8608-143">系統不會將重寫的 URL 傳回用戶端，也不會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f8608-143">The rewritten URL isn't returned to the client and doesn't appear in the browser's address bar.</span></span>

<span data-ttu-id="f8608-144">若 `/resource`「重寫」\*\* 至 `/different-resource`，則伺服器會於「內部」\*\* 擷取資源，並在 `/different-resource` 傳回資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-144">If `/resource` is *rewritten* to `/different-resource`, the server *internally* fetches and returns the resource at `/different-resource`.</span></span>

<span data-ttu-id="f8608-145">雖然用戶端或許可以在重寫的 URL 處擷取資源，但用戶端並不會在提出要求與接收回應時得知資源位於重寫的 URL 處。</span><span class="sxs-lookup"><span data-stu-id="f8608-145">Although the client might be able to retrieve the resource at the rewritten URL, the client isn't informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![伺服器上的 WebAPI 服務端點已從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="f8608-150">URL 重寫範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f8608-150">URL rewriting sample app</span></span>

<span data-ttu-id="f8608-151">您可以利用[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)，來探索 URL 重寫中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="f8608-151">You can explore the features of the URL Rewriting Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="f8608-152">範例應用程式會套用重新導向與重寫規則，還會示範多種情況下的重新導向或重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="f8608-152">The app applies redirect and rewrite rules and shows the redirected or rewritten URL for several scenarios.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="f8608-153">URL 重寫中介軟體的使用時機</span><span class="sxs-lookup"><span data-stu-id="f8608-153">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="f8608-154">當您無法使用以下方法時，請使用重寫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f8608-154">Use URL Rewriting Middleware when you're unable to use the following approaches:</span></span>

* [<span data-ttu-id="f8608-155">Windows Server 上的 IIS URL Rewrite module</span><span class="sxs-lookup"><span data-stu-id="f8608-155">URL Rewrite module with IIS on Windows Server</span></span>](https://www.iis.net/downloads/microsoft/url-rewrite)
* [<span data-ttu-id="f8608-156">Apache Server 上的 Apache mod_rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="f8608-156">Apache mod_rewrite module on Apache Server</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="f8608-157">Nginx 上的 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f8608-157">URL rewriting on Nginx</span></span>](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<span data-ttu-id="f8608-158">此外也請在應用程式裝載於 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (之前稱為 WebListener) 時使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-158">Also, use the middleware when the app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener).</span></span>

<span data-ttu-id="f8608-159">使用 IIS、Apache 和 Nginx 所提供伺服器型 URL 重寫技術的主要原因包括：</span><span class="sxs-lookup"><span data-stu-id="f8608-159">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, and Nginx are:</span></span>

* <span data-ttu-id="f8608-160">中介軟體不支援這些模組的完整功能。</span><span class="sxs-lookup"><span data-stu-id="f8608-160">The middleware doesn't support the full features of these modules.</span></span>

  <span data-ttu-id="f8608-161">有部分伺服器模組的功能不適用於 ASP.NET Core 專案，例如 IIS Rewrite Module 規則的 `IsFile` 和 `IsDirectory` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="f8608-161">Some of the features of the server modules don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="f8608-162">在這些情況下，請改為使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-162">In these scenarios, use the middleware instead.</span></span>
* <span data-ttu-id="f8608-163">中介軟體的效能可能比不上模組的效能時。</span><span class="sxs-lookup"><span data-stu-id="f8608-163">The performance of the middleware probably doesn't match that of the modules.</span></span>

  <span data-ttu-id="f8608-164">如果要確實得知哪種方法會降低最多效能，或是降低的效能可以忽略的話，進行效能評定是唯一方法。</span><span class="sxs-lookup"><span data-stu-id="f8608-164">Benchmarking is the only way to know for sure which approach degrades performance the most or if degraded performance is negligible.</span></span>

## <a name="package"></a><span data-ttu-id="f8608-165">套件</span><span class="sxs-lookup"><span data-stu-id="f8608-165">Package</span></span>

<span data-ttu-id="f8608-166">URL 重寫中介軟體由 [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) 套件所提供，其會以隱含方式包含在 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="f8608-166">URL Rewriting Middleware is provided by the [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) package, which is implicitly included in ASP.NET Core apps.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="f8608-167">延伸模組和選項</span><span class="sxs-lookup"><span data-stu-id="f8608-167">Extension and options</span></span>

<span data-ttu-id="f8608-168">若要建立 URL 重寫和重新導向規則，您可以針對每個重寫規則的擴充方法建立 [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8608-168">Establish URL rewrite and redirect rules by creating an instance of the [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) class with extension methods for each of your rewrite rules.</span></span> <span data-ttu-id="f8608-169">依據所需的處理順序來鏈結多個規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-169">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="f8608-170">系統會使用 <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*> 將 `RewriteOptions` 新增至要求管線，並將其傳入 URL 重寫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f8608-170">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="f8608-171">將非 www 重新導向 www</span><span class="sxs-lookup"><span data-stu-id="f8608-171">Redirect non-www to www</span></span>

<span data-ttu-id="f8608-172">有三個選項可讓應用程式將非 `www` 要求重新導向 `www`：</span><span class="sxs-lookup"><span data-stu-id="f8608-172">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="f8608-173"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*>：如果要求為非，則將要求永久重新導向至 `www` 子域 `www` 。</span><span class="sxs-lookup"><span data-stu-id="f8608-173"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*>: Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="f8608-174">使用 [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-174">Redirects with a [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) status code.</span></span>

* <span data-ttu-id="f8608-175"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*>： `www` 如果連入要求為非，則將要求重新導向至子域 `www` 。</span><span class="sxs-lookup"><span data-stu-id="f8608-175"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*>: Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="f8608-176">使用 [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-176">Redirects with a [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) status code.</span></span> <span data-ttu-id="f8608-177">多載可讓您提供回應的狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-177">An overload permits you to provide the status code for the response.</span></span> <span data-ttu-id="f8608-178">請使用 <xref:Microsoft.AspNetCore.Http.StatusCodes> 類別的欄位來進行狀態碼指派。</span><span class="sxs-lookup"><span data-stu-id="f8608-178">Use a field of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for a status code assignment.</span></span>

### <a name="url-redirect"></a><span data-ttu-id="f8608-179">URL 重新導向</span><span class="sxs-lookup"><span data-stu-id="f8608-179">URL redirect</span></span>

<span data-ttu-id="f8608-180">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> 將要求重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-180">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> to redirect requests.</span></span> <span data-ttu-id="f8608-181">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-181">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="f8608-182">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="f8608-182">The second parameter is the replacement string.</span></span> <span data-ttu-id="f8608-183">第三個參數 (如果有的話) 會指定狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-183">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="f8608-184">如果您未指定狀態碼，則狀態碼會預設為 302 (已找到)\*\*，表示已暫時移動或取代資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-184">If you don't specify the status code, the status code defaults to *302 - Found*, which indicates that the resource is temporarily moved or replaced.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="f8608-185">在啟用開發人員工具的瀏覽器中，使用 `/redirect-rule/1234/5678` 路徑提出範例應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-185">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="f8608-186">Regex 會比對 `redirect-rule/(.*)` 上的要求路徑，並會將路徑取代為 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="f8608-186">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="f8608-187">系統會將重新導向 URL 與 302 (已找到)\*\* 狀態碼傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-187">The redirect URL is sent back to the client with a *302 - Found* status code.</span></span> <span data-ttu-id="f8608-188">瀏覽器在重新導向 URL 處提出新要求，該 URL 也會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f8608-188">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="f8608-189">因為範例應用程式與重新導向 URL 沒有任何相符規則：</span><span class="sxs-lookup"><span data-stu-id="f8608-189">Since no rules in the sample app match on the redirect URL:</span></span>

* <span data-ttu-id="f8608-190">第二個要求會從應用程式收到 200 (確定)\*\* 回應。</span><span class="sxs-lookup"><span data-stu-id="f8608-190">The second request receives a *200 - OK* response from the app.</span></span>
* <span data-ttu-id="f8608-191">回應的本文會顯示重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="f8608-191">The body of the response shows the redirect URL.</span></span>

<span data-ttu-id="f8608-192">「重新導向」\*\* URL 時，會對伺服器進行來回行程。</span><span class="sxs-lookup"><span data-stu-id="f8608-192">A round trip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="f8608-193">建立重新導向規則時，請務必謹慎。</span><span class="sxs-lookup"><span data-stu-id="f8608-193">Be cautious when establishing redirect rules.</span></span> <span data-ttu-id="f8608-194">每向應用程式提出要求時 (包括重新導向後)，請評估您的重新導向規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-194">Redirect rules are evaluated on every request to the app, including after a redirect.</span></span> <span data-ttu-id="f8608-195">因為您有可能會不小心建立無限重新導向迴圈\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-195">It's easy to accidentally create a *loop of infinite redirects*.</span></span>

<span data-ttu-id="f8608-196">原始要求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="f8608-196">Original Request: `/redirect-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="f8608-198">括弧內所含的運算式部分稱為「擷取群組」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-198">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="f8608-199">運算式的點 (`.`) 表示「比對任何字元」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-199">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="f8608-200">星號 (`*`) 表示「比對前置字元零或多次」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-200">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="f8608-201">因此，擷取群組 `(.*)` 會擷取 URL 的 `1234/5678` 最後這兩個路徑區段。</span><span class="sxs-lookup"><span data-stu-id="f8608-201">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="f8608-202">這個單一擷取群組會擷取您在要求 URL 中的 `redirect-rule/` 之後所提供的任何值。</span><span class="sxs-lookup"><span data-stu-id="f8608-202">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="f8608-203">系統會將擷取的群組以貨幣符號 (`$`) 後接擷取序號的形式，插入取代字串中。</span><span class="sxs-lookup"><span data-stu-id="f8608-203">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="f8608-204">使用 `$1` 可取得第一個擷取群組值、使用 `$2` 則會取得第二個，依您的 Regex 擷取群組順序以此類推。</span><span class="sxs-lookup"><span data-stu-id="f8608-204">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="f8608-205">在範例應用程式中，重新導向規則 Regex 只有一個擷取的群組，因此在取代字串中只有 `$1` 這一個插入的群組。</span><span class="sxs-lookup"><span data-stu-id="f8608-205">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="f8608-206">套用規則時，URL 會變成 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="f8608-206">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="f8608-207">將 URL 重新導向至安全端點</span><span class="sxs-lookup"><span data-stu-id="f8608-207">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="f8608-208">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> 將 HTTP 要求重新導向至使用 HTTPS 通訊協定的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-208">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> to redirect HTTP requests to the same host and path using the HTTPS protocol.</span></span> <span data-ttu-id="f8608-209">如果未提供狀態碼，中介軟體會預設為 302 (已找到)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-209">If the status code isn't supplied, the middleware defaults to *302 - Found*.</span></span> <span data-ttu-id="f8608-210">若未提供連接埠：</span><span class="sxs-lookup"><span data-stu-id="f8608-210">If the port isn't supplied:</span></span>

* <span data-ttu-id="f8608-211">中介軟體會預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="f8608-211">The middleware defaults to `null`.</span></span>
* <span data-ttu-id="f8608-212">配置會變更為 `https` (HTTPS 通訊協定)，且用戶端會在連接埠 443 上存取資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-212">The scheme changes to `https` (HTTPS protocol), and the client accesses the resource on port 443.</span></span>

<span data-ttu-id="f8608-213">下列範例示範了如何將狀態碼設為 301 (已永久移動)\*\*，並將連接埠變更為 5001。</span><span class="sxs-lookup"><span data-stu-id="f8608-213">The following example shows how to set the status code to *301 - Moved Permanently* and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="f8608-214">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> 將不安全的要求重新導向至使用安全 HTTPS 通訊協定 (在連接埠 443 上) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-214">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> to redirect insecure requests to the same host and path with secure HTTPS protocol on port 443.</span></span> <span data-ttu-id="f8608-215">中介軟體會將狀態碼設定為 301 (已永久移動)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-215">The middleware sets the status code to *301 - Moved Permanently*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="f8608-216">在不要求額外重新導向規則的情況下重新導向至安全的端點時，建議使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-216">When redirecting to a secure endpoint without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="f8608-217">如需詳細資訊，請參閱[強制執行 HTTPS](xref:security/enforcing-ssl#require-https) 主題。</span><span class="sxs-lookup"><span data-stu-id="f8608-217">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="f8608-218">範例應用程式可以示範如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="f8608-218">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="f8608-219">將擴充方法新增至 `RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="f8608-219">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="f8608-220">在任何 URL 位置，向應用程式提出不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-220">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="f8608-221">關閉瀏覽器自我簽署憑證不受信任的安全性警告，或建立信任憑證的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8608-221">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="f8608-222">使用 `AddRedirectToHttps(301, 5001)` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="f8608-222">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="f8608-224">使用 `AddRedirectToHttpsPermanent` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="f8608-224">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="f8608-226">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f8608-226">URL rewrite</span></span>

<span data-ttu-id="f8608-227">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> 來建立 URL 重寫的規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-227">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> to create a rule for rewriting URLs.</span></span> <span data-ttu-id="f8608-228">第一個參數會包含 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-228">The first parameter contains the regex for matching on the incoming URL path.</span></span> <span data-ttu-id="f8608-229">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="f8608-229">The second parameter is the replacement string.</span></span> <span data-ttu-id="f8608-230">第三個參數 `skipRemainingRules: {true|false}` 指出中介軟體是否要在套用目前規則時略過其他重寫規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-230">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

<span data-ttu-id="f8608-231">原始要求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="f8608-231">Original Request: `/rewrite-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="f8608-233">運算式開頭的插入號 (`^`) 表示，比對會從 URL 路徑的開頭開始。</span><span class="sxs-lookup"><span data-stu-id="f8608-233">The carat (`^`) at the beginning of the expression means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="f8608-234">在先前的重新導向規則範例 `redirect-rule/(.*)` 中，Regex 的開頭沒有插入號 (`^`)。</span><span class="sxs-lookup"><span data-stu-id="f8608-234">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat (`^`) at the start of the regex.</span></span> <span data-ttu-id="f8608-235">因此，就算 `redirect-rule/` 前有任何字元也能成功比對。</span><span class="sxs-lookup"><span data-stu-id="f8608-235">Therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="f8608-236">路徑</span><span class="sxs-lookup"><span data-stu-id="f8608-236">Path</span></span>                               | <span data-ttu-id="f8608-237">比對</span><span class="sxs-lookup"><span data-stu-id="f8608-237">Match</span></span> |
| ---
<span data-ttu-id="f8608-238">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-238">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-239">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-239">'Blazor'</span></span>
- <span data-ttu-id="f8608-240">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-240">'Identity'</span></span>
- <span data-ttu-id="f8608-241">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-241">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-242">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-242">'Razor'</span></span>
- <span data-ttu-id="f8608-243">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-243">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-244">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-244">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-245">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-245">'Blazor'</span></span>
- <span data-ttu-id="f8608-246">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-246">'Identity'</span></span>
- <span data-ttu-id="f8608-247">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-247">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-248">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-248">'Razor'</span></span>
- <span data-ttu-id="f8608-249">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-249">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-250">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-250">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-251">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-251">'Blazor'</span></span>
- <span data-ttu-id="f8608-252">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-252">'Identity'</span></span>
- <span data-ttu-id="f8608-253">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-253">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-254">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-254">'Razor'</span></span>
- <span data-ttu-id="f8608-255">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-255">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-256">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-256">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-257">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-257">'Blazor'</span></span>
- <span data-ttu-id="f8608-258">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-258">'Identity'</span></span>
- <span data-ttu-id="f8608-259">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-259">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-260">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-260">'Razor'</span></span>
- <span data-ttu-id="f8608-261">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-261">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-262">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-262">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-263">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-263">'Blazor'</span></span>
- <span data-ttu-id="f8608-264">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-264">'Identity'</span></span>
- <span data-ttu-id="f8608-265">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-265">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-266">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-266">'Razor'</span></span>
- <span data-ttu-id="f8608-267">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-267">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-268">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-268">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-269">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-269">'Blazor'</span></span>
- <span data-ttu-id="f8608-270">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-270">'Identity'</span></span>
- <span data-ttu-id="f8608-271">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-271">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-272">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-272">'Razor'</span></span>
- <span data-ttu-id="f8608-273">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-273">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-274">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-274">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-275">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-275">'Blazor'</span></span>
- <span data-ttu-id="f8608-276">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-276">'Identity'</span></span>
- <span data-ttu-id="f8608-277">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-277">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-278">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-278">'Razor'</span></span>
- <span data-ttu-id="f8608-279">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-279">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-280">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-280">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-281">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-281">'Blazor'</span></span>
- <span data-ttu-id="f8608-282">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-282">'Identity'</span></span>
- <span data-ttu-id="f8608-283">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-283">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-284">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-284">'Razor'</span></span>
- <span data-ttu-id="f8608-285">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-285">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-286">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-286">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-287">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-287">'Blazor'</span></span>
- <span data-ttu-id="f8608-288">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-288">'Identity'</span></span>
- <span data-ttu-id="f8608-289">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-289">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-290">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-290">'Razor'</span></span>
- <span data-ttu-id="f8608-291">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-291">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-292">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-292">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-293">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-293">'Blazor'</span></span>
- <span data-ttu-id="f8608-294">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-294">'Identity'</span></span>
- <span data-ttu-id="f8608-295">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-295">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-296">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-296">'Razor'</span></span>
- <span data-ttu-id="f8608-297">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-297">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-298">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-298">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-299">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-299">'Blazor'</span></span>
- <span data-ttu-id="f8608-300">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-300">'Identity'</span></span>
- <span data-ttu-id="f8608-301">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-301">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-302">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-302">'Razor'</span></span>
- <span data-ttu-id="f8608-303">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-303">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-304">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-304">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-305">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-305">'Blazor'</span></span>
- <span data-ttu-id="f8608-306">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-306">'Identity'</span></span>
- <span data-ttu-id="f8608-307">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-307">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-308">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-308">'Razor'</span></span>
- <span data-ttu-id="f8608-309">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-309">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-310">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-310">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-311">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-311">'Blazor'</span></span>
- <span data-ttu-id="f8608-312">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-312">'Identity'</span></span>
- <span data-ttu-id="f8608-313">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-313">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-314">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-314">'Razor'</span></span>
- <span data-ttu-id="f8608-315">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-315">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-316">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-316">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-317">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-317">'Blazor'</span></span>
- <span data-ttu-id="f8608-318">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-318">'Identity'</span></span>
- <span data-ttu-id="f8608-319">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-319">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-320">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-320">'Razor'</span></span>
- <span data-ttu-id="f8608-321">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-321">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-322">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-322">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-323">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-323">'Blazor'</span></span>
- <span data-ttu-id="f8608-324">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-324">'Identity'</span></span>
- <span data-ttu-id="f8608-325">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-325">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-326">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-326">'Razor'</span></span>
- <span data-ttu-id="f8608-327">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-327">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-328">----------------- |:---: || `/redirect-rule/1234/5678`         |是 || `/my-cool-redirect-rule/1234/5678` |是 || `/anotherredirect-rule/1234/5678`  |是 |</span><span class="sxs-lookup"><span data-stu-id="f8608-328">----------------- | :---: | | `/redirect-rule/1234/5678`         | Yes   | | `/my-cool-redirect-rule/1234/5678` | Yes   | | `/anotherredirect-rule/1234/5678`  | Yes   |</span></span>

<span data-ttu-id="f8608-329">`^rewrite-rule/(\d+)/(\d+)` 重寫規則只會比對開頭為 `rewrite-rule/` 的路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-329">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="f8608-330">請注意下表中的比對差異。</span><span class="sxs-lookup"><span data-stu-id="f8608-330">In the following table, note the difference in matching.</span></span>

| <span data-ttu-id="f8608-331">路徑</span><span class="sxs-lookup"><span data-stu-id="f8608-331">Path</span></span>                              | <span data-ttu-id="f8608-332">比對</span><span class="sxs-lookup"><span data-stu-id="f8608-332">Match</span></span> |
| ---
<span data-ttu-id="f8608-333">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-333">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-334">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-334">'Blazor'</span></span>
- <span data-ttu-id="f8608-335">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-335">'Identity'</span></span>
- <span data-ttu-id="f8608-336">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-336">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-337">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-337">'Razor'</span></span>
- <span data-ttu-id="f8608-338">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-338">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-339">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-339">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-340">'Blazor'</span></span>
- <span data-ttu-id="f8608-341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-341">'Identity'</span></span>
- <span data-ttu-id="f8608-342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-342">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-343">'Razor'</span></span>
- <span data-ttu-id="f8608-344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-345">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-345">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-346">'Blazor'</span></span>
- <span data-ttu-id="f8608-347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-347">'Identity'</span></span>
- <span data-ttu-id="f8608-348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-348">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-349">'Razor'</span></span>
- <span data-ttu-id="f8608-350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-351">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-351">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-352">'Blazor'</span></span>
- <span data-ttu-id="f8608-353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-353">'Identity'</span></span>
- <span data-ttu-id="f8608-354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-354">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-355">'Razor'</span></span>
- <span data-ttu-id="f8608-356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-357">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-357">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-358">'Blazor'</span></span>
- <span data-ttu-id="f8608-359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-359">'Identity'</span></span>
- <span data-ttu-id="f8608-360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-360">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-361">'Razor'</span></span>
- <span data-ttu-id="f8608-362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-362">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-363">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-363">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-364">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-364">'Blazor'</span></span>
- <span data-ttu-id="f8608-365">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-365">'Identity'</span></span>
- <span data-ttu-id="f8608-366">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-366">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-367">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-367">'Razor'</span></span>
- <span data-ttu-id="f8608-368">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-368">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-369">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-369">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-370">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-370">'Blazor'</span></span>
- <span data-ttu-id="f8608-371">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-371">'Identity'</span></span>
- <span data-ttu-id="f8608-372">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-372">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-373">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-373">'Razor'</span></span>
- <span data-ttu-id="f8608-374">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-374">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-375">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-375">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-376">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-376">'Blazor'</span></span>
- <span data-ttu-id="f8608-377">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-377">'Identity'</span></span>
- <span data-ttu-id="f8608-378">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-378">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-379">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-379">'Razor'</span></span>
- <span data-ttu-id="f8608-380">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-380">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-381">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-381">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-382">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-382">'Blazor'</span></span>
- <span data-ttu-id="f8608-383">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-383">'Identity'</span></span>
- <span data-ttu-id="f8608-384">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-384">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-385">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-385">'Razor'</span></span>
- <span data-ttu-id="f8608-386">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-386">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-387">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-387">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-388">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-388">'Blazor'</span></span>
- <span data-ttu-id="f8608-389">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-389">'Identity'</span></span>
- <span data-ttu-id="f8608-390">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-390">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-391">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-391">'Razor'</span></span>
- <span data-ttu-id="f8608-392">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-392">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-393">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-393">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-394">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-394">'Blazor'</span></span>
- <span data-ttu-id="f8608-395">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-395">'Identity'</span></span>
- <span data-ttu-id="f8608-396">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-396">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-397">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-397">'Razor'</span></span>
- <span data-ttu-id="f8608-398">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-398">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-399">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-399">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-400">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-400">'Blazor'</span></span>
- <span data-ttu-id="f8608-401">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-401">'Identity'</span></span>
- <span data-ttu-id="f8608-402">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-402">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-403">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-403">'Razor'</span></span>
- <span data-ttu-id="f8608-404">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-404">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-405">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-405">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-406">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-406">'Blazor'</span></span>
- <span data-ttu-id="f8608-407">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-407">'Identity'</span></span>
- <span data-ttu-id="f8608-408">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-408">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-409">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-409">'Razor'</span></span>
- <span data-ttu-id="f8608-410">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-410">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-411">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-411">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-412">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-412">'Blazor'</span></span>
- <span data-ttu-id="f8608-413">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-413">'Identity'</span></span>
- <span data-ttu-id="f8608-414">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-414">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-415">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-415">'Razor'</span></span>
- <span data-ttu-id="f8608-416">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-416">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-417">----------------- |:---: || `/rewrite-rule/1234/5678`         |是 || `/my-cool-rewrite-rule/1234/5678` |否 || `/anotherrewrite-rule/1234/5678`  |否 |</span><span class="sxs-lookup"><span data-stu-id="f8608-417">----------------- | :---: | | `/rewrite-rule/1234/5678`         | Yes   | | `/my-cool-rewrite-rule/1234/5678` | No    | | `/anotherrewrite-rule/1234/5678`  | No    |</span></span>

<span data-ttu-id="f8608-418">運算式的 `^rewrite-rule/` 部分之後，有 `(\d+)/(\d+)` 這兩個擷取群組。</span><span class="sxs-lookup"><span data-stu-id="f8608-418">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="f8608-419">`\d` 表示「比對數字」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-419">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="f8608-420">加號 (`+`) 表示「比對一或多個前置字元」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-420">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="f8608-421">因此，URL 必須包含某個數字，後接斜線與另一個數字。</span><span class="sxs-lookup"><span data-stu-id="f8608-421">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="f8608-422">這些擷取群組會以 `$1` 和 `$2` 形式插入重寫的 URL。</span><span class="sxs-lookup"><span data-stu-id="f8608-422">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="f8608-423">重寫規則的取代字串會將擷取的群組放入查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="f8608-423">The rewrite rule replacement string places the captured groups into the query string.</span></span> <span data-ttu-id="f8608-424">系統會重寫 `/rewrite-rule/1234/5678` 的要求路徑，以取得位於 `/rewritten?var1=1234&var2=5678` 的資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-424">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="f8608-425">如果原始要求上有查詢字串，則會在重寫 URL 時予以保留。</span><span class="sxs-lookup"><span data-stu-id="f8608-425">If a query string is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="f8608-426">這麼做就不需要伺服器的來回行程，即可取得資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-426">There's no round trip to the server to obtain the resource.</span></span> <span data-ttu-id="f8608-427">如果資源存在，就會擷取該資源，並傳回 200 (確定)\*\* 狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-427">If the resource exists, it's fetched and returned to the client with a *200 - OK* status code.</span></span> <span data-ttu-id="f8608-428">因為系統並未重新導向用戶端，因此瀏覽器網址列中的 URL 不會變更。</span><span class="sxs-lookup"><span data-stu-id="f8608-428">Because the client isn't redirected, the URL in the browser's address bar doesn't change.</span></span> <span data-ttu-id="f8608-429">用戶端也無法偵測到伺服器上發生 URL 重寫作業。</span><span class="sxs-lookup"><span data-stu-id="f8608-429">Clients can't detect that a URL rewrite operation occurred on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="f8608-430">因為比對規則是計算繁複的程序，且會增加應用程式的回應時間，所以請盡可能使用 `skipRemainingRules: true`。</span><span class="sxs-lookup"><span data-stu-id="f8608-430">Use `skipRemainingRules: true` whenever possible because matching rules is computationally expensive and increases app response time.</span></span> <span data-ttu-id="f8608-431">如需最快速的應用程式回應：</span><span class="sxs-lookup"><span data-stu-id="f8608-431">For the fastest app response:</span></span>
>
> * <span data-ttu-id="f8608-432">排序重寫規則；從最常比對的規則排到最不常比對的規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-432">Order rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="f8608-433">當出現符合項目且不需要任何額外的規則處理時，即略過剩下的規則處理。</span><span class="sxs-lookup"><span data-stu-id="f8608-433">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-mod_rewrite"></a><span data-ttu-id="f8608-434">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="f8608-434">Apache mod_rewrite</span></span>

<span data-ttu-id="f8608-435">使用 <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*> 來套用 Apache mod_rewrite 規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-435">Apply Apache mod_rewrite rules with <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span></span> <span data-ttu-id="f8608-436">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="f8608-436">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="f8608-437">如需 mod_rewrite 規則的詳細資訊和範例，請參閱 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="f8608-437">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

<span data-ttu-id="f8608-438">系統會使用 <xref:System.IO.StreamReader> 來讀取 *ApacheModRewrite.txt* 規則檔案的規則：</span><span class="sxs-lookup"><span data-stu-id="f8608-438">A <xref:System.IO.StreamReader> is used to read the rules from the *ApacheModRewrite.txt* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

<span data-ttu-id="f8608-439">範例應用程式會將要求從 `/apache-mod-rules-redirect/(.\*)` 重新導向至 `/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="f8608-439">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="f8608-440">回應狀態碼為 302 (已找到)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-440">The response status code is *302 - Found*.</span></span>

[!code[](url-rewriting/samples/3.x/SampleApp/ApacheModRewrite.txt)]

<span data-ttu-id="f8608-441">原始要求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="f8608-441">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="f8608-443">中介軟體可支援下列 Apache mod_rewrite 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="f8608-443">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="f8608-444">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-444">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="f8608-445">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="f8608-445">HTTP_ACCEPT</span></span>
* <span data-ttu-id="f8608-446">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="f8608-446">HTTP_CONNECTION</span></span>
* <span data-ttu-id="f8608-447">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="f8608-447">HTTP_COOKIE</span></span>
* <span data-ttu-id="f8608-448">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="f8608-448">HTTP_FORWARDED</span></span>
* <span data-ttu-id="f8608-449">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="f8608-449">HTTP_HOST</span></span>
* <span data-ttu-id="f8608-450">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="f8608-450">HTTP_REFERER</span></span>
* <span data-ttu-id="f8608-451">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="f8608-451">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="f8608-452">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f8608-452">HTTPS</span></span>
* <span data-ttu-id="f8608-453">IPV6</span><span class="sxs-lookup"><span data-stu-id="f8608-453">IPV6</span></span>
* <span data-ttu-id="f8608-454">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="f8608-454">QUERY_STRING</span></span>
* <span data-ttu-id="f8608-455">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-455">REMOTE_ADDR</span></span>
* <span data-ttu-id="f8608-456">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="f8608-456">REMOTE_PORT</span></span>
* <span data-ttu-id="f8608-457">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f8608-457">REQUEST_FILENAME</span></span>
* <span data-ttu-id="f8608-458">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="f8608-458">REQUEST_METHOD</span></span>
* <span data-ttu-id="f8608-459">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="f8608-459">REQUEST_SCHEME</span></span>
* <span data-ttu-id="f8608-460">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="f8608-460">REQUEST_URI</span></span>
* <span data-ttu-id="f8608-461">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f8608-461">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="f8608-462">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-462">SERVER_ADDR</span></span>
* <span data-ttu-id="f8608-463">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="f8608-463">SERVER_PORT</span></span>
* <span data-ttu-id="f8608-464">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="f8608-464">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="f8608-465">TIME</span><span class="sxs-lookup"><span data-stu-id="f8608-465">TIME</span></span>
* <span data-ttu-id="f8608-466">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="f8608-466">TIME_DAY</span></span>
* <span data-ttu-id="f8608-467">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="f8608-467">TIME_HOUR</span></span>
* <span data-ttu-id="f8608-468">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="f8608-468">TIME_MIN</span></span>
* <span data-ttu-id="f8608-469">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="f8608-469">TIME_MON</span></span>
* <span data-ttu-id="f8608-470">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="f8608-470">TIME_SEC</span></span>
* <span data-ttu-id="f8608-471">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="f8608-471">TIME_WDAY</span></span>
* <span data-ttu-id="f8608-472">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="f8608-472">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="f8608-473">IIS URL Rewrite Module 規則</span><span class="sxs-lookup"><span data-stu-id="f8608-473">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="f8608-474">若要使用與套用至 IIS URL Rewrite Module 一樣的規則集，請使用 <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>。</span><span class="sxs-lookup"><span data-stu-id="f8608-474">To use the same rule set that applies to the IIS URL Rewrite Module, use <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span></span> <span data-ttu-id="f8608-475">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="f8608-475">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="f8608-476">在 Windows Server IIS 上執行時，請不要引導中介軟體使用應用程式的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f8608-476">Don't direct the middleware to use the app's *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="f8608-477">使用 IIS 時，這些規則應該儲存在應用程式的 *web.config* 檔案外部，以免與 IIS Rewrite Module 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="f8608-477">With IIS, these rules should be stored outside of the app's *web.config* file in order to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="f8608-478">如需 IIS URL Rewrite Module 規則的詳細資訊和範例，請參閱 [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0) 和 [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)。</span><span class="sxs-lookup"><span data-stu-id="f8608-478">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

<span data-ttu-id="f8608-479">系統會使用 <xref:System.IO.StreamReader> 來讀取 *IISUrlRewrite.xml* 規則檔案的規則：</span><span class="sxs-lookup"><span data-stu-id="f8608-479">A <xref:System.IO.StreamReader> is used to read the rules from the *IISUrlRewrite.xml* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

<span data-ttu-id="f8608-480">範例應用程式會將要求從 `/iis-rules-rewrite/(.*)` 重寫至 `/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="f8608-480">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="f8608-481">系統會將回應與 200 (確定)\*\* 狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-481">The response is sent to the client with a *200 - OK* status code.</span></span>

[!code-xml[](url-rewriting/samples/3.x/SampleApp/IISUrlRewrite.xml)]

<span data-ttu-id="f8608-482">原始要求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="f8608-482">Original Request: `/iis-rules-rewrite/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="f8608-484">如果您具備使用中且已設定伺服器層級規則的 IIS Rewrite Module，但其會以非預期的方式影響應用程式，則您可以停用應用程式的 IIS Rewrite Module 。</span><span class="sxs-lookup"><span data-stu-id="f8608-484">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="f8608-485">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="f8608-485">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="f8608-486">不支援的功能</span><span class="sxs-lookup"><span data-stu-id="f8608-486">Unsupported features</span></span>

<span data-ttu-id="f8608-487">與 ASP.NET Core 2.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="f8608-487">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="f8608-488">輸出規則</span><span class="sxs-lookup"><span data-stu-id="f8608-488">Outbound Rules</span></span>
* <span data-ttu-id="f8608-489">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f8608-489">Custom Server Variables</span></span>
* <span data-ttu-id="f8608-490">萬用字元</span><span class="sxs-lookup"><span data-stu-id="f8608-490">Wildcards</span></span>
* <span data-ttu-id="f8608-491">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="f8608-491">LogRewrittenUrl</span></span>

#### <a name="supported-server-variables"></a><span data-ttu-id="f8608-492">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f8608-492">Supported server variables</span></span>

<span data-ttu-id="f8608-493">中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="f8608-493">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="f8608-494">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="f8608-494">CONTENT_LENGTH</span></span>
* <span data-ttu-id="f8608-495">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="f8608-495">CONTENT_TYPE</span></span>
* <span data-ttu-id="f8608-496">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="f8608-496">HTTP_ACCEPT</span></span>
* <span data-ttu-id="f8608-497">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="f8608-497">HTTP_CONNECTION</span></span>
* <span data-ttu-id="f8608-498">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="f8608-498">HTTP_COOKIE</span></span>
* <span data-ttu-id="f8608-499">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="f8608-499">HTTP_HOST</span></span>
* <span data-ttu-id="f8608-500">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="f8608-500">HTTP_REFERER</span></span>
* <span data-ttu-id="f8608-501">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="f8608-501">HTTP_URL</span></span>
* <span data-ttu-id="f8608-502">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="f8608-502">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="f8608-503">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f8608-503">HTTPS</span></span>
* <span data-ttu-id="f8608-504">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-504">LOCAL_ADDR</span></span>
* <span data-ttu-id="f8608-505">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="f8608-505">QUERY_STRING</span></span>
* <span data-ttu-id="f8608-506">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-506">REMOTE_ADDR</span></span>
* <span data-ttu-id="f8608-507">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="f8608-507">REMOTE_PORT</span></span>
* <span data-ttu-id="f8608-508">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f8608-508">REQUEST_FILENAME</span></span>
* <span data-ttu-id="f8608-509">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="f8608-509">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="f8608-510">您也可以透過 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 取得 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="f8608-510">You can also obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> via a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span></span> <span data-ttu-id="f8608-511">這種方法可讓重寫規則檔案的位置更有彈性。</span><span class="sxs-lookup"><span data-stu-id="f8608-511">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="f8608-512">請確認重寫規則檔案已部署到您所提供之路徑的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f8608-512">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="f8608-513">以方法為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="f8608-513">Method-based rule</span></span>

<span data-ttu-id="f8608-514">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 以在方法中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f8608-514">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to implement your own rule logic in a method.</span></span> <span data-ttu-id="f8608-515">`Add` 會公開 <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>，使 <xref:Microsoft.AspNetCore.Http.HttpContext> 可用於您的方法中。</span><span class="sxs-lookup"><span data-stu-id="f8608-515">`Add` exposes the <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, which makes available the <xref:Microsoft.AspNetCore.Http.HttpContext> for use in your method.</span></span> <span data-ttu-id="f8608-516">[RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*)可判斷其他管線處理的執行方式。</span><span class="sxs-lookup"><span data-stu-id="f8608-516">The [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) determines how additional pipeline processing is handled.</span></span> <span data-ttu-id="f8608-517">請將值設定為下表中描述的其中一個 <xref:Microsoft.AspNetCore.Rewrite.RuleResult> 欄位。</span><span class="sxs-lookup"><span data-stu-id="f8608-517">Set the value to one of the <xref:Microsoft.AspNetCore.Rewrite.RuleResult> fields described in the following table.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="f8608-518">動作</span><span class="sxs-lookup"><span data-stu-id="f8608-518">Action</span></span>                                                           |
| ---
<span data-ttu-id="f8608-519">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-519">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-520">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-520">'Blazor'</span></span>
- <span data-ttu-id="f8608-521">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-521">'Identity'</span></span>
- <span data-ttu-id="f8608-522">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-522">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-523">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-523">'Razor'</span></span>
- <span data-ttu-id="f8608-524">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-524">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-525">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-525">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-526">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-526">'Blazor'</span></span>
- <span data-ttu-id="f8608-527">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-527">'Identity'</span></span>
- <span data-ttu-id="f8608-528">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-528">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-529">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-529">'Razor'</span></span>
- <span data-ttu-id="f8608-530">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-530">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-531">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-531">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-532">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-532">'Blazor'</span></span>
- <span data-ttu-id="f8608-533">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-533">'Identity'</span></span>
- <span data-ttu-id="f8608-534">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-534">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-535">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-535">'Razor'</span></span>
- <span data-ttu-id="f8608-536">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-536">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-537">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-537">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-538">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-538">'Blazor'</span></span>
- <span data-ttu-id="f8608-539">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-539">'Identity'</span></span>
- <span data-ttu-id="f8608-540">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-540">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-541">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-541">'Razor'</span></span>
- <span data-ttu-id="f8608-542">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-542">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-543">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-543">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-544">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-544">'Blazor'</span></span>
- <span data-ttu-id="f8608-545">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-545">'Identity'</span></span>
- <span data-ttu-id="f8608-546">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-546">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-547">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-547">'Razor'</span></span>
- <span data-ttu-id="f8608-548">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-548">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-549">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-549">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-550">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-550">'Blazor'</span></span>
- <span data-ttu-id="f8608-551">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-551">'Identity'</span></span>
- <span data-ttu-id="f8608-552">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-552">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-553">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-553">'Razor'</span></span>
- <span data-ttu-id="f8608-554">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-554">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-555">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-555">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-556">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-556">'Blazor'</span></span>
- <span data-ttu-id="f8608-557">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-557">'Identity'</span></span>
- <span data-ttu-id="f8608-558">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-558">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-559">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-559">'Razor'</span></span>
- <span data-ttu-id="f8608-560">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-560">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-561">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-561">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-562">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-562">'Blazor'</span></span>
- <span data-ttu-id="f8608-563">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-563">'Identity'</span></span>
- <span data-ttu-id="f8608-564">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-564">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-565">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-565">'Razor'</span></span>
- <span data-ttu-id="f8608-566">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-566">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-567">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-567">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-568">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-568">'Blazor'</span></span>
- <span data-ttu-id="f8608-569">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-569">'Identity'</span></span>
- <span data-ttu-id="f8608-570">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-570">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-571">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-571">'Razor'</span></span>
- <span data-ttu-id="f8608-572">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-572">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-573">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-573">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-574">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-574">'Blazor'</span></span>
- <span data-ttu-id="f8608-575">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-575">'Identity'</span></span>
- <span data-ttu-id="f8608-576">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-576">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-577">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-577">'Razor'</span></span>
- <span data-ttu-id="f8608-578">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-578">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-579">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-579">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-580">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-580">'Blazor'</span></span>
- <span data-ttu-id="f8608-581">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-581">'Identity'</span></span>
- <span data-ttu-id="f8608-582">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-582">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-583">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-583">'Razor'</span></span>
- <span data-ttu-id="f8608-584">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-584">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-585">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-585">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-586">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-586">'Blazor'</span></span>
- <span data-ttu-id="f8608-587">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-587">'Identity'</span></span>
- <span data-ttu-id="f8608-588">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-588">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-589">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-589">'Razor'</span></span>
- <span data-ttu-id="f8608-590">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-590">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-591">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-591">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-592">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-592">'Blazor'</span></span>
- <span data-ttu-id="f8608-593">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-593">'Identity'</span></span>
- <span data-ttu-id="f8608-594">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-594">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-595">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-595">'Razor'</span></span>
- <span data-ttu-id="f8608-596">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-596">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-597">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-597">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-598">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-598">'Blazor'</span></span>
- <span data-ttu-id="f8608-599">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-599">'Identity'</span></span>
- <span data-ttu-id="f8608-600">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-600">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-601">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-601">'Razor'</span></span>
- <span data-ttu-id="f8608-602">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-602">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-603">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-603">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-604">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-604">'Blazor'</span></span>
- <span data-ttu-id="f8608-605">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-605">'Identity'</span></span>
- <span data-ttu-id="f8608-606">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-606">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-607">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-607">'Razor'</span></span>
- <span data-ttu-id="f8608-608">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-608">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-609">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-609">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-610">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-610">'Blazor'</span></span>
- <span data-ttu-id="f8608-611">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-611">'Identity'</span></span>
- <span data-ttu-id="f8608-612">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-612">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-613">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-613">'Razor'</span></span>
- <span data-ttu-id="f8608-614">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-614">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-615">------------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-615">------------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-616">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-616">'Blazor'</span></span>
- <span data-ttu-id="f8608-617">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-617">'Identity'</span></span>
- <span data-ttu-id="f8608-618">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-618">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-619">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-619">'Razor'</span></span>
- <span data-ttu-id="f8608-620">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-620">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-621">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-621">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-622">'Blazor'</span></span>
- <span data-ttu-id="f8608-623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-623">'Identity'</span></span>
- <span data-ttu-id="f8608-624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-624">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-625">'Razor'</span></span>
- <span data-ttu-id="f8608-626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-626">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-627">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-627">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-628">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-628">'Blazor'</span></span>
- <span data-ttu-id="f8608-629">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-629">'Identity'</span></span>
- <span data-ttu-id="f8608-630">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-630">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-631">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-631">'Razor'</span></span>
- <span data-ttu-id="f8608-632">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-632">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-633">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-633">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-634">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-634">'Blazor'</span></span>
- <span data-ttu-id="f8608-635">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-635">'Identity'</span></span>
- <span data-ttu-id="f8608-636">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-636">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-637">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-637">'Razor'</span></span>
- <span data-ttu-id="f8608-638">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-638">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-639">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-639">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-640">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-640">'Blazor'</span></span>
- <span data-ttu-id="f8608-641">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-641">'Identity'</span></span>
- <span data-ttu-id="f8608-642">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-642">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-643">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-643">'Razor'</span></span>
- <span data-ttu-id="f8608-644">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-644">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-645">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-645">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-646">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-646">'Blazor'</span></span>
- <span data-ttu-id="f8608-647">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-647">'Identity'</span></span>
- <span data-ttu-id="f8608-648">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-648">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-649">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-649">'Razor'</span></span>
- <span data-ttu-id="f8608-650">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-650">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-651">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-651">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-652">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-652">'Blazor'</span></span>
- <span data-ttu-id="f8608-653">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-653">'Identity'</span></span>
- <span data-ttu-id="f8608-654">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-654">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-655">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-655">'Razor'</span></span>
- <span data-ttu-id="f8608-656">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-656">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-657">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-657">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-658">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-658">'Blazor'</span></span>
- <span data-ttu-id="f8608-659">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-659">'Identity'</span></span>
- <span data-ttu-id="f8608-660">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-660">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-661">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-661">'Razor'</span></span>
- <span data-ttu-id="f8608-662">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-662">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-663">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-663">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-664">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-664">'Blazor'</span></span>
- <span data-ttu-id="f8608-665">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-665">'Identity'</span></span>
- <span data-ttu-id="f8608-666">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-666">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-667">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-667">'Razor'</span></span>
- <span data-ttu-id="f8608-668">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-668">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-669">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-669">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-670">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-670">'Blazor'</span></span>
- <span data-ttu-id="f8608-671">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-671">'Identity'</span></span>
- <span data-ttu-id="f8608-672">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-672">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-673">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-673">'Razor'</span></span>
- <span data-ttu-id="f8608-674">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-674">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-675">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-675">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-676">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-676">'Blazor'</span></span>
- <span data-ttu-id="f8608-677">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-677">'Identity'</span></span>
- <span data-ttu-id="f8608-678">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-678">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-679">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-679">'Razor'</span></span>
- <span data-ttu-id="f8608-680">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-680">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-681">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-681">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-682">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-682">'Blazor'</span></span>
- <span data-ttu-id="f8608-683">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-683">'Identity'</span></span>
- <span data-ttu-id="f8608-684">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-684">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-685">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-685">'Razor'</span></span>
- <span data-ttu-id="f8608-686">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-686">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-687">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-687">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-688">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-688">'Blazor'</span></span>
- <span data-ttu-id="f8608-689">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-689">'Identity'</span></span>
- <span data-ttu-id="f8608-690">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-690">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-691">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-691">'Razor'</span></span>
- <span data-ttu-id="f8608-692">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-692">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-693">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-693">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-694">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-694">'Blazor'</span></span>
- <span data-ttu-id="f8608-695">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-695">'Identity'</span></span>
- <span data-ttu-id="f8608-696">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-696">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-697">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-697">'Razor'</span></span>
- <span data-ttu-id="f8608-698">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-698">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-699">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-699">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-700">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-700">'Blazor'</span></span>
- <span data-ttu-id="f8608-701">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-701">'Identity'</span></span>
- <span data-ttu-id="f8608-702">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-702">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-703">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-703">'Razor'</span></span>
- <span data-ttu-id="f8608-704">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-704">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-705">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-705">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-706">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-706">'Blazor'</span></span>
- <span data-ttu-id="f8608-707">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-707">'Identity'</span></span>
- <span data-ttu-id="f8608-708">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-708">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-709">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-709">'Razor'</span></span>
- <span data-ttu-id="f8608-710">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-710">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-711">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-711">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-712">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-712">'Blazor'</span></span>
- <span data-ttu-id="f8608-713">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-713">'Identity'</span></span>
- <span data-ttu-id="f8608-714">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-714">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-715">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-715">'Razor'</span></span>
- <span data-ttu-id="f8608-716">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-716">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-717">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-717">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-718">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-718">'Blazor'</span></span>
- <span data-ttu-id="f8608-719">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-719">'Identity'</span></span>
- <span data-ttu-id="f8608-720">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-720">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-721">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-721">'Razor'</span></span>
- <span data-ttu-id="f8608-722">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-722">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-723">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-723">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-724">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-724">'Blazor'</span></span>
- <span data-ttu-id="f8608-725">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-725">'Identity'</span></span>
- <span data-ttu-id="f8608-726">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-726">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-727">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-727">'Razor'</span></span>
- <span data-ttu-id="f8608-728">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-728">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-729">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-729">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-730">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-730">'Blazor'</span></span>
- <span data-ttu-id="f8608-731">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-731">'Identity'</span></span>
- <span data-ttu-id="f8608-732">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-732">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-733">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-733">'Razor'</span></span>
- <span data-ttu-id="f8608-734">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-734">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-735">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-735">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-736">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-736">'Blazor'</span></span>
- <span data-ttu-id="f8608-737">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-737">'Identity'</span></span>
- <span data-ttu-id="f8608-738">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-738">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-739">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-739">'Razor'</span></span>
- <span data-ttu-id="f8608-740">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-740">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-741">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-741">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-742">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-742">'Blazor'</span></span>
- <span data-ttu-id="f8608-743">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-743">'Identity'</span></span>
- <span data-ttu-id="f8608-744">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-744">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-745">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-745">'Razor'</span></span>
- <span data-ttu-id="f8608-746">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-746">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-747">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-747">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-748">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-748">'Blazor'</span></span>
- <span data-ttu-id="f8608-749">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-749">'Identity'</span></span>
- <span data-ttu-id="f8608-750">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-750">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-751">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-751">'Razor'</span></span>
- <span data-ttu-id="f8608-752">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-752">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-753">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-753">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-754">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-754">'Blazor'</span></span>
- <span data-ttu-id="f8608-755">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-755">'Identity'</span></span>
- <span data-ttu-id="f8608-756">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-756">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-757">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-757">'Razor'</span></span>
- <span data-ttu-id="f8608-758">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-758">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-759">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-759">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-760">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-760">'Blazor'</span></span>
- <span data-ttu-id="f8608-761">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-761">'Identity'</span></span>
- <span data-ttu-id="f8608-762">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-762">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-763">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-763">'Razor'</span></span>
- <span data-ttu-id="f8608-764">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-764">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-765">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-765">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-766">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-766">'Blazor'</span></span>
- <span data-ttu-id="f8608-767">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-767">'Identity'</span></span>
- <span data-ttu-id="f8608-768">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-768">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-769">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-769">'Razor'</span></span>
- <span data-ttu-id="f8608-770">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-770">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-771">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-771">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-772">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-772">'Blazor'</span></span>
- <span data-ttu-id="f8608-773">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-773">'Identity'</span></span>
- <span data-ttu-id="f8608-774">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-774">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-775">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-775">'Razor'</span></span>
- <span data-ttu-id="f8608-776">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-776">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-777">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-777">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-778">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-778">'Blazor'</span></span>
- <span data-ttu-id="f8608-779">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-779">'Identity'</span></span>
- <span data-ttu-id="f8608-780">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-780">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-781">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-781">'Razor'</span></span>
- <span data-ttu-id="f8608-782">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-782">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-783">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-783">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-784">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-784">'Blazor'</span></span>
- <span data-ttu-id="f8608-785">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-785">'Identity'</span></span>
- <span data-ttu-id="f8608-786">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-786">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-787">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-787">'Razor'</span></span>
- <span data-ttu-id="f8608-788">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-788">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-789">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-789">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-790">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-790">'Blazor'</span></span>
- <span data-ttu-id="f8608-791">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-791">'Identity'</span></span>
- <span data-ttu-id="f8608-792">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-792">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-793">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-793">'Razor'</span></span>
- <span data-ttu-id="f8608-794">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-794">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-795">-------------------------------- | |`RuleResult.ContinueRules`（預設） |繼續套用規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-795">-------------------------------- | | `RuleResult.ContinueRules` (default) | Continue applying rules.</span></span>                                         <span data-ttu-id="f8608-796">| |`RuleResult.EndResponse`             |停止套用規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="f8608-796">| | `RuleResult.EndResponse`             | Stop applying rules and send the response.</span></span>                       <span data-ttu-id="f8608-797">| |`RuleResult.SkipRemainingRules`      |停止套用規則，並將內容傳送至下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-797">| | `RuleResult.SkipRemainingRules`      | Stop applying rules and send the context to the next middleware.</span></span> |

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

<span data-ttu-id="f8608-798">範例應用程式會示範一個方法，以重新導向對 *.xml* 結尾之路徑的要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-798">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="f8608-799">若要求是針對 `/file.xml` 發出，則要求會重新導向至 `/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="f8608-799">If a request is made for `/file.xml`, the request is redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="f8608-800">狀態碼會設定為 301 (已永久移動)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-800">The status code is set to *301 - Moved Permanently*.</span></span> <span data-ttu-id="f8608-801">當瀏覽器針對 */xmlfiles/file.xml*發出新的要求時，靜態檔案中介軟體會從 *wwwroot/xmlfiles* 資料夾將檔案提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-801">When the browser makes a new request for */xmlfiles/file.xml*, Static File Middleware serves the file to the client from the *wwwroot/xmlfiles* folder.</span></span> <span data-ttu-id="f8608-802">若要重新導向，請明確設定回應的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-802">For a redirect, explicitly set the status code of the response.</span></span> <span data-ttu-id="f8608-803">否則會傳回 200 (確定)\*\* 狀態碼，用戶端上也不會發生重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-803">Otherwise, a *200 - OK* status code is returned, and the redirect doesn't occur on the client.</span></span>

<span data-ttu-id="f8608-804">*RewriteRules.cs*：</span><span class="sxs-lookup"><span data-stu-id="f8608-804">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

<span data-ttu-id="f8608-805">這種方法也能重寫要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-805">This approach can also rewrite requests.</span></span> <span data-ttu-id="f8608-806">範例應用程式示範了如何重寫任何文字檔要求的路徑，以從 *wwwroot* 資料夾提供 *file.txt* 文字檔。</span><span class="sxs-lookup"><span data-stu-id="f8608-806">The sample app demonstrates rewriting the path for any text file request to serve the *file.txt* text file from the *wwwroot* folder.</span></span> <span data-ttu-id="f8608-807">靜態檔案中介軟體會根據更新的要求路徑來提供檔案：</span><span class="sxs-lookup"><span data-stu-id="f8608-807">Static File Middleware serves the file based on the updated request path:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

<span data-ttu-id="f8608-808">*RewriteRules.cs*：</span><span class="sxs-lookup"><span data-stu-id="f8608-808">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a><span data-ttu-id="f8608-809">以 IRule 為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="f8608-809">IRule-based rule</span></span>

<span data-ttu-id="f8608-810">利用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 來使用類別中實作 <xref:Microsoft.AspNetCore.Rewrite.IRule> 介面的專屬規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f8608-810">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to use rule logic in a class that implements the <xref:Microsoft.AspNetCore.Rewrite.IRule> interface.</span></span> <span data-ttu-id="f8608-811">相較於使用以方法為基礎的規則方法，`IRule` 提供了更高彈性。</span><span class="sxs-lookup"><span data-stu-id="f8608-811">`IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="f8608-812">您的實作類別可以包含建構函式，以便您在其中傳入 <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> 方法的參數。</span><span class="sxs-lookup"><span data-stu-id="f8608-812">Your implementation class may include a constructor that allows you can pass in parameters for the <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> method.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="f8608-813">為了滿足若干條件，系統會檢查範例應用程式中的 `extension` 和 `newPath` 參數值。</span><span class="sxs-lookup"><span data-stu-id="f8608-813">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="f8608-814">`extension` 必須包含值，而且值必須是 *.png*、*.jpg* 或 *.gif*。</span><span class="sxs-lookup"><span data-stu-id="f8608-814">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="f8608-815">如果 `newPath` 無效，就會擲回 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="f8608-815">If the `newPath` isn't valid, an <xref:System.ArgumentException> is thrown.</span></span> <span data-ttu-id="f8608-816">若要求是針對 *image.png* 發出，則要求會重新導向至 `/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="f8608-816">If a request is made for *image.png*, the request is redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="f8608-817">若要求是針對 *image.jpg* 發出，則要求會重新導向至 `/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="f8608-817">If a request is made for *image.jpg*, the request is redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="f8608-818">狀態碼會設定為 301 (已永久移動)\*\*，而 `context.Result` 會設定為停止處理規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="f8608-818">The status code is set to *301 - Moved Permanently*, and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/samples/3.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

<span data-ttu-id="f8608-819">原始要求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="f8608-819">Original Request: `/image.png`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.png 的要求和回應](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="f8608-821">原始要求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="f8608-821">Original Request: `/image.jpg`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.jpg 的要求和回應](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="f8608-823">Regex 範例</span><span class="sxs-lookup"><span data-stu-id="f8608-823">Regex examples</span></span>

| <span data-ttu-id="f8608-824">目標</span><span class="sxs-lookup"><span data-stu-id="f8608-824">Goal</span></span> | <span data-ttu-id="f8608-825">Regex 字串及</span><span class="sxs-lookup"><span data-stu-id="f8608-825">Regex String &</span></span><br><span data-ttu-id="f8608-826">比對範例</span><span class="sxs-lookup"><span data-stu-id="f8608-826">Match Example</span></span> | <span data-ttu-id="f8608-827">取代字串及</span><span class="sxs-lookup"><span data-stu-id="f8608-827">Replacement String &</span></span><br><span data-ttu-id="f8608-828">輸出範例</span><span class="sxs-lookup"><span data-stu-id="f8608-828">Output Example</span></span> |
| ---- | ---
<span data-ttu-id="f8608-829">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-829">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-830">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-830">'Blazor'</span></span>
- <span data-ttu-id="f8608-831">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-831">'Identity'</span></span>
- <span data-ttu-id="f8608-832">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-832">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-833">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-833">'Razor'</span></span>
- <span data-ttu-id="f8608-834">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-834">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-835">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-835">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-836">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-836">'Blazor'</span></span>
- <span data-ttu-id="f8608-837">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-837">'Identity'</span></span>
- <span data-ttu-id="f8608-838">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-838">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-839">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-839">'Razor'</span></span>
- <span data-ttu-id="f8608-840">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-840">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-841">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-841">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-842">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-842">'Blazor'</span></span>
- <span data-ttu-id="f8608-843">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-843">'Identity'</span></span>
- <span data-ttu-id="f8608-844">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-844">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-845">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-845">'Razor'</span></span>
- <span data-ttu-id="f8608-846">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-846">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-847">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-847">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-848">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-848">'Blazor'</span></span>
- <span data-ttu-id="f8608-849">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-849">'Identity'</span></span>
- <span data-ttu-id="f8608-850">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-850">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-851">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-851">'Razor'</span></span>
- <span data-ttu-id="f8608-852">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-852">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-853">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-853">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-854">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-854">'Blazor'</span></span>
- <span data-ttu-id="f8608-855">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-855">'Identity'</span></span>
- <span data-ttu-id="f8608-856">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-856">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-857">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-857">'Razor'</span></span>
- <span data-ttu-id="f8608-858">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-858">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-859">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-859">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-860">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-860">'Blazor'</span></span>
- <span data-ttu-id="f8608-861">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-861">'Identity'</span></span>
- <span data-ttu-id="f8608-862">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-862">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-863">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-863">'Razor'</span></span>
- <span data-ttu-id="f8608-864">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-864">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-865">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-865">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-866">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-866">'Blazor'</span></span>
- <span data-ttu-id="f8608-867">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-867">'Identity'</span></span>
- <span data-ttu-id="f8608-868">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-868">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-869">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-869">'Razor'</span></span>
- <span data-ttu-id="f8608-870">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-870">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-871">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-871">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-872">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-872">'Blazor'</span></span>
- <span data-ttu-id="f8608-873">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-873">'Identity'</span></span>
- <span data-ttu-id="f8608-874">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-874">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-875">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-875">'Razor'</span></span>
- <span data-ttu-id="f8608-876">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-876">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-877">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-877">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-878">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-878">'Blazor'</span></span>
- <span data-ttu-id="f8608-879">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-879">'Identity'</span></span>
- <span data-ttu-id="f8608-880">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-880">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-881">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-881">'Razor'</span></span>
- <span data-ttu-id="f8608-882">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-882">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-883">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-883">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-884">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-884">'Blazor'</span></span>
- <span data-ttu-id="f8608-885">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-885">'Identity'</span></span>
- <span data-ttu-id="f8608-886">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-886">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-887">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-887">'Razor'</span></span>
- <span data-ttu-id="f8608-888">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-888">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-889">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-889">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-890">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-890">'Blazor'</span></span>
- <span data-ttu-id="f8608-891">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-891">'Identity'</span></span>
- <span data-ttu-id="f8608-892">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-892">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-893">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-893">'Razor'</span></span>
- <span data-ttu-id="f8608-894">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-894">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-895">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-895">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-896">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-896">'Blazor'</span></span>
- <span data-ttu-id="f8608-897">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-897">'Identity'</span></span>
- <span data-ttu-id="f8608-898">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-898">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-899">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-899">'Razor'</span></span>
- <span data-ttu-id="f8608-900">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-900">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-901">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-901">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-902">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-902">'Blazor'</span></span>
- <span data-ttu-id="f8608-903">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-903">'Identity'</span></span>
- <span data-ttu-id="f8608-904">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-904">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-905">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-905">'Razor'</span></span>
- <span data-ttu-id="f8608-906">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-906">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-907">---------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-907">---------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-908">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-908">'Blazor'</span></span>
- <span data-ttu-id="f8608-909">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-909">'Identity'</span></span>
- <span data-ttu-id="f8608-910">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-910">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-911">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-911">'Razor'</span></span>
- <span data-ttu-id="f8608-912">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-912">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-913">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-913">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-914">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-914">'Blazor'</span></span>
- <span data-ttu-id="f8608-915">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-915">'Identity'</span></span>
- <span data-ttu-id="f8608-916">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-916">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-917">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-917">'Razor'</span></span>
- <span data-ttu-id="f8608-918">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-918">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-919">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-919">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-920">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-920">'Blazor'</span></span>
- <span data-ttu-id="f8608-921">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-921">'Identity'</span></span>
- <span data-ttu-id="f8608-922">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-922">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-923">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-923">'Razor'</span></span>
- <span data-ttu-id="f8608-924">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-924">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-925">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-925">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-926">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-926">'Blazor'</span></span>
- <span data-ttu-id="f8608-927">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-927">'Identity'</span></span>
- <span data-ttu-id="f8608-928">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-928">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-929">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-929">'Razor'</span></span>
- <span data-ttu-id="f8608-930">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-930">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-931">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-931">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-932">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-932">'Blazor'</span></span>
- <span data-ttu-id="f8608-933">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-933">'Identity'</span></span>
- <span data-ttu-id="f8608-934">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-934">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-935">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-935">'Razor'</span></span>
- <span data-ttu-id="f8608-936">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-936">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-937">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-937">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-938">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-938">'Blazor'</span></span>
- <span data-ttu-id="f8608-939">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-939">'Identity'</span></span>
- <span data-ttu-id="f8608-940">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-940">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-941">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-941">'Razor'</span></span>
- <span data-ttu-id="f8608-942">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-942">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-943">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-943">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-944">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-944">'Blazor'</span></span>
- <span data-ttu-id="f8608-945">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-945">'Identity'</span></span>
- <span data-ttu-id="f8608-946">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-946">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-947">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-947">'Razor'</span></span>
- <span data-ttu-id="f8608-948">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-948">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-949">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-949">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-950">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-950">'Blazor'</span></span>
- <span data-ttu-id="f8608-951">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-951">'Identity'</span></span>
- <span data-ttu-id="f8608-952">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-952">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-953">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-953">'Razor'</span></span>
- <span data-ttu-id="f8608-954">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-954">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-955">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-955">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-956">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-956">'Blazor'</span></span>
- <span data-ttu-id="f8608-957">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-957">'Identity'</span></span>
- <span data-ttu-id="f8608-958">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-958">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-959">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-959">'Razor'</span></span>
- <span data-ttu-id="f8608-960">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-960">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-961">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-961">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-962">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-962">'Blazor'</span></span>
- <span data-ttu-id="f8608-963">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-963">'Identity'</span></span>
- <span data-ttu-id="f8608-964">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-964">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-965">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-965">'Razor'</span></span>
- <span data-ttu-id="f8608-966">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-966">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-967">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-967">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-968">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-968">'Blazor'</span></span>
- <span data-ttu-id="f8608-969">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-969">'Identity'</span></span>
- <span data-ttu-id="f8608-970">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-970">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-971">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-971">'Razor'</span></span>
- <span data-ttu-id="f8608-972">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-972">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-973">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-973">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-974">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-974">'Blazor'</span></span>
- <span data-ttu-id="f8608-975">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-975">'Identity'</span></span>
- <span data-ttu-id="f8608-976">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-976">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-977">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-977">'Razor'</span></span>
- <span data-ttu-id="f8608-978">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-978">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-979">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-979">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-980">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-980">'Blazor'</span></span>
- <span data-ttu-id="f8608-981">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-981">'Identity'</span></span>
- <span data-ttu-id="f8608-982">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-982">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-983">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-983">'Razor'</span></span>
- <span data-ttu-id="f8608-984">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-984">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-985">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-985">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-986">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-986">'Blazor'</span></span>
- <span data-ttu-id="f8608-987">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-987">'Identity'</span></span>
- <span data-ttu-id="f8608-988">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-988">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-989">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-989">'Razor'</span></span>
- <span data-ttu-id="f8608-990">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-990">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-991">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-991">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-992">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-992">'Blazor'</span></span>
- <span data-ttu-id="f8608-993">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-993">'Identity'</span></span>
- <span data-ttu-id="f8608-994">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-994">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-995">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-995">'Razor'</span></span>
- <span data-ttu-id="f8608-996">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-996">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-997">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-997">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-998">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-998">'Blazor'</span></span>
- <span data-ttu-id="f8608-999">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-999">'Identity'</span></span>
- <span data-ttu-id="f8608-1000">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1000">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1001">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1001">'Razor'</span></span>
- <span data-ttu-id="f8608-1002">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1002">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1003">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1003">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1004">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1004">'Blazor'</span></span>
- <span data-ttu-id="f8608-1005">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1005">'Identity'</span></span>
- <span data-ttu-id="f8608-1006">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1006">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1007">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1007">'Razor'</span></span>
- <span data-ttu-id="f8608-1008">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1008">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1009">------------------- | |將路徑重寫為 querystring |`^path/(.*)/(.*)`</span><span class="sxs-lookup"><span data-stu-id="f8608-1009">------------------- | | Rewrite path into querystring | `^path/(.*)/(.*)`</span></span><br>`/path/abc/123` | `path?var1=$1&var2=$2`<br><span data-ttu-id="f8608-1010">`/path?var1=abc&var2=123`| |去除尾端斜線 |`(.*)/$`</span><span class="sxs-lookup"><span data-stu-id="f8608-1010">`/path?var1=abc&var2=123` | | Strip trailing slash | `(.*)/$`</span></span><br>`/path/` | `$1`<br><span data-ttu-id="f8608-1011">`/path`| |強制使用尾端斜線 |`(.*[^/])$`</span><span class="sxs-lookup"><span data-stu-id="f8608-1011">`/path` | | Enforce trailing slash | `(.*[^/])$`</span></span><br>`/path` | `$1/`<br><span data-ttu-id="f8608-1012">`/path/`| |避免重寫特定要求 |`^(.*)(?<!\.axd)$`或`^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="f8608-1012">`/path/` | | Avoid rewriting specific requests | `^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="f8608-1013">是：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="f8608-1013">Yes: `/resource.htm`</span></span><br><span data-ttu-id="f8608-1014">不：`/resource.axd` | `rewritten/$1`</span><span class="sxs-lookup"><span data-stu-id="f8608-1014">No: `/resource.axd` | `rewritten/$1`</span></span><br>`/rewritten/resource.htm`<br><span data-ttu-id="f8608-1015">`/resource.axd`| |重新排列 URL 區段 |`path/(.*)/(.*)/(.*)`</span><span class="sxs-lookup"><span data-stu-id="f8608-1015">`/resource.axd` | | Rearrange URL segments | `path/(.*)/(.*)/(.*)`</span></span><br>`path/1/2/3` | `path/$3/$2/$1`<br><span data-ttu-id="f8608-1016">`path/3/2/1`| |取代 URL 區段 |`^(.*)/segment2/(.*)`</span><span class="sxs-lookup"><span data-stu-id="f8608-1016">`path/3/2/1` | | Replace a URL segment | `^(.*)/segment2/(.*)`</span></span><br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f8608-1017">本文件介紹 URL 重寫，並提供如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。</span><span class="sxs-lookup"><span data-stu-id="f8608-1017">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

<span data-ttu-id="f8608-1018">URL 重寫是指根據一或多個預先定義的規則來修改要求 URL 的動作。</span><span class="sxs-lookup"><span data-stu-id="f8608-1018">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="f8608-1019">URL 重寫會在資源位置和位址之間建立一個抽象層，讓位置和位址不那麼緊密連結。</span><span class="sxs-lookup"><span data-stu-id="f8608-1019">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses aren't tightly linked.</span></span> <span data-ttu-id="f8608-1020">下列幾種情況非常適合使用 URL 重寫：</span><span class="sxs-lookup"><span data-stu-id="f8608-1020">URL rewriting is valuable in several scenarios to:</span></span>

* <span data-ttu-id="f8608-1021">暫時或永久性移動/取代伺服器資源，並為這些資源保持穩定的定位器時。</span><span class="sxs-lookup"><span data-stu-id="f8608-1021">Move or replace server resources temporarily or permanently and maintain stable locators for those resources.</span></span>
* <span data-ttu-id="f8608-1022">跨不同應用程式或跨單一應用程式各區域來分割要求處理作業。</span><span class="sxs-lookup"><span data-stu-id="f8608-1022">Split request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="f8608-1023">移除、新增或重組傳入要求的 URL 區段。</span><span class="sxs-lookup"><span data-stu-id="f8608-1023">Remove, add, or reorganize URL segments on incoming requests.</span></span>
* <span data-ttu-id="f8608-1024">針對搜尋引擎最佳化 (SEO) 來將公開 URL 最佳化。</span><span class="sxs-lookup"><span data-stu-id="f8608-1024">Optimize public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="f8608-1025">允許使用易懂的公開 URL，來協助訪客預測要求資源所傳回的內容。</span><span class="sxs-lookup"><span data-stu-id="f8608-1025">Permit the use of friendly public URLs to help visitors predict the content returned by requesting a resource.</span></span>
* <span data-ttu-id="f8608-1026">將不安全的要求重新導向至安全的端點。</span><span class="sxs-lookup"><span data-stu-id="f8608-1026">Redirect insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="f8608-1027">防止直接連結，即外部網站透過將另一個網站的資產連結至本身的內容，來盜用該網站擁有的靜態資產。</span><span class="sxs-lookup"><span data-stu-id="f8608-1027">Prevent hotlinking, where an external site uses a hosted static asset on another site by linking the asset into its own content.</span></span>

> [!NOTE]
> <span data-ttu-id="f8608-1028">URL 重寫可能會降低應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="f8608-1028">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="f8608-1029">如果可行的話，請限制規則的數目與複雜程度。</span><span class="sxs-lookup"><span data-stu-id="f8608-1029">Where feasible, limit the number and complexity of rules.</span></span>

<span data-ttu-id="f8608-1030">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="f8608-1030">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="f8608-1031">URL 重新導向和 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f8608-1031">URL redirect and URL rewrite</span></span>

<span data-ttu-id="f8608-1032">乍看之下，「URL 重新導向」\*\* 和「URL 重寫」\*\* 在用語上的差異不太明顯，但卻對提供資源給用戶端方面有重要的影響。</span><span class="sxs-lookup"><span data-stu-id="f8608-1032">The difference in wording between *URL redirect* and *URL rewrite* is subtle but has important implications for providing resources to clients.</span></span> <span data-ttu-id="f8608-1033">ASP.NET Core 的 URL 重寫中介軟體可以同時滿足這兩種需求。</span><span class="sxs-lookup"><span data-stu-id="f8608-1033">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="f8608-1034">「URL 重新導向」\*\* 牽涉有關用戶端的作業，其會指示用戶端到其他位址存取資源，而非用戶端原本要求的位址。</span><span class="sxs-lookup"><span data-stu-id="f8608-1034">A *URL redirect* involves a client-side operation, where the client is instructed to access a resource at a different address than the client originally requested.</span></span> <span data-ttu-id="f8608-1035">這項作業需要伺服器的來回行程。</span><span class="sxs-lookup"><span data-stu-id="f8608-1035">This requires a round trip to the server.</span></span> <span data-ttu-id="f8608-1036">當用戶端提出新的資源要求時，系統會將重新導向 URL 傳回給用戶端，並顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f8608-1036">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span>

<span data-ttu-id="f8608-1037">若 `/resource` 重新導向\*\* 至 `/different-resource`，則伺服器會回應用戶端應前往 `/different-resource` 取得資源，並提供狀態碼，指出重新導向為暫時性或永久性。</span><span class="sxs-lookup"><span data-stu-id="f8608-1037">If `/resource` is *redirected* to `/different-resource`, the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span>

![伺服器上的 WebAPI 服務端點已暫時從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="f8608-1043">將要求重新導向至不同的 URL 時，可以透過指定具狀態的回應，來指出該重新導向為永久性或暫時性：</span><span class="sxs-lookup"><span data-stu-id="f8608-1043">When redirecting requests to a different URL, indicate whether the redirect is permanent or temporary by specifying the status code with the response:</span></span>

* <span data-ttu-id="f8608-1044">如果資源具有新的永久 URL，且您希望告知用戶端所有未來的資源要求皆應使用這個新 URL，請使用 301 (已永久移動)\*\* 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-1044">The *301 - Moved Permanently* status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="f8608-1045">用戶端收到 301 狀態碼時，可能會快取並重複使用回應。\*\*</span><span class="sxs-lookup"><span data-stu-id="f8608-1045">*The client may cache and reuse the response when a 301 status code is received.*</span></span>

* <span data-ttu-id="f8608-1046">若重新導向為暫時性或很有可能變更時，請使用 302 (已找到)\*\* 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-1046">The *302 - Found* status code is used where the redirection is temporary or generally subject to change.</span></span> <span data-ttu-id="f8608-1047">302 狀態碼會指示用戶端不要儲存 URL 並在之後使用。</span><span class="sxs-lookup"><span data-stu-id="f8608-1047">The 302 status code indicates to the client not to store the URL and use it in the future.</span></span>

<span data-ttu-id="f8608-1048">如需狀態碼的詳細資訊，請參閱 [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616：狀態碼定義)。</span><span class="sxs-lookup"><span data-stu-id="f8608-1048">For more information on status codes, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="f8608-1049">「URL 重寫」\*\* 伺服器端作業，會從用戶端要求以外的其他資源位址提供資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-1049">A *URL rewrite* is a server-side operation that provides a resource from a different resource address than the client requested.</span></span> <span data-ttu-id="f8608-1050">重寫 URL 並不需要伺服器的來回行程。</span><span class="sxs-lookup"><span data-stu-id="f8608-1050">Rewriting a URL doesn't require a round trip to the server.</span></span> <span data-ttu-id="f8608-1051">系統不會將重寫的 URL 傳回用戶端，也不會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f8608-1051">The rewritten URL isn't returned to the client and doesn't appear in the browser's address bar.</span></span>

<span data-ttu-id="f8608-1052">若 `/resource`「重寫」\*\* 至 `/different-resource`，則伺服器會於「內部」\*\* 擷取資源，並在 `/different-resource` 傳回資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-1052">If `/resource` is *rewritten* to `/different-resource`, the server *internally* fetches and returns the resource at `/different-resource`.</span></span>

<span data-ttu-id="f8608-1053">雖然用戶端或許可以在重寫的 URL 處擷取資源，但用戶端並不會在提出要求與接收回應時得知資源位於重寫的 URL 處。</span><span class="sxs-lookup"><span data-stu-id="f8608-1053">Although the client might be able to retrieve the resource at the rewritten URL, the client isn't informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![伺服器上的 WebAPI 服務端點已從第 1 版 (v1) 變更為第 2 版 (v2)。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="f8608-1058">URL 重寫範例應用程式</span><span class="sxs-lookup"><span data-stu-id="f8608-1058">URL rewriting sample app</span></span>

<span data-ttu-id="f8608-1059">您可以利用[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)，來探索 URL 重寫中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="f8608-1059">You can explore the features of the URL Rewriting Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="f8608-1060">範例應用程式會套用重新導向與重寫規則，還會示範多種情況下的重新導向或重寫 URL。</span><span class="sxs-lookup"><span data-stu-id="f8608-1060">The app applies redirect and rewrite rules and shows the redirected or rewritten URL for several scenarios.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="f8608-1061">URL 重寫中介軟體的使用時機</span><span class="sxs-lookup"><span data-stu-id="f8608-1061">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="f8608-1062">當您無法使用以下方法時，請使用重寫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f8608-1062">Use URL Rewriting Middleware when you're unable to use the following approaches:</span></span>

* [<span data-ttu-id="f8608-1063">Windows Server 上的 IIS URL Rewrite module</span><span class="sxs-lookup"><span data-stu-id="f8608-1063">URL Rewrite module with IIS on Windows Server</span></span>](https://www.iis.net/downloads/microsoft/url-rewrite)
* [<span data-ttu-id="f8608-1064">Apache Server 上的 Apache mod_rewrite 模組</span><span class="sxs-lookup"><span data-stu-id="f8608-1064">Apache mod_rewrite module on Apache Server</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="f8608-1065">Nginx 上的 URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f8608-1065">URL rewriting on Nginx</span></span>](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<span data-ttu-id="f8608-1066">此外也請在應用程式裝載於 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (之前稱為 WebListener) 時使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-1066">Also, use the middleware when the app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener).</span></span>

<span data-ttu-id="f8608-1067">使用 IIS、Apache 和 Nginx 所提供伺服器型 URL 重寫技術的主要原因包括：</span><span class="sxs-lookup"><span data-stu-id="f8608-1067">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, and Nginx are:</span></span>

* <span data-ttu-id="f8608-1068">中介軟體不支援這些模組的完整功能。</span><span class="sxs-lookup"><span data-stu-id="f8608-1068">The middleware doesn't support the full features of these modules.</span></span>

  <span data-ttu-id="f8608-1069">有部分伺服器模組的功能不適用於 ASP.NET Core 專案，例如 IIS Rewrite Module 規則的 `IsFile` 和 `IsDirectory` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="f8608-1069">Some of the features of the server modules don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="f8608-1070">在這些情況下，請改為使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-1070">In these scenarios, use the middleware instead.</span></span>
* <span data-ttu-id="f8608-1071">中介軟體的效能可能比不上模組的效能時。</span><span class="sxs-lookup"><span data-stu-id="f8608-1071">The performance of the middleware probably doesn't match that of the modules.</span></span>

  <span data-ttu-id="f8608-1072">如果要確實得知哪種方法會降低最多效能，或是降低的效能可以忽略的話，進行效能評定是唯一方法。</span><span class="sxs-lookup"><span data-stu-id="f8608-1072">Benchmarking is the only way to know for sure which approach degrades performance the most or if degraded performance is negligible.</span></span>

## <a name="package"></a><span data-ttu-id="f8608-1073">套件</span><span class="sxs-lookup"><span data-stu-id="f8608-1073">Package</span></span>

<span data-ttu-id="f8608-1074">若要在您的專案中包含中介軟體，請在包含 [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) 套件的專案檔中，將套件參考新增至 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="f8608-1074">To include the middleware in your project, add a package reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the project file, which contains the [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) package.</span></span>

<span data-ttu-id="f8608-1075">不使用 `Microsoft.AspNetCore.App` 中繼套件時，請將專案參考新增至 `Microsoft.AspNetCore.Rewrite` 套件。</span><span class="sxs-lookup"><span data-stu-id="f8608-1075">When not using the `Microsoft.AspNetCore.App` metapackage, add a project reference to the `Microsoft.AspNetCore.Rewrite` package.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="f8608-1076">延伸模組和選項</span><span class="sxs-lookup"><span data-stu-id="f8608-1076">Extension and options</span></span>

<span data-ttu-id="f8608-1077">若要建立 URL 重寫和重新導向規則，您可以針對每個重寫規則的擴充方法建立 [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f8608-1077">Establish URL rewrite and redirect rules by creating an instance of the [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) class with extension methods for each of your rewrite rules.</span></span> <span data-ttu-id="f8608-1078">依據所需的處理順序來鏈結多個規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1078">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="f8608-1079">系統會使用 <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*> 將 `RewriteOptions` 新增至要求管線，並將其傳入 URL 重寫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f8608-1079">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="f8608-1080">將非 www 重新導向 www</span><span class="sxs-lookup"><span data-stu-id="f8608-1080">Redirect non-www to www</span></span>

<span data-ttu-id="f8608-1081">有三個選項可讓應用程式將非 `www` 要求重新導向 `www`：</span><span class="sxs-lookup"><span data-stu-id="f8608-1081">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="f8608-1082"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*>：如果要求為非，則將要求永久重新導向至 `www` 子域 `www` 。</span><span class="sxs-lookup"><span data-stu-id="f8608-1082"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*>: Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="f8608-1083">使用 [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-1083">Redirects with a [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) status code.</span></span>

* <span data-ttu-id="f8608-1084"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*>： `www` 如果連入要求為非，則將要求重新導向至子域 `www` 。</span><span class="sxs-lookup"><span data-stu-id="f8608-1084"><xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*>: Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="f8608-1085">使用 [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) 狀態代碼重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-1085">Redirects with a [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) status code.</span></span> <span data-ttu-id="f8608-1086">多載可讓您提供回應的狀態代碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-1086">An overload permits you to provide the status code for the response.</span></span> <span data-ttu-id="f8608-1087">請使用 <xref:Microsoft.AspNetCore.Http.StatusCodes> 類別的欄位來進行狀態碼指派。</span><span class="sxs-lookup"><span data-stu-id="f8608-1087">Use a field of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for a status code assignment.</span></span>

### <a name="url-redirect"></a><span data-ttu-id="f8608-1088">URL 重新導向</span><span class="sxs-lookup"><span data-stu-id="f8608-1088">URL redirect</span></span>

<span data-ttu-id="f8608-1089">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> 將要求重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-1089">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> to redirect requests.</span></span> <span data-ttu-id="f8608-1090">第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-1090">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="f8608-1091">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="f8608-1091">The second parameter is the replacement string.</span></span> <span data-ttu-id="f8608-1092">第三個參數 (如果有的話) 會指定狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-1092">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="f8608-1093">如果您未指定狀態碼，則狀態碼會預設為 302 (已找到)\*\*，表示已暫時移動或取代資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-1093">If you don't specify the status code, the status code defaults to *302 - Found*, which indicates that the resource is temporarily moved or replaced.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="f8608-1094">在啟用開發人員工具的瀏覽器中，使用 `/redirect-rule/1234/5678` 路徑提出範例應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-1094">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="f8608-1095">Regex 會比對 `redirect-rule/(.*)` 上的要求路徑，並會將路徑取代為 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1095">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="f8608-1096">系統會將重新導向 URL 與 302 (已找到)\*\* 狀態碼傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-1096">The redirect URL is sent back to the client with a *302 - Found* status code.</span></span> <span data-ttu-id="f8608-1097">瀏覽器在重新導向 URL 處提出新要求，該 URL 也會顯示在瀏覽器的網址列中。</span><span class="sxs-lookup"><span data-stu-id="f8608-1097">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="f8608-1098">因為範例應用程式與重新導向 URL 沒有任何相符規則：</span><span class="sxs-lookup"><span data-stu-id="f8608-1098">Since no rules in the sample app match on the redirect URL:</span></span>

* <span data-ttu-id="f8608-1099">第二個要求會從應用程式收到 200 (確定)\*\* 回應。</span><span class="sxs-lookup"><span data-stu-id="f8608-1099">The second request receives a *200 - OK* response from the app.</span></span>
* <span data-ttu-id="f8608-1100">回應的本文會顯示重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="f8608-1100">The body of the response shows the redirect URL.</span></span>

<span data-ttu-id="f8608-1101">「重新導向」\*\* URL 時，會對伺服器進行來回行程。</span><span class="sxs-lookup"><span data-stu-id="f8608-1101">A round trip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="f8608-1102">建立重新導向規則時，請務必謹慎。</span><span class="sxs-lookup"><span data-stu-id="f8608-1102">Be cautious when establishing redirect rules.</span></span> <span data-ttu-id="f8608-1103">每向應用程式提出要求時 (包括重新導向後)，請評估您的重新導向規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1103">Redirect rules are evaluated on every request to the app, including after a redirect.</span></span> <span data-ttu-id="f8608-1104">因為您有可能會不小心建立無限重新導向迴圈\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1104">It's easy to accidentally create a *loop of infinite redirects*.</span></span>

<span data-ttu-id="f8608-1105">原始要求：`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="f8608-1105">Original Request: `/redirect-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="f8608-1107">括弧內所含的運算式部分稱為「擷取群組」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1107">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="f8608-1108">運算式的點 (`.`) 表示「比對任何字元」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1108">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="f8608-1109">星號 (`*`) 表示「比對前置字元零或多次」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1109">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="f8608-1110">因此，擷取群組 `(.*)` 會擷取 URL 的 `1234/5678` 最後這兩個路徑區段。</span><span class="sxs-lookup"><span data-stu-id="f8608-1110">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="f8608-1111">這個單一擷取群組會擷取您在要求 URL 中的 `redirect-rule/` 之後所提供的任何值。</span><span class="sxs-lookup"><span data-stu-id="f8608-1111">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="f8608-1112">系統會將擷取的群組以貨幣符號 (`$`) 後接擷取序號的形式，插入取代字串中。</span><span class="sxs-lookup"><span data-stu-id="f8608-1112">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="f8608-1113">使用 `$1` 可取得第一個擷取群組值、使用 `$2` 則會取得第二個，依您的 Regex 擷取群組順序以此類推。</span><span class="sxs-lookup"><span data-stu-id="f8608-1113">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="f8608-1114">在範例應用程式中，重新導向規則 Regex 只有一個擷取的群組，因此在取代字串中只有 `$1` 這一個插入的群組。</span><span class="sxs-lookup"><span data-stu-id="f8608-1114">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="f8608-1115">套用規則時，URL 會變成 `/redirected/1234/5678`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1115">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="f8608-1116">將 URL 重新導向至安全端點</span><span class="sxs-lookup"><span data-stu-id="f8608-1116">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="f8608-1117">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> 將 HTTP 要求重新導向至使用 HTTPS 通訊協定的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-1117">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> to redirect HTTP requests to the same host and path using the HTTPS protocol.</span></span> <span data-ttu-id="f8608-1118">如果未提供狀態碼，中介軟體會預設為 302 (已找到)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1118">If the status code isn't supplied, the middleware defaults to *302 - Found*.</span></span> <span data-ttu-id="f8608-1119">若未提供連接埠：</span><span class="sxs-lookup"><span data-stu-id="f8608-1119">If the port isn't supplied:</span></span>

* <span data-ttu-id="f8608-1120">中介軟體會預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1120">The middleware defaults to `null`.</span></span>
* <span data-ttu-id="f8608-1121">配置會變更為 `https` (HTTPS 通訊協定)，且用戶端會在連接埠 443 上存取資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-1121">The scheme changes to `https` (HTTPS protocol), and the client accesses the resource on port 443.</span></span>

<span data-ttu-id="f8608-1122">下列範例示範了如何將狀態碼設為 301 (已永久移動)\*\*，並將連接埠變更為 5001。</span><span class="sxs-lookup"><span data-stu-id="f8608-1122">The following example shows how to set the status code to *301 - Moved Permanently* and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="f8608-1123">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> 將不安全的要求重新導向至使用安全 HTTPS 通訊協定 (在連接埠 443 上) 的相同主機與路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-1123">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> to redirect insecure requests to the same host and path with secure HTTPS protocol on port 443.</span></span> <span data-ttu-id="f8608-1124">中介軟體會將狀態碼設定為 301 (已永久移動)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1124">The middleware sets the status code to *301 - Moved Permanently*.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="f8608-1125">在不要求額外重新導向規則的情況下重新導向至安全的端點時，建議使用 HTTPS 重新導向中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-1125">When redirecting to a secure endpoint without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="f8608-1126">如需詳細資訊，請參閱[強制執行 HTTPS](xref:security/enforcing-ssl#require-https) 主題。</span><span class="sxs-lookup"><span data-stu-id="f8608-1126">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="f8608-1127">範例應用程式可以示範如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1127">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="f8608-1128">將擴充方法新增至 `RewriteOptions`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1128">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="f8608-1129">在任何 URL 位置，向應用程式提出不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-1129">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="f8608-1130">關閉瀏覽器自我簽署憑證不受信任的安全性警告，或建立信任憑證的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f8608-1130">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="f8608-1131">使用 `AddRedirectToHttps(301, 5001)` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="f8608-1131">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="f8608-1133">使用 `AddRedirectToHttpsPermanent` 的原始要求：`http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="f8608-1133">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="f8608-1135">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="f8608-1135">URL rewrite</span></span>

<span data-ttu-id="f8608-1136">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> 來建立 URL 重寫的規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1136">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> to create a rule for rewriting URLs.</span></span> <span data-ttu-id="f8608-1137">第一個參數會包含 Regex，以比對傳入的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-1137">The first parameter contains the regex for matching on the incoming URL path.</span></span> <span data-ttu-id="f8608-1138">第二個參數是取代字串。</span><span class="sxs-lookup"><span data-stu-id="f8608-1138">The second parameter is the replacement string.</span></span> <span data-ttu-id="f8608-1139">第三個參數 `skipRemainingRules: {true|false}` 指出中介軟體是否要在套用目前規則時略過其他重寫規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1139">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

<span data-ttu-id="f8608-1140">原始要求：`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="f8608-1140">Original Request: `/rewrite-rule/1234/5678`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="f8608-1142">運算式開頭的插入號 (`^`) 表示，比對會從 URL 路徑的開頭開始。</span><span class="sxs-lookup"><span data-stu-id="f8608-1142">The carat (`^`) at the beginning of the expression means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="f8608-1143">在先前的重新導向規則範例 `redirect-rule/(.*)` 中，Regex 的開頭沒有插入號 (`^`)。</span><span class="sxs-lookup"><span data-stu-id="f8608-1143">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat (`^`) at the start of the regex.</span></span> <span data-ttu-id="f8608-1144">因此，就算 `redirect-rule/` 前有任何字元也能成功比對。</span><span class="sxs-lookup"><span data-stu-id="f8608-1144">Therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="f8608-1145">路徑</span><span class="sxs-lookup"><span data-stu-id="f8608-1145">Path</span></span>                               | <span data-ttu-id="f8608-1146">比對</span><span class="sxs-lookup"><span data-stu-id="f8608-1146">Match</span></span> |
| ---
<span data-ttu-id="f8608-1147">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1147">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1148">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1148">'Blazor'</span></span>
- <span data-ttu-id="f8608-1149">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1149">'Identity'</span></span>
- <span data-ttu-id="f8608-1150">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1150">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1151">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1151">'Razor'</span></span>
- <span data-ttu-id="f8608-1152">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1152">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1153">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1153">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1154">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1154">'Blazor'</span></span>
- <span data-ttu-id="f8608-1155">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1155">'Identity'</span></span>
- <span data-ttu-id="f8608-1156">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1156">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1157">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1157">'Razor'</span></span>
- <span data-ttu-id="f8608-1158">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1158">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1159">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1159">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1160">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1160">'Blazor'</span></span>
- <span data-ttu-id="f8608-1161">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1161">'Identity'</span></span>
- <span data-ttu-id="f8608-1162">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1162">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1163">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1163">'Razor'</span></span>
- <span data-ttu-id="f8608-1164">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1164">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1165">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1165">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1166">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1166">'Blazor'</span></span>
- <span data-ttu-id="f8608-1167">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1167">'Identity'</span></span>
- <span data-ttu-id="f8608-1168">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1168">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1169">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1169">'Razor'</span></span>
- <span data-ttu-id="f8608-1170">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1170">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1171">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1171">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1172">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1172">'Blazor'</span></span>
- <span data-ttu-id="f8608-1173">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1173">'Identity'</span></span>
- <span data-ttu-id="f8608-1174">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1174">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1175">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1175">'Razor'</span></span>
- <span data-ttu-id="f8608-1176">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1176">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1177">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1177">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1178">'Blazor'</span></span>
- <span data-ttu-id="f8608-1179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1179">'Identity'</span></span>
- <span data-ttu-id="f8608-1180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1180">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1181">'Razor'</span></span>
- <span data-ttu-id="f8608-1182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1182">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1183">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1183">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1184">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1184">'Blazor'</span></span>
- <span data-ttu-id="f8608-1185">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1185">'Identity'</span></span>
- <span data-ttu-id="f8608-1186">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1186">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1187">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1187">'Razor'</span></span>
- <span data-ttu-id="f8608-1188">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1188">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1189">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1189">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1190">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1190">'Blazor'</span></span>
- <span data-ttu-id="f8608-1191">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1191">'Identity'</span></span>
- <span data-ttu-id="f8608-1192">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1192">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1193">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1193">'Razor'</span></span>
- <span data-ttu-id="f8608-1194">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1194">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1195">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1195">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1196">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1196">'Blazor'</span></span>
- <span data-ttu-id="f8608-1197">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1197">'Identity'</span></span>
- <span data-ttu-id="f8608-1198">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1198">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1199">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1199">'Razor'</span></span>
- <span data-ttu-id="f8608-1200">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1200">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1201">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1201">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1202">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1202">'Blazor'</span></span>
- <span data-ttu-id="f8608-1203">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1203">'Identity'</span></span>
- <span data-ttu-id="f8608-1204">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1204">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1205">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1205">'Razor'</span></span>
- <span data-ttu-id="f8608-1206">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1206">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1207">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1207">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1208">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1208">'Blazor'</span></span>
- <span data-ttu-id="f8608-1209">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1209">'Identity'</span></span>
- <span data-ttu-id="f8608-1210">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1210">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1211">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1211">'Razor'</span></span>
- <span data-ttu-id="f8608-1212">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1212">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1213">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1213">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1214">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1214">'Blazor'</span></span>
- <span data-ttu-id="f8608-1215">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1215">'Identity'</span></span>
- <span data-ttu-id="f8608-1216">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1216">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1217">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1217">'Razor'</span></span>
- <span data-ttu-id="f8608-1218">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1218">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1219">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1219">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1220">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1220">'Blazor'</span></span>
- <span data-ttu-id="f8608-1221">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1221">'Identity'</span></span>
- <span data-ttu-id="f8608-1222">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1222">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1223">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1223">'Razor'</span></span>
- <span data-ttu-id="f8608-1224">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1224">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1225">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1225">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1226">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1226">'Blazor'</span></span>
- <span data-ttu-id="f8608-1227">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1227">'Identity'</span></span>
- <span data-ttu-id="f8608-1228">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1228">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1229">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1229">'Razor'</span></span>
- <span data-ttu-id="f8608-1230">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1230">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1231">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1231">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1232">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1232">'Blazor'</span></span>
- <span data-ttu-id="f8608-1233">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1233">'Identity'</span></span>
- <span data-ttu-id="f8608-1234">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1234">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1235">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1235">'Razor'</span></span>
- <span data-ttu-id="f8608-1236">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1236">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1237">----------------- |:---: || `/redirect-rule/1234/5678`         |是 || `/my-cool-redirect-rule/1234/5678` |是 || `/anotherredirect-rule/1234/5678`  |是 |</span><span class="sxs-lookup"><span data-stu-id="f8608-1237">----------------- | :---: | | `/redirect-rule/1234/5678`         | Yes   | | `/my-cool-redirect-rule/1234/5678` | Yes   | | `/anotherredirect-rule/1234/5678`  | Yes   |</span></span>

<span data-ttu-id="f8608-1238">`^rewrite-rule/(\d+)/(\d+)` 重寫規則只會比對開頭為 `rewrite-rule/` 的路徑。</span><span class="sxs-lookup"><span data-stu-id="f8608-1238">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="f8608-1239">請注意下表中的比對差異。</span><span class="sxs-lookup"><span data-stu-id="f8608-1239">In the following table, note the difference in matching.</span></span>

| <span data-ttu-id="f8608-1240">路徑</span><span class="sxs-lookup"><span data-stu-id="f8608-1240">Path</span></span>                              | <span data-ttu-id="f8608-1241">比對</span><span class="sxs-lookup"><span data-stu-id="f8608-1241">Match</span></span> |
| ---
<span data-ttu-id="f8608-1242">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1242">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1243">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1243">'Blazor'</span></span>
- <span data-ttu-id="f8608-1244">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1244">'Identity'</span></span>
- <span data-ttu-id="f8608-1245">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1245">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1246">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1246">'Razor'</span></span>
- <span data-ttu-id="f8608-1247">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1247">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1248">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1248">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1249">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1249">'Blazor'</span></span>
- <span data-ttu-id="f8608-1250">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1250">'Identity'</span></span>
- <span data-ttu-id="f8608-1251">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1251">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1252">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1252">'Razor'</span></span>
- <span data-ttu-id="f8608-1253">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1253">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1254">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1254">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1255">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1255">'Blazor'</span></span>
- <span data-ttu-id="f8608-1256">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1256">'Identity'</span></span>
- <span data-ttu-id="f8608-1257">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1257">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1258">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1258">'Razor'</span></span>
- <span data-ttu-id="f8608-1259">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1259">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1260">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1260">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1261">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1261">'Blazor'</span></span>
- <span data-ttu-id="f8608-1262">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1262">'Identity'</span></span>
- <span data-ttu-id="f8608-1263">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1263">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1264">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1264">'Razor'</span></span>
- <span data-ttu-id="f8608-1265">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1265">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1266">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1266">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1267">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1267">'Blazor'</span></span>
- <span data-ttu-id="f8608-1268">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1268">'Identity'</span></span>
- <span data-ttu-id="f8608-1269">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1269">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1270">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1270">'Razor'</span></span>
- <span data-ttu-id="f8608-1271">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1271">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1272">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1272">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1273">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1273">'Blazor'</span></span>
- <span data-ttu-id="f8608-1274">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1274">'Identity'</span></span>
- <span data-ttu-id="f8608-1275">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1275">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1276">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1276">'Razor'</span></span>
- <span data-ttu-id="f8608-1277">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1277">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1278">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1278">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1279">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1279">'Blazor'</span></span>
- <span data-ttu-id="f8608-1280">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1280">'Identity'</span></span>
- <span data-ttu-id="f8608-1281">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1281">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1282">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1282">'Razor'</span></span>
- <span data-ttu-id="f8608-1283">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1283">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1284">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1284">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1285">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1285">'Blazor'</span></span>
- <span data-ttu-id="f8608-1286">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1286">'Identity'</span></span>
- <span data-ttu-id="f8608-1287">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1287">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1288">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1288">'Razor'</span></span>
- <span data-ttu-id="f8608-1289">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1289">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1290">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1290">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1291">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1291">'Blazor'</span></span>
- <span data-ttu-id="f8608-1292">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1292">'Identity'</span></span>
- <span data-ttu-id="f8608-1293">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1293">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1294">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1294">'Razor'</span></span>
- <span data-ttu-id="f8608-1295">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1295">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1296">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1296">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1297">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1297">'Blazor'</span></span>
- <span data-ttu-id="f8608-1298">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1298">'Identity'</span></span>
- <span data-ttu-id="f8608-1299">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1299">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1300">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1300">'Razor'</span></span>
- <span data-ttu-id="f8608-1301">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1301">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1302">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1302">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1303">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1303">'Blazor'</span></span>
- <span data-ttu-id="f8608-1304">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1304">'Identity'</span></span>
- <span data-ttu-id="f8608-1305">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1305">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1306">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1306">'Razor'</span></span>
- <span data-ttu-id="f8608-1307">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1307">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1308">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1308">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1309">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1309">'Blazor'</span></span>
- <span data-ttu-id="f8608-1310">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1310">'Identity'</span></span>
- <span data-ttu-id="f8608-1311">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1311">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1312">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1312">'Razor'</span></span>
- <span data-ttu-id="f8608-1313">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1313">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1314">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1314">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1315">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1315">'Blazor'</span></span>
- <span data-ttu-id="f8608-1316">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1316">'Identity'</span></span>
- <span data-ttu-id="f8608-1317">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1317">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1318">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1318">'Razor'</span></span>
- <span data-ttu-id="f8608-1319">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1319">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1320">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1320">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1321">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1321">'Blazor'</span></span>
- <span data-ttu-id="f8608-1322">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1322">'Identity'</span></span>
- <span data-ttu-id="f8608-1323">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1323">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1324">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1324">'Razor'</span></span>
- <span data-ttu-id="f8608-1325">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1325">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1326">----------------- |:---: || `/rewrite-rule/1234/5678`         |是 || `/my-cool-rewrite-rule/1234/5678` |否 || `/anotherrewrite-rule/1234/5678`  |否 |</span><span class="sxs-lookup"><span data-stu-id="f8608-1326">----------------- | :---: | | `/rewrite-rule/1234/5678`         | Yes   | | `/my-cool-rewrite-rule/1234/5678` | No    | | `/anotherrewrite-rule/1234/5678`  | No    |</span></span>

<span data-ttu-id="f8608-1327">運算式的 `^rewrite-rule/` 部分之後，有 `(\d+)/(\d+)` 這兩個擷取群組。</span><span class="sxs-lookup"><span data-stu-id="f8608-1327">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="f8608-1328">`\d` 表示「比對數字」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1328">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="f8608-1329">加號 (`+`) 表示「比對一或多個前置字元」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1329">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="f8608-1330">因此，URL 必須包含某個數字，後接斜線與另一個數字。</span><span class="sxs-lookup"><span data-stu-id="f8608-1330">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="f8608-1331">這些擷取群組會以 `$1` 和 `$2` 形式插入重寫的 URL。</span><span class="sxs-lookup"><span data-stu-id="f8608-1331">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="f8608-1332">重寫規則的取代字串會將擷取的群組放入查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="f8608-1332">The rewrite rule replacement string places the captured groups into the query string.</span></span> <span data-ttu-id="f8608-1333">系統會重寫 `/rewrite-rule/1234/5678` 的要求路徑，以取得位於 `/rewritten?var1=1234&var2=5678` 的資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-1333">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="f8608-1334">如果原始要求上有查詢字串，則會在重寫 URL 時予以保留。</span><span class="sxs-lookup"><span data-stu-id="f8608-1334">If a query string is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="f8608-1335">這麼做就不需要伺服器的來回行程，即可取得資源。</span><span class="sxs-lookup"><span data-stu-id="f8608-1335">There's no round trip to the server to obtain the resource.</span></span> <span data-ttu-id="f8608-1336">如果資源存在，就會擷取該資源，並傳回 200 (確定)\*\* 狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-1336">If the resource exists, it's fetched and returned to the client with a *200 - OK* status code.</span></span> <span data-ttu-id="f8608-1337">因為系統並未重新導向用戶端，因此瀏覽器網址列中的 URL 不會變更。</span><span class="sxs-lookup"><span data-stu-id="f8608-1337">Because the client isn't redirected, the URL in the browser's address bar doesn't change.</span></span> <span data-ttu-id="f8608-1338">用戶端也無法偵測到伺服器上發生 URL 重寫作業。</span><span class="sxs-lookup"><span data-stu-id="f8608-1338">Clients can't detect that a URL rewrite operation occurred on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="f8608-1339">因為比對規則是計算繁複的程序，且會增加應用程式的回應時間，所以請盡可能使用 `skipRemainingRules: true`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1339">Use `skipRemainingRules: true` whenever possible because matching rules is computationally expensive and increases app response time.</span></span> <span data-ttu-id="f8608-1340">如需最快速的應用程式回應：</span><span class="sxs-lookup"><span data-stu-id="f8608-1340">For the fastest app response:</span></span>
>
> * <span data-ttu-id="f8608-1341">排序重寫規則；從最常比對的規則排到最不常比對的規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1341">Order rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="f8608-1342">當出現符合項目且不需要任何額外的規則處理時，即略過剩下的規則處理。</span><span class="sxs-lookup"><span data-stu-id="f8608-1342">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-mod_rewrite"></a><span data-ttu-id="f8608-1343">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="f8608-1343">Apache mod_rewrite</span></span>

<span data-ttu-id="f8608-1344">使用 <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*> 來套用 Apache mod_rewrite 規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1344">Apply Apache mod_rewrite rules with <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>.</span></span> <span data-ttu-id="f8608-1345">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="f8608-1345">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="f8608-1346">如需 mod_rewrite 規則的詳細資訊和範例，請參閱 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。</span><span class="sxs-lookup"><span data-stu-id="f8608-1346">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

<span data-ttu-id="f8608-1347">系統會使用 <xref:System.IO.StreamReader> 來讀取 *ApacheModRewrite.txt* 規則檔案的規則：</span><span class="sxs-lookup"><span data-stu-id="f8608-1347">A <xref:System.IO.StreamReader> is used to read the rules from the *ApacheModRewrite.txt* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

<span data-ttu-id="f8608-1348">範例應用程式會將要求從 `/apache-mod-rules-redirect/(.\*)` 重新導向至 `/redirected?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1348">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="f8608-1349">回應狀態碼為 302 (已找到)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1349">The response status code is *302 - Found*.</span></span>

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

<span data-ttu-id="f8608-1350">原始要求：`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="f8608-1350">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="f8608-1352">中介軟體可支援下列 Apache mod_rewrite 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="f8608-1352">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="f8608-1353">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-1353">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="f8608-1354">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="f8608-1354">HTTP_ACCEPT</span></span>
* <span data-ttu-id="f8608-1355">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="f8608-1355">HTTP_CONNECTION</span></span>
* <span data-ttu-id="f8608-1356">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="f8608-1356">HTTP_COOKIE</span></span>
* <span data-ttu-id="f8608-1357">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="f8608-1357">HTTP_FORWARDED</span></span>
* <span data-ttu-id="f8608-1358">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="f8608-1358">HTTP_HOST</span></span>
* <span data-ttu-id="f8608-1359">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="f8608-1359">HTTP_REFERER</span></span>
* <span data-ttu-id="f8608-1360">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="f8608-1360">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="f8608-1361">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f8608-1361">HTTPS</span></span>
* <span data-ttu-id="f8608-1362">IPV6</span><span class="sxs-lookup"><span data-stu-id="f8608-1362">IPV6</span></span>
* <span data-ttu-id="f8608-1363">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="f8608-1363">QUERY_STRING</span></span>
* <span data-ttu-id="f8608-1364">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-1364">REMOTE_ADDR</span></span>
* <span data-ttu-id="f8608-1365">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="f8608-1365">REMOTE_PORT</span></span>
* <span data-ttu-id="f8608-1366">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f8608-1366">REQUEST_FILENAME</span></span>
* <span data-ttu-id="f8608-1367">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="f8608-1367">REQUEST_METHOD</span></span>
* <span data-ttu-id="f8608-1368">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="f8608-1368">REQUEST_SCHEME</span></span>
* <span data-ttu-id="f8608-1369">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="f8608-1369">REQUEST_URI</span></span>
* <span data-ttu-id="f8608-1370">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f8608-1370">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="f8608-1371">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-1371">SERVER_ADDR</span></span>
* <span data-ttu-id="f8608-1372">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="f8608-1372">SERVER_PORT</span></span>
* <span data-ttu-id="f8608-1373">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="f8608-1373">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="f8608-1374">TIME</span><span class="sxs-lookup"><span data-stu-id="f8608-1374">TIME</span></span>
* <span data-ttu-id="f8608-1375">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="f8608-1375">TIME_DAY</span></span>
* <span data-ttu-id="f8608-1376">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="f8608-1376">TIME_HOUR</span></span>
* <span data-ttu-id="f8608-1377">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="f8608-1377">TIME_MIN</span></span>
* <span data-ttu-id="f8608-1378">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="f8608-1378">TIME_MON</span></span>
* <span data-ttu-id="f8608-1379">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="f8608-1379">TIME_SEC</span></span>
* <span data-ttu-id="f8608-1380">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="f8608-1380">TIME_WDAY</span></span>
* <span data-ttu-id="f8608-1381">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="f8608-1381">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="f8608-1382">IIS URL Rewrite Module 規則</span><span class="sxs-lookup"><span data-stu-id="f8608-1382">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="f8608-1383">若要使用與套用至 IIS URL Rewrite Module 一樣的規則集，請使用 <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>。</span><span class="sxs-lookup"><span data-stu-id="f8608-1383">To use the same rule set that applies to the IIS URL Rewrite Module, use <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>.</span></span> <span data-ttu-id="f8608-1384">請確認應用程式已部署規則檔。</span><span class="sxs-lookup"><span data-stu-id="f8608-1384">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="f8608-1385">在 Windows Server IIS 上執行時，請不要引導中介軟體使用應用程式的 *web.config* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f8608-1385">Don't direct the middleware to use the app's *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="f8608-1386">使用 IIS 時，這些規則應該儲存在應用程式的 *web.config* 檔案外部，以免與 IIS Rewrite Module 發生衝突。</span><span class="sxs-lookup"><span data-stu-id="f8608-1386">With IIS, these rules should be stored outside of the app's *web.config* file in order to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="f8608-1387">如需 IIS URL Rewrite Module 規則的詳細資訊和範例，請參閱 [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0) 和 [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)。</span><span class="sxs-lookup"><span data-stu-id="f8608-1387">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

<span data-ttu-id="f8608-1388">系統會使用 <xref:System.IO.StreamReader> 來讀取 *IISUrlRewrite.xml* 規則檔案的規則：</span><span class="sxs-lookup"><span data-stu-id="f8608-1388">A <xref:System.IO.StreamReader> is used to read the rules from the *IISUrlRewrite.xml* rules file:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

<span data-ttu-id="f8608-1389">範例應用程式會將要求從 `/iis-rules-rewrite/(.*)` 重寫至 `/rewritten?id=$1`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1389">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="f8608-1390">系統會將回應與 200 (確定)\*\* 狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-1390">The response is sent to the client with a *200 - OK* status code.</span></span>

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

<span data-ttu-id="f8608-1391">原始要求：`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="f8608-1391">Original Request: `/iis-rules-rewrite/1234`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="f8608-1393">如果您具備使用中且已設定伺服器層級規則的 IIS Rewrite Module，但其會以非預期的方式影響應用程式，則您可以停用應用程式的 IIS Rewrite Module 。</span><span class="sxs-lookup"><span data-stu-id="f8608-1393">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="f8608-1394">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="f8608-1394">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="f8608-1395">不支援的功能</span><span class="sxs-lookup"><span data-stu-id="f8608-1395">Unsupported features</span></span>

<span data-ttu-id="f8608-1396">與 ASP.NET Core 2.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：</span><span class="sxs-lookup"><span data-stu-id="f8608-1396">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="f8608-1397">輸出規則</span><span class="sxs-lookup"><span data-stu-id="f8608-1397">Outbound Rules</span></span>
* <span data-ttu-id="f8608-1398">自訂伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f8608-1398">Custom Server Variables</span></span>
* <span data-ttu-id="f8608-1399">萬用字元</span><span class="sxs-lookup"><span data-stu-id="f8608-1399">Wildcards</span></span>
* <span data-ttu-id="f8608-1400">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="f8608-1400">LogRewrittenUrl</span></span>

#### <a name="supported-server-variables"></a><span data-ttu-id="f8608-1401">支援的伺服器變數</span><span class="sxs-lookup"><span data-stu-id="f8608-1401">Supported server variables</span></span>

<span data-ttu-id="f8608-1402">中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：</span><span class="sxs-lookup"><span data-stu-id="f8608-1402">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="f8608-1403">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="f8608-1403">CONTENT_LENGTH</span></span>
* <span data-ttu-id="f8608-1404">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="f8608-1404">CONTENT_TYPE</span></span>
* <span data-ttu-id="f8608-1405">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="f8608-1405">HTTP_ACCEPT</span></span>
* <span data-ttu-id="f8608-1406">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="f8608-1406">HTTP_CONNECTION</span></span>
* <span data-ttu-id="f8608-1407">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="f8608-1407">HTTP_COOKIE</span></span>
* <span data-ttu-id="f8608-1408">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="f8608-1408">HTTP_HOST</span></span>
* <span data-ttu-id="f8608-1409">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="f8608-1409">HTTP_REFERER</span></span>
* <span data-ttu-id="f8608-1410">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="f8608-1410">HTTP_URL</span></span>
* <span data-ttu-id="f8608-1411">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="f8608-1411">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="f8608-1412">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f8608-1412">HTTPS</span></span>
* <span data-ttu-id="f8608-1413">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-1413">LOCAL_ADDR</span></span>
* <span data-ttu-id="f8608-1414">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="f8608-1414">QUERY_STRING</span></span>
* <span data-ttu-id="f8608-1415">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="f8608-1415">REMOTE_ADDR</span></span>
* <span data-ttu-id="f8608-1416">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="f8608-1416">REMOTE_PORT</span></span>
* <span data-ttu-id="f8608-1417">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="f8608-1417">REQUEST_FILENAME</span></span>
* <span data-ttu-id="f8608-1418">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="f8608-1418">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="f8608-1419">您也可以透過 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 取得 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="f8608-1419">You can also obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> via a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>.</span></span> <span data-ttu-id="f8608-1420">這種方法可讓重寫規則檔案的位置更有彈性。</span><span class="sxs-lookup"><span data-stu-id="f8608-1420">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="f8608-1421">請確認重寫規則檔案已部署到您所提供之路徑的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f8608-1421">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="f8608-1422">以方法為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="f8608-1422">Method-based rule</span></span>

<span data-ttu-id="f8608-1423">使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 以在方法中實作您自己的規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f8608-1423">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to implement your own rule logic in a method.</span></span> <span data-ttu-id="f8608-1424">`Add` 會公開 <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>，使 <xref:Microsoft.AspNetCore.Http.HttpContext> 可用於您的方法中。</span><span class="sxs-lookup"><span data-stu-id="f8608-1424">`Add` exposes the <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, which makes available the <xref:Microsoft.AspNetCore.Http.HttpContext> for use in your method.</span></span> <span data-ttu-id="f8608-1425">[RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*)可判斷其他管線處理的執行方式。</span><span class="sxs-lookup"><span data-stu-id="f8608-1425">The [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) determines how additional pipeline processing is handled.</span></span> <span data-ttu-id="f8608-1426">請將值設定為下表中描述的其中一個 <xref:Microsoft.AspNetCore.Rewrite.RuleResult> 欄位。</span><span class="sxs-lookup"><span data-stu-id="f8608-1426">Set the value to one of the <xref:Microsoft.AspNetCore.Rewrite.RuleResult> fields described in the following table.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="f8608-1427">動作</span><span class="sxs-lookup"><span data-stu-id="f8608-1427">Action</span></span>                                                           |
| ---
<span data-ttu-id="f8608-1428">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1428">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1429">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1429">'Blazor'</span></span>
- <span data-ttu-id="f8608-1430">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1430">'Identity'</span></span>
- <span data-ttu-id="f8608-1431">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1431">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1432">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1432">'Razor'</span></span>
- <span data-ttu-id="f8608-1433">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1433">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1434">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1434">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1435">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1435">'Blazor'</span></span>
- <span data-ttu-id="f8608-1436">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1436">'Identity'</span></span>
- <span data-ttu-id="f8608-1437">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1437">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1438">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1438">'Razor'</span></span>
- <span data-ttu-id="f8608-1439">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1439">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1440">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1440">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1441">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1441">'Blazor'</span></span>
- <span data-ttu-id="f8608-1442">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1442">'Identity'</span></span>
- <span data-ttu-id="f8608-1443">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1443">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1444">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1444">'Razor'</span></span>
- <span data-ttu-id="f8608-1445">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1445">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1446">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1446">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1447">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1447">'Blazor'</span></span>
- <span data-ttu-id="f8608-1448">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1448">'Identity'</span></span>
- <span data-ttu-id="f8608-1449">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1449">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1450">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1450">'Razor'</span></span>
- <span data-ttu-id="f8608-1451">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1451">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1452">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1452">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1453">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1453">'Blazor'</span></span>
- <span data-ttu-id="f8608-1454">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1454">'Identity'</span></span>
- <span data-ttu-id="f8608-1455">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1455">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1456">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1456">'Razor'</span></span>
- <span data-ttu-id="f8608-1457">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1457">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1458">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1458">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1459">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1459">'Blazor'</span></span>
- <span data-ttu-id="f8608-1460">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1460">'Identity'</span></span>
- <span data-ttu-id="f8608-1461">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1461">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1462">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1462">'Razor'</span></span>
- <span data-ttu-id="f8608-1463">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1463">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1464">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1464">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1465">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1465">'Blazor'</span></span>
- <span data-ttu-id="f8608-1466">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1466">'Identity'</span></span>
- <span data-ttu-id="f8608-1467">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1467">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1468">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1468">'Razor'</span></span>
- <span data-ttu-id="f8608-1469">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1469">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1470">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1470">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1471">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1471">'Blazor'</span></span>
- <span data-ttu-id="f8608-1472">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1472">'Identity'</span></span>
- <span data-ttu-id="f8608-1473">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1473">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1474">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1474">'Razor'</span></span>
- <span data-ttu-id="f8608-1475">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1475">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1476">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1476">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1477">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1477">'Blazor'</span></span>
- <span data-ttu-id="f8608-1478">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1478">'Identity'</span></span>
- <span data-ttu-id="f8608-1479">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1479">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1480">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1480">'Razor'</span></span>
- <span data-ttu-id="f8608-1481">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1481">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1482">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1482">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1483">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1483">'Blazor'</span></span>
- <span data-ttu-id="f8608-1484">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1484">'Identity'</span></span>
- <span data-ttu-id="f8608-1485">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1485">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1486">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1486">'Razor'</span></span>
- <span data-ttu-id="f8608-1487">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1487">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1488">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1488">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1489">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1489">'Blazor'</span></span>
- <span data-ttu-id="f8608-1490">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1490">'Identity'</span></span>
- <span data-ttu-id="f8608-1491">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1491">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1492">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1492">'Razor'</span></span>
- <span data-ttu-id="f8608-1493">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1493">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1494">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1494">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1495">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1495">'Blazor'</span></span>
- <span data-ttu-id="f8608-1496">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1496">'Identity'</span></span>
- <span data-ttu-id="f8608-1497">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1497">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1498">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1498">'Razor'</span></span>
- <span data-ttu-id="f8608-1499">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1499">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1500">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1500">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1501">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1501">'Blazor'</span></span>
- <span data-ttu-id="f8608-1502">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1502">'Identity'</span></span>
- <span data-ttu-id="f8608-1503">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1503">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1504">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1504">'Razor'</span></span>
- <span data-ttu-id="f8608-1505">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1505">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1506">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1506">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1507">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1507">'Blazor'</span></span>
- <span data-ttu-id="f8608-1508">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1508">'Identity'</span></span>
- <span data-ttu-id="f8608-1509">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1509">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1510">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1510">'Razor'</span></span>
- <span data-ttu-id="f8608-1511">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1511">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1512">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1512">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1513">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1513">'Blazor'</span></span>
- <span data-ttu-id="f8608-1514">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1514">'Identity'</span></span>
- <span data-ttu-id="f8608-1515">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1515">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1516">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1516">'Razor'</span></span>
- <span data-ttu-id="f8608-1517">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1517">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1518">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1518">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1519">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1519">'Blazor'</span></span>
- <span data-ttu-id="f8608-1520">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1520">'Identity'</span></span>
- <span data-ttu-id="f8608-1521">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1521">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1522">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1522">'Razor'</span></span>
- <span data-ttu-id="f8608-1523">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1523">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1524">------------------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1524">------------------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1525">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1525">'Blazor'</span></span>
- <span data-ttu-id="f8608-1526">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1526">'Identity'</span></span>
- <span data-ttu-id="f8608-1527">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1527">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1528">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1528">'Razor'</span></span>
- <span data-ttu-id="f8608-1529">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1529">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1530">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1530">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1531">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1531">'Blazor'</span></span>
- <span data-ttu-id="f8608-1532">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1532">'Identity'</span></span>
- <span data-ttu-id="f8608-1533">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1533">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1534">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1534">'Razor'</span></span>
- <span data-ttu-id="f8608-1535">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1535">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1536">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1536">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1537">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1537">'Blazor'</span></span>
- <span data-ttu-id="f8608-1538">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1538">'Identity'</span></span>
- <span data-ttu-id="f8608-1539">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1539">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1540">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1540">'Razor'</span></span>
- <span data-ttu-id="f8608-1541">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1541">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1542">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1542">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1543">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1543">'Blazor'</span></span>
- <span data-ttu-id="f8608-1544">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1544">'Identity'</span></span>
- <span data-ttu-id="f8608-1545">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1545">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1546">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1546">'Razor'</span></span>
- <span data-ttu-id="f8608-1547">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1547">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1548">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1548">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1549">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1549">'Blazor'</span></span>
- <span data-ttu-id="f8608-1550">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1550">'Identity'</span></span>
- <span data-ttu-id="f8608-1551">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1551">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1552">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1552">'Razor'</span></span>
- <span data-ttu-id="f8608-1553">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1553">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1554">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1554">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1555">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1555">'Blazor'</span></span>
- <span data-ttu-id="f8608-1556">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1556">'Identity'</span></span>
- <span data-ttu-id="f8608-1557">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1557">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1558">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1558">'Razor'</span></span>
- <span data-ttu-id="f8608-1559">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1559">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1560">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1560">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1561">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1561">'Blazor'</span></span>
- <span data-ttu-id="f8608-1562">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1562">'Identity'</span></span>
- <span data-ttu-id="f8608-1563">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1563">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1564">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1564">'Razor'</span></span>
- <span data-ttu-id="f8608-1565">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1565">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1566">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1566">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1567">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1567">'Blazor'</span></span>
- <span data-ttu-id="f8608-1568">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1568">'Identity'</span></span>
- <span data-ttu-id="f8608-1569">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1569">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1570">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1570">'Razor'</span></span>
- <span data-ttu-id="f8608-1571">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1571">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1572">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1572">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1573">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1573">'Blazor'</span></span>
- <span data-ttu-id="f8608-1574">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1574">'Identity'</span></span>
- <span data-ttu-id="f8608-1575">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1575">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1576">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1576">'Razor'</span></span>
- <span data-ttu-id="f8608-1577">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1577">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1578">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1578">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1579">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1579">'Blazor'</span></span>
- <span data-ttu-id="f8608-1580">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1580">'Identity'</span></span>
- <span data-ttu-id="f8608-1581">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1581">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1582">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1582">'Razor'</span></span>
- <span data-ttu-id="f8608-1583">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1583">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1584">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1584">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1585">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1585">'Blazor'</span></span>
- <span data-ttu-id="f8608-1586">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1586">'Identity'</span></span>
- <span data-ttu-id="f8608-1587">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1587">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1588">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1588">'Razor'</span></span>
- <span data-ttu-id="f8608-1589">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1589">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1590">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1590">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1591">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1591">'Blazor'</span></span>
- <span data-ttu-id="f8608-1592">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1592">'Identity'</span></span>
- <span data-ttu-id="f8608-1593">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1593">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1594">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1594">'Razor'</span></span>
- <span data-ttu-id="f8608-1595">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1595">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1596">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1596">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1597">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1597">'Blazor'</span></span>
- <span data-ttu-id="f8608-1598">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1598">'Identity'</span></span>
- <span data-ttu-id="f8608-1599">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1599">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1600">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1600">'Razor'</span></span>
- <span data-ttu-id="f8608-1601">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1601">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1602">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1602">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1603">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1603">'Blazor'</span></span>
- <span data-ttu-id="f8608-1604">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1604">'Identity'</span></span>
- <span data-ttu-id="f8608-1605">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1605">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1606">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1606">'Razor'</span></span>
- <span data-ttu-id="f8608-1607">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1607">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1608">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1608">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1609">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1609">'Blazor'</span></span>
- <span data-ttu-id="f8608-1610">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1610">'Identity'</span></span>
- <span data-ttu-id="f8608-1611">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1611">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1612">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1612">'Razor'</span></span>
- <span data-ttu-id="f8608-1613">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1613">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1614">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1614">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1615">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1615">'Blazor'</span></span>
- <span data-ttu-id="f8608-1616">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1616">'Identity'</span></span>
- <span data-ttu-id="f8608-1617">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1617">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1618">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1618">'Razor'</span></span>
- <span data-ttu-id="f8608-1619">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1619">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1620">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1620">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1621">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1621">'Blazor'</span></span>
- <span data-ttu-id="f8608-1622">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1622">'Identity'</span></span>
- <span data-ttu-id="f8608-1623">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1623">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1624">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1624">'Razor'</span></span>
- <span data-ttu-id="f8608-1625">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1625">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1626">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1626">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1627">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1627">'Blazor'</span></span>
- <span data-ttu-id="f8608-1628">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1628">'Identity'</span></span>
- <span data-ttu-id="f8608-1629">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1629">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1630">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1630">'Razor'</span></span>
- <span data-ttu-id="f8608-1631">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1631">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1632">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1632">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1633">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1633">'Blazor'</span></span>
- <span data-ttu-id="f8608-1634">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1634">'Identity'</span></span>
- <span data-ttu-id="f8608-1635">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1635">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1636">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1636">'Razor'</span></span>
- <span data-ttu-id="f8608-1637">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1637">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1638">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1638">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1639">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1639">'Blazor'</span></span>
- <span data-ttu-id="f8608-1640">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1640">'Identity'</span></span>
- <span data-ttu-id="f8608-1641">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1641">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1642">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1642">'Razor'</span></span>
- <span data-ttu-id="f8608-1643">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1643">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1644">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1644">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1645">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1645">'Blazor'</span></span>
- <span data-ttu-id="f8608-1646">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1646">'Identity'</span></span>
- <span data-ttu-id="f8608-1647">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1647">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1648">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1648">'Razor'</span></span>
- <span data-ttu-id="f8608-1649">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1649">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1650">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1650">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1651">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1651">'Blazor'</span></span>
- <span data-ttu-id="f8608-1652">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1652">'Identity'</span></span>
- <span data-ttu-id="f8608-1653">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1653">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1654">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1654">'Razor'</span></span>
- <span data-ttu-id="f8608-1655">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1655">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1656">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1656">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1657">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1657">'Blazor'</span></span>
- <span data-ttu-id="f8608-1658">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1658">'Identity'</span></span>
- <span data-ttu-id="f8608-1659">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1659">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1660">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1660">'Razor'</span></span>
- <span data-ttu-id="f8608-1661">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1661">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1662">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1662">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1663">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1663">'Blazor'</span></span>
- <span data-ttu-id="f8608-1664">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1664">'Identity'</span></span>
- <span data-ttu-id="f8608-1665">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1665">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1666">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1666">'Razor'</span></span>
- <span data-ttu-id="f8608-1667">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1667">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1668">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1668">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1669">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1669">'Blazor'</span></span>
- <span data-ttu-id="f8608-1670">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1670">'Identity'</span></span>
- <span data-ttu-id="f8608-1671">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1671">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1672">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1672">'Razor'</span></span>
- <span data-ttu-id="f8608-1673">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1673">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1674">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1674">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1675">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1675">'Blazor'</span></span>
- <span data-ttu-id="f8608-1676">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1676">'Identity'</span></span>
- <span data-ttu-id="f8608-1677">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1677">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1678">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1678">'Razor'</span></span>
- <span data-ttu-id="f8608-1679">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1679">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1680">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1680">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1681">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1681">'Blazor'</span></span>
- <span data-ttu-id="f8608-1682">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1682">'Identity'</span></span>
- <span data-ttu-id="f8608-1683">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1683">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1684">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1684">'Razor'</span></span>
- <span data-ttu-id="f8608-1685">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1685">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1686">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1686">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1687">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1687">'Blazor'</span></span>
- <span data-ttu-id="f8608-1688">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1688">'Identity'</span></span>
- <span data-ttu-id="f8608-1689">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1689">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1690">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1690">'Razor'</span></span>
- <span data-ttu-id="f8608-1691">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1691">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1692">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1692">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1693">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1693">'Blazor'</span></span>
- <span data-ttu-id="f8608-1694">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1694">'Identity'</span></span>
- <span data-ttu-id="f8608-1695">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1695">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1696">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1696">'Razor'</span></span>
- <span data-ttu-id="f8608-1697">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1697">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1698">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1698">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1699">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1699">'Blazor'</span></span>
- <span data-ttu-id="f8608-1700">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1700">'Identity'</span></span>
- <span data-ttu-id="f8608-1701">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1701">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1702">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1702">'Razor'</span></span>
- <span data-ttu-id="f8608-1703">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1703">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1704">-------------------------------- | |`RuleResult.ContinueRules`（預設） |繼續套用規則。</span><span class="sxs-lookup"><span data-stu-id="f8608-1704">-------------------------------- | | `RuleResult.ContinueRules` (default) | Continue applying rules.</span></span>                                         <span data-ttu-id="f8608-1705">| |`RuleResult.EndResponse`             |停止套用規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="f8608-1705">| | `RuleResult.EndResponse`             | Stop applying rules and send the response.</span></span>                       <span data-ttu-id="f8608-1706">| |`RuleResult.SkipRemainingRules`      |停止套用規則，並將內容傳送至下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="f8608-1706">| | `RuleResult.SkipRemainingRules`      | Stop applying rules and send the context to the next middleware.</span></span> |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

<span data-ttu-id="f8608-1707">範例應用程式會示範一個方法，以重新導向對 *.xml* 結尾之路徑的要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-1707">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="f8608-1708">若要求是針對 `/file.xml` 發出，則要求會重新導向至 `/xmlfiles/file.xml`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1708">If a request is made for `/file.xml`, the request is redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="f8608-1709">狀態碼會設定為 301 (已永久移動)\*\*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1709">The status code is set to *301 - Moved Permanently*.</span></span> <span data-ttu-id="f8608-1710">當瀏覽器針對 */xmlfiles/file.xml*發出新的要求時，靜態檔案中介軟體會從 *wwwroot/xmlfiles* 資料夾將檔案提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="f8608-1710">When the browser makes a new request for */xmlfiles/file.xml*, Static File Middleware serves the file to the client from the *wwwroot/xmlfiles* folder.</span></span> <span data-ttu-id="f8608-1711">若要重新導向，請明確設定回應的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f8608-1711">For a redirect, explicitly set the status code of the response.</span></span> <span data-ttu-id="f8608-1712">否則會傳回 200 (確定)\*\* 狀態碼，用戶端上也不會發生重新導向。</span><span class="sxs-lookup"><span data-stu-id="f8608-1712">Otherwise, a *200 - OK* status code is returned, and the redirect doesn't occur on the client.</span></span>

<span data-ttu-id="f8608-1713">*RewriteRules.cs*：</span><span class="sxs-lookup"><span data-stu-id="f8608-1713">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

<span data-ttu-id="f8608-1714">這種方法也能重寫要求。</span><span class="sxs-lookup"><span data-stu-id="f8608-1714">This approach can also rewrite requests.</span></span> <span data-ttu-id="f8608-1715">範例應用程式示範了如何重寫任何文字檔要求的路徑，以從 *wwwroot* 資料夾提供 *file.txt* 文字檔。</span><span class="sxs-lookup"><span data-stu-id="f8608-1715">The sample app demonstrates rewriting the path for any text file request to serve the *file.txt* text file from the *wwwroot* folder.</span></span> <span data-ttu-id="f8608-1716">靜態檔案中介軟體會根據更新的要求路徑來提供檔案：</span><span class="sxs-lookup"><span data-stu-id="f8608-1716">Static File Middleware serves the file based on the updated request path:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

<span data-ttu-id="f8608-1717">*RewriteRules.cs*：</span><span class="sxs-lookup"><span data-stu-id="f8608-1717">*RewriteRules.cs*:</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a><span data-ttu-id="f8608-1718">以 IRule 為基礎的規則</span><span class="sxs-lookup"><span data-stu-id="f8608-1718">IRule-based rule</span></span>

<span data-ttu-id="f8608-1719">利用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 來使用類別中實作 <xref:Microsoft.AspNetCore.Rewrite.IRule> 介面的專屬規則邏輯。</span><span class="sxs-lookup"><span data-stu-id="f8608-1719">Use <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> to use rule logic in a class that implements the <xref:Microsoft.AspNetCore.Rewrite.IRule> interface.</span></span> <span data-ttu-id="f8608-1720">相較於使用以方法為基礎的規則方法，`IRule` 提供了更高彈性。</span><span class="sxs-lookup"><span data-stu-id="f8608-1720">`IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="f8608-1721">您的實作類別可以包含建構函式，以便您在其中傳入 <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> 方法的參數。</span><span class="sxs-lookup"><span data-stu-id="f8608-1721">Your implementation class may include a constructor that allows you can pass in parameters for the <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> method.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="f8608-1722">為了滿足若干條件，系統會檢查範例應用程式中的 `extension` 和 `newPath` 參數值。</span><span class="sxs-lookup"><span data-stu-id="f8608-1722">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="f8608-1723">`extension` 必須包含值，而且值必須是 *.png*、*.jpg* 或 *.gif*。</span><span class="sxs-lookup"><span data-stu-id="f8608-1723">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="f8608-1724">如果 `newPath` 無效，就會擲回 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="f8608-1724">If the `newPath` isn't valid, an <xref:System.ArgumentException> is thrown.</span></span> <span data-ttu-id="f8608-1725">若要求是針對 *image.png* 發出，則要求會重新導向至 `/png-images/image.png`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1725">If a request is made for *image.png*, the request is redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="f8608-1726">若要求是針對 *image.jpg* 發出，則要求會重新導向至 `/jpg-images/image.jpg`。</span><span class="sxs-lookup"><span data-stu-id="f8608-1726">If a request is made for *image.jpg*, the request is redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="f8608-1727">狀態碼會設定為 301 (已永久移動)\*\*，而 `context.Result` 會設定為停止處理規則，並傳送回應。</span><span class="sxs-lookup"><span data-stu-id="f8608-1727">The status code is set to *301 - Moved Permanently*, and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

<span data-ttu-id="f8608-1728">原始要求：`/image.png`</span><span class="sxs-lookup"><span data-stu-id="f8608-1728">Original Request: `/image.png`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.png 的要求和回應](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="f8608-1730">原始要求：`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="f8608-1730">Original Request: `/image.jpg`</span></span>

![瀏覽器視窗，其中的開發人員工具會追蹤 image.jpg 的要求和回應](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="f8608-1732">Regex 範例</span><span class="sxs-lookup"><span data-stu-id="f8608-1732">Regex examples</span></span>

| <span data-ttu-id="f8608-1733">目標</span><span class="sxs-lookup"><span data-stu-id="f8608-1733">Goal</span></span> | <span data-ttu-id="f8608-1734">Regex 字串及</span><span class="sxs-lookup"><span data-stu-id="f8608-1734">Regex String &</span></span><br><span data-ttu-id="f8608-1735">比對範例</span><span class="sxs-lookup"><span data-stu-id="f8608-1735">Match Example</span></span> | <span data-ttu-id="f8608-1736">取代字串及</span><span class="sxs-lookup"><span data-stu-id="f8608-1736">Replacement String &</span></span><br><span data-ttu-id="f8608-1737">輸出範例</span><span class="sxs-lookup"><span data-stu-id="f8608-1737">Output Example</span></span> |
| ---- | ---
<span data-ttu-id="f8608-1738">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1738">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1739">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1739">'Blazor'</span></span>
- <span data-ttu-id="f8608-1740">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1740">'Identity'</span></span>
- <span data-ttu-id="f8608-1741">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1741">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1742">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1742">'Razor'</span></span>
- <span data-ttu-id="f8608-1743">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1743">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1744">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1744">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1745">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1745">'Blazor'</span></span>
- <span data-ttu-id="f8608-1746">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1746">'Identity'</span></span>
- <span data-ttu-id="f8608-1747">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1747">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1748">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1748">'Razor'</span></span>
- <span data-ttu-id="f8608-1749">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1749">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1750">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1750">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1751">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1751">'Blazor'</span></span>
- <span data-ttu-id="f8608-1752">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1752">'Identity'</span></span>
- <span data-ttu-id="f8608-1753">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1753">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1754">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1754">'Razor'</span></span>
- <span data-ttu-id="f8608-1755">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1755">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1756">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1756">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1757">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1757">'Blazor'</span></span>
- <span data-ttu-id="f8608-1758">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1758">'Identity'</span></span>
- <span data-ttu-id="f8608-1759">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1759">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1760">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1760">'Razor'</span></span>
- <span data-ttu-id="f8608-1761">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1761">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1762">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1762">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1763">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1763">'Blazor'</span></span>
- <span data-ttu-id="f8608-1764">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1764">'Identity'</span></span>
- <span data-ttu-id="f8608-1765">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1765">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1766">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1766">'Razor'</span></span>
- <span data-ttu-id="f8608-1767">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1767">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1768">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1768">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1769">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1769">'Blazor'</span></span>
- <span data-ttu-id="f8608-1770">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1770">'Identity'</span></span>
- <span data-ttu-id="f8608-1771">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1771">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1772">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1772">'Razor'</span></span>
- <span data-ttu-id="f8608-1773">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1773">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1774">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1774">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1775">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1775">'Blazor'</span></span>
- <span data-ttu-id="f8608-1776">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1776">'Identity'</span></span>
- <span data-ttu-id="f8608-1777">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1777">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1778">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1778">'Razor'</span></span>
- <span data-ttu-id="f8608-1779">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1779">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1780">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1780">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1781">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1781">'Blazor'</span></span>
- <span data-ttu-id="f8608-1782">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1782">'Identity'</span></span>
- <span data-ttu-id="f8608-1783">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1783">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1784">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1784">'Razor'</span></span>
- <span data-ttu-id="f8608-1785">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1785">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1786">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1786">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1787">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1787">'Blazor'</span></span>
- <span data-ttu-id="f8608-1788">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1788">'Identity'</span></span>
- <span data-ttu-id="f8608-1789">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1789">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1790">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1790">'Razor'</span></span>
- <span data-ttu-id="f8608-1791">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1791">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1792">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1792">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1793">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1793">'Blazor'</span></span>
- <span data-ttu-id="f8608-1794">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1794">'Identity'</span></span>
- <span data-ttu-id="f8608-1795">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1795">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1796">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1796">'Razor'</span></span>
- <span data-ttu-id="f8608-1797">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1797">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1798">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1798">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1799">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1799">'Blazor'</span></span>
- <span data-ttu-id="f8608-1800">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1800">'Identity'</span></span>
- <span data-ttu-id="f8608-1801">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1801">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1802">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1802">'Razor'</span></span>
- <span data-ttu-id="f8608-1803">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1803">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1804">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1804">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1805">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1805">'Blazor'</span></span>
- <span data-ttu-id="f8608-1806">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1806">'Identity'</span></span>
- <span data-ttu-id="f8608-1807">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1807">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1808">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1808">'Razor'</span></span>
- <span data-ttu-id="f8608-1809">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1809">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1810">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1810">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1811">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1811">'Blazor'</span></span>
- <span data-ttu-id="f8608-1812">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1812">'Identity'</span></span>
- <span data-ttu-id="f8608-1813">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1813">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1814">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1814">'Razor'</span></span>
- <span data-ttu-id="f8608-1815">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1815">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1816">---------------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1816">---------------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1817">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1817">'Blazor'</span></span>
- <span data-ttu-id="f8608-1818">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1818">'Identity'</span></span>
- <span data-ttu-id="f8608-1819">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1819">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1820">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1820">'Razor'</span></span>
- <span data-ttu-id="f8608-1821">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1821">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1822">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1822">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1823">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1823">'Blazor'</span></span>
- <span data-ttu-id="f8608-1824">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1824">'Identity'</span></span>
- <span data-ttu-id="f8608-1825">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1825">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1826">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1826">'Razor'</span></span>
- <span data-ttu-id="f8608-1827">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1827">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1828">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1828">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1829">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1829">'Blazor'</span></span>
- <span data-ttu-id="f8608-1830">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1830">'Identity'</span></span>
- <span data-ttu-id="f8608-1831">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1831">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1832">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1832">'Razor'</span></span>
- <span data-ttu-id="f8608-1833">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1833">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1834">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1834">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1835">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1835">'Blazor'</span></span>
- <span data-ttu-id="f8608-1836">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1836">'Identity'</span></span>
- <span data-ttu-id="f8608-1837">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1837">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1838">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1838">'Razor'</span></span>
- <span data-ttu-id="f8608-1839">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1839">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1840">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1840">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1841">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1841">'Blazor'</span></span>
- <span data-ttu-id="f8608-1842">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1842">'Identity'</span></span>
- <span data-ttu-id="f8608-1843">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1843">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1844">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1844">'Razor'</span></span>
- <span data-ttu-id="f8608-1845">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1845">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1846">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1846">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1847">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1847">'Blazor'</span></span>
- <span data-ttu-id="f8608-1848">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1848">'Identity'</span></span>
- <span data-ttu-id="f8608-1849">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1849">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1850">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1850">'Razor'</span></span>
- <span data-ttu-id="f8608-1851">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1851">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1852">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1852">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1853">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1853">'Blazor'</span></span>
- <span data-ttu-id="f8608-1854">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1854">'Identity'</span></span>
- <span data-ttu-id="f8608-1855">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1855">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1856">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1856">'Razor'</span></span>
- <span data-ttu-id="f8608-1857">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1857">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1858">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1858">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1859">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1859">'Blazor'</span></span>
- <span data-ttu-id="f8608-1860">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1860">'Identity'</span></span>
- <span data-ttu-id="f8608-1861">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1861">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1862">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1862">'Razor'</span></span>
- <span data-ttu-id="f8608-1863">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1863">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1864">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1864">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1865">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1865">'Blazor'</span></span>
- <span data-ttu-id="f8608-1866">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1866">'Identity'</span></span>
- <span data-ttu-id="f8608-1867">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1867">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1868">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1868">'Razor'</span></span>
- <span data-ttu-id="f8608-1869">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1869">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1870">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1870">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1871">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1871">'Blazor'</span></span>
- <span data-ttu-id="f8608-1872">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1872">'Identity'</span></span>
- <span data-ttu-id="f8608-1873">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1873">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1874">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1874">'Razor'</span></span>
- <span data-ttu-id="f8608-1875">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1875">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1876">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1876">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1877">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1877">'Blazor'</span></span>
- <span data-ttu-id="f8608-1878">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1878">'Identity'</span></span>
- <span data-ttu-id="f8608-1879">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1879">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1880">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1880">'Razor'</span></span>
- <span data-ttu-id="f8608-1881">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1881">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1882">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1882">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1883">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1883">'Blazor'</span></span>
- <span data-ttu-id="f8608-1884">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1884">'Identity'</span></span>
- <span data-ttu-id="f8608-1885">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1885">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1886">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1886">'Razor'</span></span>
- <span data-ttu-id="f8608-1887">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1887">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1888">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1888">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1889">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1889">'Blazor'</span></span>
- <span data-ttu-id="f8608-1890">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1890">'Identity'</span></span>
- <span data-ttu-id="f8608-1891">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1891">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1892">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1892">'Razor'</span></span>
- <span data-ttu-id="f8608-1893">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1893">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1894">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1894">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1895">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1895">'Blazor'</span></span>
- <span data-ttu-id="f8608-1896">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1896">'Identity'</span></span>
- <span data-ttu-id="f8608-1897">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1897">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1898">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1898">'Razor'</span></span>
- <span data-ttu-id="f8608-1899">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1899">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1900">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1900">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1901">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1901">'Blazor'</span></span>
- <span data-ttu-id="f8608-1902">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1902">'Identity'</span></span>
- <span data-ttu-id="f8608-1903">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1903">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1904">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1904">'Razor'</span></span>
- <span data-ttu-id="f8608-1905">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1905">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1906">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1906">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1907">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1907">'Blazor'</span></span>
- <span data-ttu-id="f8608-1908">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1908">'Identity'</span></span>
- <span data-ttu-id="f8608-1909">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1909">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1910">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1910">'Razor'</span></span>
- <span data-ttu-id="f8608-1911">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1911">'SignalR' uid:</span></span> 

-
<span data-ttu-id="f8608-1912">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f8608-1912">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f8608-1913">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1913">'Blazor'</span></span>
- <span data-ttu-id="f8608-1914">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f8608-1914">'Identity'</span></span>
- <span data-ttu-id="f8608-1915">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f8608-1915">'Let's Encrypt'</span></span>
- <span data-ttu-id="f8608-1916">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f8608-1916">'Razor'</span></span>
- <span data-ttu-id="f8608-1917">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f8608-1917">'SignalR' uid:</span></span> 

<span data-ttu-id="f8608-1918">------------------- | |將路徑重寫為 querystring |`^path/(.*)/(.*)`</span><span class="sxs-lookup"><span data-stu-id="f8608-1918">------------------- | | Rewrite path into querystring | `^path/(.*)/(.*)`</span></span><br>`/path/abc/123` | `path?var1=$1&var2=$2`<br><span data-ttu-id="f8608-1919">`/path?var1=abc&var2=123`| |去除尾端斜線 |`(.*)/$`</span><span class="sxs-lookup"><span data-stu-id="f8608-1919">`/path?var1=abc&var2=123` | | Strip trailing slash | `(.*)/$`</span></span><br>`/path/` | `$1`<br><span data-ttu-id="f8608-1920">`/path`| |強制使用尾端斜線 |`(.*[^/])$`</span><span class="sxs-lookup"><span data-stu-id="f8608-1920">`/path` | | Enforce trailing slash | `(.*[^/])$`</span></span><br>`/path` | `$1/`<br><span data-ttu-id="f8608-1921">`/path/`| |避免重寫特定要求 |`^(.*)(?<!\.axd)$`或`^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="f8608-1921">`/path/` | | Avoid rewriting specific requests | `^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="f8608-1922">是：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="f8608-1922">Yes: `/resource.htm`</span></span><br><span data-ttu-id="f8608-1923">不：`/resource.axd` | `rewritten/$1`</span><span class="sxs-lookup"><span data-stu-id="f8608-1923">No: `/resource.axd` | `rewritten/$1`</span></span><br>`/rewritten/resource.htm`<br><span data-ttu-id="f8608-1924">`/resource.axd`| |重新排列 URL 區段 |`path/(.*)/(.*)/(.*)`</span><span class="sxs-lookup"><span data-stu-id="f8608-1924">`/resource.axd` | | Rearrange URL segments | `path/(.*)/(.*)/(.*)`</span></span><br>`path/1/2/3` | `path/$3/$2/$1`<br><span data-ttu-id="f8608-1925">`path/3/2/1`| |取代 URL 區段 |`^(.*)/segment2/(.*)`</span><span class="sxs-lookup"><span data-stu-id="f8608-1925">`path/3/2/1` | | Replace a URL segment | `^(.*)/segment2/(.*)`</span></span><br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f8608-1926">其他資源</span><span class="sxs-lookup"><span data-stu-id="f8608-1926">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="f8608-1927">.NET 中的規則運算式</span><span class="sxs-lookup"><span data-stu-id="f8608-1927">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="f8608-1928">正則運算式語言-快速參考</span><span class="sxs-lookup"><span data-stu-id="f8608-1928">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="f8608-1929">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="f8608-1929">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* <span data-ttu-id="f8608-1930">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0 (適用於 IIS))</span><span class="sxs-lookup"><span data-stu-id="f8608-1930">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="f8608-1931">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)</span><span class="sxs-lookup"><span data-stu-id="f8608-1931">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* [<span data-ttu-id="f8608-1932">IIS URL Rewrite Module 論壇</span><span class="sxs-lookup"><span data-stu-id="f8608-1932">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* <span data-ttu-id="f8608-1933">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (保持精簡的 URL 結構)</span><span class="sxs-lookup"><span data-stu-id="f8608-1933">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en)</span></span>
* <span data-ttu-id="f8608-1934">[10 URL Rewriting Tips and Tricks](https://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 個重寫 URL 的祕訣與技巧)</span><span class="sxs-lookup"><span data-stu-id="f8608-1934">[10 URL Rewriting Tips and Tricks](https://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="f8608-1935">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (是否要使用斜線)</span><span class="sxs-lookup"><span data-stu-id="f8608-1935">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
