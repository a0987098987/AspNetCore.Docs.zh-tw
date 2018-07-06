---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: 建立單一 OData v4 中的使用 Web API 2.2 |Microsoft Docs
author: rick-anderson
description: 本主題說明如何在 Web API 2.2 中的 OData 端點定義單一值。
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830512"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="f8ba2-103">建立單一 OData v4 中的使用 Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f8ba2-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="f8ba2-104">由 Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="f8ba2-104">by Zoe Luo</span></span>

> <span data-ttu-id="f8ba2-105">傳統上，如果它封裝在實體集，可能只能存取實體。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="f8ba2-106">但 OData v4 提供兩個額外的選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="f8ba2-107">本文說明如何在 Web API 2.2 中的 OData 端點定義單一值。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="f8ba2-108">如需哪些 singleton 則和您如何受惠於使用該資訊，請參閱[來定義特殊的實體使用單一](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="f8ba2-109">若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="f8ba2-110">使用下列的資料模型在 Web API 專案中，我們會建立單一值：</span><span class="sxs-lookup"><span data-stu-id="f8ba2-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![資料模型](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="f8ba2-112">名為單一`Umbrella`會根據型別定義`Company`，且實體集具名`Employees`會根據型別定義`Employee`。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="f8ba2-113">您可以從下載本教學課程中使用的解決方案[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="f8ba2-114">定義資料模型</span><span class="sxs-lookup"><span data-stu-id="f8ba2-114">Define the data model</span></span>

1. <span data-ttu-id="f8ba2-115">定義的 CLR 型別。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="f8ba2-116">產生的 CLR 型別為基礎的 EDM 模型。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="f8ba2-117">在這裡，`builder.Singleton<Company>("Umbrella")`會告知模型產生器來建立名為單一`Umbrella`中的 EDM 模型。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="f8ba2-118">產生的中繼資料將會如下所示：</span><span class="sxs-lookup"><span data-stu-id="f8ba2-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="f8ba2-119">從中繼資料我們可以看到的導覽屬性`Company`中`Employees`實體集繫結至單一`Umbrella`。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="f8ba2-120">繫結會自動進行`ODataConventionModelBuilder`，因為只有`Umbrella`具有`Company`型別。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="f8ba2-121">如果在模型中沒有任何模稜兩可，您可以使用`HasSingletonBinding`來明確地將導覽屬性繫結至單一值;`HasSingletonBinding`具有相同的效果與使用`Singleton`CLR 型別定義中的屬性：</span><span class="sxs-lookup"><span data-stu-id="f8ba2-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="f8ba2-122">定義單一控制器</span><span class="sxs-lookup"><span data-stu-id="f8ba2-122">Define the singleton controller</span></span>

<span data-ttu-id="f8ba2-123">EntitySet 控制器，例如單一控制站會繼承自`ODataController`，而且單一控制站名稱應該`[singletonName]Controller`。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="f8ba2-124">若要處理不同類型的要求，動作必須是在控制器中預先定義。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="f8ba2-125">**屬性路由**WebApi 2.2 中的預設會啟用。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="f8ba2-126">例如，若要定義的動作，以處理查詢`Revenue`從`Company`使用屬性路由中，使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="f8ba2-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="f8ba2-127">如果您不願意定義每個動作的屬性，只會定義您遵循的動作[OData 路由慣例](../odata-routing-conventions.md)。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="f8ba2-128">找不到需要查詢為單一子句的金鑰，因為單一控制器中定義的動作是 entityset 控制器中定義的動作稍有不同。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="f8ba2-129">如需參考，如下所列的單一控制器中的每個動作定義的方法簽章。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="f8ba2-130">基本上，這是您只需要在服務端上執行。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="f8ba2-131">[範例專案](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)包含所有的解決方案以及示範如何使用單一的 OData 用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="f8ba2-132">建置用戶端中的步驟[建立 OData v4 用戶端應用程式](create-an-odata-v4-client-app.md)。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="f8ba2-133">。</span><span class="sxs-lookup"><span data-stu-id="f8ba2-133">.</span></span> 

<span data-ttu-id="f8ba2-134">*感謝您來代表 Leo Hu 這篇文章的原始內容。*</span><span class="sxs-lookup"><span data-stu-id="f8ba2-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
