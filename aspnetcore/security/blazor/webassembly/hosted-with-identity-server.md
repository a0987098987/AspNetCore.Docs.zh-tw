---
title: 使用識別伺服器保護BlazorASP.NET核心 Web 元件託管應用
author: guardrex
description: 使用[識別伺服器](https://identityserver.io/)Blazor後端介面的 Visual Studio 中使用身份驗證建立新託管應用
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: 4c51200159ced16132e15bb4a1f0915ca0cf5945
ms.sourcegitcommit: c9d1208e86160615b2d914cce74a839ae41297a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "81791622"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="676d5-103">使用識別伺服器保護BlazorASP.NET核心 Web 元件託管應用</span><span class="sxs-lookup"><span data-stu-id="676d5-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="676d5-104">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="676d5-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="676d5-105">要在 Visual Blazor Studio 中創建新託管應用,使用[識別伺服器](https://identityserver.io/)對使用者和 API 呼叫進行身份驗證,</span><span class="sxs-lookup"><span data-stu-id="676d5-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="676d5-106">使用可視化工作室創建新的**BlazorWeb 組裝**應用。</span><span class="sxs-lookup"><span data-stu-id="676d5-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="676d5-107">如需詳細資訊，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="676d5-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="676d5-108">在「**創建新Blazor應用」** 對話框中,在 **「身份驗證**」部分中選擇 **「更改**」。</span><span class="sxs-lookup"><span data-stu-id="676d5-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="676d5-109">選擇**單個使用者帳戶**後跟 **「確定**」。</span><span class="sxs-lookup"><span data-stu-id="676d5-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="676d5-110">在 **「進階」** 部分選擇 **「ASP.NET 核心託管**複選框。</span><span class="sxs-lookup"><span data-stu-id="676d5-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="676d5-111">選取 [建立]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="676d5-111">Select the **Create** button.</span></span>

<span data-ttu-id="676d5-112">要在命令外殼中建立應用,請執行以下命令:</span><span class="sxs-lookup"><span data-stu-id="676d5-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="676d5-113">要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample`</span><span class="sxs-lookup"><span data-stu-id="676d5-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="676d5-114">資料夾名稱也將成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="676d5-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="676d5-115">伺服器應用設定</span><span class="sxs-lookup"><span data-stu-id="676d5-115">Server app configuration</span></span>

<span data-ttu-id="676d5-116">以下各節介紹在包含身份驗證支援時對項目的補充。</span><span class="sxs-lookup"><span data-stu-id="676d5-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="676d5-117">啟始類別</span><span class="sxs-lookup"><span data-stu-id="676d5-117">Startup class</span></span>

<span data-ttu-id="676d5-118">這個`Startup`類別的功能:</span><span class="sxs-lookup"><span data-stu-id="676d5-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="676d5-119">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="676d5-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="676d5-120">身分識別：</span><span class="sxs-lookup"><span data-stu-id="676d5-120">Identity:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="676d5-121">識別伺服器具有附加<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>幫助器方法,在標識伺服器頂部設置一些預設ASP.NET核心約定:</span><span class="sxs-lookup"><span data-stu-id="676d5-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="676d5-122">使用其他<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>說明程式方法進行身份驗證,該方法將應用配置為驗證 IdentityServer 產生的 JWT 權杖:</span><span class="sxs-lookup"><span data-stu-id="676d5-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="676d5-123">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="676d5-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="676d5-124">負責驗證要求認證並在請求內容中設定使用者的身份驗證中間件:</span><span class="sxs-lookup"><span data-stu-id="676d5-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="676d5-125">公開開啟 ID 連線 (OIDC) 終結點的識別伺服器的中間件:</span><span class="sxs-lookup"><span data-stu-id="676d5-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="676d5-126">新增 Api 授權</span><span class="sxs-lookup"><span data-stu-id="676d5-126">AddApiAuthorization</span></span>

<span data-ttu-id="676d5-127"><xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>說明器方法為ASP.NET核心方案設定[識別伺服器](https://identityserver.io/)。</span><span class="sxs-lookup"><span data-stu-id="676d5-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="676d5-128">IdentityServer 是一個功能強大且可擴展的框架,用於處理應用安全問題。</span><span class="sxs-lookup"><span data-stu-id="676d5-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="676d5-129">標識伺服器會為最常見的方案公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="676d5-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="676d5-130">因此,提供了一組約定和配置選項,我們認為這是一個良好的起點。</span><span class="sxs-lookup"><span data-stu-id="676d5-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="676d5-131">一旦身份驗證需要更改,身份伺服器的全部功能仍可用於自定義身份驗證以滿足應用的要求。</span><span class="sxs-lookup"><span data-stu-id="676d5-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="676d5-132">新增身份伺服器Jwt</span><span class="sxs-lookup"><span data-stu-id="676d5-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="676d5-133"><xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>説明程式方法將應用的策略方案配置為預設身份驗證處理程式。</span><span class="sxs-lookup"><span data-stu-id="676d5-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="676d5-134">該策略設定為允許標識處理路由到標識 URL`/Identity`空間中的任何子路徑的所有請求。</span><span class="sxs-lookup"><span data-stu-id="676d5-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="676d5-135">處理<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>所有其他請求。</span><span class="sxs-lookup"><span data-stu-id="676d5-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="676d5-136">此外,此方法:</span><span class="sxs-lookup"><span data-stu-id="676d5-136">Additionally, this method:</span></span>

* <span data-ttu-id="676d5-137">將`{APPLICATION NAME}API`API 資源註冊為預設作用`{APPLICATION NAME}API`域 的識別伺服器。</span><span class="sxs-lookup"><span data-stu-id="676d5-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="676d5-138">配置 JWT 承載權杖中間件以驗證 IdentityServer 為應用頒發的權杖。</span><span class="sxs-lookup"><span data-stu-id="676d5-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="676d5-139">天氣預報控制器</span><span class="sxs-lookup"><span data-stu-id="676d5-139">WeatherForecastController</span></span>

<span data-ttu-id="676d5-140">在`WeatherForecastController`(*控制器/天氣預報控制器.cs*)[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)中,該屬性應用於類。</span><span class="sxs-lookup"><span data-stu-id="676d5-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="676d5-141">該屬性指示必須根據預設策略授權使用者才能訪問資源。</span><span class="sxs-lookup"><span data-stu-id="676d5-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="676d5-142">默認授權策略配置為使用預設身份驗證方案,該方案由<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>前面提到的策略方案設置。</span><span class="sxs-lookup"><span data-stu-id="676d5-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="676d5-143">説明程式方法<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>配置為對應用的請求的默認處理程式。</span><span class="sxs-lookup"><span data-stu-id="676d5-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="676d5-144">應用程式資料庫上下文</span><span class="sxs-lookup"><span data-stu-id="676d5-144">ApplicationDbContext</span></span>

<span data-ttu-id="676d5-145">在`ApplicationDbContext`(*資料/ 應用程式 DbContext.cs)* 中<xref:Microsoft.EntityFrameworkCore.DbContext>,識別中<xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601>使用相同的,但擴展 為包括識別伺服器的架構除外。</span><span class="sxs-lookup"><span data-stu-id="676d5-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="676d5-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。</span><span class="sxs-lookup"><span data-stu-id="676d5-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="676d5-147">要完全控制資料庫架構,請從其中一個可用的標識<xref:Microsoft.EntityFrameworkCore.DbContext>類繼承,並通過調`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)``OnModelCreating`用 方法將上下文配置為包括標識架構。</span><span class="sxs-lookup"><span data-stu-id="676d5-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="676d5-148">Oidc 設定控制器</span><span class="sxs-lookup"><span data-stu-id="676d5-148">OidcConfigurationController</span></span>

<span data-ttu-id="676d5-149">在`OidcConfigurationController`(*控制器/Oidc配置控制器.cs)* 中,用戶端終結點被預配以服務 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="676d5-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="676d5-150">套用設定檔</span><span class="sxs-lookup"><span data-stu-id="676d5-150">App settings files</span></span>

<span data-ttu-id="676d5-151">在專案根部的應用設定檔 (*appsettings.json*)`IdentityServer`中,該部分描述已配置的用戶端的清單。</span><span class="sxs-lookup"><span data-stu-id="676d5-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="676d5-152">在下面的示例中,只有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="676d5-152">In the following example, there's a single client.</span></span> <span data-ttu-id="676d5-153">用戶端名稱對應於應用名稱,並通過約定映射到 OAuth`ClientId`參數。</span><span class="sxs-lookup"><span data-stu-id="676d5-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="676d5-154">設定檔指示正在配置的應用類型。</span><span class="sxs-lookup"><span data-stu-id="676d5-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="676d5-155">配置檔在內部用於驅動簡化伺服器配置過程的約定。</span><span class="sxs-lookup"><span data-stu-id="676d5-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="676d5-156">在開發環境應用設定檔中(*應用設定)。在專案根目錄的開發.json*`IdentityServer`中 ,該部分描述了用於對權杖進行簽名的鍵。</span><span class="sxs-lookup"><span data-stu-id="676d5-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="676d5-157">用戶端應用設定</span><span class="sxs-lookup"><span data-stu-id="676d5-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="676d5-158">驗證驗證</span><span class="sxs-lookup"><span data-stu-id="676d5-158">Authentication package</span></span>

<span data-ttu-id="676d5-159">創建應用以使用個人使用者帳戶 ()`Individual`時,應用會自動在應用的專案檔中`Microsoft.AspNetCore.Components.WebAssembly.Authentication`接收 包的包引用。</span><span class="sxs-lookup"><span data-stu-id="676d5-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="676d5-160">該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。</span><span class="sxs-lookup"><span data-stu-id="676d5-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="676d5-161">如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:</span><span class="sxs-lookup"><span data-stu-id="676d5-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="676d5-162">在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。</span><span class="sxs-lookup"><span data-stu-id="676d5-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="676d5-163">API 授權支援</span><span class="sxs-lookup"><span data-stu-id="676d5-163">API authorization support</span></span>

<span data-ttu-id="676d5-164">通過`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包內提供的擴充方法將使用者身份驗證支援插入服務容器。</span><span class="sxs-lookup"><span data-stu-id="676d5-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="676d5-165">此方法設置應用與現有授權系統交互所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="676d5-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="676d5-166">默認情況下,它按約定從`_configuration/{client-id}`載入應用的配置。</span><span class="sxs-lookup"><span data-stu-id="676d5-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="676d5-167">按照慣例,客戶端 ID 設置為應用的程式集名稱。</span><span class="sxs-lookup"><span data-stu-id="676d5-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="676d5-168">可以通過使用選項調用重載來更改此 URL 以指向單獨的終結點。</span><span class="sxs-lookup"><span data-stu-id="676d5-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="imports-file"></a><span data-ttu-id="676d5-169">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="676d5-169">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="676d5-170">索引頁面</span><span class="sxs-lookup"><span data-stu-id="676d5-170">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="676d5-171">套用元件</span><span class="sxs-lookup"><span data-stu-id="676d5-171">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="676d5-172">重定到登入元件</span><span class="sxs-lookup"><span data-stu-id="676d5-172">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="676d5-173">登入元件</span><span class="sxs-lookup"><span data-stu-id="676d5-173">LoginDisplay component</span></span>

<span data-ttu-id="676d5-174">元件`LoginDisplay`(*共用 / LoginDisplay.razor*) 呈現在`MainLayout`元件中 ( 共用 /*MainLayout.razor*) 並管理以下行為:</span><span class="sxs-lookup"><span data-stu-id="676d5-174">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="676d5-175">對於經過身份驗證的使用者:</span><span class="sxs-lookup"><span data-stu-id="676d5-175">For authenticated users:</span></span>
  * <span data-ttu-id="676d5-176">顯示當前使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="676d5-176">Displays the current user name.</span></span>
  * <span data-ttu-id="676d5-177">提供指向ASP.NET核心標識中的使用者配置檔頁的連結。</span><span class="sxs-lookup"><span data-stu-id="676d5-177">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="676d5-178">提供一個按鈕以註銷應用程式。</span><span class="sxs-lookup"><span data-stu-id="676d5-178">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="676d5-179">對匿名使用者:</span><span class="sxs-lookup"><span data-stu-id="676d5-179">For anonymous users:</span></span>
  * <span data-ttu-id="676d5-180">提供註冊選項。</span><span class="sxs-lookup"><span data-stu-id="676d5-180">Offers the option to register.</span></span>
  * <span data-ttu-id="676d5-181">提供登錄選項。</span><span class="sxs-lookup"><span data-stu-id="676d5-181">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="676d5-182">驗證元件</span><span class="sxs-lookup"><span data-stu-id="676d5-182">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="676d5-183">擷取資料元件</span><span class="sxs-lookup"><span data-stu-id="676d5-183">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="676d5-184">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="676d5-184">Run the app</span></span>

<span data-ttu-id="676d5-185">從「伺服器」專案運行應用。</span><span class="sxs-lookup"><span data-stu-id="676d5-185">Run the app from the Server project.</span></span> <span data-ttu-id="676d5-186">使用 Visual Studio 時,在**解決方案資源管理器**中選擇「伺服器」專案,然後選擇工具列中的 **「運行」** 按鈕,或從 **「調試」** 選單啟動應用。</span><span class="sxs-lookup"><span data-stu-id="676d5-186">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="676d5-187">其他資源</span><span class="sxs-lookup"><span data-stu-id="676d5-187">Additional resources</span></span>

* [<span data-ttu-id="676d5-188">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="676d5-188">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
