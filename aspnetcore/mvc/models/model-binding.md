---
title: ASP.NET Core 中的資料繫結
author: rick-anderson
description: 了解 ASP.NET Core 中的模型繫結如何運作，以及如何自訂其行為。
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 12/18/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/models/model-binding
ms.openlocfilehash: 2e604cd1869ea077fc0465df91ec083b9db83763
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768966"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="181f0-103">ASP.NET Core 中的資料繫結</span><span class="sxs-lookup"><span data-stu-id="181f0-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="181f0-104">本文會說明何謂模型繫結、其運作方式，以及如何自訂其行為。</span><span class="sxs-lookup"><span data-stu-id="181f0-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="181f0-105">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="181f0-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="181f0-106">何謂模型繫結</span><span class="sxs-lookup"><span data-stu-id="181f0-106">What is Model binding</span></span>

<span data-ttu-id="181f0-107">控制器和Razor頁面可處理來自 HTTP 要求的資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="181f0-108">例如，路由資料可能會提供記錄索引鍵，而已張貼的表單欄位可能會提供模型屬性的值。</span><span class="sxs-lookup"><span data-stu-id="181f0-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="181f0-109">撰寫程式碼來擷取這些值的每一個並將它們從字串轉換成 .NET 類型，不但繁瑣又容易發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="181f0-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="181f0-110">模型繫結會自動化此程序。</span><span class="sxs-lookup"><span data-stu-id="181f0-110">Model binding automates this process.</span></span> <span data-ttu-id="181f0-111">模型繫結系統：</span><span class="sxs-lookup"><span data-stu-id="181f0-111">The model binding system:</span></span>

* <span data-ttu-id="181f0-112">從各種不同的來源（例如，路由資料、表單欄位和查詢字串）抓取資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="181f0-113">將資料提供給方法參數Razor和公用屬性中的控制器和頁面。</span><span class="sxs-lookup"><span data-stu-id="181f0-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="181f0-114">將字串資料轉換成 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="181f0-115">更新複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="181f0-116">範例</span><span class="sxs-lookup"><span data-stu-id="181f0-116">Example</span></span>

<span data-ttu-id="181f0-117">假設您有下列動作方法：</span><span class="sxs-lookup"><span data-stu-id="181f0-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="181f0-118">而應用程式收到具有此 URL 的要求：</span><span class="sxs-lookup"><span data-stu-id="181f0-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="181f0-119">在路由系統選取動作方法之後，模型系結會經歷下列步驟：</span><span class="sxs-lookup"><span data-stu-id="181f0-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="181f0-120">尋找第一個參數 `GetByID`，它是名為 `id` 的整數。</span><span class="sxs-lookup"><span data-stu-id="181f0-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="181f0-121">查看 HTTP 要求中所有可用的來源，在路由資料中找到 `id` = "2"。</span><span class="sxs-lookup"><span data-stu-id="181f0-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="181f0-122">將字串 "2" 轉換成整數 2。</span><span class="sxs-lookup"><span data-stu-id="181f0-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="181f0-123">尋找的下一個參數`GetByID`，也就是`dogsOnly`名為的布林值。</span><span class="sxs-lookup"><span data-stu-id="181f0-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="181f0-124">查看來源，在查詢字串中找到 "DogsOnly=true"。</span><span class="sxs-lookup"><span data-stu-id="181f0-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="181f0-125">名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="181f0-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="181f0-126">將字串 "true" 轉換成布林`true`值。</span><span class="sxs-lookup"><span data-stu-id="181f0-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="181f0-127">架構接著會呼叫 `GetById` 方法，針對 `id` 參數傳送 2、`dogsOnly` 參數傳送 `true`。</span><span class="sxs-lookup"><span data-stu-id="181f0-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="181f0-128">在上例中，模型繫結目標都是簡單型別的方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="181f0-129">目標也可以是複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="181f0-130">成功系結每個屬性之後，就會針對該屬性進行[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="181f0-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="181f0-131">哪些資料繫結至模型，以及任何繫結或驗證錯誤的記錄，都會儲存在 [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) 或 [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)。</span><span class="sxs-lookup"><span data-stu-id="181f0-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="181f0-132">為了解此程序是否成功，應用程式會檢查 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) 旗標。</span><span class="sxs-lookup"><span data-stu-id="181f0-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="181f0-133">目標</span><span class="sxs-lookup"><span data-stu-id="181f0-133">Targets</span></span>

<span data-ttu-id="181f0-134">模型繫結會嘗試尋找下列幾種目標的值：</span><span class="sxs-lookup"><span data-stu-id="181f0-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="181f0-135">要求路由目標的控制器動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="181f0-136">將要求路由Razor傳送至其中的頁面處理常式方法的參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="181f0-137">控制站的公用屬性或 `PageModel` 類別，如由屬性指定。</span><span class="sxs-lookup"><span data-stu-id="181f0-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="181f0-138">[BindProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-138">[BindProperty] attribute</span></span>

<span data-ttu-id="181f0-139">可以套用至控制器或`PageModel`類別的公用屬性，使模型系結以該屬性為目標：</span><span class="sxs-lookup"><span data-stu-id="181f0-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="181f0-140">[BindProperties] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-140">[BindProperties] attribute</span></span>

<span data-ttu-id="181f0-141">適用于 ASP.NET Core 2.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="181f0-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="181f0-142">可以套用至控制器或`PageModel`類別，以告知模型系結以類別的所有公用屬性為目標：</span><span class="sxs-lookup"><span data-stu-id="181f0-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="181f0-143">適用於 HTTP GET 要求的模型繫結</span><span class="sxs-lookup"><span data-stu-id="181f0-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="181f0-144">根據預設，屬性不會針對 HTTP GET 要求繫結。</span><span class="sxs-lookup"><span data-stu-id="181f0-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="181f0-145">一般而言，GET 要求只需要記錄識別碼參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="181f0-146">此記錄識別碼用來查詢資料庫中的項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="181f0-147">因此，不需要系結保存模型實例的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="181f0-148">在您要從 GET 要求將屬性繫結至資料的案例中，請將 `SupportsGet` 屬性設為 `true`：</span><span class="sxs-lookup"><span data-stu-id="181f0-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="181f0-149">來源</span><span class="sxs-lookup"><span data-stu-id="181f0-149">Sources</span></span>

<span data-ttu-id="181f0-150">根據預設，模型繫結會從下列 HTTP 要求的來源中，取得索引鍵/值組形式的資料：</span><span class="sxs-lookup"><span data-stu-id="181f0-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="181f0-151">表單欄位</span><span class="sxs-lookup"><span data-stu-id="181f0-151">Form fields</span></span>
1. <span data-ttu-id="181f0-152">要求主體（針對[具有 [ApiController] 屬性的控制器](xref:web-api/index#binding-source-parameter-inference)）。</span><span class="sxs-lookup"><span data-stu-id="181f0-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="181f0-153">路由傳送資料</span><span class="sxs-lookup"><span data-stu-id="181f0-153">Route data</span></span>
1. <span data-ttu-id="181f0-154">查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="181f0-154">Query string parameters</span></span>
1. <span data-ttu-id="181f0-155">已上傳的檔案</span><span class="sxs-lookup"><span data-stu-id="181f0-155">Uploaded files</span></span>

<span data-ttu-id="181f0-156">針對每個目標參數或屬性，系統會依照上述清單中所示的順序來掃描來源。</span><span class="sxs-lookup"><span data-stu-id="181f0-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="181f0-157">但也有一些例外：</span><span class="sxs-lookup"><span data-stu-id="181f0-157">There are a few exceptions:</span></span>

* <span data-ttu-id="181f0-158">路由資料和查詢字串值只用於簡單型別。</span><span class="sxs-lookup"><span data-stu-id="181f0-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="181f0-159">上傳的檔案只會系結至執行`IFormFile`或`IEnumerable<IFormFile>`的目標型別。</span><span class="sxs-lookup"><span data-stu-id="181f0-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="181f0-160">如果預設來源不正確，請使用下列其中一個屬性來指定來源：</span><span class="sxs-lookup"><span data-stu-id="181f0-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="181f0-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)-取得查詢字串中的值。</span><span class="sxs-lookup"><span data-stu-id="181f0-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="181f0-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)-從路由資料取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="181f0-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)-從已張貼的表單欄位取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="181f0-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)-從要求主體取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="181f0-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)-從 HTTP 標頭取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="181f0-166">這些屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-166">These attributes:</span></span>

* <span data-ttu-id="181f0-167">會個別新增至模型屬性 (不是模型類別)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="181f0-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="181f0-168">選擇性地接受在此函式中的模型名稱值。</span><span class="sxs-lookup"><span data-stu-id="181f0-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="181f0-169">如果屬性名稱不符合要求中的值，則會提供此選項。</span><span class="sxs-lookup"><span data-stu-id="181f0-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="181f0-170">例如，要求中的值可能是名稱中有連字號的標頭，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="181f0-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="181f0-171">[FromBody] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-171">[FromBody] attribute</span></span>

<span data-ttu-id="181f0-172">將`[FromBody]`屬性套用至參數，以從 HTTP 要求的主體填入其屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="181f0-173">ASP.NET Core 執行時間會將讀取主體的責任委派給輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="181f0-174">[本文稍後](#input-formatters)會說明輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="181f0-175">當`[FromBody]`套用至複雜型別參數時，會忽略套用至其屬性的任何系結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="181f0-176">例如，下列`Create`動作會指定其`pet`參數是從主體填入：</span><span class="sxs-lookup"><span data-stu-id="181f0-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="181f0-177">`Pet`類別會指定其`Breed`屬性是從查詢字串參數填入：</span><span class="sxs-lookup"><span data-stu-id="181f0-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="181f0-178">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="181f0-178">In the preceding example:</span></span>

* <span data-ttu-id="181f0-179">已`[FromQuery]`忽略屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="181f0-180">不`Breed`會從查詢字串參數填入屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="181f0-181">輸入格式器只會讀取主體，而不會瞭解系結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="181f0-182">如果在主體中找到適當的值，該值會用來填入`Breed`屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="181f0-183">針對每`[FromBody]`個動作方法，請勿套用至一個以上的參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="181f0-184">一旦輸入格式器讀取要求資料流程之後，就無法再讀取它來系結其他`[FromBody]`參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="181f0-185">其他來源</span><span class="sxs-lookup"><span data-stu-id="181f0-185">Additional sources</span></span>

<span data-ttu-id="181f0-186">*值提供者*會將來源資料提供給模型系結系統。</span><span class="sxs-lookup"><span data-stu-id="181f0-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="181f0-187">您可以撰寫並註冊自訂值提供者，其從其他來源取得模型繫結資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="181f0-188">例如，您可能想要來自 cookie 或會話狀態的資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="181f0-189">若要從新的來源取得資料：</span><span class="sxs-lookup"><span data-stu-id="181f0-189">To get data from a new source:</span></span>

* <span data-ttu-id="181f0-190">建立會實作 `IValueProvider` 的類別。</span><span class="sxs-lookup"><span data-stu-id="181f0-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="181f0-191">建立會實作 `IValueProviderFactory` 的類別。</span><span class="sxs-lookup"><span data-stu-id="181f0-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="181f0-192">在 `Startup.ConfigureServices` 中註冊 Factory 類別。</span><span class="sxs-lookup"><span data-stu-id="181f0-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="181f0-193">範例應用程式包含[值提供者](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs)和從 Cookie 取得值的 [actory](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) 範例。</span><span class="sxs-lookup"><span data-stu-id="181f0-193">The sample app includes a [value provider](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="181f0-194">以下是 `Startup.ConfigureServices` 中的註冊碼：</span><span class="sxs-lookup"><span data-stu-id="181f0-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4)]

<span data-ttu-id="181f0-195">顯示的程式碼會將自訂值提供者放在所有內建值提供者的後面。</span><span class="sxs-lookup"><span data-stu-id="181f0-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="181f0-196">若要將它設為清單中的第`Insert(0, new CookieValueProviderFactory())`一個， `Add`請呼叫，而不是。</span><span class="sxs-lookup"><span data-stu-id="181f0-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="181f0-197">無模型屬性的來源</span><span class="sxs-lookup"><span data-stu-id="181f0-197">No source for a model property</span></span>

<span data-ttu-id="181f0-198">根據預設，如果找不到模型屬性的值，就不會建立模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="181f0-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="181f0-199">屬性設定為 null 或預設值：</span><span class="sxs-lookup"><span data-stu-id="181f0-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="181f0-200">可為 null 的簡單類型`null`設定為。</span><span class="sxs-lookup"><span data-stu-id="181f0-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="181f0-201">不可為 Null 的實值型別會設為 `default(T)`。</span><span class="sxs-lookup"><span data-stu-id="181f0-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="181f0-202">例如，參數 `int id` 設為 0。</span><span class="sxs-lookup"><span data-stu-id="181f0-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="181f0-203">針對複雜型別，模型系結會使用預設的函式建立實例，而不會設定屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="181f0-204">陣列設為 `Array.Empty<T>()`，但 `byte[]` 陣列設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="181f0-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="181f0-205">如果在模型屬性的表單欄位中找不到任何內容時，模型狀態應該失效， [`[BindRequired]`](#bindrequired-attribute)請使用屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="181f0-206">請注意，此 `[BindRequired]` 行為適用於來自已張貼表單資料的模型繫結，不適合要求本文中的 JSON 或 XML 資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="181f0-207">[輸入](#input-formatters)格式器會處理要求主體資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="181f0-208">類型轉換錯誤</span><span class="sxs-lookup"><span data-stu-id="181f0-208">Type conversion errors</span></span>

<span data-ttu-id="181f0-209">如果找到來源，但無法轉換成目標型別，則模型狀態會標示為無效。</span><span class="sxs-lookup"><span data-stu-id="181f0-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="181f0-210">目標參數或屬性會設為 null 或預設值，如上一節中所述。</span><span class="sxs-lookup"><span data-stu-id="181f0-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="181f0-211">在具有 `[ApiController]` 屬性的 API 控制器中，無效的模型狀態會導致自動 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="181f0-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="181f0-212">在Razor頁面中，重新顯示頁面，並出現錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="181f0-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="181f0-213">用戶端驗證會攔截大部分不正確的資料，否則會提交至Razor頁面表單。</span><span class="sxs-lookup"><span data-stu-id="181f0-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="181f0-214">此驗證會讓您更難觸發上述醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="181f0-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="181f0-215">範例應用程式包含 [**具有無效日期的提交**] 按鈕，它會在 [**雇用日期**] 欄位中放入不正確的資料，並提交表單。</span><span class="sxs-lookup"><span data-stu-id="181f0-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="181f0-216">此按鈕會顯示當資料轉換錯誤發生時，重新顯示頁面的程式碼如何運作。</span><span class="sxs-lookup"><span data-stu-id="181f0-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="181f0-217">當上述程式碼重新顯示頁面時，不正確輸入不會顯示在 [表單] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="181f0-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="181f0-218">這是因為模型屬性已設為 null 或預設值。</span><span class="sxs-lookup"><span data-stu-id="181f0-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="181f0-219">無效的輸入確實會出現在錯誤訊息中。</span><span class="sxs-lookup"><span data-stu-id="181f0-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="181f0-220">但是，如果您想要在表單欄位中重新顯示不正確的資料，請考慮讓模型屬性成為字串，以手動方式執行資料轉換。</span><span class="sxs-lookup"><span data-stu-id="181f0-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="181f0-221">如果您不想要類型轉換錯誤導致模型狀態錯誤，則建議使用相同的策略。</span><span class="sxs-lookup"><span data-stu-id="181f0-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="181f0-222">在此情況下，請將模型屬性設為字串。</span><span class="sxs-lookup"><span data-stu-id="181f0-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="181f0-223">簡單型別</span><span class="sxs-lookup"><span data-stu-id="181f0-223">Simple types</span></span>

<span data-ttu-id="181f0-224">模型繫結器可將來源字串轉換成的簡單型別包括：</span><span class="sxs-lookup"><span data-stu-id="181f0-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="181f0-225">Boolean</span><span class="sxs-lookup"><span data-stu-id="181f0-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="181f0-226">[Byte](xref:System.ComponentModel.ByteConverter)、[SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="181f0-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="181f0-227">Char</span><span class="sxs-lookup"><span data-stu-id="181f0-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="181f0-228">Datetime</span><span class="sxs-lookup"><span data-stu-id="181f0-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="181f0-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="181f0-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="181f0-230">十進位</span><span class="sxs-lookup"><span data-stu-id="181f0-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="181f0-231">兩</span><span class="sxs-lookup"><span data-stu-id="181f0-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="181f0-232">列舉</span><span class="sxs-lookup"><span data-stu-id="181f0-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="181f0-233">Guid</span><span class="sxs-lookup"><span data-stu-id="181f0-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="181f0-234">[Int16](xref:System.ComponentModel.Int16Converter)、[Int32](xref:System.ComponentModel.Int32Converter)、[Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="181f0-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="181f0-235">Single</span><span class="sxs-lookup"><span data-stu-id="181f0-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="181f0-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="181f0-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="181f0-237">[UInt16](xref:System.ComponentModel.UInt16Converter)、[UInt32](xref:System.ComponentModel.UInt32Converter)、[UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="181f0-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="181f0-238">Uri</span><span class="sxs-lookup"><span data-stu-id="181f0-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="181f0-239">版本</span><span class="sxs-lookup"><span data-stu-id="181f0-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="181f0-240">複雜類型</span><span class="sxs-lookup"><span data-stu-id="181f0-240">Complex types</span></span>

<span data-ttu-id="181f0-241">複雜型別必須具有公用預設的函式，以及要系結的公用可寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="181f0-242">發生模型繫結時，類別會使用公用預設建構函式具現化。</span><span class="sxs-lookup"><span data-stu-id="181f0-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="181f0-243">針對複雜類型的每個屬性，模型繫結會查看名稱模式 *prefix.property_name* 的來源。</span><span class="sxs-lookup"><span data-stu-id="181f0-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="181f0-244">如果找不到，它會只尋找沒有前置詞的 *property_name*。</span><span class="sxs-lookup"><span data-stu-id="181f0-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="181f0-245">若要繫結至參數，則前置詞是參數名稱。</span><span class="sxs-lookup"><span data-stu-id="181f0-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="181f0-246">若要繫結至 `PageModel` 公用屬性，則前置詞為公用屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="181f0-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="181f0-247">某些屬性 (Attribute) 具有 `Prefix` 屬性 (Property)，其可讓您覆寫參數或屬性名稱的預設使用方法。</span><span class="sxs-lookup"><span data-stu-id="181f0-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="181f0-248">例如，假設複雜類型是下列 `Instructor` 類別：</span><span class="sxs-lookup"><span data-stu-id="181f0-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="181f0-249">前置詞 = 參數名稱</span><span class="sxs-lookup"><span data-stu-id="181f0-249">Prefix = parameter name</span></span>

<span data-ttu-id="181f0-250">如果要繫結的模型是名為 `instructorToUpdate` 的參數：</span><span class="sxs-lookup"><span data-stu-id="181f0-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="181f0-251">模型繫結從查看索引鍵 `instructorToUpdate.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="181f0-252">如果找不到，它會尋找`ID`不含前置詞的。</span><span class="sxs-lookup"><span data-stu-id="181f0-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="181f0-253">前置詞 = 屬性名稱</span><span class="sxs-lookup"><span data-stu-id="181f0-253">Prefix = property name</span></span>

<span data-ttu-id="181f0-254">如果要繫結的模型是控制器或 `PageModel` 類別名為 `Instructor` 的屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="181f0-255">模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="181f0-256">如果找不到，它會尋找`ID`不含前置詞的。</span><span class="sxs-lookup"><span data-stu-id="181f0-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="181f0-257">自訂前置詞</span><span class="sxs-lookup"><span data-stu-id="181f0-257">Custom prefix</span></span>

<span data-ttu-id="181f0-258">如果要繫結的模型是名為 `instructorToUpdate` 的參數，且 `Bind` 屬性指定 `Instructor` 為前置詞：</span><span class="sxs-lookup"><span data-stu-id="181f0-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="181f0-259">模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="181f0-260">如果找不到，它會尋找`ID`不含前置詞的。</span><span class="sxs-lookup"><span data-stu-id="181f0-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="181f0-261">複雜類型目標的屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-261">Attributes for complex type targets</span></span>

<span data-ttu-id="181f0-262">有數個內建屬性可用來控制複雜類型的模型系結：</span><span class="sxs-lookup"><span data-stu-id="181f0-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="181f0-263">當張貼的表單資料為值來源時，這些屬性會影響模型繫結。</span><span class="sxs-lookup"><span data-stu-id="181f0-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="181f0-264">它們不會影響處理已張貼 JSON 和 XML 要求本文的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="181f0-265">[本文稍後](#input-formatters)會說明輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="181f0-266">另請參閱[模型驗證](xref:mvc/models/validation#required-attribute)中`[Required]`的屬性討論。</span><span class="sxs-lookup"><span data-stu-id="181f0-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="181f0-267">[BindRequired] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-267">[BindRequired] attribute</span></span>

<span data-ttu-id="181f0-268">只能套用至模型屬性，不能套用到方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="181f0-269">如果模型的屬性不能發生繫結，則會造成模型繫結新增模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="181f0-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="181f0-270">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="181f0-271">[BindNever] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-271">[BindNever] attribute</span></span>

<span data-ttu-id="181f0-272">只能套用至模型屬性，不能套用到方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="181f0-273">避免模型繫結設定模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="181f0-274">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="181f0-275">[Bind] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-275">[Bind] attribute</span></span>

<span data-ttu-id="181f0-276">可以套用至類別或方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="181f0-277">指定模型繫結應包含哪些模型屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="181f0-278">在下列範例中，當呼叫任何處理常式或動作方法時，只會繫結 `Instructor` 模型的指定屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="181f0-279">在下列範例中，當呼叫 `OnPost` 方法時，只會繫結 `Instructor` 模型的指定屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="181f0-280">`[Bind]` 屬性可用來防止「建立」\*\* 案例中的大量指派。</span><span class="sxs-lookup"><span data-stu-id="181f0-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="181f0-281">它不適用於編輯案例，因為排除的屬性會設定為 null 或預設值，而不會保持不變。</span><span class="sxs-lookup"><span data-stu-id="181f0-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="181f0-282">建議使用檢視模型而非 `[Bind]` 屬性來防禦大量指派。</span><span class="sxs-lookup"><span data-stu-id="181f0-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="181f0-283">如需詳細資訊，請參閱[關於防止大量指派的安全性注意事項](xref:data/ef-mvc/crud#security-note-about-overposting)。</span><span class="sxs-lookup"><span data-stu-id="181f0-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="181f0-284">集合</span><span class="sxs-lookup"><span data-stu-id="181f0-284">Collections</span></span>

<span data-ttu-id="181f0-285">針對簡單型別集合的目標，模型繫結會尋找符合 *parameter_name* 或 *property_name* 的項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="181f0-286">如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。</span><span class="sxs-lookup"><span data-stu-id="181f0-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="181f0-287">例如：</span><span class="sxs-lookup"><span data-stu-id="181f0-287">For example:</span></span>

* <span data-ttu-id="181f0-288">假設要繫結的參數是名為 `selectedCourses` 的陣列：</span><span class="sxs-lookup"><span data-stu-id="181f0-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="181f0-289">表單或查詢字串資料可以是下列其中一種格式：</span><span class="sxs-lookup"><span data-stu-id="181f0-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="181f0-290">下列格式僅在表單資料中受到支援：</span><span class="sxs-lookup"><span data-stu-id="181f0-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="181f0-291">針對前列所有範例格式，模型繫結會將有兩個項目的陣列傳遞至 `selectedCourses` 參數：</span><span class="sxs-lookup"><span data-stu-id="181f0-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="181f0-292">selectedCourses [0] = 1050</span><span class="sxs-lookup"><span data-stu-id="181f0-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="181f0-293">selectedCourses [1] = 2000</span><span class="sxs-lookup"><span data-stu-id="181f0-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="181f0-294">使用注標編號的資料格式（.。。[0] .。。[1] ...）必須確定它們的編號順序是從零開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="181f0-295">下標編號中如有任何間距，則會忽略間隔後的所有項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="181f0-296">例如，如果下標是 0 和 2，而不是 0 和 1，則忽略第二個項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="181f0-297">字典</span><span class="sxs-lookup"><span data-stu-id="181f0-297">Dictionaries</span></span>

<span data-ttu-id="181f0-298">針對`Dictionary`目標，模型系結會尋找與*parameter_name*或*property_name*相符的專案。</span><span class="sxs-lookup"><span data-stu-id="181f0-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="181f0-299">如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。</span><span class="sxs-lookup"><span data-stu-id="181f0-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="181f0-300">例如：</span><span class="sxs-lookup"><span data-stu-id="181f0-300">For example:</span></span>

* <span data-ttu-id="181f0-301">假設目標參數是名為`Dictionary<int, string>` `selectedCourses`的：</span><span class="sxs-lookup"><span data-stu-id="181f0-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="181f0-302">已張貼的表單或查詢字串資料看起來會像下列其中一個範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="181f0-303">針對前列所有範例格式，模型繫結會將有兩個項目的字典傳遞至 `selectedCourses` 參數：</span><span class="sxs-lookup"><span data-stu-id="181f0-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="181f0-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="181f0-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="181f0-305">selectedCourses ["2000"] = "經濟效益"</span><span class="sxs-lookup"><span data-stu-id="181f0-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="181f0-306">模型系結路由資料和查詢字串的全球化行為</span><span class="sxs-lookup"><span data-stu-id="181f0-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="181f0-307">ASP.NET Core 路由值提供者和查詢字串值提供者：</span><span class="sxs-lookup"><span data-stu-id="181f0-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="181f0-308">將值視為不因文化特性而異。</span><span class="sxs-lookup"><span data-stu-id="181f0-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="181f0-309">Url 會預期文化特性不變。</span><span class="sxs-lookup"><span data-stu-id="181f0-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="181f0-310">相反地，來自表單資料的值會經歷區分文化特性的轉換。</span><span class="sxs-lookup"><span data-stu-id="181f0-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="181f0-311">這是根據設計，讓 Url 可跨地區設定進行共用。</span><span class="sxs-lookup"><span data-stu-id="181f0-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="181f0-312">若要讓 ASP.NET Core route 值提供者和查詢字串值提供者進行區分文化特性的轉換：</span><span class="sxs-lookup"><span data-stu-id="181f0-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="181f0-313">繼承自 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span><span class="sxs-lookup"><span data-stu-id="181f0-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="181f0-314">從[QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs)或[RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)複製程式碼</span><span class="sxs-lookup"><span data-stu-id="181f0-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="181f0-315">以 CurrentCulture 取代傳遞至值提供者的[文化特性值](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) [。](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="181f0-315">Replace the [culture value](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="181f0-316">將 MVC 選項中的預設值提供者 factory 取代為新的值：</span><span class="sxs-lookup"><span data-stu-id="181f0-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="181f0-317">特殊資料類型</span><span class="sxs-lookup"><span data-stu-id="181f0-317">Special data types</span></span>

<span data-ttu-id="181f0-318">模型繫結可以處理某些特殊資料類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="181f0-319">IFormFile 和 IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="181f0-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="181f0-320">HTTP 要求包含上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="181f0-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="181f0-321">也支援多個檔案的 `IEnumerable<IFormFile>`。</span><span class="sxs-lookup"><span data-stu-id="181f0-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="181f0-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="181f0-322">CancellationToken</span></span>

<span data-ttu-id="181f0-323">用來取消非同步控制器中的活動。</span><span class="sxs-lookup"><span data-stu-id="181f0-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="181f0-324">FormCollection</span><span class="sxs-lookup"><span data-stu-id="181f0-324">FormCollection</span></span>

<span data-ttu-id="181f0-325">用來擷取已張貼表單資料中的所有值。</span><span class="sxs-lookup"><span data-stu-id="181f0-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="181f0-326">輸入格式器</span><span class="sxs-lookup"><span data-stu-id="181f0-326">Input formatters</span></span>

<span data-ttu-id="181f0-327">要求主體中的資料可以是 JSON、XML 或其他格式。</span><span class="sxs-lookup"><span data-stu-id="181f0-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="181f0-328">模型繫結會使用設定處理特定內容類型的「輸入格式器」\*\*，來剖析此資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="181f0-329">根據預設，ASP.NET Core 包括用來處理 JSON 資料的 JSON 型輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="181f0-330">您可以為其他內容類型新增其他格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="181f0-331">ASP.NET Core 選取以 [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) 屬性為基礎的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="181f0-332">若無任何屬性，則它會使用 [Content-Type 標頭](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)。</span><span class="sxs-lookup"><span data-stu-id="181f0-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="181f0-333">使用內建的 XML 輸入格式器：</span><span class="sxs-lookup"><span data-stu-id="181f0-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="181f0-334">安裝 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="181f0-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="181f0-335">在 `Startup.ConfigureServices` 中，呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> 或 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>。</span><span class="sxs-lookup"><span data-stu-id="181f0-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=10)]

* <span data-ttu-id="181f0-336">將 `Consumes` 屬性套用至要求本文應為 XML 的控制器類別或動作方法。</span><span class="sxs-lookup"><span data-stu-id="181f0-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="181f0-337">如需詳細資訊，請參閱[XML 序列化簡介](/dotnet/standard/serialization/introducing-xml-serialization)。</span><span class="sxs-lookup"><span data-stu-id="181f0-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

### <a name="customize-model-binding-with-input-formatters"></a><span data-ttu-id="181f0-338">使用輸入格式器自訂模型系結</span><span class="sxs-lookup"><span data-stu-id="181f0-338">Customize model binding with input formatters</span></span>

<span data-ttu-id="181f0-339">輸入格式器會完全負責從要求主體讀取資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-339">An input formatter takes full responsibility for reading data from the request body.</span></span> <span data-ttu-id="181f0-340">若要自訂此進程，請設定輸入格式器所使用的 Api。</span><span class="sxs-lookup"><span data-stu-id="181f0-340">To customize this process, configure the APIs used by the input formatter.</span></span> <span data-ttu-id="181f0-341">本節說明如何自訂以為`System.Text.Json`基礎的輸入格式器，以瞭解名為`ObjectId`的自訂類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-341">This section describes how to customize the `System.Text.Json`-based input formatter to understand a custom type named `ObjectId`.</span></span> 

<span data-ttu-id="181f0-342">請考慮下列模型，其中包含名為`ObjectId` `Id`的自訂屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-342">Consider the following model, which contains a custom `ObjectId` property named `Id`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ModelWithObjectId.cs?name=snippet_Class&highlight=3)]

<span data-ttu-id="181f0-343">若要在使用`System.Text.Json`時自訂模型系結程式，請建立<xref:System.Text.Json.Serialization.JsonConverter%601>衍生自的類別：</span><span class="sxs-lookup"><span data-stu-id="181f0-343">To customize the model binding process when using `System.Text.Json`, create a class derived from <xref:System.Text.Json.Serialization.JsonConverter%601>:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/JsonConverters/ObjectIdConverter.cs?name=snippet_Class)]

<span data-ttu-id="181f0-344">若要使用自訂轉換器，請<xref:System.Text.Json.Serialization.JsonConverterAttribute>將屬性套用至類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-344">To use a custom converter, apply the <xref:System.Text.Json.Serialization.JsonConverterAttribute> attribute to the type.</span></span> <span data-ttu-id="181f0-345">在下列範例中， `ObjectId`類型是以`ObjectIdConverter`做為其自訂轉換器來設定：</span><span class="sxs-lookup"><span data-stu-id="181f0-345">In the following example, the `ObjectId` type is configured with `ObjectIdConverter` as its custom converter:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ObjectId.cs?name=snippet_Class&highlight=1)]

<span data-ttu-id="181f0-346">如需詳細資訊，請參閱[如何撰寫自訂轉換器](/dotnet/standard/serialization/system-text-json-converters-how-to)。</span><span class="sxs-lookup"><span data-stu-id="181f0-346">For more information, see [How to write custom converters](/dotnet/standard/serialization/system-text-json-converters-how-to).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="181f0-347">排除模型繫結中的指定類型</span><span class="sxs-lookup"><span data-stu-id="181f0-347">Exclude specified types from model binding</span></span>

<span data-ttu-id="181f0-348">模型繫結和驗證系統的行為是由 [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) 所驅動。</span><span class="sxs-lookup"><span data-stu-id="181f0-348">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="181f0-349">您可以將詳細資料提供者新增至 [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)，藉以自訂 `ModelMetadata`。</span><span class="sxs-lookup"><span data-stu-id="181f0-349">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="181f0-350">內建的詳細資料提供者可用於停用模型繫結或驗證所指定類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-350">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="181f0-351">若要停用指定類型之所有模型的模型繫結，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider>。</span><span class="sxs-lookup"><span data-stu-id="181f0-351">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="181f0-352">例如，若要對類型為 `System.Version` 的所有模型停用模型繫結：</span><span class="sxs-lookup"><span data-stu-id="181f0-352">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=5-6)]

<span data-ttu-id="181f0-353">若要停用指定類型屬性的驗證，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider>。</span><span class="sxs-lookup"><span data-stu-id="181f0-353">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="181f0-354">例如，若要針對類型為 `System.Guid` 的屬性停用驗證：</span><span class="sxs-lookup"><span data-stu-id="181f0-354">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=7-8)]

## <a name="custom-model-binders"></a><span data-ttu-id="181f0-355">自訂模型繫結器</span><span class="sxs-lookup"><span data-stu-id="181f0-355">Custom model binders</span></span>

<span data-ttu-id="181f0-356">您可以撰寫自訂的模型繫結器來擴充模型繫結，並使用 `[ModelBinder]` 屬性以針對特定目標來選取它。</span><span class="sxs-lookup"><span data-stu-id="181f0-356">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="181f0-357">深入了解[自訂模型繫結](xref:mvc/advanced/custom-model-binding)。</span><span class="sxs-lookup"><span data-stu-id="181f0-357">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="181f0-358">手動模型系結</span><span class="sxs-lookup"><span data-stu-id="181f0-358">Manual model binding</span></span> 

<span data-ttu-id="181f0-359">使用 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> 方法即可手動叫用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="181f0-359">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="181f0-360">此方法已於 `ControllerBase` 和 `PageModel` 類別中定義。</span><span class="sxs-lookup"><span data-stu-id="181f0-360">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="181f0-361">方法多載可讓您指定要使用的前置詞和值提供者。</span><span class="sxs-lookup"><span data-stu-id="181f0-361">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="181f0-362">如果模型繫結失敗，此方法會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="181f0-362">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="181f0-363">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-363">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

<span data-ttu-id="181f0-364"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>使用值提供者從表單主體、查詢字串和路由資料取得資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-364"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>  uses value providers to get data from the form body, query string, and route data.</span></span> <span data-ttu-id="181f0-365">`TryUpdateModelAsync`通常是：</span><span class="sxs-lookup"><span data-stu-id="181f0-365">`TryUpdateModelAsync` is typically:</span></span> 

* <span data-ttu-id="181f0-366">搭配Razor頁面和 MVC 應用程式使用控制器和視圖，以防止過度張貼。</span><span class="sxs-lookup"><span data-stu-id="181f0-366">Used with Razor Pages and MVC apps using controllers and views to prevent over-posting.</span></span>
* <span data-ttu-id="181f0-367">除非從表單資料、查詢字串和路由資料取用，否則不會與 Web API 搭配使用。</span><span class="sxs-lookup"><span data-stu-id="181f0-367">Not used with a web API unless consumed from form data, query strings, and route data.</span></span> <span data-ttu-id="181f0-368">取用 JSON 的 Web API 端點會使用[輸入](#input-formatters)格式器，將要求本文還原序列化為物件。</span><span class="sxs-lookup"><span data-stu-id="181f0-368">Web API endpoints that consume JSON use [Input formatters](#input-formatters) to deserialize the request body into an object.</span></span>

<span data-ttu-id="181f0-369">如需詳細資訊，請參閱[TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync)。</span><span class="sxs-lookup"><span data-stu-id="181f0-369">For more information, see [TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync).</span></span>

## <a name="fromservices-attribute"></a><span data-ttu-id="181f0-370">[FromServices] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-370">[FromServices] attribute</span></span>

<span data-ttu-id="181f0-371">這個屬性的名稱會遵循指定資料來源之模型系結屬性的模式。</span><span class="sxs-lookup"><span data-stu-id="181f0-371">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="181f0-372">但它並不是關於從值提供者系結資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-372">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="181f0-373">它從[相依性插入](xref:fundamentals/dependency-injection)容器取得類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="181f0-373">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="181f0-374">其目的是只有在呼叫特定方法時，才在您需要服務時提供建構函式插入的替代項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-374">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="181f0-375">其他資源</span><span class="sxs-lookup"><span data-stu-id="181f0-375">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="181f0-376">本文會說明何謂模型繫結、其運作方式，以及如何自訂其行為。</span><span class="sxs-lookup"><span data-stu-id="181f0-376">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="181f0-377">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="181f0-377">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="181f0-378">何謂模型繫結</span><span class="sxs-lookup"><span data-stu-id="181f0-378">What is Model binding</span></span>

<span data-ttu-id="181f0-379">控制器和Razor頁面可處理來自 HTTP 要求的資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-379">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="181f0-380">例如，路由資料可能會提供記錄索引鍵，而已張貼的表單欄位可能會提供模型屬性的值。</span><span class="sxs-lookup"><span data-stu-id="181f0-380">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="181f0-381">撰寫程式碼來擷取這些值的每一個並將它們從字串轉換成 .NET 類型，不但繁瑣又容易發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="181f0-381">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="181f0-382">模型繫結會自動化此程序。</span><span class="sxs-lookup"><span data-stu-id="181f0-382">Model binding automates this process.</span></span> <span data-ttu-id="181f0-383">模型繫結系統：</span><span class="sxs-lookup"><span data-stu-id="181f0-383">The model binding system:</span></span>

* <span data-ttu-id="181f0-384">從各種不同的來源（例如，路由資料、表單欄位和查詢字串）抓取資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-384">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="181f0-385">將資料提供給方法參數Razor和公用屬性中的控制器和頁面。</span><span class="sxs-lookup"><span data-stu-id="181f0-385">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="181f0-386">將字串資料轉換成 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-386">Converts string data to .NET types.</span></span>
* <span data-ttu-id="181f0-387">更新複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-387">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="181f0-388">範例</span><span class="sxs-lookup"><span data-stu-id="181f0-388">Example</span></span>

<span data-ttu-id="181f0-389">假設您有下列動作方法：</span><span class="sxs-lookup"><span data-stu-id="181f0-389">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="181f0-390">而應用程式收到具有此 URL 的要求：</span><span class="sxs-lookup"><span data-stu-id="181f0-390">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="181f0-391">在路由系統選取動作方法之後，模型系結會經歷下列步驟：</span><span class="sxs-lookup"><span data-stu-id="181f0-391">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="181f0-392">尋找第一個參數 `GetByID`，它是名為 `id` 的整數。</span><span class="sxs-lookup"><span data-stu-id="181f0-392">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="181f0-393">查看 HTTP 要求中所有可用的來源，在路由資料中找到 `id` = "2"。</span><span class="sxs-lookup"><span data-stu-id="181f0-393">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="181f0-394">將字串 "2" 轉換成整數 2。</span><span class="sxs-lookup"><span data-stu-id="181f0-394">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="181f0-395">尋找的下一個參數`GetByID`，也就是`dogsOnly`名為的布林值。</span><span class="sxs-lookup"><span data-stu-id="181f0-395">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="181f0-396">查看來源，在查詢字串中找到 "DogsOnly=true"。</span><span class="sxs-lookup"><span data-stu-id="181f0-396">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="181f0-397">名稱比對不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="181f0-397">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="181f0-398">將字串 "true" 轉換成布林`true`值。</span><span class="sxs-lookup"><span data-stu-id="181f0-398">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="181f0-399">架構接著會呼叫 `GetById` 方法，針對 `id` 參數傳送 2、`dogsOnly` 參數傳送 `true`。</span><span class="sxs-lookup"><span data-stu-id="181f0-399">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="181f0-400">在上例中，模型繫結目標都是簡單型別的方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-400">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="181f0-401">目標也可以是複雜類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-401">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="181f0-402">成功系結每個屬性之後，就會針對該屬性進行[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="181f0-402">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="181f0-403">哪些資料繫結至模型，以及任何繫結或驗證錯誤的記錄，都會儲存在 [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) 或 [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)。</span><span class="sxs-lookup"><span data-stu-id="181f0-403">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="181f0-404">為了解此程序是否成功，應用程式會檢查 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) 旗標。</span><span class="sxs-lookup"><span data-stu-id="181f0-404">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="181f0-405">目標</span><span class="sxs-lookup"><span data-stu-id="181f0-405">Targets</span></span>

<span data-ttu-id="181f0-406">模型繫結會嘗試尋找下列幾種目標的值：</span><span class="sxs-lookup"><span data-stu-id="181f0-406">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="181f0-407">要求路由目標的控制器動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-407">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="181f0-408">將要求路由Razor傳送至其中的頁面處理常式方法的參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-408">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="181f0-409">控制站的公用屬性或 `PageModel` 類別，如由屬性指定。</span><span class="sxs-lookup"><span data-stu-id="181f0-409">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="181f0-410">[BindProperty] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-410">[BindProperty] attribute</span></span>

<span data-ttu-id="181f0-411">可以套用至控制器或`PageModel`類別的公用屬性，使模型系結以該屬性為目標：</span><span class="sxs-lookup"><span data-stu-id="181f0-411">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="181f0-412">[BindProperties] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-412">[BindProperties] attribute</span></span>

<span data-ttu-id="181f0-413">適用于 ASP.NET Core 2.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="181f0-413">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="181f0-414">可以套用至控制器或`PageModel`類別，以告知模型系結以類別的所有公用屬性為目標：</span><span class="sxs-lookup"><span data-stu-id="181f0-414">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="181f0-415">適用於 HTTP GET 要求的模型繫結</span><span class="sxs-lookup"><span data-stu-id="181f0-415">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="181f0-416">根據預設，屬性不會針對 HTTP GET 要求繫結。</span><span class="sxs-lookup"><span data-stu-id="181f0-416">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="181f0-417">一般而言，GET 要求只需要記錄識別碼參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-417">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="181f0-418">此記錄識別碼用來查詢資料庫中的項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-418">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="181f0-419">因此，不需要系結保存模型實例的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-419">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="181f0-420">在您要從 GET 要求將屬性繫結至資料的案例中，請將 `SupportsGet` 屬性設為 `true`：</span><span class="sxs-lookup"><span data-stu-id="181f0-420">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="181f0-421">來源</span><span class="sxs-lookup"><span data-stu-id="181f0-421">Sources</span></span>

<span data-ttu-id="181f0-422">根據預設，模型繫結會從下列 HTTP 要求的來源中，取得索引鍵/值組形式的資料：</span><span class="sxs-lookup"><span data-stu-id="181f0-422">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="181f0-423">表單欄位</span><span class="sxs-lookup"><span data-stu-id="181f0-423">Form fields</span></span>
1. <span data-ttu-id="181f0-424">要求主體（針對[具有 [ApiController] 屬性的控制器](xref:web-api/index#binding-source-parameter-inference)）。</span><span class="sxs-lookup"><span data-stu-id="181f0-424">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="181f0-425">路由傳送資料</span><span class="sxs-lookup"><span data-stu-id="181f0-425">Route data</span></span>
1. <span data-ttu-id="181f0-426">查詢字串參數</span><span class="sxs-lookup"><span data-stu-id="181f0-426">Query string parameters</span></span>
1. <span data-ttu-id="181f0-427">已上傳的檔案</span><span class="sxs-lookup"><span data-stu-id="181f0-427">Uploaded files</span></span>

<span data-ttu-id="181f0-428">針對每個目標參數或屬性，系統會依照上述清單中所示的順序來掃描來源。</span><span class="sxs-lookup"><span data-stu-id="181f0-428">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="181f0-429">但也有一些例外：</span><span class="sxs-lookup"><span data-stu-id="181f0-429">There are a few exceptions:</span></span>

* <span data-ttu-id="181f0-430">路由資料和查詢字串值只用於簡單型別。</span><span class="sxs-lookup"><span data-stu-id="181f0-430">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="181f0-431">上傳的檔案只會系結至執行`IFormFile`或`IEnumerable<IFormFile>`的目標型別。</span><span class="sxs-lookup"><span data-stu-id="181f0-431">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="181f0-432">如果預設來源不正確，請使用下列其中一個屬性來指定來源：</span><span class="sxs-lookup"><span data-stu-id="181f0-432">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="181f0-433">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)-取得查詢字串中的值。</span><span class="sxs-lookup"><span data-stu-id="181f0-433">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="181f0-434">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)-從路由資料取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-434">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="181f0-435">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)-從已張貼的表單欄位取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-435">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="181f0-436">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)-從要求主體取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-436">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="181f0-437">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)-從 HTTP 標頭取得值。</span><span class="sxs-lookup"><span data-stu-id="181f0-437">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="181f0-438">這些屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-438">These attributes:</span></span>

* <span data-ttu-id="181f0-439">會個別新增至模型屬性 (不是模型類別)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="181f0-439">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="181f0-440">選擇性地接受在此函式中的模型名稱值。</span><span class="sxs-lookup"><span data-stu-id="181f0-440">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="181f0-441">如果屬性名稱不符合要求中的值，則會提供此選項。</span><span class="sxs-lookup"><span data-stu-id="181f0-441">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="181f0-442">例如，要求中的值可能是名稱中有連字號的標頭，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="181f0-442">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="181f0-443">[FromBody] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-443">[FromBody] attribute</span></span>

<span data-ttu-id="181f0-444">將`[FromBody]`屬性套用至參數，以從 HTTP 要求的主體填入其屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-444">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="181f0-445">ASP.NET Core 執行時間會將讀取主體的責任委派給輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-445">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="181f0-446">[本文稍後](#input-formatters)會說明輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-446">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="181f0-447">當`[FromBody]`套用至複雜型別參數時，會忽略套用至其屬性的任何系結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-447">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="181f0-448">例如，下列`Create`動作會指定其`pet`參數是從主體填入：</span><span class="sxs-lookup"><span data-stu-id="181f0-448">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="181f0-449">`Pet`類別會指定其`Breed`屬性是從查詢字串參數填入：</span><span class="sxs-lookup"><span data-stu-id="181f0-449">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="181f0-450">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="181f0-450">In the preceding example:</span></span>

* <span data-ttu-id="181f0-451">已`[FromQuery]`忽略屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-451">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="181f0-452">不`Breed`會從查詢字串參數填入屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-452">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="181f0-453">輸入格式器只會讀取主體，而不會瞭解系結來源屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-453">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="181f0-454">如果在主體中找到適當的值，該值會用來填入`Breed`屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-454">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="181f0-455">針對每`[FromBody]`個動作方法，請勿套用至一個以上的參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-455">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="181f0-456">一旦輸入格式器讀取要求資料流程之後，就無法再讀取它來系結其他`[FromBody]`參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-456">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="181f0-457">其他來源</span><span class="sxs-lookup"><span data-stu-id="181f0-457">Additional sources</span></span>

<span data-ttu-id="181f0-458">*值提供者*會將來源資料提供給模型系結系統。</span><span class="sxs-lookup"><span data-stu-id="181f0-458">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="181f0-459">您可以撰寫並註冊自訂值提供者，其從其他來源取得模型繫結資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-459">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="181f0-460">例如，您可能想要來自 cookie 或會話狀態的資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-460">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="181f0-461">若要從新的來源取得資料：</span><span class="sxs-lookup"><span data-stu-id="181f0-461">To get data from a new source:</span></span>

* <span data-ttu-id="181f0-462">建立會實作 `IValueProvider` 的類別。</span><span class="sxs-lookup"><span data-stu-id="181f0-462">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="181f0-463">建立會實作 `IValueProviderFactory` 的類別。</span><span class="sxs-lookup"><span data-stu-id="181f0-463">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="181f0-464">在 `Startup.ConfigureServices` 中註冊 Factory 類別。</span><span class="sxs-lookup"><span data-stu-id="181f0-464">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="181f0-465">範例應用程式包含[值提供者](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs)和從 Cookie 取得值的 [actory](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) 範例。</span><span class="sxs-lookup"><span data-stu-id="181f0-465">The sample app includes a [value provider](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="181f0-466">以下是 `Startup.ConfigureServices` 中的註冊碼：</span><span class="sxs-lookup"><span data-stu-id="181f0-466">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="181f0-467">顯示的程式碼會將自訂值提供者放在所有內建值提供者的後面。</span><span class="sxs-lookup"><span data-stu-id="181f0-467">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="181f0-468">若要將它設為清單中的第`Insert(0, new CookieValueProviderFactory())`一個， `Add`請呼叫，而不是。</span><span class="sxs-lookup"><span data-stu-id="181f0-468">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="181f0-469">無模型屬性的來源</span><span class="sxs-lookup"><span data-stu-id="181f0-469">No source for a model property</span></span>

<span data-ttu-id="181f0-470">根據預設，如果找不到模型屬性的值，就不會建立模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="181f0-470">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="181f0-471">屬性設定為 null 或預設值：</span><span class="sxs-lookup"><span data-stu-id="181f0-471">The property is set to null or a default value:</span></span>

* <span data-ttu-id="181f0-472">可為 null 的簡單類型`null`設定為。</span><span class="sxs-lookup"><span data-stu-id="181f0-472">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="181f0-473">不可為 Null 的實值型別會設為 `default(T)`。</span><span class="sxs-lookup"><span data-stu-id="181f0-473">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="181f0-474">例如，參數 `int id` 設為 0。</span><span class="sxs-lookup"><span data-stu-id="181f0-474">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="181f0-475">針對複雜型別，模型系結會使用預設的函式建立實例，而不會設定屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-475">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="181f0-476">陣列設為 `Array.Empty<T>()`，但 `byte[]` 陣列設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="181f0-476">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="181f0-477">如果在模型屬性的表單欄位中找不到任何內容時，模型狀態應該失效， [`[BindRequired]`](#bindrequired-attribute)請使用屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-477">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="181f0-478">請注意，此 `[BindRequired]` 行為適用於來自已張貼表單資料的模型繫結，不適合要求本文中的 JSON 或 XML 資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-478">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="181f0-479">[輸入](#input-formatters)格式器會處理要求主體資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-479">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="181f0-480">類型轉換錯誤</span><span class="sxs-lookup"><span data-stu-id="181f0-480">Type conversion errors</span></span>

<span data-ttu-id="181f0-481">如果找到來源，但無法轉換成目標型別，則模型狀態會標示為無效。</span><span class="sxs-lookup"><span data-stu-id="181f0-481">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="181f0-482">目標參數或屬性會設為 null 或預設值，如上一節中所述。</span><span class="sxs-lookup"><span data-stu-id="181f0-482">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="181f0-483">在具有 `[ApiController]` 屬性的 API 控制器中，無效的模型狀態會導致自動 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="181f0-483">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="181f0-484">在Razor頁面中，重新顯示頁面，並出現錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="181f0-484">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="181f0-485">用戶端驗證會攔截大部分不正確的資料，否則會提交至Razor頁面表單。</span><span class="sxs-lookup"><span data-stu-id="181f0-485">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="181f0-486">此驗證會讓您更難觸發上述醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="181f0-486">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="181f0-487">範例應用程式包含 [**具有無效日期的提交**] 按鈕，它會在 [**雇用日期**] 欄位中放入不正確的資料，並提交表單。</span><span class="sxs-lookup"><span data-stu-id="181f0-487">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="181f0-488">此按鈕會顯示當資料轉換錯誤發生時，重新顯示頁面的程式碼如何運作。</span><span class="sxs-lookup"><span data-stu-id="181f0-488">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="181f0-489">當上述程式碼重新顯示頁面時，不正確輸入不會顯示在 [表單] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="181f0-489">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="181f0-490">這是因為模型屬性已設為 null 或預設值。</span><span class="sxs-lookup"><span data-stu-id="181f0-490">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="181f0-491">無效的輸入確實會出現在錯誤訊息中。</span><span class="sxs-lookup"><span data-stu-id="181f0-491">The invalid input does appear in an error message.</span></span> <span data-ttu-id="181f0-492">但是，如果您想要在表單欄位中重新顯示不正確的資料，請考慮讓模型屬性成為字串，以手動方式執行資料轉換。</span><span class="sxs-lookup"><span data-stu-id="181f0-492">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="181f0-493">如果您不想要類型轉換錯誤導致模型狀態錯誤，則建議使用相同的策略。</span><span class="sxs-lookup"><span data-stu-id="181f0-493">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="181f0-494">在此情況下，請將模型屬性設為字串。</span><span class="sxs-lookup"><span data-stu-id="181f0-494">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="181f0-495">簡單型別</span><span class="sxs-lookup"><span data-stu-id="181f0-495">Simple types</span></span>

<span data-ttu-id="181f0-496">模型繫結器可將來源字串轉換成的簡單型別包括：</span><span class="sxs-lookup"><span data-stu-id="181f0-496">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="181f0-497">Boolean</span><span class="sxs-lookup"><span data-stu-id="181f0-497">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="181f0-498">[Byte](xref:System.ComponentModel.ByteConverter)、[SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="181f0-498">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="181f0-499">Char</span><span class="sxs-lookup"><span data-stu-id="181f0-499">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="181f0-500">Datetime</span><span class="sxs-lookup"><span data-stu-id="181f0-500">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="181f0-501">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="181f0-501">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="181f0-502">十進位</span><span class="sxs-lookup"><span data-stu-id="181f0-502">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="181f0-503">兩</span><span class="sxs-lookup"><span data-stu-id="181f0-503">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="181f0-504">列舉</span><span class="sxs-lookup"><span data-stu-id="181f0-504">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="181f0-505">Guid</span><span class="sxs-lookup"><span data-stu-id="181f0-505">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="181f0-506">[Int16](xref:System.ComponentModel.Int16Converter)、[Int32](xref:System.ComponentModel.Int32Converter)、[Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="181f0-506">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="181f0-507">Single</span><span class="sxs-lookup"><span data-stu-id="181f0-507">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="181f0-508">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="181f0-508">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="181f0-509">[UInt16](xref:System.ComponentModel.UInt16Converter)、[UInt32](xref:System.ComponentModel.UInt32Converter)、[UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="181f0-509">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="181f0-510">Uri</span><span class="sxs-lookup"><span data-stu-id="181f0-510">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="181f0-511">版本</span><span class="sxs-lookup"><span data-stu-id="181f0-511">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="181f0-512">複雜類型</span><span class="sxs-lookup"><span data-stu-id="181f0-512">Complex types</span></span>

<span data-ttu-id="181f0-513">複雜型別必須具有公用預設的函式，以及要系結的公用可寫入屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-513">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="181f0-514">發生模型繫結時，類別會使用公用預設建構函式具現化。</span><span class="sxs-lookup"><span data-stu-id="181f0-514">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="181f0-515">針對複雜類型的每個屬性，模型繫結會查看名稱模式 *prefix.property_name* 的來源。</span><span class="sxs-lookup"><span data-stu-id="181f0-515">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="181f0-516">如果找不到，它會只尋找沒有前置詞的 *property_name*。</span><span class="sxs-lookup"><span data-stu-id="181f0-516">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="181f0-517">若要繫結至參數，則前置詞是參數名稱。</span><span class="sxs-lookup"><span data-stu-id="181f0-517">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="181f0-518">若要繫結至 `PageModel` 公用屬性，則前置詞為公用屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="181f0-518">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="181f0-519">某些屬性 (Attribute) 具有 `Prefix` 屬性 (Property)，其可讓您覆寫參數或屬性名稱的預設使用方法。</span><span class="sxs-lookup"><span data-stu-id="181f0-519">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="181f0-520">例如，假設複雜類型是下列 `Instructor` 類別：</span><span class="sxs-lookup"><span data-stu-id="181f0-520">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="181f0-521">前置詞 = 參數名稱</span><span class="sxs-lookup"><span data-stu-id="181f0-521">Prefix = parameter name</span></span>

<span data-ttu-id="181f0-522">如果要繫結的模型是名為 `instructorToUpdate` 的參數：</span><span class="sxs-lookup"><span data-stu-id="181f0-522">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="181f0-523">模型繫結從查看索引鍵 `instructorToUpdate.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-523">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="181f0-524">如果找不到，它會尋找`ID`不含前置詞的。</span><span class="sxs-lookup"><span data-stu-id="181f0-524">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="181f0-525">前置詞 = 屬性名稱</span><span class="sxs-lookup"><span data-stu-id="181f0-525">Prefix = property name</span></span>

<span data-ttu-id="181f0-526">如果要繫結的模型是控制器或 `PageModel` 類別名為 `Instructor` 的屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-526">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="181f0-527">模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-527">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="181f0-528">如果找不到，它會尋找`ID`不含前置詞的。</span><span class="sxs-lookup"><span data-stu-id="181f0-528">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="181f0-529">自訂前置詞</span><span class="sxs-lookup"><span data-stu-id="181f0-529">Custom prefix</span></span>

<span data-ttu-id="181f0-530">如果要繫結的模型是名為 `instructorToUpdate` 的參數，且 `Bind` 屬性指定 `Instructor` 為前置詞：</span><span class="sxs-lookup"><span data-stu-id="181f0-530">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="181f0-531">模型繫結從查看索引鍵 `Instructor.ID` 的來源開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-531">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="181f0-532">如果找不到，它會尋找`ID`不含前置詞的。</span><span class="sxs-lookup"><span data-stu-id="181f0-532">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="181f0-533">複雜類型目標的屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-533">Attributes for complex type targets</span></span>

<span data-ttu-id="181f0-534">有數個內建屬性可用來控制複雜類型的模型系結：</span><span class="sxs-lookup"><span data-stu-id="181f0-534">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="181f0-535">當張貼的表單資料為值來源時，這些屬性會影響模型繫結。</span><span class="sxs-lookup"><span data-stu-id="181f0-535">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="181f0-536">它們不會影響處理已張貼 JSON 和 XML 要求本文的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-536">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="181f0-537">[本文稍後](#input-formatters)會說明輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-537">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="181f0-538">另請參閱[模型驗證](xref:mvc/models/validation#required-attribute)中`[Required]`的屬性討論。</span><span class="sxs-lookup"><span data-stu-id="181f0-538">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="181f0-539">[BindRequired] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-539">[BindRequired] attribute</span></span>

<span data-ttu-id="181f0-540">只能套用至模型屬性，不能套用到方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-540">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="181f0-541">如果模型的屬性不能發生繫結，則會造成模型繫結新增模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="181f0-541">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="181f0-542">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-542">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="181f0-543">[BindNever] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-543">[BindNever] attribute</span></span>

<span data-ttu-id="181f0-544">只能套用至模型屬性，不能套用到方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-544">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="181f0-545">避免模型繫結設定模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-545">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="181f0-546">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-546">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="181f0-547">[Bind] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-547">[Bind] attribute</span></span>

<span data-ttu-id="181f0-548">可以套用至類別或方法參數。</span><span class="sxs-lookup"><span data-stu-id="181f0-548">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="181f0-549">指定模型繫結應包含哪些模型屬性。</span><span class="sxs-lookup"><span data-stu-id="181f0-549">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="181f0-550">在下列範例中，當呼叫任何處理常式或動作方法時，只會繫結 `Instructor` 模型的指定屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-550">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="181f0-551">在下列範例中，當呼叫 `OnPost` 方法時，只會繫結 `Instructor` 模型的指定屬性：</span><span class="sxs-lookup"><span data-stu-id="181f0-551">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="181f0-552">`[Bind]` 屬性可用來防止「建立」\*\* 案例中的大量指派。</span><span class="sxs-lookup"><span data-stu-id="181f0-552">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="181f0-553">它不適用於編輯案例，因為排除的屬性會設定為 null 或預設值，而不會保持不變。</span><span class="sxs-lookup"><span data-stu-id="181f0-553">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="181f0-554">建議使用檢視模型而非 `[Bind]` 屬性來防禦大量指派。</span><span class="sxs-lookup"><span data-stu-id="181f0-554">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="181f0-555">如需詳細資訊，請參閱[關於防止大量指派的安全性注意事項](xref:data/ef-mvc/crud#security-note-about-overposting)。</span><span class="sxs-lookup"><span data-stu-id="181f0-555">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="181f0-556">集合</span><span class="sxs-lookup"><span data-stu-id="181f0-556">Collections</span></span>

<span data-ttu-id="181f0-557">針對簡單型別集合的目標，模型繫結會尋找符合 *parameter_name* 或 *property_name* 的項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-557">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="181f0-558">如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。</span><span class="sxs-lookup"><span data-stu-id="181f0-558">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="181f0-559">例如：</span><span class="sxs-lookup"><span data-stu-id="181f0-559">For example:</span></span>

* <span data-ttu-id="181f0-560">假設要繫結的參數是名為 `selectedCourses` 的陣列：</span><span class="sxs-lookup"><span data-stu-id="181f0-560">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="181f0-561">表單或查詢字串資料可以是下列其中一種格式：</span><span class="sxs-lookup"><span data-stu-id="181f0-561">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="181f0-562">下列格式僅在表單資料中受到支援：</span><span class="sxs-lookup"><span data-stu-id="181f0-562">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="181f0-563">針對前列所有範例格式，模型繫結會將有兩個項目的陣列傳遞至 `selectedCourses` 參數：</span><span class="sxs-lookup"><span data-stu-id="181f0-563">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="181f0-564">selectedCourses [0] = 1050</span><span class="sxs-lookup"><span data-stu-id="181f0-564">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="181f0-565">selectedCourses [1] = 2000</span><span class="sxs-lookup"><span data-stu-id="181f0-565">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="181f0-566">使用注標編號的資料格式（.。。[0] .。。[1] ...）必須確定它們的編號順序是從零開始。</span><span class="sxs-lookup"><span data-stu-id="181f0-566">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="181f0-567">下標編號中如有任何間距，則會忽略間隔後的所有項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-567">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="181f0-568">例如，如果下標是 0 和 2，而不是 0 和 1，則忽略第二個項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-568">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="181f0-569">字典</span><span class="sxs-lookup"><span data-stu-id="181f0-569">Dictionaries</span></span>

<span data-ttu-id="181f0-570">針對`Dictionary`目標，模型系結會尋找與*parameter_name*或*property_name*相符的專案。</span><span class="sxs-lookup"><span data-stu-id="181f0-570">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="181f0-571">如果找不到相符項目，它會尋找其中一種沒有前置詞的受支援格式。</span><span class="sxs-lookup"><span data-stu-id="181f0-571">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="181f0-572">例如：</span><span class="sxs-lookup"><span data-stu-id="181f0-572">For example:</span></span>

* <span data-ttu-id="181f0-573">假設目標參數是名為`Dictionary<int, string>` `selectedCourses`的：</span><span class="sxs-lookup"><span data-stu-id="181f0-573">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="181f0-574">已張貼的表單或查詢字串資料看起來會像下列其中一個範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-574">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="181f0-575">針對前列所有範例格式，模型繫結會將有兩個項目的字典傳遞至 `selectedCourses` 參數：</span><span class="sxs-lookup"><span data-stu-id="181f0-575">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="181f0-576">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="181f0-576">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="181f0-577">selectedCourses ["2000"] = "經濟效益"</span><span class="sxs-lookup"><span data-stu-id="181f0-577">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="181f0-578">模型系結路由資料和查詢字串的全球化行為</span><span class="sxs-lookup"><span data-stu-id="181f0-578">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="181f0-579">ASP.NET Core 路由值提供者和查詢字串值提供者：</span><span class="sxs-lookup"><span data-stu-id="181f0-579">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="181f0-580">將值視為不因文化特性而異。</span><span class="sxs-lookup"><span data-stu-id="181f0-580">Treat values as invariant culture.</span></span>
* <span data-ttu-id="181f0-581">Url 會預期文化特性不變。</span><span class="sxs-lookup"><span data-stu-id="181f0-581">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="181f0-582">相反地，來自表單資料的值會經歷區分文化特性的轉換。</span><span class="sxs-lookup"><span data-stu-id="181f0-582">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="181f0-583">這是根據設計，讓 Url 可跨地區設定進行共用。</span><span class="sxs-lookup"><span data-stu-id="181f0-583">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="181f0-584">若要讓 ASP.NET Core route 值提供者和查詢字串值提供者進行區分文化特性的轉換：</span><span class="sxs-lookup"><span data-stu-id="181f0-584">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="181f0-585">繼承自 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span><span class="sxs-lookup"><span data-stu-id="181f0-585">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="181f0-586">從[QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs)或[RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)複製程式碼</span><span class="sxs-lookup"><span data-stu-id="181f0-586">Copy the code from [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="181f0-587">以 CurrentCulture 取代傳遞至值提供者的[文化特性值](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) [。](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="181f0-587">Replace the [culture value](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="181f0-588">將 MVC 選項中的預設值提供者 factory 取代為新的值：</span><span class="sxs-lookup"><span data-stu-id="181f0-588">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="181f0-589">特殊資料類型</span><span class="sxs-lookup"><span data-stu-id="181f0-589">Special data types</span></span>

<span data-ttu-id="181f0-590">模型繫結可以處理某些特殊資料類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-590">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="181f0-591">IFormFile 和 IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="181f0-591">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="181f0-592">HTTP 要求包含上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="181f0-592">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="181f0-593">也支援多個檔案的 `IEnumerable<IFormFile>`。</span><span class="sxs-lookup"><span data-stu-id="181f0-593">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="181f0-594">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="181f0-594">CancellationToken</span></span>

<span data-ttu-id="181f0-595">用來取消非同步控制器中的活動。</span><span class="sxs-lookup"><span data-stu-id="181f0-595">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="181f0-596">FormCollection</span><span class="sxs-lookup"><span data-stu-id="181f0-596">FormCollection</span></span>

<span data-ttu-id="181f0-597">用來擷取已張貼表單資料中的所有值。</span><span class="sxs-lookup"><span data-stu-id="181f0-597">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="181f0-598">輸入格式器</span><span class="sxs-lookup"><span data-stu-id="181f0-598">Input formatters</span></span>

<span data-ttu-id="181f0-599">要求主體中的資料可以是 JSON、XML 或其他格式。</span><span class="sxs-lookup"><span data-stu-id="181f0-599">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="181f0-600">模型繫結會使用設定處理特定內容類型的「輸入格式器」\*\*，來剖析此資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-600">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="181f0-601">根據預設，ASP.NET Core 包括用來處理 JSON 資料的 JSON 型輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-601">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="181f0-602">您可以為其他內容類型新增其他格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-602">You can add other formatters for other content types.</span></span>

<span data-ttu-id="181f0-603">ASP.NET Core 選取以 [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) 屬性為基礎的輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="181f0-603">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="181f0-604">若無任何屬性，則它會使用 [Content-Type 標頭](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)。</span><span class="sxs-lookup"><span data-stu-id="181f0-604">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="181f0-605">使用內建的 XML 輸入格式器：</span><span class="sxs-lookup"><span data-stu-id="181f0-605">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="181f0-606">安裝 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="181f0-606">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="181f0-607">在 `Startup.ConfigureServices` 中，呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> 或 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>。</span><span class="sxs-lookup"><span data-stu-id="181f0-607">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="181f0-608">將 `Consumes` 屬性套用至要求本文應為 XML 的控制器類別或動作方法。</span><span class="sxs-lookup"><span data-stu-id="181f0-608">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="181f0-609">如需詳細資訊，請參閱[XML 序列化簡介](/dotnet/standard/serialization/introducing-xml-serialization)。</span><span class="sxs-lookup"><span data-stu-id="181f0-609">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="181f0-610">排除模型繫結中的指定類型</span><span class="sxs-lookup"><span data-stu-id="181f0-610">Exclude specified types from model binding</span></span>

<span data-ttu-id="181f0-611">模型繫結和驗證系統的行為是由 [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata) 所驅動。</span><span class="sxs-lookup"><span data-stu-id="181f0-611">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="181f0-612">您可以將詳細資料提供者新增至 [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)，藉以自訂 `ModelMetadata`。</span><span class="sxs-lookup"><span data-stu-id="181f0-612">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="181f0-613">內建的詳細資料提供者可用於停用模型繫結或驗證所指定類型。</span><span class="sxs-lookup"><span data-stu-id="181f0-613">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="181f0-614">若要停用指定類型之所有模型的模型繫結，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider>。</span><span class="sxs-lookup"><span data-stu-id="181f0-614">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="181f0-615">例如，若要對類型為 `System.Version` 的所有模型停用模型繫結：</span><span class="sxs-lookup"><span data-stu-id="181f0-615">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="181f0-616">若要停用指定類型屬性的驗證，請在 `Startup.ConfigureServices` 中新增 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider>。</span><span class="sxs-lookup"><span data-stu-id="181f0-616">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="181f0-617">例如，若要針對類型為 `System.Guid` 的屬性停用驗證：</span><span class="sxs-lookup"><span data-stu-id="181f0-617">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="181f0-618">自訂模型繫結器</span><span class="sxs-lookup"><span data-stu-id="181f0-618">Custom model binders</span></span>

<span data-ttu-id="181f0-619">您可以撰寫自訂的模型繫結器來擴充模型繫結，並使用 `[ModelBinder]` 屬性以針對特定目標來選取它。</span><span class="sxs-lookup"><span data-stu-id="181f0-619">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="181f0-620">深入了解[自訂模型繫結](xref:mvc/advanced/custom-model-binding)。</span><span class="sxs-lookup"><span data-stu-id="181f0-620">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="181f0-621">手動模型系結</span><span class="sxs-lookup"><span data-stu-id="181f0-621">Manual model binding</span></span>

<span data-ttu-id="181f0-622">使用 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> 方法即可手動叫用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="181f0-622">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="181f0-623">此方法已於 `ControllerBase` 和 `PageModel` 類別中定義。</span><span class="sxs-lookup"><span data-stu-id="181f0-623">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="181f0-624">方法多載可讓您指定要使用的前置詞和值提供者。</span><span class="sxs-lookup"><span data-stu-id="181f0-624">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="181f0-625">如果模型繫結失敗，此方法會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="181f0-625">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="181f0-626">以下是範例：</span><span class="sxs-lookup"><span data-stu-id="181f0-626">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="181f0-627">[FromServices] 屬性</span><span class="sxs-lookup"><span data-stu-id="181f0-627">[FromServices] attribute</span></span>

<span data-ttu-id="181f0-628">這個屬性的名稱會遵循指定資料來源之模型系結屬性的模式。</span><span class="sxs-lookup"><span data-stu-id="181f0-628">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="181f0-629">但它並不是關於從值提供者系結資料。</span><span class="sxs-lookup"><span data-stu-id="181f0-629">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="181f0-630">它從[相依性插入](xref:fundamentals/dependency-injection)容器取得類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="181f0-630">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="181f0-631">其目的是只有在呼叫特定方法時，才在您需要服務時提供建構函式插入的替代項目。</span><span class="sxs-lookup"><span data-stu-id="181f0-631">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="181f0-632">其他資源</span><span class="sxs-lookup"><span data-stu-id="181f0-632">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
