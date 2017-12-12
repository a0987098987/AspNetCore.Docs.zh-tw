---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: "操作 DropShadow 屬性，用戶端程式碼 (C#) |Microsoft 文件"
author: wenz
description: "自訂資料清單的編輯介面"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 59f7d4610ce610ef4357510f0e861f107278b5da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>操作 DropShadow 屬性，用戶端程式碼 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。


## <a name="overview"></a>概觀

AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 此擴充項的屬性也可以使用用戶端 JavaScript 程式碼會變更。

## <a name="steps"></a>步驟

程式碼開頭包含幾行文字的面板：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

相關聯的 CSS 類別可讓面板不錯的背景色彩：

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender`加入至擴充下拉式陰影效果，而不透明度設定為 50%的面板：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

然後，ASP.NET AJAX`ScriptManager`控制項可讓控制項在工作的工具組：

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

另一個面板包含兩個設定陰影的不透明度的 JavaScript 連結： 減號連結則會減少陰影的不透明度，加上的連結就會增加它。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

JavaScript 函式`changeOpacity()`然後必須先找到`DropShadowExtender`頁面上的控制項。 定義 ASP.NET AJAX`$find()`完全該工作的方法。 然後，`get_Opacity()`方法會擷取目前的不透明度，`set_Opacity()`方法會設定它。 JavaScript 程式碼再將目前的不透明度值放`<label>`項目：

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![在用戶端上變更不透明度](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

在用戶端上變更不透明度 ([按一下以檢視完整大小的影像](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](adjusting-the-z-index-of-a-dropshadow-cs.md)
[下一頁](adjusting-the-z-index-of-a-dropshadow-vb.md)
