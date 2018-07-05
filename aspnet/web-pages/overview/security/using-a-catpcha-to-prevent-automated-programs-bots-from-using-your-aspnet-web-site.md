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
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>使用 CAPTCHA 避免 Bot 使用 ASP.NET Web Razor） 網站
====================
by [Microsoft](https://github.com/microsoft)

> 這篇文章說明如何在 ASP.NET Web Pages (Razor) 網站中執行工作時，防止自動的程式 (bot) 使用 ReCaptcha （安全性量值）。
> 
> **您將學到什麼：** 
> 
> - 如何將 CAPTCHA 測試新增至您的網站。
> 
> 以下是所引進的發行項 ASP.NET 功能：
> 
> - `ReCaptcha`協助程式。
> 
> > [!NOTE]
> > 這篇文章中的資訊適用於 ASP.NET Web Pages 1.0 和 Web Pages 2。


## <a name="about-captchas"></a>關於 CAPTCHAs

在您的網站，或甚至只是可讓使用者註冊任何時間輸入的名稱和 URL （如部落格註解），您可能會收到一大堆假的名稱。 這些通常是左嘗試保留 Url，讓他們能夠找到每個網站中的自動化程式 (bot)。 （常見的動機是要張貼的產品銷售的 Url）。

請確定使用者使用的是真人，而不是電腦程式，您可以協助*CAPTCHA*來驗證使用者，當使用者註冊或否則輸入 使用者名稱和站台。 CAPTCHA 代表完全自動化公用 Turing 測試，以告訴電腦，以及讓人判讀分開。 是 CAPTCHA*挑戰-回應*測試執行一些作業會要求使用者執行個人的簡單但硬碟做為自動化程式。 最常見的 CAPTCHA 的類型是其中看到某些異常狀況的字母，而系統會要求輸入密碼。 （扭曲應該得相當困難的 bot 將解碼的字母。）

## <a name="adding-a-recaptcha-test"></a>加入 ReCaptcha 測試

在 ASP.NET 網頁中，您可以使用`ReCaptcha`呈現 ReCaptcha 服務為基礎的 CAPTCHA 測試協助程式 ([http://recaptcha.net](http://recaptcha.net))。 `ReCaptcha`協助程式會顯示兩個使用者需要驗證頁面之前，請輸入正確的扭曲單字的映像。 ReCaptcha.Net 服務驗證的使用者回應。

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. 註冊您的網站在 ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net))。 當您完成註冊之後時，您會取得公開金鑰和私密金鑰。
2. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
3. 如果您還沒有 *\_AppStart.cshtml*檔案中，網站的根資料夾中建立名為 *\_AppStart.cshtml*。
4. 新增下列`Recaptcha`中的協助程式設定 *\_AppStart.cshtml*檔案： 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. 設定`PublicKey`和`PrivateKey`使用您自己的公用和私用金鑰的屬性。
6. 儲存 *\_AppStart.cshtml*檔案，並將它關閉。
7. 在根資料夾中的網站，建立名為的新頁面*Recaptcha.cshtml*。
8. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. 執行*Recaptcha.cshtml*瀏覽器中的頁面。 如果`PrivateKey`值是否有效，此頁面會顯示 ReCaptcha 控制項和按鈕。 如果您必須在全域設定索引鍵 *\_AppStart.html*，頁面會顯示錯誤。 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. 測試輸入的文字。 如果您傳遞 ReCaptcha 測試時，您會看到一則訊息。 否則您會看到一則錯誤訊息，並重新顯示 ReCaptcha 控制項。

> [!NOTE]
> 如果您的電腦上使用 proxy 伺服器的網域，您可能需要設定`defaultproxy`項目*Web.config*檔案。 下列範例所示*Web.config*檔案中使用`defaultproxy`設定用來啟用 ReCaptcha 服務運作的項目。
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [針對 ASP.NET Web Pages 網站中自訂全網站行為](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha 站台](https://www.google.com/recaptcha)
