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
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="10467-103">從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0</span><span class="sxs-lookup"><span data-stu-id="10467-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="10467-104">本文概述將使用 `Microsoft.Extensions.Logging` 從2.1 到2.2 或3.0 的 Non-ASP.NET Core 應用程式遷移的一般步驟。</span><span class="sxs-lookup"><span data-stu-id="10467-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="10467-105">2.1 至 2.2</span><span class="sxs-lookup"><span data-stu-id="10467-105">2.1 to 2.2</span></span>

<span data-ttu-id="10467-106">手動建立 `ServiceCollection` 並呼叫 `AddLogging` 。</span><span class="sxs-lookup"><span data-stu-id="10467-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="10467-107">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="10467-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="10467-108">2.2 範例：</span><span class="sxs-lookup"><span data-stu-id="10467-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="10467-109">2.1 到3。0</span><span class="sxs-lookup"><span data-stu-id="10467-109">2.1 to 3.0</span></span>

<span data-ttu-id="10467-110">在3.0 中，請使用 `LoggingFactory.Create` 。</span><span class="sxs-lookup"><span data-stu-id="10467-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="10467-111">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="10467-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="10467-112">3.0 範例：</span><span class="sxs-lookup"><span data-stu-id="10467-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="10467-113">其他資源</span><span class="sxs-lookup"><span data-stu-id="10467-113">Additional resources</span></span>

* <span data-ttu-id="10467-114">[Microsoft Extensions]： [[主控台] NuGet 套件](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)。</span><span class="sxs-lookup"><span data-stu-id="10467-114">[Microsoft.Extensions.Logging.Console NuGet package](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/).</span></span>
* <xref:fundamentals/logging/index>
