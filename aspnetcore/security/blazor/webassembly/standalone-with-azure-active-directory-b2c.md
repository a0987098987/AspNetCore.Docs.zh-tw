---
title: 使用 Azure Active Directory B2C 保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: bb03ef1e6d216cfc06e2b91919c64f92f2ef634e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219268"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="ccb4f-102">使用 Azure Active Directory B2C 保護 ASP.NET Core Blazor WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="ccb4f-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="ccb4f-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ccb4f-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="ccb4f-104">若要建立使用[Azure Active Directory （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 Blazor WebAssembly 獨立應用程式：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="ccb4f-105">遵循下列主題中的指導方針，在 Azure 入口網站中建立租使用者並註冊 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="ccb4f-106">[建立 AAD B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者 &ndash; 記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="ccb4f-107">1\.</span><span class="sxs-lookup"><span data-stu-id="ccb4f-107">1\.</span></span> <span data-ttu-id="ccb4f-108">AAD B2C 實例（例如，`https://contoso.b2clogin.com/`，其中包含尾端斜線）</span><span class="sxs-lookup"><span data-stu-id="ccb4f-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="ccb4f-109">2\.</span><span class="sxs-lookup"><span data-stu-id="ccb4f-109">2\.</span></span> <span data-ttu-id="ccb4f-110">AAD B2C 租使用者網域（例如，`contoso.onmicrosoft.com`）</span><span class="sxs-lookup"><span data-stu-id="ccb4f-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="ccb4f-111">[註冊 web 應用程式](/azure/active-directory-b2c/tutorial-register-applications)&ndash; 在應用程式註冊期間進行下列選擇：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="ccb4f-112">1\.</span><span class="sxs-lookup"><span data-stu-id="ccb4f-112">1\.</span></span> <span data-ttu-id="ccb4f-113">將 [ **Web 應用程式/WEB API** ] 設定為 **[是]** 。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="ccb4f-114">2\.</span><span class="sxs-lookup"><span data-stu-id="ccb4f-114">2\.</span></span> <span data-ttu-id="ccb4f-115">將 [**允許隱含流程**] 設為 **[是]** 。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="ccb4f-116">3\.</span><span class="sxs-lookup"><span data-stu-id="ccb4f-116">3\.</span></span> <span data-ttu-id="ccb4f-117">新增 `https://localhost:5001/authentication/login-callback`的 [**回復 URL** ]。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="ccb4f-118">記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="ccb4f-119">[建立使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows)&ndash; 建立註冊和登入使用者流程。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="ccb4f-120">至少選取 **應用程式宣告** > **顯示名稱**使用者屬性，以在 `LoginDisplay` 元件（*Shared/LoginDisplay*）中填入 `context.User.Identity.Name`。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="ccb4f-121">記錄為應用程式建立的註冊和登入使用者流程名稱（例如，`B2C_1_signupsignin`）。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="ccb4f-122">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="ccb4f-123">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="ccb4f-124">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="ccb4f-125">驗證套件</span><span class="sxs-lookup"><span data-stu-id="ccb4f-125">Authentication package</span></span>

<span data-ttu-id="ccb4f-126">當您建立應用程式以使用個別 B2C 帳戶（`IndividualB2C`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="ccb4f-127">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="ccb4f-128">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="ccb4f-129">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="ccb4f-130">`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="ccb4f-131">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="ccb4f-131">Authentication service support</span></span>

<span data-ttu-id="ccb4f-132">驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="ccb4f-133">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="ccb4f-134">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-134">*Program.cs*:</span></span>

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

<span data-ttu-id="ccb4f-135">`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="ccb4f-136">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="ccb4f-137">Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="ccb4f-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="ccb4f-138">若要布建權杖做為登入流程的一部分，請將範圍新增至 `MsalProviderOptions`的預設存取權杖範圍：</span><span class="sxs-lookup"><span data-stu-id="ccb4f-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a><span data-ttu-id="ccb4f-139">索引頁面</span><span class="sxs-lookup"><span data-stu-id="ccb4f-139">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="ccb4f-140">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="ccb4f-140">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="ccb4f-141">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="ccb4f-141">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="ccb4f-142">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="ccb4f-142">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="ccb4f-143">驗證元件</span><span class="sxs-lookup"><span data-stu-id="ccb4f-143">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="ccb4f-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="ccb4f-144">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="ccb4f-145">教學課程：建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="ccb4f-145">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
