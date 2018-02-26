---
title: "在 ASP.NET Core MVC 中格式化回應資料"
author: ardalis
description: "了解如何在 ASP.NET Core MVC 中格式化回應資料。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/formatting
ms.openlocfilehash: 36231cd2bf59408e9c858ea99355c1e8dd859e6e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>在 ASP.NET Core MVC 中格式化回應資料簡介

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 具有使用固定格式或回應用戶端規格的內建回應資料格式化支援。

[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)。

## <a name="format-specific-action-results"></a>格式特定動作結果

某些動作結果類型是特定格式所特有的，例如 `JsonResult` 和 `ContentResult`。 動作可以傳回一律以特定方式格式化的特定結果。 例如，不論用戶端喜好設定為何，傳回 `JsonResult` 都會傳回 JSON 格式化資料。 同樣地，傳回 `ContentResult` 將會傳回純文字格式化字串資料 (就像只傳回字串一樣)。

> [!NOTE]
> 動作不要求任何特定的型別，MVC 支援任何物件的傳回值。 如果動作回傳 `IActionResult` 的實作並且「控制器」 繼承自 `Controller`，開發人員會有許多對應至多種選項的輔助方法。 回傳物件的動作結果不是 `IActionResult` 型別會使用適當的序列化 `IOutputFormatter` 實作。

若要從繼承自 `Controller` 基底類別的控制器傳回特定格式的資料，請使用內建協助程式方法 `Json` 傳回 JSON 和 `Content` (針對純文字)。 動作方法應該會傳回特定結果類型 (例如，`JsonResult`) 或 `IActionResult`。

傳回 JSON 格式化資料：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

此動作的範例回應：

![Microsoft Edge 中 [開發人員工具] 的 [網路] 索引標籤，顯示回應的內容類型為 application/json](formatting/_static/json-response.png)

請注意，在網路要求清單及 [回應標頭] 區段中，回應的內容類型都是 `application/json`。 另請注意，在 [要求標頭] 區段的 Accept 標頭中瀏覽器 (在此情況下為 Microsoft Edge) 所呈現的選項清單。 目前技術將會忽略此標頭。下面會討論其遵守方式。

若要傳回純文字格式化資料，請使用 `ContentResult` 和 `Content` 協助程式：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

此動作的回應：

![Microsoft Edge 中 [開發人員工具] 的 [網路] 索引標籤，顯示回應的內容類型為 text/plain](formatting/_static/text-response.png)

請注意，在此情況下，所傳回的 `Content-Type` 是 `text/plain`。 只要使用字串回應類型，也可以達到這個相同的行為：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> 針對具有多個傳回類型或選項 (例如，根據所執行作業結果的不同 HTTP 狀態碼) 的非一般動作，偏好使用 `IActionResult` 作為傳回類型。

## <a name="content-negotiation"></a>內容交涉

用戶端指定 [Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時，會進行內容交涉 (簡稱 *conneg*)。 ASP.NET Core MVC 預設使用 JSON 格式。 內容交涉是由 `ObjectResult` 所實作。 它也會內建到從協助程式方法 (全部都是根據 `ObjectResult`) 所傳回的狀態碼特定動作結果。 您也可以傳回模型類型 (已定義為資料傳輸類型的類別)，而且架構會自動將它包裝在 `ObjectResult` 中。

下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

除非要求另一種格式，而且伺服器可以傳回所要求的格式，否則會傳回 JSON 格式的回應。 您可以使用 [Fiddler](http://www.telerik.com/fiddler) 這類工具建立包含 Accept 標頭的要求，以及指定另一種格式。 在此情況下，如果伺服器的「格式器」可以使用所要求的格式產生回應，則會以用戶端慣用的格式傳回結果。

![Fiddler 主控台，顯示 Accept 標頭值為 application/xml 的手動建立 GET 要求](formatting/_static/fiddler-composer.png)

在上面的螢幕擷取畫面中，已使用 Fiddler Composer 來產生要求，並指定 `Accept: application/xml`。 ASP.NET Core MVC 預設僅支援 JSON；因此，即使指定另一種格式，所傳回的結果仍然會是 JSON 格式。 您將在下節中看到如何新增其他格式器。

控制器動作可以傳回 POCO (純舊 CLR 物件)，在此情況下，ASP.NET MVC 會自動建立可包裝物件的 `ObjectResult`。 用戶端會取得格式化的序列化物件 (JSON 格式是預設值；您可以設定 XML 或其他格式)。 如果所傳回的物件是 `null`，則架構將傳回 `204 No Content` 回應。

傳回物件類型：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

在此範例中，有效作者別名的要求將會收到包含作者資料的 200 OK 回應。 無效別名的要求將會收到「204 沒有內容」回應。 下面顯示以 XML 和 JSON 格式顯示回應的螢幕擷取畫面。

### <a name="content-negotiation-process"></a>內容交涉程序

只有在 `Accept` 標頭出現在要求中時，才會進行「內容交涉」。 要求包含 Accept 標頭時，架構會依喜好設定順序來列舉 Accept 標頭中的媒體類型，並嘗試尋找可產生回應的格式器，而回應的格式為 Accept 標頭所指定的其中一種格式。 如果找不到可滿足用戶端要求的格式器，架構會嘗試尋找第一個可產生回應的格式器 (除非開發人員已在 `MvcOptions` 上設定選項，改為傳回「406 無法接受」)。 如果要求指定 XML，但尚未設定 XML 格式器，則會使用 JSON 格式器。 更常見地是，如果未設定格式器來提供所要求的格式，則會使用可格式化物件的第一個格式器。 如果未指定標頭，則會使用可處理要傳回之物件的第一個格式器來序列化回應。 在此情況下，無法進行任何交涉，伺服器將會判斷所使用的格式。

> [!NOTE]
> 如果 Accept 標頭包含 `*/*`，則除非 `MvcOptions` 上的 `RespectBrowserAcceptHeader` 設定為 true，否則會忽略標頭。

### <a name="browsers-and-content-negotiation"></a>瀏覽器和內容交涉

與一般 API 用戶端不同，網頁瀏覽器會提供 `Accept` 標頭，內含廣泛的格式陣列 (包括萬用字元)。 根據預設，架構偵測到要求來自瀏覽器時，將會忽略 `Accept` 標頭，並改為傳回應用程式中已設定預設格式 (除非另外設定，否則為 JSON ) 的內容。 使用不同的瀏覽器來採用 API 時，這提供更一致的經驗。

如果您想要應用程式採用瀏覽器 Accept 標頭，則可以將這設定為 MVC 組態的一部分，方法是在 *Startup.cs* 的 `ConfigureServices` 方法中將 `RespectBrowserAcceptHeader` 設定為 `true`。

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>設定格式器

如果您的應用程式需要支援 JSON 預設值以外的其他格式，則可以新增 NuGet 套件，並設定 MVC 來支援它們。 輸入和輸出有個別的格式器。 [模型繫結](model-binding.md)會使用輸入格式器；輸出格式器是用來格式化回應。 您也可以設定[自訂格式器](../advanced/custom-formatters.md)。

### <a name="adding-xml-format-support"></a>新增 XML 格式支援

若要新增 XML 格式支援，請安裝 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。

將 XmlSerializerFormatters 新增至 *Startup.cs* 中的 MVC 組態：

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

或者，您可以只新增輸出格式器：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

這兩種方法將使用 `System.Xml.Serialization.XmlSerializer` 來序列化結果。 如果想要的話，可以新增其相關聯的格式器來使用 `System.Runtime.Serialization.DataContractSerializer`：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

新增 XML 格式支援之後，控制器方法應該會根據要求的 `Accept` 標頭來傳回適當的格式，如這個 Fiddler 範例所示範：

![Fiddler 主控台：要求的 [原始] 索引標籤會顯示 Accept 標頭值 application/xml。 回應的 [原始] 索引標籤會顯示 Content-Type 標頭值 application/xml。](formatting/_static/xml-response.png)

您可以在 [偵測器] 索引標籤中看到已設定 `Accept: application/xml` 標頭來提出原始 GET 要求。 回應窗格會顯示 `Content-Type: application/xml` 標頭，並已將 `Author` 物件序列化為 XML。

使用 [編輯器] 索引標籤，修改在 `Accept` 標頭中指定 `application/json` 的要求。 執行要求，並將回應格式化為 JSON：

![Fiddler 主控台：要求的 [原始] 索引標籤會顯示 Accept 標頭值 application/json。 回應的 [原始] 索引標籤會顯示 Content-Type 標頭值 application/json。](formatting/_static/json-response-fiddler.png)

在此螢幕擷取畫面中，您可以看到要求設定 `Accept: application/json` 標頭，而回應指定與其 `Content-Type` 相同的值。 `Author` 物件會以 JSON 格式顯示在回應本文中。

### <a name="forcing-a-particular-format"></a>強制執行特定格式

如果您想要限制特定動作的回應格式，則可以套用 `[Produces]` 篩選。 `[Produces]` 篩選可指定特定動作 (或控制器) 的回應格式。 與大部分[篩選](../controllers/filters.md)類似，這可以套用至動作、控制器或全域範圍。

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` 篩選將強制執行 `AuthorsController` 內的所有動作，以傳回 JSON 格式化回應，即使已設定應用程式和用戶端的其他格式器也是一樣，但前提是 `Accept` 標頭要求不同且可用的格式。 若要深入了解，請參閱[篩選](../controllers/filters.md) (包括如何全域套用篩選)。

### <a name="special-case-formatters"></a>特殊案例格式器

有些特殊案例是使用內建格式器所實作。 根據預設，`string` 傳回類型將會格式化為 *text/plain* (如果透過 `Accept` 標頭要求，則為 *text/html*)。 移除 `TextOutputFormatter`，即可移除此行為。 您可以在 *Startup.cs* 的 `Configure` 方法中移除格式器 (如下所示)。 傳回 `null` 時，具有模型物件傳回類型的動作會傳回「204 沒有內容」回應。 移除 `HttpNoContentOutputFormatter`，即可移除此行為。 下列程式碼會移除 `TextOutputFormatter` 和 `HttpNoContentOutputFormatter`。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

例如，如果沒有 `TextOutputFormatter`，則 `string` 傳回類型會傳回「406 無法接受」。 請注意，如果存在 XML 格式器，則移除 `TextOutputFormatter` 時，會格式化 `string` 傳回類型。

如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。 例如，JSON 格式器只會傳回本文為 `null` 的回應，而 XML 格式器將會傳回已設定屬性 `xsi:nil="true"` 的空白 XML 項目。

## <a name="response-format-url-mappings"></a>回應格式 URL 對應

用戶端可以要求特定格式作為 URL 的一部分 (例如在查詢字串中或作為路徑的一部分)，或是使用格式特定副檔名 (例如 .xml 或 .json)。 應該在 API 所使用的路由中指定要求路徑的對應。 例如: 

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

此路由可將所要求的格式指定為選擇性副檔名。 `[FormatFilter]` 屬性會檢查 `RouteData` 中是否有格式值，並在建立回應時將回應格式對應至適當的格式器。

| 路由                      | 格式器                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | 預設輸出格式器       |
| `/products/GetById/5.json` | JSON 格式器 (如果已設定) |
| `/products/GetById/5.xml`  | XML 格式器 (如果已設定)  |
