---
title: 透過 WS-同盟中 ASP.NET Core 驗證使用者
author: chlowell
description: 本教學課程會示範如何在 ASP.NET Core 應用程式中使用 WS-同盟。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>透過 WS-同盟中 ASP.NET Core 驗證使用者

本教學課程示範如何讓使用者使用 WS-同盟驗證提供者如 Active Directory Federation Services (ADFS) 登入或[Azure Active Directory](/azure/active-directory/) (AAD)。 它會使用 ASP.NET Core 2.0 範例應用程式中所述[Facebook、 Google、 和外部提供者驗證](xref:security/authentication/social/index)。

針對 ASP.NET Core 2.0 應用程式，支援 WS-同盟由提供[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)。 此元件會從移植[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)和有許多是該元件的機制。 不過，在幾個重要方面與不同元件。

根據預設，新的中介軟體：

* 不允許未經要求的登入。 WS-同盟通訊協定的這項功能很容易 XSRF 攻擊。 不過，在啟用與`AllowUnsolicitedLogins`選項。
* 並不會檢查登入訊息的每個表單 post。 只會要求以`CallbackPath`會檢查的符號集`CallbackPath`預設為`/signin-wsfed`但可以變更。 這個路徑可以與其他驗證提供者藉由啟用共用`SkipUnrecognizedRequests`選項。

## <a name="register-the-app-with-active-directory"></a>使用 Active Directory 註冊應用程式

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* 開啟伺服器**新增信賴憑證者信任精靈**從 ADFS 管理主控台：

![新增信賴憑證者信任精靈：  褖畫惎](ws-federation/_static/AdfsAddTrust.png)

* 選擇以手動方式輸入資料：

![新增信賴憑證者信任精靈： 選取資料來源](ws-federation/_static/AdfsSelectDataSource.png)

* 輸入信賴憑證者的合作對象的顯示名稱。 不需要 ASP.NET Core 應用程式的名稱。

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)不支援權杖加密，因此未設定權杖加密憑證：

![新增信賴憑證者信任精靈： 設定憑證](ws-federation/_static/AdfsConfigureCert.png)

* 啟用支援 WS-同盟被動式通訊協定，使用應用程式的 URL。 請確認連接埠是正確的應用程式：

![新增信賴憑證者信任精靈： 設定 URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> 這必須是 HTTPS URL。 在開發期間裝載應用程式時，IIS Express 可以提供自我簽署的憑證。 Kestrel 需要手動憑證設定。 請參閱[Kestrel 文件](xref:fundamentals/servers/kestrel)如需詳細資訊。

* 按一下**下一步**精靈的其餘和**關閉**結尾。

* ASP.NET Core 識別需要**名稱識別碼**宣告。 新增一項來自**編輯宣告規則**對話方塊：

![編輯宣告規則](ws-federation/_static/EditClaimRules.png)

* 在**新增轉換宣告規則精靈**，保留預設值**以宣告方式傳送 LDAP 屬性**範本選取，然後按一下**下一步**。 新增規則對應**SAM 帳戶名稱**LDAP 屬性**名稱識別碼**連出宣告：

![新增轉換宣告規則精靈： 設定宣告規則](ws-federation/_static/AddTransformClaimRule.png)

* 按一下**完成** > **確定**中**編輯宣告規則**視窗。

### <a name="azure-active-directory"></a>Azure Active Directory

* 瀏覽至 AAD 租用戶的應用程式註冊刀鋒視窗。 按一下**新應用程式註冊**:

![Azure Active Directory： 應用程式註冊](ws-federation/_static/AadNewAppRegistration.png)

* 輸入應用程式註冊的名稱。 這並不重要 ASP.NET Core 應用程式。
* 輸入應用程式會接聽與 URL**登入 URL**:

![Azure Active Directory： 建立應用程式註冊](ws-federation/_static/AadCreateAppRegistration.png)

* 按一下**端點**並記下**同盟中繼資料文件**URL。 這是 WS-同盟中介軟體的`MetadataAddress`:

![Azure Active Directory： 端點](ws-federation/_static/AadFederationMetadataDocument.png)

* 瀏覽至新的應用程式註冊。 按一下**設定** > **屬性**然後記**應用程式識別碼 URI**。 這是 WS-同盟中介軟體的`Wtrealm`:

![Azure Active Directory： 應用程式註冊屬性](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>加入為 ASP.NET Core 身分識別的外部登入提供者的 WS-同盟

* 加入的相依性[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)至專案。
* 加入至 WS-同盟`Configure`方法中的*Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>登入 WS-同盟

瀏覽至應用程式，然後按一下**登入**nav 標頭中的連結。 還有一個選項以登入 WsFederation:![登入頁面](ws-federation/_static/WsFederationButton.png)

使用 ADFS 為提供者，按鈕會重新導向至 ADFS 登入頁面： ![ADFS 登入頁面](ws-federation/_static/AdfsLoginPage.png)

與 Azure Active Directory，為提供者，按鈕會重新導向至 AAD 登入頁面： ![AAD 登入頁面](ws-federation/_static/AadSignIn.png)

成功登入新的使用者重新導向至應用程式的使用者註冊 頁面上：![註冊頁面](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>使用沒有 ASP.NET Core 識別 WS-同盟。

識別沒有可用的 WS-同盟中介軟體。 例如: 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
