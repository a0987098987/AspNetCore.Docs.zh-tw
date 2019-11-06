---
title: ASP.NET Core 中以角色為基礎的授權
author: rick-anderson
description: 瞭解如何藉由將角色傳遞至授權屬性來限制 ASP.NET Core 控制器和動作存取。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 28aa3df6aa661d0b762df78fe611cd827af43f75
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73660045"
---
# <a name="role-based-authorization-in-aspnet-core"></a>ASP.NET Core 中以角色為基礎的授權

<a name="security-authorization-role-based"></a>

建立身分識別時，它可能屬於一或多個角色。 例如，Tracy 可能屬於系統管理員和使用者角色，而 Scott 只能屬於使用者角色。 建立和管理這些角色的方式，取決於授權程式的備份存放區。 角色會透過[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)類別上的[IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)方法向開發人員公開。

## <a name="adding-role-checks"></a>新增角色檢查

以角色為基礎的授權檢查是由開發人員在其程式碼中，對控制器或控制器內的動作進行內嵌的宣告式&mdash;，指定目前使用者必須是其成員的角色，以存取要求的資源。

例如，下列程式碼會針對屬於 `Administrator` 角色成員的使用者，限制對 `AdministrationController` 上任何動作的存取權：

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

您可以指定多個角色做為逗號分隔清單：

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

只有身為 `HRManager` 角色或 `Finance` 角色成員的使用者，才能存取此控制器。

如果您套用多個屬性，則存取使用者必須是指定之所有角色的成員;下列範例會要求使用者必須同時是 `PowerUser` 和 `ControlPanelUser` 角色的成員。

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

您可以在動作層級套用其他角色授權屬性，進一步限制存取權：

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

在先前的程式碼片段中，`Administrator` 角色或 `PowerUser` 角色的成員可以存取控制器和 `SetTime` 動作，但只有 `Administrator` 角色的成員可以存取 `ShutDown` 動作。

您也可以鎖定控制器，但允許匿名、未經驗證的個別動作存取。

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

針對 Razor Pages，可以透過下列其中一種方式來套用 `AuthorizeAttribute`：

* 使用[慣例](xref:razor-pages/razor-pages-conventions#page-model-action-conventions)，或
* 將 `AuthorizeAttribute` 套用至 `PageModel` 實例：

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
> 篩選屬性（包括 `AuthorizeAttribute`）只能套用至 PageModel，而且不能套用至特定頁面處理常式方法。
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>以原則為基礎的角色檢查

您也可以使用新的原則語法來表示角色需求，其中開發人員會在啟動時註冊原則，作為授權服務設定的一部分。 這通常會發生在*Startup.cs*檔案的 `ConfigureServices()` 中。

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

原則會使用 `AuthorizeAttribute` 屬性上的 `Policy` 屬性來套用：

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

如果您想要在需求中指定多個允許的角色，您可以將它們指定為 `RequireRole` 方法的參數：

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

這個範例會授權屬於 `Administrator`、`PowerUser` 或 `BackupAdministrator` 角色的使用者。

### <a name="add-role-services-to-identity"></a>將角色服務新增至身分識別

附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](roles/samples/3_0/Startup.cs?name=snippet&highlight=7)]
::: moniker-end

::: moniker range="< aspnetcore-3.0"
[!code-csharp[](roles/samples/2_2/Startup.cs?name=snippet&highlight=7)]
::: moniker-end

