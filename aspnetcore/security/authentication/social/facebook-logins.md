---
title: ASP.NET Core 中的 Facebook 外部登入設定
author: rick-anderson
description: 教學課程中的程式碼範例示範如何將 Facebook 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667465"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Facebook 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

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

* 輸入您的開發 URI，並將 */signin-facebook*附加至 [**有效的 OAuth 重新導向 uri** ] 欄位（例如： `https://localhost:44320/signin-facebook`）。 本教學課程稍後設定的 Facebook 驗證會在 */signin-facebook*路由上自動處理要求，以執行 OAuth 流程。

> [!NOTE]
> URI */signin-facebook*會設定為 facebook 驗證提供者的預設回呼。 您可以在設定 Facebook 驗證中介軟體時，透過[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。

* 按一下 **[儲存變更]** 。

* 按一下左側導覽列中的 [**設定**] > [**基本**] 連結。

  在此頁面上，記下您的 `App ID` 和 `App Secret`。 您會在下一節中，將這兩個新增至您的 ASP.NET Core 應用程式：

* 部署網站時，您必須重新流覽**Facebook 登**入設定頁面，並註冊新的公用 URI。

## <a name="store-facebook-app-id-and-app-secret"></a>儲存 Facebook 應用程式識別碼和應用程式秘密

使用[秘密管理員](xref:security/app-secrets)將機密設定（例如 Facebook `App ID` 和 `App Secret`）連結至您的應用程式設定。 基於本教學課程的目的，請將權杖命名為 `Authentication:Facebook:AppId` 並 `Authentication:Facebook:AppSecret`。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

執行下列命令，使用秘密管理員安全地儲存 `App ID` 和 `App Secret`：

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>設定 Facebook 驗證

在*Startup.cs*檔案的 `ConfigureServices` 方法中新增 Facebook 服務：

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

如需 Facebook 驗證所支援之設定選項的詳細資訊，請參閱[FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API 參考。 設定選項可用於：

* 要求使用者的不同資訊。
* 加入查詢字串引數來自訂登入體驗。

## <a name="sign-in-with-facebook"></a>使用 Facebook 登入

執行您的應用程式，然後按一下 [**登入**]。 您會看到一個選項，可供您使用 Facebook 登入。

![Web 應用程式：使用者未經過驗證](index/_static/DoneFacebook.png)

當您按一下 [ **facebook**] 時，系統會將您重新導向至 facebook 以進行驗證：

![Facebook 驗證頁面](index/_static/FBLogin.png)

Facebook 驗證預設會要求公用設定檔和電子郵件地址：

![Facebook 驗證頁面同意畫面](index/_static/FBLoginDone.png)

輸入您的 Facebook 認證之後，您會重新導向至您可以設定電子郵件的網站。

您現在已使用 Facebook 認證登入：

![Web 應用程式：使用者驗證](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* **僅限 ASP.NET Core 2.x：** 如果未透過呼叫 `ConfigureServices`中的 `services.AddIdentity` 來設定身分識別，則嘗試驗證會導致*ArgumentException：必須提供 ' SignInScheme ' 選項*。 本教學課程中使用的專案範本可確保這項作業已完成。
* 如果尚未藉由套用初始遷移來建立網站資料庫，則在*處理要求錯誤時*，您會取得資料庫作業失敗。 請按 [套用**遷移**] 來建立資料庫，並重新整理以繼續發生錯誤。

## <a name="next-steps"></a>後續步驟

* 本文說明了您可以如何使用 Facebook 進行驗證。 您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。

* 將網站發佈至 Azure web 應用程式之後，您應該在 Facebook 開發人員入口網站中重設 `AppSecret`。

* 將 [`Authentication:Facebook:AppId`] 和 [`Authentication:Facebook:AppSecret`] 設定為 Azure 入口網站中的 [應用程式設定]。 設定系統已設定為從環境變數讀取金鑰。
