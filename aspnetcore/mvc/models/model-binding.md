---
title: ASP.NET Core 中的資料繫結
author: tdykstra
description: 了解 ASP.NET Core MVC 中的模型繫結如何將 HTTP 要求中的資料對應至動作方法參數。
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 08/14/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 0ce20a8040c6b19da1f57e1c053a7ef81d8bcb23
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751668"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="45171-103">ASP.NET Core 中的資料繫結</span><span class="sxs-lookup"><span data-stu-id="45171-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="45171-104">作者：[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="45171-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-binding"></a><span data-ttu-id="45171-105">模型繫結簡介</span><span class="sxs-lookup"><span data-stu-id="45171-105">Introduction to model binding</span></span>

<span data-ttu-id="45171-106">ASP.NET Core MVC 中的模型繫結會將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="45171-106">Model binding in ASP.NET Core MVC maps data from HTTP requests to action method parameters.</span></span> <span data-ttu-id="45171-107">參數可能是簡單類型，例如字串、整數或浮點數，或者可能是複雜類型。</span><span class="sxs-lookup"><span data-stu-id="45171-107">The parameters may be simple types such as strings, integers, or floats, or they may be complex types.</span></span> <span data-ttu-id="45171-108">這是 MVC 的絕佳功能，因為將內送資料對應至對應項目是經常重複的情況，而不論資料的大小或複雜度。</span><span class="sxs-lookup"><span data-stu-id="45171-108">This is a great feature of MVC because mapping incoming data to a counterpart is an often repeated scenario, regardless of size or complexity of the data.</span></span> <span data-ttu-id="45171-109">MVC 解決此問題的方式是抽離繫結，讓開發人員不需要持續重寫每個應用程式中相同程式碼的略稍不同版本。</span><span class="sxs-lookup"><span data-stu-id="45171-109">MVC solves this problem by abstracting binding away so developers don't have to keep rewriting a slightly different version of that same code in every app.</span></span> <span data-ttu-id="45171-110">撰寫您自己的文字類型轉換器程式碼十分冗長，而且容易發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="45171-110">Writing your own text to type converter code is tedious, and error prone.</span></span>

## <a name="how-model-binding-works"></a><span data-ttu-id="45171-111">模型繫結運作方式</span><span class="sxs-lookup"><span data-stu-id="45171-111">How model binding works</span></span>

<span data-ttu-id="45171-112">MVC 在收到 HTTP 要求時，會將它路由至控制器的特定動作方法。</span><span class="sxs-lookup"><span data-stu-id="45171-112">When MVC receives an HTTP request, it routes it to a specific action method of a controller.</span></span> <span data-ttu-id="45171-113">它會根據路由資料中的內容來判斷要執行的動作方法，然後將 HTTP 要求中的值繫結至該動作方法的參數。</span><span class="sxs-lookup"><span data-stu-id="45171-113">It determines which action method to run based on what is in the route data, then it binds values from the HTTP request to that action method's parameters.</span></span> <span data-ttu-id="45171-114">例如，請考慮下列 URL：</span><span class="sxs-lookup"><span data-stu-id="45171-114">For example, consider the following URL:</span></span>

`http://contoso.com/movies/edit/2`

<span data-ttu-id="45171-115">因為路由範本與 `{controller=Home}/{action=Index}/{id?}` 類似，所以 `movies/edit/2` 會路由至 `Movies` 控制器和其 `Edit` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="45171-115">Since the route template looks like this, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` routes to the `Movies` controller, and its `Edit` action method.</span></span> <span data-ttu-id="45171-116">它也接受稱為 `id` 的選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="45171-116">It also accepts an optional parameter called `id`.</span></span> <span data-ttu-id="45171-117">動作方法的程式碼應該如下：</span><span class="sxs-lookup"><span data-stu-id="45171-117">The code for the action method should look something like this:</span></span>

```csharp
public IActionResult Edit(int? id)
   ```

<span data-ttu-id="45171-118">注意：URL 路由中的字串不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="45171-118">Note: The strings in the URL route are not case sensitive.</span></span>

<span data-ttu-id="45171-119">MVC 會嘗試依名稱將要求資料繫結至動作參數。</span><span class="sxs-lookup"><span data-stu-id="45171-119">MVC will try to bind request data to the action parameters by name.</span></span> <span data-ttu-id="45171-120">MVC 會使用參數名稱以及其公用 settable 屬性的名稱來尋找每個參數的值。</span><span class="sxs-lookup"><span data-stu-id="45171-120">MVC will look for values for each parameter using the parameter name and the names of its public settable properties.</span></span> <span data-ttu-id="45171-121">在上述範例中，唯一的動作參數命名為 `id`，而 MVC 會繫結至路由值中同名的值。</span><span class="sxs-lookup"><span data-stu-id="45171-121">In the above example, the only action parameter is named `id`, which MVC binds to the value with the same name in the route values.</span></span> <span data-ttu-id="45171-122">除了路由值之外，MVC 會繫結要求各部分的資料，並且依設定的順序執行。</span><span class="sxs-lookup"><span data-stu-id="45171-122">In addition to route values MVC will bind data from various parts of the request and it does so in a set order.</span></span> <span data-ttu-id="45171-123">以下是資料來源清單，而其順序就是模型繫結查看它們的順序：</span><span class="sxs-lookup"><span data-stu-id="45171-123">Below is a list of the data sources in the order that model binding looks through them:</span></span>

1. <span data-ttu-id="45171-124">`Form values`：這些是使用 POST 方法查看 HTTP 要求的表單值 </span><span class="sxs-lookup"><span data-stu-id="45171-124">`Form values`: These are form values that go in the HTTP request using the POST method.</span></span> <span data-ttu-id="45171-125">(包括 jQuery POST 要求)。</span><span class="sxs-lookup"><span data-stu-id="45171-125">(including jQuery POST requests).</span></span>

2. <span data-ttu-id="45171-126">`Route values`：[路由](xref:fundamentals/routing)所提供的一組路由值</span><span class="sxs-lookup"><span data-stu-id="45171-126">`Route values`: The set of route values provided by [Routing](xref:fundamentals/routing)</span></span>

3. <span data-ttu-id="45171-127">`Query strings`：URI 的查詢字串組件。</span><span class="sxs-lookup"><span data-stu-id="45171-127">`Query strings`: The query string part of the URI.</span></span>

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

<span data-ttu-id="45171-128">注意：表單值、路由資料和查詢字串都是儲存為名稱/值組。</span><span class="sxs-lookup"><span data-stu-id="45171-128">Note: Form values, route data, and query strings are all stored as name-value pairs.</span></span>

<span data-ttu-id="45171-129">因為模型繫結要求名為 `id` 的索引鍵，而且表單值中沒有名為 `id` 的項目，所以會繼續尋找該索引鍵的路由值。</span><span class="sxs-lookup"><span data-stu-id="45171-129">Since model binding asked for a key named `id` and there's nothing named `id` in the form values, it moved on to the route values looking for that key.</span></span> <span data-ttu-id="45171-130">在我們的範例中，它是相符項目。</span><span class="sxs-lookup"><span data-stu-id="45171-130">In our example, it's a match.</span></span> <span data-ttu-id="45171-131">進行繫結，並且將值轉換成整數 2。</span><span class="sxs-lookup"><span data-stu-id="45171-131">Binding happens, and the value is converted to the integer 2.</span></span> <span data-ttu-id="45171-132">使用 Edit(string id) 的相同要求會轉換成字串 "2"。</span><span class="sxs-lookup"><span data-stu-id="45171-132">The same request using Edit(string id) would convert to the string "2".</span></span>

<span data-ttu-id="45171-133">到目前為止，範例會使用簡單類型。</span><span class="sxs-lookup"><span data-stu-id="45171-133">So far the example uses simple types.</span></span> <span data-ttu-id="45171-134">在 MVC 中，簡單類型是任何 .NET 基本類型或是包含字串類型轉換子的類型。</span><span class="sxs-lookup"><span data-stu-id="45171-134">In MVC simple types are any .NET primitive type or type with a string type converter.</span></span> <span data-ttu-id="45171-135">如果動作方法的參數是類別 (例如將簡單和複雜類型包含為屬性的 `Movie` 類型)，則仍會妥善地處理 MVC 的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="45171-135">If the action method's parameter were a class such as the `Movie` type, which contains both simple and complex types as properties, MVC's model binding will still handle it nicely.</span></span> <span data-ttu-id="45171-136">它會使用反映和遞迴以周遊尋找相符項目之複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-136">It uses reflection and recursion to traverse the properties of complex types looking for matches.</span></span> <span data-ttu-id="45171-137">模型繫結會尋找模式 *parameter_name.property_name*，以將值繫結至屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-137">Model binding looks for the pattern *parameter_name.property_name* to bind values to properties.</span></span> <span data-ttu-id="45171-138">如果找不到此表單的相符值，則會嘗試只使用屬性名稱進行繫結。</span><span class="sxs-lookup"><span data-stu-id="45171-138">If it doesn't find matching values of this form, it will attempt to bind using just the property name.</span></span> <span data-ttu-id="45171-139">針對這些類型 (例如 `Collection` 類型)，模型繫結會尋找 *parameter_name[index]* 或只尋找 *[index]* 的相符項目。</span><span class="sxs-lookup"><span data-stu-id="45171-139">For those types such as `Collection` types, model binding looks for matches to *parameter_name[index]* or just *[index]*.</span></span> <span data-ttu-id="45171-140">模型繫結會以同樣地方式處理 `Dictionary` 類型，並要求 *parameter_name[key]* 或只要求 *[key]*，只要索引鍵是簡單類型即可。</span><span class="sxs-lookup"><span data-stu-id="45171-140">Model binding treats  `Dictionary` types similarly, asking for *parameter_name[key]* or just *[key]*, as long as the keys are simple types.</span></span> <span data-ttu-id="45171-141">所支援的索引鍵符合針對相同模型類型所產生的欄位名稱 HTML 和標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="45171-141">Keys that are supported match the field names HTML and tag helpers generated for the same model type.</span></span> <span data-ttu-id="45171-142">這會啟用反覆存取值，讓表單欄位持續填入使用者輸入，以方便使用；例如，從建立或編輯所繫結的資料未通過驗證時。</span><span class="sxs-lookup"><span data-stu-id="45171-142">This enables round-tripping values so that the form fields remain filled with the user's input for their convenience, for example, when bound data from a create or edit didn't pass validation.</span></span>

<span data-ttu-id="45171-143">若要進行繫結，類別必須具有公用預設建構函式，而要繫結的成員必須是公用可寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-143">In order for binding to happen the class must have a public default constructor and member to be bound must be public writable properties.</span></span> <span data-ttu-id="45171-144">發生模型繫結時，只會使用公用預設建構函式來具現化此類別，接著可以設定屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-144">When model binding happens the class will only be instantiated using the public default constructor, then the properties can be set.</span></span>

<span data-ttu-id="45171-145">繫結參數時，模型繫結會停止尋找具有該名稱的值，並繼續繫結下一個參數。</span><span class="sxs-lookup"><span data-stu-id="45171-145">When a parameter is bound, model binding stops looking for values with that name and it moves on to bind the next parameter.</span></span> <span data-ttu-id="45171-146">否則，預設模型繫結行為會將參數設定為其預設值 (視其類型而定)：</span><span class="sxs-lookup"><span data-stu-id="45171-146">Otherwise, the default model binding behavior sets parameters to their default values depending on their type:</span></span>

* <span data-ttu-id="45171-147">`T[]`：繫結會將類型 `T[]` 的參數設定為 `Array.Empty<T>()`，但類型 `byte[]` 陣列除外。</span><span class="sxs-lookup"><span data-stu-id="45171-147">`T[]`: With the exception of arrays of type `byte[]`, binding sets parameters of type `T[]` to `Array.Empty<T>()`.</span></span> <span data-ttu-id="45171-148">類型 `byte[]` 陣列會設定為 `null`。</span><span class="sxs-lookup"><span data-stu-id="45171-148">Arrays of type `byte[]` are set to `null`.</span></span>

* <span data-ttu-id="45171-149">參考類型：繫結會建立具有預設建構函式的類別執行個體，但未設定屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-149">Reference Types: Binding creates an instance of a class with the default constructor without setting properties.</span></span> <span data-ttu-id="45171-150">不過，模型繫結會將 `string` 參數設定為 `null`。</span><span class="sxs-lookup"><span data-stu-id="45171-150">However, model binding sets `string` parameters to `null`.</span></span>

* <span data-ttu-id="45171-151">可為 Null 的類型：可為 Null 的類型設定為 `null`。</span><span class="sxs-lookup"><span data-stu-id="45171-151">Nullable Types: Nullable types are set to `null`.</span></span> <span data-ttu-id="45171-152">在上述範例中，模型繫結會將 `id` 設定為 `null`，因為它的類型為 `int?`。</span><span class="sxs-lookup"><span data-stu-id="45171-152">In the above example, model binding sets `id` to `null` since it's of type `int?`.</span></span>

* <span data-ttu-id="45171-153">實值型別：類型為 `T` 的不可為 Null 的實值型別會設定為 `default(T)`。</span><span class="sxs-lookup"><span data-stu-id="45171-153">Value Types: Non-nullable value types of type `T` are set to `default(T)`.</span></span> <span data-ttu-id="45171-154">例如，模型繫結會將參數 `int id` 設定為 0。</span><span class="sxs-lookup"><span data-stu-id="45171-154">For example, model binding will set a parameter `int id` to 0.</span></span> <span data-ttu-id="45171-155">請考慮使用模型驗證或可為 Null 的類型，而不要依賴預設值。</span><span class="sxs-lookup"><span data-stu-id="45171-155">Consider using model validation or nullable types rather than relying on default values.</span></span>

<span data-ttu-id="45171-156">如果繫結失敗，則 MVC 不會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="45171-156">If binding fails, MVC doesn't throw an error.</span></span> <span data-ttu-id="45171-157">接受使用者輸入的每一個動作都應該檢查 `ModelState.IsValid` 屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-157">Every action which accepts user input should check the `ModelState.IsValid` property.</span></span>

<span data-ttu-id="45171-158">注意：控制器 `ModelState` 屬性中的每個項目都是包含 `Errors` 屬性的 `ModelStateEntry`。</span><span class="sxs-lookup"><span data-stu-id="45171-158">Note: Each entry in the controller's `ModelState` property is a `ModelStateEntry` containing an `Errors` property.</span></span> <span data-ttu-id="45171-159">很少需要自行查詢此集合。</span><span class="sxs-lookup"><span data-stu-id="45171-159">It's rarely necessary to query this collection yourself.</span></span> <span data-ttu-id="45171-160">請改用 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="45171-160">Use `ModelState.IsValid` instead.</span></span>

<span data-ttu-id="45171-161">此外，MVC 必須在執行模型繫結時考量一些特殊資料類型：</span><span class="sxs-lookup"><span data-stu-id="45171-161">Additionally, there are some special data types that MVC must consider when performing model binding:</span></span>

* <span data-ttu-id="45171-162">`IFormFile`、`IEnumerable<IFormFile>`：屬於 HTTP 要求一部分的一或多個已上傳檔案。</span><span class="sxs-lookup"><span data-stu-id="45171-162">`IFormFile`, `IEnumerable<IFormFile>`: One or more uploaded files that are part of the HTTP request.</span></span>

* <span data-ttu-id="45171-163">`CancellationToken`：用來取消非同步控制器中的活動。</span><span class="sxs-lookup"><span data-stu-id="45171-163">`CancellationToken`: Used to cancel activity in asynchronous controllers.</span></span>

<span data-ttu-id="45171-164">這些類型可以繫結至類別類型上的動作參數或屬性。</span><span class="sxs-lookup"><span data-stu-id="45171-164">These types can be bound to action parameters or to properties on a class type.</span></span>

<span data-ttu-id="45171-165">完成模型繫結之後，會進行[驗證](validation.md)。</span><span class="sxs-lookup"><span data-stu-id="45171-165">Once model binding is complete, [Validation](validation.md) occurs.</span></span> <span data-ttu-id="45171-166">預設模型繫結最適用於大部分的開發案例。</span><span class="sxs-lookup"><span data-stu-id="45171-166">Default model binding works great for the vast majority of development scenarios.</span></span> <span data-ttu-id="45171-167">它也可以擴充，因此，如果您有唯一的需求，則可以自訂內建行為。</span><span class="sxs-lookup"><span data-stu-id="45171-167">It's also extensible so if you have unique needs you can customize the built-in behavior.</span></span>

## <a name="customize-model-binding-behavior-with-attributes"></a><span data-ttu-id="45171-168">使用屬性來自訂模型繫結行為</span><span class="sxs-lookup"><span data-stu-id="45171-168">Customize model binding behavior with attributes</span></span>

<span data-ttu-id="45171-169">MVC 會包含數個屬性，可用來將其預設模型繫結行為導向至不同的來源。</span><span class="sxs-lookup"><span data-stu-id="45171-169">MVC contains several attributes that you can use to direct its default model binding behavior to a different source.</span></span> <span data-ttu-id="45171-170">例如，您可以指定屬性是否需要繫結，或者使用 `[BindRequired]` 或 `[BindNever]` 屬性，讓它根本不應該發生。</span><span class="sxs-lookup"><span data-stu-id="45171-170">For example, you can specify whether binding is required for a property, or if it should never happen at all by using the `[BindRequired]` or `[BindNever]` attributes.</span></span> <span data-ttu-id="45171-171">或者，您可以覆寫預設資料來源，並指定模型繫結器的資料來源。</span><span class="sxs-lookup"><span data-stu-id="45171-171">Alternatively, you can override the default data source, and specify the model binder's data source.</span></span> <span data-ttu-id="45171-172">以下是模型繫結屬性清單：</span><span class="sxs-lookup"><span data-stu-id="45171-172">Below is a list of model binding attributes:</span></span>

* <span data-ttu-id="45171-173">`[BindRequired]`：如果無法繫結，則此屬性會新增模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="45171-173">`[BindRequired]`: This attribute adds a model state error if binding cannot occur.</span></span>

* <span data-ttu-id="45171-174">`[BindNever]`：告知模型繫結器永遠不要繫結至此參數。</span><span class="sxs-lookup"><span data-stu-id="45171-174">`[BindNever]`: Tells the model binder to never bind to this parameter.</span></span>

* <span data-ttu-id="45171-175">`[FromHeader]`、`[FromQuery]`、`[FromRoute]`、`[FromForm]`：使用這些來指定您想要套用的確切繫結來源。</span><span class="sxs-lookup"><span data-stu-id="45171-175">`[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Use these to specify the exact binding source you want to apply.</span></span>

* <span data-ttu-id="45171-176">`[FromServices]`：此屬性會使用[相依性插入](../../fundamentals/dependency-injection.md)，以從服務繫結參數。</span><span class="sxs-lookup"><span data-stu-id="45171-176">`[FromServices]`: This attribute uses [dependency injection](../../fundamentals/dependency-injection.md) to bind parameters from services.</span></span>

* <span data-ttu-id="45171-177">`[FromBody]`：使用已設定的格式器，來繫結要求本文中的資料。</span><span class="sxs-lookup"><span data-stu-id="45171-177">`[FromBody]`: Use the configured formatters to bind data from the request body.</span></span> <span data-ttu-id="45171-178">格式器是根據要求的內容類型進行選取。</span><span class="sxs-lookup"><span data-stu-id="45171-178">The formatter is selected based on content type of the request.</span></span>

* <span data-ttu-id="45171-179">`[ModelBinder]`：用來覆寫預設模型繫結器、繫結來源及名稱。</span><span class="sxs-lookup"><span data-stu-id="45171-179">`[ModelBinder]`: Used to override the default model binder, binding source and name.</span></span>

<span data-ttu-id="45171-180">當您需要覆寫模型繫結的預設行為時，屬性是很有幫助的工具。</span><span class="sxs-lookup"><span data-stu-id="45171-180">Attributes are very helpful tools when you need to override the default behavior of model binding.</span></span>

## <a name="customize-model-binding-and-validation-globally"></a><span data-ttu-id="45171-181">全域自訂模型繫結和驗證</span><span class="sxs-lookup"><span data-stu-id="45171-181">Customize model binding and validation globally</span></span>

<span data-ttu-id="45171-182">模型繫結和驗證系統的行為是由描述下列內容的 [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) 所驅動：</span><span class="sxs-lookup"><span data-stu-id="45171-182">The model binding and validation system's behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) that describes:</span></span>

* <span data-ttu-id="45171-183">如何繫結模型。</span><span class="sxs-lookup"><span data-stu-id="45171-183">How a model is to be bound.</span></span>
* <span data-ttu-id="45171-184">如何對類型和其屬性進行驗證。</span><span class="sxs-lookup"><span data-stu-id="45171-184">How validation occurs on the type and its properties.</span></span>

<span data-ttu-id="45171-185">藉由將詳細資料提供者新增至 [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders)，即可全域設定系統行為的各個層面。</span><span class="sxs-lookup"><span data-stu-id="45171-185">Aspects of the system's behavior can be configured globally by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions.modelmetadatadetailsproviders#Microsoft_AspNetCore_Mvc_MvcOptions_ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="45171-186">MVC 有一些內建的詳細資料提供者，其允許設定如停用特定類型的模型繫結或驗證等行為。</span><span class="sxs-lookup"><span data-stu-id="45171-186">MVC has a few built-in details providers that allow configuring behavior such as disabling model binding or validation for certain types.</span></span>

<span data-ttu-id="45171-187">若要對特定類型的所有模型停用模型繫結，請在 `Startup.ConfigureServices` 中新增 [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider)。</span><span class="sxs-lookup"><span data-stu-id="45171-187">To disable model binding on all models of a certain type, add an [ExcludeBindingMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.excludebindingmetadataprovider) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="45171-188">例如，若要對類型為 `System.Version` 的所有模型停用模型繫結：</span><span class="sxs-lookup"><span data-stu-id="45171-188">For example, to disable model binding on all models of type `System.Version`:</span></span>

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new ExcludeBindingMetadataProvider(typeof(System.Version))));
```

<span data-ttu-id="45171-189">若要對特定類型的屬性停用驗證，請在 `Startup.ConfigureServices` 中新增 [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider)。</span><span class="sxs-lookup"><span data-stu-id="45171-189">To disable validation on properties of a certain type, add a [SuppressChildValidationMetadataProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.suppresschildvalidationmetadataprovider) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="45171-190">例如，若要針對類型為 `System.Guid` 的屬性停用驗證：</span><span class="sxs-lookup"><span data-stu-id="45171-190">For example, to disable validation on properties of type `System.Guid`:</span></span>

```csharp
services.AddMvc().AddMvcOptions(options =>
    options.ModelMetadataDetailsProviders.Add(
        new SuppressChildValidationMetadataProvider(typeof(System.Guid))));
```

## <a name="bind-formatted-data-from-the-request-body"></a><span data-ttu-id="45171-191">繫結要求本文中的格式化資料</span><span class="sxs-lookup"><span data-stu-id="45171-191">Bind formatted data from the request body</span></span>

<span data-ttu-id="45171-192">要求資料可以有各種不同的格式，包括 JSON、XML 及其他項目。</span><span class="sxs-lookup"><span data-stu-id="45171-192">Request data can come in a variety of formats including JSON, XML and many others.</span></span> <span data-ttu-id="45171-193">當您使用 [FromBody] 屬性指出您想要將參數繫結至要求本文中的資料時，MVC 會使用一組已設定的格式器，以根據其內容類型來處理要求資料。</span><span class="sxs-lookup"><span data-stu-id="45171-193">When you use the [FromBody] attribute to indicate that you want to bind a parameter to data in the request body, MVC uses a configured set of formatters to handle the request data based on its content type.</span></span> <span data-ttu-id="45171-194">根據預設，MVC 包括 `JsonInputFormatter` 類別來處理 JSON 資料，但是您可以新增其他格式器來處理 XML 及其他自訂格式。</span><span class="sxs-lookup"><span data-stu-id="45171-194">By default MVC includes a `JsonInputFormatter` class for handling JSON data, but you can add additional formatters for handling XML and other custom formats.</span></span>

> [!NOTE]
> <span data-ttu-id="45171-195">一個以 `[FromBody]` 裝飾的動作最多可以有一個參數。</span><span class="sxs-lookup"><span data-stu-id="45171-195">There can be at most one parameter per action decorated with `[FromBody]`.</span></span> <span data-ttu-id="45171-196">ASP.NET Core MVC 執行階段會將讀取要求資料流的責任委派給格式器。</span><span class="sxs-lookup"><span data-stu-id="45171-196">The ASP.NET Core MVC run-time delegates the responsibility of reading the request stream to the formatter.</span></span> <span data-ttu-id="45171-197">讀取參數的要求資料流之後，一般無法重新讀取要求資料流，以繫結其他 `[FromBody]` 參數。</span><span class="sxs-lookup"><span data-stu-id="45171-197">Once the request stream is read for a parameter, it's generally not possible to read the request stream again for binding other `[FromBody]` parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="45171-198">`JsonInputFormatter` 是預設格式器，並且根據 [Json.NET](https://www.newtonsoft.com/json)。</span><span class="sxs-lookup"><span data-stu-id="45171-198">The `JsonInputFormatter` is the default formatter and is based on [Json.NET](https://www.newtonsoft.com/json).</span></span>

<span data-ttu-id="45171-199">除非另外指定套用屬性，否則 ASP.NET Core 會根據 [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) 標頭和參數類型來選取輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="45171-199">ASP.NET Core selects input formatters based on the [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) header and the type of the parameter, unless there's an attribute applied to it specifying otherwise.</span></span> <span data-ttu-id="45171-200">如果您想要使用 XML 或另一種格式，則必須在 *Startup.cs* 檔案中設定它，但您可能需要先使用 NuGet 來取得 `Microsoft.AspNetCore.Mvc.Formatters.Xml` 的參考。</span><span class="sxs-lookup"><span data-stu-id="45171-200">If you'd like to use XML or another format you must configure it in the *Startup.cs* file, but you may first have to obtain a reference to `Microsoft.AspNetCore.Mvc.Formatters.Xml` using NuGet.</span></span> <span data-ttu-id="45171-201">啟動程式碼應該如下：</span><span class="sxs-lookup"><span data-stu-id="45171-201">Your startup code should look something like this:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

<span data-ttu-id="45171-202">*Startup.cs* 檔案中的程式碼包含具有 `services` 引數的 `ConfigureServices` 方法，可用來建置 ASP.NET Core 應用程式的服務。</span><span class="sxs-lookup"><span data-stu-id="45171-202">Code in the *Startup.cs* file contains a `ConfigureServices` method with a `services` argument you can use to build up services for your ASP.NET Core app.</span></span> <span data-ttu-id="45171-203">在範例中，我們會將 XML 格式器新增為 MVC 針對此應用程式所提供的服務。</span><span class="sxs-lookup"><span data-stu-id="45171-203">In the sample, we are adding an XML formatter as a service that MVC will provide for this app.</span></span> <span data-ttu-id="45171-204">傳入 `AddMvc` 方法的 `options` 引數，可讓您在應用程式啟動時從 MVC 新增和管理篩選、格式器以及其他系統選項。</span><span class="sxs-lookup"><span data-stu-id="45171-204">The `options` argument passed into the `AddMvc` method allows you to add and manage filters, formatters, and other system options from MVC upon app startup.</span></span> <span data-ttu-id="45171-205">然後將 `Consumes` 屬性套用至控制器類別或動作方法，以使用您想要的格式。</span><span class="sxs-lookup"><span data-stu-id="45171-205">Then apply the `Consumes` attribute to controller classes or action methods to work with the format you want.</span></span>

### <a name="custom-model-binding"></a><span data-ttu-id="45171-206">自訂模型繫結</span><span class="sxs-lookup"><span data-stu-id="45171-206">Custom Model Binding</span></span>

<span data-ttu-id="45171-207">您可以撰寫自己的自訂模型繫結器來擴充模型繫結。</span><span class="sxs-lookup"><span data-stu-id="45171-207">You can extend model binding by writing your own custom model binders.</span></span> <span data-ttu-id="45171-208">深入了解[自訂模型繫結](../advanced/custom-model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="45171-208">Learn more about [custom model binding](../advanced/custom-model-binding.md).</span></span>
