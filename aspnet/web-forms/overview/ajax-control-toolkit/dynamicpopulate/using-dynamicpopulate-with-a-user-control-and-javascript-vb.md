---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: 使用具有 DynamicPopulate 使用者控制和 JavaScript (VB) |Microsoft Docs
author: wenz
description: DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿至 t 的目標控制項...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: c2fe7fbe0d57c6cf64fe31607215c920e67ed736
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841311"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="0deac-103">使用具有 DynamicPopulate 使用者控制和 JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="0deac-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="0deac-104">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0deac-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0deac-105">[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0deac-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="0deac-106">DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="0deac-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="0deac-107">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="0deac-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="0deac-108">不過，特別注意具有擴充項位於使用者控制項時所要採取。</span><span class="sxs-lookup"><span data-stu-id="0deac-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="0deac-109">總覽</span><span class="sxs-lookup"><span data-stu-id="0deac-109">Overview</span></span>

<span data-ttu-id="0deac-110">`DynamicPopulate`中 ASP.NET AJAX Control Toolkit 控制項呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="0deac-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="0deac-111">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="0deac-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="0deac-112">不過，特別注意具有擴充項位於使用者控制項時所要採取。</span><span class="sxs-lookup"><span data-stu-id="0deac-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="0deac-113">步驟</span><span class="sxs-lookup"><span data-stu-id="0deac-113">Steps</span></span>

<span data-ttu-id="0deac-114">首先，您必須實作呼叫方法的 ASP.NET Web 服務`DynamicPopulateExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="0deac-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="0deac-115">Web 服務實作的方法`getDate()`它需要一個引數的字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一段內容資訊與每個 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="0deac-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="0deac-116">以下是程式碼 (檔案`DynamicPopulate.vb.asmx`) 表示擷取目前的日期，三種格式之一：</span><span class="sxs-lookup"><span data-stu-id="0deac-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="0deac-117">在下一個步驟中，建立新的使用者控制項 (`.ascx`檔案)，表示其第一行中的下列宣告：</span><span class="sxs-lookup"><span data-stu-id="0deac-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="0deac-118">A &lt; `label` &gt;項目會用來顯示來自伺服器的資料。</span><span class="sxs-lookup"><span data-stu-id="0deac-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="0deac-119">也在使用者控制項檔案中，我們將使用三個選項按鈕，每一個都代表其中一個 web 服務所支援的三個可能的日期格式。</span><span class="sxs-lookup"><span data-stu-id="0deac-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="0deac-120">當使用者按一下其中一個選項按鈕時，瀏覽器會執行 JavaScript 程式碼看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0deac-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="0deac-121">這個程式碼存取`DynamicPopulateExtender`（不必擔心奇怪的識別碼，但這將在稍後說明），並觸發動態的母體擴展的資料。</span><span class="sxs-lookup"><span data-stu-id="0deac-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="0deac-122">目前的選項按鈕，內容中`this.value`也就是其值是指`format1`，`format2`或`format3`完全 web 方法所預期的內容。</span><span class="sxs-lookup"><span data-stu-id="0deac-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="0deac-123">唯一還缺少使用者控制項中的事項是`DynamicPopulateExtender`連結至 web 服務的選項按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="0deac-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="0deac-124">一次您可能注意到控制項中所使用的奇怪識別碼：`mcd1$myDate`而不是`myDate`。</span><span class="sxs-lookup"><span data-stu-id="0deac-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="0deac-125">先前，JavaScript 程式碼使用`mcd1_dpe1`若要存取`DynamicPopulateExtender`而不是`dpe1`。使用時，此命名策略是特殊的需求`DynamicPopulateExtender`使用者控制項內。</span><span class="sxs-lookup"><span data-stu-id="0deac-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="0deac-126">此外，您必須將使用者要內嵌到正常運作以特定方式。</span><span class="sxs-lookup"><span data-stu-id="0deac-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="0deac-127">建立新的 ASP.NET 網頁，並註冊您已實作的使用者控制項的標記前置詞：</span><span class="sxs-lookup"><span data-stu-id="0deac-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="0deac-128">然後，包含 ASP.NET AJAX`ScriptManager`新網頁上的控制項：</span><span class="sxs-lookup"><span data-stu-id="0deac-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="0deac-129">最後，新增至頁面的使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="0deac-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="0deac-130">您只需要設定其`ID`屬性 (並`runat="server"`當然)，但是您也必須將它設定為特定的名稱：`mcd1`由於這是使用者控制項內，用於使用 JavaScript 進行存取的前置詞。</span><span class="sxs-lookup"><span data-stu-id="0deac-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="0deac-131">就是這麼容易！</span><span class="sxs-lookup"><span data-stu-id="0deac-131">And that's it!</span></span> <span data-ttu-id="0deac-132">頁面的行為如預期般運作： 使用者按一下其中一個選項按鈕，此工具組中的控制項呼叫 web 服務，並顯示目前的日期所需的格式。</span><span class="sxs-lookup"><span data-stu-id="0deac-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="0deac-133">[![選項按鈕位於使用者控制項](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0deac-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="0deac-134">選項按鈕位於使用者控制項 ([按一下以檢視完整大小的影像](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0deac-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0deac-135">上一步</span><span class="sxs-lookup"><span data-stu-id="0deac-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
