---
title: ASP.NET Core Blazor 記錄
author: guardrex
description: 瞭解應用程式中的記錄 Blazor ，包括記錄層級的設定，以及如何從元件寫入記錄訊息 Razor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/logging
ms.openlocfilehash: 841c4021d9217312b2601b0e775542c6455cca82
ms.sourcegitcommit: dd2a1542a4a377123490034153368c135fdbd09e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85240870"
---
# <a name="aspnet-core-blazor-logging"></a><span data-ttu-id="69133-103">ASP.NET Core Blazor 記錄</span><span class="sxs-lookup"><span data-stu-id="69133-103">ASP.NET Core Blazor logging</span></span>

## <a name="blazor-webassembly"></a>Blazor<span data-ttu-id="69133-104">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="69133-104"> WebAssembly</span></span>

<span data-ttu-id="69133-105">Blazor使用中的屬性來設定 WebAssembly apps 中的記錄 `WebAssemblyHostBuilder.Logging` `Program.Main` ：</span><span class="sxs-lookup"><span data-stu-id="69133-105">Configure logging in Blazor WebAssembly apps with the `WebAssemblyHostBuilder.Logging` property in `Program.Main`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

...

var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.Logging.SetMinimumLevel(LogLevel.Debug);
builder.Logging.AddProvider(new CustomLoggingProvider());
```

<span data-ttu-id="69133-106">`Logging`屬性的類型為 <xref:Microsoft.Extensions.Logging.ILoggingBuilder> ，因此中提供的所有擴充方法 <xref:Microsoft.Extensions.Logging.ILoggingBuilder> 也可在上取得 `Logging` 。</span><span class="sxs-lookup"><span data-stu-id="69133-106">The `Logging` property is of type <xref:Microsoft.Extensions.Logging.ILoggingBuilder>, so all of the extension methods available on <xref:Microsoft.Extensions.Logging.ILoggingBuilder> are also available on `Logging`.</span></span>

<span data-ttu-id="69133-107">記錄設定可以從應用程式佈建檔載入。</span><span class="sxs-lookup"><span data-stu-id="69133-107">Logging configuration can be loaded from app settings files.</span></span> <span data-ttu-id="69133-108">如需詳細資訊，請參閱 <xref:blazor/fundamentals/configuration#logging-configuration>。</span><span class="sxs-lookup"><span data-stu-id="69133-108">For more information, see <xref:blazor/fundamentals/configuration#logging-configuration>.</span></span>

## <a name="blazor-server"></a>Blazor<span data-ttu-id="69133-109">伺服器</span><span class="sxs-lookup"><span data-stu-id="69133-109"> Server</span></span>

<span data-ttu-id="69133-110">如需一般 ASP.NET Core 記錄指引，請參閱 <xref:fundamentals/logging/index> 。</span><span class="sxs-lookup"><span data-stu-id="69133-110">For general ASP.NET Core logging guidance, see <xref:fundamentals/logging/index>.</span></span>

## <a name="blazor-webassembly-signalr-net-client-logging"></a>Blazor<span data-ttu-id="69133-111">WebAssembly SignalR .net 用戶端記錄</span><span class="sxs-lookup"><span data-stu-id="69133-111"> WebAssembly SignalR .NET client logging</span></span>

<span data-ttu-id="69133-112">插入 <xref:Microsoft.Extensions.Logging.ILoggerProvider> ，將加入至 `WebAssemblyConsoleLogger` 傳遞至的記錄提供者 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="69133-112">Inject an <xref:Microsoft.Extensions.Logging.ILoggerProvider> to add a `WebAssemblyConsoleLogger` to the logging providers passed to <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="69133-113">不同于傳統 <xref:Microsoft.Extensions.Logging.Console.ConsoleLogger> ， `WebAssemblyConsoleLogger` 是瀏覽器專屬記錄 api 的包裝函式（例如， `console.log` ）。</span><span class="sxs-lookup"><span data-stu-id="69133-113">Unlike a traditional <xref:Microsoft.Extensions.Logging.Console.ConsoleLogger>, `WebAssemblyConsoleLogger` is a wrapper around browser-specific logging APIs (for example, `console.log`).</span></span> <span data-ttu-id="69133-114">在瀏覽器內容中使用時，可以 `WebAssemblyConsoleLogger` 在 Mono 內進行記錄。</span><span class="sxs-lookup"><span data-stu-id="69133-114">Use of `WebAssemblyConsoleLogger` makes logging possible within Mono inside a browser context.</span></span>

```csharp
@using Microsoft.Extensions.Logging
@inject ILoggerProvider LoggerProvider

...

var connection = new HubConnectionBuilder()
    .WithUrl(NavigationManager.ToAbsoluteUri("/chathub"))
    .ConfigureLogging(logging => logging.AddProvider(LoggerProvider))
    .Build();
```

## <a name="log-in-razor-components"></a><span data-ttu-id="69133-115">登入 Razor 元件</span><span class="sxs-lookup"><span data-stu-id="69133-115">Log in Razor components</span></span>

<span data-ttu-id="69133-116">記錄器尊重應用程式啟動設定。</span><span class="sxs-lookup"><span data-stu-id="69133-116">Loggers respect app startup configuration.</span></span>

<span data-ttu-id="69133-117">需要的指示詞 `using` <xref:Microsoft.Extensions.Logging> ，才能支援 Api 的 Intellisense 完成（例如 <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> 和） <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> 。</span><span class="sxs-lookup"><span data-stu-id="69133-117">The `using` directive for <xref:Microsoft.Extensions.Logging> is required to support Intellisense completions for APIs, such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A>.</span></span>

<span data-ttu-id="69133-118">下列範例示範如何 <xref:Microsoft.Extensions.Logging.ILogger> 在元件中使用進行記錄 Razor ：</span><span class="sxs-lookup"><span data-stu-id="69133-118">The following example demonstrates logging with an <xref:Microsoft.Extensions.Logging.ILogger> in Razor components:</span></span>

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

<span data-ttu-id="69133-119">下列範例示範如何 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 在元件中使用進行記錄 Razor ：</span><span class="sxs-lookup"><span data-stu-id="69133-119">The following example demonstrates logging with an <xref:Microsoft.Extensions.Logging.ILoggerFactory> in Razor components:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="69133-120">其他資源</span><span class="sxs-lookup"><span data-stu-id="69133-120">Additional resources</span></span>

* <xref:fundamentals/logging/index>
