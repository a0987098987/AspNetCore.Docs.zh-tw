---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: 處理有 Updatepanel (VB) 的快顯視窗控制項回傳 |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 特別注意有進行中...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: cf17025219fbf24e749c172f2ddb47ec20980cdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395898"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="0cf1b-104">處理有 Updatepanel (VB) 的快顯視窗控制項回傳</span><span class="sxs-lookup"><span data-stu-id="0cf1b-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="0cf1b-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0cf1b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0cf1b-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0cf1b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="0cf1b-107">PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0cf1b-108">特別注意有進行這類快顯視窗內回傳時發生。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="0cf1b-109">總覽</span><span class="sxs-lookup"><span data-stu-id="0cf1b-109">Overview</span></span>

<span data-ttu-id="0cf1b-110">PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0cf1b-111">特別注意有進行這類快顯視窗內回傳時發生。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="0cf1b-112">步驟</span><span class="sxs-lookup"><span data-stu-id="0cf1b-112">Steps</span></span>

<span data-ttu-id="0cf1b-113">使用時`PopupControl`回傳時，使用`UpdatePanel`導致回傳所造成的重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="0cf1b-114">下列標記會定義幾個重要的項目：</span><span class="sxs-lookup"><span data-stu-id="0cf1b-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="0cf1b-115">A`ScriptManager`控制項，讓 ASP.NET AJAX Control Toolkit 的運作方式</span><span class="sxs-lookup"><span data-stu-id="0cf1b-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="0cf1b-116">兩個`TextBox`控制項，其會同時觸發快顯視窗</span><span class="sxs-lookup"><span data-stu-id="0cf1b-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="0cf1b-117">A`Panel`將做為快顯視窗的控制項</span><span class="sxs-lookup"><span data-stu-id="0cf1b-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="0cf1b-118">在面板中，`Calendar`控制項內嵌在`UpdatePanel`控制項</span><span class="sxs-lookup"><span data-stu-id="0cf1b-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="0cf1b-119">兩個`PopupControlExtender`將面板指派的文字方塊內的控制項</span><span class="sxs-lookup"><span data-stu-id="0cf1b-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="0cf1b-120">請注意，`OnSelectionChanged`屬性的`Calendar`控制項設定。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="0cf1b-121">因此當使用者選取內行事曆的日期，就會發生回傳和伺服器端方法`c1_SelectionChanged()`執行。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="0cf1b-122">在該方法中，目前的日期必須擷取和回寫至文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="0cf1b-123">該語法如下所示： 首先，proxy 物件`PopupControlExtender`必須產生頁面上。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="0cf1b-124">ASP.NET AJAX Control Toolkit 提供`GetProxyForCurrentPopup()`方法。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="0cf1b-125">這個方法會傳回的物件支援`Commit()`方法可將值傳送回控制項觸發快顯視窗 （不是控制項觸發方法呼叫 ！）。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="0cf1b-126">下列程式碼提供做為引數所選的日期`Commit()`方法，導致文字方塊中回寫所選的日期的程式碼：</span><span class="sxs-lookup"><span data-stu-id="0cf1b-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="0cf1b-127">現在每當您在行事曆日期，按一下選取的日期會出現在相關聯的文字 方塊中，建立日期選擇器控制項目前可在許多網站上。</span><span class="sxs-lookup"><span data-stu-id="0cf1b-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="0cf1b-128">[![當使用者按一下文字方塊，即出現日曆](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0cf1b-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="0cf1b-129">當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0cf1b-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="0cf1b-130">[![按一下某個日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0cf1b-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="0cf1b-131">按一下某個日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0cf1b-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0cf1b-132">[上一頁](using-multiple-popup-controls-vb.md)
> [下一頁](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0cf1b-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
