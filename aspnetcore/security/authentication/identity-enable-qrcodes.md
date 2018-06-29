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
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="51d6d-103">啟用 ASP.NET Core TOTP 驗證器應用程式的 QR 代碼產生</span><span class="sxs-lookup"><span data-stu-id="51d6d-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="51d6d-104">ASP.NET Core 隨附個別驗證的驗證器應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="51d6d-104">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="51d6d-105">兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法，如 2FA 產業。</span><span class="sxs-lookup"><span data-stu-id="51d6d-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="51d6d-106">2FA 使用 TOTP 最好 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="51d6d-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="51d6d-107">驗證器應用程式提供的使用者必須確認使用者名稱和密碼之後輸入 6 to 8 位數代碼。</span><span class="sxs-lookup"><span data-stu-id="51d6d-107">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="51d6d-108">通常驗證器應用程式會安裝在智慧型手機上。</span><span class="sxs-lookup"><span data-stu-id="51d6d-108">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="51d6d-109">ASP.NET Core web 應用程式範本支援的驗證器，但不提供支援 QRCode 產生。</span><span class="sxs-lookup"><span data-stu-id="51d6d-109">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="51d6d-110">QRCode 產生器可簡化 2FA 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="51d6d-110">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="51d6d-111">本文件將引導您完成新增[QR 代碼](https://wikipedia.org/wiki/QR_code)產生 2FA 組態頁面。</span><span class="sxs-lookup"><span data-stu-id="51d6d-111">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="51d6d-112">將 QR 代碼加入至 [2FA 組態] 頁面</span><span class="sxs-lookup"><span data-stu-id="51d6d-112">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="51d6d-113">這些指示使用*qrcode.js*從https://davidshimjs.github.io/qrcodejs/儲存機制。</span><span class="sxs-lookup"><span data-stu-id="51d6d-113">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="51d6d-114">下載[qrcode.js javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)至`wwwroot\lib`專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="51d6d-114">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="51d6d-115">在*Pages\Account\Manage\EnableAuthenticator.cshtml* （Razor 頁面） 或*Views\Manage\EnableAuthenticator.cshtml* (MVC)、 找出`Scripts`檔案結尾 」 一節：</span><span class="sxs-lookup"><span data-stu-id="51d6d-115">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="51d6d-116">更新`Scripts`區段，即可將參考加入`qrcodejs`您加入程式庫，並呼叫，產生的 QR 代碼。</span><span class="sxs-lookup"><span data-stu-id="51d6d-116">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="51d6d-117">它看起來應該如下：</span><span class="sxs-lookup"><span data-stu-id="51d6d-117">It should look as follows:</span></span>

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

* <span data-ttu-id="51d6d-118">刪除的段落，連結至這些指示。</span><span class="sxs-lookup"><span data-stu-id="51d6d-118">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="51d6d-119">執行您的應用程式並確定您可以用來掃描該 QR 代碼，並驗證驗證器證明程式碼。</span><span class="sxs-lookup"><span data-stu-id="51d6d-119">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="51d6d-120">變更站台名稱在 QR 代碼</span><span class="sxs-lookup"><span data-stu-id="51d6d-120">Change the site name in the QR Code</span></span>

<span data-ttu-id="51d6d-121">將 QR 代碼中的站台名稱是取自您一開始建立專案時，選擇專案名稱。</span><span class="sxs-lookup"><span data-stu-id="51d6d-121">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="51d6d-122">您可以藉由尋找來加以變更`GenerateQrCodeUri(string email, string unformattedKey)`方法中的*Pages\Account\Manage\EnableAuthenticator.cshtml.cs* （Razor 頁面） 檔案或*Controllers\ManageController.cs* (MVC) 檔案。</span><span class="sxs-lookup"><span data-stu-id="51d6d-122">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span>

<span data-ttu-id="51d6d-123">從範本的預設程式碼看起來如下：</span><span class="sxs-lookup"><span data-stu-id="51d6d-123">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="51d6d-124">呼叫中的第二個參數`string.Format`是您的站台名稱，從您的方案名稱。</span><span class="sxs-lookup"><span data-stu-id="51d6d-124">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="51d6d-125">它可變更為任何值，但它必須一律是 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="51d6d-125">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="51d6d-126">使用不同的 QR 代碼程式庫</span><span class="sxs-lookup"><span data-stu-id="51d6d-126">Using a different QR Code library</span></span>

<span data-ttu-id="51d6d-127">您可以使用您慣用的程式庫來取代 QR 代碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="51d6d-127">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="51d6d-128">包含 HTML`qrCode`您可以在其中放置 QR 代碼透過任何機制的項目會提供您的程式庫。</span><span class="sxs-lookup"><span data-stu-id="51d6d-128">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="51d6d-129">將 QR 代碼的正確格式的 URL 位於:</span><span class="sxs-lookup"><span data-stu-id="51d6d-129">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="51d6d-130">`AuthenticatorUri` 模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="51d6d-130">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="51d6d-131">`data-url` 中的屬性`qrCodeData`項目。</span><span class="sxs-lookup"><span data-stu-id="51d6d-131">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="51d6d-132">TOTP 用戶端和伺服器時間誤差</span><span class="sxs-lookup"><span data-stu-id="51d6d-132">TOTP client and server time skew</span></span>

<span data-ttu-id="51d6d-133">TOTP （以時間為基礎的單次密碼） 驗證取決於伺服器和驗證器的裝置，具有正確的時間。</span><span class="sxs-lookup"><span data-stu-id="51d6d-133">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="51d6d-134">語彙基元只有最後一個 30 秒。</span><span class="sxs-lookup"><span data-stu-id="51d6d-134">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="51d6d-135">TOTP 2FA 登入而失敗，請檢查伺服器時間是否正確，且最好是已同步處理至正確的 NTP 服務。</span><span class="sxs-lookup"><span data-stu-id="51d6d-135">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
