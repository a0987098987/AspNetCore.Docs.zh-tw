---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: 建立檢視 (UI) |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-the-view-ui"></a>建立檢視 (UI)
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將啟動定義應用程式中，HTML，並加入 HTML 和檢視模型之間的資料繫結。

開啟檔案 Views/Home/Index.cshtml。 以下列內容取代該檔案的整個內容。

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

大部分的`div`項目有[Bootstrap](http://getbootstrap.com/)樣式設定。 重要的項目會具有`data-bind`屬性。 這個屬性的 HTML 連結檢視模型。

例如: 

[!code-html[Main](part-7/samples/sample2.html)]

在此範例中， &quot; `text` &quot;繫結原因`<p>`顯示值的項目`error`從檢視模型的屬性。 請記得，`error`已宣告為`ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

每當將新值指派給`error`，Knockout 更新中的文字`<p>`項目。

`foreach`繫結會告知的內容執行迴圈的 Knockout`books`陣列。 對於陣列中每個項目，建立新 Knockout &lt;li&gt;項目。 繫結內容中的`foreach`參考陣列項目上的屬性。 例如: 

[!code-html[Main](part-7/samples/sample4.html)]

這裡`text`繫結會讀取每本書的 [作者] 內容。

如果您執行應用程式現在，它看起來應該像這樣：

![](part-7/_static/image1.png)

書籍清單會以非同步方式載入後在頁面載入。 現在，&quot;詳細資料&quot;連結沒有作用。 我們會將這項功能新增下一節。

> [!div class="step-by-step"]
> [上一頁](part-6.md)
> [下一頁](part-8.md)
