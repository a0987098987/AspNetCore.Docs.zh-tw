---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: 建立互斥的核取方塊 (C#) |Microsoft Docs
author: wenz
description: 只有其中一組選項可能會被選取時，通常用選項按鈕。 不過是一項缺點，： 一旦選取一個選項按鈕群組中的，...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d80771081a7aefefa115e68827c52092c6559bb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832029"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>建立互斥的核取方塊 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> 只有其中一組選項可能會被選取時，通常用選項按鈕。 不過是一項缺點，： 一旦選取一個選項按鈕群組中的，不能夠取消選取所有選項按鈕。 核取方塊可以隨時取消勾選，不過不會互斥。 本教學課程提供最佳的這兩種方法： 互斥的核取方塊。


## <a name="overview"></a>總覽

只有其中一組選項可能會被選取時，通常用選項按鈕。 不過是一項缺點，： 一旦選取一個選項按鈕群組中的，不能夠取消選取所有選項按鈕。 核取方塊可以隨時取消勾選，不過不會互斥。 本教學課程提供最佳的這兩種方法： 互斥的核取方塊。

## <a name="steps"></a>步驟

ASP.NET AJAX Control Toolkit 包含 MutuallyExclusiveCheckBox 擴充項。 這可讓程式設計人員指派至群組名稱的任何核取方塊 (`Key`屬性)。 從相同群組內的所有核取方塊，只有一個可選取一次。

讓我們開始將新的 ASP.NET 網頁上的兩個核取方塊。 可以有多個，但其中兩個足以示範原則：

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

針對這兩個核取方塊，則必須將 MutuallyExclusiveCheckBoxExtender 控制項放在頁面上。 這兩個索引鍵屬性必須有相同的值，就如同 HTML 選項按鈕項目的屬性必須是代表其所屬的群組相同的值。 擴充項的 TargetControlID 屬性指向的核取方塊的識別碼。

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

最後，包括 ASP.NET AJAX`ScriptManager`所需的 ASP.NET AJAX Control Toolkit 中的所有項目：

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

儲存並執行的頁面： 您可以檢查，並取消核取這兩個核取方塊，但是沒有這兩個核取方塊可檢查。


[![只有一個核取方塊可以檢查一次](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

只有一個核取方塊可以檢查一次 ([按一下以檢視完整大小的影像](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](creating-mutually-exclusive-checkboxes-vb.md)
