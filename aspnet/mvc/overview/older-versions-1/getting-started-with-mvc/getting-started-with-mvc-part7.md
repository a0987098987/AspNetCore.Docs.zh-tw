---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: 將驗證加入至模型 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871944"
---
<a name="adding-validation-to-the-model"></a>將驗證加入至模型
====================
由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


本節中，我們即將實作啟用應用程式中的輸入的驗證所需的支援。 我們會確保我們的資料庫內容永遠正確，且再試一次，且輸入電影資料無效時，提供有用的錯誤訊息給使用者。 我們一開始將極小的驗證邏輯加入至電影類別。

模型 資料夾上按一下滑鼠右鍵並選取 加入類別。 命名您電影的類別。

當我們稍早建立的電影實體模型時，IDE 就會建立影片類別。 事實上，影片類別部分可以是一個檔案與另一個部分。 這稱為部分類別。 我們將電影類別延伸從另一個檔案。

我們將建立部分影片類別指向 < 協同類別 >，以某些屬性，系統將讓驗證提示。 我們會標記為必要項、 價格和標題和也堅持這個價格是特定範圍內。 以滑鼠右鍵按一下 模型 資料夾，然後選取 加入類別。 命名您電影的類別，然後按一下 [確定] 按鈕。 以下是什麼我們部分影片類別類似。

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

重新執行您的應用程式，並嘗試輸入電影的價格超過 100。 您已送出表單之後，您會收到錯誤。 錯誤在伺服器端會遭到攔截，而表單張貼之後，就會發生。 請注意如何 ASP.NET MVC 的內建的 HTML helper 已聰明，可以顯示錯誤訊息，並為我們保留值，在文字方塊中的項目內：

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

這非常適合，但會是很好如果我們無法告訴使用者在用戶端，立即取得涉及伺服器之前。

讓我們啟用 JavaScript 的某些用戶端驗證。

## <a name="adding-client-side-validation"></a>加入用戶端驗證

我們的影片類別已經有一些驗證屬性，因為我們只需要將一些 JavaScript 檔案加入至我們 Create.aspx 檢視的範本，並加入一行程式碼，以啟用用戶端驗證，才能進行。

從 VWD 移我們影片檢視/資料夾，並開啟 Create.aspx。

開啟 方案總管 中的指令碼 資料夾，並拖曳的下列三個指令碼內&lt;head&gt;標記。

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

您想要依此順序顯示這些指令碼檔案。

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

此外，請加入 Html.BeginForm 上述這一行：

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

以下是顯示在 IDE 中的程式碼。

[![影片-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

執行您的應用程式和造訪 /Movies/Create 一次，並按一下 建立不需要輸入任何資料。 錯誤訊息會立即出現快閃，我們將關聯傳送資料的頁面不回溯到伺服器。 這是因為 ASP.NET MVC 現在驗證的輸入上兩個 （使用 JavaScript） 用戶端和伺服器上。

[![建立-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

正在尋找良好 ！ 我們現在將一個額外的資料行加入資料庫。

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part6.md)
> [下一頁](getting-started-with-mvc-part8.md)
