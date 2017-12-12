---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: "以動態方式填入控制項 (VB) |Microsoft 文件"
author: wenz
description: "在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標上的控制項 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ec0b6d429f3eb4a7243201c2a91adde462cf6345
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="dfc70-103">以動態方式填入控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="dfc70-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="dfc70-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dfc70-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dfc70-105">[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dfc70-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="dfc70-106">在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標控制項在頁面上，如果沒有頁面重新整理。</span><span class="sxs-lookup"><span data-stu-id="dfc70-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="dfc70-107">概觀</span><span class="sxs-lookup"><span data-stu-id="dfc70-107">Overview</span></span>

<span data-ttu-id="dfc70-108">`DynamicPopulate` ASP.NET AJAX Control Toolkit 中的控制呼叫 web 服務 （或頁面的方法），並填入目標控制項在頁面上，如果沒有頁面重新整理所產生的值。</span><span class="sxs-lookup"><span data-stu-id="dfc70-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="dfc70-109">本教學課程會示範如何設定此項目。</span><span class="sxs-lookup"><span data-stu-id="dfc70-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="dfc70-110">步驟</span><span class="sxs-lookup"><span data-stu-id="dfc70-110">Steps</span></span>

<span data-ttu-id="dfc70-111">首先，您需要 ASP.NET Web 服務會實作由呼叫方法`DynamicPopulate`。</span><span class="sxs-lookup"><span data-stu-id="dfc70-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="dfc70-112">Web 服務類別會要求`ScriptService`屬性內定義`Microsoft.Web.Script.Services`; 否則 ASP.NET AJAX 無法建立依次所需的 web 服務的用戶端 JavaScript proxy `DynamicPopulate`。</span><span class="sxs-lookup"><span data-stu-id="dfc70-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="dfc70-113">Web 方法必須接受一個引數之字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一個片段的內容資訊與每個 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="dfc70-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="dfc70-114">下列 web 服務所表示的格式傳回目前日期`contextKey`引數：</span><span class="sxs-lookup"><span data-stu-id="dfc70-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="dfc70-115">Web 服務然後儲存為`DynamicPopulate.vb.asmx`。</span><span class="sxs-lookup"><span data-stu-id="dfc70-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="dfc70-116">或者，您可以實作`getDate()`方法做為頁面方法，與實際的 ASP.NET 頁面內`DynamicPopulate`控制項。</span><span class="sxs-lookup"><span data-stu-id="dfc70-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="dfc70-117">在下一個步驟中，建立新的 ASP.NET 檔案。</span><span class="sxs-lookup"><span data-stu-id="dfc70-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="dfc70-118">因為一律，第一個步驟是要包含`ScriptManager`中目前的頁面載入 ASP.NET AJAX 程式庫，並讓 Control Toolkit 工作：</span><span class="sxs-lookup"><span data-stu-id="dfc70-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="dfc70-119">然後，將標籤控制項 (例如使用 HTML 控制項的名稱相同，或&lt; `asp:Label`  / &gt; web 控制項) 的稍後將會顯示 web 服務呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="dfc70-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="dfc70-120">（如 HTML 控制項，因為我們不需要向伺服器回傳） HTML 按鈕然後將用來觸發動態母體擴展：</span><span class="sxs-lookup"><span data-stu-id="dfc70-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="dfc70-121">最後，我們需要`DynamicPopulateExtender`網路進行的控制項。</span><span class="sxs-lookup"><span data-stu-id="dfc70-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="dfc70-122">將設定下列屬性 (除了明顯的`ID`和`runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="dfc70-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="dfc70-123">`TargetControlID`要放置從 web 服務呼叫的結果</span><span class="sxs-lookup"><span data-stu-id="dfc70-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="dfc70-124">`ServicePath`web 服務的路徑 （如果您想要使用的頁面方法省略）</span><span class="sxs-lookup"><span data-stu-id="dfc70-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="dfc70-125">`ServiceMethod`web 方法或頁面的方法名稱</span><span class="sxs-lookup"><span data-stu-id="dfc70-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="dfc70-126">`ContextKey`內容資訊傳送至 web 服務</span><span class="sxs-lookup"><span data-stu-id="dfc70-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="dfc70-127">`PopulateTriggerControlID`web 服務呼叫觸發程序項目</span><span class="sxs-lookup"><span data-stu-id="dfc70-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="dfc70-128">`ClearContentsDuringUpdate`是否要在 web 服務呼叫期間清空目標項目</span><span class="sxs-lookup"><span data-stu-id="dfc70-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="dfc70-129">如您所見，控制項需要一些資訊，但將所有資料放入定位，並相當簡單。</span><span class="sxs-lookup"><span data-stu-id="dfc70-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="dfc70-130">以下是標記`DynamicPopulateExtender`控制項中目前的狀況：</span><span class="sxs-lookup"><span data-stu-id="dfc70-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="dfc70-131">在瀏覽器中執行的 ASP.NET 網頁，然後按一下按鈕。您會收到目前的日期，格式為月-日-年。</span><span class="sxs-lookup"><span data-stu-id="dfc70-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="dfc70-132">[![按一下按鈕從伺服器擷取日期](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dfc70-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="dfc70-133">按一下按鈕從伺服器擷取日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dfc70-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dfc70-134">[上一頁](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[下一頁](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dfc70-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
