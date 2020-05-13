---
title: Blazor使用 Azure Active Directory 保護 ASP.NET Core WebAssembly 託管應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 6ff95f0c5c925cbafef2b997a6cb23aeb15ff1aa
ms.sourcegitcommit: 1250c90c8d87c2513532be5683640b65bfdf9ddb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83153968"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="2f93b-102">Blazor使用 Azure Active Directory 保護 ASP.NET Core WebAssembly 託管應用程式</span><span class="sxs-lookup"><span data-stu-id="2f93b-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="2f93b-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2f93b-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="2f93b-104">本文說明如何建立[ Blazor WebAssembly 託管應用程式](xref:blazor/hosting-models#blazor-webassembly)，以使用[Azure Active Directory （AAD）](https://azure.microsoft.com/services/active-directory/)進行驗證。</span><span class="sxs-lookup"><span data-stu-id="2f93b-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-and-create-solution"></a><span data-ttu-id="2f93b-105">在 AAD 中註冊應用程式並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="2f93b-105">Register apps in AAD and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="2f93b-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="2f93b-106">Create a tenant</span></span>

<span data-ttu-id="2f93b-107">遵循[快速入門：設定租](/azure/active-directory/develop/quickstart-create-new-tenant)使用者中的指導方針，在 AAD 中建立租使用者。</span><span class="sxs-lookup"><span data-stu-id="2f93b-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="2f93b-108">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="2f93b-108">Register a server API app</span></span>

<span data-ttu-id="2f93b-109">請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，在 Azure 入口網站的**Azure Active Directory**應用程式註冊區域中，為*伺服器 API 應用*程式註冊 AAD 應用程式  >  **App registrations** ：</span><span class="sxs-lookup"><span data-stu-id="2f93b-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2f93b-110">選取 [新增註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2f93b-110">Select **New registration**.</span></span>
1. <span data-ttu-id="2f93b-111">提供應用程式的**名稱**（例如， \*\* Blazor 伺服器 AAD\*\*）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="2f93b-112">選擇**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="2f93b-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="2f93b-113">在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="2f93b-114">在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="2f93b-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="2f93b-115">停用**Permissions**[授與系統  >  **管理員收到給 openid 和 offline_access 許可權**] 核取方塊的許可權。</span><span class="sxs-lookup"><span data-stu-id="2f93b-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="2f93b-116">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2f93b-116">Select **Register**.</span></span>

<span data-ttu-id="2f93b-117">在 [ **API 許可權**] 中，移除 [ **Microsoft Graph**  >  **使用者]。 [讀取**] 許可權，因為應用程式不需要登入或 uer 設定檔存取權。</span><span class="sxs-lookup"><span data-stu-id="2f93b-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="2f93b-118">在中**公開 API**：</span><span class="sxs-lookup"><span data-stu-id="2f93b-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="2f93b-119">選取 [**新增範圍**]。</span><span class="sxs-lookup"><span data-stu-id="2f93b-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="2f93b-120">選取 [儲存並繼續]  。</span><span class="sxs-lookup"><span data-stu-id="2f93b-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="2f93b-121">提供**範圍名稱**（例如， `API.Access` ）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="2f93b-122">提供系統**管理員同意顯示名稱**（例如 `Access API` ）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="2f93b-123">提供系統**管理員同意描述**（例如 `Allows the app to access server app API endpoints.` ）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="2f93b-124">確認 [**狀態**] 設定為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="2f93b-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="2f93b-125">選取 [**新增領域**]。</span><span class="sxs-lookup"><span data-stu-id="2f93b-125">Select **Add scope**.</span></span>

<span data-ttu-id="2f93b-126">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="2f93b-126">Record the following information:</span></span>

* <span data-ttu-id="2f93b-127">*伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如， `11111111-1111-1111-1111-111111111111` ）</span><span class="sxs-lookup"><span data-stu-id="2f93b-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="2f93b-128">應用程式識別碼 URI （例如， `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111` 、 `api://11111111-1111-1111-1111-111111111111` 或您提供的自訂值）</span><span class="sxs-lookup"><span data-stu-id="2f93b-128">App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, `api://11111111-1111-1111-1111-111111111111`, or the custom value that you provided)</span></span>
* <span data-ttu-id="2f93b-129">目錄識別碼（租使用者識別碼）（例如， `222222222-2222-2222-2222-222222222222` ）</span><span class="sxs-lookup"><span data-stu-id="2f93b-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="2f93b-130">AAD 租使用者網域（例如， `contoso.onmicrosoft.com` ）</span><span class="sxs-lookup"><span data-stu-id="2f93b-130">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="2f93b-131">預設範圍（例如， `API.Access` ）</span><span class="sxs-lookup"><span data-stu-id="2f93b-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="2f93b-132">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="2f93b-132">Register a client app</span></span>

<span data-ttu-id="2f93b-133">請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，在 Azure 入口網站的**Azure Active Directory**應用程式註冊區域中，為*用戶端應用程式*註冊 AAD 應用程式  >  **App registrations** ：</span><span class="sxs-lookup"><span data-stu-id="2f93b-133">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2f93b-134">選取 [新增註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2f93b-134">Select **New registration**.</span></span>
1. <span data-ttu-id="2f93b-135">提供應用程式的**名稱**（例如\*\* Blazor 用戶端 AAD\*\*）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-135">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="2f93b-136">選擇**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="2f93b-136">Choose a **Supported account types**.</span></span> <span data-ttu-id="2f93b-137">在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-137">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="2f93b-138">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供的 [重新導向 uri] `https://localhost:5001/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="2f93b-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="2f93b-139">停用**Permissions**[授與系統  >  **管理員收到給 openid 和 offline_access 許可權**] 核取方塊的許可權。</span><span class="sxs-lookup"><span data-stu-id="2f93b-139">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="2f93b-140">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2f93b-140">Select **Register**.</span></span>

<span data-ttu-id="2f93b-141">在 [**驗證**  >  **平臺**設定]  >  **Web**：</span><span class="sxs-lookup"><span data-stu-id="2f93b-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="2f93b-142">確認的重新**導向 URI** `https://localhost:5001/authentication/login-callback` 存在。</span><span class="sxs-lookup"><span data-stu-id="2f93b-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="2f93b-143">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2f93b-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="2f93b-144">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="2f93b-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="2f93b-145">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2f93b-145">Select the **Save** button.</span></span>

<span data-ttu-id="2f93b-146">在 [ **API 許可權**] 中：</span><span class="sxs-lookup"><span data-stu-id="2f93b-146">In **API permissions**:</span></span>

1. <span data-ttu-id="2f93b-147">確認應用程式已**Microsoft Graph**  >  **使用者。讀取**許可權。</span><span class="sxs-lookup"><span data-stu-id="2f93b-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="2f93b-148">選取 [**新增許可權**]，後面接著 [**我的 api**]。</span><span class="sxs-lookup"><span data-stu-id="2f93b-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="2f93b-149">從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如，[ \*\* Blazor 伺服器 AAD\*\*]）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="2f93b-150">開啟 [ **API**清單]。</span><span class="sxs-lookup"><span data-stu-id="2f93b-150">Open the **API** list.</span></span>
1. <span data-ttu-id="2f93b-151">啟用 API 的存取權（例如， `API.Access` ）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="2f93b-152">選取 [新增權限]  。</span><span class="sxs-lookup"><span data-stu-id="2f93b-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="2f93b-153">選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2f93b-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="2f93b-154">選取 [是]  確認。</span><span class="sxs-lookup"><span data-stu-id="2f93b-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="2f93b-155">記錄*用戶端應用*程式識別碼（用戶端識別碼）（例如 `33333333-3333-3333-3333-333333333333` ）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-155">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="2f93b-156">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2f93b-156">Create the app</span></span>

<span data-ttu-id="2f93b-157">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="2f93b-157">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="2f93b-158">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。</span><span class="sxs-lookup"><span data-stu-id="2f93b-158">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="2f93b-159">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="2f93b-159">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="2f93b-160">將應用程式識別碼 URI 傳遞給 `app-id-uri` 選項，但請注意，在用戶端應用程式中可能需要進行設定變更，如[存取權杖範圍](#access-token-scopes)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="2f93b-160">Pass the App ID URI to the `app-id-uri` option, but note a configuration change might be required in the client app, which is described in the [Access token scopes](#access-token-scopes) section.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="2f93b-161">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2f93b-161">Server app configuration</span></span>

<span data-ttu-id="2f93b-162">*本節適用于解決方案的**伺服器**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="2f93b-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="2f93b-163">驗證套件</span><span class="sxs-lookup"><span data-stu-id="2f93b-163">Authentication package</span></span>

<span data-ttu-id="2f93b-164">驗證和授權呼叫 ASP.NET Core Web Api 的支援是由所提供 `Microsoft.AspNetCore.Authentication.AzureAD.UI` ：</span><span class="sxs-lookup"><span data-stu-id="2f93b-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="{VERSION}" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="2f93b-165">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="2f93b-165">Authentication service support</span></span>

<span data-ttu-id="2f93b-166">方法會在 `AddAuthentication` 應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="2f93b-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="2f93b-167">`AddAzureADBearer`方法會設定 JWT 持有人處理常式中所需的特定參數，以驗證 Azure Active Directory 所發出的權杖：</span><span class="sxs-lookup"><span data-stu-id="2f93b-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="2f93b-168">`UseAuthentication`並 `UseAuthorization` 確定：</span><span class="sxs-lookup"><span data-stu-id="2f93b-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="2f93b-169">應用程式會嘗試剖析並驗證傳入要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="2f93b-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="2f93b-170">嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。</span><span class="sxs-lookup"><span data-stu-id="2f93b-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a><span data-ttu-id="2f93b-171">使用者. Identity 。檔案名</span><span class="sxs-lookup"><span data-stu-id="2f93b-171">User.Identity.Name</span></span>

<span data-ttu-id="2f93b-172">根據預設，伺服器應用程式 API 會填入 `User.Identity.Name` 來自宣告類型的值（例如 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` `2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com` ）。</span><span class="sxs-lookup"><span data-stu-id="2f93b-172">By default, the Server app API populates `User.Identity.Name` with the value from the `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` claim type (for example, `2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com`).</span></span>

<span data-ttu-id="2f93b-173">若要將應用程式設定為從宣告類型接收值 `name` ，請在中設定的[TokenValidationParameters。 NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="2f93b-173">To configure the app to receive the value from the `name` claim type, configure the [TokenValidationParameters.NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) of the <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a><span data-ttu-id="2f93b-174">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2f93b-174">App settings</span></span>

<span data-ttu-id="2f93b-175">*Appsettings*包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f93b-175">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{SERVER API APP CLIENT ID}",
  }
}
```

<span data-ttu-id="2f93b-176">範例：</span><span class="sxs-lookup"><span data-stu-id="2f93b-176">Example:</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "contoso.onmicrosoft.com",
    "TenantId": "e86c78e2-8bb4-4c41-aefd-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="2f93b-177">WeatherForecast 控制器</span><span class="sxs-lookup"><span data-stu-id="2f93b-177">WeatherForecast controller</span></span>

<span data-ttu-id="2f93b-178">WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 `[Authorize]` 屬性套用至控制器。</span><span class="sxs-lookup"><span data-stu-id="2f93b-178">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="2f93b-179">請**務必**瞭解：</span><span class="sxs-lookup"><span data-stu-id="2f93b-179">It's **important** to understand that:</span></span>

* <span data-ttu-id="2f93b-180">`[Authorize]`此 api 控制器中的屬性是保護此 api 免于未經授權存取的唯一做法。</span><span class="sxs-lookup"><span data-stu-id="2f93b-180">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="2f93b-181">`[Authorize]`WebAssembly 應用程式中所使用的屬性 Blazor 只會作為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="2f93b-181">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="2f93b-182">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2f93b-182">Client app configuration</span></span>

<span data-ttu-id="2f93b-183">*本節適用于解決方案的**用戶端**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="2f93b-183">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="2f93b-184">驗證套件</span><span class="sxs-lookup"><span data-stu-id="2f93b-184">Authentication package</span></span>

<span data-ttu-id="2f93b-185">建立應用程式以使用工作或學校帳戶（）時 `SingleOrg` ，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（）的套件參考 `Microsoft.Authentication.WebAssembly.Msal` 。</span><span class="sxs-lookup"><span data-stu-id="2f93b-185">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="2f93b-186">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="2f93b-186">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="2f93b-187">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="2f93b-187">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="2f93b-188">`{VERSION}`將前述套件參考中的取代為發行 `Microsoft.AspNetCore.Blazor.Templates` 項中所顯示的套件版本 <xref:blazor/get-started> 。</span><span class="sxs-lookup"><span data-stu-id="2f93b-188">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="2f93b-189">`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="2f93b-189">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="2f93b-190">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="2f93b-190">Authentication service support</span></span>

<span data-ttu-id="2f93b-191">`HttpClient`加入實例的支援，其中包含對伺服器專案提出要求時的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2f93b-191">Support for `HttpClient` instances is added that include access tokens when making requests to the server project.</span></span>

<span data-ttu-id="2f93b-192">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="2f93b-192">*Program.cs*:</span></span>

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
        client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

<span data-ttu-id="2f93b-193">使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 `AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal` 。</span><span class="sxs-lookup"><span data-stu-id="2f93b-193">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="2f93b-194">這個方法會設定應用程式與 Identity 提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="2f93b-194">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="2f93b-195">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="2f93b-195">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<span data-ttu-id="2f93b-196">`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="2f93b-196">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="2f93b-197">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="2f93b-197">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="2f93b-198">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="2f93b-198">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/{TENANT ID}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": true
  }
}
```

<span data-ttu-id="2f93b-199">範例：</span><span class="sxs-lookup"><span data-stu-id="2f93b-199">Example:</span></span>

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/e86c78e2-...-918e0565a45e",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": true
  }
}
```

### <a name="access-token-scopes"></a><span data-ttu-id="2f93b-200">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="2f93b-200">Access token scopes</span></span>

<span data-ttu-id="2f93b-201">預設存取權杖範圍代表存取權杖範圍的清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2f93b-201">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="2f93b-202">預設包含在登入要求中。</span><span class="sxs-lookup"><span data-stu-id="2f93b-202">Included by default in the sign in request.</span></span>
* <span data-ttu-id="2f93b-203">用來在驗證之後立即布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2f93b-203">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="2f93b-204">所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。</span><span class="sxs-lookup"><span data-stu-id="2f93b-204">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="2f93b-205">您可以視需要為其他 API 應用程式新增額外的範圍：</span><span class="sxs-lookup"><span data-stu-id="2f93b-205">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="2f93b-206">如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="2f93b-206">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="2f93b-207">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="2f93b-207">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="2f93b-208">提供不含配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="2f93b-208">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="2f93b-209">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="2f93b-209">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="2f93b-210">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="2f93b-210">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="2f93b-211">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="2f93b-211">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)


### <a name="imports-file"></a><span data-ttu-id="2f93b-212">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="2f93b-212">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="2f93b-213">索引頁面</span><span class="sxs-lookup"><span data-stu-id="2f93b-213">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="2f93b-214">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="2f93b-214">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="2f93b-215">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="2f93b-215">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="2f93b-216">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="2f93b-216">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="2f93b-217">驗證元件</span><span class="sxs-lookup"><span data-stu-id="2f93b-217">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="2f93b-218">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="2f93b-218">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="2f93b-219">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="2f93b-219">Run the app</span></span>

<span data-ttu-id="2f93b-220">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2f93b-220">Run the app from the Server project.</span></span> <span data-ttu-id="2f93b-221">使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2f93b-221">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="2f93b-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f93b-222">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* [<span data-ttu-id="2f93b-223">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="2f93b-223">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/blazor/webassembly/aad-groups-roles>
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="2f93b-224">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="2f93b-224">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
