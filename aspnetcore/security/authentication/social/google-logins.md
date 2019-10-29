---
title: ASP.NET Core 中的 Google external 登入設定
author: rick-anderson
description: 本教學課程示範 Google 帳戶使用者驗證與現有 ASP.NET Core 應用程式的整合。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/28/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 663029ecab99efd4f63f8deca026957c19c64710
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034317"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core 中的 Google external 登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程說明如何讓使用者使用在[前一頁](xref:security/authentication/social/index)建立的 ASP.NET Core 3.0 專案，以其 Google 帳戶登入。

## <a name="create-a-google-api-console-project-and-client-id"></a>建立 Google API 主控台專案和用戶端識別碼

* 安裝[AspNetCore。 Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google)。
* 流覽至 [將[Google 登入整合到您的 web 應用程式](https://developers.google.com/identity/sign-in/web/devconsole-project)]，然後選取 [**設定專案**]。
* 在 [**設定您的 OAuth 用戶端**] 對話方塊中，選取 [ **Web 服務器**]。
* 在 [**授權重新導向 uri** ] 文字輸入方塊中，設定 [重新導向 uri]。 例如：`https://localhost:44312/signin-google`
* 儲存**用戶端識別碼**和**用戶端密碼**。
* 部署網站時，從**Google 主控台**註冊新的公用 url。

## <a name="store-google-clientid-and-clientsecret"></a>儲存 Google ClientID 和 ClientSecret

使用[秘密管理員](xref:security/app-secrets)來儲存機密設定，例如 Google `Client ID` 和 `Client Secret`。 基於本教學課程的目的，請將權杖命名為 `Authentication:Google:ClientId` 並 `Authentication:Google:ClientSecret`：

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

您可以在[Api 主控台](https://console.developers.google.com/apis/dashboard)中管理 api 認證和使用方式。

## <a name="configure-google-authentication"></a>設定 Google 驗證

將 Google 服務新增至 `Startup.ConfigureServices`：

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>使用 Google 登入

* 執行應用程式，然後按一下 [**登入**]。 [使用 Google 登入] 選項隨即出現。
* 按一下 [ **google** ] 按鈕，以重新導向至 google 進行驗證。
* 輸入您的 Google 認證之後，系統會將您重新導向回到網站。

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

如需 Google 驗證所支援之設定選項的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API 參考。 這可以用來要求使用者的不同資訊。

## <a name="change-the-default-callback-uri"></a>變更預設回呼 URI

URI 區段 `/signin-google` 會設定為 Google authentication 提供者的預設回呼。 設定 Google 驗證中介軟體時，您可以透過[GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設的回呼 URI。

## <a name="troubleshooting"></a>疑難排解

* 如果登入無法正常執行，而且您沒有收到任何錯誤，請切換到開發模式，讓問題更容易進行調試。
* 如果未透過呼叫 `ConfigureServices`中的 `services.AddIdentity` 來設定身分識別，則嘗試在 ArgumentException 中驗證結果 *：必須提供 ' SignInScheme ' 選項*。 本教學課程中使用的專案範本可確保這項作業已完成。
* 如果尚未藉由套用初始遷移來建立網站資料庫，則在*處理要求錯誤時*，您會取得資料庫作業失敗。 選取 [套用**遷移**] 以建立資料庫，然後重新整理頁面以繼續發生錯誤。

## <a name="next-steps"></a>後續步驟

* 本文說明了您可以如何使用 Google 進行驗證。 您可以遵循類似的方法，向[先前頁面](xref:security/authentication/social/index)上所列的其他提供者進行驗證。
* 將應用程式發行至 Azure 之後，請在 Google API 主控台中重設 `ClientSecret`。
* 將 [`Authentication:Google:ClientId`] 和 [`Authentication:Google:ClientSecret`] 設定為 Azure 入口網站中的 [應用程式設定]。 設定系統已設定為從環境變數讀取金鑰。
