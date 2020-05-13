---
title: Blazor使用 Microsoft 帳戶保護 ASP.NET Core WebAssembly 獨立應用程式
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
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 9fc93cc02129081ac6c777677a0c8d6397724e53
ms.sourcegitcommit: 1250c90c8d87c2513532be5683640b65bfdf9ddb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83153582"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="27c2d-102">Blazor使用 Microsoft 帳戶保護 ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="27c2d-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="27c2d-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="27c2d-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="27c2d-104">若要建立 Blazor WebAssembly 獨立應用程式，以使用[具有 AZURE ACTIVE DIRECTORY （AAD）的 Microsoft 帳戶](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)進行驗證：</span><span class="sxs-lookup"><span data-stu-id="27c2d-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="27c2d-105">建立 AAD 租使用者和 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="27c2d-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="27c2d-106">在 Azure 入口網站的**Azure Active Directory**  >  **應用程式註冊**] 區域中註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="27c2d-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="27c2d-107">1 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-107">1\.</span></span> <span data-ttu-id="27c2d-108">提供應用程式的**名稱**（例如\*\* Blazor 用戶端 AAD\*\*）。</span><span class="sxs-lookup"><span data-stu-id="27c2d-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="27c2d-109">2 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-109">2\.</span></span> <span data-ttu-id="27c2d-110">在 [**支援的帳戶類型**] 中，選取 [**任何組織目錄中的帳戶**]。</span><span class="sxs-lookup"><span data-stu-id="27c2d-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="27c2d-111">3 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-111">3\.</span></span> <span data-ttu-id="27c2d-112">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供的 [重新導向 uri] `https://localhost:5001/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="27c2d-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="27c2d-113">4 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-113">4\.</span></span> <span data-ttu-id="27c2d-114">停用**Permissions**[授與系統  >  **管理員收到給 openid 和 offline_access 許可權**] 核取方塊的許可權。</span><span class="sxs-lookup"><span data-stu-id="27c2d-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="27c2d-115">5 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-115">5\.</span></span> <span data-ttu-id="27c2d-116">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="27c2d-116">Select **Register**.</span></span>

   <span data-ttu-id="27c2d-117">在 [**驗證**  >  **平臺**設定]  >  **Web**：</span><span class="sxs-lookup"><span data-stu-id="27c2d-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="27c2d-118">1 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-118">1\.</span></span> <span data-ttu-id="27c2d-119">確認的重新**導向 URI** `https://localhost:5001/authentication/login-callback` 存在。</span><span class="sxs-lookup"><span data-stu-id="27c2d-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="27c2d-120">2 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-120">2\.</span></span> <span data-ttu-id="27c2d-121">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="27c2d-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="27c2d-122">3 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-122">3\.</span></span> <span data-ttu-id="27c2d-123">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="27c2d-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="27c2d-124">4 \。</span><span class="sxs-lookup"><span data-stu-id="27c2d-124">4\.</span></span> <span data-ttu-id="27c2d-125">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="27c2d-125">Select the **Save** button.</span></span>

   <span data-ttu-id="27c2d-126">記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111` ）。</span><span class="sxs-lookup"><span data-stu-id="27c2d-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="27c2d-127">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="27c2d-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="27c2d-128">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。</span><span class="sxs-lookup"><span data-stu-id="27c2d-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="27c2d-129">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="27c2d-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="27c2d-130">建立應用程式之後，您應該能夠：</span><span class="sxs-lookup"><span data-stu-id="27c2d-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="27c2d-131">使用 Microsoft 帳戶登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="27c2d-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="27c2d-132">Blazor如果您已正確設定應用程式，請使用與獨立應用程式相同的方法，來要求 Microsoft api 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="27c2d-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="27c2d-133">如需詳細資訊，請參閱[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。</span><span class="sxs-lookup"><span data-stu-id="27c2d-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="27c2d-134">驗證套件</span><span class="sxs-lookup"><span data-stu-id="27c2d-134">Authentication package</span></span>

<span data-ttu-id="27c2d-135">建立應用程式以使用工作或學校帳戶（）時 `SingleOrg` ，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（）的套件參考 `Microsoft.Authentication.WebAssembly.Msal` 。</span><span class="sxs-lookup"><span data-stu-id="27c2d-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="27c2d-136">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="27c2d-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="27c2d-137">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="27c2d-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="27c2d-138">`{VERSION}`將前述套件參考中的取代為發行 `Microsoft.AspNetCore.Blazor.Templates` 項中所顯示的套件版本 <xref:blazor/get-started> 。</span><span class="sxs-lookup"><span data-stu-id="27c2d-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="27c2d-139">`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="27c2d-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="27c2d-140">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="27c2d-140">Authentication service support</span></span>

<span data-ttu-id="27c2d-141">使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 `AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal` 。</span><span class="sxs-lookup"><span data-stu-id="27c2d-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="27c2d-142">這個方法會設定應用程式與 Identity 提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="27c2d-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="27c2d-143">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="27c2d-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
});
```

<span data-ttu-id="27c2d-144">`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="27c2d-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="27c2d-145">當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="27c2d-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

<span data-ttu-id="27c2d-146">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="27c2d-146">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/common",
    "ClientId": "{CLIENT ID}"
  }
}
```

<span data-ttu-id="27c2d-147">範例：</span><span class="sxs-lookup"><span data-stu-id="27c2d-147">Example:</span></span>

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/common",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd"
  }
}
```

## <a name="access-token-scopes"></a><span data-ttu-id="27c2d-148">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="27c2d-148">Access token scopes</span></span>

<span data-ttu-id="27c2d-149">BlazorWebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="27c2d-149">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="27c2d-150">若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍 `MsalProviderOptions` ：</span><span class="sxs-lookup"><span data-stu-id="27c2d-150">To provision an access token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="27c2d-151">如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="27c2d-151">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="27c2d-152">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="27c2d-152">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="27c2d-153">提供不含配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="27c2d-153">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="27c2d-154">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="27c2d-154">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="27c2d-155">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="27c2d-155">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="27c2d-156">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="27c2d-156">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a><span data-ttu-id="27c2d-157">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="27c2d-157">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="27c2d-158">索引頁面</span><span class="sxs-lookup"><span data-stu-id="27c2d-158">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="27c2d-159">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="27c2d-159">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="27c2d-160">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="27c2d-160">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="27c2d-161">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="27c2d-161">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="27c2d-162">驗證元件</span><span class="sxs-lookup"><span data-stu-id="27c2d-162">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="27c2d-163">其他資源</span><span class="sxs-lookup"><span data-stu-id="27c2d-163">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* [<span data-ttu-id="27c2d-164">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="27c2d-164">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/blazor/webassembly/aad-groups-roles>
* [<span data-ttu-id="27c2d-165">快速入門：使用 Microsoft 身分識別平台來註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="27c2d-165">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="27c2d-166">快速入門：設定應用程式以公開 Web API</span><span class="sxs-lookup"><span data-stu-id="27c2d-166">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
