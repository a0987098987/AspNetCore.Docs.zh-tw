---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: "以動態方式填入控制項 (C#) |Microsoft 文件"
author: wenz
description: "在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標上的控制項 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a>以動態方式填入控制項 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> 在 ASP.NET AJAX Control Toolkit DynamicPopulate 控制項呼叫 web 服務 （或頁面的方法），並產生的值填入目標控制項在頁面上，如果沒有頁面重新整理。


## <a name="overview"></a>概觀

`DynamicPopulate` ASP.NET AJAX Control Toolkit 中的控制呼叫 web 服務 （或頁面的方法），並填入目標控制項在頁面上，如果沒有頁面重新整理所產生的值。 本教學課程會示範如何設定此項目。

## <a name="steps"></a>步驟

首先，您需要 ASP.NET Web 服務會實作由呼叫方法`DynamicPopulate`。 Web 服務類別會要求`ScriptService`屬性內定義`Microsoft.Web.Script.Services`; 否則 ASP.NET AJAX 無法建立依次所需的 web 服務的用戶端 JavaScript proxy `DynamicPopulate`。

Web 方法必須接受一個引數之字串型別，稱為`contextKey`，因為`DynamicPopulate`控制項會傳送一個片段的內容資訊與每個 web 服務呼叫。 下列 web 服務所表示的格式傳回目前日期`contextKey`引數：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Web 服務然後儲存為`DynamicPopulate.cs.asmx`。 或者，您可以實作`getDate()`方法做為頁面方法，與實際的 ASP.NET 頁面內`DynamicPopulate`控制項。

在下一個步驟中，建立新的 ASP.NET 檔案。 因為一律，第一個步驟是要包含`ScriptManager`中目前的頁面載入 ASP.NET AJAX 程式庫，並讓 Control Toolkit 工作：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

然後，將標籤控制項 (例如使用 HTML 控制項的名稱相同，或&lt; `asp:Label`  / &gt; web 控制項) 的稍後將會顯示 web 服務呼叫的結果。

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

（如 HTML 控制項，因為我們不需要向伺服器回傳） HTML 按鈕然後將用來觸發動態母體擴展：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

最後，我們需要`DynamicPopulateExtender`網路進行的控制項。 將設定下列屬性 (除了明顯的`ID`和`runat` = `"server"`):

- `TargetControlID`要放置從 web 服務呼叫的結果
- `ServicePath`web 服務的路徑 （如果您想要使用的頁面方法省略）
- `ServiceMethod`web 方法或頁面的方法名稱
- `ContextKey`內容資訊傳送至 web 服務
- `PopulateTriggerControlID`web 服務呼叫觸發程序項目
- `ClearContentsDuringUpdate`是否要在 web 服務呼叫期間清空目標項目

如您所見，控制項需要一些資訊，但將所有資料放入定位，並相當簡單。 以下是標記`DynamicPopulateExtender`控制項中目前的狀況：

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

在瀏覽器中執行的 ASP.NET 網頁，然後按一下按鈕。您會收到目前的日期，格式為月-日-年。


[![按一下按鈕從伺服器擷取日期](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

按一下按鈕從伺服器擷取日期 ([按一下以檢視完整大小的影像](dynamically-populating-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一步](dynamically-populating-a-control-using-javascript-code-cs.md)
