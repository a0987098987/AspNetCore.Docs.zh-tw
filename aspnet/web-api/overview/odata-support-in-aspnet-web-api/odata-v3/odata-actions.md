---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: 支援在 ASP.NET Web API 2 OData 動作 |Microsoft 文件
author: MikeWasson
description: 在 OData 中，動作是加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。 動作的一些用法包括： 實作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508387"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="2faaa-104">支援在 ASP.NET Web API 2 OData 動作</span><span class="sxs-lookup"><span data-stu-id="2faaa-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2faaa-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2faaa-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2faaa-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="2faaa-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="2faaa-107">在 OData 中，*動作*會加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。</span><span class="sxs-lookup"><span data-stu-id="2faaa-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="2faaa-108">動作的一些用法包括：</span><span class="sxs-lookup"><span data-stu-id="2faaa-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="2faaa-109">實作複雜的交易。</span><span class="sxs-lookup"><span data-stu-id="2faaa-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="2faaa-110">同時管理數個項目。</span><span class="sxs-lookup"><span data-stu-id="2faaa-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="2faaa-111">允許特定實體屬性的更新。</span><span class="sxs-lookup"><span data-stu-id="2faaa-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="2faaa-112">將資訊傳送到未定義實體中的伺服器。</span><span class="sxs-lookup"><span data-stu-id="2faaa-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2faaa-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="2faaa-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2faaa-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2faaa-114">Web API 2</span></span>
> - <span data-ttu-id="2faaa-115">OData 第 3 版</span><span class="sxs-lookup"><span data-stu-id="2faaa-115">OData Version 3</span></span>
> - <span data-ttu-id="2faaa-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2faaa-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="2faaa-117">範例： 評等產品</span><span class="sxs-lookup"><span data-stu-id="2faaa-117">Example: Rating a Product</span></span>

<span data-ttu-id="2faaa-118">在此範例中，我們想要讓使用者評等產品，然後再公開之每項產品的平均評等。</span><span class="sxs-lookup"><span data-stu-id="2faaa-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="2faaa-119">在資料庫上，我們將儲存評等做為索引鍵與產品的清單。</span><span class="sxs-lookup"><span data-stu-id="2faaa-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="2faaa-120">以下是我們可能會使用它來表示 Entity Framework 中的評等的模型：</span><span class="sxs-lookup"><span data-stu-id="2faaa-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="2faaa-121">但我們不想用戶端張貼`ProductRating`物件加入至 「 評等 」 集合。</span><span class="sxs-lookup"><span data-stu-id="2faaa-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="2faaa-122">直覺的方式，評等產品集合中，與相關聯且用戶端應該只需要文章的分級值。</span><span class="sxs-lookup"><span data-stu-id="2faaa-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="2faaa-123">因此，而不是使用一般的 CRUD 作業，我們會定義用戶端可以叫用動作產品上。</span><span class="sxs-lookup"><span data-stu-id="2faaa-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="2faaa-124">在 OData 術語中，動作是*繫結*產品實體。</span><span class="sxs-lookup"><span data-stu-id="2faaa-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="2faaa-125">動作的伺服器上有副作用。</span><span class="sxs-lookup"><span data-stu-id="2faaa-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="2faaa-126">基於這個理由，它們會叫用使用 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="2faaa-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="2faaa-127">動作可以有參數和傳回型別，服務中繼資料中所述。</span><span class="sxs-lookup"><span data-stu-id="2faaa-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="2faaa-128">用戶端傳送參數在要求主體中，且伺服器會在回應主體中傳送的傳回值。</span><span class="sxs-lookup"><span data-stu-id="2faaa-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="2faaa-129">要叫用 「 速率產品 」 動作，用戶端傳送 POST 至 URI，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2faaa-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="2faaa-130">POST 要求中的資料是直接的產品評比：</span><span class="sxs-lookup"><span data-stu-id="2faaa-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="2faaa-131">宣告的實體資料模型中的動作</span><span class="sxs-lookup"><span data-stu-id="2faaa-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="2faaa-132">在 Web API 設定中，將動作加入至實體資料模型 (EDM):</span><span class="sxs-lookup"><span data-stu-id="2faaa-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="2faaa-133">此程式碼會定義為可以在產品實體執行的動作"RateProduct"。</span><span class="sxs-lookup"><span data-stu-id="2faaa-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="2faaa-134">它也會宣告採取的動作**int**參數名為"評等 」，並傳回**int**值。</span><span class="sxs-lookup"><span data-stu-id="2faaa-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="2faaa-135">將動作加入至控制器</span><span class="sxs-lookup"><span data-stu-id="2faaa-135">Add the Action to the Controller</span></span>

<span data-ttu-id="2faaa-136">產品實體所繫結 」 RateProduct 」 動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="2faaa-137">若要實作動作，新增名為`RateProduct`產品控制站：</span><span class="sxs-lookup"><span data-stu-id="2faaa-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="2faaa-138">請注意方法名稱符合 EDM 中的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="2faaa-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="2faaa-139">方法有兩個參數：</span><span class="sxs-lookup"><span data-stu-id="2faaa-139">The method has two parameters:</span></span>

- <span data-ttu-id="2faaa-140">*索引鍵*： 速率的產品金鑰。</span><span class="sxs-lookup"><span data-stu-id="2faaa-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="2faaa-141">*參數*： 動作參數值的字典。</span><span class="sxs-lookup"><span data-stu-id="2faaa-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="2faaa-142">如果您使用的預設路由慣例，機碼的參數必須為 「 金鑰 」。</span><span class="sxs-lookup"><span data-stu-id="2faaa-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="2faaa-143">它也是一定要包含 **[FromOdataUri]** 屬性，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2faaa-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="2faaa-144">這個屬性會告知使用 OData 語法規則，它會剖析要求 URI 中的索引鍵時的 Web API。</span><span class="sxs-lookup"><span data-stu-id="2faaa-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="2faaa-145">使用*參數*取得動作參數的字典：</span><span class="sxs-lookup"><span data-stu-id="2faaa-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="2faaa-146">如果用戶端傳送的 action 參數中正確格式化，值**ModelState.IsValid**為 true。</span><span class="sxs-lookup"><span data-stu-id="2faaa-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="2faaa-147">在此情況下，您可以使用**ODataActionParameters**字典，以便取得參數值。</span><span class="sxs-lookup"><span data-stu-id="2faaa-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="2faaa-148">在此範例中，`RateProduct`動作接受名為"評等 」 的單一參數。</span><span class="sxs-lookup"><span data-stu-id="2faaa-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="2faaa-149">動作的中繼資料</span><span class="sxs-lookup"><span data-stu-id="2faaa-149">Action Metadata</span></span>

<span data-ttu-id="2faaa-150">若要檢視服務中繼資料，GET 要求傳送至 /odata/$ 中繼資料。</span><span class="sxs-lookup"><span data-stu-id="2faaa-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="2faaa-151">以下是宣告的中繼資料的部分`RateProduct`動作：</span><span class="sxs-lookup"><span data-stu-id="2faaa-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="2faaa-152">**FunctionImport**項目宣告的動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="2faaa-153">大部分欄位都一目了然，但兩個值得注意的是：</span><span class="sxs-lookup"><span data-stu-id="2faaa-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="2faaa-154">**Isbindable 時**表示可以叫用動作上，在目標實體至少部分時間。</span><span class="sxs-lookup"><span data-stu-id="2faaa-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="2faaa-155">**IsAlwaysBindable**表示可以永遠目標實體上叫用動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="2faaa-156">不同的是，有些動作永遠都可以使用用戶端使用，但其他動作可能會取決於實體的狀態。</span><span class="sxs-lookup"><span data-stu-id="2faaa-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="2faaa-157">例如，假設您定義 「 採購 」 動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="2faaa-158">您可以只購買是庫存項目。</span><span class="sxs-lookup"><span data-stu-id="2faaa-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="2faaa-159">如果項目存貨不足時，用戶端無法叫用該動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="2faaa-160">當您定義 EDM**動作**方法會建立可繫結的動作：</span><span class="sxs-lookup"><span data-stu-id="2faaa-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="2faaa-161">我將討論不一定為可繫結的動作 (也稱為*暫時性*動作) 本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="2faaa-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="2faaa-162">叫用動作</span><span class="sxs-lookup"><span data-stu-id="2faaa-162">Invoking the Action</span></span>

<span data-ttu-id="2faaa-163">現在讓我們來看看如何在用戶端會叫用這個動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="2faaa-164">假設用戶端想要讓識別碼的產品為等級 2 = 4。</span><span class="sxs-lookup"><span data-stu-id="2faaa-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="2faaa-165">以下是範例要求訊息，要求主體中使用 JSON 格式：</span><span class="sxs-lookup"><span data-stu-id="2faaa-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="2faaa-166">以下是回應訊息：</span><span class="sxs-lookup"><span data-stu-id="2faaa-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="2faaa-167">繫結至實體集的動作</span><span class="sxs-lookup"><span data-stu-id="2faaa-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="2faaa-168">在上述範例中，動作會繫結至單一實體： 用戶端的程度來分級單一產品。</span><span class="sxs-lookup"><span data-stu-id="2faaa-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="2faaa-169">您也可以結合動作至實體的集合。</span><span class="sxs-lookup"><span data-stu-id="2faaa-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="2faaa-170">只要進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="2faaa-170">Just make the following changes:</span></span>

<span data-ttu-id="2faaa-171">在 EDM 中，將動作加入至實體的**集合**屬性。</span><span class="sxs-lookup"><span data-stu-id="2faaa-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="2faaa-172">在控制器方法中，省略*金鑰*參數。</span><span class="sxs-lookup"><span data-stu-id="2faaa-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="2faaa-173">現在用戶端會叫用 Products 實體集的動作：</span><span class="sxs-lookup"><span data-stu-id="2faaa-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="2faaa-174">集合參數的動作</span><span class="sxs-lookup"><span data-stu-id="2faaa-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="2faaa-175">動作可以有參數接受值的集合。</span><span class="sxs-lookup"><span data-stu-id="2faaa-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="2faaa-176">在 EDM 中，使用**CollectionParameter&lt;T&gt;** 來宣告參數。</span><span class="sxs-lookup"><span data-stu-id="2faaa-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="2faaa-177">這會宣告名為"評等 」，可接受的集合參數**int**值。</span><span class="sxs-lookup"><span data-stu-id="2faaa-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="2faaa-178">在控制器方法中，您仍可以獲得參數值從**ODataActionParameters**物件，但現在值是**ICollection&lt;int&gt;** 值：</span><span class="sxs-lookup"><span data-stu-id="2faaa-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="2faaa-179">暫時性的動作</span><span class="sxs-lookup"><span data-stu-id="2faaa-179">Transient Actions</span></span>

<span data-ttu-id="2faaa-180">在 「 RateProduct 」 範例中，使用者永遠會率產品，因此永遠都是可用的動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="2faaa-181">但是某些動作取決於實體的狀態。</span><span class="sxs-lookup"><span data-stu-id="2faaa-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="2faaa-182">例如，在影片出租服務中，「 結帳 」 動作不一定可用。</span><span class="sxs-lookup"><span data-stu-id="2faaa-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="2faaa-183">（它相依該視訊的複本是否可以使用）。這種類型的動作稱為*暫時性*動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="2faaa-184">服務中繼資料中的暫時性的動作有**IsAlwaysBindable**等於 false。</span><span class="sxs-lookup"><span data-stu-id="2faaa-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="2faaa-185">是實際的預設值，讓中繼資料看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="2faaa-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="2faaa-186">以下這之所以重要的原因： 如果動作是暫時性的則伺服器需要告知用戶端時的動作是否可用。</span><span class="sxs-lookup"><span data-stu-id="2faaa-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="2faaa-187">它會在實體中包含的動作連結。</span><span class="sxs-lookup"><span data-stu-id="2faaa-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="2faaa-188">以下是電影實體的範例：</span><span class="sxs-lookup"><span data-stu-id="2faaa-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="2faaa-189">「 #CheckOut"屬性包含簽出動作的連結。</span><span class="sxs-lookup"><span data-stu-id="2faaa-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="2faaa-190">找不到動作，伺服器會忽略連結。</span><span class="sxs-lookup"><span data-stu-id="2faaa-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="2faaa-191">若要宣告 EDM 中的暫時性動作，請呼叫**TransientAction**方法：</span><span class="sxs-lookup"><span data-stu-id="2faaa-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="2faaa-192">此外，您必須提供傳回特定實體的動作連結的函式。</span><span class="sxs-lookup"><span data-stu-id="2faaa-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="2faaa-193">設定此函式，藉由呼叫**HasActionLink**。</span><span class="sxs-lookup"><span data-stu-id="2faaa-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="2faaa-194">您可以撰寫函式做為 lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="2faaa-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="2faaa-195">如果動作是否可用，lambda 運算式會傳回連結至動作。</span><span class="sxs-lookup"><span data-stu-id="2faaa-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="2faaa-196">OData 序列化程式包含此連結時，它會序列化實體。</span><span class="sxs-lookup"><span data-stu-id="2faaa-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="2faaa-197">無法使用此動作時，此函數會傳回`null`。</span><span class="sxs-lookup"><span data-stu-id="2faaa-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2faaa-198">其他資源</span><span class="sxs-lookup"><span data-stu-id="2faaa-198">Additional Resources</span></span>

[<span data-ttu-id="2faaa-199">OData 動作範例</span><span class="sxs-lookup"><span data-stu-id="2faaa-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
