---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: 開始使用 |Microsoft Docs
author: Rick-Anderson
description: WebMatrix 不再建議使用整合式的開發環境適用於 ASP.NET 網頁。 使用 Visual Studio 或 Visual Studio 程式碼。 本指南...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 79f76410e091b137e2792127b0a42524270761e8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021491"
---
<a name="getting-started"></a><span data-ttu-id="11cc1-105">快速入門</span><span class="sxs-lookup"><span data-stu-id="11cc1-105">Getting Started</span></span>
====================
<span data-ttu-id="11cc1-106">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="11cc1-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > <span data-ttu-id="11cc1-107">WebMatrix 不再建議使用整合式的開發環境適用於 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="11cc1-107">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="11cc1-108">使用[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)或是[Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-108">Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
> 
> 
> <span data-ttu-id="11cc1-109">本指南及應用程式可讓您的 ASP.NET Web Pages （版本 2 或更新版本） 的概觀和 Razor 語法，來建立動態網站的輕量型架構。</span><span class="sxs-lookup"><span data-stu-id="11cc1-109">This guidance and application gives you an overview of ASP.NET Web Pages (version 2 or later) and Razor syntax, a lightweight framework for creating dynamic websites.</span></span> <span data-ttu-id="11cc1-110">它也引入了 WebMatrix，用於建立網頁和網站的工具。</span><span class="sxs-lookup"><span data-stu-id="11cc1-110">It also introduces WebMatrix, a tool for creating pages and sites.</span></span>
> 
> <span data-ttu-id="11cc1-111">**層級**： 新增至 ASP.NET Web 網頁。</span><span class="sxs-lookup"><span data-stu-id="11cc1-111">**Level**: New to ASP.NET Web Pages.</span></span>  
> <span data-ttu-id="11cc1-112">**假設的技能**: HTML、 基本的階層式樣式表 (CSS)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-112">**Skills assumed**: HTML, basic cascading style sheets (CSS).</span></span>
> 
> <span data-ttu-id="11cc1-113">您將學到什麼集的第一個教學課程中：</span><span class="sxs-lookup"><span data-stu-id="11cc1-113">What you'll learn in the first tutorial of the set:</span></span>
> 
> - <span data-ttu-id="11cc1-114">說明 ASP.NET Web Pages 技術和用途。</span><span class="sxs-lookup"><span data-stu-id="11cc1-114">What ASP.NET Web Pages technology is and what it's for.</span></span>
> - <span data-ttu-id="11cc1-115">WebMatrix 是什麼。</span><span class="sxs-lookup"><span data-stu-id="11cc1-115">What WebMatrix is.</span></span>
> - <span data-ttu-id="11cc1-116">如何安裝所有項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-116">How to install everything.</span></span>
> - <span data-ttu-id="11cc1-117">如何使用 WebMatrix 建立網站。</span><span class="sxs-lookup"><span data-stu-id="11cc1-117">How to create a website by using WebMatrix.</span></span>
>   
> 
> <span data-ttu-id="11cc1-118">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="11cc1-118">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="11cc1-119">Microsoft Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="11cc1-119">Microsoft Web Platform Installer.</span></span>
> - <span data-ttu-id="11cc1-120">WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="11cc1-120">WebMatrix.</span></span>
> - <span data-ttu-id="11cc1-121">*.cshtml*頁面</span><span class="sxs-lookup"><span data-stu-id="11cc1-121">*.cshtml* pages</span></span>
>   
> 
> <span data-ttu-id="11cc1-122">Mike 主教最初撰寫本教學課程。</span><span class="sxs-lookup"><span data-stu-id="11cc1-122">Mike Pope originally wrote this tutorial.</span></span> <span data-ttu-id="11cc1-123">Tom FitzMacken 會更新它適用於 Microsoft WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="11cc1-123">Tom FitzMacken updated it for Microsoft WebMatrix 3.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="11cc1-124">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="11cc1-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="11cc1-125">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="11cc1-125">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="11cc1-126">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="11cc1-126">WebMatrix 3</span></span>


## <a name="what-should-you-know"></a><span data-ttu-id="11cc1-127">您應該知道什麼？</span><span class="sxs-lookup"><span data-stu-id="11cc1-127">What Should You Know?</span></span>

<span data-ttu-id="11cc1-128">我們假設您已熟悉：</span><span class="sxs-lookup"><span data-stu-id="11cc1-128">We're assuming that you're familiar with:</span></span>

- <span data-ttu-id="11cc1-129">**HTML**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-129">**HTML**.</span></span> <span data-ttu-id="11cc1-130">需要沒有深入的專業知識。</span><span class="sxs-lookup"><span data-stu-id="11cc1-130">No in-depth expertise is required.</span></span> <span data-ttu-id="11cc1-131">我們不會說明的 HTML，但我們也不會使用複雜的任何項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-131">We won't explain HTML, but we also don't use anything complex.</span></span> <span data-ttu-id="11cc1-132">我們將提供我們認為它們很好用的 HTML 教學課程的連結。</span><span class="sxs-lookup"><span data-stu-id="11cc1-132">We'll provide links to HTML tutorials where we think they're useful.</span></span>
- <span data-ttu-id="11cc1-133">**階層式樣式表 (CSS)**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-133">**Cascading style sheets (CSS)**.</span></span> <span data-ttu-id="11cc1-134">相同的 HTML。</span><span class="sxs-lookup"><span data-stu-id="11cc1-134">Same as with HTML.</span></span>
- <span data-ttu-id="11cc1-135">**基本資料庫概念**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-135">**Basic database ideas**.</span></span> <span data-ttu-id="11cc1-136">如果您已試算表用於資料和排序和篩選的資料，是層級的專業知識，我們通常假設本教學課程組。</span><span class="sxs-lookup"><span data-stu-id="11cc1-136">If you've used a spreadsheet for data and sorted and filtered the data, that's the level of expertise we're generally assuming for this tutorial set.</span></span>

<span data-ttu-id="11cc1-137">我們也假設您想要學習基本的程式設計。</span><span class="sxs-lookup"><span data-stu-id="11cc1-137">We're also assuming that you're interested in learning basic programming.</span></span> <span data-ttu-id="11cc1-138">ASP.NET Web Pages 使用稱為 C# 的程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="11cc1-138">ASP.NET Web Pages use a programming language called C#.</span></span> <span data-ttu-id="11cc1-139">您不需要有在程式設計中，只在它的感興趣的任何背景。</span><span class="sxs-lookup"><span data-stu-id="11cc1-139">You don't have to have any background in programming, just an interest in it.</span></span> <span data-ttu-id="11cc1-140">如果您曾經撰寫任何 JavaScript 在網頁上之前，您有足夠的背景。</span><span class="sxs-lookup"><span data-stu-id="11cc1-140">If you've ever written any JavaScript in a web page before, you've got plenty of background.</span></span>

<span data-ttu-id="11cc1-141">雖然我們將新的程式設計人員，了解，請注意，如果您熟悉的程式設計，您可能會發現本教學課程一開始就設定移動緩慢。</span><span class="sxs-lookup"><span data-stu-id="11cc1-141">Note that if you are familiar with programming, you might find that this tutorial set initially moves slowly while we bring new programmers up to speed.</span></span> <span data-ttu-id="11cc1-142">隨著我們越來越過去的第一個的幾個教學課程，不過，會有較基本的程式設計，以說明，且項目時，會將上，在更快速的剪輯。</span><span class="sxs-lookup"><span data-stu-id="11cc1-142">As we get past the first few tutorials, though, there will be less basic programming to explain and things will move along at a faster clip.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="11cc1-143">您需要什麼？</span><span class="sxs-lookup"><span data-stu-id="11cc1-143">What Do You Need?</span></span>

<span data-ttu-id="11cc1-144">以下是您需要的項目：</span><span class="sxs-lookup"><span data-stu-id="11cc1-144">Here's what you'll need:</span></span>

- <span data-ttu-id="11cc1-145">執行 Windows 8、 Windows 7、 Windows Server 2008 或 Windows Server 2012 的電腦。</span><span class="sxs-lookup"><span data-stu-id="11cc1-145">A computer that is running Windows 8, Windows 7, Windows Server 2008, or Windows Server 2012.</span></span>
- <span data-ttu-id="11cc1-146">即時的網際網路連線。</span><span class="sxs-lookup"><span data-stu-id="11cc1-146">A live internet connection.</span></span>
- <span data-ttu-id="11cc1-147">（所需的安裝程序） 的系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="11cc1-147">Administrator privileges (required for the installation process).</span></span>

## <a name="what-is-aspnet-web-pages"></a><span data-ttu-id="11cc1-148">ASP.NET Web Pages 是什麼？</span><span class="sxs-lookup"><span data-stu-id="11cc1-148">What Is ASP.NET Web Pages?</span></span>

<span data-ttu-id="11cc1-149">ASP.NET Web Pages 是一種架構，可用來建立動態網頁。</span><span class="sxs-lookup"><span data-stu-id="11cc1-149">ASP.NET Web Pages is a framework that you can use to create dynamic web pages.</span></span> <span data-ttu-id="11cc1-150">簡單的 HTML 網頁是靜態的;其內容取決於固定在頁面的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="11cc1-150">A simple HTML web page is static; its content is determined by the fixed HTML markup that's in the page.</span></span> <span data-ttu-id="11cc1-151">建立以 ASP.NET Web Pages 等動態頁面可讓您使用程式碼，在作業中，建立網頁內容。</span><span class="sxs-lookup"><span data-stu-id="11cc1-151">Dynamic pages like those you create with ASP.NET Web Pages let you create the page content on the fly, by using code.</span></span>

<span data-ttu-id="11cc1-152">動態頁面可讓您執行各種作業。</span><span class="sxs-lookup"><span data-stu-id="11cc1-152">Dynamic pages let you do all sorts of things.</span></span> <span data-ttu-id="11cc1-153">您可以使用的表單，要求使用者輸入，然後變更  頁面上所顯示的項目，或它的樣子。</span><span class="sxs-lookup"><span data-stu-id="11cc1-153">You can ask a user for input by using a form and then change what the page displays or how it looks.</span></span> <span data-ttu-id="11cc1-154">您可以從使用者取得資訊，將它儲存在資料庫中，則稍後列出。</span><span class="sxs-lookup"><span data-stu-id="11cc1-154">You can take information from a user, save it in a database, and then list it later.</span></span> <span data-ttu-id="11cc1-155">您可以從您的網站傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="11cc1-155">You can send email from your site.</span></span> <span data-ttu-id="11cc1-156">您可以在網站 （例如，對應服務） 上其他服務進行互動，並產生頁面，將那些來源的資訊整合。</span><span class="sxs-lookup"><span data-stu-id="11cc1-156">You can interact with other services on the web (for example, a mapping service) and produce pages that integrate information from those sources.</span></span>

## <a name="what-is-webmatrix"></a><span data-ttu-id="11cc1-157">WebMatrix 是什麼？</span><span class="sxs-lookup"><span data-stu-id="11cc1-157">What is WebMatrix?</span></span>

<span data-ttu-id="11cc1-158">WebMatrix 是一種工具，整合網頁編輯器、 資料庫公用程式、 web 伺服器以進行測試頁面，以及可將您的網站發佈到網際網路的功能。</span><span class="sxs-lookup"><span data-stu-id="11cc1-158">WebMatrix is a tool that integrates a web page editor, a database utility, a web server for testing pages, and features for publishing your website to the Internet.</span></span> <span data-ttu-id="11cc1-159">WebMatrix 是免費的而且容易安裝卻簡單易用。</span><span class="sxs-lookup"><span data-stu-id="11cc1-159">WebMatrix is free, and it's easy to install and easy to use.</span></span> <span data-ttu-id="11cc1-160">（它也適用於只是單純的 HTML 網頁，以及適用於 PHP 之類的其他技術。）</span><span class="sxs-lookup"><span data-stu-id="11cc1-160">(It also works for just plain HTML pages, as well as for other technologies like PHP.)</span></span>

<span data-ttu-id="11cc1-161">您不會實際*有*要以 ASP.NET Web Pages 使用 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="11cc1-161">You don't actually *have* to use WebMatrix to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="11cc1-162">您可以使用文字編輯器 中，建立頁面，例如及測試頁面使用 web 伺服器，您可以存取。</span><span class="sxs-lookup"><span data-stu-id="11cc1-162">You can create pages by using a text editor, for example, and test pages by using a web server that you have access to.</span></span> <span data-ttu-id="11cc1-163">不過，WebMatrix 全都非常容易，因此這些教學課程會使用整個 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="11cc1-163">However, WebMatrix makes it all very easy, so these tutorials will use WebMatrix throughout.</span></span>

## <a name="about-these-tutorials"></a><span data-ttu-id="11cc1-164">關於這些教學課程</span><span class="sxs-lookup"><span data-stu-id="11cc1-164">About These Tutorials</span></span>

<span data-ttu-id="11cc1-165">本教學課程組是如何使用 ASP.NET Web Pages 簡介。</span><span class="sxs-lookup"><span data-stu-id="11cc1-165">This tutorial set is an introduction to how to use ASP.NET Web Pages.</span></span> <span data-ttu-id="11cc1-166">9 的教學課程中總共有本入門教學課程組。</span><span class="sxs-lookup"><span data-stu-id="11cc1-166">There are 9 tutorials total in this introductory tutorial set.</span></span> <span data-ttu-id="11cc1-167">它的一系列的教學課程的集合可讓您從 ASP.NET Web Pages 新手進入建立真實的專業網站的一部分。</span><span class="sxs-lookup"><span data-stu-id="11cc1-167">It's part of a series of tutorial sets that takes you from ASP.NET Web Pages novice to creating real, professional-looking websites.</span></span>

<span data-ttu-id="11cc1-168">本教學課程中第一個設定將示範如何使用 ASP.NET Web Pages 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="11cc1-168">This first tutorial set concentrates on showing you the basics of how to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="11cc1-169">當您完成時，您可以使用其中一段時間，而可深入探索網頁收取的其他教學課程集合。</span><span class="sxs-lookup"><span data-stu-id="11cc1-169">When you're done, you can work with additional tutorial sets that pick up where this one ends and that explore Web Pages in more depth.</span></span>

<span data-ttu-id="11cc1-170">我們刻意會輕鬆深入的說明。</span><span class="sxs-lookup"><span data-stu-id="11cc1-170">We deliberately go easy on the in-depth explanations.</span></span> <span data-ttu-id="11cc1-171">然後每當我們顯示一些內容，本教學課程組我們一律選擇我們認為的方式是最容易了解。</span><span class="sxs-lookup"><span data-stu-id="11cc1-171">And whenever we show something, for this tutorial set we always choose the way that we think is easiest to understand.</span></span> <span data-ttu-id="11cc1-172">稍後的教學課程集移至更深入的討論，並告訴您更有效率或更有彈性的方法 （也更有趣）。</span><span class="sxs-lookup"><span data-stu-id="11cc1-172">Later tutorial sets go into more depth and show you more efficient or more flexible approaches (also more fun).</span></span> <span data-ttu-id="11cc1-173">但這些教學課程要求您先了解基本概念。</span><span class="sxs-lookup"><span data-stu-id="11cc1-173">But those tutorials require you to understand the basics first.</span></span>

<span data-ttu-id="11cc1-174">您剛啟動的教學課程集涵蓋這些功能的 ASP.NET Web Pages:</span><span class="sxs-lookup"><span data-stu-id="11cc1-174">The tutorial set you've just started covers these features of ASP.NET Web Pages:</span></span>

- <span data-ttu-id="11cc1-175">簡介和開始安裝的所有項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-175">Introduction and getting everything installed.</span></span> <span data-ttu-id="11cc1-176">（這是您正在閱讀教學課程中）。</span><span class="sxs-lookup"><span data-stu-id="11cc1-176">(That's in the tutorial you're reading.)</span></span>
- <span data-ttu-id="11cc1-177">ASP.NET Web Pages 程式設計的基本概念。</span><span class="sxs-lookup"><span data-stu-id="11cc1-177">The basics of ASP.NET Web Pages programming.</span></span>
- <span data-ttu-id="11cc1-178">建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="11cc1-178">Creating a database.</span></span>
- <span data-ttu-id="11cc1-179">建立及處理使用者輸入的表單。</span><span class="sxs-lookup"><span data-stu-id="11cc1-179">Creating and processing a user input form.</span></span>
- <span data-ttu-id="11cc1-180">加入、 更新和刪除資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="11cc1-180">Adding, updating, and deleting data in the database.</span></span>

## <a name="what-will-you-create"></a><span data-ttu-id="11cc1-181">您會建立什麼？</span><span class="sxs-lookup"><span data-stu-id="11cc1-181">What Will You Create?</span></span>

<span data-ttu-id="11cc1-182">本教學課程中設定，而且後續的不外乎的網站，您可以在其中列出您要的電影。</span><span class="sxs-lookup"><span data-stu-id="11cc1-182">This tutorial set and subsequent ones revolve around a website where you can list movies that you like.</span></span> <span data-ttu-id="11cc1-183">您可以輸入電影、 加以編輯，以及列出它們。</span><span class="sxs-lookup"><span data-stu-id="11cc1-183">You'll be able to enter movies, edit them, and list them.</span></span> <span data-ttu-id="11cc1-184">以下是您將建立在此教學課程的集合中的頁面數。</span><span class="sxs-lookup"><span data-stu-id="11cc1-184">Here are a couple of the pages you'll create in this tutorial set.</span></span> <span data-ttu-id="11cc1-185">第一個顯示電影清單，您將建立的頁面：</span><span class="sxs-lookup"><span data-stu-id="11cc1-185">The first one shows the movie listing page that you'll create:</span></span>

![已電影應用程式顯示電影清單](getting-started/_static/image1.png)

<span data-ttu-id="11cc1-187">而以下是可讓您將新的電影資訊新增至您的網站頁面：</span><span class="sxs-lookup"><span data-stu-id="11cc1-187">And here's the page that lets you add new movie information to your site:</span></span>

![完成的電影應用程式顯示新增影片頁面](getting-started/_static/image2.png)

<span data-ttu-id="11cc1-189">後續的教學課程將建置在此設定，並新增更多的功能，例如上傳圖片，讓使用者登入、 傳送電子郵件，以及與社交媒體整合。</span><span class="sxs-lookup"><span data-stu-id="11cc1-189">Subsequent tutorial sets build on this set and add more functionality, like uploading pictures, letting people log in, sending email, and integrating with social media.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="11cc1-190">請參閱在 Azure 上執行此應用程式</span><span class="sxs-lookup"><span data-stu-id="11cc1-190">See this App Running on Azure</span></span>

<span data-ttu-id="11cc1-191">若要查看為即時 web 應用程式正在執行的完成的網站嗎？</span><span class="sxs-lookup"><span data-stu-id="11cc1-191">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="11cc1-192">您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。</span><span class="sxs-lookup"><span data-stu-id="11cc1-192">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

<span data-ttu-id="11cc1-193">您需要有 Azure 帳戶才能將此解決方案部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="11cc1-193">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="11cc1-194">如果您還沒有帳戶，您會有下列選項：</span><span class="sxs-lookup"><span data-stu-id="11cc1-194">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="11cc1-195">[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="11cc1-195">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="11cc1-196">[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="11cc1-196">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="installing-everything"></a><span data-ttu-id="11cc1-197">安裝的所有項目</span><span class="sxs-lookup"><span data-stu-id="11cc1-197">Installing Everything</span></span>

<span data-ttu-id="11cc1-198">您可以使用 microsoft Web Platform Installer 來安裝的所有項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-198">You can install everything by using the Web Platform Installer from Microsoft.</span></span> <span data-ttu-id="11cc1-199">作用中，您會安裝安裝程式，並再用它來安裝所有其他項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-199">In effect, you install the installer, and then use it to install everything else.</span></span>

<span data-ttu-id="11cc1-200">若要使用的網頁，您必須至少有 Windows XP SP3 安裝或 Windows Server 2008 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="11cc1-200">To use Web Pages, you have to be have at least Windows XP with SP3 installed, or Windows Server 2008 or later.</span></span>

<span data-ttu-id="11cc1-201">在 [網頁 」 頁面](../../../index.md)ASP.NET 網站中，按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-201">On the [Web Pages page](../../../index.md) of the ASP.NET website, click **Install**.</span></span>

![ASP.NET Web 站台顯示&quot;安裝 WebMatrix&quot;按鈕](getting-started/_static/image3.png)

<span data-ttu-id="11cc1-203">系統會要求您接受這些授權條款和隱私權聲明，然後再安裝 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="11cc1-203">You are asked to accept the license terms and privacy statement before installing WebMatrix.</span></span>

![接受條款，以開始安裝](getting-started/_static/image4.png)

<span data-ttu-id="11cc1-205">按一下 **執行**開始安裝。</span><span class="sxs-lookup"><span data-stu-id="11cc1-205">Click **Run** to start the installation.</span></span> <span data-ttu-id="11cc1-206">(如果您想要儲存該安裝程式，請按一下**儲存**，然後從儲存位置的資料夾執行安裝程式。)</span><span class="sxs-lookup"><span data-stu-id="11cc1-206">(If you want to save the installer, click **Save** and then run the installer from the folder where you saved it.)</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="11cc1-207">Web Platform Installer 出現時，您可以準備安裝 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="11cc1-207">The Web Platform Installer appears, ready to install WebMatrix.</span></span> <span data-ttu-id="11cc1-208">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="11cc1-208">Click **Install**.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="11cc1-209">安裝程序找出它必須在電腦上安裝，並啟動程序。</span><span class="sxs-lookup"><span data-stu-id="11cc1-209">The installation process figures out what it has to install on your computer and starts the process.</span></span> <span data-ttu-id="11cc1-210">取決於什麼完全有安裝，程序可能需要幾分鐘的時間從數分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="11cc1-210">Depending on what exactly has to be installed, the process can take anywhere from a few moments to several minutes.</span></span> <span data-ttu-id="11cc1-211">選取 **我接受**接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="11cc1-211">Select **I Accept** to accept the license terms.</span></span>

## <a name="hello-webmatrix"></a><span data-ttu-id="11cc1-212">Hello WebMatrix</span><span class="sxs-lookup"><span data-stu-id="11cc1-212">Hello, WebMatrix</span></span>

<span data-ttu-id="11cc1-213">完成時，安裝程序就可以自動啟動 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="11cc1-213">When it's done, the installation process can launch WebMatrix automatically.</span></span> <span data-ttu-id="11cc1-214">如果沒有，在 Windows 中，從**開始**功能表，啟動**Microsoft WebMatrix**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-214">If it doesn't, in Windows, from the **Start** menu, launch **Microsoft WebMatrix**.</span></span>

<span data-ttu-id="11cc1-215">當您第一次啟動 WebMatrix 時，您可以使用您的 Microsoft 帳戶登入 Microsoft Azure 的機率。</span><span class="sxs-lookup"><span data-stu-id="11cc1-215">When you launch WebMatrix for the first time, you are given a chance to sign in to Microsoft Azure with your Microsoft account.</span></span> <span data-ttu-id="11cc1-216">登入，您會收到透過 Azure 的 10 個免費的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11cc1-216">By signing in, you will receive 10 free web apps through Azure.</span></span> <span data-ttu-id="11cc1-217">這些免費的 web 應用程式提供便利的方式來測試您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="11cc1-217">These free web apps provide a convenient way to test your apps.</span></span> <span data-ttu-id="11cc1-218">如果您還沒有 Azure 帳戶，但您需要 MSDN 訂用帳戶，您可以[啟用您的 MSDN 訂用帳戶權益](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-218">If you don't already have an Azure account, but you do have an MSDN subscription, you can [activate your MSDN subscription benefits](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="11cc1-219">否則，您可以建立免費的試用帳戶只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="11cc1-219">Otherwise, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="11cc1-220">如需詳細資訊，請參閱 < [Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-220">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="11cc1-221">您不必立即登入以繼續進行本教學課程。</span><span class="sxs-lookup"><span data-stu-id="11cc1-221">You do not have to sign in right now to continue with this tutorial.</span></span> <span data-ttu-id="11cc1-222">如果您未登入現在，您仍然必須稍後登入選項。</span><span class="sxs-lookup"><span data-stu-id="11cc1-222">If you do not sign in now, you will still have the option to sign in later.</span></span> <span data-ttu-id="11cc1-223">上次[主題](publishing.md)在本教學課程系列會說明如何將您的網站部署至 Azure; 因此，您必須登入以完成該主題。</span><span class="sxs-lookup"><span data-stu-id="11cc1-223">The last [topic](publishing.md) in this tutorial series covers how to deploy your website to Azure; therefore, you would need to sign in to complete that topic.</span></span>

<span data-ttu-id="11cc1-224">此時，請使用您的 Microsoft 帳戶或選取的登入**不是現在**右上角。</span><span class="sxs-lookup"><span data-stu-id="11cc1-224">At this point, either sign in with your Microsoft account or select **Not Now** in the lower right corner.</span></span>

![登入](getting-started/_static/image7.png)

<span data-ttu-id="11cc1-226">若要開始，您將建立空白的網站，並新增頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-226">To begin, you'll create a blank website and add a page.</span></span> <span data-ttu-id="11cc1-227">在稍後的教學課程中，在此集合中，您將播放使用其中一個內建的網站範本。</span><span class="sxs-lookup"><span data-stu-id="11cc1-227">In a later tutorial in this set you'll play with one of the built-in website templates.</span></span>

<span data-ttu-id="11cc1-228">在 [開始] 視窗中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-228">In the start window, click **New**.</span></span>

![WebMatrix 啟動螢幕](getting-started/_static/image8.png)

<span data-ttu-id="11cc1-230">範本是網站的預先建置的檔案和頁面的不同類型。</span><span class="sxs-lookup"><span data-stu-id="11cc1-230">Templates are prebuilt files and pages for different types of websites.</span></span> <span data-ttu-id="11cc1-231">若要查看所有可用的預設範本，請選取 [範本庫] 選項。</span><span class="sxs-lookup"><span data-stu-id="11cc1-231">To see all of the templates that are available by default, select the Template Gallery option.</span></span>

![選取範本資源庫](getting-started/_static/image9.png)

<span data-ttu-id="11cc1-233">在 **快速入門**視窗中，選取**空白網站**從**ASP.NET**群組，並命名新的站台 」 WebPagesMovies"。</span><span class="sxs-lookup"><span data-stu-id="11cc1-233">In the **Quick Start** window, select **Empty Site** from the **ASP.NET** group and name the new site "WebPagesMovies".</span></span>

![WebMatrix 快速開始視窗中選取的空白網站範本](getting-started/_static/image10.png)

<span data-ttu-id="11cc1-235">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="11cc1-235">Click **Next**.</span></span>

<span data-ttu-id="11cc1-236">如果您已登入您的 Microsoft 帳戶，您將有機會在 Azure 上建立站台。</span><span class="sxs-lookup"><span data-stu-id="11cc1-236">If you have signed in to your Microsoft account, you will be given the opportunity to create the site on Azure.</span></span> <span data-ttu-id="11cc1-237">根據您的網站的預設名稱的名稱**WebPagesMovies.azurewebsites.net**建議; 不過，驚嘆號表示此名稱不是 Windows Azure 上提供。</span><span class="sxs-lookup"><span data-stu-id="11cc1-237">Based on the name of your site, the default name of **WebPagesMovies.azurewebsites.net** is suggested; however, the exclamation point indicates that this name is not available on Windows Azure.</span></span> <span data-ttu-id="11cc1-238">為了簡單起見，請選取**略過**略過目前在 Azure 上建立網站。</span><span class="sxs-lookup"><span data-stu-id="11cc1-238">For simplicity, select **Skip** to bypass creating the web site on Azure right now.</span></span> <span data-ttu-id="11cc1-239">稍後在本系列中，我們會將站台發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="11cc1-239">Later in this series, we will publish the site to Azure.</span></span>

![建立 azure 網站](getting-started/_static/image11.png)

<span data-ttu-id="11cc1-241">WebMatrix 會建立並開啟 站台：</span><span class="sxs-lookup"><span data-stu-id="11cc1-241">WebMatrix creates and opens the site:</span></span>

![在 WebMatrix 中開啟新的 WebPagesMovies 站台](getting-started/_static/image12.png)

<span data-ttu-id="11cc1-243">在頂端，還有快速存取工具列和功能區。</span><span class="sxs-lookup"><span data-stu-id="11cc1-243">At the top, there's a Quick Access Toolbar and a ribbon.</span></span> <span data-ttu-id="11cc1-244">在左下方，您會看到工作區選取器切換工作 (**站台**，**檔案**，**資料庫**，**報表**)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-244">At the bottom left, you see the workspace selector where you switch between tasks (**Site**, **Files**, **Databases**, **Reports**).</span></span> <span data-ttu-id="11cc1-245">在右邊是 [內容] 窗格中，編輯器和報告。</span><span class="sxs-lookup"><span data-stu-id="11cc1-245">On the right is the content pane for the editor and for reports.</span></span> <span data-ttu-id="11cc1-246">和下方偶而會看到訊息的通知列。</span><span class="sxs-lookup"><span data-stu-id="11cc1-246">And across the bottom you'll occasionally see a notification bar for messages.</span></span>

<span data-ttu-id="11cc1-247">您將進一步了解 WebMatrix 和它的功能當您瀏覽這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="11cc1-247">You'll learn more about WebMatrix and its features as you go through these tutorials.</span></span>

## <a name="creating-a-web-page"></a><span data-ttu-id="11cc1-248">建立的 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="11cc1-248">Creating a Web Page</span></span>

<span data-ttu-id="11cc1-249">熟悉 WebMatrix 及 ASP.NET 網頁，您將建立簡單的頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-249">To become familiar with WebMatrix and ASP.NET Web Pages, you'll create a simple page.</span></span>

<span data-ttu-id="11cc1-250">在 工作區選取器中，選取**檔案**工作區。</span><span class="sxs-lookup"><span data-stu-id="11cc1-250">In the workspace selector, select the **Files** workspace.</span></span> <span data-ttu-id="11cc1-251">此工作區可讓您使用 檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="11cc1-251">This workspace lets you work with files and folders.</span></span> <span data-ttu-id="11cc1-252">左的窗格會顯示您的站台的檔案結構。</span><span class="sxs-lookup"><span data-stu-id="11cc1-252">The left pane shows the file structure of your site.</span></span> <span data-ttu-id="11cc1-253">功能區的變更，以顯示與檔案相關的工作。</span><span class="sxs-lookup"><span data-stu-id="11cc1-253">The ribbon changes to show file-related tasks.</span></span>

![在 WebMatrix 中的檔案工作區](getting-started/_static/image13.png)

<span data-ttu-id="11cc1-255">在 功能區中，按一下 底下的箭號**的新**，然後按一下**新檔案**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-255">In the ribbon, click the arrow under **New** and then click **New File**.</span></span>

![使用&quot;新增&quot;命令在功能區中建立新的檔案](getting-started/_static/image14.png)

<span data-ttu-id="11cc1-257">WebMatrix 會顯示一份檔案類型。</span><span class="sxs-lookup"><span data-stu-id="11cc1-257">WebMatrix displays a list of file types.</span></span> <span data-ttu-id="11cc1-258">選取  **CSHTML**，然後在**名稱**方塊中，輸入"HelloWorld"。</span><span class="sxs-lookup"><span data-stu-id="11cc1-258">Select **CSHTML**, and in the **Name** box, type "HelloWorld".</span></span> <span data-ttu-id="11cc1-259">CSHTML 頁面是 ASP.NET Web Pages 頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-259">A CSHTML page is an ASP.NET Web Pages page.</span></span>

![建立新的 CSHTML 頁面命名 HelloWorld.cshtml](getting-started/_static/image15.png)

<span data-ttu-id="11cc1-261">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="11cc1-261">Click **OK**.</span></span>

<span data-ttu-id="11cc1-262">WebMatrix 所建立的網頁，並在編輯器中開啟。</span><span class="sxs-lookup"><span data-stu-id="11cc1-262">WebMatrix creates the page and opens it in the editor.</span></span>

![WebMatrix 編輯器中新的 [HelloWorld] 頁面](getting-started/_static/image16.png)

<span data-ttu-id="11cc1-264">如您所見，頁面會包含大部分是一般的 HTML 標記，除了區塊上方看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="11cc1-264">As you can see, the page contains mostly ordinary HTML markup, except for a block at the top that looks like this:</span></span>

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

<span data-ttu-id="11cc1-265">新增程式碼，因為您很快會看到。</span><span class="sxs-lookup"><span data-stu-id="11cc1-265">That's for adding code, as you'll see shortly.</span></span>

<span data-ttu-id="11cc1-266">請注意，頁面的不同部分&mdash;項目名稱、 屬性和文字，以及位於頂端的區塊 — 所有在不同的色彩。</span><span class="sxs-lookup"><span data-stu-id="11cc1-266">Notice that the different parts of the page &mdash; the element names, attributes, and text, plus the block at the top — are all in different colors.</span></span> <span data-ttu-id="11cc1-267">這就叫做*語法反白顯示*，並讓您更輕鬆地清除所有項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-267">This is called *syntax highlighting*, and it makes it easier to keep everything clear.</span></span> <span data-ttu-id="11cc1-268">它是其中一個功能，可讓您輕鬆地在 WebMatrix 中的網頁使用。</span><span class="sxs-lookup"><span data-stu-id="11cc1-268">It's one of the features that makes it easy to work with web pages in WebMatrix.</span></span>

<span data-ttu-id="11cc1-269">新增的內容`<head>`和`<body>`如下列範例所示的項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-269">Add content for the `<head>` and `<body>` elements like in the following example.</span></span> <span data-ttu-id="11cc1-270">（如果您想，您可以只將下列區塊複製和使用此程式碼取代整個現有的頁面。）</span><span class="sxs-lookup"><span data-stu-id="11cc1-270">(If you want, you can just copy the following block and replace the entire existing page with this code.)</span></span>

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

<span data-ttu-id="11cc1-271">在 快速存取工具列或在**檔案**功能表上，按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-271">In the Quick Access Toolbar or in the **File** menu, click **Save**.</span></span>

![在 WebMatrix 快速存取工具列中 [儲存] 按鈕](getting-started/_static/image17.png)

## <a name="testing-the-page"></a><span data-ttu-id="11cc1-273">測試頁面</span><span class="sxs-lookup"><span data-stu-id="11cc1-273">Testing the Page</span></span>

<span data-ttu-id="11cc1-274">在 **檔案**工作區中，以滑鼠右鍵按一下*HelloWorld.cshtml*頁面，然後按一下**瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="11cc1-274">In the **Files** workspace, right-click the *HelloWorld.cshtml* page and then click **Launch in browser**.</span></span>

![執行使用 WebMatrix 功能區中的 [執行] 按鈕的頁面](getting-started/_static/image18.png)

<span data-ttu-id="11cc1-276">WebMatrix 啟動內建網頁伺服器 (IIS Express) 可供您測試您的電腦上的頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-276">WebMatrix starts a built-in web server (IIS Express) that you can use to test pages on your computer.</span></span> <span data-ttu-id="11cc1-277">（不含 IIS Express 在 WebMatrix 中，您必須將網頁發行到 web 伺服器位置之前，您可以測試它。）頁面會顯示在預設瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="11cc1-277">(Without IIS Express in WebMatrix, you'd have to publish your page to a web server somewhere before you could test it.) The page is displayed in your default browser.</span></span>

![&quot;Hello World&quot;瀏覽器中執行的頁面](getting-started/_static/image19.png)

<span data-ttu-id="11cc1-279">請注意，當您在 WebMatrix 中測試頁面，在瀏覽器中的 URL 會像下面`http://localhost:33651/HelloWorld.cshtml.`名稱*localhost*指的是本機伺服器，這表示頁面由您自己的電腦上的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="11cc1-279">Notice that when you test a page in WebMatrix, the URL in the browser is something like `http://localhost:33651/HelloWorld.cshtml.` The name *localhost* refers to a local server, meaning that the page is served by a web server that's on your own computer.</span></span> <span data-ttu-id="11cc1-280">如所述，WebMatrix 便會包含名為 IIS Express 時啟動的頁面上所執行的 web 伺服器程式。</span><span class="sxs-lookup"><span data-stu-id="11cc1-280">As noted, WebMatrix includes a web server program named IIS Express that runs when you launch a page.</span></span>

<span data-ttu-id="11cc1-281">之後的數字*localhost* (例如*localhost:33651*) 是指*連接埠號碼*您電腦上。</span><span class="sxs-lookup"><span data-stu-id="11cc1-281">The number after *localhost* (for example, *localhost:33651*) refers to a *port number* on your computer.</span></span> <span data-ttu-id="11cc1-282">這是 「 通道 」，IIS Express 會使用此特定的網站數目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-282">This is the number of the "channel" that IIS Express uses for this particular website.</span></span> <span data-ttu-id="11cc1-283">從 1024 到 65536 的範圍，當您建立您的網站，並對每個您所建立的網站不同的連接埠號碼會隨機選取。</span><span class="sxs-lookup"><span data-stu-id="11cc1-283">The port number is selected at random from the range 1024 through 65536 when you create your site, and it's different for every site that you create.</span></span> <span data-ttu-id="11cc1-284">（當您測試您自己的網站時，連接埠號碼幾乎肯定會比 33561 不同的數字。）每個網站使用不同的連接埠，IIS Express 可以直接保留其中一個站台通訊。</span><span class="sxs-lookup"><span data-stu-id="11cc1-284">(When you test your own site, the port number will almost certainly be a different number than 33561.) By using a different port for each website, IIS Express can keep straight which of your sites it's talking to.</span></span>

<span data-ttu-id="11cc1-285">稍後當您將網站發佈到公開的 web 伺服器，您不會再看見*localhost*在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="11cc1-285">Later when you publish your site to a public web server, you no longer see *localhost* in the URL.</span></span> <span data-ttu-id="11cc1-286">此時，您會看到更一般的 URL，例如`http://myhostingsite/mywebsite/HelloWorld.cshtml`或任何頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-286">At that point, you'll see a more typical URL like `http://myhostingsite/mywebsite/HelloWorld.cshtml` or whatever the page is.</span></span> <span data-ttu-id="11cc1-287">您將進一步了解稍後在本教學課程系列中發行站台。</span><span class="sxs-lookup"><span data-stu-id="11cc1-287">You'll learn more about publishing a site later in this tutorial series.</span></span>

## <a name="adding-some-server-side-code"></a><span data-ttu-id="11cc1-288">加入一些伺服器端程式碼</span><span class="sxs-lookup"><span data-stu-id="11cc1-288">Adding Some Server-Side Code</span></span>

<span data-ttu-id="11cc1-289">關閉瀏覽器，並返回 [WebMatrix] 頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-289">Close the browser and go back to the page in WebMatrix.</span></span>

<span data-ttu-id="11cc1-290">程式碼區塊中加入一行，使它看起來像下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="11cc1-290">Add a line to the code block so that it looks like the following code:</span></span>

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

<span data-ttu-id="11cc1-291">這是有點 Razor 程式碼。</span><span class="sxs-lookup"><span data-stu-id="11cc1-291">This is a little bit of Razor code.</span></span> <span data-ttu-id="11cc1-292">它會取得目前的日期和時間，並放到該值可能清楚*變數*名為`currentDateTime`。</span><span class="sxs-lookup"><span data-stu-id="11cc1-292">It's probably clear that it gets the current date and time and puts that value into a *variable* named `currentDateTime`.</span></span> <span data-ttu-id="11cc1-293">您將深入了解有關在下一個教學課程中的 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="11cc1-293">You'll read more about Razor syntax in the next tutorial.</span></span>

<span data-ttu-id="11cc1-294">本文的頁面上之後,`<p>Hello World!</p>`項目，新增下列：</span><span class="sxs-lookup"><span data-stu-id="11cc1-294">In the body of the page, after the `<p>Hello World!</p>` element, add the following:</span></span>

[!code-html[Main](getting-started/samples/sample4.html)]

<span data-ttu-id="11cc1-295">此程式碼取得值，您將放入`currentDateTime`頂端變數並將它插入網頁的標記。</span><span class="sxs-lookup"><span data-stu-id="11cc1-295">This code gets the value that you put into the `currentDateTime` variable at the top and inserts it into the markup of the page.</span></span> <span data-ttu-id="11cc1-296">`@`字元標示 ASP.NET Web Pages 中的程式碼頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-296">The `@` character marks the ASP.NET Web Pages code in the page.</span></span>

<span data-ttu-id="11cc1-297">執行一次 （WebMatrix 將變更儲存為您執行頁面之前） 頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-297">Run the page again (WebMatrix saves the changes for you before it runs the page).</span></span> <span data-ttu-id="11cc1-298">此時您會看到網頁的時間與日期。</span><span class="sxs-lookup"><span data-stu-id="11cc1-298">This time you see the date and time in the page.</span></span>

![&quot;Hello World&quot;與動態產生的時間顯示瀏覽器中執行的頁面](getting-started/_static/image20.png)

<span data-ttu-id="11cc1-300">請稍候片刻，然後重新整理瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-300">Wait a few moments and then refresh the page in the browser.</span></span> <span data-ttu-id="11cc1-301">日期和時間顯示已更新。</span><span class="sxs-lookup"><span data-stu-id="11cc1-301">The date and time display is updated.</span></span>

<span data-ttu-id="11cc1-302">在瀏覽器中，了解網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="11cc1-302">In the browser, look at the page source.</span></span> <span data-ttu-id="11cc1-303">它看起來像下列標記：</span><span class="sxs-lookup"><span data-stu-id="11cc1-303">It looks like the following markup:</span></span>

[!code-html[Main](getting-started/samples/sample5.html)]

<span data-ttu-id="11cc1-304">請注意，`@{ }`上層區塊不存在。</span><span class="sxs-lookup"><span data-stu-id="11cc1-304">Notice that the `@{ }` block at the top isn't there.</span></span> <span data-ttu-id="11cc1-305">也請注意，日期和時間顯示的實際字元字串 (`1/18/2012 2:49:50 PM`或任何)，而非`@currentDateTime`還用像是 *.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-305">Also notice that the date and time display shows an actual string of characters (`1/18/2012 2:49:50 PM` or whatever), not `@currentDateTime` like you had in the *.cshtml* page.</span></span> <span data-ttu-id="11cc1-306">發生了什麼事如下，當您執行頁面時，ASP.NET 會處理所有的程式碼 （很少在此情況下） 已標`@`。</span><span class="sxs-lookup"><span data-stu-id="11cc1-306">What happened here is that when you ran the page, ASP.NET processed all the code (very little in this case) that was marked with `@`.</span></span> <span data-ttu-id="11cc1-307">程式碼會產生輸出，以及該輸出插入至頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-307">The code produces output, and that output was inserted into the page.</span></span>

## <a name="this-is-what-aspnet-web-pages-are-about"></a><span data-ttu-id="11cc1-308">這是 ASP.NET Web Pages 是關於</span><span class="sxs-lookup"><span data-stu-id="11cc1-308">This Is What ASP.NET Web Pages Are About</span></span>

<span data-ttu-id="11cc1-309">當您閱讀 ASP.NET Web Pages 會產生動態網頁內容時，您已在此看到是概念。</span><span class="sxs-lookup"><span data-stu-id="11cc1-309">When you read that ASP.NET Web Pages produces dynamic web content, what you've seen here is the idea.</span></span> <span data-ttu-id="11cc1-310">您剛建立的網頁包含您曾看過的同一個 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="11cc1-310">The page you just created contains the same HTML markup that you've seen before.</span></span> <span data-ttu-id="11cc1-311">它也可以包含可執行各種工作的程式碼。</span><span class="sxs-lookup"><span data-stu-id="11cc1-311">It can also contain code that can perform all sorts of tasks.</span></span> <span data-ttu-id="11cc1-312">在此範例中，它會執行簡單的工作，取得目前的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="11cc1-312">In this example, it did the trivial task of getting the current date and time.</span></span> <span data-ttu-id="11cc1-313">如您所見，您可以顛倒的程式碼與 HTML，以在網頁中產生輸出。</span><span class="sxs-lookup"><span data-stu-id="11cc1-313">As you saw, you can intersperse code with the HTML to produce output in the page.</span></span> <span data-ttu-id="11cc1-314">當有人要求 *.cshtml*的瀏覽器，ASP.NET 頁面處理頁面上，它仍在 web 伺服器的實際操作時。</span><span class="sxs-lookup"><span data-stu-id="11cc1-314">When someone requests a *.cshtml* page in the browser, ASP.NET processes the page while it's still in the hands of the web server.</span></span> <span data-ttu-id="11cc1-315">ASP.NET 插入的程式碼的輸出 （如果有的話） 的頁面為 HTML。</span><span class="sxs-lookup"><span data-stu-id="11cc1-315">ASP.NET inserts the output of the code (if any) into the page as HTML.</span></span> <span data-ttu-id="11cc1-316">程式碼處理完成時，ASP.NET 會將產生的頁面傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="11cc1-316">When the code processing is done, ASP.NET sends the resulting page to the browser.</span></span> <span data-ttu-id="11cc1-317">像瀏覽器的只是 HTML。</span><span class="sxs-lookup"><span data-stu-id="11cc1-317">All the browser ever gets is HTML.</span></span> <span data-ttu-id="11cc1-318">以下是圖表：</span><span class="sxs-lookup"><span data-stu-id="11cc1-318">Here's a diagram:</span></span>

![ASP.NET 如何產生 HTML 動態的概念流程](getting-started/_static/image21.png)

<span data-ttu-id="11cc1-320">這個概念十分簡單，但有許多有趣的工作可執行程式碼，而且有許多有趣的方式，在其中您可以動態將 HTML 內容新增至頁面。</span><span class="sxs-lookup"><span data-stu-id="11cc1-320">The idea is simple, but there are many interesting tasks that the code can perform, and there are many interesting ways in which you can dynamically add HTML content to the page.</span></span> <span data-ttu-id="11cc1-321">和 ASP.NET *.cshtml*頁面，例如任何 HTML 頁面上，也可以包含在瀏覽器本身 （JavaScript 和 jQuery 程式碼） 中執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="11cc1-321">And ASP.NET *.cshtml* pages, like any HTML page, can also include code that runs in the browser itself (JavaScript and jQuery code).</span></span> <span data-ttu-id="11cc1-322">您將探索所有在本教學課程組和後續的這些項目。</span><span class="sxs-lookup"><span data-stu-id="11cc1-322">You'll explore all of these things in this tutorial set and in subsequent ones.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="11cc1-323">接下來的下一步</span><span class="sxs-lookup"><span data-stu-id="11cc1-323">Coming Up Next</span></span>

<span data-ttu-id="11cc1-324">在本系列中下一個教學課程中，您可以探索 ASP.NET Web Pages 程式設計稍微。</span><span class="sxs-lookup"><span data-stu-id="11cc1-324">In the next tutorial in this series, you explore ASP.NET Web Pages programming a little more.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11cc1-325">其他資源</span><span class="sxs-lookup"><span data-stu-id="11cc1-325">Additional Resources</span></span>

<span data-ttu-id="11cc1-326">[從頭開始建立 ASP.NET 網站](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-326">[Create an ASP.NET website from scratch](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span></span> <span data-ttu-id="11cc1-327">這是教學課程中，特別是關於使用 WebMatrix (不 ASP.NET Web Pages)。</span><span class="sxs-lookup"><span data-stu-id="11cc1-327">This is a tutorial that's specifically about using WebMatrix (not ASP.NET Web Pages).</span></span> <span data-ttu-id="11cc1-328">它會進入有點一些我們不會討論在此教學課程的集合中的 WebMatrix 的其他功能的更詳細。</span><span class="sxs-lookup"><span data-stu-id="11cc1-328">It goes into a little more detail about some of the additional features of WebMatrix that we won't cover in this tutorial set.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="11cc1-329">下一步</span><span class="sxs-lookup"><span data-stu-id="11cc1-329">Next</span></span>](intro-to-web-pages-programming.md)
