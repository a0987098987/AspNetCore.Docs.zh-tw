---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: 建立路由條件約束 (C#) |Microsoft 文件
author: StephenWalther
description: 在本教學課程中，作者： Stephen Walther 會示範如何控制瀏覽器藉由建立路由條件約束使用規則運算式所要求的相符項目路由。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 3159feb6538e3048f4f235f7d549e692604ca4e7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-c"></a>建立路由條件約束 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，作者： Stephen Walther 會示範如何控制瀏覽器藉由建立路由條件約束使用規則運算式所要求的相符項目路由。


您可以使用路由條件約束來限制符合特定路由的瀏覽器要求。 您可以使用規則運算式來指定路由條件約束。

例如，假設您已定義路由清單 1 中 Global.asax 檔中。

**列出 1-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

清單 1 包含名為 Product 的路由。 若要將瀏覽器要求對應至包含在清單 2 ProductController 可用產品路由。

**Listing 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

請注意，Product 控制器所公開的 Details() 動作接受名為 productId 的單一參數。 這個參數是一個整數參數。

在程式碼範例 1 中定義的路由會符合任何下列 url:

- /Product/23
- / 產品/7

不幸的是，路由也會比對下列 Url:

- /Product/blah
- /Product/apple

Details() 動作必須要有一個整數參數，因為提出要求，其中包含整數值以外的項目會導致錯誤。 例如，如果您的瀏覽器中輸入 URL /Product/apple 然後就會出現錯誤頁面中圖 1。


[![新增專案 對話方塊](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**圖 01**： 看到分裂頁面 ([按一下以檢視完整大小的影像](creating-a-route-constraint-cs/_static/image2.png))


您真的想要做什麼是只會比對包含適當的整數 productId 的 Url。 可以使用條件約束定義的路由時，來限制路由相符的 Url。 修改的產品路由中列出的 3 包含只比對整數的規則運算式條件約束。

**列出 3-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

規則運算式 \d+ 比對一個或多個整數。 此條件約束會造成產品路由，以符合下列 Url:

- / 產品/3
- /Product/8999

但下列 Url:

- /Product/apple
- / 產品

- 這些瀏覽器要求將由另一個路由，或如果沒有相符的路由，*找不到資源*會傳回錯誤。

> [!div class="step-by-step"]
> [上一頁](creating-custom-routes-cs.md)
> [下一頁](creating-a-custom-route-constraint-cs.md)
