---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: 將拖放透過 ReorderList (C#) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。 目前的清單順序應該...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42464d10f119e0ba51d5eebf2a67e76e3e419bda
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="fb49f-104">將拖放透過 ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="fb49f-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="fb49f-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fb49f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fb49f-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fb49f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="fb49f-107">AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="fb49f-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="fb49f-108">目前的清單順序應該保存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="fb49f-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="fb49f-109">總覽</span><span class="sxs-lookup"><span data-stu-id="fb49f-109">Overview</span></span>

<span data-ttu-id="fb49f-110">`ReorderList` AJAX Control Toolkit 中的控制項提供透過拖曳和卸除使用者可以重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="fb49f-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="fb49f-111">目前的清單順序應該保存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="fb49f-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="fb49f-112">步驟</span><span class="sxs-lookup"><span data-stu-id="fb49f-112">Steps</span></span>

<span data-ttu-id="fb49f-113">`ReorderList`控制項支援將資料從資料庫繫結至清單。</span><span class="sxs-lookup"><span data-stu-id="fb49f-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="fb49f-114">最棒的它也支援寫入至資料存放區的清單項目順序的變更。</span><span class="sxs-lookup"><span data-stu-id="fb49f-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="fb49f-115">這個範例會使用 Microsoft SQL Server 2005 Express 的版本做為資料存放區。</span><span class="sxs-lookup"><span data-stu-id="fb49f-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="fb49f-116">資料庫是選擇性 （和免費） 的一部分安裝的 Visual Studio，包括 express edition。</span><span class="sxs-lookup"><span data-stu-id="fb49f-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="fb49f-117">它也會提供在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="fb49f-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="fb49f-118">此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="fb49f-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="fb49f-119">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="fb49f-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="fb49f-120">將資料庫設定的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。</span><span class="sxs-lookup"><span data-stu-id="fb49f-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="fb49f-121">連接到伺服器，請按兩下`Databases`並建立新的資料庫 (以滑鼠右鍵按一下，然後選擇  `New Database`) 呼叫`Tutorials`。</span><span class="sxs-lookup"><span data-stu-id="fb49f-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="fb49f-122">在此資料庫中，建立名為的新資料表`AJAX`具有下列四個資料行：</span><span class="sxs-lookup"><span data-stu-id="fb49f-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="fb49f-123">`id` (非 NULL 主索引鍵，整數、 身分識別)</span><span class="sxs-lookup"><span data-stu-id="fb49f-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="fb49f-124">`char` （char （1)，NULL）</span><span class="sxs-lookup"><span data-stu-id="fb49f-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="fb49f-125">`description` （varchar （50)，NULL）</span><span class="sxs-lookup"><span data-stu-id="fb49f-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="fb49f-126">`position` (int，NULL)</span><span class="sxs-lookup"><span data-stu-id="fb49f-126">`position` (int, NULL)</span></span>


<span data-ttu-id="fb49f-127">[![AJAX 資料表配置](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fb49f-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="fb49f-128">AJAX 資料表配置 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fb49f-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="fb49f-129">接下來，填入幾個值的資料表。</span><span class="sxs-lookup"><span data-stu-id="fb49f-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="fb49f-130">請注意，`position`資料行包含元素的排序次序。</span><span class="sxs-lookup"><span data-stu-id="fb49f-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="fb49f-131">[![AJAX 資料表的初始資料](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fb49f-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="fb49f-132">AJAX 資料表中的初始資料 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fb49f-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="fb49f-133">下一步需要產生`SqlDataSource`與新的資料庫和其資料表進行控制。</span><span class="sxs-lookup"><span data-stu-id="fb49f-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="fb49f-134">資料來源必須支援`SELECT`和`UPDATE`SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="fb49f-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="fb49f-135">當稍後變更清單項目的順序時，`ReorderList`控制項會自動送出至資料來源的兩個值`Update`命令： 新的位置和項目的識別碼。</span><span class="sxs-lookup"><span data-stu-id="fb49f-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="fb49f-136">因此，資料來源需求`<UpdateParameters>`一節以取得這兩個值：</span><span class="sxs-lookup"><span data-stu-id="fb49f-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="fb49f-137">`ReorderList`控制項，必須先設定下列屬性：</span><span class="sxs-lookup"><span data-stu-id="fb49f-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="fb49f-138">`AllowReorder`： 是否可以重新排列清單項目</span><span class="sxs-lookup"><span data-stu-id="fb49f-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="fb49f-139">`DataSourceID`： 資料來源的識別碼</span><span class="sxs-lookup"><span data-stu-id="fb49f-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="fb49f-140">`DataKeyField`: 資料來源中的主索引鍵資料行名稱</span><span class="sxs-lookup"><span data-stu-id="fb49f-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="fb49f-141">`SortOrderField`： 提供清單項目的排序次序資料來源資料行</span><span class="sxs-lookup"><span data-stu-id="fb49f-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="fb49f-142">在`<DragHandleTemplate>`和`<ItemTemplate>`區段，清單的配置可以調整。</span><span class="sxs-lookup"><span data-stu-id="fb49f-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="fb49f-143">此外，資料繫結可能會使用`Eval()`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="fb49f-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="fb49f-144">下列 CSS 樣式資訊 (參考`<DragHandleTemplate>`區段`ReorderList`控制項) 可確保，滑鼠指標會變成適當時它將滑鼠停留拖曳控點：</span><span class="sxs-lookup"><span data-stu-id="fb49f-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="fb49f-145">最後，`ScriptManager`控制項初始化 ASP.NET AJAX 頁面：</span><span class="sxs-lookup"><span data-stu-id="fb49f-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="fb49f-146">在瀏覽器中執行此範例中，位元重新排列清單項目。</span><span class="sxs-lookup"><span data-stu-id="fb49f-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="fb49f-147">然後，重新載入該頁面及/或看看資料庫。</span><span class="sxs-lookup"><span data-stu-id="fb49f-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="fb49f-148">改變的位置受到維護和也會反映在值`position`中的資料庫資料行而不需任何程式碼直接使用的標記。</span><span class="sxs-lookup"><span data-stu-id="fb49f-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="fb49f-149">[![資料庫變更，根據新的清單項目順序中的資料](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fb49f-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="fb49f-150">資料庫變更，根據新的清單中的資料的項目順序 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="fb49f-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb49f-151">[上一頁](using-postbacks-with-reorderlist-cs.md)
> [下一頁](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fb49f-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
