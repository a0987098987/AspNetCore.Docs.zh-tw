---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: 以動態方式填入控制項 (VB) |Microsoft Docs
author: wenz
description: DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿至 t 的目標控制項...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cda816cab99867aac8770c420cab7a78ba699e4e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834277"
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="45dfd-103">以動態方式填入控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="45dfd-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="45dfd-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="45dfd-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="45dfd-105">[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="45dfd-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="45dfd-106">DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="45dfd-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="45dfd-107">總覽</span><span class="sxs-lookup"><span data-stu-id="45dfd-107">Overview</span></span>

<span data-ttu-id="45dfd-108">`DynamicPopulate`中 ASP.NET AJAX Control Toolkit 控制項呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="45dfd-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="45dfd-109">本教學課程會示範如何設定此項目。</span><span class="sxs-lookup"><span data-stu-id="45dfd-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="45dfd-110">步驟</span><span class="sxs-lookup"><span data-stu-id="45dfd-110">Steps</span></span>

<span data-ttu-id="45dfd-111">首先，您必須實作呼叫方法的 ASP.NET Web 服務`DynamicPopulate`。</span><span class="sxs-lookup"><span data-stu-id="45dfd-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="45dfd-112">Web 服務類別會要求`ScriptService`內定義的屬性`Microsoft.Web.Script.Services`; 否則為 ASP.NET AJAX 無法建立 web 服務接著所需的用戶端 JavaScript proxy `DynamicPopulate`。</span><span class="sxs-lookup"><span data-stu-id="45dfd-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="45dfd-113">Web 方法必須預期呼叫的型別字串的一個引數`contextKey`，因為`DynamicPopulate`控制項會傳送一段內容資訊與每個 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="45dfd-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="45dfd-114">下列 web 服務會傳回目前的日期格式，由`contextKey`引數：</span><span class="sxs-lookup"><span data-stu-id="45dfd-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="45dfd-115">Web 服務然後儲存為`DynamicPopulate.vb.asmx`。</span><span class="sxs-lookup"><span data-stu-id="45dfd-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="45dfd-116">或者，您可以實作`getDate()`方法作為頁面方法與實際的 ASP.NET 頁面內`DynamicPopulate`控制項。</span><span class="sxs-lookup"><span data-stu-id="45dfd-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="45dfd-117">在下一個步驟中，建立新的 ASP.NET 檔案。</span><span class="sxs-lookup"><span data-stu-id="45dfd-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="45dfd-118">如往常，第一個步驟是加入`ScriptManager`中目前的頁面，即可載入 ASP.NET AJAX 程式庫，並讓 Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="45dfd-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="45dfd-119">然後，新增一個 label 控制項 (例如使用 HTML 控制項的相同的名稱，或&lt; `asp:Label`  / &gt; web 控制項) 這稍後會顯示 web 服務呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="45dfd-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="45dfd-120">HTML 按鈕 （做為 HTML 控制項，因為我們不需要回傳至伺服器） 接著會用來觸發動態的母體擴展：</span><span class="sxs-lookup"><span data-stu-id="45dfd-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="45dfd-121">最後，我們需要`DynamicPopulateExtender`要連線的項目控制項。</span><span class="sxs-lookup"><span data-stu-id="45dfd-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="45dfd-122">會設定下列屬性 (明顯的除了`ID`並`runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="45dfd-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="45dfd-123">`TargetControlID` 要從 web 服務呼叫放置結果</span><span class="sxs-lookup"><span data-stu-id="45dfd-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="45dfd-124">`ServicePath` web 服務的路徑 （如果您想要使用頁面方法略過）</span><span class="sxs-lookup"><span data-stu-id="45dfd-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="45dfd-125">`ServiceMethod` web 方法或頁面方法的名稱</span><span class="sxs-lookup"><span data-stu-id="45dfd-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="45dfd-126">`ContextKey` 若要傳送至 web 服務的內容資訊</span><span class="sxs-lookup"><span data-stu-id="45dfd-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="45dfd-127">`PopulateTriggerControlID` 這會觸發 web 服務呼叫的項目</span><span class="sxs-lookup"><span data-stu-id="45dfd-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="45dfd-128">`ClearContentsDuringUpdate` 是否要在 web 服務呼叫期間清空目標項目</span><span class="sxs-lookup"><span data-stu-id="45dfd-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="45dfd-129">如您所見，控制項需要一些資訊，但將所有項目放入位置是相當簡單。</span><span class="sxs-lookup"><span data-stu-id="45dfd-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="45dfd-130">以下是標記`DynamicPopulateExtender`控制項中目前的案例：</span><span class="sxs-lookup"><span data-stu-id="45dfd-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="45dfd-131">瀏覽器中執行 ASP.NET 網頁，然後按一下按鈕，您會收到目前的日期，格式為月-日-年。</span><span class="sxs-lookup"><span data-stu-id="45dfd-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="45dfd-132">[![按一下按鈕從伺服器擷取的日期](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45dfd-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="45dfd-133">按一下按鈕從伺服器擷取的日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45dfd-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45dfd-134">[上一頁](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [下一頁](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="45dfd-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
