---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Cascadingdropdown 預設清單項目與 CascadingDropDown (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5133516311478d0a4faab45721c6b1d0a251b4b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817748"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="e0605-103">Cascadingdropdown 預設清單項目與 CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="e0605-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="e0605-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e0605-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e0605-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e0605-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="e0605-106">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="e0605-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e0605-107">加上一些程式碼就可以，以動態方式載入資料之後，已預先選取的清單項目。</span><span class="sxs-lookup"><span data-stu-id="e0605-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="e0605-108">總覽</span><span class="sxs-lookup"><span data-stu-id="e0605-108">Overview</span></span>

<span data-ttu-id="e0605-109">在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="e0605-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e0605-110">（比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。加上一些程式碼就可以，以動態方式載入資料之後，已預先選取的清單項目。</span><span class="sxs-lookup"><span data-stu-id="e0605-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="e0605-111">步驟</span><span class="sxs-lookup"><span data-stu-id="e0605-111">Steps</span></span>

<span data-ttu-id="e0605-112">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="e0605-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="e0605-113">DropDownList 控制項則需要：</span><span class="sxs-lookup"><span data-stu-id="e0605-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="e0605-114">此清單中，會加入 CascadingDropDown 擴充項，提供 web 服務 URL 和方法資訊：</span><span class="sxs-lookup"><span data-stu-id="e0605-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="e0605-115">CascadingDropDown extender 接著會以非同步方式呼叫 web 服務的下列方法簽章：</span><span class="sxs-lookup"><span data-stu-id="e0605-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="e0605-116">方法會傳回類型 CascadingDropDown 值的陣列。</span><span class="sxs-lookup"><span data-stu-id="e0605-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="e0605-117">類型的建構函式必須要有先清單項目的標題，然後此值 (HTML`value`屬性)。</span><span class="sxs-lookup"><span data-stu-id="e0605-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="e0605-118">如果第三個引數設定為 true，清單項目會自動選取瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="e0605-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="e0605-119">載入瀏覽器頁面，將會填滿下拉式清單中的，使用三個廠商，第二個要預先選取。</span><span class="sxs-lookup"><span data-stu-id="e0605-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="e0605-120">[![清單會填滿，且預設會自動選取](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e0605-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="e0605-121">清單會填入，且會自動預先選取 ([按一下以檢視完整大小的影像](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e0605-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0605-122">[上一頁](using-cascadingdropdown-with-a-database-vb.md)
> [下一頁](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e0605-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
