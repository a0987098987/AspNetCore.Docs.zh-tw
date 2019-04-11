---
title: 使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie
author: rick-anderson
description: 了解如何共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: security/cookie-sharing
ms.openlocfilehash: 7a049ed8787808e228859afc051b8697a6261c21
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068306"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

網站通常會組成個別的 web 應用程式一起使用。 若要提供單一登入 (SSO) 體驗，在站台內的 web 應用程式必須共用驗證 cookie。 若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

此範例會說明在使用 cookie 驗證的三個應用程式之間共用的 cookie:

* ASP.NET Core 2.0 Razor 頁面應用程式，而不使用[ASP.NET Core 身分識別](xref:security/authentication/identity)
* 使用 ASP.NET Core 身分識別的 ASP.NET Core 2.0 MVC 應用程式
* 使用 ASP.NET Identity 的 ASP.NET Framework 4.6.1 MVC 應用程式

在接下來的範例：

* 驗證 cookie 名稱設定為通用值的`.AspNet.SharedCookie`。
* `AuthenticationType`設為`Identity.Application`明確或預設。
* 常見的應用程式名稱用來啟用共用的資料保護金鑰的資料保護系統 (`SharedCookieApp`)。
* `Identity.Application` 做為驗證配置。 任何配置，則必須以一致的方式使用*內和跨*共用的 cookie 的應用程式做為預設配置，或藉由明確地設定它。 加密和解密 cookie，因此必須跨應用程式使用一致的結構描述時，會使用配置。
* 通用[的資料保護金鑰](xref:security/data-protection/implementation/key-management)會使用儲存體位置。 範例應用程式會使用名為的資料夾*KeyRing*根目錄中的方案，以保留資料保護金鑰。
* 在 ASP.NET Core 應用程式中， [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)用來設定金鑰的儲存位置。 [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)用來設定共用的應用程式的一般名稱。
* 在.NET Framework 應用程式時，cookie 驗證中介軟體會使用實作[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)。 `DataProtectionProvider` 提供的加密和解密驗證 cookie 的內容資料的資料保護服務。 `DataProtectionProvider`是分開的應用程式其他部分所使用的資料保護系統的執行個體。
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo、 動作\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__)接受[DirectoryInfo](/dotnet/api/system.io.directoryinfo)來指定資料保護的金鑰存放區的位置。 範例應用程式提供的路徑*KeyRing*資料夾，以`DirectoryInfo`。 [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)設定一般的應用程式名稱。
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)需要[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet 套件。 若要取得此套件的 ASP.NET Core 2.1 和更新版本的應用程式，請參考[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)。 當目標為.NET Framework，新增的套件參考`Microsoft.AspNetCore.DataProtection.Extensions`。

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>共用 ASP.NET Core 應用程式之間的驗證 cookie

當使用 ASP.NET Core 身分識別：

::: moniker range=">= aspnetcore-2.0"

在 `ConfigureServices`方法，請使用[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie)設定 cookie 的資料保護服務的擴充方法。

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

資料保護金鑰和應用程式名稱必須在應用程式間共用。 在範例應用程式中，`GetKeyRingDirInfo`會傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。 使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)若要設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。 如需詳細資訊，請參閱 <<c0> [ 設定資料保護](xref:security/data-protection/configuration/overview)。

當裝載子網域之間共用的 cookie 的應用程式中，指定一般的網域中[Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)屬性。 在 應用程式之間共用 cookie `contoso.com`，這類`first_subdomain.contoso.com`並`second_subdomain.contoso.com`，指定`Cookie.Domain`做為`.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

請參閱*CookieAuthWithIdentity.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:index#how-to-download-a-sample))。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

在 `Configure`方法，請使用[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)設定：

* Cookie 資料保護服務。
* `AuthenticationScheme`來比對 ASP.NET 4.x。

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

當直接使用 cookie:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

資料保護金鑰和應用程式名稱必須在應用程式間共用。 在範例應用程式中，`GetKeyRingDirInfo`會傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。 使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)若要設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。 如需詳細資訊，請參閱 <<c0> [ 設定資料保護](xref:security/data-protection/configuration/overview)。

當裝載子網域之間共用的 cookie 的應用程式中，指定一般的網域中[Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)屬性。 在 應用程式之間共用 cookie `contoso.com`，這類`first_subdomain.contoso.com`並`second_subdomain.contoso.com`，指定`Cookie.Domain`做為`.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

請參閱*CookieAuth.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:index#how-to-download-a-sample))。

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

## <a name="encrypting-data-protection-keys-at-rest"></a>加密待用資料保護金鑰

針對生產環境部署，設定`DataProtectionProvider`來加密 DPAPI 或 X509Certificate 待用的金鑰。 請參閱[加密的金鑰在 Rest](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細資訊。

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式

使用 Katana cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生相容於 ASP.NET Core cookie 驗證中介軟體的驗證 cookie。 這可讓分次升級的大型網站的個別的應用程式，同時跨站台提供順暢的 SSO 體驗。

當應用程式會使用 Katana cookie 驗證中介軟體時，它會呼叫`UseCookieAuthentication`中的專案*Startup.Auth.cs*檔案。 ASP.NET 4.x web 應用程式專案使用 Visual Studio 2013 建立和更新版本預設會使用 Katana cookie 驗證中介軟體。 雖然`UseCookieAuthentication`已過時，不支援 ASP.NET Core 應用程式，請呼叫`UseCookieAuthentication`中 ASP.NET 4.x 應用程式使用 Katana cookie 驗證中介軟體是否有效。

ASP.NET 4.x 應用程式必須為目標.NET Framework 4.5.1 或更新版本。 否則，無法安裝必要的 NuGet 套件。

若要共用的 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，如上所述，設定 ASP.NET Core 應用程式，然後執行下列步驟來設定 ASP.NET 4.x 應用程式：

1. 安裝套件[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)到每個 ASP.NET 4.x 應用程式。

2. 在  *Startup.Auth.cs*，找出呼叫`UseCookieAuthentication`並加以修改，如下所示。 變更要與 ASP.NET Core cookie 驗證中介軟體所使用的名稱相符的 cookie 名稱。 提供的執行個體`DataProtectionProvider`初始化為一般資料保護金鑰的儲存體的位置。 請確定應用程式名稱設為共用 cookie 的所有應用程式所使用的一般應用程式名稱`SharedCookieApp`範例應用程式。

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

請參閱*CookieAuthWithIdentity.NETFramework*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:index#how-to-download-a-sample))。

驗證類型時產生的使用者身分識別，必須符合中定義的類型`AuthenticationType`設有`UseCookieAuthentication`。

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>使用一般的使用者資料庫

請確認每個應用程式的身分識別系統必須指向相同的使用者資料庫。 否則，身分識別系統不會在執行階段失敗時它會嘗試比對驗證 cookie，針對其資料庫中的資訊中的資訊。

## <a name="additional-resources"></a>其他資源

<xref:host-and-deploy/web-farm>
