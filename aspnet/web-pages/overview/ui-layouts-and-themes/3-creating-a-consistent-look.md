---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: 建立一致的版面配置，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 若要更有效率的方式建立您的網站的網頁，您可以建立可重複使用內容區塊 （例如頁首和頁尾） 為您的網站和您的 c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 91cabc8c026cbdbc89812577bdeaa939bfa828d4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378429"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="39529-103">在 ASP.NET Web Pages (Razor) 網站中建立一致的版面配置</span><span class="sxs-lookup"><span data-stu-id="39529-103">Creating a Consistent Layout in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="39529-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="39529-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="39529-105">這篇文章說明如何使用版面配置頁的 ASP.NET Web Pages (Razor) 網站中建立可重複使用的區塊的內容 （例如頁首和頁尾），以及在網站中建立的所有頁面一致的外觀。</span><span class="sxs-lookup"><span data-stu-id="39529-105">This article explains how you can use layout pages in an ASP.NET Web Pages (Razor) website to create reusable blocks of content (like headers and footers) and to create a consistent look for all the pages in the site.</span></span>
> 
> <span data-ttu-id="39529-106">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="39529-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="39529-107">如何建立可重複使用的區塊的內容，例如頁首和頁尾。</span><span class="sxs-lookup"><span data-stu-id="39529-107">How to create reusable blocks of content like headers and footers.</span></span>
> - <span data-ttu-id="39529-108">如何在您的網站使用的版面配置建立一致的外觀的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-108">How to create a consistent look for all the pages in your site using a layout.</span></span>
> - <span data-ttu-id="39529-109">如何在執行階段的資料傳遞至版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="39529-109">How to pass data at run time to a layout page.</span></span>
> 
> <span data-ttu-id="39529-110">以下是所引進的發行項 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="39529-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="39529-111">內容的區塊，也就是包含在多個頁面中插入 HTML 格式之內容的檔案。</span><span class="sxs-lookup"><span data-stu-id="39529-111">Content blocks, which are files that contain HTML-formatted content to be inserted in multiple pages.</span></span>
> - <span data-ttu-id="39529-112">版面配置頁面： 包含 HTML 格式的內容可在網站上的頁面所共用的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-112">Layout pages, which are pages that contain HTML-formatted content that can be shared by pages on the website.</span></span>
> - <span data-ttu-id="39529-113">`RenderPage`， `RenderBody`，和`RenderSection`方法，告知 ASP.NET 將頁面項目插入的位置。</span><span class="sxs-lookup"><span data-stu-id="39529-113">The `RenderPage`, `RenderBody`, and `RenderSection` methods, which tell ASP.NET where to insert page elements.</span></span>
> - <span data-ttu-id="39529-114">`PageData`字典，可讓您共用內容區塊和版面配置頁之間的資料。</span><span class="sxs-lookup"><span data-stu-id="39529-114">The `PageData` dictionary that lets you share data between content blocks and layout pages.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="39529-115">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="39529-115">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="39529-116">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="39529-116">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="39529-117">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="39529-117">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-layout-pages"></a><span data-ttu-id="39529-118">關於版面配置頁</span><span class="sxs-lookup"><span data-stu-id="39529-118">About Layout Pages</span></span>

<span data-ttu-id="39529-119">許多網站都有在每個頁面上，例如標頭和頁尾中或告知使用者已登入的方塊會顯示的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-119">Many websites have content that's displayed on every page, like a header and footer, or a box that tells users that they're logged in.</span></span> <span data-ttu-id="39529-120">ASP.NET 可讓您建立個別的檔案與內容的區塊可以包含文字、 標記和程式碼，就像標準網頁一樣。</span><span class="sxs-lookup"><span data-stu-id="39529-120">ASP.NET lets you create a separate file with a content block that can contain text, markup, and code, just like a regular web page.</span></span> <span data-ttu-id="39529-121">然後，您就可以在您想要的資訊，才會出現在網站上的其他頁面中插入該內容區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-121">You can then insert the content block in other pages on the site where you want the information to appear.</span></span> <span data-ttu-id="39529-122">這樣一來，您不需要複製並貼到每個頁面的 相同的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-122">That way you don't have to copy and paste the same content into every page.</span></span> <span data-ttu-id="39529-123">建立這類的一般內容也可讓您更輕鬆地更新您的網站。</span><span class="sxs-lookup"><span data-stu-id="39529-123">Creating common content like this also makes it easier to update your site.</span></span> <span data-ttu-id="39529-124">如果您要變更內容，您可以只更新單一檔案，而且所做的變更會反映然後到處都已插入內容。</span><span class="sxs-lookup"><span data-stu-id="39529-124">If you need to change the content, you can just update a single file, and the changes are then reflected everywhere the content has been inserted.</span></span>

<span data-ttu-id="39529-125">下圖顯示內容封鎖工作的方式。</span><span class="sxs-lookup"><span data-stu-id="39529-125">The following diagram shows how content blocks work.</span></span> <span data-ttu-id="39529-126">當瀏覽器要求網頁從 web 伺服器時，ASP.NET 會在點插入內容區塊所在`RenderPage`方法會呼叫在主頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-126">When a browser requests a page from the web server, ASP.NET inserts the content blocks at the point where the `RenderPage` method is called in the main page.</span></span> <span data-ttu-id="39529-127">完成 （合併） 頁面，然後會傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="39529-127">The finished (merged) page is then sent to the browser.</span></span>

![概念的圖表，顯示 RenderPage 方法如何參考的頁插入目前的頁面。](3-creating-a-consistent-look/_static/image1.jpg)

<span data-ttu-id="39529-129">在此程序中，您將建立會參考兩個內容區塊 （標頭和頁尾） 位於不同的檔案中的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-129">In this procedure, you'll create a page that references two content blocks (a header and a footer) that are located in separate files.</span></span> <span data-ttu-id="39529-130">在您的網站中的任何網頁中，您可以使用這些相同的內容區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-130">You can use these same content blocks in any page in your site.</span></span> <span data-ttu-id="39529-131">當您完成時，您會收到如下的頁面：</span><span class="sxs-lookup"><span data-stu-id="39529-131">When you're done, you'll get a page like this:</span></span>

![在執行一個網頁，內含 RenderPage 方法的呼叫而產生的瀏覽器中顯示頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image2.jpg)

1. <span data-ttu-id="39529-133">在您的網站根資料夾中，建立名為*Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="39529-133">In the root folder of your website, create a file named *Index.cshtml*.</span></span>
2. <span data-ttu-id="39529-134">以下列內容取代現有的標記：</span><span class="sxs-lookup"><span data-stu-id="39529-134">Replace the existing markup with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. <span data-ttu-id="39529-135">在根資料夾中，建立名為*共用*。</span><span class="sxs-lookup"><span data-stu-id="39529-135">In the root folder, create a folder named *Shared*.</span></span>

    > [!NOTE]
    > <span data-ttu-id="39529-136">它是常見的作法是儲存在名為的資料夾中的 web 網頁之間共用的檔案*共用*。</span><span class="sxs-lookup"><span data-stu-id="39529-136">It's common practice to store files that are shared among web pages in a folder named *Shared*.</span></span>
4. <span data-ttu-id="39529-137">在  *Shared*資料夾中，建立名為 *\_Header.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="39529-137">In the *Shared* folder, create a file named *\_Header.cshtml*.</span></span>
5. <span data-ttu-id="39529-138">以下列內容取代任何現有的內容：</span><span class="sxs-lookup"><span data-stu-id="39529-138">Replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    <span data-ttu-id="39529-139">請注意，檔案名稱是 *\_Header.cshtml*，以底線 (\_) 做為前置詞。</span><span class="sxs-lookup"><span data-stu-id="39529-139">Notice that the file name is *\_Header.cshtml*, with an underscore (\_) as a prefix.</span></span> <span data-ttu-id="39529-140">ASP.NET 不會將頁面傳送至瀏覽器，如果其名稱開頭為底線。</span><span class="sxs-lookup"><span data-stu-id="39529-140">ASP.NET won't send a page to the browser if its name starts with an underscore.</span></span> <span data-ttu-id="39529-141">這可防止人員要求 （不小心或失敗） 這些網頁直接。</span><span class="sxs-lookup"><span data-stu-id="39529-141">This prevents people from requesting (inadvertently or otherwise) these pages directly.</span></span> <span data-ttu-id="39529-142">它是個不錯的主意，使用名稱頁具有內容區塊，前面加上的底線，因為您真的不希望使用者能夠要求這些頁面&#8212;絕對要插入至其他頁面存在。</span><span class="sxs-lookup"><span data-stu-id="39529-142">It's a good idea to use an underscore to name pages that have content blocks in them, because you don't really want users to be able to request these pages &#8212; they exist strictly to be inserted into other pages.</span></span>
6. <span data-ttu-id="39529-143">在 *共用*資料夾中，建立名為 *\_Footer.cshtml*並取代為下列內容：</span><span class="sxs-lookup"><span data-stu-id="39529-143">In the *Shared* folder, create a file named *\_Footer.cshtml* and replace the content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. <span data-ttu-id="39529-144">在  *Index.cshtml*頁面上，新增兩個呼叫`RenderPage`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39529-144">In the *Index.cshtml* page, add two calls to the `RenderPage` method, as shown here:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    <span data-ttu-id="39529-145">這會顯示如何插入 web 網頁內容的區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-145">This shows how to insert a content block into a web page.</span></span> <span data-ttu-id="39529-146">您呼叫`RenderPage`方法並將它傳遞您想要在該點插入其內容的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="39529-146">You call the `RenderPage` method and pass it the name of the file whose contents you want to insert at that point.</span></span> <span data-ttu-id="39529-147">在這裡，您要插入的內容 *\_Header.cshtml*並 *\_Footer.cshtml*檔案至*Index.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="39529-147">Here, you're inserting the contents of the *\_Header.cshtml* and *\_Footer.cshtml* files into the *Index.cshtml* file.</span></span>
8. <span data-ttu-id="39529-148">執行*Index.cshtml*瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-148">Run the *Index.cshtml* page in a browser.</span></span> <span data-ttu-id="39529-149">(在 WebMatrix 中，在**檔案**工作區中，以滑鼠右鍵按一下檔案，然後選取**在瀏覽器中啟動**。)</span><span class="sxs-lookup"><span data-stu-id="39529-149">(In WebMatrix, in the **Files** workspace, right-click the file and then select **Launch in browser**.)</span></span>
9. <span data-ttu-id="39529-150">在瀏覽器中檢視網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="39529-150">In the browser, view the page source.</span></span> <span data-ttu-id="39529-151">(例如，在 Internet Explorer 中，以滑鼠右鍵按一下  頁面上，然後按一下**檢視原始檔**。)</span><span class="sxs-lookup"><span data-stu-id="39529-151">(For example, in Internet Explorer, right-click the page and then click **View Source**.)</span></span>

    <span data-ttu-id="39529-152">這可讓您查看傳送至瀏覽器中，其結合了與內容區塊的索引頁面標記的 web 網頁標記。</span><span class="sxs-lookup"><span data-stu-id="39529-152">This lets you see the web page markup that's sent to the browser, which combines the index page markup with the content blocks.</span></span> <span data-ttu-id="39529-153">下列範例顯示呈現頁面原始碼*Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="39529-153">The following example shows the page source that's rendered for *Index.cshtml*.</span></span> <span data-ttu-id="39529-154">若要呼叫`RenderPage`插入至*Index.cshtml*已取代為實際的頁首和頁尾的檔案內容。</span><span class="sxs-lookup"><span data-stu-id="39529-154">The calls to `RenderPage` that you inserted into *Index.cshtml* have been replaced with the actual contents of the header and footer files.</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a><span data-ttu-id="39529-155">建立一致的外觀，使用版面配置頁</span><span class="sxs-lookup"><span data-stu-id="39529-155">Creating a Consistent Look Using Layout Pages</span></span>

<span data-ttu-id="39529-156">到目前為止，您已了解，就可以輕鬆地包含多個頁面上相同的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-156">So far you've seen that it's easy to include the same content on multiple pages.</span></span> <span data-ttu-id="39529-157">建立一致的外觀，站台的更具結構化的方式是使用版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="39529-157">A more structured approach to creating a consistent look for a site is to use layout pages.</span></span> <span data-ttu-id="39529-158">版面配置頁定義網頁的結構，但不包含任何實際的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-158">A layout page defines the structure of a web page, but doesn't contain any actual content.</span></span> <span data-ttu-id="39529-159">建立版面配置頁之後，您可以建立包含內容，然後將它們連結至版面配置頁的網頁。</span><span class="sxs-lookup"><span data-stu-id="39529-159">After you've created a layout page, you can create web pages that contain the content and then link them to the layout page.</span></span> <span data-ttu-id="39529-160">當顯示這些頁面時，它們會格式化根據版面配置頁中。</span><span class="sxs-lookup"><span data-stu-id="39529-160">When these pages are displayed, they'll be formatted according to the layout page.</span></span> <span data-ttu-id="39529-161">（這個意義來說，版面配置頁做為一種在其他頁面中定義的內容的範本。）</span><span class="sxs-lookup"><span data-stu-id="39529-161">(In this sense, a layout page acts as a kind of template for content that's defined in other pages.)</span></span>

<span data-ttu-id="39529-162">[配置] 頁面就像任何 HTML 頁面上，一樣，只不過它包含呼叫`RenderBody`方法。</span><span class="sxs-lookup"><span data-stu-id="39529-162">The layout page is just like any HTML page, except that it contains a call to the `RenderBody` method.</span></span> <span data-ttu-id="39529-163">位置`RenderBody`版面配置頁的方法會判斷會包含 [內容] 頁面中的資訊。</span><span class="sxs-lookup"><span data-stu-id="39529-163">The position of the `RenderBody` method in the layout page determines where the information from the content page will be included.</span></span>

<span data-ttu-id="39529-164">下圖顯示如何內容頁面和版面配置頁會結合以產生已完成的 web 網頁的執行階段。</span><span class="sxs-lookup"><span data-stu-id="39529-164">The following diagram shows how content pages and layout pages are combined at run time to produce the finished web page.</span></span> <span data-ttu-id="39529-165">瀏覽器要求內容的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-165">The browser requests a content page.</span></span> <span data-ttu-id="39529-166">[內容] 頁面中，指定要用於頁面結構的 [配置] 頁面，具有程式碼。</span><span class="sxs-lookup"><span data-stu-id="39529-166">The content page has code in it that specifies the layout page to use for the page's structure.</span></span> <span data-ttu-id="39529-167">在 [配置] 頁面中，內容會插入點位置`RenderBody`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="39529-167">In the layout page, the content is inserted at the point where the `RenderBody` method is called.</span></span> <span data-ttu-id="39529-168">內容區塊也可插入版面配置頁藉由呼叫`RenderPage`方法，就像在前一節中的方式。</span><span class="sxs-lookup"><span data-stu-id="39529-168">Content blocks can also be inserted into the layout page by calling the `RenderPage` method, the way you did in the previous section.</span></span> <span data-ttu-id="39529-169">網頁完成時，它會傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="39529-169">When the web page is complete, it's sent to the browser.</span></span>

![在執行一個網頁，內含 RenderBody 方法的呼叫而產生的瀏覽器中顯示頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image3.jpg)

<span data-ttu-id="39529-171">下列程序示範如何建立版面配置頁面和連結到它的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-171">The following procedure shows how to create a layout page and link content pages to it.</span></span>

1. <span data-ttu-id="39529-172">在  *Shared*資料夾，您的網站，建立名為 *\_Layout1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="39529-172">In the *Shared* folder of your website, create a file named *\_Layout1.cshtml*.</span></span>
2. <span data-ttu-id="39529-173">以下列內容取代任何現有的內容：</span><span class="sxs-lookup"><span data-stu-id="39529-173">Replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    <span data-ttu-id="39529-174">您使用`RenderPage`插入內容區塊的版面配置頁中的方法。</span><span class="sxs-lookup"><span data-stu-id="39529-174">You use the `RenderPage` method in a layout page to insert content blocks.</span></span> <span data-ttu-id="39529-175">版面配置頁可以包含只有一個呼叫`RenderBody`方法。</span><span class="sxs-lookup"><span data-stu-id="39529-175">A layout page can contain only one call to the `RenderBody` method.</span></span>
3. <span data-ttu-id="39529-176">在  *Shared*資料夾中，建立名為 *\_Header2.cshtml*和任何現有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="39529-176">In the *Shared* folder, create a file named *\_Header2.cshtml* and replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. <span data-ttu-id="39529-177">在根資料夾中，建立新的資料夾並將它命名*樣式*。</span><span class="sxs-lookup"><span data-stu-id="39529-177">In the root folder, create a new folder and name it *Styles*.</span></span>
5. <span data-ttu-id="39529-178">在 *樣式*資料夾中，建立名為*Site.css*並加入下列樣式定義：</span><span class="sxs-lookup"><span data-stu-id="39529-178">In the *Styles* folder, create a file named *Site.css* and add the following style definitions:</span></span>

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    <span data-ttu-id="39529-179">這些樣式定義在此僅為顯示如何使用樣式表與版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="39529-179">These style definitions are here only to show how style sheets can be used with layout pages.</span></span> <span data-ttu-id="39529-180">如果您想，您可以定義這些項目的樣式。</span><span class="sxs-lookup"><span data-stu-id="39529-180">If you want, you can define your own styles for these elements.</span></span>
6. <span data-ttu-id="39529-181">在根資料夾中，建立名為*Content1.cshtml*和任何現有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="39529-181">In the root folder, create a file named *Content1.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    <span data-ttu-id="39529-182">這是將使用版面配置頁的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-182">This is a page that will use a layout page.</span></span> <span data-ttu-id="39529-183">在頁面頂端的程式碼區塊會指出要用來格式化此內容的 [配置] 頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-183">The code block at the top of the page indicates which layout page to use to format this content.</span></span>
7. <span data-ttu-id="39529-184">執行*Content1.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="39529-184">Run *Content1.cshtml* in a browser.</span></span> <span data-ttu-id="39529-185">呈現的網頁使用的格式和樣式表中定義 *\_Layout1.cshtml*中定義的文字 （內容） 及*Content1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="39529-185">The rendered page uses the format and style sheet defined in *\_Layout1.cshtml* and the text (content) defined in *Content1.cshtml*.</span></span>

    ![[影像]](3-creating-a-consistent-look/_static/image4.jpg)

    <span data-ttu-id="39529-187">您可以重複步驟 6 以建立其他的內容頁面，然後就可以共用相同的 [配置] 頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-187">You can repeat step 6 to create additional content pages that can then share the same layout page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="39529-188">您可以設定您的網站，好讓您自動可以使用相同的 [配置] 頁面的資料夾中的所有內容頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-188">You can set up your site so that you can automatically use the same layout page for all the content pages in a folder.</span></span> <span data-ttu-id="39529-189">如需詳細資訊，請參閱 <<c0> [ 自訂全網站行為適用於 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202906)。</span><span class="sxs-lookup"><span data-stu-id="39529-189">For details, see [Customizing Site-Wide Behavior for ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).</span></span>

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a><span data-ttu-id="39529-190">有多個內容區段的設計版面配置頁</span><span class="sxs-lookup"><span data-stu-id="39529-190">Designing Layout Pages That Have Multiple Content Sections</span></span>

<span data-ttu-id="39529-191">內容頁面可以有多個區段，這很有用，如果您想要使用已使用可取代內容的多個區域的版面配置。</span><span class="sxs-lookup"><span data-stu-id="39529-191">A content page can have multiple sections, which is useful if you want to use layouts that have multiple areas with replaceable content.</span></span> <span data-ttu-id="39529-192">在 [內容] 頁面中，您提供每個區段的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="39529-192">In the content page, you give each section a unique name.</span></span> <span data-ttu-id="39529-193">(預設區段保留未命名。)在 [配置] 頁面中，您會新增`RenderBody`方法，以指定 [未命名的 （預設值）] 區段應該出現的位置。</span><span class="sxs-lookup"><span data-stu-id="39529-193">(The default section is left unnamed.) In the layout page, you add a `RenderBody` method to specify where the unnamed (default) section should appear.</span></span> <span data-ttu-id="39529-194">然後，您新增個別`RenderSection`方法，以便個別呈現具名的區段。</span><span class="sxs-lookup"><span data-stu-id="39529-194">You then add separate `RenderSection` methods in order to render named sections individually.</span></span>

<span data-ttu-id="39529-195">下圖顯示 ASP.NET 要如何處理分成多個區段的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-195">The following diagram shows how ASP.NET handles content that's divided into multiple sections.</span></span> <span data-ttu-id="39529-196">每個具名的區段會包含在 [內容] 頁面中的區段區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-196">Each named section is contained in a section block in the content page.</span></span> <span data-ttu-id="39529-197">(它們名為`Header`和`List`在範例中。)架構會插入點的 [配置] 頁面中的 [內容] 區段，`RenderSection`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="39529-197">(They're named `Header` and `List` in the example.) The framework inserts content section into the layout page at the point where the `RenderSection` method is called.</span></span> <span data-ttu-id="39529-198">未命名的 （預設） 一節會插入點位置`RenderBody`呼叫方法，如您稍早所見。</span><span class="sxs-lookup"><span data-stu-id="39529-198">The unnamed (default) section is inserted at the point where the `RenderBody` method is called, as you saw earlier.</span></span>

![概念的圖表，顯示 RenderSection 方法如何參考區段插入目前的頁面。](3-creating-a-consistent-look/_static/image5.jpg)

<span data-ttu-id="39529-200">此程序示範如何建立具有多個內容區段的內容頁面，以及如何支援多重內容區段的版面配置頁面進行轉譯。</span><span class="sxs-lookup"><span data-stu-id="39529-200">This procedure shows how to create a content page that has multiple content sections and how to render it using a layout page that supports multiple content sections.</span></span>

1. <span data-ttu-id="39529-201">在  *Shared*資料夾中，建立名為 *\_Layout2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="39529-201">In the *Shared* folder, create a file named *\_Layout2.cshtml*.</span></span>
2. <span data-ttu-id="39529-202">以下列內容取代任何現有的內容：</span><span class="sxs-lookup"><span data-stu-id="39529-202">Replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    <span data-ttu-id="39529-203">您使用`RenderSection`方法以轉譯標頭和清單的區段。</span><span class="sxs-lookup"><span data-stu-id="39529-203">You use the `RenderSection` method to render both the header and list sections.</span></span>
3. <span data-ttu-id="39529-204">在根資料夾中，建立名為*Content2.cshtml*和任何現有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="39529-204">In the root folder, create a file named *Content2.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    <span data-ttu-id="39529-205">此內容的頁面會包含在頁面頂端的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-205">This content page contains a code block at the top of the page.</span></span> <span data-ttu-id="39529-206">每個具名的區段包含區段區塊中。</span><span class="sxs-lookup"><span data-stu-id="39529-206">Each named section is contained in a section block.</span></span> <span data-ttu-id="39529-207">頁面的其餘部分包含的預設 （未命名） 的內容一節。</span><span class="sxs-lookup"><span data-stu-id="39529-207">The rest of the page contains the default (unnamed) content section.</span></span>
4. <span data-ttu-id="39529-208">執行*Content2.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="39529-208">Run *Content2.cshtml* in a browser.</span></span>

    ![在執行一個網頁，內含 RenderSection 方法的呼叫而產生的瀏覽器中顯示頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a><span data-ttu-id="39529-210">進行選擇性的內容區段</span><span class="sxs-lookup"><span data-stu-id="39529-210">Making Content Sections Optional</span></span>

<span data-ttu-id="39529-211">一般來說，您建立內容頁面中的區段，就一定要相符 [配置] 頁面中所定義的區段。</span><span class="sxs-lookup"><span data-stu-id="39529-211">Normally, the sections that you create in a content page have to match sections that are defined in the layout page.</span></span> <span data-ttu-id="39529-212">如果發生下列任一項動作，可能會收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="39529-212">You can get errors if any of the following occur:</span></span>

- <span data-ttu-id="39529-213">[內容] 頁面會包含在 [配置] 頁面中沒有對應的區段的區段。</span><span class="sxs-lookup"><span data-stu-id="39529-213">The content page contains a section that has no corresponding section in the layout page.</span></span>
- <span data-ttu-id="39529-214">[配置] 頁面包含的區段，它沒有任何內容。</span><span class="sxs-lookup"><span data-stu-id="39529-214">The layout page contains a section for which there's no content.</span></span>
- <span data-ttu-id="39529-215">[配置] 頁面包含方法呼叫，其會試圖來呈現該相同的區段，一次以上。</span><span class="sxs-lookup"><span data-stu-id="39529-215">The layout page includes method calls that try to render the same section more than once.</span></span>

<span data-ttu-id="39529-216">不過，您可以宣告為選擇性，在 [配置] 頁面中的區段來覆寫此行為的具名區段。</span><span class="sxs-lookup"><span data-stu-id="39529-216">However, you can override this behavior for a named section by declaring the section to be optional in the layout page.</span></span> <span data-ttu-id="39529-217">這可讓您定義多個內容頁面，可以共用版面配置頁，但可能會或可能沒有特定區段的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-217">This lets you define multiple content pages that can share a layout page but that might or might not have content for a specific section.</span></span>

1. <span data-ttu-id="39529-218">開啟*Content2.cshtml*及移除下列區段：</span><span class="sxs-lookup"><span data-stu-id="39529-218">Open *Content2.cshtml* and remove the following section:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. <span data-ttu-id="39529-219">儲存頁面，然後執行它在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="39529-219">Save the page and then run it in a browser.</span></span> <span data-ttu-id="39529-220">顯示一則錯誤訊息，因為 [內容] 頁面並不提供版面配置頁面，也就是標頭區段中定義的區段的內容。</span><span class="sxs-lookup"><span data-stu-id="39529-220">An error message is displayed, because the content page doesn't provide content for a section defined in the layout page, namely the header section.</span></span>

    ![螢幕擷取畫面顯示如果您執行頁面，就會發生此錯誤的呼叫 RenderSection 方法，但未提供對應的章節。](3-creating-a-consistent-look/_static/image7.jpg)
3. <span data-ttu-id="39529-222">在  *Shared*資料夾中，開啟 *\_Layout2.cshtml*頁面上，然後取代這一行：</span><span class="sxs-lookup"><span data-stu-id="39529-222">In the *Shared* folder, open the *\_Layout2.cshtml* page and replace this line:</span></span>

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    <span data-ttu-id="39529-223">以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="39529-223">with the following code:</span></span>

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    <span data-ttu-id="39529-224">或者，您無法前的一行程式碼取代下列程式碼區塊，這會產生相同的結果：</span><span class="sxs-lookup"><span data-stu-id="39529-224">As an alternative, you could replace the previous line of code with the following code block, which produces the same results:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. <span data-ttu-id="39529-225">執行*Content2.cshtml*頁面在瀏覽器中一次。</span><span class="sxs-lookup"><span data-stu-id="39529-225">Run the *Content2.cshtml* page in a browser again.</span></span> <span data-ttu-id="39529-226">（如果您仍有瀏覽器中開啟此頁面，您可以只重新整理它。）這次頁面會不顯示任何錯誤，即使它有沒有標頭。</span><span class="sxs-lookup"><span data-stu-id="39529-226">(If you still have this page open in the browser, you can just refresh it.) This time the page is displayed with no error, even though it has no header.</span></span>

## <a name="passing-data-to-layout-pages"></a><span data-ttu-id="39529-227">將資料傳遞至版面配置頁</span><span class="sxs-lookup"><span data-stu-id="39529-227">Passing Data to Layout Pages</span></span>

<span data-ttu-id="39529-228">您可能必須在 [內容] 頁面，您需要在版面配置頁中參考中定義的資料。</span><span class="sxs-lookup"><span data-stu-id="39529-228">You might have data defined in the content page that you need to refer to in a layout page.</span></span> <span data-ttu-id="39529-229">如果是這樣，您需要將 [內容] 頁面中的資料傳遞至版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="39529-229">If so, you need to pass the data from the content page to the layout page.</span></span> <span data-ttu-id="39529-230">例如，您可能想要顯示使用者的登入狀態，或您可能想要顯示或隱藏內容區域根據使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="39529-230">For example, you might want to display the login status of a user, or you might want to show or hide content areas based on user input.</span></span>

<span data-ttu-id="39529-231">若要將從內容頁面的資料傳遞至版面配置頁面中，您可以將值插入`PageData`內容頁面的屬性。</span><span class="sxs-lookup"><span data-stu-id="39529-231">To pass data from a content page to a layout page, you can put values into the `PageData` property of the content page.</span></span> <span data-ttu-id="39529-232">`PageData`屬性會保存您想要在頁面之間傳遞資料的名稱/值組的集合。</span><span class="sxs-lookup"><span data-stu-id="39529-232">The `PageData` property is a collection of name/value pairs that hold the data that you want to pass between pages.</span></span> <span data-ttu-id="39529-233">在 [配置] 頁面中，您可以接著讀取的值`PageData`屬性。</span><span class="sxs-lookup"><span data-stu-id="39529-233">In the layout page, you can then read values out of the `PageData` property.</span></span>

<span data-ttu-id="39529-234">以下是另一個圖表。</span><span class="sxs-lookup"><span data-stu-id="39529-234">Here's another diagram.</span></span> <span data-ttu-id="39529-235">這一個會顯示如何使用 ASP.NET`PageData`屬性來從內容頁面的值傳遞至版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="39529-235">This one shows how ASP.NET can use the `PageData` property to pass values from a content page to the layout page.</span></span> <span data-ttu-id="39529-236">當 ASP.NET 開始建置網頁時，它會建立`PageData`集合。</span><span class="sxs-lookup"><span data-stu-id="39529-236">When ASP.NET begins building the web page, it creates the `PageData` collection.</span></span> <span data-ttu-id="39529-237">在 [內容] 頁面中，您可以撰寫程式碼，以將資料放在`PageData`集合。</span><span class="sxs-lookup"><span data-stu-id="39529-237">In the content page, you write code to put data in the `PageData` collection.</span></span> <span data-ttu-id="39529-238">中的值`PageData`集合也可以存取 [內容] 頁面中的其他章節，或其他內容的區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-238">Values in the `PageData` collection can also be accessed by other sections in the content page or by additional content blocks.</span></span>

![說明如何內容頁面可以填入 PageData 字典，並將該資訊傳遞至版面配置頁的概念圖。](3-creating-a-consistent-look/_static/image8.jpg)

<span data-ttu-id="39529-240">下列程序示範如何將資料從內容頁面傳遞至版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="39529-240">The following procedure shows how to pass data from a content page to a layout page.</span></span> <span data-ttu-id="39529-241">當執行網頁時，它會顯示一個按鈕，可讓使用者隱藏或顯示版面配置頁面中定義的清單。</span><span class="sxs-lookup"><span data-stu-id="39529-241">When the page runs, it displays a button that lets the user hide or show a list that's defined in the layout page.</span></span> <span data-ttu-id="39529-242">當使用者按一下按鈕時，會設定為 true/false （布林值） 值中`PageData`屬性。</span><span class="sxs-lookup"><span data-stu-id="39529-242">When users click the button, it sets a true/false (Boolean) value in the `PageData` property.</span></span> <span data-ttu-id="39529-243">版面配置頁讀取該值，，和如果運算式為 false，會隱藏清單。</span><span class="sxs-lookup"><span data-stu-id="39529-243">The layout page reads that value, and if it's false, hides the list.</span></span> <span data-ttu-id="39529-244">值也會在 內容 頁面來判斷是否要顯示**隱藏清單** 按鈕或**顯示清單** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="39529-244">The value is also used in the content page to determine whether to display the **Hide List** button or the **Show List** button.</span></span>

![[影像]](3-creating-a-consistent-look/_static/image9.jpg)

1. <span data-ttu-id="39529-246">在根資料夾中，建立名為*Content3.cshtml*和任何現有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="39529-246">In the root folder, create a file named *Content3.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    <span data-ttu-id="39529-247">程式碼會儲存兩組中的資料`PageData`屬性&#8212;網頁和 true 或 false，指定是否要顯示清單的標題。</span><span class="sxs-lookup"><span data-stu-id="39529-247">The code stores two pieces of data in the `PageData` property &#8212; the title of the web page and true or false to specify whether to display a list.</span></span>

    <span data-ttu-id="39529-248">請注意，ASP.NET 可讓您將 HTML 標記放在有條件地使用程式碼區塊的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-248">Notice that ASP.NET lets you put HTML markup into the page conditionally using a code block.</span></span> <span data-ttu-id="39529-249">例如，`if/else`區塊中頁面的主體會決定哪一個表單，以顯示取決於是否`PageData["ShowList"]`設為 true。</span><span class="sxs-lookup"><span data-stu-id="39529-249">For example, the `if/else` block in the body of the page determines which form to display depending on whether `PageData["ShowList"]` is set to true.</span></span>
2. <span data-ttu-id="39529-250">在  *Shared*資料夾中，建立名為 *\_Layout3.cshtml*和任何現有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="39529-250">In the *Shared* folder, create a file named *\_Layout3.cshtml* and replace any existing content with the following:</span></span>

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    <span data-ttu-id="39529-251">[配置] 頁面包含的運算式`<title>`取得的標題值的項目`PageData`屬性。</span><span class="sxs-lookup"><span data-stu-id="39529-251">The layout page includes an expression in the `<title>` element that gets the title value from the `PageData` property.</span></span> <span data-ttu-id="39529-252">它也會使用`ShowList`的值`PageData`屬性來判斷是否要顯示的清單內容的區塊。</span><span class="sxs-lookup"><span data-stu-id="39529-252">It also uses the `ShowList` value of the `PageData` property to determine whether to display the list content block.</span></span>
3. <span data-ttu-id="39529-253">在  *Shared*資料夾中，建立名為 *\_List.cshtml*和任何現有的內容取代為下列：</span><span class="sxs-lookup"><span data-stu-id="39529-253">In the *Shared* folder, create a file named *\_List.cshtml* and replace any existing content with the following:</span></span>

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. <span data-ttu-id="39529-254">執行*Content3.cshtml*瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="39529-254">Run the *Content3.cshtml* page in a browser.</span></span> <span data-ttu-id="39529-255">頁面會顯示頁面的左側顯示的清單和**隱藏清單**底部的按鈕。</span><span class="sxs-lookup"><span data-stu-id="39529-255">The page is displayed with the list visible on the left side of the page and a **Hide List** button at the bottom.</span></span>

    ![顯示包含清單和一個按鈕，叫做 「 隱藏清單 」 頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image10.jpg)
5. <span data-ttu-id="39529-257">按一下 **隱藏清單**。</span><span class="sxs-lookup"><span data-stu-id="39529-257">Click **Hide List**.</span></span> <span data-ttu-id="39529-258">清單消失，按鈕會變成**顯示清單**。</span><span class="sxs-lookup"><span data-stu-id="39529-258">The list disappears and the button changes to **Show List**.</span></span>

    ![顯示不包含清單和一個按鈕，叫做 「 顯示清單 」 頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image11.jpg)
6. <span data-ttu-id="39529-260">按一下 **顯示清單**按鈕和清單就會再次顯示。</span><span class="sxs-lookup"><span data-stu-id="39529-260">Click the **Show List** button, and the list is displayed again.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39529-261">其他資源</span><span class="sxs-lookup"><span data-stu-id="39529-261">Additional Resources</span></span>


[<span data-ttu-id="39529-262">ASP.NET Web 網頁的自訂全網站行為</span><span class="sxs-lookup"><span data-stu-id="39529-262">Customizing Site-Wide Behavior for ASP.NET Web Pages</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
