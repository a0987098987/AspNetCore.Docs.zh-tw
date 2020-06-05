---
title: 從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0
author: pakrym
description: 瞭解如何遷移使用 Microsoft Extensions 的 non-ASP.NET Core 應用程式。從2.1 到2.2 或3.0 的記錄。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2fd738ed0e0a06d0793e3c624d40a13725b53cd8
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84274228"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0

本文概述將使用 `Microsoft.Extensions.Logging` 從2.1 到2.2 或3.0 的 Non-ASP.NET Core 應用程式遷移的一般步驟。

## <a name="21-to-22"></a>2.1 至 2.2

手動建立 `ServiceCollection` 並呼叫 `AddLogging` 。

2.1 範例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2.2 範例：

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 到3。0

在3.0 中，請使用 `LoggingFactory.Create` 。

2.1 範例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3.0 範例：

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>其他資源

* [Microsoft Extensions]： [[主控台] NuGet 套件](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)。
* <xref:fundamentals/logging/index>
