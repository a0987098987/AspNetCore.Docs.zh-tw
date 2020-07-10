---
title: Blazor WebAssembly使用伺服器保護 ASP.NET Core 託管應用 Identity 程式
author: guardrex
description: 若要 Blazor 從使用[ Identity 伺服器](https://identityserver.io/)後端的 Visual Studio 內，建立具有驗證的新託管應用程式
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/hosted-with-identity-server
ms.openlocfilehash: 1e5b4e37acd11280ec41c137426ecc4776d231be
ms.sourcegitcommit: 14c3d111f9d656c86af36ecb786037bf214f435c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86176237"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="f1f93-103">Blazor WebAssembly使用伺服器保護 ASP.NET Core 託管應用 Identity 程式</span><span class="sxs-lookup"><span data-stu-id="f1f93-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="f1f93-104">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1f93-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f1f93-105">本文說明如何建立新的 Blazor 託管應用程式，以使用[ Identity 伺服器](https://identityserver.io/)來驗證使用者和 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f1f93-105">This article explains how to create a new Blazor hosted app that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f1f93-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1f93-106">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f1f93-107">若要 Blazor WebAssembly 使用驗證機制建立新的專案：</span><span class="sxs-lookup"><span data-stu-id="f1f93-107">To create a new Blazor WebAssembly project with an authentication mechanism:</span></span>

1. <span data-ttu-id="f1f93-108">選擇 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中的\*\* Blazor WebAssembly 應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更\*\*]。</span><span class="sxs-lookup"><span data-stu-id="f1f93-108">After choosing the **Blazor WebAssembly App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

1. <span data-ttu-id="f1f93-109">使用 [**將使用者帳戶儲存在應用程式中**] 選項選取**個別的使用者帳戶**，以使用 ASP.NET Core 的系統將使用者儲存在應用程式中 [Identity](xref:security/authentication/identity) 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-109">Select **Individual User Accounts** with the **Store user accounts in-app** option to store users within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>

1. <span data-ttu-id="f1f93-110">在 [ **Advanced** ] 區段中，選取 [ **hosted ASP.NET Core** ] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="f1f93-110">Select the **ASP.NET Core hosted** check box in the **Advanced** section.</span></span>

# <a name="visual-studio-code--net-core-cli"></a>[<span data-ttu-id="f1f93-111">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f1f93-111">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="f1f93-112">若要使用 Blazor WebAssembly 空白資料夾中的驗證機制來建立新的專案，請指定 `Individual` 驗證機制，並 `-au|--auth` 選擇使用 ASP.NET Core 的系統將使用者儲存在應用程式內 [Identity](xref:security/authentication/identity) ：</span><span class="sxs-lookup"><span data-stu-id="f1f93-112">To create a new Blazor WebAssembly project with an authentication mechanism in an empty folder, specify the `Individual` authentication mechanism with the `-au|--auth` option to store users within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho -o {APP NAME}
```

| <span data-ttu-id="f1f93-113">預留位置</span><span class="sxs-lookup"><span data-stu-id="f1f93-113">Placeholder</span></span>  | <span data-ttu-id="f1f93-114">範例</span><span class="sxs-lookup"><span data-stu-id="f1f93-114">Example</span></span>        |
| ------------ | -------------- |
| `{APP NAME}` | `BlazorSample` |

<span data-ttu-id="f1f93-115">使用選項指定的輸出位置會 `-o|--output` 建立專案資料夾（如果不存在），並成為應用程式名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="f1f93-115">The output location specified with the `-o|--output` option creates a project folder if it doesn't exist and becomes part of the app's name.</span></span>

<span data-ttu-id="f1f93-116">如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。</span><span class="sxs-lookup"><span data-stu-id="f1f93-116">For more information, see the [`dotnet new`](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f1f93-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f1f93-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f1f93-118">若要 Blazor WebAssembly 使用驗證機制建立新的專案：</span><span class="sxs-lookup"><span data-stu-id="f1f93-118">To create a new Blazor WebAssembly project with an authentication mechanism:</span></span>

1. <span data-ttu-id="f1f93-119">在 [**設定新的 Blazor WebAssembly 應用程式**] 步驟中，從 [**驗證**] 下拉式下選取 [\*\*個別驗證] ([應用程式內) \*\* ]。</span><span class="sxs-lookup"><span data-stu-id="f1f93-119">On the **Configure your new Blazor WebAssembly App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="f1f93-120">應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 [Identity](xref:security/authentication/identity) 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-120">The app is created for individual users stored in the app with ASP.NET Core [Identity](xref:security/authentication/identity).</span></span>

1. <span data-ttu-id="f1f93-121">選取 [**主控 ASP.NET Core** ] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="f1f93-121">Select the **ASP.NET Core hosted** check box.</span></span>

---

## <a name="server-app-configuration"></a><span data-ttu-id="f1f93-122">伺服器應用程式設定</span><span class="sxs-lookup"><span data-stu-id="f1f93-122">Server app configuration</span></span>

<span data-ttu-id="f1f93-123">下列各節說明當包含驗證支援時，專案的新增功能。</span><span class="sxs-lookup"><span data-stu-id="f1f93-123">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="f1f93-124">啟始類別</span><span class="sxs-lookup"><span data-stu-id="f1f93-124">Startup class</span></span>

<span data-ttu-id="f1f93-125">`Startup`類別具有下列新增專案。</span><span class="sxs-lookup"><span data-stu-id="f1f93-125">The `Startup` class has the following additions.</span></span>

* <span data-ttu-id="f1f93-126">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="f1f93-126">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="f1f93-127">ASP.NET Core Identity ：</span><span class="sxs-lookup"><span data-stu-id="f1f93-127">ASP.NET Core Identity:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>(options => 
            options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * Identity<span data-ttu-id="f1f93-128">伺服器，另一個 <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper 方法會在伺服器上設定預設的 ASP.NET Core 慣例 Identity ：</span><span class="sxs-lookup"><span data-stu-id="f1f93-128">Server with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="f1f93-129">使用其他 helper 方法進行驗證，以設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 應用程式來驗證服務器所產生的 JWT 權杖 Identity ：</span><span class="sxs-lookup"><span data-stu-id="f1f93-129">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="f1f93-130">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="f1f93-130">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="f1f93-131">Identity伺服器中介軟體會公開 OPEN ID Connect (OIDC) 端點：</span><span class="sxs-lookup"><span data-stu-id="f1f93-131">The IdentityServer middleware exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

  * <span data-ttu-id="f1f93-132">驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：</span><span class="sxs-lookup"><span data-stu-id="f1f93-132">The Authentication middleware is responsible for validating request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="f1f93-133">授權中介軟體可啟用授權功能：</span><span class="sxs-lookup"><span data-stu-id="f1f93-133">Authorization Middleware enables authorization capabilities:</span></span>

    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="f1f93-134">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="f1f93-134">AddApiAuthorization</span></span>

<span data-ttu-id="f1f93-135"><xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>Helper 方法會針對 ASP.NET Core 案例設定[ Identity 伺服器](https://identityserver.io/)。</span><span class="sxs-lookup"><span data-stu-id="f1f93-135">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> Identity<span data-ttu-id="f1f93-136">伺服器是一種功能強大且可擴充的架構，可處理應用程式的安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="f1f93-136">Server is a powerful and extensible framework for handling app security concerns.</span></span> Identity<span data-ttu-id="f1f93-137">伺服器會在最常見的案例中公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="f1f93-137">Server exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="f1f93-138">因此，會提供一組慣例和設定選項，讓我們考慮一個良好的起點。</span><span class="sxs-lookup"><span data-stu-id="f1f93-138">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="f1f93-139">一旦您的驗證需要變更， Identity 就可以使用伺服器的完整功能，自訂驗證以符合應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="f1f93-139">Once your authentication needs change, the full power of IdentityServer is available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="f1f93-140">新增 Identity ServerJwt</span><span class="sxs-lookup"><span data-stu-id="f1f93-140">AddIdentityServerJwt</span></span>

<span data-ttu-id="f1f93-141"><xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>Helper 方法會將應用程式的原則配置設定為預設驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="f1f93-141">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="f1f93-142">此原則設定為允許 Identity 處理路由至 URL 空間中任何子路徑的所有要求 Identity `/Identity` 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-142">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="f1f93-143">會 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="f1f93-143">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="f1f93-144">此外，這個方法也會：</span><span class="sxs-lookup"><span data-stu-id="f1f93-144">Additionally, this method:</span></span>

* <span data-ttu-id="f1f93-145">向伺服器註冊具有 `{APPLICATION NAME}API` Identity 預設範圍的 API 資源 `{APPLICATION NAME}API` 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-145">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="f1f93-146">設定 JWT 持有人權杖中介軟體，以驗證 Identity 伺服器針對應用程式所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="f1f93-146">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="f1f93-147">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="f1f93-147">WeatherForecastController</span></span>

<span data-ttu-id="f1f93-148">在 `WeatherForecastController` (`Controllers/WeatherForecastController.cs`) 中， [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性會套用至類別。</span><span class="sxs-lookup"><span data-stu-id="f1f93-148">In the `WeatherForecastController` (`Controllers/WeatherForecastController.cs`), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="f1f93-149">屬性會指出使用者必須根據預設原則來存取資源。</span><span class="sxs-lookup"><span data-stu-id="f1f93-149">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="f1f93-150">預設的授權原則會設定為使用預設的驗證配置，這是由所設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-150">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>.</span></span> <span data-ttu-id="f1f93-151">Helper 方法會將設定 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 為應用程式要求的預設處理常式。</span><span class="sxs-lookup"><span data-stu-id="f1f93-151">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="f1f93-152">[ApplicationdbcoNtext]</span><span class="sxs-lookup"><span data-stu-id="f1f93-152">ApplicationDbContext</span></span>

<span data-ttu-id="f1f93-153">在 `ApplicationDbContext` (`Data/ApplicationDbContext.cs`) 中，會 <xref:Microsoft.EntityFrameworkCore.DbContext> 延伸 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 以包含伺服器的架構 Identity 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-153">In the `ApplicationDbContext` (`Data/ApplicationDbContext.cs`), <xref:Microsoft.EntityFrameworkCore.DbContext> extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="f1f93-154"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。</span><span class="sxs-lookup"><span data-stu-id="f1f93-154"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="f1f93-155">若要取得資料庫架構的完整控制權，請從其中一個可用的類別繼承， Identity <xref:Microsoft.EntityFrameworkCore.DbContext> 並透過 Identity `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` 在方法中呼叫來設定內容以包含架構 <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-155">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="f1f93-156">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="f1f93-156">OidcConfigurationController</span></span>

<span data-ttu-id="f1f93-157">在 `OidcConfigurationController` (`Controllers/OidcConfigurationController.cs`) 中，會布建用戶端端點來提供 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="f1f93-157">In the `OidcConfigurationController` (`Controllers/OidcConfigurationController.cs`), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings"></a><span data-ttu-id="f1f93-158">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="f1f93-158">App settings</span></span>

<span data-ttu-id="f1f93-159">在應用程式佈建檔案中 `appsettings.json` ， (專案根目錄的) ， `IdentityServer` 一節會描述已設定的用戶端清單。</span><span class="sxs-lookup"><span data-stu-id="f1f93-159">In the app settings file (`appsettings.json`) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="f1f93-160">在下列範例中，有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="f1f93-160">In the following example, there's a single client.</span></span> <span data-ttu-id="f1f93-161">用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId` 參數。</span><span class="sxs-lookup"><span data-stu-id="f1f93-161">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="f1f93-162">此設定檔會指出正在設定的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="f1f93-162">The profile indicates the app type being configured.</span></span> <span data-ttu-id="f1f93-163">此設定檔會在內部使用，以驅動可簡化伺服器設定程式的慣例。</span><span class="sxs-lookup"><span data-stu-id="f1f93-163">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "{APP ASSEMBLY}.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="f1f93-164">預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如， `BlazorSample.Client`) 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-164">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `BlazorSample.Client`).</span></span>

## <a name="client-app-configuration"></a><span data-ttu-id="f1f93-165">用戶端應用程式設定</span><span class="sxs-lookup"><span data-stu-id="f1f93-165">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="f1f93-166">驗證套件</span><span class="sxs-lookup"><span data-stu-id="f1f93-166">Authentication package</span></span>

<span data-ttu-id="f1f93-167">建立應用程式以使用個別使用者帳戶 () 時 `Individual` ，應用程式會 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 在應用程式的專案檔中自動接收套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="f1f93-167">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package in the app's project file.</span></span> <span data-ttu-id="f1f93-168">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="f1f93-168">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="f1f93-169">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="f1f93-169">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="3.2.0" />
```

### <a name="httpclient-configuration"></a><span data-ttu-id="f1f93-170">`HttpClient`配置</span><span class="sxs-lookup"><span data-stu-id="f1f93-170">`HttpClient` configuration</span></span>

<span data-ttu-id="f1f93-171">在 `Program.Main` (`Program.cs`) 中，名稱為 <xref:System.Net.Http.HttpClient> (的 `HostIS.ServerAPI`) 會設定為在 <xref:System.Net.Http.HttpClient> 對伺服器 API 提出要求時，提供包含存取權杖的實例：</span><span class="sxs-lookup"><span data-stu-id="f1f93-171">In `Program.Main` (`Program.cs`), a named <xref:System.Net.Http.HttpClient> (`HostIS.ServerAPI`) is configured to supply <xref:System.Net.Http.HttpClient> instances that include access tokens when making requests to the server API:</span></span>

```csharp
builder.Services.AddHttpClient("HostIS.ServerAPI", 
        client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("HostIS.ServerAPI"));
```

> [!NOTE]
> <span data-ttu-id="f1f93-172">如果您要將 Blazor WebAssembly 應用程式設定為使用 Identity 不屬於託管解決方案的現有伺服器實例 Blazor ，請將 <xref:System.Net.Http.HttpClient> 基底位址註冊從 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType> () 變更 `builder.HostEnvironment.BaseAddress` 為伺服器應用程式的 API 授權端點 URL。</span><span class="sxs-lookup"><span data-stu-id="f1f93-172">If you're configuring a Blazor WebAssembly app to use an existing Identity Server instance that isn't part of a Blazor Hosted solution, change the <xref:System.Net.Http.HttpClient> base address registration from <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType> (`builder.HostEnvironment.BaseAddress`) to the server app's API authorization endpoint URL.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="f1f93-173">API 授權支援</span><span class="sxs-lookup"><span data-stu-id="f1f93-173">API authorization support</span></span>

<span data-ttu-id="f1f93-174">驗證使用者的支援是由套件內提供的擴充方法插入服務容器中 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-174">The support for authenticating users is plugged into the service container by the extension method provided inside the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package.</span></span> <span data-ttu-id="f1f93-175">這個方法會設定應用程式所需的服務，以與現有的授權系統互動。</span><span class="sxs-lookup"><span data-stu-id="f1f93-175">This method sets up the services required by the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="f1f93-176">根據預設，應用程式的設定會依照慣例從載入 `_configuration/{client-id}` 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-176">By default, configuration for the app is loaded by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="f1f93-177">依照慣例，用戶端識別碼會設定為應用程式的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="f1f93-177">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="f1f93-178">您可以使用選項呼叫多載，將此 URL 變更為指向不同的端點。</span><span class="sxs-lookup"><span data-stu-id="f1f93-178">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="imports-file"></a><span data-ttu-id="f1f93-179">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="f1f93-179">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a><span data-ttu-id="f1f93-180">索引頁面</span><span class="sxs-lookup"><span data-stu-id="f1f93-180">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="f1f93-181">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="f1f93-181">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="f1f93-182">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="f1f93-182">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="f1f93-183">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="f1f93-183">LoginDisplay component</span></span>

<span data-ttu-id="f1f93-184">`LoginDisplay`元件 (`Shared/LoginDisplay.razor`) 會在 `MainLayout` 元件 () 中呈現 `Shared/MainLayout.razor` ，並管理下列行為：</span><span class="sxs-lookup"><span data-stu-id="f1f93-184">The `LoginDisplay` component (`Shared/LoginDisplay.razor`) is rendered in the `MainLayout` component (`Shared/MainLayout.razor`) and manages the following behaviors:</span></span>

* <span data-ttu-id="f1f93-185">針對已驗證的使用者：</span><span class="sxs-lookup"><span data-stu-id="f1f93-185">For authenticated users:</span></span>
  * <span data-ttu-id="f1f93-186">顯示目前的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="f1f93-186">Displays the current user name.</span></span>
  * <span data-ttu-id="f1f93-187">提供 ASP.NET Core 中 [使用者設定檔] 頁面的連結 Identity 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-187">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="f1f93-188">提供用來登出應用程式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f1f93-188">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="f1f93-189">匿名使用者：</span><span class="sxs-lookup"><span data-stu-id="f1f93-189">For anonymous users:</span></span>
  * <span data-ttu-id="f1f93-190">提供註冊的選項。</span><span class="sxs-lookup"><span data-stu-id="f1f93-190">Offers the option to register.</span></span>
  * <span data-ttu-id="f1f93-191">提供登入的選項。</span><span class="sxs-lookup"><span data-stu-id="f1f93-191">Offers the option to log in.</span></span>

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

### <a name="authentication-component"></a><span data-ttu-id="f1f93-192">驗證元件</span><span class="sxs-lookup"><span data-stu-id="f1f93-192">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="f1f93-193">FetchData 元件</span><span class="sxs-lookup"><span data-stu-id="f1f93-193">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="f1f93-194">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="f1f93-194">Run the app</span></span>

<span data-ttu-id="f1f93-195">從伺服器專案執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1f93-195">Run the app from the Server project.</span></span> <span data-ttu-id="f1f93-196">使用 Visual Studio 時，您可以：</span><span class="sxs-lookup"><span data-stu-id="f1f93-196">When using Visual Studio, either:</span></span>

* <span data-ttu-id="f1f93-197">將工具列中的 [**啟始專案**] 下拉式清單設定為*伺服器 API 應用程式*，然後選取 [**執行**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f1f93-197">Set the **Startup Projects** drop down list in the toolbar to the *Server API app* and select the **Run** button.</span></span>
* <span data-ttu-id="f1f93-198">在**方案總管**中選取伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1f93-198">Select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

## <a name="name-and-role-claim-with-api-authorization"></a><span data-ttu-id="f1f93-199">具有 API 授權的名稱和角色宣告</span><span class="sxs-lookup"><span data-stu-id="f1f93-199">Name and role claim with API authorization</span></span>

### <a name="custom-user-factory"></a><span data-ttu-id="f1f93-200">自訂使用者工廠</span><span class="sxs-lookup"><span data-stu-id="f1f93-200">Custom user factory</span></span>

<span data-ttu-id="f1f93-201">在用戶端應用程式中，建立自訂的使用者 factory。</span><span class="sxs-lookup"><span data-stu-id="f1f93-201">In the Client app, create a custom user factory.</span></span> Identity<span data-ttu-id="f1f93-202">伺服器會在單一宣告中以 JSON 陣列的形式傳送多個角色 `role` 。</span><span class="sxs-lookup"><span data-stu-id="f1f93-202"> Server sends multiple roles as a JSON array in a single `role` claim.</span></span> <span data-ttu-id="f1f93-203">單一角色會當做宣告中的字串值來傳送。</span><span class="sxs-lookup"><span data-stu-id="f1f93-203">A single role is sent as a string value in the claim.</span></span> <span data-ttu-id="f1f93-204">Factory 會 `role` 針對每個使用者角色建立個別的宣告。</span><span class="sxs-lookup"><span data-stu-id="f1f93-204">The factory creates an individual `role` claim for each of the user's roles.</span></span>

<span data-ttu-id="f1f93-205">`CustomUserFactory.cs`:</span><span class="sxs-lookup"><span data-stu-id="f1f93-205">`CustomUserFactory.cs`:</span></span>

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

<span data-ttu-id="f1f93-206">在用戶端應用程式中，于 () 中註冊 factory `Program.Main` `Program.cs` ：</span><span class="sxs-lookup"><span data-stu-id="f1f93-206">In the Client app, register the factory in `Program.Main` (`Program.cs`):</span></span>

```csharp
builder.Services.AddApiAuthorization()
    .AddAccountClaimsPrincipalFactory<CustomUserFactory>();
```

<span data-ttu-id="f1f93-207">在伺服器應用程式中，呼叫產生器 <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> 上的 Identity ，這會新增角色相關服務：</span><span class="sxs-lookup"><span data-stu-id="f1f93-207">In the Server app, call <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> on the Identity builder, which adds role-related services:</span></span>

```csharp
using Microsoft.AspNetCore.Identity;

...

services.AddDefaultIdentity<ApplicationUser>(options => 
    options.SignIn.RequireConfirmedAccount = true)
    .AddRoles<IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="configure-identity-server"></a><span data-ttu-id="f1f93-208">設定 Identity 伺服器</span><span class="sxs-lookup"><span data-stu-id="f1f93-208">Configure Identity Server</span></span>

<span data-ttu-id="f1f93-209">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="f1f93-209">Use **one** of the following approaches:</span></span>

* [<span data-ttu-id="f1f93-210">API 授權選項</span><span class="sxs-lookup"><span data-stu-id="f1f93-210">API authorization options</span></span>](#api-authorization-options)
* [<span data-ttu-id="f1f93-211">設定檔服務</span><span class="sxs-lookup"><span data-stu-id="f1f93-211">Profile Service</span></span>](#profile-service)

#### <a name="api-authorization-options"></a><span data-ttu-id="f1f93-212">API 授權選項</span><span class="sxs-lookup"><span data-stu-id="f1f93-212">API authorization options</span></span>

<span data-ttu-id="f1f93-213">在伺服器應用程式中：</span><span class="sxs-lookup"><span data-stu-id="f1f93-213">In the Server app:</span></span>

* <span data-ttu-id="f1f93-214">設定 Identity 伺服器將 `name` 和宣告放 `role` 入識別碼權杖和存取權杖中。</span><span class="sxs-lookup"><span data-stu-id="f1f93-214">Configure Identity Server to put the `name` and `role` claims into the ID token and access token.</span></span>
* <span data-ttu-id="f1f93-215">防止 JWT 權杖處理常式中的角色的預設對應。</span><span class="sxs-lookup"><span data-stu-id="f1f93-215">Prevent the default mapping for roles in the JWT token handler.</span></span>

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

#### <a name="profile-service"></a><span data-ttu-id="f1f93-216">設定檔服務</span><span class="sxs-lookup"><span data-stu-id="f1f93-216">Profile Service</span></span>

<span data-ttu-id="f1f93-217">在伺服器應用程式中，建立一個 `ProfileService` 執行。</span><span class="sxs-lookup"><span data-stu-id="f1f93-217">In the Server app, create a `ProfileService` implementation.</span></span>

<span data-ttu-id="f1f93-218">`ProfileService.cs`:</span><span class="sxs-lookup"><span data-stu-id="f1f93-218">`ProfileService.cs`:</span></span>

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

<span data-ttu-id="f1f93-219">在伺服器應用程式中，在中註冊設定檔服務 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="f1f93-219">In the Server app, register the Profile Service in `Startup.ConfigureServices`:</span></span>

```csharp
using IdentityServer4.Services;

...

services.AddTransient<IProfileService, ProfileService>();
```

### <a name="use-authorization-mechanisms"></a><span data-ttu-id="f1f93-220">使用授權機制</span><span class="sxs-lookup"><span data-stu-id="f1f93-220">Use authorization mechanisms</span></span>

<span data-ttu-id="f1f93-221">在用戶端應用程式中，元件授權方法會在此時運作。</span><span class="sxs-lookup"><span data-stu-id="f1f93-221">In the Client app, component authorization approaches are functional at this point.</span></span> <span data-ttu-id="f1f93-222">元件中的任何授權機制都可以使用角色來授權使用者：</span><span class="sxs-lookup"><span data-stu-id="f1f93-222">Any of the authorization mechanisms in components can use a role to authorize the user:</span></span>

* <span data-ttu-id="f1f93-223">[ `AuthorizeView` 元件](xref:blazor/security/index#authorizeview-component) (範例： `<AuthorizeView Roles="admin">`) </span><span class="sxs-lookup"><span data-stu-id="f1f93-223">[`AuthorizeView` component](xref:blazor/security/index#authorizeview-component) (Example: `<AuthorizeView Roles="admin">`)</span></span>
* <span data-ttu-id="f1f93-224">[ `[Authorize]` 屬性](xref:blazor/security/index#authorize-attribute)指示詞 (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>)  (範例： `@attribute [Authorize(Roles = "admin")]`) </span><span class="sxs-lookup"><span data-stu-id="f1f93-224">[`[Authorize]` attribute directive](xref:blazor/security/index#authorize-attribute) (<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute>) (Example: `@attribute [Authorize(Roles = "admin")]`)</span></span>
* <span data-ttu-id="f1f93-225">程式[邏輯](xref:blazor/security/index#procedural-logic) (範例： `if (user.IsInRole("admin")) { ... }`) </span><span class="sxs-lookup"><span data-stu-id="f1f93-225">[Procedural logic](xref:blazor/security/index#procedural-logic) (Example: `if (user.IsInRole("admin")) { ... }`)</span></span>

  <span data-ttu-id="f1f93-226">支援多個角色測試：</span><span class="sxs-lookup"><span data-stu-id="f1f93-226">Multiple role tests are supported:</span></span>

  ```csharp
  if (user.IsInRole("admin") && user.IsInRole("developer"))
  {
      ...
  }
  ```

<span data-ttu-id="f1f93-227">`User.Identity.Name`會在用戶端應用程式中填入使用者的使用者名稱，這通常是其登入電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="f1f93-227">`User.Identity.Name` is populated in the Client app with the user's user name, which is usually their sign-in email address.</span></span>

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="f1f93-228">其他資源</span><span class="sxs-lookup"><span data-stu-id="f1f93-228">Additional resources</span></span>

* [<span data-ttu-id="f1f93-229">部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f1f93-229">Deployment to Azure App Service</span></span>](xref:security/authentication/identity/spa#deploy-to-production)
* [<span data-ttu-id="f1f93-230">從 Key Vault (Azure 檔匯入憑證) </span><span class="sxs-lookup"><span data-stu-id="f1f93-230">Import a certificate from Key Vault (Azure documentation)</span></span>](/azure/app-service/configure-ssl-certificate#import-a-certificate-from-key-vault)
* <xref:blazor/security/webassembly/additional-scenarios>
* [<span data-ttu-id="f1f93-231">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="f1f93-231">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
