---
title: ASP.NET Core 上單一頁面應用程式的驗證簡介
author: javiercn
description: 將身分識別與裝載于 ASP.NET Core 應用程式內的單一頁面應用程式搭配使用。
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/05/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 6b8818cc89a87e66ecec445ff8071348aacde64a
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819928"
---
# <a name="authentication-and-authorization-for-spas"></a>Spa 的驗證和授權

ASP.NET Core 3.0 或更新版本使用 API 授權的支援, 在單一頁面應用程式 (Spa) 中提供驗證。 用於驗證和儲存使用者的 ASP.NET Core 身分識別會與[IdentityServer](https://identityserver.io/)結合, 以執行 Open ID Connect。

驗證參數已加入至「**角度**」和「**回應**」專案範本, 類似于**Web 應用程式 (模型-視圖控制器)** (MVC) 和**web 應用程式**(Razor Pages) 中的驗證參數專案範本。 允許的參數值為 [**無**] 和 [**個人**]。 [**回應 .js] 和 [Redux** ] 專案範本目前不支援 [驗證] 參數。

## <a name="create-an-app-with-api-authorization-support"></a>建立具有 API 授權支援的應用程式

使用者驗證和授權可以同時用於「角度」和「回應」 Spa。 開啟命令殼層中，然後執行下列命令：

**角度**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**回應**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

上述命令會建立一個 ASP.NET Core 應用程式, 其中具有包含 SPA 的*ClientApp*目錄。

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>應用程式 ASP.NET Core 元件的一般描述

下列各節說明當包含驗證支援時, 專案的新增功能:

### <a name="startup-class"></a>啟始類別

`Startup`類別具有下列新增專案:

* `Startup.ConfigureServices`在方法內:
  * 使用預設 UI 的身分識別:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * 使用額外`AddApiAuthorization`的 helper 方法 IdentityServer, 以在 IdentityServer 上提供一些預設的 ASP.NET Core 慣例:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * 使用其他`AddIdentityServerJwt` helper 方法進行驗證, 以設定應用程式來驗證 IdentityServer 所產生的 JWT 權杖:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* `Startup.Configure`在方法內:
  * 驗證中介軟體會負責驗證要求認證, 並在要求內容上設定使用者:

    ```csharp
    app.UseAuthentication();
    ```

  * 公開 Open ID Connect 端點的 IdentityServer 中介軟體:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

此 helper 方法會將 IdentityServer 設定為使用我們支援的設定。 IdentityServer 是功能強大且可擴充的架構, 可處理應用程式的安全性考慮。 同時, 會在最常見的案例中公開不必要的複雜性。 因此, 系統會為您提供一組慣例和設定選項, 並將其視為良好的起點。 一旦您的驗證需要變更, IdentityServer 的完整功能仍可供自訂驗證以符合您的需求。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

此 helper 方法會將應用程式的原則配置設定為預設驗證處理常式。 原則會設定為讓身分識別處理路由傳送至身分識別 URL 空間 "/Identity" 中任何子路徑的所有要求。 會`JwtBearerHandler`處理所有其他要求。 此外, 此方法會向`<<ApplicationName>>API` IdentityServer 註冊具有預設範圍的`<<ApplicationName>>API` API 資源, 並設定 JWT 持有人權杖中介軟體, 以驗證 IdentityServer 針對應用程式所簽發的權杖。

### <a name="sampledatacontroller"></a>SampleDataController

在*Controllers\SampleDataController.cs*檔案中, 請注意`[Authorize]`套用至類別的屬性, 表示使用者必須根據預設原則來存取資源。 預設的授權原則會設定為使用預設的驗證配置, 而這是由`AddIdentityServerJwt`先前所述的原則配置所設定, `JwtBearerHandler`讓這類 helper 方法為預設的處理常式設定對應用程式的要求。

### <a name="applicationdbcontext"></a>ApplicationDbContext

在*Data\ApplicationDbCoNtext.cs*檔案中, 請注意, `DbContext`識別中會使用相同的, 其延伸`ApiAuthorizationDbContext`的例外狀況 (衍生自`IdentityDbContext`的類別) 會包含 IdentityServer 的架構。

若要取得資料庫架構的完整控制權, 請從其中一個可用的身分`DbContext`識別類別繼承, 然後透過在`OnModelCreating`方法上呼叫`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`來設定內容以包含身分識別架構。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

在*Controllers\OidcConfigurationController.cs*檔案中, 請注意布建的端點可提供用戶端所需使用的 OIDC 參數。

### <a name="appsettingsjson"></a>appsettings.json

在專案根目錄的*appsettings*中, 有一個新`IdentityServer`的區段描述已設定的用戶端清單。 在下列範例中, 有一個用戶端。 用戶端名稱會對應至應用程式名稱, 並依照慣例對應至 OAuth `ClientId`參數。 此設定檔會指出正在設定的應用程式類型。 它是在內部用來驅動慣例, 以簡化伺服器的設定程式。 有數個設定檔可供使用, 如[應用程式佈建檔](#application-profiles)一節中所述。

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

在*appsettings 中。* 專案根目錄的開發 json 檔案, 其中有一個`IdentityServer`區段描述用來簽署權杖的金鑰。 部署至生產環境時, 必須隨應用程式一起布建和部署金鑰, 如[部署至生產](#deploy-to-production)一節中所述。

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>角度應用程式的一般描述

角度範本中的驗證和 API 授權支援位於*ClientApp\src\api-authorization*目錄中自己的角度模組。 模組是由下列元素所組成:

* 3個元件:
  * *login. component. ts*:處理應用程式的登入流程。
  * *登出。 component. ts*:處理應用程式的登出流程。
  * *登入-功能表。 元件. ts*:顯示下列其中一組連結的 widget:
    * 使用者設定檔管理, 並在使用者經過驗證時登出連結。
    * 當使用者未經過驗證時, 註冊並登入連結。
* 可以新增至`AuthorizeGuard`路由的路由防護, 並要求使用者在造訪路由之前進行驗證。
* HTTP 攔截`AuthorizeInterceptor`器, 會在使用者通過驗證時, 將存取權杖附加至以 API 為目標的傳出 HTTP 要求。
* 此服務`AuthorizeService`會處理驗證程式的較低層級詳細資料, 並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。
* 一個角度模組, 定義與應用程式驗證部分相關聯的路由。 它會公開登入功能表元件、攔截器、防護和服務, 以便從應用程式的其餘部分取用。

## <a name="general-description-of-the-react-app"></a>回應應用程式的一般描述

回應範本中的驗證和 API 授權支援位於*ClientApp\src\components\api-authorization*目錄中。 它是由下列元素所組成:

* 4個元件:
  * *Login .js*:處理應用程式的登入流程。
  * *登出 .js*:處理應用程式的登出流程。
  * *LoginMenu .js*:顯示下列其中一組連結的 widget:
    * 使用者設定檔管理, 並在使用者經過驗證時登出連結。
    * 當使用者未經過驗證時, 註冊並登入連結。
  * *AuthorizeRoute .js*:需要先驗證使用者, 然後才呈現`Component`參數所指示之元件的路由元件。
* 已匯出`authService`的類別`AuthorizeService`實例, 可處理較低層級的驗證程式詳細資料, 並將已驗證使用者的相關資訊公開給其餘的應用程式以供取用。

既然您已瞭解解決方案的主要元件, 您可以進一步瞭解應用程式的個別案例。

## <a name="require-authorization-on-a-new-api"></a>需要新 API 的授權

根據預設, 系統會設定為輕鬆地要求新 Api 的授權。 若要這麼做, 請建立新的控制器, `[Authorize]`並將屬性新增至控制器類別或控制器內的任何動作。

## <a name="protect-a-client-side-route-angular"></a>保護用戶端路由 (角度)

若要保護用戶端路由, 請將授權防護新增至設定路由時要執行的防護清單。 例如, 您可以在主要應用程式角度`fetch-data`模組中查看路由的設定方式:

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

請務必注意, 保護路由並不會保護實際端點 (仍然需要`[Authorize]`套用屬性), 但它只會防止使用者在未驗證時流覽至指定的用戶端路由。

## <a name="authenticate-api-requests-angular"></a>驗證 API 要求 (角度)

使用應用程式所定義的 HTTP 用戶端攔截器, 來驗證與應用程式一起裝載之 Api 的要求會自動完成。

## <a name="protect-a-client-side-route-react"></a>保護用戶端路由 (回應)

使用`AuthorizeRoute`元件 (而不是一般`Route`元件) 來保護用戶端路由。 例如, 請注意如何`fetch-data`在`App`元件中設定路由:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

保護路由:

* 不會保護實際的端點 (仍然需要`[Authorize]`套用屬性)。
* 只有在未驗證時, 才會防止使用者流覽至指定的用戶端路由。

## <a name="authenticate-api-requests-react"></a>驗證 API 要求 (回應)

藉由先`authService` `AuthorizeService`從匯入實例, 來驗證具有回應的要求。 存取權杖是從`authService`抓取, 並且附加至要求, 如下所示。 在回應元件中, 這項工作通常會在`componentDidMount`生命週期方法中完成, 或做為某些使用者互動的結果。

### <a name="import-the-authservice-into-your-component"></a>將 authService 匯入您的元件

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>取得存取權杖, 並將其附加至回應

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

若要將應用程式部署至生產環境, 您必須布建下列資源:

* 用來儲存身分識別使用者帳戶和 IdentityServer 授與的資料庫。
* 用於簽署權杖的生產憑證。
  * 此憑證沒有特定的需求;它可以是自我簽署憑證或透過 CA 授權單位布建的憑證。
  * 您可以透過 PowerShell 或 OpenSSL 之類的標準工具來產生它。
  * 它可以安裝到目的電腦上的憑證存放區, 或部署為具有強式密碼的 *.pfx*檔案。

### <a name="example-deploy-to-azure-websites"></a>範例：部署至 Azure 網站

本節說明如何使用儲存在憑證存放區中的憑證, 將應用程式部署到 Azure 網站。 若要修改應用程式以從憑證存放區載入憑證, 當您在後續步驟中設定時, App Service 方案必須至少在標準層。 在應用程式的*appsettings*中, 修改`IdentityServer`區段以包含金鑰詳細資料:

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

* 憑證上的名稱屬性會對應到憑證的辨別主旨。
* 存放區位置代表從 (`CurrentUser`或`LocalMachine`) 載入憑證的位置。
* 存放區名稱代表儲存憑證之憑證存放區的名稱。 在此情況下, 它會指向 [個人] 使用者存放區。

若要部署至 Azure 網站, 請遵循將[應用程式部署至 azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)中的步驟來部署應用程式, 以建立必要的 Azure 資源, 並將應用程式部署至生產環境。

遵循上述指示之後, 應用程式會部署至 Azure, 但尚未運作。 應用程式所使用的憑證仍然需要設定。 找出要使用的憑證指紋, 並遵循[載入您的憑證](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code)中所述的步驟。

雖然這些步驟提到 SSL, 但入口網站上有 [**私人憑證**] 區段, 您可以在其中上傳佈建的憑證以搭配應用程式使用。

在此步驟之後, 請重新開機應用程式, 它應該會正常運作。

## <a name="other-configuration-options"></a>其他設定選項

API 授權的支援建置於具有一組慣例、預設值和增強功能的 IdentityServer 之上, 以簡化 Spa 的體驗。 不用說, 如果 ASP.NET Core 整合不會涵蓋您的案例, IdentityServer 的完整功能就會在幕後提供。 ASP.NET Core 支援著重于「第一方」應用程式, 其中的所有應用程式都是由我們的組織建立和部署。 因此, 不會針對同意或同盟等專案提供支援。 針對這些案例, 請使用 IdentityServer 並遵循其檔。

### <a name="application-profiles"></a>應用程式佈建檔

應用程式佈建檔是針對進一步定義其參數之應用程式預先定義的設定。 目前支援下列設定檔:

* `IdentityServerSPA`：表示與 IdentityServer 一起裝載為單一單位的 SPA。
  * `redirect_uri`預設為`/authentication/login-callback`。
  * `post_logout_redirect_uri`預設為`/authentication/logout-callback`。
  * 一組範圍包含`openid`、 `profile`, 以及為應用程式中的 api 定義的每個範圍。
  * 一組允許的 OIDC 回應類型為`id_token token`或個別 (`id_token`、 `token`)。
  * 允許的回應模式為`fragment`。
* `SPA`：代表不是以 IdentityServer 裝載的 SPA。
  * 一組範圍包含`openid`、 `profile`, 以及為應用程式中的 api 定義的每個範圍。
  * 一組允許的 OIDC 回應類型為`id_token token`或個別 (`id_token`、 `token`)。
  * 允許的回應模式為`fragment`。
* `IdentityServerJwt`：表示與 IdentityServer 一起裝載的 API。
  * 應用程式已設定為具有單一範圍, 預設為應用程式名稱。
* `API`：代表不是以 IdentityServer 裝載的 API。
  * 應用程式已設定為具有單一範圍, 預設為應用程式名稱。

### <a name="configuration-through-appsettings"></a>透過 AppSettings 設定

藉由將應用程式新增至`Clients`或`Resources`的清單, 以設定該系統上的應用程式。

設定每個客戶`redirect_uri`端`post_logout_redirect_uri`的和屬性, 如下列範例所示:

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

設定資源時, 您可以設定資源的範圍, 如下所示:

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

您也可以使用的多載`AddApiAuthorization` (會採取動作來設定選項), 透過程式碼來設定用戶端和資源。

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
