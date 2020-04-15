---
title: 微軟帳戶外部登錄設置與ASP.NET核心
author: rick-anderson
description: 此示例演示了 Microsoft 帳戶使用者身份驗證整合到現有ASP.NET核心應用。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 12c86456dad86731b86487a3a4dd725f36677002
ms.sourcegitcommit: f29a12486313e38e0163a643d8a97c8cecc7e871
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81384050"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>微軟帳戶外部登錄設置與ASP.NET核心

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

此示例演示如何允許使用者使用[上一頁上](xref:security/authentication/social/index)創建的 ASP.NET Core 3.0 專案使用他們的工作、學校或個人 Microsoft 帳戶登錄。

## <a name="create-the-app-in-microsoft-developer-portal"></a>在 Microsoft 開發人員門戶中建立應用

* 將[Microsoft.AspNetCore.身份驗證.Microsoft 帳戶](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/)NuGet 包添加到專案中。
* 導覽[到 Azure 門戶 - 應用程式註冊](https://go.microsoft.com/fwlink/?linkid=2083908)頁面,然後建立或登入 Microsoft 帳戶:

如果您沒有 Microsoft 帳戶,請選擇「**創建一個**」。。 登錄後,您將重定向到**應用註冊**頁面:

* 選擇**新註冊**
* 輸入**名稱**。
* 為**受支援的帳戶類型**選擇一個選項。  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts. It took 24 hours after setting this up for the keys to work -->
* 在**重定向 URI**下`/signin-microsoft`,輸入追加的開發 URL。 例如： `https://localhost:5001/signin-microsoft` 。 本示例中稍後配置的 Microsoft 身份驗證方案將自動處理路`/signin-microsoft`由中 實現 OAuth 流的請求。
* 選擇 **"註冊"**

### <a name="create-client-secret"></a>建立用戶端機密

* 在左側窗格中,選擇 **「證書&機密**」。
* 在 **"用戶端機密**"下,選擇 **"新用戶端機密**"

  * 添加用戶端機密的說明。
  * 選擇「**新增**」 按鈕。

* 在**用戶端機密**下,複製用戶端機密的值。

URI`/signin-microsoft`段設定為 Microsoft 身份驗證提供程式的預設回調。 您可以透過 Microsoft[帳戶選項](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別的繼承[遠端身份驗證選項.callbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性設定 Microsoft 身份驗證中間件時更改預設回檔 URI。

## <a name="store-the-microsoft-client-id-and-secret"></a>儲存 Microsoft 客戶端 ID 和機器

使用[密鑰管理員](xref:security/app-secrets)儲存敏感設定,如Microsoft用戶端ID和機密值。 對於此示例,請使用以下步驟:

1. 根據[啟用密鑰存儲](xref:security/app-secrets#enable-secret-storage)的指令初始化了機密存儲的專案。
1. 使用金鑰`Authentication:Microsoft:ClientId`與將敏感設定儲存在本地機密儲存中`Authentication:Microsoft:ClientSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Microsoft:ClientId" "<client-id>"
    dotnet user-secrets set "Authentication:Microsoft:ClientSecret" "<client-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>設定 Microsoft 帳戶身份驗證

將 Microsoft 帳號服務新增`Startup.ConfigureServices`到 :

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

有關 Microsoft 帳戶身份驗證支援的配置選項的詳細資訊,請參閱[Microsoft 帳戶選項](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions)API 參考。 這可用於請求有關使用者的不同資訊。

## <a name="sign-in-with-microsoft-account"></a>使用 Microsoft 帳戶登入

運行應用並按下「**登錄**」。 將顯示一個與 Microsoft 登錄的選項。 按下 Microsoft 時,您將重定向到 Microsoft 進行身份驗證。 使用 Microsoft 帳號登入後,系統將提示您讓應用程式存取您的資訊:

點按 **"是**",您將被重定向回網站,您可以在該網站設置電子郵件。

您現在使用 Microsoft 認證登入:

[!INCLUDE[](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* 如果 Microsoft 帳戶提供程式將您重定向到登入錯誤頁,請記下錯誤標題和說明查詢字串參數,直接遵循`#`Uri 中的 (hashtag)。

  儘管錯誤消息似乎表明 Microsoft 身份驗證有問題,但最常見的原因是應用程式 Uri 與為**Web**平臺指定的重定向**URI**不匹配。
* 如果未透過除錯`services.AddIdentity`設定識別`ConfigureServices`,則試著認證會導致*參數異常:必須提供「SignInScheme」選項*。 此示例中使用的專案範本可確保完成此操作。
* 如果尚未透過應用程式初始移至建立網站資料庫,則在處理請求錯誤時,將取得*資料庫操作失敗*。 點按 **「應用遷移**」以建立資料庫並刷新以繼續結束錯誤。

## <a name="next-steps"></a>後續步驟

* 本文展示了如何使用 Microsoft 進行身份驗證。 您可以採用類似的方法對[上一頁](xref:security/authentication/social/index)中列出的其他提供程式進行身份驗證。

* 將網站發佈到 Azure Web 應用後,在 Microsoft 開發人員門戶中創建新的用戶端機密。

* 在`Authentication:Microsoft:ClientId`Azure`Authentication:Microsoft:ClientSecret`門戶中將 和 設置為應用程式設置。 配置系統設置為從環境變數讀取密鑰。
