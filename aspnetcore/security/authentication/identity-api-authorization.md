---
title: 在 ASP.NET Core 上的單一頁面應用程式的驗證簡介
author: javiercn
description: 使用單一頁面應用程式裝載 ASP.NET Core 應用程式內使用身分識別。
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4afc9ac0a3c54b452c6a1b23e4de31d7e2fc5284
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665333"
---
# <a name="authentication-and-authorization-for-spas"></a>Spa 的驗證和授權

3.0 或更新版本的 ASP.NET Core 可提供在單一頁面應用程式 (Spa) 中的驗證 API 授權使用的支援。 針對驗證和儲存使用者的 ASP.NET Core Identity 結合[IdentityServer](https://identityserver.io/)實作 Open ID Connect。

驗證參數已新增至**Angular**並**React**專案範本中的驗證參數的類似**Web 應用程式 （模型-檢視-控制器）** (MVC) 和**Web 應用程式**（Razor 頁面） 專案範本。 允許的參數值為**無**並**個別**。 **React.js 與 Redux**專案範本不支援這一次的驗證參數。

## <a name="create-an-app-with-api-authorization-support"></a>建立授權的 API 支援的應用程式

使用 Angular 和 React Spa 可用使用者驗證和授權。 開啟命令殼層中，然後執行下列命令：

**Angular**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**React**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

上述命令會建立 ASP.NET Core 應用程式，但*ClientApp* SPA 的所在目錄。

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>ASP.NET Core 應用程式的元件的一般描述

驗證支援包含時，下列章節將描述新增至專案：

### <a name="startup-class"></a>啟動類別

`Startup`類別具有下列各項：

* 內`Startup.ConfigureServices`方法：
  * 使用預設 UI 身分識別：

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * 加上一個 IdentityServer `AddApiAuthorization` helper 方法，該設定一些預設 IdentityServer 之上的 ASP.NET Core 慣例：

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * 有額外的驗證`AddIdentityServerJwt`IdentityServer 所產生的設定來驗證 JWT 權杖的應用程式的協助程式方法：

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* 內`Startup.Configure`方法：
  * 驗證中介軟體負責驗證要求的認證和設定使用者的要求內容：

    ```csharp
    app.UseAuthentication();
    ```

  * IdentityServer 中介軟體所公開的 Open ID Connect 端點：

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

這個 helper 方法會設定 IdentityServer 使用我們的支援的組態。 IdentityServer 是一種功能強大且可擴充的架構，來處理應用程式的安全性考量。 在此同時，會公開不必要的複雜度，針對最常見的案例。 因此，一組慣例和組態選項被提供給您認為是不錯的起點。 一旦您的驗證需求變更，IdentityServer 的完整功能是仍可自訂以符合您需求的驗證。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

這個 helper 方法會將應用程式的原則配置設定為預設的驗證處理常式。 設定此原則，讓處理所有要求路由傳送至任何子路徑的識別 URL 空間中的身分識別 」 / 身分識別 」。 `JwtBearerHandler`會處理所有其他要求。 此外，這個方法會註冊`<<ApplicationName>>API`的預設範圍的 API 資源與 IdentityServer`<<ApplicationName>>API`並設定 JWT Bearer token 中介軟體驗證的應用程式的 IdentityServer 所簽發的權杖。

### <a name="sampledatacontroller"></a>SampleDataController

在  *Controllers\SampleDataController.cs*檔案中，請注意`[Authorize]`屬性套用至類別，表示使用者必須獲得授權以存取資源的預設原則。 預設的授權原則會設定為使用預設的驗證配置，它可設定的設定`AddIdentityServerJwt`上面所提及，讓原則配置`JwtBearerHandler`這類的協助程式方法所設定的預設處理常式應用程式的要求。

### <a name="applicationdbcontext"></a>ApplicationDbContext

在*Data\ApplicationDbContext.cs*檔案中，請注意，相同`DbContext`有一點除外，它會擴充用於身分識別`ApiAuthorizationDbContext`(更多衍生類別`IdentityDbContext`) 結構描述包含如 IdentityServer。

若要取得的資料庫結構描述的完整控制權，繼承自其中一個可用的身分識別`DbContext`類別，並設定要藉由呼叫包含身分識別結構描述的內容`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`上`OnModelCreating`方法。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

在  *Controllers\OidcConfigurationController.cs*檔案中，請注意已佈建服務用戶端要使用的 OIDC 參數為端點。

### <a name="appsettingsjson"></a>appsettings.json

在  *appsettings.json*檔案的專案根目錄中，沒有新`IdentityServer`區段描述的清單，設定用戶端。 在下列範例中，沒有單一的用戶端。 用戶端名稱對應至應用程式名稱和對應的慣例，以 OAuth`ClientId`參數。 設定檔會指出正在設定應用程式型別。 它是在內部用來簡化伺服器的設定程序的磁碟機慣例。 有數個設定檔，如所述[應用程式設定檔](#application-profiles)一節。

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

在  *appsettings。Development.json*檔案的專案根目錄中，有`IdentityServer`區段，其中描述用來簽署權杖的金鑰。 部署到生產環境時，金鑰必須佈建，並與應用程式一起部署中所述[部署至生產環境](#deploy-to-production)一節。

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>在 Angular 應用程式的一般描述

驗證和授權的 API 支援在 Angular 範本位於它自己 Angular 模組*ClientApp\src\api 授權*目錄。 模組是由下列項目所組成：

* 3 個元件：
  * *login.component.ts*:處理應用程式的登入流程。
  * *logout.component.ts*:處理應用程式的登出流程。
  * *登入 menu.component.ts*:一種 widget，顯示其中一個連結的下列設定：
    * 管理使用者設定檔和記錄參考連結時驗證使用者。
    * 註冊和登入連結，當使用者未通過驗證。
* 路由 guard `AuthorizeGuard` ，可以新增至路由，並要求使用者必須經過驗證才能瀏覽的路由。
* HTTP 攔截器`AuthorizeInterceptor`，將存取權杖附加至傳出 HTTP 要求以 API 為目標，當使用者通過驗證。
* 服務`AuthorizeService`，處理驗證程序的較低層級詳細資料，並會顯示有關已驗證的使用者取用的應用程式的其餘部分。
* 定義應用程式的驗證部分相關聯的路由 Angular 模組。 它會公開登入 功能表元件、 攔截器、 成立條件和應用程式的其餘部分的耗用量的服務。

## <a name="general-description-of-the-react-app"></a>React 應用程式的一般描述

進行驗證和 React 範本中的 API 授權支援位於*ClientApp\src\components\api 授權*目錄。 它是由下列項目所組成：

* 4 個元件：
  * *Login.js*:處理應用程式的登入流程。
  * *Logout.js*:處理應用程式的登出流程。
  * *LoginMenu.js*:一種 widget，顯示其中一個連結的下列設定：
    * 管理使用者設定檔和記錄參考連結時驗證使用者。
    * 註冊和登入連結，當使用者未通過驗證。
  * *AuthorizeRoute.js*:要求使用者必須經過驗證才能轉譯元件的路由元件所示`Component`參數。
* 匯出`authService`類別的執行個體`AuthorizeService`，處理驗證程序的較低層級詳細資料，並會顯示有關已驗證的使用者取用的應用程式的其餘部分。

既然您已了解解決方案的主要元件，您可能需要更深入的了解應用程式的個別案例。

## <a name="require-authorization-on-a-new-api"></a>需要新的 API 上授權

根據預設，系統會設定可在新的 Api 輕鬆地要求授權。 若要這樣做，請建立新的控制器，並新增`[Authorize]`屬性至控制器類別或控制器內的任何動作。

## <a name="protect-a-client-side-route-angular"></a>保護用戶端路由 (Angular)

保護用戶端的路由，即可將授權成立條件新增至設定的路由時若要執行的成立條件清單。 例如，您可以看到如何`fetch-data`路由設定主應用程式的 Angular 模組：

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

務必要提到保護路由不會保護實際的端點 (仍需要`[Authorize]`套用屬性)，但它只會防止使用者未通過驗證時，瀏覽至指定的用戶端路由。

## <a name="authenticate-api-requests-angular"></a>驗證 API 要求 (Angular)

驗證應用程式一同裝載的 Api 的要求會自動完成透過 HTTP 的用戶端攔截器定義應用程式所使用。

## <a name="protect-a-client-side-route-react"></a>保護用戶端路由 (React)

使用保護的用戶端路由`AuthorizeRoute`元件，而不是純`Route`元件。 例如，請注意如何`fetch-data`內設定路由`App`元件：

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

保護路由：

* 不會保護實際的端點 (仍需要`[Authorize]`套用屬性)。
* 只會防止使用者未通過驗證時，瀏覽至指定的用戶端路由。

## <a name="authenticate-api-requests-react"></a>驗證 API 要求 (React)

驗證使用 React 的要求由第一次匯入`authService`執行個體`AuthorizeService`。 從擷取存取權杖`authService`並附加至要求，如下所示。 React 元件在這項工作通常是`componentDidMount`生命週期方法或某些使用者互動的結果。

### <a name="import-the-authservice-into-your-component"></a>AuthService 匯入您的元件

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>擷取，並將存取權杖附加至回應

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

## <a name="deploy-to-production"></a>部署到生產環境

若要將應用程式部署到生產環境中，下列資源需要將他們佈建：

* 若要儲存身分識別使用者帳戶和 IdentityServer 授與資料庫。
* 生產環境来使用的憑證來簽署權杖。
  * 此憑證; 沒有特定的需求它可以是自我簽署的憑證或憑證，透過 CA 授權單位佈建。
  * 可以透過 PowerShell 或 OpenSSL 等的標準工具產生。
  * 它可以是安裝在目標電腦上的憑證存放區或部署為 *.pfx*強式密碼的檔案。

### <a name="example-deploy-to-azure-websites"></a>範例：部署到 Azure 網站

本章節描述應用程式部署至 Azure 網站使用憑證存放區中儲存的憑證。 若要修改應用程式，以從憑證存放區載入憑證，必須至少是在標準層時您在稍後步驟中設定 App Service 方案。 在應用程式之*appsettings.json*檔案中，修改`IdentityServer`區段以包含金鑰的詳細資料：

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

* 與憑證的辨別主旨，對應上憑證的 name 屬性。
* 存放區位置代表載入之憑證的位置 (`CurrentUser`或`LocalMachine`)。
* 存放區名稱代表憑證的儲存位置的憑證存放區的名稱。 在此情況下，它會指向個人使用者存放區。

若要部署至 Azure 網站，部署應用程式中的步驟[將應用程式部署至 Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)建立所需的 Azure 資源，並將應用程式部署至生產環境。

遵循先前的指示之後, 應用程式部署至 Azure，但尚無法運作。 應用程式所使用的憑證仍然需要進行設定。 找出要使用憑證的指紋，並遵循所述的步驟[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates)。

雖然這些步驟提到 SSL，則**私用憑證**入口網站中的一節，您可以上傳要與應用程式所使用的佈建的憑證。

這個步驟之後，重新啟動應用程式，它應該才能運作。

## <a name="other-configuration-options"></a>其他組態選項

使用一組慣例，預設值，並簡化 Spa 的體驗的增強功能，IdentityServer 之上建置 API 授權的支援。 不用說，IdentityServer 的完整功能是可在幕後，如果 ASP.NET Core 整合不涵蓋您的案例。 ASP.NET Core 支援著重於 「 第一方 」 應用程式，建立並由我們的組織部署的所有應用程式。 此情況下，不提供支援，例如同意或同盟。 對於這些案例中，使用 IdentityServer 並遵循其文件。

### <a name="application-profiles"></a>應用程式設定檔

應用程式設定檔是預先定義的組態，進一步定義其參數的應用程式。 在此階段中，支援下列設定檔：

* `IdentityServerSPA`：表示裝載在一起當做單一單位的 IdentityServer SPA。
  * `redirect_uri`預設為`/authentication/login-callback`。
  * `post_logout_redirect_uri`預設為`/authentication/logout-callback`。
  * 包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。
  * 允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。
  * 允許的回應模式是`fragment`。
* `SPA`：表示未裝載與 IdentityServer SPA。
  * 包含一組範圍`openid`， `profile`，和每個應用程式中的 api 定義的範圍。
  * 允許的 OIDC 回應類型的集合`id_token token`或 每個個別 (`id_token`， `token`)。
  * 允許的回應模式是`fragment`。
* `IdentityServerJwt`：表示與 IdentityServer 裝載在一起的 API。
  * 應用程式已設定為具有單一的範圍，預設值為應用程式名稱。
* `API`：表示未裝載與 IdentityServer 的 API。
  * 應用程式已設定為具有單一的範圍，預設值為應用程式名稱。

### <a name="configuration-through-appsettings"></a>透過 AppSettings 組態

設定透過組態系統的應用程式新增至清單`Clients`或`Resources`。

設定每個用戶端`redirect_uri`和`post_logout_redirect_uri`屬性，如下列範例所示：

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

在設定的資源時，您可以設定資源的範圍，如下所示：

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

### <a name="configuration-through-code"></a>透過程式碼設定

您也可以設定用戶端和資源，透過程式碼使用的多載`AddApiAuthorization`，採取動作來設定選項。

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

## <a name="additional-resources"></a>其他資源

* <xref:spa/angular>
* <xref:spa/react>
