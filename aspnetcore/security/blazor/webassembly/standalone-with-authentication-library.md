---
title: 使用驗證程式庫保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: f9cc2884dcd94c729c45a056ae4327a2c75d34be
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083752"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="2c8f2-102">使用驗證程式庫保護 ASP.NET Core Blazor WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="2c8f2-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="2c8f2-103">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2c8f2-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="2c8f2-104">若要建立使用 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫的 Blazor WebAssembly 獨立應用程式，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2c8f2-104">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="2c8f2-105">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-105">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="2c8f2-106">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-106">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="2c8f2-107">在 Visual Studio 中，[建立 Blazor WebAssembly 應用程式](xref:blazor/get-started)。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-107">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="2c8f2-108">使用 [**儲存使用者帳戶應用程式內**] 選項，將**驗證**設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-108">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="2c8f2-109">驗證套件</span><span class="sxs-lookup"><span data-stu-id="2c8f2-109">Authentication package</span></span>

<span data-ttu-id="2c8f2-110">建立應用程式以使用個別使用者帳戶時，應用程式會在應用程式的專案檔中自動接收 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-110">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="2c8f2-111">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-111">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="2c8f2-112">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="2c8f2-112">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="2c8f2-113">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-113">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="2c8f2-114">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="2c8f2-114">Authentication service support</span></span>

<span data-ttu-id="2c8f2-115">驗證使用者的支援是以 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件所提供的 `AddOidcAuthentication` 擴充方法，在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-115">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="2c8f2-116">這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-116">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="2c8f2-117">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="2c8f2-117">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="2c8f2-118">獨立應用程式的驗證支援是使用 Open ID Connect （OIDC）提供。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-118">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="2c8f2-119">`AddOidcAuthentication` 方法會接受回呼，以使用 OIDC 來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-119">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="2c8f2-120">設定應用程式所需的值可以從 IP 取得，例如 Google、Microsoft 或其他 OIDC 相容的提供者。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-120">The values required for configuring the app can be obtained from the IP, such as Google, Microsoft, or other OIDC-compliant provider.</span></span> <span data-ttu-id="2c8f2-121">當您註冊應用程式時，請取得這些值，這通常會發生在其線上入口網站中。</span><span class="sxs-lookup"><span data-stu-id="2c8f2-121">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="index-page"></a><span data-ttu-id="2c8f2-122">索引頁面</span><span class="sxs-lookup"><span data-stu-id="2c8f2-122">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="2c8f2-123">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="2c8f2-123">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="2c8f2-124">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="2c8f2-124">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="2c8f2-125">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="2c8f2-125">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="2c8f2-126">驗證元件</span><span class="sxs-lookup"><span data-stu-id="2c8f2-126">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
