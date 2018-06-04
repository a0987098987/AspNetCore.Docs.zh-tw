---
title: 在 ASP.NET Core Google 外部登入安裝程式
author: rick-anderson
description: 本教學課程示範的整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: ccb771dbefefb007aede1bdf05ab50ec363a3089
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689031"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>在 ASP.NET Core Google 外部登入安裝程式

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者使用 Google + 帳戶使用範例 ASP.NET Core 2.0 專案上建立登入[上一頁](xref:security/authentication/social/index)。 遵循我們啟動[官方步驟](https://developers.google.com/identity/sign-in/web/devconsole-project)在 Google API 主控台中建立新的應用程式。

## <a name="create-the-app-in-google-api-console"></a>在 Google API 主控台中建立應用程式

* 瀏覽至[ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library)並登入。 如果您還沒有 Google 帳戶，使用**更多選項** > **[建立帳戶](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** 建立一個連結：

![Google API 主控台](index/_static/GoogleConsoleLogin.png)

* 將您重新導向**API Manager 程式庫**頁面：

![管理員 API 程式庫 頁面](index/_static/GoogleConsoleSwitchboard.png)

* 點選**建立**並輸入您**專案名稱**:

![[新增專案] 對話](index/_static/GoogleConsoleNewProj.png)

* 之後接受對話方塊，您會重新導向至 [程式庫] 頁面可讓您選擇新的應用程式的功能。 尋找**Google + API**清單中，按一下 新增 API 功能的連結：

![管理員 API 程式庫 頁面](index/_static/GoogleConsoleChooseApi.png)

* 新加入的 API 頁面會隨即顯示。 點選**啟用**Google + 號功能中加入您的應用程式：

![API Manager Google + API 頁面](index/_static/GoogleConsoleEnableApi.png)

* 在之後啟用 API，點選**建立認證**設定密碼：

![API Manager Google + API 頁面](index/_static/GoogleConsoleGoCredentials.png)

* 選擇：
   * **Google + 應用程式開發介面**
   * **網頁伺服器 (例如 node.js Tomcat)**，和
   * **使用者資料**:

![API 管理員認證 頁面中： 了解什麼種類的認證需要面板](index/_static/GoogleConsoleChooseCred.png)

* 點選**我需要哪些認證？** 這會引領您的應用程式設定中，第二步**建立 OAuth 2.0 用戶端識別碼**:

![API 管理員認證 頁面上： 建立 OAuth 2.0 用戶端識別碼](index/_static/GoogleConsoleCreateClient.png)

* 因為我們建立 Google + 專案的一個功能 （登入），我們可以輸入相同**名稱**OAuth 2.0 用戶端識別碼，我們使用的專案。

* 輸入您的開發 URI 與 */signin-google*附加到**授權重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-google`)。 稍後在本教學課程設定 Google 驗證將會自動處理在要求 */signin-google*實作 OAuth 流程的路由。

* 按 TAB 鍵以新增**授權重新導向 Uri**項目。

* 點選**建立用戶端識別碼**，這樣會帶您到第三個步驟中，**設定 OAuth 2.0 同意畫面**:

![API 管理員認證 頁面上： 設定 OAuth 2.0 同意畫面](index/_static/GoogleConsoleAddCred.png)

* 輸入您的對外**電子郵件地址**和**產品名稱**時提示使用者登入 Google + 顯示您的應用程式。 底下的其他選項可用**更多的自訂選項**。

* 點選**繼續**繼續進行到最後一個步驟中，**下載認證**:

![API 管理員認證 頁面上： 下載認證](index/_static/GoogleConsoleFinish.png)

* 點選**下載**儲存 JSON 檔案，應用程式密碼和**完成**，完成建立新的應用程式。

* 部署站台時必須再次瀏覽**Google 主控台**並註冊新的公用 url。

## <a name="store-google-clientid-and-clientsecret"></a>存放區 Google ClientID 和 ClientSecret

連結機密設定，例如 Google`Client ID`和`Client Secret`您應用程式組態使用[密碼管理員](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`。

這些語彙基元的值可以在下一個步驟中下載 JSON 檔案中找到`web.client_id`和`web.client_secret`。

## <a name="configure-google-authentication"></a>設定 Google 驗證

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

加入在 Google 服務`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)安裝封裝。

* 若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。
* 若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

新增 Google 中的介軟體中`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

請參閱[GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API 參考，如需 Google 驗證所支援的組態選項的詳細資訊。 這可以用於要求的使用者不同的資訊。

## <a name="sign-in-with-google"></a>使用 Google 登入

執行您的應用程式，然後按一下**登入**。 使用 Google 登入選項會出現：

![在 Microsoft Edge 中執行的 web 應用程式： 未驗證的使用者](index/_static/DoneGoogle.png)

當您按一下 Google 時，您會重新導向至 Google 驗證：

![Google 驗證對話方塊](index/_static/GoogleLogin.png)

輸入您的 Google 認證之後, 再將您重新導向回 web 站台，您可以設定您的電子郵件。

您現在會記錄在使用您的 Google 認證：

![在 Microsoft Edge 中執行的 web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a>疑難排解

* 如果您收到`403 (Forbidden)`錯誤頁面，從您自己的應用程式時執行，在開發模式 （或相同的錯誤與偵錯工具中斷），請確認**Google + API**中已啟用**API Manager 程式庫**遵循所列的步驟[更早版本在此頁面](#create-the-app-in-google-api-console)。 如果登入無法運作，且您未收到任何錯誤訊息，切換到開發模式來進行問題偵錯更容易。
* **ASP.NET Core 2.x 僅：** 如果身分識別不藉由呼叫設定`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這已完成。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。 點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 這篇文章會示範您可以向 Google。 您可以依照類似的方法，來向其他提供者列在[上一頁](xref:security/authentication/social/index)。

* 一旦您將您的網站發行至 Azure web 應用程式時，您應該重設`ClientSecret`Google API 主控台中。

* 設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`為在 Azure 入口網站應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
