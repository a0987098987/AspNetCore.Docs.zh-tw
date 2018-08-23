---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: 以動態方式填入控制項，使用 JavaScript 程式碼 (C#) |Microsoft Docs
author: wenz
description: DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿至 t 的目標控制項...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e8b49f41f132cc31ca458ce0af3b74dbb54f225e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825779"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>以動態方式填入控制項，使用 JavaScript 程式碼 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。


## <a name="overview"></a>總覽

`DynamicPopulate`中 ASP.NET AJAX Control Toolkit 控制項呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。

## <a name="steps"></a>步驟

首先，您必須實作呼叫方法的 ASP.NET Web 服務`DynamicPopulateExtender`控制項。 Web 服務實作的方法`getDate()`它需要一個引數的字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一段內容資訊與每個 web 服務呼叫。 以下是程式碼 (檔案`DynamicPopulate.cs.asmx`) 表示擷取目前的日期，三種格式之一：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

在下一個步驟中，建立新的 ASP.NET 網站，並開始使用 ASP.NET AJAX ScriptManager 控制項：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

然後，新增一個 label 控制項 (例如使用 HTML 控制項的相同的名稱，或`<asp:Label />`web 控制項) 這稍後會顯示 web 服務呼叫的結果。

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

接下來，包括`DynamicPopulateExtender`控制，並提供 web 服務的資訊，目標控制項，但不是會觸發這會在稍後完成使用自訂 JavaScript 的母體擴展的控制項名稱 ！

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

現在給的 JavaScript 部分。 `$find()` ASP.NET AJAX 程式庫所定義的函數傳回的 ASP.NET AJAX Control Toolkit 中的伺服器端物件的參考例如`DynamicPopulateExtender`。 在目前的檔案中，`$find("dpe")`傳回一個參考`DynamicPopulateExtender`頁面中的控制項。 它會公開一個方法，叫做`populate()`哪些觸發程序動態擴展處理程序。 `populate()`方法需要一個引數： 這將做為引數的內容金鑰`getDate()`web 方法。 比方說，`$find("dpe").populate("format1")`會填入與目前的日期，格式為月-日-年的標籤。

為了讓範例更有彈性，使用者現在可能會選擇數種日期格式。 針對其中每個顯示的選項按鈕。 一次使用者按下選項按鈕，JavaScript 程式碼以動態方式填入的標籤與選取的日期格式。 以下是這些選項按鈕：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

請注意，內容中的選項按鈕，JavaScript 運算式`this.value`剛好是完全相同的資訊 [目前] 按鈕的值是指`getDate()`方法可以使用。


[![按一下按鈕從伺服器中所指定的格式擷取的日期](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

按一下按鈕從伺服器中所指定的格式擷取的日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](dynamically-populating-a-control-cs.md)
> [下一頁](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
