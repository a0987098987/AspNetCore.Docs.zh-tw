---
title: 使用 ASP.NET Core 的 Twitter 外部登錄設定
author: rick-anderson
description: 本教學課程示範如何將 Twitter 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6b6fa3e50f602a92fec9112ac3ba43583de33a70
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994286"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>使用 ASP.NET Core 的 Twitter 外部登錄設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

這個範例會示範如何使用在[上一頁](xref:security/authentication/social/index)建立的範例 ASP.NET Core 2.2 專案, 讓使用者能夠以[Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)。

## <a name="create-the-app-in-twitter"></a>在 Twitter 中建立應用程式

* 瀏覽至[ https://apps.twitter.com/ ](https://apps.twitter.com/)並登入。 如果您還沒有 Twitter 帳戶，使用 **[歡迎現在註冊](https://twitter.com/signup)** 連結建立一個。

* 按一下 [**建立新的應用**程式], 並填寫應用程式**名稱**、**描述**和公用**網站**URI (這可以是暫時性的, 直到您註冊功能變數名稱):

* 輸入您的開發 URI `/signin-twitter` , 並附加至 [**有效的 OAuth 重新導向 uri** ] `https://webapp128.azurewebsites.net/signin-twitter`欄位 (例如:)。 稍後在此範例中設定的 Twitter 驗證配置會自動處理路由`/signin-twitter`上的要求, 以執行 OAuth 流程。

  > [!NOTE]
  > URI 區段`/signin-twitter`會設定為 Twitter 驗證提供者的預設回呼。 您可以在設定 Twitter 驗證中介軟體時, 透過[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。

* 填寫表單的其餘部分, 並按一下 [**建立您的 Twitter 應用程式**]。 隨即顯示新的應用程式詳細資料:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>儲存 Twitter 取用者 API 金鑰和密碼

執行下列命令以安全地儲存`ClientId`和`ClientSecret`使用「[秘密管理員](xref:security/app-secrets)」:

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

使用[秘密管理員](xref:security/app-secrets), 將`Consumer Key` Twitter `Consumer Secret`和等機密設定連結至您的應用程式設定。 基於此範例的目的, 請將權杖`Authentication:Twitter:ConsumerKey`命名為和。 `Authentication:Twitter:ConsumerSecret`

在建立新的 Twitter 應用程式之後, 您可以在 [**金鑰和存取權杖**] 索引標籤上找到這些權杖:

## <a name="configure-twitter-authentication"></a>設定 Twitter 驗證

在`ConfigureServices` *Startup.cs*檔案的方法中新增 Twitter 服務:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

如需 Twitter 驗證所支援之設定選項的詳細資訊, 請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考。 這可用來要求不同使用者的相關資訊。

## <a name="sign-in-with-twitter"></a>使用 Twitter 登入

執行應用程式, 然後選取 [**登入**]。 隨即會出現使用 Twitter 登入的選項:

按一下 [ **twitter**重新導向至 twitter 以進行驗證]:

輸入您的 Twitter 認證之後, 系統會將您重新導向回到網站, 您可以在其中設定您的電子郵件。

您現在已使用 Twitter 認證登入:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* **僅限 ASP.NET Core 2.x:** 如果未透過在中`services.AddIdentity` `ConfigureServices`呼叫*來設定身分識別, 則嘗試進行驗證會導致 ArgumentException:必須提供*' SignInScheme ' 選項。 此範例中使用的專案範本可確保完成此作業。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文說明如何使用 Twitter 進行驗證。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。

* 將網站發佈至 Azure web 應用程式之後, 您應該`ConsumerSecret`在 Twitter 開發人員入口網站中重設。

* 設定`Authentication:Twitter:ConsumerKey`和`Authentication:Twitter:ConsumerSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
