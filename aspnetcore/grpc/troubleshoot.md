---
title: 在 .NET 內核上排除 gRPC 故障
author: jamesnk
description: 在 .NET 內核上使用 gRPC 時排除錯誤。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 10/16/2019
uid: grpc/troubleshoot
ms.openlocfilehash: c501cda14f3bac9297695ece59cbc4634e4b7895
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664126"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="6b7d7-103">在 .NET 內核上排除 gRPC 故障</span><span class="sxs-lookup"><span data-stu-id="6b7d7-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="6b7d7-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="6b7d7-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="6b7d7-105">本文件討論了在 .NET 上開發 gRPC 應用時經常遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-105">This document discusses commonly encountered problems when developing gRPC apps on .NET.</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="6b7d7-106">用戶端與服務 SSL/TLS 設定不符合</span><span class="sxs-lookup"><span data-stu-id="6b7d7-106">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="6b7d7-107">默認情況下,gRPC 範本和範例使用[傳輸層安全 (TLS)](https://tools.ietf.org/html/rfc5246)來保護 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-107">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="6b7d7-108">gRPC 用戶端需要使用安全連接成功調用安全的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-108">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="6b7d7-109">您可以在應用啟動時編寫的日誌中驗證 ASP.NET Core gRPC 服務是否使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-109">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="6b7d7-110">該服務將在 HTTPS 終結點上偵聽:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-110">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="6b7d7-111">.NET Core 客戶`https`端必須在 伺服器位址中使用以進行安全連線的呼叫:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-111">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

<span data-ttu-id="6b7d7-112">所有 gRPC 客戶端實現都支援 TLS。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-112">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="6b7d7-113">來自其他語言的 gRPC 用戶端通常`SslCredentials`需要配置的通道。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-113">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="6b7d7-114">`SslCredentials`指定用戶端將使用的證書,並且必須使用該證書而不是不安全的認證。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-114">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="6b7d7-115">有關將不同的 gRPC 客戶端設定為使用 TLS 的範例,請參考[gRPC 認證](https://www.grpc.io/docs/guides/auth/)。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-115">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a><span data-ttu-id="6b7d7-116">使用不受信任的/無效憑證呼叫 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="6b7d7-116">Call a gRPC service with an untrusted/invalid certificate</span></span>

<span data-ttu-id="6b7d7-117">.NET gRPC 用戶端要求服務具有受信任的證書。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-117">The .NET gRPC client requires the service to have a trusted certificate.</span></span> <span data-ttu-id="6b7d7-118">在沒有受信任憑證的情況下呼叫 gRPC 服務時,將傳回以下錯誤訊息:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-118">The following error message is returned when calling a gRPC service without a trusted certificate:</span></span>

> <span data-ttu-id="6b7d7-119">未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-119">Unhandled exception.</span></span> <span data-ttu-id="6b7d7-120">系統.Net.http.http請求異常:無法建立 SSL 連接,請參閱內部異常。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-120">System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.</span></span>
> <span data-ttu-id="6b7d7-121">---> System.Security.Authentication.AuthenticationException: 根據驗證程序，遠端憑證是無效的。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-121">---> System.Security.Authentication.AuthenticationException: The remote certificate is invalid according to the validation procedure.</span></span>

<span data-ttu-id="6b7d7-122">如果您在本地測試應用且 ASP.NET酷睿 HTTPS 開發證書不受信任,則可能會看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-122">You may see this error if you are testing your app locally and the ASP.NET Core HTTPS development certificate is not trusted.</span></span> <span data-ttu-id="6b7d7-123">如需修正此問題的指示，請參閱[信任 Windows 和 macOS上的 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-123">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

<span data-ttu-id="6b7d7-124">如果您在另一台電腦上調用 gRPC 服務,並且無法信任證書,則可以將 gRPC 用戶端配置為忽略無效的證書。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-124">If you are calling a gRPC service on another machine and are unable to trust the certificate then the gRPC client can be configured to ignore the invalid certificate.</span></span> <span data-ttu-id="6b7d7-125">以下代碼使用[httpClientHandler.Server 憑證驗證回撥](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback)允許沒有信任憑證的呼叫:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-125">The following code uses [HttpClientHandler.ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) to allow calls without a trusted certificate:</span></span>

```csharp
var httpClientHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpClientHandler.ServerCertificateCustomValidationCallback = 
    HttpClientHandler.DangerousAcceptAnyServerCertificateValidator;
var httpClient = new HttpClient(httpClientHandler);

var channel = GrpcChannel.ForAddress("https://localhost:5001",
    new GrpcChannelOptions { HttpClient = httpClient });
var client = new Greet.GreeterClient(channel);
```

> [!WARNING]
> <span data-ttu-id="6b7d7-126">不受信任的證書應僅在應用開發期間使用。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-126">Untrusted certificates should only be used during app development.</span></span> <span data-ttu-id="6b7d7-127">生產應用應始終使用有效的證書。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-127">Production apps should always use valid certificates.</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="6b7d7-128">使用 .NET 核心用戶端呼叫不安全的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="6b7d7-128">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="6b7d7-129">使用 .NET Core 用戶端呼叫不安全的 gRPC 服務需要額外的配置。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-129">Additional configuration is required to call insecure gRPC services with the .NET Core client.</span></span> <span data-ttu-id="6b7d7-130">gRPC 客戶端`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`必須將 交換`true`機設定`http`為 伺服器位址 並在伺服器位址中使用:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-130">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch(
    "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="6b7d7-131">無法啟動 macOS 上的 ASP.NET 核心 gRPC 應用</span><span class="sxs-lookup"><span data-stu-id="6b7d7-131">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="6b7d7-132">Kestrel 不支援 MACOS 上 TLS 和舊版本的 Windows 版本(如 Windows 7)的 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-132">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="6b7d7-133">預設情況下,ASP.NET核心 gRPC 範本和範例使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="6b7d7-134">您試著啟動 gRPC 伺服器時,您將看到以下錯誤訊息:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="6b7d7-135">無法綁定到https://localhost:5001IPv4 回環介面:「macOS 上不支援」HTTP/2 通過 TLS",因為缺少 ALPN 支援。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="6b7d7-136">要解決此問題,請將 Kestrel 和 gRPC 用戶端配置為*不使用*TLS 使用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="6b7d7-137">您只應在開發期間執行此操作。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-137">You should only do this during development.</span></span> <span data-ttu-id="6b7d7-138">不使用 TLS 將導致在沒有加密的情況下發送 gRPC 消息。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="6b7d7-139">Kestrel 必須在*Program.cs*中設定沒有 TLS 的 HTTP/2 終結點:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-139">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = 
                    HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="6b7d7-140">當沒有 TLS 設定 HTTP/2 終結點時,必須將終結點的[ListenOptions.協定](xref:fundamentals/servers/kestrel#listenoptionsprotocols)設定為`HttpProtocols.Http2`。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-140">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="6b7d7-141">`HttpProtocols.Http1AndHttp2`不能使用,因為 TLS 需要協商 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-141">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="6b7d7-142">如果沒有 TLS,則到終結點的所有連接默認為 HTTP/1.1,gRPC 調用失敗。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-142">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="6b7d7-143">gRPC 客戶端也必須配置為不使用 TLS。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-143">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="6b7d7-144">有關詳細資訊,請參閱使用[.NET 核心用戶端呼叫不安全的 gRPC 服務](#call-insecure-grpc-services-with-net-core-client)。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-144">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="6b7d7-145">沒有 TLS 的 HTTP/2 應僅在應用開發期間使用。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-145">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="6b7d7-146">生產應用應始終使用傳輸安全性。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-146">Production apps should always use transport security.</span></span> <span data-ttu-id="6b7d7-147">有關詳細資訊,請參閱[gRPC 中有關 ASP.NET 核心的安全注意事項](xref:grpc/security#transport-security)。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-147">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="6b7d7-148">gRPC C++ 資產不是從 .proto 檔案產生的代碼</span><span class="sxs-lookup"><span data-stu-id="6b7d7-148">gRPC C# assets are not code generated from .proto files</span></span>

<span data-ttu-id="6b7d7-149">gRPC 代碼生成的具體用戶端和服務基類需要從專案中引用原檔和技術。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-149">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="6b7d7-150">您必須包括:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-150">You must include:</span></span>

* <span data-ttu-id="6b7d7-151">*.proto*在項目群組中使用. `<Protobuf>`</span><span class="sxs-lookup"><span data-stu-id="6b7d7-151">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="6b7d7-152">[匯入*的 .proto*檔](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions)必須由專案引用。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-152">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="6b7d7-153">封裝參考 gRPC 工具套件[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-153">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="6b7d7-154">有關生成 gRPC C++ 資產的<xref:grpc/basics>詳細資訊, 請參閱。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-154">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="6b7d7-155">預設情況下,`<Protobuf>`引用生成具體的用戶端和服務基類。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-155">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="6b7d7-156">引用元素的屬性`GrpcServices`可用於限制 C# 資產生成。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-156">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="6b7d7-157">有效`GrpcServices`選項包括:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-157">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="6b7d7-158">`Both`( 不存在時預設 )</span><span class="sxs-lookup"><span data-stu-id="6b7d7-158">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

<span data-ttu-id="6b7d7-159">託管 gRPC 服務的 ASP.NET 核心 Web 應用只需要產生的服務基類:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-159">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="6b7d7-160">進行 gRPC 呼叫 gRPC 的用戶端應用只需要產生的具體用戶端:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-160">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```

## <a name="wpf-projects-unable-to-generate-grpc-c-assets-from-proto-files"></a><span data-ttu-id="6b7d7-161">WPF 專案無法從 .proto 檔案產生 gRPC C# 資產</span><span class="sxs-lookup"><span data-stu-id="6b7d7-161">WPF projects unable to generate gRPC C# assets from .proto files</span></span>

<span data-ttu-id="6b7d7-162">WPF 專案有一[個已知問題](https://github.com/dotnet/wpf/issues/810),可阻止 gRPC 代碼生成正常工作。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-162">WPF projects have a [known issue](https://github.com/dotnet/wpf/issues/810) that prevents gRPC code generation from working correctly.</span></span> <span data-ttu-id="6b7d7-163">用參考`Grpc.Tools`與 *.proto*檔案在 WPF 專案中產生的任何 gRPC 類型在使用時都會產生編譯錯誤:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-163">Any gRPC types generated in a WPF project by referencing `Grpc.Tools` and *.proto* files will create compilation errors when used:</span></span>

> <span data-ttu-id="6b7d7-164">錯誤 CS0246: 找不到類型或命名空間名稱「MyGrpc Services」(您是否缺少使用指令或程式集引用?</span><span class="sxs-lookup"><span data-stu-id="6b7d7-164">error CS0246: The type or namespace name 'MyGrpcServices' could not be found (are you missing a using directive or an assembly reference?)</span></span>

<span data-ttu-id="6b7d7-165">您可以通過:</span><span class="sxs-lookup"><span data-stu-id="6b7d7-165">You can workaround this issue by:</span></span>

1. <span data-ttu-id="6b7d7-166">創建新的 .NET Core 類庫專案。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-166">Create a new .NET Core class library project.</span></span>
2. <span data-ttu-id="6b7d7-167">在新項目中,加入參考以開啟[*\*從 .proto*檔案產生 C# 程式碼](xref:grpc/basics#generated-c-assets):</span><span class="sxs-lookup"><span data-stu-id="6b7d7-167">In the new project, add references to enable [C# code generation from *\*.proto* files](xref:grpc/basics#generated-c-assets):</span></span>
    * <span data-ttu-id="6b7d7-168">添加對[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)套件的包引用。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-168">Add a package reference to [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) package.</span></span>
    * <span data-ttu-id="6b7d7-169">將*\*.proto*檔案`<Protobuf>`添加到專案組。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-169">Add *\*.proto* files to the `<Protobuf>` item group.</span></span>
3. <span data-ttu-id="6b7d7-170">在 WPF 應用程式中,添加對新專案的引用。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-170">In the WPF application, add a reference to the new project.</span></span>

<span data-ttu-id="6b7d7-171">WPF 應用程式可以使用新類庫專案中生成的 gRPC 類型。</span><span class="sxs-lookup"><span data-stu-id="6b7d7-171">The WPF application can use the gRPC generated types from the new class library project.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]
