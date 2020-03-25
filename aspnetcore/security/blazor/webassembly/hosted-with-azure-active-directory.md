---
title: 使用 Azure Active Directory 保護 Blazor WebAssembly 託管應用程式的 ASP.NET Core
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: fc16a7212254e73efd4cea8155975f293e5d9ebb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219281"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="b7c13-102">使用 Azure Active Directory 保護 Blazor WebAssembly 託管應用程式的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7c13-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="b7c13-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b7c13-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="b7c13-104">本文說明如何建立使用[Azure Active Directory （AAD）](https://azure.microsoft.com/services/active-directory/)進行驗證的[Blazor WebAssembly 託管應用程式](xref:blazor/hosting-models#blazor-webassembly)。</span><span class="sxs-lookup"><span data-stu-id="b7c13-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="b7c13-105">在 AAD B2C 中註冊應用程式並建立解決方案</span><span class="sxs-lookup"><span data-stu-id="b7c13-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="b7c13-106">建立租用戶</span><span class="sxs-lookup"><span data-stu-id="b7c13-106">Create a tenant</span></span>

<span data-ttu-id="b7c13-107">遵循[快速入門：設定租](/azure/active-directory/develop/quickstart-create-new-tenant)使用者中的指導方針，在 AAD 中建立租使用者。</span><span class="sxs-lookup"><span data-stu-id="b7c13-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="b7c13-108">註冊伺服器 API 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7c13-108">Register a server API app</span></span>

<span data-ttu-id="b7c13-109">請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中，為*伺服器 API 應用*程式註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="b7c13-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="b7c13-110">選取 [新增註冊]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-110">Select **New registration**.</span></span>
1. <span data-ttu-id="b7c13-111">提供應用程式的**名稱**（例如， **Blazor Server AAD**）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="b7c13-112">選擇**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="b7c13-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="b7c13-113">在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="b7c13-114">在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="b7c13-115">停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="b7c13-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="b7c13-116">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-116">Select **Register**.</span></span>

<span data-ttu-id="b7c13-117">在 [ **API 許可權**] 中，移除**Microsoft Graph** > **使用者。讀取**許可權，因為應用程式不需要登入或 uer 設定檔存取權。</span><span class="sxs-lookup"><span data-stu-id="b7c13-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="b7c13-118">在中**公開 API**：</span><span class="sxs-lookup"><span data-stu-id="b7c13-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="b7c13-119">選取 [新增範圍]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="b7c13-120">選取 [儲存並繼續]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="b7c13-121">提供**範圍名稱**（例如，`API.Access`）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="b7c13-122">提供系統**管理員同意顯示名稱**（例如，`Access API`）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="b7c13-123">提供系統**管理員同意描述**（例如，`Allows the app to access server app API endpoints.`）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="b7c13-124">確認 [**狀態**] 設定為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="b7c13-125">選取 [**新增領域**]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-125">Select **Add scope**.</span></span>

<span data-ttu-id="b7c13-126">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="b7c13-126">Record the following information:</span></span>

* <span data-ttu-id="b7c13-127">*伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）</span><span class="sxs-lookup"><span data-stu-id="b7c13-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="b7c13-128">目錄識別碼（租使用者識別碼）（例如 `222222222-2222-2222-2222-222222222222`）</span><span class="sxs-lookup"><span data-stu-id="b7c13-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="b7c13-129">AAD 租使用者網域（例如 `contoso.onmicrosoft.com`）</span><span class="sxs-lookup"><span data-stu-id="b7c13-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="b7c13-130">預設範圍（例如 `API.Access`）</span><span class="sxs-lookup"><span data-stu-id="b7c13-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="b7c13-131">註冊用戶端應用程式</span><span class="sxs-lookup"><span data-stu-id="b7c13-131">Register a client app</span></span>

<span data-ttu-id="b7c13-132">請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中註冊*用戶端應用*程式的 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="b7c13-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="b7c13-133">選取 [新增註冊]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-133">Select **New registration**.</span></span>
1. <span data-ttu-id="b7c13-134">提供應用程式的**名稱**（例如， **Blazor 用戶端 AAD**）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="b7c13-135">選擇**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="b7c13-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="b7c13-136">在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="b7c13-137">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供 `https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="b7c13-138">停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="b7c13-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="b7c13-139">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-139">Select **Register**.</span></span>

<span data-ttu-id="b7c13-140">在 [**驗證** > **平臺**設定 > **Web**]：</span><span class="sxs-lookup"><span data-stu-id="b7c13-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="b7c13-141">確認 `https://localhost:5001/authentication/login-callback` 的重新**導向 URI**存在。</span><span class="sxs-lookup"><span data-stu-id="b7c13-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="b7c13-142">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="b7c13-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="b7c13-143">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="b7c13-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="b7c13-144">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b7c13-144">Select the **Save** button.</span></span>

<span data-ttu-id="b7c13-145">在 [ **API 許可權**] 中：</span><span class="sxs-lookup"><span data-stu-id="b7c13-145">In **API permissions**:</span></span>

1. <span data-ttu-id="b7c13-146">確認應用程式已**Microsoft Graph** > **使用者。讀取**許可權。</span><span class="sxs-lookup"><span data-stu-id="b7c13-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="b7c13-147">選取 [**新增許可權**]，後面接著 [**我的 api**]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="b7c13-148">從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如， **Blazor Server AAD**）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="b7c13-149">開啟 [ **API**清單]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-149">Open the **API** list.</span></span>
1. <span data-ttu-id="b7c13-150">啟用 API 的存取權（例如，`API.Access`）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="b7c13-151">選取 [新增權限]。</span><span class="sxs-lookup"><span data-stu-id="b7c13-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="b7c13-152">選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b7c13-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="b7c13-153">選取 [是] 加以確認。</span><span class="sxs-lookup"><span data-stu-id="b7c13-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="b7c13-154">記錄*用戶端應用*程式識別碼（用戶端識別碼）（例如 `33333333-3333-3333-3333-333333333333`）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="b7c13-155">建立應用程式</span><span class="sxs-lookup"><span data-stu-id="b7c13-155">Create the app</span></span>

<span data-ttu-id="b7c13-156">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="b7c13-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="b7c13-157">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="b7c13-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="b7c13-158">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="b7c13-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="b7c13-159">如需預設存取權杖範圍的重要設定變更，請參閱[驗證服務支援](#Authentication service support)一節。</span><span class="sxs-lookup"><span data-stu-id="b7c13-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="b7c13-160">從範本建立*用戶端應用程式*之後，必須手動變更 Blazor WebAssembly 範本所提供的值。</span><span class="sxs-lookup"><span data-stu-id="b7c13-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="b7c13-161">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="b7c13-161">Server app configuration</span></span>

<span data-ttu-id="b7c13-162">*本節適用于解決方案的**伺服器**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="b7c13-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="b7c13-163">驗證套件</span><span class="sxs-lookup"><span data-stu-id="b7c13-163">Authentication package</span></span>

<span data-ttu-id="b7c13-164">`Microsoft.AspNetCore.Authentication.AzureAD.UI`提供驗證和授權呼叫 ASP.NET Core Web Api 的支援：</span><span class="sxs-lookup"><span data-stu-id="b7c13-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="b7c13-165">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="b7c13-165">Authentication service support</span></span>

<span data-ttu-id="b7c13-166">`AddAuthentication` 方法會在應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="b7c13-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="b7c13-167">`AddAzureADBearer` 方法會設定 JWT 持有人處理常式中的特定參數，以驗證 Azure Active Directory 所發出的權杖：</span><span class="sxs-lookup"><span data-stu-id="b7c13-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="b7c13-168">`UseAuthentication` 和 `UseAuthorization` 確保：</span><span class="sxs-lookup"><span data-stu-id="b7c13-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="b7c13-169">應用程式會嘗試剖析並驗證傳入要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="b7c13-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="b7c13-170">嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。</span><span class="sxs-lookup"><span data-stu-id="b7c13-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="b7c13-171">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="b7c13-171">App settings</span></span>

<span data-ttu-id="b7c13-172">*Appsettings*包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。</span><span class="sxs-lookup"><span data-stu-id="b7c13-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

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

### <a name="weatherforecast-controller"></a><span data-ttu-id="b7c13-173">WeatherForecast 控制器</span><span class="sxs-lookup"><span data-stu-id="b7c13-173">WeatherForecast controller</span></span>

<span data-ttu-id="b7c13-174">WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 `[Authorize]` 屬性套用至控制器。</span><span class="sxs-lookup"><span data-stu-id="b7c13-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="b7c13-175">請**務必**瞭解：</span><span class="sxs-lookup"><span data-stu-id="b7c13-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="b7c13-176">此 API 控制器中的 `[Authorize]` 屬性是保護此 API 免于未經授權存取的唯一做法。</span><span class="sxs-lookup"><span data-stu-id="b7c13-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="b7c13-177">Blazor WebAssembly 應用程式中使用的 `[Authorize]` 屬性僅做為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。</span><span class="sxs-lookup"><span data-stu-id="b7c13-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="b7c13-178">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="b7c13-178">Client app configuration</span></span>

<span data-ttu-id="b7c13-179">*本節適用于解決方案的**用戶端**應用程式。*</span><span class="sxs-lookup"><span data-stu-id="b7c13-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="b7c13-180">驗證套件</span><span class="sxs-lookup"><span data-stu-id="b7c13-180">Authentication package</span></span>

<span data-ttu-id="b7c13-181">當您建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="b7c13-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="b7c13-182">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="b7c13-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="b7c13-183">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="b7c13-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="b7c13-184">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="b7c13-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="b7c13-185">`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7c13-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="b7c13-186">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="b7c13-186">Authentication service support</span></span>

<span data-ttu-id="b7c13-187">驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="b7c13-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="b7c13-188">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="b7c13-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="b7c13-189">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="b7c13-189">*Program.cs*:</span></span>

<span data-ttu-id="b7c13-190">產生*用戶端應用程式*時，預設存取權杖範圍的格式為 `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`。</span><span class="sxs-lookup"><span data-stu-id="b7c13-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="b7c13-191">**移除範圍值的 `api://` 部分。**</span><span class="sxs-lookup"><span data-stu-id="b7c13-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="b7c13-192">此問題將在未來的預覽版本中解決。</span><span class="sxs-lookup"><span data-stu-id="b7c13-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="b7c13-193">預設存取權杖範圍的格式必須是 `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` （例如，`11111111-1111-1111-1111-111111111111/API.Access`）。</span><span class="sxs-lookup"><span data-stu-id="b7c13-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="b7c13-194">如果將配置或配置和主機提供給範圍設定（如 Azure 入口網站中所示），則*用戶端應用程式*會在收到來自*伺服器 API 應用程式*的*401 未經授權*回應時，擲回未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b7c13-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="b7c13-195">`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="b7c13-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="b7c13-196">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="b7c13-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="b7c13-197">預設存取權杖範圍代表存取權杖範圍的清單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b7c13-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="b7c13-198">預設包含在登入要求中。</span><span class="sxs-lookup"><span data-stu-id="b7c13-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="b7c13-199">用來在驗證之後立即布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="b7c13-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="b7c13-200">所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。</span><span class="sxs-lookup"><span data-stu-id="b7c13-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="b7c13-201">您可以視需要為其他 API 應用程式新增額外的範圍：</span><span class="sxs-lookup"><span data-stu-id="b7c13-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="b7c13-202">索引頁面</span><span class="sxs-lookup"><span data-stu-id="b7c13-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="b7c13-203">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="b7c13-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="b7c13-204">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="b7c13-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="b7c13-205">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="b7c13-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="b7c13-206">驗證元件</span><span class="sxs-lookup"><span data-stu-id="b7c13-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="b7c13-207">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="b7c13-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="b7c13-208">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b7c13-208">Run the app</span></span>

<span data-ttu-id="b7c13-209">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7c13-209">Run the app from the Server project.</span></span> <span data-ttu-id="b7c13-210">使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7c13-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="b7c13-211">其他資源</span><span class="sxs-lookup"><span data-stu-id="b7c13-211">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
