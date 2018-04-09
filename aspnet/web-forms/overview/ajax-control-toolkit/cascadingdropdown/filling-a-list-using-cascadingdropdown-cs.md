---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: 填滿使用 CascadingDropDown (C#) 的清單 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>填入清單，使用 CascadingDropDown (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。若要實際填入下拉式清單中使用此控制項是第一項挑戰，若要解決。


## <a name="overview"></a>總覽

AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。 （比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。若要實際填入下拉式清單中使用此控制項是第一項挑戰，若要解決。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

DropDownList 控制項則需要：

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

對於此清單中，就會加入 CascadingDropDown 擴充項。 非同步要求會傳送到 web 服務接著會傳回一份清單中顯示的項目。 針對此目的，需要設定下列 CascadingDropDown 屬性：

- `ServicePath`： 提供的清單項目 web 服務 URL
- `ServiceMethod`: Web 方法傳遞之清單項目
- `TargetControlID`: ID 的下拉式清單
- `Category`： 提交給 web 方法的呼叫時類別目錄資訊
- `PromptText`： 當以非同步方式從伺服器載入清單資料時顯示的文字

以下是標記`CascadingDropDown`項目。 C# 和 VB 的唯一差異是相關聯的 web 服務的名稱：

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

JavaScript 程式碼來自`CascadingDropDown`extender 呼叫 web 服務方法具有下列簽章：

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

因此重要層面是方法必須傳回型別的陣列`CascadingDropDownNameValue`（由 ASP.NET AJAX Control Toolkit 定義）。 中`CascadingDropDownNameValue`建構函式，第一個清單項目的文字，然後其值必須提供，就如同`<option value="VALUE">NAME</option>`以 HTML 做的一樣。 以下是一些範例資料：

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

載入網頁瀏覽器中的，將會觸發三位廠商填入清單。


[![清單會自動填入](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

清單會自動填入 ([按一下以檢視完整大小的影像](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](using-cascadingdropdown-with-a-database-cs.md)
