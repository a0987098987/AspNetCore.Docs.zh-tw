---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: 導入 ASP.NET Web Pages-更新資料庫資料 |Microsoft 文件
author: tfitzmac
description: 本教學課程會示範如何更新現有的資料庫 （變更） 項目，當您使用 ASP.NET Web Pages (Razor)。 它會假設您已經完成數列 th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: e889cd27e2267a08f7b6ea708c92e35edbdd7a1a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898531"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>導入的 ASP.NET Web Pages-更新資料庫的資料
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何更新現有的資料庫 （變更） 項目，當您使用 ASP.NET Web Pages (Razor)。 它會假設您已完成透過數列[輸入資料所使用的表單使用 ASP.NET Web Pages](entering-data.md)。
> 
> 您將學習：
> 
> - 如何選取中的個別記錄`WebGrid`協助程式。
> - 如何從資料庫讀取的單筆記錄。
> - 如何預先載入的表單具有資料庫記錄的值。
> - 如何更新資料庫中的現有記錄。
> - 如何將資訊儲存在頁面中，而不會顯示它。
> - 如何使用隱藏的欄位來儲存資訊。
>   
> 
> 功能/技術討論：
> 
> - `WebGrid`協助程式。
> - SQL`Update`命令。
> - `Database.Execute` 方法
> - 隱藏的欄位 (`<input type="hidden">`)。


## <a name="what-youll-build"></a>您將建置

在上一個教學課程中，您將學會如何將記錄加入至資料庫。 在這裡，您將學習如何顯示供編輯的記錄。 在*電影* 頁面上，您要更新`WebGrid`helper，因此它會顯示**編輯**每部電影旁邊的連結：

![WebGrid 顯示包括每個影片 [編輯] 連結](updating-data/_static/image1.png)

當您按一下**編輯**連結，它將您導向至其他頁面，電影資訊已在表單：

![編輯電影 畫面，顯示要編輯的電影](updating-data/_static/image2.png)

您可以變更的任何值。 當您送出變更時，頁面中的程式碼會更新資料庫，並帶您回到電影清單。

這部分的程序方式幾乎完全相同*AddMovie.cshtml*您建立先前的教學課程中，因此本教學課程中的大部分都很熟悉的頁面。

有數種方式，您可以實作來編輯個別的電影。 顯示的方法已被選擇，因為它實作簡單且輕鬆地了解。

## <a name="adding-an-edit-link-to-the-movie-listing"></a>加入的電影清單的編輯連結

若要開始，您要更新*電影*頁面，如此也列出每個影片包含**編輯**連結。

開啟*Movies.cshtml*檔案。

在網頁主體中，變更`WebGrid`加入資料行的標記。 以下是修改過的標記：

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

新的資料行是一個：

[!code-html[Main](updating-data/samples/sample2.html)]

此資料行的重點是要顯示連結 (`<a>`元素) 的文字會顯示 [編輯]。 我們之後是建立時執行的網頁時，看起來像下列的連結與`id`值不同的每個影片：

[!code-css[Main](updating-data/samples/sample3.css)]

此連結會叫用名為的頁面*EditMovie*，它將傳遞查詢字串和`?id=7`至該頁面。

新的資料行的語法看起來可能有點複雜，但這只是因為它將放在一起的幾個元素。 每個個別項目相當簡單。 如果您專注於剛`<a>`項目，您會看到這個標記：

[!code-html[Main](updating-data/samples/sample4.html)]

方格的運作方式的一些背景： 方格會顯示資料列，其中每個資料庫記錄，以及資料庫記錄中顯示的每個欄位的資料行。 建構每一個方格資料列時，雖然`item`物件包含該資料列的資料庫記錄 （項目）。 這種安排可讓您進行程式碼，讓該資料列資料中。 這是這裡所示： 運算式`item.ID`即將目前的資料庫項目的 ID 值。 您可以取得的任何資料庫值 （標題、 內容類型或年） 相同的方式使用`item.Title`， `item.Genre`，或`item.Year`。

運算式`"~/EditMovie?id=@item.ID`結合的硬式編碼的目標 URL 部分 (`~/EditMovie?id=`) 使用此動態衍生的識別碼。 (您已看到`~`運算子在上一個教學課程中，這是一個代表目前的網站根目錄的 ASP.NET 運算子。)

結果是標記的，這一部分資料行中只會產生類似下列的標記在執行階段：

[!code-xml[Main](updating-data/samples/sample5.xml)]

當然，實際值`id`將每個資料列的不同。

## <a name="creating-a-custom-display-for-a-grid-column"></a>建立自訂顯示的方格資料行

現在返回方格資料行。 三個資料行，您原本必須在方格中顯示資料的值 （標題、 類型和年）。 您指定此顯示名稱的資料庫資料行傳遞&mdash;，例如`grid.Column("Title")`。

這個新**編輯**連結資料行是不同。 而不指定資料行名稱，您傳遞`format`參數。 這個參數可讓您定義標記， `WebGrid` helper 會呈現連同`item`粗體或綠色顯示資料行的資料，或在任何格式，您想要的值。 例如，如果您想要顯示為粗體標題時，您可以建立的資料行，如下列範例：

[!code-html[Main](updating-data/samples/sample6.html)]

(各種`@`中看到的字元`format`屬性標記的標記和程式碼值之間的轉換。)

一旦您了解`format`屬性，它會更易於了解如何新**編輯**連結資料行放在一起：

[!code-html[Main](updating-data/samples/sample7.html)]

資料行包含*只*呈現連結的標記，再加上一些資訊 (ID)，從擷取的資料列的資料庫記錄。

> [!TIP]
> 
> **具名的參數和方法的位置參數**
> 
> 許多次當您呼叫方法並傳遞給它的參數，您已直接列以逗號分隔的參數值。 以下提供幾個範例：
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> 當您第一次看到此程式碼，但在每個案例中，要將參數傳遞至以特定順序的方法時，我們沒有提到問題&mdash;也就是定義在方法中的參數順序。 如`db.Execute`和`Validation.RequireFields`，如果您混您傳遞值的順序時，您會收到錯誤訊息時執行的網頁時或至少一些奇怪的結果。 很明顯地，您必須知道要傳遞的參數順序。 （在 WebMatrix 中，IntelliSense 可協助您了解找出名稱、 類型和參數的順序）。
> 
> 您可以使用順序的傳遞值的替代方案，*具名參數*。 (為了傳遞參數稱為使用*位置參數*。)若為具名參數，您明確包含參數的名稱傳遞它的值時。 您已使用具名的參數已經數次這些教學課程。 例如: 
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> 和
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> 尤其是當方法會採用許多參數，是實用的情況下，數個具名的參數。 其中一個是您想要只需要一個或兩個參數傳遞，但您想要傳遞的值不是參數清單中的第一個位置。 另一種情況時，您想要藉由使用最適合您的順序傳遞參數，讓您的程式碼更容易閱讀。
> 
> 很明顯地，若要使用具名的參數，您必須知道參數的名稱。 WebMatrix IntelliSense 可以*顯示*您名稱，但它無法目前它們為您填入。


## <a name="creating-the-edit-page"></a>建立編輯頁面

現在您可以建立*EditMovie*頁面。 當使用者按一下**編輯**連結，在最後會在此頁面上。

建立名為的頁面*EditMovie.cshtml*和取代功能的檔案，以下列標記：

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

此標記和程式碼是類似於您在*AddMovie*頁面。 沒有 [提交] 按鈕的文字上有些許不同。 如同*AddMovie*頁面上，有`Html.ValidationSummary`如果有的話，將會顯示驗證錯誤的呼叫。 目前我們正在留下任何呼叫`Validation.Message`，因為驗證摘要中會顯示錯誤。 上一個教學課程所述，您可以使用各種組合的摘要驗證和個別的錯誤訊息。

請注意一次，`method`屬性`<form>`元素設定為`post`。 如同*AddMovie.cshtml*  頁面上，此頁面變更資料庫。 因此，應該執行這種形式`POST`作業。 (如需詳細資訊之間的差異`GET`和`POST`作業，請參閱[GET、 POST 和 HTTP 動詞命令安全性](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety)HTML 表單上的教學課程中的 [資訊看板]。)

如同先前的教學課程，`value`文字方塊中的屬性使用 Razor 程式碼以預先載入這些設定。 此時，不過，您使用變數，如下`title`和`genre`為該工作，而不是`Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

做為前，這個標記會預先載入的影片值的文字方塊值。 您會看到為何方便這次使用變數，而不是使用立即`Request`物件。

另外還有`<input type="hidden">`此頁面上的項目。 這個項目不直接以便顯示在頁面上儲存電影識別碼。 識別碼一開始傳送頁面所使用的查詢字串值 (`?id=7`或類似的 URL 中)。 將識別碼值放入隱藏的欄位，您可以確保可以使用時提交表單時，即使您不再需要存取的頁面以叫用的原始 URL。

不同於*AddMovie*頁面上的程式碼*EditMovie*頁面有兩個不同的函式。 第一個函式是第一次顯示網頁時 (和*只*然後)，程式碼從查詢字串取得的電影識別碼。 接著，程式碼使用識別碼來讀取從資料庫對應的影片和顯示 （預先載入） 它的文字方塊中。

第二個函式是： 當使用者按一下**送出變更** 按鈕的程式碼讀取之文字方塊值和進行驗證。 程式碼也必須更新資料庫項目使用的新值。 這項技術非常類似於將記錄中看到*AddMovie*。

## <a name="adding-code-to-read-a-single-movie"></a>加入程式碼來讀取單一影片

若要執行第一個函式，將加入至頁面頂端的這段程式碼：

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

大部分的這段程式碼會啟動在區塊內`if(!IsPost)`。 `!`運算子代表"not，"所以運算式表示*如果這個要求不是在 post 送出*，這是間接的一種說法*此要求是否已執行過此頁面第一次*。 如前文所述，此程式碼應該執行*只*頁面會執行第一次。 如果您沒有使用中的程式碼將`if(!IsPost)`，它會在每次頁面叫用時，是否在第一次執行或回應的按鈕中按一下。

請注意，程式碼包含`else`封鎖這個時間。 當我們引進了如前所述`if`區塊，有時候您想要執行替代程式碼，如果您要測試的條件不為 true。 這是此案例。 如果條件通過 （亦即，如果傳遞至頁面的識別碼為 [確定]），您會從資料庫讀取資料列。 不過，如果條件未通過，`else`區塊會執行與此程式碼設定錯誤訊息。

## <a name="validating-a-value-passed-to-the-page"></a>驗證值，傳遞至網頁

程式碼會使用`Request.QueryString["id"]`取得傳遞至頁面的識別碼。 程式碼可確保識別碼實際傳遞值 如果不傳遞任何值，此程式碼會設定驗證錯誤。

此程式碼將示範不同的方法來驗證資訊。 在上一個教學課程中，您在使用`Validation`協助程式。 註冊若要驗證，欄位和 ASP.NET 自動未驗證，並顯示錯誤使用`Html.ValidationMessage`和`Html.ValidationSummary`。 在此情況下，不過，您正在並未真正驗證使用者輸入。 相反地，您要驗證已傳遞至網頁中其他位置的值。 `Validation`協助程式並不會為您。

因此，您自行確認值，藉由測試它與`if(!Request.QueryString["ID"].IsEmpty()`)。 如果沒有問題，您可以使用顯示錯誤`Html.ValidationSummary`，如同您一樣`Validation`協助程式。 若要這樣做，請呼叫`Validation.AddFormError`並將它傳遞要顯示的訊息。 `Validation.AddFormError` 是內建方法，可讓您定義自訂驗證系統，您已熟悉相結合的訊息。 （在本教學課程稍後我們將討論如何進行此驗證程序更穩固。）

確定電影識別碼之後，程式碼會在資料庫中，尋找用於只有單一資料庫的項目。 (您可能已注意到一般資料庫作業模式： 開啟資料庫、 定義 SQL 陳述式，並執行陳述式。)目前，SQL`Select`陳述式包含`WHERE ID = @0`。 識別碼是唯一的因為只有一筆記錄可能會傳回。

查詢透過執行`db.QuerySingle`(不`db.Query`、 所使用的電影清單)，以及程式碼將放入結果`row`變數。 名稱`row`可自行決定; 您要的任何項目名稱的變數。 初始化上方的變數則會填入電影詳細資料，使這些值可以顯示在文字方塊中。

## <a name="testing-the-edit-page-so-far"></a>測試 [編輯] 頁面 （目前）

如果您想要測試您的頁面，執行*電影*現在頁面上，按一下 **編輯**旁邊任何影片連結。 您會看到*EditMovie*詳細資料填滿頁面針對您選取的影片：

![編輯電影 畫面，顯示要編輯的電影](updating-data/_static/image3.png)

請注意，網頁的 URL 會包含類似`?id=10`（或其他數字）。 到目前為止已測試的**編輯**中連結*影片*頁面上的工作，您的頁面從查詢字串中，讀取識別碼，且資料庫查詢以取得單一影片記錄正在運作。

您可以變更的電影資訊，但是當您按一下時，會發生任何事**送出變更**。

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>加入程式碼以更新使用者的變更與電影

在*EditMovie.cshtml*檔案中，若要實作第二個函式 （儲存變更），加入下列程式碼的右括號之內`@`區塊。 (如果您不確定的確切位置放置程式碼，您可以查看[完整程式碼清單編輯電影頁面](#Complete_Page_Listing_for_EditMovie)出現在本教學課程結尾處。)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

同樣地，這個標記和程式碼是類似的程式碼中*AddMovie*。 程式碼位於`if(IsPost)`封鎖，因為只有當使用者按一下時，才執行此程式碼**送出變更**按鈕&mdash;也就是當 （和時，才） 已張貼的表單。 在此情況下，您不使用類似的測試`if(IsPost && Validation.IsValid())`— 也就您無法合併這兩項測試使用 and。 在此頁面中，您先判斷是否送出表單 (`if(IsPost)`)，然後只再註冊驗證的欄位。 然後您可以測試驗證結果 (`if(Validation.IsValid()`)。 資料流程會稍有不同於在*AddMovie.cshtml*  頁面上，但效果是一樣。

使用取得的值之文字方塊`Request.Form["title"]`和其他類似的程式碼`<input>`項目。 請注意，這次程式碼會取得影片識別碼從隱藏的欄位 (`<input type="hidden">`)。 當頁面執行第一次時，程式碼會取得從查詢字串的識別碼。 您會取得從隱藏的欄位中，請確定您可以獲得最初顯示的電影識別碼以防從那時起由於某種原因而改變的查詢字串值。

而言非常重要差異*AddMovie*碼與這個程式碼是在這段程式碼中使用 SQL`Update`陳述式來取代`Insert Into`陳述式。 下列範例示範的 SQL 語法`Update`陳述式：

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

您可以依任何順序指定的任何資料行，您不一定需要更新每個資料行期間`Update`作業。 (您無法更新本身的 ID，因為，將作用中的記錄儲存為新的記錄，並不允許的`Update`作業。)

> [!NOTE] 
> 
> **重要**`Where`子句識別碼是非常重要，因為這是資料庫如何知道哪些資料庫要更新記錄。 如果您離開`Where`子句中，資料庫會更新*每*記錄資料庫中。 大部分情況下，這可能會損毀。


在程式碼中，若要更新的值會傳遞至 SQL 陳述式使用預留位置。 若要重複我們剛才說過： 基於安全考量，*只*將值傳遞至 SQL 陳述式中使用的預留位置。

程式碼會使用後`db.Execute`執行`Update`陳述式，它將重新導向回到清單頁面上，您可以在其中看到變更。

> [!TIP] 
> 
> **不同的 SQL 陳述式，不同的方法**
> 
> 您可能已注意到您在執行不同的 SQL 陳述式使用稍微不同的方法。 若要執行`Select`亦可能查詢傳回多筆記錄，請您使用`Query`方法。 若要執行`Select`您知道的查詢會傳回只有一個資料庫項目，則使用`QuerySingle`方法。 若要執行的命令，進行變更，但不傳回資料庫項目，您使用`Execute`方法。
> 
> 您必須擁有不同的方法，因為每個會傳回不同的結果，如同已經之間的差異`Query`和`QuerySingle`。 (`Execute`方法實際上會傳回值也&mdash;也就是已受到該命令的資料庫資料列數目&mdash;您已被忽略，到目前為止，但。)
> 
> 當然，`Query`方法可能會傳回只有一個資料庫的資料列。 不過，ASP.NET 一律會將結果`Query`做為集合的方法。 即使這個方法會傳回一個資料列，您必須從集合中擷取該單一資料列。 因此，在情況下，您*知道*您會得到只有一個資料列，它是更方便使用的位元`QuerySingle`。
> 
> 有幾個其他執行特定類型的資料庫作業的方法。 您可以找到資料庫中的方法清單[ASP.NET Web Pages 應用程式開發介面的快速參考](../../api-reference/asp-net-web-pages-api-reference.md#Data)。


## <a name="making-validation-for-the-id-more-robust"></a>進行驗證 ID 更健全

執行網頁時，第一次您的影片識別碼從取得查詢字串，讓您可以從資料庫取得該電影。 您確定實際發生移，看起來就像您使用這個程式碼的值：

[!code-csharp[Main](updating-data/samples/sample13.cs)]

您用來確保當使用者取得的這段程式碼*EditMovies*頁面上，必須先選取電影*電影* 頁面上，頁面會顯示使用者易記的錯誤訊息。 （否則使用者會看到錯誤，可能會或許只要混淆）。

不過，這項驗證不是非常強大。 頁面也可能會發生這些錯誤叫用：

- 識別碼不是數字。 例如，無法 URL，例如叫用網頁`http://localhost:nnnnn/EditMovie?id=abc`。
- 識別碼是一個數字，但它會參考不存在的影片 (例如， `http://localhost:nnnnn/EditMovie?id=100934`)。

如果您想知道，請參閱錯誤所造成的這些 Url，執行*電影*頁面。 選取要編輯電影，然後變更 URL *EditMovie*識別碼或不存在影片識別碼至包含英文字母的 URL 頁面上。

因此您該怎麼做？ 第一個解決方法是確保可不只識別碼傳遞至頁面上，但識別碼是整數。 變更的程式碼`!IsPost`測試此範例類似：

[!code-csharp[Main](updating-data/samples/sample14.cs)]

您已新增第二個條件`IsEmpty`與連結的測試， `&&` (邏輯 AND):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

您可能記得從[簡介 ASP.NET Web Pages 程式設計](../introducing-razor-syntax-c.md)等方法的教學課程`AsBool``AsInt`將字元字串轉換成其他資料類型。 `IsInt`方法 (和其他項目，例如`IsBool`和`IsDateTime`) 類似。 不過，只是測試是否您*可以*轉換字串，而不需實際執行轉換。 這裡基本上就*如果查詢字串值可以轉換成整數...*.

不存在的電影正在尋找潛在問題。 若要取得電影的程式碼看起來像此程式碼：

[!code-csharp[Main](updating-data/samples/sample16.cs)]

如果您要傳入`movieId`值設定為`QuerySingle`未對應到實際的電影的方法，不會傳回與陳述式，遵循 (比方說， `title=row.Title`) 會導致錯誤。

一次是簡單的修正程式。 如果`db.QuerySingle`方法會傳回任何結果`row`變數將會是 null。 因此您可以檢查是否`row`再重新從其取得值，變數為 null。 下列程式碼加入`if`前後取得的值的陳述式區塊`row`物件：

[!code-csharp[Main](updating-data/samples/sample17.cs)]

使用下列兩個額外的驗證測試，頁面會變成更多的項目符號證明。 完整程式碼`!IsPost`分支現在看起來像此範例中：

[!code-csharp[Main](updating-data/samples/sample18.cs)]

我們將請注意一次，這項工作的好使用`else`區塊。 如果測試不成功，`else`區塊設定錯誤訊息。

## <a name="adding-a-link-to-return-to-the-movies-page"></a>加入以返回電影頁面的連結

最終且實用的詳細資料是將連結新增回到*電影*頁面。 在一般流程中的事件，使用者將會在啟動*電影*頁面上，按一下 **編輯**連結。 將它們整合到*EditMovie*頁面上，使用者可以在此編輯電影，並按一下按鈕。 程式碼會處理變更之後，它將重新導向回到*電影*頁面。

不過：

- 使用者可能決定不變更任何項目。
- 使用者可能會收到此頁面沒有先按一下**編輯**中連結*電影*頁面。

無論如何，您會想要讓他們可以輕鬆將返回主要清單。 它是簡單的修正&mdash;後方結尾加入下列標記`</form>`標記中的標記：

[!code-html[Main](updating-data/samples/sample19.html)]

這個標記會使用的相同語法`<a>`其他位置，您所見的元素。 URL 包含`~`表示 「 網站的根。 」

## <a name="testing-the-movie-update-process"></a>測試影片更新程序

現在您可以測試。 執行*電影*頁面，然後按一下**編輯**電影旁邊。 當*EditMovie*頁面出現時，變更電影，然後按一下**送出變更**。 電影清單出現時，請確定您的變更會顯示。

若要確定驗證是否運作，請按一下**編輯**另一部電影。 當您進入*EditMovie*頁面上，清除**類型**欄位 (或**年** 欄位中，或兩者)，然後再次嘗試送出您的變更。 如您所預期，您會看到錯誤：

![編輯電影 畫面，顯示驗證錯誤](updating-data/_static/image4.png)

按一下**返回電影清單**連結以放棄變更並返回*電影*頁面。

## <a name="coming-up-next"></a>接下來

在下一個教學課程中，您會看到如何刪除電影錄製。

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>電影頁面 （編輯連結以更新） 的完整清單

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>頁面的完整清單編輯電影頁面

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](../../getting-started/introducing-razor-syntax-c.md)
- [SQL UPDATE 陳述式](http://www.w3schools.com/sql/sql_update.asp)W3Schools 站台上

> [!div class="step-by-step"]
> [上一頁](entering-data.md)
> [下一頁](deleting-data.md)
