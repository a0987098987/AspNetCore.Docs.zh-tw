---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "將新的項目加入至資料庫 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>將新的項目加入至資料庫
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將加入的功能，讓使用者能夠建立新的書籍。 在 app.js，加入下列程式碼來檢視模型：

[!code-javascript[Main](part-9/samples/sample1.js)]

在 Index.cshtml，取代下列標記：

[!code-html[Main](part-9/samples/sample2.html)]

使用：

[!code-html[Main](part-9/samples/sample3.html)]

這個標記會建立新作者送出表單。 作者下拉式清單的值是資料繫結至`authors`observable 中檢視模型。 其他表單輸入的值是資料繫結至`newBook`檢視模型的屬性。

表單上的傳送處理常式繫結至`addBook`函式：

[!code-html[Main](part-9/samples/sample4.html)]

`addBook`函式會讀取目前的資料繫結表單輸入值來建立 JSON 物件。 然後它會張貼的 JSON 物件`/api/books`。

>[!div class="step-by-step"]
[上一頁](part-8.md)
[下一頁](part-10.md)
