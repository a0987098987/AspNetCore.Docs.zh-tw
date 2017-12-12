---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "建立單一 OData v4 中的使用 Web 應用程式開發介面 2.2 |Microsoft 文件"
author: rick-anderson
description: "本主題說明如何在 Web 應用程式開發介面 2.2 中的 OData 端點中定義為單一子句。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="3eb96-103">建立單一 OData v4 中的使用 Web 應用程式開發介面 2.2</span><span class="sxs-lookup"><span data-stu-id="3eb96-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="3eb96-104">由毛毯 Luo</span><span class="sxs-lookup"><span data-stu-id="3eb96-104">by Zoe Luo</span></span>

> <span data-ttu-id="3eb96-105">傳統上，如果它封裝內的實體集，可能只存取實體。</span><span class="sxs-lookup"><span data-stu-id="3eb96-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="3eb96-106">但是 OData v4 提供另外兩個選項，單一和內含項目，這兩種 WebAPI 2.2 的支援。</span><span class="sxs-lookup"><span data-stu-id="3eb96-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="3eb96-107">本文示範如何在 Web 應用程式開發介面 2.2 中的 OData 端點中定義為單一子句。</span><span class="sxs-lookup"><span data-stu-id="3eb96-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="3eb96-108">為單一子句以及如何享有使用它的資訊，請參閱[使用單一定義特殊實體](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)。</span><span class="sxs-lookup"><span data-stu-id="3eb96-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="3eb96-109">若要建立 Web API OData V4 端點，請參閱[建立 OData v4 端點使用 ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)。</span><span class="sxs-lookup"><span data-stu-id="3eb96-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="3eb96-110">我們將建立單一 Web API 專案使用下列資料模型中：</span><span class="sxs-lookup"><span data-stu-id="3eb96-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![資料模型](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="3eb96-112">名為單一`Umbrella`會根據型別定義`Company`，和名為實體`Employees`會根據型別定義`Employee`。</span><span class="sxs-lookup"><span data-stu-id="3eb96-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="3eb96-113">您可以從下載本教學課程中所用之解決方案[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)。</span><span class="sxs-lookup"><span data-stu-id="3eb96-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="3eb96-114">定義資料模型</span><span class="sxs-lookup"><span data-stu-id="3eb96-114">Define the data model</span></span>

1. <span data-ttu-id="3eb96-115">定義 CLR 類型。</span><span class="sxs-lookup"><span data-stu-id="3eb96-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="3eb96-116">產生的 CLR 型別為基礎的 EDM 模型。</span><span class="sxs-lookup"><span data-stu-id="3eb96-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="3eb96-117">在這裡，`builder.Singleton<Company>("Umbrella")`會告知模型產生器建立名為單一`Umbrella`中的 EDM 模型。</span><span class="sxs-lookup"><span data-stu-id="3eb96-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="3eb96-118">產生的中繼資料將會如下所示：</span><span class="sxs-lookup"><span data-stu-id="3eb96-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="3eb96-119">從中繼資料我們可以看到的導覽屬性`Company`中`Employees`實體集繫結至 singleton `Umbrella`。</span><span class="sxs-lookup"><span data-stu-id="3eb96-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="3eb96-120">繫結是自動由`ODataConventionModelBuilder`，因為只有`Umbrella`具有`Company`型別。</span><span class="sxs-lookup"><span data-stu-id="3eb96-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="3eb96-121">如果模型中有任何模稜兩可，您可以使用`HasSingletonBinding`明確地為單一子句; 繫結的瀏覽屬性`HasSingletonBinding`有使用相同的效果`Singleton`CLR 型別定義中的屬性：</span><span class="sxs-lookup"><span data-stu-id="3eb96-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="3eb96-122">定義單一控制器</span><span class="sxs-lookup"><span data-stu-id="3eb96-122">Define the singleton controller</span></span>

<span data-ttu-id="3eb96-123">單一控制器繼承自 EntitySet 控制器，例如`ODataController`，而且單一控制器名稱應該是`[singletonName]Controller`。</span><span class="sxs-lookup"><span data-stu-id="3eb96-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="3eb96-124">若要處理不同類型的要求，動作必須是在控制器中預先定義。</span><span class="sxs-lookup"><span data-stu-id="3eb96-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="3eb96-125">**屬性路由**WebApi 2.2 中預設會啟用。</span><span class="sxs-lookup"><span data-stu-id="3eb96-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="3eb96-126">例如，若要定義的動作來處理查詢`Revenue`從`Company`使用路由屬性，使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="3eb96-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="3eb96-127">如果您不願意定義每個動作的屬性，只會定義您的動作之後[OData 路由慣例](../odata-routing-conventions.md)。</span><span class="sxs-lookup"><span data-stu-id="3eb96-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="3eb96-128">則不需要查詢單一金鑰，因為單一控制器中定義的動作就會有稍微不同於 entityset 控制器中定義的動作。</span><span class="sxs-lookup"><span data-stu-id="3eb96-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="3eb96-129">如需參考，每個動作中定義的單一控制站的方法簽章如下所示。</span><span class="sxs-lookup"><span data-stu-id="3eb96-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="3eb96-130">基本上，這是您只需要在服務端上執行。</span><span class="sxs-lookup"><span data-stu-id="3eb96-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="3eb96-131">[範例專案](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)包含所有方案和示範如何使用單一的 OData 用戶端的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3eb96-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="3eb96-132">建置用戶端中的步驟[建立 OData v4 用戶端應用程式](create-an-odata-v4-client-app.md)。</span><span class="sxs-lookup"><span data-stu-id="3eb96-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="3eb96-133">。</span><span class="sxs-lookup"><span data-stu-id="3eb96-133">.</span></span> 

<span data-ttu-id="3eb96-134">*由於原始內容的這篇文章 Leo Hu。*</span><span class="sxs-lookup"><span data-stu-id="3eb96-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
