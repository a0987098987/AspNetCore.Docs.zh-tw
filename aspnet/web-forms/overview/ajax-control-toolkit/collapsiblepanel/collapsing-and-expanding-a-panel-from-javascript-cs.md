---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: 摺疊與展開面板從 JavaScript (C#) |Microsoft Docs
author: wenz
description: CollapsiblePanel 控制項在 ASP.NET AJAX Control Toolkit 擴充一個面板，並提供它摺疊其內容，並將其展開的能力...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 85a9c6e6cc56cc563eefea6bc863950ba71149ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402752"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="2dd3e-103">摺疊與展開面板從 JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="2dd3e-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>
====================
<span data-ttu-id="2dd3e-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2dd3e-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2dd3e-105">[下載程式碼](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2dd3e-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="2dd3e-106">CollapsiblePanel 控制項在 ASP.NET AJAX Control Toolkit 擴充面板，並提供它的功能，可將摺疊其內容，並再次將其展開。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="2dd3e-107">從自訂的 JavaScript 程式碼，也會觸發這兩個動作。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="2dd3e-108">總覽</span><span class="sxs-lookup"><span data-stu-id="2dd3e-108">Overview</span></span>

<span data-ttu-id="2dd3e-109">CollapsiblePanel 控制項在 ASP.NET AJAX Control Toolkit 擴充面板，並提供它的功能，可將摺疊其內容，並再次將其展開。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="2dd3e-110">從自訂的 JavaScript 程式碼，也會觸發這兩個動作。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2dd3e-111">步驟</span><span class="sxs-lookup"><span data-stu-id="2dd3e-111">Steps</span></span>

<span data-ttu-id="2dd3e-112">首先，建立新的 ASP.NET 網頁，並包含`ScriptManager`檔案內`<form>`項目。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="2dd3e-113">這會載入控制項工具組所需的 ASP.NET AJAX 程式庫：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="2dd3e-114">如此可看到的展開/摺疊的效果，接著，建立內含一些文字面板：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="2dd3e-115">如您所見，面板會參照 CSS 類別如下所示 （而且基本上會定義的背景色彩和面板的寬度）：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="2dd3e-116">`CollapsiblePanelExtender`控制項需要`TargetControlID`屬性，讓此工具組知道摺疊或展開收到要求時的面板：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="2dd3e-117">不幸的是，擴充項目前不會公開特定 API 的摺疊或展開面板中，但某些未記載的方法會執行。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="2dd3e-118">首先，將三個 HTML 按鈕加入頁面，其會接著觸發用戶端 JavaScript 來摺疊或展開面板的內容：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="2dd3e-119">在用戶端 JavaScript 程式碼 (開始使用`<script type="text/javascript">`)，則`$find()`方法必須用來存取`CollapsiblePanelExtender`。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="2dd3e-120">`$find("cpe")` 會傳回它的參考。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="2dd3e-121">從該處開始，特定的方法將會解決手邊的工作。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="2dd3e-122">此方法為開啟 （展開） [面板] 中稱為`_doOpen()`; 下列程式碼會實作`doOpen()`按一下第一個按鈕時呼叫的函式：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="2dd3e-123">關閉，或摺疊面板，`_doClose()`方法需要執行。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="2dd3e-124">因此當使用者按一下第二個按鈕上時，會呼叫下列 JavaScript 程式碼：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="2dd3e-125">第三個按鈕切換面板的狀態： 從摺疊成展開，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="2dd3e-126">`CollapsiblePanelExtender`公開`toggle()`方法，這個方法： 反轉面板的狀態。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="2dd3e-127">不過另外還有另一種方法 (這會在內部使用`toggle()`方法):`get_Collapsed()`方法`CollapsiblePanelExtender()`告訴我們是否或不摺疊面板。</span><span class="sxs-lookup"><span data-stu-id="2dd3e-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="2dd3e-128">根據此函式的傳回值，[面板] 中則是展開 (`_doOpen()`方法) 或摺疊 (`_doClose()`) 方法：</span><span class="sxs-lookup"><span data-stu-id="2dd3e-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


<span data-ttu-id="2dd3e-129">[![第三個按鈕變更面板的狀態： 從摺疊以展開和後端](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2dd3e-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="2dd3e-130">第三個按鈕變更面板的狀態： 從摺疊以展開和後端 ([按一下以檢視完整大小的影像](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2dd3e-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2dd3e-131">下一步</span><span class="sxs-lookup"><span data-stu-id="2dd3e-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
