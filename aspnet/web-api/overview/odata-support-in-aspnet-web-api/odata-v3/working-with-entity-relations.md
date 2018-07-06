---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: 支援使用 Web API 2 OData v3 中的實體關聯性 |Microsoft Docs
author: MikeWasson
description: 大部分的資料集定義實體之間的關聯： 客戶具有訂單;活頁簿有作者;產品有供應商。 使用 OData 用戶端可以瀏覽透過...
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: b24e3ca4e3d39b424bec6bb408bb0f85825c6761
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810740"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>支援使用 Web API 2 OData v3 中的實體關聯性
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> 大部分的資料集定義實體之間的關聯： 客戶具有訂單;活頁簿有作者;產品有供應商。 使用 OData 用戶端可以瀏覽透過實體關聯性。 指定的產品，您可以找到供應商。 您也可以建立或移除關聯性。 例如，您可以設定一項產品的供應商。
> 
> 本教學課程會示範如何在 ASP.NET Web API 中支援這些作業。 本教學課程是根據本教學課程[建立 Web API 2 OData v3 端點](creating-an-odata-endpoint.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2
> - OData 版本 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>新增 Supplier 實體

首先，我們需要將新的實體類型新增至我們的 OData 摘要。 我們將新增`Supplier`類別。

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

這個類別會使用實體索引鍵的字串。 在實務上，這可能是較不常見，比使用整數索引鍵。 但是值得查看 OData 處理整數以外的其他金鑰類型的方式。

接下來，我們將建立關聯，藉由新增`Supplier`屬性設`Product`類別：

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

加入新**DbSet**要`ProductServiceContext`類別，以便包含 Entity Framework`Supplier`資料庫資料表中的。

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

在 WebApiConfig.cs 中，加入至 EDM 模型的 「 供應商 」 實體：

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>導覽屬性

若要取得產品的供應商，用戶端會傳送 GET 要求：

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

「 供應 」 是導覽屬性上的以下`Product`型別。 在此情況下，`Supplier`指的是單一項目，但導覽屬性也可以傳回 （-一對多或多對多關聯性） 的集合。

若要支援此要求，新增下列方法加入`ProductsController`類別：

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*金鑰*參數是產品的索引鍵。 方法會傳回相關的實體 & #8212 在此情況下，`Supplier`執行個體。 方法名稱和參數名稱都很重要。 一般情況下，如果導覽屬性名稱為"X"，您需要新增一個名為"GetX 」 方法。 此方法必須採用名為的參數 」*金鑰*"相符的父索引鍵的資料類型。

它也是包含重要 **[FromOdataUri]** 屬性中*金鑰*參數。 此屬性會告知 Web API，以剖析要求 URI 中的索引鍵時，請使用 OData 語法規則。

## <a name="creating-and-deleting-links"></a>建立和刪除連結

OData 支援建立或移除兩個實體之間的關聯性。 OData 術語中的關聯性是 「 連結 」。 每個連結都具有格式的 URI*實體*/$links /*實體*。 例如，從產品到供應商的連結看起來像這樣：

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

若要建立新的連結，用戶端會傳送 POST 要求到 URI 的連結。 要求的主體是目標實體的 URI。 比方說，假設有一個具有索引鍵"CTSO 」 的供應商。 若要建立從 「 Product(1)"至"Supplier('CTSO') 」 的連結，用戶端會傳送要求，如下所示：

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

若要刪除的連結，用戶端會傳送 DELETE 要求到 URI 的連結。

**建立連結**

若要啟用用戶端來建立產品供應商的連結，新增下列程式碼`ProductsController`類別：

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

這個方法會採用三個參數：

- *索引鍵*： 父實體 （產品） 的索引鍵
- *navigationProperty*： 瀏覽屬性的名稱。 在此範例中，唯一有效的導覽屬性會是 「 供應 」。
- *連結*: OData URI 的相關實體。 這個值會從要求主體。 比方說，連結 URI 可能是"`http://localhost/odata/Suppliers('CTSO')`，這表示出具有識別碼 = 'CTSO'。

該方法會使用連結來查閱供應商。 如果找到相符的供應商，則方法會設定`Product.Supplier`屬性，並將結果儲存到資料庫。

最困難的部分剖析 URI 的連結。 基本上，您要模擬將 GET 要求傳送至該 URI 的結果。 下列 helper 方法會示範如何執行這項操作。 方法會叫用 Web API 路由程序，並取回**ODataPath**執行個體，表示已剖析的 OData 路徑。 連結 URI，其中一個區段應該是實體索引鍵。 （如果沒有，用戶端傳送不正確的 URI。）

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**刪除連結**

若要刪除的連結，新增下列程式碼`ProductsController`類別：

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

在此範例中，導覽屬性是單一`Supplier`實體。 如果導覽屬性為集合，要刪除的連結的 URI 必須包含相關的實體索引鍵。 例如: 

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

此要求移除客戶 1 的順序為 1。 在此情況下，DeleteLink 方法會具有下列簽章：

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey*參數會提供相關的實體索引鍵。 因此在您`DeleteLink`方法中，主要實體的查閱*金鑰*參數，尋找相關的實體，由*relatedKey*參數，然後移除關聯。 根據您的資料模型，您可能需要實作兩個版本的`DeleteLink`。 Web API 會呼叫要求 URI 為基礎的正確版本。
