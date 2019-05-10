---
title: 啟用 ASP.NET Core 中的 TOTP 驗證器應用程式的 QR 代碼產生
author: rick-anderson
description: 了解如何啟用 QR 程式碼產生使用 ASP.NET Core 雙因素驗證的 TOTP 驗證器應用程式。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896725"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="6ba0a-103">啟用 ASP.NET Core 中的 TOTP 驗證器應用程式的 QR 代碼產生</span><span class="sxs-lookup"><span data-stu-id="6ba0a-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="6ba0a-104">QR 代碼需要 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6ba0a-105">ASP.NET Core 隨附個別驗證的驗證器應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="6ba0a-106">兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法 2FA 的產業。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="6ba0a-107">2FA 使用 TOTP 是慣用的 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="6ba0a-108">驗證器應用程式提供的使用者必須確認使用者名稱和密碼之後輸入 6 to 8 位數代碼。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="6ba0a-109">通常驗證器應用程式會安裝在智慧型手機。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="6ba0a-110">ASP.NET Core web 應用程式範本支援驗證器，但不提供支援 QRCode 產生。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="6ba0a-111">QRCode 產生器可簡化 2FA 的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="6ba0a-112">本文件將引導您完成新增[QR 代碼](https://wikipedia.org/wiki/QR_code)2FA 的 [組態] 頁面的產生。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

<span data-ttu-id="6ba0a-113">雙因素驗證並不會使用外部驗證提供者，例如[Google](xref:security/authentication/google-logins)或是[Facebook](xref:security/authentication/facebook-logins)。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-113">Two factor authentication does not happen using an external authentication provider, such as [Google](xref:security/authentication/google-logins) or [Facebook](xref:security/authentication/facebook-logins).</span></span> <span data-ttu-id="6ba0a-114">外部登入受到任何機制所提供的外部登入提供者。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-114">External logins are protected by whatever mechanism the external login provider provides.</span></span> <span data-ttu-id="6ba0a-115">例如，請考慮[Microsoft](xref:security/authentication/microsoft-logins)驗證提供者需要硬體金鑰或另一個 2FA 方法。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-115">Consider, for example, the [Microsoft](xref:security/authentication/microsoft-logins) authentication provider requires a hardware key or another 2FA approach.</span></span> <span data-ttu-id="6ba0a-116">如果預設範本強制執行 「 本機 」 2FA 使用者會需要滿足兩種 2FA 方法，這是不常使用的案例。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-116">If the default templates enforced "local" 2FA then users would be required to satisfy two 2FA approaches, which is not a commonly used scenario.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="6ba0a-117">新增至 2FA 的 [設定] 頁面的 QR 代碼</span><span class="sxs-lookup"><span data-stu-id="6ba0a-117">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="6ba0a-118">使用這些指示*qrcode.js*從 https://davidshimjs.github.io/qrcodejs/ 存放庫。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-118">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="6ba0a-119">下載[qrcode.js javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)至`wwwroot\lib`專案中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-119">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="6ba0a-120">請依照下列中的指示[Scaffold 身分識別](xref:security/authentication/scaffold-identity)產生 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-120">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="6ba0a-121">在  */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*，找出`Scripts`檔案結尾的區段：</span><span class="sxs-lookup"><span data-stu-id="6ba0a-121">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="6ba0a-122">在  *Pages/Account/Manage/EnableAuthenticator.cshtml* （Razor 頁面） 或*Views/Manage/EnableAuthenticator.cshtml* (MVC)、 找出`Scripts`檔案結尾的區段：</span><span class="sxs-lookup"><span data-stu-id="6ba0a-122">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="6ba0a-123">更新`Scripts`一節，以將參考加入`qrcodejs`您新增的程式庫和呼叫，以產生 QR 代碼。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-123">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="6ba0a-124">它看起來應該如下：</span><span class="sxs-lookup"><span data-stu-id="6ba0a-124">It should look as follows:</span></span>

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

* <span data-ttu-id="6ba0a-125">刪除連結您在這些指示的段落。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-125">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="6ba0a-126">執行您的應用程式，並確保您可以用來掃描該 QR 代碼，並驗證驗證器證明自己的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-126">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="6ba0a-127">變更站台名稱中的 QR 代碼</span><span class="sxs-lookup"><span data-stu-id="6ba0a-127">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6ba0a-128">在 QR 程式碼中的站台名稱是取自您選擇一開始建立您的專案時的專案名稱。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-128">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="6ba0a-129">您可以變更其尋求`GenerateQrCodeUri(string email, string unformattedKey)`方法中的 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-129">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6ba0a-130">在 QR 程式碼中的站台名稱是取自您選擇一開始建立您的專案時的專案名稱。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-130">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="6ba0a-131">您可以變更其尋求`GenerateQrCodeUri(string email, string unformattedKey)`方法中的*Pages/Account/Manage/EnableAuthenticator.cshtml.cs* （Razor 頁面） 檔案或*Controllers/ManageController.cs* (MVC) 檔案。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-131">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6ba0a-132">從範本的預設程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="6ba0a-132">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="6ba0a-133">第二個參數，在呼叫`string.Format`是您的站台名稱，取自您方案的名稱。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-133">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="6ba0a-134">它可以變更為任何值，但必須一律為 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-134">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="6ba0a-135">使用不同的 QR 代碼程式庫</span><span class="sxs-lookup"><span data-stu-id="6ba0a-135">Using a different QR Code library</span></span>

<span data-ttu-id="6ba0a-136">您可以使用您慣用的程式庫，來取代 QR 程式碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-136">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="6ba0a-137">包含 HTML`qrCode`您可以在其中放置 QR 代碼透過任何機制的項目會提供您的程式庫。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-137">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="6ba0a-138">QR 代碼的正確格式的 URL 位於:</span><span class="sxs-lookup"><span data-stu-id="6ba0a-138">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="6ba0a-139">`AuthenticatorUri` 模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-139">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="6ba0a-140">`data-url` 中的屬性`qrCodeData`項目。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-140">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="6ba0a-141">用於用戶端和伺服器時間扭曲</span><span class="sxs-lookup"><span data-stu-id="6ba0a-141">TOTP client and server time skew</span></span>

<span data-ttu-id="6ba0a-142">TOTP （以時間為基礎的單次密碼） 驗證取決於伺服器和驗證器的裝置，具有正確的時間。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-142">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="6ba0a-143">權杖只會持續 30 秒。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-143">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="6ba0a-144">如果 TOTP 2FA 登入失敗，請檢查伺服器時間是否正確，且最好是同步處理至正確的 NTP 服務。</span><span class="sxs-lookup"><span data-stu-id="6ba0a-144">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
