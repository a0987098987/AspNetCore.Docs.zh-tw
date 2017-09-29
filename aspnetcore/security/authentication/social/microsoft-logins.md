---
title: "外部登入的 Microsoft 帳戶設定"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: cb4ea63664f29e39c2dd26cbf814a484a295ec6c
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-microsoft-account-authentication"></a>設定 Microsoft 帳戶驗證

<a name=security-authentication-microsoft-logins></a>

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者使用範例 ASP.NET Core 2.0 專案上建立其 Microsoft 帳戶登入[上一頁](index.md)。

## <a name="create-the-app-in-microsoft-developer-portal"></a>在 Microsoft 開發人員入口網站中建立應用程式

* 瀏覽至[https://apps.dev.microsoft.com](https://apps.dev.microsoft.com)以及建立或登入 Microsoft 帳戶：

![登入對話方塊](index/_static/MicrosoftDevLogin.png)

如果您還沒有 Microsoft 帳戶，請點選**[建立一個 ！](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** 在登入後將您重新導向**我的應用程式**頁面：

![在 Microsoft Edge 中開啟的 Microsoft 開發人員入口網站](index/_static/MicrosoftDev.png)

* 點選**新增應用程式**在右上角，然後輸入您**應用程式名稱**和**Contact Email**:

![新的應用程式註冊 對話方塊](index/_static/MicrosoftDevAppCreate.png)

* 基於本教學課程的目的，清除**指引安裝**核取方塊。

* 點選**建立**繼續**註冊**頁面。 提供**名稱**並記下的值**應用程式識別碼**，作為使用`ClientId`稍後在本教學課程：

![註冊頁面](index/_static/MicrosoftDevAppReg.png)

* 點選**加入平台**中**平台**區段，然後選取**Web**平台：

![加入平台 對話方塊](index/_static/MicrosoftDevAppPlatform.png)

* 在新**Web**平台區段中，輸入您開發的 URL 與*/signin-microsoft*附加到**重新導向 Url**欄位 (例如： `https://localhost:44320/signin-microsoft`)。 稍後在本教學課程中設定的 Microsoft 驗證配置將會自動處理在要求*/signin-microsoft*實作 OAuth 流程路由：

![Web 平台 > 一節](index/_static/MicrosoftRedirectUri.png)

* 點選**新增 URL**確定 URL 已新增。

* 填寫任何其他應用程式設定，如有必要，點選 **儲存**將變更儲存到應用程式組態頁面底部。

* 部署站台時必須再次瀏覽**註冊**頁面，然後設定新的公用 URL。

## <a name="store-microsoft-application-id-and-password"></a>儲存 Microsoft 應用程式識別碼和密碼

* 請注意`Application Id`上顯示**註冊**頁面。

* 點選**產生新的密碼**中**應用程式密碼**> 一節。 這會顯示的方塊，您可以在其中複製應用程式密碼：

![新產生的密碼對話方塊](index/_static/MicrosoftDevPassword.png)

連結機密設定，例如 Microsoft`Application ID`和`Password`您應用程式組態使用[密碼管理員](../../app-secrets.md)。 基於本教學課程的目的，名稱語彙基元`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`。

## <a name="configure-microsoft-account-authentication"></a>設定 Microsoft 帳戶驗證

在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount)封裝是否已經安裝。

* 若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。
* 若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

新增 Microsoft 帳戶服務中的`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

新增 Microsoft 帳戶中的介軟體中`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

雖然 Microsoft 開發人員入口網站上所使用的術語命名這些語彙基元`ApplicationId`和`Password`，它們會公開為`ClientId`和`ClientSecret`組態 API。

請參閱[MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考，如需有關 Microsoft 帳戶驗證所支援的組態選項。 這可以用於要求的使用者不同的資訊。

## <a name="sign-in-with-microsoft-account"></a>使用 Microsoft 帳戶登入

執行您的應用程式，然後按一下**登入**。 使用 Microsoft 登入選項會出現：

![Web 應用程式記錄檔中頁面： 未驗證的使用者](index/_static/DoneMicrosoft.png)

當您按一下 on Microsoft 時，您會重新導向到 Microsoft 進行驗證。 之後您 Microsoft 帳戶登入 （如果尚未登入） 系統會提示您讓應用程式存取您的資訊：

![Microsoft 驗證對話方塊](index/_static/MicrosoftLogin.png)

點選**是**您就會重新導向回 web 站台，您可以設定您的電子郵件。

您現在會記錄在使用您的 Microsoft 認證：

![Web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a>疑難排解

* 如果 Microsoft 帳戶提供者將您重新導向至登入錯誤頁面，請注意錯誤標題和描述查詢字串參數緊接`#`（雜湊標記） 之 Uri 中。

  雖然指出 Microsoft 驗證問題，看起來的錯誤訊息，但最常見的原因是您的應用程式 Uri 不符合任何的**重新導向 Uri**指定**Web**平台.
* **ASP.NET Core 2.x 僅：**如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這已完成。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。 點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 這篇文章會示範您可以向 Microsoft。 您可以依照類似的方法，來向其他提供者列在[上一頁](index.md)。

* 一旦您將您的網站發行至 Azure web 應用程式時，您應該建立新`Password`Microsoft 開發人員入口網站中。

* 設定`Authentication:Microsoft:ApplicationId`和`Authentication:Microsoft:Password`為在 Azure 入口網站應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
