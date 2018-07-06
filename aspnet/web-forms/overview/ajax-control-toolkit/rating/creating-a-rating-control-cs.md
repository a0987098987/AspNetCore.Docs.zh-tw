---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: 建立評等控制項 (C#) |Microsoft Docs
author: wenz
description: 許多網站中，從電子商務社群網站，以提供使用者速率文件或項目。 這通常需要一些編碼工作，但我們確實有...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 67c66d2a28a247493a0b1ea67e15935c27af80ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830950"
---
<a name="creating-a-rating-control-c"></a>建立評等控制項 (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> 許多網站中，從電子商務社群網站，以提供使用者速率文件或項目。 這通常需要一些編碼工作，但我們確實有 Control Toolkit，以供我們運用。


## <a name="overview"></a>總覽

許多網站中，從電子商務社群網站，以提供使用者速率文件或項目。 這通常需要一些編碼工作，但我們確實有 Control Toolkit，以供我們運用。

## <a name="steps"></a>步驟

首先，您必須 （至少） 兩種類型的映像： 一個用於區域分布評等項目，和一個空的評等項目。 評等項目通常是以星號或笑臉。 此案例中，您尋找三個檔案、 smiley.png 和 empty.png 和笑臉 done.png 之來源的程式碼下載本教學課程的一部分。

然後，建立新的 ASP.NET 檔案，並開始新增`ScriptManager`控制項：

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

然後，新增`Rating`從 ASP.NET AJAX Control Toolkit 控制項。 設定此範例中，需要下列屬性：

- `CurrentRating` 若要使用初始的評等
- `MaxRating` 最高分級
- `EmptyStarCssClass` 若要使用的評等項目 （star 認證） 時的 CSS 類別是空的
- `FilledStarCssClass` 若要使用的評等項目 （star 認證） 時的 CSS 類別填妥
- `StarCssClass` 要用於顯示狀態的 CSS 類別
- `WaitingStarCssClass` 要使用星級評等會傳送至伺服器時的 CSS 類別

以下是建立具有五個的評等控制項的標記項目 (smileys) 的無填妥一開始：

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

三個參考的 CSS 類別現在需要顯示適當的映像檔案中，這是容易使用 CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

請確定您提供的三個影像的高度與寬度，否則顯示看起來可能有點 messed 註冊。

最後，評等的結果應該會顯示給使用者 （或至少會儲存在資料庫中）。 因此，加入的標籤文字訊息和提交按鈕回傳到伺服器的評等形式的輸出：

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

在伺服器端程式碼中，存取 評等控制項，透過其`ID`，然後存取其`CurrentRating`屬性，也就是所選的評等項目，在本例中的值介於 0 到 5 之間的數字。

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

儲存頁面，並將其載入至您的瀏覽器。 當您暫留 （一開始是空的） 的評等項目時，就會發生 JavaScript 的效果： 評等變更。 當您按一下組星號時，會保留目前的評等。 最後，當您提交表單時，伺服器端程式碼會輸出選取的評等。


[![使用最少的程式碼建立的分級系統](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

使用最少的程式碼建立的分級系統 ([按一下以檢視完整大小的影像](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](creating-a-rating-control-vb.md)
