---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "處理實體關聯 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 58a9dfb621630f23b37247b96ed3a19a661857f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="handling-entity-relations"></a>處理實體關聯性
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

本章節描述如何 EF 載入相關的實體，以及如何處理模型類別中的循環的導覽屬性的一些詳細資料。 （本章節提供背景知識，並不需要完成本教學課程。 如果您想要的話，請跳至[第 5 部分。](part-5.md)。)

## <a name="eager-loading-versus-lazy-loading"></a>Eager 載入與消極式載入

當使用關聯式資料庫的 EF，務必了解如何 EF 載入相關的資料。

也很有用，可以查看 EF 產生的 SQL 查詢。 若要追蹤的 SQL，加入下列一行程式碼`BookServiceContext`建構函式：

[!code-csharp[Main](part-4/samples/sample1.cs)]

如果您將 GET 要求傳送至 /api/books 時，它會傳回 JSON，如下所示：

[!code-console[Main](part-4/samples/sample2.cmd)]

您可以看到，[作者] 內容為 null，即使活頁簿包含有效 AuthorId。 這是因為 EF 不載入相關的作者實體。 SQL 查詢的追蹤記錄檔確認這項目：

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT 陳述式會從活頁簿資料表，且未參考作者資料表。

如需參考，以下是中的方法`BooksController`傳回書籍清單的類別。

[!code-csharp[Main](part-4/samples/sample4.cs)]

我們來看看我們如何傳回作者做為 JSON 資料的一部分。 載入 Entity Framework 中的相關的資料的三種方式： 積極式載入、 消極式載入，並明確式載入。 有一些與每種技術，取捨，因此請務必了解其運作方式。

### <a name="eager-loading"></a>積極式載入

與*積極式載入*，EF 載入相關的實體的初始資料庫查詢的一部分。 若要執行積極式載入，請使用**System.Data.Entity.Include**擴充方法。

[!code-csharp[Main](part-4/samples/sample5.cs)]

這會告訴 EF 作者資料包含在查詢中。 如果您進行這項變更，並執行應用程式，現在 JSON 資料看起來像這樣：

[!code-console[Main](part-4/samples/sample6.cmd)]

追蹤記錄檔會顯示 EF Book 和作者資料表執行聯結。

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>消極式載入

消極式載入，與 EF 自動載入相關的實體的導覽屬性，該實體取值時。 若要啟用消極式載入，請瀏覽屬性虛擬。 例如，在活頁簿類別：

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

現在請考慮下列程式碼：

[!code-csharp[Main](part-4/samples/sample9.cs)]

啟用消極式載入時，存取`Author`屬性`books[0]`導致 EF 作者查詢資料庫。

消極式載入需要多個資料庫往返，因為 EF 傳送查詢，每次它會擷取相關的實體。 通常，您會希望您所序列化的物件停用的消極式載入。 序列化程式已讀取所有觸發程序載入相關的實體的模型上的屬性。 例如，以下是 SQL 查詢時 EF 序列化的書籍清單與啟用的消極式載入。 您可以看到 EF，會針對三個作者三個不同的查詢。

[!code-console[Main](part-4/samples/sample10.sql)]

仍有一些您可能的想来使用消極式載入。 積極式載入可能會導致 EF 產生非常複雜的聯結。 或您可能需要一小部分的資料，相關的實體，而消極式載入可能會更有效率。

若要避免序列化問題的方法之一是要序列化的資料傳輸物件 (Dto) 而不是實體物件。 在本文稍後會示範這種方法。

### <a name="explicit-loading"></a>明確式載入

明確式載入是類似於延遲載入，不同之處在於您明確取得相關的資料中的程式碼;當您存取導覽屬性時，它不會發生自動。 明確式載入讓您更充分掌控時載入相關的資料，但需要額外的程式碼。 如需明確載入的詳細資訊，請參閱[載入相關實體](https://msdn.microsoft.com/data/jj574232#explicit)。

## <a name="navigation-properties-and-circular-references"></a>導覽屬性，並循環參考

當定義書籍和作者模型時，我上定義導覽屬性`Book`活頁簿作者關聯性的類別，但我未定義另一個方向的導覽屬性。

如果您加入至對應的導覽屬性，會發生什麼事`Author`類別？

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

不幸的是，這會有問題，當您序列化模型。 如果您載入相關的資料時，它會建立循環的物件圖形。

![](part-4/_static/image1.png)

當序列化圖形嘗試 JSON 或 XML 格式器時，則會擲回例外狀況。 兩個格式器會擲回不同的例外狀況訊息。 以下是範例 JSON 格式器：

[!code-console[Main](part-4/samples/sample12.cmd)]

以下是 XML 格式器：

[!code-xml[Main](part-4/samples/sample13.xml)]

其中一種解決方案是使用 DTOs 我下一節的說明。 或者，您可以設定處理圖表循環的 JSON 和 XML 格式器。 如需詳細資訊，請參閱[處理循環的物件參考](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。

此教學課程中，您不需要`Author.Book`導覽屬性，因此您可以省略它。

>[!div class="step-by-step"]
[上一頁](part-3.md)
[下一頁](part-5.md)
