---
title: 使用 Azure Active Directory B2C 保護 Blazor WebAssembly 託管應用程式的 ASP.NET Core
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 0083f179f85371d4751fb179194417681fc1a01d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219060"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="2d9bb-102">使用 Azure Active Directory B2C 保護 Blazor WebAssembly 託管應用程式的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2d9bb-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="2d9bb-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2d9bb-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="2d9bb-104">本文說明如何建立使用[Azure Active Directory （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 Blazor WebAssembly 獨立應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="2d9bb-105">在 AAD B2C 中註冊應用程式並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="2d9bb-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="2d9bb-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="2d9bb-106">Create a tenant</span></span>

<span data-ttu-id="2d9bb-107">請遵循教學課程[：建立 Azure Active Directory B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者中的指引來建立 AAD B2C 租使用者，並記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="2d9bb-108">AAD B2C 實例（例如，`https://contoso.b2clogin.com/`，其中包含尾端斜線）</span><span class="sxs-lookup"><span data-stu-id="2d9bb-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="2d9bb-109">AAD B2C 租使用者網域（例如，`contoso.onmicrosoft.com`）</span><span class="sxs-lookup"><span data-stu-id="2d9bb-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="2d9bb-110">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="2d9bb-110">Register a server API app</span></span>

<span data-ttu-id="2d9bb-111">依照[教學課程：在 Azure Active Directory B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications)中的指導方針，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中，為*伺服器 API 應用*程式註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2d9bb-112">選取 [新增註冊]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-112">Select **New registration**.</span></span>
1. <span data-ttu-id="2d9bb-113">提供應用程式的**名稱**（例如， **Blazor Server AAD B2C**）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="2d9bb-114">針對**支援的帳戶類型**，選取**任何組織目錄中的帳戶或任何識別提供者。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="2d9bb-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="2d9bb-115">（多租使用者）此體驗。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="2d9bb-116">在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="2d9bb-117">確認**許可權** > 將系統**管理員收到授與 openid，並已啟用 offline_access 許可權**。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="2d9bb-118">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-118">Select **Register**.</span></span>

<span data-ttu-id="2d9bb-119">在中**公開 API**：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="2d9bb-120">選取 [新增範圍]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="2d9bb-121">選取 [儲存並繼續]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="2d9bb-122">提供**範圍名稱**（例如，`API.Access`）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="2d9bb-123">提供系統**管理員同意顯示名稱**（例如，`Access API`）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="2d9bb-124">提供系統**管理員同意描述**（例如，`Allows the app to access server app API endpoints.`）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="2d9bb-125">確認 [**狀態**] 設定為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="2d9bb-126">選取 [**新增領域**]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-126">Select **Add scope**.</span></span>

<span data-ttu-id="2d9bb-127">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-127">Record the following information:</span></span>

* <span data-ttu-id="2d9bb-128">*伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）</span><span class="sxs-lookup"><span data-stu-id="2d9bb-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="2d9bb-129">目錄識別碼（租使用者識別碼）（例如 `222222222-2222-2222-2222-222222222222`）</span><span class="sxs-lookup"><span data-stu-id="2d9bb-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="2d9bb-130">*伺服器 API 應用程式*應用程式識別碼 URI （例如 `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`，Azure 入口網站可能會將值預設為用戶端識別碼）</span><span class="sxs-lookup"><span data-stu-id="2d9bb-130">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="2d9bb-131">預設範圍（例如 `API.Access`）</span><span class="sxs-lookup"><span data-stu-id="2d9bb-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="2d9bb-132">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="2d9bb-132">Register a client app</span></span>

<span data-ttu-id="2d9bb-133">請遵循教學課程[：在 Azure Active Directory B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications)中的指導方針，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中註冊*用戶端應用*程式的 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-133">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2d9bb-134">選取 [新增註冊]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-134">Select **New registration**.</span></span>
1. <span data-ttu-id="2d9bb-135">提供應用程式的**名稱**（例如， **Blazor 用戶端 AAD B2C**）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-135">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="2d9bb-136">針對**支援的帳戶類型**，選取**任何組織目錄中的帳戶或任何識別提供者。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="2d9bb-136">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="2d9bb-137">（多租使用者）此體驗。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-137">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="2d9bb-138">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供 `https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="2d9bb-139">確認**許可權** > 將系統**管理員收到授與 openid，並已啟用 offline_access 許可權**。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-139">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="2d9bb-140">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-140">Select **Register**.</span></span>

<span data-ttu-id="2d9bb-141">在 [**驗證** > **平臺**設定 > **Web**]：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="2d9bb-142">確認 `https://localhost:5001/authentication/login-callback` 的重新**導向 URI**存在。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="2d9bb-143">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="2d9bb-144">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="2d9bb-145">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-145">Select the **Save** button.</span></span>

<span data-ttu-id="2d9bb-146">在 [ **API 許可權**] 中：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-146">In **API permissions**:</span></span>

1. <span data-ttu-id="2d9bb-147">確認應用程式已**Microsoft Graph** > **使用者。讀取**許可權。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="2d9bb-148">選取 [**新增許可權**]，後面接著 [**我的 api**]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="2d9bb-149">從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如， **Blazor Server AAD B2C**）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="2d9bb-150">開啟 [ **API**清單]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-150">Open the **API** list.</span></span>
1. <span data-ttu-id="2d9bb-151">啟用 API 的存取權（例如，`API.Access`）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="2d9bb-152">選取 [新增權限]。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="2d9bb-153">選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="2d9bb-154">選取 [是] 加以確認。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="2d9bb-155">在**Home** > **Azure AD B2C** > **使用者流程**：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-155">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="2d9bb-156">建立註冊和登入使用者流程</span><span class="sxs-lookup"><span data-stu-id="2d9bb-156">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="2d9bb-157">至少選取 **應用程式宣告** > **顯示名稱**使用者屬性，以在 `LoginDisplay` 元件（*Shared/LoginDisplay*）中填入 `context.User.Identity.Name`。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-157">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="2d9bb-158">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-158">Record the following information:</span></span>

* <span data-ttu-id="2d9bb-159">記錄*用戶端應用*程式識別碼（用戶端識別碼）（例如 `33333333-3333-3333-3333-333333333333`）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-159">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="2d9bb-160">記錄為應用程式建立的註冊和登入使用者流程名稱（例如，`B2C_1_signupsignin`）。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-160">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="2d9bb-161">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2d9bb-161">Create the app</span></span>

<span data-ttu-id="2d9bb-162">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-162">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="2d9bb-163">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-163">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="2d9bb-164">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-164">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="2d9bb-165">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2d9bb-165">Server app configuration</span></span>

<span data-ttu-id="2d9bb-166">*本節適用于解決方案的**伺服器**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="2d9bb-166">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="2d9bb-167">驗證套件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-167">Authentication package</span></span>

<span data-ttu-id="2d9bb-168">`Microsoft.AspNetCore.Authentication.AzureAD.UI`提供驗證和授權呼叫 ASP.NET Core Web Api 的支援：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-168">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="2d9bb-169">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="2d9bb-169">Authentication service support</span></span>

<span data-ttu-id="2d9bb-170">`AddAuthentication` 方法會在應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-170">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="2d9bb-171">`AddAzureADBearer` 方法會設定 JWT 持有人處理常式中的特定參數，以驗證 Azure Active Directory 所發出的權杖：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-171">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="2d9bb-172">`UseAuthentication` 和 `UseAuthorization` 確保：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-172">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="2d9bb-173">應用程式會嘗試剖析並驗證傳入要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-173">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="2d9bb-174">嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-174">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="2d9bb-175">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2d9bb-175">App settings</span></span>

<span data-ttu-id="2d9bb-176">*Appsettings*包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-176">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://{ORGANIZATION}.b2clogin.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="2d9bb-177">WeatherForecast 控制器</span><span class="sxs-lookup"><span data-stu-id="2d9bb-177">WeatherForecast controller</span></span>

<span data-ttu-id="2d9bb-178">WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 `[Authorize]` 屬性套用至控制器。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-178">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="2d9bb-179">請**務必**瞭解：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-179">It's **important** to understand that:</span></span>

* <span data-ttu-id="2d9bb-180">此 API 控制器中的 `[Authorize]` 屬性是保護此 API 免于未經授權存取的唯一做法。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-180">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="2d9bb-181">Blazor WebAssembly 應用程式中使用的 `[Authorize]` 屬性僅做為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-181">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="2d9bb-182">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2d9bb-182">Client app configuration</span></span>

<span data-ttu-id="2d9bb-183">*本節適用于解決方案的**用戶端**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="2d9bb-183">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="2d9bb-184">驗證套件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-184">Authentication package</span></span>

<span data-ttu-id="2d9bb-185">當您建立應用程式以使用個別 B2C 帳戶（`IndividualB2C`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-185">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="2d9bb-186">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-186">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="2d9bb-187">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-187">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="2d9bb-188">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-188">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="2d9bb-189">`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-189">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="2d9bb-190">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="2d9bb-190">Authentication service support</span></span>

<span data-ttu-id="2d9bb-191">驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-191">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="2d9bb-192">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-192">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="2d9bb-193">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-193">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{DEFAULT SCOPE}");
});
```

<span data-ttu-id="2d9bb-194">`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-194">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="2d9bb-195">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-195">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="2d9bb-196">Blazor WebAssembly 範本會自動設定應用程式，以針對提供給 `dotnet new` 命令（`{APP ID URI}/{DEFAULT SCOPE}`）的預設範圍，要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-196">The Blazor WebAssembly template automatically configures the app to request an access token for a secure API for the default scope provided to the `dotnet new` command (`{APP ID URI}/{DEFAULT SCOPE}`).</span></span>

<span data-ttu-id="2d9bb-197">預設存取權杖範圍代表存取權杖範圍的清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="2d9bb-198">預設包含在登入要求中。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="2d9bb-199">用來在驗證之後立即布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="2d9bb-200">所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="2d9bb-201">您可以視需要為其他 API 應用程式新增額外的範圍：</span><span class="sxs-lookup"><span data-stu-id="2d9bb-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="2d9bb-202">索引頁面</span><span class="sxs-lookup"><span data-stu-id="2d9bb-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="2d9bb-203">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="2d9bb-204">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="2d9bb-205">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="2d9bb-206">驗證元件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="2d9bb-207">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="2d9bb-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="2d9bb-208">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="2d9bb-208">Run the app</span></span>

<span data-ttu-id="2d9bb-209">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-209">Run the app from the Server project.</span></span> <span data-ttu-id="2d9bb-210">使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d9bb-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="2d9bb-211">其他資源</span><span class="sxs-lookup"><span data-stu-id="2d9bb-211">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="2d9bb-212">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="2d9bb-212">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
