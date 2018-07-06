---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: 改善效能，使用輸出快取 (VB) |Microsoft Docs
author: microsoft
description: 在本教學課程中，您將了解如何大幅改善您的 ASP.NET MVC web 應用程式的效能利用輸出快取。 您...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 72d73067f6f7dabe4644a35c8d9462bfbc7a93ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807326"
---
<a name="improving-performance-with-output-caching-vb"></a><span data-ttu-id="a301e-104">改善效能，使用輸出快取 (VB)</span><span class="sxs-lookup"><span data-stu-id="a301e-104">Improving Performance with Output Caching (VB)</span></span>
====================
<span data-ttu-id="a301e-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a301e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a301e-106">在本教學課程中，您將了解如何大幅改善您的 ASP.NET MVC web 應用程式的效能利用輸出快取。</span><span class="sxs-lookup"><span data-stu-id="a301e-106">In this tutorial, you learn how you can dramatically improve the performance of your ASP.NET MVC web applications by taking advantage of output caching.</span></span> <span data-ttu-id="a301e-107">您了解如何快取，因此不需要相同的內容，這是要建立新的使用者叫用動作的每個階段，從控制器動作傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="a301e-107">You learn how to cache the result returned from a controller action so that the same content does not need to be created each and every time a new user invokes the action.</span></span>


<span data-ttu-id="a301e-108">本教學課程的目標是要說明如何大幅改善效能的 ASP.NET MVC 應用程式利用輸出快取。</span><span class="sxs-lookup"><span data-stu-id="a301e-108">The goal of this tutorial is to explain how you can dramatically improve the performance of an ASP.NET MVC application by taking advantage of the output cache.</span></span> <span data-ttu-id="a301e-109">輸出快取可讓您快取控制器動作傳回的內容。</span><span class="sxs-lookup"><span data-stu-id="a301e-109">The output cache enables you to cache the content returned by a controller action.</span></span> <span data-ttu-id="a301e-110">如此一來，相同的內容不會不需要在每次叫用相同的控制器動作產生。</span><span class="sxs-lookup"><span data-stu-id="a301e-110">That way, the same content does not need to be generated each and every time the same controller action is invoked.</span></span>

<span data-ttu-id="a301e-111">例如，試想一下，ASP.NET MVC 應用程式，會在名為索引檢視中顯示一份資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a301e-111">Imagine, for example, that your ASP.NET MVC application displays a list of database records in a view named Index.</span></span> <span data-ttu-id="a301e-112">一般來說，每一次使用者叫用傳回 [索引] 檢視中，控制器動作的一組資料庫記錄必須擷取從資料庫執行資料庫查詢。</span><span class="sxs-lookup"><span data-stu-id="a301e-112">Normally, each and every time that a user invokes the controller action that returns the Index view, the set of database records must be retrieved from the database by executing a database query.</span></span>

<span data-ttu-id="a301e-113">如果相反地，您可以利用的輸出快取時您可以避免執行資料庫查詢，每次任何使用者叫用相同的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="a301e-113">If, on the other hand, you take advantage of the output cache then you can avoid executing a database query every time any user invokes the same controller action.</span></span> <span data-ttu-id="a301e-114">檢視可以擷取從快取，而不是從控制器動作重新產生。</span><span class="sxs-lookup"><span data-stu-id="a301e-114">The view can be retrieved from the cache instead of being regenerated from the controller action.</span></span> <span data-ttu-id="a301e-115">您不必執行備援快取啟用在伺服器上運作。</span><span class="sxs-lookup"><span data-stu-id="a301e-115">Caching enables you to avoid performing redundant work on the server.</span></span>

#### <a name="enabling-output-caching"></a><span data-ttu-id="a301e-116">啟用輸出快取</span><span class="sxs-lookup"><span data-stu-id="a301e-116">Enabling Output Caching</span></span>

<span data-ttu-id="a301e-117">啟用輸出快取加&lt;OutputCache&gt;屬性設定為個別的控制器動作或整個控制器類別。</span><span class="sxs-lookup"><span data-stu-id="a301e-117">You enable output caching by adding an &lt;OutputCache&gt; attribute to either an individual controller action or an entire controller class.</span></span> <span data-ttu-id="a301e-118">例如，列表 1 中的控制站會公開名為 index （） 的動作。</span><span class="sxs-lookup"><span data-stu-id="a301e-118">For example, the controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="a301e-119">Index （） 動作的輸出快取 10 秒的時間。</span><span class="sxs-lookup"><span data-stu-id="a301e-119">The output of the Index() action is cached for 10 seconds.</span></span>

<span data-ttu-id="a301e-120">**列表 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="a301e-120">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


<span data-ttu-id="a301e-121">在 Beta 版的 ASP.NET MVC 中，輸出快取不適用於之類的 URL [ http://www.MySite.com/ ](http://www.mysite.com/)。</span><span class="sxs-lookup"><span data-stu-id="a301e-121">In the Beta versions of ASP.NET MVC, output caching does not work for a URL like [http://www.MySite.com/](http://www.mysite.com/).</span></span> <span data-ttu-id="a301e-122">相反地，您必須輸入 URL，例如[ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index)。</span><span class="sxs-lookup"><span data-stu-id="a301e-122">Instead, you must enter a URL like [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).</span></span>


<span data-ttu-id="a301e-123">在列表 1 中，index （） 動作的輸出快取 10 秒的時間。</span><span class="sxs-lookup"><span data-stu-id="a301e-123">In Listing 1, the output of the Index() action is cached for 10 seconds.</span></span> <span data-ttu-id="a301e-124">如果您想，您可以指定更長的快取持續時間。</span><span class="sxs-lookup"><span data-stu-id="a301e-124">If you prefer, you can specify a much longer cache duration.</span></span> <span data-ttu-id="a301e-125">比方說，如果您想要快取一天的控制器動作的輸出，然後您可以指定快取持續時間為 86400 秒 (60 秒\*60 分鐘\*24 小時)。</span><span class="sxs-lookup"><span data-stu-id="a301e-125">For example, if you want to cache the output of a controller action for one day then you can specify a cache duration of 86400 seconds (60 seconds \* 60 minutes \* 24 hours).</span></span>

<span data-ttu-id="a301e-126">沒有內容不保證會進行快取，您指定的時間長度。</span><span class="sxs-lookup"><span data-stu-id="a301e-126">There is no guarantee that content will be cached for the amount of time that you specify.</span></span> <span data-ttu-id="a301e-127">低記憶體資源時，快取會自動啟動收回的內容。</span><span class="sxs-lookup"><span data-stu-id="a301e-127">When memory resources become low, the cache starts evicting content automatically.</span></span>

<span data-ttu-id="a301e-128">列表 1 中的主控制器會傳回列表 2 中的 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="a301e-128">The Home controller in Listing 1 returns the Index view in Listing 2.</span></span> <span data-ttu-id="a301e-129">沒有特別關於此檢視。</span><span class="sxs-lookup"><span data-stu-id="a301e-129">There is nothing special about this view.</span></span> <span data-ttu-id="a301e-130">索引 檢視只會顯示目前的時間 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="a301e-130">The Index view simply displays the current time (see Figure 1).</span></span>

<span data-ttu-id="a301e-131">**列表 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a301e-131">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

<span data-ttu-id="a301e-132">**圖 1 – 快取 索引 檢視**</span><span class="sxs-lookup"><span data-stu-id="a301e-132">**Figure 1 – Cached Index view**</span></span>

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

<span data-ttu-id="a301e-134">如果您叫用 index （） 動作數次在您的瀏覽器的網址列中輸入 URL /Home/索引，並重複按瀏覽器中的 [重新整理/重新載入] 按鈕，然後 [索引] 檢視所顯示的時間不會變更 10 秒的時間。</span><span class="sxs-lookup"><span data-stu-id="a301e-134">If you invoke the Index() action multiple times by entering the URL /Home/Index in the address bar of your browser and hitting the Refresh/Reload button in your browser repeatedly, then the time displayed by the Index view won't change for 10 seconds.</span></span> <span data-ttu-id="a301e-135">因為檢視快取，則會顯示相同的時間。</span><span class="sxs-lookup"><span data-stu-id="a301e-135">The same time is displayed because the view is cached.</span></span>

<span data-ttu-id="a301e-136">請務必了解每一位使用者造訪您的應用程式相同的檢視，會快取。</span><span class="sxs-lookup"><span data-stu-id="a301e-136">It is important to understand that the same view is cached for everyone who visits your application.</span></span> <span data-ttu-id="a301e-137">Index （） 動作會叫用的人會收到相同的快取的版本的 [索引] 檢視。</span><span class="sxs-lookup"><span data-stu-id="a301e-137">Anyone who invokes the Index() action will get the same cached version of the Index view.</span></span> <span data-ttu-id="a301e-138">這表示已大幅減少網頁伺服器必須執行來處理索引檢視的工作數量。</span><span class="sxs-lookup"><span data-stu-id="a301e-138">This means that the amount of work that the web server must perform to serve the Index view is dramatically reduced.</span></span>

<span data-ttu-id="a301e-139">列表 2 中的檢視會做的動作很簡單的項目。</span><span class="sxs-lookup"><span data-stu-id="a301e-139">The view in Listing 2 happens to be doing something really simple.</span></span> <span data-ttu-id="a301e-140">檢視只會顯示目前的時間。</span><span class="sxs-lookup"><span data-stu-id="a301e-140">The view just displays the current time.</span></span> <span data-ttu-id="a301e-141">不過，您可以如同輕鬆地快取會顯示一組資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="a301e-141">However, you could just as easily cache a view that displays a set of database records.</span></span> <span data-ttu-id="a301e-142">在此情況下的一組資料庫記錄就不需要從資料庫擷取每一次叫用傳回的檢視控制器動作。</span><span class="sxs-lookup"><span data-stu-id="a301e-142">In that case, the set of database records would not need to be retrieved from the database each and every time the controller action that returns the view is invoked.</span></span> <span data-ttu-id="a301e-143">快取可以減少您 web 伺服器和資料庫伺服器必須執行的工作數量。</span><span class="sxs-lookup"><span data-stu-id="a301e-143">Caching can reduce the amount of work that both your web server and database server must perform.</span></span>

<span data-ttu-id="a301e-144">不使用 頁面&lt;%@ OutputCache %&gt; MVC 檢視中的指示詞。</span><span class="sxs-lookup"><span data-stu-id="a301e-144">Don't use the page &lt;%@ OutputCache %&gt; directive in an MVC view.</span></span> <span data-ttu-id="a301e-145">這個指示詞透過高度，從 Web Form 世界，不應用於 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a301e-145">This directive is bleeding over from the Web Forms world and should not be used in an ASP.NET MVC application.</span></span> 

#### <a name="where-content-is-cached"></a><span data-ttu-id="a301e-146">會在快取內容</span><span class="sxs-lookup"><span data-stu-id="a301e-146">Where Content is Cached</span></span>

<span data-ttu-id="a301e-147">根據預設，當您使用&lt;OutputCache&gt;屬性，內容會快取中三個位置： 網頁伺服器、 任何 proxy 伺服器和網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="a301e-147">By default, when you use the &lt;OutputCache&gt; attribute, content is cached in three locations: the web server, any proxy servers, and the web browser.</span></span> <span data-ttu-id="a301e-148">您可以完全內容快取所在藉由修改的 Location 屬性來控制&lt;OutputCache&gt;屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-148">You can control exactly where content is cached by modifying the Location property of the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="a301e-149">您可以設定 [位置] 屬性以下列值之一：</span><span class="sxs-lookup"><span data-stu-id="a301e-149">You can set the Location property to any one of the following values:</span></span>

> <span data-ttu-id="a301e-150">·任何</span><span class="sxs-lookup"><span data-stu-id="a301e-150">· Any</span></span>
> 
> <span data-ttu-id="a301e-151">·用戶端</span><span class="sxs-lookup"><span data-stu-id="a301e-151">· Client</span></span>
> 
> <span data-ttu-id="a301e-152">·下游</span><span class="sxs-lookup"><span data-stu-id="a301e-152">· Downstream</span></span>
> 
> <span data-ttu-id="a301e-153">·伺服器</span><span class="sxs-lookup"><span data-stu-id="a301e-153">· Server</span></span>
> 
> <span data-ttu-id="a301e-154">·None</span><span class="sxs-lookup"><span data-stu-id="a301e-154">· None</span></span>
> 
> <span data-ttu-id="a301e-155">·ServerAndClient</span><span class="sxs-lookup"><span data-stu-id="a301e-155">· ServerAndClient</span></span>


<span data-ttu-id="a301e-156">根據預設，[位置] 屬性有值 Any。</span><span class="sxs-lookup"><span data-stu-id="a301e-156">By default, the Location property has the value Any.</span></span> <span data-ttu-id="a301e-157">不過，有情況下您可能想要只在瀏覽器上，或只能在伺服器上的快取。</span><span class="sxs-lookup"><span data-stu-id="a301e-157">However, there are situations in which you might want to cache only on the browser or only on the server.</span></span> <span data-ttu-id="a301e-158">比方說，如果您要快取的每個使用者個人化資訊然後您應該快取伺服器上的資訊。</span><span class="sxs-lookup"><span data-stu-id="a301e-158">For example, if you are caching information that is personalized for each user then you should not cache the information on the server.</span></span> <span data-ttu-id="a301e-159">如果您要顯示不同的資訊給不同的使用者接著您應該快取的資訊只在用戶端。</span><span class="sxs-lookup"><span data-stu-id="a301e-159">If you are displaying different information to different users then you should cache the information only on the client.</span></span>

<span data-ttu-id="a301e-160">例如，列表 3 中的控制站會公開名為 GetName() 傳回目前的使用者名稱的動作。</span><span class="sxs-lookup"><span data-stu-id="a301e-160">For example, the controller in Listing 3 exposes an action named GetName() that returns the current user name.</span></span> <span data-ttu-id="a301e-161">如果 Jack 登入網站，且叫用 GetName() 動作然後動作傳回的字串"Hi Jack"。</span><span class="sxs-lookup"><span data-stu-id="a301e-161">If Jack logs into the website and invokes the GetName() action then the action returns the string "Hi Jack".</span></span> <span data-ttu-id="a301e-162">如果接下來，Jill 登入網站，並叫用 GetName() 動作然後她也會取得字串"Hi Jack"。</span><span class="sxs-lookup"><span data-stu-id="a301e-162">If, subsequently, Jill logs into the website and invokes the GetName() action then she also will get the string "Hi Jack".</span></span> <span data-ttu-id="a301e-163">Jack 一開始會叫用的控制器動作之後，所有使用者的 web 伺服器上會快取字串。</span><span class="sxs-lookup"><span data-stu-id="a301e-163">The string is cached on the web server for all users after Jack initially invokes the controller action.</span></span>

<span data-ttu-id="a301e-164">**列表 3 – Controllers\BadUserController.vb**</span><span class="sxs-lookup"><span data-stu-id="a301e-164">**Listing 3 – Controllers\BadUserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

<span data-ttu-id="a301e-165">最有可能列表 3 中的控制站無法您想要的方式。</span><span class="sxs-lookup"><span data-stu-id="a301e-165">Most likely, the controller in Listing 3 does not work the way that you want.</span></span> <span data-ttu-id="a301e-166">您不想要顯示訊息"Hi Jack"給 Jill。</span><span class="sxs-lookup"><span data-stu-id="a301e-166">You don't want to display the message "Hi Jack" to Jill.</span></span>

<span data-ttu-id="a301e-167">您應該永遠不會快取伺服器快取中的個人化的內容。</span><span class="sxs-lookup"><span data-stu-id="a301e-167">You should never cache personalized content in the server cache.</span></span> <span data-ttu-id="a301e-168">不過，您可能要快取中的瀏覽器快取以改善效能的個人化的內容。</span><span class="sxs-lookup"><span data-stu-id="a301e-168">However, you might want to cache the personalized content in the browser cache to improve performance.</span></span> <span data-ttu-id="a301e-169">如果您快取內容，在瀏覽器中，且使用者叫用相同的控制器動作多次，內容可以擷取從瀏覽器快取，而非伺服器。</span><span class="sxs-lookup"><span data-stu-id="a301e-169">If you cache content in the browser, and a user invokes the same controller action multiple times, then the content can be retrieved from the browser cache instead of the server.</span></span>

<span data-ttu-id="a301e-170">在 列表 4 中的已修改的控制器會快取 GetName() 動作的輸出。</span><span class="sxs-lookup"><span data-stu-id="a301e-170">The modified controller in Listing 4 caches the output of the GetName() action.</span></span> <span data-ttu-id="a301e-171">不過，只有瀏覽器和伺服器上不會快取內容。</span><span class="sxs-lookup"><span data-stu-id="a301e-171">However, the content is cached only on the browser and not on the server.</span></span> <span data-ttu-id="a301e-172">這樣一來，當多位使用者叫用 GetName() 方法時，每位人員取得他們自己的使用者名稱和不可為另一個人的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="a301e-172">That way, when multiple users invoke the GetName() method, each person gets their own user name and not another person's user name.</span></span>

<span data-ttu-id="a301e-173">**列表 4 – Controllers\UserController.vb**</span><span class="sxs-lookup"><span data-stu-id="a301e-173">**Listing 4 – Controllers\UserController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

<span data-ttu-id="a301e-174">請注意， &lt;OutputCache&gt;列表 4 中的屬性包括 [位置] 屬性設定為值 OutputCacheLocation.Client。</span><span class="sxs-lookup"><span data-stu-id="a301e-174">Notice that the &lt;OutputCache&gt; attribute in Listing 4 includes a Location property set to the value OutputCacheLocation.Client.</span></span> <span data-ttu-id="a301e-175">&lt;OutputCache&gt;屬性也包含 NoStore 屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-175">The &lt;OutputCache&gt; attribute also includes a NoStore property.</span></span> <span data-ttu-id="a301e-176">NoStore 屬性用來通知 proxy 伺服器和瀏覽器，它們不應該儲存在快取內容的永久複本。</span><span class="sxs-lookup"><span data-stu-id="a301e-176">The NoStore property is used to inform proxy servers and browsers that they should not store a permanent copy of the cached content.</span></span>

#### <a name="varying-the-output-cache"></a><span data-ttu-id="a301e-177">不同的輸出快取</span><span class="sxs-lookup"><span data-stu-id="a301e-177">Varying the Output Cache</span></span>

<span data-ttu-id="a301e-178">在某些情況下，您可以使用不同的快取的版本的完全相同的內容。</span><span class="sxs-lookup"><span data-stu-id="a301e-178">In some situations, you might want different cached versions of the very same content.</span></span> <span data-ttu-id="a301e-179">例如，假設您要建立主要/詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="a301e-179">Imagine, for example, that you are creating a master/detail page.</span></span> <span data-ttu-id="a301e-180">主版頁面會顯示一份電影標題。</span><span class="sxs-lookup"><span data-stu-id="a301e-180">The master page displays a list of movie titles.</span></span> <span data-ttu-id="a301e-181">當您按一下標題時，您會取得所選的電影詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a301e-181">When you click a title, you get details for the selected movie.</span></span>

<span data-ttu-id="a301e-182">如果您要快取的詳細資料頁面，則相同電影的詳細資料會顯示在您按一下不論哪一個電影。</span><span class="sxs-lookup"><span data-stu-id="a301e-182">If you cache the details page, then the details for the same movie will be displayed no matter which movie you click.</span></span> <span data-ttu-id="a301e-183">未來的所有使用者，將會顯示第一個使用者所選取的第一部電影。</span><span class="sxs-lookup"><span data-stu-id="a301e-183">The first movie selected by the first user will be displayed to all future users.</span></span>

<span data-ttu-id="a301e-184">您可以修正這個問題，利用 VaryByParam 屬性&lt;OutputCache&gt;屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-184">You can fix this problem by taking advantage of the VaryByParam property of the &lt;OutputCache&gt; attribute.</span></span> <span data-ttu-id="a301e-185">此屬性可讓您建立的相同內容時的表單參數的不同快取的版本，或查詢字串參數而有所不同。</span><span class="sxs-lookup"><span data-stu-id="a301e-185">This property enables you to create different cached versions of the very same content when a form parameter or query string parameter varies.</span></span>

<span data-ttu-id="a301e-186">例如，列表 5 中的控制站會公開名為 Master() 和 Details() 的兩個動作。</span><span class="sxs-lookup"><span data-stu-id="a301e-186">For example, the controller in Listing 5 exposes two actions named Master() and Details().</span></span> <span data-ttu-id="a301e-187">Master() 動作會傳回一份電影標題及 Details() 動作傳回選取的影片的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a301e-187">The Master() action returns a list of movie titles and the Details() action returns the details for the selected movie.</span></span>

<span data-ttu-id="a301e-188">**列表 5 – Controllers\MoviesController.vb**</span><span class="sxs-lookup"><span data-stu-id="a301e-188">**Listing 5 – Controllers\MoviesController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

<span data-ttu-id="a301e-189">Master() 動作包括 VaryByParam 屬性具有值"none"。</span><span class="sxs-lookup"><span data-stu-id="a301e-189">The Master() action includes a VaryByParam property with the value "none".</span></span> <span data-ttu-id="a301e-190">當叫用動作，Master() 相同的快取版本的主要檢視會傳回。</span><span class="sxs-lookup"><span data-stu-id="a301e-190">When the Master() action is invoked, the same cached version of the Master view is returned.</span></span> <span data-ttu-id="a301e-191">任何形式的參數或查詢字串參數被忽略 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="a301e-191">Any form parameters or query string parameters are ignored (see Figure 2).</span></span>

<span data-ttu-id="a301e-192">**圖 2 – /Movies/Master 檢視**</span><span class="sxs-lookup"><span data-stu-id="a301e-192">**Figure 2 – The /Movies/Master view**</span></span>

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

<span data-ttu-id="a301e-194">**圖 3 – / Movies/詳細資料檢視**</span><span class="sxs-lookup"><span data-stu-id="a301e-194">**Figure 3 – The /Movies/Details view**</span></span>

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

<span data-ttu-id="a301e-196">Details() 動作包含值為"Id"VaryByParam 屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-196">The Details() action includes a VaryByParam property with the value "Id".</span></span> <span data-ttu-id="a301e-197">當不同的 Id 參數的值會傳遞至控制器動作時，會產生不同的快取的版本的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="a301e-197">When different values of the Id parameter are passed to the controller action, different cached versions of the Details view are generated.</span></span>

<span data-ttu-id="a301e-198">它是一定要了解使用 VaryByParam 屬性會導致多個快取並不是較少。</span><span class="sxs-lookup"><span data-stu-id="a301e-198">It is important to understand that using the VaryByParam property results in more caching and not less.</span></span> <span data-ttu-id="a301e-199">不同的快取的版本的詳細資料檢視會建立每個不同版本的 Id 參數。</span><span class="sxs-lookup"><span data-stu-id="a301e-199">A different cached version of the Details view is created for each different version of the Id parameter.</span></span>

<span data-ttu-id="a301e-200">您可以設定 VaryByParam 屬性為下列值：</span><span class="sxs-lookup"><span data-stu-id="a301e-200">You can set the VaryByParam property to the following values:</span></span>

> <span data-ttu-id="a301e-201">\* = 每當表單或查詢字串參數而異，請建立不同的快取的版本。</span><span class="sxs-lookup"><span data-stu-id="a301e-201">\* = Create a different cached version whenever a form or query string parameter varies.</span></span>
> 
> <span data-ttu-id="a301e-202">none = 永不建立不同的快取的版本</span><span class="sxs-lookup"><span data-stu-id="a301e-202">none = Never create different cached versions</span></span>
> 
> <span data-ttu-id="a301e-203">以分號的參數清單 = 建立不同的快取的版本，每當任何表單或查詢字串中的參數清單不同</span><span class="sxs-lookup"><span data-stu-id="a301e-203">Semicolon list of parameters = Create different cached versions whenever any of the form or query string parameters in the list varies</span></span>


#### <a name="creating-a-cache-profile"></a><span data-ttu-id="a301e-204">建立快取設定檔</span><span class="sxs-lookup"><span data-stu-id="a301e-204">Creating a Cache Profile</span></span>

<span data-ttu-id="a301e-205">除了設定輸出快取屬性修改的屬性&lt;OutputCache&gt;屬性，您可以建立快取設定檔中的 web 組態 (web.config) 檔。</span><span class="sxs-lookup"><span data-stu-id="a301e-205">As an alternative to configuring output cache properties by modifying properties of the &lt;OutputCache&gt; attribute, you can create a cache profile in the web configuration (web.config) file.</span></span> <span data-ttu-id="a301e-206">在 web 組態檔中建立快取設定檔提供了幾項重要的優點。</span><span class="sxs-lookup"><span data-stu-id="a301e-206">Creating a cache profile in the web configuration file offers a couple of important advantages.</span></span>

<span data-ttu-id="a301e-207">首先，藉由設定輸出快取 web 組態檔中，您可以控制如何控制器動作快取內容，在單一中央位置。</span><span class="sxs-lookup"><span data-stu-id="a301e-207">First, by configuring output caching in the web configuration file, you can control how controller actions cache content in one central location.</span></span> <span data-ttu-id="a301e-208">您可以建立一個快取設定檔，並將設定檔套用至數個控制器或控制器動作。</span><span class="sxs-lookup"><span data-stu-id="a301e-208">You can create one cache profile and apply the profile to several controllers or controller actions.</span></span>

<span data-ttu-id="a301e-209">第二，您可以修改 web 組態檔不需要重新編譯您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a301e-209">Second, you can modify the web configuration file without recompiling your application.</span></span> <span data-ttu-id="a301e-210">如果您需要停用快取已部署到生產環境的應用程式，您可以只修改 web 組態檔中定義的快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="a301e-210">If you need to disable caching for an application that has already been deployed to production, then you can simply modify the cache profiles defined in the web configuration file.</span></span> <span data-ttu-id="a301e-211">Web 組態檔的任何變更會自動偵測並套用。</span><span class="sxs-lookup"><span data-stu-id="a301e-211">Any changes to the web configuration file will be detected automatically and applied.</span></span>

<span data-ttu-id="a301e-212">例如，&lt;快取&gt;列表 6 中的 web 組態區段會定義名為 Cache1Hour 快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="a301e-212">For example, the &lt;caching&gt; web configuration section in Listing 6 defines a cache profile named Cache1Hour.</span></span> <span data-ttu-id="a301e-213">&lt;快取&gt;一節必須出現在&lt;system.web&gt; web 組態檔區段。</span><span class="sxs-lookup"><span data-stu-id="a301e-213">The &lt;caching&gt; section must appear within the &lt;system.web&gt; section of a web configuration file.</span></span>

<span data-ttu-id="a301e-214">**列表 6 – web.config 的快取 > 章節**</span><span class="sxs-lookup"><span data-stu-id="a301e-214">**Listing 6 – Caching section for web.config**</span></span>

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

<span data-ttu-id="a301e-215">列表 7 中的控制站會說明如何將 Cache1Hour 設定檔套用至控制器動作與&lt;OutputCache&gt;屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-215">The controller in Listing 7 illustrates how you can apply the Cache1Hour profile to a controller action with the &lt;OutputCache&gt; attribute.</span></span>

<span data-ttu-id="a301e-216">**列表 7 – Controllers\ProfileController.vb**</span><span class="sxs-lookup"><span data-stu-id="a301e-216">**Listing 7 – Controllers\ProfileController.vb**</span></span>

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

<span data-ttu-id="a301e-217">如果您叫用由 列表 7 中的控制器的 index （） 動作則會傳回相同的時間 （1 小時）。</span><span class="sxs-lookup"><span data-stu-id="a301e-217">If you invoke the Index() action exposed by the controller in Listing 7 then the same time will be returned for 1 hour.</span></span>

#### <a name="summary"></a><span data-ttu-id="a301e-218">總結</span><span class="sxs-lookup"><span data-stu-id="a301e-218">Summary</span></span>

<span data-ttu-id="a301e-219">輸出快取可以提供非常簡單的方法，大幅改善您的 ASP.NET MVC 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="a301e-219">Output caching provides you with a very easy method of dramatically improving the performance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="a301e-220">在本教學課程中，您已了解如何使用&lt;OutputCache&gt;快取的控制器動作輸出的屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-220">In this tutorial, you learned how to use the &lt;OutputCache&gt; attribute to cache the output of controller actions.</span></span> <span data-ttu-id="a301e-221">您也學到如何修改的屬性&lt;OutputCache&gt;屬性，例如修改內容取得快取持續時間和 VaryByParam 屬性。</span><span class="sxs-lookup"><span data-stu-id="a301e-221">You also learned how to modify properties of the &lt;OutputCache&gt; attribute such as the Duration and VaryByParam properties to modify how content gets cached.</span></span> <span data-ttu-id="a301e-222">最後，您已了解如何在 web 組態檔中定義快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="a301e-222">Finally, you learned how to define cache profiles in the web configuration file.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a301e-223">[上一頁](understanding-action-filters-vb.md)
> [下一頁](adding-dynamic-content-to-a-cached-page-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a301e-223">[Previous](understanding-action-filters-vb.md)
[Next](adding-dynamic-content-to-a-cached-page-vb.md)</span></span>
