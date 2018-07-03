---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: 使用具有 reorderlist 的回傳 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit reorderlist 的回傳控制項提供使用者透過拖放來重新排列的清單。 已重新排列清單，每當 po...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: c99d4dcbb884b15aadd6165871749939239e0a8b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401905"
---
<a name="using-postbacks-with-reorderlist-vb"></a>使用具有 reorderlist 的回傳 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> 在 AJAX Control Toolkit reorderlist 的回傳控制項提供使用者透過拖放來重新排列的清單。 已重新排列清單，每當回傳會通知變更的伺服器。


## <a name="overview"></a>總覽

`ReorderList` AJAX Control Toolkit 中的控制項提供使用者透過拖放來重新排列的清單。 已重新排列清單，每當回傳會通知變更的伺服器。

## <a name="steps"></a>步驟

有數個可能的資料來源`ReorderList`控制項。 其中一個是使用`XmlDataSource`控制項：

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

若要繫結至這個 XML`ReorderList`必須設定控制項，並啟用回傳中，下列屬性：

- `DataSourceID`： 資料來源的識別碼
- `SortOrderField`： 排序依據屬性
- `AllowReorder`： 是否要允許使用者重新排列清單項目
- `PostBackOnReorder`： 是否要建立回傳，每當重新排列清單

以下是控制項的適當標記：

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

內`ReorderList`控制項，從資料來源的特定資料可能會繫結使用`Eval()`方法：

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

在頁面上的任意位置，標籤會包含的資訊，最後重新調整順序發生：

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

此標籤會填入處理回傳的伺服器端程式碼中的文字：

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

最後，若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放在頁面上：

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![每個重新排列觸發回傳](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

每個重新排列觸發回傳 ([按一下以檢視完整大小的影像](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](drag-and-drop-via-reorderlist-cs.md)
> [下一頁](drag-and-drop-via-reorderlist-vb.md)
