---
title: "Twitter 外部登入安裝程式"
author: rick-anderson
description: "本教學課程示範的整合到現有的 ASP.NET Core 應用程式的 Twitter 帳戶使用者驗證。"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4fd1c2d59cc277ef3781df5e306e4a5e6064520a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="configuring-twitter-authentication"></a>設定 Twitter 驗證

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會示範如何讓使用者[使用 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)上使用範例 ASP.NET Core 2.0 專案建立[上一頁](index.md)。

## <a name="create-the-app-in-twitter"></a>在 Twitter 中建立應用程式

* 瀏覽至[https://apps.twitter.com/](https://apps.twitter.com/)並登入。 如果您還沒有 Twitter 帳戶，使用**[立即註冊](https://twitter.com/signup)**建立一個連結。 登入之後,**應用程式管理**頁面會顯示：

![Twitter 應用程式管理的 Microsoft Edge 開啟](index/_static/TwitterAppManage.png)

* 點選**建立新的應用程式**填妥應用程式和**名稱**，**描述**和公用**網站**（這可能是暫時性直到您的 URI註冊的網域名稱）：

![建立應用程式頁面](index/_static/TwitterCreate.png)

* 輸入您的開發 URI 與*/signin-twitter*附加到**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-twitter`)。 稍後在本教學課程中設定的 Twitter 驗證配置將會自動處理在要求*/signin-twitter*實作 OAuth 流程的路由。

* 填寫表單的其餘部分，並點選**建立應用程式 Twitter**。 會顯示新的應用程式詳細資料：

![應用程式頁面的詳細資料 索引標籤](index/_static/TwitterAppDetails.png)

* 部署站台時必須再次瀏覽**應用程式管理**頁面上，並註冊新的公用 URI。

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>儲存 Twitter ConsumerKey 和 ConsumerSecret

連結機密設定，例如 Twitter`Consumer Key`和`Consumer Secret`您應用程式組態使用[密碼管理員](../../app-secrets.md)。 基於本教學課程的目的，名稱語彙基元`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`。

這些語彙基元可以找到上**機碼和存取權杖**之後建立新的 Twitter 應用程式 索引標籤：

![索引鍵和存取權杖 索引標籤](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>設定 Twitter 驗證

在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter)封裝是否已經安裝。

* 若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。
* 若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Twitter 服務中的新增`ConfigureServices`方法中的*Startup.cs*檔案：

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

加入在 Twitter 中的介軟體`Configure`方法中的*Startup.cs*檔案：

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

請參閱[TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API 參考，如需有關 Twitter 驗證所支援的組態選項。 這可以用於要求的使用者不同的資訊。

## <a name="sign-in-with-twitter"></a>使用 Twitter 登入

執行您的應用程式，然後按一下**登入**。 使用 Twitter 登入選項會出現：

![Web 應用程式： 未驗證的使用者](index/_static/DoneTwitter.png)

按一下**Twitter**重新導向至 Twitter 驗證：

![Twitter 驗證頁面](index/_static/TwitterLogin.png)

輸入您的 Twitter 認證之後, 您將會重新導向回 web 站台，您可以設定您的電子郵件。

您現在會記錄在使用您的 Twitter 認證：

![Web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a>疑難排解

* **ASP.NET Core 2.x 僅：**如果身分識別不藉由呼叫設定`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這已完成。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。 點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 這篇文章會示範您可以向 Twitter。 您可以依照類似的方法，來向其他提供者列在[上一頁](index.md)。

* 一旦您將您的網站發行至 Azure web 應用程式時，您應該重設`ConsumerSecret`Twitter 開發人員入口網站中。

* 設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`為在 Azure 入口網站應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
