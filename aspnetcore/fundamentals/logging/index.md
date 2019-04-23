---
title: ASP.NET Core 中的記錄
author: tdykstra
description: 了解 ASP.NET Core 中的記錄架構。 探索內建記錄提供者，並深入了解熱門協力廠商提供者。
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 8a2e310b47e32e9015b0c127ed79d8f6bdf2e44d
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982849"
---
# <a name="logging-in-aspnet-core"></a>ASP.NET Core 中的記錄

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。 此文章說明如何搭配內建提供者使用 API。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>新增提供者

記錄提供者會顯示或儲存記錄。 例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。 您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。

::: moniker range=">= aspnetcore-2.0"

若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：

* 主控台
* 偵錯
* EventSource (從 ASP.NET Core 2.2 開始)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。 呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要使用提供者，請安裝其 NuGet 套件，並在 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 的執行個體上呼叫該提供者的擴充方法：

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供 `ILoggerFactory` 執行個體。 `AddConsole` 和 `AddDebug` 擴充方法定義於 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 和 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 套件中。 每個擴充方法會呼叫 `ILoggerFactory.AddProvider` 方法，並傳入提供者的執行個體。

> [!NOTE]
> [範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x)會在 `Startup.Configure` 方法中新增記錄提供者。 若要從先前執行的程式碼取得記錄輸出，請在 `Startup` 類別建構函式中新增記錄提供者。

::: moniker-end

在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。

## <a name="create-logs"></a>建立記錄

從 DI 取得 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。

::: moniker range=">= aspnetcore-2.0"

下列控制器範例會建立 `Information` 與 `Warning` 記錄。 「類別」是 `TodoApiSample.Controllers.TodoController` (範例應用程式中 `TodoController` 的完整類別名稱)：

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

下鎳 Razor Pages 範例會建立記錄，其中「層級」*l* 為 `Information` 且「類別」 為 `TodoApiSample.Pages.AboutModel`：

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

前述範例會建立記錄，其中「層級」 為 `Information` 與 `Warning` 且「類別」為 `TodoController`類別。 

::: moniker-end

記錄「層級」 指出已記錄事件的嚴重性。 記錄「類別」是與每個記錄關聯的字串。 `ILogger<T>` 執行個體會建立使用類型 `T` 做為類別之完整名稱的記錄。 此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>在啟動中建立記錄

若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>在程式中建立記錄

若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>無非同步記錄器方法

記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。 若您的記錄資料存放區很慢，請不要直接寫入其中。 請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。 例如，如果您要登入 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。 相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。

## <a name="configuration"></a>Configuration

記錄提供者設定是由一或多個記錄提供者提供：

* 檔案格式 (INI、JSON 及 XML)。
* 命令列引數。
* 環境變數。
* 記憶體內部 .NET 物件。
* 未加密的[祕密管理員](xref:security/app-secrets)儲存體。
* 類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。
* 自訂提供者 (已安裝或已建立)。

例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。 下列範例顯示一般 *appsettings.Development.json* 檔案的內容：

::: moniker range=">= aspnetcore-2.1"

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

`Logging` 下的其他屬性可指定記錄提供者。 範例使用主控台提供者。 若提供者支援[記錄範圍](#log-scopes)，`IncludeScopes` 會指出是否已啟用記錄範圍。 提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。 提供者下的 `LogLevel` 會指定提供者的記錄層級。

若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel` 索引鍵代表記錄名稱。 `Default` 索引鍵會套用到未明確列出的記錄。 此值代表套用到指定記錄的[記錄層級](#log-level)。

::: moniker-end

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

這些透過上一節中所示的 `ILogger` 呼叫建立的記錄是以 "TodoApi.Controllers.TodoController" 為開頭。 開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。 ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。

本文的其餘部分將說明記錄的一些詳細資料和選項。

## <a name="nuget-packages"></a>NuGet 套件

`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。

## <a name="log-category"></a>記錄類別

建立 `ILogger` 物件時，會為它指定「類別」。 該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。 類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。

使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。

## <a name="log-level"></a>記錄層級

每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。 記錄層級表示嚴重性或重要性。 例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。

下列程式碼會建立 `Information` 與 `Warning` 記錄：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。 第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。 此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。

在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。 這些方法會呼叫接受 `Log` 參數的 `LogLevel`。 您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。 如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。

ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。

* 追蹤 = 0

  針對通常只對偵錯有價值的資訊。 這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。 預設為停用。

* 偵錯 = 1

  針對可在開發與偵錯中使用的資訊。 範例：`Entering method Configure with flag set to true.` 只有在進行疑難排解時才在生產環境中啟用 `Debug` 層級記錄，因為此類記錄的數目非常多。

* 資訊 = 2

  針對一般應用程式流程的追蹤。 這些記錄通常有一些長期值。 範例：`Request received for path /api/todo`

* 警告 = 3

  針對應用程式流程中發生的異常或意外事件。 這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。 已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。 範例：`FileNotFoundException for file quotes.txt.`

* 錯誤 = 4

  發生無法處理的錯誤和例外狀況。 這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。 範例記錄訊息：`Cannot insert record due to duplicate key violation.`

* 重大 = 5

  發生需要立即注意的失敗。 範例：資料遺失情況、磁碟空間不足。

使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。 例如：

* 在生產環境中，透過 `Information` 層級將 `Trace` 傳送到大量資料存放區。 透過 `Critical` 將 `Warning` 傳送到值資料存放區。
* 在開發期間，透過 `Critical` 將 `Warning` 傳送到主控台，並在進行疑難排解時透過 `Information` 新增 `Trace`。

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

每個記錄都可以指定「事件識別碼」。 範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。 請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

此程式碼會使用依順序的參數值建立記錄訊息：

```
Parameter values: parm1, parm2
```

記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。 此資訊可讓記錄提供者將參數值儲存為欄位。 例如，假設記錄器方法呼叫看起來像這樣：

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。 查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。

## <a name="logging-exceptions"></a>記錄例外狀況

記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

不同提供者處理例外狀況資訊的方式會不同。 以下是來自上述程式碼中的偵錯提供者輸出範例。

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>記錄篩選

::: moniker range=">= aspnetcore-2.0"

您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。 最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。

若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

### <a name="create-filter-rules-in-configuration"></a>在組態中建立篩選規則

專案範本程式碼會呼叫 `CreateDefaultBuilder` 以設定主控台與偵錯提供者的記錄。 `CreateDefaultBuilder` 方法也會使用如下所示的程式碼，將記錄設定為尋找 `Logging` 區段中的組態：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。 建立 `ILogger` 物件時，會為每個提供者選取一個規則。

### <a name="filter-rules-in-code"></a>程式碼中的篩選規則

下列範例說明如何在程式碼中註冊篩選規則：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。 第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。

### <a name="how-filtering-rules-are-applied"></a>如何套用篩選規則

組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。 前六項來自組態範例，最後兩項來自程式碼範例。

| number | 提供者      | 開頭如下的類別...          | 最低記錄層級 |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | 偵錯         | 所有類別                          | 資訊       |
| 2      | 主控台       | Microsoft.AspNetCore.Mvc.Razor.Internal | 警告           |
| 3      | 主控台       | Microsoft.AspNetCore.Mvc.Razor.Razor    | 偵錯             |
| 4      | 主控台       | Microsoft.AspNetCore.Mvc.Razor          | 錯誤             |
| 5      | 主控台       | 所有類別                          | 資訊       |
| 6      | 所有提供者 | 所有類別                          | 偵錯             |
| 7      | 所有提供者 | 系統                                  | 偵錯             |
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

每個提供者都會定義「別名」，可在設定中用來取代完整類型名稱。  針對內建提供者，請使用下列別名：

* 主控台
* 偵錯
* EventLog
* AzureAppServicesFile
* AzureAppServicesBlob
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>預設最低層級

只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。 下列範例示範如何設定最低層級：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。

### <a name="filter-functions"></a>篩選函式

針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。 函式中的程式碼可以存取提供者類型、類別與記錄層級。 例如：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

某些記錄提供者可讓您指定何時應該將記錄寫入儲存媒體，何時應該根據記錄層級和類別加以略過。

`AddConsole` 與 `AddDebug` 擴充方法提供多載，可接受篩選條件。 下列範例程式碼會使主控台提供者略過 `Warning` 層級以下的記錄，而偵錯提供者則會略過架構所建立的記錄。

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` 方法具有接受 `EventLogSettings` 執行個體的多載，其 `Filter` 屬性中可能包含篩選函式。 TraceSource 提供者未提供上述任何多載，因為其記錄層級和其他參數會根據其所使用的 `SourceSwitch` 和 `TraceListener`。

若要為已向 `ILoggerFactory` 執行個體註冊的所有提供者設定篩選規則，請使用 `WithFilter` 擴充方法。 下列範例會將架構記錄 (開頭為 "Microsoft" 或 "System" 的類別) 限制為警告，同時在偵錯層級記錄由應用程式程式碼所建立的記錄。

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

若要防止寫入任何記錄，請指定 `LogLevel.None` 做為最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

`WithFilter` 擴充方法是由 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 套件提供。 此方法會傳回新的 `ILoggerFactory` 執行個體，這會篩選傳遞至用它來註冊之所有記錄器提供者的記錄訊息。 它不會影響任何其他 `ILoggerFactory` 執行個體，包括原始 `ILoggerFactory` 執行個體。

::: moniker-end

## <a name="system-categories-and-levels"></a>系統類別與層級

以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：

| 分類                            | 注意 |
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

 「範圍」可用來將邏輯作業組成群組。 此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。 例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。

範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。 透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列程式碼會啟用主控台提供者的範圍：

::: moniker range="> aspnetcore-2.0"

*Program.cs*：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。
>
> 如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*：

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

每個記錄訊息包含範圍資訊：

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>內建記錄提供者

ASP.NET Core 隨附下列提供者：

* [Console](#console-provider)
* [偵錯](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)

此文章稍後將說明 [Azure 中的記錄](#logging-in-azure)選項。

如需關於 stdout 記錄的資訊，請參閱<xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>與<xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>。

### <a name="console-provider"></a>Console 提供者

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[AddConsole 多載](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions)可讓您傳入最低記錄層級、篩選函式，以及指出是否支援範圍的布林值。 另一個選項是傳入可指定範圍支援和記錄層級的 `IConfiguration` 物件。

主控台提供者對效能有重大影響，因此通常不適合在生產環境中使用。

當您在 Visual Studio 中建立新的專案時，`AddConsole` 方法看起來像這樣：

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

此程式碼會參考 *appSettings.json* 檔案的 `Logging` 區段：

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

上述設定會將架構記錄限制為警告，同時讓應用程式在偵錯層級記錄，如[記錄篩選](#log-filtering)一節中所述。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

::: moniker-end

若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：

```console
dotnet run
```

### <a name="debug-provider"></a>Debug 提供者

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。

在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[AddDebug 多載](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions)可讓您傳入最低記錄層級或篩選函式。

::: moniker-end

### <a name="eventsource-provider"></a>EventSource 提供者

針對以 ASP.NET Core 1.1.0 或更新版本為目標的應用程式，[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 提供者套件可以實作事件追蹤。 在 Windows 上，它會使用 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。 提供者可以跨平台，但目前沒有適用於 Linux 或 macOS 的事件收集和顯示工具。

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

收集及檢視記錄的一個好方法是使用 [PerfView 公用程式](https://github.com/Microsoft/perfview)。 此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET 所發出之 ETW 事件的最佳體驗。

若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者] 清單 (請勿遺漏字串開頭的星號)。

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Windows EventLog 提供者

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 `EventLogSettings` 或最低記錄層級。

::: moniker-end

### <a name="tracesource-provider"></a>TraceSource 提供者

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。

若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。 該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。

::: moniker range="< aspnetcore-2.0"

下列範例會設定 `TraceSource` 提供者，將 `Warning` 和更高層級訊息記錄至主控台視窗。

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a>Azure 中的記錄

如需 Azure 中的記錄相關資訊，請參閱以下各節：

* [Azure App Service 提供者](#azure-app-service-provider)
* [Azure 記錄串流](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [Azure Application Insights 追蹤記錄](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a>Azure App Service 提供者

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。 提供者套件適用於以 .NET Core 1.1 或更新版本為目標的應用程式。

::: moniker range=">= aspnetcore-2.0"

若以 .NET Core 為目標，請注意下列幾點：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* ASP.NET Core [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)包含提供者套件。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。 若要使用提供者，請安裝套件。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

若以 .NET Framework 為目標，或是參考 `Microsoft.AspNetCore.App` 中繼套件，請將提供者套件新增至專案。 叫用 `AddAzureWebAppDiagnostics`：

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。 設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。 (*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

當您部署到 App Service 應用程式時，應用程式會接受 Azure 入口網站的 [App Service] 頁面之[診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段中的設定。 當更新這些設定時，變更會立即生效，而不需要重新啟動或重新部署應用程式。

![Azure 記錄設定](index/_static/azure-logging-settings.png)

記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。 預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。 預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。

此提供者僅適用於專案在 Azure 環境中執行的情況。 若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。

### <a name="azure-log-streaming"></a>Azure 記錄資料流

Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：

* 應用程式伺服器
* 網頁伺服器
* 失敗的要求追蹤

若要設定 Azure 記錄資料流：

* 從您應用程式的入口網站頁面瀏覽到 [診斷記錄]。
* 將 [應用程式記錄 (檔案系統)] 設定為 [開啟]。

![Azure 入口網站的 [診斷記錄] 頁面](index/_static/azure-diagnostic-logs.png)

瀏覽到 [記錄串流] 頁面以檢視應用程式訊息。 這些是應用程式透過 `ILogger` 介面產生的訊息。

![Azure 入口網站應用程式的 [記錄資料流]](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Azure Application Insights 追蹤記錄

Application Insights SDK 可以收集及回報由 ASP.NET Core 記錄基礎結構所產生的記錄。 如需詳細資訊，請參閱下列資源：

* [Application Insights 概觀](/azure/application-insights/app-insights-overview)
* [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md) (Application Insights 記錄配接器)。
* [Application Insights ILogger 實作範例](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a>協力廠商記錄提供者

可搭配 ASP.NET Core 使用的協力廠商記錄架構：

* [elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-extensions-logging))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))

某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。

使用協力廠商架構類似於使用內建的提供者之一：

1. 將 NuGet 套件新增至專案。
1. 呼叫 `ILoggerFactory`。

如需詳細資訊，請參閱每個提供者的文件。 Microsoft 不支援第三方記錄提供者。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/loggermessage>
