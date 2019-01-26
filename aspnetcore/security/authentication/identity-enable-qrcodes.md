---
title: 啟用 ASP.NET Core 中的 TOTP 驗證器應用程式的 QR 代碼產生
author: rick-anderson
description: 了解如何啟用 QR 程式碼產生使用 ASP.NET Core 雙因素驗證的 TOTP 驗證器應用程式。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: cf99cc21a7a1bb4d01c7cc092106d23375a1a76f
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073123"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>啟用 ASP.NET Core 中的 TOTP 驗證器應用程式的 QR 代碼產生

::: moniker range="<= aspnetcore-2.0"

QR 代碼需要 ASP.NET Core 2.0 或更新版本。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core 隨附個別驗證的驗證器應用程式的支援。 兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法 2FA 的產業。 2FA 使用 TOTP 是慣用的 SMS 2FA。 驗證器應用程式提供的使用者必須確認使用者名稱和密碼之後輸入 6 to 8 位數代碼。 通常驗證器應用程式會安裝在智慧型手機。

ASP.NET Core web 應用程式範本支援驗證器，但不提供支援 QRCode 產生。 QRCode 產生器可簡化 2FA 的安裝程式。 本文件將引導您完成新增[QR 代碼](https://wikipedia.org/wiki/QR_code)2FA 的 [組態] 頁面的產生。

雙因素驗證並不會使用外部驗證提供者，例如[Google](xref:security/authentication/google-logins)或是[Facebook](xref:security/authentication/facebook-logins)。 外部登入受到任何機制所提供的外部登入提供者。 例如，請考慮[Microsoft](xref:security/authentication/microsoft-logins)驗證提供者需要硬體金鑰或另一個 2FA 方法。 如果預設範本強制執行 「 本機 」 2FA 使用者會需要滿足兩種 2FA 方法，這是不常使用的案例。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>新增至 2FA 的 [設定] 頁面的 QR 代碼

使用這些指示*qrcode.js*從 https://davidshimjs.github.io/qrcodejs/ 存放庫。

* 下載[qrcode.js javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)至`wwwroot\lib`專案中的資料夾。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* 請依照下列中的指示[Scaffold 身分識別](xref:security/authentication/scaffold-identity)產生 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*。
* 在  */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*，找出`Scripts`檔案結尾的區段：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* 在  *Pages/Account/Manage/EnableAuthenticator.cshtml* （Razor 頁面） 或*Views/Manage/EnableAuthenticator.cshtml* (MVC)、 找出`Scripts`檔案結尾的區段：

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新`Scripts`一節，以將參考加入`qrcodejs`您新增的程式庫和呼叫，以產生 QR 代碼。 它看起來應該如下：

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* 刪除連結您在這些指示的段落。

執行您的應用程式，並確保您可以用來掃描該 QR 代碼，並驗證驗證器證明自己的程式碼。

## <a name="change-the-site-name-in-the-qr-code"></a>變更站台名稱中的 QR 代碼

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

在 QR 程式碼中的站台名稱是取自您選擇一開始建立您的專案時的專案名稱。 您可以變更其尋求`GenerateQrCodeUri(string email, string unformattedKey)`方法中的 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

在 QR 程式碼中的站台名稱是取自您選擇一開始建立您的專案時的專案名稱。 您可以變更其尋求`GenerateQrCodeUri(string email, string unformattedKey)`方法中的*Pages/Account/Manage/EnableAuthenticator.cshtml.cs* （Razor 頁面） 檔案或*Controllers/ManageController.cs* (MVC) 檔案。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

從範本的預設程式碼如下所示：

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

第二個參數，在呼叫`string.Format`是您的站台名稱，取自您方案的名稱。 它可以變更為任何值，但必須一律為 URL 編碼。

## <a name="using-a-different-qr-code-library"></a>使用不同的 QR 代碼程式庫

您可以使用您慣用的程式庫，來取代 QR 程式碼程式庫。 包含 HTML`qrCode`您可以在其中放置 QR 代碼透過任何機制的項目會提供您的程式庫。

QR 代碼的正確格式的 URL 位於:

* `AuthenticatorUri` 模型的屬性。
* `data-url` 中的屬性`qrCodeData`項目。

## <a name="totp-client-and-server-time-skew"></a>用於用戶端和伺服器時間扭曲

TOTP （以時間為基礎的單次密碼） 驗證取決於伺服器和驗證器的裝置，具有正確的時間。 權杖只會持續 30 秒。 如果 TOTP 2FA 登入失敗，請檢查伺服器時間是否正確，且最好是同步處理至正確的 NTP 服務。

::: moniker-end
