---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: 以動態方式填入控制項，使用 JavaScript 程式碼 (VB) |Microsoft Docs
author: wenz
description: DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿至 t 的目標控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9df2a66bd49ecba52b0dd8b1d52a65b36c38a5dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366844"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="78a27-103">以動態方式填入控制項，使用 JavaScript 程式碼 (VB)</span><span class="sxs-lookup"><span data-stu-id="78a27-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="78a27-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="78a27-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="78a27-105">[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="78a27-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="78a27-106">DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="78a27-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="78a27-107">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="78a27-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="78a27-108">總覽</span><span class="sxs-lookup"><span data-stu-id="78a27-108">Overview</span></span>

<span data-ttu-id="78a27-109">`DynamicPopulate`中 ASP.NET AJAX Control Toolkit 控制項呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="78a27-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="78a27-110">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="78a27-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="78a27-111">步驟</span><span class="sxs-lookup"><span data-stu-id="78a27-111">Steps</span></span>

<span data-ttu-id="78a27-112">首先，您必須實作呼叫方法的 ASP.NET Web 服務`DynamicPopulateExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="78a27-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="78a27-113">Web 服務實作的方法`getDate()`它需要一個引數的字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一段內容資訊與每個 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="78a27-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="78a27-114">以下是程式碼 (檔案`DynamicPopulate.vb.asmx`) 表示擷取目前的日期，三種格式之一：</span><span class="sxs-lookup"><span data-stu-id="78a27-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="78a27-115">在下一個步驟中，建立新的 ASP.NET 網站，並開始使用 ASP.NET AJAX ScriptManager 控制項：</span><span class="sxs-lookup"><span data-stu-id="78a27-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="78a27-116">然後，新增一個 label 控制項 (例如使用 HTML 控制項的相同的名稱，或`<asp:Label />`web 控制項) 這稍後會顯示 web 服務呼叫的結果。</span><span class="sxs-lookup"><span data-stu-id="78a27-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="78a27-117">接下來，包括`DynamicPopulateExtender`控制，並提供 web 服務的資訊，目標控制項，但不是會觸發這會在稍後完成使用自訂 JavaScript 的母體擴展的控制項名稱 ！</span><span class="sxs-lookup"><span data-stu-id="78a27-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="78a27-118">現在給的 JavaScript 部分。</span><span class="sxs-lookup"><span data-stu-id="78a27-118">Now to the JavaScript part.</span></span> <span data-ttu-id="78a27-119">`$find()` ASP.NET AJAX 程式庫所定義的函數傳回的 ASP.NET AJAX Control Toolkit 中的伺服器端物件的參考例如`DynamicPopulateExtender`。</span><span class="sxs-lookup"><span data-stu-id="78a27-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="78a27-120">在目前的檔案中，`$find("dpe")`傳回一個參考`DynamicPopulateExtender`頁面中的控制項。</span><span class="sxs-lookup"><span data-stu-id="78a27-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="78a27-121">它會公開一個方法，叫做`populate()`哪些觸發程序動態擴展處理程序。</span><span class="sxs-lookup"><span data-stu-id="78a27-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="78a27-122">`populate()`方法需要一個引數： 這將做為引數的內容金鑰`getDate()`web 方法。</span><span class="sxs-lookup"><span data-stu-id="78a27-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="78a27-123">比方說，`$find("dpe").populate("format1")`會填入與目前的日期，格式為月-日-年的標籤。</span><span class="sxs-lookup"><span data-stu-id="78a27-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="78a27-124">為了讓範例更有彈性，使用者現在可能會選擇數種日期格式。</span><span class="sxs-lookup"><span data-stu-id="78a27-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="78a27-125">針對其中每個顯示的選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="78a27-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="78a27-126">一次使用者按下選項按鈕，JavaScript 程式碼以動態方式填入的標籤與選取的日期格式。</span><span class="sxs-lookup"><span data-stu-id="78a27-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="78a27-127">以下是這些選項按鈕：</span><span class="sxs-lookup"><span data-stu-id="78a27-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="78a27-128">請注意，內容中的選項按鈕，JavaScript 運算式`this.value`剛好是完全相同的資訊 [目前] 按鈕的值是指`getDate()`方法可以使用。</span><span class="sxs-lookup"><span data-stu-id="78a27-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="78a27-129">[![按一下按鈕從伺服器中所指定的格式擷取的日期](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="78a27-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="78a27-130">按一下按鈕從伺服器中所指定的格式擷取的日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="78a27-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="78a27-131">[上一頁](dynamically-populating-a-control-vb.md)
> [下一頁](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="78a27-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
