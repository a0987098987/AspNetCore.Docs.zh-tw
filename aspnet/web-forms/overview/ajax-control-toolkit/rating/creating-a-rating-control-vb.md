---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: "建立評等控制項 (VB) |Microsoft 文件"
author: wenz
description: "許多網站中，從社群網站、 電子商務提供其使用者速率文件或項目。 這通常需要一些程式碼撰寫的投入時間，但我們沒有..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ff3394865e084c5a24e7e79469a4a7d26aabb552
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-vb"></a>建立評等控制項 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> 許多網站中，從社群網站、 電子商務提供其使用者速率文件或項目。 這通常需要一些程式碼撰寫的投入時間，但我們沒有 Control Toolkit 我們處置。


## <a name="overview"></a>概觀

許多網站中，從社群網站、 電子商務提供其使用者速率文件或項目。 這通常需要一些程式碼撰寫的投入時間，但我們沒有 Control Toolkit 我們處置。

## <a name="steps"></a>步驟

首先，您必須 （至少） 兩種類型的映像： 一個用於填滿出評等項目，而另一個用於空的評等項目。 星形或笑臉，通常是評等項目。 此案例中，您發現三個檔案、 smiley.png 和 empty.png 以及笑臉 done.png 之來源的程式碼下載此教學課程的一部分。

然後，建立新的 ASP.NET 檔案並開始新增`ScriptManager`給它的控制項：

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

接著，新增`Rating`從 ASP.NET AJAX Control Toolkit 的控制項。 需要此範例中設定下列屬性：

- `CurrentRating`用於初始的評等
- `MaxRating`最高分級
- `EmptyStarCssClass`若要使用空白的評等項目 （星號） 時之 CSS 類別
- `FilledStarCssClass`若要使用的評等項目 （星號） 填寫時之 CSS 類別
- `StarCssClass`若要使用的可見狀態之 CSS 類別
- `WaitingStarCssClass`使用星級評等會傳送至伺服器時的 CSS 類別

以下是會建立具有五個評等控制項的標記項目 (smileys) 的無填滿出一開始：

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

三個參考的 CSS 類別現在需要顯示適當的映像的檔案，這是容易使用 CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

請確定您提供的三個影像的高度與寬度，否則顯示看起來可能有點 messed 組成。

最後，評等的結果應該會顯示給使用者 （或至少儲存在資料庫中）。 因此，加入索引標籤的文字訊息和送出按鈕回分級表單張貼至伺服器的輸出：

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

在伺服器端程式碼中，存取等級控制透過其`ID`，然後存取其`CurrentRating`屬性，這是我們的範例中的值介於 0 到 5 中選取評等項目數目。

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

儲存頁面，並將其載入至您的瀏覽器。 當您將滑鼠停留在 （一開始是空的） 的評等項目時，就會發生 JavaScript 效果： 分級變更。 當您按一下組星號時，會保留目前的評等。 最後，當您提交表單時，伺服器端程式碼輸出選取的分級。


[![使用最少的程式碼建立分級系統](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

使用最少的程式碼建立分級系統 ([按一下以檢視完整大小的影像](creating-a-rating-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一步](creating-a-rating-control-cs.md)
