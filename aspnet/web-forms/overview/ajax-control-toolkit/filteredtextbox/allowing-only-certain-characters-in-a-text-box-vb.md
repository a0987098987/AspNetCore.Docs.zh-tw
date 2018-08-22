---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: 只允許特定字元在文字方塊中 (VB) |Microsoft Docs
author: wenz
description: ASP.NET 驗證控制項可以確保只有特定字元允許在使用者輸入。 不過這仍無法防止使用者輸入不正確...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: aec5a3af98cf40e460f4164fb8950e8029002937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823671"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="186ae-104">只允許特定字元在文字方塊中 (VB)</span><span class="sxs-lookup"><span data-stu-id="186ae-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="186ae-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="186ae-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="186ae-106">[下載程式碼](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="186ae-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="186ae-107">ASP.NET 驗證控制項可以確保只有特定字元允許在使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="186ae-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="186ae-108">不過這仍不會防止使用者輸入無效的字元，並嘗試送出表單。</span><span class="sxs-lookup"><span data-stu-id="186ae-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="186ae-109">總覽</span><span class="sxs-lookup"><span data-stu-id="186ae-109">Overview</span></span>

<span data-ttu-id="186ae-110">ASP.NET 驗證控制項可以確保只有特定字元允許在使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="186ae-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="186ae-111">不過這仍不會防止使用者輸入無效的字元，並嘗試送出表單。</span><span class="sxs-lookup"><span data-stu-id="186ae-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="186ae-112">步驟</span><span class="sxs-lookup"><span data-stu-id="186ae-112">Steps</span></span>

<span data-ttu-id="186ae-113">ASP.NET AJAX Control Toolkit 包含`FilteredTextBox`擴充文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="186ae-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="186ae-114">一旦啟動後，只有特定的一組字元可能會輸入欄位。</span><span class="sxs-lookup"><span data-stu-id="186ae-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="186ae-115">針對此目的，我們必須先如往常般 ASP.NET AJAX`ScriptManager`載入也會由 ASP.NET AJAX Control Toolkit 中的 JavaScript 程式庫：</span><span class="sxs-lookup"><span data-stu-id="186ae-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="186ae-116">然後，我們需要在文字方塊中：</span><span class="sxs-lookup"><span data-stu-id="186ae-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="186ae-117">最後，`FilteredTextBoxExtender`控制項負責限制允許使用者輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="186ae-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="186ae-118">首先，設定`TargetControlID`屬性設定為`ID`的`TextBox`控制項。</span><span class="sxs-lookup"><span data-stu-id="186ae-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="186ae-119">然後，選擇其中一個可用`FilterType`值：</span><span class="sxs-lookup"><span data-stu-id="186ae-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="186ae-120">`Custom` 預設值;您必須提供一份有效的字元</span><span class="sxs-lookup"><span data-stu-id="186ae-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="186ae-121">`LowercaseLetters` 只有小寫字母</span><span class="sxs-lookup"><span data-stu-id="186ae-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="186ae-122">`Numbers` 只有數字</span><span class="sxs-lookup"><span data-stu-id="186ae-122">`Numbers` digits only</span></span>
- <span data-ttu-id="186ae-123">`UppercaseLetters` 大寫英文字母</span><span class="sxs-lookup"><span data-stu-id="186ae-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="186ae-124">如果`Custom FilterType`使用時，`ValidChars`屬性必須設定，並提供一份可能輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="186ae-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="186ae-125">順便一提： 如果您嘗試將文字貼到文字方塊中，會移除所有無效的字元。</span><span class="sxs-lookup"><span data-stu-id="186ae-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="186ae-126">以下是標記`FilteredTextBoxExtender`只允許數字的控制項 (也會使用的項目`FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="186ae-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="186ae-127">執行頁面並嘗試輸入字母，如果已啟用 JavaScript，它將無法運作;數字，不過會出現在頁面上。</span><span class="sxs-lookup"><span data-stu-id="186ae-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="186ae-128">不過請注意，保護`FilteredTextBox`提供不是確實： 如果 JavaScript 功能啟用後，可能會在文字方塊中，輸入任何資料，因此您必須使用額外的驗證方法，也就是 ASP。NET 的驗證控制項。</span><span class="sxs-lookup"><span data-stu-id="186ae-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="186ae-129">[![可能會輸入只有數字](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="186ae-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="186ae-130">可能會輸入只有數字 ([按一下以檢視完整大小的影像](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="186ae-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="186ae-131">上一步</span><span class="sxs-lookup"><span data-stu-id="186ae-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
