---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: 使用 ASP.NET Web API 2.2 OData v4 中的實體關聯 |Microsoft Docs
author: MikeWasson
description: 大部分的資料集定義實體之間的關聯： 客戶具有訂單;活頁簿有作者;產品有供應商。 使用 OData 用戶端可以瀏覽透過...
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d07ddab83462ee1bc84ba8ab15fe906937f506e6
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795388"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>使用 ASP.NET Web API 2.2 OData v4 中的實體關聯
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 大部分的資料集定義實體之間的關聯： 客戶具有訂單;活頁簿有作者;產品有供應商。 使用 OData 用戶端可以瀏覽透過實體關聯性。 指定的產品，您可以找到供應商。 您也可以建立或移除關聯性。 例如，您可以設定一項產品的供應商。
>
> 本教學課程會示範如何支援這些作業使用 ASP.NET Web API OData v4 中。 本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
>
> - Web API 2.1
> - OData v4
> - Visual Studio 2013 (下載 Visual Studio 2017[此處](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>教學課程的版本
>
> OData 第 3 版，請參閱 <<c0> [ 支援 OData v3 中的實體關聯性](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations)。

## <a name="add-a-supplier-entity"></a>新增 Supplier 實體

> [!NOTE]
> 本教學課程是根據本教學課程[建立 OData v4 端點使用 ASP.NET Web API 2](create-an-odata-v4-endpoint.md)。

首先，我們需要相關的實體。 新增類別，名為`Supplier`Models 資料夾中。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

將導覽屬性，加入`Product`類別：

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

加入新**DbSet**到`ProductsContext`類別，使 Entity Framework 會包含在資料庫中的供應商資料表。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

在 WebApiConfig.cs 中，新增&quot;供應商&quot;實體集的實體資料模型：

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>新增供應商控制器

新增`SuppliersController`Controllers 資料夾中的類別。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

我不會顯示如何將此控制站的 CRUD 作業。 步驟都與 Products 控制器相同 (請參閱[建立 OData v4 端點](create-an-odata-v4-endpoint.md))。

## <a name="getting-related-entities"></a>使用者入門的相關的實體

若要取得產品的供應商，用戶端會傳送 GET 要求：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

若要支援此要求，新增下列方法加入`ProductsController`類別：

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

這個方法會使用預設的命名慣例

- 方法名稱： GetX，其中 X 是導覽屬性。
- 參數名稱：*金鑰*

如果您遵循此命名慣例時，Web API 會自動將 HTTP 要求對應至控制器方法。

範例 HTTP 要求：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

範例 HTTP 回應：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>取得相關的集合

在上述範例中，產品會有一家供應商。 導覽屬性也可以傳回的集合。 下列程式碼會取得產品的供應商：

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

在此情況下，此方法會傳回**IQueryable**而不是**SingleResult&lt;T&gt;**

範例 HTTP 要求：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

範例 HTTP 回應：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>建立實體之間的關聯性

OData 支援建立或移除兩個現有的實體之間的關聯性。 此關聯性是在 OData v4 術語中，&quot;參考&quot;。 (在 OData v3，稱為關係*連結*。 通訊協定差異沒有影響本教學課程中。）

參考具有自己的 URI，與表單`/Entity/NavigationProperty/$ref`。 例如，以下是解決產品與產品的供應商之間的參考的 URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

若要加入關聯性，用戶端會傳送 POST 或 PUT 要求到這個地址。

- 如果導覽屬性是單一實體，例如將`Product.Supplier`。
- 如果導覽屬性是集合，例如張貼`Supplier.Products`。

在要求主體包含關聯性中的其他實體的 URI。 以下是範例要求：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

在此範例中，用戶端傳送的 PUT 要求`/Products(6)/Supplier/$ref`，這是 $ref URI`Supplier`的產品 ID = 6。 如果要求成功，伺服器會傳送 204 （沒有內容） 回應：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

以下是加入至關聯性的控制器方法`Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty*參數會指定要設定哪一個關聯性。 (如果實體有一個以上的導覽屬性，您可以新增更多`case`陳述式。)

*連結*參數包含供應商的 URI。 Web API 會自動剖析要求主體，以取得此參數的值。

若要尋找供應商，我們需要的識別碼 （或金鑰），這是屬於*連結*參數。 若要這樣做，請使用下列 helper 方法：

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

基本上，此方法會使用 OData 程式庫分成區段的 URI 路徑、 包含的索引鍵的區段和索引鍵轉換為正確的型別。

## <a name="deleting-a-relationship-between-entities"></a>刪除實體之間的關聯性

若要刪除關聯性，用戶端會傳送 HTTP DELETE 要求到 $ref 的 URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

以下是刪除產品與供應商之間的關聯性的控制器方法：

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

在此情況下，`Product.Supplier`已&quot;1&quot; 1 對多關聯性，讓您可以移除關聯性，只要將它設定結尾`Product.Supplier`到`null`。

在 &quot;許多&quot;結尾的關聯性，用戶端必須指定哪些相關實體中移除。 若要這樣做，用戶端會傳送要求的查詢字串中相關實體的 URI。 例如，若要移除 「 產品 1 」，從"1" 的供應商：

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

若要在 Web API 中支援此功能，我們需要加入額外的參數中`DeleteRef`方法。 以下是要移除的產品，從控制器方法`Supplier.Products`關聯性。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*金鑰*參數是供應商的索引鍵和*relatedKey*參數是要移除的產品金鑰`Products`關聯性。 請注意，Web API 會自動從查詢字串取得的索引鍵。
