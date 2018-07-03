---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: 對抗 Bot (VB) |Microsoft Docs
author: wenz
description: 自動化的機器人 plaster 與垃圾郵件，提交而不需要任何使用者互動的註解表單的部落格和其他網站。 在 ASP.NET AJAX Con NoBot 控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f919d130cc97b5f892cc01d58a0bcba790a98e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362514"
---
<a name="fighting-bots-vb"></a><span data-ttu-id="62aa4-104">對抗 Bot (VB)</span><span class="sxs-lookup"><span data-stu-id="62aa4-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="62aa4-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="62aa4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="62aa4-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="62aa4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="62aa4-107">自動化的機器人 plaster 與垃圾郵件，提交而不需要任何使用者互動的註解表單的部落格和其他網站。</span><span class="sxs-lookup"><span data-stu-id="62aa4-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="62aa4-108">在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。</span><span class="sxs-lookup"><span data-stu-id="62aa4-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="62aa4-109">總覽</span><span class="sxs-lookup"><span data-stu-id="62aa4-109">Overview</span></span>

<span data-ttu-id="62aa4-110">自動化的機器人 plaster 與垃圾郵件，提交而不需要任何使用者互動的註解表單的部落格和其他網站。</span><span class="sxs-lookup"><span data-stu-id="62aa4-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="62aa4-111">在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。</span><span class="sxs-lookup"><span data-stu-id="62aa4-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="62aa4-112">步驟</span><span class="sxs-lookup"><span data-stu-id="62aa4-112">Steps</span></span>

<span data-ttu-id="62aa4-113">一個常見的方法，來擊敗 bot 是使用 CAPTCHAs 完全自動化公用 Turing 測試，告訴電腦，以及讓人判讀分開。</span><span class="sxs-lookup"><span data-stu-id="62aa4-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="62aa4-114">Turing 測試原本是測試有人需要決定通訊的夥伴一般人或電腦的地方。</span><span class="sxs-lookup"><span data-stu-id="62aa4-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="62aa4-115">在 web、 CAPTCHA 通常包含一些扭曲的字母，在其上的映像。</span><span class="sxs-lookup"><span data-stu-id="62aa4-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="62aa4-116">其概念是人類看得可以讀取映像上的字母而 OCR 演算法將會失敗。</span><span class="sxs-lookup"><span data-stu-id="62aa4-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="62aa4-117">有幾個優點和缺點，這種作法，但這些討論超出本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="62aa4-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="62aa4-118">不過沒有 ASP.NET AJAX Control Toolkit 提供類似的方法中的控制項： `NoBot`。</span><span class="sxs-lookup"><span data-stu-id="62aa4-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="62aa4-119">它會比較容易克服比文字驗證，但非常容易使用和時，它會被視為成功如果大多數垃圾郵件嘗試的部落格等的網站上也會被破壞，其中`NoBot`控制可以執行。</span><span class="sxs-lookup"><span data-stu-id="62aa4-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="62aa4-120">`NoBot` 符合至少其中一種情形時，會攔截目前的 ASP.NET web form 回傳：</span><span class="sxs-lookup"><span data-stu-id="62aa4-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="62aa4-121">不能解決 JavaScript 謎題的瀏覽器 （例如當 JavaScript 已停用）</span><span class="sxs-lookup"><span data-stu-id="62aa4-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="62aa4-122">使用者提交表單，以快速</span><span class="sxs-lookup"><span data-stu-id="62aa4-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="62aa4-123">用戶端 IP 位址會提交表單，往往在一段時間。</span><span class="sxs-lookup"><span data-stu-id="62aa4-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="62aa4-124">若要檢查是否有這些條件，`NoBot`控制項需要這些屬性 （全部選用）：</span><span class="sxs-lookup"><span data-stu-id="62aa4-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="62aa4-125">`ResponseMinimumDelaySeconds` 回傳之間的秒數的最小數量</span><span class="sxs-lookup"><span data-stu-id="62aa4-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="62aa4-126">`CutoffWindowSeconds` 在其中一個 IP 回傳是量值的時間間隔的長度</span><span class="sxs-lookup"><span data-stu-id="62aa4-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="62aa4-127">`CutoffMaximumInstances` 每個時間間隔的秒數的最大數量</span><span class="sxs-lookup"><span data-stu-id="62aa4-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="62aa4-128">下列標記要求至少兩秒數經過回傳之間並沒有只有五個回傳或小於 30 秒的時間間隔內：</span><span class="sxs-lookup"><span data-stu-id="62aa4-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="62aa4-129">然後如往常般請務必包含`ScriptManager`頁面中，讓 ASP.NET AJAX 程式庫已載入，並控制工具組可以用於：</span><span class="sxs-lookup"><span data-stu-id="62aa4-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="62aa4-130">因為大部分的檢查`NoBot`負責在伺服器端上進行您需要檢查這些驗證的結果。</span><span class="sxs-lookup"><span data-stu-id="62aa4-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="62aa4-131">這可以藉由呼叫`NoBot`的`IsValid()`方法。</span><span class="sxs-lookup"><span data-stu-id="62aa4-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="62aa4-132">它有一個引數 (以`out`參數 /`ByRef`參數) 的`NoBotState`。</span><span class="sxs-lookup"><span data-stu-id="62aa4-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="62aa4-133">檢查失敗時，其字串表示會包含原因和`Valid`否則。</span><span class="sxs-lookup"><span data-stu-id="62aa4-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="62aa4-134">下列程式碼會輸出訊息，以根據`NoBot`的結果：</span><span class="sxs-lookup"><span data-stu-id="62aa4-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="62aa4-135">最後，您需要提交表單和輸出訊息，label 項目，並完成 ！</span><span class="sxs-lookup"><span data-stu-id="62aa4-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="62aa4-136">當您執行此指令碼和停用 JavaScript 或送出表單時前, 兩秒內或 30 秒內七次送出表單時，會出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="62aa4-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="62aa4-137">不過更聰明地使用這個控制項，因為只有約 90-95%的使用者必須啟用 JavaScript，因此 5-10%的使用者將會失敗`NoBot`的測試。</span><span class="sxs-lookup"><span data-stu-id="62aa4-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="62aa4-138">[![此錯誤訊息可能被因為由 bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="62aa4-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="62aa4-139">此錯誤訊息可能因為由電腦控制機甲 ([按一下以檢視完整大小的影像](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="62aa4-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="62aa4-140">上一步</span><span class="sxs-lookup"><span data-stu-id="62aa4-140">Previous</span></span>](fighting-bots-cs.md)
