---
title: 帶有ASP.NET核心的Twitter外部登入設定
author: rick-anderson
description: 本教程演示了將Twitter帳戶使用者身份驗證整合到現有ASP.NET核心應用。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 1f5d667e905e49ae05f5aa31bd5b69ad126f6e28
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277284"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>帶有ASP.NET核心的Twitter外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

此範例展示如何允許使用者使用[上一頁上](xref:security/authentication/social/index)建立的範例ASP.NET Core 3.0 專案[使用 Twitter 帳戶登入](https://dev.twitter.com/web/sign-in/desktop-browser)。

## <a name="create-the-app-in-twitter"></a>在 Twitter 中建立應用程式

* 將[Microsoft.AspNetCore.身份驗證.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet 包添加到專案中。

* 導航到[https://apps.twitter.com/](https://apps.twitter.com/)並登錄。 如果您還沒有 Twitter 帳戶,請使用**[「立即註冊」](https://twitter.com/signup)** 連結創建一個帳戶。

* 選擇 **「創建應用**」。 填寫**應用程式與**公共**網站**URI(在註冊網域名稱之前,這可能是暫時的):**Application description**

* 選取使用 Twitter**啟用登入**旁邊的複選盒

* 默認情況下,Microsoft.AspNetCore.標識要求用戶擁有電子郵寄地址。 轉到 **"權限'** 選項卡,按下 **"編輯"** 按鈕,然後選中 **「向使用者請求電子郵件位址」** 旁邊的複選框。

* 輸入您的開發 URI,`/signin-twitter`並 附加到**回檔網址**欄`https://webapp128.azurewebsites.net/signin-twitter`位(例如: 。 此範例中稍後設定的 Twitter 身份驗證方案將自動處理路`/signin-twitter`由中 實現 OAuth 流的請求。

  > [!NOTE]
  > URI`/signin-twitter`段設置為 Twitter 身份驗證提供程式的預設回調。 您可以透過[TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)類別的繼承[遠端身份驗證選項.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性設定 Twitter 身份驗證中間件,同時更改預設回檔 URI。

* 填寫表單的其餘部分並選擇 **"創建**"。 將顯示新的應用程式詳細資訊:

## <a name="store-the-twitter-consumer-api-key-and-secret"></a>儲存 Twitter 消費者 API 金鑰和機密

儲存敏感設定,如Twitter消費者API金鑰和[秘密管理員](xref:security/app-secrets)。 對於此示例,請使用以下步驟:

1. 根據[啟用密鑰存儲](xref:security/app-secrets#enable-secret-storage)的指令初始化了機密存儲的專案。
1. 使用機金鑰`Authentication:Twitter:ConsumerKey`將敏感設定儲存在本地機密儲存中,並`Authentication:Twitter:ConsumerSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

建立新的 Twitter 應用程式後,可以在 **「密鑰和存取權杖」** 選項卡上找到這些權杖:

## <a name="configure-twitter-authentication"></a>設定 Twitter 認證

在`ConfigureServices`*Startup.cs*檔案中的方法中加入 Twitter 服務:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

有關 Twitter 身份驗證支援的配置選項的詳細資訊,請參閱[TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API 參考。 這可用於請求有關使用者的不同資訊。

## <a name="sign-in-with-twitter"></a>使用 Twitter 登入

運行應用並選擇 **「登錄」。** 使用 Twitter 登入的選項:

點擊**Twitter**重定向到 Twitter 進行身份驗證:

輸入 Twitter 認證後,您將被重定向回可以設置電子郵件的網站。

您現在使用 Twitter 認證登入:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

<!-- 
### React to cancel Authorize External sign-in
Twitter doesn't support AccessDeniedPath
Rather in the twitter setup, you can provide an External sign-in homepage. The external sign-in homepage doesn't support localhost. Tested with https://cors3.azurewebsites.net/ and that works.
-->

## <a name="troubleshooting"></a>疑難排解

* **只有ASP.NET核心 2.x:** 如果未透過除錯`services.AddIdentity`設定識別`ConfigureServices`,則試著認證會導致*參數異常:必須提供「SignInScheme」選項*。 此示例中使用的專案範本可確保完成此操作。
* 如果尚未透過應用程式初始移至建立網站資料庫,則在處理請求錯誤時,將取得*資料庫操作失敗*。 點按 **「應用遷移**」以建立資料庫並刷新以繼續結束錯誤。

## <a name="next-steps"></a>後續步驟

* 本文展示了如何使用 Twitter 進行身份驗證。 您可以採用類似的方法對[上一頁](xref:security/authentication/social/index)中列出的其他提供程式進行身份驗證。

* 將網站發布到 Azure Web 應用程式後,應`ConsumerSecret`重設 Twitter 開發人員門戶中的 。

* 在`Authentication:Twitter:ConsumerKey`Azure`Authentication:Twitter:ConsumerSecret`門戶中將 和 設置為應用程式設置。 配置系統設置為從環境變數讀取密鑰。
