---
title: 透過 WS-同盟中 ASP.NET Core 驗證使用者
author: chlowell
description: 本教學課程會示範如何在 ASP.NET Core 應用程式中使用 WS-同盟。
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
uid: security/authentication/ws-federation
ms.openlocfilehash: 55504ed28cf8ef1095bf16c101c09a6f374f038c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277435"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="a2133-103">透過 WS-同盟中 ASP.NET Core 驗證使用者</span><span class="sxs-lookup"><span data-stu-id="a2133-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="a2133-104">本教學課程示範如何讓使用者使用 WS-同盟驗證提供者如 Active Directory Federation Services (ADFS) 登入或[Azure Active Directory](/azure/active-directory/) (AAD)。</span><span class="sxs-lookup"><span data-stu-id="a2133-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="a2133-105">它會使用 ASP.NET Core 2.0 範例應用程式中所述[Facebook、 Google、 和外部提供者驗證](xref:security/authentication/social/index)。</span><span class="sxs-lookup"><span data-stu-id="a2133-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a2133-106">針對 ASP.NET Core 2.0 應用程式，支援 WS-同盟由提供[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)。</span><span class="sxs-lookup"><span data-stu-id="a2133-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="a2133-107">此元件會從移植[Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)和有許多是該元件的機制。</span><span class="sxs-lookup"><span data-stu-id="a2133-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="a2133-108">不過，在幾個重要方面與不同元件。</span><span class="sxs-lookup"><span data-stu-id="a2133-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="a2133-109">根據預設，新的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="a2133-109">By default, the new middleware:</span></span>

* <span data-ttu-id="a2133-110">不允許未經要求的登入。</span><span class="sxs-lookup"><span data-stu-id="a2133-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="a2133-111">WS-同盟通訊協定的這項功能很容易 XSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="a2133-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="a2133-112">不過，在啟用與`AllowUnsolicitedLogins`選項。</span><span class="sxs-lookup"><span data-stu-id="a2133-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="a2133-113">並不會檢查登入訊息的每個表單 post。</span><span class="sxs-lookup"><span data-stu-id="a2133-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="a2133-114">只會要求以`CallbackPath`會檢查的符號集`CallbackPath`預設為`/signin-wsfed`但可以變更透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="a2133-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="a2133-115">這個路徑可以與其他驗證提供者藉由啟用共用[SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests)選項。</span><span class="sxs-lookup"><span data-stu-id="a2133-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="a2133-116">使用 Active Directory 註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="a2133-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="a2133-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="a2133-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="a2133-118">開啟伺服器**新增信賴憑證者信任精靈**從 ADFS 管理主控台：</span><span class="sxs-lookup"><span data-stu-id="a2133-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![新增信賴憑證者信任精靈： 歡迎](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="a2133-120">選擇以手動方式輸入資料：</span><span class="sxs-lookup"><span data-stu-id="a2133-120">Choose to enter data manually:</span></span>

![新增信賴憑證者信任精靈： 選取資料來源](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="a2133-122">輸入信賴憑證者的合作對象的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="a2133-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="a2133-123">不需要 ASP.NET Core 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="a2133-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="a2133-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)不支援權杖加密，因此未設定權杖加密憑證：</span><span class="sxs-lookup"><span data-stu-id="a2133-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![新增信賴憑證者信任精靈： 設定憑證](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="a2133-126">啟用支援 WS-同盟被動式通訊協定，使用應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="a2133-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="a2133-127">請確認連接埠是正確的應用程式：</span><span class="sxs-lookup"><span data-stu-id="a2133-127">Verify the port is correct for the app:</span></span>

![新增信賴憑證者信任精靈： 設定 URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="a2133-129">這必須是 HTTPS URL。</span><span class="sxs-lookup"><span data-stu-id="a2133-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="a2133-130">在開發期間裝載應用程式時，IIS Express 可以提供自我簽署的憑證。</span><span class="sxs-lookup"><span data-stu-id="a2133-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="a2133-131">Kestrel 需要手動憑證設定。</span><span class="sxs-lookup"><span data-stu-id="a2133-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="a2133-132">請參閱[Kestrel 文件](xref:fundamentals/servers/kestrel)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a2133-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="a2133-133">按一下**下一步**精靈的其餘和**關閉**結尾。</span><span class="sxs-lookup"><span data-stu-id="a2133-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="a2133-134">ASP.NET Core 識別需要**名稱識別碼**宣告。</span><span class="sxs-lookup"><span data-stu-id="a2133-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="a2133-135">新增一項來自**編輯宣告規則**對話方塊：</span><span class="sxs-lookup"><span data-stu-id="a2133-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![編輯宣告規則](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="a2133-137">在**新增轉換宣告規則精靈**，保留預設值**以宣告方式傳送 LDAP 屬性**範本選取，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="a2133-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="a2133-138">新增規則對應**SAM 帳戶名稱**LDAP 屬性**名稱識別碼**連出宣告：</span><span class="sxs-lookup"><span data-stu-id="a2133-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![新增轉換宣告規則精靈： 設定宣告規則](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="a2133-140">按一下**完成** > **確定**中**編輯宣告規則**視窗。</span><span class="sxs-lookup"><span data-stu-id="a2133-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="a2133-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a2133-141">Azure Active Directory</span></span>

* <span data-ttu-id="a2133-142">瀏覽至 AAD 租用戶的應用程式註冊刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="a2133-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="a2133-143">按一下**新應用程式註冊**:</span><span class="sxs-lookup"><span data-stu-id="a2133-143">Click **New application registration**:</span></span>

![Azure Active Directory： 應用程式註冊](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="a2133-145">輸入應用程式註冊的名稱。</span><span class="sxs-lookup"><span data-stu-id="a2133-145">Enter a name for the app registration.</span></span> <span data-ttu-id="a2133-146">這並不重要 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a2133-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="a2133-147">輸入應用程式會接聽與 URL**登入 URL**:</span><span class="sxs-lookup"><span data-stu-id="a2133-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory： 建立應用程式註冊](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="a2133-149">按一下**端點**並記下**同盟中繼資料文件**URL。</span><span class="sxs-lookup"><span data-stu-id="a2133-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="a2133-150">這是 WS-同盟中介軟體的`MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="a2133-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory： 端點](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="a2133-152">瀏覽至新的應用程式註冊。</span><span class="sxs-lookup"><span data-stu-id="a2133-152">Navigate to the new app registration.</span></span> <span data-ttu-id="a2133-153">按一下**設定** > **屬性**然後記**應用程式識別碼 URI**。</span><span class="sxs-lookup"><span data-stu-id="a2133-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="a2133-154">這是 WS-同盟中介軟體的`Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="a2133-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory： 應用程式註冊屬性](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="a2133-156">加入為 ASP.NET Core 身分識別的外部登入提供者的 WS-同盟</span><span class="sxs-lookup"><span data-stu-id="a2133-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="a2133-157">加入的相依性[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)至專案。</span><span class="sxs-lookup"><span data-stu-id="a2133-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="a2133-158">加入至 WS-同盟`Configure`方法中的*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2133-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="a2133-159">登入 WS-同盟</span><span class="sxs-lookup"><span data-stu-id="a2133-159">Log in with WS-Federation</span></span>

<span data-ttu-id="a2133-160">瀏覽至應用程式，然後按一下**登入**nav 標頭中的連結。</span><span class="sxs-lookup"><span data-stu-id="a2133-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="a2133-161">還有一個選項以登入 WsFederation:![登入頁面](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="a2133-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="a2133-162">使用 ADFS 為提供者，按鈕會重新導向至 ADFS 登入頁面： ![ADFS 登入頁面](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="a2133-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="a2133-163">與 Azure Active Directory，為提供者，按鈕會重新導向至 AAD 登入頁面： ![AAD 登入頁面](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="a2133-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="a2133-164">成功登入新的使用者重新導向至應用程式的使用者註冊 頁面上：![註冊頁面](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="a2133-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="a2133-165">使用沒有 ASP.NET Core 識別 WS-同盟。</span><span class="sxs-lookup"><span data-stu-id="a2133-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="a2133-166">識別沒有可用的 WS-同盟中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a2133-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="a2133-167">例如: </span><span class="sxs-lookup"><span data-stu-id="a2133-167">For example:</span></span>

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
