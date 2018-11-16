---
title: ASP.NET Core 中的 Google 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: dfda83e1d7cf3c5ff8e31de20c15d468de5d15c0
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708448"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Google 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者使用 Google + 帳戶使用範例 ASP.NET Core 2.0 專案上建立登入[上一頁](xref:security/authentication/social/index)。 我們首先請遵循[官方步驟](https://developers.google.com/identity/sign-in/web/devconsole-project)在 Google API 主控台中建立新的應用程式。

## <a name="create-the-app-in-google-api-console"></a>在 Google API 主控台中建立應用程式

* 瀏覽至[ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library)並登入。 如果您還沒有 Google 帳戶，使用**更多選項** > **[建立帳戶](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** 連結建立一個：

![Google API 主控台](index/_static/GoogleConsoleLogin.png)

* 將您重新導向**API 管理員程式庫**頁面：

![API 管理員文件庫頁面](index/_static/GoogleConsoleSwitchboard.png)

* 點選**Create**並輸入您**專案名稱**:

![[新增專案] 對話](index/_static/GoogleConsoleNewProj.png)

* 接受之後的對話方塊，將您重新導向回媒體櫃 頁面，讓您可以選擇新的應用程式的功能。 尋找**Google + API**清單中，按一下 新增 API 功能的連結：

![API 管理員文件庫頁面](index/_static/GoogleConsoleChooseApi.png)

* 新加入的 API 頁面會隨即顯示。 點選**啟用**將 Google + 號功能新增至您的應用程式：

![API 管理員 Google + API 頁面](index/_static/GoogleConsoleEnableApi.png)

* 啟用之後，API，請點選**建立認證**設定祕密：

![API 管理員 Google + API 頁面](index/_static/GoogleConsoleGoCredentials.png)

* 選擇：
  * **Google + API**
  * **網頁伺服器 (例如 node.js，Tomcat)**，及
  * **使用者資料**:

![API 管理員認證 頁面上： 找出哪些種類的認證需要面板](index/_static/GoogleConsoleChooseCred.png)

* 點選**我需要哪些認證？** 這會引領您的應用程式設定的第二個步驟**建立 OAuth 2.0 用戶端識別碼**:

![API 管理員認證 頁面上： 建立 OAuth 2.0 用戶端識別碼](index/_static/GoogleConsoleCreateClient.png)

* 因為我們會建立 Google + 專案，與只是一項特徵 （登入），我們可以輸入相同**名稱**OAuth 2.0 用戶端識別碼，為我們所使用的專案。

* 輸入您的開發 URI 與`/signin-google`附加至**授權重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-google`)。 稍後在本教學課程中設定 Google 驗證會自動處理在要求`/signin-google`實作 OAuth 流程的路由。

> [!NOTE]
> URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。 您可以變更預設的回呼 URI 時設定 Google 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別。

* 按 TAB 鍵以新增**授權重新導向 Uri**項目。

* 點選**建立用戶端識別碼**，這會帶您到第三個步驟中，**設定 OAuth 2.0 同意畫面**:

![API 管理員認證 頁面上： 設定 OAuth 2.0 同意畫面](index/_static/GoogleConsoleAddCred.png)

* 輸入您的對外**電子郵件地址**並**產品名稱**當 Google + 提示使用者登入您的應用程式所示。 在有其他選項**更多的自訂選項**。

* 點選**繼續**繼續進行到最後一個步驟中，**下載認證**:

![API 管理員認證 頁面上： 下載認證](index/_static/GoogleConsoleFinish.png)

* 點選**下載**儲存 JSON 檔案包含應用程式祕密，以及**完成**以完成建立新的應用程式。

* 部署站台時您將必須再次瀏覽**Google 主控台**並註冊新的公用 url。

## <a name="store-google-clientid-and-clientsecret"></a>存放區 Google ClientID 和 ClientSecret

連結機密設定，例如 Google`Client ID`並`Client Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`。

這些語彙基元的值可以在下一個步驟中下載的 JSON 檔案中找到`web.client_id`和`web.client_secret`。

## <a name="configure-google-authentication"></a>設定 Google 驗證

::: moniker range=">= aspnetcore-2.0"

將 Google 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：

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

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

在本教學課程中使用的專案範本可確保[Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)安裝套件。

* 若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。
* 若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

新增在 Google 中的介軟體`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

請參閱[GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API 參考，如需有關 Google 驗證所支援的組態選項。 這可用來要求不同使用者的相關資訊。

## <a name="sign-in-with-google"></a>使用 Google 登入

執行您的應用程式，然後按一下**登入**。 此時會出現 使用 Google 登入選項：

![在 Microsoft Edge 中執行的 web 應用程式： 未驗證的使用者](index/_static/DoneGoogle.png)

當您按一下 Google 時，您會重新導向至 Google 進行驗證：

![Google 驗證對話方塊](index/_static/GoogleLogin.png)

輸入您的 Google 認證之後, 再將您重新導向回您可以在其中設定您的電子郵件的網站。

您現在用來登入您的 Google 認證：

![在 Microsoft Edge 中執行的 web 應用程式： 驗證使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* 如果您收到`403 (Forbidden)`錯誤頁面，從您自己的應用程式時執行，在開發模式 （或中斷偵錯工具相同的錯誤），確保**Google + API**中已啟用**API Manager 程式庫**依照所列的步驟[早在此頁面上](#create-the-app-in-google-api-console)。 如果登入無法運作，而且您未收到任何錯誤，切換到開發模式，以便讓您更輕鬆地偵錯問題。
* **ASP.NET Core 2.x 僅：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文說明如何您可以使用 Google 進行驗證。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。

* 一旦您發行您的網站至 Azure web 應用程式時，您應該重設`ClientSecret`Google API 主控台中。

* 設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
