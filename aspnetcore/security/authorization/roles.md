---
title: "角色型授權"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 1dc76f316b70d486febe386cc47cd1f843d8d8e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="role-based-authorization"></a>角色型授權

<a name=security-authorization-role-based></a>

當建立了識別之後可能屬於一個或多個角色，例如 Tracy 可能屬於系統管理員和使用者角色儘管 Scott 可能只屬於使用者角色。 如何建立和管理這些角色取決於備份存放區的授權程序。 角色會公開給開發人員透過[IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole)屬性[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)類別。

## <a name="adding-role-checks"></a>加入角色檢查

以角色為基礎的授權檢查宣告式-開發人員會內嵌它們對控制器或動作中控制站，其程式碼內指定目前使用者必須是成員的存取要求之資源的角色。

例如下列程式碼時，會在限制的存取權的任何動作`AdministrationController`使用者是誰隸屬`Administrator`群組。

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

您可以指定多個角色，為逗號分隔的清單。

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

您可以套用在動作層級; 其他角色授權屬性，以進一步限制存取

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

<a name=security-authorization-role-policy></a>

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

原則會套用使用`Policy`屬性`AuthorizeAttribute`屬性;

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

如果您想要指定多個允許的角色的需求，則您可以指定它們做為參數`RequireRole`方法。

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

這個範例會授與使用者隸屬於`Administrator`，`PowerUser`或`BackupAdministrator`角色。
