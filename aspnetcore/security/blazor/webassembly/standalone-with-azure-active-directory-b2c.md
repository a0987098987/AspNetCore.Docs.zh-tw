---
title: 使用 AzureBlazor活動目錄 B2C 保護 ASP.NET核心 Web 元件獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0734bad2d4281eb856783a362ef8c608a303c17a
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977050"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="c5a39-102">使用 AzureBlazor活動目錄 B2C 保護 ASP.NET核心 Web 元件獨立應用</span><span class="sxs-lookup"><span data-stu-id="c5a39-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="c5a39-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c5a39-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="c5a39-104">要建立Blazor[Azure 活動目錄 (AAD) B2C](/azure/active-directory-b2c/overview)進行身份驗證的 Web 大會獨立應用,請執行:</span><span class="sxs-lookup"><span data-stu-id="c5a39-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="c5a39-105">按照以下主題中的指南創建租戶並在 Azure 門戶中註冊 Web 應用:</span><span class="sxs-lookup"><span data-stu-id="c5a39-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="c5a39-106">[建立 AAD B2C 租戶](/azure/active-directory-b2c/tutorial-create-tenant)&ndash;記錄以下資訊:</span><span class="sxs-lookup"><span data-stu-id="c5a39-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="c5a39-107">1\.</span><span class="sxs-lookup"><span data-stu-id="c5a39-107">1\.</span></span> <span data-ttu-id="c5a39-108">AAD B2C 實體(`https://contoso.b2clogin.com/`例如 ,包括尾隨斜杠 )</span><span class="sxs-lookup"><span data-stu-id="c5a39-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="c5a39-109">2\.</span><span class="sxs-lookup"><span data-stu-id="c5a39-109">2\.</span></span> <span data-ttu-id="c5a39-110">AAD B2C 租戶域`contoso.onmicrosoft.com`(例如 ,</span><span class="sxs-lookup"><span data-stu-id="c5a39-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="c5a39-111">[註冊 Web 應用程式](/azure/active-directory-b2c/tutorial-register-applications)&ndash;在應用程式註冊期間進行以下選擇:</span><span class="sxs-lookup"><span data-stu-id="c5a39-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="c5a39-112">1\.</span><span class="sxs-lookup"><span data-stu-id="c5a39-112">1\.</span></span> <span data-ttu-id="c5a39-113">將**Web 應用/Web API**設定為 **"是**"。</span><span class="sxs-lookup"><span data-stu-id="c5a39-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="c5a39-114">2\.</span><span class="sxs-lookup"><span data-stu-id="c5a39-114">2\.</span></span> <span data-ttu-id="c5a39-115">將**允許隱式流**設置為 **"是**"。</span><span class="sxs-lookup"><span data-stu-id="c5a39-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="c5a39-116">3\.</span><span class="sxs-lookup"><span data-stu-id="c5a39-116">3\.</span></span> <span data-ttu-id="c5a39-117">添加`https://localhost:5001/authentication/login-callback`的**回覆 URL。**</span><span class="sxs-lookup"><span data-stu-id="c5a39-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="c5a39-118">記錄應用程式 ID(用戶端 ID)(`11111111-1111-1111-1111-111111111111`例如,</span><span class="sxs-lookup"><span data-stu-id="c5a39-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="c5a39-119">[創建用戶流](/azure/active-directory-b2c/tutorial-create-user-flows)&ndash;創建註冊和登錄使用者流。</span><span class="sxs-lookup"><span data-stu-id="c5a39-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="c5a39-120">至少,選擇**應用程式聲明** > **顯示名稱**使用者屬性以`context.User.Identity.Name``LoginDisplay`填充 元件中的 (*共用/ LoginDisplay.razor)。*</span><span class="sxs-lookup"><span data-stu-id="c5a39-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="c5a39-121">記錄為應用創建的註冊和登錄使用者流名稱(例如。 `B2C_1_signupsignin`</span><span class="sxs-lookup"><span data-stu-id="c5a39-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="c5a39-122">將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:</span><span class="sxs-lookup"><span data-stu-id="c5a39-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="c5a39-123">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="c5a39-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="c5a39-124">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="c5a39-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="c5a39-125">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="c5a39-125">Authentication package</span></span>

<span data-ttu-id="c5a39-126">建立使用單個 B2C 帳號 ()`IndividualB2C`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。</span><span class="sxs-lookup"><span data-stu-id="c5a39-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="c5a39-127">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="c5a39-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="c5a39-128">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="c5a39-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="c5a39-129">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="c5a39-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="c5a39-130">包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。</span><span class="sxs-lookup"><span data-stu-id="c5a39-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="c5a39-131">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="c5a39-131">Authentication service support</span></span>

<span data-ttu-id="c5a39-132">使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="c5a39-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="c5a39-133">此方法設置應用與標識提供者 (IP) 互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="c5a39-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="c5a39-134">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c5a39-134">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

<span data-ttu-id="c5a39-135">該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。</span><span class="sxs-lookup"><span data-stu-id="c5a39-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="c5a39-136">註冊應用時,可以從 Azure 門戶 AAD 配置中獲取配置應用所需的值。</span><span class="sxs-lookup"><span data-stu-id="c5a39-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="c5a39-137">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="c5a39-137">Access token scopes</span></span>

<span data-ttu-id="c5a39-138">WebAssemblyBlazor範本不會自動配置應用以請求安全 API 的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="c5a39-138">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="c5a39-139">要將權杖預先設定登入串流的一部分,請將作用網域`MsalProviderOptions`加入預設存取權杖的作用區中:</span><span class="sxs-lookup"><span data-stu-id="c5a39-139">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="c5a39-140">如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。</span><span class="sxs-lookup"><span data-stu-id="c5a39-140">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="c5a39-141">例如,Azure 門戶可以提供以下作用域 URI 格式之一:</span><span class="sxs-lookup"><span data-stu-id="c5a39-141">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="c5a39-142">提供無方案和主機的範圍 URI:</span><span class="sxs-lookup"><span data-stu-id="c5a39-142">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="c5a39-143">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="c5a39-143">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="c5a39-144">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="c5a39-144">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="c5a39-145">索引頁面</span><span class="sxs-lookup"><span data-stu-id="c5a39-145">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="c5a39-146">套用元件</span><span class="sxs-lookup"><span data-stu-id="c5a39-146">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="c5a39-147">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="c5a39-147">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="c5a39-148">登入元件</span><span class="sxs-lookup"><span data-stu-id="c5a39-148">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="c5a39-149">驗證元件</span><span class="sxs-lookup"><span data-stu-id="c5a39-149">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="c5a39-150">其他資源</span><span class="sxs-lookup"><span data-stu-id="c5a39-150">Additional resources</span></span>

* [<span data-ttu-id="c5a39-151">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="c5a39-151">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="c5a39-152">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="c5a39-152">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="c5a39-153">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="c5a39-153">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
