---
title: 搭配 ASP.NET Core 的 gRPC 服務
author: juntaoluo
description: 使用 ASP.NET Core 中寫入 gRPC 服務時，請了解基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: c99a499fad824c3ac026f6f390c826c0418fc069
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59515398"
---
# <a name="grpc-services-with-aspnet-core"></a>搭配 ASP.NET Core 的 gRPC 服務

本文件說明如何開始使用 ASP.NET Core 的 gRPC 服務使用。

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>開始在 ASP.NET Core 中使用 gRPC 服務

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

請參閱[開始使用 gRPC 服務](xref:tutorials/grpc/grpc-start)如需有關如何建立 gRPC 專案的詳細指示。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

從命令列執行 `dotnet new grpc -o GrpcGreeter`。

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>將 gRPC 服務新增至 ASP.NET Core 應用程式

gRPC 需要下列封裝：

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) protobuf 訊息 Api。
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>設定 gRPC

使用啟用 gRPC`AddGrpc`方法：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

每個 gRPC 服務新增至路由的管線透過`MapGrpcService`方法：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

ASP.NET Core 中介軟體和功能共用路由的管線，因此可以設定應用程式提供額外的要求處理常式。 與設定的 gRPC 服務的同時處理其他要求處理常式，例如 MVC 控制器。

## <a name="integration-with-aspnet-core-apis"></a>與 ASP.NET Core Api 整合

gRPC 服務具有完整存取 ASP.NET Core 功能這類[相依性插入](xref:fundamentals/dependency-injection)(DI) 和[記錄](xref:fundamentals/logging/index)。 例如，服務實作可以解決透過建構函式的 DI 容器的記錄器服務：

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

根據預設，gRPC 服務實作可以解析其他 DI 服務具有任何存留期 （單一、 範圍或暫時性）。

### <a name="resolve-httpcontext-in-grpc-methods"></a>解決 HttpContext 中 gRPC 方法

GRPC API 提供存取某些 HTTP/2 訊息資料，例如方法、 主機、 標頭和結尾。 存取是透過`ServerCallContext`引數傳遞至每個 gRPC 方法：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` 不會提供完整存取權`HttpContext`中所有的 ASP.NET Api。 `GetHttpContext`擴充方法提供的完整存取權`HttpContext`表示 ASP.NET Api 中的基礎 HTTP/2 訊息：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

### <a name="request-body-data-rate-limit"></a>要求主體資料速率限制

根據預設，Kestrel 伺服器加諸[要求主體資料速率下限](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>)。 用戶端串流處理和串流處理呼叫的雙工，可能不符合此速率，並連接可能會逾時。最小要求內文資料流的用戶端和資料流處理呼叫的雙工，包括 gRPC 服務時，必須停用資料速率限制：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
