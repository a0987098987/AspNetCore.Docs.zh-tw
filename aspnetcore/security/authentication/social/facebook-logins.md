---
title: ASP.NET Core 中的 Facebook 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Facebook 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 8bb22dc6df9879e827ff9a5ac11e9e3ad5346dc2
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121501"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Facebook 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者使用自己的 Facebook 帳戶使用範例 ASP.NET Core 2.0 專案上建立登入[上一頁](xref:security/authentication/social/index)。 Facebook 驗證所需[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet 套件。 我們先建立 Facebook 應用程式識別碼[官方步驟](https://developers.facebook.com)。

## <a name="create-the-app-in-facebook"></a>在 Facebook 中建立應用程式

* 瀏覽至[Facebook 開發人員應用程式](https://developers.facebook.com/apps/)頁面上，並登入。 如果您還沒有的 Facebook 帳戶，使用**註冊 Facebook**在登入 頁面上，若要建立一個連結。

* 點選**將新的應用程式新增**來建立新的應用程式識別碼。 右上角的按鈕

   ![Facebook 開發人員入口網站的 Microsoft Edge 中開啟](index/_static/FBMyApps.png)

* 填寫表單，然後點選**建立應用程式識別碼** 按鈕。

  ![建立新的應用程式識別碼的表單](index/_static/FBNewAppId.png)

* 在 **選取的產品**頁面上，按一下**設定**上**Facebook 登入**卡。

  ![產品 [安裝] 頁面](index/_static/FBProductSetup.png)

* **快速入門**精靈將會啟動具有**選擇平台**做的第一頁。 現在略過精靈，依序按一下**設定**在左側功能表中的連結：

  ![略過快速入門](index/_static/FBSkipQuickStart.png)

* 您會看到**用戶端 OAuth 設定**頁面：

  ![用戶端 OAuth 設定 頁面](index/_static/FBOAuthSetup.png)

* 輸入您的開發 URI 與 */signin-facebook*附加至**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-facebook`)。 稍後在本教學課程中設定 Facebook 驗證會自動處理在要求 */signin-facebook*實作 OAuth 流程的路由。

> [!NOTE]
> URI */signin-facebook*設為預設回呼 Facebook 驗證提供者。 您可以變更預設的回呼 URI 時設定 Facebook 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions)類別。

* 按一下 **儲存變更**。

* 按一下 **設定** > **基本**在左側導覽中的連結。

  在此頁面上，記下您`App ID`和您`App Secret`。 您會新增至 ASP.NET Core 應用程式下一節：

* 部署站台時，您必須先討論一下**Facebook 登入**安裝頁面，並註冊新的公用 URI。

## <a name="store-facebook-app-id-and-app-secret"></a>儲存 Facebook 應用程式識別碼和應用程式祕密

連結機密設定，例如 Facebook`App ID`並`App Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`。

執行下列命令，以安全地儲存`App ID`和`App Secret`使用 Secret Manager:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>設定 Facebook 驗證

::: moniker range=">= aspnetcore-2.0"

將 Facebook 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

安裝[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook)封裝。

* 若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。
* 若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

新增在 Facebook 中的介軟體`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

請參閱[FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Facebook 驗證所支援的組態選項的詳細資訊的 API 參考。 組態選項可以用來：

* 要求不同使用者的相關資訊。
* 新增查詢字串引數來自訂登入體驗。

## <a name="sign-in-with-facebook"></a>使用 Facebook 登入

執行您的應用程式，然後按一下**登入**。 您會看到一個選項來使用 Facebook 登入。

![Web 應用程式： 未驗證的使用者](index/_static/DoneFacebook.png)

當您按一下**Facebook**，將您重新導向至 Facebook 進行驗證：

![Facebook 驗證 頁面](index/_static/FBLogin.png)

Facebook 驗證會要求預設公用設定檔和電子郵件地址：

![Facebook 驗證頁面同意畫面](index/_static/FBLoginDone.png)

一旦您輸入您的 Facebook 認證將您重新導向回您的網站，您可以在其中設定您的電子郵件。

您現在用來登入您的 Facebook 認證：

![Web 應用程式： 驗證使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* **ASP.NET Core 2.x 僅：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文說明如何您可以使用 Facebook 進行驗證。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。

* 一旦您發行您的網站至 Azure web 應用程式時，您應該重設`AppSecret`Facebook 開發人員入口網站中。

* 設定`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
