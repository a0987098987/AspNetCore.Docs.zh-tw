---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: 將社交網路新增至 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本章說明如何整合您的網站與社交網路服務。 在本章中，您將了解如何讓他人的書籤連結您的網站...
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: e50a35d9770da247d18bbe1b3660b7bd5d46d8e9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822440"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>加入社交網路的 ASP.NET Web Pages (Razor) 網站
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何將 Facebook、 Twitter、 Reddit 及 Digg 的社交網路的連結新增至頁面在 ASP.NET Web Pages (Razor) 網站中，以及如何包括 Twitter 摘要、 Xbox 玩家卡，以及 Gravatar 影像。
> 
> 您將學到什麼：
> 
> - 如何讓他人的書籤連結您的網站。
> - 如何新增 Twitter 摘要。
> - 如何新增 Facebook**像**頁面的按鈕。
> - 如何呈現 Gravatar.com 映像。
> - 如何在您的網站上顯示的 Xbox 的玩家卡。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web 協助程式程式庫 （NuGet 套件）
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 3，除了使用 ASP.NET Web 協助程式程式庫的組件。


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>連結您的網站在社交網路網站

如果人喜歡您的網站上的內容，他們通常會想要與朋友分享。 您可以藉此輕鬆顯示使用者可以按一下 共用 Digg、 Reddit、 Facebook、 Twitter 或類似的網站上的網頁的字符 （圖示）。

若要顯示這些字符，加入`LinkSharecode`協助程式 」 頁面。 請瀏覽您的網頁的人可以按一下個別的字符。 如果他們有具有該社交網路網站的帳戶，他們就可以在該網站上，然後張貼您頁面的連結。

![圖 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有新增它。-建立名為頁面*ListLinkShare.cshtml*並新增下列標記：

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    在此範例中，當`LinkShare`協助程式執行時，網頁的標題會傳遞做為參數，接著再將傳遞到社交網路網站的網頁標題。 不過，您可以傳入任何您想要的字串。 此範例也會指定要包含在清單中的社交網站。 您可以指定與您的站台的社交網路網站。
2. 執行*ListLinkShare.cshtml*瀏覽器中的頁面。 (請確定中選取頁面**檔案**才能執行這個工作區。)
3. 按一下您要報名的網站的圖像。 此連結會帶您頁面上選取的社交網站，您可以共用連結。 例如，如果您按一下 Reddit 連結時，就會前往`submit to reddit`Reddit 網站上的頁面。

     ![圖 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>新增 Twitter 摘要

使用 Twitter API 目前版本相容的 Twitter helper 的相關資訊，請參閱[Twitter helper](../ui-layouts-and-themes/twitter-helper.md)。 此範例示範如何撰寫您自己的協助程式，所以您可以輕鬆地重複使用許多頁面的程式碼。

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>顯示 Facebook&quot;像&quot;按鈕

在某些情況下，最佳選項是直接從社交網路提供者，而不是依賴協助程式取得程式碼。 特別是如果社交網路提供者與協助程式會更新快速更新它的選項。

若要新增 Facebook 功能 （例如 「 讚 」 按鈕） 到您的網站，您可以擷取從程式碼片段[developers.facebook.com](https://developers.facebook.com/)站台。 Facebook 在網站上中,，您可以使用他們的工具來產生與您的網站相關的程式碼片段。

下列醒目提示的程式碼是從 developers.facebook.com 站台上像按鈕工具已擷取的程式碼。 您必須提供您自己的應用程式識別碼。

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>轉譯 Gravatar 影像

A *Gravatar* (&quot;全域可辨識的虛擬人偶&quot;) 是可用來在多個網站當做 avatar 影像&#8212;即表示您的映像。 比方說，Gravatar 可以識別中的論壇文章、 部落格註解中的人員等等。 (您可以註冊 Gravatar 網站在您自己 Gravatar [ http://www.gravatar.com/ ](http://www.gravatar.com/)。)如果您想要在網站上顯示人員的名稱或電子郵件地址旁的映像，您可以使用 Gravatar 協助程式。

在此範例中，您使用單一的 Gravatar 表示自己。 使用 Gravatar 的另一個方法是讓您的網站上註冊時指定其 Gravatar 位址的人員。 (您可以了解如何讓使用者在註冊[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。)然後每當您顯示該使用者的資訊，您就可以只新增 Gravatar，在您用來顯示使用者的名稱。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新網頁*Gravatar.cshtml*。
3. 檔案中加入下列標記： 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml`方法在網頁上顯示 Gravatar 影像。 若要變更影像的大小，您可以包含數字做為第二個參數。 預設大小為 80。 數字小於 80 的製作映像小。 大於 80 設定映像更大的數字。
4. 在 `Gravatar.GetHtml`方法，會取代`<Your Gravatar account here>`Gravatar 帳戶電子郵件地址。 （如果您還沒有 Gravatar 帳戶，您可以使用人員沒有電子郵件地的址）。
5. 執行網頁瀏覽器中。 此頁面會顯示您所指定的電子郵件地址的兩個 Gravatar 影像。 第二個影像會小於第一個項目。 

    ![圖 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>顯示 Xbox 玩家卡

當人們在線上遊戲時，都會播放 Microsoft Xbox 時，每個使用者都是唯一的識別碼。 統計資料會保留每個播放程式會顯示他們的評價、 玩家的分數，並在最近播放遊戲的玩家卡片的形式。 如果您是 Xbox 玩家，您可以使用在您的網站 頁面上顯示您的玩家卡`GamerCard`協助程式。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*XboxGamer.cshtml*並新增下列標記。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    您使用`GamerCard.GetHtml`屬性來指定要顯示的玩家卡片的別名。
3. 執行網頁瀏覽器中。 此頁面會顯示您所指定的 Xbox 玩家卡。

    ![圖 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
