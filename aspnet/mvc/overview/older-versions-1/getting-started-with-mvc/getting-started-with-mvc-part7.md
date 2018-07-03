---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: 將驗證加入至模型 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 1a8c186d5a6b00aaf1061bb4025f4f062203a7df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402976"
---
<a name="adding-validation-to-the-model"></a>將驗證加入至模型
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


在這一節我們即將實作啟用我們的應用程式內的輸入的驗證所需的支援。 我們會確保我們的資料庫內容永遠正確，且很有幫助的錯誤訊息提供給使用者，當他們嘗試，並輸入電影資料無效。 我們開始要先將一些驗證邏輯新增至電影類別。

以滑鼠右鍵按一下 模型 資料夾，然後選取 加入類別。 影片將類別的名稱。

當我們稍早建立的電影實體模型時，IDE 就會建立 Movie 類別。 事實上，電影類別的部分可以是一個檔案與另一個部分。 這就叫做部分類別。 我們要將電影類別延伸從另一個檔案。

我們將建立部分影片類別所指向的 「 朋友類別 」，與一些屬性，可讓系統驗證提示。 我們將會標示的標題和價格為必要項目，並也堅持的價格會在特定範圍內。 以滑鼠右鍵按一下 模型 資料夾，然後選取 加入類別。 影片將類別的名稱，然後按一下 [確定] 按鈕。 以下是我們部分的電影類別看起來像什麼。

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

重新執行您的應用程式，然後再次嘗試輸入影片價格超過 100。 在提交表單之後，您會收到錯誤。 錯誤會攔截在伺服器端，並在張貼表單之後，就會發生。 請注意如何 ASP.NET MVC 的內建 HTML 協助程式是聰明，足以顯示錯誤訊息，並為我們保留值，在文字方塊中的項目內：

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

這非常適合，但如果我們還可以告訴使用者用戶端上立即執行，才能取得有關伺服器應該會不錯。

我們就讓有些使用 JavaScript 的用戶端驗證。

## <a name="adding-client-side-validation"></a>新增用戶端驗證

因為我們的影片類別已經有一些驗證屬性，我們將只需要 Create.aspx 檢視範本中加入幾個 JavaScript 檔案，並新增一行程式碼，以便進行用戶端驗證。

從內 VWD 移的電影檢視/資料夾，然後開啟 Create.aspx。

指令碼 資料夾，在 方案總管 中開啟，並將下列三個指令碼內&lt;head&gt;標記。

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

您想要以此順序顯示這些指令碼檔案。

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

此外，新增上述 Html.BeginForm 這一行：

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

以下是顯示在 IDE 中的程式碼。

[![影片-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

執行您的應用程式和瀏覽 /Movies/Create 同樣地，然後按一下建立不需要輸入任何資料。 錯誤訊息會立即顯示沒有 [flash 我們相關聯傳送資料] 頁面上，一路回溯到伺服器。 這是因為 ASP.NET MVC 現在驗證的輸入上同時 （使用 JavaScript） 的用戶端和伺服器上。

[![建立-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

這狀況良好 ！ 讓我們現在新增一個額外的資料行的資料庫。

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part6.md)
> [下一頁](getting-started-with-mvc-part8.md)
