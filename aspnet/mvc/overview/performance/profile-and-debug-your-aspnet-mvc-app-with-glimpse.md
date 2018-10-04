---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: 分析與偵錯您的 ASP.NET MVC 應用程式，使用 Glimpse |Microsoft Docs
author: Rick-Anderson
description: Glimpse 是蓬勃發展及成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯及適用於 ASP.NET 的診斷資訊...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577284"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="3b536-103">分析與偵錯您的 ASP.NET MVC 應用程式，使用 Glimpse</span><span class="sxs-lookup"><span data-stu-id="3b536-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="3b536-104">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="3b536-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="3b536-105">Glimpse 是蓬勃發展及成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯和診斷資訊的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b536-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="3b536-106">它是一般安裝、 輕量級、 超快，並在每一頁底部顯示關鍵效能度量。</span><span class="sxs-lookup"><span data-stu-id="3b536-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="3b536-107">它可讓您向下切入至您的應用程式時您需要了解發生什麼情況在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="3b536-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="3b536-108">Glimpse 提供更有價值的資訊我們建議您在您的開發週期，包括您的 Azure 測試環境中使用它。</span><span class="sxs-lookup"><span data-stu-id="3b536-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="3b536-109">雖然[Fiddler](http://www.telerik.com/fiddler)並[F-12 開發工具](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)提供用戶端檢視 Glimpse 並提供從伺服器的詳細的檢視。</span><span class="sxs-lookup"><span data-stu-id="3b536-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="3b536-110">本教學課程著重於使用初探 ASP.NET MVC 和 EF 的套件，但許多其他套件可供使用。</span><span class="sxs-lookup"><span data-stu-id="3b536-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="3b536-111">可能的話，我會連結至適當[Glimpse docs](http://getglimpse.com/Docs/)我協助維護。</span><span class="sxs-lookup"><span data-stu-id="3b536-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="3b536-112">Glimpse 是開放原始碼專案，您也可以參與的原始程式碼和文件。</span><span class="sxs-lookup"><span data-stu-id="3b536-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="3b536-113">安裝初探</span><span class="sxs-lookup"><span data-stu-id="3b536-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="3b536-114">啟用 localhost 的初探</span><span class="sxs-lookup"><span data-stu-id="3b536-114">Enable Glimpse for localhost</span></span>](#eg)
- <span data-ttu-id="3b536-115">[[時間軸] 索引標籤](#Time)</span><span class="sxs-lookup"><span data-stu-id="3b536-115">[The Timeline tab](#Time)</span></span>
- [<span data-ttu-id="3b536-116">模型繫結</span><span class="sxs-lookup"><span data-stu-id="3b536-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="3b536-117">路由</span><span class="sxs-lookup"><span data-stu-id="3b536-117">Routes</span></span>](#route)
- [<span data-ttu-id="3b536-118">在 Azure 上使用初探</span><span class="sxs-lookup"><span data-stu-id="3b536-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="3b536-119">其他資源</span><span class="sxs-lookup"><span data-stu-id="3b536-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="3b536-120">安裝初探</span><span class="sxs-lookup"><span data-stu-id="3b536-120">Installing Glimpse</span></span>

<span data-ttu-id="3b536-121">您可以從 NuGet 套件管理員主控台或從安裝 Glimpse**管理 NuGet 套件**主控台。</span><span class="sxs-lookup"><span data-stu-id="3b536-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="3b536-122">對於這個示範，我要安裝的 Mvc5 和 EF6 封裝：</span><span class="sxs-lookup"><span data-stu-id="3b536-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![從 NuGet Dlg 安裝初探](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="3b536-124">搜尋*Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="3b536-124">Search for *Glimpse.EF*</span></span>

![從 NuGet 安裝 dlg Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="3b536-126">藉由選取**已安裝的套件**，您可以看到已安裝的初探相依模組：</span><span class="sxs-lookup"><span data-stu-id="3b536-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![從 DLg 安裝 Glimpse 套件](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="3b536-128">下列命令會從套件管理員主控台安裝 Glimpse MVC5 和 EF6 的模組：</span><span class="sxs-lookup"><span data-stu-id="3b536-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="3b536-129">啟用 localhost 的初探</span><span class="sxs-lookup"><span data-stu-id="3b536-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="3b536-130">瀏覽至 http://localhost:&lt; 連接埠 #&gt;/glimpse.axd，然後按一下<strong>開啟 Glimpse</strong>  按鈕。</span><span class="sxs-lookup"><span data-stu-id="3b536-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Glimpse axd-isapid-2.0-64 頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="3b536-132">如果您有我的最愛列顯示，您可以拖放 Glimpse 按鈕並將它們新增為 bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="3b536-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![使用 Glimpse boookmarklets IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="3b536-134">您現在可以瀏覽您的應用程式，而**抬頭顯示器**（抬頭顯示器） 會顯示在頁面底部。</span><span class="sxs-lookup"><span data-stu-id="3b536-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![抬頭顯示器 contact Manager 頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="3b536-136">[Glimpse 抬頭顯示器頁面](http://getglimpse.com/Docs/Heads-up-Display)詳述如上所示的計時資訊。</span><span class="sxs-lookup"><span data-stu-id="3b536-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="3b536-137">不顯眼的效能資料抬頭顯示器的顯示畫面可以立即通知您的問題-才能進入測試循環。</span><span class="sxs-lookup"><span data-stu-id="3b536-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="3b536-138">按一下&quot;g&quot;右上角顯示 Glimpse 面板：</span><span class="sxs-lookup"><span data-stu-id="3b536-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse 面板](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="3b536-140">在上圖中， [ 索引標籤執行](http://getglimpse.com/Docs/Execution-Tab)選取時，其中顯示管線中的動作和篩選條件的計時詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3b536-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="3b536-141">您可以看到我[停止監看式篩選計時器](http://www.nuget.org/packages/StopWatch/)開始 管線階段 6。</span><span class="sxs-lookup"><span data-stu-id="3b536-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="3b536-142">雖然我的輕量計時器可以提供有用的設定檔/計時資料，它會遺漏所有的時間花費在記憶體授權，並呈現檢視。</span><span class="sxs-lookup"><span data-stu-id="3b536-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="3b536-143">您可以閱讀我在計時器[設定檔，然後在您的 ASP.NET MVC 應用程式，一直到 Azure 的時間](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3b536-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="3b536-144">[索引標籤](http://getglimpse.com/Docs/Tabs)頁面提供每個索引標籤的詳細資訊的連結。</span><span class="sxs-lookup"><span data-stu-id="3b536-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="3b536-145">[時間軸] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="3b536-145">The Timeline tab</span></span>

<span data-ttu-id="3b536-146">我已修改 Tom Dykstra 傑出[EF 6/MVC 5 的教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)為下列程式碼中，變更為 instructors 控制器：</span><span class="sxs-lookup"><span data-stu-id="3b536-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="3b536-147">上述程式碼可讓我以查詢字串來傳遞 (`eager`) 來控制 eager 或明確載入資料。</span><span class="sxs-lookup"><span data-stu-id="3b536-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="3b536-148">在下圖中，使用明確式載入和執行時間 頁面會顯示在載入每個註冊`Index`動作方法：</span><span class="sxs-lookup"><span data-stu-id="3b536-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Explicit Loading - 明確載入](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="3b536-150">下列程式碼中，指定積極式，且每個註冊會擷取之後`Index`檢視則稱為：</span><span class="sxs-lookup"><span data-stu-id="3b536-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![立即指定](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="3b536-152">您可以暫留在某個時間區段，以取得詳細的計時資訊：</span><span class="sxs-lookup"><span data-stu-id="3b536-152">You can hover over a time segment to get detailed timing information:</span></span>

![暫留以查看詳細的計時](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="3b536-154">模型繫結</span><span class="sxs-lookup"><span data-stu-id="3b536-154">Model Binding</span></span>

<span data-ttu-id="3b536-155">[模型繫結索引標籤](http://getglimpse.com/Docs/Model-Binding-Tab)提供豐富的資訊可協助您了解您的表單變數繫結的方式，以及為什麼某些未繫結如預期般。</span><span class="sxs-lookup"><span data-stu-id="3b536-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="3b536-156">下的圖顯示**嗎？**</span><span class="sxs-lookup"><span data-stu-id="3b536-156">The image below shows the **?**</span></span> <span data-ttu-id="3b536-157">圖示，您可以按一下以顯示該功能的初探說明頁面。</span><span class="sxs-lookup"><span data-stu-id="3b536-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![一窺模型繫結檢視](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="3b536-159">路由</span><span class="sxs-lookup"><span data-stu-id="3b536-159">Routes</span></span>

 <span data-ttu-id="3b536-160">Glimpse 路由索引標籤將可協助您偵錯和了解路由。</span><span class="sxs-lookup"><span data-stu-id="3b536-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="3b536-161">下圖中選取產品路由 （和會呈現綠色，一窺慣例）。</span><span class="sxs-lookup"><span data-stu-id="3b536-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="3b536-162">![選取的產品名稱](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)路由條件約束、 區域和資料語彙基元也會顯示。</span><span class="sxs-lookup"><span data-stu-id="3b536-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="3b536-163">請參閱[Glimpse 路由](http://getglimpse.com/Docs/Routes-Tab)並[ASP.NET MVC 5 中的屬性路由](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3b536-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="3b536-164">在 Azure 上使用初探</span><span class="sxs-lookup"><span data-stu-id="3b536-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="3b536-165">Glimpse 預設安全性原則只允許從本機主機顯示 Glimpse 資料。</span><span class="sxs-lookup"><span data-stu-id="3b536-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="3b536-166">您可以變更此安全性原則，讓您可以檢視這項資料 （例如在 Azure 上的 web 應用程式） 的遠端伺服器上。</span><span class="sxs-lookup"><span data-stu-id="3b536-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="3b536-167">針對在 Azure 上的測試環境，新增 反白顯示的標記到底部*web.confg*檔案以啟用 Glimpse:</span><span class="sxs-lookup"><span data-stu-id="3b536-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="3b536-168">透過此單獨的變更，任何使用者可以在遠端站台上查看 Glimpse 資料。</span><span class="sxs-lookup"><span data-stu-id="3b536-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="3b536-169">請考慮將上述的標記新增至發行設定檔，因此它只部署套用時使用的發行設定檔的 (程式比方說，是 Azure 的測試 proifle。)若要限制 Glimpse 資料，我們會新增`canViewGlimpseData`角色而只允許這個角色，才能檢視 Glimpse 資料的使用者。</span><span class="sxs-lookup"><span data-stu-id="3b536-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="3b536-170">移除的註解*GlimpseSecurityPolicy.cs*檔案，並變更[IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)從呼叫`Administrator`至`canViewGlimpseData`角色：</span><span class="sxs-lookup"><span data-stu-id="3b536-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="3b536-171">安全性-Glimpse 所提供的豐富資料可能會公開您的應用程式的安全性。</span><span class="sxs-lookup"><span data-stu-id="3b536-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="3b536-172">Microsoft 不用於生產應用程式上執行安全性稽核的初探。</span><span class="sxs-lookup"><span data-stu-id="3b536-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="3b536-173">在 新增角色的資訊，請參閱我[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 web 應用程式部署至 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)教學課程。</span><span class="sxs-lookup"><span data-stu-id="3b536-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="3b536-174">其他資源</span><span class="sxs-lookup"><span data-stu-id="3b536-174">Additional Resources</span></span>

- [<span data-ttu-id="3b536-175">將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="3b536-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="3b536-176">[一窺組態](http://getglimpse.com/Docs/Configuration)-文件頁面上設定索引標籤、 執行階段原則、 記錄和更多功能。</span><span class="sxs-lookup"><span data-stu-id="3b536-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
