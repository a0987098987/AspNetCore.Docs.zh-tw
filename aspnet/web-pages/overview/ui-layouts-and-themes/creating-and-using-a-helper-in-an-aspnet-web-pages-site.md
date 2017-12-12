---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "建立和使用協助程式中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。 協助程式是可重複使用的元件，其中包含程式碼和效能標記..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="95e98-104">建立和使用 ASP.NET Web Pages (Razor) 網站中的協助程式</span><span class="sxs-lookup"><span data-stu-id="95e98-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="95e98-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="95e98-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="95e98-106">本文說明如何在 ASP.NET Web Pages (Razor) 網站上建立協助程式。</span><span class="sxs-lookup"><span data-stu-id="95e98-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="95e98-107">A *helper*是可重複使用的元件，包括程式碼和標記，以執行可能很繁瑣或是複雜的工作。</span><span class="sxs-lookup"><span data-stu-id="95e98-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="95e98-108">**您將學習：**</span><span class="sxs-lookup"><span data-stu-id="95e98-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="95e98-109">如何建立及使用簡單的 helper。</span><span class="sxs-lookup"><span data-stu-id="95e98-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="95e98-110">這些是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="95e98-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="95e98-111">`@helper`語法。</span><span class="sxs-lookup"><span data-stu-id="95e98-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="95e98-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="95e98-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="95e98-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="95e98-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="95e98-114">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="95e98-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="95e98-115">協助程式的概觀</span><span class="sxs-lookup"><span data-stu-id="95e98-115">Overview of Helpers</span></span>

<span data-ttu-id="95e98-116">如果您需要在您的網站不同頁面上執行相同的工作，您可以使用協助程式。</span><span class="sxs-lookup"><span data-stu-id="95e98-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="95e98-117">ASP.NET Web Pages 包含數個 helper，而且還有更多，您可以下載並安裝。</span><span class="sxs-lookup"><span data-stu-id="95e98-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="95e98-118">(ASP.NET Web Pages 中內建的協助程式的清單會列於[ASP.NET 應用程式開發介面的快速參考](https://go.microsoft.com/fwlink/?LinkId=202907)。)如果沒有現有的協助程式符合您的需求，您可以建立您自己的 helper。</span><span class="sxs-lookup"><span data-stu-id="95e98-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="95e98-119">協助程式可讓您使用一般程式碼區塊，跨多個頁面。</span><span class="sxs-lookup"><span data-stu-id="95e98-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="95e98-120">假設，在您的網頁通常想要建立附註項目，除了一般段落格式設定。</span><span class="sxs-lookup"><span data-stu-id="95e98-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="95e98-121">請注意可能會建立為`<div>`項目，具有樣式為框線方塊。</span><span class="sxs-lookup"><span data-stu-id="95e98-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="95e98-122">而非每次您想要顯示一個提示，請將這個相同的標記加入頁面，您就可以封裝標記成 helper。</span><span class="sxs-lookup"><span data-stu-id="95e98-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="95e98-123">然後，您就可以插入一行程式碼用在您需要的任何位置。</span><span class="sxs-lookup"><span data-stu-id="95e98-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="95e98-124">使用像這樣的 helper 在每個頁面中讓程式碼，更簡單且容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="95e98-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="95e98-125">它可更輕鬆地維護您的網站，因為如果您需要變更便箋的外觀，您可以變更在一個位置的標記。</span><span class="sxs-lookup"><span data-stu-id="95e98-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="95e98-126">建立 Helper</span><span class="sxs-lookup"><span data-stu-id="95e98-126">Creating a Helper</span></span>

<span data-ttu-id="95e98-127">此程序會示範如何建立建立便箋，如上所述的協助程式。</span><span class="sxs-lookup"><span data-stu-id="95e98-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="95e98-128">這是簡單的範例，但自訂 helper 可以包含任何標記，且您需要的 ASP.NET 程式碼。</span><span class="sxs-lookup"><span data-stu-id="95e98-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="95e98-129">在網站的根資料夾中建立資料夾，名為*應用程式\_程式碼*。</span><span class="sxs-lookup"><span data-stu-id="95e98-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="95e98-130">這是您可以在其中放置像協助程式元件的程式碼在 ASP.NET 中保留的資料夾名稱。</span><span class="sxs-lookup"><span data-stu-id="95e98-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="95e98-131">在*應用程式\_程式碼*資料夾建立新*.cshtml*檔案並將其命名*MyHelpers.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="95e98-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="95e98-132">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="95e98-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="95e98-133">程式碼會使用`@helper`語法以宣告新的協助程式，名為`MakeNote`。</span><span class="sxs-lookup"><span data-stu-id="95e98-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="95e98-134">此特定的協助程式可讓您將名為參數傳遞`content`，可以包含文字與標記的組合。</span><span class="sxs-lookup"><span data-stu-id="95e98-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="95e98-135">協助專家將字串插入注意本文會使用`@content`變數。</span><span class="sxs-lookup"><span data-stu-id="95e98-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="95e98-136">請注意檔案名為*MyHelpers.cshtml*，但是協助專家名為`MakeNote`。</span><span class="sxs-lookup"><span data-stu-id="95e98-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="95e98-137">您可以將多個自訂協助程式放入單一檔案。</span><span class="sxs-lookup"><span data-stu-id="95e98-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="95e98-138">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="95e98-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="95e98-139">在網頁中使用 Helper</span><span class="sxs-lookup"><span data-stu-id="95e98-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="95e98-140">在根資料夾中建立新的空白檔案，稱為*TestHelper.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="95e98-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="95e98-141">將下列程式碼加入檔案：</span><span class="sxs-lookup"><span data-stu-id="95e98-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="95e98-142">若要呼叫您建立的協助，請使用`@`後面的檔案名稱，協助專家，所在的點，再接著的協助程式名稱。</span><span class="sxs-lookup"><span data-stu-id="95e98-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="95e98-143">(如果您有多個資料夾*應用程式\_程式碼*資料夾中，您可以使用語法`@FolderName.FileName.HelperName`呼叫您的協助專家內任何巢狀資料夾層級)。</span><span class="sxs-lookup"><span data-stu-id="95e98-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="95e98-144">您加入以引號括號內的文字是協助專家將一部分請注意，在網頁上顯示的文字。</span><span class="sxs-lookup"><span data-stu-id="95e98-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="95e98-145">儲存頁面，並在瀏覽器中執行。</span><span class="sxs-lookup"><span data-stu-id="95e98-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="95e98-146">協助專家會產生附註項目右您用來呼叫 helper： 之間兩個段落。</span><span class="sxs-lookup"><span data-stu-id="95e98-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![在瀏覽器，並協助專家產生標記，放入指定的文字周圍方塊的方式顯示頁面的螢幕擷取畫面。](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="95e98-148">其他資源</span><span class="sxs-lookup"><span data-stu-id="95e98-148">Additional Resources</span></span>


<span data-ttu-id="95e98-149">[水平 功能表做 Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341)。</span><span class="sxs-lookup"><span data-stu-id="95e98-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="95e98-150">Mike 教宗此部落格文章會示範如何建立與協助專家使用標記、 CSS 和程式碼的水平的功能表。</span><span class="sxs-lookup"><span data-stu-id="95e98-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="95e98-151">[利用 HTML5 中 ASP.NET Web Pages Helper for WebMatrix 及 ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx)。</span><span class="sxs-lookup"><span data-stu-id="95e98-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="95e98-152">由 Sam 林肯此部落格文章示範 helper 呈現 HTML5`Canvas`項目。</span><span class="sxs-lookup"><span data-stu-id="95e98-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="95e98-153">[之間的差異@Helpers和@Functions在 WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix)。</span><span class="sxs-lookup"><span data-stu-id="95e98-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="95e98-154">Mike Brind 此部落格文章說明`@helper`語法和`@function`語法以及何時使用每個。</span><span class="sxs-lookup"><span data-stu-id="95e98-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
