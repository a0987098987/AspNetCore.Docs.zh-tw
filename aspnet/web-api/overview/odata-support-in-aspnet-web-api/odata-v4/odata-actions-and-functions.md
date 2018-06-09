---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: 動作和函數中 OData v4 使用 ASP.NET Web API 2.2 |Microsoft 文件
author: MikeWasson
description: 在 OData 中，動作和函數是加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。 本教學課程示範如何...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508227"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="77abf-104">動作和函數中 OData v4 使用 ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="77abf-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="77abf-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="77abf-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="77abf-106">在 OData 中，動作和函數是加入伺服器端行為，不會輕易地定義為實體上的 CRUD 作業的方式。</span><span class="sxs-lookup"><span data-stu-id="77abf-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="77abf-107">本教學課程會示範如何將動作和函數加入至 OData v4 端點，使用 Web 應用程式開發介面 2.2。</span><span class="sxs-lookup"><span data-stu-id="77abf-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="77abf-108">本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="77abf-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="77abf-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="77abf-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="77abf-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="77abf-110">Web API 2.2</span></span>
> - <span data-ttu-id="77abf-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="77abf-111">OData v4</span></span>
> - [<span data-ttu-id="77abf-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="77abf-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="77abf-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="77abf-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="77abf-114">教學課程版本</span><span class="sxs-lookup"><span data-stu-id="77abf-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="77abf-115">適用於 OData 版本 3，查看[中 ASP.NET Web API 2 OData 動作](../odata-v3/odata-actions.md)。</span><span class="sxs-lookup"><span data-stu-id="77abf-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="77abf-116">之間的差異*動作*和*函式*是動作可能有副作用，而且函式不相符。</span><span class="sxs-lookup"><span data-stu-id="77abf-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="77abf-117">動作和函數可以傳回資料。</span><span class="sxs-lookup"><span data-stu-id="77abf-117">Both actions and functions can return data.</span></span> <span data-ttu-id="77abf-118">動作的一些用法包括：</span><span class="sxs-lookup"><span data-stu-id="77abf-118">Some uses for actions include:</span></span>

- <span data-ttu-id="77abf-119">複雜的交易。</span><span class="sxs-lookup"><span data-stu-id="77abf-119">Complex transactions.</span></span>
- <span data-ttu-id="77abf-120">同時管理數個項目。</span><span class="sxs-lookup"><span data-stu-id="77abf-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="77abf-121">允許特定實體屬性的更新。</span><span class="sxs-lookup"><span data-stu-id="77abf-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="77abf-122">傳送不是實體的資料。</span><span class="sxs-lookup"><span data-stu-id="77abf-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="77abf-123">函式可用於直接至實體或集合傳回未對應的資訊。</span><span class="sxs-lookup"><span data-stu-id="77abf-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="77abf-124">動作 （或函式） 可以針對單一實體或集合。</span><span class="sxs-lookup"><span data-stu-id="77abf-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="77abf-125">在 OData 術語中，這是*繫結*。</span><span class="sxs-lookup"><span data-stu-id="77abf-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="77abf-126">您也可以讓&quot;未繫結&quot;動作/函式，它們被稱為靜態服務上的作業。</span><span class="sxs-lookup"><span data-stu-id="77abf-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="77abf-127">範例： 新增動作</span><span class="sxs-lookup"><span data-stu-id="77abf-127">Example: Adding an Action</span></span>

<span data-ttu-id="77abf-128">讓我們來定義要評等產品的動作。</span><span class="sxs-lookup"><span data-stu-id="77abf-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="77abf-129">本教學課程是本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="77abf-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="77abf-130">首先，新增`ProductRating`模型來表示評等。</span><span class="sxs-lookup"><span data-stu-id="77abf-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="77abf-131">也新增**DbSet**至`ProductsContext`類別，以便 EF 會在資料庫中建立的評等資料表。</span><span class="sxs-lookup"><span data-stu-id="77abf-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="77abf-132">新增 EDM 的動作</span><span class="sxs-lookup"><span data-stu-id="77abf-132">Add the Action to the EDM</span></span>

<span data-ttu-id="77abf-133">在 WebApiConfig.cs，加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="77abf-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="77abf-134">**EntityTypeConfiguration.Action**方法會將動作加入至實體資料模型 (EDM)。</span><span class="sxs-lookup"><span data-stu-id="77abf-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="77abf-135">**參數**方法指定型別的參數的動作。</span><span class="sxs-lookup"><span data-stu-id="77abf-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="77abf-136">這個程式碼也會設定命名空間為 EDM。</span><span class="sxs-lookup"><span data-stu-id="77abf-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="77abf-137">命名空間很重要，因為動作 URI 包含完整的動作名稱：</span><span class="sxs-lookup"><span data-stu-id="77abf-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="77abf-138">在一般的 IIS 設定中，此 URL 中的句點會導致 IIS 傳回錯誤 404。</span><span class="sxs-lookup"><span data-stu-id="77abf-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="77abf-139">您可以將下列區段加入至 Web.Config 檔案來解決此問題：</span><span class="sxs-lookup"><span data-stu-id="77abf-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="77abf-140">加入控制器方法的動作</span><span class="sxs-lookup"><span data-stu-id="77abf-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="77abf-141">若要啟用&quot;速率&quot;動作，將下列方法加入`ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="77abf-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="77abf-142">請注意方法名稱比對的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="77abf-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="77abf-143">**[HttpPost]** 屬性會指定方法為 HTTP POST 方法。</span><span class="sxs-lookup"><span data-stu-id="77abf-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="77abf-144">若要叫用動作時，用戶端會傳送 HTTP POST 要求，如下所示：</span><span class="sxs-lookup"><span data-stu-id="77abf-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="77abf-145">&quot;速率&quot;動作繫結至產品執行個體，因此動作 URI 是附加至實體 URI 的完整動作名稱。</span><span class="sxs-lookup"><span data-stu-id="77abf-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="77abf-146">(請記得我們設定的 EDM 命名空間為&quot;ProductService&quot;讓完整的動作名稱、 &quot;ProductService.Rate&quot;。)</span><span class="sxs-lookup"><span data-stu-id="77abf-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="77abf-147">要求的主體會包含動作參數做為 JSON 裝載。</span><span class="sxs-lookup"><span data-stu-id="77abf-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="77abf-148">Web 應用程式開發介面會自動將轉換的 JSON 裝載**ODataActionParameters**物件，它是只參數值的字典。</span><span class="sxs-lookup"><span data-stu-id="77abf-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="77abf-149">您可以使用此字典來存取控制站方法中的參數。</span><span class="sxs-lookup"><span data-stu-id="77abf-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="77abf-150">如果用戶端傳送的 action 參數中的錯誤，將值格式化的**ModelState.IsValid**為 false。</span><span class="sxs-lookup"><span data-stu-id="77abf-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="77abf-151">請檢查控制器方法中的這個旗標，並傳回錯誤**IsValid**為 false。</span><span class="sxs-lookup"><span data-stu-id="77abf-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="77abf-152">範例： 加入函式</span><span class="sxs-lookup"><span data-stu-id="77abf-152">Example: Adding a Function</span></span>

<span data-ttu-id="77abf-153">現在讓我們加入的 OData 函數會傳回成本最高的產品。</span><span class="sxs-lookup"><span data-stu-id="77abf-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="77abf-154">如之前，第一個步驟新增 EDM 函式。</span><span class="sxs-lookup"><span data-stu-id="77abf-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="77abf-155">在 WebApiConfig.cs，加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="77abf-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="77abf-156">在此情況下，函式繫結至產品集合中，而不是個別產品的執行個體。</span><span class="sxs-lookup"><span data-stu-id="77abf-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="77abf-157">用戶端傳送 GET 要求來叫用函式：</span><span class="sxs-lookup"><span data-stu-id="77abf-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="77abf-158">以下是這個函式的控制站方法：</span><span class="sxs-lookup"><span data-stu-id="77abf-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="77abf-159">請注意方法名稱的函數名稱相符。</span><span class="sxs-lookup"><span data-stu-id="77abf-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="77abf-160">**[HttpGet]** 屬性會指定方法是 HTTP GET 的方法。</span><span class="sxs-lookup"><span data-stu-id="77abf-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="77abf-161">以下是 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="77abf-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="77abf-162">範例： 加入未繫結的函數</span><span class="sxs-lookup"><span data-stu-id="77abf-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="77abf-163">前一個範例是繫結至集合的函式。</span><span class="sxs-lookup"><span data-stu-id="77abf-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="77abf-164">在下一個範例中，我們將建立*未繫結*函式。</span><span class="sxs-lookup"><span data-stu-id="77abf-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="77abf-165">未繫結函式稱為做為靜態服務上的作業。</span><span class="sxs-lookup"><span data-stu-id="77abf-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="77abf-166">在此範例中的函式會傳回給定的郵遞區號營業稅。</span><span class="sxs-lookup"><span data-stu-id="77abf-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="77abf-167">在到 WebApiConfig 檔案中，加入 EDM 中的函式：</span><span class="sxs-lookup"><span data-stu-id="77abf-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="77abf-168">請注意，我們正在撥打**函式**直接依據**ODataModelBuilder**，而不是集合的實體類型。</span><span class="sxs-lookup"><span data-stu-id="77abf-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="77abf-169">這會告知模型產生器函式是未繫結。</span><span class="sxs-lookup"><span data-stu-id="77abf-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="77abf-170">以下是實作的函式的控制站方法：</span><span class="sxs-lookup"><span data-stu-id="77abf-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="77abf-171">並不重要的 Web API 控制器會放置在這個方法。</span><span class="sxs-lookup"><span data-stu-id="77abf-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="77abf-172">您無法將其置於`ProductsController`，或定義不同的控制站。</span><span class="sxs-lookup"><span data-stu-id="77abf-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="77abf-173">**[ODataRoute]** 屬性定義的函式的 URI 範本。</span><span class="sxs-lookup"><span data-stu-id="77abf-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="77abf-174">以下是用戶端的要求範例：</span><span class="sxs-lookup"><span data-stu-id="77abf-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="77abf-175">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="77abf-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
