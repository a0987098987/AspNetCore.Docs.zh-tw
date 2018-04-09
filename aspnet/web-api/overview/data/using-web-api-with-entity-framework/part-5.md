---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: 建立資料傳輸物件 (Dto) |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a>建立資料傳輸物件 (Dto)
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

右現在，我們的 web 應用程式開發介面會公開給用戶端的資料庫實體。 用戶端接收會直接對應到資料庫資料表的資料。 不過，這不一定是個好主意。 有時候您會想要變更傳送至用戶端資料的外觀。 例如，您可能要：

- 請移除循環參考 （請參閱上一節）。
- 隱藏不應該用戶端檢視的特定屬性。
- 省略某些屬性，以減少裝載量。
- 壓平合併物件圖形包含巢狀的物件，使其更方便用戶端。
- 避免 「 過度張貼 」 弱點。 (請參閱[模型驗證](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)過度張貼的討論。)
- Decouple 程式從您的資料庫層級的服務層級。

若要達成此目的，您可以定義*資料傳輸物件*(DTO)。 DTO 會定義如何將透過網路傳送資料的物件。 我們來看，與活頁簿之實體的運作方式。 在 [模型] 資料夾中，加入兩個 DTO 類別：

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO`類別包含所有的屬性，從活頁簿模型，不同處在於`AuthorName`是會保留作者姓名的字串。 `BookDTO`類別包含從屬性的子集`BookDetailDTO`。

接下來，取代中的兩個 GET 方法`BooksController`類別，以傳回 DTOs 的版本。 我們會使用 LINQ**選取**Book 實體轉換 DTOs 的陳述式。

[!code-csharp[Main](part-5/samples/sample2.cs)]

以下是產生新的 SQL`GetBooks`方法。 您可以看到 EF 轉譯 LINQ**選取**成 SQL SELECT 陳述式。

[!code-sql[Main](part-5/samples/sample3.sql)]

最後，修改`PostBook`方法以傳回 DTO。

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> 在本教學課程中，我們正在將轉換成 DTOs 以手動方式在程式碼中。 另一個選項是使用程式庫如[AutoMapper](http://automapper.org/)可自動處理轉換。
> 
> [!div class="step-by-step"]
> [上一頁](part-4.md)
> [下一頁](part-6.md)
