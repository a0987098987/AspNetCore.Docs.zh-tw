---
title: 在瀏覽器應用程式中使用 gRPC
author: jamesnk
description: 瞭解如何在ASP.NET酷睿配置 gRPC 服務,以便使用 gRPC-Web 從瀏覽器應用調用 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 0bb8157525ccd32991d8925816c1b599c3d21a92
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977141"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="e272a-103">在瀏覽器應用程式中使用 gRPC</span><span class="sxs-lookup"><span data-stu-id="e272a-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="e272a-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="e272a-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e272a-105">**.NET 中的 gRPC-Web 支援是實驗性的**</span><span class="sxs-lookup"><span data-stu-id="e272a-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="e272a-106">gRPC-Web for .NET 是一個實驗專案,而不是一個承諾的產品。</span><span class="sxs-lookup"><span data-stu-id="e272a-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="e272a-107">我們想要：</span><span class="sxs-lookup"><span data-stu-id="e272a-107">We want to:</span></span>
>
> * <span data-ttu-id="e272a-108">測試我們實現 gRPC-Web 的方法有效。</span><span class="sxs-lookup"><span data-stu-id="e272a-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="e272a-109">與通過代理設定 gRPC-Web 的傳統方式相比,獲取此方法對 .NET 開發人員是否有用的回饋。</span><span class="sxs-lookup"><span data-stu-id="e272a-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="e272a-110">請留下回饋,[https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet)以確保我們建構開發者喜歡並高效使用的東西。</span><span class="sxs-lookup"><span data-stu-id="e272a-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="e272a-111">無法從基於瀏覽器的應用調用 HTTP/2 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="e272a-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="e272a-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)是一種協定,允許瀏覽器 JavaScript 和 Blazor 應用調用 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="e272a-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="e272a-113">本文介紹如何在 .NET 核心中使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="e272a-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="e272a-114">在 ASP.NET核心中設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="e272a-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="e272a-115">ASP.NET核心中託管的 gRPC 服務可以配置為支援 gRPC-Web 以及 HTTP/2 gRPC。</span><span class="sxs-lookup"><span data-stu-id="e272a-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="e272a-116">gRPC-Web 不需要對服務進行任何更改。</span><span class="sxs-lookup"><span data-stu-id="e272a-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="e272a-117">唯一的修改是啟動配置。</span><span class="sxs-lookup"><span data-stu-id="e272a-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="e272a-118">要啟用具有ASP.NET核心 gRPC 服務的 gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="e272a-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="e272a-119">添加對[Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)包的引用。</span><span class="sxs-lookup"><span data-stu-id="e272a-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="e272a-120">使用 gRPC-Web,透過`AddGrpcWeb`新增`UseGrpcWeb`與*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e272a-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="e272a-121">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e272a-121">The preceding code:</span></span>

* <span data-ttu-id="e272a-122">在路由後與終結點之前加入 gRPC-Web`UseGrpcWeb`中間件 。</span><span class="sxs-lookup"><span data-stu-id="e272a-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="e272a-123">指定方法`endpoints.MapGrpcService<GreeterService>()`支援 gRPC-Web。 `EnableGrpcWeb`</span><span class="sxs-lookup"><span data-stu-id="e272a-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="e272a-124">或者,通過添加到`services.AddGrpcWeb(o => o.GrpcWebEnabled = true);`配置服務,將所有服務配置為支援 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="e272a-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

> [!NOTE]
> <span data-ttu-id="e272a-125">在 .NET Core [3.x 中由 Http.sys 託管](xref:fundamentals/servers/httpsys)時,存在導致 gRPC-Web 失敗這一已知問題。</span><span class="sxs-lookup"><span data-stu-id="e272a-125">There is a known issue that causes gRPC-Web to fail when [hosted by Http.sys](xref:fundamentals/servers/httpsys) in .NET Core 3.x.</span></span>
>
> <span data-ttu-id="e272a-126">此處[提供了一](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202)種解決方法,用於使 gRPC-Web 在 Http.sys 上工作。</span><span class="sxs-lookup"><span data-stu-id="e272a-126">A workaround to get gRPC-Web working on Http.sys is available [here](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202).</span></span>

### <a name="grpc-web-and-cors"></a><span data-ttu-id="e272a-127">gRPC-Web 和 CORS</span><span class="sxs-lookup"><span data-stu-id="e272a-127">gRPC-Web and CORS</span></span>

<span data-ttu-id="e272a-128">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="e272a-128">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="e272a-129">此限制適用於使用瀏覽器應用進行 gRPC-Web 調用。</span><span class="sxs-lookup"><span data-stu-id="e272a-129">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="e272a-130">例如,所`https://www.contoso.com`服務的瀏覽器應用被阻止調用 託管在的`https://services.contoso.com`gRPC-Web 服務。</span><span class="sxs-lookup"><span data-stu-id="e272a-130">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="e272a-131">跨源資源分享 (CORS) 可用於放寬此限制。</span><span class="sxs-lookup"><span data-stu-id="e272a-131">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="e272a-132">要允許瀏覽器應用進行交叉源 gRPC-Web 調用,請[ASP.NET核心 中設置 CORS。](xref:security/cors)</span><span class="sxs-lookup"><span data-stu-id="e272a-132">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="e272a-133">使用內建的 CORS 支援,並使用 公開特定於<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>gRPC 的標頭。</span><span class="sxs-lookup"><span data-stu-id="e272a-133">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="e272a-134">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e272a-134">The preceding code:</span></span>

* <span data-ttu-id="e272a-135">呼叫`AddCors`以添加 CORS 服務並配置一個 CORS 策略,該策略公開 gRPC 特定的標頭。</span><span class="sxs-lookup"><span data-stu-id="e272a-135">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="e272a-136">在`UseCors`路由後和終結點之前添加 CORS 中間件的調用。</span><span class="sxs-lookup"><span data-stu-id="e272a-136">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="e272a-137">指定方法`endpoints.MapGrpcService<GreeterService>()`支援`RequiresCors`具有的 CORS。</span><span class="sxs-lookup"><span data-stu-id="e272a-137">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="e272a-138">從瀏覽器呼叫 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="e272a-138">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="e272a-139">瀏覽器應用可以使用 gRPC-Web 調用 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="e272a-139">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="e272a-140">從瀏覽器使用 gRPC-Web 呼叫 gRPC 服務時有一些要求和限制:</span><span class="sxs-lookup"><span data-stu-id="e272a-140">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="e272a-141">伺服器必須配置為支援 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="e272a-141">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="e272a-142">不支援用戶端流式處理和雙向流式處理調用。</span><span class="sxs-lookup"><span data-stu-id="e272a-142">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="e272a-143">支援伺服器流。</span><span class="sxs-lookup"><span data-stu-id="e272a-143">Server streaming is supported.</span></span>
* <span data-ttu-id="e272a-144">在不同的域上調用 gRPC 服務需要在伺服器上配置[CORS。](xref:security/cors)</span><span class="sxs-lookup"><span data-stu-id="e272a-144">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="e272a-145">JavaScript gRPC-Web 用戶端</span><span class="sxs-lookup"><span data-stu-id="e272a-145">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="e272a-146">有一個JavaScriptgRPC-Web用戶端。</span><span class="sxs-lookup"><span data-stu-id="e272a-146">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="e272a-147">有關如何使用 JavaScript 中的 gRPC-Web 的說明,請參閱[使用 gRPC-Web 撰寫 JavaScript 客戶端碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。</span><span class="sxs-lookup"><span data-stu-id="e272a-147">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="e272a-148">使用 .NET gRPC 用戶端設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="e272a-148">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="e272a-149">.NET gRPC 用戶端可以配置為進行 gRPC-Web 調用。</span><span class="sxs-lookup"><span data-stu-id="e272a-149">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="e272a-150">這對於[Blazor WebAssembly](xref:blazor/index#blazor-webassembly)應用很有用,這些應用託管在瀏覽器中,並且具有相同的 JavaScript 代碼的 HTTP 限制。</span><span class="sxs-lookup"><span data-stu-id="e272a-150">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="e272a-151">使用 .NET 用戶端呼叫 gRPC-Web[與 HTTP/2 gRPC 相同](xref:grpc/client)。</span><span class="sxs-lookup"><span data-stu-id="e272a-151">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="e272a-152">唯一的修改是如何創建通道。</span><span class="sxs-lookup"><span data-stu-id="e272a-152">The only modification is how the channel is created.</span></span>

<span data-ttu-id="e272a-153">要使用 gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="e272a-153">To use gRPC-Web:</span></span>

* <span data-ttu-id="e272a-154">添加對[Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)包的引用。</span><span class="sxs-lookup"><span data-stu-id="e272a-154">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="e272a-155">確保對[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)包的引用為 2.27.0 或更高。</span><span class="sxs-lookup"><span data-stu-id="e272a-155">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="e272a-156">將通道設定為使用`GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="e272a-156">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="e272a-157">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e272a-157">The preceding code:</span></span>

* <span data-ttu-id="e272a-158">配置通道以使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="e272a-158">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="e272a-159">創建用戶端並使用通道進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="e272a-159">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="e272a-160">建立`GrpcWebHandler`時具有以下設定選項:</span><span class="sxs-lookup"><span data-stu-id="e272a-160">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="e272a-161">**內部處理程式**:<xref:System.Net.Http.HttpMessageHandler>使 gRPC HTTP 要求`HttpClientHandler`的基礎,例如 。</span><span class="sxs-lookup"><span data-stu-id="e272a-161">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="e272a-162">**模式**:指定 gRPC`Content-Type``application/grpc-web`HTTP`application/grpc-web-text`請求 是 或的枚舉類型。</span><span class="sxs-lookup"><span data-stu-id="e272a-162">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="e272a-163">`GrpcWebMode.GrpcWeb`配置內容,無需編碼即可發送。</span><span class="sxs-lookup"><span data-stu-id="e272a-163">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="e272a-164">預設值。</span><span class="sxs-lookup"><span data-stu-id="e272a-164">Default value.</span></span>
    * <span data-ttu-id="e272a-165">`GrpcWebMode.GrpcWebText`將內容配置為基本編碼。</span><span class="sxs-lookup"><span data-stu-id="e272a-165">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="e272a-166">瀏覽器中的伺服器流式調用是必需的。</span><span class="sxs-lookup"><span data-stu-id="e272a-166">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="e272a-167">**HTTPVersion** `Version` : HTTP 協定用於在基礎 gRPC HTTP 請求上設定[HTTPRequestMessage.Version。](xref:System.Net.Http.HttpRequestMessage.Version)</span><span class="sxs-lookup"><span data-stu-id="e272a-167">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="e272a-168">gRPC-Web 不需要特定版本,除非指定,否則不會覆蓋預設值。</span><span class="sxs-lookup"><span data-stu-id="e272a-168">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e272a-169">生成的 gRPC 用戶端具有用於調用一元方法的同步和非同步方法。</span><span class="sxs-lookup"><span data-stu-id="e272a-169">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="e272a-170">例如,`SayHello`是同步`SayHelloAsync`和非同步。</span><span class="sxs-lookup"><span data-stu-id="e272a-170">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="e272a-171">在 Blazor WebAssembly 應用中調用同步方法將導致應用無回應。</span><span class="sxs-lookup"><span data-stu-id="e272a-171">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="e272a-172">在 Blazor WebAssembly 中必須始終使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="e272a-172">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e272a-173">其他資源</span><span class="sxs-lookup"><span data-stu-id="e272a-173">Additional resources</span></span>

* [<span data-ttu-id="e272a-174">GRPC</span><span class="sxs-lookup"><span data-stu-id="e272a-174">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
