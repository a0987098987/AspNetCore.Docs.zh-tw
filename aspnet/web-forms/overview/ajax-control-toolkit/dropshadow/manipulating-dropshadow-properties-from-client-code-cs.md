---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: 操作 DropShadow 屬性，從用戶端程式碼 (C#) |Microsoft Docs
author: wenz
description: 自訂 DataList 的編輯介面
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377281"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>操作 DropShadow 屬性，從用戶端程式碼 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。 此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。


## <a name="overview"></a>總覽

DropShadow 控制項在 AJAX Control Toolkit 擴充具有延伸陰影的面板。 此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。

## <a name="steps"></a>步驟

程式碼的開頭包含幾行文字的面板：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

相關聯的 CSS 類別提供不錯的背景色彩的面板：

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender`加入擴充具有延伸陰影效果，設定為 50%的不透明度的面板：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

然後，ASP.NET AJAX`ScriptManager`控制項可讓控制項工具組運作：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

另一個面板包含兩個 JavaScript 連結設定延伸陰影的不透明度： 減號的連結會減少陰影的不透明度，加上連結就會增加它。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

JavaScript 函式`changeOpacity()`則必須先找到`DropShadowExtender`頁面上的控制項。 ASP.NET AJAX 定義`$find()`完全該工作的方法。 然後，`get_Opacity()`方法會擷取目前的不透明度，`set_Opacity()`方法會設定它。 JavaScript 程式碼然後將目前的不透明度值放`<label>`項目：

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![在用戶端上的變更不透明度](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

在用戶端上的變更不透明度 ([按一下以檢視完整大小的影像](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [下一頁](adjusting-the-z-index-of-a-dropshadow-vb.md)
