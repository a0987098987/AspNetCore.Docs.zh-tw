---
title: ASP.NET Core 中的簡單授權
author: rick-anderson
description: 了解如何使用 Authorize 屬性來限制存取的 ASP.NET Core 控制器和動作。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897675"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core 中的簡單授權

<a name="security-authorization-simple"></a>

在 MVC 中的授權透過`AuthorizeAttribute`屬性以及其各種不同的參數。 簡單來說，套用`AuthorizeAttribute`控制器或動作限制存取的控制器或動作，任何已驗證的使用者屬性。

例如，下列程式碼會限制存取`AccountController`，任何已驗證的使用者。

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

如果您想要將授權套用至動作，而不是控制器，套用`AuthorizeAttribute`屬性本身的動作：

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

現在只有已驗證的使用者可以存取`Logout`函式。

您也可以使用`AllowAnonymous`允許未經驗證使用者個別動作時所存取的屬性。 例如: 

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

這可讓只有經過驗證的使用者才能`AccountController`，除了`Login`供所有人不論其驗證或未驗證 / 匿名狀態的動作。

> [!WARNING]
> `[AllowAnonymous]` 會略過授權的所有陳述式。 如果您合併`[AllowAnonymous]`以及任何`[Authorize]`屬性，`[Authorize]`屬性會被忽略。 例如，如果您套用`[AllowAnonymous]`在控制器層級中，任何`[Authorize]`屬性相同的控制站上 （或其內的任何動作） 會被忽略。
