---
title: "ASP.NET Core MVC 中的格式化回應資料"
author: ardalis
description: "了解如何設定 ASP.NET Core MVC 中的回應資料的格式。"
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
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>ASP.NET Core MVC 中的格式化回應資料簡介

由[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC 有內建支援來格式化回應資料，使用固定的格式，或在偵測到用戶端規格。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)。

## <a name="format-specific-action-results"></a>特定格式的動作結果

某些動作結果型別特有的特定格式，例如`JsonResult`和`ContentResult`。 動作可傳回特定結果永遠以特定方式格式化。 例如，傳回`JsonResult`會傳回 JSON 格式的資料，用戶端喜好設定無關。 同樣地，傳回`ContentResult`（如將只傳回一個字串），就會傳回純文字格式的字串資料。

> [!NOTE]
> 不需要採取動作，來傳回任何特定的型別。MVC 支援任何物件的傳回值。 如果動作傳回`IActionResult`實作與 「 控制器 」 繼承自`Controller`，開發人員有許多對應至數個選項的 helper 方法。 傳回物件的動作結果不是`IActionResult`型別會使用適當的序列化`IOutputFormatter`實作。

若要以特定格式傳回資料的控制站，繼承自`Controller`基底類別，請使用內建的協助程式方法`Json`傳回 JSON 和`Content`純文字。 動作方法應該傳回特定結果型別 (例如， `JsonResult`) 或`IActionResult`。

傳回 JSON 格式的資料：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

從這個動作的範例回應：

![網路索引標籤的開發人員工具的 Microsoft Edge 顯示回應的內容類型為 application/json](formatting/_static/json-response.png)

請注意，回應的內容類型為`application/json`，清單中的網路要求和回應標頭區段中所示。 也請注意顯示 （在此情況下，Microsoft Edge） Accept 標頭在要求標頭 區段中，瀏覽器選項的清單。 目前的技術會忽略此標頭。以下將討論服從它。

若要傳回格式化的純文字資料，請使用`ContentResult`和`Content`helper:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

來自此動作的回應：

![在 Microsoft Edge 顯示回應的內容類型的開發人員工具的 [網路] 索引標籤是文字/純文字](formatting/_static/text-response.png)

請注意在此情況下`Content-Type`傳回`text/plain`。 您也可以達到相同的行為使用剛才字串回應類型：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> 非簡單式具有多個動作的傳回類型或選項 （例如，根據所執行作業的結果不同的 HTTP 狀態碼）、 偏好`IActionResult`當做傳回型別。

## <a name="content-negotiation"></a>內容交涉

內容交涉 (*conneg*簡稱) 發生於用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)。 使用 ASP.NET Core MVC 的預設格式為 JSON。 內容交涉由實作`ObjectResult`。 它也內建的 helper 方法的傳回特定的動作結果的狀態碼 (其中都以基礎`ObjectResult`)。 您也可以傳回的模型型別 （類別已定義為您的資料傳輸類型） 和架構會自動將其包裝在`ObjectResult`您。

下列動作方法會使用`Ok`和`NotFound`helper 方法：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

除非要求另一種格式，而且伺服器可以傳回要求的格式，將會傳回 JSON 格式的回應。 您可以使用這類工具[Fiddler](http://www.telerik.com/fiddler)建立包含 Accept 標頭的要求，並指定另一種格式。 在此情況下，如果伺服器有*格式器*可以產生的回應要求的格式，將會傳回結果中的用戶端慣用的格式。

![Fiddler 主控台顯示手動建立取得與應用程式/xml 的 Accept 標頭值的要求](formatting/_static/fiddler-composer.png)

在上面的螢幕擷取畫面，Fiddler 編輯器已經用來產生要求，指定`Accept: application/xml`。 根據預設，ASP.NET Core MVC 僅支援 JSON，所以即使指定另一種格式，則傳回的結果時，仍 JSON 格式。 您會看到如何在下一節中加入其他格式子。

控制器動作可以傳回 POCOs （純舊 CLR 物件），將會自動建立 ASP.NET MVC`ObjectResult`您會包裝物件。 用戶端會取得格式化的序列化的物件 （JSON 格式是預設值; 您可以設定 XML 或其他格式）。 如果物件傳回為`null`，則會傳回架構`204 No Content`回應。

傳回的物件類型：

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

在範例中，有效的作者別名的要求將會接收 200 OK 回應作者的資料。 無效的別名的要求將會收到 「 204 沒有內容的回應。 螢幕擷取畫面顯示回應 XML 和 JSON 的格式如下所示。

### <a name="content-negotiation-process"></a>內容交涉程序

內容*交涉*才只會發生`Accept`標頭會出現在要求中。 當要求包含 accept 標頭時，架構會列舉 accept 標頭中的喜好設定順序中的媒體類型，並會嘗試尋找可能會產生其中一種 accept 標頭所指定格式的回應格式器。 如果找到不到格式器可以滿足用戶端的要求，架構會嘗試尋找第一個可能會產生回應的格式器 (除非開發人員已設定選項上`MvcOptions`傳回 406 無法接受改為)。 如果要求指定的 XML，但尚未設定的 XML 格式器，則會使用 JSON 格式器。 較常見地，如果設定不到格式器，可以提供要求的格式，然後使用可以格式化物件的第一個格式器。 如果沒有標頭，可以處理的物件要傳回的第一個格式器將用來序列化回應中。 在此情況下，沒有任何交涉進行-伺服器會判斷它會使用何種格式。

> [!NOTE]
> 如果 Accept 標頭包含`*/*`，將忽略標頭，除非`RespectBrowserAcceptHeader`設為 true `MvcOptions`。

### <a name="browsers-and-content-negotiation"></a>瀏覽器和內容交涉

不同於一般的 API 用戶端 web 瀏覽器通常會提供`Accept`包含廣泛的格式，包括萬用字元的標頭。 根據預設，當架構偵測到在要求來自瀏覽器中，它將會忽略`Accept`頁首和改為傳回應用程式中的內容所設定的預設格式 (JSON 除非否則設定)。 當您使用不同的瀏覽器中使用 Api 時，這會提供更一致的經驗。

如果您想要使用您的應用程式實行瀏覽器 accept 標頭，您可以設定這為 MVC 的組態一部分藉由設定`RespectBrowserAcceptHeader`至`true`中`ConfigureServices`方法中的*Startup.cs*。

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>設定格式器

若您的應用程式需要支援 JSON 的預設值以外的其他格式，您可以新增 NuGet 封裝，並設定 MVC 支援它們。 有個別的輸入和輸出的格式器。 輸入的格式器所使用的[模型繫結](model-binding.md); 輸出格式器會用來格式化回應。 您也可以設定[自訂格式器](../advanced/custom-formatters.md)。

### <a name="adding-xml-format-support"></a>加入 XML 格式的支援

若要新增為 XML 格式的支援，請安裝`Microsoft.AspNetCore.Mvc.Formatters.Xml`NuGet 封裝。

MVC 的設定中新增 XmlSerializerFormatters *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

或者，您可以加入只輸出格式器：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

這兩種方法將序列化結果使用`System.Xml.Serialization.XmlSerializer`。 如果您想要的話，您可以使用`System.Runtime.Serialization.DataContractSerializer`加上其相關聯的格式器：

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

一旦您已經加入支援 XML 格式，控制器方法應該傳回適當的格式會根據要求`Accept`標頭，為這個範例示範的 Fiddler:

![Fiddler 主控台： 要求的原始索引標籤會顯示 Accept 標頭值為 application/xml。 未經處理的索引標籤，回應會顯示應用程式/xml 的內容類型標頭值。](formatting/_static/xml-response.png)

您可以在 [偵測器] 索引標籤將未經處理的 GET 要求進行時看到`Accept: application/xml`標頭集合。 回應 窗格顯示`Content-Type: application/xml`標頭，而`Author`物件序列化為 XML。

使用編輯器索引標籤，將要求修改成指定`application/json`中`Accept`標頭。 執行要求，並將會格式化為 JSON 的回應：

![Fiddler 主控台： 要求的原始索引標籤會顯示 Accept 標頭值為 application/json。 未經處理的索引標籤，回應會顯示應用程式/json 的 Content-type 標頭值。](formatting/_static/json-response-fiddler.png)

在這個螢幕擷取畫面中，您可以看到要求設定的標頭`Accept: application/json`並回應指定相同其`Content-Type`。 `Author`物件如下所示的 JSON 格式的回應主體。

### <a name="forcing-a-particular-format"></a>強制執行特定的格式

如果您想要限制特定動作的回應格式可以您可以套用`[Produces]`篩選器。 `[Produces]`篩選條件會指定特定的動作 （或控制站） 的回應格式。 像大部分[篩選](../controllers/filters.md)，這可套用在動作、 控制器或全域範圍。

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]`篩選將會強制執行內的所有動作`AuthorsController`傳回 JSON 格式的回應，如果應用程式和用戶端提供的已設定為其他格式子`Accept`要求不同的標頭可用格式。 請參閱[篩選](../controllers/filters.md)如需詳細資訊，包括如何套用篩選器。

### <a name="special-case-formatters"></a>特殊案例的格式器

會實作某些特殊情況下，使用內建的格式器。 根據預設，`string`傳回型別將會格式化為*text/plain* (*text/html*要求透過`Accept`標頭)。 此行為可以藉由移除移除`TextOutputFormatter`。 移除中的格式器`Configure`方法中的*Startup.cs* （如下所示）。 有模型物件的動作傳回型別時將會傳回 204 沒有內容的回應傳回`null`。 此行為可以藉由移除移除`HttpNoContentOutputFormatter`。 下列程式碼移除`TextOutputFormatter`和`HttpNoContentOutputFormatter`。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

不含`TextOutputFormatter`，`string`傳回型別傳回 406 無法接受的例如。 請注意，如果存在 XML 格式器，它將會格式化`string`傳回型別，如果`TextOutputFormatter`已移除。

不含`HttpNoContentOutputFormatter`，null 物件的格式設定的格式器。 比方說，JSON 格式器只會傳回的回應主體包含`null`，而 XML 格式器將會傳回空的 XML 項目與屬性`xsi:nil="true"`設定。

## <a name="response-format-url-mappings"></a>回應格式的 URL 對應

用戶端可以要求特定格式一部分使用的 URL，例如查詢字串或組件的路徑，或使用特定格式的檔案副檔名，例如.xml 或.json 中。 從要求路徑對應應指定應用程式開發介面使用的路由。 例如: 

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

此路由會讓要求的格式指定為選擇性的副檔名。 `[FormatFilter]`屬性檢查中的格式值是否存在`RouteData`和建立回應時，將對應的回應格式至適當的格式器。

| 路由                      | 格式器                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | 預設的輸出格式器       |
| `/products/GetById/5.json` | JSON 格式子 （如果有設定） |
| `/products/GetById/5.xml`  | XML 格式器 （如果有設定）  |
