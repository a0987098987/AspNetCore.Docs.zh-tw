---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: 使用 CAPTCHA 避免使用 ASP.NET Web Razor Bot） 的站台 |Microsoft Docs
author: microsoft
description: 這篇文章說明如何使用 ReCaptcha （安全性量值），以防止自動的程式 (bot) 執行工作中 ASP.NET Web Pages (Razor) 我們...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803067"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="7397d-103">使用 CAPTCHA 避免 Bot 使用 ASP.NET Web Razor） 網站</span><span class="sxs-lookup"><span data-stu-id="7397d-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="7397d-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7397d-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7397d-105">這篇文章說明如何在 ASP.NET Web Pages (Razor) 網站中執行工作時，防止自動的程式 (bot) 使用 ReCaptcha （安全性量值）。</span><span class="sxs-lookup"><span data-stu-id="7397d-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="7397d-106">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="7397d-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="7397d-107">如何將 CAPTCHA 測試新增至您的網站。</span><span class="sxs-lookup"><span data-stu-id="7397d-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="7397d-108">以下是所引進的發行項 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="7397d-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="7397d-109">`ReCaptcha`協助程式。</span><span class="sxs-lookup"><span data-stu-id="7397d-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="7397d-110">這篇文章中的資訊適用於 ASP.NET Web Pages 1.0 和 Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="7397d-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="7397d-111">關於 CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="7397d-111">About CAPTCHAs</span></span>

<span data-ttu-id="7397d-112">在您的網站，或甚至只是可讓使用者註冊任何時間輸入的名稱和 URL （如部落格註解），您可能會收到一大堆假的名稱。</span><span class="sxs-lookup"><span data-stu-id="7397d-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="7397d-113">這些通常是左嘗試保留 Url，讓他們能夠找到每個網站中的自動化程式 (bot)。</span><span class="sxs-lookup"><span data-stu-id="7397d-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="7397d-114">（常見的動機是要張貼的產品銷售的 Url）。</span><span class="sxs-lookup"><span data-stu-id="7397d-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="7397d-115">請確定使用者使用的是真人，而不是電腦程式，您可以協助*CAPTCHA*來驗證使用者，當使用者註冊或否則輸入 使用者名稱和站台。</span><span class="sxs-lookup"><span data-stu-id="7397d-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="7397d-116">CAPTCHA 代表完全自動化公用 Turing 測試，以告訴電腦，以及讓人判讀分開。</span><span class="sxs-lookup"><span data-stu-id="7397d-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="7397d-117">是 CAPTCHA*挑戰-回應*測試執行一些作業會要求使用者執行個人的簡單但硬碟做為自動化程式。</span><span class="sxs-lookup"><span data-stu-id="7397d-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="7397d-118">最常見的 CAPTCHA 的類型是其中看到某些異常狀況的字母，而系統會要求輸入密碼。</span><span class="sxs-lookup"><span data-stu-id="7397d-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="7397d-119">（扭曲應該得相當困難的 bot 將解碼的字母。）</span><span class="sxs-lookup"><span data-stu-id="7397d-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="7397d-120">加入 ReCaptcha 測試</span><span class="sxs-lookup"><span data-stu-id="7397d-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="7397d-121">在 ASP.NET 網頁中，您可以使用`ReCaptcha`呈現 ReCaptcha 服務為基礎的 CAPTCHA 測試協助程式 ([http://recaptcha.net](http://recaptcha.net))。</span><span class="sxs-lookup"><span data-stu-id="7397d-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="7397d-122">`ReCaptcha`協助程式會顯示兩個使用者需要驗證頁面之前，請輸入正確的扭曲單字的映像。</span><span class="sxs-lookup"><span data-stu-id="7397d-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="7397d-123">ReCaptcha.Net 服務驗證的使用者回應。</span><span class="sxs-lookup"><span data-stu-id="7397d-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="7397d-124">註冊您的網站在 ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net))。</span><span class="sxs-lookup"><span data-stu-id="7397d-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="7397d-125">當您完成註冊之後時，您會取得公開金鑰和私密金鑰。</span><span class="sxs-lookup"><span data-stu-id="7397d-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="7397d-126">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="7397d-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="7397d-127">如果您還沒有 *\_AppStart.cshtml*檔案中，網站的根資料夾中建立名為 *\_AppStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="7397d-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="7397d-128">新增下列`Recaptcha`中的協助程式設定 *\_AppStart.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="7397d-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="7397d-129">設定`PublicKey`和`PrivateKey`使用您自己的公用和私用金鑰的屬性。</span><span class="sxs-lookup"><span data-stu-id="7397d-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="7397d-130">儲存 *\_AppStart.cshtml*檔案，並將它關閉。</span><span class="sxs-lookup"><span data-stu-id="7397d-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="7397d-131">在根資料夾中的網站，建立名為的新頁面*Recaptcha.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="7397d-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="7397d-132">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="7397d-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="7397d-133">執行*Recaptcha.cshtml*瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="7397d-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="7397d-134">如果`PrivateKey`值是否有效，此頁面會顯示 ReCaptcha 控制項和按鈕。</span><span class="sxs-lookup"><span data-stu-id="7397d-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="7397d-135">如果您必須在全域設定索引鍵 *\_AppStart.html*，頁面會顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="7397d-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="7397d-136">測試輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="7397d-136">Enter the words for the test.</span></span> <span data-ttu-id="7397d-137">如果您傳遞 ReCaptcha 測試時，您會看到一則訊息。</span><span class="sxs-lookup"><span data-stu-id="7397d-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="7397d-138">否則您會看到一則錯誤訊息，並重新顯示 ReCaptcha 控制項。</span><span class="sxs-lookup"><span data-stu-id="7397d-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="7397d-139">如果您的電腦上使用 proxy 伺服器的網域，您可能需要設定`defaultproxy`項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="7397d-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="7397d-140">下列範例所示*Web.config*檔案中使用`defaultproxy`設定用來啟用 ReCaptcha 服務運作的項目。</span><span class="sxs-lookup"><span data-stu-id="7397d-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7397d-141">其他資源</span><span class="sxs-lookup"><span data-stu-id="7397d-141">Additional Resources</span></span>


- [<span data-ttu-id="7397d-142">針對 ASP.NET Web Pages 網站中自訂全網站行為</span><span class="sxs-lookup"><span data-stu-id="7397d-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="7397d-143">ReCaptcha 站台</span><span class="sxs-lookup"><span data-stu-id="7397d-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
