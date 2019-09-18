---
title: ASP.NET Core 上單一頁面應用程式的驗證簡介
author: javiercn
description: 將身分識別與裝載于 ASP.NET Core 應用程式內的單一頁面應用程式搭配使用。
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/05/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4f6e3a4922c0a8a74b0e13edf1f00fe5f7bb76ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082323"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="80759-103">Spa 的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="80759-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="80759-104">ASP.NET Core 3.0 或更新版本使用 API 授權的支援，在單一頁面應用程式（Spa）中提供驗證。</span><span class="sxs-lookup"><span data-stu-id="80759-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="80759-105">用於驗證和儲存使用者的 ASP.NET Core 身分識別會與[IdentityServer](https://identityserver.io/)結合，以執行 Open ID Connect。</span><span class="sxs-lookup"><span data-stu-id="80759-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="80759-106">驗證參數已加入至「**角度**」和「**回應**」專案範本，類似于**Web 應用程式（模型-視圖控制器）** （MVC）和**web 應用程式**（Razor Pages）中的驗證參數專案範本。</span><span class="sxs-lookup"><span data-stu-id="80759-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="80759-107">允許的參數值為 [**無**] 和 [**個人**]。</span><span class="sxs-lookup"><span data-stu-id="80759-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="80759-108">[**回應 .js] 和 [Redux** ] 專案範本目前不支援 [驗證] 參數。</span><span class="sxs-lookup"><span data-stu-id="80759-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="80759-109">建立具有 API 授權支援的應用程式</span><span class="sxs-lookup"><span data-stu-id="80759-109">Create an app with API authorization support</span></span>

<span data-ttu-id="80759-110">使用者驗證和授權可以同時用於「角度」和「回應」 Spa。</span><span class="sxs-lookup"><span data-stu-id="80759-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="80759-111">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="80759-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="80759-112">**角度**：</span><span class="sxs-lookup"><span data-stu-id="80759-112">**Angular**:</span></span>

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="80759-113">**回應**：</span><span class="sxs-lookup"><span data-stu-id="80759-113">**React**:</span></span>

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="80759-114">上述命令會建立一個 ASP.NET Core 應用程式，其中具有包含 SPA 的*ClientApp*目錄。</span><span class="sxs-lookup"><span data-stu-id="80759-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="80759-115">應用程式 ASP.NET Core 元件的一般描述</span><span class="sxs-lookup"><span data-stu-id="80759-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="80759-116">下列各節說明當包含驗證支援時，專案的新增功能：</span><span class="sxs-lookup"><span data-stu-id="80759-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="80759-117">啟始類別</span><span class="sxs-lookup"><span data-stu-id="80759-117">Startup class</span></span>

<span data-ttu-id="80759-118">`Startup`類別具有下列新增專案：</span><span class="sxs-lookup"><span data-stu-id="80759-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="80759-119">`Startup.ConfigureServices`在方法內：</span><span class="sxs-lookup"><span data-stu-id="80759-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="80759-120">使用預設 UI 的身分識別：</span><span class="sxs-lookup"><span data-stu-id="80759-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="80759-121">使用額外`AddApiAuthorization`的 helper 方法 IdentityServer，以在 IdentityServer 上提供一些預設的 ASP.NET Core 慣例：</span><span class="sxs-lookup"><span data-stu-id="80759-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="80759-122">使用其他`AddIdentityServerJwt` helper 方法進行驗證，以設定應用程式來驗證 IdentityServer 所產生的 JWT 權杖：</span><span class="sxs-lookup"><span data-stu-id="80759-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="80759-123">`Startup.Configure`在方法內：</span><span class="sxs-lookup"><span data-stu-id="80759-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="80759-124">驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：</span><span class="sxs-lookup"><span data-stu-id="80759-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="80759-125">公開 Open ID Connect 端點的 IdentityServer 中介軟體：</span><span class="sxs-lookup"><span data-stu-id="80759-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="80759-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="80759-126">AddApiAuthorization</span></span>

<span data-ttu-id="80759-127">此 helper 方法會將 IdentityServer 設定為使用我們支援的設定。</span><span class="sxs-lookup"><span data-stu-id="80759-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="80759-128">IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。</span><span class="sxs-lookup"><span data-stu-id="80759-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="80759-129">同時，會在最常見的案例中公開不必要的複雜性。</span><span class="sxs-lookup"><span data-stu-id="80759-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="80759-130">因此，系統會為您提供一組慣例和設定選項，並將其視為良好的起點。</span><span class="sxs-lookup"><span data-stu-id="80759-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="80759-131">一旦您的驗證需要變更，IdentityServer 的完整功能仍可供自訂驗證以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="80759-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="80759-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="80759-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="80759-133">此 helper 方法會將應用程式的原則配置設定為預設驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="80759-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="80759-134">原則會設定為讓身分識別處理路由傳送至身分識別 URL 空間 "/Identity" 中任何子路徑的所有要求。</span><span class="sxs-lookup"><span data-stu-id="80759-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="80759-135">會`JwtBearerHandler`處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="80759-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="80759-136">此外，此方法會向`<<ApplicationName>>API` IdentityServer 註冊具有預設範圍的`<<ApplicationName>>API` API 資源，並設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="80759-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="80759-137">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="80759-137">WeatherForecastController</span></span>

<span data-ttu-id="80759-138">在*Controllers\WeatherForecastController.cs*檔案中，請注意`[Authorize]`套用至類別的屬性，表示使用者必須根據預設原則來存取資源。</span><span class="sxs-lookup"><span data-stu-id="80759-138">In the *Controllers\WeatherForecastController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="80759-139">預設的授權原則會設定為使用預設的驗證配置，而這是由`AddIdentityServerJwt`先前所述的原則配置所設定， `JwtBearerHandler`讓這類 helper 方法為預設的處理常式設定對應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="80759-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="80759-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="80759-140">ApplicationDbContext</span></span>

<span data-ttu-id="80759-141">在*Data\ApplicationDbCoNtext.cs*檔案中，請注意， `DbContext`識別中會使用相同的，其延伸`ApiAuthorizationDbContext`的例外狀況（衍生自`IdentityDbContext`的類別）會包含 IdentityServer 的架構。</span><span class="sxs-lookup"><span data-stu-id="80759-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="80759-142">若要取得資料庫架構的完整控制權，請從其中一個可用的身分`DbContext`識別類別繼承，然後透過在`OnModelCreating`方法上呼叫`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`來設定內容以包含身分識別架構。</span><span class="sxs-lookup"><span data-stu-id="80759-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="80759-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="80759-143">OidcConfigurationController</span></span>

<span data-ttu-id="80759-144">在*Controllers\OidcConfigurationController.cs*檔案中，請注意布建的端點可提供用戶端所需使用的 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="80759-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="80759-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="80759-145">appsettings.json</span></span>

<span data-ttu-id="80759-146">在專案根目錄的*appsettings*中，有一個新`IdentityServer`的區段描述已設定的用戶端清單。</span><span class="sxs-lookup"><span data-stu-id="80759-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="80759-147">在下列範例中，有一個用戶端。</span><span class="sxs-lookup"><span data-stu-id="80759-147">In the following example, there's a single client.</span></span> <span data-ttu-id="80759-148">用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId`參數。</span><span class="sxs-lookup"><span data-stu-id="80759-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="80759-149">此設定檔會指出正在設定的應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="80759-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="80759-150">它是在內部用來驅動慣例，以簡化伺服器的設定程式。</span><span class="sxs-lookup"><span data-stu-id="80759-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="80759-151">有數個設定檔可供使用，如[應用程式佈建檔](#application-profiles)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="80759-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="80759-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="80759-152">appsettings.Development.json</span></span>

<span data-ttu-id="80759-153">在*appsettings 中。* 專案根目錄的開發 json 檔案，其中有一個`IdentityServer`區段描述用來簽署權杖的金鑰。</span><span class="sxs-lookup"><span data-stu-id="80759-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="80759-154">部署至生產環境時，必須隨應用程式一起布建和部署金鑰，如[部署至生產](#deploy-to-production)一節中所述。</span><span class="sxs-lookup"><span data-stu-id="80759-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="80759-155">角度應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="80759-155">General description of the Angular app</span></span>

<span data-ttu-id="80759-156">角度範本中的驗證和 API 授權支援位於*ClientApp\src\api-authorization*目錄中自己的角度模組。</span><span class="sxs-lookup"><span data-stu-id="80759-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="80759-157">模組是由下列元素所組成：</span><span class="sxs-lookup"><span data-stu-id="80759-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="80759-158">3個元件：</span><span class="sxs-lookup"><span data-stu-id="80759-158">3 components:</span></span>
  * <span data-ttu-id="80759-159">*login. component. ts*：處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="80759-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="80759-160">*登出。 component. ts*：處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="80759-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="80759-161">*登入-功能表。 元件. ts*：顯示下列其中一組連結的 widget：</span><span class="sxs-lookup"><span data-stu-id="80759-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="80759-162">使用者設定檔管理，並在使用者經過驗證時登出連結。</span><span class="sxs-lookup"><span data-stu-id="80759-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="80759-163">當使用者未經過驗證時，註冊並登入連結。</span><span class="sxs-lookup"><span data-stu-id="80759-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="80759-164">可以新增至`AuthorizeGuard`路由的路由防護，並要求使用者在造訪路由之前進行驗證。</span><span class="sxs-lookup"><span data-stu-id="80759-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="80759-165">HTTP 攔截`AuthorizeInterceptor`器，會在使用者通過驗證時，將存取權杖附加至以 API 為目標的傳出 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="80759-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="80759-166">此服務`AuthorizeService`會處理驗證程式的較低層級詳細資料，並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。</span><span class="sxs-lookup"><span data-stu-id="80759-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="80759-167">一個角度模組，定義與應用程式驗證部分相關聯的路由。</span><span class="sxs-lookup"><span data-stu-id="80759-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="80759-168">它會公開登入功能表元件、攔截器、防護和服務，以便從應用程式的其餘部分取用。</span><span class="sxs-lookup"><span data-stu-id="80759-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="80759-169">回應應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="80759-169">General description of the React app</span></span>

<span data-ttu-id="80759-170">回應範本中的驗證和 API 授權支援位於*ClientApp\src\components\api-authorization*目錄中。</span><span class="sxs-lookup"><span data-stu-id="80759-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="80759-171">它是由下列元素所組成：</span><span class="sxs-lookup"><span data-stu-id="80759-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="80759-172">4個元件：</span><span class="sxs-lookup"><span data-stu-id="80759-172">4 components:</span></span>
  * <span data-ttu-id="80759-173">*Login .js*：處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="80759-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="80759-174">*登出 .js*：處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="80759-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="80759-175">*LoginMenu .js*：顯示下列其中一組連結的 widget：</span><span class="sxs-lookup"><span data-stu-id="80759-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="80759-176">使用者設定檔管理，並在使用者經過驗證時登出連結。</span><span class="sxs-lookup"><span data-stu-id="80759-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="80759-177">當使用者未經過驗證時，註冊並登入連結。</span><span class="sxs-lookup"><span data-stu-id="80759-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="80759-178">*AuthorizeRoute .js*：需要先驗證使用者，然後才呈現`Component`參數所指示之元件的路由元件。</span><span class="sxs-lookup"><span data-stu-id="80759-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="80759-179">已匯出`authService`的類別`AuthorizeService`實例，可處理較低層級的驗證程式詳細資料，並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。</span><span class="sxs-lookup"><span data-stu-id="80759-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="80759-180">既然您已瞭解解決方案的主要元件，您可以進一步瞭解應用程式的個別案例。</span><span class="sxs-lookup"><span data-stu-id="80759-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="80759-181">需要新 API 的授權</span><span class="sxs-lookup"><span data-stu-id="80759-181">Require authorization on a new API</span></span>

<span data-ttu-id="80759-182">根據預設，系統會設定為輕鬆地要求新 Api 的授權。</span><span class="sxs-lookup"><span data-stu-id="80759-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="80759-183">若要這麼做，請建立新的控制器， `[Authorize]`並將屬性新增至控制器類別或控制器內的任何動作。</span><span class="sxs-lookup"><span data-stu-id="80759-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="80759-184">保護用戶端路由（角度）</span><span class="sxs-lookup"><span data-stu-id="80759-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="80759-185">若要保護用戶端路由，請將授權防護新增至設定路由時要執行的防護清單。</span><span class="sxs-lookup"><span data-stu-id="80759-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="80759-186">例如，您可以在主要應用程式角度`fetch-data`模組中查看路由的設定方式：</span><span class="sxs-lookup"><span data-stu-id="80759-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="80759-187">請務必注意，保護路由並不會保護實際端點（仍然需要`[Authorize]`套用屬性），但它只會防止使用者在未驗證時流覽至指定的用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="80759-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="80759-188">驗證 API 要求（角度）</span><span class="sxs-lookup"><span data-stu-id="80759-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="80759-189">使用應用程式所定義的 HTTP 用戶端攔截器，來驗證與應用程式一起裝載之 Api 的要求會自動完成。</span><span class="sxs-lookup"><span data-stu-id="80759-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="80759-190">保護用戶端路由（回應）</span><span class="sxs-lookup"><span data-stu-id="80759-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="80759-191">使用`AuthorizeRoute`元件（而不是一般`Route`元件）來保護用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="80759-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="80759-192">例如，請注意如何`fetch-data`在`App`元件中設定路由：</span><span class="sxs-lookup"><span data-stu-id="80759-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="80759-193">保護路由：</span><span class="sxs-lookup"><span data-stu-id="80759-193">Protecting a route:</span></span>

* <span data-ttu-id="80759-194">不會保護實際的端點（仍然需要`[Authorize]`套用屬性）。</span><span class="sxs-lookup"><span data-stu-id="80759-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="80759-195">只有在未驗證時，才會防止使用者流覽至指定的用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="80759-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="80759-196">驗證 API 要求（回應）</span><span class="sxs-lookup"><span data-stu-id="80759-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="80759-197">藉由先`authService` `AuthorizeService`從匯入實例，來驗證具有回應的要求。</span><span class="sxs-lookup"><span data-stu-id="80759-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="80759-198">存取權杖是從`authService`抓取，並且附加至要求，如下所示。</span><span class="sxs-lookup"><span data-stu-id="80759-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="80759-199">在回應元件中，這項工作通常會在`componentDidMount`生命週期方法中完成，或做為某些使用者互動的結果。</span><span class="sxs-lookup"><span data-stu-id="80759-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="80759-200">將 authService 匯入您的元件</span><span class="sxs-lookup"><span data-stu-id="80759-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="80759-201">取得存取權杖，並將其附加至回應</span><span class="sxs-lookup"><span data-stu-id="80759-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="80759-202">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="80759-202">Deploy to production</span></span>

<span data-ttu-id="80759-203">若要將應用程式部署至生產環境，您必須布建下列資源：</span><span class="sxs-lookup"><span data-stu-id="80759-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="80759-204">用來儲存身分識別使用者帳戶和 IdentityServer 授與的資料庫。</span><span class="sxs-lookup"><span data-stu-id="80759-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="80759-205">用於簽署權杖的生產憑證。</span><span class="sxs-lookup"><span data-stu-id="80759-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="80759-206">此憑證沒有特定的需求;它可以是自我簽署憑證或透過 CA 授權單位布建的憑證。</span><span class="sxs-lookup"><span data-stu-id="80759-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="80759-207">您可以透過 PowerShell 或 OpenSSL 之類的標準工具來產生它。</span><span class="sxs-lookup"><span data-stu-id="80759-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="80759-208">它可以安裝到目的電腦上的憑證存放區，或部署為具有強式密碼的 *.pfx*檔案。</span><span class="sxs-lookup"><span data-stu-id="80759-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="80759-209">範例：部署至 Azure 網站</span><span class="sxs-lookup"><span data-stu-id="80759-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="80759-210">本節說明如何使用儲存在憑證存放區中的憑證，將應用程式部署到 Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="80759-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="80759-211">若要修改應用程式以從憑證存放區載入憑證，當您在後續步驟中設定時，App Service 方案必須至少在標準層。</span><span class="sxs-lookup"><span data-stu-id="80759-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="80759-212">在應用程式的*appsettings*中，修改`IdentityServer`區段以包含金鑰詳細資料：</span><span class="sxs-lookup"><span data-stu-id="80759-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="80759-213">憑證上的名稱屬性會對應到憑證的辨別主旨。</span><span class="sxs-lookup"><span data-stu-id="80759-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="80759-214">存放區位置代表從（`CurrentUser`或`LocalMachine`）載入憑證的位置。</span><span class="sxs-lookup"><span data-stu-id="80759-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="80759-215">存放區名稱代表儲存憑證之憑證存放區的名稱。</span><span class="sxs-lookup"><span data-stu-id="80759-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="80759-216">在此情況下，它會指向 [個人] 使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="80759-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="80759-217">若要部署至 Azure 網站，請遵循將[應用程式部署至 azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)中的步驟來部署應用程式，以建立必要的 Azure 資源，並將應用程式部署至生產環境。</span><span class="sxs-lookup"><span data-stu-id="80759-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="80759-218">遵循上述指示之後，應用程式會部署至 Azure，但尚未運作。</span><span class="sxs-lookup"><span data-stu-id="80759-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="80759-219">應用程式所使用的憑證仍然需要設定。</span><span class="sxs-lookup"><span data-stu-id="80759-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="80759-220">找出要使用的憑證指紋，並遵循[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code)中所述的步驟。</span><span class="sxs-lookup"><span data-stu-id="80759-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="80759-221">雖然這些步驟提到 SSL，但入口網站上有 [**私人憑證**] 區段，您可以在其中上傳佈建的憑證以搭配應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="80759-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="80759-222">在此步驟之後，請重新開機應用程式，它應該會正常運作。</span><span class="sxs-lookup"><span data-stu-id="80759-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="80759-223">其他設定選項</span><span class="sxs-lookup"><span data-stu-id="80759-223">Other configuration options</span></span>

<span data-ttu-id="80759-224">API 授權的支援建置於具有一組慣例、預設值和增強功能的 IdentityServer 之上，以簡化 Spa 的體驗。</span><span class="sxs-lookup"><span data-stu-id="80759-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="80759-225">不用說，如果 ASP.NET Core 整合不會涵蓋您的案例，IdentityServer 的完整功能就會在幕後提供。</span><span class="sxs-lookup"><span data-stu-id="80759-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="80759-226">ASP.NET Core 支援著重于「第一方」應用程式，其中的所有應用程式都是由我們的組織建立和部署。</span><span class="sxs-lookup"><span data-stu-id="80759-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="80759-227">因此，不會針對同意或同盟等專案提供支援。</span><span class="sxs-lookup"><span data-stu-id="80759-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="80759-228">針對這些案例，請使用 IdentityServer 並遵循其檔。</span><span class="sxs-lookup"><span data-stu-id="80759-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="80759-229">應用程式佈建檔</span><span class="sxs-lookup"><span data-stu-id="80759-229">Application profiles</span></span>

<span data-ttu-id="80759-230">應用程式佈建檔是針對進一步定義其參數之應用程式預先定義的設定。</span><span class="sxs-lookup"><span data-stu-id="80759-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="80759-231">目前支援下列設定檔：</span><span class="sxs-lookup"><span data-stu-id="80759-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="80759-232">`IdentityServerSPA`：表示與 IdentityServer 一起裝載為單一單位的 SPA。</span><span class="sxs-lookup"><span data-stu-id="80759-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="80759-233">`redirect_uri`預設為`/authentication/login-callback`。</span><span class="sxs-lookup"><span data-stu-id="80759-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="80759-234">`post_logout_redirect_uri`預設為`/authentication/logout-callback`。</span><span class="sxs-lookup"><span data-stu-id="80759-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="80759-235">一組範圍包含`openid`、 `profile`，以及為應用程式中的 api 定義的每個範圍。</span><span class="sxs-lookup"><span data-stu-id="80759-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="80759-236">一組允許的 OIDC 回應類型為`id_token token`或個別（`id_token`、 `token`）。</span><span class="sxs-lookup"><span data-stu-id="80759-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="80759-237">允許的回應模式為`fragment`。</span><span class="sxs-lookup"><span data-stu-id="80759-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="80759-238">`SPA`：代表不是以 IdentityServer 裝載的 SPA。</span><span class="sxs-lookup"><span data-stu-id="80759-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="80759-239">一組範圍包含`openid`、 `profile`，以及為應用程式中的 api 定義的每個範圍。</span><span class="sxs-lookup"><span data-stu-id="80759-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="80759-240">一組允許的 OIDC 回應類型為`id_token token`或個別（`id_token`、 `token`）。</span><span class="sxs-lookup"><span data-stu-id="80759-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="80759-241">允許的回應模式為`fragment`。</span><span class="sxs-lookup"><span data-stu-id="80759-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="80759-242">`IdentityServerJwt`：表示與 IdentityServer 一起裝載的 API。</span><span class="sxs-lookup"><span data-stu-id="80759-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="80759-243">應用程式已設定為具有單一範圍，預設為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="80759-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="80759-244">`API`：代表不是以 IdentityServer 裝載的 API。</span><span class="sxs-lookup"><span data-stu-id="80759-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="80759-245">應用程式已設定為具有單一範圍，預設為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="80759-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="80759-246">透過 AppSettings 設定</span><span class="sxs-lookup"><span data-stu-id="80759-246">Configuration through AppSettings</span></span>

<span data-ttu-id="80759-247">藉由將應用程式新增至`Clients`或`Resources`的清單，以設定該系統上的應用程式。</span><span class="sxs-lookup"><span data-stu-id="80759-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="80759-248">設定每個客戶`redirect_uri`端`post_logout_redirect_uri`的和屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="80759-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="80759-249">設定資源時，您可以設定資源的範圍，如下所示：</span><span class="sxs-lookup"><span data-stu-id="80759-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="80759-250">透過程式碼設定</span><span class="sxs-lookup"><span data-stu-id="80759-250">Configuration through code</span></span>

<span data-ttu-id="80759-251">您也可以使用的多載`AddApiAuthorization` （會採取動作來設定選項），透過程式碼來設定用戶端和資源。</span><span class="sxs-lookup"><span data-stu-id="80759-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="80759-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="80759-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:security/authentication/scaffold-identity>
