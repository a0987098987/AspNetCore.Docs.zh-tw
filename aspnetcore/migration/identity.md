---
title: 移轉至 ASP.NET Core 的驗證和身分識別
author: ardalis
description: 了解如何從 ASP.NET MVC 專案中的驗證和身分識別移轉至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891875"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="4fd8c-103">移轉至 ASP.NET Core 的驗證和身分識別</span><span class="sxs-lookup"><span data-stu-id="4fd8c-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="4fd8c-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4fd8c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4fd8c-105">在上一篇文章中，我們[從 ASP.NET MVC 專案的組態移轉到 ASP.NET Core MVC](xref:migration/configuration)。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="4fd8c-106">在本文中，我們可以移轉的註冊、 登入和使用者管理功能。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="4fd8c-107">設定身分識別與成員資格</span><span class="sxs-lookup"><span data-stu-id="4fd8c-107">Configure Identity and Membership</span></span>

<span data-ttu-id="4fd8c-108">在 ASP.NET MVC 中，驗證和身分識別的功能使用設定中的 ASP.NET 身分識別*Startup.Auth.cs*並*IdentityConfig.cs*位於*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="4fd8c-109">在 ASP.NET Core MVC 中，這些功能會設定於*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="4fd8c-110">安裝`Microsoft.AspNetCore.Identity.EntityFrameworkCore`和`Microsoft.AspNetCore.Authentication.Cookies`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="4fd8c-111">然後，開啟*Startup.cs* ，並更新`Startup.ConfigureServices`使用 Entity Framework 和身分識別服務的方法：</span><span class="sxs-lookup"><span data-stu-id="4fd8c-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="4fd8c-112">到目前為止，有上述程式碼中，我們還沒有尚未移轉自 ASP.NET MVC 專案中參考的兩種類型：`ApplicationDbContext`和`ApplicationUser`。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="4fd8c-113">建立新*模型*資料夾中的 ASP.NET Core 專案，並將兩個類別新增至其對應至這些型別。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="4fd8c-114">您會找到 ASP.NET MVC 中的這些類別的版本 */Models/IdentityModels.cs*，但因為這是更清楚，我們將使用一個檔案，每個移轉專案中的類別。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="4fd8c-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fd8c-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="4fd8c-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4fd8c-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="4fd8c-117">ASP.NET Core MVC 入門 Web 專案不包含太多自訂的使用者，或`ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="4fd8c-118">在移轉時實際的應用程式，您也需要移轉的所有自訂屬性和方法的應用程式的使用者和`DbContext`類別，以及您的應用程式會利用任何其他模型類別。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="4fd8c-119">例如，如果您`DbContext`已經`DbSet<Album>`，您需要移轉`Album`類別。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="4fd8c-120">這些檔案的位置， *Startup.cs*檔案可透過更新編譯其`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="4fd8c-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="4fd8c-121">我們的應用程式現在已準備好支援驗證和身分識別服務的。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="4fd8c-122">它只需要有這些功能公開給使用者。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="4fd8c-123">移轉註冊和登入的邏輯</span><span class="sxs-lookup"><span data-stu-id="4fd8c-123">Migrate registration and login logic</span></span>

<span data-ttu-id="4fd8c-124">設定應用程式的身分識別服務和資料存取使用 Entity Framework 和 SQL Server 設定，我們準備好註冊和登入的支援新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="4fd8c-125">請記得，[稍早的移轉程序](xref:migration/mvc#migrate-the-layout-file)我們加上註解的參考 *_LoginPartial*中 *_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="4fd8c-126">現在就可以返回該程式碼，取消註解，並新增必要的控制器和檢視，以支援登入功能中。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="4fd8c-127">取消註解`@Html.Partial`一行 *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4fd8c-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="4fd8c-128">現在，加入新的 Razor 檢視，稱為 *_LoginPartial*要*Views/Shared*資料夾：</span><span class="sxs-lookup"><span data-stu-id="4fd8c-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="4fd8c-129">更新 *_LoginPartial.cshtml*為下列程式碼 （取代它的所有內容）：</span><span class="sxs-lookup"><span data-stu-id="4fd8c-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="4fd8c-130">此時，您應該能夠重新整理您的瀏覽器中的站台。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="4fd8c-131">總結</span><span class="sxs-lookup"><span data-stu-id="4fd8c-131">Summary</span></span>

<span data-ttu-id="4fd8c-132">ASP.NET Core 導入了變更，ASP.NET 身分識別功能。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="4fd8c-133">在本文中，您已看到如何移轉至 ASP.NET Core 的 ASP.NET 身分識別的驗證和使用者管理功能。</span><span class="sxs-lookup"><span data-stu-id="4fd8c-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
