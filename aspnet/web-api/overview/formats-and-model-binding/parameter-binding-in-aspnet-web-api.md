---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API 中繫結的參數 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377805"
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API 中繫結的參數
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

當 Web API 控制器上呼叫方法時，它必須設定參數的值，這個程序稱為*繫結*。 這篇文章描述 Web API 如何繫結參數，以及如何自訂繫結程序。

根據預設，Web API 會使用下列規則來將參數繫結：

- 如果參數是 「 簡單 」 類型，Web API 會嘗試從 URI 取得的值。 簡單類型包括.NET[基本型別](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**， **bool**， **double**，依此類推)，再加上**TimeSpan**， **DateTime**， **Guid**， **decimal**，並**字串**，*加上*任何類型從字串可以轉換型別轉換子。 （深入了解稍後型別轉換子。）
- 複雜型別，Web API 嘗試讀取訊息主體中的值時，使用[media-type 格式器](media-formatters.md)。

例如，以下是典型的 Web API 控制器方法：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*識別碼*參數是&quot;簡單&quot;型別，因此 Web API 會嘗試取得要求 URI 中的值。 *項目*參數是複雜類型，因此 Web API 使用的媒體類型格式器來讀取的要求主體中的值。

若要從 URI 取得值，Web API 看起來的路由資料和 URI 查詢字串中。 當路由系統會剖析的 URI，並進行比對路由，則會填入的路由資料。 如需詳細資訊，請參閱 <<c0> [ 路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。

在這篇文章的其餘部分，我將示範如何自訂模型繫結程序。 複雜型別，不過，請考慮使用盡可能的媒體類型格式器。 HTTP 的主要原則是資源會在訊息主體中，使用內容交涉，以指定資源的表示法。 針對完全此用途所設計的媒體類型格式器。

## <a name="using-fromuri"></a>使用 [FromUri]

若要強制 Web API，可讀取的 URI 中的複雜型別，請新增 **[FromUri]** 屬性至參數。 下列範例會定義`GeoPoint`型別，以及取得控制器方法`GeoPoint`從 URI。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

用戶端可以將緯度和經度值放在查詢字串和 Web API 會使用這些來建構`GeoPoint`。 例如: 

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>使用 [FromBody]

若要強制 Web API，可讀取的要求主體中的簡單類型，新增 **[FromBody]** 屬性至參數：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

在此範例中，Web API 會讀取的值使用的媒體類型格式器*名稱*從要求主體。 以下是範例用戶端要求。

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

參數會有 [FromBody]，Web API 會使用 Content-type 標頭來選取格式器。 在此範例中，內容類型是&quot;application/json&quot;和要求主體是原始的 JSON 字串 （而不是 JSON 物件）。

最多一個參數可以讀取訊息主體。 因此，這不適用：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

此規則的原因是，可能只讀取一次的非緩衝處理資料流中儲存的要求主體。

## <a name="type-converters"></a>類型轉換器

您可以讓類別視為簡單型別 （以便讓 Web API 會嘗試將它繫結從 URI） 的 Web API 建立**TypeConverter**並提供字串轉換。

下列程式碼示範`GeoPoint`類別，表示地理的點，再加上**TypeConverter**可將字串的轉換`GeoPoint`執行個體。 `GeoPoint`類別以裝飾 **[TypeConverter]** 屬性來指定型別轉換子。 (此範例由 Mike Stall 的部落格文章啟發[如何繫結至 MVC/WebAPI 中的動作簽章中的自訂物件](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)。)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

現在會將 Web API`GeoPoint`為簡單型別，亦即它會嘗試繫結`GeoPoint`來自 URI 的參數。 您不需要包含 **[FromUri]** 參數上。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

用戶端可以叫用的方法，就像這樣的 uri:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>模型繫結器

型別轉換子比更有彈性的選項是建立自訂模型繫結。 使用模型繫結，您必須存取 HTTP 要求、 動作描述，以及原始值等項目從路由資料。

若要建立的模型繫結器，實作**IModelBinder**介面。 這個介面會定義單一方法**BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

以下是模型繫結器`GeoPoint`物件。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

模型繫結取得未經處理輸入的值從*值提供者*。 這項設計會分隔兩個不同的功能：

- 值提供者會接受 HTTP 要求，並於其中填入索引鍵 / 值組的字典。
- 模型繫結會使用此字典，來擴展模型。

在 Web API 中的預設值提供者會取得值，從路由資料和查詢字串。 比方說，如果 URI 為`http://localhost/api/values/1?location=48,-122`，值提供者會建立下列機碼值組：

- id = &quot;1&quot;
- 位置 = &quot;48,122&quot;

(我假設是預設的路由範本，也就是&quot;api / {controller} / {id}&quot;。)

要繫結參數的名稱會儲存在**ModelBindingContext.ModelName**屬性。 模型繫結會尋找與這個字典中值的索引鍵。 如果該值存在，而且可以轉換成`GeoPoint`，模型繫結器繫結將值指派給**ModelBindingContext.Model**屬性。

請注意，模型繫結並不會限制為簡單的型別轉換。 在此範例中，模型繫結器會先尋找已知的位置，資料表中，如果失敗，它會使用型別轉換。

**設定模型繫結**

有數種方式可設定的模型繫結器。 首先，您可以在其中加入 **[ModelBinder]** 屬性至參數。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

您也可以加入 **[ModelBinder]** 屬性型別。 Web API 會使用指定的模型繫結，該類型的所有參數。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

最後，您可以在其中新增模型繫結器提供者**HttpConfiguration**。 模型繫結器提供者是只建立的模型繫結器的 factory 類別。 您可以建立提供者透過衍生自[ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)類別。 不過，如果您的模型繫結器會處理單一的類型，它是使用內建的工作變得更容易**SimpleModelBinderProvider**，這針對此用途而設計。 下列程式碼示範如何執行這項操作。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

使用模型繫結提供者，您仍然需要新增 **[ModelBinder]** 屬性至參數，來告訴 Web API，它應該使用模型繫結和未 media-type 格式器。 但現在，您不必在屬性中指定的模型繫結器類型：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>值提供者

我說過的模型繫結器取得值，來自值提供者。 若要撰寫的自訂值提供者，實作**IValueProvider**介面。 提取要求中的 cookie 中的值範例如下：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

您也需要建立值提供者 factory 藉由衍生自**ValueProviderFactory**類別。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

新增至值提供者處理站**HttpConfiguration** ，如下所示。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API 撰寫所有值提供者，因此當模型繫結呼叫**ValueProvider.GetValue**，模型繫結會從第一個能夠產生它的值提供者收到的值。

或者，使用參數層級設定值提供者 factory **ValueProvider**屬性，如下所示：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

這會告知模型繫結使用指定的值提供者處理站，且不使用任何其他已註冊的值提供者的 Web API。

## <a name="httpparameterbinding"></a>HttpParameterBinding

模型繫結器的較通用的機制的特定執行個體。 如果您看一下 **[ModelBinder]** 屬性，您會看到它會衍生自抽象**ParameterBindingAttribute**類別。 這個類別會定義單一方法**GetBinding**，以傳回**HttpParameterBinding**物件：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding**會負責將參數繫結的值。 若是 **[ModelBinder]**，屬性會傳回**HttpParameterBinding**實作會使用**IModelBinder**執行實際的繫結。 您也可以實作您自己**HttpParameterBinding**。

例如，假設您想要取得從 Etag`if-match`和`if-none-match`要求中的標頭。 我們一開始先定義一個類別來表示 Etag。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

我們也會定義列舉，指出要取得所產生的 ETag`if-match`標頭或`if-none-match`標頭。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

以下是**HttpParameterBinding** ，取得所需的標頭的 ETag，並將它繫結至類型參數的 ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync**方法會繫結。 在此方法中，加入繫結的參數值**ActionArgument**中的字典**HttpActionContext**。

> [!NOTE]
> 如果您**ExecuteBindingAsync**方法會讀取要求訊息的本文中，覆寫**WillReadBody**屬性傳回 true。 要求主體可能只能讀取一次，讓 Web API 會強制執行規則，最多一個繫結的緩衝資料流可讀取訊息主體。


若要套用自訂**HttpParameterBinding**，您可以定義屬性衍生自**ParameterBindingAttribute**。 針對`ETagParameterBinding`，我們會定義兩個屬性，一個用於`if-match`標頭，另一個用於`if-none-match`標頭。 都是衍生自抽象基底類別。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

以下是使用控制器方法`[IfNoneMatch]`屬性。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

除了**ParameterBindingAttribute**，沒有新增自訂的另一個攔截**HttpParameterBinding**。 在  **HttpConfiguration**物件， **ParameterBindingRules**屬性是集合型別的 anomymous 函式 (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。 比方說，您可以在其中新增任何的 GET 方法上的 ETag 參數使用的規則`ETagParameterBinding`與`if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

函式應傳回`null`繫結不適用的參數。

## <a name="iactionvaluebinder"></a>IActionValueBinder

整個參數繫結程序由隨插即用的服務，控制**IActionValueBinder**。 預設實作**IActionValueBinder**會進行下列作業：

1. 尋求**ParameterBindingAttribute**參數上。 這包括 **[FromBody]**， **[FromUri]**，並 **[ModelBinder]**，或自訂屬性。
2. 否則，請查看**HttpConfiguration.ParameterBindingRules**函式會傳回非 null **HttpParameterBinding**。
3. 否則，請使用先前所述的預設規則。 

    - 如果參數類型為 「 簡單 」，或具有型別轉換子，從 URI 繫結。 這相當於將放 **[FromUri]** 參數上的屬性。
    - 否則，嘗試讀取訊息主體中的參數。 這相當於將放 **[FromBody]** 參數上。

如果您想，您可以取代整個**IActionValueBinder**服務的自訂實作。

## <a name="additional-resources"></a>其他資源

[自訂參數繫結範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall 撰寫有關 Web API 的參數繫結的良好的一連串的部落格文章：

- [Web API 是怎麼參數繫結](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API 的 MVC 樣式參數繫結](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [如何繫結至 MVC/Web API 中的動作簽章中的自訂物件](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [如何在 Web API 中建立的自訂值提供者](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [背後原理的 web API 參數繫結](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
