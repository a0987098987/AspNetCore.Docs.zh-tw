---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: 內含項目在 OData v4 中的使用 Web API 2.2 |Microsoft Docs
author: rick-anderson
description: 傳統上，如果它封裝在實體集，可能只能存取實體。 但 OData v4 提供兩個額外的選項，單一和 Con...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 56e550b56e9ad237dbf4fab04f2bd545164ee90a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824908"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="0be9b-104">內含項目在 OData v4 中使用 Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="0be9b-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="0be9b-105">由 Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="0be9b-105">by Jinfu Tan</span></span>

> <span data-ttu-id="0be9b-106">傳統上，如果它封裝在實體集，可能只能存取實體。</span><span class="sxs-lookup"><span data-stu-id="0be9b-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="0be9b-107">但 OData v4 提供兩個額外的選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。</span><span class="sxs-lookup"><span data-stu-id="0be9b-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="0be9b-108">本主題說明如何在 WebApi 2.2 中的 OData 端點中定義將內含項目。</span><span class="sxs-lookup"><span data-stu-id="0be9b-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="0be9b-109">如需有關內含項目的詳細資訊，請參閱[內含項目即將與 OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0be9b-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="0be9b-110">若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="0be9b-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="0be9b-111">首先，我們會建立內含項目領域模型中的 OData 服務，使用此資料模型：</span><span class="sxs-lookup"><span data-stu-id="0be9b-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![資料模型](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="0be9b-113">帳戶包含許多 PaymentInstruments (PI)，但我們未定義的實體集的 PI。</span><span class="sxs-lookup"><span data-stu-id="0be9b-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="0be9b-114">相反地，Pi 只能透過帳戶存取。</span><span class="sxs-lookup"><span data-stu-id="0be9b-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="0be9b-115">您可以下載從本主題中所使用的解決方案[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)。</span><span class="sxs-lookup"><span data-stu-id="0be9b-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="0be9b-116">定義資料模型</span><span class="sxs-lookup"><span data-stu-id="0be9b-116">Defining the data model</span></span>

1. <span data-ttu-id="0be9b-117">定義的 CLR 型別。</span><span class="sxs-lookup"><span data-stu-id="0be9b-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="0be9b-118">`Contained`屬性適用於內含項目導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0be9b-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="0be9b-119">產生的 CLR 型別為基礎的 EDM 模型。</span><span class="sxs-lookup"><span data-stu-id="0be9b-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="0be9b-120">`ODataConventionModelBuilder`建置的 EDM 模型，如果處理`Contained`屬性新增至對應的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="0be9b-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="0be9b-121">如果屬性是集合型別，`GetCount(string NameContains)`也會建立函式。</span><span class="sxs-lookup"><span data-stu-id="0be9b-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="0be9b-122">產生的中繼資料將會如下所示：</span><span class="sxs-lookup"><span data-stu-id="0be9b-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="0be9b-123">`ContainsTarget`屬性表示的導覽屬性會將內含項目。</span><span class="sxs-lookup"><span data-stu-id="0be9b-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="0be9b-124">定義包含實體集控制器</span><span class="sxs-lookup"><span data-stu-id="0be9b-124">Define the containing entity set controller</span></span>

<span data-ttu-id="0be9b-125">沒有自己的控制器; 包含的實體。動作是此控制器中定義包含實體集。</span><span class="sxs-lookup"><span data-stu-id="0be9b-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="0be9b-126">在此範例中，沒有 AccountsController，但沒有 PaymentInstrumentsController。</span><span class="sxs-lookup"><span data-stu-id="0be9b-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="0be9b-127">如果 4 個或多個區段的 OData 路徑，屬性路由的運作方式，例如`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上述控制器中。</span><span class="sxs-lookup"><span data-stu-id="0be9b-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="0be9b-128">否則，屬性與慣例路由的運作方式： 例如，`GetPayInPIs(int key)`符合`GET ~/Accounts(1)/PayinPIs`。</span><span class="sxs-lookup"><span data-stu-id="0be9b-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="0be9b-129">*感謝您來代表 Leo Hu 這篇文章的原始內容。*</span><span class="sxs-lookup"><span data-stu-id="0be9b-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
