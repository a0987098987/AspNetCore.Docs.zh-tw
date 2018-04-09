---
title: 使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie
author: rick-anderson
description: 了解如何共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 2c0f5de4ecedb796e85c08fc50d9697947a75a3f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

網站通常包含個別 web 應用程式一起使用。 為提供單一登入 (SSO) 體驗，在站台內的 web 應用程式必須共用的驗證 cookie。 若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

範例將說明在使用 cookie 驗證的三個應用程式之間共用的 cookie:

* ASP.NET Core 2.0 Razor 頁面應用程式，而不使用[ASP.NET Core 身分識別](xref:security/authentication/identity)
* 使用 ASP.NET Core 身分識別的 ASP.NET Core 2.0 MVC 應用程式
* 使用 ASP.NET Identity 的 ASP.NET Framework 4.6.1 MVC 應用程式

在接下來的範例：

* 驗證 cookie 名稱設定的共通的值為`.AspNet.SharedCookie`。
* `AuthenticationType`設`Identity.Application`明確或預設。
* 常見的應用程式名稱用來啟用共用的資料保護金鑰的資料保護系統 (`SharedCookieApp`)。
* `Identity.Application` 做為驗證配置。 會使用任何結構描述，則必須以一致的方式使用*內部與跨*共用的 cookie 應用程式的預設配置，或是藉由明確地設定它。 加密和解密 cookie，因此必須使用一致的配置，跨應用程式時，會使用配置。
* 一般[資料保護金鑰](xref:security/data-protection/implementation/key-management)會使用儲存體位置。 範例應用程式會使用名為資料夾*KeyRing*根目錄中的方案來保存資料保護金鑰。
* 在 ASP.NET Core 應用程式中， [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)用來設定金鑰的儲存位置。 [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)用來設定一般的共用應用程式名稱。
* 在.NET Framework 應用程式的 cookie 驗證中介軟體會使用的實作[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)。 `DataProtectionProvider` 提供的加密和解密的驗證 cookie 裝載資料的資料保護服務。 `DataProtectionProvider`是分開的應用程式其他部分所使用的資料保護系統的執行個體。
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo、 動作\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__)接受[DirectoryInfo](/dotnet/api/system.io.directoryinfo)來指定資料保護的金鑰存放區的位置。 範例應用程式提供的路徑*KeyRing*資料夾`DirectoryInfo`。 [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)設定一般應用程式名稱。
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)需要[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet 封裝。 若要取得此套件的 ASP.NET Core 2.0 和更新版本的應用程式，請參考[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage。 當目標為.NET Framework，加入封裝的參考`Microsoft.AspNetCore.DataProtection.Extensions`。

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>共用 ASP.NET Core 應用程式之間的驗證 cookie

當使用 ASP.NET Core 身分識別：

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
在`ConfigureServices`方法，請使用[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie)擴充方法來設定資料保護服務的 cookie。

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

資料保護金鑰，應用程式名稱必須在應用程式之間共用。 在範例應用程式中，`GetKeyRingDirInfo`傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。 使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。 如需詳細資訊，請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。

請參閱*CookieAuthWithIdentity.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample))。

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
在`Configure`方法，請使用[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)設定：

* Cookie 資料保護服務。
* `AuthenticationScheme`要比對 ASP.NET 4.x。

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

* * *
當直接使用 cookie:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

資料保護金鑰，應用程式名稱必須在應用程式之間共用。 在範例應用程式中，`GetKeyRingDirInfo`傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。 使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。 如需詳細資訊，請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。 

請參閱*CookieAuth.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample))。

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

* * *
## <a name="encrypting-data-protection-keys-at-rest"></a>加密在靜止的資料保護金鑰

針對實際執行部署，設定`DataProtectionProvider`來加密金鑰使用 DPAPI 或 X509Certificate 靜止。 請參閱[金鑰靜態加密](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細資訊。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式

使用 Katana cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生相容於 ASP.NET Core cookie 驗證中介軟體的驗證 cookie。 這可讓分次升級大站台的個別的應用程式，同時提供跨網站 smooth SSO 的體驗。

> [!TIP]
> 當應用程式使用 Katana cookie 驗證中介軟體時，它會呼叫`UseCookieAuthentication`在專案的*Startup.Auth.cs*檔案。 ASP.NET 4.x web 應用程式專案與 Visual Studio 2013 所建立，然後依預設使用 Katana cookie 驗證中介軟體。

> [!NOTE]
> ASP.NET 4.x 應用程式必須以.NET Framework 4.5.1 為目標或更高版本。 否則，無法安裝必要的 NuGet 套件。

若要共用的 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，如上所述，設定 ASP.NET Core 應用程式，然後遵循下列步驟來設定 ASP.NET 4.x 應用程式。

1. 安裝套件[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)到每個 ASP.NET 4.x 應用程式。

2. 在*Startup.Auth.cs*，找出呼叫`UseCookieAuthentication`並加以修改，如下所示。 變更要與 ASP.NET Core cookie 驗證中介軟體所使用的名稱相符的 cookie 名稱。 提供的執行個體`DataProtectionProvider`初始化為一般的資料保護金鑰的儲存位置。 請確定應用程式名稱已設定共用 cookie 的所有應用程式所使用的一般應用程式名稱`SharedCookieApp`範例應用程式。

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

請參閱*CookieAuthWithIdentity.NETFramework*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample))。

驗證類型時產生使用者識別時，必須符合中定義的類型`AuthenticationType`設定`UseCookieAuthentication`。

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
設定`CookieManager`以 interop`ChunkingCookieManager`使區塊格式相容。

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

* * *
## <a name="use-a-common-user-database"></a>使用一般的使用者資料庫

請確認每個應用程式的身分識別系統指向相同的使用者資料庫。 否則，識別系統會產生在執行階段失敗時，它會嘗試比對中針對其資料庫中資訊的驗證 cookie 的資訊。
