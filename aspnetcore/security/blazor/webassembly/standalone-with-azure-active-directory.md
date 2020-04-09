---
title: 使用 AzureBlazor活動目錄保護ASP.NET核心 Web 大會獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 7e132723657b7e12803b67ec12c3a33f1945baa3
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976988"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="920f3-102">使用 AzureBlazor活動目錄保護ASP.NET核心 Web 大會獨立應用</span><span class="sxs-lookup"><span data-stu-id="920f3-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="920f3-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="920f3-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="920f3-104">要建立Blazor[Azure 活動目錄 (AAD)](https://azure.microsoft.com/services/active-directory/)進行身份驗證的 Web 組裝獨立應用,請執行:</span><span class="sxs-lookup"><span data-stu-id="920f3-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="920f3-105">[建立 AAD 租戶與 Web 應用程式](/azure/active-directory/develop/v2-overview):</span><span class="sxs-lookup"><span data-stu-id="920f3-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="920f3-106">在 Azure 門戶的**Azure 活動目錄** > **應用註冊區域註冊**AAD 應用:</span><span class="sxs-lookup"><span data-stu-id="920f3-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="920f3-107">為應用提供**名稱**(例如,**Blazor用戶端 AAD**)。</span><span class="sxs-lookup"><span data-stu-id="920f3-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="920f3-108">選擇**支援的帳號型態 。**</span><span class="sxs-lookup"><span data-stu-id="920f3-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="920f3-109">您只能選擇**這個組織目錄中的帳號**。</span><span class="sxs-lookup"><span data-stu-id="920f3-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="920f3-110">將**重定向 URI**下拉清單設定為**Web,** 並提供`https://localhost:5001/authentication/login-callback`重定向 URI。</span><span class="sxs-lookup"><span data-stu-id="920f3-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="920f3-111">禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。</span><span class="sxs-lookup"><span data-stu-id="920f3-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="920f3-112">選取 [註冊]  。</span><span class="sxs-lookup"><span data-stu-id="920f3-112">Select **Register**.</span></span>

<span data-ttu-id="920f3-113">在**認證** > **平台設定中** > **, Web**:</span><span class="sxs-lookup"><span data-stu-id="920f3-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="920f3-114">確認存在 重定`https://localhost:5001/authentication/login-callback`向**URI。**</span><span class="sxs-lookup"><span data-stu-id="920f3-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="920f3-115">對於**隱式授予**,選擇 Access**權杖**和**ID 權杖的**複選框。</span><span class="sxs-lookup"><span data-stu-id="920f3-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="920f3-116">此體驗可以接受應用的剩餘預設值。</span><span class="sxs-lookup"><span data-stu-id="920f3-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="920f3-117">選取 [儲存]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="920f3-117">Select the **Save** button.</span></span>

<span data-ttu-id="920f3-118">記錄以下資訊:</span><span class="sxs-lookup"><span data-stu-id="920f3-118">Record the following information:</span></span>

* <span data-ttu-id="920f3-119">應用程式代碼(用戶端代碼)(例如`11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="920f3-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="920f3-120">目錄 ID(租戶代碼)(`22222222-2222-2222-2222-222222222222`例如 )</span><span class="sxs-lookup"><span data-stu-id="920f3-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="920f3-121">將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:</span><span class="sxs-lookup"><span data-stu-id="920f3-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="920f3-122">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="920f3-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="920f3-123">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="920f3-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="920f3-124">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="920f3-124">Authentication package</span></span>

<span data-ttu-id="920f3-125">建立應用程式以使用工作帳號或學校帳戶 ()`SingleOrg`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。</span><span class="sxs-lookup"><span data-stu-id="920f3-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="920f3-126">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="920f3-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="920f3-127">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="920f3-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="920f3-128">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="920f3-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="920f3-129">包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。</span><span class="sxs-lookup"><span data-stu-id="920f3-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="920f3-130">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="920f3-130">Authentication service support</span></span>

<span data-ttu-id="920f3-131">使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="920f3-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="920f3-132">此方法設置應用與標識提供者 (IP) 互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="920f3-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="920f3-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="920f3-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="920f3-134">該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。</span><span class="sxs-lookup"><span data-stu-id="920f3-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="920f3-135">註冊應用時,可以從 Azure 門戶 AAD 配置中獲取配置應用所需的值。</span><span class="sxs-lookup"><span data-stu-id="920f3-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="920f3-136">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="920f3-136">Access token scopes</span></span>

<span data-ttu-id="920f3-137">WebAssemblyBlazor範本不會自動配置應用以請求安全 API 的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="920f3-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="920f3-138">要將權杖預先設定登入串流的一部分,請將作用網域`MsalProviderOptions`加入預設存取權杖的作用區中:</span><span class="sxs-lookup"><span data-stu-id="920f3-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="920f3-139">如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。</span><span class="sxs-lookup"><span data-stu-id="920f3-139">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="920f3-140">例如,Azure 門戶可以提供以下作用域 URI 格式之一:</span><span class="sxs-lookup"><span data-stu-id="920f3-140">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="920f3-141">提供無方案和主機的範圍 URI:</span><span class="sxs-lookup"><span data-stu-id="920f3-141">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="920f3-142">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="920f3-142">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="920f3-143">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="920f3-143">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="920f3-144">索引頁面</span><span class="sxs-lookup"><span data-stu-id="920f3-144">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="920f3-145">套用元件</span><span class="sxs-lookup"><span data-stu-id="920f3-145">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="920f3-146">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="920f3-146">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="920f3-147">登入元件</span><span class="sxs-lookup"><span data-stu-id="920f3-147">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="920f3-148">驗證元件</span><span class="sxs-lookup"><span data-stu-id="920f3-148">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="920f3-149">其他資源</span><span class="sxs-lookup"><span data-stu-id="920f3-149">Additional resources</span></span>

* [<span data-ttu-id="920f3-150">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="920f3-150">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="920f3-151">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="920f3-151">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
