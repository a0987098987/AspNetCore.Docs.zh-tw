---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: 新增檢視 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 942d329cf0451101a24da4c3facd38b2813653d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365668"
---
<a name="adding-a-view"></a><span data-ttu-id="41a5c-104">新增檢視</span><span class="sxs-lookup"><span data-stu-id="41a5c-104">Adding a View</span></span>
====================
<span data-ttu-id="41a5c-105">藉由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="41a5c-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="41a5c-106">這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="41a5c-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="41a5c-107">您將建立簡單 web 應用程式，從資料庫讀取與寫入。</span><span class="sxs-lookup"><span data-stu-id="41a5c-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="41a5c-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。</span><span class="sxs-lookup"><span data-stu-id="41a5c-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="41a5c-109">這一節我們要看看我們如何有 HelloWorldController 類別完全封裝給用戶端產生 HTML 回應中使用檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="41a5c-109">In this section we are going to look at how we can have our HelloWorldController class use a View template file to cleanly encapsulate generating HTML responses back to a client.</span></span>

<span data-ttu-id="41a5c-110">現在就開始使用檢視範本搭配我們 Index 方法。</span><span class="sxs-lookup"><span data-stu-id="41a5c-110">Let's start by using a View template with our Index method.</span></span> <span data-ttu-id="41a5c-111">我們的方法呼叫索引，且位於 HelloWorldController。</span><span class="sxs-lookup"><span data-stu-id="41a5c-111">Our method is called Index and it's in the HelloWorldController.</span></span> <span data-ttu-id="41a5c-112">目前我們 index （） 方法傳回的訊息是控制器類別內的硬式編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="41a5c-112">Currently our Index() method returns a string with a message that is hardcoded within the Controller class.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

<span data-ttu-id="41a5c-113">讓我們現在變更 Index 方法，而是看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="41a5c-113">Let's now change the Index method to instead look like this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

<span data-ttu-id="41a5c-114">讓我們現在將檢視範本加入我們，我們可以使用我們的 index （） 方法的專案。</span><span class="sxs-lookup"><span data-stu-id="41a5c-114">Let's now add a View template to our project that we can use for our Index() method.</span></span> <span data-ttu-id="41a5c-115">若要這樣做，請使用 Index 方法中間的某處滑鼠右鍵，按一下 新增檢視...</span><span class="sxs-lookup"><span data-stu-id="41a5c-115">To do this, right-click with your mouse somewhere in the middle of the Index method and click Add View...</span></span>

![影像](getting-started-with-mvc-part3/_static/image1.png)

<span data-ttu-id="41a5c-117">這會顯示 [新增檢視] 對話方塊提供我們一些我們要如何建立可供我們 Index 方法檢視範本的選項。</span><span class="sxs-lookup"><span data-stu-id="41a5c-117">This will bring up the "Add View" dialog which provides us some options for how we want to create a view template that can be used by our Index method.</span></span> <span data-ttu-id="41a5c-118">現在，請勿變更任何項目，並只要按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="41a5c-118">For now, don't change anything, and just click the Add button.</span></span>

<span data-ttu-id="41a5c-119">[![新增檢視對話方塊](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="41a5c-119">[![Add View Dialog](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span></span>

<span data-ttu-id="41a5c-120">按一下 [新增] 後，新的資料夾和新的檔案會出現在方案資料夾中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="41a5c-120">After you click Add, a new folder and a new file will appear in the Solution Folder, as seen here.</span></span> <span data-ttu-id="41a5c-121">我現在會有 [HelloWorld] 資料夾之下的檢視和 Index.aspx 檔案在該資料夾內。</span><span class="sxs-lookup"><span data-stu-id="41a5c-121">I now have a HelloWorld folder under Views, and an Index.aspx file inside that folder.</span></span>

<span data-ttu-id="41a5c-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="41a5c-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span></span>

<span data-ttu-id="41a5c-123">新的索引檔是也已經開啟，而且準備好進行編輯。</span><span class="sxs-lookup"><span data-stu-id="41a5c-123">The new Index file is also already opened and ready for editing.</span></span> <span data-ttu-id="41a5c-124">加入一些文字置於第一個&lt;h2&gt;Index&lt;/h2&gt;喜歡"Hello World"。</span><span class="sxs-lookup"><span data-stu-id="41a5c-124">Add some text under the first &lt;h2&gt;Index&lt;/h2&gt; like "Hello World."</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

<span data-ttu-id="41a5c-125">執行應用程式，並瀏覽[ `http://localhost:xx/HelloWorld` ](http://localhostxx)一次在您的瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="41a5c-125">Run your application and visit [`http://localhost:xx/HelloWorld`](http://localhostxx) again in your browser.</span></span> <span data-ttu-id="41a5c-126">在我們在此範例中的控制器的 Index 方法未執行任何工作，但未呼叫 「 傳回 View() 」 這表示我們想要使用檢視範本檔案來呈現的回應傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="41a5c-126">The Index method in our controller in this example didn't do any work, but did call "return View()" which indicated that we wanted to use a view template file to render a response back to the client.</span></span> <span data-ttu-id="41a5c-127">因為我們未明確指定要使用的檢視範本檔案的名稱，ASP.NET MVC 會預設使用 \Views\HelloWorld 資料夾內的 Index.aspx 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="41a5c-127">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the Index.aspx view file within the \Views\HelloWorld folder.</span></span> <span data-ttu-id="41a5c-128">我們現在看到我們硬式編碼在我們的檢視中的字串。</span><span class="sxs-lookup"><span data-stu-id="41a5c-128">Now we see the string we hard-coded in our View.</span></span>

<span data-ttu-id="41a5c-129">[![索引-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="41a5c-129">[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span></span>

<span data-ttu-id="41a5c-130">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="41a5c-130">Looks pretty good.</span></span> <span data-ttu-id="41a5c-131">不過，請注意，瀏覽器的標題會顯示"Index"，並大的標題在頁面上顯示 「 我 MVC 應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="41a5c-131">However, notice that the Browser's title says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="41a5c-132">讓我們將這些變更。</span><span class="sxs-lookup"><span data-stu-id="41a5c-132">Let's change those.</span></span>

### <a name="changing-views-and-master-pages"></a><span data-ttu-id="41a5c-133">變更檢視表和主版頁面</span><span class="sxs-lookup"><span data-stu-id="41a5c-133">Changing Views and Master Pages</span></span>

<span data-ttu-id="41a5c-134">首先，讓我們變更文字 「 我 MVC 應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="41a5c-134">First, let's change the text "My MVC Application."</span></span> <span data-ttu-id="41a5c-135">該文字會共用，並出現在每一頁。</span><span class="sxs-lookup"><span data-stu-id="41a5c-135">That text is shared and appears on every page.</span></span> <span data-ttu-id="41a5c-136">它實際上不會顯示只有一個位置，在我們的程式碼，即使是在我們的應用程式中的每個頁面上。</span><span class="sxs-lookup"><span data-stu-id="41a5c-136">It actually appears in only one place in our code, even though it's on every page in our app.</span></span> <span data-ttu-id="41a5c-137">移至 /Views/Shared 資料夾，在 [方案總管] 中，開啟 Site.Master 檔。</span><span class="sxs-lookup"><span data-stu-id="41a5c-137">Go to the /Views/Shared folder in the Solution Explorer and open the Site.Master file.</span></span> <span data-ttu-id="41a5c-138">這個檔案稱為主版頁面，而且共用 「 殼層 」 的所有其他網頁使用。</span><span class="sxs-lookup"><span data-stu-id="41a5c-138">This file is called a Master Page and it's the shared "shell" that all our other pages use.</span></span>

<span data-ttu-id="41a5c-139">請注意 ContentPlaceholder"MainContent"一些文字，在此檔案。</span><span class="sxs-lookup"><span data-stu-id="41a5c-139">Notice some text that says ContentPlaceholder "MainContent" in this file.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

<span data-ttu-id="41a5c-140">該預留位置是，您所建立的所有頁面會都顯示，主版頁面中的 「 包裝 」。</span><span class="sxs-lookup"><span data-stu-id="41a5c-140">That placeholder is where all the pages you create will show up, "wrapped" in the master page.</span></span> <span data-ttu-id="41a5c-141">請嘗試變更標題，然後執行您的應用程式，並瀏覽多個頁面。</span><span class="sxs-lookup"><span data-stu-id="41a5c-141">Try changing the title, then run your app and visit multiple pages.</span></span> <span data-ttu-id="41a5c-142">您會發現您的一項變更會出現多個頁面上。</span><span class="sxs-lookup"><span data-stu-id="41a5c-142">You'll notice that your one change appears on multiple pages.</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

<span data-ttu-id="41a5c-143">現在每個頁面上會有主要標題-這是 H1-「 我的 MVC 電影應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="41a5c-143">Now every page will have the Primary Heading - that's H1 - of "My MVC Movie Application."</span></span> <span data-ttu-id="41a5c-144">可處理的白色文字頂端那里跨所有頁面共用。</span><span class="sxs-lookup"><span data-stu-id="41a5c-144">That handles the white text at the top there that's shared across all the pages.</span></span>

<span data-ttu-id="41a5c-145">以下是 Site.Master 看來我們變更標題：</span><span class="sxs-lookup"><span data-stu-id="41a5c-145">Here is the Site.Master in its entirety with our changed title:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

<span data-ttu-id="41a5c-146">現在，讓我們變更索引頁面的標題。</span><span class="sxs-lookup"><span data-stu-id="41a5c-146">Now, let's change the title of the Index page.</span></span>

<span data-ttu-id="41a5c-147">開啟 /HelloWorld/Index.aspx。</span><span class="sxs-lookup"><span data-stu-id="41a5c-147">Open /HelloWorld/Index.aspx.</span></span> <span data-ttu-id="41a5c-148">沒有變更的兩個地方。</span><span class="sxs-lookup"><span data-stu-id="41a5c-148">There's two places to change.</span></span> <span data-ttu-id="41a5c-149">第一次，標題，會出現在瀏覽器中，則次要的標頭-也是 H2-標題。</span><span class="sxs-lookup"><span data-stu-id="41a5c-149">First, the Title that appears in the title of the browser, then the secondary header - that's H2 - as well.</span></span> <span data-ttu-id="41a5c-150">我會使這些每個稍有不同，因此您可以看到的一些程式碼變更應用程式的哪個部分。</span><span class="sxs-lookup"><span data-stu-id="41a5c-150">I'll make them each slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

<span data-ttu-id="41a5c-151">執行您的應用程式，並瀏覽 /Movies。</span><span class="sxs-lookup"><span data-stu-id="41a5c-151">Run your app and visit /Movies.</span></span> <span data-ttu-id="41a5c-152">請注意，瀏覽器標題、 主要標題和次要標題已變更。</span><span class="sxs-lookup"><span data-stu-id="41a5c-152">Notice that the browser title, the primary heading and the secondary headings have changed.</span></span> <span data-ttu-id="41a5c-153">它很容易就能只進行小變更的應用程式中的重大變更對您的檢視。</span><span class="sxs-lookup"><span data-stu-id="41a5c-153">It's easy to make big changes in your app with small changes to your view.</span></span>

<span data-ttu-id="41a5c-154">[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="41a5c-154">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span></span>

<span data-ttu-id="41a5c-155">我們的一些程式 （在此情況下 「 Hello World ！"的 「 資料 」</span><span class="sxs-lookup"><span data-stu-id="41a5c-155">Our little bit of "data" (in this case the "Hello World!"</span></span> <span data-ttu-id="41a5c-156">訊息） 很難透過自動程式化。</span><span class="sxs-lookup"><span data-stu-id="41a5c-156">message) was hard coded though.</span></span> <span data-ttu-id="41a5c-157">我們有 V （檢視），我們有 C （控制站），但沒有 M （模型） 尚未。</span><span class="sxs-lookup"><span data-stu-id="41a5c-157">We've got V (Views) and we've got C (Controllers), but no M (Model) yet.</span></span> <span data-ttu-id="41a5c-158">我們將稍後詳細說明如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="41a5c-158">We'll shortly walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-a-viewmodel"></a><span data-ttu-id="41a5c-159">傳遞 ViewModel</span><span class="sxs-lookup"><span data-stu-id="41a5c-159">Passing a ViewModel</span></span>

<span data-ttu-id="41a5c-160">我們移至資料庫，並討論模型之前，不過，讓我們先討論 「 Viewmodel。 」</span><span class="sxs-lookup"><span data-stu-id="41a5c-160">Before we go to a database and talk about Models, though, let's first talk about "ViewModels."</span></span> <span data-ttu-id="41a5c-161">這些是代表檢視範本需要來呈現 HTML 回應傳回給用戶端的物件。</span><span class="sxs-lookup"><span data-stu-id="41a5c-161">These are objects that represent what a View template requires to render an HTML response back to a client.</span></span> <span data-ttu-id="41a5c-162">它們通常會建立控制器類別傳遞至檢視範本，並只可包含在檢視範本需要的資料與不再。</span><span class="sxs-lookup"><span data-stu-id="41a5c-162">They are typically created and passed by a Controller class to a View template, and should only contain the data that the View template requires - and no more.</span></span>

<span data-ttu-id="41a5c-163">先前具有 HelloWorld 範例中，我們 Welcome() 動作方法所花費的名稱和 numTimes 參數，並將它輸出到瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="41a5c-163">Previously with our HelloWorld sample, our Welcome() action method took a name and a numTimes parameter and output it to the browser.</span></span> <span data-ttu-id="41a5c-164">與其讓控制器繼續直接呈現這個回應，讓我們改為進行小的類別來保存該資料，並接著將它傳遞至轉譯回 HTML 回應使用它的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="41a5c-164">Rather than have the Controller continue to render this response directly,let's instead make a small class to hold that data and then pass it over to a View template to render back the HTML response using it.</span></span> <span data-ttu-id="41a5c-165">如此一來，控制器會關心一件事並檢視範本另一個 – 讓我們可以維護清除 」 關注點分離 」 在我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="41a5c-165">This way the Controller is concerned with one thing and the View template another – enabling us to maintain clean "separation of concerns" within our application.</span></span>

<span data-ttu-id="41a5c-166">回到 HelloWorldController.cs 檔案並加入新的 「 WelcomeViewModel 」 類別，變更您的控制器內的 歡迎使用方法。</span><span class="sxs-lookup"><span data-stu-id="41a5c-166">Return to the HelloWorldController.cs file and add a new "WelcomeViewModel" class and change the Welcome method inside your controller.</span></span> <span data-ttu-id="41a5c-167">以下是完整的 HelloWorldController.cs 與新的類別，在相同的檔案。</span><span class="sxs-lookup"><span data-stu-id="41a5c-167">Here is the complete HelloWorldController.cs with the new class in the same file.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

<span data-ttu-id="41a5c-168">即使是在多行上，我們歡迎畫面的方法其實只有兩個程式碼陳述式。</span><span class="sxs-lookup"><span data-stu-id="41a5c-168">Even though it's on multiple lines, our Welcome method is really only two code statements.</span></span> <span data-ttu-id="41a5c-169">第一個陳述式的 ViewModel 物件，以及第二個傳遞到我們兩個參數封裝在檢視上產生的物件。</span><span class="sxs-lookup"><span data-stu-id="41a5c-169">The first statement packages up our two parameters into a ViewModel object, and the second passes the resulting object onto the View.</span></span>

<span data-ttu-id="41a5c-170">現在我們需要 褖畫惎檢視範本 ！</span><span class="sxs-lookup"><span data-stu-id="41a5c-170">Now we need a Welcome View template!</span></span> <span data-ttu-id="41a5c-171">以滑鼠右鍵按一下該 褖畫惎方法中，然後選取 加入檢視。</span><span class="sxs-lookup"><span data-stu-id="41a5c-171">Right click in the Welcome method and select Add View.</span></span> <span data-ttu-id="41a5c-172">此時，我們會核取 [建立強型別檢視]，並從下拉式清單中選取我們 WelcomeViewModel 類別。</span><span class="sxs-lookup"><span data-stu-id="41a5c-172">This time, we'll check "Create a strongly-typed view" and select our WelcomeViewModel class from the drop down list.</span></span> <span data-ttu-id="41a5c-173">這個新的檢視只會知道 WelcomeViewModels 和任何其他類型的物件。</span><span class="sxs-lookup"><span data-stu-id="41a5c-173">This new view will only know about WelcomeViewModels and no other types of objects.</span></span>

> <span data-ttu-id="41a5c-174">*注意： 您必須編譯一次之後才會出現在下拉式清單中加入您的 WelcomeViewModel。*</span><span class="sxs-lookup"><span data-stu-id="41a5c-174">*NOTE: You'll need to have compiled once after adding your WelcomeViewModel for to show up in the drop down list.*</span></span>


<span data-ttu-id="41a5c-175">以下是您的 [新增檢視] 對話方塊的外觀。</span><span class="sxs-lookup"><span data-stu-id="41a5c-175">Here's what your Add View dialog should look like.</span></span> <span data-ttu-id="41a5c-176">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="41a5c-176">Click the Add button.</span></span> ![新增檢視圈選起來](getting-started-with-mvc-part3/_static/image10.png)

<span data-ttu-id="41a5c-178">新增下列程式碼底下&lt;h2&gt;中新的 Welcome.aspx。</span><span class="sxs-lookup"><span data-stu-id="41a5c-178">Add this code under the &lt;h2&gt; in your new Welcome.aspx.</span></span> <span data-ttu-id="41a5c-179">我們將進行迴圈，並認識使用者說我們應該無限次 ！</span><span class="sxs-lookup"><span data-stu-id="41a5c-179">We'll make a loop and say Hello as many times as the user says we should!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

<span data-ttu-id="41a5c-180">另外請注意，因為您正在輸入，因為我們說過這 WelcomeViewModel 相關檢視 （已婚，記得嗎？），我們得到很有幫助的 Intellisense，每次我們參考我們的模型物件，在以下螢幕擷取畫面所示：</span><span class="sxs-lookup"><span data-stu-id="41a5c-180">Also, notice while you're typing that because we told this View about the WelcomeViewModel (they are married, remember?) that we get helpful Intellisense each time we reference our Model object as seen in the screenshot below:</span></span>

<span data-ttu-id="41a5c-181">[![NumTime 原始程式碼](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="41a5c-181">[![NumTime Source Code](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span></span>

<span data-ttu-id="41a5c-182">執行應用程式，並瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`一次。</span><span class="sxs-lookup"><span data-stu-id="41a5c-182">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` again.</span></span> <span data-ttu-id="41a5c-183">現在我們從 URL 取得資料，則會自動傳遞到控制器，控制器會封裝將資料分成 ViewModel，並將拖曳至我們的檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="41a5c-183">Now we're taking data from the URL, it's passed into our Controller automatically, our Controller packages up the data into a ViewModel and passes that object onto our View.</span></span> <span data-ttu-id="41a5c-184">檢視比對使用者顯示為 HTML 的資料。</span><span class="sxs-lookup"><span data-stu-id="41a5c-184">The View than displays the data as HTML to the user.</span></span>

<span data-ttu-id="41a5c-185">[![歡迎使用-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="41a5c-185">[![Welcome - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span></span>

<span data-ttu-id="41a5c-186">這是一種模型，"M"，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="41a5c-186">Well, that was a kind of an "M" for Model, but not the database kind.</span></span> <span data-ttu-id="41a5c-187">讓我們看什麼我們已了解並建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="41a5c-187">Let's take what we've learned and create a database of Movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="41a5c-188">[上一頁](getting-started-with-mvc-part2.md)
> [下一頁](getting-started-with-mvc-part4.md)</span><span class="sxs-lookup"><span data-stu-id="41a5c-188">[Previous](getting-started-with-mvc-part2.md)
[Next](getting-started-with-mvc-part4.md)</span></span>
