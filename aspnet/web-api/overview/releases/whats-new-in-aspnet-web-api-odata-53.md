---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: 什麼是 ASP.NET Web API OData 5.3 的新功能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827230"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="f1083-102">什麼是 ASP.NET Web API OData 5.3 的新功能</span><span class="sxs-lookup"><span data-stu-id="f1083-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="f1083-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f1083-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="f1083-104">本主題會描述什麼是 ASP.NET Web API OData 5.3 的新功能。</span><span class="sxs-lookup"><span data-stu-id="f1083-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="f1083-105">下載</span><span class="sxs-lookup"><span data-stu-id="f1083-105">Download</span></span>](#download)
- [<span data-ttu-id="f1083-106">文件</span><span class="sxs-lookup"><span data-stu-id="f1083-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="f1083-107">OData 核心程式庫</span><span class="sxs-lookup"><span data-stu-id="f1083-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="f1083-108">新功能</span><span class="sxs-lookup"><span data-stu-id="f1083-108">New Features</span></span>](#newf)
- [<span data-ttu-id="f1083-109">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="f1083-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="f1083-110">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="f1083-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="f1083-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="f1083-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="f1083-112">下載</span><span class="sxs-lookup"><span data-stu-id="f1083-112">Download</span></span>

<span data-ttu-id="f1083-113">執行階段功能會以 NuGet 套件，NuGet gallery 上發行。</span><span class="sxs-lookup"><span data-stu-id="f1083-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="f1083-114">您可以安裝或使用 NuGet 套件管理員主控台更新到發行的 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="f1083-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="f1083-115">文件</span><span class="sxs-lookup"><span data-stu-id="f1083-115">Documentation</span></span>

<span data-ttu-id="f1083-116">您可以找到教學課程和其他相關文件在 ASP.NET Web API OData [ASP.NET 網站](../odata-support-in-aspnet-web-api/index.md)。</span><span class="sxs-lookup"><span data-stu-id="f1083-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="f1083-117">OData 核心程式庫</span><span class="sxs-lookup"><span data-stu-id="f1083-117">OData Core Libraries</span></span>

<span data-ttu-id="f1083-118">針對 OData v4，Web API 現在使用 ODataLib 版本 6.5.0</span><span class="sxs-lookup"><span data-stu-id="f1083-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="f1083-119">新的功能，在 ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="f1083-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="f1083-120">展開 $levels $ 中的支援</span><span class="sxs-lookup"><span data-stu-id="f1083-120">Support for $levels in $expand</span></span>

<span data-ttu-id="f1083-121">您可以使用 $levels 選項擴充查詢的查詢。</span><span class="sxs-lookup"><span data-stu-id="f1083-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="f1083-122">例如: </span><span class="sxs-lookup"><span data-stu-id="f1083-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="f1083-123">這個查詢相當於：</span><span class="sxs-lookup"><span data-stu-id="f1083-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="f1083-124">開啟的實體類型的支援</span><span class="sxs-lookup"><span data-stu-id="f1083-124">Support for Open Entity Types</span></span>

<span data-ttu-id="f1083-125">*開啟輸入*stuctured 型別，其中包含動態屬性，除了任何型別定義中宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="f1083-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="f1083-126">開放式類型可讓您將新增至您的資料模型的彈性。</span><span class="sxs-lookup"><span data-stu-id="f1083-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="f1083-127">如需詳細資訊，請參閱 xxxx。</span><span class="sxs-lookup"><span data-stu-id="f1083-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="f1083-128">支援開啟類型中的動態集合屬性</span><span class="sxs-lookup"><span data-stu-id="f1083-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="f1083-129">以前，動態屬性必須是單一值。</span><span class="sxs-lookup"><span data-stu-id="f1083-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="f1083-130">在 5.3，動態屬性可以有集合值。</span><span class="sxs-lookup"><span data-stu-id="f1083-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="f1083-131">例如，在下列的 JSON 承載，`Emails`屬性是動態屬性，而且屬於字串類型的集合：</span><span class="sxs-lookup"><span data-stu-id="f1083-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="f1083-132">支援的複雜類型的繼承</span><span class="sxs-lookup"><span data-stu-id="f1083-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="f1083-133">現在的複雜型別可以繼承自基底類型。</span><span class="sxs-lookup"><span data-stu-id="f1083-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="f1083-134">例如，OData 服務可以定義下列複雜類型：</span><span class="sxs-lookup"><span data-stu-id="f1083-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="f1083-135">以下是此範例中的 EDM:</span><span class="sxs-lookup"><span data-stu-id="f1083-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="f1083-136">如需詳細資訊，請參閱 < [OData 複雜類型繼承範例](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。</span><span class="sxs-lookup"><span data-stu-id="f1083-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f1083-137">已知的問題和重大變更</span><span class="sxs-lookup"><span data-stu-id="f1083-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="f1083-138">本章節描述的已知的問題和 ASP.NET Web API OData 5.3 的重大變更。</span><span class="sxs-lookup"><span data-stu-id="f1083-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="f1083-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="f1083-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="f1083-140">查詢選項</span><span class="sxs-lookup"><span data-stu-id="f1083-140">Query Options</span></span>

<span data-ttu-id="f1083-141">問題： 使用巢狀的 $擴充 $levels = 最大的結果中不正確的展開深度。</span><span class="sxs-lookup"><span data-stu-id="f1083-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="f1083-142">例如，假設下列要求：</span><span class="sxs-lookup"><span data-stu-id="f1083-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="f1083-143">如果`MaxExpansionDepth`為 5，此查詢會產生 6 之展開深度。</span><span class="sxs-lookup"><span data-stu-id="f1083-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="f1083-144">Bug 修正和次要功能更新</span><span class="sxs-lookup"><span data-stu-id="f1083-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="f1083-145">此版本也包含數個 bug 修正和次要功能更新。</span><span class="sxs-lookup"><span data-stu-id="f1083-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="f1083-146">您可以找到完整的清單：</span><span class="sxs-lookup"><span data-stu-id="f1083-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="f1083-147">Bug 修正</span><span class="sxs-lookup"><span data-stu-id="f1083-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="f1083-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="f1083-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="f1083-149">此版本進行[修正](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All)一些 AllowedFunctions 列舉。</span><span class="sxs-lookup"><span data-stu-id="f1083-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="f1083-150">此版本沒有任何其他錯誤修正或新功能。</span><span class="sxs-lookup"><span data-stu-id="f1083-150">This release doesn't have any other bug fixes or new features.</span></span>
