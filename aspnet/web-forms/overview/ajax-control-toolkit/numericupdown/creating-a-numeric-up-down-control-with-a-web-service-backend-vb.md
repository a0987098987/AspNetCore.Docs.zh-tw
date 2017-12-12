---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "建立數值上下按鈕控制項與 Web 服務後端 (VB) |Microsoft 文件"
author: wenz
description: "而不是讓使用者輸入的值將核取方塊，向上/向下的控制項 （Windows 和其他作業系統有） 的數值無法證明越多的 c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="89d13-103">建立數值向上/向下控制項與 Web 服務後端 (VB)</span><span class="sxs-lookup"><span data-stu-id="89d13-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="89d13-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="89d13-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="89d13-105">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="89d13-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="89d13-106">而不是讓使用者輸入的值將核取方塊，數值上下按鈕控制項 （也就存在於 Windows 和其他作業系統） 無法證明越多舒適。</span><span class="sxs-lookup"><span data-stu-id="89d13-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="89d13-107">根據預設，數值上下按鈕控制項一律會增加，或值，就會減 1，但 web 服務證明更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="89d13-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="89d13-108">概觀</span><span class="sxs-lookup"><span data-stu-id="89d13-108">Overview</span></span>

<span data-ttu-id="89d13-109">而不是讓使用者輸入的值將核取方塊，數值上下按鈕控制項 （也就存在於 Windows 和其他作業系統） 無法證明越多舒適。</span><span class="sxs-lookup"><span data-stu-id="89d13-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="89d13-110">根據預設，`NumericUpDown`控制項一律會增加，或值，就會減 1，但 web 服務證明更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="89d13-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="89d13-111">步驟</span><span class="sxs-lookup"><span data-stu-id="89d13-111">Steps</span></span>

<span data-ttu-id="89d13-112">ASP.NET AJAX Control Toolkit 包含`NumericUpDown`擴充項它會自動加入到文字方塊中的兩個按鈕： 一個用於增加其值用於減少它的其中一個。</span><span class="sxs-lookup"><span data-stu-id="89d13-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="89d13-113">不過此控制項也支援 web 服務呼叫 （或網頁方法呼叫）。</span><span class="sxs-lookup"><span data-stu-id="89d13-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="89d13-114">每當向上或向下按鈕按下時的 JavaScript 程式碼會連接到 web 伺服器和執行的方法那里。</span><span class="sxs-lookup"><span data-stu-id="89d13-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="89d13-115">方法簽章是下列其中一個：</span><span class="sxs-lookup"><span data-stu-id="89d13-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="89d13-116">`current`引數是目前的值，在文字方塊中;`tag`屬性是其他內容資料的可設定的屬性為`NumericUpDown`擴充項 （但非必要）。</span><span class="sxs-lookup"><span data-stu-id="89d13-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="89d13-117">此範例中，數值上下按鈕控制項都應該只允許的 2 乘冪的值： 1、 2、 4、 8、 16、 32、 64、 等等。</span><span class="sxs-lookup"><span data-stu-id="89d13-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="89d13-118">因此，當使用者想要增加的值時執行的方法必須 double 的舊值。其他方法必須由兩個分割的值。</span><span class="sxs-lookup"><span data-stu-id="89d13-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="89d13-119">因此，以下是完整的 web 服務：</span><span class="sxs-lookup"><span data-stu-id="89d13-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="89d13-120">最後，建立新的 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="89d13-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="89d13-121">像往常一樣，您需要`ScriptManager`控制項，`TextBox`控制項和`NumericUpDownExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="89d13-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="89d13-122">對於後者，您必須提供網頁服務資訊：</span><span class="sxs-lookup"><span data-stu-id="89d13-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="89d13-123">`ServiceDownMethod`web 方法或頁面上的方法名稱清單</span><span class="sxs-lookup"><span data-stu-id="89d13-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="89d13-124">`ServiceDownPath`web 服務的向下服務方法的路徑如果您使用頁面方法，省略</span><span class="sxs-lookup"><span data-stu-id="89d13-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="89d13-125">`ServiceUpMethod`名稱的總 web 方法或網頁方法</span><span class="sxs-lookup"><span data-stu-id="89d13-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="89d13-126">`ServiceUpPath`路徑 web 服務的最新的服務方法。如果您使用頁面方法，省略</span><span class="sxs-lookup"><span data-stu-id="89d13-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="89d13-127">以下是完整的標記頁面：</span><span class="sxs-lookup"><span data-stu-id="89d13-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="89d13-128">如果您執行頁面，請注意如何在文字方塊中的值一律會加倍當您按一下上方的按鈕，然後當您按一下下方的按鈕會變成一半。</span><span class="sxs-lookup"><span data-stu-id="89d13-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="89d13-129">[![顯示只是 2 的乘冪的數字](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="89d13-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="89d13-130">顯示只是 2 的乘冪的數字 ([按一下以檢視完整大小的影像](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="89d13-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="89d13-131">上一步</span><span class="sxs-lookup"><span data-stu-id="89d13-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
