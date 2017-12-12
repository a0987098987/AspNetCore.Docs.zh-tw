---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "第 6 單元： 建立產品和順序控制站 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a>第 6 單元： 建立產品和順序控制站
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>新增產品控制站

系統管理控制站是有系統管理員權限使用者。 客戶，相反地，可以檢視產品，但無法建立、 更新或刪除它們。

我們可以輕鬆地限制存取權的 Post、 Put 和 Delete 方法，同時保留 Get 方法。 看傳回產品的資料：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost`屬性不應該為可見的客戶 ！ 解決方法是定義*資料傳輸物件*(DTO)，其中包含應該顯示給客戶的屬性子集。 我們將使用 LINQ 專案`Product`執行個體來`ProductDTO`執行個體。

將類別命名為`ProductDTO`到 Models 資料夾。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

現在加入控制器。 在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取**新增**，然後選取**控制器**。 在**加入控制器**對話方塊中，控制器&quot;ProductsController&quot;。 在下**範本**，選取**空白的 API 控制器**。

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

取代為下列程式碼的原始程式檔中的所有項目：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

控制器仍會使用`OrdersContext`查詢資料庫。 但不傳回`Product`執行個體直接，我們稱之為`MapProducts`投影至其`ProductDTO`執行個體：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts`方法會傳回**IQueryable**，因此我們可以撰寫其他查詢參數的結果。 您可以看到此中`GetProduct`方法，將**其中**子句加入查詢：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>新增訂單控制站

接下來，加入可讓使用者建立和檢視訂單的控制站。

我們將開始另一個 DTO。 在方案總管 中，以滑鼠右鍵按一下 模型 資料夾並加入類別，名為`OrderDTO`使用下列實作：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

現在加入控制器。 在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取**新增**，然後選取**控制器**。 在**加入控制器** 對話方塊中，設定下列選項：

- 在下**控制器名稱**，輸入 「 OrdersController"。
- 在下**範本**，選取"使用 Entity Framework 的讀取/寫入動作的 API 控制器 」。
- 在下**模型類別**，選取&quot;順序 (ProductStore.Models)&quot;。
- 在下**資料內容類別**，選取&quot;OrdersContext (ProductStore.Models)&quot;。

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

按一下 [加入] 。 這樣會加入名為 OrdersController.cs 的檔案。 接下來，我們必須修改控制器的預設實作。

首先，刪除`PutOrder`和`DeleteOrder`方法。 此範例中，客戶無法修改或刪除現有的訂單。 在實際應用中，您將需要許多後端邏輯來處理這些情況。 （例如，已訂單已出貨？）

變更`GetOrders`方法來傳回使用者所屬的訂單：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

變更`GetOrder`方法，如下所示：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

以下是我們對方法的變更：

- 傳回值是`OrderDTO`執行個體，而不是`Order`。
- 當我們查詢資料庫中的訂單時，我們使用[DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395)方法來提取相關`OrderDetail`和`Product`實體。
- 我們使用投影來扁平化結果。

HTTP 回應將包含產品與數量的陣列：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

此格式為便於用戶端使用比原來的物件圖形包含巢狀的實體 （順序、 詳細資料，以及產品）。

考慮到它的最後一個方法`PostOrder`。 現在，這個方法會採用`Order`執行個體。 會發生什麼情況，但如果用戶端傳送的要求內文，像這樣：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

這是結構完善的順序，和 Entity Framework 將愉快地保存他們將其插入到資料庫。 但它包含之前不存在的產品實體。 用戶端，只在資料庫中建立新的產品 ！ 當他們看到 koala 引起的順序時，這會是順序 fullfilment 部門的意外。 重點是，請小心實際上您接受 POST 或 PUT 要求中的資料。

若要避免這個問題，變更`PostOrder`方法以讓`OrderDTO`執行個體。 使用`OrderDTO`建立`Order`。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

請注意，我們使用`ProductID`和`Quantity`屬性，以及我們忽略任何用戶端傳送的產品名稱或價格的值。 如果產品識別碼不是有效的它將違反外部索引鍵條件約束，在資料庫中，並插入將會失敗，因為它應該。

以下是完整`PostOrder`方法：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

最後，會加入**授權**屬性控制站：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

現在已註冊的使用者可以建立或檢視訂單。

>[!div class="step-by-step"]
[上一頁](using-web-api-with-entity-framework-part-5.md)
[下一頁](using-web-api-with-entity-framework-part-7.md)
