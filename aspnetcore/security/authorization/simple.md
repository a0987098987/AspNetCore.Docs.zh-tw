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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="505d7-103">ASP.NET Core 中的簡單授權</span><span class="sxs-lookup"><span data-stu-id="505d7-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="505d7-104">MVC 中的`AuthorizeAttribute`授權是透過屬性和其各種參數來控制。</span><span class="sxs-lookup"><span data-stu-id="505d7-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="505d7-105">最簡單的是`AuthorizeAttribute` ，將屬性套用至控制器或動作，會限制對任何已驗證使用者的控制器或動作的存取權。</span><span class="sxs-lookup"><span data-stu-id="505d7-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="505d7-106">例如，下列程式碼會將的存取權`AccountController`限制為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="505d7-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="505d7-107">如果您想要將授權套用至某個動作，而不是控制器， `AuthorizeAttribute`請將屬性套用至動作本身：</span><span class="sxs-lookup"><span data-stu-id="505d7-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="505d7-108">現在只有經過驗證的`Logout`使用者可以存取函式。</span><span class="sxs-lookup"><span data-stu-id="505d7-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="505d7-109">您也可以使用`AllowAnonymous`屬性，允許未經驗證的使用者存取個別動作。</span><span class="sxs-lookup"><span data-stu-id="505d7-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="505d7-110">例如：</span><span class="sxs-lookup"><span data-stu-id="505d7-110">For example:</span></span>

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

<span data-ttu-id="505d7-111">這只允許已驗證的`AccountController`使用者存取，除了可供`Login`所有人使用的動作（不論其已驗證或未驗證/匿名狀態為何）。</span><span class="sxs-lookup"><span data-stu-id="505d7-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="505d7-112">`[AllowAnonymous]`略過所有授權語句。</span><span class="sxs-lookup"><span data-stu-id="505d7-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="505d7-113">如果您結合`[AllowAnonymous]`和 any `[Authorize]`屬性，則`[Authorize]`會忽略屬性。</span><span class="sxs-lookup"><span data-stu-id="505d7-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="505d7-114">例如，如果您`[AllowAnonymous]`在控制器層級套用，則`[Authorize]`會忽略相同控制器（或其中任何動作）上的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="505d7-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
