---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: 處理回傳 ModalPopup (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 必須採取特別注意，當 pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="b8016-104">處理回傳 ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="b8016-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="b8016-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b8016-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b8016-106">[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b8016-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="b8016-107">AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。</span><span class="sxs-lookup"><span data-stu-id="b8016-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b8016-108">從快顯內建立回傳時，必須採取特別注意。</span><span class="sxs-lookup"><span data-stu-id="b8016-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="b8016-109">總覽</span><span class="sxs-lookup"><span data-stu-id="b8016-109">Overview</span></span>

<span data-ttu-id="b8016-110">AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。</span><span class="sxs-lookup"><span data-stu-id="b8016-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b8016-111">從快顯內建立回傳時，必須採取特別注意。</span><span class="sxs-lookup"><span data-stu-id="b8016-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="b8016-112">步驟</span><span class="sxs-lookup"><span data-stu-id="b8016-112">Steps</span></span>

<span data-ttu-id="b8016-113">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="b8016-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="b8016-114">接著，將會做為強制回應的快顯視窗的面板。</span><span class="sxs-lookup"><span data-stu-id="b8016-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="b8016-115">那里，使用者可以輸入的名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="b8016-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="b8016-116">按鈕會關閉快顯視窗，並儲存資訊。</span><span class="sxs-lookup"><span data-stu-id="b8016-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="b8016-117">請注意， `OnClick` ，以便在按下此按鈕時，就會發生回傳，設定屬性：</span><span class="sxs-lookup"><span data-stu-id="b8016-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="b8016-118">網頁本身的資訊完全相同的兩個標籤所組成： 名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="b8016-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="b8016-119">按鈕用來觸發強制回應的快顯視窗：</span><span class="sxs-lookup"><span data-stu-id="b8016-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="b8016-120">為了讓出現快顯視窗，加入`ModalPopupExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="b8016-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="b8016-121">設定`PopupControlID`屬性面板的識別碼和`TargetControlID`按鈕的識別碼：</span><span class="sxs-lookup"><span data-stu-id="b8016-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="b8016-122">現在每當`Save`強制回應的快顯內按一下按鈕時，伺服器端`SaveData()`執行方法。</span><span class="sxs-lookup"><span data-stu-id="b8016-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="b8016-123">您無法將輸入的資料儲存在資料存放區。</span><span class="sxs-lookup"><span data-stu-id="b8016-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="b8016-124">為了簡單起見，新的資料只會輸出在標籤中：</span><span class="sxs-lookup"><span data-stu-id="b8016-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="b8016-125">此外，強制回應的快顯內的文字方塊控制項應填入目前的名稱和電子郵件。</span><span class="sxs-lookup"><span data-stu-id="b8016-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="b8016-126">不過這是只有必要回傳發生時。</span><span class="sxs-lookup"><span data-stu-id="b8016-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="b8016-127">如果沒有回傳，ASP.NET viewstate 功能會自動填滿的適當值填入文字方塊。</span><span class="sxs-lookup"><span data-stu-id="b8016-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="b8016-128">[![強制回應的快顯視窗會回傳](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b8016-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="b8016-129">強制回應的快顯視窗會回傳 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b8016-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b8016-130">[上一頁](using-modalpopup-with-a-repeater-control-vb.md)
> [下一頁](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b8016-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
