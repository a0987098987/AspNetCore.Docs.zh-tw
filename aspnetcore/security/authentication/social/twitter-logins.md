---
title: 使用 ASP.NET Core 的 Twitter 外部登錄設定
author: rick-anderson
description: 本教學課程示範如何將 Twitter 帳戶使用者驗證整合到現有的 ASP.NET Core 應用程式中。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: b848486415fd72ce6180b4cf8fc1ba00410d694a
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989746"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>使用 ASP.NET Core 的 Twitter 外部登錄設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

這個範例會示範如何使用在[上一頁](xref:security/authentication/social/index)建立的範例 ASP.NET Core 3.0 專案，讓使用者能夠以[Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)。

## <a name="create-the-app-in-twitter"></a>在 Twitter 中建立應用程式

* 將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0)新增至該專案中。

* 流覽至[https://apps.twitter.com/](https://apps.twitter.com/)並登入。 如果您還沒有 Twitter 帳戶，請使用 [ **[立即註冊](https://twitter.com/signup)** ] 連結來建立一個。

* 選取 [建立應用程式]。 填寫 [應用程式**名稱**]、[**應用程式描述**] 和 [公用**網站**URI] （這可以是暫時性的，直到您註冊功能變數名稱）：

* 核取 [**啟用使用 Twitter 登入**] 旁的核取方塊

* AspNetCore 會要求使用者預設擁有電子郵件地址。 前往 [**許可權**] 索引標籤，按一下 [**編輯**] 按鈕，然後核取 [**要求使用者的電子郵件地址**] 旁的方塊。

* 輸入您的開發 URI，並將 `/signin-twitter` 附加至 [**回呼 url** ] 欄位（例如： `https://webapp128.azurewebsites.net/signin-twitter`）。 稍後在此範例中設定的 Twitter 驗證配置，會自動處理 `/signin-twitter` 路由的要求，以執行 OAuth 流程。

  > [!NOTE]
  > URI 區段 `/signin-twitter` 會設定為 Twitter 驗證提供者的預設回呼。 您可以在設定 Twitter 驗證中介軟體時，透過[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。

* 填寫表單的其餘部分，然後選取 [**建立**]。 隨即顯示新的應用程式詳細資料：

## <a name="store-the-twitter-consumer-api-key-and-secret"></a>儲存 Twitter 取用者 API 金鑰和密碼

使用[秘密管理員](xref:security/app-secrets)來儲存機密設定，例如 Twitter 取用者 API 金鑰和密碼。 針對此範例，請使用下列步驟：

1. 根據[啟用秘密儲存](xref:security/app-secrets#enable-secret-storage)中的指示，初始化秘密儲存的專案。
1. 將機密設定儲存在本機密碼存放區中，並使用秘密金鑰 `Authentication:Twitter:ConsumerKey` 和 `Authentication:Twitter:ConsumerSecret`：

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

在建立新的 Twitter 應用程式之後，您可以在 [**金鑰和存取權杖**] 索引標籤上找到這些權杖：

## <a name="configure-twitter-authentication"></a>設定 Twitter 驗證

在*Startup.cs*檔案的 `ConfigureServices` 方法中新增 Twitter 服務：

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

如需 Twitter 驗證所支援之設定選項的詳細資訊，請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考。 這可以用來要求使用者的不同資訊。

## <a name="sign-in-with-twitter"></a>使用 Twitter 登入

執行應用程式，然後選取 [**登入**]。 隨即會出現使用 Twitter 登入的選項：

按一下 [ **twitter**重新導向至 twitter 以進行驗證]：

輸入您的 Twitter 認證之後，系統會將您重新導向回到網站，您可以在其中設定您的電子郵件。

您現在已使用 Twitter 認證登入：

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* **僅限 ASP.NET Core 2.x：** 如果未透過呼叫 `ConfigureServices`中的 `services.AddIdentity` 來設定身分識別，則嘗試驗證會導致*ArgumentException：必須提供 ' SignInScheme ' 選項*。 此範例中使用的專案範本可確保完成此作業。
* 如果尚未藉由套用初始遷移來建立網站資料庫，則在處理要求錯誤時，您將會收到*資料庫作業失敗的*情況。 請按 [套用**遷移**] 來建立資料庫，並重新整理以繼續發生錯誤。

## <a name="next-steps"></a>後續步驟

* 本文說明如何使用 Twitter 進行驗證。 您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。

* 將網站發佈至 Azure web 應用程式之後，您應該在 Twitter 開發人員入口網站中重設 `ConsumerSecret`。

* 將 [`Authentication:Twitter:ConsumerKey`] 和 [`Authentication:Twitter:ConsumerSecret`] 設定為 Azure 入口網站中的 [應用程式設定]。 設定系統已設定為從環境變數讀取金鑰。
