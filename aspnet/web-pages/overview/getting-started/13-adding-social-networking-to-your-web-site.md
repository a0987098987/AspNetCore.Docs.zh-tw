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
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="24878-104">加入社交網路的 ASP.NET Web Pages (Razor) 站台</span><span class="sxs-lookup"><span data-stu-id="24878-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="24878-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24878-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24878-106">本文說明如何將社交網路連結的 Facebook、 Twitter、 Reddit 和 Digg 新增至網頁的 ASP.NET Web Pages (Razor) 網站，以及如何將加入 Twitter 摘要、 Xbox 玩家卡和 Gravatar 映像。</span><span class="sxs-lookup"><span data-stu-id="24878-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="24878-107">您將學習：</span><span class="sxs-lookup"><span data-stu-id="24878-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="24878-108">如何讓人員書籤連結您的網站。</span><span class="sxs-lookup"><span data-stu-id="24878-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="24878-109">如何新增 Twitter 摘要。</span><span class="sxs-lookup"><span data-stu-id="24878-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="24878-110">如何新增 Facebook**像**頁面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="24878-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="24878-111">如何呈現 Gravatar.com 影像。</span><span class="sxs-lookup"><span data-stu-id="24878-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="24878-112">如何在您的網站上顯示 Xbox 玩家卡。</span><span class="sxs-lookup"><span data-stu-id="24878-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="24878-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="24878-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="24878-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="24878-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="24878-115">ASP.NET Web 協助程式程式庫 （NuGet 套件）</span><span class="sxs-lookup"><span data-stu-id="24878-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="24878-116">本教學課程也適用於 ASP.NET Web Pages 3，除了使用 ASP.NET Web 協助程式程式庫的組件。</span><span class="sxs-lookup"><span data-stu-id="24878-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="24878-117">連結您的網站上社交網路網站</span><span class="sxs-lookup"><span data-stu-id="24878-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="24878-118">如果人喜歡在網站上的某些項目，它們通常會想要與朋友分享。</span><span class="sxs-lookup"><span data-stu-id="24878-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="24878-119">您可以輕易這藉由顯示圖像 （圖示） 的人可以按一下來共用 Digg、 Reddit、 Facebook、 Twitter 或類似的網站上的頁面。</span><span class="sxs-lookup"><span data-stu-id="24878-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="24878-120">若要顯示這些字符，加入`LinkSharecode`協助程式 」 的頁面。</span><span class="sxs-lookup"><span data-stu-id="24878-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="24878-121">請瀏覽網頁的人可以按一下個別的圖像。</span><span class="sxs-lookup"><span data-stu-id="24878-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="24878-122">如果他們有具有該社交網路網站的帳戶，他們就可以該站台上，然後張貼網頁的連結。</span><span class="sxs-lookup"><span data-stu-id="24878-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![圖片 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="24878-124">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。-建立名為的頁面*ListLinkShare.cshtml*並加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="24878-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="24878-125">在此範例中，當`LinkShare`協助程式執行時，頁面標題會傳遞做為參數，接著再將傳遞至社交網路網站的頁面標題。</span><span class="sxs-lookup"><span data-stu-id="24878-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="24878-126">不過，您可以傳入您想要的任何字串。</span><span class="sxs-lookup"><span data-stu-id="24878-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="24878-127">此範例也會指定要包含在清單中的社交網站。</span><span class="sxs-lookup"><span data-stu-id="24878-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="24878-128">您可以指定與您的網站相關的社交網路網站。</span><span class="sxs-lookup"><span data-stu-id="24878-128">You can specify the social networking sites that are relevant to your site.</span></span>
- <span data-ttu-id="24878-129">執行*ListLinkShare.cshtml*瀏覽器中的。</span><span class="sxs-lookup"><span data-stu-id="24878-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="24878-130">(請確定中選取頁面**檔案**才能執行這個工作區。)</span><span class="sxs-lookup"><span data-stu-id="24878-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
- <span data-ttu-id="24878-131">按一下其中一個已註冊的站台的圖像。</span><span class="sxs-lookup"><span data-stu-id="24878-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="24878-132">此連結會帶您到頁面上選取的社交網路網站，您可以共用連結。</span><span class="sxs-lookup"><span data-stu-id="24878-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="24878-133">例如，如果您按一下 Reddit 連結，則前往`submit to reddit`Reddit 網站頁面。</span><span class="sxs-lookup"><span data-stu-id="24878-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

    ![圖片 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="24878-135">加入 Twitter 摘要</span><span class="sxs-lookup"><span data-stu-id="24878-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="24878-136">使用 Twitter API 的目前版本相容的 Twitter helper 的相關資訊，請參閱[Twitter helper](../ui-layouts-and-themes/twitter-helper.md)。</span><span class="sxs-lookup"><span data-stu-id="24878-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="24878-137">這個範例示範如何讓您輕鬆地重複使用許多網頁的程式碼撰寫您自己的 helper。</span><span class="sxs-lookup"><span data-stu-id="24878-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="24878-138">顯示 Facebook&quot;像&quot;按鈕</span><span class="sxs-lookup"><span data-stu-id="24878-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="24878-139">在某些情況下，您最好的選擇是直接從社交網路提供者，而不是依賴協助程式取得程式碼。</span><span class="sxs-lookup"><span data-stu-id="24878-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="24878-140">特別是如果社交網路提供者會更新它的選項更快速地比協助專家會更新。</span><span class="sxs-lookup"><span data-stu-id="24878-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="24878-141">若要將 Facebook 功能 （例如 「 讚 」 按鈕） 加入至您的網站，您可以擷取從程式碼片段[developers.facebook.com](https://developers.facebook.com/)站台。</span><span class="sxs-lookup"><span data-stu-id="24878-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="24878-142">Facebook 站台上中，您可以使用他們的工具來產生與您的網站相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="24878-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="24878-143">下列反白顯示的程式碼是擷取自像按鈕上的工具 developers.facebook.com 站台的程式碼。</span><span class="sxs-lookup"><span data-stu-id="24878-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="24878-144">您必須提供您自己的應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="24878-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="24878-145">轉譯 Gravatar 映像</span><span class="sxs-lookup"><span data-stu-id="24878-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="24878-146">A *Gravatar* (&quot;全域可辨識的虛擬人偶&quot;) 映像，可以用於多個網站作為您的顯示圖片 &#8212; 也就是說，代表您的影像。</span><span class="sxs-lookup"><span data-stu-id="24878-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="24878-147">比方說，Gravatar 可識別論壇文章、 部落格的註解中的人員，依此類推。</span><span class="sxs-lookup"><span data-stu-id="24878-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="24878-148">(您可以註冊自己 Gravatar Gravatar 網站在[http://www.gravatar.com/](http://www.gravatar.com/)。)如果您想要在您的網站上顯示人的名稱或電子郵件地址旁邊的映像，您可以使用 Gravatar 協助程式。</span><span class="sxs-lookup"><span data-stu-id="24878-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="24878-149">在此範例中，您使用單一的 Gravatar 表示自己。</span><span class="sxs-lookup"><span data-stu-id="24878-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="24878-150">另一種使用 Gravatar 是可讓您指定其 Gravatar 位址當他們註冊您的網站上。</span><span class="sxs-lookup"><span data-stu-id="24878-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="24878-151">(您可以了解如何讓使用者在註冊[加入安全性和 ASP.NET Web Pages 站台中的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。)然後當您顯示該使用者的資訊，您可以加入 Gravatar 您用來顯示使用者的名稱。</span><span class="sxs-lookup"><span data-stu-id="24878-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="24878-152">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="24878-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="24878-153">建立名為的新網頁*Gravatar.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="24878-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="24878-154">將下列標記加入至檔案：</span><span class="sxs-lookup"><span data-stu-id="24878-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="24878-155">`Gravatar.GetHtml`方法在網頁上顯示 Gravatar 映像。</span><span class="sxs-lookup"><span data-stu-id="24878-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="24878-156">若要變更影像的大小，您可以包含數字做為第二個參數。</span><span class="sxs-lookup"><span data-stu-id="24878-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="24878-157">預設大小為 80。</span><span class="sxs-lookup"><span data-stu-id="24878-157">The default size is 80.</span></span> <span data-ttu-id="24878-158">數字小於 80 製作映像小。</span><span class="sxs-lookup"><span data-stu-id="24878-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="24878-159">大於 80 設定映像更大的數字。</span><span class="sxs-lookup"><span data-stu-id="24878-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="24878-160">在`Gravatar.GetHtml`方法，會取代`<Your Gravatar account here>`Gravatar 帳戶電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="24878-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="24878-161">（如果您沒有 Gravatar 帳戶，您可以使用沒有的任何人的電子郵件地址）。</span><span class="sxs-lookup"><span data-stu-id="24878-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="24878-162">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="24878-162">Run the page in your browser.</span></span> <span data-ttu-id="24878-163">頁面會顯示您所指定的電子郵件地址的兩個 Gravatar 映像。</span><span class="sxs-lookup"><span data-stu-id="24878-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="24878-164">第二個映像會小於第一個項目。</span><span class="sxs-lookup"><span data-stu-id="24878-164">The second image is smaller than the first.</span></span> 

    ![圖片 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="24878-166">顯示 Xbox 玩家卡</span><span class="sxs-lookup"><span data-stu-id="24878-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="24878-167">當使用者播放 Microsoft Xbox 線上遊戲時，每個使用者擁有的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="24878-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="24878-168">統計資料會保存每個玩家玩家卡中，以顯示其信譽、 玩家的分數，以及最近播放的遊戲的形式。</span><span class="sxs-lookup"><span data-stu-id="24878-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="24878-169">如果您的 Xbox 玩家，您可以透過使用網站的頁面上顯示玩家卡片`GamerCard`協助程式。</span><span class="sxs-lookup"><span data-stu-id="24878-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="24878-170">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="24878-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="24878-171">建立名為的新頁面*XboxGamer.cshtml*並加入下列標記。</span><span class="sxs-lookup"><span data-stu-id="24878-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="24878-172">您使用`GamerCard.GetHtml`屬性來指定要顯示玩家卡片的別名。</span><span class="sxs-lookup"><span data-stu-id="24878-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="24878-173">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="24878-173">Run the page in your browser.</span></span> <span data-ttu-id="24878-174">此頁面會顯示您所指定的 Xbox 玩家卡。</span><span class="sxs-lookup"><span data-stu-id="24878-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![圖片 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
