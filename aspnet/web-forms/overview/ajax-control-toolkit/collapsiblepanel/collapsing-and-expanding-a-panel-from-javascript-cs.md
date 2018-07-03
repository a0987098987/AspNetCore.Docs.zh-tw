---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: 摺疊與展開面板從 JavaScript (C#) |Microsoft Docs
author: wenz
description: CollapsiblePanel 控制項在 ASP.NET AJAX Control Toolkit 擴充一個面板，並提供它摺疊其內容，並將其展開的能力...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 85a9c6e6cc56cc563eefea6bc863950ba71149ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402752"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>摺疊與展開面板從 JavaScript (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> CollapsiblePanel 控制項在 ASP.NET AJAX Control Toolkit 擴充面板，並提供它的功能，可將摺疊其內容，並再次將其展開。 從自訂的 JavaScript 程式碼，也會觸發這兩個動作。


## <a name="overview"></a>總覽

CollapsiblePanel 控制項在 ASP.NET AJAX Control Toolkit 擴充面板，並提供它的功能，可將摺疊其內容，並再次將其展開。 從自訂的 JavaScript 程式碼，也會觸發這兩個動作。

## <a name="steps"></a>步驟

首先，建立新的 ASP.NET 網頁，並包含`ScriptManager`檔案內`<form>`項目。 這會載入控制項工具組所需的 ASP.NET AJAX 程式庫：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

如此可看到的展開/摺疊的效果，接著，建立內含一些文字面板：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

如您所見，面板會參照 CSS 類別如下所示 （而且基本上會定義的背景色彩和面板的寬度）：

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender`控制項需要`TargetControlID`屬性，讓此工具組知道摺疊或展開收到要求時的面板：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

不幸的是，擴充項目前不會公開特定 API 的摺疊或展開面板中，但某些未記載的方法會執行。 首先，將三個 HTML 按鈕加入頁面，其會接著觸發用戶端 JavaScript 來摺疊或展開面板的內容：

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

在用戶端 JavaScript 程式碼 (開始使用`<script type="text/javascript">`)，則`$find()`方法必須用來存取`CollapsiblePanelExtender`。 `$find("cpe")` 會傳回它的參考。 從該處開始，特定的方法將會解決手邊的工作。

此方法為開啟 （展開） [面板] 中稱為`_doOpen()`; 下列程式碼會實作`doOpen()`按一下第一個按鈕時呼叫的函式：

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

關閉，或摺疊面板，`_doClose()`方法需要執行。 因此當使用者按一下第二個按鈕上時，會呼叫下列 JavaScript 程式碼：

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

第三個按鈕切換面板的狀態： 從摺疊成展開，反之亦然。 `CollapsiblePanelExtender`公開`toggle()`方法，這個方法： 反轉面板的狀態。 不過另外還有另一種方法 (這會在內部使用`toggle()`方法):`get_Collapsed()`方法`CollapsiblePanelExtender()`告訴我們是否或不摺疊面板。 根據此函式的傳回值，[面板] 中則是展開 (`_doOpen()`方法) 或摺疊 (`_doClose()`) 方法：

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![第三個按鈕變更面板的狀態： 從摺疊以展開和後端](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

第三個按鈕變更面板的狀態： 從摺疊以展開和後端 ([按一下以檢視完整大小的影像](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](collapsing-and-expanding-a-panel-from-javascript-vb.md)
