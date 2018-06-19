---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: 使用 ASP.NET Web Pages (Razor) 網站中的 HTML 表單 |Microsoft 文件
author: tfitzmac
description: 表單是您在其中放置使用者輸入的控制，例如文字方塊、 核取方塊、 選項按鈕和下拉式清單的 HTML 文件區段。 使用表單北...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
ms.locfileid: "28046426"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>使用 ASP.NET Web Pages (Razor) 網站中的 HTML 表單
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章描述如何處理 HTML 表單 （含有文字方塊和按鈕） 當您使用的 ASP.NET Web Pages (Razor) 網站。
> 
> **您將學習：** 
> 
> - 如何建立 HTML 表單。
> - 如何讀取表單中的使用者輸入。
> - 如何驗證使用者輸入。
> - 如何將送出頁面之後，還原表單值。
> 
> 這些是 ASP.NET 程式設計概念，文章中：
> 
> - `Request` 物件。
> - 輸入的驗證。
> - HTML 編碼方式。
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
2. 在根資料夾中，會建立名為網頁*Form.cshtml*並輸入下列標記：

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. 啟動您的瀏覽器中的頁面。 (在 WebMatrix 中，在**檔案**工作區中，以滑鼠右鍵按一下檔案，然後選取**瀏覽器中啟動**。)具有三個輸入欄位的簡單表單和**送出**按鈕會顯示。

    ![具有三個文字方塊的表單螢幕擷取畫面。](4-working-with-forms/_static/image1.jpg)

    此時，如果您按一下**送出**按鈕時，會發生任何事。 若要讓表單很有用，您必須加入一些程式碼將在伺服器上執行。

## <a name="reading-user-input-from-the-form"></a>讀取表單中的使用者輸入

若要處理的表單，您加入讀取送出的欄位值，並加以處理的程式碼。 此程序會示範如何讀取的欄位，並顯示在頁面上的使用者輸入。 （在生產應用程式中，您通常執行更有趣的事情，與使用者輸入。 您將會執行，使用資料庫的相關文件中。）

1. 在頂端*Form.cshtml*檔案中，輸入下列程式碼：

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    當使用者第一次要求頁面時，會顯示空的表單。 使用者 （這會是您） 填入表單，然後按一下 **送出**。 這會將提交 （文章） 伺服器的使用者輸入。 根據預設，要求會移至相同的頁面 (也就是*Form.cshtml*)。

    當您提交頁面此時，您輸入的值會顯示表單的正上方：

    ![如果螢幕擷取畫面顯示頁面上顯示您所輸入的值。](4-working-with-forms/_static/image2.jpg)

    查看網頁的程式碼。 第一次使用`IsPost`方法，以判斷是否要公佈的頁面&#8212;亦即是否使用者按下**送出** 按鈕。 如果這是使用 post 要求`IsPost`傳回 true。 這是標準方式 ASP.NET Web Pages 中，以判斷是否正在使用的初始要求 （GET 要求） 或回傳 （POST 要求）。 (GET 和 POST 需詳細資訊，請參閱資訊看板 「 HTTP GET 和 POST 和 IsPost 屬性的"，在[ASP.NET Web Pages 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost)。)

    接下來，您會取得從填入使用者的值`Request.Form`物件，而您將它們放在變數中供稍後。 `Request.Form`物件包含所有已送出頁面上，每個索引鍵所識別的值。 機碼相當於`name`您想要讀取的表單欄位的屬性。 例如，若要讀取`companyname`欄位 （文字方塊），您使用`Request.Form["companyname"]`。

    表單值會儲存在`Request.Form`物件做為字串。 因此，當您必須使用的數字或日期，或是其他類型的值，您必須從字串轉換成該類型。 在範例中，`AsInt`方法`Request.Form`用來將員工欄位 （其中包含員工計數） 的值轉換成整數。
2. 啟動網頁瀏覽器中的，填寫表單的欄位，然後按一下**送出**。 頁面會顯示您輸入的值。

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML 編碼的外觀和安全性
> 
> HTML 具有特殊用途的字元，例如`<`， `>`，和`&`。 如果這些特殊字元出現未預期的位置，它們可以廢棄的外觀和功能的網頁。 例如，在瀏覽器會解譯`<`like HTML 項目，開頭為字元 （除非它後接空格）`<b>`或`<input ...>`。 如果瀏覽器無法辨識的項目，則只會捨棄的字串開頭為`<`直到達到的項目，它一次會辨識。 很明顯地，這可能會導致一些奇怪的轉譯頁面中。
> 
> HTML 編碼的瀏覽器將解譯成正確的符號的程式碼以取代這些保留的字元。 例如，`<`字元取代`&lt;`和`>`字元取代`&gt;`。 瀏覽器會將這些取代字串轉譯成您想要查看的字元。
> 
> 它是個不錯的主意，若要使用 HTML 編碼顯示字串的任何時間 （輸入），您會取得使用者。 如果沒有，使用者可以嘗試取得您的網頁執行惡意指令碼，或做其他事，危害您的站台安全性的或者只是無法預期。 (這是非常重要的如果您接受使用者輸入、 將其儲存在某個位置，並稍後再顯示&#8212;例如部落格的註解、 使用者檢閱或類似的。)
> 
> 為了避免這些問題，ASP.NET Web 網頁會自動將 HTML 編碼的任何文字內容，您從程式碼輸出。 例如，當您顯示變數或運算式，例如使用程式碼的內容`@MyVar`，ASP.NET Web Pages 自動編碼輸出。


## <a name="validating-user-input"></a>驗證使用者輸入

使用者會犯錯。 您要求他們以填滿的欄位中，他們忘了，或您要求使用者輸入的員工人數和它們會改為輸入的名稱。 若要確定您的表單已滿正確地處理它之前，您可以驗證使用者的輸入。

此程序示範如何驗證所有的三個表單欄位，以確定使用者未保留空白。 您也檢查員工計數的值是數字。 如果沒有錯誤，您將會顯示錯誤訊息，告知使用者哪些值未通過驗證。

1. 在*Form.cshtml*檔案中，取代為下列程式碼中的程式碼的第一個區塊： 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    若要驗證使用者輸入，您使用`Validation`協助程式。 您註冊必要的欄位，藉由呼叫`Validation.RequireField`。 您藉由呼叫註冊其他類型的驗證`Validation.Add`並指定要驗證的欄位和要執行的驗證類型。

    頁面執行時，ASP.NET 會為您的所有驗證。 您可以藉由呼叫來檢查結果`Validation.IsValid`，它會傳回 true，如果所有項目傳遞和任何欄位驗證失敗為 false。 通常您會呼叫`Validation.IsValid`在執行任何處理使用者輸入之前。
2. 更新`<body>`項目加上三次呼叫`Html.ValidationMessage`方法，就像這樣：

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    若要顯示驗證錯誤訊息，您可以呼叫 Html。`ValidationMessage` 並將它傳遞您想要的訊息欄位的名稱。
3. 執行網頁。 將欄位保留空白，然後按一下**送出**。 您看到錯誤訊息。

    ![如果螢幕擷取畫面顯示顯示錯誤訊息，如果使用者輸入未通過驗證。](4-working-with-forms/_static/image3.jpg)
4. 將字串 (例如，"ABC") 加入至**員工計數**欄位，然後按一下**送出**一次。 此時，您會看到錯誤指出，字串不正確的格式，也就是整數。

    ![如果螢幕擷取畫面顯示顯示錯誤訊息，如果使用者在 [員工] 欄位輸入字串。](4-working-with-forms/_static/image4.jpg)

ASP.NET Web 網頁會提供更多選項，驗證使用者輸入，包括自動執行驗證使用用戶端指令碼，以便讓使用者在瀏覽器中取得的立即回應的能力。 請參閱[其他資源](#Additional_Resources)> 一節稍後如需詳細資訊。

## <a name="restoring-form-values-after-postbacks"></a>在回傳後正在還原的表單值

當您在上一節中測試網頁時，您可能已注意到，如果發生驗證錯誤，您輸入 （不只是無效的資料） 的所有項目已消失，而且您必須重新輸入所有欄位的值。 下列說明處理很重要的一點： 當您送出頁面、 處理它，並再一次呈現頁面，頁面會從頭重新建立。 如您所見，這表示已在頁面時送出的任何值都會遺失。

您可以修正此問題，不過。 您可以存取已送出的值 (在`Request.Form`物件，因此頁面轉譯時，您可以回到表單欄位填入這些值。

1. 在*Form.cshtml*檔案中，取代`value`屬性`<input>`項目使用`value`屬性。: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    `value`屬性`<input>`項目已設定為以動態方式讀取欄位值，超出`Request.Form`物件。 第一次要求頁面時中的值`Request.Form`是所有空的物件。 這是沒問題，因為如此一來表單是空白。
2. 啟動網頁瀏覽器中的，填寫表單的欄位或將它們留白，然後按一下**送出**。 會顯示頁面，其中顯示送出的值。

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

- [若要取得的 Web 使用者輸入 1,001 方法](https://msdn.microsoft.com/library/ms971057.aspx)
- [使用表單和處理使用者輸入](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [在 ASP.NET Web Pages 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)
- [在 HTML 表單中使用自動完成](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
