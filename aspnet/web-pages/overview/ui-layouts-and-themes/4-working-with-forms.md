---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: 使用 ASP.NET Web Pages (Razor) 網站中的 HTML 表單 |Microsoft Docs
author: tfitzmac
description: 表單是您在其中放置使用者輸入控制項，例如文字方塊、 核取方塊、 選項按鈕和下拉式清單的 HTML 文件區段。 使用表單北...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8d52cb6406859c77687622b7a101cf67781b863d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380343"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>使用 ASP.NET Web Pages (Razor) 網站中的 HTML 表單
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何處理 HTML 表單 （使用文字方塊和按鈕） 當您使用 ASP.NET Web Pages (Razor) 網站中。
> 
> **您將學到什麼：** 
> 
> - 如何建立一個 HTML 表單。
> - 如何讀取從表單的使用者輸入。
> - 如何驗證使用者輸入。
> - 如何在送出頁面之後，還原表單值。
> 
> 這些是 ASP.NET 程式設計概念，文件中：
> 
> - `Request` 物件。
> - 輸入的驗證。
> - HTML 編碼。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="creating-a-simple-html-form"></a>建立簡單的 HTML 表單

1. 建立新的網站。
2. 在根資料夾中，建立名為網頁*Form.cshtml*並輸入下列標記：

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. 啟動您的瀏覽器頁面。 (在 WebMatrix 中，在**檔案**工作區中，以滑鼠右鍵按一下檔案，然後選取**在瀏覽器中啟動**。)簡單的表單，具有三個輸入欄位，以及**送出**按鈕會顯示。

    ![有三個文字方塊的表單螢幕擷取畫面。](4-working-with-forms/_static/image1.jpg)

    此時，如果您按一下**送出**按鈕時，會發生任何事。 若要讓表單很有用，您必須新增一些將會在伺服器執行的程式碼。

## <a name="reading-user-input-from-the-form"></a>讀取表單中的使用者輸入

若要處理的表單，您加入讀取已送出的欄位值，並執行某個動作，與他們的程式碼。 此程序會示範如何讀取的欄位，並在頁面上顯示使用者輸入。 （在生產應用程式中，您通常執行更有趣的工作，使用者輸入。 您會執行，在使用資料庫的相關文章。）

1. 在頂端*Form.cshtml*檔案中，輸入下列程式碼：

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    當使用者第一次要求頁面時，會顯示空白的表單。 （這會是您） 使用者填寫表單，並按一下**送出**。 這會將提交 （張貼） 要在伺服器的使用者輸入。 根據預設，要求會送到相同的頁面 (亦即*Form.cshtml*)。

    當您提交頁面這次時，您輸入的值會顯示在表單的正上方：

    ![如果螢幕擷取畫面顯示在頁面上顯示您輸入的值。](4-working-with-forms/_static/image2.jpg)

    看看頁面的程式碼。 您先使用`IsPost`方法，以判斷是否要張貼頁面&#8212;也就是是否使用者已按下**提交**按鈕。 如果這是 post `IsPost` ，則傳回 true。 這是標準方式 ASP.NET Web Pages 中，以判斷是否使用初始要求 （GET 要求） 或回傳 （POST 要求）。 (GET 和 POST 有關的詳細資訊，請參閱資訊看板 「 HTTP GET 和 POST 和 IsPost 屬性的 「，在[ASP.NET Web Pages 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost)。)

    接下來，您會取得使用者從填入的值`Request.Form`物件，而您將它們放在變數中供稍後使用。 `Request.Form`物件包含所有已送出頁面上，與每個索引鍵所識別的值。 索引鍵相當於`name`您想要讀取之表單欄位的屬性。 例如，若要閱讀`companyname`欄位 （文字方塊），您使用`Request.Form["companyname"]`。

    表單值會儲存在`Request.Form`物件做為字串。 因此，當您有使用的數字或日期或是其他類型的值，您必須從字串轉換成該類型。 在範例中，`AsInt`方法的`Request.Form`用來將 （其中包含的員工計數） 的 [員工] 欄位的值轉換為整數。
2. 啟動您的瀏覽器的頁面、 填寫表單欄位中，然後按一下 **送出**。 此頁面會顯示您輸入的值。

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML 編碼的外觀和安全性
> 
> HTML 具有特殊用途的字元，例如`<`， `>`，和`&`。 如果這些特殊字元出現未預期的位置，它們可以讓的外觀和功能，您的網頁。 例如，在瀏覽器解譯`<`字元 （除非它後面接著空格） 開頭的 HTML 項目，例如`<b>`或`<input ...>`。 如果瀏覽器無法辨識的項目，則只會捨棄開頭的字串`<`直到達到的項目，它一次會辨識。 很明顯地，這可能會導致一些奇怪的轉譯的頁面。
> 
> HTML 編碼方式，是在瀏覽器將解譯成正確的符號的程式碼中取代這些保留的字元。 例如，`<`字元取代成`&lt;`並`>`字元取代成`&gt;`。 瀏覽器會將這些取代字串呈現為您想要查看的字元。
> 
> 它是個不錯的主意，若要使用 HTML 編碼的隨時顯示字串 （輸入） 您從使用者取得。 如果沒有，使用者可以嘗試取得您的網頁來執行惡意指令碼，或執行其他動作，對您的站台安全性的危害，或這不只是您所預期。 (這是特別重要，如果您接受使用者輸入、 將它儲存至，並稍後再顯示&#8212;例如部落格註解、 使用者檢閱，或像這樣。)
> 
> 若要避免這些問題，ASP.NET Web Pages 會自動進行 HTML 編碼的任何文字內容，您的程式碼輸出。 例如，當您顯示的變數或使用下列程式碼的運算式內容`@MyVar`，ASP.NET Web Pages 會自動將編碼的輸出。


## <a name="validating-user-input"></a>驗證使用者輸入

使用者會犯錯。 您要求他們填寫欄位，以致忘了，或您要求他們輸入員工的數目和它們改為輸入的名稱。 若要確定，表單有已正確填入您在處理之前，您可以驗證使用者的輸入。

此程序示範如何驗證所有的三個表單欄位，以確定使用者未保留空白。 您也可以檢查的員工計數的值是數字。 如果發生錯誤，您將會顯示錯誤訊息，告知使用者哪些值未通過驗證。

1. 在  *Form.cshtml*檔案中，取代為下列程式碼中的程式碼的第一個區塊： 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    若要驗證使用者輸入，您使用`Validation`協助程式。 藉由呼叫註冊必要的欄位`Validation.RequireField`。 藉由呼叫註冊其他驗證類型`Validation.Add`並指定要驗證的欄位和要執行的驗證類型。

    當執行網頁時，ASP.NET 會為您的所有驗證。 您可以呼叫來檢查結果`Validation.IsValid`，它會傳回 true，如果所有項目傳遞和驗證失敗的任何欄位為 false。 一般而言，您呼叫`Validation.IsValid`使用者輸入上執行任何處理之前。
2. 更新`<body>`加上三次呼叫的項目`Html.ValidationMessage`方法，就像這樣：

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    若要顯示驗證錯誤訊息，您可以呼叫 Html。`ValidationMessage` 並將它傳遞您想要的訊息欄位的名稱。
3. 執行網頁。 將欄位保留空白，然後按一下 **送出**。 您會看到錯誤訊息。

    ![如果螢幕擷取畫面顯示顯示錯誤訊息，如果使用者輸入未通過驗證。](4-working-with-forms/_static/image3.jpg)
4. 將字串 (例如，"ABC") 新增至**員工計數**欄位，然後按一下**送出**一次。 此時，您會看到錯誤，指出字串不正確的格式，也就是整數。

    ![如果螢幕擷取畫面顯示顯示錯誤訊息，使用者如果輸入 [員工] 欄位的字串。](4-working-with-forms/_static/image4.jpg)

ASP.NET Web Pages 提供多個選項可用來驗證使用者輸入，包括能夠自動執行驗證使用用戶端指令碼，讓使用者在瀏覽器取得立即意見反應。 請參閱[額外的資源](#Additional_Resources)稍後如需詳細資訊 區段。

## <a name="restoring-form-values-after-postbacks"></a>在回傳後正在還原的表單值

當您在上一節中測試頁面時，您可能已經注意到，如果您有驗證錯誤，您輸入 （不只是無效的資料） 的所有項目已消失，而且您必須重新輸入所有欄位的值。 此說明很重要的一點： 當您送出頁面、 處理它，並再一次呈現的頁面，頁面會從頭重新建立。 如您所見，這表示當送出時已在頁面中的任何值都會遺失。

您可以修正此問題，不過。 您可以存取所提交的值 (在`Request.Form`物件，因此在呈現的頁面時，您可以回表單欄位填入這些值。

1. 在  *Form.cshtml*檔案中，取代`value`屬性`<input>`使用的項目`value`屬性。: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value`的屬性`<input>`項目已設定為以動態方式讀取出的欄位值`Request.Form`物件。 第一次要求頁面時中的值`Request.Form`也都是空的物件。 這是沒問題，因為如此一來表單在空白。
2. 啟動您的瀏覽器的頁面、 填寫表格欄位或留白，然後按一下**送出**。 會顯示一個頁面顯示提交的值。

    ![form 5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

- [若要從 Web 使用者取得輸入 1,001 方法](https://msdn.microsoft.com/library/ms971057.aspx)
- [使用表單和處理使用者輸入](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [在 ASP.NET Web Pages 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)
- [在 HTML 表單中使用自動完成](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
