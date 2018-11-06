---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: 將社交網路新增至 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: Rick-Anderson
description: 本章說明如何整合您的網站與社交網路服務。 在本章中，您將了解如何讓他人的書籤連結您的網站...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: d2b970e7b80e4129d0a912f648f9c4a54df531b2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021153"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="f9dbe-104">加入社交網路的 ASP.NET Web Pages (Razor) 網站</span><span class="sxs-lookup"><span data-stu-id="f9dbe-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="f9dbe-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f9dbe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f9dbe-106">這篇文章說明如何將 Facebook、 Twitter、 Reddit 及 Digg 的社交網路的連結新增至頁面在 ASP.NET Web Pages (Razor) 網站中，以及如何包括 Twitter 摘要、 Xbox 玩家卡，以及 Gravatar 影像。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="f9dbe-107">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="f9dbe-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f9dbe-108">如何讓他人的書籤連結您的網站。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="f9dbe-109">如何新增 Twitter 摘要。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="f9dbe-110">如何新增 Facebook**像**頁面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="f9dbe-111">如何呈現 Gravatar.com 映像。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="f9dbe-112">如何在您的網站上顯示的 Xbox 的玩家卡。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f9dbe-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="f9dbe-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f9dbe-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f9dbe-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f9dbe-115">ASP.NET Web 協助程式程式庫 （NuGet 套件）</span><span class="sxs-lookup"><span data-stu-id="f9dbe-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="f9dbe-116">本教學課程也適用於 ASP.NET Web Pages 3，除了使用 ASP.NET Web 協助程式程式庫的組件。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="f9dbe-117">連結您的網站在社交網路網站</span><span class="sxs-lookup"><span data-stu-id="f9dbe-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="f9dbe-118">如果人喜歡您的網站上的內容，他們通常會想要與朋友分享。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="f9dbe-119">您可以藉此輕鬆顯示使用者可以按一下 共用 Digg、 Reddit、 Facebook、 Twitter 或類似的網站上的網頁的字符 （圖示）。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="f9dbe-120">若要顯示這些字符，加入`LinkSharecode`協助程式 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="f9dbe-121">請瀏覽您的網頁的人可以按一下個別的字符。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="f9dbe-122">如果他們有具有該社交網路網站的帳戶，他們就可以在該網站上，然後張貼您頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![圖 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="f9dbe-124">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有新增它。-建立名為頁面*ListLinkShare.cshtml*並新增下列標記：</span><span class="sxs-lookup"><span data-stu-id="f9dbe-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="f9dbe-125">在此範例中，當`LinkShare`協助程式執行時，網頁的標題會傳遞做為參數，接著再將傳遞到社交網路網站的網頁標題。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="f9dbe-126">不過，您可以傳入任何您想要的字串。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="f9dbe-127">此範例也會指定要包含在清單中的社交網站。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="f9dbe-128">您可以指定與您的站台的社交網路網站。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="f9dbe-129">執行*ListLinkShare.cshtml*瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="f9dbe-130">(請確定中選取頁面**檔案**才能執行這個工作區。)</span><span class="sxs-lookup"><span data-stu-id="f9dbe-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="f9dbe-131">按一下您要報名的網站的圖像。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="f9dbe-132">此連結會帶您頁面上選取的社交網站，您可以共用連結。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="f9dbe-133">例如，如果您按一下 Reddit 連結時，就會前往`submit to reddit`Reddit 網站上的頁面。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![圖 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="f9dbe-135">新增 Twitter 摘要</span><span class="sxs-lookup"><span data-stu-id="f9dbe-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="f9dbe-136">使用 Twitter API 目前版本相容的 Twitter helper 的相關資訊，請參閱[Twitter helper](../ui-layouts-and-themes/twitter-helper.md)。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="f9dbe-137">此範例示範如何撰寫您自己的協助程式，所以您可以輕鬆地重複使用許多頁面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="f9dbe-138">顯示 Facebook&quot;像&quot;按鈕</span><span class="sxs-lookup"><span data-stu-id="f9dbe-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="f9dbe-139">在某些情況下，最佳選項是直接從社交網路提供者，而不是依賴協助程式取得程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="f9dbe-140">特別是如果社交網路提供者與協助程式會更新快速更新它的選項。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="f9dbe-141">若要新增 Facebook 功能 （例如 「 讚 」 按鈕） 到您的網站，您可以擷取從程式碼片段[developers.facebook.com](https://developers.facebook.com/)站台。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="f9dbe-142">Facebook 在網站上中,，您可以使用他們的工具來產生與您的網站相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="f9dbe-143">下列醒目提示的程式碼是從 developers.facebook.com 站台上像按鈕工具已擷取的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="f9dbe-144">您必須提供您自己的應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="f9dbe-145">轉譯 Gravatar 影像</span><span class="sxs-lookup"><span data-stu-id="f9dbe-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="f9dbe-146">A *Gravatar* (&quot;全域可辨識的虛擬人偶&quot;) 是可用來在多個網站當做 avatar 影像&#8212;即表示您的映像。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="f9dbe-147">比方說，Gravatar 可以識別中的論壇文章、 部落格註解中的人員等等。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="f9dbe-148">(您可以註冊 Gravatar 網站在您自己 Gravatar [ http://www.gravatar.com/ ](http://www.gravatar.com/)。)如果您想要在網站上顯示人員的名稱或電子郵件地址旁的映像，您可以使用 Gravatar 協助程式。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="f9dbe-149">在此範例中，您使用單一的 Gravatar 表示自己。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="f9dbe-150">使用 Gravatar 的另一個方法是讓您的網站上註冊時指定其 Gravatar 位址的人員。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="f9dbe-151">(您可以了解如何讓使用者在註冊[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。)然後每當您顯示該使用者的資訊，您就可以只新增 Gravatar，在您用來顯示使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="f9dbe-152">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="f9dbe-153">建立名為的新網頁*Gravatar.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="f9dbe-154">檔案中加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="f9dbe-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="f9dbe-155">`Gravatar.GetHtml`方法在網頁上顯示 Gravatar 影像。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="f9dbe-156">若要變更影像的大小，您可以包含數字做為第二個參數。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="f9dbe-157">預設大小為 80。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-157">The default size is 80.</span></span> <span data-ttu-id="f9dbe-158">數字小於 80 的製作映像小。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="f9dbe-159">大於 80 設定映像更大的數字。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="f9dbe-160">在 `Gravatar.GetHtml`方法，會取代`<Your Gravatar account here>`Gravatar 帳戶電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="f9dbe-161">（如果您還沒有 Gravatar 帳戶，您可以使用人員沒有電子郵件地的址）。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="f9dbe-162">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-162">Run the page in your browser.</span></span> <span data-ttu-id="f9dbe-163">此頁面會顯示您所指定的電子郵件地址的兩個 Gravatar 影像。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="f9dbe-164">第二個影像會小於第一個項目。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-164">The second image is smaller than the first.</span></span> 

    ![圖 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="f9dbe-166">顯示 Xbox 玩家卡</span><span class="sxs-lookup"><span data-stu-id="f9dbe-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="f9dbe-167">當人們在線上遊戲時，都會播放 Microsoft Xbox 時，每個使用者都是唯一的識別碼。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="f9dbe-168">統計資料會保留每個播放程式會顯示他們的評價、 玩家的分數，並在最近播放遊戲的玩家卡片的形式。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="f9dbe-169">如果您是 Xbox 玩家，您可以使用在您的網站 頁面上顯示您的玩家卡`GamerCard`協助程式。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="f9dbe-170">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="f9dbe-171">建立名為的新頁面*XboxGamer.cshtml*並新增下列標記。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="f9dbe-172">您使用`GamerCard.GetHtml`屬性來指定要顯示的玩家卡片的別名。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="f9dbe-173">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-173">Run the page in your browser.</span></span> <span data-ttu-id="f9dbe-174">此頁面會顯示您所指定的 Xbox 玩家卡。</span><span class="sxs-lookup"><span data-stu-id="f9dbe-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![圖 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
