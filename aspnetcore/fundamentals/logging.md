---
title: "在 ASP.NET Core 記錄"
author: ardalis
description: "深入了解 ASP.NET Core 的記錄架構。 探索的內建的記錄提供者，並深入了解受歡迎的協力廠商提供者。"
keywords: "ASP.NET Core 記錄、 記錄 providers,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,scopes"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2e991ea37b1b726e472d78d839143546ebd559f
ms.sourcegitcommit: 29da58de11e20c9c60448e36e7075c6b13622624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>若要登入 ASP.NET Core 簡介

由[Steve Smith](https://ardalis.com/)和[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 支援可搭配各種記錄提供者的記錄 API。 內建提供者可讓您將記錄檔傳送至一個或多個目的地，而您可以插入的第三方記錄架構。 本文示範如何在程式碼中使用的內建的記錄 API 和提供者。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>如何建立記錄檔

若要建立記錄檔，取得`ILogger`物件從[相依性插入](dependency-injection.md)容器：

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

然後呼叫該記錄器物件記錄方法：

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

這個範例會建立記錄檔與`TodoController`類別做為*類別*。  類別會說明[本文稍後](#log-category)。

ASP.NET Core 不會因為不提供非同步記錄器方法應該快速，因此並不需要使用非同步的成本，記錄。 如果您在其中，則不成立的情況下，請考慮變更您登入的方式。  如果您的資料存放區緩慢時，首先，快速存放區寫入記錄檔訊息，然後將它們移至低速的存放區。 例如，記錄至訊息佇列讀取及保存到慢的儲存體，另一個處理序。

## <a name="how-to-add-providers"></a>如何新增提供者

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

記錄提供者會使用您建立的訊息`ILogger`物件，並顯示或將其儲存。 例如，主控台提供者會在主控台中，顯示訊息，而 Azure 應用程式服務提供者可以將它們儲存在 Azure blob 儲存體。

若要使用的提供者，呼叫者`Add<ProviderName>`中的擴充方法*Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

記錄的方式，請參閱上述的程式碼中設定的預設專案範本，但`ConfigureLogging`呼叫由`CreateDefaultBuilder`方法。 下列程式碼*Program.cs* ，它由專案範本：

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

記錄提供者會使用您建立的訊息`ILogger`物件，並顯示或將其儲存。 例如，主控台提供者會在主控台中，顯示訊息，而 Azure 應用程式服務提供者可以將它們儲存在 Azure blob 儲存體。

若要使用的提供者，安裝 NuGet 封裝和提供者的擴充方法呼叫的執行個體上`ILoggerFactory`，如下列範例所示。

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core[相依性插入](dependency-injection.md)(DI) 提供`ILoggerFactory`執行個體。 `AddConsole`和`AddDebug`擴充方法定義在[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)和[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)封裝。 每個擴充方法呼叫`ILoggerFactory.AddProvider`方法，傳入的提供者執行個體。 

> [!NOTE]
> 這個發行項的範例應用程式將記錄提供者中的`Configure`方法`Startup`類別。 如果您想要從先前執行的程式碼中取得記錄檔輸出，加入記錄提供者中的`Startup`類別建構函式。 

---

您會發現每個資訊[內建的記錄提供者](#built-in-logging-providers)並連結至[協力廠商記錄提供者](#third-party-logging-providers)稍後的發行項。

## <a name="sample-logging-output"></a>記錄輸出範例

上一節中所示的範例程式碼，您會看到主控台中的記錄檔，當您從命令列執行。 主控台輸出的範例如下：

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
 
這些記錄檔所建立，請前往`http://localhost:5000/api/todo/0`，此觸發程序執行兩個`ILogger`呼叫上一節中所示。

當您執行 Visual Studio 中的範例應用程式時出現在 偵錯視窗中，以下是相同的記錄檔的範例：

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

所建立的記錄檔`ILogger`以"TodoApi.Controllers.TodoController"開頭的呼叫上一節中所示。 開頭為"Microsoft"類別的記錄檔是從 ASP.NET Core。 ASP.NET Core 本身和您的應用程式程式碼會使用相同的記錄 API 以及相同的記錄提供者。

這篇文章的其餘部分將說明一些詳細資料和記錄選項。

## <a name="nuget-packages"></a>NuGet 套件

`ILogger`和`ILoggerFactory`介面位於[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)，並為其預設實作是在[Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)。

## <a name="log-category"></a>記錄檔類別

A*類別*隨附於您所建立的每個記錄檔。  您指定的類別，當您建立`ILogger`物件。 類別可能是任何字串，但慣例是使用的類別從中寫入記錄檔的完整的名稱。  例如:"TodoApi.Controllers.TodoController"。

您可以指定為字串的類別或類別衍生自類型的擴充方法。 若要指定類別目錄做為字串，呼叫`CreateLogger`上`ILoggerFactory`執行個體，如下所示。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

大部分的情況下，會很容易使用`ILogger<T>`，如下列範例所示。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

這就相當於呼叫`CreateLogger`的完整限定的類型名稱`T`。

## <a name="log-level"></a>記錄層級

每次您將寫入記錄檔，您指定其[LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)。 記錄層級表示嚴重性或重要性的程度。  例如，您可以撰寫`Information`方法通常結束時，記錄`Warning`記錄方法會傳回 404 傳回碼，和一個`Error`時攔截預期的例外狀況記錄。

下列程式碼範例，方法的名稱 (例如， `LogWarning`) 指定的記錄層級。  第一個參數是[記錄事件識別碼](#log-event-id)（本文稍後說明）。  其餘的參數來建構記錄訊息字串。

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

方法名稱中包含的層級的記錄方法[的 ILogger 擴充方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)。 在幕後呼叫這些方法`Log`採用方法`LogLevel`參數。 您可以呼叫`Log`直接方法，而不是其中一個這些擴充方法，但語法是相當複雜。 如需詳細資訊，請參閱[ILogger 介面](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)和[記錄器延伸模組的原始程式碼](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)。

ASP.NET Core 會定義下列[記錄層級](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)，從最低到最高嚴重性此處的順序排列。

* 追蹤 = 0

  只對問題進行偵錯的開發人員有價值的資訊。 這些訊息可能包含機密的應用程式資料，因此不應啟用實際執行環境中。 *預設為停用。* 範例：`Credentials: {"User":"someuser", "Password":"P@ssword"}`

* 偵錯 = 1

  在開發和偵錯期間有短期用途的資訊。 範例：`Entering method Configure with flag set to true.`您通常不會讓`Debug`層級會記錄在生產環境中，除非您正在進行疑難排解，因為大量記錄檔。

* 資訊 = 2

  以追蹤應用程式的一般流程。 這些記錄檔通常會有一些長期的值。 範例：`Request received for path /api/todo`

* 警告 = 3

  適用於應用程式流程中的異常或非預期事件。 這些可能包含錯誤或其他條件，不會造成應用程式停止，但這可能需要進行調查。 處理的例外狀況所使用的通用位置`Warning`記錄層級。 範例：`FileNotFoundException for file quotes.txt.`

* 錯誤 = 4

  錯誤和無法處理的例外狀況。 這些訊息會指出目前的活動或作業 （例如目前的 HTTP 要求） 中的失敗，不是整個應用程式失敗。 範例記錄檔訊息：`Cannot insert record due to duplicate key violation.`

* 重大 = 5

  需要立即關注的失敗。 範例： 資料遺失情況下，磁碟空間不足。

您可以使用記錄層級來控制多少記錄輸出寫入到特定的儲存媒體，或顯示視窗。 比方說，在生產環境中您可能想要的所有記錄檔`Information`層級和較低的所有記錄檔的磁碟區資料存放區中，並移至`Warning`層級和更新版本移至 值資料存放區。 在開發期間，您可能會正常傳送記錄檔的`Warning`或更高嚴重性至主控台。 然後當您需要進行疑難排解，您可以加入`Debug`層級。 [記錄檔篩選](#log-filtering)本文中稍後的章節將說明如何控制提供者會處理的記錄層級。

ASP.NET Core framework 寫入`Debug`層級記錄檔中的 framework 事件。 記錄範例稍早在本文中排除下列記錄檔`Information`層級，所以沒有`Debug`層級的記錄檔已顯示。 以下是主控台記錄檔的範例，如果您執行範例應用程式設定為顯示`Debug`和更高版本的記錄檔，主控台提供者。

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

每次您將寫入記錄檔，您可以指定*事件識別碼*。 範例應用程式會使用本機定義`LoggingEvents`類別：

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

事件識別碼是整數值，可用來將一組記錄的事件與彼此產生關聯。 比方說，記錄檔的新增至購物車的項目可以是事件識別碼 1000年，而且完成購買的記錄檔可能是事件識別碼 1001年。

在記錄輸出可能儲存在欄位或文字訊息，根據提供者中包含的事件識別碼。  偵錯提供者不會顯示事件識別碼，但主控台提供者中會顯示這些方括號之後類別：

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>記錄檔訊息的格式字串

每次寫入記錄檔，您提供的文字訊息。 訊息字串可以包含具名的預留位置的引數的值會放置在如下列範例所示：

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

預留位置，其名稱則不順序會決定哪一個參數用它們。 例如，如果您有下列的程式碼：

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

產生的記錄檔訊息看起來會像這樣：

```
Parameter values: parm1, parm2
```

記錄架構執行訊息格式化以使記錄提供者來實作這種方式[語意的記錄，也稱為結構化記錄](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。 因為本身的引數會傳遞給記錄系統，而不只是格式化的訊息字串中，記錄提供者可以儲存的參數值做為欄位除了訊息字串。 例如，如果導向您的記錄檔輸出到 Azure 資料表儲存體，而記錄器方法呼叫看起來像這樣：

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Azure 資料表的每個實體可能會有`ID`和`RequestTime`會簡化查詢記錄資料的屬性。 您可能會發現所有記錄檔中特定`RequestTime`範圍，而無需剖析時間超出文字訊息。

## <a name="logging-exceptions"></a>記錄例外狀況

記錄器方法具有多載，可讓您傳遞例外狀況，如下列範例所示：

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

不同的提供者以不同方式處理的例外狀況資訊。 以下是從上述的程式碼的偵錯提供者輸出範例。

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>記錄檔篩選

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

您可以指定最小的記錄層級為特定的提供者、 類別目錄或所有的提供者或所有類別目錄。  下列的最低層級的任何記錄檔不被傳遞該提供者，使它們不是您取得顯示或儲存。 

如果您想要隱藏所有記錄檔，您可以指定`LogLevel.None`為最小的記錄層級。 整數值`LogLevel.None`是 6，這是高於`LogLevel.Critical`(5)。

**在組態中建立篩選器規則**

專案範本建立程式碼，呼叫`CreateDefaultBuilder`設定主控台和偵錯提供者的記錄。 `CreateDefaultBuilder`方法也可以設定記錄設定中尋找`Logging`區段中，使用程式碼如下所示：

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

設定資料指定最小的記錄層級提供者及類別，如下列範例所示：

[!code-json[](logging/sample2/appsettings.json)]

此 JSON 建立六個篩選規則，一個用於偵錯提供者，四個主控台提供者，以及適用於所有提供者。 您會看到如何只是其中一個這些規則會選擇每個提供者時`ILogger`建立物件。

**在程式碼中的篩選規則**

下列範例所示，您可以在程式碼，註冊篩選規則：

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

第二個`AddFilter`使用型別名稱來指定偵錯提供者。 第一個`AddFilter`適用於所有提供者，因為它不會指定提供者類型。

**篩選規則的方式套用**

設定資料和`AddFilter`上述範例所示的程式碼會建立下表中所顯示的規則。 前六個來自組態範例，而且最後兩個會從程式碼範例。

數字|提供者|開頭的類別|最小的記錄層級|
------|--------|--------------------------|-----------------|
1|偵錯|所有類別|資訊|
2|主控台|Microsoft.AspNetCore.Mvc.Razor.Internal|警告|
3|主控台|Microsoft.AspNetCore.Mvc.Razor.Razor|偵錯|
4|主控台|Microsoft.AspNetCore.Mvc.Razor|錯誤|
5|主控台|所有類別|資訊|
6|所有提供者|所有類別|偵錯
7|所有提供者|系統|偵錯
8|偵錯|Microsoft|追蹤

當您建立`ILogger`物件寫入記錄檔，`ILoggerFactory`物件會選取每個提供者来套用至該記錄器的單一規則。 所有訊息的寫入`ILogger`會根據選取的規則篩選物件。 從可用的規則，會選取每個提供者] 和 [類別目錄配對可能最適合的規則。

每個提供者會使用下列演算法時`ILogger`建立指定類別目錄：

* 選取符合提供者或其別名的所有規則。  如果找不到，請以空的提供者選取所有規則。
* 從上一個步驟的結果，具有最長的選取規則比對類別前置詞。 如果找不到，請選取 不指定類別的所有規則。
* 如果選取多個規則採取**最後**其中一個。
* 如果未不選取任何規則，則使用`MinimumLevel`。
 
例如，假設您有上述規則的清單，而且您建立`ILogger`"Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine"類別的物件：

* 偵錯提供者，將套用規則 1、 6 和 8。 規則 8 是最特定的因此這是所選取。
* 主控台提供者，將套用規則 3、 4、 5 和 6。 規則 3 是最適合。

當您建立的記錄檔`ILogger`為類別目錄 」 Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine 」 記錄的`Trace`層級和更新版本將會移至的偵錯提供者和記錄檔`Debug`層級和更新版本將會移至主控台提供者。

**別名提供者**

您可以在組態中，指定提供者使用的型別名稱，但每個提供者會定義短*別名*，比較容易使用。 內建的提供者，使用下列別名：

- 主控台
- 偵錯
- 事件記錄檔
- AzureAppServices
- TraceSource
- EventSource

**預設最小層級**

沒有任何組態或程式碼的規則適用於給定的提供者和類別目錄時，才會生效的最小層級設定。 下列範例會示範如何設定的最低層級：

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

如果您未明確設定的最低層級，預設值是`Information`，這表示`Trace`和`Debug`記錄檔會被忽略。

**篩選函數**

您可以撰寫程式碼中套用的篩選規則的篩選函數。 Filter 函式會叫用的所有提供者和沒有規則指派它們的組態或程式碼的類別。 函式中的程式碼可存取提供者類型、 類別和記錄層級，決定應該記錄的訊息。 例如: 

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

某些記錄提供者可讓您指定記錄檔時應該寫入儲存媒體或忽略根據記錄層級和類別目錄。

`AddConsole`和`AddDebug`擴充方法提供多載，可讓您在篩選條件中傳遞。 下列範例程式碼會導致略過下列記錄檔的主控台提供者`Warning`層級，而偵錯提供者會忽略 framework 建立的記錄檔。

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

`AddEventLog`方法具有多載採用`EventLogSettings`執行個體，其中可能包含篩選的函式，在其`Filter`屬性。 TraceSource 提供者未提供任何多載，因為其記錄層級和其他參數會根據`SourceSwitch`和`TraceListener`它會使用。

您可以設定所有的提供者註冊的篩選規則`ILoggerFactory`所使用的執行個體`WithFilter`擴充方法。 下列範例限制 framework 記錄檔 （類別開頭為 「 Microsoft 」 或 「 系統 」），以警告時讓偵錯層級的應用程式記錄檔。

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

如果您想要使用篩選來防止所有記錄檔寫入特定分類，您可以指定`LogLevel.None`為該分類的最小的記錄層級。 整數值`LogLevel.None`是 6，這是高於`LogLevel.Critical`(5)。

`WithFilter`擴充方法由[Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 封裝。 方法會傳回新`ILoggerFactory`會篩選傳遞記錄器的所有提供者的記錄檔訊息的執行個體已向它註冊。 不會影響任何其他`ILoggerFactory`執行個體，包括原始`ILoggerFactory`執行個體。

---

## <a name="log-scopes"></a>記錄檔的範圍

您可以分組一組內的邏輯作業*範圍*以相同的資料附加到建立該集一部分的每個記錄檔。  例如，您可能想要包含交易識別碼。 交易的處理過程中建立每個記錄檔

領域是`IDisposable`所傳回的型別`ILogger.BeginScope<TState>`方法，直到它已處置的持續時間。 您所包裝您記錄器呼叫中使用範圍`using`封鎖，如下所示：

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

下列程式碼可讓主控台提供者的範圍：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

在*Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在*Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

每個記錄檔訊息包含已設定領域的資訊：

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>內建的記錄提供者

ASP.NET Core 隨附下列提供者：

* [Console](#console)
* [偵錯](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>主控台提供者

[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)封裝提供者會傳送記錄檔輸出到主控台。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)可讓您傳入的最小的記錄層級、 篩選函數和布林值，指出是否支援範圍。  另一個選項是傳入`IConfiguration`物件，可以指定範圍的支援和記錄層級。 

如果您考慮在生產環境中使用的主控台提供者，請注意它對效能有重大影響。

當您在 Visual Studio 中，建立新的專案`AddConsole`方法看起來像這樣：

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

此程式碼是指`Logging`區段*appSettings.json*檔案：

[!code-json[](logging/sample//appsettings.json)]

顯示限制 framework 警告記錄檔，同時允許應用程式的設定，以中所述，在偵錯層級，記錄[記錄檔篩選](#log-filtering)> 一節。 如需詳細資訊，請參閱[組態](configuration.md)。

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>偵錯提供者

[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)封裝提供者會將記錄輸出寫入使用[System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug)類別 (`Debug.WriteLine`方法呼叫)。

On Linux，此提供者寫入記錄檔以*/var/log/message*。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)可讓您在最小的記錄層級或篩選函數中傳遞。

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource 的提供者

以目標 ASP.NET Core 1.1.0 應用程式或更新版本， [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)封裝提供者可以實作事件追蹤。 在 Windows 中，它會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。 提供者是跨平台，但不有任何事件尚未 for Linux 或 macOS 收集及顯示的工具。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

收集並檢視記錄檔的好方法是使用[PerfView 公用程式](https://www.microsoft.com/download/details.aspx?id=28567)。 有一些其他工具來檢視 ETW 記錄檔，但 PerfView 提供最佳的經驗，使用 asp.net 所發出的 ETW 事件。 

若要設定 PerfView 收集此提供者所記錄的事件，將字串加入`*Microsoft-Extensions-Logging`至**額外的提供者**清單。 （不會錯過星號開頭的字串。）

![Perfview 額外的提供者](logging/_static/perfview-additional-providers.png)

擷取在 Nano Server 上的事件需要一些額外的設定：

* 連接至 Nano Server 的 PowerShell 遠端執行功能：

  ```powershell
  Enter-PSSession [name]
  ```

* 建立 ETW 工作階段：

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* 新增 ETW 提供者[CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers)，ASP.NET Core 和其他人所需。 ASP.NET Core 提供者 GUID 是`3ac73b97-af73-50e9-0822-5da4367920d0`。 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* 執行站台，並執行您要的追蹤資訊的任何動作。

* 停止追蹤工作階段完成時：

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

產生*C:\trace.etl*可以在其他版本的 Windows 上使用 PerfView 分析檔案。

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Windows 事件記錄檔提供者

[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)封裝提供者記錄檔會將輸出傳送至 Windows 事件記錄檔。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)可讓您傳入`EventLogSettings`或最小的記錄層級。

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource 提供者

[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource)封裝提供者使用[System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource)程式庫和提供者。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource 多載](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions)可讓您傳入來源參數和追蹤接聽項。

若要使用此提供者，應用程式必須在.NET Framework （而非.NET Core） 上執行。 此提供者可讓您將訊息傳送至各種不同的[接聽程式](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners)，例如[TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr)範例應用程式中使用。

下列範例會設定`TraceSource`記錄提供者`Warning`和更高的訊息至主控台視窗。

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Azure 應用程式服務提供者

[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices)封裝提供者會將記錄寫入 Azure App Service 應用程式的檔案系統中，文字檔[blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)Azure 儲存體帳戶中。 僅適用於 ASP.NET Core 1.1.0 為目標的應用程式或更高版本的提供者。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

您不需要安裝之提供者的封裝或呼叫`AddAzureWebAppDiagnostics`擴充方法。  當您將應用程式部署至 Azure App Service 自動提供給您的應用程式提供者。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

`AddAzureWebAppDiagnostics`多載可讓您傳入[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)，與您可以覆寫預設設定，例如記錄輸出範本、 blob 名稱和檔案大小限制。 (*輸出範本*是訊息的格式字串套用至所有的記錄檔，在您提供當您呼叫的一個之上`ILogger`方法。)  

---

當您部署 App Service 應用程式時，您的應用程式會接受中的設定[診斷記錄檔](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)區段**App Service**在 Azure 入口網站頁面。 當您變更這些設定時，所做的變更會立即生效而不需要您重新啟動應用程式，或重新部署它的程式碼。 

![Azure 記錄設定](logging/_static/azure-logging-settings.png)

記錄檔的預設位置是在*d:\\家用\\LogFiles\\應用程式*資料夾和預設檔案名稱是*診斷 yyyymmdd.txt*。 預設檔案大小限制為 10 MB，並預設的保留的檔案數目上限為 2。 預設的 blob 名稱是*{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。 如需有關預設行為的詳細資訊，請參閱[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)。

在 Azure 環境中執行您的專案時，只適用於此提供者。  它沒有任何作用，當您在本機執行&mdash;它不會寫入本機檔案或 blob 的本機開發儲存體。

## <a name="third-party-logging-providers"></a>第三方記錄提供者

以下是使用 ASP.NET Core 某些協力廠商記錄架構：

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -Elmah.Io 服務提供者

* [JSNLog](http://jsnlog.com) -您的伺服器端記錄檔中記錄的 JavaScript 例外狀況和其他用戶端事件。

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -Loggr 服務提供者

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -NLog 程式庫提供者

* [Serilog](https://github.com/serilog/serilog-extensions-logging) -Serilog 程式庫提供者

可以執行一些協力廠商架構[語意的記錄，也稱為結構化記錄](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。

使用協力廠商架構，類似於使用其中一個內建的提供者： 將 NuGet 封裝加入至您的專案和擴充方法呼叫上`ILoggerFactory`。 如需詳細資訊，請參閱每個架構的文件。

您可以建立您自己的自訂提供者，以支援其他的記錄架構或您自己的記錄需求。
