---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: "ReorderList (VB) 搭配使用回傳 |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。 已重新排列清單，每當 po..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="bc02c-104">回傳使用 ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="bc02c-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="bc02c-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bc02c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bc02c-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bc02c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="bc02c-107">AJAX Control Toolkit ReorderList 控制項提供透過拖曳和卸除使用者可以重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="bc02c-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="bc02c-108">已重新排列清單，每當回傳應該通知變更的伺服器。</span><span class="sxs-lookup"><span data-stu-id="bc02c-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="bc02c-109">概觀</span><span class="sxs-lookup"><span data-stu-id="bc02c-109">Overview</span></span>

<span data-ttu-id="bc02c-110">`ReorderList` AJAX Control Toolkit 中的控制項提供透過拖曳和卸除使用者可以重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="bc02c-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="bc02c-111">已重新排列清單，每當回傳應該通知變更的伺服器。</span><span class="sxs-lookup"><span data-stu-id="bc02c-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="bc02c-112">步驟</span><span class="sxs-lookup"><span data-stu-id="bc02c-112">Steps</span></span>

<span data-ttu-id="bc02c-113">多個可能的資料來源的`ReorderList`控制項。</span><span class="sxs-lookup"><span data-stu-id="bc02c-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="bc02c-114">其中一個是使用`XmlDataSource`控制項：</span><span class="sxs-lookup"><span data-stu-id="bc02c-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="bc02c-115">若要繫結至這個 XML`ReorderList`必須設定控制項，並啟用回傳，下列屬性：</span><span class="sxs-lookup"><span data-stu-id="bc02c-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="bc02c-116">`DataSourceID`： 資料來源的識別碼</span><span class="sxs-lookup"><span data-stu-id="bc02c-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="bc02c-117">`SortOrderField`： 若要依排序屬性</span><span class="sxs-lookup"><span data-stu-id="bc02c-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="bc02c-118">`AllowReorder`： 是否要允許使用者重新排列清單項目</span><span class="sxs-lookup"><span data-stu-id="bc02c-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="bc02c-119">`PostBackOnReorder`： 是否要建立回傳時重新排列清單</span><span class="sxs-lookup"><span data-stu-id="bc02c-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="bc02c-120">以下是適當的控制項的標記：</span><span class="sxs-lookup"><span data-stu-id="bc02c-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="bc02c-121">內`ReorderList`控制項時，從資料來源的特定資料可能會使用繫結`Eval()`方法：</span><span class="sxs-lookup"><span data-stu-id="bc02c-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="bc02c-122">在頁面上的任意位置，標籤將保存的資訊，最後重新調整順序發生：</span><span class="sxs-lookup"><span data-stu-id="bc02c-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="bc02c-123">此標籤會填入處理回傳的伺服器端程式碼中的文字：</span><span class="sxs-lookup"><span data-stu-id="bc02c-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="bc02c-124">最後，為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須放置在頁面上：</span><span class="sxs-lookup"><span data-stu-id="bc02c-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="bc02c-125">[![每個重新排列觸發回傳](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bc02c-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="bc02c-126">每個重新排列觸發回傳 ([按一下以檢視完整大小的影像](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bc02c-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bc02c-127">[上一頁](drag-and-drop-via-reorderlist-cs.md)
[下一頁](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bc02c-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
