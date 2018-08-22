---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: 處理沒有 UpdatePanel (VB) 的快顯視窗控制項回傳 |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 當發生回傳時 su 中...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dcb2b09279ec6400465f79fadc2a1b6c72f8f07
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826819"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>處理沒有 UpdatePanel (VB) 的快顯視窗控制項回傳
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 回傳，就會發生這類面板中，並在頁面上有數個面板時很難判斷按下的面板。


## <a name="overview"></a>總覽

PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 回傳，就會發生這類面板中，並在頁面上有數個面板時很難判斷按下的面板。

## <a name="steps"></a>步驟

使用時`PopupControl`回傳，但無須`UpdatePanel`在頁面上，Control Toolkit 並未提供一個方式來判斷哪一個用戶端項目已觸發的快顯視窗接著造成回傳。 不過小秘訣提供此案例的因應措施。

首先，以下是基本安裝： 這兩個觸發程序相同的快顯行事曆的兩個文字方塊。 兩個`PopupControlExtenders`結合文字方塊和快顯視窗。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

基本構想是將隱藏的表單欄位中加入&lt; `form` &gt;保留啟動快顯文字方塊中的項目：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

當網頁載入時，JavaScript 程式碼會將事件處理常式加入至這兩個文字方塊： 每當使用者按一下文字方塊中，其名稱會寫入其中的隱藏的欄位：

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

在伺服器端程式碼中，您必須讀取隱藏欄位的值。 隱藏的表單欄位是 trivial 操作，因為驗證隱藏的值的允許清單方法需要。 一旦已識別正確的文字方塊中，從行事曆的日期會寫入到其中。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![當使用者按一下文字方塊，即出現日曆](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![按一下某個日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

按一下某個日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [上一步](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
