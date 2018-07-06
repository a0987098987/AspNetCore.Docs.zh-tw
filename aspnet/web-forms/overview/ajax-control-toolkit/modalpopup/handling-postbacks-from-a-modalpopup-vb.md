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
<a name="handling-postbacks-from-a-modalpopup-vb"></a>處理來自 ModalPopup (VB) 的回傳
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 從快顯視窗內建立回傳時，務必特別注意。


## <a name="overview"></a>總覽

AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 從快顯視窗內建立回傳時，務必特別注意。

## <a name="steps"></a>步驟

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

接下來，加入做為強制回應快顯面板。 使用者可以輸入的名稱和電子郵件地址。 按鈕來關閉快顯視窗，並儲存資訊。 請注意，`OnClick`屬性設定，才按一下此按鈕時，就會發生回傳：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

網頁本身包含兩個標籤的完全相同的資訊： 名稱和電子郵件地址。 按鈕用來觸發強制回應快顯視窗中：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

若要顯示快顯視窗，新增`ModalPopupExtender`控制項。 設定`PopupControlID`面板的 ID 屬性和`TargetControlID`按鈕的識別碼：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

現在每當`Save`強制回應快顯視窗中按一下按鈕時，伺服器端`SaveData()`執行方法。 您無法將輸入的資料儲存的資料存放區。 為了簡單起見，新的資料只會在標籤中輸出：

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

此外，強制回應快顯視窗中的將文字方塊控制項應該填入目前的名稱和電子郵件。 不過這需要時才回傳，就會發生。 回傳時，ASP.NET viewstate 功能會自動填滿的文字方塊，以適當的值。

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![強制回應快顯造成回傳](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

強制回應快顯造成回傳 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-modalpopup-with-a-repeater-control-vb.md)
> [下一頁](positioning-a-modalpopup-vb.md)
