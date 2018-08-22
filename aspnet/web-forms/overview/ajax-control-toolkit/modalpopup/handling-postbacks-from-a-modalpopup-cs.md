---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: 處理回傳來自 ModalPopup (C#) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 必須採取特別注意，當 pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 9bcb1ad058b800f3f957cafff07a5a54b44178a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830211"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>處理回傳來自 ModalPopup (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 從快顯視窗內建立回傳時，務必特別注意。


## <a name="overview"></a>總覽

AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 從快顯視窗內建立回傳時，務必特別注意。

## <a name="steps"></a>步驟

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

接下來，加入做為強制回應快顯面板。 使用者可以輸入的名稱和電子郵件地址。 按鈕來關閉快顯視窗，並儲存資訊。 請注意，`OnClick`屬性設定，才按一下此按鈕時，就會發生回傳：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

網頁本身包含兩個標籤的完全相同的資訊： 名稱和電子郵件地址。 按鈕用來觸發強制回應快顯視窗中：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

若要顯示快顯視窗，新增`ModalPopupExtender`控制項。 設定`PopupControlID`面板的 ID 屬性和`TargetControlID`按鈕的識別碼：

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

現在每當`Save`強制回應快顯視窗中按一下按鈕時，伺服器端`SaveData()`執行方法。 您無法將輸入的資料儲存的資料存放區。 為了簡單起見，新的資料只會在標籤中輸出：

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

此外，強制回應快顯視窗中的將文字方塊控制項應該填入目前的名稱和電子郵件。 不過這需要時才回傳，就會發生。 回傳時，ASP.NET viewstate 功能會自動填滿的文字方塊，以適當的值。

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![強制回應快顯造成回傳](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

強制回應快顯造成回傳 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-modalpopup-with-a-repeater-control-cs.md)
> [下一頁](positioning-a-modalpopup-cs.md)
