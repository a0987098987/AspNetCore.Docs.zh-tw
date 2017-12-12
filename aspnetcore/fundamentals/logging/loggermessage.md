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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="798b7-103">在 ASP.NET Core LoggerMessage 高效能記錄</span><span class="sxs-lookup"><span data-stu-id="798b7-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="798b7-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="798b7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="798b7-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage)功能建立快取的委派，需要較少的物件配置並減少計算的工作負擔超過[記錄器擴充方法](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions)，例如`LogInformation`， `LogDebug`，和`LogError`.</span><span class="sxs-lookup"><span data-stu-id="798b7-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="798b7-106">高效能記錄的情況下，使用`LoggerMessage`模式。</span><span class="sxs-lookup"><span data-stu-id="798b7-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="798b7-107">`LoggerMessage`提供的下列效能優勢記錄器擴充方法：</span><span class="sxs-lookup"><span data-stu-id="798b7-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="798b7-108">記錄器擴充方法必須將 「 boxing 」 （轉換） 實值類型，例如`int`，到`object`。</span><span class="sxs-lookup"><span data-stu-id="798b7-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="798b7-109">`LoggerMessage`模式可避免 boxing 使用靜態`Action`欄位和強型別參數的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="798b7-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="798b7-110">記錄器擴充方法必須剖析訊息範本 （具名的格式字串），每次寫入一則記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="798b7-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="798b7-111">`LoggerMessage`只需要剖析樣板，定義訊息時。</span><span class="sxs-lookup"><span data-stu-id="798b7-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="798b7-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="798b7-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="798b7-113">範例應用程式示範`LoggerMessage`功能追蹤系統的基本引號。</span><span class="sxs-lookup"><span data-stu-id="798b7-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="798b7-114">新增應用程式，並刪除使用記憶體中資料庫的引號。</span><span class="sxs-lookup"><span data-stu-id="798b7-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="798b7-115">當這些作業發生時，記錄檔訊息使用產生的`LoggerMessage`模式。</span><span class="sxs-lookup"><span data-stu-id="798b7-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="798b7-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="798b7-116">LoggerMessage.Define</span></span>

<span data-ttu-id="798b7-117">[定義 (LogLevel，EventId，String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define)建立`Action`委派記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="798b7-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="798b7-118">`Define`多載允許最多六個型別參數傳遞至具名的格式字串 （範本）。</span><span class="sxs-lookup"><span data-stu-id="798b7-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="798b7-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="798b7-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="798b7-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)建立`Func`委派定義[記錄範圍](xref:fundamentals/logging/index#log-scopes)。</span><span class="sxs-lookup"><span data-stu-id="798b7-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="798b7-121">`DefineScope`多載允許最多三個類型參數傳遞至具名的格式字串 （範本）。</span><span class="sxs-lookup"><span data-stu-id="798b7-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="798b7-122">訊息範本 （名為格式字串）</span><span class="sxs-lookup"><span data-stu-id="798b7-122">Message template (named format string)</span></span>

<span data-ttu-id="798b7-123">若要提供的字串`Define`和`DefineScope`methods 是範本，而不是插補的字串。</span><span class="sxs-lookup"><span data-stu-id="798b7-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="798b7-124">預留位置會填入所指定類型的順序。</span><span class="sxs-lookup"><span data-stu-id="798b7-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="798b7-125">在範本之間應該清楚也一致範本中的預留位置名稱。</span><span class="sxs-lookup"><span data-stu-id="798b7-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="798b7-126">它們做為結構化的記錄檔資料內的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="798b7-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="798b7-127">我們建議[Pascal 大小寫](/dotnet/standard/design-guidelines/capitalization-conventions)預留位置名稱。</span><span class="sxs-lookup"><span data-stu-id="798b7-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="798b7-128">例如， `{Count}`， `{FirstName}`。</span><span class="sxs-lookup"><span data-stu-id="798b7-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="798b7-129">實作 LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="798b7-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="798b7-130">每個記錄檔訊息是`Action`中所建立的靜態欄位保存`LoggerMessage.Define`。</span><span class="sxs-lookup"><span data-stu-id="798b7-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="798b7-131">例如，範例應用程式會建立描述之 GET 要求的索引頁的記錄檔訊息的欄位 (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="798b7-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="798b7-132">如`Action`，指定：</span><span class="sxs-lookup"><span data-stu-id="798b7-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="798b7-133">記錄層級。</span><span class="sxs-lookup"><span data-stu-id="798b7-133">The log level.</span></span>
* <span data-ttu-id="798b7-134">唯一的事件識別元 ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) 與靜態的擴充方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="798b7-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="798b7-135">訊息範本 （名為格式字串）。</span><span class="sxs-lookup"><span data-stu-id="798b7-135">The message template (named format string).</span></span> 

<span data-ttu-id="798b7-136">範例應用程式集的索引頁面的要求:</span><span class="sxs-lookup"><span data-stu-id="798b7-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="798b7-137">記錄層級來`Information`。</span><span class="sxs-lookup"><span data-stu-id="798b7-137">Log level to `Information`.</span></span>
* <span data-ttu-id="798b7-138">事件識別碼`1`名稱`IndexPageRequested`方法。</span><span class="sxs-lookup"><span data-stu-id="798b7-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="798b7-139">訊息範本 （名為格式字串） 的字串。</span><span class="sxs-lookup"><span data-stu-id="798b7-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="798b7-140">結構化的記錄存放區提供更豐富記錄事件識別碼時，可以使用事件名稱。</span><span class="sxs-lookup"><span data-stu-id="798b7-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="798b7-141">例如， [Serilog](https://github.com/serilog/serilog-extensions-logging)會使用事件名稱。</span><span class="sxs-lookup"><span data-stu-id="798b7-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="798b7-142">`Action`透過強型別擴充方法會叫用。</span><span class="sxs-lookup"><span data-stu-id="798b7-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="798b7-143">`IndexPageRequested`方法針對索引頁面 GET 要求的訊息記錄範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="798b7-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="798b7-144">`IndexPageRequested`在記錄器上呼叫`OnGetAsync`方法中的*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="798b7-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="798b7-145">檢查應用程式的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="798b7-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="798b7-146">建立靜態欄位時，將參數傳遞給記錄檔訊息，定義最多六個型別。</span><span class="sxs-lookup"><span data-stu-id="798b7-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="798b7-147">藉由定義加入引號時，範例應用程式記錄檔字串`string`輸入`Action`欄位：</span><span class="sxs-lookup"><span data-stu-id="798b7-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="798b7-148">委派的記錄檔訊息範本所提供的類型從接收其預留位置值。</span><span class="sxs-lookup"><span data-stu-id="798b7-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="798b7-149">範例應用程式定義的委派加入其中的引號參數是引號`string`:</span><span class="sxs-lookup"><span data-stu-id="798b7-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="798b7-150">靜態的擴充方法給加入引號， `QuoteAdded`、 接收報價引數值，並將其傳遞給`Action`委派：</span><span class="sxs-lookup"><span data-stu-id="798b7-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="798b7-151">在索引頁面的程式碼後置檔案中 (*Pages/Index.cshtml.cs*)，`QuoteAdded`呼叫來記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="798b7-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="798b7-152">檢查應用程式的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="798b7-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="798b7-153">範例應用程式會實作`try` &ndash; `catch`引號刪除的模式。</span><span class="sxs-lookup"><span data-stu-id="798b7-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="798b7-154">告知性訊息會記錄成功的刪除作業。</span><span class="sxs-lookup"><span data-stu-id="798b7-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="798b7-155">擲回例外狀況時，會刪除作業的記錄一則錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="798b7-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="798b7-156">記錄檔訊息不成功的刪除作業包含例外狀況堆疊追蹤 (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="798b7-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="798b7-157">請注意如何傳遞至委派中的例外狀況`QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="798b7-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="798b7-158">索引頁面的程式碼後置中，呼叫成功引號刪除`QuoteDeleted`記錄器上的方法。</span><span class="sxs-lookup"><span data-stu-id="798b7-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="798b7-159">當找不到刪除的報價`ArgumentNullException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="798b7-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="798b7-160">例外狀況會受困於連結`try` &ndash; `catch`陳述式和呼叫記錄`QuoteDeleteFailed`方法中記錄器`catch`區塊 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="798b7-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="798b7-161">已成功刪除引號時，檢查應用程式的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="798b7-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="798b7-162">引號刪除失敗時，檢查應用程式的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="798b7-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="798b7-163">請注意，例外狀況會包含在記錄檔訊息：</span><span class="sxs-lookup"><span data-stu-id="798b7-163">Note that the exception is included in the log message:</span></span>

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

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="798b7-164">實作 LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="798b7-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="798b7-165">定義[記錄範圍](xref:fundamentals/logging/index#log-scopes)要套用至一系列的記錄檔訊息使用[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope)方法。</span><span class="sxs-lookup"><span data-stu-id="798b7-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="798b7-166">範例應用程式具有**全部清除**刪除所有的資料庫中引號的按鈕。</span><span class="sxs-lookup"><span data-stu-id="798b7-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="798b7-167">刪除引號的移除其中一次。</span><span class="sxs-lookup"><span data-stu-id="798b7-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="798b7-168">刪除引號時，每次`QuoteDeleted`記錄器上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="798b7-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="798b7-169">記錄範圍會加入至這些記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="798b7-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="798b7-170">啟用`IncludeScopes`中主控台記錄器選項：</span><span class="sxs-lookup"><span data-stu-id="798b7-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="798b7-171">設定`IncludeScopes`，才能在 ASP.NET Core 2.0 應用程式中啟用記錄範圍。</span><span class="sxs-lookup"><span data-stu-id="798b7-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="798b7-172">設定`IncludeScopes`透過*appsettings*組態檔是沒有規劃 ASP.NET Core 2.1 版本的功能。</span><span class="sxs-lookup"><span data-stu-id="798b7-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="798b7-173">範例應用程式清除其他提供者，並加入篩選，來減少記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="798b7-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="798b7-174">這可讓您更輕鬆地查看示範的範例記錄檔訊息`LoggerMessage`功能。</span><span class="sxs-lookup"><span data-stu-id="798b7-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="798b7-175">若要建立記錄檔的範圍內，新增欄位以保留`Func`委派領域。</span><span class="sxs-lookup"><span data-stu-id="798b7-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="798b7-176">範例應用程式會建立名為的欄位`_allQuotesDeletedScope`(*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="798b7-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="798b7-177">使用`DefineScope`來建立委派。</span><span class="sxs-lookup"><span data-stu-id="798b7-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="798b7-178">最多三種類型可以指定用於當做範本引數時叫用委派。</span><span class="sxs-lookup"><span data-stu-id="798b7-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="798b7-179">範例應用程式會使用包含已刪除的引號數目的訊息範本 (`int`類型):</span><span class="sxs-lookup"><span data-stu-id="798b7-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="798b7-180">提供記錄檔訊息的靜態的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="798b7-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="798b7-181">訊息範本中包含任何出現的具名屬性的型別參數。</span><span class="sxs-lookup"><span data-stu-id="798b7-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="798b7-182">範例應用程式會在`count`引號刪除，然後傳回`_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="798b7-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="798b7-183">記錄擴充功能呼叫在這個範圍會包裝`using`區塊：</span><span class="sxs-lookup"><span data-stu-id="798b7-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="798b7-184">檢查記錄檔中的訊息應用程式的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="798b7-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="798b7-185">下列的結果會顯示刪除的記錄範圍訊息的三個引號：</span><span class="sxs-lookup"><span data-stu-id="798b7-185">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="798b7-186">請參閱</span><span class="sxs-lookup"><span data-stu-id="798b7-186">See also</span></span>

* [<span data-ttu-id="798b7-187">記錄</span><span class="sxs-lookup"><span data-stu-id="798b7-187">Logging</span></span>](xref:fundamentals/logging/index)
