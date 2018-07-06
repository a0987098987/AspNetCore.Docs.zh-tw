---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: 路由和 ASP.NET Web API 中的動作選項 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 2421d3dd134e9188faf6933b076fb9d22a119617
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832826"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>路由和 ASP.NET Web API 中的動作選取
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

這篇文章說明 ASP.NET Web API 將 HTTP 要求路由至特定動作的控制站上的方式。

> [!NOTE]
> 路由的高階概觀，請參閱 < [ASP.NET Web API 中的路由](routing-in-aspnet-web-api.md)。


這篇文章探討路由的處理程序的詳細資料。 如果您建立 Web API 專案時，發現其中某些要求不會路由傳送您預期的方式，希望這篇文章會幫助。

路由有三個主要階段：

1. 比對路由範本的 URI。
2. 選取控制站。
3. 選取一個動作。

您可以將程序的某些部分取代您自己的自訂行為。 在本文中，我會說明的預設行為。 在結束時，我會說明您可以在此自訂行為的地方。

## <a name="route-templates"></a>路由範本

路由範本看起來類似的 URI 路徑，但它可以有預留位置值，以大括號：

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

當您建立路由時，您可以針對部分或全部的預留位置來提供預設值：

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

您也可以提供條件約束，限制如何 URI 區段可以比對的預留位置：

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

此架構會嘗試比對中範本的 URI 路徑的區段。 在範本中的常值必須完全相符。 預留位置會符合任何值，除非您指定的條件約束。 此架構不符合的 URI，例如主機名稱或查詢參數的其他部分。 Framework 會選取符合 URI 的路由表中的第一個路由。

有兩個特殊的預留位置:"{controller}"和"{action}"。

- "{controller}"提供的控制器名稱。
- "{action}"提供動作的名稱。 在 Web API 中，一般的慣例是省略"{action}"。

### <a name="defaults"></a>預設值

如果您提供預設值時，路由會符合缺少這些區段的 URI。 例如: 

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI"`http://localhost/api/products`"符合此路由。 "{Category}"區段會指派預設值 [全部]。

### <a name="route-dictionary"></a>路由字典

如果架構找到相符項目 uri，它會建立字典，其中包含每個預留位置的值。 索引鍵是預留位置名稱，不包含大括號。 值取自 URI 的路徑或預設值。 字典會儲存在**IHttpRouteData**物件。

在此路由比對 」 階段中，特殊"{controller}"和"{action}"預留位置的處理就像其他版面配置。 它們只會儲存在其他值的字典。

預設值可以有特殊值**RouteParameter.Optional**。 預留位置會指派此值，如果值不會加入至路由字典。 例如: 

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

URI 路徑 「 api/產品 」 中，將會包含路由字典：

- 控制站: 「 產品 」
- 類別目錄: [全部]

"Api/產品/toys/123"，不過，路由字典會包含：

- 控制站: 「 產品 」
- 類別: 「 玩具 」
- 識別碼:"123"

預設值也可以在路由範本中包含值，不會不出現在任何地方。 如果路由符合，該值會儲存在字典中。 例如: 

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

如果 URI 的路徑是"api/root/8"，字典會包含兩個值：

- 控制站: 「 客戶 」
- 識別碼:"8"

## <a name="selecting-a-controller"></a>選取控制站

控制器選取項目由**IHttpControllerSelector.SelectController**方法。 這個方法會採用**HttpRequestMessage**執行個體，並傳回**HttpControllerDescriptor**。 預設實作由提供**DefaultHttpControllerSelector**類別。 這個類別會使用簡單的演算法：

1. 查看路由字典的索引鍵"controller"。
2. 此索引鍵的值，並附加 「 控制器 」 以取得控制器的型別名稱的字串。
3. 使用此型別名稱的 Web API 控制器的外觀。

比方說，如果路由字典包含索引鍵 / 值組"controller"= 「 產品 」，則控制器類型是"ProductsController 」。 如果沒有任何相符的類型或多個相符項目，架構就會傳回錯誤給用戶端。

步驟 3 中，如**DefaultHttpControllerSelector**會使用**IHttpControllerTypeResolver**介面，以取得 Web API 控制器型別的清單。 預設實作**IHttpControllerTypeResolver** （a） 實作會傳回所有公用類別**IHttpController**，（b） 都不抽象，且 （c） 中"Controller"結尾的名稱。

## <a name="action-selection"></a>動作選取

選取控制站之後, 此架構，請選取藉由呼叫的動作**IHttpActionSelector.SelectAction**方法。 這個方法會採用**HttpControllerContext** ，然後傳回**HttpActionDescriptor**。

預設實作由提供**ApiControllerActionSelector**類別。 若要選取的動作，它會查看下列：

- 要求的 HTTP 方法。
- 在路由範本中，如果有的話"{action}"預留位置。
- 在控制器上的動作參數。

之前查看選取演算法，我們必須了解有關控制器動作的一些事項。

**在控制器上的方法會視為 「 動作 」？** 當選取一個動作，架構只查看公用執行個體方法的控制站上。 此外，它會排除[「 特殊名稱 」](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) （建構函式、 事件、 運算子多載，等等） 的方法和方法繼承自**ApiController**類別。

**HTTP 方法。** 架構只會選擇符合要求的 HTTP 方法，取決於下列動作：

1. 您可以使用屬性中指定的 HTTP 方法： **AcceptVerbs**， **HttpDelete**， **HttpGet**， **HttpHead**， **HttpOptions**， **HttpPatch**， **HttpPost**，或**HttpPut**。
2. 否則，如果以"Get"、"Post"、"Put"、"Delete"、"Head"、 [選項] 或 「 修補 」 開頭的控制器方法的名稱，然後依照慣例動作支援該 HTTP 方法。
3. 如果以上皆非，方法支援 POST。

**參數繫結。** 參數繫結是 Web API 建立參數的值的方式。 以下是參數繫結的預設規則：

- 簡單型別取自 URI。
- 複雜型別會從要求主體。

簡單的型別包含的所有[.NET Framework 基本型別](https://msdn.microsoft.com/library/system.type.isprimitive)，再加上**DateTime**，**十進位**， **Guid**，**字串**，並**TimeSpan**。 針對每個動作中，最多一個參數可以讀取的要求主體。

> [!NOTE]
> 可以覆寫預設繫結規則。 請參閱[WebAPI 參數繫結，在幕後](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)。


有了此背景，以下是動作選取演算法。

1. 符合 HTTP 要求方法的控制站上建立之所有動作的清單。
2. 如果路由字典中有 「 動作 」 項目，將移除其名稱不符合此值的動作。
3. 嘗試比對 URI、 動作參數，如下所示： 

    1. 針對每個動作中，取得一份是簡單型別，其中會將繫結參數來自 URI 的參數。 排除的選擇性參數。
    2. 從這份清單中，嘗試尋找名稱相符的每個參數，在路由字典或 URI 查詢字串中。 相符項目不區分大小寫和參數的順序無關。
    3. 選取清單中的每個參數具有相符項目之 URI 中的其中一個動作。
    4. 如果更該動作符合這些準則，挑選其中的大部分參數相符項目。
4. 忽略動作 **[NonAction]** 屬性。

步驟 #3 是可能最容易造成混淆。 基本概念是來自 URI，從要求主體中，或是從自訂繫結參數可取得其值。 對於來自 URI 的參數，我們想要確定該 URI 實際上包含該參數，在 （透過路由字典） 的路徑或查詢字串中的值。

例如，請考慮下列動作：

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*識別碼*參數繫結至的 URI。 因此，此動作可以只比對 URI，其中包含 「 識別碼 」，在 路由字典或查詢字串中的值。

選擇性參數會是例外狀況，因為它們是選擇性。 選擇性參數，就可以繫結無法從 URI 取得的值。

複雜型別會因其他原因的例外狀況。 透過自訂繫結的複雜型別只能繫結至的 URI。 但在此情況下，架構無法事先知道參數會繫結至特定的 URI。 若要了解，它必須叫用繫結。 選擇演算法的目標是靜態的描述，然後再叫用任何繫結從選取的動作。 因此，複雜型別會排除比對演算法。

選取動作之後，會叫用所有的參數繫結。

摘要: 

- Action 必須符合要求的 HTTP 方法。
- 動作名稱必須符合 「 動作 」 中的項目路由字典，如果有的話。
- 每個參數的動作，如果參數取自 URI，然後參數名稱必須找到路由字典或 URI 查詢字串中。 （選擇性參數，而且具有複雜類型的參數會排除。）
- 嘗試比對參數最大數目。 最符合項目可能不含任何參數的方法。

## <a name="extended-example"></a>延伸的範例

路由：

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

控制器: 

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP 要求：

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>路由相符

URI 比對名為"DefaultApi"的路由。 路由字典包含下列項目：

- 控制站: 「 產品 」
- 識別碼:"1"

路由字典不包含查詢字串參數，"version"和"details"，但這些仍然會被視為動作選取的期間。

### <a name="controller-selection"></a>控制器選取項目

從路由字典中的 「 控制器 」 項目，是控制器類型`ProductsController`。

### <a name="action-selection"></a>動作選取

HTTP 要求是 GET 要求。 支援 GET 的控制器動作`GetAll`， `GetById`，和`FindProductsByName`。 路由字典不包含 「 動作 」，項目，因此我們不需要的動作名稱相符。

接下來，我們嘗試比對的動作，參數名稱顯見光看 GET 動作。

| 動作 | 要比對參數 |
| --- | --- |
| `GetAll` | 無 |
| `GetById` | 「 識別碼 」 |
| `FindProductsByName` | 「 名稱 」 |

請注意，*版本*參數`GetById`不是，因為它是選擇性的參數。

`GetAll`方法符合透過極簡方式。 `GetById`方法也會比對，因為路由字典包含 「 識別碼 」。 `FindProductsByName`方法不符。

`GetById` Wins 方法，因為它會比對一個參數，也沒有參數與`GetAll`。 叫用方法時，使用下列參數值：

- *id* = 1
- *版本*= 1.5

請注意，即使*版本*未使用在選取演算法參數的值來自 URI 查詢字串。

## <a name="extension-points"></a>擴充點

Web API 路由的處理程序的某些部分來提供擴充點。

| 介面 | 描述 |
| --- | --- |
| **IHttpControllerSelector** | 選取的控制器。 |
| **IHttpControllerTypeResolver** | 取得控制器型別的清單。 **DefaultHttpControllerSelector**從這份清單中選擇 控制器類型。 |
| **IAssembliesResolver** | 取得專案的組件清單。 **IHttpControllerTypeResolver**介面使用這份清單來尋找控制器類型。 |
| **IHttpControllerActivator** | 建立新的控制器執行個體。 |
| **IHttpActionSelector** | 選取的動作。 |
| **IHttpActionInvoker** | 叫用動作。 |

若要提供您自己的實作這些介面，使用**Services**收集**HttpConfiguration**物件：

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
