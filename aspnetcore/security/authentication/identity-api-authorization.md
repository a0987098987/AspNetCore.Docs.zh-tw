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
# <a name="authentication-and-authorization-for-spas"></a>Spa 的驗證和授權

ASP.NET 3.0 中，我們引進對單一頁面應用程式中使用我們的新支援的 API 授權驗證的支援。 這項支援以 ASP.NET Core 身分識別的組合進行驗證和儲存使用者和 Identity Server 實作 Open ID Connect。

我們已新增新的驗證參數至我們的 Angular 和 React 範本，類似於我們的 mvc 和 razor 頁面範本使用的驗證參數允許值 'None' 和 「 個人 」。

## <a name="create-an-angular-app-with-api-authorization-support"></a>建立授權的 API 支援的 Angular 應用程式

若要建立新的 Angular 應用程式具有支援的驗證和授權的使用者，開啟命令殼層並執行下列命令：

```console
dotnet new angular -o <output_directory_name> -au Individual
```

上述命令會建立 ASP.NET Core 應用程式，但*ClientApp*包含 Angular 應用程式的目錄。

## <a name="create-a-react-app-with-api-authorization-support"></a>建立授權的 API 支援的 React 應用程式

若要建立新的 React 應用程式具有支援的驗證和授權的使用者，開啟命令殼層並執行下列命令：

```console
dotnet new react -o <output_directory_name> -au Individual
```

上述命令會建立 ASP.NET Core 應用程式，但*ClientApp*包含 React 應用程式的目錄。

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>ASP.NET Core 應用程式的元件的一般描述

當我們加入驗證的支援時，有幾項功能，專案：

### <a name="startup-class"></a>啟動類別

如果我們看看以下的啟動類別中的程式碼，我們可以非常感謝下列包含：
* 內`public void ConfigureServices(IServiceCollection services)`:
  * 使用預設 UI 身分識別。
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * 使用其他的 AddApiAuthorization 協助程式方法的身分識別伺服器，設定一些預設 ASP.NET Identity Server 之上的慣例。
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * 使用額外的 AddIdentityServerJwt 協助程式方法，以設定應用程式驗證 Identity Server 所產生的 Jwt 權杖驗證。 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* 內`public void Configure(IApplicationBuilder app)`:
  * 驗證中介軟體負責驗證連入要求中的認證和設定上的要求內容的使用者。
    ```csharp
    app.UseAuthentication();
    ```
  * 識別伺服器中介軟體所公開的 Open ID Connect 的端點。
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
這個 helper 方法會使用我們的支援的組態的身分識別伺服器設定。 Identity Server 是一種非常強大且可延伸的架構，來處理應用程式的安全性考量，但同時公開更為複雜，我們不需要知道的最常見的案例，因此我們選擇一組慣例和我們認為您的設定選項是不錯的起點。 一旦您的驗證需求變更 Identity Server 的完整功能是仍可讓您可以自訂以配合您的需求。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
這個 helper 方法會將應用程式的原則配置設定為預設的驗證處理常式。 設定此原則，讓處理移至任何子路徑的識別 url 空間中的所有要求的身分識別 」 / 身分識別 」，並讓 JwtBearerHandler 都處理所有其他要求。
這個方法會註冊的 Addionally `<<ApplicationName>>API` Api 使用 identity server 的預設範圍的資源`<<ApplicationName>>API`並設定 JWT Bearer token 中介軟體來驗證應用程式身分識別伺服器發出的權杖。

### <a name="sampledatacontroller"></a>SampleDataController
如果我們看看檔案 Controllers\SampleDataController.cs 我們可以發現`[Authorize]`屬性套用至類別，表示使用者必須獲得授權以存取資源的預設原則。 預設的授權原則會設定為使用預設的驗證配置所設定`AddIdentityServerJwt`我們先前所述的原則配置，使 JwtBearer 處理常式設定這類的 helper 方法的預設處理常式應用程式的要求。

### <a name="applicationdbcontext"></a>ApplicationDbContext
如果我們看看 Data\ApplicationDbContext.cs 中的檔案，我們就可以看到我們使用身分識別與例外狀況中，它會擴充 ApiAuthorizationDbContext （IdentityDbContext 的衍生程度較大類別） 的相同 DbContext 結構描述包含身分識別的伺服器。
如果我們想要的資料庫結構描述的完全控制我們只可以繼承自其中一個可用的身分識別 DbContext 類別及設定要藉由呼叫包含身分識別結構描述的內容`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`上`OnModelCreating`方法。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
如果我們看看檔案 Controllers\OidcConfigurationController.cs 我們可以看到端點我們站立提供用戶端要使用的 OIDC 參數。

### <a name="appsettingsjson"></a>appsettings.json
如果我們查看專案的根目錄中的 appsettings.json 檔案時，我們可以看到新`IdentityServer`區段描述的清單，設定用戶端，而且我們可以看到有單一的用戶端。 用戶端的名稱會對應到的應用程式名稱，而由慣例，以 oAuth ClientId 參數對應。 設定檔表示我們要設定，應用程式的類型，我們使用它在內部磁碟機的慣例，可簡化伺服器的設定程序。 有數個設定檔可以使用下一節所述。

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings.Development.json
如果我們看看 appsettings。Development.json 檔案在專案根目錄中，我們可以看到新`IdentityServer`告訴您，我們會用來簽署權杖的索引鍵的一節。 當部署至生產環境索引鍵必須佈建，並與應用程式一起部署，如下所述。

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Angular 應用程式的一般描述
進行驗證和 Angular 範本中的 API 授權支援存在於自己的 Angular 模組。 ClientApp\src\api 授權和其下是由下列項目組成：
* 3 個元件：
  * 登入元件：處理應用程式的登入流程。
  * 登出元件：處理應用程式的登出流程。
  * 登入 功能表的元件：Widget 可顯示目前已驗證的使用者具有連結管理使用者設定檔，並登出或登入或使用者未通過驗證時註冊的連結。
* 路由 guard `AuthorizeGuard` ，可以新增至路由，並要求使用者必須經過驗證才能瀏覽的路由。
* Http 攔截器`AuthorizeInterceptor`，將存取權杖附加至傳出 HTTP 要求以 API 為目標，當使用者通過驗證。
* 服務`AuthorizeService`會處理較低層級的詳細資料，在驗證程序，會顯示有關已驗證的使用者取用的應用程式的其餘部分。
* 定義路由的 angular 模組相關聯的應用程式的驗證部分，並公開登入 功能表元件、 攔截器、 成立條件和耗用量的其餘部分的應用程式的服務。

## <a name="general-description-of-the-react-application"></a>React 應用程式的一般描述
驗證和 React 範本生活 ClientApp\src\components\api authorization\ 以及它在 API 授權的支援是由下列項目所組成：
* 4 個元件：
  * 登入元件：處理應用程式的登入流程。
  * 登出元件：處理應用程式的登出流程。
  * 登入 功能表的元件：Widget 可顯示目前已驗證的使用者具有連結管理使用者設定檔，並登出或登入或使用者未通過驗證時註冊的連結。
  * AuthorizeRoute:要求使用者必須經過驗證才能轉譯元件的路由元件是由元件參數所示。
  * 匯出`authService`類別的執行個體`AuthorizeService`，處理較低層級的詳細資料，在驗證程序，並會顯示有關已驗證的使用者取用的應用程式的其餘部分。

既然我們已了解解決方案的主要元件，我們可以探討特定應用程式的個別案例：

## <a name="requiring-authorization-on-a-new-api"></a>需要新的 API 上授權
系統已根據預設，得來，在新的 Api 所需的授權。 若要這樣做，只要建立新的控制器，並新增`[Authorize]`屬性至控制器類別或控制器內的任何動作。

## <a name="protecting-a-client-side-route-angular"></a>保護用戶端路由 (Angular)
保護用戶端端路由，即可將授權成立條件新增至設定的路由時若要執行的成立條件清單。 做為範例中，您可以看到主要的應用程式的 angular 模組中擷取資料路由的設定方式：

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

務必要提到保護路由不會保護實際的端點 (仍需要`[Authorize]`套用屬性)，但它只會防止使用者未經驗證時，瀏覽至指定的用戶端端路由。

## <a name="authenticate-api-requests-angular"></a>驗證 API 要求 (Angular)

驗證要求一起裝載 Api 應用程式會透過應用程式定義的 HTTP 用戶端攔截器使用自動完成。

## <a name="protect-a-client-side-route-react"></a>保護用戶端路由 (React)

保護用戶端端路由是由使用 AuthorizeRoute 元件，而不純文字的路由元件。 做為範例中，您可以看到在應用程式元件提取資料路由的設定方式：

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

務必要提到保護路由不會保護實際的端點 (仍需要`[Authorize]`套用屬性)，但它只會防止使用者未經驗證時，瀏覽至指定的用戶端端路由。

## <a name="authenticate-api-requests-react"></a>驗證 API 要求 (React)

驗證使用 react 的要求由第一次匯入`authService`執行個體`AuthorizeService`然後 authService 從擷取的存取權杖，並將它附加至要求，如下所示。 React 元件中這麼做通常在 componentDidMount 生命週期方法，或做為結果從某些使用者互動。

### <a name="import-the-authservice-into-your-component"></a>AuthService 匯入您的元件

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>擷取，並將存取權杖附加至回應

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

## <a name="deploy-into-production"></a>部署到生產環境

若要部署到生產環境應用程式中，我們需要佈建數個資源：
* 授與資料庫來儲存身分識別使用者帳戶和身分識別的伺服器。
* 生產環境来使用的憑證來簽署權杖。
  * 此憑證; 沒有特定的需求它可以是自我簽署的憑證或憑證，透過 CA 授權單位佈建。
  * 可以透過 powershell 或 openssl 等的標準工具產生。
  * 它可以安裝在目標電腦上的憑證存放區或部署為強式密碼的 pfx 檔案。

### <a name="example-deploy-into-azure-websites"></a>範例：將部署到 Azure 網站

在這一節我們即將部署到 Azure 網站使用憑證的憑證存放區中儲存應用程式。 我們需要修改應用程式，從憑證存放區載入憑證。 若要這樣做，我們的 app service 方案必須至少在標準層次當我們設定在稍後的步驟。 在我們的應用程式中我們只需要修改 IdentityServer 區段在 appsettings.json 以包含金鑰的詳細資料：
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
* 與憑證的辨別主旨，對應上憑證的 name 屬性。
* 存放區位置代表載入憑證 （CurrentUrser 或 LocalMachine） 的位置。
* 存放區名稱代表存放憑證的位置，可在此情況下，它所指向的個人使用者存放區的憑證存放區的名稱。

若要部署至 Azure 網站，部署應用程式中的步驟[將應用程式部署至 Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)建立所需的 Azure 資源，並將應用程式部署至生產環境。

完成之後，應用程式部署到 Azure，但尚未完全正常運作，我們仍需要設定應用程式所要使用的憑證。 若要這樣做，我們需要我們將使用，並遵循的步驟中所述的憑證指紋[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates)。

雖然這些步驟會提及 SSL，沒有 「 私人憑證 」 一節我們將可以上傳我們佈建的憑證，以搭配我們的應用程式在入口網站。

這個步驟之後，我們應該要能重新啟動應用程式，且應完整功能。

## <a name="other-configuration-options"></a>其他組態選項
使用一組慣例，預設值和簡化的單一頁面應用程式體驗的增強功能，Identity Server 之上建置我們的支援 API 的授權。 不用說，Identity Server 的完整功能是可在幕後，如果我們提供的整合不涵蓋您的案例。 我們支援著重於我們稱為 「 第一方 」 應用程式，其中的所有應用程式建立和部署我們的組織。 因此我們不會提供像是同意或同盟的支援。 這些情況下我們的建議是使用 Identity Server，並遵循其文件。

### <a name="application-profiles"></a>應用程式設定檔
應用程式設定檔是預先定義的組態，進一步定義其參數的應用程式。 在此階段中，我們支援兩個設定檔：
* IdentityServerSPA:表示裝載在一起當做單一單位的 Identity Server 的單一頁面應用程式。
  * Redirect_uri 預設為`/authentication/login-callback`。
  * Post_logout_redirect_uri 預設為`/authentication/logout-callback`。
  * 包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。
  * 允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。
  * 允許的回應模式是`fragment`。
* SPA:表示未裝載使用 Identity Server 的單一頁面應用程式。
  * 包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。
  * 允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。
  * 允許的回應模式是`fragment`。
* IdentityServerJwt:表示使用 Identity Server 會裝載在一起的 API。
  * 應用程式已設定為具有單一的範圍，預設值為應用程式名稱。
* API:表示使用 Identity Server 未裝載的 API。
  * 應用程式已設定為具有單一的範圍，預設值為應用程式名稱。

### <a name="configuration-through-appsettings"></a>透過 AppSettings 組態
我們可以透過我們的組態系統設定的應用程式，方法是分別將它們新增至用戶端或資源的清單。 

設定用戶端時我們可以設定`redirect_uri`而`post_logout_redirect_uri`，如下所示：
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

設定資源時我們可以設定資源的範圍，如下所示：
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

### <a name="configuration-through-code"></a>透過程式碼設定
我們也可以設定用戶端和資源，透過使用採取動作來設定選項的 AddApiAuthorization 的多載，程式碼。
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
