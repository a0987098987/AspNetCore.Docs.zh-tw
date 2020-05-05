---
title: .NET Core 與 ASP.NET Core 中的記錄
author: rick-anderson
description: 了解如何使用由 Microsoft.Extensions.Logging NuGet 套件提供的記錄架構。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/23/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/logging/index
ms.openlocfilehash: ca62e374c6031ca3c2d438df87f2d13636d9c612
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776099"
---
# <a name="logging-in-net-core-and-aspnet-core"></a>.NET Core 與 ASP.NET Core 中的記錄

::: moniker range=">= aspnetcore-3.0"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)

.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。 此文章說明如何搭配內建提供者使用 API。

本文中顯示的大部分程式碼範例都來自 ASP.NET Core 應用程式。 這些程式碼片段的記錄特定部分適用于任何使用[泛型主機](xref:fundamentals/host/generic-host)的 .net Core 應用程式。 如需如何在非 web 主控台應用程式中使用泛型主機的範例，請參閱[背景工作範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples)的<xref:fundamentals/host/hosted-services> *Program.cs*檔案（）。

不含一般主機的應用程式記錄程式碼，會因[新增提供者](#add-providers)和[建立記錄器](#create-logs)的方式而有所不同。 非主機程式碼範例顯示於本文的這些章節中。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="add-providers"></a>新增提供者

記錄提供者會顯示或儲存記錄。 例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。 您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。

若要在使用一般主機的應用程式中新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

在非主機主控台應用程式中，於建立 `LoggerFactory` 時呼叫提供者的 `Add{provider name}` 擴充方法：

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

`LoggerFactory` 和 `AddConsole` 需要 `Microsoft.Extensions.Logging` 的 `using` 陳述式。

預設 ASP.NET Core 專案範本會呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> 新增下列記錄提供者：

* [主控台](#console-provider)
* [偵錯](#debug-provider)
* [EventSource](#event-source-provider)
* [EventLog](#windows-eventlog-provider) （僅適用于在 Windows 上執行時）

您可以使用您想要的提供者來取代預設提供者。 呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。

## <a name="create-logs"></a>建立記錄

若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。 在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。 在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。

下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。 記錄「類別」** 是與每個記錄關聯的字串。 由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。 

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

下列非主機主控台應用程式範例會建立以 `LoggingConsoleApp.Program` 作為類別的記錄器。

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。 記錄「層級」** 指出已記錄事件的嚴重性。

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。

### <a name="create-logs-in-the-program-class"></a>在 Program 類別中建立記錄

若要在 ASP.NET Core 應用程式的 `Program` 類別中寫入記錄，請在建立主機之後從 DI 取得 `ILogger` 執行個體：

[!code-csharp[](index/samples_snapshot/3.x/TodoApiSample/Program.cs?highlight=9,10)]

不直接支援在主機結構期間進行記錄。 不過，您可以使用個別的記錄器。 在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateHostBuilder`。  `AddSerilog`會使用中`Log.Logger`指定的靜態設定：

```csharp
using System;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return Host.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddRazorPages();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {   
                    logging.AddSerilog();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="create-logs-in-the-startup-class"></a>在 Startup 類別中建立記錄

若要在 ASP.NET Core 應用程式的 `Startup.Configure` 方法中寫入記錄，請在方法簽章中包含 `ILogger` 參數：

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

在以 `Startup.ConfigureServices` 方法完成 DI 容器設定之前，不支援寫入記錄檔：

* 不支援將記錄器插入 `Startup` 建構函式。
* 不支援將記錄器插入 `Startup.ConfigureServices` 方法簽章

這項限制的原因是記錄相依於 DI 和組態，而後者相依於 DI。 在 `ConfigureServices` 完成後才會設定 DI 容器。

記錄器的建構函式插入 `Startup` 適用於舊版 ASP.NET Core，因為會為 Web 主機建立個別的 DI 容器。 如需為何只為一般主機建立一個容器的資訊，請參閱[重大變更公告](https://github.com/aspnet/Announcements/issues/353)。

如果您需要設定相依於 `ILogger<T>` 的服務，您仍然可以使用建構函式插入或提供 Factory 方法來執行此動作。 只有在沒有其他選項時，才建議使用 Factory 方法。 例如，假設您需要使用 DI 的服務來填入屬性：

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

前面的醒目提示程式碼是 `Func`，會在 DI 容器第一次需要建立 `MyService` 的執行個體時執行。 您可以用此方式存取任何已註冊的服務。

### <a name="create-logs-in-blazor"></a>在 Blazor 中建立記錄

#### <a name="blazor-webassembly"></a>Blazor WebAssembly

使用中的`WebAssemblyHostBuilder.Logging`屬性在 Blazor WebAssembly 應用程式中`Program.Main`設定記錄功能：

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

...

var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.Logging.SetMinimumLevel(LogLevel.Debug);
builder.Logging.AddProvider(new CustomLoggingProvider());
```

`Logging`屬性的類型<xref:Microsoft.Extensions.Logging.ILoggingBuilder>為，因此中提供<xref:Microsoft.Extensions.Logging.ILoggingBuilder>的所有擴充方法也可在上`Logging`取得。

#### <a name="log-in-razor-components"></a>Razor 元件中的登入

記錄器尊重應用程式啟動設定。

需要的`using`指示詞，才能支援 Api 的 Intellisense 完成（例如<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A>和<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A>）。 <xref:Microsoft.Extensions.Logging>

下列範例示範如何使用 Razor 元件<xref:Microsoft.Extensions.Logging.ILogger>中的進行記錄：

```razor
@page "/counter"
@using Microsoft.Extensions.Logging;
@inject ILogger<Counter> logger;

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        logger.LogWarning("Someone has clicked me!");

        currentCount++;
    }
}
```

下列範例示範如何使用 Razor 元件<xref:Microsoft.Extensions.Logging.ILoggerFactory>中的進行記錄：

```razor
@page "/counter"
@using Microsoft.Extensions.Logging;
@inject ILoggerFactory LoggerFactory

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        var logger = LoggerFactory.CreateLogger<Counter>();
        logger.LogWarning("Someone has clicked me!");

        currentCount++;
    }
}
```

### <a name="no-asynchronous-logger-methods"></a>無非同步記錄器方法

記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。 若您的記錄資料存放區很慢，請不要直接寫入其中。 請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。 例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。 相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。 如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。

## <a name="configuration"></a>設定

記錄提供者設定是由一或多個記錄提供者提供：

* 檔案格式 (INI、JSON 及 XML)。
* 命令列引數。
* 環境變數。
* 記憶體內部 .NET 物件。
* 未加密的[祕密管理員](xref:security/app-secrets)儲存體。
* 類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。
* 自訂提供者 (已安裝或已建立)。

例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。 下列範例顯示一般 *appsettings.Development.json* 檔案的內容：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。

`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。 在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。

`Logging` 下的其他屬性可指定記錄提供者。 範例使用主控台提供者。 如果提供者支援[記錄範圍](#log-scopes)， `IncludeScopes`則指出是否已啟用。 提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。 提供者下的 `LogLevel` 會指定提供者的記錄層級。

若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。

記錄 API 不包含在應用程式執行時變更記錄層級的案例。 不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。 例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)（由`CreateDefaultBuilder`新增）以讀取設定檔案，預設會重載記錄設定。 如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。

如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。

## <a name="sample-logging-output"></a>範例記錄輸出

使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。 主控台輸出範例如下：

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 84.26180000000001ms 307
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/api/todo/0
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
```

上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。

以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：

```console
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request starting HTTP/2.0 GET https://localhost:44328/api/todo/0  
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executing endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
TodoApiSample.Controllers.TodoController: Information: Getting item 0
TodoApiSample.Controllers.TodoController: Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult: Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker: Information: Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 34.167ms
Microsoft.AspNetCore.Routing.EndpointMiddleware: Information: Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
Microsoft.AspNetCore.Hosting.Diagnostics: Information: Request finished in 98.41300000000001ms 404
```

這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApiSample" 為開頭。 開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。 ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。

本文的其餘部分將說明記錄的一些詳細資料和選項。

## <a name="nuget-packages"></a>NuGet 套件

`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。

## <a name="log-category"></a>記錄分類

建立 `ILogger` 物件時，會為它指定「類別」**。 該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。 類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。

使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。

## <a name="log-level"></a>記錄層級

每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。 記錄層級表示嚴重性或重要性。 例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。

下列程式碼會建立 `Information` 與 `Warning` 記錄：

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。 第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。 此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。

在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。 這些方法會呼叫接受 `Log` 參數的 `LogLevel`。 您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。 如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。

ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。

* 追蹤 = 0

  針對通常只對偵錯有價值的資訊。 這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。 *預設為停用。*

* 偵錯 = 1

  針對可在開發與偵錯中使用的資訊。 範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。

* 資訊 = 2

  針對一般應用程式流程的追蹤。 這些記錄通常有一些長期值。 範例：`Request received for path /api/todo`

* 警告 = 3

  針對應用程式流程中發生的異常或意外事件。 這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。 已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。 範例：`FileNotFoundException for file quotes.txt.`

* 錯誤 = 4

  發生無法處理的錯誤和例外狀況。 這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。 範例記錄訊息：`Cannot insert record due to duplicate key violation.`

* 重大 = 5

  發生需要立即注意的失敗。 範例：資料遺失情況、磁碟空間不足。

使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。 例如：

* 在生產環境中：
  * `Trace`透過`Information`層級的記錄會產生大量的詳細記錄訊息。 若要控制成本，而不超過資料儲存體限制`Trace` ， `Information`請將層級訊息記錄到高容量、低成本的資料存放區。
  * `Warning`透過`Critical`層級登入通常會產生較少、較小的記錄檔訊息。 因此，成本和儲存體限制通常不會造成問題，因此可讓您更靈活地選擇資料存放區。
* 在開發期間：
  * 將`Warning` `Critical`訊息記錄到主控台。
  * 進行`Trace`疑難排解`Information`時，新增訊息。

本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。

ASP.NET Core 會寫入架構事件的記錄。 此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。 以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：

```console
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[3]
      Route matched with {action = "GetById", controller = "Todo", page = ""}. Executing controller action with signature Microsoft.AspNetCore.Mvc.IActionResult GetById(System.String) on controller TodoApiSample.Controllers.TodoController (TodoApiSample).
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of authorization filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of resource filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of action filters (in the following order): Microsoft.AspNetCore.Mvc.Filters.ControllerActionFilter (Order: -2147483648), Microsoft.AspNetCore.Mvc.ModelBinding.UnsupportedContentTypeFilter (Order: -3000)
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of exception filters (in the following order): None
dbug: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[1]
      Execution plan of result filters (in the following order): Microsoft.AspNetCore.Mvc.ViewFeatures.Filters.SaveTempDataFilter
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[22]
      Attempting to bind parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[44]
      Attempting to bind parameter 'id' of type 'System.String' using the name 'id' in request data ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.Binders.SimpleTypeModelBinder[45]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[23]
      Done attempting to bind parameter 'id' of type 'System.String'.
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[26]
      Attempting to validate the bound parameter 'id' of type 'System.String' ...
dbug: Microsoft.AspNetCore.Mvc.ModelBinding.ParameterBinder[27]
      Done attempting to validate the bound parameter 'id' of type 'System.String'.
info: TodoApiSample.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker[2]
      Executed action TodoApiSample.Controllers.TodoController.GetById (TodoApiSample) in 32.690400000000004ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'TodoApiSample.Controllers.TodoController.GetById (TodoApiSample)'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 176.9103ms 404
```

## <a name="log-event-id"></a>記錄事件識別碼

每個記錄都可以指定「事件識別碼」**。 範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

事件識別碼會將一組事件關聯。 例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。

記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。 偵錯提供者不會顯示事件識別碼。 主控台提供者會在類別後面以括弧顯示事件識別碼：

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>記錄訊息範本

每個記錄都會指定訊息範本。 訊息範本可以包含有關提供哪個引數的預留位置。 使用名稱而非數字做為預留位置。

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。 請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

此程式碼會使用依順序的參數值建立記錄訊息：

```text
Parameter values: parm1, parm2
```

記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。 此資訊可讓記錄提供者將參數值儲存為欄位。 例如，假設記錄器方法呼叫看起來像這樣：

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。 查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。

## <a name="logging-exceptions"></a>記錄例外狀況

記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

不同提供者處理例外狀況資訊的方式會不同。 以下是來自上述程式碼中的偵錯提供者輸出範例。

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>記錄篩選

您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。 最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。

若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

### <a name="create-filter-rules-in-configuration"></a>在組態中建立篩選規則

專案範本程式碼會`CreateDefaultBuilder`呼叫以設定主控台、Debug 和 EventSource （ASP.NET Core 2.2 或更新版本）提供者的記錄。 `CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。

組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。 建立 `ILogger` 物件時，會為每個提供者選取一個規則。

### <a name="filter-rules-in-code"></a>程式碼中的篩選規則

下列範例說明如何在程式碼中註冊篩選規則：

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=2-3)]

第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。 第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。

### <a name="how-filtering-rules-are-applied"></a>如何套用篩選規則

組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。 前六項來自組態範例，最後兩項來自程式碼範例。

| Number | 提供者      | 開頭如下的類別...          | 最低記錄層級 |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | 偵錯         | 所有類別                          | 資訊       |
| 2      | 主控台       | Microsoft.AspNetCore.Mvc.Razor.Internal | 警告           |
| 3      | 主控台       | Microsoft.AspNetCore.Mvc.Razor.Razor    | 偵錯             |
| 4      | 主控台       | Microsoft.AspNetCore.Mvc.Razor          | 錯誤             |
| 5      | 主控台       | 所有類別                          | 資訊       |
| 6      | 所有提供者 | 所有類別                          | 偵錯             |
| 7      | 所有提供者 | System (系統)                                  | 偵錯             |
| 8      | 偵錯         | Microsoft                               | 追蹤             |

建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。 由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。 系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。

當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：

* 選取所有符合提供者或其別名的規則。 如果找不到符合的項目，請選取所有規則搭配空白提供者。
* 從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。 如果找不到符合的項目，請選取未指定類別的所有規則。
* 如果選取多個規則，請使用**最後**一個。
* 如果未選取任何規則，請使用 `MinimumLevel`。

使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：

* 針對偵錯提供者，規則 1、6 和 8 均適用。 規則 8 最明確，因此會選取此規則。
* 針對主控台提供者，規則 3、4、5 和 6 均適用。 規則 3 最明確。

產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。 `Debug` 層級以上的記錄會傳送到主控台提供者。

### <a name="provider-aliases"></a>提供者別名

每個提供者都會定義「別名」**，可在設定中用來取代完整類型名稱。  針對內建提供者，請使用下列別名：

* 主控台
* 偵錯
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>預設最低層級

只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。 下列範例示範如何設定最低層級：

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。

### <a name="filter-functions"></a>篩選函數

針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。 函式中的程式碼可以存取提供者類型、類別與記錄層級。 例如：

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

## <a name="system-categories-and-levels"></a>系統類別與層級

以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：

| 類別                            | 備忘錄 |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | 一般 ASP.NET Core 診斷。 |
| Microsoft.AspNetCore.DataProtection | 已考慮、發現及使用哪些金鑰。 |
| Microsoft.AspNetCore.HostFiltering  | 允許主機。 |
| Microsoft.AspNetCore.Hosting        | HTTP 要求花了多少時間完成，以及其開始時間。 載入了哪些裝載啟動組件。 |
| Microsoft.AspNetCore.Mvc            | MVC 與 Razor 診斷。 模型繫結、篩選執行、檢視編譯、動作選取。 |
| Microsoft.AspNetCore.Routing        | 路由比對資訊。 |
| Microsoft.AspNetCore.Server         | 連線開始、停止與保持運作回應。 HTTPS 憑證資訊。 |
| Microsoft.AspNetCore.StaticFiles    | 提供的檔案。 |
| Microsoft.EntityFrameworkCore       | 一般 Entity Framework Core 診斷。 資料庫活動與設定、變更偵測、移轉。 |

## <a name="log-scopes"></a>記錄範圍

 「範圍」** 可用來將邏輯作業組成群組。 此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。 例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。

範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。 透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列程式碼會啟用主控台提供者的範圍：

*Program.cs*：

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

> [!NOTE]
> 您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。
>
> 如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。

每個記錄訊息包含範圍資訊：

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>內建記錄提供者

ASP.NET Core 隨附下列提供者：

* [主控台](#console-provider)
* [偵錯](#debug-provider)
* [EventSource](#event-source-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

### <a name="console-provider"></a>Console 提供者

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。 

```csharp
logging.AddConsole();
```

若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a>Debug 提供者

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。

在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a>事件來源提供者

在[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)跨平臺的事件來源中，會寫入名`Microsoft-Extensions-Logging`為的記錄檔。 在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。

```csharp
logging.AddEventSourceLogger();
```

呼叫以建立主機時`CreateDefaultBuilder` ，會自動新增事件來源提供者。

#### <a name="dotnet-trace-tooling"></a>dotnet 追蹤工具

[Dotnet 追蹤](/dotnet/core/diagnostics/dotnet-trace)工具是一種跨平臺 CLI 全域工具，可讓您收集執行中進程的 .net Core 追蹤。 此工具會<xref:Microsoft.Extensions.Logging.EventSource>使用來收集提供<xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>者資料。

使用下列命令安裝 dotnet 追蹤工具：

```dotnetcli
dotnet tool install --global dotnet-trace
```

使用 dotnet 追蹤工具，從應用程式收集追蹤：

1. 如果應用程式未使用`CreateDefaultBuilder`建立主機，請將[事件來源提供者](#event-source-provider)新增至應用程式的記錄設定。

1. 使用`dotnet run`命令執行應用程式。

1. 判斷 .NET Core 應用程式的處理序識別碼（PID）：

   * 在 Windows 上，請使用下列其中一種方法：
     * 工作管理員（Ctrl + Alt + Del）
     * [tasklist 命令](/windows-server/administration/windows-commands/tasklist)
     * [取得進程 Powershell 命令](/powershell/module/microsoft.powershell.management/get-process)
   * 在 Linux 上，請使用[pidof 命令](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html)。

   尋找與應用程式元件同名之進程的 PID。

1. 執行`dotnet trace`命令。

   一般命令語法：

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   使用 PowerShell 命令 shell 時，將`--providers`值以單引號括住（`'`）：

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   在非 Windows 平臺上，新增`-f speedscope`選項以將輸出追蹤檔案的格式變更為。 `speedscope`

   | 關鍵字 | 描述 |
   | :-----: | ----------- |
   | 1       | 記錄有關的`LoggingEventSource`中繼事件。 不會記錄來自`ILogger`的事件）。 |
   | 2       | 當呼叫時`Message` `ILogger.Log()` ，開啟事件。 以程式設計方式（未格式化）提供資訊。 |
   | 4       | 當呼叫時`FormatMessage` `ILogger.Log()` ，開啟事件。 提供資訊的格式化字串版本。 |
   | 8       | 當呼叫時`MessageJson` `ILogger.Log()` ，開啟事件。 提供引數的 JSON 標記法。 |

   | 事件層級 | 說明     |
   | :---------: | --------------- |
   | 0           | `LogAlways`     |
   | 1           | `Critical`      |
   | 2           | `Error`         |
   | 3           | `Warning`       |
   | 4           | `Informational` |
   | 5           | `Verbose`       |

   `FilterSpecs`和`{Event Level}`的`{Logger Category}`專案代表其他記錄篩選準則。 以`FilterSpecs`分號（`;`）分隔專案。

   使用 Windows 命令 shell 的範例（** ** `--providers`值前後沒有單引號）：

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   上述命令會啟用：

   * 事件來源記錄器，用來產生錯誤（`4``2`）的格式化字串（）。
   * `Microsoft.AspNetCore.Hosting`在`Informational`記錄層級進行記錄`4`（）。

1. 按 Enter 鍵或 Ctrl + C 來停止 dotnet 追蹤工具。

   追蹤會以名稱*nettrace*儲存在`dotnet trace`命令執行所在的資料夾中。

1. 使用[Perfview](#perfview)開啟追蹤。 開啟*nettrace*檔案，並流覽追蹤事件。

如需詳細資訊，請參閱

* [效能分析公用程式追蹤（dotnet-追蹤）](/dotnet/core/diagnostics/dotnet-trace) （.net Core 檔）
* [效能分析公用程式追蹤（dotnet 追蹤）](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) （dotnet/診斷 GitHub 存放庫檔）
* [LoggingEventSource 類別](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource)（.Net API 瀏覽器）
* <xref:System.Diagnostics.Tracing.EventLevel>
* [LoggingEventSource 參考來源（3.0）](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash;若要取得不同版本的參考來源，請將分支變更`release/{Version}`為， `{Version}`其中是所需 ASP.NET Core 的版本。
* [Perfview](#perfview) &ndash;適用于查看事件來源追蹤。

#### <a name="perfview"></a>Perfview

使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。 此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。

若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]**** 清單 (請勿遺漏字串開頭的星號)。

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows EventLog 提供者

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。

```csharp
logging.AddEventLog();
```

[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。 若`null`未指定，則會使用下列預設設定：

* `LogName`&ndash; 「應用程式」
* `SourceName`&ndash; 「.Net 執行時間」
* `MachineName` &ndash; 本機電腦

記錄[警告層級和更新版本](#log-level)的事件。 若要記錄低於`Warning`的事件，請明確設定記錄層級。 例如，將下列內容新增至*appsettings*檔案：

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a>TraceSource 提供者

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。

```csharp
logging.AddTraceSource(sourceSwitchName);
```

[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。

若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。 該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。

### <a name="azure-app-service-provider"></a>Azure App Service 提供者

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。

```csharp
logging.AddAzureWebAppDiagnostics();
```

該提供者套件並未包含在共用架構中。 若要使用提供者，請將提供者套件新增至專案。

如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]**** 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。 當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。

* **應用程式記錄 (檔案系統)**
* **應用程式記錄 (Blob)**

記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。 預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。 預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。

此提供者僅適用於專案在 Azure 環境中執行的情況。 若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。

#### <a name="azure-log-streaming"></a>Azure 記錄資料流

Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：

* 應用程式伺服器
* 網頁伺服器
* 失敗的要求追蹤

若要設定 Azure 記錄資料流：

* 從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]****。
* 將 [應用程式記錄 (檔案系統)]**** 設定為 [開啟]****。
* 選擇記錄 [層級]****。 此設定僅適用于 Azure 記錄串流，而不適用於應用程式中的其他記錄提供者。

瀏覽到 [記錄資料流]**** 頁面以檢視應用程式訊息。 這些是應用程式透過 `ILogger` 介面產生的訊息。

### <a name="azure-application-insights-trace-logging"></a>Azure Application Insights 追蹤記錄

[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。 Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。 如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。

記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。 如果您使用此套件，就不需安裝提供者套件。

不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。

如需詳細資訊，請參閱下列資源：

* [Application Insights 概觀](/azure/application-insights/app-insights-overview)
* [適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。
* [適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。
* [Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。
* [安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。

## <a name="third-party-logging-providers"></a>協力廠商記錄提供者

可搭配 ASP.NET Core 使用的協力廠商記錄架構：

* [elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))
* [Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）
* [Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))

某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。

使用協力廠商架構類似於使用內建的提供者之一：

1. 將 NuGet 套件新增至專案。
1. 通話記錄`ILoggerFactory`架構所提供的擴充方法。

如需詳細資訊，請參閱每個提供者的文件。 Microsoft 不支援第三方記錄提供者。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/loggermessage>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)

.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。 此文章說明如何搭配內建提供者使用 API。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="add-providers"></a>新增提供者

記錄提供者會顯示或儲存記錄。 例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。 您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。

若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

上述程式碼需要對 `Microsoft.Extensions.Logging` 與 `Microsoft.Extensions.Configuration` 的參考。

預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：

* 主控台
* 偵錯
* EventSource (從 ASP.NET Core 2.2 開始)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。 呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。

## <a name="create-logs"></a>建立記錄

若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。 在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。 在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。

下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。 記錄「類別」** 是與每個記錄關聯的字串。 由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。 記錄「層級」** 指出已記錄事件的嚴重性。

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。

### <a name="create-logs-in-startup"></a>在啟動中建立記錄

若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a>在 Program 類別中建立記錄

若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

不直接支援在主機結構期間進行記錄。 不過，您可以使用個別的記錄器。 在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateWebHostBuilder`。  `AddSerilog`會使用中`Log.Logger`指定的靜態設定：

```csharp
using System;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var builtConfig = new ConfigurationBuilder()
            .AddJsonFile("appsettings.json")
            .AddCommandLine(args)
            .Build();

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File(builtConfig["Logging:FilePath"])
            .CreateLogger();

        try
        {
            return WebHost.CreateDefaultBuilder(args)
                .ConfigureServices((context, services) =>
                {
                    services.AddMvc();
                })
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    config.AddConfiguration(builtConfig);
                })
                .ConfigureLogging(logging =>
                {
                    logging.AddSerilog();
                })
                .UseStartup<Startup>();
        }
        catch (Exception ex)
        {
            Log.Fatal(ex, "Host builder error");

            throw;
        }
        finally
        {
            Log.CloseAndFlush();
        }
    }
}
```

### <a name="no-asynchronous-logger-methods"></a>無非同步記錄器方法

記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。 若您的記錄資料存放區很慢，請不要直接寫入其中。 請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。 例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。 相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。 如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。

## <a name="configuration"></a>設定

記錄提供者設定是由一或多個記錄提供者提供：

* 檔案格式 (INI、JSON 及 XML)。
* 命令列引數。
* 環境變數。
* 記憶體內部 .NET 物件。
* 未加密的[祕密管理員](xref:security/app-secrets)儲存體。
* 類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。
* 自訂提供者 (已安裝或已建立)。

例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。 下列範例顯示一般 *appsettings.Development.json* 檔案的內容：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。

`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。 在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。

`Logging` 下的其他屬性可指定記錄提供者。 範例使用主控台提供者。 如果提供者支援[記錄範圍](#log-scopes)， `IncludeScopes`則指出是否已啟用。 提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。 提供者下的 `LogLevel` 會指定提供者的記錄層級。

若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。

記錄 API 不包含在應用程式執行時變更記錄層級的案例。 不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。 例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)（由`CreateDefaultBuilder`新增）以讀取設定檔案，預設會重載記錄設定。 如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。

如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。

## <a name="sample-logging-output"></a>範例記錄輸出

使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。 主控台輸出範例如下：

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。

以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApi" 為開頭。 開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。 ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。

本文的其餘部分將說明記錄的一些詳細資料和選項。

## <a name="nuget-packages"></a>NuGet 套件

`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。

## <a name="log-category"></a>記錄分類

建立 `ILogger` 物件時，會為它指定「類別」**。 該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。 類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。

使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。

## <a name="log-level"></a>記錄層級

每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。 記錄層級表示嚴重性或重要性。 例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。

下列程式碼會建立 `Information` 與 `Warning` 記錄：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。 第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。 此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。

在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。 這些方法會呼叫接受 `Log` 參數的 `LogLevel`。 您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。 如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。

ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。

* 追蹤 = 0

  針對通常只對偵錯有價值的資訊。 這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。 *預設為停用。*

* 偵錯 = 1

  針對可在開發與偵錯中使用的資訊。 範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。

* 資訊 = 2

  針對一般應用程式流程的追蹤。 這些記錄通常有一些長期值。 範例：`Request received for path /api/todo`

* 警告 = 3

  針對應用程式流程中發生的異常或意外事件。 這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。 已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。 範例：`FileNotFoundException for file quotes.txt.`

* 錯誤 = 4

  發生無法處理的錯誤和例外狀況。 這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。 範例記錄訊息：`Cannot insert record due to duplicate key violation.`

* 重大 = 5

  發生需要立即注意的失敗。 範例：資料遺失情況、磁碟空間不足。

使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。 例如：

* 在生產環境中：
  * `Trace`透過`Information`層級的記錄會產生大量的詳細記錄訊息。 若要控制成本，而不超過資料儲存體限制`Trace` ， `Information`請將層級訊息記錄到高容量、低成本的資料存放區。
  * `Warning`透過`Critical`層級登入通常會產生較少、較小的記錄檔訊息。 因此，成本和儲存體限制通常不會造成問題，因此可讓您更靈活地選擇資料存放區。
* 在開發期間：
  * 將`Warning` `Critical`訊息記錄到主控台。
  * 進行`Trace`疑難排解`Information`時，新增訊息。

本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。

ASP.NET Core 會寫入架構事件的記錄。 此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。 以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>記錄事件識別碼

每個記錄都可以指定「事件識別碼」**。 範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

事件識別碼會將一組事件關聯。 例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。

記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。 偵錯提供者不會顯示事件識別碼。 主控台提供者會在類別後面以括弧顯示事件識別碼：

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>記錄訊息範本

每個記錄都會指定訊息範本。 訊息範本可以包含有關提供哪個引數的預留位置。 使用名稱而非數字做為預留位置。

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。 請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

此程式碼會使用依順序的參數值建立記錄訊息：

```text
Parameter values: parm1, parm2
```

記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。 此資訊可讓記錄提供者將參數值儲存為欄位。 例如，假設記錄器方法呼叫看起來像這樣：

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。 查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。

## <a name="logging-exceptions"></a>記錄例外狀況

記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

不同提供者處理例外狀況資訊的方式會不同。 以下是來自上述程式碼中的偵錯提供者輸出範例。

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>記錄篩選

您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。 最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。

若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

### <a name="create-filter-rules-in-configuration"></a>在組態中建立篩選規則

專案範本程式碼會`CreateDefaultBuilder`呼叫以設定主控台、Debug 和 EventSource （ASP.NET Core 2.2 或更新版本）提供者的記錄。 `CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。

組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。 建立 `ILogger` 物件時，會為每個提供者選取一個規則。

### <a name="filter-rules-in-code"></a>程式碼中的篩選規則

下列範例說明如何在程式碼中註冊篩選規則：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。 第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。

### <a name="how-filtering-rules-are-applied"></a>如何套用篩選規則

組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。 前六項來自組態範例，最後兩項來自程式碼範例。

| Number | 提供者      | 開頭如下的類別...          | 最低記錄層級 |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | 偵錯         | 所有類別                          | 資訊       |
| 2      | 主控台       | Microsoft.AspNetCore.Mvc.Razor.Internal | 警告           |
| 3      | 主控台       | Microsoft.AspNetCore.Mvc.Razor.Razor    | 偵錯             |
| 4      | 主控台       | Microsoft.AspNetCore.Mvc.Razor          | 錯誤             |
| 5      | 主控台       | 所有類別                          | 資訊       |
| 6      | 所有提供者 | 所有類別                          | 偵錯             |
| 7      | 所有提供者 | System (系統)                                  | 偵錯             |
| 8      | 偵錯         | Microsoft                               | 追蹤             |

建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。 由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。 系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。

當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：

* 選取所有符合提供者或其別名的規則。 如果找不到符合的項目，請選取所有規則搭配空白提供者。
* 從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。 如果找不到符合的項目，請選取未指定類別的所有規則。
* 如果選取多個規則，請使用**最後**一個。
* 如果未選取任何規則，請使用 `MinimumLevel`。

使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：

* 針對偵錯提供者，規則 1、6 和 8 均適用。 規則 8 最明確，因此會選取此規則。
* 針對主控台提供者，規則 3、4、5 和 6 均適用。 規則 3 最明確。

產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。 `Debug` 層級以上的記錄會傳送到主控台提供者。

### <a name="provider-aliases"></a>提供者別名

每個提供者都會定義「別名」**，可在設定中用來取代完整類型名稱。  針對內建提供者，請使用下列別名：

* 主控台
* 偵錯
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>預設最低層級

只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。 下列範例示範如何設定最低層級：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。

### <a name="filter-functions"></a>篩選函數

針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。 函式中的程式碼可以存取提供者類型、類別與記錄層級。 例如：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a>系統類別與層級

以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：

| 類別                            | 備忘錄 |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | 一般 ASP.NET Core 診斷。 |
| Microsoft.AspNetCore.DataProtection | 已考慮、發現及使用哪些金鑰。 |
| Microsoft.AspNetCore.HostFiltering  | 允許主機。 |
| Microsoft.AspNetCore.Hosting        | HTTP 要求花了多少時間完成，以及其開始時間。 載入了哪些裝載啟動組件。 |
| Microsoft.AspNetCore.Mvc            | MVC 和Razor診斷。 模型繫結、篩選執行、檢視編譯、動作選取。 |
| Microsoft.AspNetCore.Routing        | 路由比對資訊。 |
| Microsoft.AspNetCore.Server         | 連線開始、停止與保持運作回應。 HTTPS 憑證資訊。 |
| Microsoft.AspNetCore.StaticFiles    | 提供的檔案。 |
| Microsoft.EntityFrameworkCore       | 一般 Entity Framework Core 診斷。 資料庫活動與設定、變更偵測、移轉。 |

## <a name="log-scopes"></a>記錄範圍

 「範圍」** 可用來將邏輯作業組成群組。 此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。 例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。

範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。 透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列程式碼會啟用主控台提供者的範圍：

*Program.cs*：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。
>
> 如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。

每個記錄訊息包含範圍資訊：

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>內建記錄提供者

ASP.NET Core 隨附下列提供者：

* [主控台](#console-provider)
* [偵錯](#debug-provider)
* [EventSource](#event-source-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。

### <a name="console-provider"></a>Console 提供者

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。 

```csharp
logging.AddConsole();
```

若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a>Debug 提供者

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。

在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a>事件來源提供者

在[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)跨平臺的事件來源中，會寫入名`Microsoft-Extensions-Logging`為的記錄檔。 在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。

```csharp
logging.AddEventSourceLogger();
```

呼叫以建立主機時`CreateDefaultBuilder` ，會自動新增事件來源提供者。

使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。 此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。

若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]**** 清單 (請勿遺漏字串開頭的星號)。

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows EventLog 提供者

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。

```csharp
logging.AddEventLog();
```

[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。 若`null`未指定，則會使用下列預設設定：

* `LogName`&ndash; 「應用程式」
* `SourceName`&ndash; 「.Net 執行時間」
* `MachineName` &ndash; 本機電腦

記錄[警告層級和更新版本](#log-level)的事件。 若要記錄低於`Warning`的事件，請明確設定記錄層級。 例如，將下列內容新增至*appsettings*檔案：

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a>TraceSource 提供者

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。

```csharp
logging.AddTraceSource(sourceSwitchName);
```

[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。

若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。 該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。

### <a name="azure-app-service-provider"></a>Azure App Service 提供者

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。

```csharp
logging.AddAzureWebAppDiagnostics();
```

[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。 當以 .NET Framework 為目標或是參考 `Microsoft.AspNetCore.App` 中繼套件時，請將提供者套件新增至專案。 

<xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。 設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。 (*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)

當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]**** 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。 當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。

* **應用程式記錄 (檔案系統)**
* **應用程式記錄 (Blob)**

記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。 預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。 預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。

此提供者僅適用於專案在 Azure 環境中執行的情況。 若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。

#### <a name="azure-log-streaming"></a>Azure 記錄資料流

Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：

* 應用程式伺服器
* 網頁伺服器
* 失敗的要求追蹤

若要設定 Azure 記錄資料流：

* 從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]****。
* 將 [應用程式記錄 (檔案系統)]**** 設定為 [開啟]****。
* 選擇記錄 [層級]****。 此設定僅適用于 Azure 記錄串流，而不適用於應用程式中的其他記錄提供者。

瀏覽到 [記錄資料流]**** 頁面以檢視應用程式訊息。 這些是應用程式透過 `ILogger` 介面產生的訊息。

### <a name="azure-application-insights-trace-logging"></a>Azure Application Insights 追蹤記錄

[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。 Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。 如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。

記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。 如果您使用此套件，就不需安裝提供者套件。

不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。

如需詳細資訊，請參閱下列資源：

* [Application Insights 概觀](/azure/application-insights/app-insights-overview)
* [適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。
* [適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。
* [Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。
* [安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。

## <a name="third-party-logging-providers"></a>協力廠商記錄提供者

可搭配 ASP.NET Core 使用的協力廠商記錄架構：

* [elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))
* [Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）
* [Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))

某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。

使用協力廠商架構類似於使用內建的提供者之一：

1. 將 NuGet 套件新增至專案。
1. 通話記錄`ILoggerFactory`架構所提供的擴充方法。

如需詳細資訊，請參閱每個提供者的文件。 Microsoft 不支援第三方記錄提供者。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
