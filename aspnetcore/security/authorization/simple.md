---
title: ASP.NET Core 中的簡單授權
author: rick-anderson
description: 瞭解如何使用授權屬性來限制 ASP.NET Core 控制器和動作的存取。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/simple
ms.openlocfilehash: 09514032349d489b73d5bb785f11e44ca18b169c
ms.sourcegitcommit: 1b89fc58114a251926abadfd5c69c120f1ba12d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87160242"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core 中的簡單授權

<a name="security-authorization-simple"></a>

ASP.NET Core 中的授權是使用 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> 和其各種參數來控制。 以最簡單的形式，將 `[Authorize]` 屬性套用至控制器、動作或 Razor 頁面，將對該元件的存取限制為任何已驗證的使用者。

例如，下列程式碼會將的存取許可權制 `AccountController` 為任何已驗證的使用者。

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

現在只有經過驗證的使用者可以存取函式 `Logout` 。

您也可以使用 `AllowAnonymous` 屬性，允許未經驗證的使用者存取個別動作。 例如：

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

這只允許已驗證的使用者 `AccountController` 存取，除了可 `Login` 供所有人使用的動作（不論其已驗證或未驗證/匿名狀態為何）。

> [!WARNING]
> `[AllowAnonymous]`略過所有授權語句。 如果您結合 `[AllowAnonymous]` 和 any `[Authorize]` 屬性，則 `[Authorize]` 會忽略屬性。 例如，如果您在 `[AllowAnonymous]` 控制器層級套用，則 `[Authorize]` 會忽略相同控制器（或其中任何動作）上的任何屬性。

[!INCLUDE[](~/includes/requireAuth.md)]

<a name="aarp"></a>

## <a name="authorize-attribute-and-no-locrazor-pages"></a>授權屬性和 Razor 頁面

<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>***無法***套用至 Razor 頁面處理常式。 例如， `[Authorize]` 無法套用至 `OnGet` 、 `OnPost` 或任何其他頁面處理常式。 針對不同處理常式具有不同授權需求的頁面，請考慮使用 ASP.NET Core 的 MVC 控制器。

您可以使用下列兩種方法，將授權套用至 Razor 頁面處理常式方法：

* 針對需要不同授權的頁面處理常式，請使用個別頁面。 已將共用的內容移至一或多個[部分視圖](xref:mvc/views/partial)中。 可能的話，這是建議的方法。
* 針對必須共用通用頁面的內容，撰寫會在 IAsyncPageFilter 中執行授權的篩選。 [OnPageHandlerSelectionAsync](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A)。 [PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) GitHub 專案會示範這種方法：
  * [AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs)會實作為授權篩選準則：[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]

  * [[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16)屬性會套用至 `OnGet` 頁面處理常式：[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]

> [!WARNING]
> [PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth)範例***方法不會：***
> * 以套用至頁面、頁面模型或全域的授權屬性進行撰寫。 當您有一或多個 `AuthorizeAttribute` `AuthorizeFilter` 實例同時套用至該頁面時，撰寫授權屬性會導致驗證和授權執行多次。
> * 與 ASP.NET Core 驗證和授權系統的其餘部分搭配使用。 您必須使用此方法來確認您的應用程式能夠正確運作。

沒有計劃可支援 `AuthorizeAttribute` Razor 頁面處理常式。 
