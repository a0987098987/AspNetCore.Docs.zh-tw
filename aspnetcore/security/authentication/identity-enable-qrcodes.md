---
title: 在 ASP.NET Core 中為 TOTP 驗證器應用程式啟用 QR 代碼產生
author: rick-anderson
description: 探索如何為使用 ASP.NET Core 雙因素驗證的 TOTP 驗證器應用程式啟用 QR 代碼產生。
ms.author: riande
ms.date: 08/14/2018
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 42ddddeaa329ac5ff5b2b40cbf9ebffa68f6d4cf
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774427"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="83ec6-103">在 ASP.NET Core 中為 TOTP 驗證器應用程式啟用 QR 代碼產生</span><span class="sxs-lookup"><span data-stu-id="83ec6-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="83ec6-104">QR 代碼需要 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="83ec6-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83ec6-105">ASP.NET Core 提供驗證器應用程式進行個別驗證的支援。</span><span class="sxs-lookup"><span data-stu-id="83ec6-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="83ec6-106">使用以時間為基礎的一次性密碼演算法（TOTP）的雙因素驗證（2FA）驗證器應用程式是2FA 的業界建議方法。</span><span class="sxs-lookup"><span data-stu-id="83ec6-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="83ec6-107">2FA 使用 TOTP 是 SMS 2FA 的慣用選項。</span><span class="sxs-lookup"><span data-stu-id="83ec6-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="83ec6-108">驗證器應用程式提供6到8位數的代碼，使用者必須在確認其使用者名稱和密碼之後輸入。</span><span class="sxs-lookup"><span data-stu-id="83ec6-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="83ec6-109">驗證器應用程式通常會安裝在智慧型手機上。</span><span class="sxs-lookup"><span data-stu-id="83ec6-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="83ec6-110">ASP.NET Core web 應用程式範本支援驗證器，但不提供 QRCode 產生的支援。</span><span class="sxs-lookup"><span data-stu-id="83ec6-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="83ec6-111">QRCode 產生器會簡化2FA 的設定。</span><span class="sxs-lookup"><span data-stu-id="83ec6-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="83ec6-112">本檔將引導您在2FA 設定頁面中新增[QR 代碼](https://wikipedia.org/wiki/QR_code)產生。</span><span class="sxs-lookup"><span data-stu-id="83ec6-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

<span data-ttu-id="83ec6-113">使用外部驗證提供者（例如[Google](xref:security/authentication/google-logins)或[Facebook](xref:security/authentication/facebook-logins)）時，不會發生雙因素驗證。</span><span class="sxs-lookup"><span data-stu-id="83ec6-113">Two factor authentication does not happen using an external authentication provider, such as [Google](xref:security/authentication/google-logins) or [Facebook](xref:security/authentication/facebook-logins).</span></span> <span data-ttu-id="83ec6-114">外部登入會受到外部登入提供者所提供的任何機制所保護。</span><span class="sxs-lookup"><span data-stu-id="83ec6-114">External logins are protected by whatever mechanism the external login provider provides.</span></span> <span data-ttu-id="83ec6-115">例如，請考慮[Microsoft](xref:security/authentication/microsoft-logins)驗證提供者需要硬體金鑰或其他2FA 方法。</span><span class="sxs-lookup"><span data-stu-id="83ec6-115">Consider, for example, the [Microsoft](xref:security/authentication/microsoft-logins) authentication provider requires a hardware key or another 2FA approach.</span></span> <span data-ttu-id="83ec6-116">如果預設範本強制執行 "local" 2FA，則使用者必須滿足兩個2FA 方法，這不是常用的案例。</span><span class="sxs-lookup"><span data-stu-id="83ec6-116">If the default templates enforced "local" 2FA then users would be required to satisfy two 2FA approaches, which is not a commonly used scenario.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="83ec6-117">將 QR 代碼新增至2FA 設定頁面</span><span class="sxs-lookup"><span data-stu-id="83ec6-117">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="83ec6-118">這些指示會*qrcode.js*使用存放庫中的https://davidshimjs.github.io/qrcodejs/ qrcode。</span><span class="sxs-lookup"><span data-stu-id="83ec6-118">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="83ec6-119">將[qrcode javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)下載到您專案`wwwroot\lib`中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="83ec6-119">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="83ec6-120">遵循[ Identity Scaffold](xref:security/authentication/scaffold-identity)中的指示來產生 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="83ec6-120">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="83ec6-121">在 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*中，于`Scripts`檔案結尾處找出區段：</span><span class="sxs-lookup"><span data-stu-id="83ec6-121">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="83ec6-122">在*pages/Account/manage/EnableAuthenticator. cshtml* （Razor pages）或*Views/manage/EnableAuthenticator* （MVC）中，找出檔`Scripts`尾的區段：</span><span class="sxs-lookup"><span data-stu-id="83ec6-122">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="83ec6-123">更新`Scripts`區段以加入您新增之連結`qrcodejs`庫的參考，以及呼叫以產生 QR 代碼。</span><span class="sxs-lookup"><span data-stu-id="83ec6-123">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="83ec6-124">看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="83ec6-124">It should look as follows:</span></span>

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

* <span data-ttu-id="83ec6-125">刪除連結至這些指示的段落。</span><span class="sxs-lookup"><span data-stu-id="83ec6-125">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="83ec6-126">執行您的應用程式，並確定您可以掃描 QR 代碼，並驗證驗證器所證明的程式碼。</span><span class="sxs-lookup"><span data-stu-id="83ec6-126">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="83ec6-127">變更 QR 代碼中的網站名稱</span><span class="sxs-lookup"><span data-stu-id="83ec6-127">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="83ec6-128">QR 代碼中的網站名稱是取自您最初建立專案時所選擇的專案名稱。</span><span class="sxs-lookup"><span data-stu-id="83ec6-128">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="83ec6-129">您可以藉由在 [ `GenerateQrCodeUri(string email, string unformattedKey)` */Areas/Identity]/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*中尋找方法來變更它。</span><span class="sxs-lookup"><span data-stu-id="83ec6-129">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="83ec6-130">QR 代碼中的網站名稱是取自您最初建立專案時所選擇的專案名稱。</span><span class="sxs-lookup"><span data-stu-id="83ec6-130">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="83ec6-131">您可以藉由尋找`GenerateQrCodeUri(string email, string unformattedKey)` *頁面/帳戶/管理/EnableAuthenticator* Razor檔中的方法，或 controller */ManageController .cs* （MVC）檔案來變更它。</span><span class="sxs-lookup"><span data-stu-id="83ec6-131">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="83ec6-132">範本的預設程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="83ec6-132">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="83ec6-133">呼叫中的第二個參數`string.Format`是您的網站名稱，取自您的解決方案名稱。</span><span class="sxs-lookup"><span data-stu-id="83ec6-133">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="83ec6-134">它可以變更為任何值，但一定要以 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="83ec6-134">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="83ec6-135">使用不同的 QR 代碼程式庫</span><span class="sxs-lookup"><span data-stu-id="83ec6-135">Using a different QR Code library</span></span>

<span data-ttu-id="83ec6-136">您可以使用慣用的程式庫來取代 QR 代碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="83ec6-136">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="83ec6-137">HTML 包含一個`qrCode`元素，您可以透過程式庫提供的任何機制，將 QR 代碼放在其中。</span><span class="sxs-lookup"><span data-stu-id="83ec6-137">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="83ec6-138">您可以在中找到 QR 代碼的正確格式 URL：</span><span class="sxs-lookup"><span data-stu-id="83ec6-138">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="83ec6-139">`AuthenticatorUri`模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="83ec6-139">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="83ec6-140">`data-url`元素中的`qrCodeData`屬性。</span><span class="sxs-lookup"><span data-stu-id="83ec6-140">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="83ec6-141">TOTP 用戶端與伺服器時間偏差</span><span class="sxs-lookup"><span data-stu-id="83ec6-141">TOTP client and server time skew</span></span>

<span data-ttu-id="83ec6-142">TOTP （以時間為基礎的單次密碼）驗證取決於伺服器和驗證器裝置是否有精確的時間。</span><span class="sxs-lookup"><span data-stu-id="83ec6-142">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="83ec6-143">權杖只會在30秒後結束。</span><span class="sxs-lookup"><span data-stu-id="83ec6-143">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="83ec6-144">如果 TOTP 2FA 登入失敗，請檢查伺服器時間是否正確，並最好同步處理到精確的 NTP 服務。</span><span class="sxs-lookup"><span data-stu-id="83ec6-144">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
