---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 瞭解使用 ASP.NET Core 撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/aspnetcore
ms.openlocfilehash: 0d05a6dcaf6677e71181d522a9f501051ec34f9d
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407547"
---
# <a name="grpc-services-with-aspnet-core"></a>搭配 ASP.NET Core 的 gRPC 服務

本檔說明如何使用 ASP.NET Core 開始使用 gRPC services。

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>開始在 ASP.NET Core 中使用 gRPC 服務

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

如需如何建立 gRPC 專案的詳細指示，請參閱[開始使用 gRPC services](xref:tutorials/grpc/grpc-start) 。

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

從命令列執行 `dotnet new grpc -o GrpcGreeter`。

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>將 gRPC 服務新增至 ASP.NET Core 應用程式

gRPC 需要[gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)套件。

### <a name="configure-grpc"></a>設定 gRPC

在 *Startup.cs* 中：

* gRPC 是以方法啟用 `AddGrpc` 。
* 每個 gRPC 服務都會透過方法加入至路由管線 `MapGrpcService` 。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

ASP.NET Core 中介軟體和功能共用路由管線，因此可以將應用程式設定為提供額外的要求處理常式。 其他要求處理常式（例如 MVC 控制器）會與已設定的 gRPC 服務平行處理。

### <a name="configure-kestrel"></a>設定 Kestrel

Kestrel gRPC 端點：

* 需要 HTTP/2。
* 應使用[傳輸層安全性（TLS）](https://tools.ietf.org/html/rfc5246)來保護。

#### <a name="http2"></a>HTTP/2

gRPC 需要 HTTP/2。 ASP.NET Core 的 gRPC 會驗證[HttpRequest。通訊協定](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)為 `HTTP/2` 。

Kestrel 支援大多數新式作業系統上的[HTTP/2](xref:fundamentals/servers/kestrel#http2-support) 。 根據預設，Kestrel 端點會設定為支援 HTTP/1.1 和 HTTP/2 連接。

#### <a name="tls"></a>TLS

用於 gRPC 的 Kestrel 端點應使用 TLS 來保護。 在開發期間，會在 `https://localhost:5001` ASP.NET Core 開發憑證存在時，自動建立以 TLS 保護的端點。 不需要組態。 `https`前置詞會驗證 Kestrel 端點是否使用 TLS。

在生產環境中，必須明確設定 TLS。 在下列*appsettings.js*範例中，會提供使用 TLS 保護的 HTTP/2 端點：

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

或者，您也可以在*Program.cs*中設定 Kestrel 端點：

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a>通訊協定交涉

TLS 用於保護通訊安全。 當端點支援多個通訊協定時，會使用 TLS[應用層通訊協定協商（ALPN）](https://tools.ietf.org/html/rfc7301#section-3)交握來協調用戶端與伺服器之間的連接通訊協定。 此協商會判斷連接使用的是 HTTP/1.1 或 HTTP/2。

如果在沒有 TLS 的情況下設定 HTTP/2 端點，則端點的[listenoptions 來](xref:fundamentals/servers/kestrel#listenoptionsprotocols)必須設定為 `HttpProtocols.Http2` 。 具有多個通訊協定（例如）的端點不能 `HttpProtocols.Http1AndHttp2` 在沒有 TLS 的情況下使用，因為沒有任何協商。 不安全端點的所有連接預設為 HTTP/1.1，而 gRPC 呼叫會失敗。

如需使用 Kestrel 啟用 HTTP/2 和 TLS 的詳細資訊，請參閱[Kestrel 端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定。

> [!NOTE]
> macOS 不支援具有 TLS 的 ASP.NET Core gRPC。 您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。 如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。

## <a name="integration-with-aspnet-core-apis"></a>與 ASP.NET Core Api 整合

gRPC 服務具有 ASP.NET Core 功能的完整存取權，例如相依性[插入](xref:fundamentals/dependency-injection)（DI）和[記錄](xref:fundamentals/logging/index)。 例如，服務執行可以透過此函式從 DI 容器解析記錄器服務：

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

根據預設，gRPC 服務執行可以使用任何存留期（單一、限定範圍或暫時性）來解析其他 DI 服務。

### <a name="resolve-httpcontext-in-grpc-methods"></a>解析 gRPC 方法中的 HttpCoNtext

GRPC API 可讓您存取某些 HTTP/2 訊息資料，例如方法、主機、標頭和結尾。 存取是透過 `ServerCallContext` 傳遞至每個 gRPC 方法的引數：

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`並未提供 `HttpContext` 所有 ASP.NET api 的完整存取權。 `GetHttpContext`擴充方法會提供完整的存取權，以 `HttpContext` 代表 ASP.NET api 中的基礎 HTTP/2 訊息：

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]


## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
