---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: 將動態內容新增至快取的頁面 (C#) |Microsoft Docs
author: microsoft
description: 了解如何混合使用動態和快取的內容，在相同的頁面。 快取後替代作業可讓您顯示橫幅廣告 o 之類的動態內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 08f680e8d057f47a3f2802b1136edfb00634637d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364141"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="83a45-104">將動態內容新增至快取的頁面 (C#)</span><span class="sxs-lookup"><span data-stu-id="83a45-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="83a45-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="83a45-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="83a45-106">了解如何混合使用動態和快取的內容，在相同的頁面。</span><span class="sxs-lookup"><span data-stu-id="83a45-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="83a45-107">快取後替代作業可讓您顯示動態內容，例如橫幅廣告或新聞項目，內有已輸出快取的頁面。</span><span class="sxs-lookup"><span data-stu-id="83a45-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="83a45-108">利用輸出快取，您可以大幅改善 ASP.NET MVC 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="83a45-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="83a45-109">而不是重新產生頁面每一次要求頁面時，網頁可以產生一次並快取在記憶體中的多個使用者。</span><span class="sxs-lookup"><span data-stu-id="83a45-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="83a45-110">但還有一個問題。</span><span class="sxs-lookup"><span data-stu-id="83a45-110">But there is a problem.</span></span> <span data-ttu-id="83a45-111">如果您需要在網頁中顯示動態內容嗎？</span><span class="sxs-lookup"><span data-stu-id="83a45-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="83a45-112">例如，假設您想要在網頁中顯示橫幅廣告。</span><span class="sxs-lookup"><span data-stu-id="83a45-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="83a45-113">您不想要快取，讓每個使用者會看到相同的廣告橫幅廣告。</span><span class="sxs-lookup"><span data-stu-id="83a45-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="83a45-114">您不會如此一來開始任何賺錢 ！</span><span class="sxs-lookup"><span data-stu-id="83a45-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="83a45-115">幸運的是，沒有簡單的解決方案。</span><span class="sxs-lookup"><span data-stu-id="83a45-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="83a45-116">您可以利用呼叫 ASP.NET framework 的功能*快取後替代*。</span><span class="sxs-lookup"><span data-stu-id="83a45-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="83a45-117">快取後替代作業可讓您取代已經在記憶體中快取的頁面中的動態內容。</span><span class="sxs-lookup"><span data-stu-id="83a45-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="83a45-118">一般來說，當您使用 [OutputCache] 屬性來輸出快取的頁面，頁面會快取伺服器和用戶端 （網頁瀏覽器） 中。</span><span class="sxs-lookup"><span data-stu-id="83a45-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="83a45-119">當您使用快取後替代時，則是只能在伺服器上快取頁面。</span><span class="sxs-lookup"><span data-stu-id="83a45-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="83a45-120">使用快取後替代</span><span class="sxs-lookup"><span data-stu-id="83a45-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="83a45-121">使用快取後替代作業需要兩個步驟。</span><span class="sxs-lookup"><span data-stu-id="83a45-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="83a45-122">首先，您需要定義傳回字串，表示您想要在快取的頁面中顯示的動態內容的方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="83a45-123">接下來，您可以呼叫 HttpResponse.WriteSubstitution() 方法，將插入頁面的動態內容。</span><span class="sxs-lookup"><span data-stu-id="83a45-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="83a45-124">例如，假設您想要隨機快取的頁面中顯示不同的新聞項目。</span><span class="sxs-lookup"><span data-stu-id="83a45-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="83a45-125">列表 1 中的類別會公開單一方法，名為 RenderNews() 隨機的三個新項目清單會傳回一個新聞項目。</span><span class="sxs-lookup"><span data-stu-id="83a45-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="83a45-126">**列表 1 – Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="83a45-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="83a45-127">若要利用快取後替代，您可以呼叫 HttpResponse.WriteSubstitution(); 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="83a45-128">WriteSubstitution() 方法會設定以取代動態內容的快取的頁面區域的程式碼。</span><span class="sxs-lookup"><span data-stu-id="83a45-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="83a45-129">WriteSubstitution() 方法用來在 列表 2 中的檢視中顯示隨機的新聞項目。</span><span class="sxs-lookup"><span data-stu-id="83a45-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="83a45-130">**列表 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="83a45-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="83a45-131">RenderNews 方法會傳遞至 WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="83a45-132">請注意，不呼叫 RenderNews 方法 （有沒有括號）。</span><span class="sxs-lookup"><span data-stu-id="83a45-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="83a45-133">改為參考，這個方法會傳遞至 WriteSubstitution()。</span><span class="sxs-lookup"><span data-stu-id="83a45-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="83a45-134">[索引] 檢視會快取。</span><span class="sxs-lookup"><span data-stu-id="83a45-134">The Index view is cached.</span></span> <span data-ttu-id="83a45-135">列表 3 中的控制站會傳回檢視。</span><span class="sxs-lookup"><span data-stu-id="83a45-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="83a45-136">請注意，會導致快取為 60 秒的 [索引] 檢視的 [OutputCache] 屬性裝飾 index （） 動作。</span><span class="sxs-lookup"><span data-stu-id="83a45-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="83a45-137">**列表 3 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="83a45-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="83a45-138">即使索引檢視會快取，當您要求 [索引] 頁面時，會顯示不同的隨機新聞項目。</span><span class="sxs-lookup"><span data-stu-id="83a45-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="83a45-139">當您要求 索引 頁面時，頁面所顯示的時間就不會變更 （請參閱 圖 1） 的 60 秒。</span><span class="sxs-lookup"><span data-stu-id="83a45-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="83a45-140">時間不會變更的事實證明，頁面會快取。</span><span class="sxs-lookup"><span data-stu-id="83a45-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="83a45-141">不過，將內容插入 WriteSubstitution() 方法 – 隨機的新聞項目 – 變更每個要求。</span><span class="sxs-lookup"><span data-stu-id="83a45-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="83a45-142">**圖 1 – 插入快取的頁面中的動態的新聞項目**</span><span class="sxs-lookup"><span data-stu-id="83a45-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="83a45-144">協助程式方法中使用快取後替代</span><span class="sxs-lookup"><span data-stu-id="83a45-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="83a45-145">利用快取後替代作業更簡單的方法是封裝 WriteSubstitution() 方法，在自訂的協助程式方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="83a45-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="83a45-146">Helper 方法，在 列表 4 說明這種方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="83a45-147">**列表 4 – AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="83a45-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="83a45-148">列表 4 包含靜態類別會公開兩種方法： RenderBanner() 和 RenderBannerInternal()。</span><span class="sxs-lookup"><span data-stu-id="83a45-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="83a45-149">RenderBanner() 方法代表實際的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="83a45-150">此方法會擴充標準的 ASP.NET MVC HtmlHelper 類別，可讓您檢視就像任何其他的 helper 方法中呼叫 Html.RenderBanner()。</span><span class="sxs-lookup"><span data-stu-id="83a45-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="83a45-151">RenderBanner() 方法呼叫將 RenderBannerInternal() 方法傳遞至 WriteSubsitution() 方法 HttpResponse.WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="83a45-152">RenderBannerInternal() 方法是私用方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="83a45-153">這個方法不會公開為 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="83a45-154">隨機 RenderBannerInternal() 方法會從三個橫幅廣告映像的清單傳回一個橫幅廣告影像。</span><span class="sxs-lookup"><span data-stu-id="83a45-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="83a45-155">修改過的索引檢視表 5 中會說明如何使用 RenderBanner() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="83a45-156">請注意，額外&lt;匯入 %@ %&gt;指示詞就會包含要匯入 MvcApplication1.Helpers 命名空間之檢視的頂端。</span><span class="sxs-lookup"><span data-stu-id="83a45-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="83a45-157">如果您不願匯入此命名空間，RenderBanner() 方法將不再顯示為 Html 屬性中的方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="83a45-158">**列表 5 – Views\Home\Index.aspx （含 RenderBanner() 方法）**</span><span class="sxs-lookup"><span data-stu-id="83a45-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="83a45-159">當您要求表 5] 中檢視轉譯的頁面時，不同的橫幅廣告會顯示與每個要求 （請參閱 [圖 2）。</span><span class="sxs-lookup"><span data-stu-id="83a45-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="83a45-160">頁面會快取，但橫幅廣告插入動態 RenderBanner() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="83a45-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="83a45-161">**圖 2 – 顯示隨機橫幅廣告的 索引 檢視**</span><span class="sxs-lookup"><span data-stu-id="83a45-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="83a45-163">總結</span><span class="sxs-lookup"><span data-stu-id="83a45-163">Summary</span></span>

<span data-ttu-id="83a45-164">本教學課程說明如何以動態方式更新快取的頁面中的內容。</span><span class="sxs-lookup"><span data-stu-id="83a45-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="83a45-165">您已了解如何使用 HttpResponse.WriteSubstitution() 方法來啟用快取的頁面中插入的動態內容。</span><span class="sxs-lookup"><span data-stu-id="83a45-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="83a45-166">您也學到如何封裝內的 HTML 協助程式方法 WriteSubstitution() 方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="83a45-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="83a45-167">利用快取盡可能 – 它可以對 web 應用程式的效能有顯著的影響。</span><span class="sxs-lookup"><span data-stu-id="83a45-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="83a45-168">本教學課程中所述，您可以利用快取即使在您需要在頁面中顯示動態內容。</span><span class="sxs-lookup"><span data-stu-id="83a45-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="83a45-169">[上一頁](improving-performance-with-output-caching-cs.md)
> [下一頁](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="83a45-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
