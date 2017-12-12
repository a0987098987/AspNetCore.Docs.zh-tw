---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: "ReorderList (C#) 搭配使用回傳 |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。 已重新排列清單，每當 po..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 5f5c5e253f6d85203a488152c5ad908157e6ee0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-c"></a>回傳使用 ReorderList (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。 已重新排列清單，每當回傳應該通知變更的伺服器。


## <a name="overview"></a>概觀

`ReorderList` AJAX Control Toolkit 中的控制項提供透過拖曳和卸除使用者可以重新排列的清單。 已重新排列清單，每當回傳應該通知變更的伺服器。

## <a name="steps"></a>步驟

多個可能的資料來源的`ReorderList`控制項。 其中一個是使用`XmlDataSource`控制項：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

若要繫結至這個 XML`ReorderList`必須設定控制項，並啟用回傳，下列屬性：

- `DataSourceID`： 資料來源的識別碼
- `SortOrderField`： 若要依排序屬性
- `AllowReorder`： 是否要允許使用者重新排列清單項目
- `PostBackOnReorder`： 是否要建立回傳時重新排列清單

以下是適當的控制項的標記：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

內`ReorderList`控制項時，從資料來源的特定資料可能會使用繫結`Eval()`方法：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

在頁面上的任意位置，標籤將保存的資訊，最後重新調整順序發生：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

此標籤會填入處理回傳的伺服器端程式碼中的文字：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

最後，為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須放置在頁面上：

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![每個重新排列觸發回傳](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

每個重新排列觸發回傳 ([按一下以檢視完整大小的影像](using-postbacks-with-reorderlist-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一步](drag-and-drop-via-reorderlist-cs.md)
