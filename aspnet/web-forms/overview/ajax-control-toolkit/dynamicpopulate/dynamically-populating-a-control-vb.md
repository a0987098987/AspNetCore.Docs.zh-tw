---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: 以動態方式填入控制項 (VB) |Microsoft Docs
author: wenz
description: DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿至 t 的目標控制項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fd55b59f9375eb320711ffea8d971a8d86179c43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368656"
---
<a name="dynamically-populating-a-control-vb"></a>以動態方式填入控制項 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> DynamicPopulate 控制項在 ASP.NET AJAX Control Toolkit 中呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。


## <a name="overview"></a>總覽

`DynamicPopulate`中 ASP.NET AJAX Control Toolkit 控制項呼叫 web 服務 （或頁面方法），並會產生的值填滿到目標控制項在頁面上，而不需要重新整理頁面。 本教學課程會示範如何設定此項目。

## <a name="steps"></a>步驟

首先，您必須實作呼叫方法的 ASP.NET Web 服務`DynamicPopulate`。 Web 服務類別會要求`ScriptService`內定義的屬性`Microsoft.Web.Script.Services`; 否則為 ASP.NET AJAX 無法建立 web 服務接著所需的用戶端 JavaScript proxy `DynamicPopulate`。

Web 方法必須預期呼叫的型別字串的一個引數`contextKey`，因為`DynamicPopulate`控制項會傳送一段內容資訊與每個 web 服務呼叫。 下列 web 服務會傳回目前的日期格式，由`contextKey`引數：

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Web 服務然後儲存為`DynamicPopulate.vb.asmx`。 或者，您可以實作`getDate()`方法作為頁面方法與實際的 ASP.NET 頁面內`DynamicPopulate`控制項。

在下一個步驟中，建立新的 ASP.NET 檔案。 如往常，第一個步驟是加入`ScriptManager`中目前的頁面，即可載入 ASP.NET AJAX 程式庫，並讓 Control Toolkit:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

然後，新增一個 label 控制項 (例如使用 HTML 控制項的相同的名稱，或&lt; `asp:Label`  / &gt; web 控制項) 這稍後會顯示 web 服務呼叫的結果。

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

HTML 按鈕 （做為 HTML 控制項，因為我們不需要回傳至伺服器） 接著會用來觸發動態的母體擴展：

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

最後，我們需要`DynamicPopulateExtender`要連線的項目控制項。 會設定下列屬性 (明顯的除了`ID`並`runat` = `"server"`):

- `TargetControlID` 要從 web 服務呼叫放置結果
- `ServicePath` web 服務的路徑 （如果您想要使用頁面方法略過）
- `ServiceMethod` web 方法或頁面方法的名稱
- `ContextKey` 若要傳送至 web 服務的內容資訊
- `PopulateTriggerControlID` 這會觸發 web 服務呼叫的項目
- `ClearContentsDuringUpdate` 是否要在 web 服務呼叫期間清空目標項目

如您所見，控制項需要一些資訊，但將所有項目放入位置是相當簡單。 以下是標記`DynamicPopulateExtender`控制項中目前的案例：

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

瀏覽器中執行 ASP.NET 網頁，然後按一下按鈕，您會收到目前的日期，格式為月-日-年。


[![按一下按鈕從伺服器擷取的日期](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

按一下按鈕從伺服器擷取的日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [下一頁](dynamically-populating-a-control-using-javascript-code-vb.md)
