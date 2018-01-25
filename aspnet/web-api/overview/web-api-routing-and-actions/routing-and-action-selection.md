---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: "路由和 ASP.NET Web API 中的動作選取 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>路由和 ASP.NET Web API 中的動作選取
====================
由[Mike Wasson](https://github.com/MikeWasson)

本文說明 ASP.NET Web API 來控制站上的特定動作所傳送的 HTTP 要求。

> [!NOTE]
> 路由的高階概觀，請參閱[中 ASP.NET Web API 的路由](routing-in-aspnet-web-api.md)。


這篇文章探討路由程序的詳細資料。 如果您建立 Web API 專案，並尋找，有些要求不會路由傳送您所預期的方式，希望這篇文章可協助。

路由有三個主要階段：

1. 比對路由範本的 URI。
2. 選取控制站。
3. 選取動作。

您可以將程序的某些部分取代您自己自訂的行為。 在本文中，我將說明的預設行為。 在結束時，我要注意的地方，您可以在其中自訂行為。

## <a name="route-templates"></a>路由範本

路由範本看起來類似的 URI 路徑，但它可以包含預留位置的值，以大括號表示：

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

當您建立路由時，您可以提供預設值進行部分或全部的預留位置：

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

您也可以提供條件約束，限制如何在 URI 片段可以比對的預留位置：

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

架構會嘗試比對中範本的 URI 路徑區段。 在範本中的常值必須完全符合。 預留位置符合任何值，除非您指定的條件約束。 架構不相符的 URI，例如主機名稱或查詢參數的其他部分。 架構會比對 URI 的路由表中選取第一個路由。

有兩個特殊的預留位置:"{controller}"和"{action}"。

- 「 {控制器} 」 提供的控制器名稱。
- "{action}"提供動作的名稱。 Web API 中的常見慣例是省略"{action}"。

### <a name="defaults"></a>預設值

如果您提供的預設值時，路由會比對缺少這些區段的 URI。 例如: 

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI"`http://localhost/api/products`"符合此路由。 {"Category}"區段會指派預設值 [全部]。

### <a name="route-dictionary"></a>路由字典

如果架構找到相符項目 uri，它會建立字典，其中包含每個預留位置的值。 索引鍵是替代名稱，不含大括號。 從 URI 路徑或預設值就會使用這些值。 字典會儲存在**IHttpRouteData**物件。

在此路由符合 」 階段中，特殊"{controller}"和"{action}"預留位置被就像其他版面配置。 它們只會儲存在字典中的其他值。

預設值可以有特殊值**RouteParameter.Optional**。 如果預留位置取得指派此值，此值不會加入至路由字典。 例如: 

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

URI 路徑 」 api/產品 」 和路由字典將會包含：

- 控制器:"products"
- 類別目錄: [全部]

「 應用程式開發介面/產品/toys/123 」，不過，路由字典會包含：

- 控制器:"products"
- 分類:"toys 」
- 識別碼:"123"

預設值也可以在路由範本中包含沒有任何位置出現的值。 如果路由符合，該值會儲存在字典中。 例如: 

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

如果 URI 路徑是"8/api/根"，字典會包含兩個值：

- 控制器: 「 客戶 」
- id: "8"

## <a name="selecting-a-controller"></a>選取控制站

控制器選取項目由**IHttpControllerSelector.SelectController**方法。 這個方法會採用**HttpRequestMessage**執行個體，並傳回**HttpControllerDescriptor**。 預設實作由提供**DefaultHttpControllerSelector**類別。 這個類別會使用簡單的演算法：

1. 查看路由字典的索引鍵 「 控制器 」。
2. 此索引鍵的值，並附加 「 控制器 」 以取得控制器的型別名稱的字串。
3. 尋找此型別名稱的 Web API 控制器。

比方說，如果路由字典中包含索引鍵-值組"controller"="products"，則控制器類型是"ProductsController"。 如果沒有相符的型別或多個相符項目，架構就會傳回錯誤給用戶端。

步驟 3， **DefaultHttpControllerSelector**使用**IHttpControllerTypeResolver**介面，以取得 Web API 控制器型別的清單。 預設實作**IHttpControllerTypeResolver** （a） 實作會傳回所有公用類別**IHttpController**，（b） 是不抽象的而且 （c） 具有以"Controller"的名稱。

## <a name="action-selection"></a>動作選取

選取控制站之後, 則架構會選取動作藉由呼叫**IHttpActionSelector.SelectAction**方法。 這個方法會採用**HttpControllerContext**並傳回**HttpActionDescriptor**。

預設實作由提供**ApiControllerActionSelector**類別。 若要選取的動作，它會查看下列：

- 要求的 HTTP 方法。
- 在路由範本中，如果有的話"{action}"預留位置。
- 在控制器上的動作參數。

之前查看選取演算法時，我們必須了解有關非同步控制器動作的一些事項。

**在控制器上的方法被視為 「 動作 」？** 當選取動作，架構只查看公用執行個體方法的控制站上。 此外，它會排除[「 特殊名稱 」](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) （建構函式、 事件、 運算子多載，以及其他等等） 的方法和方法繼承自**ApiController**類別。

**HTTP 方法。** 架構只會選擇符合的決定，如下所示，在要求之 HTTP 方法的動作：

1. 您可以使用屬性指定的 HTTP 方法： **AcceptVerbs**， **HttpDelete**， **HttpGet**， **HttpHead**， **HttpOptions**， **HttpPatch**， **HttpPost**，或**HttpPut**。
2. 否則，如果控制器方法的名稱會以"Get"、"Post"、"Put"、"Delete"、"Head"、 [選項] 或 「 修補日 」 開始，然後依照慣例動作支援的 HTTP 方法。
3. 如果以上皆非，該方法支援 POST。

**參數繫結。** 參數繫結是 Web 應用程式開發介面如何建立參數的值。 以下是參數繫結的預設規則：

- 簡單類型會從 URI 擷取。
- 複雜型別會從要求主體中取用。

簡單類型包括所有[.NET Framework 基本型別](https://msdn.microsoft.com/library/system.type.isprimitive)，加上**DateTime**，**十進位**， **Guid**，**字串**，和**TimeSpan**。 對於每個動作中，最多一個參數可以讀取的要求主體。

> [!NOTE]
> 很可能覆寫預設的繫結規則。 請參閱[WebAPI 參數繫結實際上](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)。


利用該背景，以下是動作選取演算法。

1. 符合 HTTP 要求方法的控制站上建立所有動作的清單。
2. 如果路由字典具有 「 動作 」 項目，將移除其名稱不符合此值的動作。
3. 嘗試比對 uri 的動作參數，如下所示： 

    1. 針對每個動作中，取得一份是簡單型別，繫結，從 URI 取得參數的參數。 排除的選擇性參數。
    2. 從這個清單中，嘗試尋找相符項目，每個參數名稱，路由字典或 URI 查詢字串。 相符項目都不區分大小寫，並不會依賴參數順序。
    3. 選取清單中的每個參數之相符項目之 URI 中的動作。
    4. 如果更該動作符合這些準則，挑選一個包含大部分的參數相符項目。
4. 略過動作與**[NonAction]**屬性。

步驟 3 是可能最令人混淆。 基本概念是從 URI、 從要求主體中，或從自訂繫結參數可取得其值。 來自 URI 的參數，我們想要確保的 URI 實際上包含在 （透過路由字典） 的路徑或查詢字串中該參數的值。

例如，請考慮下列動作：

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*識別碼*參數繫結至的 URI。 因此，此動作只會比對 URI，包含 「 識別碼 」，路由字典中，或查詢字串中的值。

選擇性參數是例外狀況，因為它們是選擇性。 選擇性參數，則是 [確定] 如果繫結無法從 URI 取得的值。

複雜型別是其他原因的例外狀況。 透過自訂繫結的複雜類型只能繫結至的 URI。 但在此情況下，架構無法事先知道參數會繫結至特定的 URI。 了解，它將需要叫用的繫結。 選取演算法的目標是靜態的說明，再叫用任何繫結從選取的動作。 因此，複雜型別會排除比對演算法。

選取動作之後，會叫用所有的參數繫結。

摘要: 

- Action 必須符合要求的 HTTP 方法。
- 動作名稱必須符合 「 動作 」 中的項目路由字典，如果有的話。
- 每個參數的動作，如果參數取自 URI，然後參數名稱必須位於 URI 查詢字串中或是中路由字典。 （選擇性參數，而且具有複雜類型的參數會排除。）
- 嘗試比對最多的參數。 最符合項目可能不含任何參數的方法。

## <a name="extended-example"></a>延伸的範例

路由：

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

控制器: 

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP 要求：

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>路由相符

URI 符合路由名為"DefaultApi"。 路由字典包含下列項目：

- 控制器:"products"
- id: "1"

路由字典不包含查詢字串參數、 「 版本 」 和 「 詳細資料 」，但這些仍然會被視為動作選取的期間。

### <a name="controller-selection"></a>控制器選取範圍

從路由字典中的 「 控制器 」 項目，為控制器類型`ProductsController`。

### <a name="action-selection"></a>動作選取

HTTP 要求是 GET 要求。 支援 GET 的控制器動作為`GetAll`， `GetById`，和`FindProductsByName`。 路由字典不包含 [動作]，項目，所以我們不需要比對的動作名稱。

接下來，我們嘗試比對的動作，參數名稱只能查看 GET 動作。

| 動作 | 要比對參數 |
| --- | --- |
| `GetAll` | 無 |
| `GetById` | 「 識別碼 」 |
| `FindProductsByName` | 「 名稱 」 |

請注意，*版本*參數`GetById`不是，因為它是選擇性的參數。

`GetAll`方法兩者相符。 `GetById`方法也會比對，因為路由字典包含 「 識別碼 」。 `FindProductsByName`方法不符合。

`GetById` Wins 方法，因為它會比對一個參數，也沒有參數與`GetAll`。 使用下列參數值叫用的方法：

- *id* = 1
- *版本*= 1.5

請注意，即使*版本*未使用在選取演算法參數的值來自 URI 查詢字串。

## <a name="extension-points"></a>擴充點

Web API 路由的處理程序的某些部分提供擴充點。

| 介面 | 描述 |
| --- | --- |
| **IHttpControllerSelector** | 選取的控制器。 |
| **IHttpControllerTypeResolver** | 取得控制器型別的清單。 **DefaultHttpControllerSelector**從這份清單中選擇 控制器類型。 |
| **IAssembliesResolver** | 取得專案的組件清單。 **IHttpControllerTypeResolver**介面會使用這份清單來尋找控制器類型。 |
| **IHttpControllerActivator** | 建立新的控制器執行個體。 |
| **IHttpActionSelector** | 選取的動作。 |
| **IHttpActionInvoker** | 叫用動作。 |

若要提供您自己的實作這些介面，使用**服務**集合**HttpConfiguration**物件：

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
