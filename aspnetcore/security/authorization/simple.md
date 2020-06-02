---
標題： ASP.NET Core author 中的簡單授權： rick-anderson 描述：瞭解如何使用 authorization 屬性來限制對 ASP.NET Core 控制器和動作的存取。
riande ms. date：10/14/2016 否-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid：安全性/授權/簡單

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
