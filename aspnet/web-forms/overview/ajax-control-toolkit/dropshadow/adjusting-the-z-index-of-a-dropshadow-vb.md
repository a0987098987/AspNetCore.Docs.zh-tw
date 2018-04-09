---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: 調整 DropShadow (VB) Z-索引 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 不過此陰影有時會與其他控制項，如 insta 衝突...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="748f1-104">調整 DropShadow (VB) Z-索引</span><span class="sxs-lookup"><span data-stu-id="748f1-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="748f1-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="748f1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="748f1-106">[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="748f1-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="748f1-107">AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="748f1-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="748f1-108">不過此陰影有時會與其他控制項，例如 ASP.NET 功能表控制項衝突。</span><span class="sxs-lookup"><span data-stu-id="748f1-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="748f1-109">當功能表項目快顯，它會出現在下拉式陰影後面。</span><span class="sxs-lookup"><span data-stu-id="748f1-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="748f1-110">總覽</span><span class="sxs-lookup"><span data-stu-id="748f1-110">Overview</span></span>

<span data-ttu-id="748f1-111">AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="748f1-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="748f1-112">不過此陰影有時會與其他控制項，例如 ASP.NET 功能表控制項衝突。</span><span class="sxs-lookup"><span data-stu-id="748f1-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="748f1-113">當功能表項目快顯，它會出現在下拉式陰影後面。</span><span class="sxs-lookup"><span data-stu-id="748f1-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="748f1-114">步驟</span><span class="sxs-lookup"><span data-stu-id="748f1-114">Steps</span></span>

<span data-ttu-id="748f1-115">程式碼便會開始面板本身，使面板包含足夠的效果皆可看到的文字包含足夠的文字：</span><span class="sxs-lookup"><span data-stu-id="748f1-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="748f1-116">另一個面板直接之前放置`panelShadow`面板。</span><span class="sxs-lookup"><span data-stu-id="748f1-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="748f1-117">包含具有水平方向的功能表，使上方會出現的功能表項目 (否則： 底下)`dropShadow`面板):</span><span class="sxs-lookup"><span data-stu-id="748f1-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="748f1-118">然後，`DropShadowExtender`加入至擴充`panelShadow`面板下拉式陰影效果：</span><span class="sxs-lookup"><span data-stu-id="748f1-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="748f1-119">最後，ASP.NET AJAX`ScriptManager`控制項可讓控制項在工作的工具組：</span><span class="sxs-lookup"><span data-stu-id="748f1-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="748f1-120">當您執行此指令碼時，則會在面板下方出現的功能表項目。</span><span class="sxs-lookup"><span data-stu-id="748f1-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="748f1-121">不過功能表使用的 CSS 類別`panel`您只需要定義使項目會出現在其他面板前面兩件事：</span><span class="sxs-lookup"><span data-stu-id="748f1-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="748f1-122">相對位置</span><span class="sxs-lookup"><span data-stu-id="748f1-122">Relative positioning</span></span>
- <span data-ttu-id="748f1-123">正數 z-索引</span><span class="sxs-lookup"><span data-stu-id="748f1-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="748f1-124">然後，`DropShadowExtender`控制項未與此功能表控制項不再衝突。</span><span class="sxs-lookup"><span data-stu-id="748f1-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="748f1-125">[![事前： 功能表項目看不到](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="748f1-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="748f1-126">事前： 功能表項目看不到 ([按一下以檢視完整大小的影像](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="748f1-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="748f1-127">[![功能表項目會出現在之後：](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="748f1-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="748f1-128">之後： 會出現功能表項目 ([按一下以檢視完整大小的影像](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="748f1-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="748f1-129">[上一頁](manipulating-dropshadow-properties-from-client-code-cs.md)
> [下一頁](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="748f1-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
