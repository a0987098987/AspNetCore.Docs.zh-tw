---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: 若要防止 Bot 使用 ASP.NET Web Razor 使用 CAPTCHA） 的站台 |Microsoft 文件
author: microsoft
description: 這篇文章說明如何使用 ReCaptcha （安全性量值），以防止自動的程式 (bot) 執行工作中的 ASP.NET Web Pages (Razor) 我們...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529917"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="a1b2d-103">若要防止 Bot 使用 ASP.NET Web Razor 使用 CAPTCHA） 的站台</span><span class="sxs-lookup"><span data-stu-id="a1b2d-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="a1b2d-104">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a1b2d-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a1b2d-105">本文說明如何使用 ReCaptcha （安全性量值），以防止自動的程式 (bot) 在 ASP.NET Web Pages (Razor) 網站中執行工作。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="a1b2d-106">**您將學習：**</span><span class="sxs-lookup"><span data-stu-id="a1b2d-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="a1b2d-107">如何將 CAPTCHA 測試加入至您的網站。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="a1b2d-108">這些是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="a1b2d-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="a1b2d-109">`ReCaptcha`協助程式。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="a1b2d-110">本主題資訊適用於 ASP.NET Web Pages 1.0 和 Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="a1b2d-111">關於 CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="a1b2d-111">About CAPTCHAs</span></span>

<span data-ttu-id="a1b2d-112">每當您讓註冊的人員，在您的網站，或甚至只輸入名稱和 URL （類似的部落格的註解），您可能會收到一大堆假的名稱。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="a1b2d-113">這些通常是左嘗試保留 Url 中可以找到每個網站的自動程式 (bot)。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="a1b2d-114">（常見的動機是要張貼的產品銷售的 Url）。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="a1b2d-115">您可以協助確定使用者使用真人，而不是電腦的程式是*CAPTCHA*來註冊或否則輸入其名稱和站台時，驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="a1b2d-116">CAPTCHA 代表完全自動化公用 Turing 測試，以告訴電腦，以及讓人了距離。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="a1b2d-117">CAPTCHA 是*挑戰-回應*要求執行某個動作的使用者容易供使用者執行，但難以自動化程式來執行的測試。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="a1b2d-118">最常見的 CAPTCHA 類型則是您看到某些扭曲的代號並要求您輸入它們。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="a1b2d-119">（扭曲程度應該讓進行破解字母機器人。）</span><span class="sxs-lookup"><span data-stu-id="a1b2d-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="a1b2d-120">新增 ReCaptcha 測試</span><span class="sxs-lookup"><span data-stu-id="a1b2d-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="a1b2d-121">在 ASP.NET 網頁中，您可以使用`ReCaptcha`helper 來呈現 ReCaptcha 服務為基礎的 CAPTCHA 測試 ([http://recaptcha.net](http://recaptcha.net))。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="a1b2d-122">`ReCaptcha` Helper 會顯示兩個使用者需要驗證頁面之前，請輸入正確的扭曲單字的映像。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="a1b2d-123">服務驗證使用者回應。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="a1b2d-124">註冊您的網站，在 ([http://recaptcha.net](http://recaptcha.net))。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="a1b2d-125">當您已經完成註冊時，您會取得公開金鑰和私密金鑰。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="a1b2d-126">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="a1b2d-127">如果您還沒有 *\_AppStart.cshtml*檔案中，網站的根資料夾中建立名為 *\_AppStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="a1b2d-128">加入下列`Recaptcha`協助程式設定中的 *\_AppStart.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="a1b2d-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="a1b2d-129">設定`PublicKey`和`PrivateKey`使用您自己的公用和私用金鑰的屬性。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="a1b2d-130">儲存 *\_AppStart.cshtml*檔案並將它關閉。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="a1b2d-131">在網站的根資料夾中，建立名為新頁面*Recaptcha.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="a1b2d-132">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="a1b2d-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="a1b2d-133">執行*Recaptcha.cshtml*瀏覽器中的。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="a1b2d-134">如果`PrivateKey`值是否有效，頁面會顯示 ReCaptcha 控制項和按鈕。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="a1b2d-135">如果您必須在全域設定索引鍵 *\_AppStart.html*，頁面會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="a1b2d-136">測試輸入的字。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-136">Enter the words for the test.</span></span> <span data-ttu-id="a1b2d-137">如果您傳遞 ReCaptcha 測試，您會看到一則訊息。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="a1b2d-138">否則您會看到一則錯誤訊息和 ReCaptcha 控制項重新顯示。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="a1b2d-139">如果您的電腦上使用 proxy 伺服器的網域，您可能需要設定`defaultproxy`元素*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="a1b2d-140">下列範例所示*Web.config*檔案搭配`defaultproxy`設定為啟用 ReCaptcha 服務運作的項目。</span><span class="sxs-lookup"><span data-stu-id="a1b2d-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a1b2d-141">其他資源</span><span class="sxs-lookup"><span data-stu-id="a1b2d-141">Additional Resources</span></span>


- [<span data-ttu-id="a1b2d-142">ASP.NET Web Pages 站台中的自訂全站台的行為</span><span class="sxs-lookup"><span data-stu-id="a1b2d-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="a1b2d-143">ReCaptcha 站台</span><span class="sxs-lookup"><span data-stu-id="a1b2d-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
