---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: 使用多個快顯視窗控制項 (VB) |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 此外，也可以使用 m...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a2be36ecf17a95d53e5e912ba0a90113f70f2fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807631"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="c2294-104">使用多個快顯視窗控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="c2294-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="c2294-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c2294-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c2294-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c2294-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="c2294-107">PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="c2294-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="c2294-108">它也可使用一個以上的快顯視窗控制項，在單一頁面上。</span><span class="sxs-lookup"><span data-stu-id="c2294-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="c2294-109">總覽</span><span class="sxs-lookup"><span data-stu-id="c2294-109">Overview</span></span>

<span data-ttu-id="c2294-110">PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="c2294-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="c2294-111">它也可使用一個以上的快顯視窗控制項，在單一頁面上。</span><span class="sxs-lookup"><span data-stu-id="c2294-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="c2294-112">步驟</span><span class="sxs-lookup"><span data-stu-id="c2294-112">Steps</span></span>

<span data-ttu-id="c2294-113">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="c2294-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="c2294-114">接下來，新增一個面板，以做為快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="c2294-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="c2294-115">在目前的案例中，包含面板`Calendar`控制項。</span><span class="sxs-lookup"><span data-stu-id="c2294-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="c2294-116">為了避免因行事曆的回傳的重新整理頁面，面板會放在`UpdatePanel`控制項：</span><span class="sxs-lookup"><span data-stu-id="c2294-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="c2294-117">此頁面也包含兩個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="c2294-117">The page also contains two text boxes.</span></span> <span data-ttu-id="c2294-118">針對每個文字方塊中，一旦文字方塊就會啟動，應該會出現快顯行事曆。</span><span class="sxs-lookup"><span data-stu-id="c2294-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="c2294-119">現在來擴充每個使用中的兩個文字方塊`PopupControlExtender`。</span><span class="sxs-lookup"><span data-stu-id="c2294-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="c2294-120">`TargetControlID`屬性提供繫結至擴充項控制項的 ID。</span><span class="sxs-lookup"><span data-stu-id="c2294-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="c2294-121">`PopupControlID`屬性包含的快顯面板識別碼。</span><span class="sxs-lookup"><span data-stu-id="c2294-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="c2294-122">在此情況下，這兩個擴充項會顯示相同的窗格中，但也是可行的不同的面板。</span><span class="sxs-lookup"><span data-stu-id="c2294-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="c2294-123">現在每當您按一下文字欄位中，行事曆會顯示以下欄位，可讓您選取的日期。</span><span class="sxs-lookup"><span data-stu-id="c2294-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="c2294-124">（回到選取的日期文字方塊將會說明在不同的教學課程。）</span><span class="sxs-lookup"><span data-stu-id="c2294-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="c2294-125">[![當使用者按一下文字方塊，即出現日曆](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c2294-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="c2294-126">當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c2294-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c2294-127">[上一頁](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [下一頁](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c2294-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
