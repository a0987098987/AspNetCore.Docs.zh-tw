---
title: 存取 ASP.NET Core 中的 HttpContext
author: coderandhiker
description: 了解如何存取 ASP.NET Core 中的 HttpContext。
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: b1ff80943db1788b465accd51c70a3c3a3462d5c
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202702"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="c76c9-103">存取 ASP.NET Core 中的 HttpContext</span><span class="sxs-lookup"><span data-stu-id="c76c9-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="c76c9-104">ASP.NET Core 應用程式透過 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) 介面與其預設實作 [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) 來存取 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="c76c9-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="c76c9-105">從 Razor 頁面使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="c76c9-105">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="c76c9-106">Razor 頁面 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 會公開 [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) 屬性：</span><span class="sxs-lookup"><span data-stu-id="c76c9-106">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="c76c9-107">從控制器使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="c76c9-107">Use HttpContext from a controller</span></span>

<span data-ttu-id="c76c9-108">控制器會公開 [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) 屬性：</span><span class="sxs-lookup"><span data-stu-id="c76c9-108">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="c76c9-109">從中介軟體使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="c76c9-109">Use HttpContext from middleware</span></span>

<span data-ttu-id="c76c9-110">使用自訂中介軟體元件時，`HttpContext` 會傳入 `Invoke` 或 `InvokeAsync` 方法，並且可以在設定中介軟體時存取：</span><span class="sxs-lookup"><span data-stu-id="c76c9-110">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="c76c9-111">從自訂元件中使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="c76c9-111">Use HttpContext from custom components</span></span>

<span data-ttu-id="c76c9-112">對於需要存取 `HttpContext` 的其他架構和自訂元件，建議的方法是使用內建的[相依性插入容器](xref:fundamentals/dependency-injection)來註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="c76c9-112">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="c76c9-113">相依性插入容器會將 `IHttpContextAccessor` 提供給在其建構函式中將其聲明為相依性的任何類別。</span><span class="sxs-lookup"><span data-stu-id="c76c9-113">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="c76c9-114">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="c76c9-114">In the preceding example:</span></span>

* <span data-ttu-id="c76c9-115">`UserRepository` 宣告其對 `IHttpContextAccessor` 的相依性。</span><span class="sxs-lookup"><span data-stu-id="c76c9-115">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="c76c9-116">當相依性插入解析相依性鏈結並建立 `UserRepository` 的實例時，將提供相依性。</span><span class="sxs-lookup"><span data-stu-id="c76c9-116">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
