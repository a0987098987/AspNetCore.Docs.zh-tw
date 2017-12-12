---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "建立可讀取的 Url 中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本文說明在 ASP.NET Web Pages (Razor) 網站，然後這如何可讓您使用更容易讀取與 seo 更好的 Url 路由。 您將的會..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="d1305-104">在 ASP.NET Web Pages (Razor) 網站中建立可讀取的 Url</span><span class="sxs-lookup"><span data-stu-id="d1305-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="d1305-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d1305-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d1305-106">本文說明在 ASP.NET Web Pages (Razor) 網站，然後這如何可讓您使用更容易讀取與 seo 更好的 Url 路由。</span><span class="sxs-lookup"><span data-stu-id="d1305-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="d1305-107">您將學習：</span><span class="sxs-lookup"><span data-stu-id="d1305-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d1305-108">ASP.NET 如何使用路由可讓您使用更具可讀性和可搜尋的 Url。</span><span class="sxs-lookup"><span data-stu-id="d1305-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d1305-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="d1305-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d1305-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d1305-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d1305-111">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="d1305-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="d1305-112">關於路由</span><span class="sxs-lookup"><span data-stu-id="d1305-112">About Routing</span></span>

<span data-ttu-id="d1305-113">您的網站中頁面的 Url 可能會影響網站運作的程度。</span><span class="sxs-lookup"><span data-stu-id="d1305-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="d1305-114">URL 的&quot;易記&quot;更容易讓人使用的站台。</span><span class="sxs-lookup"><span data-stu-id="d1305-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="d1305-115">它也會協助搜尋引擎最佳化 (SEO) 站台。</span><span class="sxs-lookup"><span data-stu-id="d1305-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="d1305-116">ASP.NET 網站包括自動使用易記 Url 的能力。</span><span class="sxs-lookup"><span data-stu-id="d1305-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="d1305-117">ASP.NET 可讓您建立有意義描述使用者動作，而不是指向伺服器上的檔案的 Url。</span><span class="sxs-lookup"><span data-stu-id="d1305-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="d1305-118">請考量這些 Url 虛構的部落格：</span><span class="sxs-lookup"><span data-stu-id="d1305-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="d1305-119">比較這些 Url 的下列項目：</span><span class="sxs-lookup"><span data-stu-id="d1305-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="d1305-120">在第一對中，使用者必須知道部落格會顯示使用*blog.cshtml*頁面，然後就必須建構取得正確的類別目錄或日期範圍的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d1305-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="d1305-121">第二組範例會比較容易理解，並建立項目。</span><span class="sxs-lookup"><span data-stu-id="d1305-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="d1305-122">第一個範例中的 Url 也直接指向特定的檔案 (*blog.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="d1305-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="d1305-123">如果基於某些原因部落格已移至另一個資料夾的伺服器上，或已改寫部落格，以使用不同的網頁，連結就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d1305-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="d1305-124">第二個一組 Url 未指向特定的頁面上，因此，即使部落格實作或位置變更時，Url 仍然會有效。</span><span class="sxs-lookup"><span data-stu-id="d1305-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="d1305-125">ASP.NET Web Pages 中，您可以建立更容易使用的 Url，類似上述範例中，因為 ASP.NET 會使用*路由*。</span><span class="sxs-lookup"><span data-stu-id="d1305-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="d1305-126">路由與可以完成要求的 URL 頁面 （或頁面） 中建立邏輯對應。</span><span class="sxs-lookup"><span data-stu-id="d1305-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="d1305-127">因為對應邏輯 （不是實體，特定檔案），路由會提供更多如何為您的網站定義 Url 的彈性。</span><span class="sxs-lookup"><span data-stu-id="d1305-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="d1305-128">路由的運作方式</span><span class="sxs-lookup"><span data-stu-id="d1305-128">How Routing Works</span></span>

<span data-ttu-id="d1305-129">當 ASP.NET 處理要求時，它會讀取 URL，以判斷如何將它路由傳送。</span><span class="sxs-lookup"><span data-stu-id="d1305-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="d1305-130">ASP.NET 會嘗試比對個別檔案在磁碟上，會從左到右的 URL 區段。</span><span class="sxs-lookup"><span data-stu-id="d1305-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="d1305-131">如果沒有相符項目，在 URL 中所剩餘的任何項目會傳遞至與頁面*路徑資訊*。</span><span class="sxs-lookup"><span data-stu-id="d1305-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="d1305-132">假設有其他人會使用此 URL 的要求：</span><span class="sxs-lookup"><span data-stu-id="d1305-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="d1305-133">搜尋會像這樣：</span><span class="sxs-lookup"><span data-stu-id="d1305-133">The search goes like this:</span></span>

1. <span data-ttu-id="d1305-134">是否有檔案路徑和名稱*/a/b/c.cshtml*嗎？</span><span class="sxs-lookup"><span data-stu-id="d1305-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="d1305-135">如果是這樣，執行該頁面，並將任何資訊傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="d1305-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="d1305-136">否則...</span><span class="sxs-lookup"><span data-stu-id="d1305-136">Otherwise ...</span></span>
2. <span data-ttu-id="d1305-137">是否有檔案路徑和名稱*/a/b.cshtml*嗎？</span><span class="sxs-lookup"><span data-stu-id="d1305-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="d1305-138">因此，執行該頁面，並將值傳遞如果`c`給它。</span><span class="sxs-lookup"><span data-stu-id="d1305-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="d1305-139">否則...</span><span class="sxs-lookup"><span data-stu-id="d1305-139">Otherwise …</span></span>
3. <span data-ttu-id="d1305-140">是否有檔案路徑和名稱*/a.cshtml*嗎？</span><span class="sxs-lookup"><span data-stu-id="d1305-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="d1305-141">因此，執行該頁面，並將值傳遞如果`b/c`給它。</span><span class="sxs-lookup"><span data-stu-id="d1305-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="d1305-142">如果找到不精確的搜尋相符項目如*.cshtml*依次尋找這些檔案會指定的資料夾中的檔案，ASP.NET 繼續：</span><span class="sxs-lookup"><span data-stu-id="d1305-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="d1305-143">*/a/b/c/default.cshtml* （沒有路徑資訊）。</span><span class="sxs-lookup"><span data-stu-id="d1305-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="d1305-144">*/a/b/c/index.cshtml* （沒有路徑資訊）。</span><span class="sxs-lookup"><span data-stu-id="d1305-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="d1305-145">若要很清楚地，對特定網頁的要求 (也就是所提出的要求*.cshtml*副檔名) 運作方式就如同您預期的。</span><span class="sxs-lookup"><span data-stu-id="d1305-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="d1305-146">要求喜歡`http://www.contoso.com/a/b.cshtml`會執行頁面*b.cshtml*很好。</span><span class="sxs-lookup"><span data-stu-id="d1305-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="d1305-147">在頁面上，就可以透過網頁的路徑資訊`UrlData`屬性，這是字典。</span><span class="sxs-lookup"><span data-stu-id="d1305-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="d1305-148">假設您有名稱為的檔案*ViewCustomers.cshtml*和您的網站取得此要求：</span><span class="sxs-lookup"><span data-stu-id="d1305-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="d1305-149">上述規則所述，要求將會移至您的頁面。</span><span class="sxs-lookup"><span data-stu-id="d1305-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="d1305-150">在頁面上，您可以使用下列程式碼取得並顯示路徑資訊 (在此情況下，值&quot;1000年&quot;):</span><span class="sxs-lookup"><span data-stu-id="d1305-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="d1305-151">因為路由不包含完整檔案名稱，所以可以有模稜兩可有具有相同的頁面名稱但不同的檔案名稱副檔名 (例如， *MyPage.cshtml*和*MyPage.html*).</span><span class="sxs-lookup"><span data-stu-id="d1305-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="d1305-152">為了避免發生路由問題，所以最好先確定您不需要頁面在您的網站名稱差異只在於其擴充功能。</span><span class="sxs-lookup"><span data-stu-id="d1305-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d1305-153">其他資源</span><span class="sxs-lookup"><span data-stu-id="d1305-153">Additional Resources</span></span>

<span data-ttu-id="d1305-154">[WebMatrix-Url、 UrlData 和路由 seo](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)。</span><span class="sxs-lookup"><span data-stu-id="d1305-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="d1305-155">Mike Brind 此部落格文章提供如何路由適用於 ASP.NET Web Pages 中的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d1305-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
