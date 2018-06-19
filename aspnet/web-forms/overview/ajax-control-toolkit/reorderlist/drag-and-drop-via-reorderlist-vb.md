---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: 將拖放透過 ReorderList (VB) |Microsoft 文件
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 99f47b969dc75efeec8485254d311c93dc0b5d35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878785"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="39743-103">將拖放透過 ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="39743-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="39743-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="39743-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="39743-105">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="39743-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="39743-106">AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="39743-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="39743-107">目前的清單順序應該保存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="39743-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="39743-108">總覽</span><span class="sxs-lookup"><span data-stu-id="39743-108">Overview</span></span>

<span data-ttu-id="39743-109">`ReorderList` AJAX Control Toolkit 中的控制項提供透過拖曳和卸除使用者可以重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="39743-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="39743-110">目前的清單順序應該保存在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="39743-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="39743-111">步驟</span><span class="sxs-lookup"><span data-stu-id="39743-111">Steps</span></span>

<span data-ttu-id="39743-112">`ReorderList`控制項支援將資料從資料庫繫結至清單。</span><span class="sxs-lookup"><span data-stu-id="39743-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="39743-113">最棒的它也支援寫入至資料存放區的清單項目順序的變更。</span><span class="sxs-lookup"><span data-stu-id="39743-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="39743-114">這個範例會使用 Microsoft SQL Server 2005 Express 的版本做為資料存放區。</span><span class="sxs-lookup"><span data-stu-id="39743-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="39743-115">資料庫是選擇性 （和免費） 的一部分安裝的 Visual Studio，包括 express edition。</span><span class="sxs-lookup"><span data-stu-id="39743-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="39743-116">它也會提供在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="39743-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="39743-117">此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="39743-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="39743-118">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="39743-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="39743-119">將資料庫設定的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。</span><span class="sxs-lookup"><span data-stu-id="39743-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="39743-120">連接到伺服器，請按兩下`Databases`並建立新的資料庫 (以滑鼠右鍵按一下，然後選擇  `New Database`) 呼叫`Tutorials`。</span><span class="sxs-lookup"><span data-stu-id="39743-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="39743-121">在此資料庫中，建立名為的新資料表`AJAX`具有下列四個資料行：</span><span class="sxs-lookup"><span data-stu-id="39743-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="39743-122">`id` (非 NULL 主索引鍵，整數、 身分識別)</span><span class="sxs-lookup"><span data-stu-id="39743-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="39743-123">`char` （char （1)，NULL）</span><span class="sxs-lookup"><span data-stu-id="39743-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="39743-124">`description` （varchar （50)，NULL）</span><span class="sxs-lookup"><span data-stu-id="39743-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="39743-125">`position` (int，NULL)</span><span class="sxs-lookup"><span data-stu-id="39743-125">`position` (int, NULL)</span></span>


<span data-ttu-id="39743-126">[![AJAX 資料表配置](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="39743-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="39743-127">AJAX 資料表配置 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="39743-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="39743-128">接下來，填入幾個值的資料表。</span><span class="sxs-lookup"><span data-stu-id="39743-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="39743-129">請注意，`position`資料行包含元素的排序次序。</span><span class="sxs-lookup"><span data-stu-id="39743-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="39743-130">[![AJAX 資料表的初始資料](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="39743-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="39743-131">AJAX 資料表中的初始資料 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="39743-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="39743-132">下一步需要產生`SqlDataSource`與新的資料庫和其資料表進行控制。</span><span class="sxs-lookup"><span data-stu-id="39743-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="39743-133">資料來源必須支援`SELECT`和`UPDATE`SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="39743-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="39743-134">當稍後變更清單項目的順序時，`ReorderList`控制項會自動送出至資料來源的兩個值`Update`命令： 新的位置和項目的識別碼。</span><span class="sxs-lookup"><span data-stu-id="39743-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="39743-135">因此，資料來源需求`<UpdateParameters>`一節以取得這兩個值：</span><span class="sxs-lookup"><span data-stu-id="39743-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="39743-136">`ReorderList`控制項，必須先設定下列屬性：</span><span class="sxs-lookup"><span data-stu-id="39743-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="39743-137">`AllowReorder`： 是否可以重新排列清單項目</span><span class="sxs-lookup"><span data-stu-id="39743-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="39743-138">`DataSourceID`： 資料來源的識別碼</span><span class="sxs-lookup"><span data-stu-id="39743-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="39743-139">`DataKeyField`: 資料來源中的主索引鍵資料行名稱</span><span class="sxs-lookup"><span data-stu-id="39743-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="39743-140">`SortOrderField`： 提供清單項目的排序次序資料來源資料行</span><span class="sxs-lookup"><span data-stu-id="39743-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="39743-141">在`<DragHandleTemplate>`和`<ItemTemplate>`區段，清單的配置可以調整。</span><span class="sxs-lookup"><span data-stu-id="39743-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="39743-142">此外，資料繫結可能會使用`Eval()`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="39743-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="39743-143">下列 CSS 樣式資訊 (參考`<DragHandleTemplate>`區段`ReorderList`控制項) 可確保，滑鼠指標會變成適當時它將滑鼠停留拖曳控點：</span><span class="sxs-lookup"><span data-stu-id="39743-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="39743-144">最後，`ScriptManager`控制項初始化 ASP.NET AJAX 頁面：</span><span class="sxs-lookup"><span data-stu-id="39743-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="39743-145">在瀏覽器中執行此範例中，位元重新排列清單項目。</span><span class="sxs-lookup"><span data-stu-id="39743-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="39743-146">然後，重新載入該頁面及/或看看資料庫。</span><span class="sxs-lookup"><span data-stu-id="39743-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="39743-147">改變的位置受到維護和也會反映在值`position`中的資料庫資料行而不需任何程式碼直接使用的標記。</span><span class="sxs-lookup"><span data-stu-id="39743-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="39743-148">[![資料庫變更，根據新的清單項目順序中的資料](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="39743-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="39743-149">資料庫變更，根據新的清單中的資料的項目順序 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="39743-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="39743-150">上一步</span><span class="sxs-lookup"><span data-stu-id="39743-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
