---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: "對抗 Bot (C#) |Microsoft 文件"
author: wenz
description: "自動的 bot plaster 網誌及其他網站與垃圾郵件，而不需要任何使用者互動的註解表單送出。 在 ASP.NET AJAX Con NoBot 控制..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: b8eedff4691c1115e242be884f9e74663dc0b4f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-c"></a><span data-ttu-id="4371f-104">對抗 Bot (C#)</span><span class="sxs-lookup"><span data-stu-id="4371f-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="4371f-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4371f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4371f-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4371f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="4371f-107">自動的 bot plaster 網誌及其他網站與垃圾郵件，而不需要任何使用者互動的註解表單送出。</span><span class="sxs-lookup"><span data-stu-id="4371f-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="4371f-108">在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。</span><span class="sxs-lookup"><span data-stu-id="4371f-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="4371f-109">概觀</span><span class="sxs-lookup"><span data-stu-id="4371f-109">Overview</span></span>

<span data-ttu-id="4371f-110">自動的 bot plaster 網誌及其他網站與垃圾郵件，而不需要任何使用者互動的註解表單送出。</span><span class="sxs-lookup"><span data-stu-id="4371f-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="4371f-111">在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。</span><span class="sxs-lookup"><span data-stu-id="4371f-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="4371f-112">步驟</span><span class="sxs-lookup"><span data-stu-id="4371f-112">Steps</span></span>

<span data-ttu-id="4371f-113">擊敗 bot 的一個常見方式是使用 CAPTCHAs 完全自動化公用 Turing 測試，告訴電腦，然後讓人了距離。</span><span class="sxs-lookup"><span data-stu-id="4371f-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="4371f-114">Turing 測試原本測試其他人需要決定通訊夥伴是一般人或電腦的位置。</span><span class="sxs-lookup"><span data-stu-id="4371f-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="4371f-115">在 web、 CAPTCHA 通常組成上某些扭曲字母的映像。</span><span class="sxs-lookup"><span data-stu-id="4371f-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="4371f-116">這個概念是人類看得可以讀取映像上的字母而 OCR 演算法將會失敗。</span><span class="sxs-lookup"><span data-stu-id="4371f-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="4371f-117">有數個優點和缺點，這種方法，但對此的討論超出本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="4371f-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="4371f-118">不過這會產生 ASP.NET AJAX Control Toolkit 會提供類似的方法中的控制項： `NoBot`。</span><span class="sxs-lookup"><span data-stu-id="4371f-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="4371f-119">它比較容易克服比 CAPTCHA，但非常容易使用及 fares 非常好上類似，它會被視為成功如果大部分垃圾郵件嘗試的部落格網站會被破壞，其中`NoBot`控制可以執行。</span><span class="sxs-lookup"><span data-stu-id="4371f-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="4371f-120">`NoBot`如果符合下列情況之一時，會攔截目前的 ASP.NET web form 回傳：</span><span class="sxs-lookup"><span data-stu-id="4371f-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="4371f-121">不能解決 JavaScript 拼圖的瀏覽器 （例如當 JavaScript 停用時）</span><span class="sxs-lookup"><span data-stu-id="4371f-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="4371f-122">使用者提交表單，以快速</span><span class="sxs-lookup"><span data-stu-id="4371f-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="4371f-123">用戶端 IP 位址太常在一段時間中提交表單。</span><span class="sxs-lookup"><span data-stu-id="4371f-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="4371f-124">若要檢查這些條件，`NoBot`控制項需要這些屬性 （全部選用）：</span><span class="sxs-lookup"><span data-stu-id="4371f-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="4371f-125">`ResponseMinimumDelaySeconds`最小數量回傳之間的秒數</span><span class="sxs-lookup"><span data-stu-id="4371f-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="4371f-126">`CutoffWindowSeconds`在其中一個 IP 從回傳是量值的時間間隔的長度</span><span class="sxs-lookup"><span data-stu-id="4371f-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="4371f-127">`CutoffMaximumInstances`每個時間間隔的秒數的最大數量</span><span class="sxs-lookup"><span data-stu-id="4371f-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="4371f-128">下列標記要求該至少兩秒經過回傳之間並沒有只有五個回傳或小於 30 秒的時間間隔內：</span><span class="sxs-lookup"><span data-stu-id="4371f-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="4371f-129">然後如往常般請務必包含`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：</span><span class="sxs-lookup"><span data-stu-id="4371f-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="4371f-130">因為大部分的檢查`NoBot`執行的動作就會發生在伺服器端，您需要檢查這些驗證的結果。</span><span class="sxs-lookup"><span data-stu-id="4371f-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="4371f-131">這可藉由呼叫`NoBot`的`IsValid()`方法。</span><span class="sxs-lookup"><span data-stu-id="4371f-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="4371f-132">它有一個引數 (為`out`參數 /`ByRef`參數) 類型`NoBotState`。</span><span class="sxs-lookup"><span data-stu-id="4371f-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="4371f-133">檢查失敗時，其字串表示法包含原因和`Valid`否則。</span><span class="sxs-lookup"><span data-stu-id="4371f-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="4371f-134">下列程式碼將輸出訊息，以根據`NoBot`的結果：</span><span class="sxs-lookup"><span data-stu-id="4371f-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="4371f-135">最後，您需要提交表單和輸出訊息，label 元素，並完成 ！</span><span class="sxs-lookup"><span data-stu-id="4371f-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="4371f-136">當您執行這個指令碼和停用 JavaScript 或第 2 秒鐘內送出表單或七次 30 秒內送出表單時，會出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="4371f-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="4371f-137">不過請明智地使用這個控制項，因為只有大約 90 95%的使用者必須啟用 JavaScript，因此 5-10%的使用者將會失敗`NoBot`的測試。</span><span class="sxs-lookup"><span data-stu-id="4371f-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="4371f-138">[![這則錯誤訊息可能被因為由 bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4371f-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="4371f-139">這則錯誤訊息可能因為由 bot ([按一下以檢視完整大小的影像](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4371f-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4371f-140">下一步</span><span class="sxs-lookup"><span data-stu-id="4371f-140">Next</span></span>](fighting-bots-vb.md)
