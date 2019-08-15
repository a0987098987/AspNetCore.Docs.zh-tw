---
title: 在 ASP.NET apps 之間共用驗證 cookie
author: rick-anderson
description: 了解如何共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: security/cookie-sharing
ms.openlocfilehash: 1650afce5c371d0830bb207618b9c1495f0ce587
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022395"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="8dabe-103">在 ASP.NET apps 之間共用驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="8dabe-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="8dabe-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8dabe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8dabe-105">網站通常包含個別的 web 應用程式一起運作。</span><span class="sxs-lookup"><span data-stu-id="8dabe-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="8dabe-106">若要提供單一登入 (SSO) 體驗, 網站內的 web 應用程式必須共用驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="8dabe-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="8dabe-107">若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。</span><span class="sxs-lookup"><span data-stu-id="8dabe-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="8dabe-108">在下列範例中:</span><span class="sxs-lookup"><span data-stu-id="8dabe-108">In the examples that follow:</span></span>

* <span data-ttu-id="8dabe-109">驗證 cookie 名稱會設定為的通用值`.AspNet.SharedCookie`。</span><span class="sxs-lookup"><span data-stu-id="8dabe-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="8dabe-110">會明確或依預設設定為`Identity.Application`。 `AuthenticationType`</span><span class="sxs-lookup"><span data-stu-id="8dabe-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="8dabe-111">通用應用程式名稱是用來讓資料保護系統共用資料保護金鑰 (`SharedCookieApp`)。</span><span class="sxs-lookup"><span data-stu-id="8dabe-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="8dabe-112">`Identity.Application`是用來做為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="8dabe-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="8dabe-113">無論使用哪種配置, 都必須以一致的方式在共用的 cookie 應用程式*中*使用, 或是透過明確地設定它。</span><span class="sxs-lookup"><span data-stu-id="8dabe-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="8dabe-114">配置會用於加密和解密 cookie, 因此必須跨應用程式使用一致的配置。</span><span class="sxs-lookup"><span data-stu-id="8dabe-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="8dabe-115">使用的是通用[資料保護金鑰](xref:security/data-protection/implementation/key-management)儲存位置。</span><span class="sxs-lookup"><span data-stu-id="8dabe-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="8dabe-116">在 ASP.NET Core 應用程式<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>中, 是用來設定金鑰儲存位置。</span><span class="sxs-lookup"><span data-stu-id="8dabe-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="8dabe-117">在 .NET Framework 應用程式中, Cookie 驗證中介軟體會<xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>使用的執行。</span><span class="sxs-lookup"><span data-stu-id="8dabe-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="8dabe-118">`DataProtectionProvider`提供用於加密和解密驗證 cookie 承載資料的資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="8dabe-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="8dabe-119">`DataProtectionProvider`實例會與其他應用程式元件所使用的資料保護系統隔離。</span><span class="sxs-lookup"><span data-stu-id="8dabe-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="8dabe-120">[DataProtectionProvider (DirectoryInfo, Action\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*)會接受<xref:System.IO.DirectoryInfo>來指定資料保護金鑰儲存的位置。</span><span class="sxs-lookup"><span data-stu-id="8dabe-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="8dabe-121">`DataProtectionProvider`需要[AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)的 NuGet 套件:</span><span class="sxs-lookup"><span data-stu-id="8dabe-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="8dabe-122">在 ASP.NET Core 2.x 應用程式中, 參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="8dabe-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="8dabe-123">在 .NET Framework 應用程式中, 將套件參考新增至[AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)。</span><span class="sxs-lookup"><span data-stu-id="8dabe-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="8dabe-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>設定一般應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="8dabe-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="8dabe-125">共用 ASP.NET Core 應用程式之間的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="8dabe-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="8dabe-126">當使用 ASP.NET Core 身分識別：</span><span class="sxs-lookup"><span data-stu-id="8dabe-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="8dabe-127">資料保護金鑰和應用程式名稱必須在應用程式之間共用。</span><span class="sxs-lookup"><span data-stu-id="8dabe-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="8dabe-128">在下列範例中, 會將一般金鑰<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>儲存位置提供給方法。</span><span class="sxs-lookup"><span data-stu-id="8dabe-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="8dabe-129">使用<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>來設定通用共用應用程式名稱 (`SharedCookieApp`在下列範例中為)。</span><span class="sxs-lookup"><span data-stu-id="8dabe-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="8dabe-130">如需詳細資訊，請參閱 <xref:security/data-protection/configuration/overview>。</span><span class="sxs-lookup"><span data-stu-id="8dabe-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="8dabe-131"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*>使用擴充方法來設定 cookie 的資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="8dabe-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="8dabe-132">預設驗證類型為`Identity.Application`。</span><span class="sxs-lookup"><span data-stu-id="8dabe-132">The default authentication type is `Identity.Application`.</span></span>

<span data-ttu-id="8dabe-133">在 `Startup.ConfigureServices`中：</span><span class="sxs-lookup"><span data-stu-id="8dabe-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="8dabe-134">直接使用 cookie 而不 ASP.NET Core 身分識別時, 請在中`Startup.ConfigureServices`設定資料保護和驗證。</span><span class="sxs-lookup"><span data-stu-id="8dabe-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8dabe-135">在下列範例中, 驗證類型設定為`Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="8dabe-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="8dabe-136">在裝載跨子域共用 cookie 的應用程式時, 請在 [ [Cookie. domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) ] 屬性中指定一個通用網域。</span><span class="sxs-lookup"><span data-stu-id="8dabe-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="8dabe-137">若`contoso.com`要在上跨應用程式共用 cookie ( `second_subdomain.contoso.com` `first_subdomain.contoso.com`例如和), `.contoso.com`請將`Cookie.Domain`指定為:</span><span class="sxs-lookup"><span data-stu-id="8dabe-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="8dabe-138">加密待用資料保護金鑰</span><span class="sxs-lookup"><span data-stu-id="8dabe-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="8dabe-139">針對生產環境部署, 請`DataProtectionProvider`將設定為使用 DPAPI 或 X509Certificate 來加密靜止的金鑰。</span><span class="sxs-lookup"><span data-stu-id="8dabe-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="8dabe-140">如需詳細資訊，請參閱 <xref:security/data-protection/implementation/key-encryption-at-rest>。</span><span class="sxs-lookup"><span data-stu-id="8dabe-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="8dabe-141">在下列範例中, 會將憑證指紋提供給<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="8dabe-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="8dabe-142">共用 ASP.NET 4.x 和 ASP.NET Core 應用程式之間的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="8dabe-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="8dabe-143">您可以設定使用 Katana Cookie 驗證中介軟體的 ASP.NET 4.x 應用程式, 以產生與 ASP.NET Core Cookie 驗證中介軟體相容的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="8dabe-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="8dabe-144">這可讓您在多個步驟中升級大型網站的個別應用程式, 同時在網站上提供順暢的 SSO 體驗。</span><span class="sxs-lookup"><span data-stu-id="8dabe-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="8dabe-145">當應用程式使用 Katana Cookie 驗證中介軟體時, `UseCookieAuthentication`它會在專案的*Startup.Auth.cs*檔中呼叫。</span><span class="sxs-lookup"><span data-stu-id="8dabe-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="8dabe-146">使用 Visual Studio 2013 和更新版本建立的 ASP.NET 4.x web 應用程式專案預設會使用 Katana Cookie 驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8dabe-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="8dabe-147">雖然`UseCookieAuthentication`已過時且不支援 ASP.NET Core 應用程式, `UseCookieAuthentication`但在使用 Katana Cookie 驗證中介軟體的 ASP.NET 4.x 應用程式中呼叫是有效的。</span><span class="sxs-lookup"><span data-stu-id="8dabe-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="8dabe-148">ASP.NET 4.x 應用程式必須以 .NET Framework 4.5.1 或更新版本為目標。</span><span class="sxs-lookup"><span data-stu-id="8dabe-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="8dabe-149">否則, 就無法安裝必要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8dabe-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="8dabe-150">若要在 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間共用驗證 cookie, 請依照在[ASP.NET Core apps 之間共用驗證 cookie](#share-authentication-cookies-among-aspnet-core-apps)一節中所述的方式設定 ASP.NET Core 應用程式, 然後設定 ASP.NET 4.x 應用程式, 如下所示。</span><span class="sxs-lookup"><span data-stu-id="8dabe-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="8dabe-151">確認應用程式的套件已更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="8dabe-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="8dabe-152">將[Owin](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)套件安裝到每個 ASP.NET 4.x 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8dabe-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="8dabe-153">找出並修改對的`UseCookieAuthentication`呼叫:</span><span class="sxs-lookup"><span data-stu-id="8dabe-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="8dabe-154">變更 cookie 名稱, 使其符合 ASP.NET Core cookie 驗證中介軟體所使用的名稱`.AspNet.SharedCookie` (在此範例中為)。</span><span class="sxs-lookup"><span data-stu-id="8dabe-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="8dabe-155">在下列範例中, 驗證類型設定為`Identity.Application`。</span><span class="sxs-lookup"><span data-stu-id="8dabe-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="8dabe-156">提供已`DataProtectionProvider`初始化的實例給通用資料保護金鑰儲存位置。</span><span class="sxs-lookup"><span data-stu-id="8dabe-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="8dabe-157">確認應用程式名稱已設定為所有共用驗證 cookie 之應用程式所使用的通用應用程式名稱`SharedCookieApp` (在此範例中為)。</span><span class="sxs-lookup"><span data-stu-id="8dabe-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="8dabe-158">如果未設定`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`和`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, 請<xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier>將設定為可區別唯一使用者的宣告。</span><span class="sxs-lookup"><span data-stu-id="8dabe-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="8dabe-159">*App_Start/Startup. Auth .cs*:</span><span class="sxs-lookup"><span data-stu-id="8dabe-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="8dabe-160">產生使用者識別時`Identity.Application`, 驗證類型 () 必須符合在*App_Start/Startup*中`AuthenticationType` set with `UseCookieAuthentication`所定義的類型。</span><span class="sxs-lookup"><span data-stu-id="8dabe-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="8dabe-161">*模型/IdentityModels .cs*:</span><span class="sxs-lookup"><span data-stu-id="8dabe-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="8dabe-162">使用一般使用者資料庫</span><span class="sxs-lookup"><span data-stu-id="8dabe-162">Use a common user database</span></span>

<span data-ttu-id="8dabe-163">當應用程式使用相同的身分識別架構 (相同的身分識別版本) 時, 請確認每個應用程式的身分識別系統都指向相同的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="8dabe-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="8dabe-164">否則, 當身分識別系統嘗試比對驗證 cookie 中的資訊與其資料庫中的資訊時, 會在執行時間產生失敗。</span><span class="sxs-lookup"><span data-stu-id="8dabe-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="8dabe-165">當應用程式之間的身分識別架構不同時 (通常是因為應用程式使用不同的身分識別版本), 不需要重新對應並在其他應用程式的身分識別架構中新增資料行, 就能以最新版本的身分識別共用通用資料庫。</span><span class="sxs-lookup"><span data-stu-id="8dabe-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="8dabe-166">將其他應用程式升級為使用最新的身分識別版本, 讓應用程式可以共用通用資料庫, 通常會更有效率。</span><span class="sxs-lookup"><span data-stu-id="8dabe-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8dabe-167">其他資源</span><span class="sxs-lookup"><span data-stu-id="8dabe-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
