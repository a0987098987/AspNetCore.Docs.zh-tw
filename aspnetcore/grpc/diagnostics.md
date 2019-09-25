---
title: .NET 上 gRPC 中的記錄和診斷
author: jamesnk
description: 瞭解如何從 .NET 上的 gRPC 應用程式收集診斷。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250728"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="4e053-103">.NET 上 gRPC 中的記錄和診斷</span><span class="sxs-lookup"><span data-stu-id="4e053-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="4e053-104">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="4e053-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="4e053-105">本文提供從您的 gRPC 應用程式收集診斷資訊，以協助疑難排解問題的指引。</span><span class="sxs-lookup"><span data-stu-id="4e053-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="4e053-106">gRPC services 記錄</span><span class="sxs-lookup"><span data-stu-id="4e053-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="4e053-107">伺服器端記錄可能包含來自您應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="4e053-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="4e053-108">**絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。</span><span class="sxs-lookup"><span data-stu-id="4e053-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="4e053-109">因為 gRPC 服務是在 ASP.NET Core 上主控，所以它會使用 ASP.NET Core 記錄系統。</span><span class="sxs-lookup"><span data-stu-id="4e053-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="4e053-110">在預設設定中，gRPC 會記錄非常少的資訊，但這可加以設定。</span><span class="sxs-lookup"><span data-stu-id="4e053-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="4e053-111">如需設定 ASP.NET Core 記錄的詳細資訊，請參閱有關[ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration)的檔。</span><span class="sxs-lookup"><span data-stu-id="4e053-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="4e053-112">gRPC 會將記錄新增`Grpc`至類別之下。</span><span class="sxs-lookup"><span data-stu-id="4e053-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="4e053-113">若要啟用 gRPC 的詳細記錄，請`Grpc`將下列專案`Debug`新增至中 `LogLevel` `Logging`的子區段，以在 appsettings 中設定層級的前置詞：</span><span class="sxs-lookup"><span data-stu-id="4e053-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

<span data-ttu-id="4e053-114">您也可以在*Startup.cs*中使用`ConfigureLogging`來設定此項：</span><span class="sxs-lookup"><span data-stu-id="4e053-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

<span data-ttu-id="4e053-115">如果您不是使用以 JSON 為基礎的設定，請在您的配置系統中設定下列設定值：</span><span class="sxs-lookup"><span data-stu-id="4e053-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="4e053-116">請查看設定系統的檔，以判斷如何指定嵌套的設定值。</span><span class="sxs-lookup"><span data-stu-id="4e053-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="4e053-117">例如，使用環境變數時，會使用`_`兩個字元，而不`:`是（ `Logging__LogLevel__Grpc`例如）。</span><span class="sxs-lookup"><span data-stu-id="4e053-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="4e053-118">針對您的應用`Debug`程式收集更詳細的診斷資訊時，我們建議使用此層級。</span><span class="sxs-lookup"><span data-stu-id="4e053-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="4e053-119">`Trace`層級會產生非常低層級的診斷，而且很少需要診斷應用程式中的問題。</span><span class="sxs-lookup"><span data-stu-id="4e053-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="4e053-120">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="4e053-120">Sample logging output</span></span>

<span data-ttu-id="4e053-121">以下是 gRPC 服務`Debug`層級的主控台輸出範例：</span><span class="sxs-lookup"><span data-stu-id="4e053-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

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

## <a name="access-server-side-logs"></a><span data-ttu-id="4e053-122">存取伺服器端記錄</span><span class="sxs-lookup"><span data-stu-id="4e053-122">Access server-side logs</span></span>

<span data-ttu-id="4e053-123">您存取伺服器端記錄的方式，取決於您執行的環境。</span><span class="sxs-lookup"><span data-stu-id="4e053-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="4e053-124">作為主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="4e053-124">As a console app</span></span>

<span data-ttu-id="4e053-125">如果您是在主控台應用程式中執行，預設應該啟用[主控台記錄器](xref:fundamentals/logging/index#console-provider)。</span><span class="sxs-lookup"><span data-stu-id="4e053-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="4e053-126">gRPC 記錄將會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="4e053-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="4e053-127">其他環境</span><span class="sxs-lookup"><span data-stu-id="4e053-127">Other environments</span></span>

<span data-ttu-id="4e053-128">如果應用程式部署到另一個環境（例如 Docker、Kubernetes 或 Windows 服務），請參閱<xref:fundamentals/logging/index> ，以取得有關如何設定適用于環境之記錄提供者的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="4e053-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="4e053-129">gRPC 用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="4e053-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="4e053-130">用戶端記錄可能包含來自您應用程式的機密資訊。</span><span class="sxs-lookup"><span data-stu-id="4e053-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="4e053-131">**絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。</span><span class="sxs-lookup"><span data-stu-id="4e053-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="4e053-132">若要從 .net 用戶端取得記錄，您可以在`GrpcChannelOptions.LoggerFactory`建立用戶端通道時設定屬性。</span><span class="sxs-lookup"><span data-stu-id="4e053-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="4e053-133">如果您是從 ASP.NET Core 應用程式呼叫 gRPC 服務，則可以從相依性插入（DI）解析記錄器 factory：</span><span class="sxs-lookup"><span data-stu-id="4e053-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="4e053-134">啟用用戶端記錄的另一種方式是使用[gRPC 用戶端 factory](xref:grpc/clientfactory)來建立用戶端。</span><span class="sxs-lookup"><span data-stu-id="4e053-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="4e053-135">向用戶端 factory 註冊並從 DI 解析的 gRPC 用戶端，會自動使用應用程式設定的記錄。</span><span class="sxs-lookup"><span data-stu-id="4e053-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="4e053-136">如果您的應用程式未使用 DI，則您可以`ILoggerFactory`使用[server.loggerfactory](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)建立新的實例。</span><span class="sxs-lookup"><span data-stu-id="4e053-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="4e053-137">若要存取此方法，請在您的應用程式中新增[Microsoft Extensions. 記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)套件。</span><span class="sxs-lookup"><span data-stu-id="4e053-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="4e053-138">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="4e053-138">Sample logging output</span></span>

<span data-ttu-id="4e053-139">以下是 gRPC 用戶端`Debug`層級的主控台輸出範例：</span><span class="sxs-lookup"><span data-stu-id="4e053-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="4e053-140">其他資源</span><span class="sxs-lookup"><span data-stu-id="4e053-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
