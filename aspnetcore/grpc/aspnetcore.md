---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 瞭解使用 ASP.NET Core 撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 2507ce6df05403cb19e8bfa2565d410d6140b144
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925082"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="1d92e-103">搭配 ASP.NET Core 的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="1d92e-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="1d92e-104">本檔說明如何使用 ASP.NET Core 開始使用 gRPC services。</span><span class="sxs-lookup"><span data-stu-id="1d92e-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d92e-105">必要條件</span><span class="sxs-lookup"><span data-stu-id="1d92e-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1d92e-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d92e-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1d92e-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1d92e-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1d92e-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1d92e-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="1d92e-109">開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="1d92e-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="1d92e-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1d92e-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1d92e-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d92e-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1d92e-112">如需如何建立 gRPC 專案的詳細指示, 請參閱[開始使用 gRPC services](xref:tutorials/grpc/grpc-start) 。</span><span class="sxs-lookup"><span data-stu-id="1d92e-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="1d92e-113">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1d92e-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="1d92e-114">從命令列執行 `dotnet new grpc -o GrpcGreeter`。</span><span class="sxs-lookup"><span data-stu-id="1d92e-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="1d92e-115">將 gRPC 服務新增至 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="1d92e-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="1d92e-116">gRPC 需要[gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)套件。</span><span class="sxs-lookup"><span data-stu-id="1d92e-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="1d92e-117">設定 gRPC</span><span class="sxs-lookup"><span data-stu-id="1d92e-117">Configure gRPC</span></span>

<span data-ttu-id="1d92e-118">在 *Startup.cs* 中：</span><span class="sxs-lookup"><span data-stu-id="1d92e-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="1d92e-119">gRPC 是以`AddGrpc`方法啟用。</span><span class="sxs-lookup"><span data-stu-id="1d92e-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="1d92e-120">每個 gRPC 服務都會透過`MapGrpcService`方法加入至路由管線。</span><span class="sxs-lookup"><span data-stu-id="1d92e-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="1d92e-121">ASP.NET Core 中介軟體和功能共用路由管線, 因此可以將應用程式設定為提供額外的要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="1d92e-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="1d92e-122">其他要求處理常式 (例如 MVC 控制器) 會與已設定的 gRPC 服務平行處理。</span><span class="sxs-lookup"><span data-stu-id="1d92e-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="1d92e-123">設定 Kestrel</span><span class="sxs-lookup"><span data-stu-id="1d92e-123">Configure Kestrel</span></span>

<span data-ttu-id="1d92e-124">Kestrel gRPC 端點：</span><span class="sxs-lookup"><span data-stu-id="1d92e-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="1d92e-125">需要 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="1d92e-125">Require HTTP/2.</span></span>
* <span data-ttu-id="1d92e-126">應使用[傳輸層安全性（TLS）](https://tools.ietf.org/html/rfc5246)來保護。</span><span class="sxs-lookup"><span data-stu-id="1d92e-126">Should be secured with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span>

#### <a name="http2"></a><span data-ttu-id="1d92e-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="1d92e-127">HTTP/2</span></span>

<span data-ttu-id="1d92e-128">gRPC 需要 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="1d92e-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="1d92e-129">ASP.NET Core 的 gRPC 會驗證[HttpRequest。通訊協定](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)為`HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="1d92e-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="1d92e-130">Kestrel 支援大多數新式作業系統上的[HTTP/2](xref:fundamentals/servers/kestrel#http2-support) 。</span><span class="sxs-lookup"><span data-stu-id="1d92e-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="1d92e-131">根據預設，Kestrel 端點會設定為支援 HTTP/1.1 和 HTTP/2 連接。</span><span class="sxs-lookup"><span data-stu-id="1d92e-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="tls"></a><span data-ttu-id="1d92e-132">TLS</span><span class="sxs-lookup"><span data-stu-id="1d92e-132">TLS</span></span>

<span data-ttu-id="1d92e-133">用於 gRPC 的 Kestrel 端點應使用 TLS 來保護。</span><span class="sxs-lookup"><span data-stu-id="1d92e-133">Kestrel endpoints used for gRPC should be secured with TLS.</span></span> <span data-ttu-id="1d92e-134">在開發期間，會在 ASP.NET Core 開發憑證存在`https://localhost:5001`時，自動建立以 TLS 保護的端點。</span><span class="sxs-lookup"><span data-stu-id="1d92e-134">In development, an endpoint secured with TLS is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="1d92e-135">不需要進行任何設定。</span><span class="sxs-lookup"><span data-stu-id="1d92e-135">No configuration is required.</span></span> <span data-ttu-id="1d92e-136">`https`前置詞會驗證 Kestrel 端點是否使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="1d92e-136">An `https` prefix verifies the Kestrel endpoint is using TLS.</span></span>

<span data-ttu-id="1d92e-137">在生產環境中，必須明確設定 TLS。</span><span class="sxs-lookup"><span data-stu-id="1d92e-137">In production, TLS must be explicitly configured.</span></span> <span data-ttu-id="1d92e-138">在下列*appsettings*範例中，會提供使用 TLS 保護的 HTTP/2 端點：</span><span class="sxs-lookup"><span data-stu-id="1d92e-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with TLS is provided:</span></span>

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

<span data-ttu-id="1d92e-139">或者，您也可以在*Program.cs*中設定 Kestrel 端點：</span><span class="sxs-lookup"><span data-stu-id="1d92e-139">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a><span data-ttu-id="1d92e-140">通訊協定協商</span><span class="sxs-lookup"><span data-stu-id="1d92e-140">Protocol negotiation</span></span>

<span data-ttu-id="1d92e-141">TLS 用於保護通訊安全。</span><span class="sxs-lookup"><span data-stu-id="1d92e-141">TLS is used for more than securing communication.</span></span> <span data-ttu-id="1d92e-142">當端點支援多個通訊協定時，會使用 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握來協調用戶端與伺服器之間的連接通訊協定。</span><span class="sxs-lookup"><span data-stu-id="1d92e-142">The TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake is used to negotiate the connection protocol between the client and the server when an endpoint supports multiple protocols.</span></span> <span data-ttu-id="1d92e-143">此協商會判斷連接使用的是 HTTP/1.1 或 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="1d92e-143">This negotiation determines whether the connection uses HTTP/1.1 or HTTP/2.</span></span>

<span data-ttu-id="1d92e-144">如果在沒有 TLS 的情況下設定 HTTP/2 端點，則端點的[listenoptions 來](xref:fundamentals/servers/kestrel#listenoptionsprotocols)必須設定為`HttpProtocols.Http2`。</span><span class="sxs-lookup"><span data-stu-id="1d92e-144">If an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="1d92e-145">具有多個通訊協定（例如）的`HttpProtocols.Http1AndHttp2`端點不能在沒有 TLS 的情況下使用，因為沒有任何協商。</span><span class="sxs-lookup"><span data-stu-id="1d92e-145">An endpoint with multiple protocols (for example, `HttpProtocols.Http1AndHttp2`) can't be used without TLS because there is no negotiation.</span></span> <span data-ttu-id="1d92e-146">不安全端點的所有連接預設為 HTTP/1.1，而 gRPC 呼叫會失敗。</span><span class="sxs-lookup"><span data-stu-id="1d92e-146">All connections to the unsecured endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="1d92e-147">如需使用 Kestrel 啟用 HTTP/2 和 TLS 的詳細資訊，請參閱[Kestrel 端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定。</span><span class="sxs-lookup"><span data-stu-id="1d92e-147">For more information on enabling HTTP/2 and TLS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="1d92e-148">macOS 不支援具有 TLS 的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="1d92e-148">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="1d92e-149">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="1d92e-149">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="1d92e-150">如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="1d92e-150">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="1d92e-151">與 ASP.NET Core Api 整合</span><span class="sxs-lookup"><span data-stu-id="1d92e-151">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="1d92e-152">gRPC 服務具有 ASP.NET Core 功能的完整存取權, 例如相依性[插入](xref:fundamentals/dependency-injection)(DI) 和[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="1d92e-152">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="1d92e-153">例如, 服務執行可以透過此函式從 DI 容器解析記錄器服務:</span><span class="sxs-lookup"><span data-stu-id="1d92e-153">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="1d92e-154">根據預設, gRPC 服務執行可以使用任何存留期 (單一、限定範圍或暫時性) 來解析其他 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="1d92e-154">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="1d92e-155">解析 gRPC 方法中的 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="1d92e-155">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="1d92e-156">GRPC API 可讓您存取某些 HTTP/2 訊息資料, 例如方法、主機、標頭和結尾。</span><span class="sxs-lookup"><span data-stu-id="1d92e-156">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="1d92e-157">存取是透過傳遞`ServerCallContext`至每個 gRPC 方法的引數:</span><span class="sxs-lookup"><span data-stu-id="1d92e-157">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="1d92e-158">`ServerCallContext`並未提供所有 ASP.NET api `HttpContext`的完整存取權。</span><span class="sxs-lookup"><span data-stu-id="1d92e-158">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="1d92e-159">擴充方法會提供完整的`HttpContext`存取權, 以代表 ASP.NET api 中的基礎 HTTP/2 訊息: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="1d92e-159">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="1d92e-160">其他資源</span><span class="sxs-lookup"><span data-stu-id="1d92e-160">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
