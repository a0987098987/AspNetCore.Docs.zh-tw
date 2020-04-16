---
title: .NET Core 與 ASP.NET Core 中的記錄
author: rick-anderson
description: 了解如何使用由 Microsoft.Extensions.Logging NuGet 套件提供的記錄架構。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 4/17/2020
uid: fundamentals/logging/index
ms.openlocfilehash: a3c63b738d3eaa51249475b88d78572038348a7a
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440736"
---
# <a name="logging-in-net-core-and-aspnet-core"></a><span data-ttu-id="255ce-103">.NET Core 與 ASP.NET Core 中的記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-103">Logging in .NET Core and ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="255ce-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="255ce-104">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="255ce-105">.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="255ce-105">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="255ce-106">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="255ce-106">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="255ce-107">本文中顯示的大部分程式碼範例都來自 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="255ce-107">Most of the code examples shown in this article are from ASP.NET Core apps.</span></span> <span data-ttu-id="255ce-108">這些代碼段的特定於日誌記錄的部分適用於使用[通用主機](xref:fundamentals/host/generic-host)的任何 .NET Core 應用。</span><span class="sxs-lookup"><span data-stu-id="255ce-108">The logging-specific parts of these code snippets apply to any .NET Core app that uses the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="255ce-109">有關如何在非 Web 控制台應用中使用通用主機的範例,請參閱[後台任務範例應用](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples)() 的<xref:fundamentals/host/hosted-services>*Program.cs*檔。</span><span class="sxs-lookup"><span data-stu-id="255ce-109">For an example of how to use the Generic Host in a non-web console app, see the *Program.cs* file of the [Background Tasks sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples) (<xref:fundamentals/host/hosted-services>).</span></span>

<span data-ttu-id="255ce-110">不含一般主機的應用程式記錄程式碼，會因[新增提供者](#add-providers)和[建立記錄器](#create-logs)的方式而有所不同。</span><span class="sxs-lookup"><span data-stu-id="255ce-110">Logging code for apps without Generic Host differs in the way [providers are added](#add-providers) and [loggers are created](#create-logs).</span></span> <span data-ttu-id="255ce-111">非主機程式碼範例顯示於本文的這些章節中。</span><span class="sxs-lookup"><span data-stu-id="255ce-111">Non-host code examples are shown in those sections of the article.</span></span>

<span data-ttu-id="255ce-112">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="255ce-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="255ce-113">新增提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-113">Add providers</span></span>

<span data-ttu-id="255ce-114">記錄提供者會顯示或儲存記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-114">A logging provider displays or stores logs.</span></span> <span data-ttu-id="255ce-115">例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="255ce-115">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="255ce-116">您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。</span><span class="sxs-lookup"><span data-stu-id="255ce-116">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

<span data-ttu-id="255ce-117">若要在使用一般主機的應用程式中新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="255ce-117">To add a provider in an app that uses Generic Host, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=6)]

<span data-ttu-id="255ce-118">在非主機主控台應用程式中，於建立 `LoggerFactory` 時呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="255ce-118">In a non-host console app, call the provider's `Add{provider name}` extension method while creating a `LoggerFactory`:</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=1,7)]

<span data-ttu-id="255ce-119">`LoggerFactory` 和 `AddConsole` 需要 `Microsoft.Extensions.Logging` 的 `using` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="255ce-119">`LoggerFactory` and `AddConsole` require a `using` statement for `Microsoft.Extensions.Logging`.</span></span>

<span data-ttu-id="255ce-120">預設 ASP.NET Core 專案範本會呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> 新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="255ce-120">The default ASP.NET Core project templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* [<span data-ttu-id="255ce-121">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-121">Console</span></span>](#console-provider)
* [<span data-ttu-id="255ce-122">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-122">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="255ce-123">事件來源</span><span class="sxs-lookup"><span data-stu-id="255ce-123">EventSource</span></span>](#event-source-provider)
* <span data-ttu-id="255ce-124">[事件紀錄](#windows-eventlog-provider)(只有 Windows 執行時)</span><span class="sxs-lookup"><span data-stu-id="255ce-124">[EventLog](#windows-eventlog-provider) (only when running on Windows)</span></span>

<span data-ttu-id="255ce-125">您可以使用您想要的提供者來取代預設提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-125">You can replace the default providers with your own choices.</span></span> <span data-ttu-id="255ce-126">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-126">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AddProvider&highlight=5)]

<span data-ttu-id="255ce-127">在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="255ce-127">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="255ce-128">建立記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-128">Create logs</span></span>

<span data-ttu-id="255ce-129">若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="255ce-129">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="255ce-130">在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-130">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="255ce-131">在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-131">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="255ce-132">下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-132">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="255ce-133">記錄「類別」\*\* 是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="255ce-133">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="255ce-134">由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-134">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="255ce-135">下列非主機主控台應用程式範例會建立以 `LoggingConsoleApp.Program` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-135">The following non-host console app example creates a logger with `LoggingConsoleApp.Program` as the category.</span></span>

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=10)]

<span data-ttu-id="255ce-136">在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-136">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="255ce-137">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="255ce-137">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

[!code-csharp[](index/samples/3.x/LoggingConsoleApp/Program.cs?name=snippet_LoggerFactory&highlight=11)]

<span data-ttu-id="255ce-138">此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="255ce-138">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span>

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="255ce-139">在 Program 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-139">Create logs in the Program class</span></span>

<span data-ttu-id="255ce-140">若要在 ASP.NET Core 應用程式的 `Program` 類別中寫入記錄，請在建立主機之後從 DI 取得 `ILogger` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="255ce-140">To write logs in the `Program` class of an ASP.NET Core app, get an `ILogger` instance from DI after building the host:</span></span>

[!code-csharp[](index/samples_snapshot/3.x/TodoApiSample/Program.cs?highlight=9,10)]

<span data-ttu-id="255ce-141">不支援在主機構造期間進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-141">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="255ce-142">但是,可以使用單獨的記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-142">However, a separate logger can be used.</span></span> <span data-ttu-id="255ce-143">在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateHostBuilder`。 </span><span class="sxs-lookup"><span data-stu-id="255ce-143">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateHostBuilder`.</span></span> <span data-ttu-id="255ce-144">`AddSerilog`使用 在`Log.Logger`中指定的靜態配置。</span><span class="sxs-lookup"><span data-stu-id="255ce-144">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="create-logs-in-the-startup-class"></a><span data-ttu-id="255ce-145">在 Startup 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-145">Create logs in the Startup class</span></span>

<span data-ttu-id="255ce-146">若要在 ASP.NET Core 應用程式的 `Startup.Configure` 方法中寫入記錄，請在方法簽章中包含 `ILogger` 參數：</span><span class="sxs-lookup"><span data-stu-id="255ce-146">To write logs in the `Startup.Configure` method of an ASP.NET Core app, include an `ILogger` parameter in the method signature:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_Configure&highlight=1,5)]

<span data-ttu-id="255ce-147">在以 `Startup.ConfigureServices` 方法完成 DI 容器設定之前，不支援寫入記錄檔：</span><span class="sxs-lookup"><span data-stu-id="255ce-147">Writing logs before completion of the DI container setup in the `Startup.ConfigureServices` method is not supported:</span></span>

* <span data-ttu-id="255ce-148">不支援將記錄器插入 `Startup` 建構函式。</span><span class="sxs-lookup"><span data-stu-id="255ce-148">Logger injection into the `Startup` constructor is not supported.</span></span>
* <span data-ttu-id="255ce-149">不支援將記錄器插入 `Startup.ConfigureServices` 方法簽章</span><span class="sxs-lookup"><span data-stu-id="255ce-149">Logger injection into the `Startup.ConfigureServices` method signature is not supported</span></span>

<span data-ttu-id="255ce-150">這項限制的原因是記錄相依於 DI 和組態，而後者相依於 DI。</span><span class="sxs-lookup"><span data-stu-id="255ce-150">The reason for this restriction is that logging depends on DI and on configuration, which in turns depends on DI.</span></span> <span data-ttu-id="255ce-151">在 `ConfigureServices` 完成後才會設定 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="255ce-151">The DI container isn't set up until `ConfigureServices` finishes.</span></span>

<span data-ttu-id="255ce-152">記錄器的建構函式插入 `Startup` 適用於舊版 ASP.NET Core，因為會為 Web 主機建立個別的 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="255ce-152">Constructor injection of a logger into `Startup` works in earlier versions of ASP.NET Core because a separate DI container is created for the Web Host.</span></span> <span data-ttu-id="255ce-153">如需為何只為一般主機建立一個容器的資訊，請參閱[重大變更公告](https://github.com/aspnet/Announcements/issues/353)。</span><span class="sxs-lookup"><span data-stu-id="255ce-153">For information about why only one container is created for the Generic Host, see the [breaking change announcement](https://github.com/aspnet/Announcements/issues/353).</span></span>

<span data-ttu-id="255ce-154">如果您需要設定相依於 `ILogger<T>` 的服務，您仍然可以使用建構函式插入或提供 Factory 方法來執行此動作。</span><span class="sxs-lookup"><span data-stu-id="255ce-154">If you need to configure a service that depends on `ILogger<T>`, you can still do that by using constructor injection or by providing a factory method.</span></span> <span data-ttu-id="255ce-155">只有在沒有其他選項時，才建議使用 Factory 方法。</span><span class="sxs-lookup"><span data-stu-id="255ce-155">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="255ce-156">例如，假設您需要使用 DI 的服務來填入屬性：</span><span class="sxs-lookup"><span data-stu-id="255ce-156">For example, suppose you need to fill a property with a service from DI:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Startup.cs?name=snippet_ConfigureServices&highlight=6-10)]

<span data-ttu-id="255ce-157">前面的醒目提示程式碼是 `Func`，會在 DI 容器第一次需要建立 `MyService` 的執行個體時執行。</span><span class="sxs-lookup"><span data-stu-id="255ce-157">The preceding highlighted code is a `Func` that runs the first time the DI container needs to construct an instance of `MyService`.</span></span> <span data-ttu-id="255ce-158">您可以用此方式存取任何已註冊的服務。</span><span class="sxs-lookup"><span data-stu-id="255ce-158">You can access any of the registered services in this way.</span></span>

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="255ce-159">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="255ce-159">No asynchronous logger methods</span></span>

<span data-ttu-id="255ce-160">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="255ce-160">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="255ce-161">若您的記錄資料存放區很慢，請不要直接寫入其中。</span><span class="sxs-lookup"><span data-stu-id="255ce-161">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="255ce-162">請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="255ce-162">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="255ce-163">例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="255ce-163">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="255ce-164">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="255ce-164">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="255ce-165">有關詳細資訊,請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="255ce-165">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="255ce-166">組態</span><span class="sxs-lookup"><span data-stu-id="255ce-166">Configuration</span></span>

<span data-ttu-id="255ce-167">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="255ce-167">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="255ce-168">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="255ce-168">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="255ce-169">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="255ce-169">Command-line arguments.</span></span>
* <span data-ttu-id="255ce-170">環境變數。</span><span class="sxs-lookup"><span data-stu-id="255ce-170">Environment variables.</span></span>
* <span data-ttu-id="255ce-171">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="255ce-171">In-memory .NET objects.</span></span>
* <span data-ttu-id="255ce-172">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="255ce-172">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="255ce-173">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="255ce-173">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="255ce-174">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="255ce-174">Custom providers (installed or created).</span></span>

<span data-ttu-id="255ce-175">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="255ce-175">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="255ce-176">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="255ce-176">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="255ce-177">`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。</span><span class="sxs-lookup"><span data-stu-id="255ce-177">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="255ce-178">`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="255ce-178">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="255ce-179">在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-179">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="255ce-180">`Logging` 下的其他屬性可指定記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-180">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="255ce-181">範例使用主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-181">The example is for the Console provider.</span></span> <span data-ttu-id="255ce-182">如果提供程式支援[日誌作用域](#log-scopes),`IncludeScopes`則指示它們是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="255ce-182">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="255ce-183">提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="255ce-183">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="255ce-184">提供者下的 `LogLevel` 會指定提供者的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-184">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="255ce-185">若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="255ce-185">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

<span data-ttu-id="255ce-186">日誌記錄 API 不包括在應用運行時更改日誌級別的方案。</span><span class="sxs-lookup"><span data-stu-id="255ce-186">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="255ce-187">但是,某些配置提供程式能夠重新載入配置,這將立即對日誌記錄配置產生影響。</span><span class="sxs-lookup"><span data-stu-id="255ce-187">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="255ce-188">例如,[檔案設定提供者](xref:fundamentals/configuration/index#file-configuration-provider)`CreateDefaultBuilder`() 被添加到讀取設定檔,預設情況下重新載入紀錄記錄配置。</span><span class="sxs-lookup"><span data-stu-id="255ce-188">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="255ce-189">如果在應用運行時的代碼中更改了配置,則應用可以調用[I配置Root.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)以更新應用的日誌記錄配置。</span><span class="sxs-lookup"><span data-stu-id="255ce-189">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="255ce-190">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="255ce-190">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="255ce-191">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="255ce-191">Sample logging output</span></span>

<span data-ttu-id="255ce-192">使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="255ce-192">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="255ce-193">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="255ce-193">Here's an example of console output:</span></span>

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

<span data-ttu-id="255ce-194">上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。</span><span class="sxs-lookup"><span data-stu-id="255ce-194">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="255ce-195">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="255ce-195">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

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

<span data-ttu-id="255ce-196">這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApiSample" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="255ce-196">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApiSample".</span></span> <span data-ttu-id="255ce-197">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="255ce-197">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="255ce-198">ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-198">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="255ce-199">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="255ce-199">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="255ce-200">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="255ce-200">NuGet packages</span></span>

<span data-ttu-id="255ce-201">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="255ce-201">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="255ce-202">記錄分類</span><span class="sxs-lookup"><span data-stu-id="255ce-202">Log category</span></span>

<span data-ttu-id="255ce-203">建立 `ILogger` 物件時，會為它指定「類別」\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-203">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="255ce-204">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="255ce-204">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="255ce-205">類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="255ce-205">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="255ce-206">使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：</span><span class="sxs-lookup"><span data-stu-id="255ce-206">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="255ce-207">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="255ce-207">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="255ce-208">`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-208">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="255ce-209">記錄層級</span><span class="sxs-lookup"><span data-stu-id="255ce-209">Log level</span></span>

<span data-ttu-id="255ce-210">每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。</span><span class="sxs-lookup"><span data-stu-id="255ce-210">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="255ce-211">記錄層級表示嚴重性或重要性。</span><span class="sxs-lookup"><span data-stu-id="255ce-211">The log level indicates the severity or importance.</span></span> <span data-ttu-id="255ce-212">例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-212">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="255ce-213">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="255ce-213">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="255ce-214">在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="255ce-214">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="255ce-215">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="255ce-215">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="255ce-216">此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="255ce-216">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="255ce-217">在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。</span><span class="sxs-lookup"><span data-stu-id="255ce-217">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="255ce-218">這些方法會呼叫接受 `Log` 參數的 `LogLevel`。</span><span class="sxs-lookup"><span data-stu-id="255ce-218">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="255ce-219">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="255ce-219">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="255ce-220">如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="255ce-220">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="255ce-221">ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="255ce-221">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="255ce-222">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="255ce-222">Trace = 0</span></span>

  <span data-ttu-id="255ce-223">針對通常只對偵錯有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-223">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="255ce-224">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="255ce-224">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="255ce-225">*預設為停用。*</span><span class="sxs-lookup"><span data-stu-id="255ce-225">*Disabled by default.*</span></span>

* <span data-ttu-id="255ce-226">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="255ce-226">Debug = 1</span></span>

  <span data-ttu-id="255ce-227">針對可在開發與偵錯中使用的資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-227">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="255ce-228">範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-228">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="255ce-229">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="255ce-229">Information = 2</span></span>

  <span data-ttu-id="255ce-230">針對一般應用程式流程的追蹤。</span><span class="sxs-lookup"><span data-stu-id="255ce-230">For tracking the general flow of the app.</span></span> <span data-ttu-id="255ce-231">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="255ce-231">These logs typically have some long-term value.</span></span> <span data-ttu-id="255ce-232">範例： `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="255ce-232">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="255ce-233">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="255ce-233">Warning = 3</span></span>

  <span data-ttu-id="255ce-234">針對應用程式流程中發生的異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-234">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="255ce-235">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="255ce-235">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="255ce-236">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-236">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="255ce-237">範例： `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="255ce-237">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="255ce-238">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="255ce-238">Error = 4</span></span>

  <span data-ttu-id="255ce-239">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="255ce-239">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="255ce-240">這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="255ce-240">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="255ce-241">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="255ce-241">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="255ce-242">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="255ce-242">Critical = 5</span></span>

  <span data-ttu-id="255ce-243">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="255ce-243">For failures that require immediate attention.</span></span> <span data-ttu-id="255ce-244">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="255ce-244">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="255ce-245">使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="255ce-245">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="255ce-246">例如：</span><span class="sxs-lookup"><span data-stu-id="255ce-246">For example:</span></span>

* <span data-ttu-id="255ce-247">在生產中:</span><span class="sxs-lookup"><span data-stu-id="255ce-247">In production:</span></span>
  * <span data-ttu-id="255ce-248">在`Trace``Information`通過級別日誌記錄會產生大量詳細的日誌消息。</span><span class="sxs-lookup"><span data-stu-id="255ce-248">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="255ce-249">為了控制成本且不超過數據存儲限制,請將`Trace``Information`級別消息記錄到高容量、低成本數據存儲。</span><span class="sxs-lookup"><span data-stu-id="255ce-249">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="255ce-250">在通過`Warning``Critical`級別進行日誌記錄通常會產生更少、更小的日誌消息。</span><span class="sxs-lookup"><span data-stu-id="255ce-250">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="255ce-251">因此,成本和存儲限制通常不是問題,因此在選擇數據存儲時具有更大的靈活性。</span><span class="sxs-lookup"><span data-stu-id="255ce-251">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="255ce-252">在開發過程中:</span><span class="sxs-lookup"><span data-stu-id="255ce-252">During development:</span></span>
  * <span data-ttu-id="255ce-253">將`Warning``Critical`消息記錄到主控台。</span><span class="sxs-lookup"><span data-stu-id="255ce-253">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="255ce-254">故障排除`Trace``Information`時 添加消息。</span><span class="sxs-lookup"><span data-stu-id="255ce-254">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="255ce-255">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-255">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="255ce-256">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-256">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="255ce-257">此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-257">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="255ce-258">以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：</span><span class="sxs-lookup"><span data-stu-id="255ce-258">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="255ce-259">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="255ce-259">Log event ID</span></span>

<span data-ttu-id="255ce-260">每個記錄都可以指定「事件識別碼」\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-260">Each log can specify an *event ID*.</span></span> <span data-ttu-id="255ce-261">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="255ce-261">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/3.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="255ce-262">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="255ce-262">An event ID associates a set of events.</span></span> <span data-ttu-id="255ce-263">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="255ce-263">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="255ce-264">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="255ce-264">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="255ce-265">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="255ce-265">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="255ce-266">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="255ce-266">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="255ce-267">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="255ce-267">Log message template</span></span>

<span data-ttu-id="255ce-268">每個記錄都會指定訊息範本。</span><span class="sxs-lookup"><span data-stu-id="255ce-268">Each log specifies a message template.</span></span> <span data-ttu-id="255ce-269">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-269">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="255ce-270">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-270">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="255ce-271">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="255ce-271">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="255ce-272">請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：</span><span class="sxs-lookup"><span data-stu-id="255ce-272">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="255ce-273">此程式碼會使用依順序的參數值建立記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="255ce-273">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="255ce-274">記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="255ce-274">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="255ce-275">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="255ce-275">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="255ce-276">此資訊可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="255ce-276">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="255ce-277">例如，假設記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="255ce-277">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="255ce-278">若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="255ce-278">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="255ce-279">查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。</span><span class="sxs-lookup"><span data-stu-id="255ce-279">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="255ce-280">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="255ce-280">Logging exceptions</span></span>

<span data-ttu-id="255ce-281">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="255ce-281">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="255ce-282">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="255ce-282">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="255ce-283">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="255ce-283">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="255ce-284">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="255ce-284">Log filtering</span></span>

<span data-ttu-id="255ce-285">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-285">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="255ce-286">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="255ce-286">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="255ce-287">若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-287">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="255ce-288">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="255ce-288">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="255ce-289">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="255ce-289">Create filter rules in configuration</span></span>

<span data-ttu-id="255ce-290">專案範本代碼調用`CreateDefaultBuilder`為主控台、除錯和事件來源(ASP.NET核心 2.2或更高版本)提供程式設定日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-290">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="255ce-291">`CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。</span><span class="sxs-lookup"><span data-stu-id="255ce-291">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="255ce-292">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="255ce-292">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/3.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="255ce-293">此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-293">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="255ce-294">建立 `ILogger` 物件時，會為每個提供者選取一個規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-294">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="255ce-295">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="255ce-295">Filter rules in code</span></span>

<span data-ttu-id="255ce-296">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="255ce-296">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=2-3)]

<span data-ttu-id="255ce-297">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-297">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="255ce-298">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-298">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="255ce-299">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="255ce-299">How filtering rules are applied</span></span>

<span data-ttu-id="255ce-300">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-300">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="255ce-301">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="255ce-301">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="255ce-302">Number</span><span class="sxs-lookup"><span data-stu-id="255ce-302">Number</span></span> | <span data-ttu-id="255ce-303">提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-303">Provider</span></span>      | <span data-ttu-id="255ce-304">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="255ce-304">Categories that begin with ...</span></span>          | <span data-ttu-id="255ce-305">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="255ce-305">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="255ce-306">1</span><span class="sxs-lookup"><span data-stu-id="255ce-306">1</span></span>      | <span data-ttu-id="255ce-307">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-307">Debug</span></span>         | <span data-ttu-id="255ce-308">所有類別</span><span class="sxs-lookup"><span data-stu-id="255ce-308">All categories</span></span>                          | <span data-ttu-id="255ce-309">資訊</span><span class="sxs-lookup"><span data-stu-id="255ce-309">Information</span></span>       |
| <span data-ttu-id="255ce-310">2</span><span class="sxs-lookup"><span data-stu-id="255ce-310">2</span></span>      | <span data-ttu-id="255ce-311">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-311">Console</span></span>       | <span data-ttu-id="255ce-312">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="255ce-312">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="255ce-313">警告</span><span class="sxs-lookup"><span data-stu-id="255ce-313">Warning</span></span>           |
| <span data-ttu-id="255ce-314">3</span><span class="sxs-lookup"><span data-stu-id="255ce-314">3</span></span>      | <span data-ttu-id="255ce-315">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-315">Console</span></span>       | <span data-ttu-id="255ce-316">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="255ce-316">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="255ce-317">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-317">Debug</span></span>             |
| <span data-ttu-id="255ce-318">4</span><span class="sxs-lookup"><span data-stu-id="255ce-318">4</span></span>      | <span data-ttu-id="255ce-319">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-319">Console</span></span>       | <span data-ttu-id="255ce-320">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="255ce-320">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="255ce-321">錯誤</span><span class="sxs-lookup"><span data-stu-id="255ce-321">Error</span></span>             |
| <span data-ttu-id="255ce-322">5</span><span class="sxs-lookup"><span data-stu-id="255ce-322">5</span></span>      | <span data-ttu-id="255ce-323">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-323">Console</span></span>       | <span data-ttu-id="255ce-324">所有類別</span><span class="sxs-lookup"><span data-stu-id="255ce-324">All categories</span></span>                          | <span data-ttu-id="255ce-325">資訊</span><span class="sxs-lookup"><span data-stu-id="255ce-325">Information</span></span>       |
| <span data-ttu-id="255ce-326">6</span><span class="sxs-lookup"><span data-stu-id="255ce-326">6</span></span>      | <span data-ttu-id="255ce-327">所有提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-327">All providers</span></span> | <span data-ttu-id="255ce-328">所有類別</span><span class="sxs-lookup"><span data-stu-id="255ce-328">All categories</span></span>                          | <span data-ttu-id="255ce-329">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-329">Debug</span></span>             |
| <span data-ttu-id="255ce-330">7</span><span class="sxs-lookup"><span data-stu-id="255ce-330">7</span></span>      | <span data-ttu-id="255ce-331">所有提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-331">All providers</span></span> | <span data-ttu-id="255ce-332">系統</span><span class="sxs-lookup"><span data-stu-id="255ce-332">System</span></span>                                  | <span data-ttu-id="255ce-333">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-333">Debug</span></span>             |
| <span data-ttu-id="255ce-334">8</span><span class="sxs-lookup"><span data-stu-id="255ce-334">8</span></span>      | <span data-ttu-id="255ce-335">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-335">Debug</span></span>         | <span data-ttu-id="255ce-336">Microsoft</span><span class="sxs-lookup"><span data-stu-id="255ce-336">Microsoft</span></span>                               | <span data-ttu-id="255ce-337">追蹤</span><span class="sxs-lookup"><span data-stu-id="255ce-337">Trace</span></span>             |

<span data-ttu-id="255ce-338">建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-338">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="255ce-339">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="255ce-339">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="255ce-340">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-340">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="255ce-341">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="255ce-341">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="255ce-342">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-342">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="255ce-343">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-343">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="255ce-344">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-344">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="255ce-345">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-345">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="255ce-346">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="255ce-346">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="255ce-347">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="255ce-347">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="255ce-348">使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="255ce-348">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="255ce-349">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="255ce-349">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="255ce-350">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-350">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="255ce-351">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="255ce-351">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="255ce-352">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="255ce-352">Rule 3 is most specific.</span></span>

<span data-ttu-id="255ce-353">產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-353">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="255ce-354">`Debug` 層級以上的記錄會傳送到主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-354">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="255ce-355">提供者別名</span><span class="sxs-lookup"><span data-stu-id="255ce-355">Provider aliases</span></span>

<span data-ttu-id="255ce-356">每個提供者都會定義「別名」\*\*，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="255ce-356">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="255ce-357">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="255ce-357">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="255ce-358">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-358">Console</span></span>
* <span data-ttu-id="255ce-359">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-359">Debug</span></span>
* <span data-ttu-id="255ce-360">EventSource</span><span class="sxs-lookup"><span data-stu-id="255ce-360">EventSource</span></span>
* <span data-ttu-id="255ce-361">EventLog</span><span class="sxs-lookup"><span data-stu-id="255ce-361">EventLog</span></span>
* <span data-ttu-id="255ce-362">TraceSource</span><span class="sxs-lookup"><span data-stu-id="255ce-362">TraceSource</span></span>
* <span data-ttu-id="255ce-363">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="255ce-363">AzureAppServicesFile</span></span>
* <span data-ttu-id="255ce-364">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="255ce-364">AzureAppServicesBlob</span></span>
* <span data-ttu-id="255ce-365">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="255ce-365">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="255ce-366">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="255ce-366">Default minimum level</span></span>

<span data-ttu-id="255ce-367">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="255ce-367">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="255ce-368">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="255ce-368">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="255ce-369">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-369">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="255ce-370">篩選函數</span><span class="sxs-lookup"><span data-stu-id="255ce-370">Filter functions</span></span>

<span data-ttu-id="255ce-371">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="255ce-371">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="255ce-372">函式中的程式碼可以存取提供者類型、類別與記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-372">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="255ce-373">例如：</span><span class="sxs-lookup"><span data-stu-id="255ce-373">For example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=3-11)]

## <a name="system-categories-and-levels"></a><span data-ttu-id="255ce-374">系統類別與層級</span><span class="sxs-lookup"><span data-stu-id="255ce-374">System categories and levels</span></span>

<span data-ttu-id="255ce-375">以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：</span><span class="sxs-lookup"><span data-stu-id="255ce-375">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="255ce-376">類別</span><span class="sxs-lookup"><span data-stu-id="255ce-376">Category</span></span>                            | <span data-ttu-id="255ce-377">注意</span><span class="sxs-lookup"><span data-stu-id="255ce-377">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="255ce-378">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="255ce-378">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="255ce-379">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="255ce-379">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="255ce-380">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="255ce-380">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="255ce-381">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="255ce-381">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="255ce-382">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="255ce-382">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="255ce-383">允許主機。</span><span class="sxs-lookup"><span data-stu-id="255ce-383">Hosts allowed.</span></span> |
| <span data-ttu-id="255ce-384">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="255ce-384">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="255ce-385">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="255ce-385">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="255ce-386">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="255ce-386">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="255ce-387">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="255ce-387">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="255ce-388">MVC 與 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="255ce-388">MVC and Razor diagnostics.</span></span> <span data-ttu-id="255ce-389">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="255ce-389">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="255ce-390">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="255ce-390">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="255ce-391">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-391">Route matching information.</span></span> |
| <span data-ttu-id="255ce-392">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="255ce-392">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="255ce-393">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="255ce-393">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="255ce-394">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-394">HTTPS certificate information.</span></span> |
| <span data-ttu-id="255ce-395">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="255ce-395">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="255ce-396">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="255ce-396">Files served.</span></span> |
| <span data-ttu-id="255ce-397">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="255ce-397">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="255ce-398">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="255ce-398">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="255ce-399">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="255ce-399">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="255ce-400">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="255ce-400">Log scopes</span></span>

 <span data-ttu-id="255ce-401">「範圍」\*\* 可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="255ce-401">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="255ce-402">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-402">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="255ce-403">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="255ce-403">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="255ce-404">範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="255ce-404">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="255ce-405">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="255ce-405">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="255ce-406">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="255ce-406">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="255ce-407">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="255ce-407">*Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=6)]

> [!NOTE]
> <span data-ttu-id="255ce-408">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-408">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="255ce-409">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="255ce-409">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="255ce-410">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="255ce-410">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="255ce-411">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-411">Built-in logging providers</span></span>

<span data-ttu-id="255ce-412">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="255ce-412">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="255ce-413">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-413">Console</span></span>](#console-provider)
* [<span data-ttu-id="255ce-414">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-414">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="255ce-415">事件來源</span><span class="sxs-lookup"><span data-stu-id="255ce-415">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="255ce-416">EventLog</span><span class="sxs-lookup"><span data-stu-id="255ce-416">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="255ce-417">TraceSource</span><span class="sxs-lookup"><span data-stu-id="255ce-417">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="255ce-418">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="255ce-418">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="255ce-419">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="255ce-419">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="255ce-420">應用洞察</span><span class="sxs-lookup"><span data-stu-id="255ce-420">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="255ce-421">如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="255ce-421">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="255ce-422">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-422">Console provider</span></span>

<span data-ttu-id="255ce-423">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="255ce-423">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="255ce-424">若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="255ce-424">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="255ce-425">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-425">Debug provider</span></span>

<span data-ttu-id="255ce-426">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="255ce-426">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="255ce-427">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="255ce-427">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="255ce-428">事件來源提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-428">Event Source provider</span></span>

<span data-ttu-id="255ce-429">[Microsoft.擴展.日誌記錄.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)提供程式包寫入具有`Microsoft-Extensions-Logging`名稱 的事件源跨平臺。</span><span class="sxs-lookup"><span data-stu-id="255ce-429">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="255ce-430">在 Windows 上,提供者使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="255ce-430">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="255ce-431">調用事件源提供程式以生成主機時`CreateDefaultBuilder`自動添加。</span><span class="sxs-lookup"><span data-stu-id="255ce-431">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="255ce-432">點網追蹤工具</span><span class="sxs-lookup"><span data-stu-id="255ce-432">dotnet trace tooling</span></span>

<span data-ttu-id="255ce-433">[點網追蹤](/dotnet/core/diagnostics/dotnet-trace)工具是一個跨平臺 CLI 全域工具,用於收集正在運行的進程的 .NET Core 跟蹤。</span><span class="sxs-lookup"><span data-stu-id="255ce-433">The [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="255ce-434">該工具使用收集<xref:Microsoft.Extensions.Logging.EventSource>提供程式<xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>資料 。</span><span class="sxs-lookup"><span data-stu-id="255ce-434">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="255ce-435">使用以下指令安裝 dotnet 追蹤工具:</span><span class="sxs-lookup"><span data-stu-id="255ce-435">Install the dotnet trace tooling with the following command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-trace
```

<span data-ttu-id="255ce-436">使用點網追蹤工具從應用程式收集追蹤:</span><span class="sxs-lookup"><span data-stu-id="255ce-436">Use the dotnet trace tooling to collect a trace from an app:</span></span>

1. <span data-ttu-id="255ce-437">如果應用未使用`CreateDefaultBuilder`生成主機,則將[事件源提供程式](#event-source-provider)添加到應用的日誌記錄配置中。</span><span class="sxs-lookup"><span data-stu-id="255ce-437">If the app doesn't build the host with `CreateDefaultBuilder`, add the [Event Source provider](#event-source-provider) to the app's logging configuration.</span></span>

1. <span data-ttu-id="255ce-438">使用`dotnet run`命令運行應用。</span><span class="sxs-lookup"><span data-stu-id="255ce-438">Run the app with the `dotnet run` command.</span></span>

1. <span data-ttu-id="255ce-439">確定 .NET Core 應用程式識別碼 (PID):</span><span class="sxs-lookup"><span data-stu-id="255ce-439">Determine the process identifier (PID) of the .NET Core app:</span></span>

   * <span data-ttu-id="255ce-440">在 Windows 上,使用以下方法之一:</span><span class="sxs-lookup"><span data-stu-id="255ce-440">On Windows, use one of the following approaches:</span></span>
     * <span data-ttu-id="255ce-441">工作管理員 (Ctrl_Alt_Del)</span><span class="sxs-lookup"><span data-stu-id="255ce-441">Task Manager (Ctrl+Alt+Del)</span></span>
     * [<span data-ttu-id="255ce-442">工作清單命令</span><span class="sxs-lookup"><span data-stu-id="255ce-442">tasklist command</span></span>](/windows-server/administration/windows-commands/tasklist)
     * [<span data-ttu-id="255ce-443">取得行程電源外殼命令</span><span class="sxs-lookup"><span data-stu-id="255ce-443">Get-Process Powershell command</span></span>](/powershell/module/microsoft.powershell.management/get-process)
   * <span data-ttu-id="255ce-444">在 Linux 上,使用[pidof 指令](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html)。</span><span class="sxs-lookup"><span data-stu-id="255ce-444">On Linux, use the [pidof command](https://refspecs.linuxfoundation.org/LSB_5.0.0/LSB-Core-generic/LSB-Core-generic/pidof.html).</span></span>

   <span data-ttu-id="255ce-445">尋找與應用程式集同名的程序的 PID。</span><span class="sxs-lookup"><span data-stu-id="255ce-445">Find the PID for the process that has the same name as the app's assembly.</span></span>

1. <span data-ttu-id="255ce-446">執行指令`dotnet trace`。</span><span class="sxs-lookup"><span data-stu-id="255ce-446">Execute the `dotnet trace` command.</span></span>

   <span data-ttu-id="255ce-447">一般命令語法:</span><span class="sxs-lookup"><span data-stu-id="255ce-447">General command syntax:</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"
   ```

   <span data-ttu-id="255ce-448">使用 PowerShell 命令外殼時,`--providers`以單 引號括`'`起來的值 ( :</span><span class="sxs-lookup"><span data-stu-id="255ce-448">When using a PowerShell command shell, enclose the `--providers` value in single quotes (`'`):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} 
       --providers 'Microsoft-Extensions-Logging:{Keyword}:{Event Level}
           :FilterSpecs=\"
               {Logger Category 1}:{Event Level 1};
               {Logger Category 2}:{Event Level 2};
               ...
               {Logger Category N}:{Event Level N}\"'
   ```

   <span data-ttu-id="255ce-449">在非 Windows 平臺`-f speedscope`上, 添加將輸出追蹤檔案的`speedscope`格式更改為的選項。</span><span class="sxs-lookup"><span data-stu-id="255ce-449">On non-Windows platforms, add the `-f speedscope` option to change the format of the output trace file to `speedscope`.</span></span>

   | <span data-ttu-id="255ce-450">關鍵字</span><span class="sxs-lookup"><span data-stu-id="255ce-450">Keyword</span></span> | <span data-ttu-id="255ce-451">描述</span><span class="sxs-lookup"><span data-stu-id="255ce-451">Description</span></span> |
   | :-----: | ----------- |
   | <span data-ttu-id="255ce-452">1</span><span class="sxs-lookup"><span data-stu-id="255ce-452">1</span></span>       | <span data-ttu-id="255ce-453">記錄有關的`LoggingEventSource`元事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-453">Log meta events about the `LoggingEventSource`.</span></span> <span data-ttu-id="255ce-454">不記錄事件`ILogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-454">Doesn't log events from `ILogger`).</span></span> |
   | <span data-ttu-id="255ce-455">2</span><span class="sxs-lookup"><span data-stu-id="255ce-455">2</span></span>       | <span data-ttu-id="255ce-456">調用時`Message``ILogger.Log()`打開事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-456">Turns on the `Message` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="255ce-457">以程式設計(未格式化)方式提供資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-457">Provides information in a programmatic (not formatted) way.</span></span> |
   | <span data-ttu-id="255ce-458">4</span><span class="sxs-lookup"><span data-stu-id="255ce-458">4</span></span>       | <span data-ttu-id="255ce-459">調用時`FormatMessage``ILogger.Log()`打開事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-459">Turns on the `FormatMessage` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="255ce-460">提供資訊的格式化字串版本。</span><span class="sxs-lookup"><span data-stu-id="255ce-460">Provides the formatted string version of the information.</span></span> |
   | <span data-ttu-id="255ce-461">8</span><span class="sxs-lookup"><span data-stu-id="255ce-461">8</span></span>       | <span data-ttu-id="255ce-462">調用時`MessageJson``ILogger.Log()`打開事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-462">Turns on the `MessageJson` event when `ILogger.Log()` is called.</span></span> <span data-ttu-id="255ce-463">提供參數的 JSON 表示形式。</span><span class="sxs-lookup"><span data-stu-id="255ce-463">Provides a JSON representation of the arguments.</span></span> |

   | <span data-ttu-id="255ce-464">事件層級</span><span class="sxs-lookup"><span data-stu-id="255ce-464">Event Level</span></span> | <span data-ttu-id="255ce-465">描述</span><span class="sxs-lookup"><span data-stu-id="255ce-465">Description</span></span>     |
   | :---------: | --------------- |
   | <span data-ttu-id="255ce-466">0</span><span class="sxs-lookup"><span data-stu-id="255ce-466">0</span></span>           | `LogAlways`     |
   | <span data-ttu-id="255ce-467">1</span><span class="sxs-lookup"><span data-stu-id="255ce-467">1</span></span>           | `Critical`      |
   | <span data-ttu-id="255ce-468">2</span><span class="sxs-lookup"><span data-stu-id="255ce-468">2</span></span>           | `Error`         |
   | <span data-ttu-id="255ce-469">3</span><span class="sxs-lookup"><span data-stu-id="255ce-469">3</span></span>           | `Warning`       |
   | <span data-ttu-id="255ce-470">4</span><span class="sxs-lookup"><span data-stu-id="255ce-470">4</span></span>           | `Informational` |
   | <span data-ttu-id="255ce-471">5</span><span class="sxs-lookup"><span data-stu-id="255ce-471">5</span></span>           | `Verbose`       |

   <span data-ttu-id="255ce-472">`FilterSpecs`條目`{Logger Category}`,`{Event Level}`並表示其他日誌篩選條件。</span><span class="sxs-lookup"><span data-stu-id="255ce-472">`FilterSpecs` entries for `{Logger Category}` and `{Event Level}` represent additional log filtering conditions.</span></span> <span data-ttu-id="255ce-473">分`FilterSpecs`號 (`;`的單獨條目)。</span><span class="sxs-lookup"><span data-stu-id="255ce-473">Separate `FilterSpecs` entries with a semicolon (`;`).</span></span>

   <span data-ttu-id="255ce-474">使用 Windows 命令外殼的範`--providers`例(**value 周圍沒有**單個引號):</span><span class="sxs-lookup"><span data-stu-id="255ce-474">Example using a Windows command shell (**no** single quotes around the `--providers` value):</span></span>

   ```dotnetcli
   dotnet trace collect -p {PID} --providers Microsoft-Extensions-Logging:4:2:FilterSpecs=\"Microsoft.AspNetCore.Hosting*:4\"
   ```

   <span data-ttu-id="255ce-475">前面的指令啟動:</span><span class="sxs-lookup"><span data-stu-id="255ce-475">The preceding command activates:</span></span>

   * <span data-ttu-id="255ce-476">事件來源記錄器產生錯誤的格式化字串 (`4`(`2`)</span><span class="sxs-lookup"><span data-stu-id="255ce-476">The Event Source logger to produce formatted strings (`4`) for errors (`2`).</span></span>
   * <span data-ttu-id="255ce-477">`Microsoft.AspNetCore.Hosting``Informational`紀錄紀錄等級(`4`。</span><span class="sxs-lookup"><span data-stu-id="255ce-477">`Microsoft.AspNetCore.Hosting` logging at the `Informational` logging level (`4`).</span></span>

1. <span data-ttu-id="255ce-478">通過按 Enter 鍵或 Ctrl_C 停止點網跟蹤工具。</span><span class="sxs-lookup"><span data-stu-id="255ce-478">Stop the dotnet trace tooling by pressing the Enter key or Ctrl+C.</span></span>

   <span data-ttu-id="255ce-479">追蹤將隨執行`dotnet trace`指令的資料夾中的名稱*trace*一起儲存。</span><span class="sxs-lookup"><span data-stu-id="255ce-479">The trace is saved with the name *trace.nettrace* in the folder where the `dotnet trace` command is executed.</span></span>

1. <span data-ttu-id="255ce-480">用[Perfview](#perfview)打開追蹤。</span><span class="sxs-lookup"><span data-stu-id="255ce-480">Open the trace with [Perfview](#perfview).</span></span> <span data-ttu-id="255ce-481">打開*追蹤.net 追蹤*檔案並瀏覽追蹤事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-481">Open the *trace.nettrace* file and explore the trace events.</span></span>

<span data-ttu-id="255ce-482">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="255ce-482">For more information, see:</span></span>

* <span data-ttu-id="255ce-483">[追蹤效能分析實用程式 (點網追蹤)( .NET](/dotnet/core/diagnostics/dotnet-trace)核心文檔)</span><span class="sxs-lookup"><span data-stu-id="255ce-483">[Trace for performance analysis utility (dotnet-trace)](/dotnet/core/diagnostics/dotnet-trace) (.NET Core documentation)</span></span>
* <span data-ttu-id="255ce-484">[追蹤效能分析實用程式 (點網追蹤) (](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md)點網 / 診斷 GitHub 儲存函式庫文件 )</span><span class="sxs-lookup"><span data-stu-id="255ce-484">[Trace for performance analysis utility (dotnet-trace)](https://github.com/dotnet/diagnostics/blob/master/documentation/dotnet-trace-instructions.md) (dotnet/diagnostics GitHub repository documentation)</span></span>
* <span data-ttu-id="255ce-485">[紀錄記錄事件來源類別](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource)(.NET API 瀏覽器)</span><span class="sxs-lookup"><span data-stu-id="255ce-485">[LoggingEventSource Class](xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource) (.NET API Browser)</span></span>
* <xref:System.Diagnostics.Tracing.EventLevel>
* <span data-ttu-id="255ce-486">[記錄紀錄事件來源來源 (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs)&ndash;要取得不同版本的引用來源,將分支更改為`release/{Version}`,ASP.NET `{Version}` Core 所需的版本所在的位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-486">[LoggingEventSource reference source (3.0)](https://github.com/dotnet/extensions/blob/release/3.0/src/Logging/Logging.EventSource/src/LoggingEventSource.cs) &ndash; To obtain reference source for a different version, change the branch to `release/{Version}`, where `{Version}` is the version of ASP.NET Core desired.</span></span>
* <span data-ttu-id="255ce-487">[Perfview](#perfview)&ndash;可用於查看事件源追蹤。</span><span class="sxs-lookup"><span data-stu-id="255ce-487">[Perfview](#perfview) &ndash; Useful for viewing Event Source traces.</span></span>

#### <a name="perfview"></a><span data-ttu-id="255ce-488">佩爾夫維</span><span class="sxs-lookup"><span data-stu-id="255ce-488">Perfview</span></span>

<span data-ttu-id="255ce-489">使用[PerfView 實用程式](https://github.com/Microsoft/perfview)收集和查看日誌。</span><span class="sxs-lookup"><span data-stu-id="255ce-489">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="255ce-490">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="255ce-490">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="255ce-491">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]\*\*\*\* 清單</span><span class="sxs-lookup"><span data-stu-id="255ce-491">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="255ce-492">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="255ce-492">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="255ce-494">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-494">Windows EventLog provider</span></span>

<span data-ttu-id="255ce-495">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="255ce-495">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="255ce-496">[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。</span><span class="sxs-lookup"><span data-stu-id="255ce-496">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="255ce-497">如果`null`或未指定,則使用以下預設設定:</span><span class="sxs-lookup"><span data-stu-id="255ce-497">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="255ce-498">`LogName`&ndash; "申請"</span><span class="sxs-lookup"><span data-stu-id="255ce-498">`LogName` &ndash; "Application"</span></span>
* <span data-ttu-id="255ce-499">`SourceName`&ndash; ".NET 運行時"</span><span class="sxs-lookup"><span data-stu-id="255ce-499">`SourceName` &ndash; ".NET Runtime"</span></span>
* <span data-ttu-id="255ce-500">`MachineName` &ndash; 本機電腦</span><span class="sxs-lookup"><span data-stu-id="255ce-500">`MachineName` &ndash; local machine</span></span>

<span data-ttu-id="255ce-501">事件記錄為[警告等級和更高](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="255ce-501">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="255ce-502">要記錄低於`Warning`的事件,請顯式設置日誌級別。</span><span class="sxs-lookup"><span data-stu-id="255ce-502">To log events lower than `Warning`, explicitly set the log level.</span></span> <span data-ttu-id="255ce-503">例如,將以下內容添加到*appsettings.json*檔:</span><span class="sxs-lookup"><span data-stu-id="255ce-503">For example, add the following to the *appsettings.json* file:</span></span>

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="255ce-504">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-504">TraceSource provider</span></span>

<span data-ttu-id="255ce-505">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-505">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="255ce-506">[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="255ce-506">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="255ce-507">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="255ce-507">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="255ce-508">該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。</span><span class="sxs-lookup"><span data-stu-id="255ce-508">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="255ce-509">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-509">Azure App Service provider</span></span>

<span data-ttu-id="255ce-510">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="255ce-510">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="255ce-511">該提供者套件並未包含在共用架構中。</span><span class="sxs-lookup"><span data-stu-id="255ce-511">The provider package isn't included in the shared framework.</span></span> <span data-ttu-id="255ce-512">若要使用提供者，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="255ce-512">To use the provider, add the provider package to the project.</span></span>

<span data-ttu-id="255ce-513">如果要進行提供者設定，請使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下範例中所示：</span><span class="sxs-lookup"><span data-stu-id="255ce-513">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/3.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=17-28)]

<span data-ttu-id="255ce-514">當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]\*\*\*\* 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。</span><span class="sxs-lookup"><span data-stu-id="255ce-514">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="255ce-515">當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="255ce-515">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="255ce-516">**應用程式記錄 (檔案系統)**</span><span class="sxs-lookup"><span data-stu-id="255ce-516">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="255ce-517">**應用程式記錄 (Blob)**</span><span class="sxs-lookup"><span data-stu-id="255ce-517">**Application Logging (Blob)**</span></span>

<span data-ttu-id="255ce-518">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="255ce-518">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="255ce-519">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="255ce-519">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="255ce-520">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="255ce-520">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="255ce-521">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="255ce-521">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="255ce-522">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="255ce-522">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="255ce-523">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="255ce-523">Azure log streaming</span></span>

<span data-ttu-id="255ce-524">Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="255ce-524">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="255ce-525">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="255ce-525">The app server</span></span>
* <span data-ttu-id="255ce-526">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="255ce-526">The web server</span></span>
* <span data-ttu-id="255ce-527">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="255ce-527">Failed request tracing</span></span>

<span data-ttu-id="255ce-528">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="255ce-528">To configure Azure log streaming:</span></span>

* <span data-ttu-id="255ce-529">從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-529">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="255ce-530">將 [應用程式記錄 (檔案系統)]\*\*\*\* 設定為 [開啟]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-530">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="255ce-531">選擇記錄 [層級]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-531">Choose the log **Level**.</span></span> <span data-ttu-id="255ce-532">此設置僅適用於 Azure 日誌流,不適用於應用中的其他日誌記錄提供程式。</span><span class="sxs-lookup"><span data-stu-id="255ce-532">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="255ce-533">瀏覽到 [記錄資料流]\*\*\*\* 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="255ce-533">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="255ce-534">這些是應用程式透過 `ILogger` 介面產生的訊息。</span><span class="sxs-lookup"><span data-stu-id="255ce-534">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="255ce-535">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-535">Azure Application Insights trace logging</span></span>

<span data-ttu-id="255ce-536">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。</span><span class="sxs-lookup"><span data-stu-id="255ce-536">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="255ce-537">Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。</span><span class="sxs-lookup"><span data-stu-id="255ce-537">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="255ce-538">如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-538">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="255ce-539">記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。</span><span class="sxs-lookup"><span data-stu-id="255ce-539">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="255ce-540">如果您使用此套件，就不需安裝提供者套件。</span><span class="sxs-lookup"><span data-stu-id="255ce-540">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="255ce-541">不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="255ce-541">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="255ce-542">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="255ce-542">For more information, see the following resources:</span></span>

* [<span data-ttu-id="255ce-543">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="255ce-543">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="255ce-544">[適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="255ce-544">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="255ce-545">[適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="255ce-545">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="255ce-546">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="255ce-546">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="255ce-547">[安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。</span><span class="sxs-lookup"><span data-stu-id="255ce-547">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="255ce-548">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-548">Third-party logging providers</span></span>

<span data-ttu-id="255ce-549">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="255ce-549">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="255ce-550">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-550">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="255ce-551">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-551">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="255ce-552">[JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="255ce-552">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="255ce-553">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="255ce-553">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="255ce-554">[紀錄4Net](https://logging.apache.org/log4net/) ([GitHub 儲存函式庫](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="255ce-554">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="255ce-555">[Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-555">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="255ce-556">[NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-556">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="255ce-557">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="255ce-557">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="255ce-558">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="255ce-558">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="255ce-559">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="255ce-559">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="255ce-560">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="255ce-560">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="255ce-561">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="255ce-561">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="255ce-562">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="255ce-562">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="255ce-563">調用日誌記錄`ILoggerFactory`框架提供的擴展方法。</span><span class="sxs-lookup"><span data-stu-id="255ce-563">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="255ce-564">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="255ce-564">For more information, see each provider's documentation.</span></span> <span data-ttu-id="255ce-565">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-565">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="255ce-566">其他資源</span><span class="sxs-lookup"><span data-stu-id="255ce-566">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="255ce-567">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="255ce-567">By [Tom Dykstra](https://github.com/tdykstra) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="255ce-568">.NET Core 支援記錄 API，此 API 能與各種內建和第三方記錄提供者搭配使用。</span><span class="sxs-lookup"><span data-stu-id="255ce-568">.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="255ce-569">此文章說明如何搭配內建提供者使用 API。</span><span class="sxs-lookup"><span data-stu-id="255ce-569">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="255ce-570">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="255ce-570">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="255ce-571">新增提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-571">Add providers</span></span>

<span data-ttu-id="255ce-572">記錄提供者會顯示或儲存記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-572">A logging provider displays or stores logs.</span></span> <span data-ttu-id="255ce-573">例如，主控台提供者會在主控台上顯示記錄，而 Azure Application Insights 提供者則會將記錄儲存在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="255ce-573">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="255ce-574">您可以透過新增多個提供者的方式來將記錄傳送到多個目的地。</span><span class="sxs-lookup"><span data-stu-id="255ce-574">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

<span data-ttu-id="255ce-575">若要新增提供者，請在 *Program.cs* 中呼叫提供者的 `Add{provider name}` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="255ce-575">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=18-20)]

<span data-ttu-id="255ce-576">上述程式碼需要對 `Microsoft.Extensions.Logging` 與 `Microsoft.Extensions.Configuration` 的參考。</span><span class="sxs-lookup"><span data-stu-id="255ce-576">The preceding code requires references to `Microsoft.Extensions.Logging` and `Microsoft.Extensions.Configuration`.</span></span>

<span data-ttu-id="255ce-577">預設專案範本會呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>，該項目會新增下列記錄提供者：</span><span class="sxs-lookup"><span data-stu-id="255ce-577">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="255ce-578">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-578">Console</span></span>
* <span data-ttu-id="255ce-579">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-579">Debug</span></span>
* <span data-ttu-id="255ce-580">EventSource (從 ASP.NET Core 2.2 開始)</span><span class="sxs-lookup"><span data-stu-id="255ce-580">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="255ce-581">若您使用 `CreateDefaultBuilder`，您可以使用您想要的提供者來取代預設提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-581">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="255ce-582">呼叫 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 並新增您要的提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-582">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

<span data-ttu-id="255ce-583">在此文章中深入了解[內建記錄提供者](#built-in-logging-providers)與[第三方記錄提供者](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="255ce-583">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="255ce-584">建立記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-584">Create logs</span></span>

<span data-ttu-id="255ce-585">若要建立記錄，請使用 <xref:Microsoft.Extensions.Logging.ILogger%601> 物件。</span><span class="sxs-lookup"><span data-stu-id="255ce-585">To create logs, use an <xref:Microsoft.Extensions.Logging.ILogger%601> object.</span></span> <span data-ttu-id="255ce-586">在 Web 應用程式或託管服務中，從相依性插入 (DI) 取得 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-586">In a web app or hosted service, get an `ILogger` from dependency injection (DI).</span></span> <span data-ttu-id="255ce-587">在非主機主控台應用程式中，使用 `LoggerFactory` 來建立 `ILogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-587">In non-host console apps, use the `LoggerFactory` to create an `ILogger`.</span></span>

<span data-ttu-id="255ce-588">下列 ASP.NET Core 範例會建立以 `TodoApiSample.Pages.AboutModel` 作為類別的記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-588">The following ASP.NET Core example creates a logger with `TodoApiSample.Pages.AboutModel` as the category.</span></span> <span data-ttu-id="255ce-589">記錄「類別」\*\* 是與每個記錄關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="255ce-589">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="255ce-590">由 DI 提供的 `ILogger<T>` 執行個體，會建立使用型別 `T` 作為類別的完整名稱記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-590">The `ILogger<T>` instance provided by DI creates logs that have the fully qualified name of type `T` as the category.</span></span> 

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3,5,7)]

<span data-ttu-id="255ce-591">在下列 ASP.NET Core 和主控台應用程式範例中，記錄器會用於以 `Information` 作為層級來建立記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-591">In the following ASP.NET Core and console app examples, the logger is used to create logs with `Information` as the level.</span></span> <span data-ttu-id="255ce-592">記錄「層級」\*\* 指出已記錄事件的嚴重性。</span><span class="sxs-lookup"><span data-stu-id="255ce-592">The Log *level* indicates the severity of the logged event.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

<span data-ttu-id="255ce-593">此文章稍後將詳細說明[層級](#log-level)與[類別](#log-category)。</span><span class="sxs-lookup"><span data-stu-id="255ce-593">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span>

### <a name="create-logs-in-startup"></a><span data-ttu-id="255ce-594">在啟動中建立記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-594">Create logs in Startup</span></span>

<span data-ttu-id="255ce-595">若要在 `Startup` 類別中寫入記錄，請在建構函式簽章中包括 `ILogger` 參數：</span><span class="sxs-lookup"><span data-stu-id="255ce-595">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-the-program-class"></a><span data-ttu-id="255ce-596">在 Program 類別中建立記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-596">Create logs in the Program class</span></span>

<span data-ttu-id="255ce-597">若要在 `Program` 類別中寫入記錄，請從 DI 取得 `ILogger` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="255ce-597">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

<span data-ttu-id="255ce-598">不支援在主機構造期間進行日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-598">Logging during host construction isn't directly supported.</span></span> <span data-ttu-id="255ce-599">但是,可以使用單獨的記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-599">However, a separate logger can be used.</span></span> <span data-ttu-id="255ce-600">在下列範例中，會使用 [Serilog](https://serilog.net/) 記錄器來記錄 `CreateWebHostBuilder`。 </span><span class="sxs-lookup"><span data-stu-id="255ce-600">In the following example, a [Serilog](https://serilog.net/) logger is used to log in `CreateWebHostBuilder`.</span></span> <span data-ttu-id="255ce-601">`AddSerilog`使用 在`Log.Logger`中指定的靜態配置。</span><span class="sxs-lookup"><span data-stu-id="255ce-601">`AddSerilog` uses the static configuration specified in `Log.Logger`:</span></span>

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

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="255ce-602">無非同步記錄器方法</span><span class="sxs-lookup"><span data-stu-id="255ce-602">No asynchronous logger methods</span></span>

<span data-ttu-id="255ce-603">記錄速度應該很快，不值得花費非同步程式碼的效能成本來處理。</span><span class="sxs-lookup"><span data-stu-id="255ce-603">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="255ce-604">若您的記錄資料存放區很慢，請不要直接寫入其中。</span><span class="sxs-lookup"><span data-stu-id="255ce-604">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="255ce-605">請考慮一開始將記錄寫入到快速的存放區，稍後再將它們移到慢速存放區。</span><span class="sxs-lookup"><span data-stu-id="255ce-605">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="255ce-606">例如，如果您要記錄到 SQL Server，您不希望在 `Log` 方法中直接執行，因為 `Log` 方法是同步的。</span><span class="sxs-lookup"><span data-stu-id="255ce-606">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="255ce-607">相反地，以同步方式將記錄訊息新增到記憶體內佇列，並讓背景工作角色提取出佇列的訊息，藉此執行推送資料到 SQL Server 的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="255ce-607">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span> <span data-ttu-id="255ce-608">有關詳細資訊,請參閱[此](https://github.com/dotnet/AspNetCore.Docs/issues/11801)GitHub 問題。</span><span class="sxs-lookup"><span data-stu-id="255ce-608">For more information, see [this](https://github.com/dotnet/AspNetCore.Docs/issues/11801) GitHub issue.</span></span>

## <a name="configuration"></a><span data-ttu-id="255ce-609">組態</span><span class="sxs-lookup"><span data-stu-id="255ce-609">Configuration</span></span>

<span data-ttu-id="255ce-610">記錄提供者設定是由一或多個記錄提供者提供：</span><span class="sxs-lookup"><span data-stu-id="255ce-610">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="255ce-611">檔案格式 (INI、JSON 及 XML)。</span><span class="sxs-lookup"><span data-stu-id="255ce-611">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="255ce-612">命令列引數。</span><span class="sxs-lookup"><span data-stu-id="255ce-612">Command-line arguments.</span></span>
* <span data-ttu-id="255ce-613">環境變數。</span><span class="sxs-lookup"><span data-stu-id="255ce-613">Environment variables.</span></span>
* <span data-ttu-id="255ce-614">記憶體內部 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="255ce-614">In-memory .NET objects.</span></span>
* <span data-ttu-id="255ce-615">未加密的[祕密管理員](xref:security/app-secrets)儲存體。</span><span class="sxs-lookup"><span data-stu-id="255ce-615">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="255ce-616">類似 [Azure Key Vault](xref:security/key-vault-configuration)的加密使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="255ce-616">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="255ce-617">自訂提供者 (已安裝或已建立)。</span><span class="sxs-lookup"><span data-stu-id="255ce-617">Custom providers (installed or created).</span></span>

<span data-ttu-id="255ce-618">例如，記錄設定通常是由應用程式的 `Logging` 區段所提供的。</span><span class="sxs-lookup"><span data-stu-id="255ce-618">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="255ce-619">下列範例顯示一般 *appsettings.Development.json* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="255ce-619">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="255ce-620">`Logging` 屬性可以有 `LogLevel` 與記錄提供者屬性 (會顯示主控台)。</span><span class="sxs-lookup"><span data-stu-id="255ce-620">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="255ce-621">`Logging` 下的 `LogLevel` 屬性會指定要針對所選類記錄的最小[層級](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="255ce-621">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="255ce-622">在範例中，`System` 與d `Microsoft` 類別會在 `Information` 層級記錄，而所有其他記錄則會在 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-622">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="255ce-623">`Logging` 下的其他屬性可指定記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-623">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="255ce-624">範例使用主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-624">The example is for the Console provider.</span></span> <span data-ttu-id="255ce-625">如果提供程式支援[日誌作用域](#log-scopes),`IncludeScopes`則指示它們是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="255ce-625">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="255ce-626">提供者屬性 (例如範例中的 `Console`) 可能也會指定 `LogLevel` 屬性。</span><span class="sxs-lookup"><span data-stu-id="255ce-626">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="255ce-627">提供者下的 `LogLevel` 會指定提供者的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-627">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="255ce-628">若已在 `Logging.{providername}.LogLevel` 中指定層級，它們會覆寫 `Logging.LogLevel` 中設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="255ce-628">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

<span data-ttu-id="255ce-629">日誌記錄 API 不包括在應用運行時更改日誌級別的方案。</span><span class="sxs-lookup"><span data-stu-id="255ce-629">The Logging API doesn't include a scenario to change log levels while an app is running.</span></span> <span data-ttu-id="255ce-630">但是,某些配置提供程式能夠重新載入配置,這將立即對日誌記錄配置產生影響。</span><span class="sxs-lookup"><span data-stu-id="255ce-630">However, some configuration providers are capable of reloading configuration, which takes immediate effect on logging configuration.</span></span> <span data-ttu-id="255ce-631">例如,[檔案設定提供者](xref:fundamentals/configuration/index#file-configuration-provider)`CreateDefaultBuilder`() 被添加到讀取設定檔,預設情況下重新載入紀錄記錄配置。</span><span class="sxs-lookup"><span data-stu-id="255ce-631">For example, the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider), which is added by `CreateDefaultBuilder` to read settings files, reloads logging configuration by default.</span></span> <span data-ttu-id="255ce-632">如果在應用運行時的代碼中更改了配置,則應用可以調用[I配置Root.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*)以更新應用的日誌記錄配置。</span><span class="sxs-lookup"><span data-stu-id="255ce-632">If configuration is changed in code while an app is running, the app can call [IConfigurationRoot.Reload](xref:Microsoft.Extensions.Configuration.IConfigurationRoot.Reload*) to update the app's logging configuration.</span></span>

<span data-ttu-id="255ce-633">如需有關如何實作設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="255ce-633">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="255ce-634">範例記錄輸出</span><span class="sxs-lookup"><span data-stu-id="255ce-634">Sample logging output</span></span>

<span data-ttu-id="255ce-635">使用上一節中顯示的範例程式碼時，當從命令列執行應用程式時，記錄會出現在主控台中。</span><span class="sxs-lookup"><span data-stu-id="255ce-635">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="255ce-636">主控台輸出範例如下：</span><span class="sxs-lookup"><span data-stu-id="255ce-636">Here's an example of console output:</span></span>

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

<span data-ttu-id="255ce-637">上述記錄是透過向位於 `http://localhost:5000/api/todo/0` 的範例應用程式建立 HTTP Get 要求所產生。</span><span class="sxs-lookup"><span data-stu-id="255ce-637">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="255ce-638">以下是您在 Visual Studio 中執行相同應用程式時，出現在 [偵錯] 視窗中的相同記錄範例：</span><span class="sxs-lookup"><span data-stu-id="255ce-638">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="255ce-639">這些透過上一節中所示 `ILogger` 呼叫所建立的記錄是以 "TodoApi" 為開頭。</span><span class="sxs-lookup"><span data-stu-id="255ce-639">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi".</span></span> <span data-ttu-id="255ce-640">開頭為 "Microsoft" 類別的記錄則是來自 ASP.NET Core 架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="255ce-640">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="255ce-641">ASP.NET Core 與應用程式程式碼會使用相同的記錄 API 與提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-641">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="255ce-642">本文的其餘部分將說明記錄的一些詳細資料和選項。</span><span class="sxs-lookup"><span data-stu-id="255ce-642">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="255ce-643">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="255ce-643">NuGet packages</span></span>

<span data-ttu-id="255ce-644">`ILogger` 和 `ILoggerFactory` 介面位於 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 中，其預設實作則位於 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 中。</span><span class="sxs-lookup"><span data-stu-id="255ce-644">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="255ce-645">記錄分類</span><span class="sxs-lookup"><span data-stu-id="255ce-645">Log category</span></span>

<span data-ttu-id="255ce-646">建立 `ILogger` 物件時，會為它指定「類別」\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-646">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="255ce-647">該類別會包含在每個由該 `ILogger` 執行個體所產生的記錄訊息中。</span><span class="sxs-lookup"><span data-stu-id="255ce-647">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="255ce-648">類別可以是任意字串，但慣例是使用類別名稱，例如 "TodoApi.Controllers.TodoController"。</span><span class="sxs-lookup"><span data-stu-id="255ce-648">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="255ce-649">使用 `ILogger<T>` 來取得`ILogger` 執行個體，它使用 `T` 的完整類型名稱做為類別：</span><span class="sxs-lookup"><span data-stu-id="255ce-649">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="255ce-650">若要明確指定類別，請呼叫 `ILoggerFactory.CreateLogger`：</span><span class="sxs-lookup"><span data-stu-id="255ce-650">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="255ce-651">`ILogger<T>` 相當於使用 `T`的完整類型名稱來呼叫 `CreateLogger`。</span><span class="sxs-lookup"><span data-stu-id="255ce-651">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="255ce-652">記錄層級</span><span class="sxs-lookup"><span data-stu-id="255ce-652">Log level</span></span>

<span data-ttu-id="255ce-653">每個記錄都會指定 <xref:Microsoft.Extensions.Logging.LogLevel> 值。</span><span class="sxs-lookup"><span data-stu-id="255ce-653">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="255ce-654">記錄層級表示嚴重性或重要性。</span><span class="sxs-lookup"><span data-stu-id="255ce-654">The log level indicates the severity or importance.</span></span> <span data-ttu-id="255ce-655">例如，您可以在方法不正常終止時寫入 `Information` 記錄，並在方法傳回 *404 找不到* 狀態碼時寫入 `Warning` 記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-655">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="255ce-656">下列程式碼會建立 `Information` 與 `Warning` 記錄：</span><span class="sxs-lookup"><span data-stu-id="255ce-656">The following code creates `Information` and `Warning` logs:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="255ce-657">在上述程式碼中，第一個參數是[記錄事件識別碼](#log-event-id)。</span><span class="sxs-lookup"><span data-stu-id="255ce-657">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="255ce-658">第二個參數是訊息範本，其中的預留位置會置入其餘方法參數所提供的引數值。</span><span class="sxs-lookup"><span data-stu-id="255ce-658">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="255ce-659">此文章稍後的[訊息範本小節](#log-message-template)將詳細說明方法參數。</span><span class="sxs-lookup"><span data-stu-id="255ce-659">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="255ce-660">在方法名稱中包含層級的記錄方法 (例如 `LogInformation` 與 `LogWarning`) 是 [ILogger 的擴充方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)。</span><span class="sxs-lookup"><span data-stu-id="255ce-660">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="255ce-661">這些方法會呼叫接受 `Log` 參數的 `LogLevel`。</span><span class="sxs-lookup"><span data-stu-id="255ce-661">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="255ce-662">您可以直接呼叫 `Log` 方法，而不是呼叫其中一個擴充方法，但語法會更複雜。</span><span class="sxs-lookup"><span data-stu-id="255ce-662">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="255ce-663">如需詳細資訊，請參閱 <xref:Microsoft.Extensions.Logging.ILogger> 與[記錄器延伸模組原始程式碼](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)。</span><span class="sxs-lookup"><span data-stu-id="255ce-663">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/dotnet/extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="255ce-664">ASP.NET Core 定義下列記錄層級，並從最低嚴重性排列到最高嚴重性。</span><span class="sxs-lookup"><span data-stu-id="255ce-664">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="255ce-665">追蹤 = 0</span><span class="sxs-lookup"><span data-stu-id="255ce-665">Trace = 0</span></span>

  <span data-ttu-id="255ce-666">針對通常只對偵錯有價值的資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-666">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="255ce-667">這些訊息可能包含敏感性應用程式資料，因此不應該在生產環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="255ce-667">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="255ce-668">*預設為停用。*</span><span class="sxs-lookup"><span data-stu-id="255ce-668">*Disabled by default.*</span></span>

* <span data-ttu-id="255ce-669">偵錯 = 1</span><span class="sxs-lookup"><span data-stu-id="255ce-669">Debug = 1</span></span>

  <span data-ttu-id="255ce-670">針對可在開發與偵錯中使用的資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-670">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="255ce-671">範例：`Entering method Configure with flag set to true.` 由於記錄的數目很龐大，因此除非您正在進行疑難排解，否則通常不會在生產環境中啟用 `Debug` 層級記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-671">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="255ce-672">資訊 = 2</span><span class="sxs-lookup"><span data-stu-id="255ce-672">Information = 2</span></span>

  <span data-ttu-id="255ce-673">針對一般應用程式流程的追蹤。</span><span class="sxs-lookup"><span data-stu-id="255ce-673">For tracking the general flow of the app.</span></span> <span data-ttu-id="255ce-674">這些記錄通常有一些長期值。</span><span class="sxs-lookup"><span data-stu-id="255ce-674">These logs typically have some long-term value.</span></span> <span data-ttu-id="255ce-675">範例： `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="255ce-675">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="255ce-676">警告 = 3</span><span class="sxs-lookup"><span data-stu-id="255ce-676">Warning = 3</span></span>

  <span data-ttu-id="255ce-677">針對應用程式流程中發生的異常或意外事件。</span><span class="sxs-lookup"><span data-stu-id="255ce-677">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="255ce-678">這些記錄可能包含不會造成應用程式停止，但可能需要進行調查的錯誤或其他狀況。</span><span class="sxs-lookup"><span data-stu-id="255ce-678">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="255ce-679">已處理的例外狀況即為使用 `Warning` 記錄層級的常見位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-679">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="255ce-680">範例： `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="255ce-680">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="255ce-681">錯誤 = 4</span><span class="sxs-lookup"><span data-stu-id="255ce-681">Error = 4</span></span>

  <span data-ttu-id="255ce-682">發生無法處理的錯誤和例外狀況。</span><span class="sxs-lookup"><span data-stu-id="255ce-682">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="255ce-683">這些訊息指出目前活動或作業 (例如目前的 HTTP 要求) 中發生失敗，這不是整個應用程式的失敗。</span><span class="sxs-lookup"><span data-stu-id="255ce-683">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="255ce-684">範例記錄訊息：`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="255ce-684">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="255ce-685">重大 = 5</span><span class="sxs-lookup"><span data-stu-id="255ce-685">Critical = 5</span></span>

  <span data-ttu-id="255ce-686">發生需要立即注意的失敗。</span><span class="sxs-lookup"><span data-stu-id="255ce-686">For failures that require immediate attention.</span></span> <span data-ttu-id="255ce-687">範例：資料遺失情況、磁碟空間不足。</span><span class="sxs-lookup"><span data-stu-id="255ce-687">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="255ce-688">使用此記錄層級來控制要寫入至特定儲存媒體或顯示視窗的記錄輸出量。</span><span class="sxs-lookup"><span data-stu-id="255ce-688">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="255ce-689">例如：</span><span class="sxs-lookup"><span data-stu-id="255ce-689">For example:</span></span>

* <span data-ttu-id="255ce-690">在生產中:</span><span class="sxs-lookup"><span data-stu-id="255ce-690">In production:</span></span>
  * <span data-ttu-id="255ce-691">在`Trace``Information`通過級別日誌記錄會產生大量詳細的日誌消息。</span><span class="sxs-lookup"><span data-stu-id="255ce-691">Logging at the `Trace` through `Information` levels produces a high-volume of detailed log messages.</span></span> <span data-ttu-id="255ce-692">為了控制成本且不超過數據存儲限制,請將`Trace``Information`級別消息記錄到高容量、低成本數據存儲。</span><span class="sxs-lookup"><span data-stu-id="255ce-692">To control costs and not exceed data storage limits, log `Trace` through `Information` level messages to a high-volume, low-cost data store.</span></span>
  * <span data-ttu-id="255ce-693">在通過`Warning``Critical`級別進行日誌記錄通常會產生更少、更小的日誌消息。</span><span class="sxs-lookup"><span data-stu-id="255ce-693">Logging at `Warning` through `Critical` levels typically produces fewer, smaller log messages.</span></span> <span data-ttu-id="255ce-694">因此,成本和存儲限制通常不是問題,因此在選擇數據存儲時具有更大的靈活性。</span><span class="sxs-lookup"><span data-stu-id="255ce-694">Therefore, costs and storage limits usually aren't a concern, which results in greater flexibility of data store choice.</span></span>
* <span data-ttu-id="255ce-695">在開發過程中:</span><span class="sxs-lookup"><span data-stu-id="255ce-695">During development:</span></span>
  * <span data-ttu-id="255ce-696">將`Warning``Critical`消息記錄到主控台。</span><span class="sxs-lookup"><span data-stu-id="255ce-696">Log `Warning` through `Critical` messages to the console.</span></span>
  * <span data-ttu-id="255ce-697">故障排除`Trace``Information`時 添加消息。</span><span class="sxs-lookup"><span data-stu-id="255ce-697">Add `Trace` through `Information` messages when troubleshooting.</span></span>

<span data-ttu-id="255ce-698">本文稍後的[記錄篩選](#log-filtering)一節將說明如何控制提供者所處理的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-698">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="255ce-699">ASP.NET Core 會寫入架構事件的記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-699">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="255ce-700">此文章稍早的記錄範例已排除 `Information` 層級以下的記錄，因此未建立 `Debug` 或 `Trace` 層級的任何記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-700">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="255ce-701">以下是透過執行設定為顯示 `Debug` 記錄之範例應用程式所產生的主控台記錄範例：</span><span class="sxs-lookup"><span data-stu-id="255ce-701">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="255ce-702">記錄事件識別碼</span><span class="sxs-lookup"><span data-stu-id="255ce-702">Log event ID</span></span>

<span data-ttu-id="255ce-703">每個記錄都可以指定「事件識別碼」\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-703">Each log can specify an *event ID*.</span></span> <span data-ttu-id="255ce-704">範例應用程式透過使用本機定義的 `LoggingEvents` 類別來執行此動作：</span><span class="sxs-lookup"><span data-stu-id="255ce-704">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="255ce-705">事件識別碼會將一組事件關聯。</span><span class="sxs-lookup"><span data-stu-id="255ce-705">An event ID associates a set of events.</span></span> <span data-ttu-id="255ce-706">例如，與在頁面上顯示項目清單相關的所有記錄可能都是 1001。</span><span class="sxs-lookup"><span data-stu-id="255ce-706">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="255ce-707">記錄提供者可能將事件識別碼存放到識別碼欄位、記錄訊息中或完全不存放。</span><span class="sxs-lookup"><span data-stu-id="255ce-707">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="255ce-708">偵錯提供者不會顯示事件識別碼。</span><span class="sxs-lookup"><span data-stu-id="255ce-708">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="255ce-709">主控台提供者會在類別後面以括弧顯示事件識別碼：</span><span class="sxs-lookup"><span data-stu-id="255ce-709">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="255ce-710">記錄訊息範本</span><span class="sxs-lookup"><span data-stu-id="255ce-710">Log message template</span></span>

<span data-ttu-id="255ce-711">每個記錄都會指定訊息範本。</span><span class="sxs-lookup"><span data-stu-id="255ce-711">Each log specifies a message template.</span></span> <span data-ttu-id="255ce-712">訊息範本可以包含有關提供哪個引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-712">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="255ce-713">使用名稱而非數字做為預留位置。</span><span class="sxs-lookup"><span data-stu-id="255ce-713">Use names for the placeholders, not numbers.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="255ce-714">預留位置的順序 (而不是其名稱) 會決定使用哪些參數來提供其值。</span><span class="sxs-lookup"><span data-stu-id="255ce-714">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="255ce-715">請注意，在下列程式碼中，參數名稱在訊息範本中未依順序出現：</span><span class="sxs-lookup"><span data-stu-id="255ce-715">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="255ce-716">此程式碼會使用依順序的參數值建立記錄訊息：</span><span class="sxs-lookup"><span data-stu-id="255ce-716">This code creates a log message with the parameter values in sequence:</span></span>

```text
Parameter values: parm1, parm2
```

<span data-ttu-id="255ce-717">記錄架構以這種方式運作，因此記錄提供者可以實作[語意記錄 (亦稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="255ce-717">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="255ce-718">引數本身會被傳遞到記錄系統，而不只是格式化的訊息範本。</span><span class="sxs-lookup"><span data-stu-id="255ce-718">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="255ce-719">此資訊可讓記錄提供者將參數值儲存為欄位。</span><span class="sxs-lookup"><span data-stu-id="255ce-719">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="255ce-720">例如，假設記錄器方法呼叫看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="255ce-720">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {Id} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="255ce-721">若您正在將記錄傳送到 Azure 表格儲存體，每個 Azure 資料表實體都可以有 `ID` 與 `RequestTime` 屬性，以簡化記錄資料的查詢。</span><span class="sxs-lookup"><span data-stu-id="255ce-721">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="255ce-722">查詢可以尋找特定 `RequestTime` 範圍內的所有記錄，而不需要從文字訊息剖析時間。</span><span class="sxs-lookup"><span data-stu-id="255ce-722">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="255ce-723">記錄例外狀況</span><span class="sxs-lookup"><span data-stu-id="255ce-723">Logging exceptions</span></span>

<span data-ttu-id="255ce-724">記錄器方法具有多載，可讓您傳入例外狀況，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="255ce-724">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="255ce-725">不同提供者處理例外狀況資訊的方式會不同。</span><span class="sxs-lookup"><span data-stu-id="255ce-725">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="255ce-726">以下是來自上述程式碼中的偵錯提供者輸出範例。</span><span class="sxs-lookup"><span data-stu-id="255ce-726">Here's an example of Debug provider output from the code shown above.</span></span>

```text
TodoApiSample.Controllers.TodoController: Warning: GetById(55) NOT FOUND

System.Exception: Item not found exception.
   at TodoApiSample.Controllers.TodoController.GetById(String id) in C:\TodoApiSample\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="255ce-727">記錄篩選</span><span class="sxs-lookup"><span data-stu-id="255ce-727">Log filtering</span></span>

<span data-ttu-id="255ce-728">您可以指定特定提供者和類別的最低記錄層級，也可以指定所有提供者或所有類別的最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-728">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="255ce-729">最低層級以下的任何記錄都不會傳遞給該提供者，因此不會顯示或儲存。</span><span class="sxs-lookup"><span data-stu-id="255ce-729">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="255ce-730">若要隱藏所有記錄，請指定 `LogLevel.None` 作為最低記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-730">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="255ce-731">`LogLevel.None` 的整數值為 6，高於 `LogLevel.Critical` (5)。</span><span class="sxs-lookup"><span data-stu-id="255ce-731">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="255ce-732">在組態中建立篩選規則</span><span class="sxs-lookup"><span data-stu-id="255ce-732">Create filter rules in configuration</span></span>

<span data-ttu-id="255ce-733">專案範本代碼調用`CreateDefaultBuilder`為主控台、除錯和事件來源(ASP.NET核心 2.2或更高版本)提供程式設定日誌記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-733">The project template code calls `CreateDefaultBuilder` to set up logging for the Console, Debug, and EventSource (ASP.NET Core 2.2 or later) providers.</span></span> <span data-ttu-id="255ce-734">`CreateDefaultBuilder` 方法會設定記錄以尋找 `Logging` 區段中的組態，如[本文先前所述](#configuration)。</span><span class="sxs-lookup"><span data-stu-id="255ce-734">The `CreateDefaultBuilder` method sets up logging to look for configuration in a `Logging` section, as explained [earlier in this article](#configuration).</span></span>

<span data-ttu-id="255ce-735">組態資料會依提供者和類別指定最低記錄層級，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="255ce-735">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="255ce-736">此 JSON 會建立六個篩選規則：一個適用於偵錯提供者、四個適用於主控台提供者，一個適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-736">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="255ce-737">建立 `ILogger` 物件時，會為每個提供者選取一個規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-737">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="255ce-738">程式碼中的篩選規則</span><span class="sxs-lookup"><span data-stu-id="255ce-738">Filter rules in code</span></span>

<span data-ttu-id="255ce-739">下列範例說明如何在程式碼中註冊篩選規則：</span><span class="sxs-lookup"><span data-stu-id="255ce-739">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="255ce-740">第二個 `AddFilter` 會使用其類型名稱來指定偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-740">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="255ce-741">第一個 `AddFilter` 由於未指定提供者類型，因此適用於所有提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-741">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="255ce-742">如何套用篩選規則</span><span class="sxs-lookup"><span data-stu-id="255ce-742">How filtering rules are applied</span></span>

<span data-ttu-id="255ce-743">組態資料和上述範例中所示的 `AddFilter` 程式碼會建立下表中所示的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-743">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="255ce-744">前六項來自組態範例，最後兩項來自程式碼範例。</span><span class="sxs-lookup"><span data-stu-id="255ce-744">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="255ce-745">Number</span><span class="sxs-lookup"><span data-stu-id="255ce-745">Number</span></span> | <span data-ttu-id="255ce-746">提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-746">Provider</span></span>      | <span data-ttu-id="255ce-747">開頭如下的類別...</span><span class="sxs-lookup"><span data-stu-id="255ce-747">Categories that begin with ...</span></span>          | <span data-ttu-id="255ce-748">最低記錄層級</span><span class="sxs-lookup"><span data-stu-id="255ce-748">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="255ce-749">1</span><span class="sxs-lookup"><span data-stu-id="255ce-749">1</span></span>      | <span data-ttu-id="255ce-750">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-750">Debug</span></span>         | <span data-ttu-id="255ce-751">所有類別</span><span class="sxs-lookup"><span data-stu-id="255ce-751">All categories</span></span>                          | <span data-ttu-id="255ce-752">資訊</span><span class="sxs-lookup"><span data-stu-id="255ce-752">Information</span></span>       |
| <span data-ttu-id="255ce-753">2</span><span class="sxs-lookup"><span data-stu-id="255ce-753">2</span></span>      | <span data-ttu-id="255ce-754">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-754">Console</span></span>       | <span data-ttu-id="255ce-755">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="255ce-755">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="255ce-756">警告</span><span class="sxs-lookup"><span data-stu-id="255ce-756">Warning</span></span>           |
| <span data-ttu-id="255ce-757">3</span><span class="sxs-lookup"><span data-stu-id="255ce-757">3</span></span>      | <span data-ttu-id="255ce-758">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-758">Console</span></span>       | <span data-ttu-id="255ce-759">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="255ce-759">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="255ce-760">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-760">Debug</span></span>             |
| <span data-ttu-id="255ce-761">4</span><span class="sxs-lookup"><span data-stu-id="255ce-761">4</span></span>      | <span data-ttu-id="255ce-762">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-762">Console</span></span>       | <span data-ttu-id="255ce-763">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="255ce-763">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="255ce-764">錯誤</span><span class="sxs-lookup"><span data-stu-id="255ce-764">Error</span></span>             |
| <span data-ttu-id="255ce-765">5</span><span class="sxs-lookup"><span data-stu-id="255ce-765">5</span></span>      | <span data-ttu-id="255ce-766">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-766">Console</span></span>       | <span data-ttu-id="255ce-767">所有類別</span><span class="sxs-lookup"><span data-stu-id="255ce-767">All categories</span></span>                          | <span data-ttu-id="255ce-768">資訊</span><span class="sxs-lookup"><span data-stu-id="255ce-768">Information</span></span>       |
| <span data-ttu-id="255ce-769">6</span><span class="sxs-lookup"><span data-stu-id="255ce-769">6</span></span>      | <span data-ttu-id="255ce-770">所有提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-770">All providers</span></span> | <span data-ttu-id="255ce-771">所有類別</span><span class="sxs-lookup"><span data-stu-id="255ce-771">All categories</span></span>                          | <span data-ttu-id="255ce-772">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-772">Debug</span></span>             |
| <span data-ttu-id="255ce-773">7</span><span class="sxs-lookup"><span data-stu-id="255ce-773">7</span></span>      | <span data-ttu-id="255ce-774">所有提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-774">All providers</span></span> | <span data-ttu-id="255ce-775">系統</span><span class="sxs-lookup"><span data-stu-id="255ce-775">System</span></span>                                  | <span data-ttu-id="255ce-776">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-776">Debug</span></span>             |
| <span data-ttu-id="255ce-777">8</span><span class="sxs-lookup"><span data-stu-id="255ce-777">8</span></span>      | <span data-ttu-id="255ce-778">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-778">Debug</span></span>         | <span data-ttu-id="255ce-779">Microsoft</span><span class="sxs-lookup"><span data-stu-id="255ce-779">Microsoft</span></span>                               | <span data-ttu-id="255ce-780">追蹤</span><span class="sxs-lookup"><span data-stu-id="255ce-780">Trace</span></span>             |

<span data-ttu-id="255ce-781">建立 `ILogger` 物件時，`ILoggerFactory` 物件會針對每個提供者選取一個規則來套用到該記錄器。</span><span class="sxs-lookup"><span data-stu-id="255ce-781">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="255ce-782">由 `ILogger` 執行個體寫入的所有訊息都會根據選取的規則進行篩選。</span><span class="sxs-lookup"><span data-stu-id="255ce-782">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="255ce-783">系統會從可用的規則中，盡可能選取對每個提供者和類別配對最明確的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-783">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="255ce-784">當建立指定類別的 `ILogger` 時，系統會針對每個提供者使用下列演算法：</span><span class="sxs-lookup"><span data-stu-id="255ce-784">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="255ce-785">選取所有符合提供者或其別名的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-785">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="255ce-786">如果找不到符合的項目，請選取所有規則搭配空白提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-786">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="255ce-787">從上一個步驟的結果中，選取具有最長相符類別前置字元的規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-787">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="255ce-788">如果找不到符合的項目，請選取未指定類別的所有規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-788">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="255ce-789">如果選取多個規則，請使用**最後**一個。</span><span class="sxs-lookup"><span data-stu-id="255ce-789">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="255ce-790">如果未選取任何規則，請使用 `MinimumLevel`。</span><span class="sxs-lookup"><span data-stu-id="255ce-790">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="255ce-791">使用上述規則清單時，假設您建立 "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" 類別的 `ILogger` 物件：</span><span class="sxs-lookup"><span data-stu-id="255ce-791">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="255ce-792">針對偵錯提供者，規則 1、6 和 8 均適用。</span><span class="sxs-lookup"><span data-stu-id="255ce-792">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="255ce-793">規則 8 最明確，因此會選取此規則。</span><span class="sxs-lookup"><span data-stu-id="255ce-793">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="255ce-794">針對主控台提供者，規則 3、4、5 和 6 均適用。</span><span class="sxs-lookup"><span data-stu-id="255ce-794">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="255ce-795">規則 3 最明確。</span><span class="sxs-lookup"><span data-stu-id="255ce-795">Rule 3 is most specific.</span></span>

<span data-ttu-id="255ce-796">產生的 `ILogger` 執行個體會傳送 `Trace` 層級以上的記錄到偵錯提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-796">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="255ce-797">`Debug` 層級以上的記錄會傳送到主控台提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-797">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="255ce-798">提供者別名</span><span class="sxs-lookup"><span data-stu-id="255ce-798">Provider aliases</span></span>

<span data-ttu-id="255ce-799">每個提供者都會定義「別名」\*\*，可在設定中用來取代完整類型名稱。</span><span class="sxs-lookup"><span data-stu-id="255ce-799">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="255ce-800">針對內建提供者，請使用下列別名：</span><span class="sxs-lookup"><span data-stu-id="255ce-800">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="255ce-801">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-801">Console</span></span>
* <span data-ttu-id="255ce-802">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-802">Debug</span></span>
* <span data-ttu-id="255ce-803">EventSource</span><span class="sxs-lookup"><span data-stu-id="255ce-803">EventSource</span></span>
* <span data-ttu-id="255ce-804">EventLog</span><span class="sxs-lookup"><span data-stu-id="255ce-804">EventLog</span></span>
* <span data-ttu-id="255ce-805">TraceSource</span><span class="sxs-lookup"><span data-stu-id="255ce-805">TraceSource</span></span>
* <span data-ttu-id="255ce-806">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="255ce-806">AzureAppServicesFile</span></span>
* <span data-ttu-id="255ce-807">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="255ce-807">AzureAppServicesBlob</span></span>
* <span data-ttu-id="255ce-808">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="255ce-808">ApplicationInsights</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="255ce-809">預設最低層級</span><span class="sxs-lookup"><span data-stu-id="255ce-809">Default minimum level</span></span>

<span data-ttu-id="255ce-810">只有組態或程式碼中沒有適用於指定提供者和類別的規則時，最低層級設定才會生效。</span><span class="sxs-lookup"><span data-stu-id="255ce-810">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="255ce-811">下列範例示範如何設定最低層級：</span><span class="sxs-lookup"><span data-stu-id="255ce-811">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="255ce-812">如果您未明確設定最低層級，預設值為 `Information`，這表示會略過 `Trace` 和 `Debug` 記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-812">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="255ce-813">篩選函數</span><span class="sxs-lookup"><span data-stu-id="255ce-813">Filter functions</span></span>

<span data-ttu-id="255ce-814">針對組態或程式碼未指派規則的所有提供者和類別，會叫用篩選函式。</span><span class="sxs-lookup"><span data-stu-id="255ce-814">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="255ce-815">函式中的程式碼可以存取提供者類型、類別與記錄層級。</span><span class="sxs-lookup"><span data-stu-id="255ce-815">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="255ce-816">例如：</span><span class="sxs-lookup"><span data-stu-id="255ce-816">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

## <a name="system-categories-and-levels"></a><span data-ttu-id="255ce-817">系統類別與層級</span><span class="sxs-lookup"><span data-stu-id="255ce-817">System categories and levels</span></span>

<span data-ttu-id="255ce-818">以下是由 ASP.NET Core 與 Entity Framework Core 所使用的一些類別，以及有關它們可傳回哪些記錄的附註：</span><span class="sxs-lookup"><span data-stu-id="255ce-818">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="255ce-819">類別</span><span class="sxs-lookup"><span data-stu-id="255ce-819">Category</span></span>                            | <span data-ttu-id="255ce-820">注意</span><span class="sxs-lookup"><span data-stu-id="255ce-820">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="255ce-821">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="255ce-821">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="255ce-822">一般 ASP.NET Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="255ce-822">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="255ce-823">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="255ce-823">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="255ce-824">已考慮、發現及使用哪些金鑰。</span><span class="sxs-lookup"><span data-stu-id="255ce-824">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="255ce-825">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="255ce-825">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="255ce-826">允許主機。</span><span class="sxs-lookup"><span data-stu-id="255ce-826">Hosts allowed.</span></span> |
| <span data-ttu-id="255ce-827">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="255ce-827">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="255ce-828">HTTP 要求花了多少時間完成，以及其開始時間。</span><span class="sxs-lookup"><span data-stu-id="255ce-828">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="255ce-829">載入了哪些裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="255ce-829">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="255ce-830">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="255ce-830">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="255ce-831">MVC 與 Razor 診斷。</span><span class="sxs-lookup"><span data-stu-id="255ce-831">MVC and Razor diagnostics.</span></span> <span data-ttu-id="255ce-832">模型繫結、篩選執行、檢視編譯、動作選取。</span><span class="sxs-lookup"><span data-stu-id="255ce-832">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="255ce-833">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="255ce-833">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="255ce-834">路由比對資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-834">Route matching information.</span></span> |
| <span data-ttu-id="255ce-835">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="255ce-835">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="255ce-836">連線開始、停止與保持運作回應。</span><span class="sxs-lookup"><span data-stu-id="255ce-836">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="255ce-837">HTTPS 憑證資訊。</span><span class="sxs-lookup"><span data-stu-id="255ce-837">HTTPS certificate information.</span></span> |
| <span data-ttu-id="255ce-838">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="255ce-838">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="255ce-839">提供的檔案。</span><span class="sxs-lookup"><span data-stu-id="255ce-839">Files served.</span></span> |
| <span data-ttu-id="255ce-840">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="255ce-840">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="255ce-841">一般 Entity Framework Core 診斷。</span><span class="sxs-lookup"><span data-stu-id="255ce-841">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="255ce-842">資料庫活動與設定、變更偵測、移轉。</span><span class="sxs-lookup"><span data-stu-id="255ce-842">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="255ce-843">記錄範圍</span><span class="sxs-lookup"><span data-stu-id="255ce-843">Log scopes</span></span>

 <span data-ttu-id="255ce-844">「範圍」\*\* 可用來將邏輯作業組成群組。</span><span class="sxs-lookup"><span data-stu-id="255ce-844">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="255ce-845">此分組功能可用來將相同的資料附加到已建立為集合之一部分的每個記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-845">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="255ce-846">例如，在處理邀交易時建立的每個記錄都可以包括該交易識別碼。</span><span class="sxs-lookup"><span data-stu-id="255ce-846">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="255ce-847">範圍是 <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> 方法所傳回的 `IDisposable` 類型，並會持續到被處置為止。</span><span class="sxs-lookup"><span data-stu-id="255ce-847">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="255ce-848">透過將記錄器呼叫封裝在 `using` 區塊中以使用範圍：</span><span class="sxs-lookup"><span data-stu-id="255ce-848">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="255ce-849">下列程式碼會啟用主控台提供者的範圍：</span><span class="sxs-lookup"><span data-stu-id="255ce-849">The following code enables scopes for the console provider:</span></span>

<span data-ttu-id="255ce-850">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="255ce-850">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="255ce-851">您必須設定 `IncludeScopes` 主控台記錄器選項才能啟用範圍記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-851">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="255ce-852">如需有關設定的詳細資訊，請參閱[設定](#configuration)一節。</span><span class="sxs-lookup"><span data-stu-id="255ce-852">For information on configuration, see the [Configuration](#configuration) section.</span></span>

<span data-ttu-id="255ce-853">每個記錄訊息包含範圍資訊：</span><span class="sxs-lookup"><span data-stu-id="255ce-853">Each log message includes the scoped information:</span></span>

```
info: TodoApiSample.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApiSample.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApiSample.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="255ce-854">內建記錄提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-854">Built-in logging providers</span></span>

<span data-ttu-id="255ce-855">ASP.NET Core 隨附下列提供者：</span><span class="sxs-lookup"><span data-stu-id="255ce-855">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="255ce-856">主控台</span><span class="sxs-lookup"><span data-stu-id="255ce-856">Console</span></span>](#console-provider)
* [<span data-ttu-id="255ce-857">偵錯</span><span class="sxs-lookup"><span data-stu-id="255ce-857">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="255ce-858">事件來源</span><span class="sxs-lookup"><span data-stu-id="255ce-858">EventSource</span></span>](#event-source-provider)
* [<span data-ttu-id="255ce-859">EventLog</span><span class="sxs-lookup"><span data-stu-id="255ce-859">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="255ce-860">TraceSource</span><span class="sxs-lookup"><span data-stu-id="255ce-860">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="255ce-861">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="255ce-861">AzureAppServicesFile</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="255ce-862">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="255ce-862">AzureAppServicesBlob</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="255ce-863">應用洞察</span><span class="sxs-lookup"><span data-stu-id="255ce-863">ApplicationInsights</span></span>](#azure-application-insights-trace-logging)

<span data-ttu-id="255ce-864">如需 ASP.NET Core 模組的 StdOut 和偵錯記錄相關資訊，請參閱 <xref:test/troubleshoot-azure-iis> 和 <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>。</span><span class="sxs-lookup"><span data-stu-id="255ce-864">For information on stdout and debug logging with the ASP.NET Core Module, see <xref:test/troubleshoot-azure-iis> and <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="255ce-865">Console 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-865">Console provider</span></span>

<span data-ttu-id="255ce-866">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 提供者套件會將記錄輸出傳送至主控台。</span><span class="sxs-lookup"><span data-stu-id="255ce-866">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

```csharp
logging.AddConsole();
```

<span data-ttu-id="255ce-867">若要查看主控台記錄輸出，請在專案資料夾中開啟命令提示字元，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="255ce-867">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```dotnetcli
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="255ce-868">Debug 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-868">Debug provider</span></span>

<span data-ttu-id="255ce-869">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 提供者套件使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 類別 (`Debug.WriteLine` 方法呼叫) 來寫入記錄輸出。</span><span class="sxs-lookup"><span data-stu-id="255ce-869">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="255ce-870">在 Linux 上，此提供者會將記錄寫入至 */var/log/message*。</span><span class="sxs-lookup"><span data-stu-id="255ce-870">On Linux, this provider writes logs to */var/log/message*.</span></span>

```csharp
logging.AddDebug();
```

### <a name="event-source-provider"></a><span data-ttu-id="255ce-871">事件來源提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-871">Event Source provider</span></span>

<span data-ttu-id="255ce-872">[Microsoft.擴展.日誌記錄.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)提供程式包寫入具有`Microsoft-Extensions-Logging`名稱 的事件源跨平臺。</span><span class="sxs-lookup"><span data-stu-id="255ce-872">The [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package writes to an Event Source cross-platform with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="255ce-873">在 Windows 上,提供者使用[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)。</span><span class="sxs-lookup"><span data-stu-id="255ce-873">On Windows, the provider uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span>

```csharp
logging.AddEventSourceLogger();
```

<span data-ttu-id="255ce-874">調用事件源提供程式以生成主機時`CreateDefaultBuilder`自動添加。</span><span class="sxs-lookup"><span data-stu-id="255ce-874">The Event Source provider is added automatically when `CreateDefaultBuilder` is called to build the host.</span></span>

<span data-ttu-id="255ce-875">使用[PerfView 實用程式](https://github.com/Microsoft/perfview)收集和查看日誌。</span><span class="sxs-lookup"><span data-stu-id="255ce-875">Use the [PerfView utility](https://github.com/Microsoft/perfview) to collect and view logs.</span></span> <span data-ttu-id="255ce-876">此外還有一些其他工具可檢視 ETW 記錄，但 PerfView 提供處理 ASP.NET Core 所發出 ETW 事件的最佳體驗。</span><span class="sxs-lookup"><span data-stu-id="255ce-876">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET Core.</span></span>

<span data-ttu-id="255ce-877">若要設定 PerfView 以收集此提供者所記錄的事件，請將字串 `*Microsoft-Extensions-Logging` 新增至 [其他提供者]\*\*\*\* 清單</span><span class="sxs-lookup"><span data-stu-id="255ce-877">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="255ce-878">(請勿遺漏字串開頭的星號)。</span><span class="sxs-lookup"><span data-stu-id="255ce-878">(Don't miss the asterisk at the start of the string.)</span></span>

![PerfView 的其他提供者](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="255ce-880">Windows EventLog 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-880">Windows EventLog provider</span></span>

<span data-ttu-id="255ce-881">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 提供者套件會將記錄輸出傳送至 Windows 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="255ce-881">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

```csharp
logging.AddEventLog();
```

<span data-ttu-id="255ce-882">[AddEventLog 多載](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可讓您傳入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。</span><span class="sxs-lookup"><span data-stu-id="255ce-882">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="255ce-883">如果`null`或未指定,則使用以下預設設定:</span><span class="sxs-lookup"><span data-stu-id="255ce-883">If `null` or not specified, the following default settings are used:</span></span>

* <span data-ttu-id="255ce-884">`LogName`&ndash; "申請"</span><span class="sxs-lookup"><span data-stu-id="255ce-884">`LogName` &ndash; "Application"</span></span>
* <span data-ttu-id="255ce-885">`SourceName`&ndash; ".NET 運行時"</span><span class="sxs-lookup"><span data-stu-id="255ce-885">`SourceName` &ndash; ".NET Runtime"</span></span>
* <span data-ttu-id="255ce-886">`MachineName` &ndash; 本機電腦</span><span class="sxs-lookup"><span data-stu-id="255ce-886">`MachineName` &ndash; local machine</span></span>

<span data-ttu-id="255ce-887">事件記錄為[警告等級和更高](#log-level)。</span><span class="sxs-lookup"><span data-stu-id="255ce-887">Events are logged for [Warning level and higher](#log-level).</span></span> <span data-ttu-id="255ce-888">要記錄低於`Warning`的事件,請顯式設置日誌級別。</span><span class="sxs-lookup"><span data-stu-id="255ce-888">To log events lower than `Warning`, explicitly set the log level.</span></span> <span data-ttu-id="255ce-889">例如,將以下內容添加到*appsettings.json*檔:</span><span class="sxs-lookup"><span data-stu-id="255ce-889">For example, add the following to the *appsettings.json* file:</span></span>

```json
"EventLog": {
  "LogLevel": {
    "Default": "Information"
  }
}
```

### <a name="tracesource-provider"></a><span data-ttu-id="255ce-890">TraceSource 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-890">TraceSource provider</span></span>

<span data-ttu-id="255ce-891">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) 提供者套件使用 <xref:System.Diagnostics.TraceSource> 程式庫與提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-891">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

```csharp
logging.AddTraceSource(sourceSwitchName);
```

<span data-ttu-id="255ce-892">[AddTraceSource 多載](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)可讓您傳入來源參數和追蹤接聽項。</span><span class="sxs-lookup"><span data-stu-id="255ce-892">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="255ce-893">若要使用此提供者，應用程式必須在 .NET Framework (而非 .NET Core) 上執行。</span><span class="sxs-lookup"><span data-stu-id="255ce-893">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="255ce-894">該提供者可讓您將訊息路由傳送到種不同的[接聽程式](/dotnet/framework/debug-trace-profile/trace-listeners)，例如範例應用程式中所使用的 <xref:System.Diagnostics.TextWriterTraceListener>。</span><span class="sxs-lookup"><span data-stu-id="255ce-894">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

### <a name="azure-app-service-provider"></a><span data-ttu-id="255ce-895">Azure App Service 提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-895">Azure App Service provider</span></span>

<span data-ttu-id="255ce-896">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供者套件會將記錄寫入至 Azure App Service 應用程式檔案系統中的文字檔，並寫入至 Azure 儲存體帳戶中的 [Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="255ce-896">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="255ce-897">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含提供者套件。</span><span class="sxs-lookup"><span data-stu-id="255ce-897">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="255ce-898">當以 .NET Framework 為目標或是參考 `Microsoft.AspNetCore.App` 中繼套件時，請將提供者套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="255ce-898">When targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> 

<span data-ttu-id="255ce-899"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> 多載可讓您傳入 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>。</span><span class="sxs-lookup"><span data-stu-id="255ce-899">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="255ce-900">設定物件可覆寫預設設定，例如記錄輸出範本、Blob 名稱與檔案大小限制。</span><span class="sxs-lookup"><span data-stu-id="255ce-900">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="255ce-901">(*輸出範本*是訊息範本，它除了會套用到 `ILogger` 方法呼叫所提供的記錄之外，還會套用到所有記錄。)</span><span class="sxs-lookup"><span data-stu-id="255ce-901">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

<span data-ttu-id="255ce-902">當您部署到 App Service 應用程式時，應用程式會遵循 Azure 入口網站 [App Service]\*\*\*\* 頁面中 [App Service 記錄](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag)區段的設定。</span><span class="sxs-lookup"><span data-stu-id="255ce-902">When you deploy to an App Service app, the application honors the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="255ce-903">當下列設定更新時，變更會立即生效，而不需要重新啟動或重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="255ce-903">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

* <span data-ttu-id="255ce-904">**應用程式記錄 (檔案系統)**</span><span class="sxs-lookup"><span data-stu-id="255ce-904">**Application Logging (Filesystem)**</span></span>
* <span data-ttu-id="255ce-905">**應用程式記錄 (Blob)**</span><span class="sxs-lookup"><span data-stu-id="255ce-905">**Application Logging (Blob)**</span></span>

<span data-ttu-id="255ce-906">記錄檔的預設位置為 *D:\\home\\LogFiles\\Application* 資料夾，而預設檔案名稱為 *diagnostics-yyyymmdd.txt*。</span><span class="sxs-lookup"><span data-stu-id="255ce-906">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="255ce-907">預設檔案大小限制為 10 MB，而預設保留的檔案數目上限為 2。</span><span class="sxs-lookup"><span data-stu-id="255ce-907">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="255ce-908">預設 Blob 名稱為 *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*。</span><span class="sxs-lookup"><span data-stu-id="255ce-908">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="255ce-909">此提供者僅適用於專案在 Azure 環境中執行的情況。</span><span class="sxs-lookup"><span data-stu-id="255ce-909">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="255ce-910">若在本機執行專案，不會有任何作用，即它不會寫入本機檔案或 blob 的本機開發儲存體。</span><span class="sxs-lookup"><span data-stu-id="255ce-910">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="255ce-911">Azure 記錄資料流</span><span class="sxs-lookup"><span data-stu-id="255ce-911">Azure log streaming</span></span>

<span data-ttu-id="255ce-912">Azure 記錄串流可讓您即時檢視來自下列位置的記錄活動：</span><span class="sxs-lookup"><span data-stu-id="255ce-912">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="255ce-913">應用程式伺服器</span><span class="sxs-lookup"><span data-stu-id="255ce-913">The app server</span></span>
* <span data-ttu-id="255ce-914">網頁伺服器</span><span class="sxs-lookup"><span data-stu-id="255ce-914">The web server</span></span>
* <span data-ttu-id="255ce-915">失敗的要求追蹤</span><span class="sxs-lookup"><span data-stu-id="255ce-915">Failed request tracing</span></span>

<span data-ttu-id="255ce-916">若要設定 Azure 記錄資料流：</span><span class="sxs-lookup"><span data-stu-id="255ce-916">To configure Azure log streaming:</span></span>

* <span data-ttu-id="255ce-917">從您應用程式的入口網站頁面瀏覽到 [App Service 記錄]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-917">Navigate to the **App Service logs** page from your app's portal page.</span></span>
* <span data-ttu-id="255ce-918">將 [應用程式記錄 (檔案系統)]\*\*\*\* 設定為 [開啟]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-918">Set **Application Logging (Filesystem)** to **On**.</span></span>
* <span data-ttu-id="255ce-919">選擇記錄 [層級]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="255ce-919">Choose the log **Level**.</span></span> <span data-ttu-id="255ce-920">此設置僅適用於 Azure 日誌流,不適用於應用中的其他日誌記錄提供程式。</span><span class="sxs-lookup"><span data-stu-id="255ce-920">This setting only applies to Azure log streaming, not other logging providers in the app.</span></span>

<span data-ttu-id="255ce-921">瀏覽到 [記錄資料流]\*\*\*\* 頁面以檢視應用程式訊息。</span><span class="sxs-lookup"><span data-stu-id="255ce-921">Navigate to the **Log Stream** page to view app messages.</span></span> <span data-ttu-id="255ce-922">這些是應用程式透過 `ILogger` 介面產生的訊息。</span><span class="sxs-lookup"><span data-stu-id="255ce-922">They're logged by the app through the `ILogger` interface.</span></span>

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="255ce-923">Azure Application Insights 追蹤記錄</span><span class="sxs-lookup"><span data-stu-id="255ce-923">Azure Application Insights trace logging</span></span>

<span data-ttu-id="255ce-924">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) \(英文\) 提供者套件會將記錄寫入至 Azure Application Insights。</span><span class="sxs-lookup"><span data-stu-id="255ce-924">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to Azure Application Insights.</span></span> <span data-ttu-id="255ce-925">Application Insights 是可監視 Web 應用程式的服務，並提供可用來查詢及分析遙測資料的工具。</span><span class="sxs-lookup"><span data-stu-id="255ce-925">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="255ce-926">如果您使用此提供者，就可以使用 Application Insights 工具來查詢及分析記錄。</span><span class="sxs-lookup"><span data-stu-id="255ce-926">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="255ce-927">記錄提供者會以 [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) \(英文\) 的相依性形式隨附，這是針對 ASP.NET Core 提供所有可用遙測的套件。</span><span class="sxs-lookup"><span data-stu-id="255ce-927">The logging provider is included as a dependency of [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), which is the package that provides all available telemetry for ASP.NET Core.</span></span> <span data-ttu-id="255ce-928">如果您使用此套件，就不需安裝提供者套件。</span><span class="sxs-lookup"><span data-stu-id="255ce-928">If you use this package, you don't have to install the provider package.</span></span>

<span data-ttu-id="255ce-929">不要使用 [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) \(英文\) 套件&mdash;該套件適用於 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="255ce-929">Don't use the [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;that's for ASP.NET 4.x.</span></span>

<span data-ttu-id="255ce-930">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="255ce-930">For more information, see the following resources:</span></span>

* [<span data-ttu-id="255ce-931">Application Insights 概觀</span><span class="sxs-lookup"><span data-stu-id="255ce-931">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* <span data-ttu-id="255ce-932">[適用於 ASP.NET Core 應用程式的 Application Insights](/azure/azure-monitor/app/asp-net-core)：如果您想要實作完整範圍的 Application Insights 遙測以及記錄，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="255ce-932">[Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) - Start here if you want to implement the full range of Application Insights telemetry along with logging.</span></span>
* <span data-ttu-id="255ce-933">[適用於 .NET Core ILogger 記錄的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger)：如果您想要實作記錄提供者，而不需要 Application Insights 遙測的其餘部分，請從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="255ce-933">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
* <span data-ttu-id="255ce-934">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs) (Application Insights 記錄配接器)。</span><span class="sxs-lookup"><span data-stu-id="255ce-934">[Application Insights logging adapters](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
* <span data-ttu-id="255ce-935">[安裝、設定及初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights)：Microsoft Learn 網站上的互動式教學課程。</span><span class="sxs-lookup"><span data-stu-id="255ce-935">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="255ce-936">協力廠商記錄提供者</span><span class="sxs-lookup"><span data-stu-id="255ce-936">Third-party logging providers</span></span>

<span data-ttu-id="255ce-937">可搭配 ASP.NET Core 使用的協力廠商記錄架構：</span><span class="sxs-lookup"><span data-stu-id="255ce-937">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="255ce-938">[elmah.io](https://elmah.io/) ([GitHub 存放庫](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-938">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="255ce-939">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub 存放庫](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-939">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="255ce-940">[JSNLog](https://jsnlog.com/) ([GitHub 存放庫](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="255ce-940">[JSNLog](https://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="255ce-941">[KissLog.net](https://kisslog.net/) ([GitHub 存放庫](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="255ce-941">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="255ce-942">[紀錄4Net](https://logging.apache.org/log4net/) ([GitHub 儲存函式庫](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span><span class="sxs-lookup"><span data-stu-id="255ce-942">[Log4Net](https://logging.apache.org/log4net/) ([GitHub repo](https://github.com/huorswords/Microsoft.Extensions.Logging.Log4Net.AspNetCore))</span></span>
* <span data-ttu-id="255ce-943">[Loggr](https://loggr.net/) ([GitHub 存放庫](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-943">[Loggr](https://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="255ce-944">[NLog](https://nlog-project.org/) ([GitHub 存放庫](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="255ce-944">[NLog](https://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="255ce-945">[Sentry](https://sentry.io/welcome/) ([GitHub 存放庫](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="255ce-945">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="255ce-946">[Serilog](https://serilog.net/) ([GitHub 存放庫](https://github.com/serilog/serilog-aspnetcore))</span><span class="sxs-lookup"><span data-stu-id="255ce-946">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-aspnetcore))</span></span>
* <span data-ttu-id="255ce-947">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github 存放庫](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="255ce-947">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="255ce-948">某些協力廠商架構可以執行[語意記錄 (也稱為結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="255ce-948">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="255ce-949">使用協力廠商架構類似於使用內建的提供者之一：</span><span class="sxs-lookup"><span data-stu-id="255ce-949">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="255ce-950">將 NuGet 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="255ce-950">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="255ce-951">調用日誌記錄`ILoggerFactory`框架提供的擴展方法。</span><span class="sxs-lookup"><span data-stu-id="255ce-951">Call an `ILoggerFactory` extension method provided by the logging framework.</span></span>

<span data-ttu-id="255ce-952">如需詳細資訊，請參閱每個提供者的文件。</span><span class="sxs-lookup"><span data-stu-id="255ce-952">For more information, see each provider's documentation.</span></span> <span data-ttu-id="255ce-953">Microsoft 不支援第三方記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="255ce-953">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="255ce-954">其他資源</span><span class="sxs-lookup"><span data-stu-id="255ce-954">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>

::: moniker-end
