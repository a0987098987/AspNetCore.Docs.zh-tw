---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "ASP.NET MVC 4 行動功能 |Microsoft 文件"
author: Rick-Anderson
description: "現在是在部署 ASP.NET MVC 5 Mobile Web 應用程式在 Azure 網站的程式碼範例使用本教學課程的 MVC 5 版本。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: f4e0e4eb558e0c7b9e94fc83ede986fa4c666739
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4-mobile-features"></a><span data-ttu-id="13a55-103">ASP.NET MVC 4 Mobile 功能</span><span class="sxs-lookup"><span data-stu-id="13a55-103">ASP.NET MVC 4 Mobile Features</span></span>
====================
<span data-ttu-id="13a55-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="13a55-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="13a55-105">現在是使用程式碼範例，在本教學課程的 MVC 5 版本[ASP.NET MVC 5 Mobile Web 應用程式部署在 Azure 網站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)。</span><span class="sxs-lookup"><span data-stu-id="13a55-105">There is now an MVC 5 version of this tutorial with code samples at [Deploy an ASP.NET MVC 5 Mobile Web Application on Azure Web Sites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).</span></span>


<span data-ttu-id="13a55-106">本教學課程將告訴您如何使用 ASP.NET MVC 4 Web 應用程式中的行動裝置功能的基本概念。</span><span class="sxs-lookup"><span data-stu-id="13a55-106">This tutorial will teach you the basics of how to work with mobile features in an ASP.NET MVC 4 Web application.</span></span> <span data-ttu-id="13a55-107">此教學課程中，您可以使用[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)或 Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer 或 VWD&quot;)。</span><span class="sxs-lookup"><span data-stu-id="13a55-107">For this tutorial, you can use [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) or Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer or VWD&quot;).</span></span> <span data-ttu-id="13a55-108">如果您已經有的您可以使用 Visual Studio professional 版。</span><span class="sxs-lookup"><span data-stu-id="13a55-108">You can use the professional version of Visual Studio if you already have that.</span></span>

<span data-ttu-id="13a55-109">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="13a55-109">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="13a55-110">[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) （建議選項） 或 Visual Studio Web Developer Express SP1。</span><span class="sxs-lookup"><span data-stu-id="13a55-110">[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recommended) or Visual Studio Web Developer Express SP1.</span></span> <span data-ttu-id="13a55-111">Visual Studio 2012 包含 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="13a55-111">Visual Studio 2012 contains ASP.NET MVC 4.</span></span> <span data-ttu-id="13a55-112">如果您使用 Visual Web Developer 2010，您必須安裝[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)。</span><span class="sxs-lookup"><span data-stu-id="13a55-112">If you are using Visual Web Developer 2010, you must install [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).</span></span>

<span data-ttu-id="13a55-113">您也需要行動電話瀏覽器模擬器。</span><span class="sxs-lookup"><span data-stu-id="13a55-113">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="13a55-114">下列其中一項工作將會：</span><span class="sxs-lookup"><span data-stu-id="13a55-114">Any of the following will work:</span></span>

- <span data-ttu-id="13a55-115">[Windows 7 Phone 模擬器](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。</span><span class="sxs-lookup"><span data-stu-id="13a55-115">[Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx).</span></span> <span data-ttu-id="13a55-116">（這是用於此教學課程中大部分的螢幕擷取畫面中的模擬器）。</span><span class="sxs-lookup"><span data-stu-id="13a55-116">(This is the emulator that's used in most of the screen shots in this tutorial.)</span></span>
- <span data-ttu-id="13a55-117">變更模擬 iPhone 的使用者代理字串。</span><span class="sxs-lookup"><span data-stu-id="13a55-117">Change the user agent string to emulate an iPhone.</span></span> <span data-ttu-id="13a55-118">請參閱[這](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)部落格文章。</span><span class="sxs-lookup"><span data-stu-id="13a55-118">See [this](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) blog entry.</span></span>
- [<span data-ttu-id="13a55-119">Opera Mobile 模擬器</span><span class="sxs-lookup"><span data-stu-id="13a55-119">Opera Mobile Emulator</span></span>](http://www.opera.com/developer/tools/mobile/)
- <span data-ttu-id="13a55-120">[Apple Safari](http://www.apple.com/safari/download/)設 iPhone 使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="13a55-120">[Apple Safari](http://www.apple.com/safari/download/) with the user agent set to iPhone.</span></span> <span data-ttu-id="13a55-121">如需如何將使用者代理程式在 Safari 中設定為"iPhone 」 的指示，請參閱[如何讓 Safari 假裝其 IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison 部落格上。</span><span class="sxs-lookup"><span data-stu-id="13a55-121">For instructions on how to set the user agent in Safari to "iPhone", see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) on David Alison's blog.</span></span>

<span data-ttu-id="13a55-122">使用 C# 原始程式碼的 visual Studio 專案會隨附本主題：</span><span class="sxs-lookup"><span data-stu-id="13a55-122">Visual Studio projects with C# source code are available to accompany this topic:</span></span>

- [<span data-ttu-id="13a55-123">入門專案下載</span><span class="sxs-lookup"><span data-stu-id="13a55-123">Starter project download</span></span>](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [<span data-ttu-id="13a55-124">完成專案下載</span><span class="sxs-lookup"><span data-stu-id="13a55-124">Completed project download</span></span>](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a><span data-ttu-id="13a55-125">您將建置</span><span class="sxs-lookup"><span data-stu-id="13a55-125">What You'll Build</span></span>

<span data-ttu-id="13a55-126">此教學課程中，您會加入 mobile 功能中提供的簡單會議清單應用程式至[入門專案](https://go.microsoft.com/fwlink/?LinkId=228307)。</span><span class="sxs-lookup"><span data-stu-id="13a55-126">For this tutorial, you'll add mobile features to the simple conference-listing application that's provided in the [starter project](https://go.microsoft.com/fwlink/?LinkId=228307).</span></span> <span data-ttu-id="13a55-127">下列螢幕擷取畫面會顯示完成的應用程式的標記頁面中所見[Windows 7 Phone 模擬器](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)。</span><span class="sxs-lookup"><span data-stu-id="13a55-127">The following screenshot shows the tags page of the completed application as seen in the [Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx).</span></span> <span data-ttu-id="13a55-128">請參閱[鍵盤對應的 Windows Phone 模擬器](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx)簡化鍵盤輸入。</span><span class="sxs-lookup"><span data-stu-id="13a55-128">See [Keyboard Mapping for Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) to simplify keyboard input.</span></span>

<span data-ttu-id="13a55-129">[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-129">[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)</span></span>

<span data-ttu-id="13a55-130">您可以使用 Internet Explorer 9 或 10、 FireFox 或 Chrome 開發所設定的行動應用程式版本[使用者代理字串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)。</span><span class="sxs-lookup"><span data-stu-id="13a55-130">You can use Internet Explorer version 9 or 10, FireFox or Chrome to develop your mobile application by setting the [user agent string](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/).</span></span> <span data-ttu-id="13a55-131">下圖顯示已完成本教學課程，並使用模擬 iPhone 的 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="13a55-131">The following image shows the completed tutorial using Internet Explorer emulating an iPhone.</span></span> <span data-ttu-id="13a55-132">您可以使用 Internet Explorer F-12 開發者工具與[Fiddler 工具](http://www.fiddler2.com/fiddler2/)協助偵錯您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="13a55-132">You can use the Internet Explorer F-12 developer tools and the [Fiddler tool](http://www.fiddler2.com/fiddler2/) to help debug your application.</span></span>

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a><span data-ttu-id="13a55-133">您將學習到的技術</span><span class="sxs-lookup"><span data-stu-id="13a55-133">Skills You'll Learn</span></span>

<span data-ttu-id="13a55-134">以下是您將學習：</span><span class="sxs-lookup"><span data-stu-id="13a55-134">Here's what you'll learn:</span></span>

- <span data-ttu-id="13a55-135">ASP.NET MVC 4 範本如何使用 HTML5`viewport`屬性和調整呈現，若要改善顯示行動裝置上。</span><span class="sxs-lookup"><span data-stu-id="13a55-135">How the ASP.NET MVC 4 templates use the HTML5 `viewport` attribute and adaptive rendering to improve display on mobile devices.</span></span>
- <span data-ttu-id="13a55-136">如何建立行動裝置的特定檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-136">How to create mobile-specific views.</span></span>
- <span data-ttu-id="13a55-137">如何建立檢視切換器，可讓使用者切換行動檢視和應用程式的桌面檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-137">How to create a view switcher that lets users toggle between a mobile view and a desktop view of the application.</span></span>

### <a name="getting-started"></a><span data-ttu-id="13a55-138">快速入門</span><span class="sxs-lookup"><span data-stu-id="13a55-138">Getting Started</span></span>

<span data-ttu-id="13a55-139">下載會議清單應用程式入門專案，使用下列連結：[下載](https://go.microsoft.com/fwlink/?LinkId=228307)。</span><span class="sxs-lookup"><span data-stu-id="13a55-139">Download the conference-listing application for the starter project using the following link: [Download](https://go.microsoft.com/fwlink/?LinkId=228307).</span></span> <span data-ttu-id="13a55-140">然後在 Windows 檔案總管中，以滑鼠右鍵按一下*MvcMobile.zip*檔案，然後選擇**屬性**。</span><span class="sxs-lookup"><span data-stu-id="13a55-140">Then in Windows Explorer, right-click the *MvcMobile.zip* file and choose **Properties**.</span></span> <span data-ttu-id="13a55-141">在**MvcMobile.zip 屬性**對話方塊方塊中，選擇**解除封鎖** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="13a55-141">In the **MvcMobile.zip Properties** dialog box, choose the **Unblock** button.</span></span> <span data-ttu-id="13a55-142">(解除封鎖可避免安全性警告，當您嘗試使用時，就會發生*.zip*您已經從 web 下載的檔案。)</span><span class="sxs-lookup"><span data-stu-id="13a55-142">(Unblocking prevents a security warning that occurs when you try to use a *.zip* file that you've downloaded from the web.)</span></span>

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

<span data-ttu-id="13a55-144">以滑鼠右鍵按一下*MvcMobile.zip*檔案，然後選取**全部解壓縮**來解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-144">Right-click the *MvcMobile.zip* file and select **Extract All** to unzip the file.</span></span> <span data-ttu-id="13a55-145">在 Visual Studio 中開啟*MvcMobile.sln*檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-145">In Visual Studio, open the *MvcMobile.sln* file.</span></span>

<span data-ttu-id="13a55-146">按 CTRL + F5 執行應用程式，將會顯示在您的桌面瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="13a55-146">Press CTRL+F5 to run the application, which will display it in your desktop browser.</span></span> <span data-ttu-id="13a55-147">啟動您的行動電話瀏覽器模擬器，將會議應用程式的 URL 複製到模擬器，然後按一下**標記來瀏覽**連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-147">Start your mobile browser emulator, copy the URL for the conference application into the emulator, and then click the **Browse by tag** link.</span></span> <span data-ttu-id="13a55-148">如果您使用 Windows Phone 模擬器，在 URL 橫條中按一下，然後按鍵盤存取 Pause 鍵。</span><span class="sxs-lookup"><span data-stu-id="13a55-148">If you are using the Windows Phone Emulator, click in the URL bar and press the Pause key to get keyboard access.</span></span> <span data-ttu-id="13a55-149">下的圖顯示*AllTags*檢視 (從選擇**標記來瀏覽**)。</span><span class="sxs-lookup"><span data-stu-id="13a55-149">The image below shows the *AllTags* view (from choosing **Browse by tag**).</span></span>

<span data-ttu-id="13a55-150">[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-150">[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)</span></span>

<span data-ttu-id="13a55-151">顯示是很容易閱讀，行動裝置上。</span><span class="sxs-lookup"><span data-stu-id="13a55-151">The display is very readable on a mobile device.</span></span> <span data-ttu-id="13a55-152">選擇 ASP.NET 連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-152">Choose the ASP.NET link.</span></span>

<span data-ttu-id="13a55-153">[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-153">[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)</span></span>

<span data-ttu-id="13a55-154">ASP.NET 標記檢視是得雜亂。</span><span class="sxs-lookup"><span data-stu-id="13a55-154">The ASP.NET tag view is very cluttered.</span></span> <span data-ttu-id="13a55-155">例如，**日期**資料行是非常難以閱讀。</span><span class="sxs-lookup"><span data-stu-id="13a55-155">For example, the **Date** column is very difficult to read.</span></span> <span data-ttu-id="13a55-156">稍後在本教學課程中，您將建立的版本*AllTags*檢視，是專為行動瀏覽器，並將會成為可讀取的顯示。</span><span class="sxs-lookup"><span data-stu-id="13a55-156">Later in the tutorial you'll create a version of the *AllTags* view that's specifically for mobile browsers and that will make the display readable.</span></span>

<span data-ttu-id="13a55-157">注意： 目前 bug 存在行動的快取引擎中。</span><span class="sxs-lookup"><span data-stu-id="13a55-157">Note: Currently a bug exists in the mobile caching engine.</span></span> <span data-ttu-id="13a55-158">對於生產應用程式，您必須安裝[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget 封裝。</span><span class="sxs-lookup"><span data-stu-id="13a55-158">For production applications, you must install the [Fixed DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget package.</span></span> <span data-ttu-id="13a55-159">請參閱[ASP.NET MVC 4 行動快取 Bug 固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)如修正程式的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="13a55-159">See [ASP.NET MVC 4 Mobile Caching Bug Fixed](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) for details on the fix.</span></span>

## <a name="css-media-queries"></a><span data-ttu-id="13a55-160">CSS 媒體查詢</span><span class="sxs-lookup"><span data-stu-id="13a55-160">CSS Media Queries</span></span>

<span data-ttu-id="13a55-161">[CSS 媒體查詢](http://www.w3.org/TR/css3-mediaqueries/)是 CSS 的媒體類型的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="13a55-161">[CSS media queries](http://www.w3.org/TR/css3-mediaqueries/) are an extension to CSS for media types.</span></span> <span data-ttu-id="13a55-162">它們可讓您建立覆寫預設 CSS 規則的特定瀏覽器 （使用者代理程式） 的規則。</span><span class="sxs-lookup"><span data-stu-id="13a55-162">They allow you to create rules that override the default CSS rules for specific browsers (user agents).</span></span> <span data-ttu-id="13a55-163">CSS 行動瀏覽器為目標的一般規則定義的最大螢幕大小。</span><span class="sxs-lookup"><span data-stu-id="13a55-163">A common rule for CSS that targets mobile browsers is defining the maximum screen size.</span></span> <span data-ttu-id="13a55-164">*Content\Site.css*當您建立新的 ASP.NET MVC 4 網際網路專案時所建立的檔案包含下列的媒體查詢：</span><span class="sxs-lookup"><span data-stu-id="13a55-164">The *Content\Site.css* file that's created when you create a new ASP.NET MVC 4 Internet project contains the following media query:</span></span>

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

<span data-ttu-id="13a55-165">如果瀏覽器視窗中是 850 像素寬或較少，則會使用此媒體區塊內的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="13a55-165">If the browser window is 850 pixels wide or less, it will use the CSS rules inside this media block.</span></span> <span data-ttu-id="13a55-166">您可以使用像這樣的 CSS 媒體查詢提供較寬的桌面瀏覽器顯示 HTML 內容的更佳顯示小的瀏覽器 （例如行動瀏覽器） 比預設 CSS 規則所設計。</span><span class="sxs-lookup"><span data-stu-id="13a55-166">You can use CSS media queries like this to provide a better display of HTML content on small browsers (like mobile browsers) than the default CSS rules that are designed for the wider displays of desktop browsers.</span></span>

## <a name="the-viewport-meta-tag"></a><span data-ttu-id="13a55-167">檢視區的中繼標籤</span><span class="sxs-lookup"><span data-stu-id="13a55-167">The Viewport Meta Tag</span></span>

<span data-ttu-id="13a55-168">大部分行動瀏覽器定義虛擬瀏覽器視窗寬度 (*檢視區*)，會遠超過在行動裝置的實際寬度。</span><span class="sxs-lookup"><span data-stu-id="13a55-168">Most mobile browsers define a virtual browser window width (the *viewport*) that's much larger than the actual width of the mobile device.</span></span> <span data-ttu-id="13a55-169">這可讓行動瀏覽器，以符合整個網頁內虛擬顯示器。</span><span class="sxs-lookup"><span data-stu-id="13a55-169">This allows mobile browsers to fit the entire web page inside the virtual display.</span></span> <span data-ttu-id="13a55-170">使用者可以再放大有趣的內容。</span><span class="sxs-lookup"><span data-stu-id="13a55-170">Users can then zoom in on interesting content.</span></span> <span data-ttu-id="13a55-171">不過，如果您的檢視區寬度設定為在實際裝置寬度時，沒有縮放是必要項目，因為內容可以放入行動瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="13a55-171">However, if you set the viewport width to the actual device width, no zooming is required, because the content fits in the mobile browser.</span></span>

<span data-ttu-id="13a55-172">檢視區`<meta>`ASP.NET MVC 4 版面配置檔案中的標記將檢視區設定為裝置寬度。</span><span class="sxs-lookup"><span data-stu-id="13a55-172">The viewport `<meta>` tag in the ASP.NET MVC 4 layout file sets the viewport to the device width.</span></span> <span data-ttu-id="13a55-173">下列的線條將會顯示在檢視區`<meta>`ASP.NET MVC 4 版面配置檔案中的標記。</span><span class="sxs-lookup"><span data-stu-id="13a55-173">The following line shows the viewport `<meta>` tag in the ASP.NET MVC 4 layout file.</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a><span data-ttu-id="13a55-174">檢查 CSS 媒體查詢和檢視區 Meta 標記的效果</span><span class="sxs-lookup"><span data-stu-id="13a55-174">Examining the Effect of CSS Media Queries and the Viewport Meta Tag</span></span>

<span data-ttu-id="13a55-175">開啟*_layout.cshtml\\_Layout.cshtml*編輯器和檢視區外的註解中的檔案`<meta>`標記。</span><span class="sxs-lookup"><span data-stu-id="13a55-175">Open the *Views\Shared\\_Layout.cshtml* file in the editor and comment out the viewport `<meta>` tag.</span></span> <span data-ttu-id="13a55-176">下列標記顯示標記為註解。</span><span class="sxs-lookup"><span data-stu-id="13a55-176">The following markup shows the commented-out line.</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

<span data-ttu-id="13a55-177">開啟*MvcMobile\Content\Site.css*檔案在編輯器中，並將媒體查詢中的最大寬度變更為零像素。</span><span class="sxs-lookup"><span data-stu-id="13a55-177">Open the *MvcMobile\Content\Site.css* file in the editor and change the maximum width in the media query to zero pixels.</span></span> <span data-ttu-id="13a55-178">這會防止行動瀏覽器中使用 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="13a55-178">This will prevent the CSS rules from being used in mobile browsers.</span></span> <span data-ttu-id="13a55-179">下面顯示的已修改的媒體查詢：</span><span class="sxs-lookup"><span data-stu-id="13a55-179">The following line shows the modified media query:</span></span>

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

<span data-ttu-id="13a55-180">儲存您的變更，並瀏覽至會議中的應用程式的行動電話瀏覽器模擬器。</span><span class="sxs-lookup"><span data-stu-id="13a55-180">Save your changes and browse to the Conference application in a mobile browser emulator.</span></span> <span data-ttu-id="13a55-181">在下圖中的小字是移除檢視區的結果`<meta>`標記。</span><span class="sxs-lookup"><span data-stu-id="13a55-181">The tiny text in the following image is the result of removing the viewport `<meta>` tag.</span></span> <span data-ttu-id="13a55-182">沒有檢視區與`<meta>`標籤中，瀏覽器會縮小為預設檢視區寬度 (850 像素或更多的大部分行動瀏覽器。)</span><span class="sxs-lookup"><span data-stu-id="13a55-182">With no viewport `<meta>` tag, the browser is zooming out to the default viewport width (850 pixels or wider for most mobile browsers.)</span></span>

<span data-ttu-id="13a55-183">[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-183">[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)</span></span>

<span data-ttu-id="13a55-184">復原您的變更，請取消註解檢視區`<meta>`標記配置檔案中，並還原 850 像素的媒體查詢*Site.css*檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-184">Undo your changes — uncomment the viewport `<meta>` tag in the layout file and restore the media query to 850 pixels in the *Site.css* file.</span></span> <span data-ttu-id="13a55-185">儲存變更並重新整理行動瀏覽器，以確認行動設備友善顯示已經還原。</span><span class="sxs-lookup"><span data-stu-id="13a55-185">Save your changes and refresh the mobile browser to verify that the mobile-friendly display has been restored.</span></span>

<span data-ttu-id="13a55-186">檢視區`<meta>`標記和 CSS 的媒體查詢不特定的 ASP.NET MVC 4，且您可在任何 web 應用程式中使用這些功能。</span><span class="sxs-lookup"><span data-stu-id="13a55-186">The viewport `<meta>` tag and the CSS media query are not specific to ASP.NET MVC 4, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="13a55-187">但現在建您建立新的 ASP.NET MVC 4 專案時，所產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-187">But they are now built into the files that are generated when you create a new ASP.NET MVC 4 project.</span></span>

<span data-ttu-id="13a55-188">如需有關檢視區`<meta>`標記中，請參閱[故事的兩個檢視區，第二部分](http://www.quirksmode.org/mobile/viewports2.html)。</span><span class="sxs-lookup"><span data-stu-id="13a55-188">For more information about the viewport `<meta>` tag, see [A tale of two viewports — part two](http://www.quirksmode.org/mobile/viewports2.html).</span></span>

<span data-ttu-id="13a55-189">下一節中，您會看到如何提供行動裝置瀏覽器的特定檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-189">In the next section you'll see how to provide mobile-browser specific views.</span></span>

## <a name="overriding-views-layouts-and-partial-views"></a><span data-ttu-id="13a55-190">覆寫檢視、 配置和部分檢視</span><span class="sxs-lookup"><span data-stu-id="13a55-190">Overriding Views, Layouts, and Partial Views</span></span>

<span data-ttu-id="13a55-191">ASP.NET MVC 4 中重要的新功能是一個簡單的機制，可讓您覆寫任何檢視 （包括版面配置和部分檢視） 的行動瀏覽器，在一般情況下，針對個別的行動電話瀏覽器，或任何特定瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="13a55-191">A significant new feature in ASP.NET MVC 4 is a simple mechanism that lets you override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="13a55-192">若要提供特定的行動檢視，您可以複製檢視檔案並加入*。Mobile*的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="13a55-192">To provide a mobile-specific view, you can copy a view file and add *.Mobile* to the file name.</span></span> <span data-ttu-id="13a55-193">例如，若要建立行動*索引*檢視中，複製*Views\Home\Index.cshtml*至*Views\Home\Index.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-193">For example, to create a mobile *Index* view, copy *Views\Home\Index.cshtml* to *Views\Home\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="13a55-194">在本節中，您將建立行動特定配置檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-194">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="13a55-195">若要開始，複製*_layout.cshtml\\_Layout.cshtml*至*_layout.cshtml\\_Layout.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-195">To start, copy *Views\Shared\\_Layout.cshtml* to *Views\Shared\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="13a55-196">開啟 *\_Layout.Mobile.cshtml*並將變更從標題**MVC4 會議**至**會議 （行動裝置版）**。</span><span class="sxs-lookup"><span data-stu-id="13a55-196">Open *\_Layout.Mobile.cshtml* and change the title from **MVC4 Conference** to **Conference (Mobile)**.</span></span>

<span data-ttu-id="13a55-197">在每個`Html.ActionLink`呼叫，移除 瀏覽方式 」 中每個連結*ActionLink*。</span><span class="sxs-lookup"><span data-stu-id="13a55-197">In each `Html.ActionLink` call, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="13a55-198">下列程式碼會顯示行動配置檔的已完成的主體區段。</span><span class="sxs-lookup"><span data-stu-id="13a55-198">The following code shows the completed body section of the mobile layout file.</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

<span data-ttu-id="13a55-199">複製*Views\Home\AllTags.cshtml*檔案*Views\Home\AllTags.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-199">Copy the *Views\Home\AllTags.cshtml* file to *Views\Home\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="13a55-200">開啟新的檔案，並變更`<h2>`項目從 「 標記 」 到 「 標記 (M) 」:</span><span class="sxs-lookup"><span data-stu-id="13a55-200">Open the new file and change the `<h2>` element from "Tags" to "Tags (M)":</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

<span data-ttu-id="13a55-201">瀏覽至使用桌面瀏覽器，並使用行動電話瀏覽器模擬器中的 [標記] 頁面。</span><span class="sxs-lookup"><span data-stu-id="13a55-201">Browse to the tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="13a55-202">行動電話瀏覽器模擬器顯示兩個您所做的變更。</span><span class="sxs-lookup"><span data-stu-id="13a55-202">The mobile browser emulator shows the two changes you made.</span></span>

<span data-ttu-id="13a55-203">[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-203">[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)</span></span>

<span data-ttu-id="13a55-204">相反地，未變更桌面顯示。</span><span class="sxs-lookup"><span data-stu-id="13a55-204">In contrast, the desktop display has not changed.</span></span>

<span data-ttu-id="13a55-205">[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-205">[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)</span></span>

## <a name="browser-specific-views"></a><span data-ttu-id="13a55-206">瀏覽器的特定檢視</span><span class="sxs-lookup"><span data-stu-id="13a55-206">Browser-Specific Views</span></span>

<span data-ttu-id="13a55-207">除了特定的行動和桌面特定檢視中，您可以建立個別的瀏覽器的檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-207">In addition to mobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="13a55-208">例如，您可以建立專為 iPhone 瀏覽器的檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-208">For example, you can create views that are specifically for the iPhone browser.</span></span> <span data-ttu-id="13a55-209">在本節中，您將建立之 iPhone 瀏覽器和 iPhone 版本的版面配置*AllTags*檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-209">In this section, you'll create a layout for the iPhone browser and an iPhone version of the *AllTags* view.</span></span>

<span data-ttu-id="13a55-210">開啟*Global.asax*檔案，然後加入下列程式碼加入`Application_Start`方法。</span><span class="sxs-lookup"><span data-stu-id="13a55-210">Open the *Global.asax* file and add the following code to the `Application_Start` method.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

<span data-ttu-id="13a55-211">此程式碼會定義名為"iPhone"將會針對每個傳入要求符合新的顯示模式。</span><span class="sxs-lookup"><span data-stu-id="13a55-211">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="13a55-212">如果傳入要求符合您定義 （亦即，如果使用者代理程式包含字串"iPhone"） 的條件，ASP.NET MVC 會尋找其名稱中包含"iPhone"後置詞的檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-212">If the incoming request matches the condition you defined (that is, if the user agent contains the string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

<span data-ttu-id="13a55-213">在程式碼中，以滑鼠右鍵按一下`DefaultDisplayMode`，選擇**解決**，然後選擇  `using System.Web.WebPages;`。</span><span class="sxs-lookup"><span data-stu-id="13a55-213">In the code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="13a55-214">這將參考加入`System.Web.WebPages`命名空間，這是 where`DisplayModes`和`DefaultDisplayMode`類型定義。</span><span class="sxs-lookup"><span data-stu-id="13a55-214">This adds a reference to the `System.Web.WebPages` namespace, which is where the `DisplayModes` and `DefaultDisplayMode` types are defined.</span></span>

<span data-ttu-id="13a55-215">[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-215">[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)</span></span>

<span data-ttu-id="13a55-216">或者，您可以只手動新增下列這一行`using`檔的區段。</span><span class="sxs-lookup"><span data-stu-id="13a55-216">Alternatively, you can just manually add the following line to the `using` section of the file.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

<span data-ttu-id="13a55-217">完整內容*Global.asax*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="13a55-217">The complete contents of the *Global.asax* file is shown below.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

<span data-ttu-id="13a55-218">儲存變更。</span><span class="sxs-lookup"><span data-stu-id="13a55-218">Save the changes.</span></span> <span data-ttu-id="13a55-219">複製*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-219">Copy the *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file to *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="13a55-220">開啟新的檔案，然後變更`h1`標題從`Conference (Mobile)`至`Conference (iPhone)`。</span><span class="sxs-lookup"><span data-stu-id="13a55-220">Open the new file and then change the `h1` heading from `Conference (Mobile)` to `Conference (iPhone)`.</span></span>

<span data-ttu-id="13a55-221">複製*MvcMobile\Views\Home\AllTags.Mobile.cshtml*檔案*MvcMobile\Views\Home\AllTags.iPhone.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-221">Copy the *MvcMobile\Views\Home\AllTags.Mobile.cshtml* file to *MvcMobile\Views\Home\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="13a55-222">在新的檔案中，變更`<h2>`要"Tags (iPhone)"的項目從 「 標記 (M)"。</span><span class="sxs-lookup"><span data-stu-id="13a55-222">In the new file, change the `<h2>` element from "Tags (M)" to "Tags (iPhone)".</span></span>

<span data-ttu-id="13a55-223">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="13a55-223">Run the application.</span></span> <span data-ttu-id="13a55-224">執行行動瀏覽器模擬器，請確定其使用者代理程式設定為"iPhone"，並瀏覽至*AllTags*檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-224">Run a mobile browser emulator, make sure its user agent is set to "iPhone", and browse to the *AllTags* view.</span></span> <span data-ttu-id="13a55-225">下列螢幕擷取畫面顯示*AllTags*中檢視轉譯[Safari](http://www.apple.com/safari/download/)瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="13a55-225">The following screenshot shows the *AllTags* view rendered in the [Safari](http://www.apple.com/safari/download/) browser.</span></span> <span data-ttu-id="13a55-226">您可以下載 Windows 的 Safari[這裡](https://support.apple.com/kb/DL1531)。</span><span class="sxs-lookup"><span data-stu-id="13a55-226">You can download Safari for Windows [here](https://support.apple.com/kb/DL1531).</span></span>

<span data-ttu-id="13a55-227">[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-227">[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)</span></span>

<span data-ttu-id="13a55-228">本節中，我們已看到如何建立行動裝置的版面配置和檢視以及如何建立版面配置和特定裝置，例如 iPhone 的檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-228">In this section we've seen how to create mobile layouts and views and how to create layouts and views for specific devices such as the iPhone.</span></span> <span data-ttu-id="13a55-229">下一節中，您會看到如何運用 jQuery Mobile 的更多強大的行動檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-229">In the next section you'll see how to leverage jQuery Mobile for more compelling mobile views.</span></span>

## <a name="using-jquery-mobile"></a><span data-ttu-id="13a55-230">使用 jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="13a55-230">Using jQuery Mobile</span></span>

<span data-ttu-id="13a55-231">[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)程式庫會提供適用於所有主要的行動瀏覽器的使用者介面架構。</span><span class="sxs-lookup"><span data-stu-id="13a55-231">The [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) library provides a user interface framework that works on all the major mobile browsers.</span></span> <span data-ttu-id="13a55-232">jQuery Mobile 套用*漸進式增強功能*行動瀏覽器支援 CSS 和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="13a55-232">jQuery Mobile applies *progressive enhancement* to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="13a55-233">漸進式增強功能可讓所有瀏覽器同時允許更強大的瀏覽器及裝置有更豐富的顯示中顯示的網頁，基本的內容。</span><span class="sxs-lookup"><span data-stu-id="13a55-233">Progressive enhancement allows all browsers to display the basic content of a web page, while allowing more powerful browsers and devices to have a richer display.</span></span> <span data-ttu-id="13a55-234">隨附於 jQuery Mobile 的 JavaScript 和 CSS 檔案設定許多項目以符合行動瀏覽器，但不變更標記樣式。</span><span class="sxs-lookup"><span data-stu-id="13a55-234">The JavaScript and CSS files that are included with jQuery Mobile style many elements to fit mobile browsers without making any markup changes.</span></span>

<span data-ttu-id="13a55-235">本節中，您將安裝*jQuery.Mobile.MVC* NuGet 封裝，這會安裝 jQuery Mobile 和檢視切換程式 widget。</span><span class="sxs-lookup"><span data-stu-id="13a55-235">In this section you'll install the *jQuery.Mobile.MVC* NuGet package, which installs jQuery Mobile and a view-switcher widget.</span></span>

<span data-ttu-id="13a55-236">若要開始，刪除*共用\\_Layout.Mobile.cshtml*和*共用\\_Layout.iPhone.cshtml*您稍早建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-236">To start, delete the *Shared\\_Layout.Mobile.cshtml* and *Shared\\_Layout.iPhone.cshtml* files that you created earlier.</span></span>

<span data-ttu-id="13a55-237">重新命名*Views\Home\AllTags.Mobile.cshtml*和*Views\Home\AllTags.iPhone.cshtml*檔案到*Views\Home\AllTags.iPhone.cshtml.hide*和*Views\Home\AllTags.Mobile.cshtml.hide*。</span><span class="sxs-lookup"><span data-stu-id="13a55-237">Rename *Views\Home\AllTags.Mobile.cshtml* and *Views\Home\AllTags.iPhone.cshtml* files to *Views\Home\AllTags.iPhone.cshtml.hide* and *Views\Home\AllTags.Mobile.cshtml.hide*.</span></span> <span data-ttu-id="13a55-238">因為這些檔案不再有*.cshtml*擴充功能，它們不使用 ASP.NET MVC 執行階段呈現*AllTags*檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-238">Because the files no longer have a *.cshtml* extension, they won't be used by the ASP.NET MVC runtime to render the *AllTags* view.</span></span>

<span data-ttu-id="13a55-239">安裝*jQuery.Mobile.MVC*藉此 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="13a55-239">Install the *jQuery.Mobile.MVC* NuGet package by doing this:</span></span>

1. <span data-ttu-id="13a55-240">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="13a55-240">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

    <span data-ttu-id="13a55-241">[![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-241">[![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)</span></span>
2. <span data-ttu-id="13a55-242">在**Package Manager Console**，輸入`Install-Package jQuery.Mobile.MVC -version 1.0.0`</span><span class="sxs-lookup"><span data-stu-id="13a55-242">In the **Package Manager Console**, enter `Install-Package jQuery.Mobile.MVC -version 1.0.0`</span></span>

<span data-ttu-id="13a55-243">下圖顯示加入和變更 MvcMobile 專案 NuGet jQuery.Mobile.MVC 封裝的檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-243">The following image shows the files added and changed to the MvcMobile project by the NuGet jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="13a55-244">[新增] 的檔案名稱後面加入的檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-244">Files which are added have [add] appended after the file name.</span></span> <span data-ttu-id="13a55-245">映像不會顯示 GIF 和 PNG 檔案新增至*Content\images*資料夾。</span><span class="sxs-lookup"><span data-stu-id="13a55-245">The image does not show the GIF and PNG files added to the *Content\images* folder.</span></span>

![](aspnet-mvc-4-mobile-features/_static/image21.png)

<span data-ttu-id="13a55-246">JQuery.Mobile.MVC NuGet 封裝會安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="13a55-246">The jQuery.Mobile.MVC NuGet package installs the following:</span></span>

- <span data-ttu-id="13a55-247">*應用程式\_Start\BundleMobileConfig.cs*必要參考加入 jQuery JavaScript 和 CSS 檔案的檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-247">The *App\_Start\BundleMobileConfig.cs* file, which is needed to reference the jQuery JavaScript and CSS files added.</span></span> <span data-ttu-id="13a55-248">您必須遵循下列指示，並參考這個檔案中定義的行動配套。</span><span class="sxs-lookup"><span data-stu-id="13a55-248">You must follow the instructions below and reference the mobile bundle defined in this file.</span></span>
- <span data-ttu-id="13a55-249">jQuery Mobile CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-249">jQuery Mobile CSS files.</span></span>
- <span data-ttu-id="13a55-250">A`ViewSwitcher`控制器 widget (*Controllers\ViewSwitcherController.cs*)。</span><span class="sxs-lookup"><span data-stu-id="13a55-250">A `ViewSwitcher` controller widget (*Controllers\ViewSwitcherController.cs*).</span></span>
- <span data-ttu-id="13a55-251">jQuery Mobile JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-251">jQuery Mobile JavaScript files.</span></span>
- <span data-ttu-id="13a55-252">JQuery Mobile 樣式的配置檔案 (*_layout.cshtml\\_Layout.Mobile.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="13a55-252">A jQuery Mobile-styled layout file (*Views\Shared\\_Layout.Mobile.cshtml*).</span></span>
- <span data-ttu-id="13a55-253">檢視切換器的部分檢視*(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*)，提供從桌面檢視行動檢視，反之亦然切換每個頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-253">A view-switcher partial view *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) that provides a link at the top of each page to switch from desktop view to mobile view and vice versa.</span></span>
- <span data-ttu-id="13a55-254">數個*.png*和*.gif*影像檔*Content\images*資料夾。</span><span class="sxs-lookup"><span data-stu-id="13a55-254">Several*.png* and *.gif* image files in the *Content\images* folder.</span></span>

<span data-ttu-id="13a55-255">開啟*Global.asax*檔案，然後加入下列程式碼的最後一行`Application_Start`方法。</span><span class="sxs-lookup"><span data-stu-id="13a55-255">Open the *Global.asax* file and add the following code as the last line of the `Application_Start` method.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

<span data-ttu-id="13a55-256">下列程式碼顯示完整*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-256">The following code shows the complete *Global.asax* file.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> <span data-ttu-id="13a55-257">如果您使用 Internet Explorer 9，而且您沒有看到`BundleMobileConfig`以黃色反白顯示列上方，按一下[相容性檢視 按鈕](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![（關閉） 的相容性檢視 按鈕的圖片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（關閉） 的相容性檢視 按鈕的圖片")讓變更從大綱圖示 ie ![（關閉） 的相容性檢視 按鈕的圖片](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "（關閉） 的相容性檢視 按鈕的圖片")為純色![(on) 相容性檢視 按鈕的圖片](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(on) 相容性檢視 按鈕的圖片")。</span><span class="sxs-lookup"><span data-stu-id="13a55-257">If you are using Internet Explorer 9 and you don't see the `BundleMobileConfig` line above in yellow highlight, click the [Compatibility View button](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![Picture of the Compatibility View button (off)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Picture of the Compatibility View button (off)") in IE to make the icon change from an outline ![Picture of the Compatibility View button (off)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Picture of the Compatibility View button (off)") to a solid color ![Picture of the Compatibility View button (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Picture of the Compatibility View button (on)").</span></span> <span data-ttu-id="13a55-258">或者，您可以在 FireFox 或 Chrome 檢視本教學課程。</span><span class="sxs-lookup"><span data-stu-id="13a55-258">Alternatively you can view this tutorial in FireFox or Chrome.</span></span>


<span data-ttu-id="13a55-259">開啟*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案，然後加入下列標記直接之後`Html.Partial`呼叫：</span><span class="sxs-lookup"><span data-stu-id="13a55-259">Open the *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file and add the following markup directly after the `Html.Partial` call:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

<span data-ttu-id="13a55-260">完整*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="13a55-260">The complete *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file is shown below:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

<span data-ttu-id="13a55-261">建置應用程式，並在模擬器中您行動電話瀏覽器瀏覽至*AllTags*檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-261">Build the application, and in your mobile browser emulator browse to the *AllTags* view.</span></span> <span data-ttu-id="13a55-262">您看到下列訊息：</span><span class="sxs-lookup"><span data-stu-id="13a55-262">You see the following:</span></span>

<span data-ttu-id="13a55-263">[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-263">[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)</span></span>

> [!NOTE]
> <span data-ttu-id="13a55-264">您可以將行動裝置的特定程式碼的偵錯[設定的使用者代理字串](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)IE 或 Chrome 才能 iPhone，然後使用 F-12 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="13a55-264">You can debug the mobile specific code by [setting the user agent string](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) for IE or Chrome to iPhone and then using the F-12 developer tools.</span></span> <span data-ttu-id="13a55-265">如果未顯示您的行動瀏覽器**首頁**，**喇叭**，**標記**，和**日期**為按鈕的連結，jQuery Mobile 的參考指令碼和 CSS 檔案可能不是正確的。</span><span class="sxs-lookup"><span data-stu-id="13a55-265">If your mobile browser doesn't display the **Home**, **Speaker**, **Tag**, and **Date** links as buttons, the references to jQuery Mobile scripts and CSS files are probably not correct.</span></span>


<span data-ttu-id="13a55-266">除了樣式變更，您會看到**顯示行動檢視**以及可讓您從行動檢視切換到桌面檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-266">In addition to the style changes, you see **Displaying mobile view** and a link that lets you switch from mobile view to desktop view.</span></span> <span data-ttu-id="13a55-267">選擇**桌面檢視**連結和桌面檢視會顯示。</span><span class="sxs-lookup"><span data-stu-id="13a55-267">Choose the **Desktop view** link, and the desktop view is displayed.</span></span>

<span data-ttu-id="13a55-268">[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-268">[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)</span></span>

<span data-ttu-id="13a55-269">桌面檢視不會提供方法來直接瀏覽回到行動檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-269">The desktop view doesn't provide a way to directly navigate back to the mobile view.</span></span> <span data-ttu-id="13a55-270">您可以立即修正。</span><span class="sxs-lookup"><span data-stu-id="13a55-270">You'll fix that now.</span></span> <span data-ttu-id="13a55-271">開啟*_layout.cshtml\\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-271">Open the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="13a55-272">頁面的下方`body`項目，加入下列程式碼，會呈現檢視切換程式 widget:</span><span class="sxs-lookup"><span data-stu-id="13a55-272">Just under the page `body` element, add the following code, which renders the view-switcher widget:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

<span data-ttu-id="13a55-273">重新整理*AllTags*行動瀏覽器中的檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-273">Refresh the *AllTags* view in the mobile browser.</span></span> <span data-ttu-id="13a55-274">您現在可以桌面和行動裝置版的檢視之間巡覽。</span><span class="sxs-lookup"><span data-stu-id="13a55-274">You can now navigate between desktop and mobile views.</span></span>

<span data-ttu-id="13a55-275">[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-275">[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)</span></span>

> [!NOTE]
> <span data-ttu-id="13a55-276">偵錯附註： 您可以將下列程式碼加入 _layout.cshtml 結尾\\_ViewSwitcher.cshtml 協助偵錯檢視，當使用瀏覽器使用者代理字串設定為行動裝置。</span><span class="sxs-lookup"><span data-stu-id="13a55-276">Debug note: You can add the following code to the end of the Views\Shared\\_ViewSwitcher.cshtml to help debug views when using a browser the user agent string set to a mobile device.</span></span>
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  <span data-ttu-id="13a55-277">並加入下列的標題， *_layout.cshtml\\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="13a55-277">and adding the following heading to the *Views\Shared\\_Layout.cshtml* file.</span></span>  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


<span data-ttu-id="13a55-278">瀏覽至*AllTags*桌面瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="13a55-278">Browse to the *AllTags* page in a desktop browser.</span></span> <span data-ttu-id="13a55-279">檢視切換程式小工具不會顯示在桌面瀏覽器，因為它只加入至行動裝置的版面配置頁面。</span><span class="sxs-lookup"><span data-stu-id="13a55-279">The view-switcher widget is not displayed in a desktop browser because it's added only to the mobile layout page.</span></span> <span data-ttu-id="13a55-280">稍後在教學課程中，您會看到您，如何將檢視切換程式 widget 加入桌面檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-280">Later in the tutorial you'll see how you can add the view-switcher widget to the desktop view.</span></span>

<span data-ttu-id="13a55-281">[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-281">[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)</span></span>

## <a name="improving-the-speakers-list"></a><span data-ttu-id="13a55-282">提高喇叭清單</span><span class="sxs-lookup"><span data-stu-id="13a55-282">Improving the Speakers List</span></span>

<span data-ttu-id="13a55-283">在行動裝置瀏覽器中，選取**喇叭**連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-283">In the mobile browser, select the **Speakers** link.</span></span> <span data-ttu-id="13a55-284">因為沒有行動檢視 (*AllSpeakers.Mobile.cshtml*)，顯示預設喇叭 (*AllSpeakers.cshtml*) 使用行動裝置的版面配置檢視呈現 ( *\_Layout.Mobile.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="13a55-284">Because there's no mobile view(*AllSpeakers.Mobile.cshtml*), the default speakers display (*AllSpeakers.cshtml*) is rendered using the mobile layout view (*\_Layout.Mobile.cshtml*).</span></span>

<span data-ttu-id="13a55-285">[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-285">[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)</span></span>

<span data-ttu-id="13a55-286">您可以藉由設定全域停用預設的 （非行動電話） 檢視行動配置內呈現`RequireConsistentDisplayMode`至`true`中*檢視\\_viewstart.vbhtml*檔案，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="13a55-286">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in the *Views\\_ViewStart.cshtml* file, like this:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

<span data-ttu-id="13a55-287">當`RequireConsistentDisplayMode`設`true`，行動裝置的版面配置 (*\_Layout.Mobile.cshtml*) 只適用於行動檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-287">When `RequireConsistentDisplayMode` is set to `true`, the mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views.</span></span> <span data-ttu-id="13a55-288">(亦即，檢視檔案是表單的 ***ViewName**。Mobile.cshtml*。)要設定`RequireConsistentDisplayMode`至`true`如果您的行動配置效果不佳非行動檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-288">(That is, the view file is of the form ***ViewName**.Mobile.cshtml*.) You might want to set `RequireConsistentDisplayMode` to `true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="13a55-289">螢幕擷取畫面所示方式*喇叭*頁面轉譯時`RequireConsistentDisplayMode`設`true`。</span><span class="sxs-lookup"><span data-stu-id="13a55-289">The screenshot below shows how the *Speakers* page renders when `RequireConsistentDisplayMode` is set to `true`.</span></span>

<span data-ttu-id="13a55-290">[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-290">[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)</span></span>

<span data-ttu-id="13a55-291">您可以藉由設定停用在檢視中的一致的顯示模式`RequireConsistentDisplayMode`至`false`檢視檔案中。</span><span class="sxs-lookup"><span data-stu-id="13a55-291">You can disable consistent display mode in a view by setting `RequireConsistentDisplayMode` to `false` in the view file.</span></span> <span data-ttu-id="13a55-292">中的下列標記*Views\Home\AllSpeakers.cshtml*檔案集`RequireConsistentDisplayMode`至`false`:</span><span class="sxs-lookup"><span data-stu-id="13a55-292">The following markup in the *Views\Home\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` to `false`:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a><span data-ttu-id="13a55-293">建立行動喇叭檢視</span><span class="sxs-lookup"><span data-stu-id="13a55-293">Creating a Mobile Speakers View</span></span>

<span data-ttu-id="13a55-294">如您剛才所見，*喇叭* 檢視來得容易讀懂，但連結都很小而且很難進行行動裝置上點選。</span><span class="sxs-lookup"><span data-stu-id="13a55-294">As you just saw, the *Speakers* view is readable, but the links are small and are difficult to tap on a mobile device.</span></span> <span data-ttu-id="13a55-295">在本節中，您將建立行動裝置特定*喇叭*檢視看起來像現代的行動應用程式，它會顯示大型、 簡單點選連結，並包含在搜尋方塊，以快速找出喇叭。</span><span class="sxs-lookup"><span data-stu-id="13a55-295">In this section, you'll create a mobile-specific *Speakers* view that looks like a modern mobile application — it displays large, easy-to-tap links and contains a search box to quickly find speakers.</span></span>

<span data-ttu-id="13a55-296">複製*AllSpeakers.cshtml*至*AllSpeakers.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-296">Copy *AllSpeakers.cshtml* to *AllSpeakers.Mobile.cshtml*.</span></span> <span data-ttu-id="13a55-297">開啟*AllSpeakers.Mobile.cshtml*檔案，並移除`<h2>`標題項目。</span><span class="sxs-lookup"><span data-stu-id="13a55-297">Open the *AllSpeakers.Mobile.cshtml* file and remove the `<h2>` heading element.</span></span>

<span data-ttu-id="13a55-298">在`<ul>`標記中加入`data-role`屬性，並將其值設定為`listview`。</span><span class="sxs-lookup"><span data-stu-id="13a55-298">In the `<ul>` tag, add the `data-role` attribute and set its value to `listview`.</span></span> <span data-ttu-id="13a55-299">就像其他[`data-*`屬性](http://html5doctor.com/html5-custom-data-attributes/)，`data-role="listview"`點選時，對大型清單項目更容易。</span><span class="sxs-lookup"><span data-stu-id="13a55-299">Like other [`data-*` attributes](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` makes the large list items easier to tap.</span></span> <span data-ttu-id="13a55-300">這是完整的標記外觀如下：</span><span class="sxs-lookup"><span data-stu-id="13a55-300">This is what the completed markup looks like:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

<span data-ttu-id="13a55-301">重新整理行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="13a55-301">Refresh the mobile browser.</span></span> <span data-ttu-id="13a55-302">更新的檢視看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="13a55-302">The updated view looks like this:</span></span>

<span data-ttu-id="13a55-303">[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-303">[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)</span></span>

<span data-ttu-id="13a55-304">雖然行動檢視已經改善，很難瀏覽的喇叭長的清單。</span><span class="sxs-lookup"><span data-stu-id="13a55-304">Although the mobile view has improved, it's difficult to navigate the long list of speakers.</span></span> <span data-ttu-id="13a55-305">若要解決此問題，在`<ul>`標記中加入`data-filter`屬性，並將它設定為`true`。</span><span class="sxs-lookup"><span data-stu-id="13a55-305">To fix this, in the `<ul>` tag, add the `data-filter` attribute and set it to `true`.</span></span> <span data-ttu-id="13a55-306">程式碼所示`ul`標記。</span><span class="sxs-lookup"><span data-stu-id="13a55-306">The code below shows the `ul` markup.</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

<span data-ttu-id="13a55-307">下圖顯示在所產生的頁面頂端的 [搜尋] 篩選器方塊`data-filter`屬性。</span><span class="sxs-lookup"><span data-stu-id="13a55-307">The following image shows the search filter box at the top of the page that results from the `data-filter` attribute.</span></span>

<span data-ttu-id="13a55-308">[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-308">[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)</span></span>

<span data-ttu-id="13a55-309">當您在 [搜尋] 方塊中輸入每個字母，jQuery Mobile 來篩選所顯示的清單中下圖所示。</span><span class="sxs-lookup"><span data-stu-id="13a55-309">As you type each letter in the search box, jQuery Mobile filters the displayed list as shown in the image below.</span></span>

<span data-ttu-id="13a55-310">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-310">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)</span></span>

## <a name="improving-the-tags-list"></a><span data-ttu-id="13a55-311">改善標籤清單</span><span class="sxs-lookup"><span data-stu-id="13a55-311">Improving the Tags List</span></span>

<span data-ttu-id="13a55-312">喜歡預設值*喇叭* 檢視中，*標記* 檢視來得容易讀懂，但連結是小型且難以行動裝置上點選。</span><span class="sxs-lookup"><span data-stu-id="13a55-312">Like the default *Speakers* view, the *Tags* view is readable, but the links are small and difficult to tap on a mobile device.</span></span> <span data-ttu-id="13a55-313">在本節中，您將會修正*標記*檢視您固定的相同方式*喇叭*檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-313">In this section, you'll fix the *Tags* view the same way you fixed the *Speakers* view.</span></span>

<span data-ttu-id="13a55-314">移除&quot;隱藏&quot;尾碼*Views\Home\AllTags.Mobile.cshtml.hide*檔案讓名稱*Views\Home\AllTags.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-314">Remove the &quot;hide&quot; suffix to the *Views\Home\AllTags.Mobile.cshtml.hide* file so the name is *Views\Home\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="13a55-315">開啟 重新命名的檔案，然後移除`<h2>`項目。</span><span class="sxs-lookup"><span data-stu-id="13a55-315">Open the renamed file and remove the `<h2>` element.</span></span>

<span data-ttu-id="13a55-316">新增`data-role`和`data-filter`屬性至`<ul>`標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="13a55-316">Add the `data-role` and `data-filter` attributes to the `<ul>` tag, as shown here:</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

<span data-ttu-id="13a55-317">下圖顯示 [標記] 頁面上篩選的字母`J`。</span><span class="sxs-lookup"><span data-stu-id="13a55-317">The image below shows the tags page filtering on the letter `J`.</span></span>

<span data-ttu-id="13a55-318">[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-318">[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)</span></span>

## <a name="improving-the-dates-list"></a><span data-ttu-id="13a55-319">改善 [日期] 清單</span><span class="sxs-lookup"><span data-stu-id="13a55-319">Improving the Dates List</span></span>

<span data-ttu-id="13a55-320">您可以改善*日期*檢視您的改善，例如*喇叭*和*標記*檢視，以便更輕鬆地在行動裝置上使用。</span><span class="sxs-lookup"><span data-stu-id="13a55-320">You can improve the *Dates* view like you improved the *Speakers* and *Tags* views, so that it's easier to use on a mobile device.</span></span>

<span data-ttu-id="13a55-321">複製*Views\Home\AllDates.cshtml*檔案*Views\Home\AllDates.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="13a55-321">Copy the *Views\Home\AllDates.cshtml* file to *Views\Home\AllDates.Mobile.cshtml*.</span></span> <span data-ttu-id="13a55-322">開啟新的檔案，然後移除`<h2>`項目。</span><span class="sxs-lookup"><span data-stu-id="13a55-322">Open the new file and remove the `<h2>` element.</span></span>

<span data-ttu-id="13a55-323">新增`data-role="listview"`至`<ul>`標記，像這樣：</span><span class="sxs-lookup"><span data-stu-id="13a55-323">Add `data-role="listview"` to the `<ul>` tag, like this:</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

<span data-ttu-id="13a55-324">下圖顯示當**日期**頁面看起來像與`data-role`就地屬性。</span><span class="sxs-lookup"><span data-stu-id="13a55-324">The image below shows what the **Date** page looks like with the `data-role` attribute in place.</span></span>

<span data-ttu-id="13a55-325">[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)的內容取代*Views\Home\AllDates.Mobile.cshtml*以下列程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="13a55-325">[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Replace the contents of the *Views\Home\AllDates.Mobile.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

<span data-ttu-id="13a55-326">此程式碼群組依日期的所有工作階段。</span><span class="sxs-lookup"><span data-stu-id="13a55-326">This code groups all sessions by days.</span></span> <span data-ttu-id="13a55-327">建立清單分隔每個新的一天，並列出在分割線的每一天的所有工作階段。</span><span class="sxs-lookup"><span data-stu-id="13a55-327">It creates a list divider for each new day, and it lists all the sessions for each day under a divider.</span></span> <span data-ttu-id="13a55-328">以下是什麼樣子執行此程式碼：</span><span class="sxs-lookup"><span data-stu-id="13a55-328">Here's what it looks like when this code runs:</span></span>

<span data-ttu-id="13a55-329">[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-329">[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)</span></span>

## <a name="improving-the-sessionstable-view"></a><span data-ttu-id="13a55-330">改善 SessionsTable 檢視</span><span class="sxs-lookup"><span data-stu-id="13a55-330">Improving the SessionsTable View</span></span>

<span data-ttu-id="13a55-331">在本節中，您將建立工作階段特定的行動的檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-331">In this section, you'll create a mobile-specific view of sessions.</span></span> <span data-ttu-id="13a55-332">我們所做的變更會比在檢視中，我們建立了更廣泛。</span><span class="sxs-lookup"><span data-stu-id="13a55-332">The changes we make will be more extensive than in other views we have created.</span></span>

<span data-ttu-id="13a55-333">在行動裝置瀏覽器中，點選**喇叭**按鈕，然後輸入`Sc`[搜尋] 方塊中。</span><span class="sxs-lookup"><span data-stu-id="13a55-333">In the mobile browser, tap the **Speaker** button, then enter `Sc` in the search box.</span></span>

<span data-ttu-id="13a55-334">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-334">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)</span></span>

<span data-ttu-id="13a55-335">點選**Scott Hanselman**連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-335">Tap the **Scott Hanselman** link.</span></span>

<span data-ttu-id="13a55-336">[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-336">[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)</span></span>

<span data-ttu-id="13a55-337">如您所見，顯示會很難讀取行動瀏覽器上。</span><span class="sxs-lookup"><span data-stu-id="13a55-337">As you can see, the display is difficult to read on a mobile browser.</span></span> <span data-ttu-id="13a55-338">日期資料行是難以讀取的標記 資料行超出檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-338">The date column is hard to read and the tags column is out of the view.</span></span> <span data-ttu-id="13a55-339">若要修正此問題，將複製*Views\Home\SessionsTable.cshtml*至*Views\Home\SessionsTable.Mobile.cshtml*，並以下列程式碼取代檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="13a55-339">To fix this, copy *Views\Home\SessionsTable.cshtml* to *Views\Home\SessionsTable.Mobile.cshtml*, and then replace the contents of the file with the following code:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

<span data-ttu-id="13a55-340">程式碼移除聊天室和標記資料行，並格式化標題、 演講者和日期，使這項資訊可在行動裝置瀏覽器上讀取。</span><span class="sxs-lookup"><span data-stu-id="13a55-340">The code removes the room and tags columns, and formats the title, speaker, and date vertically, so that all this information is readable on a mobile browser.</span></span> <span data-ttu-id="13a55-341">下圖會反映程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="13a55-341">The image below reflects the code changes.</span></span>

<span data-ttu-id="13a55-342">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-342">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)</span></span>

## <a name="improving-the-sessionbycode-view"></a><span data-ttu-id="13a55-343">改善 SessionByCode 檢視</span><span class="sxs-lookup"><span data-stu-id="13a55-343">Improving the SessionByCode View</span></span>

<span data-ttu-id="13a55-344">最後，您將建立的行動裝置的特定檢視*SessionByCode*檢視。</span><span class="sxs-lookup"><span data-stu-id="13a55-344">Finally, you'll create a mobile-specific view of the *SessionByCode* view.</span></span> <span data-ttu-id="13a55-345">在行動裝置瀏覽器中，點選**喇叭**按鈕，然後輸入`Sc`[搜尋] 方塊中。</span><span class="sxs-lookup"><span data-stu-id="13a55-345">In the mobile browser, tap the **Speaker** button, then enter `Sc` in the search box.</span></span>

<span data-ttu-id="13a55-346">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-346">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)</span></span>

<span data-ttu-id="13a55-347">點選**Scott Hanselman**連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-347">Tap the **Scott Hanselman** link.</span></span> <span data-ttu-id="13a55-348">Scott Hanselman 工作階段會顯示。</span><span class="sxs-lookup"><span data-stu-id="13a55-348">Scott Hanselman's sessions are displayed.</span></span>

<span data-ttu-id="13a55-349">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-349">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)</span></span>

<span data-ttu-id="13a55-350">選擇**概觀 MS Web 堆疊的愛**連結。</span><span class="sxs-lookup"><span data-stu-id="13a55-350">Choose the **An Overview of the MS Web Stack of Love** link.</span></span>

<span data-ttu-id="13a55-351">[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-351">[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)</span></span>

<span data-ttu-id="13a55-352">預設的桌面檢視沒有問題，但可加以改進。</span><span class="sxs-lookup"><span data-stu-id="13a55-352">The default desktop view is fine, but you can improve it.</span></span>

<span data-ttu-id="13a55-353">複製*Views\Home\SessionByCode.cshtml*至*Views\Home\SessionByCode.Mobile.cshtml*取代的內容和*Views\Home\SessionByCode.Mobile.cshtml*檔案以下列標記：</span><span class="sxs-lookup"><span data-stu-id="13a55-353">Copy the *Views\Home\SessionByCode.cshtml* to *Views\Home\SessionByCode.Mobile.cshtml* and replace the contents of the *Views\Home\SessionByCode.Mobile.cshtml* file with the following markup:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

<span data-ttu-id="13a55-354">使用新的標記`data-role`屬性，以改善檢視的配置。</span><span class="sxs-lookup"><span data-stu-id="13a55-354">The new markup uses the `data-role` attribute to improve the layout of the view.</span></span>

<span data-ttu-id="13a55-355">重新整理行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="13a55-355">Refresh the mobile browser.</span></span> <span data-ttu-id="13a55-356">下圖會反映您剛建立的程式碼變更：</span><span class="sxs-lookup"><span data-stu-id="13a55-356">The following image reflects the code changes that you just made:</span></span>

<span data-ttu-id="13a55-357">[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)</span><span class="sxs-lookup"><span data-stu-id="13a55-357">[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)</span></span>

## <a name="wrapup-and-review"></a><span data-ttu-id="13a55-358">簡便和檢閱</span><span class="sxs-lookup"><span data-stu-id="13a55-358">Wrapup and Review</span></span>

<span data-ttu-id="13a55-359">本教學課程已引進新的行動裝置功能的 ASP.NET MVC 4 Developer Preview。</span><span class="sxs-lookup"><span data-stu-id="13a55-359">This tutorial has introduced the new mobile features of ASP.NET MVC 4 Developer Preview.</span></span> <span data-ttu-id="13a55-360">行動裝置的功能包括：</span><span class="sxs-lookup"><span data-stu-id="13a55-360">The mobile features include:</span></span>

- <span data-ttu-id="13a55-361">覆寫版面配置、 檢視和部分檢視，全域和個別的檢視能力。</span><span class="sxs-lookup"><span data-stu-id="13a55-361">The ability to override layout, views, and partial views, both globally and for an individual view.</span></span>
- <span data-ttu-id="13a55-362">控制版面配置，以及部分覆寫強制使用`RequireConsistentDisplayMode`屬性。</span><span class="sxs-lookup"><span data-stu-id="13a55-362">Control over layout and partial override enforcement using the `RequireConsistentDisplayMode` property.</span></span>
- <span data-ttu-id="13a55-363">行動應用程式檢視切換程式 widget 檢視比也可以在桌面檢視中顯示。</span><span class="sxs-lookup"><span data-stu-id="13a55-363">A view-switcher widget for mobile views than can also be displayed in desktop views.</span></span>
- <span data-ttu-id="13a55-364">支援特定瀏覽器，例如 iPhone 瀏覽器支援。</span><span class="sxs-lookup"><span data-stu-id="13a55-364">Support for supporting specific browsers, such as the iPhone browser.</span></span>

## <a name="see-also"></a><span data-ttu-id="13a55-365">請參閱</span><span class="sxs-lookup"><span data-stu-id="13a55-365">See Also</span></span>

- <span data-ttu-id="13a55-366">[jQuery Mobile](http://jquerymobile.com)站台。</span><span class="sxs-lookup"><span data-stu-id="13a55-366">[jQuery Mobile](http://jquerymobile.com) site.</span></span>
- [<span data-ttu-id="13a55-367">jQuery Mobile 概觀</span><span class="sxs-lookup"><span data-stu-id="13a55-367">jQuery Mobile Overview</span></span>](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [<span data-ttu-id="13a55-368">W3C 建議行動裝置 Web 應用程式最佳作法</span><span class="sxs-lookup"><span data-stu-id="13a55-368">W3C Recommendation Mobile Web Application Best Practices</span></span>](http://www.w3.org/TR/mwabp/)
- [<span data-ttu-id="13a55-369">W3C Candidate Recommendation 的媒體查詢</span><span class="sxs-lookup"><span data-stu-id="13a55-369">W3C Candidate Recommendation for media queries</span></span>](http://www.w3.org/TR/css3-mediaqueries/)
