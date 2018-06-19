---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: 允許文字方塊 (VB) 中的某些字元 |Microsoft 文件
author: wenz
description: ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。 不過這仍無法防止使用者輸入不正確...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b63a3582c09e08310c97d4adfc7b8273458a723
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870150"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="866fd-104">允許文字方塊 (VB) 中的某些字元</span><span class="sxs-lookup"><span data-stu-id="866fd-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="866fd-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="866fd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="866fd-106">[下載程式碼](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="866fd-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="866fd-107">ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="866fd-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="866fd-108">不過這仍不會無法防止使用者輸入無效的字元，而且嘗試送出表單。</span><span class="sxs-lookup"><span data-stu-id="866fd-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="866fd-109">總覽</span><span class="sxs-lookup"><span data-stu-id="866fd-109">Overview</span></span>

<span data-ttu-id="866fd-110">ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="866fd-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="866fd-111">不過這仍不會無法防止使用者輸入無效的字元，而且嘗試送出表單。</span><span class="sxs-lookup"><span data-stu-id="866fd-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="866fd-112">步驟</span><span class="sxs-lookup"><span data-stu-id="866fd-112">Steps</span></span>

<span data-ttu-id="866fd-113">ASP.NET AJAX Control Toolkit 包含`FilteredTextBox`擴充文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="866fd-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="866fd-114">一旦啟動後，只有特定的一組字元可能會輸入到欄位。</span><span class="sxs-lookup"><span data-stu-id="866fd-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="866fd-115">針對此目的，我們必須先如往常般 ASP.NET AJAX`ScriptManager`載入也會使用 ASP.NET AJAX Control Toolkit 的 JavaScript 程式庫：</span><span class="sxs-lookup"><span data-stu-id="866fd-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="866fd-116">然後，我們需要的文字方塊：</span><span class="sxs-lookup"><span data-stu-id="866fd-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="866fd-117">最後，`FilteredTextBoxExtender`控制項負責限制允許使用者輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="866fd-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="866fd-118">首先，設定`TargetControlID`屬性`ID`的`TextBox`控制項。</span><span class="sxs-lookup"><span data-stu-id="866fd-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="866fd-119">然後，選擇其中一個可用`FilterType`值：</span><span class="sxs-lookup"><span data-stu-id="866fd-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="866fd-120">`Custom` 預設值;您必須提供有效的字元的清單</span><span class="sxs-lookup"><span data-stu-id="866fd-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="866fd-121">`LowercaseLetters` 只使用小寫字母</span><span class="sxs-lookup"><span data-stu-id="866fd-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="866fd-122">`Numbers` 只有數字</span><span class="sxs-lookup"><span data-stu-id="866fd-122">`Numbers` digits only</span></span>
- <span data-ttu-id="866fd-123">`UppercaseLetters` 只有大寫的字母</span><span class="sxs-lookup"><span data-stu-id="866fd-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="866fd-124">如果`Custom FilterType`使用時，`ValidChars`屬性必須設定，並提供一份可能輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="866fd-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="866fd-125">方式： 如果您嘗試將文字貼到文字方塊中，會移除所有無效的字元。</span><span class="sxs-lookup"><span data-stu-id="866fd-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="866fd-126">以下是標記`FilteredTextBoxExtender`控制項，僅允許 數字 (已經使用的項目`FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="866fd-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="866fd-127">執行頁面並再試一次輸入字母，如果已啟用 JavaScript，它將無法運作。數字，但會出現在頁面上。</span><span class="sxs-lookup"><span data-stu-id="866fd-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="866fd-128">不過請注意，保護`FilteredTextBox`提供不是項目符號證明： 如果 JavaScript 已啟用，因此您需要使用額外的驗證方法，也就是 ASP，可能會在文字方塊中，輸入任何資料。網路的驗證控制項。</span><span class="sxs-lookup"><span data-stu-id="866fd-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="866fd-129">[![可能會輸入只有數字](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="866fd-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="866fd-130">可能會輸入只有數字 ([按一下以檢視完整大小的影像](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="866fd-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="866fd-131">上一步</span><span class="sxs-lookup"><span data-stu-id="866fd-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
