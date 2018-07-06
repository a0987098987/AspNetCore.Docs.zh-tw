---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 第 6 節： 建立產品和訂單控制器 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6ba7c20d9f529ccee83ce4fd85a1294047643f85
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841075"
---
<a name="part-6-creating-product-and-order-controllers"></a>第 6 節： 建立產品和訂單控制器
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>新增產品控制器

系統管理員控制器是具有系統管理員權限的使用者。 客戶，相反地，可以檢視產品，但無法建立、 更新或刪除它們。

我們輕鬆可以限制存取的 Post、 Put 和 Delete 方法，同時將 Get 方法維持開啟。 不過請看一下傳回產品的資料：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost`屬性不應該為可見的客戶 ！ 解決方法是定義*資料傳輸物件*(DTO)，其中包含應該向客戶顯示的屬性子集。 我們將使用 LINQ 來投射`Product`執行個體`ProductDTO`執行個體。

新增類別，名為`ProductDTO`到 Models 資料夾。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

現在加入控制器。 在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後選取**控制站**。 在 [**新增控制器**] 對話方塊中，將控制器命名&quot;ProductsController&quot;。 底下**樣板**，選取**空白 API 控制器**。

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

取代為下列程式碼的原始程式檔中的所有項目：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

控制器仍會使用`OrdersContext`查詢資料庫。 而不是傳回，但是`Product`執行個體直接，我們呼叫`MapProducts`投影到它們`ProductDTO`執行個體：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts`方法會傳回**IQueryable**，因此我們可以撰寫其他查詢參數的結果。 您可以看到這`GetProduct`方法，將**其中**子句加入查詢：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>新增訂單控制器

接下來，新增可讓使用者建立和檢視訂單的控制站。

我們將開始另一個的 DTO。 在方案總管 中，以滑鼠右鍵按一下 模型 資料夾並將類別加入名為`OrderDTO`使用下列實作：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

現在加入控制器。 在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後選取**控制站**。 在 [**新增控制器**] 對話方塊中，設定下列選項：

- 底下**控制器名稱**，輸入 「 OrdersController"。
- 底下**範本**，選取 「 讀取/寫入動作，使用 Entity Framework 的 API 控制器 」。
- 底下**模型類別**，選取&quot;順序 (ProductStore.Models)&quot;。
- 底下**資料內容類別**，選取&quot;OrdersContext (ProductStore.Models)&quot;。

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

按一下 [加入] 。 如此會將名為 OrdersController.cs 檔案。 接下來，我們需要修改控制器的預設實作。

首先，刪除`PutOrder`和`DeleteOrder`方法。 此範例中，客戶無法修改或刪除現有的訂單。 在實際的應用程式中，您需要後端邏輯來處理這些情況下很多。 （比方說，是訂單已出貨？）

變更`GetOrders`方法來傳回屬於使用者的訂單：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

變更`GetOrder`方法，如下所示：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

以下是我們對方法進行的變更：

- 傳回值是`OrderDTO`執行個體，而不是`Order`。
- 當我們查詢資料庫中的訂單時，我們會使用[DbQuery.Include](https://msdn.microsoft.com/library/gg696395)方法來擷取相關`OrderDetail`和`Product`實體。
- 我們使用投影來扁平化結果。

HTTP 回應將包含數量的產品的陣列：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

此格式會比原始的物件圖形包含巢狀的實體 （「 訂單 」、 「 詳細資訊和 「 產品 」） 所取用的用戶端更容易。

考慮到它的最後一個方法`PostOrder`。 現在，這個方法會採用`Order`執行個體。 請考慮會發生什麼事，但如果用戶端傳送的要求內文，像這樣：

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

這是以結構良好的順序，和 Entity Framework 會值得高興的是將它插入資料庫。 但它包含不存在先前的產品實體。 用戶端，只在資料庫中建立新的產品 ！ 當使用者查看 koala 引起的訂單時，這會是訂單履約部門的意外。 重點在於，真的小心您接受 POST 或 PUT 要求中的資料。

若要避免這個問題，變更`PostOrder`方法以讓`OrderDTO`執行個體。 使用`OrderDTO`來建立`Order`。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

請注意，我們會使用`ProductID`和`Quantity`屬性，和我們會略過任何產品名稱或價格的用戶端傳送的值。 如果產品識別碼不是有效的它會違反外部索引鍵條件約束，在資料庫中，並插入將會失敗，因為它應該。

以下是完整`PostOrder`方法：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

最後，新增**授權**屬性控制站：

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

現在已註冊的使用者可以建立或檢視的訂單。

> [!div class="step-by-step"]
> [上一頁](using-web-api-with-entity-framework-part-5.md)
> [下一頁](using-web-api-with-entity-framework-part-7.md)
