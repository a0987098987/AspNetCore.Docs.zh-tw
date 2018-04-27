---
title: ASP.NET Core 中的記錄
author: ardalis
description: 了解 ASP.NET Core 中的記錄架構。 探索內建記錄提供者，並深入了解熱門協力廠商提供者。
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: aab1190467c13ae121625c377d0908eac2fe8d95
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2018
---
# <a name="logging-in-aspnet-core"></a>ASP.NET Core 中的記錄

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。 內建提供者可讓您將記錄傳送至一或多個目的地，而且您可以外掛協力廠商記錄架構。 本文說明如何在程式碼中使用內建記錄 API 和提供者。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>如何建立記錄

若要建立記錄，請取得[相依性插入](xref:fundamentals/dependency-injection)容器中的 `ILogger` 物件：

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

然後在該記錄器物件上呼叫記錄方法：

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

此範例會建立以 `TodoController` 為「類別」的記錄。 [本文稍後](#log-category)將說明類別。

ASP.NET Core 不會提供非同步記錄器方法，因為記錄應該相當快速，因此不值得使用非同步方法。 如果發生與其相反的情況，請考慮變更您的記錄方式。 如果您的資料存放區很慢，請先將記錄訊息寫入至快速存放區，稍後再移至慢速存放區。 例如，記錄至訊息佇列以供其他處理序讀取及保存至慢速儲存體。

## <a name="how-to-add-providers"></a>如何新增提供者

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
記錄提供者會擷取您使用 `ILogger` 物件建立的訊息，並加以顯示或儲存。 例如，主控台提供者可在主控台中顯示訊息，而 Azure App Service 提供者則可將其儲存在 Azure Blob 儲存體中。

若要使用提供者，請在 *Program.cs* 中呼叫提供者的 `Add<ProviderName>` 擴充方法：

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

預設專案範本可使用 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) 方法來記錄：

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
記錄提供者會擷取您使用 `ILogger` 物件建立的訊息，並加以顯示或儲存。 例如，主控台提供者可在主控台中顯示訊息，而 Azure App Service 提供者則可將其儲存在 Azure Blob 儲存體中。

若要使用提供者，請安裝其 NuGet 套件，並在 `ILoggerFactory` 的執行個體上呼叫該提供者的擴充方法，如下列範例所示。

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [相依性插入](xref:fundamentals/dependency-injection) (DI) 提供 `ILoggerFactory` 執行個體。 `AddConsole` 和 `AddDebug` 擴充方法定義於 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 和 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 套件中。 每個擴充方法會呼叫 `ILoggerFactory.AddProvider` 方法，並傳入提供者的執行個體。 

> [!NOTE]
> 本文的範例應用程式會在 `Startup` 類別的 `Configure` 方法中新增記錄提供者。 如果您想要從先前稍早執行的程式碼取得記錄輸出，請改為在 `Startup` 類別建構函式中新增記錄提供者。 

* * *
您會在本文稍後找到每個[內建記錄提供者](#built-in-logging-providers)的相關資訊，以及[協力廠商記錄提供者](#third-party-logging-providers)的連結。

## <a name="sample-logging-output"></a>範例記錄輸出

當您從命令列執行上一節中所示的範例程式碼時，您會在主控台中看到記錄。 主控台輸出範例如下：

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

這些記錄是透過前往 `http://localhost:5000/api/todo/0` 來建立，這會觸發上一節中所示之兩個 `ILogger` 呼叫的執行。

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

這些透過上一節中所示的 `ILogger` 呼叫建立的記錄是以 "TodoApi.Controllers.TodoController" 為開頭。 開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core。 ASP.NET Core 本身和您的應用程式程式碼會使用相同的記錄 API 和相同的記錄提供者。

本文的其餘部分將說明記錄的一些詳細資料和選項。

## <a name="nuget-packages"></a>NuGet 套件

`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。

## <a name="log-category"></a>記錄類別

您建立的每個記錄都會包含一個「類別」。 當您建立 `ILogger` 物件時會指定類別。 類別可以是任何字串，但通常會使用寫入記錄之來源類別的完整名稱。 例如："TodoApi.Controllers.TodoController"。

您可以將類別指定為字串，或使用擴充方法從類型衍生類別。 若要將類別指定為字串，請在 `ILoggerFactory` 執行個體上呼叫 `CreateLogger`，如下所示。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

在大多時候，使用 `ILogger<T>` 會更容易，如下列範例所示。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

這相當於使用 `T` 的完整類型名稱呼叫 `CreateLogger`。

## <a name="log-level"></a>記錄層級

每次寫入記錄，您都會指定其 [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel)。 記錄層級表示嚴重或重要程度。 例如，您可以在某個方法正常結束時寫入 `Information` 記錄，在某個方法傳回 404 傳回碼時寫入 `Warning` 記錄，並在攔截到未預期的例外狀況時寫入 `Error` 記錄。

在下列程式碼範例中，方法的名稱 (例如 `LogWarning`) 會指定記錄層級。 第一個參數是[記錄事件識別碼](#log-event-id)。 第二個參數是[訊息範本](#log-message-template)，其中的預留位置會置入其餘方法參數所提供的引數值。 本文稍後將更詳細說明方法參數。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

在方法名稱中包含層級的記錄方法是 [ILogger 的擴充方法](/dotnet/api/microsoft.extensions.logging.loggerextensions)。 這些方法會在幕後呼叫接受 `LogLevel` 參數的 `Log` 方法。 您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。 如需詳細資訊，請參閱 [ILogger Interface](/dotnet/api/microsoft.extensions.logging.ilogger) (ILogger 介面) 和[記錄器延伸模組的原始程式碼](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)。

ASP.NET Core 定義下列[記錄層級](/dotnet/api/microsoft.extensions.logging.loglevel)，並從最低嚴重性排列到最高嚴重性。

* 追蹤 = 0

  只對偵錯問題的開發人員重要的資訊。 這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。 預設為停用。 範例：`Credentials: {"User":"someuser", "Password":"P@ssword"}`

* 偵錯 = 1

  在開發和偵錯期間短暫有用的資訊。 範例：`Entering method Configure with flag set to true.`由於 `Debug` 層級記錄的數量很大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用此記錄。

* 資訊 = 2

  用於追蹤應用程式的一般流程。 這些記錄通常有一些長期值。 範例：`Request received for path /api/todo`

* 警告 = 3

  在應用程式流程中發生異常或意外事件。 這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。 已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。 範例：`FileNotFoundException for file quotes.txt.`

* 錯誤 = 4

  發生無法處理的錯誤和例外狀況。 這些訊息會指出目前的活動或作業 (例如目前的 HTTP 要求） 中的失敗，而不是整個應用程式的失敗。 範例記錄訊息：`Cannot insert record due to duplicate key violation.`

* 重大 = 5

  發生需要立即注意的失敗。 範例：資料遺失情況、磁碟空間不足。

您可以使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。 例如，您可能想要在生產環境中，將 `Information` 和較低層級的所有記錄移至磁碟區資料存放區，並將 `Warning` 和更高層級的所有記錄移至數值資料存放區。 在開發期間，您可以如往常般將 `Warning` 或更高嚴重性的記錄傳送至主控台。 然後當您需要進行疑難排解時，您可以新增 `Debug` 層級。 本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。

ASP.NET Core 架構會寫入架構事件的 `Debug` 層級記錄。 本文稍早的記錄範例已排除 `Information` 層級以下的記錄，因此不會顯示任何 `Debug` 層級記錄。 如果您執行的範例應用程式已設定為顯示主控台提供者的 `Debug` 和更高層級記錄，其主控台記錄範例如下。

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

每次寫入記錄，您都會指定一個「事件識別碼」。 範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行這項作業：

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

事件識別碼是您可用來將兩組記錄的事件建立關聯的整數值。 例如，將某個項目新增至購物車的記錄可以是事件識別碼 1000，而完成購買的記錄可以是事件識別碼 1001。

在記錄輸出中，視提供者而定，事件識別碼可能會儲存在欄位中或包含在文字訊息中。 偵錯提供者不會顯示事件識別碼，但主控台提供者會在類別後面的括弧中顯示事件識別碼：

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>記錄訊息範本

每次寫入記錄訊息，您都會提供一個訊息範本。 訊息範本可以是字串，也可以包含要置入引數值的具名預留位置。 範本不是格式字串，而且預留位置必須命名而不是編號。

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。 如果您有下列程式碼：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

產生的記錄訊息看起來像這樣：

```
Parameter values: parm1, parm2
```

記錄架構會以此方式來格式化訊息，以便記錄提供者能夠實作[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 由於引數本身會傳遞給記錄系統，而不只是格式化的訊息範本，因此除了訊息範本以外，記錄提供者還可以將參數值儲存為欄位。 如果您要將記錄輸出導向至 Azure 資料表儲存體，您的記錄器方法呼叫看起來像這樣：

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

每個 Azure 資料表實體可以有 `ID` 和 `RequestTime` 屬性，以簡化記錄資料的查詢。 您可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要剖析文字訊息中的時間。

## <a name="logging-exceptions"></a>記錄例外狀況

記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

不同提供者處理例外狀況資訊的方式會不同。 以下是來自上述程式碼中的偵錯提供者輸出範例。

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>記錄篩選

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。 最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。 

如果您想要隱藏所有記錄，您可以指定 `LogLevel.None` 作為最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

**在組態中建立篩選規則**

這些專案範本會建立程式碼，以呼叫 `CreateDefaultBuilder` 設定主控台和偵錯提供者的記錄。 `CreateDefaultBuilder` 方法也會使用如下所示的程式碼，將記錄設定為尋找 `Logging` 區段中的組態：

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：

[!code-json[](index/sample2/appsettings.json)]

此 JSON 會建立六項篩選規則，一項適用於偵錯提供者、四項適用於主控台提供者，還有一項適用於所有提供者。 您稍後會看到在建立 `ILogger` 物件時，如何為每個提供者只選擇其中一項規則。

**程式碼中的篩選規則**

您可以在程式碼中註冊篩選規則，如下列範例所示：

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。 第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。

**如何套用篩選規則**

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

當您建立要用來寫入記錄的 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一項規則來套用至該記錄器。 由該 `ILogger` 寫入的所有訊息都會根據選取的規則進行篩選。 系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。

當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：

* 選取所有符合提供者或其別名的規則。 如果找不到，請選擇提供者為空白的所有規則。
* 從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。 如果找不到，請選取未指定類別的所有規則。
* 如果選取了多項規則，請使用**最後**一項。
* 如果未選取任何規則，請使用 `MinimumLevel`。

例如，假設您有上述規則清單，並建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：

* 針對偵錯提供者，規則 1、6 和 8 均適用。 規則 8 最明確，因此會選取此規則。
* 針對主控台提供者，規則 3、4、5 和 6 均適用。 規則 3 最明確。

當您使用 `ILogger` 建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的記錄時，`Trace` 和更高層級的記錄會移至偵錯提供者，而 `Debug` 和更高層級的記錄則會移至主控台提供者。

**提供者別名**

您可以在組態中使用類型名稱來指定提供者，但每個提供者定義了方便您使用的更短「別名」。 針對內建提供者，請使用下列別名：

- 主控台
- 偵錯
- EventLog
- AzureAppServices
- TraceSource
- EventSource

**預設最低層級**

只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。 下列範例示範如何設定最低層級：

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。

**篩選函式**

您可以在篩選函式中撰寫程式碼來套用篩選規則。 針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。 函式中的程式碼可存取提供者類型、類別和記錄層級，以判斷是否應該記錄訊息。 例如: 

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
某些記錄提供者可讓您指定何時應該將記錄寫入儲存媒體，何時應該根據記錄層級和類別加以略過。

`AddConsole` 和 `AddDebug` 擴充方法提供多載，可讓您傳入篩選條件。 下列範例程式碼會使主控台提供者略過 `Warning` 層級以下的記錄，而偵錯提供者則會略過架構所建立的記錄。

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog` 方法具有接受 `EventLogSettings` 執行個體的多載，其 `Filter` 屬性中可能包含篩選函式。 TraceSource 提供者未提供上述任何多載，因為其記錄層級和其他參數會根據其所使用的 `SourceSwitch` 和 `TraceListener`。

您可以使用 `WithFilter` 擴充方法，為 `ILoggerFactory` 執行個體中註冊的所有提供者設定篩選規則。 下列範例會將架構記錄 (開頭為 "Microsoft" 或 "System" 的類別) 限制為警告，同時讓應用程式在偵錯層級記錄。

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

如果您想要使用篩選來防止所有記錄針對特定類別寫入，您可以指定 `LogLevel.None` 作為該類別的最低記錄層級。 `LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。

`WithFilter` 擴充方法是由 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 套件提供。 此方法會傳回新的 `ILoggerFactory` 執行個體，這會篩選傳遞至用它來註冊之所有記錄器提供者的記錄訊息。 它不會影響任何其他 `ILoggerFactory` 執行個體，包括原始 `ILoggerFactory` 執行個體。

* * *
## <a name="log-scopes"></a>記錄範圍

您可以將某個「範圍」內的一組邏輯作業群組在一起，以便將相同的資料附加至作為該集合一部分建立的每個記錄。 例如，您可能想要將每個記錄建立為處理交易的一部分，以包含交易識別碼。

範圍是 `ILogger.BeginScope<TState>` 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。 您可以透過將記錄器呼叫包裝在 `using` 區塊中來使用範圍，如下所示：

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列程式碼會啟用主控台提供者的範圍：

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
在 *Program.cs* 中：

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> 您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。 ASP.NET Core 2.1 版將可使用 *appsettings* 組態檔來設定 `IncludeScopes`。

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
在 *Startup.cs* 中：

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

* * *
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

* [Console](#console)
* [偵錯](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>主控台提供者

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。 

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
```csharp
logging.AddConsole()
```

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
loggerFactory.AddConsole()
```

[AddConsole 多載](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions)可讓您傳入最低記錄層級、篩選函式，以及指出是否支援範圍的布林值。 另一個選項是傳入可指定範圍支援和記錄層級的 `IConfiguration` 物件。 

如果您考慮在生產環境中使用主控台提供者，請注意它對效能有重大影響。

當您在 Visual Studio 中建立新的專案時，`AddConsole` 方法看起來像這樣：

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

此程式碼會參考 *appSettings.json* 檔案的 `Logging` 區段：

[!code-json[](index/sample//appsettings.json)]

上述設定會將架構記錄限制為警告，同時讓應用程式在偵錯層級記錄，如[記錄篩選](#log-filtering)一節中所述。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

* * *
<a id="debug"></a>
### <a name="the-debug-provider"></a>偵錯提供者

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。

在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug 多載](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions)可讓您傳入最低記錄層級或篩選函式。

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource 提供者

針對以 ASP.NET Core 1.1.0 或更高版本為目標的應用程式，[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 提供者套件可以實作事件追蹤。 在 Windows 上，它會使用 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。 提供者可以跨平台，但目前沒有適用於 Linux 或 macOS 的事件收集和顯示工具。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

收集及檢視記錄的一個好方法是使用 [PerfView 公用程式](https://www.microsoft.com/download/details.aspx?id=28567)。 此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET 所發出之 ETW 事件的最佳體驗。 

若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者] 清單 (請勿遺漏字串開頭的星號)。

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Windows EventLog 提供者

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog 多載](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions)可讓您傳入 `EventLogSettings` 或最低記錄層級。

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource 提供者

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) 程式庫和提供者。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource 多載](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions)可讓您傳入來源參數和追蹤接聽項。

若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。 此提供者可讓您將訊息路由傳送至各種不同的[接聽項](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr)。

下列範例會設定 `TraceSource` 提供者，將 `Warning` 和更高層級訊息記錄至主控台視窗。

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Azure App Service 提供者

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。 此提供者僅適用於以 ASP.NET Core 1.1.0 或更高版本為目標的應用程式。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若是以 .NET Core 作為目標，即無須安裝提供者套件或明確地呼叫 `AddAzureWebAppDiagnostics`。 當您將應用程式部署至 Azure App Service 時，此提供者就會自動提供給應用程式。

若是以 .NET Core 作為目標，請將提供者套件新增至專案，然後叫用 `AddAzureWebAppDiagnostics`：

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics` 多載可讓您傳入 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)，您可以用它來覆寫預設值，例如記錄輸出範本、Blob 名稱和檔案大小限制 (「輸出範本」是套用至呼叫 `ILogger` 方法時所提供記錄上方之所有記錄的訊息範本)。

---

當您部署至 App Service 應用程式時，您的應用程式會接受 Azure 入口網站的 [App Service] 頁面之[診斷記錄](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)區段中的設定。 當您變更這些設定時，所做的變更會立即生效，而不需要您重新啟動應用程式或對其重新部署程式碼。 

![Azure 記錄設定](index/_static/azure-logging-settings.png)

記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。 預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。 預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。 如需預設行為的詳細資訊，請參閱 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)。

此提供者僅適用於您的專案在 Azure 環境中執行的情況。 當您在本機執行時，不會有任何作用 &mdash; 它不會寫入本機檔案或本機開發 Blob 儲存體。

## <a name="third-party-logging-providers"></a>協力廠商記錄提供者

以下是可搭配 ASP.NET Core 使用的一些協力廠商記錄架構：

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - Elmah.Io 服務的提供者

* [JSNLog](http://jsnlog.com) - 在您的伺服器端記錄中記錄 JavaScript 例外狀況和其他用戶端事件。

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - Loggr 服務的提供者

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) - NLog 程式庫的提供者

* [Serilog](https://github.com/serilog/serilog-extensions-logging) - Serilog 程式庫的提供者

某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。

使用協力廠商架構類似於使用其中一個內建提供者：將 NuGet 套件新增至您的專案，然後在 `ILoggerFactory` 上呼叫擴充方法。 如需詳細資訊，請參閱每個架構的文件。

您也可以建立自己的自訂提供者，以支援其他記錄架構或您自己的記錄需求。

## <a name="azure-log-streaming"></a>Azure 記錄資料流

Azure 記錄資料流可讓您即時檢視來自下列位置的記錄活動： 

* 應用程式伺服器 
* 網頁伺服器
* 失敗的要求追蹤 

若要設定 Azure 記錄資料流： 

* 從您應用程式的入口網站頁面巡覽至 [診斷記錄]
* 將 [應用程式記錄 (檔案系統)] 設定為 [開啟]。 

![Azure 入口網站的 [診斷記錄] 頁面](index/_static/azure-diagnostic-logs.png)

巡覽至 [記錄資料流] 頁面以檢視應用程式訊息。 這些是應用程式透過 `ILogger` 介面記錄的訊息。 

![Azure 入口網站應用程式的 [記錄資料流]](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>另請參閱

[使用 LoggerMessage 進行高效能記錄](xref:fundamentals/logging/loggermessage)
