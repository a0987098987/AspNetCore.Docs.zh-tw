---
title: 啟用 ASP.NET Core TOTP 驗證器應用程式的 QR 代碼產生
author: rick-anderson
description: 了解如何啟用 QR TOTP 驗證器應用程式可搭配 ASP.NET Core 雙因素驗證的程式碼產生。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/24/2017
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: b0d8f104119340b97bd65f1826bb921ca875acf8
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089967"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>啟用 ASP.NET Core TOTP 驗證器應用程式的 QR 代碼產生

ASP.NET Core 隨附個別驗證的驗證器應用程式的支援。 兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法，如 2FA 產業。 2FA 使用 TOTP 最好 SMS 2FA。 驗證器應用程式提供的使用者必須確認使用者名稱和密碼之後輸入 6 to 8 位數代碼。 通常驗證器應用程式會安裝在智慧型手機上。

ASP.NET Core web 應用程式範本支援的驗證器，但不提供支援 QRCode 產生。 QRCode 產生器可簡化 2FA 的安裝程式。 本文件將引導您完成新增[QR 代碼](https://wikipedia.org/wiki/QR_code)產生 2FA 組態頁面。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>將 QR 代碼加入至 [2FA 組態] 頁面

這些指示使用*qrcode.js*從https://davidshimjs.github.io/qrcodejs/儲存機制。

* 下載[qrcode.js javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)至`wwwroot\lib`專案資料夾中的。

* 在*Pages\Account\Manage\EnableAuthenticator.cshtml* （Razor 頁面） 或*Views\Manage\EnableAuthenticator.cshtml* (MVC)、 找出`Scripts`檔案結尾 」 一節：

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新`Scripts`區段，即可將參考加入`qrcodejs`您加入程式庫，並呼叫，產生的 QR 代碼。 它看起來應該如下：

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

* 刪除的段落，連結至這些指示。

執行您的應用程式並確定您可以用來掃描該 QR 代碼，並驗證驗證器證明程式碼。

## <a name="change-the-site-name-in-the-qr-code"></a>變更站台名稱在 QR 代碼

將 QR 代碼中的站台名稱是取自您一開始建立專案時，選擇專案名稱。 您可以藉由尋找來加以變更`GenerateQrCodeUri(string email, string unformattedKey)`方法中的*Pages\Account\Manage\EnableAuthenticator.cshtml.cs* （Razor 頁面） 檔案或*Controllers\ManageController.cs* (MVC) 檔案。

從範本的預設程式碼看起來如下：

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

呼叫中的第二個參數`string.Format`是您的站台名稱，從您的方案名稱。 它可變更為任何值，但它必須一律是 URL 編碼。

## <a name="using-a-different-qr-code-library"></a>使用不同的 QR 代碼程式庫

您可以使用您慣用的程式庫來取代 QR 代碼程式庫。 包含 HTML`qrCode`您可以在其中放置 QR 代碼透過任何機制的項目會提供您的程式庫。

將 QR 代碼的正確格式的 URL 位於:

* `AuthenticatorUri` 模型的屬性。
* `data-url` 中的屬性`qrCodeData`項目。

## <a name="totp-client-and-server-time-skew"></a>TOTP 用戶端和伺服器時間誤差

TOTP （以時間為基礎的單次密碼） 驗證取決於伺服器和驗證器的裝置，具有正確的時間。 語彙基元只有最後一個 30 秒。 TOTP 2FA 登入而失敗，請檢查伺服器時間是否正確，且最好是已同步處理至正確的 NTP 服務。
