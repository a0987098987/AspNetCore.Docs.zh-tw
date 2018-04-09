---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: 顯示項目詳細資料 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="display-item-details"></a><span data-ttu-id="260c2-102">顯示項目詳細資料</span><span class="sxs-lookup"><span data-stu-id="260c2-102">Display Item Details</span></span>
====================
<span data-ttu-id="260c2-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="260c2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="260c2-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="260c2-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="260c2-105">在本節中，您將加入檢視每本書的詳細資料的能力。</span><span class="sxs-lookup"><span data-stu-id="260c2-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="260c2-106">在 app.js，加入下列程式碼來檢視模型：</span><span class="sxs-lookup"><span data-stu-id="260c2-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="260c2-107">Views/Home/Index.cshtml，在詳細資料 連結加入資料繫結項目：</span><span class="sxs-lookup"><span data-stu-id="260c2-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="260c2-108">這會將繫結 click 處理常式&lt;&gt;元素`getBookDetail`檢視模型上的函式。</span><span class="sxs-lookup"><span data-stu-id="260c2-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="260c2-109">在同一個檔案中，將下列的標記總：</span><span class="sxs-lookup"><span data-stu-id="260c2-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="260c2-110">取代為這個：</span><span class="sxs-lookup"><span data-stu-id="260c2-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="260c2-111">這個標記會建立的資料表是資料繫結的屬性`detail`observable 中檢視模型。</span><span class="sxs-lookup"><span data-stu-id="260c2-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="260c2-112">「&lt;！-僅限 ko-&gt; &quot;語法可讓您包含了 DOM 項目之外的 Knockout 繫結。</span><span class="sxs-lookup"><span data-stu-id="260c2-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="260c2-113">在此情況下，`if`繫結會使標記来顯示的這一節時，才`details`為非 null。</span><span class="sxs-lookup"><span data-stu-id="260c2-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="260c2-114">現在，如果您執行應用程式，然後按一下其中一個&quot;詳細&quot;應用程式的連結，將會顯示活頁簿的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="260c2-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="260c2-115">[上一頁](part-7.md)
> [下一頁](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="260c2-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
