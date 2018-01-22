---
title: "模型繫結"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 84b9c5dc3a87b739affaeaecaa180d1b01f49b8e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="model-binding"></a><span data-ttu-id="f826a-102">模型繫結</span><span class="sxs-lookup"><span data-stu-id="f826a-102">Model Binding</span></span>

<span data-ttu-id="f826a-103">由[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f826a-103">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-binding"></a><span data-ttu-id="f826a-104">模型繫結的簡介</span><span class="sxs-lookup"><span data-stu-id="f826a-104">Introduction to model binding</span></span>

<span data-ttu-id="f826a-105">在 ASP.NET Core MVC 模型繫結會對應至動作方法參數的資料從 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="f826a-105">Model binding in ASP.NET Core MVC maps data from HTTP requests to action method parameters.</span></span> <span data-ttu-id="f826a-106">參數可能是簡單類型，例如字串、 整數或浮點數，或者可能是複雜類型。</span><span class="sxs-lookup"><span data-stu-id="f826a-106">The parameters may be simple types such as strings, integers, or floats, or they may be complex types.</span></span> <span data-ttu-id="f826a-107">這是 MVC 的絕佳功能，因為內送資料對應到對應的是經常重複的情況下，無論資料的大小或複雜度。</span><span class="sxs-lookup"><span data-stu-id="f826a-107">This is a great feature of MVC because mapping incoming data to a counterpart is an often repeated scenario, regardless of size or complexity of the data.</span></span> <span data-ttu-id="f826a-108">MVC 再抽離繫結，因此開發人員不需要保留重寫稍有不同版本的每個應用程式中相同的程式碼，來解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="f826a-108">MVC solves this problem by abstracting binding away so developers don't have to keep rewriting a slightly different version of that same code in every app.</span></span> <span data-ttu-id="f826a-109">撰寫您自己的文字類型轉換子的程式碼過於冗長，而且容易產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f826a-109">Writing your own text to type converter code is tedious, and error prone.</span></span>

## <a name="how-model-binding-works"></a><span data-ttu-id="f826a-110">模型繫結的運作方式</span><span class="sxs-lookup"><span data-stu-id="f826a-110">How model binding works</span></span>

<span data-ttu-id="f826a-111">MVC 收到 HTTP 要求時，它將其路由至特定的動作方法的控制器。</span><span class="sxs-lookup"><span data-stu-id="f826a-111">When MVC receives an HTTP request, it routes it to a specific action method of a controller.</span></span> <span data-ttu-id="f826a-112">它會判斷要根據執行的路由資料中的動作方法，則它將值繫結從 HTTP 要求到該動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="f826a-112">It determines which action method to run based on what is in the route data, then it binds values from the HTTP request to that action method's parameters.</span></span> <span data-ttu-id="f826a-113">例如，請考慮下列 URL:</span><span class="sxs-lookup"><span data-stu-id="f826a-113">For example, consider the following URL:</span></span>

`http://contoso.com/movies/edit/2`

<span data-ttu-id="f826a-114">因為路由範本看起來像這樣， `{controller=Home}/{action=Index}/{id?}`，`movies/edit/2`會路由傳送至`Movies`控制站，且其`Edit`動作方法。</span><span class="sxs-lookup"><span data-stu-id="f826a-114">Since the route template looks like this, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` routes to the `Movies` controller, and its `Edit` action method.</span></span> <span data-ttu-id="f826a-115">它也可接受選擇性參數呼叫`id`。</span><span class="sxs-lookup"><span data-stu-id="f826a-115">It also accepts an optional parameter called `id`.</span></span> <span data-ttu-id="f826a-116">動作方法的程式碼看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="f826a-116">The code for the action method should look something like this:</span></span>

```csharp
public IActionResult Edit(int? id)
   ```

<span data-ttu-id="f826a-117">注意： 不區分大小寫的字串中的 URL 路由。</span><span class="sxs-lookup"><span data-stu-id="f826a-117">Note: The strings in the URL route are not case sensitive.</span></span>

<span data-ttu-id="f826a-118">MVC 會嘗試要求將資料繫結至動作參數名稱。</span><span class="sxs-lookup"><span data-stu-id="f826a-118">MVC will try to bind request data to the action parameters by name.</span></span> <span data-ttu-id="f826a-119">MVC 會尋找每個參數值使用的參數名稱和公用的可設定屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="f826a-119">MVC will look for values for each parameter using the parameter name and the names of its public settable properties.</span></span> <span data-ttu-id="f826a-120">在上述範例中，唯一的動作參數之所以名為`id`，後者則 MVC 繫結至具有相同名稱的路由值的值。</span><span class="sxs-lookup"><span data-stu-id="f826a-120">In the above example, the only action parameter is named `id`, which MVC binds to the value with the same name in the route values.</span></span> <span data-ttu-id="f826a-121">除了路由值 MVC 將資料從繫結要求的各個部分，而不按照順序。</span><span class="sxs-lookup"><span data-stu-id="f826a-121">In addition to route values MVC will bind data from various parts of the request and it does so in a set order.</span></span> <span data-ttu-id="f826a-122">以下是在模型繫結會透過它們的順序中的資料來源的清單：</span><span class="sxs-lookup"><span data-stu-id="f826a-122">Below is a list of the data sources in the order that model binding looks through them:</span></span>

1. <span data-ttu-id="f826a-123">`Form values`： 這些是在使用 POST 方法的 HTTP 要求的表單值。</span><span class="sxs-lookup"><span data-stu-id="f826a-123">`Form values`: These are form values that go in the HTTP request using the POST method.</span></span> <span data-ttu-id="f826a-124">（包括 jQuery POST 要求）。</span><span class="sxs-lookup"><span data-stu-id="f826a-124">(including jQuery POST requests).</span></span>

2. <span data-ttu-id="f826a-125">`Route values`： 所提供的路由值的集合[路由](../../fundamentals/routing.md)</span><span class="sxs-lookup"><span data-stu-id="f826a-125">`Route values`: The set of route values provided by [Routing](../../fundamentals/routing.md)</span></span>

3. <span data-ttu-id="f826a-126">`Query strings`: URI 查詢字串組件。</span><span class="sxs-lookup"><span data-stu-id="f826a-126">`Query strings`: The query string part of the URI.</span></span>

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

<span data-ttu-id="f826a-127">注意： 表單值、 路由資料和查詢字串會儲存為名稱 / 值組。</span><span class="sxs-lookup"><span data-stu-id="f826a-127">Note: Form values, route data, and query strings are all stored as name-value pairs.</span></span>

<span data-ttu-id="f826a-128">由於模型繫結要求名為`id`任何名為`id`表單值，它進入尋找該索引鍵的路由值。</span><span class="sxs-lookup"><span data-stu-id="f826a-128">Since model binding asked for a key named `id` and there is nothing named `id` in the form values, it moved on to the route values looking for that key.</span></span> <span data-ttu-id="f826a-129">在本例中，相符。</span><span class="sxs-lookup"><span data-stu-id="f826a-129">In our example, it's a match.</span></span> <span data-ttu-id="f826a-130">繫結發生，並將值轉換為 2 的整數。</span><span class="sxs-lookup"><span data-stu-id="f826a-130">Binding happens, and the value is converted to the integer 2.</span></span> <span data-ttu-id="f826a-131">使用 編輯 (字串 id) 相同的要求會將轉換成"2"的字串。</span><span class="sxs-lookup"><span data-stu-id="f826a-131">The same request using Edit(string id) would convert to the string "2".</span></span>

<span data-ttu-id="f826a-132">到目前為止的範例會使用簡單類型。</span><span class="sxs-lookup"><span data-stu-id="f826a-132">So far the example uses simple types.</span></span> <span data-ttu-id="f826a-133">在 MVC 簡單類型會是任何.NET 基本型別或字串型別轉換子的型別。</span><span class="sxs-lookup"><span data-stu-id="f826a-133">In MVC simple types are any .NET primitive type or type with a string type converter.</span></span> <span data-ttu-id="f826a-134">如果動作方法的參數是類別，例如`Movie`型別，其中包含簡單和複雜類型，如同屬性，MVC 的模型繫結仍會妥善處理。</span><span class="sxs-lookup"><span data-stu-id="f826a-134">If the action method's parameter were a class such as the `Movie` type, which contains both simple and complex types as properties, MVC's model binding will still handle it nicely.</span></span> <span data-ttu-id="f826a-135">它會使用反映和遞迴周遊尋找相符的複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-135">It uses reflection and recursion to traverse the properties of complex types looking for matches.</span></span> <span data-ttu-id="f826a-136">模型繫結會尋找模式*parameter_name.property_name*繫結至屬性的值。</span><span class="sxs-lookup"><span data-stu-id="f826a-136">Model binding looks for the pattern *parameter_name.property_name* to bind values to properties.</span></span> <span data-ttu-id="f826a-137">如果找不到相符的值，這種形式的它會嘗試使用屬性名稱進行繫結。</span><span class="sxs-lookup"><span data-stu-id="f826a-137">If it doesn't find matching values of this form, it will attempt to bind using just the property name.</span></span> <span data-ttu-id="f826a-138">針對這些類型，例如`Collection`型別，模型繫結會尋找相符項目*parameter_name [index]*或簡稱*[index]*。</span><span class="sxs-lookup"><span data-stu-id="f826a-138">For those types such as `Collection` types, model binding looks for matches to *parameter_name[index]* or just *[index]*.</span></span> <span data-ttu-id="f826a-139">模型繫結會視為`Dictionary`同樣地，類型要求*parameter_name [key]*或簡稱*[key]*，只要索引鍵是簡單類型。</span><span class="sxs-lookup"><span data-stu-id="f826a-139">Model binding treats  `Dictionary` types similarly, asking for *parameter_name[key]* or just *[key]*, as long as the keys are simple types.</span></span> <span data-ttu-id="f826a-140">支援的索引鍵符合欄位名稱 HTML 和標記協助程式產生相同的模型型別。</span><span class="sxs-lookup"><span data-stu-id="f826a-140">Keys that are supported match the field names HTML and tag helpers generated for the same model type.</span></span> <span data-ttu-id="f826a-141">這可讓反覆存取值，讓表單欄位時，維持填滿與使用者的輸入，其方便起見，比方說，從建立或編輯繫結的資料未通過驗證。</span><span class="sxs-lookup"><span data-stu-id="f826a-141">This enables round-tripping values so that the form fields remain filled with the user's input for their convenience, for example, when bound data from a create or edit did not pass validation.</span></span>

<span data-ttu-id="f826a-142">順序，就可能發生的繫結的類別必須具有公用預設建構函式，並繫結的成員必須是公用的可寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-142">In order for binding to happen the class must have a public default constructor and member to be bound must be public writable properties.</span></span> <span data-ttu-id="f826a-143">當模型繫結發生時，此類別將只使用具現化公用預設建構函式時，可以設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-143">When model binding happens the class will only be instantiated using the public default constructor, then the properties can be set.</span></span>

<span data-ttu-id="f826a-144">參數繫結，模型繫結會停止尋找具有該名稱的值，它會移至下一個參數繫結。</span><span class="sxs-lookup"><span data-stu-id="f826a-144">When a parameter is bound, model binding stops looking for values with that name and it moves on to bind the next parameter.</span></span> <span data-ttu-id="f826a-145">否則，預設模型繫結行為會將參數設為預設值根據其類型而定：</span><span class="sxs-lookup"><span data-stu-id="f826a-145">Otherwise, the default model binding behavior sets parameters to their default values depending on their type:</span></span>

* <span data-ttu-id="f826a-146">`T[]`： 使用的陣列型別的`byte[]`，繫結會設定參數的型別`T[]`至`Array.Empty<T>()`。</span><span class="sxs-lookup"><span data-stu-id="f826a-146">`T[]`: With the exception of arrays of type `byte[]`, binding sets parameters of type `T[]` to `Array.Empty<T>()`.</span></span> <span data-ttu-id="f826a-147">類型的陣列`byte[]`設為`null`。</span><span class="sxs-lookup"><span data-stu-id="f826a-147">Arrays of type `byte[]` are set to `null`.</span></span>

* <span data-ttu-id="f826a-148">參考型別： 繫結建立執行個體類別的預設建構函式未設定屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-148">Reference Types: Binding creates an instance of a class with the default constructor without setting properties.</span></span> <span data-ttu-id="f826a-149">不過，模型繫結設定`string`參數`null`。</span><span class="sxs-lookup"><span data-stu-id="f826a-149">However, model binding sets `string` parameters to `null`.</span></span>

* <span data-ttu-id="f826a-150">可為 null 的類型： 可為 Null 的型別會設定為`null`。</span><span class="sxs-lookup"><span data-stu-id="f826a-150">Nullable Types: Nullable types are set to `null`.</span></span> <span data-ttu-id="f826a-151">在上述範例中，模型繫結設定`id`至`null`因為它是型別`int?`。</span><span class="sxs-lookup"><span data-stu-id="f826a-151">In the above example, model binding sets `id` to `null` since it is of type `int?`.</span></span>

* <span data-ttu-id="f826a-152">實值型別： 不可為 null 值類型的型別`T`設為`default(T)`。</span><span class="sxs-lookup"><span data-stu-id="f826a-152">Value Types: Non-nullable value types of type `T` are set to `default(T)`.</span></span> <span data-ttu-id="f826a-153">例如，模型繫結會設定參數`int id`設為 0。</span><span class="sxs-lookup"><span data-stu-id="f826a-153">For example, model binding will set a parameter `int id` to 0.</span></span> <span data-ttu-id="f826a-154">請考慮使用模型驗證或可為 null 的類型，而不要依賴預設值。</span><span class="sxs-lookup"><span data-stu-id="f826a-154">Consider using model validation or nullable types rather than relying on default values.</span></span>

<span data-ttu-id="f826a-155">如果繫結失敗時，MVC 不會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="f826a-155">If binding fails, MVC does not throw an error.</span></span> <span data-ttu-id="f826a-156">可接受使用者輸入的每一個動作應該檢查`ModelState.IsValid`屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-156">Every action which accepts user input should check the `ModelState.IsValid` property.</span></span>

<span data-ttu-id="f826a-157">注意： 每個項目中的控制站`ModelState`屬性是`ModelStateEntry`包含`Errors`屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-157">Note: Each entry in the controller's `ModelState` property is a `ModelStateEntry` containing an `Errors` property.</span></span> <span data-ttu-id="f826a-158">它是很少會需要自行查詢此集合。</span><span class="sxs-lookup"><span data-stu-id="f826a-158">It's rarely necessary to query this collection yourself.</span></span> <span data-ttu-id="f826a-159">請改用 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="f826a-159">Use `ModelState.IsValid` instead.</span></span>

<span data-ttu-id="f826a-160">此外，還有當 MVC 模型繫結時，必須考量某些特殊的資料類型：</span><span class="sxs-lookup"><span data-stu-id="f826a-160">Additionally, there are some special data types that MVC must consider when performing model binding:</span></span>

* <span data-ttu-id="f826a-161">`IFormFile``IEnumerable<IFormFile>`： 一或多個上傳的檔案的 HTTP 要求的一部分。</span><span class="sxs-lookup"><span data-stu-id="f826a-161">`IFormFile`, `IEnumerable<IFormFile>`: One or more uploaded files that are part of the HTTP request.</span></span>

* <span data-ttu-id="f826a-162">`CancellationToken`： 用來取消非同步控制器中的活動。</span><span class="sxs-lookup"><span data-stu-id="f826a-162">`CancellationToken`: Used to cancel activity in asynchronous controllers.</span></span>

<span data-ttu-id="f826a-163">這些型別可以繫結至動作參數或屬性的類別類型。</span><span class="sxs-lookup"><span data-stu-id="f826a-163">These types can be bound to action parameters or to properties on a class type.</span></span>

<span data-ttu-id="f826a-164">模型繫結完成時，[驗證](validation.md)，就會發生。</span><span class="sxs-lookup"><span data-stu-id="f826a-164">Once model binding is complete, [Validation](validation.md) occurs.</span></span> <span data-ttu-id="f826a-165">預設模型繫結的運作方式很適合用於大部分的開發案例。</span><span class="sxs-lookup"><span data-stu-id="f826a-165">Default model binding works great for the vast majority of development scenarios.</span></span> <span data-ttu-id="f826a-166">它也是可延伸因此如果您有唯一的需求，您可以自訂內建行為。</span><span class="sxs-lookup"><span data-stu-id="f826a-166">It is also extensible so if you have unique needs you can customize the built-in behavior.</span></span>

## <a name="customize-model-binding-behavior-with-attributes"></a><span data-ttu-id="f826a-167">自訂屬性的模型繫結行為</span><span class="sxs-lookup"><span data-stu-id="f826a-167">Customize model binding behavior with attributes</span></span>

<span data-ttu-id="f826a-168">MVC 包含數個屬性可讓您將導向至不同來源的預設模型繫結行為。</span><span class="sxs-lookup"><span data-stu-id="f826a-168">MVC contains several attributes that you can use to direct its default model binding behavior to a different source.</span></span> <span data-ttu-id="f826a-169">例如，您可以指定繫結是否需要屬性，則它應該永遠不會發生完全使用`[BindRequired]`或`[BindNever]`屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-169">For example, you can specify whether binding is required for a property, or if it should never happen at all by using the `[BindRequired]` or `[BindNever]` attributes.</span></span> <span data-ttu-id="f826a-170">或者，您可以覆寫預設的資料來源，並指定模型繫結器的資料來源。</span><span class="sxs-lookup"><span data-stu-id="f826a-170">Alternatively, you can override the default data source, and specify the model binder's data source.</span></span> <span data-ttu-id="f826a-171">以下是一份模型繫結屬性：</span><span class="sxs-lookup"><span data-stu-id="f826a-171">Below is a list of model binding attributes:</span></span>

* <span data-ttu-id="f826a-172">`[BindRequired]`： 如果無法繫結這個屬性會將模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="f826a-172">`[BindRequired]`: This attribute adds a model state error if binding cannot occur.</span></span>

* <span data-ttu-id="f826a-173">`[BindNever]`： 會告知模型繫結器將永遠不會繫結至這個參數。</span><span class="sxs-lookup"><span data-stu-id="f826a-173">`[BindNever]`: Tells the model binder to never bind to this parameter.</span></span>

* <span data-ttu-id="f826a-174">`[FromHeader]``[FromQuery]`， `[FromRoute]`， `[FromForm]`： 用來指定您想要套用的確切繫結來源。</span><span class="sxs-lookup"><span data-stu-id="f826a-174">`[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Use these to specify the exact binding source you want to apply.</span></span>

* <span data-ttu-id="f826a-175">`[FromServices]`： 這個屬性會使用[相依性插入](../../fundamentals/dependency-injection.md)將從服務繫結參數。</span><span class="sxs-lookup"><span data-stu-id="f826a-175">`[FromServices]`: This attribute uses [dependency injection](../../fundamentals/dependency-injection.md) to bind parameters from services.</span></span>

* <span data-ttu-id="f826a-176">`[FromBody]`： 將資料繫結從要求主體中使用設定的格式器。</span><span class="sxs-lookup"><span data-stu-id="f826a-176">`[FromBody]`: Use the configured formatters to bind data from the request body.</span></span> <span data-ttu-id="f826a-177">根據要求的內容類型選取格式器。</span><span class="sxs-lookup"><span data-stu-id="f826a-177">The formatter is selected based on content type of the request.</span></span>

* <span data-ttu-id="f826a-178">`[ModelBinder]`： 用來覆寫預設模型繫結器、 繫結來源和名稱。</span><span class="sxs-lookup"><span data-stu-id="f826a-178">`[ModelBinder]`: Used to override the default model binder, binding source and name.</span></span>

<span data-ttu-id="f826a-179">當您需要覆寫預設行為的模型繫結時，屬性是很有幫助的工具。</span><span class="sxs-lookup"><span data-stu-id="f826a-179">Attributes are very helpful tools when you need to override the default behavior of model binding.</span></span>

## <a name="binding-formatted-data-from-the-request-body"></a><span data-ttu-id="f826a-180">從要求主體格式的繫結資料</span><span class="sxs-lookup"><span data-stu-id="f826a-180">Binding formatted data from the request body</span></span>

<span data-ttu-id="f826a-181">要求的資料可以來自各種不同的格式，包括 JSON、 XML 及其他等等。</span><span class="sxs-lookup"><span data-stu-id="f826a-181">Request data can come in a variety of formats including JSON, XML and many others.</span></span> <span data-ttu-id="f826a-182">當您使用 [FromBody] 屬性來指出您想要將參數繫結至的要求主體中的資料時，MVC 會使用設定的格式器來處理要求資料，根據其內容的型別。</span><span class="sxs-lookup"><span data-stu-id="f826a-182">When you use the [FromBody] attribute to indicate that you want to bind a parameter to data in the request body, MVC uses a configured set of formatters to handle the request data based on its content type.</span></span> <span data-ttu-id="f826a-183">根據預設，MVC 包含`JsonInputFormatter`類別處理 JSON 資料，但是您可以新增其他的格式器來處理 XML 和其他自訂格式。</span><span class="sxs-lookup"><span data-stu-id="f826a-183">By default MVC includes a `JsonInputFormatter` class for handling JSON data, but you can add additional formatters for handling XML and other custom formats.</span></span>

> [!NOTE]
> <span data-ttu-id="f826a-184">有最多可達一個參數，每個動作以裝飾`[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="f826a-184">There can be at most one parameter per action decorated with `[FromBody]`.</span></span> <span data-ttu-id="f826a-185">ASP.NET Core MVC 執行階段委派要求資料流讀取的格式器的責任。</span><span class="sxs-lookup"><span data-stu-id="f826a-185">The ASP.NET Core MVC run-time delegates the responsibility of reading the request stream to the formatter.</span></span> <span data-ttu-id="f826a-186">一旦參數讀取要求資料流時，它通常不可能是讀取要求資料流，再為其他繫結`[FromBody]`參數。</span><span class="sxs-lookup"><span data-stu-id="f826a-186">Once the request stream is read for a parameter, it's generally not possible to read the request stream again for binding other `[FromBody]` parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="f826a-187">`JsonInputFormatter`是預設的格式器，並且根據[Json.NET](https://www.newtonsoft.com/json)。</span><span class="sxs-lookup"><span data-stu-id="f826a-187">The `JsonInputFormatter` is the default formatter and is based on [Json.NET](https://www.newtonsoft.com/json).</span></span>

<span data-ttu-id="f826a-188">ASP.NET 選取輸入的格式器基礎[Content-type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)標頭和參數的型別除非套用，否則為指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="f826a-188">ASP.NET selects input formatters based on the [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) header and the type of the parameter, unless there is an attribute applied to it specifying otherwise.</span></span> <span data-ttu-id="f826a-189">如果您想要使用 XML 或另一種格式必須設定在*Startup.cs*檔案，但您可能需要取得的參考`Microsoft.AspNetCore.Mvc.Formatters.Xml`使用 NuGet。</span><span class="sxs-lookup"><span data-stu-id="f826a-189">If you'd like to use XML or another format you must configure it in the *Startup.cs* file, but you may first have to obtain a reference to `Microsoft.AspNetCore.Mvc.Formatters.Xml` using NuGet.</span></span> <span data-ttu-id="f826a-190">您的啟始程式碼看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="f826a-190">Your startup code should look something like this:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

<span data-ttu-id="f826a-191">中的程式碼*Startup.cs*檔案包含`ConfigureServices`方法`services`引數可用來建置您的 ASP.NET 應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="f826a-191">Code in the *Startup.cs* file contains a `ConfigureServices` method with a `services` argument you can use to build up services for your ASP.NET app.</span></span> <span data-ttu-id="f826a-192">在範例中，我們會將 XML 格式器加入為 MVC 會提供此應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="f826a-192">In the sample, we are adding an XML formatter as a service that MVC will provide for this app.</span></span> <span data-ttu-id="f826a-193">`options`引數傳遞至`AddMvc`方法可讓您新增及管理篩選器、 格式器，以及其他系統選項從 MVC 應用程式啟動時。</span><span class="sxs-lookup"><span data-stu-id="f826a-193">The `options` argument passed into the `AddMvc` method allows you to add and manage filters, formatters, and other system options from MVC upon app startup.</span></span> <span data-ttu-id="f826a-194">然後套用`Consumes`屬性到控制器類別或動作方法，才能使用您想要的格式。</span><span class="sxs-lookup"><span data-stu-id="f826a-194">Then apply the `Consumes` attribute to controller classes or action methods to work with the format you want.</span></span>

### <a name="custom-model-binding"></a><span data-ttu-id="f826a-195">自訂的模型繫結</span><span class="sxs-lookup"><span data-stu-id="f826a-195">Custom Model Binding</span></span>

<span data-ttu-id="f826a-196">您可以透過撰寫您自己自訂的模型繫結器擴充模型繫結。</span><span class="sxs-lookup"><span data-stu-id="f826a-196">You can extend model binding by writing your own custom model binders.</span></span> <span data-ttu-id="f826a-197">深入了解[自訂模型繫結](../advanced/custom-model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="f826a-197">Learn more about [custom model binding](../advanced/custom-model-binding.md).</span></span>
