---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: ASP.NET Web API 2 中的路由慣例 Odata |Microsoft Docs
author: MikeWasson
description: 這篇文章描述 Web API OData 端點所使用的路由慣例。
ms.author: aspnetcontent
ms.date: 07/31/2013
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 645e820d3b03d1e3d2ac088973f6296efa5c2f40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818261"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>ASP.NET Web API 2 中的路由慣例 Odata
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 這篇文章描述 Web API OData 端點所使用的路由慣例。


當 Web API 取得 OData 要求時，它會將要求對應至控制器和動作名稱中。 對應根據 HTTP 方法和 URI。 例如，`GET /odata/Products(1)`對應至`ProductsController.GetProduct`。

在這篇文章的第 1 部分，我說明的內建的 OData 路由慣例。 這些慣例專為 OData 端點，而且取代預設的 Web API 路由系統。 (您呼叫時會更換**MapODataRoute**。)

在第 2 部分，我會示範如何新增自訂路由慣例。 目前的內建慣例不會涵蓋整個範圍的 OData Uri，但您可以延伸它們來處理額外情況。

- [內建的路由慣例](#conventions)
- [自訂路由慣例](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>內建的路由慣例

我將說明在 Web API 中的 OData 路由慣例之前，最好先了解 OData Uri。 [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)所組成：

- 服務根目錄
- 資源路徑
- 查詢選項

![](odata-routing-conventions/_static/image1.png)

路由，重要的部分是資源路徑。 資源路徑會分成區段。 比方說，`/Products(1)/Supplier`有三個區段：

- `Products` 指的是名為 「 產品 」 實體集。
- `1` 是實體索引鍵，從集合中選取單一實體。
- `Supplier` 已選取 相關的實體的導覽屬性。

因此這個路徑會挑選出產品 1 的供應商。

> [!NOTE]
> OData 路徑區段未一律對應到 URI 區段。 例如，"1"會被視為路徑區段。


**控制器名稱。** 控制器名稱永遠被衍生自實體集資源路徑的根目錄中。 例如，如果資源路徑是`/Products(1)/Supplier`，Web API 會尋找名為控制器`ProductsController`。

**動作名稱。** 動作名稱被衍生自的路徑區段，再加上 entity data model (EDM) 中，下列表格所列的。 在某些情況下，有兩個選項的動作名稱。 例如，"Get"或&quot;GetProducts&quot;。

**查詢實體**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| 取得 /entityset | / 產品 | GetEntitySet 或 Get | GetProducts |
| 取得 /entityset(key) | /Products(1) | GetEntityType 或 Get | GetProduct |
| 取得 /entityset （索引鍵）/轉換 | /Products(1)/Models.Book | GetEntityType 或 Get | GetBook |

如需詳細資訊，請參閱 <<c0> [ 建立唯讀的 OData 端點](odata-v3/creating-an-odata-endpoint.md)。

**建立、 更新和刪除實體**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| 張貼 /entityset | / 產品 | PostEntityType 或 Post | PostProduct |
| PUT /entityset(key) | /Products(1) | PutEntityType 或 Put | PutProduct |
| PUT /entityset （索引鍵）/轉換 | /Products(1)/Models.Book | PutEntityType 或 Put | PutBook |
| 修補程式 /entityset(key) | /Products(1) | PatchEntityType 或修補程式 | PatchProduct |
| 修補 /entityset （索引鍵）/轉換 | /Products(1)/Models.Book | PatchEntityType 或修補程式 | PatchBook |
| 刪除 /entityset(key) | /Products(1) | DeleteEntityType 或 Delete | DeleteProduct |
| 轉換/刪除 /entityset （索引鍵） | /Products(1)/Models.Book | DeleteEntityType 或 Delete | DeleteBook |

**查詢的導覽屬性**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| 取得 /entityset （索引鍵） / 瀏覽 | / 產品 （1）/供應商 | GetNavigationFromEntityType 或 GetNavigation | GetSupplierFromProduct |
| 取得 /entityset （索引鍵）/轉換/瀏覽 | /Products(1)/Models.Book/Author | GetNavigationFromEntityType 或 GetNavigation | GetAuthorFromBook |

如需詳細資訊，請參閱 <<c0> [ 使用實體關係](odata-v3/working-with-entity-relations.md)。

**建立和刪除連結**

| 要求 | 範例 URI | 動作名稱 |
| --- | --- | --- |
| POST /entityset （索引鍵） / $links/瀏覽 | / 產品 （1） / $ 連結/供應商 | CreateLink |
| PUT 的 /entityset （索引鍵） / $links/瀏覽 | / 產品 （1） / $ 連結/供應商 | CreateLink |
| 刪除 /entityset （索引鍵） / $links/瀏覽 | / 產品 （1） / $ 連結/供應商 | DeleteLink |
| 刪除 /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

如需詳細資訊，請參閱 <<c0> [ 使用實體關係](odata-v3/working-with-entity-relations.md)。

**屬性**

*需要 Web API 2*

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| 取得 /entityset （索引鍵） / 屬性 | /Products(1)/Name | GetPropertyFromEntityType 或 GetProperty | GetNameFromProduct |
| 取得 /entityset （索引鍵）/轉換/屬性 | /Products(1)/Models.Book/Author | GetPropertyFromEntityType 或 GetProperty | GetTitleFromBook |

**動作**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| POST /entityset （索引鍵） / 動作 | / 產品 （1）/速率 | ActionNameOnEntityType 或 ActionName | RateOnProduct |
| 張貼 /entityset （索引鍵）/轉換/動作 | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType 或 ActionName | CheckOutOnBook |

如需詳細資訊，請參閱 < [OData 動作](odata-v3/odata-actions.md)。

**方法簽章**

以下是一些方法簽章的規則：

- 如果路徑包含索引鍵，動作應該有一個名為參數*金鑰*。
- 如果路徑包含索引鍵轉換為導覽屬性，動作應該有一個名為參數*relatedKey*。
- 裝飾*金鑰*並*relatedKey*參數搭配 **[FromODataUri]** 參數。
- POST 和 PUT 要求需要一個實體型別的參數。
- PATCH 要求採用參數的型別**差異&lt;T&gt;**，其中*T*是實體類型。

如需參考，以下是範例，並顯示每個內建的 OData 路由慣例的方法簽章。

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>自訂路由慣例

目前的內建慣例並未涵蓋所有可能的 OData Uri。 您可以藉由實作加入新的慣例**IODataRoutingConvention**介面。 此介面有兩種方法：

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController**傳回控制器的名稱。
- **SelectAction**傳回動作的名稱。

如需這兩個方法中，如果慣例不適用於該要求，方法應該傳回 null。

**ODataPath**參數代表的已剖析的 OData 資源路徑。 它包含一份**[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** 執行個體，其中每個區段的資源路徑。 **ODataPathSegment**是抽象類別，每個區段的類型由衍生自類別**ODataPathSegment**。

**ODataPath.TemplatePath**屬性是字串，表示串連所有路徑區段。 例如，URI 是否`/Products(1)/Supplier`，路徑範本&quot;~/entityset/key/navigation&quot;。 請注意，這些區段不會直接對應至 URI 區段。 例如，將實體索引鍵 (1) 表示為其自身**ODataPathSegment**。

一般而言，實作**IODataRoutingConvention**會進行下列作業：

1. 比較以查看此慣例套用至目前要求的路徑範本。 如果它不適用，會傳回 null。
2. 慣例會套用，如果使用的屬性**ODataPathSegment**衍生控制器和動作名稱的執行個體。
3. 動作，將新增至路由字典應該繫結至動作參數 （通常是實體索引鍵） 的任何值。

讓我們看看一個特定的範例。 內建的路由慣例不支援對巡覽集合進行索引。 換句話說，沒有任何 Uri 慣例如下所示：

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

以下是查詢的自訂的路由慣例，來處理這種類型。

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

附註：

1. 我是衍生自**EntitySetRoutingConvention**，因為**SelectController**該類別中的方法是適用於這個新的路由慣例。 這表示我不需要重新實作**SelectController**。
2. 慣例只適用於 GET 要求和路徑範本時，才&quot;~/entityset/key/navigation/key&quot;。
3. 動作名稱是&quot;取得 {EntityType}&quot;，其中 *{EntityType}* 是瀏覽集合型別。 例如， &quot;GetSupplier&quot;。 您可以使用任何您喜歡的命名慣例&#8212;只要確定您符合對控制器動作。
4. 動作會採用兩個參數，叫做*金鑰*並*relatedKey*。 (如需一些預先定義的參數名稱的清單，請參閱 < [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)

下一個步驟加入新的慣例路由慣例的清單。 發生這種情況在設定期間，如下列程式碼所示：

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

以下是一些其他範例路由慣例，會有幫助研究：

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

和 Web API 本身當然是開放原始碼，因此您所見[原始程式碼](http://aspnetwebstack.codeplex.com/)內建的路由慣例。 這些定義在**System.Web.Http.OData.Routing.Conventions**命名空間。
