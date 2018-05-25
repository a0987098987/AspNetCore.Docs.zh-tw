---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API 中的繫結參數 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API 中的繫結的參數
====================
由[Mike Wasson](https://github.com/MikeWasson)

當 Web 應用程式開發介面的控制站上呼叫方法時，它必須設定參數的值，稱為程序*繫結*。 本文說明如何 Web API 繫結參數，以及如何自訂繫結程序。

根據預設，Web API 會使用下列規則來繫結參數：

- 如果參數是 「 簡單 」 類型，Web API 會嘗試從 URI 取得的值。 簡單類型包括.NET[基本型別](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**， **bool**， **double**，依此類推)，加上**TimeSpan**， **DateTime**， **Guid**，**十進位**，和**字串**，*加上*任何類型與類型轉換器可以從字串轉換。 （深入了解稍後型別轉換子。）
- 針對複雜型別，Web API 會嘗試從訊息本文讀取的值，使用[媒體類型格式器](media-formatters.md)。

例如，以下是典型的 Web API 控制器方法：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*識別碼*參數是&quot;簡單&quot;類型，因此 Web API 會嘗試取得要求 URI 中的值。 *項目*參數是複雜類型，因此 Web 應用程式開發介面使用的媒體類型格式器來讀取的要求主體中的值。

若要從 URI 取得值，Web API 會查詢的路由資料和 URI 查詢字串。 當路由系統剖析 URI 和比對路由，則會填入的路由資料。 如需詳細資訊，請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。

在本文的其餘部分，我將示範如何自訂模型繫結程序。 複雜型別，不過，請考慮使用盡可能的媒體類型格式器。 HTTP 主要原則就是在訊息主體中，使用內容交涉，以指定資源的表示法傳送的資源。 針對完全此用途所設計的媒體類型格式器。

## <a name="using-fromuri"></a>使用 [FromUri]

若要強制 Web API 來讀取從 URI 的複雜類型，新增 **[FromUri]** 屬性的參數。 下列範例會定義`GeoPoint`型別，以及控制站方法，以取得`GeoPoint`從 URI。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

用戶端可以將經度和緯度值放在查詢字串和 Web API 會使用它們來建構`GeoPoint`。 例如: 

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>使用 [FromBody]

若要強制 Web API 來讀取的要求主體中的簡單類型，新增 **[FromBody]** 屬性的參數：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

在此範例中，Web 應用程式開發介面將使用的媒體類型格式器來讀取的值*名稱*從要求主體。 以下是範例用戶端要求。

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

[FromBody] 參數時，Web API 所使用的內容類型標頭來選取格式器。 在此範例中，內容類型是&quot;應用程式 /json&quot;和要求主體是未經處理的 JSON 字串 （而不是 JSON 物件）。

最多一個參數可以讀取訊息本文。 因此這無法運作：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

此規則的原因是可能只讀取一次的非緩衝處理資料流中儲存的要求主體。

## <a name="type-converters"></a>類型轉換器

您可以讓 （以便 Web API 將會嘗試將它從 URI 繫結），將類別視為簡單類型的 Web API 建立**TypeConverter**並提供字串轉換。

下列程式碼會示範`GeoPoint`類別，表示地理位置的點，再加上**TypeConverter**將從字串轉換`GeoPoint`執行個體。 `GeoPoint`類別以裝飾 **[TypeConverter]** 屬性來指定型別轉換子。 (這個範例啟發的 Mike 拖延部落格文章[如何繫結至 MVC/WebAPI 中的動作簽章中的自訂物件](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)。)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

現在會將 Web API`GeoPoint`為簡單類型，這表示它將會嘗試繫結`GeoPoint`從 URI 的參數。 您不需要包含 **[FromUri]** 參數上。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

用戶端可以叫用的方法就像這樣的 uri:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>模型繫結器

型別轉換子比更有彈性的選項是建立自訂的模型繫結。 使用模型繫結，您必須存取 HTTP 要求、 動作描述，和原始值從路由資料。

若要建立的模型繫結器，實作**IModelBinder**介面。 這個介面會定義單一方法**BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

以下是模型繫結器`GeoPoint`物件。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

模型繫結取得未經處理的輸入的值，從*值提供者*。 這項設計會分隔兩個不同的函式：

- 值提供者會收到 HTTP 要求，並於其中填入索引鍵-值組的字典。
- 模型繫結器會使用此字典來擴展模型。

Web API 中的預設值提供者從路由資料和查詢字串取得值。 例如，如果 URI 是`http://localhost/api/values/1?location=48,-122`，值提供者會建立下列機碼值組：

- id = &quot;1&quot;
- 位置 = &quot;48,122&quot;

(假設是預設路由範本&quot;api / {controller} / {id}&quot;。)

若要繫結參數的名稱儲存在**ModelBindingContext.ModelName**屬性。 模型繫結器會尋找此字典中值的索引鍵。 如果值存在，而且可以轉換成`GeoPoint`，模型繫結器繫結將值指派至**ModelBindingContext.Model**屬性。

請注意，模型繫結器不限制為簡單的型別轉換。 在此範例中，模型繫結器會先尋找已知的位置，資料表中，如果失敗，它會使用型別轉換。

**設定模型繫結器**

有幾種方式來設定模型繫結。 首先，您可以加入 **[ModelBinder]** 屬性的參數。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

您也可以加入 **[ModelBinder]** 屬性加入型別。 Web API 將會使用該類型的所有參數指定的模型繫結器。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

最後，您可以在其中加入模型繫結器提供者**HttpConfiguration**。 模型繫結器提供者是只建立的模型繫結器 factory 類別。 您可以建立提供者藉由衍生自[ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)類別。 不過，如果您的模型繫結器可以處理單一類型，它很容易使用內建**SimpleModelBinderProvider**，它設計為此目的。 下列程式碼示範如何執行這項操作。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

與模型繫結的提供者，您仍需要加入 **[ModelBinder]** 屬性的參數，告知它應該使用模型繫結和不媒體類型格式器的 Web API。 但是，現在您不需要在屬性中指定的模型繫結器類型：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>值提供者

模型繫結取得值，來自值提供者所述。 若要撰寫的自訂值提供者，實作**IValueProvider**介面。 提取要求中的 cookie 中的值範例如下：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

您也必須由衍生自建立值提供者 factory **ValueProviderFactory**類別。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

加入至值提供者 factory **HttpConfiguration** ，如下所示。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web 應用程式開發介面撰寫所有值提供者，因此當模型繫結呼叫**ValueProvider.GetValue**，模型繫結器會從第一個能夠產生它的值提供者收到的值。

或者，使用參數層級設定值提供者 factory **ValueProvider**屬性，如下所示：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

這會告知模型繫結使用指定的值提供者處理站，且不使用任何其他已註冊的值提供者的 Web API。

## <a name="httpparameterbinding"></a>HttpParameterBinding

模型繫結器的較通用的機制的特定執行個體。 如果您看一下 **[ModelBinder]** 屬性，您會看到它衍生自抽象**ParameterBindingAttribute**類別。 這個類別會定義單一方法**GetBinding**，它會傳回**HttpParameterBinding**物件：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding**會負責將參數繫結的值。 如果是 **[ModelBinder]**，屬性會傳回**HttpParameterBinding**使用實作**IModelBinder**執行實際的繫結。 您也可以實作您自己**HttpParameterBinding**。

例如，假設您想要取得從 Etag`if-match`和`if-none-match`要求中的標頭。 我們會先定義的類別來表示的 Etag。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

我們也會定義列舉，指出要取得所產生的 ETag`if-match`標頭或`if-none-match`標頭。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

以下是**HttpParameterBinding** ，取得所需的標頭的 ETag，並將它繫結至類型 ETag 的參數：

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync**方法中會繫結。 在此方法中，加入繫結的參數值**ActionArgument**字典中的**HttpActionContext**。

> [!NOTE]
> 如果您**ExecuteBindingAsync**方法會讀取要求訊息的本文，請覆寫**WillReadBody**屬性傳回 true。 要求主體可能只能讀取一次，讓 Web 應用程式開發介面會強制執行規則的最多只有一個繫結的緩衝資料流可以讀取訊息本文。


若要套用自訂**HttpParameterBinding**，您可以定義衍生自屬性**ParameterBindingAttribute**。 如`ETagParameterBinding`，我們會定義兩個屬性，一個用於`if-match`標頭，另一個用於`if-none-match`標頭。 兩者都衍生自抽象基底類別。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

以下是使用控制器方法`[IfNoneMatch]`屬性。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

除了**ParameterBindingAttribute**，還有另一個勾點加入自訂**HttpParameterBinding**。 在**HttpConfiguration**物件**ParameterBindingRules**屬性為 anomymous 函式型別的集合 (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。 例如，您可以加入任何 ETag 上的參數 GET 方法使用的規則`ETagParameterBinding`與`if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

函式應傳回`null`參數的繫結不適用。

## <a name="iactionvaluebinder"></a>IActionValueBinder

整個參數繫結程序由隨插即用服務， **IActionValueBinder**。 預設實作**IActionValueBinder**會進行下列作業：

1. 尋找**ParameterBindingAttribute**參數上。 這包括 **[FromBody]**， **[FromUri]**，和 **[ModelBinder]**，或自訂屬性。
2. 否則，請查看**HttpConfiguration.ParameterBindingRules**函式會傳回非 null **HttpParameterBinding**。
3. 否則，請使用先前所述的預設規則。 

    - 如果參數類型為 「 簡單 」，或是從 URI 繫結的型別轉換子。 這相當於將 **[FromUri]** 參數上的屬性。
    - 否則，嘗試從訊息本文讀取參數。 這相當於將 **[FromBody]** 參數上。

如果您想，您可以取代整個**IActionValueBinder**服務的自訂實作。

## <a name="additional-resources"></a>其他資源

[自訂參數繫結範例](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike 拖延提到 Web 應用程式開發介面參數繫結的良好的一連串的部落格文章：

- [Web 應用程式開發介面如何執行參數繫結](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API 的 MVC 樣式參數繫結](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [如何繫結至 MVC/Web API 中的動作簽章中的自訂物件](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [如何在 Web 應用程式開發介面中建立的自訂值提供者](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [背後原理的 web 應用程式開發介面參數繫結](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
