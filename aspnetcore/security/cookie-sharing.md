---
title: 使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie
author: rick-anderson
description: 了解如何共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: 504d3369c908aabddeaa2fc3d1ef16a42b5cb771
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208511"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="96fbd-103">使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie</span><span class="sxs-lookup"><span data-stu-id="96fbd-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="96fbd-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="96fbd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="96fbd-105">網站通常會組成個別的 web 應用程式一起使用。</span><span class="sxs-lookup"><span data-stu-id="96fbd-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="96fbd-106">若要提供單一登入 (SSO) 體驗，在站台內的 web 應用程式必須共用驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="96fbd-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="96fbd-107">若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。</span><span class="sxs-lookup"><span data-stu-id="96fbd-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="96fbd-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96fbd-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="96fbd-109">此範例會說明在使用 cookie 驗證的三個應用程式之間共用的 cookie:</span><span class="sxs-lookup"><span data-stu-id="96fbd-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="96fbd-110">ASP.NET Core 2.0 Razor 頁面應用程式，而不使用[ASP.NET Core 身分識別](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="96fbd-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="96fbd-111">使用 ASP.NET Core 身分識別的 ASP.NET Core 2.0 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="96fbd-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="96fbd-112">使用 ASP.NET Identity 的 ASP.NET Framework 4.6.1 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="96fbd-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="96fbd-113">在接下來的範例：</span><span class="sxs-lookup"><span data-stu-id="96fbd-113">In the examples that follow:</span></span>

* <span data-ttu-id="96fbd-114">驗證 cookie 名稱設定為通用值的`.AspNet.SharedCookie`。</span><span class="sxs-lookup"><span data-stu-id="96fbd-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="96fbd-115">`AuthenticationType`設為`Identity.Application`明確或預設。</span><span class="sxs-lookup"><span data-stu-id="96fbd-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="96fbd-116">常見的應用程式名稱用來啟用共用的資料保護金鑰的資料保護系統 (`SharedCookieApp`)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="96fbd-117">`Identity.Application` 做為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="96fbd-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="96fbd-118">任何配置，則必須以一致的方式使用*內和跨*共用的 cookie 的應用程式做為預設配置，或藉由明確地設定它。</span><span class="sxs-lookup"><span data-stu-id="96fbd-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="96fbd-119">加密和解密 cookie，因此必須跨應用程式使用一致的結構描述時，會使用配置。</span><span class="sxs-lookup"><span data-stu-id="96fbd-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="96fbd-120">通用[的資料保護金鑰](xref:security/data-protection/implementation/key-management)會使用儲存體位置。</span><span class="sxs-lookup"><span data-stu-id="96fbd-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="96fbd-121">範例應用程式會使用名為的資料夾*KeyRing*根目錄中的方案，以保留資料保護金鑰。</span><span class="sxs-lookup"><span data-stu-id="96fbd-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="96fbd-122">在 ASP.NET Core 應用程式中， [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)用來設定金鑰的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="96fbd-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="96fbd-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)用來設定共用的應用程式的一般名稱。</span><span class="sxs-lookup"><span data-stu-id="96fbd-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="96fbd-124">在.NET Framework 應用程式時，cookie 驗證中介軟體會使用實作[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="96fbd-125">`DataProtectionProvider` 提供的加密和解密驗證 cookie 的內容資料的資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="96fbd-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="96fbd-126">`DataProtectionProvider`是分開的應用程式其他部分所使用的資料保護系統的執行個體。</span><span class="sxs-lookup"><span data-stu-id="96fbd-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="96fbd-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo、 動作\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__)接受[DirectoryInfo](/dotnet/api/system.io.directoryinfo)來指定資料保護的金鑰存放區的位置。</span><span class="sxs-lookup"><span data-stu-id="96fbd-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="96fbd-128">範例應用程式提供的路徑*KeyRing*資料夾，以`DirectoryInfo`。</span><span class="sxs-lookup"><span data-stu-id="96fbd-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="96fbd-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)設定一般的應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="96fbd-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="96fbd-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)需要[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="96fbd-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="96fbd-131">若要取得此套件的 ASP.NET Core 2.1 和更新版本的應用程式，請參考[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="96fbd-132">當目標為.NET Framework，新增的套件參考`Microsoft.AspNetCore.DataProtection.Extensions`。</span><span class="sxs-lookup"><span data-stu-id="96fbd-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="96fbd-133">共用 ASP.NET Core 應用程式之間的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="96fbd-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="96fbd-134">當使用 ASP.NET Core 身分識別：</span><span class="sxs-lookup"><span data-stu-id="96fbd-134">When using ASP.NET Core Identity:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96fbd-135">在 `ConfigureServices`方法，請使用[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie)設定 cookie 的資料保護服務的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="96fbd-135">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="96fbd-136">資料保護金鑰和應用程式名稱必須在應用程式間共用。</span><span class="sxs-lookup"><span data-stu-id="96fbd-136">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="96fbd-137">在範例應用程式中，`GetKeyRingDirInfo`會傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。</span><span class="sxs-lookup"><span data-stu-id="96fbd-137">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="96fbd-138">使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)若要設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-138">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="96fbd-139">如需詳細資訊，請參閱 <<c0> [ 設定資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-139">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="96fbd-140">當裝載子網域之間共用的 cookie 的應用程式中，指定一般的網域中[Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)屬性。</span><span class="sxs-lookup"><span data-stu-id="96fbd-140">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="96fbd-141">在 應用程式之間共用 cookie `contoso.com`，這類`first_subdomain.contoso.com`並`second_subdomain.contoso.com`，指定`Cookie.Domain`做為`.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="96fbd-141">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="96fbd-142">請參閱*CookieAuthWithIdentity.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="96fbd-142">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96fbd-143">在 `Configure`方法，請使用[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)設定：</span><span class="sxs-lookup"><span data-stu-id="96fbd-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="96fbd-144">Cookie 資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="96fbd-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="96fbd-145">`AuthenticationScheme`來比對 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="96fbd-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

::: moniker-end

<span data-ttu-id="96fbd-146">當直接使用 cookie:</span><span class="sxs-lookup"><span data-stu-id="96fbd-146">When using cookies directly:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="96fbd-147">資料保護金鑰和應用程式名稱必須在應用程式間共用。</span><span class="sxs-lookup"><span data-stu-id="96fbd-147">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="96fbd-148">在範例應用程式中，`GetKeyRingDirInfo`會傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。</span><span class="sxs-lookup"><span data-stu-id="96fbd-148">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="96fbd-149">使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)若要設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-149">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="96fbd-150">如需詳細資訊，請參閱 <<c0> [ 設定資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="96fbd-150">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="96fbd-151">當裝載子網域之間共用的 cookie 的應用程式中，指定一般的網域中[Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)屬性。</span><span class="sxs-lookup"><span data-stu-id="96fbd-151">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="96fbd-152">在 應用程式之間共用 cookie `contoso.com`，這類`first_subdomain.contoso.com`並`second_subdomain.contoso.com`，指定`Cookie.Domain`做為`.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="96fbd-152">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="96fbd-153">請參閱*CookieAuth.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="96fbd-153">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

::: moniker-end

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="96fbd-154">加密待用資料保護金鑰</span><span class="sxs-lookup"><span data-stu-id="96fbd-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="96fbd-155">針對生產環境部署，設定`DataProtectionProvider`來加密 DPAPI 或 X509Certificate 待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="96fbd-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="96fbd-156">請參閱[加密的金鑰在 Rest](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="96fbd-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

::: moniker-end

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="96fbd-157">共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="96fbd-157">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="96fbd-158">使用 Katana cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生相容於 ASP.NET Core cookie 驗證中介軟體的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="96fbd-158">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="96fbd-159">這可讓分次升級的大型網站的個別的應用程式，同時跨站台提供順暢的 SSO 體驗。</span><span class="sxs-lookup"><span data-stu-id="96fbd-159">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="96fbd-160">當應用程式會使用 Katana cookie 驗證中介軟體時，它會呼叫`UseCookieAuthentication`中的專案*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="96fbd-160">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="96fbd-161">ASP.NET 4.x web 應用程式專案使用 Visual Studio 2013 建立和更新版本預設會使用 Katana cookie 驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="96fbd-161">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="96fbd-162">雖然`UseCookieAuthentication`已過時，不支援 ASP.NET Core 應用程式，請呼叫`UseCookieAuthentication`中 ASP.NET 4.x 應用程式使用 Katana cookie 驗證中介軟體是否有效。</span><span class="sxs-lookup"><span data-stu-id="96fbd-162">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="96fbd-163">ASP.NET 4.x 應用程式必須以.NET Framework 4.5.1 為目標或更高版本。</span><span class="sxs-lookup"><span data-stu-id="96fbd-163">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="96fbd-164">否則，無法安裝必要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="96fbd-164">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="96fbd-165">若要共用的 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，如上所述，設定 ASP.NET Core 應用程式，然後執行下列步驟來設定 ASP.NET 4.x 應用程式：</span><span class="sxs-lookup"><span data-stu-id="96fbd-165">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="96fbd-166">安裝套件[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)到每個 ASP.NET 4.x 應用程式。</span><span class="sxs-lookup"><span data-stu-id="96fbd-166">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="96fbd-167">在  *Startup.Auth.cs*，找出呼叫`UseCookieAuthentication`並加以修改，如下所示。</span><span class="sxs-lookup"><span data-stu-id="96fbd-167">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="96fbd-168">變更要與 ASP.NET Core cookie 驗證中介軟體所使用的名稱相符的 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="96fbd-168">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="96fbd-169">提供的執行個體`DataProtectionProvider`初始化為一般資料保護金鑰的儲存體的位置。</span><span class="sxs-lookup"><span data-stu-id="96fbd-169">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="96fbd-170">請確定應用程式名稱設為共用 cookie 的所有應用程式所使用的一般應用程式名稱`SharedCookieApp`範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="96fbd-170">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="96fbd-171">請參閱*CookieAuthWithIdentity.NETFramework*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="96fbd-171">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="96fbd-172">驗證類型時產生的使用者身分識別，必須符合中定義的類型`AuthenticationType`設有`UseCookieAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="96fbd-172">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="96fbd-173">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="96fbd-173">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="96fbd-174">使用一般的使用者資料庫</span><span class="sxs-lookup"><span data-stu-id="96fbd-174">Use a common user database</span></span>

<span data-ttu-id="96fbd-175">請確認每個應用程式的身分識別系統必須指向相同的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="96fbd-175">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="96fbd-176">否則，身分識別系統不會在執行階段失敗時它會嘗試比對驗證 cookie，針對其資料庫中的資訊中的資訊。</span><span class="sxs-lookup"><span data-stu-id="96fbd-176">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96fbd-177">其他資源</span><span class="sxs-lookup"><span data-stu-id="96fbd-177">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
