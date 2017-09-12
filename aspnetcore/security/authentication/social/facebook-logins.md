---
title: "在 ASP.NET Core Facebook 外部登入安裝程式"
author: rick-anderson
description: "在 ASP.NET Core Facebook 外部登入安裝程式"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 8/1/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: da019ad3fd6cefa23b8331c98cc36e50ac9c1105
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="configuring-facebook-authentication"></a>設定 Facebook 驗證

<a name=security-authentication-facebook-logins></a>

由[Valeriy Novytskyy](https://github.com/01binary)和[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者以使用範例 ASP.NET Core 2.0 專案上建立其 Facebook 帳戶登入[上一頁](index.md)。 我們先建立 Facebook 應用程式識別碼依照[官方步驟](https://www.facebook.com/unsupportedbrowser)。

## <a name="create-the-app-in-facebook"></a>在 Facebook 中建立應用程式

*  瀏覽至[Facebook 開發人員](https://www.facebook.com/unsupportedbrowser)頁面上，並登入。 如果您還沒有的 Facebook 帳戶，使用**註冊 Facebook**建立一個登入頁面上的連結。

* 點選**建立應用程式**按鈕右上角來建立新的應用程式識別碼。

   ![在 Microsoft Edge 開啟 Facebook 開發人員入口網站](index/_static/FBMyApps.png)

* 填寫表單，然後點選**建立應用程式識別碼** 按鈕。

   ![建立新的應用程式識別碼的表單](index/_static/FBNewAppId.png)

* 當有**選取產品**提示字元中，按一下**設定**上**Facebook 登入**卡。

   ![產品安裝程式頁面](index/_static/FBProductSetup.png)

* **快速入門**精靈將會啟動具有**選擇平台**做為第一頁。 立即略過此精靈，即可**設定**左側功能表中的連結：

   ![略過快速入門](index/_static/FBSkipQuickStart.png)

* 您會看到**用戶端的 OAuth 設定**頁面：

![用戶端的 OAuth 設定頁面](index/_static/FBOAuthSetup.png)

* 輸入您的開發 URI 與*/signin-facebook*附加到**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-facebook`)。 稍後在本教學課程中設定的 Facebook 驗證將會自動處理在要求*/signin-facebook*實作 OAuth 流程的路由。

* 按一下**儲存變更**。

* 按一下**儀表板**位於左邊功能窗格中的連結。 

    這個頁面上，請記下您`App ID`和您`App Secret`。 您將在下一節加入到您的 ASP.NET Core 應用程式：

   ![Facebook 開發人員儀表板](index/_static/FBDashboard.png)

* 部署站台時需要檢討**Facebook 登入**安裝頁面上，並註冊新的公用 URI。

## <a name="store-facebook-app-id-and-app-secret"></a>儲存 Facebook 應用程式識別碼和應用程式密碼

連結機密設定，例如 Facebook`App ID`和`App Secret`您應用程式組態使用[密碼管理員](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`。

## <a name="configure-facebook-authentication"></a>設定 Facebook 驗證

在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook)封裝是否已經安裝。

* 若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。
* 若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

加入在 Facebook 服務`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

`AddAuthentication`方法只能呼叫一次時新增多個驗證提供者。 後續呼叫可能會覆寫任何先前設定的[AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions)屬性。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

新增 Facebook 中的介軟體中`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

請參閱[FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API 參考，如需有關支援 Facebook 驗證的組態選項。 組態選項可用來：

* 要求的使用者不同的資訊。
* 加入自訂登入體驗的查詢字串引數。

## <a name="sign-in-with-facebook"></a>使用 Facebook 登入

執行您的應用程式，然後按一下**登入**。 您會看到一個選項來使用 Facebook 登入。

![Web 應用程式： 未驗證的使用者](index/_static/DoneFacebook.png)

當您按一下  **Facebook**，將您重新導向 Facebook 進行驗證：

![Facebook 驗證頁面](index/_static/FBLogin.png)

Facebook 驗證會要求預設公用設定檔和電子郵件地址：

![Facebook 驗證頁面](index/_static/FBLoginDone.png)

一旦您輸入您的 Facebook 認證將您重新導向回您的網站，您可以設定您的電子郵件。

您現在會記錄在使用您的 Facebook 認證：

![Web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a>疑難排解

* **ASP.NET Core 2.x 僅：**如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這已完成。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。 點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 這篇文章會示範您可以向 Facebook。 您可以依照類似的方法，來向其他提供者列在[上一頁](index.md)。

* 一旦您將您的網站發行至 Azure web 應用程式時，您應該重設`AppSecret`Facebook 開發人員入口網站中。

* 設定`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`為在 Azure 入口網站應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
