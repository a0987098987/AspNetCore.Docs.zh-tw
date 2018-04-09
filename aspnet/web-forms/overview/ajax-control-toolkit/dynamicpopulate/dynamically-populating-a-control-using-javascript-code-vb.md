---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: 以動態方式填入控制項，使用 JavaScript 程式碼 (VB) |Microsoft 文件
author: wenz
description: 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標上的控制項 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>以動態方式填入控制項，使用 JavaScript 程式碼 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標控制項在頁面上，如果沒有頁面重新整理。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。


## <a name="overview"></a>總覽

`DynamicPopulate` ASP.NET AJAX Control Toolkit 中的控制呼叫 web 服務 （或頁面的方法），並填入目標控制項在頁面上，如果沒有頁面重新整理所產生的值。 它也可觸發程序使用自訂用戶端 JavaScript 程式碼的母體擴展。

## <a name="steps"></a>步驟

首先，您需要 ASP.NET Web 服務會實作由呼叫方法`DynamicPopulateExtender`控制項。 Web 服務實作的方法`getDate()`，必須要有一個引數之字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一個片段的內容資訊與每個 web 服務呼叫。 下列程式碼 (檔`DynamicPopulate.vb.asmx`) 抓取目前的日期，三種格式之一：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

在下一個步驟中，建立新的 ASP.NET 網站，並開始使用 ASP.NET AJAX ScriptManager 控制項：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

然後，將標籤控制項 (例如使用 HTML 控制項的名稱相同，或`<asp:Label />`web 控制項) 的稍後將會顯示 web 服務呼叫的結果。

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

接下來，包括`DynamicPopulateExtender`控制，並提供網頁服務資訊、 目標控制項，但不是會觸發這將在稍後完成使用自訂 JavaScript 的母體擴展的控制項名稱 ！

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

現在給的 JavaScript 部分。 `$find()`函式，由 ASP.NET AJAX 程式庫中，將參考傳回給伺服器端物件的 ASP.NET AJAX Control Toolkit 例如`DynamicPopulateExtender`。 在目前的檔案中，`$find("dpe")`傳回一個參考`DynamicPopulateExtender`網頁中的控制項。 它會公開方法，稱為`populate()`此觸發程序動態擴展處理程序。 `populate()`方法需要一個引數： 將做為引數的內容金鑰`getDate()`web 方法。 例如，`$find("dpe").populate("format1")`會填入與目前的日期，格式為月-日-年的標籤。

為了讓範例更有彈性，使用者現在可以選擇數個日期格式之間。 針對其中每個選項按鈕隨即出現。 一次使用者按一下選項按鈕，JavaScript 程式碼動態填入的標籤與選取的日期格式。 以下是這些選項按鈕：

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

請注意，選項按鈕，JavaScript 運算式的內容中`this.value`目前按鈕，這正好是資訊完全相同的值是指`getDate()`方法可以使用。


[![按一下按鈕從伺服器中所指定的格式擷取日期](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

按一下按鈕從伺服器中所指定的格式擷取日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](dynamically-populating-a-control-vb.md)
> [下一頁](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
