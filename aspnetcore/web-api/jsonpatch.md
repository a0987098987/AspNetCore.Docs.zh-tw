---
title: ASP.NET Core Web API 中的 JsonPatch
author: tdykstra
description: 了解如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: 97264903d85dbb397e85fdbf7b070e2aaae74bc8
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815546"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="73ab5-103">ASP.NET Core Web API 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="73ab5-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="73ab5-104">作者：[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="73ab5-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="73ab5-105">本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。</span><span class="sxs-lookup"><span data-stu-id="73ab5-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="73ab5-106">PATCH HTTP 要求方法</span><span class="sxs-lookup"><span data-stu-id="73ab5-106">PATCH HTTP request method</span></span>

<span data-ttu-id="73ab5-107">PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。</span><span class="sxs-lookup"><span data-stu-id="73ab5-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="73ab5-108">它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。</span><span class="sxs-lookup"><span data-stu-id="73ab5-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="73ab5-109">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="73ab5-109">JSON Patch</span></span>

<span data-ttu-id="73ab5-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。</span><span class="sxs-lookup"><span data-stu-id="73ab5-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="73ab5-111">JSON Patch 文件具有一個「作業」  陣列。</span><span class="sxs-lookup"><span data-stu-id="73ab5-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="73ab5-112">每個作業都會識別特定類型的變更，例如，加入陣列元素或取代屬性值。</span><span class="sxs-lookup"><span data-stu-id="73ab5-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="73ab5-113">例如，下列 JSON 文件代表一個資源、一份適用於該資源的 JSON 修補文件，以及套用修補作業的結果。</span><span class="sxs-lookup"><span data-stu-id="73ab5-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="73ab5-114">資源範例</span><span class="sxs-lookup"><span data-stu-id="73ab5-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="73ab5-115">JSON 修補範例</span><span class="sxs-lookup"><span data-stu-id="73ab5-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="73ab5-116">在上述 JSON 中：</span><span class="sxs-lookup"><span data-stu-id="73ab5-116">In the preceding JSON:</span></span>

* <span data-ttu-id="73ab5-117">`op` 屬性會指出作業的類型。</span><span class="sxs-lookup"><span data-stu-id="73ab5-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="73ab5-118">`path` 屬性會指出要更新的元素。</span><span class="sxs-lookup"><span data-stu-id="73ab5-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="73ab5-119">`value` 屬性會提供新值。</span><span class="sxs-lookup"><span data-stu-id="73ab5-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="73ab5-120">修補之後的資源</span><span class="sxs-lookup"><span data-stu-id="73ab5-120">Resource after patch</span></span>

<span data-ttu-id="73ab5-121">以下是套用上述 JSON Patch 文件之後的資源：</span><span class="sxs-lookup"><span data-stu-id="73ab5-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="73ab5-122">藉由將 JSON Patch 文件套用至資源所做的變更是不可部分完成的：如果清單中有任何作業失敗，就不會套用清單中的任何作業。</span><span class="sxs-lookup"><span data-stu-id="73ab5-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="73ab5-123">路徑語法</span><span class="sxs-lookup"><span data-stu-id="73ab5-123">Path syntax</span></span>

<span data-ttu-id="73ab5-124">作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。</span><span class="sxs-lookup"><span data-stu-id="73ab5-124">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="73ab5-125">例如，`"/address/zipCode"`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="73ab5-126">以零為起始的索引可用來指定陣列元素。</span><span class="sxs-lookup"><span data-stu-id="73ab5-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="73ab5-127">`addresses` 陣列的第一個元素會在 `/addresses/0` 上。</span><span class="sxs-lookup"><span data-stu-id="73ab5-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="73ab5-128">若要 `add` 到陣列結尾處，請使用連字號 (-) 而不是索引號碼：`/addresses/-`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="73ab5-129">作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-129">Operations</span></span>

<span data-ttu-id="73ab5-130">下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：</span><span class="sxs-lookup"><span data-stu-id="73ab5-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="73ab5-131">運算</span><span class="sxs-lookup"><span data-stu-id="73ab5-131">Operation</span></span>  | <span data-ttu-id="73ab5-132">注意</span><span class="sxs-lookup"><span data-stu-id="73ab5-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="73ab5-133">加入屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="73ab5-133">Add a property or array element.</span></span> <span data-ttu-id="73ab5-134">針對現有的屬性：設定值。</span><span class="sxs-lookup"><span data-stu-id="73ab5-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="73ab5-135">移除屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="73ab5-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="73ab5-136">與 `remove` 之後接著在同一個位置上 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="73ab5-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="73ab5-137">與從來源 `remove` 之後接著使用來源的值 `add` 到目的地相同。</span><span class="sxs-lookup"><span data-stu-id="73ab5-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="73ab5-138">與使用來源的值 `add` 到目的地相同。</span><span class="sxs-lookup"><span data-stu-id="73ab5-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="73ab5-139">如果 `path` 上的值 = 所提供的 `value`，即會傳回成功狀態碼。</span><span class="sxs-lookup"><span data-stu-id="73ab5-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="73ab5-140">ASP.NET Core 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="73ab5-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="73ab5-141">[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。</span><span class="sxs-lookup"><span data-stu-id="73ab5-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="73ab5-142">此套件隨附於 [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) 中繼套件中。</span><span class="sxs-lookup"><span data-stu-id="73ab5-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="73ab5-143">動作方法程式碼</span><span class="sxs-lookup"><span data-stu-id="73ab5-143">Action method code</span></span>

<span data-ttu-id="73ab5-144">在 API 控制器中，JSON Patch 的動作方法：</span><span class="sxs-lookup"><span data-stu-id="73ab5-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="73ab5-145">使用 `HttpPatch` 屬性來標註。</span><span class="sxs-lookup"><span data-stu-id="73ab5-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="73ab5-146">通常會使用 [FromBody] 來接受 `JsonPatchDocument<T>`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="73ab5-147">呼叫修補文件上的 `ApplyTo` 以套用變更。</span><span class="sxs-lookup"><span data-stu-id="73ab5-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="73ab5-148">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="73ab5-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="73ab5-149">這段來自範例應用程式的程式碼會使用下列 `Customer` 模型。</span><span class="sxs-lookup"><span data-stu-id="73ab5-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="73ab5-150">範例動作方法：</span><span class="sxs-lookup"><span data-stu-id="73ab5-150">The sample action method:</span></span>

* <span data-ttu-id="73ab5-151">建構 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="73ab5-152">套用修補檔案。</span><span class="sxs-lookup"><span data-stu-id="73ab5-152">Applies the patch.</span></span>
* <span data-ttu-id="73ab5-153">在回應本文中傳回結果。</span><span class="sxs-lookup"><span data-stu-id="73ab5-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="73ab5-154">在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="73ab5-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="73ab5-155">模型狀態</span><span class="sxs-lookup"><span data-stu-id="73ab5-155">Model state</span></span>

<span data-ttu-id="73ab5-156">上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。</span><span class="sxs-lookup"><span data-stu-id="73ab5-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="73ab5-157">使用此選項，您就能在回應中收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="73ab5-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="73ab5-158">下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：</span><span class="sxs-lookup"><span data-stu-id="73ab5-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="73ab5-159">動態物件</span><span class="sxs-lookup"><span data-stu-id="73ab5-159">Dynamic objects</span></span>

<span data-ttu-id="73ab5-160">下列動作方法範例示範如何將修補檔案套用至動態物件。</span><span class="sxs-lookup"><span data-stu-id="73ab5-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="73ab5-161">新增作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-161">The add operation</span></span>

* <span data-ttu-id="73ab5-162">如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。</span><span class="sxs-lookup"><span data-stu-id="73ab5-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="73ab5-163">如果 `path` 指向屬性：設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="73ab5-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="73ab5-164">如果 `path` 指向不存在的位置：</span><span class="sxs-lookup"><span data-stu-id="73ab5-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="73ab5-165">如果要修補的資源是動態物件：加入屬性。</span><span class="sxs-lookup"><span data-stu-id="73ab5-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="73ab5-166">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="73ab5-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="73ab5-167">下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。</span><span class="sxs-lookup"><span data-stu-id="73ab5-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="73ab5-168">移除作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-168">The remove operation</span></span>

* <span data-ttu-id="73ab5-169">如果 `path` 指向陣列元素：移除該元素。</span><span class="sxs-lookup"><span data-stu-id="73ab5-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="73ab5-170">如果 `path` 指向屬性：</span><span class="sxs-lookup"><span data-stu-id="73ab5-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="73ab5-171">如果要修補的資源是動態物件：移除屬性。</span><span class="sxs-lookup"><span data-stu-id="73ab5-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="73ab5-172">如果要修補的資源是靜態物件：</span><span class="sxs-lookup"><span data-stu-id="73ab5-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="73ab5-173">如果屬性可為 Null：將它設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="73ab5-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="73ab5-174">如果屬性不可為 Null，則將它設定為 `default<T>`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="73ab5-175">下列範例修補文件會將 `CustomerName` 設定為 Null 並刪除 `Orders[0]`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="73ab5-176">取代作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-176">The replace operation</span></span>

<span data-ttu-id="73ab5-177">此作業在功能上與 `remove` 之後接著 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="73ab5-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="73ab5-178">下列範例修補文件會設定 `CustomerName` 的值，並使用新的 `Order` 物件來取代 `Orders[0]`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="73ab5-179">移動作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-179">The move operation</span></span>

* <span data-ttu-id="73ab5-180">如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `from` 元素上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="73ab5-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="73ab5-181">如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `from` 屬性上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="73ab5-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="73ab5-182">如果 `path` 指向不存在的屬性：</span><span class="sxs-lookup"><span data-stu-id="73ab5-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="73ab5-183">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="73ab5-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="73ab5-184">如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `from` 屬性上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="73ab5-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="73ab5-185">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="73ab5-185">The following sample patch document:</span></span>

* <span data-ttu-id="73ab5-186">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="73ab5-187">將 `Orders[0].OrderName` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="73ab5-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="73ab5-188">將 `Orders[1]` 移到 `Orders[0]` 前面。</span><span class="sxs-lookup"><span data-stu-id="73ab5-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="73ab5-189">複製作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-189">The copy operation</span></span>

<span data-ttu-id="73ab5-190">此作業在功能上與不含最後 `remove` 步驟的 `move` 作業相同。</span><span class="sxs-lookup"><span data-stu-id="73ab5-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="73ab5-191">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="73ab5-191">The following sample patch document:</span></span>

* <span data-ttu-id="73ab5-192">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="73ab5-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="73ab5-193">在 `Orders[0]` 前面插入 `Orders[1]` 的複本。</span><span class="sxs-lookup"><span data-stu-id="73ab5-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="73ab5-194">測試作業</span><span class="sxs-lookup"><span data-stu-id="73ab5-194">The test operation</span></span>

<span data-ttu-id="73ab5-195">如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="73ab5-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="73ab5-196">在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。</span><span class="sxs-lookup"><span data-stu-id="73ab5-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="73ab5-197">`test` 作業通常會用來防止在發生並行衝突時進行更新。</span><span class="sxs-lookup"><span data-stu-id="73ab5-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="73ab5-198">如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：</span><span class="sxs-lookup"><span data-stu-id="73ab5-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="73ab5-199">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="73ab5-199">Get the code</span></span>

<span data-ttu-id="73ab5-200">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2)。</span><span class="sxs-lookup"><span data-stu-id="73ab5-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="73ab5-201">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="73ab5-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="73ab5-202">若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="73ab5-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="73ab5-203">URL：`http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="73ab5-203">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="73ab5-204">HTTP 方法：`PATCH`</span><span class="sxs-lookup"><span data-stu-id="73ab5-204">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="73ab5-205">標題：`Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="73ab5-205">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="73ab5-206">本文：從 *JSON* 專案資料夾中，複製並貼上其中一個 JSON 修補文件範例。</span><span class="sxs-lookup"><span data-stu-id="73ab5-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73ab5-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="73ab5-207">Additional resources</span></span>

* <span data-ttu-id="73ab5-208">[IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="73ab5-208">[IETF RFC 5789 PATCH method specification](https://tools.ietf.org/html/rfc5789)</span></span>
* <span data-ttu-id="73ab5-209">[IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="73ab5-209">[IETF RFC 6902 JSON Patch specification](https://tools.ietf.org/html/rfc6902)</span></span>
* <span data-ttu-id="73ab5-210">[IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="73ab5-210">[IETF RFC 6901 JSON Patch path format spec](https://tools.ietf.org/html/rfc6901)</span></span>
* <span data-ttu-id="73ab5-211">[JSON Patch 文件](https://jsonpatch.com/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="73ab5-211">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="73ab5-212">包含用於建立 JSON Patch 文件的資源連結。</span><span class="sxs-lookup"><span data-stu-id="73ab5-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="73ab5-213">ASP.NET Core JSON Patch 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="73ab5-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
