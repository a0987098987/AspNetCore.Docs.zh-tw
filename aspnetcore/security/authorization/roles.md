---
title: ASP.NET Core 中的角色為基礎的授權
author: rick-anderson
description: 了解如何將角色傳遞至 Authorize 屬性來限制 ASP.NET Core 控制器和動作的存取。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0e01e1976e2721ca64720a67c6341661f646395c
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209092"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="bce55-103">ASP.NET Core 中的角色為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="bce55-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="bce55-104">建立身分識別時它可能屬於一個或多個角色。</span><span class="sxs-lookup"><span data-stu-id="bce55-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="bce55-105">比方說，Tracy 可能隸屬於系統管理員和使用者角色中，儘管 Scott 可能只屬於使用者角色。</span><span class="sxs-lookup"><span data-stu-id="bce55-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="bce55-106">建立及管理這些角色的方式取決於備份存放區的授權程序。</span><span class="sxs-lookup"><span data-stu-id="bce55-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="bce55-107">角色會公開給開發人員逐步[IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)方法[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)類別。</span><span class="sxs-lookup"><span data-stu-id="bce55-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="bce55-108">新增角色檢查</span><span class="sxs-lookup"><span data-stu-id="bce55-108">Adding role checks</span></span>

<span data-ttu-id="bce55-109">以角色為基礎的授權檢查是宣告式&mdash;開發人員將其內嵌控制器或動作的控制器內的程式碼內指定目前使用者必須是成員的存取要求的資源角色。</span><span class="sxs-lookup"><span data-stu-id="bce55-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="bce55-110">例如，下列程式碼的限制任何動作的存取權`AdministrationController`對使用者是誰的成員`Administrator`角色：</span><span class="sxs-lookup"><span data-stu-id="bce55-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="bce55-111">您可以為以逗號分隔清單指定多個角色：</span><span class="sxs-lookup"><span data-stu-id="bce55-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="bce55-112">此控制器會只可存取的使用者是成員的`HRManager`角色或`Finance`角色。</span><span class="sxs-lookup"><span data-stu-id="bce55-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="bce55-113">如果您套用多個屬性，則必須指定; 的所有角色的成員存取的使用者。下列範例會要求使用者必須是兩者的成員`PowerUser`和`ControlPanelUser`角色。</span><span class="sxs-lookup"><span data-stu-id="bce55-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="bce55-114">您可以進一步限制存取，藉由套用在動作層級的其他角色授權屬性：</span><span class="sxs-lookup"><span data-stu-id="bce55-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="bce55-115">在先前的程式碼片段成員的`Administrator`角色或`PowerUser`角色可以存取控制器並`SetTime`動作，但只有成員`Administrator`角色可以存取`ShutDown`動作。</span><span class="sxs-lookup"><span data-stu-id="bce55-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="bce55-116">您也可以鎖定的控制站，但允許匿名、 未經驗證存取個別的動作。</span><span class="sxs-lookup"><span data-stu-id="bce55-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<span data-ttu-id="bce55-117">Razor 頁面`AuthorizeAttribute`可以透過下列方式套用：</span><span class="sxs-lookup"><span data-stu-id="bce55-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="bce55-118">使用[慣例](xref:razor-pages/razor-pages-conventions#page-model-action-conventions)，或</span><span class="sxs-lookup"><span data-stu-id="bce55-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="bce55-119">套用`AuthorizeAttribute`至`PageModel`執行個體：</span><span class="sxs-lookup"><span data-stu-id="bce55-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

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
> <span data-ttu-id="bce55-120">篩選屬性，包括`AuthorizeAttribute`只能套用至 PageModel，無法套用至特定頁面處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="bce55-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="bce55-121">原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="bce55-121">Policy based role checks</span></span>

<span data-ttu-id="bce55-122">角色需求也可以使用新的原則語法，其中的開發人員會在啟動原則註冊授權服務組態的一部分來表示。</span><span class="sxs-lookup"><span data-stu-id="bce55-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="bce55-123">這通常發生在`ConfigureServices()`在您*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="bce55-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="bce55-124">原則會套用使用`Policy`屬性上的`AuthorizeAttribute`屬性：</span><span class="sxs-lookup"><span data-stu-id="bce55-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="bce55-125">如果您想要指定多個允許的角色的需求，則您可以指定它們做為參數`RequireRole`方法：</span><span class="sxs-lookup"><span data-stu-id="bce55-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="bce55-126">此範例會授權使用者屬於`Administrator`，`PowerUser`或`BackupAdministrator`角色。</span><span class="sxs-lookup"><span data-stu-id="bce55-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="bce55-127">將角色服務新增到 身分識別</span><span class="sxs-lookup"><span data-stu-id="bce55-127">Add Role services to Identity</span></span>

<span data-ttu-id="bce55-128">附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)來新增角色服務：</span><span class="sxs-lookup"><span data-stu-id="bce55-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
