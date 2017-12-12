---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: "Presetting CascadingDropDown (C#) 的清單項目 |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d86c5887a8b175a60c32a0970482266b7d690162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="b4d1d-103">Presetting CascadingDropDown (C#) 的清單項目</span><span class="sxs-lookup"><span data-stu-id="b4d1d-103">Presetting List Entries with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="b4d1d-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b4d1d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b4d1d-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b4d1d-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="b4d1d-106">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b4d1d-107">利用最少的程式碼可能會以動態方式載入資料之後的清單項目已預先選取。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="b4d1d-108">概觀</span><span class="sxs-lookup"><span data-stu-id="b4d1d-108">Overview</span></span>

<span data-ttu-id="b4d1d-109">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b4d1d-110">（比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。利用最少的程式碼可能會以動態方式載入資料之後的清單項目已預先選取。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="b4d1d-111">步驟</span><span class="sxs-lookup"><span data-stu-id="b4d1d-111">Steps</span></span>

<span data-ttu-id="b4d1d-112">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="b4d1d-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="b4d1d-113">DropDownList 控制項則需要：</span><span class="sxs-lookup"><span data-stu-id="b4d1d-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="b4d1d-114">對於此清單中，就會加入 CascadingDropDown extender，提供 web 服務 URL 和方法資訊：</span><span class="sxs-lookup"><span data-stu-id="b4d1d-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="b4d1d-115">然後 CascadingDropDown extender 以非同步方式呼叫下列方法簽章的 web 服務：</span><span class="sxs-lookup"><span data-stu-id="b4d1d-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="b4d1d-116">方法會傳回類型 CascadingDropDown 值的陣列。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="b4d1d-117">清單項目的標題，然後此值的型別建構函式需要先 (HTML`value`屬性)。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="b4d1d-118">如果第三個引數設定為 true，清單項目會自動選取瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="b4d1d-119">載入網頁瀏覽器中的，將會填滿下拉式清單具有三位廠商，第二個正在預先選取。</span><span class="sxs-lookup"><span data-stu-id="b4d1d-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="b4d1d-120">[![清單會填入，且自動預先選取](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b4d1d-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="b4d1d-121">填入清單並預先選取自動 ([按一下以檢視完整大小的影像](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b4d1d-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b4d1d-122">[上一頁](using-cascadingdropdown-with-a-database-cs.md)
[下一頁](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b4d1d-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
