---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: 以動態方式新增 Accordion 窗格 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常宣告 w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 555a8a5cdd701bba3d41c3ff3313fe119a350e53
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381528"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>以動態方式新增 Accordion 窗格 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> 在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面中，但伺服器端程式碼可用來達到相同的結果。


## <a name="overview"></a>總覽

在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面中，但伺服器端程式碼可用來達到相同的結果。

## <a name="steps"></a>步驟

Accordion 控制項公開 （expose) 所有重要的屬性，以伺服器端程式碼。 除此之外，`Panes`屬性授與存取權的組成 Accordion 窗格集合。 沒有類型的每個窗格`AccordionPane`。 因此，它為 trivial 建立這類的窗格：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer`的屬性`AccordionPane`提供的存取權 窗格中，標頭區段內的 ASP.NET 控制項`ContentContainer`屬性`AccordionPane`對窗格的 內容 區段執行相同作業。 這可讓 ASP.NET 程式碼，將內容新增至窗格：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

最後，您必須將窗格新增至`Panes`Accordion 的集合：

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

以下是完整的伺服器端程式碼新增至 Accordion 控制項的兩個窗格：

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

唯一的遺漏項目是 Accordion 本身，這取決於是否存在 ASP.NET`ScriptManager`控制項：

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

若要完成此範例，兩個 Accordion 控制項中所參考的 CSS 類別會提供瀏覽器的樣式資訊：

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![伺服器端程式碼已以動態方式新增 accordion 中的資料](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

伺服器端程式碼已以動態方式新增 accordion 中的資料 ([按一下以檢視完整大小的影像](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](databinding-to-an-accordion-cs.md)
> [下一頁](databinding-to-an-accordion-vb.md)
