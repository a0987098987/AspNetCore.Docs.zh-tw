---
title: .NET Core 與 ASP.NET Core 中的記錄
author: rick-anderson
description: 了解如何使用由 Microsoft.Extensions.Logging NuGet 套件提供的記錄架構。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/23/2020
uid: fundamentals/logging/index
ms.openlocfilehash: 7be8cef3377132ed43efde209db67401d7bdb6dc
ms.sourcegitcommit: 7bb14d005155a5044c7902a08694ee8ccb20c113
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82110911"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="216b8-103">.NET Core 與 ASP.NET Core 中的記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-103">Logging in .NET Core and ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="216b8-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="216b8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="216b8-105">.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="216b8-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="216b8-106">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="216b8-106">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="216b8-107">本文中顯示的大部分程式碼範例都來自 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="216b8-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="216b8-108">這些程式碼片段的記錄特定部分適用于任何使用[泛型主機](xref:fundamentals/host/generic-host)的 .net Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="216b8-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="216b8-109">如需如何在非 web 主控台應用程式中使用泛型主機的範例，請參閱[背景工作範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples)的<xref:fundamentals/host/hosted-services> *Program.cs*檔案（）。</span><span class="sxs-lookup"><span data-stu-id="216b8-109">For an example of how to use the Generic Host in a non-web console app, see the *Program.cs* file of the [Background Tasks sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span></span>

<span data-ttu-id="216b8-110">不含一般主機的應用程式記錄程式碼，會因[新增提供者](#add-providers)和[建立記錄器](#create-logs)的方式而有所不同。</span><span class="sxs-lookup"><span data-stu-id="216b8-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="216b8-111">非主機程式碼範例顯示於本文的這些章節中。</span><span class="sxs-lookup"><span data-stu-id="216b8-111">Non-host code examples are shown in those sections of the article.</span></span>

<span data-ttu-id="216b8-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="216b8-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="216b8-113">新增提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-113">Add providers</span></span>

<span data-ttu-id="216b8-114">記錄提供者會顯示或儲存記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="216b8-115">例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="216b8-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="216b8-116">您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。</span><span class="sxs-lookup"><span data-stu-id="216b8-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

<span data-ttu-id="216b8-117">若要在使用一般主機的應用程式中新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="216b8-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="216b8-118">在非主機主控台應用程式中，於建立 `LoggerFactory` 時呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="216b8-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="216b8-119">`LoggerFactory` 和 `AddConsole` 需要 `Microsoft.Extensions.Logging` 的 `using` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="216b8-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="216b8-120">預設 ASP.NET Core 專案範本會呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> 新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="216b8-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* [<span data-ttu-id="216b8-121">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-121">Console</span></span>](#console-provider)
* [<span data-ttu-id="216b8-122">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-122">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="216b8-123">EventSource</span><span class="sxs-lookup"><span data-stu-id="216b8-123">EventSource</span></span>](#event-source-provider)
* <span data-ttu-id="216b8-124">[EventLog](#windows-eventlog-provider) （僅適用于在 Windows 上執行時）</span><span class="sxs-lookup"><span data-stu-id="216b8-124">[EventLog](#windows-eventlog-provider) (only when running on Windows)</span></span>

<span data-ttu-id="216b8-125">您可以使用您想要的提供者來取代預設提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="216b8-126">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

<span data-ttu-id="216b8-127">在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="216b8-127">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="216b8-128">建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-128">Create logs</span></span>

<span data-ttu-id="216b8-129">若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="216b8-129">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="216b8-130">在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="216b8-130">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="216b8-131">在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="216b8-131">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="216b8-132">下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-132">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="216b8-133">記錄「類別」\*\* 是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="216b8-133">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="216b8-134">由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-134">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="216b8-135">下列非主機主控台應用程式範例會建立以 `LoggingConsoleApp.Program` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-135">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

<span data-ttu-id="216b8-136">在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-136">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="216b8-137">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="216b8-137">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

<span data-ttu-id="216b8-138">此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="216b8-138">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span>

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="216b8-139">在 Program 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-139">Create logs in the Program class</span></span>

<span data-ttu-id="216b8-140">若要在 ASP.NET Core 應用程式的 `Program` 類別中寫入記錄，請在建立主機之後從 DI 取得 `ILogger` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="216b8-140">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/TodoApiSample/Program.cs?highlight=9,10)]

<span data-ttu-id="216b8-141">不直接支援在主機結構期間進行記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-141">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="216b8-142">不過，您可以使用個別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-142">However, a separate logger can be used.</span></span> <span data-ttu-id="216b8-143">在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateHostBuilder`。 </span><span class="sxs-lookup"><span data-stu-id="216b8-143">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="216b8-144">`AddSerilog`會使用中`Log.Logger`指定的靜態設定：</span><span class="sxs-lookup"><span data-stu-id="216b8-144">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="216b8-145">在 Startup 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-145">Create logs in the Startup class</span></span>

<span data-ttu-id="216b8-146">若要在 ASP.NET Core 應用程式的 `Startup.Configure` 方法中寫入記錄，請在方法簽章中包含 `ILogger` 參數：</span><span class="sxs-lookup"><span data-stu-id="216b8-146">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="216b8-147">在以 `Startup.ConfigureServices` 方法完成 DI 容器設定之前，不支援寫入記錄檔：</span><span class="sxs-lookup"><span data-stu-id="216b8-147">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="216b8-148">不支援將記錄器插入 `Startup` 建構函式。</span><span class="sxs-lookup"><span data-stu-id="216b8-148">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="216b8-149">不支援將記錄器插入 `Startup.ConfigureServices` 方法簽章</span><span class="sxs-lookup"><span data-stu-id="216b8-149">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="216b8-150">這項限制的原因是記錄相依於 DI 和組態，而後者相依於 DI。</span><span class="sxs-lookup"><span data-stu-id="216b8-150">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="216b8-151">在 `ConfigureServices` 完成後才會設定 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="216b8-151">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="216b8-152">記錄器的建構函式插入 `Startup` 適用於舊版 ASP.NET Core，因為會為 Web 主機建立個別的 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="216b8-152">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="216b8-153">如需為何只為一般主機建立一個容器的資訊，請參閱[重大變更公告](https://github.com/aspnet/Announcements/issues/353)。</span><span class="sxs-lookup"><span data-stu-id="216b8-153">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="216b8-154">如果您需要設定相依於 `ILogger<T>` 的服務，您仍然可以使用建構函式插入或提供 Factory 方法來執行此動作。</span><span class="sxs-lookup"><span data-stu-id="216b8-154">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="216b8-155">只有在沒有其他選項時，才建議使用 Factory 方法。</span><span class="sxs-lookup"><span data-stu-id="216b8-155">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="216b8-156">例如，假設您需要使用 DI 的服務來填入屬性：</span><span class="sxs-lookup"><span data-stu-id="216b8-156">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="216b8-157">前面的醒目提示程式碼是 `Func`，會在 DI 容器第一次需要建立 `MyService` 的執行個體時執行。</span><span class="sxs-lookup"><span data-stu-id="216b8-157">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="216b8-158">您可以用此方式存取任何已註冊的服務。</span><span class="sxs-lookup"><span data-stu-id="216b8-158">You can access any of the registered services in this way.</span></span>

### <a name="create-logs-in-blazor"></a><span data-ttu-id="216b8-159">在 Blazor 中建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-159">Create logs in Blazor</span></span>

#### <a name="blazor-webassembly"></a><span data-ttu-id="216b8-160">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="216b8-160">Blazor WebAssembly</span></span>

<span data-ttu-id="216b8-161">使用中的`WebAssemblyHostBuilder.Logging`屬性在 Blazor WebAssembly 應用程式中`Program.Main`設定記錄功能：</span><span class="sxs-lookup"><span data-stu-id="216b8-161">Configure logging in Blazor WebAssembly apps with the `WebAssemblyHostBuilder.Logging` property in `Program.Main`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

...

var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.Logging.SetMinimumLevel(LogLevel.Debug);
builder.Logging.AddProvider(new CustomLoggingProvider());
```

<span data-ttu-id="216b8-162">`Logging`屬性的類型<xref:Microsoft.Extensions.Logging.ILoggingBuilder>為，因此中提供<xref:Microsoft.Extensions.Logging.ILoggingBuilder>的所有擴充方法也可在上`Logging`取得。</span><span class="sxs-lookup"><span data-stu-id="216b8-162">The `Logging` property is of type <xref:Microsoft.Extensions.Logging.ILoggingBuilder>, so all of the extension methods available on <xref:Microsoft.Extensions.Logging.ILoggingBuilder> are also available on `Logging`.</span></span>

#### <a name="log-in-razor-components"></a><span data-ttu-id="216b8-163">Razor 元件中的登入</span><span class="sxs-lookup"><span data-stu-id="216b8-163">Log in Razor components</span></span>

<span data-ttu-id="216b8-164">記錄器尊重應用程式啟動設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-164">Loggers respect app startup configuration.</span></span>

<span data-ttu-id="216b8-165">需要的`using`指示詞，才能支援 Api 的 Intellisense 完成（例如<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A>和<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A>）。 <xref:Microsoft.Extensions.Logging></span><span class="sxs-lookup"><span data-stu-id="216b8-165">The `using` directive for <xref:Microsoft.Extensions.Logging> is required to support Intellisense completions for APIs, such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A>.</span></span>

<span data-ttu-id="216b8-166">下列範例示範如何使用 Razor 元件<xref:Microsoft.Extensions.Logging.ILogger>中的進行記錄：</span><span class="sxs-lookup"><span data-stu-id="216b8-166">The following example demonstrates logging with an <xref:Microsoft.Extensions.Logging.ILogger> in Razor components:</span></span>

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

<span data-ttu-id="216b8-167">下列範例示範如何使用 Razor 元件<xref:Microsoft.Extensions.Logging.ILoggerFactory>中的進行記錄：</span><span class="sxs-lookup"><span data-stu-id="216b8-167">The following example demonstrates logging with an <xref:Microsoft.Extensions.Logging.ILoggerFactory> in Razor components:</span></span>

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

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="216b8-168">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="216b8-168">No asynchronous logger methods</span></span>

<span data-ttu-id="216b8-169">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="216b8-169">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="216b8-170">若您的記錄資料存放區很慢，請不要直接寫入其中。</span><span class="sxs-lookup"><span data-stu-id="216b8-170">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="216b8-171">請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-171">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="216b8-172">例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="216b8-172">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="216b8-173">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="216b8-173">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="216b8-174">如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="216b8-174">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="216b8-175">設定</span><span class="sxs-lookup"><span data-stu-id="216b8-175">Configuration</span></span>

<span data-ttu-id="216b8-176">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="216b8-176">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="216b8-177">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="216b8-177">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="216b8-178">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="216b8-178">Command-line arguments.</span></span>
* <span data-ttu-id="216b8-179">環境變數。</span><span class="sxs-lookup"><span data-stu-id="216b8-179">Environment variables.</span></span>
* <span data-ttu-id="216b8-180">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="216b8-180">In-memory .NET objects.</span></span>
* <span data-ttu-id="216b8-181">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="216b8-181">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="216b8-182">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-182">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="216b8-183">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="216b8-183">Custom providers (installed or created).</span></span>

<span data-ttu-id="216b8-184">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="216b8-184">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="216b8-185">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="216b8-185">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="216b8-186">`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。</span><span class="sxs-lookup"><span data-stu-id="216b8-186">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="216b8-187">`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="216b8-187">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="216b8-188">在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-188">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="216b8-189">`Logging` 下的其他屬性可指定記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-189">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="216b8-190">範例使用主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-190">The example is for the Console provider.</span></span> <span data-ttu-id="216b8-191">如果提供者支援[記錄範圍](#log-scopes)， `IncludeScopes`則指出是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="216b8-191">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="216b8-192">提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="216b8-192">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="216b8-193">提供者下的 `LogLevel` 會指定提供者的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-193">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="216b8-194">若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="216b8-194">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

<span data-ttu-id="216b8-195">記錄 API 不包含在應用程式執行時變更記錄層級的案例。</span><span class="sxs-lookup"><span data-stu-id="216b8-195">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="216b8-196">不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-196">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="216b8-197">例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)（由`CreateDefaultBuilder`新增）以讀取設定檔案，預設會重載記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-197">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="216b8-198">如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-198">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="216b8-199">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="216b8-199">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="216b8-200">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="216b8-200">Sample logging output</span></span>

<span data-ttu-id="216b8-201">使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="216b8-201">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="216b8-202">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="216b8-202">Here's an example of console output:</span></span>

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

<span data-ttu-id="216b8-203">上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。</span><span class="sxs-lookup"><span data-stu-id="216b8-203">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="216b8-204">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="216b8-204">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

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

<span data-ttu-id="216b8-205">這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApiSample" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="216b8-205">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="216b8-206">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="216b8-206">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="216b8-207">ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-207">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="216b8-208">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="216b8-208">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="216b8-209">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="216b8-209">NuGet packages</span></span>

<span data-ttu-id="216b8-210">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="216b8-210">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="216b8-211">記錄分類</span><span class="sxs-lookup"><span data-stu-id="216b8-211">Log category</span></span>

<span data-ttu-id="216b8-212">建立 `ILogger` 物件時，會為它指定「類別」\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-212">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="216b8-213">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="216b8-213">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="216b8-214">類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="216b8-214">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="216b8-215">使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：</span><span class="sxs-lookup"><span data-stu-id="216b8-215">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="216b8-216">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="216b8-216">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="216b8-217">`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="216b8-217">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="216b8-218">記錄層級</span><span class="sxs-lookup"><span data-stu-id="216b8-218">Log level</span></span>

<span data-ttu-id="216b8-219">每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。</span><span class="sxs-lookup"><span data-stu-id="216b8-219">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="216b8-220">記錄層級表示嚴重性或重要性。</span><span class="sxs-lookup"><span data-stu-id="216b8-220">The log level indicates the severity or importance.</span></span> <span data-ttu-id="216b8-221">例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-221">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="216b8-222">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="216b8-222">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="216b8-223">在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="216b8-223">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="216b8-224">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="216b8-224">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="216b8-225">此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="216b8-225">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="216b8-226">在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。</span><span class="sxs-lookup"><span data-stu-id="216b8-226">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="216b8-227">這些方法會呼叫接受 `Log` 參數的 `LogLevel`。</span><span class="sxs-lookup"><span data-stu-id="216b8-227">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="216b8-228">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="216b8-228">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="216b8-229">如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="216b8-229">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="216b8-230">ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="216b8-230">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="216b8-231">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="216b8-231">Trace = 0</span></span>

  <span data-ttu-id="216b8-232">針對通常只對偵錯有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-232">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="216b8-233">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="216b8-233">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="216b8-234">*預設為停用。*</span><span class="sxs-lookup"><span data-stu-id="216b8-234">*Disabled by default.*</span></span>

* <span data-ttu-id="216b8-235">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="216b8-235">Debug = 1</span></span>

  <span data-ttu-id="216b8-236">針對可在開發與偵錯中使用的資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-236">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="216b8-237">範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-237">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="216b8-238">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="216b8-238">Information = 2</span></span>

  <span data-ttu-id="216b8-239">針對一般應用程式流程的追蹤。</span><span class="sxs-lookup"><span data-stu-id="216b8-239">For tracking the general flow of the app.</span></span> <span data-ttu-id="216b8-240">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="216b8-240">These logs typically have some long-term value.</span></span> <span data-ttu-id="216b8-241">範例： `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="216b8-241">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="216b8-242">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="216b8-242">Warning = 3</span></span>

  <span data-ttu-id="216b8-243">針對應用程式流程中發生的異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-243">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="216b8-244">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="216b8-244">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="216b8-245">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="216b8-245">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="216b8-246">範例： `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="216b8-246">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="216b8-247">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="216b8-247">Error = 4</span></span>

  <span data-ttu-id="216b8-248">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="216b8-248">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="216b8-249">這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="216b8-249">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="216b8-250">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="216b8-250">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="216b8-251">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="216b8-251">Critical = 5</span></span>

  <span data-ttu-id="216b8-252">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="216b8-252">For failures that require immediate attention.</span></span> <span data-ttu-id="216b8-253">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="216b8-253">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="216b8-254">使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="216b8-254">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="216b8-255">例如：</span><span class="sxs-lookup"><span data-stu-id="216b8-255">For example:</span></span>

* <span data-ttu-id="216b8-256">在生產環境中：</span><span class="sxs-lookup"><span data-stu-id="216b8-256">In production:</span></span>
  * <span data-ttu-id="216b8-257">`Trace`透過`Information`層級的記錄會產生大量的詳細記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-257">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="216b8-258">若要控制成本，而不超過資料儲存體限制`Trace` ， `Information`請將層級訊息記錄到高容量、低成本的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-258">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="216b8-259">`Warning`透過`Critical`層級登入通常會產生較少、較小的記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-259">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="216b8-260">因此，成本和儲存體限制通常不會造成問題，因此可讓您更靈活地選擇資料存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-260">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="216b8-261">在開發期間：</span><span class="sxs-lookup"><span data-stu-id="216b8-261">During development:</span></span>
  * <span data-ttu-id="216b8-262">將`Warning` `Critical`訊息記錄到主控台。</span><span class="sxs-lookup"><span data-stu-id="216b8-262">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="216b8-263">進行`Trace`疑難排解`Information`時，新增訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-263">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="216b8-264">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-264">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="216b8-265">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-265">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="216b8-266">此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-266">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="216b8-267">以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：</span><span class="sxs-lookup"><span data-stu-id="216b8-267">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="216b8-268">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="216b8-268">Log event ID</span></span>

<span data-ttu-id="216b8-269">每個記錄都可以指定「事件識別碼」\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-269">Each log can specify an *event ID*.</span></span> <span data-ttu-id="216b8-270">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="216b8-270">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="216b8-271">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="216b8-271">An event ID associates a set of events.</span></span> <span data-ttu-id="216b8-272">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="216b8-272">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="216b8-273">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="216b8-273">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="216b8-274">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="216b8-274">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="216b8-275">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="216b8-275">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="216b8-276">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="216b8-276">Log message template</span></span>

<span data-ttu-id="216b8-277">每個記錄都會指定訊息範本。</span><span class="sxs-lookup"><span data-stu-id="216b8-277">Each log specifies a message template.</span></span> <span data-ttu-id="216b8-278">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="216b8-278">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="216b8-279">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="216b8-279">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="216b8-280">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="216b8-280">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="216b8-281">請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：</span><span class="sxs-lookup"><span data-stu-id="216b8-281">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="216b8-282">此程式碼會使用依順序的參數值建立記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="216b8-282">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="216b8-283">記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="216b8-283">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="216b8-284">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="216b8-284">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="216b8-285">此資訊可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="216b8-285">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="216b8-286">例如，假設記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="216b8-286">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="216b8-287">若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="216b8-287">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="216b8-288">查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。</span><span class="sxs-lookup"><span data-stu-id="216b8-288">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="216b8-289">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="216b8-289">Logging exceptions</span></span>

<span data-ttu-id="216b8-290">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="216b8-290">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="216b8-291">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="216b8-291">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="216b8-292">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="216b8-292">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="216b8-293">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="216b8-293">Log filtering</span></span>

<span data-ttu-id="216b8-294">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-294">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="216b8-295">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="216b8-295">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="216b8-296">若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-296">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="216b8-297">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="216b8-297">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="216b8-298">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="216b8-298">Create filter rules in configuration</span></span>

<span data-ttu-id="216b8-299">專案範本程式碼會`CreateDefaultBuilder`呼叫以設定主控台、Debug 和 EventSource （ASP.NET Core 2.2 或更新版本）提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-299">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="216b8-300">`CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。</span><span class="sxs-lookup"><span data-stu-id="216b8-300">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="216b8-301">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="216b8-301">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="216b8-302">此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-302">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="216b8-303">建立 `ILogger` 物件時，會為每個提供者選取一個規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-303">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="216b8-304">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="216b8-304">Filter rules in code</span></span>

<span data-ttu-id="216b8-305">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="216b8-305">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=2-3)]

<span data-ttu-id="216b8-306">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-306">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="216b8-307">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-307">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="216b8-308">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="216b8-308">How filtering rules are applied</span></span>

<span data-ttu-id="216b8-309">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-309">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="216b8-310">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="216b8-310">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="216b8-311">Number</span><span class="sxs-lookup"><span data-stu-id="216b8-311">Number</span></span> | <span data-ttu-id="216b8-312">提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-312">Provider</span></span>      | <span data-ttu-id="216b8-313">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="216b8-313">Categories that begin with ...</span></span>          | <span data-ttu-id="216b8-314">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="216b8-314">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="216b8-315">1</span><span class="sxs-lookup"><span data-stu-id="216b8-315">1</span></span>      | <span data-ttu-id="216b8-316">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-316">Debug</span></span>         | <span data-ttu-id="216b8-317">所有類別</span><span class="sxs-lookup"><span data-stu-id="216b8-317">All categories</span></span>                          | <span data-ttu-id="216b8-318">資訊</span><span class="sxs-lookup"><span data-stu-id="216b8-318">Information</span></span>       |
| <span data-ttu-id="216b8-319">2</span><span class="sxs-lookup"><span data-stu-id="216b8-319">2</span></span>      | <span data-ttu-id="216b8-320">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-320">Console</span></span>       | <span data-ttu-id="216b8-321">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="216b8-321">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="216b8-322">警告</span><span class="sxs-lookup"><span data-stu-id="216b8-322">Warning</span></span>           |
| <span data-ttu-id="216b8-323">3</span><span class="sxs-lookup"><span data-stu-id="216b8-323">3</span></span>      | <span data-ttu-id="216b8-324">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-324">Console</span></span>       | <span data-ttu-id="216b8-325">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="216b8-325">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="216b8-326">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-326">Debug</span></span>             |
| <span data-ttu-id="216b8-327">4</span><span class="sxs-lookup"><span data-stu-id="216b8-327">4</span></span>      | <span data-ttu-id="216b8-328">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-328">Console</span></span>       | <span data-ttu-id="216b8-329">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="216b8-329">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="216b8-330">錯誤</span><span class="sxs-lookup"><span data-stu-id="216b8-330">Error</span></span>             |
| <span data-ttu-id="216b8-331">5</span><span class="sxs-lookup"><span data-stu-id="216b8-331">5</span></span>      | <span data-ttu-id="216b8-332">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-332">Console</span></span>       | <span data-ttu-id="216b8-333">所有類別</span><span class="sxs-lookup"><span data-stu-id="216b8-333">All categories</span></span>                          | <span data-ttu-id="216b8-334">資訊</span><span class="sxs-lookup"><span data-stu-id="216b8-334">Information</span></span>       |
| <span data-ttu-id="216b8-335">6</span><span class="sxs-lookup"><span data-stu-id="216b8-335">6</span></span>      | <span data-ttu-id="216b8-336">所有提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-336">All providers</span></span> | <span data-ttu-id="216b8-337">所有類別</span><span class="sxs-lookup"><span data-stu-id="216b8-337">All categories</span></span>                          | <span data-ttu-id="216b8-338">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-338">Debug</span></span>             |
| <span data-ttu-id="216b8-339">7</span><span class="sxs-lookup"><span data-stu-id="216b8-339">7</span></span>      | <span data-ttu-id="216b8-340">所有提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-340">All providers</span></span> | <span data-ttu-id="216b8-341">System</span><span class="sxs-lookup"><span data-stu-id="216b8-341">System</span></span>                                  | <span data-ttu-id="216b8-342">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-342">Debug</span></span>             |
| <span data-ttu-id="216b8-343">8</span><span class="sxs-lookup"><span data-stu-id="216b8-343">8</span></span>      | <span data-ttu-id="216b8-344">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-344">Debug</span></span>         | <span data-ttu-id="216b8-345">Microsoft</span><span class="sxs-lookup"><span data-stu-id="216b8-345">Microsoft</span></span>                               | <span data-ttu-id="216b8-346">追蹤</span><span class="sxs-lookup"><span data-stu-id="216b8-346">Trace</span></span>             |

<span data-ttu-id="216b8-347">建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-347">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="216b8-348">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="216b8-348">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="216b8-349">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-349">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="216b8-350">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="216b8-350">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="216b8-351">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-351">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="216b8-352">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-352">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="216b8-353">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-353">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="216b8-354">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-354">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="216b8-355">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="216b8-355">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="216b8-356">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="216b8-356">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="216b8-357">使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="216b8-357">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="216b8-358">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="216b8-358">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="216b8-359">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-359">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="216b8-360">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="216b8-360">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="216b8-361">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="216b8-361">Rule 3 is most specific.</span></span>

<span data-ttu-id="216b8-362">產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-362">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="216b8-363">`Debug` 層級以上的記錄會傳送到主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-363">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="216b8-364">提供者別名</span><span class="sxs-lookup"><span data-stu-id="216b8-364">Provider aliases</span></span>

<span data-ttu-id="216b8-365">每個提供者都會定義「別名」\*\*，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="216b8-365">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="216b8-366">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="216b8-366">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="216b8-367">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-367">Console</span></span>
* <span data-ttu-id="216b8-368">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-368">Debug</span></span>
* <span data-ttu-id="216b8-369">EventSource</span><span class="sxs-lookup"><span data-stu-id="216b8-369">EventSource</span></span>
* <span data-ttu-id="216b8-370">EventLog</span><span class="sxs-lookup"><span data-stu-id="216b8-370">EventLog</span></span>
* <span data-ttu-id="216b8-371">TraceSource</span><span class="sxs-lookup"><span data-stu-id="216b8-371">TraceSource</span></span>
* <span data-ttu-id="216b8-372">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="216b8-372">AzureAppServicesFile</span></span>
* <span data-ttu-id="216b8-373">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="216b8-373">AzureAppServicesBlob</span></span>
* <span data-ttu-id="216b8-374">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="216b8-374">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="216b8-375">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="216b8-375">Default minimum level</span></span>

<span data-ttu-id="216b8-376">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="216b8-376">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="216b8-377">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="216b8-377">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="216b8-378">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-378">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="216b8-379">篩選函數</span><span class="sxs-lookup"><span data-stu-id="216b8-379">Filter functions</span></span>

<span data-ttu-id="216b8-380">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="216b8-380">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="216b8-381">函式中的程式碼可以存取提供者類型、類別與記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-381">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="216b8-382">例如：</span><span class="sxs-lookup"><span data-stu-id="216b8-382">For example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

## <a name="system-categories-and-levels"></a><span data-ttu-id="216b8-383">系統類別與層級</span><span class="sxs-lookup"><span data-stu-id="216b8-383">System categories and levels</span></span>

<span data-ttu-id="216b8-384">以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：</span><span class="sxs-lookup"><span data-stu-id="216b8-384">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="216b8-385">類別</span><span class="sxs-lookup"><span data-stu-id="216b8-385">Category</span></span>                            | <span data-ttu-id="216b8-386">注意</span><span class="sxs-lookup"><span data-stu-id="216b8-386">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="216b8-387">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="216b8-387">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="216b8-388">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="216b8-388">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="216b8-389">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="216b8-389">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="216b8-390">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="216b8-390">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="216b8-391">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="216b8-391">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="216b8-392">允許主機。</span><span class="sxs-lookup"><span data-stu-id="216b8-392">Hosts allowed.</span></span> |
| <span data-ttu-id="216b8-393">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="216b8-393">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="216b8-394">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="216b8-394">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="216b8-395">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="216b8-395">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="216b8-396">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="216b8-396">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="216b8-397">MVC 與 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="216b8-397">MVC and Razor diagnostics.</span></span> <span data-ttu-id="216b8-398">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="216b8-398">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="216b8-399">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="216b8-399">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="216b8-400">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-400">Route matching information.</span></span> |
| <span data-ttu-id="216b8-401">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="216b8-401">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="216b8-402">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="216b8-402">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="216b8-403">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-403">HTTPS certificate information.</span></span> |
| <span data-ttu-id="216b8-404">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="216b8-404">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="216b8-405">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="216b8-405">Files served.</span></span> |
| <span data-ttu-id="216b8-406">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="216b8-406">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="216b8-407">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="216b8-407">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="216b8-408">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="216b8-408">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="216b8-409">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="216b8-409">Log scopes</span></span>

 <span data-ttu-id="216b8-410">「範圍」\*\* 可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="216b8-410">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="216b8-411">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-411">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="216b8-412">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="216b8-412">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="216b8-413">範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="216b8-413">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="216b8-414">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="216b8-414">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="216b8-415">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="216b8-415">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="216b8-416">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="216b8-416">*Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

> [!NOTE]
> <span data-ttu-id="216b8-417">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-417">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="216b8-418">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="216b8-418">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="216b8-419">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="216b8-419">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="216b8-420">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-420">Built-in logging providers</span></span>

<span data-ttu-id="216b8-421">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="216b8-421">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="216b8-422">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-422">Console</span></span>](#console-provider)
* [<span data-ttu-id="216b8-423">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-423">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="216b8-424">EventSource</span><span class="sxs-lookup"><span data-stu-id="216b8-424">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="216b8-425">EventLog</span><span class="sxs-lookup"><span data-stu-id="216b8-425">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="216b8-426">TraceSource</span><span class="sxs-lookup"><span data-stu-id="216b8-426">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="216b8-427">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="216b8-427">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="216b8-428">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="216b8-428">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="216b8-429">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="216b8-429">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="216b8-430">如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="216b8-430">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="216b8-431">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-431">Console provider</span></span>

<span data-ttu-id="216b8-432">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="216b8-432">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="216b8-433">若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="216b8-433">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="216b8-434">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-434">Debug provider</span></span>

<span data-ttu-id="216b8-435">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="216b8-435">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="216b8-436">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="216b8-436">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="216b8-437">事件來源提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-437">Event Source provider</span></span>

<span data-ttu-id="216b8-438">在[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)跨平臺的事件來源中，會寫入名`Microsoft-Extensions-Logging`為的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="216b8-438">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="216b8-439">在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="216b8-439">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="216b8-440">呼叫以建立主機時`CreateDefaultBuilder` ，會自動新增事件來源提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-440">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="216b8-441">dotnet 追蹤工具</span><span class="sxs-lookup"><span data-stu-id="216b8-441">dotnet trace tooling</span></span>

<span data-ttu-id="216b8-442">[Dotnet 追蹤](/dotnet/core/diagnostics/dotnet-trace)工具是一種跨平臺 CLI 全域工具，可讓您收集執行中進程的 .net Core 追蹤。</span><span class="sxs-lookup"><span data-stu-id="216b8-442">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="216b8-443">此工具會<xref:Microsoft.Extensions.Logging.EventSource>使用來收集提供<xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>者資料。</span><span class="sxs-lookup"><span data-stu-id="216b8-443">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="216b8-444">使用下列命令安裝 dotnet 追蹤工具：</span><span class="sxs-lookup"><span data-stu-id="216b8-444">Install the dotnet trace tooling with the following command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-trace
```

<span data-ttu-id="216b8-445">使用 dotnet 追蹤工具，從應用程式收集追蹤：</span><span class="sxs-lookup"><span data-stu-id="216b8-445">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="216b8-446">如果應用程式未使用`CreateDefaultBuilder`建立主機，請將[事件來源提供者](#event-source-provider)新增至應用程式的記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-446">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

1. <span data-ttu-id="216b8-447">使用`dotnet run`命令執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="216b8-447">Run the app with the `dotnet run` command.</span></span>

1. <span data-ttu-id="216b8-448">判斷 .NET Core 應用程式的處理序識別碼（PID）：</span><span class="sxs-lookup"><span data-stu-id="216b8-448">Determine the process identifier (PID) of the .NET Core app:</span></span>

   * <span data-ttu-id="216b8-449">在 Windows 上，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="216b8-449">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="216b8-450">工作管理員（Ctrl + Alt + Del）</span><span class="sxs-lookup"><span data-stu-id="216b8-450">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="216b8-451">tasklist 命令</span><span class="sxs-lookup"><span data-stu-id="216b8-451">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="216b8-452">取得進程 Powershell 命令</span><span class="sxs-lookup"><span data-stu-id="216b8-452">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="216b8-453">在 Linux 上，請使用[pidof 命令](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html)。</span><span class="sxs-lookup"><span data-stu-id="216b8-453">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="216b8-454">尋找與應用程式元件同名之進程的 PID。</span><span class="sxs-lookup"><span data-stu-id="216b8-454">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="216b8-455">執行`dotnet trace`命令。</span><span class="sxs-lookup"><span data-stu-id="216b8-455">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="216b8-456">一般命令語法：</span><span class="sxs-lookup"><span data-stu-id="216b8-456">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="216b8-457">使用 PowerShell 命令 shell 時，將`--providers`值以單引號括住（`'`）：</span><span class="sxs-lookup"><span data-stu-id="216b8-457">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="216b8-458">在非 Windows 平臺上，新增`-f speedscope`選項以將輸出追蹤檔案的格式變更為。 `speedscope`</span><span class="sxs-lookup"><span data-stu-id="216b8-458">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="216b8-459">關鍵字</span><span class="sxs-lookup"><span data-stu-id="216b8-459">Keyword</span></span> | <span data-ttu-id="216b8-460">描述</span><span class="sxs-lookup"><span data-stu-id="216b8-460">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="216b8-461">1</span><span class="sxs-lookup"><span data-stu-id="216b8-461">1</span></span>       | <span data-ttu-id="216b8-462">記錄有關的`LoggingEventSource`中繼事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-462">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="216b8-463">不會記錄來自`ILogger`的事件）。</span><span class="sxs-lookup"><span data-stu-id="216b8-463">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="216b8-464">2</span><span class="sxs-lookup"><span data-stu-id="216b8-464">2</span></span>       | <span data-ttu-id="216b8-465">當呼叫時`Message` `ILogger.Log()` ，開啟事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-465">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="216b8-466">以程式設計方式（未格式化）提供資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-466">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="216b8-467">4</span><span class="sxs-lookup"><span data-stu-id="216b8-467">4</span></span>       | <span data-ttu-id="216b8-468">當呼叫時`FormatMessage` `ILogger.Log()` ，開啟事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-468">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="216b8-469">提供資訊的格式化字串版本。</span><span class="sxs-lookup"><span data-stu-id="216b8-469">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="216b8-470">8</span><span class="sxs-lookup"><span data-stu-id="216b8-470">8</span></span>       | <span data-ttu-id="216b8-471">當呼叫時`MessageJson` `ILogger.Log()` ，開啟事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-471">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="216b8-472">提供引數的 JSON 標記法。</span><span class="sxs-lookup"><span data-stu-id="216b8-472">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="216b8-473">事件層級</span><span class="sxs-lookup"><span data-stu-id="216b8-473">Event Level</span></span> | <span data-ttu-id="216b8-474">說明</span><span class="sxs-lookup"><span data-stu-id="216b8-474">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="216b8-475">0</span><span class="sxs-lookup"><span data-stu-id="216b8-475">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="216b8-476">1</span><span class="sxs-lookup"><span data-stu-id="216b8-476">1</span></span>           | `Critical`      |
   | <span data-ttu-id="216b8-477">2</span><span class="sxs-lookup"><span data-stu-id="216b8-477">2</span></span>           | `Error`         |
   | <span data-ttu-id="216b8-478">3</span><span class="sxs-lookup"><span data-stu-id="216b8-478">3</span></span>           | `Warning`       |
   | <span data-ttu-id="216b8-479">4</span><span class="sxs-lookup"><span data-stu-id="216b8-479">4</span></span>           | `Informational` |
   | <span data-ttu-id="216b8-480">5</span><span class="sxs-lookup"><span data-stu-id="216b8-480">5</span></span>           | `Verbose`       |

   <span data-ttu-id="216b8-481">`FilterSpecs`和`{Event Level}`的`{Logger Category}`專案代表其他記錄篩選準則。</span><span class="sxs-lookup"><span data-stu-id="216b8-481">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="216b8-482">以`FilterSpecs`分號（`;`）分隔專案。</span><span class="sxs-lookup"><span data-stu-id="216b8-482">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="216b8-483">使用 Windows 命令 shell 的範例（\*\* \*\* `--providers`值前後沒有單引號）：</span><span class="sxs-lookup"><span data-stu-id="216b8-483">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="216b8-484">上述命令會啟用：</span><span class="sxs-lookup"><span data-stu-id="216b8-484">The preceding command activates:</span></span>

   * <span data-ttu-id="216b8-485">事件來源記錄器，用來產生錯誤（`4``2`）的格式化字串（）。</span><span class="sxs-lookup"><span data-stu-id="216b8-485">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="216b8-486">`Microsoft.AspNetCore.Hosting`在`Informational`記錄層級進行記錄`4`（）。</span><span class="sxs-lookup"><span data-stu-id="216b8-486">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="216b8-487">按 Enter 鍵或 Ctrl + C 來停止 dotnet 追蹤工具。</span><span class="sxs-lookup"><span data-stu-id="216b8-487">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="216b8-488">追蹤會以名稱*nettrace*儲存在`dotnet trace`命令執行所在的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="216b8-488">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="216b8-489">使用[Perfview](#perfview)開啟追蹤。</span><span class="sxs-lookup"><span data-stu-id="216b8-489">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="216b8-490">開啟*nettrace*檔案，並流覽追蹤事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-490">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="216b8-491">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="216b8-491">For more information, see:</span></span>

* <span data-ttu-id="216b8-492">[效能分析公用程式追蹤（dotnet-追蹤）](/dotnet/core/diagnostics/dotnet-trace) （.net Core 檔）</span><span class="sxs-lookup"><span data-stu-id="216b8-492">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="216b8-493">[效能分析公用程式追蹤（dotnet 追蹤）](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) （dotnet/診斷 GitHub 存放庫檔）</span><span class="sxs-lookup"><span data-stu-id="216b8-493">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="216b8-494">[LoggingEventSource 類別](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource)（.Net API 瀏覽器）</span><span class="sxs-lookup"><span data-stu-id="216b8-494">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="216b8-495">[LoggingEventSource 參考來源（3.0）](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash;若要取得不同版本的參考來源，請將分支變更`release/{Version}`為， `{Version}`其中是所需 ASP.NET Core 的版本。</span><span class="sxs-lookup"><span data-stu-id="216b8-495">[LoggingEventSource reference source (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="216b8-496">[Perfview](#perfview) &ndash;適用于查看事件來源追蹤。</span><span class="sxs-lookup"><span data-stu-id="216b8-496">[Perfview](#perfview) &ndash; Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="216b8-497">Perfview</span><span class="sxs-lookup"><span data-stu-id="216b8-497">Perfview</span></span>

<span data-ttu-id="216b8-498">使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-498">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="216b8-499">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="216b8-499">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="216b8-500">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]\*\*\*\* 清單</span><span class="sxs-lookup"><span data-stu-id="216b8-500">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="216b8-501">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="216b8-501">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="216b8-503">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-503">Windows EventLog provider</span></span>

<span data-ttu-id="216b8-504">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="216b8-504">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="216b8-505">[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。</span><span class="sxs-lookup"><span data-stu-id="216b8-505">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="216b8-506">若`null`未指定，則會使用下列預設設定：</span><span class="sxs-lookup"><span data-stu-id="216b8-506">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="216b8-507">`LogName`&ndash; 「應用程式」</span><span class="sxs-lookup"><span data-stu-id="216b8-507">`LogName` &ndash; "Application"</span></span>
* <span data-ttu-id="216b8-508">`SourceName`&ndash; 「.Net 執行時間」</span><span class="sxs-lookup"><span data-stu-id="216b8-508">`SourceName` &ndash; ".NET Runtime"</span></span>
* <span data-ttu-id="216b8-509">`MachineName` &ndash; 本機電腦</span><span class="sxs-lookup"><span data-stu-id="216b8-509">`MachineName` &ndash; local machine</span></span>

<span data-ttu-id="216b8-510">記錄[警告層級和更新版本](#log-level)的事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-510">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="216b8-511">若要記錄低於`Warning`的事件，請明確設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-511">To log events lower than `Warning`, explicitly set the log level.</span></span> <span data-ttu-id="216b8-512">例如，將下列內容新增至*appsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="216b8-512">For example, add the following to the *appsettings.json* file:</span></span>

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="216b8-513">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-513">TraceSource provider</span></span>

<span data-ttu-id="216b8-514">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-514">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="216b8-515">[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="216b8-515">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="216b8-516">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="216b8-516">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="216b8-517">該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。</span><span class="sxs-lookup"><span data-stu-id="216b8-517">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="216b8-518">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-518">Azure App Service provider</span></span>

<span data-ttu-id="216b8-519">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="216b8-519">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="216b8-520">該提供者套件並未包含在共用架構中。</span><span class="sxs-lookup"><span data-stu-id="216b8-520">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="216b8-521">若要使用提供者，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="216b8-521">To use the provider, add the provider package to the project.</span></span>

<span data-ttu-id="216b8-522">如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：</span><span class="sxs-lookup"><span data-stu-id="216b8-522">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

<span data-ttu-id="216b8-523">當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]\*\*\*\* 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-523">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="216b8-524">當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="216b8-524">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="216b8-525">**應用程式記錄 (檔案系統)**</span><span class="sxs-lookup"><span data-stu-id="216b8-525">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="216b8-526">**應用程式記錄 (Blob)**</span><span class="sxs-lookup"><span data-stu-id="216b8-526">**Application Logging (Blob)**</span></span>

<span data-ttu-id="216b8-527">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="216b8-527">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="216b8-528">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="216b8-528">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="216b8-529">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="216b8-529">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="216b8-530">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="216b8-530">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="216b8-531">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="216b8-531">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="216b8-532">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="216b8-532">Azure log streaming</span></span>

<span data-ttu-id="216b8-533">Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="216b8-533">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="216b8-534">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="216b8-534">The app server</span></span>
* <span data-ttu-id="216b8-535">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="216b8-535">The web server</span></span>
* <span data-ttu-id="216b8-536">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="216b8-536">Failed request tracing</span></span>

<span data-ttu-id="216b8-537">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="216b8-537">To configure Azure log streaming:</span></span>

* <span data-ttu-id="216b8-538">從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-538">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="216b8-539">將 [應用程式記錄 (檔案系統)]\*\*\*\* 設定為 [開啟]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-539">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="216b8-540">選擇記錄 [層級]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-540">Choose the log **Level**.</span></span> <span data-ttu-id="216b8-541">此設定僅適用于 Azure 記錄串流，而不適用於應用程式中的其他記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-541">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="216b8-542">瀏覽到 [記錄資料流]\*\*\*\* 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-542">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="216b8-543">這些是應用程式透過 `ILogger` 介面產生的訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-543">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="216b8-544">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-544">Azure Application Insights trace logging</span></span>

<span data-ttu-id="216b8-545">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。</span><span class="sxs-lookup"><span data-stu-id="216b8-545">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="216b8-546">Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。</span><span class="sxs-lookup"><span data-stu-id="216b8-546">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="216b8-547">如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-547">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="216b8-548">記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。</span><span class="sxs-lookup"><span data-stu-id="216b8-548">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="216b8-549">如果您使用此套件，就不需安裝提供者套件。</span><span class="sxs-lookup"><span data-stu-id="216b8-549">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="216b8-550">不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="216b8-550">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="216b8-551">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="216b8-551">For more information, see the following resources:</span></span>

* [<span data-ttu-id="216b8-552">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="216b8-552">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="216b8-553">[適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="216b8-553">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="216b8-554">[適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="216b8-554">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="216b8-555">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="216b8-555">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="216b8-556">[安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。</span><span class="sxs-lookup"><span data-stu-id="216b8-556">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="216b8-557">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-557">Third-party logging providers</span></span>

<span data-ttu-id="216b8-558">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="216b8-558">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="216b8-559">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-559">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="216b8-560">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-560">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="216b8-561">[JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="216b8-561">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="216b8-562">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="216b8-562">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="216b8-563">[Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）</span><span class="sxs-lookup"><span data-stu-id="216b8-563">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="216b8-564">[Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-564">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="216b8-565">[NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-565">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="216b8-566">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="216b8-566">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="216b8-567">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="216b8-567">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="216b8-568">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="216b8-568">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="216b8-569">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="216b8-569">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="216b8-570">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="216b8-570">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="216b8-571">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="216b8-571">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="216b8-572">通話記錄`ILoggerFactory`架構所提供的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="216b8-572">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="216b8-573">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="216b8-573">For more information, see each provider's documentation.</span></span> <span data-ttu-id="216b8-574">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-574">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="216b8-575">其他資源</span><span class="sxs-lookup"><span data-stu-id="216b8-575">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="216b8-576">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="216b8-576">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="216b8-577">.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="216b8-577">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="216b8-578">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="216b8-578">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="216b8-579">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="216b8-579">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="216b8-580">新增提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-580">Add providers</span></span>

<span data-ttu-id="216b8-581">記錄提供者會顯示或儲存記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-581">A logging provider displays or stores logs.</span></span> <span data-ttu-id="216b8-582">例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="216b8-582">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="216b8-583">您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。</span><span class="sxs-lookup"><span data-stu-id="216b8-583">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

<span data-ttu-id="216b8-584">若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="216b8-584">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="216b8-585">上述程式碼需要對 `Microsoft.Extensions.Logging` 與 `Microsoft.Extensions.Configuration` 的參考。</span><span class="sxs-lookup"><span data-stu-id="216b8-585">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="216b8-586">預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="216b8-586">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="216b8-587">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-587">Console</span></span>
* <span data-ttu-id="216b8-588">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-588">Debug</span></span>
* <span data-ttu-id="216b8-589">EventSource (從 ASP.NET Core 2.2 開始)</span><span class="sxs-lookup"><span data-stu-id="216b8-589">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="216b8-590">若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-590">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="216b8-591">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-591">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

<span data-ttu-id="216b8-592">在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="216b8-592">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="216b8-593">建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-593">Create logs</span></span>

<span data-ttu-id="216b8-594">若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="216b8-594">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="216b8-595">在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="216b8-595">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="216b8-596">在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="216b8-596">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="216b8-597">下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-597">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="216b8-598">記錄「類別」\*\* 是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="216b8-598">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="216b8-599">由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-599">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="216b8-600">在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-600">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="216b8-601">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="216b8-601">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

<span data-ttu-id="216b8-602">此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="216b8-602">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span>

### <a name="create-logs-in-startup"></a><span data-ttu-id="216b8-603">在啟動中建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-603">Create logs in Startup</span></span>

<span data-ttu-id="216b8-604">若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：</span><span class="sxs-lookup"><span data-stu-id="216b8-604">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="216b8-605">在 Program 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-605">Create logs in the Program class</span></span>

<span data-ttu-id="216b8-606">若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="216b8-606">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="216b8-607">不直接支援在主機結構期間進行記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-607">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="216b8-608">不過，您可以使用個別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-608">However, a separate logger can be used.</span></span> <span data-ttu-id="216b8-609">在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateWebHostBuilder`。 </span><span class="sxs-lookup"><span data-stu-id="216b8-609">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="216b8-610">`AddSerilog`會使用中`Log.Logger`指定的靜態設定：</span><span class="sxs-lookup"><span data-stu-id="216b8-610">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="216b8-611">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="216b8-611">No asynchronous logger methods</span></span>

<span data-ttu-id="216b8-612">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="216b8-612">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="216b8-613">若您的記錄資料存放區很慢，請不要直接寫入其中。</span><span class="sxs-lookup"><span data-stu-id="216b8-613">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="216b8-614">請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-614">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="216b8-615">例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="216b8-615">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="216b8-616">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="216b8-616">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="216b8-617">如需詳細資訊，請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="216b8-617">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="216b8-618">設定</span><span class="sxs-lookup"><span data-stu-id="216b8-618">Configuration</span></span>

<span data-ttu-id="216b8-619">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="216b8-619">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="216b8-620">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="216b8-620">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="216b8-621">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="216b8-621">Command-line arguments.</span></span>
* <span data-ttu-id="216b8-622">環境變數。</span><span class="sxs-lookup"><span data-stu-id="216b8-622">Environment variables.</span></span>
* <span data-ttu-id="216b8-623">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="216b8-623">In-memory .NET objects.</span></span>
* <span data-ttu-id="216b8-624">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="216b8-624">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="216b8-625">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-625">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="216b8-626">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="216b8-626">Custom providers (installed or created).</span></span>

<span data-ttu-id="216b8-627">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="216b8-627">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="216b8-628">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="216b8-628">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="216b8-629">`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。</span><span class="sxs-lookup"><span data-stu-id="216b8-629">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="216b8-630">`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="216b8-630">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="216b8-631">在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-631">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="216b8-632">`Logging` 下的其他屬性可指定記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-632">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="216b8-633">範例使用主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-633">The example is for the Console provider.</span></span> <span data-ttu-id="216b8-634">如果提供者支援[記錄範圍](#log-scopes)， `IncludeScopes`則指出是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="216b8-634">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="216b8-635">提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="216b8-635">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="216b8-636">提供者下的 `LogLevel` 會指定提供者的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-636">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="216b8-637">若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="216b8-637">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

<span data-ttu-id="216b8-638">記錄 API 不包含在應用程式執行時變更記錄層級的案例。</span><span class="sxs-lookup"><span data-stu-id="216b8-638">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="216b8-639">不過，某些設定提供者能夠重載設定，這會立即影響記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-639">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="216b8-640">例如，檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)（由`CreateDefaultBuilder`新增）以讀取設定檔案，預設會重載記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-640">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="216b8-641">如果應用程式正在執行時，程式碼中的設定有所變更，應用程式可以呼叫[IConfigurationRoot](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)來更新應用程式的記錄設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-641">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="216b8-642">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="216b8-642">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="216b8-643">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="216b8-643">Sample logging output</span></span>

<span data-ttu-id="216b8-644">使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="216b8-644">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="216b8-645">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="216b8-645">Here's an example of console output:</span></span>

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

<span data-ttu-id="216b8-646">上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。</span><span class="sxs-lookup"><span data-stu-id="216b8-646">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="216b8-647">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="216b8-647">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="216b8-648">這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApi" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="216b8-648">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="216b8-649">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="216b8-649">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="216b8-650">ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-650">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="216b8-651">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="216b8-651">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="216b8-652">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="216b8-652">NuGet packages</span></span>

<span data-ttu-id="216b8-653">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="216b8-653">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="216b8-654">記錄分類</span><span class="sxs-lookup"><span data-stu-id="216b8-654">Log category</span></span>

<span data-ttu-id="216b8-655">建立 `ILogger` 物件時，會為它指定「類別」\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-655">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="216b8-656">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="216b8-656">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="216b8-657">類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="216b8-657">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="216b8-658">使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：</span><span class="sxs-lookup"><span data-stu-id="216b8-658">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="216b8-659">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="216b8-659">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="216b8-660">`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="216b8-660">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="216b8-661">記錄層級</span><span class="sxs-lookup"><span data-stu-id="216b8-661">Log level</span></span>

<span data-ttu-id="216b8-662">每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。</span><span class="sxs-lookup"><span data-stu-id="216b8-662">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="216b8-663">記錄層級表示嚴重性或重要性。</span><span class="sxs-lookup"><span data-stu-id="216b8-663">The log level indicates the severity or importance.</span></span> <span data-ttu-id="216b8-664">例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-664">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="216b8-665">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="216b8-665">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="216b8-666">在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="216b8-666">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="216b8-667">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="216b8-667">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="216b8-668">此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="216b8-668">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="216b8-669">在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。</span><span class="sxs-lookup"><span data-stu-id="216b8-669">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="216b8-670">這些方法會呼叫接受 `Log` 參數的 `LogLevel`。</span><span class="sxs-lookup"><span data-stu-id="216b8-670">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="216b8-671">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="216b8-671">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="216b8-672">如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="216b8-672">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="216b8-673">ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="216b8-673">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="216b8-674">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="216b8-674">Trace = 0</span></span>

  <span data-ttu-id="216b8-675">針對通常只對偵錯有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-675">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="216b8-676">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="216b8-676">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="216b8-677">*預設為停用。*</span><span class="sxs-lookup"><span data-stu-id="216b8-677">*Disabled by default.*</span></span>

* <span data-ttu-id="216b8-678">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="216b8-678">Debug = 1</span></span>

  <span data-ttu-id="216b8-679">針對可在開發與偵錯中使用的資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-679">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="216b8-680">範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-680">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="216b8-681">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="216b8-681">Information = 2</span></span>

  <span data-ttu-id="216b8-682">針對一般應用程式流程的追蹤。</span><span class="sxs-lookup"><span data-stu-id="216b8-682">For tracking the general flow of the app.</span></span> <span data-ttu-id="216b8-683">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="216b8-683">These logs typically have some long-term value.</span></span> <span data-ttu-id="216b8-684">範例： `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="216b8-684">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="216b8-685">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="216b8-685">Warning = 3</span></span>

  <span data-ttu-id="216b8-686">針對應用程式流程中發生的異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-686">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="216b8-687">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="216b8-687">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="216b8-688">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="216b8-688">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="216b8-689">範例： `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="216b8-689">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="216b8-690">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="216b8-690">Error = 4</span></span>

  <span data-ttu-id="216b8-691">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="216b8-691">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="216b8-692">這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="216b8-692">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="216b8-693">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="216b8-693">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="216b8-694">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="216b8-694">Critical = 5</span></span>

  <span data-ttu-id="216b8-695">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="216b8-695">For failures that require immediate attention.</span></span> <span data-ttu-id="216b8-696">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="216b8-696">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="216b8-697">使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="216b8-697">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="216b8-698">例如：</span><span class="sxs-lookup"><span data-stu-id="216b8-698">For example:</span></span>

* <span data-ttu-id="216b8-699">在生產環境中：</span><span class="sxs-lookup"><span data-stu-id="216b8-699">In production:</span></span>
  * <span data-ttu-id="216b8-700">`Trace`透過`Information`層級的記錄會產生大量的詳細記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-700">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="216b8-701">若要控制成本，而不超過資料儲存體限制`Trace` ， `Information`請將層級訊息記錄到高容量、低成本的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-701">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="216b8-702">`Warning`透過`Critical`層級登入通常會產生較少、較小的記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-702">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="216b8-703">因此，成本和儲存體限制通常不會造成問題，因此可讓您更靈活地選擇資料存放區。</span><span class="sxs-lookup"><span data-stu-id="216b8-703">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="216b8-704">在開發期間：</span><span class="sxs-lookup"><span data-stu-id="216b8-704">During development:</span></span>
  * <span data-ttu-id="216b8-705">將`Warning` `Critical`訊息記錄到主控台。</span><span class="sxs-lookup"><span data-stu-id="216b8-705">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="216b8-706">進行`Trace`疑難排解`Information`時，新增訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-706">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="216b8-707">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-707">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="216b8-708">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-708">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="216b8-709">此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-709">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="216b8-710">以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：</span><span class="sxs-lookup"><span data-stu-id="216b8-710">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="216b8-711">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="216b8-711">Log event ID</span></span>

<span data-ttu-id="216b8-712">每個記錄都可以指定「事件識別碼」\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-712">Each log can specify an *event ID*.</span></span> <span data-ttu-id="216b8-713">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="216b8-713">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="216b8-714">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="216b8-714">An event ID associates a set of events.</span></span> <span data-ttu-id="216b8-715">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="216b8-715">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="216b8-716">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="216b8-716">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="216b8-717">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="216b8-717">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="216b8-718">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="216b8-718">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="216b8-719">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="216b8-719">Log message template</span></span>

<span data-ttu-id="216b8-720">每個記錄都會指定訊息範本。</span><span class="sxs-lookup"><span data-stu-id="216b8-720">Each log specifies a message template.</span></span> <span data-ttu-id="216b8-721">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="216b8-721">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="216b8-722">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="216b8-722">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="216b8-723">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="216b8-723">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="216b8-724">請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：</span><span class="sxs-lookup"><span data-stu-id="216b8-724">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="216b8-725">此程式碼會使用依順序的參數值建立記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="216b8-725">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="216b8-726">記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="216b8-726">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="216b8-727">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="216b8-727">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="216b8-728">此資訊可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="216b8-728">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="216b8-729">例如，假設記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="216b8-729">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="216b8-730">若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="216b8-730">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="216b8-731">查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。</span><span class="sxs-lookup"><span data-stu-id="216b8-731">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="216b8-732">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="216b8-732">Logging exceptions</span></span>

<span data-ttu-id="216b8-733">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="216b8-733">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="216b8-734">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="216b8-734">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="216b8-735">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="216b8-735">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="216b8-736">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="216b8-736">Log filtering</span></span>

<span data-ttu-id="216b8-737">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-737">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="216b8-738">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="216b8-738">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="216b8-739">若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-739">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="216b8-740">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="216b8-740">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="216b8-741">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="216b8-741">Create filter rules in configuration</span></span>

<span data-ttu-id="216b8-742">專案範本程式碼會`CreateDefaultBuilder`呼叫以設定主控台、Debug 和 EventSource （ASP.NET Core 2.2 或更新版本）提供者的記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-742">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="216b8-743">`CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。</span><span class="sxs-lookup"><span data-stu-id="216b8-743">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="216b8-744">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="216b8-744">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="216b8-745">此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-745">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="216b8-746">建立 `ILogger` 物件時，會為每個提供者選取一個規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-746">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="216b8-747">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="216b8-747">Filter rules in code</span></span>

<span data-ttu-id="216b8-748">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="216b8-748">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="216b8-749">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-749">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="216b8-750">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-750">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="216b8-751">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="216b8-751">How filtering rules are applied</span></span>

<span data-ttu-id="216b8-752">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-752">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="216b8-753">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="216b8-753">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="216b8-754">Number</span><span class="sxs-lookup"><span data-stu-id="216b8-754">Number</span></span> | <span data-ttu-id="216b8-755">提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-755">Provider</span></span>      | <span data-ttu-id="216b8-756">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="216b8-756">Categories that begin with ...</span></span>          | <span data-ttu-id="216b8-757">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="216b8-757">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="216b8-758">1</span><span class="sxs-lookup"><span data-stu-id="216b8-758">1</span></span>      | <span data-ttu-id="216b8-759">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-759">Debug</span></span>         | <span data-ttu-id="216b8-760">所有類別</span><span class="sxs-lookup"><span data-stu-id="216b8-760">All categories</span></span>                          | <span data-ttu-id="216b8-761">資訊</span><span class="sxs-lookup"><span data-stu-id="216b8-761">Information</span></span>       |
| <span data-ttu-id="216b8-762">2</span><span class="sxs-lookup"><span data-stu-id="216b8-762">2</span></span>      | <span data-ttu-id="216b8-763">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-763">Console</span></span>       | <span data-ttu-id="216b8-764">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="216b8-764">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="216b8-765">警告</span><span class="sxs-lookup"><span data-stu-id="216b8-765">Warning</span></span>           |
| <span data-ttu-id="216b8-766">3</span><span class="sxs-lookup"><span data-stu-id="216b8-766">3</span></span>      | <span data-ttu-id="216b8-767">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-767">Console</span></span>       | <span data-ttu-id="216b8-768">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="216b8-768">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="216b8-769">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-769">Debug</span></span>             |
| <span data-ttu-id="216b8-770">4</span><span class="sxs-lookup"><span data-stu-id="216b8-770">4</span></span>      | <span data-ttu-id="216b8-771">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-771">Console</span></span>       | <span data-ttu-id="216b8-772">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="216b8-772">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="216b8-773">錯誤</span><span class="sxs-lookup"><span data-stu-id="216b8-773">Error</span></span>             |
| <span data-ttu-id="216b8-774">5</span><span class="sxs-lookup"><span data-stu-id="216b8-774">5</span></span>      | <span data-ttu-id="216b8-775">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-775">Console</span></span>       | <span data-ttu-id="216b8-776">所有類別</span><span class="sxs-lookup"><span data-stu-id="216b8-776">All categories</span></span>                          | <span data-ttu-id="216b8-777">資訊</span><span class="sxs-lookup"><span data-stu-id="216b8-777">Information</span></span>       |
| <span data-ttu-id="216b8-778">6</span><span class="sxs-lookup"><span data-stu-id="216b8-778">6</span></span>      | <span data-ttu-id="216b8-779">所有提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-779">All providers</span></span> | <span data-ttu-id="216b8-780">所有類別</span><span class="sxs-lookup"><span data-stu-id="216b8-780">All categories</span></span>                          | <span data-ttu-id="216b8-781">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-781">Debug</span></span>             |
| <span data-ttu-id="216b8-782">7</span><span class="sxs-lookup"><span data-stu-id="216b8-782">7</span></span>      | <span data-ttu-id="216b8-783">所有提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-783">All providers</span></span> | <span data-ttu-id="216b8-784">System</span><span class="sxs-lookup"><span data-stu-id="216b8-784">System</span></span>                                  | <span data-ttu-id="216b8-785">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-785">Debug</span></span>             |
| <span data-ttu-id="216b8-786">8</span><span class="sxs-lookup"><span data-stu-id="216b8-786">8</span></span>      | <span data-ttu-id="216b8-787">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-787">Debug</span></span>         | <span data-ttu-id="216b8-788">Microsoft</span><span class="sxs-lookup"><span data-stu-id="216b8-788">Microsoft</span></span>                               | <span data-ttu-id="216b8-789">追蹤</span><span class="sxs-lookup"><span data-stu-id="216b8-789">Trace</span></span>             |

<span data-ttu-id="216b8-790">建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="216b8-790">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="216b8-791">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="216b8-791">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="216b8-792">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-792">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="216b8-793">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="216b8-793">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="216b8-794">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-794">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="216b8-795">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-795">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="216b8-796">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-796">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="216b8-797">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-797">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="216b8-798">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="216b8-798">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="216b8-799">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="216b8-799">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="216b8-800">使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="216b8-800">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="216b8-801">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="216b8-801">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="216b8-802">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="216b8-802">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="216b8-803">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="216b8-803">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="216b8-804">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="216b8-804">Rule 3 is most specific.</span></span>

<span data-ttu-id="216b8-805">產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-805">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="216b8-806">`Debug` 層級以上的記錄會傳送到主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-806">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="216b8-807">提供者別名</span><span class="sxs-lookup"><span data-stu-id="216b8-807">Provider aliases</span></span>

<span data-ttu-id="216b8-808">每個提供者都會定義「別名」\*\*，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="216b8-808">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="216b8-809">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="216b8-809">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="216b8-810">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-810">Console</span></span>
* <span data-ttu-id="216b8-811">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-811">Debug</span></span>
* <span data-ttu-id="216b8-812">EventSource</span><span class="sxs-lookup"><span data-stu-id="216b8-812">EventSource</span></span>
* <span data-ttu-id="216b8-813">EventLog</span><span class="sxs-lookup"><span data-stu-id="216b8-813">EventLog</span></span>
* <span data-ttu-id="216b8-814">TraceSource</span><span class="sxs-lookup"><span data-stu-id="216b8-814">TraceSource</span></span>
* <span data-ttu-id="216b8-815">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="216b8-815">AzureAppServicesFile</span></span>
* <span data-ttu-id="216b8-816">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="216b8-816">AzureAppServicesBlob</span></span>
* <span data-ttu-id="216b8-817">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="216b8-817">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="216b8-818">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="216b8-818">Default minimum level</span></span>

<span data-ttu-id="216b8-819">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="216b8-819">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="216b8-820">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="216b8-820">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="216b8-821">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-821">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="216b8-822">篩選函數</span><span class="sxs-lookup"><span data-stu-id="216b8-822">Filter functions</span></span>

<span data-ttu-id="216b8-823">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="216b8-823">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="216b8-824">函式中的程式碼可以存取提供者類型、類別與記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-824">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="216b8-825">例如：</span><span class="sxs-lookup"><span data-stu-id="216b8-825">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a><span data-ttu-id="216b8-826">系統類別與層級</span><span class="sxs-lookup"><span data-stu-id="216b8-826">System categories and levels</span></span>

<span data-ttu-id="216b8-827">以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：</span><span class="sxs-lookup"><span data-stu-id="216b8-827">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="216b8-828">類別</span><span class="sxs-lookup"><span data-stu-id="216b8-828">Category</span></span>                            | <span data-ttu-id="216b8-829">注意</span><span class="sxs-lookup"><span data-stu-id="216b8-829">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="216b8-830">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="216b8-830">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="216b8-831">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="216b8-831">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="216b8-832">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="216b8-832">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="216b8-833">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="216b8-833">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="216b8-834">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="216b8-834">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="216b8-835">允許主機。</span><span class="sxs-lookup"><span data-stu-id="216b8-835">Hosts allowed.</span></span> |
| <span data-ttu-id="216b8-836">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="216b8-836">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="216b8-837">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="216b8-837">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="216b8-838">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="216b8-838">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="216b8-839">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="216b8-839">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="216b8-840">MVC 與 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="216b8-840">MVC and Razor diagnostics.</span></span> <span data-ttu-id="216b8-841">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="216b8-841">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="216b8-842">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="216b8-842">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="216b8-843">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-843">Route matching information.</span></span> |
| <span data-ttu-id="216b8-844">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="216b8-844">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="216b8-845">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="216b8-845">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="216b8-846">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="216b8-846">HTTPS certificate information.</span></span> |
| <span data-ttu-id="216b8-847">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="216b8-847">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="216b8-848">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="216b8-848">Files served.</span></span> |
| <span data-ttu-id="216b8-849">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="216b8-849">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="216b8-850">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="216b8-850">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="216b8-851">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="216b8-851">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="216b8-852">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="216b8-852">Log scopes</span></span>

 <span data-ttu-id="216b8-853">「範圍」\*\* 可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="216b8-853">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="216b8-854">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-854">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="216b8-855">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="216b8-855">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="216b8-856">範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="216b8-856">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="216b8-857">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="216b8-857">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="216b8-858">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="216b8-858">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="216b8-859">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="216b8-859">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="216b8-860">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-860">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="216b8-861">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="216b8-861">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="216b8-862">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="216b8-862">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="216b8-863">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-863">Built-in logging providers</span></span>

<span data-ttu-id="216b8-864">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="216b8-864">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="216b8-865">主控台</span><span class="sxs-lookup"><span data-stu-id="216b8-865">Console</span></span>](#console-provider)
* [<span data-ttu-id="216b8-866">偵錯</span><span class="sxs-lookup"><span data-stu-id="216b8-866">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="216b8-867">EventSource</span><span class="sxs-lookup"><span data-stu-id="216b8-867">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="216b8-868">EventLog</span><span class="sxs-lookup"><span data-stu-id="216b8-868">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="216b8-869">TraceSource</span><span class="sxs-lookup"><span data-stu-id="216b8-869">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="216b8-870">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="216b8-870">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="216b8-871">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="216b8-871">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="216b8-872">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="216b8-872">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="216b8-873">如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="216b8-873">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="216b8-874">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-874">Console provider</span></span>

<span data-ttu-id="216b8-875">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="216b8-875">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="216b8-876">若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="216b8-876">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="216b8-877">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-877">Debug provider</span></span>

<span data-ttu-id="216b8-878">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="216b8-878">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="216b8-879">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="216b8-879">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="216b8-880">事件來源提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-880">Event Source provider</span></span>

<span data-ttu-id="216b8-881">在[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)跨平臺的事件來源中，會寫入名`Microsoft-Extensions-Logging`為的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="216b8-881">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="216b8-882">在 Windows 上，提供者會使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="216b8-882">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="216b8-883">呼叫以建立主機時`CreateDefaultBuilder` ，會自動新增事件來源提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-883">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

<span data-ttu-id="216b8-884">使用[PerfView 公用程式](https://github.com/Microsoft/perfview)來收集及查看記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-884">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="216b8-885">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="216b8-885">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="216b8-886">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]\*\*\*\* 清單</span><span class="sxs-lookup"><span data-stu-id="216b8-886">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="216b8-887">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="216b8-887">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="216b8-889">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-889">Windows EventLog provider</span></span>

<span data-ttu-id="216b8-890">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="216b8-890">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="216b8-891">[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。</span><span class="sxs-lookup"><span data-stu-id="216b8-891">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="216b8-892">若`null`未指定，則會使用下列預設設定：</span><span class="sxs-lookup"><span data-stu-id="216b8-892">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="216b8-893">`LogName`&ndash; 「應用程式」</span><span class="sxs-lookup"><span data-stu-id="216b8-893">`LogName` &ndash; "Application"</span></span>
* <span data-ttu-id="216b8-894">`SourceName`&ndash; 「.Net 執行時間」</span><span class="sxs-lookup"><span data-stu-id="216b8-894">`SourceName` &ndash; ".NET Runtime"</span></span>
* <span data-ttu-id="216b8-895">`MachineName` &ndash; 本機電腦</span><span class="sxs-lookup"><span data-stu-id="216b8-895">`MachineName` &ndash; local machine</span></span>

<span data-ttu-id="216b8-896">記錄[警告層級和更新版本](#log-level)的事件。</span><span class="sxs-lookup"><span data-stu-id="216b8-896">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="216b8-897">若要記錄低於`Warning`的事件，請明確設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="216b8-897">To log events lower than `Warning`, explicitly set the log level.</span></span> <span data-ttu-id="216b8-898">例如，將下列內容新增至*appsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="216b8-898">For example, add the following to the *appsettings.json* file:</span></span>

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="216b8-899">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-899">TraceSource provider</span></span>

<span data-ttu-id="216b8-900">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-900">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="216b8-901">[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="216b8-901">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="216b8-902">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="216b8-902">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="216b8-903">該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。</span><span class="sxs-lookup"><span data-stu-id="216b8-903">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="216b8-904">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-904">Azure App Service provider</span></span>

<span data-ttu-id="216b8-905">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="216b8-905">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="216b8-906">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。</span><span class="sxs-lookup"><span data-stu-id="216b8-906">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="216b8-907">當以 .NET Framework 為目標或是參考 `Microsoft.AspNetCore.App` 中繼套件時，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="216b8-907">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

<span data-ttu-id="216b8-908"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。</span><span class="sxs-lookup"><span data-stu-id="216b8-908">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="216b8-909">設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。</span><span class="sxs-lookup"><span data-stu-id="216b8-909">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="216b8-910">(*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)</span><span class="sxs-lookup"><span data-stu-id="216b8-910">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

<span data-ttu-id="216b8-911">當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]\*\*\*\* 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。</span><span class="sxs-lookup"><span data-stu-id="216b8-911">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="216b8-912">當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="216b8-912">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="216b8-913">**應用程式記錄 (檔案系統)**</span><span class="sxs-lookup"><span data-stu-id="216b8-913">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="216b8-914">**應用程式記錄 (Blob)**</span><span class="sxs-lookup"><span data-stu-id="216b8-914">**Application Logging (Blob)**</span></span>

<span data-ttu-id="216b8-915">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="216b8-915">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="216b8-916">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="216b8-916">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="216b8-917">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="216b8-917">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="216b8-918">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="216b8-918">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="216b8-919">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="216b8-919">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="216b8-920">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="216b8-920">Azure log streaming</span></span>

<span data-ttu-id="216b8-921">Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="216b8-921">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="216b8-922">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="216b8-922">The app server</span></span>
* <span data-ttu-id="216b8-923">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="216b8-923">The web server</span></span>
* <span data-ttu-id="216b8-924">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="216b8-924">Failed request tracing</span></span>

<span data-ttu-id="216b8-925">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="216b8-925">To configure Azure log streaming:</span></span>

* <span data-ttu-id="216b8-926">從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-926">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="216b8-927">將 [應用程式記錄 (檔案系統)]\*\*\*\* 設定為 [開啟]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-927">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="216b8-928">選擇記錄 [層級]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="216b8-928">Choose the log **Level**.</span></span> <span data-ttu-id="216b8-929">此設定僅適用于 Azure 記錄串流，而不適用於應用程式中的其他記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-929">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="216b8-930">瀏覽到 [記錄資料流]\*\*\*\* 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-930">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="216b8-931">這些是應用程式透過 `ILogger` 介面產生的訊息。</span><span class="sxs-lookup"><span data-stu-id="216b8-931">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="216b8-932">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="216b8-932">Azure Application Insights trace logging</span></span>

<span data-ttu-id="216b8-933">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。</span><span class="sxs-lookup"><span data-stu-id="216b8-933">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="216b8-934">Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。</span><span class="sxs-lookup"><span data-stu-id="216b8-934">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="216b8-935">如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。</span><span class="sxs-lookup"><span data-stu-id="216b8-935">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="216b8-936">記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。</span><span class="sxs-lookup"><span data-stu-id="216b8-936">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="216b8-937">如果您使用此套件，就不需安裝提供者套件。</span><span class="sxs-lookup"><span data-stu-id="216b8-937">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="216b8-938">不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="216b8-938">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="216b8-939">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="216b8-939">For more information, see the following resources:</span></span>

* [<span data-ttu-id="216b8-940">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="216b8-940">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="216b8-941">[適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="216b8-941">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="216b8-942">[適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="216b8-942">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="216b8-943">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="216b8-943">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="216b8-944">[安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。</span><span class="sxs-lookup"><span data-stu-id="216b8-944">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="216b8-945">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="216b8-945">Third-party logging providers</span></span>

<span data-ttu-id="216b8-946">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="216b8-946">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="216b8-947">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-947">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="216b8-948">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-948">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="216b8-949">[JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="216b8-949">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="216b8-950">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="216b8-950">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="216b8-951">[Log4Net](https://logging.apache.org/log4net/) （[GitHub](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore)存放庫）</span><span class="sxs-lookup"><span data-stu-id="216b8-951">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="216b8-952">[Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-952">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="216b8-953">[NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="216b8-953">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="216b8-954">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="216b8-954">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="216b8-955">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="216b8-955">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="216b8-956">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="216b8-956">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="216b8-957">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="216b8-957">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="216b8-958">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="216b8-958">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="216b8-959">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="216b8-959">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="216b8-960">通話記錄`ILoggerFactory`架構所提供的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="216b8-960">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="216b8-961">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="216b8-961">For more information, see each provider's documentation.</span></span> <span data-ttu-id="216b8-962">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="216b8-962">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="216b8-963">其他資源</span><span class="sxs-lookup"><span data-stu-id="216b8-963">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
