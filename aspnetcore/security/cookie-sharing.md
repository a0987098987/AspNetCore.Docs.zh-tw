---
title: 在 ASP.NET 應用程式之間共用的驗證 cookie
author: rick-anderson
description: 了解如何共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223915"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>在 ASP.NET 應用程式之間共用的驗證 cookie

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

網站通常會組成個別的 web 應用程式一起使用。 若要提供單一登入 (SSO) 體驗，在站台內的 web 應用程式必須共用驗證 cookie。 若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。

在接下來的範例：

* 驗證 cookie 名稱設定為通用值的`.AspNet.SharedCookie`。
* `AuthenticationType`設為`Identity.Application`明確或預設。
* 常見的應用程式名稱用來啟用共用的資料保護金鑰的資料保護系統 (`SharedCookieApp`)。
* `Identity.Application` 做為驗證配置。 任何配置，則必須以一致的方式使用*內和跨*共用的 cookie 的應用程式做為預設配置，或藉由明確地設定它。 加密和解密 cookie，因此必須跨應用程式使用一致的結構描述時，會使用配置。
* 通用[的資料保護金鑰](xref:security/data-protection/implementation/key-management)會使用儲存體位置。
  * 在 ASP.NET Core 應用程式，<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>用來設定金鑰儲存位置。
  * 在.NET Framework 應用程式中，Cookie 驗證中介軟體會使用實作<xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>。 `DataProtectionProvider` 提供的加密和解密驗證 cookie 的內容資料的資料保護服務。 `DataProtectionProvider`是分開的應用程式其他部分所使用的資料保護系統的執行個體。 [DataProtectionProvider.Create (System.IO.DirectoryInfo、 動作\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*)接受<xref:System.IO.DirectoryInfo>來指定資料保護的金鑰存放區的位置。
* `DataProtectionProvider` 需要[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet 套件：
  * 在 ASP.NET Core 2.x 應用程式，可以參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。
  * 在.NET Framework 應用程式，將新增的套件參考[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)。
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> 設定一般的應用程式名稱。

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>共用 ASP.NET Core 應用程式之間的驗證 cookie

當使用 ASP.NET Core 身分識別：

* 資料保護金鑰和應用程式名稱必須在應用程式間共用。 常見的金鑰儲存位置提供給<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>在下列範例中的方法。 使用<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>若要設定共用的應用程式的一般名稱 (`SharedCookieApp`在下列範例中)。 如需詳細資訊，請參閱 <xref:security/data-protection/configuration/overview>。
* 使用<xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*>設定 cookie 的資料保護服務的擴充方法。
* 在下列範例中，驗證類型設為`Identity.Application`預設。

在 `Startup.ConfigureServices`中：

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

當使用直接沒有 ASP.NET Core 身分識別的 cookie，設定資料保護和驗證`Startup.ConfigureServices`。 在下列範例中，驗證類型設為`Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

當裝載子網域之間共用的 cookie 的應用程式中，指定一般的網域中[Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain)屬性。 在 應用程式之間共用 cookie `contoso.com`，這類`first_subdomain.contoso.com`並`second_subdomain.contoso.com`，指定`Cookie.Domain`做為`.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>加密待用資料保護金鑰

針對生產環境部署，設定`DataProtectionProvider`來加密 DPAPI 或 X509Certificate 待用的金鑰。 如需詳細資訊，請參閱 <xref:security/data-protection/implementation/key-encryption-at-rest>。 在下列範例中，憑證指紋提供給<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>驗證間共用 cookie ASP.NET 4.x 和 ASP.NET Core 應用程式

使用 Katana Cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生與 ASP.NET Core Cookie 驗證中介軟體相容的驗證 cookie 中。 這可讓升級個別的應用程式的大型網站在幾個步驟中，同時跨站台提供順暢的 SSO 體驗。

當應用程式會使用 Katana Cookie 驗證中介軟體時，它會呼叫`UseCookieAuthentication`中的專案*Startup.Auth.cs*檔案。 ASP.NET 4.x web 應用程式專案使用 Visual Studio 2013 建立和更新版本預設會使用 Katana Cookie 驗證中介軟體。 雖然`UseCookieAuthentication`; 已過時，不支援 ASP.NET Core 應用程式，呼叫`UseCookieAuthentication`中 ASP.NET 4.x 應用程式使用 Katana Cookie 驗證中介軟體會有效。

ASP.NET 4.x 應用程式必須為目標.NET Framework 4.5.1 或更新版本。 否則，無法安裝必要的 NuGet 套件。

若要共用 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，設定 ASP.NET Core 應用程式中所述[共用在 ASP.NET Core 應用程式之間的驗證 cookie](#share-authentication-cookies-among-aspnet-core-apps)區段，然後設定 ASP.NET 4.x 應用程式如下所示。

確認應用程式的套件會更新為最新版本。 安裝[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)封裝到每個 ASP.NET 4.x 應用程式。

找出並修改呼叫`UseCookieAuthentication`:

* 變更要比對 ASP.NET Core Cookie 驗證中介軟體所使用的名稱的 cookie 名稱 (`.AspNet.SharedCookie`在範例中)。
* 在下列範例中，驗證類型設為`Identity.Application`。
* 提供的執行個體`DataProtectionProvider`初始化為一般資料保護金鑰的儲存體的位置。
* 確認應用程式名稱已設定共用的驗證 cookie 的所有應用程式所使用的通用應用程式名稱 (`SharedCookieApp`在範例中)。

如果未設定`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`並`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`，將<xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier>來區別的唯一使用者的宣告。

*App_Start/Startup.Auth.cs*:

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
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
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

產生的 「 使用者 」 身分識別的驗證類型時 (`Identity.Application`) 必須符合所定義的型別`AuthenticationType`設有`UseCookieAuthentication`中*App_Start/Startup.Auth.cs*。

*Models/IdentityModels.cs*:

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

## <a name="use-a-common-user-database"></a>使用一般的使用者資料庫

當應用程式使用相同的身分識別結構描述 （身分識別的相同版本），可讓您確認每個應用程式的身分識別系統必須指向相同的使用者資料庫。 否則，身分識別系統不會在執行階段失敗時它會嘗試比對驗證 cookie，針對其資料庫中的資訊中的資訊。

在應用程式間不同的身分識別結構描述時，通常是因為應用程式正在使用身分識別的版本不同，共用通用資料庫識別的最新版本為基礎簡直是不可能重新對應和其他應用程式的身分識別結構描述中加入資料行。 它通常會較有效率的方式是升級的其他應用程式使用最新版的身分識別，以便可由應用程式共用一個通用資料庫。

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/web-farm>
