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
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="bca46-103">在要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="bca46-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="bca46-104">[授權的處理常式必須註冊](policies.md#handler-registration)在設定期間的服務集合中 (使用[相依性插入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection))。</span><span class="sxs-lookup"><span data-stu-id="bca46-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="bca46-105">假設您有想要評估之授權的處理常式內部的規則的儲存機制，該儲存機制已登錄在服務集合。</span><span class="sxs-lookup"><span data-stu-id="bca46-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="bca46-106">授權會解決，然後將之插入您建構函式。</span><span class="sxs-lookup"><span data-stu-id="bca46-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="bca46-107">例如，如果您想要使用 ASP。網路的記錄基礎結構，您會想要插入`ILoggerFactory`到您的處理常式。</span><span class="sxs-lookup"><span data-stu-id="bca46-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="bca46-108">這類處理常式可能如下：</span><span class="sxs-lookup"><span data-stu-id="bca46-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="bca46-109">您會註冊處理常式和`services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="bca46-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="bca46-110">執行個體的處理常式將您的應用程式啟動時，建立並插入已註冊的 DI 將`ILoggerFactory`到建構函式。</span><span class="sxs-lookup"><span data-stu-id="bca46-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="bca46-111">使用 Entity Framework 的處理常式應該不會註冊為 singleton。</span><span class="sxs-lookup"><span data-stu-id="bca46-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
