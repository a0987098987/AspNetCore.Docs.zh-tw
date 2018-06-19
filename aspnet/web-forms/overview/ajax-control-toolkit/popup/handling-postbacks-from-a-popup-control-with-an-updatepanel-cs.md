---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: 處理來自 UpdatePanel (C#) 的快顯視窗控制項的回傳 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 特別注意，必須採取...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879630"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="28c33-104">處理回傳從快顯視窗控制項與 UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="28c33-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="28c33-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="28c33-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="28c33-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="28c33-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="28c33-107">AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="28c33-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="28c33-108">快顯內發生回傳時必須特別注意。</span><span class="sxs-lookup"><span data-stu-id="28c33-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="28c33-109">總覽</span><span class="sxs-lookup"><span data-stu-id="28c33-109">Overview</span></span>

<span data-ttu-id="28c33-110">AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="28c33-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="28c33-111">快顯內發生回傳時必須特別注意。</span><span class="sxs-lookup"><span data-stu-id="28c33-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="28c33-112">步驟</span><span class="sxs-lookup"><span data-stu-id="28c33-112">Steps</span></span>

<span data-ttu-id="28c33-113">當使用`PopupControl`回傳，`UpdatePanel`導致回傳所造成的頁面重新整理。</span><span class="sxs-lookup"><span data-stu-id="28c33-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="28c33-114">下列標記會定義幾個重要的項目：</span><span class="sxs-lookup"><span data-stu-id="28c33-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="28c33-115">A`ScriptManager`控制，好讓 ASP.NET AJAX Control Toolkit 的運作方式</span><span class="sxs-lookup"><span data-stu-id="28c33-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="28c33-116">兩個`TextBox`控制項，其會同時觸發快顯視窗</span><span class="sxs-lookup"><span data-stu-id="28c33-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="28c33-117">A`Panel`將做為快顯視窗的控制項</span><span class="sxs-lookup"><span data-stu-id="28c33-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="28c33-118">面板中`Calendar`控制項內嵌在`UpdatePanel`控制項</span><span class="sxs-lookup"><span data-stu-id="28c33-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="28c33-119">兩個`PopupControlExtender`指派 [面板] 中，在文字方塊中的控制項</span><span class="sxs-lookup"><span data-stu-id="28c33-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="28c33-120">請注意，`OnSelectionChanged`屬性`Calendar`控制項設定。</span><span class="sxs-lookup"><span data-stu-id="28c33-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="28c33-121">因此當使用者選取內行事曆日期，就會發生回傳和伺服器端方法`c1_SelectionChanged()`執行。</span><span class="sxs-lookup"><span data-stu-id="28c33-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="28c33-122">在該方法中，必須擷取並寫回至文字方塊中目前的日期。</span><span class="sxs-lookup"><span data-stu-id="28c33-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="28c33-123">語法，如下所示： 首先，proxy 物件`PopupControlExtender`頁面上，必須產生。</span><span class="sxs-lookup"><span data-stu-id="28c33-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="28c33-124">ASP.NET AJAX Control Toolkit 提供`GetProxyForCurrentPopup()`方法。</span><span class="sxs-lookup"><span data-stu-id="28c33-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="28c33-125">這個方法會傳回的物件支援`Commit()`方法可將值傳送至觸發的快顯視窗 （不是控制項觸發方法呼叫 ！） 的控制項。</span><span class="sxs-lookup"><span data-stu-id="28c33-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="28c33-126">下列程式碼提供做為引數的選取的日期`Commit()`方法，造成回寫至文字方塊中的所選的日期的程式碼：</span><span class="sxs-lookup"><span data-stu-id="28c33-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="28c33-127">現在每當您按一下在行事曆日期，將選取的日期會出現在相關聯的文字方塊中，建立日期選擇器控制項可以目前找到許多網站上。</span><span class="sxs-lookup"><span data-stu-id="28c33-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="28c33-128">[![當使用者按一下的文字方塊中，就會出現日曆](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="28c33-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="28c33-129">當使用者按一下的文字方塊中，就會出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="28c33-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="28c33-130">[![按一下日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="28c33-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="28c33-131">按一下日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="28c33-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28c33-132">[上一頁](using-multiple-popup-controls-cs.md)
> [下一頁](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="28c33-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
