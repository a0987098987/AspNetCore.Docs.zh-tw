---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: 處理沒有 UpdatePanel (C#) 的快顯視窗控制項回傳 |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 當發生回傳時 su 中...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 108595a37d6c15a4bd6ddee365ca718969ca9c8c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804031"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>處理沒有 UpdatePanel (C#) 的快顯視窗控制項回傳
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 回傳，就會發生這類面板中，並在頁面上有數個面板時很難判斷按下的面板。


## <a name="overview"></a>總覽

PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 回傳，就會發生這類面板中，並在頁面上有數個面板時很難判斷按下的面板。

## <a name="steps"></a>步驟

使用時`PopupControl`回傳，但無須`UpdatePanel`在頁面上，Control Toolkit 並未提供一個方式來判斷哪一個用戶端項目已觸發的快顯視窗接著造成回傳。 不過小秘訣提供此案例的因應措施。

首先，以下是基本安裝： 這兩個觸發程序相同的快顯行事曆的兩個文字方塊。 兩個`PopupControlExtenders`結合文字方塊和快顯視窗。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

基本構想是將隱藏的表單欄位中加入&lt; `form` &gt;保留啟動快顯文字方塊中的項目：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

當網頁載入時，JavaScript 程式碼會將事件處理常式加入至這兩個文字方塊： 每當使用者按一下文字方塊中，其名稱會寫入其中的隱藏的欄位：

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

在伺服器端程式碼中，您必須讀取隱藏欄位的值。 隱藏的表單欄位是 trivial 操作，因為驗證隱藏的值的允許清單方法需要。 一旦已識別正確的文字方塊中，從行事曆的日期會寫入到其中。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![當使用者按一下文字方塊，即出現日曆](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![按一下某個日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

按一下某個日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [上一頁](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [下一頁](using-multiple-popup-controls-vb.md)
