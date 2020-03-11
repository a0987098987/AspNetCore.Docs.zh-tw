---
title: 在 ASP.NET Core 中使用 SMS 的雙因素驗證
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 應用程式來設定雙因素驗證（2FA）。
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661452"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="71620-103">在 ASP.NET Core 中使用 SMS 的雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="71620-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="71620-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士開發人員](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="71620-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="71620-105">使用以時間為基礎的一次性密碼演算法（TOTP）的雙因素驗證（2FA）驗證器應用程式是2FA 的業界建議方法。</span><span class="sxs-lookup"><span data-stu-id="71620-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="71620-106">2FA 使用 TOTP 是 SMS 2FA 的慣用選項。</span><span class="sxs-lookup"><span data-stu-id="71620-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="71620-107">如需詳細資訊，請參閱在 ASP.NET Core for ASP.NET Core 2.0 和更新版本[中啟用 TOTP 驗證器應用程式的 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)。</span><span class="sxs-lookup"><span data-stu-id="71620-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="71620-108">本教學課程說明如何使用 SMS 來設定雙因素驗證（2FA）。</span><span class="sxs-lookup"><span data-stu-id="71620-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="71620-109">系統會提供[twilio](https://www.twilio.com/)和[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)的指示，但是您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="71620-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="71620-110">我們建議您在開始本教學課程之前，先完成[帳戶確認和密碼](xref:security/authentication/accconfirm)復原。</span><span class="sxs-lookup"><span data-stu-id="71620-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="71620-111">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。</span><span class="sxs-lookup"><span data-stu-id="71620-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="71620-112">[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="71620-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="71620-113">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="71620-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="71620-114">使用個別使用者帳戶建立名為 `Web2FA` 的新 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="71620-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="71620-115">依照 <xref:security/enforcing-ssl> 中的指示來設定和要求 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="71620-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="71620-116">建立 SMS 帳戶</span><span class="sxs-lookup"><span data-stu-id="71620-116">Create an SMS account</span></span>

<span data-ttu-id="71620-117">建立 SMS 帳戶，例如，從[twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。</span><span class="sxs-lookup"><span data-stu-id="71620-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="71620-118">記錄驗證認證（適用于 twilio： accountSid 和 authToken，適用于 ASPSMS： Userkey 和 Password）。</span><span class="sxs-lookup"><span data-stu-id="71620-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="71620-119">找出 SMS 提供者認證</span><span class="sxs-lookup"><span data-stu-id="71620-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="71620-120">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="71620-120">**Twilio:**</span></span>

<span data-ttu-id="71620-121">從 Twilio 帳戶的 [儀表板] 索引標籤中，複製 [**帳戶 SID** ] 和 [**驗證權杖**]。</span><span class="sxs-lookup"><span data-stu-id="71620-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="71620-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="71620-122">**ASPSMS:**</span></span>

<span data-ttu-id="71620-123">從您的帳戶設定中，流覽至**Userkey** ，並將它與您的**密碼**一起複製。</span><span class="sxs-lookup"><span data-stu-id="71620-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="71620-124">稍後我們會將這些值儲存在中，並在金鑰 `SMSAccountIdentification` 和 `SMSAccountPassword`內使用秘密管理員工具。</span><span class="sxs-lookup"><span data-stu-id="71620-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="71620-125">指定 SenderID/發信者</span><span class="sxs-lookup"><span data-stu-id="71620-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="71620-126">**Twilio：** 在 [數位] 索引標籤中，複製您的 Twilio**電話號碼**。</span><span class="sxs-lookup"><span data-stu-id="71620-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="71620-127">**ASPSMS：** 在 [解除鎖定擁有者] 功能表中，解除鎖定一或多個始發者，或選擇英數位元（並非所有網路都支援）。</span><span class="sxs-lookup"><span data-stu-id="71620-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="71620-128">我們稍後將使用金鑰 `SMSAccountFrom`中的秘密管理員工具來儲存此值。</span><span class="sxs-lookup"><span data-stu-id="71620-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="71620-129">提供 SMS 服務的認證</span><span class="sxs-lookup"><span data-stu-id="71620-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="71620-130">我們將使用 [[選項] 模式](xref:fundamentals/configuration/options)來存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="71620-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="71620-131">建立類別來提取安全 SMS 金鑰。</span><span class="sxs-lookup"><span data-stu-id="71620-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="71620-132">在此範例中，會在*Services/SMSoptions. .cs*檔案中建立 `SMSoptions` 類別。</span><span class="sxs-lookup"><span data-stu-id="71620-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="71620-133">使用「[密碼管理員」工具](xref:security/app-secrets)來設定 `SMSAccountIdentification`、`SMSAccountPassword` 和 `SMSAccountFrom`。</span><span class="sxs-lookup"><span data-stu-id="71620-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="71620-134">例如：</span><span class="sxs-lookup"><span data-stu-id="71620-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="71620-135">新增 SMS 提供者的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="71620-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="71620-136">從 [套件管理員主控台] （PMC）執行：</span><span class="sxs-lookup"><span data-stu-id="71620-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="71620-137">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="71620-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="71620-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="71620-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="71620-139">在*Services/MessageServices .cs*檔案中新增程式碼，以啟用 SMS。</span><span class="sxs-lookup"><span data-stu-id="71620-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="71620-140">請使用 Twilio 或 ASPSMS 區段：</span><span class="sxs-lookup"><span data-stu-id="71620-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="71620-141">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="71620-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="71620-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="71620-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="71620-143">設定要使用 `SMSoptions` 的啟動</span><span class="sxs-lookup"><span data-stu-id="71620-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="71620-144">在*Startup.cs*的 `ConfigureServices` 方法中，將 `SMSoptions` 新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="71620-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="71620-145">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="71620-145">Enable two-factor authentication</span></span>

<span data-ttu-id="71620-146">開啟*Views/Manage/Index. cshtml* Razor view 檔案並移除批註字元（因此不會註解標記）。</span><span class="sxs-lookup"><span data-stu-id="71620-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="71620-147">使用雙因素驗證進行登入</span><span class="sxs-lookup"><span data-stu-id="71620-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="71620-148">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="71620-148">Run the app and register a new user</span></span>

![在 Microsoft Edge 中開啟 Web 應用程式註冊視圖](2fa/_static/login2fa1.png)

* <span data-ttu-id="71620-150">在您的使用者名稱上按一下，即可啟動 [管理控制器] 中的 [`Index` 動作] 方法。</span><span class="sxs-lookup"><span data-stu-id="71620-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="71620-151">然後，按一下 [電話號碼] [**新增**] 連結。</span><span class="sxs-lookup"><span data-stu-id="71620-151">Then tap the phone number **Add** link.</span></span>

![管理檢視-點一下 [新增] 連結](2fa/_static/login2fa2.png)

* <span data-ttu-id="71620-153">新增將接收驗證碼的電話號碼，然後按一下 [**傳送驗證碼**]。</span><span class="sxs-lookup"><span data-stu-id="71620-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![[新增電話號碼] 頁面](2fa/_static/login2fa3.png)

* <span data-ttu-id="71620-155">您會收到包含驗證碼的文字訊息。</span><span class="sxs-lookup"><span data-stu-id="71620-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="71620-156">輸入它，然後按 [**提交**]</span><span class="sxs-lookup"><span data-stu-id="71620-156">Enter it and tap **Submit**</span></span>

![驗證電話號碼頁面](2fa/_static/login2fa4.png)

<span data-ttu-id="71620-158">如果您沒有收到文字訊息，請參閱 twilio 記錄頁面。</span><span class="sxs-lookup"><span data-stu-id="71620-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="71620-159">[管理] 視圖顯示已成功新增您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="71620-159">The Manage view shows your phone number was added successfully.</span></span>

![管理檢視-已成功新增電話號碼](2fa/_static/login2fa5.png)

* <span data-ttu-id="71620-161">請按 [**啟用**] 以啟用雙因素驗證。</span><span class="sxs-lookup"><span data-stu-id="71620-161">Tap **Enable** to enable two-factor authentication.</span></span>

![管理 view-啟用雙因素驗證](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="71620-163">測試雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="71620-163">Test two-factor authentication</span></span>

* <span data-ttu-id="71620-164">登出。</span><span class="sxs-lookup"><span data-stu-id="71620-164">Log off.</span></span>

* <span data-ttu-id="71620-165">登入。</span><span class="sxs-lookup"><span data-stu-id="71620-165">Log in.</span></span>

* <span data-ttu-id="71620-166">使用者帳戶已啟用雙因素驗證，因此您必須提供第二個驗證要素。</span><span class="sxs-lookup"><span data-stu-id="71620-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="71620-167">在本教學課程中，您已啟用電話驗證。</span><span class="sxs-lookup"><span data-stu-id="71620-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="71620-168">內建範本也可讓您設定電子郵件作為第二個因素。</span><span class="sxs-lookup"><span data-stu-id="71620-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="71620-169">您可以針對驗證（例如 QR 代碼）設定其他第二個因素。</span><span class="sxs-lookup"><span data-stu-id="71620-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="71620-170">請按 [**提交**]。</span><span class="sxs-lookup"><span data-stu-id="71620-170">Tap **Submit**.</span></span>

![傳送驗證碼視圖](2fa/_static/login2fa7.png)

* <span data-ttu-id="71620-172">輸入您在 SMS 訊息中取得的程式碼。</span><span class="sxs-lookup"><span data-stu-id="71620-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="71620-173">按一下 [**記住此瀏覽器**] 核取方塊，可讓您不需要在使用相同的裝置和瀏覽器時，使用2FA 登入。</span><span class="sxs-lookup"><span data-stu-id="71620-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="71620-174">啟用2FA 並按一下 [**記住此瀏覽器**] 可提供您強式2FA 保護，讓惡意使用者嘗試存取您的帳戶，只要他們無法存取您的裝置即可。</span><span class="sxs-lookup"><span data-stu-id="71620-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="71620-175">您可以在您經常使用的任何私人裝置上執行此動作。</span><span class="sxs-lookup"><span data-stu-id="71620-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="71620-176">藉由設定 [**記住此瀏覽器**]，您可以從不常使用的裝置獲得更2FA 的安全性，並讓您不必在自己的裝置上進行2FA。</span><span class="sxs-lookup"><span data-stu-id="71620-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![確認視圖](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="71620-178">防止暴力密碼破解攻擊的帳戶鎖定</span><span class="sxs-lookup"><span data-stu-id="71620-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="71620-179">建議使用2FA 來鎖定帳戶。</span><span class="sxs-lookup"><span data-stu-id="71620-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="71620-180">使用者透過本機帳戶或社交帳戶登入之後，就會在2FA 中儲存每個失敗的嘗試。</span><span class="sxs-lookup"><span data-stu-id="71620-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="71620-181">如果達到失敗的存取嘗試次數上限，則使用者會被鎖定（預設值：5分鐘的鎖定嘗試失敗存取次數）。</span><span class="sxs-lookup"><span data-stu-id="71620-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="71620-182">成功的驗證會重設失敗的存取嘗試計數，並重設時鐘。</span><span class="sxs-lookup"><span data-stu-id="71620-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="71620-183">失敗的存取嘗試和鎖定時間上限可以使用[MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts)和[DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)來設定。</span><span class="sxs-lookup"><span data-stu-id="71620-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="71620-184">以下設定在10次失敗的存取嘗試之後10分鐘的帳戶鎖定：</span><span class="sxs-lookup"><span data-stu-id="71620-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="71620-185">確認[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)將 `lockoutOnFailure` 設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="71620-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
