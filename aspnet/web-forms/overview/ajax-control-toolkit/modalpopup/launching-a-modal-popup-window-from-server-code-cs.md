---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: 啟動 從伺服器程式碼 (C#) 強制回應的快顯視窗 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 但是某些情況下會需要該 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>啟動強制回應的快顯視窗，從伺服器程式碼 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 不過，某些情況下需要開啟強制回應的快顯視窗會觸發伺服器端上。


## <a name="overview"></a>總覽

AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 不過，某些情況下需要開啟強制回應的快顯視窗會觸發伺服器端上。

## <a name="steps"></a>步驟

首先，ASP.NET 按鈕 web 控制項才能示範 ModalPopup 控制的運作方式。 將這類按鈕內加入&lt;表單&gt;新頁面上的元素：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

然後，您必須標記您想要建立快顯功能表。 它定義為`<asp:Panel>`控制項，並請確定它包含按鈕控制項。 ModalPopup 控制項提供的功能，使這類按鈕來關閉快顯視窗。否則便沒有簡單的方法，讓它會消失。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

接下來將 ASP.NET AJAX toolkit ModalPopup 控制項加入頁面。 設定屬性載入控制項的按鈕、 [] 按鈕便會消失，以及實際的快顯視窗的識別碼。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

如同所有 ASP.NET AJAX; 為基礎的 web 網頁指令碼管理員，才能載入必要的 JavaScript 程式庫，針對不同的目標瀏覽器：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

在瀏覽器中執行範例。 當您按一下按鈕，強制回應的快顯視窗出現。 為了達到使用伺服器端程式碼相同的效果，新的按鈕，則需要：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

如您所見，按一下按鈕會產生回傳，並且執行`ServerButton_Click()`在伺服器上的方法。 這種方法呼叫的 JavaScript 函式`launchModal()`執行很精確，JavaScript 函式會執行一次載入頁面：

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

作業的`launchModal()`是顯示 ModalPopup。 `launchModal()`完整的 HTML 頁面已載入後，會執行函式。 在該時間點，不過，ASP.NET AJAX framework 尚未完全載入。 因此，`launchModal()`函式只會將 ModalPopup 控制項必須在稍後顯示的變數：

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` JavaScript 函式是執行 ASP.NET AJAX 已完全載入後的特殊函式。 因此我們將程式碼加入這個函數來顯示 ModalPopup 控制項，但是只有`launchModal()`之前已呼叫：

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()`函式尋找具名的項目頁面上，並且預期會做為參數的伺服器端識別碼。 因此，`$find("mpe")`傳回 ModalPopup 控制項的用戶端表示法，則其`show()`方法可讓顯示的快顯視窗。


[![強制回應的快顯視窗出現時按下的按鈕](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

強制回應的快顯視窗出現時的按鈕按下 ([按一下以檢視完整大小的影像](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](using-modalpopup-with-a-repeater-control-cs.md)
