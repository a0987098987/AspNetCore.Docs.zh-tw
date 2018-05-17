---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: 模型中 ASP.NET Web API 驗證 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="190d5-102">ASP.NET Web API 中的模型驗證</span><span class="sxs-lookup"><span data-stu-id="190d5-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="190d5-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="190d5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="190d5-104">當用戶端會將資料傳送至您的 web 應用程式開發介面時，通常您要驗證資料，再進行任何處理。</span><span class="sxs-lookup"><span data-stu-id="190d5-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="190d5-105">本文將說明如何加上註解您的模型、 使用進行資料驗證的註解和處理您的 web 應用程式開發介面中的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="190d5-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="190d5-106">資料註釋</span><span class="sxs-lookup"><span data-stu-id="190d5-106">Data Annotations</span></span>

<span data-ttu-id="190d5-107">在 ASP.NET Web API 中，您可以使用屬性[System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)在模型上設定屬性的驗證規則的命名空間。</span><span class="sxs-lookup"><span data-stu-id="190d5-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="190d5-108">請考慮下列模型：</span><span class="sxs-lookup"><span data-stu-id="190d5-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="190d5-109">如果您已在 ASP.NET MVC 中使用模型驗證，這應該看起來很熟悉。</span><span class="sxs-lookup"><span data-stu-id="190d5-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="190d5-110">**需要**屬性指出，`Name`屬性不可為 null。</span><span class="sxs-lookup"><span data-stu-id="190d5-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="190d5-111">**範圍**屬性指出，`Weight`必須是介於 0 到 999 之間。</span><span class="sxs-lookup"><span data-stu-id="190d5-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="190d5-112">假設用戶端傳送 POST 要求的下列 JSON 表示法：</span><span class="sxs-lookup"><span data-stu-id="190d5-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="190d5-113">您可以看到用戶端未包含`Name`屬性標示為不需要。</span><span class="sxs-lookup"><span data-stu-id="190d5-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="190d5-114">當 Web 應用程式開發介面將轉換成 JSON`Product`執行個體，它會驗證`Product`針對驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="190d5-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="190d5-115">在控制器動作中，您可以檢查模型是否為有效：</span><span class="sxs-lookup"><span data-stu-id="190d5-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="190d5-116">模型驗證並不保證用戶端資料安全無虞。</span><span class="sxs-lookup"><span data-stu-id="190d5-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="190d5-117">應用程式的其他圖層中，可能會需要額外的驗證。</span><span class="sxs-lookup"><span data-stu-id="190d5-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="190d5-118">（例如，資料層可能會強制執行 foreign key 條件約束。）本教學課程[使用 Web API 和 Entity Framework](../data/using-web-api-with-entity-framework/part-1.md)探討這些問題。</span><span class="sxs-lookup"><span data-stu-id="190d5-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="190d5-119">**「 資料張貼 」**： 用戶端將被排除一些屬性時會張貼資料。</span><span class="sxs-lookup"><span data-stu-id="190d5-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="190d5-120">例如，假設用戶端會傳送下列：</span><span class="sxs-lookup"><span data-stu-id="190d5-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="190d5-121">在這裡，用戶端未指定值`Price`或`Weight`。</span><span class="sxs-lookup"><span data-stu-id="190d5-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="190d5-122">JSON 格式子會指派預設值是零到遺漏的屬性。</span><span class="sxs-lookup"><span data-stu-id="190d5-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="190d5-123">模型狀態無效，因為 0 是有效的值，這些屬性。</span><span class="sxs-lookup"><span data-stu-id="190d5-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="190d5-124">這是問題，是否取決於您的案例。</span><span class="sxs-lookup"><span data-stu-id="190d5-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="190d5-125">比方說，在更新作業中，您可能想要區別 「 零 」 和 「 未設定。 」</span><span class="sxs-lookup"><span data-stu-id="190d5-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="190d5-126">若要強制用戶端設定值，將屬性設為可為 null，並設定**需要**屬性：</span><span class="sxs-lookup"><span data-stu-id="190d5-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="190d5-127">**「 過度張貼 」**： 用戶端也可以傳送*詳細*比您預期的資料。</span><span class="sxs-lookup"><span data-stu-id="190d5-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="190d5-128">例如: </span><span class="sxs-lookup"><span data-stu-id="190d5-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="190d5-129">JSON，包括屬性 （[色彩]），不存在於`Product`模型。</span><span class="sxs-lookup"><span data-stu-id="190d5-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="190d5-130">在此情況下，只是 JSON 格式器會忽略此值。</span><span class="sxs-lookup"><span data-stu-id="190d5-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="190d5-131">（XML 格式器會執行相同的。）如果您的模型具有您預定處於唯讀模式的屬性，過度張貼會造成問題。</span><span class="sxs-lookup"><span data-stu-id="190d5-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="190d5-132">例如: </span><span class="sxs-lookup"><span data-stu-id="190d5-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="190d5-133">您不想要更新的使用者`IsAdmin`屬性本身提高為系統管理員和 ！</span><span class="sxs-lookup"><span data-stu-id="190d5-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="190d5-134">最安全的策略是使用完全符合用戶端允許傳送的模型類別：</span><span class="sxs-lookup"><span data-stu-id="190d5-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="190d5-135">Brad Wilson 的部落格文章"[vs 輸入驗證。模型驗證 ASP.NET MVC 中的](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)「 有探討資料張貼以及過度。</span><span class="sxs-lookup"><span data-stu-id="190d5-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="190d5-136">雖然文章是關於 ASP.NET MVC 2，問題是仍與 Web 應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="190d5-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="190d5-137">處理驗證錯誤</span><span class="sxs-lookup"><span data-stu-id="190d5-137">Handling Validation Errors</span></span>

<span data-ttu-id="190d5-138">Web 應用程式開發介面不會自動傳回錯誤給用戶端驗證失敗時。</span><span class="sxs-lookup"><span data-stu-id="190d5-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="190d5-139">這是由控制器動作，以檢查模型狀態，並適當地回應。</span><span class="sxs-lookup"><span data-stu-id="190d5-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="190d5-140">您也可以建立動作篩選條件之前叫用控制器動作，檢查模型狀態。</span><span class="sxs-lookup"><span data-stu-id="190d5-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="190d5-141">下列程式碼顯示範例：</span><span class="sxs-lookup"><span data-stu-id="190d5-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="190d5-142">如果模型驗證失敗，此篩選條件會傳回 HTTP 回應，其中包含驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="190d5-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="190d5-143">在此情況下，控制器動作不會叫用。</span><span class="sxs-lookup"><span data-stu-id="190d5-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="190d5-144">若要將此篩選器套用至所有的 Web API 控制器，將加入的篩選條件的執行個體**HttpConfiguration.Filters**集合在設定期間：</span><span class="sxs-lookup"><span data-stu-id="190d5-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="190d5-145">另一個選項是設定為屬性的篩選器，在個別的控制站或控制器的動作：</span><span class="sxs-lookup"><span data-stu-id="190d5-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
