---
title: ASP.NET Core 上單一頁面應用程式的驗證簡介
author: javiercn
description: 將身分識別與裝載于 ASP.NET Core 應用程式內的單一頁面應用程式搭配使用。
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/08/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 623f739b17c0bed3ce929f562c9581ab26ecf5bc
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928553"
---
# <a name="authentication-and-authorization-for-spas"></a>Spa 的驗證和授權

ASP.NET Core 3.0 或更新版本使用 API 授權的支援，在單一頁面應用程式（Spa）中提供驗證。 用於驗證和儲存使用者的 ASP.NET Core 身分識別會與[IdentityServer](https://identityserver.io/)結合，以執行 Open ID Connect。

驗證參數已加入至「**角度**」和「**回應**」專案範本，類似于**Web 應用程式（模型-視圖控制器）** （MVC）和**web 應用程式**（Razor Pages）專案範本中的驗證參數。 允許的參數值為 [**無**] 和 [**個人**]。 [**回應 .js] 和 [Redux** ] 專案範本目前不支援 [驗證] 參數。

## <a name="create-an-app-with-api-authorization-support"></a>建立具有 API 授權支援的應用程式

使用者驗證和授權可以同時用於「角度」和「回應」 Spa。 開啟命令 shell，然後執行下列命令：

**角度**：

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

**回應**：

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

上述命令會建立一個 ASP.NET Core 應用程式，其中具有包含 SPA 的*ClientApp*目錄。

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>應用程式 ASP.NET Core 元件的一般描述

下列各節說明當包含驗證支援時，專案的新增功能：

### <a name="startup-class"></a>啟始類別

`Startup` 類別具有下列新增專案：

* 在 `Startup.ConfigureServices` 方法內：
  * 使用預設 UI 的身分識別：

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer 有額外的 `AddApiAuthorization` helper 方法，會在 IdentityServer 上設定一些預設的 ASP.NET Core 慣例：

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * 使用額外的 `AddIdentityServerJwt` helper 方法進行驗證，以設定應用程式來驗證 IdentityServer 所產生的 JWT 權杖：

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* 在 `Startup.Configure` 方法內：
  * 驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：

    ```csharp
    app.UseAuthentication();
    ```

  * 公開 Open ID Connect 端點的 IdentityServer 中介軟體：

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

此 helper 方法會將 IdentityServer 設定為使用我們支援的設定。 IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。 同時，會在最常見的案例中公開不必要的複雜性。 因此，系統會為您提供一組慣例和設定選項，並將其視為良好的起點。 一旦您的驗證需要變更，IdentityServer 的完整功能仍可供自訂驗證以符合您的需求。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

此 helper 方法會將應用程式的原則配置設定為預設驗證處理常式。 原則會設定為讓身分識別處理路由傳送至身分識別 URL 空間 "/Identity" 中任何子路徑的所有要求。 `JwtBearerHandler` 會處理所有的其他要求。 此外，此方法會向 IdentityServer 註冊具有預設範圍 `<<ApplicationName>>API` 的 `<<ApplicationName>>API` API 資源，並設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。

### <a name="weatherforecastcontroller"></a>WeatherForecastController

在*Controllers\WeatherForecastController.cs*檔案中，請注意套用至類別的 `[Authorize]` 屬性，表示使用者必須根據預設原則來存取資源。 預設的授權原則會設定為使用預設的驗證配置，這是由 `AddIdentityServerJwt` 到上述原則配置所設定，讓這類 helper 方法為應用程式要求的預設處理常式設定 `JwtBearerHandler`。

### <a name="applicationdbcontext"></a>[ApplicationdbcoNtext]

在*Data\ApplicationDbCoNtext.cs*檔案中，請注意，識別中會使用相同的 `DbContext`，但其延伸 `ApiAuthorizationDbContext` （來自 `IdentityDbContext`的衍生類別）以包含 IdentityServer 的架構。

若要取得資料庫架構的完整控制權，請從 `DbContext` 類別的其中一個可用身分識別繼承，並設定內容以包含身分識別架構，方法是在 `OnModelCreating` 方法上呼叫 `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

在*Controllers\OidcConfigurationController.cs*檔案中，請注意布建的端點可提供用戶端所需使用的 OIDC 參數。

### <a name="appsettingsjson"></a>appsettings.json

在專案根目錄的*appsettings*中，有一個新的 `IdentityServer` 區段，說明已設定的用戶端清單。 在下列範例中，有一個用戶端。 用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId` 參數。 此設定檔會指出正在設定的應用程式類型。 它是在內部用來驅動慣例，以簡化伺服器的設定程式。 有數個設定檔可供使用，如[應用程式佈建檔](#application-profiles)一節中所述。

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings.開發 json

在*appsettings 中。* 專案根目錄的開發 json 檔案，其中有一個 `IdentityServer` 區段，描述用來簽署權杖的金鑰。 部署至生產環境時，必須隨應用程式一起布建和部署金鑰，如[部署至生產](#deploy-to-production)一節中所述。

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>角度應用程式的一般描述

角度範本中的驗證和 API 授權支援位於*ClientApp\src\api-authorization*目錄中自己的角度模組。 模組是由下列元素所組成：

* 3個元件：
  * *login. component. ts*：處理應用程式的登入流程。
  * *登出。 component. ts*：處理應用程式的登出流程。
  * *[登入]-[component. ts*]：顯示下列其中一組連結的 widget：
    * 使用者設定檔管理，並在使用者經過驗證時登出連結。
    * 當使用者未經過驗證時，註冊並登入連結。
* 可以新增至路由並要求使用者在造訪路由之前進行驗證的 route guard `AuthorizeGuard`。
* HTTP 攔截器 `AuthorizeInterceptor`，會在使用者通過驗證時，將存取權杖附加至以 API 為目標的傳出 HTTP 要求。
* 服務 `AuthorizeService`，可處理驗證程式的較低層級詳細資料，並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。
* 一個角度模組，定義與應用程式驗證部分相關聯的路由。 它會公開登入功能表元件、攔截器、防護和服務，以便從應用程式的其餘部分取用。

## <a name="general-description-of-the-react-app"></a>回應應用程式的一般描述

回應範本中的驗證和 API 授權支援位於*ClientApp\src\components\api-authorization*目錄中。 它是由下列元素所組成：

* 4個元件：
  * *登入 .js*：處理應用程式的登入流程。
  * *登出 .js*：處理應用程式的登出流程。
  * *LoginMenu*：顯示下列其中一組連結的 widget：
    * 使用者設定檔管理，並在使用者經過驗證時登出連結。
    * 當使用者未經過驗證時，註冊並登入連結。
  * *AuthorizeRoute*：必須先驗證使用者才能轉譯 `Component` 參數所指示之元件的路由元件。
* 匯出的 `authService` 類別 `AuthorizeService` 實例，可處理驗證程式的較低層級詳細資料，並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。

既然您已瞭解解決方案的主要元件，您可以進一步瞭解應用程式的個別案例。

## <a name="require-authorization-on-a-new-api"></a>需要新 API 的授權

根據預設，系統會設定為輕鬆地要求新 Api 的授權。 若要這麼做，請建立新的控制器，並將 `[Authorize]` 屬性新增至控制器類別或控制器內的任何動作。

## <a name="customize-the-api-authentication-handler"></a>自訂 API 驗證處理常式

若要自訂 API 之 JWT 處理常式的設定，請設定其 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> 實例：

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

API 的 JWT 處理常式會引發事件，讓您能夠使用 `JwtBearerEvents`來控制驗證進程。 為了提供 API 授權的支援，`AddIdentityServerJwt` 會註冊自己的事件處理常式。

若要自訂事件的處理，請視需要使用其他邏輯來包裝現有的事件處理常式。 例如：

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

在上述程式碼中，會將 `OnTokenValidated` 事件處理常式取代為自訂的執行。 此實作為：

1. 呼叫 API 授權支援所提供的原始實作為。
1. 執行它自己的自訂邏輯。

## <a name="protect-a-client-side-route-angular"></a>保護用戶端路由（角度）

若要保護用戶端路由，請將授權防護新增至設定路由時要執行的防護清單。 例如，您可以在主要應用程式角度模組內查看 `fetch-data` 路由的設定方式：

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

請務必注意，保護路由並不會保護實際端點（仍然需要套用 `[Authorize]` 屬性），但它只會防止使用者在未驗證時流覽至指定的用戶端路由。

## <a name="authenticate-api-requests-angular"></a>驗證 API 要求（角度）

使用應用程式所定義的 HTTP 用戶端攔截器，來驗證與應用程式一起裝載之 Api 的要求會自動完成。

## <a name="protect-a-client-side-route-react"></a>保護用戶端路由（回應）

使用 `AuthorizeRoute` 元件，而不是純 `Route` 元件來保護用戶端路由。 例如，請注意 `App` 元件內如何設定 `fetch-data` 路由：

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

保護路由：

* 不會保護實際的端點（仍然需要套用 `[Authorize]` 屬性）。
* 只有在未驗證時，才會防止使用者流覽至指定的用戶端路由。

## <a name="authenticate-api-requests-react"></a>驗證 API 要求（回應）

藉由先從 `AuthorizeService`匯入 `authService` 實例，來驗證具有回應的要求。 存取權杖會從 `authService` 抓取，並附加至要求，如下所示。 在回應元件中，這項工作通常會在 `componentDidMount` 生命週期方法中完成，或做為某些使用者互動的結果。

### <a name="import-the-authservice-into-your-component"></a>將 authService 匯入您的元件

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>取得存取權杖，並將其附加至回應

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

若要將應用程式部署至生產環境，您必須布建下列資源：

* 用來儲存身分識別使用者帳戶和 IdentityServer 授與的資料庫。
* 用於簽署權杖的生產憑證。
  * 此憑證沒有特定的需求;它可以是自我簽署憑證或透過 CA 授權單位布建的憑證。
  * 您可以透過 PowerShell 或 OpenSSL 之類的標準工具來產生它。
  * 它可以安裝到目的電腦上的憑證存放區，或部署為具有強式密碼的 *.pfx*檔案。

### <a name="example-deploy-to-azure-websites"></a>範例：部署至 Azure 網站

本節說明如何使用儲存在憑證存放區中的憑證，將應用程式部署到 Azure 網站。 若要修改應用程式以從憑證存放區載入憑證，當您在後續步驟中設定時，App Service 方案必須至少在標準層。 在應用程式的*appsettings*中，修改 `IdentityServer` 區段以包含金鑰詳細資料：

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

* 存放區名稱代表儲存憑證之憑證存放區的名稱。 在此情況下，它會指向 [個人] 使用者存放區。
* 存放區位置代表從何處載入憑證（`CurrentUser` 或 `LocalMachine`）。
* 憑證上的名稱屬性會對應到憑證的辨別主旨。

若要部署至 Azure 網站，請遵循將[應用程式部署至 azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)中的步驟來部署應用程式，以建立必要的 Azure 資源，並將應用程式部署至生產環境。

遵循上述指示之後，應用程式會部署至 Azure，但尚未運作。 應用程式所使用的憑證仍然需要設定。 找出要使用的憑證指紋，並遵循[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code)中所述的步驟。

雖然這些步驟提到 SSL，但入口網站上有 [**私人憑證**] 區段，您可以在其中上傳佈建的憑證以搭配應用程式使用。

在此步驟之後，請重新開機應用程式，它應該會正常運作。

## <a name="other-configuration-options"></a>其他設定選項

API 授權的支援建置於具有一組慣例、預設值和增強功能的 IdentityServer 之上，以簡化 Spa 的體驗。 不用說，如果 ASP.NET Core 整合不會涵蓋您的案例，IdentityServer 的完整功能就會在幕後提供。 ASP.NET Core 支援著重于「第一方」應用程式，其中的所有應用程式都是由我們的組織建立和部署。 因此，不會針對同意或同盟等專案提供支援。 針對這些案例，請使用 IdentityServer 並遵循其檔。

### <a name="application-profiles"></a>應用程式佈建檔

應用程式佈建檔是針對進一步定義其參數之應用程式預先定義的設定。 目前支援下列設定檔：

* `IdentityServerSPA`：代表與 IdentityServer 一起裝載為單一單位的 SPA。
  * `redirect_uri` 預設為 `/authentication/login-callback`。
  * `post_logout_redirect_uri` 預設為 `/authentication/logout-callback`。
  * 一組範圍包括針對應用程式中的 Api 所定義的 `openid`、`profile`和每個範圍。
  * 一組允許的 OIDC 回應類型為 `id_token token` 或個別的（`id_token`，`token`）。
  * 允許的回應模式為 `fragment`。
* `SPA`：代表不是以 IdentityServer 裝載的 SPA。
  * 一組範圍包括針對應用程式中的 Api 所定義的 `openid`、`profile`和每個範圍。
  * 一組允許的 OIDC 回應類型為 `id_token token` 或個別的（`id_token`，`token`）。
  * 允許的回應模式為 `fragment`。
* `IdentityServerJwt`：代表與 IdentityServer 一起裝載的 API。
  * 應用程式已設定為具有單一範圍，預設為應用程式名稱。
* `API`：代表不是以 IdentityServer 裝載的 API。
  * 應用程式已設定為具有單一範圍，預設為應用程式名稱。

### <a name="configuration-through-appsettings"></a>透過 AppSettings 設定

將應用程式新增至 `Clients` 或 `Resources`清單，即可透過設定系統來進行。

設定每個用戶端的 `redirect_uri` 和 `post_logout_redirect_uri` 屬性，如下列範例所示：

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

設定資源時，您可以設定資源的範圍，如下所示：

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

您也可以使用接受動作來設定選項的 `AddApiAuthorization` 多載，透過程式碼來設定用戶端和資源。

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
* <xref:security/authentication/scaffold-identity>
