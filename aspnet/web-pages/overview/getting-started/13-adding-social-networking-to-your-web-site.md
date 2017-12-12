---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: "社交網路加入 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本章節說明如何將您的網站與社交網路服務的整合。 在本章中，您將學習如何讓人書籤連結您的網站..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2c43fa7d286e43f3a4581662ce421c7435e1871f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>加入社交網路的 ASP.NET Web Pages (Razor) 站台
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何將社交網路連結的 Facebook、 Twitter、 Reddit 和 Digg 新增至網頁的 ASP.NET Web Pages (Razor) 網站，以及如何將加入 Twitter 摘要、 Xbox 玩家卡和 Gravatar 映像。
> 
> 您將學習：
> 
> - 如何讓人員書籤連結您的網站。
> - 如何新增 Twitter 摘要。
> - 如何新增 Facebook**像**頁面的按鈕。
> - 如何呈現 Gravatar.com 影像。
> - 如何在您的網站上顯示 Xbox 玩家卡。
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
## <a name="linking-your-website-on-social-networking-sites"></a>連結您的網站上社交網路網站

如果人喜歡在網站上的某些項目，它們通常會想要與朋友分享。 您可以輕易這藉由顯示圖像 （圖示） 的人可以按一下來共用 Digg、 Reddit、 Facebook、 Twitter 或類似的網站上的頁面。

若要顯示這些字符，加入`LinkSharecode`協助程式 」 的頁面。 請瀏覽網頁的人可以按一下個別的圖像。 如果他們有具有該社交網路網站的帳戶，他們就可以該站台上，然後張貼網頁的連結。

![圖片 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。-建立名為的頁面*ListLinkShare.cshtml*並加入下列標記：

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    在此範例中，當`LinkShare`協助程式執行時，頁面標題會傳遞做為參數，接著再將傳遞至社交網路網站的頁面標題。 不過，您可以傳入您想要的任何字串。 此範例也會指定要包含在清單中的社交網站。 您可以指定與您的網站相關的社交網路網站。
- 執行*ListLinkShare.cshtml*瀏覽器中的。 (請確定中選取頁面**檔案**才能執行這個工作區。)
- 按一下其中一個已註冊的站台的圖像。 此連結會帶您到頁面上選取的社交網路網站，您可以共用連結。 例如，如果您按一下 Reddit 連結，則前往`submit to reddit`Reddit 網站頁面。

    ![圖片 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>加入 Twitter 摘要

使用 Twitter API 的目前版本相容的 Twitter helper 的相關資訊，請參閱[Twitter helper](../ui-layouts-and-themes/twitter-helper.md)。 這個範例示範如何讓您輕鬆地重複使用許多網頁的程式碼撰寫您自己的 helper。

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>顯示 Facebook&quot;像&quot;按鈕

在某些情況下，您最好的選擇是直接從社交網路提供者，而不是依賴協助程式取得程式碼。 特別是如果社交網路提供者會更新它的選項更快速地比協助專家會更新。

若要將 Facebook 功能 （例如 「 讚 」 按鈕） 加入至您的網站，您可以擷取從程式碼片段[developers.facebook.com](https://developers.facebook.com/)站台。 Facebook 站台上中，您可以使用他們的工具來產生與您的網站相關的程式碼片段。

下列反白顯示的程式碼是擷取自像按鈕上的工具 developers.facebook.com 站台的程式碼。 您必須提供您自己的應用程式識別碼。

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>轉譯 Gravatar 映像

A *Gravatar* (&quot;全域可辨識的虛擬人偶&quot;) 映像，可以用於多個網站作為您的顯示圖片 &#8212; 也就是說，代表您的影像。 比方說，Gravatar 可識別論壇文章、 部落格的註解中的人員，依此類推。 (您可以註冊自己 Gravatar Gravatar 網站在[http://www.gravatar.com/](http://www.gravatar.com/)。)如果您想要在您的網站上顯示人的名稱或電子郵件地址旁邊的映像，您可以使用 Gravatar 協助程式。

在此範例中，您使用單一的 Gravatar 表示自己。 另一種使用 Gravatar 是可讓您指定其 Gravatar 位址當他們註冊您的網站上。 (您可以了解如何讓使用者在註冊[加入安全性和 ASP.NET Web Pages 站台中的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。)然後當您顯示該使用者的資訊，您可以加入 Gravatar 您用來顯示使用者的名稱。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新網頁*Gravatar.cshtml*。
3. 將下列標記加入至檔案： 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml`方法在網頁上顯示 Gravatar 映像。 若要變更影像的大小，您可以包含數字做為第二個參數。 預設大小為 80。 數字小於 80 製作映像小。 大於 80 設定映像更大的數字。
4. 在`Gravatar.GetHtml`方法，會取代`<Your Gravatar account here>`Gravatar 帳戶電子郵件地址。 （如果您沒有 Gravatar 帳戶，您可以使用沒有的任何人的電子郵件地址）。
5. 執行網頁瀏覽器中。 頁面會顯示您所指定的電子郵件地址的兩個 Gravatar 映像。 第二個映像會小於第一個項目。 

    ![圖片 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>顯示 Xbox 玩家卡

當使用者播放 Microsoft Xbox 線上遊戲時，每個使用者擁有的唯一識別碼。 統計資料會保存每個玩家玩家卡中，以顯示其信譽、 玩家的分數，以及最近播放的遊戲的形式。 如果您的 Xbox 玩家，您可以透過使用網站的頁面上顯示玩家卡片`GamerCard`協助程式。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*XboxGamer.cshtml*並加入下列標記。

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    您使用`GamerCard.GetHtml`屬性來指定要顯示玩家卡片的別名。
3. 執行網頁瀏覽器中。 此頁面會顯示您所指定的 Xbox 玩家卡。

    ![圖片 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
