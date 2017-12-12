---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: "建立互斥的核取方塊 (VB) |Microsoft 文件"
author: wenz
description: "只有其中一個的一組選項可能會被選取時，通常用選項按鈕。 雖然會有缺點： 一旦選取一個選項按鈕群組中的，..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 023ca0b145de8147a98e78f4dba20846dc344f06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>建立互斥的核取方塊 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> 只有其中一個的一組選項可能會被選取時，通常用選項按鈕。 雖然會有缺點： 一旦選取一個選項按鈕群組中的，不可能取消選取所有選項按鈕。 核取方塊可以是未檢查在任何時間，但是不會互斥。 本教學課程提供了最佳的這兩種方法： 互斥的核取方塊。


## <a name="overview"></a>概觀

只有其中一個的一組選項可能會被選取時，通常用選項按鈕。 雖然會有缺點： 一旦選取一個選項按鈕群組中的，不可能取消選取所有選項按鈕。 核取方塊可以是未檢查在任何時間，但是不會互斥。 本教學課程提供了最佳的這兩種方法： 互斥的核取方塊。

## <a name="steps"></a>步驟

ASP.NET AJAX Control Toolkit 包含 MutuallyExclusiveCheckBox 擴充項。 這可讓程式設計人員指派至群組名稱的任何核取方塊 (`Key`屬性)。 從同一個群組內的所有核取方塊，只有一個可選取一次。

讓我們開始新的 ASP.NET 網頁上將兩個核取方塊。 可以有多個，但其中兩個已足夠來示範原則：

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

這兩個核取方塊，進行 MutuallyExclusiveCheckBoxExtender 控制項必須放在頁面上。 這兩個索引鍵屬性需要有相同的值，如同 HTML 選項按鈕項目的屬性必須是代表其所屬的群組相同的值。 擴充項的 TargetControlID 屬性指向的核取方塊的識別碼。

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

最後，包含 ASP.NET AJAX`ScriptManager`所需的 ASP.NET AJAX Control Toolkit 的所有項目：

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

儲存並執行頁面： 您可以檢查並取消核取這兩個核取方塊，但是沒有這兩個核取方塊能夠檢查。


[![只有一個核取方塊可檢查一次](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

只有一個核取方塊可檢查一次 ([按一下以檢視完整大小的影像](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一步](creating-mutually-exclusive-checkboxes-cs.md)
