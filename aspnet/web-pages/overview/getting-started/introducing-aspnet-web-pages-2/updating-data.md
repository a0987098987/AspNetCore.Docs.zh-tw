---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: 簡介 ASP.NET Web Pages-更新資料庫的資料 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何更新 （變更） 現有的資料庫項目，當您使用 ASP.NET Web Pages (Razor)。 它假設您已完成序列個...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 206d7e209857aceb3eb92c2405bb73f7ff7dbaeb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021426"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>ASP.NET Web Pages 簡介-更新資料庫資料
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何更新 （變更） 現有的資料庫項目，當您使用 ASP.NET Web Pages (Razor)。 它假設您已完成透過數列[輸入資料所使用的表單使用 ASP.NET Web Pages](entering-data.md)。
> 
> 您將學到什麼：
> 
> - 如何選取中的個別記錄`WebGrid`協助程式。
> - 如何從資料庫讀取單一記錄。
> - 如何預先載入表單，以從資料庫記錄的值。
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

在上一個教學課程中，您已了解如何將記錄新增至資料庫。 在這裡，您將了解如何顯示供編輯的記錄。 在 *電影*頁面上，您將更新`WebGrid`協助程式，因此它會顯示**編輯**每部電影旁邊的連結：

![WebGrid 顯示包括每一部電影的 [編輯] 連結](updating-data/_static/image1.png)

當您按一下 **編輯**連結，它會帶您到不同的頁面中，電影資訊已經在表單中：

![編輯顯示要編輯影片的影片頁面](updating-data/_static/image2.png)

您可以變更任何值。 當您提交的變更時，頁面中的程式碼會更新資料庫，並帶您回到的電影清單。

這部分的程序的方式幾乎完全相同*AddMovie.cshtml*您建立在上一個教學課程中，因此本教學課程中的大部分都很熟悉的頁面。

有數種方式，您可以實作編輯個別的電影的方法。 已選擇所顯示的方式，因為很容易實作且容易理解。

## <a name="adding-an-edit-link-to-the-movie-listing"></a>加入的電影清單的編輯連結

若要開始，您將更新*電影*頁面上，使每個電影清單也會一併包含**編輯**連結。

開啟*Movies.cshtml*檔案。

在頁面的主體中，變更`WebGrid`標記加入資料行。 以下是修改過的標記：

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

新的資料行是如下：

[!code-html[Main](updating-data/samples/sample2.html)]

本專欄的重點是要示範的連結 (`<a>`項目) 的文字會顯示 [編輯]。 我們之後建立時執行網頁時，看起來類似下列的連結是與`id`每一部電影的不同值：

[!code-css[Main](updating-data/samples/sample3.css)]

此連結會叫用名為的頁面*EditMovie*，它會將傳遞查詢字串和`?id=7`至該頁面。

新的資料行的語法看起來可能有點複雜，但這只是因為它會將它們集結數個項目。 每個個別的項目很簡單。 如果您專注於只`<a>`項目，您會看到此標記：

[!code-html[Main](updating-data/samples/sample4.html)]

方格的運作方式的一些背景： 方格會顯示資料列，其中每個資料庫記錄，並在資料庫記錄中顯示每個欄位的資料行。 建構每個格線資料列時，雖然`item`物件包含該資料列的資料庫記錄 （項目）。 這種安排可讓您在程式碼，以取得該資料列的資料。 這是這裡所示： 運算式`item.ID`會取得目前的資料庫項目識別碼值。 若要取得的任何資料庫值 （標題、 內容類型或年） 相同的方式，使用`item.Title`， `item.Genre`，或`item.Year`。

運算式`"~/EditMovie?id=@item.ID`結合的目標 URL 的硬式編碼部分 (`~/EditMovie?id=`) 使用此動態衍生的識別碼。 (您所見`~`運算子在上一個教學課程中，這是一個 ASP.NET 運算子，表示目前的網站根目錄。)

結果是標記的，這一部分的資料行中只會產生類似下列的標記在執行階段：

[!code-xml[Main](updating-data/samples/sample5.xml)]

當然，實際值的`id`會不同，每個資料列。

## <a name="creating-a-custom-display-for-a-grid-column"></a>建立自訂的顯示方格資料行

現在我們回到方格資料行。 三個資料行最初所顯示的格線唯一資料值 （title、 genre 和年）。 您可以指定此顯示的資料庫資料行名稱傳遞&mdash;比方說， `grid.Column("Title")`。

這個新**編輯**連結資料行是不同。 而不指定的資料行名稱，您只要傳遞`format`參數。 此參數可讓您定義標記所`WebGrid`helper 會呈現連同`item`粗體或綠色顯示資料行的資料，或在預定的格式，您想要的值。 比方說，如果您想要顯示為粗體標題時，您可以建立的資料行，如這個範例所示：

[!code-html[Main](updating-data/samples/sample6.html)]

(各種`@`中所見的字元`format`屬性標記的標記和程式碼的值之間的轉換。)

一旦您已了解`format`屬性，會比較容易了解如何新增**編輯**連結資料行放在一起：

[!code-html[Main](updating-data/samples/sample7.html)]

資料行包含*只*的轉譯連結標記，再加上一些資訊 (ID)，從擷取的資料列的資料庫記錄。

> [!TIP]
> 
> **具名的參數和方法的位置參數**
> 
> 許多次時已呼叫方法並傳遞參數給它，您已只被列以逗號分隔的參數值。 以下提供幾個範例：
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> 當您第一次看到此程式碼，但在每個案例中，要將參數傳遞給特定的順序中的方法時，我們沒有提到問題&mdash;也就是定義在該方法之參數的順序。 針對`db.Execute`和`Validation.RequireFields`，如果您混您傳遞值的順序時，您會得到錯誤訊息時執行網頁時或至少一些奇怪的結果。 很明顯地，您必須知道要傳遞的參數順序。 （在 WebMatrix 中，IntelliSense 可以幫助您了解釐清名稱、 類型和參數的順序）。
> 
> 為將值傳遞順序的替代方案，您可以使用*具名參數*。 (為了傳遞參數就是使用*位置參數*。)用於具名參數，您明確地包含參數的名稱傳遞它的值時。 您已使用具名的參數已經數次，在這些教學課程。 例如: 
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> 和
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> 尤其是當方法會接受許多參數，是實用的幾個情況下，具名的參數。 其中一個時，您想要將一個或兩個參數傳遞，但您想要傳遞的值不是參數清單中的第一個位置。 另一種情況時，您想要對您最有意義的順序傳遞的參數，讓您的程式碼更容易閱讀。
> 
> 很明顯地，若要使用具名的參數，您必須知道參數的名稱。 WebMatrix IntelliSense 可以*顯示*您的名稱，但它無法目前他們為您填入。


## <a name="creating-the-edit-page"></a>建立 [編輯] 頁面

現在您可以建立*EditMovie*頁面。 當使用者按一下**編輯**連結，他們就可以得到此頁面上。

建立名為的頁面*EditMovie.cshtml*和取代功能的檔案，以下列標記：

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

此標記和程式碼是類似於您在*AddMovie*頁面。 沒有有些許不同的 [提交] 按鈕的文字。 如同*AddMovie*頁面上，有`Html.ValidationSummary`如果有的話，會顯示驗證錯誤的呼叫。 此時，我們會留下任何呼叫`Validation.Message`，因為錯誤會顯示在驗證摘要。 如先前的教學課程中所述，您可以使用各種組合中的驗證摘要和個別的錯誤訊息。

請注意再次`method`的屬性`<form>`元素設定為`post`。 如同*AddMovie.cshtml*  頁面上，此頁面可變更資料庫。 因此，此表單應該執行`POST`作業。 (如需詳細資訊之間的差異`GET`並`POST`作業，請參閱[GET、 POST 和 HTTP 動詞命令安全性](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety)HTML 表單的教學課程中的資訊看板。)

如您在先前的教學課程中所見`value`文字方塊中的屬性 Razor 程式碼以它們預先載入設定。 此時，不過，您使用變數，例如`title`並`genre`而不是該工作`Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

為之前，此標記會預先載入的電影值的文字方塊值。 您會看到它為什麼很方便地這次使用變數，而不是使用稍後`Request`物件。

另外還有`<input type="hidden">`此頁面上的項目。 這個項目而不讓它顯示在頁面上儲存的電影識別碼。 識別碼一開始傳遞至頁面所使用的查詢字串值 (`?id=7`或類似的 URL 中)。 藉由將的識別碼值放入隱藏的欄位中，您可以確保它可供當提交表單時，即使您不再需要存取的頁面以叫用的原始 URL。

不同於*AddMovie*頁面上的程式碼*EditMovie*頁面有兩個不同的功能。 第一個函式是第一次顯示網頁時 (和*只*然後)，此程式碼從查詢字串取得的電影識別碼。 接著，程式碼會使用識別碼來讀取對應影片移出資料庫和顯示 （預先載入） 它的文字方塊中。

第二個函式是，當使用者按下**送出變更** 按鈕，程式碼會讀取這些值的文字方塊中，並驗證它們。 程式碼也會對新的值來更新資料庫項目。 如您所見的就像是加入一筆記錄，這項技術*AddMovie*。

## <a name="adding-code-to-read-a-single-movie"></a>新增程式碼來讀取單一影片

若要執行第一個函式，請將此程式碼加入頁面頂端：

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

大部分的這段程式碼會啟動在區塊內`if(!IsPost)`。 `!`運算子代表"not，"所以運算式表示*如果此要求不是 post 提交*，這是間接的一種說法*此要求是否已執行此頁面第一次*。 如先前所述，應該執行此程式碼*只*此頁面會執行第一次。 如果您沒有使用中的程式碼`if(!IsPost)`，它會每次頁面叫用時，是否在第一次執行，或按一下 [回應] 按鈕。

請注意，程式碼包含`else`封鎖此時間。 我們說過當我們引進了`if`區塊，有時您想要執行替代程式碼，如果您要測試的條件不是，則為 true。 這種情況如下。 如果符合條件 （亦即，如果傳遞至頁面的識別碼為 [確定]），您會從資料庫讀取資料列。 不過，如果條件未通過，`else`區塊會執行，並將程式碼設定錯誤訊息。

## <a name="validating-a-value-passed-to-the-page"></a>驗證傳遞至頁面的值

此程式碼使用`Request.QueryString["id"]`取得傳遞至頁面的識別碼。 程式碼可確保值實際上傳遞做為識別碼。 如果不傳遞任何值，此程式碼會設定驗證錯誤。

此程式碼會示範不同的方法來驗證資訊。 在上一個教學課程中，您用過`Validation`協助程式。 註冊要驗證，欄位和 ASP.NET 自動未驗證，並使用顯示錯誤`Html.ValidationMessage`和`Html.ValidationSummary`。 在此情況下，不過，您不是真正驗證使用者輸入。 相反地，您要驗證已傳遞至頁面上，從其他位置的值。 `Validation`協助程式為您不會進行。

因此，您檢查的值，藉由測試它與`if(!Request.QueryString["ID"].IsEmpty()`)。 如果沒有問題，您可以使用來顯示錯誤`Html.ValidationSummary`，當您使用`Validation`協助程式。 若要這樣做，請呼叫`Validation.AddFormError`並將它傳遞要顯示的訊息。 `Validation.AddFormError` 是一種內建方法，可讓您定義自訂的訊息，您已經熟悉的驗證系統相連結。 （稍後在本教學課程將討論如何進行此驗證程序更健全。）

確定您影片的識別碼之後, 程式碼會讀取資料庫中，尋找只有單一資料庫的項目。 (您可能已經注意到一般資料庫作業模式： 開啟資料庫、 定義 SQL 陳述式，並執行陳述式。)此時，SQL`Select`陳述式包含`WHERE ID = @0`。 因為識別碼是唯一的可傳回只有一筆記錄。

查詢透過執行`db.QuerySingle`(不`db.Query`，因為您使用的電影清單)，並將程式碼將放入結果`row`變數。 名稱`row`是任意的; 您可以隨意命名的變數。 頂端初始化的變數則會填入電影詳細資料，這些值可以顯示的文字方塊中。

## <a name="testing-the-edit-page-so-far"></a>測試 [編輯] 頁面 （目前）

如果您想要測試您的頁面，執行*電影*現在頁面，然後按一下**編輯**旁邊任何影片的連結。 您會看到*EditMovie*詳細資料已填入資料頁面您選取的電影：

![編輯顯示要編輯影片的影片頁面](updating-data/_static/image3.png)

請注意，頁面的 URL 會包含類似`?id=10`（或其他數字）。 到目前為止，測試**編輯**中的連結*電影*頁面上的工作，您的頁面從查詢字串中，讀取識別碼，而且資料庫取得單一影片記錄查詢運作。

您可以變更的電影資訊，但當您按一下時，會發生任何事**送出變更**。

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>加入程式碼以使用使用者的變更來更新影片

在  *EditMovie.cshtml*檔案中，若要實作 （儲存變更） 的第二個函式，新增下列程式碼的右括號之內`@`區塊。 (如果您不確定確實要放置程式碼的位置，您可以看看[完整的程式碼清單編輯影片頁面](#Complete_Page_Listing_for_EditMovie)出現在本教學課程結尾處。)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

同樣地，此標記和程式碼是中的程式碼類似*AddMovie*。 程式碼位於`if(IsPost)`封鎖，因為只有在使用者按一下時，才執行此程式碼**送出變更** 按鈕&mdash;也就是當 （並時，才） 已張貼的表單。 在此情況下，您不使用類似的測試`if(IsPost && Validation.IsValid())`— 也就無法結合這兩項測試使用 and。 在此頁面中，您先判斷是否有表單提交 (`if(IsPost)`)，然後只再登錄進行驗證的欄位。 然後您可以測試驗證結果 (`if(Validation.IsValid()`)。 流程會稍有不同*AddMovie.cshtml*  頁面上，但效果是一樣。

使用取得的文字方塊中的值`Request.Form["title"]`和其他類似的程式碼`<input>`項目。 請注意，此時，程式碼會取得電影識別碼從隱藏的欄位 (`<input type="hidden">`)。 當頁面執行第一次時，程式碼會取得從查詢字串的識別碼。 您從隱藏的欄位，藉此確定您取得識別碼的影片，原本顯示，以防之後以某種方式修改查詢字串取得的值。

之間非常重要的差異*AddMovie*程式碼，此程式碼是在這段程式碼中使用 SQL`Update`陳述式來取代`Insert Into`陳述式。 下列範例示範的 SQL 語法`Update`陳述式：

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

您可以依任何順序指定的任何資料行，而且不一定需要更新期間的每個資料行`Update`作業。 (因為，實際上會為新的記錄，來儲存記錄，而且不允許的無法更新識別碼本身，`Update`作業。)

> [!NOTE] 
> 
> **重要**`Where`識別碼子句是很重要，因為這是資料庫如何知道哪一個資料庫記錄您想要更新。 如果您離開`Where`子句中，資料庫會更新*每個*記錄資料庫中。 在大部分情況下，是嚴重損壞。


在程式碼中，若要更新的值會傳遞至 SQL 陳述式使用預留位置。 若要重複我們剛才說過： 基於安全性理由*只*使用預留位置來將值傳遞至 SQL 陳述式。

程式碼會使用後`db.Execute`執行`Update`陳述式，它會重新導向回到清單頁面上，您可以在其中看到所做的變更。

> [!TIP] 
> 
> **不同的 SQL 陳述式，不同的方法**
> 
> 您可能已經注意到，您會使用稍微不同的方法來執行不同的 SQL 陳述式。 若要執行`Select`可能查詢會傳回多筆記錄，您使用`Query`方法。 若要執行`Select`您知道的查詢會傳回只有一個資料庫項目，則使用`QuerySingle`方法。 若要執行的命令，進行變更，但不傳回資料庫項目，您使用`Execute`方法。
> 
> 您必須有不同的方法，因為每個會傳回不同的結果，如您所見已經在之間的差異`Query`和`QuerySingle`。 (`Execute`方法實際上會傳回值也&mdash;亦即已受到該命令的資料庫資料列數目&mdash;您已被忽略，到目前為止，但。)
> 
> 當然，`Query`方法可能會傳回只有一個資料庫的資料列。 不過，ASP.NET 一律會將結果`Query`為集合的方法。 即使方法傳回只有一個資料列，您必須從集合擷取該單一資料列。 因此，在情況下，您*知道*您會得到只有一個資料列，更方便使用的位元`QuerySingle`。
> 
> 有幾個其他執行特定類型的資料庫作業的方法。 您可以找到資料庫中的方法清單[ASP.NET Web Pages API 的快速參考](../../api-reference/asp-net-web-pages-api-reference.md#Data)。


## <a name="making-validation-for-the-id-more-robust"></a>讓穩固，識別碼越多的驗證

初次執行網頁時，您取得的電影識別碼從查詢字串，以便您即可從資料庫取得該影片。 您很確定實際發生去尋找，使用此程式碼的值：

[!code-csharp[Main](updating-data/samples/sample13.cs)]

使用此代碼可確保當使用者到達*EditMovies*頁面上，必須先選取電影*電影* 頁面上，頁面會顯示使用者易記錯誤訊息。 （否則使用者會看到錯誤，可能會可能混淆。）

不過，這項驗證不是非常強大。 只有這些錯誤也可能會叫用頁面：

- 識別碼不是數字。 例如，無法之類的 URL 叫用頁面`http://localhost:nnnnn/EditMovie?id=abc`。
- 識別碼是數字，但它會參考不存在的影片 (例如`http://localhost:nnnnn/EditMovie?id=100934`)。

如果您想知道，請參閱錯誤所造成的這些 Url 當中，執行*電影*頁面。 選取要編輯的影片，然後變更 的 URL *EditMovie*頁面，即可為 URL，內含字母識別碼或不存在電影識別碼。

因此您怎麼辦？ 第一次的修正方法是確保，不只識別碼傳遞至頁面上，但識別碼是整數。 變更的程式碼`!IsPost`測試此範例所示：

[!code-csharp[Main](updating-data/samples/sample14.cs)]

您已新增第二個條件`IsEmpty`測試，與連結`&&`(邏輯 AND):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

從大概還記得[ASP.NET Web Pages 程式設計簡介](../introducing-razor-syntax-c.md)等方法的教學課程`AsBool``AsInt`將字元字串轉換成其他資料類型。 `IsInt`方法 (和其他項目，例如`IsBool`和`IsDateTime`) 類似。 不過，僅限測試是否您*可以*轉換的字串，而不實際執行轉換。 這裡您基本上就表示*如果查詢字串值可以轉換成整數...*.

不存在的電影會尋找潛在的問題。 取得影片的程式碼看起來像此程式碼：

[!code-csharp[Main](updating-data/samples/sample16.cs)]

如果您傳遞`movieId`值加入`QuerySingle`未對應到實際電影的方法，沒有傳回任何內容和陳述式，遵循 (比方說， `title=row.Title`) 會導致錯誤。

一次很容易解決。 如果`db.QuerySingle`方法會傳回任何結果，`row`變數將會是 null。 因此您可以檢查是否`row`變數為 null，然後再從它取得的值。 下列程式碼加入`if`取得的值的陳述式前後的區塊`row`物件：

[!code-csharp[Main](updating-data/samples/sample17.cs)]

這兩個額外的驗證測試，網頁變得更確實。 完整程式碼`!IsPost`分支現在看起來像此範例中：

[!code-csharp[Main](updating-data/samples/sample18.cs)]

我們會請注意一次，這項工作的好用途`else`區塊。 如果測試不成功，`else`區塊設定錯誤訊息。

## <a name="adding-a-link-to-return-to-the-movies-page"></a>新增傳回至 Movies 頁面的連結

最後也很有幫助的詳細資料是要將連結新增回到*電影*頁面。 在一般的事件流程中，使用者將會在啟動*電影*頁面，然後按一下**編輯**連結。 予以更正，使*EditMovie* ] 頁面上，他們可以在此編輯影片，然後按一下 [[] 按鈕。 程式碼會處理變更之後，它會重新導向回到*電影*頁面。

但是：

- 使用者不可能會決定在變更任何項目。
- 使用者可能沒有先按一下收到此網頁**編輯**連結*電影*頁面。

無論如何，您會想要讓他們可以輕鬆將返回主要清單。 它很容易解決&mdash;後方結尾加入下列標記`</form>`在標記中的標記：

[!code-html[Main](updating-data/samples/sample19.html)]

此標記會使用的相同語法`<a>`您在其他地方看到的項目。 URL 包含`~`來表示 「 根網站的 」。

## <a name="testing-the-movie-update-process"></a>測試影片的更新程序

現在您可以測試。 執行*電影*頁面，然後按一下**編輯**電影旁邊。 當*EditMovie*頁面隨即出現，變更至影片，然後按一下**送出變更**。 當電影清單隨即出現時，請確定您的變更，會顯示。

若要確定驗證是否運作，請按一下**編輯**另一部電影。 當您進入*EditMovie*頁面上，清除**內容類型**欄位 (或**年** 欄位中，或兩者)，並嘗試提交您的變更。 如您所預期，您會看到錯誤：

![編輯影片頁面顯示驗證錯誤](updating-data/_static/image4.png)

按一下 **傳回的電影清單**連結，以放棄變更並返回*電影*頁面。

## <a name="coming-up-next"></a>接下來的下一步

在下一個教學課程中，您會看到如何刪除電影錄製。

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>（更新的編輯連結） 的電影頁面的完整清單

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>完成編輯影片頁面的頁面清單

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>其他資源

- [使用 Razor 語法的 ASP.NET Web 程式設計簡介](../../getting-started/introducing-razor-syntax-c.md)
- [SQL UPDATE 陳述式](http://www.w3schools.com/sql/sql_update.asp)W3Schools 站台上

> [!div class="step-by-step"]
> [上一頁](entering-data.md)
> [下一頁](deleting-data.md)
