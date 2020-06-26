---
title: ASP.NET Core 中以角色為基礎的授權
author: rick-anderson
description: 瞭解如何藉由將角色傳遞至授權屬性來限制 ASP.NET Core 控制器和動作存取。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/roles
ms.openlocfilehash: 5d2ea6b9be0c993d62fa75fb8b471b5923747bac
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407859"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="e8c4b-103">ASP.NET Core 中以角色為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="e8c4b-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="e8c4b-104">建立身分識別時，它可能屬於一或多個角色。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="e8c4b-105">例如，Tracy 可能屬於系統管理員和使用者角色，而 Scott 只能屬於使用者角色。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="e8c4b-106">建立和管理這些角色的方式，取決於授權程式的備份存放區。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="e8c4b-107">角色會透過[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)類別上的[IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)方法向開發人員公開。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="e8c4b-108">新增角色檢查</span><span class="sxs-lookup"><span data-stu-id="e8c4b-108">Adding role checks</span></span>

<span data-ttu-id="e8c4b-109">以角色為基礎的授權檢查是由 &mdash; 開發人員在其程式碼中，對控制器或控制器內的動作進行內嵌，並指定目前使用者必須是其成員的角色，以存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="e8c4b-110">例如，下列程式碼會針對屬於角色成員的使用者，限制對的任何動作的存取權 `AdministrationController` `Administrator` ：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="e8c4b-111">您可以指定多個角色做為逗號分隔清單：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="e8c4b-112">只有身為角色或角色成員的使用者才能存取此控制器 `HRManager` `Finance` 。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="e8c4b-113">如果您套用多個屬性，則存取使用者必須是指定之所有角色的成員;下列範例會要求使用者必須同時是 `PowerUser` 和角色的成員 `ControlPanelUser` 。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="e8c4b-114">您可以在動作層級套用其他角色授權屬性，進一步限制存取權：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="e8c4b-115">在先前的程式碼片段中 `Administrator` ，角色或角色的成員 `PowerUser` 可以存取控制器和 `SetTime` 動作，但只有角色的成員 `Administrator` 可以存取此 `ShutDown` 動作。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="e8c4b-116">您也可以鎖定控制器，但允許匿名、未經驗證的個別動作存取。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e8c4b-117">針對 Razor 頁面， `AuthorizeAttribute` 可以透過下列其中一種方式來套用：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="e8c4b-118">使用[慣例](xref:razor-pages/razor-pages-conventions#page-model-action-conventions)，或</span><span class="sxs-lookup"><span data-stu-id="e8c4b-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="e8c4b-119">將套用 `AuthorizeAttribute` 至 `PageModel` 實例：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public class UpdateModel : PageModel
{
    public ActionResult OnPost()
    {
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="e8c4b-120">篩選屬性（包括 `AuthorizeAttribute` ）只能套用至 PageModel，而且不能套用至特定頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="e8c4b-121">以原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="e8c4b-121">Policy based role checks</span></span>

<span data-ttu-id="e8c4b-122">您也可以使用新的原則語法來表示角色需求，其中開發人員會在啟動時註冊原則，作為授權服務設定的一部分。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="e8c4b-123">這通常會發生在 Startup.cs 檔案的中 `ConfigureServices()` 。 *Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="e8c4b-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

::: moniker range=">= aspnetcore-3.0"
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```
::: moniker-end

::: moniker range="< aspnetcore-3.0"
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```
::: moniker-end

<span data-ttu-id="e8c4b-124">原則會在 `Policy` 屬性上使用屬性來套用 `AuthorizeAttribute` ：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="e8c4b-125">如果您想要在需求中指定多個允許的角色，您可以將它們指定為 `RequireRole` 方法的參數：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="e8c4b-126">這個範例會授權屬於 `Administrator` 、 `PowerUser` 或角色的使用者 `BackupAdministrator` 。</span><span class="sxs-lookup"><span data-stu-id="e8c4b-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="e8c4b-127">將角色服務新增至Identity</span><span class="sxs-lookup"><span data-stu-id="e8c4b-127">Add Role services to Identity</span></span>

<span data-ttu-id="e8c4b-128">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="e8c4b-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](roles/samples/3_0/Startup.cs?name=snippet&highlight=7)]
::: moniker-end

::: moniker range="< aspnetcore-3.0"
[!code-csharp[](roles/samples/2_2/Startup.cs?name=snippet&highlight=7)]
::: moniker-end

