---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 瞭解使用 ASP.NET Core 撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/28/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 128f5b36eac9112460c33693db5537134a077476
ms.sourcegitcommit: 23f79bd71d49c4efddb56377c1f553cc993d781b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2019
ms.locfileid: "70130709"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="f454a-103">搭配 ASP.NET Core 的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="f454a-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="f454a-104">本檔說明如何使用 ASP.NET Core 開始使用 gRPC services。</span><span class="sxs-lookup"><span data-stu-id="f454a-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f454a-105">必要條件</span><span class="sxs-lookup"><span data-stu-id="f454a-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f454a-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f454a-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f454a-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f454a-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f454a-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f454a-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="f454a-109">開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="f454a-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="f454a-110">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f454a-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f454a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f454a-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f454a-112">如需如何建立 gRPC 專案的詳細指示, 請參閱[開始使用 gRPC services](xref:tutorials/grpc/grpc-start) 。</span><span class="sxs-lookup"><span data-stu-id="f454a-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f454a-113">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f454a-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f454a-114">從命令列執行 `dotnet new grpc -o GrpcGreeter`。</span><span class="sxs-lookup"><span data-stu-id="f454a-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="f454a-115">將 gRPC 服務新增至 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="f454a-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="f454a-116">gRPC 需要[gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)套件。</span><span class="sxs-lookup"><span data-stu-id="f454a-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="f454a-117">設定 gRPC</span><span class="sxs-lookup"><span data-stu-id="f454a-117">Configure gRPC</span></span>

<span data-ttu-id="f454a-118">在 *Startup.cs* 中：</span><span class="sxs-lookup"><span data-stu-id="f454a-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="f454a-119">gRPC 是以`AddGrpc`方法啟用。</span><span class="sxs-lookup"><span data-stu-id="f454a-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="f454a-120">每個 gRPC 服務都會透過`MapGrpcService`方法加入至路由管線。</span><span class="sxs-lookup"><span data-stu-id="f454a-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="f454a-121">ASP.NET Core 中介軟體和功能共用路由管線, 因此可以將應用程式設定為提供額外的要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="f454a-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="f454a-122">其他要求處理常式 (例如 MVC 控制器) 會與已設定的 gRPC 服務平行處理。</span><span class="sxs-lookup"><span data-stu-id="f454a-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="f454a-123">設定 Kestrel</span><span class="sxs-lookup"><span data-stu-id="f454a-123">Configure Kestrel</span></span>

<span data-ttu-id="f454a-124">Kestrel gRPC 端點:</span><span class="sxs-lookup"><span data-stu-id="f454a-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="f454a-125">需要 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="f454a-125">Require HTTP/2.</span></span>
* <span data-ttu-id="f454a-126">應使用 HTTPS 來保護。</span><span class="sxs-lookup"><span data-stu-id="f454a-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="f454a-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="f454a-127">HTTP/2</span></span>

<span data-ttu-id="f454a-128">Kestrel 支援大多數新式作業系統上的[HTTP/2](xref:fundamentals/servers/kestrel#http2-support) 。</span><span class="sxs-lookup"><span data-stu-id="f454a-128">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="f454a-129">根據預設, Kestrel 端點會設定為支援 HTTP/1.1 和 HTTP/2 連接。</span><span class="sxs-lookup"><span data-stu-id="f454a-129">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

> [!NOTE]
> <span data-ttu-id="f454a-130">macOS 不支援具有[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="f454a-130">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="f454a-131">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="f454a-131">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="f454a-132">如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="f454a-132">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

#### <a name="https"></a><span data-ttu-id="f454a-133">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f454a-133">HTTPS</span></span>

<span data-ttu-id="f454a-134">用於 gRPC 的 Kestrel 端點應使用 HTTPS 來保護。</span><span class="sxs-lookup"><span data-stu-id="f454a-134">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="f454a-135">在開發期間, 會在 ASP.NET Core 開發憑證存在`https://localhost:5001`時, 自動建立 HTTPS 端點。</span><span class="sxs-lookup"><span data-stu-id="f454a-135">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="f454a-136">不需要進行任何設定。</span><span class="sxs-lookup"><span data-stu-id="f454a-136">No configuration is required.</span></span>

<span data-ttu-id="f454a-137">在生產環境中，必須明確設定 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="f454a-137">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="f454a-138">在下列*appsettings*範例中, 提供了使用 HTTPS 保護的 HTTP/2 端點:</span><span class="sxs-lookup"><span data-stu-id="f454a-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="f454a-139">或者, 您也可以在*Program.cs*中設定 Kestrel endspoints:</span><span class="sxs-lookup"><span data-stu-id="f454a-139">Alternatively, Kestrel endspoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="f454a-140">如需使用 Kestrel 啟用 HTTP/2 和 HTTPS 的詳細資訊, 請參閱[Kestrel 端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定。</span><span class="sxs-lookup"><span data-stu-id="f454a-140">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="f454a-141">與 ASP.NET Core Api 整合</span><span class="sxs-lookup"><span data-stu-id="f454a-141">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="f454a-142">gRPC 服務具有 ASP.NET Core 功能的完整存取權, 例如相依性[插入](xref:fundamentals/dependency-injection)(DI) 和[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="f454a-142">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="f454a-143">例如, 服務執行可以透過此函式從 DI 容器解析記錄器服務:</span><span class="sxs-lookup"><span data-stu-id="f454a-143">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="f454a-144">根據預設, gRPC 服務執行可以使用任何存留期 (單一、限定範圍或暫時性) 來解析其他 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="f454a-144">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="f454a-145">解析 gRPC 方法中的 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="f454a-145">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="f454a-146">GRPC API 可讓您存取某些 HTTP/2 訊息資料, 例如方法、主機、標頭和結尾。</span><span class="sxs-lookup"><span data-stu-id="f454a-146">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="f454a-147">存取是透過傳遞`ServerCallContext`至每個 gRPC 方法的引數:</span><span class="sxs-lookup"><span data-stu-id="f454a-147">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="f454a-148">`ServerCallContext`並未提供所有 ASP.NET api `HttpContext`的完整存取權。</span><span class="sxs-lookup"><span data-stu-id="f454a-148">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="f454a-149">擴充方法會提供完整的`HttpContext`存取權, 以代表 ASP.NET api 中的基礎 HTTP/2 訊息: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="f454a-149">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="f454a-150">其他資源</span><span class="sxs-lookup"><span data-stu-id="f454a-150">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
