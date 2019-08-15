---
title: 將驗證和身分識別遷移至 ASP.NET Core
author: ardalis
description: 瞭解如何將驗證和身分識別從 ASP.NET MVC 專案遷移至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: f821930dbd36de18db31104cddf34c563009a506
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022274"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="c427f-103">將驗證和身分識別遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c427f-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="c427f-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c427f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c427f-105">在前一篇文章中, 我們將設定[從 ASP.NET mvc 專案遷移至 ASP.NET CORE mvc](xref:migration/configuration)。</span><span class="sxs-lookup"><span data-stu-id="c427f-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="c427f-106">在本文中, 我們會遷移註冊、登入和使用者管理功能。</span><span class="sxs-lookup"><span data-stu-id="c427f-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="c427f-107">設定身分識別和成員資格</span><span class="sxs-lookup"><span data-stu-id="c427f-107">Configure Identity and Membership</span></span>

<span data-ttu-id="c427f-108">在 ASP.NET MVC 中, 驗證和身分識別功能是使用*Startup.Auth.cs*和*IdentityConfig.cs*中的 ASP.NET Identity (位於*App_Start*資料夾) 來設定。</span><span class="sxs-lookup"><span data-stu-id="c427f-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="c427f-109">在 ASP.NET Core MVC 中, 這些功能會在*Startup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="c427f-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="c427f-110">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`安裝和`Microsoft.AspNetCore.Authentication.Cookies` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="c427f-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="c427f-111">然後, 開啟*Startup.cs*並更新`Startup.ConfigureServices`方法, 以使用 Entity Framework 和身分識別服務:</span><span class="sxs-lookup"><span data-stu-id="c427f-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="c427f-112">此時, 上述程式碼中已參考了兩種類型, 我們尚未從 ASP.NET MVC 專案進行遷移: `ApplicationDbContext`和。 `ApplicationUser`</span><span class="sxs-lookup"><span data-stu-id="c427f-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="c427f-113">在 ASP.NET Core 專案中建立新的 [*模型*] 資料夾, 並在其中加入對應至這些類型的兩個類別。</span><span class="sxs-lookup"><span data-stu-id="c427f-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="c427f-114">您會在 */Models/IdentityModels.cs*中找到這些類別的 ASP.NET MVC 版本, 但在遷移的專案中, 每個類別都會使用一個檔案, 因為這更為清楚。</span><span class="sxs-lookup"><span data-stu-id="c427f-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="c427f-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="c427f-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="c427f-116">*ApplicationDbCoNtext.cs*:</span><span class="sxs-lookup"><span data-stu-id="c427f-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
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

<span data-ttu-id="c427f-117">ASP.NET Core MVC Starter Web 專案不包含許多使用者自訂, 或`ApplicationDbContext`。</span><span class="sxs-lookup"><span data-stu-id="c427f-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="c427f-118">在遷移實際的應用程式時, 您也需要遷移應用程式使用者和`DbContext`類別的所有自訂屬性和方法, 以及應用程式所使用的任何其他模型類別。</span><span class="sxs-lookup"><span data-stu-id="c427f-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="c427f-119">例如, 如果您`DbContext`的`DbSet<Album>`具有, `Album`則需要遷移類別。</span><span class="sxs-lookup"><span data-stu-id="c427f-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="c427f-120">這些檔案都備妥之後,就可以更新`using` Startup.cs 檔案的語句來進行編譯:</span><span class="sxs-lookup"><span data-stu-id="c427f-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="c427f-121">我們的應用程式現在已準備好支援驗證和身分識別服務。</span><span class="sxs-lookup"><span data-stu-id="c427f-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="c427f-122">這只需要將這些功能公開給使用者。</span><span class="sxs-lookup"><span data-stu-id="c427f-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="c427f-123">遷移註冊和登入邏輯</span><span class="sxs-lookup"><span data-stu-id="c427f-123">Migrate registration and login logic</span></span>

<span data-ttu-id="c427f-124">針對使用 Entity Framework 和 SQL Server 設定的應用程式和資料存取所設定的身分識別服務, 我們已準備好新增對應用程式註冊和登入的支援。</span><span class="sxs-lookup"><span data-stu-id="c427f-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="c427f-125">回想一下, 稍[早在遷移程式中](xref:migration/mvc#migrate-the-layout-file), 我們已在 *_Layout*中將 *_LoginPartial*的參考批註化。</span><span class="sxs-lookup"><span data-stu-id="c427f-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="c427f-126">現在可以回到該程式碼, 將它取消批註, 然後加入必要的控制器和視圖, 以支援登入功能。</span><span class="sxs-lookup"><span data-stu-id="c427f-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="c427f-127">取消批註`@Html.Partial` *_Layout*中的那一行:</span><span class="sxs-lookup"><span data-stu-id="c427f-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="c427f-128">現在, 將名為 *_LoginPartial*的新 Razor view 新增至*Views/Shared*資料夾:</span><span class="sxs-lookup"><span data-stu-id="c427f-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="c427f-129">使用下列程式碼更新 *_LoginPartial* (取代其所有內容):</span><span class="sxs-lookup"><span data-stu-id="c427f-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="c427f-130">此時, 您應該能夠在瀏覽器中重新整理網站。</span><span class="sxs-lookup"><span data-stu-id="c427f-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="c427f-131">總結</span><span class="sxs-lookup"><span data-stu-id="c427f-131">Summary</span></span>

<span data-ttu-id="c427f-132">ASP.NET Core 引進 ASP.NET Identity 功能的變更。</span><span class="sxs-lookup"><span data-stu-id="c427f-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="c427f-133">在本文中, 您已瞭解如何將 ASP.NET Identity 的驗證和使用者管理功能遷移至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="c427f-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
