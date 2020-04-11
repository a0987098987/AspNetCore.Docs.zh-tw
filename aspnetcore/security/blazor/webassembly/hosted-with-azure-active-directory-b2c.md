---
title: 使用 AzureBlazor活動目錄 B2C 保護 ASP.NET核心 Web 元件託管應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: e2bb0c1bd807d590331b714e3b80d8c4ab434e2f
ms.sourcegitcommit: e8dc30453af8bbefcb61857987090d79230a461d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123461"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="8fd64-102">使用 AzureBlazor活動目錄 B2C 保護 ASP.NET核心 Web 元件託管應用</span><span class="sxs-lookup"><span data-stu-id="8fd64-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="8fd64-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8fd64-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="8fd64-104">本文介紹如何創建使用 Azure Blazor [活動目錄 (AAD) B2C](/azure/active-directory-b2c/overview)進行身份驗證的 WebAssembly 獨立應用。</span><span class="sxs-lookup"><span data-stu-id="8fd64-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="8fd64-105">在 AAD B2C 中註冊應用並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="8fd64-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="8fd64-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="8fd64-106">Create a tenant</span></span>

<span data-ttu-id="8fd64-107">按照教學中的指南[:建立 Azure 活動目錄 B2C 租戶](/azure/active-directory-b2c/tutorial-create-tenant)以建立 AAD B2C 租戶並記錄以下資訊:</span><span class="sxs-lookup"><span data-stu-id="8fd64-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="8fd64-108">AAD B2C 實體(`https://contoso.b2clogin.com/`例如 ,包括尾隨斜杠 )</span><span class="sxs-lookup"><span data-stu-id="8fd64-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="8fd64-109">AAD B2C 租戶域`contoso.onmicrosoft.com`(例如 ,</span><span class="sxs-lookup"><span data-stu-id="8fd64-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="8fd64-110">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="8fd64-110">Register a server API app</span></span>

<span data-ttu-id="8fd64-111">按照教學中的指南[:在 Azure 活動目錄 B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications),在 Azure 門戶的 Azure**活動目錄** > **應用註冊**區域註冊 伺服器*API 應用*的 AAD 應用:</span><span class="sxs-lookup"><span data-stu-id="8fd64-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="8fd64-112">選取 [新增註冊]  。</span><span class="sxs-lookup"><span data-stu-id="8fd64-112">Select **New registration**.</span></span>
1. <span data-ttu-id="8fd64-113">為應用提供**名稱**(例如**Blazor, 伺服器 AAD B2C**)。</span><span class="sxs-lookup"><span data-stu-id="8fd64-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="8fd64-114">對於**支援的帳戶類型**,選擇**任何組織目錄中的帳戶或任何標識提供程式。用於使用 Azure AD B2C 對使用者進行身份驗證。**</span><span class="sxs-lookup"><span data-stu-id="8fd64-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="8fd64-115">(多租戶)用於此體驗。</span><span class="sxs-lookup"><span data-stu-id="8fd64-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="8fd64-116">在這種情況下,*伺服器 API 應用*不需要重定向**URI,** 因此請將下拉清單設定為**Web,** 並且不輸入重定向 URI。</span><span class="sxs-lookup"><span data-stu-id="8fd64-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="8fd64-117">確認**啟用權限** > **的權限來設定管理員集中開啟 id 與offline_access權限**。</span><span class="sxs-lookup"><span data-stu-id="8fd64-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="8fd64-118">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="8fd64-118">Select **Register**.</span></span>

<span data-ttu-id="8fd64-119">在**公開 API 中**:</span><span class="sxs-lookup"><span data-stu-id="8fd64-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="8fd64-120">選擇 **「添加範圍**」。</span><span class="sxs-lookup"><span data-stu-id="8fd64-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="8fd64-121">選取 [儲存並繼續]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8fd64-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="8fd64-122">提供**範圍名稱**(例如`API.Access`,</span><span class="sxs-lookup"><span data-stu-id="8fd64-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="8fd64-123">提供**管理員同意顯示名稱**(例如`Access API`,</span><span class="sxs-lookup"><span data-stu-id="8fd64-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="8fd64-124">提供**管理員同意說明**(例如`Allows the app to access server app API endpoints.`,</span><span class="sxs-lookup"><span data-stu-id="8fd64-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="8fd64-125">確認**狀態**設定為**啟用**。</span><span class="sxs-lookup"><span data-stu-id="8fd64-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="8fd64-126">選擇 **「添加範圍**」。</span><span class="sxs-lookup"><span data-stu-id="8fd64-126">Select **Add scope**.</span></span>

<span data-ttu-id="8fd64-127">記錄以下資訊:</span><span class="sxs-lookup"><span data-stu-id="8fd64-127">Record the following information:</span></span>

* <span data-ttu-id="8fd64-128">*伺服器 API 應用程式*應用程式代碼(用戶端代碼)(例如`11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="8fd64-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="8fd64-129">套用 ID`https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`URI(例如,`api://11111111-1111-1111-1111-111111111111`或您提供的自訂值 )</span><span class="sxs-lookup"><span data-stu-id="8fd64-129">App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, `api://11111111-1111-1111-1111-111111111111`, or the custom value that you provided)</span></span>
* <span data-ttu-id="8fd64-130">目錄 ID(租戶代碼)(`222222222-2222-2222-2222-222222222222`例如 )</span><span class="sxs-lookup"><span data-stu-id="8fd64-130">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="8fd64-131">*伺服器 API 應用程式*套用 ID`https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`URI(例如 ,Azure 入口會將該值預設為客戶端代碼)</span><span class="sxs-lookup"><span data-stu-id="8fd64-131">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="8fd64-132">默認作用域(例如), `API.Access`</span><span class="sxs-lookup"><span data-stu-id="8fd64-132">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="8fd64-133">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="8fd64-133">Register a client app</span></span>

<span data-ttu-id="8fd64-134">按照教學中的指南[:再次在 Azure 活動目錄 B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications),在 Azure 門戶的**Azure 活動目錄** > **應用註冊**區域為*用戶端應用*註冊 AAD 應用:</span><span class="sxs-lookup"><span data-stu-id="8fd64-134">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="8fd64-135">選取 [新增註冊]  。</span><span class="sxs-lookup"><span data-stu-id="8fd64-135">Select **New registration**.</span></span>
1. <span data-ttu-id="8fd64-136">為應用提供**名稱**(例如**Blazor, 用戶端 AAD B2C**)。</span><span class="sxs-lookup"><span data-stu-id="8fd64-136">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="8fd64-137">對於**支援的帳戶類型**,選擇**任何組織目錄中的帳戶或任何標識提供程式。用於使用 Azure AD B2C 對使用者進行身份驗證。**</span><span class="sxs-lookup"><span data-stu-id="8fd64-137">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="8fd64-138">(多租戶)用於此體驗。</span><span class="sxs-lookup"><span data-stu-id="8fd64-138">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="8fd64-139">將**重定向 URI**下拉清單設定為**Web,** 並提供`https://localhost:5001/authentication/login-callback`重定向 URI。</span><span class="sxs-lookup"><span data-stu-id="8fd64-139">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="8fd64-140">確認**啟用權限** > **的權限來設定管理員集中開啟 id 與offline_access權限**。</span><span class="sxs-lookup"><span data-stu-id="8fd64-140">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="8fd64-141">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="8fd64-141">Select **Register**.</span></span>

<span data-ttu-id="8fd64-142">在**認證** > **平台設定中** > **, Web**:</span><span class="sxs-lookup"><span data-stu-id="8fd64-142">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="8fd64-143">確認存在 重定`https://localhost:5001/authentication/login-callback`向**URI。**</span><span class="sxs-lookup"><span data-stu-id="8fd64-143">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="8fd64-144">對於**隱式授予**,選擇 Access**權杖**和**ID 權杖的**複選框。</span><span class="sxs-lookup"><span data-stu-id="8fd64-144">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="8fd64-145">此體驗可以接受應用的剩餘預設值。</span><span class="sxs-lookup"><span data-stu-id="8fd64-145">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="8fd64-146">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8fd64-146">Select the **Save** button.</span></span>

<span data-ttu-id="8fd64-147">在**API 權限**中:</span><span class="sxs-lookup"><span data-stu-id="8fd64-147">In **API permissions**:</span></span>

1. <span data-ttu-id="8fd64-148">確認應用具有**Microsoft 圖形** > **使用者.讀取**許可權。</span><span class="sxs-lookup"><span data-stu-id="8fd64-148">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="8fd64-149">選擇 **「新增」** 後跟**我的 API 的權限**。</span><span class="sxs-lookup"><span data-stu-id="8fd64-149">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="8fd64-150">從**名稱**列中選擇*伺服器 API 應用*(例如,**Blazor伺服器 AAD B2C)。**</span><span class="sxs-lookup"><span data-stu-id="8fd64-150">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="8fd64-151">開啟**API**清單。</span><span class="sxs-lookup"><span data-stu-id="8fd64-151">Open the **API** list.</span></span>
1. <span data-ttu-id="8fd64-152">啟用對 API 的訪問(`API.Access`例如,</span><span class="sxs-lookup"><span data-stu-id="8fd64-152">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="8fd64-153">選取 [新增權限]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="8fd64-153">Select **Add permissions**.</span></span>
1. <span data-ttu-id="8fd64-154">選擇 **[TENANT 名稱] 按鈕的管理員內容**。</span><span class="sxs-lookup"><span data-stu-id="8fd64-154">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="8fd64-155">選取 [是] \*\*\*\* 加以確認。</span><span class="sxs-lookup"><span data-stu-id="8fd64-155">Select **Yes** to confirm.</span></span>

<span data-ttu-id="8fd64-156">在**家庭** > **Azure AD B2C** > **使用者串流中**:</span><span class="sxs-lookup"><span data-stu-id="8fd64-156">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="8fd64-157">建立註冊和登入使用者流程</span><span class="sxs-lookup"><span data-stu-id="8fd64-157">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="8fd64-158">至少,選擇**應用程式聲明** > **顯示名稱**使用者屬性以`context.User.Identity.Name``LoginDisplay`填充 元件中的 (*共用/ LoginDisplay.razor)。*</span><span class="sxs-lookup"><span data-stu-id="8fd64-158">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="8fd64-159">記錄以下資訊:</span><span class="sxs-lookup"><span data-stu-id="8fd64-159">Record the following information:</span></span>

* <span data-ttu-id="8fd64-160">記錄*客戶端應用*應用程式ID(用戶端ID)(`33333333-3333-3333-3333-333333333333`例如。</span><span class="sxs-lookup"><span data-stu-id="8fd64-160">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="8fd64-161">記錄為應用創建的註冊和登錄使用者流名稱(例如。 `B2C_1_signupsignin`</span><span class="sxs-lookup"><span data-stu-id="8fd64-161">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="8fd64-162">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="8fd64-162">Create the app</span></span>

<span data-ttu-id="8fd64-163">將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:</span><span class="sxs-lookup"><span data-stu-id="8fd64-163">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="8fd64-164">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="8fd64-164">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="8fd64-165">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="8fd64-165">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="8fd64-166">將應用 ID`app-id-uri`URI 傳遞給該選項,但請注意客戶端應用中可能需要更改配置,這在[Access 權杖作用域](#access-token-scopes)部分中對此進行了說明。</span><span class="sxs-lookup"><span data-stu-id="8fd64-166">Pass the App ID URI to the `app-id-uri` option, but note a configuration change might be required in the client app, which is described in the [Access token scopes](#access-token-scopes) section.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="8fd64-167">伺服器應用設定</span><span class="sxs-lookup"><span data-stu-id="8fd64-167">Server app configuration</span></span>

<span data-ttu-id="8fd64-168">*本節涉及解決方案的 **「伺服器」** 應用。*</span><span class="sxs-lookup"><span data-stu-id="8fd64-168">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="8fd64-169">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="8fd64-169">Authentication package</span></span>

<span data-ttu-id="8fd64-170">認證與授權呼叫ASP.NET核心 Web API 的支援`Microsoft.AspNetCore.Authentication.AzureADB2C.UI`由:</span><span class="sxs-lookup"><span data-stu-id="8fd64-170">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureADB2C.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureADB2C.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="8fd64-171">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="8fd64-171">Authentication service support</span></span>

<span data-ttu-id="8fd64-172">該方法`AddAuthentication`在應用中設置身份驗證服務,並將 JWT 承載處理程式配置為預設身份驗證方法。</span><span class="sxs-lookup"><span data-stu-id="8fd64-172">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="8fd64-173">該方法`AddAzureADB2CBearer`在 JWT 承載處理程式中設定驗證 Azure 活動目錄 B2C 發出的權杖所需的特定參數:</span><span class="sxs-lookup"><span data-stu-id="8fd64-173">The `AddAzureADB2CBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory B2C:</span></span>

```csharp
services.AddAuthentication(AzureADB2CDefaults.BearerAuthenticationScheme)
    .AddAzureADB2CBearer(options => Configuration.Bind("AzureAdB2C", options));
```

<span data-ttu-id="8fd64-174">`UseAuthentication`並確保`UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="8fd64-174">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="8fd64-175">應用嘗試對傳入請求解析和驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="8fd64-175">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="8fd64-176">嘗試在沒有適當認證的權利訪問受保護資源的任何請求都失敗。</span><span class="sxs-lookup"><span data-stu-id="8fd64-176">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a><span data-ttu-id="8fd64-177">User.Identity.Name</span><span class="sxs-lookup"><span data-stu-id="8fd64-177">User.Identity.Name</span></span>

<span data-ttu-id="8fd64-178">預設情況下,未填充`User.Identity.Name`。</span><span class="sxs-lookup"><span data-stu-id="8fd64-178">By default, the `User.Identity.Name` isn't populated.</span></span>

<span data-ttu-id="8fd64-179">要將應用配置為從`name`聲明類型接收值,請<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions>配置`Startup.ConfigureServices`中的[令牌驗證參數。](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType)</span><span class="sxs-lookup"><span data-stu-id="8fd64-179">To configure the app to receive the value from the `name` claim type, configure the [TokenValidationParameters.NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) of the <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<JwtBearerOptions>(
    AzureADB2CDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a><span data-ttu-id="8fd64-180">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="8fd64-180">App settings</span></span>

<span data-ttu-id="8fd64-181">*appsettings.json*檔包含用於配置用於驗證存取權杖的JWT承載處理程式的選項。</span><span class="sxs-lookup"><span data-stu-id="8fd64-181">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

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

### <a name="weatherforecast-controller"></a><span data-ttu-id="8fd64-182">天氣預報控制器</span><span class="sxs-lookup"><span data-stu-id="8fd64-182">WeatherForecast controller</span></span>

<span data-ttu-id="8fd64-183">天氣預報控制器 (*控制器/天氣預報控制器.cs)* 公開受保護的`[Authorize]`API, 該屬性應用於控制器。</span><span class="sxs-lookup"><span data-stu-id="8fd64-183">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="8fd64-184">**請務必**瞭解:</span><span class="sxs-lookup"><span data-stu-id="8fd64-184">It's **important** to understand that:</span></span>

* <span data-ttu-id="8fd64-185">此`[Authorize]`API 控制器中的屬性是保護此 API 免受未經授權的訪問的唯一原因。</span><span class="sxs-lookup"><span data-stu-id="8fd64-185">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="8fd64-186">Blazor WebAssembly 應用中`[Authorize]`使用的 屬性僅用作應用的提示,即應授權使用者使應用正常工作。</span><span class="sxs-lookup"><span data-stu-id="8fd64-186">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="8fd64-187">用戶端應用設定</span><span class="sxs-lookup"><span data-stu-id="8fd64-187">Client app configuration</span></span>

<span data-ttu-id="8fd64-188">*本節涉及解決方案的**用戶端**應用。*</span><span class="sxs-lookup"><span data-stu-id="8fd64-188">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="8fd64-189">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="8fd64-189">Authentication package</span></span>

<span data-ttu-id="8fd64-190">建立使用單個 B2C 帳號 ()`IndividualB2C`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。</span><span class="sxs-lookup"><span data-stu-id="8fd64-190">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="8fd64-191">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="8fd64-191">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="8fd64-192">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="8fd64-192">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="8fd64-193">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="8fd64-193">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="8fd64-194">包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。</span><span class="sxs-lookup"><span data-stu-id="8fd64-194">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="8fd64-195">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="8fd64-195">Authentication service support</span></span>

<span data-ttu-id="8fd64-196">使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="8fd64-196">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="8fd64-197">此方法設置應用與標識提供者 (IP) 互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="8fd64-197">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="8fd64-198">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8fd64-198">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<span data-ttu-id="8fd64-199">該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。</span><span class="sxs-lookup"><span data-stu-id="8fd64-199">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="8fd64-200">註冊應用時,可以從 Azure 門戶 AAD 配置中獲取配置應用所需的值。</span><span class="sxs-lookup"><span data-stu-id="8fd64-200">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

### <a name="access-token-scopes"></a><span data-ttu-id="8fd64-201">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="8fd64-201">Access token scopes</span></span>

<span data-ttu-id="8fd64-202">預設存取權杖範圍表示存取權杖的清單,這些作用網域是:</span><span class="sxs-lookup"><span data-stu-id="8fd64-202">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="8fd64-203">默認情況下包含在登錄請求中。</span><span class="sxs-lookup"><span data-stu-id="8fd64-203">Included by default in the sign in request.</span></span>
* <span data-ttu-id="8fd64-204">用於在身份驗證后立即預配訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="8fd64-204">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="8fd64-205">根據 Azure 活動目錄規則,所有作用域必須屬於同一應用。</span><span class="sxs-lookup"><span data-stu-id="8fd64-205">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="8fd64-206">根據需要為其他 API 應用添加其他作用域:</span><span class="sxs-lookup"><span data-stu-id="8fd64-206">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="8fd64-207">如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。</span><span class="sxs-lookup"><span data-stu-id="8fd64-207">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="8fd64-208">例如,Azure 門戶可以提供以下作用域 URI 格式之一:</span><span class="sxs-lookup"><span data-stu-id="8fd64-208">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="8fd64-209">提供無方案和主機的範圍 URI:</span><span class="sxs-lookup"><span data-stu-id="8fd64-209">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="8fd64-210">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="8fd64-210">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

### <a name="imports-file"></a><span data-ttu-id="8fd64-211">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="8fd64-211">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="8fd64-212">索引頁面</span><span class="sxs-lookup"><span data-stu-id="8fd64-212">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="8fd64-213">套用元件</span><span class="sxs-lookup"><span data-stu-id="8fd64-213">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="8fd64-214">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="8fd64-214">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="8fd64-215">登入元件</span><span class="sxs-lookup"><span data-stu-id="8fd64-215">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="8fd64-216">驗證元件</span><span class="sxs-lookup"><span data-stu-id="8fd64-216">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="8fd64-217">擷取資料元件</span><span class="sxs-lookup"><span data-stu-id="8fd64-217">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="8fd64-218">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="8fd64-218">Run the app</span></span>

<span data-ttu-id="8fd64-219">從「伺服器」專案運行應用。</span><span class="sxs-lookup"><span data-stu-id="8fd64-219">Run the app from the Server project.</span></span> <span data-ttu-id="8fd64-220">使用 Visual Studio 時,在**解決方案資源管理器**中選擇「伺服器」專案,然後選擇工具列中的 **「運行」** 按鈕,或從 **「調試」** 選單啟動應用。</span><span class="sxs-lookup"><span data-stu-id="8fd64-220">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="8fd64-221">其他資源</span><span class="sxs-lookup"><span data-stu-id="8fd64-221">Additional resources</span></span>

* [<span data-ttu-id="8fd64-222">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="8fd64-222">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="8fd64-223">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="8fd64-223">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="8fd64-224">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="8fd64-224">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
