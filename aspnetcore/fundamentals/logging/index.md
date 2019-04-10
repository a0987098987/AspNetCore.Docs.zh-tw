---
title: ASP.NET Core 中的記錄
author: tdykstra
description: 了解 ASP.NET Core 中的記錄架構。 探索內建記錄提供者，並深入了解熱門協力廠商提供者。
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 065b2016d3a2dcc2243ec6869e027c5fabe4dad8
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068400"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="0bec6-104">ASP.NET Core 中的記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="0bec6-105">作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0bec6-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0bec6-106">ASP.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="0bec6-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="0bec6-107">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="0bec6-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="0bec6-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0bec6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="0bec6-109">新增提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-109">Add providers</span></span>

<span data-ttu-id="0bec6-110">記錄提供者會顯示或儲存記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="0bec6-111">例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="0bec6-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="0bec6-112">您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。</span><span class="sxs-lookup"><span data-stu-id="0bec6-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0bec6-113">若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="0bec6-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="0bec6-114">預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="0bec6-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="0bec6-115">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-115">Console</span></span>
* <span data-ttu-id="0bec6-116">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-116">Debug</span></span>
* <span data-ttu-id="0bec6-117">EventSource (從 ASP.NET Core 2.2 開始)</span><span class="sxs-lookup"><span data-stu-id="0bec6-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="0bec6-118">若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="0bec6-119">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0bec6-120">若要使用提供者，請安裝其 NuGet 套件，並在 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 的執行個體上呼叫該提供者的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="0bec6-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="0bec6-121">ASP.NET Core [相依性插入 (DI)](xref:fundamentals/dependency-injection) 提供 `ILoggerFactory` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bec6-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="0bec6-122">`AddConsole` 和 `AddDebug` 擴充方法定義於 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) 和 [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="0bec6-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="0bec6-123">每個擴充方法會呼叫 `ILoggerFactory.AddProvider` 方法，並傳入提供者的執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bec6-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="0bec6-124">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x)會在 `Startup.Configure` 方法中新增記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="0bec6-125">若要從先前執行的程式碼取得記錄輸出，請在 `Startup` 類別建構函式中新增記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="0bec6-126">在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="0bec6-127">建立記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-127">Create logs</span></span>

<span data-ttu-id="0bec6-128">從 DI 取得 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0bec6-129">下列控制器範例會建立 `Information` 與 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="0bec6-130">「類別」是 `TodoApiSample.Controllers.TodoController` (範例應用程式中 `TodoController` 的完整類別名稱)：</span><span class="sxs-lookup"><span data-stu-id="0bec6-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="0bec6-131">下鎳 Razor Pages 範例會建立記錄，其中「層級」*l* 為 `Information` 且「類別」 為 `TodoApiSample.Pages.AboutModel`：</span><span class="sxs-lookup"><span data-stu-id="0bec6-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="0bec6-132">前述範例會建立記錄，其中「層級」 為 `Information` 與 `Warning` 且「類別」為 `TodoController`類別。</span><span class="sxs-lookup"><span data-stu-id="0bec6-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="0bec6-133">記錄「層級」 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="0bec6-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="0bec6-134">記錄「類別」是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="0bec6-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="0bec6-135">`ILogger<T>` 執行個體會建立使用類型 `T` 做為類別之完整名稱的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="0bec6-136">此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="0bec6-137">在啟動中建立記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-137">Create logs in Startup</span></span>

<span data-ttu-id="0bec6-138">若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：</span><span class="sxs-lookup"><span data-stu-id="0bec6-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="0bec6-139">在程式中建立記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-139">Create logs in Program</span></span>

<span data-ttu-id="0bec6-140">若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="0bec6-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="0bec6-141">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="0bec6-141">No asynchronous logger methods</span></span>

<span data-ttu-id="0bec6-142">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="0bec6-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="0bec6-143">若您的記錄資料存放區很慢，請不要直接寫入其中。</span><span class="sxs-lookup"><span data-stu-id="0bec6-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="0bec6-144">請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="0bec6-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="0bec6-145">例如，如果您要登入 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="0bec6-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="0bec6-146">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="0bec6-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="0bec6-147">設定</span><span class="sxs-lookup"><span data-stu-id="0bec6-147">Configuration</span></span>

<span data-ttu-id="0bec6-148">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="0bec6-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="0bec6-149">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="0bec6-150">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="0bec6-150">Command-line arguments.</span></span>
* <span data-ttu-id="0bec6-151">環境變數。</span><span class="sxs-lookup"><span data-stu-id="0bec6-151">Environment variables.</span></span>
* <span data-ttu-id="0bec6-152">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="0bec6-153">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="0bec6-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="0bec6-154">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="0bec6-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="0bec6-155">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="0bec6-156">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="0bec6-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="0bec6-157">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="0bec6-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="0bec6-158">`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="0bec6-159">`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="0bec6-160">在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="0bec6-161">`Logging` 下的其他屬性可指定記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="0bec6-162">範例使用主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-162">The example is for the Console provider.</span></span> <span data-ttu-id="0bec6-163">若提供者支援[記錄範圍](#log-scopes)，`IncludeScopes` 會指出是否已啟用記錄範圍。</span><span class="sxs-lookup"><span data-stu-id="0bec6-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="0bec6-164">提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="0bec6-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> `LogLevel` <span data-ttu-id="0bec6-165">(在提供者底下) 會指定提供者的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-165">under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="0bec6-166">若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="0bec6-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

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

`LogLevel` <span data-ttu-id="0bec6-167">機碼代表記錄名稱。</span><span class="sxs-lookup"><span data-stu-id="0bec6-167">keys represent log names.</span></span> <span data-ttu-id="0bec6-168">`Default` 機碼會套用到未明確列出的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="0bec6-169">此值代表套用到指定記錄的[記錄層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="0bec6-170">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="0bec6-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="0bec6-171">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="0bec6-171">Sample logging output</span></span>

<span data-ttu-id="0bec6-172">使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="0bec6-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="0bec6-173">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="0bec6-173">Here's an example of console output:</span></span>

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

<span data-ttu-id="0bec6-174">上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。</span><span class="sxs-lookup"><span data-stu-id="0bec6-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="0bec6-175">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="0bec6-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="0bec6-176">這些透過上一節中所示的 `ILogger` 呼叫建立的記錄是以 "TodoApi.Controllers.TodoController" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="0bec6-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="0bec6-177">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="0bec6-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="0bec6-178">ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="0bec6-179">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="0bec6-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="0bec6-180">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="0bec6-180">NuGet packages</span></span>

<span data-ttu-id="0bec6-181">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="0bec6-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="0bec6-182">記錄類別</span><span class="sxs-lookup"><span data-stu-id="0bec6-182">Log category</span></span>

<span data-ttu-id="0bec6-183">建立 `ILogger` 物件時，會為它指定「類別」。</span><span class="sxs-lookup"><span data-stu-id="0bec6-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="0bec6-184">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="0bec6-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="0bec6-185">類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="0bec6-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="0bec6-186">使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：</span><span class="sxs-lookup"><span data-stu-id="0bec6-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="0bec6-187">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="0bec6-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` <span data-ttu-id="0bec6-188">相當於使用 `T` 的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="0bec6-188">is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="0bec6-189">記錄層級</span><span class="sxs-lookup"><span data-stu-id="0bec6-189">Log level</span></span>

<span data-ttu-id="0bec6-190">每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。</span><span class="sxs-lookup"><span data-stu-id="0bec6-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="0bec6-191">記錄層級表示嚴重性或重要性。</span><span class="sxs-lookup"><span data-stu-id="0bec6-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="0bec6-192">例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="0bec6-193">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="0bec6-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="0bec6-194">在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="0bec6-195">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="0bec6-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="0bec6-196">此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="0bec6-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="0bec6-197">在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="0bec6-198">這些方法會呼叫接受 `Log` 參數的 `LogLevel`。</span><span class="sxs-lookup"><span data-stu-id="0bec6-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="0bec6-199">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="0bec6-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="0bec6-200">如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="0bec6-201">ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="0bec6-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="0bec6-202">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="0bec6-202">Trace = 0</span></span>

  <span data-ttu-id="0bec6-203">針對通常只對偵錯有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="0bec6-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="0bec6-204">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="0bec6-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> *<span data-ttu-id="0bec6-205">預設為停用。</span><span class="sxs-lookup"><span data-stu-id="0bec6-205">Disabled by default.</span></span>*

* <span data-ttu-id="0bec6-206">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="0bec6-206">Debug = 1</span></span>

  <span data-ttu-id="0bec6-207">針對可在開發與偵錯中使用的資訊。</span><span class="sxs-lookup"><span data-stu-id="0bec6-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="0bec6-208">範例：`Entering method Configure with flag set to true.` 只有在進行疑難排解時才在生產環境中啟用 `Debug` 層級記錄，因為此類記錄的數目非常多。</span><span class="sxs-lookup"><span data-stu-id="0bec6-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="0bec6-209">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="0bec6-209">Information = 2</span></span>

  <span data-ttu-id="0bec6-210">針對一般應用程式流程的追蹤。</span><span class="sxs-lookup"><span data-stu-id="0bec6-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="0bec6-211">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="0bec6-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="0bec6-212">範例：</span><span class="sxs-lookup"><span data-stu-id="0bec6-212">Example:</span></span> `Request received for path /api/todo`

* <span data-ttu-id="0bec6-213">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="0bec6-213">Warning = 3</span></span>

  <span data-ttu-id="0bec6-214">針對應用程式流程中發生的異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="0bec6-215">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="0bec6-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="0bec6-216">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="0bec6-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="0bec6-217">範例：</span><span class="sxs-lookup"><span data-stu-id="0bec6-217">Example:</span></span> `FileNotFoundException for file quotes.txt.`

* <span data-ttu-id="0bec6-218">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="0bec6-218">Error = 4</span></span>

  <span data-ttu-id="0bec6-219">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0bec6-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="0bec6-220">這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="0bec6-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="0bec6-221">範例記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="0bec6-221">Example log message:</span></span> `Cannot insert record due to duplicate key violation.`

* <span data-ttu-id="0bec6-222">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="0bec6-222">Critical = 5</span></span>

  <span data-ttu-id="0bec6-223">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="0bec6-223">For failures that require immediate attention.</span></span> <span data-ttu-id="0bec6-224">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="0bec6-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="0bec6-225">使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="0bec6-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="0bec6-226">例如：</span><span class="sxs-lookup"><span data-stu-id="0bec6-226">For example:</span></span>

* <span data-ttu-id="0bec6-227">在生產環境中，透過 `Information` 層級將 `Trace` 傳送到大量資料存放區。</span><span class="sxs-lookup"><span data-stu-id="0bec6-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="0bec6-228">透過 `Critical` 將 `Warning` 傳送到值資料存放區。</span><span class="sxs-lookup"><span data-stu-id="0bec6-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="0bec6-229">在開發期間，透過 `Critical` 將 `Warning` 傳送到主控台，並在進行疑難排解時透過 `Information` 新增 `Trace`。</span><span class="sxs-lookup"><span data-stu-id="0bec6-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="0bec6-230">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="0bec6-231">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="0bec6-232">此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="0bec6-233">以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：</span><span class="sxs-lookup"><span data-stu-id="0bec6-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="0bec6-234">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="0bec6-234">Log event ID</span></span>

<span data-ttu-id="0bec6-235">每個記錄都可以指定「事件識別碼」。</span><span class="sxs-lookup"><span data-stu-id="0bec6-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="0bec6-236">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="0bec6-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="0bec6-237">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="0bec6-237">An event ID associates a set of events.</span></span> <span data-ttu-id="0bec6-238">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="0bec6-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="0bec6-239">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="0bec6-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="0bec6-240">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="0bec6-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="0bec6-241">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="0bec6-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="0bec6-242">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="0bec6-242">Log message template</span></span>

<span data-ttu-id="0bec6-243">每個記錄都會指定訊息範本。</span><span class="sxs-lookup"><span data-stu-id="0bec6-243">Each log specifies a message template.</span></span> <span data-ttu-id="0bec6-244">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="0bec6-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="0bec6-245">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="0bec6-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="0bec6-246">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="0bec6-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="0bec6-247">請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：</span><span class="sxs-lookup"><span data-stu-id="0bec6-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="0bec6-248">此程式碼會使用依順序的參數值建立記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="0bec6-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="0bec6-249">記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="0bec6-250">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="0bec6-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="0bec6-251">此資訊可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="0bec6-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="0bec6-252">例如，假設記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0bec6-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="0bec6-253">若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="0bec6-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="0bec6-254">查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。</span><span class="sxs-lookup"><span data-stu-id="0bec6-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="0bec6-255">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="0bec6-255">Logging exceptions</span></span>

<span data-ttu-id="0bec6-256">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0bec6-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="0bec6-257">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="0bec6-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="0bec6-258">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="0bec6-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="0bec6-259">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="0bec6-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0bec6-260">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="0bec6-261">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="0bec6-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="0bec6-262">若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="0bec6-263">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="0bec6-264">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="0bec6-264">Create filter rules in configuration</span></span>

<span data-ttu-id="0bec6-265">專案範本程式碼會呼叫 `CreateDefaultBuilder` 以設定主控台與偵錯提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="0bec6-266">`CreateDefaultBuilder` 方法也會使用如下所示的程式碼，將記錄設定為尋找 `Logging` 區段中的組態：</span><span class="sxs-lookup"><span data-stu-id="0bec6-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="0bec6-267">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="0bec6-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="0bec6-268">此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="0bec6-269">建立 `ILogger` 物件時，會為每個提供者選取一個規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="0bec6-270">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="0bec6-270">Filter rules in code</span></span>

<span data-ttu-id="0bec6-271">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="0bec6-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="0bec6-272">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="0bec6-273">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="0bec6-274">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="0bec6-274">How filtering rules are applied</span></span>

<span data-ttu-id="0bec6-275">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="0bec6-276">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="0bec6-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="0bec6-277">number</span><span class="sxs-lookup"><span data-stu-id="0bec6-277">Number</span></span> | <span data-ttu-id="0bec6-278">提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-278">Provider</span></span>      | <span data-ttu-id="0bec6-279">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="0bec6-279">Categories that begin with ...</span></span>          | <span data-ttu-id="0bec6-280">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="0bec6-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="0bec6-281">1</span><span class="sxs-lookup"><span data-stu-id="0bec6-281">1</span></span>      | <span data-ttu-id="0bec6-282">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-282">Debug</span></span>         | <span data-ttu-id="0bec6-283">所有類別</span><span class="sxs-lookup"><span data-stu-id="0bec6-283">All categories</span></span>                          | <span data-ttu-id="0bec6-284">資訊</span><span class="sxs-lookup"><span data-stu-id="0bec6-284">Information</span></span>       |
| <span data-ttu-id="0bec6-285">2</span><span class="sxs-lookup"><span data-stu-id="0bec6-285">2</span></span>      | <span data-ttu-id="0bec6-286">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-286">Console</span></span>       | <span data-ttu-id="0bec6-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="0bec6-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="0bec6-288">警告</span><span class="sxs-lookup"><span data-stu-id="0bec6-288">Warning</span></span>           |
| <span data-ttu-id="0bec6-289">3</span><span class="sxs-lookup"><span data-stu-id="0bec6-289">3</span></span>      | <span data-ttu-id="0bec6-290">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-290">Console</span></span>       | <span data-ttu-id="0bec6-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="0bec6-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="0bec6-292">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-292">Debug</span></span>             |
| <span data-ttu-id="0bec6-293">4</span><span class="sxs-lookup"><span data-stu-id="0bec6-293">4</span></span>      | <span data-ttu-id="0bec6-294">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-294">Console</span></span>       | <span data-ttu-id="0bec6-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="0bec6-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="0bec6-296">錯誤</span><span class="sxs-lookup"><span data-stu-id="0bec6-296">Error</span></span>             |
| <span data-ttu-id="0bec6-297">5</span><span class="sxs-lookup"><span data-stu-id="0bec6-297">5</span></span>      | <span data-ttu-id="0bec6-298">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-298">Console</span></span>       | <span data-ttu-id="0bec6-299">所有類別</span><span class="sxs-lookup"><span data-stu-id="0bec6-299">All categories</span></span>                          | <span data-ttu-id="0bec6-300">資訊</span><span class="sxs-lookup"><span data-stu-id="0bec6-300">Information</span></span>       |
| <span data-ttu-id="0bec6-301">6</span><span class="sxs-lookup"><span data-stu-id="0bec6-301">6</span></span>      | <span data-ttu-id="0bec6-302">所有提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-302">All providers</span></span> | <span data-ttu-id="0bec6-303">所有類別</span><span class="sxs-lookup"><span data-stu-id="0bec6-303">All categories</span></span>                          | <span data-ttu-id="0bec6-304">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-304">Debug</span></span>             |
| <span data-ttu-id="0bec6-305">7</span><span class="sxs-lookup"><span data-stu-id="0bec6-305">7</span></span>      | <span data-ttu-id="0bec6-306">所有提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-306">All providers</span></span> | <span data-ttu-id="0bec6-307">系統</span><span class="sxs-lookup"><span data-stu-id="0bec6-307">System</span></span>                                  | <span data-ttu-id="0bec6-308">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-308">Debug</span></span>             |
| <span data-ttu-id="0bec6-309">8</span><span class="sxs-lookup"><span data-stu-id="0bec6-309">8</span></span>      | <span data-ttu-id="0bec6-310">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-310">Debug</span></span>         | <span data-ttu-id="0bec6-311">Microsoft</span><span class="sxs-lookup"><span data-stu-id="0bec6-311">Microsoft</span></span>                               | <span data-ttu-id="0bec6-312">追蹤</span><span class="sxs-lookup"><span data-stu-id="0bec6-312">Trace</span></span>             |

<span data-ttu-id="0bec6-313">建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="0bec6-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="0bec6-314">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="0bec6-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="0bec6-315">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="0bec6-316">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="0bec6-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="0bec6-317">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="0bec6-318">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="0bec6-319">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="0bec6-320">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="0bec6-321">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="0bec6-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="0bec6-322">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="0bec6-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="0bec6-323">使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="0bec6-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="0bec6-324">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="0bec6-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="0bec6-325">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="0bec6-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="0bec6-326">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="0bec6-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="0bec6-327">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="0bec6-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="0bec6-328">產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="0bec6-329">`Debug` 層級以上的記錄會傳送到主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="0bec6-330">提供者別名</span><span class="sxs-lookup"><span data-stu-id="0bec6-330">Provider aliases</span></span>

<span data-ttu-id="0bec6-331">每個提供者都會定義「別名」，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="0bec6-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="0bec6-332">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="0bec6-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="0bec6-333">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-333">Console</span></span>
* <span data-ttu-id="0bec6-334">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-334">Debug</span></span>
* <span data-ttu-id="0bec6-335">EventLog</span><span class="sxs-lookup"><span data-stu-id="0bec6-335">EventLog</span></span>
* <span data-ttu-id="0bec6-336">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="0bec6-336">AzureAppServices</span></span>
* <span data-ttu-id="0bec6-337">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0bec6-337">TraceSource</span></span>
* <span data-ttu-id="0bec6-338">EventSource</span><span class="sxs-lookup"><span data-stu-id="0bec6-338">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="0bec6-339">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="0bec6-339">Default minimum level</span></span>

<span data-ttu-id="0bec6-340">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="0bec6-340">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="0bec6-341">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="0bec6-341">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="0bec6-342">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-342">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="0bec6-343">篩選函式</span><span class="sxs-lookup"><span data-stu-id="0bec6-343">Filter functions</span></span>

<span data-ttu-id="0bec6-344">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="0bec6-344">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="0bec6-345">函式中的程式碼可以存取提供者類型、類別與記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-345">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="0bec6-346">例如：</span><span class="sxs-lookup"><span data-stu-id="0bec6-346">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0bec6-347">某些記錄提供者可讓您指定何時應該將記錄寫入儲存媒體，何時應該根據記錄層級和類別加以略過。</span><span class="sxs-lookup"><span data-stu-id="0bec6-347">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="0bec6-348">`AddConsole` 與 `AddDebug` 擴充方法提供多載，可接受篩選條件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-348">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="0bec6-349">下列範例程式碼會使主控台提供者略過 `Warning` 層級以下的記錄，而偵錯提供者則會略過架構所建立的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-349">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="0bec6-350">`AddEventLog` 方法具有接受 `EventLogSettings` 執行個體的多載，其 `Filter` 屬性中可能包含篩選函式。</span><span class="sxs-lookup"><span data-stu-id="0bec6-350">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="0bec6-351">TraceSource 提供者未提供上述任何多載，因為其記錄層級和其他參數會根據其所使用的 `SourceSwitch` 和 `TraceListener`。</span><span class="sxs-lookup"><span data-stu-id="0bec6-351">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="0bec6-352">若要為已向 `ILoggerFactory` 執行個體註冊的所有提供者設定篩選規則，請使用 `WithFilter` 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="0bec6-352">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="0bec6-353">下列範例會將架構記錄 (開頭為 "Microsoft" 或 "System" 的類別) 限制為警告，同時在偵錯層級記錄由應用程式程式碼所建立的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-353">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="0bec6-354">若要防止寫入任何記錄，請指定 `LogLevel.None` 做為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-354">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="0bec6-355">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-355">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="0bec6-356">`WithFilter` 擴充方法是由 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet 套件提供。</span><span class="sxs-lookup"><span data-stu-id="0bec6-356">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="0bec6-357">此方法會傳回新的 `ILoggerFactory` 執行個體，這會篩選傳遞至用它來註冊之所有記錄器提供者的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="0bec6-357">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="0bec6-358">它不會影響任何其他 `ILoggerFactory` 執行個體，包括原始 `ILoggerFactory` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="0bec6-358">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="0bec6-359">系統類別與層級</span><span class="sxs-lookup"><span data-stu-id="0bec6-359">System categories and levels</span></span>

<span data-ttu-id="0bec6-360">以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：</span><span class="sxs-lookup"><span data-stu-id="0bec6-360">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="0bec6-361">分類</span><span class="sxs-lookup"><span data-stu-id="0bec6-361">Category</span></span>                            | <span data-ttu-id="0bec6-362">注意</span><span class="sxs-lookup"><span data-stu-id="0bec6-362">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="0bec6-363">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="0bec6-363">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="0bec6-364">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="0bec6-364">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="0bec6-365">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="0bec6-365">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="0bec6-366">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="0bec6-366">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="0bec6-367">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="0bec6-367">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="0bec6-368">允許主機。</span><span class="sxs-lookup"><span data-stu-id="0bec6-368">Hosts allowed.</span></span> |
| <span data-ttu-id="0bec6-369">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="0bec6-369">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="0bec6-370">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="0bec6-370">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="0bec6-371">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-371">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="0bec6-372">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="0bec6-372">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="0bec6-373">MVC 與 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="0bec6-373">MVC and Razor diagnostics.</span></span> <span data-ttu-id="0bec6-374">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="0bec6-374">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="0bec6-375">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="0bec6-375">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="0bec6-376">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="0bec6-376">Route matching information.</span></span> |
| <span data-ttu-id="0bec6-377">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="0bec6-377">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="0bec6-378">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="0bec6-378">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="0bec6-379">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="0bec6-379">HTTPS certificate information.</span></span> |
| <span data-ttu-id="0bec6-380">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="0bec6-380">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="0bec6-381">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="0bec6-381">Files served.</span></span> |
| <span data-ttu-id="0bec6-382">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0bec6-382">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="0bec6-383">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="0bec6-383">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="0bec6-384">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="0bec6-384">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="0bec6-385">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="0bec6-385">Log scopes</span></span>

 <span data-ttu-id="0bec6-386">「範圍」可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="0bec6-386">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="0bec6-387">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-387">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="0bec6-388">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="0bec6-388">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="0bec6-389">範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="0bec6-389">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="0bec6-390">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="0bec6-390">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="0bec6-391">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="0bec6-391">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="0bec6-392">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="0bec6-392">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="0bec6-393">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-393">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="0bec6-394">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="0bec6-394">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0bec6-395">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="0bec6-395">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="0bec6-396">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-396">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0bec6-397">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="0bec6-397">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="0bec6-398">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="0bec6-398">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="0bec6-399">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-399">Built-in logging providers</span></span>

<span data-ttu-id="0bec6-400">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="0bec6-400">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="0bec6-401">主控台</span><span class="sxs-lookup"><span data-stu-id="0bec6-401">Console</span></span>](#console-provider)
* [<span data-ttu-id="0bec6-402">偵錯</span><span class="sxs-lookup"><span data-stu-id="0bec6-402">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="0bec6-403">EventSource</span><span class="sxs-lookup"><span data-stu-id="0bec6-403">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="0bec6-404">EventLog</span><span class="sxs-lookup"><span data-stu-id="0bec6-404">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="0bec6-405">TraceSource</span><span class="sxs-lookup"><span data-stu-id="0bec6-405">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="0bec6-406">此文章稍後將說明 [Azure 中的記錄](#logging-in-azure)選項。</span><span class="sxs-lookup"><span data-stu-id="0bec6-406">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="0bec6-407">如需關於 stdout 記錄的資訊，請參閱<xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>與<xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>。</span><span class="sxs-lookup"><span data-stu-id="0bec6-407">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="0bec6-408">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-408">Console provider</span></span>

<span data-ttu-id="0bec6-409">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="0bec6-409">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="0bec6-410">[AddConsole 多載](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions)可讓您傳入最低記錄層級、篩選函式，以及指出是否支援範圍的布林值。</span><span class="sxs-lookup"><span data-stu-id="0bec6-410">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="0bec6-411">另一個選項是傳入可指定範圍支援和記錄層級的 `IConfiguration` 物件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-411">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="0bec6-412">主控台提供者對效能有重大影響，因此通常不適合在生產環境中使用。</span><span class="sxs-lookup"><span data-stu-id="0bec6-412">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="0bec6-413">當您在 Visual Studio 中建立新的專案時，`AddConsole` 方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0bec6-413">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="0bec6-414">此程式碼會參考 *appSettings.json* 檔案的 `Logging` 區段：</span><span class="sxs-lookup"><span data-stu-id="0bec6-414">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="0bec6-415">上述設定會將架構記錄限制為警告，同時讓應用程式在偵錯層級記錄，如[記錄篩選](#log-filtering)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="0bec6-415">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="0bec6-416">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-416">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="0bec6-417">若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0bec6-417">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="0bec6-418">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-418">Debug provider</span></span>

<span data-ttu-id="0bec6-419">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="0bec6-419">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="0bec6-420">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="0bec6-420">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="0bec6-421">[AddDebug 多載](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions)可讓您傳入最低記錄層級或篩選函式。</span><span class="sxs-lookup"><span data-stu-id="0bec6-421">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="0bec6-422">EventSource 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-422">EventSource provider</span></span>

<span data-ttu-id="0bec6-423">針對以 ASP.NET Core 1.1.0 或更新版本為目標的應用程式，[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) 提供者套件可以實作事件追蹤。</span><span class="sxs-lookup"><span data-stu-id="0bec6-423">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="0bec6-424">在 Windows 上，它會使用 [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-424">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="0bec6-425">提供者可以跨平台，但目前沒有適用於 Linux 或 macOS 的事件收集和顯示工具。</span><span class="sxs-lookup"><span data-stu-id="0bec6-425">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="0bec6-426">收集及檢視記錄的一個好方法是使用 [PerfView 公用程式](https://github.com/Microsoft/perfview)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-426">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="0bec6-427">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET 所發出之 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="0bec6-427">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="0bec6-428">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者] 清單</span><span class="sxs-lookup"><span data-stu-id="0bec6-428">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="0bec6-429">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-429">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="0bec6-431">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-431">Windows EventLog provider</span></span>

<span data-ttu-id="0bec6-432">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="0bec6-432">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="0bec6-433">[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 `EventLogSettings` 或最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="0bec6-433">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="0bec6-434">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-434">TraceSource provider</span></span>

<span data-ttu-id="0bec6-435">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-435">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

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

<span data-ttu-id="0bec6-436">[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="0bec6-436">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="0bec6-437">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="0bec6-437">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="0bec6-438">該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。</span><span class="sxs-lookup"><span data-stu-id="0bec6-438">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0bec6-439">下列範例會設定 `TraceSource` 提供者，將 `Warning` 和更高層級訊息記錄至主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="0bec6-439">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="0bec6-440">Azure 中的記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-440">Logging in Azure</span></span>

<span data-ttu-id="0bec6-441">如需 Azure 中的記錄相關資訊，請參閱以下各節：</span><span class="sxs-lookup"><span data-stu-id="0bec6-441">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="0bec6-442">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-442">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="0bec6-443">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="0bec6-443">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="0bec6-444">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-444">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="0bec6-445">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-445">Azure App Service provider</span></span>

<span data-ttu-id="0bec6-446">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-446">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="0bec6-447">提供者套件適用於以 .NET Core 1.1 或更新版本為目標的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bec6-447">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0bec6-448">若以 .NET Core 為目標，請注意下列幾點：</span><span class="sxs-lookup"><span data-stu-id="0bec6-448">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="0bec6-449">ASP.NET Core [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)包含提供者套件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-449">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="0bec6-450">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-450">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="0bec6-451">若要使用提供者，請安裝套件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-451">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="0bec6-452">不要明確地呼叫 <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>。</span><span class="sxs-lookup"><span data-stu-id="0bec6-452">Don't explicitly call <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span></span> <span data-ttu-id="0bec6-453">當應用程式部署至 Azure App Service 時，就會自動提供此提供者給應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bec6-453">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="0bec6-454">若以 .NET Framework 為目標，或是參考 `Microsoft.AspNetCore.App` 中繼套件，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="0bec6-454">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="0bec6-455">叫用 `AddAzureWebAppDiagnostics`：</span><span class="sxs-lookup"><span data-stu-id="0bec6-455">Invoke `AddAzureWebAppDiagnostics`:</span></span>

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

<span data-ttu-id="0bec6-456"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。</span><span class="sxs-lookup"><span data-stu-id="0bec6-456">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="0bec6-457">設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。</span><span class="sxs-lookup"><span data-stu-id="0bec6-457">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="0bec6-458">(*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)</span><span class="sxs-lookup"><span data-stu-id="0bec6-458">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0bec6-459">如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：</span><span class="sxs-lookup"><span data-stu-id="0bec6-459">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="0bec6-460">當您部署到 App Service 應用程式時，應用程式會接受 Azure 入口網站的 [App Service] 頁面之[診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段中的設定。</span><span class="sxs-lookup"><span data-stu-id="0bec6-460">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="0bec6-461">當更新這些設定時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bec6-461">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure 記錄設定](index/_static/azure-logging-settings.png)

<span data-ttu-id="0bec6-463">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="0bec6-463">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="0bec6-464">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="0bec6-464">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="0bec6-465">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="0bec6-465">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="0bec6-466">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="0bec6-466">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="0bec6-467">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="0bec6-467">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="0bec6-468">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="0bec6-468">Azure log streaming</span></span>

<span data-ttu-id="0bec6-469">Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="0bec6-469">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="0bec6-470">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="0bec6-470">The app server</span></span>
* <span data-ttu-id="0bec6-471">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="0bec6-471">The web server</span></span>
* <span data-ttu-id="0bec6-472">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="0bec6-472">Failed request tracing</span></span>

<span data-ttu-id="0bec6-473">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="0bec6-473">To configure Azure log streaming:</span></span>

* <span data-ttu-id="0bec6-474">從您應用程式的入口網站頁面瀏覽到 [診斷記錄]。</span><span class="sxs-lookup"><span data-stu-id="0bec6-474">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="0bec6-475">將 [應用程式記錄 (檔案系統)] 設定為 [開啟]。</span><span class="sxs-lookup"><span data-stu-id="0bec6-475">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Azure 入口網站的 [診斷記錄] 頁面](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="0bec6-477">瀏覽到 [記錄串流] 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="0bec6-477">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="0bec6-478">這些是應用程式透過 `ILogger` 介面產生的訊息。</span><span class="sxs-lookup"><span data-stu-id="0bec6-478">They're logged by the app through the `ILogger` interface.</span></span>

![Azure 入口網站應用程式的 [記錄資料流]](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="0bec6-480">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="0bec6-480">Azure Application Insights trace logging</span></span>

<span data-ttu-id="0bec6-481">Application Insights SDK 可以收集及回報由 ASP.NET Core 記錄基礎結構所產生的記錄。</span><span class="sxs-lookup"><span data-stu-id="0bec6-481">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="0bec6-482">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="0bec6-482">For more information, see the following resources:</span></span>

* [<span data-ttu-id="0bec6-483">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="0bec6-483">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="0bec6-484">Application Insights for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0bec6-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="0bec6-485">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-485">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="0bec6-486">Application Insights ILogger 實作範例</span><span class="sxs-lookup"><span data-stu-id="0bec6-486">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="0bec6-487">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="0bec6-487">Third-party logging providers</span></span>

<span data-ttu-id="0bec6-488">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="0bec6-488">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="0bec6-489">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0bec6-489">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="0bec6-490">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="0bec6-490">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="0bec6-491">[JSNLog](http://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="0bec6-491">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="0bec6-492">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="0bec6-492">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="0bec6-493">[Loggr](http://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0bec6-493">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="0bec6-494">[NLog](http://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="0bec6-494">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="0bec6-495">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="0bec6-495">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="0bec6-496">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="0bec6-496">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="0bec6-497">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="0bec6-497">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="0bec6-498">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="0bec6-498">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="0bec6-499">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="0bec6-499">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="0bec6-500">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="0bec6-500">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="0bec6-501">呼叫 `ILoggerFactory`。</span><span class="sxs-lookup"><span data-stu-id="0bec6-501">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="0bec6-502">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="0bec6-502">For more information, see each provider's documentation.</span></span> <span data-ttu-id="0bec6-503">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="0bec6-503">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0bec6-504">其他資源</span><span class="sxs-lookup"><span data-stu-id="0bec6-504">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
