---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "導入 ASP.NET Web Pages-刪除資料庫資料 |Microsoft 文件"
author: tfitzmac
description: "本教學課程會示範如何刪除個別的資料庫項目。 它會假設您已完成透過更新 ASP.NET Web 的 pa 中的資料庫資料數列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>導入的 ASP.NET Web Pages-刪除資料庫資料
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何刪除個別的資料庫項目。 它會假設您已完成透過數列[更新 ASP.NET Web Pages 中的資料庫資料](updating-data.md)。
> 
> 您將學習：
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
> - `Database.Execute`方法執行 SQL`Delete`命令。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，您將學會如何更新現有的資料庫記錄。 本教學課程會很類似，只不過而不是更新記錄，將會刪除它。 處理程序完全一樣，除了刪除是比較簡單，所以本教學課程會簡短。

在*電影* 頁面上，您要更新`WebGrid`helper，因此它會顯示**刪除**旁邊隨著每個影片連結**編輯**您先前加入的連結。

![顯示每個影片的刪除連結的影片頁面](deleting-data/_static/image1.png)

與編輯，當您按一下**刪除**連結，它將您導向至其他頁面，電影資訊已在表單：

![刪除與電影顯示影片頁面](deleting-data/_static/image2.png)

接著，您可以按一下按鈕以永久刪除記錄。

## <a name="adding-a-delete-link-to-the-movie-listing"></a>將刪除連結新增至電影清單

您會先加入**刪除**連結至`WebGrid`協助程式。 此連結會類似於**編輯**您在上一個教學課程中加入的連結。

開啟*Movies.cshtml*檔案。

變更`WebGrid`加入資料行頁面主體中的標記。 以下是修改過的標記：

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

新的資料行是一個：

[!code-html[Main](deleting-data/samples/sample2.html)]

方格中的設定的方式，**編輯**方格中最左邊和**刪除**資料行是最右邊。 (沒有逗號之後`Year`資料行現在，如果您沒有注意。)沒有特別關於這些連結資料行的位置，而且您無法輕鬆地將它們放相鄰。 在此情況下，變更就會不同，使其更難以取得混。

![標示為顯示它們不相鄰的影片頁面與 [編輯] 和 [詳細資料] 連結](deleting-data/_static/image3.png)

新的資料行顯示的連結 (`<a>`元素) 的文字為 「 刪除 」。 連結的目標 (其`href`屬性) 是最終會解析成類似此 URL，使用的程式碼`id`值不同的每個影片：

[!code-css[Main](deleting-data/samples/sample3.css)]

此連結會叫用名為的頁面*DeleteMovie*並將它傳遞您所選取的電影識別碼。

本教學課程將不會移到此連結的建構方式的詳細資料，因為它是與幾乎完全相同**編輯**從上一個教學課程的連結 ([更新 ASP.NET Web Pages 中的資料庫資料](updating-data.md))。

## <a name="creating-the-delete-page"></a>建立刪除頁面

現在您可以建立將會為目標的網頁**刪除**方格中的連結。

> [!NOTE] 
> 
> **重要**先選取要刪除的記錄，然後使用不同的網頁和按鈕確認處理程序的技術是極為重要的安全性。 當您已閱讀先前的教學課程中，進行*任何*變更到您的網站應該*一律*完成使用表單&mdash;也就使用 HTTP POST 作業。 如果您所做變更站台，只要按一下 （亦即，使用 「 取得 」 作業） 的連結，人員無法簡單要求對您的網站，並刪除您的資料。 甚至索引您的站台為搜尋引擎編目程式可能只要下列連結會不小心刪除資料。
> 
> 當您的應用程式可讓您變更記錄的人時，您必須向使用者呈現記錄進行編輯嗎。 但是，您可能會想要跳過這個步驟的刪除記錄。 請勿略過該步驟，不過。 （它也是很有幫助使用者，請參閱記錄，並確認它們正在刪除這些 runbook 的記錄）。
> 
> 在後續的教學課程組中，您會看到如何將登入功能，因此使用者必須登入，然後再刪除記錄。


建立名為的頁面*DeleteMovie.cshtml*和取代功能的檔案，以下列標記：

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

這個標記就像是*EditMovie*頁面，而不是使用文字方塊中，除了 (`<input type="text">`)，包括標記`<span>`項目。 沒有任何這裡編輯。 您只需要為顯示電影詳細資料，讓使用者可以確保它們正在刪除右邊的電影。

標記已包含可讓使用者返回電影清單頁面的連結。

在*EditMovie*頁面上，選取的影片識別碼儲存在隱藏欄位。 （它是作為傳遞至頁面在第一次的查詢字串值。）沒有`Html.ValidationSummary`呼叫將會顯示驗證錯誤。 在此情況下，此錯誤可能是沒有影片識別碼已傳遞至網頁或影片識別碼無效。 如果其他人必須先選取影片，以在執行此頁面，就可能發生這種情況下*電影*頁面。

按鈕標題是**刪除影片**，且其名稱屬性設為`buttonDelete`。 `name`屬性將會在程式碼中用來識別提交表單的按鈕。

您必須撰寫程式碼 1） 讀取電影詳細資料，當第一次顯示網頁，2） 實際刪除影片，當使用者按一下按鈕。

## <a name="adding-code-to-read-a-single-movie"></a>加入程式碼來讀取單一影片

在頂端*DeleteMovie.cshtml*頁面上，加入下列程式碼區塊：

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

此標記是與對應的程式碼，在相同*EditMovie*頁面。 它會取得從查詢字串的影片識別碼，並會使用從資料庫讀取記錄的識別碼。 程式碼包含驗證測試 (`IsInt()`和`row != null`) 傳遞至頁面的影片識別碼是否有效。

請記住這段程式碼應該只執行網頁執行第一次。 您不希望重新讀取電影中的記錄資料庫，當使用者按一下**刪除影片** 按鈕。 因此，程式碼來讀取內部測試指出電影`if(!IsPost)`&mdash;也就是*要求不是在 post 作業 （表單送出）*。

## <a name="adding-code-to-delete-the-selected-movie"></a>加入程式碼，若要刪除選取的影片

若要刪除的影片，當使用者按一下按鈕，加入下列程式碼的右括號之內`@`區塊：

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

此程式碼是類似的程式碼來更新現有的記錄，但更簡單。 程式碼基本上會執行 SQL`Delete`陳述式。

 在*EditMovie*  頁面上，程式碼是在`if(IsPost)`區塊。 這次`if()`是更複雜的條件： 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

有以下兩種情況。 第一個方法是，在提交頁面，如您所見之前&mdash; `if(IsPost)`。

第二個條件是`!Request["buttonDelete"].IsEmpty()`，這表示要求是否有一個名為物件`buttonDelete`。 不可否認，它是間接測試哪一個按鈕送出表單的方式。 如果表單包含多個提交按鈕，只有已按下按鈕的名稱會出現在要求中。 因此，邏輯上，如果特定的按鈕名稱會出現在要求中&mdash;或在程式碼中所述，如果該按鈕不是空的&mdash;，會提交表單的按鈕。

`&&`運算子表示 「 和 」 (邏輯 AND)。 因此整個`if`條件是...

*此要求為 post （不是第一次要求）*  
  
 AND  
  
 `buttonDelete`*按鈕已送出表單的按鈕。*

（事實上，此頁面） 此表單包含一個按鈕，因此其他測試`buttonDelete`技術上來說則不需要。 儘管如此，您要執行的作業將會永久移除資料。 因此，您需要為務必盡可能使用者明確提出要求時，才在執行此作業。 例如，假設您之後展開此頁面，並加入其他按鈕。 即使如此，刪除影片的程式碼才會執行`buttonDelete`button 已按下。

在*EditMovie*  頁面上，您從隱藏的欄位取得識別碼，然後再執行 SQL 命令。 語法`Delete`陳述式：

`DELETE FROM table WHERE ID = value`

請務必包含`WHERE`子句和識別碼。 如果您省略 WHERE 子句，*將刪除資料表中的所有記錄*。 如您所見，您的識別碼值傳遞給 SQL 命令使用的預留位置。

## <a name="testing-the-movie-delete-process"></a>測試影片刪除處理程序

現在您可以測試。 執行*電影*頁面，然後按一下**刪除**電影旁邊。 當*DeleteMovie*頁面出現時，按一下**刪除影片**。

![刪除與刪除影片按鈕反白顯示的電影頁面](deleting-data/_static/image4.png)

當您按一下按鈕時，程式碼會刪除電影，並傳回的電影清單。 您可以在那里搜尋已刪除的影片，並確認它已被刪除。

## <a name="coming-up-next"></a>接下來

下一個教學課程會示範如何提供網站上的所有頁面，常見的外觀和版面配置。

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>電影頁面 （更新與刪除連結） 的完整清單

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>DeleteMovie 頁面的完整清單

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](../introducing-razor-syntax-c.md)
- [SQL DELETE 陳述式](http://www.w3schools.com/sql/sql_delete.asp)W3Schools 站台上

>[!div class="step-by-step"]
[上一頁](updating-data.md)
[下一頁](layouts.md)
