---
title: 使用 Microsoft 帳戶Blazor保護 ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: a12cc8f94a97882e4a0ac3a6553628df4da2e82c
ms.sourcegitcommit: 7bb14d005155a5044c7902a08694ee8ccb20c113
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82111184"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="27d68-102">使用 Microsoft 帳戶Blazor保護 ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="27d68-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="27d68-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="27d68-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

> [!NOTE]
> <span data-ttu-id="27d68-104">本文中的指導方針適用于 ASP.NET Core 3.2 Preview 4。</span><span class="sxs-lookup"><span data-stu-id="27d68-104">The guidance in this article applies to ASP.NET Core 3.2 Preview 4.</span></span> <span data-ttu-id="27d68-105">本主題將在4月24日星期五更新為涵蓋 Preview 5。</span><span class="sxs-lookup"><span data-stu-id="27d68-105">This topic will be updated to cover Preview 5 on Friday, April 24.</span></span>

<span data-ttu-id="27d68-106">若要建立Blazor WebAssembly 獨立應用程式，以使用[具有 Azure Active Directory （AAD）的 Microsoft 帳戶](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)進行驗證：</span><span class="sxs-lookup"><span data-stu-id="27d68-106">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="27d68-107">建立 AAD 租使用者和 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="27d68-107">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="27d68-108">在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**] 區域中註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="27d68-108">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="27d68-109">1 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-109">1\.</span></span> <span data-ttu-id="27d68-110">提供應用程式的**名稱**（例如\*\* Blazor用戶端 AAD\*\*）。</span><span class="sxs-lookup"><span data-stu-id="27d68-110">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="27d68-111">2 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-111">2\.</span></span> <span data-ttu-id="27d68-112">在 [**支援的帳戶類型**] 中，選取 [**任何組織目錄中的帳戶**]。</span><span class="sxs-lookup"><span data-stu-id="27d68-112">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="27d68-113">3 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-113">3\.</span></span> <span data-ttu-id="27d68-114">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供`https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="27d68-114">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="27d68-115">4 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-115">4\.</span></span> <span data-ttu-id="27d68-116">停用 [授與系統**管理員收到給 openid 和 offline_access 許可權**] 核取方塊的**許可權** > 。</span><span class="sxs-lookup"><span data-stu-id="27d68-116">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="27d68-117">5 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-117">5\.</span></span> <span data-ttu-id="27d68-118">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="27d68-118">Select **Register**.</span></span>

   <span data-ttu-id="27d68-119">在 [**驗證** > **平臺** > 設定]**Web**：</span><span class="sxs-lookup"><span data-stu-id="27d68-119">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="27d68-120">1 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-120">1\.</span></span> <span data-ttu-id="27d68-121">確認的重新**導向 URI** `https://localhost:5001/authentication/login-callback`存在。</span><span class="sxs-lookup"><span data-stu-id="27d68-121">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="27d68-122">2 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-122">2\.</span></span> <span data-ttu-id="27d68-123">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="27d68-123">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="27d68-124">3 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-124">3\.</span></span> <span data-ttu-id="27d68-125">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="27d68-125">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="27d68-126">4 \。</span><span class="sxs-lookup"><span data-stu-id="27d68-126">4\.</span></span> <span data-ttu-id="27d68-127">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="27d68-127">Select the **Save** button.</span></span>

   <span data-ttu-id="27d68-128">記錄應用程式識別碼（用戶端識別碼）（例如`11111111-1111-1111-1111-111111111111`）。</span><span class="sxs-lookup"><span data-stu-id="27d68-128">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="27d68-129">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="27d68-129">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="27d68-130">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。</span><span class="sxs-lookup"><span data-stu-id="27d68-130">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="27d68-131">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="27d68-131">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="27d68-132">建立應用程式之後，您應該能夠：</span><span class="sxs-lookup"><span data-stu-id="27d68-132">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="27d68-133">使用 Microsoft 帳戶登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="27d68-133">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="27d68-134">如果您已正確設定應用程式，請使用與獨立Blazor應用程式相同的方法，來要求 Microsoft api 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="27d68-134">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="27d68-135">如需詳細資訊，請參閱[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。</span><span class="sxs-lookup"><span data-stu-id="27d68-135">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="27d68-136">驗證套件</span><span class="sxs-lookup"><span data-stu-id="27d68-136">Authentication package</span></span>

<span data-ttu-id="27d68-137">建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="27d68-137">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="27d68-138">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="27d68-138">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="27d68-139">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="27d68-139">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="27d68-140">將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。</span><span class="sxs-lookup"><span data-stu-id="27d68-140">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="27d68-141">`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="27d68-141">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="27d68-142">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="27d68-142">Authentication service support</span></span>

<span data-ttu-id="27d68-143">使用`AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal`封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援。</span><span class="sxs-lookup"><span data-stu-id="27d68-143">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="27d68-144">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="27d68-144">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="27d68-145">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="27d68-145">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="27d68-146">`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="27d68-146">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="27d68-147">當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="27d68-147">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="27d68-148">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="27d68-148">Access token scopes</span></span>

<span data-ttu-id="27d68-149">Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="27d68-149">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="27d68-150">若要布建權杖做為登入流程的一部分，請將範圍新增至的預設存取權杖範圍`MsalProviderOptions`：</span><span class="sxs-lookup"><span data-stu-id="27d68-150">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="27d68-151">如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="27d68-151">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="27d68-152">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="27d68-152">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="27d68-153">提供不含配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="27d68-153">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="27d68-154">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="27d68-154">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

<!--
    For more information, see <xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests>.
-->

## <a name="imports-file"></a><span data-ttu-id="27d68-155">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="27d68-155">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="27d68-156">索引頁面</span><span class="sxs-lookup"><span data-stu-id="27d68-156">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="27d68-157">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="27d68-157">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="27d68-158">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="27d68-158">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="27d68-159">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="27d68-159">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="27d68-160">驗證元件</span><span class="sxs-lookup"><span data-stu-id="27d68-160">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="27d68-161">其他資源</span><span class="sxs-lookup"><span data-stu-id="27d68-161">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* [<span data-ttu-id="27d68-162">快速入門：使用 Microsoft 身分識別平台來註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="27d68-162">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="27d68-163">快速入門：設定應用程式以公開 web Api</span><span class="sxs-lookup"><span data-stu-id="27d68-163">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
