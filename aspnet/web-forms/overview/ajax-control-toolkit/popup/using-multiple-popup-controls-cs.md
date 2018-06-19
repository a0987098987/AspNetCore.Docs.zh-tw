---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: 使用多個快顯視窗控制項 (C#) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 此外，也可以使用 m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 7acd1b53e1b3e3e0d09d248b68941b166da3e81e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869682"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="d8c49-104">使用多個快顯視窗控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="d8c49-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="d8c49-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d8c49-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d8c49-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d8c49-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="d8c49-107">AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="d8c49-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="d8c49-108">它也可用於在單一頁面上的多個 popup 控制項。</span><span class="sxs-lookup"><span data-stu-id="d8c49-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="d8c49-109">總覽</span><span class="sxs-lookup"><span data-stu-id="d8c49-109">Overview</span></span>

<span data-ttu-id="d8c49-110">AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="d8c49-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="d8c49-111">它也可用於在單一頁面上的多個 popup 控制項。</span><span class="sxs-lookup"><span data-stu-id="d8c49-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="d8c49-112">步驟</span><span class="sxs-lookup"><span data-stu-id="d8c49-112">Steps</span></span>

<span data-ttu-id="d8c49-113">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="d8c49-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="d8c49-114">接著，將會做為快顯視窗的面板。</span><span class="sxs-lookup"><span data-stu-id="d8c49-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="d8c49-115">在目前的案例中，包含面板`Calendar`控制項。</span><span class="sxs-lookup"><span data-stu-id="d8c49-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="d8c49-116">為了避免因行事曆的回傳的重新整理頁面，[面板] 中皆會置於`UpdatePanel`控制項：</span><span class="sxs-lookup"><span data-stu-id="d8c49-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="d8c49-117">這個頁面也包含兩個文字方塊。</span><span class="sxs-lookup"><span data-stu-id="d8c49-117">The page also contains two text boxes.</span></span> <span data-ttu-id="d8c49-118">針對每個文字方塊中，當啟動時在文字方塊中，應該會出現快顯行事曆。</span><span class="sxs-lookup"><span data-stu-id="d8c49-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="d8c49-119">現在擴充的兩個文字方塊與每個`PopupControlExtender`。</span><span class="sxs-lookup"><span data-stu-id="d8c49-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="d8c49-120">`TargetControlID`屬性提供的繫結至擴充項控制項的識別碼。</span><span class="sxs-lookup"><span data-stu-id="d8c49-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="d8c49-121">`PopupControlID`屬性包含快顯面板的識別碼。</span><span class="sxs-lookup"><span data-stu-id="d8c49-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="d8c49-122">在此情況下，這兩種 extender 顯示相同的面板中，但也是可行的不同的面板。</span><span class="sxs-lookup"><span data-stu-id="d8c49-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="d8c49-123">現在每當您按一下文字欄位中，就會出現日曆以下欄位，可讓您選取的日期。</span><span class="sxs-lookup"><span data-stu-id="d8c49-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="d8c49-124">（選取的日期取回在文字方塊中會涵蓋不同的教學課程中。）</span><span class="sxs-lookup"><span data-stu-id="d8c49-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="d8c49-125">[![當使用者按一下的文字方塊中，就會出現日曆](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d8c49-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="d8c49-126">當使用者按一下的文字方塊中，就會出現日曆 ([按一下以檢視完整大小的影像](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d8c49-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d8c49-127">下一步</span><span class="sxs-lookup"><span data-stu-id="d8c49-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
