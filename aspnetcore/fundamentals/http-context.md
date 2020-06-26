---
title: 存取 ASP.NET Core 中的 HttpContext
author: coderandhiker
description: 了解如何存取 ASP.NET Core 中的 HttpContext。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/5/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/httpcontext
ms.openlocfilehash: d4512c9fa136e518fa0230c0cf9c607519eed6d8
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399448"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="238f2-103">存取 ASP.NET Core 中的 HttpContext</span><span class="sxs-lookup"><span data-stu-id="238f2-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="238f2-104">ASP.NET Core 應用程式會 `HttpContext` 透過 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 介面和其預設實作為存取權 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> 。</span><span class="sxs-lookup"><span data-stu-id="238f2-104">ASP.NET Core apps access `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span> <span data-ttu-id="238f2-105">只有當您需要存取服務內的 `HttpContext` 時，才需要使用 `IHttpContextAccessor`。</span><span class="sxs-lookup"><span data-stu-id="238f2-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="238f2-106">從頁面使用 HttpCoNtext Razor</span><span class="sxs-lookup"><span data-stu-id="238f2-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="238f2-107">這些 Razor 頁面會 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 公開 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> 屬性：</span><span class="sxs-lookup"><span data-stu-id="238f2-107">The Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> exposes the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="238f2-108">從視圖使用 HttpCoNtext Razor</span><span class="sxs-lookup"><span data-stu-id="238f2-108">Use HttpContext from a Razor view</span></span>

Razor<span data-ttu-id="238f2-109">views 會 `HttpContext` 直接透過 view 的[RazorPage 內容](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context)來公開。</span><span class="sxs-lookup"><span data-stu-id="238f2-109"> views expose the `HttpContext` directly via a [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) property on the view.</span></span> <span data-ttu-id="238f2-110">下列範例會使用 Windows 驗證來抓取內部網路應用程式中目前的使用者名稱：</span><span class="sxs-lookup"><span data-stu-id="238f2-110">The following example retrieves the current username in an intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="238f2-111">從控制器使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="238f2-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="238f2-112">控制器會公開 [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) 屬性：</span><span class="sxs-lookup"><span data-stu-id="238f2-112">Controllers expose the [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="238f2-113">從中介軟體使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="238f2-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="238f2-114">使用自訂中介軟體元件時，`HttpContext` 會傳入 `Invoke` 或 `InvokeAsync` 方法，並且可以在設定中介軟體時存取：</span><span class="sxs-lookup"><span data-stu-id="238f2-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="238f2-115">從自訂元件中使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="238f2-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="238f2-116">對於需要存取 `HttpContext` 的其他架構和自訂元件，建議的方法是使用內建的[相依性插入容器](xref:fundamentals/dependency-injection)來註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="238f2-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="238f2-117">相依性插入容器會提供 `IHttpContextAccessor` 給任何類別，將它宣告為其函式中的相依性：</span><span class="sxs-lookup"><span data-stu-id="238f2-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors:</span></span>

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

<span data-ttu-id="238f2-118">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="238f2-118">In the following example:</span></span>

* <span data-ttu-id="238f2-119">`UserRepository` 宣告其對 `IHttpContextAccessor` 的相依性。</span><span class="sxs-lookup"><span data-stu-id="238f2-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="238f2-120">當相依性插入解析相依性鏈結並建立 `UserRepository` 的實例時，將提供相依性。</span><span class="sxs-lookup"><span data-stu-id="238f2-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="238f2-121">從背景執行緒存取 HttpContext</span><span class="sxs-lookup"><span data-stu-id="238f2-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="238f2-122">`HttpContext`不是安全線程。</span><span class="sxs-lookup"><span data-stu-id="238f2-122">`HttpContext` isn't thread-safe.</span></span> <span data-ttu-id="238f2-123">在處理要求之外讀取或寫入 `HttpContext` 的屬性，可能會導致 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="238f2-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a <xref:System.NullReferenceException>.</span></span>

> [!NOTE]
> <span data-ttu-id="238f2-124">如果您的應用程式 `NullReferenceException` 會產生偶爾發生的錯誤，請檢查啟動背景處理或在要求完成後繼續處理的程式碼部分。</span><span class="sxs-lookup"><span data-stu-id="238f2-124">If your app generates sporadic `NullReferenceException` errors, review parts of the code that start background processing or that continue processing after a request completes.</span></span> <span data-ttu-id="238f2-125">尋找錯誤，例如將控制器方法定義為 `async void` 。</span><span class="sxs-lookup"><span data-stu-id="238f2-125">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="238f2-126">使用 `HttpContext` 資料安全地執行背景工作：</span><span class="sxs-lookup"><span data-stu-id="238f2-126">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="238f2-127">在要求處理期間複製所需的資料。</span><span class="sxs-lookup"><span data-stu-id="238f2-127">Copy the required data during request processing.</span></span>
* <span data-ttu-id="238f2-128">將複製的資料傳遞至背景工作。</span><span class="sxs-lookup"><span data-stu-id="238f2-128">Pass the copied data to a background task.</span></span>

<span data-ttu-id="238f2-129">若要避免不安全的程式碼，請勿將傳遞 `HttpContext` 至執行背景工作的方法。</span><span class="sxs-lookup"><span data-stu-id="238f2-129">To avoid unsafe code, never pass the `HttpContext` into a method that performs background work.</span></span> <span data-ttu-id="238f2-130">請改為傳遞所需的資料。</span><span class="sxs-lookup"><span data-stu-id="238f2-130">Pass the required data instead.</span></span> <span data-ttu-id="238f2-131">在下列範例中， `SendEmailCore` 會呼叫來開始傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="238f2-131">In the following example, `SendEmailCore` is called to start sending an email.</span></span> <span data-ttu-id="238f2-132">`correlationId`會傳遞至 `SendEmailCore` ，而不是 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="238f2-132">The `correlationId` is passed to `SendEmailCore`, not the `HttpContext`.</span></span> <span data-ttu-id="238f2-133">程式碼執行不會等待 `SendEmailCore` 完成：</span><span class="sxs-lookup"><span data-stu-id="238f2-133">Code execution doesn't wait for `SendEmailCore` to complete:</span></span>

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
```

## <a name="blazor-and-shared-state"></a>Blazor<span data-ttu-id="238f2-134">和共用狀態</span><span class="sxs-lookup"><span data-stu-id="238f2-134"> and shared state</span></span>

[!INCLUDE[](~/includes/blazor-security/blazor-shared-state.md)]
