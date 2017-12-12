---
title: "簡單的授權"
author: rick-anderson
description: "本文件說明如何使用授權屬性來限制對 ASP.NET Core 控制器和動作的存取。"
keywords: "ASP.NET Core，授權 AuthorizeAttribute"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="30252-104">簡單的授權</span><span class="sxs-lookup"><span data-stu-id="30252-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="30252-105">在 MVC 中的授權由`AuthorizeAttribute`屬性和各種不同的參數。</span><span class="sxs-lookup"><span data-stu-id="30252-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="30252-106">簡單來說，套用`AuthorizeAttribute`屬性到控制器或動作限制存取控制器或動作，任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="30252-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="30252-107">例如，下列程式碼會限制存取`AccountController`任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="30252-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="30252-108">如果您想要將授權套用至動作，而非控制站，套用`AuthorizeAttribute`屬性本身的動作：</span><span class="sxs-lookup"><span data-stu-id="30252-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="30252-109">現在已驗證的使用者可以存取`Logout`函式。</span><span class="sxs-lookup"><span data-stu-id="30252-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="30252-110">您也可以使用`AllowAnonymousAttribute`允許未經驗證的使用者，對個別動作所存取的屬性。</span><span class="sxs-lookup"><span data-stu-id="30252-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="30252-111">例如: </span><span class="sxs-lookup"><span data-stu-id="30252-111">For example:</span></span>

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

<span data-ttu-id="30252-112">這樣可讓只有經過驗證的使用者`AccountController`，除了`Login`動作，都可以存取所有人不論他們已驗證或未驗證 / 匿名的狀態。</span><span class="sxs-lookup"><span data-stu-id="30252-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="30252-113">`[AllowAnonymous]`會略過所有授權陳述式。</span><span class="sxs-lookup"><span data-stu-id="30252-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="30252-114">如果您套用結合`[AllowAnonymous]`和任何`[Authorize]`屬性然後 Authorize 屬性將永遠會被忽略。</span><span class="sxs-lookup"><span data-stu-id="30252-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="30252-115">例如，如果您套用`[AllowAnonymous]`在控制器層級任何`[Authorize]`屬性相同的控制站，或在其中任何動作將會被忽略。</span><span class="sxs-lookup"><span data-stu-id="30252-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
