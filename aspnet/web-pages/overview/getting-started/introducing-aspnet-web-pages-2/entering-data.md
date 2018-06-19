---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 導入 ASP.NET Web Pages-輸入資料庫的資料，使用表單 |Microsoft 文件
author: tfitzmac
description: 本教學課程會示範如何建立項目表單，然後輸入 得到的資料從表單到資料庫資料表時使用 ASP.NET Web Pages （...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: bbccf8134e90c19e29efaa5afe1e46e15320c189
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897958"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>導入 ASP.NET Web Pages-使用表單中輸入資料庫的資料
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何建立項目表單，然後輸入 得到的資料從表單到資料庫資料表時使用 ASP.NET Web Pages (Razor)。 它會假設您已完成透過數列[基本概念的 HTML 表單中的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581)。
> 
> 您將學習：
> 
> - 深入了解如何處理項目表單。
> - 如何在資料庫中加入資料 (insert)。
> - 如何確定使用者有 （如何驗證使用者輸入） 表單中輸入必要的值。
> - 如何顯示驗證錯誤。
> - 如何從目前的頁面跳至另一個頁面。
>   
> 
> 功能/技術討論：
> 
> - `Database.Execute` 方法
> - SQL`Insert Into`陳述式
> - `Validation`協助程式。
> - `Response.Redirect` 方法


## <a name="what-youll-build"></a>您將建置

教學課程中先前的範例說明了如何建立資料庫，您必須輸入資料庫的資料藉由編輯直接在 WebMatrix 中，使用資料庫**資料庫**工作區。 在大部分的應用程式，這是不可行的方式，可將資料放入資料庫，不過。 因此在本教學課程中，您將建立的 web 型介面，可讓您或任何人輸入的資料，並將它儲存到資料庫。

您要建立的頁面，您可以在此輸入新的電影。 頁面會包含具有的欄位 （文字方塊），您可以在其中輸入電影標題、 類型和年份的項目表單。 網頁看起來像此頁面：

![瀏覽器中的 [新增電影] 頁面](entering-data/_static/image1.png)

文字方塊中，將會是 HTML`<input>`項目，看起來像此標記：

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>建立基本的項目表單

建立名為的頁面*AddMovie.cshtml*。

取代什麼是以下列標記檔案中。 覆寫所有項目。您稍後要加入的程式碼區塊頂端。

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

這個範例示範典型的 HTML 建立表單。 它會使用`<input>`[提交] 按鈕和文字方塊中的項目。 使用標準所建立之文字方塊的標題`<label>`項目。 `<fieldset>`和`<legend>`項目放置在表單上四處 nice 方塊。

請注意，在此頁面上，`<form>`項目使用`post`做為值`method`屬性。 在上一個教學課程中，您已建立一個表單，用`get`方法。 因為雖然表單送出至伺服器的值，但要求並未進行任何變更，這是正確的。 一樣是擷取資料以不同的方式。 不過，在這個頁面上您*將*進行變更，您要加入新的資料庫記錄。 因此，應該使用這種形式`post`方法。 (如需詳細資訊之間的差異`GET`和`POST`作業，請參閱[GET、 POST 和 HTTP 動詞命令安全性](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)在上一個教學課程中的 [資訊看板]。)

請注意，每個文字方塊具有`name`元素 (`title`， `genre`， `year`)。 如同上一個教學課程中，這些名稱很重要，因為您必須有這些名稱，因此您可以稍後取得使用者的輸入。 您可以使用任何名稱。 最好先使用有意義的名稱，協助您記住您正在使用哪些資料。

`value`每個屬性`<input>`項目包含的位元的 Razor 程式碼 (例如， `Request.Form["title"]`)。 在上一個教學課程，以保留 （如果有的話） 在文字方塊中輸入的值中，在提交表單之後，您學到這個技巧的版本。

## <a name="getting-the-form-values"></a>取得表單值

接著，您將處理表單的程式碼。 您將在大綱中，進行下列項目：

1. 檢查網頁是否正在回傳 （提交）。 您想要您的程式碼只有在使用者按下按鈕，不會在頁面上第一次執行時，才執行。
2. 取得使用者在文字方塊中輸入的值。 在此情況下，因為使用的表單`POST`動詞命令，取得從表單值`Request.Form`集合。
3. 為新記錄中插入值*電影*資料庫資料表。

在檔案頂端加入下列程式碼：

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

前幾行建立的變數 (`title`， `genre`，和`year`) 來保存從文字方塊中的值。 線條`if(IsPost)`可確保所設定的變數*只*當使用者按一下**新增電影**按鈕 — 也就是當表單張貼。

如同先前的教學課程中，您會取得文字方塊的值使用如下的運算式`Request.Form["name"]`，其中*名稱*名稱`<input>`項目。

變數的名稱 (`title`， `genre`，和`year`) 是任意的。 要指派給名稱`<input>`項目，您要的任何項目呼叫。 (變數的名稱不一定要比對的 name 屬性`<input>`項目表單上的。)但是如同`<input>`項目，最好使用變數的名稱，以反映它們所包含的資料。 當您撰寫程式碼時，則 一致的名稱讓您更輕鬆地記住您正在使用哪些資料。

## <a name="adding-data-to-the-database"></a>將資料加入至資料庫

在程式碼中封鎖您剛加入，請直接*內*右大括號 ( `}` ) 的`if`封鎖 （不只是在程式碼區塊） 內，加入下列程式碼：

[!code-csharp[Main](entering-data/samples/sample3.cs)]

這個範例是類似於您用來擷取和顯示資料的上一個教學課程中的程式碼。 開頭的行`db =`像之前，資料庫和下一行定義 SQL 陳述式，一次開啟你之前。 不過，此時它會定義 SQL`Insert Into`陳述式。 下列範例顯示的一般語法`Insert Into`陳述式：

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

換句話說，您指定的資料表中插入，則列出的資料行中插入，並列出要插入之值。 （如之前所述，SQL 是不區分大小寫，但有些人大寫的關鍵字，讓您更輕鬆地讀取命令）。

要插入的資料行已經列在命令中 — `(Title, Genre, Year)`。 有趣的部分，就是您從文字方塊中，到取得的值`VALUES`命令的一部分。 而不是實際的值，您會看到`@0`， `@1`，和`@2`，當然，這是預留位置。 當您執行命令 (在`db.Execute`列)，傳遞所得的文字方塊的值。

**重要！** 請記住，您應該永遠包含線上使用者輸入的 SQL 陳述式中的資料的唯一方式是使用預留位置，如下所示 (`VALUES(@0, @1, @2)`)。 串連成 SQL 陳述式的使用者輸入時，如果您自行開啟 SQL 插入式攻擊，如所述[表單基本 ASP.NET Web Pages 中](https://go.microsoft.com/fwlink/?LinkId=251581)（上一個教學課程）。

仍內部`if`封鎖，加入下列一行之後`db.Execute`行：

[!code-css[Main](entering-data/samples/sample4.css)]

新的電影已插入至資料庫之後，這一行就會將您 （重新導向） 跳到*電影*頁面上，才可看到您剛才輸入的電影。 `~`運算子表示 「 網站的根。 」 (`~`運算子只適用於不在 HTML 中的 ASP.NET 網頁通常。)

此範例看起來的完整程式碼區塊：

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>測試插入命令 （目前）

您未完成，但現在已測試的好時機。

在 WebMatrix 中的檔案樹狀結構檢視中，以滑鼠右鍵按一下*AddMovie.cshtml*頁面，然後按一下**瀏覽器中啟動**。

![瀏覽器中的 [新增電影] 頁面](entering-data/_static/image2.png)

(如果您得到不同的網頁瀏覽器中，請確定 URL 是`http://localhost:nnnnn/AddMovie`)，其中*nnnnn*是您所使用的連接埠號碼。)

您未取得錯誤網頁嗎？ 如果是的話，請仔細閱讀，並確定該程式碼看起來完全什麼先前已列出。

在表單中輸入電影&mdash;比方說，使用"公民 Kane"、"戲劇 」 和 「 1941"。 （或任何）。然後按一下 **新增電影**。

如果一切順利，您要重新導向至*電影*頁面。 請確定其中已列出新的電影。

![顯示新的電影頁面加入影片](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>驗證使用者輸入

請返回*AddMovie*頁面上，或執行一次。 輸入另一部影片，但這次，請輸入只標題&mdash;，例如輸入"登 ' 中 Rain"。 然後按一下 **新增電影**。

正在重新導向至*電影*頁面上一次。 您可以找到新的電影，但不完整。

![影片頁面上顯示新的電影遺失某些值，](entering-data/_static/image4.png)

當您建立*電影*資料表中，您明確地說，欄位不可以是 null。 這裡您有新的電影，項目表單時，您要將欄位保留空白。 這是錯誤。

在此情況下，並未實際引發資料庫 (或*擲回*) 錯誤。 您沒有因此提供內容類型或年中的程式碼*AddMovie*頁面被視為這些值所謂*空白字串*。 當 SQL`Insert Into`命令執行、 內容類型及年欄位沒有有用的資料，但不為 null。

很明顯地，您不想要讓使用者輸入半空白電影資訊到資料庫。 解決方法是驗證使用者的輸入。 一開始，驗證將會只是確定使用者已為所有欄位都輸入的值 （也就是無一包含空字串）。

> [!TIP]
> 
> **Null 和空字串**
> 
> 在程式設計中，沒有區分不同的概念的 「 任何值 」。 值，一般是*null*如果從未已設定或以任何方式初始化。 相反地，預期字元資料 （字串） 的變數可以將*空字串*。 在此情況下，值不是 null。它只已明確設定為字串，其長度為零的字元。 這些兩個陳述式顯示的差異：
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> 這有點較為複雜，但是很重要的一點是`null`代表一種不明的狀態。
> 
> 現在，然後務必了解剛好有值為 null 時，而只是空字串時。 在程式碼*AddMovie*  頁面上，您收到的值的文字方塊使用`Request.Form["title"]`，依此類推。 當頁面第一次執行時 （在您按一下按鈕時）、 值`Request.Form["title"]`為 null。 當您送出表單，但是`Request.Form["title"]`取得的值`title`文字方塊。 並不明顯，但空白的文字方塊不是 null。它只會包含在它的空字串。 因此當程式碼執行以回應 [] 按鈕按一下，`Request.Form["title"]`中有為空字串。
> 
> 這個區別為何如此重要？ 當您建立*電影*資料表中，您明確地說，欄位不可以是 null。 但是這裡您有新的電影，項目表單，而且您正在將欄位保留空白。 您會合理預期資料庫會抱怨當您嘗試要儲存新的電影是沒有內容類型或年份的值。 但是，這是點&mdash;即使這些文字方塊留白，值不為 null，而空字串。 如此一來，您就可以將空白這些資料行的資料庫來儲存新電影&mdash;但不是 null ！ &mdash; 值。 因此，您必須確定使用者不會提交空的字串，您可以先驗證使用者的輸入。


### <a name="the-validation-helper"></a>驗證 Helper

ASP.NET 網頁包括 helper &mdash; `Validation` helper&mdash;可用來確定使用者輸入符合您需求的資料。 `Validation`協助程式是其中一個內建到 ASP.NET Web Pages 中，所以您不需要使用 NuGet，您在先前的教學課程安裝 Gravatar 協助程式的方式將它安裝為封裝的協助程式。

若要驗證使用者的輸入，將會執行下列作業：

- 您可以使用程式碼，指定您想要要求在頁面上的文字方塊中的值。
- 使電影資訊加入至資料庫的所有項目正確驗證時，才放置測試的程式碼。
- 加入程式碼來顯示錯誤訊息的標記。

中的程式碼區塊中*AddMovie*頁面上，往右上在變數宣告之前，最上方加入下列程式碼：

[!code-csharp[Main](entering-data/samples/sample7.cs)]

您呼叫`Validation.RequireField`一次是針對每個欄位 (`<input>`項目) 您要需要項目。 您也可以新增自訂錯誤訊息，每個呼叫，您在這裡看到的一樣。 （我們改變只是為了顯示，您可以將任何您喜歡的項目那里訊息）。

如果沒有問題，您會想要避免新的電影資訊插入資料庫。 在`if(IsPost)`封鎖，請使用`&&`(邏輯 AND) 若要加入測試的另一個條件`Validation.IsValid()`。 當您完成時，整個`if(IsPost)`區塊看起來像此程式碼：

[!code-csharp[Main](entering-data/samples/sample8.cs)]

如果沒有與任何使用您註冊之欄位的驗證錯誤`Validation`helper，`Validation.IsValid`方法會傳回 false。 在此情況下，無該區塊中的程式碼會執行，並讓任何無效的電影項目會不插入至資料庫。 當然您不到重新導向時*電影*頁面。

完整的程式碼區塊中，包括驗證程式碼現在看起來像此範例中：

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>顯示驗證錯誤

最後一個步驟是顯示任何錯誤訊息。 您可以顯示個別的訊息，每個驗證錯誤，或您可以顯示摘要，或兩者。 此教學課程中，您將會執行同時，讓您能夠查看它的運作方式。

每個旁邊`<input>`正在驗證的項目，請呼叫`Html.ValidationMessage`方法並將它傳遞的名稱`<input>`正在驗證的項目。 您將`Html.ValidationMessage`方法正確的地方出現的錯誤訊息。 當頁面執行，則`Html.ValidationMessage`方法呈現`<span>`位置驗證錯誤的項目。 (如果沒有發生錯誤，`<span>`項目會呈現，但在它沒有任何文字。)

變更頁面中的標記，使其包含`Html.ValidationMessage`每三個方法`<input>`元素在頁面上，例如此範例中：

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

若要查看摘要的運作方式，也加入下列標記和程式碼後緊接著`<h1>Add a Movie</h1>`頁面上的元素：

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

根據預設，`Html.ValidationSummary`方法會顯示在清單中的所有驗證訊息 (`<ul>`內的項目`<div>`項目)。 如同`Html.ValidationMessage`方法時，永遠會轉譯驗證摘要的標記; 如果沒有任何錯誤，會呈現任何清單項目。

摘要可以是顯示驗證訊息，而不是使用的替代方式`Html.ValidationMessage`方法，以顯示每個欄位特有的錯誤。 或者，您可以使用的摘要和詳細資料。 您可以使用或`Html.ValidationSummary`方法來顯示一般錯誤，然後使用 個別`Html.ValidationMessage`呼叫，以顯示詳細資料。

[完成] 頁面現在看起來像此範例中：

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

就這麼容易！ 您現在可以測試頁面新增電影，但留下其中一個或多個欄位。 當您這樣做時，您會看到顯示下列錯誤：

![新增電影的畫面，顯示驗證錯誤訊息](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>設定樣式的驗證錯誤訊息

您可以看到錯誤訊息，但是它們不突顯很好。 沒有簡單的方法，以設定樣式的錯誤訊息，不過。

若要設定個別的錯誤訊息所顯示的樣式`Html.ValidationMessage`，建立名為的 CSS 樣式類別`field-validation-error`。 若要定義驗證摘要尋找，建立名為的 CSS 樣式類別`validation-summary-errors`。

若要查看這項技術的運作方式，加入`<style>`元素內`<head>`頁面區段。 然後定義名為的樣式類別`field-validation-error`和`validation-summary-errors`，其中包含下列規則：

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

通常您可能會放置到不同的樣式資訊 *.css*檔案，但為求簡化您可以將它們放在頁面現在。 (稍後在此教學課程的集中，您將會移動 CSS 規則到個別 *.css*檔案。)

如果沒有驗證錯誤時，`Html.ValidationMessage`方法呈現`<span>`包含的項目`class="field-validation-error"`。 藉由加入該類別的樣式定義，您可以設定訊息的外觀。 如果發生錯誤，`ValidationSummary`方法同樣的動態呈現屬性`class="validation-summary-errors"`。

再次執行頁面，刻意省略幾個欄位。 錯誤現在是更值得注意。 （事實上，它們做得太過火，但這只是為了顯示您可以執行）。

![新增電影的畫面，顯示驗證錯誤已設定樣式](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>將連結加入至 [影片] 頁面

最後一個步驟是方便您取得*AddMovie*頁面從原始的電影清單。

開啟*電影*頁面上一次。 關閉後`</div>`標記後面`WebGrid`helper，加入下列標記：

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

如同之前，ASP.NET 會解譯`~`運算子作為網站的根目錄。 您不必使用`~`運算子; 您可以使用標記`<a href="./AddMovie">Add a movie</a>`或其他方式來定義 HTML 的了解的路徑。 但是`~`運算子是良好的一般方法建立 Razor 頁面的連結時因為這會讓網站更有彈性 — 如果您將目前的頁面移至子資料夾時，連結仍將會移至*AddMovie*頁面。 (請記住，`~`運算子僅適用於 *.cshtml*頁面。 ASP.NET 了解，但它不是標準 HTML）。

當您完成時，執行*電影*頁面。 它看起來像此頁面：

![新增電影的頁面連結的影片頁面](entering-data/_static/image7.png)

按一下**新增電影**連結，以確定它會移至*AddMovie*頁面。

## <a name="coming-up-next"></a>接下來

在下一個教學課程中，您將學習如何讓使用者編輯已在資料庫中的資料。

## <a name="complete-listing-for-addmovie-page"></a>AddMovie 頁面的完整清單

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO 陳述式](http://www.w3schools.com/sql/sql_insert.asp)W3Schools 站台上
- [驗證使用者輸入，在 ASP.NET Web Pages 站台](https://go.microsoft.com/fwlink/?LinkId=253002)。 使用的詳細資訊`Validation`協助程式。

> [!div class="step-by-step"]
> [上一頁](form-basics.md)
> [下一頁](updating-data.md)
