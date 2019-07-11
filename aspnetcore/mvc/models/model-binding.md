---
title: ASP.NET Core 中的資料繫結
author: tdykstra
description: 了解 ASP.NET Core 中的模型繫結如何運作，以及如何自訂其行為。
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 10a9f8327bf16d11ec1e04ac3888d701f1ab1778
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724536"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="83856-103">ASP.NET Core 中的資料繫結</span><span class="sxs-lookup"><span data-stu-id="83856-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="83856-104">本文會說明何謂模型繫結、其運作方式，以及如何自訂其行為。</span><span class="sxs-lookup"><span data-stu-id="83856-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="83856-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="83856-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="83856-106">何謂模型繫結</span><span class="sxs-lookup"><span data-stu-id="83856-106">What is Model binding</span></span>

<span data-ttu-id="83856-107">控制器和 Razor Pages 使用來自 HTTP 要求的資料。</span><span class="sxs-lookup"><span data-stu-id="83856-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="83856-108">例如，路由資料可能會提供記錄索引鍵，而已張貼的表單欄位可能會提供模型屬性的值。</span><span class="sxs-lookup"><span data-stu-id="83856-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="83856-109">撰寫程式碼來擷取這些值的每一個並將它們從字串轉換成 .NET 類型，不但繁瑣又容易發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="83856-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="83856-110">模型繫結會自動化此程序。</span><span class="sxs-lookup"><span data-stu-id="83856-110">Model binding automates this process.</span></span> <span data-ttu-id="83856-111">模型繫結系統：</span><span class="sxs-lookup"><span data-stu-id="83856-111">The model binding system:</span></span>

* <span data-ttu-id="83856-112">擷取來自各種來源的資料，例如路由資料、表單欄位和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="83856-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="83856-113">在方法參數和公用屬性中，將資料提供給控制器和 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="83856-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="83856-114">將字串資料轉換成 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="83856-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="83856-115">更新複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="83856-116">範例</span><span class="sxs-lookup"><span data-stu-id="83856-116">Example</span></span>

<span data-ttu-id="83856-117">假設您有下列動作方法：</span><span class="sxs-lookup"><span data-stu-id="83856-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="83856-118">而應用程式收到具有此 URL 的要求：</span><span class="sxs-lookup"><span data-stu-id="83856-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="83856-119">在路由系統選取動作方法之後，模型繫結會透過下列步驟：</span><span class="sxs-lookup"><span data-stu-id="83856-119">Model binding goes though the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="83856-120">尋找第一個參數 `GetByID`，它是名為 `id` 的整數。</span><span class="sxs-lookup"><span data-stu-id="83856-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="83856-121">查看 HTTP 要求中所有可用的來源，在路由資料中找到 `id` = "2"。</span><span class="sxs-lookup"><span data-stu-id="83856-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="83856-122">將字串 "2" 轉換成整數 2。</span><span class="sxs-lookup"><span data-stu-id="83856-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="83856-123">尋找下一個 `GetByID` 的參數，它是名為 `dogsOnly` 的布林值。</span><span class="sxs-lookup"><span data-stu-id="83856-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="83856-124">查看來源，在查詢字串中找到 "DogsOnly=true"。</span><span class="sxs-lookup"><span data-stu-id="83856-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="83856-125">名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="83856-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="83856-126">將字串 "true" 轉換成布林值 `true`。</span><span class="sxs-lookup"><span data-stu-id="83856-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="83856-127">架構接著會呼叫 `GetById` 方法，針對 `id` 參數傳送 2、`dogsOnly` 參數傳送 `true`。</span><span class="sxs-lookup"><span data-stu-id="83856-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="83856-128">在上例中，模型繫結目標都是簡單型別的方法參數。</span><span class="sxs-lookup"><span data-stu-id="83856-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="83856-129">目標也可能是複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="83856-130">成功繫結每一個屬性後，該屬性就會發生[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="83856-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="83856-131">哪些資料繫結至模型，以及任何繫結或驗證錯誤的記錄，都會儲存在 [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) 或 [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)。</span><span class="sxs-lookup"><span data-stu-id="83856-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="83856-132">為了解此程序是否成功，應用程式會檢查 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) 旗標。</span><span class="sxs-lookup"><span data-stu-id="83856-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="83856-133">目標</span><span class="sxs-lookup"><span data-stu-id="83856-133">Targets</span></span>

<span data-ttu-id="83856-134">模型繫結會嘗試尋找下列幾種目標的值：</span><span class="sxs-lookup"><span data-stu-id="83856-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="83856-135">要求路由目標的控制器動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="83856-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="83856-136">路由要求之目標 Razor Pages 處理常式方法的參數。</span><span class="sxs-lookup"><span data-stu-id="83856-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="83856-137">控制站的公用屬性或 `PageModel` 類別，如由屬性指定。</span><span class="sxs-lookup"><span data-stu-id="83856-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="83856-138">[BindProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-138">[BindProperty] attribute</span></span>

<span data-ttu-id="83856-139">可以套用至控制器公用屬性或 `PageModel` 類別，讓模型繫結以該屬性為目標：</span><span class="sxs-lookup"><span data-stu-id="83856-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="83856-140">[BindProperties] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-140">[BindProperties] attribute</span></span>

<span data-ttu-id="83856-141">支援 ASP.NET Core 2.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="83856-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="83856-142">可以套用至控制器或 `PageModel` 類別，通知模型繫結以該類別的所有公用屬性為目標：</span><span class="sxs-lookup"><span data-stu-id="83856-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="83856-143">適用於 HTTP GET 要求的模型繫結</span><span class="sxs-lookup"><span data-stu-id="83856-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="83856-144">根據預設，屬性不會針對 HTTP GET 要求繫結。</span><span class="sxs-lookup"><span data-stu-id="83856-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="83856-145">一般而言，GET 要求只需要記錄識別碼參數。</span><span class="sxs-lookup"><span data-stu-id="83856-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="83856-146">此記錄識別碼用來查詢資料庫中的項目。</span><span class="sxs-lookup"><span data-stu-id="83856-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="83856-147">因此，不需要繫結保留模型執行個體的屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="83856-148">在您要從 GET 要求將屬性繫結至資料的案例中，請將 `SupportsGet` 屬性設為 `true`：</span><span class="sxs-lookup"><span data-stu-id="83856-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="83856-149">來源</span><span class="sxs-lookup"><span data-stu-id="83856-149">Sources</span></span>

<span data-ttu-id="83856-150">根據預設，模型繫結會從下列 HTTP 要求的來源中，取得索引鍵/值組形式的資料：</span><span class="sxs-lookup"><span data-stu-id="83856-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="83856-151">表單欄位</span><span class="sxs-lookup"><span data-stu-id="83856-151">Form fields</span></span> 
1. <span data-ttu-id="83856-152">要求本文 (適用於[具有 [ApiController] 屬性的控制器](xref:web-api/index#binding-source-parameter-inference)。)</span><span class="sxs-lookup"><span data-stu-id="83856-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="83856-153">路由資料</span><span class="sxs-lookup"><span data-stu-id="83856-153">Route data</span></span>
1. <span data-ttu-id="83856-154">查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="83856-154">Query string parameters</span></span>
1. <span data-ttu-id="83856-155">已上傳的檔案</span><span class="sxs-lookup"><span data-stu-id="83856-155">Uploaded files</span></span> 

<span data-ttu-id="83856-156">針對每個目標參數或屬性，會依照本清單顯示的順序掃描來源。</span><span class="sxs-lookup"><span data-stu-id="83856-156">For each target parameter or property, the sources are scanned in the order indicated in this list.</span></span> <span data-ttu-id="83856-157">但也有一些例外：</span><span class="sxs-lookup"><span data-stu-id="83856-157">There are a few exceptions:</span></span>

* <span data-ttu-id="83856-158">路由資料和查詢字串值只用於簡單型別。</span><span class="sxs-lookup"><span data-stu-id="83856-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="83856-159">所上傳檔案只會繫結至實作 `IFormFile` 或 `IEnumerable<IFormFile>` 的目標類型。</span><span class="sxs-lookup"><span data-stu-id="83856-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="83856-160">如果預設行為不提供正確的結果，您可以使用下列其中一個屬性來指定用於任何所指定目標的來源。</span><span class="sxs-lookup"><span data-stu-id="83856-160">If the default behavior doesn't give the right results, you can use one of the following attributes to specify the source to use for any given target.</span></span> 

* <span data-ttu-id="83856-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - 從查詢字串取得值。</span><span class="sxs-lookup"><span data-stu-id="83856-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="83856-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - 從路由資料取得值。</span><span class="sxs-lookup"><span data-stu-id="83856-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="83856-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - 從已張貼的表單欄位取得值。</span><span class="sxs-lookup"><span data-stu-id="83856-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="83856-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - 從要求本文取得值。</span><span class="sxs-lookup"><span data-stu-id="83856-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="83856-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - 從 HTTP 標頭取得值。</span><span class="sxs-lookup"><span data-stu-id="83856-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="83856-166">這些屬性：</span><span class="sxs-lookup"><span data-stu-id="83856-166">These attributes:</span></span>

* <span data-ttu-id="83856-167">會個別新增至模型屬性 (不是模型類別)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="83856-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="83856-168">選擇性地接受建構函式中的模型名稱值。</span><span class="sxs-lookup"><span data-stu-id="83856-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="83856-169">如果屬性名稱不符合要求中的值，則提供此選項。</span><span class="sxs-lookup"><span data-stu-id="83856-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="83856-170">例如，要求中的值可能是名稱中有連字號的標頭，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="83856-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="83856-171">[FromBody] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-171">[FromBody] attribute</span></span>

<span data-ttu-id="83856-172">要求本文資料會使用要求內容類型特定的輸入格式器來剖析。</span><span class="sxs-lookup"><span data-stu-id="83856-172">The request body data is parsed by using input formatters specific to the content type of the request.</span></span> <span data-ttu-id="83856-173">[本文稍後](#input-formatters)會說明輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="83856-173">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="83856-174">針對每個動作方法，請不要將 `[FromBody]` 套用至多個參數。</span><span class="sxs-lookup"><span data-stu-id="83856-174">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="83856-175">ASP.NET Core 執行階段會將讀取要求資料流的責任委派給輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="83856-175">The ASP.NET Core runtime delegates the responsibility of reading the request stream to the input formatter.</span></span> <span data-ttu-id="83856-176">要求資料經讀取後，即無法再次讀取以用來繫結其他 `[FromBody]` 參數。</span><span class="sxs-lookup"><span data-stu-id="83856-176">Once the request stream is read, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="83856-177">其他來源</span><span class="sxs-lookup"><span data-stu-id="83856-177">Additional sources</span></span>

<span data-ttu-id="83856-178">來源資料由「值提供者」  提供給模型繫結系統。</span><span class="sxs-lookup"><span data-stu-id="83856-178">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="83856-179">您可以撰寫並註冊自訂值提供者，其從其他來源取得模型繫結資料。</span><span class="sxs-lookup"><span data-stu-id="83856-179">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="83856-180">例如，您可能想要 Cookie 或工作階段狀態的資料。</span><span class="sxs-lookup"><span data-stu-id="83856-180">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="83856-181">從新來源取得資料：</span><span class="sxs-lookup"><span data-stu-id="83856-181">To get data from a new source:</span></span>

* <span data-ttu-id="83856-182">建立會實作 `IValueProvider` 的類別。</span><span class="sxs-lookup"><span data-stu-id="83856-182">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="83856-183">建立會實作 `IValueProviderFactory` 的類別。</span><span class="sxs-lookup"><span data-stu-id="83856-183">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="83856-184">在 `Startup.ConfigureServices` 中註冊 Factory 類別。</span><span class="sxs-lookup"><span data-stu-id="83856-184">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="83856-185">範例應用程式包含[值提供者](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs)和從 Cookie 取得值的 [actory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) 範例。</span><span class="sxs-lookup"><span data-stu-id="83856-185">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="83856-186">以下是 `Startup.ConfigureServices` 中的註冊碼：</span><span class="sxs-lookup"><span data-stu-id="83856-186">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="83856-187">顯示的程式碼會將自訂值提供者放在所有內建值提供者的後面。</span><span class="sxs-lookup"><span data-stu-id="83856-187">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="83856-188">若要讓它在清單中排位第一，請呼叫 `Insert(0, new CookieValueProviderFactory())` 而非 `Add`。</span><span class="sxs-lookup"><span data-stu-id="83856-188">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="83856-189">無模型屬性的來源</span><span class="sxs-lookup"><span data-stu-id="83856-189">No source for a model property</span></span>

<span data-ttu-id="83856-190">根據預設，如果找不到模型屬性值，就不會建立模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="83856-190">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="83856-191">屬性設定為 null 或預設值：</span><span class="sxs-lookup"><span data-stu-id="83856-191">The property is set to null or a default value:</span></span>

* <span data-ttu-id="83856-192">可為 Null 的簡單型別會設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="83856-192">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="83856-193">不可為 Null 的實值型別會設為 `default(T)`。</span><span class="sxs-lookup"><span data-stu-id="83856-193">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="83856-194">例如，參數 `int id` 設為 0。</span><span class="sxs-lookup"><span data-stu-id="83856-194">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="83856-195">針對複雜類型，模型繫結會使用預設建構函式建立執行個體，但不設定屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-195">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="83856-196">陣列設為 `Array.Empty<T>()`，但 `byte[]` 陣列設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="83856-196">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="83856-197">如果模型狀態在模型屬性表單欄位中找不到任何項目時應該失效，則請使用 [[BindRequired] 屬性](#bindrequired-attribute)。</span><span class="sxs-lookup"><span data-stu-id="83856-197">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="83856-198">請注意，此 `[BindRequired]` 行為適用於來自已張貼表單資料的模型繫結，不適合要求本文中的 JSON 或 XML 資料。</span><span class="sxs-lookup"><span data-stu-id="83856-198">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="83856-199">要求本文資料由[輸入格式器](#input-formatters)處理。</span><span class="sxs-lookup"><span data-stu-id="83856-199">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="83856-200">類型轉換錯誤</span><span class="sxs-lookup"><span data-stu-id="83856-200">Type conversion errors</span></span>

<span data-ttu-id="83856-201">如果找到來源，但無法轉換成目標型別，則模型狀態會標記為無效。</span><span class="sxs-lookup"><span data-stu-id="83856-201">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="83856-202">目標參數或屬性會設為 null 或預設值，如上一節中所述。</span><span class="sxs-lookup"><span data-stu-id="83856-202">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="83856-203">在具有 `[ApiController]` 屬性的 API 控制器中，無效的模型狀態會導致自動 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="83856-203">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="83856-204">在 Razor 頁面中，重新顯示有錯誤訊息的頁面：</span><span class="sxs-lookup"><span data-stu-id="83856-204">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="83856-205">用戶端驗證會攔截大部分不正確的資料，否則會被提交至 Razor Pages 表單。</span><span class="sxs-lookup"><span data-stu-id="83856-205">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="83856-206">此驗證會讓您更難觸發上述醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="83856-206">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="83856-207">範例應用程式包含 [Submit with Invalid Date] \(以無效日期送出\)  按鈕，將不正確的資料放入 [雇用日期]  欄位並提交表單。</span><span class="sxs-lookup"><span data-stu-id="83856-207">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="83856-208">此按鈕會顯示當資料轉換錯誤發生時，重新顯示頁面的程式碼如何運作。</span><span class="sxs-lookup"><span data-stu-id="83856-208">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="83856-209">當上述程式碼重新顯示頁面時，無效的輸入不會顯示在表單欄位中。</span><span class="sxs-lookup"><span data-stu-id="83856-209">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="83856-210">這是因為模型屬性已設為 null 或預設值。</span><span class="sxs-lookup"><span data-stu-id="83856-210">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="83856-211">無效的輸入確實會出現在錯誤訊息中。</span><span class="sxs-lookup"><span data-stu-id="83856-211">The invalid input does appear in an error message.</span></span> <span data-ttu-id="83856-212">但是，如果您想要在表單欄位中重新顯示不正確的資料，請考慮讓模型屬性成為字串，以手動方式執行資料轉換。</span><span class="sxs-lookup"><span data-stu-id="83856-212">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="83856-213">如果您不希望類型轉換錯誤導致模型狀態錯誤，建議使用相同的策略。</span><span class="sxs-lookup"><span data-stu-id="83856-213">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="83856-214">在此情況下，讓模型屬性成為字串。</span><span class="sxs-lookup"><span data-stu-id="83856-214">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="83856-215">簡單型別</span><span class="sxs-lookup"><span data-stu-id="83856-215">Simple types</span></span>

<span data-ttu-id="83856-216">模型繫結器可將來源字串轉換成的簡單型別包括：</span><span class="sxs-lookup"><span data-stu-id="83856-216">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="83856-217">布林值</span><span class="sxs-lookup"><span data-stu-id="83856-217">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="83856-218">[Byte](xref:System.ComponentModel.ByteConverter)、[SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="83856-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="83856-219">Char</span><span class="sxs-lookup"><span data-stu-id="83856-219">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="83856-220">DateTime</span><span class="sxs-lookup"><span data-stu-id="83856-220">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="83856-221">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="83856-221">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="83856-222">Decimal</span><span class="sxs-lookup"><span data-stu-id="83856-222">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="83856-223">Double</span><span class="sxs-lookup"><span data-stu-id="83856-223">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="83856-224">Enum</span><span class="sxs-lookup"><span data-stu-id="83856-224">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="83856-225">Guid</span><span class="sxs-lookup"><span data-stu-id="83856-225">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="83856-226">[Int16](xref:System.ComponentModel.Int16Converter)、[Int32](xref:System.ComponentModel.Int32Converter)、[Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="83856-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="83856-227">Single</span><span class="sxs-lookup"><span data-stu-id="83856-227">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="83856-228">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="83856-228">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="83856-229">[UInt16](xref:System.ComponentModel.UInt16Converter)、[UInt32](xref:System.ComponentModel.UInt32Converter)、[UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="83856-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="83856-230">Uri</span><span class="sxs-lookup"><span data-stu-id="83856-230">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="83856-231">版本</span><span class="sxs-lookup"><span data-stu-id="83856-231">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="83856-232">複雜類型</span><span class="sxs-lookup"><span data-stu-id="83856-232">Complex types</span></span>

<span data-ttu-id="83856-233">複雜類型必須具有要繫結的公用預設建構函式及公用可寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-233">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="83856-234">發生模型繫結時，類別會使用公用預設建構函式具現化。</span><span class="sxs-lookup"><span data-stu-id="83856-234">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="83856-235">針對複雜類型的每個屬性，模型繫結會查看名稱模式 *prefix.property_name* 的來源。</span><span class="sxs-lookup"><span data-stu-id="83856-235">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="83856-236">如果找不到，它會只尋找沒有前置詞的 *property_name*。</span><span class="sxs-lookup"><span data-stu-id="83856-236">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="83856-237">若要繫結至參數，則前置詞是參數名稱。</span><span class="sxs-lookup"><span data-stu-id="83856-237">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="83856-238">若要繫結至 `PageModel` 公用屬性，則前置詞為公用屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="83856-238">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="83856-239">某些屬性 (Attribute) 具有 `Prefix` 屬性 (Property)，其可讓您覆寫參數或屬性名稱的預設使用方法。</span><span class="sxs-lookup"><span data-stu-id="83856-239">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="83856-240">例如，假設複雜類型是下列 `Instructor` 類別：</span><span class="sxs-lookup"><span data-stu-id="83856-240">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="83856-241">前置詞 = 參數名稱</span><span class="sxs-lookup"><span data-stu-id="83856-241">Prefix = parameter name</span></span>

<span data-ttu-id="83856-242">如果要繫結的模型是名為 `instructorToUpdate` 的參數：</span><span class="sxs-lookup"><span data-stu-id="83856-242">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="83856-243">模型繫結從查看索引鍵 `instructorToUpdate.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="83856-243">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="83856-244">如果找不到，則它會尋找不含前置詞的 `ID`。</span><span class="sxs-lookup"><span data-stu-id="83856-244">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="83856-245">前置詞 = 屬性名稱</span><span class="sxs-lookup"><span data-stu-id="83856-245">Prefix = property name</span></span>

<span data-ttu-id="83856-246">如果要繫結的模型是控制器或 `PageModel` 類別名為 `Instructor` 的屬性：</span><span class="sxs-lookup"><span data-stu-id="83856-246">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="83856-247">模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="83856-247">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="83856-248">如果找不到，則它會尋找不含前置詞的 `ID`。</span><span class="sxs-lookup"><span data-stu-id="83856-248">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="83856-249">自訂前置詞</span><span class="sxs-lookup"><span data-stu-id="83856-249">Custom prefix</span></span>

<span data-ttu-id="83856-250">如果要繫結的模型是名為 `instructorToUpdate` 的參數，且 `Bind` 屬性指定 `Instructor` 為前置詞：</span><span class="sxs-lookup"><span data-stu-id="83856-250">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="83856-251">模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="83856-251">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="83856-252">如果找不到，則它會尋找不含前置詞的 `ID`。</span><span class="sxs-lookup"><span data-stu-id="83856-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="83856-253">複雜類型目標的屬性</span><span class="sxs-lookup"><span data-stu-id="83856-253">Attributes for complex type targets</span></span>

<span data-ttu-id="83856-254">有數個內建屬性可供控制複雜類型的模型繫結：</span><span class="sxs-lookup"><span data-stu-id="83856-254">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="83856-255">當張貼的表單資料為值來源時，這些屬性會影響模型繫結。</span><span class="sxs-lookup"><span data-stu-id="83856-255">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="83856-256">它們不會影響處理已張貼 JSON 和 XML 要求本文的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="83856-256">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="83856-257">[本文稍後](#input-formatters)會說明輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="83856-257">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="83856-258">另請參閱[模型驗證](xref:mvc/models/validation#required-attribute)中的 `[Required]` 屬性討論。</span><span class="sxs-lookup"><span data-stu-id="83856-258">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="83856-259">[BindRequired] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-259">[BindRequired] attribute</span></span>

<span data-ttu-id="83856-260">只能套用至模型屬性，不能套用到方法參數。</span><span class="sxs-lookup"><span data-stu-id="83856-260">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="83856-261">如果模型的屬性不能發生繫結，則會造成模型繫結新增模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="83856-261">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="83856-262">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="83856-262">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="83856-263">[BindNever] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-263">[BindNever] attribute</span></span>

<span data-ttu-id="83856-264">只能套用至模型屬性，不能套用到方法參數。</span><span class="sxs-lookup"><span data-stu-id="83856-264">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="83856-265">避免模型繫結設定模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-265">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="83856-266">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="83856-266">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="83856-267">[Bind] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-267">[Bind] attribute</span></span>

<span data-ttu-id="83856-268">可以套用至類別或方法參數。</span><span class="sxs-lookup"><span data-stu-id="83856-268">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="83856-269">指定模型繫結應包含哪些模型屬性。</span><span class="sxs-lookup"><span data-stu-id="83856-269">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="83856-270">在下列範例中，當呼叫任何處理常式或動作方法時，只會繫結 `Instructor` 模型的指定屬性：</span><span class="sxs-lookup"><span data-stu-id="83856-270">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="83856-271">在下列範例中，當呼叫 `OnPost` 方法時，只會繫結 `Instructor` 模型的指定屬性：</span><span class="sxs-lookup"><span data-stu-id="83856-271">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="83856-272">`[Bind]` 屬性可用來防止「建立」  案例中的大量指派。</span><span class="sxs-lookup"><span data-stu-id="83856-272">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="83856-273">它在編輯案例中運作不良，因為排除的屬性都設定為 null 或預設值，而非保持不變。</span><span class="sxs-lookup"><span data-stu-id="83856-273">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="83856-274">建議使用檢視模型而非 `[Bind]` 屬性來防禦大量指派。</span><span class="sxs-lookup"><span data-stu-id="83856-274">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="83856-275">如需詳細資訊，請參閱[關於大量指派的安全性注意事項](xref:data/ef-mvc/crud#security-note-about-overposting)。</span><span class="sxs-lookup"><span data-stu-id="83856-275">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="83856-276">集合</span><span class="sxs-lookup"><span data-stu-id="83856-276">Collections</span></span>

<span data-ttu-id="83856-277">針對簡單型別集合的目標，模型繫結會尋找符合 *parameter_name* 或 *property_name* 的項目。</span><span class="sxs-lookup"><span data-stu-id="83856-277">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="83856-278">如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。</span><span class="sxs-lookup"><span data-stu-id="83856-278">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="83856-279">例如：</span><span class="sxs-lookup"><span data-stu-id="83856-279">For example:</span></span>

* <span data-ttu-id="83856-280">假設要繫結的參數是名為 `selectedCourses` 的陣列：</span><span class="sxs-lookup"><span data-stu-id="83856-280">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="83856-281">表單或查詢字串資料可以是下列其中一種格式：</span><span class="sxs-lookup"><span data-stu-id="83856-281">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="83856-282">只有表單資料支援下列格式：</span><span class="sxs-lookup"><span data-stu-id="83856-282">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="83856-283">針對前列所有範例格式，模型繫結會將有兩個項目的陣列傳遞至 `selectedCourses` 參數：</span><span class="sxs-lookup"><span data-stu-id="83856-283">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="83856-284">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="83856-284">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="83856-285">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="83856-285">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="83856-286">使用下標數字 (... [0] ... [1] ...) 的資料格式，必須確定從零開始依序編號。</span><span class="sxs-lookup"><span data-stu-id="83856-286">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="83856-287">下標編號中如有任何間距，則會忽略間隔後的所有項目。</span><span class="sxs-lookup"><span data-stu-id="83856-287">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="83856-288">例如，如果下標是 0 和 2，而不是 0 和 1，則忽略第二個項目。</span><span class="sxs-lookup"><span data-stu-id="83856-288">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="83856-289">字典</span><span class="sxs-lookup"><span data-stu-id="83856-289">Dictionaries</span></span>

<span data-ttu-id="83856-290">針對 `Dictionary` 目標，模型繫結會尋找符合 *parameter_name* 或 *property_name* 的項目。</span><span class="sxs-lookup"><span data-stu-id="83856-290">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="83856-291">如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。</span><span class="sxs-lookup"><span data-stu-id="83856-291">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="83856-292">例如：</span><span class="sxs-lookup"><span data-stu-id="83856-292">For example:</span></span>

* <span data-ttu-id="83856-293">假設目標參數是名為 `selectedCourses` 的 `Dictionary<int, string>`：</span><span class="sxs-lookup"><span data-stu-id="83856-293">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="83856-294">已張貼的表單或查詢字串資料看起來會像下列其中一個範例：</span><span class="sxs-lookup"><span data-stu-id="83856-294">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="83856-295">針對前列所有範例格式，模型繫結會將有兩個項目的字典傳遞至 `selectedCourses` 參數：</span><span class="sxs-lookup"><span data-stu-id="83856-295">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="83856-296">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="83856-296">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="83856-297">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="83856-297">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="83856-298">特殊資料類型</span><span class="sxs-lookup"><span data-stu-id="83856-298">Special data types</span></span>

<span data-ttu-id="83856-299">模型繫結可以處理某些特殊資料類型。</span><span class="sxs-lookup"><span data-stu-id="83856-299">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="83856-300">IFormFile 和 IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="83856-300">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="83856-301">HTTP 要求包含上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="83856-301">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="83856-302">也支援多個檔案的 `IEnumerable<IFormFile>`。</span><span class="sxs-lookup"><span data-stu-id="83856-302">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="83856-303">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="83856-303">CancellationToken</span></span>

<span data-ttu-id="83856-304">用來取消非同步控制器中的活動。</span><span class="sxs-lookup"><span data-stu-id="83856-304">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="83856-305">FormCollection</span><span class="sxs-lookup"><span data-stu-id="83856-305">FormCollection</span></span>

<span data-ttu-id="83856-306">用來擷取已張貼表單資料中的所有值。</span><span class="sxs-lookup"><span data-stu-id="83856-306">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="83856-307">輸入格式器</span><span class="sxs-lookup"><span data-stu-id="83856-307">Input formatters</span></span>

<span data-ttu-id="83856-308">要求本文中的資料可以是 JSON、XML 或某些其他格式。</span><span class="sxs-lookup"><span data-stu-id="83856-308">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="83856-309">模型繫結會使用設定處理特定內容類型的「輸入格式器」  ，來剖析此資料。</span><span class="sxs-lookup"><span data-stu-id="83856-309">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="83856-310">根據預設，ASP.NET Core 包含 JSON 型輸入格式器以處理 JSON 資料。</span><span class="sxs-lookup"><span data-stu-id="83856-310">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="83856-311">您可以新增其他內容類型的其他格式器。</span><span class="sxs-lookup"><span data-stu-id="83856-311">You can add other formatters for other content types.</span></span>

<span data-ttu-id="83856-312">ASP.NET Core 選取以 [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) 屬性為基礎的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="83856-312">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="83856-313">若無任何屬性，則它會使用 [Content-Type 標頭](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)。</span><span class="sxs-lookup"><span data-stu-id="83856-313">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="83856-314">使用內建的 XML 輸入格式器：</span><span class="sxs-lookup"><span data-stu-id="83856-314">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="83856-315">安裝 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="83856-315">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="83856-316">在 `Startup.ConfigureServices` 中，呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> 或 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>。</span><span class="sxs-lookup"><span data-stu-id="83856-316">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="83856-317">將 `Consumes` 屬性套用至要求本文應為 XML 的控制器類別或動作方法。</span><span class="sxs-lookup"><span data-stu-id="83856-317">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="83856-318">如需詳細資訊，請參閱 [XML 序列化簡介](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization)。</span><span class="sxs-lookup"><span data-stu-id="83856-318">For more information, see [Introducing XML Serialization](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="83856-319">排除模型繫結中的指定類型</span><span class="sxs-lookup"><span data-stu-id="83856-319">Exclude specified types from model binding</span></span>

<span data-ttu-id="83856-320">模型繫結和驗證系統的行為是由 [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) 所驅動。</span><span class="sxs-lookup"><span data-stu-id="83856-320">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="83856-321">您可以將詳細資料提供者新增至 [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)，藉以自訂 `ModelMetadata`。</span><span class="sxs-lookup"><span data-stu-id="83856-321">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="83856-322">內建的詳細資料提供者可用於停用模型繫結或驗證所指定類型。</span><span class="sxs-lookup"><span data-stu-id="83856-322">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="83856-323">若要停用指定類型之所有模型的模型繫結，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider>。</span><span class="sxs-lookup"><span data-stu-id="83856-323">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="83856-324">例如，若要對類型為 `System.Version` 的所有模型停用模型繫結：</span><span class="sxs-lookup"><span data-stu-id="83856-324">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="83856-325">若要停用指定類型屬性的驗證，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider>。</span><span class="sxs-lookup"><span data-stu-id="83856-325">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="83856-326">例如，若要針對類型為 `System.Guid` 的屬性停用驗證：</span><span class="sxs-lookup"><span data-stu-id="83856-326">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="83856-327">自訂模型繫結器</span><span class="sxs-lookup"><span data-stu-id="83856-327">Custom model binders</span></span>

<span data-ttu-id="83856-328">您可以撰寫自訂的模型繫結器來擴充模型繫結，並使用 `[ModelBinder]` 屬性以針對特定目標來選取它。</span><span class="sxs-lookup"><span data-stu-id="83856-328">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="83856-329">深入了解[自訂模型繫結](xref:mvc/advanced/custom-model-binding)。</span><span class="sxs-lookup"><span data-stu-id="83856-329">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="83856-330">手動模型繫結</span><span class="sxs-lookup"><span data-stu-id="83856-330">Manual model binding</span></span>

<span data-ttu-id="83856-331">使用 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> 方法即可手動叫用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="83856-331">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="83856-332">此方法已於 `ControllerBase` 和 `PageModel` 類別中定義。</span><span class="sxs-lookup"><span data-stu-id="83856-332">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="83856-333">方法多載可讓您指定要使用的前置詞和值提供者。</span><span class="sxs-lookup"><span data-stu-id="83856-333">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="83856-334">如果模型繫結失敗，此方法會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="83856-334">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="83856-335">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="83856-335">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="83856-336">[FromServices] 屬性</span><span class="sxs-lookup"><span data-stu-id="83856-336">[FromServices] attribute</span></span>

<span data-ttu-id="83856-337">這個屬性名稱會遵循指定資料來源的模型繫結屬性模式。</span><span class="sxs-lookup"><span data-stu-id="83856-337">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="83856-338">但它與來自值提供者的資料繫結無關。</span><span class="sxs-lookup"><span data-stu-id="83856-338">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="83856-339">它從[相依性插入](xref:fundamentals/dependency-injection)容器取得類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="83856-339">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="83856-340">其目的是只有在呼叫特定方法時，才在您需要服務時提供建構函式插入的替代項目。</span><span class="sxs-lookup"><span data-stu-id="83856-340">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83856-341">其他資源</span><span class="sxs-lookup"><span data-stu-id="83856-341">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
