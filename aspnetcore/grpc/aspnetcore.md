---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 使用 ASP.NET Core 中寫入 gRPC 服務時，請了解基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: ca06478e6168c59d9abf43d99213fa8a7091e178
ms.sourcegitcommit: 756114cab5e24e99ec23de62b0c1c16b15197ac2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2019
ms.locfileid: "67169524"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="27ab7-103">搭配 ASP.NET Core 的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="27ab7-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="27ab7-104">本文件說明如何開始使用 ASP.NET Core 的 gRPC 服務使用。</span><span class="sxs-lookup"><span data-stu-id="27ab7-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="27ab7-105">開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="27ab7-105">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="27ab7-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="27ab7-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27ab7-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27ab7-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27ab7-108">請參閱[開始使用 gRPC 服務](xref:tutorials/grpc/grpc-start)如需有關如何建立 gRPC 專案的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="27ab7-108">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="27ab7-109">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="27ab7-109">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="27ab7-110">從命令列執行 `dotnet new grpc -o GrpcGreeter`。</span><span class="sxs-lookup"><span data-stu-id="27ab7-110">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="27ab7-111">將 gRPC 服務新增至 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="27ab7-111">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="27ab7-112">gRPC 需要下列封裝：</span><span class="sxs-lookup"><span data-stu-id="27ab7-112">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="27ab7-113">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="27ab7-113">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="27ab7-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) protobuf 訊息 Api。</span><span class="sxs-lookup"><span data-stu-id="27ab7-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="27ab7-115">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="27ab7-115">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="27ab7-116">設定 gRPC</span><span class="sxs-lookup"><span data-stu-id="27ab7-116">Configure gRPC</span></span>

<span data-ttu-id="27ab7-117">使用啟用 gRPC`AddGrpc`方法：</span><span class="sxs-lookup"><span data-stu-id="27ab7-117">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="27ab7-118">每個 gRPC 服務新增至路由的管線透過`MapGrpcService`方法：</span><span class="sxs-lookup"><span data-stu-id="27ab7-118">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="27ab7-119">ASP.NET Core 中介軟體和功能共用路由的管線，因此可以設定應用程式提供額外的要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="27ab7-119">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="27ab7-120">與設定的 gRPC 服務的同時處理其他要求處理常式，例如 MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="27ab7-120">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="27ab7-121">與 ASP.NET Core Api 整合</span><span class="sxs-lookup"><span data-stu-id="27ab7-121">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="27ab7-122">gRPC 服務具有完整存取 ASP.NET Core 功能這類[相依性插入](xref:fundamentals/dependency-injection)(DI) 和[記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="27ab7-122">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="27ab7-123">例如，服務實作可以解決透過建構函式的 DI 容器的記錄器服務：</span><span class="sxs-lookup"><span data-stu-id="27ab7-123">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="27ab7-124">根據預設，gRPC 服務實作可以解析其他 DI 服務具有任何存留期 （單一、 範圍或暫時性）。</span><span class="sxs-lookup"><span data-stu-id="27ab7-124">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="27ab7-125">解決 HttpContext 中 gRPC 方法</span><span class="sxs-lookup"><span data-stu-id="27ab7-125">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="27ab7-126">GRPC API 提供存取某些 HTTP/2 訊息資料，例如方法、 主機、 標頭和結尾。</span><span class="sxs-lookup"><span data-stu-id="27ab7-126">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="27ab7-127">存取是透過`ServerCallContext`引數傳遞至每個 gRPC 方法：</span><span class="sxs-lookup"><span data-stu-id="27ab7-127">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="27ab7-128">`ServerCallContext` 不會提供完整存取權`HttpContext`中所有的 ASP.NET Api。</span><span class="sxs-lookup"><span data-stu-id="27ab7-128">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="27ab7-129">`GetHttpContext`擴充方法提供的完整存取權`HttpContext`表示 ASP.NET Api 中的基礎 HTTP/2 訊息：</span><span class="sxs-lookup"><span data-stu-id="27ab7-129">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="27ab7-130">其他資源</span><span class="sxs-lookup"><span data-stu-id="27ab7-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
