---
title: 在 ASP.NET Core 簡單的授權
author: rick-anderson
description: 了解如何使用授權屬性來限制對 ASP.NET Core 控制器和動作的存取。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30078240"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="5e688-103">在 ASP.NET Core 簡單的授權</span><span class="sxs-lookup"><span data-stu-id="5e688-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="5e688-104">在 MVC 中的授權由`AuthorizeAttribute`屬性和各種不同的參數。</span><span class="sxs-lookup"><span data-stu-id="5e688-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="5e688-105">簡單來說，套用`AuthorizeAttribute`屬性到控制器或動作限制存取控制器或動作，任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="5e688-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="5e688-106">例如，下列程式碼會限制存取`AccountController`任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="5e688-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="5e688-107">如果您想要將授權套用至動作，而非控制站，套用`AuthorizeAttribute`屬性本身的動作：</span><span class="sxs-lookup"><span data-stu-id="5e688-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="5e688-108">現在已驗證的使用者可以存取`Logout`函式。</span><span class="sxs-lookup"><span data-stu-id="5e688-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="5e688-109">您也可以使用`AllowAnonymous`允許未經驗證的使用者，對個別動作所存取的屬性。</span><span class="sxs-lookup"><span data-stu-id="5e688-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="5e688-110">例如: </span><span class="sxs-lookup"><span data-stu-id="5e688-110">For example:</span></span>

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

<span data-ttu-id="5e688-111">這樣可讓只有經過驗證的使用者`AccountController`，除了`Login`動作，都可以存取所有人不論他們已驗證或未驗證 / 匿名的狀態。</span><span class="sxs-lookup"><span data-stu-id="5e688-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="5e688-112">`[AllowAnonymous]` 會略過所有授權陳述式。</span><span class="sxs-lookup"><span data-stu-id="5e688-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="5e688-113">如果您套用結合`[AllowAnonymous]`和任何`[Authorize]`屬性然後 Authorize 屬性將永遠會被忽略。</span><span class="sxs-lookup"><span data-stu-id="5e688-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="5e688-114">例如，如果您套用`[AllowAnonymous]`在控制器層級任何`[Authorize]`屬性相同的控制站，或在其中任何動作將會被忽略。</span><span class="sxs-lookup"><span data-stu-id="5e688-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
