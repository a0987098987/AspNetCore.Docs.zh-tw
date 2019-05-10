---
title: 存取 ASP.NET Core 中的 HttpContext
author: coderandhiker
description: 了解如何存取 ASP.NET Core 中的 HttpContext。
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 373c036e0839ce51259e23f8503fbe4691b48751
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886513"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="d8f18-103">存取 ASP.NET Core 中的 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="d8f18-104">ASP.NET Core 應用程式透過 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) 介面與其預設實作 [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) 來存取 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="d8f18-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="d8f18-105">只有當您需要存取服務內的 `HttpContext` 時，才需要使用 `IHttpContextAccessor`。</span><span class="sxs-lookup"><span data-stu-id="d8f18-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="d8f18-106">從 Razor 頁面使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="d8f18-107">Razor 頁面 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 會公開 [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) 屬性：</span><span class="sxs-lookup"><span data-stu-id="d8f18-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="d8f18-108">從 Razor 檢視使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="d8f18-109">Razor 檢視會透過檢視上的 [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) 屬性，直接公開 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="d8f18-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="d8f18-110">以下範例會在內部網路應用程式中使用 Windows 驗證，擷取目前的使用者名稱：</span><span class="sxs-lookup"><span data-stu-id="d8f18-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="d8f18-111">從控制器使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="d8f18-112">控制器會公開 [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) 屬性：</span><span class="sxs-lookup"><span data-stu-id="d8f18-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

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

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="d8f18-113">從中介軟體使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="d8f18-114">使用自訂中介軟體元件時，`HttpContext` 會傳入 `Invoke` 或 `InvokeAsync` 方法，並且可以在設定中介軟體時存取：</span><span class="sxs-lookup"><span data-stu-id="d8f18-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="d8f18-115">從自訂元件中使用 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="d8f18-116">對於需要存取 `HttpContext` 的其他架構和自訂元件，建議的方法是使用內建的[相依性插入容器](xref:fundamentals/dependency-injection)來註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="d8f18-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d8f18-117">相依性插入容器會將 `IHttpContextAccessor` 提供給在其建構函式中將其聲明為相依性的任何類別。</span><span class="sxs-lookup"><span data-stu-id="d8f18-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

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

<span data-ttu-id="d8f18-118">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="d8f18-118">In the following example:</span></span>

* <span data-ttu-id="d8f18-119">`UserRepository` 宣告其對 `IHttpContextAccessor` 的相依性。</span><span class="sxs-lookup"><span data-stu-id="d8f18-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="d8f18-120">當相依性插入解析相依性鏈結並建立 `UserRepository` 的實例時，將提供相依性。</span><span class="sxs-lookup"><span data-stu-id="d8f18-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="d8f18-121">從背景執行緒存取 HttpContext</span><span class="sxs-lookup"><span data-stu-id="d8f18-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="d8f18-122">`HttpContext` 不是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="d8f18-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="d8f18-123">在處理要求之外讀取或寫入 `HttpContext` 的屬性，可能會導致 `NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="d8f18-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="d8f18-124">在處理要求之外使用 `HttpContext`，通常會導致 `NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="d8f18-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="d8f18-125">如果您的應用程式偶爾會產生 `NullReferenceException`，請檢閱啟動背景處理的程式碼部分，或在要求完成之後繼續處理的部分。</span><span class="sxs-lookup"><span data-stu-id="d8f18-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="d8f18-126">尋找錯誤，例如，將控制器方法定義為 `async void`。</span><span class="sxs-lookup"><span data-stu-id="d8f18-126">Look for a mistakes like defining a controller method as `async void`.</span></span>

<span data-ttu-id="d8f18-127">使用 `HttpContext` 資料安全地執行背景工作：</span><span class="sxs-lookup"><span data-stu-id="d8f18-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="d8f18-128">在要求處理期間複製所需的資料。</span><span class="sxs-lookup"><span data-stu-id="d8f18-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="d8f18-129">將複製的資料傳遞至背景工作。</span><span class="sxs-lookup"><span data-stu-id="d8f18-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="d8f18-130">為了避免不安全的程式碼，絕對不會將 `HttpContext` 傳遞至執行背景工作的方法，而是改為傳遞您所需的資料。</span><span class="sxs-lookup"><span data-stu-id="d8f18-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController
{
    public ActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}
