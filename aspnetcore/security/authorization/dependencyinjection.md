---
title: ASP.NET Core 中的要求處理常式中的相依性插入
author: rick-anderson
description: 了解如何使用相依性插入將 ASP.NET Core 應用程式中插入授權要求處理常式。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896365"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core 中的要求處理常式中的相依性插入

<a name="security-authorization-di"></a>

[必須註冊授權處理常式](xref:security/authorization/policies#handler-registration)在設定期間的服務集合中 (使用[相依性插入](xref:fundamentals/dependency-injection))。

假設您擁有您想要評估授權處理常式內的規則的儲存機制和服務集合中註冊該存放庫。 授權會解決，並將之插入您建構函式。

例如，如果您想要使用 ASP。NET 的記錄基礎結構，您會想要插入`ILoggerFactory`到您的處理常式。 這類處理常式可能如下：

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

當您的應用程式啟動時，建立執行個體的處理常式將和插入的已註冊的 DI 會`ILoggerFactory`至您的建構函式。

> [!NOTE]
> 使用 Entity Framework 的處理常式不應該註冊為單一子句。
