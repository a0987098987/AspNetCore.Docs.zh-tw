---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 中最新消息 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="d646d-102">ASP.NET MVC 5.1 中最新消息</span><span class="sxs-lookup"><span data-stu-id="d646d-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="d646d-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d646d-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="d646d-104">本主題說明 ASP.NET Web MVC 5.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="d646d-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="d646d-105">軟體需求</span><span class="sxs-lookup"><span data-stu-id="d646d-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="d646d-106">下載</span><span class="sxs-lookup"><span data-stu-id="d646d-106">Download</span></span>](#download)
- [<span data-ttu-id="d646d-107">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="d646d-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="d646d-108">ASP.NET MVC 5.1 的新功能</span><span class="sxs-lookup"><span data-stu-id="d646d-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="d646d-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="d646d-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="d646d-110">啟動程序支援編輯器範本</span><span class="sxs-lookup"><span data-stu-id="d646d-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="d646d-111">列舉檢視中的支援</span><span class="sxs-lookup"><span data-stu-id="d646d-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="d646d-112">不顯眼的 MinLength/MaxLength 屬性驗證</span><span class="sxs-lookup"><span data-stu-id="d646d-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="d646d-113">支援不顯眼的 Ajax 中的 'this' 內容</span><span class="sxs-lookup"><span data-stu-id="d646d-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="d646d-114">[已知問題和重大變更](#KnownBreakingChanges)- [Bug 修正](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="d646d-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="d646d-115">軟體需求</span><span class="sxs-lookup"><span data-stu-id="d646d-115">Software Requirements</span></span>

- <span data-ttu-id="d646d-116">下載 visual Studio 2012: [ASP.NET 及 Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)。</span><span class="sxs-lookup"><span data-stu-id="d646d-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="d646d-117">下載 visual Studio 2013: [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064)。</span><span class="sxs-lookup"><span data-stu-id="d646d-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="d646d-118">編輯 ASP.NET MVC 5.1 Razor 檢視需要此更新。</span><span class="sxs-lookup"><span data-stu-id="d646d-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="d646d-119">下載</span><span class="sxs-lookup"><span data-stu-id="d646d-119">Download</span></span>

<span data-ttu-id="d646d-120">執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="d646d-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="d646d-121">所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="d646d-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="d646d-122">最新的 ASP.NET MVC 5.1 RTM 封裝有下列版本:"5.1.2"。</span><span class="sxs-lookup"><span data-stu-id="d646d-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="d646d-123">您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)。</span><span class="sxs-lookup"><span data-stu-id="d646d-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="d646d-124">此版也包含對應當地語系化的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="d646d-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="d646d-125">您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="d646d-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="d646d-126">文件</span><span class="sxs-lookup"><span data-stu-id="d646d-126">Documentation</span></span>

<span data-ttu-id="d646d-127">教學課程和 ASP.NET MVC 5.1 RTM 的其他資訊都是從 ASP.NET 網站 ( https://www.asp.net)。</span><span class="sxs-lookup"><span data-stu-id="d646d-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="d646d-128">ASP.NET MVC 5.1 的新功能</span><span class="sxs-lookup"><span data-stu-id="d646d-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="d646d-129">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="d646d-129">Attribute routing improvements</span></span>

 <span data-ttu-id="d646d-130">路由現在支援條件約束，啟用版本控制和標頭的屬性型路由選取。</span><span class="sxs-lookup"><span data-stu-id="d646d-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="d646d-131">現在可透過自訂屬性路由的許多層面是`IDirectRouteFactory`介面和`RouteFactoryAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="d646d-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="d646d-132">路由前置字元現在是可透過延伸`IRoutePrefix`介面和`RoutePrefixAttribute`類別。</span><span class="sxs-lookup"><span data-stu-id="d646d-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="d646d-133">列舉檢視中的支援</span><span class="sxs-lookup"><span data-stu-id="d646d-133">Enum support in views</span></span>

1. <span data-ttu-id="d646d-134">新`@Html.EnumDropDownListFor()`helper 方法。</span><span class="sxs-lookup"><span data-stu-id="d646d-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="d646d-135">這些應該用像大部分的 HTML helper，但請注意，運算式必須評估為[列舉](https://msdn.microsoft.com/en-us/library/cc138362.aspx)類型或[Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)其中`T`是[列舉](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型別。</span><span class="sxs-lookup"><span data-stu-id="d646d-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="d646d-136">使用`EnumHelper.IsValidForEnumHelper()`來檢查這些需求。</span><span class="sxs-lookup"><span data-stu-id="d646d-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="d646d-137">新`EnumHelper.GetSelectList()`方法會傳回`IList<SelectListItem>`。</span><span class="sxs-lookup"><span data-stu-id="d646d-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="d646d-138">當您需要管理選取清單之前呼叫，例如，這非常有用`@Html.DropDownListFor()`，或當您想要顯示的名稱這`@Html.EnumDropDownListFor()`顯示。</span><span class="sxs-lookup"><span data-stu-id="d646d-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="d646d-139">下列程式碼會示範這些 Api。</span><span class="sxs-lookup"><span data-stu-id="d646d-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="d646d-140">您可以看到完整的範例[這裡](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/)。</span><span class="sxs-lookup"><span data-stu-id="d646d-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="d646d-141">啟動程序支援編輯器範本</span><span class="sxs-lookup"><span data-stu-id="d646d-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="d646d-142">我們現在允許 HTML 屬性中傳遞[EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx)為[匿名物件](https://msdn.microsoft.com/en-us/library/bb397696.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d646d-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="d646d-143">例如: </span><span class="sxs-lookup"><span data-stu-id="d646d-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="d646d-144">MinLengthAttribute 和 MaxLengthAttribute 不顯眼的驗證</span><span class="sxs-lookup"><span data-stu-id="d646d-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="d646d-145">字串和陣列類型的用戶端驗證將現在支援的屬性以裝飾[MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx)和[MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="d646d-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="d646d-146">支援不顯眼的 Ajax 中的 'this' 內容</span><span class="sxs-lookup"><span data-stu-id="d646d-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="d646d-147">回呼函式 (`OnBegin, OnComplete, OnFailure, OnSuccess`) 現在可以找出透過叫用的項目`this`內容。</span><span class="sxs-lookup"><span data-stu-id="d646d-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="d646d-148">例如: </span><span class="sxs-lookup"><span data-stu-id="d646d-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d646d-149">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="d646d-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="d646d-150">路由屬性</span><span class="sxs-lookup"><span data-stu-id="d646d-150">Attribute Routing</span></span>

<span data-ttu-id="d646d-151">發生錯誤，而不是選擇第一個相符項目，現在將會報告屬性路由相符項目中的模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="d646d-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="d646d-152">屬性路由嚴禁`{controller}`參數，並從使用`{action}`路由參數放在 [動作]。</span><span class="sxs-lookup"><span data-stu-id="d646d-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="d646d-153">這些參數的用法很可能會導致模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="d646d-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="d646d-154">Scaffolding MVC/Web 應用程式開發介面到 5.1 封裝在中使用結果 5.0 封裝還不存在專案中的專案</span><span class="sxs-lookup"><span data-stu-id="d646d-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="d646d-155">ASP.NET MVC 5.1 rtm 更新 NuGet 封裝不會更新的 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="d646d-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="d646d-156">他們使用的舊版 ASP.NET 執行階段封裝 (5.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="d646d-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="d646d-157">如此一來，ASP.NET scaffolding 會安裝必要的封裝前, 一版 (5.0.0.0)，如果還沒有出現在您的專案。</span><span class="sxs-lookup"><span data-stu-id="d646d-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="d646d-158">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。</span><span class="sxs-lookup"><span data-stu-id="d646d-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="d646d-159">如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新專案中的封裝之後，請確定一致的 Web API 和 ASP.NET MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="d646d-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="d646d-160">Visual Studio 2013 中的 Razor 檢視的反白顯示語法</span><span class="sxs-lookup"><span data-stu-id="d646d-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="d646d-161">如果您未更新 Visual Studio 2013 更新至 ASP.NET MVC 5.1 RTM，您才會得到 Visual Studio 編輯器支援語法反白顯示在編輯 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="d646d-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="d646d-162">您必須更新 Visual Studio 2013，若要取得這項支援。</span><span class="sxs-lookup"><span data-stu-id="d646d-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="d646d-163">型別重新命名</span><span class="sxs-lookup"><span data-stu-id="d646d-163">Type Renames</span></span>

<span data-ttu-id="d646d-164">某些屬性的路由擴充性所使用的型別會重新命名 5.1 RTM 的侷限。</span><span class="sxs-lookup"><span data-stu-id="d646d-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="d646d-165">**舊的型別名稱 (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="d646d-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="d646d-166">**新的類型名稱 (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="d646d-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="d646d-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="d646d-167">IDirectRouteProvider</span></span> | <span data-ttu-id="d646d-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="d646d-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="d646d-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="d646d-169">RouteProviderAttribute</span></span> | <span data-ttu-id="d646d-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="d646d-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="d646d-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="d646d-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="d646d-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="d646d-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="d646d-173">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="d646d-173">Bug Fixes</span></span>

<span data-ttu-id="d646d-174">此版本也包含數個 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="d646d-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="d646d-175">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="d646d-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="d646d-176">5.1.0 封裝</span><span class="sxs-lookup"><span data-stu-id="d646d-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="d646d-177">5.1.1 封裝</span><span class="sxs-lookup"><span data-stu-id="d646d-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="d646d-178">5.1.2 封裝包含 IntelliSense 的更新，但沒有 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="d646d-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
