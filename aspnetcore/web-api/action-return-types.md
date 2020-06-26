---
title: ASP.NET Core Web API 中的控制器動作傳回類型
author: scottaddie
description: 瞭解如何在 ASP.NET Core Web API 中使用各種控制器動作方法傳回類型。
ms.author: scaddie
ms.custom: mvc
ms.date: 02/03/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/action-return-types
ms.openlocfilehash: 7227a86db9b94cc68851cb1670ce9668148639ef
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404440"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>ASP.NET Core Web API 中的控制器動作傳回類型

作者：[Scott Addie](https://github.com/scottaddie)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples)（[如何下載](xref:index#how-to-download-a-sample)）

ASP.NET Core 提供 Web API 控制器動作傳回類型的下列選項：

::: moniker range=">= aspnetcore-2.1"

* [特定類型](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [特定類型](#specific-type)
* [IActionResult](#iactionresult-type)

::: moniker-end

本文件說明使用每個傳回型別的最適時機。

## <a name="specific-type"></a>特定類型

最簡單的動作傳回基本或複雜的資料類型 (例如，`string` 或自訂物件類型)。 請考慮下列動作，它會傳回自訂 `Product` 物件的集合：

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

無已知條件，無法確保在動作執行期間能滿足傳回特定類型。 上述動作不接受任何參數，因此不需要驗證參數條件約束。

當可能有多個傳回型別時，通常會 <xref:Microsoft.AspNetCore.Mvc.ActionResult> 使用基本或複雜的傳回型別來混合傳回型別。 必須有[IActionResult](#iactionresult-type)或[ActionResult \<T> ](#actionresultt-type) ，才能配合這種類型的動作。 本檔提供多個傳回類型的數個範例。

### <a name="return-ienumerablet-or-iasyncenumerablet"></a>傳回 IEnumerable \<T> 或 IAsyncEnumerable\<T>

在 ASP.NET Core 2.2 和更早版本中， <xref:System.Collections.Generic.IEnumerable%601> 從動作傳回會導致序列化程式進行同步集合反復專案。 結果是封鎖呼叫，而且執行緒集區的可能會耗盡。 為了說明，請想像，Entity Framework （EF） Core 是用於 Web API 的資料存取需求。 在序列化期間，會同步列舉下列動作的傳回型別：

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

若要避免在 ASP.NET Core 2.2 和更早版本的資料庫上進行同步列舉和封鎖等候，請叫用 `ToListAsync` ：

```csharp
public async Task<IEnumerable<Product>> GetOnSaleProducts() =>
    await _context.Products.Where(p => p.IsOnSale).ToListAsync();
```

在 ASP.NET Core 3.0 和更新版本中， `IAsyncEnumerable<T>` 從動作傳回：

* 不再產生同步反復專案。
* 變得有效率地傳回 <xref:System.Collections.Generic.IEnumerable%601> 。

ASP.NET Core 3.0 和更新版本會先緩衝下列動作的結果，再將它提供給序列化程式：

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

請考慮將動作簽章的傳回型別宣告為 `IAsyncEnumerable<T>` ，以確保非同步反復專案。 最後，反復專案模式是以所傳回的基礎具象型別為基礎。 MVC 會自動緩衝任何實作為的具象型別 `IAsyncEnumerable<T>` 。

請考慮下列動作，其會傳回銷售定價的產品記錄，如下所示 `IEnumerable<Product>` ：

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProducts)]

`IAsyncEnumerable<Product>`上述動作的對應項是：

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProductsAsync)]

在 ASP.NET Core 3.0 時，上述兩個動作都不會封鎖。

## <a name="iactionresult-type"></a>IActionResult 類型

<xref:Microsoft.AspNetCore.Mvc.IActionResult>當 `ActionResult` 動作中可能有多個傳回類型時，傳回類型是適當的。 `ActionResult` 類型代表各種 HTTP 狀態碼。 衍生自的任何非抽象類別都 `ActionResult` 符合資格，做為有效的傳回型別。 此分類中的一些常見傳回類型為 <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> （400）、 <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> （404）和 <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> （200）。 或者，您可以使用類別中的便利方法， <xref:Microsoft.AspNetCore.Mvc.ControllerBase> `ActionResult` 從動作傳回類型。 例如， `return BadRequest();` 是的簡短形式 `return new BadRequestResult();` 。

由於此類型的動作中有多個傳回類型和路徑，因此 [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) 需要使用屬性。 這個屬性會針對[Swagger](xref:tutorials/web-api-help-pages-using-swagger)之類的工具所產生的 Web API 說明頁面，產生更具描述性的回應詳細資料。 `[ProducesResponseType]` 表示已知類型和 HTTP 狀態碼要由動作傳回。

### <a name="synchronous-action"></a>同步動作

請考慮下列有兩個可能傳回型別的同步動作：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdIActionResult&highlight=8,11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

::: moniker-end

在上述動作中：

* 當所代表的產品 `id` 不存在於基礎資料存放區時，會傳回404狀態碼。 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>便利的方法會被叫用為的速記 `return new NotFoundResult();` 。
* 當產品存在時，會以物件傳回200狀態碼 `Product` 。 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*>便利的方法會被叫用為的速記 `return new OkObjectResult(product);` 。

### <a name="asynchronous-action"></a>非同步動作

請考慮下列有兩個可能傳回型別的同步動作：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncIActionResult&highlight=9,14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=9,14)]

::: moniker-end

在上述動作中：

* 當產品描述包含 "XYZ Widget" 時，會傳回400狀態碼。 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>便利的方法會被叫用為的速記 `return new BadRequestResult();` 。
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>當建立產品時，便利性方法會產生201狀態碼。 呼叫的替代方法 `CreatedAtAction` 是 `return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);` 。 在此程式碼路徑中， `Product` 會在回應主體中提供物件。 `Location`提供包含新建立之產品 URL 的回應標頭。

例如，下列模型指出要求必須包含 `Name` 和 `Description` 屬性。 無法 `Name` `Description` 在要求中提供和，會導致模型驗證失敗。

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

如果套用 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) ASP.NET Core 2.1 或更新版本中的屬性，則模型驗證錯誤會導致400狀態碼。 如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。

## <a name="actionresultt-type"></a>ActionResult \<T> 類型

ASP.NET Core 2.1 引進了 Web API 控制器動作的[ActionResult \<T> ](xref:Microsoft.AspNetCore.Mvc.ActionResult`1)傳回型別。 它可讓您傳回衍生自或傳回 <xref:Microsoft.AspNetCore.Mvc.ActionResult> [特定類型](#specific-type)的類型。 `ActionResult<T>` 透過 [IActionResult 類型](#iactionresult-type)提供下列優點：

* [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) `Type` 可以排除屬性的屬性。 例如，`[ProducesResponseType(200, Type = typeof(Product))]` 簡化為 `[ProducesResponseType(200)]`。 該動作的預期傳回型別會改為從 `ActionResult<T>` 中的 `T` 推斷。
* [隱含轉型運算子](/dotnet/csharp/language-reference/keywords/implicit)支援 `T` 和 `ActionResult` 轉換成 `ActionResult<T>`。 `T`將轉換為 <xref:Microsoft.AspNetCore.Mvc.ObjectResult> ，這表示 `return new ObjectResult(T);` 已簡化為 `return T;` 。

C# 不支援介面上的隱含轉換運算子。 因此，必須將介面轉換為具象型別才能使用 `ActionResult<T>`。 例如，在下列範例中使用 `IEnumerable` 將無法運作：

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get() =>
    _repository.GetProducts();
```

修正上述程式碼的其中一個選項是傳回 `_repository.GetProducts().ToList();`。

大部分的動作都有特定的傳回型別。 如果在動作執行期間發生非預期的狀況，就不會傳回特定的類型。 例如，動作的輸入參數可能無法驗證模型。 在此情況下，通常會傳回適當的 `ActionResult` 類型而不是特定的類型。

### <a name="synchronous-action"></a>同步動作

請考慮有兩個可能傳回型別的同步動作：

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdActionResult&highlight=8,11)]

在上述動作中：

* 當此產品不存在於資料庫中時，會傳回404狀態碼。
* 當產品存在時，會隨對應的物件傳回200狀態碼 `Product` 。 在 ASP.NET Core 2.1 之前，這 `return product;` 一行必須是 `return Ok(product);` 。

### <a name="asynchronous-action"></a>非同步動作

請考慮有兩個可能傳回型別的非同步動作：

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncActionResult&highlight=9,14)]

在上述動作中：

* 在 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> 下列情況中，ASP.NET Core 執行時間會傳回400狀態碼（）：
  * 已套用 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性，而且模型驗證失敗。
  * 產品描述包含 "XYZ Widget"。
* 當建立產品時，方法會產生201狀態碼 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 。 在此程式碼路徑中， `Product` 會在回應主體中提供物件。 `Location`提供包含新建立之產品 URL 的回應標頭。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
