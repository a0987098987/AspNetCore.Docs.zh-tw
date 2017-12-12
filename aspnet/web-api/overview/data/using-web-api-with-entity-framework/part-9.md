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
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="ca132-102">將新的項目加入至資料庫</span><span class="sxs-lookup"><span data-stu-id="ca132-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="ca132-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ca132-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ca132-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="ca132-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ca132-105">在本節中，您將加入的功能，讓使用者能夠建立新的書籍。</span><span class="sxs-lookup"><span data-stu-id="ca132-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="ca132-106">在 app.js，加入下列程式碼來檢視模型：</span><span class="sxs-lookup"><span data-stu-id="ca132-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="ca132-107">在 Index.cshtml，取代下列標記：</span><span class="sxs-lookup"><span data-stu-id="ca132-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="ca132-108">使用：</span><span class="sxs-lookup"><span data-stu-id="ca132-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="ca132-109">這個標記會建立新作者送出表單。</span><span class="sxs-lookup"><span data-stu-id="ca132-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="ca132-110">作者下拉式清單的值是資料繫結至`authors`observable 中檢視模型。</span><span class="sxs-lookup"><span data-stu-id="ca132-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="ca132-111">其他表單輸入的值是資料繫結至`newBook`檢視模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="ca132-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="ca132-112">表單上的傳送處理常式繫結至`addBook`函式：</span><span class="sxs-lookup"><span data-stu-id="ca132-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="ca132-113">`addBook`函式會讀取目前的資料繫結表單輸入值來建立 JSON 物件。</span><span class="sxs-lookup"><span data-stu-id="ca132-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="ca132-114">然後它會張貼的 JSON 物件`/api/books`。</span><span class="sxs-lookup"><span data-stu-id="ca132-114">Then it POSTs the JSON object to `/api/books`.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ca132-115">[上一頁](part-8.md)
[下一頁](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="ca132-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
