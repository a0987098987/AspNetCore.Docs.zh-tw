---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: 顯示項目詳細資料 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 29402e70e5fcaac04972788499695ddde4b96531
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832142"
---
<a name="display-item-details"></a>顯示項目詳細資料
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將能夠檢視每個活頁簿的詳細資料。 在 app.js 中加入下列程式碼，檢視模型：

[!code-javascript[Main](part-8/samples/sample1.js)]

在 Views/Home/Index.cshtml，加入詳細資料 連結中的資料繫結項目：

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

這會將繫結的 click 處理常式&lt;&gt;項目`getBookDetail`檢視模型上的函式。

在相同的檔案，取代下列的標記總：

[!code-html[Main](part-8/samples/sample3.html)]

取代為這個：

[!code-html[Main](part-8/samples/sample4.html)]

此標記會建立資料繫結的屬性資料表`detail`可觀察的檢視模型中。

「&lt;！-僅限 ko-&gt; &quot;語法可讓您包含 Knockout 繫結之外的 DOM 項目。 在此情況下，`if`繫結會讓要顯示的標記的這一部分時，才`details`為非 null。

[!code-html[Main](part-8/samples/sample5.html)]

現在，如果您執行應用程式，然後按一下其中一個&quot;詳細資料&quot;應用程式的連結，將會顯示活頁簿詳細資料。

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [上一頁](part-7.md)
> [下一頁](part-9.md)
