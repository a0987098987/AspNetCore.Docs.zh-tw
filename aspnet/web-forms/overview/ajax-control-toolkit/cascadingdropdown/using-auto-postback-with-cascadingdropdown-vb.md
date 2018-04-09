---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: 使用自動回傳 CascadingDropDown (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="4d7a8-103">使用自動回傳 CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="4d7a8-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="4d7a8-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4d7a8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4d7a8-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4d7a8-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="4d7a8-106">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="4d7a8-107">不過使用 ASP CascadingDropDown 控制項時。網路的 DropDownList 控制項的 AutoPostBack 功能無法運作，因為會以非同步方式將資料載入清單產生的 （非必要） 的回傳本身。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="4d7a8-108">一些 JavaScript 程式碼中，您可以避免這種效果。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="4d7a8-109">總覽</span><span class="sxs-lookup"><span data-stu-id="4d7a8-109">Overview</span></span>

<span data-ttu-id="4d7a8-110">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="4d7a8-111">（比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。不過使用 ASP CascadingDropDown 控制項時。網路的 DropDownList 控制項的 AutoPostBack 功能無法運作，因為會以非同步方式將資料載入清單產生的 （非必要） 的回傳本身。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="4d7a8-112">一些 JavaScript 程式碼中，您可以避免這種效果。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="4d7a8-113">步驟</span><span class="sxs-lookup"><span data-stu-id="4d7a8-113">Steps</span></span>

<span data-ttu-id="4d7a8-114">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部&lt; `form` &gt;項目):</span><span class="sxs-lookup"><span data-stu-id="4d7a8-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="4d7a8-115">DropDownList 控制項則需要：</span><span class="sxs-lookup"><span data-stu-id="4d7a8-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="4d7a8-116">對於此清單中，就會加入 CascadingDropDown extender，提供 web 服務 URL 和方法資訊：</span><span class="sxs-lookup"><span data-stu-id="4d7a8-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="4d7a8-117">然後 CascadingDropDown extender 以非同步方式呼叫下列方法簽章的 web 服務：</span><span class="sxs-lookup"><span data-stu-id="4d7a8-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="4d7a8-118">方法會傳回類型 CascadingDropDown 值的陣列。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="4d7a8-119">清單項目的標題，然後此值的型別建構函式需要先 (HTML`value`屬性)。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="4d7a8-120">載入網頁瀏覽器中的，將會填滿下拉式清單具有三位廠商，第二個正在預先選取。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="4d7a8-121">此外，ASP.NET 定義`__doPostBack()`JavaScript 方法。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="4d7a8-122">一經頁面已載入，下拉式清單中，但只有中是否有項目就會加入這個 JavaScript 呼叫。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="4d7a8-123">如果在清單中沒有任何項目，控制工具組目前正在載入它們，讓 JavaScript 程式碼會使用逾時和半秒一次嘗試。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="4d7a8-124">如此一來，回傳才會執行實際的項目在清單中，使用者選取項目。</span><span class="sxs-lookup"><span data-stu-id="4d7a8-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="4d7a8-125">[![選取的清單項目會回傳](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4d7a8-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="4d7a8-126">選取的清單項目會回傳 ([按一下以檢視完整大小的影像](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4d7a8-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4d7a8-127">上一步</span><span class="sxs-lookup"><span data-stu-id="4d7a8-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
