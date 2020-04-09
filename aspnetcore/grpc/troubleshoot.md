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
# <a name="troubleshoot-grpc-on-net-core"></a>在 .NET 內核上排除 gRPC 故障

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

本文件討論了在 .NET 上開發 gRPC 應用時經常遇到的問題。

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>用戶端與服務 SSL/TLS 設定不符合

默認情況下,gRPC 範本和範例使用[傳輸層安全 (TLS)](https://tools.ietf.org/html/rfc5246)來保護 gRPC 服務。 gRPC 用戶端需要使用安全連接成功調用安全的 gRPC 服務。

您可以在應用啟動時編寫的日誌中驗證 ASP.NET Core gRPC 服務是否使用 TLS。 該服務將在 HTTPS 終結點上偵聽:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

.NET Core 客戶`https`端必須在 伺服器位址中使用以進行安全連線的呼叫:

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

所有 gRPC 客戶端實現都支援 TLS。 來自其他語言的 gRPC 用戶端通常`SslCredentials`需要配置的通道。 `SslCredentials`指定用戶端將使用的證書,並且必須使用該證書而不是不安全的認證。 有關將不同的 gRPC 客戶端設定為使用 TLS 的範例,請參考[gRPC 認證](https://www.grpc.io/docs/guides/auth/)。

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a>使用不受信任的/無效憑證呼叫 gRPC 服務

.NET gRPC 用戶端要求服務具有受信任的證書。 在沒有受信任憑證的情況下呼叫 gRPC 服務時,將傳回以下錯誤訊息:

> 未處理的例外狀況。 系統.Net.http.http請求異常:無法建立 SSL 連接,請參閱內部異常。
> ---> System.Security.Authentication.AuthenticationException: 根據驗證程序，遠端憑證是無效的。

如果您在本地測試應用且 ASP.NET酷睿 HTTPS 開發證書不受信任,則可能會看到此錯誤。 如需修正此問題的指示，請參閱[信任 Windows 和 macOS上的 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。

如果您在另一台電腦上調用 gRPC 服務,並且無法信任證書,則可以將 gRPC 用戶端配置為忽略無效的證書。 以下代碼使用[httpClientHandler.Server 憑證驗證回撥](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback)允許沒有信任憑證的呼叫:

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
> 不受信任的證書應僅在應用開發期間使用。 生產應用應始終使用有效的證書。

## <a name="call-insecure-grpc-services-with-net-core-client"></a>使用 .NET 核心用戶端呼叫不安全的 gRPC 服務

使用 .NET Core 用戶端呼叫不安全的 gRPC 服務需要額外的配置。 gRPC 客戶端`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`必須將 交換`true`機設定`http`為 伺服器位址 並在伺服器位址中使用:

```csharp
// This switch must be set before creating the GrpcChannel/HttpClient.
AppContext.SetSwitch(
    "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

// The port number(5000) must match the port of the gRPC server.
var channel = GrpcChannel.ForAddress("http://localhost:5000");
var client = new Greet.GreeterClient(channel);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>無法啟動 macOS 上的 ASP.NET 核心 gRPC 應用

Kestrel 不支援 MACOS 上 TLS 和舊版本的 Windows 版本(如 Windows 7)的 HTTP/2。 預設情況下,ASP.NET核心 gRPC 範本和範例使用 TLS。 您試著啟動 gRPC 伺服器時,您將看到以下錯誤訊息:

> 無法綁定到https://localhost:5001IPv4 回環介面:「macOS 上不支援」HTTP/2 通過 TLS",因為缺少 ALPN 支援。

要解決此問題,請將 Kestrel 和 gRPC 用戶端配置為*不使用*TLS 使用 HTTP/2。 您只應在開發期間執行此操作。 不使用 TLS 將導致在沒有加密的情況下發送 gRPC 消息。

Kestrel 必須在*Program.cs*中設定沒有 TLS 的 HTTP/2 終結點:

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

當沒有 TLS 設定 HTTP/2 終結點時,必須將終結點的[ListenOptions.協定](xref:fundamentals/servers/kestrel#listenoptionsprotocols)設定為`HttpProtocols.Http2`。 `HttpProtocols.Http1AndHttp2`不能使用,因為 TLS 需要協商 HTTP/2。 如果沒有 TLS,則到終結點的所有連接默認為 HTTP/1.1,gRPC 調用失敗。

gRPC 客戶端也必須配置為不使用 TLS。 有關詳細資訊,請參閱使用[.NET 核心用戶端呼叫不安全的 gRPC 服務](#call-insecure-grpc-services-with-net-core-client)。

> [!WARNING]
> 沒有 TLS 的 HTTP/2 應僅在應用開發期間使用。 生產應用應始終使用傳輸安全性。 有關詳細資訊,請參閱[gRPC 中有關 ASP.NET 核心的安全注意事項](xref:grpc/security#transport-security)。

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>gRPC C++ 資產不是從 .proto 檔案產生的代碼

gRPC 代碼生成的具體用戶端和服務基類需要從專案中引用原檔和技術。 您必須包括:

* *.proto*在項目群組中使用. `<Protobuf>` [匯入*的 .proto*檔](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions)必須由專案引用。
* 封裝參考 gRPC 工具套件[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)。

有關生成 gRPC C++ 資產的<xref:grpc/basics>詳細資訊, 請參閱。

預設情況下,`<Protobuf>`引用生成具體的用戶端和服務基類。 引用元素的屬性`GrpcServices`可用於限制 C# 資產生成。 有效`GrpcServices`選項包括:

* `Both`( 不存在時預設 )
* `Server`
* `Client`
* `None`

託管 gRPC 服務的 ASP.NET 核心 Web 應用只需要產生的服務基類:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

進行 gRPC 呼叫 gRPC 的用戶端應用只需要產生的具體用戶端:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```

## <a name="wpf-projects-unable-to-generate-grpc-c-assets-from-proto-files"></a>WPF 專案無法從 .proto 檔案產生 gRPC C# 資產

WPF 專案有一[個已知問題](https://github.com/dotnet/wpf/issues/810),可阻止 gRPC 代碼生成正常工作。 用參考`Grpc.Tools`與 *.proto*檔案在 WPF 專案中產生的任何 gRPC 類型在使用時都會產生編譯錯誤:

> 錯誤 CS0246: 找不到類型或命名空間名稱「MyGrpc Services」(您是否缺少使用指令或程式集引用?

您可以通過:

1. 創建新的 .NET Core 類庫專案。
2. 在新項目中,加入參考以開啟[*\*從 .proto*檔案產生 C# 程式碼](xref:grpc/basics#generated-c-assets):
    * 添加對[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)套件的包引用。
    * 將*\*.proto*檔案`<Protobuf>`添加到專案組。
3. 在 WPF 應用程式中,添加對新專案的引用。

WPF 應用程式可以使用新類庫專案中生成的 gRPC 類型。

[!INCLUDE[](~/includes/gRPCazure.md)]
