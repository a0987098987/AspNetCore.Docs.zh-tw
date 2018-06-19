---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: 使用 JavaScript (VB) 的使用者控制項與 DynamicPopulate |Microsoft 文件
author: wenz
description: 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標上的控制項 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872997"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="b06f7-103">使用 DynamicPopulate 使用者控制和 JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="b06f7-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="b06f7-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b06f7-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b06f7-105">[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b06f7-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="b06f7-106">在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標控制項在頁面上，如果沒有頁面重新整理。</span><span class="sxs-lookup"><span data-stu-id="b06f7-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="b06f7-107">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="b06f7-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="b06f7-108">不過特別注意有 extender 位於使用者控制項時。</span><span class="sxs-lookup"><span data-stu-id="b06f7-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="b06f7-109">總覽</span><span class="sxs-lookup"><span data-stu-id="b06f7-109">Overview</span></span>

<span data-ttu-id="b06f7-110">`DynamicPopulate` ASP.NET AJAX Control Toolkit 中的控制呼叫 web 服務 （或頁面的方法），並填入目標控制項在頁面上，如果沒有頁面重新整理所產生的值。</span><span class="sxs-lookup"><span data-stu-id="b06f7-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="b06f7-111">它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。</span><span class="sxs-lookup"><span data-stu-id="b06f7-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="b06f7-112">不過特別注意有 extender 位於使用者控制項時。</span><span class="sxs-lookup"><span data-stu-id="b06f7-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="b06f7-113">步驟</span><span class="sxs-lookup"><span data-stu-id="b06f7-113">Steps</span></span>

<span data-ttu-id="b06f7-114">首先，您需要 ASP.NET Web 服務會實作由呼叫方法`DynamicPopulateExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="b06f7-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="b06f7-115">Web 服務實作的方法`getDate()`，必須要有一個引數之字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一個片段的內容資訊與每個 web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="b06f7-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="b06f7-116">下列程式碼 (檔`DynamicPopulate.vb.asmx`) 抓取目前的日期，三種格式之一：</span><span class="sxs-lookup"><span data-stu-id="b06f7-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="b06f7-117">在下一個步驟中，建立新的使用者控制項 (`.ascx`檔案)、 說明其第一個列中的下列宣告：</span><span class="sxs-lookup"><span data-stu-id="b06f7-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="b06f7-118">A &lt; `label` &gt;項目會用來顯示來自伺服器的資料。</span><span class="sxs-lookup"><span data-stu-id="b06f7-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="b06f7-119">也在使用者控制項檔案中，我們將使用三個選項按鈕，每一個都代表其中一個 web 服務所支援的三個可能的日期格式。</span><span class="sxs-lookup"><span data-stu-id="b06f7-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="b06f7-120">當使用者按一下其中一個選項按鈕時，瀏覽器將會執行 JavaScript 程式碼看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="b06f7-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="b06f7-121">此程式碼會存取`DynamicPopulateExtender`（不必擔心奇怪的識別碼，但這會在稍後說明），並且觸發動態的母體擴展的資料。</span><span class="sxs-lookup"><span data-stu-id="b06f7-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="b06f7-122">目前的選擇鈕，內容中`this.value`指的是其值為`format1`，`format2`或`format3`完全 web 方法所預期的內容。</span><span class="sxs-lookup"><span data-stu-id="b06f7-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="b06f7-123">尚未遺漏使用者控制項中的唯一是`DynamicPopulateExtender`連結至 web 服務的選項按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="b06f7-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="b06f7-124">一次您可能注意到控制項中所使用的奇怪 ID:`mcd1$myDate`而不是`myDate`。</span><span class="sxs-lookup"><span data-stu-id="b06f7-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="b06f7-125">先前，JavaScript 程式碼使用`mcd1_dpe1`存取`DynamicPopulateExtender`而不是`dpe1`。此命名策略是特殊的需求，使用時`DynamicPopulateExtender`使用者控制項內。</span><span class="sxs-lookup"><span data-stu-id="b06f7-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="b06f7-126">此外，您必須將使用者要針對內嵌在特定的方式，讓所有工作。</span><span class="sxs-lookup"><span data-stu-id="b06f7-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="b06f7-127">建立新的 ASP.NET 網頁，並註冊您已實作的使用者控制項的標記前置詞：</span><span class="sxs-lookup"><span data-stu-id="b06f7-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="b06f7-128">然後，包含 ASP.NET AJAX`ScriptManager`新網頁上的控制項：</span><span class="sxs-lookup"><span data-stu-id="b06f7-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="b06f7-129">最後，將使用者控制項加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="b06f7-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="b06f7-130">您只需要設定其`ID`屬性 (和`runat="server"`當然)，但是您也必須將它設定為特定的名稱：`mcd1`因為這是在使用者控制項用來存取使用 JavaScript 的前置詞。</span><span class="sxs-lookup"><span data-stu-id="b06f7-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="b06f7-131">就是這麼容易！</span><span class="sxs-lookup"><span data-stu-id="b06f7-131">And that's it!</span></span> <span data-ttu-id="b06f7-132">頁面在如預期般運作： 使用者按一下其中一個選項按鈕，此工具組中的控制項呼叫 web 服務和所需的格式顯示目前的日期。</span><span class="sxs-lookup"><span data-stu-id="b06f7-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="b06f7-133">[![選項按鈕位於使用者控制項](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b06f7-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="b06f7-134">選項按鈕位於使用者控制項 ([按一下以檢視完整大小的影像](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b06f7-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b06f7-135">上一步</span><span class="sxs-lookup"><span data-stu-id="b06f7-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
