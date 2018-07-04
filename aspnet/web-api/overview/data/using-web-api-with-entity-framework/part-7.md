---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: 建立檢視 (UI) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364977"
---
<a name="create-the-view-ui"></a>建立檢視 (UI)
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您會開始定義應用程式中，HTML，並加入 HTML 和檢視模型之間的資料繫結。

開啟檔案 Views/Home/Index.cshtml。 以下列取代該檔案的整個內容。

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

大部分`div`項目是否有針對[Bootstrap](http://getbootstrap.com/)樣式。 重要的項目是與`data-bind`屬性。 這個屬性會連結至檢視模型的 HTML。

例如: 

[!code-html[Main](part-7/samples/sample2.html)]

在此範例中， &quot; `text` &quot;繫結的原因`<p>`若要顯示的值的項目`error`從檢視模型的屬性。 請記得，`error`已宣告為`ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

每當新的值指派給`error`，Knockout 更新中的文字`<p>`項目。

`foreach`繫結會指示的內容執行迴圈的 Knockout`books`陣列。 針對陣列中每個項目，建立新 Knockout &lt;li&gt;項目。 內容中的繫結`foreach`參考陣列項目上的屬性。 例如: 

[!code-html[Main](part-7/samples/sample4.html)]

這裡`text`繫結會讀取每本書的 [作者] 內容。

如果您執行應用程式現在，它看起來應該像這樣：

![](part-7/_static/image1.png)

網頁載入後，會以非同步方式載入的書籍清單。 立即&quot;詳細資料&quot;連結沒有作用。 我們將在下一節中新增這項功能。

> [!div class="step-by-step"]
> [上一頁](part-6.md)
> [下一頁](part-8.md)
