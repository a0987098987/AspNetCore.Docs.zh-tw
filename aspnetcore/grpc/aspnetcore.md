---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 瞭解使用 ASP.NET Core 撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862932"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="92c7f-103">搭配 ASP.NET Core 的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="92c7f-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="92c7f-104">本檔說明如何使用 ASP.NET Core 開始使用 gRPC services。</span><span class="sxs-lookup"><span data-stu-id="92c7f-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92c7f-105">必要條件</span><span class="sxs-lookup"><span data-stu-id="92c7f-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="92c7f-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92c7f-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="92c7f-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="92c7f-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="92c7f-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="92c7f-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="92c7f-109">開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="92c7f-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="92c7f-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="92c7f-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="92c7f-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92c7f-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="92c7f-112">如需如何建立 gRPC 專案的詳細指示, 請參閱[開始使用 gRPC services](xref:tutorials/grpc/grpc-start) 。</span><span class="sxs-lookup"><span data-stu-id="92c7f-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="92c7f-113">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="92c7f-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="92c7f-114">從命令列執行 `dotnet new grpc -o GrpcGreeter`。</span><span class="sxs-lookup"><span data-stu-id="92c7f-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="92c7f-115">將 gRPC 服務新增至 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="92c7f-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="92c7f-116">gRPC 需要[gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)套件。</span><span class="sxs-lookup"><span data-stu-id="92c7f-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="92c7f-117">設定 gRPC</span><span class="sxs-lookup"><span data-stu-id="92c7f-117">Configure gRPC</span></span>

<span data-ttu-id="92c7f-118">gRPC 是以`AddGrpc`方法啟用:</span><span class="sxs-lookup"><span data-stu-id="92c7f-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="92c7f-119">每個 gRPC 服務都會透過`MapGrpcService`方法新增至路由管線:</span><span class="sxs-lookup"><span data-stu-id="92c7f-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="92c7f-120">ASP.NET Core 中介軟體和功能共用路由管線, 因此可以將應用程式設定為提供額外的要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="92c7f-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="92c7f-121">其他要求處理常式 (例如 MVC 控制器) 會與已設定的 gRPC 服務平行處理。</span><span class="sxs-lookup"><span data-stu-id="92c7f-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="92c7f-122">與 ASP.NET Core Api 整合</span><span class="sxs-lookup"><span data-stu-id="92c7f-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="92c7f-123">gRPC 服務具有 ASP.NET Core 功能的完整存取權, 例如相依性[插入](xref:fundamentals/dependency-injection)(DI) 和[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="92c7f-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="92c7f-124">例如, 服務執行可以透過此函式從 DI 容器解析記錄器服務:</span><span class="sxs-lookup"><span data-stu-id="92c7f-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="92c7f-125">根據預設, gRPC 服務執行可以使用任何存留期 (單一、限定範圍或暫時性) 來解析其他 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="92c7f-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="92c7f-126">解析 gRPC 方法中的 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="92c7f-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="92c7f-127">GRPC API 可讓您存取某些 HTTP/2 訊息資料, 例如方法、主機、標頭和結尾。</span><span class="sxs-lookup"><span data-stu-id="92c7f-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="92c7f-128">存取是透過傳遞`ServerCallContext`至每個 gRPC 方法的引數:</span><span class="sxs-lookup"><span data-stu-id="92c7f-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="92c7f-129">`ServerCallContext`並未提供所有 ASP.NET api `HttpContext`的完整存取權。</span><span class="sxs-lookup"><span data-stu-id="92c7f-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="92c7f-130">擴充方法會提供完整的`HttpContext`存取權, 以代表 ASP.NET api 中的基礎 HTTP/2 訊息: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="92c7f-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a><span data-ttu-id="92c7f-131">macOS 上的 gRPC 和 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92c7f-131">gRPC and ASP.NET Core on macOS</span></span>

<span data-ttu-id="92c7f-132">Kestrel 不支援在 macOS 上使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="92c7f-132">Kestrel doesn't support HTTP/2 with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) on macOS.</span></span> <span data-ttu-id="92c7f-133">ASP.NET Core gRPC 範本和範例預設會使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="92c7f-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="92c7f-134">當您嘗試啟動 gRPC 伺服器時, 您會看到下列錯誤訊息:</span><span class="sxs-lookup"><span data-stu-id="92c7f-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="92c7f-135">無法在 IPv4 回 https://localhost:5001 送介面上系結至:由於遺漏 ALPN 支援, macOS 上不支援 HTTP/2 over TLS。 '。</span><span class="sxs-lookup"><span data-stu-id="92c7f-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="92c7f-136">若要解決此問題, 請將 Kestrel 和 gRPC 用戶端設定為使用**沒有**TLS 的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="92c7f-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 **without** TLS.</span></span> <span data-ttu-id="92c7f-137">您應該只在開發期間執行此動作。</span><span class="sxs-lookup"><span data-stu-id="92c7f-137">You should only do this during development.</span></span> <span data-ttu-id="92c7f-138">不使用 TLS 會導致傳送未加密的 gRPC 訊息。</span><span class="sxs-lookup"><span data-stu-id="92c7f-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="92c7f-139">Kestrel 必須在中`Program.cs`設定沒有 TLS 的 HTTP/2 端點:</span><span class="sxs-lookup"><span data-stu-id="92c7f-139">Kestrel must configure a HTTP/2 endpoint without TLS in `Program.cs`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="92c7f-140">GRPC 用戶端必須將`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`參數設定為`true` , 並在伺服器位址中使用: `http`</span><span class="sxs-lookup"><span data-stu-id="92c7f-140">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> <span data-ttu-id="92c7f-141">只有在應用程式開發期間, 才應該使用不含 TLS 的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="92c7f-141">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="92c7f-142">生產應用程式應該一律使用傳輸安全性。</span><span class="sxs-lookup"><span data-stu-id="92c7f-142">Production applications should always use transport security.</span></span> <span data-ttu-id="92c7f-143">如需詳細資訊, 請參閱[gRPC for ASP.NET Core 中的安全性考慮](xref:grpc/security#transport-security)。</span><span class="sxs-lookup"><span data-stu-id="92c7f-143">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92c7f-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="92c7f-144">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
