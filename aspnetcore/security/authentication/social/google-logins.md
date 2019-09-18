---
title: ASP.NET Core 中的 Google 外部登入設定
author: rick-anderson
description: 本教學課程示範如何整合到現有的 ASP.NET Core 應用程式的 Google 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082478"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Google 外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

[從2019年3月7日起，舊版 Google + api 已關閉](https://developers.google.com/+/api-shutdown)。 Google + 登入和開發人員必須移至新的 Google 登入系統。 適用于 Google 驗證的 ASP.NET Core 2.1 和2.2 封裝已更新，以配合變更。 如需 ASP.NET Core 的詳細資訊和暫時緩和措施，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/6486)。 本教學課程已更新為新的安裝程式。

本教學課程說明如何讓使用者使用在[前一頁](xref:security/authentication/social/index)建立的 ASP.NET Core 2.2 專案，以其 Google 帳戶登入。

## <a name="create-a-google-api-console-project-and-client-id"></a>建立 Google API 主控台專案和用戶端識別碼

* 流覽至 [將[Google 登入整合到您的 web 應用程式](https://developers.google.com/identity/sign-in/web/devconsole-project)]，然後選取 [**設定專案**]。
* 在 [**設定您的 OAuth 用戶端**] 對話方塊中，選取 [ **Web 服務器**]。
* 在 [**授權重新導向 uri** ] 文字輸入方塊中，設定 [重新導向 uri]。 例如： `https://localhost:5001/signin-google`
* 儲存**用戶端識別碼**和**用戶端密碼**。
* 部署網站時，從**Google 主控台**註冊新的公用 url。

## <a name="store-google-clientid-and-clientsecret"></a>存放區 Google ClientID 和 ClientSecret

使用[秘密管理員](xref:security/app-secrets)來儲存機密設定`Client ID` ， `Client Secret`例如 Google 和。 基於本教學課程的目的，請將權杖`Authentication:Google:ClientId`命名`Authentication:Google:ClientSecret`為，並：

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

您可以在[Api 主控台](https://console.developers.google.com/apis/dashboard)中管理 api 認證和使用方式。

## <a name="configure-google-authentication"></a>設定 Google 驗證

將 Google 服務新增至`Startup.ConfigureServices`：

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>使用 Google 登入

* 執行應用程式，然後按一下 [**登入**]。 [使用 Google 登入] 選項隨即出現。
* 按一下 [ **google** ] 按鈕，以重新導向至 google 進行驗證。
* 輸入您的 Google 認證之後，系統會將您重新導向回到網站。

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

如需 Google 驗證所支援之設定選項的詳細資訊，請參閱API參考。<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> 這可用來要求不同使用者的相關資訊。

## <a name="change-the-default-callback-uri"></a>變更預設回呼 URI

URI 區段`/signin-google`會設定為 Google 驗證提供者的預設回呼。 您可以變更預設的回呼 URI 時設定 Google 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別。

## <a name="troubleshooting"></a>疑難排解

* 如果登入無法正常執行，而且您沒有收到任何錯誤，請切換到開發模式，讓問題更容易進行調試。
* 如果未藉由在中`services.AddIdentity` `ConfigureServices`呼叫來設定身分識別，則*嘗試驗證 ArgumentException 中的結果：必須提供*' SignInScheme ' 選項。 在本教學課程中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 選取 [套用**遷移**] 以建立資料庫，然後重新整理頁面以繼續發生錯誤。

## <a name="next-steps"></a>後續步驟

* 本文說明如何您可以使用 Google 進行驗證。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。
* 將應用程式發行至 Azure 之後，請`ClientSecret`在 Google API 主控台中重設。
* 設定`Authentication:Google:ClientId`和`Authentication:Google:ClientSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。
