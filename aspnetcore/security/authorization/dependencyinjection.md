---
title: "在要求處理常式中的相依性插入"
author: rick-anderson
description: "本文件概述如何使用相依性插入的 ASP.NET Core 應用程式中插入授權需求的處理常式。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 1b7506b49109264a8c628ea2e39ded9f5ace95d3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a>在要求處理常式中的相依性插入

<a name="security-authorization-di"></a>

[授權的處理常式必須註冊](policies.md#handler-registration)在設定期間的服務集合中 (使用[相依性插入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection))。

假設您有想要評估之授權的處理常式內部的規則的儲存機制，該儲存機制已登錄在服務集合。 授權會解決，然後將之插入您建構函式。

例如，如果您想要使用 ASP。網路的記錄基礎結構，您會想要插入`ILoggerFactory`到您的處理常式。 這類處理常式可能如下：

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

您會註冊處理常式和`services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

執行個體的處理常式將您的應用程式啟動時，建立並插入已註冊的 DI 將`ILoggerFactory`到建構函式。

> [!NOTE]
> 使用 Entity Framework 的處理常式應該不會註冊為 singleton。
