---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: 驗證控制項 (C#) 搭配使用 TextBoxWatermark |Microsoft 文件
author: wenz
description: AJAX Control Toolkit TextBoxWatermark 控制項擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下 [到] 方塊，它我...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>使用 TextBoxWatermark 與驗證控制項 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> AJAX Control Toolkit TextBoxWatermark 控制項擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下此方塊時，它會清空。 如果使用者離開 [] 方塊中，而不需輸入文字，其中已預先的文字會重新出現。 這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。


## <a name="overview"></a>總覽

`TextBoxWatermark` AJAX Control Toolkit 中的控制會擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下此方塊時，它會清空。 如果使用者離開 [] 方塊中，而不需輸入文字，其中已預先的文字會重新出現。 這可能會與在相同頁面上，ASP.NET 驗證控制項發生衝突，但可能會解決這些問題。

## <a name="steps"></a>步驟

基本設定的範例如下所示：`TextBox`控制項使用的浮水印`TextBoxWatermarkExtender`控制項。 按鈕會觸發回傳，並稍後會用來觸發驗證控制項在頁面上。 此外，`ScriptManager`控制項，才能初始化 ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

現在，加入`RequiredFieldValidator`會檢查是否有文字欄位中時提交此表單的控制項。 `InitialValue`驗證程式的屬性必須設定為相同的值，用於`TextBoxWatermarkExtender`控制項： 提交表單時，不變的文字方塊的值時，浮水印值，其中：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

但是沒有這種方法有一個問題： 如果用戶端停用 JavaScript，在文字欄位不因此使具有浮水印文字`RequiredFieldValidator`不會觸發錯誤訊息。 因此，第二個`RequiredFieldValidator`控制項是必要也會檢查是否有空白的文字方塊中 (省略`InitialValue`屬性)。

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

因為這兩個驗證程式使用`Display` = `"Dynamic"`，終端使用者無法區別這兩個驗證程式所引發的視覺外觀; 相反地，您似乎沒有只有一個。

最後，將某些伺服器端程式碼加入輸出的文字欄位中，如果沒有驗證程式會發出錯誤訊息：

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![驗證程式指出其欄位中沒有任何文字](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

驗證程式指出其欄位中沒有任何文字 ([按一下以檢視完整大小的影像](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-textboxwatermark-in-a-formview-cs.md)
> [下一頁](using-textboxwatermark-in-a-formview-vb.md)
