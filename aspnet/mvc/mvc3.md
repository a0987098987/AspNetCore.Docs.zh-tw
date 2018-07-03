---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (包含 2011 年 4 月工具更新)ASP.NET MVC 3 是一個來建置可調整、 以標準為基礎的 web 應用程式，使用堅實的設計模式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: ed3bf81943ac2ffa143cf169c87b3012269d9e77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375787"
---
<a name="aspnet-mvc-3"></a><span data-ttu-id="ee624-103">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="ee624-103">ASP.NET MVC 3</span></span>
====================
> <span data-ttu-id="ee624-104">*(包含 2011 年 4 月工具更新)*</span><span class="sxs-lookup"><span data-stu-id="ee624-104">*(includes April 2011 Tools Update)*</span></span>
> 
> <span data-ttu-id="ee624-105">ASP.NET MVC 3 是用來建置可調整、 以標準為基礎的 web 應用程式使用完善的設計模式，以及 ASP.NET 和.NET Framework 的強大功能的架構。</span><span class="sxs-lookup"><span data-stu-id="ee624-105">ASP.NET MVC 3 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of ASP.NET and the .NET Framework.</span></span>
> 
> <span data-ttu-id="ee624-106">它會並排顯示安裝 ASP.NET MVC 2，因此開始立即使用它 ！</span><span class="sxs-lookup"><span data-stu-id="ee624-106">It installs side-by-side with ASP.NET MVC 2, so get started using it today!</span></span>
> 
> <span data-ttu-id="ee624-107">下載[這裡安裝程式](https://go.microsoft.com/fwlink/?LinkID=208140)</span><span class="sxs-lookup"><span data-stu-id="ee624-107">Download the [installer here](https://go.microsoft.com/fwlink/?LinkID=208140)</span></span>


## <a name="top-features"></a><span data-ttu-id="ee624-108">精選功能</span><span class="sxs-lookup"><span data-stu-id="ee624-108">Top Features</span></span>

- <span data-ttu-id="ee624-109">整合式的 Scaffolding 系統可透過 NuGet 延伸</span><span class="sxs-lookup"><span data-stu-id="ee624-109">Integrated Scaffolding system extensible via NuGet</span></span>
- <span data-ttu-id="ee624-110">HTML 5 已啟用的專案範本</span><span class="sxs-lookup"><span data-stu-id="ee624-110">HTML 5 enabled project templates</span></span>
- <span data-ttu-id="ee624-111">易懂的檢視，包括新的 Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="ee624-111">Expressive Views including the new Razor View Engine</span></span>
- <span data-ttu-id="ee624-112">功能強大的攔截程序相依性插入和全域動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="ee624-112">Powerful hooks with Dependency Injection and Global Action Filters</span></span>
- <span data-ttu-id="ee624-113">不顯眼的 JavaScript、 jQuery 驗證，與 JSON 繫結的豐富 JavaScript 支援</span><span class="sxs-lookup"><span data-stu-id="ee624-113">Rich JavaScript support with unobtrusive JavaScript, jQuery Validation, and JSON binding</span></span>
- <span data-ttu-id="ee624-114">*閱讀完整的功能清單[下方](#overview)*</span><span class="sxs-lookup"><span data-stu-id="ee624-114">*Read the full feature list [below](#overview)*</span></span>

## <a name="top-links"></a><span data-ttu-id="ee624-115">上方的連結</span><span class="sxs-lookup"><span data-stu-id="ee624-115">Top Links</span></span>

<span data-ttu-id="ee624-116">ASP.NET MVC 3 中最新消息</span><span class="sxs-lookup"><span data-stu-id="ee624-116">What's New in ASP.NET MVC 3</span></span>

- <span data-ttu-id="ee624-117">Phil Haack: [ASP.NET MVC 3 的發行](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)</span><span class="sxs-lookup"><span data-stu-id="ee624-117">Phil Haack: [ASP.NET MVC 3 Released](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)</span></span>
- <span data-ttu-id="ee624-118">Scott Hanselman: [ASP.NET MVC3、 WebMatrix、 NuGet、 IIS Express 和 Orchard 推出-Microsoft 年 1 月 Web 版本，在內容中](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)</span><span class="sxs-lookup"><span data-stu-id="ee624-118">Scott Hanselman: [ASP.NET MVC3, WebMatrix, NuGet, IIS Express and Orchard released - The Microsoft January Web Release in Context](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)</span></span>
- <span data-ttu-id="ee624-119">Scott Guthrie:[宣布新版 ASP.NET MVC 3 中，IIS Express、 SQL CE 4，Web 伺服陣列架構、 Orchard，WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)</span><span class="sxs-lookup"><span data-stu-id="ee624-119">Scott Guthrie: [Announcing release of ASP.NET MVC 3, IIS Express, SQL CE 4, Web Farm Framework, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)</span></span>
- [<span data-ttu-id="ee624-120">ASP.NET MVC 3 版本資訊</span><span class="sxs-lookup"><span data-stu-id="ee624-120">Release Notes for ASP.NET MVC 3</span></span>](../whitepapers/mvc3-release-notes.md)

<span data-ttu-id="ee624-121">安裝和說明</span><span class="sxs-lookup"><span data-stu-id="ee624-121">Installation and Help</span></span>

- <span data-ttu-id="ee624-122">安裝 ASP.NET MVC 3 使用[Web Platform Installer （建議選項）](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)</span><span class="sxs-lookup"><span data-stu-id="ee624-122">Install ASP.NET MVC 3 using the [Web Platform Installer (recommended)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)</span></span>
- <span data-ttu-id="ee624-123">安裝 ASP.NET MVC 3 使用[安裝程式可執行檔](https://go.microsoft.com/fwlink/?LinkID=208140)</span><span class="sxs-lookup"><span data-stu-id="ee624-123">Install ASP.NET MVC 3 using the [installer executable](https://go.microsoft.com/fwlink/?LinkID=208140)</span></span>
- <span data-ttu-id="ee624-124">安裝[ASP.NET MVC 3 for Visual Studio 11 開發人員預覽](https://go.microsoft.com/fwlink/?LinkID=208140)</span><span class="sxs-lookup"><span data-stu-id="ee624-124">Install [ASP.NET MVC 3 for Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)</span></span>
- <span data-ttu-id="ee624-125">讀取[ASP.NET MVC 3 教學課程簡介](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)</span><span class="sxs-lookup"><span data-stu-id="ee624-125">Read the [Intro to ASP.NET MVC 3 tutorial](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)</span></span>
- <span data-ttu-id="ee624-126">取得說明並討論在 ASP.NET MVC 3[論壇](https://forums.asp.net/1146.aspx)</span><span class="sxs-lookup"><span data-stu-id="ee624-126">Get help and discuss ASP.NET MVC 3 in the [forums](https://forums.asp.net/1146.aspx)</span></span>

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a><span data-ttu-id="ee624-127">ASP.NET MVC 3 概觀</span><span class="sxs-lookup"><span data-stu-id="ee624-127">ASP.NET MVC 3 Overview</span></span>

<span data-ttu-id="ee624-128">ASP.NET MVC 3 是根據 ASP.NET MVC 1 和 2，新增最棒的功能，同時簡化您的程式碼，並允許更深入的擴充性。</span><span class="sxs-lookup"><span data-stu-id="ee624-128">ASP.NET MVC 3 builds on ASP.NET MVC 1 and 2, adding great features that both simplify your code and allow deeper extensibility.</span></span> <span data-ttu-id="ee624-129">本主題會提供許多包含在此版本中，組織成下列各節中的新功能的概觀：</span><span class="sxs-lookup"><span data-stu-id="ee624-129">This topic provides an overview of many of the new features that are included in this release, organized into the following sections:</span></span>

- [<span data-ttu-id="ee624-130">可延伸的 Scaffolding MvcScaffold 整合</span><span class="sxs-lookup"><span data-stu-id="ee624-130">Extensible Scaffolding with MvcScaffold integration</span></span>](#BM_MvcScaffolding)
- [<span data-ttu-id="ee624-131">HTML 5 已啟用的專案範本</span><span class="sxs-lookup"><span data-stu-id="ee624-131">HTML 5 enabled project templates</span></span>](#BM_HTML5)
- [<span data-ttu-id="ee624-132">Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="ee624-132">The Razor View Engine</span></span>](#BM_TheRazorViewEngine)
- [<span data-ttu-id="ee624-133">支援多個檢視引擎</span><span class="sxs-lookup"><span data-stu-id="ee624-133">Support for Multiple View Engines</span></span>](#BM_Support_for_Multiple_View_Engines)
- [<span data-ttu-id="ee624-134">控制器的改進</span><span class="sxs-lookup"><span data-stu-id="ee624-134">Controller Improvements</span></span>](#BM_Controller_Improvements)
- [<span data-ttu-id="ee624-135">JavaScript 和 Ajax</span><span class="sxs-lookup"><span data-stu-id="ee624-135">JavaScript and Ajax</span></span>](#BM_JavaScript_and_Ajax_Improvements)
- [<span data-ttu-id="ee624-136">模型驗證增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-136">Model Validation Improvements</span></span>](#BM_Model_Validation_Improvements)
- [<span data-ttu-id="ee624-137">相依性插入增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-137">Dependency Injection Improvements</span></span>](#BM_Dependency_Injection_Improvements)
- [<span data-ttu-id="ee624-138">其他新功能</span><span class="sxs-lookup"><span data-stu-id="ee624-138">Other New Features</span></span>](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a><span data-ttu-id="ee624-139">可延伸的 Scaffolding MvcScaffold 整合</span><span class="sxs-lookup"><span data-stu-id="ee624-139">Extensible Scaffolding with MvcScaffold integration</span></span>

<span data-ttu-id="ee624-140">新 Scaffolding 系統容易挑選並開始有效率地使用，如果您不熟悉整個 framework，和一般開發工作自動化，如果您是經驗豐富，並已經知道您的所作所為。</span><span class="sxs-lookup"><span data-stu-id="ee624-140">The new Scaffolding system makes it easier to pick up and start using productively if you're entirely new to the framework, and to automate common development tasks if you're experienced and already know what you're doing.</span></span>

<span data-ttu-id="ee624-141">這新的 NuGet 支援*scaffolding*套件，稱為**MvcScaffolding**。</span><span class="sxs-lookup"><span data-stu-id="ee624-141">This is supported by new NuGet *scaffolding* package called **MvcScaffolding**.</span></span> <span data-ttu-id="ee624-142">"Scaffolding"由許多軟體技術來表示 「 快速產生基本的外框的軟體，您可以接著編輯和自訂 」 一詞。</span><span class="sxs-lookup"><span data-stu-id="ee624-142">The term "Scaffolding" is used by many software technologies to mean "quickly generating a basic outline of your software that you can then edit and customize".</span></span> <span data-ttu-id="ee624-143">我們將建立 ASP.NET mvc scaffolding 封裝是數種案例中大幅有幫助：</span><span class="sxs-lookup"><span data-stu-id="ee624-143">The scaffolding package we're creating for ASP.NET MVC is greatly beneficial in several scenarios:</span></span>

- <span data-ttu-id="ee624-144">**如果您第一次學習 ASP.NET MVC**，因為它可讓您快速取得一些很有用，工作程式碼，您可以編輯並根據您的需求調整。</span><span class="sxs-lookup"><span data-stu-id="ee624-144">**If you're learning ASP.NET MVC for the first time**, because it gives you a fast way to get some useful, working code, that you can then edit and adapt according to your needs.</span></span> <span data-ttu-id="ee624-145">它可讓您查看空白頁面，且不知道要從何處開始創傷 ！</span><span class="sxs-lookup"><span data-stu-id="ee624-145">It saves you from the trauma of looking at a blank page and having no idea where to start!</span></span>
- <span data-ttu-id="ee624-146">**如果您很了解 ASP.NET MVC，現在瀏覽一些新的附加元件技術**一種物件關聯式對應程式、 檢視引擎、 測試的程式庫，例如等等，因為這項技術的建立者可能也建立了 scaffolding 封裝它。</span><span class="sxs-lookup"><span data-stu-id="ee624-146">**If you know ASP.NET MVC well and are now exploring some new add-on technology** such as an object-relational mapper, a view engine, a testing library, etc., because the creator of that technology may have also created a scaffolding package for it.</span></span>
- <span data-ttu-id="ee624-147">**如果您的工作涉及重複建立類似的類別或某種類型的檔案**，因為您可以建立自訂 scaffolder 測試裝置、 部署指令碼，或您所需要的其他任何輸出。</span><span class="sxs-lookup"><span data-stu-id="ee624-147">**If your work involves repeatedly creating similar classes or files of some sort**, because you can create custom scaffolders that output test fixtures, deployment scripts, or whatever else you need.</span></span> <span data-ttu-id="ee624-148">您的小組上的所有人也可以使用您的自訂 scaffolder。</span><span class="sxs-lookup"><span data-stu-id="ee624-148">Everyone on your team can use your custom scaffolders, too.</span></span>

<span data-ttu-id="ee624-149">MvcScaffolding 中的其他功能包括：</span><span class="sxs-lookup"><span data-stu-id="ee624-149">Other features in MvcScaffolding include:</span></span>

- <span data-ttu-id="ee624-150">C# 和 VB 專案的支援</span><span class="sxs-lookup"><span data-stu-id="ee624-150">Support for C# and VB projects</span></span>
- <span data-ttu-id="ee624-151">ASPX 與 Razor 檢視引擎的支援</span><span class="sxs-lookup"><span data-stu-id="ee624-151">Support for the Razor and ASPX view engines</span></span>
- <span data-ttu-id="ee624-152">支援 ASP.NET MVC 區域 scaffolding 並使用自訂檢視版面配置/主機</span><span class="sxs-lookup"><span data-stu-id="ee624-152">Supports scaffolding into ASP.NET MVC areas and using custom view layouts/masters</span></span>
- <span data-ttu-id="ee624-153">您可以輕鬆地編輯來自訂輸出 T4 範本</span><span class="sxs-lookup"><span data-stu-id="ee624-153">You can easily customize the output by editing T4 templates</span></span>
- <span data-ttu-id="ee624-154">您可以新增全新 scaffolder 使用 PowerShell 的自訂邏輯和自訂的 T4 範本。</span><span class="sxs-lookup"><span data-stu-id="ee624-154">You can add entirely new scaffolders using custom PowerShell logic and custom T4 templates.</span></span> <span data-ttu-id="ee624-155">這些 （以及您提供使用者任何自訂參數） 會自動出現在主控台 tab 鍵自動完成清單。</span><span class="sxs-lookup"><span data-stu-id="ee624-155">These (and any custom parameters you've given them) automatically appear in the console tab-completion list.</span></span>
- <span data-ttu-id="ee624-156">您可以取得 NuGet 套件，其中包含適用於不同技術的其他 scaffolder （例如，-概念的其中一個 linq to SQL 目前沒有） 和混合和比對，在一起</span><span class="sxs-lookup"><span data-stu-id="ee624-156">You can get NuGet packages containing additional scaffolders for different technologies (e.g., there's a proof-of-concept one for LINQ to SQL now) and mix and match them together</span></span>

<span data-ttu-id="ee624-157">ASP.NET MVC 3 Tools Update 包含很棒的 Visual Studio 支援此 scaffolding 系統中，例如：</span><span class="sxs-lookup"><span data-stu-id="ee624-157">The ASP.NET MVC 3 Tools Update includes great Visual Studio support for this scaffolding system, such as:</span></span>

- <span data-ttu-id="ee624-158">新增控制器 對話方塊現在支援建立、 讀取、 更新和刪除的控制器動作和對應的檢視表的完整自動 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="ee624-158">Add Controller Dialog now supports full automatic scaffolding of Create, Read, Update, and Delete controller actions and corresponding views.</span></span> <span data-ttu-id="ee624-159">根據預設，這則使用 EF Code First 資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="ee624-159">By default, this scaffolds data access code using EF Code First.</span></span>
- <span data-ttu-id="ee624-160">新增控制器 對話方塊支援*可延伸的架構*透過 NuGet 封裝這類*MvcScaffolding*。</span><span class="sxs-lookup"><span data-stu-id="ee624-160">Add Controller Dialog supports *extensible scaffolds* via NuGet packages such as *MvcScaffolding*.</span></span> <span data-ttu-id="ee624-161">這可讓插入自訂的架構，可讓您使用 ODBCDirect 建立其他的資料存取技術，例如 NHibernate 或甚至是 JET 的架構，如果您因此比較傾向對話方塊 ！</span><span class="sxs-lookup"><span data-stu-id="ee624-161">This allows plugging in custom scaffolds into the dialog which would allow you to create scaffolds for other data access technologies such as NHibernate or even JET with ODBCDirect if you're so inclined!</span></span>

<span data-ttu-id="ee624-162">如需有關 ASP.NET MVC 3 中的 Scaffolding 的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ee624-162">For more information about Scaffolding in ASP.NET MVC 3, see the following resources:</span></span>

- <span data-ttu-id="ee624-163">Steve Sanderson 文章系列，包括：</span><span class="sxs-lookup"><span data-stu-id="ee624-163">Steve Sanderson's post series, including:</span></span> 

    1. [<span data-ttu-id="ee624-164">簡介： 建立含有 MvcScaffolding 套件的 ASP.NET MVC 3 專案的結構</span><span class="sxs-lookup"><span data-stu-id="ee624-164">Introduction: Scaffold your ASP.NET MVC 3 project with the MvcScaffolding package</span></span>](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [<span data-ttu-id="ee624-165">標準的使用方式： 一般使用案例和選項</span><span class="sxs-lookup"><span data-stu-id="ee624-165">Standard usage: Typical use cases and options</span></span>](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [<span data-ttu-id="ee624-166">一對多關聯性</span><span class="sxs-lookup"><span data-stu-id="ee624-166">One-to-Many Relationships</span></span>](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [<span data-ttu-id="ee624-167">Scaffolding 動作和單元測試</span><span class="sxs-lookup"><span data-stu-id="ee624-167">Scaffolding Actions and Unit Tests</span></span>](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [<span data-ttu-id="ee624-168">覆寫的 T4 範本</span><span class="sxs-lookup"><span data-stu-id="ee624-168">Overriding the T4 templates</span></span>](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [<span data-ttu-id="ee624-169">這篇文章： 建立自訂 scaffolder</span><span class="sxs-lookup"><span data-stu-id="ee624-169">This post: Creating custom scaffolders</span></span>](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- <span data-ttu-id="ee624-170">Scott Hanselman 的 post，從他的 PDC 2010 講習會[建置與 Microsoft 「 未具名套件的 Web Love 」 部落格](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)</span><span class="sxs-lookup"><span data-stu-id="ee624-170">Scott Hanselman's post from his PDC 2010 session [Building a Blog with Microsoft "Unnamed Package of Web Love"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)</span></span>
- [<span data-ttu-id="ee624-171">MVC 3 版本資訊</span><span class="sxs-lookup"><span data-stu-id="ee624-171">MVC 3 Release Notes</span></span>](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a><span data-ttu-id="ee624-172">HTML 5 專案範本</span><span class="sxs-lookup"><span data-stu-id="ee624-172">HTML 5 Project Templates</span></span>

<span data-ttu-id="ee624-173">新增專案 對話方塊會包含專案範本的核取方塊啟用 HTML 5 版本。</span><span class="sxs-lookup"><span data-stu-id="ee624-173">The New Project dialog includes a checkbox enable HTML 5 versions of project templates.</span></span> <span data-ttu-id="ee624-174">這些範本會利用以舊版瀏覽器中的相容性支援的 html5 和 CSS 3 的 Modernizr 1.7。</span><span class="sxs-lookup"><span data-stu-id="ee624-174">These templates leverage Modernizr 1.7 to provide compatibility support for HTML 5 and CSS 3 in down-level browsers.</span></span>

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a><span data-ttu-id="ee624-175">Razor 檢視引擎</span><span class="sxs-lookup"><span data-stu-id="ee624-175">The Razor View Engine</span></span>

<span data-ttu-id="ee624-176">ASP.NET MVC 3 隨附新的檢視引擎，名為 Razor 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="ee624-176">ASP.NET MVC 3 comes with a new view engine named Razor that offers the following benefits:</span></span>

- <span data-ttu-id="ee624-177">Razor 語法是整齊精簡，需要最少的按鍵輸入。</span><span class="sxs-lookup"><span data-stu-id="ee624-177">Razor syntax is clean and concise, requiring a minimum number of keystrokes.</span></span>
- <span data-ttu-id="ee624-178">Razor 是容易學習，部分因為它根據現有的語言，例如 C# 和 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="ee624-178">Razor is easy to learn, in part because it's based on existing languages like C# and Visual Basic.</span></span>
- <span data-ttu-id="ee624-179">Visual Studio 包含 Razor 語法的 IntelliSense 和程式碼顏色標示。</span><span class="sxs-lookup"><span data-stu-id="ee624-179">Visual Studio includes IntelliSense and code colorization for Razor syntax.</span></span>
- <span data-ttu-id="ee624-180">Razor 檢視可以進行單元測試而不需要您執行應用程式，或啟動 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ee624-180">Razor views can be unit tested without requiring that you run the application or launch a web server.</span></span>

<span data-ttu-id="ee624-181">一些新 Razor 功能包括：</span><span class="sxs-lookup"><span data-stu-id="ee624-181">Some new Razor features include the following:</span></span>

- <span data-ttu-id="ee624-182">`@model` 指定要傳遞至檢視的型別語法。</span><span class="sxs-lookup"><span data-stu-id="ee624-182">`@model` syntax for specifying the type being passed to the view.</span></span>
- <span data-ttu-id="ee624-183">`@* *@` 註解語法。</span><span class="sxs-lookup"><span data-stu-id="ee624-183">`@* *@` comment syntax.</span></span>
- <span data-ttu-id="ee624-184">指定預設值的功能 (例如`layoutpage`) 整個站台的一次。</span><span class="sxs-lookup"><span data-stu-id="ee624-184">The ability to specify defaults (such as `layoutpage`) once for an entire site.</span></span>
- <span data-ttu-id="ee624-185">`Html.Raw`方法，來顯示文字，而不進行 HTML 編碼它。</span><span class="sxs-lookup"><span data-stu-id="ee624-185">The `Html.Raw` method for displaying text without HTML-encoding it.</span></span>
- <span data-ttu-id="ee624-186">支援多個檢視之間共用程式碼 (*\_viewstart.cshtml*或是 *\_viewstart.vbhtml*檔案)。</span><span class="sxs-lookup"><span data-stu-id="ee624-186">Support for sharing code among multiple views (*\_viewstart.cshtml* or *\_viewstart.vbhtml* files).</span></span>

<span data-ttu-id="ee624-187">Razor 也包含新的 HTML 協助程式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ee624-187">Razor also includes  new HTML helpers, such as the following:</span></span>

- <span data-ttu-id="ee624-188">`Chart`.</span><span class="sxs-lookup"><span data-stu-id="ee624-188">`Chart`.</span></span> <span data-ttu-id="ee624-189">呈現的圖表，提供與 ASP.NET 4 中的 chart 控制項相同的功能。</span><span class="sxs-lookup"><span data-stu-id="ee624-189">Renders a chart, offering the same features as the chart control in ASP.NET 4.</span></span>
- <span data-ttu-id="ee624-190">`WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="ee624-190">`WebGrid`.</span></span> <span data-ttu-id="ee624-191">呈現的資料格，完整的分頁和排序功能。</span><span class="sxs-lookup"><span data-stu-id="ee624-191">Renders a data grid, complete with paging and sorting functionality.</span></span>
- <span data-ttu-id="ee624-192">`Crypto`.</span><span class="sxs-lookup"><span data-stu-id="ee624-192">`Crypto`.</span></span> <span data-ttu-id="ee624-193">它使用雜湊演算法，以建立正確 salted 和雜湊密碼。</span><span class="sxs-lookup"><span data-stu-id="ee624-193">Uses hashing algorithms to create properly salted and hashed passwords.</span></span>
- <span data-ttu-id="ee624-194">`WebImage`.</span><span class="sxs-lookup"><span data-stu-id="ee624-194">`WebImage`.</span></span> <span data-ttu-id="ee624-195">呈現影像。</span><span class="sxs-lookup"><span data-stu-id="ee624-195">Renders an image.</span></span>
- <span data-ttu-id="ee624-196">`WebMail`.</span><span class="sxs-lookup"><span data-stu-id="ee624-196">`WebMail`.</span></span> <span data-ttu-id="ee624-197">傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="ee624-197">Sends an email message.</span></span>

<span data-ttu-id="ee624-198">如需 Razor 的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ee624-198">For more information about Razor, see the following resources:</span></span>

- [<span data-ttu-id="ee624-199">Razor 簡介 Scott Guthrie 的部落格文章</span><span class="sxs-lookup"><span data-stu-id="ee624-199">Scott Guthrie's blog post introducing Razor</span></span>](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [<span data-ttu-id="ee624-200">Scott Guthrie 的部落格文章簡介@model關鍵字</span><span class="sxs-lookup"><span data-stu-id="ee624-200">Scott Guthrie's blog post introducing the @model keyword</span></span>](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [<span data-ttu-id="ee624-201">Scott Guthrie 的部落格文章簡介 Razor 版面配置</span><span class="sxs-lookup"><span data-stu-id="ee624-201">Scott Guthrie's blog post introducing Razor layouts</span></span>](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [<span data-ttu-id="ee624-202">Razor API 快速參考</span><span class="sxs-lookup"><span data-stu-id="ee624-202">Razor API Quick Reference</span></span>](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [<span data-ttu-id="ee624-203">MVC 3 版本資訊</span><span class="sxs-lookup"><span data-stu-id="ee624-203">MVC 3 Release Notes</span></span>](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a><span data-ttu-id="ee624-204">支援多個檢視引擎</span><span class="sxs-lookup"><span data-stu-id="ee624-204">Support for Multiple View Engines</span></span>

<span data-ttu-id="ee624-205">**加入檢視**ASP.NET MVC 3 中的對話方塊可讓您選擇您想要使用的檢視引擎，**新的專案**對話方塊可讓您指定專案的預設檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="ee624-205">The **Add View** dialog box in ASP.NET MVC 3 lets you choose the view engine you want to work with, and the **New Project** dialog box lets you specify the default view engine for a project.</span></span> <span data-ttu-id="ee624-206">您可以選擇 Web Form 檢視引擎 (ASPX)，Razor 或開放原始碼檢視引擎例如[Spark](http://sparkviewengine.com/)， [NHaml](https://code.google.com/p/nhaml/)，或[NDjango](http://ndjango.org/)。</span><span class="sxs-lookup"><span data-stu-id="ee624-206">You can choose the Web Forms view engine (ASPX), Razor, or an open-source view engine such as [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), or [NDjango](http://ndjango.org/).</span></span>

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a><span data-ttu-id="ee624-207">控制器的改進</span><span class="sxs-lookup"><span data-stu-id="ee624-207">Controller Improvements</span></span>

### <a name="global-action-filters"></a><span data-ttu-id="ee624-208">全域動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="ee624-208">Global Action Filters</span></span>

<span data-ttu-id="ee624-209">有時您想要執行的邏輯在動作方法執行之前或之後的動作方法執行。</span><span class="sxs-lookup"><span data-stu-id="ee624-209">Sometimes you want to perform logic either before an action method runs or after an action method runs.</span></span> <span data-ttu-id="ee624-210">若要支援此功能，ASP.NET MVC 2 會提供動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ee624-210">To support this, ASP.NET MVC 2 provided action filters.</span></span> <span data-ttu-id="ee624-211">動作篩選條件會提供宣告式方法，將前置動作和動作後的行為加入至特定的控制器動作方法的自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="ee624-211">Action filters are custom attributes that provide a declarative means to add pre-action and post-action behavior to specific controller action methods.</span></span> <span data-ttu-id="ee624-212">不過，在某些情況下，您可能想要指定前置動作或後續動作套用至所有動作方法的行為。</span><span class="sxs-lookup"><span data-stu-id="ee624-212">However, in some cases you might want to specify pre-action or post-action behavior that applies to all action methods.</span></span> <span data-ttu-id="ee624-213">MVC 3 可讓您指定全域篩選器加入至`GlobalFilters`集合。</span><span class="sxs-lookup"><span data-stu-id="ee624-213">MVC 3 lets you specify global filters by adding them to the `GlobalFilters` collection.</span></span> <span data-ttu-id="ee624-214">如需全域動作篩選條件的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ee624-214">For more information about global action filters, see the following resources:</span></span>

- [<span data-ttu-id="ee624-215">Scott Guthrie 的部落格上的 MVC 3 Preview</span><span class="sxs-lookup"><span data-stu-id="ee624-215">Scott Guthrie's blog on the MVC 3 Preview</span></span>](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- <span data-ttu-id="ee624-216">[ASP.NET MVC 中的篩選](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)</span><span class="sxs-lookup"><span data-stu-id="ee624-216">[Filtering in ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)</span></span>

### <a name="new-viewbag-property"></a><span data-ttu-id="ee624-217">新的 「 ViewBag 」 屬性</span><span class="sxs-lookup"><span data-stu-id="ee624-217">New "ViewBag" Property</span></span>

<span data-ttu-id="ee624-218">MVC 2 控制器支援`ViewData`屬性，可讓您將資料傳遞至使用晚期繫結字典 API 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="ee624-218">MVC 2 controllers support a `ViewData` property that enables you to pass data to a view template using a late-bound dictionary API.</span></span> <span data-ttu-id="ee624-219">在 MVC 3 中，您也可以使用較為簡單的語法，與`ViewBag`屬性來達成相同目的。</span><span class="sxs-lookup"><span data-stu-id="ee624-219">In MVC 3, you can also use somewhat simpler syntax with the `ViewBag` property to accomplish the same purpose.</span></span> <span data-ttu-id="ee624-220">比方說，反而比撰寫`ViewData["Message"]="text"`，您可以撰寫`ViewBag.Message="text"`。</span><span class="sxs-lookup"><span data-stu-id="ee624-220">For example, instead of writing `ViewData["Message"]="text"`, you can write `ViewBag.Message="text"`.</span></span> <span data-ttu-id="ee624-221">您不需要定義任何強型別類別，才能使用`ViewBag`屬性。</span><span class="sxs-lookup"><span data-stu-id="ee624-221">You do not need to define any strongly-typed classes to use the `ViewBag` property.</span></span> <span data-ttu-id="ee624-222">因為它是動態屬性時，您可以改為直接取得或設定屬性，並將它們以動態方式在執行階段解析。</span><span class="sxs-lookup"><span data-stu-id="ee624-222">Because it is a dynamic property, you can instead just get or set properties and it will resolve them dynamically at run time.</span></span> <span data-ttu-id="ee624-223">就內部而言，`ViewBag`屬性會儲存成名稱/值組中`ViewData`字典。</span><span class="sxs-lookup"><span data-stu-id="ee624-223">Internally, `ViewBag` properties are stored as name/value pairs in the `ViewData` dictionary.</span></span> <span data-ttu-id="ee624-224">(注意： 在大多數的 MVC 3 的發行前版本`ViewBag`屬性名為`ViewModel`屬性。)</span><span class="sxs-lookup"><span data-stu-id="ee624-224">(Note: in most pre-release versions of MVC 3, the `ViewBag` property was named the `ViewModel` property.)</span></span>

### <a name="new-actionresult-types"></a><span data-ttu-id="ee624-225">新的 「 ActionResult"類型</span><span class="sxs-lookup"><span data-stu-id="ee624-225">New "ActionResult" Types</span></span>

<span data-ttu-id="ee624-226">下列`ActionResult`類型和對應的協助程式方法都是新的或增強 MVC 3 中：</span><span class="sxs-lookup"><span data-stu-id="ee624-226">The following `ActionResult` types and corresponding helper methods are new or enhanced in MVC 3:</span></span>

- <span data-ttu-id="ee624-227">[HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx)。</span><span class="sxs-lookup"><span data-stu-id="ee624-227">[HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx).</span></span> <span data-ttu-id="ee624-228">傳回 「 404 的 HTTP 狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="ee624-228">Returns a 404 HTTP status code to the client.</span></span>
- <span data-ttu-id="ee624-229">[RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx)。</span><span class="sxs-lookup"><span data-stu-id="ee624-229">[RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx).</span></span> <span data-ttu-id="ee624-230">傳回暫時重新導向 （HTTP 302 狀態碼） 或根據布林參數成為永久重新導向 （HTTP 301 狀態碼）。</span><span class="sxs-lookup"><span data-stu-id="ee624-230">Returns a temporary redirect (HTTP 302 status code) or a permanent redirect (HTTP 301 status code), depending on a Boolean parameter.</span></span> <span data-ttu-id="ee624-231">這項變更，搭配[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx)類別現在有三種方法來執行永久重新導向： `RedirectPermanent`， `RedirectToRoutePermanent`，和`RedirectToActionPermanent`。</span><span class="sxs-lookup"><span data-stu-id="ee624-231">In conjunction with this change, the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) class now has three methods for performing permanent redirects: `RedirectPermanent`, `RedirectToRoutePermanent`, and `RedirectToActionPermanent`.</span></span> <span data-ttu-id="ee624-232">這些方法會傳回的執行個體`RedirectResult`具有`Permanent`屬性設定為`true`。</span><span class="sxs-lookup"><span data-stu-id="ee624-232">These methods return an instance of `RedirectResult` with the `Permanent` property set to `true`.</span></span>
- <span data-ttu-id="ee624-233">[HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx)。</span><span class="sxs-lookup"><span data-stu-id="ee624-233">[HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx).</span></span> <span data-ttu-id="ee624-234">傳回使用者指定的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="ee624-234">Returns a user-specified HTTP status code.</span></span>

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a><span data-ttu-id="ee624-235">JavaScript 和 Ajax 的增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-235">JavaScript and Ajax Improvements</span></span>

<span data-ttu-id="ee624-236">根據預設，MVC 3 中的 Ajax 和驗證協助程式會使用不顯眼的 JavaScript 方法。</span><span class="sxs-lookup"><span data-stu-id="ee624-236">By default, Ajax and validation helpers in MVC 3 use an unobtrusive JavaScript approach.</span></span> <span data-ttu-id="ee624-237">不顯眼的 JavaScript 中，可避免插入 HTML 中的內嵌 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="ee624-237">Unobtrusive JavaScript avoids injecting inline JavaScript into HTML.</span></span> <span data-ttu-id="ee624-238">這可讓您的 HTML 小且較不雜亂，並可讓您更輕鬆地交換或自訂的 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="ee624-238">This makes your HTML smaller and less cluttered, and makes it easier to swap out or customize JavaScript libraries.</span></span> <span data-ttu-id="ee624-239">在 MVC 3 驗證協助程式也使用`jQueryValidate`預設的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="ee624-239">Validation helpers in MVC 3 also use the `jQueryValidate` plugin by default.</span></span> <span data-ttu-id="ee624-240">如果您想要 MVC 2 的行為，您可以停用使用不顯眼的 JavaScript *web.config*檔案設定。</span><span class="sxs-lookup"><span data-stu-id="ee624-240">If you want MVC 2 behavior, you can disable unobtrusive JavaScript using a *web.config* file setting.</span></span> <span data-ttu-id="ee624-241">如需有關 JavaScript 和 Ajax 的增強功能的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ee624-241">For more information about JavaScript and Ajax improvements, see the following resources:</span></span>

- [<span data-ttu-id="ee624-242">不顯眼的 JavaScript 維基百科網站上的基本簡介</span><span class="sxs-lookup"><span data-stu-id="ee624-242">Basic introduction to unobtrusive JavaScript on the Wikipedia site</span></span>](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [<span data-ttu-id="ee624-243">Brad Wilson 的不顯眼的 JavaScript 文章</span><span class="sxs-lookup"><span data-stu-id="ee624-243">Brad Wilson's Unobtrusive JavaScript Post</span></span>](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [<span data-ttu-id="ee624-244">Brad Wilson 的不顯眼的 JavaScript 驗證 Post</span><span class="sxs-lookup"><span data-stu-id="ee624-244">Brad Wilson's Unobtrusive JavaScript Validation Post</span></span>](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- <span data-ttu-id="ee624-245">[使用 Razor 和不顯眼的 JavaScript 建立 MVC 3 應用程式](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md)（ASP.NET 網站上教學課程）</span><span class="sxs-lookup"><span data-stu-id="ee624-245">[Creating a MVC 3 Application with Razor and Unobtrusive JavaScript](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial on the ASP.NET site)</span></span>
- [<span data-ttu-id="ee624-246">MVC 3 版本資訊</span><span class="sxs-lookup"><span data-stu-id="ee624-246">MVC 3 Release Notes</span></span>](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a><span data-ttu-id="ee624-247">依預設啟用的用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="ee624-247">Client-Side Validation Enabled by Default</span></span>

<span data-ttu-id="ee624-248">在舊版的 MVC 中，您需要明確地呼叫`Html.EnableClientValidation`從檢視表，以啟用用戶端驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="ee624-248">In earlier versions of MVC, you need to explicitly call the `Html.EnableClientValidation` method from a view in order to enable client-side validation.</span></span> <span data-ttu-id="ee624-249">MVC 3 中這是不再需要因為預設會啟用用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="ee624-249">In MVC 3 this is no longer required because client-side validation is enabled by default.</span></span> <span data-ttu-id="ee624-250">(您可以停用此使用中的設定*web.config*檔案。)</span><span class="sxs-lookup"><span data-stu-id="ee624-250">(You can disable this using a setting in the *web.config* file.)</span></span>

<span data-ttu-id="ee624-251">為了讓用戶端驗證運作，您仍然需要參考適當的 jQuery 和 jQuery 驗證程式庫，在您的網站。</span><span class="sxs-lookup"><span data-stu-id="ee624-251">In order for client-side validation to work, you still need to reference the appropriate jQuery and jQuery Validation libraries in your site.</span></span> <span data-ttu-id="ee624-252">您可以裝載您自己的伺服器上的這些程式庫，或從像是來自 Microsoft 或 Google Cdn 的內容傳遞網路 (CDN) 中參考它們。</span><span class="sxs-lookup"><span data-stu-id="ee624-252">You can host those libraries on your own server or reference them from a content delivery network (CDN) like the CDNs from Microsoft or Google.</span></span>

### <a name="remote-validator"></a><span data-ttu-id="ee624-253">遠端驗證程式</span><span class="sxs-lookup"><span data-stu-id="ee624-253">Remote Validator</span></span>

<span data-ttu-id="ee624-254">支援新的 ASP.NET MVC 3 [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx)類別，可讓您充分利用 jQuery 驗證外掛程式中的遠端驗證支援。</span><span class="sxs-lookup"><span data-stu-id="ee624-254">ASP.NET MVC 3 supports the new [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) class that enables you to take advantage of the jQuery Validation plug-in's remote validator support.</span></span> <span data-ttu-id="ee624-255">這可讓用戶端驗證程式庫會自動呼叫伺服器端以執行只能進行的驗證邏輯伺服器定義的自訂方法。</span><span class="sxs-lookup"><span data-stu-id="ee624-255">This enables the client-side validation library to automatically call a custom method that you define on the server in order to perform validation logic that can only be done server-side.</span></span>

<span data-ttu-id="ee624-256">在下列範例中，`Remote`屬性會指定用戶端驗證將會呼叫名為動作`UserNameAvailable`上`UsersController`類別，以驗證`UserName`欄位。</span><span class="sxs-lookup"><span data-stu-id="ee624-256">In the following example, the `Remote` attribute specifies that client validation will call an action named `UserNameAvailable` on the `UsersController` class in order to validate the `UserName` field.</span></span>

[!code-csharp[Main](mvc3/samples/sample1.cs)]

<span data-ttu-id="ee624-257">下列範例會顯示對應的控制器。</span><span class="sxs-lookup"><span data-stu-id="ee624-257">The following example shows the corresponding controller.</span></span>

[!code-csharp[Main](mvc3/samples/sample2.cs)]

<span data-ttu-id="ee624-258">如需有關如何使用`Remote`屬性，請參閱[How to: ASP.NET MVC 中實作遠端驗證](https://msdn.microsoft.com/library/gg508808(VS.98).aspx)MSDN library 中。</span><span class="sxs-lookup"><span data-stu-id="ee624-258">For more information about how to use the `Remote` attribute, see [How to: Implement Remote Validation in ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) in the MSDN library.</span></span>

### <a name="json-binding-support"></a><span data-ttu-id="ee624-259">JSON 繫結支援</span><span class="sxs-lookup"><span data-stu-id="ee624-259">JSON Binding Support</span></span>

<span data-ttu-id="ee624-260">ASP.NET MVC 3 包含內建的 JSON 繫結支援，可讓動作方法接收 JSON 編碼的資料和模型繫結它至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="ee624-260">ASP.NET MVC 3 includes built-in JSON binding support that enables action methods to receive JSON-encoded data and model-bind it to action-method parameters.</span></span> <span data-ttu-id="ee624-261">這項功能可用於涉及用戶端範本與資料繫結案例。</span><span class="sxs-lookup"><span data-stu-id="ee624-261">This capability is useful in scenarios involving client templates and data binding.</span></span> <span data-ttu-id="ee624-262">（用戶端範本可讓您進行格式化並顯示單一資料項目的集合，使用用戶端執行的範本。）MVC 3 可讓您輕鬆地在伺服器上的動作方法，傳送和接收 JSON 資料連接用戶端範本。</span><span class="sxs-lookup"><span data-stu-id="ee624-262">(Client templates enable you to format and display a single data item or set of data items by using templates that execute on the client.) MVC 3 enables you to easily connect client templates with action methods on the server that send and receive JSON data.</span></span> <span data-ttu-id="ee624-263">如需 JSON 繫結支援的詳細資訊，請參閱**JavaScript 和 AJAX 改進**一節[Scott Guthrie 的 MVC 3 Preview 部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ee624-263">For more information about JSON binding support, see the **JavaScript and AJAX Improvements** section of [Scott Guthrie's MVC 3 Preview blog post](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).</span></span>

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a><span data-ttu-id="ee624-264">模型驗證增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-264">Model Validation Improvements</span></span>

### <a name="dataannotations-metadata-attributes"></a><span data-ttu-id="ee624-265">「 DataAnnotations"中繼資料屬性</span><span class="sxs-lookup"><span data-stu-id="ee624-265">"DataAnnotations" Metadata Attributes</span></span>

<span data-ttu-id="ee624-266">ASP.NET MVC 3 支援`DataAnnotations`等中繼資料屬性`DisplayAttribute`。</span><span class="sxs-lookup"><span data-stu-id="ee624-266">ASP.NET MVC 3 supports `DataAnnotations` metadata attributes such as `DisplayAttribute`.</span></span>

### <a name="validationattribute-class"></a><span data-ttu-id="ee624-267">「 ValidationAttribute"類別</span><span class="sxs-lookup"><span data-stu-id="ee624-267">"ValidationAttribute" Class</span></span>

<span data-ttu-id="ee624-268">`ValidationAttribute`類別已改進在.NET Framework 4 中，以支援新`IsValid`提供目前的驗證內容，例如哪些物件正在驗證的詳細資訊的多載。</span><span class="sxs-lookup"><span data-stu-id="ee624-268">The `ValidationAttribute` class was improved in the .NET Framework 4 to support a new `IsValid` overload that provides more information about the current validation context, such as what object is being validated.</span></span> <span data-ttu-id="ee624-269">這可讓您可以在此驗證目前模型的另一個屬性為基礎的值更豐富的案例。</span><span class="sxs-lookup"><span data-stu-id="ee624-269">This enables richer scenarios where you can validate the current value based on another property of the model.</span></span> <span data-ttu-id="ee624-270">比方說，新`CompareAttribute`屬性可讓您比較模型的兩個屬性的值。</span><span class="sxs-lookup"><span data-stu-id="ee624-270">For example, the new `CompareAttribute` attribute lets you compare the values of two properties of a model.</span></span> <span data-ttu-id="ee624-271">在下列範例中，`ComparePassword`屬性必須符合`Password`欄位才會生效。</span><span class="sxs-lookup"><span data-stu-id="ee624-271">In the following example, the `ComparePassword` property must match the `Password` field in order to be valid.</span></span>

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a><span data-ttu-id="ee624-272">驗證介面</span><span class="sxs-lookup"><span data-stu-id="ee624-272">Validation Interfaces</span></span>

<span data-ttu-id="ee624-273">[IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx)介面可讓您執行模型層級驗證，並可讓您提供的驗證錯誤訊息所特有的整體模型中，或在模型內的兩個屬性之間的狀態.</span><span class="sxs-lookup"><span data-stu-id="ee624-273">The [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interface enables you to perform model-level validation, and it enables you to provide validation error messages that are specific to the state of the overall model, or between two properties within the model.</span></span> <span data-ttu-id="ee624-274">MVC 3 現在會擷取錯誤`IValidatableObject`介面時，模型繫結，並自動旗標或反白顯示受影響的檢視，使用內建的 HTML 表單協助程式中的欄位。</span><span class="sxs-lookup"><span data-stu-id="ee624-274">MVC 3 now retrieves errors from the `IValidatableObject` interface when model binding, and automatically flags or highlights affected fields within a view using the built-in HTML form helpers.</span></span>

<span data-ttu-id="ee624-275">[IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx)介面可讓 ASP.NET MVC 驗證程式是否支援用戶端驗證，在執行階段探索。</span><span class="sxs-lookup"><span data-stu-id="ee624-275">The [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interface enables ASP.NET MVC to discover at run time whether a validator has support for client validation.</span></span> <span data-ttu-id="ee624-276">此介面，讓它可以與各種不同的驗證架構整合設計。</span><span class="sxs-lookup"><span data-stu-id="ee624-276">This interface has been designed so that it can be integrated with a variety of validation frameworks.</span></span>

<span data-ttu-id="ee624-277">如需有關驗證介面的詳細資訊，請參閱**模型驗證改進**一節[Scott Guthrie 的 MVC 3 Preview 部落格文章](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ee624-277">For more information about validation interfaces, see the **Model Validation Improvements** section of [Scott Guthrie's MVC 3 Preview blog post](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).</span></span> <span data-ttu-id="ee624-278">（不過請注意"IValidateObject 」 部落格中的參考，應該是 「 IValidatableObject"）。</span><span class="sxs-lookup"><span data-stu-id="ee624-278">(However, note that the reference to "IValidateObject" in the blog should be "IValidatableObject".)</span></span>

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a><span data-ttu-id="ee624-279">相依性插入增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-279">Dependency Injection Improvements</span></span>

<span data-ttu-id="ee624-280">套用相依性插入 (DI) 和整合與相依性插入或的控制反轉 (IOC) 容器，則 ASP.NET MVC 3 提供更佳的支援。</span><span class="sxs-lookup"><span data-stu-id="ee624-280">ASP.NET MVC 3 provides better support for applying Dependency Injection (DI) and for integrating with Dependency Injection or Inversion of Control (IOC) containers.</span></span> <span data-ttu-id="ee624-281">DI 支援已新增下列方面：</span><span class="sxs-lookup"><span data-stu-id="ee624-281">Support for DI has been added in the following areas:</span></span>

- <span data-ttu-id="ee624-282">控制站 （註冊和插入控制器 factory，將控制器）。</span><span class="sxs-lookup"><span data-stu-id="ee624-282">Controllers (registering and injecting controller factories, injecting controllers).</span></span>
- <span data-ttu-id="ee624-283">檢視 （註冊和插入檢視引擎，相依性插入檢視頁面）。</span><span class="sxs-lookup"><span data-stu-id="ee624-283">Views (registering and injecting view engines, injecting dependencies into view pages).</span></span>
- <span data-ttu-id="ee624-284">動作篩選條件 （尋找和插入篩選）。</span><span class="sxs-lookup"><span data-stu-id="ee624-284">Action filters (locating and injecting filters).</span></span>
- <span data-ttu-id="ee624-285">模型繫結器 （註冊和插入）。</span><span class="sxs-lookup"><span data-stu-id="ee624-285">Model binders (registering and injecting).</span></span>
- <span data-ttu-id="ee624-286">模型驗證提供者 （註冊和插入）。</span><span class="sxs-lookup"><span data-stu-id="ee624-286">Model validation providers (registering and injecting).</span></span>
- <span data-ttu-id="ee624-287">模型中繼資料提供者 （註冊和插入）。</span><span class="sxs-lookup"><span data-stu-id="ee624-287">Model metadata providers (registering and injecting).</span></span>
- <span data-ttu-id="ee624-288">值提供者 （註冊和插入）。</span><span class="sxs-lookup"><span data-stu-id="ee624-288">Value providers (registering and injecting).</span></span>

<span data-ttu-id="ee624-289">MVC 3 還支援[通用服務定位程式](https://github.com/unitycontainer/commonservicelocator)程式庫和任何支援該程式庫的 DI 容器`IServiceLocator`介面。</span><span class="sxs-lookup"><span data-stu-id="ee624-289">MVC 3 supports the [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) library and any DI container that supports that library's `IServiceLocator` interface.</span></span> <span data-ttu-id="ee624-290">它也支援新`IDependencyResolver`介面可讓您更輕鬆地整合 DI 架構。</span><span class="sxs-lookup"><span data-stu-id="ee624-290">It also supports a new `IDependencyResolver` interface that makes it easier to integrate DI frameworks.</span></span>

<span data-ttu-id="ee624-291">如需有關 MVC 3 中的 DI 的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ee624-291">For more information about DI in MVC 3, see the following resources:</span></span>

- [<span data-ttu-id="ee624-292">服務位置有關的 Brad Wilson 的部落格系列文章</span><span class="sxs-lookup"><span data-stu-id="ee624-292">Brad Wilson's series of blog posts on Service Location</span></span>](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [<span data-ttu-id="ee624-293">MVC 3 版本資訊</span><span class="sxs-lookup"><span data-stu-id="ee624-293">MVC 3 Release Notes</span></span>](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a><span data-ttu-id="ee624-294">其他新功能</span><span class="sxs-lookup"><span data-stu-id="ee624-294">Other New Features</span></span>

### <a name="nuget-integration"></a><span data-ttu-id="ee624-295">NuGet 整合</span><span class="sxs-lookup"><span data-stu-id="ee624-295">NuGet Integration</span></span>

<span data-ttu-id="ee624-296">ASP.NET MVC 3 自動安裝，並將其安裝程序讓 NuGet。</span><span class="sxs-lookup"><span data-stu-id="ee624-296">ASP.NET MVC 3 automatically installs and enables NuGet as part of its setup.</span></span> <span data-ttu-id="ee624-297">NuGet 是免費的開放原始碼套件管理員，可讓您輕鬆地尋找、 安裝及使用您在專案中的.NET 程式庫和工具。</span><span class="sxs-lookup"><span data-stu-id="ee624-297">NuGet is a free open-source package manager that makes it easy to find, install, and use .NET libraries and tools in your projects.</span></span> <span data-ttu-id="ee624-298">它適用於所有的 Visual Studio 專案類型 （包括 ASP.NET Web Form 和 ASP.NET MVC）。</span><span class="sxs-lookup"><span data-stu-id="ee624-298">It works with all Visual Studio project types (including ASP.NET Web Forms and ASP.NET MVC).</span></span>

<span data-ttu-id="ee624-299">NuGet 可讓開發人員維護其程式庫封裝，並在線上組件庫中註冊它們的開放原始碼專案 （例如，專案等 Moq、 NHibernate、 Ninject、 StructureMap、 NUnit，Windsor、 RhinoMocks，Elmah）。</span><span class="sxs-lookup"><span data-stu-id="ee624-299">NuGet enables developers who maintain open source projects (for example, projects like Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks, and Elmah) to package their libraries and register them in an online gallery.</span></span> <span data-ttu-id="ee624-300">然後是簡單的.NET 開發人員想要使用這些程式庫的其中一個找到的套件，並將它安裝在他們正在處理的專案。</span><span class="sxs-lookup"><span data-stu-id="ee624-300">It is then easy for .NET developers who want to use one of these libraries to find the package and install it in projects they are working on.</span></span>

<span data-ttu-id="ee624-301">使用 ASP.NET 3 Tools Update 中，專案範本包含 JavaScript 程式庫預先安裝的 NuGet 套件，讓它們可透過 NuGet 更新。</span><span class="sxs-lookup"><span data-stu-id="ee624-301">With the ASP.NET 3 Tools Update, project templates include JavaScript libraries pre-installed NuGet packages, so they are updatable via NuGet.</span></span> <span data-ttu-id="ee624-302">Entity Framework Code First 也會預先安裝為 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="ee624-302">Entity Framework Code First is also pre-installed as a NuGet package.</span></span>

<span data-ttu-id="ee624-303">如需 NuGet 的詳細資訊，請參閱 [NuGet 文件](https://docs.microsoft.com/nuget/) (英文)。</span><span class="sxs-lookup"><span data-stu-id="ee624-303">For more information about NuGet, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span>

### <a name="partial-page-output-caching"></a><span data-ttu-id="ee624-304">部分頁面輸出快取</span><span class="sxs-lookup"><span data-stu-id="ee624-304">Partial-Page Output Caching</span></span>

<span data-ttu-id="ee624-305">ASP.NET MVC 有支援第 1 版自輸出快取的整頁回應。</span><span class="sxs-lookup"><span data-stu-id="ee624-305">ASP.NET MVC has supported output caching of full page responses since version 1.</span></span> <span data-ttu-id="ee624-306">MVC 3 也支援部分頁面輸出快取，這可讓您輕鬆地快取區域或回應的片段。</span><span class="sxs-lookup"><span data-stu-id="ee624-306">MVC 3 also supports partial-page output caching, which allows you to easily cache regions or fragments of a response.</span></span> <span data-ttu-id="ee624-307">如需有關快取的詳細資訊，請參閱**部分的頁面輸出快取**一節[Scott Guthrie 的部落格文章，在 MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)而**子動作的輸出快取**一節[MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="ee624-307">For more information about caching, see the **Partial Page Output Caching** section of [Scott Guthrie's blog post on the MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) and the **Child Action Output Caching** section of the [MVC 3 Release Notes](../whitepapers/mvc3-release-notes.md).</span></span>

### <a name="granular-control-over-request-validation"></a><span data-ttu-id="ee624-308">更精確地控制要求驗證</span><span class="sxs-lookup"><span data-stu-id="ee624-308">Granular Control over Request Validation</span></span>

<span data-ttu-id="ee624-309">ASP.NET MVC 有內建的要求驗證，會自動可協助防止 XSS 和 HTML 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="ee624-309">ASP.NET MVC has built-in request validation that automatically helps protect against XSS and HTML injection attacks.</span></span> <span data-ttu-id="ee624-310">不過，有時候您想要明確停用要求驗證，例如，如果您想要讓使用者張貼 HTML 內容 （例如，在部落格文章或 CMS 內容）。</span><span class="sxs-lookup"><span data-stu-id="ee624-310">However, sometimes you want to explicitly disable request validation, such as if you want to let users post HTML content (for example, in blog entries or CMS content).</span></span> <span data-ttu-id="ee624-311">您現在可以新增[AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx)屬性加入模型或檢視模型，以停用模型繫結期間以每個屬性為基礎的要求驗證。</span><span class="sxs-lookup"><span data-stu-id="ee624-311">You can now add an [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) attribute to models or view models to disable request validation on a per-property basis during model binding.</span></span> <span data-ttu-id="ee624-312">如需有關要求驗證的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="ee624-312">For more information about request validation, see the following resources:</span></span>

- <span data-ttu-id="ee624-313">**不顯眼的 JavaScript 和驗證**一節[Scott Guthrie 的部落格文章，在 MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ee624-313">The **Unobtrusive JavaScript and Validation** section in [Scott Guthrie's blog post on the MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).</span></span>
- [<span data-ttu-id="ee624-314">MVC 3 版本資訊</span><span class="sxs-lookup"><span data-stu-id="ee624-314">MVC 3 Release Notes</span></span>](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a><span data-ttu-id="ee624-315">可延伸的 [新增專案] 對話方塊</span><span class="sxs-lookup"><span data-stu-id="ee624-315">Extensible "New Project" Dialog Box</span></span>

<span data-ttu-id="ee624-316">在 ASP.NET MVC 3 中，您可以加入專案範本，檢視引擎和單元測試專案的架構**新的專案** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="ee624-316">In ASP.NET MVC 3 you can add project templates, view engines, and unit test project frameworks to the **New Project** dialog box.</span></span>

### <a name="template-scaffolding-improvements"></a><span data-ttu-id="ee624-317">範本 Scaffolding 增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-317">Template Scaffolding Improvements</span></span>

<span data-ttu-id="ee624-318">ASP.NET MVC 3 scaffolding 範本工作表現較佳的識別模型上的主索引鍵屬性，並處理它們適當地比在舊版的 MVC。</span><span class="sxs-lookup"><span data-stu-id="ee624-318">ASP.NET MVC 3 scaffolding templates do a better job of identifying primary-key properties on models and handling them appropriately than in earlier versions of MVC.</span></span> <span data-ttu-id="ee624-319">（例如，scaffolding 範本現在請確定主索引鍵不會包含 scaffold 的可編輯的表單欄位。）</span><span class="sxs-lookup"><span data-stu-id="ee624-319">(For example, the scaffolding templates now make sure that the primary key is not scaffolded as an editable form field.)</span></span>

<span data-ttu-id="ee624-320">根據預設，建立與編輯架構現在使用`Html.EditorFor`協助程式，而不是`Html.TextBoxFor`協助程式。</span><span class="sxs-lookup"><span data-stu-id="ee624-320">By default, the Create and Edit scaffolds now use the `Html.EditorFor` helper instead of the `Html.TextBoxFor` helper.</span></span> <span data-ttu-id="ee624-321">這可改善支援中的資料形式的模型上的中繼資料屬性註釋**加入檢視**對話方塊就會產生一個檢視。</span><span class="sxs-lookup"><span data-stu-id="ee624-321">This improves support for metadata on the model in the form of data annotation attributes when the **Add View** dialog box generates a view.</span></span>

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a><span data-ttu-id="ee624-322">「 Html.LabelFor"和"Html.LabelForModel 」 的新多載</span><span class="sxs-lookup"><span data-stu-id="ee624-322">New Overloads for "Html.LabelFor" and "Html.LabelForModel"</span></span>

<span data-ttu-id="ee624-323">已新增新的方法多載`LabelFor`和`LabelForModel`helper 方法。</span><span class="sxs-lookup"><span data-stu-id="ee624-323">New method overloads have been added for the `LabelFor` and `LabelForModel` helper methods.</span></span> <span data-ttu-id="ee624-324">新的多載可讓您指定或覆寫的標籤文字。</span><span class="sxs-lookup"><span data-stu-id="ee624-324">The new overloads enable you to specify or override the label text.</span></span>

### <a name="sessionless-controller-support"></a><span data-ttu-id="ee624-325">無工作階段的控制站支援</span><span class="sxs-lookup"><span data-stu-id="ee624-325">Sessionless Controller Support</span></span>

<span data-ttu-id="ee624-326">在 ASP.NET MVC 3，您可以指定是否要使用工作階段狀態的控制器類別，而如果是的話，不論工作階段狀態應該是讀取/寫入或唯寫。</span><span class="sxs-lookup"><span data-stu-id="ee624-326">In ASP.NET MVC 3 you can indicate whether you want a controller class to use session state, and if so, whether session state should be read/write or read-only.</span></span> <span data-ttu-id="ee624-327">如需有關 sessionless 控制器支援的詳細資訊，請參閱[MVC 3 版本資訊](../whitepapers/mvc3-release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="ee624-327">For more information about sessionless controller support, see [MVC 3 Release Notes](../whitepapers/mvc3-release-notes.md).</span></span>

### <a name="new-additionalmetadataattribute-class"></a><span data-ttu-id="ee624-328">新的 「 AdditionalMetadataAttribute"類別</span><span class="sxs-lookup"><span data-stu-id="ee624-328">New "AdditionalMetadataAttribute" Class</span></span>

<span data-ttu-id="ee624-329">您可以使用[AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx)屬性來填入`ModelMetadata.AdditionalValues`模型屬性的字典。</span><span class="sxs-lookup"><span data-stu-id="ee624-329">You can use the [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) attribute to populate the `ModelMetadata.AdditionalValues` dictionary for a model property.</span></span> <span data-ttu-id="ee624-330">例如，如果檢視模型應該僅向系統管理員顯示的屬性，您可以標註該屬性在下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="ee624-330">For example, if a view model has a property that should be displayed only to an administrator, you can annotate that property as shown in the following example:</span></span>

[!code-csharp[Main](mvc3/samples/sample4.cs)]

<span data-ttu-id="ee624-331">此中繼資料開放給任何的顯示器或編輯器範本呈現產品檢視模型時。</span><span class="sxs-lookup"><span data-stu-id="ee624-331">This metadata is made available to any display or editor template when a product view model is rendered.</span></span> <span data-ttu-id="ee624-332">它可以決定要解譯的中繼資料資訊。</span><span class="sxs-lookup"><span data-stu-id="ee624-332">It is up to you to interpret the metadata information.</span></span>

### <a name="accountcontroller-improvements"></a><span data-ttu-id="ee624-333">AccountController 增強功能</span><span class="sxs-lookup"><span data-stu-id="ee624-333">AccountController improvements</span></span>

<span data-ttu-id="ee624-334">AccountController 網際網路專案範本中的已大幅改善。</span><span class="sxs-lookup"><span data-stu-id="ee624-334">The AccountController in the Internet project template has been greatly improved.</span></span>

### <a name="new-intranet-project-template"></a><span data-ttu-id="ee624-335">新的內部網路專案範本</span><span class="sxs-lookup"><span data-stu-id="ee624-335">New Intranet Project Template</span></span>

<span data-ttu-id="ee624-336">新的內部網路專案範本已納入將會啟用 Windows 驗證，然後移除 AccountController。</span><span class="sxs-lookup"><span data-stu-id="ee624-336">A new Intranet Project Template is included which enables Windows Authentication and removes the AccountController.</span></span>
