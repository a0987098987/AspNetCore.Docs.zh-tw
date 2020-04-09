---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 使用 ASP.NET 核心編寫 gRPC 服務時,瞭解基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 6107704a4b4d9c629a7abe907efd5b1932019130
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667626"
---
# <a name="grpc-services-with-aspnet-core"></a>搭配 ASP.NET Core 的 gRPC 服務

本文檔展示如何使用ASP.NET酷睿開始使用 gRPC 服務。

## <a name="prerequisites"></a>Prerequisites

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

有關如何創建 gRPC 專案的詳細說明,請參閱[gRPC 服務入門](xref:tutorials/grpc/grpc-start)。

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

從命令列執行 `dotnet new grpc -o GrpcGreeter`。

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>將 gRPC 服務新增到 ASP.NET 核心應用

gRPC 需要[Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore)包。

### <a name="configure-grpc"></a>設定 gRPC

在 *Startup.cs* 中：

* 使用`AddGrpc`方法啟用 gRPC。
* 通過`MapGrpcService`該方法將每個 gRPC 服務添加到路由管道中。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

ASP.NET核心中間件和功能共用路由管道,因此可以配置應用以提供其他請求處理程式。 其他請求處理程式(如MVC控制器)與配置的gRPC服務並行工作。

### <a name="configure-kestrel"></a>設定凱斯特雷爾

凱斯特雷爾 gRPC 終結點:

* 需要 HTTP/2。
* 應使用[傳輸層安全 (TLS)](https://tools.ietf.org/html/rfc5246)進行保護。

#### <a name="http2"></a>HTTP/2

gRPC 需要 HTTP/2。 gRPCASP.NET核心驗證[HTTPRequest.協定](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)是`HTTP/2`。

Kestrel 在大多數現代作業系統上[支持 HTTP/2。](xref:fundamentals/servers/kestrel#http2-support) 預設情況下,Kestrel 終結點配置為支援 HTTP/1.1 和 HTTP/2 連接。

#### <a name="tls"></a>TLS

用於 gRPC 的 Kestrel 端點應用 TLS 進行保護。 在開發中,當存在ASP.NET核心開發證書`https://localhost:5001`時,將自動創建使用TLS保護的終結點。 不需要組態。 前置`https`授權 Kestrel 終結點正在使用 TLS。

在生產中,必須顯式配置 TLS。 在下面的*應用設定.json*範例中,提供了一個使用 TLS 保護的 HTTP/2 終結點:

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

或者,Kestrel 終結點可以在*Program.cs*中配置:

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a>通訊協定交涉

TLS 僅用於保護通信。 當終結點支援多個協定時,TLS[應用程式層協議協商 (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)握手用於協商客戶端和伺服器之間的連接協定。 此協商確定連接是使用 HTTP/1.1 還是 HTTP/2。

如果 HTTP/2 終結點設定時沒有 TLS,則終結點的[ListenOptions.協定](xref:fundamentals/servers/kestrel#listenoptionsprotocols)必須設定為`HttpProtocols.Http2`。 沒有 TLS,不能使用具有多個協議`HttpProtocols.Http1AndHttp2`的終結點(例如,)。如果不存在協商。 與不安全的終結點的所有連接預設為 HTTP/1.1,gRPC 調用失敗。

有關使用 Kestrel 啟用 HTTP/2 與 TLS 的詳細資訊,請參閱[Kestrel 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)。

> [!NOTE]
> macOS 不支援具有 TLS 的 ASP.NET Core gRPC。 您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。 如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。

## <a name="integration-with-aspnet-core-apis"></a>與ASP.NET核心 API 整合

gRPC 服務具有對ASP.NET核心功能(如[依賴項注入](xref:fundamentals/dependency-injection)(DI) 和[日誌記錄](xref:fundamentals/logging/index))的完全訪問許可權。 例如,服務實現可以透過構造函數從 DI 容器解析記錄器服務:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

默認情況下,gRPC 服務實現可以解決具有任何存留期的其他 DI 服務(單例、作用域或瞬態)。

### <a name="resolve-httpcontext-in-grpc-methods"></a>在 gRPC 方法解析 HTTP:/ 1: HTTPContext

gRPC API 提供對某些 HTTP/2 消息數據(如方法、主機、標頭和預告片)的訪問。 存取是透過以提供`ServerCallContext`每個 gRPC 方法的參數:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`不提供對所有`HttpContext`ASP.NET API 的完全訪問許可權。 擴`GetHttpContext`充方法提供對 ASP.NET`HttpContext`API 中表示基礎 HTTP/2 消息的完全存取權限:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
