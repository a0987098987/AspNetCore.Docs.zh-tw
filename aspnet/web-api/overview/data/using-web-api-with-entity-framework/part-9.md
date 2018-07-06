---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: 將新的項目加入至資料庫 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818504"
---
<a name="add-a-new-item-to-the-database"></a>將新的項目加入至資料庫
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

在本節中，您將加入可讓使用者建立新的書籍。 在 app.js 中加入檢視模型中的下列程式碼：

[!code-javascript[Main](part-9/samples/sample1.js)]

在 Index.cshtml 中，將下列標記：

[!code-html[Main](part-9/samples/sample2.html)]

成為：

[!code-html[Main](part-9/samples/sample3.html)]

此標記會建立的表單提交新的作者。 [作者] 下拉式清單的值是資料繫結至`authors`可觀察的檢視模型中。 其他表單輸入的值是資料繫結至`newBook`檢視模型的屬性。

提交處理常式，在表單上的繫結至`addBook`函式：

[!code-html[Main](part-9/samples/sample4.html)]

`addBook`函式會讀取所需資料繫結表單的輸入建立的 JSON 物件的目前值。 然後它會張貼的 JSON 物件`/api/books`。

> [!div class="step-by-step"]
> [上一頁](part-8.md)
> [下一頁](part-10.md)
