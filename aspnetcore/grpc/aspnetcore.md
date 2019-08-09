---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 瞭解使用 ASP.NET Core 撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862932"
---
# <a name="grpc-services-with-aspnet-core"></a>搭配 ASP.NET Core 的 gRPC 服務

本檔說明如何使用 ASP.NET Core 開始使用 gRPC services。

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>開始在 ASP.NET Core 中使用 gRPC 服務

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

如需如何建立 gRPC 專案的詳細指示, 請參閱[開始使用 gRPC services](xref:tutorials/grpc/grpc-start) 。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

從命令列執行 `dotnet new grpc -o GrpcGreeter`。

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>將 gRPC 服務新增至 ASP.NET Core 應用程式

gRPC 需要[gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)套件。

### <a name="configure-grpc"></a>設定 gRPC

gRPC 是以`AddGrpc`方法啟用:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

每個 gRPC 服務都會透過`MapGrpcService`方法新增至路由管線:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core 中介軟體和功能共用路由管線, 因此可以將應用程式設定為提供額外的要求處理常式。 其他要求處理常式 (例如 MVC 控制器) 會與已設定的 gRPC 服務平行處理。

## <a name="integration-with-aspnet-core-apis"></a>與 ASP.NET Core Api 整合

gRPC 服務具有 ASP.NET Core 功能的完整存取權, 例如相依性[插入](xref:fundamentals/dependency-injection)(DI) 和[記錄](xref:fundamentals/logging/index)。 例如, 服務執行可以透過此函式從 DI 容器解析記錄器服務:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

根據預設, gRPC 服務執行可以使用任何存留期 (單一、限定範圍或暫時性) 來解析其他 DI 服務。

### <a name="resolve-httpcontext-in-grpc-methods"></a>解析 gRPC 方法中的 HttpCoNtext

GRPC API 可讓您存取某些 HTTP/2 訊息資料, 例如方法、主機、標頭和結尾。 存取是透過傳遞`ServerCallContext`至每個 gRPC 方法的引數:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`並未提供所有 ASP.NET api `HttpContext`的完整存取權。 擴充方法會提供完整的`HttpContext`存取權, 以代表 ASP.NET api 中的基礎 HTTP/2 訊息: `GetHttpContext`

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a>macOS 上的 gRPC 和 ASP.NET Core

Kestrel 不支援在 macOS 上使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)的 HTTP/2。 ASP.NET Core gRPC 範本和範例預設會使用 TLS。 當您嘗試啟動 gRPC 伺服器時, 您會看到下列錯誤訊息:

> 無法在 IPv4 回 https://localhost:5001 送介面上系結至:由於遺漏 ALPN 支援, macOS 上不支援 HTTP/2 over TLS。 '。

若要解決此問題, 請將 Kestrel 和 gRPC 用戶端設定為使用**沒有**TLS 的 HTTP/2。 您應該只在開發期間執行此動作。 不使用 TLS 會導致傳送未加密的 gRPC 訊息。

Kestrel 必須在中`Program.cs`設定沒有 TLS 的 HTTP/2 端點:

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

GRPC 用戶端必須將`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`參數設定為`true` , 並在伺服器位址中使用: `http`

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> 只有在應用程式開發期間, 才應該使用不含 TLS 的 HTTP/2。 生產應用程式應該一律使用傳輸安全性。 如需詳細資訊, 請參閱[gRPC for ASP.NET Core 中的安全性考慮](xref:grpc/security#transport-security)。

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
