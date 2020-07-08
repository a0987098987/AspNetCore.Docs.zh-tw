---
title: ASP.NET Core 上單一頁面應用程式的驗證簡介
author: javiercn
description: 搭配 Identity ASP.NET Core 應用程式內裝載的單一頁面應用程式使用。
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/08/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity/spa
ms.openlocfilehash: 2b587517268208dcf66cd2895b7aa22bfa381f84
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060354"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="c9988-103">Spa 的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="c9988-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="c9988-104">ASP.NET Core 3.0 或更新版本使用 API 授權的支援，在單一頁面應用程式（Spa）中提供驗證。</span><span class="sxs-lookup"><span data-stu-id="c9988-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="c9988-105">Identity用於驗證和儲存使用者的 ASP.NET Core 會與[IdentityServer](https://identityserver.io/)整合，以執行 Open ID Connect。</span><span class="sxs-lookup"><span data-stu-id="c9988-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="c9988-106">驗證參數已加入至「**角度**」和「**回應**」專案範本，類似于 Web 應用程式中的驗證參數 **（[模型-視圖-控制器）** ] （MVC）和 [ **web 應用程式**（ Razor 頁面）] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="c9988-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="c9988-107">允許的參數值為 [**無**] 和 [**個人**]。</span><span class="sxs-lookup"><span data-stu-id="c9988-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="c9988-108">**React.js 和 Redux**專案範本目前不支援驗證參數。</span><span class="sxs-lookup"><span data-stu-id="c9988-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="c9988-109">建立具有 API 授權支援的應用程式</span><span class="sxs-lookup"><span data-stu-id="c9988-109">Create an app with API authorization support</span></span>

<span data-ttu-id="c9988-110">使用者驗證和授權可以同時用於「角度」和「回應」 Spa。</span><span class="sxs-lookup"><span data-stu-id="c9988-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="c9988-111">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c9988-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="c9988-112">**角度**：</span><span class="sxs-lookup"><span data-stu-id="c9988-112">**Angular**:</span></span>

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="c9988-113">**回應**：</span><span class="sxs-lookup"><span data-stu-id="c9988-113">**React**:</span></span>

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="c9988-114">上述命令會建立一個 ASP.NET Core 應用程式，其中具有包含 SPA 的*ClientApp*目錄。</span><span class="sxs-lookup"><span data-stu-id="c9988-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="c9988-115">應用程式 ASP.NET Core 元件的一般描述</span><span class="sxs-lookup"><span data-stu-id="c9988-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="c9988-116">下列各節說明當包含驗證支援時，專案的新增功能：</span><span class="sxs-lookup"><span data-stu-id="c9988-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="c9988-117">啟始類別</span><span class="sxs-lookup"><span data-stu-id="c9988-117">Startup class</span></span>

<span data-ttu-id="c9988-118">下列程式碼範例會依賴[ApiAuthorization IdentityServer](https://www.nuget.org/packages/Microsoft.AspNetCore.ApiAuthorization.IdentityServer) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="c9988-118">The following code examples rely on the [Microsoft.AspNetCore.ApiAuthorization.IdentityServer](https://www.nuget.org/packages/Microsoft.AspNetCore.ApiAuthorization.IdentityServer) NuGet package.</span></span> <span data-ttu-id="c9988-119">這些範例會使用 <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> 和擴充方法來設定 API 驗證和授權 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiResourceCollection.AddIdentityServerJwt%2A> 。</span><span class="sxs-lookup"><span data-stu-id="c9988-119">The examples configure API authentication and authorization using the <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> and <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiResourceCollection.AddIdentityServerJwt%2A> extension methods.</span></span> <span data-ttu-id="c9988-120">使用 [回應] 或 [角度 SPA] 專案範本搭配驗證的專案，包括對此套件的參考。</span><span class="sxs-lookup"><span data-stu-id="c9988-120">Projects using the React or Angular SPA project templates with authentication include a reference to this package.</span></span>

<span data-ttu-id="c9988-121">`Startup`類別具有下列新增專案：</span><span class="sxs-lookup"><span data-stu-id="c9988-121">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="c9988-122">在 `Startup.ConfigureServices` 方法內：</span><span class="sxs-lookup"><span data-stu-id="c9988-122">Inside the `Startup.ConfigureServices` method:</span></span>
  * Identity<span data-ttu-id="c9988-123">使用預設 UI：</span><span class="sxs-lookup"><span data-stu-id="c9988-123"> with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="c9988-124">IdentityServer 使用額外的協助 `AddApiAuthorization` 程式方法，在 IdentityServer 上設定一些預設的 ASP.NET Core 慣例：</span><span class="sxs-lookup"><span data-stu-id="c9988-124">IdentityServer with an additional `AddApiAuthorization` helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="c9988-125">使用其他 helper 方法進行驗證，以設定 `AddIdentityServerJwt` 應用程式來驗證 IdentityServer 所產生的 JWT 權杖：</span><span class="sxs-lookup"><span data-stu-id="c9988-125">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="c9988-126">在 `Startup.Configure` 方法內：</span><span class="sxs-lookup"><span data-stu-id="c9988-126">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="c9988-127">驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：</span><span class="sxs-lookup"><span data-stu-id="c9988-127">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="c9988-128">公開 Open ID Connect 端點的 IdentityServer 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="c9988-128">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="c9988-129">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="c9988-129">AddApiAuthorization</span></span>

<span data-ttu-id="c9988-130">此 helper 方法會將 IdentityServer 設定為使用我們支援的設定。</span><span class="sxs-lookup"><span data-stu-id="c9988-130">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="c9988-131">IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="c9988-131">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="c9988-132">同時，會在最常見的案例中公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="c9988-132">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="c9988-133">因此，系統會為您提供一組慣例和設定選項，並將其視為良好的起點。</span><span class="sxs-lookup"><span data-stu-id="c9988-133">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="c9988-134">一旦您的驗證需要變更，IdentityServer 的完整功能仍可供自訂驗證以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="c9988-134">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="c9988-135">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="c9988-135">AddIdentityServerJwt</span></span>

<span data-ttu-id="c9988-136">此 helper 方法會將應用程式的原則配置設定為預設驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="c9988-136">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="c9988-137">原則會設定為讓 Identity 處理所有傳送至 Identity URL 空間 "/" 中任何子路徑的要求 Identity 。</span><span class="sxs-lookup"><span data-stu-id="c9988-137">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="c9988-138">會 `JwtBearerHandler` 處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="c9988-138">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="c9988-139">此外，此方法會向 `<<ApplicationName>>API` IdentityServer 註冊具有預設範圍的 API 資源 `<<ApplicationName>>API` ，並設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="c9988-139">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="c9988-140">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="c9988-140">WeatherForecastController</span></span>

<span data-ttu-id="c9988-141">在*Controllers\WeatherForecastController.cs*檔案中，請注意套用 `[Authorize]` 至類別的屬性，表示使用者必須根據預設原則來存取資源。</span><span class="sxs-lookup"><span data-stu-id="c9988-141">In the *Controllers\WeatherForecastController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="c9988-142">預設的授權原則會設定為使用預設的驗證配置，這是由先前所 `AddIdentityServerJwt` 述的原則配置所設定，讓 `JwtBearerHandler` 這類 helper 方法為應用程式要求提供預設的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c9988-142">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="c9988-143">[ApplicationdbcoNtext]</span><span class="sxs-lookup"><span data-stu-id="c9988-143">ApplicationDbContext</span></span>

<span data-ttu-id="c9988-144">請注意，在*Data\ApplicationDbCoNtext.cs*檔案中，使用了相同的， `DbContext` Identity 因為它所擴充的例外 `ApiAuthorizationDbContext` （衍生的類別來自 `IdentityDbContext` ），以包含 IdentityServer 的架構。</span><span class="sxs-lookup"><span data-stu-id="c9988-144">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="c9988-145">若要取得資料庫架構的完整控制權，請從其中一個可用的類別繼承，然後透過在 Identity `DbContext` 方法上呼叫來設定內容以包含 Identity 架構 `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` `OnModelCreating` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-145">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="c9988-146">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="c9988-146">OidcConfigurationController</span></span>

<span data-ttu-id="c9988-147">在*Controllers\OidcConfigurationController.cs*檔案中，請注意布建的端點可提供用戶端所需使用的 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="c9988-147">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c9988-148">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="c9988-148">appsettings.json</span></span>

<span data-ttu-id="c9988-149">在專案根目錄的*appsettings.json*檔案中，有一個新的 `IdentityServer` 區段描述已設定的用戶端清單。</span><span class="sxs-lookup"><span data-stu-id="c9988-149">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="c9988-150">在下列範例中，有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="c9988-150">In the following example, there's a single client.</span></span> <span data-ttu-id="c9988-151">用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId` 參數。</span><span class="sxs-lookup"><span data-stu-id="c9988-151">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="c9988-152">此設定檔會指出正在設定的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="c9988-152">The profile indicates the app type being configured.</span></span> <span data-ttu-id="c9988-153">它是在內部用來驅動慣例，以簡化伺服器的設定程式。</span><span class="sxs-lookup"><span data-stu-id="c9988-153">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="c9988-154">有數個設定檔可供使用，如[應用程式佈建檔](#application-profiles)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="c9988-154">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="c9988-155">appsettings.Development.js于</span><span class="sxs-lookup"><span data-stu-id="c9988-155">appsettings.Development.json</span></span>

<span data-ttu-id="c9988-156">在專案根目錄的*appsettings.Development.json*檔案中，有一個 `IdentityServer` 區段描述用來簽署權杖的金鑰。</span><span class="sxs-lookup"><span data-stu-id="c9988-156">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="c9988-157">部署至生產環境時，必須隨應用程式一起布建和部署金鑰，如[部署至生產](#deploy-to-production)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="c9988-157">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="c9988-158">角度應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="c9988-158">General description of the Angular app</span></span>

<span data-ttu-id="c9988-159">角度範本中的驗證和 API 授權支援位於*ClientApp\src\api-authorization*目錄中自己的角度模組。</span><span class="sxs-lookup"><span data-stu-id="c9988-159">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="c9988-160">模組是由下列元素所組成：</span><span class="sxs-lookup"><span data-stu-id="c9988-160">The module is composed of the following elements:</span></span>

* <span data-ttu-id="c9988-161">3個元件：</span><span class="sxs-lookup"><span data-stu-id="c9988-161">3 components:</span></span>
  * <span data-ttu-id="c9988-162">*login. component. ts*：處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="c9988-162">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="c9988-163">*登出。 component. ts*：處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="c9988-163">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="c9988-164">*[登入]-[component. ts*]：顯示下列其中一組連結的 widget：</span><span class="sxs-lookup"><span data-stu-id="c9988-164">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="c9988-165">使用者設定檔管理，並在使用者經過驗證時登出連結。</span><span class="sxs-lookup"><span data-stu-id="c9988-165">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="c9988-166">當使用者未經過驗證時，註冊並登入連結。</span><span class="sxs-lookup"><span data-stu-id="c9988-166">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="c9988-167">`AuthorizeGuard`可以新增至路由的路由防護，並要求使用者在造訪路由之前進行驗證。</span><span class="sxs-lookup"><span data-stu-id="c9988-167">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="c9988-168">HTTP 攔截器 `AuthorizeInterceptor` ，會在使用者通過驗證時，將存取權杖附加至以 API 為目標的傳出 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="c9988-168">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="c9988-169">此服務 `AuthorizeService` 會處理驗證程式的較低層級詳細資料，並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。</span><span class="sxs-lookup"><span data-stu-id="c9988-169">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="c9988-170">一個角度模組，定義與應用程式驗證部分相關聯的路由。</span><span class="sxs-lookup"><span data-stu-id="c9988-170">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="c9988-171">它會公開登入功能表元件、攔截器、防護和服務，以便從應用程式的其餘部分取用。</span><span class="sxs-lookup"><span data-stu-id="c9988-171">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="c9988-172">回應應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="c9988-172">General description of the React app</span></span>

<span data-ttu-id="c9988-173">回應範本中的驗證和 API 授權支援位於*ClientApp\src\components\api-authorization*目錄中。</span><span class="sxs-lookup"><span data-stu-id="c9988-173">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="c9988-174">它是由下列元素所組成：</span><span class="sxs-lookup"><span data-stu-id="c9988-174">It's composed of the following elements:</span></span>

* <span data-ttu-id="c9988-175">4個元件：</span><span class="sxs-lookup"><span data-stu-id="c9988-175">4 components:</span></span>
  * <span data-ttu-id="c9988-176">*Login.js*：處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="c9988-176">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="c9988-177">*Logout.js*：處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="c9988-177">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="c9988-178">*LoginMenu.js*：顯示下列其中一組連結的 widget：</span><span class="sxs-lookup"><span data-stu-id="c9988-178">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="c9988-179">使用者設定檔管理，並在使用者經過驗證時登出連結。</span><span class="sxs-lookup"><span data-stu-id="c9988-179">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="c9988-180">當使用者未經過驗證時，註冊並登入連結。</span><span class="sxs-lookup"><span data-stu-id="c9988-180">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="c9988-181">*AuthorizeRoute.js*：需要先驗證使用者，然後才呈現參數所指示之元件的路由元件 `Component` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-181">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="c9988-182">已匯出 `authService` 的類別實例 `AuthorizeService` ，可處理較低層級的驗證程式詳細資料，並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。</span><span class="sxs-lookup"><span data-stu-id="c9988-182">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="c9988-183">既然您已瞭解解決方案的主要元件，您可以進一步瞭解應用程式的個別案例。</span><span class="sxs-lookup"><span data-stu-id="c9988-183">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="c9988-184">需要新 API 的授權</span><span class="sxs-lookup"><span data-stu-id="c9988-184">Require authorization on a new API</span></span>

<span data-ttu-id="c9988-185">根據預設，系統會設定為輕鬆地要求新 Api 的授權。</span><span class="sxs-lookup"><span data-stu-id="c9988-185">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="c9988-186">若要這麼做，請建立新的控制器，並將 `[Authorize]` 屬性新增至控制器類別或控制器內的任何動作。</span><span class="sxs-lookup"><span data-stu-id="c9988-186">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="customize-the-api-authentication-handler"></a><span data-ttu-id="c9988-187">自訂 API 驗證處理常式</span><span class="sxs-lookup"><span data-stu-id="c9988-187">Customize the API authentication handler</span></span>

<span data-ttu-id="c9988-188">若要自訂 API 之 JWT 處理常式的設定，請設定其 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> 實例：</span><span class="sxs-lookup"><span data-stu-id="c9988-188">To customize the configuration of the API's JWT handler, configure its <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> instance:</span></span>

```csharp
services.AddAuthentication()
    .AddIdentityServerJwt();

services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        ...
    });
```

<span data-ttu-id="c9988-189">API 的 JWT 處理常式會引發事件，讓您能夠使用來控制驗證進程 `JwtBearerEvents` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-189">The API's JWT handler raises events that enable control over the authentication process using `JwtBearerEvents`.</span></span> <span data-ttu-id="c9988-190">若要提供 API 授權的支援，請 `AddIdentityServerJwt` 註冊自己的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="c9988-190">To provide support for API authorization, `AddIdentityServerJwt` registers its own event handlers.</span></span>

<span data-ttu-id="c9988-191">若要自訂事件的處理，請視需要使用其他邏輯來包裝現有的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="c9988-191">To customize the handling of an event, wrap the existing event handler with additional logic as required.</span></span> <span data-ttu-id="c9988-192">例如：</span><span class="sxs-lookup"><span data-stu-id="c9988-192">For example:</span></span>

```csharp
services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        var onTokenValidated = options.Events.OnTokenValidated;       
        
        options.Events.OnTokenValidated = async context =>
        {
            await onTokenValidated(context);
            ...
        }
    });
```

<span data-ttu-id="c9988-193">在上述程式碼中， `OnTokenValidated` 事件處理常式會取代為自訂執行。</span><span class="sxs-lookup"><span data-stu-id="c9988-193">In the preceding code, the `OnTokenValidated` event handler is replaced with a custom implementation.</span></span> <span data-ttu-id="c9988-194">此實作為：</span><span class="sxs-lookup"><span data-stu-id="c9988-194">This implementation:</span></span>

1. <span data-ttu-id="c9988-195">呼叫 API 授權支援所提供的原始實作為。</span><span class="sxs-lookup"><span data-stu-id="c9988-195">Calls the original implementation provided by the API authorization support.</span></span>
1. <span data-ttu-id="c9988-196">執行它自己的自訂邏輯。</span><span class="sxs-lookup"><span data-stu-id="c9988-196">Run its own custom logic.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="c9988-197">保護用戶端路由（角度）</span><span class="sxs-lookup"><span data-stu-id="c9988-197">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="c9988-198">若要保護用戶端路由，請將授權防護新增至設定路由時要執行的防護清單。</span><span class="sxs-lookup"><span data-stu-id="c9988-198">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="c9988-199">例如，您可以在 `fetch-data` 主要應用程式角度模組中查看路由的設定方式：</span><span class="sxs-lookup"><span data-stu-id="c9988-199">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="c9988-200">請務必注意，保護路由並不會保護實際端點（仍然需要套用 `[Authorize]` 屬性），但它只會防止使用者在未驗證時流覽至指定的用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="c9988-200">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="c9988-201">驗證 API 要求（角度）</span><span class="sxs-lookup"><span data-stu-id="c9988-201">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="c9988-202">使用應用程式所定義的 HTTP 用戶端攔截器，來驗證與應用程式一起裝載之 Api 的要求會自動完成。</span><span class="sxs-lookup"><span data-stu-id="c9988-202">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="c9988-203">保護用戶端路由（回應）</span><span class="sxs-lookup"><span data-stu-id="c9988-203">Protect a client-side route (React)</span></span>

<span data-ttu-id="c9988-204">使用 `AuthorizeRoute` 元件（而不是一般元件）來保護用戶端路由 `Route` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-204">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="c9988-205">例如，請注意如何在 `fetch-data` 元件中設定路由 `App` ：</span><span class="sxs-lookup"><span data-stu-id="c9988-205">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="c9988-206">保護路由：</span><span class="sxs-lookup"><span data-stu-id="c9988-206">Protecting a route:</span></span>

* <span data-ttu-id="c9988-207">不會保護實際的端點（仍然需要套用 `[Authorize]` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="c9988-207">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="c9988-208">只有在未驗證時，才會防止使用者流覽至指定的用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="c9988-208">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="c9988-209">驗證 API 要求（回應）</span><span class="sxs-lookup"><span data-stu-id="c9988-209">Authenticate API requests (React)</span></span>

<span data-ttu-id="c9988-210">藉由先從匯入實例，來驗證具有回應的要求 `authService` `AuthorizeService` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-210">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="c9988-211">存取權杖是從抓取 `authService` ，並且附加至要求，如下所示。</span><span class="sxs-lookup"><span data-stu-id="c9988-211">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="c9988-212">在回應元件中，這項工作通常會在 `componentDidMount` 生命週期方法中完成，或做為某些使用者互動的結果。</span><span class="sxs-lookup"><span data-stu-id="c9988-212">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="c9988-213">將 authService 匯入您的元件</span><span class="sxs-lookup"><span data-stu-id="c9988-213">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="c9988-214">取得存取權杖，並將其附加至回應</span><span class="sxs-lookup"><span data-stu-id="c9988-214">Retrieve and attach the access token to the response</span></span>

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a><span data-ttu-id="c9988-215">部署到生產</span><span class="sxs-lookup"><span data-stu-id="c9988-215">Deploy to production</span></span>

<span data-ttu-id="c9988-216">若要將應用程式部署至生產環境，您必須布建下列資源：</span><span class="sxs-lookup"><span data-stu-id="c9988-216">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="c9988-217">用來儲存 Identity 使用者帳戶和 IdentityServer 授與的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c9988-217">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="c9988-218">用於簽署權杖的生產憑證。</span><span class="sxs-lookup"><span data-stu-id="c9988-218">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="c9988-219">此憑證沒有特定的需求;它可以是自我簽署憑證或透過 CA 授權單位布建的憑證。</span><span class="sxs-lookup"><span data-stu-id="c9988-219">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="c9988-220">您可以透過 PowerShell 或 OpenSSL 之類的標準工具來產生它。</span><span class="sxs-lookup"><span data-stu-id="c9988-220">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="c9988-221">它可以安裝到目的電腦上的憑證存放區，或部署為具有強式密碼的 *.pfx*檔案。</span><span class="sxs-lookup"><span data-stu-id="c9988-221">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-app-service"></a><span data-ttu-id="c9988-222">範例：部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c9988-222">Example: Deploy to Azure App Service</span></span>

<span data-ttu-id="c9988-223">本節說明如何使用儲存在憑證存放區中的憑證，將應用程式部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="c9988-223">This section describes deploying the app to Azure App Service using a certificate stored in the certificate store.</span></span> <span data-ttu-id="c9988-224">若要修改應用程式以從憑證存放區載入憑證，當您在稍後步驟的 Azure 入口網站中設定應用程式時，需要標準層服務方案或更佳。</span><span class="sxs-lookup"><span data-stu-id="c9988-224">To modify the app to load a certificate from the certificate store, a Standard tier service plan or better is required when you configure the app in the Azure portal in a later step.</span></span>

<span data-ttu-id="c9988-225">在應用程式的*appsettings.json* file，修改 `IdentityServer` 區段以包含金鑰詳細資料：</span><span class="sxs-lookup"><span data-stu-id="c9988-225">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* <span data-ttu-id="c9988-226">存放區名稱代表儲存憑證之憑證存放區的名稱。</span><span class="sxs-lookup"><span data-stu-id="c9988-226">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="c9988-227">在此情況下，它會指向 [個人] 使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="c9988-227">In this case, it points to the personal user store.</span></span>
* <span data-ttu-id="c9988-228">存放區位置代表從（或）載入憑證的 `CurrentUser` 位置 `LocalMachine` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-228">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="c9988-229">憑證上的名稱屬性會對應到憑證的辨別主旨。</span><span class="sxs-lookup"><span data-stu-id="c9988-229">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>

<span data-ttu-id="c9988-230">若要部署至 Azure App Service，請遵循將[應用程式部署至 Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)中的步驟，其中說明如何建立必要的 azure 資源，並將應用程式部署至生產環境。</span><span class="sxs-lookup"><span data-stu-id="c9988-230">To deploy to Azure App Service, follow the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure), which explains how to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="c9988-231">遵循上述指示之後，應用程式會部署至 Azure，但尚未運作。</span><span class="sxs-lookup"><span data-stu-id="c9988-231">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="c9988-232">應用程式所使用的憑證必須在 Azure 入口網站中設定。</span><span class="sxs-lookup"><span data-stu-id="c9988-232">The certificate used by the app must be configured in the Azure portal.</span></span> <span data-ttu-id="c9988-233">找出憑證的指紋，並遵循[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code)中所述的步驟。</span><span class="sxs-lookup"><span data-stu-id="c9988-233">Locate the thumbprint for the certificate and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="c9988-234">雖然這些步驟提及 SSL，但 Azure 入口網站中有一個**私人憑證**區段，您可以在其中上傳佈建的憑證以搭配應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="c9988-234">While these steps mention SSL, there's a **Private certificates** section in the Azure portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="c9988-235">在 Azure 入口網站中設定應用程式和應用程式的設定之後，請在入口網站中重新開機應用程式。</span><span class="sxs-lookup"><span data-stu-id="c9988-235">After configuring the app and the app's settings in the Azure portal, restart the app in the portal.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="c9988-236">其他設定選項</span><span class="sxs-lookup"><span data-stu-id="c9988-236">Other configuration options</span></span>

<span data-ttu-id="c9988-237">API 授權的支援建置於具有一組慣例、預設值和增強功能的 IdentityServer 之上，以簡化 Spa 的體驗。</span><span class="sxs-lookup"><span data-stu-id="c9988-237">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="c9988-238">不用說，如果 ASP.NET Core 整合不會涵蓋您的案例，IdentityServer 的完整功能就會在幕後提供。</span><span class="sxs-lookup"><span data-stu-id="c9988-238">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="c9988-239">ASP.NET Core 支援著重于「第一方」應用程式，其中的所有應用程式都是由我們的組織建立和部署。</span><span class="sxs-lookup"><span data-stu-id="c9988-239">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="c9988-240">因此，不會針對同意或同盟等專案提供支援。</span><span class="sxs-lookup"><span data-stu-id="c9988-240">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="c9988-241">針對這些案例，請使用 IdentityServer 並遵循其檔。</span><span class="sxs-lookup"><span data-stu-id="c9988-241">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="c9988-242">應用程式佈建檔</span><span class="sxs-lookup"><span data-stu-id="c9988-242">Application profiles</span></span>

<span data-ttu-id="c9988-243">應用程式佈建檔是針對進一步定義其參數之應用程式預先定義的設定。</span><span class="sxs-lookup"><span data-stu-id="c9988-243">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="c9988-244">目前支援下列設定檔：</span><span class="sxs-lookup"><span data-stu-id="c9988-244">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="c9988-245">`IdentityServerSPA`：代表與 IdentityServer 一起裝載為單一單位的 SPA。</span><span class="sxs-lookup"><span data-stu-id="c9988-245">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="c9988-246">`redirect_uri`預設為 `/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-246">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="c9988-247">`post_logout_redirect_uri`預設為 `/authentication/logout-callback` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-247">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="c9988-248">一組範圍包含 `openid` 、 `profile` ，以及為應用程式中的 api 定義的每個範圍。</span><span class="sxs-lookup"><span data-stu-id="c9988-248">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="c9988-249">一組允許的 OIDC 回應類型為 `id_token token` 或個別（ `id_token` 、 `token` ）。</span><span class="sxs-lookup"><span data-stu-id="c9988-249">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="c9988-250">允許的回應模式為 `fragment` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-250">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="c9988-251">`SPA`：代表不是以 IdentityServer 裝載的 SPA。</span><span class="sxs-lookup"><span data-stu-id="c9988-251">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="c9988-252">一組範圍包含 `openid` 、 `profile` ，以及為應用程式中的 api 定義的每個範圍。</span><span class="sxs-lookup"><span data-stu-id="c9988-252">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="c9988-253">一組允許的 OIDC 回應類型為 `id_token token` 或個別（ `id_token` 、 `token` ）。</span><span class="sxs-lookup"><span data-stu-id="c9988-253">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="c9988-254">允許的回應模式為 `fragment` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-254">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="c9988-255">`IdentityServerJwt`：代表與 IdentityServer 一起裝載的 API。</span><span class="sxs-lookup"><span data-stu-id="c9988-255">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="c9988-256">應用程式已設定為具有單一範圍，預設為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="c9988-256">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="c9988-257">`API`：代表不是以 IdentityServer 裝載的 API。</span><span class="sxs-lookup"><span data-stu-id="c9988-257">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="c9988-258">應用程式已設定為具有單一範圍，預設為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="c9988-258">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="c9988-259">透過 AppSettings 設定</span><span class="sxs-lookup"><span data-stu-id="c9988-259">Configuration through AppSettings</span></span>

<span data-ttu-id="c9988-260">藉由將應用程式新增至或的清單，以設定該系統上的應用程式 `Clients` `Resources` 。</span><span class="sxs-lookup"><span data-stu-id="c9988-260">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="c9988-261">設定每個用戶端的 `redirect_uri` 和 `post_logout_redirect_uri` 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="c9988-261">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

<span data-ttu-id="c9988-262">設定資源時，您可以設定資源的範圍，如下所示：</span><span class="sxs-lookup"><span data-stu-id="c9988-262">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="c9988-263">透過程式碼設定</span><span class="sxs-lookup"><span data-stu-id="c9988-263">Configuration through code</span></span>

<span data-ttu-id="c9988-264">您也可以使用的多載 `AddApiAuthorization` （會採取動作來設定選項），透過程式碼來設定用戶端和資源。</span><span class="sxs-lookup"><span data-stu-id="c9988-264">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a><span data-ttu-id="c9988-265">其他資源</span><span class="sxs-lookup"><span data-stu-id="c9988-265">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:security/authentication/scaffold-identity>
