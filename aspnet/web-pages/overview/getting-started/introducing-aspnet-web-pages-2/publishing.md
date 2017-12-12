---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: "導入 ASP.NET Web Pages-使用 WebMatrix 中發行站台 |Microsoft 文件"
author: tfitzmac
description: "本教學課程是導入了 ASP.NET Web Pages 和 Microsoft WebMatrix 的教學課程組中最後一篇。 其中也會討論如何發佈網站 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 1e718c92a2f94df50fcf7af6859917746a4982ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="fea58-104">簡介使用 WebMatrix 中發行站台的 ASP.NET Web Pages-</span><span class="sxs-lookup"><span data-stu-id="fea58-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="fea58-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fea58-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fea58-106">本教學課程是導入了 ASP.NET Web Pages 和 Microsoft WebMatrix 的教學課程組中最後一篇。</span><span class="sxs-lookup"><span data-stu-id="fea58-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="fea58-107">它討論如何將您的站台發佈至網際網路，以便其他人可以使用它。</span><span class="sxs-lookup"><span data-stu-id="fea58-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="fea58-108">它會假設您已完成透過數列[建立 ASP.NET Web Pages 站台中的一致查詢](https://go.microsoft.com/fwlink/?LinkId=251585)。</span><span class="sxs-lookup"><span data-stu-id="fea58-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="fea58-109">您將學習如何發佈站台使用：</span><span class="sxs-lookup"><span data-stu-id="fea58-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="fea58-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fea58-110">Microsoft Azure</span></span>
> - <span data-ttu-id="fea58-111">Web 代管公司</span><span class="sxs-lookup"><span data-stu-id="fea58-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="fea58-112">關於發佈您的網站</span><span class="sxs-lookup"><span data-stu-id="fea58-112">About Publishing Your Site</span></span>

<span data-ttu-id="fea58-113">到目前為止，您所做過您的本機電腦上，包括測試您的網頁的所有工作。</span><span class="sxs-lookup"><span data-stu-id="fea58-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="fea58-114">若要執行您*.cshtml*頁面，您已使用的 WebMatrix，IIS Express 也就是內建的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fea58-114">To run your*.cshtml* pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="fea58-115">但當然沒有人可以看到您已建立您的站台。</span><span class="sxs-lookup"><span data-stu-id="fea58-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="fea58-116">若要讓其他人使用您的網站，您必須發佈至網際網路。</span><span class="sxs-lookup"><span data-stu-id="fea58-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="fea58-117">除非您已經有公用的 web 伺服器的存取權，表示您所擁有的帳戶具有發佈*雲端平台*或*主機服務提供者*。</span><span class="sxs-lookup"><span data-stu-id="fea58-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="fea58-118">雲端平台，例如 Microsoft Azure 提供您的應用程式視基礎結構。</span><span class="sxs-lookup"><span data-stu-id="fea58-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="fea58-119">主機服務提供者是的公司擁有可公開存取的網頁伺服器，並可將租用您網站的空間。</span><span class="sxs-lookup"><span data-stu-id="fea58-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="fea58-120">主控方案，執行從一個月數美元 （或甚至是免費） 小型網站數以百計的一個月的高容量商業網站 （美元）。</span><span class="sxs-lookup"><span data-stu-id="fea58-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="fea58-121">您可能必須透過您取得在家網際網路服務使用的網際網路服務提供者 (ISP) 的公用 web 伺服器的存取權。</span><span class="sxs-lookup"><span data-stu-id="fea58-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="fea58-122">不過，您的主機服務提供者必須支援 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="fea58-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="fea58-123">許多 Isp 不這麼做，但值得一律檢查。</span><span class="sxs-lookup"><span data-stu-id="fea58-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="fea58-124">在本教學課程中，我們會提供您如何發行的概觀。</span><span class="sxs-lookup"><span data-stu-id="fea58-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="fea58-125">因為在程序的位元與不同的每個主機服務提供者不適合所有項目，提供精確的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fea58-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="fea58-126">但是，您會取得建議的程序的運作方式。</span><span class="sxs-lookup"><span data-stu-id="fea58-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="fea58-127">本教學課程包含四個區段：</span><span class="sxs-lookup"><span data-stu-id="fea58-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="fea58-128">設定預設頁面</span><span class="sxs-lookup"><span data-stu-id="fea58-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="fea58-129">發行 （請選擇下列其中之一）</span><span class="sxs-lookup"><span data-stu-id="fea58-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="fea58-130">a.</span><span class="sxs-lookup"><span data-stu-id="fea58-130">a.</span></span> [<span data-ttu-id="fea58-131">您的站台發佈至 Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fea58-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="fea58-132">b.</span><span class="sxs-lookup"><span data-stu-id="fea58-132">b.</span></span> [<span data-ttu-id="fea58-133">您的網站發行至 Web 主控公司</span><span class="sxs-lookup"><span data-stu-id="fea58-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="fea58-134">更新即時網站： 重新發行</span><span class="sxs-lookup"><span data-stu-id="fea58-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="fea58-135">設定預設頁面</span><span class="sxs-lookup"><span data-stu-id="fea58-135">Setting up the default page</span></span>

<span data-ttu-id="fea58-136">當使用者巡覽至您的網站的基底位址時，您的站台的預設頁面會顯示給使用者。</span><span class="sxs-lookup"><span data-stu-id="fea58-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="fea58-137">例如，當先前的範例在 www.contoso.com 設定為預設的網頁站台，然後瀏覽至**www.contoso.com**瀏覽至相同**www.contoso.com/Default.htm**。</span><span class="sxs-lookup"><span data-stu-id="fea58-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to **www.contoso.com** is the same as navigating to **www.contoso.com/Default.htm**.</span></span>

<span data-ttu-id="fea58-138">目前，您的網站會使用**Default.cshtml**為預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="fea58-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="fea58-139">此頁面是適合您的預設頁面上，但在本教學課程您尚未加入任何內容至該頁面，它會顯示空白頁面。</span><span class="sxs-lookup"><span data-stu-id="fea58-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="fea58-140">開啟 Default.cshtml，並取代為下列程式碼中的內容。</span><span class="sxs-lookup"><span data-stu-id="fea58-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="fea58-141">現在您的網站可供發行集。</span><span class="sxs-lookup"><span data-stu-id="fea58-141">Now your site is ready for publication.</span></span> <span data-ttu-id="fea58-142">首先，您會看到如何將網站部署至 Azure，以及如何將它部署至 web 主控公司。</span><span class="sxs-lookup"><span data-stu-id="fea58-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="fea58-143">其中一個的選項適用於您的網站，而且您只需要遵循其中一種部署選項。</span><span class="sxs-lookup"><span data-stu-id="fea58-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="fea58-144">您的站台發佈至 Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="fea58-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="fea58-145">本教學課程會先顯示如何將您的網站部署至 Microsoft Azure。</span><span class="sxs-lookup"><span data-stu-id="fea58-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="fea58-146">Microsoft 帳戶登入，您可以在 Azure 上建立最多 10 個可用的站台。</span><span class="sxs-lookup"><span data-stu-id="fea58-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="fea58-147">這些可用的網站提供便利的方式來測試您的網站。</span><span class="sxs-lookup"><span data-stu-id="fea58-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="fea58-148">您隨時都可以刪除此範例站台，稍後若要避免使用所有可用的網站。</span><span class="sxs-lookup"><span data-stu-id="fea58-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="fea58-149">您可以建立免費的試用帳戶只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="fea58-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="fea58-150">如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="fea58-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="fea58-151">在 WebMatrix 功能區中，按一下**發行** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fea58-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

![WebMatrix 功能區中的 [發佈] 按鈕](publishing/_static/image1.png)

<span data-ttu-id="fea58-153">**發行您的站台**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="fea58-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="fea58-154">如果您有未登入您的 Microsoft 帳戶，對話方塊會包含**開始使用 Azure**連結。</span><span class="sxs-lookup"><span data-stu-id="fea58-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="fea58-155">按一下此連結。</span><span class="sxs-lookup"><span data-stu-id="fea58-155">Click this link.</span></span>

![發行您的網站](publishing/_static/image2.png)

<span data-ttu-id="fea58-157">如果您不具有 Microsoft 帳戶登入，您可以有一次機會來登入。</span><span class="sxs-lookup"><span data-stu-id="fea58-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="fea58-158">您必須發佈 azure 網站來登入 Microsoft 帳戶。</span><span class="sxs-lookup"><span data-stu-id="fea58-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![登入](publishing/_static/image3.png)

<span data-ttu-id="fea58-160">之後您的 Microsoft 帳戶登入，對話方塊會包含在 Azure 上建立新的網站，或連接到您現有的網站在 Azure 上的連結。</span><span class="sxs-lookup"><span data-stu-id="fea58-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![建立新站台](publishing/_static/image4.png)

<span data-ttu-id="fea58-162">選取**建立新的站台**。</span><span class="sxs-lookup"><span data-stu-id="fea58-162">Select **Create a new site**.</span></span>

<span data-ttu-id="fea58-163">如果您命名為您的專案**WebPagesMovies**，為您的網站的預設名稱會是**webpagesmovies.azurewebsites.net**。</span><span class="sxs-lookup"><span data-stu-id="fea58-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="fea58-164">這個預設名稱是最有可能無法使用，紅色的驚嘆號所指示。</span><span class="sxs-lookup"><span data-stu-id="fea58-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![預設的網站名稱](publishing/_static/image5.png)

<span data-ttu-id="fea58-166">將站台名稱變更為可用，並選取接近您位置的位置。</span><span class="sxs-lookup"><span data-stu-id="fea58-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![變更站台名稱](publishing/_static/image6.png)

<span data-ttu-id="fea58-168">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="fea58-168">Click **OK**.</span></span>

<span data-ttu-id="fea58-169">WebMatrix performss 測試來判斷伺服器是否與您的網站相容。</span><span class="sxs-lookup"><span data-stu-id="fea58-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![相容性測試](publishing/_static/image7.png)

<span data-ttu-id="fea58-171">選取**繼續**。</span><span class="sxs-lookup"><span data-stu-id="fea58-171">Select **Continue**.</span></span>

<span data-ttu-id="fea58-172">會顯示相容性測試的結果。</span><span class="sxs-lookup"><span data-stu-id="fea58-172">The results of the compatibility test are displayed.</span></span>

![相容性結果](publishing/_static/image8.png)

<span data-ttu-id="fea58-174">選取**繼續**。</span><span class="sxs-lookup"><span data-stu-id="fea58-174">Select **Continue**.</span></span>

<span data-ttu-id="fea58-175">WebMatrix 會顯示檔案和資料庫，將會發行至網站。</span><span class="sxs-lookup"><span data-stu-id="fea58-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="fea58-176">由於這是您要發佈的站台的第一次時，會列出所有的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="fea58-177">您可以取消核取尚未準備好要發行的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="fea58-178">在後續的發行集，會顯示已變更的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="fea58-179">請參閱[更新即時網站： 重新發行](#update)。</span><span class="sxs-lookup"><span data-stu-id="fea58-179">See [Updating the Live Site: Republishing](#update).</span></span>

![發行預覽](publishing/_static/image9.png)

<span data-ttu-id="fea58-181">選取**繼續**。</span><span class="sxs-lookup"><span data-stu-id="fea58-181">Select **Continue**.</span></span>

<span data-ttu-id="fea58-182">已將網站部署至 Azure 之後，會顯示訊息，指出已完成部署。</span><span class="sxs-lookup"><span data-stu-id="fea58-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![發行完成](publishing/_static/image10.png)

<span data-ttu-id="fea58-184">您的網站和資料庫已經發行至 Azure，而現在提供給大眾使用。</span><span class="sxs-lookup"><span data-stu-id="fea58-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="fea58-185">按一下訊息，指出發行完成，而且您現在可以看到您已部署的網站中的連結。</span><span class="sxs-lookup"><span data-stu-id="fea58-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="fea58-186">您或任何具有網際網路存取權的人可以新增或修改資料庫中的記錄。</span><span class="sxs-lookup"><span data-stu-id="fea58-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="fea58-187">您的網站發行至 Web 主控公司</span><span class="sxs-lookup"><span data-stu-id="fea58-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="fea58-188">如果您決定將發佈到 Azure，您可以改為虛擬主機公司來發行您的網站。</span><span class="sxs-lookup"><span data-stu-id="fea58-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="fea58-189">按一下**尋找虛擬主機**連結。</span><span class="sxs-lookup"><span data-stu-id="fea58-189">Click the **Find web hosting** link.</span></span>

![發行設定 對話方塊中的 '到 web 裝載' 按鈕](publishing/_static/image12.png)

<span data-ttu-id="fea58-191">列出主機服務提供者支援 ASP.NET 的 Microsoft 網站上移至頁面。</span><span class="sxs-lookup"><span data-stu-id="fea58-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![列出主機服務提供者的 Microsoft 網站上的頁面](publishing/_static/image13.png)

<span data-ttu-id="fea58-193">很明顯地，可能很難知道現在完全透過長期來看，您可能需要哪些主控功能。</span><span class="sxs-lookup"><span data-stu-id="fea58-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="fea58-194">以下是幾個要考慮的事項：</span><span class="sxs-lookup"><span data-stu-id="fea58-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="fea58-195">基於 WebPagesMovies 站台的詳細資訊，您不需要個別的附加元件的 SQL Server，通常的額外成本。</span><span class="sxs-lookup"><span data-stu-id="fea58-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="fea58-196">在您的網站，您使用 SQL Server Compact Edition，這各自獨立。</span><span class="sxs-lookup"><span data-stu-id="fea58-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="fea58-197">不過，您可能需要存取 SQL Server 的某些未來網站工作。</span><span class="sxs-lookup"><span data-stu-id="fea58-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="fea58-198">如果您認為您可能，請確定您可以稍後新增 SQL Server 功能。</span><span class="sxs-lookup"><span data-stu-id="fea58-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="fea58-199">請檢查主機服務提供者是否支援 Web Deploy 發行通訊協定。</span><span class="sxs-lookup"><span data-stu-id="fea58-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="fea58-200">您可以使用 FTP 通訊協定，發行，但更方便地使用 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="fea58-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="fea58-201">某些網站提供免費的試用期間。</span><span class="sxs-lookup"><span data-stu-id="fea58-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="fea58-202">免費試用版是一個好的方法，再試一次發行並同時裝載正在仍實驗 WebMatrix 及 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="fea58-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="fea58-203">選取您要的其中一個。</span><span class="sxs-lookup"><span data-stu-id="fea58-203">Pick one that you like.</span></span> <span data-ttu-id="fea58-204">此教學課程中，我們會選取 DiscountASP.NET，因為我們已建立本教學課程，而該公司促銷，讓使用者在裝載站台的幾個月免費。</span><span class="sxs-lookup"><span data-stu-id="fea58-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="fea58-205">我們選擇的主機服務提供者在此教學課程不應解譯為該公司支持，透過任何其他。</span><span class="sxs-lookup"><span data-stu-id="fea58-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="fea58-206">我們必須挑選一個圖中，但 DiscountASP.NET 是其中一個支援的 ASP.NET Web Pages 和 Web Deploy 通訊協定發行許多公司。</span><span class="sxs-lookup"><span data-stu-id="fea58-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="fea58-207">一般而言，您已經註冊過主機服務提供者之後，公司會傳送電子郵件，其中包含使用者名稱和密碼，網頁伺服器等的 URL。</span><span class="sxs-lookup"><span data-stu-id="fea58-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="fea58-208">如果代管公司會支援 Web Deploy 通訊協定，他們可能會傳送您所在的檔案發行設定，或可讓您下載一個。</span><span class="sxs-lookup"><span data-stu-id="fea58-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="fea58-209">發行設定檔讓您簡化程序。</span><span class="sxs-lookup"><span data-stu-id="fea58-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="fea58-210">當您已經註冊，並準備好要發行時，按一下**發行**WebMatrix 功能區中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="fea58-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="fea58-211">**發行設定**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="fea58-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="fea58-212">如果主機服務提供者傳送給您的發行設定檔，按一下**匯入發行設定**連結，並將檔案匯入。</span><span class="sxs-lookup"><span data-stu-id="fea58-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="fea58-213">如果您還沒有的發行設定檔，請使用代管公司傳送給您電子郵件中的值填寫的欄位。</span><span class="sxs-lookup"><span data-stu-id="fea58-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="fea58-214">以下是**發行設定**對話方塊看起來會像當您完成：</span><span class="sxs-lookup"><span data-stu-id="fea58-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![填入發行設定 對話方塊中的發行設定](publishing/_static/image14.png)

<span data-ttu-id="fea58-216">按一下**驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="fea58-216">Click **Validate Connection**.</span></span> <span data-ttu-id="fea58-217">如果所有項目是 確定，對話方塊報告**已成功連接**，這表示它可以與裝載提供者的伺服器通訊。</span><span class="sxs-lookup"><span data-stu-id="fea58-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![成功訊息，如果將發行設定正確無誤](publishing/_static/image15.png)

<span data-ttu-id="fea58-219">如果沒有問題，WebMatrix 會會盡可能地告訴您此問題：</span><span class="sxs-lookup"><span data-stu-id="fea58-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![如果沒有發生問題的錯誤訊息發行設定](publishing/_static/image16.png)

<span data-ttu-id="fea58-221">按一下**儲存**以儲存設定。</span><span class="sxs-lookup"><span data-stu-id="fea58-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="fea58-222">若要執行測試，以確定它可以正確通訊與裝載站台，WebMatrix 提供：</span><span class="sxs-lookup"><span data-stu-id="fea58-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![執行測試，在發行過程中提供的訊息](publishing/_static/image17.png)

<span data-ttu-id="fea58-224">按一下 [ **是**]。</span><span class="sxs-lookup"><span data-stu-id="fea58-224">Click **Yes**.</span></span> <span data-ttu-id="fea58-225">WebMatrix 會上載至主機服務提供者的某些範例檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="fea58-226">當相容性測試完成時，WebMatrix 會報告結果：</span><span class="sxs-lookup"><span data-stu-id="fea58-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![發行測試結果](publishing/_static/image18.png)

<span data-ttu-id="fea58-228">如果您準備好移，請繼續進行，然後按一下**繼續**實際啟動發行程序。</span><span class="sxs-lookup"><span data-stu-id="fea58-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="fea58-229">WebMatrix 找出哪些檔案位於您的網站和已存在的主機伺服器 （現在無） 以及發行程序的預覽可讓您：</span><span class="sxs-lookup"><span data-stu-id="fea58-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![哪些發行程序都會將上傳的檔案的預覽](publishing/_static/image19.png)

<span data-ttu-id="fea58-231">要發行的檔案清單包含您已建立像網頁*Movies.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="fea58-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="fea58-232">清單也包含協助程式，您已安裝，為您的資料庫執行 SQL Server Compact Edition 等檔案的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="fea58-233">如此一來，初始發行處理程序很嚴重。</span><span class="sxs-lookup"><span data-stu-id="fea58-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="fea58-234">按一下 [ **繼續**]。</span><span class="sxs-lookup"><span data-stu-id="fea58-234">Click **Continue**.</span></span> <span data-ttu-id="fea58-235">WebMatrix 會將您的檔案複製到裝載提供者的伺服器。</span><span class="sxs-lookup"><span data-stu-id="fea58-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="fea58-236">完成時，狀態列會報告結果：</span><span class="sxs-lookup"><span data-stu-id="fea58-236">When it's done, the results are reported in the status bar:</span></span>

![狀態列訊息發行程序成功完成時](publishing/_static/image20.png)

<span data-ttu-id="fea58-238">若要查看您即時網站，請按一下 [狀態] 列中的連結。</span><span class="sxs-lookup"><span data-stu-id="fea58-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="fea58-239">新增*電影*至的 URL，您將會看到*Movies.cshtml*您所建立的檔案：</span><span class="sxs-lookup"><span data-stu-id="fea58-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![顯示 [影片] 頁面上的即時網站](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="fea58-241">更新即時網站： 重新發行</span><span class="sxs-lookup"><span data-stu-id="fea58-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="fea58-242">一旦您已發行您的網站 （到 Azure 或虛擬主機公司），有兩份&mdash;電腦及服務提供者上的版本上的版本。</span><span class="sxs-lookup"><span data-stu-id="fea58-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="fea58-243">您可能會想要繼續開發網站 (如果沒有其他資料，做為下一個教學課程組的一部分)。</span><span class="sxs-lookup"><span data-stu-id="fea58-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="fea58-244">當您這樣做時，您必須重新發佈您的網站，以將變更從您的電腦複製到服務提供者。</span><span class="sxs-lookup"><span data-stu-id="fea58-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="fea58-245">在 WebMatrix 中發行流程可以判斷檔案已變更您的站台上，並發佈僅擁有的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="fea58-246">若要重新發佈的運作方式，請開啟*Movies.cshtml*站台，進行一些小型的變更，並儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="fea58-247">例如，將標題變更為`Movies - Updated`。</span><span class="sxs-lookup"><span data-stu-id="fea58-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="fea58-248">按一下**發行**功能區按鈕。</span><span class="sxs-lookup"><span data-stu-id="fea58-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="fea58-249">WebMatrix 可決定變更，並顯示您的檔案，它會將發佈的預覽。</span><span class="sxs-lookup"><span data-stu-id="fea58-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![[發行] 對話方塊，顯示已變更檔案隨時可供重新發行](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="fea58-251">根據預設，WebMatrix 發行您的資料庫 (*.sdf*檔案) 只在第一次發行站台。</span><span class="sxs-lookup"><span data-stu-id="fea58-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="fea58-252">一旦發行您的網站，並與網站互動的人員，即時網站上的資料庫通常有站台的實際資料。</span><span class="sxs-lookup"><span data-stu-id="fea58-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="fea58-253">您一定要非常小心，不要覆寫即時資料庫與*.sdf*在您的電腦，通常只包含測試資料上的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="fea58-254">這就是為什麼您看到警告**發行將會覆寫任何遠端資料庫**，為什麼的核取方塊和*WebPagesMovies.sdf*預設為清除。</span><span class="sxs-lookup"><span data-stu-id="fea58-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="fea58-255">按一下 [ **繼續**]。</span><span class="sxs-lookup"><span data-stu-id="fea58-255">Click **Continue**.</span></span> <span data-ttu-id="fea58-256">WebMatrix 發行變更的檔案，並顯示成功訊息，如同您發行的第一次。</span><span class="sxs-lookup"><span data-stu-id="fea58-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="fea58-257">移至即時網站 （您可以按一下連結成功訊息中的如果仍然顯示），並確認您的變更已發行。</span><span class="sxs-lookup"><span data-stu-id="fea58-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="fea58-258">**從遠端編輯檔案**</span><span class="sxs-lookup"><span data-stu-id="fea58-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="fea58-259">變更您的網站，然後再重新發佈的替代方案，您可以編輯直接在 WebMatrix 中的遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="fea58-260">在此案例中，開啟位於服務提供者的檔案和 WebMatrix 下載複製它供您編輯。</span><span class="sxs-lookup"><span data-stu-id="fea58-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="fea58-261">每次您儲存檔案時，WebMatrix 會將變更傳送至站台。</span><span class="sxs-lookup"><span data-stu-id="fea58-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="fea58-262">遠端編輯是簡單的方式，可讓您即時網站的變更。</span><span class="sxs-lookup"><span data-stu-id="fea58-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="fea58-263">不過，您進行這種方式的變更未同步您的本機網站中的檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="fea58-264">若要同步的本機檔案與遠端站台，您可以下載的遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="fea58-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="fea58-265">此程序的作用很像發行，除了以反向方向。</span><span class="sxs-lookup"><span data-stu-id="fea58-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="fea58-266">我們將不會更說明有關遠端編輯和遠端下載設備的 WebMatrix 這裡。</span><span class="sxs-lookup"><span data-stu-id="fea58-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="fea58-267">它們很多人有不同的電腦上相同的網站上運作的很有用。</span><span class="sxs-lookup"><span data-stu-id="fea58-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="fea58-268">如需詳細資訊，請參閱[發行並編輯使用 WebMatrix 2 Beta 的遠端站台](https://go.microsoft.com/fwlink/?LinkId=251591)。</span><span class="sxs-lookup"><span data-stu-id="fea58-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="fea58-269">其他資源</span><span class="sxs-lookup"><span data-stu-id="fea58-269">Additional Resources</span></span>

- <span data-ttu-id="fea58-270">[ASP.NET WebMatrix ASP.NET Web Pages 論壇](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)，不妨張貼問題和取得解答。</span><span class="sxs-lookup"><span data-stu-id="fea58-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="fea58-271">上一步</span><span class="sxs-lookup"><span data-stu-id="fea58-271">Previous</span></span>](layouts.md)
