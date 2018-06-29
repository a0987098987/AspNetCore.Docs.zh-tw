---
title: 使用 SMS 中 ASP.NET Core 雙因素驗證
author: rick-anderson
description: 了解如何設定雙因素驗證 (2FA) 與 ASP.NET Core 應用程式。
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 0308b05ebcda1af7f6850549d7a33f1df1a912a0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089980"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="64d87-103">使用 SMS 中 ASP.NET Core 雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="64d87-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="64d87-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士開發人員](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="64d87-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

 <span data-ttu-id="64d87-105">兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的方法，如 2FA 產業。</span><span class="sxs-lookup"><span data-stu-id="64d87-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="64d87-106">2FA 使用 TOTP 最好 SMS 2FA。</span><span class="sxs-lookup"><span data-stu-id="64d87-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="64d87-107">如需詳細資訊，請參閱[TOTP 驗證器應用程式中 ASP.NET Core 啟用 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)ASP.NET Core 2.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="64d87-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="64d87-108">本教學課程會示範如何設定使用 SMS 進行雙因素驗證 (2FA)。</span><span class="sxs-lookup"><span data-stu-id="64d87-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="64d87-109">針對給定指示[twilio](https://www.twilio.com/)和[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)，但是您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="64d87-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="64d87-110">我們建議您完成[帳戶確認和密碼復原](xref:security/authentication/accconfirm)再開始本教學課程。</span><span class="sxs-lookup"><span data-stu-id="64d87-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="64d87-111">檢視[完成的範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。</span><span class="sxs-lookup"><span data-stu-id="64d87-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="64d87-112">[如何下載](xref:tutorials/index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="64d87-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="64d87-113">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="64d87-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="64d87-114">建立新的 ASP.NET Core web app，名為`Web2FA`與個別使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="64d87-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="64d87-115">請依照下列中的指示[強制執行的 ASP.NET Core 應用程式中的 SSL](xref:security/enforcing-ssl)設定，並要求使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="64d87-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="64d87-116">建立 SMS 帳戶</span><span class="sxs-lookup"><span data-stu-id="64d87-116">Create an SMS account</span></span>

<span data-ttu-id="64d87-117">SMS 帳戶，例如，建立從[twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。</span><span class="sxs-lookup"><span data-stu-id="64d87-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="64d87-118">記錄的驗證認證 (如 twilio: accountSid 和 authToken，如 ASPSMS： 使用者金鑰和密碼)。</span><span class="sxs-lookup"><span data-stu-id="64d87-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="64d87-119">找出 SMS 提供者認證</span><span class="sxs-lookup"><span data-stu-id="64d87-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="64d87-120">**Twilio:** 您 Twilio 帳戶的 [儀表板] 索引標籤中，從複製**帳戶 SID**和**驗證語彙基元**。</span><span class="sxs-lookup"><span data-stu-id="64d87-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="64d87-121">**ASPSMS:** 從您的帳戶設定，瀏覽至**使用者金鑰**並將它連同複製您**密碼**。</span><span class="sxs-lookup"><span data-stu-id="64d87-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="64d87-122">我們稍後會儲存密碼管理員工具內的金鑰以這些值`SMSAccountIdentification`和`SMSAccountPassword`。</span><span class="sxs-lookup"><span data-stu-id="64d87-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="64d87-123">指定寄件者識別碼 / 訂閱者</span><span class="sxs-lookup"><span data-stu-id="64d87-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="64d87-124">**Twilio:** 從數字 索引標籤上，複製到您的 Twilio**電話號碼**。</span><span class="sxs-lookup"><span data-stu-id="64d87-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="64d87-125">**ASPSMS:** 中解除鎖定發送者功能表上，解除鎖定一或多個的建立者，或選擇英數字元的建立者 （所有網路不都支援）。</span><span class="sxs-lookup"><span data-stu-id="64d87-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="64d87-126">我們稍後將儲存這個值與密碼管理員工具，在機碼`SMSAccountFrom`。</span><span class="sxs-lookup"><span data-stu-id="64d87-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="64d87-127">SMS 服務提供的認證</span><span class="sxs-lookup"><span data-stu-id="64d87-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="64d87-128">我們將使用[選項模式](xref:fundamentals/configuration/options)存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="64d87-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="64d87-129">建立類別以擷取安全 SMS 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="64d87-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="64d87-130">此範例中，`SMSoptions`中建立類別*Services/SMSoptions.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="64d87-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="64d87-131">設定`SMSAccountIdentification`，`SMSAccountPassword`和`SMSAccountFrom`與[密碼管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="64d87-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="64d87-132">例如: </span><span class="sxs-lookup"><span data-stu-id="64d87-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="64d87-133">新增 SMS 提供者 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="64d87-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="64d87-134">從封裝管理員主控台 (PMC) 執行：</span><span class="sxs-lookup"><span data-stu-id="64d87-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="64d87-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="64d87-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="64d87-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="64d87-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="64d87-137">將程式碼中的加入*Services/MessageServices.cs*檔案，以啟用 SMS。</span><span class="sxs-lookup"><span data-stu-id="64d87-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="64d87-138">使用 Twilio 或 ASPSMS 區段：</span><span class="sxs-lookup"><span data-stu-id="64d87-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="64d87-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="64d87-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="64d87-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="64d87-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="64d87-141">設定要使用的啟動 `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="64d87-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="64d87-142">新增`SMSoptions`中的服務容器`ConfigureServices`方法中的*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="64d87-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="64d87-143">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="64d87-143">Enable two-factor authentication</span></span>

<span data-ttu-id="64d87-144">開啟*Views/Manage/Index.cshtml* Razor 檢視檔案和移除註解字元 （因此沒有標記為元 out）。</span><span class="sxs-lookup"><span data-stu-id="64d87-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="64d87-145">登入雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="64d87-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="64d87-146">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="64d87-146">Run the app and register a new user</span></span>

![Web 應用程式在 Microsoft Edge 中開啟檢視的暫存器](2fa/_static/login2fa1.png)

* <span data-ttu-id="64d87-148">點選 您的使用者名稱，就會啟動`Index`管理控制器中的動作方法。</span><span class="sxs-lookup"><span data-stu-id="64d87-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="64d87-149">然後點選 電話號碼**新增**連結。</span><span class="sxs-lookup"><span data-stu-id="64d87-149">Then tap the phone number **Add** link.</span></span>

![管理檢視](2fa/_static/login2fa2.png)

* <span data-ttu-id="64d87-151">新增電話號碼，將會收到驗證碼，並點選**傳送驗證碼**。</span><span class="sxs-lookup"><span data-stu-id="64d87-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![新增電話號碼 頁面](2fa/_static/login2fa3.png)

* <span data-ttu-id="64d87-153">您會取得驗證碼的簡訊。</span><span class="sxs-lookup"><span data-stu-id="64d87-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="64d87-154">請輸入並點選**送出**</span><span class="sxs-lookup"><span data-stu-id="64d87-154">Enter it and tap **Submit**</span></span>

![確認電話號碼 頁面](2fa/_static/login2fa4.png)

<span data-ttu-id="64d87-156">如果您未收到簡訊，請參閱 twilio 記錄 頁面。</span><span class="sxs-lookup"><span data-stu-id="64d87-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="64d87-157">管理檢視會顯示已成功新增您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="64d87-157">The Manage view shows your phone number was added successfully.</span></span>

![管理檢視](2fa/_static/login2fa5.png)

* <span data-ttu-id="64d87-159">點選**啟用**啟用雙因素驗證。</span><span class="sxs-lookup"><span data-stu-id="64d87-159">Tap **Enable** to enable two-factor authentication.</span></span>

![管理檢視](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="64d87-161">測試兩個雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="64d87-161">Test two-factor authentication</span></span>

* <span data-ttu-id="64d87-162">登出。</span><span class="sxs-lookup"><span data-stu-id="64d87-162">Log off.</span></span>

* <span data-ttu-id="64d87-163">登入。</span><span class="sxs-lookup"><span data-stu-id="64d87-163">Log in.</span></span>

* <span data-ttu-id="64d87-164">使用者帳戶已啟用雙因素驗證，所以您不必提供第二個驗證因素。</span><span class="sxs-lookup"><span data-stu-id="64d87-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="64d87-165">在本教學課程中，您已啟用電話驗證。</span><span class="sxs-lookup"><span data-stu-id="64d87-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="64d87-166">內建的範本也可讓您設定電子郵件做為第二個因素。</span><span class="sxs-lookup"><span data-stu-id="64d87-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="64d87-167">您可以設定其他的第二個因素，例如 QR 代碼驗證。</span><span class="sxs-lookup"><span data-stu-id="64d87-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="64d87-168">點選**提交**。</span><span class="sxs-lookup"><span data-stu-id="64d87-168">Tap **Submit**.</span></span>

![傳送驗證碼 檢視](2fa/_static/login2fa7.png)

* <span data-ttu-id="64d87-170">輸入您在簡訊中收到的程式碼。</span><span class="sxs-lookup"><span data-stu-id="64d87-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="64d87-171">按一下**記住此瀏覽器**核取方塊將會豁免需要使用 2FA 時使用相同的裝置和瀏覽器登入。</span><span class="sxs-lookup"><span data-stu-id="64d87-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="64d87-172">啟用 2FA，並按一下**記住此瀏覽器**會為您提供強式 2FA 保護惡意使用者嘗試存取您的帳戶，只要它們不能存取您的裝置。</span><span class="sxs-lookup"><span data-stu-id="64d87-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="64d87-173">您可以經常使用的任何私用裝置上執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="64d87-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="64d87-174">藉由設定**記住此瀏覽器**、 2FA 的額外的安全性收到您不定期使用的裝置，且您在不需要經過您自己的裝置上的 2FA 方便。</span><span class="sxs-lookup"><span data-stu-id="64d87-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![確認檢視](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="64d87-176">帳戶鎖定，防止暴力攻擊</span><span class="sxs-lookup"><span data-stu-id="64d87-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="64d87-177">帳戶鎖定的建議是使用 2FA。</span><span class="sxs-lookup"><span data-stu-id="64d87-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="64d87-178">一旦使用者登入時透過社交帳戶或本機帳戶，會儲存在 2FA 每個失敗的嘗試。</span><span class="sxs-lookup"><span data-stu-id="64d87-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="64d87-179">如果已到達最大失敗的存取嘗試，鎖定使用者 (預設： 5 存取嘗試失敗後的 5 分鐘鎖定)。</span><span class="sxs-lookup"><span data-stu-id="64d87-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="64d87-180">驗證成功重設失敗的存取嘗試次數和重設時鐘。</span><span class="sxs-lookup"><span data-stu-id="64d87-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="64d87-181">最大失敗存取嘗試，且可以使用設定鎖定的時間[MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts)和[DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan)。</span><span class="sxs-lookup"><span data-stu-id="64d87-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="64d87-182">下列組態會設定帳戶鎖定存取嘗試失敗 10 後 10 分鐘：</span><span class="sxs-lookup"><span data-stu-id="64d87-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="64d87-183">確認[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)設定`lockoutOnFailure`至`true`:</span><span class="sxs-lookup"><span data-stu-id="64d87-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
