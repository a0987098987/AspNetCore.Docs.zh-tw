---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "加入檢視 |Microsoft 文件"
author: shanselman
description: "這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 8725d054861c857ceac10a42b0cc3f2afe056aea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-view"></a><span data-ttu-id="c11fb-104">加入的檢視</span><span class="sxs-lookup"><span data-stu-id="c11fb-104">Adding a View</span></span>
====================
<span data-ttu-id="c11fb-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c11fb-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c11fb-106">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="c11fb-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c11fb-107">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="c11fb-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c11fb-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="c11fb-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c11fb-109">本節中，我們即將看看如何我們有 HelloWorldController 類別完全封裝產生的 HTML 回應傳回給用戶端使用檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="c11fb-109">In this section we are going to look at how we can have our HelloWorldController class use a View template file to cleanly encapsulate generating HTML responses back to a client.</span></span>

<span data-ttu-id="c11fb-110">讓我們開始使用我們的方法中的索引檢視範本。</span><span class="sxs-lookup"><span data-stu-id="c11fb-110">Let's start by using a View template with our Index method.</span></span> <span data-ttu-id="c11fb-111">我們的方法呼叫的索引，且 HelloWorldController。</span><span class="sxs-lookup"><span data-stu-id="c11fb-111">Our method is called Index and it's in the HelloWorldController.</span></span> <span data-ttu-id="c11fb-112">目前我們 index （） 方法傳回的訊息是在控制器類別內的硬式編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="c11fb-112">Currently our Index() method returns a string with a message that is hardcoded within the Controller class.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

<span data-ttu-id="c11fb-113">讓我們現在變更索引方法，而是看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="c11fb-113">Let's now change the Index method to instead look like this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

<span data-ttu-id="c11fb-114">我們現在將加入檢視範本至受測專案，我們可以使用我們的 index （） 方法。</span><span class="sxs-lookup"><span data-stu-id="c11fb-114">Let's now add a View template to our project that we can use for our Index() method.</span></span> <span data-ttu-id="c11fb-115">若要這樣做，以滑鼠右鍵按一下使用滑鼠中間索引方法的某個位置並按一下 新增檢視...</span><span class="sxs-lookup"><span data-stu-id="c11fb-115">To do this, right-click with your mouse somewhere in the middle of the Index method and click Add View...</span></span>

![影像](getting-started-with-mvc-part3/_static/image1.png)

<span data-ttu-id="c11fb-117">這會顯示我們提供一些選項，我們要如何建立可供我們的方法中的索引檢視表範本的 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="c11fb-117">This will bring up the "Add View" dialog which provides us some options for how we want to create a view template that can be used by our Index method.</span></span> <span data-ttu-id="c11fb-118">現在請不要變更任何項目，並只要按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c11fb-118">For now, don't change anything, and just click the Add button.</span></span>

<span data-ttu-id="c11fb-119">[![新增檢視對話方塊](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="c11fb-119">[![Add View Dialog](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span></span>

<span data-ttu-id="c11fb-120">按一下 [新增] 後，新的資料夾和新的檔案會出現在方案資料夾中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="c11fb-120">After you click Add, a new folder and a new file will appear in the Solution Folder, as seen here.</span></span> <span data-ttu-id="c11fb-121">我現在會有 [HelloWorld] 資料夾之下的檢視表和 Index.aspx 檔案資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c11fb-121">I now have a HelloWorld folder under Views, and an Index.aspx file inside that folder.</span></span>

<span data-ttu-id="c11fb-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c11fb-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span></span>

<span data-ttu-id="c11fb-123">新的索引檔也是已開啟並且可供編輯。</span><span class="sxs-lookup"><span data-stu-id="c11fb-123">The new Index file is also already opened and ready for editing.</span></span> <span data-ttu-id="c11fb-124">加入一些文字，第一個下的&lt;h2&gt;索引&lt;/h2&gt;喜歡"Hello World"。</span><span class="sxs-lookup"><span data-stu-id="c11fb-124">Add some text under the first &lt;h2&gt;Index&lt;/h2&gt; like "Hello World."</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

<span data-ttu-id="c11fb-125">執行您的應用程式，請瀏覽[ `http://localhost:xx/HelloWorld` ](http://localhostxx)瀏覽器中一次。</span><span class="sxs-lookup"><span data-stu-id="c11fb-125">Run your application and visit [`http://localhost:xx/HelloWorld`](http://localhostxx) again in your browser.</span></span> <span data-ttu-id="c11fb-126">索引中的方法在此範例中我們控制站未做任何的工作，但未呼叫"傳回 View()"這表示我們想要使用的檢視範本檔案來呈現回應傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="c11fb-126">The Index method in our controller in this example didn't do any work, but did call "return View()" which indicated that we wanted to use a view template file to render a response back to the client.</span></span> <span data-ttu-id="c11fb-127">因為我們並未明確指定要使用的檢視範本檔案的名稱，ASP.NET MVC 預設 \Views\HelloWorld 資料夾內使用 Index.aspx 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="c11fb-127">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the Index.aspx view file within the \Views\HelloWorld folder.</span></span> <span data-ttu-id="c11fb-128">現在我們可以看到我們硬式編碼在我們的檢視中的字串。</span><span class="sxs-lookup"><span data-stu-id="c11fb-128">Now we see the string we hard-coded in our View.</span></span>

<span data-ttu-id="c11fb-129">[![索引-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="c11fb-129">[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span></span>

<span data-ttu-id="c11fb-130">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="c11fb-130">Looks pretty good.</span></span> <span data-ttu-id="c11fb-131">不過，請注意，"Index"的瀏覽器的標題指出大的標題，在頁面上顯示 「 我 MVC 應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="c11fb-131">However, notice that the Browser's title says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="c11fb-132">讓我們將這些變更。</span><span class="sxs-lookup"><span data-stu-id="c11fb-132">Let's change those.</span></span>

### <a name="changing-views-and-master-pages"></a><span data-ttu-id="c11fb-133">變更檢視和主版頁面</span><span class="sxs-lookup"><span data-stu-id="c11fb-133">Changing Views and Master Pages</span></span>

<span data-ttu-id="c11fb-134">首先，我們來變更文字 「 我 MVC 應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="c11fb-134">First, let's change the text "My MVC Application."</span></span> <span data-ttu-id="c11fb-135">該文字共用，並顯示每一頁上。</span><span class="sxs-lookup"><span data-stu-id="c11fb-135">That text is shared and appears on every page.</span></span> <span data-ttu-id="c11fb-136">它實際上不會顯示只有一個位置中的程式碼，即使它是我們的應用程式中的每一頁上。</span><span class="sxs-lookup"><span data-stu-id="c11fb-136">It actually appears in only one place in our code, even though it's on every page in our app.</span></span> <span data-ttu-id="c11fb-137">移至 [方案總管] 中的 /Views/Shared 資料夾，然後開啟 Site.Master 檔。</span><span class="sxs-lookup"><span data-stu-id="c11fb-137">Go to the /Views/Shared folder in the Solution Explorer and open the Site.Master file.</span></span> <span data-ttu-id="c11fb-138">這個檔案稱為主版頁面，它是共用 「 殼層 」，其他所有網頁都使用。</span><span class="sxs-lookup"><span data-stu-id="c11fb-138">This file is called a Master Page and it's the shared "shell" that all our other pages use.</span></span>

<span data-ttu-id="c11fb-139">這個檔案會注意到 ContentPlaceholder"MainContent"這些文字。</span><span class="sxs-lookup"><span data-stu-id="c11fb-139">Notice some text that says ContentPlaceholder "MainContent" in this file.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

<span data-ttu-id="c11fb-140">該預留位置是在您建立的所有頁面會都顯示，主版頁面中的 「 包裝 」。</span><span class="sxs-lookup"><span data-stu-id="c11fb-140">That placeholder is where all the pages you create will show up, "wrapped" in the master page.</span></span> <span data-ttu-id="c11fb-141">請嘗試變更標題，然後執行您的應用程式，並瀏覽多個頁面。</span><span class="sxs-lookup"><span data-stu-id="c11fb-141">Try changing the title, then run your app and visit multiple pages.</span></span> <span data-ttu-id="c11fb-142">您會發現您的一項變更會出現多個頁面上。</span><span class="sxs-lookup"><span data-stu-id="c11fb-142">You'll notice that your one change appears on multiple pages.</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

<span data-ttu-id="c11fb-143">現在每一頁會有主要的標題-這是 H1-的 「 我 MVC 電影應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="c11fb-143">Now every page will have the Primary Heading - that's H1 - of "My MVC Movie Application."</span></span> <span data-ttu-id="c11fb-144">可處理的白色文字上方那里共用的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="c11fb-144">That handles the white text at the top there that's shared across all the pages.</span></span>

<span data-ttu-id="c11fb-145">以下是我們變更標題整個 Site.Master:</span><span class="sxs-lookup"><span data-stu-id="c11fb-145">Here is the Site.Master in its entirety with our changed title:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

<span data-ttu-id="c11fb-146">現在，讓我們來變更索引頁的標題。</span><span class="sxs-lookup"><span data-stu-id="c11fb-146">Now, let's change the title of the Index page.</span></span>

<span data-ttu-id="c11fb-147">開啟 /HelloWorld/Index.aspx。</span><span class="sxs-lookup"><span data-stu-id="c11fb-147">Open /HelloWorld/Index.aspx.</span></span> <span data-ttu-id="c11fb-148">沒有變更的兩個地方。</span><span class="sxs-lookup"><span data-stu-id="c11fb-148">There's two places to change.</span></span> <span data-ttu-id="c11fb-149">首先，標題，會出現在瀏覽器，然後次要標頭-也是 H2-的標題。</span><span class="sxs-lookup"><span data-stu-id="c11fb-149">First, the Title that appears in the title of the browser, then the secondary header - that's H2 - as well.</span></span> <span data-ttu-id="c11fb-150">我要進行每個稍有不同，因此您可以看到哪些程式碼變更的應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="c11fb-150">I'll make them each slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

<span data-ttu-id="c11fb-151">執行您的應用程式，並瀏覽 /Movies。</span><span class="sxs-lookup"><span data-stu-id="c11fb-151">Run your app and visit /Movies.</span></span> <span data-ttu-id="c11fb-152">請注意，瀏覽器標題、 標題主要和次要的標題已變更。</span><span class="sxs-lookup"><span data-stu-id="c11fb-152">Notice that the browser title, the primary heading and the secondary headings have changed.</span></span> <span data-ttu-id="c11fb-153">很容易就能讓您檢視中進行小變更您的應用程式項重大變更。</span><span class="sxs-lookup"><span data-stu-id="c11fb-153">It's easy to make big changes in your app with small changes to your view.</span></span>

<span data-ttu-id="c11fb-154">[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="c11fb-154">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span></span>

<span data-ttu-id="c11fb-155">我們一點點的 「 資料 」 （在此情況下的"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="c11fb-155">Our little bit of "data" (in this case the "Hello World!"</span></span> <span data-ttu-id="c11fb-156">訊息） 很難透過自動程式碼。</span><span class="sxs-lookup"><span data-stu-id="c11fb-156">message) was hard coded though.</span></span> <span data-ttu-id="c11fb-157">我們有 V (Views)，我們有 C （控制站），但沒有 M （模型） 尚未。</span><span class="sxs-lookup"><span data-stu-id="c11fb-157">We've got V (Views) and we've got C (Controllers), but no M (Model) yet.</span></span> <span data-ttu-id="c11fb-158">我們將稍後逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="c11fb-158">We'll shortly walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-a-viewmodel"></a><span data-ttu-id="c11fb-159">傳遞 ViewModel</span><span class="sxs-lookup"><span data-stu-id="c11fb-159">Passing a ViewModel</span></span>

<span data-ttu-id="c11fb-160">我們請移至資料庫，並討論模型之前，不過，讓我們先討論 「 ViewModels。 」</span><span class="sxs-lookup"><span data-stu-id="c11fb-160">Before we go to a database and talk about Models, though, let's first talk about "ViewModels."</span></span> <span data-ttu-id="c11fb-161">這些是代表檢視範本來呈現 HTML 回應傳回給用戶端需要的物件。</span><span class="sxs-lookup"><span data-stu-id="c11fb-161">These are objects that represent what a View template requires to render an HTML response back to a client.</span></span> <span data-ttu-id="c11fb-162">它們通常建立由控制器類別傳遞給檢視範本，並應該只包含需要檢視表範本的資料的動作。</span><span class="sxs-lookup"><span data-stu-id="c11fb-162">They are typically created and passed by a Controller class to a View template, and should only contain the data that the View template requires - and no more.</span></span>

<span data-ttu-id="c11fb-163">先前使用 HelloWorld 範例中，我們 Welcome() 動作方法的名稱和 numTimes 參數，並輸出至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c11fb-163">Previously with our HelloWorld sample, our Welcome() action method took a name and a numTimes parameter and output it to the browser.</span></span> <span data-ttu-id="c11fb-164">而非保有繼續直接轉譯這個回應的控制站，讓我們反而是讓保存該資料，然後將其傳遞至回轉譯 HTML 回應使用它的檢視範本的小型類別。</span><span class="sxs-lookup"><span data-stu-id="c11fb-164">Rather than have the Controller continue to render this response directly,let's instead make a small class to hold that data and then pass it over to a View template to render back the HTML response using it.</span></span> <span data-ttu-id="c11fb-165">如此一來，控制器關於一件事並檢視範本另 – 讓我們能夠維護清除 」 的重要性分離 」 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="c11fb-165">This way the Controller is concerned with one thing and the View template another – enabling us to maintain clean "separation of concerns" within our application.</span></span>

<span data-ttu-id="c11fb-166">返回 HelloWorldController.cs 檔案並加入新的 「 WelcomeViewModel 」 類別並變更您的控制器內的 歡迎使用方法。</span><span class="sxs-lookup"><span data-stu-id="c11fb-166">Return to the HelloWorldController.cs file and add a new "WelcomeViewModel" class and change the Welcome method inside your controller.</span></span> <span data-ttu-id="c11fb-167">以下是完整的 HelloWorldController.cs 與新的類別，在相同的檔案。</span><span class="sxs-lookup"><span data-stu-id="c11fb-167">Here is the complete HelloWorldController.cs with the new class in the same file.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

<span data-ttu-id="c11fb-168">即使是在多行上，我們 褖畫惎方法實際上是只有兩個程式碼陳述式。</span><span class="sxs-lookup"><span data-stu-id="c11fb-168">Even though it's on multiple lines, our Welcome method is really only two code statements.</span></span> <span data-ttu-id="c11fb-169">我們到 ViewModel 物件，而第二個階段的兩個參數封裝的第一個陳述式在檢視上產生的物件。</span><span class="sxs-lookup"><span data-stu-id="c11fb-169">The first statement packages up our two parameters into a ViewModel object, and the second passes the resulting object onto the View.</span></span>

<span data-ttu-id="c11fb-170">現在我們需要 褖畫惎檢視範本 ！</span><span class="sxs-lookup"><span data-stu-id="c11fb-170">Now we need a Welcome View template!</span></span> <span data-ttu-id="c11fb-171">以滑鼠右鍵按一下該 褖畫惎方法中，然後選取 加入檢視。</span><span class="sxs-lookup"><span data-stu-id="c11fb-171">Right click in the Welcome method and select Add View.</span></span> <span data-ttu-id="c11fb-172">這次我們會檢查 「 建立強型別檢視 」，並且從下拉式清單中選取 WelcomeViewModel 類別。</span><span class="sxs-lookup"><span data-stu-id="c11fb-172">This time, we'll check "Create a strongly-typed view" and select our WelcomeViewModel class from the drop down list.</span></span> <span data-ttu-id="c11fb-173">這個新的檢視只會知道 WelcomeViewModels 和其他類型的物件。</span><span class="sxs-lookup"><span data-stu-id="c11fb-173">This new view will only know about WelcomeViewModels and no other types of objects.</span></span>

> <span data-ttu-id="c11fb-174">*注意： 您將需要編譯一次後才會出現在下拉式清單中加入您的 WelcomeViewModel。*</span><span class="sxs-lookup"><span data-stu-id="c11fb-174">*NOTE: You'll need to have compiled once after adding your WelcomeViewModel for to show up in the drop down list.*</span></span>


<span data-ttu-id="c11fb-175">以下是您加入檢視的對話方塊外觀應該為何。</span><span class="sxs-lookup"><span data-stu-id="c11fb-175">Here's what your Add View dialog should look like.</span></span> <span data-ttu-id="c11fb-176">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c11fb-176">Click the Add button.</span></span> ![加入檢視圈選起來](getting-started-with-mvc-part3/_static/image10.png)

<span data-ttu-id="c11fb-178">此程式碼下新增&lt;h2&gt;您新 Welcome.aspx 中。</span><span class="sxs-lookup"><span data-stu-id="c11fb-178">Add this code under the &lt;h2&gt; in your new Welcome.aspx.</span></span> <span data-ttu-id="c11fb-179">我們將進行迴圈，並依需求多次使用者可指定我們應該看看 ！</span><span class="sxs-lookup"><span data-stu-id="c11fb-179">We'll make a loop and say Hello as many times as the user says we should!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

<span data-ttu-id="c11fb-180">另外而且請注意，因為您正在輸入，因為我們會告知這 WelcomeViewModel 有關檢視 （未婚，記得） 嗎？ 我們取得很有幫助我們每次參考我們所見以下螢幕擷取畫面中的模型物件的 Intellisense:</span><span class="sxs-lookup"><span data-stu-id="c11fb-180">Also, notice while you're typing that because we told this View about the WelcomeViewModel (they are married, remember?) that we get helpful Intellisense each time we reference our Model object as seen in the screenshot below:</span></span>

<span data-ttu-id="c11fb-181">[![NumTime 原始程式碼](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="c11fb-181">[![NumTime Source Code](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span></span>

<span data-ttu-id="c11fb-182">執行您的應用程式，請瀏覽`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`一次。</span><span class="sxs-lookup"><span data-stu-id="c11fb-182">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` again.</span></span> <span data-ttu-id="c11fb-183">現在我們將帶資料從 URL，它會自動傳遞到我們的控制器，Controller ViewModel 資料的封裝，並傳遞到我們檢視該物件。</span><span class="sxs-lookup"><span data-stu-id="c11fb-183">Now we're taking data from the URL, it's passed into our Controller automatically, our Controller packages up the data into a ViewModel and passes that object onto our View.</span></span> <span data-ttu-id="c11fb-184">檢視比對使用者顯示為 HTML 的資料。</span><span class="sxs-lookup"><span data-stu-id="c11fb-184">The View than displays the data as HTML to the user.</span></span>

<span data-ttu-id="c11fb-185">[![歡迎使用-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="c11fb-185">[![Welcome - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span></span>

<span data-ttu-id="c11fb-186">這是一種模式下，"M"，但資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="c11fb-186">Well, that was a kind of an "M" for Model, but not the database kind.</span></span> <span data-ttu-id="c11fb-187">讓我們來看我們所學的內容和建立資料庫的電影。</span><span class="sxs-lookup"><span data-stu-id="c11fb-187">Let's take what we've learned and create a database of Movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c11fb-188">[上一頁](getting-started-with-mvc-part2.md)
[下一頁](getting-started-with-mvc-part4.md)</span><span class="sxs-lookup"><span data-stu-id="c11fb-188">[Previous](getting-started-with-mvc-part2.md)
[Next](getting-started-with-mvc-part4.md)</span></span>
