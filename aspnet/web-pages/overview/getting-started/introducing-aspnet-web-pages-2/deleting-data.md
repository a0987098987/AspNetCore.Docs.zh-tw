---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 簡介 ASP.NET Web Pages-刪除資料庫的資料 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何刪除個別的資料庫項目。 它會假設您已完成透過 ASP.NET Web 的 pa 中的 更新資料庫資料數列...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020438"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>ASP.NET Web Pages 簡介-刪除資料庫資料
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何刪除個別的資料庫項目。 它假設您已完成透過數列[更新 ASP.NET Web Pages 中的資料庫資料](updating-data.md)。
> 
> 您將學到什麼：
> 
> - 如何從記錄清單中選取個別的記錄。
> - 如何刪除資料庫中的單一記錄。
> - 如何檢查特定按鈕已按下表單中。
>   
> 
> 功能/技術討論：
> 
> - `WebGrid`協助程式。
> - SQL`Delete`命令。
> - `Database.Execute`方法來執行 SQL`Delete`命令。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，您已了解如何更新現有的資料庫記錄。 本教學課程是類似，只不過是而不是更新記錄，您將會刪除它。 處理程序也大致相同，不同之處在於刪除是更簡單，因此本教學課程中會很短。

在*電影*頁面上，您將更新`WebGrid`協助程式，因此它會顯示**刪除**隨附的每個電影旁邊的連結**編輯**您先前加入的連結。

![顯示每一部電影的刪除連結的影片頁面](deleting-data/_static/image1.png)

與編輯，當您按一下 **刪除**連結，它會帶您到不同的頁面中，電影資訊已經在表單中：

![刪除顯示對影片的影片頁面](deleting-data/_static/image2.png)

您可以按一下按鈕以永久刪除的記錄。

## <a name="adding-a-delete-link-to-the-movie-listing"></a>將刪除連結新增至電影清單

您會開始藉由新增**刪除**連結至`WebGrid`協助程式。 這個連結是類似**編輯**您在上一個教學課程中加入的連結。

開啟*Movies.cshtml*檔案。

變更`WebGrid`標記中加入資料行頁面的主體。 以下是修改過的標記：

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

新的資料行是如下：

[!code-html[Main](deleting-data/samples/sample2.html)]

方格中的設定的方式**編輯**資料行是在方格中最左邊，**刪除**資料行是最右邊。 (沒有逗號之後`Year`資料行現在，如果您沒有注意。)沒有特別關於這些連結資料行在哪裡，以及您可以輕鬆將它們放彼此相鄰。 在此情況下，它們是個別使它們更難以取得混。

![標示為顯示並未彼此相鄰的 movies 頁面，使用 [編輯] 和 [詳細資料] 連結](deleting-data/_static/image3.png)

新的資料行顯示的連結 (`<a>`項目) 的文字會顯示 [刪除]。 連結的目標 (及其`href`屬性) 是最終會解析為類似此 URL 中，使用的程式碼`id`每一部電影的不同值：

[!code-css[Main](deleting-data/samples/sample3.css)]

此連結會叫用名為的頁面*DeleteMovie*並將它傳遞您所選取的電影識別碼。

本教學課程不會探討如何建構此連結，因為它幾乎等同於**編輯**從上一個教學課程的連結 ([更新 ASP.NET Web Pages 中的資料庫資料](updating-data.md))。

## <a name="creating-the-delete-page"></a>建立 [刪除] 頁面

現在您可以建立將會針對目標頁面**刪除**方格中的連結。

> [!NOTE] 
> 
> **重要**先選取 刪除的記錄，然後確認此程序中使用不同的網頁和按鈕的技術是非常重要的安全性。 當您已閱讀先前教學課程中，進行*任何*變更至您的網站應該*一律*完成使用表單&mdash;也就使用 HTTP POST 作業。 如果您所做變更站台，只要按一下的連結 （亦即，使用 「 取得 」 作業），人無法對您的網站進行簡單的要求，並刪除您的資料。 即使您的網站在索引為搜尋引擎編目程式可能只是透過下列連結在無意間刪除資料。
> 
> 當您的應用程式可讓您變更記錄的人員時，您必須向使用者顯示的記錄，仍要編輯的。 不過，您可能想要跳過此步驟，刪除記錄。 請勿略過該步驟，不過。 （它也是讓使用者看到的記錄，並確認它們要刪除的記錄，它們能幫助。）
> 
> 在後續的教學課程組中，您會看到如何將登入功能，因此使用者必須登入，再刪除記錄。


建立名為的頁面*DeleteMovie.cshtml*和取代功能的檔案，以下列標記：

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

此標記就像是*EditMovie*頁面，除了，而不是使用文字方塊中 (`<input type="text">`)，標記包括`<span>`項目。 這裡沒有編輯。 您必須執行的只是顯示電影詳細資料，以便使用者可以確定他們要刪除正確的影片。

標記已包含可讓使用者返回電影清單頁面的連結。

依照*EditMovie*  頁面上，選取的影片識別碼儲存在隱藏欄位。 （它會傳遞至頁面一開始作為查詢字串值。）沒有`Html.ValidationSummary`會顯示驗證錯誤的呼叫。 在此情況下，錯誤可能是任何影片識別碼已傳遞至網頁或影片識別碼無效。 如果某個人必須先選取影片，以在執行此頁面，可能會發生這種情況下*電影*頁面。

是按鈕標題**刪除電影**，且其名稱屬性設為`buttonDelete`。 `name`屬性會在程式碼中用來識別按鈕送出表單。

您必須撰寫 1） 讀取電影詳細資料，當第一次顯示網頁的程式碼，以及 2） 實際刪除電影，當使用者按一下按鈕。

## <a name="adding-code-to-read-a-single-movie"></a>新增程式碼來讀取單一影片

在頂端*DeleteMovie.cshtml*頁面上，新增下列程式碼區塊：

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

此標記是與對應的程式碼，在相同*EditMovie*頁面。 它會取得從查詢字串的電影識別碼，並會使用從資料庫讀取記錄的識別碼。 程式碼包含驗證測試 (`IsInt()`和`row != null`) 以確定傳遞至頁面的電影識別碼無效。

請記住此程式碼應該只執行此頁面會執行第一次。 您不打算重新讀取資料庫中的電影記錄，當使用者按一下**刪除電影** 按鈕。 因此，程式碼來讀取內部測試指出電影`if(!IsPost)`&mdash;亦即*要求不是 post 作業 （表單提交）*。

## <a name="adding-code-to-delete-the-selected-movie"></a>加入程式碼以刪除選取的影片

使用者按一下按鈕時，請刪除電影，新增下列程式碼的右括號之內`@`區塊：

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

此程式碼是類似的程式碼更新現有的記錄，但更簡單。 程式碼基本上會執行 SQL`Delete`陳述式。

 依照*EditMovie*頁面上，程式碼是在`if(IsPost)`區塊。 此時，`if()`條件是稍微複雜一點： 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

有以下兩種情況。 第一個是，正在提交頁面，如您所見之前&mdash; `if(IsPost)`。

第二個條件是`!Request["buttonDelete"].IsEmpty()`，這表示該要求具有名為物件`buttonDelete`。 無可否認地，這是間接的方式，測試哪一個按鈕送出表單。 如果表單包含多個提交按鈕，只有按下按鈕的名稱會出現在要求中。 因此，邏輯上來說，如果特定按鈕的名稱會出現在要求&mdash;中所述的程式碼中，如果該按鈕不是空的或&mdash;是提交表單的按鈕。

`&&`運算子表示 「 和 」 (邏輯 AND)。 因此整個`if`條件是...

*此要求是某篇文章 （而不是第一次要求）*  
  
 AND  
  
`buttonDelete` *按鈕已提交表單的按鈕。*

（事實上，此頁面） 此表單包含只有一個按鈕，以便其他測試`buttonDelete`技術上來說並不需要。 儘管如此，您要執行的作業，將會永久移除資料。 因此，您想要為確保越好，您要執行此作業，使用者已明確要求它時，才。 例如，假設您於稍後展開此頁面，並加入其他按鈕。 即使如此，刪除影片的程式碼才會執行`buttonDelete`所按的按鈕。

依照*EditMovie* ] 頁面上，您取得的識別碼，從隱藏的欄位，並接著執行 [SQL 命令。 語法`Delete`陳述式：

`DELETE FROM table WHERE ID = value`

請務必包含`WHERE`子句和識別碼。 如果您省略 WHERE 子句中，*資料表中的所有記錄將會都刪除*。 如您所見，您傳遞的識別碼值給 SQL 命令所使用的預留位置。

## <a name="testing-the-movie-delete-process"></a>測試影片的刪除程序

現在您可以測試。 執行*電影*頁面，然後按一下**刪除**電影旁邊。 當*DeleteMovie*  頁面出現時，按一下**刪除電影**。

![反白顯示的刪除電影按鈕刪除影片頁面](deleting-data/_static/image4.png)

當您按一下按鈕時，程式碼會刪除電影，並傳回的電影清單。 您可以在那里搜尋已刪除的影片，並確認它已被刪除。

## <a name="coming-up-next"></a>接下來的下一步

下一個教學課程會示範如何在您的網站上提供的所有頁面，常見的外觀和版面配置。

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>（更新與刪除連結） 的電影頁面的完整清單

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>DeleteMovie 頁面的完整清單

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](../introducing-razor-syntax-c.md)
- [SQL DELETE 陳述式](http://www.w3schools.com/sql/sql_delete.asp)W3Schools 站台上

> [!div class="step-by-step"]
> [上一頁](updating-data.md)
> [下一頁](layouts.md)
