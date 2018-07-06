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
<a name="handling-entity-relations"></a><span data-ttu-id="b2bcd-102">處理實體關聯性</span><span class="sxs-lookup"><span data-stu-id="b2bcd-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="b2bcd-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b2bcd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b2bcd-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="b2bcd-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b2bcd-105">本章節描述 EF 載入相關的實體的方式，以及如何處理您的模型類別中的循環的導覽屬性的一些詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="b2bcd-106">（本章節提供背景知識，並不需要完成這個教學課程。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="b2bcd-107">如果您偏好，請跳至[第 5 部分。](part-5.md)。)</span><span class="sxs-lookup"><span data-stu-id="b2bcd-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="b2bcd-108">積極式載入和消極式載入的比較</span><span class="sxs-lookup"><span data-stu-id="b2bcd-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="b2bcd-109">當 EF 使用關聯式資料庫，請務必了解 EF 載入相關的資料的方式。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="b2bcd-110">它也適合以查看 EF 產生的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="b2bcd-111">若要追蹤的 SQL，新增下列一行程式碼`BookServiceContext`建構函式：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="b2bcd-112">如果您將 GET 要求傳送至 /api/books 時，它會傳回 JSON，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="b2bcd-113">您可以看到，[作者] 內容為 null，即使活頁簿包含有效的 AuthorId。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="b2bcd-114">這是因為 EF 不載入相關的作者實體。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="b2bcd-115">追蹤記錄的 SQL 查詢進一步確認這一點：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="b2bcd-116">SELECT 陳述式會從活頁簿資料表中，且未參考作者資料表。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="b2bcd-117">如需參考，以下是中的方法`BooksController`傳回書籍清單的類別。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="b2bcd-118">我們來看看我們如何傳回作者的 JSON 資料的一部分。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="b2bcd-119">有三種方式可以載入 Entity Framework 中的相關的資料： 積極式載入，消極式載入，並明確式載入。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="b2bcd-120">有取捨，運用每一項技巧，因此請務必了解其運作方式。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="b2bcd-121">積極式載入</span><span class="sxs-lookup"><span data-stu-id="b2bcd-121">Eager Loading</span></span>

<span data-ttu-id="b2bcd-122">具有*積極式載入*，EF 會載入相關的實體做為初始資料庫查詢的一部分。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="b2bcd-123">若要執行積極式載入，請使用**System.Data.Entity.Include**擴充方法。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="b2bcd-124">這會告知 EF 來包含作者資料在查詢中。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="b2bcd-125">如果您進行這項變更，並執行應用程式，現在看起來像這樣的 JSON 資料：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="b2bcd-126">追蹤記錄檔會顯示 EF 執行的活頁簿和作者資料表的聯結。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="b2bcd-127">消極式載入</span><span class="sxs-lookup"><span data-stu-id="b2bcd-127">Lazy Loading</span></span>

<span data-ttu-id="b2bcd-128">消極式載入，EF 會自動載入相關的實體的導覽屬性，該實體取值時。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="b2bcd-129">若要啟用消極式載入，請瀏覽屬性虛擬。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="b2bcd-130">例如，在活頁簿類別：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="b2bcd-131">現在，請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="b2bcd-132">啟用消極式載入時，存取`Author`屬性上的`books[0]`導致 EF 來撰寫查詢的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="b2bcd-133">消極式載入會需要多個資料庫來回行程，因為 EF 會傳送查詢，每次它會擷取相關的實體。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="b2bcd-134">一般而言，您會想停用您序列化的物件的消極式載入。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="b2bcd-135">序列化程式，就必須讀取所有模型中，這會觸發載入相關的實體上的屬性。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="b2bcd-136">例如，以下是 SQL 查詢時 EF 啟用消極式載入序列化的書籍清單。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="b2bcd-137">您可以看到，EF 會使三個不同的查詢針對三個作者。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="b2bcd-138">仍有一些您可能的想来使用消極式載入。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="b2bcd-139">積極式載入可能會導致 EF 來產生非常複雜的聯結。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="b2bcd-140">或您可能需要一小部分的資料，相關的實體，而消極式載入可能會更有效率。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="b2bcd-141">為了避免發生序列化問題的一個方式是將資料傳輸物件 (Dto) 而不是實體物件序列化。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="b2bcd-142">在本文稍後，我將示範這種方法。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="b2bcd-143">明確式載入</span><span class="sxs-lookup"><span data-stu-id="b2bcd-143">Explicit Loading</span></span>

<span data-ttu-id="b2bcd-144">明確式載入是類似於消極式載入，不同之處在於您在程式碼，明確地取得相關的資料當您存取導覽屬性時，它不會發生自動。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="b2bcd-145">明確式載入可讓您更充分掌控時載入相關的資料，但需要額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="b2bcd-146">如需明確式載入的詳細資訊，請參閱[載入相關實體](https://msdn.microsoft.com/data/jj574232#explicit)。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="b2bcd-147">導覽屬性和循環參考</span><span class="sxs-lookup"><span data-stu-id="b2bcd-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="b2bcd-148">當我定義的活頁簿和作者模型時，我定義導覽屬性上`Book`類別的書籍作者關聯性，但我做並不會在另一個方向定義導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="b2bcd-149">如果您新增至對應的導覽屬性，會發生什麼事`Author`類別？</span><span class="sxs-lookup"><span data-stu-id="b2bcd-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="b2bcd-150">不幸的是，當您序列化模型時造成問題。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="b2bcd-151">如果您載入相關的資料時，它會建立循環的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="b2bcd-152">當 JSON 或 XML 格式器會嘗試將序列化圖形時，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="b2bcd-153">兩個格式器擲回不同的例外狀況訊息。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="b2bcd-154">以下是 JSON 格式器的範例：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="b2bcd-155">以下是 XML 格式器：</span><span class="sxs-lookup"><span data-stu-id="b2bcd-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="b2bcd-156">其中一個解決方案是使用 Dto，我在下一節中說明。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="b2bcd-157">或者，您可以設定 JSON 和 XML 格式器，來處理圖形循環。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="b2bcd-158">如需詳細資訊，請參閱 <<c0> [ 處理循環物件參考](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="b2bcd-159">本教學課程中，您不需要`Author.Book`導覽屬性，因此您可以省略它。</span><span class="sxs-lookup"><span data-stu-id="b2bcd-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2bcd-160">[上一頁](part-3.md)
> [下一頁](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="b2bcd-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
