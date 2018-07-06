---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 簡介 ASP.NET Web Pages-程式設計基本概念 |Microsoft Docs
author: tfitzmac
description: 本教學課程可提供您的概觀含有 Razor 語法的 ASP.NET Web Pages 中程式。 您將學到什麼： 您的提取要求所使用的基本 'Razor' 語法...
ms.author: aspnetcontent
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 56268943b09d366b15d3a11e641d6c6b6c95aa16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839745"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a><span data-ttu-id="40d35-104">ASP.NET Web Pages 簡介-程式設計基本概念</span><span class="sxs-lookup"><span data-stu-id="40d35-104">Introducing ASP.NET Web Pages - Programming Basics</span></span>
====================
<span data-ttu-id="40d35-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="40d35-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="40d35-106">本教學課程可提供您的概觀含有 Razor 語法的 ASP.NET Web Pages 中程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-106">This tutorial gives you an overview of how to program in ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="40d35-107">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="40d35-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="40d35-108">您用於 ASP.NET Web Pages 中的程式設計基本 「 Razor 」 語法。</span><span class="sxs-lookup"><span data-stu-id="40d35-108">The basic "Razor" syntax that you use for programming in ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="40d35-109">一些基本 C# 中，也就是您將使用的程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="40d35-109">Some basic C#, which is the programming language you'll use.</span></span>
> - <span data-ttu-id="40d35-110">網頁的某些基本程式設計概念。</span><span class="sxs-lookup"><span data-stu-id="40d35-110">Some fundamental programming concepts for Web Pages.</span></span>
> - <span data-ttu-id="40d35-111">如何安裝封裝 （包含預先建置的程式碼的元件） 以使用與您的網站。</span><span class="sxs-lookup"><span data-stu-id="40d35-111">How to install packages (components that contain prebuilt code) to use with your site.</span></span>
> - <span data-ttu-id="40d35-112">如何使用*協助程式*來執行常見程式設計工作。</span><span class="sxs-lookup"><span data-stu-id="40d35-112">How to use *helpers* to perform common programming tasks.</span></span>
>   
> 
> <span data-ttu-id="40d35-113">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="40d35-113">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="40d35-114">NuGet 和套件管理員。</span><span class="sxs-lookup"><span data-stu-id="40d35-114">NuGet and the package manager.</span></span>
> - <span data-ttu-id="40d35-115">`Gravatar`協助程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-115">The `Gravatar` helper.</span></span>


<span data-ttu-id="40d35-116">本教學課程是向您介紹程式設計的語法，您將使用 ASP.NET Web Pages 中的主要活動。</span><span class="sxs-lookup"><span data-stu-id="40d35-116">This tutorial is primarily an exercise in introducing you to the programming syntax that you'll use for ASP.NET Web Pages.</span></span> <span data-ttu-id="40d35-117">您將了解*Razor 語法*和 C# 中撰寫的程式碼程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="40d35-117">You'll learn about *Razor syntax* and code that's written in the C# programming language.</span></span> <span data-ttu-id="40d35-118">您在上一個教學課程中，有一窺這種語法在本教學課程中，我們將說明詳細的語法。</span><span class="sxs-lookup"><span data-stu-id="40d35-118">You got a glimpse of this syntax in the previous tutorial; in this tutorial we'll explain the syntax more.</span></span>

<span data-ttu-id="40d35-119">我們保證，本教學課程包含程式設計，您會看到在單一的教學課程中，以及它是唯一的教學課程，是最*只*關於程式設計。</span><span class="sxs-lookup"><span data-stu-id="40d35-119">We promise that this tutorial involves the most programming that you'll see in a single tutorial, and that it's the only tutorial that is *only* about programming.</span></span> <span data-ttu-id="40d35-120">在此集合中其餘的教學課程，您將實際建立有趣的事情頁面。</span><span class="sxs-lookup"><span data-stu-id="40d35-120">In the remaining tutorials in this set, you'll actually create pages that do interesting things.</span></span>

<span data-ttu-id="40d35-121">您也將了解*協助程式*。</span><span class="sxs-lookup"><span data-stu-id="40d35-121">You'll also learn about *helpers*.</span></span> <span data-ttu-id="40d35-122">協助程式是一種元件 — 封裝最多段程式碼 — 您可以加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="40d35-122">A helper is a component — a packaged-up piece of code — that you can add to a page.</span></span> <span data-ttu-id="40d35-123">協助程式會執行，否則可能會很繁瑣或是以手動方式執行複雜的工作。</span><span class="sxs-lookup"><span data-stu-id="40d35-123">The helper performs work for you that otherwise might be tedious or complex to do by hand.</span></span>

## <a name="creating-a-page-to-play-with-razor"></a><span data-ttu-id="40d35-124">建立使用 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="40d35-124">Creating a Page to Play with Razor</span></span>

<span data-ttu-id="40d35-125">在本節中您將播放有點 Razor 的因此您可以了解的基本語法。</span><span class="sxs-lookup"><span data-stu-id="40d35-125">In this section you'll play a bit with Razor so you can get a sense of the basic syntax.</span></span>

<span data-ttu-id="40d35-126">如果尚未執行，請啟動 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="40d35-126">Start WebMatrix if it's not already running.</span></span> <span data-ttu-id="40d35-127">您將使用您在上一個教學課程中建立的網站 ([取得開始使用 Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578))。</span><span class="sxs-lookup"><span data-stu-id="40d35-127">You'll use the website you created in the previous tutorial ([Getting Started With Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578)).</span></span> <span data-ttu-id="40d35-128">若要重新開啟它，請按一下**My Sites** ，然後選擇**WebPageMovies**:</span><span class="sxs-lookup"><span data-stu-id="40d35-128">To reopen it, click **My Sites** and choose **WebPageMovies**:</span></span>

![WebMatrix 啟動畫面，顯示反白顯示了我的網站與開啟的站台選項](intro-to-web-pages-programming/_static/image1.png)

<span data-ttu-id="40d35-130">選取 **檔案**工作區。</span><span class="sxs-lookup"><span data-stu-id="40d35-130">Select the **Files** workspace.</span></span>

<span data-ttu-id="40d35-131">在 功能區中，按一下**新增**建立頁面。</span><span class="sxs-lookup"><span data-stu-id="40d35-131">In the ribbon, click **New** to create a page.</span></span> <span data-ttu-id="40d35-132">選取  **CSHTML**並將新頁面命名*TestRazor.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="40d35-132">Select **CSHTML** and name the new page *TestRazor.cshtml*.</span></span>

<span data-ttu-id="40d35-133">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="40d35-133">Click **OK**.</span></span>

<span data-ttu-id="40d35-134">將以下內容複製到檔案中，完全取代項目已經有。</span><span class="sxs-lookup"><span data-stu-id="40d35-134">Copy the following into the file, completely replacing what's there already.</span></span>

> [!NOTE]
> <span data-ttu-id="40d35-135">當您到頁面上，複製程式碼或標記的範例時，縮排和對齊方式可能無法與本教學課程相同。</span><span class="sxs-lookup"><span data-stu-id="40d35-135">When you copy code or markup from the examples into a page, the indentation and alignment might not be the same as in the tutorial.</span></span> <span data-ttu-id="40d35-136">縮排和對齊方式不會影響程式碼的執行方式，不過。</span><span class="sxs-lookup"><span data-stu-id="40d35-136">Indentation and alignment don't affect how the code runs, though.</span></span>


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a><span data-ttu-id="40d35-137">檢查範例頁面</span><span class="sxs-lookup"><span data-stu-id="40d35-137">Examining the Example Page</span></span>

<span data-ttu-id="40d35-138">您所看到的大部分是一般的 HTML。</span><span class="sxs-lookup"><span data-stu-id="40d35-138">Most of what you see is ordinary HTML.</span></span> <span data-ttu-id="40d35-139">不過，在頂端沒有此程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="40d35-139">However, at the top there's this code block:</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

<span data-ttu-id="40d35-140">請注意下列事項，關於此程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="40d35-140">Notice the following things about this code block:</span></span>

- <span data-ttu-id="40d35-141">@ 字元會告知 ASP.NET，接下來是 Razor 程式碼，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="40d35-141">The @ character tells ASP.NET that what follows is Razor code, not HTML.</span></span> <span data-ttu-id="40d35-142">ASP.NET 會將所有項目之後 @ 字元，直到它再次執行成一些 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="40d35-142">ASP.NET will treat everything after the @ character as code until it runs into some HTML again.</span></span> <span data-ttu-id="40d35-143">(在此情況下，這&lt;！DOCTYPE&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="40d35-143">(In this case, that's the &lt;!DOCTYPE&gt; element.</span></span>
- <span data-ttu-id="40d35-144">大括號 （{和}） 括住的 Razor 程式碼區塊，如果程式碼有多個列。</span><span class="sxs-lookup"><span data-stu-id="40d35-144">The braces ( { and } ) enclose a block of Razor code if the code has more than one line.</span></span> <span data-ttu-id="40d35-145">大括號會告知 ASP.NET，該區塊的程式碼開頭與結尾的位置。</span><span class="sxs-lookup"><span data-stu-id="40d35-145">The braces tell ASP.NET where the code for that block starts and ends.</span></span>
- <span data-ttu-id="40d35-146">/ / 字元標示註解，也就是不會執行的程式碼的一部分。</span><span class="sxs-lookup"><span data-stu-id="40d35-146">The // characters mark a comment — that is, a part of the code that won't execute.</span></span>
- <span data-ttu-id="40d35-147">每個陳述式必須以分號 （;） 結尾。</span><span class="sxs-lookup"><span data-stu-id="40d35-147">Each statement has to end with a semicolon (;).</span></span> <span data-ttu-id="40d35-148">（未註解，雖然。）</span><span class="sxs-lookup"><span data-stu-id="40d35-148">(Not comments, though.)</span></span>
- <span data-ttu-id="40d35-149">您可以儲存中的值*變數*，您建立 (*宣告*) 與關鍵字 var。</span><span class="sxs-lookup"><span data-stu-id="40d35-149">You can store values in *variables*, which you create (*declare*) with the keyword var.</span></span> <span data-ttu-id="40d35-150">當您建立變數時，您為它命名，其中可以包含字母、 數字和底線 (\_)。</span><span class="sxs-lookup"><span data-stu-id="40d35-150">When you create a variable, you give it a name, which can include letters, numbers, and underscore (\_).</span></span> <span data-ttu-id="40d35-151">變數名稱不能以數字開頭，而且無法使用程式設計的關鍵字 （例如 var) 的名稱。</span><span class="sxs-lookup"><span data-stu-id="40d35-151">Variable names can't start with a number and can't use the name of a programming keyword (like var).</span></span>
- <span data-ttu-id="40d35-152">以引號括住字元字串 （例如 「 ASP.NET 」 和 「 網頁 」）。</span><span class="sxs-lookup"><span data-stu-id="40d35-152">You enclose character strings (like "ASP.NET" and "Web Pages") in quotation marks.</span></span> <span data-ttu-id="40d35-153">（它們必須是雙引號）。數字不在引號中。</span><span class="sxs-lookup"><span data-stu-id="40d35-153">(They must be double quotation marks.) Numbers are not in quotation marks.</span></span>
- <span data-ttu-id="40d35-154">引號之外的空白區域並不重要。</span><span class="sxs-lookup"><span data-stu-id="40d35-154">Whitespace outside of quotation marks doesn't matter.</span></span> <span data-ttu-id="40d35-155">換行大部分不重要;例外情況是您無法跨行分割引號的字串。</span><span class="sxs-lookup"><span data-stu-id="40d35-155">Line breaks mostly don't matter; the exception is that you can't split a string in quotation marks across lines.</span></span> <span data-ttu-id="40d35-156">縮排和對齊方式沒有影響。</span><span class="sxs-lookup"><span data-stu-id="40d35-156">Indentation and alignment don't matter.</span></span>

<span data-ttu-id="40d35-157">不是明顯來自此範例中的項目是所有的程式碼會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="40d35-157">Something that's not obvious from this example is that all code is case sensitive.</span></span> <span data-ttu-id="40d35-158">這表示變數的總和是不同的變數數目多於可能名為總和的變數數目。</span><span class="sxs-lookup"><span data-stu-id="40d35-158">This means that the variable TheSum is a different variable than variables that might be named theSum or thesum.</span></span> <span data-ttu-id="40d35-159">同樣地，var 是關鍵字，但不是 Var。</span><span class="sxs-lookup"><span data-stu-id="40d35-159">Similarly, var is a keyword, but Var is not.</span></span>

### <a name="objects-and-properties-and-methods"></a><span data-ttu-id="40d35-160">物件與屬性和方法</span><span class="sxs-lookup"><span data-stu-id="40d35-160">Objects and properties and methods</span></span>

<span data-ttu-id="40d35-161">然後是 DateTime.Now 的運算式。</span><span class="sxs-lookup"><span data-stu-id="40d35-161">Then there's the expression DateTime.Now.</span></span> <span data-ttu-id="40d35-162">簡單地說，是日期時間*物件*。</span><span class="sxs-lookup"><span data-stu-id="40d35-162">In simple terms, DateTime is an *object*.</span></span> <span data-ttu-id="40d35-163">物件是您可以使用程式設計的功能 — 頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄等等。物件具有一或多個*屬性*描述它們的特性。</span><span class="sxs-lookup"><span data-stu-id="40d35-163">An object is a thing that you can program with—a page, a text box, a file, an image, a web request, an email message, a customer record, etc. Objects have one or more *properties* that describe their characteristics.</span></span> <span data-ttu-id="40d35-164">文字 方塊物件都有文字屬性 （還有其他）、 request 物件具有 Url 屬性 （以及其他）、 電子郵件訊息有 From 屬性和 To 屬性，依此類推。</span><span class="sxs-lookup"><span data-stu-id="40d35-164">A text box object has a Text property (among others), a request object has a Url property (and others), an email message has a From property and a To property, and so on.</span></span> <span data-ttu-id="40d35-165">物件也有*方法*，是他們可以執行 「 動詞 」。</span><span class="sxs-lookup"><span data-stu-id="40d35-165">Objects also have *methods* that are the "verbs" they can perform.</span></span> <span data-ttu-id="40d35-166">您會使用物件很多。</span><span class="sxs-lookup"><span data-stu-id="40d35-166">You'll be working with objects a lot.</span></span>

<span data-ttu-id="40d35-167">您可以看到範例中，日期時間是可讓您程式的日期和時間的物件。</span><span class="sxs-lookup"><span data-stu-id="40d35-167">As you can see from the example, DateTime is an object that lets you program dates and times.</span></span> <span data-ttu-id="40d35-168">它具有名為現在會傳回目前的日期和時間的屬性。</span><span class="sxs-lookup"><span data-stu-id="40d35-168">It has a property named Now that returns the current date and time.</span></span>

### <a name="using-code-to-render-markup-in-the-page"></a><span data-ttu-id="40d35-169">使用程式碼來呈現頁面中的標記</span><span class="sxs-lookup"><span data-stu-id="40d35-169">Using code to render markup in the page</span></span>

<span data-ttu-id="40d35-170">在頁面的主體，請注意下列各項：</span><span class="sxs-lookup"><span data-stu-id="40d35-170">In the body of the page, notice the following:</span></span>

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

<span data-ttu-id="40d35-171">同樣地，@ 字元會告知 ASP.NET，以下是程式碼，不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="40d35-171">Again, the @ character tells ASP.NET that what follows is code, not HTML.</span></span> <span data-ttu-id="40d35-172">在標記中您可以加上 @ 後面的程式碼運算式，而 ASP.NET 會在該點轉譯該運算式右邊的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-172">In the markup you can add @ followed by a code expression, and ASP.NET will render the value of that expression right at that point.</span></span> <span data-ttu-id="40d35-173">在範例中，@a將會呈現任何內容的值是名為的變數，@product呈現無論是在變數的具名產品中，並依此類推。</span><span class="sxs-lookup"><span data-stu-id="40d35-173">In the example, @a will render whatever the value is of the variable named a, @product renders whatever is in the variable named product, and so on.</span></span>

<span data-ttu-id="40d35-174">您可以使用不到變數，不過。</span><span class="sxs-lookup"><span data-stu-id="40d35-174">You're not limited to variables, though.</span></span> <span data-ttu-id="40d35-175">在少數情況下在這裡，@ 字元前面的運算式：</span><span class="sxs-lookup"><span data-stu-id="40d35-175">In a few instances here, the @ character precedes an expression:</span></span>

- <span data-ttu-id="40d35-176">@(\*b） 呈現的變數中的產品和 b。</span><span class="sxs-lookup"><span data-stu-id="40d35-176">@(a\*b) renders the product of whatever is in the variables a and b.</span></span> <span data-ttu-id="40d35-177">(\*運算子表示乘法。)</span><span class="sxs-lookup"><span data-stu-id="40d35-177">(The \* operator means multiplication.)</span></span>
- <span data-ttu-id="40d35-178">@(技術 +""+ 產品) 之後將它們串連並之間加入空格呈現變數技術和產品中的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-178">@(technology + " " + product) renders the values in the variables technology and product after concatenating them and adding a space in between.</span></span> <span data-ttu-id="40d35-179">將字串串連運算子 （+） 是與運算子相同個數字相加。</span><span class="sxs-lookup"><span data-stu-id="40d35-179">The operator (+) for concatenating strings is the same as the operator for adding numbers.</span></span> <span data-ttu-id="40d35-180">ASP.NET 通常會告訴您是否正在與數字或字串，並進行正確的作業，使用 + 運算子。</span><span class="sxs-lookup"><span data-stu-id="40d35-180">ASP.NET can usually tell whether you're working with numbers or with strings and does the right thing with the + operator.</span></span>
- <span data-ttu-id="40d35-181">@Request.Url 呈現要求物件的 Url 屬性。</span><span class="sxs-lookup"><span data-stu-id="40d35-181">@Request.Url renders the Url property of the Request object.</span></span> <span data-ttu-id="40d35-182">要求物件包含瀏覽器中，從目前要求的相關資訊，而當然 Url 屬性包含目前要求的 URL。</span><span class="sxs-lookup"><span data-stu-id="40d35-182">The Request object contains information about the current request from the browser, and of course the Url property contains the URL of that current request.</span></span>

<span data-ttu-id="40d35-183">此範例也可顯示您可以執行不同的方式運作。</span><span class="sxs-lookup"><span data-stu-id="40d35-183">The example is also designed to show you that you can do work in different ways.</span></span> <span data-ttu-id="40d35-184">您可以執行程式碼區塊頂端的計算，將結果放入變數中，然後轉譯在標記中的變數。</span><span class="sxs-lookup"><span data-stu-id="40d35-184">You can do calculations in the code block at the top, put the results into a variable, and then render the variable in markup.</span></span> <span data-ttu-id="40d35-185">或者，您可以執行在標記中的運算式右邊的計算結果。</span><span class="sxs-lookup"><span data-stu-id="40d35-185">Or you can do calculations in an expression right in the markup.</span></span> <span data-ttu-id="40d35-186">您使用的方法取決於您正在做什麼，以及某種程度來說，在您自己的喜好設定。</span><span class="sxs-lookup"><span data-stu-id="40d35-186">The approach you use depends on what you're doing and, to some extent, on your own preference.</span></span>

### <a name="seeing-the-code-in-action"></a><span data-ttu-id="40d35-187">查看動作中的程式碼</span><span class="sxs-lookup"><span data-stu-id="40d35-187">Seeing the code in action</span></span>

<span data-ttu-id="40d35-188">以滑鼠右鍵按一下檔案的名稱，然後選擇 **在瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="40d35-188">Right-click the name of the file and then choose **Launch in browser**.</span></span> <span data-ttu-id="40d35-189">您會看到所有的值和在頁面中解析運算式的瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="40d35-189">You see the page in the browser with all the values and expressions resolved in the page.</span></span>

![在瀏覽器中執行的 'TestRazor' 頁面](intro-to-web-pages-programming/_static/image2.png)

<span data-ttu-id="40d35-191">看看瀏覽器中的來源。</span><span class="sxs-lookup"><span data-stu-id="40d35-191">Look at the source in the browser.</span></span>

![在瀏覽器中測試 Razor 頁面原始碼](intro-to-web-pages-programming/_static/image3.png)

<span data-ttu-id="40d35-193">如您預期您在上一個教學課程中的體驗，Razor 程式碼都是在的頁面。</span><span class="sxs-lookup"><span data-stu-id="40d35-193">As you expect from your experience in the previous tutorial, none of the Razor code is in the page.</span></span> <span data-ttu-id="40d35-194">您會看到所有會實際顯示的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-194">All you see are the actual display values.</span></span> <span data-ttu-id="40d35-195">當您執行頁面時，您實際上正在要求 WebMatrix 內建的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="40d35-195">When you run a page, you're actually making a request to the web server that's built into WebMatrix.</span></span> <span data-ttu-id="40d35-196">當收到要求時，ASP.NET 會解析所有的值和運算式，並將其值轉譯成網頁。</span><span class="sxs-lookup"><span data-stu-id="40d35-196">When the request is received, ASP.NET resolves all the values and expressions and renders their values into the page.</span></span> <span data-ttu-id="40d35-197">然後，將頁面傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="40d35-197">It then sends the page to the browser.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="40d35-198">**Razor 和 C#**</span><span class="sxs-lookup"><span data-stu-id="40d35-198">**Razor and C#**</span></span>
> 
> <span data-ttu-id="40d35-199">到目前為止我們所說的內容，您正在使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="40d35-199">Up to now we've said that you're working with Razor syntax.</span></span> <span data-ttu-id="40d35-200">為 true，但它不是完成的劇本。</span><span class="sxs-lookup"><span data-stu-id="40d35-200">That's true, but it's not the complete story.</span></span> <span data-ttu-id="40d35-201">實際的程式設計語言，您使用稱為*C#*。</span><span class="sxs-lookup"><span data-stu-id="40d35-201">The actual programming language you're using is called *C#*.</span></span> <span data-ttu-id="40d35-202">C# 年前由 Microsoft 所建立而且已經成為主要的程式設計語言，用於建立 Windows 應用程式的其中一個。</span><span class="sxs-lookup"><span data-stu-id="40d35-202">C# was created by Microsoft over a decade ago and has become one of the primary programming languages for creating Windows apps.</span></span> <span data-ttu-id="40d35-203">您已了解有關如何為變數命名，以及如何建立陳述式等的所有規則都是實際的 C# 語言的所有規則。</span><span class="sxs-lookup"><span data-stu-id="40d35-203">All the rules you've seen about how to name a variable and how to create statements and so on are actually all rules of the C# language.</span></span>
> 
> <span data-ttu-id="40d35-204">Razor 更具體來說是指在小型組的慣例，您將此程式碼內嵌到頁面。</span><span class="sxs-lookup"><span data-stu-id="40d35-204">Razor refers more specifically to the small set of conventions for how you embed this code into a page.</span></span> <span data-ttu-id="40d35-205">例如，使用標記頁面中的程式碼和使用的慣例 @ {} 內嵌程式碼區塊是頁面的 Razor 層面。</span><span class="sxs-lookup"><span data-stu-id="40d35-205">For example, the convention of using @ to mark code in the page and using @{ } to embed a code block is the Razor aspect of a page.</span></span> <span data-ttu-id="40d35-206">協助程式也會被視為 Razor 的一部分。</span><span class="sxs-lookup"><span data-stu-id="40d35-206">Helpers are also considered to be part of Razor.</span></span> <span data-ttu-id="40d35-207">Razor 語法用在更多地方比只是 ASP.NET Web Pages 中。</span><span class="sxs-lookup"><span data-stu-id="40d35-207">Razor syntax is used in more places than just in ASP.NET Web Pages.</span></span> <span data-ttu-id="40d35-208">（例如，它使用 ASP.NET MVC 檢視表中）。</span><span class="sxs-lookup"><span data-stu-id="40d35-208">(For example, it's used in ASP.NET MVC views as well.)</span></span>
> 
> <span data-ttu-id="40d35-209">我們之所以提到這，因為如果您尋找程式 ASP.NET Web Pages 」 的相關資訊，您會發現許多 Razor 的參考。</span><span class="sxs-lookup"><span data-stu-id="40d35-209">We mention this because if you look for information about programming ASP.NET Web Pages, you'll find lots of references to Razor.</span></span> <span data-ttu-id="40d35-210">不過，這些參考大量不適用於什麼是這麼做，因此可能會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="40d35-210">However, a lot of those references don't apply to what you're doing and might therefore be confusing.</span></span> <span data-ttu-id="40d35-211">而且事實上，許多程式設計問題的真正想要使用 C#，或使用 ASP.NET 的相關。</span><span class="sxs-lookup"><span data-stu-id="40d35-211">And in fact, many of your programming questions are really going to be about either working with C# or working with ASP.NET.</span></span> <span data-ttu-id="40d35-212">因此如果您特別尋找 Razor 的相關資訊，您可能無法找到所需的解答。</span><span class="sxs-lookup"><span data-stu-id="40d35-212">So if you look specifically for information about Razor, you might not find the answers you need.</span></span>


## <a name="adding-some-conditional-logic"></a><span data-ttu-id="40d35-213">新增一些條件式邏輯</span><span class="sxs-lookup"><span data-stu-id="40d35-213">Adding Some Conditional Logic</span></span>

<span data-ttu-id="40d35-214">在網頁中使用程式碼的相關最棒的功能之一是，您可以變更會發生什麼事根據各種條件。</span><span class="sxs-lookup"><span data-stu-id="40d35-214">One of the great features about using code in a page is that you can change what happens based on various conditions.</span></span> <span data-ttu-id="40d35-215">在本教學課程的這個部分，您會試用一些方法，若要變更頁面中顯示的內容。</span><span class="sxs-lookup"><span data-stu-id="40d35-215">In this part of the tutorial, you'll play around with some ways to change what's displayed in the page.</span></span>

<span data-ttu-id="40d35-216">此範例會簡單而且較自然，讓我們可以專注於條件式邏輯。</span><span class="sxs-lookup"><span data-stu-id="40d35-216">The example will be simple and somewhat contrived so that we can concentrate on the conditional logic.</span></span> <span data-ttu-id="40d35-217">您將建立的網頁將會執行這項操作：</span><span class="sxs-lookup"><span data-stu-id="40d35-217">The page you'll create will do this:</span></span>

- <span data-ttu-id="40d35-218">取決於它是否為第一次顯示網頁時，或是否已按下送出頁面上的按鈕，請在頁面上顯示不同的文字。</span><span class="sxs-lookup"><span data-stu-id="40d35-218">Show different text on the page depending on whether it's the first time the page is displayed or whether you've clicked a button to submit the page.</span></span> <span data-ttu-id="40d35-219">這會是第一項條件式測試。</span><span class="sxs-lookup"><span data-stu-id="40d35-219">That will be the first conditional test.</span></span>
- <span data-ttu-id="40d35-220">為某個值傳入查詢字串的 URL (http://...?show=true) 時，才會顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="40d35-220">Display the message only if a certain value is passed in the query string of the URL (http://...?show=true).</span></span> <span data-ttu-id="40d35-221">這會是第二個條件測試。</span><span class="sxs-lookup"><span data-stu-id="40d35-221">That will be the second conditional test.</span></span>

<span data-ttu-id="40d35-222">在 WebMatrix 中，建立網頁並將它命名*TestRazorPart2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="40d35-222">In WebMatrix, create a page and name it *TestRazorPart2.cshtml*.</span></span> <span data-ttu-id="40d35-223">(在功能區中，按一下**的新**，選擇**CSHTML**檔案，命名，然後按一下 **確定**。)</span><span class="sxs-lookup"><span data-stu-id="40d35-223">(In the ribbon, click **New**, choose **CSHTML**, name the file, and then click **OK**.)</span></span>

<span data-ttu-id="40d35-224">以下列取代該網頁的內容：</span><span class="sxs-lookup"><span data-stu-id="40d35-224">Replace the contents of that page with the following:</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

<span data-ttu-id="40d35-225">在頂端的程式碼區塊會初始化名為一些文字訊息的變數。</span><span class="sxs-lookup"><span data-stu-id="40d35-225">The code block at the top initializes a variable named message with some text.</span></span> <span data-ttu-id="40d35-226">在頁面的主體中，訊息變數的內容會顯示在&lt;p&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="40d35-226">In the body of the page, the contents of the message variable are displayed inside a &lt;p&gt; element.</span></span> <span data-ttu-id="40d35-227">也包含標記&lt;輸入&gt;元素，來建立**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="40d35-227">The markup also contains an &lt;input&gt; element to create a **Submit** button.</span></span>

<span data-ttu-id="40d35-228">執行頁面，以查看它如何運作。</span><span class="sxs-lookup"><span data-stu-id="40d35-228">Run the page to see how it works now.</span></span> <span data-ttu-id="40d35-229">現在，它基本上是靜態的頁面上，即使您按**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="40d35-229">For now, it's basically a static page, even if you click the **Submit** button.</span></span>

<span data-ttu-id="40d35-230">返回至 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="40d35-230">Go back to WebMatrix.</span></span> <span data-ttu-id="40d35-231">在程式碼區塊中，新增下列醒目提示的程式碼*之後*初始化訊息的那一行：</span><span class="sxs-lookup"><span data-stu-id="40d35-231">Inside the code block, add the following highlighted code *after* the line that initializes message:</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a><span data-ttu-id="40d35-232">If {} 區塊</span><span class="sxs-lookup"><span data-stu-id="40d35-232">The if { } block</span></span>

<span data-ttu-id="40d35-233">您剛才加入的 if 條件。</span><span class="sxs-lookup"><span data-stu-id="40d35-233">What you just added was an if condition.</span></span> <span data-ttu-id="40d35-234">在程式碼，如果條件有如下的結構：</span><span class="sxs-lookup"><span data-stu-id="40d35-234">In code, the if condition has a structure like this:</span></span>

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

<span data-ttu-id="40d35-235">要測試的條件是括號括住。</span><span class="sxs-lookup"><span data-stu-id="40d35-235">The condition to test is in parentheses.</span></span> <span data-ttu-id="40d35-236">它必須是值或傳回 true 或 false 的運算式。</span><span class="sxs-lookup"><span data-stu-id="40d35-236">It has to be a value or an expression that returns true or false.</span></span> <span data-ttu-id="40d35-237">如果條件為 true，ASP.NET 會執行陳述式或陳述式的括號內。</span><span class="sxs-lookup"><span data-stu-id="40d35-237">If the condition is true, ASP.NET runs the statement or statements that are inside the braces.</span></span> <span data-ttu-id="40d35-238">(這些都是*再*屬於*if-then*邏輯。)如果條件為 false，則會略過的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="40d35-238">(Those are the *then* part of the *if-then* logic.) If the condition is false, the block of code is skipped.</span></span>

<span data-ttu-id="40d35-239">以下是一些您可以在 if 測試的條件的範例陳述式：</span><span class="sxs-lookup"><span data-stu-id="40d35-239">Here are a few examples of conditions you can test in an if statement:</span></span>

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

<span data-ttu-id="40d35-240">您可以使用，以測試針對值或運算式的變數<em>邏輯運算子</em>或是<em>比較運算子</em>： 等於 （= =）、 大於 (&gt;)、 小於 (&lt;)，大於或等於 (&gt;=)，且小於或等於 (&lt;=)。</span><span class="sxs-lookup"><span data-stu-id="40d35-240">You can test variables against values or against expressions by using a <em>logical operator</em> or <em>comparison operator</em>: equal to (==), greater than (&gt;), less than (&lt;), greater than or equal to (&gt;=), and less than or equal to (&lt;=).</span></span> <span data-ttu-id="40d35-241">！ = 運算子表示不等於 — 比方說，如果 (！ = 0) 表示<em>如果</em> <em>不等於 0</em>。</span><span class="sxs-lookup"><span data-stu-id="40d35-241">The != operator means not equal to — for example, if(a != 0) means <em>if</em> <em>a</em><em>is not equal to 0</em>.</span></span>

> [!NOTE]
> <span data-ttu-id="40d35-242">請確定您會注意到，等於 （= =） 比較運算子不相同 =。</span><span class="sxs-lookup"><span data-stu-id="40d35-242">Make sure you notice that the comparison operator for equals to (==) is not the same as =.</span></span> <span data-ttu-id="40d35-243">= 運算子只能用來將值指派 (var = 2)。</span><span class="sxs-lookup"><span data-stu-id="40d35-243">The = operator is used only to assign values (var a=2).</span></span> <span data-ttu-id="40d35-244">如果您混這些運算子時，您可能會收到錯誤，否則您會得到一些奇怪的結果。</span><span class="sxs-lookup"><span data-stu-id="40d35-244">If you mix these operators up, you'll either get an error or you'll get some strange results.</span></span>


<span data-ttu-id="40d35-245">若要測試的項目是否為 true，完整的語法是 if(IsDone == true)。</span><span class="sxs-lookup"><span data-stu-id="40d35-245">To test whether something is true, the complete syntax is if(IsDone == true).</span></span> <span data-ttu-id="40d35-246">但是，您也可以使用捷徑 if(IsDone)。</span><span class="sxs-lookup"><span data-stu-id="40d35-246">But you can also use the shortcut if(IsDone).</span></span> <span data-ttu-id="40d35-247">如果沒有任何比較運算子，ASP.NET 會假設您要測試為 true。</span><span class="sxs-lookup"><span data-stu-id="40d35-247">If there's no comparison operator, ASP.NET assumes that you're testing for true.</span></span>

<span data-ttu-id="40d35-248">！</span><span class="sxs-lookup"><span data-stu-id="40d35-248">The !</span></span> <span data-ttu-id="40d35-249">運算子本身表示邏輯 NOT。</span><span class="sxs-lookup"><span data-stu-id="40d35-249">operator by itself means a logical NOT.</span></span> <span data-ttu-id="40d35-250">例如，條件 if （！IsPost) 方法*IsPost 是否不為 true*。</span><span class="sxs-lookup"><span data-stu-id="40d35-250">For example, the condition if(!IsPost) means *if IsPost is not true*.</span></span>

<span data-ttu-id="40d35-251">您可以使用邏輯 AND 結合條件 (&amp; &amp;運算子) 或邏輯 OR (| | 運算子)。</span><span class="sxs-lookup"><span data-stu-id="40d35-251">You can combine conditions by using a logical AND (&amp;&amp; operator) or logical OR (|| operator).</span></span> <span data-ttu-id="40d35-252">If 的最後一個上述的範例方法中的條件，例如*如果 FileProcessingIsDone 未設定為 true AND displayMessage 設為 false*。</span><span class="sxs-lookup"><span data-stu-id="40d35-252">For example, the last of the if conditions in the preceding examples means *if FileProcessingIsDone is not set to true AND displayMessage is set to false*.</span></span>

### <a name="the-else-block"></a><span data-ttu-id="40d35-253">在 else 區塊</span><span class="sxs-lookup"><span data-stu-id="40d35-253">The else block</span></span>

<span data-ttu-id="40d35-254">最後一有關 if 區塊： 如果區塊後面可以接著 else 區塊。</span><span class="sxs-lookup"><span data-stu-id="40d35-254">One final thing about if blocks: an if block can be followed by an else block.</span></span> <span data-ttu-id="40d35-255">適合其他區塊是您必須在條件為 false 時，執行不同的程式碼。</span><span class="sxs-lookup"><span data-stu-id="40d35-255">An else block is useful is you have to execute different code when the condition is false.</span></span> <span data-ttu-id="40d35-256">以下是簡單的範例：</span><span class="sxs-lookup"><span data-stu-id="40d35-256">Here's a simple example:</span></span>

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

<span data-ttu-id="40d35-257">在稍後的教學課程中，使用其他區塊會很實用的這一系列中，您會看到一些範例。</span><span class="sxs-lookup"><span data-stu-id="40d35-257">You'll see some examples in later tutorials in this series where using an else block is useful.</span></span>

### <a name="testing-whether-the-request-is-a-submit-post"></a><span data-ttu-id="40d35-258">測試這個要求提交 （文章）</span><span class="sxs-lookup"><span data-stu-id="40d35-258">Testing whether the request is a submit (post)</span></span>

<span data-ttu-id="40d35-259">還有更多，但讓我們回到範例中，有條件 if(IsPost) {...}。</span><span class="sxs-lookup"><span data-stu-id="40d35-259">There's more, but let's get back to the example, which has the condition if(IsPost){ ... }.</span></span> <span data-ttu-id="40d35-260">IsPost 是實際目前頁面的屬性。</span><span class="sxs-lookup"><span data-stu-id="40d35-260">IsPost is actually a property of the current page.</span></span> <span data-ttu-id="40d35-261">第一次要求頁面時，IsPost 會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="40d35-261">The first time the page is requested, IsPost returns false.</span></span> <span data-ttu-id="40d35-262">不過，如果您按一下按鈕或否則送出頁面 — 也就是將其張貼 — IsPost，則傳回 true。</span><span class="sxs-lookup"><span data-stu-id="40d35-262">However, if you click a button or otherwise submit the page — that is, you post it — IsPost returns true.</span></span> <span data-ttu-id="40d35-263">因此 IsPost 可讓您判斷您是否正在處理送出表單。</span><span class="sxs-lookup"><span data-stu-id="40d35-263">So IsPost lets you determine whether you're dealing with a form submission.</span></span> <span data-ttu-id="40d35-264">（根據 HTTP 指令動詞，如果要求是 GET 作業，IsPost 傳回 false。</span><span class="sxs-lookup"><span data-stu-id="40d35-264">(In terms of HTTP verbs, if the request is a GET operation, IsPost returns false.</span></span> <span data-ttu-id="40d35-265">如果要求是 POST 作業，IsPost 會傳回 true。）在稍後的教學課程中您將使用輸入表單，其中這項測試變得特別實用。</span><span class="sxs-lookup"><span data-stu-id="40d35-265">If the request is a POST operation, IsPost returns true.) In a later tutorial you'll work with input forms, where this test becomes particularly useful.</span></span>

<span data-ttu-id="40d35-266">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="40d35-266">Run the page.</span></span> <span data-ttu-id="40d35-267">因為這是第一次要求您頁面上，看到 「 這是您要求的頁面第一次 」。</span><span class="sxs-lookup"><span data-stu-id="40d35-267">Because this is the first time you're requested the page, you see "This is the first time you've requested the page".</span></span> <span data-ttu-id="40d35-268">該字串會是您初始化訊息變數的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-268">That string is the value that you initialized the message variable to.</span></span> <span data-ttu-id="40d35-269">If(IsPost) 測試，但讓置於 if 的程式碼區塊傳回 false 目前，不會執行。</span><span class="sxs-lookup"><span data-stu-id="40d35-269">There's an if(IsPost) test, but that returns false at the moment, so the code inside the if block doesn't run.</span></span>

<span data-ttu-id="40d35-270">按一下 [**送出**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="40d35-270">Click the **Submit** button.</span></span> <span data-ttu-id="40d35-271">要求頁面時一次。</span><span class="sxs-lookup"><span data-stu-id="40d35-271">The page is requested again.</span></span> <span data-ttu-id="40d35-272">為之前，訊息變數設定為 「 這是第一次...」。</span><span class="sxs-lookup"><span data-stu-id="40d35-272">As before, the message variable is set to "This is the first time ...".</span></span> <span data-ttu-id="40d35-273">但這次測試 if(IsPost) 傳回 true，因此 if 內的程式碼區塊執行。</span><span class="sxs-lookup"><span data-stu-id="40d35-273">But this time, the test if(IsPost) returns true, so the code inside the if block runs.</span></span> <span data-ttu-id="40d35-274">程式碼會變更為不同的值，也就是要呈現的標記中的訊息變數的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-274">The code changes the value of the message variable to a different value, which is what's rendered in the markup.</span></span>

<span data-ttu-id="40d35-275">現在將 if 條件在標記中的。</span><span class="sxs-lookup"><span data-stu-id="40d35-275">Now add an if condition in the markup.</span></span> <span data-ttu-id="40d35-276">下面&lt;p&gt;包含的項目**送出**按鈕，加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="40d35-276">Below the &lt;p&gt; element that contains the **Submit** button, add the following markup:</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

<span data-ttu-id="40d35-277">您要新增標記內的程式碼，因此您必須開始@。</span><span class="sxs-lookup"><span data-stu-id="40d35-277">You're adding code inside the markup, so you have to start with @.</span></span> <span data-ttu-id="40d35-278">If 就類似於您新增稍早的程式碼區塊中的測試。</span><span class="sxs-lookup"><span data-stu-id="40d35-278">Then there's an if test similar to the one you added earlier up in the code block.</span></span> <span data-ttu-id="40d35-279">在大括弧，不過，您要加入一般 HTML — 至少，這是一般直到到達@DateTime.Now。</span><span class="sxs-lookup"><span data-stu-id="40d35-279">Inside the braces, though, you're adding ordinary HTML — at least, it's ordinary until it gets to @DateTime.Now.</span></span> <span data-ttu-id="40d35-280">這是另一個少量 Razor 程式碼，因此一次您必須在其前面加上 @。</span><span class="sxs-lookup"><span data-stu-id="40d35-280">This is another little bit of Razor code, so again you have to add @ in front of it.</span></span>

<span data-ttu-id="40d35-281">此處的重點是，如果您可以新增條件中的程式碼區塊的上方，並在標記中。</span><span class="sxs-lookup"><span data-stu-id="40d35-281">The point here is that you can add if conditions in both the code block at the top and in the markup.</span></span> <span data-ttu-id="40d35-282">如果您使用 [if] 頁面上，在區塊內的行主體中的條件可以是標記或程式碼。</span><span class="sxs-lookup"><span data-stu-id="40d35-282">If you use an if condition in the body of the page, the lines inside the block can be markup or code.</span></span> <span data-ttu-id="40d35-283">在此情況下，以及每當您混合使用標記和程式碼，則為 true，因為您必須使用將它清除給 ASP.NET 程式碼的所在。</span><span class="sxs-lookup"><span data-stu-id="40d35-283">In that case, and as is true anytime you mix markup and code, you have to use @ to make it clear to ASP.NET where the code is.</span></span>

<span data-ttu-id="40d35-284">執行頁面，然後按一下**送出**。</span><span class="sxs-lookup"><span data-stu-id="40d35-284">Run the page and click **Submit**.</span></span> <span data-ttu-id="40d35-285">此時不只顯示不同的訊息時送出之後 （「 現在您已上傳..."），但您會看到新的訊息，列出的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="40d35-285">This time you not only see a different message when you submit ("Now you've submitted ..."), but you see a new message that lists the date and time.</span></span>

![提交 'Test Razor 2' 的網頁瀏覽器中執行之後所顯示的時間戳記](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a><span data-ttu-id="40d35-287">測試值的查詢字串</span><span class="sxs-lookup"><span data-stu-id="40d35-287">Testing the value of a query string</span></span>

<span data-ttu-id="40d35-288">一項更多測試。</span><span class="sxs-lookup"><span data-stu-id="40d35-288">One more test.</span></span> <span data-ttu-id="40d35-289">此時，您將新增 if 測試一個值的區塊，名為 「 可能會傳遞查詢字串中的顯示。</span><span class="sxs-lookup"><span data-stu-id="40d35-289">This time, you'll add an if block that tests a value named show that might be passed in the query string.</span></span> <span data-ttu-id="40d35-290">(如下所示： `http://localhost:43097/TestRazorPart2.cshtml?show=true`)，以便顯示您已顯示，您要變更頁面 （「 這是第一次...」 等） 只有當顯示的值為 true，才會顯示。</span><span class="sxs-lookup"><span data-stu-id="40d35-290">(Like this: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) You'll change the page so that the message you've been displaying ("This is the first time ...", etc.) is only displayed if the value of show is true.</span></span>

<span data-ttu-id="40d35-291">在底部 （但內部） 程式碼區塊頂端的頁面上，新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="40d35-291">At the bottom (but inside) the code block at the top of the page, add the following:</span></span>

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

<span data-ttu-id="40d35-292">完整的程式碼區塊現在看起來與下列範例。</span><span class="sxs-lookup"><span data-stu-id="40d35-292">The complete code block now look like the following example.</span></span> <span data-ttu-id="40d35-293">（請記住，當您將程式碼複製到您的頁面時，縮排看起來可能不同。</span><span class="sxs-lookup"><span data-stu-id="40d35-293">(Remember that when you copy the code into your page, the indentation might look different.</span></span> <span data-ttu-id="40d35-294">不過，不會影響程式碼的執行方式）。</span><span class="sxs-lookup"><span data-stu-id="40d35-294">But that doesn't affect how the code runs.)</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

<span data-ttu-id="40d35-295">新的程式碼區塊中初始化變數，名為 showMessage 為 false。</span><span class="sxs-lookup"><span data-stu-id="40d35-295">The new code in the block initializes a variable named showMessage to false.</span></span> <span data-ttu-id="40d35-296">然後再執行 if 測試，以尋找查詢字串中的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-296">It then does an if test to look for a value in the query string.</span></span> <span data-ttu-id="40d35-297">當您第一次要求頁面時，它會有如下的 URL:</span><span class="sxs-lookup"><span data-stu-id="40d35-297">When you first request the page, it has a URL like this one:</span></span>

`http://localhost:43097/TestRazorPart2.cshtml`

<span data-ttu-id="40d35-298">程式碼會判斷 URL 是否包含名為查詢字串，例如 URL 的此版本中顯示的變數：</span><span class="sxs-lookup"><span data-stu-id="40d35-298">The code determines whether the URL contains a variable named show in the query string, like this version of the URL:</span></span>

<span data-ttu-id="40d35-299">`http://localhost:43097/TestRazorPart2.cshtml`？ 顯示 = true</span><span class="sxs-lookup"><span data-stu-id="40d35-299">`http://localhost:43097/TestRazorPart2.cshtml`?show=true</span></span>

<span data-ttu-id="40d35-300">測試本身會查看要求物件的查詢字串屬性。</span><span class="sxs-lookup"><span data-stu-id="40d35-300">The test itself looks at the QueryString property of the Request object.</span></span> <span data-ttu-id="40d35-301">如果查詢字串包含項目已命名的顯示，而且該項目的設定為 true，if 區塊執行，並將 showMessage 變數設定為 true。</span><span class="sxs-lookup"><span data-stu-id="40d35-301">If the query string contains an item named show, and if that item is set to true, the if block runs and sets the showMessage variable to true.</span></span>

<span data-ttu-id="40d35-302">如您所見，沒有這裡的技巧。</span><span class="sxs-lookup"><span data-stu-id="40d35-302">There's a trick here, as you can see.</span></span> <span data-ttu-id="40d35-303">如其名，則查詢字串會是字串。</span><span class="sxs-lookup"><span data-stu-id="40d35-303">Like the name says, the query string is a string.</span></span> <span data-ttu-id="40d35-304">不過，您可以只測試 true 和 false 如果您要測試的值是布林 (true/false) 值。</span><span class="sxs-lookup"><span data-stu-id="40d35-304">However, you can only test for true and false if the value you're testing is a Boolean (true/false) value.</span></span> <span data-ttu-id="40d35-305">您可以測試查詢字串中的顯示變數的值之前，您必須將它轉換成布林值。</span><span class="sxs-lookup"><span data-stu-id="40d35-305">Before you can test the value of the show variable in the query string, you have to convert it to a Boolean value.</span></span> <span data-ttu-id="40d35-306">這是 AsBool 方法的功能 — 它會接受字串做為輸入，並將它轉換成布林值。</span><span class="sxs-lookup"><span data-stu-id="40d35-306">That's what the AsBool method does — it takes a string as input and converts it to a Boolean value.</span></span> <span data-ttu-id="40d35-307">很明顯地，如果字串為"true"，AsBool 方法會將該值轉換為 true。</span><span class="sxs-lookup"><span data-stu-id="40d35-307">Clearly, if the string is "true", the AsBool method converts that value to true.</span></span> <span data-ttu-id="40d35-308">如果字串的值可以是任何其他項目，AsBool 會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="40d35-308">If the value of the string is anything else, AsBool returns false.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="40d35-309">**資料類型和 as （） 方法**</span><span class="sxs-lookup"><span data-stu-id="40d35-309">**Data Types and As() Methods**</span></span>
> 
> <span data-ttu-id="40d35-310">我們只能說了到目前為止，當您建立變數時，您使用關鍵字 var。</span><span class="sxs-lookup"><span data-stu-id="40d35-310">We've only said so far that when you create a variable, you use the keyword var.</span></span> <span data-ttu-id="40d35-311">這不是整個本文中，不過。</span><span class="sxs-lookup"><span data-stu-id="40d35-311">That's not the entire story, though.</span></span> <span data-ttu-id="40d35-312">若要操作的值 — 新增數字，或串連字串，或比較日期，或測試的 true/false-C# 必須使用適當的內部表示法的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-312">In order to manipulate values — to add numbers, or concatenate strings, or compare dates, or test for true/false — C# has to work with an appropriate internal representation of the value.</span></span> <span data-ttu-id="40d35-313">C# 可以*通常*找出該表示應該為何 (也就是什麼*型別*資料) 根據您所做的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-313">C# can *usually* figure out what that representation should be (that is, what *type* the data is) based on what you're doing with the values.</span></span> <span data-ttu-id="40d35-314">不過它無法這麼做。</span><span class="sxs-lookup"><span data-stu-id="40d35-314">Now and then, though, it can't do that.</span></span> <span data-ttu-id="40d35-315">如果沒有，您必須藉由明確指出 C# 應該代表資料的方式來助一臂之力。</span><span class="sxs-lookup"><span data-stu-id="40d35-315">If not, you have to help out by explicitly indicating how C# should represent the data.</span></span> <span data-ttu-id="40d35-316">AsBool 方法這麼 — 它會告訴 C#"true"或"false"的字串值，應該視為布林值。</span><span class="sxs-lookup"><span data-stu-id="40d35-316">The AsBool method does that — it tells C# that a string value of "true" or "false" should be treated as a Boolean value.</span></span> <span data-ttu-id="40d35-317">類似的方法可用來做為其他型別，表示字串，例如 AsInt （視為整數）、 AsDateTime （視為日期/時間）、 AsFloat （視為浮點數），依此類推。</span><span class="sxs-lookup"><span data-stu-id="40d35-317">Similar methods exist to represent strings as other types as well, like AsInt (treat as an integer), AsDateTime (treat as a date/time), AsFloat (treat as a floating-point number), and so on.</span></span> <span data-ttu-id="40d35-318">當您使用這些作為 （） 方法，如果要求 C# 無法表示的字串值時，您會看到錯誤。</span><span class="sxs-lookup"><span data-stu-id="40d35-318">When you use these As( ) methods, if C# can't represent the string value as requested, you'll see an error.</span></span>


<span data-ttu-id="40d35-319">在頁面標記中，移除或註解此項目 （在此它會顯示標成註解）：</span><span class="sxs-lookup"><span data-stu-id="40d35-319">In the markup of the page, remove or comment out this element (here it's shown commented out):</span></span>

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

<span data-ttu-id="40d35-320">權限，讓您移除或標記為註解文字，新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="40d35-320">Right where you removed or commented out that text, add the following:</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

<span data-ttu-id="40d35-321">如果測試顯示，則為 true，showMessage 變數是否呈現&lt;p&gt;與訊息變數的值的項目。</span><span class="sxs-lookup"><span data-stu-id="40d35-321">The if test says that if the showMessage variable is true, render a &lt;p&gt; element with the value of the message variable.</span></span>

### <a name="summary-of-your-conditional-logic"></a><span data-ttu-id="40d35-322">您的條件式邏輯的摘要</span><span class="sxs-lookup"><span data-stu-id="40d35-322">Summary of your conditional logic</span></span>

<span data-ttu-id="40d35-323">如果您不清楚什麼您剛才所完成的此為摘要。</span><span class="sxs-lookup"><span data-stu-id="40d35-323">In case you're not entirely sure of what you've just done, here's a summary.</span></span>

- <span data-ttu-id="40d35-324">訊息變數會初始化為預設的字串 ("This is 第一次...")。</span><span class="sxs-lookup"><span data-stu-id="40d35-324">The message variable is initialized to a default string ("This is the first time ...").</span></span>
- <span data-ttu-id="40d35-325">如果頁面要求 (post) 提交的結果，訊息的值會變成 「 現在您已提交...」</span><span class="sxs-lookup"><span data-stu-id="40d35-325">If the page request is the result of a submit (post), the value of message is changed to "Now you've submitted ..."</span></span>
- <span data-ttu-id="40d35-326">ShowMessage 變數會初始化為 false。</span><span class="sxs-lookup"><span data-stu-id="40d35-326">The showMessage variable is initialized to false.</span></span>
- <span data-ttu-id="40d35-327">如果查詢字串包含？ 顯示 = showMessage 變數會設定為 true，則為 true。</span><span class="sxs-lookup"><span data-stu-id="40d35-327">If the query string contains ?show=true, the showMessage variable is set to true.</span></span>
- <span data-ttu-id="40d35-328">在標記中，如果 showMessage 成立&lt;p&gt;項目會呈現，顯示訊息的值。</span><span class="sxs-lookup"><span data-stu-id="40d35-328">In the markup, if showMessage is true, a &lt;p&gt; element is rendered that shows the value of message.</span></span> <span data-ttu-id="40d35-329">（如果 showMessage 為 false，不會呈現在該點的標記中。）</span><span class="sxs-lookup"><span data-stu-id="40d35-329">(If showMessage is false, nothing is rendered at that point in the markup.)</span></span>
- <span data-ttu-id="40d35-330">在標記中，如果要求是文章時， &lt;p&gt;項目會呈現可顯示的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="40d35-330">In the markup, if the request is a post, a &lt;p&gt; element is rendered that displays the date and time.</span></span>

<span data-ttu-id="40d35-331">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="40d35-331">Run the page.</span></span> <span data-ttu-id="40d35-332">沒有任何訊息，因為 showMessage 是 false，因此在標記中 if(showMessage) 測試會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="40d35-332">There's no message, because showMessage is false, so in the markup the if(showMessage) test returns false.</span></span>

<span data-ttu-id="40d35-333">按一下 [提交]。</span><span class="sxs-lookup"><span data-stu-id="40d35-333">Click **Submit**.</span></span> <span data-ttu-id="40d35-334">您會看到日期和時間，但仍然沒有任何訊息。</span><span class="sxs-lookup"><span data-stu-id="40d35-334">You see the date and time, but still no message.</span></span>

<span data-ttu-id="40d35-335">在瀏覽器中移至 URL 方塊中，並將下列新增至 URL 的結尾:？ 顯示 = true，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="40d35-335">In your browser, go to the URL box and add the following to the end of the URL: ?show=true and then press Enter.</span></span>

![在瀏覽器中顯示查詢字串 'Test Razor 2' 的頁面](intro-to-web-pages-programming/_static/image5.png)

<span data-ttu-id="40d35-337">頁面會再次顯示。</span><span class="sxs-lookup"><span data-stu-id="40d35-337">The page is displayed again.</span></span> <span data-ttu-id="40d35-338">（因為您變更 URL，這是新要求，而不送出）。按一下 **送出**一次。</span><span class="sxs-lookup"><span data-stu-id="40d35-338">(Because you changed the URL, this is a new request, not a submit.) Click **Submit** again.</span></span> <span data-ttu-id="40d35-339">因為日期和時間時，會一次，顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="40d35-339">The message is displayed again, as is the date and time.</span></span>

![查詢字串時，送出後 'Test Razor 2' 的頁面](intro-to-web-pages-programming/_static/image6.png)

<span data-ttu-id="40d35-341">在 URL 中，變更嗎？ 顯示 true =？ 顯示 = false，然後按 Enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="40d35-341">In the URL, change ?show=true to ?show=false and press Enter.</span></span> <span data-ttu-id="40d35-342">提交頁面一次。</span><span class="sxs-lookup"><span data-stu-id="40d35-342">Submit the page again.</span></span> <span data-ttu-id="40d35-343">頁面會回到您的啟動方式 — 任何訊息。</span><span class="sxs-lookup"><span data-stu-id="40d35-343">The page is back to how you started — no message.</span></span>

<span data-ttu-id="40d35-344">如先前所述，此範例中的邏輯是有點自然。</span><span class="sxs-lookup"><span data-stu-id="40d35-344">As noted earlier, the logic of this example is a little contrived.</span></span> <span data-ttu-id="40d35-345">不過，如果要出現在許多頁面，而且將會需要一或多個您已了解的形式這裡。</span><span class="sxs-lookup"><span data-stu-id="40d35-345">However, if is going to come up in many of your pages, and it will take one or more of the forms you've seen here.</span></span>

## <a name="installing-a-helper-displaying-a-gravatar-image"></a><span data-ttu-id="40d35-346">安裝協助程式 （顯示 Gravatar 影像）</span><span class="sxs-lookup"><span data-stu-id="40d35-346">Installing a Helper (Displaying a Gravatar Image)</span></span>

<span data-ttu-id="40d35-347">使用者通常會想要在網頁上執行某些工作需要大量的程式碼，或需要額外的知識。</span><span class="sxs-lookup"><span data-stu-id="40d35-347">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="40d35-348">範例： 顯示資料的圖表將 Facebook [讚] 按鈕放在頁面的從您的網站; 傳送電子郵件裁剪或調整大小的影像;為您的網站使用 PayPal。</span><span class="sxs-lookup"><span data-stu-id="40d35-348">Examples: displaying a chart for data; putting a Facebook "Like" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="40d35-349">若要讓您輕鬆地執行這類作業，ASP.NET Web Pages 可讓您使用*協助程式*。</span><span class="sxs-lookup"><span data-stu-id="40d35-349">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="40d35-350">協助程式是您安裝站台，並讓您執行一般工作所使用的 Razor 程式碼只需要幾行的元件。</span><span class="sxs-lookup"><span data-stu-id="40d35-350">Helpers are components that you install for a site and that let you perform typical tasks by using just a few lines of Razor code.</span></span>

<span data-ttu-id="40d35-351">ASP.NET Web Pages 有幾個內建的協助程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-351">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="40d35-352">不過，許多協助程式可使用 NuGet 套件管理員提供的封裝 （增益集） 中。</span><span class="sxs-lookup"><span data-stu-id="40d35-352">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="40d35-353">NuGet 可讓您選取要安裝的套件，然後它會負責安裝的所有詳細資料。</span><span class="sxs-lookup"><span data-stu-id="40d35-353">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

<span data-ttu-id="40d35-354">在這個部分的教學課程中，您將安裝的協助程式可讓您顯示 Gravatar （「 全域可辨識的顯示圖片 」） 映像。</span><span class="sxs-lookup"><span data-stu-id="40d35-354">In this part of the tutorial, you'll install a helper that lets you display a Gravatar ("globally recognized avatar") image.</span></span> <span data-ttu-id="40d35-355">您將了解兩件事。</span><span class="sxs-lookup"><span data-stu-id="40d35-355">You'll learn two things.</span></span> <span data-ttu-id="40d35-356">其中一個是如何尋找及安裝協助程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-356">One is how to find and install a helper.</span></span> <span data-ttu-id="40d35-357">此外，您也將了解如何協助程式輕鬆地進行否則您必須利用許多您必須自行撰寫的程式碼來完成。</span><span class="sxs-lookup"><span data-stu-id="40d35-357">You'll also learn how a helper makes it easy to do something you'd otherwise need to do by using a lot of code you'd have to write yourself.</span></span>

<span data-ttu-id="40d35-358">您可以註冊 Gravatar 網站在您自己 Gravatar [ http://www.gravatar.com/ ](http://www.gravatar.com/)，但不需要建立 Gravatar 帳戶來執行本教學課程的這個部分。</span><span class="sxs-lookup"><span data-stu-id="40d35-358">You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/), but it is not essential to create a Gravatar account to perform this part of the tutorial.</span></span>

<span data-ttu-id="40d35-359">在 WebMatrix 中，按一下**NuGet**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="40d35-359">In WebMatrix, click the **NuGet** button.</span></span>

![在 WebMatrix 中的 [NuGet 資源庫] 對話方塊](intro-to-web-pages-programming/_static/image7.png)

<span data-ttu-id="40d35-361">這會啟動 NuGet 套件管理員，並顯示可用的套件。</span><span class="sxs-lookup"><span data-stu-id="40d35-361">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="40d35-362">(不是所有的封裝是協助程式，一些將功能新增至本身的 WebMatrix，一些其他範本，並依此類推。)您可能會收到有關版本不相容的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="40d35-362">(Not all of the packages are helpers; some add functionality to WebMatrix itself, some are additional templates, and so on.) You may get an error message about version incompatibility.</span></span> <span data-ttu-id="40d35-363">您可以忽略此錯誤訊息，依序按一下**確定**和繼續進行本教學課程。</span><span class="sxs-lookup"><span data-stu-id="40d35-363">You can ignore this error message by clicking **OK** and proceeding with this tutorial.</span></span>

![在 WebMatrix 中的 [NuGet 資源庫] 對話方塊](intro-to-web-pages-programming/_static/image8.png)

<span data-ttu-id="40d35-365">在 [搜尋] 方塊中，輸入 「 asp.net 協助程式 」。</span><span class="sxs-lookup"><span data-stu-id="40d35-365">In the search box, enter "asp.net helpers".</span></span> <span data-ttu-id="40d35-366">NuGet 會顯示符合搜尋條件的封裝。</span><span class="sxs-lookup"><span data-stu-id="40d35-366">NuGet shows the packages that match the search terms.</span></span>

![在 WebMatrix 中顯示套件的 NuGet 資源庫](intro-to-web-pages-programming/_static/image9.png)

<span data-ttu-id="40d35-368">ASP.NET Web Helpers Library 包含程式碼，以簡化許多常見的工作，包括使用 Gravatar 影像。</span><span class="sxs-lookup"><span data-stu-id="40d35-368">The ASP.NET Web Helpers Library contains code to simplify many common tasks, including the use of Gravatar images.</span></span> <span data-ttu-id="40d35-369">選取  **ASP.NET Web Helpers Library**封裝，然後按一下**安裝**啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-369">Select the **ASP.NET Web Helpers Library** package and then click **Install** to launch the installer.</span></span> <span data-ttu-id="40d35-370">選取 **是**時詢問您是否要安裝套件，並接受條款才能完成安裝。</span><span class="sxs-lookup"><span data-stu-id="40d35-370">Select **Yes** when asked if you want to install the package, and accept the terms to complete the installation.</span></span>

<span data-ttu-id="40d35-371">就這麼容易！</span><span class="sxs-lookup"><span data-stu-id="40d35-371">That's it.</span></span> <span data-ttu-id="40d35-372">NuGet 會下載並安裝所有項目，包括任何可能需要的其他元件 (*相依性*)。</span><span class="sxs-lookup"><span data-stu-id="40d35-372">NuGet downloads and installs everything, including any additional components that might be required (*dependencies*).</span></span>

<span data-ttu-id="40d35-373">如果基於某些原因，您必須解除安裝的協助程式，此程序是非常類似。</span><span class="sxs-lookup"><span data-stu-id="40d35-373">If for some reason you have to uninstall a helper, the process is very similar.</span></span> <span data-ttu-id="40d35-374">按一下  **NuGet**按鈕，再按一下**已安裝**索引標籤，然後挑選您想要解除安裝的封裝。</span><span class="sxs-lookup"><span data-stu-id="40d35-374">Click the **NuGet** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="using-a-helper-in-a-page"></a><span data-ttu-id="40d35-375">在網頁中使用協助程式</span><span class="sxs-lookup"><span data-stu-id="40d35-375">Using a Helper in a Page</span></span>

<span data-ttu-id="40d35-376">現在您可以使用您剛才安裝的協助程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-376">Now you'll use the helper that you just installed.</span></span> <span data-ttu-id="40d35-377">加入網頁中的協助程式的程序是類似的大多數協助程式。</span><span class="sxs-lookup"><span data-stu-id="40d35-377">The process for adding a helper to a page is similar for most helpers.</span></span>

<span data-ttu-id="40d35-378">在 WebMatrix 中，建立網頁並將它命名*GravatarTest.cshml*。</span><span class="sxs-lookup"><span data-stu-id="40d35-378">In WebMatrix, create a page and name it *GravatarTest.cshml*.</span></span> <span data-ttu-id="40d35-379">（您要建立特殊的頁面，即可測試協助程式，但您可以使用協助程式在您的網站中的任何分頁）。</span><span class="sxs-lookup"><span data-stu-id="40d35-379">(You're creating a special page to test the helper, but you can use helpers in any page in your site.)</span></span>

<span data-ttu-id="40d35-380">內部&lt;主體&gt;項目，新增&lt;div&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="40d35-380">Inside the &lt;body&gt; element, add a &lt;div&gt; element.</span></span> <span data-ttu-id="40d35-381">內部&lt;div&gt;項目中，輸入：</span><span class="sxs-lookup"><span data-stu-id="40d35-381">Inside the &lt;div&gt; element, type this:</span></span>

<span data-ttu-id="40d35-382">@Gravatar.</span><span class="sxs-lookup"><span data-stu-id="40d35-382">@Gravatar.</span></span>

<span data-ttu-id="40d35-383">@ 字元則是您已將 Razor 程式碼中使用的相同字元。</span><span class="sxs-lookup"><span data-stu-id="40d35-383">The @ character is the same character you've been using to mark Razor code.</span></span> <span data-ttu-id="40d35-384">**Gravatar**是您正在使用的協助程式物件。</span><span class="sxs-lookup"><span data-stu-id="40d35-384">**Gravatar** is the helper object that you're working with.</span></span>

<span data-ttu-id="40d35-385">只要您輸入句號 （.） 時，WebMatrix 會顯示一份*方法*Gravatar 協助程式會提供 （函數）：</span><span class="sxs-lookup"><span data-stu-id="40d35-385">As soon as you type the period (.), WebMatrix displays a list of *methods* (functions) that the Gravatar helper makes available:</span></span>

![Gravatar helper IntelliSense 下拉式清單](intro-to-web-pages-programming/_static/image10.png)

<span data-ttu-id="40d35-387">這項功能就所謂*IntelliSense*。</span><span class="sxs-lookup"><span data-stu-id="40d35-387">This feature is known as *IntelliSense*.</span></span> <span data-ttu-id="40d35-388">它會協助您的程式碼藉由提供適當的內容選項。</span><span class="sxs-lookup"><span data-stu-id="40d35-388">It helps you code by providing context-appropriate choices.</span></span> <span data-ttu-id="40d35-389">IntelliSense 會使用 HTML、 CSS、 ASP.NET 程式碼、 JavaScript 和 WebMatrix 支援其他語言。</span><span class="sxs-lookup"><span data-stu-id="40d35-389">IntelliSense works with HTML, CSS, ASP.NET code, JavaScript, and other languages that are supported in WebMatrix.</span></span> <span data-ttu-id="40d35-390">它是另一項功能，可讓您更輕鬆地開發在 WebMatrix 中的網頁。</span><span class="sxs-lookup"><span data-stu-id="40d35-390">It's another feature that makes it easier to develop web pages in WebMatrix.</span></span>

<span data-ttu-id="40d35-391">按 G 鍵盤，然後您會看到 IntelliSense 尋找 GetHtml 方法。</span><span class="sxs-lookup"><span data-stu-id="40d35-391">Press G on the keyboard, and you see that IntelliSense finds the GetHtml method.</span></span> <span data-ttu-id="40d35-392">按 Tab 鍵。IntelliSense 會為您插入選取的方法 (GetHtml)。</span><span class="sxs-lookup"><span data-stu-id="40d35-392">Press Tab. IntelliSense inserts the selected method (GetHtml) for you.</span></span> <span data-ttu-id="40d35-393">輸入左括號，並請注意，自動加入右括弧。</span><span class="sxs-lookup"><span data-stu-id="40d35-393">Type an open parenthesis, and notice that the closing parenthesis is automatically added.</span></span> <span data-ttu-id="40d35-394">在引號內的兩個括號之間輸入您的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="40d35-394">Type your email address in quotation marks between the two parenthesis.</span></span> <span data-ttu-id="40d35-395">如果您有 Gravatar 帳戶時，就會傳回您的個人資料圖片。</span><span class="sxs-lookup"><span data-stu-id="40d35-395">If you have a Gravatar account, your profile picture will be returned.</span></span> <span data-ttu-id="40d35-396">如果您沒有 Gravatar 帳戶，則會傳回預設映像。</span><span class="sxs-lookup"><span data-stu-id="40d35-396">If you do not have a Gravatar account, a default image is returned.</span></span> <span data-ttu-id="40d35-397">當您完成時，該行看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="40d35-397">When you're done, the line looks like this:</span></span>

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

<span data-ttu-id="40d35-398">現在檢視的網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="40d35-398">Now view the page in a browser.</span></span> <span data-ttu-id="40d35-399">您的圖片或預設的影像會顯示，根據您所擁有的 Gravatar 帳戶。</span><span class="sxs-lookup"><span data-stu-id="40d35-399">Either your picture or the default image is displayed, depending on whether you have a Gravatar account.</span></span>

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![預設映像](intro-to-web-pages-programming/_static/image12.png)

<span data-ttu-id="40d35-402">若要了解的功能為您執行協助程式，在瀏覽器中檢視頁面的來源。</span><span class="sxs-lookup"><span data-stu-id="40d35-402">To get an idea of what the helper is doing for you, view the source of the page in the browser.</span></span> <span data-ttu-id="40d35-403">以及 HTML 網頁中，您會看到影像項目，其中包含識別項。</span><span class="sxs-lookup"><span data-stu-id="40d35-403">Along with the HTML that you had in your page, you see an image element that includes an identifier.</span></span> <span data-ttu-id="40d35-404">這是協助程式轉譯成 [在您擁有的位置] 頁面的程式碼@Gravatar.GetHtml。</span><span class="sxs-lookup"><span data-stu-id="40d35-404">This is code that the helper rendered into the page at the place where you had @Gravatar.GetHtml.</span></span> <span data-ttu-id="40d35-405">協助程式花費了您所提供，並產生與直接對 Gravatar，若要提供的帳戶取得回正確的映像的程式碼的資訊。</span><span class="sxs-lookup"><span data-stu-id="40d35-405">The helper took the information you provided and generated the code that talks directly to Gravatar in order to get back the correct image for supplied account.</span></span>

<span data-ttu-id="40d35-406">GetHtml 方法也可讓您自訂映像，藉由提供其他參數。</span><span class="sxs-lookup"><span data-stu-id="40d35-406">The GetHtml method also enables you to customize the image by providing other parameters.</span></span> <span data-ttu-id="40d35-407">下列程式碼示範如何要求影像寬度和高度為 40 像素，並使用名為指定的預設映像**wavatar**如果指定的帳戶不存在。</span><span class="sxs-lookup"><span data-stu-id="40d35-407">The following code shows how to request an image has a width and height of 40 pixels, and uses a specified default image named **wavatar** if the specified account does not exist.</span></span>

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

<span data-ttu-id="40d35-408">此程式碼會產生類似下列的結果 （預設映像隨機會有所不同）。</span><span class="sxs-lookup"><span data-stu-id="40d35-408">This code produces something like the following result (the default image will randomly vary).</span></span>

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a><span data-ttu-id="40d35-409">接下來的下一步</span><span class="sxs-lookup"><span data-stu-id="40d35-409">Coming Up Next</span></span>

<span data-ttu-id="40d35-410">為了讓本教學課程簡短，我們將焦點放在只有少數的基本概念。</span><span class="sxs-lookup"><span data-stu-id="40d35-410">To keep this tutorial short, we had to focus on only a few basics.</span></span> <span data-ttu-id="40d35-411">當然，還有*許多*Razor 和 C# 更多。</span><span class="sxs-lookup"><span data-stu-id="40d35-411">Naturally, there's a *lot* more to Razor and C#.</span></span> <span data-ttu-id="40d35-412">您將了解更多您瀏覽這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="40d35-412">You'll learn more as you go through these tutorials.</span></span> <span data-ttu-id="40d35-413">如果您想要立即深入了解 Razor 和 C# 程式設計的層面，您可以閱讀更全面的簡介： [ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkID=202890)。</span><span class="sxs-lookup"><span data-stu-id="40d35-413">If you're interested in learning more about the programming aspects of Razor and C# right now, you can read a more thorough introduction here: [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=202890).</span></span>

<span data-ttu-id="40d35-414">下一個教學課程會介紹您使用的資料庫。</span><span class="sxs-lookup"><span data-stu-id="40d35-414">The next tutorial introduces you to working with a database.</span></span> <span data-ttu-id="40d35-415">在該教學課程中，您會開始建立範例應用程式，可讓您列出您最愛的電影。</span><span class="sxs-lookup"><span data-stu-id="40d35-415">In that tutorial, you'll begin creating the sample application that lets you list your favorite movies.</span></span>

## <a name="complete-listing-for-testrazor-page"></a><span data-ttu-id="40d35-416">TestRazor 頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="40d35-416">Complete Listing for TestRazor Page</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a><span data-ttu-id="40d35-417">TestRazorPart2 頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="40d35-417">Complete Listing for TestRazorPart2 Page</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a><span data-ttu-id="40d35-418">GravatarTest 頁面的完整清單</span><span class="sxs-lookup"><span data-stu-id="40d35-418">Complete Listing for GravatarTest Page</span></span>

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="40d35-419">其他資源</span><span class="sxs-lookup"><span data-stu-id="40d35-419">Additional Resources</span></span>

- [<span data-ttu-id="40d35-420">使用 Razor 語法的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="40d35-420">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- [<span data-ttu-id="40d35-421">Twitter 協助程式</span><span class="sxs-lookup"><span data-stu-id="40d35-421">Twitter helper</span></span>](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> <span data-ttu-id="40d35-422">[上一頁](getting-started.md)
> [下一頁](displaying-data.md)</span><span class="sxs-lookup"><span data-stu-id="40d35-422">[Previous](getting-started.md)
[Next](displaying-data.md)</span></span>
