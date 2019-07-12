---
title: 在 ASP.NET Core 上的單一頁面應用程式的驗證簡介
author: javiercn
description: 使用單一頁面應用程式裝載 ASP.NET Core 應用程式內使用身分識別。
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 302a5e10a70e40e75ab9fe4b3e5a98c4e847b822
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815224"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="9cba8-103">Spa 的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="9cba8-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="9cba8-104">3\.0 或更新版本的 ASP.NET Core 可提供在單一頁面應用程式 (Spa) 中的驗證 API 授權使用的支援。</span><span class="sxs-lookup"><span data-stu-id="9cba8-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="9cba8-105">針對驗證和儲存使用者的 ASP.NET Core Identity 結合[IdentityServer](https://identityserver.io/)實作 Open ID Connect。</span><span class="sxs-lookup"><span data-stu-id="9cba8-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="9cba8-106">驗證參數已新增至**Angular**並**React**專案範本中的驗證參數的類似**Web 應用程式 （模型-檢視-控制器）** (MVC) 和**Web 應用程式**（Razor 頁面） 專案範本。</span><span class="sxs-lookup"><span data-stu-id="9cba8-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="9cba8-107">允許的參數值為**無**並**個別**。</span><span class="sxs-lookup"><span data-stu-id="9cba8-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="9cba8-108">**React.js 與 Redux**專案範本不支援這一次的驗證參數。</span><span class="sxs-lookup"><span data-stu-id="9cba8-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="9cba8-109">建立授權的 API 支援的應用程式</span><span class="sxs-lookup"><span data-stu-id="9cba8-109">Create an app with API authorization support</span></span>

<span data-ttu-id="9cba8-110">使用 Angular 和 React Spa 可用使用者驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="9cba8-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="9cba8-111">開啟命令殼層中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9cba8-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="9cba8-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="9cba8-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="9cba8-113">**React**:</span><span class="sxs-lookup"><span data-stu-id="9cba8-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="9cba8-114">上述命令會建立 ASP.NET Core 應用程式，但*ClientApp* SPA 的所在目錄。</span><span class="sxs-lookup"><span data-stu-id="9cba8-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="9cba8-115">ASP.NET Core 應用程式的元件的一般描述</span><span class="sxs-lookup"><span data-stu-id="9cba8-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="9cba8-116">驗證支援包含時，下列章節將描述新增至專案：</span><span class="sxs-lookup"><span data-stu-id="9cba8-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="9cba8-117">啟動類別</span><span class="sxs-lookup"><span data-stu-id="9cba8-117">Startup class</span></span>

<span data-ttu-id="9cba8-118">`Startup`類別具有下列各項：</span><span class="sxs-lookup"><span data-stu-id="9cba8-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="9cba8-119">內`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="9cba8-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="9cba8-120">使用預設 UI 身分識別：</span><span class="sxs-lookup"><span data-stu-id="9cba8-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="9cba8-121">加上一個 IdentityServer `AddApiAuthorization` helper 方法，該設定一些預設 IdentityServer 之上的 ASP.NET Core 慣例：</span><span class="sxs-lookup"><span data-stu-id="9cba8-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="9cba8-122">有額外的驗證`AddIdentityServerJwt`IdentityServer 所產生的設定來驗證 JWT 權杖的應用程式的協助程式方法：</span><span class="sxs-lookup"><span data-stu-id="9cba8-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="9cba8-123">內`Startup.Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="9cba8-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="9cba8-124">驗證中介軟體負責驗證要求的認證和設定使用者的要求內容：</span><span class="sxs-lookup"><span data-stu-id="9cba8-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="9cba8-125">IdentityServer 中介軟體所公開的 Open ID Connect 端點：</span><span class="sxs-lookup"><span data-stu-id="9cba8-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="9cba8-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="9cba8-126">AddApiAuthorization</span></span>

<span data-ttu-id="9cba8-127">這個 helper 方法會設定 IdentityServer 使用我們的支援的組態。</span><span class="sxs-lookup"><span data-stu-id="9cba8-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="9cba8-128">IdentityServer 是一種功能強大且可擴充的架構，來處理應用程式的安全性考量。</span><span class="sxs-lookup"><span data-stu-id="9cba8-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="9cba8-129">在此同時，會公開不必要的複雜度，針對最常見的案例。</span><span class="sxs-lookup"><span data-stu-id="9cba8-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="9cba8-130">因此，一組慣例和組態選項被提供給您認為是不錯的起點。</span><span class="sxs-lookup"><span data-stu-id="9cba8-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="9cba8-131">一旦您的驗證需求變更，IdentityServer 的完整功能是仍可自訂以符合您需求的驗證。</span><span class="sxs-lookup"><span data-stu-id="9cba8-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="9cba8-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="9cba8-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="9cba8-133">這個 helper 方法會將應用程式的原則配置設定為預設的驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="9cba8-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="9cba8-134">設定此原則，讓處理所有要求路由傳送至任何子路徑的識別 URL 空間中的身分識別 」 / 身分識別 」。</span><span class="sxs-lookup"><span data-stu-id="9cba8-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="9cba8-135">`JwtBearerHandler`會處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="9cba8-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="9cba8-136">此外，這個方法會註冊`<<ApplicationName>>API`的預設範圍的 API 資源與 IdentityServer`<<ApplicationName>>API`並設定 JWT Bearer token 中介軟體驗證的應用程式的 IdentityServer 所簽發的權杖。</span><span class="sxs-lookup"><span data-stu-id="9cba8-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="9cba8-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="9cba8-137">SampleDataController</span></span>

<span data-ttu-id="9cba8-138">在  *Controllers\SampleDataController.cs*檔案中，請注意`[Authorize]`屬性套用至類別，表示使用者必須獲得授權以存取資源的預設原則。</span><span class="sxs-lookup"><span data-stu-id="9cba8-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="9cba8-139">預設的授權原則會設定為使用預設的驗證配置，它可設定的設定`AddIdentityServerJwt`上面所提及，讓原則配置`JwtBearerHandler`這類的協助程式方法所設定的預設處理常式應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="9cba8-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="9cba8-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="9cba8-140">ApplicationDbContext</span></span>

<span data-ttu-id="9cba8-141">在*Data\ApplicationDbContext.cs*檔案中，請注意，相同`DbContext`有一點除外，它會擴充用於身分識別`ApiAuthorizationDbContext`(更多衍生類別`IdentityDbContext`) 結構描述包含如 IdentityServer。</span><span class="sxs-lookup"><span data-stu-id="9cba8-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="9cba8-142">若要取得的資料庫結構描述的完整控制權，繼承自其中一個可用的身分識別`DbContext`類別，並設定要藉由呼叫包含身分識別結構描述的內容`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`上`OnModelCreating`方法。</span><span class="sxs-lookup"><span data-stu-id="9cba8-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="9cba8-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="9cba8-143">OidcConfigurationController</span></span>

<span data-ttu-id="9cba8-144">在  *Controllers\OidcConfigurationController.cs*檔案中，請注意已佈建服務用戶端要使用的 OIDC 參數為端點。</span><span class="sxs-lookup"><span data-stu-id="9cba8-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9cba8-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="9cba8-145">appsettings.json</span></span>

<span data-ttu-id="9cba8-146">在  *appsettings.json*檔案的專案根目錄中，沒有新`IdentityServer`區段描述的清單，設定用戶端。</span><span class="sxs-lookup"><span data-stu-id="9cba8-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="9cba8-147">在下列範例中，沒有單一的用戶端。</span><span class="sxs-lookup"><span data-stu-id="9cba8-147">In the following example, there's a single client.</span></span> <span data-ttu-id="9cba8-148">用戶端名稱對應至應用程式名稱和對應的慣例，以 OAuth`ClientId`參數。</span><span class="sxs-lookup"><span data-stu-id="9cba8-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="9cba8-149">設定檔會指出正在設定應用程式型別。</span><span class="sxs-lookup"><span data-stu-id="9cba8-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="9cba8-150">它是在內部用來簡化伺服器的設定程序的磁碟機慣例。</span><span class="sxs-lookup"><span data-stu-id="9cba8-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="9cba8-151">有數個設定檔，如所述[應用程式設定檔](#application-profiles)一節。</span><span class="sxs-lookup"><span data-stu-id="9cba8-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="9cba8-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="9cba8-152">appsettings.Development.json</span></span>

<span data-ttu-id="9cba8-153">在  *appsettings。Development.json*檔案的專案根目錄中，有`IdentityServer`區段，其中描述用來簽署權杖的金鑰。</span><span class="sxs-lookup"><span data-stu-id="9cba8-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="9cba8-154">部署到生產環境時，金鑰必須佈建，並與應用程式一起部署中所述[部署至生產環境](#deploy-to-production)一節。</span><span class="sxs-lookup"><span data-stu-id="9cba8-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="9cba8-155">在 Angular 應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="9cba8-155">General description of the Angular app</span></span>

<span data-ttu-id="9cba8-156">驗證和授權的 API 支援在 Angular 範本位於它自己 Angular 模組*ClientApp\src\api 授權*目錄。</span><span class="sxs-lookup"><span data-stu-id="9cba8-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="9cba8-157">模組是由下列項目所組成：</span><span class="sxs-lookup"><span data-stu-id="9cba8-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="9cba8-158">3 個元件：</span><span class="sxs-lookup"><span data-stu-id="9cba8-158">3 components:</span></span>
  * <span data-ttu-id="9cba8-159">*login.component.ts*:處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="9cba8-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="9cba8-160">*logout.component.ts*:處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="9cba8-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="9cba8-161">*登入 menu.component.ts*:一種 widget，顯示其中一個連結的下列設定：</span><span class="sxs-lookup"><span data-stu-id="9cba8-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="9cba8-162">管理使用者設定檔和記錄參考連結時驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="9cba8-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="9cba8-163">註冊和登入連結，當使用者未通過驗證。</span><span class="sxs-lookup"><span data-stu-id="9cba8-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="9cba8-164">路由 guard `AuthorizeGuard` ，可以新增至路由，並要求使用者必須經過驗證才能瀏覽的路由。</span><span class="sxs-lookup"><span data-stu-id="9cba8-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="9cba8-165">HTTP 攔截器`AuthorizeInterceptor`，將存取權杖附加至傳出 HTTP 要求以 API 為目標，當使用者通過驗證。</span><span class="sxs-lookup"><span data-stu-id="9cba8-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="9cba8-166">服務`AuthorizeService`，處理驗證程序的較低層級詳細資料，並會顯示有關已驗證的使用者取用的應用程式的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="9cba8-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="9cba8-167">定義應用程式的驗證部分相關聯的路由 Angular 模組。</span><span class="sxs-lookup"><span data-stu-id="9cba8-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="9cba8-168">它會公開登入 功能表元件、 攔截器、 成立條件和應用程式的其餘部分的耗用量的服務。</span><span class="sxs-lookup"><span data-stu-id="9cba8-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="9cba8-169">React 應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="9cba8-169">General description of the React app</span></span>

<span data-ttu-id="9cba8-170">進行驗證和 React 範本中的 API 授權支援位於*ClientApp\src\components\api 授權*目錄。</span><span class="sxs-lookup"><span data-stu-id="9cba8-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="9cba8-171">它是由下列項目所組成：</span><span class="sxs-lookup"><span data-stu-id="9cba8-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="9cba8-172">4 個元件：</span><span class="sxs-lookup"><span data-stu-id="9cba8-172">4 components:</span></span>
  * <span data-ttu-id="9cba8-173">*Login.js*:處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="9cba8-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="9cba8-174">*Logout.js*:處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="9cba8-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="9cba8-175">*LoginMenu.js*:一種 widget，顯示其中一個連結的下列設定：</span><span class="sxs-lookup"><span data-stu-id="9cba8-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="9cba8-176">管理使用者設定檔和記錄參考連結時驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="9cba8-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="9cba8-177">註冊和登入連結，當使用者未通過驗證。</span><span class="sxs-lookup"><span data-stu-id="9cba8-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="9cba8-178">*AuthorizeRoute.js*:要求使用者必須經過驗證才能轉譯元件的路由元件所示`Component`參數。</span><span class="sxs-lookup"><span data-stu-id="9cba8-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="9cba8-179">匯出`authService`類別的執行個體`AuthorizeService`，處理驗證程序的較低層級詳細資料，並會顯示有關已驗證的使用者取用的應用程式的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="9cba8-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="9cba8-180">既然您已了解解決方案的主要元件，您可能需要更深入的了解應用程式的個別案例。</span><span class="sxs-lookup"><span data-stu-id="9cba8-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="9cba8-181">需要新的 API 上授權</span><span class="sxs-lookup"><span data-stu-id="9cba8-181">Require authorization on a new API</span></span>

<span data-ttu-id="9cba8-182">根據預設，系統會設定可在新的 Api 輕鬆地要求授權。</span><span class="sxs-lookup"><span data-stu-id="9cba8-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="9cba8-183">若要這樣做，請建立新的控制器，並新增`[Authorize]`屬性至控制器類別或控制器內的任何動作。</span><span class="sxs-lookup"><span data-stu-id="9cba8-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="9cba8-184">保護用戶端路由 (Angular)</span><span class="sxs-lookup"><span data-stu-id="9cba8-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="9cba8-185">保護用戶端的路由，即可將授權成立條件新增至設定的路由時若要執行的成立條件清單。</span><span class="sxs-lookup"><span data-stu-id="9cba8-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="9cba8-186">例如，您可以看到如何`fetch-data`路由設定主應用程式的 Angular 模組：</span><span class="sxs-lookup"><span data-stu-id="9cba8-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="9cba8-187">務必要提到保護路由不會保護實際的端點 (仍需要`[Authorize]`套用屬性)，但它只會防止使用者未通過驗證時，瀏覽至指定的用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="9cba8-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="9cba8-188">驗證 API 要求 (Angular)</span><span class="sxs-lookup"><span data-stu-id="9cba8-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="9cba8-189">驗證應用程式一同裝載的 Api 的要求會自動完成透過 HTTP 的用戶端攔截器定義應用程式所使用。</span><span class="sxs-lookup"><span data-stu-id="9cba8-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="9cba8-190">保護用戶端路由 (React)</span><span class="sxs-lookup"><span data-stu-id="9cba8-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="9cba8-191">使用保護的用戶端路由`AuthorizeRoute`元件，而不是純`Route`元件。</span><span class="sxs-lookup"><span data-stu-id="9cba8-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="9cba8-192">例如，請注意如何`fetch-data`內設定路由`App`元件：</span><span class="sxs-lookup"><span data-stu-id="9cba8-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="9cba8-193">保護路由：</span><span class="sxs-lookup"><span data-stu-id="9cba8-193">Protecting a route:</span></span>

* <span data-ttu-id="9cba8-194">不會保護實際的端點 (仍需要`[Authorize]`套用屬性)。</span><span class="sxs-lookup"><span data-stu-id="9cba8-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="9cba8-195">只會防止使用者未通過驗證時，瀏覽至指定的用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="9cba8-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="9cba8-196">驗證 API 要求 (React)</span><span class="sxs-lookup"><span data-stu-id="9cba8-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="9cba8-197">驗證使用 React 的要求由第一次匯入`authService`執行個體`AuthorizeService`。</span><span class="sxs-lookup"><span data-stu-id="9cba8-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="9cba8-198">從擷取存取權杖`authService`並附加至要求，如下所示。</span><span class="sxs-lookup"><span data-stu-id="9cba8-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="9cba8-199">React 元件在這項工作通常是`componentDidMount`生命週期方法或某些使用者互動的結果。</span><span class="sxs-lookup"><span data-stu-id="9cba8-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="9cba8-200">AuthService 匯入您的元件</span><span class="sxs-lookup"><span data-stu-id="9cba8-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="9cba8-201">擷取，並將存取權杖附加至回應</span><span class="sxs-lookup"><span data-stu-id="9cba8-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="9cba8-202">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="9cba8-202">Deploy to production</span></span>

<span data-ttu-id="9cba8-203">若要將應用程式部署到生產環境中，下列資源需要將他們佈建：</span><span class="sxs-lookup"><span data-stu-id="9cba8-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="9cba8-204">若要儲存身分識別使用者帳戶和 IdentityServer 授與資料庫。</span><span class="sxs-lookup"><span data-stu-id="9cba8-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="9cba8-205">生產環境来使用的憑證來簽署權杖。</span><span class="sxs-lookup"><span data-stu-id="9cba8-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="9cba8-206">此憑證; 沒有特定的需求它可以是自我簽署的憑證或憑證，透過 CA 授權單位佈建。</span><span class="sxs-lookup"><span data-stu-id="9cba8-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="9cba8-207">可以透過 PowerShell 或 OpenSSL 等的標準工具產生。</span><span class="sxs-lookup"><span data-stu-id="9cba8-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="9cba8-208">它可以是安裝在目標電腦上的憑證存放區或部署為 *.pfx*強式密碼的檔案。</span><span class="sxs-lookup"><span data-stu-id="9cba8-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="9cba8-209">範例：部署到 Azure 網站</span><span class="sxs-lookup"><span data-stu-id="9cba8-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="9cba8-210">本章節描述應用程式部署至 Azure 網站使用憑證存放區中儲存的憑證。</span><span class="sxs-lookup"><span data-stu-id="9cba8-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="9cba8-211">若要修改應用程式，以從憑證存放區載入憑證，必須至少是在標準層時您在稍後步驟中設定 App Service 方案。</span><span class="sxs-lookup"><span data-stu-id="9cba8-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="9cba8-212">在應用程式之*appsettings.json*檔案中，修改`IdentityServer`區段以包含金鑰的詳細資料：</span><span class="sxs-lookup"><span data-stu-id="9cba8-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="9cba8-213">與憑證的辨別主旨，對應上憑證的 name 屬性。</span><span class="sxs-lookup"><span data-stu-id="9cba8-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="9cba8-214">存放區位置代表載入之憑證的位置 (`CurrentUser`或`LocalMachine`)。</span><span class="sxs-lookup"><span data-stu-id="9cba8-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="9cba8-215">存放區名稱代表憑證的儲存位置的憑證存放區的名稱。</span><span class="sxs-lookup"><span data-stu-id="9cba8-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="9cba8-216">在此情況下，它會指向個人使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="9cba8-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="9cba8-217">若要部署至 Azure 網站，部署應用程式中的步驟[將應用程式部署至 Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)建立所需的 Azure 資源，並將應用程式部署至生產環境。</span><span class="sxs-lookup"><span data-stu-id="9cba8-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="9cba8-218">遵循先前的指示之後, 應用程式部署至 Azure，但尚無法運作。</span><span class="sxs-lookup"><span data-stu-id="9cba8-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="9cba8-219">應用程式所使用的憑證仍然需要進行設定。</span><span class="sxs-lookup"><span data-stu-id="9cba8-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="9cba8-220">找出要使用憑證的指紋，並遵循所述的步驟[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code)。</span><span class="sxs-lookup"><span data-stu-id="9cba8-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="9cba8-221">雖然這些步驟提到 SSL，則**私用憑證**入口網站中的一節，您可以上傳要與應用程式所使用的佈建的憑證。</span><span class="sxs-lookup"><span data-stu-id="9cba8-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="9cba8-222">這個步驟之後，重新啟動應用程式，它應該才能運作。</span><span class="sxs-lookup"><span data-stu-id="9cba8-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="9cba8-223">其他組態選項</span><span class="sxs-lookup"><span data-stu-id="9cba8-223">Other configuration options</span></span>

<span data-ttu-id="9cba8-224">使用一組慣例，預設值，並簡化 Spa 的體驗的增強功能，IdentityServer 之上建置 API 授權的支援。</span><span class="sxs-lookup"><span data-stu-id="9cba8-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="9cba8-225">不用說，IdentityServer 的完整功能是可在幕後，如果 ASP.NET Core 整合不涵蓋您的案例。</span><span class="sxs-lookup"><span data-stu-id="9cba8-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="9cba8-226">ASP.NET Core 支援著重於 「 第一方 」 應用程式，建立並由我們的組織部署的所有應用程式。</span><span class="sxs-lookup"><span data-stu-id="9cba8-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="9cba8-227">此情況下，不提供支援，例如同意或同盟。</span><span class="sxs-lookup"><span data-stu-id="9cba8-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="9cba8-228">對於這些案例中，使用 IdentityServer 並遵循其文件。</span><span class="sxs-lookup"><span data-stu-id="9cba8-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="9cba8-229">應用程式設定檔</span><span class="sxs-lookup"><span data-stu-id="9cba8-229">Application profiles</span></span>

<span data-ttu-id="9cba8-230">應用程式設定檔是預先定義的組態，進一步定義其參數的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9cba8-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="9cba8-231">在此階段中，支援下列設定檔：</span><span class="sxs-lookup"><span data-stu-id="9cba8-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="9cba8-232">`IdentityServerSPA`：表示裝載在一起當做單一單位的 IdentityServer SPA。</span><span class="sxs-lookup"><span data-stu-id="9cba8-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="9cba8-233">`redirect_uri`預設為`/authentication/login-callback`。</span><span class="sxs-lookup"><span data-stu-id="9cba8-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="9cba8-234">`post_logout_redirect_uri`預設為`/authentication/logout-callback`。</span><span class="sxs-lookup"><span data-stu-id="9cba8-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="9cba8-235">包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。</span><span class="sxs-lookup"><span data-stu-id="9cba8-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="9cba8-236">允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。</span><span class="sxs-lookup"><span data-stu-id="9cba8-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="9cba8-237">允許的回應模式是`fragment`。</span><span class="sxs-lookup"><span data-stu-id="9cba8-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="9cba8-238">`SPA`：表示未裝載與 IdentityServer SPA。</span><span class="sxs-lookup"><span data-stu-id="9cba8-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="9cba8-239">包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。</span><span class="sxs-lookup"><span data-stu-id="9cba8-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="9cba8-240">允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。</span><span class="sxs-lookup"><span data-stu-id="9cba8-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="9cba8-241">允許的回應模式是`fragment`。</span><span class="sxs-lookup"><span data-stu-id="9cba8-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="9cba8-242">`IdentityServerJwt`：表示與 IdentityServer 裝載在一起的 API。</span><span class="sxs-lookup"><span data-stu-id="9cba8-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="9cba8-243">應用程式已設定為具有單一的範圍，預設值為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="9cba8-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="9cba8-244">`API`：表示未裝載與 IdentityServer 的 API。</span><span class="sxs-lookup"><span data-stu-id="9cba8-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="9cba8-245">應用程式已設定為具有單一的範圍，預設值為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="9cba8-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="9cba8-246">透過 AppSettings 組態</span><span class="sxs-lookup"><span data-stu-id="9cba8-246">Configuration through AppSettings</span></span>

<span data-ttu-id="9cba8-247">設定透過組態系統的應用程式新增至清單`Clients`或`Resources`。</span><span class="sxs-lookup"><span data-stu-id="9cba8-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="9cba8-248">設定每個用戶端`redirect_uri`和`post_logout_redirect_uri`屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9cba8-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="9cba8-249">在設定的資源時，您可以設定資源的範圍，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9cba8-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="9cba8-250">透過程式碼設定</span><span class="sxs-lookup"><span data-stu-id="9cba8-250">Configuration through code</span></span>

<span data-ttu-id="9cba8-251">您也可以設定用戶端和資源，透過程式碼使用的多載`AddApiAuthorization`，採取動作來設定選項。</span><span class="sxs-lookup"><span data-stu-id="9cba8-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="9cba8-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="9cba8-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
