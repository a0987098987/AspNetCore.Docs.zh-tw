---
title: 在 ASP.NET Core 中為 TOTP 驗證器應用程式啟用 QR 代碼產生
author: rick-anderson
description: 探索如何為使用 ASP.NET Core 雙因素驗證的 TOTP 驗證器應用程式啟用 QR 代碼產生。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665309"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>在 ASP.NET Core 中為 TOTP 驗證器應用程式啟用 QR 代碼產生

::: moniker range="<= aspnetcore-2.0"

QR 代碼需要 ASP.NET Core 2.0 或更新版本。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core 提供驗證器應用程式進行個別驗證的支援。 使用以時間為基礎的一次性密碼演算法（TOTP）的雙因素驗證（2FA）驗證器應用程式是2FA 的業界建議方法。 2FA 使用 TOTP 是 SMS 2FA 的慣用選項。 驗證器應用程式提供6到8位數的代碼，使用者必須在確認其使用者名稱和密碼之後輸入。 驗證器應用程式通常會安裝在智慧型手機上。

ASP.NET Core web 應用程式範本支援驗證器，但不提供 QRCode 產生的支援。 QRCode 產生器會簡化2FA 的設定。 本檔將引導您在2FA 設定頁面中新增[QR 代碼](https://wikipedia.org/wiki/QR_code)產生。

使用外部驗證提供者（例如[Google](xref:security/authentication/google-logins)或[Facebook](xref:security/authentication/facebook-logins)）時，不會發生雙因素驗證。 外部登入會受到外部登入提供者所提供的任何機制所保護。 例如，請考慮[Microsoft](xref:security/authentication/microsoft-logins)驗證提供者需要硬體金鑰或其他2FA 方法。 如果預設範本強制執行 "local" 2FA，則使用者必須滿足兩個2FA 方法，這不是常用的案例。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>將 QR 代碼新增至2FA 設定頁面

這些指示會使用 https://davidshimjs.github.io/qrcodejs/ 存放庫中的*qrcode* 。

* 將[qrcode javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)下載到您專案中的 `wwwroot\lib` 資料夾。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* 遵循[Scaffold Identity](xref:security/authentication/scaffold-identity)中的指示來產生 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*。
* 在 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*中，找出檔案結尾的 `Scripts` 區段：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* 在*Pages/Account/manage/EnableAuthenticator* （Razor Pages）或*Views/manage/EnableAuthenticator* （MVC）中，找出檔案結尾的 `Scripts` 區段：

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新 `Scripts` 區段，將參考新增至您所新增的 `qrcodejs` 程式庫，以及呼叫以產生 QR 代碼。 看起來應該如下所示：

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

* 刪除連結至這些指示的段落。

執行您的應用程式，並確定您可以掃描 QR 代碼，並驗證驗證器所證明的程式碼。

## <a name="change-the-site-name-in-the-qr-code"></a>變更 QR 代碼中的網站名稱

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

QR 代碼中的網站名稱是取自您最初建立專案時所選擇的專案名稱。 您可以藉由尋找 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*中的 `GenerateQrCodeUri(string email, string unformattedKey)` 方法來加以變更。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

QR 代碼中的網站名稱是取自您最初建立專案時所選擇的專案名稱。 您可以藉由尋找*頁面/帳戶/管理/Razor Pages EnableAuthenticator*檔中的 `GenerateQrCodeUri(string email, string unformattedKey)` 方法或 Controller */ManageController .cs* （MVC）檔案來加以變更。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

範本的預設程式碼如下所示：

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

`string.Format` 呼叫中的第二個參數是您的網站名稱，取自您的解決方案名稱。 它可以變更為任何值，但一定要以 URL 編碼。

## <a name="using-a-different-qr-code-library"></a>使用不同的 QR 代碼程式庫

您可以使用慣用的程式庫來取代 QR 代碼程式庫。 HTML 包含一個 `qrCode` 專案，您可以透過程式庫提供的任何機制，將 QR 代碼放在其中。

您可以在中找到 QR 代碼的正確格式 URL：

* 模型的 `AuthenticatorUri` 屬性。
* `qrCodeData` 元素中的 `data-url` 屬性。

## <a name="totp-client-and-server-time-skew"></a>TOTP 用戶端與伺服器時間偏差

TOTP （以時間為基礎的單次密碼）驗證取決於伺服器和驗證器裝置是否有精確的時間。 權杖只會在30秒後結束。 如果 TOTP 2FA 登入失敗，請檢查伺服器時間是否正確，並最好同步處理到精確的 NTP 服務。

::: moniker-end
