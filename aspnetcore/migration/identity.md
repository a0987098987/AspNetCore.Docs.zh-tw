---
title: 移轉至 ASP.NET Core 的驗證和身分識別
author: ardalis
description: 了解如何從 ASP.NET MVC 專案中的驗證和身分識別移轉至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2019
ms.locfileid: "41832061"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>移轉至 ASP.NET Core 的驗證和身分識別

作者：[Steve Smith](https://ardalis.com/)

在上一篇文章中，我們[從 ASP.NET MVC 專案的組態移轉到 ASP.NET Core MVC](xref:migration/configuration)。 在本文中，我們可以移轉的註冊、 登入和使用者管理功能。

## <a name="configure-identity-and-membership"></a>設定身分識別與成員資格

在 ASP.NET MVC 中，驗證和身分識別的功能使用設定中的 ASP.NET 身分識別*Startup.Auth.cs*並*IdentityConfig.cs*位於*App_Start*資料夾。 在 ASP.NET Core MVC 中，這些功能會設定於*Startup.cs*。

安裝`Microsoft.AspNetCore.Identity.EntityFrameworkCore`和`Microsoft.AspNetCore.Authentication.Cookies`NuGet 套件。

然後，開啟*Startup.cs* ，並更新`Startup.ConfigureServices`使用 Entity Framework 和身分識別服務的方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

到目前為止，有上述程式碼中，我們還沒有尚未移轉自 ASP.NET MVC 專案中參考的兩種類型：`ApplicationDbContext`和`ApplicationUser`。 建立新*模型*資料夾中的 ASP.NET Core 專案，並將兩個類別新增至其對應至這些型別。 您會找到 ASP.NET MVC 中的這些類別的版本 */Models/IdentityModels.cs*，但因為這是更清楚，我們將使用一個檔案，每個移轉專案中的類別。

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

ASP.NET Core MVC 入門 Web 專案不包含太多自訂的使用者，或`ApplicationDbContext`。 在移轉時實際的應用程式，您也需要移轉的所有自訂屬性和方法的應用程式的使用者和`DbContext`類別，以及您的應用程式會利用任何其他模型類別。 例如，如果您`DbContext`已經`DbSet<Album>`，您需要移轉`Album`類別。

這些檔案的位置， *Startup.cs*檔案可透過更新編譯其`using`陳述式：

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

我們的應用程式現在已準備好支援驗證和身分識別服務的。 它只需要有這些功能公開給使用者。

## <a name="migrate-registration-and-login-logic"></a>移轉註冊和登入的邏輯

設定應用程式的身分識別服務和資料存取使用 Entity Framework 和 SQL Server 設定，我們準備好註冊和登入的支援新增至應用程式。 請記得，[稍早的移轉程序](xref:migration/mvc#migrate-the-layout-file)我們加上註解的參考 *_LoginPartial*中 *_Layout.cshtml*。 現在就可以返回該程式碼，取消註解，並新增必要的控制器和檢視，以支援登入功能中。

取消註解`@Html.Partial`一行 *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

現在，加入新的 Razor 檢視，稱為 *_LoginPartial*要*Views/Shared*資料夾：

更新 *_LoginPartial.cshtml*為下列程式碼 （取代它的所有內容）：

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

此時，您應該能夠重新整理您的瀏覽器中的站台。

## <a name="summary"></a>總結

ASP.NET Core 導入了變更，ASP.NET 身分識別功能。 在本文中，您已看到如何移轉至 ASP.NET Core 的 ASP.NET 身分識別的驗證和使用者管理功能。
