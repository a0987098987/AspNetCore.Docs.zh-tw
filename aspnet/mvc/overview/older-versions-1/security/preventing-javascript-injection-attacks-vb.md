---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: 防止 JavaScript 插入式攻擊 (VB) |Microsoft Docs
author: StephenWalther
description: 防止 JavaScript 插入式攻擊和跨網站指令攻擊發生給您。 在本教學課程中，Stephen Walther 會說明如何輕鬆地 de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c022847462239624d5b84816999918ec77f8337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402337"
---
<a name="preventing-javascript-injection-attacks-vb"></a>防止 JavaScript 插入式攻擊 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

[下載 PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> 防止 JavaScript 插入式攻擊和跨網站指令攻擊發生給您。 在本教學課程中，Stephen Walther 會說明如何輕鬆地擊敗這些類型的 HTML 編碼內容的攻擊。


本教學課程的目標是要說明如何防止 JavaScript 插入式攻擊，以及在您的 ASP.NET MVC 應用程式中。 本教學課程會討論兩種方法來保護您的網站，JavaScript 插入式攻擊。 您了解如何為您顯示的資料進行編碼，以防止 JavaScript 插入式攻擊。 您也了解如何防止 JavaScript 插入式攻擊的編碼方式表示您接受的資料。

## <a name="what-is-a-javascript-injection-attack"></a>什麼是 JavaScript 插入式攻擊？

當您接受使用者輸入，並重新顯示使用者輸入時，您就會開啟您的網站以 JavaScript 插入式攻擊。 讓我們來檢查具象的應用程式開啟 JavaScript 插入式攻擊。

假設您已建立客戶的意見反應網站 （請參閱 圖 1）。 客戶可以瀏覽網站，並使用您的產品使用者體驗上輸入意見反應。 當客戶提交他們的意見反應時，意見反應會重新顯示在意見反應 頁面上。


[![客戶意見反應網站](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**圖 01**： 客戶意見反應網站 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-vb/_static/image3.png))


客戶意見反應網站使用`controller`列表 1 中。 這`controller`包含名為兩個動作`Index()`和`Create()`。

**列表 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

`Index()`方法顯示`Index`檢視。 此方法會傳遞所有過去的客戶意見反應，以`Index`（使用 LINQ to SQL 查詢），從資料庫擷取意見反應的檢視。

`Create()`方法會建立新的意見項目，並將它加入至資料庫。 客戶輸入表單中的訊息會傳遞至`Create()`訊息參數中的方法。 建立意見項目並將訊息指派給意見項目`Message`屬性。 意見項目提交至資料庫`DataContext.SubmitChanges()`方法呼叫。 最後，訪客會被重新導向回到`Index`檢視顯示所有意見反應。

`Index`檢視包含在 列表 2。

**列表 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

`Index`檢視有兩個區段。 上方區段會包含實際的客戶意見反應表單。 底部區段包含一個 For...每個迴圈，迴圈的所有先前的客戶意見反應項目，並顯示每個意見項目 EntryDate 和訊息屬性。

客戶意見反應網站是一個簡單的網站。 不幸的是，網站會開啟 JavaScript 插入式攻擊。

假設在客戶的意見反應表單中輸入下列文字：

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

這段文字表示的 JavaScript 指令碼，會顯示警示訊息方塊。 有人將此指令碼提交至意見反應之後形成，訊息<em>Boo ！</em>會出現時的任何人造訪客戶意見反應網站未來 （請參閱 圖 2）。


[![JavaScript 插入式攻擊](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**圖 02**: JavaScript 插入式攻擊 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-vb/_static/image6.png))


現在，您 JavaScript 插入式攻擊的初始回應可能會讓您失去動力。 您可能會認為，JavaScript 插入式攻擊是只是一種*竄改*攻擊。 您可能會認為，沒有人可以任何動作真正邪惡認可 JavaScript 插入式攻擊。

不幸的是，駭客就可以進行一些真的很邪惡的項目，藉由插入網站中的 JavaScript。 若要執行跨網站指令碼 (XSS) 攻擊，您可以使用 JavaScript 插入式攻擊。 在跨網站指令攻擊中，您可以竊取機密的使用者資訊，並將資訊傳送給另一個網站。

比方說，駭客就可以使用 JavaScript 插入式攻擊竊取其他使用者的瀏覽器 cookie 的值。 機密資訊，例如密碼、 信用卡號碼或身分證號碼 – 儲存在瀏覽器 cookie，如果駭客可以竊取這項資訊使用 JavaScript 插入式攻擊。 或者，如果使用者輸入具有以 JavaScript 攻擊遭入侵的頁面所包含的表單欄位中的機密資訊，然後駭客可以使用插入的 JavaScript 來抓取表單資料，並將它傳送給另一個網站。

*請害怕*。 重視 JavaScript 插入式攻擊，並保護您的使用者機密資訊。 在接下來兩節中，我們會討論兩種技術可供您保護您的 ASP.NET MVC 應用程式，從 JavaScript 插入式攻擊。

## <a name="approach-1-html-encode-in-the-view"></a>方法 #1： 在檢視中的 HTML 編碼

其中一個簡單的方法，防止 JavaScript 插入式攻擊是以 HTML 編碼重新顯示在檢視中的資料時，網站使用者所輸入的任何資料。 已更新`Index`列表 3 中的檢視會遵循這種方法。

**列表 3 – `Index.aspx` (編碼的 HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

請注意，值`feedback.Message`是 HTML 編碼之前的值會顯示為下列程式碼：

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

什麼平均值為 HTML 編碼字串？ 當您以 HTML 編碼字串時，危險字元，例如`<`並`>`這類的 HTML 實體參考會取代`&lt;`和`&gt;`。 因此當字串`<script>alert("Boo!")</script>`是 HTML 編碼，將它轉換成`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`。 做為解譯的瀏覽器時，JavaScript 指令碼不會再執行編碼的字串。 相反地，您可以取得無害的頁面 [圖 3] 中。


[![失效的 JavaScript 攻擊](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**圖 03**： 讓 JavaScript 攻擊 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-vb/_static/image9.png))


請注意，在`Index`檢視中 列表 3 的值`feedback.Message`編碼。 值`feedback.EntryDate`就未編碼。 您只需要將使用者輸入的資料進行編碼。 在控制器中產生 EntryDate 的值，因為您不需要為 HTML 編碼此值。

## <a name="approach-2-html-encode-in-the-controller"></a>方法 2： 在控制器中的 HTML 編碼

而不是 HTML 編碼的資料，當您在檢視中顯示資料時，您可以 HTML 編碼之前您送出至資料庫資料的資料。 此第二種方法會是`controller`列表 4 中。

**列表 4 – `HomeController.cs` (編碼的 HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

請注意，訊息的值是 HTML 編碼之前在該值提交至資料庫內`Create()`動作。 當訊息會重新顯示在檢視中時，訊息是 HTML 編碼，而且不會執行任何插入訊息中的 JavaScript。

一般而言，您應該偏好使用透過此第二種方法在本教學課程所討論的第一個方法。 此第二個方法的問題是，您得到 HTML 編碼的資料在資料庫中。 換句話說，您資料庫的資料會遇到一些瘋狂的尋找字元時修改。

為什麼這是不正確？ 如果您需要在 web 網頁以外的項目中顯示的資料庫資料，您會遇到問題。 例如，您可以在 Windows Forms 應用程式中不再那麼容易顯示資料。

## <a name="summary"></a>總結

本教學課程的目的是要把您唬住有關 JavaScript 插入式攻擊的潛在客戶。 本教學課程中討論過防禦您的 ASP.NET MVC 應用程式，防止 JavaScript 插入式攻擊的兩種方法： 可以是 HTML 編碼使用者提交資料的檢視，或者您可以 HTML 編碼使用者提交控制器中的資料。

> [!div class="step-by-step"]
> [上一步](authenticating-users-with-windows-authentication-vb.md)
