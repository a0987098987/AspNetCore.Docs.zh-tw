---
title: 從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0
author: pakrym
description: 瞭解如何遷移使用 Microsoft Extensions 的 non-ASP.NET Core 應用程式。從2.1 到2.2 或3.0 的記錄。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659317"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="b410b-103">從 Microsoft 的副檔名遷移。記錄2.1 到2.2 或3。0</span><span class="sxs-lookup"><span data-stu-id="b410b-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="b410b-104">本文概述將使用 `Microsoft.Extensions.Logging` 從2.1 到2.2 或3.0 的 non-ASP.NET Core 應用程式遷移的一般步驟。</span><span class="sxs-lookup"><span data-stu-id="b410b-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="b410b-105">2.1 至 2.2</span><span class="sxs-lookup"><span data-stu-id="b410b-105">2.1 to 2.2</span></span>

<span data-ttu-id="b410b-106">手動建立 `ServiceCollection` 並呼叫 `AddLogging`。</span><span class="sxs-lookup"><span data-stu-id="b410b-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="b410b-107">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="b410b-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="b410b-108">2.2 範例：</span><span class="sxs-lookup"><span data-stu-id="b410b-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="b410b-109">2.1 到3。0</span><span class="sxs-lookup"><span data-stu-id="b410b-109">2.1 to 3.0</span></span>

<span data-ttu-id="b410b-110">在3.0 中，使用 `LoggingFactory.Create`。</span><span class="sxs-lookup"><span data-stu-id="b410b-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="b410b-111">2.1 範例：</span><span class="sxs-lookup"><span data-stu-id="b410b-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="b410b-112">3.0 範例：</span><span class="sxs-lookup"><span data-stu-id="b410b-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="b410b-113">其他資源</span><span class="sxs-lookup"><span data-stu-id="b410b-113">Additional resources</span></span>

<xref:fundamentals/logging/index>