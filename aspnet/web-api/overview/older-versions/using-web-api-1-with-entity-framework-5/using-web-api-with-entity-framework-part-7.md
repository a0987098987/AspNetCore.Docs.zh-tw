---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 第 7 節： 建立主要頁面 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: c5b6cb0f2e48cdea3d6cc5cde72d08a99126f05f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825631"
---
<a name="part-7-creating-the-main-page"></a>第 7 節： 建立主要頁面
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>建立主要頁面

在本節中，您將建立主應用程式頁面。 此頁面將會比 [管理員] 頁面中，更為複雜，因此我們將方法的幾個步驟。 過程中，您會看到一些更進階的 Knockout.js 技術。 以下是基本的版面配置的頁面：

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- 「 產品 」 包含產品的陣列。
- 「 購物車 」 保留數量的產品的陣列。 按一下 「 加入購物車 」 更新購物車。
- 「 訂單 」 保留訂單識別碼的陣列。
- [詳細資料] 保留訂單詳細資料，也就是陣列的項目 （產品以數量）

我們一開始先在 HTML 中，定義一些基本的版面配置，不含資料繫結或指令碼。 開啟檔案 Views/Home/Index.cshtml，並將所有的內容取代為下列：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

接下來，新增指令碼區段，並建立空的檢視模型：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

根據設計，稍早所述，我們的檢視模型可以需要可預見值的產品、 購物車、 訂單及詳細資料。 新增下列變數以`AppViewModel`物件：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

使用者可以從產品清單的項目加入購物車，並移除購物車中的項目。 若要封裝這些函式，我們將建立代表產品的另一個檢視模型類別。 將下列程式碼新增至 `AppViewModel`：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel`類別包含兩個函式用來移動進出購物車的產品：`addItemToCart`一個單位的產品加入購物車和`removeAllFromCart`移除所有數量的產品。

使用者可以選取現有的訂單，並取得訂單的詳細資料。 我們將會封裝至其他檢視模型的這項功能：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel`初始化的順序，它將 AJAX 要求傳送至伺服器擷取訂單詳細資料。

另外而且請注意`total`屬性上的`OrderDetailsViewModel`。 這個屬性是一種特殊的可預見值呼叫[計算可預見值](http://knockoutjs.com/documentation/computedObservables.html)。 如同名稱所暗示，計算的可預見值可讓您計算的值的資料繫結&#8212;在此情況下，總成本的順序。

接下來，新增這些函式來`AppViewModel`:

- `resetCart` 移除購物車中的所有項目。
- `getDetails` 取得訂單的詳細資料 (由新的 pusing`OrderDetailsViewModel`拖曳至`details`清單)。
- `createOrder` 建立新的訂單，然後清空購物車。


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

最後，藉由提出 AJAX 要求產品和訂單的初始化檢視模型：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

好，這是許多程式碼，但我們在建置向上逐步，希望設計很清楚。 現在我們可以將某些 Knockout.js 繫結的 html。

**產品**

以下是產品清單中的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

這會逐一查看產品陣列，並顯示名稱和價格。 只有當使用者登入時，會顯示 [新增順序] 按鈕。

[新增順序] 按鈕呼叫`addItemToCart`上`ProductViewModel`產品的執行個體。 這示範了不錯的功能的 Knockout.js： 當檢視模型包含其他檢視模型時，您可以將繫結套用至內部模型。 在此範例中，內的繫結`foreach`套用至每個`ProductViewModel`執行個體。 這個方法會比較簡潔，比將所有的功能放入單一檢視模型。

**購物車**

以下是購物車的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

這會逐一查看車陣列，並顯示名稱、 價格和數量。 請注意，「 移除 」 連結的 「 建立訂單 」 按鈕會繫結至檢視模型函式。

**訂單**

以下是訂單清單的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

這會逐一查看訂單，並顯示訂單識別碼。 連結上的 click 事件繫結至`getDetails`函式。

**訂單詳細資料**

以下是訂單明細的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

這會逐一查看在順序中的項目，並顯示產品、 價格和 quanity。 周圍的 div 時，會詳細資料陣列包含一或多個項目，只顯示。

## <a name="conclusion"></a>結論

在本教學課程中，您可以建立使用 Entity Framework 與資料庫，並提供在資料層之上的公眾對應介面的 ASP.NET Web API 通訊的應用程式。 我們可以利用 ASP.NET MVC 4 來轉譯 HTML 網頁和 Knockout.js 以及 jQuery 提供動態的互動，而不需要頁面重新載入。

其他資源：

- [ASP.NET 資料存取內容對應](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework 開發人員中心](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [上一步](using-web-api-with-entity-framework-part-6.md)
