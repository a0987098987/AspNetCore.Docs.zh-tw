---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: 新 ASP.NET Web API 2.2 功能 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="aec2b-102">ASP.NET Web API 2.2 中最新消息</span><span class="sxs-lookup"><span data-stu-id="aec2b-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="aec2b-103">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="aec2b-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="aec2b-104">本主題說明 ASP.NET Web API 2.2 的新功能。</span><span class="sxs-lookup"><span data-stu-id="aec2b-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="aec2b-105">下載</span><span class="sxs-lookup"><span data-stu-id="aec2b-105">Download</span></span>](#download)
- [<span data-ttu-id="aec2b-106">文件 (英文)</span><span class="sxs-lookup"><span data-stu-id="aec2b-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="aec2b-107">ASP.NET Web API 2.2 的新功能</span><span class="sxs-lookup"><span data-stu-id="aec2b-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="aec2b-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="aec2b-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="aec2b-109">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="aec2b-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="aec2b-110">適用於 Windows Phone 8.1 的 web API 的用戶端支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="aec2b-111">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="aec2b-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="aec2b-112">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="aec2b-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="aec2b-113">Microsoft.AspNet.OData 5.2.1 章</span><span class="sxs-lookup"><span data-stu-id="aec2b-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="aec2b-114">Microsoft.AspNet.WebAPI 5.2.2 章</span><span class="sxs-lookup"><span data-stu-id="aec2b-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="aec2b-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="aec2b-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="aec2b-116">下載</span><span class="sxs-lookup"><span data-stu-id="aec2b-116">Download</span></span>

<span data-ttu-id="aec2b-117">執行階段功能會以 NuGet 封裝在 NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="aec2b-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="aec2b-118">所有執行階段封裝遵循[語意版本設定](http://semver.org/)規格。</span><span class="sxs-lookup"><span data-stu-id="aec2b-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="aec2b-119">最新的 ASP.NET Web API 2.2 封裝有下列版本:"5.2.0"。</span><span class="sxs-lookup"><span data-stu-id="aec2b-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="aec2b-120">您可以安裝或更新這些套件透過[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)。</span><span class="sxs-lookup"><span data-stu-id="aec2b-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="aec2b-121">此版也包含對應當地語系化的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="aec2b-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="aec2b-122">您可以安裝或使用 NuGet 封裝管理員主控台更新到發行的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="aec2b-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="aec2b-123">文件</span><span class="sxs-lookup"><span data-stu-id="aec2b-123">Documentation</span></span>

<span data-ttu-id="aec2b-124">教學課程和 ASP.NET Web API 2.2 的其他資訊都是從 ASP.NET 網站 ([https://www.asp.net/web-api](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="aec2b-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="aec2b-125">ASP.NET Web API 2.2 的新功能</span><span class="sxs-lookup"><span data-stu-id="aec2b-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="aec2b-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="aec2b-126">OData v4</span></span>

<span data-ttu-id="aec2b-127">此版本中加入 OData v4 通訊協定的支援。</span><span class="sxs-lookup"><span data-stu-id="aec2b-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="aec2b-128">如需詳細資訊，請參閱[Web API OData v4 文件。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="aec2b-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="aec2b-129">以下是一些主要功能與 OData v4 的變更：</span><span class="sxs-lookup"><span data-stu-id="aec2b-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="aec2b-130">OData 模型中的別名屬性的支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="aec2b-131">ComplexTypeAttribute、 AssociationAttribute、 TimesTampAttribute 和 ConcurrencyCheckAttribute ODataConventionModelBuilder 中支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="aec2b-132">提供功能，提供易記的標題的動作</span><span class="sxs-lookup"><span data-stu-id="aec2b-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="aec2b-133">ODL UriParser 與整合</span><span class="sxs-lookup"><span data-stu-id="aec2b-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="aec2b-134">支援[列舉](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)，[內含項目](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)和[單一](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="aec2b-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="aec2b-135">支援的基本型別轉換</span><span class="sxs-lookup"><span data-stu-id="aec2b-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="aec2b-136">已新增的 OData 函數支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="aec2b-137">函式呼叫的支援參數別名</span><span class="sxs-lookup"><span data-stu-id="aec2b-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="aec2b-138">在模型中支援 camel 命名法的大小寫命名慣例</span><span class="sxs-lookup"><span data-stu-id="aec2b-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="aec2b-139">支援在 $filter cast （）</span><span class="sxs-lookup"><span data-stu-id="aec2b-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="aec2b-140">開啟的複雜類型的支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-140">Support for open complex type</span></span>
- <span data-ttu-id="aec2b-141">移除的 EntitySetController 和 AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="aec2b-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="aec2b-142">變更至 $ref $link</span><span class="sxs-lookup"><span data-stu-id="aec2b-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="aec2b-143">已新增的屬性路由支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="aec2b-144">使用 OData 核心程式庫 6.4.0</span><span class="sxs-lookup"><span data-stu-id="aec2b-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="aec2b-145">屬性路由的增強功能</span><span class="sxs-lookup"><span data-stu-id="aec2b-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="aec2b-146">屬性現在路由提供稱為 IDirectRouteProvider，可讓您探索及設定屬性路由的方式的完整控制權的擴充點。</span><span class="sxs-lookup"><span data-stu-id="aec2b-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="aec2b-147">IDirectRouteProvider 負責提供動作與控制器以及相關聯的路由資訊來指定完全何種路由組態所需的那些動作的清單。</span><span class="sxs-lookup"><span data-stu-id="aec2b-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="aec2b-148">呼叫 MapAttributes/MapHttpAttributeRoutes 時，可能指定 IDirectRouteProvider 實作。</span><span class="sxs-lookup"><span data-stu-id="aec2b-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="aec2b-149">自訂 IDirectRouteProvider 會最簡單方法是將我們的預設實作、 DefaultDirectRouteProvider 延伸。</span><span class="sxs-lookup"><span data-stu-id="aec2b-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="aec2b-150">這個類別會提供個別可覆寫的虛擬方法，若要變更探索屬性、 建立路由項目，以及探索路由前置字元和區域前置詞的邏輯。</span><span class="sxs-lookup"><span data-stu-id="aec2b-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="aec2b-151">以下是有關您可以執行與此新的擴充性點的部分範例：</span><span class="sxs-lookup"><span data-stu-id="aec2b-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="aec2b-152">支援路由屬性的繼承</span><span class="sxs-lookup"><span data-stu-id="aec2b-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="aec2b-153">範例：</span><span class="sxs-lookup"><span data-stu-id="aec2b-153">Example:</span></span>

    <span data-ttu-id="aec2b-154">這裡 like"/ api/值 10"的要求會成功傳回"成功： 10"</span><span class="sxs-lookup"><span data-stu-id="aec2b-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="aec2b-155">提供屬性路由的預設路由名稱遵循您喜歡的一些慣例。</span><span class="sxs-lookup"><span data-stu-id="aec2b-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="aec2b-156">根據預設，屬性路由不會自動建立屬性路由的名稱。</span><span class="sxs-lookup"><span data-stu-id="aec2b-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="aec2b-157">它們在路由表結束之前，請修改在同一個地方屬性路由的路由範本。</span><span class="sxs-lookup"><span data-stu-id="aec2b-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="aec2b-158">適用於 Windows Phone 8.1 的 web API 的用戶端支援</span><span class="sxs-lookup"><span data-stu-id="aec2b-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="aec2b-159">您現在可以使用 Web API 的用戶端 NuGet 封裝來實作您的 Web API 用戶端邏輯，以 Windows Phone 8.1 為目標時，或從通用應用程式中。</span><span class="sxs-lookup"><span data-stu-id="aec2b-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="aec2b-160">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="aec2b-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="aec2b-161">本章節描述的已知的問題和 ASP.NET Web API 2.2 中的突破性變更。</span><span class="sxs-lookup"><span data-stu-id="aec2b-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="aec2b-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="aec2b-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="aec2b-163">模型產生器</span><span class="sxs-lookup"><span data-stu-id="aec2b-163">Model builder</span></span>

<span data-ttu-id="aec2b-164">問題： 多載函式可能不會公開為 FunctionImport</span><span class="sxs-lookup"><span data-stu-id="aec2b-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="aec2b-165">如果有 2 多載函式，它們也是 FunctionImport 然後要求在 System.InvalidOperationException ~/GetAllConventionCustomers(CustomerName={customerName}) 結果如下所示。</span><span class="sxs-lookup"><span data-stu-id="aec2b-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="aec2b-166">因應措施： 此問題的因應措施是將這兩個函式多載為 FunctionImports。</span><span class="sxs-lookup"><span data-stu-id="aec2b-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="aec2b-167">OData 路由</span><span class="sxs-lookup"><span data-stu-id="aec2b-167">OData Routing</span></span>

<span data-ttu-id="aec2b-168">字串常值包含 URL 編碼斜線 (%2f)，而 backslash(%5C) 用於 OData 資源路徑時，會導致 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="aec2b-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="aec2b-169">例如，字串常值可以當做 OData 資源路徑中的函式的參數或實體集的索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="aec2b-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="aec2b-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="aec2b-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="aec2b-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="aec2b-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="aec2b-172">當服務收到這類要求主機將會取消逸出這些逸出序列，然後將它們傳遞至 Web API 執行階段。</span><span class="sxs-lookup"><span data-stu-id="aec2b-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="aec2b-173">這樣可防止攻擊，如下所示：</span><span class="sxs-lookup"><span data-stu-id="aec2b-173">This protects against attacks like the following:</span></span>  
  
 <span data-ttu-id="aec2b-174">http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:</span><span class="sxs-lookup"><span data-stu-id="aec2b-174">http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:</span></span>

<span data-ttu-id="aec2b-175">這會導致 Web API OData 堆疊傳回 404 錯誤 （找不到）。</span><span class="sxs-lookup"><span data-stu-id="aec2b-175">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="aec2b-176">若要避免這個錯誤，您的用戶端應該使用雙重逸出序列的斜線 （%252f) 和反斜線 （%255 C)。</span><span class="sxs-lookup"><span data-stu-id="aec2b-176">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="aec2b-177">這不會發生的查詢字串，例如 /Employees？ $filter = Name eq '名稱 %2f'</span><span class="sxs-lookup"><span data-stu-id="aec2b-177">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="aec2b-178">**請注意未逸出的斜線 （'/') 和反斜線 （"） 是不合法的 OData 資源路徑字串常值。斜線應只做為路徑分隔符號，反斜線不應該完全出現在 OData 資源路徑。（兩者都可用於 OData 查詢字串的某些部分。）**</span><span class="sxs-lookup"><span data-stu-id="aec2b-178">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="aec2b-179">因應措施： 您無法覆寫來逸出斜線和字串常值反斜線，實際上會剖析它們之前 DefaultODataPathHandler 的剖析方法。</span><span class="sxs-lookup"><span data-stu-id="aec2b-179">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="aec2b-180">您可以找到這種方法的範例。</span><span class="sxs-lookup"><span data-stu-id="aec2b-180">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="aec2b-181">OData v3</span><span class="sxs-lookup"><span data-stu-id="aec2b-181">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="aec2b-182">[查詢]</span><span class="sxs-lookup"><span data-stu-id="aec2b-182">[Queryable]</span></span>

<span data-ttu-id="aec2b-183">[Queryable] 屬性已被取代。</span><span class="sxs-lookup"><span data-stu-id="aec2b-183">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="aec2b-184">新的 OData v3 應用程式應該使用**System.Web.Http.OData.EnableQueryAttribute**。</span><span class="sxs-lookup"><span data-stu-id="aec2b-184">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="aec2b-185">**ODataHttpConfigurationExtensions.EnableQuerySupport**擴充方法現在將**EnableQueryAttribute**至全域篩選集合。</span><span class="sxs-lookup"><span data-stu-id="aec2b-185">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="aec2b-186">如果有任何控制站 **[Queryable]** 屬性，呼叫`config.EnableQuerySupport()`會導致 **[Queryable]** 屬性失敗</span><span class="sxs-lookup"><span data-stu-id="aec2b-186">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="aec2b-187">若要解決此問題的建議的方式是要取代的所有執行個體**根據 QueryableAttribute**與**System.Web.Http.OData.EnableQueryAttribute**。</span><span class="sxs-lookup"><span data-stu-id="aec2b-187">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="aec2b-188">一種解決方法是在您的 Web API 設定中使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="aec2b-188">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="aec2b-189">路由屬性</span><span class="sxs-lookup"><span data-stu-id="aec2b-189">Attribute Routing</span></span>

<span data-ttu-id="aec2b-190">問題： 使用 FromUri 屬性裝飾的複雜類型的模型繫結時的行為以不同的方式使用屬性的路由。</span><span class="sxs-lookup"><span data-stu-id="aec2b-190">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="aec2b-191">下列連結會追蹤問題，而且也有因應措施的相關詳細資料。</span><span class="sxs-lookup"><span data-stu-id="aec2b-191">Following link is tracking the issue and also has details about a workaround.</span></span>  
[<span data-ttu-id="aec2b-192">http://aspnetwebstack.codeplex.com/workitem/1944</span><span class="sxs-lookup"><span data-stu-id="aec2b-192">http://aspnetwebstack.codeplex.com/workitem/1944</span></span>](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="aec2b-193">問題： Scaffolding MVC/Web API，在專案中使用 5.2.0 到找出已不存在於專案中封裝的封裝導致 5.1.2</span><span class="sxs-lookup"><span data-stu-id="aec2b-193">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="aec2b-194">ASP.NET MVC 5.2 更新 NuGet 封裝不會更新的 Visual Studio 工具，例如 ASP.NET scaffolding 或 ASP.NET Web 應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="aec2b-194">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="aec2b-195">他們使用的舊版 ASP.NET 執行階段封裝 (例如 5.1.2 在 Update 2)。</span><span class="sxs-lookup"><span data-stu-id="aec2b-195">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="aec2b-196">如此一來，ASP.NET scaffolding 會安裝舊版 (例如 5.1.2 在 Update 2) 必要的封裝，如果還沒有出現在您的專案。</span><span class="sxs-lookup"><span data-stu-id="aec2b-196">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="aec2b-197">不過，在 Visual Studio 2013 RTM 或更新 1 中的 ASP.NET scaffolding 不會覆寫您的專案中最新的封裝。</span><span class="sxs-lookup"><span data-stu-id="aec2b-197">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="aec2b-198">如果您使用 ASP.NET scaffolding Web API 2.2 或 ASP.NET MVC 5.2 更新專案中的封裝之後，請確定一致的 Web API 和 ASP.NET MVC 版本。</span><span class="sxs-lookup"><span data-stu-id="aec2b-198">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="aec2b-199">Bug 修正和次要功能更新</span><span class="sxs-lookup"><span data-stu-id="aec2b-199">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="aec2b-200">此版本也包含數個 bug 修正和次要功能更新。</span><span class="sxs-lookup"><span data-stu-id="aec2b-200">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="aec2b-201">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="aec2b-201">You can find the complete list here:</span></span>

- [<span data-ttu-id="aec2b-202">5.2 封裝</span><span class="sxs-lookup"><span data-stu-id="aec2b-202">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="aec2b-203">Microsoft.AspNet.OData 5.2.1 章</span><span class="sxs-lookup"><span data-stu-id="aec2b-203">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="aec2b-204">Microsoft.AspNet.OData 5.2.1 章封裝包含 NuGet 相依性更新，但沒有 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="aec2b-204">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="aec2b-205">透過這項更新，但是再也不嚴格的相依性上 Microsoft.OData.Core 6.4.0，但其中一個可以升級到 6.4.0 和 7.0.0 之間的任何版本。</span><span class="sxs-lookup"><span data-stu-id="aec2b-205">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="aec2b-206">Microsoft.AspNet.WebAPI 5.2.2 章</span><span class="sxs-lookup"><span data-stu-id="aec2b-206">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="aec2b-207">在此版本中我們所做變更的相依性`Json.Net 6.0.4`。</span><span class="sxs-lookup"><span data-stu-id="aec2b-207">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="aec2b-208">如需有關在此版本中的最新消息`Json.NET`，請參閱[Json.NET 6.0 版本 4-JSON 合併，相依性插入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="aec2b-208">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="aec2b-209">在 Web 應用程式開發介面中沒有任何其他新功能或 bug 修正這一版。</span><span class="sxs-lookup"><span data-stu-id="aec2b-209">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="aec2b-210">接下來，我們已更新我們擁有相依於這個新版本的 Web API 的所有其他相依套件。</span><span class="sxs-lookup"><span data-stu-id="aec2b-210">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="aec2b-211">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="aec2b-211">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="aec2b-212">您可以閱讀發行[這裡](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)。</span><span class="sxs-lookup"><span data-stu-id="aec2b-212">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="aec2b-213">此版本包含只 bug 修正。</span><span class="sxs-lookup"><span data-stu-id="aec2b-213">This release contains only bug fixes.</span></span> <span data-ttu-id="aec2b-214">您可以使用[此查詢](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)若要查看此版本中修正的問題清單。</span><span class="sxs-lookup"><span data-stu-id="aec2b-214">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
