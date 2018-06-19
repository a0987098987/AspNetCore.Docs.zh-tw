---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: 對抗 Bot (C#) |Microsoft 文件
author: wenz
description: 自動的 bot plaster 網誌及其他網站與垃圾郵件，而不需要任何使用者互動的註解表單送出。 在 ASP.NET AJAX Con NoBot 控制...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ea3aaa5461c2f58a927ae975568f18a34a4729b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872009"
---
<a name="fighting-bots-c"></a>對抗 Bot (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> 自動的 bot plaster 網誌及其他網站與垃圾郵件，而不需要任何使用者互動的註解表單送出。 在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。


## <a name="overview"></a>總覽

自動的 bot plaster 網誌及其他網站與垃圾郵件，而不需要任何使用者互動的註解表單送出。 在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。

## <a name="steps"></a>步驟

擊敗 bot 的一個常見方式是使用 CAPTCHAs 完全自動化公用 Turing 測試，告訴電腦，然後讓人了距離。 Turing 測試原本測試其他人需要決定通訊夥伴是一般人或電腦的位置。 在 web、 CAPTCHA 通常組成上某些扭曲字母的映像。 這個概念是人類看得可以讀取映像上的字母而 OCR 演算法將會失敗。

有數個優點和缺點，這種方法，但對此的討論超出本教學課程的範圍。 不過這會產生 ASP.NET AJAX Control Toolkit 會提供類似的方法中的控制項： `NoBot`。 它比較容易克服比 CAPTCHA，但非常容易使用及 fares 非常好上類似，它會被視為成功如果大部分垃圾郵件嘗試的部落格網站會被破壞，其中`NoBot`控制可以執行。

`NoBot` 如果符合下列情況之一時，會攔截目前的 ASP.NET web form 回傳：

- 不能解決 JavaScript 拼圖的瀏覽器 （例如當 JavaScript 停用時）
- 使用者提交表單，以快速
- 用戶端 IP 位址太常在一段時間中提交表單。

若要檢查這些條件，`NoBot`控制項需要這些屬性 （全部選用）：

- `ResponseMinimumDelaySeconds` 最小數量回傳之間的秒數
- `CutoffWindowSeconds` 在其中一個 IP 從回傳是量值的時間間隔的長度
- `CutoffMaximumInstances` 每個時間間隔的秒數的最大數量

下列標記要求該至少兩秒經過回傳之間並沒有只有五個回傳或小於 30 秒的時間間隔內：

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

然後如往常般請務必包含`ScriptManager`頁面中，而 ASP.NET AJAX library 載入 Control Toolkit 可用：

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

因為大部分的檢查`NoBot`執行的動作就會發生在伺服器端，您需要檢查這些驗證的結果。 這可藉由呼叫`NoBot`的`IsValid()`方法。 它有一個引數 (為`out`參數 /`ByRef`參數) 類型`NoBotState`。 檢查失敗時，其字串表示法包含原因和`Valid`否則。 下列程式碼將輸出訊息，以根據`NoBot`的結果：

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

最後，您需要提交表單和輸出訊息，label 元素，並完成 ！

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

當您執行這個指令碼和停用 JavaScript 或第 2 秒鐘內送出表單或七次 30 秒內送出表單時，會出現錯誤訊息。 不過請明智地使用這個控制項，因為只有大約 90 95%的使用者必須啟用 JavaScript，因此 5-10%的使用者將會失敗`NoBot`的測試。


[![這則錯誤訊息可能被因為由 bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

這則錯誤訊息可能因為由 bot ([按一下以檢視完整大小的影像](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](fighting-bots-vb.md)
