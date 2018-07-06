---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: 顯示項目詳細資料 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 29402e70e5fcaac04972788499695ddde4b96531
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832142"
---
<a name="display-item-details"></a><span data-ttu-id="c9b56-102">顯示項目詳細資料</span><span class="sxs-lookup"><span data-stu-id="c9b56-102">Display Item Details</span></span>
====================
<span data-ttu-id="c9b56-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c9b56-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c9b56-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="c9b56-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c9b56-105">在本節中，您將能夠檢視每個活頁簿的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c9b56-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="c9b56-106">在 app.js 中加入下列程式碼，檢視模型：</span><span class="sxs-lookup"><span data-stu-id="c9b56-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="c9b56-107">在 Views/Home/Index.cshtml，加入詳細資料 連結中的資料繫結項目：</span><span class="sxs-lookup"><span data-stu-id="c9b56-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="c9b56-108">這會將繫結的 click 處理常式&lt;&gt;項目`getBookDetail`檢視模型上的函式。</span><span class="sxs-lookup"><span data-stu-id="c9b56-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="c9b56-109">在相同的檔案，取代下列的標記總：</span><span class="sxs-lookup"><span data-stu-id="c9b56-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="c9b56-110">取代為這個：</span><span class="sxs-lookup"><span data-stu-id="c9b56-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="c9b56-111">此標記會建立資料繫結的屬性資料表`detail`可觀察的檢視模型中。</span><span class="sxs-lookup"><span data-stu-id="c9b56-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="c9b56-112">「&lt;！-僅限 ko-&gt; &quot;語法可讓您包含 Knockout 繫結之外的 DOM 項目。</span><span class="sxs-lookup"><span data-stu-id="c9b56-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="c9b56-113">在此情況下，`if`繫結會讓要顯示的標記的這一部分時，才`details`為非 null。</span><span class="sxs-lookup"><span data-stu-id="c9b56-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="c9b56-114">現在，如果您執行應用程式，然後按一下其中一個&quot;詳細資料&quot;應用程式的連結，將會顯示活頁簿詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c9b56-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c9b56-115">[上一頁](part-7.md)
> [下一頁](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="c9b56-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
