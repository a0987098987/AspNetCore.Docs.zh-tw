---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: "定位 ModalPopup (VB) |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 但是控制項不提供..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9c0790d1696bcf478bcdea089d4d3d92450369
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-vb"></a>定位 ModalPopup (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 但是控制項不提供內建的功能，將快顯視窗。


## <a name="overview"></a>概觀

AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 但是控制項不提供內建的功能，將快顯視窗。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`。 控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

接著，將會做為強制回應的快顯視窗的面板。 按鈕來關閉快顯視窗：

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

每當在快顯視窗會顯示，它應該位於網頁中的特定位置。 這項工作，會建立用戶端 JavaScript 函式。 會先嘗試存取面板。 如果成功，面板的位置會設定使用 CSS 和 JavaScript （在快顯視窗的位置將會的變更）。 不過，`ModalPopupExtender`控制項也會嘗試將快顯視窗。 因此，JavaScript 程式碼重複上面的快顯視窗中，而每個十分之一秒。

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

您可以看到，傳回值`setTimeout()`JavaScript 方法會儲存在全域變數。 這可停止重複的位置快顯視窗，視使用`clearTimeout()`方法：

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

現在所有就是使呼叫這些函式，只要有適當的瀏覽器。 `movePanel()`觸發程序 [面板] 中，按一下此按鈕時，就必須呼叫 JavaScript 函式：

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

和`stopMoving()`函式來自於快顯視窗已關閉，可能會觸發`ModalPopupExtender`控制項：

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![強制回應的快顯視窗會出現在指定的位置](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

強制回應的快顯視窗會出現在指定的位置 ([按一下以檢視完整大小的影像](positioning-a-modalpopup-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一步](handling-postbacks-from-a-modalpopup-vb.md)
