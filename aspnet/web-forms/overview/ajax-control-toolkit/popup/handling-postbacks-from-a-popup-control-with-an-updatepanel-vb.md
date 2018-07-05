---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: 處理有 Updatepanel (VB) 的快顯視窗控制項回傳 |Microsoft Docs
author: wenz
description: PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 特別注意有進行中...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: cf17025219fbf24e749c172f2ddb47ec20980cdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395898"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>處理有 Updatepanel (VB) 的快顯視窗控制項回傳
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 特別注意有進行這類快顯視窗內回傳時發生。


## <a name="overview"></a>總覽

PopupControl 擴充項在 AJAX Control Toolkit 提供簡單的方式，來啟動任何其他控制項時，觸發快顯視窗。 特別注意有進行這類快顯視窗內回傳時發生。

## <a name="steps"></a>步驟

使用時`PopupControl`回傳時，使用`UpdatePanel`導致回傳所造成的重新整理頁面。 下列標記會定義幾個重要的項目：

- A`ScriptManager`控制項，讓 ASP.NET AJAX Control Toolkit 的運作方式
- 兩個`TextBox`控制項，其會同時觸發快顯視窗
- A`Panel`將做為快顯視窗的控制項
- 在面板中，`Calendar`控制項內嵌在`UpdatePanel`控制項
- 兩個`PopupControlExtender`將面板指派的文字方塊內的控制項

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

請注意，`OnSelectionChanged`屬性的`Calendar`控制項設定。 因此當使用者選取內行事曆的日期，就會發生回傳和伺服器端方法`c1_SelectionChanged()`執行。 在該方法中，目前的日期必須擷取和回寫至文字方塊。

該語法如下所示： 首先，proxy 物件`PopupControlExtender`必須產生頁面上。 ASP.NET AJAX Control Toolkit 提供`GetProxyForCurrentPopup()`方法。 這個方法會傳回的物件支援`Commit()`方法可將值傳送回控制項觸發快顯視窗 （不是控制項觸發方法呼叫 ！）。 下列程式碼提供做為引數所選的日期`Commit()`方法，導致文字方塊中回寫所選的日期的程式碼：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

現在每當您在行事曆日期，按一下選取的日期會出現在相關聯的文字 方塊中，建立日期選擇器控制項目前可在許多網站上。


[![當使用者按一下文字方塊，即出現日曆](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

當使用者按一下文字方塊，即出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![按一下某個日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

按一下某個日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [上一頁](using-multiple-popup-controls-vb.md)
> [下一頁](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
