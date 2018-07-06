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
<a name="drag-and-drop-via-reorderlist-vb"></a>將拖放透過 reorderlist 的回傳 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> 在 AJAX Control Toolkit reorderlist 的回傳控制項提供使用者透過拖放來重新排列的清單。 目前的清單順序應該保存在伺服器上。


## <a name="overview"></a>總覽

`ReorderList` AJAX Control Toolkit 中的控制項提供使用者透過拖放來重新排列的清單。 目前的清單順序應該保存在伺服器上。

## <a name="steps"></a>步驟

`ReorderList`控制項支援將資料從資料庫繫結至清單。 最棒的是，它也支援寫入回資料存放區的清單項目順序的變更。

此範例會使用 Microsoft SQL Server 2005 Express 的 Edition 作為資料存放區。 Visual Studio 安裝程式，包括 express edition 是選擇性的 （且免費的） 一部分的資料庫。 也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。 如果您的設定不同，您必須調整資料庫的連接資訊。

若要將資料庫設定的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。 連接到伺服器，只要按兩下`Databases`並建立新的資料庫 (以滑鼠右鍵按一下，然後選擇  `New Database`) 稱為`Tutorials`。

在此資料庫中，建立新的資料表，稱為`AJAX`具有下列四個資料行：

- `id` (主索引鍵的整數，身分識別，不為 NULL)
- `char` (char(1)，NULL)
- `description` (varchar(50，NULL)
- `position` (int，NULL)


[![AJAX 資料表配置](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

AJAX 資料表配置 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image3.png))


接下來，填寫幾個值的資料表。 請注意，`position`資料行包含元素的排序次序。


[![中的 AJAX 資料表的初始資料](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

AJAX 資料表中的初始資料 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image6.png))


下一個步驟需要產生`SqlDataSource`控制項與新的資料庫和其資料表進行通訊。 資料來源必須支援`SELECT`和`UPDATE`SQL 命令。 稍後變更清單項目的順序，當`ReorderList`控制項會自動送出至資料來源的兩個值`Update`命令： 新的位置和項目的識別碼。 因此，在資料來源需求`<UpdateParameters>`這兩個值的區段：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList`控制項，必須先設定下列屬性：

- `AllowReorder`： 是否可以重新排列的清單項目
- `DataSourceID`： 資料來源的識別碼
- `DataKeyField`: 資料來源中的主索引鍵資料行的名稱
- `SortOrderField`： 提供清單項目的排序次序資料來源資料行

在 `<DragHandleTemplate>`和`<ItemTemplate>`章節中，清單的配置可以調整。 此外，資料繫結是可能使用`Eval()`方法，如下所示：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

下列 CSS 樣式資訊 (參考中`<DragHandleTemplate>`一節`ReorderList`控制項) 可確保，滑鼠指標適當地變更它停留拖曳控點上方時：

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

最後，`ScriptManager`控制項初始化 ASP.NET AJAX 頁面：

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

瀏覽器中執行此範例中，並稍微重新排列的清單項目。 然後，重新載入頁面及/或看看資料庫。 改變的位置以為受到維護的也會反映在值`position`資料行的資料庫中所有不需要任何程式碼中，只使用標記。


[![資料庫變更，根據新的清單項目順序中的資料](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

資料庫變更，根據新的清單中的資料，項目順序 ([按一下以檢視完整大小的影像](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [上一步](using-postbacks-with-reorderlist-vb.md)
