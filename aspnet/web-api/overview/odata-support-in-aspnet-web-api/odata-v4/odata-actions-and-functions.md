---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: 動作和函式在 OData v4 中的使用 ASP.NET Web API 2.2 |Microsoft Docs
author: MikeWasson
description: 在 OData 中，動作和函式會將不會輕易地定義為實體上的 CRUD 作業的伺服器端行為的方式。 本教學課程示範如何...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832635"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="853d1-104">動作和函式在 OData v4 中的使用 ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="853d1-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="853d1-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="853d1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="853d1-106">在 OData 中，動作和函式會將不會輕易地定義為實體上的 CRUD 作業的伺服器端行為的方式。</span><span class="sxs-lookup"><span data-stu-id="853d1-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="853d1-107">本教學課程會示範如何將 OData v4 端點，使用 Web API 2.2 中的動作和函式。</span><span class="sxs-lookup"><span data-stu-id="853d1-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="853d1-108">本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="853d1-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="853d1-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="853d1-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="853d1-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="853d1-110">Web API 2.2</span></span>
> - <span data-ttu-id="853d1-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="853d1-111">OData v4</span></span>
> - [<span data-ttu-id="853d1-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="853d1-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="853d1-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="853d1-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="853d1-114">教學課程的版本</span><span class="sxs-lookup"><span data-stu-id="853d1-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="853d1-115">適用於 OData 版本 3，請參閱[ASP.NET Web API 2 中的 OData 動作](../odata-v3/odata-actions.md)。</span><span class="sxs-lookup"><span data-stu-id="853d1-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="853d1-116">之間的差異*動作*並*函式*是動作可能有副作用，而且函式不這麼做。</span><span class="sxs-lookup"><span data-stu-id="853d1-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="853d1-117">動作和函式可以傳回資料。</span><span class="sxs-lookup"><span data-stu-id="853d1-117">Both actions and functions can return data.</span></span> <span data-ttu-id="853d1-118">動作的部分用法包括：</span><span class="sxs-lookup"><span data-stu-id="853d1-118">Some uses for actions include:</span></span>

- <span data-ttu-id="853d1-119">複雜的交易。</span><span class="sxs-lookup"><span data-stu-id="853d1-119">Complex transactions.</span></span>
- <span data-ttu-id="853d1-120">一次處理數個實體。</span><span class="sxs-lookup"><span data-stu-id="853d1-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="853d1-121">允許特定實體屬性的更新。</span><span class="sxs-lookup"><span data-stu-id="853d1-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="853d1-122">傳送不是實體的資料。</span><span class="sxs-lookup"><span data-stu-id="853d1-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="853d1-123">函式可用於直接至實體或集合傳回未對應的資訊。</span><span class="sxs-lookup"><span data-stu-id="853d1-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="853d1-124">動作 （或函式） 可以為單一實體或集合目標。</span><span class="sxs-lookup"><span data-stu-id="853d1-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="853d1-125">這是在 OData 術語*繫結*。</span><span class="sxs-lookup"><span data-stu-id="853d1-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="853d1-126">您也可以&quot;未繫結&quot;動作/函式，名為靜態服務上的作業。</span><span class="sxs-lookup"><span data-stu-id="853d1-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="853d1-127">範例： 加入動作</span><span class="sxs-lookup"><span data-stu-id="853d1-127">Example: Adding an Action</span></span>

<span data-ttu-id="853d1-128">讓我們定義產品評分的動作。</span><span class="sxs-lookup"><span data-stu-id="853d1-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="853d1-129">本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="853d1-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="853d1-130">首先，新增`ProductRating`來表示評等模型。</span><span class="sxs-lookup"><span data-stu-id="853d1-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="853d1-131">也加入**DbSet**到`ProductsContext`類別，因此，EF 會建立在資料庫中的評等資料表。</span><span class="sxs-lookup"><span data-stu-id="853d1-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="853d1-132">將動作加入 EDM</span><span class="sxs-lookup"><span data-stu-id="853d1-132">Add the Action to the EDM</span></span>

<span data-ttu-id="853d1-133">在 WebApiConfig.cs 中，新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="853d1-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="853d1-134">**EntityTypeConfiguration.Action**方法會將 entity data model (EDM) 中的動作。</span><span class="sxs-lookup"><span data-stu-id="853d1-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="853d1-135">**參數**方法指定動作的具型別的參數。</span><span class="sxs-lookup"><span data-stu-id="853d1-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="853d1-136">此程式碼也會設定為 EDM 命名空間。</span><span class="sxs-lookup"><span data-stu-id="853d1-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="853d1-137">命名空間很重要，因為動作 URI 包含完整的動作名稱：</span><span class="sxs-lookup"><span data-stu-id="853d1-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="853d1-138">在典型的 IIS 設定中，此 URL 中的句點會導致 IIS 傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="853d1-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="853d1-139">您可以藉由將您的 Web.Config 檔案中的下一節來解決此問題：</span><span class="sxs-lookup"><span data-stu-id="853d1-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="853d1-140">新增動作的控制器方法</span><span class="sxs-lookup"><span data-stu-id="853d1-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="853d1-141">若要啟用&quot;速率&quot;動作，將下列方法加入`ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="853d1-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="853d1-142">請注意方法名稱比對的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="853d1-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="853d1-143">**[HttpPost]** 屬性可讓您指定的方法是 HTTP POST 方法。</span><span class="sxs-lookup"><span data-stu-id="853d1-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="853d1-144">若要叫用動作，用戶端會傳送 HTTP POST 要求，如下所示：</span><span class="sxs-lookup"><span data-stu-id="853d1-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="853d1-145">&quot;速率&quot;動作繫結到 Product 執行個體，因此動作 URI 是附加至實體 URI 的完整動作名稱。</span><span class="sxs-lookup"><span data-stu-id="853d1-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="853d1-146">(您應該記得我們將 EDM 命名空間設定為&quot;ProductService&quot;，因此，完整的動作名稱&quot;ProductService.Rate&quot;。)</span><span class="sxs-lookup"><span data-stu-id="853d1-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="853d1-147">要求的主體會包含動作參數作為 JSON 承載。</span><span class="sxs-lookup"><span data-stu-id="853d1-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="853d1-148">Web API 會自動將轉換的 JSON 承載，以**ODataActionParameters**物件，也就是只是參數值的字典。</span><span class="sxs-lookup"><span data-stu-id="853d1-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="853d1-149">您可以使用此字典，存取控制器方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="853d1-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="853d1-150">如果用戶端傳送錯誤的動作參數，值格式化**ModelState.IsValid**為 false。</span><span class="sxs-lookup"><span data-stu-id="853d1-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="853d1-151">檢查此旗標，在您的控制器方法中，並傳回錯誤，如果**IsValid**為 false。</span><span class="sxs-lookup"><span data-stu-id="853d1-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="853d1-152">範例： 加入函式</span><span class="sxs-lookup"><span data-stu-id="853d1-152">Example: Adding a Function</span></span>

<span data-ttu-id="853d1-153">現在讓我們新增 OData 函式會傳回最便宜的產品。</span><span class="sxs-lookup"><span data-stu-id="853d1-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="853d1-154">如之前，第一個步驟加入至 EDM 的函式。</span><span class="sxs-lookup"><span data-stu-id="853d1-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="853d1-155">在 WebApiConfig.cs 中，新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="853d1-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="853d1-156">在此情況下，函式繫結至產品集合，而不是個別產品執行個體。</span><span class="sxs-lookup"><span data-stu-id="853d1-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="853d1-157">用戶端叫用函式，藉由傳送 GET 要求：</span><span class="sxs-lookup"><span data-stu-id="853d1-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="853d1-158">以下是此函式的控制器方法：</span><span class="sxs-lookup"><span data-stu-id="853d1-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="853d1-159">請注意方法名稱的函數名稱相符。</span><span class="sxs-lookup"><span data-stu-id="853d1-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="853d1-160">**[HttpGet]** 屬性可讓您指定的方法是 HTTP GET 方法。</span><span class="sxs-lookup"><span data-stu-id="853d1-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="853d1-161">以下是 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="853d1-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="853d1-162">範例： 加入未繫結的函式</span><span class="sxs-lookup"><span data-stu-id="853d1-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="853d1-163">前一個範例是繫結至集合的函式。</span><span class="sxs-lookup"><span data-stu-id="853d1-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="853d1-164">在下一個範例中，我們將建立*未繫結*函式。</span><span class="sxs-lookup"><span data-stu-id="853d1-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="853d1-165">作為服務上的靜態作業呼叫未繫結的函式。</span><span class="sxs-lookup"><span data-stu-id="853d1-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="853d1-166">在此範例中的函式會傳回指定的郵遞區號的營業稅。</span><span class="sxs-lookup"><span data-stu-id="853d1-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="853d1-167">在 WebApiConfig 檔案中，加入至 EDM 的函式：</span><span class="sxs-lookup"><span data-stu-id="853d1-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="853d1-168">請注意，我們會撥打**函式**直接依據**ODataModelBuilder**，而不是實體類型或集合。</span><span class="sxs-lookup"><span data-stu-id="853d1-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="853d1-169">這會告知模型產生器函式是未繫結。</span><span class="sxs-lookup"><span data-stu-id="853d1-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="853d1-170">以下是實作函式的控制器方法：</span><span class="sxs-lookup"><span data-stu-id="853d1-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="853d1-171">不論您放置在這個方法的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="853d1-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="853d1-172">您可以將它放`ProductsController`，或定義個別的控制器。</span><span class="sxs-lookup"><span data-stu-id="853d1-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="853d1-173">**[ODataRoute]** 屬性定義的函式的 URI 範本。</span><span class="sxs-lookup"><span data-stu-id="853d1-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="853d1-174">以下是範例用戶端要求：</span><span class="sxs-lookup"><span data-stu-id="853d1-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="853d1-175">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="853d1-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
