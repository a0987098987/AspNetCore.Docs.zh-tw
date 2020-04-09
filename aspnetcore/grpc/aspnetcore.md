---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 使用 ASP.NET 核心編寫 gRPC 服務時,瞭解基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 6107704a4b4d9c629a7abe907efd5b1932019130
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667626"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="c7a3b-103">搭配 ASP.NET Core 的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="c7a3b-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="c7a3b-104">本文檔展示如何使用ASP.NET酷睿開始使用 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7a3b-105">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="c7a3b-105">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c7a3b-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7a3b-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="c7a3b-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7a3b-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="c7a3b-108">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c7a3b-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="c7a3b-109">開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="c7a3b-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="c7a3b-110">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c7a3b-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7a3b-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c7a3b-112">有關如何創建 gRPC 專案的詳細說明,請參閱[gRPC 服務入門](xref:tutorials/grpc/grpc-start)。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c7a3b-113">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c7a3b-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c7a3b-114">從命令列執行 `dotnet new grpc -o GrpcGreeter`。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="c7a3b-115">將 gRPC 服務新增到 ASP.NET 核心應用</span><span class="sxs-lookup"><span data-stu-id="c7a3b-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="c7a3b-116">gRPC 需要[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)包。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="c7a3b-117">設定 gRPC</span><span class="sxs-lookup"><span data-stu-id="c7a3b-117">Configure gRPC</span></span>

<span data-ttu-id="c7a3b-118">在 *Startup.cs* 中：</span><span class="sxs-lookup"><span data-stu-id="c7a3b-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="c7a3b-119">使用`AddGrpc`方法啟用 gRPC。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="c7a3b-120">通過`MapGrpcService`該方法將每個 gRPC 服務添加到路由管道中。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="c7a3b-121">ASP.NET核心中間件和功能共用路由管道,因此可以配置應用以提供其他請求處理程式。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="c7a3b-122">其他請求處理程式(如MVC控制器)與配置的gRPC服務並行工作。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="c7a3b-123">設定凱斯特雷爾</span><span class="sxs-lookup"><span data-stu-id="c7a3b-123">Configure Kestrel</span></span>

<span data-ttu-id="c7a3b-124">凱斯特雷爾 gRPC 終結點:</span><span class="sxs-lookup"><span data-stu-id="c7a3b-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="c7a3b-125">需要 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-125">Require HTTP/2.</span></span>
* <span data-ttu-id="c7a3b-126">應使用[傳輸層安全 (TLS)](https://tools.ietf.org/html/rfc5246)進行保護。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-126">Should be secured with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span>

#### <a name="http2"></a><span data-ttu-id="c7a3b-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c7a3b-127">HTTP/2</span></span>

<span data-ttu-id="c7a3b-128">gRPC 需要 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="c7a3b-129">gRPCASP.NET核心驗證[HTTPRequest.協定](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)是`HTTP/2`。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="c7a3b-130">Kestrel 在大多數現代作業系統上[支持 HTTP/2。](xref:fundamentals/servers/kestrel#http2-support)</span><span class="sxs-lookup"><span data-stu-id="c7a3b-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="c7a3b-131">預設情況下,Kestrel 終結點配置為支援 HTTP/1.1 和 HTTP/2 連接。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="tls"></a><span data-ttu-id="c7a3b-132">TLS</span><span class="sxs-lookup"><span data-stu-id="c7a3b-132">TLS</span></span>

<span data-ttu-id="c7a3b-133">用於 gRPC 的 Kestrel 端點應用 TLS 進行保護。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-133">Kestrel endpoints used for gRPC should be secured with TLS.</span></span> <span data-ttu-id="c7a3b-134">在開發中,當存在ASP.NET核心開發證書`https://localhost:5001`時,將自動創建使用TLS保護的終結點。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-134">In development, an endpoint secured with TLS is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="c7a3b-135">不需要組態。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-135">No configuration is required.</span></span> <span data-ttu-id="c7a3b-136">前置`https`授權 Kestrel 終結點正在使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-136">An `https` prefix verifies the Kestrel endpoint is using TLS.</span></span>

<span data-ttu-id="c7a3b-137">在生產中,必須顯式配置 TLS。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-137">In production, TLS must be explicitly configured.</span></span> <span data-ttu-id="c7a3b-138">在下面的*應用設定.json*範例中,提供了一個使用 TLS 保護的 HTTP/2 終結點:</span><span class="sxs-lookup"><span data-stu-id="c7a3b-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with TLS is provided:</span></span>

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

<span data-ttu-id="c7a3b-139">或者,Kestrel 終結點可以在*Program.cs*中配置:</span><span class="sxs-lookup"><span data-stu-id="c7a3b-139">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a><span data-ttu-id="c7a3b-140">通訊協定交涉</span><span class="sxs-lookup"><span data-stu-id="c7a3b-140">Protocol negotiation</span></span>

<span data-ttu-id="c7a3b-141">TLS 僅用於保護通信。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-141">TLS is used for more than securing communication.</span></span> <span data-ttu-id="c7a3b-142">當終結點支援多個協定時,TLS[應用程式層協議協商 (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)握手用於協商客戶端和伺服器之間的連接協定。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-142">The TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake is used to negotiate the connection protocol between the client and the server when an endpoint supports multiple protocols.</span></span> <span data-ttu-id="c7a3b-143">此協商確定連接是使用 HTTP/1.1 還是 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-143">This negotiation determines whether the connection uses HTTP/1.1 or HTTP/2.</span></span>

<span data-ttu-id="c7a3b-144">如果 HTTP/2 終結點設定時沒有 TLS,則終結點的[ListenOptions.協定](xref:fundamentals/servers/kestrel#listenoptionsprotocols)必須設定為`HttpProtocols.Http2`。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-144">If an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="c7a3b-145">沒有 TLS,不能使用具有多個協議`HttpProtocols.Http1AndHttp2`的終結點(例如,)。如果不存在協商。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-145">An endpoint with multiple protocols (for example, `HttpProtocols.Http1AndHttp2`) can't be used without TLS because there is no negotiation.</span></span> <span data-ttu-id="c7a3b-146">與不安全的終結點的所有連接預設為 HTTP/1.1,gRPC 調用失敗。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-146">All connections to the unsecured endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="c7a3b-147">有關使用 Kestrel 啟用 HTTP/2 與 TLS 的詳細資訊,請參閱[Kestrel 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-147">For more information on enabling HTTP/2 and TLS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="c7a3b-148">macOS 不支援具有 TLS 的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-148">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="c7a3b-149">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-149">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="c7a3b-150">如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-150">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="c7a3b-151">與ASP.NET核心 API 整合</span><span class="sxs-lookup"><span data-stu-id="c7a3b-151">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="c7a3b-152">gRPC 服務具有對ASP.NET核心功能(如[依賴項注入](xref:fundamentals/dependency-injection)(DI) 和[日誌記錄](xref:fundamentals/logging/index))的完全訪問許可權。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-152">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="c7a3b-153">例如,服務實現可以透過構造函數從 DI 容器解析記錄器服務:</span><span class="sxs-lookup"><span data-stu-id="c7a3b-153">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="c7a3b-154">默認情況下,gRPC 服務實現可以解決具有任何存留期的其他 DI 服務(單例、作用域或瞬態)。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-154">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="c7a3b-155">在 gRPC 方法解析 HTTP:/ 1: HTTPContext</span><span class="sxs-lookup"><span data-stu-id="c7a3b-155">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="c7a3b-156">gRPC API 提供對某些 HTTP/2 消息數據(如方法、主機、標頭和預告片)的訪問。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-156">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="c7a3b-157">存取是透過以提供`ServerCallContext`每個 gRPC 方法的參數:</span><span class="sxs-lookup"><span data-stu-id="c7a3b-157">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="c7a3b-158">`ServerCallContext`不提供對所有`HttpContext`ASP.NET API 的完全訪問許可權。</span><span class="sxs-lookup"><span data-stu-id="c7a3b-158">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="c7a3b-159">擴`GetHttpContext`充方法提供對 ASP.NET`HttpContext`API 中表示基礎 HTTP/2 消息的完全存取權限:</span><span class="sxs-lookup"><span data-stu-id="c7a3b-159">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="c7a3b-160">其他資源</span><span class="sxs-lookup"><span data-stu-id="c7a3b-160">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
