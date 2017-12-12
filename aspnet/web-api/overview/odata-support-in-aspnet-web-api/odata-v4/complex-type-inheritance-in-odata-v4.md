---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "使用 ASP.NET Web API OData v4 中的複雜型別繼承 |Microsoft 文件"
author: microsoft
description: "根據 OData v4 規格中，複雜類型可以繼承自另一個複雜類型。 （複雜型別是結構化的型別沒有索引鍵）。Web API..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="f7e37-104">使用 ASP.NET Web API OData v4 中的複雜型別繼承</span><span class="sxs-lookup"><span data-stu-id="f7e37-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="f7e37-105">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f7e37-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f7e37-106">根據 OData v4[規格](http://www.odata.org/documentation/odata-version-4-0/)，複雜類型可以繼承自另一個複雜類型。</span><span class="sxs-lookup"><span data-stu-id="f7e37-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="f7e37-107">(A*複雜*類型是結構化型別沒有索引鍵。)Web API OData 5.3 支援複雜類型的繼承。</span><span class="sxs-lookup"><span data-stu-id="f7e37-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="f7e37-108">本主題示範如何建置繼承複雜類型的實體資料模型 (EDM)。</span><span class="sxs-lookup"><span data-stu-id="f7e37-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="f7e37-109">完整的原始程式碼，請參閱[OData 複雜型別繼承範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)。</span><span class="sxs-lookup"><span data-stu-id="f7e37-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f7e37-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="f7e37-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f7e37-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="f7e37-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="f7e37-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="f7e37-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="f7e37-113">模型階層</span><span class="sxs-lookup"><span data-stu-id="f7e37-113">Model Hierarchy</span></span>

<span data-ttu-id="f7e37-114">為了說明複雜型別繼承，我們將使用下列類別階層架構。</span><span class="sxs-lookup"><span data-stu-id="f7e37-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="f7e37-115">`Shape`是抽象的複雜類型。</span><span class="sxs-lookup"><span data-stu-id="f7e37-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="f7e37-116">`Rectangle``Triangle`，和`Circle`複雜型別衍生自`Shape`，和`RoundRectangle`衍生自`Rectangle`。</span><span class="sxs-lookup"><span data-stu-id="f7e37-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="f7e37-117">`Window`是實體類型，包含`Shape`執行個體。</span><span class="sxs-lookup"><span data-stu-id="f7e37-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="f7e37-118">以下是定義這些類型的 CLR 類別。</span><span class="sxs-lookup"><span data-stu-id="f7e37-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="f7e37-119">建置的 EDM 模型</span><span class="sxs-lookup"><span data-stu-id="f7e37-119">Build the EDM Model</span></span>

<span data-ttu-id="f7e37-120">若要建立 EDM，您可以使用**ODataConventionModelBuilder**，這會推斷從 CLR 類型的繼承關聯性。</span><span class="sxs-lookup"><span data-stu-id="f7e37-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="f7e37-121">您也可以建置 EDM 明確地使用**ODataModelBuilder**。</span><span class="sxs-lookup"><span data-stu-id="f7e37-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="f7e37-122">這會採用更多的程式碼，但讓您更充分掌控 EDM。</span><span class="sxs-lookup"><span data-stu-id="f7e37-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="f7e37-123">這兩個範例會建立相同的 EDM 結構描述。</span><span class="sxs-lookup"><span data-stu-id="f7e37-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="f7e37-124">中繼資料文件</span><span class="sxs-lookup"><span data-stu-id="f7e37-124">Metadata Document</span></span>

<span data-ttu-id="f7e37-125">以下是 OData 中繼資料文件，顯示複雜型別繼承。</span><span class="sxs-lookup"><span data-stu-id="f7e37-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="f7e37-126">中繼資料文件中，您可以看到：</span><span class="sxs-lookup"><span data-stu-id="f7e37-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="f7e37-127">`Shape`複雜類型是抽象的。</span><span class="sxs-lookup"><span data-stu-id="f7e37-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="f7e37-128">`Rectangle`， `Triangle`，和`Circle`複雜類型具有基底型別`Shape`。</span><span class="sxs-lookup"><span data-stu-id="f7e37-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="f7e37-129">`RoundRectangle`類型具有基底類型`Rectangle`。</span><span class="sxs-lookup"><span data-stu-id="f7e37-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="f7e37-130">將複雜型別轉型</span><span class="sxs-lookup"><span data-stu-id="f7e37-130">Casting Complex Types</span></span>

<span data-ttu-id="f7e37-131">現在支援複雜類型上進行轉換。</span><span class="sxs-lookup"><span data-stu-id="f7e37-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="f7e37-132">例如，下列查詢會轉換`Shape`至`Rectangle`。</span><span class="sxs-lookup"><span data-stu-id="f7e37-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="f7e37-133">以下是回應裝載：</span><span class="sxs-lookup"><span data-stu-id="f7e37-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
