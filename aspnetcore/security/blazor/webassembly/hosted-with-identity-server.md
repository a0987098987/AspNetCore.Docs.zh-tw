---
title: 使用身分識別伺服器保護 ASP.NET Core Blazor WebAssembly 託管應用程式
author: guardrex
description: 從使用[IdentityServer](https://identityserver.io/)後端的 Visual Studio 內，建立具有驗證的新 Blazor 託管應用程式
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: 6c7942a827d88a620e6f295af3f523c23f4b3890
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219047"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="f486c-103">使用身分識別伺服器保護 ASP.NET Core Blazor WebAssembly 託管應用程式</span><span class="sxs-lookup"><span data-stu-id="f486c-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="f486c-104">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f486c-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="f486c-105">若要在 Visual Studio 中建立新的 Blazor 託管應用程式，以使用[IdentityServer](https://identityserver.io/)來驗證使用者和 API 呼叫：</span><span class="sxs-lookup"><span data-stu-id="f486c-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="f486c-106">使用 Visual Studio 建立新的 **Blazor WebAssembly**應用程式。</span><span class="sxs-lookup"><span data-stu-id="f486c-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="f486c-107">如需詳細資訊，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="f486c-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="f486c-108">在 [**建立新的 Blazor 應用程式**] 對話方塊中，選取 [**驗證**] 區段中的 [**變更**]。</span><span class="sxs-lookup"><span data-stu-id="f486c-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="f486c-109">選取 [**個別使用者帳戶**]，後面接著 **[確定]** 。</span><span class="sxs-lookup"><span data-stu-id="f486c-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="f486c-110">選取 [ **Advanced** ] 區段中的 [ **ASP.NET Core 託管**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="f486c-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="f486c-111">選取 [建立] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f486c-111">Select the **Create** button.</span></span>

<span data-ttu-id="f486c-112">若要在命令 shell 中建立應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f486c-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="f486c-113">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。</span><span class="sxs-lookup"><span data-stu-id="f486c-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="f486c-114">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="f486c-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="f486c-115">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="f486c-115">Server app configuration</span></span>

<span data-ttu-id="f486c-116">下列各節說明當包含驗證支援時，專案的新增功能。</span><span class="sxs-lookup"><span data-stu-id="f486c-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="f486c-117">啟始類別</span><span class="sxs-lookup"><span data-stu-id="f486c-117">Startup class</span></span>

<span data-ttu-id="f486c-118">`Startup` 類別具有下列新增專案：</span><span class="sxs-lookup"><span data-stu-id="f486c-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="f486c-119">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="f486c-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="f486c-120">使用預設 UI 的身分識別：</span><span class="sxs-lookup"><span data-stu-id="f486c-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="f486c-121">IdentityServer 有額外的 <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper 方法，會在 IdentityServer 上設定一些預設的 ASP.NET Core 慣例：</span><span class="sxs-lookup"><span data-stu-id="f486c-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="f486c-122">使用額外的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper 方法進行驗證，以設定應用程式來驗證 IdentityServer 所產生的 JWT 權杖：</span><span class="sxs-lookup"><span data-stu-id="f486c-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="f486c-123">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="f486c-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="f486c-124">驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：</span><span class="sxs-lookup"><span data-stu-id="f486c-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="f486c-125">公開 Open ID Connect （OIDC）端點的 IdentityServer 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="f486c-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="f486c-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="f486c-126">AddApiAuthorization</span></span>

<span data-ttu-id="f486c-127"><xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper 方法會設定 ASP.NET Core 案例的[IdentityServer](https://identityserver.io/) 。</span><span class="sxs-lookup"><span data-stu-id="f486c-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="f486c-128">IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="f486c-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="f486c-129">IdentityServer 會在最常見的案例中公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="f486c-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="f486c-130">因此，會提供一組慣例和設定選項，讓我們考慮一個良好的起點。</span><span class="sxs-lookup"><span data-stu-id="f486c-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="f486c-131">一旦您的驗證需要變更，IdentityServer 的完整功能仍然可以自訂驗證以符合應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="f486c-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="f486c-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="f486c-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="f486c-133"><xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper 方法會將應用程式的原則配置設定為預設驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="f486c-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="f486c-134">此原則設定為允許身分識別處理路由傳送至身分識別 URL 空間 `/Identity`中任何子路徑的所有要求。</span><span class="sxs-lookup"><span data-stu-id="f486c-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="f486c-135"><xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 會處理所有的其他要求。</span><span class="sxs-lookup"><span data-stu-id="f486c-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="f486c-136">此外，這個方法也會：</span><span class="sxs-lookup"><span data-stu-id="f486c-136">Additionally, this method:</span></span>

* <span data-ttu-id="f486c-137">使用預設範圍為 `{APPLICATION NAME}API`的 IdentityServer 來註冊 `{APPLICATION NAME}API` API 資源。</span><span class="sxs-lookup"><span data-stu-id="f486c-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="f486c-138">設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="f486c-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="f486c-139">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="f486c-139">WeatherForecastController</span></span>

<span data-ttu-id="f486c-140">在 `WeatherForecastController` （*控制器/WeatherForecastController*）中， [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性會套用至類別。</span><span class="sxs-lookup"><span data-stu-id="f486c-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="f486c-141">屬性會指出使用者必須根據預設原則來存取資源。</span><span class="sxs-lookup"><span data-stu-id="f486c-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="f486c-142">預設的授權原則會設定為使用預設的驗證配置，這是由 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 到稍早所述的原則配置。</span><span class="sxs-lookup"><span data-stu-id="f486c-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="f486c-143">Helper 方法會將 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 設定為應用程式要求的預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="f486c-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="f486c-144">[ApplicationdbcoNtext]</span><span class="sxs-lookup"><span data-stu-id="f486c-144">ApplicationDbContext</span></span>

<span data-ttu-id="f486c-145">在 `ApplicationDbContext` （*Data/[applicationdbcoNtext] .cs*）中，相同的 <xref:Microsoft.EntityFrameworkCore.DbContext> 會在識別中用於擴充 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 以包含 IdentityServer 的架構的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f486c-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="f486c-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。</span><span class="sxs-lookup"><span data-stu-id="f486c-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="f486c-147">若要取得資料庫架構的完整控制權，請從 <xref:Microsoft.EntityFrameworkCore.DbContext> 類別的其中一個可用身分識別繼承，並設定內容以包含身分識別架構，方法是在 `OnModelCreating` 方法中呼叫 `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`。</span><span class="sxs-lookup"><span data-stu-id="f486c-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="f486c-148">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="f486c-148">OidcConfigurationController</span></span>

<span data-ttu-id="f486c-149">在 `OidcConfigurationController` （*控制器/OidcConfigurationController*）中，會布建用戶端端點以提供 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="f486c-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="f486c-150">應用程式佈建檔案</span><span class="sxs-lookup"><span data-stu-id="f486c-150">App settings files</span></span>

<span data-ttu-id="f486c-151">在專案根目錄的應用程式佈建檔案（*appsettings*）中，[`IdentityServer`] 區段會說明已設定的用戶端清單。</span><span class="sxs-lookup"><span data-stu-id="f486c-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="f486c-152">在下列範例中，有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="f486c-152">In the following example, there's a single client.</span></span> <span data-ttu-id="f486c-153">用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId` 參數。</span><span class="sxs-lookup"><span data-stu-id="f486c-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="f486c-154">此設定檔會指出正在設定的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="f486c-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="f486c-155">此設定檔會在內部使用，以驅動可簡化伺服器設定程式的慣例。</span><span class="sxs-lookup"><span data-stu-id="f486c-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="f486c-156">在開發環境應用程式佈建檔案（*appsettings 中。開發. json*），在專案根目錄中，`IdentityServer` 區段會描述用來簽署權杖的金鑰。</span><span class="sxs-lookup"><span data-stu-id="f486c-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="f486c-157">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="f486c-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="f486c-158">驗證套件</span><span class="sxs-lookup"><span data-stu-id="f486c-158">Authentication package</span></span>

<span data-ttu-id="f486c-159">建立應用程式以使用個別使用者帳戶（`Individual`）時，應用程式會在應用程式的專案檔中自動接收 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="f486c-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="f486c-160">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="f486c-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="f486c-161">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="f486c-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="f486c-162">以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。</span><span class="sxs-lookup"><span data-stu-id="f486c-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="f486c-163">API 授權支援</span><span class="sxs-lookup"><span data-stu-id="f486c-163">API authorization support</span></span>

<span data-ttu-id="f486c-164">驗證使用者的支援是由 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件內提供的擴充方法插入服務容器中。</span><span class="sxs-lookup"><span data-stu-id="f486c-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="f486c-165">這個方法會設定應用程式與現有授權系統互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="f486c-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="f486c-166">根據預設，它會依照慣例從 `_configuration/{client-id}`載入應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="f486c-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="f486c-167">依照慣例，用戶端識別碼會設定為應用程式的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="f486c-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="f486c-168">您可以使用選項呼叫多載，將此 URL 變更為指向不同的端點。</span><span class="sxs-lookup"><span data-stu-id="f486c-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="index-page"></a><span data-ttu-id="f486c-169">索引頁面</span><span class="sxs-lookup"><span data-stu-id="f486c-169">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="f486c-170">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="f486c-170">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="f486c-171">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="f486c-171">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="f486c-172">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="f486c-172">LoginDisplay component</span></span>

<span data-ttu-id="f486c-173">`LoginDisplay` 元件（*shared/LoginDisplay*）會在 `MainLayout` 元件（*shared/MainLayout*）中轉譯，並管理下列行為：</span><span class="sxs-lookup"><span data-stu-id="f486c-173">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="f486c-174">針對已驗證的使用者：</span><span class="sxs-lookup"><span data-stu-id="f486c-174">For authenticated users:</span></span>
  * <span data-ttu-id="f486c-175">顯示目前的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="f486c-175">Displays the current user name.</span></span>
  * <span data-ttu-id="f486c-176">提供 ASP.NET Core 身分識別中 [使用者設定檔] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="f486c-176">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="f486c-177">提供用來登出應用程式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f486c-177">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="f486c-178">匿名使用者：</span><span class="sxs-lookup"><span data-stu-id="f486c-178">For anonymous users:</span></span>
  * <span data-ttu-id="f486c-179">提供註冊的選項。</span><span class="sxs-lookup"><span data-stu-id="f486c-179">Offers the option to register.</span></span>
  * <span data-ttu-id="f486c-180">提供登入的選項。</span><span class="sxs-lookup"><span data-stu-id="f486c-180">Offers the option to log in.</span></span>

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

### <a name="authentication-component"></a><span data-ttu-id="f486c-181">驗證元件</span><span class="sxs-lookup"><span data-stu-id="f486c-181">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="f486c-182">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="f486c-182">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="f486c-183">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f486c-183">Run the app</span></span>

<span data-ttu-id="f486c-184">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f486c-184">Run the app from the Server project.</span></span> <span data-ttu-id="f486c-185">使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f486c-185">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
