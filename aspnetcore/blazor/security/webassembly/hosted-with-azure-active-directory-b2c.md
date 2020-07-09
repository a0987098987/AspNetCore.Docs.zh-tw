---
title: Blazor WebAssembly使用 Azure Active Directory B2C 保護 ASP.NET Core 託管應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/08/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: b9125526db9a7484aca50f2ffa6175fd99b11453
ms.sourcegitcommit: f7873c02c1505c99106cbc708f37e18fc0a496d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147766"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="82c4b-102">Blazor WebAssembly使用 Azure Active Directory B2C 保護 ASP.NET Core 託管應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="82c4b-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82c4b-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="82c4b-104">本文說明如何建立 Blazor WebAssembly 使用[AZURE ACTIVE DIRECTORY （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的獨立應用程式。</span><span class="sxs-lookup"><span data-stu-id="82c4b-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="82c4b-105">在 AAD B2C 中註冊應用程式並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="82c4b-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="82c4b-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="82c4b-106">Create a tenant</span></span>

<span data-ttu-id="82c4b-107">依照[教學課程：建立 Azure Active Directory B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者中的指引來建立 AAD B2C 租使用者。</span><span class="sxs-lookup"><span data-stu-id="82c4b-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant.</span></span>

<span data-ttu-id="82c4b-108">記錄 AAD B2C 實例（例如， `https://contoso.b2clogin.com/` 包含尾端斜線的）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-108">Record the AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash).</span></span> <span data-ttu-id="82c4b-109">實例是 Azure B2C 應用程式註冊的配置和主機，您可以從 Azure 入口網站的 [**應用程式註冊**] 頁面中開啟 [**端點**] 視窗來找到此功能。</span><span class="sxs-lookup"><span data-stu-id="82c4b-109">The instance is the scheme and host of an Azure B2C app registration, which can be found by opening the **Endpoints** window from the **App registrations** page in the Azure portal.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="82c4b-110">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-110">Register a server API app</span></span>

<span data-ttu-id="82c4b-111">請遵循教學課程[：在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*伺服器 API 應用*程式註冊 AAD 應用程式，然後執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="82c4b-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* and then do the following:</span></span>

1. <span data-ttu-id="82c4b-112">在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-112">In **Azure Active Directory** > **App registrations**, select **New registration**.</span></span>
1. <span data-ttu-id="82c4b-113">提供應用程式的**名稱**（例如， \*\* Blazor Server AAD B2C\*\*）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="82c4b-114">針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="82c4b-114">For **Supported account types**, select the multi-tenant option: **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span>
1. <span data-ttu-id="82c4b-115">在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-115">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="82c4b-116">確認**Permissions**  >  已啟用 [許可權]，授與系統**管理員同意 openid 和 offline_access 的許可權**。</span><span class="sxs-lookup"><span data-stu-id="82c4b-116">Confirm that **Permissions** > **Grant admin consent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="82c4b-117">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-117">Select **Register**.</span></span>

<span data-ttu-id="82c4b-118">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="82c4b-118">Record the following information:</span></span>

* <span data-ttu-id="82c4b-119">*伺服器 API 應用程式*應用程式（用戶端）識別碼（例如， `41451fa7-82d9-4673-8fa5-69eff5a761fd` ）</span><span class="sxs-lookup"><span data-stu-id="82c4b-119">*Server API app* Application (client) ID (for example, `41451fa7-82d9-4673-8fa5-69eff5a761fd`)</span></span>
* <span data-ttu-id="82c4b-120">AAD 主要/發行者/租使用者網域（例如， `contoso.onmicrosoft.com` ）：在已註冊的應用程式之 Azure 入口網站的 [**商標**] 分頁中，此網域可作為 [**發行者] 網域**。</span><span class="sxs-lookup"><span data-stu-id="82c4b-120">AAD Primary/Publisher/Tenant domain (for example, `contoso.onmicrosoft.com`): The domain is available as the **Publisher domain** in the **Branding** blade of the Azure portal for the registered app.</span></span>

<span data-ttu-id="82c4b-121">在中**公開 API**：</span><span class="sxs-lookup"><span data-stu-id="82c4b-121">In **Expose an API**:</span></span>

1. <span data-ttu-id="82c4b-122">選取 [新增範圍]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-122">Select **Add a scope**.</span></span>
1. <span data-ttu-id="82c4b-123">選取 [儲存並繼續]  。</span><span class="sxs-lookup"><span data-stu-id="82c4b-123">Select **Save and continue**.</span></span>
1. <span data-ttu-id="82c4b-124">提供**範圍名稱**（例如， `API.Access` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-124">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="82c4b-125">提供系統**管理員同意顯示名稱**（例如 `Access API` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-125">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="82c4b-126">提供系統**管理員同意描述**（例如 `Allows the app to access server app API endpoints.` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-126">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="82c4b-127">確認 [**狀態**] 設定為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-127">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="82c4b-128">選取 [新增範圍]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-128">Select **Add scope**.</span></span>

<span data-ttu-id="82c4b-129">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="82c4b-129">Record the following information:</span></span>

* <span data-ttu-id="82c4b-130">應用程式識別碼 URI （例如， `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd` 、 `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` 或您提供的自訂值）</span><span class="sxs-lookup"><span data-stu-id="82c4b-130">App ID URI (for example, `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd`, `api://41451fa7-82d9-4673-8fa5-69eff5a761fd`, or the custom value that you provided)</span></span>
* <span data-ttu-id="82c4b-131">預設範圍（例如， `API.Access` ）</span><span class="sxs-lookup"><span data-stu-id="82c4b-131">Default scope (for example, `API.Access`)</span></span>

<span data-ttu-id="82c4b-132">應用程式識別碼 URI 可能需要用戶端應用程式中的特殊設定，如本主題稍後的[存取權杖範圍](#access-token-scopes)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="82c4b-132">The App ID URI might require a special configuration in the client app, which is described in the [Access token scopes](#access-token-scopes) section later in this topic.</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="82c4b-133">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-133">Register a client app</span></span>

<span data-ttu-id="82c4b-134">請遵循教學課程[：再次在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*用戶端應用程式*註冊 AAD 應用程式，然後執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="82c4b-134">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* and then do the following:</span></span>

1. <span data-ttu-id="82c4b-135">在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-135">In **Azure Active Directory** > **App registrations**, select **New registration**.</span></span>
1. <span data-ttu-id="82c4b-136">提供應用程式的**名稱**（例如， \*\* Blazor 用戶端 AAD B2C\*\*）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-136">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="82c4b-137">針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="82c4b-137">For **Supported account types**, select the multi-tenant option: **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span>
1. <span data-ttu-id="82c4b-138">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="82c4b-138">Leave the **Redirect URI** drop down set to **Web** and provide the following redirect URI: `https://localhost:{PORT}/authentication/login-callback`.</span></span> <span data-ttu-id="82c4b-139">在 Kestrel 上執行之應用程式的預設埠是5001。</span><span class="sxs-lookup"><span data-stu-id="82c4b-139">The default port for an app running on Kestrel is 5001.</span></span> <span data-ttu-id="82c4b-140">如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。</span><span class="sxs-lookup"><span data-stu-id="82c4b-140">If the app is run on a different Kestrel port, use the app's port.</span></span> <span data-ttu-id="82c4b-141">針對 IIS Express，在 [**調試**程式] 面板的伺服器應用程式屬性中，可以找到應用程式的隨機產生埠。</span><span class="sxs-lookup"><span data-stu-id="82c4b-141">For IIS Express, the randomly generated port for the app can be found in the Server app's properties in the **Debug** panel.</span></span> <span data-ttu-id="82c4b-142">由於應用程式目前不存在，且 IIS Express 埠未知，請在建立應用程式之後返回此步驟，並更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="82c4b-142">Since the app doesn't exist at this point and the IIS Express port isn't known, return to this step after the app is created and update the redirect URI.</span></span> <span data-ttu-id="82c4b-143">[[建立應用程式](#create-the-app)] 區段中會出現一個批註，提醒 IIS Express 使用者更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="82c4b-143">A remark appears in the [Create the app](#create-the-app) section to remind IIS Express users to update the redirect URI.</span></span>
1. <span data-ttu-id="82c4b-144">確認**Permissions**  >  已啟用 [許可權]，授與系統**管理員同意 openid 和 offline_access 的許可權**。</span><span class="sxs-lookup"><span data-stu-id="82c4b-144">Confirm that **Permissions** > **Grant admin consent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="82c4b-145">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-145">Select **Register**.</span></span>

<span data-ttu-id="82c4b-146">記錄應用程式（用戶端）識別碼（例如 `4369008b-21fa-427c-abaa-9b53bf58e538` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-146">Record the Application (client) ID (for example, `4369008b-21fa-427c-abaa-9b53bf58e538`).</span></span>

<span data-ttu-id="82c4b-147">在 [**驗證**  >  **平臺**設定]  >  **Web**：</span><span class="sxs-lookup"><span data-stu-id="82c4b-147">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="82c4b-148">確認的重新**導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。</span><span class="sxs-lookup"><span data-stu-id="82c4b-148">Confirm the **Redirect URI** of `https://localhost:{PORT}/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="82c4b-149">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="82c4b-149">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="82c4b-150">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="82c4b-150">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="82c4b-151">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82c4b-151">Select the **Save** button.</span></span>

<span data-ttu-id="82c4b-152">在 [ **API 許可權**] 中：</span><span class="sxs-lookup"><span data-stu-id="82c4b-152">In **API permissions**:</span></span>

1. <span data-ttu-id="82c4b-153">選取 [**新增許可權**]，後面接著 [**我的 api**]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-153">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="82c4b-154">從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如， \*\* Blazor Server AAD B2C\*\*）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-154">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="82c4b-155">開啟 [ **API**清單]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-155">Open the **API** list.</span></span>
1. <span data-ttu-id="82c4b-156">啟用 API 的存取權（例如， `API.Access` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-156">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="82c4b-157">選取 [新增權限]。</span><span class="sxs-lookup"><span data-stu-id="82c4b-157">Select **Add permissions**.</span></span>
1. <span data-ttu-id="82c4b-158">選取 [為 **{租使用者名稱} 授與系統管理員同意**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82c4b-158">Select the **Grant admin consent for {TENANT NAME}** button.</span></span> <span data-ttu-id="82c4b-159">選取 [是] \*\*\*\* 加以確認。</span><span class="sxs-lookup"><span data-stu-id="82c4b-159">Select **Yes** to confirm.</span></span>

<span data-ttu-id="82c4b-160">在**Home**  >  **Azure AD B2C**  >  **使用者流程**：</span><span class="sxs-lookup"><span data-stu-id="82c4b-160">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="82c4b-161">建立註冊和登入使用者流程</span><span class="sxs-lookup"><span data-stu-id="82c4b-161">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="82c4b-162">至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` 元件中的 `LoginDisplay` （ `Shared/LoginDisplay.razor` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-162">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (`Shared/LoginDisplay.razor`).</span></span>

<span data-ttu-id="82c4b-163">記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-163">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="82c4b-164">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-164">Create the app</span></span>

<span data-ttu-id="82c4b-165">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="82c4b-165">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -o {APP NAME} -ssp "{SIGN UP OR SIGN IN POLICY}"
```

| <span data-ttu-id="82c4b-166">預留位置</span><span class="sxs-lookup"><span data-stu-id="82c4b-166">Placeholder</span></span>                   | <span data-ttu-id="82c4b-167">Azure 入口網站名稱</span><span class="sxs-lookup"><span data-stu-id="82c4b-167">Azure portal name</span></span>                                     | <span data-ttu-id="82c4b-168">範例</span><span class="sxs-lookup"><span data-stu-id="82c4b-168">Example</span></span>                                |
| ----------------------------- | ----------------------------------------------------- | -------------------------------------- |
| `{AAD B2C INSTANCE}`          | <span data-ttu-id="82c4b-169">執行個體</span><span class="sxs-lookup"><span data-stu-id="82c4b-169">Instance</span></span>                                              | `https://contoso.b2clogin.com/`        |
| `{APP NAME}`                  | &mdash;                                               | `BlazorSample`                         |
| `{CLIENT APP CLIENT ID}`      | <span data-ttu-id="82c4b-170">*用戶端應用*程式的應用程式（用戶端）識別碼</span><span class="sxs-lookup"><span data-stu-id="82c4b-170">Application (client) ID for the *Client app*</span></span>          | `4369008b-21fa-427c-abaa-9b53bf58e538` |
| `{DEFAULT SCOPE}`             | <span data-ttu-id="82c4b-171">領域名稱</span><span class="sxs-lookup"><span data-stu-id="82c4b-171">Scope name</span></span>                                            | `API.Access`                           |
| `{SERVER API APP CLIENT ID}`  | <span data-ttu-id="82c4b-172">*伺服器 API 應用*程式的應用程式（用戶端）識別碼</span><span class="sxs-lookup"><span data-stu-id="82c4b-172">Application (client) ID for the *Server API app*</span></span>      | `41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{SERVER API APP ID URI}`     | <span data-ttu-id="82c4b-173">應用程式識別碼 URI （[請參閱附注](#access-token-scopes)）</span><span class="sxs-lookup"><span data-stu-id="82c4b-173">Application ID URI ([see note](#access-token-scopes))</span></span> | `41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{SIGN UP OR SIGN IN POLICY}` | <span data-ttu-id="82c4b-174">註冊/登入使用者流程</span><span class="sxs-lookup"><span data-stu-id="82c4b-174">Sign-up/sign-in user flow</span></span>                             | `B2C_1_signupsignin1`                  |
| `{TENANT DOMAIN}`             | <span data-ttu-id="82c4b-175">主要/發行者/租使用者網域</span><span class="sxs-lookup"><span data-stu-id="82c4b-175">Primary/Publisher/Tenant domain</span></span>                       | `contoso.onmicrosoft.com`              |

<span data-ttu-id="82c4b-176">使用選項指定的輸出位置會 `-o|--output` 建立專案資料夾（如果不存在），並成為應用程式名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="82c4b-176">The output location specified with the `-o|--output` option creates a project folder if it doesn't exist and becomes part of the app's name.</span></span>

> [!NOTE]
> <span data-ttu-id="82c4b-177">將應用程式識別碼 URI 傳遞給 `app-id-uri` 選項，但請注意，在用戶端應用程式中可能需要進行設定變更，如[存取權杖範圍](#access-token-scopes)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="82c4b-177">Pass the App ID URI to the `app-id-uri` option, but note a configuration change might be required in the client app, which is described in the [Access token scopes](#access-token-scopes) section.</span></span>
>
> <span data-ttu-id="82c4b-178">此外，由裝載的範本所設定的範圍 Blazor 可能會重複應用程式識別碼 URI 主機。</span><span class="sxs-lookup"><span data-stu-id="82c4b-178">Additionally, the scope set up by the Hosted Blazor template might have the App ID URI host repeated.</span></span> <span data-ttu-id="82c4b-179">確認 `DefaultAccessTokenScopes` 在 `Program.Main` `Program.cs` *用戶端應用程式*的（）中為集合設定的範圍是正確的。</span><span class="sxs-lookup"><span data-stu-id="82c4b-179">Confirm that the scope configured for the `DefaultAccessTokenScopes` collection is correct in `Program.Main` (`Program.cs`) of the *Client app*.</span></span>

> [!NOTE]
> <span data-ttu-id="82c4b-180">在 Azure 入口網站中，*用戶端應用程式的\*\*\*驗證*\*  >  **平臺**  >  設定**Web**重新  >  **導向 URI**會針對使用預設設定在 Kestrel 伺服器上執行的應用程式，設定為埠5001。</span><span class="sxs-lookup"><span data-stu-id="82c4b-180">In the Azure portal, the *Client app's* **Authentication** > **Platform configurations** > **Web** > **Redirect URI** is configured for port 5001 for apps that run on the Kestrel server with default settings.</span></span>
>
> <span data-ttu-id="82c4b-181">如果*用戶端應用程式*是在隨機 IIS Express 埠上執行，則可以在 [**調試**程式] 面板的*伺服器 API 應用程式*屬性中找到應用程式的埠。</span><span class="sxs-lookup"><span data-stu-id="82c4b-181">If the *Client app* is run on a random IIS Express port, the port for the app can be found in the *Server API app's* properties in the **Debug** panel.</span></span>
>
> <span data-ttu-id="82c4b-182">如果未在*用戶端應用程式的*已知埠之前設定埠，請回到 Azure 入口網站中的*用戶端應用程式*註冊，並使用正確的埠更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="82c4b-182">If the port wasn't configured earlier with the *Client app's* known port, return to the *Client app's* registration in the Azure portal and update the redirect URI with the correct port.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="82c4b-183">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="82c4b-183">Server app configuration</span></span>

<span data-ttu-id="82c4b-184">*本節適用于解決方案的 **`Server`** 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="82c4b-184">*This section pertains to the solution's **`Server`** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="82c4b-185">驗證套件</span><span class="sxs-lookup"><span data-stu-id="82c4b-185">Authentication package</span></span>

<span data-ttu-id="82c4b-186">封裝會提供驗證和授權呼叫 ASP.NET Core Web Api 的支援 [`Microsoft.AspNetCore.Authentication.AzureADB2C.UI`](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI/) ：</span><span class="sxs-lookup"><span data-stu-id="82c4b-186">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the [`Microsoft.AspNetCore.Authentication.AzureADB2C.UI`](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI/) package:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureADB2C.UI" 
  Version="3.1.4" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="82c4b-187">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="82c4b-187">Authentication service support</span></span>

<span data-ttu-id="82c4b-188">方法會在 `AddAuthentication` 應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="82c4b-188">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="82c4b-189"><xref:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions.AddAzureADB2CBearer%2A>方法會設定 JWT 持有人處理常式中所需的特定參數，以驗證 Azure Active Directory B2C 所發出的權杖：</span><span class="sxs-lookup"><span data-stu-id="82c4b-189">The <xref:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions.AddAzureADB2CBearer%2A> method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory B2C:</span></span>

```csharp
services.AddAuthentication(AzureADB2CDefaults.BearerAuthenticationScheme)
    .AddAzureADB2CBearer(options => Configuration.Bind("AzureAdB2C", options));
```

<span data-ttu-id="82c4b-190"><xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A>並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> 確定：</span><span class="sxs-lookup"><span data-stu-id="82c4b-190"><xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A> and <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> ensure that:</span></span>

* <span data-ttu-id="82c4b-191">應用程式會嘗試剖析並驗證傳入要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="82c4b-191">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="82c4b-192">嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。</span><span class="sxs-lookup"><span data-stu-id="82c4b-192">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a><span data-ttu-id="82c4b-193">使用者. Identity 。檔案名</span><span class="sxs-lookup"><span data-stu-id="82c4b-193">User.Identity.Name</span></span>

<span data-ttu-id="82c4b-194">根據預設， `User.Identity.Name` 不會填入。</span><span class="sxs-lookup"><span data-stu-id="82c4b-194">By default, the `User.Identity.Name` isn't populated.</span></span>

<span data-ttu-id="82c4b-195">若要將應用程式設定為從宣告類型接收值 `name` ，請 <xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType?displayProperty=nameWithType> <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> 在中設定的 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="82c4b-195">To configure the app to receive the value from the `name` claim type, configure the <xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType?displayProperty=nameWithType> of the <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> in `Startup.ConfigureServices`:</span></span>

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADB2CDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a><span data-ttu-id="82c4b-196">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="82c4b-196">App settings</span></span>

<span data-ttu-id="82c4b-197">檔案 `appsettings.json` 包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。</span><span class="sxs-lookup"><span data-stu-id="82c4b-197">The `appsettings.json` file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAdB2C": {
    "Instance": "https://{TENANT}.b2clogin.com/",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "Domain": "{TENANT DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

<span data-ttu-id="82c4b-198">範例：</span><span class="sxs-lookup"><span data-stu-id="82c4b-198">Example:</span></span>

```json
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com/",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "Domain": "contoso.onmicrosoft.com",
    "SignUpSignInPolicyId": "B2C_1_signupsignin1",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="82c4b-199">WeatherForecast 控制器</span><span class="sxs-lookup"><span data-stu-id="82c4b-199">WeatherForecast controller</span></span>

<span data-ttu-id="82c4b-200">WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至控制器。</span><span class="sxs-lookup"><span data-stu-id="82c4b-200">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute applied to the controller.</span></span> <span data-ttu-id="82c4b-201">請**務必**瞭解：</span><span class="sxs-lookup"><span data-stu-id="82c4b-201">It's **important** to understand that:</span></span>

* <span data-ttu-id="82c4b-202">[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)此 api 控制器中的屬性是保護此 api 免于未經授權存取的唯一做法。</span><span class="sxs-lookup"><span data-stu-id="82c4b-202">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="82c4b-203">[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)應用程式中所使用的屬性 Blazor WebAssembly 只會作為應用程式的提示，使用者應該獲得授權，應用程式才能正確運作。</span><span class="sxs-lookup"><span data-stu-id="82c4b-203">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="82c4b-204">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="82c4b-204">Client app configuration</span></span>

<span data-ttu-id="82c4b-205">*本節適用于解決方案的 **`Client`** 應用程式。*</span><span class="sxs-lookup"><span data-stu-id="82c4b-205">*This section pertains to the solution's **`Client`** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="82c4b-206">驗證套件</span><span class="sxs-lookup"><span data-stu-id="82c4b-206">Authentication package</span></span>

<span data-ttu-id="82c4b-207">建立應用程式以使用個別 B2C 帳戶（ `IndividualB2C` ）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（）的套件參考 [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) 。</span><span class="sxs-lookup"><span data-stu-id="82c4b-207">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) ([`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)).</span></span> <span data-ttu-id="82c4b-208">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="82c4b-208">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="82c4b-209">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="82c4b-209">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

<span data-ttu-id="82c4b-210">[`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)封裝可傳遞會將 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="82c4b-210">The [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) package transitively adds the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="82c4b-211">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="82c4b-211">Authentication service support</span></span>

<span data-ttu-id="82c4b-212"><xref:System.Net.Http.HttpClient>加入實例的支援，其中包含對伺服器專案提出要求時的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="82c4b-212">Support for <xref:System.Net.Http.HttpClient> instances is added that include access tokens when making requests to the server project.</span></span>

<span data-ttu-id="82c4b-213">`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="82c4b-213">`Program.cs`:</span></span>

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

<span data-ttu-id="82c4b-214">預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱（例如， `BlazorSample.ServerAPI` ）。</span><span class="sxs-lookup"><span data-stu-id="82c4b-214">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `BlazorSample.ServerAPI`).</span></span>

<span data-ttu-id="82c4b-215">使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) 。</span><span class="sxs-lookup"><span data-stu-id="82c4b-215">Support for authenticating users is registered in the service container with the <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> extension method provided by the [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) package.</span></span> <span data-ttu-id="82c4b-216">這個方法會設定應用程式與 Identity 提供者（IP）互動所需的服務。</span><span class="sxs-lookup"><span data-stu-id="82c4b-216">This method sets up the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="82c4b-217">`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="82c4b-217">`Program.cs`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<span data-ttu-id="82c4b-218"><xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="82c4b-218">The <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="82c4b-219">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="82c4b-219">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="82c4b-220">設定是由檔案所提供 `wwwroot/appsettings.json` ：</span><span class="sxs-lookup"><span data-stu-id="82c4b-220">Configuration is supplied by the `wwwroot/appsettings.json` file:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{TENANT DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": false
  }
}
```

<span data-ttu-id="82c4b-221">範例：</span><span class="sxs-lookup"><span data-stu-id="82c4b-221">Example:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": false
  }
}
```

### <a name="access-token-scopes"></a><span data-ttu-id="82c4b-222">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="82c4b-222">Access token scopes</span></span>

<span data-ttu-id="82c4b-223">預設存取權杖範圍代表存取權杖範圍的清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82c4b-223">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="82c4b-224">預設包含在登入要求中。</span><span class="sxs-lookup"><span data-stu-id="82c4b-224">Included by default in the sign in request.</span></span>
* <span data-ttu-id="82c4b-225">用來在驗證之後立即布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="82c4b-225">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="82c4b-226">所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。</span><span class="sxs-lookup"><span data-stu-id="82c4b-226">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="82c4b-227">您可以視需要為其他 API 應用程式新增額外的範圍：</span><span class="sxs-lookup"><span data-stu-id="82c4b-227">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

<span data-ttu-id="82c4b-228">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="82c4b-228">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="82c4b-229">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="82c4b-229">Request additional access tokens</span></span>](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="82c4b-230">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="82c4b-230">Attach tokens to outgoing requests</span></span>](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)


### <a name="imports-file"></a><span data-ttu-id="82c4b-231">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="82c4b-231">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="82c4b-232">索引頁面</span><span class="sxs-lookup"><span data-stu-id="82c4b-232">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="82c4b-233">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="82c4b-233">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="82c4b-234">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="82c4b-234">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="82c4b-235">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="82c4b-235">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="82c4b-236">驗證元件</span><span class="sxs-lookup"><span data-stu-id="82c4b-236">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="82c4b-237">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="82c4b-237">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="82c4b-238">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="82c4b-238">Run the app</span></span>

<span data-ttu-id="82c4b-239">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="82c4b-239">Run the app from the Server project.</span></span> <span data-ttu-id="82c4b-240">使用 Visual Studio 時，您可以：</span><span class="sxs-lookup"><span data-stu-id="82c4b-240">When using Visual Studio, either:</span></span>

* <span data-ttu-id="82c4b-241">將工具列中的 [**啟始專案**] 下拉式清單設定為*伺服器 API 應用程式*，然後選取 [**執行**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="82c4b-241">Set the **Startup Projects** drop down list in the toolbar to the *Server API app* and select the **Run** button.</span></span>
* <span data-ttu-id="82c4b-242">在**方案總管**中選取伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="82c4b-242">Select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="82c4b-243">其他資源</span><span class="sxs-lookup"><span data-stu-id="82c4b-243">Additional resources</span></span>

* <xref:blazor/security/webassembly/additional-scenarios>
* [<span data-ttu-id="82c4b-244">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="82c4b-244">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="82c4b-245">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="82c4b-245">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="82c4b-246">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="82c4b-246">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
