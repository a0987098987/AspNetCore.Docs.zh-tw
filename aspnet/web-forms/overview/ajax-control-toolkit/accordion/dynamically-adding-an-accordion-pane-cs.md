---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: "以動態方式加入 Accordion 窗格 (C#) |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c6af79186ca21082647beec500c47974a47c35a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-c"></a>以動態方式加入 Accordion 窗格 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面內，但伺服器端程式碼可用來達到相同的結果。


## <a name="overview"></a>概觀

AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面內，但伺服器端程式碼可用來達到相同的結果。

## <a name="steps"></a>步驟

Accordion 控制項會公開伺服器端程式碼的所有重要屬性。 在其他方面，`Panes`屬性授與存取權窗格 Accordion 所組成的集合。 沒有類型的每個窗格`AccordionPane`。 因此，它是 trivial 建立這類的窗格：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer`屬性`AccordionPane`提供 ASP.NET 控制項的窗格; 標頭區段內存取`ContentContainer`屬性`AccordionPane`沒有相同的窗格的 [內容] 區段。 這可讓 ASP.NET 程式碼，將內容加入至 [] 窗格：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

最後，窗格必須新增至`Panes`accordion 設定的集合：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

以下是完整的伺服器端程式碼加入 accordion 設定控制項的兩個窗格：

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

只有遺漏的項目是 Accordion 本身，這取決於是否存在的 ASP.NET`ScriptManager`控制項：

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

若要完成此範例，這兩個 accordion 設定控制項中所參考的 CSS 類別會提供瀏覽器的樣式資訊：

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![依伺服器端程式碼以動態方式加入 accordion 設定中的資料](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

依伺服器端程式碼以動態方式加入 accordion 設定中的資料 ([按一下以檢視完整大小的影像](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](databinding-to-an-accordion-cs.md)
[下一頁](databinding-to-an-accordion-vb.md)
