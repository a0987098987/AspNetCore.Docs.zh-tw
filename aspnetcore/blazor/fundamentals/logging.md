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
# <a name="aspnet-core-blazor-logging"></a>ASP.NET Core Blazor 記錄

## <a name="blazor-webassembly"></a>BlazorWebAssembly

Blazor使用中的屬性來設定 WebAssembly apps 中的記錄 `WebAssemblyHostBuilder.Logging` `Program.Main` ：

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

...

var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.Logging.SetMinimumLevel(LogLevel.Debug);
builder.Logging.AddProvider(new CustomLoggingProvider());
```

`Logging`屬性的類型為 <xref:Microsoft.Extensions.Logging.ILoggingBuilder> ，因此中提供的所有擴充方法 <xref:Microsoft.Extensions.Logging.ILoggingBuilder> 也可在上取得 `Logging` 。

記錄設定可以從應用程式佈建檔載入。 如需詳細資訊，請參閱 <xref:blazor/fundamentals/configuration#logging-configuration>。

## <a name="blazor-server"></a>Blazor伺服器

如需一般 ASP.NET Core 記錄指引，請參閱 <xref:fundamentals/logging/index> 。

## <a name="blazor-webassembly-signalr-net-client-logging"></a>BlazorWebAssembly SignalR .net 用戶端記錄

插入 <xref:Microsoft.Extensions.Logging.ILoggerProvider> ，將加入至 `WebAssemblyConsoleLogger` 傳遞至的記錄提供者 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> 。 不同于傳統 <xref:Microsoft.Extensions.Logging.Console.ConsoleLogger> ， `WebAssemblyConsoleLogger` 是瀏覽器專屬記錄 api 的包裝函式（例如， `console.log` ）。 在瀏覽器內容中使用時，可以 `WebAssemblyConsoleLogger` 在 Mono 內進行記錄。

```csharp
@using Microsoft.Extensions.Logging
@inject ILoggerProvider LoggerProvider

...

var connection = new HubConnectionBuilder()
    .WithUrl(NavigationManager.ToAbsoluteUri("/chathub"))
    .ConfigureLogging(logging => logging.AddProvider(LoggerProvider))
    .Build();
```

## <a name="log-in-razor-components"></a>登入 Razor 元件

記錄器尊重應用程式啟動設定。

需要的指示詞 `using` <xref:Microsoft.Extensions.Logging> ，才能支援 Api 的 Intellisense 完成（例如 <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogWarning%2A> 和） <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError%2A> 。

下列範例示範如何 <xref:Microsoft.Extensions.Logging.ILogger> 在元件中使用進行記錄 Razor ：

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

下列範例示範如何 <xref:Microsoft.Extensions.Logging.ILoggerFactory> 在元件中使用進行記錄 Razor ：

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

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/index>
