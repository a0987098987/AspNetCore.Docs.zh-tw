---
title: ASP.NET Core Web API 中的 JsonPatch
author: rick-anderson
description: 了解如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: cf1a00c1928652bf5210b2442087209e23b8868e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661781"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a>ASP.NET Core Web API 中的 JsonPatch

由[Tom 作者: dykstra](https://github.com/tdykstra)和[Kirk Larkin](https://github.com/serpent5)

::: moniker range=">= aspnetcore-3.0"

本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。

## <a name="package-installation"></a>套件安裝

您可使用 `Microsoft.AspNetCore.Mvc.NewtonsoftJson` 套件來啟用對 JsonPatch 的支援。 若要啟用這項功能，應用程式必須：

* 安裝[AspNetCore NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 套件。
* 更新專案的 `Startup.ConfigureServices` 方法以包括對 `AddNewtonsoftJson` 的呼叫：

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

`AddNewtonsoftJson` 與 MVC 服務註冊方法相容：

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a>JsonPatch、AddNewtonsoftJson 和 System.webserver
  
`AddNewtonsoftJson` 取代用來格式化**所有**JSON 內容的 `System.Text.Json` 型輸入和輸出格式器。 若要使用 `Newtonsoft.Json`加入 `JsonPatch` 的支援，同時讓其他格式器保持不變，請更新專案的 `Startup.ConfigureServices`，如下所示：

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

上述程式碼需要[NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson)的參考和下列 using 語句：

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a>PATCH HTTP 要求方法

PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。 它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。

## <a name="json-patch"></a>JSON Patch

[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。 JSON Patch 文件具有一個「作業」陣列。 每個作業都會識別特定類型的變更，例如，加入陣列元素或取代屬性值。

例如，下列 JSON 文件代表一個資源、一份適用於該資源的 JSON 修補文件，以及套用修補作業的結果。

### <a name="resource-example"></a>資源範例

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>JSON 修補範例

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

在上述 JSON 中：

* `op` 屬性會指出作業的類型。
* `path` 屬性會指出要更新的元素。
* `value` 屬性會提供新值。

### <a name="resource-after-patch"></a>修補之後的資源

以下是套用上述 JSON Patch 文件之後的資源：

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

藉由將 JSON Patch 文件套用至資源所做的變更是不可部分完成的：如果清單中有任何作業失敗，就不會套用清單中的任何作業。

## <a name="path-syntax"></a>路徑語法

作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。 例如： `"/address/zipCode"` 。

以零為起始的索引可用來指定陣列元素。 `addresses` 陣列的第一個元素會在 `/addresses/0` 上。 若要 `add` 到陣列結尾處，請使用連字號 (-) 而不是索引號碼：`/addresses/-`。

### <a name="operations"></a>作業

下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：

|作業  | 注意 |
|-----------|--------------------------------|
| `add`     | 加入屬性或陣列元素。 針對現有的屬性：設定值。|
| `remove`  | 移除屬性或陣列元素。 |
| `replace` | 與 `remove` 之後接著在同一個位置上 `add` 相同。 |
| `move`    | 與從來源 `remove` 之後接著使用來源的值 `add` 到目的地相同。 |
| `copy`    | 與使用來源的值 `add` 到目的地相同。 |
| `test`    | 如果 `path` 上的值 = 所提供的 `value`，即會傳回成功狀態碼。|

## <a name="jsonpatch-in-aspnet-core"></a>ASP.NET Core 中的 JsonPatch

[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。

## <a name="action-method-code"></a>動作方法程式碼

在 API 控制器中，JSON Patch 的動作方法：

* 使用 `HttpPatch` 屬性來標註。
* 接受 `JsonPatchDocument<T>`，通常具有 `[FromBody]`。
* 呼叫修補文件上的 `ApplyTo` 以套用變更。

以下是範例：

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

這段來自範例應用程式的程式碼會使用下列 `Customer` 模型。

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

範例動作方法：

* 建構 `Customer`。
* 套用修補檔案。
* 在回應本文中傳回結果。

 在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。

### <a name="model-state"></a>模型狀態

上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。 使用此選項，您就能在回應中收到錯誤訊息。 下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>動態物件

下列動作方法範例示範如何將修補檔案套用至動態物件。

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>新增作業

* 如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。
* 如果 `path` 指向屬性：設定屬性值。
* 如果 `path` 指向不存在的位置：
  * 如果要修補的資源是動態物件：加入屬性。
  * 如果要修補的資源是靜態物件：要求失敗。

下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>移除作業

* 如果 `path` 指向陣列元素：移除該元素。
* 如果 `path` 指向屬性：
  * 如果要修補的資源是動態物件：移除屬性。
  * 如果要修補的資源是靜態物件：
    * 如果屬性可為 Null：將它設定為 Null。
    * 如果屬性不可為 Null，則將它設定為 `default<T>`。

下列範例修補文件會將 `CustomerName` 設定為 Null 並刪除 `Orders[0]`。

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>取代作業

此作業在功能上與 `remove` 之後接著 `add` 相同。

下列範例修補文件會設定 `CustomerName` 的值，並使用新的 `Orders[0]` 物件來取代 `Order`。

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>移動作業

* 如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `remove` 元素上執行 `from` 作業。
* 如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `remove` 屬性上執行 `from` 作業。
* 如果 `path` 指向不存在的屬性：
  * 如果要修補的資源是靜態物件：要求失敗。
  * 如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `remove` 屬性上執行 `from` 作業。

下列範例修補文件：

* 將 `Orders[0].OrderName` 的值複製到 `CustomerName`。
* 將 `Orders[0].OrderName` 設定為 Null。
* 將 `Orders[1]` 移到 `Orders[0]` 前面。

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>複製作業

此作業在功能上與不含最後 `move` 步驟的 `remove` 作業相同。

下列範例修補文件：

* 將 `Orders[0].OrderName` 的值複製到 `CustomerName`。
* 在 `Orders[1]` 前面插入 `Orders[0]` 的複本。

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>測試作業

如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。 在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。

`test` 作業通常會用來防止在發生並行衝突時進行更新。

如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>取得程式碼

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2)。 ([如何下載](xref:index#how-to-download-a-sample))。

若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：

* URL： `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* HTTP 方法：`PATCH`
* 標題：`Content-Type: application/json-patch+json`
* 主體：從*json*專案資料夾複製並貼上其中一個 json 修補程式檔範例。

## <a name="additional-resources"></a>其他資源

* [IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)
* [IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)
* [IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)
* [JSON Patch 文件](https://jsonpatch.com/) \(英文\)。 包含用於建立 JSON Patch 文件的資源連結。
* [ASP.NET Core JSON Patch 原始程式碼](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。

## <a name="patch-http-request-method"></a>PATCH HTTP 要求方法

PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。 它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。

## <a name="json-patch"></a>JSON Patch

[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。 JSON Patch 文件具有一個「作業」陣列。 每個作業都會識別特定類型的變更，例如，加入陣列元素或取代屬性值。

例如，下列 JSON 文件代表一個資源、一份適用於該資源的 JSON 修補文件，以及套用修補作業的結果。

### <a name="resource-example"></a>資源範例

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>JSON 修補範例

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

在上述 JSON 中：

* `op` 屬性會指出作業的類型。
* `path` 屬性會指出要更新的元素。
* `value` 屬性會提供新值。

### <a name="resource-after-patch"></a>修補之後的資源

以下是套用上述 JSON Patch 文件之後的資源：

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

藉由將 JSON Patch 文件套用至資源所做的變更是不可部分完成的：如果清單中有任何作業失敗，就不會套用清單中的任何作業。

## <a name="path-syntax"></a>路徑語法

作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。 例如： `"/address/zipCode"` 。

以零為起始的索引可用來指定陣列元素。 `addresses` 陣列的第一個元素會在 `/addresses/0` 上。 若要 `add` 到陣列結尾處，請使用連字號 (-) 而不是索引號碼：`/addresses/-`。

### <a name="operations"></a>作業

下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：

|作業  | 注意 |
|-----------|--------------------------------|
| `add`     | 加入屬性或陣列元素。 針對現有的屬性：設定值。|
| `remove`  | 移除屬性或陣列元素。 |
| `replace` | 與 `remove` 之後接著在同一個位置上 `add` 相同。 |
| `move`    | 與從來源 `remove` 之後接著使用來源的值 `add` 到目的地相同。 |
| `copy`    | 與使用來源的值 `add` 到目的地相同。 |
| `test`    | 如果 `path` 上的值 = 所提供的 `value`，即會傳回成功狀態碼。|

## <a name="jsonpatch-in-aspnet-core"></a>ASP.NET Core 中的 JsonPatch

[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。 此套件隨附於 [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) 中繼套件中。

## <a name="action-method-code"></a>動作方法程式碼

在 API 控制器中，JSON Patch 的動作方法：

* 使用 `HttpPatch` 屬性來標註。
* 接受 `JsonPatchDocument<T>`，通常具有 `[FromBody]`。
* 呼叫修補文件上的 `ApplyTo` 以套用變更。

以下是範例：

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

這段來自範例應用程式的程式碼會使用下列 `Customer` 模型。

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

範例動作方法：

* 建構 `Customer`。
* 套用修補檔案。
* 在回應本文中傳回結果。

 在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。

### <a name="model-state"></a>模型狀態

上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。 使用此選項，您就能在回應中收到錯誤訊息。 下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>動態物件

下列動作方法範例示範如何將修補檔案套用至動態物件。

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>新增作業

* 如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。
* 如果 `path` 指向屬性：設定屬性值。
* 如果 `path` 指向不存在的位置：
  * 如果要修補的資源是動態物件：加入屬性。
  * 如果要修補的資源是靜態物件：要求失敗。

下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>移除作業

* 如果 `path` 指向陣列元素：移除該元素。
* 如果 `path` 指向屬性：
  * 如果要修補的資源是動態物件：移除屬性。
  * 如果要修補的資源是靜態物件：
    * 如果屬性可為 Null：將它設定為 Null。
    * 如果屬性不可為 Null，則將它設定為 `default<T>`。

下列範例修補文件會將 `CustomerName` 設定為 Null 並刪除 `Orders[0]`。

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>取代作業

此作業在功能上與 `remove` 之後接著 `add` 相同。

下列範例修補文件會設定 `CustomerName` 的值，並使用新的 `Orders[0]` 物件來取代 `Order`。

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>移動作業

* 如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `remove` 元素上執行 `from` 作業。
* 如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `remove` 屬性上執行 `from` 作業。
* 如果 `path` 指向不存在的屬性：
  * 如果要修補的資源是靜態物件：要求失敗。
  * 如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `remove` 屬性上執行 `from` 作業。

下列範例修補文件：

* 將 `Orders[0].OrderName` 的值複製到 `CustomerName`。
* 將 `Orders[0].OrderName` 設定為 Null。
* 將 `Orders[1]` 移到 `Orders[0]` 前面。

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>複製作業

此作業在功能上與不含最後 `move` 步驟的 `remove` 作業相同。

下列範例修補文件：

* 將 `Orders[0].OrderName` 的值複製到 `CustomerName`。
* 在 `Orders[1]` 前面插入 `Orders[0]` 的複本。

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>測試作業

如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。 在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。

`test` 作業通常會用來防止在發生並行衝突時進行更新。

如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>取得程式碼

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2)。 ([如何下載](xref:index#how-to-download-a-sample))。

若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：

* URL： `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* HTTP 方法：`PATCH`
* 標題：`Content-Type: application/json-patch+json`
* 主體：從*json*專案資料夾複製並貼上其中一個 json 修補程式檔範例。

## <a name="additional-resources"></a>其他資源

* [IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)
* [IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)
* [IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)
* [JSON Patch 文件](https://jsonpatch.com/) \(英文\)。 包含用於建立 JSON Patch 文件的資源連結。
* [ASP.NET Core JSON Patch 原始程式碼](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
