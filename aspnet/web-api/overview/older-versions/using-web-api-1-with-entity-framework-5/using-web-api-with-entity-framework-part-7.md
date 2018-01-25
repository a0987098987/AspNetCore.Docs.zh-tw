---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: "第 7 部分： 建立主頁面 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 4d06e72bc664f707bbbe4603be41347158c58903
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="part-7-creating-the-main-page"></a>第 7 部分： 建立主頁面
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>建立主頁面

在本節中，您將建立主要應用程式頁面。 此頁面會比系統管理員 頁面上，更為複雜，所以我們會愈趨近它以幾個步驟。 過程中，您會看到一些更進階的解 Knockout.js 技術。 以下是頁面的基本的版面配置:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Products"保存產品的陣列。
- 「 車 」 會保存陣列的產品數量。 按一下 「 加入購物車 」 更新購物車。
- "Orders"保存訂單識別碼的陣列。
- [詳細資料] 保留順序的詳細資料，也就是陣列的項目 （產品與數量）

我們會先定義在 HTML 中，某些基本版面配置，不含資料繫結或指令碼。 開啟檔案 Views/Home/Index.cshtml，並以下列內容取代中的所有內容：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

接下來，加入指令碼區段，並建立空的檢視模型：

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

根據設計，稍早所述，我們的檢視模型中就需要可預見值的產品、 車、 訂單及詳細資料。 將下列變數加入`AppViewModel`物件：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

使用者可以將產品清單的項目新增至購物車，並移除購物車的項目。 若要將封裝這些函式，我們將建立另一個代表產品的檢視模型類別。 將下列程式碼新增至 `AppViewModel`：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel`類別包含兩個函式，可用來移動與購物車的產品：`addItemToCart`一個單位的產品加入購物車，和`removeAllFromCart`移除所有的產品數量。

使用者可以選取現有的訂單，並取得訂單詳細資料。 我們將會封裝到另一個檢視模型的這項功能：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel`初始化與訂單時，它將 AJAX 要求傳送至伺服器擷取的訂單詳細資料。

另外而且請注意`total`屬性`OrderDetailsViewModel`。 這個屬性是特殊種類的可觀察呼叫[計算 observable](http://knockoutjs.com/documentation/computedObservables.html)。 正如其名，計算的 observable 可讓您資料繫結到計算的值 &#8212; 在此情況下，總成本的順序。

接下來，加入這些函式`AppViewModel`:

- `resetCart`移除購物車中的所有項目。
- `getDetails`取得訂單的詳細資料 (由新 pusing`OrderDetailsViewModel`到`details`清單)。
- `createOrder`建立新的訂單，並清空購物車。


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

最後，藉由 products 和 orders 的 AJAX 要求初始化檢視模型：

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

好，這是大量程式碼，但我們建置它向上逐步，希望的設計是清除。 現在我們可以將某些解 Knockout.js 繫結的 html。

**產品**

以下是產品清單中的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

這會逐一查看產品陣列，並顯示名稱和價格。 只有當使用者登入時，會顯示 [新增順序] 按鈕。

[新增順序] 按鈕呼叫`addItemToCart`上`ProductViewModel`產品的執行個體。 這示範了解 Knockout.js 的方便的功能： 當檢視模型包含其他檢視模型時，您可以將繫結套用至內部模型。 在此範例中，內的繫結`foreach`套用到每一個`ProductViewModel`執行個體。 這個方法是比較簡單比置於單一檢視模型的所有功能。

**Cart**

以下是購物車的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

這會逐一查看車陣列，並顯示名稱、 價格和數量。 請注意 「 移除 」 連結和 [建立順序] 按鈕會繫結至檢視模型函式。

**訂單**

以下是訂單清單的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

這會逐一查看訂單，並顯示訂單識別碼。 Click 事件連結繫結至`getDetails`函式。

**訂單詳細資料**

以下是訂單詳細資料的繫結：

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

這會逐一查看順序中的項目，並顯示產品、 價格和 quanity。 周圍 div 是詳細資料陣列包含一或多個項目時，才看得。

## <a name="conclusion"></a>結論

在此教學課程中，您可以建立使用 Entity Framework 來與通訊的資料庫，並提供公開的介面，資料層之上的 ASP.NET Web API 的應用程式。 我們使用 ASP.NET MVC 4 來呈現 HTML 網頁和解 Knockout.js 加 jQuery 提供動態互動沒有頁面重新載入。

其他資源：

- [ASP.NET 資料存取內容對應](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework 開發人員中心](https://msdn.microsoft.com/data/ef)

>[!div class="step-by-step"]
[上一步](using-web-api-with-entity-framework-part-6.md)
