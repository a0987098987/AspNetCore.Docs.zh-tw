---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "ASP.NET Web API 中的繫結參數 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="e1221-102">ASP.NET Web API 中的繫結的參數</span><span class="sxs-lookup"><span data-stu-id="e1221-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e1221-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e1221-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e1221-104">當 Web 應用程式開發介面的控制站上呼叫方法時，它必須設定參數的值，稱為程序*繫結*。</span><span class="sxs-lookup"><span data-stu-id="e1221-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="e1221-105">本文說明如何 Web API 繫結參數，以及如何自訂繫結程序。</span><span class="sxs-lookup"><span data-stu-id="e1221-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="e1221-106">根據預設，Web API 會使用下列規則來繫結參數：</span><span class="sxs-lookup"><span data-stu-id="e1221-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="e1221-107">如果參數是 「 簡單 」 類型，Web API 會嘗試從 URI 取得的值。</span><span class="sxs-lookup"><span data-stu-id="e1221-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="e1221-108">簡單類型包括.NET[基本型別](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**， **bool**， **double**，依此類推)，加上**TimeSpan**， **DateTime**， **Guid**，**十進位**，和**字串**，*加上*任何類型與類型轉換器可以從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="e1221-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="e1221-109">（深入了解稍後型別轉換子。）</span><span class="sxs-lookup"><span data-stu-id="e1221-109">(More about type converters later.)</span></span>
- <span data-ttu-id="e1221-110">針對複雜型別，Web API 會嘗試從訊息本文讀取的值，使用[媒體類型格式器](media-formatters.md)。</span><span class="sxs-lookup"><span data-stu-id="e1221-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="e1221-111">例如，以下是典型的 Web API 控制器方法：</span><span class="sxs-lookup"><span data-stu-id="e1221-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e1221-112">*識別碼*參數是&quot;簡單&quot;類型，因此 Web API 會嘗試取得要求 URI 中的值。</span><span class="sxs-lookup"><span data-stu-id="e1221-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="e1221-113">*項目*參數是複雜類型，因此 Web 應用程式開發介面使用的媒體類型格式器來讀取的要求主體中的值。</span><span class="sxs-lookup"><span data-stu-id="e1221-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="e1221-114">若要從 URI 取得值，Web API 會查詢的路由資料和 URI 查詢字串。</span><span class="sxs-lookup"><span data-stu-id="e1221-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="e1221-115">當路由系統剖析 URI 和比對路由，則會填入的路由資料。</span><span class="sxs-lookup"><span data-stu-id="e1221-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="e1221-116">如需詳細資訊，請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="e1221-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="e1221-117">在本文的其餘部分，我將示範如何自訂模型繫結程序。</span><span class="sxs-lookup"><span data-stu-id="e1221-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="e1221-118">複雜型別，不過，請考慮使用盡可能的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="e1221-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="e1221-119">HTTP 主要原則就是在訊息主體中，使用內容交涉，以指定資源的表示法傳送的資源。</span><span class="sxs-lookup"><span data-stu-id="e1221-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="e1221-120">針對完全此用途所設計的媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="e1221-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="e1221-121">使用 [FromUri]</span><span class="sxs-lookup"><span data-stu-id="e1221-121">Using [FromUri]</span></span>

<span data-ttu-id="e1221-122">若要強制 Web API 來讀取從 URI 的複雜類型，新增**[FromUri]**屬性的參數。</span><span class="sxs-lookup"><span data-stu-id="e1221-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="e1221-123">下列範例會定義`GeoPoint`型別，以及控制站方法，以取得`GeoPoint`從 URI。</span><span class="sxs-lookup"><span data-stu-id="e1221-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e1221-124">用戶端可以將經度和緯度值放在查詢字串和 Web API 會使用它們來建構`GeoPoint`。</span><span class="sxs-lookup"><span data-stu-id="e1221-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="e1221-125">例如: </span><span class="sxs-lookup"><span data-stu-id="e1221-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="e1221-126">使用 [FromBody]</span><span class="sxs-lookup"><span data-stu-id="e1221-126">Using [FromBody]</span></span>

<span data-ttu-id="e1221-127">若要強制 Web API 來讀取的要求主體中的簡單類型，新增**[FromBody]**屬性的參數：</span><span class="sxs-lookup"><span data-stu-id="e1221-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e1221-128">在此範例中，Web 應用程式開發介面將使用的媒體類型格式器來讀取的值*名稱*從要求主體。</span><span class="sxs-lookup"><span data-stu-id="e1221-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="e1221-129">以下是範例用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="e1221-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="e1221-130">[FromBody] 參數時，Web API 所使用的內容類型標頭來選取格式器。</span><span class="sxs-lookup"><span data-stu-id="e1221-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="e1221-131">在此範例中，內容類型是&quot;應用程式 /json&quot;和要求主體是未經處理的 JSON 字串 （而不是 JSON 物件）。</span><span class="sxs-lookup"><span data-stu-id="e1221-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="e1221-132">最多一個參數可以讀取訊息本文。</span><span class="sxs-lookup"><span data-stu-id="e1221-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="e1221-133">因此這無法運作：</span><span class="sxs-lookup"><span data-stu-id="e1221-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e1221-134">此規則的原因是可能只讀取一次的非緩衝處理資料流中儲存的要求主體。</span><span class="sxs-lookup"><span data-stu-id="e1221-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="e1221-135">類型轉換器</span><span class="sxs-lookup"><span data-stu-id="e1221-135">Type Converters</span></span>

<span data-ttu-id="e1221-136">您可以讓 （以便 Web API 將會嘗試將它從 URI 繫結），將類別視為簡單類型的 Web API 建立**TypeConverter**並提供字串轉換。</span><span class="sxs-lookup"><span data-stu-id="e1221-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="e1221-137">下列程式碼會示範`GeoPoint`類別，表示地理位置的點，再加上**TypeConverter**將從字串轉換`GeoPoint`執行個體。</span><span class="sxs-lookup"><span data-stu-id="e1221-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="e1221-138">`GeoPoint`類別以裝飾**[TypeConverter]**屬性來指定型別轉換子。</span><span class="sxs-lookup"><span data-stu-id="e1221-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="e1221-139">(這個範例啟發的 Mike 拖延部落格文章[如何繫結至 MVC/WebAPI 中的動作簽章中的自訂物件](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)。)</span><span class="sxs-lookup"><span data-stu-id="e1221-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e1221-140">現在會將 Web API`GeoPoint`為簡單類型，這表示它將會嘗試繫結`GeoPoint`從 URI 的參數。</span><span class="sxs-lookup"><span data-stu-id="e1221-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="e1221-141">您不需要包含**[FromUri]**參數上。</span><span class="sxs-lookup"><span data-stu-id="e1221-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="e1221-142">用戶端可以叫用的方法就像這樣的 uri:</span><span class="sxs-lookup"><span data-stu-id="e1221-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="e1221-143">模型繫結器</span><span class="sxs-lookup"><span data-stu-id="e1221-143">Model Binders</span></span>

<span data-ttu-id="e1221-144">型別轉換子比更有彈性的選項是建立自訂的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="e1221-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="e1221-145">使用模型繫結，您必須存取 HTTP 要求、 動作描述，和原始值從路由資料。</span><span class="sxs-lookup"><span data-stu-id="e1221-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="e1221-146">若要建立的模型繫結器，實作**IModelBinder**介面。</span><span class="sxs-lookup"><span data-stu-id="e1221-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="e1221-147">這個介面會定義單一方法**BindModel**:</span><span class="sxs-lookup"><span data-stu-id="e1221-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="e1221-148">以下是模型繫結器`GeoPoint`物件。</span><span class="sxs-lookup"><span data-stu-id="e1221-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="e1221-149">模型繫結取得未經處理的輸入的值，從*值提供者*。</span><span class="sxs-lookup"><span data-stu-id="e1221-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="e1221-150">這項設計會分隔兩個不同的函式：</span><span class="sxs-lookup"><span data-stu-id="e1221-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="e1221-151">值提供者會收到 HTTP 要求，並於其中填入索引鍵-值組的字典。</span><span class="sxs-lookup"><span data-stu-id="e1221-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="e1221-152">模型繫結器會使用此字典來擴展模型。</span><span class="sxs-lookup"><span data-stu-id="e1221-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="e1221-153">Web API 中的預設值提供者從路由資料和查詢字串取得值。</span><span class="sxs-lookup"><span data-stu-id="e1221-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="e1221-154">例如，如果 URI 是`http://localhost/api/values/1?location=48,-122`，值提供者會建立下列機碼值組：</span><span class="sxs-lookup"><span data-stu-id="e1221-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="e1221-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="e1221-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="e1221-156">位置 = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="e1221-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="e1221-157">(假設是預設路由範本&quot;api / {controller} / {id}&quot;。)</span><span class="sxs-lookup"><span data-stu-id="e1221-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="e1221-158">若要繫結參數的名稱儲存在**ModelBindingContext.ModelName**屬性。</span><span class="sxs-lookup"><span data-stu-id="e1221-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="e1221-159">模型繫結器會尋找此字典中值的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e1221-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="e1221-160">如果值存在，而且可以轉換成`GeoPoint`，模型繫結器繫結將值指派至**ModelBindingContext.Model**屬性。</span><span class="sxs-lookup"><span data-stu-id="e1221-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="e1221-161">請注意，模型繫結器不限制為簡單的型別轉換。</span><span class="sxs-lookup"><span data-stu-id="e1221-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="e1221-162">在此範例中，模型繫結器會先尋找已知的位置，資料表中，如果失敗，它會使用型別轉換。</span><span class="sxs-lookup"><span data-stu-id="e1221-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="e1221-163">**設定模型繫結器**</span><span class="sxs-lookup"><span data-stu-id="e1221-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="e1221-164">有幾種方式來設定模型繫結。</span><span class="sxs-lookup"><span data-stu-id="e1221-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="e1221-165">首先，您可以加入**[ModelBinder]**屬性的參數。</span><span class="sxs-lookup"><span data-stu-id="e1221-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="e1221-166">您也可以加入**[ModelBinder]**屬性加入型別。</span><span class="sxs-lookup"><span data-stu-id="e1221-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="e1221-167">Web API 將會使用該類型的所有參數指定的模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="e1221-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="e1221-168">最後，您可以在其中加入模型繫結器提供者**HttpConfiguration**。</span><span class="sxs-lookup"><span data-stu-id="e1221-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="e1221-169">模型繫結器提供者是只建立的模型繫結器 factory 類別。</span><span class="sxs-lookup"><span data-stu-id="e1221-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="e1221-170">您可以建立提供者藉由衍生自[ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="e1221-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="e1221-171">不過，如果您的模型繫結器可以處理單一類型，它很容易使用內建**SimpleModelBinderProvider**，它設計為此目的。</span><span class="sxs-lookup"><span data-stu-id="e1221-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="e1221-172">下列程式碼示範如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="e1221-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="e1221-173">與模型繫結的提供者，您仍需要加入**[ModelBinder]**屬性的參數，告知它應該使用模型繫結和不媒體類型格式器的 Web API。</span><span class="sxs-lookup"><span data-stu-id="e1221-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="e1221-174">但是，現在您不需要在屬性中指定的模型繫結器類型：</span><span class="sxs-lookup"><span data-stu-id="e1221-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="e1221-175">值提供者</span><span class="sxs-lookup"><span data-stu-id="e1221-175">Value Providers</span></span>

<span data-ttu-id="e1221-176">模型繫結取得值，來自值提供者所述。</span><span class="sxs-lookup"><span data-stu-id="e1221-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="e1221-177">若要撰寫的自訂值提供者，實作**IValueProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="e1221-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="e1221-178">提取要求中的 cookie 中的值範例如下：</span><span class="sxs-lookup"><span data-stu-id="e1221-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="e1221-179">您也必須由衍生自建立值提供者 factory **ValueProviderFactory**類別。</span><span class="sxs-lookup"><span data-stu-id="e1221-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="e1221-180">加入至值提供者 factory **HttpConfiguration** ，如下所示。</span><span class="sxs-lookup"><span data-stu-id="e1221-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="e1221-181">Web 應用程式開發介面撰寫所有值提供者，因此當模型繫結呼叫**ValueProvider.GetValue**，模型繫結器會從第一個能夠產生它的值提供者收到的值。</span><span class="sxs-lookup"><span data-stu-id="e1221-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="e1221-182">或者，使用參數層級設定值提供者 factory **ValueProvider**屬性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e1221-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="e1221-183">這會告知模型繫結使用指定的值提供者處理站，且不使用任何其他已註冊的值提供者的 Web API。</span><span class="sxs-lookup"><span data-stu-id="e1221-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="e1221-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="e1221-184">HttpParameterBinding</span></span>

<span data-ttu-id="e1221-185">模型繫結器的較通用的機制的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="e1221-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="e1221-186">如果您看一下**[ModelBinder]**屬性，您會看到它衍生自抽象**ParameterBindingAttribute**類別。</span><span class="sxs-lookup"><span data-stu-id="e1221-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="e1221-187">這個類別會定義單一方法**GetBinding**，它會傳回**HttpParameterBinding**物件：</span><span class="sxs-lookup"><span data-stu-id="e1221-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="e1221-188">**HttpParameterBinding**會負責將參數繫結的值。</span><span class="sxs-lookup"><span data-stu-id="e1221-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="e1221-189">如果是**[ModelBinder]**，屬性會傳回**HttpParameterBinding**使用實作**IModelBinder**執行實際的繫結。</span><span class="sxs-lookup"><span data-stu-id="e1221-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="e1221-190">您也可以實作您自己**HttpParameterBinding**。</span><span class="sxs-lookup"><span data-stu-id="e1221-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="e1221-191">例如，假設您想要取得從 Etag`if-match`和`if-none-match`要求中的標頭。</span><span class="sxs-lookup"><span data-stu-id="e1221-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="e1221-192">我們會先定義的類別來表示的 Etag。</span><span class="sxs-lookup"><span data-stu-id="e1221-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="e1221-193">我們也會定義列舉，指出要取得所產生的 ETag`if-match`標頭或`if-none-match`標頭。</span><span class="sxs-lookup"><span data-stu-id="e1221-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="e1221-194">以下是**HttpParameterBinding** ，取得所需的標頭的 ETag，並將它繫結至類型 ETag 的參數：</span><span class="sxs-lookup"><span data-stu-id="e1221-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="e1221-195">**ExecuteBindingAsync**方法中會繫結。</span><span class="sxs-lookup"><span data-stu-id="e1221-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="e1221-196">在此方法中，加入繫結的參數值**ActionArgument**字典中的**HttpActionContext**。</span><span class="sxs-lookup"><span data-stu-id="e1221-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="e1221-197">如果您**ExecuteBindingAsync**方法會讀取要求訊息的本文，請覆寫**WillReadBody**屬性傳回 true。</span><span class="sxs-lookup"><span data-stu-id="e1221-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="e1221-198">要求主體可能只能讀取一次，讓 Web 應用程式開發介面會強制執行規則的最多只有一個繫結的緩衝資料流可以讀取訊息本文。</span><span class="sxs-lookup"><span data-stu-id="e1221-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="e1221-199">若要套用自訂**HttpParameterBinding**，您可以定義衍生自屬性**ParameterBindingAttribute**。</span><span class="sxs-lookup"><span data-stu-id="e1221-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="e1221-200">如`ETagParameterBinding`，我們會定義兩個屬性，一個用於`if-match`標頭，另一個用於`if-none-match`標頭。</span><span class="sxs-lookup"><span data-stu-id="e1221-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="e1221-201">兩者都衍生自抽象基底類別。</span><span class="sxs-lookup"><span data-stu-id="e1221-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="e1221-202">以下是使用控制器方法`[IfNoneMatch]`屬性。</span><span class="sxs-lookup"><span data-stu-id="e1221-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="e1221-203">除了**ParameterBindingAttribute**，還有另一個勾點加入自訂**HttpParameterBinding**。</span><span class="sxs-lookup"><span data-stu-id="e1221-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="e1221-204">在**HttpConfiguration**物件**ParameterBindingRules**屬性為 anomymous 函式型別的集合 (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。</span><span class="sxs-lookup"><span data-stu-id="e1221-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="e1221-205">例如，您可以加入任何 ETag 上的參數 GET 方法使用的規則`ETagParameterBinding`與`if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="e1221-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="e1221-206">函式應傳回`null`參數的繫結不適用。</span><span class="sxs-lookup"><span data-stu-id="e1221-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="e1221-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="e1221-207">IActionValueBinder</span></span>

<span data-ttu-id="e1221-208">整個參數繫結程序由隨插即用服務， **IActionValueBinder**。</span><span class="sxs-lookup"><span data-stu-id="e1221-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="e1221-209">預設實作**IActionValueBinder**會進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e1221-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="e1221-210">尋找**ParameterBindingAttribute**參數上。</span><span class="sxs-lookup"><span data-stu-id="e1221-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="e1221-211">這包括**[FromBody]**， **[FromUri]**，和**[ModelBinder]**，或自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="e1221-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="e1221-212">否則，請查看**HttpConfiguration.ParameterBindingRules**函式會傳回非 null **HttpParameterBinding**。</span><span class="sxs-lookup"><span data-stu-id="e1221-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="e1221-213">否則，請使用先前所述的預設規則。</span><span class="sxs-lookup"><span data-stu-id="e1221-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="e1221-214">如果參數類型為 「 簡單 」，或是從 URI 繫結的型別轉換子。</span><span class="sxs-lookup"><span data-stu-id="e1221-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="e1221-215">這相當於將**[FromUri]**參數上的屬性。</span><span class="sxs-lookup"><span data-stu-id="e1221-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="e1221-216">否則，嘗試從訊息本文讀取參數。</span><span class="sxs-lookup"><span data-stu-id="e1221-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="e1221-217">這相當於將**[FromBody]**參數上。</span><span class="sxs-lookup"><span data-stu-id="e1221-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="e1221-218">如果您想，您可以取代整個**IActionValueBinder**服務的自訂實作。</span><span class="sxs-lookup"><span data-stu-id="e1221-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1221-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="e1221-219">Additional Resources</span></span>

[<span data-ttu-id="e1221-220">自訂參數繫結範例</span><span class="sxs-lookup"><span data-stu-id="e1221-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="e1221-221">Mike 拖延提到 Web 應用程式開發介面參數繫結的良好的一連串的部落格文章：</span><span class="sxs-lookup"><span data-stu-id="e1221-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="e1221-222">Web 應用程式開發介面如何執行參數繫結</span><span class="sxs-lookup"><span data-stu-id="e1221-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="e1221-223">Web API 的 MVC 樣式參數繫結</span><span class="sxs-lookup"><span data-stu-id="e1221-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="e1221-224">如何繫結至 MVC/Web API 中的動作簽章中的自訂物件</span><span class="sxs-lookup"><span data-stu-id="e1221-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="e1221-225">如何在 Web 應用程式開發介面中建立的自訂值提供者</span><span class="sxs-lookup"><span data-stu-id="e1221-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="e1221-226">背後原理的 web 應用程式開發介面參數繫結</span><span class="sxs-lookup"><span data-stu-id="e1221-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
