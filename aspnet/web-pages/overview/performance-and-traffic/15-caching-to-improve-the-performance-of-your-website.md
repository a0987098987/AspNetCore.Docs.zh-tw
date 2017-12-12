---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "快取 ASP.NET Web 中的資料頁面 (Razor) 站台，以提升效能 |Microsoft 文件"
author: tfitzmac
description: "您可以加速您的網站將它儲存-也就是快取的資料，但通常需要相當長的時間來擷取，或處理結果..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: c747fef33a6d1db19f09fd0303c47d689b956687
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a><span data-ttu-id="811ea-103">為了達到最佳效能的 ASP.NET Web Pages (Razor) 網站中的快取資料</span><span class="sxs-lookup"><span data-stu-id="811ea-103">Caching Data in an ASP.NET Web Pages (Razor) Site for Better Performance</span></span>
====================
<span data-ttu-id="811ea-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="811ea-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="811ea-105">本文說明如何使用更快的效能，在 ASP.NET Web Pages (Razor) 的網站中的協助程式快取資訊。</span><span class="sxs-lookup"><span data-stu-id="811ea-105">This article explains how to use a helper to cache information for faster performance in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="811ea-106">您可以透過將它存放區 &#8212; 加快您的網站也就是快取 &#8212;資料通常會花費相當長的時間來擷取，或處理和，不常變更的結果。</span><span class="sxs-lookup"><span data-stu-id="811ea-106">You can speed up your website by having it store &#8212; that is, cache &#8212; the results of data that ordinarily would take considerable time to retrieve or process and that does not change often.</span></span>
> 
> <span data-ttu-id="811ea-107">**您將學習：**</span><span class="sxs-lookup"><span data-stu-id="811ea-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="811ea-108">如何使用快取來改善您的網站的回應能力。</span><span class="sxs-lookup"><span data-stu-id="811ea-108">How to use caching to improve the responsiveness of your website.</span></span>
> 
> <span data-ttu-id="811ea-109">這些是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="811ea-109">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="811ea-110">`WebCache`協助程式。</span><span class="sxs-lookup"><span data-stu-id="811ea-110">The `WebCache` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="811ea-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="811ea-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="811ea-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="811ea-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="811ea-113">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="811ea-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="811ea-114">每次有人從您的網站要求網頁，網頁伺服器必須執行一些工作，才能完成要求。</span><span class="sxs-lookup"><span data-stu-id="811ea-114">Every time someone requests a page from your site, the web server has to do some work in order to fulfill the request.</span></span> <span data-ttu-id="811ea-115">某些網頁伺服器可能必須執行需要 （相對） 長的時間，例如從資料庫擷取資料的工作。</span><span class="sxs-lookup"><span data-stu-id="811ea-115">For some of your pages, the server might have to perform tasks that take a (comparatively) long time, such as retrieving data from a database.</span></span> <span data-ttu-id="811ea-116">即使這些工作才長時間在絕對詞彙中，如果您的網站體驗有大量的流量，整個系列的個別要求導致網頁伺服器來執行複雜或緩慢的工作很多的工作可以加總。</span><span class="sxs-lookup"><span data-stu-id="811ea-116">Even if these tasks don't take long in absolute terms, if your site experiences a lot of traffic, a whole series of individual requests that cause the web server to perform the complicated or slow task can add up to a lot of work.</span></span> <span data-ttu-id="811ea-117">最後，這可能會影響網站效能。</span><span class="sxs-lookup"><span data-stu-id="811ea-117">This can ultimately affect the performance of the site.</span></span>

<span data-ttu-id="811ea-118">快取資料是網站的一種方法改善您在像這樣的情況下的效能。</span><span class="sxs-lookup"><span data-stu-id="811ea-118">One way to improve the performance of your website in circumstances like this is to cache data.</span></span> <span data-ttu-id="811ea-119">如果您的網站取得重複的要求相同的資訊，且不需要修改的每個人，資訊不是時間區分大小寫，而不是重新提取或重新計算，您可以一次擷取資料並儲存該結果。</span><span class="sxs-lookup"><span data-stu-id="811ea-119">If your site gets repeated requests for the same information, and the information does not need to be modified for each person, and it's not time sensitive, instead of re-fetching or recalculating it, you can fetch the data once and then store the results.</span></span> <span data-ttu-id="811ea-120">下一次要求進入該資訊，您只出現它在快取。</span><span class="sxs-lookup"><span data-stu-id="811ea-120">The next time a request comes in for that information, you just get it out of the cache.</span></span>

<span data-ttu-id="811ea-121">一般情況下，您的快取不會經常變更的資訊。</span><span class="sxs-lookup"><span data-stu-id="811ea-121">In general, you cache information that doesn't change frequently.</span></span> <span data-ttu-id="811ea-122">當您將資訊放在快取中時，它會儲存在 web 伺服器上的記憶體。</span><span class="sxs-lookup"><span data-stu-id="811ea-122">When you put information in the cache, it's stored in memory on the web server.</span></span> <span data-ttu-id="811ea-123">您可以指定多久它應該快取，從數秒到天。</span><span class="sxs-lookup"><span data-stu-id="811ea-123">You can specify how long it should be cached, from seconds to days.</span></span> <span data-ttu-id="811ea-124">在快取期間過期時，從快取時，會自動移除資訊。</span><span class="sxs-lookup"><span data-stu-id="811ea-124">When the caching period expires, the information is automatically removed from the cache.</span></span>

> [!NOTE]
> <span data-ttu-id="811ea-125">快取中的項目可能會移除原因不是已過期。</span><span class="sxs-lookup"><span data-stu-id="811ea-125">Entries in the cache might be removed for reasons other than that they've expired.</span></span> <span data-ttu-id="811ea-126">例如，web 伺服器可能會暫時記憶體不足，而它可以回收記憶體的其中一個方法是藉由擲回在快取的項目。</span><span class="sxs-lookup"><span data-stu-id="811ea-126">For example, the web server might temporarily run low on memory, and one way it can reclaim memory is by throwing entries out of the cache.</span></span> <span data-ttu-id="811ea-127">您會看到，即使您已將資訊放入快取，您必須檢查以確定它仍在需要時。</span><span class="sxs-lookup"><span data-stu-id="811ea-127">As you'll see, even if you've put information into the cache, you have to check to be sure it's still there when you need it.</span></span>


<span data-ttu-id="811ea-128">假設您的網站有一個顯示目前的溫度和氣象預報預測的頁面。</span><span class="sxs-lookup"><span data-stu-id="811ea-128">Imagine your website has a page that displays the current temperature and weather forecast.</span></span> <span data-ttu-id="811ea-129">若要取得這種類型的資訊，您可能會將要求傳送到外部服務。</span><span class="sxs-lookup"><span data-stu-id="811ea-129">To get this type of information, you might send a request to an external service.</span></span> <span data-ttu-id="811ea-130">因為這項資訊不會變更 （在兩小時期間內，例如） 的許多，因為外部呼叫需要時間和頻寬，它是絕佳候選快取。</span><span class="sxs-lookup"><span data-stu-id="811ea-130">Since this information doesn't change much (within a two-hour time period, for example) and since external calls require time and bandwidth, it's a good candidate for caching.</span></span>

## <a name="adding-caching-to-a-page"></a><span data-ttu-id="811ea-131">加入快取的頁面</span><span class="sxs-lookup"><span data-stu-id="811ea-131">Adding Caching to a Page</span></span>

<span data-ttu-id="811ea-132">ASP.NET 包含`WebCache`helper，可讓您輕鬆地將快取加入至您的網站，並將資料加入至快取。</span><span class="sxs-lookup"><span data-stu-id="811ea-132">ASP.NET includes a `WebCache` helper that makes it easy to add caching to your site and add data to the cache.</span></span> <span data-ttu-id="811ea-133">在此程序，您將建立快取的目前時間的頁面。</span><span class="sxs-lookup"><span data-stu-id="811ea-133">In this procedure, you'll create a page that caches the current time.</span></span> <span data-ttu-id="811ea-134">這不是真實世界範例中，因為目前的時間則是項目，並經常變更，而且這並不複雜計算。</span><span class="sxs-lookup"><span data-stu-id="811ea-134">This isn't a real-world example, since the current time is something that does change often, and that moreover isn't complex to calculate.</span></span> <span data-ttu-id="811ea-135">不過，它是為了說明動作中的快取的好方法。</span><span class="sxs-lookup"><span data-stu-id="811ea-135">However, it's a good way to illustrate caching in action.</span></span>

1. <span data-ttu-id="811ea-136">加入新的頁面名稱為*WebCache.cshtml*網站。</span><span class="sxs-lookup"><span data-stu-id="811ea-136">Add a new page named *WebCache.cshtml* to the website.</span></span>
2. <span data-ttu-id="811ea-137">將下列程式碼和標記加入頁面：</span><span class="sxs-lookup"><span data-stu-id="811ea-137">Add the following code and markup to the page:</span></span>

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    <span data-ttu-id="811ea-138">當您快取資料時，請在放入快取使用的名稱這是唯一的網站。</span><span class="sxs-lookup"><span data-stu-id="811ea-138">When you cache data, you put it into the cache using a name this is unique across the website.</span></span> <span data-ttu-id="811ea-139">在此情況下，您將使用名為快取項目`CachedTime`。</span><span class="sxs-lookup"><span data-stu-id="811ea-139">In this case, you'll use a cache entry named `CachedTime`.</span></span> <span data-ttu-id="811ea-140">這是`cacheItemKey`程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="811ea-140">This is the `cacheItemKey` shown in the code example.</span></span>

    <span data-ttu-id="811ea-141">程式碼會先讀取`CachedTime`快取項目。</span><span class="sxs-lookup"><span data-stu-id="811ea-141">The code first reads the `CachedTime` cache entry.</span></span> <span data-ttu-id="811ea-142">如果傳回值 （亦即，如果快取項目不是 null），程式碼只會將時間變數的值來快取資料。</span><span class="sxs-lookup"><span data-stu-id="811ea-142">If a value is returned (that is, if the cache entry isn't null), the code just sets the value of the time variable to the cache data.</span></span>

    <span data-ttu-id="811ea-143">不過，如果快取項目不存在 （也就是說，它是 null），程式碼設定時間值、 將它加入至快取中，並設定到期值為一分鐘。</span><span class="sxs-lookup"><span data-stu-id="811ea-143">However, if the cache entry doesn't exist (that is, it's null), the code sets the time value, adds it to the cache, and sets an expiration value to one minute.</span></span> <span data-ttu-id="811ea-144">在一分鐘之後, 會捨棄快取項目。</span><span class="sxs-lookup"><span data-stu-id="811ea-144">After one minute, the cache entry is discarded.</span></span> <span data-ttu-id="811ea-145">（項目在快取中的預設到期值是 20 分鐘）。此命令`WebCache.Set(cacheItemKey, time, 1, false)`示範如何將目前的時間值加入至快取以及其期限設為 1 分鐘。</span><span class="sxs-lookup"><span data-stu-id="811ea-145">(The default expiration value for an item in the cache is 20 minutes.) The command `WebCache.Set(cacheItemKey, time, 1, false)` shows how to add the current time value to the cache and set its expiration to 1 minute.</span></span> <span data-ttu-id="811ea-146">設定*slidingExpiration*參數`false`表示的到期時間未更新每次要求時。</span><span class="sxs-lookup"><span data-stu-id="811ea-146">Setting the *slidingExpiration* parameter to `false` means the expiration time is not renewed each time it is requested.</span></span> <span data-ttu-id="811ea-147">到期完全原先加入至快取後 1 分鐘。</span><span class="sxs-lookup"><span data-stu-id="811ea-147">It will expire exactly 1 minute after it was originally added to the cache.</span></span> <span data-ttu-id="811ea-148">如果您將此值設定為`true`從快取要求的值每次重設的 1 分鐘到期時間。</span><span class="sxs-lookup"><span data-stu-id="811ea-148">If you set this value to `true` the 1 minute expiration time is reset each time the value is requested from the cache.</span></span>

    <span data-ttu-id="811ea-149">此程式碼說明時，您應該一律使用快取資料的模式。</span><span class="sxs-lookup"><span data-stu-id="811ea-149">This code illustrates the pattern you should always use when you cache data.</span></span> <span data-ttu-id="811ea-150">您會得到在快取之前，一律先查看是否`WebCache.Get`方法已傳回 null。</span><span class="sxs-lookup"><span data-stu-id="811ea-150">Before you get something out of the cache, always check first whether the `WebCache.Get` method has returned null.</span></span> <span data-ttu-id="811ea-151">請記住快取項目可能已過期或可能已移除基於其他原因，所以永遠不會保證任何指定的項目，快取中。</span><span class="sxs-lookup"><span data-stu-id="811ea-151">Remember that the cache entry might have expired or might have been removed for some other reason, so any given entry is never guaranteed to be in the cache.</span></span>
3. <span data-ttu-id="811ea-152">執行*WebCache.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="811ea-152">Run *WebCache.cshtml* in a browser.</span></span> <span data-ttu-id="811ea-153">(請確定中選取頁面**檔案**才能執行這個工作區。)第一次要求頁面的時間資料沒有快取中，而且的程式碼新增至快取的時間值。</span><span class="sxs-lookup"><span data-stu-id="811ea-153">(Make sure the page is selected in the **Files** workspace before you run it.) The first time you request the page, the time data isn't in the cache, and the code has to add the time value to the cache.</span></span>

    ![快取-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. <span data-ttu-id="811ea-155">重新整理*WebCache.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="811ea-155">Refresh *WebCache.cshtml* in the browser.</span></span> <span data-ttu-id="811ea-156">此時，[時間] 資料是快取中。</span><span class="sxs-lookup"><span data-stu-id="811ea-156">This time, the time data is in the cache.</span></span> <span data-ttu-id="811ea-157">請注意，時間尚未變更檢視頁面最後一次。</span><span class="sxs-lookup"><span data-stu-id="811ea-157">Notice that the time hasn't changed since the last time you viewed the page.</span></span>

    ![快取-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. <span data-ttu-id="811ea-159">等待一分鐘先清空快取，然後重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="811ea-159">Wait one minute for the cache to be emptied, and then refresh the page.</span></span> <span data-ttu-id="811ea-160">頁面會再次指出快取中，找不到時間資料，並新增到快取更新的時間。</span><span class="sxs-lookup"><span data-stu-id="811ea-160">The page again indicates that the time data wasn't found in the cache, and the updated time is added to the cache.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="811ea-161">其他資源</span><span class="sxs-lookup"><span data-stu-id="811ea-161">Additional Resources</span></span>


- [<span data-ttu-id="811ea-162">在圖表中顯示資料</span><span class="sxs-lookup"><span data-stu-id="811ea-162">Displaying Data in a Chart</span></span>](https://go.microsoft.com/fwlink/?LinkId=202895)
- <span data-ttu-id="811ea-163">[WebCache API 參考](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="811ea-163">[WebCache API reference](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)</span></span>
