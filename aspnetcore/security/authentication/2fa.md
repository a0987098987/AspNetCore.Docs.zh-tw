---
title: "使用 SMS 雙因素驗證"
author: rick-anderson
description: "示範如何設定 ASP.NET Core 的雙因素驗證 (2FA)"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 721c4c20234c7232b509a0cff444538c2cfeb166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="e2eba-103">使用 SMS 雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="e2eba-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="e2eba-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[瑞士開發人員](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="e2eba-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="e2eba-105">此教學課程適用於 ASP.NET Core 只 1.x。</span><span class="sxs-lookup"><span data-stu-id="e2eba-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="e2eba-106">請參閱[啟用 QR 代碼產生的驗證器應用程式中 ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) ASP.NET Core 2.0 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="e2eba-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="e2eba-107">本教學課程會示範如何設定使用 SMS 進行雙因素驗證 (2FA)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="e2eba-108">針對給定指示[twilio](https://www.twilio.com/)和[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)，但是您可以使用任何其他 SMS 提供者。</span><span class="sxs-lookup"><span data-stu-id="e2eba-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="e2eba-109">我們建議您完成[帳戶確認和密碼復原](accconfirm.md)再開始本教學課程。</span><span class="sxs-lookup"><span data-stu-id="e2eba-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="e2eba-110">檢視[完成的範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="e2eba-111">[如何下載](xref:tutorials/index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="e2eba-112">建立新的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="e2eba-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="e2eba-113">建立新的 ASP.NET Core web app，名為`Web2FA`與個別使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="e2eba-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="e2eba-114">請依照下列中的指示[強制執行的 ASP.NET Core 應用程式中的 SSL](xref:security/enforcing-ssl)設定，並要求使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="e2eba-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="e2eba-115">建立 SMS 帳戶</span><span class="sxs-lookup"><span data-stu-id="e2eba-115">Create an SMS account</span></span>

<span data-ttu-id="e2eba-116">SMS 帳戶，例如，建立從[twilio](https://www.twilio.com/)或[ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="e2eba-117">記錄的驗證認證 (如 twilio: accountSid 和 authToken，如 ASPSMS： 使用者金鑰和密碼)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="e2eba-118">找出 SMS 提供者認證</span><span class="sxs-lookup"><span data-stu-id="e2eba-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="e2eba-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-119">**Twilio:**</span></span>  
<span data-ttu-id="e2eba-120">從您的 Twilio 帳戶的 [儀表板] 索引標籤中，複製**帳戶 SID**和**驗證語彙基元**。</span><span class="sxs-lookup"><span data-stu-id="e2eba-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="e2eba-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-121">**ASPSMS:**</span></span>  
<span data-ttu-id="e2eba-122">從您的帳戶設定，瀏覽至**使用者金鑰**並將它連同複製您**密碼**。</span><span class="sxs-lookup"><span data-stu-id="e2eba-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="e2eba-123">我們稍後會儲存密碼管理員工具內的金鑰以這些值`SMSAccountIdentification`和`SMSAccountPassword`。</span><span class="sxs-lookup"><span data-stu-id="e2eba-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="e2eba-124">指定寄件者識別碼 / 訂閱者</span><span class="sxs-lookup"><span data-stu-id="e2eba-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="e2eba-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-125">**Twilio:**</span></span>  
<span data-ttu-id="e2eba-126">從 [數字] 索引標籤中，複製到您的 Twilio**電話號碼**。</span><span class="sxs-lookup"><span data-stu-id="e2eba-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="e2eba-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-127">**ASPSMS:**</span></span>  
<span data-ttu-id="e2eba-128">在解除鎖定發送者功能表上，解除鎖定一或多個發送者或選擇英數字元的建立者 （所有網路不都支援）。</span><span class="sxs-lookup"><span data-stu-id="e2eba-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="e2eba-129">我們稍後將儲存這個值與密碼管理員工具，在機碼`SMSAccountFrom`。</span><span class="sxs-lookup"><span data-stu-id="e2eba-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="e2eba-130">SMS 服務提供的認證</span><span class="sxs-lookup"><span data-stu-id="e2eba-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="e2eba-131">我們將使用[選項模式](xref:fundamentals/configuration/options)存取使用者帳戶和金鑰設定。</span><span class="sxs-lookup"><span data-stu-id="e2eba-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="e2eba-132">建立類別以擷取安全 SMS 索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e2eba-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="e2eba-133">此範例中，`SMSoptions`中建立類別*Services/SMSoptions.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="e2eba-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="e2eba-134">設定`SMSAccountIdentification`，`SMSAccountPassword`和`SMSAccountFrom`與[密碼管理員工具](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="e2eba-135">例如: </span><span class="sxs-lookup"><span data-stu-id="e2eba-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="e2eba-136">新增 SMS 提供者 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e2eba-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="e2eba-137">從封裝管理員主控台 (PMC) 執行：</span><span class="sxs-lookup"><span data-stu-id="e2eba-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="e2eba-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="e2eba-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="e2eba-140">將程式碼中的加入*Services/MessageServices.cs*檔案，以啟用 SMS。</span><span class="sxs-lookup"><span data-stu-id="e2eba-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="e2eba-141">使用 Twilio 或 ASPSMS 區段：</span><span class="sxs-lookup"><span data-stu-id="e2eba-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="e2eba-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-142">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="e2eba-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2eba-143">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="e2eba-144">設定要使用的啟動 `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="e2eba-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="e2eba-145">新增`SMSoptions`中的服務容器`ConfigureServices`方法中的*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e2eba-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="e2eba-146">啟用雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="e2eba-146">Enable two-factor authentication</span></span>

<span data-ttu-id="e2eba-147">開啟*Views/Manage/Index.cshtml* Razor 檢視檔案和移除註解字元 （因此沒有標記為元 out）。</span><span class="sxs-lookup"><span data-stu-id="e2eba-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="e2eba-148">登入雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="e2eba-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="e2eba-149">執行應用程式並註冊新的使用者</span><span class="sxs-lookup"><span data-stu-id="e2eba-149">Run the app and register a new user</span></span>

![Web 應用程式在 Microsoft Edge 中開啟檢視的暫存器](2fa/_static/login2fa1.png)

* <span data-ttu-id="e2eba-151">點選 您的使用者名稱，就會啟動`Index`管理控制器中的動作方法。</span><span class="sxs-lookup"><span data-stu-id="e2eba-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="e2eba-152">然後點選 電話號碼**新增**連結。</span><span class="sxs-lookup"><span data-stu-id="e2eba-152">Then tap the phone number **Add** link.</span></span>

![管理檢視](2fa/_static/login2fa2.png)

* <span data-ttu-id="e2eba-154">新增電話號碼，將會收到驗證碼，並點選**傳送驗證碼**。</span><span class="sxs-lookup"><span data-stu-id="e2eba-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![新增電話號碼 頁面](2fa/_static/login2fa3.png)

* <span data-ttu-id="e2eba-156">您會取得驗證碼的簡訊。</span><span class="sxs-lookup"><span data-stu-id="e2eba-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="e2eba-157">請輸入並點選**送出**</span><span class="sxs-lookup"><span data-stu-id="e2eba-157">Enter it and tap **Submit**</span></span>

![確認電話號碼 頁面](2fa/_static/login2fa4.png)

<span data-ttu-id="e2eba-159">如果您未收到簡訊，請參閱 twilio 記錄 頁面。</span><span class="sxs-lookup"><span data-stu-id="e2eba-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="e2eba-160">管理檢視會顯示已成功新增您的電話號碼。</span><span class="sxs-lookup"><span data-stu-id="e2eba-160">The Manage view shows your phone number was added successfully.</span></span>

![管理檢視](2fa/_static/login2fa5.png)

* <span data-ttu-id="e2eba-162">點選**啟用**啟用雙因素驗證。</span><span class="sxs-lookup"><span data-stu-id="e2eba-162">Tap **Enable** to enable two-factor authentication.</span></span>

![管理檢視](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="e2eba-164">測試兩個雙因素驗證</span><span class="sxs-lookup"><span data-stu-id="e2eba-164">Test two-factor authentication</span></span>

* <span data-ttu-id="e2eba-165">登出。</span><span class="sxs-lookup"><span data-stu-id="e2eba-165">Log off.</span></span>

* <span data-ttu-id="e2eba-166">登入。</span><span class="sxs-lookup"><span data-stu-id="e2eba-166">Log in.</span></span>

* <span data-ttu-id="e2eba-167">使用者帳戶已啟用雙因素驗證，所以您不必提供第二個驗證因素。</span><span class="sxs-lookup"><span data-stu-id="e2eba-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="e2eba-168">在本教學課程中，您已啟用電話驗證。</span><span class="sxs-lookup"><span data-stu-id="e2eba-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="e2eba-169">內建的範本也可讓您設定電子郵件做為第二個因素。</span><span class="sxs-lookup"><span data-stu-id="e2eba-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="e2eba-170">您可以設定其他的第二個因素，例如 QR 代碼驗證。</span><span class="sxs-lookup"><span data-stu-id="e2eba-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="e2eba-171">點選**提交**。</span><span class="sxs-lookup"><span data-stu-id="e2eba-171">Tap **Submit**.</span></span>

![傳送驗證碼 檢視](2fa/_static/login2fa7.png)

* <span data-ttu-id="e2eba-173">輸入您在簡訊中收到的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e2eba-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="e2eba-174">按一下**記住此瀏覽器**核取方塊將會豁免需要使用 2FA 時使用相同的裝置和瀏覽器登入。</span><span class="sxs-lookup"><span data-stu-id="e2eba-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="e2eba-175">啟用 2FA，並按一下**記住此瀏覽器**會為您提供強式 2FA 保護惡意使用者嘗試存取您的帳戶，只要它們不能存取您的裝置。</span><span class="sxs-lookup"><span data-stu-id="e2eba-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="e2eba-176">您可以經常使用的任何私用裝置上執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="e2eba-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="e2eba-177">藉由設定**記住此瀏覽器**、 2FA 的額外的安全性收到您不定期使用的裝置，且您在不需要經過您自己的裝置上的 2FA 方便。</span><span class="sxs-lookup"><span data-stu-id="e2eba-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![確認檢視](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="e2eba-179">帳戶鎖定，防止暴力攻擊</span><span class="sxs-lookup"><span data-stu-id="e2eba-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="e2eba-180">我們建議您在使用 2FA 帳戶鎖定。</span><span class="sxs-lookup"><span data-stu-id="e2eba-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="e2eba-181">一旦在使用者登入 （透過社交帳戶或本機帳戶），儲存每個在 2FA 的嘗試失敗，而且達到最大嘗試次數 （預設值為 5） 時，如果使用者遭到鎖定五分鐘的時間 (您可以設定時間內無鎖定`DefaultAccountLockoutTimeSpan`)。</span><span class="sxs-lookup"><span data-stu-id="e2eba-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="e2eba-182">下列設定帳戶鎖定的嘗試失敗 10 後 10 分鐘。</span><span class="sxs-lookup"><span data-stu-id="e2eba-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
