---
title: ASP.NET Core 中的簡單授權
author: rick-anderson
description: 瞭解如何使用授權屬性來限制 ASP.NET Core 控制器和動作的存取。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663580"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core 中的簡單授權

<a name="security-authorization-simple"></a>

MVC 中的授權是透過 `AuthorizeAttribute` 屬性和其各種參數來控制。 最簡單的說，將 `AuthorizeAttribute` 屬性套用至控制器或動作，會將控制器或動作的存取限制為任何已驗證的使用者。

例如，下列程式碼會將 `AccountController` 的存取限制為任何已驗證的使用者。

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

如果您想要將授權套用至某個動作，而不是控制器，請將 `AuthorizeAttribute` 屬性套用至動作本身：

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

現在只有經過驗證的使用者可以存取 `Logout` 函式。

您也可以使用 `AllowAnonymous` 屬性，以允許未經驗證的使用者存取個別動作。 例如：

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

這只允許已驗證的使用者存取 `AccountController`，除了 `Login` 動作以外，所有人都可以存取，不論其已驗證或未驗證/匿名的狀態為何。

> [!WARNING]
> `[AllowAnonymous]` 略過所有授權語句。 如果您結合 `[AllowAnonymous]` 和任何 `[Authorize]` 屬性，則會忽略 `[Authorize]` 屬性。 例如，如果您在控制器層級套用 `[AllowAnonymous]`，則會忽略相同控制器上的任何 `[Authorize]` 屬性（或其中任何動作）。
