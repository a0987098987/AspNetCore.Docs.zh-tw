---
title: 從 ClaimsPrincipal.Current 移轉
author: mjrousos
description: 了解如何將從要擷取目前已驗證的使用者的身分識別與 ASP.NET Core 中的宣告的 ClaimsPrincipal.Current 轉移。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 477f9fe8f0249bdfc528e1b401f072851f2f0d23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277182"
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="34812-103">從 ClaimsPrincipal.Current 移轉</span><span class="sxs-lookup"><span data-stu-id="34812-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="34812-104">在 ASP.NET 專案中，已使用[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)擷取目前驗證使用者的身分識別和宣告。</span><span class="sxs-lookup"><span data-stu-id="34812-104">In ASP.NET projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="34812-105">在 ASP.NET Core 無法再設定這個屬性。</span><span class="sxs-lookup"><span data-stu-id="34812-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="34812-106">取決於它的程式碼需要更新，以取得目前已驗證的使用者的身分識別，透過不同的方式。</span><span class="sxs-lookup"><span data-stu-id="34812-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="34812-107">特定內容的資料，而非靜態資料</span><span class="sxs-lookup"><span data-stu-id="34812-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="34812-108">使用 ASP.NET Core，兩者的值時`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`未設定。</span><span class="sxs-lookup"><span data-stu-id="34812-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="34812-109">這些屬性都代表靜態狀態，通常可以避免 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="34812-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="34812-110">相反地，ASP.NET Core 架構是從特定內容的服務集合擷取相依性 （例如目前的使用者身分識別） (使用其[相依性插入](xref:fundamentals/dependency-injection)(DI) 模型)。</span><span class="sxs-lookup"><span data-stu-id="34812-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="34812-111">什麼是多個，`Thread.CurrentPrincipal`是執行緒靜態的因此它可能不是保存在某些非同步案例中的變更 (和`ClaimsPrincipal.Current`只會呼叫`Thread.CurrentPrincipal`依預設)。</span><span class="sxs-lookup"><span data-stu-id="34812-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="34812-112">若要了解的各種問題執行緒靜態成員可能導致在非同步案例中，請考慮下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="34812-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

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

<span data-ttu-id="34812-113">上述範例程式碼集`Thread.CurrentPrincipal`，並檢查其值之前和之後等候非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="34812-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="34812-114">`Thread.CurrentPrincipal` 特定*執行緒*在其上設定，而方法可能繼續的 await 之後不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="34812-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="34812-115">因此，`Thread.CurrentPrincipal`時的存在，當第一次檢查，但在呼叫之後為 null `await Task.Yield()`。</span><span class="sxs-lookup"><span data-stu-id="34812-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="34812-116">取得目前使用者的身分識別，從應用程式的 DI 服務集合太多測試，因為測試身分識別可輕鬆地插入。</span><span class="sxs-lookup"><span data-stu-id="34812-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="34812-117">擷取目前使用者的 ASP.NET Core 應用程式中</span><span class="sxs-lookup"><span data-stu-id="34812-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="34812-118">有幾個選項來擷取目前已驗證的使用者的`ClaimsPrincipal`中取代 ASP.NET Core `ClaimsPrincipal.Current`:</span><span class="sxs-lookup"><span data-stu-id="34812-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="34812-119">**ControllerBase.User**。</span><span class="sxs-lookup"><span data-stu-id="34812-119">**ControllerBase.User**.</span></span> <span data-ttu-id="34812-120">MVC 控制站可以存取與目前已驗證的使用者其[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性。</span><span class="sxs-lookup"><span data-stu-id="34812-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="34812-121">**HttpContext.User**。</span><span class="sxs-lookup"><span data-stu-id="34812-121">**HttpContext.User**.</span></span> <span data-ttu-id="34812-122">元件的存取權目前`HttpContext`（中的介軟體，例如） 可以取得目前使用者的`ClaimsPrincipal`從[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)。</span><span class="sxs-lookup"><span data-stu-id="34812-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="34812-123">**傳入呼叫端從**。</span><span class="sxs-lookup"><span data-stu-id="34812-123">**Passed in from caller**.</span></span> <span data-ttu-id="34812-124">如果沒有為目前的權限的程式庫`HttpContext`通常稱為從控制器或中介軟體元件，且可以有目前使用者的身分識別做為引數。</span><span class="sxs-lookup"><span data-stu-id="34812-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="34812-125">**IHttpContextAccessor**。</span><span class="sxs-lookup"><span data-stu-id="34812-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="34812-126">在移轉至 ASP.NET Core ASP.NET 專案可能太大，無法輕鬆地將目前的使用者識別傳遞給所有必要的位置。</span><span class="sxs-lookup"><span data-stu-id="34812-126">The ASP.NET project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="34812-127">在這種情況下， [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)可用來當做因應措施。</span><span class="sxs-lookup"><span data-stu-id="34812-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="34812-128">`IHttpContextAccessor` 可以存取目前`HttpContext`（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="34812-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="34812-129">若要在尚未尚未更新為使用 ASP.NET Core DI 驅動的架構的程式碼中取得目前使用者的身分識別的短期方案是：</span><span class="sxs-lookup"><span data-stu-id="34812-129">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="34812-130">請`IHttpContextAccessor`藉由呼叫 DI 容器中[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="34812-130">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="34812-131">取得的執行個體`IHttpContextAccessor`在啟動期間並將它儲存在靜態變數中。</span><span class="sxs-lookup"><span data-stu-id="34812-131">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="34812-132">執行個體才可以提供給先前已從靜態屬性擷取目前使用者的程式碼。</span><span class="sxs-lookup"><span data-stu-id="34812-132">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="34812-133">擷取目前使用者的`ClaimsPrincipal`使用`HttpContextAccessor.HttpContext?.User`。</span><span class="sxs-lookup"><span data-stu-id="34812-133">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="34812-134">如果此程式碼使用的 HTTP 要求，環境之外`HttpContext`為 null。</span><span class="sxs-lookup"><span data-stu-id="34812-134">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="34812-135">最終選項，使用`IHttpContextAccessor`，違反 （靜態相依性偏好插入相依性） 的 ASP.NET 核心原則。</span><span class="sxs-lookup"><span data-stu-id="34812-135">The final option, using `IHttpContextAccessor`, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="34812-136">打算最後移除的相依性靜態`IHttpContextAccessor`協助程式。</span><span class="sxs-lookup"><span data-stu-id="34812-136">Plan to eventually remove the dependency on the static `IHttpContextAccessor` helper.</span></span> <span data-ttu-id="34812-137">就很有用的橋接器，不過，移轉大型的現有 ASP.NET 應用程式先前使用`ClaimsPrincipal.Current`。</span><span class="sxs-lookup"><span data-stu-id="34812-137">It can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>
