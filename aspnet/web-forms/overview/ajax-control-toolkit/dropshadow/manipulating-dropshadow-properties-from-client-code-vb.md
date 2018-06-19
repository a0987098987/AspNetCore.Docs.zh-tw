---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: 操作 DropShadow 屬性，用戶端程式碼 (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 此擴充項的屬性也可以使用用戶端 JavaScrip 來變更...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870904"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="73f55-104">操作 DropShadow 屬性，用戶端程式碼 (VB)</span><span class="sxs-lookup"><span data-stu-id="73f55-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="73f55-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="73f55-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="73f55-106">[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="73f55-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="73f55-107">AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="73f55-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="73f55-108">此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="73f55-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="73f55-109">總覽</span><span class="sxs-lookup"><span data-stu-id="73f55-109">Overview</span></span>

<span data-ttu-id="73f55-110">AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。</span><span class="sxs-lookup"><span data-stu-id="73f55-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="73f55-111">此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。</span><span class="sxs-lookup"><span data-stu-id="73f55-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="73f55-112">步驟</span><span class="sxs-lookup"><span data-stu-id="73f55-112">Steps</span></span>

<span data-ttu-id="73f55-113">程式碼開頭包含幾行文字的面板：</span><span class="sxs-lookup"><span data-stu-id="73f55-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="73f55-114">相關聯的 CSS 類別可讓面板不錯的背景色彩：</span><span class="sxs-lookup"><span data-stu-id="73f55-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="73f55-115">`DropShadowExtender`加入至擴充下拉式陰影效果，而不透明度設定為 50%的面板：</span><span class="sxs-lookup"><span data-stu-id="73f55-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="73f55-116">然後，ASP.NET AJAX`ScriptManager`控制項可讓控制項在工作的工具組：</span><span class="sxs-lookup"><span data-stu-id="73f55-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="73f55-117">另一個面板包含兩個設定陰影的不透明度的 JavaScript 連結： 減號連結則會減少陰影的不透明度，加上的連結就會增加它。</span><span class="sxs-lookup"><span data-stu-id="73f55-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="73f55-118">JavaScript 函式`changeOpacity()`然後必須先找到`DropShadowExtender`頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="73f55-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="73f55-119">定義 ASP.NET AJAX`$find()`完全該工作的方法。</span><span class="sxs-lookup"><span data-stu-id="73f55-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="73f55-120">然後，`get_Opacity()`方法會擷取目前的不透明度，`set_Opacity()`方法會設定它。</span><span class="sxs-lookup"><span data-stu-id="73f55-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="73f55-121">JavaScript 程式碼再將目前的不透明度值放`<label>`項目：</span><span class="sxs-lookup"><span data-stu-id="73f55-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="73f55-122">[![在用戶端上變更不透明度](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="73f55-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="73f55-123">在用戶端上變更不透明度 ([按一下以檢視完整大小的影像](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="73f55-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="73f55-124">上一步</span><span class="sxs-lookup"><span data-stu-id="73f55-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
