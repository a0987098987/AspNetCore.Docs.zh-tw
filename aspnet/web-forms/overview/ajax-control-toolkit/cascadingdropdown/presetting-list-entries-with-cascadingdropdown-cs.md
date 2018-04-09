---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Presetting CascadingDropDown (C#) 的清單項目 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d87da6c19f6dbdff70eff410ba7573c3e26884fb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Presetting CascadingDropDown (C#) 的清單項目
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 利用最少的程式碼可能會以動態方式載入資料之後的清單項目已預先選取。


## <a name="overview"></a>總覽

AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。利用最少的程式碼可能會以動態方式載入資料之後的清單項目已預先選取。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

DropDownList 控制項則需要：

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

對於此清單中，就會加入 CascadingDropDown extender，提供 web 服務 URL 和方法資訊：

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

然後 CascadingDropDown extender 以非同步方式呼叫下列方法簽章的 web 服務：

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

方法會傳回類型 CascadingDropDown 值的陣列。 清單項目的標題，然後此值的型別建構函式需要先 (HTML`value`屬性)。 如果第三個引數設定為 true，清單項目會自動選取瀏覽器中。

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

載入網頁瀏覽器中的，將會填滿下拉式清單具有三位廠商，第二個正在預先選取。


[![清單會填入，且自動預先選取](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

填入清單並預先選取自動 ([按一下以檢視完整大小的影像](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-cascadingdropdown-with-a-database-cs.md)
> [下一頁](using-auto-postback-with-cascadingdropdown-cs.md)
