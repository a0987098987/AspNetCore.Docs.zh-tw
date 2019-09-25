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
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>.NET 上 gRPC 中的記錄和診斷

依[James 牛頓-王](https://twitter.com/jamesnk)

本文提供從您的 gRPC 應用程式收集診斷資訊，以協助疑難排解問題的指引。

## <a name="grpc-services-logging"></a>gRPC services 記錄

> [!WARNING]
> 伺服器端記錄可能包含來自您應用程式的機密資訊。 **絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。

因為 gRPC 服務是在 ASP.NET Core 上主控，所以它會使用 ASP.NET Core 記錄系統。 在預設設定中，gRPC 會記錄非常少的資訊，但這可加以設定。 如需設定 ASP.NET Core 記錄的詳細資訊，請參閱有關[ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration)的檔。

gRPC 會將記錄新增`Grpc`至類別之下。 若要啟用 gRPC 的詳細記錄，請`Grpc`將下列專案`Debug`新增至中 `LogLevel` `Logging`的子區段，以在 appsettings 中設定層級的前置詞：

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

您也可以在*Startup.cs*中使用`ConfigureLogging`來設定此項：

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

如果您不是使用以 JSON 為基礎的設定，請在您的配置系統中設定下列設定值：

* `Logging:LogLevel:Grpc` = `Debug`

請查看設定系統的檔，以判斷如何指定嵌套的設定值。 例如，使用環境變數時，會使用`_`兩個字元，而不`:`是（ `Logging__LogLevel__Grpc`例如）。

針對您的應用`Debug`程式收集更詳細的診斷資訊時，我們建議使用此層級。 `Trace`層級會產生非常低層級的診斷，而且很少需要診斷應用程式中的問題。

### <a name="sample-logging-output"></a>範例記錄輸出

以下是 gRPC 服務`Debug`層級的主控台輸出範例：

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

## <a name="access-server-side-logs"></a>存取伺服器端記錄

您存取伺服器端記錄的方式，取決於您執行的環境。

### <a name="as-a-console-app"></a>作為主控台應用程式

如果您是在主控台應用程式中執行，預設應該啟用[主控台記錄器](xref:fundamentals/logging/index#console-provider)。 gRPC 記錄將會出現在主控台中。

### <a name="other-environments"></a>其他環境

如果應用程式部署到另一個環境（例如 Docker、Kubernetes 或 Windows 服務），請參閱<xref:fundamentals/logging/index> ，以取得有關如何設定適用于環境之記錄提供者的詳細資訊。

## <a name="grpc-client-logging"></a>gRPC 用戶端記錄

> [!WARNING]
> 用戶端記錄可能包含來自您應用程式的機密資訊。 **絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。

若要從 .net 用戶端取得記錄，您可以在`GrpcChannelOptions.LoggerFactory`建立用戶端通道時設定屬性。 如果您是從 ASP.NET Core 應用程式呼叫 gRPC 服務，則可以從相依性插入（DI）解析記錄器 factory：

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

啟用用戶端記錄的另一種方式是使用[gRPC 用戶端 factory](xref:grpc/clientfactory)來建立用戶端。 向用戶端 factory 註冊並從 DI 解析的 gRPC 用戶端，會自動使用應用程式設定的記錄。

如果您的應用程式未使用 DI，則您可以`ILoggerFactory`使用[server.loggerfactory](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)建立新的實例。 若要存取此方法，請在您的應用程式中新增[Microsoft Extensions. 記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)套件。

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>範例記錄輸出

以下是 gRPC 用戶端`Debug`層級的主控台輸出範例：

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

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
