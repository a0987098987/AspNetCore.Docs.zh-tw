---
title: 在瀏覽器應用程式中使用 gRPC
author: jamesnk
description: 瞭解如何在ASP.NET酷睿配置 gRPC 服務,以便使用 gRPC-Web 從瀏覽器應用調用 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 04/15/2020
uid: grpc/browser
ms.openlocfilehash: a20e604488b1fb919f18932599ba690bfa308f0c
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440762"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="5cda4-103">在瀏覽器應用程式中使用 gRPC</span><span class="sxs-lookup"><span data-stu-id="5cda4-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="5cda4-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="5cda4-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5cda4-105">**.NET 中的 gRPC-Web 支援是實驗性的**</span><span class="sxs-lookup"><span data-stu-id="5cda4-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="5cda4-106">gRPC-Web for .NET 是一個實驗專案,而不是一個承諾的產品。</span><span class="sxs-lookup"><span data-stu-id="5cda4-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="5cda4-107">我們想要：</span><span class="sxs-lookup"><span data-stu-id="5cda4-107">We want to:</span></span>
>
> * <span data-ttu-id="5cda4-108">測試我們實現 gRPC-Web 的方法有效。</span><span class="sxs-lookup"><span data-stu-id="5cda4-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="5cda4-109">與通過代理設定 gRPC-Web 的傳統方式相比,獲取此方法對 .NET 開發人員是否有用的回饋。</span><span class="sxs-lookup"><span data-stu-id="5cda4-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="5cda4-110">請留下回饋,[https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet)以確保我們建構開發者喜歡並高效使用的東西。</span><span class="sxs-lookup"><span data-stu-id="5cda4-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="5cda4-111">無法從基於瀏覽器的應用調用 HTTP/2 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5cda4-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="5cda4-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)是一種協定,允許瀏覽器 JavaScript 和 Blazor 應用調用 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5cda4-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="5cda4-113">本文介紹如何在 .NET 核心中使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="5cda4-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="grpc-web-in-aspnet-core-vs-envoy"></a><span data-ttu-id="5cda4-114">gRPC-Web ASP.NET核心與特使</span><span class="sxs-lookup"><span data-stu-id="5cda4-114">gRPC-Web in ASP.NET Core vs. Envoy</span></span>

<span data-ttu-id="5cda4-115">有關如何將 gRPC-Web 新增到 ASP.NET 核心應用有兩種選擇:</span><span class="sxs-lookup"><span data-stu-id="5cda4-115">There are two choices for how to add gRPC-Web to an ASP.NET Core app:</span></span>

* <span data-ttu-id="5cda4-116">在 ASP.NET核心中支援 gRPC-Web 與 gRPC HTTP/2 一起。</span><span class="sxs-lookup"><span data-stu-id="5cda4-116">Support gRPC-Web alongside gRPC HTTP/2 in ASP.NET Core.</span></span> <span data-ttu-id="5cda4-117">此選項使用`Grpc.AspNetCore.Web`包提供的中間件。</span><span class="sxs-lookup"><span data-stu-id="5cda4-117">This option uses middleware provided by the `Grpc.AspNetCore.Web` package.</span></span>
* <span data-ttu-id="5cda4-118">使用[特使代理的](https://www.envoyproxy.io/)gRPC-Web 支援將 gRPC-Web 轉換為 gRPC HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="5cda4-118">Use the [Envoy proxy's](https://www.envoyproxy.io/) gRPC-Web support to translate gRPC-Web to gRPC HTTP/2.</span></span> <span data-ttu-id="5cda4-119">然後,翻譯的呼叫將轉發到ASP.NET核心應用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-119">The translated call is then forwarded onto the ASP.NET Core app.</span></span>

<span data-ttu-id="5cda4-120">每種方法都有優缺點。</span><span class="sxs-lookup"><span data-stu-id="5cda4-120">There are pros and cons to each approach.</span></span> <span data-ttu-id="5cda4-121">如果您已經在應用環境中使用特使作為代理,則使用它提供 gRPC-Web 支援可能也有意義。</span><span class="sxs-lookup"><span data-stu-id="5cda4-121">If you're already using Envoy as a proxy in your app's environment, it might make sense to also use it to provide gRPC-Web support.</span></span> <span data-ttu-id="5cda4-122">如果想要一個簡單的 gRPC-Web 解決方案,只需要ASP.NET`Grpc.AspNetCore.Web`核心, 這是一個不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="5cda4-122">If you want a simple solution for gRPC-Web that only requires ASP.NET Core, `Grpc.AspNetCore.Web` is a good choice.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="5cda4-123">在 ASP.NET核心中設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="5cda4-123">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="5cda4-124">ASP.NET核心中託管的 gRPC 服務可以配置為支援 gRPC-Web 以及 HTTP/2 gRPC。</span><span class="sxs-lookup"><span data-stu-id="5cda4-124">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="5cda4-125">gRPC-Web 不需要對服務進行任何更改。</span><span class="sxs-lookup"><span data-stu-id="5cda4-125">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="5cda4-126">唯一的修改是啟動配置。</span><span class="sxs-lookup"><span data-stu-id="5cda4-126">The only modification is startup configuration.</span></span>

<span data-ttu-id="5cda4-127">要啟用具有ASP.NET核心 gRPC 服務的 gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="5cda4-127">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="5cda4-128">添加對[Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)包的引用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-128">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="5cda4-129">使用 gRPC-Web,透過`AddGrpcWeb`新增`UseGrpcWeb`與*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5cda4-129">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="5cda4-130">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cda4-130">The preceding code:</span></span>

* <span data-ttu-id="5cda4-131">在路由後與終結點之前加入 gRPC-Web`UseGrpcWeb`中間件 。</span><span class="sxs-lookup"><span data-stu-id="5cda4-131">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="5cda4-132">指定方法`endpoints.MapGrpcService<GreeterService>()`支援 gRPC-Web。 `EnableGrpcWeb`</span><span class="sxs-lookup"><span data-stu-id="5cda4-132">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="5cda4-133">或者,通過添加到`services.AddGrpcWeb(o => o.GrpcWebEnabled = true);`配置服務,將所有服務配置為支援 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="5cda4-133">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

> [!NOTE]
> <span data-ttu-id="5cda4-134">在 .NET Core [3.x 中由 Http.sys 託管](xref:fundamentals/servers/httpsys)時,存在導致 gRPC-Web 失敗這一已知問題。</span><span class="sxs-lookup"><span data-stu-id="5cda4-134">There is a known issue that causes gRPC-Web to fail when [hosted by Http.sys](xref:fundamentals/servers/httpsys) in .NET Core 3.x.</span></span>
>
> <span data-ttu-id="5cda4-135">此處[提供了一](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202)種解決方法,用於使 gRPC-Web 在 Http.sys 上工作。</span><span class="sxs-lookup"><span data-stu-id="5cda4-135">A workaround to get gRPC-Web working on Http.sys is available [here](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202).</span></span>

### <a name="grpc-web-and-cors"></a><span data-ttu-id="5cda4-136">gRPC-Web 和 CORS</span><span class="sxs-lookup"><span data-stu-id="5cda4-136">gRPC-Web and CORS</span></span>

<span data-ttu-id="5cda4-137">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="5cda4-137">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="5cda4-138">此限制適用於使用瀏覽器應用進行 gRPC-Web 調用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-138">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="5cda4-139">例如,所`https://www.contoso.com`服務的瀏覽器應用被阻止調用 託管在的`https://services.contoso.com`gRPC-Web 服務。</span><span class="sxs-lookup"><span data-stu-id="5cda4-139">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="5cda4-140">跨源資源分享 (CORS) 可用於放寬此限制。</span><span class="sxs-lookup"><span data-stu-id="5cda4-140">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="5cda4-141">要允許瀏覽器應用進行交叉源 gRPC-Web 調用,請[ASP.NET核心 中設置 CORS。](xref:security/cors)</span><span class="sxs-lookup"><span data-stu-id="5cda4-141">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="5cda4-142">使用內建的 CORS 支援,並使用 公開特定於<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>gRPC 的標頭。</span><span class="sxs-lookup"><span data-stu-id="5cda4-142">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="5cda4-143">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cda4-143">The preceding code:</span></span>

* <span data-ttu-id="5cda4-144">呼叫`AddCors`以添加 CORS 服務並配置一個 CORS 策略,該策略公開 gRPC 特定的標頭。</span><span class="sxs-lookup"><span data-stu-id="5cda4-144">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="5cda4-145">在`UseCors`路由後和終結點之前添加 CORS 中間件的調用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-145">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="5cda4-146">指定方法`endpoints.MapGrpcService<GreeterService>()`支援`RequiresCors`具有的 CORS。</span><span class="sxs-lookup"><span data-stu-id="5cda4-146">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="5cda4-147">從瀏覽器呼叫 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="5cda4-147">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="5cda4-148">瀏覽器應用可以使用 gRPC-Web 調用 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5cda4-148">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="5cda4-149">從瀏覽器使用 gRPC-Web 呼叫 gRPC 服務時有一些要求和限制:</span><span class="sxs-lookup"><span data-stu-id="5cda4-149">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="5cda4-150">伺服器必須配置為支援 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="5cda4-150">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="5cda4-151">不支援用戶端流式處理和雙向流式處理調用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-151">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="5cda4-152">支援伺服器流。</span><span class="sxs-lookup"><span data-stu-id="5cda4-152">Server streaming is supported.</span></span>
* <span data-ttu-id="5cda4-153">在不同的域上調用 gRPC 服務需要在伺服器上配置[CORS。](xref:security/cors)</span><span class="sxs-lookup"><span data-stu-id="5cda4-153">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="5cda4-154">JavaScript gRPC-Web 用戶端</span><span class="sxs-lookup"><span data-stu-id="5cda4-154">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="5cda4-155">有一個JavaScriptgRPC-Web用戶端。</span><span class="sxs-lookup"><span data-stu-id="5cda4-155">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="5cda4-156">有關如何使用 JavaScript 中的 gRPC-Web 的說明,請參閱[使用 gRPC-Web 撰寫 JavaScript 客戶端碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。</span><span class="sxs-lookup"><span data-stu-id="5cda4-156">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="5cda4-157">使用 .NET gRPC 用戶端設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="5cda4-157">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="5cda4-158">.NET gRPC 用戶端可以配置為進行 gRPC-Web 調用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-158">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="5cda4-159">這對於[Blazor WebAssembly](xref:blazor/index#blazor-webassembly)應用很有用,這些應用託管在瀏覽器中,並且具有相同的 JavaScript 代碼的 HTTP 限制。</span><span class="sxs-lookup"><span data-stu-id="5cda4-159">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="5cda4-160">使用 .NET 用戶端呼叫 gRPC-Web[與 HTTP/2 gRPC 相同](xref:grpc/client)。</span><span class="sxs-lookup"><span data-stu-id="5cda4-160">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="5cda4-161">唯一的修改是如何創建通道。</span><span class="sxs-lookup"><span data-stu-id="5cda4-161">The only modification is how the channel is created.</span></span>

<span data-ttu-id="5cda4-162">要使用 gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="5cda4-162">To use gRPC-Web:</span></span>

* <span data-ttu-id="5cda4-163">添加對[Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)包的引用。</span><span class="sxs-lookup"><span data-stu-id="5cda4-163">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="5cda4-164">確保對[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)包的引用為 2.27.0 或更高。</span><span class="sxs-lookup"><span data-stu-id="5cda4-164">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="5cda4-165">將通道設定為使用`GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="5cda4-165">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="5cda4-166">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="5cda4-166">The preceding code:</span></span>

* <span data-ttu-id="5cda4-167">配置通道以使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="5cda4-167">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="5cda4-168">創建用戶端並使用通道進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="5cda4-168">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="5cda4-169">建立`GrpcWebHandler`時具有以下設定選項:</span><span class="sxs-lookup"><span data-stu-id="5cda4-169">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="5cda4-170">**內部處理程式**:<xref:System.Net.Http.HttpMessageHandler>使 gRPC HTTP 要求`HttpClientHandler`的基礎,例如 。</span><span class="sxs-lookup"><span data-stu-id="5cda4-170">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="5cda4-171">**模式**:指定 gRPC`Content-Type``application/grpc-web`HTTP`application/grpc-web-text`請求 是 或的枚舉類型。</span><span class="sxs-lookup"><span data-stu-id="5cda4-171">**Mode**: An enumeration type that specifies whether the gRPC HTTP request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="5cda4-172">`GrpcWebMode.GrpcWeb`配置內容,無需編碼即可發送。</span><span class="sxs-lookup"><span data-stu-id="5cda4-172">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="5cda4-173">預設值。</span><span class="sxs-lookup"><span data-stu-id="5cda4-173">Default value.</span></span>
    * <span data-ttu-id="5cda4-174">`GrpcWebMode.GrpcWebText`將內容配置為基本編碼。</span><span class="sxs-lookup"><span data-stu-id="5cda4-174">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="5cda4-175">瀏覽器中的伺服器流式調用是必需的。</span><span class="sxs-lookup"><span data-stu-id="5cda4-175">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="5cda4-176">**HTTPVersion** `Version` : HTTP 協定用於在基礎 gRPC HTTP 請求上設定[HTTPRequestMessage.Version。](xref:System.Net.Http.HttpRequestMessage.Version)</span><span class="sxs-lookup"><span data-stu-id="5cda4-176">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="5cda4-177">gRPC-Web 不需要特定版本,除非指定,否則不會覆蓋預設值。</span><span class="sxs-lookup"><span data-stu-id="5cda4-177">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5cda4-178">生成的 gRPC 用戶端具有用於調用一元方法的同步和非同步方法。</span><span class="sxs-lookup"><span data-stu-id="5cda4-178">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="5cda4-179">例如,`SayHello`是同步`SayHelloAsync`和非同步。</span><span class="sxs-lookup"><span data-stu-id="5cda4-179">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="5cda4-180">在 Blazor WebAssembly 應用中調用同步方法將導致應用無回應。</span><span class="sxs-lookup"><span data-stu-id="5cda4-180">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="5cda4-181">在 Blazor WebAssembly 中必須始終使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="5cda4-181">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5cda4-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="5cda4-182">Additional resources</span></span>

* [<span data-ttu-id="5cda4-183">GRPC</span><span class="sxs-lookup"><span data-stu-id="5cda4-183">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
