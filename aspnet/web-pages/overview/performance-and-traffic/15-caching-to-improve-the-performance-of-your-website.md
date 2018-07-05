---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: 快取資料在 ASP.NET Web Pages (Razor) 網站以提升效能 |Microsoft Docs
author: tfitzmac
description: 您可以加速您的網站透過儲存-也就是讓快取-通常會花相當長的時間，以擷取或處理資料的結果...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 4134c80d7eed4752c90a06aab796a0fd8c2a9782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383405"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a><span data-ttu-id="31d26-103">快取 ASP.NET Web Pages (Razor) 網站中的資料，以提升效能</span><span class="sxs-lookup"><span data-stu-id="31d26-103">Caching Data in an ASP.NET Web Pages (Razor) Site for Better Performance</span></span>
====================
<span data-ttu-id="31d26-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="31d26-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="31d26-105">這篇文章說明如何使用更快的效能，在 ASP.NET Web Pages (Razor) 網站中的協助程式快取資訊。</span><span class="sxs-lookup"><span data-stu-id="31d26-105">This article explains how to use a helper to cache information for faster performance in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="31d26-106">您可以透過讓儲存加快您的網站&#8212;也就是快取&#8212;的結果通常會花費相當長的時間，以擷取或處理和可不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="31d26-106">You can speed up your website by having it store &#8212; that is, cache &#8212; the results of data that ordinarily would take considerable time to retrieve or process and that does not change often.</span></span>
> 
> <span data-ttu-id="31d26-107">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="31d26-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="31d26-108">如何使用快取來改善您的網站的回應能力。</span><span class="sxs-lookup"><span data-stu-id="31d26-108">How to use caching to improve the responsiveness of your website.</span></span>
> 
> <span data-ttu-id="31d26-109">以下是所引進的發行項 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="31d26-109">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="31d26-110">`WebCache`協助程式。</span><span class="sxs-lookup"><span data-stu-id="31d26-110">The `WebCache` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31d26-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="31d26-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="31d26-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="31d26-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="31d26-113">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="31d26-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="31d26-114">每次有人從您的網站要求網頁，網頁伺服器必須執行一些工作，以履行要求。</span><span class="sxs-lookup"><span data-stu-id="31d26-114">Every time someone requests a page from your site, the web server has to do some work in order to fulfill the request.</span></span> <span data-ttu-id="31d26-115">針對某些頁面，伺服器可能必須執行需要 （相較之下） 長的時間，例如從資料庫擷取資料的工作。</span><span class="sxs-lookup"><span data-stu-id="31d26-115">For some of your pages, the server might have to perform tasks that take a (comparatively) long time, such as retrieving data from a database.</span></span> <span data-ttu-id="31d26-116">即使這些工作不需要時間以絕對的條款中,，如果您的網站遇到大量流量，會導致 web 伺服器來執行複雜或速度很慢的工作的個別要求的整個系列最多可新增很多工作。</span><span class="sxs-lookup"><span data-stu-id="31d26-116">Even if these tasks don't take long in absolute terms, if your site experiences a lot of traffic, a whole series of individual requests that cause the web server to perform the complicated or slow task can add up to a lot of work.</span></span> <span data-ttu-id="31d26-117">最後，這可能會影響網站的效能。</span><span class="sxs-lookup"><span data-stu-id="31d26-117">This can ultimately affect the performance of the site.</span></span>

<span data-ttu-id="31d26-118">快取資料是網站的一種方法改善效能的情況下，像這樣在您。</span><span class="sxs-lookup"><span data-stu-id="31d26-118">One way to improve the performance of your website in circumstances like this is to cache data.</span></span> <span data-ttu-id="31d26-119">如果您的網站取得重複的要求相同的資訊，資訊就不需要修改每位人員，且不是時間區分大小寫，而不是重新擷取或重新計算，您可以一次提取資料並儲存結果。</span><span class="sxs-lookup"><span data-stu-id="31d26-119">If your site gets repeated requests for the same information, and the information does not need to be modified for each person, and it's not time sensitive, instead of re-fetching or recalculating it, you can fetch the data once and then store the results.</span></span> <span data-ttu-id="31d26-120">下次要求該傳入的詳細資訊，您只取得從快取。</span><span class="sxs-lookup"><span data-stu-id="31d26-120">The next time a request comes in for that information, you just get it out of the cache.</span></span>

<span data-ttu-id="31d26-121">一般情況下，您快取不會經常變更的資訊。</span><span class="sxs-lookup"><span data-stu-id="31d26-121">In general, you cache information that doesn't change frequently.</span></span> <span data-ttu-id="31d26-122">當您將資訊放在快取時，它會儲存在 web 伺服器上的記憶體。</span><span class="sxs-lookup"><span data-stu-id="31d26-122">When you put information in the cache, it's stored in memory on the web server.</span></span> <span data-ttu-id="31d26-123">您可以指定多久它應該快取，從 天前的秒數。</span><span class="sxs-lookup"><span data-stu-id="31d26-123">You can specify how long it should be cached, from seconds to days.</span></span> <span data-ttu-id="31d26-124">當快取期間到期時，從快取時，會自動移除資訊。</span><span class="sxs-lookup"><span data-stu-id="31d26-124">When the caching period expires, the information is automatically removed from the cache.</span></span>

> [!NOTE]
> <span data-ttu-id="31d26-125">快取中的項目可能已過期而不移除原因。</span><span class="sxs-lookup"><span data-stu-id="31d26-125">Entries in the cache might be removed for reasons other than that they've expired.</span></span> <span data-ttu-id="31d26-126">例如，網頁伺服器可能會暫時記憶體不足，而可以回收記憶體的其中一個方法是藉由擲回從快取的項目。</span><span class="sxs-lookup"><span data-stu-id="31d26-126">For example, the web server might temporarily run low on memory, and one way it can reclaim memory is by throwing entries out of the cache.</span></span> <span data-ttu-id="31d26-127">如您所見，即使您已經將資訊放入快取，您必須檢查以確定它仍然會有在需要時。</span><span class="sxs-lookup"><span data-stu-id="31d26-127">As you'll see, even if you've put information into the cache, you have to check to be sure it's still there when you need it.</span></span>


<span data-ttu-id="31d26-128">假設您的網站具有會顯示目前的溫度和氣象預報的頁面。</span><span class="sxs-lookup"><span data-stu-id="31d26-128">Imagine your website has a page that displays the current temperature and weather forecast.</span></span> <span data-ttu-id="31d26-129">若要取得這種類型的資訊，您可能會將要求傳送至外部服務。</span><span class="sxs-lookup"><span data-stu-id="31d26-129">To get this type of information, you might send a request to an external service.</span></span> <span data-ttu-id="31d26-130">因為這項資訊不會變更大部分 （在兩小時期間內，例如），因為外部呼叫都需要時間和頻寬，它是適合快取。</span><span class="sxs-lookup"><span data-stu-id="31d26-130">Since this information doesn't change much (within a two-hour time period, for example) and since external calls require time and bandwidth, it's a good candidate for caching.</span></span>

## <a name="adding-caching-to-a-page"></a><span data-ttu-id="31d26-131">加入至頁面快取</span><span class="sxs-lookup"><span data-stu-id="31d26-131">Adding Caching to a Page</span></span>

<span data-ttu-id="31d26-132">ASP.NET 包含`WebCache`協助程式，可讓您輕鬆地新增至您網站的 快取，並將資料新增至快取。</span><span class="sxs-lookup"><span data-stu-id="31d26-132">ASP.NET includes a `WebCache` helper that makes it easy to add caching to your site and add data to the cache.</span></span> <span data-ttu-id="31d26-133">在此程序中，您將建立快取的目前時間的頁面。</span><span class="sxs-lookup"><span data-stu-id="31d26-133">In this procedure, you'll create a page that caches the current time.</span></span> <span data-ttu-id="31d26-134">這不是真實世界範例中，因為目前的時間，並經常變更且此外沒有複雜計算的項目。</span><span class="sxs-lookup"><span data-stu-id="31d26-134">This isn't a real-world example, since the current time is something that does change often, and that moreover isn't complex to calculate.</span></span> <span data-ttu-id="31d26-135">不過，很適合用來說明快取的運作。</span><span class="sxs-lookup"><span data-stu-id="31d26-135">However, it's a good way to illustrate caching in action.</span></span>

1. <span data-ttu-id="31d26-136">新增名為頁面*WebCache.cshtml*網站。</span><span class="sxs-lookup"><span data-stu-id="31d26-136">Add a new page named *WebCache.cshtml* to the website.</span></span>
2. <span data-ttu-id="31d26-137">您可以將下列程式碼和標記加入頁面：</span><span class="sxs-lookup"><span data-stu-id="31d26-137">Add the following code and markup to the page:</span></span>

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    <span data-ttu-id="31d26-138">當您要快取資料時，您將它放入快取使用的名稱這是唯一的網站。</span><span class="sxs-lookup"><span data-stu-id="31d26-138">When you cache data, you put it into the cache using a name this is unique across the website.</span></span> <span data-ttu-id="31d26-139">在此情況下，您將使用名為快取項目`CachedTime`。</span><span class="sxs-lookup"><span data-stu-id="31d26-139">In this case, you'll use a cache entry named `CachedTime`.</span></span> <span data-ttu-id="31d26-140">這是`cacheItemKey`程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="31d26-140">This is the `cacheItemKey` shown in the code example.</span></span>

    <span data-ttu-id="31d26-141">程式碼會先讀取`CachedTime`快取項目。</span><span class="sxs-lookup"><span data-stu-id="31d26-141">The code first reads the `CachedTime` cache entry.</span></span> <span data-ttu-id="31d26-142">（亦即，如果快取項目不是 null），則會傳回值，如果程式碼只快取資料設定時間變數的值。</span><span class="sxs-lookup"><span data-stu-id="31d26-142">If a value is returned (that is, if the cache entry isn't null), the code just sets the value of the time variable to the cache data.</span></span>

    <span data-ttu-id="31d26-143">不過，如果快取項目不存在 （也就是它是 null），程式碼，可設定的時間值、 將它新增至快取中，和在逾期值設定為一分鐘。</span><span class="sxs-lookup"><span data-stu-id="31d26-143">However, if the cache entry doesn't exist (that is, it's null), the code sets the time value, adds it to the cache, and sets an expiration value to one minute.</span></span> <span data-ttu-id="31d26-144">一分鐘後，會捨棄快取項目。</span><span class="sxs-lookup"><span data-stu-id="31d26-144">After one minute, the cache entry is discarded.</span></span> <span data-ttu-id="31d26-145">（項目在快取的預設到期值是 20 分鐘）。此命令`WebCache.Set(cacheItemKey, time, 1, false)`示範如何將目前的時間值新增至快取，並將其到期設定為 1 分鐘。</span><span class="sxs-lookup"><span data-stu-id="31d26-145">(The default expiration value for an item in the cache is 20 minutes.) The command `WebCache.Set(cacheItemKey, time, 1, false)` shows how to add the current time value to the cache and set its expiration to 1 minute.</span></span> <span data-ttu-id="31d26-146">設定*slidingExpiration*參數來`false`表示到期時間未更新每次要求時。</span><span class="sxs-lookup"><span data-stu-id="31d26-146">Setting the *slidingExpiration* parameter to `false` means the expiration time is not renewed each time it is requested.</span></span> <span data-ttu-id="31d26-147">到期完全原先加入至快取之後 1 分鐘。</span><span class="sxs-lookup"><span data-stu-id="31d26-147">It will expire exactly 1 minute after it was originally added to the cache.</span></span> <span data-ttu-id="31d26-148">如果您將此值設定為`true`從快取所要求的值每次重設的 1 分鐘到期時間。</span><span class="sxs-lookup"><span data-stu-id="31d26-148">If you set this value to `true` the 1 minute expiration time is reset each time the value is requested from the cache.</span></span>

    <span data-ttu-id="31d26-149">此程式碼說明快取資料時，應該一律會使用您的模式。</span><span class="sxs-lookup"><span data-stu-id="31d26-149">This code illustrates the pattern you should always use when you cache data.</span></span> <span data-ttu-id="31d26-150">您的項目從快取之前，請一律先檢查是否`WebCache.Get`方法傳回 null。</span><span class="sxs-lookup"><span data-stu-id="31d26-150">Before you get something out of the cache, always check first whether the `WebCache.Get` method has returned null.</span></span> <span data-ttu-id="31d26-151">請記住，快取項目可能已過期或可能已移除某些其他原因，因此永遠不會保證任何指定的項目，快取。</span><span class="sxs-lookup"><span data-stu-id="31d26-151">Remember that the cache entry might have expired or might have been removed for some other reason, so any given entry is never guaranteed to be in the cache.</span></span>
3. <span data-ttu-id="31d26-152">執行*WebCache.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="31d26-152">Run *WebCache.cshtml* in a browser.</span></span> <span data-ttu-id="31d26-153">(請確定中選取頁面**檔案**才能執行這個工作區。)第一次要求頁面上，[時間] 資料快取中，但程式碼會將時間值新增至快取。</span><span class="sxs-lookup"><span data-stu-id="31d26-153">(Make sure the page is selected in the **Files** workspace before you run it.) The first time you request the page, the time data isn't in the cache, and the code has to add the time value to the cache.</span></span>

    ![快取-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. <span data-ttu-id="31d26-155">重新整理*WebCache.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="31d26-155">Refresh *WebCache.cshtml* in the browser.</span></span> <span data-ttu-id="31d26-156">此時，[時間] 資料位於快取中。</span><span class="sxs-lookup"><span data-stu-id="31d26-156">This time, the time data is in the cache.</span></span> <span data-ttu-id="31d26-157">請注意，時間沒有變更上次檢視頁面。</span><span class="sxs-lookup"><span data-stu-id="31d26-157">Notice that the time hasn't changed since the last time you viewed the page.</span></span>

    ![快取-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. <span data-ttu-id="31d26-159">等候一分鐘，讓快取會清空，然後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="31d26-159">Wait one minute for the cache to be emptied, and then refresh the page.</span></span> <span data-ttu-id="31d26-160">頁面會再次指出快取中，找不到時間資料，並快取中加入更新的時間。</span><span class="sxs-lookup"><span data-stu-id="31d26-160">The page again indicates that the time data wasn't found in the cache, and the updated time is added to the cache.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="31d26-161">其他資源</span><span class="sxs-lookup"><span data-stu-id="31d26-161">Additional Resources</span></span>


- [<span data-ttu-id="31d26-162">以圖表顯示資料</span><span class="sxs-lookup"><span data-stu-id="31d26-162">Displaying Data in a Chart</span></span>](https://go.microsoft.com/fwlink/?LinkId=202895)
- <span data-ttu-id="31d26-163">[WebCache API 參考](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="31d26-163">[WebCache API reference](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)</span></span>
