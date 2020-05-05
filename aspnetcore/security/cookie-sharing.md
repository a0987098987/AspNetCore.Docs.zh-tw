---
title: 在 ASP.NET apps 之間共用驗證 cookie
author: rick-anderson
description: 瞭解如何在 ASP.NET 4.x 和 ASP.NET Core 應用程式之間共用驗證 cookie。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/cookie-sharing
ms.openlocfilehash: a6aac53008e634e87b18bd34b3d2babdad74ae50
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776138"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>在 ASP.NET apps 之間共用驗證 cookie

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

網站通常包含個別的 web 應用程式一起運作。 若要提供單一登入（SSO）體驗，網站內的 web 應用程式必須共用驗證 cookie。 為了支援此案例，資料保護堆疊允許共用 Katana cookie 驗證，並 ASP.NET Core cookie 驗證票證。

在下列範例中：

* 驗證 cookie 名稱會設定為的通用值`.AspNet.SharedCookie`。
* `AuthenticationType`會明確或依`Identity.Application`預設設定為。
* 通用應用程式名稱是用來讓資料保護系統共用資料保護金鑰（`SharedCookieApp`）。
* `Identity.Application`是用來做為驗證配置。 無論使用哪種配置，都必須以一致的方式在共用的 cookie 應用程式*中*使用，或是透過明確地設定它。 配置會用於加密和解密 cookie，因此必須跨應用程式使用一致的配置。
* 使用的是通用[資料保護金鑰](xref:security/data-protection/implementation/key-management)儲存位置。
  * 在 ASP.NET Core 應用程式<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>中，是用來設定金鑰儲存位置。
  * 在 .NET Framework 應用程式中，Cookie 驗證中介軟體會<xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>使用的執行。 `DataProtectionProvider`提供用於加密和解密驗證 cookie 承載資料的資料保護服務。 `DataProtectionProvider`實例會與其他應用程式元件所使用的資料保護系統隔離。 [DataProtectionProvider （DirectoryInfo，Action\<IDataProtectionBuilder>）](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*)會接受<xref:System.IO.DirectoryInfo>來指定資料保護金鑰儲存的位置。
* `DataProtectionProvider`需要[AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)的 NuGet 套件：
  * 在 ASP.NET Core 2.x 應用程式中，參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。
  * 在 .NET Framework 應用程式中，將套件參考新增至[AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)。
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>設定一般應用程式名稱。

## <a name="share-authentication-cookies-with-aspnet-core-identity"></a>與 ASP.NET Core 共用驗證 cookieIdentity

使用 ASP.NET Core Identity時：

* 資料保護金鑰和應用程式名稱必須在應用程式之間共用。 在下列範例中，會將一般金鑰<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>儲存位置提供給方法。 使用<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>來設定通用共用應用程式名稱（`SharedCookieApp`在下列範例中為）。 如需詳細資訊，請參閱<xref:security/data-protection/configuration/overview>。
* 使用<xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*>擴充方法來設定 cookie 的資料保護服務。
* 預設驗證類型為`Identity.Application`。

在 `Startup.ConfigureServices` 中：

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

## <a name="share-authentication-cookies-without-aspnet-core-identity"></a>共用驗證 cookie 而不 ASP.NET CoreIdentity

直接使用沒有 ASP.NET Core Identity的 cookie 時，請在中`Startup.ConfigureServices`設定資料保護和驗證。 在下列範例中，驗證類型設定為`Identity.Application`：

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

## <a name="share-cookies-across-different-base-paths"></a>跨不同的基底路徑共用 cookie

驗證 cookie 會使用[HttpRequest. PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase)作為其預設[cookie。路徑](xref:Microsoft.AspNetCore.Http.CookieBuilder.Path)。 如果應用程式的 cookie 必須在不同的基底路徑之間`Path`共用，則必須覆寫：

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
    options.Cookie.Path = "/";
});
```

## <a name="share-cookies-across-subdomains"></a>跨子域共用 cookie

在裝載跨子域共用 cookie 的應用程式時，請在 [ [Cookie. domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) ] 屬性中指定一個通用網域。 若要在上`contoso.com`跨應用程式共用 cookie `first_subdomain.contoso.com` （ `second_subdomain.contoso.com`例如和） `Cookie.Domain` ， `.contoso.com`請將指定為：

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>加密待用資料保護金鑰

針對生產環境部署，請`DataProtectionProvider`將設定為使用 DPAPI 或 X509Certificate 來加密靜止的金鑰。 如需詳細資訊，請參閱<xref:security/data-protection/implementation/key-encryption-at-rest>。 在下列範例中，會將憑證指紋提供給<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>：

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>共用 ASP.NET 4.x 和 ASP.NET Core 應用程式之間的驗證 cookie

您可以設定使用 Katana Cookie 驗證中介軟體的 ASP.NET 4.x 應用程式，以產生與 ASP.NET Core Cookie 驗證中介軟體相容的驗證 cookie。 這可讓您在多個步驟中升級大型網站的個別應用程式，同時在網站上提供順暢的 SSO 體驗。

當應用程式使用 Katana Cookie 驗證中介軟體時， `UseCookieAuthentication`它會在專案的*Startup.Auth.cs*檔中呼叫。 使用 Visual Studio 2013 和更新版本建立的 ASP.NET 4.x web 應用程式專案預設會使用 Katana Cookie 驗證中介軟體。 雖然`UseCookieAuthentication`已過時且不支援 ASP.NET Core 應用程式， `UseCookieAuthentication`但在使用 Katana Cookie 驗證中介軟體的 ASP.NET 4.x 應用程式中呼叫是有效的。

ASP.NET 4.x 應用程式必須以 .NET Framework 4.5.1 或更新版本為目標。 否則，就無法安裝必要的 NuGet 套件。

若要在 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間共用驗證 cookie，請依照在[ASP.NET Core apps 之間共用驗證 cookie](#share-authentication-cookies-with-aspnet-core-identity)一節中所述的方式設定 ASP.NET Core 應用程式，然後設定 ASP.NET 4.x 應用程式，如下所示。

確認應用程式的套件已更新為最新版本。 將[Owin](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)套件安裝到每個 ASP.NET 4.x 應用程式。

找出並修改對的`UseCookieAuthentication`呼叫：

* 變更 cookie 名稱，使其符合 ASP.NET Core Cookie 驗證中介軟體所使用的名稱`.AspNet.SharedCookie` （在此範例中為）。
* 在下列範例中，驗證類型設定為`Identity.Application`。
* 提供已`DataProtectionProvider`初始化的實例給通用資料保護金鑰儲存位置。
* 確認應用程式名稱已設定為所有共用驗證 cookie 之應用程式所使用的通用應用程式名稱`SharedCookieApp` （在此範例中為）。

如果未設定`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`和`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`，請<xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier>將設定為可區別唯一使用者的宣告。

*App_Start/startup.auth.cs*：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create("{PATH TO COMMON KEY RING FOLDER}",
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

產生使用者識別時，驗證`Identity.Application`類型（）必須符合在*App_Start/startup.auth.cs* `UseCookieAuthentication`中以 set `AuthenticationType`中定義的類型。

*模型/IdentityModels .cs*：

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a>使用一般使用者資料庫

當應用程式使用相同Identity的架構（相同版本Identity的）時，請Identity確認每個應用程式的系統都指向相同的使用者資料庫。 否則，當身分識別系統嘗試比對驗證 cookie 中的資訊與其資料庫中的資訊時，會在執行時間產生失敗。

當應用Identity程式之間的架構不同時，通常是因為應用程式Identity使用不同的版本，在沒有重新對應並在其他Identity應用程式Identity架構中新增資料行的情況下，不可能以最新版本共用通用資料庫。 將其他應用程式升級為使用最新Identity版本，讓應用程式可以共用通用資料庫，通常會更有效率。

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/web-farm>
