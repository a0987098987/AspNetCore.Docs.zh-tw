---
title: 在 .NET 上的 gRPC 中記錄與診斷
author: jamesnk
description: 瞭解如何從 .NET 上的 gRPC 應用收集診斷。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 131144bf7a2c637eb2c1a1d5c54990dd4d429502
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417511"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>在 .NET 上的 gRPC 中記錄與診斷

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

本文提供有關從 gRPC 應用收集診斷以説明解決問題的指導。 涵蓋的主題包括：

* **記錄紀錄**- 寫入[.NET 核心紀錄記錄](xref:fundamentals/logging/index)的結構化日誌。 <xref:Microsoft.Extensions.Logging.ILogger>應用框架用於編寫日誌,以及使用者用於在應用中自己的登錄。
* **追蹤**-`DiaganosticSource`與`Activity`使用與 編寫的操作相關的事件。 從診斷源的追蹤通常用於收集應用程式遙測庫(如[應用程式見解](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)與[Open遙測](https://github.com/open-telemetry/opentelemetry-dotnet)) 。
* **指標**- 資料度量的表示時間間隔,例如每秒的請求。 指標是使用`EventCounter`點[網計數器](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters)命令列工具或使用[應用程式見解](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)發出的,並且可以觀察到。

## <a name="logging"></a>記錄

gRPC 服務與 gRPC 用戶端使用[.NET Core 紀錄紀錄寫入紀錄](xref:fundamentals/logging/index)。 當您需要在應用中調試意外行為時,日誌是一個很好的開始位置。

### <a name="grpc-services-logging"></a>gRPC 服務紀錄記錄

> [!WARNING]
> 伺服器端日誌可能包含來自應用的敏感資訊。 **切勿**將原始日誌從生產應用發佈到 GitHub 等公共論壇。

由於 gRPC 服務託管在 ASP.NET 酷中,因此它使用 ASP.NET 核心日誌記錄系統。 在預設配置中,gRPC 記錄的資訊很少,但可以配置。 有關配置ASP.NET核心日誌記錄的詳細資訊,請參閱[有關ASP.NET核心日誌記錄](xref:fundamentals/logging/index#configuration)的文檔。

gRPC`Grpc`在類別下添加日誌。 要啟用 gRPC 的詳細日誌,`Grpc`請將以下項`Debug``LogLevel`添加`Logging`到 中的子部分,將前綴配置為*appsettings.json*檔中的級別。

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

還可以在*Startup.cs*中配置此點, `ConfigureLogging`

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

如果不使用基於 JSON 的設定,請在設定系統中設定以下配置值:

* `Logging:LogLevel:Grpc` = `Debug`

檢查設定系統的文檔,以確定如何指定嵌套配置值。 例如,使用環境變數時,使用兩`_`個字元而不是`:`(例如`Logging__LogLevel__Grpc`。

我們建議在為應用收集`Debug`更詳細的診斷時使用該級別。 該`Trace`級別生成非常低級別的診斷,很少需要診斷應用中的問題。

#### <a name="sample-logging-output"></a>範例記錄輸出

下面是 gRPC`Debug`服務 等級的主控台輸出範例:

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

訪問伺服器端日誌的方式取決於正在運行的環境。

#### <a name="as-a-console-app"></a>作為主控台應用

如果在主控台應用中執行,預設情況下應啟用[主控台紀錄器](xref:fundamentals/logging/index#console-provider)。 gRPC 日誌將顯示在控制台中。

#### <a name="other-environments"></a>其他環境

如果應用部署到其他環境(例如 Docker、Kubernetes 或 Windows 服務),請參閱<xref:fundamentals/logging/index>有關如何配置適合該環境的日誌記錄提供程式的詳細資訊。

### <a name="grpc-client-logging"></a>gRPC 客戶端記錄記錄

> [!WARNING]
> 用戶端日誌可能包含來自應用的敏感資訊。 **切勿**將原始日誌從生產應用發佈到 GitHub 等公共論壇。

要從 .NET 客戶端取得`GrpcChannelOptions.LoggerFactory`紀錄, 可以在建立用戶端通道時設置該屬性。 如果從 ASP.NET 酷睿應用呼叫 gRPC 服務,則可以從依賴項注入 (DI) 解析記錄器工廠:

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

啟用用戶端日誌記錄的另一種方法是使用[gRPC 用戶端工廠](xref:grpc/clientfactory)創建用戶端。 在用戶端工廠註冊並從 DI 解析的 gRPC 用戶端將自動使用應用配置的日誌記錄。

如果你的應用不使用DI,那麼您可以使用`ILoggerFactory`[記錄器工廠](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)創建新實例。 要造訪此方法,請將[Microsoft.擴展.日誌記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)包添加到你的應用。

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a>gRPC 客戶端紀錄範圍

gRPC 客戶端到 gRPC 呼叫期間所做的紀錄的[紀錄紀錄的功能 。](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes) 範圍有與 gRPC 呼叫相關的中繼資料:

* **GrpcMethodtype** - gRPC 方法類型。 可能的值是`Grpc.Core.MethodType`枚舉中的名稱,例如未編號
* **Grpcuri** - gRPC 方法的相對 URI,例如 /greet。格雷特/賽赫洛

#### <a name="sample-logging-output"></a>範例記錄輸出

下面是 gRPC`Debug`客戶端 的主控台輸出範例:

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

gRPC 服務和 gRPC 用戶端使用[診斷源](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource)和[活動](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity)提供有關 gRPC 調用的資訊。

* .NET gRPC 使用活動來表示 gRPC 調用。
* 在 gRPC 調用活動的開始和停止處,跟蹤事件寫入診斷源。
* 跟蹤不會捕獲有關在 gRPC 流式處理調用的生存期內何時發送消息的資訊。

### <a name="grpc-service-tracing"></a>gRPC 服務追蹤

gRPC 服務託管在 ASP.NET 酷中,用於報告有關傳入 HTTP 請求的事件。 gRPC 特定的中繼資料將添加到 ASP.NET Core 提供的現有 HTTP 請求診斷中。

* 診斷來源名稱稱為`Microsoft.AspNetCore`。
* 活動名稱稱為`Microsoft.AspNetCore.Hosting.HttpRequestIn`。
  * gRPC 調用呼叫的 gRPC 方法的名稱將添加`grpc.method`為具有名稱的標記。
  * gRPC 調用的狀態代碼完成後,將添加為帶有`grpc.status_code`名稱 的標記。

### <a name="grpc-client-tracing"></a>gRPC 客戶端追蹤

.NET gRPC`HttpClient`用戶端 用於進行 gRPC 調用。 儘管`HttpClient`寫入診斷事件,但 .NET gRPC 用戶端提供自定義診斷源、活動和事件,以便收集有關 gRPC 調用的完整資訊。

* 診斷來源名稱稱為`Grpc.Net.Client`。
* 活動名稱稱為`Grpc.Net.Client.GrpcOut`。
  * gRPC 調用呼叫的 gRPC 方法的名稱將添加`grpc.method`為具有名稱的標記。
  * gRPC 調用的狀態代碼完成後,將添加為帶有`grpc.status_code`名稱 的標記。

### <a name="collecting-tracing"></a>收集追蹤

最簡單的使用`DiagnosticSource`方法是在應用中設定遙測庫,如[應用程式見解](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)或[Open 遙測](https://github.com/open-telemetry/opentelemetry-dotnet)。 該庫將處理有關 gRPC 調用沿其他應用遙測的資訊。

可以在應用程式見解等託管服務中查看跟蹤,也可以選擇運行自己的分散式跟蹤系統。 Open 遙測支援將追蹤資料匯出到[傑格](https://www.jaegertracing.io/)與[齊普金](https://zipkin.io/)。

`DiagnosticSource`可以使用使用的代碼中使用追蹤`DiagnosticListener`事件。 有關收聽包含代碼的診斷源的資訊,請參閱[診斷源使用者指南](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener)。

> [!NOTE]
> 遙測庫當前不捕獲 gRPC`Grpc.Net.Client.GrpcOut`特定 遙測。 改進捕獲此跟蹤的遙測庫的工作正在進行中。

## <a name="metrics"></a>計量

指標是數據度量在時間間隔(例如每秒請求)的表示形式。 指標數據允許在高級別上觀察應用的狀態。 .NET gRPC指標`EventCounter`使用 發出。

### <a name="grpc-service-metrics"></a>gRPC 服務指標

gRPC 伺服器指標報告在`Grpc.AspNetCore.Server`事件 源上。

| 名稱                      | 描述                   |
| --------------------------|-------------------------------|
| `total-calls`             | 呼叫總數                   |
| `current-calls`           | 目前通話                 |
| `calls-failed`            | 通話總數失敗            |
| `calls-deadline-exceeded` | 超過總呼叫截止日期 |
| `messages-sent`           | 傳送的郵件總數           |
| `messages-received`       | 已接收的訊息總數       |
| `calls-unimplemented`     | 未實用的總呼叫數     |

ASP.NET核心還提供其自己的`Microsoft.AspNetCore.Hosting`事件源指標。

### <a name="grpc-client-metrics"></a>gRPC 客戶端指標

gRPC 客戶端指標報告`Grpc.Net.Client`在事件 來源上。

| 名稱                      | 描述                   |
| --------------------------|-------------------------------|
| `total-calls`             | 呼叫總數                   |
| `current-calls`           | 目前通話                 |
| `calls-failed`            | 通話總數失敗            |
| `calls-deadline-exceeded` | 超過總呼叫截止日期 |
| `messages-sent`           | 傳送的郵件總數           |
| `messages-received`       | 已接收的訊息總數       |

### <a name="observe-metrics"></a>觀察指標

[點網計數器](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters)是一種用於臨時運行狀況監控和一級性能調查的性能監控工具。 使用或`Grpc.AspNetCore.Server``Grpc.Net.Client`作為提供程式名稱監視 .NET 應用。

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

觀察 gRPC 指標的另一種方法是使用應用程式見解的[Microsoft.Application Insights.eventCounterCollector 包](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)捕獲計數器數據。 設置後,應用程式見解在運行時收集常見的 .NET 計數器。 預設情況下不收集 gRPC 的計數器,但可以自訂應用見解[以包括其他計數器](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected)。

指定用於應用程式洞察的 gRPC 計數器,以便*以 Startup.cs*收集:

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
