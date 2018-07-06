---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: 使用自動回傳與 CascadingDropDown (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f0ed4c88789504c09649254ea98a89b01bdd20c5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813837"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="c8dff-103">使用自動回傳與 CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="c8dff-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="c8dff-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c8dff-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c8dff-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c8dff-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="c8dff-106">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="c8dff-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c8dff-107">不過使用 CascadingDropDown 控制，ASP 時。NET 的 DropDownList 控制項的 AutoPostBack 功能無法運作，因為會以非同步方式將資料載入清單產生本身 （非必要） 回傳。</span><span class="sxs-lookup"><span data-stu-id="c8dff-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c8dff-108">使用一些 JavaScript 程式碼中，您可以避免這種效果。</span><span class="sxs-lookup"><span data-stu-id="c8dff-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="c8dff-109">總覽</span><span class="sxs-lookup"><span data-stu-id="c8dff-109">Overview</span></span>

<span data-ttu-id="c8dff-110">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="c8dff-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c8dff-111">（比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。不過使用 CascadingDropDown 控制，ASP 時。NET 的 DropDownList 控制項的 AutoPostBack 功能無法運作，因為會以非同步方式將資料載入清單產生本身 （非必要） 回傳。</span><span class="sxs-lookup"><span data-stu-id="c8dff-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c8dff-112">使用一些 JavaScript 程式碼中，您可以避免這種效果。</span><span class="sxs-lookup"><span data-stu-id="c8dff-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="c8dff-113">步驟</span><span class="sxs-lookup"><span data-stu-id="c8dff-113">Steps</span></span>

<span data-ttu-id="c8dff-114">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但內&lt; `form` &gt;項目):</span><span class="sxs-lookup"><span data-stu-id="c8dff-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="c8dff-115">DropDownList 控制項則需要：</span><span class="sxs-lookup"><span data-stu-id="c8dff-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="c8dff-116">此清單中，會加入 CascadingDropDown 擴充項，提供 web 服務 URL 和方法資訊：</span><span class="sxs-lookup"><span data-stu-id="c8dff-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="c8dff-117">CascadingDropDown extender 接著會以非同步方式呼叫 web 服務的下列方法簽章：</span><span class="sxs-lookup"><span data-stu-id="c8dff-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="c8dff-118">方法會傳回類型 CascadingDropDown 值的陣列。</span><span class="sxs-lookup"><span data-stu-id="c8dff-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="c8dff-119">類型的建構函式必須要有先清單項目的標題，然後此值 (HTML`value`屬性)。</span><span class="sxs-lookup"><span data-stu-id="c8dff-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="c8dff-120">載入瀏覽器頁面，將會填滿下拉式清單中的，使用三個廠商，第二個要預先選取。</span><span class="sxs-lookup"><span data-stu-id="c8dff-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="c8dff-121">此外，ASP.NET 會定義`__doPostBack()`JavaScript 方法。</span><span class="sxs-lookup"><span data-stu-id="c8dff-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="c8dff-122">頁面載入之後，此 JavaScript 呼叫將會加入下拉式清單中，但只能有項目中。</span><span class="sxs-lookup"><span data-stu-id="c8dff-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="c8dff-123">如果在清單中不有任何項目，Control Toolkit 目前正在載入它們，讓 JavaScript 程式碼會使用逾時和在半秒後重新嘗試。</span><span class="sxs-lookup"><span data-stu-id="c8dff-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="c8dff-124">如此一來，當清單中有實際的項目，而且使用者選取一個項目只執行回傳。</span><span class="sxs-lookup"><span data-stu-id="c8dff-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="c8dff-125">[![選取清單項目造成回傳](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c8dff-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="c8dff-126">選取清單項目造成回傳 ([按一下以檢視完整大小的影像](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c8dff-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8dff-127">上一步</span><span class="sxs-lookup"><span data-stu-id="c8dff-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
