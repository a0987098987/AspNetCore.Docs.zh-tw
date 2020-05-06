---
title: ASP.NET Core 2.2 的新功能
author: rick-anderson
description: 了解 ASP.NET Core 2.2 的新功能。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: aspnetcore-2.2
ms.openlocfilehash: 3b510c7f4788a59145ef16720276fc7e4560f07e
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774141"
---
# <a name="whats-new-in-aspnet-core-22"></a><span data-ttu-id="51a77-103">ASP.NET Core 2.2 的新功能</span><span class="sxs-lookup"><span data-stu-id="51a77-103">What's new in ASP.NET Core 2.2</span></span>

<span data-ttu-id="51a77-104">本文會重點說明 ASP.NET Core 2.2 最重要的變更，附有相關文件的連結。</span><span class="sxs-lookup"><span data-stu-id="51a77-104">This article highlights the most significant changes in ASP.NET Core 2.2, with links to relevant documentation.</span></span>

## <a name="openapi-analyzers--conventions"></a><span data-ttu-id="51a77-105">OpenAPI 分析器與慣例</span><span class="sxs-lookup"><span data-stu-id="51a77-105">OpenAPI Analyzers & Conventions</span></span>

<span data-ttu-id="51a77-106">OpenAPI (之前稱為 Swagger) 是用來描述 REST API 的語言無關規格。</span><span class="sxs-lookup"><span data-stu-id="51a77-106">OpenAPI (formerly known as Swagger) is a language-agnostic specification for describing REST APIs.</span></span> <span data-ttu-id="51a77-107">OpenAPI 生態系統中已有工具，可讓您使用此規格來探索、測試和產生用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="51a77-107">The OpenAPI ecosystem has tools that allow for discovering, testing, and producing client code using the specification.</span></span> <span data-ttu-id="51a77-108">在 ASP.NET Core MVC 中產生和視覺化 OpenAPI 檔的支援是透過社區驅動專案（例如[NSwag](https://github.com/RicoSuter/NSwag)和[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)）提供。</span><span class="sxs-lookup"><span data-stu-id="51a77-108">Support for generating and visualizing OpenAPI documents in ASP.NET Core MVC is provided via community driven projects such as [NSwag](https://github.com/RicoSuter/NSwag) and [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore).</span></span> <span data-ttu-id="51a77-109">ASP.NET Core 2.2 提供改善的工具和執行階段體驗來建立 OpenAPI 文件。</span><span class="sxs-lookup"><span data-stu-id="51a77-109">ASP.NET Core 2.2 provides improved tooling and runtime experiences for creating OpenAPI documents.</span></span>

<span data-ttu-id="51a77-110">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="51a77-110">For more information, see the following resources:</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:web-api/advanced/conventions>
* [<span data-ttu-id="51a77-111">ASP.NET Core 2.2.0-preview1： OpenAPI 分析器 & 慣例</span><span class="sxs-lookup"><span data-stu-id="51a77-111">ASP.NET Core 2.2.0-preview1: OpenAPI Analyzers & Conventions</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/)

## <a name="problem-details-support"></a><span data-ttu-id="51a77-112">問題詳細資料支援</span><span class="sxs-lookup"><span data-stu-id="51a77-112">Problem details support</span></span>

<span data-ttu-id="51a77-113">ASP.NET Core 2.1 引進`ProblemDetails`的是以[RFC 7807](https://tools.ietf.org/html/rfc7807)規格為基礎，其使用 HTTP 回應來攜帶錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="51a77-113">ASP.NET Core 2.1 introduced `ProblemDetails`, based on the [RFC 7807](https://tools.ietf.org/html/rfc7807) specification for carrying details of an error with an HTTP Response.</span></span> <span data-ttu-id="51a77-114">在 2.2 中，`ProblemDetails` 是對具有 `ApiControllerAttribute` 屬性之控制器中用戶端錯誤碼的標準回應。</span><span class="sxs-lookup"><span data-stu-id="51a77-114">In 2.2, `ProblemDetails` is the standard response for client error codes in controllers attributed with `ApiControllerAttribute`.</span></span> <span data-ttu-id="51a77-115">傳回用戶端錯誤狀態碼 (4xx) 的 `IActionResult` 現在會傳回 `ProblemDetails` 主體。</span><span class="sxs-lookup"><span data-stu-id="51a77-115">An `IActionResult` returning a client error status code (4xx) now returns a `ProblemDetails` body.</span></span> <span data-ttu-id="51a77-116">結果中也會包含相互關聯識別碼，可用來透過要求記錄檔與錯誤相互關聯。</span><span class="sxs-lookup"><span data-stu-id="51a77-116">The result also includes a correlation ID that can be used to correlate the error using request logs.</span></span> <span data-ttu-id="51a77-117">對於用戶端錯誤，`ProducesResponseType` 預設會使用 `ProblemDetails` 作為回應類型。</span><span class="sxs-lookup"><span data-stu-id="51a77-117">For client errors, `ProducesResponseType` defaults to using `ProblemDetails` as the response type.</span></span> <span data-ttu-id="51a77-118">這會記載於使用 NSwag 或 Swashbuckle.AspNetCore 產生的 OpenAPI/Swagger 輸出中。</span><span class="sxs-lookup"><span data-stu-id="51a77-118">This is documented in OpenAPI / Swagger output generated using NSwag or Swashbuckle.AspNetCore.</span></span>

## <a name="endpoint-routing"></a><span data-ttu-id="51a77-119">端點路由</span><span class="sxs-lookup"><span data-stu-id="51a77-119">Endpoint Routing</span></span>

<span data-ttu-id="51a77-120">ASP.NET Core 2.2 使用新的「端點路由」\*\* 系統來改善要求的分派。</span><span class="sxs-lookup"><span data-stu-id="51a77-120">ASP.NET Core 2.2 uses a new *endpoint routing* system for improved dispatching of requests.</span></span> <span data-ttu-id="51a77-121">其變更包括新的連結產生 API 成員和路由參數轉換器。</span><span class="sxs-lookup"><span data-stu-id="51a77-121">The changes include new link generation API members and route parameter transformers.</span></span>

<span data-ttu-id="51a77-122">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="51a77-122">For more information, see the following resources:</span></span>

* <span data-ttu-id="51a77-123">[Endpoint routing in 2.2](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/) (2.2 中的端點路由)</span><span class="sxs-lookup"><span data-stu-id="51a77-123">[Endpoint routing in 2.2](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/)</span></span>
* <span data-ttu-id="51a77-124">[路由參數轉換器](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (請參閱＜Routing＞(路由)\*\*\*\* 一節)</span><span class="sxs-lookup"><span data-stu-id="51a77-124">[Route parameter transformers](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (see **Routing** section)</span></span>
* [<span data-ttu-id="51a77-125">IRouter 路由與端點路由之間的差異</span><span class="sxs-lookup"><span data-stu-id="51a77-125">Differences between IRouter- and endpoint-based routing</span></span>](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing)

## <a name="health-checks"></a><span data-ttu-id="51a77-126">健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="51a77-126">Health checks</span></span>

<span data-ttu-id="51a77-127">新的健康狀態檢查服務可讓您更輕鬆地在需要健康狀態檢查的環境中 (例如 Kubernetes) 使用 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="51a77-127">A new health checks service makes it easier to use ASP.NET Core in environments that require health checks, such as Kubernetes.</span></span> <span data-ttu-id="51a77-128">健康狀態檢查包括中介軟體，以及定義 `IHealthCheck` 抽象概念和服務的一組程式庫。</span><span class="sxs-lookup"><span data-stu-id="51a77-128">Health checks includes middleware and a set of libraries that define an `IHealthCheck` abstraction and service.</span></span>

<span data-ttu-id="51a77-129">容器協調器或負載平衡器使用健康狀態檢查，來快速判斷系統是否正常回應要求。</span><span class="sxs-lookup"><span data-stu-id="51a77-129">Health checks are used by a container orchestrator or load balancer to quickly determine if a system is responding to requests normally.</span></span> <span data-ttu-id="51a77-130">容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="51a77-130">A container orchestrator might respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="51a77-131">負載平衡器可能會從服務的失敗執行個體路由出流量，來回應健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="51a77-131">A load balancer might respond to a health check by routing traffic away from the failing instance of the service.</span></span>

<span data-ttu-id="51a77-132">應用程式會將健康狀態檢查公開為監控系統所使用的 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="51a77-132">Health checks are exposed by an application as an HTTP endpoint used by monitoring systems.</span></span> <span data-ttu-id="51a77-133">您可以針對各種即時監控案例和監控系統來設定健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="51a77-133">Health checks can be configured for a variety of real-time monitoring scenarios and monitoring systems.</span></span> <span data-ttu-id="51a77-134">健康狀態檢查服務與 [BeatPulse 專案](https://github.com/Xabaril/BeatPulse)整合。</span><span class="sxs-lookup"><span data-stu-id="51a77-134">The health checks service integrates with the [BeatPulse project](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="51a77-135">這可讓您更輕鬆地新增數十個熱門系統和相依性的檢查。</span><span class="sxs-lookup"><span data-stu-id="51a77-135">which makes it easier to add checks for dozens of popular systems and dependencies.</span></span>

<span data-ttu-id="51a77-136">如需詳細資訊，請參閱 [ASP.NET Core 中的健康狀態檢查](xref:host-and-deploy/health-checks)。</span><span class="sxs-lookup"><span data-stu-id="51a77-136">For more information, see [Health checks in ASP.NET Core](xref:host-and-deploy/health-checks).</span></span>

## <a name="http2-in-kestrel"></a><span data-ttu-id="51a77-137">Kestrel 中的 HTTP/2</span><span class="sxs-lookup"><span data-stu-id="51a77-137">HTTP/2 in Kestrel</span></span>

<span data-ttu-id="51a77-138">ASP.NET Core 2.2 新增 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="51a77-138">ASP.NET Core 2.2 adds support for HTTP/2.</span></span>

<span data-ttu-id="51a77-139">HTTP/2 是 HTTP 通訊協定的主要版本。</span><span class="sxs-lookup"><span data-stu-id="51a77-139">HTTP/2 is a major revision of the HTTP protocol.</span></span> <span data-ttu-id="51a77-140">值得注意的 HTTP/2 功能包括：</span><span class="sxs-lookup"><span data-stu-id="51a77-140">Notable features of HTTP/2 include:</span></span>

* <span data-ttu-id="51a77-141">支援標頭壓縮。</span><span class="sxs-lookup"><span data-stu-id="51a77-141">Support for header compression.</span></span>
* <span data-ttu-id="51a77-142">透過單一連線的完整多工串流。</span><span class="sxs-lookup"><span data-stu-id="51a77-142">Fully multiplexed streams over a single connection.</span></span>

<span data-ttu-id="51a77-143">雖然 HTTP/2 會保留 HTTP 的語法（例如，HTTP 標頭和方法），但這是從 HTTP/1.x 進行的重大變更，以瞭解資料在用戶端與伺服器之間的框架和傳送方式。</span><span class="sxs-lookup"><span data-stu-id="51a77-143">While HTTP/2 preserves HTTP's semantics (for example, HTTP headers and methods), it's a breaking change from HTTP/1.x on how data is framed and sent between the client and server.</span></span>

<span data-ttu-id="51a77-144">這項框架處理變更導致伺服器和用戶端必須交涉所使用的通訊協定版本。</span><span class="sxs-lookup"><span data-stu-id="51a77-144">As a consequence of this change in framing, servers and clients need to negotiate the protocol version used.</span></span> <span data-ttu-id="51a77-145">Application-Layer Protocol Negotiation (ALPN) 是 TLS 延伸模組，可讓伺服器和用戶端交涉在其 TLS 信號交換過程中所使用的通訊協定版本。</span><span class="sxs-lookup"><span data-stu-id="51a77-145">Application-Layer Protocol Negotiation (ALPN) is a TLS extension that allows the server and client to negotiate the protocol version used as part of their TLS handshake.</span></span> <span data-ttu-id="51a77-146">雖然您可能事先知道伺服器與用戶端之間的通訊協定，但所有主要瀏覽器都支援 ALPN 作為建立 HTTP/2 連線的唯一方式。</span><span class="sxs-lookup"><span data-stu-id="51a77-146">While it is possible to have prior knowledge between the server and the client on the protocol, all major browsers support ALPN as the only way to establish an HTTP/2 connection.</span></span>

<span data-ttu-id="51a77-147">如需詳細資訊，請參閱 [HTTP/2 支援](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support)。</span><span class="sxs-lookup"><span data-stu-id="51a77-147">For more information, see [HTTP/2 support](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).</span></span>

## <a name="kestrel-configuration"></a><span data-ttu-id="51a77-148">Kestrel 組態</span><span class="sxs-lookup"><span data-stu-id="51a77-148">Kestrel configuration</span></span>

<span data-ttu-id="51a77-149">在舊版的 ASP.NET Core 中，Kestrel 選項是透過呼叫 `UseKestrel` 設定。</span><span class="sxs-lookup"><span data-stu-id="51a77-149">In earlier versions of ASP.NET Core, Kestrel options are configured by calling `UseKestrel`.</span></span> <span data-ttu-id="51a77-150">在 2.2 中，Kestrel 選項是透過在主機產生器上呼叫 `ConfigureKestrel` 設定。</span><span class="sxs-lookup"><span data-stu-id="51a77-150">In 2.2, Kestrel options are configured by calling `ConfigureKestrel` on the host builder.</span></span> <span data-ttu-id="51a77-151">這項變更解決了同處理序裝載的 `IServer` 註冊順序問題。</span><span class="sxs-lookup"><span data-stu-id="51a77-151">This change resolves an issue with the order of `IServer` registrations for in-process hosting.</span></span> <span data-ttu-id="51a77-152">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="51a77-152">For more information, see the following resources:</span></span>

* <span data-ttu-id="51a77-153">[Mitigate UseIIS conflict](https://github.com/aspnet/KestrelHttpServer/issues/2760) (降低 UseIIS 衝突)</span><span class="sxs-lookup"><span data-stu-id="51a77-153">[Mitigate UseIIS conflict](https://github.com/aspnet/KestrelHttpServer/issues/2760)</span></span>
* [<span data-ttu-id="51a77-154">使用 ConfigureKestrel 設定 Kestrel 伺服器選項</span><span class="sxs-lookup"><span data-stu-id="51a77-154">Configure Kestrel server options with ConfigureKestrel</span></span>](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a><span data-ttu-id="51a77-155">IIS 同處理序裝載</span><span class="sxs-lookup"><span data-stu-id="51a77-155">IIS in-process hosting</span></span>

<span data-ttu-id="51a77-156">在舊版的 ASP.NET Core 中，IIS 是作為反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="51a77-156">In earlier versions of ASP.NET Core, IIS serves as a reverse proxy.</span></span> <span data-ttu-id="51a77-157">在 2.2 中，ASP.NET Core 模組可啟動 CoreCLR，並在 IIS 背景工作處理序 (*w3wp.exe*) 內裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="51a77-157">In 2.2, the ASP.NET Core Module can boot the CoreCLR and host an app inside the IIS worker process (*w3wp.exe*).</span></span> <span data-ttu-id="51a77-158">同處理序裝載以 IIS 執行時可改善效能和診斷。</span><span class="sxs-lookup"><span data-stu-id="51a77-158">In-process hosting provides performance and diagnostic gains when running with IIS.</span></span>

<span data-ttu-id="51a77-159">如需詳細資訊，請參閱 [IIS 的同處理序裝載](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model)。</span><span class="sxs-lookup"><span data-stu-id="51a77-159">For more information, see [in-process hosting for IIS](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).</span></span>

## <a name="signalr-java-client"></a>SignalR<span data-ttu-id="51a77-160">JAVA 用戶端</span><span class="sxs-lookup"><span data-stu-id="51a77-160"> Java client</span></span>

<span data-ttu-id="51a77-161">ASP.NET Core 2.2 引進的 JAVA 用戶端SignalR。</span><span class="sxs-lookup"><span data-stu-id="51a77-161">ASP.NET Core 2.2 introduces a Java Client for SignalR.</span></span> <span data-ttu-id="51a77-162">此用戶端支援從 JAVA 程式SignalR代碼連接到 ASP.NET Core 伺服器，包括 Android 應用程式。</span><span class="sxs-lookup"><span data-stu-id="51a77-162">This client supports connecting to an ASP.NET Core SignalR Server from Java code, including Android apps.</span></span>

<span data-ttu-id="51a77-163">如需詳細資訊，請參閱[ASP.NET Core SignalR JAVA 用戶端](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="51a77-163">For more information, see [ASP.NET Core SignalR Java client](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).</span></span>

## <a name="cors-improvements"></a><span data-ttu-id="51a77-164">CORS 改善</span><span class="sxs-lookup"><span data-stu-id="51a77-164">CORS improvements</span></span>

<span data-ttu-id="51a77-165">在舊版的 ASP.NET Core 中，CORS 中介軟體允許傳送 `Accept`、`Accept-Language`、`Content-Language` 和 `Origin` 標頭，無論 `CorsPolicy.Headers` 中設定的值為何。</span><span class="sxs-lookup"><span data-stu-id="51a77-165">In earlier versions of ASP.NET Core, CORS Middleware allows `Accept`, `Accept-Language`, `Content-Language`, and `Origin` headers to be sent regardless of the values configured in `CorsPolicy.Headers`.</span></span> <span data-ttu-id="51a77-166">在 2.2 中，只有 `Access-Control-Request-Headers` 中傳送的標頭與 `WithHeaders` 中指定的標頭完全相符時，才可能符合 CORS 中介軟體原則。</span><span class="sxs-lookup"><span data-stu-id="51a77-166">In 2.2, a CORS Middleware policy match is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="51a77-167">如需詳細資訊，請參閱 [CORS 中介軟體](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers)。</span><span class="sxs-lookup"><span data-stu-id="51a77-167">For more information, see [CORS Middleware](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).</span></span>

## <a name="response-compression"></a><span data-ttu-id="51a77-168">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="51a77-168">Response compression</span></span>

<span data-ttu-id="51a77-169">ASP.NET Core 2.2 可以使用 [Brotli 壓縮格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="51a77-169">ASP.NET Core 2.2 can compress responses with the [Brotli compression format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="51a77-170">如需詳細資訊，請參閱[回應壓縮中介軟體支援 Brotli 壓縮](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="51a77-170">For more information, see [Response Compression Middleware supports Brotli compression](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).</span></span>

## <a name="project-templates"></a><span data-ttu-id="51a77-171">專案範本</span><span class="sxs-lookup"><span data-stu-id="51a77-171">Project templates</span></span>

<span data-ttu-id="51a77-172">ASP.NET Core Web 專案範本已更新為 [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) 和 [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4)。</span><span class="sxs-lookup"><span data-stu-id="51a77-172">ASP.NET Core web project templates were updated to [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) and [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4).</span></span> <span data-ttu-id="51a77-173">新的外觀看起來比較簡單，因此更容易看出應用程式的重要結構。</span><span class="sxs-lookup"><span data-stu-id="51a77-173">The new look is visually simpler and makes it easier to see the important structures of the app.</span></span>

![Home 或 Index 頁面](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a><span data-ttu-id="51a77-175">驗證效能</span><span class="sxs-lookup"><span data-stu-id="51a77-175">Validation performance</span></span>

<span data-ttu-id="51a77-176">MVC 的驗證系統設計成可延伸和彈性，可讓您根據每個要求判斷哪些驗證程式適用于指定的模型。</span><span class="sxs-lookup"><span data-stu-id="51a77-176">MVC's validation system is designed to be extensible and flexible, allowing you to determine on a per request basis which validators apply to a given model.</span></span> <span data-ttu-id="51a77-177">這有助於撰寫複雜的驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="51a77-177">This is great for authoring complex validation providers.</span></span> <span data-ttu-id="51a77-178">不過，在最常見的情況下，應用程式只會使用內建的驗證程式，而不需要這個額外的彈性。</span><span class="sxs-lookup"><span data-stu-id="51a77-178">However, in the most common case an application only uses the built-in validators and don't require this extra flexibility.</span></span> <span data-ttu-id="51a77-179">內建驗證程式包括 DataAnnotations (例如 [Required] 和 [StringLength]) 以及 `IValidatableObject`。</span><span class="sxs-lookup"><span data-stu-id="51a77-179">Built-in validators include DataAnnotations such as [Required] and [StringLength], and `IValidatableObject`.</span></span>

<span data-ttu-id="51a77-180">在 ASP.NET Core 2.2 中，若 MVC 判斷指定的模型圖形不需要驗證，則會跳過驗證。</span><span class="sxs-lookup"><span data-stu-id="51a77-180">In ASP.NET Core 2.2, MVC can short-circuit validation if it determines that a given model graph doesn't require validation.</span></span> <span data-ttu-id="51a77-181">當驗證無法驗證或沒有任何驗證程式的模型時，跳過驗證可大幅改善效能。</span><span class="sxs-lookup"><span data-stu-id="51a77-181">Skipping validation results in significant improvements when validating models that can't or don't have any validators.</span></span> <span data-ttu-id="51a77-182">這包括諸如基本類型集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 的物件，或沒有許多驗證程式的複雜物件圖形。</span><span class="sxs-lookup"><span data-stu-id="51a77-182">This includes objects such as collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`), or complex object graphs without many validators.</span></span>

## <a name="http-client-performance"></a><span data-ttu-id="51a77-183">HTTP 用戶端效能</span><span class="sxs-lookup"><span data-stu-id="51a77-183">HTTP Client performance</span></span>

<span data-ttu-id="51a77-184">在 ASP.NET Core 2.2 中，由於降低連接集區鎖定競爭情況，因此改善了 `SocketsHttpHandler` 的效能。</span><span class="sxs-lookup"><span data-stu-id="51a77-184">In ASP.NET Core 2.2, the performance of `SocketsHttpHandler` was improved by reducing connection pool locking contention.</span></span> <span data-ttu-id="51a77-185">針對進行許多傳出 HTTP 要求的應用程式 (例如某些微服務架構)，輸送量已獲得改善。</span><span class="sxs-lookup"><span data-stu-id="51a77-185">For apps that make many outgoing HTTP requests, such as some microservices architectures, throughput is improved.</span></span> <span data-ttu-id="51a77-186">在負載情況下，`HttpClient` 輸送量在 Linux 上可提升高達 60%，在 Windows 上可提升高達 20%。</span><span class="sxs-lookup"><span data-stu-id="51a77-186">Under load, `HttpClient` throughput can be improved by up to 60% on Linux and 20% on Windows.</span></span>

<span data-ttu-id="51a77-187">如需詳細資訊，請參閱[已做出這項改善的提取要求](https://github.com/dotnet/corefx/pull/32568)。</span><span class="sxs-lookup"><span data-stu-id="51a77-187">For more information, see [the pull request that made this improvement](https://github.com/dotnet/corefx/pull/32568).</span></span>

## <a name="additional-information"></a><span data-ttu-id="51a77-188">其他資訊</span><span class="sxs-lookup"><span data-stu-id="51a77-188">Additional information</span></span>

<span data-ttu-id="51a77-189">如需完整的變更清單，請參閱 [ASP.NET Core 2.2 版本資訊](https://github.com/dotnet/aspnetcore/releases/tag/2.2.0)。</span><span class="sxs-lookup"><span data-stu-id="51a77-189">For the complete list of changes, see the [ASP.NET Core 2.2 Release Notes](https://github.com/dotnet/aspnetcore/releases/tag/2.2.0).</span></span>
