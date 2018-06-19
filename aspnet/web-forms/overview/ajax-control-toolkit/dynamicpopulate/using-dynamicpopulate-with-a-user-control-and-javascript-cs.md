---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: 使用 JavaScript (C#) 的使用者控制項與 DynamicPopulate |Microsoft 文件
author: wenz
description: 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標上的控制項 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: cced645733375de7ab6235efa46b8d20ed262e50
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879565"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>使用 DynamicPopulate 使用者控制和 JavaScript (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標控制項在頁面上，如果沒有頁面重新整理。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。 不過特別注意有 extender 位於使用者控制項時。


## <a name="overview"></a>總覽

`DynamicPopulate` ASP.NET AJAX Control Toolkit 中的控制呼叫 web 服務 （或頁面的方法），並填入目標控制項在頁面上，如果沒有頁面重新整理所產生的值。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。 不過特別注意有 extender 位於使用者控制項時。

## <a name="steps"></a>步驟

首先，您需要 ASP.NET Web 服務會實作由呼叫方法`DynamicPopulateExtender`控制項。 Web 服務實作的方法`getDate()`，必須要有一個引數之字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一個片段的內容資訊與每個 web 服務呼叫。 下列程式碼 (檔`DynamicPopulate.cs.asmx`) 抓取目前的日期，三種格式之一：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

在下一個步驟中，建立新的使用者控制項 (`.ascx`檔案)、 說明其第一個列中的下列宣告：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

A &lt; `label` &gt;項目會用來顯示來自伺服器的資料。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

也在使用者控制項檔案中，我們將使用三個選項按鈕，每一個都代表其中一個 web 服務所支援的三個可能的日期格式。 當使用者按一下其中一個選項按鈕時，瀏覽器將會執行 JavaScript 程式碼看起來像這樣：

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

此程式碼會存取`DynamicPopulateExtender`（不必擔心奇怪的識別碼，但這會在稍後說明），並且觸發動態的母體擴展的資料。 目前的選擇鈕，內容中`this.value`指的是其值為`format1`，`format2`或`format3`完全 web 方法所預期的內容。

尚未遺漏使用者控制項中的唯一是`DynamicPopulateExtender`連結至 web 服務的選項按鈕控制項。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

一次您可能注意到控制項中所使用的奇怪 ID:`mcd1$myDate`而不是`myDate`。 先前，JavaScript 程式碼使用`mcd1_dpe1`存取`DynamicPopulateExtender`而不是`dpe1`。此命名策略是特殊的需求，使用時`DynamicPopulateExtender`使用者控制項內。 此外，您必須將使用者要針對內嵌在特定的方式，讓所有工作。 建立新的 ASP.NET 網頁，並註冊您已實作的使用者控制項的標記前置詞：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

然後，包含 ASP.NET AJAX`ScriptManager`新網頁上的控制項：

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

最後，將使用者控制項加入至頁面。 您只需要設定其`ID`屬性 (和`runat="server"`當然)，但是您也必須將它設定為特定的名稱：`mcd1`因為這是在使用者控制項用來存取使用 JavaScript 的前置詞。

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

就是這麼容易！ 頁面在如預期般運作： 使用者按一下其中一個選項按鈕，此工具組中的控制項呼叫 web 服務和所需的格式顯示目前的日期。


[![選項按鈕位於使用者控制項](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

選項按鈕位於使用者控制項 ([按一下以檢視完整大小的影像](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](dynamically-populating-a-control-using-javascript-code-cs.md)
> [下一頁](dynamically-populating-a-control-vb.md)
