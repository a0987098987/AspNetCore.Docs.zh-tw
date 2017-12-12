---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: "簡介 ASP.NET Web 網頁的 HTML 表單的基本概念 |Microsoft 文件"
author: tfitzmac
description: "本教學課程會示範如何建立一個輸入的表單，以及如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。 現在您..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 97e4a2a1794dbdccf80f0b44c1246c743fa23019
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>簡介 ASP.NET Web 網頁的 HTML 表單的基本概念
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何建立一個輸入的表單，以及如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。 以及，既然了資料庫，請您將使用表單技術，讓使用者在資料庫中找到特定的影片。 它會假設您已完成透過數列[簡介來顯示資料使用 ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)。
> 
> 您將學習：
> 
> - 如何建立使用標準 HTML 項目表單。
> - 如何讀取使用者的輸入表單中。
> - 提供如何建立 SQL 查詢，選擇性地取得資料所使用的搜尋詞彙的使用者。
> - 如何在頁面中 [記住] 使用者的輸入欄位。
>   
> 
> 功能/技術討論：
> 
> - `Request` 物件。
> - SQL`Where`子句。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，您建立資料庫、 加入資料，並接著使用`WebGrid`helper 來顯示資料。 在本教學課程中，您將新增搜尋方塊，可讓您尋找特定類型的電影，或其標題包含您輸入任何文字。 （例如，您可以尋找內容類型為 「 動作 」 或其標題包含"Harry"Adventure"。 所有的影片）

當您完成本教學課程時，則必須與下列類似的頁面：

![影片內容類型和標題的搜尋頁面](form-basics/_static/image1.png)

頁面的清單部分是與上一個教學課程中的相同&mdash;方格。 差異將會確認此方格會顯示只有電影您搜尋的。

## <a name="about-html-forms"></a>關於 HTML 表單

(如果您有建立 HTML 表單與之間差異的經驗`GET`和`POST`，您可以略過這一節。)

表單具有使用者輸入項目的&mdash;文字方塊、 按鈕、 選項按鈕、 核取方塊、 下拉式清單等等。 使用者填入這些控制項或進行選擇，然後按一下按鈕來送出表單。

此範例所說明的表單的基本 HTML 語法：

[!code-html[Main](form-basics/samples/sample1.html)]

在頁面中，執行此標記，它會建立簡單的表單看起來像下圖：

![基本的 HTML 表單，做為呈現在瀏覽器](form-basics/_static/image2.png)

`<form>`元素會括住提交的 HTML 項目。 (進行簡單的錯誤是將項目加入至頁面，但忘記放在`<form>`項目。 在此情況下，執行任何動作提交。）`method`屬性會告知瀏覽器如何提交使用者的輸入。 將此設`post`如果您將執行的更新，在伺服器上，或者`get`如果您只從伺服器擷取資料。

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET、 POST 和 HTTP 動詞命令安全性**
> 
> HTTP，瀏覽器和伺服器用來交換資訊的通訊協定是非常簡單，其基本作業。 瀏覽器會使用只有少數的動詞命令，對伺服器提出要求。 當您撰寫的 web 的程式碼時，最好先了解這些動詞命令，以及它們如何使用瀏覽器和伺服器。 毫無疑問，最常使用的動詞命令如下：
> 
> - `GET`. 瀏覽器會使用此動詞命令，從伺服器擷取的項目。 例如，當您將 URL 輸入瀏覽器，瀏覽器執行`GET`作業要求您要的頁面。 如果此頁面包含圖形、 瀏覽器就會執行其他`GET`取得映像的作業。 如果`GET`作業已將資訊傳遞至伺服器，將資訊傳遞做為查詢字串中的 URL 的一部分。
> - `POST`. 瀏覽器傳送`POST`要求才能送出要新增或變更伺服器上的資料。 例如，`POST`動詞命令用來在資料庫中建立記錄，或變更現有的。 大部分的情況下，當您填寫表單，然後按一下 [提交] 按鈕，瀏覽器執行`POST`作業。 在`POST`作業，正在傳遞至伺服器的資料是在頁面主體。
> 
> 這些動詞命令中重要的區別在於`GET`作業不會變更伺服器上的任何項目，或將其置於稍微更抽象的方式，`GET`作業並不會導致在伺服器上的狀態變更。 您可以執行`GET`依需求多次，也不會變更這些資源相同的資源上的作業。 (A`GET`作業通常稱為 「 安全 」，或使用技術的詞彙，是*等冪*。)相較之下，當然`POST`要求變更在伺服器上的項目每次執行作業。
> 
> 兩個範例將說明這種區別。 當您執行搜尋時使用例如 Bing 或 Google、 引擎您填寫表單包含一個文字方塊中，，然後按一下 [搜尋] 按鈕。 瀏覽器執行`GET`作業，並具有您做為 URL 的一部分傳遞的方塊中輸入的值。 使用`GET`作業因為搜尋作業不會變更伺服器上的任何資源，為沒問題，這種類型的表單，它只會擷取資訊。
> 
> 現在請試想排序東西的程序。 您填寫訂單詳細資料，然後按一下 [提交] 按鈕。 這項作業將會`POST`要求，因為作業會導致在伺服器上，例如新的訂單記錄、 變更您的帳戶資訊，以及可能是其他多項變更的變更。 不同於`GET`不能重複作業，您`POST`要求 — 如果您這樣做，每次您重新提交要求，您會產生新的順序，在伺服器上。 （在這種情況下，網站通常會警告您不必一次以上，按一下 [提交] 按鈕或將停用 [提交] 按鈕，使您不小心重新提交表單）。
> 
> 在此教學課程中，您將使用兩者`GET`作業和`POST`作業來處理 HTML 表單。 我們將說明在每個案例為何您使用的動詞命令是適當的一個。
> 
> (若要深入了解 HTTP 動詞命令，請參閱[方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的發行項。)


大部分的使用者輸入項目會以 HTML`<input>`項目。 它們看起來像是`<input type="type" name="name">,`其中*類型*表示您想要的使用者輸入控制項的類型。 這些項目是常見的：

- 文字方塊中：`<input type="text">`
- 核取方塊：`<input type="check">`
- 選項按鈕：`<input type="radio">`
- 按鈕：`<input type="button">`
- 送出按鈕：`<input type="submit">`

您也可以使用`<textarea>`項目來建立多行文字方塊和`<select>`建立下拉式清單或可捲動的清單項目。 (如需有關 HTML 表單項目，請參閱[HTML 表單和輸入](http://www.w3schools.com/html/html_forms.asp)W3Schools 站台上。)

`name`屬性是很重要，因為名稱就是將取得的項目更新版本中，值的方式不久，您會看到。

有趣的部分是網頁開發人員，您將使用者輸入具有執行的動作。 沒有任何與這些項目相關聯的內建行為。 相反地，您必須取得使用者輸入或選取的值，並運用它們。 這是您將在本教學課程中學習。

> [!TIP] 
> 
> **HTML5 和輸入的表單**
> 
> 您可能知道，HTML 是轉換中，並最新版本 (HTML5) 包含支援更直覺的方式，讓使用者輸入資訊。 比方說，html5 格式，您 （網頁開發人員） 可以通知網頁您想讓使用者輸入的日期。 行事曆，而不是需要使用者手動輸入日期，然後可以自動顯示瀏覽器。 不過，HTML5 新，而且尚不支援在所有瀏覽器。
> 
> ASP.NET Web Pages 支援 HTML5 輸入使用者的瀏覽器未確認。 以新屬性的了解`<input>`元素，html5 格式，請參閱[HTML&lt;輸入&gt;輸入屬性](http://www.w3schools.com/html/html_form_input_types.asp)W3Schools 站台上。


## <a name="creating-the-form"></a>建立表單

在 WebMatrix 中，在**檔案**工作區中，開啟*Movies.cshtml*頁面。

關閉後`</h1>`標記和開頭之前`<div>`標記`grid.GetHtml`呼叫中，加入下列標記：

[!code-html[Main](form-basics/samples/sample2.html)]

這個標記會建立名為文字方塊的表單`searchGenre`及提交按鈕。 文字並提交 按鈕會括住`<form>`項目其`method`屬性設為`get`。 (請記住，如果沒有將放在文字方塊中，然後送出按鈕內`<form>`項目，當您按一下按鈕會提交做任何動作。)您使用`GET`動詞這裡因為您要建立表單，不會進行任何變更在伺服器上，則只會導致搜尋。 (在上一個教學課程中，您已經使用`post`方法，這是如何您將變更提交至伺服器。 您會看到下一個教學課程中一次。）

執行網頁。 您尚未定義任何形式的行為，雖然您可以看到它的外觀：

![「 內容類型的 [搜尋] 方塊的影片頁面](form-basics/_static/image3.png)

輸入的值，在文字方塊中，像是 「 喜劇。 」 然後按一下 **搜尋內容類型**。

記下的網頁 URL。 因為您將設定`<form>`項目的`method`屬性`get`，您輸入的值現在是在 URL 中，像這樣的查詢字串的一部分：

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>讀取表單值

此頁面已包含一些程式碼，取得資料庫資料，並將結果顯示在方格中。 現在，您必須加入讀取文字方塊的值，因此您可以執行 SQL 查詢，以包含搜尋詞彙的一些程式碼。

因為您設定表單的方法為`get`，您可以閱讀使用類似下列程式碼到文字方塊中輸入的值：

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString`物件 (`QueryString`屬性`Request`物件) 包含已送出的項目值`GET`作業。 `Request.QueryString`屬性包含*集合*（清單） 會提交表單中的值。 若要取得任何個別值，您可以指定您想要的項目名稱。 這就是為什麼您必須擁有`name`屬性`<input>`元素 (`searchTerm`) 會建立在文字方塊。 (如需詳細資訊`Request`物件，請參閱[資訊看板](#BKMK_TheRequestObject)更新版本。)

它是簡單讀取文字方塊的值。 但是，如果使用者未輸入任何項目完全在文字方塊中，但已按下**搜尋**，忽略按一下的因為沒有要搜尋項目。

下列程式碼是範例，示範如何實作這些條件。 （您不需要尚未加入此程式碼，您將會立即執行的）。

[!code-csharp[Main](form-basics/samples/sample3.cs)]

測試細分以這種方式：

- 取得值的`Request.QueryString["searchGenre"]`，也就是輸入的值`<input>`名為項目`searchGenre`。
- 找出是否為空白使用`IsEmpty`方法。 這個方法是以判斷項目 （例如，表單項目） 是否包含值的標準方式。 但是，您不在乎只有當*不*空的因此...
- 新增`!`運算子的前面`IsEmpty`測試。 (`!`運算子代表邏輯 NOT)。

一般的語言，將整個`if`條件會轉譯成下列：*如果表單的 searchGenre 項目不是空的然後...*

此區塊會設定為建立使用搜尋詞彙的查詢。 您要進行的下一節。

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **要求物件**
> 
> `Request`物件包含瀏覽器時，會傳送至您的應用程式頁面要求或送出的所有資訊。 此物件包含使用者提供，例如文字方塊值或要上傳檔案的任何資訊。 它也包含各式各樣的其他資訊，例如 cookie 值中的 URL 查詢字串 （如果有的話），頁面是否正在執行，型別瀏覽器的使用者所使用的瀏覽器中設定的語言清單的檔案路徑以及其他更多功能。
> 
> `Request`物件是*集合*（清單） 的值。 您藉由指定其名稱取得集合的個別值：
> 
> `var someValue = Request["name"];`
> 
> `Request`物件實際上會公開數個子集。 例如: 
> 
> - `Request.Form`可讓您從項目內已提交的值`<form>`項目，如果要求是`POST`要求。
> - `Request.QueryString`URL 查詢字串中，提供您剛才的值。 (例如 URL 中`http://mysite/myapp/page?searchGenre=action&page=2`、 `?searchGenre=action&page=2` URL 區段是查詢字串。)
> - `Request.Cookies`集合可讓您存取瀏覽器已傳送的 cookie。
> 
> 若要取得您知道值是在送出表單，您可以使用`Request["name"]`。 或者，您可以使用更特定的版本`Request.Form["name"]`(如`POST`要求) 或`Request.QueryString["name"]`(如`GET`要求)。 當然，*名稱*是要取得之項目的名稱。
> 
> 您想要取得的項目名稱必須是唯一您正在使用在集合中。 這就是為什麼`Request`物件所提供的子集喜歡`Request.Form`和`Request.QueryString`。 假設您的頁面包含名為表單項目`userName`和*也*包含名為 cookie `userName`。 如果您收到`Request["userName"]`，模稜兩可是否要讓表單值或 cookie。 不過，如果您收到`Request.Form["userName"]`或`Request.Cookie["userName"]`，您要明確了解要取得的值。
> 
> 它是最好的作法是特定，且使用的子集`Request`，您感興趣，例如`Request.Form`或`Request.QueryString`。 在本教學課程，您要建立簡單的頁面，它可能不會真的會有任何影響。 不過，當您建立更複雜的頁面，使用明確的版本`Request.Form`或`Request.QueryString`可協助您避免當頁面包含表單 （或多個表單），可以發生的問題，cookie、 查詢字串值等等。


## <a name="creating-a-query-by-using-a-search-term"></a>建立查詢所使用的搜尋詞彙

您現在知道如何取得使用者輸入搜尋詞彙，您可以建立使用它的查詢。 請記住，若要取得移出資料庫的所有電影項目，您正在使用 SQL 查詢，看起來像此陳述式：

`SELECT * FROM Movies`

若要取得特定的電影，您必須使用包含查詢`Where`子句。 這個子句可讓您設定的條件的查詢所傳回資料列。 以下為範例：

`SELECT * FROM Movies WHERE Genre = 'Action'`

基本格式是`WHERE column = value`。 您可以使用不同的運算子，只除了`=`、 like `>` （大於）、 `<` （小於）、 `<>` （不等於）、 `<=` （小於或等於）、 等等，根據您要尋找的。

如果您想知道，SQL 陳述式不區分大小寫&mdash;`SELECT`相同`Select`(或甚至`select`)。 不過，人們通常大寫 SQL 陳述式中的關鍵字 like`SELECT`和`WHERE`，讓您更輕鬆地閱讀。

### <a name="passing-the-search-term-as-a-parameter"></a>傳遞做為參數的搜尋詞彙

搜尋特定的內容類型也很簡單 (`WHERE Genre = 'Action'`)，但您想要能夠搜尋任何使用者輸入的內容類型。 若要這樣做，請您建立為包含要搜尋的預留位置值的 SQL 查詢。 它看起來像下列命令：

`SELECT * FROM Movies WHERE Genre = @0`

預留位置是`@`字元後面接著零。 您可能會猜測，查詢可以包含多個 「 預留位置 」，以及會命名為`@0`， `@1`，`@2`等等。

若要設定查詢和實際傳遞的值，您可以使用如下的程式碼：

[!code-sql[Main](form-basics/samples/sample4.sql)]

此程式碼很類似什麼您已完成在方格中顯示資料。 唯一的差異如下：

- 查詢包含預留位置 (`WHERE Genre = @0"`)。
- 查詢會放入變數 (`selectCommand`); 之前，您查詢直接傳遞至`db.Query`方法。
- 當您呼叫`db.Query`方法，您將傳遞查詢和要使用的預留位置的值。 (如果查詢在多個預留位置，您會將其傳遞做為個別值的方法。)

如果您同時將所有這些項目，您會得到下列程式碼：

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **重要 ！** 使用預留位置 (例如`@0`) 將值傳遞至 SQL 命令為*極為重要*安全性。 您看到該元件，和預留位置的變數，方法是您應該建構 SQL 命令的唯一方式。
> 
> 永遠不會以建構 SQL 陳述式放在一起，（串連） 的常值文字和您從使用者取得的值。 串連使用者輸入 SQL 陳述式會開啟您的站台*SQL 資料隱碼攻擊*當惡意使用者送出至您的頁面 hack 資料庫的值。 (您可以讀取多個發行項中[SQL 資料隱碼](https://msdn.microsoft.com/en-us/library/ms161953.aspx)MSDN 網站。)


## <a name="updating-the-movies-page-with-search-code"></a>搜尋程式碼以更新 [影片] 頁面

現在您可以更新中的程式碼*Movies.cshtml*檔案。 若要開始，請在頁面頂端的程式碼區塊中的程式碼取代這段程式碼：

[!code-csharp[Main](form-basics/samples/sample6.cs)]

此處的差異在於您已將放入查詢`selectCommand`變數，您會將它傳遞給`db.Query`更新版本。 將放入變數的 SQL 陳述式，可讓您變更陳述式，也就是您將會執行執行搜尋。

您也已移除這兩行，您將會放回更新版本：

[!code-csharp[Main](form-basics/samples/sample7.cs)]

您不想要尚未執行查詢 (也就是呼叫`db.Query`) 而您不想要初始化`WebGrid`協助程式，但其中。 說已經找出哪些 SQL 陳述式都必須執行之後，您要進行這些工作。

之後重寫區塊中，您可以加入新的邏輯來處理的搜尋。 已完成的程式碼看起來如下所示。 更新您的頁面中的程式碼，使其符合此範例中：

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

頁面現在的運作方式如下。 每次執行網頁時，程式碼會開啟該資料庫和`selectCommand`變數會設為 SQL 陳述式可取得所有記錄`Movies`資料表。 程式碼也會初始化`searchTerm`變數。

不過，如果目前的要求中包含的值`searchGenre`項目、 程式碼集`selectCommand`至不同的查詢，也就是要包含`Where`子句以搜尋內容類型。 它也會設定`searchTerm`任何已傳遞的 [搜尋] 方塊 （這可能是 nothing）。

不論哪一個 SQL 陳述式是在`selectCommand`，程式碼接著會呼叫`db.Query`為執行查詢時，將其傳遞任何處於`searchTerm`。 如果沒有任何`searchTerm`，它並不重要，因為在此情況下沒有任何參數將值傳遞至`selectCommand`嗎。

最後，程式碼會初始化`WebGrid`使用查詢結果，就像之前的協助程式。

您可以看到將放入 SQL 陳述式，並搜尋詞彙到變數中，您已加入彈性的程式碼。 您會發現稍後在本教學課程，您可以使用這個基本架構，並保留加入不同類型的搜尋邏輯。

## <a name="testing-the-search-by-genre-feature"></a>測試的內容類型的搜尋功能

在 WebMatrix 中，執行*Movies.cshtml*頁面。 您看到的頁面類型的文字方塊。

輸入，您已為您的測試記錄的其中一個輸入，然後按一下 內容類型**搜尋**。 此時您會看到剛才電影符合該類型的清單：

![列出搜尋內容類型 'Comedies' 之後影片頁面](form-basics/_static/image4.png)

輸入不同的內容類型，然後再搜尋一次。 請嘗試使用所有小寫或全部大寫的字母，讓您能夠查看搜尋不區分大小寫輸入的內容類型。

## <a name="remembering-what-the-user-entered"></a>「 記住 」 使用者的輸入

您可能已注意到，之後您輸入的內容類型，然後再按下**搜尋內容類型**，您會看到該內容類型的清單。 不過，[搜尋] 方塊是空的&mdash;換句話說，它不記得您已輸入。

請務必了解為什麼會發生此問題。 當您提交頁面時，瀏覽器會傳送要求至 web 伺服器。 當 ASP.NET 要求時，建立網頁的全新執行個體、 執行的程式碼，以及接著再次轉譯至瀏覽器頁面。 作用中，不過，頁面並不知道，您只使用本身的先前版本。 它知道它取得具有一些要求所有表單中的資料。

每次要求頁面&mdash;第一次還是送出&mdash;您收到新的頁面。 網頁伺服器有沒有您的最後一個要求的記憶體。 都不會 ASP.NET 中，而且都不會在瀏覽器。 頁面的這些個別執行個體之間的唯一連接是您在它們之間傳送的任何資料。 比方說，如果送出頁面之後，新的頁面執行個體可以取得舊版的執行個體所送出表單資料。 （頁面之間傳遞資料的另一種方式是使用 cookie）。

型式的方式來描述這種情況是說出網頁是*無狀態*。 Web 伺服器本身的頁面和頁面中的項目不會維護任何先前的頁面狀態的相關資訊。 網站的設計是這種方式，因為維護個別要求的狀態會快速耗盡資源的網頁伺服器，通常可能處理上千，甚至數百個，每秒要求數。

因此，這就是為什麼文字方塊是空的。 送出頁面之後，ASP.NET 會建立頁面的新執行個體，並執行透過程式碼和標記。 沒有任何會告知 ASP.NET，若要將值置入文字方塊中之程式碼中。 因此 ASP.NET 並不執行，並沒有在它的值已轉譯的文字方塊。

沒有實際上可以輕鬆地取得解決此問題。 您在文字方塊中輸入的內容類型*是*為您提供的程式碼&mdash;處於`Request.QueryString["searchGenre"]`。

更新在文字方塊中的標記以便`value`屬性取得其值從`searchTerm`，類似這個範例：

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

在此頁面上，您可能有也設定`value`屬性`searchTerm`您輸入變數，因為該變數也包含內容類型。 但使用`Request`物件來設定`value`屬性所示如下的標準方式完成這項工作。 (假設要執行這個動作&mdash;在某些情況下，您可能想要呈現頁面*沒有*欄位中的值。 這所有取決於您的應用程式使用狀況。）

> [!NOTE]
> 您無法 [記住] 文字方塊中所使用的密碼值。 將安全性漏洞，來允許人員填寫密碼欄位，使用程式碼。


再次執行頁面，輸入類型，然後按一下**搜尋內容類型**。 這次不只執行您看到搜尋的結果，但文字方塊會記住您輸入上一次：

![顯示在文字方塊中有 '記住' 先前的項目頁面](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>搜尋標題中的任何文字

您現在可以搜尋的任何內容類型，但您也可以搜尋的標題。 很難取得標題正確，當您搜尋，而您可搜尋的任何位置出現在標題文字。 若要這樣做，在 SQL 中，您使用`LIKE`運算子和語法，例如下列：

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

此命令會取得其標題包含"adventure"的所有電影。 當您使用`LIKE`運算子，包括萬用字元`%`做為搜尋詞彙的一部分。 搜尋`LIKE 'adventure%'`表示 「 開頭 'adventure' 」。 （技術上來說，表示 「 字串 'adventure' 後面的任何項目。 」）同樣地，搜尋詞彙`LIKE '%adventure'`表示 「 任何項目後面的字串 'adventure'"，這是另一種方式說 「 結束與 'adventure' 」。

搜尋詞彙`LIKE '%adventure%'`因此表示 「 具"adventure' 標題中任何位置。" （技術上來說，「 任何項目在標題中，後面接著 'adventure'，後面接著任何項目。 」）

內部`<form>`項目，加入下列標記結尾下的`</div>`類型搜尋的標記 (的結束`</form>`項目):

[!code-html[Main](form-basics/samples/sample10.html)]

處理此搜尋的程式碼是類似的程式碼類型的搜尋，只不過您必須組合`LIKE`搜尋。 在頁面頂端的程式碼區塊內加入下列`if`後方封鎖`if`類型搜尋的區塊：

[!code-csharp[Main](form-basics/samples/sample11.cs)]

此程式碼使用您稍早所見的相同邏輯，不同之處在於會使用搜尋`LIKE`運算子和程式碼將 「`%`"之前和之後的搜尋字詞。

請注意如何很容易就能加入網頁中的另一個搜尋。 您必須是：

- 建立`if`測試，以查看相關的搜尋方塊是否具有值的區塊。
- 設定`selectCommand`變數設為新的 SQL 陳述式。
- 設定`searchTerm`變數設為要傳遞給查詢的值。

以下是完整的程式碼區塊，其中包含項目的搜尋的新邏輯：

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

此程式碼的用途的摘要如下：

- 變數`searchTerm`和`selectCommand`頂端會初始化。 您要將這些變數設定為適當的搜尋詞彙 （如果有的話），使用者沒有在頁面根據適當的 SQL 命令。 預設搜尋是簡單的情況下，從資料庫取得所有影片。
- 中的測試`searchGenre`和`searchTitle`，程式碼集`searchTerm`您想要搜尋的值。 這些程式碼區塊也設定`selectCommand`到該項搜尋適當的 SQL 命令。
- `db.Query`方法叫用一次，使用任何 SQL 命令`selectedCommand`和任何值存在於`searchTerm`。 如果沒有任何搜尋詞彙 （沒有內容類型和任何標題 word） 的值`searchTerm`為空字串。 不過，，並不重要，因為查詢不在此情況下需要參數。

## <a name="testing-the-title-search-feature"></a>測試的標題搜尋功能

現在您可以測試您已完成的搜尋頁面。 執行*Movies.cshtml*。

輸入的內容類型，然後按一下**搜尋內容類型**。 方格會顯示類似之前電影的該內容類型。

輸入標題文字，然後按一下**搜尋標題**。 方格會顯示在標題中有該單字的電影。

![列出搜尋之後 '' 標題中的影片頁面](form-basics/_static/image6.png)

將這兩個文字方塊保留空白，然後按一下其中一個按鈕。 方格會顯示所有影片。

## <a name="combining-the-queries"></a>結合查詢

您可能會注意到，您可以執行的搜尋會獨佔。 您無法搜尋標題與內容類型在相同的時間，即使這兩個搜尋方塊中有值。 例如，您無法搜尋其標題包含"Adventure"的所有動作影片。 （如頁面自動程式碼現在，如果您輸入值的內容類型和標題，標題搜尋取得優先順序。）若要建立結合條件的搜尋，您就必須建立 SQL 查詢的語法如下所示：

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

而且您必須使用類似下面的陳述式來執行查詢 （大致來說）：

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

建立邏輯，可讓許多的搜尋準則的排列方式可讓有點複雜，您可以看到。 因此，我們將在此停止。

## <a name="coming-up-next"></a>接下來

在下一個教學課程中，您將建立使用表單來讓使用者資料庫中新增電影的頁面。

## <a name="complete-listing-for-movie-page-updated-with-search"></a>電影頁面 （使用搜尋更新） 的完整清單

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL WHERE 子句](http://www.w3schools.com/sql/sql_where.asp)W3Schools 站台上
- [方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的發行項

>[!div class="step-by-step"]
[上一頁](displaying-data.md)
[下一頁](entering-data.md)
