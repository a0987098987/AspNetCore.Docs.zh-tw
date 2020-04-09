---
title: 在 ASP.NET 核心專案中向識別新增、下載和刪除使用者資料
author: rick-anderson
description: 瞭解如何在ASP.NET核心專案中向標識添加自定義用戶數據。 刪除每個 GDPR 的數據。
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 76b83df22381429feab80056c36dbdac1e5f20c7
ms.sourcegitcommit: 1d8f1396ccc66a0c3fcb5e5f36ea29b50db6d92a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501222"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>在 ASP.NET 核心項目中將自訂使用者資料新增、下載和刪除為識別

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文將說明如何：

* 將自定義用戶數據添加到ASP.NET核心 Web 應用。
* 使用<xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>屬性標記自定義用戶數據模型,以便它自動可供下載和刪除。 使數據能夠下載和刪除有助於滿足[GDPR](xref:security/gdpr)要求。

專案示例是從 Razor Pages Web 應用創建的,但 ASP.NET 核心 MVC Web 應用的說明類似。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data)([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**** > [專案]**** 。 如果要命名專案**WebApp1,** 請將其與[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)代碼的命名空間匹配。
* 選擇**ASP.NET 核心 Web 應用程式**>**確定**
* 在下拉清單中**選擇ASP.NET核心 3.0**
* 選擇**Web 應用程式**>**確定**
* 建置並執行專案。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**** > [專案]**** 。 如果要命名專案**WebApp1,** 請將其與[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)代碼的命名空間匹配。
* 選擇**ASP.NET 核心 Web 應用程式**>**確定**
* 選擇 ASP.NET核心**2.2**
* 選擇**Web 應用程式**>**確定**
* 建置並執行專案。

::: moniker-end


# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>執行識別基架

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**解決方案資源管理員**中,右鍵按一次專案>**新增新** > **的文手架項目**。
* 從 **「添加基架」** 對話框的左邊窗格中,選擇 **「標識** > **添加**」 。
* 在 **'新增識別'** 對話框中, 以下選項:
  * 選擇現有佈局檔 **/頁面/共用/_Layout.cshtml*
  * 選擇要覆寫的以下檔案:
    * **帳戶/註冊**
    * **帳號/管理/索引**
  * 選擇按鈕**+** 建立新**的資料內容 。** 接受類型 **(WebApp1.模型.WebApp1上下文**,如果專案名為**WebApp1**)。
  * 選擇按鈕**+** 建立新**的使用者類別**。 接受類型(如果專案名為**WebApp1)>****添加**, 則接受**WebApp1User。**
* 選取 [新增]  。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果以前未安裝ASP.NET核心基架,請立即安裝:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

向[Microsoft.VisualStudio.Web.CodeGeneration.設計](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)添加對 Microsoft 的包引用(.csproj)檔。 在項目目錄中執行以下指令:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行以下指令以列出識別基架選項:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

在項目資料夾中,執行識別基架:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

依[移動、使用認證與佈局](xref:security/authentication/scaffold-identity#efm)中的說明執行以下步驟:

* 創建遷移並更新資料庫。
* 將 `UseAuthentication` 新增至 `Startup.Configure`。
* 添加到`<partial name="_LoginPartial" />`佈局檔。
* 測試應用程式：
  * 註冊使用者
  * 選擇新的使用者名稱(**註銷**連結旁邊)。 您可能需要展開視窗或選擇導航列圖示以顯示使用者名和其他連結。
  * 選擇 **『個人資料**』選項卡。
  * 選擇 **「下載**」按鈕並檢查*個人資料.json*檔。
  * 測試 **「刪除**」按鈕,該按鈕將刪除登錄的使用者。

## <a name="add-custom-user-data-to-the-identity-db"></a>將自訂使用者資料加入到識別資料庫

使用自定義`IdentityUser`屬性更新派生類。 如果命名專案 WebApp1,則檔名為 *"區域/標識/資料/WebApp1User.cs*"。 使用以下代碼更新檔案:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

具有[「個人資料」](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)屬性的屬性包括:

* 刪除時*的區域/身份/頁面/帳戶/管理/刪除個人資料.cshtml*剃刀頁面呼`UserManager.Delete`叫 。
* 包含在下載的數據由*區域/身份/頁面/帳戶/管理/下載個人數據.cshtml*剃刀頁面。

### <a name="update-the-accountmanageindexcshtml-page"></a>更新帳號/管理/索引.cshtml 頁面

使用以下`InputModel`突顯的代碼更新*區域/識別/頁面/帳戶/管理/索引.cshtml.cs:*

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

使用以下突顯的標記更新*區域/識別/頁面/帳戶/管理/索引.cshtml:*

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

使用以下突顯的標記更新*區域/識別/頁面/帳戶/管理/索引.cshtml:*

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>更新帳號/註冊.cshtml 頁面

使用以下`InputModel`突顯的代碼更新*區域/識別/頁面/帳戶/註冊.cshtml.cs:*

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

使用以下突顯的標記更新*區域/標識/頁面/帳戶/註冊.cshtml:*

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

使用以下突顯的標記更新*區域/標識/頁面/帳戶/註冊.cshtml:*

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


建置專案。

### <a name="add-a-migration-for-the-custom-user-data"></a>為自訂使用者資料加入移轉

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在視覺化工作室**套件管理員控制台中**:

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

## <a name="test-create-view-download-delete-custom-user-data"></a>測試建立、檢視、下載、刪除自訂使用者資料

測試應用程式：

* 註冊新使用者。
* 查看頁面上的`/Identity/Account/Manage`自定義用戶數據。
* 從`/Identity/Account/Manage/PersonalData`頁面下載並查看使用者的個人數據。

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a>使用 IUserClaims 主要工廠向識別新增聲明<ApplicationUser>

可以使用`IUserClaimsPrincipalFactory<T>`介面將其他聲明添加到ASP.NET核心標識。 類可以添加到`Startup.ConfigureServices`方法中的應用。 將類的自定義實現添加如下:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

展示程式碼使用`ApplicationUser`類別 。 此類添加用於`IsAdmin`添加附加聲明的屬性。

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

`AdditionalUserClaimsPrincipalFactory` 會實作 `UserClaimsPrincipalFactory` 介面。 新角色宣告將新增到`ClaimsPrincipal`。

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

然後,可以在應用中使用其他聲明。 在 Razor`IAuthorizationService`頁中 ,實例可用於訪問聲明值。

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
