---
title: 針對 .NET Core 上的 gRPC 進行疑難排解
author: jamesnk
description: 針對在 .NET Core 上使用 gRPC 時的錯誤進行疑難排解。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/12/2019
uid: grpc/troubleshoot
ms.openlocfilehash: ad74bfa57d2dde316734d55d86075f463e78ee56
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029031"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="5df3a-103">針對 .NET Core 上的 gRPC 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="5df3a-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="5df3a-104">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="5df3a-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="5df3a-105">用戶端與服務 SSL/TLS 設定不相符</span><span class="sxs-lookup"><span data-stu-id="5df3a-105">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="5df3a-106">GRPC 範本和範例預設會使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)來保護 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5df3a-106">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="5df3a-107">gRPC 用戶端必須使用安全連線, 才能順利呼叫受保護的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5df3a-107">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="5df3a-108">您可以確認 ASP.NET Core gRPC 服務在應用程式啟動時所寫入的記錄中使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="5df3a-108">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="5df3a-109">服務將接聽 HTTPS 端點:</span><span class="sxs-lookup"><span data-stu-id="5df3a-109">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="5df3a-110">.Net Core 用戶端必須在`https`伺服器位址中使用, 以透過安全的連線進行呼叫:</span><span class="sxs-lookup"><span data-stu-id="5df3a-110">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

<span data-ttu-id="5df3a-111">所有 gRPC 用戶端都支援 TLS。</span><span class="sxs-lookup"><span data-stu-id="5df3a-111">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="5df3a-112">從其他語言 gRPC 用戶端時, 通常需要使用`SslCredentials`設定的通道。</span><span class="sxs-lookup"><span data-stu-id="5df3a-112">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="5df3a-113">`SslCredentials`指定用戶端將使用的憑證, 而且必須使用它來取代不安全的認證。</span><span class="sxs-lookup"><span data-stu-id="5df3a-113">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="5df3a-114">如需將不同的 gRPC 用戶端執行設定為使用 TLS 的範例, 請參閱[GRPC Authentication](https://www.grpc.io/docs/guides/auth/)。</span><span class="sxs-lookup"><span data-stu-id="5df3a-114">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="5df3a-115">使用 .NET Core 用戶端呼叫不安全的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="5df3a-115">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="5df3a-116">需要其他設定, 才能使用 .NET Core 用戶端呼叫不安全的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5df3a-116">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="5df3a-117">GRPC 用戶端必須將`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`參數設定為`true` , 並在伺服器位址中使用: `http`</span><span class="sxs-lookup"><span data-stu-id="5df3a-117">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="5df3a-118">無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式</span><span class="sxs-lookup"><span data-stu-id="5df3a-118">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="5df3a-119">Kestrel 不支援 macOS 上的 TLS 和舊版 Windows (例如 Windows 7) 上的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="5df3a-119">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="5df3a-120">ASP.NET Core gRPC 範本和範例預設會使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="5df3a-120">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="5df3a-121">當您嘗試啟動 gRPC 伺服器時, 您會看到下列錯誤訊息:</span><span class="sxs-lookup"><span data-stu-id="5df3a-121">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="5df3a-122">無法在 IPv4 回 https://localhost:5001 送介面上系結至:由於遺漏 ALPN 支援, macOS 上不支援 HTTP/2 over TLS。 '。</span><span class="sxs-lookup"><span data-stu-id="5df3a-122">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="5df3a-123">若要解決此問題, 請將 Kestrel 和 gRPC 用戶端設定為使用*沒有*TLS 的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="5df3a-123">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="5df3a-124">您應該只在開發期間執行此動作。</span><span class="sxs-lookup"><span data-stu-id="5df3a-124">You should only do this during development.</span></span> <span data-ttu-id="5df3a-125">不使用 TLS 會導致傳送未加密的 gRPC 訊息。</span><span class="sxs-lookup"><span data-stu-id="5df3a-125">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="5df3a-126">Kestrel 必須在*Program.cs*中設定沒有 TLS 的 HTTP/2 端點:</span><span class="sxs-lookup"><span data-stu-id="5df3a-126">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

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

<span data-ttu-id="5df3a-127">GRPC 用戶端也必須設定為不使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="5df3a-127">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="5df3a-128">如需詳細資訊, 請參閱[使用 .Net Core 用戶端呼叫不安全的 gRPC 服務](#call-insecure-grpc-services-with-net-core-client)。</span><span class="sxs-lookup"><span data-stu-id="5df3a-128">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="5df3a-129">只有在應用程式開發期間, 才應該使用不含 TLS 的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="5df3a-129">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="5df3a-130">生產應用程式應該一律使用傳輸安全性。</span><span class="sxs-lookup"><span data-stu-id="5df3a-130">Production apps should always use transport security.</span></span> <span data-ttu-id="5df3a-131">如需詳細資訊, 請參閱[gRPC for ASP.NET Core 中的安全性考慮](xref:grpc/security#transport-security)。</span><span class="sxs-lookup"><span data-stu-id="5df3a-131">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="5df3a-132">gRPC C#資產不是從 *\*proto*檔案產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="5df3a-132">gRPC C# assets are not code generated from *\*.proto* files</span></span>

<span data-ttu-id="5df3a-133">gRPC 程式碼產生具體的用戶端和服務基類需要從專案參考 protobuf 檔案和工具。</span><span class="sxs-lookup"><span data-stu-id="5df3a-133">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="5df3a-134">您必須包括:</span><span class="sxs-lookup"><span data-stu-id="5df3a-134">You must include:</span></span>

* <span data-ttu-id="5df3a-135">您想要在`<Protobuf>`專案群組中使用的 *. proto*檔案。</span><span class="sxs-lookup"><span data-stu-id="5df3a-135">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="5df3a-136">專案必須參考已匯[入的*proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions)檔案。</span><span class="sxs-lookup"><span data-stu-id="5df3a-136">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="5df3a-137">GRPC 工具套件[gRPC](https://www.nuget.org/packages/Grpc.Tools/)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5df3a-137">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="5df3a-138">如需產生 gRPC C#資產的詳細資訊, <xref:grpc/basics>請參閱。</span><span class="sxs-lookup"><span data-stu-id="5df3a-138">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="5df3a-139">根據預設, `<Protobuf>`參考會產生具體的用戶端和服務基類。</span><span class="sxs-lookup"><span data-stu-id="5df3a-139">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="5df3a-140">參考元素的`GrpcServices`屬性可用來限制C#資產產生。</span><span class="sxs-lookup"><span data-stu-id="5df3a-140">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="5df3a-141">有效`GrpcServices`的選項為:</span><span class="sxs-lookup"><span data-stu-id="5df3a-141">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="5df3a-142">`Both`(不存在時的預設值)</span><span class="sxs-lookup"><span data-stu-id="5df3a-142">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="5df3a-143">裝載 gRPC 服務的 ASP.NET Core web 應用程式只需要產生服務基類:</span><span class="sxs-lookup"><span data-stu-id="5df3a-143">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="5df3a-144">建立 gRPC 呼叫的 gRPC 用戶端應用程式只需要產生的具體用戶端:</span><span class="sxs-lookup"><span data-stu-id="5df3a-144">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
