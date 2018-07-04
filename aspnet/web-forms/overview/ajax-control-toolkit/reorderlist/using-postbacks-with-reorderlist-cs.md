---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: 使用具有 reorderlist 的回傳 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit reorderlist 的回傳控制項提供使用者透過拖放來重新排列的清單。 已重新排列清單，每當 po...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 71e806b7915c010cec66931d87bd8c1f3b6d1fb3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365629"
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="52b2f-104">使用具有 reorderlist 的回傳 (C#)</span><span class="sxs-lookup"><span data-stu-id="52b2f-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="52b2f-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="52b2f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="52b2f-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="52b2f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="52b2f-107">在 AJAX Control Toolkit reorderlist 的回傳控制項提供使用者透過拖放來重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="52b2f-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="52b2f-108">已重新排列清單，每當回傳會通知變更的伺服器。</span><span class="sxs-lookup"><span data-stu-id="52b2f-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="52b2f-109">總覽</span><span class="sxs-lookup"><span data-stu-id="52b2f-109">Overview</span></span>

<span data-ttu-id="52b2f-110">`ReorderList` AJAX Control Toolkit 中的控制項提供使用者透過拖放來重新排列的清單。</span><span class="sxs-lookup"><span data-stu-id="52b2f-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="52b2f-111">已重新排列清單，每當回傳會通知變更的伺服器。</span><span class="sxs-lookup"><span data-stu-id="52b2f-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="52b2f-112">步驟</span><span class="sxs-lookup"><span data-stu-id="52b2f-112">Steps</span></span>

<span data-ttu-id="52b2f-113">有數個可能的資料來源`ReorderList`控制項。</span><span class="sxs-lookup"><span data-stu-id="52b2f-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="52b2f-114">其中一個是使用`XmlDataSource`控制項：</span><span class="sxs-lookup"><span data-stu-id="52b2f-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="52b2f-115">若要繫結至這個 XML`ReorderList`必須設定控制項，並啟用回傳中，下列屬性：</span><span class="sxs-lookup"><span data-stu-id="52b2f-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="52b2f-116">`DataSourceID`： 資料來源的識別碼</span><span class="sxs-lookup"><span data-stu-id="52b2f-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="52b2f-117">`SortOrderField`： 排序依據屬性</span><span class="sxs-lookup"><span data-stu-id="52b2f-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="52b2f-118">`AllowReorder`： 是否要允許使用者重新排列清單項目</span><span class="sxs-lookup"><span data-stu-id="52b2f-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="52b2f-119">`PostBackOnReorder`： 是否要建立回傳，每當重新排列清單</span><span class="sxs-lookup"><span data-stu-id="52b2f-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="52b2f-120">以下是控制項的適當標記：</span><span class="sxs-lookup"><span data-stu-id="52b2f-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="52b2f-121">內`ReorderList`控制項，從資料來源的特定資料可能會繫結使用`Eval()`方法：</span><span class="sxs-lookup"><span data-stu-id="52b2f-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="52b2f-122">在頁面上的任意位置，標籤會包含的資訊，最後重新調整順序發生：</span><span class="sxs-lookup"><span data-stu-id="52b2f-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="52b2f-123">此標籤會填入處理回傳的伺服器端程式碼中的文字：</span><span class="sxs-lookup"><span data-stu-id="52b2f-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="52b2f-124">最後，若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放在頁面上：</span><span class="sxs-lookup"><span data-stu-id="52b2f-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="52b2f-125">[![每個重新排列觸發回傳](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="52b2f-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="52b2f-126">每個重新排列觸發回傳 ([按一下以檢視完整大小的影像](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="52b2f-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="52b2f-127">下一步</span><span class="sxs-lookup"><span data-stu-id="52b2f-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
