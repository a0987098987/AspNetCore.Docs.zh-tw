---
title: .NET 上 gRPC 中的記錄和診斷
author: jamesnk
description: 瞭解如何從 .NET 上的 gRPC 應用程式收集診斷。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/diagnostics
ms.openlocfilehash: 33b2ee29830cd3012ff791c949c3a7c23a2e98c7
ms.sourcegitcommit: 16b3abec1ed70f9a206f0cfa7cf6404eebaf693d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2020
ms.locfileid: "83444343"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>.NET 上 gRPC 中的記錄和診斷

依[James 牛頓-王](https://twitter.com/jamesnk)

本文提供從 gRPC 應用程式收集診斷，以協助疑難排解問題的指引。 涵蓋的主題包括：

* **記錄**-寫入[.net Core 記錄](xref:fundamentals/logging/index)的結構化記錄。 <xref:Microsoft.Extensions.Logging.ILogger>應用程式架構會使用來寫入記錄，並由使用者在應用程式中進行自己的記錄。
* **追蹤**-與使用和撰寫之作業相關的事件 `DiaganosticSource` `Activity` 。 診斷來源的追蹤通常用來根據程式庫（例如[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)和[OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet)）收集應用程式遙測。
* **計量**-依時間間隔表示的資料量值，例如每秒要求數。 計量是使用發出 `EventCounter` ，而且可以使用[dotnet-計數器](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters)命令列工具或[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)來觀察。

## <a name="logging"></a>記錄

gRPC services 和 gRPC 用戶端會使用[.Net Core 記錄](xref:fundamentals/logging/index)來寫入記錄。 當您需要在應用程式中偵測到非預期的行為時，記錄是很好的起點。

### <a name="grpc-services-logging"></a>gRPC services 記錄

> [!WARNING]
> 伺服器端記錄可能包含來自您應用程式的機密資訊。 **絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。

因為 gRPC 服務是在 ASP.NET Core 上主控，所以它會使用 ASP.NET Core 記錄系統。 在預設設定中，gRPC 會記錄非常少的資訊，但這可加以設定。 如需設定 ASP.NET Core 記錄的詳細資訊，請參閱有關[ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration)的檔。

gRPC 會將記錄新增至 `Grpc` 類別之下。 若要啟用 gRPC 的詳細記錄，請將 `Grpc` `Debug` 下列專案新增至中的子區段，以在*appsettings*中設定層級的前置詞 `LogLevel` `Logging` ：

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

您也可以在*Startup.cs*中使用來設定此項 `ConfigureLogging` ：

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

如果您不是使用以 JSON 為基礎的設定，請在您的配置系統中設定下列設定值：

* `Logging:LogLevel:Grpc` = `Debug`

請查看設定系統的檔，以判斷如何指定嵌套的設定值。 例如，使用環境變數時， `_` 會使用兩個字元，而不是 `:` （例如 `Logging__LogLevel__Grpc` ）。

`Debug`針對您的應用程式收集更詳細的診斷資訊時，我們建議使用此層級。 `Trace`層級會產生非常低層級的診斷，而且很少需要診斷應用程式中的問題。

#### <a name="sample-logging-output"></a>範例記錄輸出

以下是 `Debug` gRPC 服務層級的主控台輸出範例：

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

### <a name="access-server-side-logs"></a>存取伺服器端記錄

您存取伺服器端記錄的方式，取決於您執行的環境。

#### <a name="as-a-console-app"></a>作為主控台應用程式

如果您是在主控台應用程式中執行，預設應該啟用[主控台記錄器](xref:fundamentals/logging/index#console)。 gRPC 記錄將會出現在主控台中。

#### <a name="other-environments"></a>其他環境

如果應用程式部署到另一個環境（例如 Docker、Kubernetes 或 Windows 服務），請參閱，以 <xref:fundamentals/logging/index> 取得有關如何設定適用于環境之記錄提供者的詳細資訊。

### <a name="grpc-client-logging"></a>gRPC 用戶端記錄

> [!WARNING]
> 用戶端記錄可能包含來自您應用程式的機密資訊。 **絕對不要**將未經處理的記錄從生產應用程式張貼到 GitHub 之類的公用論壇。

若要從 .NET 用戶端取得記錄，您可以在建立 `GrpcChannelOptions.LoggerFactory` 用戶端通道時設定屬性。 如果您是從 ASP.NET Core 應用程式呼叫 gRPC 服務，則可以從相依性插入（DI）解析記錄器 factory：

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

啟用用戶端記錄的另一種方式是使用[gRPC 用戶端 factory](xref:grpc/clientfactory)來建立用戶端。 向用戶端 factory 註冊並從 DI 解析的 gRPC 用戶端，會自動使用應用程式設定的記錄。

如果您的應用程式未使用 DI，則您可以使用 Server.loggerfactory 建立新的 `ILoggerFactory` 實例[LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)。 若要存取此方法，請在您的應用程式中新增[Microsoft Extensions. 記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)套件。

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a>gRPC 用戶端記錄範圍

GRPC 用戶端會將[記錄範圍](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes)新增至 gRPC 呼叫期間所進行的記錄。 範圍具有與 gRPC 呼叫相關的中繼資料：

* **GrpcMethodType** -gRPC 方法類型。 可能的值是來自 `Grpc.Core.MethodType` 列舉的名稱，例如一元
* **GrpcUri** -gRPC 方法的相對 URI，例如/greet。Greeter/SayHellos

#### <a name="sample-logging-output"></a>範例記錄輸出

以下是 `Debug` gRPC 用戶端層級的主控台輸出範例：

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

## <a name="tracing"></a>追蹤

gRPC services 和 gRPC 用戶端會使用[DiagnosticSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource)和[Activity](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity)提供 gRPC 呼叫的相關資訊。

* .NET gRPC 會使用活動來代表 gRPC 呼叫。
* 追蹤事件會在 gRPC 呼叫活動開始和停止時寫入診斷來源。
* 追蹤不會捕捉在 gRPC 串流呼叫的存留期間傳送訊息的相關資訊。

### <a name="grpc-service-tracing"></a>gRPC 服務追蹤

gRPC 服務裝載于 ASP.NET Core，其會報告傳入 HTTP 要求的相關事件。 gRPC 特定的中繼資料會新增至 ASP.NET Core 提供的現有 HTTP 要求診斷。

* 診斷來源名稱是 `Microsoft.AspNetCore` 。
* 活動名稱為 `Microsoft.AspNetCore.Hosting.HttpRequestIn` 。
  * GRPC 呼叫所叫用之 gRPC 方法的名稱會新增為名稱為的標記 `grpc.method` 。
  * GRPC 呼叫完成時的狀態碼會新增為名稱為的標記 `grpc.status_code` 。

### <a name="grpc-client-tracing"></a>gRPC 用戶端追蹤

.NET gRPC 用戶端會使用 `HttpClient` 來進行 gRPC 呼叫。 雖然會 `HttpClient` 寫入診斷事件，但 .Net gRPC 用戶端會提供自訂診斷來源、活動和事件，以便收集有關 gRPC 呼叫的完整資訊。

* 診斷來源名稱是 `Grpc.Net.Client` 。
* 活動名稱為 `Grpc.Net.Client.GrpcOut` 。
  * GRPC 呼叫所叫用之 gRPC 方法的名稱會新增為名稱為的標記 `grpc.method` 。
  * GRPC 呼叫完成時的狀態碼會新增為名稱為的標記 `grpc.status_code` 。

### <a name="collecting-tracing"></a>收集追蹤

最簡單的使用方式 `DiagnosticSource` 是在您的應用程式中設定遙測程式庫（例如[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)或[OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet) ）。 程式庫會處理有關 gRPC 呼叫的資訊，以及其他應用程式遙測。

追蹤可以在 Application Insights 之類的受控服務中查看，或者您也可以選擇執行自己的分散式追蹤系統。 OpenTelemetry 支援將追蹤資料匯出至[Jaeger](https://www.jaegertracing.io/)和[Zipkin](https://zipkin.io/)。

`DiagnosticSource`可以使用程式碼中的追蹤事件 `DiagnosticListener` 。 如需以程式碼接聽診斷來源的相關資訊，請參閱[DiagnosticSource 使用者指南](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener)。

> [!NOTE]
> 遙測程式庫目前不會捕捉 gRPC 特有 `Grpc.Net.Client.GrpcOut` 的遙測。 改善遙測程式庫的工作正在進行追蹤。

## <a name="metrics"></a>計量

計量是以時間間隔表示的資料量值，例如每秒要求數。 計量資料可讓您觀察高階應用程式的狀態。 .NET gRPC 計量是使用發出的 `EventCounter` 。

### <a name="grpc-service-metrics"></a>gRPC 服務計量

事件來源上會回報 gRPC 伺服器計量 `Grpc.AspNetCore.Server` 。

| Name                      | 說明                   |
| --------------------------|-------------------------------|
| `total-calls`             | 呼叫總數                   |
| `current-calls`           | 目前的呼叫                 |
| `calls-failed`            | 失敗的總呼叫數            |
| `calls-deadline-exceeded` | 超過呼叫期限總計 |
| `messages-sent`           | 傳送的郵件總數           |
| `messages-received`       | 已接收的訊息總數       |
| `calls-unimplemented`     | 未實現的總呼叫數     |

ASP.NET Core 也會在事件來源上提供自己的計量 `Microsoft.AspNetCore.Hosting` 。

### <a name="grpc-client-metrics"></a>gRPC 用戶端計量

事件來源上會回報 gRPC 用戶端計量 `Grpc.Net.Client` 。

| Name                      | 說明                   |
| --------------------------|-------------------------------|
| `total-calls`             | 呼叫總數                   |
| `current-calls`           | 目前的呼叫                 |
| `calls-failed`            | 失敗的總呼叫數            |
| `calls-deadline-exceeded` | 超過呼叫期限總計 |
| `messages-sent`           | 傳送的郵件總數           |
| `messages-received`       | 已接收的訊息總數       |

### <a name="observe-metrics"></a>觀察計量

[dotnet-計數器](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters)是一種效能監視工具，可用於臨機操作健全狀況監視和第一層效能調查。 使用 `Grpc.AspNetCore.Server` 或 `Grpc.Net.Client` 做為提供者名稱，監視 .net 應用程式。

```console
> dotnet-counters monitor --process-id 1902 Grpc.AspNetCore.Server

Press p to pause, r to resume, q to quit.
    Status: Running
[Grpc.AspNetCore.Server]
    Total Calls                                 300
    Current Calls                               5
    Total Calls Failed                          0
    Total Calls Deadline Exceeded               0
    Total Messages Sent                         295
    Total Messages Received                     300
    Total Calls Unimplemented                   0
```

另一個觀察 gRPC 度量的方式，是使用 Application Insights 的[EventCounterCollector 套件](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)來捕捉計數器資料。 一旦設定之後，Application Insights 會在執行時間收集一般的 .NET 計數器。 預設不會收集 gRPC 的計數器，但可以自訂 App Insights[以包含額外的計數器](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected)。

針對要在*Startup.cs*中收集的應用程式深入解析指定 gRPC 計數器：

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // Configure App Insights to collect gRPC counters gRPC services hosted in an ASP.NET Core app
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "current-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "total-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "calls-failed"));
            }
        );
    }
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
