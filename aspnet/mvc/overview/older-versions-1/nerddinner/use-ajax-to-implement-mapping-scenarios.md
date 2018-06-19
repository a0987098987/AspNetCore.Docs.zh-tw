---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: 若要實作對應實例中使用 AJAX |Microsoft 文件
author: microsoft
description: 步驟 11 會示範如何將 AJAX 對應支援整合到我們的 NerdDinner 應用程式，讓使用者建立、 編輯或檢視以查看 l dinners...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872711"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>用於 AJAX 實作對應實例
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 11 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 11 示範如何整合到我們 NerdDinner 應用程式，讓使用者建立、 編輯或檢視以圖形方式查看 dinner 位置 dinners AJAX 對應支援。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 步驟 11： 整合的 AJAX 對應

我們現在要我們的應用程式一些更視覺化的方式的全新整合 AJAX 對應支援。 這會讓使用者建立、 編輯或檢視以圖形方式查看 dinner 位置 dinners。

### <a name="creating-a-map-partial-view"></a>建立對應的部分檢視

我們會使用對應功能，我們的應用程式內的數個位置中。 若要讓我們的程式碼乾我們將會封裝我們可以重複使用跨多個控制器的動作和檢視單一部分範本內的一般對應功能。 我們將"map.ascx 」 的這個部分檢視的名稱，並建立 \Views\Dinners 目錄內。

我們可以建立 map.ascx 部分 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;檢視功能表命令。 我們會檢視 「 Map.ascx 」 的名稱、 檢查做為部分檢視，並表示我們會將它傳遞強型別"Dinner 」 模型類別：

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

當我們按一下 [新增] 按鈕將會建立我們部分的範本。 然後，我們會將更新 Map.ascx 檔案包含下列內容：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

第一個&lt;指令碼&gt;參考指向 Microsoft 虛擬地球 6.2 對應程式庫。 第二個&lt;指令碼&gt;參考指向的 map.js 檔案我們很快就會建立將會封裝我們一般 Javascript 對應邏輯。 &lt;d i v id ="theMap"&gt;項目是要使用虛擬地球裝載對應的 HTML 容器。

然後我們會內嵌&lt;指令碼&gt;包含兩個此檢視特定的 JavaScript 函式區塊。 第一個函式會使用來自 jQuery 至 wire 上準備好要執行用戶端指令碼頁面時執行的函式。 它會呼叫 LoadMap() helper 函式，我們會定義載入虛擬地球地圖控制項我們 Map.js 指令碼檔案中。 第二個函式是回呼事件處理常式，以識別位置地圖上加 pin。

請注意我們如何使用伺服器端&lt;%= %&gt;內嵌的緯度與經度的 Dinner 我們想要將對應至 JavaScript 的用戶端指令碼區塊內的區塊。 這是一個實用的方法輸出可供用戶端指令碼 （而不需要個別 AJAX 呼叫至伺服器來擷取值 – 使其更快） 的動態值。 &lt;%= %&gt;檢視在伺服器中 – 上轉譯時，區塊會執行，而且因此 HTML 的輸出只最後會與內嵌的 JavaScript 值 (例如： var 緯度 = 47.64312;)。

### <a name="creating-a-mapjs-utility-library"></a>建立 Map.js 公用程式程式庫

我們現在來建立 Map.js 檔案，我們可以用來封裝我們地圖的 JavaScript 功能 （和實作 LoadMap 和 LoadPin 上述的方法）。 我們可以執行這項操作，在專案中，在 \Scripts 目錄上按一下滑鼠右鍵，然後選擇 「 加入-&gt;新項目 」 的功能表命令選取 JScript 項目，並將它命名為"Map.js"。

以下是我們會將新增的 JavaScript 程式碼將與虛擬地球，以顯示我們對應並加入至該位置 pin 我們 dinners 互動 Map.js 檔案：

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>建立和編輯表單與整合對應

我們現在會整合地圖支援與我們現有的建立與編輯案例。 這是很簡單的待辦事項，而我們變更控制器的程式碼的任何不需要好消息。 因為我們建立與編輯檢視共用通用"DinnerForm 」 部分檢視，以實作 dinner 表單 UI，所以我們可以將地圖加入在同一個地方，有兩種我們建立與編輯案例並用。

我們只需要待辦事項是開啟 \Views\Dinners\DinnerForm.ascx 部分檢視並更新它包含我們新的對應部分。 以下是更新的 DinnerForm 外觀加入地圖之後 (請注意： 為畫面簡潔起見下面程式碼片段中會省略 HTML 表單元素):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

部分上方 DinnerForm 會採用"DinnerFormViewModel 」 類型的物件做為模型類型 （因為它需要的 Dinner 物件，以及填入國家/地區 dropdownlist SelectList）。 我們部分的對應只需要 「 Dinner 」 類型的物件做為模型類型，且因此當我們呈現地圖部分我們傳遞只會將 Dinner 子屬性的 DinnerFormViewModel 它：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

JavaScript 函式我們已加入至部分會使用 jQuery 附加至 「 位址 」 HTML 文字方塊中的 「 模糊 」 事件。 您可能聽過的 「 焦點 」 引發事件，當使用者按一下或索引標籤的文字方塊。 相反的操作是使用者結束文字方塊時所引發的 「 模糊 」 事件。 當這種情況，，然後繪製我們地圖上的新位址位置時，上述事件處理常式便會清除緯度和經度的文字方塊值。 我們 map.js 檔所定義的回呼事件處理常式會更新經度和緯度的文字方塊，我們使用虛擬地球根據我們會給它的位址傳回值的表單上。

和現在當我們我們再次執行應用程式，並按一下 「 主機 Dinner 」 索引標籤，我們會看到對應的預設值連同我們標準 Dinner 表單項目一起顯示：

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

當我們輸入位址，並立即索引標籤時，對應會動態更新以顯示位置，和事件處理常式將會填入經緯度文字方塊的位置值：

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

如果儲存新 dinner 後再重新開啟進行編輯，我們會發現，在載入頁面時，顯示地圖位置：

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

每次變更 [位址] 欄位時，將會更新對應和經緯度座標。

既然地圖會顯示 Dinner 位置，我們也可以變更為緯度和經度的表單欄位修改為隱藏的項目 （因為對應自動更新每次輸入的地址） 所顯示的文字方塊。 待辦事項這我們將切換使用 Html.TextBox() HTML helper 來使用 Html.Hidden() helper 方法：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

和我們 form 現在有點更加易懂易記，並避免顯示未經處理的經緯度 （同時仍儲存這些項目資料庫中的每個 Dinner）：

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>整合地圖與詳細資料檢視

現在，我們已經對應整合與我們建立與編輯的情況下，我們也將它整合到我們的案例中的詳細資料。 我們只需要待辦事項就是呼叫&lt;%html.renderpartial("map"); %&gt;詳細資料檢視中。

以下是完整詳細資料檢視 （與對應整合） 的原始碼看起來像：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

現在當使用者巡覽至 /Dinners/詳細資料 / [id] URL 它們將會看到有關 dinner，在地圖上 dinner 位置的詳細資料 （完整的圖釘，當停留顯示 dinner 的標題和它的位址），而有 RSVP fo AJAX 連結r 它：

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>在我們的資料庫和儲存機制中實作位置搜尋

[完成] 關閉我們 AJAX 的實作，讓我們將地圖加入至應用程式，可讓使用者以圖形方式搜尋 dinners 接近它們的首頁。

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

我們一開始會藉由實作有效率地執行 Dinners 位置為主的 radius 搜尋我們資料庫和資料儲存機制層內的支援。 我們可以使用新[地理空間功能的 SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)來實作，或者我們可以使用以下文章中討論的 Gary Dryden SQL 函式方法： [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)和 Rob Conery這裡使用 linq to SQL 相關龐大的網友： [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

若要實作這項技術，我們會開啟 「 伺服器總管 」 在 Visual Studio 中選取 NerdDinner 資料庫，並以滑鼠右鍵按一下其下的 < 函數 > 子節點上，選擇建立新 「 純量值的函式 」:

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

然後，我們將貼上下列 DistanceBetween 函式中：

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

我們再將 SQL Server，我們將稱之為 「 NearestDinners 」 中建立新的資料表值函式：

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

此"NearestDinners 」 資料表函數會使用 DistanceBetween helper 函式來傳回所有 Dinners 內 100 英哩的緯度和經度我們提供：

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

若要呼叫此函式，我們會先開啟 LINQ to SQL 設計工具按兩下 NerdDinner.dbml 檔案，我們 \Models 目錄中：

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

我們會再將 NearestDinners 和 DistanceBetween 函數拖曳至 LINQ 到 SQL 設計工具，會使其以我們的 LINQ 方法加入至 SQL NerdDinnerDataContext 類別：

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

我們可以接著公開"FindByLocation"查詢方法會使用 NearestDinner 函式傳回即將我們 DinnerRepository 類別上的指定位置的 100 英哩內 Dinners:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>JSON 型 AJAX 搜尋動作方法的實作

我們現在將實作控制器動作方法採用新 FindByLocation() 儲存機制的方法傳回到 Dinner 資料可以用來填入對應的清單。 我們必須傳回到 Dinner 資料 JSON （JavaScript 物件標記法） 格式，讓它可以輕鬆管理使用 JavaScript 用戶端上此動作方法。

若要實作這種情況，我們將建立新的 「 SearchController 」 類別 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;控制器功能表命令。 然後，我們會實作下面類似新 SearchController 類別內的"SearchByLocation 」 動作方法：

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController SearchByLocation 動作方法內部 FindByLocation 上呼叫方法來取得一份附近 dinners DinnerRespository。 而非直接給用戶端傳回 Dinner 物件，不過，它改為傳回 JsonDinner 物件。 JsonDinner 類別會公開 Dinner 屬性的子集 (例如： 基於安全理由，它不會揭露的晚餐具有 rsvp 的活動的人的名稱)。 它也包含 RSVPCount Dinner – 上不存在和屬性的動態計算方式是計算特定 dinner 相關聯的 RSVP 物件數目。

我們再返回 dinners 使用 JSON 基礎 wire 格式的順序控制站的基底類別上使用 Json() helper 方法。 JSON 是代表簡單的資料結構的標準文字格式。 以下是範例 JSON 格式的兩個 JsonDinner 物件清單看起來像我們的動作方法傳回時：

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>呼叫使用 jQuery JSON 型 AJAX 方法

我們現在已準備好更新 NerdDinner 在使用 SearchController SearchByLocation 動作方法的應用程式的首頁。 待辦事項，我們將會開啟 /Views/Home/Index.aspx 檢視範本，並更新如文字方塊、 [搜尋] 按鈕、 地圖中，讓它和&lt;div&gt;名為 dinnerList 項目：

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

我們就可以加入至頁面的兩個 JavaScript 函式：

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

第一次載入頁面時，第一個 JavaScript 函式會載入對應。 JavaScript 函式第二個線路向上 JavaScript 按一下搜尋按鈕的事件處理常式。 按下按鈕時呼叫 FindDinnersGivenLocation() JavaScript 函式，我們會將其加入我們 Map.js 檔案：

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

此 FindDinnersGivenLocation() 函式呼叫的對應。Find() 虛擬地球控制項置上輸入的位置。 當虛擬地球對應服務傳回時，對應。Find() 方法會叫用我們當做最後的引數傳遞給 callbackUpdateMapDinners 回呼方法。

這是 callbackUpdateMapDinners() 方法是實際工作。 它使用 jQuery 的 $.post() helper 方法以執行我們 SearchController SearchByLocation() 動作方法 – 將傳遞給它的緯度與經度新置中對齊對應的 AJAX 呼叫。 它會定義內嵌函式 $.post() helper 方法完成時，會呼叫方法以及從動作方法會將傳遞給使用名為"dinners"SearchByLocation() 傳回 JSON 格式 dinner 結果。 然後 foreach 未透過每個傳回的 dinner 並在地圖上加入新的 pin 碼中使用 dinner 緯度和經度和其他屬性。 也會增加 dinner 項目右邊的對應 dinners HTML 清單。 它然後線路總圖釘和 HTML 清單在暫留事件使使用者停留時顯示 dinner 有關的詳細資料：

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

現在當我們執行應用程式，並瀏覽我們將會看到對應的主頁面。 當我們輸入城市名稱的對應會顯示其附近即將 dinners:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

停留 dinner 會顯示其相關的詳細資料。

按一下 Dinner 標題在泡泡，或在 [HTML] 清單右邊將瀏覽至我們的 dinner – 我們可以再選擇性地 RSVP 的：

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>下一個步驟

我們現在已實作 NerdDinner 應用程式的所有應用程式的功能。 讓我們看看我們如何啟用自動化單元現在它的測試。

> [!div class="step-by-step"]
> [上一頁](use-ajax-to-deliver-dynamic-updates.md)
> [下一頁](enable-automated-unit-testing.md)
