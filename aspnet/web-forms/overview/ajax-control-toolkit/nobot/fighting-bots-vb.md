---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: 對抗 Bot (VB) |Microsoft Docs
author: wenz
description: 自動化的機器人 plaster 與垃圾郵件，提交而不需要任何使用者互動的註解表單的部落格和其他網站。 在 ASP.NET AJAX Con NoBot 控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f919d130cc97b5f892cc01d58a0bcba790a98e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362514"
---
<a name="fighting-bots-vb"></a>對抗 Bot (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> 自動化的機器人 plaster 與垃圾郵件，提交而不需要任何使用者互動的註解表單的部落格和其他網站。 在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。


## <a name="overview"></a>總覽

自動化的機器人 plaster 與垃圾郵件，提交而不需要任何使用者互動的註解表單的部落格和其他網站。 在 ASP.NET AJAX Control Toolkit NoBot 控制項來協助對抗這些機器人。

## <a name="steps"></a>步驟

一個常見的方法，來擊敗 bot 是使用 CAPTCHAs 完全自動化公用 Turing 測試，告訴電腦，以及讓人判讀分開。 Turing 測試原本是測試有人需要決定通訊的夥伴一般人或電腦的地方。 在 web、 CAPTCHA 通常包含一些扭曲的字母，在其上的映像。 其概念是人類看得可以讀取映像上的字母而 OCR 演算法將會失敗。

有幾個優點和缺點，這種作法，但這些討論超出本教學課程的範圍。 不過沒有 ASP.NET AJAX Control Toolkit 提供類似的方法中的控制項： `NoBot`。 它會比較容易克服比文字驗證，但非常容易使用和時，它會被視為成功如果大多數垃圾郵件嘗試的部落格等的網站上也會被破壞，其中`NoBot`控制可以執行。

`NoBot` 符合至少其中一種情形時，會攔截目前的 ASP.NET web form 回傳：

- 不能解決 JavaScript 謎題的瀏覽器 （例如當 JavaScript 已停用）
- 使用者提交表單，以快速
- 用戶端 IP 位址會提交表單，往往在一段時間。

若要檢查是否有這些條件，`NoBot`控制項需要這些屬性 （全部選用）：

- `ResponseMinimumDelaySeconds` 回傳之間的秒數的最小數量
- `CutoffWindowSeconds` 在其中一個 IP 回傳是量值的時間間隔的長度
- `CutoffMaximumInstances` 每個時間間隔的秒數的最大數量

下列標記要求至少兩秒數經過回傳之間並沒有只有五個回傳或小於 30 秒的時間間隔內：

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

然後如往常般請務必包含`ScriptManager`頁面中，讓 ASP.NET AJAX 程式庫已載入，並控制工具組可以用於：

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

因為大部分的檢查`NoBot`負責在伺服器端上進行您需要檢查這些驗證的結果。 這可以藉由呼叫`NoBot`的`IsValid()`方法。 它有一個引數 (以`out`參數 /`ByRef`參數) 的`NoBotState`。 檢查失敗時，其字串表示會包含原因和`Valid`否則。 下列程式碼會輸出訊息，以根據`NoBot`的結果：

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

最後，您需要提交表單和輸出訊息，label 項目，並完成 ！

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

當您執行此指令碼和停用 JavaScript 或送出表單時前, 兩秒內或 30 秒內七次送出表單時，會出現錯誤訊息。 不過更聰明地使用這個控制項，因為只有約 90-95%的使用者必須啟用 JavaScript，因此 5-10%的使用者將會失敗`NoBot`的測試。


[![此錯誤訊息可能被因為由 bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

此錯誤訊息可能因為由電腦控制機甲 ([按一下以檢視完整大小的影像](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](fighting-bots-cs.md)
