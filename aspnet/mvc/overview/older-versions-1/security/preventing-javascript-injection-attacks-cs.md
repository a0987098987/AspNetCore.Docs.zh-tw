---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: 防止 JavaScript 插入式攻擊 (C#) |Microsoft 文件
author: StephenWalther
description: 防止 JavaScript 插入式攻擊和跨網站指令碼處理攻擊發生給您。 在此教學課程中，作者： Stephen Walther 會說明如何輕鬆地 de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: fbec58c009640164d908db5a45557c9e50041173
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-c"></a>防止 JavaScript 插入式攻擊 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

[下載 PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> 防止 JavaScript 插入式攻擊和跨網站指令碼處理攻擊發生給您。 在本教學課程中，作者： Stephen Walther 會說明如何輕鬆地擊敗這類攻擊的 HTML 編碼您的內容。


本教學課程的目標是以說明您可以在如何在 ASP.NET MVC 應用程式中防止 JavaScript 插入式攻擊。 本教學課程將討論兩種方法來保護您的網站 JavaScript 資料隱碼攻擊。 您了解如何防止 JavaScript 插入式攻擊編碼您顯示的資料。 您也了解如何編碼的資料，您接受防止 JavaScript 插入式攻擊。

## <a name="what-is-a-javascript-injection-attack"></a>什麼是 JavaScript 資料隱碼攻擊？

每當您接受使用者輸入，並重新顯示使用者輸入，您會開啟您的網站以 JavaScript 資料隱碼攻擊。 讓我們來檢查具象的應用程式開啟的 JavaScript 資料隱碼攻擊。

假設您已建立客戶的意見反應網站 （請參閱圖 1）。 客戶可以瀏覽的網站，並使用您的產品他們經驗上輸入意見反應。 當客戶提交意見反應時，意見反應會重新顯示在意見反應 頁面上。


[![客戶意見反應網站](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**圖 01**： 客戶的意見反應網站 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image3.png))


客戶意見反應網站會使用`controller`列表 1 中。 這`controller`包含名為兩個動作`Index()`和`Create()`。

**列出 1 – `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

`Index()`方法顯示`Index`檢視。 這個方法會傳遞所有先前的客戶意見反應給`Index`檢視擷取從資料庫 （使用 LINQ to SQL 查詢） 的意見反應。

`Create()`方法會建立新的意見反應項目，並將它加入至資料庫。 在表單中輸入客戶的訊息傳遞至`Create()`message 參數中的方法。 建立意見反應項目，且訊息已指派給意見項目的`Message`屬性。 意見反應項目提交至資料庫`DataContext.SubmitChanges()`方法呼叫。 最後，造訪者重新導向回到`Index`檢視顯示所有的意見反應。

`Index`檢視包含在清單 2。

**列出 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

`Index`檢視有兩個區段。 上方區段包含實際的客戶回函表單。 下面區段包含 For...每個迴圈，迴圈的所有先前的客戶意見反應項目，並顯示每個意見項目的 EntryDate 和訊息屬性。

客戶意見反應網站是一個簡單的網站。 不幸的是，網站會開啟 JavaScript 資料隱碼攻擊。

假設有下列的文字輸入客戶回函表單：

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

這段文字表示的 JavaScript 指令碼，會顯示警示訊息方塊。 有人將此指令碼提交至意見反應之後形成，訊息<em>Boo ！</em>任何人拜訪客戶意見反應網站將來 （請參閱圖 2） 時，會出現。


[![JavaScript 資料隱碼](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**圖 02**: JavaScript 插入 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image6.png))


現在，您的初始回應 JavaScript 資料隱碼攻擊可能種冷漠態度。 您可以將 JavaScript 資料隱碼攻擊是只是一種*損毀*攻擊。 您可能認為，沒有人可以任何動作真正惡意所認可的 JavaScript 插入式攻擊。

不幸的是，駭客就可以進行一些 really 真的惡意的項目插入至網站的 JavaScript。 您可以使用 JavaScript 插入式攻擊，以執行跨網站指令碼 (XSS) 攻擊。 在跨網站指令碼處理攻擊中，您會竊取機密的使用者資訊，並將資訊傳送給另一個網站。

比方說，駭客就可以使用 JavaScript 資料隱碼攻擊竊取其他使用者的瀏覽器 cookie 的值。 如果瀏覽器 cookie 中儲存機密資訊-例如密碼、 信用卡號碼或身分證號碼 –，駭客可以竊取這項資訊使用 JavaScript 插入式攻擊。 或者，如果使用者在駭客就可以使用插入的 JavaScript 來抓取表單資料，並將它傳送給另一個網站已遭洩漏 JavaScript 攻擊中，在頁面所包含的表單欄位中輸入機密資訊。

*請害怕*。 重視 JavaScript 插入式攻擊，並保護您的使用者機密資訊。 在下面兩個區段中，我們將討論兩項技術可讓您保護您的 ASP.NET MVC 應用程式，從 JavaScript 資料隱碼攻擊。

## <a name="approach-1-html-encode-in-the-view"></a>方法 1： 在檢視中的 HTML 編碼

一個簡單的方法，防止 JavaScript 插入式攻擊是 HTML 編碼重新顯示在檢視中的資料時，網站使用者所輸入的任何資料。 已更新`Index` 檢視中列出的 3 遵循此方法。

**列出 3 – `Index.aspx` (HTML 編碼)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

請注意，值`feedback.Message`是 HTML 編碼的值顯示為下列程式碼之前：

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

什麼平均值為 HTML 編碼字串？ 當您以 HTML 編碼字串時，請危險字元，例如`<`和`>`HTML 實體參考，例如以取代`&lt;`和`&gt;`。 因此字串`<script>alert("Boo!")</script>`是 HTML 編碼，則取得轉換成`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`。 會由瀏覽器的解譯時，JavaScript 指令碼不會再執行編碼的字串。 相反地，您會收到無害頁面圖 3 中。


[![讓的 JavaScript 攻擊](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**圖 03**： 擊敗 JavaScript 攻擊 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image9.png))


請注意，在`Index`檢視中列出的 3 的值`feedback.Message`編碼。 值`feedback.EntryDate`就未編碼。 您只需要使用者輸入的資料進行編碼。 由於控制器已產生 EntryDate 的值，因此您不需要 html 編碼這個值。

## <a name="approach-2-html-encode-in-the-controller"></a>方法 2： 在控制器中的 HTML 編碼

而不是 HTML 編碼的資料，當您在檢視中顯示資料時，您可以 HTML 編碼的資料之前您送出至資料庫資料。 會從這個第二種方法是`controller`中列出的 4。

**列出 4 – `HomeController.cs` (HTML 編碼)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

請注意，訊息的值是 HTML 編碼的值會送出到資料庫中之前`Create()`動作。 當訊息會重新顯示在檢視中時，訊息是 HTML 編碼，而且不會執行任何插入訊息中的 JavaScript。

一般而言，您可能偏好在本教學課程所討論，透過這個第二種方法的第一個方法。 此第二個方法的問題是，您以結束 HTML 編碼的資料在資料庫中。 換句話說，不論您資料庫的資料是與有趣的字元。

為什麼這是不正確？ 如果您需要在網頁以外的項目中顯示的資料庫資料，您就會發生問題。 比方說，您可以在 Windows Form 應用程式不再輕鬆地顯示資料。

## <a name="summary"></a>總結

本教學課程的用途是為了嚇到您的 JavaScript 插入式攻擊的潛在相關。 本教學課程所討論的防禦 JavaScript 資料隱碼攻擊您的 ASP.NET MVC 應用程式的兩種方法： 可以是 HTML 編碼提交的使用者資料的檢視，或者您可以 HTML 編碼使用者提交控制器中的資料。

> [!div class="step-by-step"]
> [上一頁](authenticating-users-with-windows-authentication-cs.md)
> [下一頁](authenticating-users-with-forms-authentication-vb.md)
