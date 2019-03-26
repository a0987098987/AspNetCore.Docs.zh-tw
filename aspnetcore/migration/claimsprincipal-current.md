---
title: 移轉 ClaimsPrincipal.Current 從
author: mjrousos
description: 了解如何將從要擷取目前已驗證的使用者的身分識別與 ASP.NET Core 中的宣告的 ClaimsPrincipal.Current 轉移。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: 526cc3cf3a58a656e2a1b162cfaccacc7694dc51
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488638"
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="690e7-103">移轉 ClaimsPrincipal.Current 從</span><span class="sxs-lookup"><span data-stu-id="690e7-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="690e7-104">在 ASP.NET 4.x 專案中，已使用通用[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)若要擷取目前已驗證使用者的身分識別和宣告。</span><span class="sxs-lookup"><span data-stu-id="690e7-104">In ASP.NET 4.x projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="690e7-105">在 ASP.NET Core 無法再設定這個屬性。</span><span class="sxs-lookup"><span data-stu-id="690e7-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="690e7-106">根據它的程式碼需要更新，以取得目前已驗證的使用者的身分識別，透過不同的方式。</span><span class="sxs-lookup"><span data-stu-id="690e7-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="690e7-107">特定內容的資料，而不是靜態的資料</span><span class="sxs-lookup"><span data-stu-id="690e7-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="690e7-108">使用 ASP.NET Core，兩者的值時`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`未設定。</span><span class="sxs-lookup"><span data-stu-id="690e7-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="690e7-109">這些屬性都代表靜態狀態，通常可以避免 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="690e7-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="690e7-110">相反地，ASP.NET Core 架構是從特定內容的服務集合擷取相依性 （例如目前的使用者身分識別） (使用其[相依性插入](xref:fundamentals/dependency-injection)(DI) 模型)。</span><span class="sxs-lookup"><span data-stu-id="690e7-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="690e7-111">什麼是等等`Thread.CurrentPrincipal`是執行緒靜態的因此它可能不會保存在某些非同步案例中的變更 (和`ClaimsPrincipal.Current`只會呼叫`Thread.CurrentPrincipal`預設)。</span><span class="sxs-lookup"><span data-stu-id="690e7-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="690e7-112">若要了解的各種問題執行緒靜態成員可能導致在非同步的情況下，請考慮下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="690e7-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

<span data-ttu-id="690e7-113">上述的範例程式碼集`Thread.CurrentPrincipal`並檢查其值之前和之後等候的非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="690e7-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="690e7-114">`Thread.CurrentPrincipal` 特有*執行緒*在其上設定，而方法為可能會繼續執行不同的執行緒上的 await 之後。</span><span class="sxs-lookup"><span data-stu-id="690e7-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="690e7-115">因此，`Thread.CurrentPrincipal`存在時它會先檢查，但在呼叫之後為 null `await Task.Yield()`。</span><span class="sxs-lookup"><span data-stu-id="690e7-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="690e7-116">從應用程式的 DI 服務集合中取得目前使用者的身分識別太多測試，因為可以輕鬆地插入測試身分識別。</span><span class="sxs-lookup"><span data-stu-id="690e7-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="690e7-117">擷取目前使用者的 ASP.NET Core 應用程式中</span><span class="sxs-lookup"><span data-stu-id="690e7-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="690e7-118">有幾個選項來擷取目前已驗證的使用者的`ClaimsPrincipal`中取代 ASP.NET Core `ClaimsPrincipal.Current`:</span><span class="sxs-lookup"><span data-stu-id="690e7-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="690e7-119">**ControllerBase.User**。</span><span class="sxs-lookup"><span data-stu-id="690e7-119">**ControllerBase.User**.</span></span> <span data-ttu-id="690e7-120">MVC 控制站可以存取目前已驗證的使用者與他們[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性。</span><span class="sxs-lookup"><span data-stu-id="690e7-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="690e7-121">**HttpContext.User**。</span><span class="sxs-lookup"><span data-stu-id="690e7-121">**HttpContext.User**.</span></span> <span data-ttu-id="690e7-122">存取目前的元件`HttpContext`（例如中的介軟體） 可以取得目前的使用者`ClaimsPrincipal`從[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)。</span><span class="sxs-lookup"><span data-stu-id="690e7-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="690e7-123">**從呼叫端傳入**。</span><span class="sxs-lookup"><span data-stu-id="690e7-123">**Passed in from caller**.</span></span> <span data-ttu-id="690e7-124">目前無法存取的程式庫`HttpContext`通常稱為從控制器或中介軟體元件，且可以做為引數傳遞的目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="690e7-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="690e7-125">**IHttpContextAccessor**。</span><span class="sxs-lookup"><span data-stu-id="690e7-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="690e7-126">正在移轉至 ASP.NET Core 專案可能太大，無法輕鬆地將目前使用者的身分識別傳遞給所有必要的位置。</span><span class="sxs-lookup"><span data-stu-id="690e7-126">The project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="690e7-127">在此情況下， [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)可用因應措施。</span><span class="sxs-lookup"><span data-stu-id="690e7-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="690e7-128">`IHttpContextAccessor` 能夠存取目前`HttpContext`（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="690e7-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="690e7-129">若要在尚未尚未更新為使用 ASP.NET Core DI 驅動的架構的程式碼中取得目前使用者的身分識別的短期方案是：</span><span class="sxs-lookup"><span data-stu-id="690e7-129">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="690e7-130">製作`IHttpContextAccessor`DI 容器藉由呼叫中[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)在`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="690e7-130">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="690e7-131">取得的執行個體`IHttpContextAccessor`在啟動期間並將它儲存到靜態變數中。</span><span class="sxs-lookup"><span data-stu-id="690e7-131">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="690e7-132">執行個體是開放給先前已從靜態屬性擷取目前使用者的程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="690e7-132">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="690e7-133">擷取目前的使用者`ClaimsPrincipal`使用`HttpContextAccessor.HttpContext?.User`。</span><span class="sxs-lookup"><span data-stu-id="690e7-133">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="690e7-134">如果這個程式碼會使用 HTTP 要求的內容之外`HttpContext`為 null。</span><span class="sxs-lookup"><span data-stu-id="690e7-134">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="690e7-135">最後一個選項，使用`IHttpContextAccessor`執行個體儲存在靜態變數，是 ASP.NET Core 的原則 （優先靜態的相依性插入的相依性） 相反。</span><span class="sxs-lookup"><span data-stu-id="690e7-135">The final option, using an `IHttpContextAccessor` instance stored in a static variable, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="690e7-136">打算最終擷取`IHttpContextAccessor`改為從相依性插入執行個體。</span><span class="sxs-lookup"><span data-stu-id="690e7-136">Plan to eventually retrieve `IHttpContextAccessor` instances from dependency injection instead.</span></span> <span data-ttu-id="690e7-137">靜態協助程式時，可能會很有用的橋接器，不過，移轉大型的現有 ASP.NET 應用程式先前使用`ClaimsPrincipal.Current`。</span><span class="sxs-lookup"><span data-stu-id="690e7-137">A static helper can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>
