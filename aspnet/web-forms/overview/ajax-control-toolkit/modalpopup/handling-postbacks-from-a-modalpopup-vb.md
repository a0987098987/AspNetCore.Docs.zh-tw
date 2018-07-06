---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: 處理來自 ModalPopup (VB) 的回傳 |Microsoft Docs
author: wenz
description: AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 必須採取特別注意，當 pos...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: c4c067401f88c0bba7269406d08b7b3857f022d6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804364"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="9c71e-104">處理來自 ModalPopup (VB) 的回傳</span><span class="sxs-lookup"><span data-stu-id="9c71e-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="9c71e-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9c71e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9c71e-106">[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9c71e-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="9c71e-107">AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。</span><span class="sxs-lookup"><span data-stu-id="9c71e-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="9c71e-108">從快顯視窗內建立回傳時，務必特別注意。</span><span class="sxs-lookup"><span data-stu-id="9c71e-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="9c71e-109">總覽</span><span class="sxs-lookup"><span data-stu-id="9c71e-109">Overview</span></span>

<span data-ttu-id="9c71e-110">AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。</span><span class="sxs-lookup"><span data-stu-id="9c71e-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="9c71e-111">從快顯視窗內建立回傳時，務必特別注意。</span><span class="sxs-lookup"><span data-stu-id="9c71e-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="9c71e-112">步驟</span><span class="sxs-lookup"><span data-stu-id="9c71e-112">Steps</span></span>

<span data-ttu-id="9c71e-113">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="9c71e-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="9c71e-114">接下來，加入做為強制回應快顯面板。</span><span class="sxs-lookup"><span data-stu-id="9c71e-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="9c71e-115">使用者可以輸入的名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="9c71e-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="9c71e-116">按鈕來關閉快顯視窗，並儲存資訊。</span><span class="sxs-lookup"><span data-stu-id="9c71e-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="9c71e-117">請注意，`OnClick`屬性設定，才按一下此按鈕時，就會發生回傳：</span><span class="sxs-lookup"><span data-stu-id="9c71e-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="9c71e-118">網頁本身包含兩個標籤的完全相同的資訊： 名稱和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="9c71e-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="9c71e-119">按鈕用來觸發強制回應快顯視窗中：</span><span class="sxs-lookup"><span data-stu-id="9c71e-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="9c71e-120">若要顯示快顯視窗，新增`ModalPopupExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="9c71e-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="9c71e-121">設定`PopupControlID`面板的 ID 屬性和`TargetControlID`按鈕的識別碼：</span><span class="sxs-lookup"><span data-stu-id="9c71e-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="9c71e-122">現在每當`Save`強制回應快顯視窗中按一下按鈕時，伺服器端`SaveData()`執行方法。</span><span class="sxs-lookup"><span data-stu-id="9c71e-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="9c71e-123">您無法將輸入的資料儲存的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="9c71e-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="9c71e-124">為了簡單起見，新的資料只會在標籤中輸出：</span><span class="sxs-lookup"><span data-stu-id="9c71e-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="9c71e-125">此外，強制回應快顯視窗中的將文字方塊控制項應該填入目前的名稱和電子郵件。</span><span class="sxs-lookup"><span data-stu-id="9c71e-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="9c71e-126">不過這需要時才回傳，就會發生。</span><span class="sxs-lookup"><span data-stu-id="9c71e-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="9c71e-127">回傳時，ASP.NET viewstate 功能會自動填滿的文字方塊，以適當的值。</span><span class="sxs-lookup"><span data-stu-id="9c71e-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="9c71e-128">[![強制回應快顯造成回傳](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9c71e-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="9c71e-129">強制回應快顯造成回傳 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9c71e-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c71e-130">[上一頁](using-modalpopup-with-a-repeater-control-vb.md)
> [下一頁](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9c71e-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
