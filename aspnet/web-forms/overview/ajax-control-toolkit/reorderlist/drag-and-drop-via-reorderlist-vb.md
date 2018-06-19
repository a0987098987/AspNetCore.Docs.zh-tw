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
<a name="drag-and-drop-via-reorderlist-vb"></a>將拖放透過 ReorderList (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。 目前的清單順序應該保存在伺服器上。


## <a name="overview"></a>總覽

`ReorderList` AJAX Control Toolkit 中的控制項提供透過拖曳和卸除使用者可以重新排列的清單。 目前的清單順序應該保存在伺服器上。

## <a name="steps"></a>步驟

`ReorderList`控制項支援將資料從資料庫繫結至清單。 最棒的它也支援寫入至資料存放區的清單項目順序的變更。

這個範例會使用 Microsoft SQL Server 2005 Express 的版本做為資料存放區。 資料庫是選擇性 （和免費） 的一部分安裝的 Visual Studio，包括 express edition。 它也會提供在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。

將資料庫設定的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。 連接到伺服器，請按兩下`Databases`並建立新的資料庫 (以滑鼠右鍵按一下，然後選擇  `New Database`) 呼叫`Tutorials`。

在此資料庫中，建立名為的新資料表`AJAX`具有下列四個資料行：

- `id` (非 NULL 主索引鍵，整數、 身分識別)
- `char` （char （1)，NULL）
- `description` （varchar （50)，NULL）
- `position` (int，NULL)


[![AJAX 資料表配置](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

AJAX 資料表配置 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image3.png))


接下來，填入幾個值的資料表。 請注意，`position`資料行包含元素的排序次序。


[![AJAX 資料表的初始資料](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

AJAX 資料表中的初始資料 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image6.png))


下一步需要產生`SqlDataSource`與新的資料庫和其資料表進行控制。 資料來源必須支援`SELECT`和`UPDATE`SQL 命令。 當稍後變更清單項目的順序時，`ReorderList`控制項會自動送出至資料來源的兩個值`Update`命令： 新的位置和項目的識別碼。 因此，資料來源需求`<UpdateParameters>`一節以取得這兩個值：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList`控制項，必須先設定下列屬性：

- `AllowReorder`： 是否可以重新排列清單項目
- `DataSourceID`： 資料來源的識別碼
- `DataKeyField`: 資料來源中的主索引鍵資料行名稱
- `SortOrderField`： 提供清單項目的排序次序資料來源資料行

在`<DragHandleTemplate>`和`<ItemTemplate>`區段，清單的配置可以調整。 此外，資料繫結可能會使用`Eval()`方法，如下所示：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

下列 CSS 樣式資訊 (參考`<DragHandleTemplate>`區段`ReorderList`控制項) 可確保，滑鼠指標會變成適當時它將滑鼠停留拖曳控點：

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

最後，`ScriptManager`控制項初始化 ASP.NET AJAX 頁面：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

在瀏覽器中執行此範例中，位元重新排列清單項目。 然後，重新載入該頁面及/或看看資料庫。 改變的位置受到維護和也會反映在值`position`中的資料庫資料行而不需任何程式碼直接使用的標記。


[![資料庫變更，根據新的清單項目順序中的資料](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

資料庫變更，根據新的清單中的資料的項目順序 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [上一步](using-postbacks-with-reorderlist-vb.md)
