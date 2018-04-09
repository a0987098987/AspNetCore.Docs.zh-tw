---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: 處理回傳從快顯視窗控制項沒有 UpdatePanel (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 當回傳 su 中...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>處理回傳從快顯視窗控制項沒有 UpdatePanel (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 回傳發生這類面板中，在頁面上有數個面板時很難判斷已按下的面板。


## <a name="overview"></a>總覽

AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 回傳發生這類面板中，在頁面上有數個面板時很難判斷已按下的面板。

## <a name="steps"></a>步驟

當使用`PopupControl`回傳，但不需要`UpdatePanel`在頁面上，Control Toolkit 不提供一個方式來判斷哪些用戶端項目已經觸發輪流導致回傳快顯視窗。 不過小墩提供此案例中因應措施。

首先，以下是基本安裝： 這兩個觸發程序相同的快顯行事曆的兩個文字方塊。 兩個`PopupControlExtenders`將文字方塊和快顯結合在一起。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

若要加入隱藏的表單欄位中的基本概念是&lt; `form` &gt;保留文字方塊啟動快顯功能表項目：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

當網頁載入時，JavaScript 程式碼會將事件處理常式加入至這兩個文字方塊： 每當使用者在文字方塊中，其名稱會寫入的隱藏的欄位：

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

在伺服器端程式碼中，就必須讀取隱藏欄位的值。 隱藏的表單欄位是 trivial 操作，因為驗證隱藏的值的白名單方法需要。 一旦已識別正確的文字方塊，從行事曆日期會寫入到其中。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![當使用者按一下的文字方塊中，就會出現日曆](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

當使用者按一下的文字方塊中，就會出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![按一下日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

按一下日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [上一步](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
