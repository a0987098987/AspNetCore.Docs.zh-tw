---
title: 從 Microsoft.Extensions.Logging 2.1 至 2.2 或 3.0 移轉
author: pakrym
description: 了解如何移轉使用 Microsoft.Extensions.Logging 2.1 2.2 或 3.0 非 ASP.NET Core 應用程式。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099468"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="3ec84-103">從 Microsoft.Extensions.Logging 2.1 至 2.2 或 3.0 移轉</span><span class="sxs-lookup"><span data-stu-id="3ec84-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="3ec84-104">本文概述使用非 ASP.NET Core 應用程式移轉的常見步驟`Microsoft.Extensions.Logging`2.1 2.2 或 3.0。</span><span class="sxs-lookup"><span data-stu-id="3ec84-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="3ec84-105">2.1 至 2.2</span><span class="sxs-lookup"><span data-stu-id="3ec84-105">2.1 to 2.2</span></span>

<span data-ttu-id="3ec84-106">手動建立`ServiceCollection`並呼叫`AddLogging`。</span><span class="sxs-lookup"><span data-stu-id="3ec84-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="3ec84-107">2.1 的範例：</span><span class="sxs-lookup"><span data-stu-id="3ec84-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="3ec84-108">2.2 的範例：</span><span class="sxs-lookup"><span data-stu-id="3ec84-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="3ec84-109">2.1 到 3.0</span><span class="sxs-lookup"><span data-stu-id="3ec84-109">2.1 to 3.0</span></span>

<span data-ttu-id="3ec84-110">在 3.0 中，使用`LoggingFactory.Create`。</span><span class="sxs-lookup"><span data-stu-id="3ec84-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="3ec84-111">2.1 的範例：</span><span class="sxs-lookup"><span data-stu-id="3ec84-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="3ec84-112">3.0 版的範例：</span><span class="sxs-lookup"><span data-stu-id="3ec84-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="3ec84-113">其他資源</span><span class="sxs-lookup"><span data-stu-id="3ec84-113">Additional resources</span></span>

<xref:fundamentals/logging/index>