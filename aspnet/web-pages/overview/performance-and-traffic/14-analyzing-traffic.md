---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: 追蹤 ASP.NET Web Pages (Razor) 網站訪客資訊 （分析） |Microsoft Docs
author: tfitzmac
description: 您覺得您要的網站之後，您可能想要分析網站流量。
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: aabe3177ba9479bfafafe81e1ea99a58f29d5271
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831837"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="182fd-103">追蹤 ASP.NET Web Pages (Razor) 網站的造訪者資訊 （分析）</span><span class="sxs-lookup"><span data-stu-id="182fd-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="182fd-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="182fd-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="182fd-105">本文說明如何使用協助程式，在 ASP.NET Web Pages (Razor) 網站中的頁面加入網站分析。</span><span class="sxs-lookup"><span data-stu-id="182fd-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="182fd-106">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="182fd-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="182fd-107">如何將您的網站流量的相關資訊傳送給分析提供者。</span><span class="sxs-lookup"><span data-stu-id="182fd-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="182fd-108">這些是 ASP.NET 程式設計文章中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="182fd-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="182fd-109">`Analytics`協助程式。</span><span class="sxs-lookup"><span data-stu-id="182fd-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="182fd-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="182fd-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="182fd-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="182fd-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="182fd-112">ASP.NET Web Helpers Library （NuGet 套件）</span><span class="sxs-lookup"><span data-stu-id="182fd-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="182fd-113">Analytics 是一個通稱測量您的網站上的流量，因此您可以了解使用者如何使用站台的技術。</span><span class="sxs-lookup"><span data-stu-id="182fd-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="182fd-114">許多分析服務已推出，包括 Google、 Yahoo、 StatCounter，和其他服務。</span><span class="sxs-lookup"><span data-stu-id="182fd-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="182fd-115">想要追蹤的運作方式是，您登入帳戶與分析提供者，讓您註冊站台時，您的方式分析。提供者會傳送包含 ID 或追蹤您的帳戶的程式碼的 JavaScript 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="182fd-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="182fd-116">您新增至您想要追蹤之站台上的網頁的 JavaScript 程式碼片段。（您通常新增分析程式碼片段的頁尾或版面配置頁面或其他網站中的每個頁面出現的 HTML 標記）。當使用者要求網頁包含其中一個這些 JavaScript 程式碼片段時，程式碼片段會將目前頁面的相關資訊傳送分析提供者，會記錄各種詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="182fd-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="182fd-117">當您想要看看您的網站統計資料時，您會登入分析提供者的網站。</span><span class="sxs-lookup"><span data-stu-id="182fd-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="182fd-118">然後，您可以檢視有關您的網站，各種報表類似：</span><span class="sxs-lookup"><span data-stu-id="182fd-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="182fd-119">個別頁面的頁面檢視數目。</span><span class="sxs-lookup"><span data-stu-id="182fd-119">The number of page views for individual pages.</span></span> <span data-ttu-id="182fd-120">這會告訴您 （大約） 有多少人正在瀏覽網站，並在您的網站上的哪些分頁是最受歡迎。</span><span class="sxs-lookup"><span data-stu-id="182fd-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="182fd-121">多久人員花在特定的頁面上。</span><span class="sxs-lookup"><span data-stu-id="182fd-121">How long people spend on specific pages.</span></span> <span data-ttu-id="182fd-122">這會告訴您像是您的首頁上是否讓人的感興趣。</span><span class="sxs-lookup"><span data-stu-id="182fd-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="182fd-123">哪些站台有人上前客戶造訪您的網站。</span><span class="sxs-lookup"><span data-stu-id="182fd-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="182fd-124">這可協助您了解您的流量是否來自連結，從搜尋，依此類推。</span><span class="sxs-lookup"><span data-stu-id="182fd-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="182fd-125">當您的網站和其停留多久，請瀏覽的人員。</span><span class="sxs-lookup"><span data-stu-id="182fd-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="182fd-126">為您的訪客哪些國家/地區。</span><span class="sxs-lookup"><span data-stu-id="182fd-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="182fd-127">哪些瀏覽器與作業系統會使用您的訪客。</span><span class="sxs-lookup"><span data-stu-id="182fd-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="182fd-129">若要將分析加入頁面中使用協助程式</span><span class="sxs-lookup"><span data-stu-id="182fd-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="182fd-130">ASP.NET Web 網頁包含數個分析協助程式 (`Analytics.GetGoogleHtml`， `Analytics.GetYahooHtml`，和`Analytics.GetStatCounterHtml`)，讓您輕鬆管理用於分析的 JavaScript 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="182fd-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="182fd-131">而方式和位置，找出不是把 JavaScript 程式碼中，您只需要只將協助程式新增至頁面。</span><span class="sxs-lookup"><span data-stu-id="182fd-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="182fd-132">您必須提供的唯一資訊是您的帳戶名稱、 識別碼或追蹤程式碼。</span><span class="sxs-lookup"><span data-stu-id="182fd-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="182fd-133">（如 StatCounter，您也必須提供一些其他的值。）</span><span class="sxs-lookup"><span data-stu-id="182fd-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="182fd-134">在此程序中，您將建立會使用的版面配置頁`GetGoogleHtml`協助程式。</span><span class="sxs-lookup"><span data-stu-id="182fd-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="182fd-135">如果您已經有帳戶，與其中一個其他分析提供者，您可以改為使用該帳戶，並視需要進行些微調整。</span><span class="sxs-lookup"><span data-stu-id="182fd-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="182fd-136">當您建立 analytics 帳戶時，您會註冊您想要追蹤之網站的 URL。</span><span class="sxs-lookup"><span data-stu-id="182fd-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="182fd-137">如果您要測試的所有項目在您的本機電腦上，您將不會追蹤實際的資料傳輸 （唯一的流量是您），因此您無法再記錄及檢視網站統計資料。</span><span class="sxs-lookup"><span data-stu-id="182fd-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="182fd-138">但此程序示範如何將分析協助程式新增至頁面。</span><span class="sxs-lookup"><span data-stu-id="182fd-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="182fd-139">當您發行您的網站時，即時網站會將資訊傳送給您的分析提供者。</span><span class="sxs-lookup"><span data-stu-id="182fd-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="182fd-140">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有新增它。</span><span class="sxs-lookup"><span data-stu-id="182fd-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="182fd-141">建立具有 Google Analytics 帳戶，並記下帳戶名稱。</span><span class="sxs-lookup"><span data-stu-id="182fd-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="182fd-142">建立名為版面配置頁*Analytics.cshtml*並新增下列標記：</span><span class="sxs-lookup"><span data-stu-id="182fd-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="182fd-143">您必須將放在呼叫`Analytics`在您的網頁主體中的協助程式 (之前`</body>`標記)。</span><span class="sxs-lookup"><span data-stu-id="182fd-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="182fd-144">否則，瀏覽器不會執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="182fd-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="182fd-145">如果您使用不同的分析提供者，請改用下列協助程式的其中一個：</span><span class="sxs-lookup"><span data-stu-id="182fd-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="182fd-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="182fd-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="182fd-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="182fd-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="182fd-148">取代`myaccount`帳戶、 識別碼或您在步驟 1 中建立的追蹤程式碼的名稱。</span><span class="sxs-lookup"><span data-stu-id="182fd-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="182fd-149">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="182fd-149">Run the page in the browser.</span></span> <span data-ttu-id="182fd-150">(請確定中選取頁面**檔案**才能執行這個工作區。)</span><span class="sxs-lookup"><span data-stu-id="182fd-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="182fd-151">在瀏覽器中檢視網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="182fd-151">In the browser, view the page source.</span></span> <span data-ttu-id="182fd-152">您將能夠看到轉譯的分析程式碼：</span><span class="sxs-lookup"><span data-stu-id="182fd-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="182fd-153">登入 Google Analytics 網站，並檢查您的站台的統計資料。</span><span class="sxs-lookup"><span data-stu-id="182fd-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="182fd-154">如果您即時站台上執行的頁面，您會看到記錄到您的網頁瀏覽的項目。</span><span class="sxs-lookup"><span data-stu-id="182fd-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="182fd-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="182fd-155">Additional Resources</span></span>

- [<span data-ttu-id="182fd-156">Google Analytics 網站</span><span class="sxs-lookup"><span data-stu-id="182fd-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="182fd-157">Yahoo ！Web Analytics 網站</span><span class="sxs-lookup"><span data-stu-id="182fd-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="182fd-158">StatCounter 站台</span><span class="sxs-lookup"><span data-stu-id="182fd-158">StatCounter site</span></span>](http://statcounter.com/)
