---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: 使用具有 DynamicPopulate 使用者控制和 JavaScript (VB) |Microsoft Docs
author: wenz
description: DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿至 t 的目標控制項...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: a275eed17552d26b63f98762c6c870bd53dd455d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826218"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>使用具有 DynamicPopulate 使用者控制和 JavaScript (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。 不過，特別注意具有擴充項位於使用者控制項時所要採取。


## <a name="overview"></a>總覽

`DynamicPopulate`中 ASP.NET AJAX Control Toolkit 控制項呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。 不過，特別注意具有擴充項位於使用者控制項時所要採取。

## <a name="steps"></a>步驟

首先，您必須實作呼叫方法的 ASP.NET Web 服務`DynamicPopulateExtender`控制項。 Web 服務實作的方法`getDate()`它需要一個引數的字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一段內容資訊與每個 web 服務呼叫。 以下是程式碼 (檔案`DynamicPopulate.vb.asmx`) 表示擷取目前的日期，三種格式之一：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

在下一個步驟中，建立新的使用者控制項 (`.ascx`檔案)，表示其第一行中的下列宣告：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

A &lt; `label` &gt;項目會用來顯示來自伺服器的資料。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

也在使用者控制項檔案中，我們將使用三個選項按鈕，每一個都代表其中一個 web 服務所支援的三個可能的日期格式。 當使用者按一下其中一個選項按鈕時，瀏覽器會執行 JavaScript 程式碼看起來像這樣：

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

這個程式碼存取`DynamicPopulateExtender`（不必擔心奇怪的識別碼，但這將在稍後說明），並觸發動態的母體擴展的資料。 目前的選項按鈕，內容中`this.value`也就是其值是指`format1`，`format2`或`format3`完全 web 方法所預期的內容。

唯一還缺少使用者控制項中的事項是`DynamicPopulateExtender`連結至 web 服務的選項按鈕控制項。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

一次您可能注意到控制項中所使用的奇怪識別碼：`mcd1$myDate`而不是`myDate`。 先前，JavaScript 程式碼使用`mcd1_dpe1`若要存取`DynamicPopulateExtender`而不是`dpe1`。使用時，此命名策略是特殊的需求`DynamicPopulateExtender`使用者控制項內。 此外，您必須將使用者要內嵌到正常運作以特定方式。 建立新的 ASP.NET 網頁，並註冊您已實作的使用者控制項的標記前置詞：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

然後，包含 ASP.NET AJAX`ScriptManager`新網頁上的控制項：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

最後，新增至頁面的使用者控制項。 您只需要設定其`ID`屬性 (並`runat="server"`當然)，但是您也必須將它設定為特定的名稱：`mcd1`由於這是使用者控制項內，用於使用 JavaScript 進行存取的前置詞。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

就是這麼容易！ 頁面的行為如預期般運作： 使用者按一下其中一個選項按鈕，此工具組中的控制項呼叫 web 服務，並顯示目前的日期所需的格式。


[![選項按鈕位於使用者控制項](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

選項按鈕位於使用者控制項 ([按一下以檢視完整大小的影像](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](dynamically-populating-a-control-using-javascript-code-vb.md)
