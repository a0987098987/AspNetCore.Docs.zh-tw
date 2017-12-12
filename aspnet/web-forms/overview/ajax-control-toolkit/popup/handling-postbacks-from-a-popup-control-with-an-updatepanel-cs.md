---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: "處理來自 UpdatePanel (C#) 的快顯視窗控制項的回傳 |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 特別注意，必須採取..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 2abcf381aa95ad10d2f36e72d1be64c70fb7d9e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>處理回傳從快顯視窗控制項與 UpdatePanel (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 快顯內發生回傳時必須特別注意。


## <a name="overview"></a>概觀

AJAX Control Toolkit PopupControl extender 提供簡單的方式來啟動任何其他控制項時，觸發快顯視窗。 快顯內發生回傳時必須特別注意。

## <a name="steps"></a>步驟

當使用`PopupControl`回傳，`UpdatePanel`導致回傳所造成的頁面重新整理。 下列標記會定義幾個重要的項目：

- A`ScriptManager`控制，好讓 ASP.NET AJAX Control Toolkit 的運作方式
- 兩個`TextBox`控制項，其會同時觸發快顯視窗
- A`Panel`將做為快顯視窗的控制項
- 面板中`Calendar`控制項內嵌在`UpdatePanel`控制項
- 兩個`PopupControlExtender`指派 [面板] 中，在文字方塊中的控制項

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

請注意，`OnSelectionChanged`屬性`Calendar`控制項設定。 因此當使用者選取內行事曆日期，就會發生回傳和伺服器端方法`c1_SelectionChanged()`執行。 在該方法中，必須擷取並寫回至文字方塊中目前的日期。

語法，如下所示： 首先，proxy 物件`PopupControlExtender`頁面上，必須產生。 ASP.NET AJAX Control Toolkit 提供`GetProxyForCurrentPopup()`方法。 這個方法會傳回的物件支援`Commit()`方法可將值傳送至觸發的快顯視窗 （不是控制項觸發方法呼叫 ！） 的控制項。 下列程式碼提供做為引數的選取的日期`Commit()`方法，造成回寫至文字方塊中的所選的日期的程式碼：

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

現在每當您按一下在行事曆日期，將選取的日期會出現在相關聯的文字方塊中，建立日期選擇器控制項可以目前找到許多網站上。


[![當使用者按一下的文字方塊中，就會出現日曆](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

當使用者按一下的文字方塊中，就會出現日曆 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![按一下日期將它放在文字方塊中](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

按一下日期將它放在文字方塊中 ([按一下以檢視完整大小的影像](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[上一頁](using-multiple-popup-controls-cs.md)
[下一頁](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
