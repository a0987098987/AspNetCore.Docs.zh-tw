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
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="d07c5-103">在 ASP.NET 應用程式之間共用的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="d07c5-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="d07c5-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d07c5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d07c5-105">網站通常會組成個別的 web 應用程式一起使用。</span><span class="sxs-lookup"><span data-stu-id="d07c5-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="d07c5-106">若要提供單一登入 (SSO) 體驗，在站台內的 web 應用程式必須共用驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="d07c5-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="d07c5-107">若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。</span><span class="sxs-lookup"><span data-stu-id="d07c5-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="d07c5-108">在接下來的範例：</span><span class="sxs-lookup"><span data-stu-id="d07c5-108">In the examples that follow:</span></span>

* <span data-ttu-id="d07c5-109">驗證 cookie 名稱設定為通用值的`.AspNet.SharedCookie`。</span><span class="sxs-lookup"><span data-stu-id="d07c5-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="d07c5-110">`AuthenticationType`設為`Identity.Application`明確或預設。</span><span class="sxs-lookup"><span data-stu-id="d07c5-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="d07c5-111">常見的應用程式名稱用來啟用共用的資料保護金鑰的資料保護系統 (`SharedCookieApp`)。</span><span class="sxs-lookup"><span data-stu-id="d07c5-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="d07c5-112">`Identity.Application` 做為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="d07c5-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="d07c5-113">任何配置，則必須以一致的方式使用*內和跨*共用的 cookie 的應用程式做為預設配置，或藉由明確地設定它。</span><span class="sxs-lookup"><span data-stu-id="d07c5-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="d07c5-114">加密和解密 cookie，因此必須跨應用程式使用一致的結構描述時，會使用配置。</span><span class="sxs-lookup"><span data-stu-id="d07c5-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="d07c5-115">通用[的資料保護金鑰](xref:security/data-protection/implementation/key-management)會使用儲存體位置。</span><span class="sxs-lookup"><span data-stu-id="d07c5-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="d07c5-116">在 ASP.NET Core 應用程式，<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>用來設定金鑰儲存位置。</span><span class="sxs-lookup"><span data-stu-id="d07c5-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="d07c5-117">在.NET Framework 應用程式中，Cookie 驗證中介軟體會使用實作<xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>。</span><span class="sxs-lookup"><span data-stu-id="d07c5-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="d07c5-118">`DataProtectionProvider` 提供的加密和解密驗證 cookie 的內容資料的資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="d07c5-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="d07c5-119">`DataProtectionProvider`是分開的應用程式其他部分所使用的資料保護系統的執行個體。</span><span class="sxs-lookup"><span data-stu-id="d07c5-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="d07c5-120">[DataProtectionProvider.Create (System.IO.DirectoryInfo、 動作\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*)接受<xref:System.IO.DirectoryInfo>來指定資料保護的金鑰存放區的位置。</span><span class="sxs-lookup"><span data-stu-id="d07c5-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="d07c5-121">`DataProtectionProvider` 需要[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="d07c5-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="d07c5-122">在 ASP.NET Core 2.x 應用程式，可以參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="d07c5-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="d07c5-123">在.NET Framework 應用程式，將新增的套件參考[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)。</span><span class="sxs-lookup"><span data-stu-id="d07c5-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="d07c5-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> 設定一般的應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="d07c5-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="d07c5-125">共用 ASP.NET Core 應用程式之間的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="d07c5-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="d07c5-126">當使用 ASP.NET Core 身分識別：</span><span class="sxs-lookup"><span data-stu-id="d07c5-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="d07c5-127">資料保護金鑰和應用程式名稱必須在應用程式間共用。</span><span class="sxs-lookup"><span data-stu-id="d07c5-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="d07c5-128">常見的金鑰儲存位置提供給<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*>在下列範例中的方法。</span><span class="sxs-lookup"><span data-stu-id="d07c5-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="d07c5-129">使用<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>若要設定共用的應用程式的一般名稱 (`SharedCookieApp`在下列範例中)。</span><span class="sxs-lookup"><span data-stu-id="d07c5-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="d07c5-130">如需詳細資訊，請參閱 <xref:security/data-protection/configuration/overview>。</span><span class="sxs-lookup"><span data-stu-id="d07c5-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="d07c5-131">使用<xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*>設定 cookie 的資料保護服務的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d07c5-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="d07c5-132">在下列範例中，驗證類型設為`Identity.Application`預設。</span><span class="sxs-lookup"><span data-stu-id="d07c5-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="d07c5-133">在 `Startup.ConfigureServices`中：</span><span class="sxs-lookup"><span data-stu-id="d07c5-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="d07c5-134">當使用直接沒有 ASP.NET Core 身分識別的 cookie，設定資料保護和驗證`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="d07c5-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d07c5-135">在下列範例中，驗證類型設為`Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="d07c5-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="d07c5-136">當裝載子網域之間共用的 cookie 的應用程式中，指定一般的網域中[Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain)屬性。</span><span class="sxs-lookup"><span data-stu-id="d07c5-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="d07c5-137">在 應用程式之間共用 cookie `contoso.com`，這類`first_subdomain.contoso.com`並`second_subdomain.contoso.com`，指定`Cookie.Domain`做為`.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="d07c5-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="d07c5-138">加密待用資料保護金鑰</span><span class="sxs-lookup"><span data-stu-id="d07c5-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="d07c5-139">針對生產環境部署，設定`DataProtectionProvider`來加密 DPAPI 或 X509Certificate 待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="d07c5-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="d07c5-140">如需詳細資訊，請參閱 <xref:security/data-protection/implementation/key-encryption-at-rest>。</span><span class="sxs-lookup"><span data-stu-id="d07c5-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="d07c5-141">在下列範例中，憑證指紋提供給<xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="d07c5-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="d07c5-142">驗證間共用 cookie ASP.NET 4.x 和 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="d07c5-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="d07c5-143">使用 Katana Cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生與 ASP.NET Core Cookie 驗證中介軟體相容的驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="d07c5-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="d07c5-144">這可讓升級個別的應用程式的大型網站在幾個步驟中，同時跨站台提供順暢的 SSO 體驗。</span><span class="sxs-lookup"><span data-stu-id="d07c5-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="d07c5-145">當應用程式會使用 Katana Cookie 驗證中介軟體時，它會呼叫`UseCookieAuthentication`中的專案*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="d07c5-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="d07c5-146">ASP.NET 4.x web 應用程式專案使用 Visual Studio 2013 建立和更新版本預設會使用 Katana Cookie 驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d07c5-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="d07c5-147">雖然`UseCookieAuthentication`; 已過時，不支援 ASP.NET Core 應用程式，呼叫`UseCookieAuthentication`中 ASP.NET 4.x 應用程式使用 Katana Cookie 驗證中介軟體會有效。</span><span class="sxs-lookup"><span data-stu-id="d07c5-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="d07c5-148">ASP.NET 4.x 應用程式必須為目標.NET Framework 4.5.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="d07c5-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="d07c5-149">否則，無法安裝必要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="d07c5-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="d07c5-150">若要共用 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，設定 ASP.NET Core 應用程式中所述[共用在 ASP.NET Core 應用程式之間的驗證 cookie](#share-authentication-cookies-among-aspnet-core-apps)區段，然後設定 ASP.NET 4.x 應用程式如下所示。</span><span class="sxs-lookup"><span data-stu-id="d07c5-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="d07c5-151">確認應用程式的套件會更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="d07c5-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="d07c5-152">安裝[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)封裝到每個 ASP.NET 4.x 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d07c5-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="d07c5-153">找出並修改呼叫`UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="d07c5-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="d07c5-154">變更要比對 ASP.NET Core Cookie 驗證中介軟體所使用的名稱的 cookie 名稱 (`.AspNet.SharedCookie`在範例中)。</span><span class="sxs-lookup"><span data-stu-id="d07c5-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="d07c5-155">在下列範例中，驗證類型設為`Identity.Application`。</span><span class="sxs-lookup"><span data-stu-id="d07c5-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="d07c5-156">提供的執行個體`DataProtectionProvider`初始化為一般資料保護金鑰的儲存體的位置。</span><span class="sxs-lookup"><span data-stu-id="d07c5-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="d07c5-157">確認應用程式名稱已設定共用的驗證 cookie 的所有應用程式所使用的通用應用程式名稱 (`SharedCookieApp`在範例中)。</span><span class="sxs-lookup"><span data-stu-id="d07c5-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="d07c5-158">如果未設定`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`並`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`，將<xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier>來區別的唯一使用者的宣告。</span><span class="sxs-lookup"><span data-stu-id="d07c5-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="d07c5-159">*App_Start/Startup.Auth.cs*:</span><span class="sxs-lookup"><span data-stu-id="d07c5-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="d07c5-160">產生的 「 使用者 」 身分識別的驗證類型時 (`Identity.Application`) 必須符合所定義的型別`AuthenticationType`設有`UseCookieAuthentication`中*App_Start/Startup.Auth.cs*。</span><span class="sxs-lookup"><span data-stu-id="d07c5-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="d07c5-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="d07c5-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="d07c5-162">使用一般的使用者資料庫</span><span class="sxs-lookup"><span data-stu-id="d07c5-162">Use a common user database</span></span>

<span data-ttu-id="d07c5-163">當應用程式使用相同的身分識別結構描述 （身分識別的相同版本），可讓您確認每個應用程式的身分識別系統必須指向相同的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="d07c5-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="d07c5-164">否則，身分識別系統不會在執行階段失敗時它會嘗試比對驗證 cookie，針對其資料庫中的資訊中的資訊。</span><span class="sxs-lookup"><span data-stu-id="d07c5-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="d07c5-165">在應用程式間不同的身分識別結構描述時，通常是因為應用程式正在使用身分識別的版本不同，共用通用資料庫識別的最新版本為基礎簡直是不可能重新對應和其他應用程式的身分識別結構描述中加入資料行。</span><span class="sxs-lookup"><span data-stu-id="d07c5-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="d07c5-166">它通常會較有效率的方式是升級的其他應用程式使用最新版的身分識別，以便可由應用程式共用一個通用資料庫。</span><span class="sxs-lookup"><span data-stu-id="d07c5-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d07c5-167">其他資源</span><span class="sxs-lookup"><span data-stu-id="d07c5-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
