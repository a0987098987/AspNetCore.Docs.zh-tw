---
title: Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 託管應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: b369bf0e9b20bcb87345e3e10c314ae6227464d1
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84215092"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="7e0ae-102">Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 託管應用程式</span><span class="sxs-lookup"><span data-stu-id="7e0ae-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="7e0ae-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7e0ae-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7e0ae-104">本文說明如何建立 Blazor WebAssembly 獨立應用程式，以使用[AZURE ACTIVE DIRECTORY （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="7e0ae-105">在 AAD B2C 中註冊應用程式並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="7e0ae-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="7e0ae-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="7e0ae-106">Create a tenant</span></span>

<span data-ttu-id="7e0ae-107">依照[教學課程：建立 Azure Active Directory B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者中的指引來建立 AAD B2C 租使用者。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant.</span></span>

<span data-ttu-id="7e0ae-108">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-108">Record the following information:</span></span>

* <span data-ttu-id="7e0ae-109">AAD B2C 實例（例如， `https://contoso.b2clogin.com/` 包含尾端斜線的）</span><span class="sxs-lookup"><span data-stu-id="7e0ae-109">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="7e0ae-110">AAD B2C 租使用者網域（例如， `contoso.onmicrosoft.com` ）</span><span class="sxs-lookup"><span data-stu-id="7e0ae-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="7e0ae-111">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="7e0ae-111">Register a server API app</span></span>

<span data-ttu-id="7e0ae-112">請遵循教學課程[：在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*伺服器 API 應用*程式註冊 AAD 應用程式，然後執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-112">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* and then do the following:</span></span>

1. <span data-ttu-id="7e0ae-113">在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-113">In **Azure Active Directory** > **App registrations**, select **New registration**.</span></span>
1. <span data-ttu-id="7e0ae-114">提供應用程式的**名稱**（例如， \*\* Blazor 伺服器 AAD B2C\*\*）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-114">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="7e0ae-115">針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="7e0ae-115">For **Supported account types**, select the multi-tenant option: **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span>
1. <span data-ttu-id="7e0ae-116">在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="7e0ae-117">確認**許可權**  >  **授與系統管理員收到給 openid，並已啟用 offline_access 許可權**。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="7e0ae-118">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-118">Select **Register**.</span></span>

<span data-ttu-id="7e0ae-119">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-119">Record the following information:</span></span>

* <span data-ttu-id="7e0ae-120">*伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如， `11111111-1111-1111-1111-111111111111` ）</span><span class="sxs-lookup"><span data-stu-id="7e0ae-120">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="7e0ae-121">目錄識別碼（租使用者識別碼）（例如， `222222222-2222-2222-2222-222222222222` ）</span><span class="sxs-lookup"><span data-stu-id="7e0ae-121">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="7e0ae-122">AAD 租使用者網域（例如， `contoso.onmicrosoft.com` ）：在已註冊的應用程式之 Azure 入口網站的 [**商標**] 分頁中，網域會以**發行者網域**的形式提供。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-122">AAD Tenant domain (for example, `contoso.onmicrosoft.com`): The domain is available as the **Publisher domain** in the **Branding** blade of the Azure portal for the registered app.</span></span>

<span data-ttu-id="7e0ae-123">在中**公開 API**：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-123">In **Expose an API**:</span></span>

1. <span data-ttu-id="7e0ae-124">選取 [新增範圍]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-124">Select **Add a scope**.</span></span>
1. <span data-ttu-id="7e0ae-125">選取 [儲存並繼續]  。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-125">Select **Save and continue**.</span></span>
1. <span data-ttu-id="7e0ae-126">提供**範圍名稱**（例如， `API.Access` ）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-126">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="7e0ae-127">提供系統**管理員同意顯示名稱**（例如 `Access API` ）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-127">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="7e0ae-128">提供系統**管理員同意描述**（例如 `Allows the app to access server app API endpoints.` ）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-128">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="7e0ae-129">確認 [**狀態**] 設定為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-129">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="7e0ae-130">選取 [新增範圍]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-130">Select **Add scope**.</span></span>

<span data-ttu-id="7e0ae-131">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-131">Record the following information:</span></span>

* <span data-ttu-id="7e0ae-132">應用程式識別碼 URI （例如， `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111` 、 `api://11111111-1111-1111-1111-111111111111` 或您提供的自訂值）</span><span class="sxs-lookup"><span data-stu-id="7e0ae-132">App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, `api://11111111-1111-1111-1111-111111111111`, or the custom value that you provided)</span></span>
* <span data-ttu-id="7e0ae-133">預設範圍（例如， `API.Access` ）</span><span class="sxs-lookup"><span data-stu-id="7e0ae-133">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="7e0ae-134">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="7e0ae-134">Register a client app</span></span>

<span data-ttu-id="7e0ae-135">請遵循教學課程[：再次在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*用戶端應用程式*註冊 AAD 應用程式，然後執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-135">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* and then do the following:</span></span>

1. <span data-ttu-id="7e0ae-136">在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-136">In **Azure Active Directory** > **App registrations**, select **New registration**.</span></span>
1. <span data-ttu-id="7e0ae-137">提供應用程式的**名稱**（例如， \*\* Blazor 用戶端 AAD B2C\*\*）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-137">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="7e0ae-138">針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**</span><span class="sxs-lookup"><span data-stu-id="7e0ae-138">For **Supported account types**, select the multi-tenant option: **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span>
1. <span data-ttu-id="7e0ae-139">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-139">Leave the **Redirect URI** drop down set to **Web** and provide the following redirect URI: `https://localhost:{PORT}/authentication/login-callback`.</span></span> <span data-ttu-id="7e0ae-140">在 Kestrel 上執行之應用程式的預設埠是5001。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-140">The default port for an app running on Kestrel is 5001.</span></span> <span data-ttu-id="7e0ae-141">如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-141">If the app is run on a different Kestrel port, use the app's port.</span></span> <span data-ttu-id="7e0ae-142">針對 IIS Express，在 [**調試**程式] 面板的伺服器應用程式屬性中，可以找到應用程式的隨機產生埠。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-142">For IIS Express, the randomly generated port for the app can be found in the Server app's properties in the **Debug** panel.</span></span> <span data-ttu-id="7e0ae-143">由於應用程式目前不存在，且 IIS Express 埠未知，請在建立應用程式之後返回此步驟，並更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-143">Since the app doesn't exist at this point and the IIS Express port isn't known, return to this step after the app is created and update the redirect URI.</span></span> <span data-ttu-id="7e0ae-144">[[建立應用程式](#create-the-app)] 區段中會出現一個批註，提醒 IIS Express 使用者更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-144">A remark appears in the [Create the app](#create-the-app) section to remind IIS Express users to update the redirect URI.</span></span>
1. <span data-ttu-id="7e0ae-145">確認**許可權**  >  **授與系統管理員收到給 openid，並已啟用 offline_access 許可權**。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-145">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="7e0ae-146">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-146">Select **Register**.</span></span>

<span data-ttu-id="7e0ae-147">記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111` ）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-147">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

<span data-ttu-id="7e0ae-148">在 [**驗證**  >  **平臺**設定]  >  **Web**：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-148">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="7e0ae-149">確認的重新**導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-149">Confirm the **Redirect URI** of `https://localhost:{PORT}/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="7e0ae-150">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-150">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="7e0ae-151">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-151">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="7e0ae-152">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-152">Select the **Save** button.</span></span>

<span data-ttu-id="7e0ae-153">在 [ **API 許可權**] 中：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-153">In **API permissions**:</span></span>

1. <span data-ttu-id="7e0ae-154">選取 [**新增許可權**]，後面接著 [**我的 api**]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-154">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="7e0ae-155">從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如，[ \*\* Blazor 伺服器 AAD B2C\*\*]）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-155">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="7e0ae-156">開啟 [ **API**清單]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-156">Open the **API** list.</span></span>
1. <span data-ttu-id="7e0ae-157">啟用 API 的存取權（例如， `API.Access` ）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-157">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="7e0ae-158">選取 [新增權限]。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-158">Select **Add permissions**.</span></span>
1. <span data-ttu-id="7e0ae-159">選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-159">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="7e0ae-160">選取 [是]  確認。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-160">Select **Yes** to confirm.</span></span>

<span data-ttu-id="7e0ae-161">在**Home**  >  **Azure AD B2C**  >  **使用者流程**：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-161">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="7e0ae-162">建立註冊和登入使用者流程</span><span class="sxs-lookup"><span data-stu-id="7e0ae-162">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="7e0ae-163">至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` `LoginDisplay` 元件（*Shared/LoginDisplay*）中的。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-163">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="7e0ae-164">記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin` ）。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-164">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="7e0ae-165">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="7e0ae-165">Create the app</span></span>

<span data-ttu-id="7e0ae-166">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-166">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="7e0ae-167">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-167">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="7e0ae-168">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-168">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="7e0ae-169">將應用程式識別碼 URI 傳遞給 `app-id-uri` 選項，但請注意，在用戶端應用程式中可能需要進行設定變更，如[存取權杖範圍](#access-token-scopes)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-169">Pass the App ID URI to the `app-id-uri` option, but note a configuration change might be required in the client app, which is described in the [Access token scopes](#access-token-scopes) section.</span></span>
>
> <span data-ttu-id="7e0ae-170">此外，由裝載的範本所設定的範圍 Blazor 可能會重複應用程式識別碼 URI 主機。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-170">Additionally, the scope set up by the Hosted Blazor template might have the App ID URI host repeated.</span></span> <span data-ttu-id="7e0ae-171">確認 `DefaultAccessTokenScopes` 在 `Program.Main` *用戶端應用程式*的（*Program.cs*）中為集合設定的範圍是正確的。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-171">Confirm that the scope configured for the `DefaultAccessTokenScopes` collection is correct in `Program.Main` (*Program.cs*) of the *Client app*.</span></span>

> [!NOTE]
> <span data-ttu-id="7e0ae-172">在 Azure 入口網站中，*用戶端應用程式的\*\*\*驗證*\*  >  **平臺**  >  設定**Web**重新  >  **導向 URI**會針對使用預設設定在 Kestrel 伺服器上執行的應用程式，設定為埠5001。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-172">In the Azure portal, the *Client app's* **Authentication** > **Platform configurations** > **Web** > **Redirect URI** is configured for port 5001 for apps that run on the Kestrel server with default settings.</span></span>
>
> <span data-ttu-id="7e0ae-173">如果*用戶端應用程式*是在隨機 IIS Express 埠上執行，則可以在 [**調試**程式] 面板的*伺服器應用程式*屬性中找到應用程式的埠。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-173">If the *Client app* is run on a random IIS Express port, the port for the app can be found in the *Server app's* properties in the **Debug** panel.</span></span>
>
> <span data-ttu-id="7e0ae-174">如果未在*用戶端應用程式的*已知埠之前設定埠，請回到 Azure 入口網站中的*用戶端應用程式*註冊，並使用正確的埠更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-174">If the port wasn't configured earlier with the *Client app's* known port, return to the *Client app's* registration in the Azure portal and update the redirect URI with the correct port.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="7e0ae-175">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="7e0ae-175">Server app configuration</span></span>

<span data-ttu-id="7e0ae-176">*本節適用于解決方案的**伺服器**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="7e0ae-176">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="7e0ae-177">驗證套件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-177">Authentication package</span></span>

<span data-ttu-id="7e0ae-178">對 ASP.NET Core Web Api 的驗證和授權呼叫的支援，是由[AspNetCore. AzureADB2C. UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI/)套件所提供：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-178">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the [Microsoft.AspNetCore.Authentication.AzureADB2C.UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI/) package:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureADB2C.UI" 
  Version="3.1.4" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="7e0ae-179">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="7e0ae-179">Authentication service support</span></span>

<span data-ttu-id="7e0ae-180">方法會在 `AddAuthentication` 應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-180">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="7e0ae-181"><xref:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions.AddAzureADB2CBearer%2A>方法會設定 JWT 持有人處理常式中所需的特定參數，以驗證 Azure Active Directory B2C 所發出的權杖：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-181">The <xref:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions.AddAzureADB2CBearer%2A> method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory B2C:</span></span>

```csharp
services.AddAuthentication(AzureADB2CDefaults.BearerAuthenticationScheme)
    .AddAzureADB2CBearer(options => Configuration.Bind("AzureAdB2C", options));
```

<span data-ttu-id="7e0ae-182"><xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A>並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> 確定：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-182"><xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A> and <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> ensure that:</span></span>

* <span data-ttu-id="7e0ae-183">應用程式會嘗試剖析並驗證傳入要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-183">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="7e0ae-184">嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-184">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a><span data-ttu-id="7e0ae-185">使用者. Identity 。檔案名</span><span class="sxs-lookup"><span data-stu-id="7e0ae-185">User.Identity.Name</span></span>

<span data-ttu-id="7e0ae-186">根據預設， `User.Identity.Name` 不會填入。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-186">By default, the `User.Identity.Name` isn't populated.</span></span>

<span data-ttu-id="7e0ae-187">若要將應用程式設定為從宣告類型接收值 `name` ，請在中設定的[TokenValidationParameters。 NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-187">To configure the app to receive the value from the `name` claim type, configure the [TokenValidationParameters.NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) of the <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> in `Startup.ConfigureServices`:</span></span>

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADB2CDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a><span data-ttu-id="7e0ae-188">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="7e0ae-188">App settings</span></span>

<span data-ttu-id="7e0ae-189">*Appsettings*包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-189">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

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

<span data-ttu-id="7e0ae-190">範例：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-190">Example:</span></span>

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

### <a name="weatherforecast-controller"></a><span data-ttu-id="7e0ae-191">WeatherForecast 控制器</span><span class="sxs-lookup"><span data-stu-id="7e0ae-191">WeatherForecast controller</span></span>

<span data-ttu-id="7e0ae-192">WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至控制器。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-192">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute applied to the controller.</span></span> <span data-ttu-id="7e0ae-193">請**務必**瞭解：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-193">It's **important** to understand that:</span></span>

* <span data-ttu-id="7e0ae-194">[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)此 api 控制器中的屬性是保護此 api 免于未經授權存取的唯一做法。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-194">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="7e0ae-195">[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)WebAssembly 應用程式中所使用的屬性 Blazor 只會作為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-195">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="7e0ae-196">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="7e0ae-196">Client app configuration</span></span>

<span data-ttu-id="7e0ae-197">*本節適用于解決方案的**用戶端**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="7e0ae-197">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="7e0ae-198">驗證套件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-198">Authentication package</span></span>

<span data-ttu-id="7e0ae-199">當您建立應用程式以使用個別 B2C 帳戶（ `IndividualB2C` ）時，應用程式會自動接收[microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（[WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-199">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) ([Microsoft.Authentication.WebAssembly.Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)).</span></span> <span data-ttu-id="7e0ae-200">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-200">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="7e0ae-201">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-201">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

<span data-ttu-id="7e0ae-202">[WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)套件可轉移會將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)套件新增至應用程式中的。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-202">The [Microsoft.Authentication.WebAssembly.Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) package transitively adds the [Microsoft.AspNetCore.Components.WebAssembly.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="7e0ae-203">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="7e0ae-203">Authentication service support</span></span>

<span data-ttu-id="7e0ae-204"><xref:System.Net.Http.HttpClient>加入實例的支援，其中包含對伺服器專案提出要求時的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-204">Support for <xref:System.Net.Http.HttpClient> instances is added that include access tokens when making requests to the server project.</span></span>

<span data-ttu-id="7e0ae-205">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-205">*Program.cs*:</span></span>

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

<span data-ttu-id="7e0ae-206">驗證使用者的支援是在服務容器中註冊，並使用 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> [WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)套件所提供的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-206">Support for authenticating users is registered in the service container with the <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> extension method provided by the [Microsoft.Authentication.WebAssembly.Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) package.</span></span> <span data-ttu-id="7e0ae-207">這個方法會設定應用程式與 Identity 提供者（IP）互動所需的服務。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-207">This method sets up the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="7e0ae-208">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-208">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<span data-ttu-id="7e0ae-209"><xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-209">The <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="7e0ae-210">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-210">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="7e0ae-211">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-211">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{TENANT DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": false
  }
}
```

<span data-ttu-id="7e0ae-212">範例：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-212">Example:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": false
  }
}
```

### <a name="access-token-scopes"></a><span data-ttu-id="7e0ae-213">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="7e0ae-213">Access token scopes</span></span>

<span data-ttu-id="7e0ae-214">預設存取權杖範圍代表存取權杖範圍的清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-214">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="7e0ae-215">預設包含在登入要求中。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-215">Included by default in the sign in request.</span></span>
* <span data-ttu-id="7e0ae-216">用來在驗證之後立即布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-216">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="7e0ae-217">所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-217">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="7e0ae-218">您可以視需要為其他 API 應用程式新增額外的範圍：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-218">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

<span data-ttu-id="7e0ae-219">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-219">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="7e0ae-220">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="7e0ae-220">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="7e0ae-221">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="7e0ae-221">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)


### <a name="imports-file"></a><span data-ttu-id="7e0ae-222">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="7e0ae-222">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="7e0ae-223">索引頁面</span><span class="sxs-lookup"><span data-stu-id="7e0ae-223">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="7e0ae-224">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-224">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="7e0ae-225">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-225">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="7e0ae-226">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-226">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="7e0ae-227">驗證元件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-227">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="7e0ae-228">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-228">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="7e0ae-229">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7e0ae-229">Run the app</span></span>

<span data-ttu-id="7e0ae-230">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-230">Run the app from the Server project.</span></span> <span data-ttu-id="7e0ae-231">使用 Visual Studio 時，您可以：</span><span class="sxs-lookup"><span data-stu-id="7e0ae-231">When using Visual Studio, either:</span></span>

* <span data-ttu-id="7e0ae-232">將工具列中的 [**啟始專案**] 下拉式清單設定為*伺服器 API 應用程式*，然後選取 [**執行**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-232">Set the **Startup Projects** drop down list in the toolbar to the *Server API app* and select the **Run** button.</span></span>
* <span data-ttu-id="7e0ae-233">在**方案總管**中選取伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e0ae-233">Select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="7e0ae-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="7e0ae-234">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* [<span data-ttu-id="7e0ae-235">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="7e0ae-235">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="7e0ae-236">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="7e0ae-236">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="7e0ae-237">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="7e0ae-237">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
