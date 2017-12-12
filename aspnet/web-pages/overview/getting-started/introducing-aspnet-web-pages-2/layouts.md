---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: "導入 ASP.NET Web Pages-建立一致的版面配置 |Microsoft 文件"
author: tfitzmac
description: "本教學課程會示範如何使用配置，以使用 ASP.NET Web Pages 站台建立外觀一致的頁面。 它會假設您已經完成..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 692adc5a03892f27c91fe8868c8eab6ce08f49cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="85c18-104">導入的 ASP.NET Web Pages-建立一致的版面配置</span><span class="sxs-lookup"><span data-stu-id="85c18-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="85c18-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="85c18-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="85c18-106">本教學課程會示範如何使用*配置*使用 ASP.NET Web Pages 站台上建立一致的外觀的頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="85c18-107">它會假設您已完成透過數列[刪除 ASP.NET Web Pages 中的資料庫資料](https://go.microsoft.com/fwlink/?LinkId=251584)。</span><span class="sxs-lookup"><span data-stu-id="85c18-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="85c18-108">您將學習：</span><span class="sxs-lookup"><span data-stu-id="85c18-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="85c18-109">版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="85c18-109">What a layout page is.</span></span>
> - <span data-ttu-id="85c18-110">如何結合動態內容的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="85c18-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="85c18-111">如何將值傳遞至版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="85c18-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="85c18-112">關於配置</span><span class="sxs-lookup"><span data-stu-id="85c18-112">About Layouts</span></span>

<span data-ttu-id="85c18-113">到目前為止，您已建立的頁面所有已完成之後，獨立的頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="85c18-114">它們都屬於相同的站台，但是沒有任何共同的項目或標準的外觀。</span><span class="sxs-lookup"><span data-stu-id="85c18-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="85c18-115">大部分的站台沒有一致的外觀和版面配置。</span><span class="sxs-lookup"><span data-stu-id="85c18-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="85c18-116">例如，如果您移至[Microsoft.com/web](https://www.microsoft.com/web/)站台和尋找，，您會看到所有頁面都遵守整體的版面配置，並視覺化佈景主題：</span><span class="sxs-lookup"><span data-stu-id="85c18-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![顯示標頭、 導覽區域中，內容區域和頁尾的配置 Microsoft.com/web 網站頁面](layouts/_static/image1.png)

<span data-ttu-id="85c18-118">*效率不佳*建立這個配置的方式是在每個頁面上個別定義標頭、 導覽列中和頁尾。</span><span class="sxs-lookup"><span data-stu-id="85c18-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="85c18-119">會被複製的相同標記每次。</span><span class="sxs-lookup"><span data-stu-id="85c18-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="85c18-120">如果您想要變更的項目 （例如，更新頁尾），您必須個別變更每個頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="85c18-121">這是 where*版面配置頁*出現。</span><span class="sxs-lookup"><span data-stu-id="85c18-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="85c18-122">ASP.NET Web Pages 中，您可以定義提供整體的容器網站上頁面的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="85c18-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="85c18-123">例如，[配置] 頁面可以包含標頭、 導覽區域中和頁尾。</span><span class="sxs-lookup"><span data-stu-id="85c18-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="85c18-124">[配置] 頁面包含的主要內容前往的地方的預留位置。</span><span class="sxs-lookup"><span data-stu-id="85c18-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="85c18-125">接著，您可以定義個別內容的網頁包含標記和程式碼，只有該頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="85c18-126">內容頁面不必是完整的 HTML 網頁。即使不一定要有`<body>`項目。</span><span class="sxs-lookup"><span data-stu-id="85c18-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="85c18-127">他們也擁有您想要顯示的內容中哪些版面配置頁會告知 ASP.NET 的程式碼行。</span><span class="sxs-lookup"><span data-stu-id="85c18-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="85c18-128">以下是此關聯性的運作方式大致上顯示的圖片：</span><span class="sxs-lookup"><span data-stu-id="85c18-128">Here's a picture that shows roughly how this relationship works:</span></span>

![顯示兩個內容頁面，以及它們所容納的版面配置頁的概念圖](layouts/_static/image2.png)

<span data-ttu-id="85c18-130">這種互動很容易了解何時觀看它的運作。</span><span class="sxs-lookup"><span data-stu-id="85c18-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="85c18-131">在本教學課程中，您要變更您的電影網頁使用的配置。</span><span class="sxs-lookup"><span data-stu-id="85c18-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="85c18-132">加入版面配置頁</span><span class="sxs-lookup"><span data-stu-id="85c18-132">Adding a Layout Page</span></span>

<span data-ttu-id="85c18-133">您會先建立定義具有頁首、 頁尾及主要內容區域的一般版面配置頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="85c18-134">在 WebPagesMovies 網站中，加入名為的 CSHTML 頁面 *\_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="85c18-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="85c18-135">前置底線 ( `_` ) 字元是很重要。</span><span class="sxs-lookup"><span data-stu-id="85c18-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="85c18-136">如果頁面名稱會以底線開頭，ASP.NET 不會直接將該頁面傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="85c18-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="85c18-137">這個慣例可讓您定義所需的網站，但使用者不應該能夠直接要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="85c18-138">中頁面的內容取代為下面：</span><span class="sxs-lookup"><span data-stu-id="85c18-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="85c18-139">如您所見，此標記是只會使用的 HTML`<div>`更定義三個區段中的頁面加一的項目`<div>`保存三個區段的項目。</span><span class="sxs-lookup"><span data-stu-id="85c18-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="85c18-140">頁尾包含 Razor 程式碼的位元： `@DateTime.Now.Year`，它會呈現目前年度網頁中的該位置。</span><span class="sxs-lookup"><span data-stu-id="85c18-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="85c18-141">請注意，有名稱為樣式表的連結*Movies.css*。</span><span class="sxs-lookup"><span data-stu-id="85c18-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="85c18-142">樣式表是將要定義之元素的實際配置的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="85c18-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="85c18-143">您將在不久後建立的。</span><span class="sxs-lookup"><span data-stu-id="85c18-143">You'll create that in a moment.</span></span>

<span data-ttu-id="85c18-144">唯一不尋常的功能在這個 *\_Layout.cshtml*頁面是`@Render.Body()`列。</span><span class="sxs-lookup"><span data-stu-id="85c18-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="85c18-145">這是的預留位置，內容位置和另一個頁面合併此版面配置時。</span><span class="sxs-lookup"><span data-stu-id="85c18-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="85c18-146">若要加入.css 檔案</span><span class="sxs-lookup"><span data-stu-id="85c18-146">Adding a .css File</span></span>

<span data-ttu-id="85c18-147">在頁面上定義的項目實際的排列 （亦即，外觀），最好是使用階層式樣式表 (CSS) 規則。</span><span class="sxs-lookup"><span data-stu-id="85c18-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="85c18-148">因此，您會建立*.css*檔案，您的新配置的規則。</span><span class="sxs-lookup"><span data-stu-id="85c18-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="85c18-149">在 WebMatrix 中，選取您網站的根。</span><span class="sxs-lookup"><span data-stu-id="85c18-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="85c18-150">接著在**檔案**] 索引標籤的 [功能區中，按一下底下的箭頭**新增**按鈕，然後按一下**新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="85c18-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![在功能區 下 新資料夾 選項。](layouts/_static/image3.png)

<span data-ttu-id="85c18-152">將新的資料夾命名*樣式*。</span><span class="sxs-lookup"><span data-stu-id="85c18-152">Name the new folder *Styles*.</span></span>

![命名新的資料夾 '樣式'](layouts/_static/image4.png)

<span data-ttu-id="85c18-154">在新*樣式*資料夾中，建立名為*Movies.css*。</span><span class="sxs-lookup"><span data-stu-id="85c18-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![建立新的 Movies.css 檔](layouts/_static/image5.png)

<span data-ttu-id="85c18-156">新的內容取代*.css*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="85c18-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="85c18-157">我們將不會假設這些 CSS 規則，但要注意兩件事太多。</span><span class="sxs-lookup"><span data-stu-id="85c18-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="85c18-158">其中一個是，除了設定字型和大小，規則使用絕對位置來建立頁首、 頁尾及主要內容區域的位置。</span><span class="sxs-lookup"><span data-stu-id="85c18-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="85c18-159">如果您還不熟悉定位在 CSS 中，您可以閱讀[CSS 位置](http://www.w3schools.com/css/css_positioning.asp)教學課程 W3Schools 站台。</span><span class="sxs-lookup"><span data-stu-id="85c18-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="85c18-160">請注意在底部，我們已複製原本使用的樣式規則中分別定義另一項作業*Movies.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="85c18-161">這些規則中所使用[顯示資料所使用的 ASP.NET Web Pages 簡介](https://go.microsoft.com/fwlink/?LinkId=251580)進行教學課程`WebGrid`helper 呈現新增等量分散到資料表的標記。</span><span class="sxs-lookup"><span data-stu-id="85c18-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="85c18-162">(如果您打算使用*.css*檔案樣式定義，您可能也放整個網站的樣式規則中。)</span><span class="sxs-lookup"><span data-stu-id="85c18-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="85c18-163">更新要使用的版面配置的電影檔案</span><span class="sxs-lookup"><span data-stu-id="85c18-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="85c18-164">現在您可以更新您的網站才能使用新的版面配置中的現有檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="85c18-165">開啟*Movies.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="85c18-166">在頂端，做為第一行程式碼，加入下列內容：</span><span class="sxs-lookup"><span data-stu-id="85c18-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="85c18-167">頁面現在開始這種方式是：</span><span class="sxs-lookup"><span data-stu-id="85c18-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="85c18-168">這一行程式碼會告知 ASP.NET，當*電影*頁面會執行，它應該與合併 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="85c18-169">因為*Movies.cshtml*檔案現在會使用版面配置頁面，您可以移除從標記*Movies.cshtml*具有所處理的頁面 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="85c18-170">取出`<!DOCTYPE>`， `<html>`，和`<body>`開頭和結尾標記。</span><span class="sxs-lookup"><span data-stu-id="85c18-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="85c18-171">取出整個`<head>`項目和其內容，其中包括格線的樣式規則，因為您現在有這些規則中*.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="85c18-172">當您在到達時，變更現有`<h1>`元素`<h2>`項目; 您`<h1>`已經 [配置] 頁面中的項目。</span><span class="sxs-lookup"><span data-stu-id="85c18-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="85c18-173">變更`<h2>`」 清單電影"的文字。</span><span class="sxs-lookup"><span data-stu-id="85c18-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="85c18-174">通常您不會有這些類型的變更，讓內容頁面中。</span><span class="sxs-lookup"><span data-stu-id="85c18-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="85c18-175">當您啟動您的站台與版面配置頁面時，會開始建立沒有所有這些項目的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="85c18-176">在此情況下，不過，您要轉換的獨立頁面使用版面配置，所以清除的位元的其中一個。</span><span class="sxs-lookup"><span data-stu-id="85c18-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="85c18-177">當您完成時， *Movies.cshtml*頁面將如下所示：</span><span class="sxs-lookup"><span data-stu-id="85c18-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="85c18-178">測試版面配置</span><span class="sxs-lookup"><span data-stu-id="85c18-178">Testing the Layout</span></span>

<span data-ttu-id="85c18-179">現在您可以看到版面配置的外觀。</span><span class="sxs-lookup"><span data-stu-id="85c18-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="85c18-180">在 WebMatrix 中，以滑鼠右鍵按一下*Movies.cshtml*頁面，然後選取**瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="85c18-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="85c18-181">當瀏覽器顯示頁面時，它看起來此頁面：</span><span class="sxs-lookup"><span data-stu-id="85c18-181">When the browser displays the page, it looks like this page:</span></span>

![使用配置轉譯影片頁面](layouts/_static/image6.png)

<span data-ttu-id="85c18-183">ASP.NET 已合併到 Movies.cshtml 頁面的內容 *\_Layout.cshtml*頁面右 where`RenderBody`方法。</span><span class="sxs-lookup"><span data-stu-id="85c18-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="85c18-184">和當然 *\_Layout.cshtml*頁面參考*.css*檔案，定義網頁的外觀。</span><span class="sxs-lookup"><span data-stu-id="85c18-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="85c18-185">更新要使用配置 AddMovie 頁面</span><span class="sxs-lookup"><span data-stu-id="85c18-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="85c18-186">配置的好處是，您可以使用它們的所有頁面中您的網站。</span><span class="sxs-lookup"><span data-stu-id="85c18-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="85c18-187">開啟*AddMovie.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="85c18-188">您可能會記住*AddMovie.cshtml*頁面原來有一些 CSS 規則來定義驗證錯誤訊息的外觀。</span><span class="sxs-lookup"><span data-stu-id="85c18-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="85c18-189">由於您*.css*現在您網站的檔案，您可以移動至這些規則*.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="85c18-190">移除*AddMovie.cshtml*檔案，並將它們加入到底部*Movies.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="85c18-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="85c18-191">您要移動的下列規則：</span><span class="sxs-lookup"><span data-stu-id="85c18-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="85c18-192">現在進行中的變更相同的排序*AddMovie.cshtml*進行*Movies.cshtml* — 新增`Layout="~/_Layout.cshtml;`並移除現在是多餘的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="85c18-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="85c18-193">變更`<h1>`元素`<h2>`。</span><span class="sxs-lookup"><span data-stu-id="85c18-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="85c18-194">當您完成時，頁面看起來像本範例中：</span><span class="sxs-lookup"><span data-stu-id="85c18-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="85c18-195">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="85c18-195">Run the page.</span></span> <span data-ttu-id="85c18-196">現在它看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="85c18-196">Now it looks like this illustration:</span></span>

!['Add 電影' 的頁面使用配置轉譯](layouts/_static/image7.png)

<span data-ttu-id="85c18-198">您想要在站台內的頁面進行類似的變更 — *EditMovie.cshtml*和*DeleteMovie.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="85c18-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="85c18-199">不過，在執行之前，您可以對另一項變更會更有彈性的配置。</span><span class="sxs-lookup"><span data-stu-id="85c18-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="85c18-200">傳遞至版面配置頁的標題資訊</span><span class="sxs-lookup"><span data-stu-id="85c18-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="85c18-201"> *\_Layout.cshtml*您所建立的網頁有`<title>`設為 「 我的電影網站 」 的項目。</span><span class="sxs-lookup"><span data-stu-id="85c18-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="85c18-202">大部分的瀏覽器會顯示此項目的內容，做為文字索引標籤上：</span><span class="sxs-lookup"><span data-stu-id="85c18-202">Most browsers display the content of this element as the text on a tab:</span></span>

![在頁面的&lt;標題&gt;瀏覽器索引標籤中顯示的項目](layouts/_static/image8.png)

<span data-ttu-id="85c18-204">此項目資訊是泛型。</span><span class="sxs-lookup"><span data-stu-id="85c18-204">This title information is generic.</span></span> <span data-ttu-id="85c18-205">假設您想要更精確來目前頁面標題文字。</span><span class="sxs-lookup"><span data-stu-id="85c18-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="85c18-206">（標題文字也會使用搜尋引擎來判斷您的頁面是關於。）您可以將資訊傳遞內容的頁面，例如從*Movies.cshtml*或*AddMovie.cshtml*到版面配置頁面，然後使用該資訊自訂版面配置頁面呈現。</span><span class="sxs-lookup"><span data-stu-id="85c18-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="85c18-207">開啟*Movies.cshtml*頁面上一次。</span><span class="sxs-lookup"><span data-stu-id="85c18-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="85c18-208">在程式碼的頂端加入下列行：</span><span class="sxs-lookup"><span data-stu-id="85c18-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="85c18-209">`Page`物件可供所有使用*.cshtml*頁面，並且是基於此目的，也就是它的版面配置和網頁之間共用資訊。</span><span class="sxs-lookup"><span data-stu-id="85c18-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="85c18-210">開啟*\_Layout.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-210">Open the*\_Layout.cshtml* page.</span></span> <span data-ttu-id="85c18-211">變更`<title>`讓它看起來像是這個標記的項目：</span><span class="sxs-lookup"><span data-stu-id="85c18-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="85c18-212">此程式碼會呈現任何處於`Page.Title`直接在網頁中的該位置的屬性。</span><span class="sxs-lookup"><span data-stu-id="85c18-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="85c18-213">執行*Movies.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="85c18-214">此時間瀏覽器索引標籤會顯示您傳遞的值為`Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="85c18-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![顯示的標題以動態方式建立瀏覽器索引標籤](layouts/_static/image9.png)

<span data-ttu-id="85c18-216">如果您想在瀏覽器中檢視網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="85c18-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="85c18-217">您可以看到`<title>`項目會轉譯為`<title>List Movies</title>`。</span><span class="sxs-lookup"><span data-stu-id="85c18-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="85c18-218">**Page 物件**</span><span class="sxs-lookup"><span data-stu-id="85c18-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="85c18-219">有用功能`Page`是動態的物件-`Title`屬性不是固定或保留的名稱。</span><span class="sxs-lookup"><span data-stu-id="85c18-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="85c18-220">您可以使用*任何*名稱的值`Page`物件。</span><span class="sxs-lookup"><span data-stu-id="85c18-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="85c18-221">例如，您可以輕鬆地傳遞標題使用名為的屬性`Page.CurrentName`或`Page.MyPage`。</span><span class="sxs-lookup"><span data-stu-id="85c18-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="85c18-222">唯一的限制是名稱必須遵照哪些屬性可以具名的一般規則。</span><span class="sxs-lookup"><span data-stu-id="85c18-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="85c18-223">（例如，名稱不能包含空格。）</span><span class="sxs-lookup"><span data-stu-id="85c18-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="85c18-224">您可以傳遞任何數目的值使用`Page`物件。</span><span class="sxs-lookup"><span data-stu-id="85c18-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="85c18-225">如果您想要將電影資訊傳遞至版面配置頁面，您無法將值傳遞使用類似`Page.MovieTitle`和`Page.Genre`和`Page.MovieYear`。</span><span class="sxs-lookup"><span data-stu-id="85c18-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="85c18-226">（或任何其他您用來儲存資訊的名稱）。唯一的需求，這是可能明顯 — 是您必須使用相同的名稱，在 [內容] 頁面和 [配置] 頁面中。</span><span class="sxs-lookup"><span data-stu-id="85c18-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="85c18-227">您使用傳遞的資訊`Page`物件並不限於只是要顯示在 [配置] 頁面上的文字。</span><span class="sxs-lookup"><span data-stu-id="85c18-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="85c18-228">您可以將值傳遞至版面配置 頁面和 配置 頁面中的程式碼再使用值來決定是否要顯示的頁面上，區段什麼*.css*檔案使用，等等。</span><span class="sxs-lookup"><span data-stu-id="85c18-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="85c18-229">您要傳入的值`Page`物件會像任何其他值，您使用程式碼。</span><span class="sxs-lookup"><span data-stu-id="85c18-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="85c18-230">它只是值來自 [內容] 頁面中，而且會傳遞至版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="85c18-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="85c18-231">開啟*AddMovie.cshtml*頁面上，然後將行加入至提供的標題的程式碼頂端*AddMovie.cshtml*頁面：</span><span class="sxs-lookup"><span data-stu-id="85c18-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="85c18-232">執行*AddMovie.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="85c18-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="85c18-233">您會看到那里新的標題：</span><span class="sxs-lookup"><span data-stu-id="85c18-233">You see the new title there:</span></span>

![顯示將加入電影標題，以動態方式建立瀏覽器索引標籤](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="85c18-235">更新其餘的頁面使用的版面配置</span><span class="sxs-lookup"><span data-stu-id="85c18-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="85c18-236">現在您可以在您的站台完成其餘的頁面，使其使用新的版面配置。</span><span class="sxs-lookup"><span data-stu-id="85c18-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="85c18-237">開啟*EditMovie.cshtml*和*DeleteMovie.cshtml*中開啟，並在每個進行相同變更。</span><span class="sxs-lookup"><span data-stu-id="85c18-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="85c18-238">新增連結至版面配置頁面的程式碼行：</span><span class="sxs-lookup"><span data-stu-id="85c18-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="85c18-239">加入線條以設定頁面的標題：</span><span class="sxs-lookup"><span data-stu-id="85c18-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="85c18-240">或：</span><span class="sxs-lookup"><span data-stu-id="85c18-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="85c18-241">移除所有多餘的 HTML 標記 — 基本上，將保留在內部的位元`<body>`元素 （加上程式碼區塊頂端）。</span><span class="sxs-lookup"><span data-stu-id="85c18-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="85c18-242">變更`<h1>`的項目可以`<h2>`項目。</span><span class="sxs-lookup"><span data-stu-id="85c18-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="85c18-243">當您完成這些變更，每個測試，並請確定正確顯示，並標題正確。</span><span class="sxs-lookup"><span data-stu-id="85c18-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="85c18-244">版面配置頁的臨別想法</span><span class="sxs-lookup"><span data-stu-id="85c18-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="85c18-245">在本教學課程建立 *\_Layout.cshtml*頁面上，使用`RenderBody`方法，將合併從另一頁的內容。</span><span class="sxs-lookup"><span data-stu-id="85c18-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="85c18-246">這是在網頁中使用的版面配置的基本模式。</span><span class="sxs-lookup"><span data-stu-id="85c18-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="85c18-247">版面配置頁有我們這裡未涵蓋的其他功能。</span><span class="sxs-lookup"><span data-stu-id="85c18-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="85c18-248">例如，您可以巢狀版面配置頁面 — 一個版面配置頁接著可以參考另一個。</span><span class="sxs-lookup"><span data-stu-id="85c18-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="85c18-249">巢狀的配置很有用，如果您正在使用的站台需要不同的版面配置的各小節。</span><span class="sxs-lookup"><span data-stu-id="85c18-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="85c18-250">您也可以使用其他方法 (例如， `RenderSection`) 來設定名為 [配置] 頁面中的章節。</span><span class="sxs-lookup"><span data-stu-id="85c18-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="85c18-251">版面配置頁的組合和*.css*檔案是強大的功能。</span><span class="sxs-lookup"><span data-stu-id="85c18-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="85c18-252">您會看到下一個教學課程中，在 WebMatrix 中您可以建立根據*範本*，可讓您的網站中預先建立的功能。</span><span class="sxs-lookup"><span data-stu-id="85c18-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="85c18-253">範本可讓充分利用版面配置頁及 CSS 來建立的網站，看起來不錯，而且有類似功能表的功能。</span><span class="sxs-lookup"><span data-stu-id="85c18-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="85c18-254">以下是從範本，顯示使用版面配置頁和 CSS 的功能為基礎的網站 [首頁] 的螢幕擷取畫面：</span><span class="sxs-lookup"><span data-stu-id="85c18-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![顯示標頭、 導覽區域中，內容區域、 選擇性區段和登入連結的 WebMatrix 網站範本所建立的版面配置](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="85c18-256">電影頁面 （若要使用的版面配置頁面更新） 的完整清單</span><span class="sxs-lookup"><span data-stu-id="85c18-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="85c18-257">完成清單 頁面新增電影的頁面 （針對版面配置更新）</span><span class="sxs-lookup"><span data-stu-id="85c18-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="85c18-258">頁面的完整清單刪除影片頁面 （針對版面配置更新）</span><span class="sxs-lookup"><span data-stu-id="85c18-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="85c18-259">頁面的完整清單編輯電影頁面 （針對版面配置更新）</span><span class="sxs-lookup"><span data-stu-id="85c18-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="85c18-260">接下來</span><span class="sxs-lookup"><span data-stu-id="85c18-260">Coming Up Next</span></span>

<span data-ttu-id="85c18-261">在下一個教學課程中，您將學習如何將網站發佈到網際網路，好讓每個人都可以看到它。</span><span class="sxs-lookup"><span data-stu-id="85c18-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85c18-262">其他資源</span><span class="sxs-lookup"><span data-stu-id="85c18-262">Additional Resources</span></span>

- <span data-ttu-id="85c18-263">[建立一致的外觀](https://go.microsoft.com/fwlink/?LinkID=202891)— 使用配置提供更多詳細資料的發行項。</span><span class="sxs-lookup"><span data-stu-id="85c18-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="85c18-264">它也會說明如何將值傳遞至版面配置頁的顯示或隱藏的某些內容。</span><span class="sxs-lookup"><span data-stu-id="85c18-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="85c18-265">[巢狀配置頁面 Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — 的 Mike Brind 部落格如何巢狀化版面配置頁的範例。</span><span class="sxs-lookup"><span data-stu-id="85c18-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="85c18-266">（包括頁面下載）。</span><span class="sxs-lookup"><span data-stu-id="85c18-266">(Includes a download of the pages.)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="85c18-267">[上一頁](deleting-data.md)
[下一頁](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="85c18-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
