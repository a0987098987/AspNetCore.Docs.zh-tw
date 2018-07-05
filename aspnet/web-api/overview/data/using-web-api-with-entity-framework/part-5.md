---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: 建立資料傳輸物件 (Dto) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 6a066f1aca909afc2956e2026d9025cb87f10b1f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399921"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="1fcce-102">建立資料傳輸物件 (Dto)</span><span class="sxs-lookup"><span data-stu-id="1fcce-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="1fcce-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1fcce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1fcce-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="1fcce-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1fcce-105">權限現在，我們的 web API 會公開至用戶端的資料庫實體。</span><span class="sxs-lookup"><span data-stu-id="1fcce-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="1fcce-106">用戶端收到直接對應到資料庫資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="1fcce-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="1fcce-107">不過，這不一定是個好主意。</span><span class="sxs-lookup"><span data-stu-id="1fcce-107">However, that's not always a good idea.</span></span> <span data-ttu-id="1fcce-108">有時您想要變更的資料，您將傳送至用戶端物件的形狀。</span><span class="sxs-lookup"><span data-stu-id="1fcce-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="1fcce-109">例如，您可能要：</span><span class="sxs-lookup"><span data-stu-id="1fcce-109">For example, you might want to:</span></span>

- <span data-ttu-id="1fcce-110">請移除循環參考 （請參閱上一節）。</span><span class="sxs-lookup"><span data-stu-id="1fcce-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="1fcce-111">隱藏特定用戶端不應該檢視的屬性。</span><span class="sxs-lookup"><span data-stu-id="1fcce-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="1fcce-112">省略某些屬性，以降低承載量大小。</span><span class="sxs-lookup"><span data-stu-id="1fcce-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="1fcce-113">壓平合併包含巢狀的物件，使其更方便用戶端的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="1fcce-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="1fcce-114">避免 「 過度發佈 」 弱點。</span><span class="sxs-lookup"><span data-stu-id="1fcce-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="1fcce-115">(請參閱[模型驗證](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)過度發佈的討論。)</span><span class="sxs-lookup"><span data-stu-id="1fcce-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="1fcce-116">減少您的服務層，從您的資料庫層級。</span><span class="sxs-lookup"><span data-stu-id="1fcce-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="1fcce-117">若要達成此目的，您可以定義*資料傳輸物件*(DTO)。</span><span class="sxs-lookup"><span data-stu-id="1fcce-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="1fcce-118">DTO 是定義如何將透過網路傳送資料的物件。</span><span class="sxs-lookup"><span data-stu-id="1fcce-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="1fcce-119">我們來看，與活頁簿之實體的運作方式。</span><span class="sxs-lookup"><span data-stu-id="1fcce-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="1fcce-120">在 [模型] 資料夾中，新增兩個的 DTO 類別：</span><span class="sxs-lookup"><span data-stu-id="1fcce-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="1fcce-121">`BookDetailDTO`類別包含的所有屬性從活頁簿模型中，不同之處在於`AuthorName`是可將保留作者名稱的字串。</span><span class="sxs-lookup"><span data-stu-id="1fcce-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="1fcce-122">`BookDTO`類別包含的屬性子集`BookDetailDTO`。</span><span class="sxs-lookup"><span data-stu-id="1fcce-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="1fcce-123">接下來，將兩個 GET 方法中的`BooksController`類別，以傳回 Dto 的版本。</span><span class="sxs-lookup"><span data-stu-id="1fcce-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="1fcce-124">我們將使用 LINQ**選取**將從活頁簿實體到 Dto 的陳述式。</span><span class="sxs-lookup"><span data-stu-id="1fcce-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="1fcce-125">以下是產生新的 SQL`GetBooks`方法。</span><span class="sxs-lookup"><span data-stu-id="1fcce-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="1fcce-126">您可以看到 EF 轉譯 LINQ**選取**成 SQL SELECT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="1fcce-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="1fcce-127">最後，修改`PostBook`方法來傳回 DTO。</span><span class="sxs-lookup"><span data-stu-id="1fcce-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="1fcce-128">在本教學課程中，我們將轉換到 Dto 以手動方式在程式碼中。</span><span class="sxs-lookup"><span data-stu-id="1fcce-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="1fcce-129">另一個選項是使用程式庫，如[AutoMapper](http://automapper.org/)可自動處理轉換。</span><span class="sxs-lookup"><span data-stu-id="1fcce-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="1fcce-130">[上一頁](part-4.md)
> [下一頁](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="1fcce-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
