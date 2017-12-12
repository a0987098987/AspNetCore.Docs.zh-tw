---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: "顯示項目詳細資料 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a>顯示項目詳細資料
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將加入檢視每本書的詳細資料的能力。 在 app.js，加入下列程式碼來檢視模型：

[!code-javascript[Main](part-8/samples/sample1.js)]

Views/Home/Index.cshtml，在詳細資料 連結加入資料繫結項目：

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

這會將繫結 click 處理常式&lt;&gt;元素`getBookDetail`檢視模型上的函式。

在同一個檔案中，將下列的標記總：

[!code-html[Main](part-8/samples/sample3.html)]

取代為這個：

[!code-html[Main](part-8/samples/sample4.html)]

這個標記會建立的資料表是資料繫結的屬性`detail`observable 中檢視模型。

「&lt;！-僅限 ko-&gt; &quot;語法可讓您包含了 DOM 項目之外的 Knockout 繫結。 在此情況下，`if`繫結會使標記来顯示的這一節時，才`details`為非 null。

[!code-html[Main](part-8/samples/sample5.html)]

現在，如果您執行應用程式，然後按一下其中一個&quot;詳細&quot;應用程式的連結，將會顯示活頁簿的詳細資料。

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
[上一頁](part-7.md)
[下一頁](part-9.md)
