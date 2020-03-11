---
title: 在瀏覽器應用程式中使用 gRPC
author: jamesnk
description: 瞭解如何在 ASP.NET Core 上設定 gRPC 服務，以使用 gRPC-Web 從瀏覽器應用程式呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 3beeffc26ffd3c2dc85bfc22a46d97d5fd78d3d0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664196"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="39b30-103">在瀏覽器應用程式中使用 gRPC</span><span class="sxs-lookup"><span data-stu-id="39b30-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="39b30-104">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="39b30-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="39b30-105">**gRPC-.NET 中的 Web 支援為實驗性**</span><span class="sxs-lookup"><span data-stu-id="39b30-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="39b30-106">gRPC-適用于 .NET 的 Web 是實驗性專案，而不是認可的產品。</span><span class="sxs-lookup"><span data-stu-id="39b30-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="39b30-107">我們想要：</span><span class="sxs-lookup"><span data-stu-id="39b30-107">We want to:</span></span>
>
> * <span data-ttu-id="39b30-108">測試我們用來執行 gRPC 的方法-Web works。</span><span class="sxs-lookup"><span data-stu-id="39b30-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="39b30-109">如果這種方法對 .NET 開發人員來說很有用，請取得意見反應，相較于傳統的方法是透過 proxy 設定 gRPC Web。</span><span class="sxs-lookup"><span data-stu-id="39b30-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="39b30-110">請在[https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet)留下意見反應，以確保我們建立了開發人員所需的專案，並提高生產力。</span><span class="sxs-lookup"><span data-stu-id="39b30-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="39b30-111">您無法從以瀏覽器為基礎的應用程式呼叫 HTTP/2 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="39b30-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="39b30-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)是一種通訊協定，可讓瀏覽器 JavaScript 和 Blazor 應用程式呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="39b30-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="39b30-113">本文說明如何在 .NET Core 中使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="39b30-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="39b30-114">在 ASP.NET Core 中設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="39b30-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="39b30-115">ASP.NET Core 中裝載的 gRPC 服務可以設定為支援 gRPC-Web 與 HTTP/2 gRPC。</span><span class="sxs-lookup"><span data-stu-id="39b30-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="39b30-116">gRPC-Web 不需要對服務進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="39b30-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="39b30-117">唯一的修改是啟動設定。</span><span class="sxs-lookup"><span data-stu-id="39b30-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="39b30-118">若要使用 ASP.NET Core gRPC 服務啟用 gRPC-Web：</span><span class="sxs-lookup"><span data-stu-id="39b30-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="39b30-119">新增對[Grpc. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web)封裝的參考。</span><span class="sxs-lookup"><span data-stu-id="39b30-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="39b30-120">藉由將 `AddGrpcWeb` 和 `UseGrpcWeb` 新增至*Startup.cs*，將應用程式設定為使用 gRPC：</span><span class="sxs-lookup"><span data-stu-id="39b30-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="39b30-121">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="39b30-121">The preceding code:</span></span>

* <span data-ttu-id="39b30-122">在路由和結束端點之後，新增 gRPC Web 中介軟體、`UseGrpcWeb`。</span><span class="sxs-lookup"><span data-stu-id="39b30-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="39b30-123">指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援具有 `EnableGrpcWeb`的 gRPC Web。</span><span class="sxs-lookup"><span data-stu-id="39b30-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="39b30-124">或者，將 `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` 新增至 ConfigureServices，以設定所有服務以支援 gRPC Web。</span><span class="sxs-lookup"><span data-stu-id="39b30-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

### <a name="grpc-web-and-cors"></a><span data-ttu-id="39b30-125">gRPC-Web 和 CORS</span><span class="sxs-lookup"><span data-stu-id="39b30-125">gRPC-Web and CORS</span></span>

<span data-ttu-id="39b30-126">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="39b30-126">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="39b30-127">這種限制適用于使用瀏覽器應用程式進行 gRPC Web 呼叫。</span><span class="sxs-lookup"><span data-stu-id="39b30-127">This restriction applies to making gRPC-Web calls with browser apps.</span></span> <span data-ttu-id="39b30-128">例如，`https://www.contoso.com` 所提供的瀏覽器應用程式遭到封鎖，無法呼叫裝載于 `https://services.contoso.com`的 gRPC Web 服務。</span><span class="sxs-lookup"><span data-stu-id="39b30-128">For example, a browser app served by `https://www.contoso.com` is blocked from calling gRPC-Web services hosted on `https://services.contoso.com`.</span></span> <span data-ttu-id="39b30-129">跨原始來源資源分享（CORS）可以用來放寬這種限制。</span><span class="sxs-lookup"><span data-stu-id="39b30-129">Cross Origin Resource Sharing (CORS) can be used to relax this restriction.</span></span>

<span data-ttu-id="39b30-130">若要允許您的瀏覽器應用程式進行跨原始來源的 gRPC 呼叫，請[在 ASP.NET Core 中設定 CORS](xref:security/cors)。</span><span class="sxs-lookup"><span data-stu-id="39b30-130">To allow your browser app to make cross-origin gRPC-Web calls, set up [CORS in ASP.NET Core](xref:security/cors).</span></span> <span data-ttu-id="39b30-131">使用內建的 CORS 支援，並使用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>公開 gRPC 特有的標頭。</span><span class="sxs-lookup"><span data-stu-id="39b30-131">Use the built-in CORS support, and expose gRPC-specific headers with <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.</span></span>

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

<span data-ttu-id="39b30-132">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="39b30-132">The preceding code:</span></span>

* <span data-ttu-id="39b30-133">呼叫 `AddCors` 來新增 CORS 服務，並設定會公開 gRPC 特定標頭的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="39b30-133">Calls `AddCors` to add CORS services and configures a CORS policy that exposes gRPC-specific headers.</span></span>
* <span data-ttu-id="39b30-134">呼叫 `UseCors` 在路由和結束端點之後新增 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="39b30-134">Calls `UseCors` to add the CORS middleware after routing and before endpoints.</span></span>
* <span data-ttu-id="39b30-135">指定 `endpoints.MapGrpcService<GreeterService>()` 方法支援具有 `RequiresCors`的 CORS。</span><span class="sxs-lookup"><span data-stu-id="39b30-135">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports CORS with `RequiresCors`.</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="39b30-136">從瀏覽器呼叫 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="39b30-136">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="39b30-137">瀏覽器應用程式可以使用 gRPC 來呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="39b30-137">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="39b30-138">從瀏覽器呼叫具有 gRPC 的 gRPC 服務時，有一些需求和限制：</span><span class="sxs-lookup"><span data-stu-id="39b30-138">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="39b30-139">伺服器必須已設定為支援 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="39b30-139">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="39b30-140">不支援用戶端串流和雙向串流呼叫。</span><span class="sxs-lookup"><span data-stu-id="39b30-140">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="39b30-141">支援伺服器串流。</span><span class="sxs-lookup"><span data-stu-id="39b30-141">Server streaming is supported.</span></span>
* <span data-ttu-id="39b30-142">若要在不同的網域上呼叫 gRPC 服務，必須在伺服器上設定[CORS](xref:security/cors) 。</span><span class="sxs-lookup"><span data-stu-id="39b30-142">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="39b30-143">JavaScript gRPC-Web 用戶端</span><span class="sxs-lookup"><span data-stu-id="39b30-143">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="39b30-144">有一個 JavaScript gRPC Web 用戶端。</span><span class="sxs-lookup"><span data-stu-id="39b30-144">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="39b30-145">如需如何從 JavaScript 使用 gRPC Web 的指示，請參閱[使用 gRPC 撰寫 javascript 用戶端程式代碼](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)。</span><span class="sxs-lookup"><span data-stu-id="39b30-145">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="39b30-146">使用 .NET gRPC 用戶端設定 gRPC-Web</span><span class="sxs-lookup"><span data-stu-id="39b30-146">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="39b30-147">您可以設定 .NET gRPC 用戶端進行 gRPC-Web 呼叫。</span><span class="sxs-lookup"><span data-stu-id="39b30-147">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="39b30-148">這適用于[Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps，這些應用程式裝載于瀏覽器中，並具有 JavaScript 程式碼的相同 HTTP 限制。</span><span class="sxs-lookup"><span data-stu-id="39b30-148">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="39b30-149">使用 .NET 用戶端呼叫 gRPC Web 與[HTTP/2 gRPC 相同](xref:grpc/client)。</span><span class="sxs-lookup"><span data-stu-id="39b30-149">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="39b30-150">唯一的修改是建立通道的方式。</span><span class="sxs-lookup"><span data-stu-id="39b30-150">The only modification is how the channel is created.</span></span>

<span data-ttu-id="39b30-151">若要使用 gRPC-Web：</span><span class="sxs-lookup"><span data-stu-id="39b30-151">To use gRPC-Web:</span></span>

* <span data-ttu-id="39b30-152">將參考新增至[Grpc 的 .net. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web)封裝。</span><span class="sxs-lookup"><span data-stu-id="39b30-152">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="39b30-153">請確定[Grpc .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)封裝的參考已2.27.0 或更高。</span><span class="sxs-lookup"><span data-stu-id="39b30-153">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="39b30-154">設定通道以使用 `GrpcWebHandler`：</span><span class="sxs-lookup"><span data-stu-id="39b30-154">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="39b30-155">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="39b30-155">The preceding code:</span></span>

* <span data-ttu-id="39b30-156">設定通道以使用 gRPC-Web。</span><span class="sxs-lookup"><span data-stu-id="39b30-156">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="39b30-157">建立用戶端，並使用通道進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="39b30-157">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="39b30-158">建立時，`GrpcWebHandler` 具有下列設定選項：</span><span class="sxs-lookup"><span data-stu-id="39b30-158">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="39b30-159">**InnerHandler**：建立 gRPC HTTP 要求的基礎 <xref:System.Net.Http.HttpMessageHandler>，例如 `HttpClientHandler`。</span><span class="sxs-lookup"><span data-stu-id="39b30-159">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="39b30-160">**模式**：指定 gRPC HTTP 要求要求 `Content-Type` 是否 `application/grpc-web` 或 `application/grpc-web-text`的列舉類型。</span><span class="sxs-lookup"><span data-stu-id="39b30-160">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="39b30-161">`GrpcWebMode.GrpcWeb` 會設定要在不進行編碼的情況下傳送的內容。</span><span class="sxs-lookup"><span data-stu-id="39b30-161">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="39b30-162">預設值。</span><span class="sxs-lookup"><span data-stu-id="39b30-162">Default value.</span></span>
    * <span data-ttu-id="39b30-163">`GrpcWebMode.GrpcWebText` 會將內容設定為以 base64 編碼。</span><span class="sxs-lookup"><span data-stu-id="39b30-163">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="39b30-164">瀏覽器中的伺服器串流呼叫所需。</span><span class="sxs-lookup"><span data-stu-id="39b30-164">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="39b30-165">**HttpVersion**：用來在基礎 gRPC HTTP 要求上設定[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Version)的 HTTP 通訊協定 `Version`。</span><span class="sxs-lookup"><span data-stu-id="39b30-165">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="39b30-166">gRPC-Web 不需要特定版本，除非指定，否則不會覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="39b30-166">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="39b30-167">產生的 gRPC 用戶端具有呼叫一元方法的同步和非同步方法。</span><span class="sxs-lookup"><span data-stu-id="39b30-167">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="39b30-168">例如，`SayHello` 是同步處理，`SayHelloAsync` 是非同步。</span><span class="sxs-lookup"><span data-stu-id="39b30-168">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="39b30-169">在 Blazor WebAssembly 應用程式中呼叫同步方法，會導致應用程式變得沒有回應。</span><span class="sxs-lookup"><span data-stu-id="39b30-169">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="39b30-170">非同步方法必須一律用於 Blazor WebAssembly 中。</span><span class="sxs-lookup"><span data-stu-id="39b30-170">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39b30-171">其他資源</span><span class="sxs-lookup"><span data-stu-id="39b30-171">Additional resources</span></span>

* [<span data-ttu-id="39b30-172">適用于 Web 用戶端的 gRPC GitHub 專案</span><span class="sxs-lookup"><span data-stu-id="39b30-172">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
