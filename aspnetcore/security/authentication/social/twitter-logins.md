---
title: 使用 ASP.NET Core 的 twitter 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Twitter 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735800"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>使用 ASP.NET Core 的 twitter 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者[使用他們的 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)上使用範例 ASP.NET Core 2.0 專案建立[上一頁](xref:security/authentication/social/index)。

## <a name="create-the-app-in-twitter"></a>在 Twitter 中建立應用程式

* 瀏覽至[ https://apps.twitter.com/ ](https://apps.twitter.com/)並登入。 如果您還沒有 Twitter 帳戶，使用 **[歡迎現在註冊](https://twitter.com/signup)** 連結建立一個。 登入之後，**應用程式管理**頁面會顯示：

  ![在 Microsoft Edge 中開啟的 twitter 應用程式管理](index/_static/TwitterAppManage.png)

* 點選**建立新的應用程式**並填妥應用程式**名稱**，**描述**和公用**網站**（這可以是暫時直到您的 URI註冊的網域名稱）：

  ![建立應用程式頁面](index/_static/TwitterCreate.png)

* 輸入您的開發 URI 與`/signin-twitter`附加至**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-twitter`)。 本教學課程稍後設定 Twitter 驗證配置會自動處理在要求`/signin-twitter`實作 OAuth 流程的路由。

  > [!NOTE]
  > URI 區段`/signin-twitter`設為 Twitter 驗證提供者的預設回呼。 您可以變更預設的回呼 URI 時設定 Twitter 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別。

* 填寫表單的其餘部分，然後點選**建立 Twitter 應用程式**。 會顯示新的應用程式詳細資料：

  ![應用程式頁面的詳細資料 索引標籤](index/_static/TwitterAppDetails.png)

* 部署站台時您將必須再次瀏覽**應用程式管理**頁面上，並註冊新的公用 URI。

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>儲存 Twitter ConsumerKey 和 ConsumerSecret

連結機密設定，例如 Twitter`Consumer Key`並`Consumer Secret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。

這些權杖可於**金鑰和存取權杖**之後建立新的 Twitter 應用程式 索引標籤：

![金鑰和存取權杖 索引標籤](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>設定 Twitter 驗證

在本教學課程中使用的專案範本可確保[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)已經安裝套件。

* 若要使用 Visual Studio 2017 安裝此套件，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 套件**。
* 若要使用.NET Core CLI 安裝，請執行下列命令在您的專案目錄中：

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

將 Twitter 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

新增在 Twitter 中的介軟體`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考，如 Twitter 驗證所支援的組態選項的詳細資訊。 這可用來要求不同使用者的相關資訊。

## <a name="sign-in-with-twitter"></a>使用 Twitter 登入

執行您的應用程式，然後按一下**登入**。 此時會出現以 Twitter 登入選項：

![Web 應用程式：未經驗證的使用者](index/_static/DoneTwitter.png)

按一下**Twitter**將重新導向至 Twitter 進行驗證：

![Twitter 驗證頁面](index/_static/TwitterLogin.png)

輸入您的 Twitter 認證之後, 您將被重新導向回到網站，您可以在其中設定您的電子郵件。

您現在用來登入您的 Twitter 認證：

![Web 應用程式：已驗證的使用者](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* **ASP.NET Core 2.x 只有：** 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文說明如何您可以使用 Twitter 進行驗證。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。

* 一旦您發行您的網站至 Azure web 應用程式時，您應該重設`ConsumerSecret`Twitter 開發人員入口網站中。

* 設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
