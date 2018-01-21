---
title: "角色型授權"
author: rick-anderson
description: "本文件將示範如何藉由傳遞角色 Authorize 屬性來限制 ASP.NET Core 控制器和動作的存取。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 18964464fea76c91d716202d89ee3a3eb36c3078
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="role-based-authorization"></a><span data-ttu-id="d7ad5-103">角色型授權</span><span class="sxs-lookup"><span data-stu-id="d7ad5-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="d7ad5-104">建立身分識別時它可能屬於一個或多個角色。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="d7ad5-105">比方說，Tracy 可能屬於系統管理員和使用者角色，儘管 Scott 可能只屬於使用者角色。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="d7ad5-106">如何建立和管理這些角色取決於備份存放區的授權程序。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="d7ad5-107">角色會公開給開發人員透過[IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole)方法[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)類別。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-107">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="d7ad5-108">加入角色檢查</span><span class="sxs-lookup"><span data-stu-id="d7ad5-108">Adding role checks</span></span>

<span data-ttu-id="d7ad5-109">以角色為基礎的授權檢查是宣告式&mdash;開發人員會內嵌它們對控制器或動作中控制站，其程式碼內指定目前使用者必須是成員的存取要求之資源的角色。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="d7ad5-110">例如，下列程式碼限制的存取權的任何動作`AdministrationController`使用者是誰隸屬`Administrator`角色：</span><span class="sxs-lookup"><span data-stu-id="d7ad5-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="d7ad5-111">您可以為以逗號分隔清單指定多個角色：</span><span class="sxs-lookup"><span data-stu-id="d7ad5-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="d7ad5-112">此控制站可以只存取的使用者是成員的`HRManager`角色或`Finance`角色。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="d7ad5-113">如果您將套用多個屬性，則必須指定; 的所有角色的成員存取的使用者。下列範例會要求使用者必須是兩者的成員`PowerUser`和`ControlPanelUser`角色。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="d7ad5-114">您可以進一步限制存取，藉由套用在動作層級的其他角色授權屬性：</span><span class="sxs-lookup"><span data-stu-id="d7ad5-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="d7ad5-115">中的前一個程式碼片段成員`Administrator`角色或`PowerUser`角色可以存取控制器和`SetTime`動作，但是只有`Administrator`角色可以存取`ShutDown`動作。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="d7ad5-116">您也可以鎖定控制站，但允許匿名、 未經驗證存取個別的動作。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="d7ad5-117">原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="d7ad5-117">Policy based role checks</span></span>

<span data-ttu-id="d7ad5-118">角色需求，也可以使用新的原則語法中，開發人員，註冊原則，以在啟動授權服務組態的一部分來表示。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="d7ad5-119">這通常發生在`ConfigureServices()`中您*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="d7ad5-120">原則會套用使用`Policy`屬性`AuthorizeAttribute`屬性：</span><span class="sxs-lookup"><span data-stu-id="d7ad5-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="d7ad5-121">如果您想要指定多個允許的角色的需求，則您可以指定它們做為參數`RequireRole`方法：</span><span class="sxs-lookup"><span data-stu-id="d7ad5-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="d7ad5-122">這個範例會授與使用者隸屬於`Administrator`，`PowerUser`或`BackupAdministrator`角色。</span><span class="sxs-lookup"><span data-stu-id="d7ad5-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
