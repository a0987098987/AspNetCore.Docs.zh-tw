---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: 建立資料傳輸物件 (Dto) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: a1be1b72559aaffdf530402313f58d6c2e25b189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828159"
---
<a name="create-data-transfer-objects-dtos"></a>建立資料傳輸物件 (Dto)
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

權限現在，我們的 web API 會公開至用戶端的資料庫實體。 用戶端收到直接對應到資料庫資料表的資料。 不過，這不一定是個好主意。 有時您想要變更的資料，您將傳送至用戶端物件的形狀。 例如，您可能要：

- 請移除循環參考 （請參閱上一節）。
- 隱藏特定用戶端不應該檢視的屬性。
- 省略某些屬性，以降低承載量大小。
- 壓平合併包含巢狀的物件，使其更方便用戶端的物件圖形。
- 避免 「 過度發佈 」 弱點。 (請參閱[模型驗證](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)過度發佈的討論。)
- 減少您的服務層，從您的資料庫層級。

若要達成此目的，您可以定義*資料傳輸物件*(DTO)。 DTO 是定義如何將透過網路傳送資料的物件。 我們來看，與活頁簿之實體的運作方式。 在 [模型] 資料夾中，新增兩個的 DTO 類別：

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO`類別包含的所有屬性從活頁簿模型中，不同之處在於`AuthorName`是可將保留作者名稱的字串。 `BookDTO`類別包含的屬性子集`BookDetailDTO`。

接下來，將兩個 GET 方法中的`BooksController`類別，以傳回 Dto 的版本。 我們將使用 LINQ**選取**將從活頁簿實體到 Dto 的陳述式。

[!code-csharp[Main](part-5/samples/sample2.cs)]

以下是產生新的 SQL`GetBooks`方法。 您可以看到 EF 轉譯 LINQ**選取**成 SQL SELECT 陳述式。

[!code-sql[Main](part-5/samples/sample3.sql)]

最後，修改`PostBook`方法來傳回 DTO。

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> 在本教學課程中，我們將轉換到 Dto 以手動方式在程式碼中。 另一個選項是使用程式庫，如[AutoMapper](http://automapper.org/)可自動處理轉換。
> 
> [!div class="step-by-step"]
> [上一頁](part-4.md)
> [下一頁](part-6.md)
