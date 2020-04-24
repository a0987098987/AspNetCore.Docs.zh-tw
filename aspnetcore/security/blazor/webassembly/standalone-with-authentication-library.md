---
title: 使用驗證連結Blazor庫保護 ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 25aa7761b9c1acc72081653422e80cb004500573
ms.sourcegitcommit: 4f91da9ce4543b39dba5e8920a9500d3ce959746
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82138517"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="5fdec-102">使用驗證連結Blazor庫保護 ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="5fdec-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="5fdec-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5fdec-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="5fdec-104">*若為 Azure Active Directory （AAD）和 Azure Active Directory B2C （AAD B2C），請不要遵循本主題中的指導方針。請參閱此目錄節點中的 AAD 和 AAD B2C 主題。*</span><span class="sxs-lookup"><span data-stu-id="5fdec-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="5fdec-105">若要建立Blazor使用`Microsoft.AspNetCore.Components.WebAssembly.Authentication`程式庫的 WebAssembly 獨立應用程式，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="5fdec-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="5fdec-106">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。</span><span class="sxs-lookup"><span data-stu-id="5fdec-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="5fdec-107">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="5fdec-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="5fdec-108">在 Visual Studio 中，[建立Blazor WebAssembly 應用程式](xref:blazor/get-started)。</span><span class="sxs-lookup"><span data-stu-id="5fdec-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="5fdec-109">使用 [**儲存使用者帳戶應用程式內**] 選項，將**驗證**設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="5fdec-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="5fdec-110">驗證套件</span><span class="sxs-lookup"><span data-stu-id="5fdec-110">Authentication package</span></span>

<span data-ttu-id="5fdec-111">建立應用程式以使用個別使用者帳戶時，應用程式會在應用程式的專案檔中`Microsoft.AspNetCore.Components.WebAssembly.Authentication`自動接收套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5fdec-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="5fdec-112">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="5fdec-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="5fdec-113">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="5fdec-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="5fdec-114">將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。</span><span class="sxs-lookup"><span data-stu-id="5fdec-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="5fdec-115">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="5fdec-115">Authentication service support</span></span>

<span data-ttu-id="5fdec-116">使用`AddOidcAuthentication` `Microsoft.AspNetCore.Components.WebAssembly.Authentication`封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援。</span><span class="sxs-lookup"><span data-stu-id="5fdec-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="5fdec-117">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="5fdec-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="5fdec-118">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="5fdec-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    builder.Configuration.Bind("Local", options.ProviderOptions);
});
```

<span data-ttu-id="5fdec-119">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="5fdec-119">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
    "Local": {
        "Authority": "{AUTHORITY}",
        "ClientId": "{CLIENT ID}"
    }
}
```

<span data-ttu-id="5fdec-120">獨立應用程式的驗證支援是使用 Open ID Connect （OIDC）提供。</span><span class="sxs-lookup"><span data-stu-id="5fdec-120">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="5fdec-121">`AddOidcAuthentication`方法會接受回呼來設定使用 OIDC 驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="5fdec-121">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="5fdec-122">設定應用程式所需的值可從符合 OIDC 規範的 IP 取得。</span><span class="sxs-lookup"><span data-stu-id="5fdec-122">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="5fdec-123">當您註冊應用程式時，請取得這些值，這通常會發生在其線上入口網站中。</span><span class="sxs-lookup"><span data-stu-id="5fdec-123">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="5fdec-124">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="5fdec-124">Access token scopes</span></span>

<span data-ttu-id="5fdec-125">Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="5fdec-125">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="5fdec-126">若要在登入流程中布建存取權杖，請將範圍新增至的預設權杖範圍`OidcProviderOptions`：</span><span class="sxs-lookup"><span data-stu-id="5fdec-126">To provision an access token as part of the sign-in flow, add the scope to the default token scopes of the `OidcProviderOptions`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="5fdec-127">如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="5fdec-127">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="5fdec-128">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="5fdec-128">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="5fdec-129">提供不含配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="5fdec-129">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="5fdec-130">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="5fdec-130">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="5fdec-131">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="5fdec-131">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="5fdec-132">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="5fdec-132">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a><span data-ttu-id="5fdec-133">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="5fdec-133">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="5fdec-134">索引頁面</span><span class="sxs-lookup"><span data-stu-id="5fdec-134">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="5fdec-135">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="5fdec-135">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="5fdec-136">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="5fdec-136">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="5fdec-137">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="5fdec-137">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="5fdec-138">驗證元件</span><span class="sxs-lookup"><span data-stu-id="5fdec-138">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="5fdec-139">其他資源</span><span class="sxs-lookup"><span data-stu-id="5fdec-139">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
