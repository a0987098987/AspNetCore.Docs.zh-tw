---
title: 從 ClaimsPrincipal 遷移
author: mjrousos
description: 瞭解如何從 ClaimsPrincipal 遷移，以取得目前已驗證的使用者身分識別和 ASP.NET Core 中的宣告。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115963"
---
# <a name="migrate-from-claimsprincipalcurrent"></a><span data-ttu-id="8ad91-103">從 ClaimsPrincipal 遷移</span><span class="sxs-lookup"><span data-stu-id="8ad91-103">Migrate from ClaimsPrincipal.Current</span></span>

<span data-ttu-id="8ad91-104">在 ASP.NET 4.x 專案中，通常會使用[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal.current)來抓取目前已驗證使用者的身分識別和宣告。</span><span class="sxs-lookup"><span data-stu-id="8ad91-104">In ASP.NET 4.x projects, it was common to use [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) to retrieve the current authenticated user's identity and claims.</span></span> <span data-ttu-id="8ad91-105">在 ASP.NET Core 中，已不再設定此屬性。</span><span class="sxs-lookup"><span data-stu-id="8ad91-105">In ASP.NET Core, this property is no longer set.</span></span> <span data-ttu-id="8ad91-106">必須更新程式碼，以透過不同的方法來取得目前已驗證的使用者身分識別。</span><span class="sxs-lookup"><span data-stu-id="8ad91-106">Code that was depending on it needs to be updated to get the current authenticated user's identity through a different means.</span></span>

## <a name="context-specific-data-instead-of-static-data"></a><span data-ttu-id="8ad91-107">內容特定的資料，而不是靜態資料</span><span class="sxs-lookup"><span data-stu-id="8ad91-107">Context-specific data instead of static data</span></span>

<span data-ttu-id="8ad91-108">使用 ASP.NET Core 時，不會同時設定 `ClaimsPrincipal.Current` 和 `Thread.CurrentPrincipal` 的值。</span><span class="sxs-lookup"><span data-stu-id="8ad91-108">When using ASP.NET Core, the values of both `ClaimsPrincipal.Current` and `Thread.CurrentPrincipal` aren't set.</span></span> <span data-ttu-id="8ad91-109">這些屬性都代表靜態狀態，ASP.NET Core 通常會避免這種情況。</span><span class="sxs-lookup"><span data-stu-id="8ad91-109">These properties both represent static state, which ASP.NET Core generally avoids.</span></span> <span data-ttu-id="8ad91-110">相反地，ASP.NET Core 的架構是從內容特定服務集合（使用其相依性[插入](xref:fundamentals/dependency-injection)（DI）模型）抓取相依性（例如目前使用者的身分識別）。</span><span class="sxs-lookup"><span data-stu-id="8ad91-110">Instead, ASP.NET Core's architecture is to retrieve dependencies (like the current user's identity) from context-specific service collections (using its [dependency injection](xref:fundamentals/dependency-injection) (DI) model).</span></span> <span data-ttu-id="8ad91-111">此外，`Thread.CurrentPrincipal` 是執行緒靜態的，因此它可能不會在某些非同步案例中保存變更（而且 `ClaimsPrincipal.Current` 預設只會呼叫 `Thread.CurrentPrincipal`）。</span><span class="sxs-lookup"><span data-stu-id="8ad91-111">What's more, `Thread.CurrentPrincipal` is thread static, so it may not persist changes in some asynchronous scenarios (and `ClaimsPrincipal.Current` just calls `Thread.CurrentPrincipal` by default).</span></span>

<span data-ttu-id="8ad91-112">若要瞭解執行緒靜態成員在非同步案例中可能會導致的問題，請考慮下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="8ad91-112">To understand the sorts of problems thread static members can lead to in asynchronous scenarios, consider the following code snippet:</span></span>

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

<span data-ttu-id="8ad91-113">上述範例程式碼會設定 `Thread.CurrentPrincipal`，並在等候非同步呼叫之前和之後檢查其值。</span><span class="sxs-lookup"><span data-stu-id="8ad91-113">The preceding sample code sets `Thread.CurrentPrincipal` and checks its value before and after awaiting an asynchronous call.</span></span> <span data-ttu-id="8ad91-114">`Thread.CurrentPrincipal` 專屬於其設定所在的*執行緒*，而方法可能會在 await 之後繼續在不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="8ad91-114">`Thread.CurrentPrincipal` is specific to the *thread* on which it's set, and the method is likely to resume execution on a different thread after the await.</span></span> <span data-ttu-id="8ad91-115">因此，在第一次檢查時，`Thread.CurrentPrincipal` 會存在，但在呼叫 `await Task.Yield()`之後，會是 null。</span><span class="sxs-lookup"><span data-stu-id="8ad91-115">Consequently, `Thread.CurrentPrincipal` is present when it's first checked but is null after the call to `await Task.Yield()`.</span></span>

<span data-ttu-id="8ad91-116">從應用程式的 DI 服務集合取得目前使用者的身分識別，也會更具測試性，因為可以輕鬆地插入測試身分識別。</span><span class="sxs-lookup"><span data-stu-id="8ad91-116">Getting the current user's identity from the app's DI service collection is more testable, too, since test identities can be easily injected.</span></span>

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a><span data-ttu-id="8ad91-117">在 ASP.NET Core 應用程式中取出目前的使用者</span><span class="sxs-lookup"><span data-stu-id="8ad91-117">Retrieve the current user in an ASP.NET Core app</span></span>

<span data-ttu-id="8ad91-118">有數個選項可讓您在 ASP.NET Core 中抓取目前已驗證的使用者 `ClaimsPrincipal`，以取代 `ClaimsPrincipal.Current`：</span><span class="sxs-lookup"><span data-stu-id="8ad91-118">There are several options for retrieving the current authenticated user's `ClaimsPrincipal` in ASP.NET Core in place of `ClaimsPrincipal.Current`:</span></span>

* <span data-ttu-id="8ad91-119">**ControllerBase. User**。</span><span class="sxs-lookup"><span data-stu-id="8ad91-119">**ControllerBase.User**.</span></span> <span data-ttu-id="8ad91-120">MVC 控制器可以使用其[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性來存取目前已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="8ad91-120">MVC controllers can access the current authenticated user with their [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) property.</span></span>
* <span data-ttu-id="8ad91-121">**HttpCoNtext**。</span><span class="sxs-lookup"><span data-stu-id="8ad91-121">**HttpContext.User**.</span></span> <span data-ttu-id="8ad91-122">具有目前 `HttpContext` （例如中介軟體）存取權的元件，可以從[HttpCoNtext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)取得目前使用者的 `ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="8ad91-122">Components with access to the current `HttpContext` (middleware, for example) can get the current user's `ClaimsPrincipal` from [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).</span></span>
* <span data-ttu-id="8ad91-123">**從呼叫端傳入**。</span><span class="sxs-lookup"><span data-stu-id="8ad91-123">**Passed in from caller**.</span></span> <span data-ttu-id="8ad91-124">不需要存取目前 `HttpContext` 的程式庫通常是從控制器或中介軟體元件呼叫，而且可以將目前使用者的身分識別當做引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="8ad91-124">Libraries without access to the current `HttpContext` are often called from controllers or middleware components and can have the current user's identity passed as an argument.</span></span>
* <span data-ttu-id="8ad91-125">**IHttpCoNtextAccessor**。</span><span class="sxs-lookup"><span data-stu-id="8ad91-125">**IHttpContextAccessor**.</span></span> <span data-ttu-id="8ad91-126">要遷移至 ASP.NET Core 的專案可能太大，而無法輕鬆地將目前使用者的身分識別傳遞至所有必要的位置。</span><span class="sxs-lookup"><span data-stu-id="8ad91-126">The project being migrated to ASP.NET Core may be too large to easily pass the current user's identity to all necessary locations.</span></span> <span data-ttu-id="8ad91-127">在這種情況下，您可以使用[IHttpCoNtextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)作為因應措施。</span><span class="sxs-lookup"><span data-stu-id="8ad91-127">In such cases, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) can be used as a workaround.</span></span> <span data-ttu-id="8ad91-128">`IHttpContextAccessor` 可以存取目前的 `HttpContext` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="8ad91-128">`IHttpContextAccessor` is able to access the current `HttpContext` (if one exists).</span></span> <span data-ttu-id="8ad91-129">如果正在使用 DI，請參閱 <xref:fundamentals/httpcontext>。</span><span class="sxs-lookup"><span data-stu-id="8ad91-129">If DI is being used, see <xref:fundamentals/httpcontext>.</span></span> <span data-ttu-id="8ad91-130">在程式碼中取得目前使用者的身分識別，但尚未更新為使用 ASP.NET Core 的 DI 驅動架構的短期解決方案會是：</span><span class="sxs-lookup"><span data-stu-id="8ad91-130">A short-term solution to getting the current user's identity in code that hasn't yet been updated to work with ASP.NET Core's DI-driven architecture would be:</span></span>

  * <span data-ttu-id="8ad91-131">藉由呼叫 `Startup.ConfigureServices`中的[AddHttpCoNtextAccessor](https://github.com/aspnet/Hosting/issues/793) ，讓 DI 容器中的 `IHttpContextAccessor` 可供使用。</span><span class="sxs-lookup"><span data-stu-id="8ad91-131">Make `IHttpContextAccessor` available in the DI container by calling [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.</span></span>
  * <span data-ttu-id="8ad91-132">在啟動期間取得 `IHttpContextAccessor` 的實例，並將它儲存在靜態變數中。</span><span class="sxs-lookup"><span data-stu-id="8ad91-132">Get an instance of `IHttpContextAccessor` during startup and store it in a static variable.</span></span> <span data-ttu-id="8ad91-133">實例可供先前從靜態屬性取得目前使用者的程式碼使用。</span><span class="sxs-lookup"><span data-stu-id="8ad91-133">The instance is made available to code that was previously retrieving the current user from a static property.</span></span>
  * <span data-ttu-id="8ad91-134">使用 `HttpContextAccessor.HttpContext?.User`取出目前使用者的 `ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="8ad91-134">Retrieve the current user's `ClaimsPrincipal` using `HttpContextAccessor.HttpContext?.User`.</span></span> <span data-ttu-id="8ad91-135">如果在 HTTP 要求的內容外部使用此程式碼，則 `HttpContext` 為 null。</span><span class="sxs-lookup"><span data-stu-id="8ad91-135">If this code is used outside of the context of an HTTP request, the `HttpContext` is null.</span></span>

<span data-ttu-id="8ad91-136">最後一個選項是使用儲存在靜態變數中的 `IHttpContextAccessor` 實例，就跟 ASP.NET Core 原則（偏好插入靜態相依性的相依性）相反。</span><span class="sxs-lookup"><span data-stu-id="8ad91-136">The final option, using an `IHttpContextAccessor` instance stored in a static variable, is contrary to ASP.NET Core principles (preferring injected dependencies to static dependencies).</span></span> <span data-ttu-id="8ad91-137">請改為根據相依性插入，規劃最終取出 `IHttpContextAccessor` 實例。</span><span class="sxs-lookup"><span data-stu-id="8ad91-137">Plan to eventually retrieve `IHttpContextAccessor` instances from dependency injection instead.</span></span> <span data-ttu-id="8ad91-138">不過，靜態協助程式可以是很有用的橋接器，但在遷移先前使用 `ClaimsPrincipal.Current`的大型現有 ASP.NET 應用程式時。</span><span class="sxs-lookup"><span data-stu-id="8ad91-138">A static helper can be a useful bridge, though, when migrating large existing ASP.NET apps that were previously using `ClaimsPrincipal.Current`.</span></span>
