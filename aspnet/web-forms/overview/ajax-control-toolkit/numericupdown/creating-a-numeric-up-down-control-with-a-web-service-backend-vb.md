---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: 建立數值上下按鈕控制項與 Web 服務後端 (VB) |Microsoft 文件
author: wenz
description: 而不是讓使用者輸入的值將核取方塊，向上/向下的控制項 （Windows 和其他作業系統有） 的數值無法證明越多的 c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 690fd89c552407ec5d77419aae2488e4832efe44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871385"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>建立數值向上/向下控制項與 Web 服務後端 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> 而不是讓使用者輸入的值將核取方塊，數值上下按鈕控制項 （也就存在於 Windows 和其他作業系統） 無法證明越多舒適。 根據預設，數值上下按鈕控制項一律會增加，或值，就會減 1，但 web 服務證明更大的彈性。


## <a name="overview"></a>總覽

而不是讓使用者輸入的值將核取方塊，數值上下按鈕控制項 （也就存在於 Windows 和其他作業系統） 無法證明越多舒適。 根據預設，`NumericUpDown`控制項一律會增加，或值，就會減 1，但 web 服務證明更大的彈性。

## <a name="steps"></a>步驟

ASP.NET AJAX Control Toolkit 包含`NumericUpDown`擴充項它會自動加入到文字方塊中的兩個按鈕： 一個用於增加其值用於減少它的其中一個。 不過此控制項也支援 web 服務呼叫 （或網頁方法呼叫）。 每當向上或向下按鈕按下時的 JavaScript 程式碼會連接到 web 伺服器和執行的方法那里。 方法簽章是下列其中一個：

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

`current`引數是目前的值，在文字方塊中;`tag`屬性是其他內容資料的可設定的屬性為`NumericUpDown`擴充項 （但非必要）。

此範例中，數值上下按鈕控制項都應該只允許的 2 乘冪的值： 1、 2、 4、 8、 16、 32、 64、 等等。 因此，當使用者想要增加的值時執行的方法必須 double 的舊值。其他方法必須由兩個分割的值。 因此，以下是完整的 web 服務：

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

最後，建立新的 ASP.NET 網頁。 像往常一樣，您需要`ScriptManager`控制項，`TextBox`控制項和`NumericUpDownExtender`控制項。 對於後者，您必須提供網頁服務資訊：

- `ServiceDownMethod` web 方法或頁面上的方法名稱清單
- `ServiceDownPath` web 服務的向下服務方法的路徑如果您使用頁面方法，省略
- `ServiceUpMethod` 名稱的總 web 方法或網頁方法
- `ServiceUpPath` 路徑 web 服務的最新的服務方法。如果您使用頁面方法，省略

以下是完整的標記頁面：

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

如果您執行頁面，請注意如何在文字方塊中的值一律會加倍當您按一下上方的按鈕，然後當您按一下下方的按鈕會變成一半。


[![顯示只是 2 的乘冪的數字](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

顯示只是 2 的乘冪的數字 ([按一下以檢視完整大小的影像](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
