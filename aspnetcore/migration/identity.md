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
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="e185c-103">將驗證和身分識別移轉至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e185c-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="e185c-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e185c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e185c-105">在舊版文章我們[從 ASP.NET MVC 專案的組態移轉到 ASP.NET Core MVC](configuration.md)。</span><span class="sxs-lookup"><span data-stu-id="e185c-105">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="e185c-106">在本文中，我們可以移轉的註冊、 登入和使用者管理功能。</span><span class="sxs-lookup"><span data-stu-id="e185c-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="e185c-107">設定身分識別與成員資格</span><span class="sxs-lookup"><span data-stu-id="e185c-107">Configure Identity and Membership</span></span>

<span data-ttu-id="e185c-108">在 ASP.NET MVC 中，驗證和身分識別的功能是使用 ASP.NET Identity Startup.Auth.cs 和 IdentityConfig.cs，位於 App_Start 資料夾中所設定的。</span><span class="sxs-lookup"><span data-stu-id="e185c-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="e185c-109">在 ASP.NET Core MVC 中，在設定這些功能*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="e185c-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="e185c-110">安裝`Microsoft.AspNetCore.Identity.EntityFrameworkCore`和`Microsoft.AspNetCore.Authentication.Cookies`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e185c-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="e185c-111">然後，開啟 Startup.cs 和 update`ConfigureServices()`使用 Entity Framework 和身分識別服務的方法：</span><span class="sxs-lookup"><span data-stu-id="e185c-111">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="e185c-112">此時，有兩種類型，尚未您尚未從 ASP.NET MVC 專案移轉上述程式碼中參考：`ApplicationDbContext`和`ApplicationUser`。</span><span class="sxs-lookup"><span data-stu-id="e185c-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="e185c-113">建立新*模型*資料夾中 ASP.NET Core 專案，然後將兩個類別加入至其對應至這些型別。</span><span class="sxs-lookup"><span data-stu-id="e185c-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="e185c-114">您可以找到 ASP.NET MVC 中的這些類別的版本`/Models/IdentityModels.cs`，但因為這是更清楚，我們將使用每個類別已移轉的專案中的一個檔案。</span><span class="sxs-lookup"><span data-stu-id="e185c-114">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="e185c-115">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="e185c-115">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="e185c-116">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="e185c-116">ApplicationDbContext.cs:</span></span>

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

<span data-ttu-id="e185c-117">ASP.NET Core MVC 入門 Web 專案不包含太多自訂的使用者或 ApplicationDbContext。</span><span class="sxs-lookup"><span data-stu-id="e185c-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="e185c-118">在移轉時實際的應用程式，您也必須移轉的所有自訂屬性和方法的應用程式的使用者和 DbContext 類別，以及其他任何模型類別 （例如，如果您 DbContext DbSet，會利用您的應用程式<Album>，當然必須移轉專輯類別)。</span><span class="sxs-lookup"><span data-stu-id="e185c-118">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="e185c-119">與就地這些檔案，Startup.cs 檔案，可以藉由編譯藉由更新其使用陳述式：</span><span class="sxs-lookup"><span data-stu-id="e185c-119">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="e185c-120">我們的應用程式現在已準備好支援驗證和身分識別服務，您只需要有這些功能公開給使用者。</span><span class="sxs-lookup"><span data-stu-id="e185c-120">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="e185c-121">移轉註冊和登入邏輯</span><span class="sxs-lookup"><span data-stu-id="e185c-121">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="e185c-122">身分識別設定之服務的應用程式和資料存取使用 Entity Framework 和 SQL Server 設定，我們現在都已準備將支援註冊和登入新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="e185c-122">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="e185c-123">請記得，[移轉程序中稍早](mvc.md#migrate-layout-file)我們標記為註解中 _Layout.cshtml _LoginPartial 的參考。</span><span class="sxs-lookup"><span data-stu-id="e185c-123">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="e185c-124">現在它是傳回至該程式碼，請取消註解，並新增必要的控制器與檢視，以支援登入功能的時間。</span><span class="sxs-lookup"><span data-stu-id="e185c-124">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="e185c-125">更新 _Layout.cshtml;請取消註解@Html.Partial行：</span><span class="sxs-lookup"><span data-stu-id="e185c-125">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="e185c-126">現在，加入新的 MVC 檢視頁面，稱為 _LoginPartial Views/Shared 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e185c-126">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="e185c-127">下列程式碼以更新 _LoginPartial.cshtml （取代其所有內容）：</span><span class="sxs-lookup"><span data-stu-id="e185c-127">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="e185c-128">此時，您應該能夠重新整理您的瀏覽器中的站台。</span><span class="sxs-lookup"><span data-stu-id="e185c-128">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="e185c-129">總結</span><span class="sxs-lookup"><span data-stu-id="e185c-129">Summary</span></span>

<span data-ttu-id="e185c-130">ASP.NET Core 導入了變更，ASP.NET 識別的功能。</span><span class="sxs-lookup"><span data-stu-id="e185c-130">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="e185c-131">在本文中，您已經瞭解如何將 ASP.NET 識別的驗證和使用者管理功能移轉到 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="e185c-131">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
