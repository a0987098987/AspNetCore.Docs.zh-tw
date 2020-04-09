---
title: 使用 MicrosoftBlazor帳戶保護ASP.NET核心 Web 大會獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 8c409651b3338c2baeae497bef43b994823a20f9
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977076"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="f5d9d-102">使用 MicrosoftBlazor帳戶保護ASP.NET核心 Web 大會獨立應用</span><span class="sxs-lookup"><span data-stu-id="f5d9d-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="f5d9d-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f5d9d-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="f5d9d-104">要建立使用Blazor[具有 Azure 活動目錄 (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)的 Microsoft 帳戶進行身份驗證的 WebAssembly 獨立應用,請執行:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="f5d9d-105">建立 AAD 租戶與 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f5d9d-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="f5d9d-106">在 Azure 門戶的**Azure 活動目錄** > **應用註冊區域註冊**AAD 應用:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="f5d9d-107">1\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-107">1\.</span></span> <span data-ttu-id="f5d9d-108">為應用提供**名稱**(例如,**Blazor用戶端 AAD**)。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="f5d9d-109">2\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-109">2\.</span></span> <span data-ttu-id="f5d9d-110">在**支援帳號型態**型**態中,選擇任何組織目錄中的帳戶**。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="f5d9d-111">3\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-111">3\.</span></span> <span data-ttu-id="f5d9d-112">將**重定向 URI**下拉清單設定為**Web,** 並提供`https://localhost:5001/authentication/login-callback`重定向 URI。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="f5d9d-113">4\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-113">4\.</span></span> <span data-ttu-id="f5d9d-114">禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="f5d9d-115">5\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-115">5\.</span></span> <span data-ttu-id="f5d9d-116">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-116">Select **Register**.</span></span>

   <span data-ttu-id="f5d9d-117">在**認證** > **平台設定中** > **, Web**:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="f5d9d-118">1\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-118">1\.</span></span> <span data-ttu-id="f5d9d-119">確認存在 重定`https://localhost:5001/authentication/login-callback`向**URI。**</span><span class="sxs-lookup"><span data-stu-id="f5d9d-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="f5d9d-120">2\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-120">2\.</span></span> <span data-ttu-id="f5d9d-121">對於**隱式授予**,選擇 Access**權杖**和**ID 權杖的**複選框。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="f5d9d-122">3\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-122">3\.</span></span> <span data-ttu-id="f5d9d-123">此體驗可以接受應用的剩餘預設值。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="f5d9d-124">4\.</span><span class="sxs-lookup"><span data-stu-id="f5d9d-124">4\.</span></span> <span data-ttu-id="f5d9d-125">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-125">Select the **Save** button.</span></span>

   <span data-ttu-id="f5d9d-126">記錄應用程式 ID(用戶端 ID)(`11111111-1111-1111-1111-111111111111`例如,</span><span class="sxs-lookup"><span data-stu-id="f5d9d-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="f5d9d-127">將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="f5d9d-128">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="f5d9d-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="f5d9d-129">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="f5d9d-130">建立應用程式後,您應該能夠:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="f5d9d-131">使用 Microsoft 帳戶登錄應用。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="f5d9d-132">使用與獨立Blazor應用相同的方法請求 Microsoft API 的訪問權杖,前提是您已正確配置應用。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="f5d9d-133">有關詳細資訊,請參閱[快速入門:設定應用程式以公開 Web API](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="f5d9d-134">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="f5d9d-134">Authentication package</span></span>

<span data-ttu-id="f5d9d-135">建立應用程式以使用工作帳號或學校帳戶 ()`SingleOrg`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="f5d9d-136">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="f5d9d-137">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="f5d9d-138">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="f5d9d-139">包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="f5d9d-140">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="f5d9d-140">Authentication service support</span></span>

<span data-ttu-id="f5d9d-141">使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="f5d9d-142">此方法設置應用與標識提供者 (IP) 互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="f5d9d-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="f5d9d-144">該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="f5d9d-145">註冊應用時,可以從 Microsoft 帳戶配置中獲取配置應用所需的值。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="f5d9d-146">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="f5d9d-146">Access token scopes</span></span>

<span data-ttu-id="f5d9d-147">WebAssemblyBlazor範本不會自動配置應用以請求安全 API 的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-147">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="f5d9d-148">要將權杖預先設定登入串流的一部分,請將作用網域`MsalProviderOptions`加入預設存取權杖的作用區中:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-148">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="f5d9d-149">如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-149">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="f5d9d-150">例如,Azure 門戶可以提供以下作用域 URI 格式之一:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-150">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="f5d9d-151">提供無方案和主機的範圍 URI:</span><span class="sxs-lookup"><span data-stu-id="f5d9d-151">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="f5d9d-152">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="f5d9d-152">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="f5d9d-153">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="f5d9d-153">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="f5d9d-154">索引頁面</span><span class="sxs-lookup"><span data-stu-id="f5d9d-154">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="f5d9d-155">套用元件</span><span class="sxs-lookup"><span data-stu-id="f5d9d-155">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="f5d9d-156">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="f5d9d-156">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="f5d9d-157">登入元件</span><span class="sxs-lookup"><span data-stu-id="f5d9d-157">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="f5d9d-158">驗證元件</span><span class="sxs-lookup"><span data-stu-id="f5d9d-158">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="f5d9d-159">其他資源</span><span class="sxs-lookup"><span data-stu-id="f5d9d-159">Additional resources</span></span>

* [<span data-ttu-id="f5d9d-160">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="f5d9d-160">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="f5d9d-161">快速入門:使用 Microsoft 識別平台註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="f5d9d-161">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="f5d9d-162">快速入門:設定應用程式以公開 Web API</span><span class="sxs-lookup"><span data-stu-id="f5d9d-162">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
