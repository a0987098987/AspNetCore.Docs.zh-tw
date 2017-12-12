---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "設定檔，然後使用 glimpse 進行 ASP.NET MVC 應用程式進行偵錯 |Microsoft 文件"
author: Rick-Anderson
description: "初探是蓬勃發展和成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯和 ASP.NET 的診斷資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 98b21a54ba00a8c82c3be7ba4e39d44041ed42c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="d06b5-103">分析和偵錯使用 glimpse 進行 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="d06b5-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="d06b5-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d06b5-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="d06b5-105">初探是蓬勃發展和成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯和診斷資訊的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d06b5-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="d06b5-106">它是 trivial 安裝、 輕量級超快，並在每一頁底部顯示關鍵效能度量。</span><span class="sxs-lookup"><span data-stu-id="d06b5-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="d06b5-107">它可讓您向下鑽研至您的應用程式時您需要了解在伺服器上運作。</span><span class="sxs-lookup"><span data-stu-id="d06b5-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="d06b5-108">初探提供許多有用的資訊我們建議您在整個開發週期，包括 Azure 測試環境使用它。</span><span class="sxs-lookup"><span data-stu-id="d06b5-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="d06b5-109">雖然[Fiddler](http://www.telerik.com/fiddler)和[F-12 開發工具](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx)提供用戶端 檢視中，瀏覽提供從伺服器的詳細的檢視。</span><span class="sxs-lookup"><span data-stu-id="d06b5-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="d06b5-110">本教學課程著重於使用 glimpse 進行 ASP.NET MVC 和 EF 封裝，但是許多其他封裝可以使用。</span><span class="sxs-lookup"><span data-stu-id="d06b5-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="d06b5-111">盡可能將連結至適當[以前並未使用過的文件](http://getglimpse.com/Docs/)這可協助維護。</span><span class="sxs-lookup"><span data-stu-id="d06b5-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="d06b5-112">初探開放原始碼專案，您太可以參與原始碼及文件。</span><span class="sxs-lookup"><span data-stu-id="d06b5-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="d06b5-113">安裝初探</span><span class="sxs-lookup"><span data-stu-id="d06b5-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="d06b5-114">啟用初探 localhost</span><span class="sxs-lookup"><span data-stu-id="d06b5-114">Enable Glimpse for localhost</span></span>](#eg)
- <span data-ttu-id="d06b5-115">[[時間軸] 索引標籤](#Time)</span><span class="sxs-lookup"><span data-stu-id="d06b5-115">[The Timeline tab](#Time)</span></span>
- [<span data-ttu-id="d06b5-116">模型繫結</span><span class="sxs-lookup"><span data-stu-id="d06b5-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="d06b5-117">路由</span><span class="sxs-lookup"><span data-stu-id="d06b5-117">Routes</span></span>](#route)
- [<span data-ttu-id="d06b5-118">在 Azure 上使用 glimpse 進行</span><span class="sxs-lookup"><span data-stu-id="d06b5-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="d06b5-119">其他資源</span><span class="sxs-lookup"><span data-stu-id="d06b5-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="d06b5-120">安裝初探</span><span class="sxs-lookup"><span data-stu-id="d06b5-120">Installing Glimpse</span></span>

<span data-ttu-id="d06b5-121">您可以在從 NuGet 封裝管理員主控台或從安裝初探**管理 NuGet 封裝**主控台。</span><span class="sxs-lookup"><span data-stu-id="d06b5-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="d06b5-122">對於這個示範，我要安裝的 Mvc5 和 EF6 封裝：</span><span class="sxs-lookup"><span data-stu-id="d06b5-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![從 NuGet Dlg 安裝初探](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="d06b5-124">搜尋*Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="d06b5-124">Search for *Glimpse.EF*</span></span>

![從 NuGet 安裝新細明體 Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="d06b5-126">藉由選取**安裝封裝**，您可以查看已安裝的 Glimpse 相依模組：</span><span class="sxs-lookup"><span data-stu-id="d06b5-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![新細明體從已安裝瀏覽封裝](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="d06b5-128">下列命令會從封裝管理員主控台安裝初探 MVC5 和 EF6 模組：</span><span class="sxs-lookup"><span data-stu-id="d06b5-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="d06b5-129">啟用初探 localhost</span><span class="sxs-lookup"><span data-stu-id="d06b5-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="d06b5-130">瀏覽至 http://localhost:&lt;連接埠 #&gt;/glimpse.axd，然後按一下**開啟初探上** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d06b5-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![初探 axd-isapid-2.0-64 頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="d06b5-132">如果您有我的最愛列顯示，您可以拖曳和卸除的瀏覽按鈕並將其新增為 bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="d06b5-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![使用 glimpse 進行 boookmarklets IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="d06b5-134">您現在可以瀏覽您的應用程式和**抬頭顯示器**（抬頭顯示器） 會顯示在頁面底部。</span><span class="sxs-lookup"><span data-stu-id="d06b5-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![抬頭顯示器與連絡人管理員頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="d06b5-136">[初探抬頭顯示器頁面](http://getglimpse.com/Docs/Heads-up-Display)詳細說明以上所示的計時資訊。</span><span class="sxs-lookup"><span data-stu-id="d06b5-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="d06b5-137">不顯眼的效能資料抬頭顯示器顯示會通知您發生問題的立即-之前測試週期。</span><span class="sxs-lookup"><span data-stu-id="d06b5-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="d06b5-138">按一下&quot;g&quot;在右下角會顯示初探面板：</span><span class="sxs-lookup"><span data-stu-id="d06b5-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![初探面板](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="d06b5-140">在上圖中，[執行 索引標籤](http://getglimpse.com/Docs/Execution-Tab)選取時，會顯示在管線中的動作和篩選的計時詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d06b5-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="d06b5-141">您可以看到我[停止監看式篩選器計時器](http://www.nuget.org/packages/StopWatch/)在管線階段 6 開始。</span><span class="sxs-lookup"><span data-stu-id="d06b5-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="d06b5-142">雖然我輕量型計時器可以提供有用的設定檔計時資料，它會遺漏所有時間花在授權和呈現檢視。</span><span class="sxs-lookup"><span data-stu-id="d06b5-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="d06b5-143">您可以閱讀我在計時器[分析和時間 ASP.NET MVC 應用程式到 Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d06b5-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="d06b5-144">[索引標籤](http://getglimpse.com/Docs/Tabs)頁面每個索引標籤會提供詳細資訊連結。</span><span class="sxs-lookup"><span data-stu-id="d06b5-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="d06b5-145">[時間軸] 索引標籤</span><span class="sxs-lookup"><span data-stu-id="d06b5-145">The Timeline tab</span></span>

<span data-ttu-id="d06b5-146">修改了 Tom Dykstra 尚未處理完畢[EF 6 MVC 5 教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)為下列程式碼變更講師控制站：</span><span class="sxs-lookup"><span data-stu-id="d06b5-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="d06b5-147">上述程式碼可讓我傳入查詢字串 (`eager`) 來控制 eager 或明確載入的資料。</span><span class="sxs-lookup"><span data-stu-id="d06b5-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="d06b5-148">在下面的影像中，會使用明確式載入和計時 頁面會顯示在載入每個註冊`Index`動作方法：</span><span class="sxs-lookup"><span data-stu-id="d06b5-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Explicit Loading - 明確載入](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="d06b5-150">下列程式碼中，指定 eager，且每個註冊會擷取之後`Index`檢視則稱為：</span><span class="sxs-lookup"><span data-stu-id="d06b5-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager 會指定](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="d06b5-152">您可以將滑鼠停留在時間區段，以取得詳細的計時資訊：</span><span class="sxs-lookup"><span data-stu-id="d06b5-152">You can hover over a time segment to get detailed timing information:</span></span>

![若要查看詳細的計時的動態顯示](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="d06b5-154">模型繫結</span><span class="sxs-lookup"><span data-stu-id="d06b5-154">Model Binding</span></span>

<span data-ttu-id="d06b5-155">[模型繫結索引標籤](http://getglimpse.com/Docs/Model-Binding-Tab)提供豐富的資訊可協助您了解您的表單變數的繫結的方式，以及為什麼有些不會繫結所預期。</span><span class="sxs-lookup"><span data-stu-id="d06b5-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="d06b5-156">下的圖顯示**嗎？**</span><span class="sxs-lookup"><span data-stu-id="d06b5-156">The image below shows the **?**</span></span> <span data-ttu-id="d06b5-157">圖示，您可以按一下以顯示該功能的瀏覽說明頁面。</span><span class="sxs-lookup"><span data-stu-id="d06b5-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![繫結檢視的瀏覽模型](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="d06b5-159">路由</span><span class="sxs-lookup"><span data-stu-id="d06b5-159">Routes</span></span>

 <span data-ttu-id="d06b5-160">初探路由索引標籤將可協助您偵錯和了解路由。</span><span class="sxs-lookup"><span data-stu-id="d06b5-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="d06b5-161">在下列影像中，選取產品路由，並會呈現綠色，初探慣例。</span><span class="sxs-lookup"><span data-stu-id="d06b5-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="d06b5-162">![選取的產品名稱](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)路由條件約束、 區域和資料語彙基元也會顯示。</span><span class="sxs-lookup"><span data-stu-id="d06b5-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="d06b5-163">請參閱[初探路由](http://getglimpse.com/Docs/Routes-Tab)和[屬性路由在 ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="d06b5-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="d06b5-164">在 Azure 上使用 glimpse 進行</span><span class="sxs-lookup"><span data-stu-id="d06b5-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="d06b5-165">初探預設的安全性原則只允許從本機主機要顯示的資料瀏覽。</span><span class="sxs-lookup"><span data-stu-id="d06b5-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="d06b5-166">您可以變更此安全性原則，讓您可以檢視這項資料 （例如在 Azure 上的 web 應用程式） 的遠端伺服器上。</span><span class="sxs-lookup"><span data-stu-id="d06b5-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="d06b5-167">對於在 Azure 上的測試環境，新增到底部的反白顯示的標記*web.confg*檔案，以啟用瀏覽：</span><span class="sxs-lookup"><span data-stu-id="d06b5-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="d06b5-168">此單獨的變更，任何使用者可以看到遠端站台上的瀏覽資料。</span><span class="sxs-lookup"><span data-stu-id="d06b5-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="d06b5-169">請考慮將上方的標記加入至發行設定檔，因此有只部署套用時使用的發行設定檔的 (程式例如，是 Azure 測試 proifle。)若要限制瀏覽資料，我們將加入`canViewGlimpseData`角色並只允許使用者在此角色，以檢視瀏覽資料。</span><span class="sxs-lookup"><span data-stu-id="d06b5-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="d06b5-170">移除從註解*GlimpseSecurityPolicy.cs*檔案，並且變更[IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)從呼叫`Administrator`至`canViewGlimpseData`角色：</span><span class="sxs-lookup"><span data-stu-id="d06b5-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="d06b5-171">安全性-瀏覽所提供的豐富資料可能會公開您的應用程式的安全性。</span><span class="sxs-lookup"><span data-stu-id="d06b5-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="d06b5-172">Microsoft 不用於生產應用程式上執行安全性稽核的 Glimpse。</span><span class="sxs-lookup"><span data-stu-id="d06b5-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="d06b5-173">如需加入角色的資訊，請參閱我[成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 web 應用程式部署至 Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)教學課程。</span><span class="sxs-lookup"><span data-stu-id="d06b5-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="d06b5-174">其他資源</span><span class="sxs-lookup"><span data-stu-id="d06b5-174">Additional Resources</span></span>

- [<span data-ttu-id="d06b5-175">將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="d06b5-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="d06b5-176">[以前並未設定使用過](http://getglimpse.com/Docs/Configuration)-設定索引標籤中，執行階段原則、 記錄和多個文件頁面。</span><span class="sxs-lookup"><span data-stu-id="d06b5-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
