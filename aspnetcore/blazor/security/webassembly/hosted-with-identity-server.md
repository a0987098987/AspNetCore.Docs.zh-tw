---
title: Blazor使用伺服器保護 ASP.NET Core WebAssembly 託管應用程式 Identity
author: guardrex
description: Blazor從使用[IdentityServer](https://identityserver.io/)後端的 Visual Studio 中，建立具有驗證的新託管應用程式
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/hosted-with-identity-server
ms.openlocfilehash: 8251658a2bb99ec17424ec26b8a44171082fab05
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243462"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="eccee-103">Blazor使用伺服器保護 ASP.NET Core WebAssembly 託管應用程式 Identity</span><span class="sxs-lookup"><span data-stu-id="eccee-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="eccee-104">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="eccee-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="eccee-105">本文說明如何建立新的 Blazor 託管應用程式，以使用[IdentityServer](https://identityserver.io/)來驗證使用者和 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="eccee-105">This article explains how to create a new Blazor hosted app that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="eccee-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eccee-106">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eccee-107">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="eccee-107">In Visual Studio:</span></span>

1. <span data-ttu-id="eccee-108">建立新的\*\* Blazor WebAssembly\*\*應用程式。</span><span class="sxs-lookup"><span data-stu-id="eccee-108">Create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="eccee-109">如需詳細資訊，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="eccee-109">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="eccee-110">在 [**建立新的 Blazor 應用程式**] 對話方塊中，選取 [**驗證**] 區段中的 [**變更**]。</span><span class="sxs-lookup"><span data-stu-id="eccee-110">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="eccee-111">選取 [**個別使用者帳戶**]，後面接著 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="eccee-111">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="eccee-112">選取 [ **Advanced** ] 區段中的 [ **ASP.NET Core 託管**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="eccee-112">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="eccee-113">選取 [建立] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="eccee-113">Select the **Create** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="eccee-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="eccee-114">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="eccee-115">若要在命令 shell 中建立應用程式，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="eccee-115">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="eccee-116">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。</span><span class="sxs-lookup"><span data-stu-id="eccee-116">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="eccee-117">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="eccee-117">The folder name also becomes part of the project's name.</span></span>

---

## <a name="server-app-configuration"></a><span data-ttu-id="eccee-118">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="eccee-118">Server app configuration</span></span>

<span data-ttu-id="eccee-119">下列各節說明當包含驗證支援時，專案的新增功能。</span><span class="sxs-lookup"><span data-stu-id="eccee-119">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="eccee-120">啟始類別</span><span class="sxs-lookup"><span data-stu-id="eccee-120">Startup class</span></span>

<span data-ttu-id="eccee-121">`Startup`類別具有下列新增專案。</span><span class="sxs-lookup"><span data-stu-id="eccee-121">The `Startup` class has the following additions.</span></span>

* <span data-ttu-id="eccee-122">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="eccee-122">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="eccee-123">ASP.NET Core Identity ：</span><span class="sxs-lookup"><span data-stu-id="eccee-123">ASP.NET Core Identity:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>(options => 
            options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="eccee-124">IdentityServer 使用額外 <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> 的協助程式方法，在 IdentityServer 上設定預設的 ASP.NET Core 慣例：</span><span class="sxs-lookup"><span data-stu-id="eccee-124">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="eccee-125">使用其他 helper 方法進行驗證，以設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 應用程式來驗證 IdentityServer 所產生的 JWT 權杖：</span><span class="sxs-lookup"><span data-stu-id="eccee-125">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="eccee-126">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="eccee-126">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="eccee-127">IdentityServer 中介軟體會公開 Open ID Connect （OIDC）端點：</span><span class="sxs-lookup"><span data-stu-id="eccee-127">The IdentityServer middleware exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

  * <span data-ttu-id="eccee-128">驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：</span><span class="sxs-lookup"><span data-stu-id="eccee-128">The Authentication middleware is responsible for validating request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="eccee-129">授權中介軟體可啟用授權功能：</span><span class="sxs-lookup"><span data-stu-id="eccee-129">Authorization Middleware enables authorization capabilities:</span></span>

    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="eccee-130">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="eccee-130">AddApiAuthorization</span></span>

<span data-ttu-id="eccee-131"><xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>Helper 方法會設定 ASP.NET Core 案例的[IdentityServer](https://identityserver.io/) 。</span><span class="sxs-lookup"><span data-stu-id="eccee-131">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="eccee-132">IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="eccee-132">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="eccee-133">IdentityServer 會在最常見的案例中公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="eccee-133">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="eccee-134">因此，會提供一組慣例和設定選項，讓我們考慮一個良好的起點。</span><span class="sxs-lookup"><span data-stu-id="eccee-134">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="eccee-135">一旦您的驗證需要變更，就可以使用 IdentityServer 的完整功能，自訂驗證以符合應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="eccee-135">Once your authentication needs change, the full power of IdentityServer is available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="eccee-136">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="eccee-136">AddIdentityServerJwt</span></span>

<span data-ttu-id="eccee-137"><xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>Helper 方法會將應用程式的原則配置設定為預設驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="eccee-137">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="eccee-138">此原則設定為允許 Identity 處理路由至 URL 空間中任何子路徑的所有要求 Identity `/Identity` 。</span><span class="sxs-lookup"><span data-stu-id="eccee-138">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="eccee-139">會 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="eccee-139">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="eccee-140">此外，這個方法也會：</span><span class="sxs-lookup"><span data-stu-id="eccee-140">Additionally, this method:</span></span>

* <span data-ttu-id="eccee-141">向 `{APPLICATION NAME}API` IdentityServer 註冊具有預設範圍的 API 資源 `{APPLICATION NAME}API` 。</span><span class="sxs-lookup"><span data-stu-id="eccee-141">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="eccee-142">設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="eccee-142">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="eccee-143">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="eccee-143">WeatherForecastController</span></span>

<span data-ttu-id="eccee-144">在 `WeatherForecastController` （ `Controllers/WeatherForecastController.cs` ）中， [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性會套用至類別。</span><span class="sxs-lookup"><span data-stu-id="eccee-144">In the `WeatherForecastController` (`Controllers/WeatherForecastController.cs`), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="eccee-145">屬性會指出使用者必須根據預設原則來存取資源。</span><span class="sxs-lookup"><span data-stu-id="eccee-145">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="eccee-146">預設的授權原則會設定為使用預設的驗證配置，這是由所設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 。</span><span class="sxs-lookup"><span data-stu-id="eccee-146">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>.</span></span> <span data-ttu-id="eccee-147">Helper 方法會將設定 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 為應用程式要求的預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="eccee-147">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="eccee-148">[ApplicationdbcoNtext]</span><span class="sxs-lookup"><span data-stu-id="eccee-148">ApplicationDbContext</span></span>

<span data-ttu-id="eccee-149">在 `ApplicationDbContext` （ `Data/ApplicationDbContext.cs` ）中，會 <xref:Microsoft.EntityFrameworkCore.DbContext> 擴充 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 以包含 IdentityServer 的架構。</span><span class="sxs-lookup"><span data-stu-id="eccee-149">In the `ApplicationDbContext` (`Data/ApplicationDbContext.cs`), <xref:Microsoft.EntityFrameworkCore.DbContext> extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="eccee-150"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。</span><span class="sxs-lookup"><span data-stu-id="eccee-150"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="eccee-151">若要取得資料庫架構的完整控制權，請從其中一個可用的類別繼承， Identity <xref:Microsoft.EntityFrameworkCore.DbContext> 並透過 Identity `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` 在方法中呼叫來設定內容以包含架構 <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> 。</span><span class="sxs-lookup"><span data-stu-id="eccee-151">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="eccee-152">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="eccee-152">OidcConfigurationController</span></span>

<span data-ttu-id="eccee-153">在 `OidcConfigurationController` （ `Controllers/OidcConfigurationController.cs` ）中，會布建用戶端端點來提供 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="eccee-153">In the `OidcConfigurationController` (`Controllers/OidcConfigurationController.cs`), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="eccee-154">應用程式佈建檔案</span><span class="sxs-lookup"><span data-stu-id="eccee-154">App settings files</span></span>

<span data-ttu-id="eccee-155">在專案根目錄的應用程式佈建檔案（ `appsettings.json` ）中， `IdentityServer` 區段會描述已設定的用戶端清單。</span><span class="sxs-lookup"><span data-stu-id="eccee-155">In the app settings file (`appsettings.json`) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="eccee-156">在下列範例中，有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="eccee-156">In the following example, there's a single client.</span></span> <span data-ttu-id="eccee-157">用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId` 參數。</span><span class="sxs-lookup"><span data-stu-id="eccee-157">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="eccee-158">此設定檔會指出正在設定的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="eccee-158">The profile indicates the app type being configured.</span></span> <span data-ttu-id="eccee-159">此設定檔會在內部使用，以驅動可簡化伺服器設定程式的慣例。</span><span class="sxs-lookup"><span data-stu-id="eccee-159">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "{APP ASSEMBLY}.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="eccee-160">預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱（例如， `BlazorSample.Client` ）。</span><span class="sxs-lookup"><span data-stu-id="eccee-160">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `BlazorSample.Client`).</span></span>

## <a name="client-app-configuration"></a><span data-ttu-id="eccee-161">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="eccee-161">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="eccee-162">驗證套件</span><span class="sxs-lookup"><span data-stu-id="eccee-162">Authentication package</span></span>

<span data-ttu-id="eccee-163">建立應用程式以使用個別使用者帳戶（）時 `Individual` ，應用程式會 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 在應用程式的專案檔中自動接收套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="eccee-163">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package in the app's project file.</span></span> <span data-ttu-id="eccee-164">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="eccee-164">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="eccee-165">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="eccee-165">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="3.2.0" />
```

### <a name="api-authorization-support"></a><span data-ttu-id="eccee-166">API 授權支援</span><span class="sxs-lookup"><span data-stu-id="eccee-166">API authorization support</span></span>

<span data-ttu-id="eccee-167">驗證使用者的支援是由套件內提供的擴充方法插入服務容器中 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 。</span><span class="sxs-lookup"><span data-stu-id="eccee-167">The support for authenticating users is plugged into the service container by the extension method provided inside the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package.</span></span> <span data-ttu-id="eccee-168">這個方法會設定應用程式所需的服務，以與現有的授權系統互動。</span><span class="sxs-lookup"><span data-stu-id="eccee-168">This method sets up the services required by the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="eccee-169">根據預設，應用程式的設定會依照慣例從載入 `_configuration/{client-id}` 。</span><span class="sxs-lookup"><span data-stu-id="eccee-169">By default, configuration for the app is loaded by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="eccee-170">依照慣例，用戶端識別碼會設定為應用程式的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="eccee-170">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="eccee-171">您可以使用選項呼叫多載，將此 URL 變更為指向不同的端點。</span><span class="sxs-lookup"><span data-stu-id="eccee-171">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="imports-file"></a><span data-ttu-id="eccee-172">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="eccee-172">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="eccee-173">索引頁面</span><span class="sxs-lookup"><span data-stu-id="eccee-173">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="eccee-174">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="eccee-174">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="eccee-175">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="eccee-175">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="eccee-176">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="eccee-176">LoginDisplay component</span></span>

<span data-ttu-id="eccee-177">`LoginDisplay`元件（ `Shared/LoginDisplay.razor` ）會在 `MainLayout` 元件（）中轉譯 `Shared/MainLayout.razor` ，並管理下列行為：</span><span class="sxs-lookup"><span data-stu-id="eccee-177">The `LoginDisplay` component (`Shared/LoginDisplay.razor`) is rendered in the `MainLayout` component (`Shared/MainLayout.razor`) and manages the following behaviors:</span></span>

* <span data-ttu-id="eccee-178">針對已驗證的使用者：</span><span class="sxs-lookup"><span data-stu-id="eccee-178">For authenticated users:</span></span>
  * <span data-ttu-id="eccee-179">顯示目前的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="eccee-179">Displays the current user name.</span></span>
  * <span data-ttu-id="eccee-180">提供 ASP.NET Core 中 [使用者設定檔] 頁面的連結 Identity 。</span><span class="sxs-lookup"><span data-stu-id="eccee-180">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="eccee-181">提供用來登出應用程式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="eccee-181">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="eccee-182">匿名使用者：</span><span class="sxs-lookup"><span data-stu-id="eccee-182">For anonymous users:</span></span>
  * <span data-ttu-id="eccee-183">提供註冊的選項。</span><span class="sxs-lookup"><span data-stu-id="eccee-183">Offers the option to register.</span></span>
  * <span data-ttu-id="eccee-184">提供登入的選項。</span><span class="sxs-lookup"><span data-stu-id="eccee-184">Offers the option to log in.</span></span>

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

### <a name="authentication-component"></a><span data-ttu-id="eccee-185">驗證元件</span><span class="sxs-lookup"><span data-stu-id="eccee-185">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="eccee-186">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="eccee-186">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="eccee-187">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="eccee-187">Run the app</span></span>

<span data-ttu-id="eccee-188">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="eccee-188">Run the app from the Server project.</span></span> <span data-ttu-id="eccee-189">使用 Visual Studio 時，您可以：</span><span class="sxs-lookup"><span data-stu-id="eccee-189">When using Visual Studio, either:</span></span>

* <span data-ttu-id="eccee-190">將工具列中的 [**啟始專案**] 下拉式清單設定為*伺服器 API 應用程式*，然後選取 [**執行**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="eccee-190">Set the **Startup Projects** drop down list in the toolbar to the *Server API app* and select the **Run** button.</span></span>
* <span data-ttu-id="eccee-191">在**方案總管**中選取伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="eccee-191">Select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

## <a name="name-and-role-claim-with-api-authorization"></a><span data-ttu-id="eccee-192">具有 API 授權的名稱和角色宣告</span><span class="sxs-lookup"><span data-stu-id="eccee-192">Name and role claim with API authorization</span></span>

### <a name="custom-user-factory"></a><span data-ttu-id="eccee-193">自訂使用者工廠</span><span class="sxs-lookup"><span data-stu-id="eccee-193">Custom user factory</span></span>

<span data-ttu-id="eccee-194">在用戶端應用程式中，建立自訂的使用者 factory。</span><span class="sxs-lookup"><span data-stu-id="eccee-194">In the Client app, create a custom user factory.</span></span> Identity<span data-ttu-id="eccee-195">伺服器會在單一宣告中以 JSON 陣列的形式傳送多個角色 `role` 。</span><span class="sxs-lookup"><span data-stu-id="eccee-195"> Server sends multiple roles as a JSON array in a single `role` claim.</span></span> <span data-ttu-id="eccee-196">單一角色會當做宣告中的字串值來傳送。</span><span class="sxs-lookup"><span data-stu-id="eccee-196">A single role is sent as a string value in the claim.</span></span> <span data-ttu-id="eccee-197">Factory 會 `role` 針對每個使用者角色建立個別的宣告。</span><span class="sxs-lookup"><span data-stu-id="eccee-197">The factory creates an individual `role` claim for each of the user's roles.</span></span>

<span data-ttu-id="eccee-198">`CustomUserFactory.cs`:</span><span class="sxs-lookup"><span data-stu-id="eccee-198">`CustomUserFactory.cs`:</span></span>

```csharp
using System.Linq;
using System.Security.Claims;
using System.Text.Json;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;

public class CustomUserFactory
    : AccountClaimsPrincipalFactory<RemoteUserAccount>
{
    public CustomUserFactory(IAccessTokenProviderAccessor accessor)
        : base(accessor)
    {
    }

    public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
        RemoteUserAccount account,
        RemoteAuthenticationUserOptions options)
    {
        var user = await base.CreateUserAsync(account, options);

        if (user.Identity.IsAuthenticated)
        {
            var identity = (ClaimsIdentity)user.Identity;
            var roleClaims = identity.FindAll(identity.RoleClaimType);

            if (roleClaims != null && roleClaims.Any())
            {
                foreach (var existingClaim in roleClaims)
                {
                    identity.RemoveClaim(existingClaim);
                }

                var rolesElem = account.AdditionalProperties[identity.RoleClaimType];

                if (rolesElem is JsonElement roles)
                {
                    if (roles.ValueKind == JsonValueKind.Array)
                    {
                        foreach (var role in roles.EnumerateArray())
                        {
                            identity.AddClaim(new Claim(options.RoleClaim, role.GetString()));
                        }
                    }
                    else
                    {
                        identity.AddClaim(new Claim(options.RoleClaim, roles.GetString()));
                    }
                }
            }
        }

        return user;
    }
}
```

<span data-ttu-id="eccee-199">在用戶端應用程式中，于（）中註冊 factory `Program.Main` `Program.cs` ：</span><span class="sxs-lookup"><span data-stu-id="eccee-199">In the Client app, register the factory in `Program.Main` (`Program.cs`):</span></span>

```csharp
builder.Services.AddApiAuthorization()
    .AddAccountClaimsPrincipalFactory<CustomUserFactory>();
```

<span data-ttu-id="eccee-200">在伺服器應用程式中，呼叫產生器 <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> 上的 Identity ，這會新增角色相關服務：</span><span class="sxs-lookup"><span data-stu-id="eccee-200">In the Server app, call <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> on the Identity builder, which adds role-related services:</span></span>

```csharp
using Microsoft.AspNetCore.Identity;

...

services.AddDefaultIdentity<ApplicationUser>(options => 
    options.SignIn.RequireConfirmedAccount = true)
    .AddRoles<IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="configure-identity-server"></a><span data-ttu-id="eccee-201">設定 Identity 伺服器</span><span class="sxs-lookup"><span data-stu-id="eccee-201">Configure Identity Server</span></span>

<span data-ttu-id="eccee-202">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="eccee-202">Use **one** of the following approaches:</span></span>

* [<span data-ttu-id="eccee-203">API 授權選項</span><span class="sxs-lookup"><span data-stu-id="eccee-203">API authorization options</span></span>](#api-authorization-options)
* [<span data-ttu-id="eccee-204">設定檔服務</span><span class="sxs-lookup"><span data-stu-id="eccee-204">Profile Service</span></span>](#profile-service)

#### <a name="api-authorization-options"></a><span data-ttu-id="eccee-205">API 授權選項</span><span class="sxs-lookup"><span data-stu-id="eccee-205">API authorization options</span></span>

<span data-ttu-id="eccee-206">在伺服器應用程式中：</span><span class="sxs-lookup"><span data-stu-id="eccee-206">In the Server app:</span></span>

* <span data-ttu-id="eccee-207">設定 Identity 伺服器將 `name` 和宣告放 `role` 入識別碼權杖和存取權杖中。</span><span class="sxs-lookup"><span data-stu-id="eccee-207">Configure Identity Server to put the `name` and `role` claims into the ID token and access token.</span></span>
* <span data-ttu-id="eccee-208">防止 JWT 權杖處理常式中的角色的預設對應。</span><span class="sxs-lookup"><span data-stu-id="eccee-208">Prevent the default mapping for roles in the JWT token handler.</span></span>

```csharp
using System.IdentityModel.Tokens.Jwt;
using System.Linq;

...

services.AddIdentityServer()
    .AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options => {
        options.IdentityResources["openid"].UserClaims.Add("name");
        options.ApiResources.Single().UserClaims.Add("name");
        options.IdentityResources["openid"].UserClaims.Add("role");
        options.ApiResources.Single().UserClaims.Add("role");
    });

JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Remove("role");
```

#### <a name="profile-service"></a><span data-ttu-id="eccee-209">設定檔服務</span><span class="sxs-lookup"><span data-stu-id="eccee-209">Profile Service</span></span>

<span data-ttu-id="eccee-210">在伺服器應用程式中，建立一個 `ProfileService` 執行。</span><span class="sxs-lookup"><span data-stu-id="eccee-210">In the Server app, create a `ProfileService` implementation.</span></span>

<span data-ttu-id="eccee-211">`ProfileService.cs`:</span><span class="sxs-lookup"><span data-stu-id="eccee-211">`ProfileService.cs`:</span></span>

```csharp
using IdentityModel;
using IdentityServer4.Models;
using IdentityServer4.Services;
using System.Threading.Tasks;

public class ProfileService : IProfileService
{
    public ProfileService()
    {
    }

    public Task GetProfileDataAsync(ProfileDataRequestContext context)
    {
        var nameClaim = context.Subject.FindAll(JwtClaimTypes.Name);
        context.IssuedClaims.AddRange(nameClaim);

        var roleClaims = context.Subject.FindAll(JwtClaimTypes.Role);
        context.IssuedClaims.AddRange(roleClaims);

        return Task.CompletedTask;
    }

    public Task IsActiveAsync(IsActiveContext context)
    {
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="eccee-212">在伺服器應用程式中，在中註冊設定檔服務 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="eccee-212">In the Server app, register the Profile Service in `Startup.ConfigureServices`:</span></span>

```csharp
using IdentityServer4.Services;

...

services.AddTransient<IProfileService, ProfileService>();
```

### <a name="use-authorization-mechanisms"></a><span data-ttu-id="eccee-213">使用授權機制</span><span class="sxs-lookup"><span data-stu-id="eccee-213">Use authorization mechanisms</span></span>

<span data-ttu-id="eccee-214">在用戶端應用程式中，元件授權方法會在此時運作。</span><span class="sxs-lookup"><span data-stu-id="eccee-214">In the Client app, component authorization approaches are functional at this point.</span></span> <span data-ttu-id="eccee-215">元件中的任何授權機制都可以使用角色來授權使用者：</span><span class="sxs-lookup"><span data-stu-id="eccee-215">Any of the authorization mechanisms in components can use a role to authorize the user:</span></span>

* <span data-ttu-id="eccee-216">[ `AuthorizeView` 元件](xref:blazor/security/index#authorizeview-component)（範例： `<AuthorizeView Roles="admin">` ）</span><span class="sxs-lookup"><span data-stu-id="eccee-216">[`AuthorizeView` component](xref:blazor/security/index#authorizeview-component) (Example: `<AuthorizeView Roles="admin">`)</span></span>
* <span data-ttu-id="eccee-217">[ `[Authorize]` attribute](xref:blazor/security/index#authorize-attribute)指示詞（ <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ）（範例： `@attribute [Authorize(Roles = "admin")]` ）</span><span class="sxs-lookup"><span data-stu-id="eccee-217">[`[Authorize]` attribute directive](xref:blazor/security/index#authorize-attribute) (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>) (Example: `@attribute [Authorize(Roles = "admin")]`)</span></span>
* <span data-ttu-id="eccee-218">程式[邏輯](xref:blazor/security/index#procedural-logic)（範例： `if (user.IsInRole("admin")) { ... }` ）</span><span class="sxs-lookup"><span data-stu-id="eccee-218">[Procedural logic](xref:blazor/security/index#procedural-logic) (Example: `if (user.IsInRole("admin")) { ... }`)</span></span>

  <span data-ttu-id="eccee-219">支援多個角色測試：</span><span class="sxs-lookup"><span data-stu-id="eccee-219">Multiple role tests are supported:</span></span>

  ```csharp
  if (user.IsInRole("admin") && user.IsInRole("developer"))
  {
      ...
  }
  ```

<span data-ttu-id="eccee-220">`User.Identity.Name`會在用戶端應用程式中填入使用者的使用者名稱，這通常是其登入電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="eccee-220">`User.Identity.Name` is populated in the Client app with the user's user name, which is usually their sign-in email address.</span></span>

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="eccee-221">其他資源</span><span class="sxs-lookup"><span data-stu-id="eccee-221">Additional resources</span></span>

* [<span data-ttu-id="eccee-222">部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eccee-222">Deployment to Azure App Service</span></span>](xref:security/authentication/identity/spa#deploy-to-production)
* [<span data-ttu-id="eccee-223">從 Key Vault 匯入憑證（Azure 檔）</span><span class="sxs-lookup"><span data-stu-id="eccee-223">Import a certificate from Key Vault (Azure documentation)</span></span>](/azure/app-service/configure-ssl-certificate#import-a-certificate-from-key-vault)
* <xref:blazor/security/webassembly/additional-scenarios>
* [<span data-ttu-id="eccee-224">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="eccee-224">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
