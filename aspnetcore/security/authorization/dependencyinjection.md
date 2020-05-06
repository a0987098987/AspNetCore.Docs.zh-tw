---
title: ASP.NET Core 中的需求處理常式中的相依性插入
author: rick-anderson
description: 瞭解如何使用相依性插入將授權需求處理常式插入 ASP.NET Core 應用程式中。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 16285f6f731455d6e45a04f82437793891a77668
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775116"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core 中的需求處理常式中的相依性插入

<a name="security-authorization-di"></a>

在設定期間，必須在服務集合中[註冊授權處理常式](xref:security/authorization/policies#handler-registration)（使用相依性[插入](xref:fundamentals/dependency-injection)）。

假設您有想要在授權處理常式中評估的規則存放庫，而且該儲存機制已在服務集合中註冊。 授權將會解析，並將其插入您的函式。

例如，如果您想要使用 ASP。您想要插入`ILoggerFactory`處理常式中的網路記錄基礎結構。 這類處理常式看起來可能像這樣：

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

您會向註冊處理常式`services.AddSingleton()`：

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

當您的應用程式啟動時，將會建立處理常式的實例，而 DI 會`ILoggerFactory`將註冊的插入您的函式中。

> [!NOTE]
> 使用 Entity Framework 的處理常式不應該註冊為單次個體。
