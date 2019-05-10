---
title: 從 Microsoft.Extensions.Logging 2.1 至 2.2 或 3.0 移轉
author: pakrym
description: 了解如何移轉使用 Microsoft.Extensions.Logging 2.1 2.2 或 3.0 非 ASP.NET Core 應用程式。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892455"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>從 Microsoft.Extensions.Logging 2.1 至 2.2 或 3.0 移轉

本文概述使用非 ASP.NET Core 應用程式移轉的常見步驟`Microsoft.Extensions.Logging`2.1 2.2 或 3.0。

## <a name="21-to-22"></a>2.1 至 2.2

手動建立`ServiceCollection`並呼叫`AddLogging`。

2.1 的範例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2.2 的範例：

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 到 3.0

在 3.0 中，使用`LoggingFactory.Create`。

2.1 的範例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3.0 版的範例：

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>其他資源

<xref:fundamentals/logging/index>