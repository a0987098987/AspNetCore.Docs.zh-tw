---
title: "在 ASP.NET Core LoggerMessage 高效能記錄"
author: guardrex
description: "了解如何使用 LoggerMessage 功能來建立可快取的高效能記錄案例需要較少的物件配置比記錄器擴充方法的委派。"
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>在 ASP.NET Core LoggerMessage 高效能記錄

作者：[Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage)功能建立快取的委派，需要較少的物件配置並減少計算的工作負擔超過[記錄器擴充方法](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions)，例如`LogInformation`， `LogDebug`，和`LogError`. 高效能記錄的情況下，使用`LoggerMessage`模式。

`LoggerMessage`提供的下列效能優勢記錄器擴充方法：

* 記錄器擴充方法必須將 「 boxing 」 （轉換） 實值類型，例如`int`，到`object`。 `LoggerMessage`模式可避免 boxing 使用靜態`Action`欄位和強型別參數的擴充方法。
* 記錄器擴充方法必須剖析訊息範本 （具名的格式字串），每次寫入一則記錄訊息。 `LoggerMessage`只需要剖析樣板，定義訊息時。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例應用程式示範`LoggerMessage`功能追蹤系統的基本引號。 新增應用程式，並刪除使用記憶體中資料庫的引號。 當這些作業發生時，記錄檔訊息使用產生的`LoggerMessage`模式。

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[定義 (LogLevel，EventId，String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define)建立`Action`委派記錄訊息。 `Define`多載允許最多六個型別參數傳遞至具名的格式字串 （範本）。

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)建立`Func`委派定義[記錄範圍](xref:fundamentals/logging/index#log-scopes)。 `DefineScope`多載允許最多三個類型參數傳遞至具名的格式字串 （範本）。

## <a name="message-template-named-format-string"></a>訊息範本 （名為格式字串）

若要提供的字串`Define`和`DefineScope`methods 是範本，而不是插補的字串。 預留位置會填入所指定類型的順序。 在範本之間應該清楚也一致範本中的預留位置名稱。 它們做為結構化的記錄檔資料內的屬性名稱。 我們建議[Pascal 大小寫](/dotnet/standard/design-guidelines/capitalization-conventions)預留位置名稱。 例如， `{Count}`， `{FirstName}`。

## <a name="implementing-loggermessagedefine"></a>實作 LoggerMessage.Define

每個記錄檔訊息是`Action`中所建立的靜態欄位保存`LoggerMessage.Define`。 例如，範例應用程式會建立描述之 GET 要求的索引頁的記錄檔訊息的欄位 (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

如`Action`，指定：

* 記錄層級。
* 唯一的事件識別元 ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) 與靜態的擴充方法的名稱。
* 訊息範本 （名為格式字串）。 

範例應用程式集的索引頁面的要求:

* 記錄層級來`Information`。
* 事件識別碼`1`名稱`IndexPageRequested`方法。
* 訊息範本 （名為格式字串） 的字串。

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

結構化的記錄存放區提供更豐富記錄事件識別碼時，可以使用事件名稱。 例如， [Serilog](https://github.com/serilog/serilog-extensions-logging)會使用事件名稱。

`Action`透過強型別擴充方法會叫用。 `IndexPageRequested`方法針對索引頁面 GET 要求的訊息記錄範例應用程式：

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`在記錄器上呼叫`OnGetAsync`方法中的*Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

檢查應用程式的主控台輸出：

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

建立靜態欄位時，將參數傳遞給記錄檔訊息，定義最多六個型別。 藉由定義加入引號時，範例應用程式記錄檔字串`string`輸入`Action`欄位：

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

委派的記錄檔訊息範本所提供的類型從接收其預留位置值。 範例應用程式定義的委派加入其中的引號參數是引號`string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

靜態的擴充方法給加入引號， `QuoteAdded`、 接收報價引數值，並將其傳遞給`Action`委派：

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

在索引頁面的程式碼後置檔案中 (*Pages/Index.cshtml.cs*)，`QuoteAdded`呼叫來記錄訊息：

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

檢查應用程式的主控台輸出：

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

範例應用程式會實作`try` &ndash; `catch`引號刪除的模式。 告知性訊息會記錄成功的刪除作業。 擲回例外狀況時，會刪除作業的記錄一則錯誤訊息。 記錄檔訊息不成功的刪除作業包含例外狀況堆疊追蹤 (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

請注意如何傳遞至委派中的例外狀況`QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

索引頁面的程式碼後置中，呼叫成功引號刪除`QuoteDeleted`記錄器上的方法。 當找不到刪除的報價`ArgumentNullException`就會擲回。 例外狀況會受困於連結`try` &ndash; `catch`陳述式和呼叫記錄`QuoteDeleteFailed`方法中記錄器`catch`區塊 (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

已成功刪除引號時，檢查應用程式的主控台輸出：

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

引號刪除失敗時，檢查應用程式的主控台輸出。 請注意，例外狀況會包含在記錄檔訊息：

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a>實作 LoggerMessage.DefineScope

定義[記錄範圍](xref:fundamentals/logging/index#log-scopes)要套用至一系列的記錄檔訊息使用[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)方法。

範例應用程式具有**全部清除**刪除所有的資料庫中引號的按鈕。 刪除引號的移除其中一次。 刪除引號時，每次`QuoteDeleted`記錄器上呼叫方法。 記錄範圍會加入至這些記錄訊息。

啟用`IncludeScopes`中主控台記錄器選項：

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

設定`IncludeScopes`，才能在 ASP.NET Core 2.0 應用程式中啟用記錄範圍。 設定`IncludeScopes`透過*appsettings*組態檔是沒有規劃 ASP.NET Core 2.1 版本的功能。

範例應用程式清除其他提供者，並加入篩選，來減少記錄輸出。 這可讓您更輕鬆地查看示範的範例記錄檔訊息`LoggerMessage`功能。

若要建立記錄檔的範圍內，新增欄位以保留`Func`委派領域。 範例應用程式會建立名為的欄位`_allQuotesDeletedScope`(*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

使用`DefineScope`來建立委派。 最多三種類型可以指定用於當做範本引數時叫用委派。 範例應用程式會使用包含已刪除的引號數目的訊息範本 (`int`類型):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

提供記錄檔訊息的靜態的擴充方法。 訊息範本中包含任何出現的具名屬性的型別參數。 範例應用程式會在`count`引號刪除，然後傳回`_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

記錄擴充功能呼叫在這個範圍會包裝`using`區塊：

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

檢查記錄檔中的訊息應用程式的主控台輸出。 下列的結果會顯示刪除的記錄範圍訊息的三個引號：

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a>請參閱

* [記錄](xref:fundamentals/logging/index)
