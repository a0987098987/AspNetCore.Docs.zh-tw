---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: 填滿清單使用 CascadingDropDown (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: ad716222335e8b6f2484953296cc263119718105
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834652"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="897c4-103">填滿清單使用 CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="897c4-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="897c4-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="897c4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="897c4-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="897c4-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="897c4-106">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="897c4-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="897c4-107">（比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。若要解決的第一個挑戰是實際填入下拉式清單中使用這個控制項。</span><span class="sxs-lookup"><span data-stu-id="897c4-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="897c4-108">總覽</span><span class="sxs-lookup"><span data-stu-id="897c4-108">Overview</span></span>

<span data-ttu-id="897c4-109">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="897c4-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="897c4-110">（比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。若要解決的第一個挑戰是實際填入下拉式清單中使用這個控制項。</span><span class="sxs-lookup"><span data-stu-id="897c4-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="897c4-111">步驟</span><span class="sxs-lookup"><span data-stu-id="897c4-111">Steps</span></span>

<span data-ttu-id="897c4-112">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="897c4-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="897c4-113">DropDownList 控制項則需要：</span><span class="sxs-lookup"><span data-stu-id="897c4-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="897c4-114">對於此清單中，就會加入 CascadingDropDown 擴充項。</span><span class="sxs-lookup"><span data-stu-id="897c4-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="897c4-115">非同步要求會傳送到一項 web 服務，接著會傳回一份要顯示在清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="897c4-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="897c4-116">針對此目的，需要設定下列 CascadingDropDown 屬性：</span><span class="sxs-lookup"><span data-stu-id="897c4-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="897c4-117">`ServicePath`: 提供的清單項目在 web 服務的 URL</span><span class="sxs-lookup"><span data-stu-id="897c4-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="897c4-118">`ServiceMethod`： 提供的清單項目 web 方法</span><span class="sxs-lookup"><span data-stu-id="897c4-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="897c4-119">`TargetControlID`: ID 的下拉式清單</span><span class="sxs-lookup"><span data-stu-id="897c4-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="897c4-120">`Category`： 提交給 web 方法的呼叫時類別目錄資訊</span><span class="sxs-lookup"><span data-stu-id="897c4-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="897c4-121">`PromptText`： 以非同步方式從伺服器載入清單資料時顯示的文字</span><span class="sxs-lookup"><span data-stu-id="897c4-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="897c4-122">以下是標記`CascadingDropDown`項目。</span><span class="sxs-lookup"><span data-stu-id="897c4-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="897c4-123">C# 和 VB 之間唯一的差別是相關聯的 web 服務的名稱：</span><span class="sxs-lookup"><span data-stu-id="897c4-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="897c4-124">JavaScript 程式碼來自`CascadingDropDown`擴充項會呼叫 web 服務方法具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="897c4-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="897c4-125">因此重要的是，此方法需要傳回類型的陣列`CascadingDropDownNameValue`（由 ASP.NET AJAX Control Toolkit 中定義）。</span><span class="sxs-lookup"><span data-stu-id="897c4-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="897c4-126">在 `CascadingDropDownNameValue`建構函式，第一個清單項目的文字，然後其值必須提供，就如同`<option value="VALUE">NAME</option>`在 HTML 中一樣。</span><span class="sxs-lookup"><span data-stu-id="897c4-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="897c4-127">以下是一些範例資料：</span><span class="sxs-lookup"><span data-stu-id="897c4-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="897c4-128">載入瀏覽器頁面，將會觸發要填入三個廠商的清單。</span><span class="sxs-lookup"><span data-stu-id="897c4-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="897c4-129">[![清單會自動填入](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="897c4-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="897c4-130">自動填滿清單 ([按一下以檢視完整大小的影像](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="897c4-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="897c4-131">[上一頁](using-auto-postback-with-cascadingdropdown-cs.md)
> [下一頁](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="897c4-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
