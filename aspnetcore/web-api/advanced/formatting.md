---
title: 在 ASP.NET Core Web API 中格式化回應資料
author: ardalis
description: 了解如何在 ASP.NET Core Web API 中格式化回應資料。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/05/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: cab383053751598b882f3716943d3d9392c56f4a
ms.sourcegitcommit: 29ace642ca0e1f0b48a18d66de266d8811df2b83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2019
ms.locfileid: "74987965"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>在 ASP.NET Core Web API 中格式化回應資料

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

ASP.NET Core MVC 支援格式化回應資料。 您可以使用特定格式或回應用戶端要求的格式來格式化回應資料。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>特定格式的動作結果

某些動作結果類型是特定格式所特有的，例如 <xref:Microsoft.AspNetCore.Mvc.JsonResult> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult>。 無論用戶端喜好設定為何，動作都可以傳回以特定格式格式化的結果。 例如，傳回 `JsonResult` 會傳回 JSON 格式的資料。 傳回 `ContentResult` 或字串會傳回純文字格式的字串資料。

動作不需要傳回任何特定的類型。 ASP.NET Core 支援任何物件傳回值。  傳回不是 <xref:Microsoft.AspNetCore.Mvc.IActionResult> 類型之物件的動作結果，會使用適當的 <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> 執行進行序列化。 如需詳細資訊，請參閱<xref:web-api/action-return-types>。

內建 helper 方法 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> 會傳回 JSON 格式的資料： [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]

下載範例會傳回作者清單。 使用 F12 瀏覽器開發人員工具或[Postman](https://www.getpostman.com/tools)搭配先前的程式碼：

* 顯示包含**內容類型：** `application/json; charset=utf-8` 的回應標頭。
* 系統會顯示要求標頭。 例如，`Accept` 標頭。 前面的程式碼會忽略 `Accept` 標頭。

若要傳回純文字格式化資料，請使用 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 協助程式：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

在上述程式碼中，傳回的 `Content-Type` 是 `text/plain`。 傳回字串會傳遞 `text/plain`的 `Content-Type`：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

針對具有多個傳回類型的動作，傳回 `IActionResult`。 例如，根據執行的作業結果傳回不同的 HTTP 狀態碼。

## <a name="content-negotiation"></a>內容協調

當用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時，就會發生內容協商。 ASP.NET Core 使用的預設格式為[JSON](https://json.org/)。 內容協商為：

* 由 <xref:Microsoft.AspNetCore.Mvc.ObjectResult>執行。
* 內建自 helper 方法所傳回的狀態碼特定動作結果。 動作結果 helper 方法是以 `ObjectResult`為基礎。

傳回模型類型時，傳回類型為 `ObjectResult`。

下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

根據預設，ASP.NET Core 支援 `application/json`、`text/json`和 `text/plain` 媒體類型。 [Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/tools)這類工具可以設定 `Accept` 要求標頭，以指定傳回格式。 當 `Accept` 標頭包含伺服器支援的類型時，就會傳回該類型。 下一節將說明如何新增其他格式器。

控制器動作可以傳回 Poco （簡單的 CLR 物件）。 當 POCO 傳回時，執行時間會自動建立包裝物件的 `ObjectResult`。 用戶端會取得已格式化的序列化物件。 如果要傳回的物件是 `null`，則會傳回 `204 No Content` 回應。

傳回物件類型：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

在上述程式碼中，有效作者別名的要求會以作者的資料傳回 `200 OK` 回應。 對無效別名的要求會傳回 `204 No Content` 回應。

### <a name="the-accept-header"></a>Accept 標頭

當要求中出現 `Accept` 標頭時，就會進行內容*協商*。 當要求包含 accept 標頭時，ASP.NET Core：

* 依照喜好設定順序，列舉 accept 標頭中的媒體類型。
* 嘗試尋找可以使用其中一種指定的格式產生回應的格式器。

如果找不到可滿足用戶端要求的格式器，請 ASP.NET Core：

* 如果已設定 <xref:Microsoft.AspNetCore.Mvc.MvcOptions>，則傳回 `406 Not Acceptable`，或-
* 嘗試尋找可產生回應的第一個格式器。

如果未針對要求的格式設定格式器，則會使用可格式化物件的第一個格式器。 如果要求中未出現 `Accept` 標頭：

* 第一個可以處理物件的格式器是用來序列化回應。
* 沒有任何進行中的協商。 伺服器正在決定要傳回的格式。

如果 Accept 標頭包含 `*/*`，除非 <xref:Microsoft.AspNetCore.Mvc.MvcOptions>上 `RespectBrowserAcceptHeader` 設定為 true，否則會忽略標頭。

### <a name="browsers-and-content-negotiation"></a>瀏覽器和內容協調

與一般 API 用戶端不同的是，網頁瀏覽器會提供 `Accept` 標頭。 網頁瀏覽器會指定許多格式，包括萬用字元。 根據預設，當架構偵測到要求來自瀏覽器時：

* 已忽略 `Accept` 標頭。
* 除非另有設定，否則內容會以 JSON 格式傳回。

使用 Api 時，這會在瀏覽器之間提供更一致的體驗。

若要將應用程式設定為接受瀏覽器 accept 標頭，請將 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> 設為 `true`：

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a>設定格式器

需要支援其他格式的應用程式可以新增適當的 NuGet 套件，並設定支援。 輸入和輸出有個別的格式器。 [模型](xref:mvc/models/model-binding)系結會使用輸入格式器。 輸出格式器是用來格式化回應。 如需建立自訂格式器的詳細資訊，請參閱[自訂格式化](xref:web-api/advanced/custom-formatters)器。

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a>新增 XML 格式支援

使用 <xref:System.Xml.Serialization.XmlSerializer> 所執行的 XML 格式器是藉由呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>所設定：

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

上述程式碼會使用 `XmlSerializer`來序列化結果。

使用上述程式碼時，控制器方法會根據要求的 `Accept` 標頭傳回適當的格式。

### <a name="configure-systemtextjson-based-formatters"></a>設定 System.Text.Json-based 格式器

`System.Text.Json` 型格式器可以使用 `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` 來設定。

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

以每個動作為基礎的輸出序列化選項，可以使用 `JsonResult`來設定。 例如：

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>新增 Newtonsoft.Json 型 JSON 格式支援

在 ASP.NET Core 3.0 之前，會使用 `Newtonsoft.Json` 封裝來實作為預設使用的 JSON 格式器。 在 ASP.NET Core 3.0 或更新版本中，預設 JSON 格式器是以 `System.Text.Json` 為基礎。 安裝[NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 套件並在 `Startup.ConfigureServices`中進行設定，即可取得 `Newtonsoft.Json` 型格式器和功能的支援。

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

某些功能可能不適用於以 `System.Text.Json`為基礎的格式器，而且需要參考 `Newtonsoft.Json`格式的格式化程式。 如果應用程式有下列情況，請繼續使用以 `Newtonsoft.Json`為基礎的格式器：

* 使用 `Newtonsoft.Json` 的屬性。 例如，`[JsonProperty]` 或 `[JsonIgnore]`。
* 自訂序列化設定。
* 依賴 `Newtonsoft.Json` 提供的功能。
* 設定 `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`。 在 ASP.NET Core 3.0 版之前，`JsonResult.SerializerSettings` 接受 `Newtonsoft.Json` 專屬的 `JsonSerializerSettings` 執行個體。
* 產生 [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) 文件。

`Newtonsoft.Json`型格式器的功能可以使用 `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`來設定：

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

以每個動作為基礎的輸出序列化選項，可以使用 `JsonResult`來設定。 例如：

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a>新增 XML 格式支援

XML 格式設定需要[AspNetCore 的 xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet 套件。

使用 <xref:System.Xml.Serialization.XmlSerializer> 所執行的 XML 格式器是藉由呼叫 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>所設定：

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

上述程式碼會使用 `XmlSerializer`來序列化結果。

使用上述程式碼時，控制器方法應該根據要求的 `Accept` 標頭傳回適當的格式。

::: moniker-end

### <a name="specify-a-format"></a>指定格式

若要限制回應格式，請套用[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)篩選準則。 如同大部分的[篩選](xref:mvc/controllers/filters)條件，`[Produces]` 可以套用至動作、控制器或全域範圍：

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

前面的[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)篩選準則：

* 強制控制器內的所有動作傳回 JSON 格式的回應。
* 如果設定了其他格式器，而且用戶端指定了不同的格式，則會傳回 JSON。

如需詳細資訊，請參閱[篩選](xref:mvc/controllers/filters)。

### <a name="special-case-formatters"></a>特殊案例格式器

有些特殊案例是使用內建格式器所實作。 根據預設，`string` 傳回類型會格式化為*text/純文字*（如果透過 `Accept` 標頭要求，則為*text/html* ）。 藉由移除 <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>，即可刪除此行為。 `ConfigureServices` 方法中會移除格式器。 傳回 `null`時，具有模型物件傳回類型的動作會傳回 `204 No Content`。 藉由移除 <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>，即可刪除此行為。 下列程式碼會移除 `StringOutputFormatter` 和 `HttpNoContentOutputFormatter`。

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

若沒有 `StringOutputFormatter`，內建的 JSON 格式器格式 `string` 傳回類型。 如果已移除內建 JSON 格式器，而且有 XML 格式器，則 XML 格式器格式 `string` 傳回類型。 否則，`string` 傳回類型會傳回 `406 Not Acceptable`。

如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。 例如：

* JSON 格式器會傳回具有 `null`主體的回應。
* XML 格式器會傳回空的 XML 專案，並將屬性 `xsi:nil="true"` 設定。

## <a name="response-format-url-mappings"></a>回應格式 URL 對應

用戶端可以要求特定格式做為 URL 的一部分，例如：

* 在查詢字串或部分路徑中。
* 使用格式特定的副檔名，例如 .xml 或. json。

應該在 API 所使用的路由中指定要求路徑的對應。 例如：

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

先前的路由可讓要求的格式指定為選用的副檔名。 [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)屬性會檢查 `RouteData` 中的格式值是否存在，並在建立回應時，將回應格式對應至適當的格式器。

|           路由        |             Formatter              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    預設輸出格式器    |
| `/api/products/5.json` | JSON 格式器 (如果已設定) |
| `/api/products/5.xml`  | XML 格式器 (如果已設定)  |
