---
title: ASP.NET Core 中的簡單授權
author: rick-anderson
description: 瞭解如何使用授權屬性來限制 ASP.NET Core 控制器和動作的存取。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/simple
ms.openlocfilehash: f273c3e9db74fa63de85c65d94223d0ef7326036
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775631"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core 中的簡單授權

<a name="security-authorization-simple"></a>

MVC 中的`AuthorizeAttribute`授權是透過屬性和其各種參數來控制。 最簡單的是`AuthorizeAttribute` ，將屬性套用至控制器或動作，會限制對任何已驗證使用者的控制器或動作的存取權。

例如，下列程式碼會將的存取權`AccountController`限制為任何已驗證的使用者。

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

如果您想要將授權套用至某個動作，而不是控制器， `AuthorizeAttribute`請將屬性套用至動作本身：

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

現在只有經過驗證的`Logout`使用者可以存取函式。

您也可以使用`AllowAnonymous`屬性，允許未經驗證的使用者存取個別動作。 例如：

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

這只允許已驗證的`AccountController`使用者存取，除了可供`Login`所有人使用的動作（不論其已驗證或未驗證/匿名狀態為何）。

> [!WARNING]
> `[AllowAnonymous]`略過所有授權語句。 如果您結合`[AllowAnonymous]`和 any `[Authorize]`屬性，則`[Authorize]`會忽略屬性。 例如，如果您`[AllowAnonymous]`在控制器層級套用，則`[Authorize]`會忽略相同控制器（或其中任何動作）上的任何屬性。
