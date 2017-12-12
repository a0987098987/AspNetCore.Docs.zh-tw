---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "建立資料傳輸物件 (Dto) |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="69ff4-102">建立資料傳輸物件 (Dto)</span><span class="sxs-lookup"><span data-stu-id="69ff4-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="69ff4-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="69ff4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="69ff4-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="69ff4-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="69ff4-105">右現在，我們的 web 應用程式開發介面會公開給用戶端的資料庫實體。</span><span class="sxs-lookup"><span data-stu-id="69ff4-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="69ff4-106">用戶端接收會直接對應到資料庫資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="69ff4-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="69ff4-107">不過，這不一定是個好主意。</span><span class="sxs-lookup"><span data-stu-id="69ff4-107">However, that's not always a good idea.</span></span> <span data-ttu-id="69ff4-108">有時候您會想要變更傳送至用戶端資料的外觀。</span><span class="sxs-lookup"><span data-stu-id="69ff4-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="69ff4-109">例如，您可能要：</span><span class="sxs-lookup"><span data-stu-id="69ff4-109">For example, you might want to:</span></span>

- <span data-ttu-id="69ff4-110">請移除循環參考 （請參閱上一節）。</span><span class="sxs-lookup"><span data-stu-id="69ff4-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="69ff4-111">隱藏不應該用戶端檢視的特定屬性。</span><span class="sxs-lookup"><span data-stu-id="69ff4-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="69ff4-112">省略某些屬性，以減少裝載量。</span><span class="sxs-lookup"><span data-stu-id="69ff4-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="69ff4-113">壓平合併物件圖形包含巢狀的物件，使其更方便用戶端。</span><span class="sxs-lookup"><span data-stu-id="69ff4-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="69ff4-114">避免 「 過度張貼 」 弱點。</span><span class="sxs-lookup"><span data-stu-id="69ff4-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="69ff4-115">(請參閱[模型驗證](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)過度張貼的討論。)</span><span class="sxs-lookup"><span data-stu-id="69ff4-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="69ff4-116">Decouple 程式從您的資料庫層級的服務層級。</span><span class="sxs-lookup"><span data-stu-id="69ff4-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="69ff4-117">若要達成此目的，您可以定義*資料傳輸物件*(DTO)。</span><span class="sxs-lookup"><span data-stu-id="69ff4-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="69ff4-118">DTO 會定義如何將透過網路傳送資料的物件。</span><span class="sxs-lookup"><span data-stu-id="69ff4-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="69ff4-119">我們來看，與活頁簿之實體的運作方式。</span><span class="sxs-lookup"><span data-stu-id="69ff4-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="69ff4-120">在 [模型] 資料夾中，加入兩個 DTO 類別：</span><span class="sxs-lookup"><span data-stu-id="69ff4-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="69ff4-121">`BookDetailDTO`類別包含所有的屬性，從活頁簿模型，不同處在於`AuthorName`是會保留作者姓名的字串。</span><span class="sxs-lookup"><span data-stu-id="69ff4-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="69ff4-122">`BookDTO`類別包含從屬性的子集`BookDetailDTO`。</span><span class="sxs-lookup"><span data-stu-id="69ff4-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="69ff4-123">接下來，取代中的兩個 GET 方法`BooksController`類別，以傳回 DTOs 的版本。</span><span class="sxs-lookup"><span data-stu-id="69ff4-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="69ff4-124">我們會使用 LINQ**選取**Book 實體轉換 DTOs 的陳述式。</span><span class="sxs-lookup"><span data-stu-id="69ff4-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="69ff4-125">以下是產生新的 SQL`GetBooks`方法。</span><span class="sxs-lookup"><span data-stu-id="69ff4-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="69ff4-126">您可以看到 EF 轉譯 LINQ**選取**成 SQL SELECT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="69ff4-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="69ff4-127">最後，修改`PostBook`方法以傳回 DTO。</span><span class="sxs-lookup"><span data-stu-id="69ff4-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="69ff4-128">在本教學課程中，我們正在將轉換成 DTOs 以手動方式在程式碼中。</span><span class="sxs-lookup"><span data-stu-id="69ff4-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="69ff4-129">另一個選項是使用程式庫如[AutoMapper](http://automapper.org/)可自動處理轉換。</span><span class="sxs-lookup"><span data-stu-id="69ff4-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="69ff4-130">[上一頁](part-4.md)
[下一頁](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="69ff4-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
