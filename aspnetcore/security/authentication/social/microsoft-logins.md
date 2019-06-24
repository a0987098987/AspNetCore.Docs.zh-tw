---
title: 使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定
author: rick-anderson
description: 這個範例會示範整合到現有的 ASP.NET Core 應用程式的 Microsoft 帳戶使用者驗證。
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 16ec2d5f2bccc59958b884869ef42af9cfa13df0
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316594"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

這個範例會示範如何讓使用者使用他們使用 ASP.NET Core 2.2 專案上建立的 Microsoft 帳戶登入[上一頁](xref:security/authentication/social/index)。

## <a name="create-the-app-in-microsoft-developer-portal"></a>在 Microsoft 開發人員入口網站中建立應用程式

* 瀏覽至[Azure 入口網站-應用程式註冊](https://go.microsoft.com/fwlink/?linkid=2083908)頁面上，建立或登入 Microsoft 帳戶：

如果您沒有 Microsoft 帳戶，請選取**建立一個**。 登入之後將您重新導向**應用程式註冊**頁面：

* 選取**新增註冊**
* 請輸入**名稱**。
* 選取的選項**支援的帳戶類型**。  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* 底下**重新導向 URI**，輸入您的開發 URL 與`/signin-microsoft`附加。 例如， `https://localhost:44389/signin-microsoft` 。 稍後在此範例中所設定的 Microsoft 驗證配置會自動處理在要求`/signin-microsoft`實作 OAuth 流程的路由。
* 選取**註冊**

### <a name="create-client-secret"></a>建立用戶端祕密

* 在左窗格中，選取**憑證與祕密**。
* 底下**用戶端祕密**，選取**新的用戶端祕密**

  * 加入用戶端祕密的描述。
  * 選取 [**新增**] 按鈕。

* 底下**用戶端祕密**，複製用戶端祕密的值。

> [!NOTE]
> URI 區段`/signin-microsoft`設為預設回呼的 Microsoft 驗證提供者。 您可以變更預設的回呼 URI 時設定 Microsoft 驗證中的介軟體，透過繼承[RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別。

## <a name="store-the-microsoft-client-id-and-client-secret"></a>儲存 Microsoft 用戶端識別碼和用戶端祕密

執行下列命令，以安全地儲存`ClientId`並`ClientSecret`使用[Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

連結機密設定，例如 Microsoft`ClientId`並`ClientSecret`至您的應用程式的組態使用[Secret Manager](xref:security/app-secrets)。 基於此範例的目的，名稱語彙基元`Authentication:Microsoft:ClientId`和`Authentication:Microsoft:ClientSecret`。

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>設定 Microsoft 帳戶驗證

新增 Microsoft 帳戶服務`Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

請參閱[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考，如需有關 Microsoft 帳戶驗證所支援的組態選項。 這可用來要求不同使用者的相關資訊。

## <a name="sign-in-with-microsoft-account"></a>使用 Microsoft 帳戶登入

執行，按一下 **登入**。 若要使用 Microsoft 登入的選項會出現。 當您按一下 Microsoft 時，您會重新導向到 Microsoft 進行驗證。 之後您 Microsoft 帳戶登入 （如果您尚未登入） 將會提示您讓應用程式存取您的資訊：

點選**是**您就會重新導向回 web 站台，您可以在其中設定您的電子郵件。

您現在用來登入您的 Microsoft 認證：

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* 如果 Microsoft 帳戶提供者會將您導向登入錯誤頁面，請注意錯誤標題和描述查詢字串參數直接跟`#`（主題標籤） 中的 Uri。

  雖然指出使用 Microsoft 驗證問題似乎出現錯誤訊息，最常見的原因是您的應用程式 Uri 不符合任一**重新導向 Uri**指定**Web**平台.
* 如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，來驗證嘗試會導致*ArgumentException:必須提供 'SignInScheme' 選項*。 此範例中使用的專案範本可確保，這麼做。
* 如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時資料庫作業失敗*時發生錯誤。 點選**套用移轉**建立資料庫，並重新整理 以忽略錯誤繼續執行。

## <a name="next-steps"></a>後續步驟

* 本文章說明您可以向 Microsoft。 您可以依照類似的方法，來驗證與列在其他提供者[上一頁](xref:security/authentication/social/index)。

* 一旦您發行至 Azure web 應用程式的網站，建立新的用戶端祕密中 Microsoft 開發人員入口網站。

* 設定`Authentication:Microsoft:ClientId`和`Authentication:Microsoft:ClientSecret`當做 Azure 入口網站中的應用程式設定。 設定系統設定來讀取環境變數中的索引鍵。