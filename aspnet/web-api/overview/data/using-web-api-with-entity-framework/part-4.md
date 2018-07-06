---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: 處理實體關聯性 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: fcfddb3d56d0be641d2df9d92c334776975621be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826153"
---
<a name="handling-entity-relations"></a>處理實體關聯性
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

本章節描述 EF 載入相關的實體的方式，以及如何處理您的模型類別中的循環的導覽屬性的一些詳細資料。 （本章節提供背景知識，並不需要完成這個教學課程。 如果您偏好，請跳至[第 5 部分。](part-5.md)。)

## <a name="eager-loading-versus-lazy-loading"></a>積極式載入和消極式載入的比較

當 EF 使用關聯式資料庫，請務必了解 EF 載入相關的資料的方式。

它也適合以查看 EF 產生的 SQL 查詢。 若要追蹤的 SQL，新增下列一行程式碼`BookServiceContext`建構函式：

[!code-csharp[Main](part-4/samples/sample1.cs)]

如果您將 GET 要求傳送至 /api/books 時，它會傳回 JSON，如下所示：

[!code-console[Main](part-4/samples/sample2.cmd)]

您可以看到，[作者] 內容為 null，即使活頁簿包含有效的 AuthorId。 這是因為 EF 不載入相關的作者實體。 追蹤記錄的 SQL 查詢進一步確認這一點：

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT 陳述式會從活頁簿資料表中，且未參考作者資料表。

如需參考，以下是中的方法`BooksController`傳回書籍清單的類別。

[!code-csharp[Main](part-4/samples/sample4.cs)]

我們來看看我們如何傳回作者的 JSON 資料的一部分。 有三種方式可以載入 Entity Framework 中的相關的資料： 積極式載入，消極式載入，並明確式載入。 有取捨，運用每一項技巧，因此請務必了解其運作方式。

### <a name="eager-loading"></a>積極式載入

具有*積極式載入*，EF 會載入相關的實體做為初始資料庫查詢的一部分。 若要執行積極式載入，請使用**System.Data.Entity.Include**擴充方法。

[!code-csharp[Main](part-4/samples/sample5.cs)]

這會告知 EF 來包含作者資料在查詢中。 如果您進行這項變更，並執行應用程式，現在看起來像這樣的 JSON 資料：

[!code-console[Main](part-4/samples/sample6.cmd)]

追蹤記錄檔會顯示 EF 執行的活頁簿和作者資料表的聯結。

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>消極式載入

消極式載入，EF 會自動載入相關的實體的導覽屬性，該實體取值時。 若要啟用消極式載入，請瀏覽屬性虛擬。 例如，在活頁簿類別：

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

現在，請考慮下列程式碼：

[!code-csharp[Main](part-4/samples/sample9.cs)]

啟用消極式載入時，存取`Author`屬性上的`books[0]`導致 EF 來撰寫查詢的資料庫。

消極式載入會需要多個資料庫來回行程，因為 EF 會傳送查詢，每次它會擷取相關的實體。 一般而言，您會想停用您序列化的物件的消極式載入。 序列化程式，就必須讀取所有模型中，這會觸發載入相關的實體上的屬性。 例如，以下是 SQL 查詢時 EF 啟用消極式載入序列化的書籍清單。 您可以看到，EF 會使三個不同的查詢針對三個作者。

[!code-console[Main](part-4/samples/sample10.sql)]

仍有一些您可能的想来使用消極式載入。 積極式載入可能會導致 EF 來產生非常複雜的聯結。 或您可能需要一小部分的資料，相關的實體，而消極式載入可能會更有效率。

為了避免發生序列化問題的一個方式是將資料傳輸物件 (Dto) 而不是實體物件序列化。 在本文稍後，我將示範這種方法。

### <a name="explicit-loading"></a>明確式載入

明確式載入是類似於消極式載入，不同之處在於您在程式碼，明確地取得相關的資料當您存取導覽屬性時，它不會發生自動。 明確式載入可讓您更充分掌控時載入相關的資料，但需要額外的程式碼。 如需明確式載入的詳細資訊，請參閱[載入相關實體](https://msdn.microsoft.com/data/jj574232#explicit)。

## <a name="navigation-properties-and-circular-references"></a>導覽屬性和循環參考

當我定義的活頁簿和作者模型時，我定義導覽屬性上`Book`類別的書籍作者關聯性，但我做並不會在另一個方向定義導覽屬性。

如果您新增至對應的導覽屬性，會發生什麼事`Author`類別？

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

不幸的是，當您序列化模型時造成問題。 如果您載入相關的資料時，它會建立循環的物件圖形。

![](part-4/_static/image1.png)

當 JSON 或 XML 格式器會嘗試將序列化圖形時，則會擲回例外狀況。 兩個格式器擲回不同的例外狀況訊息。 以下是 JSON 格式器的範例：

[!code-console[Main](part-4/samples/sample12.cmd)]

以下是 XML 格式器：

[!code-xml[Main](part-4/samples/sample13.xml)]

其中一個解決方案是使用 Dto，我在下一節中說明。 或者，您可以設定 JSON 和 XML 格式器，來處理圖形循環。 如需詳細資訊，請參閱 <<c0> [ 處理循環物件參考](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。

本教學課程中，您不需要`Author.Book`導覽屬性，因此您可以省略它。

> [!div class="step-by-step"]
> [上一頁](part-3.md)
> [下一頁](part-5.md)
