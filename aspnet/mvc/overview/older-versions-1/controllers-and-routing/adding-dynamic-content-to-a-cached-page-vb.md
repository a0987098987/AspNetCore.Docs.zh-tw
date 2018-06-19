---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: 將動態內容加入至快取的頁面 (VB) |Microsoft 文件
author: microsoft
description: 了解如何混用動態和快取的內容，在相同的頁面。 快取後替換可讓您顯示橫幅廣告 o 之類的動態內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879760"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="c4438-104">將動態內容加入至快取的頁面 (VB)</span><span class="sxs-lookup"><span data-stu-id="c4438-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="c4438-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c4438-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c4438-106">了解如何混用動態和快取的內容，在相同的頁面。</span><span class="sxs-lookup"><span data-stu-id="c4438-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="c4438-107">快取後替換可讓您顯示橫幅廣告或具有已輸出快取網頁內的新聞項目，例如的動態內容。</span><span class="sxs-lookup"><span data-stu-id="c4438-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="c4438-108">藉由運用輸出快取，您可以大幅改善 ASP.NET MVC 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="c4438-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="c4438-109">而是重新產生頁面，每次要求頁面時，可以產生一次並快取在記憶體中的多個使用者 頁面。</span><span class="sxs-lookup"><span data-stu-id="c4438-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="c4438-110">但有問題。</span><span class="sxs-lookup"><span data-stu-id="c4438-110">But there is a problem.</span></span> <span data-ttu-id="c4438-111">如果您需要在頁面中顯示動態內容嗎？</span><span class="sxs-lookup"><span data-stu-id="c4438-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="c4438-112">例如，假設您想要在頁面中顯示橫幅廣告。</span><span class="sxs-lookup"><span data-stu-id="c4438-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="c4438-113">您不想要快取，讓每個使用者會看到相同的廣告橫幅廣告。</span><span class="sxs-lookup"><span data-stu-id="c4438-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="c4438-114">您不會進行任何 money 如此 ！</span><span class="sxs-lookup"><span data-stu-id="c4438-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="c4438-115">幸運的是，沒有簡單的解決方案。</span><span class="sxs-lookup"><span data-stu-id="c4438-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="c4438-116">您可以利用一項功能稱為 ASP.NET framework 的*快取後替換*。</span><span class="sxs-lookup"><span data-stu-id="c4438-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="c4438-117">快取後替換可讓您以取代在記憶體中快取的頁面中的動態內容。</span><span class="sxs-lookup"><span data-stu-id="c4438-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="c4438-118">一般來說，當您在輸出快取網頁使用&lt;OutputCache&gt;屬性頁面會快取伺服器和用戶端 （web 瀏覽器）。</span><span class="sxs-lookup"><span data-stu-id="c4438-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="c4438-119">當您使用快取後替換時，則是只能在伺服器上快取頁面。</span><span class="sxs-lookup"><span data-stu-id="c4438-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="c4438-120">使用快取後替換</span><span class="sxs-lookup"><span data-stu-id="c4438-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="c4438-121">使用快取後替換需要兩個步驟。</span><span class="sxs-lookup"><span data-stu-id="c4438-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="c4438-122">首先，您需要定義傳回字串，表示您想要在快取的頁面中顯示的動態內容的方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="c4438-123">接下來，您呼叫 HttpResponse.WriteSubstitution() 方法來插入頁面的動態內容。</span><span class="sxs-lookup"><span data-stu-id="c4438-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="c4438-124">例如，假設您想要隨機快取的頁面中顯示不同的新聞項目。</span><span class="sxs-lookup"><span data-stu-id="c4438-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="c4438-125">程式碼範例 1 中的類別會公開單一方法，名為 RenderNews() 隨機傳回一個新聞項目清單中的 3 個新聞項目。</span><span class="sxs-lookup"><span data-stu-id="c4438-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="c4438-126">**列出 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="c4438-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="c4438-127">若要充分利用快取後替換，您可以呼叫 HttpResponse.WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="c4438-128">WriteSubstitution() 方法會設定要取代動態內容的快取的頁面區域的程式碼。</span><span class="sxs-lookup"><span data-stu-id="c4438-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="c4438-129">WriteSubstitution() 方法用來顯示的檢視清單 2 中的隨機新聞項目。</span><span class="sxs-lookup"><span data-stu-id="c4438-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="c4438-130">**列出 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="c4438-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="c4438-131">RenderNews 方法會傳遞至 WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="c4438-132">請注意 RenderNews 方法不會呼叫。</span><span class="sxs-lookup"><span data-stu-id="c4438-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="c4438-133">不過此方法的參考傳遞至 WriteSubstitution() AddressOf 運算子的說明。</span><span class="sxs-lookup"><span data-stu-id="c4438-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="c4438-134">索引檢視會快取。</span><span class="sxs-lookup"><span data-stu-id="c4438-134">The Index view is cached.</span></span> <span data-ttu-id="c4438-135">檢視會傳回所列出的 3 中的控制站。</span><span class="sxs-lookup"><span data-stu-id="c4438-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="c4438-136">請注意，index （） 動作以裝飾&lt;OutputCache&gt;造成快取為 60 秒的索引檢視的屬性。</span><span class="sxs-lookup"><span data-stu-id="c4438-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="c4438-137">**列出 3 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="c4438-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="c4438-138">即使索引檢視會快取，當您要求的索引頁面時，會顯示不同的隨機新聞項目。</span><span class="sxs-lookup"><span data-stu-id="c4438-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="c4438-139">當您要求的索引頁面時，頁面所顯示的時間不會變更 （請參閱圖 1） 的 60 秒。</span><span class="sxs-lookup"><span data-stu-id="c4438-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="c4438-140">時間不會變更的事實證明快取頁面。</span><span class="sxs-lookup"><span data-stu-id="c4438-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="c4438-141">不過，內容插入 WriteSubstitution() 方法-隨機新聞項目 – 變更每個要求。</span><span class="sxs-lookup"><span data-stu-id="c4438-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="c4438-142">**圖 1 – 插入快取的頁面中的動態新聞項目**</span><span class="sxs-lookup"><span data-stu-id="c4438-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="c4438-144">使用快取後替換中的 Helper 方法</span><span class="sxs-lookup"><span data-stu-id="c4438-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="c4438-145">若要善用快取後替換更簡單的方法是封裝 WriteSubstitution() 方法內的自訂 helper 方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="c4438-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="c4438-146">這種方法是 helper 方法，在列出的 4 所示。</span><span class="sxs-lookup"><span data-stu-id="c4438-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="c4438-147">**列出 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="c4438-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="c4438-148">列出 4 包含 Visual Basic 的 module 可公開兩個方法： RenderBanner() 和 RenderBannerInternal()。</span><span class="sxs-lookup"><span data-stu-id="c4438-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="c4438-149">RenderBanner() 方法代表實際的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="c4438-150">此方法會擴充標準的 ASP.NET MVC HtmlHelper 類別，可讓您呼叫 Html.RenderBanner() 就像任何其他 helper 方法一樣檢視中。</span><span class="sxs-lookup"><span data-stu-id="c4438-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="c4438-151">RenderBanner() 方法呼叫傳遞到 WriteSubsitution() 方法的 RenderBannerInternal() 方法 HttpResponse.WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="c4438-152">RenderBannerInternal() 方法是私用方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="c4438-153">這個方法不會公開為 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="c4438-154">隨機 RenderBannerInternal() 方法會從三個橫幅廣告映像的清單傳回一個橫幅廣告映像。</span><span class="sxs-lookup"><span data-stu-id="c4438-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="c4438-155">列出 5 中修改的索引檢視表會說明如何使用 RenderBanner() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="c4438-156">請注意，額外&lt;%@ 匯入 %&gt;指示詞就會包含要匯入 MvcApplication1.Helpers 命名空間之檢視的頂端。</span><span class="sxs-lookup"><span data-stu-id="c4438-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="c4438-157">如果您沒有匯入此命名空間，RenderBanner() 方法將會顯示為 Html 屬性上的方法。</span><span class="sxs-lookup"><span data-stu-id="c4438-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="c4438-158">**列出 5 – Views\Home\Index.aspx （具有 RenderBanner() 方法）**</span><span class="sxs-lookup"><span data-stu-id="c4438-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="c4438-159">當您要求列表 5 中的檢視所呈現的頁面時，每個要求會顯示不同的橫幅廣告 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="c4438-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="c4438-160">頁面會快取，但 RenderBanner() helper 方法以動態方式注入橫幅廣告。</span><span class="sxs-lookup"><span data-stu-id="c4438-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="c4438-161">**圖 2 – 顯示隨機橫幅廣告的索引檢視**</span><span class="sxs-lookup"><span data-stu-id="c4438-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="c4438-163">總結</span><span class="sxs-lookup"><span data-stu-id="c4438-163">Summary</span></span>

<span data-ttu-id="c4438-164">本教學課程將說明如何以動態方式更新快取的頁面中的內容。</span><span class="sxs-lookup"><span data-stu-id="c4438-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="c4438-165">您已學習如何使用 HttpResponse.WriteSubstitution() 方法來啟用無法插入快取的頁面中的動態內容。</span><span class="sxs-lookup"><span data-stu-id="c4438-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="c4438-166">您也學到如何封裝 WriteSubstitution() 方法內的 HTML helper 方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="c4438-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="c4438-167">利用快取盡可能 – 它對 web 應用程式的效能有很大的影響。</span><span class="sxs-lookup"><span data-stu-id="c4438-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="c4438-168">本教學課程所述，您可以利用快取，即使您要在您的網頁中顯示動態內容。</span><span class="sxs-lookup"><span data-stu-id="c4438-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c4438-169">[上一頁](improving-performance-with-output-caching-vb.md)
> [下一頁](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c4438-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
