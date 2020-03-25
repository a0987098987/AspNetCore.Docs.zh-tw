---
title: 使用 Azure Active Directory 保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: e12c38ed42a4e2714d785ef8f03097246c40d36e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218971"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="45fa1-102">使用 Azure Active Directory 保護 ASP.NET Core Blazor WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="45fa1-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="45fa1-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="45fa1-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="45fa1-104">建立使用[Azure Active Directory （AAD）](https://azure.microsoft.com/services/active-directory/)進行驗證的 Blazor WebAssembly 獨立應用程式：</span><span class="sxs-lookup"><span data-stu-id="45fa1-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="45fa1-105">[建立 AAD 租使用者和 web 應用程式](/azure/active-directory/develop/v2-overview)：</span><span class="sxs-lookup"><span data-stu-id="45fa1-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="45fa1-106">在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="45fa1-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="45fa1-107">提供應用程式的**名稱**（例如， **Blazor 用戶端 AAD**）。</span><span class="sxs-lookup"><span data-stu-id="45fa1-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="45fa1-108">選擇**支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="45fa1-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="45fa1-109">只有在此體驗中，您可以選取**此組織目錄中的帳戶**。</span><span class="sxs-lookup"><span data-stu-id="45fa1-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="45fa1-110">將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供 `https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。</span><span class="sxs-lookup"><span data-stu-id="45fa1-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="45fa1-111">停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="45fa1-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="45fa1-112">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="45fa1-112">Select **Register**.</span></span>

<span data-ttu-id="45fa1-113">在 [**驗證** > **平臺**設定 > **Web**]：</span><span class="sxs-lookup"><span data-stu-id="45fa1-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="45fa1-114">確認 `https://localhost:5001/authentication/login-callback` 的重新**導向 URI**存在。</span><span class="sxs-lookup"><span data-stu-id="45fa1-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="45fa1-115">針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="45fa1-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="45fa1-116">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="45fa1-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="45fa1-117">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="45fa1-117">Select the **Save** button.</span></span>

<span data-ttu-id="45fa1-118">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="45fa1-118">Record the following information:</span></span>

* <span data-ttu-id="45fa1-119">應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）</span><span class="sxs-lookup"><span data-stu-id="45fa1-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="45fa1-120">目錄識別碼（租使用者識別碼）（例如 `22222222-2222-2222-2222-222222222222`）</span><span class="sxs-lookup"><span data-stu-id="45fa1-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="45fa1-121">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="45fa1-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="45fa1-122">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="45fa1-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="45fa1-123">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="45fa1-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="45fa1-124">驗證套件</span><span class="sxs-lookup"><span data-stu-id="45fa1-124">Authentication package</span></span>

<span data-ttu-id="45fa1-125">當您建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。</span><span class="sxs-lookup"><span data-stu-id="45fa1-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="45fa1-126">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="45fa1-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="45fa1-127">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="45fa1-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="45fa1-128">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="45fa1-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="45fa1-129">`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="45fa1-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="45fa1-130">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="45fa1-130">Authentication service support</span></span>

<span data-ttu-id="45fa1-131">驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="45fa1-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="45fa1-132">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="45fa1-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="45fa1-133">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="45fa1-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="45fa1-134">`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="45fa1-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="45fa1-135">當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="45fa1-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="45fa1-136">Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="45fa1-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="45fa1-137">若要布建權杖做為登入流程的一部分，請將範圍新增至 `MsalProviderOptions`的預設存取權杖範圍：</span><span class="sxs-lookup"><span data-stu-id="45fa1-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="45fa1-138">預設存取權杖範圍的格式必須是 `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` （例如，`11111111-1111-1111-1111-111111111111/API.Access`）。</span><span class="sxs-lookup"><span data-stu-id="45fa1-138">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="45fa1-139">如果將配置或配置和主機提供給範圍設定（如 Azure 入口網站中所示），則*用戶端應用程式*會在收到來自*伺服器 API 應用程式*的*401 未經授權*回應時，擲回未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="45fa1-139">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

## <a name="index-page"></a><span data-ttu-id="45fa1-140">索引頁面</span><span class="sxs-lookup"><span data-stu-id="45fa1-140">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="45fa1-141">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="45fa1-141">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="45fa1-142">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="45fa1-142">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="45fa1-143">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="45fa1-143">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="45fa1-144">驗證元件</span><span class="sxs-lookup"><span data-stu-id="45fa1-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="45fa1-145">其他資源</span><span class="sxs-lookup"><span data-stu-id="45fa1-145">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
