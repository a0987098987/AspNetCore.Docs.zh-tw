---
title: 在 ASP.NET Core Web API 中格式化回應資料
author: ardalis
description: 了解如何在 ASP.NET Core Web API 中格式化回應資料。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 04/17/2020
uid: web-api/advanced/formatting
ms.openlocfilehash: 392e4905126ffb6801cc55055f1d511f5fa99dd1
ms.sourcegitcommit: 3d07e21868dafc503530ecae2cfa18a7490b58a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642701"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>在 ASP.NET Core Web API 中格式化回應資料

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

ASP.NET核心 MVC 支援格式化回應數據。 回應數據可以使用特定格式或回應用戶端請求的格式進行格式化。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting)([如何下載](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>特定於格式的操作結果

某些動作結果類型是特定格式所特有的，例如 <xref:Microsoft.AspNetCore.Mvc.JsonResult> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult>。 操作可以返回以特定格式格式化的結果,而不考慮用戶端首選項。 例如,返回`JsonResult`返回 JSON 格式的數據。 傳`ContentResult`回或字串傳回純文字格式的字串資料。

返回任何特定類型不需要操作。 ASP.NET核心支援任何物件返回值。  返回不是<xref:Microsoft.AspNetCore.Mvc.IActionResult>類型的物件的操作的結果使用適當的<xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter>實現進行序列化。 如需詳細資訊，請參閱 <xref:web-api/action-return-types>。

內建說明器方法<xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*>傳回 JSON 格式的資料:[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]

範例下載返回作者清單。 使用 F12 瀏覽器開發人員工具或[Postman](https://www.getpostman.com/tools)與前面的代碼:

* 將顯示包含**內容類型的**`application/json; charset=utf-8`回應標頭:
* 將顯示請求標頭。 例如,標頭`Accept`。 前面的`Accept`代碼將忽略標頭。

若要傳回純文字格式化資料，請使用 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> 協助程式：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

在前面的代碼中,`Content-Type`傳回`text/plain`的為 。 傳回字串的傳`Content-Type``text/plain`回字串的 :

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

對於具有多個傳回類型的操作,`IActionResult`傳回 。 例如,根據執行的操作的結果返回不同的 HTTP 狀態代碼。

## <a name="content-negotiation"></a>內容協商

當用戶端指定[「接受」標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時,將發生內容協商。 ASP.NET核心使用的預設格式是[JSON。](https://json.org/) 內容要用:

* 由<xref:Microsoft.AspNetCore.Mvc.ObjectResult>實現。
* 內置於從幫助器方法返回的狀態代碼特定操作結果中。 操作結果說明器方法`ObjectResult`。

傳回模型類型時,傳回`ObjectResult`類型為 。

下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

預設情況下,ASP.NET核心支援`application/json`和`text/json``text/plain`和媒體類型。 [Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/tools)`Accept`等工具可以 設定請求標頭以指定傳回格式。 當`Accept`標頭包含伺服器支援的類型時,將返回該類型。 下一節將演示如何添加其他事務。

控制器操作可以返回 POCO(普通舊 CLR 物件)。 傳回 POCO 時,執行時會自動建立環`ObjectResult`繞物件的 。 用戶端獲取格式化的序列化物件。 如果返回的物件為`null`,則`204 No Content`返回 回應。

傳回物件類型：

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

在前面的代碼中,請求有效的作者別名返回包含作者數據的`200 OK`回應。 不合法的要求傳回回應`204 No Content`。

### <a name="the-accept-header"></a>Accept 標頭

當*negotiation*`Accept`請求中出現標頭時,將進行內容協商。 當請求包含接受標頭時,ASP.NET核心:

* 按首選項順序枚舉接受標頭中的介質類型。
* 嘗試尋找格式化,該事件可以指定的格式之一生成回應。

如果找不到能夠滿足客戶請求的事項,ASP.NET核心:

* 如果`406 Not Acceptable`<xref:Microsoft.AspNetCore.Mvc.MvcOptions>已設定,則傳回 , 或
* 嘗試查找可以產生回應的第一個事件。

如果未為請求的格式配置任何格式化,則將使用第一個可以格式化物件的格式化的格式化。 如果要求`Accept`中未顯示標頭:

* 可以處理物件的第一個要件用於序列化回應。
* 沒有任何談判正在進行。 伺服器正在確定要返回的格式。

如果 Accept 標`*/*`頭包含 ,則將`RespectBrowserAcceptHeader`忽略標頭<xref:Microsoft.AspNetCore.Mvc.MvcOptions>,除非在 上 設置為 true。

### <a name="browsers-and-content-negotiation"></a>瀏覽器與內容協商

與典型的 API 用戶端不同,Web`Accept`瀏覽器提供 標頭。 Web 瀏覽器指定多種格式,包括通配符。 預設情況下,當框架檢測到請求來自瀏覽器時:

* 將`Accept`忽略標頭。
* 除非另有配置,否則內容將返回 JSON。

這在使用 API 時跨瀏覽器提供了更一致的體驗。

要設定應用以尊重瀏覽器接受標頭,請設定為<xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader>`true`:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a>設定要事項

需要支援其他格式的應用可以添加適當的 NuGet 包並配置支援。 輸入和輸出有個別的格式器。 模型繫結用等事項的[輸入](xref:mvc/models/model-binding)。 輸出事務用於格式化回應。 有關建立自訂事件的資訊,請參閱[自訂事件](xref:web-api/advanced/custom-formatters)。

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a>新增 XML 格式支援

使用<xref:System.Xml.Serialization.XmlSerializer>XML 透過呼叫<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>設定:

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

前面的代碼使用`XmlSerializer`序列化結果。

使用上述代碼時,控制器方法基於請求的`Accept`標頭返回適當的格式。

### <a name="configure-systemtextjson-based-formatters"></a>設定 System.Text.Json-based 格式器

`System.Text.Json` 型格式器可以使用 `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` 來設定。

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.JsonSerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.JsonSerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

輸出序列化選項,以每個操作,可以使用設定使用`JsonResult`。 例如：

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

在ASP.NET Core 3.0 之前,預設`Newtonsoft.Json`使用 JSON 處理使用包實現的事項。 在 ASP.NET Core 3.0 或更新版本中，預設 JSON 格式器是以 `System.Text.Json` 為基礎。 通過安裝`Newtonsoft.Json` [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 包並將`Startup.ConfigureServices`其配置在 中,可以支援基於的 for 物質和功能。

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

某些功能可能無法很好地處理`System.Text.Json`基於的工件,並且需要`Newtonsoft.Json`參考 基於的工件。 `Newtonsoft.Json`如果套用:

* 使用`Newtonsoft.Json`屬性。 例如，`[JsonProperty]` 或 `[JsonIgnore]`。
* 自定義序列化設置。
* 依賴於`Newtonsoft.Json`提供的功能。
* 設定 `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`。 在 ASP.NET Core 3.0 版之前，`JsonResult.SerializerSettings` 接受 `Newtonsoft.Json` 專屬的 `JsonSerializerSettings` 執行個體。
* 產生 [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) 文件。

可以使用`Newtonsoft.Json`以下設定的要事的項目`Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerSettings.Converters.Add(new MyCustomJsonConverter());
});
```

輸出序列化選項,以每個操作,可以使用設定使用`JsonResult`。 例如：

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

XML 格式需要[Microsoft.AspNetCore.Mvc.formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet 包。

使用<xref:System.Xml.Serialization.XmlSerializer>XML 透過呼叫<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>設定:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

前面的代碼使用`XmlSerializer`序列化結果。

使用上述代碼時,控制器方法應基於請求的`Accept`標頭返回適當的格式。

::: moniker-end

### <a name="specify-a-format"></a>指定格式

要限制回應格式,請[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)應用篩選器。 與大多數[篩選器](xref:mvc/controllers/filters)`[Produces]`一樣 ,可以在操作、控制器或全域作用域中應用:

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

前面的[`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute)篩選器:

* 強制控制器中的所有操作返回 JSON 格式的回應。
* 如果配置了其他格式化,並且客戶端指定了不同的格式,則返回 JSON。

有關詳細資訊,請參閱[篩選器](xref:mvc/controllers/filters)。

### <a name="special-case-formatters"></a>特殊案例

有些特殊案例是使用內建格式器所實作。 預設情況下,`string`傳回類型格式為*文字/純*`Accept`(如果透過標頭請求 *,則文字/html)。* 可以通過刪除<xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>來 刪除此行為。 在`ConfigureServices`方法中刪除事點。 具有模型物件的操作傳回類型`204 No Content`傳回`null`時傳回 。 可以通過刪除<xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>來 刪除此行為。 下列程式碼會移除 `StringOutputFormatter` 和 `HttpNoContentOutputFormatter`。

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

如果沒有`StringOutputFormatter`,內置 JSON`string`格式化格式 返回類型。 如果刪除內建 JSON 格式化,並且 XML 格式化可用,則`string`XML 格式化格式 將返回類型。 否則,`string`傳回`406 Not Acceptable`型態傳回 。

如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。 例如：

* JSON 事所返回具有 正`null`文的回應。
* XML 前物質返回具有`xsi:nil="true"`屬性 集的空 XML 元素。

## <a name="response-format-url-mappings"></a>回應格式 URL 對應

用戶端可以要求特定格式作為網址的一部分,例如:

* 在查詢字串或路徑的一部分中。
* 通過使用特定於格式的檔副檔名,如 .xml 或 .json。

應該在 API 所使用的路由中指定要求路徑的對應。 例如：

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

前面的路由允許將請求的格式指定為可選檔案擴展名。 屬性[`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)檢查 格式值`RouteData`是否存在, 並在建立回應時將回應格式映射到相應的格式化。

|           路由        |             格式器              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    預設輸出格式器    |
| `/api/products/5.json` | JSON 格式器 (如果已設定) |
| `/api/products/5.xml`  | XML 格式器 (如果已設定)  |
