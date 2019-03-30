---
title: ASP.NET Core 中的 Google 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 1328bcbce3e6e4786f9d410d1f28f309dc9d2722
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750541"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Google 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Google 開始在 2019 年 1 月[關閉](https://developers.google.com/+/api-shutdown)Google + 登入和開發人員必須將移至新的 Google 登系統中的年 3 月。 ASP.NET Core 2.1 和 2.2 Google 驗證的封裝將會更新在 2 月的變更。 如需詳細資訊和 ASP.NET core 的暫存緩和措施，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/6486)。 本教學課程已更新新的安裝程序。

本教學課程會示範如何讓使用者使用其使用 ASP.NET Core 2.2 專案上建立的 Google 帳戶登入[上一頁](xref:security/authentication/social/index)。

## <a name="create-a-google-api-console-project-and-client-id"></a>建立 Google API 主控台中的專案和用戶端識別碼

* 瀏覽至[整合 Google 登入到您的 web 應用程式](https://developers.google.com/identity/sign-in/web/devconsole-project)，然後選取**設定專案**。
* 在 **設定您的 OAuth 用戶端**對話方塊中，選取**Web 伺服器**。
* 在 **授權重新導向 Uri**文字方塊項目中，設定重新導向 URI。 例如： `https://localhost:5001/signin-google` 
* 儲存**用戶端識別碼**並**用戶端祕密**。
* 當部署的網站，註冊新的公用 url，從**Google 主控台**。

## <a name="store-google-clientid-and-clientsecret"></a>存放區 Google ClientID 和 ClientSecret

儲存機密設定，例如 Google`Client ID`並`Client Secret`具有[Secret Manager](xref:security/app-secrets)。 基於本教學課程的目的，名稱語彙基元`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`:

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

您可以管理您的 API 認證和使用量[API 主控台](https://console.developers.google.com/apis/dashboard)。

## <a name="configure-google-authentication"></a>設定 Google 驗證

新增 Google 服務，以`Startup.ConfigureServices`。

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>使用 Google 登入

* 執行應用程式，然後按一下**登入**。 若要使用 Google 登入的選項會出現。
* 按一下  **Google**按鈕，將重新導向至 Google 進行驗證。
* 輸入您的 Google 認證之後, 您將被重新導向回到網站。

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

請參閱[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API 參考，如需有關 Google 驗證所支援的組態選項。 這可用來要求不同使用者的相關資訊。

## <a name="change-the-default-callback-uri"></a>變更預設的回呼 URI

URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。 您可以變更預設的回呼 URI 時設定 Google 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別。

## <a name="troubleshooting"></a>疑難排解

* 如果登入無法運作，而且您未收到任何錯誤，切換到開發模式，以便讓您更輕鬆地偵錯問題。
* 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證會導致*ArgumentException:必須提供 'SignInScheme' 選項*。 在本教學課程中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文說明如何您可以使用 Google 進行驗證。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。
* 一旦您將應用程式發佈至 Azure 時，會重設`ClientSecret`Google API 主控台中。
* 設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
