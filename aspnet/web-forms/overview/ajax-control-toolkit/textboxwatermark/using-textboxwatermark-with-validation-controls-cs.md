---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: 驗證控制項 (C#) 搭配使用 TextBoxWatermark |Microsoft 文件
author: wenz
description: AJAX Control Toolkit TextBoxWatermark 控制項擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下 [到] 方塊，它我...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="20180-104">使用 TextBoxWatermark 與驗證控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="20180-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="20180-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="20180-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="20180-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="20180-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="20180-107">AJAX Control Toolkit TextBoxWatermark 控制項擴充文字方塊中，使方塊內會顯示文字。</span><span class="sxs-lookup"><span data-stu-id="20180-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="20180-108">當使用者按一下此方塊時，它會清空。</span><span class="sxs-lookup"><span data-stu-id="20180-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="20180-109">如果使用者離開 [] 方塊中，而不需輸入文字，其中已預先的文字會重新出現。</span><span class="sxs-lookup"><span data-stu-id="20180-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="20180-110">這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。</span><span class="sxs-lookup"><span data-stu-id="20180-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="20180-111">總覽</span><span class="sxs-lookup"><span data-stu-id="20180-111">Overview</span></span>

<span data-ttu-id="20180-112">`TextBoxWatermark` AJAX Control Toolkit 中的控制會擴充文字方塊中，使方塊內會顯示文字。</span><span class="sxs-lookup"><span data-stu-id="20180-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="20180-113">當使用者按一下此方塊時，它會清空。</span><span class="sxs-lookup"><span data-stu-id="20180-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="20180-114">如果使用者離開 [] 方塊中，而不需輸入文字，其中已預先的文字會重新出現。</span><span class="sxs-lookup"><span data-stu-id="20180-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="20180-115">這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。</span><span class="sxs-lookup"><span data-stu-id="20180-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="20180-116">步驟</span><span class="sxs-lookup"><span data-stu-id="20180-116">Steps</span></span>

<span data-ttu-id="20180-117">基本設定的範例如下所示：`TextBox`控制項使用的浮水印`TextBoxWatermarkExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="20180-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="20180-118">按鈕會觸發回傳，並稍後會用來觸發驗證控制項在頁面上。</span><span class="sxs-lookup"><span data-stu-id="20180-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="20180-119">此外，`ScriptManager`控制項，才能初始化 ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="20180-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="20180-120">現在，加入`RequiredFieldValidator`會檢查是否有文字欄位中時提交此表單的控制項。</span><span class="sxs-lookup"><span data-stu-id="20180-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="20180-121">`InitialValue`驗證程式的屬性必須設定為相同的值，用於`TextBoxWatermarkExtender`控制項： 提交表單時，不變的文字方塊的值時，浮水印值，其中：</span><span class="sxs-lookup"><span data-stu-id="20180-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="20180-122">但是沒有這種方法有一個問題： 如果用戶端停用 JavaScript，在文字欄位不因此使具有浮水印文字`RequiredFieldValidator`不會觸發錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="20180-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="20180-123">因此，第二個`RequiredFieldValidator`控制項是必要也會檢查是否有空白的文字方塊中 (省略`InitialValue`屬性)。</span><span class="sxs-lookup"><span data-stu-id="20180-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="20180-124">因為這兩個驗證程式使用`Display` = `"Dynamic"`，終端使用者無法區別這兩個驗證程式所引發的視覺外觀; 相反地，您似乎沒有只有一個。</span><span class="sxs-lookup"><span data-stu-id="20180-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="20180-125">最後，將某些伺服器端程式碼加入輸出的文字欄位中，如果沒有驗證程式會發出錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="20180-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="20180-126">[![驗證程式指出其欄位中沒有任何文字](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20180-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="20180-127">驗證程式指出其欄位中沒有任何文字 ([按一下以檢視完整大小的影像](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="20180-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="20180-128">[上一頁](using-textboxwatermark-in-a-formview-cs.md)
> [下一頁](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="20180-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
