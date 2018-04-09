---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: 調整 DropShadow (C#) Z-索引 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 不過此陰影有時會與其他控制項，如 insta 衝突...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 82add8427c8e574b213b67315e69bb4c28846095
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>調整目的 Z-索引 DropShadow (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 不過此陰影有時會與其他控制項，例如 ASP.NET 功能表控制項衝突。 當功能表項目快顯，它會出現在下拉式陰影後面。


## <a name="overview"></a>總覽

AJAX Control Toolkit DropShadow 控制項擴充有陰影的面板。 不過此陰影有時會與其他控制項，例如 ASP.NET 功能表控制項衝突。 當功能表項目快顯，它會出現在下拉式陰影後面。

## <a name="steps"></a>步驟

程式碼便會開始面板本身，使面板包含足夠的效果皆可看到的文字包含足夠的文字：

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

另一個面板直接之前放置`panelShadow`面板。 包含具有水平方向的功能表，使上方會出現的功能表項目 (否則： 底下)`dropShadow`面板):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

然後，`DropShadowExtender`加入至擴充`panelShadow`面板下拉式陰影效果：

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

最後，ASP.NET AJAX`ScriptManager`控制項可讓控制項在工作的工具組：

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

當您執行此指令碼時，則會在面板下方出現的功能表項目。 不過功能表使用的 CSS 類別`panel`您只需要定義使項目會出現在其他面板前面兩件事：

- 相對位置
- 正數 z-索引

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

然後，`DropShadowExtender`控制項未與此功能表控制項不再衝突。


[![事前： 功能表項目看不到](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

事前： 功能表項目看不到 ([按一下以檢視完整大小的影像](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![功能表項目會出現在之後：](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

之後： 會出現功能表項目 ([按一下以檢視完整大小的影像](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [下一步](manipulating-dropshadow-properties-from-client-code-cs.md)
