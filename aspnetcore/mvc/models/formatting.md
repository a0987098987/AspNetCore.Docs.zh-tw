---
title: "ASP.NET Core MVC 的格式化回應資料"
author: ardalis
description: "了解如何設定 ASP.NET Core MVC 回應資料的格式。"
keywords: "ASP.NET Core、 回應資料，IOutputFormatter、 IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>ASP.NET Core MVC 的格式化回應資料簡介

由[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 內建支援，可以使用固定的格式或者偵測來自用戶端的規格，從而將回應資料格式化。

[檢視或下載 GitHub 範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)。

## <a name="format-specific-action-results"></a>特定格式的動作結果

某些動作結果型別有特定的格式，例如`JsonResult`和`ContentResult`。系統始終會透過特定的方式來將動作所能傳回的結果格式化。例如，傳回`JsonResult`時，無論用戶端的喜好設定為何，都只會傳回的 JSON 格式的資料。同樣地，傳回`ContentResult`時，就只會傳回純文字格式的字串資料 (單一字串)。

> [!NOTE]
> 動作不要求任何特定的型別，MVC 支援任何物件的傳回值。 如果動作回傳`IActionResult`的實作並且「控制器」 繼承自`Controller`，開發人員會有許多對應至多種選項的輔助方法。回傳物件的動作結果不是`IActionResult`型別會使用適當的序列化`IOutputFormatter`實作。

繼承自`Controller`基底類別的控制器，若要以特定格式傳回資料，請使用內建的輔助方法`Json`來傳回 JSON，或使用`Content`來傳回純文字。你的動作方法應傳回特定結果型別 (例如`JsonResult`) 或`IActionResult`。

傳回 JSON 格式的資料：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

此動作的回應範例：

![網路索引標籤的開發人員工具的 Microsoft Edge 顯示回應的內容類型為 application/json](formatting/_static/json-response.png)

請注意，上述範例的回應內容類型為`application/json`，可參考網路要求清單以及回應標頭區段。也請注意在瀏覽器的選項清單中（在此情況下，Microsoft Edge），要求標頭區段的 Accept 標頭，目前的技術會忽略此標頭，以下將討論它。

若要回傳純文字格式的資料，請使用`ContentResult`和`Content`輔助方法:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

來自此動作的回應：

![在 Microsoft Edge 顯示回應的內容類型的開發人員工具的 [網路] 索引標籤是文字/純文字](formatting/_static/text-response.png)

請注意在此情況下`Content-Type`傳回`text/plain`，您也可以使用字串作為回應類型，達到相同的行為：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> 對於非簡單式且具有多個動作的傳回類型或選項（例如，根據所執行作業的結果不同而回傳不同的 HTTP 狀態碼），建議使用`IActionResult`當做傳回型別。

## <a name="content-negotiation"></a>內容交涉

內容交涉 (Content Negotiation) 發生於用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)的時候。ASP.NET Core MVC 預設使用 JSON 格式。內容交涉由`ObjectResult`實作，它也內建多種輔助方法，用來回傳特定的動作結果及狀態碼 (都以`ObjectResult`為基礎)。您也可以傳回的模型型別（以您所定義的類別作為資料傳輸類型），框架會自動幫您將其包裝成在`ObjectResult`。

下列動作方法使用`Ok`和`NotFound`輔助方法：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

除非要求另一種可伺服器傳回的格式，否則就會傳回 JSON 格式的回應。您可以使用 [Fiddler](http://www.telerik.com/fiddler) 這類的工具，建立包含 Accept 標頭的要求，並指定另一種格式，在此情況下，如果伺服器有格式器可以產生回應要求的格式，就能使用用戶端喜好的格式傳回結果。

![Fiddler 主控台顯示手動建立取得與應用程式/xml 的 Accept 標頭值的要求](formatting/_static/fiddler-composer.png)

在上面的螢幕擷取畫面中，已使用 Fiddler 編輯器來產生了一個指定著`Accept: application/xml`的要求。 ASP.NET Core MVC 預設僅支援 JSON，所以即使指定另一種格式，傳回的結果仍是 JSON 格式。下一節中您會看到如何新增其他格式器。

控制器動作可以回傳 POCOs（Plain Old CLR Objects），ASP.NET MVC 將會自動幫您將物件包裝成`ObjectResult`。用戶端會取得序列化後的物件格式（預設值是 JSON 格式；您可以設定成 XML 或其他格式）。如果物件回傳`null`，則框架會傳回`204 No Content`回應。

傳回物件類型：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

如範例所示，如果要求的是有效的作者別名，就會收到 `200 OK` 以及作者的資料作為回應。如果要求的是無效的別名，則會收到`204 No Content`的回應。下方所顯示的螢幕結取化麵包括了 XML 和 JSON 的回應。

### <a name="content-negotiation-process"></a>內容交涉程序

*內容交涉*只會發生在`Accept`標頭會出現於要求中時，當要求包含 accept 標頭時，框架會列舉 accept 標頭中的喜好設定順序的媒體類型，並會嘗試尋找可能會產生其中一種 accept 標頭所指定格式的回應格式器。如果找到不到格式器可以滿足用戶端的要求，框架會嘗試尋找第一個可能會產生回應的格式器（除非開發人員修改`MvcOptions`選項，設定回傳 `406 Not Acceptable`)。如果要求指定的 XML，但尚未設定 XML 格式器，則會使用 JSON 格式器。一般來說，如果找不到可以提供要求格式的格式器，會使用第一個可格式化該物件的格式器。如果沒有標頭，會使用第一個可以處理該物件的格式器，來序列化回應。在此情況下，沒有任何交涉進行，伺服器會判斷它適合使用何種格式。

> [!NOTE]
> 如果 Accept 標頭包含`*/*`，將忽略標頭，除非`MvcOptions`的`RespectBrowserAcceptHeader`設為 true。

### <a name="browsers-and-content-negotiation"></a>瀏覽器和內容交涉

不同於一般的 API 用戶端，Web 瀏覽器所提供的`Accept`標頭通常會包含較廣泛的格式，包括使用萬用字元的標頭。根據預設，當架構偵測到來自瀏覽器的要求時，便會會忽略`Accept`標頭，並改為傳回應用程式中所設定的內容預設格式 (預設為 JSON)。當您透過不同的瀏覽器來使用 Api 時，這會提供更一致的經驗。

若要讓您的應用程式使用瀏覽器的 Accept 標頭，您可以在 MVC 組態中設定，在 Startup.cs 的`ConfigureServices`方法中，將 MVC 的組態選項 `RespectBrowserAcceptHeader`設定為`true`。

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>設定格式器

若您的應用程式需要支援 JSON 預設值以外的其他格式，您可以新增 NuGet 套件，並設定 MVC 加以支援。NuGet 上有多種輸入和輸出的格式器。輸入的格式器將用於[模型繫結](model-binding.md)；輸出格式器則用來將回應格式化。您也可以設定[自訂格式器](../advanced/custom-formatters.md)。

### <a name="adding-xml-format-support"></a>加入 XML 格式的支援

若要支援 XML 格式，請安裝`Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet 套件。

在 *Startup.cs* 的 MVC 設定中新增 XmlSerializerFormatters:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

或者，您也可以只加入輸出格式器：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

這兩種方法都將使用`System.Xml.Serialization.XmlSerializer`來將結果序列化。如果想改用`System.Runtime.Serialization.DataContractSerializer`的話，加上與相關聯的格式器即可：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

只要能支援 XML 格式，控制器方法應該就能根據要求的 Accept 標頭來回傳適當的格式，如同這個 Fiddler 範例所示範的:

![Fiddler 主控台： 要求的原始索引標籤會顯示 Accept 標頭值為 application/xml。 未經處理的索引標籤，回應會顯示應用程式/xml 的內容類型標頭值。](formatting/_static/xml-response.png)

您可以在 [Inspectors] 索引標籤的 [Raw] 中，看到未經處理的 GET 要求中包含了`Accept: application/xml`標頭。回應窗格顯示`Content-Type: application/xml`標頭，而`Author`物件已序列化為 XML 格式。

使用編輯器索引標籤來修改要求，藉此在`Accept`標頭中指定`application/json`。此後執行要求，系統便會將回應的物件格式化為 JSON 格式：

![Fiddler 主控台： 要求的原始索引標籤會顯示 Accept 標頭值為 application/json。 未經處理的索引標籤，回應會顯示應用程式/json 的 Content-type 標頭值。](formatting/_static/json-response-fiddler.png)

在這個螢幕擷取畫面中，您可以看到要求設定的標頭為`Accept: application/json`，並回應相同的`Content-Type`。回應主體中的`Author`物件是以 JSON 格式呈現。

### <a name="forcing-a-particular-format"></a>強制執行特定的格式

您可以套用`[Produces]`篩選器來限制特定動作的回應格式。`[Produces]`篩選條件能為特定動作 (或控制器站) 指定特定的回應格式。與大部分的[篩選器](../controllers/filters.md)一樣，此功能可套用於動作、控制器或全域範圍。

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]`篩選將會強制執行內的所有動作`AuthorsController`傳回 JSON 格式的回應，如果應用程式和用戶端提供的已設定為其他格式子`Accept`要求不同的標頭可用格式。 請參閱[篩選](../controllers/filters.md)如需詳細資訊，包括如何套用篩選器。

### <a name="special-case-formatters"></a>特殊案例的格式器

某些特殊情況下會使用內建的格式器實作。根據預設，傳回型別為`string`時，將會格式化為*text/plain* (或透過要求的`Accept`標頭格式化成*text/html*)。此行為可以藉由移除`TextOutputFormatter`來取消。您可以在*Startup.cs*的`Configure`方法中移除該格式器（如下所示）。當動作回傳的模型物件值為`null`時，會回傳 `204 No Content`回應。此行為可以藉由移除`HttpNoContentOutputFormatter`來取消。下列程式碼顯示如何移除`TextOutputFormatter`和`HttpNoContentOutputFormatter`。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

舉例來說，移除`TextOutputFormatter`後，傳回型別為`string`時將傳回 `406 Not Acceptable`。請注意，如果移除`TextOutputFormatter`後，尚存在 XML 格式器，傳回型別將會格式化成`string`。

移除`HttpNoContentOutputFormatter`後，null 物件的格式會依據不同的格式器而改變。 比方說，JSON 格式器只會傳回包含`null`的回應主體，而 XML 格式器則會傳回空的 XML 項目與屬性`xsi:nil="true"`設定。

## <a name="response-format-url-mappings"></a>回應格式的 URL 對應

用戶端可以在 URL 中要求特定格式，例如查詢字串或組件的路徑，或使用特定格式的檔案副檔名，例如.xml 或.json。這需要在 API 路由路徑中特別指定對應方式，例如：

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

此路由允許您將要求的格式指定為選擇性的副檔名。`[FormatFilter]`屬性會檢查格式值是否存在於`RouteData`，並根據回應格式挑選適當的格式器。

| 路由                      | 格式器                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | 預設的輸出格式器       |
| `/products/GetById/5.json` | JSON 格式器 （如果有設定） |
| `/products/GetById/5.xml`  | XML 格式器 （如果有設定）  |
