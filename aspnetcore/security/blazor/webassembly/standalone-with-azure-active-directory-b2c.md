---
<span data-ttu-id="6b66d-101">標題：「 Blazor 使用 Azure Active Directory B2C 的作者來保護 ASP.NET Core WebAssembly 獨立應用程式：描述： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="6b66d-101">title: 'Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="6b66d-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="6b66d-102">'Blazor'</span></span>
- <span data-ttu-id="6b66d-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="6b66d-103">'Identity'</span></span>
- <span data-ttu-id="6b66d-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="6b66d-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="6b66d-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="6b66d-105">'Razor'</span></span>
- <span data-ttu-id="6b66d-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="6b66d-106">'SignalR' uid:</span></span> 

---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="6b66d-107">Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="6b66d-107">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="6b66d-108">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6b66d-108">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6b66d-109">若要建立 Blazor 使用[AZURE ACTIVE DIRECTORY （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 WebAssembly 獨立應用程式：</span><span class="sxs-lookup"><span data-stu-id="6b66d-109">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

<span data-ttu-id="6b66d-110">遵循下列主題中的指導方針，在 Azure 入口網站中建立租使用者並註冊 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="6b66d-110">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

[<span data-ttu-id="6b66d-111">建立 AAD B2C 租使用者</span><span class="sxs-lookup"><span data-stu-id="6b66d-111">Create an AAD B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)

<span data-ttu-id="6b66d-112">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="6b66d-112">Record the following information:</span></span>

* <span data-ttu-id="6b66d-113">AAD B2C 實例（例如， `https://contoso.b2clogin.com/` 包含尾端斜線的）。</span><span class="sxs-lookup"><span data-stu-id="6b66d-113">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash).</span></span>
* <span data-ttu-id="6b66d-114">AAD B2C 租使用者網域（例如 `contoso.onmicrosoft.com` ）。</span><span class="sxs-lookup"><span data-stu-id="6b66d-114">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`).</span></span>

<span data-ttu-id="6b66d-115">請依照教學課程[：在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*用戶端應用程式*註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="6b66d-115">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app*:</span></span>

1. <span data-ttu-id="6b66d-116">在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。</span><span class="sxs-lookup"><span data-stu-id="6b66d-116">In **Azure Active Directory** > **App registrations**, select **New registration**.</span></span>
1. <span data-ttu-id="6b66d-117">提供應用程式的**名稱**（例如， \*\* Blazor 獨立 AAD B2C\*\*）。</span><span class="sxs-lookup"><span data-stu-id="6b66d-117">Provide a **Name** for the app (for example, **Blazor Standalone AAD B2C**).</span></span>
1. <span data-ttu-id="6b66d-118">針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="6b66d-118">For **Supported account types**, select the multi-tenant option: **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span>
1. <span data-ttu-id="6b66d-119">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="6b66d-119">Leave the **Redirect URI** drop down set to **Web**, and provide the following redirect URI: `https://localhost:{PORT}/authentication/login-callback`.</span></span> <span data-ttu-id="6b66d-120">在 Kestrel 上執行之應用程式的預設埠是5001。</span><span class="sxs-lookup"><span data-stu-id="6b66d-120">The default port for an app running on Kestrel is 5001.</span></span> <span data-ttu-id="6b66d-121">針對 IIS Express，隨機產生的埠可以在 [**調試**程式] 面板的 [屬性] 中找到。</span><span class="sxs-lookup"><span data-stu-id="6b66d-121">For IIS Express, the randomly generated port can be found in the app's properties in the **Debug** panel.</span></span>
1. <span data-ttu-id="6b66d-122">確認**許可權**  >  **授與系統管理員收到給 openid，並已啟用 offline_access 許可權**。</span><span class="sxs-lookup"><span data-stu-id="6b66d-122">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="6b66d-123">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="6b66d-123">Select **Register**.</span></span>

<span data-ttu-id="6b66d-124">記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111` ）。</span><span class="sxs-lookup"><span data-stu-id="6b66d-124">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

<span data-ttu-id="6b66d-125">在 [**驗證**  >  **平臺**設定]  >  **Web**：</span><span class="sxs-lookup"><span data-stu-id="6b66d-125">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="6b66d-126">確認的重新**導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。</span><span class="sxs-lookup"><span data-stu-id="6b66d-126">Confirm the **Redirect URI** of `https://localhost:{PORT}/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="6b66d-127">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="6b66d-127">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="6b66d-128">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="6b66d-128">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="6b66d-129">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="6b66d-129">Select the **Save** button.</span></span>

<span data-ttu-id="6b66d-130">在**Home**  >  **Azure AD B2C**  >  **使用者流程**：</span><span class="sxs-lookup"><span data-stu-id="6b66d-130">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="6b66d-131">建立註冊和登入使用者流程</span><span class="sxs-lookup"><span data-stu-id="6b66d-131">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="6b66d-132">至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` `LoginDisplay` 元件（*Shared/LoginDisplay*）中的。</span><span class="sxs-lookup"><span data-stu-id="6b66d-132">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="6b66d-133">記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin` ）。</span><span class="sxs-lookup"><span data-stu-id="6b66d-133">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

<span data-ttu-id="6b66d-134">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="6b66d-134">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{TENANT DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
```

<span data-ttu-id="6b66d-135">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。</span><span class="sxs-lookup"><span data-stu-id="6b66d-135">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="6b66d-136">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="6b66d-136">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="6b66d-137">建立應用程式之後，您應該能夠：</span><span class="sxs-lookup"><span data-stu-id="6b66d-137">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="6b66d-138">使用 AAD 使用者帳戶登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="6b66d-138">Log into the app using an AAD user account.</span></span>
* <span data-ttu-id="6b66d-139">要求 Microsoft Api 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="6b66d-139">Request access tokens for Microsoft APIs.</span></span> <span data-ttu-id="6b66d-140">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="6b66d-140">For more information, see:</span></span>
  * [<span data-ttu-id="6b66d-141">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="6b66d-141">Access token scopes</span></span>](#access-token-scopes)
  * <span data-ttu-id="6b66d-142">[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。</span><span class="sxs-lookup"><span data-stu-id="6b66d-142">[Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="6b66d-143">驗證套件</span><span class="sxs-lookup"><span data-stu-id="6b66d-143">Authentication package</span></span>

<span data-ttu-id="6b66d-144">建立應用程式以使用個別 B2C 帳戶（ `IndividualB2C` ）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（）的套件參考 `Microsoft.Authentication.WebAssembly.Msal` 。</span><span class="sxs-lookup"><span data-stu-id="6b66d-144">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="6b66d-145">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="6b66d-145">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="6b66d-146">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="6b66d-146">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

<span data-ttu-id="6b66d-147">`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="6b66d-147">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="6b66d-148">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="6b66d-148">Authentication service support</span></span>

<span data-ttu-id="6b66d-149">使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 `AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal` 。</span><span class="sxs-lookup"><span data-stu-id="6b66d-149">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="6b66d-150">這個方法會設定應用程式與 Identity 提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="6b66d-150">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="6b66d-151">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="6b66d-151">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
});
```

<span data-ttu-id="6b66d-152">`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="6b66d-152">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="6b66d-153">當您註冊應用程式時，可以從 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="6b66d-153">The values required for configuring the app can be obtained from the AAD configuration when you register the app.</span></span>

<span data-ttu-id="6b66d-154">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="6b66d-154">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT ID}",
    "ValidateAuthority": false
  }
}
```

<span data-ttu-id="6b66d-155">範例：</span><span class="sxs-lookup"><span data-stu-id="6b66d-155">Example:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "ValidateAuthority": false
  }
}
```

## <a name="access-token-scopes"></a><span data-ttu-id="6b66d-156">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="6b66d-156">Access token scopes</span></span>

<span data-ttu-id="6b66d-157">BlazorWebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="6b66d-157">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="6b66d-158">若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍 `MsalProviderOptions` ：</span><span class="sxs-lookup"><span data-stu-id="6b66d-158">To provision an access token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

<span data-ttu-id="6b66d-159">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="6b66d-159">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="6b66d-160">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="6b66d-160">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="6b66d-161">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="6b66d-161">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a><span data-ttu-id="6b66d-162">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="6b66d-162">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="6b66d-163">索引頁面</span><span class="sxs-lookup"><span data-stu-id="6b66d-163">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="6b66d-164">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="6b66d-164">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="6b66d-165">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="6b66d-165">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="6b66d-166">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="6b66d-166">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="6b66d-167">驗證元件</span><span class="sxs-lookup"><span data-stu-id="6b66d-167">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="6b66d-168">其他資源</span><span class="sxs-lookup"><span data-stu-id="6b66d-168">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* [<span data-ttu-id="6b66d-169">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="6b66d-169">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="6b66d-170">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="6b66d-170">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="6b66d-171">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="6b66d-171">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
