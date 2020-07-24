---
title: ASP.NET Core 中的簡單授權
author: rick-anderson
description: 瞭解如何使用授權屬性來限制 ASP.NET Core 控制器和動作的存取。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
uid: security/authorization/simple
ms.openlocfilehash: 09514032349d489b73d5bb785f11e44ca18b169c
ms.sourcegitcommit: 1b89fc58114a251926abadfd5c69c120f1ba12d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87160242"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="5646d-103">ASP.NET Core 中的簡單授權</span><span class="sxs-lookup"><span data-stu-id="5646d-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="5646d-104">ASP.NET Core 中的授權是使用 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> 和其各種參數來控制。</span><span class="sxs-lookup"><span data-stu-id="5646d-104">Authorization in ASP.NET Core is controlled with <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> and its various parameters.</span></span> <span data-ttu-id="5646d-105">以最簡單的形式，將 `[Authorize]` 屬性套用至控制器、動作或 :::no-loc(Razor)::: 頁面，將對該元件的存取限制為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="5646d-105">In its simplest form, applying the `[Authorize]` attribute to a controller, action, or :::no-loc(Razor)::: Page, limits access to that component to any authenticated user.</span></span>

<span data-ttu-id="5646d-106">例如，下列程式碼會將的存取許可權制 `AccountController` 為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="5646d-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="5646d-107">如果您想要將授權套用至某個動作，而不是控制器，請將 `AuthorizeAttribute` 屬性套用至動作本身：</span><span class="sxs-lookup"><span data-stu-id="5646d-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="5646d-108">現在只有經過驗證的使用者可以存取函式 `Logout` 。</span><span class="sxs-lookup"><span data-stu-id="5646d-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="5646d-109">您也可以使用 `AllowAnonymous` 屬性，允許未經驗證的使用者存取個別動作。</span><span class="sxs-lookup"><span data-stu-id="5646d-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="5646d-110">例如：</span><span class="sxs-lookup"><span data-stu-id="5646d-110">For example:</span></span>

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

<span data-ttu-id="5646d-111">這只允許已驗證的使用者 `AccountController` 存取，除了可 `Login` 供所有人使用的動作（不論其已驗證或未驗證/匿名狀態為何）。</span><span class="sxs-lookup"><span data-stu-id="5646d-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="5646d-112">`[AllowAnonymous]`略過所有授權語句。</span><span class="sxs-lookup"><span data-stu-id="5646d-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="5646d-113">如果您結合 `[AllowAnonymous]` 和 any `[Authorize]` 屬性，則 `[Authorize]` 會忽略屬性。</span><span class="sxs-lookup"><span data-stu-id="5646d-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="5646d-114">例如，如果您在 `[AllowAnonymous]` 控制器層級套用，則 `[Authorize]` 會忽略相同控制器（或其中任何動作）上的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="5646d-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>

[!INCLUDE[](~/includes/requireAuth.md)]

<a name="aarp"></a>

## <a name="authorize-attribute-and-no-locrazor-pages"></a><span data-ttu-id="5646d-115">授權屬性和 :::no-loc(Razor)::: 頁面</span><span class="sxs-lookup"><span data-stu-id="5646d-115">Authorize attribute and :::no-loc(Razor)::: Pages</span></span>

<span data-ttu-id="5646d-116"><xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>***無法***套用至 :::no-loc(Razor)::: 頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="5646d-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> can ***not*** be applied to :::no-loc(Razor)::: Page handlers.</span></span> <span data-ttu-id="5646d-117">例如， `[Authorize]` 無法套用至 `OnGet` 、 `OnPost` 或任何其他頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="5646d-117">For example, `[Authorize]` can't be applied to `OnGet`, `OnPost`, or any other page handler.</span></span> <span data-ttu-id="5646d-118">針對不同處理常式具有不同授權需求的頁面，請考慮使用 ASP.NET Core 的 MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="5646d-118">Consider using an ASP.NET Core MVC controller for pages with different authorization requirements for different handlers.</span></span>

<span data-ttu-id="5646d-119">您可以使用下列兩種方法，將授權套用至 :::no-loc(Razor)::: 頁面處理常式方法：</span><span class="sxs-lookup"><span data-stu-id="5646d-119">The following two approaches can be used to apply authorization to :::no-loc(Razor)::: Page handler methods:</span></span>

* <span data-ttu-id="5646d-120">針對需要不同授權的頁面處理常式，請使用個別頁面。</span><span class="sxs-lookup"><span data-stu-id="5646d-120">Use separate pages for page handlers requiring different authorization.</span></span> <span data-ttu-id="5646d-121">已將共用的內容移至一或多個[部分視圖](xref:mvc/views/partial)中。</span><span class="sxs-lookup"><span data-stu-id="5646d-121">Moved shared content into one or more [partial views](xref:mvc/views/partial).</span></span> <span data-ttu-id="5646d-122">可能的話，這是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="5646d-122">When possible, this is the recommended approach.</span></span>
* <span data-ttu-id="5646d-123">針對必須共用通用頁面的內容，撰寫會在 IAsyncPageFilter 中執行授權的篩選。 [OnPageHandlerSelectionAsync](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A)。</span><span class="sxs-lookup"><span data-stu-id="5646d-123">For content that must share a common page, write a filter that performs authorization as part of [IAsyncPageFilter.OnPageHandlerSelectionAsync](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A).</span></span> <span data-ttu-id="5646d-124">[PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) GitHub 專案會示範這種方法：</span><span class="sxs-lookup"><span data-stu-id="5646d-124">The [PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) GitHub project demonstrates this approach:</span></span>
  * <span data-ttu-id="5646d-125">[AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs)會實作為授權篩選準則：[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="5646d-125">The [AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs) implements the authorization filter: [!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]</span></span>

  * <span data-ttu-id="5646d-126">[[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16)屬性會套用至 `OnGet` 頁面處理常式：[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="5646d-126">The [[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16) attribute is applied to the `OnGet` page handler: [!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]</span></span>

> [!WARNING]
> <span data-ttu-id="5646d-127">[PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth)範例***方法不會：***</span><span class="sxs-lookup"><span data-stu-id="5646d-127">The [PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth) sample approach does ***not***:</span></span>
> * <span data-ttu-id="5646d-128">以套用至頁面、頁面模型或全域的授權屬性進行撰寫。</span><span class="sxs-lookup"><span data-stu-id="5646d-128">Compose with authorization attributes applied to the page, page model, or globally.</span></span> <span data-ttu-id="5646d-129">當您有一或多個 `AuthorizeAttribute` `AuthorizeFilter` 實例同時套用至該頁面時，撰寫授權屬性會導致驗證和授權執行多次。</span><span class="sxs-lookup"><span data-stu-id="5646d-129">Composing authorization attributes results in authentication and authorization executing multiple times when you have one more `AuthorizeAttribute` or `AuthorizeFilter` instances also applied to the page.</span></span>
> * <span data-ttu-id="5646d-130">與 ASP.NET Core 驗證和授權系統的其餘部分搭配使用。</span><span class="sxs-lookup"><span data-stu-id="5646d-130">Work in conjunction with the rest of ASP.NET Core authentication and authorization system.</span></span> <span data-ttu-id="5646d-131">您必須使用此方法來確認您的應用程式能夠正確運作。</span><span class="sxs-lookup"><span data-stu-id="5646d-131">You must verify using this approach works correctly for your application.</span></span>

<span data-ttu-id="5646d-132">沒有計劃可支援 `AuthorizeAttribute` :::no-loc(Razor)::: 頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="5646d-132">There are no plans to support the `AuthorizeAttribute` on :::no-loc(Razor)::: Page handlers.</span></span> 
