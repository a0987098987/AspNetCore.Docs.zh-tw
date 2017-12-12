---
title: "模型繫結"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899763613a97
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 40aa105dcf06b269025d0c44e5cd7bffef271e9d
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2017
---
# <a name="model-binding"></a>模型繫結

由[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>模型繫結的簡介

在 ASP.NET Core MVC 模型繫結會對應至動作方法參數的資料從 HTTP 要求。 參數可能是簡單類型，例如字串、 整數或浮點數，或者可能是複雜類型。 這是 MVC 的絕佳功能，因為內送資料對應到對應的是經常重複的情況下，無論資料的大小或複雜度。 MVC 再抽離繫結，因此開發人員不需要保留重寫稍有不同版本的每個應用程式中相同的程式碼，來解決這個問題。 撰寫您自己的文字類型轉換子的程式碼過於冗長，而且容易產生錯誤。

## <a name="how-model-binding-works"></a>模型繫結的運作方式

MVC 收到 HTTP 要求時，它將其路由至特定的動作方法的控制器。 它會判斷要根據執行的路由資料中的動作方法，則它將值繫結從 HTTP 要求到該動作方法的參數。 例如，請考慮下列 URL:

`http://contoso.com/movies/edit/2`

因為路由範本看起來像這樣， `{controller=Home}/{action=Index}/{id?}`，`movies/edit/2`會路由傳送至`Movies`控制站，且其`Edit`動作方法。 它也可接受選擇性參數呼叫`id`。 動作方法的程式碼看起來應該像這樣：

```csharp
public IActionResult Edit(int? id)
   ```

注意： 不區分大小寫的字串中的 URL 路由。

MVC 會嘗試要求將資料繫結至動作參數名稱。 MVC 會尋找每個參數值使用的參數名稱和公用的可設定屬性的名稱。 在上述範例中，唯一的動作參數之所以名為`id`，後者則 MVC 繫結至具有相同名稱的路由值的值。 除了路由值 MVC 將資料從繫結要求的各個部分，而不按照順序。 以下是在模型繫結會透過它們的順序中的資料來源的清單：

1. `Form values`： 這些是在使用 POST 方法的 HTTP 要求的表單值。 （包括 jQuery POST 要求）。

2. `Route values`： 所提供的路由值的集合[路由](../../fundamentals/routing.md)

3. `Query strings`: URI 查詢字串組件。

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

注意： 表單值、 路由資料和查詢字串會儲存為名稱 / 值組。

由於模型繫結要求名為`id`任何名為`id`表單值，它進入尋找該索引鍵的路由值。 在本例中，相符。 繫結發生，並將值轉換為 2 的整數。 使用 編輯 (字串 id) 相同的要求會將轉換成"2"的字串。

到目前為止的範例會使用簡單類型。 在 MVC 簡單類型會是任何.NET 基本型別或字串型別轉換子的型別。 如果動作方法的參數是類別，例如`Movie`型別，其中包含簡單和複雜類型，如同屬性，MVC 的模型繫結仍會妥善處理。 它會使用反映和遞迴周遊尋找相符的複雜類型的屬性。 模型繫結會尋找模式*parameter_name.property_name*繫結至屬性的值。 如果找不到相符的值，這種形式的它會嘗試使用屬性名稱進行繫結。 針對這些類型，例如`Collection`型別，模型繫結會尋找相符項目*parameter_name [index]*或簡稱*[index]*。 模型繫結會視為`Dictionary`同樣地，類型要求*parameter_name [key]*或簡稱*[key]*，只要索引鍵是簡單類型。 支援的索引鍵符合欄位名稱 HTML 和標記協助程式產生相同的模型型別。 這可讓反覆存取值，讓表單欄位時，維持填滿與使用者的輸入，其方便起見，比方說，從建立或編輯繫結的資料未通過驗證。

順序，就可能發生的繫結的類別必須具有公用預設建構函式，並繫結的成員必須是公用的可寫入屬性。 當模型繫結發生時，此類別將只使用具現化公用預設建構函式時，可以設定的屬性。

參數繫結，模型繫結會停止尋找具有該名稱的值，它會移至下一個參數繫結。 否則，預設模型繫結行為會將參數設為預設值根據其類型而定：

* `T[]`： 使用的陣列型別的`byte[]`，繫結會設定參數的型別`T[]`至`Array.Empty<T>()`。 類型的陣列`byte[]`設為`null`。

* 參考型別： 繫結建立執行個體類別的預設建構函式未設定屬性。 不過，模型繫結設定`string`參數`null`。

* 可為 null 的類型： 可為 Null 的型別會設定為`null`。 在上述範例中，模型繫結設定`id`至`null`因為它是型別`int?`。

* 實值型別： 不可為 null 值類型的型別`T`設為`default(T)`。 例如，模型繫結會設定參數`int id`設為 0。 請考慮使用模型驗證或可為 null 的類型，而不要依賴預設值。

如果繫結失敗時，MVC 不會擲回錯誤。 可接受使用者輸入的每一個動作應該檢查`ModelState.IsValid`屬性。

注意： 每個項目中的控制站`ModelState`屬性是`ModelStateEntry`包含`Errors`屬性。 它是很少會需要自行查詢此集合。 請改用 `ModelState.IsValid` 。

此外，還有當 MVC 模型繫結時，必須考量某些特殊的資料類型：

* `IFormFile``IEnumerable<IFormFile>`： 一或多個上傳的檔案的 HTTP 要求的一部分。

* `CancellationToken`： 用來取消非同步控制器中的活動。

這些型別可以繫結至動作參數或屬性的類別類型。

模型繫結完成時，[驗證](validation.md)，就會發生。 預設模型繫結的運作方式很適合用於大部分的開發案例。 它也是可延伸因此如果您有唯一的需求，您可以自訂內建行為。

## <a name="customize-model-binding-behavior-with-attributes"></a>自訂屬性的模型繫結行為

MVC 包含數個屬性可讓您將導向至不同來源的預設模型繫結行為。 例如，您可以指定繫結是否需要屬性，則它應該永遠不會發生完全使用`[BindRequired]`或`[BindNever]`屬性。 或者，您可以覆寫預設的資料來源，並指定模型繫結器的資料來源。 以下是一份模型繫結屬性：

* `[BindRequired]`： 如果無法繫結這個屬性會將模型狀態錯誤。

* `[BindNever]`： 會告知模型繫結器將永遠不會繫結至這個參數。

* `[FromHeader]``[FromQuery]`， `[FromRoute]`， `[FromForm]`： 用來指定您想要套用的確切繫結來源。

* `[FromServices]`： 這個屬性會使用[相依性插入](../../fundamentals/dependency-injection.md)將從服務繫結參數。

* `[FromBody]`： 將資料繫結從要求主體中使用設定的格式器。 根據要求的內容類型選取格式器。

* `[ModelBinder]`： 用來覆寫預設模型繫結器、 繫結來源和名稱。

當您需要覆寫預設行為的模型繫結時，屬性是很有幫助的工具。

## <a name="binding-formatted-data-from-the-request-body"></a>從要求主體格式的繫結資料

要求的資料可以來自各種不同的格式，包括 JSON、 XML 及其他等等。 當您使用 [FromBody] 屬性來指出您想要將參數繫結至的要求主體中的資料時，MVC 會使用設定的格式器來處理要求資料，根據其內容的型別。 根據預設，MVC 包含`JsonInputFormatter`類別處理 JSON 資料，但是您可以新增其他的格式器來處理 XML 和其他自訂格式。

> [!NOTE]
> 有最多可達一個參數，每個動作以裝飾`[FromBody]`。 ASP.NET Core MVC 執行階段委派要求資料流讀取的格式器的責任。 一旦參數讀取要求資料流時，它通常不可能是讀取要求資料流，再為其他繫結`[FromBody]`參數。

> [!NOTE]
> `JsonInputFormatter`是預設的格式器，並且根據[Json.NET](https://www.newtonsoft.com/json)。

ASP.NET 選取輸入的格式器基礎[Content-type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)標頭和參數的型別除非套用，否則為指定的屬性。 如果您想要使用 XML 或另一種格式必須設定在*Startup.cs*檔案，但您可能需要取得的參考`Microsoft.AspNetCore.Mvc.Formatters.Xml`使用 NuGet。 您的啟始程式碼看起來應該像這樣：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

中的程式碼*Startup.cs*檔案包含`ConfigureServices`方法`services`引數可用來建置您的 ASP.NET 應用程式的服務。 在範例中，我們會將 XML 格式器加入為 MVC 會提供此應用程式的服務。 `options`引數傳遞至`AddMvc`方法可讓您新增及管理篩選器、 格式器，以及其他系統選項從 MVC 應用程式啟動時。 然後套用`Consumes`屬性到控制器類別或動作方法，才能使用您想要的格式。

### <a name="custom-model-binding"></a>自訂的模型繫結

您可以透過撰寫您自己自訂的模型繫結器擴充模型繫結。 深入了解[自訂模型繫結](../advanced/custom-model-binding.md)。
