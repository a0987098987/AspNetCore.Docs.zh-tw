---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 瞭解使用 ASP.NET Core 撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/28/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 128f5b36eac9112460c33693db5537134a077476
ms.sourcegitcommit: 23f79bd71d49c4efddb56377c1f553cc993d781b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2019
ms.locfileid: "70130709"
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

在 *Startup.cs* 中：

* gRPC 是以`AddGrpc`方法啟用。
* 每個 gRPC 服務都會透過`MapGrpcService`方法加入至路由管線。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

ASP.NET Core 中介軟體和功能共用路由管線, 因此可以將應用程式設定為提供額外的要求處理常式。 其他要求處理常式 (例如 MVC 控制器) 會與已設定的 gRPC 服務平行處理。

### <a name="configure-kestrel"></a>設定 Kestrel

Kestrel gRPC 端點:

* 需要 HTTP/2。
* 應使用 HTTPS 來保護。

#### <a name="http2"></a>HTTP/2

Kestrel 支援大多數新式作業系統上的[HTTP/2](xref:fundamentals/servers/kestrel#http2-support) 。 根據預設, Kestrel 端點會設定為支援 HTTP/1.1 和 HTTP/2 連接。

> [!NOTE]
> macOS 不支援具有[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)的 ASP.NET Core gRPC。 您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。 如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。

#### <a name="https"></a>HTTPS

用於 gRPC 的 Kestrel 端點應使用 HTTPS 來保護。 在開發期間, 會在 ASP.NET Core 開發憑證存在`https://localhost:5001`時, 自動建立 HTTPS 端點。 不需要進行任何設定。

在生產環境中，必須明確設定 HTTPS。 在下列*appsettings*範例中, 提供了使用 HTTPS 保護的 HTTP/2 端點:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

或者, 您也可以在*Program.cs*中設定 Kestrel endspoints:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

如需使用 Kestrel 啟用 HTTP/2 和 HTTPS 的詳細資訊, 請參閱[Kestrel 端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定。

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

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
