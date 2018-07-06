---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: 使用 TextBoxWatermark 與驗證控制項 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit TextBoxWatermark 控制延伸的文字方塊，讓文字會顯示在方塊內。 當使用者在方塊中，按一下它我...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 144e4ee8f8096076b4e498844429ebf96e6ea402
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836185"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>使用 TextBoxWatermark 與驗證控制項 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> 在 AJAX Control Toolkit TextBoxWatermark 控制延伸的文字方塊，讓文字會顯示在方塊內。 當使用者按一下方塊時，它會清空。 如果使用者離開不需要輸入文字的方塊中，預先填入的文字隨即再度出現。 這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。


## <a name="overview"></a>總覽

`TextBoxWatermark`中 AJAX Control Toolkit 控制項擴充文字方塊中，使方塊內顯示的文字。 當使用者按一下方塊時，它會清空。 如果使用者離開不需要輸入文字的方塊中，預先填入的文字隨即再度出現。 這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。

## <a name="steps"></a>步驟

基本設定的範例如下：`TextBox`控制項使用浮水印`TextBoxWatermarkExtender`控制項。 按鈕觸發回傳，並稍後將用來觸發頁面上的驗證控制項。 此外，`ScriptManager`控制項，才能初始化 ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

現在加入`RequiredFieldValidator`會檢查是否有文字欄位中送出表單時的控制項。 `InitialValue`驗證程式的屬性必須設定為相同的值，用於`TextBoxWatermarkExtender`控制項： 送出表單時，不變的文字方塊的值是在其中的水位線值：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

不過還有使用這種方法的一個問題： 如果用戶端停用 JavaScript，在文字欄位已不預先填入浮水印文字，因此`RequiredFieldValidator`不會觸發錯誤訊息。 因此，第二個`RequiredFieldValidator`控制，則它會檢查是否有空白的文字方塊 (省略`InitialValue`屬性)。

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

由於這兩種驗證採用`Display` = `"Dynamic"`，終端使用者無法區別這兩個驗證程式引發的視覺外觀; 相反地，它看起來像是時發生只有其中之一。

最後，新增一些的伺服器端程式碼，以輸出欄位中的文字，如果未驗證發出一則錯誤訊息：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![驗證程式產生負面反應，在欄位中沒有任何文字](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

驗證程式產生負面反應，在欄位中沒有任何文字 ([按一下以檢視完整大小的影像](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-textboxwatermark-in-a-formview-cs.md)
> [下一頁](using-textboxwatermark-in-a-formview-vb.md)
