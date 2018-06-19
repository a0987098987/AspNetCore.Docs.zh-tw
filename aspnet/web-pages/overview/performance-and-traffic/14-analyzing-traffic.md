---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: 追蹤的 ASP.NET Web Pages (Razor) 網站訪客資訊 （分析） |Microsoft 文件
author: tfitzmac
description: 您您之後將您的網站之後，您可能想要分析網站流量。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528757"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d4a69-103">追蹤的 ASP.NET Web Pages (Razor) 網站訪客資訊 （分析）</span><span class="sxs-lookup"><span data-stu-id="d4a69-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="d4a69-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d4a69-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d4a69-105">本文說明如何使用協助程式，將網站分析加入 ASP.NET Web Pages (Razor) 網站中的網頁。</span><span class="sxs-lookup"><span data-stu-id="d4a69-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="d4a69-106">您將學習：</span><span class="sxs-lookup"><span data-stu-id="d4a69-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d4a69-107">如何將您的網站流量的相關資訊傳送至分析提供者。</span><span class="sxs-lookup"><span data-stu-id="d4a69-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="d4a69-108">這些是 ASP.NET 程式設計文件中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="d4a69-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="d4a69-109">`Analytics`協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4a69-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d4a69-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="d4a69-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d4a69-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d4a69-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d4a69-112">ASP.NET Web Helpers Library （NuGet 套件）</span><span class="sxs-lookup"><span data-stu-id="d4a69-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="d4a69-113">分析是技術，測量您的網站上的流量，因此您可以了解使用者如何使用站台的一般詞彙。</span><span class="sxs-lookup"><span data-stu-id="d4a69-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="d4a69-114">許多分析可用的服務，包括 Google、 Yahoo、 StatCounter，和其他服務。</span><span class="sxs-lookup"><span data-stu-id="d4a69-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="d4a69-115">想要追蹤的運作方式是確認您登入帳戶分析提供者使用，讓您在其中註冊站台，您的方式分析。提供者會傳送包含識別碼或追蹤程式碼，為您的帳戶的 JavaScript 程式碼的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d4a69-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="d4a69-116">您可以將 JavaScript 程式碼片段加入您想要追蹤的站台上的網頁。（您通常將分析的程式碼片段加入頁尾或版面配置頁或其他 HTML 標記出現在網站中，每個頁面上。）當使用者要求網頁包含其中一種這些 JavaScript 程式碼片段時，程式碼片段會傳送給的分析提供者，會記錄各種詳細資料頁面的目前頁面的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d4a69-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="d4a69-117">當您想要看看您的網站統計資料時，您登入分析提供者的網站。</span><span class="sxs-lookup"><span data-stu-id="d4a69-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="d4a69-118">然後，您可以檢視有關您站台，各種報表類似：</span><span class="sxs-lookup"><span data-stu-id="d4a69-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="d4a69-119">個別頁面的頁面檢視數目。</span><span class="sxs-lookup"><span data-stu-id="d4a69-119">The number of page views for individual pages.</span></span> <span data-ttu-id="d4a69-120">這會告訴您大致上有多人造訪網站，而且最受歡迎網站上的頁面。</span><span class="sxs-lookup"><span data-stu-id="d4a69-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="d4a69-121">多久人員花上的特定頁面。</span><span class="sxs-lookup"><span data-stu-id="d4a69-121">How long people spend on specific pages.</span></span> <span data-ttu-id="d4a69-122">這會告訴您等您的首頁是否保持人感興趣。</span><span class="sxs-lookup"><span data-stu-id="d4a69-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="d4a69-123">站台人們為何上之前造訪您的網站。</span><span class="sxs-lookup"><span data-stu-id="d4a69-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="d4a69-124">這可協助您了解您的流量是否來自連結，從搜尋，依此類推。</span><span class="sxs-lookup"><span data-stu-id="d4a69-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="d4a69-125">當使用者造訪您的網站，他們保持多久。</span><span class="sxs-lookup"><span data-stu-id="d4a69-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="d4a69-126">為您的訪客哪些國家/地區。</span><span class="sxs-lookup"><span data-stu-id="d4a69-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="d4a69-127">何種瀏覽器和作業系統會使用您的訪客。</span><span class="sxs-lookup"><span data-stu-id="d4a69-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="d4a69-129">使用 Helper 將分析加入頁面</span><span class="sxs-lookup"><span data-stu-id="d4a69-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="d4a69-130">ASP.NET Web Pages 包含數個分析 helper (`Analytics.GetGoogleHtml`， `Analytics.GetYahooHtml`，和`Analytics.GetStatCounterHtml`)，讓您輕鬆管理用於分析的 JavaScript 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d4a69-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="d4a69-131">而方式和位置，找出不是將 JavaScript 程式碼中，您只需要為協助專家將加入頁面。</span><span class="sxs-lookup"><span data-stu-id="d4a69-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="d4a69-132">您需要提供的唯一資訊是您的帳戶名稱、 識別碼或追蹤程式碼。</span><span class="sxs-lookup"><span data-stu-id="d4a69-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="d4a69-133">（如 StatCounter，您也必須提供幾個額外的值。）</span><span class="sxs-lookup"><span data-stu-id="d4a69-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="d4a69-134">在此程序，您將建立使用的版面配置頁`GetGoogleHtml`協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4a69-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="d4a69-135">如果您已經與其他廠商分析提供者的其中一個帳戶，您可以改為使用該帳戶，並視需要進行了些微調整。</span><span class="sxs-lookup"><span data-stu-id="d4a69-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="d4a69-136">當您建立 analytics 帳戶時，您會註冊您想要追蹤的網站 URL。</span><span class="sxs-lookup"><span data-stu-id="d4a69-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="d4a69-137">如果您要測試的所有項目在本機電腦上，您將不會追蹤實際流量 （只流量您），因此您無法再記錄和檢視的網站統計資料。</span><span class="sxs-lookup"><span data-stu-id="d4a69-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="d4a69-138">但是，此程序示範如何將分析 helper 加入頁面。</span><span class="sxs-lookup"><span data-stu-id="d4a69-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="d4a69-139">當您發行您的網站時，即時網站會將您的分析提供者資訊。</span><span class="sxs-lookup"><span data-stu-id="d4a69-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="d4a69-140">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。</span><span class="sxs-lookup"><span data-stu-id="d4a69-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="d4a69-141">建立 Google Analytics 帳戶，並記錄的帳戶名稱。</span><span class="sxs-lookup"><span data-stu-id="d4a69-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="d4a69-142">建立名為的版面配置頁面*Analytics.cshtml*並加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="d4a69-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="d4a69-143">您必須將呼叫`Analytics`網頁主體中的協助程式 (之前`</body>`標記)。</span><span class="sxs-lookup"><span data-stu-id="d4a69-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="d4a69-144">否則，瀏覽器不會執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="d4a69-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="d4a69-145">如果您使用不同的分析提供者，請改用下列 helper 的其中一個：</span><span class="sxs-lookup"><span data-stu-id="d4a69-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="d4a69-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="d4a69-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="d4a69-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="d4a69-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="d4a69-148">取代`myaccount`帳戶、 識別碼或您在步驟 1 中建立的追蹤程式碼的名稱。</span><span class="sxs-lookup"><span data-stu-id="d4a69-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="d4a69-149">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="d4a69-149">Run the page in the browser.</span></span> <span data-ttu-id="d4a69-150">(請確定中選取頁面**檔案**才能執行這個工作區。)</span><span class="sxs-lookup"><span data-stu-id="d4a69-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="d4a69-151">在瀏覽器中檢視網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="d4a69-151">In the browser, view the page source.</span></span> <span data-ttu-id="d4a69-152">您可以查看轉譯的分析程式碼：</span><span class="sxs-lookup"><span data-stu-id="d4a69-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="d4a69-153">登入 Google Analytics 網站，並檢查您的站台的統計資料。</span><span class="sxs-lookup"><span data-stu-id="d4a69-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="d4a69-154">如果您即時的站台上執行的頁面，您會看到記錄到您的網頁瀏覽的項目。</span><span class="sxs-lookup"><span data-stu-id="d4a69-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d4a69-155">其他資源</span><span class="sxs-lookup"><span data-stu-id="d4a69-155">Additional Resources</span></span>

- [<span data-ttu-id="d4a69-156">Google Analytics 網站</span><span class="sxs-lookup"><span data-stu-id="d4a69-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="d4a69-157">Yahoo ！網站分析</span><span class="sxs-lookup"><span data-stu-id="d4a69-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="d4a69-158">StatCounter 站台</span><span class="sxs-lookup"><span data-stu-id="d4a69-158">StatCounter site</span></span>](http://statcounter.com/)
