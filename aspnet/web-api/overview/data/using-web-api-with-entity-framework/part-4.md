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
ms.openlocfilehash: 9294da7cd5b7a362d4ade9d1bf7e7747e20ee1a8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="handling-entity-relations"></a><span data-ttu-id="24a80-102">處理實體關聯性</span><span class="sxs-lookup"><span data-stu-id="24a80-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="24a80-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="24a80-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="24a80-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="24a80-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="24a80-105">本章節描述如何 EF 載入相關的實體，以及如何處理模型類別中的循環的導覽屬性的一些詳細資料。</span><span class="sxs-lookup"><span data-stu-id="24a80-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="24a80-106">（本章節提供背景知識，並不需要完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="24a80-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="24a80-107">如果您想要的話，請跳至[第 5 部分。](part-5.md)。)</span><span class="sxs-lookup"><span data-stu-id="24a80-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="24a80-108">Eager 載入與消極式載入</span><span class="sxs-lookup"><span data-stu-id="24a80-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="24a80-109">當使用關聯式資料庫的 EF，務必了解如何 EF 載入相關的資料。</span><span class="sxs-lookup"><span data-stu-id="24a80-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="24a80-110">也很有用，可以查看 EF 產生的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="24a80-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="24a80-111">若要追蹤的 SQL，加入下列一行程式碼`BookServiceContext`建構函式：</span><span class="sxs-lookup"><span data-stu-id="24a80-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="24a80-112">如果您將 GET 要求傳送至 /api/books 時，它會傳回 JSON，如下所示：</span><span class="sxs-lookup"><span data-stu-id="24a80-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="24a80-113">您可以看到，[作者] 內容為 null，即使活頁簿包含有效 AuthorId。</span><span class="sxs-lookup"><span data-stu-id="24a80-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="24a80-114">這是因為 EF 不載入相關的作者實體。</span><span class="sxs-lookup"><span data-stu-id="24a80-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="24a80-115">SQL 查詢的追蹤記錄檔確認這項目：</span><span class="sxs-lookup"><span data-stu-id="24a80-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="24a80-116">SELECT 陳述式會從活頁簿資料表，且未參考作者資料表。</span><span class="sxs-lookup"><span data-stu-id="24a80-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="24a80-117">如需參考，以下是中的方法`BooksController`傳回書籍清單的類別。</span><span class="sxs-lookup"><span data-stu-id="24a80-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="24a80-118">我們來看看我們如何傳回作者做為 JSON 資料的一部分。</span><span class="sxs-lookup"><span data-stu-id="24a80-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="24a80-119">載入 Entity Framework 中的相關的資料的三種方式： 積極式載入、 消極式載入，並明確式載入。</span><span class="sxs-lookup"><span data-stu-id="24a80-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="24a80-120">有一些與每種技術，取捨，因此請務必了解其運作方式。</span><span class="sxs-lookup"><span data-stu-id="24a80-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="24a80-121">積極式載入</span><span class="sxs-lookup"><span data-stu-id="24a80-121">Eager Loading</span></span>

<span data-ttu-id="24a80-122">與*積極式載入*，EF 載入相關的實體的初始資料庫查詢的一部分。</span><span class="sxs-lookup"><span data-stu-id="24a80-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="24a80-123">若要執行積極式載入，請使用**System.Data.Entity.Include**擴充方法。</span><span class="sxs-lookup"><span data-stu-id="24a80-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="24a80-124">這會告訴 EF 作者資料包含在查詢中。</span><span class="sxs-lookup"><span data-stu-id="24a80-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="24a80-125">如果您進行這項變更，並執行應用程式，現在 JSON 資料看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="24a80-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="24a80-126">追蹤記錄檔會顯示 EF Book 和作者資料表執行聯結。</span><span class="sxs-lookup"><span data-stu-id="24a80-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="24a80-127">消極式載入</span><span class="sxs-lookup"><span data-stu-id="24a80-127">Lazy Loading</span></span>

<span data-ttu-id="24a80-128">消極式載入，與 EF 自動載入相關的實體的導覽屬性，該實體取值時。</span><span class="sxs-lookup"><span data-stu-id="24a80-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="24a80-129">若要啟用消極式載入，請瀏覽屬性虛擬。</span><span class="sxs-lookup"><span data-stu-id="24a80-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="24a80-130">例如，在活頁簿類別：</span><span class="sxs-lookup"><span data-stu-id="24a80-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="24a80-131">現在請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="24a80-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="24a80-132">啟用消極式載入時，存取`Author`屬性`books[0]`導致 EF 作者查詢資料庫。</span><span class="sxs-lookup"><span data-stu-id="24a80-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="24a80-133">消極式載入需要多個資料庫往返，因為 EF 傳送查詢，每次它會擷取相關的實體。</span><span class="sxs-lookup"><span data-stu-id="24a80-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="24a80-134">通常，您會希望您所序列化的物件停用的消極式載入。</span><span class="sxs-lookup"><span data-stu-id="24a80-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="24a80-135">序列化程式已讀取所有觸發程序載入相關的實體的模型上的屬性。</span><span class="sxs-lookup"><span data-stu-id="24a80-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="24a80-136">例如，以下是 SQL 查詢時 EF 序列化的書籍清單與啟用的消極式載入。</span><span class="sxs-lookup"><span data-stu-id="24a80-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="24a80-137">您可以看到 EF，會針對三個作者三個不同的查詢。</span><span class="sxs-lookup"><span data-stu-id="24a80-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="24a80-138">仍有一些您可能的想来使用消極式載入。</span><span class="sxs-lookup"><span data-stu-id="24a80-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="24a80-139">積極式載入可能會導致 EF 產生非常複雜的聯結。</span><span class="sxs-lookup"><span data-stu-id="24a80-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="24a80-140">或您可能需要一小部分的資料，相關的實體，而消極式載入可能會更有效率。</span><span class="sxs-lookup"><span data-stu-id="24a80-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="24a80-141">若要避免序列化問題的方法之一是要序列化的資料傳輸物件 (Dto) 而不是實體物件。</span><span class="sxs-lookup"><span data-stu-id="24a80-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="24a80-142">在本文稍後會示範這種方法。</span><span class="sxs-lookup"><span data-stu-id="24a80-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="24a80-143">明確式載入</span><span class="sxs-lookup"><span data-stu-id="24a80-143">Explicit Loading</span></span>

<span data-ttu-id="24a80-144">明確式載入是類似於延遲載入，不同之處在於您明確取得相關的資料中的程式碼;當您存取導覽屬性時，它不會發生自動。</span><span class="sxs-lookup"><span data-stu-id="24a80-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="24a80-145">明確式載入讓您更充分掌控時載入相關的資料，但需要額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="24a80-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="24a80-146">如需明確載入的詳細資訊，請參閱[載入相關實體](https://msdn.microsoft.com/en-us/data/jj574232#explicit)。</span><span class="sxs-lookup"><span data-stu-id="24a80-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/en-us/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="24a80-147">導覽屬性，並循環參考</span><span class="sxs-lookup"><span data-stu-id="24a80-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="24a80-148">當定義書籍和作者模型時，我上定義導覽屬性`Book`活頁簿作者關聯性的類別，但我未定義另一個方向的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="24a80-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="24a80-149">如果您加入至對應的導覽屬性，會發生什麼事`Author`類別？</span><span class="sxs-lookup"><span data-stu-id="24a80-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="24a80-150">不幸的是，這會有問題，當您序列化模型。</span><span class="sxs-lookup"><span data-stu-id="24a80-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="24a80-151">如果您載入相關的資料時，它會建立循環的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="24a80-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="24a80-152">當序列化圖形嘗試 JSON 或 XML 格式器時，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="24a80-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="24a80-153">兩個格式器會擲回不同的例外狀況訊息。</span><span class="sxs-lookup"><span data-stu-id="24a80-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="24a80-154">以下是範例 JSON 格式器：</span><span class="sxs-lookup"><span data-stu-id="24a80-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="24a80-155">以下是 XML 格式器：</span><span class="sxs-lookup"><span data-stu-id="24a80-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="24a80-156">其中一種解決方案是使用 DTOs 我下一節的說明。</span><span class="sxs-lookup"><span data-stu-id="24a80-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="24a80-157">或者，您可以設定處理圖表循環的 JSON 和 XML 格式器。</span><span class="sxs-lookup"><span data-stu-id="24a80-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="24a80-158">如需詳細資訊，請參閱[處理循環的物件參考](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。</span><span class="sxs-lookup"><span data-stu-id="24a80-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="24a80-159">此教學課程中，您不需要`Author.Book`導覽屬性，因此您可以省略它。</span><span class="sxs-lookup"><span data-stu-id="24a80-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="24a80-160">[上一頁](part-3.md)
[下一頁](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="24a80-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
