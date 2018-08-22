---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: 什麼是 ASP.NET MVC 5.2 的新功能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 8c3c5de55396635d2e7f2b7726f54be1c06bb691
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834637"
---
<a name="whats-new-in-aspnet-mvc-52"></a><span data-ttu-id="ecaac-102">什麼是 ASP.NET MVC 5.2 的新功能</span><span class="sxs-lookup"><span data-stu-id="ecaac-102">What’s New in ASP.NET MVC 5.2</span></span>
====================
<span data-ttu-id="ecaac-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ecaac-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="ecaac-104">本主題描述適用於 ASP.NET MVC 5.2，最新消息[Microsoft.AspNet.MVC 5.2.2](#52)和[ASP.NET MVC 5.2.3 beta 版](#mvc523Beta)</span><span class="sxs-lookup"><span data-stu-id="ecaac-104">This topic describes what's new for ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) and [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span></span>

- [<span data-ttu-id="ecaac-105">軟體需求</span><span class="sxs-lookup"><span data-stu-id="ecaac-105">Software Requirements</span></span>](#softRequire)
- [<span data-ttu-id="ecaac-106">下載</span><span class="sxs-lookup"><span data-stu-id="ecaac-106">Download</span></span>](#download)
- [<span data-ttu-id="ecaac-107">文件</span><span class="sxs-lookup"><span data-stu-id="ecaac-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="ecaac-108">ASP.NET mvc 5.2 的新功能</span><span class="sxs-lookup"><span data-stu-id="ecaac-108">New Features in ASP.NET MVC 5.2</span></span>](#new-features)

    - [<span data-ttu-id="ecaac-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="ecaac-109">Attribute routing improvements</span></span>](#attributerouting)
- [<span data-ttu-id="ecaac-110">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="ecaac-110">Known Issues and Breaking Changes</span></span>](#knownbreakingchanges)
- [<span data-ttu-id="ecaac-111">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="ecaac-111">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="ecaac-112">Microsoft.AspNet.MVC 5.2.2</span><span class="sxs-lookup"><span data-stu-id="ecaac-112">Microsoft.AspNet.MVC 5.2.2</span></span>](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a><span data-ttu-id="ecaac-113">軟體需求</span><span class="sxs-lookup"><span data-stu-id="ecaac-113">Software Requirements</span></span>

- <span data-ttu-id="ecaac-114">Visual Studio 2012： 下載[ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062)。</span><span class="sxs-lookup"><span data-stu-id="ecaac-114">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="ecaac-115">Visual Studio 2013： 下載[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ecaac-115">Visual Studio 2013: Download [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) or higher.</span></span> <span data-ttu-id="ecaac-116">此更新所需的編輯 ASP.NET MVC 5.2 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="ecaac-116">This update is needed for editing ASP.NET MVC 5.2 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="ecaac-117">下載</span><span class="sxs-lookup"><span data-stu-id="ecaac-117">Download</span></span>

<span data-ttu-id="ecaac-118">執行階段功能會以 NuGet 套件，NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="ecaac-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="ecaac-119">所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="ecaac-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="ecaac-120">最新的 ASP.NET MVC 5.2 套件有下列版本:"5.2.0 」。</span><span class="sxs-lookup"><span data-stu-id="ecaac-120">The latest ASP.NET MVC 5.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="ecaac-121">您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。</span><span class="sxs-lookup"><span data-stu-id="ecaac-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="ecaac-122">此版也包含在 NuGet 上的對應當地語系化的套件。</span><span class="sxs-lookup"><span data-stu-id="ecaac-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="ecaac-123">您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="ecaac-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

<span data-ttu-id="ecaac-124">安裝套件 Microsoft.AspNet.Mvc-5.2.0 版</span><span class="sxs-lookup"><span data-stu-id="ecaac-124">Install-Package Microsoft.AspNet.Mvc -Version 5.2.0</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="ecaac-125">文件</span><span class="sxs-lookup"><span data-stu-id="ecaac-125">Documentation</span></span>

<span data-ttu-id="ecaac-126">教學課程和 ASP.NET MVC 5.2 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/mvc](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="ecaac-126">Tutorials and other information about ASP.NET MVC 5.2 are available from the ASP.NET web site ([https://www.asp.net/mvc](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a><span data-ttu-id="ecaac-127">ASP.NET mvc 5.2 的新功能</span><span class="sxs-lookup"><span data-stu-id="ecaac-127">New Features in ASP.NET MVC 5.2</span></span>

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="ecaac-128">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="ecaac-128">Attribute routing improvements</span></span>

<span data-ttu-id="ecaac-129">屬性路由現在提供稱為 IDirectRouteProvider，可讓您完整控制如何屬性路由會探索及設定的擴充點。</span><span class="sxs-lookup"><span data-stu-id="ecaac-129">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="ecaac-130">IDirectRouteProvider 負責提供一份動作與控制器相關聯的路由資訊，來指定這些動作只需要何種路由的設定。</span><span class="sxs-lookup"><span data-stu-id="ecaac-130">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="ecaac-131">呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能會指定 IDirectRouteProvider 實作。</span><span class="sxs-lookup"><span data-stu-id="ecaac-131">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="ecaac-132">自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作，DefaultDirectRouteProvider 延伸。</span><span class="sxs-lookup"><span data-stu-id="ecaac-132">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="ecaac-133">這個類別會提供不同可覆寫的虛擬方法，以變更邏輯探索屬性、 建立路由項目，以及探索路由前置詞和區域前置詞。</span><span class="sxs-lookup"><span data-stu-id="ecaac-133">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="ecaac-134">使用新屬性路由的擴充性 IDirectRouteProvider，使用者可以執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="ecaac-134">With the new attribute routing extensibility of IDirectRouteProvider, a user could do the following:</span></span>

1. <span data-ttu-id="ecaac-135">支援的屬性路由的繼承。</span><span class="sxs-lookup"><span data-stu-id="ecaac-135">Support Inheritance of attribute routes.</span></span> <span data-ttu-id="ecaac-136">例如，在下列案例中的部落格和存放區的控制站使用 BaseController 所定義的屬性路由慣例。</span><span class="sxs-lookup"><span data-stu-id="ecaac-136">For example, in the following scenario Blog and Store controllers are using an attribute route convention that is defined by the BaseController.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. <span data-ttu-id="ecaac-137">自動產生屬性路由的路由的名稱。</span><span class="sxs-lookup"><span data-stu-id="ecaac-137">Automatically generate route names for attribute routes.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. <span data-ttu-id="ecaac-138">路由加入至路由表之前，請修改路由前置詞，在一個集中位置。</span><span class="sxs-lookup"><span data-stu-id="ecaac-138">Modify route prefixes in one central place before the routes get added to the route table.</span></span>
4. <span data-ttu-id="ecaac-139">篩選出您想要尋找的屬性路由的控制器。</span><span class="sxs-lookup"><span data-stu-id="ecaac-139">Filter out the controllers on which you want the attribute routing to look for.</span></span> <span data-ttu-id="ecaac-140">我們很快就希望部落格上 3 和 4。</span><span class="sxs-lookup"><span data-stu-id="ecaac-140">We hope to blog on 3 and 4 soon.</span></span>

### <a name="facebook-fixes-for-changed-api-surface"></a><span data-ttu-id="ecaac-141">Facebook 的修正程式已變更的 API 介面</span><span class="sxs-lookup"><span data-stu-id="ecaac-141">Facebook fixes for changed API surface</span></span>

<span data-ttu-id="ecaac-142">MVC Facebook 封裝[已中斷](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)因為 Facebook 所做的一些 API 變更。</span><span class="sxs-lookup"><span data-stu-id="ecaac-142">The MVC Facebook package [was broken](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) due to few API changes made by Facebook.</span></span> <span data-ttu-id="ecaac-143">我們也將推出新的 Facebook 封裝來修正這些問題的 (Microsoft.AspNet.Facebook 1.0.0)。</span><span class="sxs-lookup"><span data-stu-id="ecaac-143">We are also releasing a new Facebook package (Microsoft.AspNet.Facebook 1.0.0) to fix these issues.</span></span>

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="ecaac-144">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="ecaac-144">Known Issues and Breaking Changes</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="ecaac-145">Scaffolding MVC/Web API，到使用 5.2.0 專案不存在專案中的封裝的封裝導致 5.1.2</span><span class="sxs-lookup"><span data-stu-id="ecaac-145">Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="ecaac-146">ASP.NET mvc 5.2.0 更新 NuGet 套件不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="ecaac-146">Updating NuGet packages for ASP.NET MVC 5.2.0 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="ecaac-147">他們使用舊版 ASP.NET 執行階段套件 (例如在 Update 2 的 5.1.2)。</span><span class="sxs-lookup"><span data-stu-id="ecaac-147">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="ecaac-148">如此一來，ASP.NET scaffolding 會安裝必要的套件，先前的版本 (例如在 Update 2 的 5.1.2)，如果它們還不在您的專案中。</span><span class="sxs-lookup"><span data-stu-id="ecaac-148">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="ecaac-149">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET 樣板不能覆寫您在專案中最新的套件。</span><span class="sxs-lookup"><span data-stu-id="ecaac-149">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="ecaac-150">如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新您的專案的封裝之後，請確定 Web API 和 ASP.NET MVC 的版本不一致。</span><span class="sxs-lookup"><span data-stu-id="ecaac-150">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a><span data-ttu-id="ecaac-151">Microsoft.jQuery.Unobtrusive.Validation NuGet 套件安裝失敗，因為它是找不到 Microsoft.jQuery.Unobtrusive.Validation 的版本相容，jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ecaac-151">Microsoft.jQuery.Unobtrusive.Validation NuGet package installation fails because it is unable to find a version of Microsoft.jQuery.Unobtrusive.Validation compatible to jQuery 1.4.1</span></span>

<span data-ttu-id="ecaac-152">Microsoft.jQuery.Unobtrusive.Validation 需要 jQuery &gt;= 1.8 和 jQuery.Validation &gt;= 1.8。</span><span class="sxs-lookup"><span data-stu-id="ecaac-152">Microsoft.jQuery.Unobtrusive.Validation requires jQuery &gt;=1.8 and jQuery.Validation &gt;=1.8.</span></span> <span data-ttu-id="ecaac-153">But,jQuery.Validation (1.8) 需要 jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。</span><span class="sxs-lookup"><span data-stu-id="ecaac-153">But,jQuery.Validation (1.8) needs jQuery (&#8805; 1.3.2 &amp;&amp; &#8804; 1.6).</span></span> <span data-ttu-id="ecaac-154">因為這個緣故，如果 NuGet 所安裝的 JQuery 1.8 和 jQuery.Validation 1.8 在此同時，就會失敗。</span><span class="sxs-lookup"><span data-stu-id="ecaac-154">Because of this, when NuGet installs the JQuery 1.8 and jQuery.Validation 1.8 at the same time, it fails.</span></span> <span data-ttu-id="ecaac-155">當您看到此問題時，則只要更新至 jQuery.Validation 新版&gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)具有固定的 jQuery 上限前，您應該能夠安裝Microsoft.jQuery.Unobtrusive.Validation。</span><span class="sxs-lookup"><span data-stu-id="ecaac-155">When you see this issue, you can simply update the version of jQuery.Validation to &gt;= [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) which has the jQuery cap fixed first, you should be able to install Microsoft.jQuery.Unobtrusive.Validation.</span></span>

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a><span data-ttu-id="ecaac-156">Jquery。驗證 nuget 套件版本 1.13.0 無法辨識某些國際電子郵件地址</span><span class="sxs-lookup"><span data-stu-id="ecaac-156">The jquery.Validation nuget package version 1.13.0 does not recognize some international email addresses</span></span>

<span data-ttu-id="ecaac-157">jQuery.Validation nuget 套件版本 1.11.1 是最後一個已知的版本可辨識下列有效的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="ecaac-157">jQuery.Validation nuget package version 1.11.1 is the last known version which recognizes following valid email addresses.</span></span> <span data-ttu-id="ecaac-158">任何更新的版本可能無法辨識它們。</span><span class="sxs-lookup"><span data-stu-id="ecaac-158">Any later versions might not be able to recognize them.</span></span> <span data-ttu-id="ecaac-159">例如: </span><span class="sxs-lookup"><span data-stu-id="ecaac-159">For example:</span></span>

<span data-ttu-id="ecaac-160">電子郵件地址國際化 (EAI) 標準 (例如[ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))</span><span class="sxs-lookup"><span data-stu-id="ecaac-160">E-Mail Address Internationalization (EAI) standard (e.g., [&#29992;&#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))</span></span>   
 <span data-ttu-id="ecaac-161">EAI + 國際化資源識別項 (Iri) (例如。， [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;。&#1088;&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span><span class="sxs-lookup"><span data-stu-id="ecaac-161">EAI + Internationalized Resource Identifiers (IRIs) (eg., [&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span></span>

<span data-ttu-id="ecaac-162">在報告的問題 [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span><span class="sxs-lookup"><span data-stu-id="ecaac-162">The issue is reported at [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span></span>

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="ecaac-163">Visual Studio 2013 中的 Razor 檢視的語法醒目提示</span><span class="sxs-lookup"><span data-stu-id="ecaac-163">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="ecaac-164">如果您將更新 ASP.NET MVC 5.2，而不需要更新 Visual Studio 2013，您會取得 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="ecaac-164">If you update to ASP.NET MVC 5.2 without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="ecaac-165">您必須更新以取得這項支援的 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="ecaac-165">You will need to update Visual Studio 2013 to get this support.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="ecaac-166">Bug 修正和次要功能更新</span><span class="sxs-lookup"><span data-stu-id="ecaac-166">Bug Fixes and Minor feature updates</span></span>

<span data-ttu-id="ecaac-167">此版本也包含數個 bug 修正和次要功能更新。</span><span class="sxs-lookup"><span data-stu-id="ecaac-167">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="ecaac-168">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="ecaac-168">You can find the complete list here:</span></span>

- [<span data-ttu-id="ecaac-169">5.2 封裝</span><span class="sxs-lookup"><span data-stu-id="ecaac-169">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a><span data-ttu-id="ecaac-170">Microsoft.AspNet.MVC 5.2.2</span><span class="sxs-lookup"><span data-stu-id="ecaac-170">Microsoft.AspNet.MVC 5.2.2</span></span>

<span data-ttu-id="ecaac-171">在 MVC 中沒有任何新功能或 bug 修正這一版。</span><span class="sxs-lookup"><span data-stu-id="ecaac-171">This release doesn't have any new features or bug fixes in MVC.</span></span> <span data-ttu-id="ecaac-172">我們所做[網頁中變更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)顯著的效能改進，隨後更新現存相依於這個新版網頁的所有其他相依套件。</span><span class="sxs-lookup"><span data-stu-id="ecaac-172">We made a [change in Web Pages](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) for a significant performance improvement and have subsequently updated all other dependent packages we own to depend on this new version of Web Pages.</span></span>

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a><span data-ttu-id="ecaac-173">ASP.NET MVC 5.2.3 beta 版</span><span class="sxs-lookup"><span data-stu-id="ecaac-173">ASP.NET MVC 5.2.3 Beta</span></span>

<span data-ttu-id="ecaac-174">您可以閱讀發行[此處](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ecaac-174">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="ecaac-175">此版本包含只 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="ecaac-175">This release contains only bug fixes.</span></span> <span data-ttu-id="ecaac-176">您可以使用[這項查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。</span><span class="sxs-lookup"><span data-stu-id="ecaac-176">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
