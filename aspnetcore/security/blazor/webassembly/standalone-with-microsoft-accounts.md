---
title: 使用 Microsoft 帳戶保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 6883af3486256e7c6905626d8da09e8ae0c4a896
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083822"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="2c457-102">使用 Microsoft 帳戶保護 ASP.NET Core Blazor WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="2c457-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="2c457-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2c457-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="2c457-104">若要建立 Blazor WebAssembly 獨立應用程式，以使用[具有 Azure Active Directory （AAD）的 Microsoft 帳戶](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)進行驗證：</span><span class="sxs-lookup"><span data-stu-id="2c457-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="2c457-105">建立 AAD 租使用者和 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2c457-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="2c457-106">在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="2c457-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="2c457-107">1\.</span><span class="sxs-lookup"><span data-stu-id="2c457-107">1\.</span></span> <span data-ttu-id="2c457-108">提供應用程式的**名稱**（例如， **Blazor 用戶端 AAD**）。</span><span class="sxs-lookup"><span data-stu-id="2c457-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="2c457-109">2\.</span><span class="sxs-lookup"><span data-stu-id="2c457-109">2\.</span></span> <span data-ttu-id="2c457-110">在 [**支援的帳戶類型**] 中，選取 [**任何組織目錄中的帳戶**]。</span><span class="sxs-lookup"><span data-stu-id="2c457-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="2c457-111">3\.</span><span class="sxs-lookup"><span data-stu-id="2c457-111">3\.</span></span> <span data-ttu-id="2c457-112">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供 `https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="2c457-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="2c457-113">4\.</span><span class="sxs-lookup"><span data-stu-id="2c457-113">4\.</span></span> <span data-ttu-id="2c457-114">停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2c457-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="2c457-115">5\.</span><span class="sxs-lookup"><span data-stu-id="2c457-115">5\.</span></span> <span data-ttu-id="2c457-116">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="2c457-116">Select **Register**.</span></span>

   <span data-ttu-id="2c457-117">在 [**驗證** > **平臺**設定 > **Web**]：</span><span class="sxs-lookup"><span data-stu-id="2c457-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="2c457-118">1\.</span><span class="sxs-lookup"><span data-stu-id="2c457-118">1\.</span></span> <span data-ttu-id="2c457-119">確認 `https://localhost:5001/authentication/login-callback` 的重新**導向 URI**存在。</span><span class="sxs-lookup"><span data-stu-id="2c457-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="2c457-120">2\.</span><span class="sxs-lookup"><span data-stu-id="2c457-120">2\.</span></span> <span data-ttu-id="2c457-121">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2c457-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="2c457-122">3\.</span><span class="sxs-lookup"><span data-stu-id="2c457-122">3\.</span></span> <span data-ttu-id="2c457-123">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="2c457-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="2c457-124">4\.</span><span class="sxs-lookup"><span data-stu-id="2c457-124">4\.</span></span> <span data-ttu-id="2c457-125">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="2c457-125">Select the **Save** button.</span></span>

   <span data-ttu-id="2c457-126">記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）。</span><span class="sxs-lookup"><span data-stu-id="2c457-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="2c457-127">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="2c457-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="2c457-128">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="2c457-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="2c457-129">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="2c457-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="2c457-130">建立應用程式之後，您應該能夠：</span><span class="sxs-lookup"><span data-stu-id="2c457-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="2c457-131">使用 Microsoft 帳戶登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c457-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="2c457-132">如果您已正確設定應用程式，請使用與獨立 Blazor 應用程式相同的方法，來要求 Microsoft Api 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="2c457-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="2c457-133">如需詳細資訊，請參閱[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。</span><span class="sxs-lookup"><span data-stu-id="2c457-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="2c457-134">驗證套件</span><span class="sxs-lookup"><span data-stu-id="2c457-134">Authentication package</span></span>

<span data-ttu-id="2c457-135">當您建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="2c457-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="2c457-136">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="2c457-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="2c457-137">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="2c457-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="2c457-138">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="2c457-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="2c457-139">`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="2c457-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="2c457-140">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="2c457-140">Authentication service support</span></span>

<span data-ttu-id="2c457-141">驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="2c457-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="2c457-142">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="2c457-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="2c457-143">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="2c457-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="2c457-144">`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="2c457-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="2c457-145">當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="2c457-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="index-page"></a><span data-ttu-id="2c457-146">索引頁面</span><span class="sxs-lookup"><span data-stu-id="2c457-146">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="2c457-147">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="2c457-147">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="2c457-148">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="2c457-148">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="2c457-149">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="2c457-149">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="2c457-150">驗證元件</span><span class="sxs-lookup"><span data-stu-id="2c457-150">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="2c457-151">其他資源</span><span class="sxs-lookup"><span data-stu-id="2c457-151">Additional resources</span></span>

* [<span data-ttu-id="2c457-152">快速入門：使用 Microsoft 身分識別平臺註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="2c457-152">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="2c457-153">快速入門：設定應用程式以公開 web Api</span><span class="sxs-lookup"><span data-stu-id="2c457-153">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
