---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: 主版頁面 (C#) 中的 Url |Microsoft 文件
author: rick-anderson
description: 解決方式中的主版頁面的 Url 可能會中斷由於是在不同的相對目錄的內容頁面之外的主版頁面檔案。 查看重定基底...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 494338f1a0705c8d7e15bc693ae1ec6362b26d64
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887703"
---
<a name="urls-in-master-pages-c"></a><span data-ttu-id="542ad-104">主版頁面 (C#) 中的 Url</span><span class="sxs-lookup"><span data-stu-id="542ad-104">URLs in Master Pages (C#)</span></span>
====================
<span data-ttu-id="542ad-105">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="542ad-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="542ad-106">[下載程式碼](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip)或[下載 PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="542ad-106">[Download Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) or [Download PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)</span></span>

> <span data-ttu-id="542ad-107">解決方式中的主版頁面的 Url 可能會中斷由於是在不同的相對目錄的內容頁面之外的主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="542ad-107">Addresses how URLs in the master page can break due to the master page file being in a different relative directory than the content page.</span></span> <span data-ttu-id="542ad-108">查看重定基底 Url，透過 ~ 宣告式語法和以程式設計方式使用 ResolveUrl ResolveClientUrl 中。</span><span class="sxs-lookup"><span data-stu-id="542ad-108">Looks at rebasing URLs via ~ in the declarative syntax and using ResolveUrl and ResolveClientUrl programmatically.</span></span> <span data-ttu-id="542ad-109">（另請查看</span><span class="sxs-lookup"><span data-stu-id="542ad-109">(Also look at</span></span>


## <a name="introduction"></a><span data-ttu-id="542ad-110">簡介</span><span class="sxs-lookup"><span data-stu-id="542ad-110">Introduction</span></span>

<span data-ttu-id="542ad-111">在所有範例中我們看到了到目前為止，主要和內容頁面具有位於相同的資料夾 （網站的根資料夾）。</span><span class="sxs-lookup"><span data-stu-id="542ad-111">In all the examples we've seen thus far, the master and content pages have been located in the same folder (the website's root folder).</span></span> <span data-ttu-id="542ad-112">但是，沒有的理由為何 master 和內容頁必須是相同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="542ad-112">But there's no reason why the master and content pages must be in the same folder.</span></span> <span data-ttu-id="542ad-113">您當然可以在子資料夾中建立內容頁面。</span><span class="sxs-lookup"><span data-stu-id="542ad-113">You can certainly create content pages in subfolders.</span></span> <span data-ttu-id="542ad-114">同樣地，您可能會建立`~/MasterPages/`放置站台的主版頁面的資料夾。</span><span class="sxs-lookup"><span data-stu-id="542ad-114">Similarly, you might create a `~/MasterPages/` folder where you place your site's master pages.</span></span>

<span data-ttu-id="542ad-115">一個可能的問題，將主要和內容頁面放在不同的資料夾包括中斷的 Url。</span><span class="sxs-lookup"><span data-stu-id="542ad-115">One potential issue with placing master and content pages in different folders involves broken URLs.</span></span> <span data-ttu-id="542ad-116">如果主版頁面包含超連結、 影像或其他項目中的相對 Url，連結將會位於不同的資料夾中的內容頁面的無效。</span><span class="sxs-lookup"><span data-stu-id="542ad-116">If the master page contains relative URLs in hyperlinks, images, or other elements, the link will be invalid for content pages that reside in a different folder.</span></span> <span data-ttu-id="542ad-117">在本教學課程，我們會檢查此問題，以及因應措施的來源。</span><span class="sxs-lookup"><span data-stu-id="542ad-117">In this tutorial we examine the source of this problem as well as workarounds.</span></span>

## <a name="the-problem-with-relative-urls"></a><span data-ttu-id="542ad-118">使用相對 Url 的問題</span><span class="sxs-lookup"><span data-stu-id="542ad-118">The Problem with Relative URLs</span></span>

<span data-ttu-id="542ad-119">在網頁上的 URL 即為*相對 URL*如果它所指向的資源的位置是相對於網站的資料夾結構中的網頁上的位置。</span><span class="sxs-lookup"><span data-stu-id="542ad-119">A URL on a web page is said to be a *relative URL* if the location of the resource it points to is relative to the web page's location in the website's folder structure.</span></span> <span data-ttu-id="542ad-120">任何不是以正斜線開頭的 URL (`/`) 或通訊協定 (例如`http://`) 是相對的因為它會解析瀏覽器的網頁，其中包含 URL 的位置。</span><span class="sxs-lookup"><span data-stu-id="542ad-120">Any URL that does not start with a leading forward slash (`/`) or a protocol (such as `http://`) is relative because it is resolved by the browser based on the location of the web page that contains the URL.</span></span>

<span data-ttu-id="542ad-121">例如，我們的網站有`~/Images/`單一映像檔案後，資料夾`PoweredByASPNET.gif`。</span><span class="sxs-lookup"><span data-stu-id="542ad-121">For example, our website has an `~/Images/` folder with a single image file, `PoweredByASPNET.gif`.</span></span> <span data-ttu-id="542ad-122">主版頁面檔案`Site.master`具有`<img>`中的項目`footerContent`區域以下列標記：</span><span class="sxs-lookup"><span data-stu-id="542ad-122">The master page file `Site.master` has an `<img>` element in the `footerContent` region with the following markup:</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

<span data-ttu-id="542ad-123">`src`屬性值中的`<img>`項目為相對 URL，因為它不是以開頭`/`或`http://`。</span><span class="sxs-lookup"><span data-stu-id="542ad-123">The `src` attribute value in the `<img>` element is a relative URL because it does not start with `/` or `http://`.</span></span> <span data-ttu-id="542ad-124">簡單地說，`src`屬性值會告知瀏覽器中呈現的外觀`Images`子資料夾名為的檔案`PoweredByASPNET.gif`。</span><span class="sxs-lookup"><span data-stu-id="542ad-124">In short, the `src` attribute value tells the browser to look in the `Images` subfolder for a file named `PoweredByASPNET.gif`.</span></span>

<span data-ttu-id="542ad-125">當造訪內容頁面上，上述的標記是直接傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="542ad-125">When visiting a content page, the above markup is sent directly to the browser.</span></span> <span data-ttu-id="542ad-126">請花一點時間瀏覽`About.aspx`並檢視傳送到瀏覽器的 HTML 原始檔。</span><span class="sxs-lookup"><span data-stu-id="542ad-126">Take a moment to visit `About.aspx` and view the HTML source sent to the browser.</span></span> <span data-ttu-id="542ad-127">您會發現主版頁面中的完全相同標記已傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="542ad-127">You will find that the exact same markup in the master page was sent to the browser.</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

<span data-ttu-id="542ad-128">如果 [內容] 頁面的根資料夾中 (因為`About.aspx`) 的所有項目如預期般運作，所以`Images`相對於根資料夾的子資料夾。</span><span class="sxs-lookup"><span data-stu-id="542ad-128">If the content page is in the root folder (as is `About.aspx`) everything works as expected because there is an `Images` subfolder relative to the root folder.</span></span> <span data-ttu-id="542ad-129">不過，事情細分如果內容的頁面是在不同的主版頁面的資料夾。</span><span class="sxs-lookup"><span data-stu-id="542ad-129">However, things break down if the content page is in a different folder than the master page.</span></span> <span data-ttu-id="542ad-130">為了說明這點，建立名稱為的子`Admin`。</span><span class="sxs-lookup"><span data-stu-id="542ad-130">To illustrate this, create a subfolder named `Admin`.</span></span> <span data-ttu-id="542ad-131">接下來，加入名為的內容頁面`Default.aspx`至`Admin`資料夾，並確認新的頁面，即可將繫結`Site.master`主版頁面。</span><span class="sxs-lookup"><span data-stu-id="542ad-131">Next, add a content page named `Default.aspx` to the `Admin` folder, making sure to bind the new page to the `Site.master` master page.</span></span>

> [!NOTE]
> <span data-ttu-id="542ad-132">在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立名為基礎的自訂頁面類別`BasePage`，自動設定內容頁面的標題 (如果它未明確指派）。</span><span class="sxs-lookup"><span data-stu-id="542ad-132">In the [*Specifying the Title, Meta Tags, and Other HTML Headers in the Master Page*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial we created a custom base page class named `BasePage` that automatically set the content page's title (if it was not explicitly assigned).</span></span> <span data-ttu-id="542ad-133">別忘了讓新建立的網頁程式碼後置類別衍生自`BasePage`，讓它可以利用這項功能。</span><span class="sxs-lookup"><span data-stu-id="542ad-133">Don't forget to have the newly created page's code-behind class derive from `BasePage` so that it can utilize this functionality.</span></span>


<span data-ttu-id="542ad-134">建立此內容的頁面之後，您的方案總管 中看起來應該類似於圖 1。</span><span class="sxs-lookup"><span data-stu-id="542ad-134">After you have created this content page, your Solution Explorer should look similar to Figure 1.</span></span>


![新的資料夾和 ASP.NET 頁面加入至專案](urls-in-master-pages-cs/_static/image1.png)

<span data-ttu-id="542ad-136">**圖 01**： 新的資料夾和 ASP.NET 頁面加入至專案</span><span class="sxs-lookup"><span data-stu-id="542ad-136">**Figure 01**: A New Folder and ASP.NET Page Have Been Added to the Project</span></span>


<span data-ttu-id="542ad-137">接下來，更新`Web.sitemap`檔案以納入新`<siteMapNode>`這一課中的項目。</span><span class="sxs-lookup"><span data-stu-id="542ad-137">Next, update the `Web.sitemap` file to include a new `<siteMapNode>` entry for this lesson.</span></span> <span data-ttu-id="542ad-138">下列 XML 顯示完整`Web.sitemap`標記，現在包含加入的第三個`<siteMapNode>`項目。</span><span class="sxs-lookup"><span data-stu-id="542ad-138">The following XML shows the complete `Web.sitemap` markup, which now includes the addition of a third `<siteMapNode>` element.</span></span>


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

<span data-ttu-id="542ad-139">新建立`Default.aspx`頁面應該會有四個內容控制項對應中的四個 ContentPlaceHolders `Site.master`。</span><span class="sxs-lookup"><span data-stu-id="542ad-139">The newly created `Default.aspx` page should have four Content controls corresponding to the four ContentPlaceHolders in `Site.master`.</span></span> <span data-ttu-id="542ad-140">將一些文字加入至內容控制項參考`MainContent`ContentPlaceHolder，然後瀏覽透過瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="542ad-140">Add some text to the Content control referencing the `MainContent` ContentPlaceHolder and then visit the page through a browser.</span></span> <span data-ttu-id="542ad-141">如圖 2 所示，在瀏覽器找不到`PoweredByASPNET.gif`映像檔。</span><span class="sxs-lookup"><span data-stu-id="542ad-141">As Figure 2 shows, the browser cannot find the `PoweredByASPNET.gif` image file.</span></span> <span data-ttu-id="542ad-142">什麼呢？</span><span class="sxs-lookup"><span data-stu-id="542ad-142">What's going on here?</span></span>

<span data-ttu-id="542ad-143">`~/Admin/Default.aspx`內容頁面會將相同的 HTML 傳送給`footerContent`區域為`About.aspx`頁面：</span><span class="sxs-lookup"><span data-stu-id="542ad-143">The `~/Admin/Default.aspx` content page is sent the same HTML for the `footerContent` region as was the `About.aspx` page:</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

<span data-ttu-id="542ad-144">因為`<img>`項目的`src`屬性為相對 URL，在瀏覽器會嘗試尋找`Images`相對於網頁的資料夾位置的資料夾。</span><span class="sxs-lookup"><span data-stu-id="542ad-144">Because the `<img>` element's `src` attribute is a relative URL, the browser attempts to look for an `Images` folder relative to the web page's folder location.</span></span> <span data-ttu-id="542ad-145">換句話說，在瀏覽器正在尋找映像檔`Admin/Images/PoweredByASPNET.gif`。</span><span class="sxs-lookup"><span data-stu-id="542ad-145">In other words, the browser is looking for the image file `Admin/Images/PoweredByASPNET.gif`.</span></span>


<span data-ttu-id="542ad-146">[![找不到 PoweredByASPNET.gif 映像檔](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="542ad-146">[![The PoweredByASPNET.gif Image File Cannot Be Found](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)</span></span>

<span data-ttu-id="542ad-147">**圖 02**:`PoweredByASPNET.gif`映像找不到檔案 ([按一下以檢視完整大小的影像](urls-in-master-pages-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="542ad-147">**Figure 02**: The `PoweredByASPNET.gif` Image File Cannot Be Found  ([Click to view full-size image](urls-in-master-pages-cs/_static/image4.png))</span></span>


### <a name="replacing-relative-urls-with-absolute-urls"></a><span data-ttu-id="542ad-148">相對的 Url 取代為絕對 Url</span><span class="sxs-lookup"><span data-stu-id="542ad-148">Replacing Relative URLs with Absolute URLs</span></span>

<span data-ttu-id="542ad-149">相對 URL 的情況則相反*絕對 URL*，這是它的開頭是正斜線 (`/`) 或通訊協定，例如`http://`。</span><span class="sxs-lookup"><span data-stu-id="542ad-149">The opposite of a relative URL is an *absolute URL*, which is one that starts with a forward slash (`/`) or a protocol such as `http://`.</span></span> <span data-ttu-id="542ad-150">絕對 URL 指定位置的已知的固定端點中的資源，因為相同的絕對 URL 無效，在任何網頁上，不論網站的資料夾結構中的網頁上的位置。</span><span class="sxs-lookup"><span data-stu-id="542ad-150">Because an absolute URL specifies the location of a resource from a known fixed point, the same absolute URL is valid in any web page, regardless of the web page's location in the website's folder structure.</span></span>

<span data-ttu-id="542ad-151">若要補救不完整的影像圖 2 所示，我們需要更新`<img>`項目的`src`屬性，讓它使用而不是相對的絕對 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-151">To remedy the broken image shown in Figure 2, we need to update the `<img>` element's `src` attribute so that it uses an absolute URL instead of a relative one.</span></span> <span data-ttu-id="542ad-152">若要判斷正確的絕對 URL，請瀏覽您的網站中的網頁的其中一個並檢查 [網址] 列。</span><span class="sxs-lookup"><span data-stu-id="542ad-152">To determine the correct absolute URL, visit one of the web pages in your website and examine the Address bar.</span></span> <span data-ttu-id="542ad-153">如圖 2 中的網址列所示，web 應用程式的完整的路徑是`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`。</span><span class="sxs-lookup"><span data-stu-id="542ad-153">As the Address bar in Figure 2 shows, the fully qualified path to the web application is `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`.</span></span> <span data-ttu-id="542ad-154">因此，我們無法更新`<img>`項目的`src`屬性設定為下列兩個絕對 url:</span><span class="sxs-lookup"><span data-stu-id="542ad-154">Therefore, we could update the `<img>` element's `src` attribute to either of the following two absolute URLs:</span></span>

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

<span data-ttu-id="542ad-155">請花一點時間更新`<img>`項目的`src`屬性設定為使用其中一種形式，如上所示的絕對 URL，然後瀏覽`~/Admin/Default.aspx`透過瀏覽器的頁面。</span><span class="sxs-lookup"><span data-stu-id="542ad-155">Take a moment to update the `<img>` element's `src` attribute to an absolute URL using one of the forms shown above and then visit the `~/Admin/Default.aspx` page through a browser.</span></span> <span data-ttu-id="542ad-156">目前瀏覽器會正確地尋找並顯示`PoweredByASPNET.gif`映像檔案 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="542ad-156">This time the browser will correctly find and display the `PoweredByASPNET.gif` image file (see Figure 3).</span></span>


<span data-ttu-id="542ad-157">[![PoweredByASPNET.gif 影像是現在顯示](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="542ad-157">[![The PoweredByASPNET.gif Image is Now Displayed](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)</span></span>

<span data-ttu-id="542ad-158">**圖 03**:`PoweredByASPNET.gif`影像是現在顯示 ([按一下以檢視完整大小的影像](urls-in-master-pages-cs/_static/image7.png))</span><span class="sxs-lookup"><span data-stu-id="542ad-158">**Figure 03**: The `PoweredByASPNET.gif` Image is Now Displayed  ([Click to view full-size image](urls-in-master-pages-cs/_static/image7.png))</span></span>


<span data-ttu-id="542ad-159">硬式編碼的絕對 URL 中的運作方式，雖然緊密涉入 HTML 網站的伺服器和資料夾位置，可能會變更。</span><span class="sxs-lookup"><span data-stu-id="542ad-159">While hard coding in the absolute URL works, it tightly couples your HTML to the website's server and folder location, which may change.</span></span> <span data-ttu-id="542ad-160">使用表單的絕對 URL`http://localhost:3908/...`很容易損毀因為先前的連接埠號碼`localhost`會自動選取每次啟動 Visual Studio 內建 ASP.NET 開發 Web 伺服器時。</span><span class="sxs-lookup"><span data-stu-id="542ad-160">Using an absolute URL of the form `http://localhost:3908/...` is brittle because the port number preceding `localhost` is selected automatically each time Visual Studio's built-in ASP.NET Development Web Server is started.</span></span> <span data-ttu-id="542ad-161">同樣地，`http://localhost`時在本機測試組件才有效。</span><span class="sxs-lookup"><span data-stu-id="542ad-161">Similarly, the `http://localhost` part is only valid when testing locally.</span></span> <span data-ttu-id="542ad-162">程式碼部署到實際執行伺服器之後, URL 基底會變更為其他項目，例如`http://www.yourserver.com`。</span><span class="sxs-lookup"><span data-stu-id="542ad-162">Once the code is deployed to a production server, the URL base will change to something else, like `http://www.yourserver.com`.</span></span> <span data-ttu-id="542ad-163">在表單中的絕對 URL`/ASPNET_MasterPages_Tutorial_04_CS/...`也受到相同損毀，因為此應用程式路徑有時候開發和生產伺服器有不同。</span><span class="sxs-lookup"><span data-stu-id="542ad-163">The absolute URL in the form `/ASPNET_MasterPages_Tutorial_04_CS/...` also suffers from the same brittleness because oftentimes this application path differs between development and production servers.</span></span>

<span data-ttu-id="542ad-164">好消息是 ASP.NET 提供方法，以產生在執行階段的有效相對 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-164">The good news is that ASP.NET offers a method for generating a valid relative URL at runtime.</span></span>

## <a name="usingandresolveclienturl"></a><span data-ttu-id="542ad-165">使用`~`和`ResolveClientUrl`</span><span class="sxs-lookup"><span data-stu-id="542ad-165">Using`~`and`ResolveClientUrl`</span></span>

<span data-ttu-id="542ad-166">而是硬式編碼的絕對 URL 位址，比 ASP.NET 可讓網頁開發人員可以使用波狀符號 (`~`) 來表示 web 應用程式的根目錄。</span><span class="sxs-lookup"><span data-stu-id="542ad-166">Rather than hard code an absolute URL, ASP.NET allows page developers to use the tilde (`~`) to indicate the root of the web application.</span></span> <span data-ttu-id="542ad-167">例如，如稍早在本教學課程我可以使用標記法`~/Admin/Default.aspx`中的文字來參考`Default.aspx`頁面`Admin`資料夾。</span><span class="sxs-lookup"><span data-stu-id="542ad-167">For example, earlier in this tutorial I used the notation `~/Admin/Default.aspx` in the text to refer to the `Default.aspx` page in the `Admin` folder.</span></span> <span data-ttu-id="542ad-168">`~`表示`Admin`資料夾是 web 應用程式根目錄的子資料夾。</span><span class="sxs-lookup"><span data-stu-id="542ad-168">The `~` indicates that the `Admin` folder is a subfolder of the web application's root.</span></span>

<span data-ttu-id="542ad-169">`Control`類別的[`ResolveClientUrl`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)採用 URL 並加以修改以適當的控制項所在的網頁的相對 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-169">The `Control` class's [`ResolveClientUrl` method](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) takes a URL and modifies it to a relative URL appropriate for the web page on which the control resides.</span></span> <span data-ttu-id="542ad-170">例如，呼叫`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`從`About.aspx`傳回`Images/PoweredByASPNET.gif`。</span><span class="sxs-lookup"><span data-stu-id="542ad-170">For example, calling `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` from `About.aspx` returns `Images/PoweredByASPNET.gif`.</span></span> <span data-ttu-id="542ad-171">呼叫從`~/Admin/Default.aspx`，不過，會傳回`../Images/PoweredByASPNET.gif`。</span><span class="sxs-lookup"><span data-stu-id="542ad-171">Calling it from `~/Admin/Default.aspx`, however, returns `../Images/PoweredByASPNET.gif`.</span></span>

> [!NOTE]
> <span data-ttu-id="542ad-172">因為所有 ASP.NET 伺服器控制項都衍生自`Control`類別，所有的伺服器控制項有存取權`ResolveClientUrl`方法。</span><span class="sxs-lookup"><span data-stu-id="542ad-172">Because all ASP.NET server controls derive from the `Control` class, all server controls have access to the `ResolveClientUrl` method.</span></span> <span data-ttu-id="542ad-173">即使`Page`類別衍生自`Control`類別，這表示您可以使用這個方法，直接從您的 ASP.NET 網頁程式碼後置類別。</span><span class="sxs-lookup"><span data-stu-id="542ad-173">Even the `Page` class derives from the `Control` class, meaning that you can use this method directly from your ASP.NET pages' code-behind classes.</span></span>


### <a name="usingin-the-declarative-markup"></a><span data-ttu-id="542ad-174">使用`~`宣告式標記中</span><span class="sxs-lookup"><span data-stu-id="542ad-174">Using`~`in the Declarative Markup</span></span>

<span data-ttu-id="542ad-175">數個的 ASP.NET Web 控制項包含 URL 相關的屬性： 超連結控制項有`NavigateUrl`屬性; 影像控制項有`ImageUrl`屬性; 等等。</span><span class="sxs-lookup"><span data-stu-id="542ad-175">Several ASP.NET Web controls include URL-related properties: the HyperLink control has a `NavigateUrl` property; the Image control has an `ImageUrl` property; and so on.</span></span> <span data-ttu-id="542ad-176">進行轉譯時，這些控制項傳遞至其 URL 相關的屬性值`ResolveClientUrl`。</span><span class="sxs-lookup"><span data-stu-id="542ad-176">When rendered, these controls pass their URL-related property values to `ResolveClientUrl`.</span></span> <span data-ttu-id="542ad-177">因此，如果這些屬性包含`~`表示 web 應用程式的根目錄，URL 將會修改為有效的相對 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-177">Consequently, if these properties contain a `~` to indicate the root of the web application, the URL will be modified to a valid relative URL.</span></span>

<span data-ttu-id="542ad-178">請注意，ASP.NET 伺服器控制項轉換`~`URL 相關屬性中。</span><span class="sxs-lookup"><span data-stu-id="542ad-178">Keep in mind that only ASP.NET server controls transform the `~` in their URL-related properties.</span></span> <span data-ttu-id="542ad-179">如果`~`會顯示在靜態的 HTML 標記中，像是`<img src="~/Images/PoweredByASPNET.gif" />`，ASP.NET 引擎會將傳送`~`HTML 內容的其餘部分一起瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="542ad-179">If a `~` appears in static HTML markup, such as `<img src="~/Images/PoweredByASPNET.gif" />`, the ASP.NET engine sends the `~` to the browser along with the rest of the HTML content.</span></span> <span data-ttu-id="542ad-180">瀏覽器假設`~`是 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="542ad-180">The browser assumes that the `~` is part of the URL.</span></span> <span data-ttu-id="542ad-181">例如，如果瀏覽器收到標記`<img src="~/Images/PoweredByASPNET.gif" />`假設有一個名為子`~`子資料夾`Images`包含映像檔`PoweredByASPNET.gif`。</span><span class="sxs-lookup"><span data-stu-id="542ad-181">For example, if the browser receives the markup `<img src="~/Images/PoweredByASPNET.gif" />` it assumes there is a subfolder named `~` with a subfolder `Images` that contains the image file `PoweredByASPNET.gif`.</span></span>

<span data-ttu-id="542ad-182">若要修正的映像標記`Site.master`，取代現有`<img>`的 ASP.NET 影像 Web 控制項的項目。</span><span class="sxs-lookup"><span data-stu-id="542ad-182">To fix the image markup in `Site.master`, replace the existing `<img>` element with an ASP.NET Image Web control.</span></span> <span data-ttu-id="542ad-183">將映像 Web 控制項的`ID`至`PoweredByImage`、 其`ImageUrl`屬性`~/Images/PoweredByASPNET.gif`，且其`AlternateText`"提供 asp.net ！"的屬性</span><span class="sxs-lookup"><span data-stu-id="542ad-183">Set the Image Web control's `ID` to `PoweredByImage`, its `ImageUrl` property to `~/Images/PoweredByASPNET.gif`, and its `AlternateText` property to "Powered by ASP.NET!"</span></span>


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

<span data-ttu-id="542ad-184">變更主版頁面之後，再次瀏覽`~/Admin/Default.aspx`頁面上一次。</span><span class="sxs-lookup"><span data-stu-id="542ad-184">After making this change to the master page, revisit the `~/Admin/Default.aspx` page again.</span></span> <span data-ttu-id="542ad-185">這次`PoweredByASPNET.gif`頁面上會出現映像檔案 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="542ad-185">This time the `PoweredByASPNET.gif` image file appears in the page (see Figure 3).</span></span> <span data-ttu-id="542ad-186">控制項是映像網頁呈現時它會使用`ResolveClientUrl`方法來解決其`ImageUrl`屬性值。</span><span class="sxs-lookup"><span data-stu-id="542ad-186">When the Image Web control is rendered it uses the `ResolveClientUrl` method to resolve its `ImageUrl` property value.</span></span> <span data-ttu-id="542ad-187">在`~/Admin/Default.aspx``ImageUrl`轉換成適當的相對 URL，做為 HTML 來源顯示下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="542ad-187">In `~/Admin/Default.aspx` the `ImageUrl` is converted into an appropriate relative URL, as the following snippet of HTML source shows:</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> <span data-ttu-id="542ad-188">除了在 URL 為基礎的 Web 控制項的屬性，在使用`~`也可以用時呼叫`Response.Redirect`和`Server.MapPath`方法，和其他項目。</span><span class="sxs-lookup"><span data-stu-id="542ad-188">In addition to being used in URL-based Web control properties, the `~` can also be used when calling the `Response.Redirect` and `Server.MapPath` methods, among others.</span></span> <span data-ttu-id="542ad-189">此外，`ResolveClientUrl`方法可能會直接從叫用 ASP.NET 或主版頁面的宣告式標記，如有需要請參閱 < [Fritz 洋蔥](https://www.pluralsight.com/blogs/fritz/)的部落格項目[使用`ResolveClientUrl`標記中](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)。</span><span class="sxs-lookup"><span data-stu-id="542ad-189">Also, the `ResolveClientUrl` method may be invoked directly from an ASP.NET or master page's declarative markup, if needed; see [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)'s blog entry [Using `ResolveClientUrl` in Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).</span></span>


## <a name="fixing-the-master-pages-remaining-relative-urls"></a><span data-ttu-id="542ad-190">修正主版頁面的剩餘相對的 Url</span><span class="sxs-lookup"><span data-stu-id="542ad-190">Fixing the Master Page's Remaining Relative URLs</span></span>

<span data-ttu-id="542ad-191">除了`<img>`中的項目`footerContent`我們修正，、 主版頁面包含需要注意我們的一個更相對 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-191">In addition to the `<img>` element in the `footerContent` that we just fixed, the master page contains one more relative URL that requires our attention.</span></span> <span data-ttu-id="542ad-192">`topContent`區域包含連結 」 主版頁面教學課程中，「 指向`Default.aspx`。</span><span class="sxs-lookup"><span data-stu-id="542ad-192">The `topContent` region includes the link "Master Pages Tutorials," which points to `Default.aspx`.</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

<span data-ttu-id="542ad-193">此 URL 是相對的因為它會傳送使用者`Default.aspx`內容頁面他們造訪的資料夾中的頁面。</span><span class="sxs-lookup"><span data-stu-id="542ad-193">Because this URL is relative, it will send the user to the `Default.aspx` page in the folder of the content page they are visiting.</span></span> <span data-ttu-id="542ad-194">指向此連結`Default.aspx`我們需要更換的根資料夾中`<a>`項目與超連結 Web 控制項，以便我們可以使用`~`標記法。</span><span class="sxs-lookup"><span data-stu-id="542ad-194">To have this link always point to `Default.aspx` in the root folder we need to replace the `<a>` element with a HyperLink Web control so that we can use the `~` notation.</span></span>

<span data-ttu-id="542ad-195">移除`<a>`元素標記並將超連結控制項加入其所在位置。</span><span class="sxs-lookup"><span data-stu-id="542ad-195">Remove the `<a>` element markup and add a HyperLink control in its place.</span></span> <span data-ttu-id="542ad-196">設定超連結的`ID`至`lnkHome`、 其`NavigateUrl`屬性`~/Default.aspx`，且其`Text`屬性為"主版頁面教學課程 」。</span><span class="sxs-lookup"><span data-stu-id="542ad-196">Set the HyperLink's `ID` to `lnkHome`, its `NavigateUrl` property to `~/Default.aspx`, and its `Text` property to "Master Pages Tutorials."</span></span>


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

<span data-ttu-id="542ad-197">就這麼容易！</span><span class="sxs-lookup"><span data-stu-id="542ad-197">That's it!</span></span> <span data-ttu-id="542ad-198">此時，所有主版頁面和內容頁面，無論何種資料夾的內容頁轉譯時，我們主版頁面中的 Url 會正確基礎位於中。</span><span class="sxs-lookup"><span data-stu-id="542ad-198">At this point all the URLs in our master page are properly based when rendered by a content page regardless of what folders the master page and content page are located in.</span></span>

### <a name="automatic-url-resolution-in-theheadsection"></a><span data-ttu-id="542ad-199">中的自動 URL 解析`<head>`區段</span><span class="sxs-lookup"><span data-stu-id="542ad-199">Automatic URL Resolution in the`<head>`Section</span></span>

<span data-ttu-id="542ad-200">在[*建立全站台的版面配置使用主版頁面*](creating-a-site-wide-layout-using-master-pages-cs.md)教學課程中，我們加入`<link>`至`Styles.css`檔案`<head>`區域：</span><span class="sxs-lookup"><span data-stu-id="542ad-200">In the [*Creating a Site-Wide Layout Using Master Pages*](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial we added a `<link>` to the `Styles.css` file in the `<head>` region:</span></span>


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

<span data-ttu-id="542ad-201">雖然`<link>`項目的`href`屬性是相對，它會自動轉換為適當的路徑，在執行階段。</span><span class="sxs-lookup"><span data-stu-id="542ad-201">While the `<link>` element's `href` attribute is relative, it's automatically converted to an appropriate path at runtime.</span></span> <span data-ttu-id="542ad-202">如我們所述[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中，`<head>`區域是實際的伺服器端控制項，讓它能夠修改其內部的控制項呈現時的內容。</span><span class="sxs-lookup"><span data-stu-id="542ad-202">As we discussed in the [*Specifying the Title, Meta Tags, and Other HTML Headers in the Master Page*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial, the `<head>` region is actually a server-side control, which enables it to modify the contents of its inner controls when it is rendered.</span></span>

<span data-ttu-id="542ad-203">若要確認這種情況，請再次造訪`~/Admin/Default.aspx`頁面，並檢視傳送到瀏覽器的 HTML 原始檔。</span><span class="sxs-lookup"><span data-stu-id="542ad-203">To verify this, revisit the `~/Admin/Default.aspx` page and view the HTML source sent to the browser.</span></span> <span data-ttu-id="542ad-204">如以下程式碼片段所示，`<link>`項目的`href`屬性會自動修改以適當的相對 URL， `../Styles.css`。</span><span class="sxs-lookup"><span data-stu-id="542ad-204">As the snippet below illustrates, the `<link>` element's `href` attribute has been automatically modified to an appropriate relative URL, `../Styles.css`.</span></span>


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a><span data-ttu-id="542ad-205">總結</span><span class="sxs-lookup"><span data-stu-id="542ad-205">Summary</span></span>

<span data-ttu-id="542ad-206">主版頁面通常包括連結、 影像和其他外部資源，必須指定透過 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-206">Master pages very often include links, images, and other external resources that must be specified via a URL.</span></span> <span data-ttu-id="542ad-207">因為主版頁面和內容頁面可能不存在於相同的資料夾中，務必 abstain 使用相對的 Url。</span><span class="sxs-lookup"><span data-stu-id="542ad-207">Because the master page and content pages might not exist in the same folder, it is important to abstain from using relative URLs.</span></span> <span data-ttu-id="542ad-208">雖然可以使用硬式編碼的絕對 Url，因此緊密結合的絕對 URL 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="542ad-208">While it is possible to use hard coded absolute URLs, doing so tightly couples the absolute URL to the web application.</span></span> <span data-ttu-id="542ad-209">如果變更的絕對 URL-通常會移動或 web 應用程式的部署時，您必須記得要返回並更新絕對 Url。</span><span class="sxs-lookup"><span data-stu-id="542ad-209">If the absolute URL changes - as it often does when moving or deploying a web application - you'll have to remember to go back and update the absolute URLs.</span></span>

<span data-ttu-id="542ad-210">理想的方法是使用波狀符號 (`~`) 來表示應用程式根目錄。</span><span class="sxs-lookup"><span data-stu-id="542ad-210">The ideal approach is to use the tilde (`~`) to indicate the application root.</span></span> <span data-ttu-id="542ad-211">ASP.NET Web 控制項包含 URL 相關的屬性對應`~`在執行階段應用程式根目錄。</span><span class="sxs-lookup"><span data-stu-id="542ad-211">ASP.NET Web controls that contain URL-related properties map the `~` to the application root at runtime.</span></span> <span data-ttu-id="542ad-212">就內部而言，Web 控制項使用`Control`類別的`ResolveClientUrl`方法來產生有效的相對 URL。</span><span class="sxs-lookup"><span data-stu-id="542ad-212">Internally, the Web controls use the `Control` class's `ResolveClientUrl` method to generate a valid relative URL.</span></span> <span data-ttu-id="542ad-213">這個方法是公用的而且可從每個伺服器控制項 (包括`Page`類別)，因此您可以使用它以程式設計方式從您的程式碼後置類別，如有需要。</span><span class="sxs-lookup"><span data-stu-id="542ad-213">This method is public and available from every server control (including the `Page` class), so you can use it programmatically from your code-behind classes, if needed.</span></span>

<span data-ttu-id="542ad-214">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="542ad-214">Happy Programming!</span></span>

### <a name="further-reading"></a><span data-ttu-id="542ad-215">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="542ad-215">Further Reading</span></span>

<span data-ttu-id="542ad-216">如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="542ad-216">For more information on the topics discussed in this tutorial, refer to the following resources:</span></span>

- [<span data-ttu-id="542ad-217">在 ASP.NET 中的主版頁面</span><span class="sxs-lookup"><span data-stu-id="542ad-217">Master Pages in ASP.NET</span></span>](http://www.odetocode.com/Articles/419.aspx)
- [<span data-ttu-id="542ad-218">在主版頁面的 URL 重定基底</span><span class="sxs-lookup"><span data-stu-id="542ad-218">URL Rebasing in a Master Page</span></span>](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [<span data-ttu-id="542ad-219">使用`ResolveClientUrl`標記中</span><span class="sxs-lookup"><span data-stu-id="542ad-219">Using `ResolveClientUrl` in Markup</span></span>](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a><span data-ttu-id="542ad-220">關於作者</span><span class="sxs-lookup"><span data-stu-id="542ad-220">About the Author</span></span>

<span data-ttu-id="542ad-221">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。</span><span class="sxs-lookup"><span data-stu-id="542ad-221">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of multiple ASP/ASP.NET books and founder of 4GuysFromRolla.com, has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="542ad-222">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="542ad-222">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="542ad-223">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="542ad-223">His latest book is [*Sams Teach Yourself ASP.NET 3.5 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="542ad-224">在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。</span><span class="sxs-lookup"><span data-stu-id="542ad-224">Scott can be reached at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) or via his blog at [http://ScottOnWriting.NET](http://scottonwriting.net/).</span></span>

### <a name="special-thanks-to"></a><span data-ttu-id="542ad-225">特別感謝</span><span class="sxs-lookup"><span data-stu-id="542ad-225">Special Thanks To</span></span>

<span data-ttu-id="542ad-226">檢閱我即將推出的 MSDN 文件有興趣嗎？</span><span class="sxs-lookup"><span data-stu-id="542ad-226">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="542ad-227">如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。</span><span class="sxs-lookup"><span data-stu-id="542ad-227">If so, drop me a line at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="542ad-228">[上一頁](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [下一頁](control-id-naming-in-content-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="542ad-228">[Previous](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
[Next](control-id-naming-in-content-pages-cs.md)</span></span>
