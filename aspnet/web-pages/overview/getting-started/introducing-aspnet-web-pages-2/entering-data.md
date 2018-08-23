---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 簡介 ASP.NET Web Pages-使用表單輸入資料庫的資料 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何建立項目表單，然後輸入 得到資料從表單到資料庫資料表時使用 ASP.NET Web Pages （...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 453ed888bc219175c5a3a25f857dfa79ab22b608
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832247"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>簡介 ASP.NET Web Pages-使用表單輸入資料庫的資料
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何建立項目表單，然後輸入您取得從表單到資料庫資料表時使用 ASP.NET Web Pages (Razor) 的資料。 它假設您已完成透過數列[基本概念的 HTML 表單 ASP.NET Web Pages 中](https://go.microsoft.com/fwlink/?LinkId=251581)。
> 
> 您將學到什麼：
> 
> - 深入了解如何處理項目表單。
> - 如何在資料庫中加入資料 （插入）。
> - 如何確定使用者有 （如何驗證使用者輸入） 表單中輸入必要的值。
> - 如何顯示驗證錯誤。
> - 如何從目前的頁面移至另一個頁面。
>   
> 
> 功能/技術討論：
> 
> - `Database.Execute` 方法
> - SQL`Insert Into`陳述式
> - `Validation`協助程式。
> - `Response.Redirect` 方法


## <a name="what-youll-build"></a>您將建置

教學課程中稍早的說明了如何建立資料庫，您必須輸入資料庫的資料藉由編輯資料庫直接在 WebMatrix 中，使用**資料庫**工作區。 在大部分的應用程式，這不是實用的方法，將資料放入資料庫，不過。 因此在本教學課程中，您將建立的 web 型介面，可讓您或輸入資料，並將它儲存到資料庫的任何人。

您將建立的頁面，您可以在此輸入新的電影。 頁面會包含具有的欄位 （文字方塊），您可以在其中輸入電影標題、 內容類型和年度的項目表單。 網頁看起來像此頁面：

![在瀏覽器中的 [新增影片] 頁面](entering-data/_static/image1.png)

文字方塊會是 HTML`<input>`項目看起來會像此標記：

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>建立基本的項目表單

建立名為的頁面*AddMovie.cshtml*。

取代項目是以下列標記檔案中。 覆寫所有項目;您很快就將新增程式碼區塊頂端。

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

這個範例會示範一般的 HTML 來建立表單。 它會使用`<input>`文字方塊和提交按鈕的項目。 文字方塊的標題藉由使用標準`<label>`項目。 `<fieldset>`和`<legend>`項目周圍加上好方塊形式。

請注意，在此頁面上，`<form>`項目會使用`post`的值為`method`屬性。 在上一個教學課程中，您已建立的表單，使用`get`方法。 因為正確的雖然表單送出至伺服器的值，但要求並未進行任何變更。 一樣是提取資料以不同的方式。 不過，在此頁面*將*進行變更，您要新增新的資料庫記錄。 因此，應該使用這種形式`post`方法。 (如需詳細資訊之間的差異`GET`並`POST`作業，請參閱[GET、 POST 和 HTTP 動詞命令安全性](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety)先前的教學課程中的資訊看板。)

請注意，每個文字方塊已`name`項目 (`title`， `genre`， `year`)。 如您所見先前的教學課程中，這些名稱很重要，因為您必須擁有這些名稱，因此您可在稍後取得使用者的輸入。 您可以使用任何名稱。 最好使用有意義的名稱，幫助您記住您正在使用哪些資料。

`value`屬性的每個`<input>`項目包含的 Razor 程式碼 (例如`Request.Form["title"]`)。 在先前的教學課程，以保留 （如果有的話），在文字方塊中輸入的值中，在提交表單之後，您了解這個技巧的版本。

## <a name="getting-the-form-values"></a>取得表單值

接下來，您會加入處理表單的程式碼。 在 [大綱] 中，您會執行下列作業：

1. 檢查網頁是否正在回傳 （已提交）。 您想要您的程式碼，只有在使用者按下按鈕，只有在頁面上第一次執行時，不時，才執行。
2. 取得使用者輸入到文字方塊中的值。 在此情況下，因為正在使用的表單`POST`動詞命令，取得從表單值`Request.Form`集合。
3. 為新的記錄中插入值*電影*資料庫資料表。

在檔案頂端，新增下列程式碼：

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

前幾行建立的變數 (`title`， `genre`，和`year`) 來保存從文字方塊中的值。 線條`if(IsPost)`可確保所設定的變數*只*當使用者按一下**新增影片**按鈕 — 也就是當表單張貼。

如您所見在先前的教學課程中，您會取得文字方塊的值使用如下的運算式`Request.Form["name"]`，其中*名稱*名稱`<input>`項目。

變數名稱 (`title`， `genre`，和`year`) 是任意的。 喜歡名稱，您指派給`<input>`項目，您要的任何項目呼叫。 (變數的名稱不必符合的 name 屬性`<input>`表單上的項目。)但如同`<input>`項目，它是個不錯的主意，使用變數的名稱會反映其所包含的資料。 當您撰寫程式碼時，則 一致的名稱讓您更輕鬆地記住您正在使用哪些資料。

## <a name="adding-data-to-the-database"></a>將資料加入至資料庫

在程式碼中封鎖您剛加入，您只要*內*右大括號 ( `}` ) 的`if`封鎖 （不只是程式碼區塊中） 內，新增下列程式碼：

[!code-csharp[Main](entering-data/samples/sample3.cs)]

這個範例是類似於您用來擷取及顯示資料的先前教學課程中的程式碼。 開頭的那一行`db =`開啟像以前，資料庫和下一行會定義 SQL 陳述式，一次為你之前看到。 不過，此時它會定義 SQL`Insert Into`陳述式。 下列範例示範的一般語法`Insert Into`陳述式：

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

換句話說，指定要插入，資料表則列出的資料行中插入，以及則列出要插入的值。 （如先前所述，SQL 是不區分大小寫，但有些人充分利用的關鍵字，以便於讀取命令）。

您要插入到資料行已在命令列- `(Title, Genre, Year)`。 重點在於如何從文字方塊中，到取得這些值`VALUES`命令的一部分。 而不是實際的值，您會看到`@0`， `@1`，和`@2`，這當然是預留位置。 當您執行命令 (在`db.Execute`列)，您傳遞的值，所得的文字方塊。

**重要！** 請記住，您應該包含線上使用者輸入的 SQL 陳述式中的資料的唯一方式是使用預留位置，如下所示 (`VALUES(@0, @1, @2)`)。 如果您串連使用者輸入 SQL 陳述式時，您自行開啟 SQL 插入式攻擊中, 所述[表單的基本 ASP.NET Web Pages 中](https://go.microsoft.com/fwlink/?LinkId=251581)（上一個教學課程）。

內部仍`if`區塊中，新增下列行之後`db.Execute`列：

[!code-css[Main](entering-data/samples/sample4.css)]

新的影片已插入至資料庫之後，這行就會將您 （重新導向） 跳至*電影*頁面上，您可以看到您剛輸入的影片。 `~`運算子表示 「 根網站的 」。 (`~`運算子只適用於不在 HTML 中的 ASP.NET 網頁通常。)

完整的程式碼區塊是由本範例所示：

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>測試插入命令 （目前）

您還沒，結束，但現在是要測試的好時機。

在 [樹狀] 檢視的檔案在 WebMatrix 中，以滑鼠右鍵按一下*AddMovie.cshtml*頁面，然後按一下**在瀏覽器中啟動**。

![在瀏覽器中的 [新增影片] 頁面](entering-data/_static/image2.png)

(如果您得到不同的網頁瀏覽器中，請確定 URL 是`http://localhost:nnnnn/AddMovie`)，其中*nnnnn*是您使用的連接埠號碼。)

您收到的錯誤網頁嗎？ 如果是的話，請仔細閱讀，並確定該程式碼看起來完全功能已在稍早列出。

在表單中輸入電影&mdash;比方說，使用"公民 Kane"、"戲劇"和"1941 」。 （或者任何）。然後按一下**新增影片**。

如果一切順利，您會被重新導向至*電影*頁面。 請確定其中已列出新電影。

![顯示新的影片頁面新增影片](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>驗證使用者輸入

請返回*AddMovie*頁面上，或執行一次。 輸入另一部影片，但這次，然後輸入只有標題&mdash;比方說，請輸入"登 ' 在 Rain"。 然後按一下**新增影片**。

若要將您重新導向*電影*頁面上一次。 您可以找到新的影片，但不完整。

![影片頁面上顯示遺失某些值的新影片](entering-data/_static/image4.png)

您在建立時*電影*資料表中，您明確地說，沒有的欄位可能是 null。 這裡的項目表單，對於新的影片，而且您要離開讓欄位空白。 這是錯誤。

在此情況下，實際上並未引發到資料庫 (或*擲回*) 錯誤。 您沒有因此提供的內容類型或年中的程式碼*AddMovie*頁面來處理這些值為所謂*空字串*。 當 SQL`Insert Into`命令執行、 內容類型和年度的欄位沒有實用的資料，但它們不為 null。

很明顯地，您不想讓使用者輸入半空白電影資訊到資料庫。 解決方法是驗證使用者的輸入。 一開始，驗證將會只是確定使用者已輸入的所有欄位的值 （亦即，無一包含空字串）。

> [!TIP]
> 
> **Null 和空字串**
> 
> 在程式設計中，沒有區別不同概念的 「 無值 」。 一般情況下，值是*null*如果從未已設定或以任何方式初始化。 相反地，可以預期字元資料 （字串） 的變數設定為*空字串*。 在此情況下，值不是 null;它只是已明確設定為字串，其長度為零的字元。 這兩個陳述式可顯示的差異：
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> 它稍微比較複雜的容量，但重要的一點是，`null`代表一種未定狀態。
> 
> 現在，然後務必了解完全 null 值時，以及只是空字串時。 在程式碼*AddMovie*頁面上，您取得的值的文字方塊使用`Request.Form["title"]`，依此類推。 當頁面第一次執行 （在您按一下按鈕時） 之前，值`Request.Form["title"]`為 null。 當您提交表單，但是`Request.Form["title"]`取得的值`title`文字方塊。 它並不明顯，但空白的文字方塊不是 null;它只會在它包含空字串。 因此當程式碼執行以回應 [] 按鈕按一下，`Request.Form["title"]`中有一個空字串。
> 
> 這項區別為何如此重要？ 您在建立時*電影*資料表中，您明確地說，沒有的欄位可能是 null。 這裡會有的項目表單，對於新的影片，但您要離開讓欄位空白。 您都應該有抱怨當您嘗試儲存沒有內容類型或年份值的新上映電影的資料庫。 但這就是重點&mdash;即使這些文字方塊保留空白，值不為 null，而空字串。 如此一來，您就可以將這些專欄是空的資料庫來儲存新上映電影&mdash;但不是為 null ！ &mdash; 值。 因此，您必須先確定使用者不會提交空的字串，您可以藉由驗證使用者的輸入。


### <a name="the-validation-helper"></a>驗證協助程式

ASP.NET Web 網頁包含 helper &mdash; `Validation`協助程式&mdash;可用來確定使用者輸入符合您需求的資料。 `Validation`協助程式是其中一個內建至 ASP.NET Web Pages 中，因此您不必使用 NuGet，您在先前的教學課程中安裝 Gravatar 協助程式的方式將它安裝為套件的協助程式。

若要驗證使用者的輸入，您會執行下列作業：

- 您可以使用程式碼，指定您想要在頁面上的文字方塊中的值。
- 使電影資訊加入資料庫，只有當所有項目正確驗證，則您可以放程式碼測試。
- 您可以將程式碼加入標記，以顯示錯誤訊息。

在中的程式碼區塊*AddMovie*頁面上，居然發動的 在變數宣告之前，加入下列程式碼：

[!code-csharp[Main](entering-data/samples/sample7.cs)]

您呼叫`Validation.RequireField`一次，每個欄位 (`<input>`項目) 您想要用來要求項目。 您也可以新增自訂錯誤訊息，每次呼叫，您在這裡看到的一樣。 （我們不同只是為了顯示您可以將任何您喜歡的項目那里訊息）。

如果沒有問題，您會想要防止新電影資訊插入至資料庫。 在 `if(IsPost)`區塊中，使用`&&`以新增另一個測試的條件 (邏輯 AND) `Validation.IsValid()`。 當您完成時，整個`if(IsPost)`區塊看起來像此程式碼：

[!code-csharp[Main](entering-data/samples/sample8.cs)]

如果沒有與任何使用您註冊的欄位驗證錯誤`Validation`協助程式，`Validation.IsValid`方法會傳回 false。 而且在此情況下，無該區塊中的程式碼會執行，因此沒有無效的電影項目會插入至資料庫。 當然您不到重新導向時，*電影*頁面。

完整的程式碼區塊中，包括驗證程式碼現在看起來如下列範例：

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>顯示驗證錯誤

最後一個步驟是要顯示任何錯誤訊息。 您可以顯示個別的訊息，每個驗證錯誤，或您可以顯示摘要，或兩者。 本教學課程中，您會執行同時，讓您可以查看其運作方式。

每個旁邊`<input>`您要驗證的項目，請呼叫`Html.ValidationMessage`方法並傳遞給它的名稱`<input>`您要驗證的項目。 您放置`Html.ValidationMessage`方法正確的地方出現的錯誤訊息。 當頁面執行時，則`Html.ValidationMessage`方法會轉譯`<span>`位置驗證錯誤的項目。 (如果沒有發生錯誤，`<span>`項目會呈現，但在其中沒有任何文字。)

變更頁面中的標記，使其包含`Html.ValidationMessage`方法，每三個`<input>`元素在頁面上，如這個範例所示：

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

若要查看摘要的運作方式，也加入下列標記和程式碼後面`<h1>Add a Movie</h1>`頁面上的項目：

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

根據預設，`Html.ValidationSummary`方法會顯示在清單中的所有驗證訊息 (`<ul>`內的項目`<div>`項目)。 如同`Html.ValidationMessage`方法，則一律會轉譯為驗證摘要標記; 如果沒有任何錯誤，會轉譯任何清單項目。

摘要可以顯示驗證訊息，而不是使用的替代方式`Html.ValidationMessage`方法，以顯示每個欄位特有的錯誤。 或者，您可以使用摘要和詳細資料。 或者您可以使用`Html.ValidationSummary`方法來顯示一般錯誤，然後使用 個別`Html.ValidationMessage`呼叫，以顯示詳細資料。

[完成] 頁面現在看起來如下列範例：

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

就這麼容易！ 您現在可以測試頁面新增影片，但留下其中一個或多個欄位。 當您這樣做時，您會看到顯示下列錯誤：

![新增電影的畫面，顯示驗證錯誤訊息](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>設定驗證錯誤訊息的樣式

您可以看到有錯誤訊息，但它們不真正脫穎而出非常好。 沒有簡單的方法，來設定樣式的錯誤訊息，不過。

若要設定個別的錯誤訊息所顯示的樣式`Html.ValidationMessage`，建立名為 CSS 樣式類別`field-validation-error`。 若要定義驗證摘要的外觀，建立名為 CSS 樣式類別`validation-summary-errors`。

若要查看這項技術的運作方式，新增`<style>`內的項目`<head>`頁面區段。 然後定義樣式的類別，叫做`field-validation-error`和`validation-summary-errors`，其中包含下列規則：

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

通常您可能會放置到不同的樣式資訊 *.css*檔案，但為求簡化您可以將它們放在頁面現在。 (稍後在此教學課程的集中，您會將 CSS 規則移至個別 *.css*檔案。)

如果有驗證錯誤時，`Html.ValidationMessage`方法會轉譯`<span>`項目，包括`class="field-validation-error"`。 藉由新增樣式定義該類別，您可以設定訊息的外觀。 如果發生錯誤，`ValidationSummary`方法同樣的動態呈現屬性`class="validation-summary-errors"`。

再次執行頁面，刻意省略的欄位數。 錯誤現在會更明顯。 （事實上，他們正在做得太過火，但這只是為了顯示您可以執行）。

![新增電影的畫面，顯示已設定樣式的驗證錯誤](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>將連結加入至 Movies 頁面

最後一個步驟是以取得方便*AddMovie*頁面從原始的電影清單。

開啟*電影*頁面上一次。 在結尾後面`</div>`標記後面`WebGrid`協助程式，加入下列標記：

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

如您所見之前，ASP.NET 會解譯`~`運算子來作為網站的根目錄。 您不需要使用`~`運算子; 您可以使用標記`<a href="./AddMovie">Add a movie</a>`或其他方法來定義 HTML 的了解的路徑。 但是`~`運算子是不錯的一般方法時建立 Razor 頁面的連結因為它可讓網站更有彈性 — 如果您將目前的頁面移至子資料夾時，連結仍會移至*AddMovie*頁面。 (請記住`~`運算子僅適用於 *.cshtml*頁面。 ASP.NET 了解，但它不是標準 HTML）。

當您完成時，執行*電影*頁面。 它看起來如下列網頁：

![「 新增影片頁面連結的影片頁面](entering-data/_static/image7.png)

按一下 **加入電影**連結，以確定能前往*AddMovie*頁面。

## <a name="coming-up-next"></a>接下來的下一步

在下一個教學課程中，您將了解如何讓使用者編輯已在資料庫中的資料。

## <a name="complete-listing-for-addmovie-page"></a>AddMovie 頁面的完整清單

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL INSERT INTO 陳述式](http://www.w3schools.com/sql/sql_insert.asp)W3Schools 站台上
- [驗證使用者輸入，在 ASP.NET Web Pages 網站](https://go.microsoft.com/fwlink/?LinkId=253002)。 使用的詳細資訊`Validation`協助程式。

> [!div class="step-by-step"]
> [上一頁](form-basics.md)
> [下一頁](updating-data.md)
