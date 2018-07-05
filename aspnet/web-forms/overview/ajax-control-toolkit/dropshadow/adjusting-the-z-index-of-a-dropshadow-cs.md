---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: 調整 DropShadow (C#) 的 Z 軸索引 |Microsoft Docs
author: wenz
description: DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。 不過此陰影有時會與其他控制項，如安裝衝突...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 22732bc44a56819a2ec73346dc32659d027eaee0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369566"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="830fa-104">調整 DropShadow (C#) 的 Z 軸索引</span><span class="sxs-lookup"><span data-stu-id="830fa-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="830fa-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="830fa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="830fa-106">[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="830fa-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="830fa-107">DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="830fa-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="830fa-108">不過此陰影與其他控制項，例如 ASP.NET Menu 控制項中有時會發生衝突。</span><span class="sxs-lookup"><span data-stu-id="830fa-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="830fa-109">當功能表項目出現，它會出現在下拉式陰影後面。</span><span class="sxs-lookup"><span data-stu-id="830fa-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="830fa-110">總覽</span><span class="sxs-lookup"><span data-stu-id="830fa-110">Overview</span></span>

<span data-ttu-id="830fa-111">DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="830fa-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="830fa-112">不過此陰影與其他控制項，例如 ASP.NET Menu 控制項中有時會發生衝突。</span><span class="sxs-lookup"><span data-stu-id="830fa-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="830fa-113">當功能表項目出現，它會出現在下拉式陰影後面。</span><span class="sxs-lookup"><span data-stu-id="830fa-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="830fa-114">步驟</span><span class="sxs-lookup"><span data-stu-id="830fa-114">Steps</span></span>

<span data-ttu-id="830fa-115">程式碼開始面板本身，包含足夠的文字，使面板包含足夠的文字為可見的效果：</span><span class="sxs-lookup"><span data-stu-id="830fa-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="830fa-116">另一個面板直接之前放置`panelShadow`面板。</span><span class="sxs-lookup"><span data-stu-id="830fa-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="830fa-117">它包含具有水平方向的功能表，以便透過出現的功能表項目 (或者： 在)`dropShadow`面板):</span><span class="sxs-lookup"><span data-stu-id="830fa-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="830fa-118">然後，`DropShadowExtender`加入至擴充`panelShadow`面板下拉式陰影效果：</span><span class="sxs-lookup"><span data-stu-id="830fa-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="830fa-119">最後，ASP.NET AJAX`ScriptManager`控制項可讓控制項工具組運作：</span><span class="sxs-lookup"><span data-stu-id="830fa-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="830fa-120">當您執行此指令碼時，則會在下方面板出現的功能表項目。</span><span class="sxs-lookup"><span data-stu-id="830fa-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="830fa-121">不過功能表使用的 CSS 類別`panel`您只需要定義使項目會顯示在 [其他] 面板前面的兩件事：</span><span class="sxs-lookup"><span data-stu-id="830fa-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="830fa-122">相對位置</span><span class="sxs-lookup"><span data-stu-id="830fa-122">Relative positioning</span></span>
- <span data-ttu-id="830fa-123">正 z 軸索引</span><span class="sxs-lookup"><span data-stu-id="830fa-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="830fa-124">然後，`DropShadowExtender`控制項沒有與此功能表控制項沒事取這麼長衝突。</span><span class="sxs-lookup"><span data-stu-id="830fa-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="830fa-125">[![事前： 功能表項目看不到](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="830fa-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="830fa-126">事前： 不可見的功能表項目 ([按一下以檢視完整大小的影像](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="830fa-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="830fa-127">[![功能表項目會出現在之後：](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="830fa-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="830fa-128">之後： 會出現的功能表項目 ([按一下以檢視完整大小的影像](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="830fa-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="830fa-129">下一步</span><span class="sxs-lookup"><span data-stu-id="830fa-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
