---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: 內含項目中 OData v4 使用 Web 應用程式開發介面 2.2 |Microsoft 文件
author: rick-anderson
description: 傳統上，如果它封裝內的實體集，可能只存取實體。 但是，OData v4 提供另外兩個選項，單一和 Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26507997"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="0e1a2-104">內含項目中 OData v4 使用 Web 應用程式開發介面 2.2</span><span class="sxs-lookup"><span data-stu-id="0e1a2-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="0e1a2-105">由 Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="0e1a2-105">by Jinfu Tan</span></span>

> <span data-ttu-id="0e1a2-106">傳統上，如果它封裝內的實體集，可能只存取實體。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="0e1a2-107">但是 OData v4 提供另外兩個選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="0e1a2-108">本主題說明如何在 WebApi 2.2 中的 OData 端點中定義的內含項目。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="0e1a2-109">如需內含項目，請參閱[內含項目來自與 OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="0e1a2-110">若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="0e1a2-111">首先，我們將建立內含項目網域模型中的 OData 服務，請使用此資料模型：</span><span class="sxs-lookup"><span data-stu-id="0e1a2-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![資料模型](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="0e1a2-113">帳戶包含許多 PaymentInstruments (PI)，但我們不會定義實體集 PI。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="0e1a2-114">相反地，Pi 只能透過帳戶存取。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="0e1a2-115">您可以下載從本主題中所用之解決方案[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="0e1a2-116">定義資料模型</span><span class="sxs-lookup"><span data-stu-id="0e1a2-116">Defining the data model</span></span>

1. <span data-ttu-id="0e1a2-117">定義 CLR 類型。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="0e1a2-118">`Contained`屬性用於內含項目導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="0e1a2-119">產生的 CLR 型別為基礎的 EDM 模型。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="0e1a2-120">`ODataConventionModelBuilder`建置的 EDM 模型，如果將處理`Contained`屬性加入至對應的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="0e1a2-121">如果屬性是集合型別，`GetCount(string NameContains)`也會建立函式。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="0e1a2-122">產生的中繼資料將會如下所示：</span><span class="sxs-lookup"><span data-stu-id="0e1a2-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="0e1a2-123">`ContainsTarget`屬性會指出的導覽屬性是否將內含項目。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="0e1a2-124">定義包含實體集控制器</span><span class="sxs-lookup"><span data-stu-id="0e1a2-124">Define the containing entity set controller</span></span>

<span data-ttu-id="0e1a2-125">包含的實體沒有自己的控制器。動作被定義中包含的實體集控制器。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="0e1a2-126">在此範例中，沒有 AccountsController，但沒有 PaymentInstrumentsController。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="0e1a2-127">如果 OData 路徑區段 4 或更多，僅屬性路由的運作方式，例如`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上述控制器中。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="0e1a2-128">否則，屬性和傳統路由的運作方式： 例如，`GetPayInPIs(int key)`符合`GET ~/Accounts(1)/PayinPIs`。</span><span class="sxs-lookup"><span data-stu-id="0e1a2-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="0e1a2-129">*由於原始內容的這篇文章 Leo Hu。*</span><span class="sxs-lookup"><span data-stu-id="0e1a2-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
