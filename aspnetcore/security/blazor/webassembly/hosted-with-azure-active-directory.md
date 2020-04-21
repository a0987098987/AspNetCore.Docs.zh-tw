---
title: 使用 AzureBlazor活動目錄保護ASP.NET核心 Web 元件託管應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: a80be8d145b7c58be35e2c353a448db7e234e20b
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661845"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="2704a-102">使用 AzureBlazor活動目錄保護ASP.NET核心 Web 元件託管應用</span><span class="sxs-lookup"><span data-stu-id="2704a-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="2704a-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2704a-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="2704a-104">本文介紹如何建立使用[Azure 活動目錄 (AAD)](https://azure.microsoft.com/services/active-directory/)進行身份認證的[BlazorWebAssembly 託管應用](xref:blazor/hosting-models#blazor-webassembly)。</span><span class="sxs-lookup"><span data-stu-id="2704a-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="2704a-105">在 AAD B2C 中註冊應用並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="2704a-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="2704a-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="2704a-106">Create a tenant</span></span>

<span data-ttu-id="2704a-107">按照[「快速入門」中的指南:設置租戶](/azure/active-directory/develop/quickstart-create-new-tenant)以在 AAD 中創建租戶。</span><span class="sxs-lookup"><span data-stu-id="2704a-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="2704a-108">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="2704a-108">Register a server API app</span></span>

<span data-ttu-id="2704a-109">按照['快速入門'中的指南操作:將應用程式註冊到 Microsoft 識別平台](/azure/active-directory/develop/quickstart-register-app)和後續 Azure AAD 主題,以在 Azure 門戶的**Azure 活動目錄** > **應用註冊**區域註冊 伺服器*API 應用*的 AAD 應用:</span><span class="sxs-lookup"><span data-stu-id="2704a-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2704a-110">選取 [新增註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2704a-110">Select **New registration**.</span></span>
1. <span data-ttu-id="2704a-111">為應用提供**名稱**(例如,**Blazor伺服器 AAD**)。</span><span class="sxs-lookup"><span data-stu-id="2704a-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="2704a-112">選擇**支援的帳號型態 。**</span><span class="sxs-lookup"><span data-stu-id="2704a-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="2704a-113">對於此體驗,您只能選擇**此組織目錄中的帳戶**(單個租戶)。</span><span class="sxs-lookup"><span data-stu-id="2704a-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="2704a-114">在這種情況下,*伺服器 API 應用*不需要重定向**URI,** 因此請將下拉清單設定為**Web,** 並且不輸入重定向 URI。</span><span class="sxs-lookup"><span data-stu-id="2704a-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="2704a-115">禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。</span><span class="sxs-lookup"><span data-stu-id="2704a-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="2704a-116">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2704a-116">Select **Register**.</span></span>

<span data-ttu-id="2704a-117">在**API 許可權**中,刪除 Microsoft**圖形** > **使用者.讀取**許可權,因為應用不需要登錄或存取設定檔。</span><span class="sxs-lookup"><span data-stu-id="2704a-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="2704a-118">在**公開 API 中**:</span><span class="sxs-lookup"><span data-stu-id="2704a-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="2704a-119">選擇 **「添加範圍**」。</span><span class="sxs-lookup"><span data-stu-id="2704a-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="2704a-120">選取 [儲存並繼續]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="2704a-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="2704a-121">提供**範圍名稱**(例如`API.Access`,</span><span class="sxs-lookup"><span data-stu-id="2704a-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="2704a-122">提供**管理員同意顯示名稱**(例如`Access API`,</span><span class="sxs-lookup"><span data-stu-id="2704a-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="2704a-123">提供**管理員同意說明**(例如`Allows the app to access server app API endpoints.`,</span><span class="sxs-lookup"><span data-stu-id="2704a-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="2704a-124">確認**狀態**設定為**啟用**。</span><span class="sxs-lookup"><span data-stu-id="2704a-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="2704a-125">選擇 **「添加範圍**」。</span><span class="sxs-lookup"><span data-stu-id="2704a-125">Select **Add scope**.</span></span>

<span data-ttu-id="2704a-126">記錄以下資訊:</span><span class="sxs-lookup"><span data-stu-id="2704a-126">Record the following information:</span></span>

* <span data-ttu-id="2704a-127">*伺服器 API 應用程式*應用程式代碼(用戶端代碼)(例如`11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="2704a-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="2704a-128">套用 ID`https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`URI(例如,`api://11111111-1111-1111-1111-111111111111`或您提供的自訂值 )</span><span class="sxs-lookup"><span data-stu-id="2704a-128">App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, `api://11111111-1111-1111-1111-111111111111`, or the custom value that you provided)</span></span>
* <span data-ttu-id="2704a-129">目錄 ID(租戶代碼)(`222222222-2222-2222-2222-222222222222`例如 )</span><span class="sxs-lookup"><span data-stu-id="2704a-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="2704a-130">AAD 租戶域(例如`contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="2704a-130">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="2704a-131">默認作用域(例如), `API.Access`</span><span class="sxs-lookup"><span data-stu-id="2704a-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="2704a-132">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="2704a-132">Register a client app</span></span>

<span data-ttu-id="2704a-133">按照[「快速入門」中的指南操作:將應用程式註冊到 Microsoft 標識平台](/azure/active-directory/develop/quickstart-register-app)和後續 Azure AAD 主題,以在 Azure 門戶的**Azure 活動目錄** > **應用註冊**區域註冊*客戶端應用*的 AAD 應用:</span><span class="sxs-lookup"><span data-stu-id="2704a-133">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2704a-134">選取 [新增註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2704a-134">Select **New registration**.</span></span>
1. <span data-ttu-id="2704a-135">為應用提供**名稱**(例如,**Blazor用戶端 AAD**)。</span><span class="sxs-lookup"><span data-stu-id="2704a-135">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="2704a-136">選擇**支援的帳號型態 。**</span><span class="sxs-lookup"><span data-stu-id="2704a-136">Choose a **Supported account types**.</span></span> <span data-ttu-id="2704a-137">對於此體驗,您只能選擇**此組織目錄中的帳戶**(單個租戶)。</span><span class="sxs-lookup"><span data-stu-id="2704a-137">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="2704a-138">將**重定向 URI**下拉清單設定為**Web,** 並提供`https://localhost:5001/authentication/login-callback`重定向 URI。</span><span class="sxs-lookup"><span data-stu-id="2704a-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="2704a-139">禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。</span><span class="sxs-lookup"><span data-stu-id="2704a-139">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="2704a-140">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="2704a-140">Select **Register**.</span></span>

<span data-ttu-id="2704a-141">在**認證** > **平台設定中** > **, Web**:</span><span class="sxs-lookup"><span data-stu-id="2704a-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="2704a-142">確認存在 重定`https://localhost:5001/authentication/login-callback`向**URI。**</span><span class="sxs-lookup"><span data-stu-id="2704a-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="2704a-143">對於**隱式授予**,選擇 Access**權杖**和**ID 權杖的**複選框。</span><span class="sxs-lookup"><span data-stu-id="2704a-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="2704a-144">此體驗可以接受應用的剩餘預設值。</span><span class="sxs-lookup"><span data-stu-id="2704a-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="2704a-145">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2704a-145">Select the **Save** button.</span></span>

<span data-ttu-id="2704a-146">在**API 權限**中:</span><span class="sxs-lookup"><span data-stu-id="2704a-146">In **API permissions**:</span></span>

1. <span data-ttu-id="2704a-147">確認應用具有**Microsoft 圖形** > **使用者.讀取**許可權。</span><span class="sxs-lookup"><span data-stu-id="2704a-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="2704a-148">選擇 **「新增」** 後跟**我的 API 的權限**。</span><span class="sxs-lookup"><span data-stu-id="2704a-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="2704a-149">從 **「名稱」** 列中選擇*伺服器 API 應用*(例如,**Blazor伺服器 AAD**)。</span><span class="sxs-lookup"><span data-stu-id="2704a-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="2704a-150">開啟**API**清單。</span><span class="sxs-lookup"><span data-stu-id="2704a-150">Open the **API** list.</span></span>
1. <span data-ttu-id="2704a-151">啟用對 API 的訪問(`API.Access`例如,</span><span class="sxs-lookup"><span data-stu-id="2704a-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="2704a-152">選取 [新增權限]  。</span><span class="sxs-lookup"><span data-stu-id="2704a-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="2704a-153">選擇 **[TENANT 名稱] 按鈕的管理員內容**。</span><span class="sxs-lookup"><span data-stu-id="2704a-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="2704a-154">選取 [是] \*\*\*\* 加以確認。</span><span class="sxs-lookup"><span data-stu-id="2704a-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="2704a-155">記錄*客戶端應用*應用程式ID(用戶端ID)(`33333333-3333-3333-3333-333333333333`例如。</span><span class="sxs-lookup"><span data-stu-id="2704a-155">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="2704a-156">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2704a-156">Create the app</span></span>

<span data-ttu-id="2704a-157">將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:</span><span class="sxs-lookup"><span data-stu-id="2704a-157">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="2704a-158">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="2704a-158">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="2704a-159">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="2704a-159">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="2704a-160">將應用 ID`app-id-uri`URI 傳遞給該選項,但請注意客戶端應用中可能需要更改配置,這在[Access 權杖作用域](#access-token-scopes)部分中對此進行了說明。</span><span class="sxs-lookup"><span data-stu-id="2704a-160">Pass the App ID URI to the `app-id-uri` option, but note a configuration change might be required in the client app, which is described in the [Access token scopes](#access-token-scopes) section.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="2704a-161">伺服器應用設定</span><span class="sxs-lookup"><span data-stu-id="2704a-161">Server app configuration</span></span>

<span data-ttu-id="2704a-162">*本節涉及解決方案的 **「伺服器」** 應用。*</span><span class="sxs-lookup"><span data-stu-id="2704a-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="2704a-163">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="2704a-163">Authentication package</span></span>

<span data-ttu-id="2704a-164">認證與授權呼叫ASP.NET核心 Web API 的支援`Microsoft.AspNetCore.Authentication.AzureAD.UI`由:</span><span class="sxs-lookup"><span data-stu-id="2704a-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="2704a-165">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="2704a-165">Authentication service support</span></span>

<span data-ttu-id="2704a-166">該方法`AddAuthentication`在應用中設置身份驗證服務,並將 JWT 承載處理程式配置為預設身份驗證方法。</span><span class="sxs-lookup"><span data-stu-id="2704a-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="2704a-167">該方法`AddAzureADBearer`在 JWT 承載處理程式中設定驗證 Azure 活動目錄發出的權杖所需的特定參數:</span><span class="sxs-lookup"><span data-stu-id="2704a-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="2704a-168">`UseAuthentication`並確保`UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="2704a-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="2704a-169">應用嘗試對傳入請求解析和驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="2704a-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="2704a-170">嘗試在沒有適當認證的權利訪問受保護資源的任何請求都失敗。</span><span class="sxs-lookup"><span data-stu-id="2704a-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a><span data-ttu-id="2704a-171">User.Identity.Name</span><span class="sxs-lookup"><span data-stu-id="2704a-171">User.Identity.Name</span></span>

<span data-ttu-id="2704a-172">預設情況下,伺服器應用`User.Identity.Name`API`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`使用 宣告類型中的值填充`2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com`(例如,</span><span class="sxs-lookup"><span data-stu-id="2704a-172">By default, the Server app API populates `User.Identity.Name` with the value from the `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` claim type (for example, `2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com`).</span></span>

<span data-ttu-id="2704a-173">要將應用配置為從`name`聲明類型接收值,請<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions>配置`Startup.ConfigureServices`中的[令牌驗證參數。](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType)</span><span class="sxs-lookup"><span data-stu-id="2704a-173">To configure the app to receive the value from the `name` claim type, configure the [TokenValidationParameters.NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) of the <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a><span data-ttu-id="2704a-174">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="2704a-174">App settings</span></span>

<span data-ttu-id="2704a-175">*appsettings.json*檔包含用於配置用於驗證存取權杖的JWT承載處理程式的選項。</span><span class="sxs-lookup"><span data-stu-id="2704a-175">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="2704a-176">天氣預報控制器</span><span class="sxs-lookup"><span data-stu-id="2704a-176">WeatherForecast controller</span></span>

<span data-ttu-id="2704a-177">天氣預報控制器 (*控制器/天氣預報控制器.cs)* 公開受保護的`[Authorize]`API, 該屬性應用於控制器。</span><span class="sxs-lookup"><span data-stu-id="2704a-177">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="2704a-178">**請務必**瞭解:</span><span class="sxs-lookup"><span data-stu-id="2704a-178">It's **important** to understand that:</span></span>

* <span data-ttu-id="2704a-179">此`[Authorize]`API 控制器中的屬性是保護此 API 免受未經授權的訪問的唯一原因。</span><span class="sxs-lookup"><span data-stu-id="2704a-179">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="2704a-180">Blazor WebAssembly 應用中`[Authorize]`使用的 屬性僅用作應用的提示,即應授權使用者使應用正常工作。</span><span class="sxs-lookup"><span data-stu-id="2704a-180">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="2704a-181">用戶端應用設定</span><span class="sxs-lookup"><span data-stu-id="2704a-181">Client app configuration</span></span>

<span data-ttu-id="2704a-182">*本節涉及解決方案的**用戶端**應用。*</span><span class="sxs-lookup"><span data-stu-id="2704a-182">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="2704a-183">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="2704a-183">Authentication package</span></span>

<span data-ttu-id="2704a-184">建立應用程式以使用工作帳號或學校帳戶 ()`SingleOrg`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。</span><span class="sxs-lookup"><span data-stu-id="2704a-184">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="2704a-185">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="2704a-185">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="2704a-186">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="2704a-186">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="2704a-187">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="2704a-187">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="2704a-188">包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。</span><span class="sxs-lookup"><span data-stu-id="2704a-188">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="2704a-189">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="2704a-189">Authentication service support</span></span>

<span data-ttu-id="2704a-190">使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="2704a-190">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="2704a-191">此方法設置應用與標識提供者 (IP) 互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="2704a-191">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="2704a-192">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2704a-192">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<span data-ttu-id="2704a-193">該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。</span><span class="sxs-lookup"><span data-stu-id="2704a-193">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="2704a-194">註冊應用時,可以從 Azure 門戶 AAD 配置中獲取配置應用所需的值。</span><span class="sxs-lookup"><span data-stu-id="2704a-194">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

### <a name="access-token-scopes"></a><span data-ttu-id="2704a-195">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="2704a-195">Access token scopes</span></span>

<span data-ttu-id="2704a-196">預設存取權杖範圍表示存取權杖的清單,這些作用網域是:</span><span class="sxs-lookup"><span data-stu-id="2704a-196">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="2704a-197">默認情況下包含在登錄請求中。</span><span class="sxs-lookup"><span data-stu-id="2704a-197">Included by default in the sign in request.</span></span>
* <span data-ttu-id="2704a-198">用於在身份驗證后立即預配訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="2704a-198">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="2704a-199">根據 Azure 活動目錄規則,所有作用域必須屬於同一應用。</span><span class="sxs-lookup"><span data-stu-id="2704a-199">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="2704a-200">根據需要為其他 API 應用添加其他作用域:</span><span class="sxs-lookup"><span data-stu-id="2704a-200">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="2704a-201">如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。</span><span class="sxs-lookup"><span data-stu-id="2704a-201">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="2704a-202">例如,Azure 門戶可以提供以下作用域 URI 格式之一:</span><span class="sxs-lookup"><span data-stu-id="2704a-202">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="2704a-203">提供無方案和主機的範圍 URI:</span><span class="sxs-lookup"><span data-stu-id="2704a-203">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="2704a-204">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="2704a-204">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

### <a name="imports-file"></a><span data-ttu-id="2704a-205">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="2704a-205">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="2704a-206">索引頁面</span><span class="sxs-lookup"><span data-stu-id="2704a-206">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="2704a-207">套用元件</span><span class="sxs-lookup"><span data-stu-id="2704a-207">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="2704a-208">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="2704a-208">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="2704a-209">登入元件</span><span class="sxs-lookup"><span data-stu-id="2704a-209">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="2704a-210">驗證元件</span><span class="sxs-lookup"><span data-stu-id="2704a-210">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="2704a-211">擷取資料元件</span><span class="sxs-lookup"><span data-stu-id="2704a-211">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="2704a-212">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="2704a-212">Run the app</span></span>

<span data-ttu-id="2704a-213">從「伺服器」專案運行應用。</span><span class="sxs-lookup"><span data-stu-id="2704a-213">Run the app from the Server project.</span></span> <span data-ttu-id="2704a-214">使用 Visual Studio 時,在**解決方案資源管理器**中選擇「伺服器」專案,然後選擇工具列中的 **「運行」** 按鈕,或從 **「調試」** 選單啟動應用。</span><span class="sxs-lookup"><span data-stu-id="2704a-214">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="2704a-215">其他資源</span><span class="sxs-lookup"><span data-stu-id="2704a-215">Additional resources</span></span>

* [<span data-ttu-id="2704a-216">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="2704a-216">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="2704a-217">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="2704a-217">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
