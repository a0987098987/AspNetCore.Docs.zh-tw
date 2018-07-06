---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: 建立和使用協助程式中 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。 協助程式是一種可重複使用的元件，包括程式碼和標記，以效能...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: c2edc10e01f4d2358dd0b0bdf450348f01eb2bfa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811895"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="cfdd4-104">建立和使用 ASP.NET Web Pages (Razor) 網站中的協助程式</span><span class="sxs-lookup"><span data-stu-id="cfdd4-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="cfdd4-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cfdd4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cfdd4-106">本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="cfdd4-107">A*協助程式*是一種可重複使用的元件，包括程式碼和標記，以執行可能冗長或複雜的工作。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="cfdd4-108">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="cfdd4-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="cfdd4-109">如何建立及使用簡單的協助程式。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="cfdd4-110">以下是所引進的發行項 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="cfdd4-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="cfdd4-111">`@helper`語法。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cfdd4-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="cfdd4-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cfdd4-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="cfdd4-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="cfdd4-114">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="cfdd4-115">協助程式的概觀</span><span class="sxs-lookup"><span data-stu-id="cfdd4-115">Overview of Helpers</span></span>

<span data-ttu-id="cfdd4-116">如果您需要在您的網站不同頁面上執行相同的工作，您可以使用協助程式。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="cfdd4-117">ASP.NET Web 網頁包含數個協助程式，而且有更多，您可以下載並安裝。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="cfdd4-118">(一份內建的協助程式 ASP.NET Web Pages 中列入[ASP.NET API 快速參考](https://go.microsoft.com/fwlink/?LinkId=202907)。)如果沒有現有的協助程式符合您的需求，您可以建立您自己的協助程式。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="cfdd4-119">協助程式可讓您使用的常見程式碼區塊，跨多個頁面。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="cfdd4-120">假設您的頁面中您通常要建立附註項目，除了一般的段落設定。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="cfdd4-121">附註可能會建立為`<div>`項目，有樣式設定為具有框線的方塊。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="cfdd4-122">而不是每次您想要的方式顯示便箋，請將這個相同的標記新增至頁面中，您就可以封裝標記做為協助程式。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="cfdd4-123">然後，您就可以插入一行程式碼與附註您需要的任何位置。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="cfdd4-124">使用像這樣的協助程式在每個頁面中會讓程式碼，更簡單且容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="cfdd4-125">它也可讓您更輕鬆地維護您的網站，因為如果您需要變更便箋的外觀，您可以變更一個位置中的標記。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="cfdd4-126">建立 Helper</span><span class="sxs-lookup"><span data-stu-id="cfdd4-126">Creating a Helper</span></span>

<span data-ttu-id="cfdd4-127">此程序會示範如何建立協助程式會建立附註，如上所述。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="cfdd4-128">這是一個簡單的範例，但自訂協助程式可以包含任何標記和您所需的 ASP.NET 程式碼。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="cfdd4-129">在網站的根資料夾中，建立名為*應用程式\_程式碼*。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="cfdd4-130">這是您可以在其中放置例如協助程式元件的程式碼在 ASP.NET 中保留的資料夾名稱。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="cfdd4-131">在 *應用程式\_程式碼*資料夾中建立的新 *.cshtml*檔案並將它命名*MyHelpers.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="cfdd4-132">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="cfdd4-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="cfdd4-133">程式碼會使用`@helper`語法來宣告名為新的協助程式`MakeNote`。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="cfdd4-134">此特定協助程式可讓您將名為參數傳遞`content`可包含文字和標記的組合。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="cfdd4-135">協助程式會將字串插入便箋內文使用`@content`變數。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="cfdd4-136">請注意，檔案名稱為*MyHelpers.cshtml*，但是協助專家名為`MakeNote`。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="cfdd4-137">您可以將多個自訂的協助程式放入單一檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="cfdd4-138">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="cfdd4-139">在網頁中使用協助程式</span><span class="sxs-lookup"><span data-stu-id="cfdd4-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="cfdd4-140">在根資料夾中，建立新的空白檔案，稱為*TestHelper.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="cfdd4-141">將下列程式碼加入檔案：</span><span class="sxs-lookup"><span data-stu-id="cfdd4-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="cfdd4-142">若要呼叫您所建立的協助程式，使用`@`後面接著檔案名稱，協助程式所在位置，一個點，然後按一下 是協助程式名稱。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="cfdd4-143">(如果您有多個資料夾*應用程式\_程式碼*資料夾中，您可以使用語法`@FolderName.FileName.HelperName`呼叫您的協助專家內任何巢狀資料夾層級)。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="cfdd4-144">您加入以引號括號內的文字是協助程式會顯示過程中網頁的附註文字。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="cfdd4-145">儲存頁面，並在瀏覽器中執行它。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="cfdd4-146">協助程式會產生附註項目直接呼叫的協助程式的位置： 這兩個段落。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![在瀏覽器，並協助程式產生的標記，將指定的文字周圍方塊的方式顯示頁面的螢幕擷取畫面。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="cfdd4-148">其他資源</span><span class="sxs-lookup"><span data-stu-id="cfdd4-148">Additional Resources</span></span>


<span data-ttu-id="cfdd4-149">[做為 Razor 協助程式的水平功能表](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="cfdd4-150">由 Mike 主教此部落格文章會示範如何建立水平功能表做為使用標記、 CSS 和程式碼的協助程式。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="cfdd4-151">[利用 HTML5，在 ASP.NET Web Pages Helper for WebMatrix 及 ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="cfdd4-152">此部落格文章，Sam abraham （英文） 所顯示的協助程式呈現 HTML5`Canvas`項目。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="cfdd4-153">[之間的差異@Helpers並@Functions在 WebMatrix 中](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="cfdd4-154">由 Mike Brind 此部落格文章說明`@helper`語法和`@function`語法和使用時機。</span><span class="sxs-lookup"><span data-stu-id="cfdd4-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
