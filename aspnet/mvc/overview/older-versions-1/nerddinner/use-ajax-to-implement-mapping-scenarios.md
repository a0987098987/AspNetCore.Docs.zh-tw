---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: 使用 AJAX 實作對應實例 |Microsoft Docs
author: microsoft
description: 步驟 11 顯示如何將對應的 AJAX 支援整合到我們 NerdDinner 的應用程式，讓使用者建立、 編輯或檢視以查看 l dinners...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 173ba551ca453ad46dbfd83ce1a2eb4a9b8eba3f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374288"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>使用 AJAX 實作對應實例
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 11 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 11 會示範如何整合 NerdDinner 應用程式，讓使用者建立、 編輯或檢視以圖形方式查看的 dinner 位置 dinners AJAX 對應支援。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 步驟 11： 整合的 AJAX 地圖

現在我們要讓我們的應用程式有點呈現更令人興奮藉由整合 AJAX 對應支援。 這可讓使用者建立、 編輯或檢視 dinners 以圖形方式查看 dinner 的位置。

### <a name="creating-a-map-partial-view"></a>建立對應的部分檢視

我們使用我們的應用程式內的數個位置中的對應功能。 若要將我們的程式碼 DRY 我們會封裝單一的部分範本，我們可以重複使用跨多個控制器動作和檢視表內的一般對應功能。 我們會命名為"map.ascx 」 這個部分檢視，並建立 \Views\Dinners 目錄內。

我們可以建立 map.ascx 部分 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [加入]-&gt;檢視功能表命令。 我們將檢視命名為"Map.ascx 」、 檢查做為部分的檢視，並指出我們要將它傳遞強型別 「 Dinner"模型類別：

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

當我們按一下 [新增] 按鈕時，將會建立我們部分的範本。 然後，我們會更新 Map.ascx 檔案包含下列內容：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

第一個&lt;指令碼&gt;參考指向 Microsoft Virtual Earth 6.2 對應程式庫。 第二個&lt;指令碼&gt;參考指向的 map.js 檔案，我們很快就會建立將會封裝我們常見的 Javascript 對應邏輯。 &lt;Div id ="theMap"&gt;項目是 Virtual Earth 將用來裝載對應的 HTML 容器。

然後我們會內嵌&lt;指令碼&gt;包含兩個此檢視特定的 JavaScript 函式的區塊。 第一個函式會使用 jQuery，來連線接連執行網頁時準備好要執行用戶端指令碼的函式。 它會呼叫 LoadMap() helper 函式，我們會將 virtual earth map control 我們 Map.js 指令碼檔案中定義。 第二個函式會將圖釘新增至對應，用於識別位置的回呼事件處理常式。

請注意我們如何使用伺服器端&lt;%= %&gt;內嵌的緯度與經度的 Dinner 我們想要將對應至 JavaScript 的用戶端指令碼區塊內的區塊。 這是很有用的技巧，輸出可供用戶端指令碼 （而不需要個別 AJAX 呼叫回傳至伺服器擷取的值 – 讓它更快） 的動態值。 &lt;%= %&gt;伺服器 – 上呈現檢視時，會執行區塊，並因此 HTML 的輸出將只會得到內嵌的 JavaScript 值 (例如： var 緯度 = 47.64312;)。

### <a name="creating-a-mapjs-utility-library"></a>建立 Map.js 公用程式庫

我們現在來建立可用來在封裝我們的對應中的 JavaScript 功能 （和實作 LoadMap 和 LoadPin 上述的方法） Map.js 檔案。 我們可以執行這項操作上的 \Scripts 目錄，在我們的專案中，按一下滑鼠右鍵，然後選擇 「 Add-&gt;新項目 」 功能表命令，選取 JScript 項目，並將它命名為"Map.js 」。

以下是我們將新增的 JavaScript 程式碼會顯示我們的地圖，並新增至該位置 pin 我們 dinners Virtual Earth 與互動 Map.js 檔案：

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>整合地圖與建立和編輯表單

現在我們會將對應支援整合我們現有的建立與編輯案例。 好消息是，這是很簡單的待辦事項，而且不需要我們要變更任何控制器程式碼。 因為我們的 Create 和 Edit 檢視共用常見"DinnerForm 」 部分檢視來實作 dinner 表單 UI，我們可以在一個位置中新增對應，這兩個我們建立與編輯案例使用它。

我們只需要待辦事項是開啟 \Views\Dinners\DinnerForm.ascx 部分檢視並更新以包含我們新的對應部分。 以下是 已更新的 DinnerForm 會如下所示加入地圖之後 (請注意： 下面程式碼片段，為求簡單明瞭會省略 HTML 表單元素):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

DinnerForm 部分上方，將"DinnerFormViewModel 」 類型的物件會做為其模型類型 （因為它需要一個 Dinner 物件，以及要填入的國家/地區 dropdownlist SelectList）。 我們部分的對應只需要 「 Dinner"類型的物件做為其模型類型，且因此當我們呈現地圖部分我們傳遞只 Dinner 子的屬性 DinnerFormViewModel 給它：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

JavaScript 函式我們已新增至 「 模糊 」 事件附加至"Address"HTML 文字方塊部分會使用 jQuery。 您可能聽過 「 焦點 」 引發事件，當使用者按一下或索引標籤的文字方塊。 的相反是當使用者結束文字方塊時，就會引發的 「 模糊 」 事件。 當這種情況，，，然後繪製在我們的地圖上的新位址位置時，上述的事件處理常式會清除緯度和經度的文字方塊值。 我們 map.js 檔案中定義的回呼事件處理常式會接著更新經度和緯度的文字方塊，我們使用我們為它的位址為基礎的虛擬地球所傳回的值的表單上。

而現在當我們執行我們應用程式並按一下 「 主機 Dinner 」 索引標籤，我們會看到對應的預設值與我們標準 Dinner 表單項目一起顯示：

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

當我們輸入位址，並立即索引標籤時，地圖會動態更新以顯示位置，和我們的事件處理常式將會填入的位置值的緯度/經度文字方塊：

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

如果我們將儲存新的 dinner 並再一次開啟進行編輯時，我們會發現，在頁面載入時，會顯示地圖位置：

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

每次變更 [位址] 欄位時，將會更新對應和緯度/經度座標。

現在，地圖會顯示 Dinner 位置，我們也可以從所顯示的文字方塊 （因為地圖會自動更新每次輸入的地址），而隱藏的項目來變更緯度和經度的表單欄位。 待辦事項這我們將切換使用 Html.TextBox() HTML 協助程式，以使用 Html.Hidden() helper 方法：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

而現在我們 form 有點更加易懂易記，並避免顯示未經處理的緯度/經度 （同時仍將它們儲存到與資料庫中的每個 Dinner）：

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>整合地圖與詳細資料檢視

既然我們已經有對應的 Create 和 Edit 情節的整合，讓我們也將它與整合我們詳細資料的案例。 我們只需要待辦事項就是呼叫&lt;%html.renderpartial("map"); %&gt;詳細資料檢視中。

以下是完整詳細資料檢視 （與對應的整合） 的來源程式碼的樣子：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

現在當使用者瀏覽至 /Dinners/詳細資料 / [id] URL 時就會看到詳細的 dinner dinner 在地圖上的位置和 （完整的圖釘，當停留顯示 dinner 的標題和它的位址），而且具有 RSVP fo AJAX 連結r 它：

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>在我們的資料庫和儲存機制的實作位置搜尋

若要完成關閉 AJAX 實作，讓我們新增對應到 [首頁] 頁面可讓使用者以圖形方式搜尋 dinners 接近它們的應用程式。

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

我們開始要先實作有效率地執行 Dinners 位置為主的 radius 搜尋我們資料庫和資料儲存機制層內的支援。 我們可以使用新[開放式地理空間功能的 SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)若要這樣做，或者我們可以使用以下文章中討論的 Gary Dryden SQL 函式方法： [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)和 Rob Conery有以下使用 linq to SQL 的相關部落格文章： [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

若要實作這項技術，我們將開啟 Visual Studio 中 伺服器總管、 選取 NerdDinner 的資料庫，然後以滑鼠右鍵按一下 在其下的 「 函式 」 子節點和選擇建立新 「 純量值的函式 」:

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

然後，我們將貼上下列 DistanceBetween 函式中：

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

我們接著會建立新的資料表值函式中，我們就呼叫 「 NearestDinners"的 SQL Server:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

此 「 NearestDinners"資料表函式會使用 DistanceBetween helper 函式來傳回所有 Dinners 100 英里的緯度和經度我們提供：

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

若要呼叫此函式，我們會先開啟 LINQ to SQL 設計工具按兩下我們 \Models 目錄內 NerdDinner.dbml 檔案：

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

我們會再將 NearestDinners 和 DistanceBetween 函數拖曳至 LINQ to SQL 設計工具，會使其成為 SQL NerdDinnerDataContext 類別上我們 LINQ 的方法：

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

我們可以接著使用 NearestDinner 函式傳回即將推出我們 DinnerRepository 類別上公開"FindByLocation 」 查詢方法內指定位置的 100 英哩的 Dinners:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>實作以 JSON 為基礎的 AJAX 搜尋動作方法

我們現在將實作會充分利用新 FindByLocation() 存放庫的方法傳回一份可用來填入對應的 Dinner 資料控制器動作方法。 我們必須傳回 Dinner 資料以 JSON （JavaScript 物件標記法） 格式，讓它可以輕鬆地操作用戶端上使用 JavaScript 的此動作方法。

若要這樣做，我們將建立新的 「 SearchController"類別的 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [加入]-&gt;控制器功能表命令。 然後，我們將實作 「 SearchByLocation 」 動作方法內新 SearchController 之類的類別如下：

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController SearchByLocation 動作方法在內部呼叫 FindByLocation 方法，以取得一份附近 dinners DinnerRespository 上。 而不是傳回 Dinner 物件直接用戶端，但它改為傳回 JsonDinner 物件。 JsonDinner 類別會公開 Dinner 屬性的子集 (例如： 基於安全理由，它不會揭露吃晚餐具有 rsvp 的活動的人的名稱)。 它也包含 RSVPCount 屬性不存在於上 Dinner – 和它的動態計算方式是計算特定 dinner 與相關聯的 RSVP 物件數目。

然後，我們會使用控制器的基底類別上 json （） helper 方法以傳回 dinners 使用 JSON 為基礎的 wire 格式的序列。 JSON 是代表簡單的資料結構的標準文字格式。 以下是範例 JSON 格式的兩個 JsonDinner 物件清單看起來像我們的動作方法傳回時：

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>呼叫使用 jQuery 的 JSON 為基礎的 AJAX 方法

我們現在已準備好更新 NerdDinner 在使用 SearchController SearchByLocation 動作方法的應用程式的首頁。 待辦事項，我們將開啟 /Views/Home/Index.aspx 檢視範本，並更新它具有的文字方塊中，[搜尋] 按鈕，我們的對應，以及&lt;div&gt;名為 dinnerList 項目：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

我們就可以加入至頁面的兩個 JavaScript 函式：

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

第一次載入頁面時，第一個 JavaScript 函式就會載入對應。 第二個的 JavaScript 函式線設定 JavaScript 按一下搜尋按鈕上的事件處理常式。 按下按鈕時它會呼叫我們將新增至我們的 Map.js 檔案 FindDinnersGivenLocation() JavaScript 函式：

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

此 FindDinnersGivenLocation() 函式會呼叫對應。Find() Virtual Earth 控制項以將它置入輸入的位置上。 當 virtual earth 地圖服務傳回時，對應。Find() 方法會叫用 callbackUpdateMapDinners 回呼方法，我們將它傳遞做為最後一個引數。

CallbackUpdateMapDinners() 方法是進行實際工作的地方。 它會使用 jQuery 的 $.post() helper 方法來進行 AJAX 呼叫至我們的 SearchController SearchByLocation() 動作方法 – 傳遞的緯度與經度的置中新的對應。 它會定義 $.post() helper 方法完成時，將會呼叫內嵌函式和從動作方法將會傳遞它使用名為"dinners"SearchByLocation() 傳回 JSON 格式化 dinner 結果。 然後到每個傳回，並為 foreach，並使用 加入新的 pin 碼在地圖上的 dinner 的緯度和經度和其他屬性。 它也會加入到 dinners 右邊的對應 HTML 清單的 dinner 項目。 它然後線接圖釘和 HTML 清單的滑鼠停留事件，讓使用者停留於它們時，會顯示有關 dinner 詳細資料：

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

現在當我們執行應用程式，並瀏覽我們將會看到地圖首頁。 當我們輸入的城市所在州名地圖會顯示即將推出的 dinners 接近它：

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

將滑鼠游標移到會顯示其相關的詳細資料。

按一下 Dinner 標題在泡泡圖，或在 [HTML] 清單右邊會巡覽我們 dinner – 我們可以再選擇性地回覆至：

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>下一個步驟

我們現在已實作 NerdDinner 應用程式的所有應用程式功能。 讓我們立即看看我們如何啟用自動化單元測試它。

> [!div class="step-by-step"]
> [上一頁](use-ajax-to-deliver-dynamic-updates.md)
> [下一頁](enable-automated-unit-testing.md)
