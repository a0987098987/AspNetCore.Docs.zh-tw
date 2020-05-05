---
title: 使用Identity伺服器保護Blazor ASP.NET Core WebAssembly 託管應用程式
author: guardrex
description: 從使用 IdentityServer 後Blazor端的 Visual Studio 中，建立具有驗證的新[IdentityServer](https://identityserver.io/)託管應用程式
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: bf2298618e922df412e0742177afd390c4116388
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768115"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="5f13a-103">使用Identity伺服器保護Blazor ASP.NET Core WebAssembly 託管應用程式</span><span class="sxs-lookup"><span data-stu-id="5f13a-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="5f13a-104">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5f13a-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="5f13a-105">若要在 Visual Studio Blazor中建立新的託管應用程式，以使用[IdentityServer](https://identityserver.io/)來驗證使用者和 API 呼叫：</span><span class="sxs-lookup"><span data-stu-id="5f13a-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="5f13a-106">使用 Visual Studio 建立新\*\* Blazor的 WebAssembly\*\*應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f13a-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="5f13a-107">如需詳細資訊，請參閱<xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="5f13a-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="5f13a-108">在 [**建立新Blazor的應用程式**] 對話方塊中，選取 [**驗證**] 區段中的 [**變更**]。</span><span class="sxs-lookup"><span data-stu-id="5f13a-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="5f13a-109">選取 [**個別使用者帳戶**]，後面接著 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="5f13a-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="5f13a-110">選取 [ **Advanced** ] 區段中的 [ **ASP.NET Core 託管**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="5f13a-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="5f13a-111">選取 [建立]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5f13a-111">Select the **Create** button.</span></span>

<span data-ttu-id="5f13a-112">若要在命令 shell 中建立應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="5f13a-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="5f13a-113">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。</span><span class="sxs-lookup"><span data-stu-id="5f13a-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="5f13a-114">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="5f13a-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="5f13a-115">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="5f13a-115">Server app configuration</span></span>

<span data-ttu-id="5f13a-116">下列各節說明當包含驗證支援時，專案的新增功能。</span><span class="sxs-lookup"><span data-stu-id="5f13a-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="5f13a-117">啟始類別</span><span class="sxs-lookup"><span data-stu-id="5f13a-117">Startup class</span></span>

<span data-ttu-id="5f13a-118">`Startup`類別具有下列新增專案：</span><span class="sxs-lookup"><span data-stu-id="5f13a-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="5f13a-119">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="5f13a-119">In `Startup.ConfigureServices`:</span></span>

  * Identity<span data-ttu-id="5f13a-120">:</span><span class="sxs-lookup"><span data-stu-id="5f13a-120">:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>(options => 
            options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="5f13a-121">IdentityServer 使用額外<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>的協助程式方法，在 IdentityServer 上設定一些預設的 ASP.NET Core 慣例：</span><span class="sxs-lookup"><span data-stu-id="5f13a-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="5f13a-122">使用其他<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper 方法進行驗證，以設定應用程式來驗證 IdentityServer 所產生的 JWT 權杖：</span><span class="sxs-lookup"><span data-stu-id="5f13a-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="5f13a-123">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="5f13a-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="5f13a-124">驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：</span><span class="sxs-lookup"><span data-stu-id="5f13a-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="5f13a-125">公開 Open ID Connect （OIDC）端點的 IdentityServer 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="5f13a-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

  * <span data-ttu-id="5f13a-126">驗證和授權中介軟體：</span><span class="sxs-lookup"><span data-stu-id="5f13a-126">Authentication and Authorization Middleware:</span></span>

    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="5f13a-127">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="5f13a-127">AddApiAuthorization</span></span>

<span data-ttu-id="5f13a-128"><xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> Helper 方法會設定 ASP.NET Core 案例的[IdentityServer](https://identityserver.io/) 。</span><span class="sxs-lookup"><span data-stu-id="5f13a-128">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="5f13a-129">IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="5f13a-129">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="5f13a-130">IdentityServer 會在最常見的案例中公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="5f13a-130">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="5f13a-131">因此，會提供一組慣例和設定選項，讓我們考慮一個良好的起點。</span><span class="sxs-lookup"><span data-stu-id="5f13a-131">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="5f13a-132">一旦您的驗證需要變更，IdentityServer 的完整功能仍然可以自訂驗證以符合應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="5f13a-132">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="5f13a-133">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="5f13a-133">AddIdentityServerJwt</span></span>

<span data-ttu-id="5f13a-134"><xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> Helper 方法會將應用程式的原則配置設定為預設驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="5f13a-134">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="5f13a-135">此原則設定為允許Identity處理路由至Identity URL 空間`/Identity`中任何子路徑的所有要求。</span><span class="sxs-lookup"><span data-stu-id="5f13a-135">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="5f13a-136">會<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="5f13a-136">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="5f13a-137">此外，這個方法也會：</span><span class="sxs-lookup"><span data-stu-id="5f13a-137">Additionally, this method:</span></span>

* <span data-ttu-id="5f13a-138">向 IdentityServer `{APPLICATION NAME}API`註冊具有預設範圍的`{APPLICATION NAME}API`API 資源。</span><span class="sxs-lookup"><span data-stu-id="5f13a-138">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="5f13a-139">設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="5f13a-139">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="5f13a-140">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="5f13a-140">WeatherForecastController</span></span>

<span data-ttu-id="5f13a-141">在`WeatherForecastController` （*控制器/WeatherForecastController*）中， [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性會套用至類別。</span><span class="sxs-lookup"><span data-stu-id="5f13a-141">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="5f13a-142">屬性會指出使用者必須根據預設原則來存取資源。</span><span class="sxs-lookup"><span data-stu-id="5f13a-142">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="5f13a-143">預設的授權原則會設定為使用預設的驗證配置，這是由<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>設定為先前所述的原則配置。</span><span class="sxs-lookup"><span data-stu-id="5f13a-143">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="5f13a-144">Helper 方法會將<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>設定為應用程式要求的預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="5f13a-144">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="5f13a-145">[ApplicationdbcoNtext]</span><span class="sxs-lookup"><span data-stu-id="5f13a-145">ApplicationDbContext</span></span>

<span data-ttu-id="5f13a-146">`ApplicationDbContext`在（*Data/[applicationdbcoNtext]*）中，會使用<xref:Microsoft.EntityFrameworkCore.DbContext>相同的， Identity因為它會擴充<xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601>以包含 IdentityServer 的架構。</span><span class="sxs-lookup"><span data-stu-id="5f13a-146">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="5f13a-147"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。</span><span class="sxs-lookup"><span data-stu-id="5f13a-147"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="5f13a-148">若要取得資料庫架構的完整控制權，請從Identity <xref:Microsoft.EntityFrameworkCore.DbContext>其中一個可用的類別繼承，並透過在Identity `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` `OnModelCreating`方法中呼叫來設定內容以包含架構。</span><span class="sxs-lookup"><span data-stu-id="5f13a-148">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="5f13a-149">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="5f13a-149">OidcConfigurationController</span></span>

<span data-ttu-id="5f13a-150">在`OidcConfigurationController` （*控制器/OidcConfigurationController*）中，會布建用戶端端點來提供 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="5f13a-150">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="5f13a-151">應用程式佈建檔案</span><span class="sxs-lookup"><span data-stu-id="5f13a-151">App settings files</span></span>

<span data-ttu-id="5f13a-152">在專案根目錄的應用程式佈建檔案（*appsettings*）中， `IdentityServer`區段會描述已設定的用戶端清單。</span><span class="sxs-lookup"><span data-stu-id="5f13a-152">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="5f13a-153">在下列範例中，有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="5f13a-153">In the following example, there's a single client.</span></span> <span data-ttu-id="5f13a-154">用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId`參數。</span><span class="sxs-lookup"><span data-stu-id="5f13a-154">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="5f13a-155">此設定檔會指出正在設定的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="5f13a-155">The profile indicates the app type being configured.</span></span> <span data-ttu-id="5f13a-156">此設定檔會在內部使用，以驅動可簡化伺服器設定程式的慣例。</span><span class="sxs-lookup"><span data-stu-id="5f13a-156">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "{APP ASSEMBLY}.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="5f13a-157">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="5f13a-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="5f13a-158">驗證套件</span><span class="sxs-lookup"><span data-stu-id="5f13a-158">Authentication package</span></span>

<span data-ttu-id="5f13a-159">建立應用程式以使用個別使用者帳戶（`Individual`）時，應用程式會在應用程式的專案檔中`Microsoft.AspNetCore.Components.WebAssembly.Authentication`自動接收套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="5f13a-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="5f13a-160">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="5f13a-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="5f13a-161">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="5f13a-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="5f13a-162">將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。</span><span class="sxs-lookup"><span data-stu-id="5f13a-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="5f13a-163">API 授權支援</span><span class="sxs-lookup"><span data-stu-id="5f13a-163">API authorization support</span></span>

<span data-ttu-id="5f13a-164">驗證使用者的支援是由`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件內提供的擴充方法插入服務容器中。</span><span class="sxs-lookup"><span data-stu-id="5f13a-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="5f13a-165">這個方法會設定應用程式與現有授權系統互動所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="5f13a-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="5f13a-166">根據預設，它會從`_configuration/{client-id}`依照慣例載入應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="5f13a-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="5f13a-167">依照慣例，用戶端識別碼會設定為應用程式的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="5f13a-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="5f13a-168">您可以使用選項呼叫多載，將此 URL 變更為指向不同的端點。</span><span class="sxs-lookup"><span data-stu-id="5f13a-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="imports-file"></a><span data-ttu-id="5f13a-169">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="5f13a-169">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="5f13a-170">索引頁面</span><span class="sxs-lookup"><span data-stu-id="5f13a-170">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="5f13a-171">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="5f13a-171">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="5f13a-172">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="5f13a-172">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="5f13a-173">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="5f13a-173">LoginDisplay component</span></span>

<span data-ttu-id="5f13a-174">元件（*shared/LoginDisplay* `MainLayout` ）會在元件（*shared/MainLayout*）中轉譯，並管理下列行為： `LoginDisplay`</span><span class="sxs-lookup"><span data-stu-id="5f13a-174">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="5f13a-175">針對已驗證的使用者：</span><span class="sxs-lookup"><span data-stu-id="5f13a-175">For authenticated users:</span></span>
  * <span data-ttu-id="5f13a-176">顯示目前的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="5f13a-176">Displays the current user name.</span></span>
  * <span data-ttu-id="5f13a-177">提供 ASP.NET Core Identity中 [使用者設定檔] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="5f13a-177">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="5f13a-178">提供用來登出應用程式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="5f13a-178">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="5f13a-179">匿名使用者：</span><span class="sxs-lookup"><span data-stu-id="5f13a-179">For anonymous users:</span></span>
  * <span data-ttu-id="5f13a-180">提供註冊的選項。</span><span class="sxs-lookup"><span data-stu-id="5f13a-180">Offers the option to register.</span></span>
  * <span data-ttu-id="5f13a-181">提供登入的選項。</span><span class="sxs-lookup"><span data-stu-id="5f13a-181">Offers the option to log in.</span></span>

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

### <a name="authentication-component"></a><span data-ttu-id="5f13a-182">驗證元件</span><span class="sxs-lookup"><span data-stu-id="5f13a-182">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="5f13a-183">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="5f13a-183">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="5f13a-184">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="5f13a-184">Run the app</span></span>

<span data-ttu-id="5f13a-185">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f13a-185">Run the app from the Server project.</span></span> <span data-ttu-id="5f13a-186">使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f13a-186">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="5f13a-187">其他資源</span><span class="sxs-lookup"><span data-stu-id="5f13a-187">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
