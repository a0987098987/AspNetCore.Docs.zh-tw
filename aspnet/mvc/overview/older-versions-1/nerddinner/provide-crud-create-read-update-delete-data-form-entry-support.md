---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: 提供 CRUD （建立、 讀取、 更新、 刪除） 資料表單項目支援 |Microsoft Docs
author: microsoft
description: 步驟 5 會示範如何使用我們 DinnersController 類別的進一步所啟用的編輯、 建立及刪除 Dinners 與其，也支援。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 821684c0753967fc1a693b061d5d539951cd7c23
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388804"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>提供 CRUD （建立、 讀取、 更新、 刪除） 資料表單項目支援
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 5 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 5 會示範如何使用我們 DinnersController 類別的進一步所啟用的編輯、 建立及刪除 Dinners 與其，也支援。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner 步驟 5： 建立、 更新及刪除表單案例

我們已導入控制器和檢視，並涵蓋如何使用它們來在站台上實作 Dinners 經驗的清單/詳細資料。 下一步將能夠進一步採取 DinnersController 類別，並且啟用編輯、 建立和刪除 Dinners 與其，也支援。

### <a name="urls-handled-by-dinnerscontroller"></a>由 DinnersController 的 Url

實作支援兩個 Url 的 DinnersController 先前加入動作方法： */Dinners*並 */Dinners/詳細資料 / [id]*。

| **URL** | **VERB** | **目的** |
| --- | --- | --- |
| */Dinners/* | GET | 顯示即將推出的 dinners HTML 清單。 |
| */Dinners/Details/[id]* | GET | 顯示有關特定 dinner 詳細資料。 |

我們現在會將動作方法，實作三個額外的 Url: <em>/Dinners/編輯 / [id]、 Dinners/建立]</em>並<em>/Dinners/Delete / [id]</em>。 這些 Url 可讓您編輯現有的 Dinners，建立新的 Dinners 和刪除 Dinners。

我們即將支援使用這些新的 Url 的 HTTP GET 與 HTTP POST 動詞命令互動。 這些 url 的 HTTP GET 要求會顯示初始的 HTML 檢視的資料 （表單填入 Dinner 資料，在 編輯 的情況下，在 「 建立 」 的情況下的空白表單和刪除確認 畫面，在 「 刪除 」 的情況下）。 這些 url 的 HTTP POST 要求將會儲存/更新/刪除 Dinner 資料中我們 DinnerRepository （和至資料庫）。

| **URL** | **VERB** | **目的** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | 顯示可編輯的 HTML 表單 Dinner 資料來擴展。 |
| POST | 將表單變更儲存到資料庫的特定晚餐。 |
| */Dinners/Create* | GET | 顯示空的 HTML 表單，好讓使用者定義新 Dinners。 |
| POST | 建立新的 Dinner，並將它儲存在資料庫中。 |
| */Dinners/Delete/[id]* | GET | 顯示刪除確認 畫面。 |
| POST | 從資料庫刪除指定的 dinner。 |

### <a name="edit-support"></a>編輯支援

讓我們先實作 [編輯] 案例。

#### <a name="the-http-get-edit-action-method"></a>HTTP GET Edit 動作方法

我們一開始是實作 HTTP"GET"方法的行為我們編輯動作。 這個方法會叫用時機 */Dinners/編輯 / [id]* 要求 URL。 我們的實作會看起來像：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

上述程式碼會使用 DinnerRepository 擷取 Dinner 物件。 然後，它會呈現使用 Dinner 物件的檢視範本。 因為我們尚未明確範本名稱傳遞給*View()* 協助程式方法，它會使用慣例為基礎的預設路徑來解析檢視範本： /Views/Dinners/Edit.aspx。

我們現在來建立此檢視範本。 我們將編輯方法內按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令來這麼做：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

在 [新增檢視] 對話方塊中，我們將指出我們要將 Dinner 物件傳遞至我們的檢視範本，為其模型，並選擇 [編輯] 的範本自動 scaffold:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners 」 目錄內就讓我們加入新的"Edit.aspx 」 檢視範本檔案。 它也會開啟新的"Edit.aspx 」 檢視範本內的程式碼編輯器 – 初始 「 編輯 」 scaffold 實作類似下方所填入：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

讓我們變更幾個預設的 [編輯] scaffold 產生，並更新有以下內容 （這會移除數個我們不想要公開 （expose） 的屬性） 的 [編輯] 檢視樣板：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

當我們執行的應用程式和要求 *"/ Dinners/編輯/1"* 我們會看到下列頁面的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

我們的檢視所產生的 HTML 標記如下所示。 它是標準 HTML – 具有&lt;表單&gt;項目，執行 HTTP POST */Dinners/Edit/1* URL 時 儲存&lt;輸入類型 ="提交"/&gt;推入 按鈕。 在 HTML &lt;type ="text"/&gt;項目已針對每個可編輯屬性的輸出：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() 和 Html.TextBox() Html 協助程式方法

我們 「 Edit.aspx 」 檢視範本使用的數個 「 Html 協助程式 」 的方法： Html.ValidationSummary()、 Html.BeginForm()、 Html.TextBox() 和 Html.ValidationMessage()。 除了為我們產生 HTML 標記，這些 helper 方法會提供內建的錯誤處理和驗證支援。

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() helper 方法

Html.BeginForm() helper 方法是什麼輸出 HTML&lt;表單&gt;我們標記中的項目。 在我們 Edit.aspx 檢視範本中，您會發現，我們會將它套用 C#"using"陳述式時使用此方法。 開啟的大括號表示開頭&lt;表單&gt;內容和右大括號就會指出結尾&lt;/形成&gt;項目：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

或者，如果您找到的"using"陳述式方法不太適用於這類案例，您可以使用 Html.BeginForm() 和 Html.EndForm() 的組合 （這會執行相同的動作）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

不含任何參數呼叫 Html.BeginForm() 會讓它輸出到目前要求的 URL 執行 HTTP POST 表單項目。 也就是為什麼我們編輯檢視就會產生*&lt;形成動作 ="/ Dinners/編輯/1"的方法 ="post"&gt;* 項目。 我們可能具有或者傳遞明確參數至 Html.BeginForm() 如果我們想要張貼到不同的 URL。

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() helper 方法

我們 Edit.aspx 檢視使用 Html.TextBox() helper 方法來輸出&lt;type ="text"/&gt;項目：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

上述的 Html.TextBox() 方法接受單一參數-這要用來指定這兩個識別碼/名稱的屬性&lt;type ="text"/&gt;輸出，以及填入文字方塊值從模型屬性的項目。 比方說，我們傳遞到 [編輯] 檢視的 Dinner 物件具有 「.NET Futures"的"Title"屬性值，並因此我們 Html.TextBox("Title") 方法呼叫輸出： *&lt;輸入的識別碼 ="Title"name ="Title"type ="text"value =".NET Futures"/&gt;*.

或者，我們可以使用第一個 Html.TextBox() 參數來指定識別碼/名稱的項目，並明確地傳遞在要做為第二個參數的值：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

通常我們會希望執行自訂的格式是輸出的值。 String.format （) 內建於.NET 靜態方法可用於這些案例。 我們 Edit.aspx 檢視範本會使用此來格式化 EventDate 值 （這是 DateTime 類型的），讓它不會顯示時間 （秒）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() 第三個參數 （選擇性） 可以用於輸出其他的 HTML 屬性。 程式碼-下列程式碼片段示範如何轉譯的其他大小 ="30"的屬性以及一個 class ="mycssclass 」 屬性&lt;輸入類型 ="text"/&gt;項目。 請注意如何我們會逸出的類別屬性使用名稱"@" character because "類別 」 是保留的關鍵字在 C# 中：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>實作 HTTP POST Edit 動作方法

我們現在有實作我們編輯動作方法的 HTTP GET 版本。 當使用者要求 */Dinners/Edit/1*收到如下所示的 HTML 網頁的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

按下 [儲存] 按鈕會造成表單 post */Dinners/Edit/1* URL，並送出 HTML&lt;輸入&gt;形成使用 HTTP POST 動詞命令的值。 讓我們現在實作 HTTP POST 方法的行為我們編輯動作，這將會處理儲存 Dinner。

首先我們加入我們 DinnersController 對 「 AcceptVerbs"屬性，指出它會處理 HTTP POST 的案例中的多載的 「 編輯 」 動作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

當 [AcceptVerbs] 屬性套用至多載的動作方法時，ASP.NET MVC 會自動處理分派到適當的動作方法會根據傳入的 HTTP 指令動詞的要求。 HTTP POST 要求<em>/Dinners/編輯 / [id]</em> Url 將會移至上述的 Edit 方法，而所有其他 HTTP 動詞命令要求<em>/Dinners/編輯 / [id]</em>Url 將會移至第一個編輯方法我們實作了 （這並未不具有 [AcceptVerbs] 屬性）。

| **透過 HTTP 動詞命令為什麼區分端主題： 嗎？** |
| --- |
| 您可能會要求 – 我們為何使用單一的 URL 和區隔其行為透過 HTTP 指令動詞嗎？ 為什麼不只需要兩個不同的 Url，以處理載入及儲存所做的變更嗎？ 例如： /Dinners/編輯 / [id] 來顯示初始表單和 /Dinners/Save / [id] 處理表單張貼來儲存它嗎？ 發佈兩個不同的 Url 與缺點是，在我們張貼到 /Dinners/Save/2，，然後需要重新顯示 HTML 格式，因為發生輸入錯誤的情況下，使用者最後會擁有 Dinners/Save/2 URL 在其瀏覽器的網址列 （因為那是URL 格式張貼至）。 如果使用者設定為書籤 redisplayed 的本頁一致瀏覽器的我的最愛清單，或複製/貼上 URL 和電子郵件傳送給朋友，它們將會儲存不會在未來運作 （因為該 URL 會因張貼值而定） 的 URL。 藉由公開單一的 URL (例如： /Dinners/Edit/[id])，區分的 HTTP 指令動詞的處理是不是安全的書籤 [編輯] 頁面及 （或） 將 URL 傳送給其他人的使用者。 |

#### <a name="retrieving-form-post-values"></a>擷取表單張貼值

有多種方式可以存取張貼在我們的 HTTP POST 「 編輯 」 方法的表單參數。 一個簡單的作法是只控制站的基底類別上使用要求屬性來存取表單集合，並直接擷取已張貼的值：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

我們將新增錯誤處理邏輯之後，特別是，是，有一點繁瑣，上述的方法。

此案例中是利用內建之*UpdateModel()* 控制器的基底類別上的協助程式方法。 它支援更新的屬性，我們將它使用的連入的表單參數傳遞的物件。 使用反映來判斷，在物件上的屬性名稱，然後自動轉換並指派值給變數根據用戶端所提交的輸入值。

我們可以使用 UpdateModel() 方法，以簡化我們 HTTP-POST 編輯動作使用此程式碼：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

我們現在可以瀏覽 */Dinners/Edit/1* URL，以及變更我們 Dinner 標題：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

當我們按一下 [儲存] 按鈕時，我們會執行表單張貼至我們的編輯動作，並更新後的值會保存在資料庫中。 我們會再重新導向至詳細資料 URL 如 Dinner （這會顯示最近儲存的值）：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>處理編輯錯誤

我們目前 HTTP POST 實作可以正常運作 – 除非有錯誤。

當使用者編輯表單的錯誤，我們必須確定將引導他們要修正此問題的資訊的錯誤訊息會重新顯示表單。 這包括使用者要將張貼輸入不正確的情況下 (例如： 的格式不正確的日期字串)，如情況以及輸入的格式有效，但是沒有商務規則違規。 何時會發生錯誤，格式應該保留原先輸入，讓他們不必手動將其變更重新填滿使用者輸入的資料。 表單已成功完成之前，應該會視需要多次重複此程序。

ASP.NET MVC 包含一些不錯的內建功能可簡化錯誤處理和表單重新顯示。 若要查看作用中的這些功能，讓我們使用下列程式碼更新我們的編輯動作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

上述程式碼是類似於我們先前的實作 –，不同之處在於我們現在正在包裝 try/catch 區塊中的錯誤處理有關的工作。 如果發生例外狀況時呼叫 UpdateModel()，或當我們嘗試並儲存 DinnerRepository （這將會引發例外狀況，如果我們嘗試儲存 Dinner 物件是無效，因為我們的模型中的規則違規），我們 catch 錯誤處理區塊就會將執行。 在其中我們使用迴圈處理任何存在於 Dinner 物件的規則違規，並將其新增的 ModelState 物件 （我們將討論）。 然後，我們重新顯示的檢視。

若要查看此運作讓我們重新執行應用程式，請編輯 Dinner，並將它變更為有空白的標題，EventDate 的"BOGUS 」，和美國國家/地區值使用英國電話號碼。 當我們按下 [儲存] 按鈕時我們編輯的 HTTP POST 方法將無法儲存 Dinner （因為沒有錯誤），將會重新顯示表單：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

我們的應用程式有適當錯誤體驗。 與無效輸入的文字項目會以紅色反白顯示，並驗證錯誤訊息會顯示其相關的使用者。 表單也會保留原本輸入的使用者 – 輸入的資料，讓他們不必重新填滿任何項目。

如何執行，您可能會問，這會發生？ 如何沒有標題、 EventDate 和 ContactPhone 文字方塊以紅色強調本身和知道輸出原本輸入的使用者值？ 以及如何沒有錯誤訊息顯示在頂端的清單中？ 好消息是，這不會發生魔術一樣-而是這是因為我們使用一些內建的 ASP.NET MVC 功能可簡化輸入的驗證和錯誤處理案例。

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>了解 ModelState 及驗證 HTML 協助程式方法

控制器類別有 「 ModelState"的屬性集合可提供一個方法來指示要傳遞至檢視模型物件與有錯誤。 ModelState 集合內的錯誤項目識別與問題的模型屬性的名稱 (例如:"Title"、"EventDate 」 或 「 ContactPhone")，並允許指定的人類易錯誤訊息 (例如: 「 標題 」 所需)。

*UpdateModel()* helper 方法會自動填入 ModelState 集合，嘗試將表單的值指派給模型物件上的屬性時遇到錯誤時。 比方說，我們的 Dinner 物件 EventDate 屬性是 DateTime 類型。 當 UpdateModel() 方法無法指派給它的字串值"BOGUS 」，在上述案例中時，UpdateModel() 方法會加入至 ModelState 集合的指派錯誤，指出項目發生與該屬性。

開發人員也可以撰寫程式碼，以明確地加入錯誤 ModelState 集合的項目，像我們下面我們"catch"錯誤處理區塊內，這將 ModelState 集合填入基礎中作用中的規則違規的項目晚餐物件：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>與 ModelState 的 html 協助程式整合

轉譯輸出時，HTML 協助程式之類的方法-Html.TextBox()-請檢查 ModelState 集合。 如果發生錯誤的項目存在，它們會呈現使用者輸入的值和 CSS 錯誤類別。

比方說，在 [編輯] 檢視中，我們會使用 Html.TextBox() helper 方法來呈現我們 Dinner 物件的 EventDate:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

當檢視就會呈現在錯誤狀況時，Html.TextBox() 方法會檢查 ModelState 集合，以查看是否發生任何 「 EventDate"我們 Dinner 物件屬性相關聯的錯誤。 它判斷發生錯誤時它呈現送出的使用者輸入 ("BOGUS 」) 的值，並已新增至 css 錯誤類別&lt;type ="textbox"/&gt;它產生的標記：

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

您可以自訂的 css 錯誤類別，以便看不過您想要的外觀。 預設 CSS 錯誤-「 輸入-驗證-錯誤 」 – 類別定義於*\content\site.css*樣式表，且看起來如下所示：

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

這個 CSS 規則會導致我們無效的輸入項目以反白顯示類似如下：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() Helper 方法

Html.ValidationMessage() helper 方法可以用於輸出的特定模型屬性相關聯的 ModelState 錯誤訊息：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

上述程式碼會輸出：  *&lt;/><span class ="欄位的驗證錯誤的 「&gt; 'BOGUS' 的值無效 &lt; /span>&gt;*

Html.ValidationMessage() 協助程式方法也支援可讓開發人員覆寫會顯示錯誤文字訊息的第二個參數：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

上述程式碼會輸出：  <em>&lt;/><span class ="欄位的驗證錯誤的 「&gt;\*&lt;/span>&gt;</em>而不是針對有錯誤時，才會進行預設錯誤文字EventDate 屬性。

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() Helper 方法

Html.ValidationSummary() 協助程式方法可以用來呈現摘要錯誤訊息，附帶&lt;ul&gt;&lt;li /&gt;&lt;/u l&gt;之所有詳細錯誤訊息清單中ModelState 集合：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Html.ValidationSummary() helper 方法會採用選擇性的字串參數 – 定義詳細的錯誤清單上方顯示的摘要錯誤訊息：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

CSS （選擇性） 可用來覆寫 錯誤清單的樣貌。

#### <a name="using-a-addruleviolations-helper-method"></a>使用 AddRuleViolations 協助程式方法

我們最初的 HTTP POST 編輯實作會使用 foreach 陳述式，它的 catch 區塊內使用迴圈處理 Dinner 物件的規則違規，並將其新增至控制器的 ModelState 集合：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

我們可以讓此程式碼，加上"ControllerHelpers 」 的一些更乾淨 NerdDinner 專案中，類別並實作將協助程式方法新增至 ASP.NET MVC ModelStateDictionary 類別"AddRuleViolations 」 延伸模組方法內。 這個擴充方法可以將封裝以填入一份 RuleViolation 錯誤 ModelStateDictionary 所需的邏輯：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

然後，我們可以更新我們編輯 HTTP POST 動作方法，若要使用這個擴充方法來填入 ModelState 我們 Dinner 規則違規集合。

#### <a name="complete-edit-action-method-implementations"></a>完成編輯動作方法的實作

下列程式碼會實作所有控制站所需的邏輯我們編輯的案例：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

我們的編輯實作不賴的一點是我們的控制器類別和我們的檢視範本都不具有任何項目知道特定的驗證或強制執行我們的 Dinner 模型的商務規則。 我們可以新增其他規則至我們的模型在未來，並不需要變更任何程式碼到我們的控制器或檢視，使其受到支援。 這可讓我們輕鬆發展我們應用程式的需求在未來最少程式碼變更的彈性。

### <a name="create-support"></a>建立支援

我們已經完成實作 DinnersController 類別的 [編輯] 行為。 讓我們現在移至它-這可讓使用者加入新的 Dinners 實作的 「 建立 」 支援。

#### <a name="the-http-get-create-action-method"></a>HTTP GET 建立動作方法

藉由實作 HTTP"GET"的行為，我們一開始我們建立動作方法。 會呼叫這個方法，當有人瀏覽*Dinners/建立*URL。 我們的實作如下所示：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

上述程式碼會建立新的 Dinner 物件，並指派其 EventDate; 屬性是在未來一週。 然後，它會呈現新 Dinner 物件為基礎的檢視。 因為我們尚未明確名稱傳遞給*View()* 協助程式方法，它會使用慣例為基礎的預設路徑來解析檢視範本： /Views/Dinners/Create.aspx。

我們現在來建立此檢視範本。 我們可以在建立動作方法內按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令來這麼做。 在 [新增檢視] 對話方塊中，我們會表示我們會將 Dinner 物件傳遞至檢視範本，並選擇 [建立] 範本自動 scaffold:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

當我們按一下 [新增] 按鈕時，Visual Studio 會將新的 scaffold 型"Create.aspx 」 檢視儲存"\Views\Dinners"目錄，並在 IDE 中開啟它：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

讓我們變更幾個預設 「 建立 」 scaffold 檔案所產生，並修改設定如下所示：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

現在，當我們執行我們的應用程式和存取並 *"/ Dinners/建立 「* ，它會從我們建立動作實作轉譯類似如下的 UI 的瀏覽器內的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>實作 HTTP POST 建立動作方法

我們已實作我們建立動作方法的 HTTP GET 版本。 當使用者按一下 [儲存] 按鈕會執行的表單 post *Dinners/Create* URL，並送出 HTML&lt;輸入&gt;形成使用 HTTP POST 動詞命令的值。

讓我們現在實作的 HTTP POST 行為我們建立動作方法。 首先我們加入我們 DinnersController 對 「 AcceptVerbs"屬性，指出它會處理 HTTP POST 的案例中的多載的 [建立] 動作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

有多種方式可以存取已張貼的表單參數我們啟用 HTTP POST 「 建立 」 方法中。

其中一個方法是建立新的 Dinner 物件，然後使用*UpdateModel()* （就像我們一樣編輯動作） 的協助程式方法來填入表單的值。 然後，我們可以將它新增至我們 DinnerRepository、 將它保存到資料庫，並將使用者重新導向至我們的詳細資料動作，以顯示新建立的 Dinner 使用下列程式碼：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

或者，我們可以使用的方法有我們採用 Dinner 物件作為方法參數的 create （） 動作方法。 ASP.NET MVC 會再自動物件具現化新 Dinner 我們、 填入使用表單的輸入，其屬性並將它傳遞至我們的動作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

我們上面的動作方法會驗證，Dinner 物件已成功填入表單張貼值檢查 ModelState.IsValid 屬性。 這會傳回 false，如果有輸入轉換問題 (例如： EventDate 屬性 「 BOGUS"的字串)，以及是否有任何問題我們動作方法會重新顯示表單。

如果輸入的值有效，然後動作方法會嘗試加入，並將新的 Dinner 儲存至 DinnerRepository。 它會包裝在 try/catch 區塊內的這項工作，並重新顯示表單，如果有任何商務規則違規 （這會導致引發例外狀況的 dinnerRepository.Save() 方法）。

若要查看此錯誤處理中的行為，我們可以要求*Dinners/建立*URL，並填入新的 Dinner 相關詳細資料。 輸入不正確或值將會導致建立表單，會重新顯示具有反白顯示類似如下的錯誤：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

請注意，我們建立表單的確切相同驗證和商務規則做為我們的編輯表單中的特殊接受。 這是因為我們驗證和商務規則已定義在模型中，而不內嵌於 UI 或應用程式的控制站。 這表示我們可以稍後變更/改進我們的驗證，或商務規則，在單一放置，並將它們套用在我們的應用程式。 我們不會變更任何程式碼內我們編輯或建立會自動接受任何新的規則或修改現有的動作方法。

當我們修正的輸入的值，然後按一下 [儲存] 按鈕時同樣地，DinnerRepository 我們新增會成功，而且新 Dinner 會加入至資料庫。 我們將接著會重新導向至 */Dinners/詳細資料 / [id]* URL – 我們呈現含有新建立的 Dinner 相關詳細資訊：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>刪除支援

讓我們現在新增 「 刪除 」 支援我們 DinnersController。

#### <a name="the-http-get-delete-action-method"></a>HTTP GET Delete 動作方法

我們開始要先實作 HTTP GET 方法的行為我們刪除動作。 當有人瀏覽時，會取得呼叫這個方法 */Dinners/Delete / [id]* URL。 以下是實作：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

擷取要刪除 Dinner 嘗試的動作方法。 如果 Dinner 已存在，則轉譯的檢視會根據 Dinner 物件。 如果物件不存在 （或已刪除） 它傳回"NotFound"會呈現的檢視會檢視我們稍早為我們的 [詳細資料] 動作方法建立的範本。

我們可以建立 「 刪除 」 檢視範本內的 Delete 動作方法上按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令。 在 [新增檢視] 對話方塊中，我們將指出我們要將 Dinner 物件傳遞至我們的檢視範本，為其模型，並選擇建立空白的範本：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

當我們按一下 [新增] 按鈕時，Visual Studio 將新的 「 Delete.aspx 」 檢視範本檔案為我們在我們的 「 \Views\Dinners"目錄。 我們會將一些 HTML 和程式碼新增至範本中，實作刪除確認畫面如下所示：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

上述程式碼顯示要刪除的 Dinner 和輸出的標題&lt;表單&gt;/Dinners/Delete / [id] URL 執行 POST，如果使用者按一下 [刪除] 按鈕，其內的項目。

當我們執行我們的應用程式和存取 *"/ Dinners/Delete / [id] 」* URL 有效 Dinner 物件的呈現 UI，如下所示：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **側邊的主題內容： 為什麼我們所做的文章？** |
| --- |
| 您可能會要求 – 我們到底為什麼瀏覽建立的精力&lt;表單&gt;我們刪除確認 畫面中？ 為什麼不直接使用標準的超連結來連結至動作方法執行實際的刪除作業？ 原因是因為我們想要小心，以防 web 編目程式，並搜尋引擎探索我們的 Url 和不小心造成資料遭到刪除時它們遵循下列連結。 HTTP GET 的基礎 Url 會被視為 「 安全 」，才能存取/搜耙，而且它們應該遵循 HTTP POST 的。 請確定您一律將 破壞性或 HTTP POST 要求背後的資料修改作業為理想的規則。 |

#### <a name="implementing-the-http-post-delete-action-method"></a>實作 HTTP POST 的 Delete 動作方法

我們現在有實作我們 Delete 動作方法的 HTTP GET 版本會顯示刪除確認 畫面。 當使用者按一下 [刪除] 按鈕時，它就會執行的表單 post */Dinners/Dinner / [id]* URL。

讓我們現在實作下列程式碼的 delete 動作方法的 HTTP"POST"行為：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

我們的 Delete 動作方法的 HTTP POST 版本嘗試擷取要刪除的 dinner 物件。 如果找不到它 （因為它已經被刪除） 呈現"NotFound"範本。 如果找到 Dinner，它從 DinnerRepository 刪除它。 然後，它會呈現 「 已刪除 」 範本。

若要實作的 「 已刪除 」 範本，我們會在動作方法中以滑鼠右鍵按一下並選擇 [新增檢視] 內容功能表。 我們將會命名為 「 已刪除 」 檢視，並將它為空白的範本 （而且不接受強型別模型物件）。 我們會再加入一些 HTML 內容：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

現在，當我們執行我們的應用程式和存取並 *"/ Dinners/Delete / [id] 」* URL 的物件，它會轉譯我們 Dinner 刪除確認有效晚餐類似畫面如下：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

當我們按一下 [刪除] 按鈕時，它會執行到 HTTP POST */Dinners/Delete / [id]* URL，這將會從資料庫中刪除 Dinner 並顯示 「 已刪除 」 檢視範本：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>模型繫結安全性

我們已討論過兩種不同的方式，使用 ASP.NET MVC 的內建模型繫結功能。 第一次使用 UpdateModel() 方法來更新現有的模型物件上的屬性和第二個使用 ASP.NET MVC 的支援，可將模型物件中的傳遞做為動作方法參數。 這兩種技巧是非常強大且非常有用。

此能力也會伴隨的責任。 請務必一律是關於安全性擇善時接受使用者輸入，並也是如此當物件繫結至表單的輸入。 您應該要特別注意 HTML 編碼任何使用者輸入的值，以避免 HTML 和 JavaScript 插入式攻擊，需要注意的 SQL 資料隱碼攻擊的永遠 (注意： 我們會在我們的應用程式，將會自動編碼參數以防止這些使用 LINQ to SQL類型的攻擊）。 您永遠不應該依賴用戶端驗證，並一定能採用以防範駭客嘗試傳送假的值給您的伺服器端驗證。

藉此確定您考慮使用 ASP.NET mvc 的繫結功能時的一個其他安全性項目是您要繫結之物件的範圍。 具體來說，您會想要確定您了解您允許將繫結，並確定您只允許實際上應該要更新的使用者可更新這些屬性的屬性的安全性含意。

根據預設，UpdateModel() 方法會嘗試更新所有模型物件上的屬性符合連入的表單參數值。 同樣地，預設也傳遞做為動作方法參數的物件可以有的所有表單參數中設定其屬性。

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>鎖定 依使用量為基礎的繫結

您可以提供明確的"include 清單"的可更新的屬性，以鎖定依使用量為基礎的繫結原則。 做法是將額外的字串陣列參數傳遞至 UpdateModel() 方法類似如下：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

也做為動作方法參數傳遞的物件支援的 [繫結] 屬性，可讓 「 include 清單 」 的允許下面，例如指定的屬性：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>鎖定類型為基礎的繫結

您也可以鎖定型別為基礎的繫結規則。 這可讓您一次，指定繫結規則，然後讓它們所有控制器和動作方法都套用在所有案例中 （包括 UpdateModel 和動作方法參數的案例）。

加入至類型中，[繫結] 屬性，或註冊 （適用於您不在其中擁有類型的案例） 的應用程式的 Global.asax 檔案中，您可以自訂每個型別繫結規則。 然後，您可以使用繫結屬性的 Include 和 Exclude 屬性，以控制哪些屬性是可繫結的特定類別或介面。

我們將 Dinner 類別使用這項技術，在我們 NerdDinner 應用程式，並加入 [繫結] 屬性，將可繫結的屬性清單限制為下列：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

請注意我們不允許操作繫結，透過像是集合也我們允許透過繫結設定 DinnerID 或 HostedBy 的屬性。 基於安全性考量我們將改為只操作這些特定的屬性，使用明確的程式碼，在我們的動作方法內。

### <a name="crud-wrap-up"></a>CRUD 總結

ASP.NET MVC 包含多種內建的功能，可協助您實作表單張貼案例。 我們可以使用各種不同的這些功能來提供我們 DinnerRepository 之上的 CRUD UI 支援。

我們會使用模型為主的方法來實作我們的應用程式。 這表示我們所有的驗證和我們的模型層 – 內和不在我們的控制器或檢視中定義邏輯的商務規則。 我們的控制器類別和我們的檢視範本都不了解 Dinner 模型類別強制執行特定的商務規則。

這將保留我們的應用程式架構的初始狀態，並輕鬆地測試。 我們可以新增其他商務規則到我們的模型層未來並*不需要變更任何程式碼*到我們的控制器或檢視，使其受到支援。 這要提供我們極大的改進和變更我們的應用程式中未來的靈活度。

我們 DinnersController 現在可讓 Dinner 清單/詳細資料，以及建立、 編輯和刪除的支援。 此類別的完整程式碼可在下方：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>下一個步驟

我們現在有我們 DinnersController 類別內實作的基本 CRUD （建立、 讀取、 更新和刪除） 支援。

現在來看看我們如何使用 ViewData 和 ViewModel 類別以在我們的表單上啟用更豐富的 UI。

> [!div class="step-by-step"]
> [上一頁](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [下一頁](use-viewdata-and-implement-viewmodel-classes.md)
