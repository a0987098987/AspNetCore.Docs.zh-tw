---
<span data-ttu-id="6f5df-101">標題： ASP.NET Core author 中的簡單授權： rick-anderson 描述：瞭解如何使用 authorization 屬性來限制對 ASP.NET Core 控制器和動作的存取。</span><span class="sxs-lookup"><span data-stu-id="6f5df-101">title: Simple authorization in ASP.NET Core author: rick-anderson description: Learn how to use the Authorize attribute to restrict access to ASP.NET Core controllers and actions.</span></span>
<span data-ttu-id="6f5df-102">riande ms. date：10/14/2016 否-loc：</span><span class="sxs-lookup"><span data-stu-id="6f5df-102">ms.author: riande ms.date: 10/14/2016 no-loc:</span></span>
- <span data-ttu-id="6f5df-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6f5df-103">'Blazor'</span></span>
- <span data-ttu-id="6f5df-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6f5df-104">'Identity'</span></span>
- <span data-ttu-id="6f5df-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6f5df-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="6f5df-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6f5df-106">'Razor'</span></span>
- <span data-ttu-id="6f5df-107">' SignalR ' uid：安全性/授權/簡單</span><span class="sxs-lookup"><span data-stu-id="6f5df-107">'SignalR' uid: security/authorization/simple</span></span>

---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="6f5df-108">ASP.NET Core 中的簡單授權</span><span class="sxs-lookup"><span data-stu-id="6f5df-108">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="6f5df-109">ASP.NET Core 中的授權是使用 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> 和其各種參數來控制。</span><span class="sxs-lookup"><span data-stu-id="6f5df-109">Authorization in ASP.NET Core is controlled with <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> and its various parameters.</span></span> <span data-ttu-id="6f5df-110">以最簡單的形式，將 `[Authorize]` 屬性套用至控制器、動作或 Razor 頁面，將對該元件的存取限制為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="6f5df-110">In its simplest form, applying the `[Authorize]` attribute to a controller, action, or Razor Page, limits access to that component to any authenticated user.</span></span>

<span data-ttu-id="6f5df-111">例如，下列程式碼會將的存取許可權制 `AccountController` 為任何已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="6f5df-111">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="6f5df-112">如果您想要將授權套用至某個動作，而不是控制器，請將 `AuthorizeAttribute` 屬性套用至動作本身：</span><span class="sxs-lookup"><span data-stu-id="6f5df-112">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="6f5df-113">現在只有經過驗證的使用者可以存取函式 `Logout` 。</span><span class="sxs-lookup"><span data-stu-id="6f5df-113">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="6f5df-114">您也可以使用 `AllowAnonymous` 屬性，允許未經驗證的使用者存取個別動作。</span><span class="sxs-lookup"><span data-stu-id="6f5df-114">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="6f5df-115">例如：</span><span class="sxs-lookup"><span data-stu-id="6f5df-115">For example:</span></span>

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

<span data-ttu-id="6f5df-116">這只允許已驗證的使用者 `AccountController` 存取，除了可 `Login` 供所有人使用的動作（不論其已驗證或未驗證/匿名狀態為何）。</span><span class="sxs-lookup"><span data-stu-id="6f5df-116">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="6f5df-117">`[AllowAnonymous]`略過所有授權語句。</span><span class="sxs-lookup"><span data-stu-id="6f5df-117">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="6f5df-118">如果您結合 `[AllowAnonymous]` 和 any `[Authorize]` 屬性，則 `[Authorize]` 會忽略屬性。</span><span class="sxs-lookup"><span data-stu-id="6f5df-118">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="6f5df-119">例如，如果您在 `[AllowAnonymous]` 控制器層級套用，則 `[Authorize]` 會忽略相同控制器（或其中任何動作）上的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="6f5df-119">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
