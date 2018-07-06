---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 中最新消息 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838399"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="b2098-102">ASP.NET Web API 2.1 中最新消息</span><span class="sxs-lookup"><span data-stu-id="b2098-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="b2098-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b2098-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="b2098-104">本主題會描述什麼是 ASP.NET Web API 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="b2098-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="b2098-105">下載</span><span class="sxs-lookup"><span data-stu-id="b2098-105">Download</span></span>](#download)
- [<span data-ttu-id="b2098-106">文件</span><span class="sxs-lookup"><span data-stu-id="b2098-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="b2098-107">ASP.NET Web API 2.1 中的新功能</span><span class="sxs-lookup"><span data-stu-id="b2098-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="b2098-108">全域錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b2098-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="b2098-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="b2098-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="b2098-110">說明頁面改善</span><span class="sxs-lookup"><span data-stu-id="b2098-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="b2098-111">IgnoreRoute 支援</span><span class="sxs-lookup"><span data-stu-id="b2098-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="b2098-112">BSON 的媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="b2098-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="b2098-113">非同步篩選條件有更好的支援</span><span class="sxs-lookup"><span data-stu-id="b2098-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="b2098-114">查詢剖析為格式化的媒體櫃用戶端</span><span class="sxs-lookup"><span data-stu-id="b2098-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="b2098-115">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="b2098-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="b2098-116">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="b2098-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="b2098-117">下載</span><span class="sxs-lookup"><span data-stu-id="b2098-117">Download</span></span>

<span data-ttu-id="b2098-118">執行階段功能會以 NuGet 套件，NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="b2098-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="b2098-119">所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="b2098-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="b2098-120">最新的 ASP.NET Web API 2.1 RTM 套件有下列版本:"5.1.2"。</span><span class="sxs-lookup"><span data-stu-id="b2098-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="b2098-121">您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。</span><span class="sxs-lookup"><span data-stu-id="b2098-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="b2098-122">此版也包含在 NuGet 上的對應當地語系化的套件。</span><span class="sxs-lookup"><span data-stu-id="b2098-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="b2098-123">您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="b2098-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="b2098-124">文件</span><span class="sxs-lookup"><span data-stu-id="b2098-124">Documentation</span></span>

<span data-ttu-id="b2098-125">教學課程和 ASP.NET Web API 2.1 RTM 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="b2098-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="b2098-126">ASP.NET Web API 2.1 中的新功能</span><span class="sxs-lookup"><span data-stu-id="b2098-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="b2098-127">全域錯誤處理</span><span class="sxs-lookup"><span data-stu-id="b2098-127">Global Error Handling</span></span>

<span data-ttu-id="b2098-128">現在可以透過一個中央的機制，記錄所有未處理的例外狀況，並可自訂的未處理例外狀況的行為。</span><span class="sxs-lookup"><span data-stu-id="b2098-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="b2098-129">架構支援多個例外狀況記錄器，其所有的未處理的例外狀況和其發生的例如當時正在處理的要求內容的相關資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="b2098-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="b2098-130">例如，下列程式碼會使用 System.Diagnostics.TraceSource 記錄所有未處理的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="b2098-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="b2098-131">您也可以取代預設的例外狀況處理常式，以便您可以完全自訂的未處理的例外狀況時傳送的 HTTP 回應訊息就會發生。</span><span class="sxs-lookup"><span data-stu-id="b2098-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="b2098-132">我們提供了[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)會記錄所有未處理的例外狀況，透過受歡迎的 ELMAH 架構。</span><span class="sxs-lookup"><span data-stu-id="b2098-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="b2098-133">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="b2098-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="b2098-134">屬性路由現在支援條件約束，啟用版本控制和標頭為基礎的路由選取。</span><span class="sxs-lookup"><span data-stu-id="b2098-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="b2098-135">此外，已可透過自訂屬性路由的許多層面**IDirectRouteFactory**介面並**RouteFactoryAttribute**類別。</span><span class="sxs-lookup"><span data-stu-id="b2098-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="b2098-136">路由前置字元現在是透過可延伸**IRoutePrefix**介面並**RoutePrefixAttribute**類別。</span><span class="sxs-lookup"><span data-stu-id="b2098-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="b2098-137">我們提供了[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)來動態篩選控制器，' api 版本 ' HTTP 標頭所使用的條件約束。</span><span class="sxs-lookup"><span data-stu-id="b2098-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="b2098-138">說明頁面改善</span><span class="sxs-lookup"><span data-stu-id="b2098-138">Help Page Improvements</span></span>

<span data-ttu-id="b2098-139">Web API 2.1 包含下列增強功能[API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="b2098-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="b2098-140">文件的個別屬性的參數或傳回類型的動作。</span><span class="sxs-lookup"><span data-stu-id="b2098-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="b2098-141">文件的資料模型註釋。</span><span class="sxs-lookup"><span data-stu-id="b2098-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="b2098-142">[說明] 頁的 UI 設計也已更新，成配合這些變更。</span><span class="sxs-lookup"><span data-stu-id="b2098-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="b2098-143">IgnoreRoute 支援</span><span class="sxs-lookup"><span data-stu-id="b2098-143">IgnoreRoute Support</span></span>

<span data-ttu-id="b2098-144">Web API 2.1 支援略過在一組透過 Web API 路由的 URL 模式**IgnoreRoute**擴充方法**HttpRouteCollection**。</span><span class="sxs-lookup"><span data-stu-id="b2098-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="b2098-145">這些方法會導致 Web API，可忽略符合指定的範本，任何 Url，並允許將額外的處理視主機。</span><span class="sxs-lookup"><span data-stu-id="b2098-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="b2098-146">下列範例會略過開頭的 Uri&quot;內容&quot;區段：</span><span class="sxs-lookup"><span data-stu-id="b2098-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="b2098-147">BSON 的媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="b2098-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="b2098-148">Web API 現在支援[BSON](http://bsonspec.org/)電傳格式，在用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="b2098-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="b2098-149">若要在伺服器端啟用 BSON，新增**BsonMediaTypeFormatter**格式器集合：</span><span class="sxs-lookup"><span data-stu-id="b2098-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="b2098-150">以下是.NET 用戶端如何使用 BSON 格式：</span><span class="sxs-lookup"><span data-stu-id="b2098-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="b2098-151">我們提供了[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)示範用戶端和伺服器端。</span><span class="sxs-lookup"><span data-stu-id="b2098-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="b2098-152">如需詳細資訊，請參閱[Web API 2.1 中的 BSON 支援](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="b2098-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="b2098-153">非同步篩選條件有更好的支援</span><span class="sxs-lookup"><span data-stu-id="b2098-153">Better Support for Async Filters</span></span>

<span data-ttu-id="b2098-154">Web API 現在支援簡單的方法，來建立篩選器，以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="b2098-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="b2098-155">這個功能非常實用是您的篩選器必須執行非同步動作，例如資料庫的存取。</span><span class="sxs-lookup"><span data-stu-id="b2098-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="b2098-156">以前，若要建立非同步篩選條件，您必須實作篩選條件介面，因為篩選器的基底類別只會公開同步的方法。</span><span class="sxs-lookup"><span data-stu-id="b2098-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="b2098-157">現在可以覆寫虛擬`On*Async`方法篩選條件的基底類別。</span><span class="sxs-lookup"><span data-stu-id="b2098-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="b2098-158">例如: </span><span class="sxs-lookup"><span data-stu-id="b2098-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="b2098-159">**AuthorizationFilterAttribute**， **Actionfilterattribut**，並**ExceptionFilterAttribute**類別全都支援 Web API 2.1 中的非同步。</span><span class="sxs-lookup"><span data-stu-id="b2098-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="b2098-160">查詢剖析為格式化的媒體櫃用戶端</span><span class="sxs-lookup"><span data-stu-id="b2098-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="b2098-161">先前**System.Net.Http.Formatting**支援剖析及更新的伺服器端程式碼中，URI 查詢，但對應的可攜式程式庫缺少這項功能。</span><span class="sxs-lookup"><span data-stu-id="b2098-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="b2098-162">在 Web API 2.1 中，用戶端應用程式現在可以輕鬆剖析並更新查詢字串。</span><span class="sxs-lookup"><span data-stu-id="b2098-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="b2098-163">下列範例示範如何剖析、 修改和產生 URI 查詢。</span><span class="sxs-lookup"><span data-stu-id="b2098-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="b2098-164">（範例顯示簡單的主控台應用程式）。</span><span class="sxs-lookup"><span data-stu-id="b2098-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="b2098-165">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="b2098-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="b2098-166">本章節描述的已知的問題和 ASP.NET Web API 2.1 RTM 中的重大變更。</span><span class="sxs-lookup"><span data-stu-id="b2098-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="b2098-167">屬性路由</span><span class="sxs-lookup"><span data-stu-id="b2098-167">Attribute Routing</span></span>

<span data-ttu-id="b2098-168">模稜兩可的屬性路由的相符項目現在會報告錯誤，而不是選擇第一個相符項目。</span><span class="sxs-lookup"><span data-stu-id="b2098-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="b2098-169">屬性路由會嚴禁 *{controller}* 參數，透過 *{action}* 路由參數放在動作上。</span><span class="sxs-lookup"><span data-stu-id="b2098-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="b2098-170">這些參數，很可能會導致模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="b2098-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="b2098-171">到 5.1 封裝在中使用結果 5.0 封裝不存在專案中之專案的 scaffolding MVC/Web API</span><span class="sxs-lookup"><span data-stu-id="b2098-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="b2098-172">ASP.NET Web API 2.1 rtm 更新 NuGet 套件不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="b2098-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="b2098-173">他們使用舊版 ASP.NET 執行階段套件 (version=5.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="b2098-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="b2098-174">如此一來，ASP.NET scaffolding 會安裝必要的套件，先前的版本 (version=5.0.0.0)，如果它們還不在您的專案中。</span><span class="sxs-lookup"><span data-stu-id="b2098-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="b2098-175">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET 樣板不能覆寫您在專案中最新的套件。</span><span class="sxs-lookup"><span data-stu-id="b2098-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="b2098-176">如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新封裝之後，請確定 Web API 和 MVC 的版本不一致。</span><span class="sxs-lookup"><span data-stu-id="b2098-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="b2098-177">型別重新命名</span><span class="sxs-lookup"><span data-stu-id="b2098-177">Type Renames</span></span>

<span data-ttu-id="b2098-178">一些用於屬性路由的擴充性的類型已重新命名從 RC 為 2.1 RTM。</span><span class="sxs-lookup"><span data-stu-id="b2098-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="b2098-179">舊的型別名稱 (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="b2098-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="b2098-180">新的型別名稱 (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="b2098-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="b2098-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="b2098-181">IDirectRouteProvider</span></span> | <span data-ttu-id="b2098-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="b2098-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="b2098-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="b2098-183">RouteProviderAttribute</span></span> | <span data-ttu-id="b2098-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="b2098-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="b2098-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="b2098-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="b2098-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="b2098-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="b2098-187">例外狀況篩選條件不解除包裝彙總中非同步動作擲回的例外狀況</span><span class="sxs-lookup"><span data-stu-id="b2098-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="b2098-188">先前，如果非同步動作擲回**AggregateException**，例外狀況篩選條件會解除包裝為例外狀況，並**OnException**會取得基底例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b2098-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="b2098-189">在 2.1 中，例外狀況篩選條件不會不解除包裝，並**OnException**取得原始**AggregateException**。</span><span class="sxs-lookup"><span data-stu-id="b2098-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="b2098-190">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="b2098-190">Bug Fixes</span></span>

<span data-ttu-id="b2098-191">此版本也包含數個 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="b2098-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="b2098-192">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="b2098-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="b2098-193">5.1.0 套件</span><span class="sxs-lookup"><span data-stu-id="b2098-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="b2098-194">5.1.1 封裝</span><span class="sxs-lookup"><span data-stu-id="b2098-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="b2098-195">5.1.2 封裝包含 IntelliSense 的更新，但任何錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="b2098-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
