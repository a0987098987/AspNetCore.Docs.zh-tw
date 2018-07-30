---
title: ASP.NET Core 中的要求處理常式中的相依性插入
author: rick-anderson
description: 了解如何使用相依性插入將 ASP.NET Core 應用程式中插入授權要求處理常式。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342110"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="8614c-103">ASP.NET Core 中的要求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="8614c-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="8614c-104">[必須註冊授權處理常式](xref:security/authorization/policies#handler-registration)在設定期間的服務集合中 (使用[相依性插入](xref:fundamentals/dependency-injection))。</span><span class="sxs-lookup"><span data-stu-id="8614c-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="8614c-105">假設您擁有您想要評估授權處理常式內的規則的儲存機制和服務集合中註冊該存放庫。</span><span class="sxs-lookup"><span data-stu-id="8614c-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="8614c-106">授權會解決，並將之插入您建構函式。</span><span class="sxs-lookup"><span data-stu-id="8614c-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="8614c-107">例如，如果您想要使用 ASP。NET 的記錄基礎結構，您會想要插入`ILoggerFactory`到您的處理常式。</span><span class="sxs-lookup"><span data-stu-id="8614c-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="8614c-108">這類處理常式可能如下：</span><span class="sxs-lookup"><span data-stu-id="8614c-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="8614c-109">您會註冊處理常式和`services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="8614c-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="8614c-110">當您的應用程式啟動時，建立執行個體的處理常式將和插入的已註冊的 DI 會`ILoggerFactory`至您的建構函式。</span><span class="sxs-lookup"><span data-stu-id="8614c-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="8614c-111">使用 Entity Framework 的處理常式不應該註冊為單一子句。</span><span class="sxs-lookup"><span data-stu-id="8614c-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
