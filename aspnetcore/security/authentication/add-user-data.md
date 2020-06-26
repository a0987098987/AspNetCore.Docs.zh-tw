---
title: 在 ASP.NET Core 專案中新增、下載及刪除使用者資料 Identity
author: rick-anderson
description: 瞭解如何 Identity 在 ASP.NET Core 專案中將自訂使用者資料新增至。 刪除每個 GDPR 的資料。
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/add-user-data
ms.openlocfilehash: e5c23cc49a52b8772a43853e9e953dd416d69f69
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408730"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>在 ASP.NET Core 專案中加入、下載和刪除自訂使用者資料 Identity

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文將說明如何：

* 將自訂使用者資料新增至 ASP.NET Core web 應用程式。
* 將自訂使用者資料模型標記為 <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> 屬性，使其自動可供下載和刪除。 讓資料能夠下載和刪除有助於符合[GDPR](xref:security/gdpr)需求。

專案範例是從 Razor 頁面 web 應用程式建立的，但這些指示類似于 ASP.NET CORE MVC web 應用程式。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="prerequisites"></a>必要條件

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>建立 Razor web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**** > [專案]**** 。 如果您想要符合[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。
* 選取 [ **ASP.NET Core Web 應用程式** > **正常]**
* 選取下拉式清單中的 [ **ASP.NET Core 3.0** ]
* 選取**Web 應用程式** > **正常**
* 建置並執行專案。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**** > [專案]**** 。 如果您想要符合[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。
* 選取 [ **ASP.NET Core Web 應用程式** > **正常]**
* 選取下拉式清單中的 [ **ASP.NET Core 2.2** ]
* 選取**Web 應用程式** > **正常**
* 建置並執行專案。

::: moniker-end


# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>執行 Identity scaffolder

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在**方案總管**中，以滑鼠右鍵按一下專案 > [**加入**  >  **新的 scaffold 專案**]。
* 從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [ **Identity**  >  **新增**]。
* 在 [**新增 Identity ** ] 對話方塊中，有下列選項：
  * 選取現有的版面配置檔案 *~/Pages/Shared/_Layout. cshtml*
  * 選取要覆寫的下列檔案：
    * **帳戶/註冊**
    * **帳戶/管理/索引**
  * 選取 [] **+** 按鈕以建立新的**資料內容類別**。 如果專案名為**WebApp1**，請接受類型（**WebApp1. WebApp1CoNtext** ）。
  * 選取 [] **+** 按鈕以建立新的**使用者類別**。 接受類型（如果專案名為**WebApp1**，則為**WebApp1User** ） >**新增**]。
* 選取 [新增]****。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

將 Nswag.codegeneration.csharp 的套件參考新增至專案（.csproj）檔案。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) ）。 在專案目錄中執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令以列出 Identity scaffolder 選項：

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中，執行 Identity scaffolder：

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

依照 [遷移] [、[UseAuthentication] 和 [版面](xref:security/authentication/scaffold-identity#efm)配置] 中的指示執行下列步驟：

* 建立遷移並更新資料庫。
* 將 `UseAuthentication` 新增至 `Startup.Configure`。
* 將加入至配置檔案 `<partial name="_LoginPartial" />` 。
* 測試應用程式：
  * 註冊使用者
  * 選取新的使用者名稱（[**登出**] 連結旁）。 您可能需要展開視窗，或選取 [導覽列] 圖示，以顯示使用者名稱和其他連結。
  * 選取 [**個人資料**] 索引標籤。
  * 選取 [**下載**] 按鈕，並*在檔案上檢查PersonalData.js* 。
  * 測試**刪除**按鈕，這會刪除已登入的使用者。

## <a name="add-custom-user-data-to-the-identity-db"></a>將自訂使用者資料新增至 Identity 資料庫

`IdentityUser`以自訂屬性更新衍生類別。 如果您將專案命名為 WebApp1，檔案就會命名為*Areas/ Identity /Data/WebApp1User.cs*。 使用下列程式碼更新檔案：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

具有[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)屬性的屬性如下：

* 當*區域/ Identity /Pages/Account/Manage/DeletePersonalData.cshtml* Razor 頁面呼叫時刪除 `UserManager.Delete` 。
* 包含在已下載資料中的 [*區域/ Identity /Pages/Account/Manage/DownloadPersonalData.cshtml* ] Razor 頁面。

### <a name="update-the-accountmanageindexcshtml-page"></a>更新 [帳戶/管理/索引. cshtml] 頁面

`InputModel`使用下列反白顯示的程式碼，更新*區域/ Identity /Pages/Account/Manage/Index.cshtml.cs*中的：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Manage/Index.cshtml* ：

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Manage/Index.cshtml* ：

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>更新 [帳戶/註冊. cshtml] 頁面

`InputModel`使用下列反白顯示的程式碼，更新*區域/ Identity /Pages/Account/Register.cshtml.cs*中的：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Register.cshtml* ：

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

使用下列反白顯示的標記來更新*區域/ Identity /Pages/Account/Register.cshtml* ：

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


建置專案。

### <a name="add-a-migration-for-the-custom-user-data"></a>新增自訂使用者資料的遷移

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [Visual Studio**套件管理員主控台**] 中：

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>測試建立、查看、下載、刪除自訂使用者資料

測試應用程式：

* 註冊新的使用者。
* 在頁面上，查看自訂使用者資料 `/Identity/Account/Manage` 。
* 從頁面下載並查看使用者的個人資料 `/Identity/Account/Manage/PersonalData` 。

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a>使用 IUserClaimsPrincipalFactory 將宣告新增至 Identity<ApplicationUser>

> [!NOTE]
> 本章節不是上一個教學課程的延伸。 若要將下列步驟套用至使用教學課程所建立的應用程式，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/18797)。

您可以使用介面，將額外的宣告新增至 ASP.NET Core Identity `IUserClaimsPrincipalFactory<T>` 。 這個類別可以新增至方法中的應用程式 `Startup.ConfigureServices` 。 新增類別的自訂執行，如下所示：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

示範程式碼會使用 `ApplicationUser` 類別。 這個類別會加入 `IsAdmin` 用來新增額外宣告的屬性。

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

`AdditionalUserClaimsPrincipalFactory` 會實作 `UserClaimsPrincipalFactory` 介面。 新的角色宣告會加入至 `ClaimsPrincipal` 。

```csharp
public class AdditionalUserClaimsPrincipalFactory 
        : UserClaimsPrincipalFactory<ApplicationUser, IdentityRole>
{
    public AdditionalUserClaimsPrincipalFactory( 
        UserManager<ApplicationUser> userManager,
        RoleManager<IdentityRole> roleManager, 
        IOptions<IdentityOptions> optionsAccessor) 
        : base(userManager, roleManager, optionsAccessor)
    {}

    public async override Task<ClaimsPrincipal> CreateAsync(ApplicationUser user)
    {
        var principal = await base.CreateAsync(user);
        var identity = (ClaimsIdentity)principal.Identity;

        var claims = new List<Claim>();
        if (user.IsAdmin)
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "admin"));
        }
        else
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "user"));
        }

        identity.AddClaims(claims);
        return principal;
    }
}
```

接著，您可以在應用程式中使用額外的宣告。 在 Razor 頁面中， `IAuthorizationService` 可以使用實例來存取宣告值。

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@if ((await AuthorizationService.AuthorizeAsync(User, "IsAdmin")).Succeeded)
{
    <ul class="mr-auto navbar-nav">
        <li class="nav-item">
            <a class="nav-link" asp-controller="Admin" asp-action="Index">ADMIN</a>
        </li>
    </ul>
}
```
