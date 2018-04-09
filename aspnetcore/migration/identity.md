---
title: 將驗證和身分識別移轉至 ASP.NET Core
author: ardalis
description: 了解如何將驗證和身分識別從 ASP.NET MVC 專案移轉至 ASP.NET Core MVC 專案。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 320f5e079316114832e639d62c780a0639df0c61
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>將驗證和身分識別移轉至 ASP.NET Core

<a name="migration-identity"></a>

作者：[Steve Smith](https://ardalis.com/)

在舊版文章我們[從 ASP.NET MVC 專案的組態移轉到 ASP.NET Core MVC](configuration.md)。 在本文中，我們可以移轉的註冊、 登入和使用者管理功能。

## <a name="configure-identity-and-membership"></a>設定身分識別與成員資格

在 ASP.NET MVC 中，驗證和身分識別的功能是使用 ASP.NET Identity Startup.Auth.cs 和 IdentityConfig.cs，位於 App_Start 資料夾中所設定的。 在 ASP.NET Core MVC 中，在設定這些功能*Startup.cs*。

安裝`Microsoft.AspNetCore.Identity.EntityFrameworkCore`和`Microsoft.AspNetCore.Authentication.Cookies`NuGet 封裝。

然後，開啟 Startup.cs 和 update`ConfigureServices()`使用 Entity Framework 和身分識別服務的方法：

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

此時，有兩種類型，尚未您尚未從 ASP.NET MVC 專案移轉上述程式碼中參考：`ApplicationDbContext`和`ApplicationUser`。 建立新*模型*資料夾中 ASP.NET Core 專案，然後將兩個類別加入至其對應至這些型別。 您可以找到 ASP.NET MVC 中的這些類別的版本`/Models/IdentityModels.cs`，但因為這是更清楚，我們將使用每個類別已移轉的專案中的一個檔案。

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

ASP.NET Core MVC 入門 Web 專案不包含太多自訂的使用者或 ApplicationDbContext。 在移轉時實際的應用程式，您也必須移轉的所有自訂屬性和方法的應用程式的使用者和 DbContext 類別，以及其他任何模型類別 （例如，如果您 DbContext DbSet，會利用您的應用程式<Album>，當然必須移轉專輯類別)。

與就地這些檔案，Startup.cs 檔案，可以藉由編譯藉由更新其使用陳述式：

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

我們的應用程式現在已準備好支援驗證和身分識別服務，您只需要有這些功能公開給使用者。

## <a name="migrate-registration-and-login-logic"></a>移轉註冊和登入邏輯

身分識別設定之服務的應用程式和資料存取使用 Entity Framework 和 SQL Server 設定，我們現在都已準備將支援註冊和登入新增至應用程式。 請記得，[移轉程序中稍早](mvc.md#migrate-layout-file)我們標記為註解中 _Layout.cshtml _LoginPartial 的參考。 現在它是傳回至該程式碼，請取消註解，並新增必要的控制器與檢視，以支援登入功能的時間。

更新 _Layout.cshtml;請取消註解@Html.Partial行：

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

現在，加入新的 MVC 檢視頁面，稱為 _LoginPartial Views/Shared 資料夾：

下列程式碼以更新 _LoginPartial.cshtml （取代其所有內容）：

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
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

ASP.NET Core 導入了變更，ASP.NET 識別的功能。 在本文中，您已經瞭解如何將 ASP.NET 識別的驗證和使用者管理功能移轉到 ASP.NET Core。
