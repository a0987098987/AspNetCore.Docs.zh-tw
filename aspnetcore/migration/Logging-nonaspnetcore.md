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
ms.openlocfilehash: 3a84d53cb925a518f6c3e244dd342a3228a1fe17
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777055"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="6f3d7-103">從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0</span><span class="sxs-lookup"><span data-stu-id="6f3d7-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="6f3d7-104">本文概述將使用`Microsoft.Extensions.Logging`從2.1 到2.2 或3.0 的 non-ASP.NET Core 應用程式遷移的一般步驟。</span><span class="sxs-lookup"><span data-stu-id="6f3d7-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="6f3d7-105">2.1 至 2.2</span><span class="sxs-lookup"><span data-stu-id="6f3d7-105">2.1 to 2.2</span></span>

<span data-ttu-id="6f3d7-106">手動建立`ServiceCollection`並呼叫`AddLogging`。</span><span class="sxs-lookup"><span data-stu-id="6f3d7-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="6f3d7-107">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="6f3d7-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="6f3d7-108">2.2 範例：</span><span class="sxs-lookup"><span data-stu-id="6f3d7-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="6f3d7-109">2.1 到3。0</span><span class="sxs-lookup"><span data-stu-id="6f3d7-109">2.1 to 3.0</span></span>

<span data-ttu-id="6f3d7-110">在3.0 中， `LoggingFactory.Create`請使用。</span><span class="sxs-lookup"><span data-stu-id="6f3d7-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="6f3d7-111">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="6f3d7-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="6f3d7-112">3.0 範例：</span><span class="sxs-lookup"><span data-stu-id="6f3d7-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="6f3d7-113">其他資源</span><span class="sxs-lookup"><span data-stu-id="6f3d7-113">Additional resources</span></span>

<xref:fundamentals/logging/index>