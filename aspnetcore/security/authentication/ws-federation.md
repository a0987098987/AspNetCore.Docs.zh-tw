---
title: 在 ASP.NET Core 中使用 WS-同盟來驗證使用者
author: chlowell
description: 本教學課程示範如何在 ASP.NET Core 應用程式中使用 WS-同盟。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/ws-federation
ms.openlocfilehash: fede3887ad7dacd40cf3bb5d1b785392a9bc1480
ms.sourcegitcommit: 4a9321db7ca4e69074fa08a678dcc91e16215b1e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850457"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="a98e5-103">在 ASP.NET Core 中使用 WS-同盟來驗證使用者</span><span class="sxs-lookup"><span data-stu-id="a98e5-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="a98e5-104">本教學課程示範如何讓使用者使用 WS-同盟驗證提供者（例如 Active Directory 同盟服務（ADFS）或[Azure Active Directory](/azure/active-directory/) （AAD））進行登入。</span><span class="sxs-lookup"><span data-stu-id="a98e5-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="a98e5-105">它會使用[Facebook、Google 及外部提供者驗證](xref:security/authentication/social/index)中所述的 ASP.NET Core 範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="a98e5-105">It uses the ASP.NET Core sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a98e5-106">針對 ASP.NET Core 應用程式，WS-同盟支援是由[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)提供。 WsFederation。</span><span class="sxs-lookup"><span data-stu-id="a98e5-106">For ASP.NET Core apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="a98e5-107">此元件是從 Owin 進行移植。 [WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)並共用其中許多元件的機制。</span><span class="sxs-lookup"><span data-stu-id="a98e5-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="a98e5-108">不過，這些元件的差異在於幾個重要的方式。</span><span class="sxs-lookup"><span data-stu-id="a98e5-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="a98e5-109">根據預設，新的中介軟體：</span><span class="sxs-lookup"><span data-stu-id="a98e5-109">By default, the new middleware:</span></span>

* <span data-ttu-id="a98e5-110">不允許未經要求的登入。</span><span class="sxs-lookup"><span data-stu-id="a98e5-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="a98e5-111">WS-同盟通訊協定的這項功能很容易遭受 XSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="a98e5-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="a98e5-112">不過，您可以使用`AllowUnsolicitedLogins`選項來啟用它。</span><span class="sxs-lookup"><span data-stu-id="a98e5-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="a98e5-113">不會檢查每個表單張貼的登入訊息。</span><span class="sxs-lookup"><span data-stu-id="a98e5-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="a98e5-114">只會檢查對`CallbackPath`的要求是否有登入。 `CallbackPath`預設為`/signin-wsfed` ，但可透過[WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性加以變更。</span><span class="sxs-lookup"><span data-stu-id="a98e5-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="a98e5-115">藉由啟用[SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests)選項，可以與其他驗證提供者共用此路徑。</span><span class="sxs-lookup"><span data-stu-id="a98e5-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="a98e5-116">向 Active Directory 註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="a98e5-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="a98e5-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="a98e5-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="a98e5-118">從 ADFS 管理主控台開啟伺服器的 [**新增信賴憑證者信任]** 。</span><span class="sxs-lookup"><span data-stu-id="a98e5-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![新增信賴憑證者信任嚮導：歡迎使用](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="a98e5-120">選擇手動輸入資料：</span><span class="sxs-lookup"><span data-stu-id="a98e5-120">Choose to enter data manually:</span></span>

![新增信賴憑證者信任嚮導：選取資料來源](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="a98e5-122">輸入信賴憑證者的 [顯示名稱]。</span><span class="sxs-lookup"><span data-stu-id="a98e5-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="a98e5-123">此名稱對 ASP.NET Core 應用程式而言並不重要。</span><span class="sxs-lookup"><span data-stu-id="a98e5-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="a98e5-124">[AspNetCore，WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)缺少權杖加密的支援，因此請勿設定權杖加密憑證：</span><span class="sxs-lookup"><span data-stu-id="a98e5-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![新增信賴憑證者信任嚮導：設定憑證](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="a98e5-126">使用應用程式的 URL，啟用 WS-同盟被動式通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="a98e5-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="a98e5-127">確認此應用程式的埠是否正確：</span><span class="sxs-lookup"><span data-stu-id="a98e5-127">Verify the port is correct for the app:</span></span>

![新增信賴憑證者信任嚮導：設定 URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="a98e5-129">這必須是 HTTPS URL。</span><span class="sxs-lookup"><span data-stu-id="a98e5-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="a98e5-130">IIS Express 可以在開發期間裝載應用程式時提供自我簽署憑證。</span><span class="sxs-lookup"><span data-stu-id="a98e5-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="a98e5-131">Kestrel 需要手動設定憑證。</span><span class="sxs-lookup"><span data-stu-id="a98e5-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="a98e5-132">如需詳細資訊，請參閱[Kestrel 檔](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="a98e5-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="a98e5-133">在嚮導的其餘部分按 [**下一步**]，然後在結尾處**關閉**。</span><span class="sxs-lookup"><span data-stu-id="a98e5-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="a98e5-134">ASP.NET Core Identity需要**名稱識別碼**宣告。</span><span class="sxs-lookup"><span data-stu-id="a98e5-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="a98e5-135">從 [編輯宣告**規則**] 對話方塊新增一個：</span><span class="sxs-lookup"><span data-stu-id="a98e5-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![編輯宣告規則](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="a98e5-137">在 [**新增轉換宣告規則] 嚮導**中，將預設的 [**傳送 LDAP 屬性**] 保留為已選取，然後按 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="a98e5-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="a98e5-138">新增規則，將**SAM-Account-Name** LDAP 屬性對應至**名稱識別碼**傳出宣告：</span><span class="sxs-lookup"><span data-stu-id="a98e5-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![新增轉換宣告規則嚮導：設定宣告規則](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="a98e5-140">在 [**編輯宣告規則**] 視窗中，按一下 [**完成** > **確定**]。</span><span class="sxs-lookup"><span data-stu-id="a98e5-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="a98e5-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a98e5-141">Azure Active Directory</span></span>

* <span data-ttu-id="a98e5-142">流覽至 AAD 租使用者的 [應用程式註冊] 分頁。</span><span class="sxs-lookup"><span data-stu-id="a98e5-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="a98e5-143">按一下 [**新增應用程式註冊**]：</span><span class="sxs-lookup"><span data-stu-id="a98e5-143">Click **New application registration**:</span></span>

![Azure Active Directory：應用程式註冊](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="a98e5-145">輸入應用程式註冊的名稱。</span><span class="sxs-lookup"><span data-stu-id="a98e5-145">Enter a name for the app registration.</span></span> <span data-ttu-id="a98e5-146">這對 ASP.NET Core 應用程式來說並不重要。</span><span class="sxs-lookup"><span data-stu-id="a98e5-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="a98e5-147">在 [登**入 url**] 中輸入應用程式接聽的 url：</span><span class="sxs-lookup"><span data-stu-id="a98e5-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory：建立應用程式註冊](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="a98e5-149">按一下 [**端點**]，並記下 [**同盟元資料檔案**URL]。</span><span class="sxs-lookup"><span data-stu-id="a98e5-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="a98e5-150">這是 WS-同盟中介軟體的`MetadataAddress`：</span><span class="sxs-lookup"><span data-stu-id="a98e5-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory：端點](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="a98e5-152">流覽至新的應用程式註冊。</span><span class="sxs-lookup"><span data-stu-id="a98e5-152">Navigate to the new app registration.</span></span> <span data-ttu-id="a98e5-153">按一下 [**設定** > ] [**屬性**]，並記下 [**應用程式識別碼 URI**]。</span><span class="sxs-lookup"><span data-stu-id="a98e5-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="a98e5-154">這是 WS-同盟中介軟體的`Wtrealm`：</span><span class="sxs-lookup"><span data-stu-id="a98e5-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory：應用程式註冊屬性](ws-federation/_static/AadAppIdUri.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="a98e5-156">使用不含 ASP.NET Core 的 WS-同盟Identity</span><span class="sxs-lookup"><span data-stu-id="a98e5-156">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="a98e5-157">您可以使用 WS-同盟中介軟體而Identity不需要。</span><span class="sxs-lookup"><span data-stu-id="a98e5-157">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="a98e5-158">例如：</span><span class="sxs-lookup"><span data-stu-id="a98e5-158">For example:</span></span>
::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon21.cs?name=snippet)]
::: moniker-end

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="a98e5-159">新增 WS-同盟做為 ASP.NET Core 的外部登入提供者Identity</span><span class="sxs-lookup"><span data-stu-id="a98e5-159">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="a98e5-160">新增對 AspNetCore 的相依性。 [WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation)至專案。</span><span class="sxs-lookup"><span data-stu-id="a98e5-160">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="a98e5-161">將 WS-同盟新增`Startup.ConfigureServices`至：</span><span class="sxs-lookup"><span data-stu-id="a98e5-161">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup21.cs?name=snippet)]
::: moniker-end

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="a98e5-162">使用 WS-同盟進行登入</span><span class="sxs-lookup"><span data-stu-id="a98e5-162">Log in with WS-Federation</span></span>

<span data-ttu-id="a98e5-163">流覽至應用程式，然後按一下導覽標頭中的 [**登入**] 連結。</span><span class="sxs-lookup"><span data-stu-id="a98e5-163">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="a98e5-164">有一個選項可使用 WsFederation： ![登入頁面登入](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="a98e5-164">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="a98e5-165">使用 ADFS 做為提供者時，按鈕會重新導向至 ADFS 登入頁面![： adfs 登入頁面](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="a98e5-165">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="a98e5-166">使用 Azure Active Directory 做為提供者時，按鈕會重新導向至 AAD 登入頁面![： aad 登入頁面](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="a98e5-166">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="a98e5-167">成功登入新的使用者會重新導向至應用程式的使用者註冊頁面： ![[註冊] 頁面](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="a98e5-167">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>
