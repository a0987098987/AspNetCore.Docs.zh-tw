---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: 導入 ASP.NET Web Pages-開始使用 |Microsoft 文件
author: tfitzmac
description: WebMatrix 是不建議使用做為整合式的開發環境的 ASP.NET Web Pages。 使用 Visual Studio 或 Visual Studio 程式碼。 本指南...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 5fd67a230f76774e102094f42426b8bb126c0cc6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a><span data-ttu-id="d5477-105">介紹的 ASP.NET Web Pages-快速入門</span><span class="sxs-lookup"><span data-stu-id="d5477-105">Introducing ASP.NET Web Pages - Getting Started</span></span>
====================
<span data-ttu-id="d5477-106">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d5477-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> > [!NOTE] 
> > 
> > <span data-ttu-id="d5477-107">WebMatrix 是不建議使用做為整合式的開發環境的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="d5477-107">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="d5477-108">使用[Visual Studio](../program-asp-net-web-pages-in-visual-studio.md)或[Visual Studio 程式碼](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="d5477-108">Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
> 
> 
> <span data-ttu-id="d5477-109">本指南及應用程式可讓您的 ASP.NET Web Pages （第 2 版或更新版本） 的概觀，以及 Razor 語法，輕量型的架構，用於建立動態網站。</span><span class="sxs-lookup"><span data-stu-id="d5477-109">This guidance and application gives you an overview of ASP.NET Web Pages (version 2 or later) and Razor syntax, a lightweight framework for creating dynamic websites.</span></span> <span data-ttu-id="d5477-110">也引進了 WebMatrix，用於建立網頁和網站的工具。</span><span class="sxs-lookup"><span data-stu-id="d5477-110">It also introduces WebMatrix, a tool for creating pages and sites.</span></span>
> 
> <span data-ttu-id="d5477-111">**層級**： 新 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="d5477-111">**Level**: New to ASP.NET Web Pages.</span></span>  
> <span data-ttu-id="d5477-112">**技術假設**: HTML、 基本的階層式樣式表 (CSS)。</span><span class="sxs-lookup"><span data-stu-id="d5477-112">**Skills assumed**: HTML, basic cascading style sheets (CSS).</span></span>
> 
> <span data-ttu-id="d5477-113">您將了解的內容集的第一個教學課程中：</span><span class="sxs-lookup"><span data-stu-id="d5477-113">What you'll learn in the first tutorial of the set:</span></span>
> 
> - <span data-ttu-id="d5477-114">ASP.NET Web Pages 技術是，它為。</span><span class="sxs-lookup"><span data-stu-id="d5477-114">What ASP.NET Web Pages technology is and what it's for.</span></span>
> - <span data-ttu-id="d5477-115">WebMatrix 是什麼。</span><span class="sxs-lookup"><span data-stu-id="d5477-115">What WebMatrix is.</span></span>
> - <span data-ttu-id="d5477-116">如何安裝的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d5477-116">How to install everything.</span></span>
> - <span data-ttu-id="d5477-117">如何建立使用 WebMatrix 的網站。</span><span class="sxs-lookup"><span data-stu-id="d5477-117">How to create a website by using WebMatrix.</span></span>
>   
> 
> <span data-ttu-id="d5477-118">功能/技術討論：</span><span class="sxs-lookup"><span data-stu-id="d5477-118">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="d5477-119">Microsoft Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="d5477-119">Microsoft Web Platform Installer.</span></span>
> - <span data-ttu-id="d5477-120">WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="d5477-120">WebMatrix.</span></span>
> - <span data-ttu-id="d5477-121">*.cshtml*頁面</span><span class="sxs-lookup"><span data-stu-id="d5477-121">*.cshtml* pages</span></span>
>   
> 
> <span data-ttu-id="d5477-122">Mike 教宗最初撰寫本教學課程。</span><span class="sxs-lookup"><span data-stu-id="d5477-122">Mike Pope originally wrote this tutorial.</span></span> <span data-ttu-id="d5477-123">Tom FitzMacken 更新它適用於 Microsoft WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="d5477-123">Tom FitzMacken updated it for Microsoft WebMatrix 3.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d5477-124">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="d5477-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d5477-125">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d5477-125">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d5477-126">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="d5477-126">WebMatrix 3</span></span>


## <a name="what-should-you-know"></a><span data-ttu-id="d5477-127">您應該知道什麼？</span><span class="sxs-lookup"><span data-stu-id="d5477-127">What Should You Know?</span></span>

<span data-ttu-id="d5477-128">我們假設您已熟悉：</span><span class="sxs-lookup"><span data-stu-id="d5477-128">We're assuming that you're familiar with:</span></span>

- <span data-ttu-id="d5477-129">**HTML**。</span><span class="sxs-lookup"><span data-stu-id="d5477-129">**HTML**.</span></span> <span data-ttu-id="d5477-130">沒有深入的專業知識是必要項。</span><span class="sxs-lookup"><span data-stu-id="d5477-130">No in-depth expertise is required.</span></span> <span data-ttu-id="d5477-131">我們將不會說明 HTML，但我們也不會使用複雜的任何項目。</span><span class="sxs-lookup"><span data-stu-id="d5477-131">We won't explain HTML, but we also don't use anything complex.</span></span> <span data-ttu-id="d5477-132">我們會提供 HTML 教學課程，我們認為非常有用的連結。</span><span class="sxs-lookup"><span data-stu-id="d5477-132">We'll provide links to HTML tutorials where we think they're useful.</span></span>
- <span data-ttu-id="d5477-133">**階層式樣式表 (CSS)**。</span><span class="sxs-lookup"><span data-stu-id="d5477-133">**Cascading style sheets (CSS)**.</span></span> <span data-ttu-id="d5477-134">與使用相同 HTML。</span><span class="sxs-lookup"><span data-stu-id="d5477-134">Same as with HTML.</span></span>
- <span data-ttu-id="d5477-135">**基本資料庫概念**。</span><span class="sxs-lookup"><span data-stu-id="d5477-135">**Basic database ideas**.</span></span> <span data-ttu-id="d5477-136">如果您已試算表用於資料和排序和篩選是專業知識的層級的資料，我們通常假設這個教學課程的集合。</span><span class="sxs-lookup"><span data-stu-id="d5477-136">If you've used a spreadsheet for data and sorted and filtered the data, that's the level of expertise we're generally assuming for this tutorial set.</span></span>

<span data-ttu-id="d5477-137">我們也假設您感興趣學習基本程式設計。</span><span class="sxs-lookup"><span data-stu-id="d5477-137">We're also assuming that you're interested in learning basic programming.</span></span> <span data-ttu-id="d5477-138">ASP.NET Web 網頁會使用稱為 C# 的程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="d5477-138">ASP.NET Web Pages use a programming language called C#.</span></span> <span data-ttu-id="d5477-139">您不必使用任何在程式設計中，只在它感興趣的背景。</span><span class="sxs-lookup"><span data-stu-id="d5477-139">You don't have to have any background in programming, just an interest in it.</span></span> <span data-ttu-id="d5477-140">如果您曾經編寫任何 JavaScript 之前的網頁中，您有足夠的背景。</span><span class="sxs-lookup"><span data-stu-id="d5477-140">If you've ever written any JavaScript in a web page before, you've got plenty of background.</span></span>

<span data-ttu-id="d5477-141">請注意，是否您很熟悉程式設計，您可能會發現本教學課程一開始設定緩時變移動時發生我們將新的程式設計人員，了解。</span><span class="sxs-lookup"><span data-stu-id="d5477-141">Note that if you are familiar with programming, you might find that this tutorial set initially moves slowly while we bring new programmers up to speed.</span></span> <span data-ttu-id="d5477-142">隨著過去的第一個的幾個教學課程，不過，會有較少的基本程式設計，以說明，且項目時，會將上，在更快的剪輯。</span><span class="sxs-lookup"><span data-stu-id="d5477-142">As we get past the first few tutorials, though, there will be less basic programming to explain and things will move along at a faster clip.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="d5477-143">您需要什麼？</span><span class="sxs-lookup"><span data-stu-id="d5477-143">What Do You Need?</span></span>

<span data-ttu-id="d5477-144">以下是您需要的項目：</span><span class="sxs-lookup"><span data-stu-id="d5477-144">Here's what you'll need:</span></span>

- <span data-ttu-id="d5477-145">執行 Windows 8、 Windows 7、 Windows Server 2008 或 Windows Server 2012 的電腦。</span><span class="sxs-lookup"><span data-stu-id="d5477-145">A computer that is running Windows 8, Windows 7, Windows Server 2008, or Windows Server 2012.</span></span>
- <span data-ttu-id="d5477-146">即時的網際網路連線。</span><span class="sxs-lookup"><span data-stu-id="d5477-146">A live internet connection.</span></span>
- <span data-ttu-id="d5477-147">系統管理員權限 （所需的安裝程序）。</span><span class="sxs-lookup"><span data-stu-id="d5477-147">Administrator privileges (required for the installation process).</span></span>

## <a name="what-is-aspnet-web-pages"></a><span data-ttu-id="d5477-148">ASP.NET Web Pages 是什麼？</span><span class="sxs-lookup"><span data-stu-id="d5477-148">What Is ASP.NET Web Pages?</span></span>

<span data-ttu-id="d5477-149">ASP.NET Web Pages 是一種架構，可用來建立動態網頁。</span><span class="sxs-lookup"><span data-stu-id="d5477-149">ASP.NET Web Pages is a framework that you can use to create dynamic web pages.</span></span> <span data-ttu-id="d5477-150">簡單的 HTML 網頁是靜態的。取決於其內容頁的固定 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="d5477-150">A simple HTML web page is static; its content is determined by the fixed HTML markup that's in the page.</span></span> <span data-ttu-id="d5477-151">如同您建立以 ASP.NET Web Pages 動態頁面可讓您使用程式碼即時建立網頁內容。</span><span class="sxs-lookup"><span data-stu-id="d5477-151">Dynamic pages like those you create with ASP.NET Web Pages let you create the page content on the fly, by using code.</span></span>

<span data-ttu-id="d5477-152">動態網頁可讓您執行各種作業。</span><span class="sxs-lookup"><span data-stu-id="d5477-152">Dynamic pages let you do all sorts of things.</span></span> <span data-ttu-id="d5477-153">您可以使用的表單，要求使用者輸入，然後變更 此頁面會顯示，或它的外觀。</span><span class="sxs-lookup"><span data-stu-id="d5477-153">You can ask a user for input by using a form and then change what the page displays or how it looks.</span></span> <span data-ttu-id="d5477-154">您可以從使用者取得資訊、 將它儲存在資料庫中，以及然後稍後列出。</span><span class="sxs-lookup"><span data-stu-id="d5477-154">You can take information from a user, save it in a database, and then list it later.</span></span> <span data-ttu-id="d5477-155">您可以從您的網站，傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="d5477-155">You can send email from your site.</span></span> <span data-ttu-id="d5477-156">您可以與其他網站 （例如，對應服務） 上的服務互動，並產生整合來自這些來源的資訊的頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-156">You can interact with other services on the web (for example, a mapping service) and produce pages that integrate information from those sources.</span></span>

## <a name="what-is-webmatrix"></a><span data-ttu-id="d5477-157">WebMatrix 是什麼？</span><span class="sxs-lookup"><span data-stu-id="d5477-157">What is WebMatrix?</span></span>

<span data-ttu-id="d5477-158">WebMatrix 是一種工具，整合網頁編輯器、 資料庫公用程式、 web 伺服器以進行測試頁面，以及您的網站發佈到網際網路的功能。</span><span class="sxs-lookup"><span data-stu-id="d5477-158">WebMatrix is a tool that integrates a web page editor, a database utility, a web server for testing pages, and features for publishing your website to the Internet.</span></span> <span data-ttu-id="d5477-159">WebMatrix 是免費的而且容易安裝且容易使用。</span><span class="sxs-lookup"><span data-stu-id="d5477-159">WebMatrix is free, and it's easy to install and easy to use.</span></span> <span data-ttu-id="d5477-160">（它也可以只 plain HTML 網頁，以及其他 PHP 之類的技術。）</span><span class="sxs-lookup"><span data-stu-id="d5477-160">(It also works for just plain HTML pages, as well as for other technologies like PHP.)</span></span>

<span data-ttu-id="d5477-161">您不會實際*有*運作以 ASP.NET Web Pages 使用 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="d5477-161">You don't actually *have* to use WebMatrix to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="d5477-162">您可以使用文字編輯器 中，建立頁面，例如並測試頁面所使用的 web 伺服器，您具有存取權。</span><span class="sxs-lookup"><span data-stu-id="d5477-162">You can create pages by using a text editor, for example, and test pages by using a web server that you have access to.</span></span> <span data-ttu-id="d5477-163">不過，WebMatrix 都非常容易，因此這些教學課程會使用整個 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="d5477-163">However, WebMatrix makes it all very easy, so these tutorials will use WebMatrix throughout.</span></span>

## <a name="about-these-tutorials"></a><span data-ttu-id="d5477-164">關於這些教學課程</span><span class="sxs-lookup"><span data-stu-id="d5477-164">About These Tutorials</span></span>

<span data-ttu-id="d5477-165">此教學課程集是如何使用 ASP.NET Web Pages 的簡介。</span><span class="sxs-lookup"><span data-stu-id="d5477-165">This tutorial set is an introduction to how to use ASP.NET Web Pages.</span></span> <span data-ttu-id="d5477-166">9 教學課程中總共有此簡介的教學課程集。</span><span class="sxs-lookup"><span data-stu-id="d5477-166">There are 9 tutorials total in this introductory tutorial set.</span></span> <span data-ttu-id="d5477-167">它的一系列的教學課程集，會讓您從 ASP.NET Web Pages 新手建立 real、 專業網站的一部分。</span><span class="sxs-lookup"><span data-stu-id="d5477-167">It's part of a series of tutorial sets that takes you from ASP.NET Web Pages novice to creating real, professional-looking websites.</span></span>

<span data-ttu-id="d5477-168">此第一個教學課程設定著重於說明如何使用以 ASP.NET Web Pages 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="d5477-168">This first tutorial set concentrates on showing you the basics of how to work with ASP.NET Web Pages.</span></span> <span data-ttu-id="d5477-169">當您完成時，您可以使用其他挑選其中一段時間，而，更深入地瀏覽網頁的教學課程組。</span><span class="sxs-lookup"><span data-stu-id="d5477-169">When you're done, you can work with additional tutorial sets that pick up where this one ends and that explore Web Pages in more depth.</span></span>

<span data-ttu-id="d5477-170">我們故意繼續輕鬆深入說明。</span><span class="sxs-lookup"><span data-stu-id="d5477-170">We deliberately go easy on the in-depth explanations.</span></span> <span data-ttu-id="d5477-171">然後每當我們顯示的項目，此教學課程集我們一律選擇我們認為的方式是最容易了解。</span><span class="sxs-lookup"><span data-stu-id="d5477-171">And whenever we show something, for this tutorial set we always choose the way that we think is easiest to understand.</span></span> <span data-ttu-id="d5477-172">更新的教學課程集移到更深入，並且顯示更有效率或更有彈性方法 （也更有趣）。</span><span class="sxs-lookup"><span data-stu-id="d5477-172">Later tutorial sets go into more depth and show you more efficient or more flexible approaches (also more fun).</span></span> <span data-ttu-id="d5477-173">但這些教學課程會要求您先了解基本概念。</span><span class="sxs-lookup"><span data-stu-id="d5477-173">But those tutorials require you to understand the basics first.</span></span>

<span data-ttu-id="d5477-174">您已啟動的教學課程集涵蓋這些功能的 ASP.NET Web Pages:</span><span class="sxs-lookup"><span data-stu-id="d5477-174">The tutorial set you've just started covers these features of ASP.NET Web Pages:</span></span>

- <span data-ttu-id="d5477-175">導入，並取得安裝的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d5477-175">Introduction and getting everything installed.</span></span> <span data-ttu-id="d5477-176">（這是您正在閱讀的教學課程中）。</span><span class="sxs-lookup"><span data-stu-id="d5477-176">(That's in the tutorial you're reading.)</span></span>
- <span data-ttu-id="d5477-177">ASP.NET Web Pages 程式設計基本概念。</span><span class="sxs-lookup"><span data-stu-id="d5477-177">The basics of ASP.NET Web Pages programming.</span></span>
- <span data-ttu-id="d5477-178">建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="d5477-178">Creating a database.</span></span>
- <span data-ttu-id="d5477-179">建立及處理使用者輸入的表單。</span><span class="sxs-lookup"><span data-stu-id="d5477-179">Creating and processing a user input form.</span></span>
- <span data-ttu-id="d5477-180">加入、 更新和刪除資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="d5477-180">Adding, updating, and deleting data in the database.</span></span>

## <a name="what-will-you-create"></a><span data-ttu-id="d5477-181">您將建立什麼？</span><span class="sxs-lookup"><span data-stu-id="d5477-181">What Will You Create?</span></span>

<span data-ttu-id="d5477-182">本教學課程中設定，而且後續的心力都圍繞網站，您可以在其中列出您喜歡的電影。</span><span class="sxs-lookup"><span data-stu-id="d5477-182">This tutorial set and subsequent ones revolve around a website where you can list movies that you like.</span></span> <span data-ttu-id="d5477-183">您可以輸入電影、 編輯它們，並將它們列出。</span><span class="sxs-lookup"><span data-stu-id="d5477-183">You'll be able to enter movies, edit them, and list them.</span></span> <span data-ttu-id="d5477-184">以下是幾個您將建立在此教學課程的集合中的頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-184">Here are a couple of the pages you'll create in this tutorial set.</span></span> <span data-ttu-id="d5477-185">第一個示範影片，列出您要建立的頁面：</span><span class="sxs-lookup"><span data-stu-id="d5477-185">The first one shows the movie listing page that you'll create:</span></span>

![顯示的電影清單已經電影應用程式](getting-started/_static/image1.png)

<span data-ttu-id="d5477-187">此外，這裡有可讓您將新的電影資訊新增至您的網站頁面：</span><span class="sxs-lookup"><span data-stu-id="d5477-187">And here's the page that lets you add new movie information to your site:</span></span>

![顯示將加入電影頁面完成的電影應用程式](getting-started/_static/image2.png)

<span data-ttu-id="d5477-189">後續的教學課程集組建上設定，並加入更多的功能，例如將圖片上載、 讓登入的人、 傳送電子郵件，以及整合社交媒體和。</span><span class="sxs-lookup"><span data-stu-id="d5477-189">Subsequent tutorial sets build on this set and add more functionality, like uploading pictures, letting people log in, sending email, and integrating with social media.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="d5477-190">請參閱 < 在 Azure 上執行此應用程式</span><span class="sxs-lookup"><span data-stu-id="d5477-190">See this App Running on Azure</span></span>

<span data-ttu-id="d5477-191">若要查看即時 web 應用程式執行完成站台嗎？</span><span class="sxs-lookup"><span data-stu-id="d5477-191">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="d5477-192">您可以部署到 Azure 帳戶的完整版本的應用程式，只要按一下下列按鈕。</span><span class="sxs-lookup"><span data-stu-id="d5477-192">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

<span data-ttu-id="d5477-193">您需要 Azure 帳戶，以將此方案部署到 Azure。</span><span class="sxs-lookup"><span data-stu-id="d5477-193">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="d5477-194">如果您沒有帳戶，您會有下列選項：</span><span class="sxs-lookup"><span data-stu-id="d5477-194">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="d5477-195">[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="d5477-195">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="d5477-196">[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。</span><span class="sxs-lookup"><span data-stu-id="d5477-196">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="installing-everything"></a><span data-ttu-id="d5477-197">安裝的所有項目</span><span class="sxs-lookup"><span data-stu-id="d5477-197">Installing Everything</span></span>

<span data-ttu-id="d5477-198">您可以使用 microsoft Web Platform Installer 安裝的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d5477-198">You can install everything by using the Web Platform Installer from Microsoft.</span></span> <span data-ttu-id="d5477-199">作用中，您會安裝安裝程式，並再使用它來安裝其他項目。</span><span class="sxs-lookup"><span data-stu-id="d5477-199">In effect, you install the installer, and then use it to install everything else.</span></span>

<span data-ttu-id="d5477-200">若要使用的網頁，您必須至少有 Windows XP SP3 安裝或 Windows Server 2008 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="d5477-200">To use Web Pages, you have to be have at least Windows XP with SP3 installed, or Windows Server 2008 or later.</span></span>

<span data-ttu-id="d5477-201">在[網頁 」 頁面](../../../index.md)ASP.NET 網站中，按一下 **安裝**。</span><span class="sxs-lookup"><span data-stu-id="d5477-201">On the [Web Pages page](../../../index.md) of the ASP.NET website, click **Install**.</span></span>

![ASP.NET 網頁站台顯示&quot;安裝 WebMatrix&quot;按鈕](getting-started/_static/image3.png)

<span data-ttu-id="d5477-203">系統會要求您接受授權條款及隱私權聲明，才能安裝 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="d5477-203">You are asked to accept the license terms and privacy statement before installing WebMatrix.</span></span>

![接受條款，以開始安裝](getting-started/_static/image4.png)

<span data-ttu-id="d5477-205">按一下**執行**開始安裝。</span><span class="sxs-lookup"><span data-stu-id="d5477-205">Click **Run** to start the installation.</span></span> <span data-ttu-id="d5477-206">(如果您想要儲存安裝程式，按一下**儲存**，然後從儲存所在的資料夾執行安裝程式。)</span><span class="sxs-lookup"><span data-stu-id="d5477-206">(If you want to save the installer, click **Save** and then run the installer from the folder where you saved it.)</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="d5477-207">Web Platform Installer 出現時，您可以準備安裝 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="d5477-207">The Web Platform Installer appears, ready to install WebMatrix.</span></span> <span data-ttu-id="d5477-208">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="d5477-208">Click **Install**.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="d5477-209">安裝程序找出它必須在電腦上安裝並啟動程序。</span><span class="sxs-lookup"><span data-stu-id="d5477-209">The installation process figures out what it has to install on your computer and starts the process.</span></span> <span data-ttu-id="d5477-210">根據什麼完全已安裝，處理程序可以需要幾分鐘的時間到幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="d5477-210">Depending on what exactly has to be installed, the process can take anywhere from a few moments to several minutes.</span></span> <span data-ttu-id="d5477-211">選取**我接受**接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="d5477-211">Select **I Accept** to accept the license terms.</span></span>

## <a name="hello-webmatrix"></a><span data-ttu-id="d5477-212">Hello WebMatrix</span><span class="sxs-lookup"><span data-stu-id="d5477-212">Hello, WebMatrix</span></span>

<span data-ttu-id="d5477-213">完成時，安裝程序就可以自動啟動 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="d5477-213">When it's done, the installation process can launch WebMatrix automatically.</span></span> <span data-ttu-id="d5477-214">如果不是，在 Windows 中，從**啟動**功能表，啟動**Microsoft WebMatrix**。</span><span class="sxs-lookup"><span data-stu-id="d5477-214">If it doesn't, in Windows, from the **Start** menu, launch **Microsoft WebMatrix**.</span></span>

<span data-ttu-id="d5477-215">當您第一次啟動 WebMatrix 時，您可以使用您的 Microsoft 帳戶登入 Microsoft Azure 的機率。</span><span class="sxs-lookup"><span data-stu-id="d5477-215">When you launch WebMatrix for the first time, you are given a chance to sign in to Microsoft Azure with your Microsoft account.</span></span> <span data-ttu-id="d5477-216">只要登入，您會收到 10 的免費 web 應用程式透過 Azure。</span><span class="sxs-lookup"><span data-stu-id="d5477-216">By signing in, you will receive 10 free web apps through Azure.</span></span> <span data-ttu-id="d5477-217">這些免費的 web 應用程式提供便利的方式來測試您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d5477-217">These free web apps provide a convenient way to test your apps.</span></span> <span data-ttu-id="d5477-218">如果您還沒有 Azure 帳戶，但您需要具有 MSDN 訂用帳戶，您可以[啟動您的 MSDN 訂用帳戶權益](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="d5477-218">If you don't already have an Azure account, but you do have an MSDN subscription, you can [activate your MSDN subscription benefits](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="d5477-219">否則，您可以建立免費的試用帳戶只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="d5477-219">Otherwise, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d5477-220">如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="d5477-220">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="d5477-221">您不必立即登入以繼續進行這個教學課程。</span><span class="sxs-lookup"><span data-stu-id="d5477-221">You do not have to sign in right now to continue with this tutorial.</span></span> <span data-ttu-id="d5477-222">如果您沒有登入現在，您仍然可以選擇稍後登入。</span><span class="sxs-lookup"><span data-stu-id="d5477-222">If you do not sign in now, you will still have the option to sign in later.</span></span> <span data-ttu-id="d5477-223">最後一個[主題](publishing.md)在本教學課程系列涵蓋如何將您的網站部署到 Azure; 因此，您必須登入才能完成該主題。</span><span class="sxs-lookup"><span data-stu-id="d5477-223">The last [topic](publishing.md) in this tutorial series covers how to deploy your website to Azure; therefore, you would need to sign in to complete that topic.</span></span>

<span data-ttu-id="d5477-224">此時，請使用您的 Microsoft 帳戶或選取的登入**現在不要**在右下角。</span><span class="sxs-lookup"><span data-stu-id="d5477-224">At this point, either sign in with your Microsoft account or select **Not Now** in the lower right corner.</span></span>

![登入](getting-started/_static/image7.png)

<span data-ttu-id="d5477-226">若要開始，您會建立空白網站，並加入頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-226">To begin, you'll create a blank website and add a page.</span></span> <span data-ttu-id="d5477-227">在稍後的教學課程中，在此集合中，您將播放的其中一個內建的網站範本。</span><span class="sxs-lookup"><span data-stu-id="d5477-227">In a later tutorial in this set you'll play with one of the built-in website templates.</span></span>

<span data-ttu-id="d5477-228">在 [開始] 視窗中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="d5477-228">In the start window, click **New**.</span></span>

![WebMatrix 啟動螢幕](getting-started/_static/image8.png)

<span data-ttu-id="d5477-230">範本是網站的預先建置的檔案和頁面的不同類型。</span><span class="sxs-lookup"><span data-stu-id="d5477-230">Templates are prebuilt files and pages for different types of websites.</span></span> <span data-ttu-id="d5477-231">若要查看所有可用的預設範本，請選取範本庫選項。</span><span class="sxs-lookup"><span data-stu-id="d5477-231">To see all of the templates that are available by default, select the Template Gallery option.</span></span>

![選取範本組件庫](getting-started/_static/image9.png)

<span data-ttu-id="d5477-233">在**快速入門**視窗中，選取**空白網站**從**ASP.NET**群組，並命名新的站台 」 WebPagesMovies"。</span><span class="sxs-lookup"><span data-stu-id="d5477-233">In the **Quick Start** window, select **Empty Site** from the **ASP.NET** group and name the new site "WebPagesMovies".</span></span>

![WebMatrix 快速入門與選取的空白網站範本的視窗](getting-started/_static/image10.png)

<span data-ttu-id="d5477-235">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="d5477-235">Click **Next**.</span></span>

<span data-ttu-id="d5477-236">如果您有登入您的 Microsoft 帳戶，您將有機會在 Azure 上建立站台。</span><span class="sxs-lookup"><span data-stu-id="d5477-236">If you have signed in to your Microsoft account, you will be given the opportunity to create the site on Azure.</span></span> <span data-ttu-id="d5477-237">根據您的網站的預設名稱名稱**WebPagesMovies.azurewebsites.net**建議; 不過，驚嘆號，表示這個名稱並不適用於 Windows Azure。</span><span class="sxs-lookup"><span data-stu-id="d5477-237">Based on the name of your site, the default name of **WebPagesMovies.azurewebsites.net** is suggested; however, the exclamation point indicates that this name is not available on Windows Azure.</span></span> <span data-ttu-id="d5477-238">為了簡單起見，請選取**略過**略過目前在 Azure 上建立網站。</span><span class="sxs-lookup"><span data-stu-id="d5477-238">For simplicity, select **Skip** to bypass creating the web site on Azure right now.</span></span> <span data-ttu-id="d5477-239">在這一系列，稍後我們將發佈站台至 Azure。</span><span class="sxs-lookup"><span data-stu-id="d5477-239">Later in this series, we will publish the site to Azure.</span></span>

![建立 azure 站台](getting-started/_static/image11.png)

<span data-ttu-id="d5477-241">WebMatrix 會建立並開啟的網站：</span><span class="sxs-lookup"><span data-stu-id="d5477-241">WebMatrix creates and opens the site:</span></span>

![在 WebMatrix 中開啟新的 WebPagesMovies 網站](getting-started/_static/image12.png)

<span data-ttu-id="d5477-243">在頂端，也無需快速存取工具列功能區。</span><span class="sxs-lookup"><span data-stu-id="d5477-243">At the top, there's a Quick Access Toolbar and a ribbon.</span></span> <span data-ttu-id="d5477-244">在左下方，您會看到工作區選取器切換的工作 (**網站**，**檔案**，**資料庫**，**報表**)。</span><span class="sxs-lookup"><span data-stu-id="d5477-244">At the bottom left, you see the workspace selector where you switch between tasks (**Site**, **Files**, **Databases**, **Reports**).</span></span> <span data-ttu-id="d5477-245">右邊是內容窗格，針對編輯器 中與報表。</span><span class="sxs-lookup"><span data-stu-id="d5477-245">On the right is the content pane for the editor and for reports.</span></span> <span data-ttu-id="d5477-246">和底端有時候會看到訊息的通知列。</span><span class="sxs-lookup"><span data-stu-id="d5477-246">And across the bottom you'll occasionally see a notification bar for messages.</span></span>

<span data-ttu-id="d5477-247">您將進一步了解 WebMatrix 和它的功能當您瀏覽這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="d5477-247">You'll learn more about WebMatrix and its features as you go through these tutorials.</span></span>

## <a name="creating-a-web-page"></a><span data-ttu-id="d5477-248">建立的 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="d5477-248">Creating a Web Page</span></span>

<span data-ttu-id="d5477-249">熟悉 WebMatrix 及 ASP.NET 網頁，您將建立簡單的網頁。</span><span class="sxs-lookup"><span data-stu-id="d5477-249">To become familiar with WebMatrix and ASP.NET Web Pages, you'll create a simple page.</span></span>

<span data-ttu-id="d5477-250">在工作區中選擇器 中選取**檔案**工作區。</span><span class="sxs-lookup"><span data-stu-id="d5477-250">In the workspace selector, select the **Files** workspace.</span></span> <span data-ttu-id="d5477-251">此工作區可讓您使用檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="d5477-251">This workspace lets you work with files and folders.</span></span> <span data-ttu-id="d5477-252">左的窗格會顯示您的站台的檔案結構。</span><span class="sxs-lookup"><span data-stu-id="d5477-252">The left pane shows the file structure of your site.</span></span> <span data-ttu-id="d5477-253">功能區的變更，以顯示與檔案相關的工作。</span><span class="sxs-lookup"><span data-stu-id="d5477-253">The ribbon changes to show file-related tasks.</span></span>

![檔案在 WebMatrix 工作區](getting-started/_static/image13.png)

<span data-ttu-id="d5477-255">在 功能區中，按一下底下的箭頭**新增**，然後按一下 **新檔案**。</span><span class="sxs-lookup"><span data-stu-id="d5477-255">In the ribbon, click the arrow under **New** and then click **New File**.</span></span>

![使用&quot;新增&quot;命令在功能區中建立新的檔案](getting-started/_static/image14.png)

<span data-ttu-id="d5477-257">WebMatrix 會顯示一份檔案類型。</span><span class="sxs-lookup"><span data-stu-id="d5477-257">WebMatrix displays a list of file types.</span></span> <span data-ttu-id="d5477-258">選取**CSHTML**，然後在**名稱**方塊中，輸入"HelloWorld"。</span><span class="sxs-lookup"><span data-stu-id="d5477-258">Select **CSHTML**, and in the **Name** box, type "HelloWorld".</span></span> <span data-ttu-id="d5477-259">CSHTML 頁面為 ASP.NET Web Pages 頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-259">A CSHTML page is an ASP.NET Web Pages page.</span></span>

![建立新的 CSHTML 頁面命名 HelloWorld.cshtml](getting-started/_static/image15.png)

<span data-ttu-id="d5477-261">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="d5477-261">Click **OK**.</span></span>

<span data-ttu-id="d5477-262">WebMatrix 所建立的網頁，並在編輯器中開啟。</span><span class="sxs-lookup"><span data-stu-id="d5477-262">WebMatrix creates the page and opens it in the editor.</span></span>

![WebMatrix 編輯器中新的 [HelloWorld] 頁面](getting-started/_static/image16.png)

<span data-ttu-id="d5477-264">如您所見，此頁面包含大部分一般 HTML 標記中的，除了區塊上方看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d5477-264">As you can see, the page contains mostly ordinary HTML markup, except for a block at the top that looks like this:</span></span>

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

<span data-ttu-id="d5477-265">新增程式碼中，您將會很快看到。</span><span class="sxs-lookup"><span data-stu-id="d5477-265">That's for adding code, as you'll see shortly.</span></span>

<span data-ttu-id="d5477-266">請注意，頁面的不同部分&mdash;項目名稱、 屬性和文字，再加上位於最上方的區塊，都是完全不同的色彩。</span><span class="sxs-lookup"><span data-stu-id="d5477-266">Notice that the different parts of the page &mdash; the element names, attributes, and text, plus the block at the top — are all in different colors.</span></span> <span data-ttu-id="d5477-267">這稱為*語法反白顯示*，並讓您更輕鬆地清除所有項目。</span><span class="sxs-lookup"><span data-stu-id="d5477-267">This is called *syntax highlighting*, and it makes it easier to keep everything clear.</span></span> <span data-ttu-id="d5477-268">它是其中一個功能，以簡化使用 WebMatrix 中的 web 網頁。</span><span class="sxs-lookup"><span data-stu-id="d5477-268">It's one of the features that makes it easy to work with web pages in WebMatrix.</span></span>

<span data-ttu-id="d5477-269">新增內容`<head>`和`<body>`項目如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="d5477-269">Add content for the `<head>` and `<body>` elements like in the following example.</span></span> <span data-ttu-id="d5477-270">（如果您想，您可以只複製下列區塊並使用此程式碼取代整個的現有頁面。）</span><span class="sxs-lookup"><span data-stu-id="d5477-270">(If you want, you can just copy the following block and replace the entire existing page with this code.)</span></span>

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

<span data-ttu-id="d5477-271">在快速存取工具列或**檔案**功能表上，按一下 **儲存**。</span><span class="sxs-lookup"><span data-stu-id="d5477-271">In the Quick Access Toolbar or in the **File** menu, click **Save**.</span></span>

![WebMatrix 快速存取工具列中的儲存按鈕](getting-started/_static/image17.png)

## <a name="testing-the-page"></a><span data-ttu-id="d5477-273">測試網頁</span><span class="sxs-lookup"><span data-stu-id="d5477-273">Testing the Page</span></span>

<span data-ttu-id="d5477-274">在**檔案**工作區中，以滑鼠右鍵按一下*HelloWorld.cshtml*頁面，然後按一下**瀏覽器中啟動**。</span><span class="sxs-lookup"><span data-stu-id="d5477-274">In the **Files** workspace, right-click the *HelloWorld.cshtml* page and then click **Launch in browser**.</span></span>

![執行網頁使用 WebMatrix 功能區中的 [執行] 按鈕](getting-started/_static/image18.png)

<span data-ttu-id="d5477-276">WebMatrix 啟動內建的 web 伺服器 (IIS Express) 可讓您測試您的電腦上的頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-276">WebMatrix starts a built-in web server (IIS Express) that you can use to test pages on your computer.</span></span> <span data-ttu-id="d5477-277">（如果沒有 IIS Express 在 WebMatrix 中，您就必須將網頁發行到 web 伺服器位置無法測試之前。）頁面會顯示在預設瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d5477-277">(Without IIS Express in WebMatrix, you'd have to publish your page to a web server somewhere before you could test it.) The page is displayed in your default browser.</span></span>

![&quot;Hello World&quot;網頁瀏覽器中執行](getting-started/_static/image19.png)

<span data-ttu-id="d5477-279">請注意，當您在 WebMatrix 中測試網頁，在瀏覽器的 URL 會像下面`http://localhost:33651/HelloWorld.cshtml.`名稱*localhost*指的是本機伺服器，這表示頁面由您自己電腦上的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d5477-279">Notice that when you test a page in WebMatrix, the URL in the browser is something like `http://localhost:33651/HelloWorld.cshtml.` The name *localhost* refers to a local server, meaning that the page is served by a web server that's on your own computer.</span></span> <span data-ttu-id="d5477-280">如前所述，WebMatrix 會包含名為 IIS Express 時啟動頁面執行的 web 伺服器程式。</span><span class="sxs-lookup"><span data-stu-id="d5477-280">As noted, WebMatrix includes a web server program named IIS Express that runs when you launch a page.</span></span>

<span data-ttu-id="d5477-281">之後的數字*localhost* (例如， *localhost:33651*) 是指*連接埠號碼*您電腦上。</span><span class="sxs-lookup"><span data-stu-id="d5477-281">The number after *localhost* (for example, *localhost:33651*) refers to a *port number* on your computer.</span></span> <span data-ttu-id="d5477-282">這是 「 通道 」 IIS Express 用於此特定的網站數目。</span><span class="sxs-lookup"><span data-stu-id="d5477-282">This is the number of the "channel" that IIS Express uses for this particular website.</span></span> <span data-ttu-id="d5477-283">當您建立您的網站，並對每個您所建立的網站不同的連接埠號碼就會隨機選取從 1024 到 65536 的範圍。</span><span class="sxs-lookup"><span data-stu-id="d5477-283">The port number is selected at random from the range 1024 through 65536 when you create your site, and it's different for every site that you create.</span></span> <span data-ttu-id="d5477-284">（當您測試自己的站台時，連接埠號碼幾乎肯定會比 33561 數目不同。）每個網站使用不同的通訊埠，IIS Express 可以直接保留其中一個站台進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d5477-284">(When you test your own site, the port number will almost certainly be a different number than 33561.) By using a different port for each website, IIS Express can keep straight which of your sites it's talking to.</span></span>

<span data-ttu-id="d5477-285">稍後當您將網站發佈到公開的 web 伺服器，不會再看見*localhost*在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="d5477-285">Later when you publish your site to a public web server, you no longer see *localhost* in the URL.</span></span> <span data-ttu-id="d5477-286">此時，您會看到較為典型的 URL，如`http://myhostingsite/mywebsite/HelloWorld.cshtml`或任何頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-286">At that point, you'll see a more typical URL like `http://myhostingsite/mywebsite/HelloWorld.cshtml` or whatever the page is.</span></span> <span data-ttu-id="d5477-287">您將進一步了解發行稍後在本教學課程系列的網站。</span><span class="sxs-lookup"><span data-stu-id="d5477-287">You'll learn more about publishing a site later in this tutorial series.</span></span>

## <a name="adding-some-server-side-code"></a><span data-ttu-id="d5477-288">伺服器端程式碼加入</span><span class="sxs-lookup"><span data-stu-id="d5477-288">Adding Some Server-Side Code</span></span>

<span data-ttu-id="d5477-289">關閉瀏覽器，並返回到 WebMatrix 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-289">Close the browser and go back to the page in WebMatrix.</span></span>

<span data-ttu-id="d5477-290">程式碼區塊中加入一行，使它看起來像下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d5477-290">Add a line to the code block so that it looks like the following code:</span></span>

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

<span data-ttu-id="d5477-291">這是有點 Razor 程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5477-291">This is a little bit of Razor code.</span></span> <span data-ttu-id="d5477-292">取得目前的日期和時間，以及將到該值可能清楚*變數*名為`currentDateTime`。</span><span class="sxs-lookup"><span data-stu-id="d5477-292">It's probably clear that it gets the current date and time and puts that value into a *variable* named `currentDateTime`.</span></span> <span data-ttu-id="d5477-293">您要閱讀更多有關 Razor 語法，在下一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="d5477-293">You'll read more about Razor syntax in the next tutorial.</span></span>

<span data-ttu-id="d5477-294">本文的頁面上之後,`<p>Hello World!</p>`項目，加入下列：</span><span class="sxs-lookup"><span data-stu-id="d5477-294">In the body of the page, after the `<p>Hello World!</p>` element, add the following:</span></span>

[!code-html[Main](getting-started/samples/sample4.html)]

<span data-ttu-id="d5477-295">這個程式碼取得您放入值`currentDateTime`頂端變數並將它插入頁面的標記。</span><span class="sxs-lookup"><span data-stu-id="d5477-295">This code gets the value that you put into the `currentDateTime` variable at the top and inserts it into the markup of the page.</span></span> <span data-ttu-id="d5477-296">`@`字元標示 ASP.NET Web Pages 中的程式碼頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-296">The `@` character marks the ASP.NET Web Pages code in the page.</span></span>

<span data-ttu-id="d5477-297">執行一次 （WebMatrix 會將變更儲存為您執行的頁面之前） 的頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-297">Run the page again (WebMatrix saves the changes for you before it runs the page).</span></span> <span data-ttu-id="d5477-298">此時您會看到的日期和時間在頁面中。</span><span class="sxs-lookup"><span data-stu-id="d5477-298">This time you see the date and time in the page.</span></span>

![&quot;Hello World&quot;與動態產生的時間顯示瀏覽器中執行的頁面](getting-started/_static/image20.png)

<span data-ttu-id="d5477-300">請稍候片刻，然後重新整理瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-300">Wait a few moments and then refresh the page in the browser.</span></span> <span data-ttu-id="d5477-301">日期和時間顯示已更新。</span><span class="sxs-lookup"><span data-stu-id="d5477-301">The date and time display is updated.</span></span>

<span data-ttu-id="d5477-302">在瀏覽器中，查看網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="d5477-302">In the browser, look at the page source.</span></span> <span data-ttu-id="d5477-303">它看起來像下列標記：</span><span class="sxs-lookup"><span data-stu-id="d5477-303">It looks like the following markup:</span></span>

[!code-html[Main](getting-started/samples/sample5.html)]

<span data-ttu-id="d5477-304">請注意，`@{ }`上層區塊不存在。</span><span class="sxs-lookup"><span data-stu-id="d5477-304">Notice that the `@{ }` block at the top isn't there.</span></span> <span data-ttu-id="d5477-305">也請注意，日期和時間顯示顯示的實際字元字串 (`1/18/2012 2:49:50 PM`或任何)，而非`@currentDateTime`像是中*.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-305">Also notice that the date and time display shows an actual string of characters (`1/18/2012 2:49:50 PM` or whatever), not `@currentDateTime` like you had in the *.cshtml* page.</span></span> <span data-ttu-id="d5477-306">發生了什麼事如下，當您執行 [] 頁面上，ASP.NET 會處理所有的程式碼 （幾乎不在此情況下） 已標記為`@`。</span><span class="sxs-lookup"><span data-stu-id="d5477-306">What happened here is that when you ran the page, ASP.NET processed all the code (very little in this case) that was marked with `@`.</span></span> <span data-ttu-id="d5477-307">程式碼會產生輸出，以及該輸出插入至頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-307">The code produces output, and that output was inserted into the page.</span></span>

## <a name="this-is-what-aspnet-web-pages-are-about"></a><span data-ttu-id="d5477-308">這是 ASP.NET 網頁所需</span><span class="sxs-lookup"><span data-stu-id="d5477-308">This Is What ASP.NET Web Pages Are About</span></span>

<span data-ttu-id="d5477-309">當您讀取 ASP.NET Web Pages 會產生動態網頁內容時，您已在此看到最好。</span><span class="sxs-lookup"><span data-stu-id="d5477-309">When you read that ASP.NET Web Pages produces dynamic web content, what you've seen here is the idea.</span></span> <span data-ttu-id="d5477-310">您剛建立的網頁包含您所見過的相同 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="d5477-310">The page you just created contains the same HTML markup that you've seen before.</span></span> <span data-ttu-id="d5477-311">它也可以包含可執行各種工作的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5477-311">It can also contain code that can perform all sorts of tasks.</span></span> <span data-ttu-id="d5477-312">在此範例中，它沒有簡單的工作，取得目前的日期和時間。</span><span class="sxs-lookup"><span data-stu-id="d5477-312">In this example, it did the trivial task of getting the current date and time.</span></span> <span data-ttu-id="d5477-313">如前所述，您可以與 HTML，以產生輸出的頁面中穿插程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5477-313">As you saw, you can intersperse code with the HTML to produce output in the page.</span></span> <span data-ttu-id="d5477-314">當其他人要求*.cshtml*仍在指針的網頁伺服器時，瀏覽器，ASP.NET 網頁處理 page。</span><span class="sxs-lookup"><span data-stu-id="d5477-314">When someone requests a *.cshtml* page in the browser, ASP.NET processes the page while it's still in the hands of the web server.</span></span> <span data-ttu-id="d5477-315">ASP.NET 插入的程式碼的輸出 （如果有的話） 頁面為 HTML。</span><span class="sxs-lookup"><span data-stu-id="d5477-315">ASP.NET inserts the output of the code (if any) into the page as HTML.</span></span> <span data-ttu-id="d5477-316">當程式碼處理完成時，ASP.NET 會傳送至瀏覽器的結果頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-316">When the code processing is done, ASP.NET sends the resulting page to the browser.</span></span> <span data-ttu-id="d5477-317">所有瀏覽器範疇是 HTML。</span><span class="sxs-lookup"><span data-stu-id="d5477-317">All the browser ever gets is HTML.</span></span> <span data-ttu-id="d5477-318">以下是圖表：</span><span class="sxs-lookup"><span data-stu-id="d5477-318">Here's a diagram:</span></span>

![如何 ASP.NET 產生 HTML 動態的概念流程](getting-started/_static/image21.png)

<span data-ttu-id="d5477-320">概念很簡單，但許多有趣的工作可執行程式碼，而且有許多有趣的方式可以在其中您可以動態地將 HTML 內容加入頁面。</span><span class="sxs-lookup"><span data-stu-id="d5477-320">The idea is simple, but there are many interesting tasks that the code can perform, and there are many interesting ways in which you can dynamically add HTML content to the page.</span></span> <span data-ttu-id="d5477-321">和 ASP.NET *.cshtml* ，像是任何 HTML 頁面上，也可以包含在瀏覽器本身 （JavaScript 和 jQuery 程式碼） 中執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d5477-321">And ASP.NET *.cshtml* pages, like any HTML page, can also include code that runs in the browser itself (JavaScript and jQuery code).</span></span> <span data-ttu-id="d5477-322">您將探索所有的這些作業在此教學課程集和後續的。</span><span class="sxs-lookup"><span data-stu-id="d5477-322">You'll explore all of these things in this tutorial set and in subsequent ones.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="d5477-323">接下來</span><span class="sxs-lookup"><span data-stu-id="d5477-323">Coming Up Next</span></span>

<span data-ttu-id="d5477-324">在此系列的下一個教學課程，您瀏覽 ASP.NET Web Pages 稍微程式設計。</span><span class="sxs-lookup"><span data-stu-id="d5477-324">In the next tutorial in this series, you explore ASP.NET Web Pages programming a little more.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5477-325">其他資源</span><span class="sxs-lookup"><span data-stu-id="d5477-325">Additional Resources</span></span>

<span data-ttu-id="d5477-326">[從頭開始建立 ASP.NET 網站](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch)。</span><span class="sxs-lookup"><span data-stu-id="d5477-326">[Create an ASP.NET website from scratch](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch).</span></span> <span data-ttu-id="d5477-327">這是教學課程，特別是有關使用 WebMatrix (不 ASP.NET Web Pages)。</span><span class="sxs-lookup"><span data-stu-id="d5477-327">This is a tutorial that's specifically about using WebMatrix (not ASP.NET Web Pages).</span></span> <span data-ttu-id="d5477-328">它會進入有點有關的一些其他功能不包含在此教學課程的集合中的 WebMatrix 的其他詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d5477-328">It goes into a little more detail about some of the additional features of WebMatrix that we won't cover in this tutorial set.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d5477-329">下一步</span><span class="sxs-lookup"><span data-stu-id="d5477-329">Next</span></span>](intro-to-web-pages-programming.md)
