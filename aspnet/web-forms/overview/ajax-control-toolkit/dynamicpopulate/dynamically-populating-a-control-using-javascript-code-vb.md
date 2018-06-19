---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: 以動態方式填入控制項，使用 JavaScript 程式碼 (VB) |Microsoft 文件
author: wenz
description: 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標上的控制項 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869981"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="e541d-103">以動態方式填入控制項，使用 JavaScript 程式碼 (VB)</span><span class="sxs-lookup"><span data-stu-id="e541d-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="e541d-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e541d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e541d-105">[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e541d-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="e541d-106">在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標控制項在頁面上，如果沒有頁面重新整理。</span><span class="sxs-lookup"><span data-stu-id="e541d-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="e541d-107">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="e541d-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="e541d-108">總覽</span><span class="sxs-lookup"><span data-stu-id="e541d-108">Overview</span></span>

<span data-ttu-id="e541d-109">`DynamicPopulate` ASP.NET AJAX Control Toolkit 中的控制呼叫 web 服務 （或頁面的方法），並填入目標控制項在頁面上，如果沒有頁面重新整理所產生的值。</span><span class="sxs-lookup"><span data-stu-id="e541d-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="e541d-110">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="e541d-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="e541d-111">步驟</span><span class="sxs-lookup"><span data-stu-id="e541d-111">Steps</span></span>

<span data-ttu-id="e541d-112">首先，您需要 ASP.NET Web 服務會實作由呼叫方法`DynamicPopulateExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="e541d-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="e541d-113">Web 服務實作的方法`getDate()`，必須要有一個引數之字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一個片段的內容資訊與每個 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="e541d-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="e541d-114">下列程式碼 (檔`DynamicPopulate.vb.asmx`) 抓取目前的日期，三種格式之一：</span><span class="sxs-lookup"><span data-stu-id="e541d-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="e541d-115">在下一個步驟中，建立新的 ASP.NET 網站，並開始使用 ASP.NET AJAX ScriptManager 控制項：</span><span class="sxs-lookup"><span data-stu-id="e541d-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="e541d-116">然後，將標籤控制項 (例如使用 HTML 控制項的名稱相同，或`<asp:Label />`web 控制項) 的稍後將會顯示 web 服務呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="e541d-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="e541d-117">接下來，包括`DynamicPopulateExtender`控制，並提供網頁服務資訊、 目標控制項，但不是會觸發這將在稍後完成使用自訂 JavaScript 的母體擴展的控制項名稱 ！</span><span class="sxs-lookup"><span data-stu-id="e541d-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="e541d-118">現在給的 JavaScript 部分。</span><span class="sxs-lookup"><span data-stu-id="e541d-118">Now to the JavaScript part.</span></span> <span data-ttu-id="e541d-119">`$find()`函式，由 ASP.NET AJAX 程式庫中，將參考傳回給伺服器端物件的 ASP.NET AJAX Control Toolkit 例如`DynamicPopulateExtender`。</span><span class="sxs-lookup"><span data-stu-id="e541d-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="e541d-120">在目前的檔案中，`$find("dpe")`傳回一個參考`DynamicPopulateExtender`網頁中的控制項。</span><span class="sxs-lookup"><span data-stu-id="e541d-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="e541d-121">它會公開方法，稱為`populate()`此觸發程序動態擴展處理程序。</span><span class="sxs-lookup"><span data-stu-id="e541d-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="e541d-122">`populate()`方法需要一個引數： 將做為引數的內容金鑰`getDate()`web 方法。</span><span class="sxs-lookup"><span data-stu-id="e541d-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="e541d-123">例如，`$find("dpe").populate("format1")`會填入與目前的日期，格式為月-日-年的標籤。</span><span class="sxs-lookup"><span data-stu-id="e541d-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="e541d-124">為了讓範例更有彈性，使用者現在可以選擇數個日期格式之間。</span><span class="sxs-lookup"><span data-stu-id="e541d-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="e541d-125">針對其中每個選項按鈕隨即出現。</span><span class="sxs-lookup"><span data-stu-id="e541d-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="e541d-126">一次使用者按一下選項按鈕，JavaScript 程式碼動態填入的標籤與選取的日期格式。</span><span class="sxs-lookup"><span data-stu-id="e541d-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="e541d-127">以下是這些選項按鈕：</span><span class="sxs-lookup"><span data-stu-id="e541d-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="e541d-128">請注意，選項按鈕，JavaScript 運算式的內容中`this.value`目前按鈕，這正好是資訊完全相同的值是指`getDate()`方法可以使用。</span><span class="sxs-lookup"><span data-stu-id="e541d-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="e541d-129">[![按一下按鈕從伺服器中所指定的格式擷取日期](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e541d-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="e541d-130">按一下按鈕從伺服器中所指定的格式擷取日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e541d-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e541d-131">[上一頁](dynamically-populating-a-control-vb.md)
> [下一頁](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e541d-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
