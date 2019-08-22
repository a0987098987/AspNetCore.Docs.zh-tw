---
title: 針對 .NET Core 上的 gRPC 進行疑難排解
author: jamesnk
description: 針對在 .NET Core 上使用 gRPC 時的錯誤進行疑難排解。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/17/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 7621266dfe26b7126d1607e195dd5dcaab4efa55
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886484"
---
# <a name="troubleshoot-grpc-on-net-core"></a>針對 .NET Core 上的 gRPC 進行疑難排解

依[James 牛頓-王](https://twitter.com/jamesnk)

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>用戶端與服務 SSL/TLS 設定不相符

GRPC 範本和範例預設會使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)來保護 gRPC 服務。 gRPC 用戶端必須使用安全連線, 才能順利呼叫受保護的 gRPC 服務。

您可以確認 ASP.NET Core gRPC 服務在應用程式啟動時所寫入的記錄中使用 TLS。 服務將接聽 HTTPS 端點:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

.Net Core 用戶端必須在`https`伺服器位址中使用, 以透過安全的連線進行呼叫:

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

所有 gRPC 用戶端都支援 TLS。 從其他語言 gRPC 用戶端時, 通常需要使用`SslCredentials`設定的通道。 `SslCredentials`指定用戶端將使用的憑證, 而且必須使用它來取代不安全的認證。 如需將不同的 gRPC 用戶端執行設定為使用 TLS 的範例, 請參閱[GRPC Authentication](https://www.grpc.io/docs/guides/auth/)。

## <a name="call-insecure-grpc-services-with-net-core-client"></a>使用 .NET Core 用戶端呼叫不安全的 gRPC 服務

需要其他設定, 才能使用 .NET Core 用戶端呼叫不安全的 gRPC 服務。 GRPC 用戶端必須將`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`參數設定為`true` , 並在伺服器位址中使用: `http`

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式

Kestrel 不支援 macOS 上的 TLS 和舊版 Windows (例如 Windows 7) 上的 HTTP/2。 ASP.NET Core gRPC 範本和範例預設會使用 TLS。 當您嘗試啟動 gRPC 伺服器時, 您會看到下列錯誤訊息:

> 無法在 IPv4 回 https://localhost:5001 送介面上系結至:由於遺漏 ALPN 支援, macOS 上不支援 HTTP/2 over TLS。 '。

若要解決此問題, 請將 Kestrel 和 gRPC 用戶端設定為使用*沒有*TLS 的 HTTP/2。 您應該只在開發期間執行此動作。 不使用 TLS 會導致傳送未加密的 gRPC 訊息。

Kestrel 必須在*Program.cs*中設定沒有 TLS 的 HTTP/2 端點:

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

設定 HTTP/2 端點而不使用 TLS 時, 端點的[listenoptions 來](xref:fundamentals/servers/kestrel#listenoptionsprotocols)必須設定為`HttpProtocols.Http2`。 `HttpProtocols.Http1AndHttp2`無法使用, 因為需要 TLS 才能協調 HTTP/2。 如果沒有 TLS, 端點的所有連接都會預設為 HTTP/1.1, 而 gRPC 呼叫會失敗。

GRPC 用戶端也必須設定為不使用 TLS。 如需詳細資訊, 請參閱[使用 .Net Core 用戶端呼叫不安全的 gRPC 服務](#call-insecure-grpc-services-with-net-core-client)。

> [!WARNING]
> 只有在應用程式開發期間, 才應該使用不含 TLS 的 HTTP/2。 生產應用程式應該一律使用傳輸安全性。 如需詳細資訊, 請參閱[gRPC for ASP.NET Core 中的安全性考慮](xref:grpc/security#transport-security)。

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>gRPC C#資產不是從 *\*proto*檔案產生的程式碼

gRPC 程式碼產生具體的用戶端和服務基類需要從專案參考 protobuf 檔案和工具。 您必須包括:

* 您想要在`<Protobuf>`專案群組中使用的 *. proto*檔案。 專案必須參考已匯[入的*proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions)檔案。
* GRPC 工具套件[gRPC](https://www.nuget.org/packages/Grpc.Tools/)的套件參考。

如需產生 gRPC C#資產的詳細資訊, <xref:grpc/basics>請參閱。

根據預設, `<Protobuf>`參考會產生具體的用戶端和服務基類。 參考元素的`GrpcServices`屬性可用來限制C#資產產生。 有效`GrpcServices`的選項為:

* `Both`(不存在時的預設值)
* `Server`
* `Client`
* `None`

裝載 gRPC 服務的 ASP.NET Core web 應用程式只需要產生服務基類:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

建立 gRPC 呼叫的 gRPC 用戶端應用程式只需要產生的具體用戶端:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
