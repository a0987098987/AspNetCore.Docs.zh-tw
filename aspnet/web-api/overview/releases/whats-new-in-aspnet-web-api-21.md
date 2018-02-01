---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "ASP.NET Web API 2.1 中最新消息 |Microsoft 文件"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="42440-102">ASP.NET Web API 2.1 中最新消息</span><span class="sxs-lookup"><span data-stu-id="42440-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="42440-103">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="42440-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="42440-104">本主題說明 ASP.NET Web API 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="42440-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="42440-105">下載</span><span class="sxs-lookup"><span data-stu-id="42440-105">Download</span></span>](#download)
- [<span data-ttu-id="42440-106">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="42440-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="42440-107">ASP.NET Web API 2.1 的新功能</span><span class="sxs-lookup"><span data-stu-id="42440-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="42440-108">全域錯誤處理</span><span class="sxs-lookup"><span data-stu-id="42440-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="42440-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="42440-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="42440-110">說明頁面增強功能</span><span class="sxs-lookup"><span data-stu-id="42440-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="42440-111">IgnoreRoute 支援</span><span class="sxs-lookup"><span data-stu-id="42440-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="42440-112">BSON 的媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="42440-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="42440-113">非同步篩選器更佳的支援</span><span class="sxs-lookup"><span data-stu-id="42440-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="42440-114">格式化文件庫的用戶端所剖析的查詢</span><span class="sxs-lookup"><span data-stu-id="42440-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="42440-115">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="42440-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="42440-116">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="42440-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="42440-117">下載</span><span class="sxs-lookup"><span data-stu-id="42440-117">Download</span></span>

<span data-ttu-id="42440-118">執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="42440-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="42440-119">所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="42440-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="42440-120">最新的 ASP.NET Web API 2.1 RTM 封裝有下列版本:"5.1.2"。</span><span class="sxs-lookup"><span data-stu-id="42440-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="42440-121">您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。</span><span class="sxs-lookup"><span data-stu-id="42440-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="42440-122">此版也包含對應當地語系化的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="42440-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="42440-123">您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="42440-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="42440-124">文件</span><span class="sxs-lookup"><span data-stu-id="42440-124">Documentation</span></span>

<span data-ttu-id="42440-125">教學課程和 ASP.NET Web API 2.1 RTM 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="42440-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="42440-126">ASP.NET Web API 2.1 的新功能</span><span class="sxs-lookup"><span data-stu-id="42440-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="42440-127">全域錯誤處理</span><span class="sxs-lookup"><span data-stu-id="42440-127">Global Error Handling</span></span>

<span data-ttu-id="42440-128">所有未處理的例外狀況現在會記錄透過一個中央的機制，並可自訂的未處理例外狀況行為。</span><span class="sxs-lookup"><span data-stu-id="42440-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="42440-129">架構支援多個例外狀況記錄器，其所有未處理的例外狀況和發生，例如時正在處理的要求內容的相關資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="42440-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="42440-130">例如，下列程式碼會使用 System.Diagnostics.TraceSource 記錄所有未處理的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="42440-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="42440-131">您也可以取代預設的例外狀況處理常式，可讓您完全可以自訂處理的例外狀況時，會傳送 HTTP 回應訊息，就會發生。</span><span class="sxs-lookup"><span data-stu-id="42440-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="42440-132">我們已提供[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt)記錄透過受歡迎的 ELMAH framework 的所有未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="42440-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="42440-133">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="42440-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="42440-134">屬性現在路由支援條件約束，啟用版本控制和標頭為基礎的路由選取。</span><span class="sxs-lookup"><span data-stu-id="42440-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="42440-135">此外，已可透過自訂屬性路由的許多層面**IDirectRouteFactory**介面和**RouteFactoryAttribute**類別。</span><span class="sxs-lookup"><span data-stu-id="42440-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="42440-136">路由前置字元現在是可透過延伸**IRoutePrefix**介面和**RoutePrefixAttribute**類別。</span><span class="sxs-lookup"><span data-stu-id="42440-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="42440-137">我們已提供[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt)來動態篩選控制站 'api-version' HTTP 標頭所使用的條件約束。</span><span class="sxs-lookup"><span data-stu-id="42440-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="42440-138">說明頁面增強功能</span><span class="sxs-lookup"><span data-stu-id="42440-138">Help Page Improvements</span></span>

<span data-ttu-id="42440-139">Web API 2.1 包含下列增強功能[API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="42440-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="42440-140">文件的個別屬性的參數或傳回類型的動作。</span><span class="sxs-lookup"><span data-stu-id="42440-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="42440-141">文件集的資料模型的註解。</span><span class="sxs-lookup"><span data-stu-id="42440-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="42440-142">說明網頁的 UI 設計也已更新，以容納這些變更。</span><span class="sxs-lookup"><span data-stu-id="42440-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="42440-143">IgnoreRoute 支援</span><span class="sxs-lookup"><span data-stu-id="42440-143">IgnoreRoute Support</span></span>

<span data-ttu-id="42440-144">Web 應用程式開發介面 2.1 支援忽略中透過一組 Web API 路由的 URL 模式**IgnoreRoute**的擴充方法**HttpRouteCollection**。</span><span class="sxs-lookup"><span data-stu-id="42440-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="42440-145">這些方法會導致 Web API，略過任何符合指定的範本的 Url，而且允許套用額外的處理視主機。</span><span class="sxs-lookup"><span data-stu-id="42440-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="42440-146">下列範例會忽略開頭的 Uri&quot;內容&quot;區段：</span><span class="sxs-lookup"><span data-stu-id="42440-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="42440-147">BSON 的媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="42440-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="42440-148">Web API 現在支援[BSON](http://bsonspec.org/) wire 格式，在用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="42440-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="42440-149">將加入伺服器端上，讓 BSON **BsonMediaTypeFormatter**格式器集合：</span><span class="sxs-lookup"><span data-stu-id="42440-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="42440-150">以下是如何在.NET 用戶端可以取用 BSON 格式：</span><span class="sxs-lookup"><span data-stu-id="42440-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="42440-151">我們已提供[範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt)，它會顯示用戶端和伺服器端。</span><span class="sxs-lookup"><span data-stu-id="42440-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="42440-152">如需詳細資訊，請參閱[BSON 支援 Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="42440-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="42440-153">非同步篩選器更佳的支援</span><span class="sxs-lookup"><span data-stu-id="42440-153">Better Support for Async Filters</span></span>

<span data-ttu-id="42440-154">Web API 現在支援用來建立篩選器，以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="42440-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="42440-155">此功能十分有用是您的篩選條件必須執行非同步動作，例如資料庫的存取。</span><span class="sxs-lookup"><span data-stu-id="42440-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="42440-156">以前，若要建立非同步篩選器，您必須實作篩選介面自行，因為篩選器的基底類別只會公開同步方法。</span><span class="sxs-lookup"><span data-stu-id="42440-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="42440-157">現在可以覆寫虛擬`On*Async`方法篩選器的基底類別。</span><span class="sxs-lookup"><span data-stu-id="42440-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="42440-158">例如: </span><span class="sxs-lookup"><span data-stu-id="42440-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="42440-159">**AuthorizationFilterAttribute**， **Actionfilterattribut**，和**ExceptionFilterAttribute**類別所有 Web 應用程式開發介面 2.1 中來都支援非同步。</span><span class="sxs-lookup"><span data-stu-id="42440-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="42440-160">格式化文件庫的用戶端所剖析的查詢</span><span class="sxs-lookup"><span data-stu-id="42440-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="42440-161">先前， **System.Net.Http.Formatting**支援剖析及更新的伺服器端程式碼中，URI 查詢，但對等的可攜式程式庫遺失此功能。</span><span class="sxs-lookup"><span data-stu-id="42440-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="42440-162">在 Web 應用程式開發介面 2.1 中，用戶端應用程式現在可以輕鬆地剖析，並更新查詢字串。</span><span class="sxs-lookup"><span data-stu-id="42440-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="42440-163">下列範例顯示如何剖析、 修改及產生 URI 查詢。</span><span class="sxs-lookup"><span data-stu-id="42440-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="42440-164">（範例將說明簡單的主控台應用程式）。</span><span class="sxs-lookup"><span data-stu-id="42440-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="42440-165">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="42440-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="42440-166">本章節描述已知的問題和 ASP.NET Web API 2.1 RTM 中的重大變更。</span><span class="sxs-lookup"><span data-stu-id="42440-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="42440-167">路由屬性</span><span class="sxs-lookup"><span data-stu-id="42440-167">Attribute Routing</span></span>

<span data-ttu-id="42440-168">屬性路由相符項目中的模稜兩可現在會報告錯誤，而不是選擇第一個相符項目。</span><span class="sxs-lookup"><span data-stu-id="42440-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="42440-169">屬性路由嚴禁*{控制器}*參數，並從使用*{action}*路由參數放在 [動作]。</span><span class="sxs-lookup"><span data-stu-id="42440-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="42440-170">這些參數會非常有可能會導致模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="42440-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="42440-171">Scaffolding MVC/Web 應用程式開發介面到 5.1 封裝在中使用結果 5.0 封裝還不存在專案中的專案</span><span class="sxs-lookup"><span data-stu-id="42440-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="42440-172">ASP.NET Web API 2.1 rtm 更新 NuGet 封裝不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="42440-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="42440-173">他們使用的舊版 ASP.NET 執行階段封裝 (5.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="42440-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="42440-174">如此一來，ASP.NET scaffolding 會安裝必要的封裝前, 一版 (5.0.0.0)，如果還沒有出現在您的專案。</span><span class="sxs-lookup"><span data-stu-id="42440-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="42440-175">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。</span><span class="sxs-lookup"><span data-stu-id="42440-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="42440-176">如果您使用 ASP.NET scaffolding Web API 2.1 或 ASP.NET MVC 5.1 更新封裝之後，請確定 Web API 和 MVC 的版本不一致。</span><span class="sxs-lookup"><span data-stu-id="42440-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="42440-177">型別重新命名</span><span class="sxs-lookup"><span data-stu-id="42440-177">Type Renames</span></span>

<span data-ttu-id="42440-178">某些屬性的路由擴充性所使用的類型已重新命名從 RC 2.1 RTM。</span><span class="sxs-lookup"><span data-stu-id="42440-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="42440-179">舊的型別名稱 (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="42440-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="42440-180">新的類型名稱 (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="42440-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="42440-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="42440-181">IDirectRouteProvider</span></span> | <span data-ttu-id="42440-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="42440-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="42440-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="42440-183">RouteProviderAttribute</span></span> | <span data-ttu-id="42440-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="42440-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="42440-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="42440-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="42440-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="42440-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="42440-187">例外狀況篩選條件不解除包裝在非同步動作中擲回的例外狀況彙總</span><span class="sxs-lookup"><span data-stu-id="42440-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="42440-188">過去，如果擲回的非同步動作**AggregateException**，例外狀況篩選條件會解除包裝的例外狀況和**OnException**得到基底例外狀況。</span><span class="sxs-lookup"><span data-stu-id="42440-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="42440-189">在 2.1 中，例外狀況篩選條件不會不解除包裝，和**OnException**取得原始**AggregateException**。</span><span class="sxs-lookup"><span data-stu-id="42440-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="42440-190">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="42440-190">Bug Fixes</span></span>

<span data-ttu-id="42440-191">此版本也包含數個 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="42440-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="42440-192">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="42440-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="42440-193">5.1.0 封裝</span><span class="sxs-lookup"><span data-stu-id="42440-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="42440-194">5.1.1 封裝</span><span class="sxs-lookup"><span data-stu-id="42440-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="42440-195">5.1.2 封裝包含 IntelliSense 的更新，但沒有 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="42440-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
