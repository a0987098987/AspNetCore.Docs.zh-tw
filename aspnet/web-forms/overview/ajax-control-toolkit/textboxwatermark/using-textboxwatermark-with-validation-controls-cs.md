---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: 使用 TextBoxWatermark 與驗證控制項 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit TextBoxWatermark 控制延伸的文字方塊，讓文字會顯示在方塊內。 當使用者在方塊中，按一下它我...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 144e4ee8f8096076b4e498844429ebf96e6ea402
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836185"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="c7122-104">使用 TextBoxWatermark 與驗證控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="c7122-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="c7122-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c7122-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c7122-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c7122-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="c7122-107">在 AJAX Control Toolkit TextBoxWatermark 控制延伸的文字方塊，讓文字會顯示在方塊內。</span><span class="sxs-lookup"><span data-stu-id="c7122-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="c7122-108">當使用者按一下方塊時，它會清空。</span><span class="sxs-lookup"><span data-stu-id="c7122-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="c7122-109">如果使用者離開不需要輸入文字的方塊中，預先填入的文字隨即再度出現。</span><span class="sxs-lookup"><span data-stu-id="c7122-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="c7122-110">這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。</span><span class="sxs-lookup"><span data-stu-id="c7122-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="c7122-111">總覽</span><span class="sxs-lookup"><span data-stu-id="c7122-111">Overview</span></span>

<span data-ttu-id="c7122-112">`TextBoxWatermark`中 AJAX Control Toolkit 控制項擴充文字方塊中，使方塊內顯示的文字。</span><span class="sxs-lookup"><span data-stu-id="c7122-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="c7122-113">當使用者按一下方塊時，它會清空。</span><span class="sxs-lookup"><span data-stu-id="c7122-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="c7122-114">如果使用者離開不需要輸入文字的方塊中，預先填入的文字隨即再度出現。</span><span class="sxs-lookup"><span data-stu-id="c7122-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="c7122-115">這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。</span><span class="sxs-lookup"><span data-stu-id="c7122-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="c7122-116">步驟</span><span class="sxs-lookup"><span data-stu-id="c7122-116">Steps</span></span>

<span data-ttu-id="c7122-117">基本設定的範例如下：`TextBox`控制項使用浮水印`TextBoxWatermarkExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="c7122-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="c7122-118">按鈕觸發回傳，並稍後將用來觸發頁面上的驗證控制項。</span><span class="sxs-lookup"><span data-stu-id="c7122-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="c7122-119">此外，`ScriptManager`控制項，才能初始化 ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="c7122-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="c7122-120">現在加入`RequiredFieldValidator`會檢查是否有文字欄位中送出表單時的控制項。</span><span class="sxs-lookup"><span data-stu-id="c7122-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="c7122-121">`InitialValue`驗證程式的屬性必須設定為相同的值，用於`TextBoxWatermarkExtender`控制項： 送出表單時，不變的文字方塊的值是在其中的水位線值：</span><span class="sxs-lookup"><span data-stu-id="c7122-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="c7122-122">不過還有使用這種方法的一個問題： 如果用戶端停用 JavaScript，在文字欄位已不預先填入浮水印文字，因此`RequiredFieldValidator`不會觸發錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c7122-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="c7122-123">因此，第二個`RequiredFieldValidator`控制，則它會檢查是否有空白的文字方塊 (省略`InitialValue`屬性)。</span><span class="sxs-lookup"><span data-stu-id="c7122-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="c7122-124">由於這兩種驗證採用`Display` = `"Dynamic"`，終端使用者無法區別這兩個驗證程式引發的視覺外觀; 相反地，它看起來像是時發生只有其中之一。</span><span class="sxs-lookup"><span data-stu-id="c7122-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="c7122-125">最後，新增一些的伺服器端程式碼，以輸出欄位中的文字，如果未驗證發出一則錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="c7122-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="c7122-126">[![驗證程式產生負面反應，在欄位中沒有任何文字](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c7122-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="c7122-127">驗證程式產生負面反應，在欄位中沒有任何文字 ([按一下以檢視完整大小的影像](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c7122-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7122-128">[上一頁](using-textboxwatermark-in-a-formview-cs.md)
> [下一頁](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c7122-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
