---
title: ASP.NET Core Web API 中的 JsonPatch
author: rick-anderson
description: 了解如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: e57556e4b3fba55c6c187092593ffab4e31ee2d9
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727061"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="dcbfc-103">ASP.NET Core Web API 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="dcbfc-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="dcbfc-104">由[Tom 作者: dykstra](https://github.com/tdykstra)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-104">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dcbfc-105">本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="dcbfc-106">套件安裝</span><span class="sxs-lookup"><span data-stu-id="dcbfc-106">Package installation</span></span>

<span data-ttu-id="dcbfc-107">您可使用 `Microsoft.AspNetCore.Mvc.NewtonsoftJson` 套件來啟用對 JsonPatch 的支援。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-107">Support for JsonPatch is enabled using the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package.</span></span> <span data-ttu-id="dcbfc-108">若要啟用這項功能，應用程式必須：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-108">To enable this feature, apps must:</span></span>

* <span data-ttu-id="dcbfc-109">安裝[AspNetCore NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-109">Install the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
* <span data-ttu-id="dcbfc-110">更新專案的 `Startup.ConfigureServices` 方法以包括對 `AddNewtonsoftJson` 的呼叫：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-110">Update the project's `Startup.ConfigureServices` method to include a call to `AddNewtonsoftJson`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

<span data-ttu-id="dcbfc-111">`AddNewtonsoftJson` 與 MVC 服務註冊方法相容：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-111">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="dcbfc-112">JsonPatch、AddNewtonsoftJson 和 System.webserver</span><span class="sxs-lookup"><span data-stu-id="dcbfc-112">JsonPatch, AddNewtonsoftJson, and System.Text.Json</span></span>
  
<span data-ttu-id="dcbfc-113">`AddNewtonsoftJson` 取代用來格式化**所有**JSON 內容的 `System.Text.Json` 型輸入和輸出格式器。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-113">`AddNewtonsoftJson` replaces the `System.Text.Json` based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="dcbfc-114">若要使用 `Newtonsoft.Json`加入 `JsonPatch` 的支援，同時讓其他格式器保持不變，請更新專案的 `Startup.ConfigureServices`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-114">To add support for `JsonPatch` using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="dcbfc-115">上述程式碼需要[NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson)的參考和下列 using 語句：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-115">The preceding code requires a reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) and the following using statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="dcbfc-116">PATCH HTTP 要求方法</span><span class="sxs-lookup"><span data-stu-id="dcbfc-116">PATCH HTTP request method</span></span>

<span data-ttu-id="dcbfc-117">PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-117">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="dcbfc-118">它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-118">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="dcbfc-119">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="dcbfc-119">JSON Patch</span></span>

<span data-ttu-id="dcbfc-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="dcbfc-121">JSON Patch 文件具有一個「作業」陣列。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-121">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="dcbfc-122">每個作業都會識別特定類型的變更，例如，加入陣列元素或取代屬性值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-122">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="dcbfc-123">例如，下列 JSON 文件代表一個資源、一份適用於該資源的 JSON 修補文件，以及套用修補作業的結果。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-123">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="dcbfc-124">資源範例</span><span class="sxs-lookup"><span data-stu-id="dcbfc-124">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="dcbfc-125">JSON 修補範例</span><span class="sxs-lookup"><span data-stu-id="dcbfc-125">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="dcbfc-126">在上述 JSON 中：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-126">In the preceding JSON:</span></span>

* <span data-ttu-id="dcbfc-127">`op` 屬性會指出作業的類型。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-127">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="dcbfc-128">`path` 屬性會指出要更新的元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-128">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="dcbfc-129">`value` 屬性會提供新值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-129">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="dcbfc-130">修補之後的資源</span><span class="sxs-lookup"><span data-stu-id="dcbfc-130">Resource after patch</span></span>

<span data-ttu-id="dcbfc-131">以下是套用上述 JSON Patch 文件之後的資源：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-131">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="dcbfc-132">藉由將 JSON Patch 文件套用至資源所做的變更是不可部分完成的：如果清單中有任何作業失敗，就不會套用清單中的任何作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-132">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="dcbfc-133">路徑語法</span><span class="sxs-lookup"><span data-stu-id="dcbfc-133">Path syntax</span></span>

<span data-ttu-id="dcbfc-134">作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-134">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="dcbfc-135">例如，`"/address/zipCode"`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-135">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="dcbfc-136">以零為起始的索引可用來指定陣列元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-136">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="dcbfc-137">`addresses` 陣列的第一個元素會在 `/addresses/0` 上。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-137">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="dcbfc-138">若要 `add` 到陣列結尾處，請使用連字號 (-) 而不是索引號碼：`/addresses/-`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-138">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="dcbfc-139">作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-139">Operations</span></span>

<span data-ttu-id="dcbfc-140">下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-140">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="dcbfc-141">運算</span><span class="sxs-lookup"><span data-stu-id="dcbfc-141">Operation</span></span>  | <span data-ttu-id="dcbfc-142">注意事項</span><span class="sxs-lookup"><span data-stu-id="dcbfc-142">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="dcbfc-143">加入屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-143">Add a property or array element.</span></span> <span data-ttu-id="dcbfc-144">針對現有的屬性：設定值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-144">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="dcbfc-145">移除屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-145">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="dcbfc-146">與 `remove` 之後接著在同一個位置上 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-146">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="dcbfc-147">與從來源 `remove` 之後接著使用來源的值 `add` 到目的地相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-147">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="dcbfc-148">與使用來源的值 `add` 到目的地相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-148">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="dcbfc-149">如果 `path` 上的值 = 所提供的 `value`，即會傳回成功狀態碼。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-149">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="dcbfc-150">ASP.NET Core 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="dcbfc-150">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="dcbfc-151">[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-151">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="dcbfc-152">動作方法程式碼</span><span class="sxs-lookup"><span data-stu-id="dcbfc-152">Action method code</span></span>

<span data-ttu-id="dcbfc-153">在 API 控制器中，JSON Patch 的動作方法：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-153">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="dcbfc-154">使用 `HttpPatch` 屬性來標註。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-154">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="dcbfc-155">接受 `JsonPatchDocument<T>`，通常具有 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-155">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="dcbfc-156">呼叫修補文件上的 `ApplyTo` 以套用變更。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-156">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="dcbfc-157">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-157">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="dcbfc-158">這段來自範例應用程式的程式碼會使用下列 `Customer` 模型。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-158">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="dcbfc-159">範例動作方法：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-159">The sample action method:</span></span>

* <span data-ttu-id="dcbfc-160">建構 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-160">Constructs a `Customer`.</span></span>
* <span data-ttu-id="dcbfc-161">套用修補檔案。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-161">Applies the patch.</span></span>
* <span data-ttu-id="dcbfc-162">在回應本文中傳回結果。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-162">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="dcbfc-163">在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-163">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="dcbfc-164">模型狀態</span><span class="sxs-lookup"><span data-stu-id="dcbfc-164">Model state</span></span>

<span data-ttu-id="dcbfc-165">上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-165">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="dcbfc-166">使用此選項，您就能在回應中收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-166">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="dcbfc-167">下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-167">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="dcbfc-168">動態物件</span><span class="sxs-lookup"><span data-stu-id="dcbfc-168">Dynamic objects</span></span>

<span data-ttu-id="dcbfc-169">下列動作方法範例示範如何將修補檔案套用至動態物件。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-169">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="dcbfc-170">新增作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-170">The add operation</span></span>

* <span data-ttu-id="dcbfc-171">如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-171">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="dcbfc-172">如果 `path` 指向屬性：設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-172">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="dcbfc-173">如果 `path` 指向不存在的位置：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-173">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="dcbfc-174">如果要修補的資源是動態物件：加入屬性。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-174">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="dcbfc-175">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-175">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="dcbfc-176">下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-176">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="dcbfc-177">移除作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-177">The remove operation</span></span>

* <span data-ttu-id="dcbfc-178">如果 `path` 指向陣列元素：移除該元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-178">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="dcbfc-179">如果 `path` 指向屬性：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-179">If `path` points to a property:</span></span>
  * <span data-ttu-id="dcbfc-180">如果要修補的資源是動態物件：移除屬性。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-180">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="dcbfc-181">如果要修補的資源是靜態物件：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-181">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="dcbfc-182">如果屬性可為 Null：將它設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-182">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="dcbfc-183">如果屬性不可為 Null，則將它設定為 `default<T>`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-183">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="dcbfc-184">下列範例修補文件會將 `CustomerName` 設定為 Null 並刪除 `Orders[0]`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-184">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="dcbfc-185">取代作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-185">The replace operation</span></span>

<span data-ttu-id="dcbfc-186">此作業在功能上與 `remove` 之後接著 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-186">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="dcbfc-187">下列範例修補文件會設定 `CustomerName` 的值，並使用新的 `Orders[0]` 物件來取代 `Order`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-187">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="dcbfc-188">移動作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-188">The move operation</span></span>

* <span data-ttu-id="dcbfc-189">如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `remove` 元素上執行 `from` 作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-189">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="dcbfc-190">如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `remove` 屬性上執行 `from` 作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-190">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="dcbfc-191">如果 `path` 指向不存在的屬性：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-191">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="dcbfc-192">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-192">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="dcbfc-193">如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `remove` 屬性上執行 `from` 作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-193">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="dcbfc-194">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-194">The following sample patch document:</span></span>

* <span data-ttu-id="dcbfc-195">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-195">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="dcbfc-196">將 `Orders[0].OrderName` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-196">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="dcbfc-197">將 `Orders[1]` 移到 `Orders[0]` 前面。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-197">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="dcbfc-198">複製作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-198">The copy operation</span></span>

<span data-ttu-id="dcbfc-199">此作業在功能上與不含最後 `move` 步驟的 `remove` 作業相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-199">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="dcbfc-200">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-200">The following sample patch document:</span></span>

* <span data-ttu-id="dcbfc-201">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-201">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="dcbfc-202">在 `Orders[1]` 前面插入 `Orders[0]` 的複本。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-202">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="dcbfc-203">測試作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-203">The test operation</span></span>

<span data-ttu-id="dcbfc-204">如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-204">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="dcbfc-205">在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-205">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="dcbfc-206">`test` 作業通常會用來防止在發生並行衝突時進行更新。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-206">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="dcbfc-207">如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-207">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="dcbfc-208">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="dcbfc-208">Get the code</span></span>

<span data-ttu-id="dcbfc-209">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2)。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-209">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="dcbfc-210">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-210">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="dcbfc-211">若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-211">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="dcbfc-212">URL： `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="dcbfc-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="dcbfc-213">HTTP 方法：`PATCH`</span><span class="sxs-lookup"><span data-stu-id="dcbfc-213">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="dcbfc-214">標題：`Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="dcbfc-214">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="dcbfc-215">主體：從*json*專案資料夾複製並貼上其中一個 json 修補程式檔範例。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-215">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dcbfc-216">其他資源</span><span class="sxs-lookup"><span data-stu-id="dcbfc-216">Additional resources</span></span>

* <span data-ttu-id="dcbfc-217">[IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-217">[IETF RFC 5789 PATCH method specification](https://tools.ietf.org/html/rfc5789)</span></span>
* <span data-ttu-id="dcbfc-218">[IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-218">[IETF RFC 6902 JSON Patch specification](https://tools.ietf.org/html/rfc6902)</span></span>
* <span data-ttu-id="dcbfc-219">[IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-219">[IETF RFC 6901 JSON Patch path format spec](https://tools.ietf.org/html/rfc6901)</span></span>
* <span data-ttu-id="dcbfc-220">[JSON Patch 文件](https://jsonpatch.com/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-220">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="dcbfc-221">包含用於建立 JSON Patch 文件的資源連結。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-221">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="dcbfc-222">ASP.NET Core JSON Patch 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="dcbfc-222">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dcbfc-223">本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-223">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="dcbfc-224">PATCH HTTP 要求方法</span><span class="sxs-lookup"><span data-stu-id="dcbfc-224">PATCH HTTP request method</span></span>

<span data-ttu-id="dcbfc-225">PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-225">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="dcbfc-226">它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-226">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="dcbfc-227">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="dcbfc-227">JSON Patch</span></span>

<span data-ttu-id="dcbfc-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="dcbfc-229">JSON Patch 文件具有一個「作業」陣列。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-229">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="dcbfc-230">每個作業都會識別特定類型的變更，例如，加入陣列元素或取代屬性值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-230">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="dcbfc-231">例如，下列 JSON 文件代表一個資源、一份適用於該資源的 JSON 修補文件，以及套用修補作業的結果。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-231">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="dcbfc-232">資源範例</span><span class="sxs-lookup"><span data-stu-id="dcbfc-232">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="dcbfc-233">JSON 修補範例</span><span class="sxs-lookup"><span data-stu-id="dcbfc-233">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="dcbfc-234">在上述 JSON 中：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-234">In the preceding JSON:</span></span>

* <span data-ttu-id="dcbfc-235">`op` 屬性會指出作業的類型。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-235">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="dcbfc-236">`path` 屬性會指出要更新的元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-236">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="dcbfc-237">`value` 屬性會提供新值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-237">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="dcbfc-238">修補之後的資源</span><span class="sxs-lookup"><span data-stu-id="dcbfc-238">Resource after patch</span></span>

<span data-ttu-id="dcbfc-239">以下是套用上述 JSON Patch 文件之後的資源：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-239">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="dcbfc-240">藉由將 JSON Patch 文件套用至資源所做的變更是不可部分完成的：如果清單中有任何作業失敗，就不會套用清單中的任何作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-240">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="dcbfc-241">路徑語法</span><span class="sxs-lookup"><span data-stu-id="dcbfc-241">Path syntax</span></span>

<span data-ttu-id="dcbfc-242">作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-242">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="dcbfc-243">例如，`"/address/zipCode"`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-243">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="dcbfc-244">以零為起始的索引可用來指定陣列元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-244">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="dcbfc-245">`addresses` 陣列的第一個元素會在 `/addresses/0` 上。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-245">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="dcbfc-246">若要 `add` 到陣列結尾處，請使用連字號 (-) 而不是索引號碼：`/addresses/-`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-246">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="dcbfc-247">作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-247">Operations</span></span>

<span data-ttu-id="dcbfc-248">下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-248">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="dcbfc-249">運算</span><span class="sxs-lookup"><span data-stu-id="dcbfc-249">Operation</span></span>  | <span data-ttu-id="dcbfc-250">注意事項</span><span class="sxs-lookup"><span data-stu-id="dcbfc-250">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="dcbfc-251">加入屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-251">Add a property or array element.</span></span> <span data-ttu-id="dcbfc-252">針對現有的屬性：設定值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-252">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="dcbfc-253">移除屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-253">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="dcbfc-254">與 `remove` 之後接著在同一個位置上 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-254">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="dcbfc-255">與從來源 `remove` 之後接著使用來源的值 `add` 到目的地相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-255">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="dcbfc-256">與使用來源的值 `add` 到目的地相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-256">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="dcbfc-257">如果 `path` 上的值 = 所提供的 `value`，即會傳回成功狀態碼。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-257">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="dcbfc-258">ASP.NET Core 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="dcbfc-258">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="dcbfc-259">[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-259">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="dcbfc-260">此套件隨附於 [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) 中繼套件中。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-260">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="dcbfc-261">動作方法程式碼</span><span class="sxs-lookup"><span data-stu-id="dcbfc-261">Action method code</span></span>

<span data-ttu-id="dcbfc-262">在 API 控制器中，JSON Patch 的動作方法：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-262">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="dcbfc-263">使用 `HttpPatch` 屬性來標註。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-263">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="dcbfc-264">接受 `JsonPatchDocument<T>`，通常具有 `[FromBody]`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-264">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="dcbfc-265">呼叫修補文件上的 `ApplyTo` 以套用變更。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-265">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="dcbfc-266">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-266">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="dcbfc-267">這段來自範例應用程式的程式碼會使用下列 `Customer` 模型。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-267">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="dcbfc-268">範例動作方法：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-268">The sample action method:</span></span>

* <span data-ttu-id="dcbfc-269">建構 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-269">Constructs a `Customer`.</span></span>
* <span data-ttu-id="dcbfc-270">套用修補檔案。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-270">Applies the patch.</span></span>
* <span data-ttu-id="dcbfc-271">在回應本文中傳回結果。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-271">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="dcbfc-272">在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-272">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="dcbfc-273">模型狀態</span><span class="sxs-lookup"><span data-stu-id="dcbfc-273">Model state</span></span>

<span data-ttu-id="dcbfc-274">上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-274">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="dcbfc-275">使用此選項，您就能在回應中收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-275">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="dcbfc-276">下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-276">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="dcbfc-277">動態物件</span><span class="sxs-lookup"><span data-stu-id="dcbfc-277">Dynamic objects</span></span>

<span data-ttu-id="dcbfc-278">下列動作方法範例示範如何將修補檔案套用至動態物件。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-278">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="dcbfc-279">新增作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-279">The add operation</span></span>

* <span data-ttu-id="dcbfc-280">如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-280">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="dcbfc-281">如果 `path` 指向屬性：設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-281">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="dcbfc-282">如果 `path` 指向不存在的位置：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-282">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="dcbfc-283">如果要修補的資源是動態物件：加入屬性。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-283">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="dcbfc-284">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-284">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="dcbfc-285">下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-285">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="dcbfc-286">移除作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-286">The remove operation</span></span>

* <span data-ttu-id="dcbfc-287">如果 `path` 指向陣列元素：移除該元素。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-287">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="dcbfc-288">如果 `path` 指向屬性：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-288">If `path` points to a property:</span></span>
  * <span data-ttu-id="dcbfc-289">如果要修補的資源是動態物件：移除屬性。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-289">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="dcbfc-290">如果要修補的資源是靜態物件：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-290">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="dcbfc-291">如果屬性可為 Null：將它設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-291">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="dcbfc-292">如果屬性不可為 Null，則將它設定為 `default<T>`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-292">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="dcbfc-293">下列範例修補文件會將 `CustomerName` 設定為 Null 並刪除 `Orders[0]`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-293">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="dcbfc-294">取代作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-294">The replace operation</span></span>

<span data-ttu-id="dcbfc-295">此作業在功能上與 `remove` 之後接著 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-295">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="dcbfc-296">下列範例修補文件會設定 `CustomerName` 的值，並使用新的 `Orders[0]` 物件來取代 `Order`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-296">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="dcbfc-297">移動作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-297">The move operation</span></span>

* <span data-ttu-id="dcbfc-298">如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `remove` 元素上執行 `from` 作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-298">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="dcbfc-299">如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `remove` 屬性上執行 `from` 作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-299">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="dcbfc-300">如果 `path` 指向不存在的屬性：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-300">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="dcbfc-301">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-301">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="dcbfc-302">如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `remove` 屬性上執行 `from` 作業。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-302">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="dcbfc-303">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-303">The following sample patch document:</span></span>

* <span data-ttu-id="dcbfc-304">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-304">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="dcbfc-305">將 `Orders[0].OrderName` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-305">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="dcbfc-306">將 `Orders[1]` 移到 `Orders[0]` 前面。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-306">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="dcbfc-307">複製作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-307">The copy operation</span></span>

<span data-ttu-id="dcbfc-308">此作業在功能上與不含最後 `move` 步驟的 `remove` 作業相同。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-308">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="dcbfc-309">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-309">The following sample patch document:</span></span>

* <span data-ttu-id="dcbfc-310">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-310">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="dcbfc-311">在 `Orders[1]` 前面插入 `Orders[0]` 的複本。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-311">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="dcbfc-312">測試作業</span><span class="sxs-lookup"><span data-stu-id="dcbfc-312">The test operation</span></span>

<span data-ttu-id="dcbfc-313">如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-313">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="dcbfc-314">在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-314">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="dcbfc-315">`test` 作業通常會用來防止在發生並行衝突時進行更新。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-315">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="dcbfc-316">如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-316">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="dcbfc-317">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="dcbfc-317">Get the code</span></span>

<span data-ttu-id="dcbfc-318">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2)。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-318">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="dcbfc-319">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-319">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="dcbfc-320">若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="dcbfc-320">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="dcbfc-321">URL： `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="dcbfc-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="dcbfc-322">HTTP 方法：`PATCH`</span><span class="sxs-lookup"><span data-stu-id="dcbfc-322">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="dcbfc-323">標題：`Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="dcbfc-323">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="dcbfc-324">主體：從*json*專案資料夾複製並貼上其中一個 json 修補程式檔範例。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-324">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dcbfc-325">其他資源</span><span class="sxs-lookup"><span data-stu-id="dcbfc-325">Additional resources</span></span>

* <span data-ttu-id="dcbfc-326">[IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-326">[IETF RFC 5789 PATCH method specification](https://tools.ietf.org/html/rfc5789)</span></span>
* <span data-ttu-id="dcbfc-327">[IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-327">[IETF RFC 6902 JSON Patch specification](https://tools.ietf.org/html/rfc6902)</span></span>
* <span data-ttu-id="dcbfc-328">[IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="dcbfc-328">[IETF RFC 6901 JSON Patch path format spec](https://tools.ietf.org/html/rfc6901)</span></span>
* <span data-ttu-id="dcbfc-329">[JSON Patch 文件](https://jsonpatch.com/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-329">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="dcbfc-330">包含用於建立 JSON Patch 文件的資源連結。</span><span class="sxs-lookup"><span data-stu-id="dcbfc-330">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="dcbfc-331">ASP.NET Core JSON Patch 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="dcbfc-331">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
