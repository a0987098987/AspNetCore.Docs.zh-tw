---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: 簡介 ASP.NET Web Pages-HTML 表單基本概念 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何建立一個輸入的表單如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。 而現在您...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 609c1c06ed8f29db82b5dd565a935440d4430819
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815391"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>ASP.NET Web Pages 簡介-HTML 表單基本概念
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何建立一個輸入的表單如何處理使用者的輸入，當您使用 ASP.NET Web Pages (Razor) 的基本概念。 現在您有資料庫，您將使用您的表單技術，讓使用者在資料庫中尋找特定的影片。 它假設您已完成透過數列[若要顯示的資料使用 ASP.NET Web Pages 簡介](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)。
> 
> 您將學到什麼：
> 
> - 如何使用標準的 HTML 項目建立的表單。
> - 如何讀取使用者的輸入表單中。
> - 提供如何建立 SQL 查詢，選擇性地取得資料所使用的搜尋詞彙的使用者。
> - 如何在網頁中 「 記住 」 使用者的輸入欄位。
>   
> 
> 功能/技術討論：
> 
> - `Request` 物件。
> - SQL`Where`子句。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，您建立資料庫、 資料加入至它，並接著使用`WebGrid`helper 來顯示資料。 在本教學課程中，您將新增搜尋方塊中，可讓您尋找特定的內容類型的電影，或其標題會包含任何您所輸入的文字。 （比方說，您將能夠找到所有的影片，內容類型是 「 動作 」，或其標題包含"Harry"Adventure"。）

當您完成本教學課程中時，您會有如下的頁面：

![使用內容類型和標題搜尋影片頁面](form-basics/_static/image1.png)

頁面的清單部分是最後一個教學課程中相同&mdash;方格。 的差別是，此方格會顯示只有電影您搜尋的。

## <a name="about-html-forms"></a>關於 HTML 表單

(如果您有經驗，建立 HTML 表單，並且之間的差異`GET`和`POST`，您可以略過這一節。)

表單中的使用者輸入項目的&mdash;文字方塊、 按鈕、 選項按鈕、 核取方塊、 下拉式清單等等。 使用者填入這些控制項或進行選擇，然後再按一下按鈕來提交表單。

此範例說明表單的基本 HTML 語法：

[!code-html[Main](form-basics/samples/sample1.html)]

當在頁面中，執行此標記時，它會建立一個簡單的表單看起來像下圖：

![基本的 HTML 表單，以瀏覽器中轉譯](form-basics/_static/image2.png)

`<form>`元素會括住提交的 HTML 項目。 (進行簡單的錯誤是將項目新增至頁面，但務必放在`<form>`項目。 在此情況下，不會送出。）`method`屬性會告知瀏覽器如何提交使用者輸入。 您將此設`post`如果您執行在伺服器上，或更新`get`如果您只從伺服器擷取資料。

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET、 POST 和 HTTP 動詞命令安全性**
> 
> HTTP 通訊協定的瀏覽器和伺服器用來交換資訊，是其基本的作業非常簡單。 瀏覽器會使用只有少數的動詞命令，對伺服器提出要求。 當您撰寫適用於 web 的程式碼時，最好先了解這些動詞，以及它們如何使用瀏覽器和伺服器。 毫無疑問，最常使用的動詞命令為：
> 
> - `GET`. 瀏覽器會使用此動詞命令，從伺服器擷取的項目。 例如，當您在瀏覽器輸入 URL，瀏覽器執行`GET`作業要求您想要的頁面。 當頁面包括圖形、 瀏覽器會執行其他`GET`作業，以取得映像。 如果`GET`作業已將資訊傳遞至伺服器，將資訊傳遞做為查詢字串中的 URL 的一部分。
> - `POST`. 瀏覽器傳送`POST`要求，以便將資料提交至要加入或變更伺服器上。 比方說，`POST`動詞命令用來在資料庫中建立記錄，或變更現有的。 大部分的情況下，當您填寫表單，並按一下 [提交] 按鈕，瀏覽器執行`POST`作業。 在 `POST`作業，傳遞至伺服器的資料是在頁面的主體。
> 
> 這些動詞重大的差別在於`GET`作業不應變更伺服器上的任何項目，或將它放在稍微更抽象的方式，`GET`作業不會導致在伺服器上的狀態變更。 您可以執行`GET`上相同的資源的次數，而不會變更這些資源的作業。 (A`GET`作業通常稱為 「 安全 」，或使用的技術的詞彙，是*具有等冪性*。)相較之下，當然，`POST`要求會變更伺服器上的項目，每次執行作業。
> 
> 兩個範例將說明這項區別。 當您執行搜尋時使用的引擎，例如 Bing 或 Google，您填寫表單，包含一個文字方塊中，然後按一下 [搜尋] 按鈕。 瀏覽器執行`GET`作業，您做為 URL 的一部分傳遞的方塊中輸入的值。 使用`GET`作業因為搜尋作業不會變更伺服器上的任何資源，是沒問題，這種類型的表單，它只會擷取資訊。
> 
> 現在，請考慮線上項目排序程的序。 您填寫訂單詳細資料，然後按一下 [提交] 按鈕。 這項作業將會`POST`要求，因為此作業會導致在伺服器上，例如新的訂單記錄、 變更您的帳戶資訊，以及可能是許多其他變更的變更。 不同於`GET`作業，您不能重複您`POST`要求 — 如果您這樣做，每次您重新提交要求，您會產生新的順序，在伺服器上。 （在這種情況下，網站通常會警告您不要一次以上，按一下 [提交] 按鈕或將會停用 [提交] 按鈕，使您不小心重新提交表單。）
> 
> 在本教學課程的過程中，您將兩者`GET`作業和`POST`作業來處理 HTML 表單。 我們將說明在每個案例的原因您所使用的動詞命令是適當。
> 
> (若要深入了解 HTTP 動詞命令，請參閱[方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的文章。)


大部分的使用者輸入項目會以 HTML`<input>`項目。 它們看起來像是`<input type="type" name="name">,`何處*型別*表示您想要的使用者輸入控制項的類型。 這些項目是常見的：

- 文字方塊中： `<input type="text">`
- 核取方塊： `<input type="check">`
- 選項按鈕： `<input type="radio">`
- 按鈕： `<input type="button">`
- 提交按鈕： `<input type="submit">`

您也可以使用`<textarea>`元素，來建立多行文字方塊和`<select>`来建立之下拉式清單或可捲動的清單項目。 (如更多有關 HTML 表單項目，請參閱 < [HTML 表單和輸入](http://www.w3schools.com/html/html_forms.asp)W3Schools 站台上。)

`name`屬性是很重要，因為名稱會得到更新版本中，元素的值的方式，您很快會看到。

有趣的部分是網頁開發人員的您，請勿與使用者的輸入。 沒有任何與這些項目相關聯的內建行為。 相反地，您必須取得使用者輸入或選取的值，並運用它們。 這是您將了解在本教學課程。

> [!TIP] 
> 
> **HTML5 和輸入的表單**
> 
> 您可能已經知道，HTML 是轉換中，最新版本 (HTML5) 支援更直覺的方式，讓使用者輸入資訊。 比方說，在 HTML5，您 （網頁開發人員） 所見頁面您希望使用者輸入日期。 行事曆，而不需要使用者手動輸入日期，然後可以自動顯示瀏覽器。 不過，HTML5 新功能，而且尚不支援在所有瀏覽器中。
> 
> ASP.NET Web Pages 支援 HTML5 輸入使用者的瀏覽器所沒有的。 以了解的新屬性`<input>`項目以 HTML5，請參閱[HTML&lt;輸入&gt;type 屬性](http://www.w3schools.com/html/html_form_input_types.asp)W3Schools 站台上。


## <a name="creating-the-form"></a>建立表單

在 WebMatrix 中，在**檔案**工作區中，開啟*Movies.cshtml*頁面。

在結尾後面`</h1>`標記之前開頭`<div>`標記`grid.GetHtml`呼叫時，加入下列標記：

[!code-html[Main](form-basics/samples/sample2.html)]

此標記會建立名為文字方塊的表單`searchGenre`和提交按鈕。 文字並提交 按鈕會括住`<form>`項目其`method`屬性設為`get`。 (請記住，如果您不將文字方塊和提交按鈕內`<form>`項目，當您按一下按鈕時將會提交執行任何動作。)您使用`GET`動詞這裡因為您要建立表單，不會進行任何變更伺服器上 — 它只會造成在搜尋中。 (在上一個教學課程中，您已使用`post`方法，這是您要如何提交到伺服器的變更。 您會看到，在下一個教學課程中一次。）

執行網頁。 雖然您尚未定義表單的任何行為，您可以看到它的外觀：

![「 內容類型的 [搜尋] 方塊的影片頁面](form-basics/_static/image3.png)

輸入的值，在文字方塊中，例如"喜劇。 」 然後按一下**搜尋內容類型**。

記下的網頁 URL。 因為您設定`<form>`項目的`method`屬性設定為`get`，您所輸入的值現在是在 URL 中，像這樣的查詢字串的一部分：

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>讀取表單值

頁面已經包含一些程式碼，取得資料庫的資料，並將結果顯示在方格中。 現在，您必須新增一些程式碼，讀取文字方塊的值，因此您可以執行 SQL 查詢，其中包含搜尋詞彙。

因為您設定表單的方法為`get`，您可以閱讀使用如下所示的程式碼輸入到文字方塊中的值：

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString`物件 (`QueryString`屬性`Request`物件) 包含已提交為一部分的項目值`GET`作業。 `Request.QueryString`屬性包含*集合*（清單） 的形式送出的值。 若要取得任何個別值，您可以指定您想要的項目名稱。 這就是為什麼您必須擁有`name`屬性`<input>`項目 (`searchTerm`)，會建立文字方塊。 (如需詳細資訊`Request`物件，請參閱 <<c2> [ 資訊看板](#BKMK_TheRequestObject)更新版本。)

很容易閱讀的文字方塊的值。 但是，如果使用者未輸入任何項目完全在文字方塊中，但按下**搜尋**無論如何，您可以忽略，按一下，因為沒有要搜尋項目。

下列程式碼是範例，示範如何實作這些條件。 （您不需要尚未新增此程式碼，在您稍後將執行的）。

[!code-csharp[Main](form-basics/samples/sample3.cs)]

測試會細分以這種方式：

- 取得的值`Request.QueryString["searchGenre"]`，也就是輸入的值`<input>`名為的項目`searchGenre`。
- 了解是否空白，便使用`IsEmpty`方法。 這個方法會判斷項目 （例如，表單項目） 是否包含值的標準方式。 但是，您在意有它才*不*空的因此...
- 新增`!`運算子前面`IsEmpty`測試。 (`!`運算子代表邏輯 NOT)。

以純文字的英文，整個`if`條件會轉譯成下列：*表單的 searchGenre 項目不是空的則如果...*

此區塊會設定來建立使用搜尋詞彙的查詢階段。 您將在下一節來這麼做。

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **要求物件**
> 
> `Request`物件包含當您要求或送出頁面時，瀏覽器傳送您的應用程式的所有資訊。 此物件包含使用者提供，例如文字方塊值或上傳檔案的任何資訊。 它也包含各式各樣的其他資訊，例如 cookie 值中的 URL 查詢字串 （如果有的話），正在執行的瀏覽器類型的使用者使用，會在瀏覽器中設定的語言的清單頁面的檔案路徑以及其他更多功能。
> 
> `Request`物件是否*集合*（清單） 的值。 您可以指定其名稱，以取得集合的個別值：
> 
> `var someValue = Request["name"];`
> 
> `Request`物件實際上會公開數個子集。 例如: 
> 
> - `Request.Form` 可讓您從項目內已提交的值`<form>`項目，如果要求是`POST`要求。
> - `Request.QueryString` 提供值的 URL 查詢字串。 (在之類的 URL `http://mysite/myapp/page?searchGenre=action&page=2`，則`?searchGenre=action&page=2`URL 區段是查詢字串。)
> - `Request.Cookies` 集合可讓您存取瀏覽器所傳送的 cookie。
> 
> 若要取得值，您知道處於 已提交表單，您可以使用`Request["name"]`。 或者，您可以使用更特定的版本`Request.Form["name"]`(如`POST`要求) 或`Request.QueryString["name"]`(如`GET`要求)。 當然*名稱*是要取得之項目的名稱。
> 
> 您想要取得的項目的名稱必須是唯一您使用在集合中。 這就是為什麼`Request`物件所提供的子集喜歡`Request.Form`和`Request.QueryString`。 假設您的頁面包含名為表單項目`userName`並*也*包含名為 cookie `userName`。 如果您收到`Request["userName"]`，模稜兩可您要的表單值或 cookie。 不過，如果您收到`Request.Form["userName"]`或`Request.Cookie["userName"]`，您要瞭如指掌要取得的值。
> 
> 它是個不錯的做法是特定且使用的子集`Request`感興趣，例如`Request.Form`或`Request.QueryString`。 如您在本教學課程中建立簡單的頁面，它可能不會真的會造成任何差異。 不過，當您建立更複雜的頁面時，使用明確的版本`Request.Form`或`Request.QueryString`可協助您避免發生問題時的頁面包含表單 （或多個表單），可能發生的 cookie、 查詢字串值等等。


## <a name="creating-a-query-by-using-a-search-term"></a>建立查詢利用搜尋詞彙

既然您已經知道如何取得使用者輸入搜尋詞彙，您可以建立使用它的查詢。 請記住，若要取得移出資料庫的所有影片項目，您使用 SQL 查詢，看起來像這個陳述式：

`SELECT * FROM Movies`

若要取得特定的電影，您必須使用包含的查詢`Where`子句。 此子句可讓您設定的條件的查詢所傳回的資料列。 以下為範例：

`SELECT * FROM Movies WHERE Genre = 'Action'`

基本格式`WHERE column = value`。 Besides 只是，您可以使用不同的運算子`=`，例如`>`（大於）、 `<` （小於）、 `<>` （不等於）、 `<=` （小於或等於）、 逗號等等，視您要尋找的內容。

您可能會感到奇怪，SQL 陳述式都不會區分大小寫&mdash;`SELECT`等同於`Select`(或甚至`select`)。 不過，人們通常利用 SQL 陳述式中的關鍵字類似`SELECT`和`WHERE`，以便於讀取。

### <a name="passing-the-search-term-as-a-parameter"></a>傳遞做為參數的搜尋字詞

搜尋特定的內容類型也很簡單 (`WHERE Genre = 'Action'`)，但您想要能夠搜尋任何使用者輸入的內容類型。 若要這樣做，您會建立為包含要搜尋的預留位置值的 SQL 查詢。 它看起來像這個命令：

`SELECT * FROM Movies WHERE Genre = @0`

預留位置是`@`字元後面接著零。 您可能會猜想，查詢可以包含多個預留位置，並會命名為`@0`， `@1`，`@2`等等。

若要設定查詢，並實際將它傳遞值，您可以使用如下所示的程式碼：

[!code-sql[Main](form-basics/samples/sample4.sql)]

此程式碼很類似什麼您已經完成此方格中顯示資料。 唯一的差異如下：

- 此查詢會包含預留位置 (`WHERE Genre = @0"`)。
- 查詢會放入變數 (`selectCommand`); 之前，您也可以直接將查詢傳遞`db.Query`方法。
- 當您呼叫`db.Query`方法中，您傳送查詢和要使用的預留位置的值。 (如果查詢有多個預留位置，您會將其傳遞做為個別值的方法。)

如果您同時將所有這些項目，您會取得下列的程式碼：

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **重要！** 使用預留位置 (例如`@0`) 若要將值傳遞給 SQL 命令*極為重要*安全性。 您在此見報，具有預留位置變數的資料，方法是您應該建構 SQL 命令的唯一方法。
> 
> 永遠不會建構 SQL 陳述式放在一起，（串連） 的常值文字和您從使用者取得的值。 串連使用者輸入 SQL 陳述式會開啟您的站台*SQL 資料隱碼攻擊*當惡意使用者送出至您的頁面 hack 您資料庫的值。 (您可以閱讀更多文件中[SQL 資料隱碼](https://msdn.microsoft.com/library/ms161953.aspx)MSDN 網站。)


## <a name="updating-the-movies-page-with-search-code"></a>使用 搜尋程式碼更新影片頁面

現在您可以更新中的程式碼*Movies.cshtml*檔案。 若要開始，請在頁面頂端的程式碼區塊中的程式碼取代此程式碼：

[!code-csharp[Main](form-basics/samples/sample6.cs)]

此處的差異在於，您將查詢貼入`selectCommand`變數，您要傳遞至`db.Query`更新版本。 將放入變數的 SQL 陳述式，可讓您變更陳述式，也就是您需要執行搜尋。

您也移除了下列兩行，您會將放回在更新版本：

[!code-csharp[Main](form-basics/samples/sample7.cs)]

您不打算尚未執行查詢 (也就是呼叫`db.Query`)，您不想要初始化`WebGrid`協助程式，但其中一個。 您已了解哪些 SQL 陳述式已執行之後，您會執行這些項目。

之後重寫區塊中，您可以加入新的邏輯來處理搜尋。 已完成的程式碼看起來如下所示。 使它符合此範例中，請更新您的頁面中的程式碼：

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

頁面現在的運作方式如下。 每次執行網頁時，程式碼會開啟資料庫和`selectCommand`變數設為 SQL 陳述式可取得所有記錄`Movies`資料表。 程式碼也會初始化`searchTerm`變數。

不過，如果目前的要求中包含的值`searchGenre`項目、 程式碼集`selectCommand`不同的查詢 — 那就是，為包含`Where`子句來搜尋的內容類型。 它也會設定`searchTerm`任何已傳遞的搜尋方塊中 （這可能是 nothing）。

不論 SQL 陳述式是在`selectCommand`，然後呼叫程式碼`db.Query`若要執行查詢時，將它傳遞任何位於`searchTerm`。 如果沒有`searchTerm`，它並不重要，因為在此情況下沒有任何參數，將值傳遞至`selectCommand`嗎。

最後，程式碼會初始化`WebGrid`使用查詢結果中的，像以前一樣的協助程式。

您所見，將 SQL 陳述式，並搜尋詞彙，到變數中，您已加入彈性的程式碼。 如您所見本教學課程稍後，您可以使用此基本架構，並持續新增不同類型的搜尋邏輯。

## <a name="testing-the-search-by-genre-feature"></a>要測試的內容類型的搜尋功能

在 WebMatrix 中，執行*Movies.cshtml*頁面。 您會看到內容類型的文字方塊中的頁面。

輸入，您已為您的測試記錄的其中一個輸入，然後按一下 內容類型**搜尋**。 此時您會看到只比對該內容類型的影片清單：

![列出搜尋的內容類型 'Comedies' 之後的影片頁面](form-basics/_static/image4.png)

輸入不同的內容類型，然後再搜尋一次。 請嘗試輸入的內容類型，藉由使用全部小寫或全部大寫的字母，如此一來，您可以看到搜尋不區分大小寫。

## <a name="remembering-what-the-user-entered"></a>「 記住 」 使用者所輸入的內容

您可能已經注意到，在您輸入的內容類型，並按下之後**搜尋內容類型**，您會看到該內容類型清單。 不過，[搜尋] 方塊是空&mdash;換句話說，它不記得您必須輸入。

請務必了解為什麼會發生此問題。 當您提交頁面時，瀏覽器會傳送要求到 web 伺服器。 當 ASP.NET 取得要求時，它會建立頁面的全新執行個體、 執行的程式碼，並再一次呈現至瀏覽器頁面。 實際上，不過，頁面並不知道，您就已使用的舊版本身。 所有它知道它有要求，其中有一些表單中的資料。

每次要求頁面&mdash;第一次還是將其提交出去&mdash;您收到新的頁面。 Web 伺服器會有上次要求的任何記憶體。 兩者都不會執行 ASP.NET，並都不會在瀏覽器。 這些個別的執行個體的頁面之間唯一的連線是您傳送其間的任何資料。 如果您送出頁面，比方說，新的頁面執行個體可以取得舊版的執行個體送出表單資料。 （另一種頁面之間傳遞資料的方式是使用 cookie）。

型式的方式，來說明這種情況下為網頁都*無狀態*。 Web 伺服器和網頁本身和頁面中的項目不會維護任何先前的頁面狀態的相關資訊。 Web 的設計是這種方式，因為維護個別要求的狀態會快速耗盡資源的 web 伺服器，通常可能處理上千條，甚至數百個，每秒的要求。

所以這就是為什麼文字方塊為空白。 送出頁面之後，ASP.NET 會建立頁面的新執行個體，並執行程式碼和標記。 沒有任何在該告知 ASP.NET 將值放入文字方塊中的程式碼。 因此，不執行任何動作，ASP.NET 並沒有值，以在其中呈現文字方塊。

沒有實際輕鬆地取得解決此問題。 您在文字方塊中輸入的內容類型*已*為您提供的程式碼&mdash;處於`Request.QueryString["searchGenre"]`。

更新 [] 文字方塊中的標記以便`value`取得其值的屬性， `searchTerm`，如這個範例所示：

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

在此頁面上，您可能有也設定`value`屬性設定為`searchTerm`您輸入變數，因為該變數也包含內容類型。 但使用`Request`物件來設定`value`屬性所示，以下是完成這項工作的標準方式。 (假設您甚至想要執行這項操作&mdash;在某些情況下，您可能想要呈現的頁面*而不需要*中欄位的值。 一切取決於發生什麼情況與您的應用程式。）

> [!NOTE]
> 您無法 「 記住 」 所使用的密碼 文字方塊中的值。 它會讓人可以使用程式碼填入在密碼欄位中的安全性漏洞。


再次執行頁面，輸入內容類型，然後按一下**搜尋內容類型**。 這次不只您看的搜尋結果，但文字方塊會記住您上次輸入：

![顯示文字方塊具有 '記住' 先前的項目頁面](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>搜尋標題中的任何文字

您現在可以搜尋任何內容類型，但您也可以在搜尋標題。 很難取得標題沒錯，當您搜尋，因此您可搜尋的任何位置出現在標題文字。 若要在 SQL 中這麼做，您使用`LIKE`運算子和語法如下所示：

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

此命令會取得標題中含有"adventure"的所有影片。 當您使用`LIKE`運算子，包括萬用字元`%`做為搜尋條件的一部分。 搜尋`LIKE 'adventure%'`表示 「 開頭為 'adventure' 」。 （技術上來說，這表示 「 字串 'adventure' 後面的任何項目。 」）同樣地，搜尋字詞`LIKE '%adventure'`表示 「 任何項目後面的字串 'adventure' 」，這是說: 「 'adventure' 做為結尾 」 的另一種方式。

搜尋字詞`LIKE '%adventure%'`因此表示 「 具有 'adventure' 標題中任何地方。 」 （技術上來說，「 任何標題，後面接著 'adventure'，後面接著的任何項目中。"）

內`<form>`項目，加入下列標記正下方的右`</div>`內容類型搜尋的標記 (只在關閉前`</form>`項目):

[!code-html[Main](form-basics/samples/sample10.html)]

程式碼來處理此搜尋是內容類型搜尋的程式碼類似，不同之處在於您必須組合`LIKE`搜尋。 程式碼區塊頂端的頁面中，新增這`if`正後方封鎖`if`內容類型搜尋的區塊：

[!code-csharp[Main](form-basics/samples/sample11.cs)]

此程式碼會使用您先前看到的相同邏輯，不同之處在於會使用`LIKE`運算子，而且程式碼將 「`%`"之前和之後的搜尋字詞。

請注意如何很容易就能新增至頁面的另一項搜尋。 那就行了：

- 建立`if`測試，以查看相關的搜尋方塊中是否有值的區塊。
- 設定`selectCommand`變數設為新的 SQL 陳述式。
- 設定`searchTerm`變數的值，以傳遞給查詢。

以下是完整的程式碼區塊，其中包含項目的搜尋的新邏輯：

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

此程式碼所執行的作業的摘要如下：

- 變數`searchTerm`和`selectCommand`頂端會初始化。 您要將這些變數設定為適當的搜尋字詞 （如果有的話），使用者會在頁面根據適當的 SQL 命令。 預設搜尋是簡單的情況下，從資料庫取得所有影片。
- 中的測試`searchGenre`並`searchTitle`，程式碼集`searchTerm`您想要搜尋的值。 這些程式碼區塊也設定`selectCommand`該搜尋適當的 SQL 命令。
- `db.Query`方法叫用一次，使用 SQL 命令`selectedCommand`中的任何值，而且`searchTerm`。 如果沒有搜尋詞彙 （沒有內容類型和任何標題 word） 的值`searchTerm`為空字串。 不過，，並不重要，因為在此情況下查詢並不需要參數。

## <a name="testing-the-title-search-feature"></a>測試的標題搜尋功能

現在您可以測試您的已完成的搜尋頁面。 執行*Movies.cshtml*。

輸入的內容類型，然後按一下**搜尋內容類型**。 方格會顯示影片的該內容類型，例如之前。

輸入 標題文字，然後按一下**搜尋標題**。 方格會顯示在標題中有該單字的電影。

!['' 中搜尋標題之後列出影片頁面](form-basics/_static/image6.png)

將兩個文字方塊保留空白，然後按一下任一個按鈕。 方格會顯示所有影片。

## <a name="combining-the-queries"></a>合併查詢

您可能會注意到，您可以執行的搜尋會獨佔。 您無法搜尋標題和內容類型在此同時，即使這兩個搜尋方塊中有值。 例如，您無法搜尋其標題包含"Adventure"的所有動作電影。 （頁面是程式碼現在，如果您輸入的內容類型和標題值項目的搜尋取得優先順序。）若要建立結合條件的搜尋，您必須建立 SQL 查詢，如下所示的語法：

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

而且您必須使用如下的陳述式來執行查詢 （粗略地說，）：

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

建立邏輯，以允許的搜尋準則的許多排列方式可讓有點複雜，如您所見。 因此，我們將在此停止。

## <a name="coming-up-next"></a>接下來的下一步

在下一個教學課程中，您將建立使用表單，讓使用者可將影片新增到資料庫的頁面。

## <a name="complete-listing-for-movie-page-updated-with-search"></a>（使用搜尋服務更新） 的電影頁面的完整清單

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](https://go.microsoft.com/fwlink/?LinkID=202890)
- [SQL WHERE 子句](http://www.w3schools.com/sql/sql_where.asp)W3Schools 站台上
- [方法定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C 網站上的文章

> [!div class="step-by-step"]
> [上一頁](displaying-data.md)
> [下一頁](entering-data.md)
