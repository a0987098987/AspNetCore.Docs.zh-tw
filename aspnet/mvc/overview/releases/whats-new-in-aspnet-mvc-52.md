---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 中最新消息 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a><span data-ttu-id="5219a-102">ASP.NET MVC 5.2 中最新消息</span><span class="sxs-lookup"><span data-stu-id="5219a-102">What’s New in ASP.NET MVC 5.2</span></span>
====================
<span data-ttu-id="5219a-103">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5219a-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="5219a-104">本主題說明什麼是新的 ASP.NET MVC 5.2， [Microsoft.AspNet.MVC 5.2.2 章](#52)和[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span><span class="sxs-lookup"><span data-stu-id="5219a-104">This topic describes what's new for ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) and [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span></span>

- [<span data-ttu-id="5219a-105">軟體需求</span><span class="sxs-lookup"><span data-stu-id="5219a-105">Software Requirements</span></span>](#softRequire)
- [<span data-ttu-id="5219a-106">下載</span><span class="sxs-lookup"><span data-stu-id="5219a-106">Download</span></span>](#download)
- [<span data-ttu-id="5219a-107">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="5219a-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="5219a-108">ASP.NET MVC 5.2 中的新功能</span><span class="sxs-lookup"><span data-stu-id="5219a-108">New Features in ASP.NET MVC 5.2</span></span>](#new-features)

    - [<span data-ttu-id="5219a-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="5219a-109">Attribute routing improvements</span></span>](#attributerouting)
- [<span data-ttu-id="5219a-110">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="5219a-110">Known Issues and Breaking Changes</span></span>](#knownbreakingchanges)
- [<span data-ttu-id="5219a-111">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="5219a-111">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="5219a-112">Microsoft.AspNet.MVC 5.2.2 章</span><span class="sxs-lookup"><span data-stu-id="5219a-112">Microsoft.AspNet.MVC 5.2.2</span></span>](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a><span data-ttu-id="5219a-113">軟體需求</span><span class="sxs-lookup"><span data-stu-id="5219a-113">Software Requirements</span></span>

- <span data-ttu-id="5219a-114">下載 visual Studio 2012: [ASP.NET 及 Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)。</span><span class="sxs-lookup"><span data-stu-id="5219a-114">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="5219a-115">下載 visual Studio 2013: [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="5219a-115">Visual Studio 2013: Download [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) or higher.</span></span> <span data-ttu-id="5219a-116">編輯 ASP.NET MVC 5.2 Razor 檢視需要此更新。</span><span class="sxs-lookup"><span data-stu-id="5219a-116">This update is needed for editing ASP.NET MVC 5.2 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="5219a-117">下載</span><span class="sxs-lookup"><span data-stu-id="5219a-117">Download</span></span>

<span data-ttu-id="5219a-118">執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="5219a-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="5219a-119">所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="5219a-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="5219a-120">最新的 ASP.NET MVC 5.2 封裝有下列版本:"5.2.0"。</span><span class="sxs-lookup"><span data-stu-id="5219a-120">The latest ASP.NET MVC 5.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="5219a-121">您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。</span><span class="sxs-lookup"><span data-stu-id="5219a-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="5219a-122">此版也包含對應當地語系化的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="5219a-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="5219a-123">您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="5219a-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

<span data-ttu-id="5219a-124">安裝套件 Microsoft.AspNet.Mvc-5.2.0 版</span><span class="sxs-lookup"><span data-stu-id="5219a-124">Install-Package Microsoft.AspNet.Mvc -Version 5.2.0</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="5219a-125">文件</span><span class="sxs-lookup"><span data-stu-id="5219a-125">Documentation</span></span>

<span data-ttu-id="5219a-126">教學課程和其他關於 ASP.NET MVC 5.2 資訊都是從 ASP.NET 網站 ([https://www.asp.net/mvc](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="5219a-126">Tutorials and other information about ASP.NET MVC 5.2 are available from the ASP.NET web site ([https://www.asp.net/mvc](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a><span data-ttu-id="5219a-127">ASP.NET MVC 5.2 中的新功能</span><span class="sxs-lookup"><span data-stu-id="5219a-127">New Features in ASP.NET MVC 5.2</span></span>

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="5219a-128">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="5219a-128">Attribute routing improvements</span></span>

<span data-ttu-id="5219a-129">屬性現在路由提供稱為 IDirectRouteProvider，可讓您探索及設定屬性路由的方式的完整控制權的擴充點。</span><span class="sxs-lookup"><span data-stu-id="5219a-129">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="5219a-130">IDirectRouteProvider 負責提供動作與控制器以及相關聯的路由資訊來指定完全何種路由組態所需的那些動作的清單。</span><span class="sxs-lookup"><span data-stu-id="5219a-130">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="5219a-131">呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能指定 IDirectRouteProvider 實作。</span><span class="sxs-lookup"><span data-stu-id="5219a-131">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="5219a-132">自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作、 DefaultDirectRouteProvider 延伸。</span><span class="sxs-lookup"><span data-stu-id="5219a-132">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="5219a-133">這個類別會提供個別可覆寫的虛擬方法，若要變更探索屬性、 建立路由項目，以及探索路由前置字元和區域前置詞的邏輯。</span><span class="sxs-lookup"><span data-stu-id="5219a-133">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="5219a-134">使用新屬性路由擴充功能的 IDirectRouteProvider，使用者可以執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="5219a-134">With the new attribute routing extensibility of IDirectRouteProvider, a user could do the following:</span></span>

1. <span data-ttu-id="5219a-135">支援的屬性路由的繼承。</span><span class="sxs-lookup"><span data-stu-id="5219a-135">Support Inheritance of attribute routes.</span></span> <span data-ttu-id="5219a-136">例如，在下列案例中部落格和存放區的控制站使用 BaseController 所定義的屬性路由慣例。</span><span class="sxs-lookup"><span data-stu-id="5219a-136">For example, in the following scenario Blog and Store controllers are using an attribute route convention that is defined by the BaseController.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. <span data-ttu-id="5219a-137">自動產生屬性路由的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="5219a-137">Automatically generate route names for attribute routes.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. <span data-ttu-id="5219a-138">修改路由前置詞在單一中央位置，才能路由加入至路由表。</span><span class="sxs-lookup"><span data-stu-id="5219a-138">Modify route prefixes in one central place before the routes get added to the route table.</span></span>
4. <span data-ttu-id="5219a-139">篩選掉您想要尋找的屬性路由的控制器。</span><span class="sxs-lookup"><span data-stu-id="5219a-139">Filter out the controllers on which you want the attribute routing to look for.</span></span> <span data-ttu-id="5219a-140">我們希望 3 和 4 上的部落格推出。</span><span class="sxs-lookup"><span data-stu-id="5219a-140">We hope to blog on 3 and 4 soon.</span></span>

### <a name="facebook-fixes-for-changed-api-surface"></a><span data-ttu-id="5219a-141">Facebook 修正已變更的 API 介面</span><span class="sxs-lookup"><span data-stu-id="5219a-141">Facebook fixes for changed API surface</span></span>

<span data-ttu-id="5219a-142">MVC Facebook 封裝[中斷](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)因為 Facebook 所做的一些應用程式開發介面變更。</span><span class="sxs-lookup"><span data-stu-id="5219a-142">The MVC Facebook package [was broken](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) due to few API changes made by Facebook.</span></span> <span data-ttu-id="5219a-143">我們也將推出新的 Facebook 封裝以修正這些問題的 (Microsoft.AspNet.Facebook 1.0.0)。</span><span class="sxs-lookup"><span data-stu-id="5219a-143">We are also releasing a new Facebook package (Microsoft.AspNet.Facebook 1.0.0) to fix these issues.</span></span>

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="5219a-144">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="5219a-144">Known Issues and Breaking Changes</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="5219a-145">在專案中使用 5.2.0 到 scaffolding MVC/Web API 找出已不存在於專案中封裝的封裝導致 5.1.2</span><span class="sxs-lookup"><span data-stu-id="5219a-145">Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="5219a-146">ASP.NET mvc 5.2.0 更新 NuGet 封裝不會更新的 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="5219a-146">Updating NuGet packages for ASP.NET MVC 5.2.0 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="5219a-147">他們使用的舊版 ASP.NET 執行階段封裝 (例如 5.1.2 在 Update 2)。</span><span class="sxs-lookup"><span data-stu-id="5219a-147">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="5219a-148">如此一來，ASP.NET scaffolding 會安裝舊版 (例如 5.1.2 在 Update 2) 必要的封裝，如果還沒有出現在您的專案。</span><span class="sxs-lookup"><span data-stu-id="5219a-148">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="5219a-149">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。</span><span class="sxs-lookup"><span data-stu-id="5219a-149">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="5219a-150">如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新專案中的封裝之後，請確定一致的 Web API 和 ASP.NET MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="5219a-150">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a><span data-ttu-id="5219a-151">Microsoft.jQuery.Unobtrusive.Validation NuGet 套件安裝失敗，因為它是找不到 Microsoft.jQuery.Unobtrusive.Validation 的版本相容，jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="5219a-151">Microsoft.jQuery.Unobtrusive.Validation NuGet package installation fails because it is unable to find a version of Microsoft.jQuery.Unobtrusive.Validation compatible to jQuery 1.4.1</span></span>

<span data-ttu-id="5219a-152">Microsoft.jQuery.Unobtrusive.Validation 需要 jQuery &gt;= 1.8 和 jQuery.Validation &gt;= 1.8。</span><span class="sxs-lookup"><span data-stu-id="5219a-152">Microsoft.jQuery.Unobtrusive.Validation requires jQuery &gt;=1.8 and jQuery.Validation &gt;=1.8.</span></span> <span data-ttu-id="5219a-153">But,jQuery.Validation (1.8) 需要 jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。</span><span class="sxs-lookup"><span data-stu-id="5219a-153">But,jQuery.Validation (1.8) needs jQuery (&#8805; 1.3.2 &amp;&amp; &#8804; 1.6).</span></span> <span data-ttu-id="5219a-154">因為這個緣故，NuGet 會 JQuery 1.8 和 jQuery.Validation 1.8 安裝在相同的時間，它就會失敗。</span><span class="sxs-lookup"><span data-stu-id="5219a-154">Because of this, when NuGet installs the JQuery 1.8 and jQuery.Validation 1.8 at the same time, it fails.</span></span> <span data-ttu-id="5219a-155">當您看到這個問題時，您僅更新至 jQuery.Validation 版本&gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)具有固定的 jQuery cap 首先，您應該能夠安裝Microsoft.jQuery.Unobtrusive.Validation。</span><span class="sxs-lookup"><span data-stu-id="5219a-155">When you see this issue, you can simply update the version of jQuery.Validation to &gt;= [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) which has the jQuery cap fixed first, you should be able to install Microsoft.jQuery.Unobtrusive.Validation.</span></span>

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a><span data-ttu-id="5219a-156">Jquery。驗證 nuget 封裝版本 1.13.0 無法辨識某些國際電子郵件地址</span><span class="sxs-lookup"><span data-stu-id="5219a-156">The jquery.Validation nuget package version 1.13.0 does not recognize some international email addresses</span></span>

<span data-ttu-id="5219a-157">jQuery.Validation nuget 封裝版本 1.11.1 是最後已知的版本可辨識下列有效的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="5219a-157">jQuery.Validation nuget package version 1.11.1 is the last known version which recognizes following valid email addresses.</span></span> <span data-ttu-id="5219a-158">任何更新的版本可能無法辨識它們。</span><span class="sxs-lookup"><span data-stu-id="5219a-158">Any later versions might not be able to recognize them.</span></span> <span data-ttu-id="5219a-159">例如: </span><span class="sxs-lookup"><span data-stu-id="5219a-159">For example:</span></span>

<span data-ttu-id="5219a-160">電子郵件地址國際化 (EAI) 標準 (例如[&#29992; &#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))</span><span class="sxs-lookup"><span data-stu-id="5219a-160">E-Mail Address Internationalization (EAI) standard (e.g., [&#29992;&#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))</span></span>   
 <span data-ttu-id="5219a-161">EAI + 國際化資源識別元 （光圈） (例如。， [（& s) #29992; &#25143; @ （& s) #1076; （& s) #1086; （& s) #1084; （& s) #1077; （& s) #1085;。&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span><span class="sxs-lookup"><span data-stu-id="5219a-161">EAI + Internationalized Resource Identifiers (IRIs) (eg., [&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span></span>

<span data-ttu-id="5219a-162">在報告的問題[https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span><span class="sxs-lookup"><span data-stu-id="5219a-162">The issue is reported at [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span></span>

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="5219a-163">Visual Studio 2013 中的 Razor 檢視的反白顯示語法</span><span class="sxs-lookup"><span data-stu-id="5219a-163">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="5219a-164">如果您將更新 ASP.NET MVC 5.2，但未更新 Visual Studio 2013，您會取得 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="5219a-164">If you update to ASP.NET MVC 5.2 without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="5219a-165">您必須更新 Visual Studio 2013，若要取得這項支援。</span><span class="sxs-lookup"><span data-stu-id="5219a-165">You will need to update Visual Studio 2013 to get this support.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="5219a-166">Bug 修正和次要功能更新</span><span class="sxs-lookup"><span data-stu-id="5219a-166">Bug Fixes and Minor feature updates</span></span>

<span data-ttu-id="5219a-167">此版本也包含數個 bug 修正和次要功能更新。</span><span class="sxs-lookup"><span data-stu-id="5219a-167">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="5219a-168">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="5219a-168">You can find the complete list here:</span></span>

- [<span data-ttu-id="5219a-169">5.2 封裝</span><span class="sxs-lookup"><span data-stu-id="5219a-169">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a><span data-ttu-id="5219a-170">Microsoft.AspNet.MVC 5.2.2 章</span><span class="sxs-lookup"><span data-stu-id="5219a-170">Microsoft.AspNet.MVC 5.2.2</span></span>

<span data-ttu-id="5219a-171">在 MVC 中沒有任何新功能或 bug 修正這一版。</span><span class="sxs-lookup"><span data-stu-id="5219a-171">This release doesn't have any new features or bug fixes in MVC.</span></span> <span data-ttu-id="5219a-172">我們做了[網頁中變更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)顯著的效能改進及後續更新我們擁有相依於這個新版本的 Web 網頁的所有其他相依套件。</span><span class="sxs-lookup"><span data-stu-id="5219a-172">We made a [change in Web Pages](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) for a significant performance improvement and have subsequently updated all other dependent packages we own to depend on this new version of Web Pages.</span></span>

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a><span data-ttu-id="5219a-173">ASP.NET MVC 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="5219a-173">ASP.NET MVC 5.2.3 Beta</span></span>

<span data-ttu-id="5219a-174">您可以閱讀發行[這裡](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。</span><span class="sxs-lookup"><span data-stu-id="5219a-174">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="5219a-175">此版本包含只 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="5219a-175">This release contains only bug fixes.</span></span> <span data-ttu-id="5219a-176">您可以使用[此查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。</span><span class="sxs-lookup"><span data-stu-id="5219a-176">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
