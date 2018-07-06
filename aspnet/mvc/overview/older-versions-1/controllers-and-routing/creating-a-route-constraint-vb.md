---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: 建立路由條件約束 (VB) |Microsoft Docs
author: StephenWalther
description: 在本教學課程中，Stephen Walther 會示範如何控制瀏覽器使用規則運算式中建立路由條件約束所要求的相符項目路由。
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 4be9b3c26fe456ae429160766b3366fef54ef1cc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806690"
---
<a name="creating-a-route-constraint-vb"></a>建立路由條件約束 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，Stephen Walther 會示範如何控制瀏覽器使用規則運算式中建立路由條件約束所要求的相符項目路由。


您可以使用路由條件約束來限制瀏覽器會要求符合特定的路由。 您可以使用規則運算式來指定路由條件約束。

例如，假設您已定義的路由清單 1 在 Global.asax 檔案中。

**列表 1-Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

列表 1 包含名為 Product 的路由。 您可以使用產品路由以將瀏覽器要求對應至 列表 2 中所包含的 ProductController。

**列表 2-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

請注意，Product 控制器所公開的 Details() 動作會接受名為 productId 的單一參數。 這個參數是一個整數參數。

在 列表 1 中定義的路由會符合任何下列 url:

- / 產品/23
- / 產品/7

不幸的是，路由也會比對下列 Url:

- / 產品/blah
- / 產品/apple

由於 Details() 動作預期整數參數，提出要求，其中包含整數值以外的項目會造成錯誤。 比方說，如果您的瀏覽器中輸入 URL /Product/apple 然後就會出現錯誤頁面 [圖 1] 中。


[![[新增專案] 對話方塊](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**圖 01**： 看到 explode 頁面 ([按一下以檢視完整大小的影像](creating-a-route-constraint-vb/_static/image2.png))


您真的要做什麼是只比對包含適當的整數 productId 的 Url。 可以使用條件約束定義的路由時，來限制路由相符的 Url。 修改的產品路由列表 3 中包含的規則運算式條件約束，只會比對整數。

**列表 3-Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

規則運算式 \d+ 比對一個或多個整數。 此條件約束會造成產品路由，以符合下列 Url:

- / 產品/3
- / 產品/8999

但下列 Url:

- / 產品/apple
- / 產品

這些瀏覽器要求交由另一個路由或，如果不有任何相符的路由*找不到資源*便會傳回錯誤。

> [!div class="step-by-step"]
> [上一頁](creating-custom-routes-vb.md)
> [下一頁](creating-a-custom-route-constraint-vb.md)
