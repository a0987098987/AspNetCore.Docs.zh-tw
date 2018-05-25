---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: 路由慣例，在 ASP.NET Web API 2 Odata |Microsoft 文件
author: MikeWasson
description: 本文說明 Web API OData 端點所使用的路由慣例。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>路由慣例，在 ASP.NET Web API 2 Odata
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 本文說明 Web API OData 端點所使用的路由慣例。


當 Web API 取得 OData 要求時，它會將要求對應至控制器名稱和動作名稱中。 對應為基礎的 HTTP 方法和 URI。 例如，`GET /odata/Products(1)`對應至`ProductsController.GetProduct`。

在本文的第 1 部分，我會描述的內建的 OData 路由慣例。 這些慣例專為 OData 端點，這些屬性取代預設的 Web API 路由系統。 (當您呼叫時，會發生取代**MapODataRoute**。)

在第 2 部分，我會示範如何加入自訂路由慣例。 目前的內建的慣例不涵蓋整個範圍的 OData Uri，但您可以將其處理其他情況下擴充。

- [內建路由慣例](#conventions)
- [自訂路由慣例](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>內建路由慣例

我描述 Web API 中的 OData 路由慣例之前，最好先了解 OData Uri。 [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)所組成：

- 服務根目錄
- 資源路徑
- 查詢選項

![](odata-routing-conventions/_static/image1.png)

路由，重要的部分是資源路徑。 資源路徑會分成區段。 例如，`/Products(1)/Supplier`有三個區段：

- `Products`指的是名為 「 產品 」 實體集。
- `1`從集合中選取單一實體為實體索引鍵。
- `Supplier`為選取相關的實體的導覽屬性。

因此這個路徑會挑選出 1 產品的供應商。

> [!NOTE]
> OData 路徑區段未一律對應至 URI 區段。 例如，"1"會被視為路徑區段。


**控制器名稱。** 控制器名稱一定被衍生自的實體集資源路徑的根目錄。 例如，如果資源路徑是`/Products(1)/Supplier`，Web API 會尋找名為控制器`ProductsController`。

**動作名稱。** 下列表格所列的動作名稱衍生自的路徑區段，再加上的實體資料模型 (EDM)。 在某些情況下，您會有兩種選項的動作名稱。 例如，「 取得 」 或&quot;GetProducts&quot;。

**查詢實體**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| 取得 /entityset | / 產品 | GetEntitySet 或 Get | GetProducts |
| 取得 /entityset(key) | /Products(1) | GetEntityType 或 Get | GetProduct |
| 取得 /entityset （金鑰）/轉換 | /Products(1)/Models.Book | GetEntityType 或 Get | GetBook |

如需詳細資訊，請參閱[建立唯讀的 OData 端點](odata-v3/creating-an-odata-endpoint.md)。

**建立、 更新及刪除實體**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| 張貼 /entityset | / 產品 | PostEntityType 或 Post | PostProduct |
| PUT /entityset(key) | /Products(1) | PutEntityType 或 Put | PutProduct |
| PUT /entityset （金鑰）/轉換 | /Products(1)/Models.Book | PutEntityType 或 Put | PutBook |
| 修補程式 /entityset(key) | /Products(1) | PatchEntityType 或修補程式 | PatchProduct |
| 修補程式 /entityset （金鑰）/轉換 | /Products(1)/Models.Book | PatchEntityType 或修補程式 | PatchBook |
| DELETE /entityset(key) | /Products(1) | DeleteEntityType 或刪除 | DeleteProduct |
| DELETE /entityset(key)/cast | /Products(1)/Models.Book | DeleteEntityType 或刪除 | DeleteBook |

**查詢的導覽屬性**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| GET /entityset （金鑰） / 瀏覽 | /Products(1)/Supplier | GetNavigationFromEntityType or GetNavigation | GetSupplierFromProduct |
| 取得 /entityset （金鑰）/轉換/瀏覽 | /Products(1)/Models.Book/Author | GetNavigationFromEntityType or GetNavigation | GetAuthorFromBook |

如需詳細資訊，請參閱[使用實體關係](odata-v3/working-with-entity-relations.md)。

**建立和刪除連結**

| 要求 | 範例 URI | 動作名稱 |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | CreateLink |
| PUT 的 /entityset （金鑰） / $links/瀏覽 | /Products(1)/$links/Supplier | CreateLink |
| DELETE /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

如需詳細資訊，請參閱[使用實體關係](odata-v3/working-with-entity-relations.md)。

**屬性**

*需要 Web API 2*

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| GET /entityset （金鑰） / 屬性 | /Products(1)/Name | GetPropertyFromEntityType 或 GetProperty | GetNameFromProduct |
| 取得 /entityset （金鑰）/轉換/屬性 | /Products(1)/Models.Book/Author | GetPropertyFromEntityType 或 GetProperty | GetTitleFromBook |

**動作**

| 要求 | 範例 URI | 動作名稱 | 範例動作 |
| --- | --- | --- | --- |
| POST /entityset(key)/action | /Products(1)/Rate | ActionNameOnEntityType 或 ActionName | RateOnProduct |
| POST /entityset(key)/cast/action | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType 或 ActionName | CheckOutOnBook |

如需詳細資訊，請參閱[OData 動作](odata-v3/odata-actions.md)。

**方法簽章**

以下是一些方法簽章的規則：

- 如果路徑包含索引鍵，動作應該有一個名為參數*金鑰*。
- 如果路徑包含索引鍵到導覽屬性，此動作應該具有名為參數*relatedKey*。
- 裝飾*金鑰*和*relatedKey*參數 **[FromODataUri]** 參數。
- POST 和 PUT 要求需要實體類型的參數。
- PATCH 要求接受參數的型別**差異&lt;T&gt;**，其中*T*是實體類型。

如需參考，以下是範例，顯示每個內建的 OData 路由慣例的方法簽章。

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>自訂路由慣例

目前的內建的慣例不會討論所有可能的 OData Uri。 您可以藉由實作加入新的慣例**IODataRoutingConvention**介面。 這個介面有兩種方法：

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController**傳回控制器的名稱。
- **SelectAction**傳回動作的名稱。

這兩種方法，如果慣例不適用於該要求，方法應該傳回 null。

**ODataPath**參數所代表的已剖析的 OData 資源路徑。 它包含一份**[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** 執行個體，其中每個區段的資源路徑。 **ODataPathSegment**是抽象類別; 每個區段的類型由衍生自類別**ODataPathSegment**。

**ODataPath.TemplatePath**屬性是字串，代表串連所有的路徑區段。 例如，如果 URI 是`/Products(1)/Supplier`，路徑範本&quot;~/entityset/key/navigation&quot;。 請注意，區段不能直接對應到 URI 區段。 例如，實體索引鍵 (1) 以其本身**ODataPathSegment**。

一般而言，實作**IODataRoutingConvention**會進行下列作業：

1. 比較看這個慣例會套用至目前要求的路徑範本。 如果不適用，則傳回 null。
2. 適用於慣例，如果使用的屬性**ODataPathSegment**衍生控制器和動作名稱的執行個體。
3. 對於動作，加入路由字典，其中應該繫結至動作參數 （通常是實體索引鍵） 中的任何值。

讓我們看看特定範例。 內建路由慣例不支援瀏覽集合中進行索引。 換句話說，沒有任何慣例 uri，如下所示：

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

以下是查詢的自訂的路由慣例，來處理這種類型。

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

附註：

1. 我是衍生自**EntitySetRoutingConvention**，因為**SelectController**該類別中的方法是適用於這個新的路由慣例。 也就是說，不必重新實作**SelectController**。
2. 慣例僅適用於 GET 要求和路徑範本時才&quot;~/entityset/key/navigation/key&quot;。
3. 動作名稱是&quot;取得 {EntityType}&quot;，其中 *{EntityType}* 是瀏覽集合的型別。 例如， &quot;GetSupplier&quot;。 您可以使用任何您喜歡的命名慣例 & #8212;請確定您符合控制器的動作。
4. 採取的動作名稱為兩個參數*金鑰*和*relatedKey*。 (如需某些預先定義的參數名稱的清單，請參閱[ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)

下一個步驟新增新的慣例路由慣例的清單。 發生這種情況在設定期間，如下列程式碼所示：

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

以下是一些其他範例路由慣例來研究實用：

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

和 Web API 本身當然是開放原始碼，因此您可以看到[原始程式碼](http://aspnetwebstack.codeplex.com/)的內建路由慣例。 這些定義在**System.Web.Http.OData.Routing.Conventions**命名空間。
