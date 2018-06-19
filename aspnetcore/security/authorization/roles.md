---
title: 在 ASP.NET Core 角色為基礎的授權
author: rick-anderson
description: 了解如何藉由傳遞角色來授權屬性限制 ASP.NET Core 控制器和動作的存取。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: f1e7209cae1e2a58ad536547d655dd744ca0d3f7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740032"
---
# <a name="role-based-authorization-in-aspnet-core"></a>在 ASP.NET Core 角色為基礎的授權

<a name="security-authorization-role-based"></a>

建立身分識別時它可能屬於一個或多個角色。 比方說，Tracy 可能屬於系統管理員和使用者角色，儘管 Scott 可能只屬於使用者角色。 如何建立和管理這些角色取決於備份存放區的授權程序。 角色會公開給開發人員透過[IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole)方法[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)類別。

## <a name="adding-role-checks"></a>加入角色檢查

以角色為基礎的授權檢查是宣告式&mdash;開發人員會內嵌它們對控制器或動作中控制站，其程式碼內指定目前使用者必須是成員的存取要求之資源的角色。

例如，下列程式碼限制的存取權的任何動作`AdministrationController`使用者是誰隸屬`Administrator`角色：

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

您可以為以逗號分隔清單指定多個角色：

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

此控制站可以只存取的使用者是成員的`HRManager`角色或`Finance`角色。

如果您將套用多個屬性，則必須指定; 的所有角色的成員存取的使用者。下列範例會要求使用者必須是兩者的成員`PowerUser`和`ControlPanelUser`角色。

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

您可以進一步限制存取，藉由套用在動作層級的其他角色授權屬性：

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

中的前一個程式碼片段成員`Administrator`角色或`PowerUser`角色可以存取控制器和`SetTime`動作，但是只有`Administrator`角色可以存取`ShutDown`動作。

您也可以鎖定控制站，但允許匿名、 未經驗證存取個別的動作。

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

## <a name="policy-based-role-checks"></a>原則為基礎的角色檢查

角色需求，也可以使用新的原則語法中，開發人員，註冊原則，以在啟動授權服務組態的一部分來表示。 這通常發生在`ConfigureServices()`中您*Startup.cs*檔案。

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

原則會套用使用`Policy`屬性`AuthorizeAttribute`屬性：

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

如果您想要指定多個允許的角色的需求，則您可以指定它們做為參數`RequireRole`方法：

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

這個範例會授與使用者隸屬於`Administrator`，`PowerUser`或`BackupAdministrator`角色。
