---
title: "啟用驗證器應用程式中 ASP.NET Core 的 QR 代碼產生"
author: rick-anderson
description: "啟用驗證器應用程式中 ASP.NET Core 的 QR 代碼產生"
keywords: "ASP.NET Core、 MVC、 QR 代碼產生驗證器、 2FA"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a3029e68164dd91d1bc43704c5e96bd591bcae05
ms.sourcegitcommit: a73fb5161fbdbee1d2e8474205e9840a134ee2ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="6f066-104">啟用驗證器應用程式中 ASP.NET Core 的 QR 代碼產生</span><span class="sxs-lookup"><span data-stu-id="6f066-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="6f066-105">注意： 本主題適用於 ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f066-105">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="6f066-106">ASP.NET Core 隨附個別驗證的驗證器應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="6f066-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="6f066-107">兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法，如 2FA 產業。</span><span class="sxs-lookup"><span data-stu-id="6f066-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="6f066-108">2FA 使用 TOTP 最好 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="6f066-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="6f066-109">驗證器應用程式提供的使用者必須確認使用者名稱和密碼之後輸入 6 to 8 位數代碼。</span><span class="sxs-lookup"><span data-stu-id="6f066-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="6f066-110">通常驗證器應用程式會安裝在智慧型手機上。</span><span class="sxs-lookup"><span data-stu-id="6f066-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="6f066-111">ASP.NET Core web 應用程式範本支援的驗證器，但不是提供支援 QRCode 產生。</span><span class="sxs-lookup"><span data-stu-id="6f066-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="6f066-112">QRCode 產生器可簡化 2FA 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="6f066-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="6f066-113">本文件將引導您完成新增[QR 代碼](https://wikipedia.org/wiki/QR_code)產生 2FA 組態頁面。</span><span class="sxs-lookup"><span data-stu-id="6f066-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="6f066-114">將 QR 代碼加入至 [2FA 組態] 頁面</span><span class="sxs-lookup"><span data-stu-id="6f066-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="6f066-115">這些指示使用*qrcode.js*從 https://davidshimjs.github.io/qrcodejs/ 儲存機制。</span><span class="sxs-lookup"><span data-stu-id="6f066-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="6f066-116">下載[qrcode.js javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)至`wwwroot\lib`專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="6f066-116">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="6f066-117">在*Pages\Account\Manage\EnableAuthenticator.cshtml* （Razor 頁面） 或*Views\Manage\EnableAuthenticator.cshtml* (MVC)、 找出`Scripts`檔案結尾 」 一節：</span><span class="sxs-lookup"><span data-stu-id="6f066-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="6f066-118">更新`Scripts`區段，即可將參考加入`qrcodejs`您加入程式庫，並呼叫，產生的 QR 代碼。</span><span class="sxs-lookup"><span data-stu-id="6f066-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="6f066-119">它看起來應該如下：</span><span class="sxs-lookup"><span data-stu-id="6f066-119">It should look as follows:</span></span>

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

* <span data-ttu-id="6f066-120">刪除的段落，連結至這些指示。</span><span class="sxs-lookup"><span data-stu-id="6f066-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="6f066-121">執行您的應用程式並確定您可以用來掃描該 QR 代碼，並驗證驗證器證明程式碼。</span><span class="sxs-lookup"><span data-stu-id="6f066-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="6f066-122">變更站台名稱在 QR 代碼</span><span class="sxs-lookup"><span data-stu-id="6f066-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="6f066-123">將 QR 代碼中的站台名稱是取自您一開始建立專案時，選擇專案名稱。</span><span class="sxs-lookup"><span data-stu-id="6f066-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="6f066-124">您可以藉由尋找來加以變更`GenerateQrCodeUri(string email, string unformattedKey)`方法中的*Pages\Account\Manage\EnableAuthenticator.cshtml.cs* （Razor 頁面） 檔案或*Controllers\ManageController.cs* (MVC) 檔案。</span><span class="sxs-lookup"><span data-stu-id="6f066-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="6f066-125">從範本的預設程式碼看起來如下：</span><span class="sxs-lookup"><span data-stu-id="6f066-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="6f066-126">呼叫中的第二個參數`string.Format`是您的站台名稱，從您的方案名稱。</span><span class="sxs-lookup"><span data-stu-id="6f066-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="6f066-127">它可變更為任何值，但它必須一律是 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="6f066-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="6f066-128">使用不同的 QR 代碼程式庫</span><span class="sxs-lookup"><span data-stu-id="6f066-128">Using a different QR Code library</span></span>

<span data-ttu-id="6f066-129">您可以使用您慣用的程式庫來取代 QR 代碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="6f066-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="6f066-130">包含 HTML`qrCode`您可以在其中放置 QR 代碼透過任何機制的項目會提供您的程式庫。</span><span class="sxs-lookup"><span data-stu-id="6f066-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="6f066-131">將 QR 代碼的正確格式的 URL 位於:</span><span class="sxs-lookup"><span data-stu-id="6f066-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="6f066-132">`AuthenticatorUri`模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="6f066-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="6f066-133">`data-url`中的屬性`qrCodeData`項目。</span><span class="sxs-lookup"><span data-stu-id="6f066-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="6f066-134">使用`@Html.Raw`存取檢視中的模型屬性 （否則會雙重編碼 url 中的連字號和 QR 代碼的標籤參數將被忽略）。</span><span class="sxs-lookup"><span data-stu-id="6f066-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="6f066-135">TOTP 用戶端和伺服器時間誤差</span><span class="sxs-lookup"><span data-stu-id="6f066-135">TOTP client and server time skew</span></span>

<span data-ttu-id="6f066-136">TOTP 驗證取決於伺服器和驗證器的裝置，具有正確的時間。</span><span class="sxs-lookup"><span data-stu-id="6f066-136">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="6f066-137">語彙基元只有最後一個 30 秒。</span><span class="sxs-lookup"><span data-stu-id="6f066-137">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="6f066-138">TOTP 2FA 登入而失敗，請檢查伺服器時間是否正確，且最好是已同步處理至正確的 NTP 服務。</span><span class="sxs-lookup"><span data-stu-id="6f066-138">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
