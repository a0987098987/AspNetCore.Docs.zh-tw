---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: "處理回傳 ModalPopup (VB) |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 必須採取特別注意，當 pos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 7915d555fef363f41aa51bd77f0183c97c2f3769
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>處理回傳 ModalPopup (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 從快顯內建立回傳時，必須採取特別注意。


## <a name="overview"></a>概觀

AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 從快顯內建立回傳時，必須採取特別注意。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

接著，將會做為強制回應的快顯視窗的面板。 那里，使用者可以輸入的名稱和電子郵件地址。 按鈕會關閉快顯視窗，並儲存資訊。 請注意， `OnClick` ，以便在按下此按鈕時，就會發生回傳，設定屬性：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

網頁本身的資訊完全相同的兩個標籤所組成： 名稱和電子郵件地址。 按鈕用來觸發強制回應的快顯視窗：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

為了讓出現快顯視窗，加入`ModalPopupExtender`控制項。 設定`PopupControlID`屬性面板的識別碼和`TargetControlID`按鈕的識別碼：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

現在每當`Save`強制回應的快顯內按一下按鈕時，伺服器端`SaveData()`執行方法。 您無法將輸入的資料儲存在資料存放區。 為了簡單起見，新的資料只會輸出在標籤中：

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

此外，強制回應的快顯內的文字方塊控制項應填入目前的名稱和電子郵件。 不過這是只有必要回傳發生時。 如果沒有回傳，ASP.NET viewstate 功能會自動填滿的適當值填入文字方塊。

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![強制回應的快顯視窗會回傳](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

強制回應的快顯視窗會回傳 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](using-modalpopup-with-a-repeater-control-vb.md)
[下一頁](positioning-a-modalpopup-vb.md)
