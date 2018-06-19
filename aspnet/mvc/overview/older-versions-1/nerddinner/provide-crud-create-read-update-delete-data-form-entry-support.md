---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: 提供 CRUD （建立、 讀取、 更新、 刪除） 資料組成項目支援 |Microsoft 文件
author: microsoft
description: 步驟 5 會示範如何啟用的編輯、 建立及刪除 Dinners 它也支援接受更我們 DinnersController 類別。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876744"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>提供 CRUD （建立、 讀取、 更新、 刪除） 資料組成項目支援
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 5 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 5 會示範如何啟用的編輯、 建立及刪除 Dinners 它也支援接受更我們 DinnersController 類別。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner 步驟 5： 建立、 更新、 刪除表單案例

我們已導入控制器和檢視，並涵蓋如何使用它們來實作 Dinners 經驗的詳細資料清單/站台上。 下一步是進一步採取 DinnersController 類別，並啟用編輯、 建立和刪除 Dinners 它也支援。

### <a name="urls-handled-by-dinnerscontroller"></a>Url 由 DinnersController

我們先前加入動作方法實作支援兩個 Url 的 DinnersController: */Dinners*和 */Dinners/詳細資料 / [id]*。

| **URL** | **VERB** | **目的** |
| --- | --- | --- |
| */Dinners/* | GET | 顯示即將 dinners HTML 清單。 |
| */Dinners/Details/[id]* | GET | 顯示有關特定 dinner 詳細資料。 |

我們現在會將動作方法，來實作三個額外的 Url: <em>/Dinners/編輯 / [id]、 / Dinners/建立、</em>和<em>/Dinners/刪除 / [id]</em>。 這些 Url 將會啟用編輯現有的 Dinners，建立新的 Dinners 和刪除 Dinners 支援。

我們將會支援這些新的 Url 使用 HTTP GET 和 HTTP POST 動詞命令互動。 HTTP GET 要求到這些 Url 會顯示初始的 HTML 檢視的資料 （表單，以在"edit"的情況下 Dinner 資料填入、 空白表單在 「 建立 」 和 「 刪除 」 在刪除確認 畫面）。 HTTP POST 要求到這些 Url 會儲存/更新/刪除 Dinner 資料中我們 DinnerRepository （及從該處資料庫）。

| **URL** | **VERB** | **目的** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | 顯示可編輯 Dinner 資料以填入 HTML 表單。 |
| POST | 儲存至資料庫的特定 Dinner 表單變更。 |
| */Dinners/Create* | GET | 顯示空的 HTML 表單，好讓使用者定義新 Dinners。 |
| POST | 建立新的 Dinner，並將它儲存在資料庫中。 |
| */Dinners/Delete/[id]* | GET | 顯示刪除確認 畫面中。 |
| POST | 從資料庫刪除指定的 dinner。 |

### <a name="edit-support"></a>編輯支援

讓我們先實作"edit"案例。

#### <a name="the-http-get-edit-action-method"></a>HTTP GET 編輯動作方法

我們會先實作我們編輯動作方法的 HTTP 「 取得 」 行為。 這個方法會叫用時 */Dinners/編輯 / [id]* 要求 URL。 我們實作看起來像：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

上述程式碼會使用 DinnerRepository 擷取 Dinner 物件。 然後，它會呈現檢視的範本，使用 Dinner 物件。 因為我們尚未明確傳遞至範本名稱*View()* helper 方法，它將使用的慣例為基礎的預設路徑解析檢視範本： /Views/Dinners/Edit.aspx。

我們現在來建立此檢視範本。 我們會以編輯方法內按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令來這樣做：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

在 [新增檢視] 對話方塊中，我們會指出我們 Dinner 物件傳遞至為其模型中，我們檢視的範本，並選擇 [編輯] 的範本自動 scaffold:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

當我們按一下 [新增] 按鈕時，Visual Studio 會將新的 「 Edit.aspx 」 檢視的範本檔之我們"\Views\Dinners"目錄中。 它也會開啟新的"Edit.aspx 」 檢視範本內程式碼編輯器 – 填入初始 「 編輯 」 scaffold 實作類似如下：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

讓我們進行一些變更以預設 「 編輯 」 scaffold 產生，並更新 編輯檢視範本的內容下 （這會移除一些不想要公開的屬性）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

當我們執行的應用程式和要求 *"/ 1/編輯 Dinners /"* 我們會看到下列頁面的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

我們檢視所產生的 HTML 標記看起來像下面。 它是標準 HTML – 具有&lt;表單&gt;項目，執行 HTTP POST 來 */Dinners/Edit/1* URL 時 儲存&lt;輸入類型 = 「 送出"/&gt;推入 按鈕。 HTML&lt;輸入類型 ="text"/&gt;項目已為每個可編輯屬性的輸出：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() 和 Html.TextBox() Html Helper 方法

我們 「 Edit.aspx 」 檢視的範本使用數種 「 Html Helper"方法： Html.ValidationSummary()、 Html.BeginForm()、 Html.TextBox() 和 Html.ValidationMessage()。 除了為我們產生 HTML 標記中，這些 helper 方法會提供內建的錯誤處理和驗證支援。

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() helper 方法

Html.BeginForm() helper 方法是什麼輸出 HTML&lt;表單&gt;我們標記中的項目。 我們 Edit.aspx 檢視範本中，您會發現，我們要套用以 C#"using"陳述式時使用此方法。 左大括號表示開頭&lt;表單&gt;內容和右大括號就會指出結尾&lt;/形成&gt;項目：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

或者，如果您找到的"using"陳述式方式比較不自然，像這樣的案例，您可以使用 Html.BeginForm() 和 Html.EndForm() 組合 （它會進行相同的工作）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

不含任何參數呼叫 Html.BeginForm() 會導致輸出至目前要求 URL 中沒有 HTTP POST 的表單項目。 也就是為什麼我們編輯檢視產生*&lt;形成動作 ="/ 1/編輯 Dinners /"方法 ="post"&gt;* 項目。 我們可能具有或者明確參數傳送至 Html.BeginForm() 如果我們想要張貼至不同的 URL。

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() helper 方法

我們 Edit.aspx 檢視會使用 Html.TextBox() helper 方法來輸出&lt;輸入類型 ="text"/&gt;項目：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Html.TextBox() 上述方法，會採用單一參數 – 用來指定這兩個識別碼/名稱的屬性&lt;輸入類型 ="text"/&gt;輸出，以及要填入的文字方塊值的模型屬性的項目。 比方說，我們傳遞到編輯檢視的 Dinner 物件有 「.NET 未來"，"Title"屬性值，因此我們 Html.TextBox("Title") 方法呼叫輸出： *&lt;輸入的識別碼 ="Title"name ="Title"type ="text"value =".NET 未來"/&gt;*.

或者，我們可以使用第一個 Html.TextBox() 參數指定識別碼/名稱的項目，並明確傳遞中要做為第二個參數的值：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

通常我們會想要執行的輸出值的自訂格式。 String.format （) 內建.NET 靜態方法是適用於這些案例。 我們 Edit.aspx 檢視範本使用這個來格式化 EventDate 值 （這是 DateTime 類型的），因此它不會顯示時間 （秒）：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Html.TextBox() 的第三個參數可以選擇性地用於輸出其他的 HTML 屬性。 下列程式碼的片段會示範如何轉譯的其他大小 ="30"的屬性和類別上 ="mycssclass"屬性&lt;輸入類型 ="text"/&gt;項目。 請注意如何我們會逸出的類別屬性使用名稱"@" character because "類別 」 是保留的關鍵字在 C# 中：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>實作 HTTP POST 的編輯動作方法

我們現在有我們實作的編輯動作方法的 HTTP GET 版本。 當使用者要求 */Dinners/Edit/1*他們會收到類似下列的 HTML 網頁的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

按下 [儲存] 按鈕會導致表單張貼至 */Dinners/Edit/1* URL，並送出 HTML&lt;輸入&gt;形成使用 HTTP POST 動詞命令的值。 我們現在可實作我們編輯動作方法 – 將會處理儲存 Dinner 的 HTTP POST 行為。

我們一開始會將多載的 「 編輯 」 動作方法加入至我們 DinnersController 其上已有 「 AcceptVerbs"屬性，表示它會處理 HTTP POST 案例：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

當 [AcceptVerbs] 屬性套用至多載的動作方法時，ASP.NET MVC 會自動處理分派至適當的動作方法會根據傳入的 HTTP 指令動詞的要求。 HTTP POST 要求到<em>/Dinners/編輯 / [id]</em> Url 將會移至上述的編輯方法，對所有其他 HTTP 動詞命令要求時<em>/Dinners/編輯 / [id]</em>Url 將會移至第一個編輯方法我們實作 （這並未沒有 [AcceptVerbs] 屬性）。

| **側邊主題內容： 原因區分透過 HTTP 指令動詞嗎？** |
| --- |
| 您可能會要求 – 我們為何使用單一的 URL 和區隔其行為，透過 HTTP 指令動詞嗎？ 為什麼不只需要兩個不同的 Url，以處理載入及儲存所做的變更嗎？ 例如： /Dinners/編輯 / [id] 顯示的初始表單和 /Dinners/Save / [id] 以處理表單 post 要求來儲存它嗎？ 發行的兩個不同 Url 的缺點是，在我們公佈到 /Dinners/Save/2，並需要重新顯示 HTML 表單，因為發生輸入錯誤的情況下，使用者將最後會有 Dinners/Save/2 URL 其瀏覽器的網址列中 （因為這是URL 表單張貼到）。 如果使用者設定為書籤 redisplayed 的本頁一致瀏覽器的我的最愛清單，或複製/貼上 URL 和電子郵件寄給給朋友，它們將最後儲存會無法運作，未來 （因為該 URL 張貼值而定） 的 URL。 藉由公開單一 URL (例如： /Dinners/Edit/[id]) 並處理它的 HTTP 動詞命令，並加入書籤 編輯頁面及 （或） 將 URL 傳送給其他人的使用者和安全。 |

#### <a name="retrieving-form-post-values"></a>擷取表單張貼值

有多種方式可以存取公佈在我們的 HTTP POST [編輯] 方法中的表單參數。 一個簡單的作法是只控制站的基底類別上使用 要求屬性來存取表單集合和直接擷取已張貼的值：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

我們加入錯誤處理邏輯之後，特別是，會，更詳細資訊，上述的方法。

A 更接近此案例中是利用內建*UpdateModel()* 控制器的基底類別的 helper 方法。 它支援更新，我們將它使用的連入的表單參數傳遞的物件的屬性。 使用反映來判斷物件上的屬性名稱，然後自動轉換並指派值給變數根據用戶端所送出的輸入值。

我們可以使用 UpdateModel() 方法，以簡化我們 HTTP POST 編輯動作使用此程式碼：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

我們現在可以瀏覽 */Dinners/Edit/1* URL，然後變更我們 Dinner 標題：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

當我們按一下 [儲存] 按鈕時，我們會執行表單張貼到我們的編輯動作，並已更新的值將會保存在資料庫中。 我們將然後被重新導向至詳細資料的 URL 為 Dinner （將會顯示最近儲存的值）：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>處理編輯錯誤

我們目前 HTTP POST 實作可以正常運作 – 除非有錯誤。

當使用者編輯表單的錯誤，我們需要確定表單會重新顯示使用資訊的錯誤訊息，將引導他們修正此問題。 這包括使用者要將張貼輸入不正確的情況下 (例如： 的格式不正確的日期字串)，就如同狀況下以及輸入的格式是否有效，但沒有商務規則違規。 發生錯誤時表單應該保留原本輸入，讓他們不必手動將其變更重新填滿使用者輸入的資料。 表單已成功完成之前，應該會為視需要多次重複此程序。

ASP.NET MVC 包括一些不錯的內建功能可簡化錯誤處理和形式顯示。 若要查看這些功能的動作，讓我們以下列程式碼更新我們的編輯動作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

上述程式碼是類似於先前實作中 –，不同之處在於我們現在會周圍的工作中包裝 try/catch 區塊中的錯誤處理。 如果發生例外狀況呼叫 UpdateModel()，或是當我們再試一次，並儲存 DinnerRepository （這會引發例外狀況，如果我們嘗試儲存的 Dinner 物件無效因為我們的模型內的規則違規的緣故），我們 catch 錯誤處理區塊就會將執行。 在其中我們迴圈 Dinner 物件存在於任何規則違規時，並加入 ModelState 物件 （我們將討論）。 然後，我們重新顯示檢視。

若要查看此工作讓我們重新執行應用程式，請編輯 Dinner，並將它變更為有空白的標題，EventDate 的"BOGUS"，，美國國家 （地區） 值會使用英式電話號碼。 當我們按下 [儲存] 按鈕時編輯的 HTTP POST 方法將無法儲存 Dinner （因為沒有錯誤），將會重新顯示表單：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

我們的應用程式擁有不錯錯誤體驗。 具有無效的輸入的文字項目會以紅色反白顯示和其相關的使用者會顯示驗證錯誤訊息。 表單也會保留原本輸入的使用者 – 輸入的資料，讓他們不必重新填滿任何項目。

如何，您可能會問，這會發生？ 如何未標題、 EventDate 和 ContactPhone 文字方塊以紅色強調本身和知道輸出原本輸入的使用者值？ 以及如何未顯示錯誤訊息取得上方清單中？ 好消息是，這不會發生魔術-而不是已因為我們使用的一些內建 ASP.NET MVC 功能可簡化輸入的驗證和錯誤處理案例。

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>了解 ModelState 和驗證的 HTML Helper 方法

控制器類別有 「 ModelState"屬性集合會提供一個方法來指示與正在傳遞至檢視的模型物件有錯誤。 錯誤 ModelState 集合內的項目會識別模型屬性的名稱與問題 (例如:"Title"、"EventDate，"或"ContactPhone")，並允許指定人類看得方便的錯誤訊息 (例如: 「 標題 」 需要)。

*UpdateModel()* helper 方法會自動填入 ModelState 集合，當它嘗試將表單值指派給模型物件上的內容時遇到錯誤。 例如，我們的 Dinner 物件 EventDate 屬性是 DateTime 類型。 無法將上述案例中，指派給它的字串值"BOGUS"UpdateModel() 方法時，UpdateModel() 方法所加入至指出作業錯誤 ModelState 集合的項目發生與屬性。

開發人員也可以撰寫程式碼以明確地加入錯誤 ModelState 集合內的項目，像我們做以下我們 「 攔截 」 錯誤處理在區塊內，這 ModelState 集合填入中作用中的規則違規的項目Dinner 物件：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>與 ModelState 的 html Helper 整合

HTML helper 方法-例如 Html.TextBox()-轉譯輸出時，請檢查 ModelState 集合。 如果發生錯誤的項目存在，它們會呈現使用者輸入的值和 CSS 錯誤類別。

例如，在我們的 [編輯] 檢視中我們使用 Html.TextBox() helper 方法轉譯我們 Dinner 物件的 EventDate:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

當錯誤狀況中呈現的檢視時，Html.TextBox() 方法會檢查 ModelState 集合，以查看只有 「 EventDate"我們 Dinner 物件屬性相關聯的任何錯誤時。 判斷已發生錯誤時呈現送出的使用者輸入 (「 BOGUS") 的值，並加入至 css 錯誤類別&lt;輸入類型 ="textbox"/&gt;它產生的標記：

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

您可以自訂 css 錯誤類別，尋找但是您希望的外觀。 中已定義預設 CSS 錯誤類別 – 「 輸入-驗證-錯誤 」 – *\content\site.css*樣式表，看起來類似下面的：

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

這個 CSS 規則是什麼造成我們無效的輸入項目以反白顯示類似如下：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() Helper Method

Html.ValidationMessage() helper 方法可以用來輸出與特定模型屬性相關聯的 ModelState 錯誤訊息：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

上述程式碼的輸出：  *&lt;/><span class ="欄位的驗證錯誤的 「&gt; 'BOGUS' 的值無效 &lt; /s p&gt;*

Html.ValidationMessage() 協助程式方法也支援可讓開發人員覆寫會顯示錯誤文字訊息的第二個參數：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

上述程式碼的輸出：  <em>&lt;/><span class ="欄位的驗證錯誤的 「&gt;\*&lt;/s p a&gt;</em>而不是針對有錯誤時的預設錯誤文字EventDate 屬性。

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() Helper Method

Html.ValidationSummary() helper 方法可以用來呈現摘要錯誤訊息，伴隨著&lt;ul&gt;&lt;li /&gt;&lt;/u l&gt;之所有詳細錯誤訊息清單中ModelState 集合：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Html.ValidationSummary() helper 方法會接受選擇性的字串參數 – 定義詳細的錯誤清單上方顯示的摘要錯誤訊息：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

您可以選擇覆寫 錯誤清單看起來像使用 CSS。

#### <a name="using-a-addruleviolations-helper-method"></a>使用 AddRuleViolations Helper 方法

初始 HTTP POST 編輯實作會使用 foreach 陳述式的 catch 區塊中迴圈 Dinner 物件的規則違規時，並將它們新增至控制器的 ModelState 集合：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

我們可以讓此程式碼，加上"ControllerHelpers"小清潔器 NerdDinner 專案中，類別，並實作加入至 ASP.NET MVC ModelStateDictionary 類別的 helper 方法的"AddRuleViolations 」 擴充方法中。 這個擴充方法可以將封裝以填入 ModelStateDictionary RuleViolation 錯誤的清單所需的邏輯：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

然後，我們可以更新 HTTP POST 編輯動作方法使用這個擴充方法來填入我們 Dinner 規則違規的 ModelState 集合。

#### <a name="complete-edit-action-method-implementations"></a>完成編輯動作方法的實作

下列程式碼會實作所有控制器所需的邏輯我們的案例中編輯：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

編輯實作的好處就是我們控制器類別都我們檢視的範本必須知道的任何項目有關的特定驗證或我們的 Dinner 模型中強制執行商務規則。 我們可以將其他規則加入我們的模型在未來，不需要對我們的控制器或檢視中，都必須支援的任何程式碼變更。 這會讓我們有彈性地輕鬆發展我們應用程式的需求在未來，最少的程式碼變更。

### <a name="create-support"></a>建立的支援

我們已完成實作 DinnersController 類別的 「 編輯 」 行為。 我們現在移到它-這可讓使用者加入新 Dinners 上實作的 「 建立 」 支援。

#### <a name="the-http-get-create-action-method"></a>HTTP GET 建立動作方法

我們一開始會藉由實作的 HTTP 「 取得 」 行為我們建立動作方法。 會呼叫這個方法，當有人造訪 */Dinners/建立*URL。 我們實作如下所示：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

上述程式碼會建立新的 Dinner 物件，並指派其 EventDate 屬性是在未來一週。 然後，它會呈現新 Dinner 物件為基礎的檢視。 因為我們尚未明確傳遞的名稱*View()* helper 方法，它將使用的慣例為基礎的預設路徑解析檢視範本： /Views/Dinners/Create.aspx。

我們現在來建立此檢視範本。 建立動作方法內按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令，我們可以執行這項操作。 在 [新增檢視] 對話方塊中，我們會指出我們會將 Dinner 物件傳遞至檢視的範本，並選擇自動 scaffold 「 建立 」 範本：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners"目錄中，儲存新的 scaffold 基礎"Create.aspx 」 檢視，並在 IDE 中開啟它：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

讓我們來變更幾個預設 「 建立 」 scaffold 檔案所產生的是，，並向上修改看起來如下：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

現在，當我們執行我們的應用程式，以及存取和 *"/ Dinners/建立 「* 瀏覽器，它會呈現 UI 類似下面我們建立動作實作中的 URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>實作 HTTP POST 建立動作方法

我們有實作我們建立動作方法的 HTTP GET 版本。 當使用者按一下 [儲存] 按鈕會執行的表單 post */Dinners/建立*URL，並送出 HTML&lt;輸入&gt;形成使用 HTTP POST 動詞命令的值。

現在讓我們實作的 HTTP POST 行為我們建立動作方法。 我們一開始會將多載的 「 建立 」 動作方法加入至我們 DinnersController 其上已有 「 AcceptVerbs"屬性，表示它會處理 HTTP POST 案例：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

有各種不同的方式，所以我們可以存取的已張貼的表單參數中啟用 HTTP POST 「 建立 」 方法。

其中一個方法是建立新的 Dinner 物件，然後使用*UpdateModel()* helper 方法 （例如我們所做的編輯動作） 要填入的已張貼的表單值。 我們可以將它加入至我們 DinnerRepository、 將它保存到資料庫，並將使用者重新導向至我們的詳細資料動作以顯示新建立的 Dinner 使用下列程式碼：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

或者，我們可以使用的方法有我們採取 Dinner 物件做為方法參數的 create （） 的動作方法。 ASP.NET MVC 接著自動具現化新的 Dinner 物件我們、 填入其屬性使用的格式輸入，並將它傳遞至動作方法：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

我們上面的動作方法會驗證，Dinner 物件已成功填入表單張貼值藉由檢查 ModelState.IsValid 屬性。 這會傳回 false，如果沒有輸入轉換問題 (例如:"BOGUS"EventDate 屬性的字串)，並有任何問題，如果我們動作方法重新顯示表單。

如果輸入的值是有效的動作方法就會嘗試加入及儲存 DinnerRepository 新 Dinner。 它會包裝在 try/catch 區塊內的這個工作，並重新顯示表單，如果沒有任何商務規則違規 （這會導致引發例外狀況的 dinnerRepository.Save() 方法）。

若要查看此錯誤處理動作中的行為，我們可以要求 */Dinners/建立*URL 和填滿出新 Dinner 相關詳細資料。 不正確的輸入值，會導致建立表單，會重新顯示具有反白顯示類似如下的錯誤：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

請注意，我們建立表單的完全相同驗證和商務規則為我們的編輯表單中的特殊區分。 這是因為我們驗證和商務規則已定義在模型中，而且沒有 UI 或控制站的應用程式中嵌入。 這表示我們可以稍後變更/改良我們的驗證，或在單一的商務規則將和已將它們套用在我們的應用程式。 我們不會變更任何程式碼內我們編輯或建立要自動接受任何新的規則或修改現有的動作方法。

當我們修正 輸入的值，然後按一下 儲存 按鈕同樣地，我們除了 DinnerRepository 會成功，而且新 Dinner 會加入至資料庫。 我們會接著會重新導向至 */Dinners/詳細資料 / [id]* URL-我們看到新建立的 Dinner 的相關詳細資料：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>刪除支援

我們現在加入 「 刪除 」 支援我們 DinnersController。

#### <a name="the-http-get-delete-action-method"></a>HTTP GET Delete 動作方法

我們一開始會藉由實作我們 delete 動作方法的 HTTP GET 行為。 這個方法會呼叫當有人造訪 */Dinners/刪除 / [id]* URL。 以下是實作：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

擷取要刪除 Dinner 嘗試的動作方法。 如果 Dinner 已存在，則會呈現檢視會根據 Dinner 物件。 如果物件不存在 （或已刪除） 其傳回"NotFound"會呈現的檢視會檢視我們稍早為 [詳細資料] 動作方法建立的範本。

我們可以建立 「 刪除 」 檢視範本內的 Delete 動作方法上按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令。 在 [新增檢視] 對話方塊中，我們會表示我們要將 Dinner 物件傳遞給為其模型中，我們檢視的範本，並選擇建立空白的範本：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

當我們按一下 [新增] 按鈕時，Visual Studio 會將新的 「 Delete.aspx 」 檢視的範本檔之我們我們"\Views\Dinners"目錄中。 我們會將某些 HTML 和程式碼新增至範本中，實作 [刪除確認] 畫面中類似下面的：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

上述程式碼顯示要刪除的 Dinner 和輸出的標題&lt;表單&gt;未 /Dinners/刪除 / [id] url 的 POST，如果使用者按一下 [刪除] 按鈕，在其中的項目。

當我們執行我們的應用程式和存取 *"/ Dinners/刪除 / [id]"* URL 有效 Dinner 物件它呈現 UI 類似下面的：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **側邊主題內容： 為什麼我們所做的 POST？** |
| --- |
| 您可能會問： 為什麼我們未瀏覽建立的投入時間&lt;表單&gt;我們刪除確認 畫面中？ 為何不直接使用標準的超連結來連結至動作方法會執行實際的刪除作業？ 原因是因為我們想要非常小心，防範網頁自動尋檢及搜尋引擎探索我們的 Url 和不小心造成它們遵循下列連結時要刪除的資料。 HTTP GET Url 會被視為 「 安全 」，才能存取/搜耙，而且應該遵循 HTTP POST 的。 請確定您一律將破壞型或 HTTP POST 要求背後的資料修改作業是理想的規則。 |

#### <a name="implementing-the-http-post-delete-action-method"></a>實作 HTTP POST 的 Delete 動作方法

我們現在有我們實作的刪除動作方法的 HTTP GET 版本以顯示 [刪除確認] 畫面中。 當使用者按一下 [刪除] 按鈕就會執行的表單 post */Dinners/Dinner / [id]* URL。

讓我們實作了使用下列程式碼的 delete 動作方法的 HTTP"POST"行為：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

擷取要刪除的 dinner 物件嘗試刪除動作方法的 HTTP POST 版本。 如果它找不到 （因為它已經被刪除） 呈現我們"NotFound"的範本。 如果找到 Dinner，它從 DinnerRepository 刪除它。 然後，它會呈現 [已刪除] 範本。

若要實作我們將動作方法中以滑鼠右鍵按一下並選擇 [新增檢視] 內容功能表的 [已刪除] 範本。 我們將命名我們檢視 [已刪除]，並讓它為空白的範本 （而且不接受強型別模型物件）。 然後，我們會將一些 HTML 內容新增至它：

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

現在，當我們執行我們的應用程式，以及存取和 *"/ Dinners/刪除 / [id]"* URL 有效 Dinner，它會呈現我們 Dinner 物件刪除確認畫面上類似如下：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

當我們按一下 [刪除] 按鈕就會執行至 HTTP POST */Dinners/刪除 / [id]* URL，這將刪除 Dinner 從我們的資料庫，並顯示我們的 [已刪除] 檢視範本：

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>模型繫結安全性

我們已討論過兩個不同的方式，使用內建 ASP.NET MVC 模型繫結功能。 第一次使用 UpdateModel() 方法來更新現有的模型物件的屬性和第二個使用 ASP.NET MVC 支援傳遞做為動作方法參數中的模型物件。 這兩種技巧是非常強大且非常有用。

這項強大也會伴隨著責任。 請務必一律是不合理，關於安全性接受使用者輸入時，這一點也成立時物件繫結至表單的輸入。 您應該要特別注意一律 HTML 編碼可避免 HTML 和 JavaScript 插入式攻擊，並注意 SQL 資料隱碼攻擊的任何使用者輸入的值 (注意： 我們會使用 LINQ to SQL 應用程式，將會自動編碼參數以避免這些類型的攻擊）。 永遠不應該依賴用戶端驗證時，一律使用伺服器端驗證，以防止駭客嘗試傳送您假的值。

先確定您的想法時使用 ASP.NET MVC 的繫結功能的一個額外的安全性項目是您要繫結物件的領域。 具體來說，您會想要確定您了解您允許將可繫結，並請確定您只允許實際上應該由要更新的使用者可更新這些屬性的屬性的安全性含意。

根據預設，UpdateModel() 方法會嘗試更新的模型物件上符合連入的表單參數值的所有屬性。 同樣地，根據預設也傳遞做為動作方法參數的物件可以有所有表單參數透過設定其屬性。

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>鎖定每次使用量為基礎的繫結

您可以提供明確"include 清單 」 可以更新的內容，以鎖定每次使用量為基礎的繫結原則。 將額外的字串陣列參數傳遞至像下面 UpdateModel() 方法可以達成此目的：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

物件傳遞為動作方法參數也支援可讓 「 include 清單 」 的允許屬性，以相同方式來指定以下的 [繫結] 屬性：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>鎖定類型為基礎的繫結

您也可以鎖定個別類型為基礎的繫結規則。 這可讓您一次，指定繫結規則，然後再將它們套用在所有案例 （包括 UpdateModel 和動作方法參數的案例） 中跨所有控制器和動作方法。

加入至類型，[繫結] 屬性，或藉由註冊 （適用於案例，您並不擁有型別） 的應用程式 Global.asax 檔案中，您可以自訂每個型別繫結規則。 然後，您可以使用該繫結屬性的包含和排除的屬性，以控制哪些屬性是可繫結的特定類別或介面。

我們將 Dinner 類別的這項技術用於 NerdDinner 應用程式中，並加入 [繫結] 屬性，以限制可繫結的屬性清單所示：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

請注意，我們不允許繫結，透過可操作的與會者集合，也不我們允許透過繫結來設定 DinnerID 或 HostedBy 的內容。 基於安全性考量我們將改為只操作這些特定的屬性，使用明確的程式碼，在我們的動作方法。

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

ASP.NET MVC 包含數個內建功能可協助完成實作表單張貼案例。 我們使用各種不同的這些功能將在我們 DinnerRepository 之上提供 CRUD UI 支援。

我們使用模型已取得焦點的方法來實作我們的應用程式。 這表示，我們所有的驗證和我們的模型層 – 內和不在我們的控制器或檢視表中定義邏輯的商務規則。 控制器類別都我們檢視的範本有任何了解 Dinner 模型類別所強制執行特定的商務規則。

這將保留我們的應用程式架構的初始狀態，並更輕鬆地測試。 我們可以新增其他商務規則到我們的模型層未來和*不需要變更任何程式碼*到我們的控制器或檢視中，都必須支援。 這要提供更多的發展及變更應用程式在未來的靈活性。

我們 DinnersController 現在可讓 Dinner 清單/詳細資料，以及建立、 編輯和刪除支援。 此類別的完整程式碼可以找到如下：

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>下一個步驟

我們現在有我們 DinnersController 類別內實作的基本 CRUD （建立、 讀取、 更新和刪除） 支援。

我們現在看我們如何使用我們的表單上啟用更豐富的 UI 別的 ViewData 和 ViewModel 類別。

> [!div class="step-by-step"]
> [上一頁](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [下一頁](use-viewdata-and-implement-viewmodel-classes.md)
