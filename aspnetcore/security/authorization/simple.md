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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="66638-103">ASP.NET Core 中的簡單授權</span><span class="sxs-lookup"><span data-stu-id="66638-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="66638-104">MVC 中的授權是透過 `AuthorizeAttribute` 屬性和其各種參數來控制。</span><span class="sxs-lookup"><span data-stu-id="66638-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="66638-105">最簡單的說，將 `AuthorizeAttribute` 屬性套用至控制器或動作，會將控制器或動作的存取限制為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="66638-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="66638-106">例如，下列程式碼會將 `AccountController` 的存取限制為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="66638-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="66638-107">如果您想要將授權套用至某個動作，而不是控制器，請將 `AuthorizeAttribute` 屬性套用至動作本身：</span><span class="sxs-lookup"><span data-stu-id="66638-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="66638-108">現在只有經過驗證的使用者可以存取 `Logout` 函式。</span><span class="sxs-lookup"><span data-stu-id="66638-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="66638-109">您也可以使用 `AllowAnonymous` 屬性，以允許未經驗證的使用者存取個別動作。</span><span class="sxs-lookup"><span data-stu-id="66638-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="66638-110">例如：</span><span class="sxs-lookup"><span data-stu-id="66638-110">For example:</span></span>

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

<span data-ttu-id="66638-111">這只允許已驗證的使用者存取 `AccountController`，除了 `Login` 動作以外，所有人都可以存取，不論其已驗證或未驗證/匿名的狀態為何。</span><span class="sxs-lookup"><span data-stu-id="66638-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="66638-112">`[AllowAnonymous]` 略過所有授權語句。</span><span class="sxs-lookup"><span data-stu-id="66638-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="66638-113">如果您結合 `[AllowAnonymous]` 和任何 `[Authorize]` 屬性，則會忽略 `[Authorize]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="66638-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="66638-114">例如，如果您在控制器層級套用 `[AllowAnonymous]`，則會忽略相同控制器上的任何 `[Authorize]` 屬性（或其中任何動作）。</span><span class="sxs-lookup"><span data-stu-id="66638-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
