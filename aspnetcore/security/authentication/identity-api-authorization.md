---
title: 在 ASP.NET Core 上的單一頁面應用程式的驗證簡介
author: ''
description: 您可以使用身分識別與裝載 ASP.NET Core 應用程式內的單一頁面應用程式。
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346720"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="bac48-103">Spa 的驗證和授權</span><span class="sxs-lookup"><span data-stu-id="bac48-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="bac48-104">ASP.NET 3.0 中，我們引進對單一頁面應用程式中使用我們的新支援的 API 授權驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="bac48-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="bac48-105">這項支援以 ASP.NET Core 身分識別的組合進行驗證和儲存使用者和 Identity Server 實作 Open ID Connect。</span><span class="sxs-lookup"><span data-stu-id="bac48-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="bac48-106">我們已新增新的驗證參數至我們的 Angular 和 React 範本，類似於我們的 mvc 和 razor 頁面範本使用的驗證參數允許值 'None' 和 「 個人 」。</span><span class="sxs-lookup"><span data-stu-id="bac48-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="bac48-107">建立授權的 API 支援的 Angular 應用程式</span><span class="sxs-lookup"><span data-stu-id="bac48-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="bac48-108">若要建立新的 Angular 應用程式具有支援的驗證和授權的使用者，開啟命令殼層並執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bac48-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="bac48-109">上述命令會建立 ASP.NET Core 應用程式，但*ClientApp*包含 Angular 應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="bac48-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="bac48-110">建立授權的 API 支援的 React 應用程式</span><span class="sxs-lookup"><span data-stu-id="bac48-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="bac48-111">若要建立新的 React 應用程式具有支援的驗證和授權的使用者，開啟命令殼層並執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bac48-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="bac48-112">上述命令會建立 ASP.NET Core 應用程式，但*ClientApp*包含 React 應用程式的目錄。</span><span class="sxs-lookup"><span data-stu-id="bac48-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="bac48-113">ASP.NET Core 應用程式的元件的一般描述</span><span class="sxs-lookup"><span data-stu-id="bac48-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="bac48-114">當我們加入驗證的支援時，有幾項功能，專案：</span><span class="sxs-lookup"><span data-stu-id="bac48-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="bac48-115">啟動類別</span><span class="sxs-lookup"><span data-stu-id="bac48-115">Startup class</span></span>

<span data-ttu-id="bac48-116">如果我們看看以下的啟動類別中的程式碼，我們可以非常感謝下列包含：</span><span class="sxs-lookup"><span data-stu-id="bac48-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="bac48-117">內`public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="bac48-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="bac48-118">使用預設 UI 身分識別。</span><span class="sxs-lookup"><span data-stu-id="bac48-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="bac48-119">使用其他的 AddApiAuthorization 協助程式方法的身分識別伺服器，設定一些預設 ASP.NET Identity Server 之上的慣例。</span><span class="sxs-lookup"><span data-stu-id="bac48-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="bac48-120">使用額外的 AddIdentityServerJwt 協助程式方法，以設定應用程式驗證 Identity Server 所產生的 Jwt 權杖驗證。</span><span class="sxs-lookup"><span data-stu-id="bac48-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="bac48-121">內`public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="bac48-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="bac48-122">驗證中介軟體負責驗證連入要求中的認證和設定上的要求內容的使用者。</span><span class="sxs-lookup"><span data-stu-id="bac48-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="bac48-123">識別伺服器中介軟體所公開的 Open ID Connect 的端點。</span><span class="sxs-lookup"><span data-stu-id="bac48-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="bac48-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="bac48-124">AddApiAuthorization</span></span> 
<span data-ttu-id="bac48-125">這個 helper 方法會使用我們的支援的組態的身分識別伺服器設定。</span><span class="sxs-lookup"><span data-stu-id="bac48-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="bac48-126">Identity Server 是一種非常強大且可延伸的架構，來處理應用程式的安全性考量，但同時公開更為複雜，我們不需要知道的最常見的案例，因此我們選擇一組慣例和我們認為您的設定選項是不錯的起點。</span><span class="sxs-lookup"><span data-stu-id="bac48-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="bac48-127">一旦您的驗證需求變更 Identity Server 的完整功能是仍可讓您可以自訂以配合您的需求。</span><span class="sxs-lookup"><span data-stu-id="bac48-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="bac48-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="bac48-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="bac48-129">這個 helper 方法會將應用程式的原則配置設定為預設的驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="bac48-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="bac48-130">設定此原則，讓處理移至任何子路徑的識別 url 空間中的所有要求的身分識別 」 / 身分識別 」，並讓 JwtBearerHandler 都處理所有其他要求。</span><span class="sxs-lookup"><span data-stu-id="bac48-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="bac48-131">這個方法會註冊的 Addionally `<<ApplicationName>>API` Api 使用 identity server 的預設範圍的資源`<<ApplicationName>>API`並設定 JWT Bearer token 中介軟體來驗證應用程式身分識別伺服器發出的權杖。</span><span class="sxs-lookup"><span data-stu-id="bac48-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="bac48-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="bac48-132">SampleDataController</span></span>
<span data-ttu-id="bac48-133">如果我們看看檔案 Controllers\SampleDataController.cs 我們可以發現`[Authorize]`屬性套用至類別，表示使用者必須獲得授權以存取資源的預設原則。</span><span class="sxs-lookup"><span data-stu-id="bac48-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="bac48-134">預設的授權原則會設定為使用預設的驗證配置所設定`AddIdentityServerJwt`我們先前所述的原則配置，使 JwtBearer 處理常式設定這類的 helper 方法的預設處理常式應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="bac48-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="bac48-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="bac48-135">ApplicationDbContext</span></span>
<span data-ttu-id="bac48-136">如果我們看看 Data\ApplicationDbContext.cs 中的檔案，我們就可以看到我們使用身分識別與例外狀況中，它會擴充 ApiAuthorizationDbContext （IdentityDbContext 的衍生程度較大類別） 的相同 DbContext 結構描述包含身分識別的伺服器。</span><span class="sxs-lookup"><span data-stu-id="bac48-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="bac48-137">如果我們想要的資料庫結構描述的完全控制我們只可以繼承自其中一個可用的身分識別 DbContext 類別及設定要藉由呼叫包含身分識別結構描述的內容`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`上`OnModelCreating`方法。</span><span class="sxs-lookup"><span data-stu-id="bac48-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="bac48-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="bac48-138">OidcConfigurationController</span></span>
<span data-ttu-id="bac48-139">如果我們看看檔案 Controllers\OidcConfigurationController.cs 我們可以看到端點我們站立提供用戶端要使用的 OIDC 參數。</span><span class="sxs-lookup"><span data-stu-id="bac48-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="bac48-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="bac48-140">appsettings.json</span></span>
<span data-ttu-id="bac48-141">如果我們查看專案的根目錄中的 appsettings.json 檔案時，我們可以看到新`IdentityServer`區段描述的清單，設定用戶端，而且我們可以看到有單一的用戶端。</span><span class="sxs-lookup"><span data-stu-id="bac48-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="bac48-142">用戶端的名稱會對應到的應用程式名稱，而由慣例，以 oAuth ClientId 參數對應。</span><span class="sxs-lookup"><span data-stu-id="bac48-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="bac48-143">設定檔表示我們要設定，應用程式的類型，我們使用它在內部磁碟機的慣例，可簡化伺服器的設定程序。</span><span class="sxs-lookup"><span data-stu-id="bac48-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="bac48-144">有數個設定檔可以使用下一節所述。</span><span class="sxs-lookup"><span data-stu-id="bac48-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="bac48-145">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="bac48-145">appsettings.Development.json</span></span>
<span data-ttu-id="bac48-146">如果我們看看 appsettings。Development.json 檔案在專案根目錄中，我們可以看到新`IdentityServer`告訴您，我們會用來簽署權杖的索引鍵的一節。</span><span class="sxs-lookup"><span data-stu-id="bac48-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="bac48-147">當部署至生產環境索引鍵必須佈建，並與應用程式一起部署，如下所述。</span><span class="sxs-lookup"><span data-stu-id="bac48-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="bac48-148">Angular 應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="bac48-148">General description of the Angular application</span></span>
<span data-ttu-id="bac48-149">進行驗證和 Angular 範本中的 API 授權支援存在於自己的 Angular 模組。</span><span class="sxs-lookup"><span data-stu-id="bac48-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="bac48-150">ClientApp\src\api 授權和其下是由下列項目組成：</span><span class="sxs-lookup"><span data-stu-id="bac48-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="bac48-151">3 個元件：</span><span class="sxs-lookup"><span data-stu-id="bac48-151">3 Components:</span></span>
  * <span data-ttu-id="bac48-152">登入元件：處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="bac48-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="bac48-153">登出元件：處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="bac48-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="bac48-154">登入 功能表的元件：Widget 可顯示目前已驗證的使用者具有連結管理使用者設定檔，並登出或登入或使用者未通過驗證時註冊的連結。</span><span class="sxs-lookup"><span data-stu-id="bac48-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="bac48-155">路由 guard `AuthorizeGuard` ，可以新增至路由，並要求使用者必須經過驗證才能瀏覽的路由。</span><span class="sxs-lookup"><span data-stu-id="bac48-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="bac48-156">Http 攔截器`AuthorizeInterceptor`，將存取權杖附加至傳出 HTTP 要求以 API 為目標，當使用者通過驗證。</span><span class="sxs-lookup"><span data-stu-id="bac48-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="bac48-157">服務`AuthorizeService`會處理較低層級的詳細資料，在驗證程序，會顯示有關已驗證的使用者取用的應用程式的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="bac48-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="bac48-158">定義路由的 angular 模組相關聯的應用程式的驗證部分，並公開登入 功能表元件、 攔截器、 成立條件和耗用量的其餘部分的應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="bac48-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="bac48-159">React 應用程式的一般描述</span><span class="sxs-lookup"><span data-stu-id="bac48-159">General description of the React application</span></span>
<span data-ttu-id="bac48-160">驗證和 React 範本生活 ClientApp\src\components\api authorization\ 以及它在 API 授權的支援是由下列項目所組成：</span><span class="sxs-lookup"><span data-stu-id="bac48-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="bac48-161">4 個元件：</span><span class="sxs-lookup"><span data-stu-id="bac48-161">4 Components:</span></span>
  * <span data-ttu-id="bac48-162">登入元件：處理應用程式的登入流程。</span><span class="sxs-lookup"><span data-stu-id="bac48-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="bac48-163">登出元件：處理應用程式的登出流程。</span><span class="sxs-lookup"><span data-stu-id="bac48-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="bac48-164">登入 功能表的元件：Widget 可顯示目前已驗證的使用者具有連結管理使用者設定檔，並登出或登入或使用者未通過驗證時註冊的連結。</span><span class="sxs-lookup"><span data-stu-id="bac48-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="bac48-165">AuthorizeRoute:要求使用者必須經過驗證才能轉譯元件的路由元件是由元件參數所示。</span><span class="sxs-lookup"><span data-stu-id="bac48-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="bac48-166">匯出`authService`類別的執行個體`AuthorizeService`，處理較低層級的詳細資料，在驗證程序，並會顯示有關已驗證的使用者取用的應用程式的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="bac48-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="bac48-167">既然我們已了解解決方案的主要元件，我們可以探討特定應用程式的個別案例：</span><span class="sxs-lookup"><span data-stu-id="bac48-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="bac48-168">需要新的 API 上授權</span><span class="sxs-lookup"><span data-stu-id="bac48-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="bac48-169">系統已根據預設，得來，在新的 Api 所需的授權。</span><span class="sxs-lookup"><span data-stu-id="bac48-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="bac48-170">若要這樣做，只要建立新的控制器，並新增`[Authorize]`屬性至控制器類別或控制器內的任何動作。</span><span class="sxs-lookup"><span data-stu-id="bac48-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="bac48-171">保護用戶端路由 (Angular)</span><span class="sxs-lookup"><span data-stu-id="bac48-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="bac48-172">保護用戶端端路由，即可將授權成立條件新增至設定的路由時若要執行的成立條件清單。</span><span class="sxs-lookup"><span data-stu-id="bac48-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="bac48-173">做為範例中，您可以看到主要的應用程式的 angular 模組中擷取資料路由的設定方式：</span><span class="sxs-lookup"><span data-stu-id="bac48-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="bac48-174">務必要提到保護路由不會保護實際的端點 (仍需要`[Authorize]`套用屬性)，但它只會防止使用者未經驗證時，瀏覽至指定的用戶端端路由。</span><span class="sxs-lookup"><span data-stu-id="bac48-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="bac48-175">驗證 API 要求 (Angular)</span><span class="sxs-lookup"><span data-stu-id="bac48-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="bac48-176">驗證要求一起裝載 Api 應用程式會透過應用程式定義的 HTTP 用戶端攔截器使用自動完成。</span><span class="sxs-lookup"><span data-stu-id="bac48-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="bac48-177">保護用戶端路由 (React)</span><span class="sxs-lookup"><span data-stu-id="bac48-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="bac48-178">保護用戶端端路由是由使用 AuthorizeRoute 元件，而不純文字的路由元件。</span><span class="sxs-lookup"><span data-stu-id="bac48-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="bac48-179">做為範例中，您可以看到在應用程式元件提取資料路由的設定方式：</span><span class="sxs-lookup"><span data-stu-id="bac48-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="bac48-180">務必要提到保護路由不會保護實際的端點 (仍需要`[Authorize]`套用屬性)，但它只會防止使用者未經驗證時，瀏覽至指定的用戶端端路由。</span><span class="sxs-lookup"><span data-stu-id="bac48-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="bac48-181">驗證 API 要求 (React)</span><span class="sxs-lookup"><span data-stu-id="bac48-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="bac48-182">驗證使用 react 的要求由第一次匯入`authService`執行個體`AuthorizeService`然後 authService 從擷取的存取權杖，並將它附加至要求，如下所示。</span><span class="sxs-lookup"><span data-stu-id="bac48-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="bac48-183">React 元件中這麼做通常在 componentDidMount 生命週期方法，或做為結果從某些使用者互動。</span><span class="sxs-lookup"><span data-stu-id="bac48-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="bac48-184">AuthService 匯入您的元件</span><span class="sxs-lookup"><span data-stu-id="bac48-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="bac48-185">擷取，並將存取權杖附加至回應</span><span class="sxs-lookup"><span data-stu-id="bac48-185">Retrieve and attach the access token to the response</span></span>

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a><span data-ttu-id="bac48-186">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="bac48-186">Deploy into production</span></span>

<span data-ttu-id="bac48-187">若要部署到生產環境應用程式中，我們需要佈建數個資源：</span><span class="sxs-lookup"><span data-stu-id="bac48-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="bac48-188">授與資料庫來儲存身分識別使用者帳戶和身分識別的伺服器。</span><span class="sxs-lookup"><span data-stu-id="bac48-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="bac48-189">生產環境来使用的憑證來簽署權杖。</span><span class="sxs-lookup"><span data-stu-id="bac48-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="bac48-190">此憑證; 沒有特定的需求它可以是自我簽署的憑證或憑證，透過 CA 授權單位佈建。</span><span class="sxs-lookup"><span data-stu-id="bac48-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="bac48-191">可以透過 powershell 或 openssl 等的標準工具產生。</span><span class="sxs-lookup"><span data-stu-id="bac48-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="bac48-192">它可以安裝在目標電腦上的憑證存放區或部署為強式密碼的 pfx 檔案。</span><span class="sxs-lookup"><span data-stu-id="bac48-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="bac48-193">範例：將部署到 Azure 網站</span><span class="sxs-lookup"><span data-stu-id="bac48-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="bac48-194">在這一節我們即將部署到 Azure 網站使用憑證的憑證存放區中儲存應用程式。</span><span class="sxs-lookup"><span data-stu-id="bac48-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="bac48-195">我們需要修改應用程式，從憑證存放區載入憑證。</span><span class="sxs-lookup"><span data-stu-id="bac48-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="bac48-196">若要這樣做，我們的 app service 方案必須至少在標準層次當我們設定在稍後的步驟。</span><span class="sxs-lookup"><span data-stu-id="bac48-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="bac48-197">在我們的應用程式中我們只需要修改 IdentityServer 區段在 appsettings.json 以包含金鑰的詳細資料：</span><span class="sxs-lookup"><span data-stu-id="bac48-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* <span data-ttu-id="bac48-198">與憑證的辨別主旨，對應上憑證的 name 屬性。</span><span class="sxs-lookup"><span data-stu-id="bac48-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="bac48-199">存放區位置代表載入憑證 （CurrentUrser 或 LocalMachine） 的位置。</span><span class="sxs-lookup"><span data-stu-id="bac48-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="bac48-200">存放區名稱代表存放憑證的位置，可在此情況下，它所指向的個人使用者存放區的憑證存放區的名稱。</span><span class="sxs-lookup"><span data-stu-id="bac48-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="bac48-201">若要部署至 Azure 網站，部署應用程式中的步驟[將應用程式部署至 Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)建立所需的 Azure 資源，並將應用程式部署至生產環境。</span><span class="sxs-lookup"><span data-stu-id="bac48-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="bac48-202">完成之後，應用程式部署到 Azure，但尚未完全正常運作，我們仍需要設定應用程式所要使用的憑證。</span><span class="sxs-lookup"><span data-stu-id="bac48-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="bac48-203">若要這樣做，我們需要我們將使用，並遵循的步驟中所述的憑證指紋[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates)。</span><span class="sxs-lookup"><span data-stu-id="bac48-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="bac48-204">雖然這些步驟會提及 SSL，沒有 「 私人憑證 」 一節我們將可以上傳我們佈建的憑證，以搭配我們的應用程式在入口網站。</span><span class="sxs-lookup"><span data-stu-id="bac48-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="bac48-205">這個步驟之後，我們應該要能重新啟動應用程式，且應完整功能。</span><span class="sxs-lookup"><span data-stu-id="bac48-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="bac48-206">其他組態選項</span><span class="sxs-lookup"><span data-stu-id="bac48-206">Other configuration options</span></span>
<span data-ttu-id="bac48-207">使用一組慣例，預設值和簡化的單一頁面應用程式體驗的增強功能，Identity Server 之上建置我們的支援 API 的授權。</span><span class="sxs-lookup"><span data-stu-id="bac48-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="bac48-208">不用說，Identity Server 的完整功能是可在幕後，如果我們提供的整合不涵蓋您的案例。</span><span class="sxs-lookup"><span data-stu-id="bac48-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="bac48-209">我們支援著重於我們稱為 「 第一方 」 應用程式，其中的所有應用程式建立和部署我們的組織。</span><span class="sxs-lookup"><span data-stu-id="bac48-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="bac48-210">因此我們不會提供像是同意或同盟的支援。</span><span class="sxs-lookup"><span data-stu-id="bac48-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="bac48-211">這些情況下我們的建議是使用 Identity Server，並遵循其文件。</span><span class="sxs-lookup"><span data-stu-id="bac48-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="bac48-212">應用程式設定檔</span><span class="sxs-lookup"><span data-stu-id="bac48-212">Application profiles</span></span>
<span data-ttu-id="bac48-213">應用程式設定檔是預先定義的組態，進一步定義其參數的應用程式。</span><span class="sxs-lookup"><span data-stu-id="bac48-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="bac48-214">在此階段中，我們支援兩個設定檔：</span><span class="sxs-lookup"><span data-stu-id="bac48-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="bac48-215">IdentityServerSPA:表示裝載在一起當做單一單位的 Identity Server 的單一頁面應用程式。</span><span class="sxs-lookup"><span data-stu-id="bac48-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="bac48-216">Redirect_uri 預設為`/authentication/login-callback`。</span><span class="sxs-lookup"><span data-stu-id="bac48-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="bac48-217">Post_logout_redirect_uri 預設為`/authentication/logout-callback`。</span><span class="sxs-lookup"><span data-stu-id="bac48-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="bac48-218">包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。</span><span class="sxs-lookup"><span data-stu-id="bac48-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="bac48-219">允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。</span><span class="sxs-lookup"><span data-stu-id="bac48-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="bac48-220">允許的回應模式是`fragment`。</span><span class="sxs-lookup"><span data-stu-id="bac48-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="bac48-221">SPA:表示未裝載使用 Identity Server 的單一頁面應用程式。</span><span class="sxs-lookup"><span data-stu-id="bac48-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="bac48-222">包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。</span><span class="sxs-lookup"><span data-stu-id="bac48-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="bac48-223">允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。</span><span class="sxs-lookup"><span data-stu-id="bac48-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="bac48-224">允許的回應模式是`fragment`。</span><span class="sxs-lookup"><span data-stu-id="bac48-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="bac48-225">IdentityServerJwt:表示使用 Identity Server 會裝載在一起的 API。</span><span class="sxs-lookup"><span data-stu-id="bac48-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="bac48-226">應用程式已設定為具有單一的範圍，預設值為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="bac48-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="bac48-227">API:表示使用 Identity Server 未裝載的 API。</span><span class="sxs-lookup"><span data-stu-id="bac48-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="bac48-228">應用程式已設定為具有單一的範圍，預設值為應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="bac48-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="bac48-229">透過 AppSettings 組態</span><span class="sxs-lookup"><span data-stu-id="bac48-229">Configuration through AppSettings</span></span>
<span data-ttu-id="bac48-230">我們可以透過我們的組態系統設定的應用程式，方法是分別將它們新增至用戶端或資源的清單。</span><span class="sxs-lookup"><span data-stu-id="bac48-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="bac48-231">設定用戶端時我們可以設定`redirect_uri`而`post_logout_redirect_uri`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bac48-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
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

<span data-ttu-id="bac48-232">設定資源時我們可以設定資源的範圍，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bac48-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="bac48-233">透過程式碼設定</span><span class="sxs-lookup"><span data-stu-id="bac48-233">Configuration through code</span></span>
<span data-ttu-id="bac48-234">我們也可以設定用戶端和資源，透過使用採取動作來設定選項的 AddApiAuthorization 的多載，程式碼。</span><span class="sxs-lookup"><span data-stu-id="bac48-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
