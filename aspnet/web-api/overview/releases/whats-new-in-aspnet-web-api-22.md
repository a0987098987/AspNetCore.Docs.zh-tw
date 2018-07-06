---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 中最新消息 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 0f0794988da712897092ab808a08fca5eeebb6d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813274"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="9b07e-102">ASP.NET Web API 2.2 中最新消息</span><span class="sxs-lookup"><span data-stu-id="9b07e-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="9b07e-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9b07e-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="9b07e-104">本主題會描述什麼是 ASP.NET Web API 2.2 的新功能。</span><span class="sxs-lookup"><span data-stu-id="9b07e-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="9b07e-105">下載</span><span class="sxs-lookup"><span data-stu-id="9b07e-105">Download</span></span>](#download)
- [<span data-ttu-id="9b07e-106">文件</span><span class="sxs-lookup"><span data-stu-id="9b07e-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="9b07e-107">ASP.NET Web API 2.2 中的新功能</span><span class="sxs-lookup"><span data-stu-id="9b07e-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="9b07e-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="9b07e-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="9b07e-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="9b07e-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="9b07e-110">Web API 用戶端支援 Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="9b07e-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="9b07e-111">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="9b07e-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="9b07e-112">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="9b07e-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="9b07e-113">Microsoft.AspNet.OData 5.2.1 章</span><span class="sxs-lookup"><span data-stu-id="9b07e-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="9b07e-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="9b07e-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="9b07e-115">Microsoft.AspNet.WebAPI 5.2.3 beta 版</span><span class="sxs-lookup"><span data-stu-id="9b07e-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="9b07e-116">下載</span><span class="sxs-lookup"><span data-stu-id="9b07e-116">Download</span></span>

<span data-ttu-id="9b07e-117">執行階段功能會以 NuGet 套件，NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="9b07e-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="9b07e-118">所有執行階段套件遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="9b07e-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="9b07e-119">最新的 ASP.NET Web API 2.2 套件有下列版本:"5.2.0 」。</span><span class="sxs-lookup"><span data-stu-id="9b07e-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="9b07e-120">您可以安裝或更新這些套件，透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。</span><span class="sxs-lookup"><span data-stu-id="9b07e-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="9b07e-121">此版也包含在 NuGet 上的對應當地語系化的套件。</span><span class="sxs-lookup"><span data-stu-id="9b07e-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="9b07e-122">您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="9b07e-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="9b07e-123">文件</span><span class="sxs-lookup"><span data-stu-id="9b07e-123">Documentation</span></span>

<span data-ttu-id="9b07e-124">教學課程和 ASP.NET Web API 2.2 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="9b07e-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="9b07e-125">ASP.NET Web API 2.2 中的新功能</span><span class="sxs-lookup"><span data-stu-id="9b07e-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="9b07e-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="9b07e-126">OData v4</span></span>

<span data-ttu-id="9b07e-127">此版本新增支援 OData v4 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="9b07e-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="9b07e-128">如需詳細資訊，請參閱[Web API OData v4 文件。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="9b07e-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="9b07e-129">以下是一些主要功能與 OData v4 的變更：</span><span class="sxs-lookup"><span data-stu-id="9b07e-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="9b07e-130">支援 OData 模型中的別名屬性</span><span class="sxs-lookup"><span data-stu-id="9b07e-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="9b07e-131">ComplexTypeAttribute、 AssociationAttribute、 TimesTampAttribute 和 ConcurrencyCheckAttribute ODataConventionModelBuilder 中支援</span><span class="sxs-lookup"><span data-stu-id="9b07e-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="9b07e-132">提供能夠提供動作的易記標題</span><span class="sxs-lookup"><span data-stu-id="9b07e-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="9b07e-133">ODL UriParser 與整合</span><span class="sxs-lookup"><span data-stu-id="9b07e-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="9b07e-134">支援[enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)，[內含項目](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)和[單一](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="9b07e-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="9b07e-135">支援基本類型的轉換</span><span class="sxs-lookup"><span data-stu-id="9b07e-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="9b07e-136">已新增的 OData 函式支援</span><span class="sxs-lookup"><span data-stu-id="9b07e-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="9b07e-137">函式呼叫的支援參數別名</span><span class="sxs-lookup"><span data-stu-id="9b07e-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="9b07e-138">在模型中支援 camel 命名法大小寫的命名慣例</span><span class="sxs-lookup"><span data-stu-id="9b07e-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="9b07e-139">支援在 $filter cast （）</span><span class="sxs-lookup"><span data-stu-id="9b07e-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="9b07e-140">開啟的複雜類型的支援</span><span class="sxs-lookup"><span data-stu-id="9b07e-140">Support for open complex type</span></span>
- <span data-ttu-id="9b07e-141">已移除的 EntitySetController 和 AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="9b07e-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="9b07e-142">變更至 $ref $link</span><span class="sxs-lookup"><span data-stu-id="9b07e-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="9b07e-143">已新增的屬性路由支援</span><span class="sxs-lookup"><span data-stu-id="9b07e-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="9b07e-144">使用 OData 核心程式庫 6.4.0</span><span class="sxs-lookup"><span data-stu-id="9b07e-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="9b07e-145">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="9b07e-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="9b07e-146">屬性路由現在提供稱為 IDirectRouteProvider，可讓您完整控制如何屬性路由會探索及設定的擴充點。</span><span class="sxs-lookup"><span data-stu-id="9b07e-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="9b07e-147">IDirectRouteProvider 負責提供一份動作與控制器相關聯的路由資訊，來指定這些動作只需要何種路由的設定。</span><span class="sxs-lookup"><span data-stu-id="9b07e-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="9b07e-148">呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能會指定 IDirectRouteProvider 實作。</span><span class="sxs-lookup"><span data-stu-id="9b07e-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="9b07e-149">自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作，DefaultDirectRouteProvider 延伸。</span><span class="sxs-lookup"><span data-stu-id="9b07e-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="9b07e-150">這個類別會提供不同可覆寫的虛擬方法，以變更邏輯探索屬性、 建立路由項目，以及探索路由前置詞和區域前置詞。</span><span class="sxs-lookup"><span data-stu-id="9b07e-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="9b07e-151">以下是一些您可以執行與此新的擴充性點的範例：</span><span class="sxs-lookup"><span data-stu-id="9b07e-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="9b07e-152">支援的路由屬性的繼承</span><span class="sxs-lookup"><span data-stu-id="9b07e-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="9b07e-153">範例：</span><span class="sxs-lookup"><span data-stu-id="9b07e-153">Example:</span></span>

    <span data-ttu-id="9b07e-154">這裡 like"/ 10/api/值 」 的要求會成功會傳回 「 成功： 10"</span><span class="sxs-lookup"><span data-stu-id="9b07e-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="9b07e-155">提供屬性路由的預設路由名稱，依照您喜歡某些慣例。</span><span class="sxs-lookup"><span data-stu-id="9b07e-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="9b07e-156">根據預設，屬性路由不會自動建立屬性路由的名稱。</span><span class="sxs-lookup"><span data-stu-id="9b07e-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="9b07e-157">它們最後會在路由表之前，請修改屬性路由的路由範本，在一個集中位置。</span><span class="sxs-lookup"><span data-stu-id="9b07e-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="9b07e-158">適用於 Windows Phone 8.1 的 web API 用戶端支援</span><span class="sxs-lookup"><span data-stu-id="9b07e-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="9b07e-159">您現在可以使用 Web API 用戶端 NuGet 套件來實作您的 Web API 用戶端邏輯，以 Windows Phone 8.1 為目標時，或從通用應用程式中。</span><span class="sxs-lookup"><span data-stu-id="9b07e-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="9b07e-160">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="9b07e-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="9b07e-161">本章節描述的已知的問題和 ASP.NET Web API 2.2 中的重大變更。</span><span class="sxs-lookup"><span data-stu-id="9b07e-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="9b07e-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="9b07e-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="9b07e-163">模型產生器</span><span class="sxs-lookup"><span data-stu-id="9b07e-163">Model builder</span></span>

<span data-ttu-id="9b07e-164">問題： 多載函式可能不會公開成 FunctionImport</span><span class="sxs-lookup"><span data-stu-id="9b07e-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="9b07e-165">如果有 2 個多載函式，但它們也 FunctionImport 然後要求 System.InvalidOperationException ~/GetAllConventionCustomers(CustomerName={customerName}) 結果如下所示。</span><span class="sxs-lookup"><span data-stu-id="9b07e-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="9b07e-166">因應措施： 此問題的因應措施是將這兩個函式多載新增 FunctionImports 為。</span><span class="sxs-lookup"><span data-stu-id="9b07e-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="9b07e-167">OData 路由</span><span class="sxs-lookup"><span data-stu-id="9b07e-167">OData Routing</span></span>

<span data-ttu-id="9b07e-168">字串常值，包含 URL 編碼 (%2f)、 斜線和 backslash(%5C) 用於 OData 資源路徑時，會造成 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="9b07e-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="9b07e-169">比方說，字串常值都可以用於 OData 資源路徑做為函式參數或索引鍵值的實體集。</span><span class="sxs-lookup"><span data-stu-id="9b07e-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="9b07e-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="9b07e-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="9b07e-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="9b07e-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="9b07e-172">當服務收到這類要求主機將會取消逸出的逸出序列，然後將它們傳遞至 Web API 執行階段。</span><span class="sxs-lookup"><span data-stu-id="9b07e-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="9b07e-173">這樣可防止攻擊，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9b07e-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="9b07e-174">這會導致 Web API OData 堆疊傳回 404 （找不到） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="9b07e-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="9b07e-175">若要避免這個錯誤，您的用戶端應使用斜線 （%252f) 的雙重逸出序列和反斜線 （%255 C)。</span><span class="sxs-lookup"><span data-stu-id="9b07e-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="9b07e-176">這不會針對查詢字串，例如 /Employees？ $filter = 名稱 eq '名稱 %2f'</span><span class="sxs-lookup"><span data-stu-id="9b07e-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="9b07e-177">**請注意未逸出的斜線 （'/') 和反斜線 （"） 是不合法的 OData 資源路徑的字串常值中。斜線應該只做為路徑分隔符號和反斜線不應該完全出現在 OData 資源路徑。（兩者都是可在某部份的 OData 查詢字串中使用。）**</span><span class="sxs-lookup"><span data-stu-id="9b07e-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="9b07e-178">因應措施： 您無法覆寫來逸出斜線和字串常值中的反斜線，實際上剖析之前 DefaultODataPathHandler 的剖析方法。</span><span class="sxs-lookup"><span data-stu-id="9b07e-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="9b07e-179">您可以找到這種方法的範例。</span><span class="sxs-lookup"><span data-stu-id="9b07e-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="9b07e-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="9b07e-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="9b07e-181">[查詢]</span><span class="sxs-lookup"><span data-stu-id="9b07e-181">[Queryable]</span></span>

<span data-ttu-id="9b07e-182">[Queryable] 屬性已被取代。</span><span class="sxs-lookup"><span data-stu-id="9b07e-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="9b07e-183">新的 OData v3 應用程式應該改用**System.Web.Http.OData.EnableQueryAttribute**。</span><span class="sxs-lookup"><span data-stu-id="9b07e-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="9b07e-184">**ODataHttpConfigurationExtensions.EnableQuerySupport**擴充方法現在會加入**EnableQueryAttribute**至全域篩選集合。</span><span class="sxs-lookup"><span data-stu-id="9b07e-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="9b07e-185">如果有任何控制站 **[Queryable]** 屬性，呼叫`config.EnableQuerySupport()`會導致 **[Queryable]** 屬性失敗</span><span class="sxs-lookup"><span data-stu-id="9b07e-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="9b07e-186">若要解決此問題的建議的方式是取代的所有執行個體**QueryableAttribute**具有**System.Web.Http.OData.EnableQueryAttribute**。</span><span class="sxs-lookup"><span data-stu-id="9b07e-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="9b07e-187">一種解決方法是在您的 Web API 設定中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9b07e-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="9b07e-188">屬性路由</span><span class="sxs-lookup"><span data-stu-id="9b07e-188">Attribute Routing</span></span>

<span data-ttu-id="9b07e-189">問題： FromUri 屬性裝飾的複雜型別的模型繫結時的行為以不同的方式使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="9b07e-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="9b07e-190">下列連結正在追蹤此問題，而且也有因應措施的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="9b07e-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="9b07e-191">問題： Scaffolding MVC/Web API，到使用 5.2.0 專案不存在專案中的封裝的封裝導致 5.1.2</span><span class="sxs-lookup"><span data-stu-id="9b07e-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="9b07e-192">適用於 ASP.NET MVC 5.2 更新 NuGet 套件不會更新 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="9b07e-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="9b07e-193">他們使用舊版 ASP.NET 執行階段套件 (例如在 Update 2 的 5.1.2)。</span><span class="sxs-lookup"><span data-stu-id="9b07e-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="9b07e-194">如此一來，ASP.NET scaffolding 會安裝必要的套件，先前的版本 (例如在 Update 2 的 5.1.2)，如果它們還不在您的專案中。</span><span class="sxs-lookup"><span data-stu-id="9b07e-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="9b07e-195">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET 樣板不能覆寫您在專案中最新的套件。</span><span class="sxs-lookup"><span data-stu-id="9b07e-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="9b07e-196">如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新您的專案的封裝之後，請確定 Web API 和 ASP.NET MVC 的版本不一致。</span><span class="sxs-lookup"><span data-stu-id="9b07e-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="9b07e-197">Bug 修正和次要功能更新</span><span class="sxs-lookup"><span data-stu-id="9b07e-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="9b07e-198">此版本也包含數個 bug 修正和次要功能更新。</span><span class="sxs-lookup"><span data-stu-id="9b07e-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="9b07e-199">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="9b07e-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="9b07e-200">5.2 封裝</span><span class="sxs-lookup"><span data-stu-id="9b07e-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="9b07e-201">Microsoft.AspNet.OData 5.2.1 章</span><span class="sxs-lookup"><span data-stu-id="9b07e-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="9b07e-202">Microsoft.AspNet.OData 5.2.1 章套件中包含 NuGet 相依性更新，但任何錯誤修正。</span><span class="sxs-lookup"><span data-stu-id="9b07e-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="9b07e-203">透過這項更新，不會再嚴格的相依性上有 Microsoft.OData.Core 6.4.0，但是其中一個可以升級至 6.4.0 和 7.0.0 之間的任何版本。</span><span class="sxs-lookup"><span data-stu-id="9b07e-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="9b07e-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="9b07e-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="9b07e-205">在此版本中，我們已進行變更的相依性`Json.Net 6.0.4`。</span><span class="sxs-lookup"><span data-stu-id="9b07e-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="9b07e-206">如需有關此版本的最新消息`Json.NET`，請參閱 < [Json.NET 6.0 第 4 版-JSON 合併、 相依性插入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="9b07e-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="9b07e-207">此版本沒有任何其他的新功能或 bug 修正 Web API 中。</span><span class="sxs-lookup"><span data-stu-id="9b07e-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="9b07e-208">我們隨後更新現存相依於這個新版的 Web API 的所有其他相依套件。</span><span class="sxs-lookup"><span data-stu-id="9b07e-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="9b07e-209">Microsoft.AspNet.WebAPI 5.2.3 beta 版</span><span class="sxs-lookup"><span data-stu-id="9b07e-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="9b07e-210">您可以閱讀發行[此處](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9b07e-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="9b07e-211">此版本包含只 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="9b07e-211">This release contains only bug fixes.</span></span> <span data-ttu-id="9b07e-212">您可以使用[這項查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。</span><span class="sxs-lookup"><span data-stu-id="9b07e-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
