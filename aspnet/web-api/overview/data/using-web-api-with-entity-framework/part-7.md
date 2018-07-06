---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: 建立檢視 (UI) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820982"
---
<a name="create-the-view-ui"></a><span data-ttu-id="a8d2d-102">建立檢視 (UI)</span><span class="sxs-lookup"><span data-stu-id="a8d2d-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="a8d2d-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a8d2d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a8d2d-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="a8d2d-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="a8d2d-105">在本節中，您會開始定義應用程式中，HTML，並加入 HTML 和檢視模型之間的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="a8d2d-106">開啟檔案 Views/Home/Index.cshtml。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="a8d2d-107">以下列取代該檔案的整個內容。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="a8d2d-108">大部分`div`項目是否有針對[Bootstrap](http://getbootstrap.com/)樣式。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="a8d2d-109">重要的項目是與`data-bind`屬性。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="a8d2d-110">這個屬性會連結至檢視模型的 HTML。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="a8d2d-111">例如: </span><span class="sxs-lookup"><span data-stu-id="a8d2d-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="a8d2d-112">在此範例中， &quot; `text` &quot;繫結的原因`<p>`若要顯示的值的項目`error`從檢視模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="a8d2d-113">請記得，`error`已宣告為`ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="a8d2d-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="a8d2d-114">每當新的值指派給`error`，Knockout 更新中的文字`<p>`項目。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="a8d2d-115">`foreach`繫結會指示的內容執行迴圈的 Knockout`books`陣列。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="a8d2d-116">針對陣列中每個項目，建立新 Knockout &lt;li&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="a8d2d-117">內容中的繫結`foreach`參考陣列項目上的屬性。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="a8d2d-118">例如: </span><span class="sxs-lookup"><span data-stu-id="a8d2d-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="a8d2d-119">這裡`text`繫結會讀取每本書的 [作者] 內容。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="a8d2d-120">如果您執行應用程式現在，它看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="a8d2d-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="a8d2d-121">網頁載入後，會以非同步方式載入的書籍清單。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="a8d2d-122">立即&quot;詳細資料&quot;連結沒有作用。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="a8d2d-123">我們將在下一節中新增這項功能。</span><span class="sxs-lookup"><span data-stu-id="a8d2d-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a8d2d-124">[上一頁](part-6.md)
> [下一頁](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="a8d2d-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
