---
title: 使用 Azure Active Directory 保護Blazor ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 4c5fc6a07ce52ee9560cd2aa0c7dbc3032522f00
ms.sourcegitcommit: 4f91da9ce4543b39dba5e8920a9500d3ce959746
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82138445"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="25124-102">使用 Azure Active Directory 保護Blazor ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="25124-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="25124-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="25124-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="25124-104">若要建立Blazor使用[Azure Active Directory （AAD）](https://azure.microsoft.com/services/active-directory/)進行驗證的 WebAssembly 獨立應用程式：</span><span class="sxs-lookup"><span data-stu-id="25124-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="25124-105">[建立 AAD 租使用者和 web 應用程式](/azure/active-directory/develop/v2-overview)：</span><span class="sxs-lookup"><span data-stu-id="25124-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="25124-106">在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**] 區域中註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="25124-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="25124-107">提供應用程式的**名稱**（例如\*\* Blazor用戶端 AAD\*\*）。</span><span class="sxs-lookup"><span data-stu-id="25124-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="25124-108">選擇**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="25124-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="25124-109">只有在此體驗中，您可以選取**此組織目錄中的帳戶**。</span><span class="sxs-lookup"><span data-stu-id="25124-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="25124-110">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供`https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="25124-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="25124-111">停用 [授與系統**管理員收到給 openid 和 offline_access 許可權**] 核取方塊的**許可權** > 。</span><span class="sxs-lookup"><span data-stu-id="25124-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="25124-112">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="25124-112">Select **Register**.</span></span>

<span data-ttu-id="25124-113">在 [**驗證** > **平臺** > 設定]**Web**：</span><span class="sxs-lookup"><span data-stu-id="25124-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="25124-114">確認的重新**導向 URI** `https://localhost:5001/authentication/login-callback`存在。</span><span class="sxs-lookup"><span data-stu-id="25124-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="25124-115">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="25124-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="25124-116">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="25124-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="25124-117">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="25124-117">Select the **Save** button.</span></span>

<span data-ttu-id="25124-118">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="25124-118">Record the following information:</span></span>

* <span data-ttu-id="25124-119">應用程式識別碼（用戶端識別碼）（例如`11111111-1111-1111-1111-111111111111`，）</span><span class="sxs-lookup"><span data-stu-id="25124-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="25124-120">目錄識別碼（租使用者識別碼）（例如， `22222222-2222-2222-2222-222222222222`）</span><span class="sxs-lookup"><span data-stu-id="25124-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="25124-121">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="25124-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="25124-122">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。</span><span class="sxs-lookup"><span data-stu-id="25124-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="25124-123">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="25124-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="25124-124">驗證套件</span><span class="sxs-lookup"><span data-stu-id="25124-124">Authentication package</span></span>

<span data-ttu-id="25124-125">建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="25124-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="25124-126">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="25124-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="25124-127">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="25124-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="25124-128">將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。</span><span class="sxs-lookup"><span data-stu-id="25124-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="25124-129">`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="25124-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="25124-130">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="25124-130">Authentication service support</span></span>

<span data-ttu-id="25124-131">使用`AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal`封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援。</span><span class="sxs-lookup"><span data-stu-id="25124-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="25124-132">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="25124-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="25124-133">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="25124-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
});
```

<span data-ttu-id="25124-134">`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="25124-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="25124-135">當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="25124-135">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

<span data-ttu-id="25124-136">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="25124-136">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
    "AzureAd": {
        "Authority": "https://login.microsoftonline.com/{TENANT ID}",
        "ClientId": "{CLIENT ID}"
    }
}
```

<span data-ttu-id="25124-137">範例：</span><span class="sxs-lookup"><span data-stu-id="25124-137">Example:</span></span>

```json
{
    "AzureAd": {
        "Authority": "https://login.microsoftonline.com/e86c78e2-...-918e0565a45e",
        "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd"
    }
}
```

## <a name="access-token-scopes"></a><span data-ttu-id="25124-138">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="25124-138">Access token scopes</span></span>

<span data-ttu-id="25124-139">Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="25124-139">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="25124-140">若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍`MsalProviderOptions`：</span><span class="sxs-lookup"><span data-stu-id="25124-140">To provision an access token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="25124-141">如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="25124-141">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="25124-142">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="25124-142">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="25124-143">提供不含配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="25124-143">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="25124-144">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="25124-144">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="25124-145">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="25124-145">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="25124-146">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="25124-146">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a><span data-ttu-id="25124-147">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="25124-147">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="25124-148">索引頁面</span><span class="sxs-lookup"><span data-stu-id="25124-148">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="25124-149">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="25124-149">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="25124-150">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="25124-150">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="25124-151">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="25124-151">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="25124-152">驗證元件</span><span class="sxs-lookup"><span data-stu-id="25124-152">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="25124-153">其他資源</span><span class="sxs-lookup"><span data-stu-id="25124-153">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="25124-154">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="25124-154">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
