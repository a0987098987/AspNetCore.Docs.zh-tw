---
title: 在 .NET 上的 gRPC 中記錄與診斷
author: jamesnk
description: 瞭解如何從 .NET 上的 gRPC 應用收集診斷。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 131144bf7a2c637eb2c1a1d5c54990dd4d429502
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417511"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="bd1c3-103">在 .NET 上的 gRPC 中記錄與診斷</span><span class="sxs-lookup"><span data-stu-id="bd1c3-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="bd1c3-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="bd1c3-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="bd1c3-105">本文提供有關從 gRPC 應用收集診斷以説明解決問題的指導。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-105">This article provides guidance for gathering diagnostics from a gRPC app to help troubleshoot issues.</span></span> <span data-ttu-id="bd1c3-106">涵蓋的主題包括：</span><span class="sxs-lookup"><span data-stu-id="bd1c3-106">Topics covered include:</span></span>

* <span data-ttu-id="bd1c3-107">**記錄紀錄**- 寫入[.NET 核心紀錄記錄](xref:fundamentals/logging/index)的結構化日誌。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-107">**Logging** - Structured logs written to [.NET Core logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="bd1c3-108"><xref:Microsoft.Extensions.Logging.ILogger>應用框架用於編寫日誌,以及使用者用於在應用中自己的登錄。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-108"><xref:Microsoft.Extensions.Logging.ILogger> is used by app frameworks to write logs, and by users for their own logging in an app.</span></span>
* <span data-ttu-id="bd1c3-109">**追蹤**-`DiaganosticSource`與`Activity`使用與 編寫的操作相關的事件。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-109">**Tracing** - Events related to an operation written using `DiaganosticSource` and `Activity`.</span></span> <span data-ttu-id="bd1c3-110">從診斷源的追蹤通常用於收集應用程式遙測庫(如[應用程式見解](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)與[Open遙測](https://github.com/open-telemetry/opentelemetry-dotnet)) 。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-110">Traces from diagnostic source are commonly used to collect app telemetry by libraries such as [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) and [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet).</span></span>
* <span data-ttu-id="bd1c3-111">**指標**- 資料度量的表示時間間隔,例如每秒的請求。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-111">**Metrics** - Representation of data measures over intervals of time, for example, requests per second.</span></span> <span data-ttu-id="bd1c3-112">指標是使用`EventCounter`點[網計數器](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters)命令列工具或使用[應用程式見解](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)發出的,並且可以觀察到。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-112">Metrics are emitted using `EventCounter` and can be observed using [dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) command line tool or with [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).</span></span>

## <a name="logging"></a><span data-ttu-id="bd1c3-113">記錄</span><span class="sxs-lookup"><span data-stu-id="bd1c3-113">Logging</span></span>

<span data-ttu-id="bd1c3-114">gRPC 服務與 gRPC 用戶端使用[.NET Core 紀錄紀錄寫入紀錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-114">gRPC services and the gRPC client write logs using [.NET Core logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="bd1c3-115">當您需要在應用中調試意外行為時,日誌是一個很好的開始位置。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-115">Logs are a good place to start when you need to debug unexpected behavior in your apps.</span></span>

### <a name="grpc-services-logging"></a><span data-ttu-id="bd1c3-116">gRPC 服務紀錄記錄</span><span class="sxs-lookup"><span data-stu-id="bd1c3-116">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="bd1c3-117">伺服器端日誌可能包含來自應用的敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-117">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="bd1c3-118">**切勿**將原始日誌從生產應用發佈到 GitHub 等公共論壇。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-118">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="bd1c3-119">由於 gRPC 服務託管在 ASP.NET 酷中,因此它使用 ASP.NET 核心日誌記錄系統。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-119">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="bd1c3-120">在預設配置中,gRPC 記錄的資訊很少,但可以配置。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-120">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="bd1c3-121">有關配置ASP.NET核心日誌記錄的詳細資訊,請參閱[有關ASP.NET核心日誌記錄](xref:fundamentals/logging/index#configuration)的文檔。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-121">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="bd1c3-122">gRPC`Grpc`在類別下添加日誌。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-122">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="bd1c3-123">要啟用 gRPC 的詳細日誌,`Grpc`請將以下項`Debug``LogLevel`添加`Logging`到 中的子部分,將前綴配置為*appsettings.json*檔中的級別。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-123">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

<span data-ttu-id="bd1c3-124">還可以在*Startup.cs*中配置此點, `ConfigureLogging`</span><span class="sxs-lookup"><span data-stu-id="bd1c3-124">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

<span data-ttu-id="bd1c3-125">如果不使用基於 JSON 的設定,請在設定系統中設定以下配置值:</span><span class="sxs-lookup"><span data-stu-id="bd1c3-125">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="bd1c3-126">檢查設定系統的文檔,以確定如何指定嵌套配置值。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-126">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="bd1c3-127">例如,使用環境變數時,使用兩`_`個字元而不是`:`(例如`Logging__LogLevel__Grpc`。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-127">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="bd1c3-128">我們建議在為應用收集`Debug`更詳細的診斷時使用該級別。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-128">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="bd1c3-129">該`Trace`級別生成非常低級別的診斷,很少需要診斷應用中的問題。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-129">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

#### <a name="sample-logging-output"></a><span data-ttu-id="bd1c3-130">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="bd1c3-130">Sample logging output</span></span>

<span data-ttu-id="bd1c3-131">下面是 gRPC`Debug`服務 等級的主控台輸出範例:</span><span class="sxs-lookup"><span data-stu-id="bd1c3-131">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

### <a name="access-server-side-logs"></a><span data-ttu-id="bd1c3-132">存取伺服器端記錄</span><span class="sxs-lookup"><span data-stu-id="bd1c3-132">Access server-side logs</span></span>

<span data-ttu-id="bd1c3-133">訪問伺服器端日誌的方式取決於正在運行的環境。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-133">How you access server-side logs depends on the environment in which you're running.</span></span>

#### <a name="as-a-console-app"></a><span data-ttu-id="bd1c3-134">作為主控台應用</span><span class="sxs-lookup"><span data-stu-id="bd1c3-134">As a console app</span></span>

<span data-ttu-id="bd1c3-135">如果在主控台應用中執行,預設情況下應啟用[主控台紀錄器](xref:fundamentals/logging/index#console-provider)。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-135">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="bd1c3-136">gRPC 日誌將顯示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-136">gRPC logs will appear in the console.</span></span>

#### <a name="other-environments"></a><span data-ttu-id="bd1c3-137">其他環境</span><span class="sxs-lookup"><span data-stu-id="bd1c3-137">Other environments</span></span>

<span data-ttu-id="bd1c3-138">如果應用部署到其他環境(例如 Docker、Kubernetes 或 Windows 服務),請參閱<xref:fundamentals/logging/index>有關如何配置適合該環境的日誌記錄提供程式的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-138">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

### <a name="grpc-client-logging"></a><span data-ttu-id="bd1c3-139">gRPC 客戶端記錄記錄</span><span class="sxs-lookup"><span data-stu-id="bd1c3-139">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="bd1c3-140">用戶端日誌可能包含來自應用的敏感資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-140">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="bd1c3-141">**切勿**將原始日誌從生產應用發佈到 GitHub 等公共論壇。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-141">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="bd1c3-142">要從 .NET 客戶端取得`GrpcChannelOptions.LoggerFactory`紀錄, 可以在建立用戶端通道時設置該屬性。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-142">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="bd1c3-143">如果從 ASP.NET 酷睿應用呼叫 gRPC 服務,則可以從依賴項注入 (DI) 解析記錄器工廠:</span><span class="sxs-lookup"><span data-stu-id="bd1c3-143">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="bd1c3-144">啟用用戶端日誌記錄的另一種方法是使用[gRPC 用戶端工廠](xref:grpc/clientfactory)創建用戶端。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-144">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="bd1c3-145">在用戶端工廠註冊並從 DI 解析的 gRPC 用戶端將自動使用應用配置的日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-145">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="bd1c3-146">如果你的應用不使用DI,那麼您可以使用`ILoggerFactory`[記錄器工廠](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)創建新實例。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-146">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="bd1c3-147">要造訪此方法,請將[Microsoft.擴展.日誌記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)包添加到你的應用。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-147">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a><span data-ttu-id="bd1c3-148">gRPC 客戶端紀錄範圍</span><span class="sxs-lookup"><span data-stu-id="bd1c3-148">gRPC client log scopes</span></span>

<span data-ttu-id="bd1c3-149">gRPC 客戶端到 gRPC 呼叫期間所做的紀錄的[紀錄紀錄的功能 。](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes)</span><span class="sxs-lookup"><span data-stu-id="bd1c3-149">The gRPC client adds a [logging scope](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes) to logs made during a gRPC call.</span></span> <span data-ttu-id="bd1c3-150">範圍有與 gRPC 呼叫相關的中繼資料:</span><span class="sxs-lookup"><span data-stu-id="bd1c3-150">The scope has metadata related to the gRPC call:</span></span>

* <span data-ttu-id="bd1c3-151">**GrpcMethodtype** - gRPC 方法類型。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-151">**GrpcMethodType** - The gRPC method type.</span></span> <span data-ttu-id="bd1c3-152">可能的值是`Grpc.Core.MethodType`枚舉中的名稱,例如未編號</span><span class="sxs-lookup"><span data-stu-id="bd1c3-152">Possible values are names from `Grpc.Core.MethodType` enum, e.g. Unary</span></span>
* <span data-ttu-id="bd1c3-153">**Grpcuri** - gRPC 方法的相對 URI,例如 /greet。格雷特/賽赫洛</span><span class="sxs-lookup"><span data-stu-id="bd1c3-153">**GrpcUri** - The relative URI of the gRPC method, e.g. /greet.Greeter/SayHellos</span></span>

#### <a name="sample-logging-output"></a><span data-ttu-id="bd1c3-154">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="bd1c3-154">Sample logging output</span></span>

<span data-ttu-id="bd1c3-155">下面是 gRPC`Debug`客戶端 的主控台輸出範例:</span><span class="sxs-lookup"><span data-stu-id="bd1c3-155">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="tracing"></a><span data-ttu-id="bd1c3-156">追蹤</span><span class="sxs-lookup"><span data-stu-id="bd1c3-156">Tracing</span></span>

<span data-ttu-id="bd1c3-157">gRPC 服務和 gRPC 用戶端使用[診斷源](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource)和[活動](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity)提供有關 gRPC 調用的資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-157">gRPC services and the gRPC client provide information about gRPC calls using [DiagnosticSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource) and [Activity](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity).</span></span>

* <span data-ttu-id="bd1c3-158">.NET gRPC 使用活動來表示 gRPC 調用。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-158">.NET gRPC uses an activity to represent a gRPC call.</span></span>
* <span data-ttu-id="bd1c3-159">在 gRPC 調用活動的開始和停止處,跟蹤事件寫入診斷源。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-159">Tracing events are written to the diagnostic source at the start and stop of the gRPC call activity.</span></span>
* <span data-ttu-id="bd1c3-160">跟蹤不會捕獲有關在 gRPC 流式處理調用的生存期內何時發送消息的資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-160">Tracing doesn't capture information about when messages are sent over the lifetime of gRPC streaming calls.</span></span>

### <a name="grpc-service-tracing"></a><span data-ttu-id="bd1c3-161">gRPC 服務追蹤</span><span class="sxs-lookup"><span data-stu-id="bd1c3-161">gRPC service tracing</span></span>

<span data-ttu-id="bd1c3-162">gRPC 服務託管在 ASP.NET 酷中,用於報告有關傳入 HTTP 請求的事件。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-162">gRPC services are hosted on ASP.NET Core which reports events about incoming HTTP requests.</span></span> <span data-ttu-id="bd1c3-163">gRPC 特定的中繼資料將添加到 ASP.NET Core 提供的現有 HTTP 請求診斷中。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-163">gRPC specific metadata is added to the existing HTTP request diagnostics that ASP.NET Core provides.</span></span>

* <span data-ttu-id="bd1c3-164">診斷來源名稱稱為`Microsoft.AspNetCore`。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-164">Diagnostic source name is `Microsoft.AspNetCore`.</span></span>
* <span data-ttu-id="bd1c3-165">活動名稱稱為`Microsoft.AspNetCore.Hosting.HttpRequestIn`。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-165">Activity name is `Microsoft.AspNetCore.Hosting.HttpRequestIn`.</span></span>
  * <span data-ttu-id="bd1c3-166">gRPC 調用呼叫的 gRPC 方法的名稱將添加`grpc.method`為具有名稱的標記。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-166">Name of the gRPC method invoked by the gRPC call is added as a tag with the name `grpc.method`.</span></span>
  * <span data-ttu-id="bd1c3-167">gRPC 調用的狀態代碼完成後,將添加為帶有`grpc.status_code`名稱 的標記。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-167">Status code of the gRPC call when it is complete is added as a tag with the name `grpc.status_code`.</span></span>

### <a name="grpc-client-tracing"></a><span data-ttu-id="bd1c3-168">gRPC 客戶端追蹤</span><span class="sxs-lookup"><span data-stu-id="bd1c3-168">gRPC client tracing</span></span>

<span data-ttu-id="bd1c3-169">.NET gRPC`HttpClient`用戶端 用於進行 gRPC 調用。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-169">The .NET gRPC client uses `HttpClient` to make gRPC calls.</span></span> <span data-ttu-id="bd1c3-170">儘管`HttpClient`寫入診斷事件,但 .NET gRPC 用戶端提供自定義診斷源、活動和事件,以便收集有關 gRPC 調用的完整資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-170">Although `HttpClient` writes diagnostic events, the .NET gRPC client provides a custom diagnostic source, activity and events so that complete information about a gRPC call can be collected.</span></span>

* <span data-ttu-id="bd1c3-171">診斷來源名稱稱為`Grpc.Net.Client`。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-171">Diagnostic source name is `Grpc.Net.Client`.</span></span>
* <span data-ttu-id="bd1c3-172">活動名稱稱為`Grpc.Net.Client.GrpcOut`。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-172">Activity name is `Grpc.Net.Client.GrpcOut`.</span></span>
  * <span data-ttu-id="bd1c3-173">gRPC 調用呼叫的 gRPC 方法的名稱將添加`grpc.method`為具有名稱的標記。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-173">Name of the gRPC method invoked by the gRPC call is added as a tag with the name `grpc.method`.</span></span>
  * <span data-ttu-id="bd1c3-174">gRPC 調用的狀態代碼完成後,將添加為帶有`grpc.status_code`名稱 的標記。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-174">Status code of the gRPC call when it is complete is added as a tag with the name `grpc.status_code`.</span></span>

### <a name="collecting-tracing"></a><span data-ttu-id="bd1c3-175">收集追蹤</span><span class="sxs-lookup"><span data-stu-id="bd1c3-175">Collecting tracing</span></span>

<span data-ttu-id="bd1c3-176">最簡單的使用`DiagnosticSource`方法是在應用中設定遙測庫,如[應用程式見解](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)或[Open 遙測](https://github.com/open-telemetry/opentelemetry-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-176">The easiest way to use `DiagnosticSource` is to configure a telemetry library such as [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) or [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet) in your app.</span></span> <span data-ttu-id="bd1c3-177">該庫將處理有關 gRPC 調用沿其他應用遙測的資訊。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-177">The library will process information about gRPC calls along-side other app telemetry.</span></span>

<span data-ttu-id="bd1c3-178">可以在應用程式見解等託管服務中查看跟蹤,也可以選擇運行自己的分散式跟蹤系統。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-178">Tracing can be viewed in a managed service like Application Insights, or you can choose to run your own distributed tracing system.</span></span> <span data-ttu-id="bd1c3-179">Open 遙測支援將追蹤資料匯出到[傑格](https://www.jaegertracing.io/)與[齊普金](https://zipkin.io/)。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-179">OpenTelemetry supports exporting tracing data to [Jaeger](https://www.jaegertracing.io/) and [Zipkin](https://zipkin.io/).</span></span>

<span data-ttu-id="bd1c3-180">`DiagnosticSource`可以使用使用的代碼中使用追蹤`DiagnosticListener`事件。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-180">`DiagnosticSource` can consume tracing events in code using `DiagnosticListener`.</span></span> <span data-ttu-id="bd1c3-181">有關收聽包含代碼的診斷源的資訊,請參閱[診斷源使用者指南](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener)。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-181">For information about listening to a diagnostic source with code, see the [DiagnosticSource user's guide](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener).</span></span>

> [!NOTE]
> <span data-ttu-id="bd1c3-182">遙測庫當前不捕獲 gRPC`Grpc.Net.Client.GrpcOut`特定 遙測。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-182">Telemetry libraries do not capture gRPC specific `Grpc.Net.Client.GrpcOut` telemetry currently.</span></span> <span data-ttu-id="bd1c3-183">改進捕獲此跟蹤的遙測庫的工作正在進行中。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-183">Work to improve telemetry libraries capturing this tracing is ongoing.</span></span>

## <a name="metrics"></a><span data-ttu-id="bd1c3-184">計量</span><span class="sxs-lookup"><span data-stu-id="bd1c3-184">Metrics</span></span>

<span data-ttu-id="bd1c3-185">指標是數據度量在時間間隔(例如每秒請求)的表示形式。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-185">Metrics is a representation of data measures over intervals of time, for example, requests per second.</span></span> <span data-ttu-id="bd1c3-186">指標數據允許在高級別上觀察應用的狀態。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-186">Metrics data allows observation of the state of an app at a high-level.</span></span> <span data-ttu-id="bd1c3-187">.NET gRPC指標`EventCounter`使用 發出。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-187">.NET gRPC metrics are emitted using `EventCounter`.</span></span>

### <a name="grpc-service-metrics"></a><span data-ttu-id="bd1c3-188">gRPC 服務指標</span><span class="sxs-lookup"><span data-stu-id="bd1c3-188">gRPC service metrics</span></span>

<span data-ttu-id="bd1c3-189">gRPC 伺服器指標報告在`Grpc.AspNetCore.Server`事件 源上。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-189">gRPC server metrics are reported on `Grpc.AspNetCore.Server` event source.</span></span>

| <span data-ttu-id="bd1c3-190">名稱</span><span class="sxs-lookup"><span data-stu-id="bd1c3-190">Name</span></span>                      | <span data-ttu-id="bd1c3-191">描述</span><span class="sxs-lookup"><span data-stu-id="bd1c3-191">Description</span></span>                   |
| --------------------------|-------------------------------|
| `total-calls`             | <span data-ttu-id="bd1c3-192">呼叫總數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-192">Total Calls</span></span>                   |
| `current-calls`           | <span data-ttu-id="bd1c3-193">目前通話</span><span class="sxs-lookup"><span data-stu-id="bd1c3-193">Current Calls</span></span>                 |
| `calls-failed`            | <span data-ttu-id="bd1c3-194">通話總數失敗</span><span class="sxs-lookup"><span data-stu-id="bd1c3-194">Total Calls Failed</span></span>            |
| `calls-deadline-exceeded` | <span data-ttu-id="bd1c3-195">超過總呼叫截止日期</span><span class="sxs-lookup"><span data-stu-id="bd1c3-195">Total Calls Deadline Exceeded</span></span> |
| `messages-sent`           | <span data-ttu-id="bd1c3-196">傳送的郵件總數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-196">Total Messages Sent</span></span>           |
| `messages-received`       | <span data-ttu-id="bd1c3-197">已接收的訊息總數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-197">Total Messages Received</span></span>       |
| `calls-unimplemented`     | <span data-ttu-id="bd1c3-198">未實用的總呼叫數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-198">Total Calls Unimplemented</span></span>     |

<span data-ttu-id="bd1c3-199">ASP.NET核心還提供其自己的`Microsoft.AspNetCore.Hosting`事件源指標。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-199">ASP.NET Core also provides its own metrics on `Microsoft.AspNetCore.Hosting` event source.</span></span>

### <a name="grpc-client-metrics"></a><span data-ttu-id="bd1c3-200">gRPC 客戶端指標</span><span class="sxs-lookup"><span data-stu-id="bd1c3-200">gRPC client metrics</span></span>

<span data-ttu-id="bd1c3-201">gRPC 客戶端指標報告`Grpc.Net.Client`在事件 來源上。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-201">gRPC client metrics are reported on `Grpc.Net.Client` event source.</span></span>

| <span data-ttu-id="bd1c3-202">名稱</span><span class="sxs-lookup"><span data-stu-id="bd1c3-202">Name</span></span>                      | <span data-ttu-id="bd1c3-203">描述</span><span class="sxs-lookup"><span data-stu-id="bd1c3-203">Description</span></span>                   |
| --------------------------|-------------------------------|
| `total-calls`             | <span data-ttu-id="bd1c3-204">呼叫總數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-204">Total Calls</span></span>                   |
| `current-calls`           | <span data-ttu-id="bd1c3-205">目前通話</span><span class="sxs-lookup"><span data-stu-id="bd1c3-205">Current Calls</span></span>                 |
| `calls-failed`            | <span data-ttu-id="bd1c3-206">通話總數失敗</span><span class="sxs-lookup"><span data-stu-id="bd1c3-206">Total Calls Failed</span></span>            |
| `calls-deadline-exceeded` | <span data-ttu-id="bd1c3-207">超過總呼叫截止日期</span><span class="sxs-lookup"><span data-stu-id="bd1c3-207">Total Calls Deadline Exceeded</span></span> |
| `messages-sent`           | <span data-ttu-id="bd1c3-208">傳送的郵件總數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-208">Total Messages Sent</span></span>           |
| `messages-received`       | <span data-ttu-id="bd1c3-209">已接收的訊息總數</span><span class="sxs-lookup"><span data-stu-id="bd1c3-209">Total Messages Received</span></span>       |

### <a name="observe-metrics"></a><span data-ttu-id="bd1c3-210">觀察指標</span><span class="sxs-lookup"><span data-stu-id="bd1c3-210">Observe metrics</span></span>

<span data-ttu-id="bd1c3-211">[點網計數器](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters)是一種用於臨時運行狀況監控和一級性能調查的性能監控工具。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-211">[dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) is a performance monitoring tool for ad-hoc health monitoring and first-level performance investigation.</span></span> <span data-ttu-id="bd1c3-212">使用或`Grpc.AspNetCore.Server``Grpc.Net.Client`作為提供程式名稱監視 .NET 應用。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-212">Monitor a .NET app with either `Grpc.AspNetCore.Server` or `Grpc.Net.Client` as the provider name.</span></span>

```console
> dotnet-counters monitor --process-id 1902 Grpc.AspNetCore.Server

Press p to pause, r to resume, q to quit.
    Status: Running
[Grpc.AspNetCore.Server]
    Total Calls                                 300
    Current Calls                               5
    Total Calls Failed                          0
    Total Calls Deadline Exceeded               0
    Total Messages Sent                         295
    Total Messages Received                     300
    Total Calls Unimplemented                   0
```

<span data-ttu-id="bd1c3-213">觀察 gRPC 指標的另一種方法是使用應用程式見解的[Microsoft.Application Insights.eventCounterCollector 包](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)捕獲計數器數據。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-213">Another way to observe gRPC metrics is to capture counter data using Application Insights's [Microsoft.ApplicationInsights.EventCounterCollector package](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).</span></span> <span data-ttu-id="bd1c3-214">設置後,應用程式見解在運行時收集常見的 .NET 計數器。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-214">Once setup, Application Insights collects common .NET counters at runtime.</span></span> <span data-ttu-id="bd1c3-215">預設情況下不收集 gRPC 的計數器,但可以自訂應用見解[以包括其他計數器](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected)。</span><span class="sxs-lookup"><span data-stu-id="bd1c3-215">gRPC's counters are not collected by default, but App Insights can be [customized to include additional counters](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).</span></span>

<span data-ttu-id="bd1c3-216">指定用於應用程式洞察的 gRPC 計數器,以便*以 Startup.cs*收集:</span><span class="sxs-lookup"><span data-stu-id="bd1c3-216">Specify the gRPC counters for Application Insight to collect in *Startup.cs*:</span></span>

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // Configure App Insights to collect gRPC counters gRPC services hosted in an ASP.NET Core app
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "current-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "total-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "calls-failed"));
            }
        );
    }
```

## <a name="additional-resources"></a><span data-ttu-id="bd1c3-217">其他資源</span><span class="sxs-lookup"><span data-stu-id="bd1c3-217">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
