---
title: 使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Microsoft 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750999"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者使用其使用範例 ASP.NET Core 2.0 專案上建立的 Microsoft 帳戶登入[上一頁](xref:security/authentication/social/index)。

## <a name="create-the-app-in-microsoft-developer-portal"></a>在 Microsoft 開發人員入口網站中建立應用程式

* 瀏覽至[ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com)和建立，或登入 Microsoft 帳戶：

![登入對話方塊](index/_static/MicrosoftDevLogin.png)

如果您還沒有 Microsoft 帳戶，請點選 **[建立一個 ！](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** 登入之後將您重新導向 **我的應用程式** 頁面：

![開啟 Microsoft Edge 中的 Microsoft 開發人員入口網站](index/_static/MicrosoftDev.png)

* 點選**新增應用程式**右上角，然後輸入您**應用程式名稱**並**Contact Email**:

![新的應用程式註冊 對話方塊](index/_static/MicrosoftDevAppCreate.png)

* 基於本教學課程的目的，請清除**引導式設定**核取方塊。

* 點選**Create**繼續**註冊**頁面。 提供**名稱**，並記下的值**應用程式識別碼**，使用即`ClientId`稍後的教學課程：

![註冊頁面](index/_static/MicrosoftDevAppReg.png)

* 點選**新增平台**中**平台**區段，然後選取**Web**平台：

![新增平台 對話方塊](index/_static/MicrosoftDevAppPlatform.png)

* 在新**Web**平台區段中，輸入與您開發的 URL`/signin-microsoft`附加到**重新導向 Url**欄位 (例如： `https://localhost:44320/signin-microsoft`)。 稍後在本教學課程中設定的 Microsoft 驗證配置會自動處理在要求`/signin-microsoft`實作 OAuth 流程的路由：

![Web 平台 」 一節](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> URI 區段`/signin-microsoft`設為預設回呼的 Microsoft 驗證提供者。 您可以變更預設的回呼 URI 時設定 Microsoft 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別。

* 點選**加入 URL**確定 URL 已新增。

* 填寫任何其他應用程式設定，如有必要，點選**儲存**底部的 [將變更儲存至應用程式設定] 頁面。

* 部署站台時您將必須再次瀏覽**註冊**頁面，然後將新的公用 URL。

## <a name="store-microsoft-application-id-and-password"></a>儲存 Microsoft 應用程式識別碼和密碼

* 附註`Application Id`上顯示**註冊**頁面。

* 點選**產生新密碼**中**應用程式祕密**一節。 這會顯示的方塊，您可以在其中複製應用程式密碼：

![新產生的密碼 對話方塊](index/_static/MicrosoftDevPassword.png)

連結機密設定，例如 Microsoft`Application ID`並`Password`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>設定 Microsoft 帳戶驗證

在本教學課程中使用的專案範本可確保[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)已經安裝套件。

* 若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。
* 若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

新增 Microsoft 帳戶服務中的`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

新增在 Microsoft 帳戶中的介軟體`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

雖然 Microsoft 開發人員入口網站所使用的術語命名這些語彙基元`ApplicationId`並`Password`，它們會公開為`ClientId`和`ClientSecret`組態 API。

請參閱[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考，如需有關 Microsoft 帳戶驗證所支援的組態選項。 這可用來要求不同使用者的相關資訊。

## <a name="sign-in-with-microsoft-account"></a>使用 Microsoft 帳戶登入

執行您的應用程式，然後按一下**登入**。 此時會出現 使用 Microsoft 登入選項：

![Web 應用程式記錄檔 頁面中：未經驗證的使用者](index/_static/DoneMicrosoft.png)

當您按一下 Microsoft 時，您會重新導向到 Microsoft 進行驗證。 之後您 Microsoft 帳戶登入 （如果您尚未登入） 將會提示您讓應用程式存取您的資訊：

![Microsoft 驗證對話方塊](index/_static/MicrosoftLogin.png)

點選**是**您就會重新導向回 web 站台，您可以在其中設定您的電子郵件。

您現在用來登入您的 Microsoft 認證：

![Web 應用程式：已驗證的使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* 如果 Microsoft 帳戶提供者會將您導向登入錯誤頁面，請注意錯誤標題和描述查詢字串參數直接跟`#`（主題標籤） 中的 Uri。

  雖然指出使用 Microsoft 驗證問題似乎出現錯誤訊息，最常見的原因是您的應用程式 Uri 不符合任一**重新導向 Uri**指定**Web**平台.
* **ASP.NET Core 2.x 只有：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文章說明您可以向 Microsoft。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。

* 一旦您發行您的網站至 Azure web 應用程式時，您應該建立新`Password`Microsoft 開發人員入口網站中。

* 設定`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
