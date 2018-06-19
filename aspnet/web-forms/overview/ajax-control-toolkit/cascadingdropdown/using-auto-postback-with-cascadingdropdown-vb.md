---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: 使用自動回傳 CascadingDropDown (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871372"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>使用自動回傳 CascadingDropDown (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 不過使用 ASP CascadingDropDown 控制項時。網路的 DropDownList 控制項的 AutoPostBack 功能無法運作，因為會以非同步方式將資料載入清單產生的 （非必要） 的回傳本身。 一些 JavaScript 程式碼中，您可以避免這種效果。


## <a name="overview"></a>總覽

AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。不過使用 ASP CascadingDropDown 控制項時。網路的 DropDownList 控制項的 AutoPostBack 功能無法運作，因為會以非同步方式將資料載入清單產生的 （非必要） 的回傳本身。 一些 JavaScript 程式碼中，您可以避免這種效果。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部&lt; `form` &gt;項目):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

DropDownList 控制項則需要：

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

對於此清單中，就會加入 CascadingDropDown extender，提供 web 服務 URL 和方法資訊：

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

然後 CascadingDropDown extender 以非同步方式呼叫下列方法簽章的 web 服務：

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

方法會傳回類型 CascadingDropDown 值的陣列。 清單項目的標題，然後此值的型別建構函式需要先 (HTML`value`屬性)。

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

載入網頁瀏覽器中的，將會填滿下拉式清單具有三位廠商，第二個正在預先選取。 此外，ASP.NET 定義`__doPostBack()`JavaScript 方法。 一經頁面已載入，下拉式清單中，但只有中是否有項目就會加入這個 JavaScript 呼叫。 如果在清單中沒有任何項目，控制工具組目前正在載入它們，讓 JavaScript 程式碼會使用逾時和半秒一次嘗試。

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

如此一來，回傳才會執行實際的項目在清單中，使用者選取項目。


[![選取的清單項目會回傳](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

選取的清單項目會回傳 ([按一下以檢視完整大小的影像](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](presetting-list-entries-with-cascadingdropdown-vb.md)
