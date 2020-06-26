---
title: ASP.NET Core 中的 Facebook 外部登入設定
author: rick-anderson
description: 教學課程中的程式碼範例示範如何將 Facebook 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/facebook-logins
ms.openlocfilehash: df91b6f324de70b8492ccf0aef74c9264c3e9711
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403946"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Facebook 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

<!-- per @rick-anderson and scott addie, don't update images. Remove images and point the customer to the FB set up page. FB needs to maintain  instructions to get key and secret.
-->

本教學課程包含程式碼範例，說明如何讓使用者使用在[上一頁](xref:security/authentication/social/index)建立的範例 ASP.NET Core 3.0 專案，以其 Facebook 帳戶登入。 我們一開始會遵循[官方步驟](https://developers.facebook.com)來建立 Facebook 應用程式識別碼。

## <a name="create-the-app-in-facebook"></a>在 Facebook 中建立應用程式

* 將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet 套件新增至專案。

* 流覽至[Facebook 開發人員應用程式](https://developers.facebook.com/apps/)頁面並登入。 如果您還沒有 Facebook 帳戶，請使用登入頁面上的 [**註冊 Facebook** ] 連結來建立一個。  擁有 Facebook 帳戶後，請遵循指示註冊為 Facebook 開發人員。

* 從 [**我的應用程式**] 功能表中，選取 [**建立應用程式**] 以建立新的應用程式識別碼。

   ![適用于開發人員的 Facebook 入口網站在 Microsoft Edge 中開啟](index/_static/FBMyApps.png)

* 填寫表單，並按一下 [**建立應用程式識別碼**] 按鈕。

  ![建立新的應用程式識別碼表單](index/_static/FBNewAppId.png)

* 在 [新的應用程式] 卡片上，選取 [**新增產品**]。  在**Facebook 登**入卡上，按一下 [**設定**] 

  ![產品設定頁面](index/_static/FBProductSetup.png)

* [**快速入門**] 嚮導會以 **[選擇平臺**] 作為第一頁來啟動。 立即略過嚮導，方法是按一下左下方功能表中的 [ **FaceBook 登**入**設定**] 連結：

  ![略過快速入門](index/_static/FBSkipQuickStart.png)

* 您會看到 [**用戶端 OAuth 設定**] 頁面：

  ![[用戶端 OAuth 設定] 頁面](index/_static/FBOAuthSetup.png)

* 輸入您的開發 URI，並將 */signin-facebook*附加至 [**有效的 OAuth 重新導向 uri** ] 欄位（例如： `https://localhost:44320/signin-facebook` ）。 本教學課程稍後設定的 Facebook 驗證會在 */signin-facebook*路由上自動處理要求，以執行 OAuth 流程。

> [!NOTE]
> URI */signin-facebook*會設定為 facebook 驗證提供者的預設回呼。 您可以在設定 Facebook 驗證中介軟體時，透過[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。

* 按一下 **[儲存變更]** 。

* 按一下左側導覽列中的 [**設定**] [  >  **基本**] 連結。

  在此頁面上，記下您的 `App ID` 和 `App Secret` 。 您會在下一節中，將這兩個新增至您的 ASP.NET Core 應用程式：

* 部署網站時，您必須重新流覽**Facebook 登**入設定頁面，並註冊新的公用 URI。

## <a name="store-the-facebook-app-id-and-secret"></a>儲存 Facebook 應用程式識別碼和密碼

使用[秘密管理員](xref:security/app-secrets)來儲存機密設定（例如 Facebook 應用程式識別碼和秘密值）。 針對此範例，請使用下列步驟：

1. 根據[啟用秘密儲存](xref:security/app-secrets#enable-secret-storage)中的指示，初始化秘密儲存的專案。
1. 使用秘密金鑰和，將敏感性設定儲存在本機密碼存放區中 `Authentication:Facebook:AppId` `Authentication:Facebook:AppSecret` ：

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a>設定 Facebook 驗證

在 Startup.cs 檔案的方法中新增 Facebook 服務 `ConfigureServices` ： *Startup.cs*

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

## <a name="sign-in-with-facebook"></a>使用 Facebook 登入

* 執行應用程式，然後選取 [**登入**]。 
* 在 [**使用其他服務登入**] 底下，選取 [Facebook]。
* 系統會將您重新導向至**Facebook**以進行驗證。
* 輸入您的 Facebook 認證。
* 系統會將您重新導向回到您的網站，您可以在其中設定您的電子郵件。

您現在已使用 Facebook 認證登入：

<a name="react"></a>

## <a name="react-to-cancel-authorize-external-sign-in"></a>回應取消授權外部登入

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.AccessDeniedPath>當使用者未核准要求的授權需求時，可以提供使用者代理程式的重新導向路徑。

下列程式碼會將設定 `AccessDeniedPath` 為 `"/AccessDeniedPathInfo"` ：

[!code-csharp[](~/security/authentication/social/social-code/StartupAccessDeniedPath.cs?name=snippetFB)]

我們建議 `AccessDeniedPath` 頁面包含下列資訊：

*  遠端驗證已取消。
* 此應用程式需要驗證。
* 若要再次嘗試登入，請選取 [登入] 連結。

### <a name="test-accessdeniedpath"></a>測試 AccessDeniedPath

* 流覽至[facebook.com](https://www.facebook.com/)
* 如果您已登入，則必須登出。
* 執行應用程式，然後選取 [Facebook 登入]。
* 選取 [**不是現在**]。 系統會將您重新導向至指定的 `AccessDeniedPath` 頁面。

<!-- End of React  -->
[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

如需 Facebook 驗證所支援之設定選項的詳細資訊，請參閱[FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API 參考。 設定選項可用於：

* 要求使用者的不同資訊。
* 加入查詢字串引數來自訂登入體驗。

## <a name="troubleshooting"></a>疑難排解

* **僅限 ASP.NET Core 2.x：** 如果 Identity 未透過 `services.AddIdentity` 在中呼叫 `ConfigureServices` 來設定，則嘗試驗證會導致*ArgumentException：必須提供 ' SignInScheme ' 選項*。 本教學課程中使用的專案範本可確保這項作業已完成。
* 如果尚未藉由套用初始遷移來建立網站資料庫，則在*處理要求錯誤時*，您會取得資料庫作業失敗。 請按 [套用**遷移**] 來建立資料庫，並重新整理以繼續發生錯誤。

## <a name="next-steps"></a>後續步驟

* 本文說明了您可以如何使用 Facebook 進行驗證。 您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。

* 將網站發佈至 Azure web 應用程式之後，您應該 `AppSecret` 在 Facebook 開發人員入口網站中重設。

* 將 `Authentication:Facebook:AppId` 和設定 `Authentication:Facebook:AppSecret` 為 Azure 入口網站中的應用程式設定。 設定系統已設定為從環境變數讀取金鑰。
