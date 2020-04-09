---
title: 使用認證庫保護BlazorASP.NET核心 Web 大會獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 893fff10df37e1c2be549604f4cb83cd20049108
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977037"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="48ba6-102">使用認證庫保護BlazorASP.NET核心 Web 大會獨立應用</span><span class="sxs-lookup"><span data-stu-id="48ba6-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="48ba6-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="48ba6-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="48ba6-104">*對於 Azure 活動目錄 (AAD) 和 Azure 活動目錄 B2C (AAD B2C),不要遵循本主題中的指南。請參閱此目錄節點中的 AAD 和 AAD B2C 主題。*</span><span class="sxs-lookup"><span data-stu-id="48ba6-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="48ba6-105">要建立Blazor`Microsoft.AspNetCore.Components.WebAssembly.Authentication`使用函式庫的 WebAssembly 獨立應用,請在命令外殼中執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="48ba6-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="48ba6-106">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="48ba6-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="48ba6-107">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="48ba6-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="48ba6-108">在視覺化工作室[中,建立BlazorWeb 組裝應用](xref:blazor/get-started)。</span><span class="sxs-lookup"><span data-stu-id="48ba6-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="48ba6-109">使用**應用程式商店使用者帳戶套用內**選項將**認證**設定為**單個使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="48ba6-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="48ba6-110">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="48ba6-110">Authentication package</span></span>

<span data-ttu-id="48ba6-111">創建應用以使用個人使用者帳戶時,應用會自動在應用的專案檔中接收`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包的包引用。</span><span class="sxs-lookup"><span data-stu-id="48ba6-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="48ba6-112">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="48ba6-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="48ba6-113">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="48ba6-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="48ba6-114">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="48ba6-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="48ba6-115">認證服務支援</span><span class="sxs-lookup"><span data-stu-id="48ba6-115">Authentication service support</span></span>

<span data-ttu-id="48ba6-116">使用`AddOidcAuthentication``Microsoft.AspNetCore.Components.WebAssembly.Authentication`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="48ba6-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="48ba6-117">此方法設置應用與標識提供者 (IP) 互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="48ba6-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="48ba6-118">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="48ba6-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="48ba6-119">使用開放 ID 連接 (OIDC) 為獨立應用提供身份驗證支援。</span><span class="sxs-lookup"><span data-stu-id="48ba6-119">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="48ba6-120">該方法`AddOidcAuthentication`接受回調以配置使用 OIDC 對應用進行身份驗證所需的參數。</span><span class="sxs-lookup"><span data-stu-id="48ba6-120">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="48ba6-121">配置應用所需的值可以從符合 OIDC 的 IP 獲得。</span><span class="sxs-lookup"><span data-stu-id="48ba6-121">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="48ba6-122">註冊應用時獲取值,這些值通常發生在其連線門戶中。</span><span class="sxs-lookup"><span data-stu-id="48ba6-122">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="48ba6-123">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="48ba6-123">Access token scopes</span></span>

<span data-ttu-id="48ba6-124">WebAssemblyBlazor範本不會自動配置應用以請求安全 API 的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="48ba6-124">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="48ba6-125">要將權杖預先設定登入串流的一部分,請將作用網域`OidcProviderOptions`加入預設的權限的功能的功能的功能:</span><span class="sxs-lookup"><span data-stu-id="48ba6-125">To provision a token as part of the sign-in flow, add the scope to the default token scopes of the `OidcProviderOptions`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="48ba6-126">如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。</span><span class="sxs-lookup"><span data-stu-id="48ba6-126">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="48ba6-127">例如,Azure 門戶可以提供以下作用域 URI 格式之一:</span><span class="sxs-lookup"><span data-stu-id="48ba6-127">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="48ba6-128">提供無方案和主機的範圍 URI:</span><span class="sxs-lookup"><span data-stu-id="48ba6-128">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="48ba6-129">如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。</span><span class="sxs-lookup"><span data-stu-id="48ba6-129">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="48ba6-130">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="48ba6-130">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="48ba6-131">索引頁面</span><span class="sxs-lookup"><span data-stu-id="48ba6-131">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="48ba6-132">套用元件</span><span class="sxs-lookup"><span data-stu-id="48ba6-132">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="48ba6-133">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="48ba6-133">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="48ba6-134">登入元件</span><span class="sxs-lookup"><span data-stu-id="48ba6-134">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="48ba6-135">驗證元件</span><span class="sxs-lookup"><span data-stu-id="48ba6-135">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="48ba6-136">其他資源</span><span class="sxs-lookup"><span data-stu-id="48ba6-136">Additional resources</span></span>

* [<span data-ttu-id="48ba6-137">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="48ba6-137">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
