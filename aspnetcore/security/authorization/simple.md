---
title: "簡單的授權"
author: rick-anderson
description: "本文件說明如何使用授權屬性來限制對 ASP.NET Core 控制器和動作的存取。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 503ebc665efd460a85f49844ddc847eb12114308
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="simple-authorization"></a>簡單的授權

<a name="security-authorization-simple"></a>

在 MVC 中的授權由`AuthorizeAttribute`屬性和各種不同的參數。 簡單來說，套用`AuthorizeAttribute`屬性到控制器或動作限制存取控制器或動作，任何已驗證的使用者。

例如，下列程式碼會限制存取`AccountController`任何已驗證的使用者。

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

如果您想要將授權套用至動作，而非控制站，套用`AuthorizeAttribute`屬性本身的動作：

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

現在已驗證的使用者可以存取`Logout`函式。

您也可以使用`AllowAnonymous`允許未經驗證的使用者，對個別動作所存取的屬性。 例如: 

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

這樣可讓只有經過驗證的使用者`AccountController`，除了`Login`動作，都可以存取所有人不論他們已驗證或未驗證 / 匿名的狀態。

>[!WARNING]
> `[AllowAnonymous]`會略過所有授權陳述式。 如果您套用結合`[AllowAnonymous]`和任何`[Authorize]`屬性然後 Authorize 屬性將永遠會被忽略。 例如，如果您套用`[AllowAnonymous]`在控制器層級任何`[Authorize]`屬性相同的控制站，或在其中任何動作將會被忽略。
