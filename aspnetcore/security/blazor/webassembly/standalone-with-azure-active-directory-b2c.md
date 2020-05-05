---
title: 使用 Azure Active Directory B2C 保護Blazor ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0fb4f4176f214d6bf0c005838a0ccbe4487243f2
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767970"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="d8379-102">使用 Azure Active Directory B2C 保護Blazor ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="d8379-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="d8379-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d8379-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="d8379-104">若要建立Blazor使用[Azure Active Directory （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 WebAssembly 獨立應用程式：</span><span class="sxs-lookup"><span data-stu-id="d8379-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="d8379-105">遵循下列主題中的指導方針，在 Azure 入口網站中建立租使用者並註冊 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="d8379-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="d8379-106">[建立 AAD B2C 租](/azure/active-directory-b2c/tutorial-create-tenant) &ndash;使用者記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="d8379-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="d8379-107">1 \。</span><span class="sxs-lookup"><span data-stu-id="d8379-107">1\.</span></span> <span data-ttu-id="d8379-108">AAD B2C 實例（ `https://contoso.b2clogin.com/`例如，包含尾端斜線的）</span><span class="sxs-lookup"><span data-stu-id="d8379-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="d8379-109">2 \。</span><span class="sxs-lookup"><span data-stu-id="d8379-109">2\.</span></span> <span data-ttu-id="d8379-110">AAD B2C 租使用者網域（例如， `contoso.onmicrosoft.com`）</span><span class="sxs-lookup"><span data-stu-id="d8379-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="d8379-111">[註冊 web 應用程式](/azure/active-directory-b2c/tutorial-register-applications) &ndash;在應用程式註冊期間進行下列選擇：</span><span class="sxs-lookup"><span data-stu-id="d8379-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="d8379-112">1 \。</span><span class="sxs-lookup"><span data-stu-id="d8379-112">1\.</span></span> <span data-ttu-id="d8379-113">將 [ **Web 應用程式/WEB API** ] 設定為 **[是]**。</span><span class="sxs-lookup"><span data-stu-id="d8379-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="d8379-114">2 \。</span><span class="sxs-lookup"><span data-stu-id="d8379-114">2\.</span></span> <span data-ttu-id="d8379-115">將 [**允許隱含流程**] 設為 **[是]**。</span><span class="sxs-lookup"><span data-stu-id="d8379-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="d8379-116">3 \。</span><span class="sxs-lookup"><span data-stu-id="d8379-116">3\.</span></span> <span data-ttu-id="d8379-117">新增的 [**回復 URL** ] `https://localhost:5001/authentication/login-callback`。</span><span class="sxs-lookup"><span data-stu-id="d8379-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="d8379-118">記錄應用程式識別碼（用戶端識別碼）（例如`11111111-1111-1111-1111-111111111111`）。</span><span class="sxs-lookup"><span data-stu-id="d8379-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="d8379-119">[建立使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash;建立註冊和登入使用者流程。</span><span class="sxs-lookup"><span data-stu-id="d8379-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="d8379-120">至少選取 [**應用程式宣告** > **顯示名稱**] 使用者屬性，以填入`context.User.Identity.Name` `LoginDisplay`元件（*Shared/LoginDisplay*）中的。</span><span class="sxs-lookup"><span data-stu-id="d8379-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="d8379-121">記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin`）。</span><span class="sxs-lookup"><span data-stu-id="d8379-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="d8379-122">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="d8379-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="d8379-123">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。</span><span class="sxs-lookup"><span data-stu-id="d8379-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="d8379-124">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="d8379-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="d8379-125">驗證套件</span><span class="sxs-lookup"><span data-stu-id="d8379-125">Authentication package</span></span>

<span data-ttu-id="d8379-126">建立應用程式以使用個別 B2C 帳戶（`IndividualB2C`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="d8379-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="d8379-127">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="d8379-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="d8379-128">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="d8379-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="d8379-129">將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。</span><span class="sxs-lookup"><span data-stu-id="d8379-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="d8379-130">`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="d8379-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="d8379-131">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="d8379-131">Authentication service support</span></span>

<span data-ttu-id="d8379-132">使用`AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal`封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援。</span><span class="sxs-lookup"><span data-stu-id="d8379-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="d8379-133">這個方法會設定應用程式與Identity提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="d8379-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="d8379-134">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="d8379-134">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
});
```

<span data-ttu-id="d8379-135">`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="d8379-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="d8379-136">當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="d8379-136">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

<span data-ttu-id="d8379-137">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="d8379-137">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

<span data-ttu-id="d8379-138">範例：</span><span class="sxs-lookup"><span data-stu-id="d8379-138">Example:</span></span>

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd"
  }
}
```

## <a name="access-token-scopes"></a><span data-ttu-id="d8379-139">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="d8379-139">Access token scopes</span></span>

<span data-ttu-id="d8379-140">Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="d8379-140">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="d8379-141">若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍`MsalProviderOptions`：</span><span class="sxs-lookup"><span data-stu-id="d8379-141">To provision an access token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="d8379-142">如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="d8379-142">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="d8379-143">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="d8379-143">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="d8379-144">提供不含配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="d8379-144">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="d8379-145">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="d8379-145">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="d8379-146">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="d8379-146">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="d8379-147">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="d8379-147">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a><span data-ttu-id="d8379-148">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="d8379-148">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="d8379-149">索引頁面</span><span class="sxs-lookup"><span data-stu-id="d8379-149">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="d8379-150">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="d8379-150">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="d8379-151">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="d8379-151">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="d8379-152">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="d8379-152">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="d8379-153">驗證元件</span><span class="sxs-lookup"><span data-stu-id="d8379-153">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="d8379-154">其他資源</span><span class="sxs-lookup"><span data-stu-id="d8379-154">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="d8379-155">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="d8379-155">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="d8379-156">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="d8379-156">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
