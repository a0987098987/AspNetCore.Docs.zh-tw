---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: 操作 DropShadow 屬性，從用戶端程式碼 (C#) |Microsoft Docs
author: wenz
description: 自訂 DataList 的編輯介面
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: f751e2378621a6ab74f9f15fe51fd18bdd8b4070
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824717"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="fbab4-103">操作 DropShadow 屬性，從用戶端程式碼 (C#)</span><span class="sxs-lookup"><span data-stu-id="fbab4-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="fbab4-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fbab4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fbab4-105">[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fbab4-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="fbab4-106">DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="fbab4-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="fbab4-107">此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="fbab4-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="fbab4-108">總覽</span><span class="sxs-lookup"><span data-stu-id="fbab4-108">Overview</span></span>

<span data-ttu-id="fbab4-109">DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="fbab4-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="fbab4-110">此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="fbab4-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="fbab4-111">步驟</span><span class="sxs-lookup"><span data-stu-id="fbab4-111">Steps</span></span>

<span data-ttu-id="fbab4-112">程式碼的開頭包含幾行文字的面板：</span><span class="sxs-lookup"><span data-stu-id="fbab4-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="fbab4-113">相關聯的 CSS 類別提供不錯的背景色彩的面板：</span><span class="sxs-lookup"><span data-stu-id="fbab4-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="fbab4-114">`DropShadowExtender`加入擴充具有延伸陰影效果，設定為 50%的不透明度的面板：</span><span class="sxs-lookup"><span data-stu-id="fbab4-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="fbab4-115">然後，ASP.NET AJAX`ScriptManager`控制項可讓控制項工具組運作：</span><span class="sxs-lookup"><span data-stu-id="fbab4-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="fbab4-116">另一個面板包含兩個 JavaScript 連結設定延伸陰影的不透明度： 減號的連結會減少陰影的不透明度，加上連結就會增加它。</span><span class="sxs-lookup"><span data-stu-id="fbab4-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="fbab4-117">JavaScript 函式`changeOpacity()`則必須先找到`DropShadowExtender`頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="fbab4-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="fbab4-118">ASP.NET AJAX 定義`$find()`完全該工作的方法。</span><span class="sxs-lookup"><span data-stu-id="fbab4-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="fbab4-119">然後，`get_Opacity()`方法會擷取目前的不透明度，`set_Opacity()`方法會設定它。</span><span class="sxs-lookup"><span data-stu-id="fbab4-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="fbab4-120">JavaScript 程式碼然後將目前的不透明度值放`<label>`項目：</span><span class="sxs-lookup"><span data-stu-id="fbab4-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="fbab4-121">[![在用戶端上的變更不透明度](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fbab4-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="fbab4-122">在用戶端上的變更不透明度 ([按一下以檢視完整大小的影像](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fbab4-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fbab4-123">[上一頁](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [下一頁](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fbab4-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
