---
<span data-ttu-id="efc56-101">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="efc56-101">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="efc56-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="efc56-102">'Blazor'</span></span>
- <span data-ttu-id="efc56-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="efc56-103">'Identity'</span></span>
- <span data-ttu-id="efc56-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="efc56-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="efc56-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="efc56-105">'Razor'</span></span>
- <span data-ttu-id="efc56-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="efc56-106">'SignalR' uid:</span></span> 

---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="efc56-107">在瀏覽器應用程式中使用 gRPC</span><span class="sxs-lookup"><span data-stu-id="efc56-107">Use gRPC in browser apps</span></span>

<span data-ttu-id="efc56-108">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="efc56-108">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="efc56-109">您無法從以瀏覽器為基礎的應用程式呼叫 HTTP/2 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="efc56-109">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="efc56-110">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)是一種通訊協定，可讓瀏覽器 JavaScript 和 Blazor 應用程式呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="efc56-110">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="efc56-111">本文說明如何在 .NET Core 中使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="efc56-111">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="grpc-web-in-aspnet-core-vs-envoy"></a><span data-ttu-id="efc56-112">gRPC-Web in ASP.NET Core 與 Envoy 的比較</span><span class="sxs-lookup"><span data-stu-id="efc56-112">gRPC-Web in ASP.NET Core vs. Envoy</span></span>

<span data-ttu-id="efc56-113">有兩種方式可讓您選擇如何將 gRPC 新增至 ASP.NET Core 應用程式：</span><span class="sxs-lookup"><span data-stu-id="efc56-113">There are two choices for how to add gRPC-Web to an ASP.NET Core app:</span></span>

* <span data-ttu-id="efc56-114">在 ASP.NET Core 中支援 gRPC 和 gRPC HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="efc56-114">Support gRPC-Web alongside gRPC HTTP/2 in ASP.NET Core.</span></span> <span data-ttu-id="efc56-115">此選項會使用封裝所提供的中介軟體 `Grpc.AspNetCore.Web` 。</span><span class="sxs-lookup"><span data-stu-id="efc56-115">This option uses middleware provided by the `Grpc.AspNetCore.Web` package.</span></span>
* <span data-ttu-id="efc56-116">使用[Envoy proxy 的](https://www.envoyproxy.io/)GRPC-web 支援，將 GRPC-web 轉譯為 gRPC HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="efc56-116">Use the [Envoy proxy's](https://www.envoyproxy.io/) gRPC-Web support to translate gRPC-Web to gRPC HTTP/2.</span></span> <span data-ttu-id="efc56-117">然後，將已轉譯的呼叫轉送到 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="efc56-117">The translated call is then forwarded onto the ASP.NET Core app.</span></span>

<span data-ttu-id="efc56-118">每種方法都有優缺點。</span><span class="sxs-lookup"><span data-stu-id="efc56-118">There are pros and cons to each approach.</span></span> <span data-ttu-id="efc56-119">如果您已經在應用程式的環境中使用 Envoy 作為 proxy，也可以使用它來提供 gRPC Web 支援。</span><span class="sxs-lookup"><span data-stu-id="efc56-119">If you're already using Envoy as a proxy in your app's environment, it might make sense to also use it to provide gRPC-Web support.</span></span> <span data-ttu-id="efc56-120">如果您想要只需要 ASP.NET Core 的簡單 gRPC Web 解決方案， `Grpc.AspNetCore.Web` 是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="efc56-120">If you want a simple solution for gRPC-Web that only requires ASP.NET Core, `Grpc.AspNetCore.Web` is a good choice.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="efc56-121">在 ASP.NET Core 中設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="efc56-121">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="efc56-122">ASP.NET Core 中裝載的 gRPC 服務可以設定為支援 gRPC-Web 與 HTTP/2 gRPC。</span><span class="sxs-lookup"><span data-stu-id="efc56-122">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="efc56-123">gRPC-Web 不需要對服務進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="efc56-123">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="efc56-124">唯一的修改是啟動設定。</span><span class="sxs-lookup"><span data-stu-id="efc56-124">The only modification is startup configuration.</span></span>

<span data-ttu-id="efc56-125">若要使用 ASP.NET Core gRPC 服務啟用 gRPC-Web：</span><span class="sxs-lookup"><span data-stu-id="efc56-125">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="efc56-126">新增對[Grpc. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)封裝的參考。</span><span class="sxs-lookup"><span data-stu-id="efc56-126">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="efc56-127">藉由將 `UseGrpcWeb` 和新增 `EnableGrpcWeb` 至*Startup.cs*，將應用程式設定為使用 gRPC Web：</span><span class="sxs-lookup"><span data-stu-id="efc56-127">Configure the app to use gRPC-Web by adding `UseGrpcWeb` and `EnableGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="efc56-128">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="efc56-128">The preceding code:</span></span>

* <span data-ttu-id="efc56-129">加入 gRPC-Web 中介軟體，在 `UseGrpcWeb` 路由之後和結束點之前。</span><span class="sxs-lookup"><span data-stu-id="efc56-129">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="efc56-130">指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援使用的 GRPC Web `EnableGrpcWeb` 。</span><span class="sxs-lookup"><span data-stu-id="efc56-130">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="efc56-131">或者，您可以設定 gRPC-Web 中介軟體，讓所有服務預設都支援 gRPC-Web，而 `EnableGrpcWeb` 不需要。</span><span class="sxs-lookup"><span data-stu-id="efc56-131">Alternatively, the gRPC-Web middleware can be configured so all services support gRPC-Web by default and `EnableGrpcWeb` isn't required.</span></span> <span data-ttu-id="efc56-132">指定 `new GrpcWebOptions { DefaultEnabled = true }` 加入中介軟體的時機。</span><span class="sxs-lookup"><span data-stu-id="efc56-132">Specify `new GrpcWebOptions { DefaultEnabled = true }` when the middleware is added.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=12)]

> [!NOTE]
> <span data-ttu-id="efc56-133">已知的問題是，當[HTTP.sys](xref:fundamentals/servers/httpsys)在 .net Core 3.x 中裝載時，會導致 gRPC 失敗。</span><span class="sxs-lookup"><span data-stu-id="efc56-133">There is a known issue that causes gRPC-Web to fail when [hosted by Http.sys](xref:fundamentals/servers/httpsys) in .NET Core 3.x.</span></span>
>
> <span data-ttu-id="efc56-134">您可以在[這裡](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202)取得在 HTTP.sys 上運作 gRPC 的因應措施。</span><span class="sxs-lookup"><span data-stu-id="efc56-134">A workaround to get gRPC-Web working on Http.sys is available [here](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202).</span></span>

### <a name="grpc-web-and-cors"></a><span data-ttu-id="efc56-135">gRPC-Web 和 CORS</span><span class="sxs-lookup"><span data-stu-id="efc56-135">gRPC-Web and CORS</span></span>

<span data-ttu-id="efc56-136">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="efc56-136">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="efc56-137">這種限制適用于使用瀏覽器應用程式進行 gRPC Web 呼叫。</span><span class="sxs-lookup"><span data-stu-id="efc56-137">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="efc56-138">例如，服務所提供的瀏覽器應用程式 `https://www.contoso.com` 遭到封鎖，無法呼叫裝載于的 GRPC Web 服務 `https://services.contoso.com` 。</span><span class="sxs-lookup"><span data-stu-id="efc56-138">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="efc56-139">跨原始來源資源分享（CORS）可以用來放寬這種限制。</span><span class="sxs-lookup"><span data-stu-id="efc56-139">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="efc56-140">若要允許您的瀏覽器應用程式進行跨原始來源的 gRPC 呼叫，請[在 ASP.NET Core 中設定 CORS](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="efc56-140">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="efc56-141">使用內建的 CORS 支援，並使用公開 gRPC 特有的標頭 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*> 。</span><span class="sxs-lookup"><span data-stu-id="efc56-141">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="efc56-142">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="efc56-142">The preceding code:</span></span>

* <span data-ttu-id="efc56-143">呼叫 `AddCors` 以新增 cors 服務，並設定會公開 gRPC 特定標頭的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="efc56-143">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="efc56-144">呼叫 `UseCors` ，以在路由和結束端點之後加入 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="efc56-144">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="efc56-145">指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援 CORS 與 `RequiresCors` 。</span><span class="sxs-lookup"><span data-stu-id="efc56-145">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="efc56-146">從瀏覽器呼叫 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="efc56-146">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="efc56-147">瀏覽器應用程式可以使用 gRPC 來呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="efc56-147">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="efc56-148">從瀏覽器呼叫具有 gRPC 的 gRPC 服務時，有一些需求和限制：</span><span class="sxs-lookup"><span data-stu-id="efc56-148">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="efc56-149">伺服器必須已設定為支援 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="efc56-149">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="efc56-150">不支援用戶端串流和雙向串流呼叫。</span><span class="sxs-lookup"><span data-stu-id="efc56-150">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="efc56-151">支援伺服器串流。</span><span class="sxs-lookup"><span data-stu-id="efc56-151">Server streaming is supported.</span></span>
* <span data-ttu-id="efc56-152">若要在不同的網域上呼叫 gRPC 服務，必須在伺服器上設定[CORS](xref:security/cors) 。</span><span class="sxs-lookup"><span data-stu-id="efc56-152">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="efc56-153">JavaScript gRPC-Web 用戶端</span><span class="sxs-lookup"><span data-stu-id="efc56-153">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="efc56-154">有一個 JavaScript gRPC Web 用戶端。</span><span class="sxs-lookup"><span data-stu-id="efc56-154">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="efc56-155">如需如何從 JavaScript 使用 gRPC Web 的指示，請參閱[使用 gRPC 撰寫 javascript 用戶端程式代碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。</span><span class="sxs-lookup"><span data-stu-id="efc56-155">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="efc56-156">使用 .NET gRPC 用戶端設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="efc56-156">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="efc56-157">您可以設定 .NET gRPC 用戶端進行 gRPC-Web 呼叫。</span><span class="sxs-lookup"><span data-stu-id="efc56-157">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="efc56-158">這適用于[ Blazor WebAssembly](xref:blazor/index#blazor-webassembly)應用程式（裝載于瀏覽器中），並具有 JavaScript 程式碼的相同 HTTP 限制。</span><span class="sxs-lookup"><span data-stu-id="efc56-158">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="efc56-159">使用 .NET 用戶端呼叫 gRPC Web 與[HTTP/2 gRPC 相同](xref:grpc/client)。</span><span class="sxs-lookup"><span data-stu-id="efc56-159">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="efc56-160">唯一的修改是建立通道的方式。</span><span class="sxs-lookup"><span data-stu-id="efc56-160">The only modification is how the channel is created.</span></span>

<span data-ttu-id="efc56-161">若要使用 gRPC-Web：</span><span class="sxs-lookup"><span data-stu-id="efc56-161">To use gRPC-Web:</span></span>

* <span data-ttu-id="efc56-162">將參考新增至[Grpc 的 .net. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)封裝。</span><span class="sxs-lookup"><span data-stu-id="efc56-162">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="efc56-163">請確定[Grpc .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)封裝的參考已2.29.0 或更高。</span><span class="sxs-lookup"><span data-stu-id="efc56-163">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.29.0 or greater.</span></span>
* <span data-ttu-id="efc56-164">設定通道以使用 `GrpcWebHandler` ：</span><span class="sxs-lookup"><span data-stu-id="efc56-164">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="efc56-165">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="efc56-165">The preceding code:</span></span>

* <span data-ttu-id="efc56-166">設定通道以使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="efc56-166">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="efc56-167">建立用戶端，並使用通道進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="efc56-167">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="efc56-168">`GrpcWebHandler`具有下列設定選項：</span><span class="sxs-lookup"><span data-stu-id="efc56-168">`GrpcWebHandler` has the following configuration options:</span></span>

* <span data-ttu-id="efc56-169">**InnerHandler**： <xref:System.Net.Http.HttpMessageHandler> 發出 gRPC HTTP 要求的基礎，例如 `HttpClientHandler` 。</span><span class="sxs-lookup"><span data-stu-id="efc56-169">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="efc56-170">**GrpcWebMode**：指定 gRPC HTTP 要求是否為或的列舉類型 `Content-Type` `application/grpc-web` `application/grpc-web-text` 。</span><span class="sxs-lookup"><span data-stu-id="efc56-170">**GrpcWebMode**: An enumeration type that specifies whether the gRPC HTTP request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="efc56-171">`GrpcWebMode.GrpcWeb`設定要在沒有編碼的情況下傳送的內容。</span><span class="sxs-lookup"><span data-stu-id="efc56-171">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="efc56-172">預設值。</span><span class="sxs-lookup"><span data-stu-id="efc56-172">Default value.</span></span>
    * <span data-ttu-id="efc56-173">`GrpcWebMode.GrpcWebText`將內容設定為 base64 編碼。</span><span class="sxs-lookup"><span data-stu-id="efc56-173">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="efc56-174">瀏覽器中的伺服器串流呼叫所需。</span><span class="sxs-lookup"><span data-stu-id="efc56-174">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="efc56-175">**HttpVersion**： `Version` 用來在基礎 gRPC HTTP 要求上設定[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Version)的 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="efc56-175">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="efc56-176">gRPC-Web 不需要特定版本，除非指定，否則不會覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="efc56-176">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="efc56-177">產生的 gRPC 用戶端具有呼叫一元方法的同步和非同步方法。</span><span class="sxs-lookup"><span data-stu-id="efc56-177">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="efc56-178">例如， `SayHello` 會同步處理，而且 `SayHelloAsync` 是非同步。</span><span class="sxs-lookup"><span data-stu-id="efc56-178">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="efc56-179">在 WebAssembly 應用程式中呼叫同步處理方法， Blazor 會導致應用程式沒有回應。</span><span class="sxs-lookup"><span data-stu-id="efc56-179">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="efc56-180">非同步方法必須一律在 WebAssembly 中使用 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="efc56-180">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="efc56-181">其他資源</span><span class="sxs-lookup"><span data-stu-id="efc56-181">Additional resources</span></span>

* [<span data-ttu-id="efc56-182">適用于 Web 用戶端的 gRPC GitHub 專案</span><span class="sxs-lookup"><span data-stu-id="efc56-182">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
