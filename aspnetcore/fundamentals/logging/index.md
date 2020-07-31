---
title: .NET Core 與 ASP.NET Core 中的記錄
author: rick-anderson
description: 了解如何使用由 Microsoft.Extensions.Logging NuGet 套件提供的記錄架構。
ms.author: riande
ms.custom: mvc
ms.date: 6/29/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/logging/index
ms.openlocfilehash: a4962c3132c60b68cb2d4b5a97e9b082aba15d15
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "87330717"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="4bef2-103">.NET Core 與 ASP.NET Core 中的記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-103">Logging in .NET Core and ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4bef2-104">By [Kirk Larkin](https://twitter.com/serpent5)、 [Juergen Gutsch](https://github.com/JuergenGutsch)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4bef2-104">By [Kirk Larkin](https://twitter.com/serpent5), [Juergen Gutsch](https://github.com/JuergenGutsch) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4bef2-105">.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="4bef2-106">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="4bef2-106">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="4bef2-107">本文中顯示的大部分程式碼範例都來自 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="4bef2-108">這些程式碼片段的記錄特定部分適用于任何使用[泛型主機](xref:fundamentals/host/generic-host)的 .net Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="4bef2-109">ASP.NET Core web 應用程式範本會使用泛型主機。</span><span class="sxs-lookup"><span data-stu-id="4bef2-109">The ASP.NET Core web app templates use the Generic Host.</span></span>

<span data-ttu-id="4bef2-110">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/3.x)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="4bef2-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<a name="lp"></a>

## <a name="logging-providers"></a><span data-ttu-id="4bef2-111">記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-111">Logging providers</span></span>

<span data-ttu-id="4bef2-112">記錄提供者會儲存記錄檔，但 `Console` 顯示記錄的提供者除外。</span><span class="sxs-lookup"><span data-stu-id="4bef2-112">Logging providers store logs, except for the `Console` provider which displays logs.</span></span> <span data-ttu-id="4bef2-113">例如，Azure 應用程式 Insights 提供者會將記錄儲存在 Azure 應用程式的深入解析中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-113">For example, the Azure Application Insights provider stores logs in Azure Application Insights.</span></span> <span data-ttu-id="4bef2-114">可以啟用多個提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-114">Multiple providers can be enabled.</span></span>

<span data-ttu-id="4bef2-115">預設 ASP.NET Core web 應用程式範本：</span><span class="sxs-lookup"><span data-stu-id="4bef2-115">The default ASP.NET Core web app templates:</span></span>

* <span data-ttu-id="4bef2-116">使用[泛型主機](xref:fundamentals/host/generic-host)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-116">Use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>
* <span data-ttu-id="4bef2-117">呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> ，這會新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="4bef2-117">Call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>
  * [<span data-ttu-id="4bef2-118">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-118">Console</span></span>](#console)
  * [<span data-ttu-id="4bef2-119">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-119">Debug</span></span>](#debug)
  * [<span data-ttu-id="4bef2-120">EventSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-120">EventSource</span></span>](#event-source)
  * <span data-ttu-id="4bef2-121">[EventLog](#welog)：僅限 Windows</span><span class="sxs-lookup"><span data-stu-id="4bef2-121">[EventLog](#welog): Windows only</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_TemplateCode&highlight=9)]

<span data-ttu-id="4bef2-122">上述程式碼會顯示 `Program` 使用 ASP.NET Core web 應用程式範本所建立的類別。</span><span class="sxs-lookup"><span data-stu-id="4bef2-122">The preceding code shows the `Program` class created with the ASP.NET Core web app templates.</span></span> <span data-ttu-id="4bef2-123">接下來的幾節會根據使用泛型主機的 ASP.NET Core web 應用程式範本來提供範例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-123">The next several sections provide samples based on the ASP.NET Core web app templates, which use the Generic Host.</span></span> <span data-ttu-id="4bef2-124">本檔稍後會討論[非主機主控台應用程式](#nhca)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-124">[Non-host console apps](#nhca) are discussed later in this document.</span></span>

<span data-ttu-id="4bef2-125">若要覆寫所新增的預設記錄提供者集合 `Host.CreateDefaultBuilder` ，請呼叫 `ClearProviders` 並加入所需的記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-125">To override the default set of logging providers added by `Host.CreateDefaultBuilder`, call `ClearProviders` and add the required logging providers.</span></span> <span data-ttu-id="4bef2-126">例如，下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4bef2-126">For example, the following code:</span></span>

* <span data-ttu-id="4bef2-127">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> ，以從產生器中移除所有 <xref:Microsoft.Extensions.Logging.ILoggerProvider> 實例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-127">Calls <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> to remove all the <xref:Microsoft.Extensions.Logging.ILoggerProvider> instances from the builder.</span></span>
* <span data-ttu-id="4bef2-128">加入[主控台](#console)記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-128">Adds the [Console](#console) logging provider.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_AddProvider&highlight=5-6)]

<span data-ttu-id="4bef2-129">如需其他提供者，請參閱：</span><span class="sxs-lookup"><span data-stu-id="4bef2-129">For additional providers, see:</span></span>

* [<span data-ttu-id="4bef2-130">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-130">Built-in logging providers</span></span>](#bilp)
* <span data-ttu-id="4bef2-131">[協力廠商記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-131">[Third-party logging providers](#third-party-logging-providers).</span></span>

## <a name="create-logs"></a><span data-ttu-id="4bef2-132">建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-132">Create logs</span></span>

<span data-ttu-id="4bef2-133">若要建立記錄，請使用相依性 <xref:Microsoft.Extensions.Logging.ILogger%601> [插入](xref:fundamentals/dependency-injection)（DI）中的物件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-133">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object from [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="4bef2-134">下列範例將：</span><span class="sxs-lookup"><span data-stu-id="4bef2-134">The following example:</span></span>

* <span data-ttu-id="4bef2-135">建立記錄器， `ILogger<AboutModel>` 它會使用類型之完整名稱的記錄*類別* `AboutModel` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-135">Creates a logger, `ILogger<AboutModel>`, which uses a log *category* of the fully qualified name of the type `AboutModel`.</span></span> <span data-ttu-id="4bef2-136">記錄「類別」是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="4bef2-136">The log category is a string that is associated with each log.</span></span>
* <span data-ttu-id="4bef2-137"><xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>在層級通話記錄檔 `Information` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-137">Calls <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*> to log at the `Information` level.</span></span> <span data-ttu-id="4bef2-138">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-138">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=5,14)]

<span data-ttu-id="4bef2-139">本檔稍後將更詳細說明[層級](#log-level)和[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-139">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this document.</span></span>

<span data-ttu-id="4bef2-140">如需的詳細資訊 Blazor ，請參閱本檔[中 Blazor Blazor WebAssembly 的建立記錄](#clib)檔。</span><span class="sxs-lookup"><span data-stu-id="4bef2-140">For information on Blazor, see [Create logs in Blazor and Blazor WebAssembly](#clib) in this document.</span></span>

<span data-ttu-id="4bef2-141">[在主要和啟動中建立記錄](#clms)顯示如何在和中建立記錄 `Main` `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-141">[Create logs in Main and Startup](#clms) shows how to create logs in `Main` and `Startup`.</span></span>

## <a name="configure-logging"></a><span data-ttu-id="4bef2-142">設定記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-142">Configure logging</span></span>

<span data-ttu-id="4bef2-143">記錄設定通常是由 appsettings 的 `Logging` 區段所*appsettings*提供。 `{Environment}`*json*檔案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-143">Logging configuration is commonly provided by the `Logging` section of *appsettings*.`{Environment}`*.json* files.</span></span> <span data-ttu-id="4bef2-144">下列*appsettings.Development.js*檔案是由 ASP.NET Core web 應用程式範本所產生：</span><span class="sxs-lookup"><span data-stu-id="4bef2-144">The following *appsettings.Development.json* file is generated by the ASP.NET Core web app templates:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Development.json)]

<span data-ttu-id="4bef2-145">在上述 JSON 中：</span><span class="sxs-lookup"><span data-stu-id="4bef2-145">In the preceding JSON:</span></span>

* <span data-ttu-id="4bef2-146">`"Default"` `"Microsoft"` 已指定、和 `"Microsoft.Hosting.Lifetime"` 類別。</span><span class="sxs-lookup"><span data-stu-id="4bef2-146">The `"Default"`, `"Microsoft"`, and `"Microsoft.Hosting.Lifetime"` categories are specified.</span></span>
* <span data-ttu-id="4bef2-147">`"Microsoft"`類別目錄會套用至以開頭的所有類別 `"Microsoft"` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-147">The `"Microsoft"` category applies to all categories that start with `"Microsoft"`.</span></span> <span data-ttu-id="4bef2-148">例如，此設定會套用至 `"Microsoft.AspNetCore.Routing.EndpointMiddleware"` 類別。</span><span class="sxs-lookup"><span data-stu-id="4bef2-148">For example, this setting applies to the `"Microsoft.AspNetCore.Routing.EndpointMiddleware"` category.</span></span>
* <span data-ttu-id="4bef2-149">`"Microsoft"`記錄層級和更新版本的類別記錄 `Warning` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-149">The `"Microsoft"` category logs at log level `Warning` and higher.</span></span>
* <span data-ttu-id="4bef2-150">`"Microsoft.Hosting.Lifetime"`類別比類別更明確 `"Microsoft"` ，因此 `"Microsoft.Hosting.Lifetime"` 類別目錄記錄的記錄層級為「資訊」和更高。</span><span class="sxs-lookup"><span data-stu-id="4bef2-150">The `"Microsoft.Hosting.Lifetime"` category is more specific than the `"Microsoft"` category, so the `"Microsoft.Hosting.Lifetime"` category logs at log level "Information" and higher.</span></span>
* <span data-ttu-id="4bef2-151">未指定特定的記錄提供者，因此會 `LogLevel` 套用至所有已啟用的記錄提供者，但不包括[Windows EventLog](#welog)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-151">A specific log provider is not specified, so `LogLevel` applies to all the enabled logging providers except for the [Windows EventLog](#welog).</span></span>

<span data-ttu-id="4bef2-152">`Logging`屬性可以有 <xref:Microsoft.Extensions.Logging.LogLevel> 和記錄提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-152">The `Logging` property can have <xref:Microsoft.Extensions.Logging.LogLevel> and log provider properties.</span></span> <span data-ttu-id="4bef2-153">會 `LogLevel` 指定要記錄所選分類的最低[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-153">The `LogLevel` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="4bef2-154">在先前的 JSON 中 `Information` ， `Warning` 會指定和記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-154">In the preceding JSON, `Information` and `Warning` log levels are specified.</span></span> <span data-ttu-id="4bef2-155">`LogLevel`指出記錄檔的嚴重性，範圍從0到6：</span><span class="sxs-lookup"><span data-stu-id="4bef2-155">`LogLevel` indicates the severity of the log and ranges from 0 to 6:</span></span>

<span data-ttu-id="4bef2-156">`Trace`= 0、 `Debug` = 1、 `Information` = 2、 `Warning` = 3、 `Error` = 4、 `Critical` = 5 和 `None` = 6。</span><span class="sxs-lookup"><span data-stu-id="4bef2-156">`Trace` = 0, `Debug` = 1, `Information` = 2, `Warning` = 3, `Error` = 4, `Critical` = 5, and `None` = 6.</span></span>

<span data-ttu-id="4bef2-157">當 `LogLevel` 指定時，會針對指定層級和更新版本中的訊息啟用記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-157">When a `LogLevel` is specified, logging is enabled for messages at the specified level and higher.</span></span> <span data-ttu-id="4bef2-158">在先前的 JSON 中， `Default` 類別目錄會針對 `Information` 和更高版本進行記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-158">In the preceding JSON, the `Default` category is logged for `Information` and higher.</span></span> <span data-ttu-id="4bef2-159">例如， `Information` `Warning` 會記錄、、 `Error` 和 `Critical` 訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-159">For example, `Information`, `Warning`, `Error`, and `Critical` messages are logged.</span></span> <span data-ttu-id="4bef2-160">如果未 `LogLevel` 指定，則記錄預設為 `Information` 層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-160">If no `LogLevel` is specified, logging defaults to the `Information` level.</span></span> <span data-ttu-id="4bef2-161">如需詳細資訊，請參閱[記錄層級](#llvl)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-161">For more information, see [Log levels](#llvl).</span></span>

<span data-ttu-id="4bef2-162">提供者屬性可以指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-162">A provider property can specify a `LogLevel` property.</span></span> <span data-ttu-id="4bef2-163">`LogLevel`在提供者底下，指定該提供者的記錄層級，並覆寫非提供者記錄檔設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-163">`LogLevel` under a provider specifies levels to log for that provider, and overrides the non-provider log settings.</span></span> <span data-ttu-id="4bef2-164">請考慮下列*appsettings.js*檔案：</span><span class="sxs-lookup"><span data-stu-id="4bef2-164">Consider the following *appsettings.json* file:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Prod2.json)]

<span data-ttu-id="4bef2-165">中的設定會覆 `Logging.{providername}.LogLevel` 寫中的設定 `Logging.LogLevel` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-165">Settings in `Logging.{providername}.LogLevel` override settings in `Logging.LogLevel`.</span></span> <span data-ttu-id="4bef2-166">在上述 JSON 中， `Debug` 提供者的預設記錄層級會設定為 `Information` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-166">In the preceding JSON, the `Debug` provider's default log level is set to `Information`:</span></span>

`Logging:Debug:LogLevel:Default:Information`

<span data-ttu-id="4bef2-167">上述設定會 `Information` 針對每個類別目錄指定不同的記錄層級 `Logging:Debug:` `Microsoft.Hosting` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-167">The preceding setting specifies the `Information` log level for every `Logging:Debug:` category except `Microsoft.Hosting`.</span></span> <span data-ttu-id="4bef2-168">當列出特定分類時，特定類別會覆寫預設分類。</span><span class="sxs-lookup"><span data-stu-id="4bef2-168">When a specific category is listed, the specific category overrides the default category.</span></span> <span data-ttu-id="4bef2-169">在上述 JSON 中， `Logging:Debug:LogLevel` 類別和會覆 `"Microsoft.Hosting"` `"Default"` 寫中的設定`Logging:LogLevel`</span><span class="sxs-lookup"><span data-stu-id="4bef2-169">In the preceding JSON, the `Logging:Debug:LogLevel` categories `"Microsoft.Hosting"` and `"Default"` override the settings in `Logging:LogLevel`</span></span>

<span data-ttu-id="4bef2-170">您可以為下列任一項指定最小記錄層級：</span><span class="sxs-lookup"><span data-stu-id="4bef2-170">The minimum log level can be specified for any of:</span></span>

* <span data-ttu-id="4bef2-171">特定提供者：例如，`Logging:EventSource:LogLevel:Default:Information`</span><span class="sxs-lookup"><span data-stu-id="4bef2-171">Specific providers:  For example, `Logging:EventSource:LogLevel:Default:Information`</span></span>
* <span data-ttu-id="4bef2-172">特定類別：例如，`Logging:LogLevel:Microsoft:Warning`</span><span class="sxs-lookup"><span data-stu-id="4bef2-172">Specific categories: For example, `Logging:LogLevel:Microsoft:Warning`</span></span>
* <span data-ttu-id="4bef2-173">所有提供者和所有類別：`Logging:LogLevel:Default:Warning`</span><span class="sxs-lookup"><span data-stu-id="4bef2-173">All providers and all categories: `Logging:LogLevel:Default:Warning`</span></span>

<span data-ttu-id="4bef2-174">低於最低層級的任何記錄都***不***是：</span><span class="sxs-lookup"><span data-stu-id="4bef2-174">Any logs below the minimum level are ***not***:</span></span>

* <span data-ttu-id="4bef2-175">傳遞至提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-175">Passed to the provider.</span></span>
* <span data-ttu-id="4bef2-176">已記錄或顯示。</span><span class="sxs-lookup"><span data-stu-id="4bef2-176">Logged or displayed.</span></span>

<span data-ttu-id="4bef2-177">若要隱藏所有記錄，請指定[LogLevel](xref:Microsoft.Extensions.Logging.LogLevel)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-177">To suppress all logs, specify [LogLevel.None](xref:Microsoft.Extensions.Logging.LogLevel).</span></span> <span data-ttu-id="4bef2-178">`LogLevel.None`的值為6，其高於 `LogLevel.Critical` （5）。</span><span class="sxs-lookup"><span data-stu-id="4bef2-178">`LogLevel.None` has a value of 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="4bef2-179">如果提供者支援[記錄範圍](#logscopes)，則 `IncludeScopes` 指出是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-179">If a provider supports [log scopes](#logscopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="4bef2-180">如需詳細資訊，請參閱[記錄範圍](#logscopes)</span><span class="sxs-lookup"><span data-stu-id="4bef2-180">For more information, see [log scopes](#logscopes)</span></span>

<span data-ttu-id="4bef2-181">下列*appsettings.json*檔案包含預設啟用的所有提供者：</span><span class="sxs-lookup"><span data-stu-id="4bef2-181">The following *appsettings.json* file contains all the providers enabled by default:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Production.json)]

<span data-ttu-id="4bef2-182">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="4bef2-182">In the preceding sample:</span></span>

* <span data-ttu-id="4bef2-183">類別和層級不是建議的值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-183">The categories and levels are not suggested values.</span></span> <span data-ttu-id="4bef2-184">提供的範例會顯示所有預設的提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-184">The sample is provided to show all the default providers.</span></span>
* <span data-ttu-id="4bef2-185">中的設定會覆 `Logging.{providername}.LogLevel` 寫中的設定 `Logging.LogLevel` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-185">Settings in `Logging.{providername}.LogLevel` override settings in `Logging.LogLevel`.</span></span> <span data-ttu-id="4bef2-186">例如，中的層級會 `Debug.LogLevel.Default` 覆寫中的層級 `LogLevel.Default` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-186">For example, the level in `Debug.LogLevel.Default` overrides the level in `LogLevel.Default`.</span></span>
* <span data-ttu-id="4bef2-187">會使用每個預設的提供者*別名*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-187">Each default provider *alias* is used.</span></span> <span data-ttu-id="4bef2-188">每個提供者都會定義「別名」\*\*，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="4bef2-188">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span> <span data-ttu-id="4bef2-189">內建提供者別名如下：</span><span class="sxs-lookup"><span data-stu-id="4bef2-189">The built-in providers aliases are:</span></span>
  * <span data-ttu-id="4bef2-190">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-190">Console</span></span>
  * <span data-ttu-id="4bef2-191">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-191">Debug</span></span>
  * <span data-ttu-id="4bef2-192">EventSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-192">EventSource</span></span>
  * <span data-ttu-id="4bef2-193">EventLog</span><span class="sxs-lookup"><span data-stu-id="4bef2-193">EventLog</span></span>
  * <span data-ttu-id="4bef2-194">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="4bef2-194">AzureAppServicesFile</span></span>
  * <span data-ttu-id="4bef2-195">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="4bef2-195">AzureAppServicesBlob</span></span>
  * <span data-ttu-id="4bef2-196">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="4bef2-196">ApplicationInsights</span></span>

## <a name="set-log-level-by-command-line-environment-variables-and-other-configuration"></a><span data-ttu-id="4bef2-197">依命令列、環境變數及其他設定來設定記錄層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-197">Set log level by command line, environment variables, and other configuration</span></span>

<span data-ttu-id="4bef2-198">記錄層級可以由任何設定[提供者](xref:fundamentals/configuration/index)來設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-198">Log level can be set by any of the [configuration providers](xref:fundamentals/configuration/index).</span></span> 

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="4bef2-199">下列命令：</span><span class="sxs-lookup"><span data-stu-id="4bef2-199">The following commands:</span></span>

* <span data-ttu-id="4bef2-200">將環境索引鍵設定 `Logging:LogLevel:Microsoft` 為 `Information` Windows 上的值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-200">Set the environment key `Logging:LogLevel:Microsoft` to a value of `Information` on Windows.</span></span>
* <span data-ttu-id="4bef2-201">使用以 ASP.NET Core web 應用程式範本建立的應用程式時，請測試設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-201">Test the settings when using an app created with the ASP.NET Core web application templates.</span></span> <span data-ttu-id="4bef2-202">`dotnet run`使用之後，必須在專案目錄中執行命令 `set` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-202">The `dotnet run` command must be run in the project directory after using `set`.</span></span>

```cmd
set Logging__LogLevel__Microsoft=Information
dotnet run
```

<span data-ttu-id="4bef2-203">先前的環境設定：</span><span class="sxs-lookup"><span data-stu-id="4bef2-203">The preceding environment setting:</span></span>

* <span data-ttu-id="4bef2-204">只會在從其設定所在的命令視窗中啟動的進程中設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-204">Is only set in processes launched from the command window they were set in.</span></span>
* <span data-ttu-id="4bef2-205">以 Visual Studio 啟動的瀏覽器不會讀取。</span><span class="sxs-lookup"><span data-stu-id="4bef2-205">Isn't read by browsers launched with Visual Studio.</span></span>

<span data-ttu-id="4bef2-206">下列[setx](/windows-server/administration/windows-commands/setx)命令也會在 Windows 上設定環境金鑰和值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-206">The following [setx](/windows-server/administration/windows-commands/setx) command also sets the environment key and value on Windows.</span></span> <span data-ttu-id="4bef2-207">不同于 `set` ， `setx` 設定會保存下來。</span><span class="sxs-lookup"><span data-stu-id="4bef2-207">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="4bef2-208">`/M`參數會設定系統內容中的變數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-208">The `/M` switch sets the variable in the system environment.</span></span> <span data-ttu-id="4bef2-209">如果 `/M` 未使用，則會設定使用者環境變數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-209">If `/M` isn't used, a user environment variable is set.</span></span>

```cmd
setx Logging__LogLevel__Microsoft=Information /M
```

<span data-ttu-id="4bef2-210">在[Azure App Service](https://azure.microsoft.com/services/app-service/)上，選取 [**設定] > 設定**] 頁面上的 [**新增應用程式設定**]。</span><span class="sxs-lookup"><span data-stu-id="4bef2-210">On [Azure App Service](https://azure.microsoft.com/services/app-service/), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="4bef2-211">Azure App Service 的應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="4bef2-211">Azure App Service application settings are:</span></span>

* <span data-ttu-id="4bef2-212">待用加密，並透過加密通道傳輸。</span><span class="sxs-lookup"><span data-stu-id="4bef2-212">Encrypted at rest and transmitted over an encrypted channel.</span></span>
* <span data-ttu-id="4bef2-213">公開為環境變數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-213">Exposed as environment variables.</span></span>

<span data-ttu-id="4bef2-214">如需詳細資訊，請參閱 [Azure App：使用 Azure 入口網站覆寫應用程式設定](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-214">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="4bef2-215">如需使用環境變數設定 ASP.NET Core 設定值的詳細資訊，請參閱[環境變數](xref:fundamentals/configuration/index#environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-215">For more information on setting ASP.NET Core configuration values using environment variables, see [environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span> <span data-ttu-id="4bef2-216">如需使用其他設定來源（包括命令列、Azure Key Vault、Azure 應用程式組態、其他檔案格式等等）的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-216">For information on using other configuration sources, including the command line, Azure Key Vault, Azure App Configuration, other file formats, and more, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="4bef2-217">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="4bef2-217">How filtering rules are applied</span></span>

<span data-ttu-id="4bef2-218">建立 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件時，<xref:Microsoft.Extensions.Logging.ILoggerFactory> 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-218">When an <xref:Microsoft.Extensions.Logging.ILogger%601> object is created, the <xref:Microsoft.Extensions.Logging.ILoggerFactory> object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="4bef2-219">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="4bef2-219">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="4bef2-220">會從可用的規則中選取每個提供者和類別目錄配對的最特定規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-220">The most specific rule for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="4bef2-221">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-221">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="4bef2-222">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-222">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="4bef2-223">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-223">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="4bef2-224">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-224">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="4bef2-225">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-225">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="4bef2-226">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="4bef2-226">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="4bef2-227">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-227">If no rules are selected, use `MinimumLevel`.</span></span>

<a name="dnrvs"></a>

## <a name="logging-output-from-dotnet-run-and-visual-studio"></a><span data-ttu-id="4bef2-228">記錄來自 dotnet 執行和 Visual Studio 的輸出</span><span class="sxs-lookup"><span data-stu-id="4bef2-228">Logging output from dotnet run and Visual Studio</span></span>

<span data-ttu-id="4bef2-229">系統會顯示使用[預設記錄提供者](#lp)所建立的記錄：</span><span class="sxs-lookup"><span data-stu-id="4bef2-229">Logs created with the [default logging providers](#lp) are displayed:</span></span>

* <span data-ttu-id="4bef2-230">在 Visual Studio 中</span><span class="sxs-lookup"><span data-stu-id="4bef2-230">In Visual Studio</span></span>
  * <span data-ttu-id="4bef2-231">在調試時的 [調試輸出] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-231">In the Debug output window when debugging.</span></span>
  * <span data-ttu-id="4bef2-232">在 [ASP.NET Core Web 服務器] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-232">In the ASP.NET Core Web Server window.</span></span>
* <span data-ttu-id="4bef2-233">在以執行應用程式時的主控台視窗中 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-233">In the console window when the app is run with `dotnet run`.</span></span>

<span data-ttu-id="4bef2-234">開頭為 "Microsoft" 類別的記錄來自 ASP.NET Core framework 程式碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-234">Logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="4bef2-235">ASP.NET Core 和應用程式程式碼會使用相同的記錄 API 和提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-235">ASP.NET Core and application code use the same logging API and providers.</span></span>

<a name="lcat"></a>

## <a name="log-category"></a><span data-ttu-id="4bef2-236">記錄分類</span><span class="sxs-lookup"><span data-stu-id="4bef2-236">Log category</span></span>

<span data-ttu-id="4bef2-237">`ILogger`建立物件時，會指定*分類*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-237">When an `ILogger` object is created, a *category* is specified.</span></span> <span data-ttu-id="4bef2-238">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-238">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="4bef2-239">類別字串是任意的，但慣例是使用類別名稱。</span><span class="sxs-lookup"><span data-stu-id="4bef2-239">The category string is arbitrary, but the convention is to use the class name.</span></span> <span data-ttu-id="4bef2-240">例如，在控制器中，名稱可能是 `"TodoApi.Controllers.TodoController"` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-240">For example, in a controller the name might be `"TodoApi.Controllers.TodoController"`.</span></span> <span data-ttu-id="4bef2-241">ASP.NET Core web 應用程式 `ILogger<T>` 會使用來自動取得 `ILogger` 使用的完整類型名稱 `T` 做為分類的實例：</span><span class="sxs-lookup"><span data-stu-id="4bef2-241">The ASP.NET Core web apps use `ILogger<T>` to automatically get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="4bef2-242">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="4bef2-242">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Pages/Contact.cshtml.cs?name=snippet)]

<span data-ttu-id="4bef2-243">`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-243">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

<a name="llvl"></a>

## <a name="log-level"></a><span data-ttu-id="4bef2-244">記錄層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-244">Log level</span></span>

<span data-ttu-id="4bef2-245">下表列出 <xref:Microsoft.Extensions.Logging.LogLevel> 值、便利性 `Log{LogLevel}` 擴充方法，以及建議的使用方式：</span><span class="sxs-lookup"><span data-stu-id="4bef2-245">The following table lists the <xref:Microsoft.Extensions.Logging.LogLevel> values, the convenience `Log{LogLevel}` extension method, and the suggested usage:</span></span>

| <span data-ttu-id="4bef2-246">LogLevel</span><span class="sxs-lookup"><span data-stu-id="4bef2-246">LogLevel</span></span> | <span data-ttu-id="4bef2-247">值</span><span class="sxs-lookup"><span data-stu-id="4bef2-247">Value</span></span> | <span data-ttu-id="4bef2-248">方法</span><span class="sxs-lookup"><span data-stu-id="4bef2-248">Method</span></span> | <span data-ttu-id="4bef2-249">描述</span><span class="sxs-lookup"><span data-stu-id="4bef2-249">Description</span></span> |
| -------- | ----- | ------ | ----------- |
| [<span data-ttu-id="4bef2-250">追蹤</span><span class="sxs-lookup"><span data-stu-id="4bef2-250">Trace</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-251">0</span><span class="sxs-lookup"><span data-stu-id="4bef2-251">0</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogTrace%2A> | <span data-ttu-id="4bef2-252">包含最詳細的訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-252">Contain the most detailed messages.</span></span> <span data-ttu-id="4bef2-253">這些訊息可能包含敏感性應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="4bef2-253">These messages may contain sensitive app data.</span></span> <span data-ttu-id="4bef2-254">這些訊息預設為停用，且***不***應在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-254">These messages are disabled by default and should ***not*** be enabled in production.</span></span> |
| [<span data-ttu-id="4bef2-255">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-255">Debug</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-256">1</span><span class="sxs-lookup"><span data-stu-id="4bef2-256">1</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug%2A> | <span data-ttu-id="4bef2-257">用於偵錯工具和開發。</span><span class="sxs-lookup"><span data-stu-id="4bef2-257">For debugging and development.</span></span> <span data-ttu-id="4bef2-258">請在生產環境中小心使用，因為有大量的數量。</span><span class="sxs-lookup"><span data-stu-id="4bef2-258">Use with caution in production due to the high volume.</span></span> |
| [<span data-ttu-id="4bef2-259">資訊</span><span class="sxs-lookup"><span data-stu-id="4bef2-259">Information</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-260">2</span><span class="sxs-lookup"><span data-stu-id="4bef2-260">2</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation%2A> | <span data-ttu-id="4bef2-261">追蹤應用程式的一般流程。</span><span class="sxs-lookup"><span data-stu-id="4bef2-261">Tracks the general flow of the app.</span></span> <span data-ttu-id="4bef2-262">可能具有長期值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-262">May have long-term value.</span></span> |
| [<span data-ttu-id="4bef2-263">警告</span><span class="sxs-lookup"><span data-stu-id="4bef2-263">Warning</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-264">3</span><span class="sxs-lookup"><span data-stu-id="4bef2-264">3</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> | <span data-ttu-id="4bef2-265">針對異常或非預期的事件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-265">For abnormal or unexpected events.</span></span> <span data-ttu-id="4bef2-266">通常會包含不會導致應用程式失敗的錯誤或狀況。</span><span class="sxs-lookup"><span data-stu-id="4bef2-266">Typically includes errors or conditions that don't cause the app to fail.</span></span> |
| [<span data-ttu-id="4bef2-267">錯誤</span><span class="sxs-lookup"><span data-stu-id="4bef2-267">Error</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-268">4</span><span class="sxs-lookup"><span data-stu-id="4bef2-268">4</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> | <span data-ttu-id="4bef2-269">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4bef2-269">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="4bef2-270">這些訊息表示目前的作業或要求失敗，而不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="4bef2-270">These messages indicate a failure in the current operation or request, not an app-wide failure.</span></span> |
| [<span data-ttu-id="4bef2-271">嚴重</span><span class="sxs-lookup"><span data-stu-id="4bef2-271">Critical</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-272">5</span><span class="sxs-lookup"><span data-stu-id="4bef2-272">5</span></span> | <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogCritical%2A> | <span data-ttu-id="4bef2-273">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="4bef2-273">For failures that require immediate attention.</span></span> <span data-ttu-id="4bef2-274">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="4bef2-274">Examples: data loss scenarios, out of disk space.</span></span> |
| [<span data-ttu-id="4bef2-275">None</span><span class="sxs-lookup"><span data-stu-id="4bef2-275">None</span></span>](xref:Microsoft.Extensions.Logging.LogLevel) | <span data-ttu-id="4bef2-276">6</span><span class="sxs-lookup"><span data-stu-id="4bef2-276">6</span></span> | | <span data-ttu-id="4bef2-277">指定記錄類別不應寫入任何訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-277">Specifies that a logging category should not write any messages.</span></span> |

<span data-ttu-id="4bef2-278">在上表中， `LogLevel` 會從最低到最高的嚴重性列出。</span><span class="sxs-lookup"><span data-stu-id="4bef2-278">In the previous table, the `LogLevel` is listed from lowest to highest severity.</span></span>

<span data-ttu-id="4bef2-279">[記錄](xref:Microsoft.Extensions.Logging.LoggerExtensions)方法的第一個參數， <xref:Microsoft.Extensions.Logging.LogLevel> 表示記錄檔的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-279">The [Log](xref:Microsoft.Extensions.Logging.LoggerExtensions) method's first parameter, <xref:Microsoft.Extensions.Logging.LogLevel>, indicates the severity of the log.</span></span> <span data-ttu-id="4bef2-280">`Log(LogLevel, ...)`大部分的開發人員會呼叫[Log {LogLevel}](xref:Microsoft.Extensions.Logging.LoggerExtensions)擴充方法，而不是呼叫。</span><span class="sxs-lookup"><span data-stu-id="4bef2-280">Rather than calling `Log(LogLevel, ...)`, most developers call the [Log{LogLevel}](xref:Microsoft.Extensions.Logging.LoggerExtensions) extension methods.</span></span> <span data-ttu-id="4bef2-281">`Log{LogLevel}`擴充方法會[呼叫 Log 方法並指定 LogLevel](https://github.com/dotnet/extensions/blob/release/3.1/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-281">The `Log{LogLevel}` extension methods [call the Log method and specify the LogLevel](https://github.com/dotnet/extensions/blob/release/3.1/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span> <span data-ttu-id="4bef2-282">例如，下列兩個記錄呼叫的功能相同，且會產生相同的記錄檔：</span><span class="sxs-lookup"><span data-stu-id="4bef2-282">For example, the following two logging calls are functionally equivalent and produce the same log:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet0&highlight=6-7)]

<span data-ttu-id="4bef2-283">`MyLogEvents.TestItem`這是事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-283">`MyLogEvents.TestItem` is the event ID.</span></span> <span data-ttu-id="4bef2-284">`MyLogEvents`是範例應用程式的一部分，而且會顯示在 [[記錄檔事件識別碼](#leid)] 區段中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-284">`MyLogEvents` is part of the sample app and is displayed in the [Log event ID](#leid) section.</span></span>

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

<span data-ttu-id="4bef2-285">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="4bef2-285">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

<span data-ttu-id="4bef2-286">在上述程式碼中，第一個 `Log{LogLevel}` 參數 `MyLogEvents.GetItem` 是[記錄事件識別碼](#leid)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-286">In the preceding code, the first `Log{LogLevel}` parameter,`MyLogEvents.GetItem`, is the [Log event ID](#leid).</span></span> <span data-ttu-id="4bef2-287">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-287">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="4bef2-288">本檔稍後的「[訊息範本](#lmt)」一節中會說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-288">The method parameters are explained in the [message template](#lmt) section later in this document.</span></span>

<span data-ttu-id="4bef2-289">呼叫適當的 `Log{LogLevel}` 方法，以控制要將多少記錄輸出寫入特定的儲存媒體。</span><span class="sxs-lookup"><span data-stu-id="4bef2-289">Call the appropriate `Log{LogLevel}` method to control how much log output is written to a particular storage medium.</span></span> <span data-ttu-id="4bef2-290">例如：</span><span class="sxs-lookup"><span data-stu-id="4bef2-290">For example:</span></span>

* <span data-ttu-id="4bef2-291">在生產環境中：</span><span class="sxs-lookup"><span data-stu-id="4bef2-291">In production:</span></span>
  * <span data-ttu-id="4bef2-292">在 `Trace` 或 `Information` 層級記錄會產生大量的詳細記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-292">Logging at the `Trace` or `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="4bef2-293">若要控制成本，而不超過資料儲存體限制，請記錄 `Trace` 和 `Information` 層級訊息到高容量、低成本的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-293">To control costs and not exceed data storage limits, log `Trace` and `Information` level messages to a high-volume, low-cost data store.</span></span> <span data-ttu-id="4bef2-294">請考慮 `Trace` 將和限制 `Information` 在特定的類別目錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-294">Consider limiting `Trace` and `Information` to specific categories.</span></span>
  * <span data-ttu-id="4bef2-295">`Warning`透過 `Critical` 層級登入應該會產生幾個記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-295">Logging at `Warning` through `Critical` levels should produce few log messages.</span></span>
    * <span data-ttu-id="4bef2-296">成本和儲存體限制通常不是問題。</span><span class="sxs-lookup"><span data-stu-id="4bef2-296">Costs and storage limits usually aren't a concern.</span></span>
    * <span data-ttu-id="4bef2-297">少數記錄可讓您更有彈性地選擇資料存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-297">Few logs allow more flexibility in data store choices.</span></span>
* <span data-ttu-id="4bef2-298">開發中：</span><span class="sxs-lookup"><span data-stu-id="4bef2-298">In development:</span></span>
  * <span data-ttu-id="4bef2-299">設定為 `Warning`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-299">Set to `Warning`.</span></span>
  * <span data-ttu-id="4bef2-300">`Trace` `Information` 疑難排解時新增或訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-300">Add `Trace` or `Information` messages when troubleshooting.</span></span> <span data-ttu-id="4bef2-301">若要限制輸出， `Trace` 請 `Information` 針對 [調查] 底下的類別設定或。</span><span class="sxs-lookup"><span data-stu-id="4bef2-301">To limit output, set `Trace` or `Information` only for the categories under investigation.</span></span>

<span data-ttu-id="4bef2-302">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-302">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="4bef2-303">例如，請考慮的記錄輸出：</span><span class="sxs-lookup"><span data-stu-id="4bef2-303">For example, consider the log output for:</span></span>

* <span data-ttu-id="4bef2-304">Razor使用 ASP.NET Core 範本建立的頁面應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-304">A Razor Pages app created with the ASP.NET Core templates.</span></span>
* <span data-ttu-id="4bef2-305">記錄設定為`Logging:Console:LogLevel:Microsoft:Information`</span><span class="sxs-lookup"><span data-stu-id="4bef2-305">Logging set to `Logging:Console:LogLevel:Microsoft:Information`</span></span>
* <span data-ttu-id="4bef2-306">流覽至 [隱私權] 頁面：</span><span class="sxs-lookup"><span data-stu-id="4bef2-306">Navigation to the Privacy page:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 GET https://localhost:5001/Privacy
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint '/Privacy'
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[3]
      Route matched with {page = "/Privacy"}. Executing page /Privacy
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[101]
      Executing handler method DefaultRP.Pages.PrivacyModel.OnGet - ModelState is Valid
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[102]
      Executed handler method OnGet, returned result .
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[103]
      Executing an implicit handler method - ModelState is Valid
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[104]
      Executed an implicit handler method, returned result Microsoft.AspNetCore.Mvc.RazorPages.PageResult.
info: Microsoft.AspNetCore.Mvc.RazorPages.Infrastructure.PageActionInvoker[4]
      Executed page /Privacy in 74.5188ms
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint '/Privacy'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 149.3023ms 200 text/html; charset=utf-8
```

<span data-ttu-id="4bef2-307">下列 JSON 集合 `Logging:Console:LogLevel:Microsoft:Information` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-307">The following JSON sets `Logging:Console:LogLevel:Microsoft:Information`:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

<a name="leid"></a>

## <a name="log-event-id"></a><span data-ttu-id="4bef2-308">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="4bef2-308">Log event ID</span></span>

<span data-ttu-id="4bef2-309">每個記錄都可以指定「事件識別碼」\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-309">Each log can specify an *event ID*.</span></span> <span data-ttu-id="4bef2-310">範例應用程式會使用 `MyLogEvents` 類別來定義事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="4bef2-310">The sample app uses the `MyLogEvents` class to define event IDs:</span></span>

<!-- Review: to bad there is no way to use an enum for event ID's -->
[!code-csharp[](index/samples/3.x/TodoApiDTO/Models/MyLogEvents.cs?name=snippet_LoggingEvents)]

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

<span data-ttu-id="4bef2-311">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="4bef2-311">An event ID associates a set of events.</span></span> <span data-ttu-id="4bef2-312">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="4bef2-312">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="4bef2-313">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="4bef2-313">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="4bef2-314">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-314">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="4bef2-315">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="4bef2-315">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoItemsController[1002]
      Getting item 1
warn: TodoApi.Controllers.TodoItemsController[4000]
      Get(1) NOT FOUND
```

<span data-ttu-id="4bef2-316">有些記錄提供者會將事件識別碼儲存在欄位中，這可讓您篩選識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-316">Some logging providers store the event ID in a field, which allows for filtering on the ID.</span></span>

<a name="lmt"></a>

## <a name="log-message-template"></a><span data-ttu-id="4bef2-317">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="4bef2-317">Log message template</span></span>
<!-- Review, Each log API uses a message template. -->
<span data-ttu-id="4bef2-318">每個記錄 API 都會使用訊息範本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-318">Each log API uses a message template.</span></span> <span data-ttu-id="4bef2-319">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="4bef2-319">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="4bef2-320">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="4bef2-320">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet_CallLogMethods&highlight=4,10)]

<span data-ttu-id="4bef2-321">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-321">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="4bef2-322">在下列程式碼中，參數名稱在訊息範本中不是順序的：</span><span class="sxs-lookup"><span data-stu-id="4bef2-322">In the following code, the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "param1";
string p2 = "param2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="4bef2-323">上述程式碼會依序建立具有參數值的記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="4bef2-323">The preceding code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: param1, param2
```

<span data-ttu-id="4bef2-324">這種方法可讓記錄提供者執行[語義或結構化記錄](https://github.com/NLog/NLog/wiki/How-to-use-structured-logging)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-324">This approach allows logging providers to implement [semantic or structured logging](https://github.com/NLog/NLog/wiki/How-to-use-structured-logging).</span></span> <span data-ttu-id="4bef2-325">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-325">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="4bef2-326">這可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="4bef2-326">This enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="4bef2-327">例如，請考慮下列記錄器方法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-327">For example, consider the following logger method:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="4bef2-328">例如，記錄至 Azure 表格儲存體：</span><span class="sxs-lookup"><span data-stu-id="4bef2-328">For example, when logging to Azure Table Storage:</span></span>

* <span data-ttu-id="4bef2-329">每個 Azure 資料表實體都可以有 `ID` 和 `RequestTime` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-329">Each Azure Table entity can have `ID` and `RequestTime` properties.</span></span>
* <span data-ttu-id="4bef2-330">具有屬性的資料表會簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="4bef2-330">Tables with properties simplify queries on logged data.</span></span> <span data-ttu-id="4bef2-331">例如，查詢可以尋找特定範圍內的所有記錄， `RequestTime` 而不需要剖析文字訊息的時間。</span><span class="sxs-lookup"><span data-stu-id="4bef2-331">For example, a query can find all logs within a particular `RequestTime` range without having to parse the time out of the text message.</span></span>

## <a name="log-exceptions"></a><span data-ttu-id="4bef2-332">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="4bef2-332">Log exceptions</span></span>

<span data-ttu-id="4bef2-333">記錄器方法具有接受例外狀況參數的多載：</span><span class="sxs-lookup"><span data-stu-id="4bef2-333">The logger methods have overloads that take an exception parameter:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Exp)]

[!INCLUDE[](~/includes/MyDisplayRouteInfoBoth.md)]

<span data-ttu-id="4bef2-334">例外狀況記錄是提供者特有的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-334">Exception logging is provider-specific.</span></span>

### <a name="default-log-level"></a><span data-ttu-id="4bef2-335">預設記錄層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-335">Default log level</span></span>

<span data-ttu-id="4bef2-336">如果未設定預設記錄層級，則預設的記錄層級值為 `Information` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-336">If the default log level is not set, the default log level value is `Information`.</span></span>

<span data-ttu-id="4bef2-337">例如，請考慮下列 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="4bef2-337">For example, consider the following web app:</span></span>

* <span data-ttu-id="4bef2-338">使用 ASP.NET web 應用程式範本建立。</span><span class="sxs-lookup"><span data-stu-id="4bef2-338">Created with the ASP.NET web app templates.</span></span>
* <span data-ttu-id="4bef2-339">已刪除或重新命名時， *appsettings.json*和*appsettings.Development.js* 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-339">*appsettings.json* and *appsettings.Development.json* deleted or renamed.</span></span>

<span data-ttu-id="4bef2-340">在上述設定中，流覽至 [隱私權] 或 [首頁] `Trace` 會 `Debug` `Information` `Microsoft` 在類別名稱中產生許多、和訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-340">With the preceding setup, navigating to the privacy or home page produces many `Trace`, `Debug`, and `Information`  messages with `Microsoft` in the category name.</span></span>

<span data-ttu-id="4bef2-341">下列程式碼會設定預設記錄層級未設定在 configuration 中時的預設記錄層級：</span><span class="sxs-lookup"><span data-stu-id="4bef2-341">The following code sets the default log level when the default log level is not set in configuration:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_MinLevel&highlight=10)]

<!-- review required: I say this a couple times -->
<span data-ttu-id="4bef2-342">一般來說，記錄層級應指定于設定中，而不是程式碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-342">Generally, log levels should be specified in configuration and not code.</span></span>

### <a name="filter-function"></a><span data-ttu-id="4bef2-343">Filter 函數</span><span class="sxs-lookup"><span data-stu-id="4bef2-343">Filter function</span></span>

<span data-ttu-id="4bef2-344">針對未透過設定或程式碼指派規則的所有提供者和類別，會叫用篩選函數：</span><span class="sxs-lookup"><span data-stu-id="4bef2-344">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterFunction)]

<span data-ttu-id="4bef2-345">上述程式碼會在分類包含 `Controller` 或 `Microsoft` 且記錄層級為 `Information` 或更高時，顯示主控台記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-345">The preceding code displays console logs when the category contains `Controller` or `Microsoft` and the log level is `Information` or higher.</span></span>

<span data-ttu-id="4bef2-346">一般來說，記錄層級應指定于設定中，而不是程式碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-346">Generally, log levels should be specified in configuration and not code.</span></span>

## <a name="aspnet-core-and-ef-core-categories"></a><span data-ttu-id="4bef2-347">ASP.NET Core 和 EF Core 分類</span><span class="sxs-lookup"><span data-stu-id="4bef2-347">ASP.NET Core and EF Core categories</span></span>

<span data-ttu-id="4bef2-348">下表包含 ASP.NET Core 和 Entity Framework Core 所使用的一些分類，以及記錄檔的相關注意事項：</span><span class="sxs-lookup"><span data-stu-id="4bef2-348">The following table contains some categories used by ASP.NET Core and Entity Framework Core, with notes about the logs:</span></span>

| <span data-ttu-id="4bef2-349">類別</span><span class="sxs-lookup"><span data-stu-id="4bef2-349">Category</span></span>                            | <span data-ttu-id="4bef2-350">注意</span><span class="sxs-lookup"><span data-stu-id="4bef2-350">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="4bef2-351">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="4bef2-351">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="4bef2-352">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="4bef2-352">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="4bef2-353">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="4bef2-353">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="4bef2-354">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="4bef2-354">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="4bef2-355">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="4bef2-355">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="4bef2-356">允許主機。</span><span class="sxs-lookup"><span data-stu-id="4bef2-356">Hosts allowed.</span></span> |
| <span data-ttu-id="4bef2-357">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="4bef2-357">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="4bef2-358">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="4bef2-358">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="4bef2-359">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-359">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="4bef2-360">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="4bef2-360">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="4bef2-361">MVC 和 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="4bef2-361">MVC and Razor diagnostics.</span></span> <span data-ttu-id="4bef2-362">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="4bef2-362">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="4bef2-363">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="4bef2-363">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="4bef2-364">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-364">Route matching information.</span></span> |
| <span data-ttu-id="4bef2-365">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="4bef2-365">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="4bef2-366">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="4bef2-366">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="4bef2-367">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-367">HTTPS certificate information.</span></span> |
| <span data-ttu-id="4bef2-368">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="4bef2-368">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="4bef2-369">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-369">Files served.</span></span> |
| <span data-ttu-id="4bef2-370">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="4bef2-370">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="4bef2-371">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="4bef2-371">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="4bef2-372">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="4bef2-372">Database activity and configuration, change detection, migrations.</span></span> |

<span data-ttu-id="4bef2-373">若要在主控台視窗中查看更多類別，請將**appsettings.Development.js**設定為下列內容：</span><span class="sxs-lookup"><span data-stu-id="4bef2-373">To view more categories in the console window, set **appsettings.Development.json** to the following:</span></span>

[!code-json[](index/samples/3.x/MyMain/appsettings.Trace.json)]

<!-- Review: What other providers support scopes? Console is not generally used in staging/production  -->

<a name="logscopes"></a>

## <a name="log-scopes"></a><span data-ttu-id="4bef2-374">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="4bef2-374">Log scopes</span></span>

 <span data-ttu-id="4bef2-375">「範圍」\*\* 可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="4bef2-375">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="4bef2-376">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-376">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="4bef2-377">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-377">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="4bef2-378">範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-378">A scope:</span></span>

* <span data-ttu-id="4bef2-379">是 <xref:System.IDisposable> 方法所傳回的類型 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-379">Is an <xref:System.IDisposable> type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method.</span></span>
* <span data-ttu-id="4bef2-380">會持續到處置為止。</span><span class="sxs-lookup"><span data-stu-id="4bef2-380">Lasts until it's disposed.</span></span>

<span data-ttu-id="4bef2-381">下列提供者支援範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-381">The following providers support scopes:</span></span>

* `Console`
* [<span data-ttu-id="4bef2-382">AzureAppServicesFile 和 AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="4bef2-382">AzureAppServicesFile and AzureAppServicesBlob</span></span>](xref:Microsoft.Extensions.Logging.AzureAppServices.BatchingLoggerOptions.IncludeScopes)

<span data-ttu-id="4bef2-383">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-383">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Controllers/TestController.cs?name=snippet_Scopes)]

<span data-ttu-id="4bef2-384">下列 JSON 會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-384">The following JSON enables scopes for the console provider:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.Scopes.json)]

<span data-ttu-id="4bef2-385">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-385">The following code enables scopes for the console provider:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_Scopes)]

<span data-ttu-id="4bef2-386">一般來說，您應該在設定中指定記錄，而不是在程式碼中指定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-386">Generally, logging should be specified in configuration and not code.</span></span>

<a name="bilp"></a>

## <a name="built-in-logging-providers"></a><span data-ttu-id="4bef2-387">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-387">Built-in logging providers</span></span>

<span data-ttu-id="4bef2-388">ASP.NET Core 包括下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="4bef2-388">ASP.NET Core includes the following logging providers:</span></span>

* [<span data-ttu-id="4bef2-389">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-389">Console</span></span>](#console)
* [<span data-ttu-id="4bef2-390">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-390">Debug</span></span>](#debug)
* [<span data-ttu-id="4bef2-391">EventSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-391">EventSource</span></span>](#event-source)
* [<span data-ttu-id="4bef2-392">EventLog</span><span class="sxs-lookup"><span data-stu-id="4bef2-392">EventLog</span></span>](#welog)
* [<span data-ttu-id="4bef2-393">AzureAppServicesFile 和 AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="4bef2-393">AzureAppServicesFile and AzureAppServicesBlob</span></span>](#azure-app-service)
* [<span data-ttu-id="4bef2-394">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="4bef2-394">ApplicationInsights</span></span>](#azure-application-insights)

<span data-ttu-id="4bef2-395">如需有關 `stdout` 使用 ASP.NET Core 模組的記錄和偵錯工具的詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection> 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-395">For information on `stdout` and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console"></a><span data-ttu-id="4bef2-396">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-396">Console</span></span>

<span data-ttu-id="4bef2-397">`Console`提供者會將輸出記錄到主控台。</span><span class="sxs-lookup"><span data-stu-id="4bef2-397">The `Console` provider logs output to the console.</span></span> <span data-ttu-id="4bef2-398">如需有關在開發中查看記錄的詳細資訊 `Console` ，請參閱[dotnet run 和 Visual Studio 的記錄輸出](#dnrvs)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-398">For more information on viewing `Console` logs in development, see [Logging output from dotnet run and Visual Studio](#dnrvs).</span></span>

### <a name="debug"></a><span data-ttu-id="4bef2-399">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-399">Debug</span></span>

<span data-ttu-id="4bef2-400">`Debug`提供者會使用 system.servicemodel 類別來寫入[System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug)記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="4bef2-400">The `Debug` provider writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class.</span></span> <span data-ttu-id="4bef2-401">呼叫以 `System.Diagnostics.Debug.WriteLine` 寫入 `Debug` 提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-401">Calls to `System.Diagnostics.Debug.WriteLine` write to the `Debug` provider.</span></span>

<span data-ttu-id="4bef2-402">在 Linux 上， `Debug` 提供者記錄檔位置與散發相依，而且可能是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="4bef2-402">On Linux, the `Debug` provider log location is distribution-dependent and may be one of the following:</span></span>

* <span data-ttu-id="4bef2-403">*/var/log/message*</span><span class="sxs-lookup"><span data-stu-id="4bef2-403">*/var/log/message*</span></span>
* <span data-ttu-id="4bef2-404">*/var/log/syslog*</span><span class="sxs-lookup"><span data-stu-id="4bef2-404">*/var/log/syslog*</span></span>

### <a name="event-source"></a><span data-ttu-id="4bef2-405">事件來源</span><span class="sxs-lookup"><span data-stu-id="4bef2-405">Event Source</span></span>

<span data-ttu-id="4bef2-406">`EventSource`提供者會寫入名為的跨平臺事件來源 `Microsoft-Extensions-Logging` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-406">The `EventSource` provider writes to a cross-platform event source with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="4bef2-407">在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-407">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="4bef2-408">dotnet 追蹤工具</span><span class="sxs-lookup"><span data-stu-id="4bef2-408">dotnet trace tooling</span></span>

<span data-ttu-id="4bef2-409">[Dotnet 追蹤](/dotnet/core/diagnostics/dotnet-trace)工具是一種跨平臺 CLI 全域工具，可讓您收集執行中進程的 .net Core 追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bef2-409">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="4bef2-410">此工具會 <xref:Microsoft.Extensions.Logging.EventSource> 使用來收集提供者資料 <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource> 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-410">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="4bef2-411">如需安裝指示，請參閱[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-411">See [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) for installation instructions.</span></span>

<span data-ttu-id="4bef2-412">使用 dotnet 追蹤工具，從應用程式收集追蹤：</span><span class="sxs-lookup"><span data-stu-id="4bef2-412">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="4bef2-413">使用命令執行應用程式 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-413">Run the app with the `dotnet run` command.</span></span>
1. <span data-ttu-id="4bef2-414">判斷 .NET Core 應用程式的處理序識別碼（PID）：</span><span class="sxs-lookup"><span data-stu-id="4bef2-414">Determine the process identifier (PID) of the .NET Core app:</span></span>
   * <span data-ttu-id="4bef2-415">在 Windows 上，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-415">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="4bef2-416">工作管理員（Ctrl + Alt + Del）</span><span class="sxs-lookup"><span data-stu-id="4bef2-416">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="4bef2-417">tasklist 命令</span><span class="sxs-lookup"><span data-stu-id="4bef2-417">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="4bef2-418">取得進程 Powershell 命令</span><span class="sxs-lookup"><span data-stu-id="4bef2-418">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="4bef2-419">在 Linux 上，請使用[pidof 命令](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-419">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="4bef2-420">尋找與應用程式元件同名之進程的 PID。</span><span class="sxs-lookup"><span data-stu-id="4bef2-420">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="4bef2-421">執行 `dotnet trace` 命令。</span><span class="sxs-lookup"><span data-stu-id="4bef2-421">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="4bef2-422">一般命令語法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-422">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="4bef2-423">使用 PowerShell 命令 shell 時，將 `--providers` 值以單引號括住（ `'` ）：</span><span class="sxs-lookup"><span data-stu-id="4bef2-423">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="4bef2-424">在非 Windows 平臺上，新增 `-f speedscope` 選項以將輸出追蹤檔案的格式變更為 `speedscope` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-424">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="4bef2-425">關鍵字</span><span class="sxs-lookup"><span data-stu-id="4bef2-425">Keyword</span></span> | <span data-ttu-id="4bef2-426">描述</span><span class="sxs-lookup"><span data-stu-id="4bef2-426">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="4bef2-427">1</span><span class="sxs-lookup"><span data-stu-id="4bef2-427">1</span></span>       | <span data-ttu-id="4bef2-428">記錄有關的中繼事件 `LoggingEventSource` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-428">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="4bef2-429">不會記錄來自的事件 `ILogger` ）。</span><span class="sxs-lookup"><span data-stu-id="4bef2-429">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="4bef2-430">2</span><span class="sxs-lookup"><span data-stu-id="4bef2-430">2</span></span>       | <span data-ttu-id="4bef2-431">`Message`當呼叫時，開啟事件 `ILogger.Log()` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-431">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="4bef2-432">以程式設計方式（未格式化）提供資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-432">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="4bef2-433">4</span><span class="sxs-lookup"><span data-stu-id="4bef2-433">4</span></span>       | <span data-ttu-id="4bef2-434">`FormatMessage`當呼叫時，開啟事件 `ILogger.Log()` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-434">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="4bef2-435">提供資訊的格式化字串版本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-435">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="4bef2-436">8</span><span class="sxs-lookup"><span data-stu-id="4bef2-436">8</span></span>       | <span data-ttu-id="4bef2-437">`MessageJson`當呼叫時，開啟事件 `ILogger.Log()` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-437">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="4bef2-438">提供引數的 JSON 標記法。</span><span class="sxs-lookup"><span data-stu-id="4bef2-438">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="4bef2-439">事件層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-439">Event Level</span></span> | <span data-ttu-id="4bef2-440">描述</span><span class="sxs-lookup"><span data-stu-id="4bef2-440">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="4bef2-441">0</span><span class="sxs-lookup"><span data-stu-id="4bef2-441">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="4bef2-442">1</span><span class="sxs-lookup"><span data-stu-id="4bef2-442">1</span></span>           | `Critical`      |
   | <span data-ttu-id="4bef2-443">2</span><span class="sxs-lookup"><span data-stu-id="4bef2-443">2</span></span>           | `Error`         |
   | <span data-ttu-id="4bef2-444">3</span><span class="sxs-lookup"><span data-stu-id="4bef2-444">3</span></span>           | `Warning`       |
   | <span data-ttu-id="4bef2-445">4</span><span class="sxs-lookup"><span data-stu-id="4bef2-445">4</span></span>           | `Informational` |
   | <span data-ttu-id="4bef2-446">5</span><span class="sxs-lookup"><span data-stu-id="4bef2-446">5</span></span>           | `Verbose`       |

   <span data-ttu-id="4bef2-447">`FilterSpecs`和的 `{Logger Category}` 專案 `{Event Level}` 代表其他記錄篩選準則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-447">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="4bef2-448">`FilterSpecs`以分號（）分隔專案 `;` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-448">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="4bef2-449">使用 Windows 命令 shell 的範例（值前後**沒有**單引號 `--providers` ）：</span><span class="sxs-lookup"><span data-stu-id="4bef2-449">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="4bef2-450">上述命令會啟用：</span><span class="sxs-lookup"><span data-stu-id="4bef2-450">The preceding command activates:</span></span>

   * <span data-ttu-id="4bef2-451">事件來源記錄器，用來產生 `4` 錯誤（）的格式化字串（） `2` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-451">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="4bef2-452">`Microsoft.AspNetCore.Hosting`在 `Informational` 記錄層級進行記錄（ `4` ）。</span><span class="sxs-lookup"><span data-stu-id="4bef2-452">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="4bef2-453">按 Enter 鍵或 Ctrl + C 來停止 dotnet 追蹤工具。</span><span class="sxs-lookup"><span data-stu-id="4bef2-453">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="4bef2-454">追蹤會以名稱*nettrace*儲存在命令執行所在的資料夾中 `dotnet trace` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-454">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="4bef2-455">使用[Perfview](#perfview)開啟追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bef2-455">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="4bef2-456">開啟*nettrace*檔案，並流覽追蹤事件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-456">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="4bef2-457">如果應用程式未使用建立主機 `CreateDefaultBuilder` ，請將[事件來源提供者](#event-source-provider)新增至應用程式的記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-457">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

<span data-ttu-id="4bef2-458">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="4bef2-458">For more information, see:</span></span>

* <span data-ttu-id="4bef2-459">[效能分析公用程式追蹤（dotnet-追蹤）](/dotnet/core/diagnostics/dotnet-trace) （.net Core 檔）</span><span class="sxs-lookup"><span data-stu-id="4bef2-459">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="4bef2-460">[效能分析公用程式追蹤（dotnet 追蹤）](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) （dotnet/診斷 GitHub 存放庫檔）</span><span class="sxs-lookup"><span data-stu-id="4bef2-460">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="4bef2-461">[LoggingEventSource 類別](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource)（.Net API 瀏覽器）</span><span class="sxs-lookup"><span data-stu-id="4bef2-461">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="4bef2-462">[LoggingEventSource 參考來源（3.0）](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs)：若要取得不同版本的參考來源，請將分支變更為 `release/{Version}` ，其中 `{Version}` 是所需 ASP.NET Core 的版本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-462">[LoggingEventSource reference source (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs): To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="4bef2-463">[Perfview](#perfview)：適用于查看事件來源追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bef2-463">[Perfview](#perfview): Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="4bef2-464">Perfview</span><span class="sxs-lookup"><span data-stu-id="4bef2-464">Perfview</span></span>

<span data-ttu-id="4bef2-465">使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-465">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="4bef2-466">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="4bef2-466">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="4bef2-467">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]\*\*\*\* 清單</span><span class="sxs-lookup"><span data-stu-id="4bef2-467">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="4bef2-468">請不要錯過 `*` 字串開頭的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-468">Don't miss the `*` at the start of the string.</span></span>

<a name="welog"></a>

### <a name="windows-eventlog"></a><span data-ttu-id="4bef2-469">Windows EventLog</span><span class="sxs-lookup"><span data-stu-id="4bef2-469">Windows EventLog</span></span>

<span data-ttu-id="4bef2-470">`EventLog`提供者會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="4bef2-470">The `EventLog` provider sends log output to the Windows Event Log.</span></span> <span data-ttu-id="4bef2-471">與其他提供者不同的是， `EventLog` 提供者***不***會繼承預設的非提供者設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-471">Unlike the other providers, the `EventLog` provider does ***not*** inherit the default non-provider settings.</span></span> <span data-ttu-id="4bef2-472">如果 `EventLog` 未指定記錄檔設定，它們會[預設為 LogLevel。](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L99-L103)</span><span class="sxs-lookup"><span data-stu-id="4bef2-472">If `EventLog` log settings aren't specified, they [default to LogLevel.Warning](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L99-L103).</span></span>

<span data-ttu-id="4bef2-473">若要記錄低於 <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> 的事件，請明確設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-473">To log events lower than <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType>, explicitly set the log level.</span></span> <span data-ttu-id="4bef2-474">下列範例會將事件記錄檔的預設記錄層級設定為 <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-474">The following example sets the Event Log default log level to <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>:</span></span>

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

<span data-ttu-id="4bef2-475">[AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)多載可以傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings> 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-475">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) can pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="4bef2-476">若 `null` 未指定，則會使用下列預設設定：</span><span class="sxs-lookup"><span data-stu-id="4bef2-476">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="4bef2-477">`LogName`：「應用程式」</span><span class="sxs-lookup"><span data-stu-id="4bef2-477">`LogName`: "Application"</span></span>
* <span data-ttu-id="4bef2-478">`SourceName`： ".NET Runtime"</span><span class="sxs-lookup"><span data-stu-id="4bef2-478">`SourceName`: ".NET Runtime"</span></span>
* <span data-ttu-id="4bef2-479">`MachineName`：使用本機電腦名稱稱。</span><span class="sxs-lookup"><span data-stu-id="4bef2-479">`MachineName`: The local machine name is used.</span></span>

<span data-ttu-id="4bef2-480">下列程式碼會將的 `SourceName` 預設值變更 `".NET Runtime"` 為 `MyLogs` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-480">The following code changes the `SourceName` from the default value of `".NET Runtime"` to `MyLogs`:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippetEventLog)]

### <a name="azure-app-service"></a><span data-ttu-id="4bef2-481">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4bef2-481">Azure App Service</span></span>

<span data-ttu-id="4bef2-482">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-482">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

<span data-ttu-id="4bef2-483">該提供者套件並未包含在共用架構中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-483">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="4bef2-484">若要使用提供者，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-484">To use the provider, add the provider package to the project.</span></span>

<span data-ttu-id="4bef2-485">如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：</span><span class="sxs-lookup"><span data-stu-id="4bef2-485">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_AzLogOptions)]

<span data-ttu-id="4bef2-486">當部署到 Azure App Service 時，應用程式會使用 Azure 入口網站 [ **App Service** ] 頁面的 [ [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows)] 區段中的設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-486">When deployed to Azure App Service, the app uses the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="4bef2-487">當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-487">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="4bef2-488">**應用程式記錄 (檔案系統)**</span><span class="sxs-lookup"><span data-stu-id="4bef2-488">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="4bef2-489">**應用程式記錄 (Blob)**</span><span class="sxs-lookup"><span data-stu-id="4bef2-489">**Application Logging (Blob)**</span></span>

<span data-ttu-id="4bef2-490">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-490">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="4bef2-491">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="4bef2-491">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="4bef2-492">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-492">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="4bef2-493">此提供者只會在專案于 Azure 環境中執行時記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-493">This provider only logs when the project runs in the Azure environment.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="4bef2-494">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="4bef2-494">Azure log streaming</span></span>

<span data-ttu-id="4bef2-495">Azure 記錄串流支援從下列時間即時查看記錄活動：</span><span class="sxs-lookup"><span data-stu-id="4bef2-495">Azure log streaming supports viewing log activity in real time from:</span></span>

* <span data-ttu-id="4bef2-496">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="4bef2-496">The app server</span></span>
* <span data-ttu-id="4bef2-497">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="4bef2-497">The web server</span></span>
* <span data-ttu-id="4bef2-498">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="4bef2-498">Failed request tracing</span></span>

<span data-ttu-id="4bef2-499">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="4bef2-499">To configure Azure log streaming:</span></span>

* <span data-ttu-id="4bef2-500">從應用程式的入口網站頁面流覽至 [ **App Service 記錄**] 頁面。</span><span class="sxs-lookup"><span data-stu-id="4bef2-500">Navigate to the **App Service logs** page from the app's portal page.</span></span>
* <span data-ttu-id="4bef2-501">將 [應用程式記錄 (檔案系統)]\*\*\*\* 設定為 [開啟]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-501">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="4bef2-502">選擇記錄 [層級]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-502">Choose the log **Level**.</span></span> <span data-ttu-id="4bef2-503">此設定僅適用于 Azure 記錄串流。</span><span class="sxs-lookup"><span data-stu-id="4bef2-503">This setting only applies to Azure log streaming.</span></span>

<span data-ttu-id="4bef2-504">流覽至 [**記錄資料流程**] 頁面以查看記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-504">Navigate to the **Log Stream** page to view logs.</span></span> <span data-ttu-id="4bef2-505">記錄的訊息會以介面記錄 `ILogger` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-505">The logged messages are logged with the `ILogger` interface.</span></span>

### <a name="azure-application-insights"></a><span data-ttu-id="4bef2-506">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="4bef2-506">Azure Application Insights</span></span>

<span data-ttu-id="4bef2-507">[ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights)提供者套件會將記錄寫入[Azure 應用程式深入](/azure/azure-monitor/app/cloudservices)解析。</span><span class="sxs-lookup"><span data-stu-id="4bef2-507">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to [Azure Application Insights](/azure/azure-monitor/app/cloudservices).</span></span> <span data-ttu-id="4bef2-508">Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。</span><span class="sxs-lookup"><span data-stu-id="4bef2-508">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="4bef2-509">如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-509">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="4bef2-510">記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-510">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="4bef2-511">如果您使用此套件，就不需安裝提供者套件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-511">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="4bef2-512">[ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)套件適用于 ASP.NET 4.x，而不是 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="4bef2-512">The [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package is for ASP.NET 4.x, not ASP.NET Core.</span></span>

<span data-ttu-id="4bef2-513">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="4bef2-513">For more information, see the following resources:</span></span>

* [<span data-ttu-id="4bef2-514">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="4bef2-514">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="4bef2-515">[適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="4bef2-515">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="4bef2-516">[適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="4bef2-516">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="4bef2-517">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-517">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="4bef2-518">[安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。</span><span class="sxs-lookup"><span data-stu-id="4bef2-518">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="4bef2-519">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-519">Third-party logging providers</span></span>

<span data-ttu-id="4bef2-520">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="4bef2-520">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="4bef2-521">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-521">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="4bef2-522">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-522">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="4bef2-523">[JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="4bef2-523">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="4bef2-524">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="4bef2-524">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="4bef2-525">[Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）</span><span class="sxs-lookup"><span data-stu-id="4bef2-525">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="4bef2-526">[Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-526">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="4bef2-527">[NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-527">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="4bef2-528">[PLogger](https://www.nuget.org/packages/InvertedSoftware.PLogger.Core/) （[GitHub](https://github.com/invertedsoftware/InvertedSoftware.PLogger.Core)存放庫）</span><span class="sxs-lookup"><span data-stu-id="4bef2-528">[PLogger](https://www.nuget.org/packages/InvertedSoftware.PLogger.Core/) ([GitHub repo](https://github.com/invertedsoftware/InvertedSoftware.PLogger.Core))</span></span>
* <span data-ttu-id="4bef2-529">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="4bef2-529">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="4bef2-530">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="4bef2-530">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="4bef2-531">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="4bef2-531">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="4bef2-532">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-532">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="4bef2-533">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="4bef2-533">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="4bef2-534">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-534">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="4bef2-535">呼叫 `ILoggerFactory` 記錄架構所提供的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="4bef2-535">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="4bef2-536">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-536">For more information, see each provider's documentation.</span></span> <span data-ttu-id="4bef2-537">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-537">Third-party logging providers aren't supported by Microsoft.</span></span>

<a name="nhca"></a>

## <a name="non-host-console-app"></a><span data-ttu-id="4bef2-538">非主機主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="4bef2-538">Non-host console app</span></span>

<span data-ttu-id="4bef2-539">如需如何在非 web 主控台應用程式中使用泛型主機的範例，請參閱[背景工作範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples)的*Program.cs*檔案（ <xref:fundamentals/host/hosted-services> ）。</span><span class="sxs-lookup"><span data-stu-id="4bef2-539">For an example of how to use the Generic Host in a non-web console app, see the *Program.cs* file of the [Background Tasks sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span></span>

<span data-ttu-id="4bef2-540">不含一般主機的應用程式記錄程式碼，會因[新增提供者](#add-providers)和[建立記錄器](#create-logs)的方式而有所不同。</span><span class="sxs-lookup"><span data-stu-id="4bef2-540">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> 

### <a name="logging-providers"></a><span data-ttu-id="4bef2-541">記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-541">Logging providers</span></span>

<span data-ttu-id="4bef2-542">在非主機主控台應用程式中，於建立 `LoggerFactory` 時呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-542">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11-12)]

### <a name="create-logs"></a><span data-ttu-id="4bef2-543">建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-543">Create logs</span></span>

<span data-ttu-id="4bef2-544">若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-544">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="4bef2-545">使用 `LoggerFactory` 來建立 `ILogger` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-545">Use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="4bef2-546">下列範例會使用 `LoggingConsoleApp.Program` 做為類別目錄來建立記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-546">The following example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=14)]

<span data-ttu-id="4bef2-547">在下列 ASP.NET 核心範例中，記錄器是用來建立記錄，並以 `Information` 做為層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-547">In the following ASP.NET CORE examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="4bef2-548">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-548">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=15)]

<span data-ttu-id="4bef2-549">本檔會更詳細地說明[層級](#log-level)和[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-549">[Levels](#log-level) and [categories](#log-category) are explained in more detail in this document.</span></span>

<a name="lhc"></a>

## <a name="log-during-host-construction"></a><span data-ttu-id="4bef2-550">在主機結構中記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-550">Log during host construction</span></span>

<span data-ttu-id="4bef2-551">不直接支援在主機結構期間進行記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-551">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="4bef2-552">不過，您可以使用個別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-552">However, a separate logger can be used.</span></span> <span data-ttu-id="4bef2-553">在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateHostBuilder`。 </span><span class="sxs-lookup"><span data-stu-id="4bef2-553">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="4bef2-554">`AddSerilog`會使用中指定的靜態設定 `Log.Logger` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-554">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

<a name="csdi"></a>

## <a name="configure-a-service-that-depends-on-ilogger"></a><span data-ttu-id="4bef2-555">設定相依于 ILogger 的服務</span><span class="sxs-lookup"><span data-stu-id="4bef2-555">Configure a service that depends on ILogger</span></span>

<span data-ttu-id="4bef2-556">記錄器的建構函式插入 `Startup` 適用於舊版 ASP.NET Core，因為會為 Web 主機建立個別的 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-556">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="4bef2-557">如需為何只為一般主機建立一個容器的資訊，請參閱[重大變更公告](https://github.com/aspnet/Announcements/issues/353)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-557">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="4bef2-558">若要設定相依于的服務 `ILogger<T>` ，請使用「處理常式」插入或提供 factory 方法。</span><span class="sxs-lookup"><span data-stu-id="4bef2-558">To configure a service that depends on `ILogger<T>`, use constructor injection or provide a factory method.</span></span> <span data-ttu-id="4bef2-559">只有在沒有其他選項時，才建議使用 Factory 方法。</span><span class="sxs-lookup"><span data-stu-id="4bef2-559">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="4bef2-560">例如，假設有一個服務需要 DI 所 `ILogger<T>` 提供的實例：</span><span class="sxs-lookup"><span data-stu-id="4bef2-560">For example, consider a service that needs an `ILogger<T>` instance provided by DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup2.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="4bef2-561">上述反白顯示的程式碼是在第一次使用 DI 容器來建立實例時，所執行的[Func](/dotnet/api/system.func-2) `MyService` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-561">The preceding highlighted code is a [Func](/dotnet/api/system.func-2) that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="4bef2-562">您可以用此方式存取任何已註冊的服務。</span><span class="sxs-lookup"><span data-stu-id="4bef2-562">You can access any of the registered services in this way.</span></span>

<a name="clms"></a>

## <a name="create-logs-in-main"></a><span data-ttu-id="4bef2-563">在 Main 中建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-563">Create logs in Main</span></span>

<span data-ttu-id="4bef2-564">下列程式碼會在 `Main` `ILogger` 建立主機之後，從 DI 取得實例來登入：</span><span class="sxs-lookup"><span data-stu-id="4bef2-564">The following code logs in `Main` by getting an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Program.cs?name=snippet_LogProgram)]

### <a name="create-logs-in-startup"></a><span data-ttu-id="4bef2-565">在啟動中建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-565">Create logs in Startup</span></span>

<span data-ttu-id="4bef2-566">下列程式碼會寫入中的記錄 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-566">The following code writes logs in `Startup.Configure`:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiDTO/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="4bef2-567">在以 `Startup.ConfigureServices` 方法完成 DI 容器設定之前，不支援寫入記錄檔：</span><span class="sxs-lookup"><span data-stu-id="4bef2-567">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="4bef2-568">不支援將記錄器插入 `Startup` 建構函式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-568">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="4bef2-569">不支援將記錄器插入 `Startup.ConfigureServices` 方法簽章</span><span class="sxs-lookup"><span data-stu-id="4bef2-569">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="4bef2-570">這項限制的原因是記錄相依於 DI 和組態，而後者相依於 DI。</span><span class="sxs-lookup"><span data-stu-id="4bef2-570">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="4bef2-571">在 `ConfigureServices` 完成後才會設定 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-571">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="4bef2-572">如需設定服務的相關資訊，而這項服務會相依于 `ILogger<T>` 或為何要 `Startup` 在舊版中處理記錄器的程式，請參閱[設定相依于 ILogger 的服務](#csdi)</span><span class="sxs-lookup"><span data-stu-id="4bef2-572">For information on configuring a service that depends on `ILogger<T>` or why constructor injection of a logger into `Startup` worked in earlier versions, see [Configure a service that depends on ILogger](#csdi)</span></span>

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="4bef2-573">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="4bef2-573">No asynchronous logger methods</span></span>

<span data-ttu-id="4bef2-574">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="4bef2-574">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="4bef2-575">如果記錄資料存放區的速度很慢，請不要直接寫入。</span><span class="sxs-lookup"><span data-stu-id="4bef2-575">If a logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="4bef2-576">請考慮一開始先將記錄檔訊息寫入快速存放區，然後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-576">Consider writing the log messages to a fast store initially, then moving them to the slow store later.</span></span> <span data-ttu-id="4bef2-577">例如，當記錄到 SQL Server 時，請不要直接在方法中執行這項操作 `Log` ，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-577">For example, when logging to SQL Server, don't do so directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="4bef2-578">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="4bef2-578">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="4bef2-579">如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="4bef2-579">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

<a name="clib"></a>

## <a name="change-log-levels-in-a-running-app"></a><span data-ttu-id="4bef2-580">變更執行中應用程式的記錄層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-580">Change log levels in a running app</span></span>

<span data-ttu-id="4bef2-581">記錄 API 不包含在應用程式執行時變更記錄層級的案例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-581">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="4bef2-582">不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-582">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="4bef2-583">例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)預設會重載記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-583">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), reloads logging configuration by default.</span></span> <span data-ttu-id="4bef2-584">如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-584">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

## <a name="ilogger-and-iloggerfactory"></a><span data-ttu-id="4bef2-585">ILogger 和 ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="4bef2-585">ILogger and ILoggerFactory</span></span>

<span data-ttu-id="4bef2-586"><xref:Microsoft.Extensions.Logging.ILogger%601>和 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 介面和實作為包含在 .NET Core SDK 中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-586">The <xref:Microsoft.Extensions.Logging.ILogger%601> and <xref:Microsoft.Extensions.Logging.ILoggerFactory> interfaces and implementations are included in the .NET Core SDK.</span></span> <span data-ttu-id="4bef2-587">下列 NuGet 套件也提供這些功能：</span><span class="sxs-lookup"><span data-stu-id="4bef2-587">They are also available in the following NuGet packages:</span></span>  

* <span data-ttu-id="4bef2-588">介面位於[Microsoft. Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-588">The interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/).</span></span>
* <span data-ttu-id="4bef2-589">預設的實作為 [[記錄](https://www.nuget.org/packages/microsoft.extensions.logging/)]。</span><span class="sxs-lookup"><span data-stu-id="4bef2-589">The default implementations are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

<!-- review. Why would you want to hard code filtering rules in code? -->
<a name="fric"></a>

## <a name="apply-log-filter-rules-in-code"></a><span data-ttu-id="4bef2-590">在程式碼中套用記錄篩選規則</span><span class="sxs-lookup"><span data-stu-id="4bef2-590">Apply log filter rules in code</span></span>

<span data-ttu-id="4bef2-591">設定記錄篩選規則的慣用方法是使用[Configuration](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-591">The preferred approach for setting log filter rules is by using [Configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="4bef2-592">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="4bef2-592">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/3.x/MyMain/Program.cs?name=snippet_FilterInCode)]

<span data-ttu-id="4bef2-593">`logging.AddFilter("System", LogLevel.Debug)`指定 `System` 類別目錄和記錄層級 `Debug` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-593">`logging.AddFilter("System", LogLevel.Debug)` specifies the `System` category and log level `Debug`.</span></span> <span data-ttu-id="4bef2-594">篩選準則會套用至所有提供者，因為未設定特定的提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-594">The filter is applied to all providers because a specific provider was not configured.</span></span>

<span data-ttu-id="4bef2-595">`AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Information)`指出</span><span class="sxs-lookup"><span data-stu-id="4bef2-595">`AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Information)` specifies:</span></span>

* <span data-ttu-id="4bef2-596">`Debug`記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-596">The `Debug` logging provider.</span></span>
* <span data-ttu-id="4bef2-597">記錄層級 `Information` 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-597">Log level `Information` and higher.</span></span>
* <span data-ttu-id="4bef2-598">所有以為開頭的分類 `"Microsoft"` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-598">All categories starting with `"Microsoft"`.</span></span>

## <a name="create-a-custom-logger"></a><span data-ttu-id="4bef2-599">建立自訂記錄器</span><span class="sxs-lookup"><span data-stu-id="4bef2-599">Create a custom logger</span></span>

<span data-ttu-id="4bef2-600">若要新增自訂記錄器，請新增 <xref:Microsoft.Extensions.Logging.ILoggerProvider> 具有 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 下列內容的：</span><span class="sxs-lookup"><span data-stu-id="4bef2-600">To add a custom logger, add an <xref:Microsoft.Extensions.Logging.ILoggerProvider> with <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

```csharp
public void Configure(
    IApplicationBuilder app,
    IWebHostEnvironment env,
    ILoggerFactory loggerFactory)
{
    loggerFactory.AddProvider(new CustomLoggerProvider(new CustomLoggerConfiguration()));
```

<span data-ttu-id="4bef2-601">會 `ILoggerProvider` 建立一或多個 `ILogger` 實例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-601">The `ILoggerProvider` creates one or more `ILogger` instances.</span></span> <span data-ttu-id="4bef2-602">`ILogger`架構會使用實例來記錄資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-602">The `ILogger` instances are used by the framework to log the information.</span></span>

### <a name="sample-custom-logger-configuration"></a><span data-ttu-id="4bef2-603">範例自訂記錄器設定</span><span class="sxs-lookup"><span data-stu-id="4bef2-603">Sample custom logger configuration</span></span>

<span data-ttu-id="4bef2-604">此範例：</span><span class="sxs-lookup"><span data-stu-id="4bef2-604">The sample:</span></span>

* <span data-ttu-id="4bef2-605">的設計是非常基本的範例，會依照事件識別碼和記錄層級設定記錄主控台的色彩。</span><span class="sxs-lookup"><span data-stu-id="4bef2-605">Is designed to be a very basic sample that sets the color of the log console by event ID and log level.</span></span> <span data-ttu-id="4bef2-606">記錄器通常不會因為事件識別碼而變更，而且不是記錄層級特有的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-606">Loggers generally don't change by event ID and are not specific to log level.</span></span>
* <span data-ttu-id="4bef2-607">使用下列設定類型，為每個記錄層級和事件識別碼建立不同的色彩主控台專案：</span><span class="sxs-lookup"><span data-stu-id="4bef2-607">Creates different color console entries per log level and event ID using the following configuration type:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerConfiguration.cs?name=snippet)]

<span data-ttu-id="4bef2-608">上述程式碼會將預設層級設為 `Warning` ，並將色彩設定為 `Yellow` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-608">The preceding code sets the default level to `Warning` and the color to `Yellow`.</span></span> <span data-ttu-id="4bef2-609">如果 `EventId` 設定為0，我們將會記錄所有事件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-609">If the `EventId` is set to 0, we will log all events.</span></span>

### <a name="create-the-custom-logger"></a><span data-ttu-id="4bef2-610">建立自訂記錄器</span><span class="sxs-lookup"><span data-stu-id="4bef2-610">Create the custom logger</span></span>

<span data-ttu-id="4bef2-611">`ILogger`執行分類名稱通常是記錄來源。</span><span class="sxs-lookup"><span data-stu-id="4bef2-611">The `ILogger` implementation category name is typically the logging source.</span></span> <span data-ttu-id="4bef2-612">例如，建立記錄器的類型：</span><span class="sxs-lookup"><span data-stu-id="4bef2-612">For example, the type where the logger is created:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLogger.cs?name=snippet)]

<span data-ttu-id="4bef2-613">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="4bef2-613">The preceding code:</span></span>

* <span data-ttu-id="4bef2-614">建立每個類別目錄名稱的記錄器實例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-614">Creates a logger instance per category name.</span></span>
* <span data-ttu-id="4bef2-615">簽 `logLevel == _config.LogLevel` 入 `IsEnabled` ，讓每個 `logLevel` 都有唯一的記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-615">Checks `logLevel == _config.LogLevel` in `IsEnabled`, so each `logLevel` has a unique logger.</span></span> <span data-ttu-id="4bef2-616">通常，記錄器也應該針對所有較高的記錄層級啟用：</span><span class="sxs-lookup"><span data-stu-id="4bef2-616">Generally, loggers should also be enabled for all higher log levels:</span></span>

```csharp
public bool IsEnabled(LogLevel logLevel)
{
    return logLevel >= _config.LogLevel;
}
```

### <a name="create-the-custom-loggerprovider"></a><span data-ttu-id="4bef2-617">建立自訂 LoggerProvider</span><span class="sxs-lookup"><span data-stu-id="4bef2-617">Create the custom LoggerProvider</span></span>

<span data-ttu-id="4bef2-618">`LoggerProvider`是建立記錄器實例的類別。</span><span class="sxs-lookup"><span data-stu-id="4bef2-618">The `LoggerProvider` is the class that creates the logger instances.</span></span> <span data-ttu-id="4bef2-619">可能不需要為每個類別建立記錄器實例，但這對某些記錄器（例如 NLog 或 log4net）而言是合理的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-619">Maybe it is not needed to create a logger instance per category, but this makes sense for some Loggers, like NLog or log4net.</span></span> <span data-ttu-id="4bef2-620">如此一來，您也可以視需要選擇每個類別的不同記錄輸出目標：</span><span class="sxs-lookup"><span data-stu-id="4bef2-620">Doing this you are also able to choose different logging output targets per category if needed:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerProvider.cs?name=snippet)]

<span data-ttu-id="4bef2-621">在上述程式碼中， <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger*> `ColorConsoleLogger` 會建立每個分類名稱的單一實例，並將其儲存在中 [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2) 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-621">In the preceding code, <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger*> creates a single instance of the `ColorConsoleLogger` per category name and stores it in the [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2);</span></span>

### <a name="usage-and-registration-of-the-custom-logger"></a><span data-ttu-id="4bef2-622">自訂記錄器的使用方式和註冊</span><span class="sxs-lookup"><span data-stu-id="4bef2-622">Usage and registration of the custom logger</span></span>

<span data-ttu-id="4bef2-623">在中註冊記錄器 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-623">Register the logger in the `Startup.Configure`:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/Startup.cs?name=snippet)]

<span data-ttu-id="4bef2-624">針對上述程式碼，為提供至少一個擴充方法 `ILoggerFactory` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-624">For the preceding code, provide at least one extension method for the `ILoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/CustomLogger/ColorConsoleLogger/ColorConsoleLoggerExtensions.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="4bef2-625">其他資源</span><span class="sxs-lookup"><span data-stu-id="4bef2-625">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
* <span data-ttu-id="4bef2-626">記錄錯誤應該會在[github.com/dotnet/runtime/](https://github.com/dotnet/runtime/issues)存放庫中建立。</span><span class="sxs-lookup"><span data-stu-id="4bef2-626">Logging bugs should be created in the [github.com/dotnet/runtime/](https://github.com/dotnet/runtime/issues) repo.</span></span>
* <xref:blazor/fundamentals/logging>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4bef2-627">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4bef2-627">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4bef2-628">.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-628">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="4bef2-629">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="4bef2-629">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="4bef2-630">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="4bef2-630">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="4bef2-631">新增提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-631">Add providers</span></span>

<span data-ttu-id="4bef2-632">記錄提供者會顯示或儲存記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-632">A logging provider displays or stores logs.</span></span> <span data-ttu-id="4bef2-633">例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-633">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="4bef2-634">您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。</span><span class="sxs-lookup"><span data-stu-id="4bef2-634">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

<span data-ttu-id="4bef2-635">若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-635">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="4bef2-636">上述程式碼需要對 `Microsoft.Extensions.Logging` 與 `Microsoft.Extensions.Configuration` 的參考。</span><span class="sxs-lookup"><span data-stu-id="4bef2-636">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="4bef2-637">預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="4bef2-637">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="4bef2-638">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-638">Console</span></span>
* <span data-ttu-id="4bef2-639">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-639">Debug</span></span>
* <span data-ttu-id="4bef2-640">EventSource (從 ASP.NET Core 2.2 開始)</span><span class="sxs-lookup"><span data-stu-id="4bef2-640">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="4bef2-641">若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-641">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="4bef2-642">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-642">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

<span data-ttu-id="4bef2-643">在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-643">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="4bef2-644">建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-644">Create logs</span></span>

<span data-ttu-id="4bef2-645">若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-645">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="4bef2-646">在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-646">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="4bef2-647">在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-647">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="4bef2-648">下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-648">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="4bef2-649">記錄「類別」\*\* 是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="4bef2-649">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="4bef2-650">由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-650">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="4bef2-651">在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-651">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="4bef2-652">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-652">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

<span data-ttu-id="4bef2-653">此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-653">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span>

### <a name="create-logs-in-startup"></a><span data-ttu-id="4bef2-654">在啟動中建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-654">Create logs in Startup</span></span>

<span data-ttu-id="4bef2-655">若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：</span><span class="sxs-lookup"><span data-stu-id="4bef2-655">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="4bef2-656">在 Program 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-656">Create logs in the Program class</span></span>

<span data-ttu-id="4bef2-657">若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="4bef2-657">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="4bef2-658">不直接支援在主機結構期間進行記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-658">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="4bef2-659">不過，您可以使用個別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-659">However, a separate logger can be used.</span></span> <span data-ttu-id="4bef2-660">在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateWebHostBuilder`。 </span><span class="sxs-lookup"><span data-stu-id="4bef2-660">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="4bef2-661">`AddSerilog`會使用中指定的靜態設定 `Log.Logger` ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-661">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="4bef2-662">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="4bef2-662">No asynchronous logger methods</span></span>

<span data-ttu-id="4bef2-663">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="4bef2-663">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="4bef2-664">若您的記錄資料存放區很慢，請不要直接寫入其中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-664">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="4bef2-665">請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-665">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="4bef2-666">例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-666">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="4bef2-667">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="4bef2-667">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="4bef2-668">如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="4bef2-668">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="4bef2-669">組態</span><span class="sxs-lookup"><span data-stu-id="4bef2-669">Configuration</span></span>

<span data-ttu-id="4bef2-670">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="4bef2-670">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="4bef2-671">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-671">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="4bef2-672">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-672">Command-line arguments.</span></span>
* <span data-ttu-id="4bef2-673">環境變數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-673">Environment variables.</span></span>
* <span data-ttu-id="4bef2-674">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-674">In-memory .NET objects.</span></span>
* <span data-ttu-id="4bef2-675">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="4bef2-675">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="4bef2-676">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-676">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="4bef2-677">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-677">Custom providers (installed or created).</span></span>

<span data-ttu-id="4bef2-678">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="4bef2-678">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="4bef2-679">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="4bef2-679">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="4bef2-680">`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-680">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="4bef2-681">`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-681">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="4bef2-682">在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-682">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="4bef2-683">`Logging` 下的其他屬性可指定記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-683">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="4bef2-684">範例使用主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-684">The example is for the Console provider.</span></span> <span data-ttu-id="4bef2-685">如果提供者支援[記錄範圍](#log-scopes)，則 `IncludeScopes` 指出是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-685">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="4bef2-686">提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-686">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="4bef2-687">提供者下的 `LogLevel` 會指定提供者的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-687">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="4bef2-688">若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="4bef2-688">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span> <span data-ttu-id="4bef2-689">例如，請考慮下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="4bef2-689">For example, consider the following JSON:</span></span>

[!code-json[](index/samples/3.x/TodoApiDTO/appsettings.MSFT.json)]

<span data-ttu-id="4bef2-690">在先前的 JSON 中， `Console` 提供者設定會覆寫先前的（預設）記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-690">In the preceding JSON, the `Console` provider settings overrides the preceding (default) log level.</span></span>

<span data-ttu-id="4bef2-691">記錄 API 不包含在應用程式執行時變更記錄層級的案例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-691">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="4bef2-692">不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-692">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="4bef2-693">例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)（由新增） `CreateDefaultBuilder` 以讀取設定檔案，預設會重載記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-693">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="4bef2-694">如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-694">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="4bef2-695">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="4bef2-695">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="4bef2-696">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="4bef2-696">Sample logging output</span></span>

<span data-ttu-id="4bef2-697">使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-697">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="4bef2-698">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="4bef2-698">Here's an example of console output:</span></span>

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

<span data-ttu-id="4bef2-699">上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。</span><span class="sxs-lookup"><span data-stu-id="4bef2-699">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="4bef2-700">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="4bef2-700">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="4bef2-701">這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApi" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="4bef2-701">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="4bef2-702">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-702">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="4bef2-703">ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-703">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="4bef2-704">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="4bef2-704">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="4bef2-705">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="4bef2-705">NuGet packages</span></span>

<span data-ttu-id="4bef2-706">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-706">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="4bef2-707">記錄分類</span><span class="sxs-lookup"><span data-stu-id="4bef2-707">Log category</span></span>

<span data-ttu-id="4bef2-708">建立 `ILogger` 物件時，會為它指定「類別」\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-708">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="4bef2-709">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="4bef2-709">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="4bef2-710">類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="4bef2-710">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="4bef2-711">使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：</span><span class="sxs-lookup"><span data-stu-id="4bef2-711">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="4bef2-712">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="4bef2-712">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="4bef2-713">`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-713">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="4bef2-714">記錄層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-714">Log level</span></span>

<span data-ttu-id="4bef2-715">每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-715">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="4bef2-716">記錄層級表示嚴重性或重要性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-716">The log level indicates the severity or importance.</span></span> <span data-ttu-id="4bef2-717">例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-717">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="4bef2-718">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="4bef2-718">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4bef2-719">在上述程式碼中， `MyLogEvents.GetItem` 和 `MyLogEvents.GetItemNotFound` 參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-719">In the preceding code, the `MyLogEvents.GetItem` and `MyLogEvents.GetItemNotFound` parameters are the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="4bef2-720">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-720">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="4bef2-721">在本文的[記錄訊息範本一節](#lmt)中會說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="4bef2-721">The method parameters are explained in the [Log message template section](#lmt) in this article.</span></span>

<span data-ttu-id="4bef2-722">在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-722">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="4bef2-723">這些方法會呼叫接受 `Log` 參數的 `LogLevel`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-723">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="4bef2-724">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="4bef2-724">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="4bef2-725">如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-725">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="4bef2-726">ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="4bef2-726">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="4bef2-727">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="4bef2-727">Trace = 0</span></span>

  <span data-ttu-id="4bef2-728">針對通常只對偵錯有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-728">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="4bef2-729">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-729">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="4bef2-730">*預設為停用。*</span><span class="sxs-lookup"><span data-stu-id="4bef2-730">*Disabled by default.*</span></span>

* <span data-ttu-id="4bef2-731">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="4bef2-731">Debug = 1</span></span>

  <span data-ttu-id="4bef2-732">針對可在開發與偵錯中使用的資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-732">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="4bef2-733">範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-733">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="4bef2-734">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="4bef2-734">Information = 2</span></span>

  <span data-ttu-id="4bef2-735">針對一般應用程式流程的追蹤。</span><span class="sxs-lookup"><span data-stu-id="4bef2-735">For tracking the general flow of the app.</span></span> <span data-ttu-id="4bef2-736">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-736">These logs typically have some long-term value.</span></span> <span data-ttu-id="4bef2-737">範例： `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="4bef2-737">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="4bef2-738">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="4bef2-738">Warning = 3</span></span>

  <span data-ttu-id="4bef2-739">針對應用程式流程中發生的異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-739">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="4bef2-740">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="4bef2-740">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="4bef2-741">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="4bef2-741">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="4bef2-742">範例： `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="4bef2-742">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="4bef2-743">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="4bef2-743">Error = 4</span></span>

  <span data-ttu-id="4bef2-744">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4bef2-744">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="4bef2-745">這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="4bef2-745">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="4bef2-746">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="4bef2-746">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="4bef2-747">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="4bef2-747">Critical = 5</span></span>

  <span data-ttu-id="4bef2-748">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="4bef2-748">For failures that require immediate attention.</span></span> <span data-ttu-id="4bef2-749">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="4bef2-749">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="4bef2-750">使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="4bef2-750">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="4bef2-751">例如：</span><span class="sxs-lookup"><span data-stu-id="4bef2-751">For example:</span></span>

* <span data-ttu-id="4bef2-752">在生產環境中：</span><span class="sxs-lookup"><span data-stu-id="4bef2-752">In production:</span></span>
  * <span data-ttu-id="4bef2-753">`Trace`透過層級的記錄會 `Information` 產生大量的詳細記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-753">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="4bef2-754">若要控制成本，而不超過資料儲存體限制，請將 `Trace` `Information` 層級訊息記錄到高容量、低成本的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-754">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="4bef2-755">`Warning`透過 `Critical` 層級登入通常會產生較少、較小的記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-755">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="4bef2-756">因此，成本和儲存體限制通常不會造成問題，因此可讓您更靈活地選擇資料存放區。</span><span class="sxs-lookup"><span data-stu-id="4bef2-756">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="4bef2-757">在開發期間：</span><span class="sxs-lookup"><span data-stu-id="4bef2-757">During development:</span></span>
  * <span data-ttu-id="4bef2-758">`Warning` `Critical` 將訊息記錄到主控台。</span><span class="sxs-lookup"><span data-stu-id="4bef2-758">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="4bef2-759">`Trace` `Information` 進行疑難排解時，新增訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-759">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="4bef2-760">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-760">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="4bef2-761">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-761">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="4bef2-762">此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-762">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="4bef2-763">以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：</span><span class="sxs-lookup"><span data-stu-id="4bef2-763">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="4bef2-764">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="4bef2-764">Log event ID</span></span>

<span data-ttu-id="4bef2-765">每個記錄都可以指定「事件識別碼」\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-765">Each log can specify an *event ID*.</span></span> <span data-ttu-id="4bef2-766">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="4bef2-766">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="4bef2-767">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="4bef2-767">An event ID associates a set of events.</span></span> <span data-ttu-id="4bef2-768">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="4bef2-768">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="4bef2-769">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="4bef2-769">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="4bef2-770">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-770">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="4bef2-771">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="4bef2-771">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="4bef2-772">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="4bef2-772">Log message template</span></span>

<span data-ttu-id="4bef2-773">每個記錄都會指定訊息範本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-773">Each log specifies a message template.</span></span> <span data-ttu-id="4bef2-774">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="4bef2-774">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="4bef2-775">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="4bef2-775">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="4bef2-776">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="4bef2-776">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="4bef2-777">請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：</span><span class="sxs-lookup"><span data-stu-id="4bef2-777">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="4bef2-778">此程式碼會使用依順序的參數值建立記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="4bef2-778">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="4bef2-779">記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-779">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="4bef2-780">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="4bef2-780">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="4bef2-781">此資訊可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="4bef2-781">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="4bef2-782">例如，假設記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="4bef2-782">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="4bef2-783">若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="4bef2-783">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="4bef2-784">查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。</span><span class="sxs-lookup"><span data-stu-id="4bef2-784">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="4bef2-785">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="4bef2-785">Logging exceptions</span></span>

<span data-ttu-id="4bef2-786">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4bef2-786">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="4bef2-787">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="4bef2-787">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="4bef2-788">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-788">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="4bef2-789">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="4bef2-789">Log filtering</span></span>

<span data-ttu-id="4bef2-790">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-790">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="4bef2-791">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="4bef2-791">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="4bef2-792">若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-792">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="4bef2-793">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-793">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="4bef2-794">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="4bef2-794">Create filter rules in configuration</span></span>

<span data-ttu-id="4bef2-795">專案範本程式碼會呼叫 `CreateDefaultBuilder` 以設定主控台、Debug 和 EventSource （ASP.NET Core 2.2 或更新版本）提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-795">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="4bef2-796">`CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-796">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="4bef2-797">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4bef2-797">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="4bef2-798">此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-798">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="4bef2-799">建立 `ILogger` 物件時，會為每個提供者選取一個規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-799">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="4bef2-800">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="4bef2-800">Filter rules in code</span></span>

<span data-ttu-id="4bef2-801">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="4bef2-801">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="4bef2-802">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-802">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="4bef2-803">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-803">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="4bef2-804">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="4bef2-804">How filtering rules are applied</span></span>

<span data-ttu-id="4bef2-805">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-805">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="4bef2-806">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="4bef2-806">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="4bef2-807">Number</span><span class="sxs-lookup"><span data-stu-id="4bef2-807">Number</span></span> | <span data-ttu-id="4bef2-808">提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-808">Provider</span></span>      | <span data-ttu-id="4bef2-809">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="4bef2-809">Categories that begin with ...</span></span>          | <span data-ttu-id="4bef2-810">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-810">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="4bef2-811">1</span><span class="sxs-lookup"><span data-stu-id="4bef2-811">1</span></span>      | <span data-ttu-id="4bef2-812">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-812">Debug</span></span>         | <span data-ttu-id="4bef2-813">所有類別</span><span class="sxs-lookup"><span data-stu-id="4bef2-813">All categories</span></span>                          | <span data-ttu-id="4bef2-814">資訊</span><span class="sxs-lookup"><span data-stu-id="4bef2-814">Information</span></span>       |
| <span data-ttu-id="4bef2-815">2</span><span class="sxs-lookup"><span data-stu-id="4bef2-815">2</span></span>      | <span data-ttu-id="4bef2-816">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-816">Console</span></span>       | <span data-ttu-id="4bef2-817">AspNetCore Razor 。內部</span><span class="sxs-lookup"><span data-stu-id="4bef2-817">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="4bef2-818">警告</span><span class="sxs-lookup"><span data-stu-id="4bef2-818">Warning</span></span>           |
| <span data-ttu-id="4bef2-819">3</span><span class="sxs-lookup"><span data-stu-id="4bef2-819">3</span></span>      | <span data-ttu-id="4bef2-820">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-820">Console</span></span>       | <span data-ttu-id="4bef2-821">AspNetCore Razor 。Razor</span><span class="sxs-lookup"><span data-stu-id="4bef2-821">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="4bef2-822">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-822">Debug</span></span>             |
| <span data-ttu-id="4bef2-823">4</span><span class="sxs-lookup"><span data-stu-id="4bef2-823">4</span></span>      | <span data-ttu-id="4bef2-824">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-824">Console</span></span>       | <span data-ttu-id="4bef2-825">AspNetCore。Razor</span><span class="sxs-lookup"><span data-stu-id="4bef2-825">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="4bef2-826">錯誤</span><span class="sxs-lookup"><span data-stu-id="4bef2-826">Error</span></span>             |
| <span data-ttu-id="4bef2-827">5</span><span class="sxs-lookup"><span data-stu-id="4bef2-827">5</span></span>      | <span data-ttu-id="4bef2-828">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-828">Console</span></span>       | <span data-ttu-id="4bef2-829">所有類別</span><span class="sxs-lookup"><span data-stu-id="4bef2-829">All categories</span></span>                          | <span data-ttu-id="4bef2-830">資訊</span><span class="sxs-lookup"><span data-stu-id="4bef2-830">Information</span></span>       |
| <span data-ttu-id="4bef2-831">6</span><span class="sxs-lookup"><span data-stu-id="4bef2-831">6</span></span>      | <span data-ttu-id="4bef2-832">所有提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-832">All providers</span></span> | <span data-ttu-id="4bef2-833">所有類別</span><span class="sxs-lookup"><span data-stu-id="4bef2-833">All categories</span></span>                          | <span data-ttu-id="4bef2-834">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-834">Debug</span></span>             |
| <span data-ttu-id="4bef2-835">7</span><span class="sxs-lookup"><span data-stu-id="4bef2-835">7</span></span>      | <span data-ttu-id="4bef2-836">所有提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-836">All providers</span></span> | <span data-ttu-id="4bef2-837">系統</span><span class="sxs-lookup"><span data-stu-id="4bef2-837">System</span></span>                                  | <span data-ttu-id="4bef2-838">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-838">Debug</span></span>             |
| <span data-ttu-id="4bef2-839">8</span><span class="sxs-lookup"><span data-stu-id="4bef2-839">8</span></span>      | <span data-ttu-id="4bef2-840">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-840">Debug</span></span>         | <span data-ttu-id="4bef2-841">Microsoft</span><span class="sxs-lookup"><span data-stu-id="4bef2-841">Microsoft</span></span>                               | <span data-ttu-id="4bef2-842">追蹤</span><span class="sxs-lookup"><span data-stu-id="4bef2-842">Trace</span></span>             |

<span data-ttu-id="4bef2-843">建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="4bef2-843">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="4bef2-844">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="4bef2-844">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="4bef2-845">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-845">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="4bef2-846">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="4bef2-846">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="4bef2-847">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-847">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="4bef2-848">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-848">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="4bef2-849">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-849">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="4bef2-850">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-850">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="4bef2-851">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="4bef2-851">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="4bef2-852">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="4bef2-852">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="4bef2-853">在上述的規則清單中，假設您建立了 `ILogger` 類別 "AspNetCore Razor Razor " 的物件。ViewEngine":</span><span class="sxs-lookup"><span data-stu-id="4bef2-853">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="4bef2-854">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-854">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="4bef2-855">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="4bef2-855">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="4bef2-856">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="4bef2-856">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="4bef2-857">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="4bef2-857">Rule 3 is most specific.</span></span>

<span data-ttu-id="4bef2-858">產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-858">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="4bef2-859">`Debug` 層級以上的記錄會傳送到主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-859">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="4bef2-860">提供者別名</span><span class="sxs-lookup"><span data-stu-id="4bef2-860">Provider aliases</span></span>

<span data-ttu-id="4bef2-861">每個提供者都會定義「別名」\*\*，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="4bef2-861">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="4bef2-862">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="4bef2-862">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="4bef2-863">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-863">Console</span></span>
* <span data-ttu-id="4bef2-864">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-864">Debug</span></span>
* <span data-ttu-id="4bef2-865">EventSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-865">EventSource</span></span>
* <span data-ttu-id="4bef2-866">EventLog</span><span class="sxs-lookup"><span data-stu-id="4bef2-866">EventLog</span></span>
* <span data-ttu-id="4bef2-867">TraceSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-867">TraceSource</span></span>
* <span data-ttu-id="4bef2-868">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="4bef2-868">AzureAppServicesFile</span></span>
* <span data-ttu-id="4bef2-869">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="4bef2-869">AzureAppServicesBlob</span></span>
* <span data-ttu-id="4bef2-870">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="4bef2-870">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="4bef2-871">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-871">Default minimum level</span></span>

<span data-ttu-id="4bef2-872">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="4bef2-872">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="4bef2-873">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="4bef2-873">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="4bef2-874">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-874">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="4bef2-875">篩選函數</span><span class="sxs-lookup"><span data-stu-id="4bef2-875">Filter functions</span></span>

<span data-ttu-id="4bef2-876">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-876">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="4bef2-877">函式中的程式碼可以存取提供者類型、類別與記錄層級。</span><span class="sxs-lookup"><span data-stu-id="4bef2-877">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="4bef2-878">例如：</span><span class="sxs-lookup"><span data-stu-id="4bef2-878">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a><span data-ttu-id="4bef2-879">系統類別與層級</span><span class="sxs-lookup"><span data-stu-id="4bef2-879">System categories and levels</span></span>

<span data-ttu-id="4bef2-880">以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：</span><span class="sxs-lookup"><span data-stu-id="4bef2-880">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="4bef2-881">類別</span><span class="sxs-lookup"><span data-stu-id="4bef2-881">Category</span></span>                            | <span data-ttu-id="4bef2-882">注意</span><span class="sxs-lookup"><span data-stu-id="4bef2-882">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="4bef2-883">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="4bef2-883">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="4bef2-884">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="4bef2-884">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="4bef2-885">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="4bef2-885">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="4bef2-886">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="4bef2-886">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="4bef2-887">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="4bef2-887">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="4bef2-888">允許主機。</span><span class="sxs-lookup"><span data-stu-id="4bef2-888">Hosts allowed.</span></span> |
| <span data-ttu-id="4bef2-889">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="4bef2-889">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="4bef2-890">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="4bef2-890">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="4bef2-891">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-891">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="4bef2-892">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="4bef2-892">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="4bef2-893">MVC 和 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="4bef2-893">MVC and Razor diagnostics.</span></span> <span data-ttu-id="4bef2-894">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="4bef2-894">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="4bef2-895">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="4bef2-895">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="4bef2-896">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-896">Route matching information.</span></span> |
| <span data-ttu-id="4bef2-897">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="4bef2-897">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="4bef2-898">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="4bef2-898">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="4bef2-899">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="4bef2-899">HTTPS certificate information.</span></span> |
| <span data-ttu-id="4bef2-900">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="4bef2-900">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="4bef2-901">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-901">Files served.</span></span> |
| <span data-ttu-id="4bef2-902">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="4bef2-902">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="4bef2-903">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="4bef2-903">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="4bef2-904">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="4bef2-904">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="4bef2-905">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="4bef2-905">Log scopes</span></span>

 <span data-ttu-id="4bef2-906">「範圍」\*\* 可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="4bef2-906">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="4bef2-907">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-907">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="4bef2-908">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="4bef2-908">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="4bef2-909">範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="4bef2-909">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="4bef2-910">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-910">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="4bef2-911">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="4bef2-911">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="4bef2-912">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="4bef2-912">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="4bef2-913">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-913">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="4bef2-914">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="4bef2-914">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="4bef2-915">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="4bef2-915">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="4bef2-916">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-916">Built-in logging providers</span></span>

<span data-ttu-id="4bef2-917">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="4bef2-917">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="4bef2-918">主控台</span><span class="sxs-lookup"><span data-stu-id="4bef2-918">Console</span></span>](#console-provider)
* [<span data-ttu-id="4bef2-919">偵錯</span><span class="sxs-lookup"><span data-stu-id="4bef2-919">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="4bef2-920">EventSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-920">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="4bef2-921">EventLog</span><span class="sxs-lookup"><span data-stu-id="4bef2-921">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="4bef2-922">TraceSource</span><span class="sxs-lookup"><span data-stu-id="4bef2-922">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="4bef2-923">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="4bef2-923">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="4bef2-924">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="4bef2-924">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="4bef2-925">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="4bef2-925">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="4bef2-926">如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="4bef2-926">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="4bef2-927">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-927">Console provider</span></span>

<span data-ttu-id="4bef2-928">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="4bef2-928">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="4bef2-929">若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4bef2-929">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="4bef2-930">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-930">Debug provider</span></span>

<span data-ttu-id="4bef2-931">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="4bef2-931">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="4bef2-932">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-932">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="4bef2-933">事件來源提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-933">Event Source provider</span></span>

<span data-ttu-id="4bef2-934">在跨平臺的事件來源中，會寫入名為的[記錄](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) `Microsoft-Extensions-Logging` 檔。</span><span class="sxs-lookup"><span data-stu-id="4bef2-934">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="4bef2-935">在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-935">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="4bef2-936">呼叫以建立主機時，會自動新增事件來源提供者 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="4bef2-936">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

<span data-ttu-id="4bef2-937">使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-937">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="4bef2-938">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="4bef2-938">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="4bef2-939">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]\*\*\*\* 清單</span><span class="sxs-lookup"><span data-stu-id="4bef2-939">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="4bef2-940">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-940">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="4bef2-942">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-942">Windows EventLog provider</span></span>

<span data-ttu-id="4bef2-943">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="4bef2-943">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="4bef2-944">[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。</span><span class="sxs-lookup"><span data-stu-id="4bef2-944">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="4bef2-945">若 `null` 未指定，則會使用下列預設設定：</span><span class="sxs-lookup"><span data-stu-id="4bef2-945">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="4bef2-946">`LogName`：「應用程式」</span><span class="sxs-lookup"><span data-stu-id="4bef2-946">`LogName`: "Application"</span></span>
* <span data-ttu-id="4bef2-947">`SourceName`： ".NET Runtime"</span><span class="sxs-lookup"><span data-stu-id="4bef2-947">`SourceName`: ".NET Runtime"</span></span>
* <span data-ttu-id="4bef2-948">`MachineName`：使用本機電腦名稱稱。</span><span class="sxs-lookup"><span data-stu-id="4bef2-948">`MachineName`: The local machine name is used.</span></span>

<span data-ttu-id="4bef2-949">記錄[警告層級和更新版本](#log-level)的事件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-949">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="4bef2-950">下列範例會將事件記錄檔的預設記錄層級設定為 <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> ：</span><span class="sxs-lookup"><span data-stu-id="4bef2-950">The following example sets the Event Log default log level to <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>:</span></span>

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="4bef2-951">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-951">TraceSource provider</span></span>

<span data-ttu-id="4bef2-952">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-952">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="4bef2-953">[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="4bef2-953">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="4bef2-954">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="4bef2-954">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="4bef2-955">該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。</span><span class="sxs-lookup"><span data-stu-id="4bef2-955">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="4bef2-956">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-956">Azure App Service provider</span></span>

<span data-ttu-id="4bef2-957">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-957">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="4bef2-958">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-958">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4bef2-959">當以 .NET Framework 為目標或是參考 `Microsoft.AspNetCore.App` 中繼套件時，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-959">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

<span data-ttu-id="4bef2-960"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。</span><span class="sxs-lookup"><span data-stu-id="4bef2-960">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="4bef2-961">設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。</span><span class="sxs-lookup"><span data-stu-id="4bef2-961">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="4bef2-962">(*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)</span><span class="sxs-lookup"><span data-stu-id="4bef2-962">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

<span data-ttu-id="4bef2-963">當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]\*\*\*\* 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。</span><span class="sxs-lookup"><span data-stu-id="4bef2-963">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="4bef2-964">當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bef2-964">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="4bef2-965">**應用程式記錄 (檔案系統)**</span><span class="sxs-lookup"><span data-stu-id="4bef2-965">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="4bef2-966">**應用程式記錄 (Blob)**</span><span class="sxs-lookup"><span data-stu-id="4bef2-966">**Application Logging (Blob)**</span></span>

<span data-ttu-id="4bef2-967">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-967">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="4bef2-968">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="4bef2-968">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="4bef2-969">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-969">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="4bef2-970">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="4bef2-970">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="4bef2-971">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="4bef2-971">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="4bef2-972">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="4bef2-972">Azure log streaming</span></span>

<span data-ttu-id="4bef2-973">Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="4bef2-973">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="4bef2-974">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="4bef2-974">The app server</span></span>
* <span data-ttu-id="4bef2-975">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="4bef2-975">The web server</span></span>
* <span data-ttu-id="4bef2-976">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="4bef2-976">Failed request tracing</span></span>

<span data-ttu-id="4bef2-977">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="4bef2-977">To configure Azure log streaming:</span></span>

* <span data-ttu-id="4bef2-978">從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-978">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="4bef2-979">將 [應用程式記錄 (檔案系統)]\*\*\*\* 設定為 [開啟]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-979">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="4bef2-980">選擇記錄 [層級]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="4bef2-980">Choose the log **Level**.</span></span> <span data-ttu-id="4bef2-981">此設定僅適用于 Azure 記錄串流，而不適用於應用程式中的其他記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-981">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="4bef2-982">瀏覽到 [記錄資料流]\*\*\*\* 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-982">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="4bef2-983">這些是應用程式透過 `ILogger` 介面產生的訊息。</span><span class="sxs-lookup"><span data-stu-id="4bef2-983">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="4bef2-984">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="4bef2-984">Azure Application Insights trace logging</span></span>

<span data-ttu-id="4bef2-985">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。</span><span class="sxs-lookup"><span data-stu-id="4bef2-985">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="4bef2-986">Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。</span><span class="sxs-lookup"><span data-stu-id="4bef2-986">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="4bef2-987">如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。</span><span class="sxs-lookup"><span data-stu-id="4bef2-987">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="4bef2-988">記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-988">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="4bef2-989">如果您使用此套件，就不需安裝提供者套件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-989">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="4bef2-990">不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="4bef2-990">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="4bef2-991">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="4bef2-991">For more information, see the following resources:</span></span>

* [<span data-ttu-id="4bef2-992">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="4bef2-992">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="4bef2-993">[適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="4bef2-993">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="4bef2-994">[適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="4bef2-994">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="4bef2-995">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-995">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="4bef2-996">[安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。</span><span class="sxs-lookup"><span data-stu-id="4bef2-996">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="4bef2-997">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="4bef2-997">Third-party logging providers</span></span>

<span data-ttu-id="4bef2-998">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="4bef2-998">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="4bef2-999">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-999">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="4bef2-1000">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1000">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="4bef2-1001">[JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1001">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="4bef2-1002">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1002">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="4bef2-1003">[Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）</span><span class="sxs-lookup"><span data-stu-id="4bef2-1003">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="4bef2-1004">[Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1004">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="4bef2-1005">[NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1005">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="4bef2-1006">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1006">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="4bef2-1007">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1007">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="4bef2-1008">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="4bef2-1008">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="4bef2-1009">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="4bef2-1009">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="4bef2-1010">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="4bef2-1010">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="4bef2-1011">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="4bef2-1011">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="4bef2-1012">呼叫 `ILoggerFactory` 記錄架構所提供的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="4bef2-1012">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="4bef2-1013">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="4bef2-1013">For more information, see each provider's documentation.</span></span> <span data-ttu-id="4bef2-1014">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="4bef2-1014">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4bef2-1015">其他資源</span><span class="sxs-lookup"><span data-stu-id="4bef2-1015">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
