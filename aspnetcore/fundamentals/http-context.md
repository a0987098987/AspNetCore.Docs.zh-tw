---
title: 存取 ASP.NET Core 中的 HttpContext
author: coderandhiker
description: 了解如何存取 ASP.NET Core 中的 HttpContext。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: fundamentals/httpcontext
ms.openlocfilehash: 8a7ee180380c42ea745c91b8e6a18c1baa820220
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658743"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="0d7d1-103">存取 ASP.NET Core 中的 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="0d7d1-104">ASP.NET核心應用`HttpContext`程序<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>訪問通過 介面及其<xref:Microsoft.AspNetCore.Http.HttpContextAccessor>預設實現。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-104">ASP.NET Core apps access `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span> <span data-ttu-id="0d7d1-105">只有當您需要存取服務內的 `HttpContext` 時，才需要使用 `IHttpContextAccessor`。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="0d7d1-106">從 Razor 頁面使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="0d7d1-107">剃刀頁<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel>公開<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext>屬性 :</span><span class="sxs-lookup"><span data-stu-id="0d7d1-107">The Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> exposes the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="0d7d1-108">從 Razor 檢視使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="0d7d1-109">Razor 檢視會透過檢視上的 [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) 屬性，直接公開 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) property on the view.</span></span> <span data-ttu-id="0d7d1-110">以下範例使用 Windows 認證認證索 Intranet 應用程式中的目前使用者名稱:</span><span class="sxs-lookup"><span data-stu-id="0d7d1-110">The following example retrieves the current username in an intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="0d7d1-111">從控制器使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="0d7d1-112">控制器會公開 [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) 屬性：</span><span class="sxs-lookup"><span data-stu-id="0d7d1-112">Controllers expose the [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;

        ...

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="0d7d1-113">從中介軟體使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="0d7d1-114">使用自訂中介軟體元件時，`HttpContext` 會傳入 `Invoke` 或 `InvokeAsync` 方法，並且可以在設定中介軟體時存取：</span><span class="sxs-lookup"><span data-stu-id="0d7d1-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="0d7d1-115">從自訂元件中使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="0d7d1-116">對於需要存取 `HttpContext` 的其他架構和自訂元件，建議的方法是使用內建的[相依性插入容器](xref:fundamentals/dependency-injection)來註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0d7d1-117">相依項注入`IHttpContextAccessor`容器 向任何將之聲明為建構函式的相依項的類別提供 :</span><span class="sxs-lookup"><span data-stu-id="0d7d1-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddControllersWithViews();
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="0d7d1-118">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="0d7d1-118">In the following example:</span></span>

* <span data-ttu-id="0d7d1-119">`UserRepository` 宣告其對 `IHttpContextAccessor` 的相依性。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="0d7d1-120">當相依性插入解析相依性鏈結並建立 `UserRepository` 的實例時，將提供相依性。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="0d7d1-121">從背景執行緒存取 HttpContext</span><span class="sxs-lookup"><span data-stu-id="0d7d1-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="0d7d1-122">`HttpContext`不是線程安全的。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-122">`HttpContext` isn't thread-safe.</span></span> <span data-ttu-id="0d7d1-123">在處理要求之外讀取或寫入 `HttpContext` 的屬性，可能會導致 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a <xref:System.NullReferenceException>.</span></span>

> [!NOTE]
> <span data-ttu-id="0d7d1-124">如果應用生成零星`NullReferenceException`錯誤,請查看啟動後台處理或請求完成後繼續處理的代碼的某些部分。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-124">If your app generates sporadic `NullReferenceException` errors, review parts of the code that start background processing or that continue processing after a request completes.</span></span> <span data-ttu-id="0d7d1-125">尋找錯誤,例如控制器方法定義為`async void`。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-125">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="0d7d1-126">使用 `HttpContext` 資料安全地執行背景工作：</span><span class="sxs-lookup"><span data-stu-id="0d7d1-126">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="0d7d1-127">在要求處理期間複製所需的資料。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-127">Copy the required data during request processing.</span></span>
* <span data-ttu-id="0d7d1-128">將複製的資料傳遞至背景工作。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-128">Pass the copied data to a background task.</span></span>

<span data-ttu-id="0d7d1-129">為了避免不安全的代碼,切勿將 轉換`HttpContext`為 執行後台工作的方法。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-129">To avoid unsafe code, never pass the `HttpContext` into a method that performs background work.</span></span> <span data-ttu-id="0d7d1-130">而是傳遞所需的數據。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-130">Pass the required data instead.</span></span> <span data-ttu-id="0d7d1-131">在下面的範例中,`SendEmailCore`呼叫 以開始發送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-131">In the following example, `SendEmailCore` is called to start sending an email.</span></span> <span data-ttu-id="0d7d1-132">傳遞給`correlationId``SendEmailCore`而不是`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="0d7d1-132">The `correlationId` is passed to `SendEmailCore`, not the `HttpContext`.</span></span> <span data-ttu-id="0d7d1-133">程式碼執行不會`SendEmailCore`等待 完成:</span><span class="sxs-lookup"><span data-stu-id="0d7d1-133">Code execution doesn't wait for `SendEmailCore` to complete:</span></span>

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        _ = SendEmailCore(correlationId);

        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        ...
    }
}
