---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: 允許文字方塊 (C#) 中的某些字元 |Microsoft 文件
author: wenz
description: ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。 不過這仍無法防止使用者輸入不正確...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d2ffc4b741bd0c7f9c456b6e76017f5350ab6378
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="bb599-104">允許文字方塊 (C#) 中的某些字元</span><span class="sxs-lookup"><span data-stu-id="bb599-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="bb599-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bb599-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bb599-106">[下載程式碼](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bb599-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="bb599-107">ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="bb599-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="bb599-108">不過這仍不會無法防止使用者輸入無效的字元，而且嘗試送出表單。</span><span class="sxs-lookup"><span data-stu-id="bb599-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="bb599-109">總覽</span><span class="sxs-lookup"><span data-stu-id="bb599-109">Overview</span></span>

<span data-ttu-id="bb599-110">ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="bb599-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="bb599-111">不過這仍不會無法防止使用者輸入無效的字元，而且嘗試送出表單。</span><span class="sxs-lookup"><span data-stu-id="bb599-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="bb599-112">步驟</span><span class="sxs-lookup"><span data-stu-id="bb599-112">Steps</span></span>

<span data-ttu-id="bb599-113">ASP.NET AJAX Control Toolkit 包含`FilteredTextBox`擴充文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="bb599-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="bb599-114">一旦啟動後，只有特定的一組字元可能會輸入到欄位。</span><span class="sxs-lookup"><span data-stu-id="bb599-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="bb599-115">針對此目的，我們必須先如往常般 ASP.NET AJAX`ScriptManager`載入也會使用 ASP.NET AJAX Control Toolkit 的 JavaScript 程式庫：</span><span class="sxs-lookup"><span data-stu-id="bb599-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="bb599-116">然後，我們需要的文字方塊：</span><span class="sxs-lookup"><span data-stu-id="bb599-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="bb599-117">最後，`FilteredTextBoxExtender`控制項負責限制允許使用者輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="bb599-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="bb599-118">首先，設定`TargetControlID`屬性`ID`的`TextBox`控制項。</span><span class="sxs-lookup"><span data-stu-id="bb599-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="bb599-119">然後，選擇其中一個可用`FilterType`值：</span><span class="sxs-lookup"><span data-stu-id="bb599-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="bb599-120">`Custom` 預設值;您必須提供有效的字元的清單</span><span class="sxs-lookup"><span data-stu-id="bb599-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="bb599-121">`LowercaseLetters` 只使用小寫字母</span><span class="sxs-lookup"><span data-stu-id="bb599-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="bb599-122">`Numbers` 只有數字</span><span class="sxs-lookup"><span data-stu-id="bb599-122">`Numbers` digits only</span></span>
- <span data-ttu-id="bb599-123">`UppercaseLetters` 只有大寫的字母</span><span class="sxs-lookup"><span data-stu-id="bb599-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="bb599-124">如果`Custom FilterType`使用時，`ValidChars`屬性必須設定，並提供一份可能輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="bb599-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="bb599-125">方式： 如果您嘗試將文字貼到文字方塊中，會移除所有無效的字元。</span><span class="sxs-lookup"><span data-stu-id="bb599-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="bb599-126">以下是標記`FilteredTextBoxExtender`控制項，僅允許 數字 (已經使用的項目`FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="bb599-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="bb599-127">執行頁面並再試一次輸入字母，如果已啟用 JavaScript，它將無法運作。數字，但會出現在頁面上。</span><span class="sxs-lookup"><span data-stu-id="bb599-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="bb599-128">不過請注意，保護`FilteredTextBox`提供不是項目符號證明： 如果 JavaScript 已啟用，因此您需要使用額外的驗證方法，也就是 ASP，可能會在文字方塊中，輸入任何資料。網路的驗證控制項。</span><span class="sxs-lookup"><span data-stu-id="bb599-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="bb599-129">[![可能會輸入只有數字](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bb599-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="bb599-130">可能會輸入只有數字 ([按一下以檢視完整大小的影像](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bb599-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bb599-131">下一步</span><span class="sxs-lookup"><span data-stu-id="bb599-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
