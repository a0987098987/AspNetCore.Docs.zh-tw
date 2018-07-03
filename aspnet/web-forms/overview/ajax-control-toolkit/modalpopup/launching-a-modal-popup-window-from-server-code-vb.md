---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: 啟動強制回應快顯視窗中的，從伺服器程式碼 (VB) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 不過有些情況下會需要該 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: ea5149e9dece5393bb4c431bfc440a745611496d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362835"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>啟動強制回應快顯視窗中的，從伺服器程式碼 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 不過，某些情況下需要開啟強制回應快顯會觸發伺服器端上。


## <a name="overview"></a>總覽

AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 不過，某些情況下需要開啟強制回應快顯會觸發伺服器端上。

## <a name="steps"></a>步驟

首先，ASP.NET 按鈕 web 控制項，才能示範 ModalPopup 控制的運作方式。 新增這類的按鈕內&lt;表單&gt;新的頁面上的項目：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

然後，您需要的標記您想要建立快顯視窗。 它定義為`<asp:Panel>`控制項，並確定它包含按鈕控制項。 ModalPopup 控制項提供的功能，使這類按鈕來關閉快顯視窗;否則會沒有簡單的方法，讓它消失。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

接下來將從 ASP.NET AJAX Toolkit ModalPopup 控制項加入頁面。 設定屬性載入控制項的按鈕、 按鈕，因此會消失，以及實際的快顯視窗的識別碼。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

如同所有 ASP.NET AJAX; 為基礎的 web 網頁指令碼管理員，才能載入必要的 JavaScript 程式庫，針對不同的目標瀏覽器：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

在瀏覽器中執行範例。 當您按一下按鈕，強制回應快顯視窗隨即出現。 為了達成使用伺服器端程式碼相同的效果，新的按鈕，則需要：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

如您所見，按一下按鈕會產生回傳，並且執行`ServerButton_Click()`伺服器上的方法。 這種方法的 JavaScript 函式呼叫`launchModal()`執行很精確，JavaScript 函式會執行一次載入頁面：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

工作`launchModal()`是顯示 ModalPopup。 `launchModal()`完整的 HTML 頁面載入之後，會執行函式。 在該時間點，不過，ASP.NET AJAX 架構尚未完全載入。 因此，`launchModal()`函式只會將 ModalPopup 控制項必須在稍後顯示的變數：

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` JavaScript 函式是 ASP.NET AJAX 完全載入後執行的特殊函式。 因此我們將程式碼加入這個函數來顯示 ModalPopup 控制項，但是只有`launchModal()`之前已呼叫：

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()`函式會尋找具名項目頁面上，並預期伺服器端 ID，做為參數。 因此，`$find("mpe")`傳回 ModalPopup 控制項的用戶端表示法，其`show()`方法可讓快顯視窗會出現。


[![強制回應快顯視窗出現時按一下的按鈕](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

強制回應快顯視窗出現時按一下任一按鈕時 ([按一下以檢視完整大小的影像](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](positioning-a-modalpopup-cs.md)
> [下一頁](using-modalpopup-with-a-repeater-control-vb.md)
