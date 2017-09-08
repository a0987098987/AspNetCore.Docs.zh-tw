---
title: "在要求處理常式中的相依性插入"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 37d197d7696a6e91fa236b2defc577959c95c49f
ms.sourcegitcommit: 0a70706a3814d2684f3ff96095d1e8291d559cc7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a>在要求處理常式中的相依性插入

<a name=security-authorization-di></a>

[授權的處理常式必須註冊](policies.md#security-authorization-policies-based-handler-registration)在設定期間的服務集合中 (使用[相依性插入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection))。

假設您有想要評估之授權的處理常式內部的規則的儲存機制，該儲存機制已登錄在服務集合。  授權會解決，然後將之插入您建構函式。

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
