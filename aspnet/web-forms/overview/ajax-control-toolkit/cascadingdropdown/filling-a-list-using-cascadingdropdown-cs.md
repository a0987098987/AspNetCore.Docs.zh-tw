---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: 填滿清單使用 CascadingDropDown (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 795b2a2ee80d3d2bed6f752887d5f9d974151a56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824552"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>填滿清單使用 CascadingDropDown (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> 在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。若要解決的第一個挑戰是實際填入下拉式清單中使用這個控制項。


## <a name="overview"></a>總覽

在 AJAX Control Toolkit CascadingDropDown 控制擴充 DropDownList 控制項以讓一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單則填入該狀態中主要城市）。若要解決的第一個挑戰是實際填入下拉式清單中使用這個控制項。

## <a name="steps"></a>步驟

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

DropDownList 控制項則需要：

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

對於此清單中，就會加入 CascadingDropDown 擴充項。 非同步要求會傳送到一項 web 服務，接著會傳回一份要顯示在清單中的項目。 針對此目的，需要設定下列 CascadingDropDown 屬性：

- `ServicePath`: 提供的清單項目在 web 服務的 URL
- `ServiceMethod`： 提供的清單項目 web 方法
- `TargetControlID`: ID 的下拉式清單
- `Category`： 提交給 web 方法的呼叫時類別目錄資訊
- `PromptText`： 以非同步方式從伺服器載入清單資料時顯示的文字

以下是標記`CascadingDropDown`項目。 C# 和 VB 之間唯一的差別是相關聯的 web 服務的名稱：

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

JavaScript 程式碼來自`CascadingDropDown`擴充項會呼叫 web 服務方法具有下列簽章：

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

因此重要的是，此方法需要傳回類型的陣列`CascadingDropDownNameValue`（由 ASP.NET AJAX Control Toolkit 中定義）。 在 `CascadingDropDownNameValue`建構函式，第一個清單項目的文字，然後其值必須提供，就如同`<option value="VALUE">NAME</option>`在 HTML 中一樣。 以下是一些範例資料：

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

載入瀏覽器頁面，將會觸發要填入三個廠商的清單。


[![清單會自動填入](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

自動填滿清單 ([按一下以檢視完整大小的影像](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](using-cascadingdropdown-with-a-database-cs.md)
