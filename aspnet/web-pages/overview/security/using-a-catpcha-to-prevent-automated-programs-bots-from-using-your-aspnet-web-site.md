---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: "若要防止 Bot 使用 ASP.NET Web Razor 使用 CAPTCHA） 的站台 |Microsoft 文件"
author: microsoft
description: "這篇文章說明如何使用 ReCaptcha （安全性量值），以防止自動的程式 (bot) 執行工作中的 ASP.NET Web Pages (Razor) 我們..."
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
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>若要防止 Bot 使用 ASP.NET Web Razor 使用 CAPTCHA） 的站台
====================
由[Microsoft](https://github.com/microsoft)

> 本文說明如何使用 ReCaptcha （安全性量值），以防止自動的程式 (bot) 在 ASP.NET Web Pages (Razor) 網站中執行工作。
> 
> **您將學習：** 
> 
> - 如何將 CAPTCHA 測試加入至您的網站。
> 
> 這些是發行項中所引進的 ASP.NET 功能：
> 
> - `ReCaptcha`協助程式。
> 
> > [!NOTE]
> > 本主題資訊適用於 ASP.NET Web Pages 1.0 和 Web Pages 2。


## <a name="about-captchas"></a>關於 CAPTCHAs

每當您讓註冊的人員，在您的網站，或甚至只輸入名稱和 URL （類似的部落格的註解），您可能會收到一大堆假的名稱。 這些通常是左嘗試保留 Url 中可以找到每個網站的自動程式 (bot)。 （常見的動機是要張貼的產品銷售的 Url）。

您可以協助確定使用者使用真人，而不是電腦的程式是*CAPTCHA*來註冊或否則輸入其名稱和站台時，驗證使用者。 CAPTCHA 代表完全自動化公用 Turing 測試，以告訴電腦，以及讓人了距離。 CAPTCHA 是*挑戰-回應*要求執行某個動作的使用者容易供使用者執行，但難以自動化程式來執行的測試。 最常見的 CAPTCHA 類型則是您看到某些扭曲的代號並要求您輸入它們。 （扭曲程度應該讓進行破解字母機器人。）

## <a name="adding-a-recaptcha-test"></a>新增 ReCaptcha 測試

在 ASP.NET 網頁中，您可以使用`ReCaptcha`helper 來呈現 ReCaptcha 服務為基礎的 CAPTCHA 測試 ([http://recaptcha.net](http://recaptcha.net))。 `ReCaptcha` Helper 會顯示兩個使用者需要驗證頁面之前，請輸入正確的扭曲單字的映像。 服務驗證使用者回應。

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. 註冊您的網站，在 ([http://recaptcha.net](http://recaptcha.net))。 當您已經完成註冊時，您會取得公開金鑰和私密金鑰。
2. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
3. 如果您還沒有 *\_AppStart.cshtml*檔案中，網站的根資料夾中建立名為 *\_AppStart.cshtml*。
4. 加入下列`Recaptcha`協助程式設定中的 *\_AppStart.cshtml*檔案： 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. 設定`PublicKey`和`PrivateKey`使用您自己的公用和私用金鑰的屬性。
6. 儲存 *\_AppStart.cshtml*檔案並將它關閉。
7. 在網站的根資料夾中，建立名為新頁面*Recaptcha.cshtml*。
8. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. 執行*Recaptcha.cshtml*瀏覽器中的。 如果`PrivateKey`值是否有效，頁面會顯示 ReCaptcha 控制項和按鈕。 如果您必須在全域設定索引鍵 *\_AppStart.html*，頁面會顯示錯誤訊息。 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. 測試輸入的字。 如果您傳遞 ReCaptcha 測試，您會看到一則訊息。 否則您會看到一則錯誤訊息和 ReCaptcha 控制項重新顯示。

> [!NOTE]
> 如果您的電腦上使用 proxy 伺服器的網域，您可能需要設定`defaultproxy`元素*Web.config*檔案。 下列範例所示*Web.config*檔案搭配`defaultproxy`設定為啟用 ReCaptcha 服務運作的項目。
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [ASP.NET Web Pages 站台中的自訂全站台的行為](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha 站台](https://www.google.com/recaptcha)
