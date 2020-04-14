---
title: ASP.NET核心中的 Facebook 外部登入設定
author: rick-anderson
description: 包含代碼示例的教程,演示 Facebook 帳戶使用者身份驗證集成到現有ASP.NET核心應用。
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 9b3128addafb41ad6ec44af5cb12e89607e1ae59
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277297"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET核心中的 Facebook 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

<!-- per @rick-anderson and scott addie, don't update images. Remove images and point the customer to the FB set up page. FB needs to maintain  instructions to get key and secret.
-->

本教程包含代碼示例展示如何允許使用者使用[上一頁上](xref:security/authentication/social/index)創建的範例ASP.NET Core 3.0 專案使用 Facebook 帳戶登錄。 我們首先按照[官方步驟](https://developers.facebook.com)創建 Facebook 應用程式 ID。

## <a name="create-the-app-in-facebook"></a>在 Facebook 中建立應用

* 將[Microsoft.AspNetCore.身份驗證.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet 包添加到專案中。

* 導航到[Facebook 開發人員應用](https://developers.facebook.com/apps/)頁面並登錄。 如果您還沒有 Facebook 帳戶,請使用登入頁面上**的「註冊 Facebook」** 連結創建一個帳戶。  擁有 Facebook 帳戶後,請按照說明註冊為 Facebook 開發人員。

* 從「**我的應用」** 選單中選擇 **「創建應用**」以建立新的應用 ID。

   ![Facebook 面向開發人員門戶在微軟邊緣打開](index/_static/FBMyApps.png)

* 填寫表單並點擊「**創建應用 ID」** 按鈕。

  ![建立新的應用程式識別碼表單](index/_static/FBNewAppId.png)

* 在新應用卡上,選擇 **「添加產品**」。。  在**Facebook 登錄**卡上,單擊"**設置"** 

  ![產品設定頁面](index/_static/FBProductSetup.png)

* **"快速入門**"嚮導以 **"選擇平台**作為第一頁"啟動。 按下左下角選單中的**FaceBook 登錄****設定**連結,立即繞過嚮導:

  ![跳過快速入門](index/_static/FBSkipQuickStart.png)

* 將顯示 **「 客戶端設定」** 頁面:

  ![用戶端 Auth 設定頁面](index/_static/FBOAuthSetup.png)

* 輸入您的開發*URI,將 /signin-facebook*新增到**有效 OAuth 重定向 URI**欄位`https://localhost:44320/signin-facebook`(例如: 本教程稍後配置的 Facebook 身份驗證將自動處理 */signin-facebook*路由上的請求,以實現 OAuth 流。

> [!NOTE]
> URI */signin-facebook*設置為 Facebook 身份驗證供應商的預設回調。 您可以通過繼承的[「遠端身份驗證選項.CallbackPath」](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性配置 Facebook 身份驗證中間件,同時[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)更改預設回檔 URI。

* 按一下 [儲存變更]****。

* 按一下左側導航中的 **「設置** > **基本」** 連結。

  在此頁上,記下您和您的`App ID``App Secret`。 在下一節中,您將兩者加入ASP.NET核心應用程式中:

* 部署網站時,您需要重新訪問**Facebook 登錄**設置頁面並註冊新的公共 URI。

## <a name="store-the-facebook-app-id-and-secret"></a>儲存 Facebook 應用程式識別碼和機密

使用[秘密管理器](xref:security/app-secrets)儲存敏感設置,如 Facebook 應用 ID 和機密值。 對於此示例,請使用以下步驟:

1. 根據[啟用密鑰存儲](xref:security/app-secrets#enable-secret-storage)的指令初始化了機密存儲的專案。
1. 使用金鑰`Authentication:Facebook:AppId`與將敏感設定儲存在本地機密儲存中`Authentication:Facebook:AppSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a>設定 Facebook 認證

在`ConfigureServices`*Startup.cs*檔案中的方法中新增 Facebook 服務:

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

## <a name="sign-in-with-facebook"></a>使用 Facebook 登入

* 運行應用並選擇 **「登錄」。** 
* 在 **"使用其他服務登錄"** 下,選擇 Facebook。
* 您將被重定向到**Facebook**進行身份驗證。
* 輸入您的 Facebook 認證。
* 您將被重定向回您的網站,您可以在其中設定電子郵件。

您現在使用 Facebook 認證登入:

<a name="react"></a>

## <a name="react-to-cancel-authorize-external-sign-in"></a>回應取消授權外部登入

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.AccessDeniedPath>當使用者不批准請求的授權請求時,可以為使用者代理提供重定向路徑。

以下代碼將`AccessDeniedPath``"/AccessDeniedPathInfo"`集 :

[!code-csharp[](~/security/authentication/social/social-code/StartupAccessDeniedPath.cs?name=snippetFB)]

我們建議該`AccessDeniedPath`頁面包含以下資訊:

*  遠端身份驗證已取消。
* 此應用程式需要身份驗證。
* 要再次嘗試登錄,請選擇"登錄"連結。

### <a name="test-accessdeniedpath"></a>測試存取拒絕路徑

* 導航到[facebook.com](https://www.facebook.com/)
* 如果您已登錄,則必須註銷。
* 運行應用並選擇 Facebook 登錄。
* 選擇 **「現在不**」。 您將重定到指定的`AccessDeniedPath`頁面。

<!-- End of React  -->
[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

有關 Facebook 身份驗證支援的配置選項的詳細資訊,請參閱[Facebook 選項](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions)API 參考。 設定選項可用於:

* 請求有關使用者的不同資訊。
* 添加查詢字串參數以自定義登錄體驗。

## <a name="troubleshooting"></a>疑難排解

* **只有ASP.NET核心 2.x:** 如果未透過除錯`services.AddIdentity`設定識別`ConfigureServices`,則試著認證會導致*參數異常:必須提供「SignInScheme」選項*。 本教學中使用的專案範本可確保完成此操作。
* 如果尚未透過應用程式初始移至建立網站資料庫,則在處理請求錯誤時,將取得*資料庫操作失敗*。 點按 **「應用遷移**」以建立資料庫並刷新以繼續結束錯誤。

## <a name="next-steps"></a>後續步驟

* 本文展示了如何使用 Facebook 進行身份驗證。 您可以採用類似的方法對[上一頁](xref:security/authentication/social/index)中列出的其他提供程式進行身份驗證。

* 將網站發布到 Azure Web 應用程式後,應重`AppSecret`置 Facebook 開發人員門戶中的 。

* 在`Authentication:Facebook:AppId`Azure`Authentication:Facebook:AppSecret`門戶中將 和 設置為應用程式設置。 配置系統設置為從環境變數讀取密鑰。
