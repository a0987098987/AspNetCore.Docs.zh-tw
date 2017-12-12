---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "允許文字方塊 (VB) 中的某些字元 |Microsoft 文件"
author: wenz
description: "ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。 不過這仍無法防止使用者輸入不正確..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>允許文字方塊 (VB) 中的某些字元
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。 不過這仍不會無法防止使用者輸入無效的字元，而且嘗試送出表單。


## <a name="overview"></a>概觀

ASP.NET 驗證控制項可以確保特定的字元，允許在使用者輸入。 不過這仍不會無法防止使用者輸入無效的字元，而且嘗試送出表單。

## <a name="steps"></a>步驟

ASP.NET AJAX Control Toolkit 包含`FilteredTextBox`擴充文字方塊控制項。 一旦啟動後，只有特定的一組字元可能會輸入到欄位。

針對此目的，我們必須先如往常般 ASP.NET AJAX`ScriptManager`載入也會使用 ASP.NET AJAX Control Toolkit 的 JavaScript 程式庫：

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

然後，我們需要的文字方塊：

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

最後，`FilteredTextBoxExtender`控制項負責限制允許使用者輸入的字元。 首先，設定`TargetControlID`屬性`ID`的`TextBox`控制項。 然後，選擇其中一個可用`FilterType`值：

- `Custom`預設值;您必須提供有效的字元的清單
- `LowercaseLetters`只使用小寫字母
- `Numbers`只有數字
- `UppercaseLetters`只有大寫的字母

如果`Custom FilterType`使用時，`ValidChars`屬性必須設定，並提供一份可能輸入的字元。 方式： 如果您嘗試將文字貼到文字方塊中，會移除所有無效的字元。

以下是標記`FilteredTextBoxExtender`控制項，僅允許 數字 (已經使用的項目`FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

執行頁面並再試一次輸入字母，如果已啟用 JavaScript，它將無法運作。數字，但會出現在頁面上。 不過請注意，保護`FilteredTextBox`提供不是項目符號證明： 如果 JavaScript 已啟用，因此您需要使用額外的驗證方法，也就是 ASP，可能會在文字方塊中，輸入任何資料。網路的驗證控制項。


[![可能會輸入只有數字](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

可能會輸入只有數字 ([按一下以檢視完整大小的影像](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一步](allowing-only-certain-characters-in-a-text-box-cs.md)
