---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: 從用戶端程式碼 (VB) 操作 DropShadow 屬性 |Microsoft Docs
author: wenz
description: DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。 此擴充項的屬性也可以使用用戶端 Javascript 來變更...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b9220eecca224c2b1e0f19c972c6c2a4dc9c7d35
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841405"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="2ed6b-104">從用戶端程式碼 (VB) 操作 DropShadow 屬性</span><span class="sxs-lookup"><span data-stu-id="2ed6b-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="2ed6b-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2ed6b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2ed6b-106">[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ed6b-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="2ed6b-107">DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="2ed6b-108">此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="2ed6b-109">總覽</span><span class="sxs-lookup"><span data-stu-id="2ed6b-109">Overview</span></span>

<span data-ttu-id="2ed6b-110">DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="2ed6b-111">此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2ed6b-112">步驟</span><span class="sxs-lookup"><span data-stu-id="2ed6b-112">Steps</span></span>

<span data-ttu-id="2ed6b-113">程式碼的開頭包含幾行文字的面板：</span><span class="sxs-lookup"><span data-stu-id="2ed6b-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="2ed6b-114">相關聯的 CSS 類別提供不錯的背景色彩的面板：</span><span class="sxs-lookup"><span data-stu-id="2ed6b-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="2ed6b-115">`DropShadowExtender`加入擴充具有延伸陰影效果，設定為 50%的不透明度的面板：</span><span class="sxs-lookup"><span data-stu-id="2ed6b-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="2ed6b-116">然後，ASP.NET AJAX`ScriptManager`控制項可讓控制項工具組運作：</span><span class="sxs-lookup"><span data-stu-id="2ed6b-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="2ed6b-117">另一個面板包含兩個 JavaScript 連結設定延伸陰影的不透明度： 減號的連結會減少陰影的不透明度，加上連結就會增加它。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="2ed6b-118">JavaScript 函式`changeOpacity()`則必須先找到`DropShadowExtender`頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="2ed6b-119">ASP.NET AJAX 定義`$find()`完全該工作的方法。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="2ed6b-120">然後，`get_Opacity()`方法會擷取目前的不透明度，`set_Opacity()`方法會設定它。</span><span class="sxs-lookup"><span data-stu-id="2ed6b-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="2ed6b-121">JavaScript 程式碼然後將目前的不透明度值放`<label>`項目：</span><span class="sxs-lookup"><span data-stu-id="2ed6b-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="2ed6b-122">[![在用戶端上的變更不透明度](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ed6b-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="2ed6b-123">在用戶端上的變更不透明度 ([按一下以檢視完整大小的影像](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2ed6b-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2ed6b-124">上一步</span><span class="sxs-lookup"><span data-stu-id="2ed6b-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
