---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: 將拖放透過 reorderlist 的回傳 (VB) |Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 78cdb53ce6cbf32ccdf913f30439734f94922e8f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814206"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="3002d-103">將拖放透過 reorderlist 的回傳 (VB)</span><span class="sxs-lookup"><span data-stu-id="3002d-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="3002d-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3002d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3002d-105">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3002d-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="3002d-106">在 AJAX Control Toolkit reorderlist 的回傳控制項提供使用者透過拖放來重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="3002d-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="3002d-107">目前的清單順序應該保存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="3002d-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="3002d-108">總覽</span><span class="sxs-lookup"><span data-stu-id="3002d-108">Overview</span></span>

<span data-ttu-id="3002d-109">`ReorderList` AJAX Control Toolkit 中的控制項提供使用者透過拖放來重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="3002d-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="3002d-110">目前的清單順序應該保存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="3002d-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="3002d-111">步驟</span><span class="sxs-lookup"><span data-stu-id="3002d-111">Steps</span></span>

<span data-ttu-id="3002d-112">`ReorderList`控制項支援將資料從資料庫繫結至清單。</span><span class="sxs-lookup"><span data-stu-id="3002d-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="3002d-113">最棒的是，它也支援寫入回資料存放區的清單項目順序的變更。</span><span class="sxs-lookup"><span data-stu-id="3002d-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="3002d-114">此範例會使用 Microsoft SQL Server 2005 Express 的 Edition 作為資料存放區。</span><span class="sxs-lookup"><span data-stu-id="3002d-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="3002d-115">Visual Studio 安裝程式，包括 express edition 是選擇性的 （且免費的） 一部分的資料庫。</span><span class="sxs-lookup"><span data-stu-id="3002d-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="3002d-116">也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="3002d-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="3002d-117">此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。</span><span class="sxs-lookup"><span data-stu-id="3002d-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="3002d-118">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="3002d-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="3002d-119">若要將資料庫設定的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。</span><span class="sxs-lookup"><span data-stu-id="3002d-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="3002d-120">連接到伺服器，只要按兩下`Databases`並建立新的資料庫 (以滑鼠右鍵按一下，然後選擇  `New Database`) 稱為`Tutorials`。</span><span class="sxs-lookup"><span data-stu-id="3002d-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="3002d-121">在此資料庫中，建立新的資料表，稱為`AJAX`具有下列四個資料行：</span><span class="sxs-lookup"><span data-stu-id="3002d-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="3002d-122">`id` (主索引鍵的整數，身分識別，不為 NULL)</span><span class="sxs-lookup"><span data-stu-id="3002d-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="3002d-123">`char` (char(1)，NULL)</span><span class="sxs-lookup"><span data-stu-id="3002d-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="3002d-124">`description` (varchar(50，NULL)</span><span class="sxs-lookup"><span data-stu-id="3002d-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="3002d-125">`position` (int，NULL)</span><span class="sxs-lookup"><span data-stu-id="3002d-125">`position` (int, NULL)</span></span>


<span data-ttu-id="3002d-126">[![AJAX 資料表配置](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3002d-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="3002d-127">AJAX 資料表配置 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3002d-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="3002d-128">接下來，填寫幾個值的資料表。</span><span class="sxs-lookup"><span data-stu-id="3002d-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="3002d-129">請注意，`position`資料行包含元素的排序次序。</span><span class="sxs-lookup"><span data-stu-id="3002d-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="3002d-130">[![中的 AJAX 資料表的初始資料](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3002d-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="3002d-131">AJAX 資料表中的初始資料 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3002d-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="3002d-132">下一個步驟需要產生`SqlDataSource`控制項與新的資料庫和其資料表進行通訊。</span><span class="sxs-lookup"><span data-stu-id="3002d-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="3002d-133">資料來源必須支援`SELECT`和`UPDATE`SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="3002d-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="3002d-134">稍後變更清單項目的順序，當`ReorderList`控制項會自動送出至資料來源的兩個值`Update`命令： 新的位置和項目的識別碼。</span><span class="sxs-lookup"><span data-stu-id="3002d-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="3002d-135">因此，在資料來源需求`<UpdateParameters>`這兩個值的區段：</span><span class="sxs-lookup"><span data-stu-id="3002d-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="3002d-136">`ReorderList`控制項，必須先設定下列屬性：</span><span class="sxs-lookup"><span data-stu-id="3002d-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="3002d-137">`AllowReorder`： 是否可以重新排列的清單項目</span><span class="sxs-lookup"><span data-stu-id="3002d-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="3002d-138">`DataSourceID`： 資料來源的識別碼</span><span class="sxs-lookup"><span data-stu-id="3002d-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="3002d-139">`DataKeyField`: 資料來源中的主索引鍵資料行的名稱</span><span class="sxs-lookup"><span data-stu-id="3002d-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="3002d-140">`SortOrderField`： 提供清單項目的排序次序資料來源資料行</span><span class="sxs-lookup"><span data-stu-id="3002d-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="3002d-141">在 `<DragHandleTemplate>`和`<ItemTemplate>`章節中，清單的配置可以調整。</span><span class="sxs-lookup"><span data-stu-id="3002d-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="3002d-142">此外，資料繫結是可能使用`Eval()`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3002d-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="3002d-143">下列 CSS 樣式資訊 (參考中`<DragHandleTemplate>`一節`ReorderList`控制項) 可確保，滑鼠指標適當地變更它停留拖曳控點上方時：</span><span class="sxs-lookup"><span data-stu-id="3002d-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="3002d-144">最後，`ScriptManager`控制項初始化 ASP.NET AJAX 頁面：</span><span class="sxs-lookup"><span data-stu-id="3002d-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="3002d-145">瀏覽器中執行此範例中，並稍微重新排列的清單項目。</span><span class="sxs-lookup"><span data-stu-id="3002d-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="3002d-146">然後，重新載入頁面及/或看看資料庫。</span><span class="sxs-lookup"><span data-stu-id="3002d-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="3002d-147">改變的位置以為受到維護的也會反映在值`position`資料行的資料庫中所有不需要任何程式碼中，只使用標記。</span><span class="sxs-lookup"><span data-stu-id="3002d-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="3002d-148">[![資料庫變更，根據新的清單項目順序中的資料](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="3002d-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="3002d-149">資料庫變更，根據新的清單中的資料，項目順序 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="3002d-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3002d-150">上一步</span><span class="sxs-lookup"><span data-stu-id="3002d-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
