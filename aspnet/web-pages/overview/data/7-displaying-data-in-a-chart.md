---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: "顯示資料在圖表中以 ASP.NET Web Pages (Razor) |Microsoft 文件"
author: microsoft
description: "本章節將說明如何在圖表中顯示資料。 在先前章節中，您將學會如何以手動方式，在方格中顯示資料。 本章說明..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: f252b74bc42d0ea65b8b1150973c4f3c50cc9cf4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>顯示資料在圖表中以 ASP.NET Web Pages (Razor)
====================
by [Microsoft](https://github.com/microsoft)

> 這篇文章說明如何使用圖表來顯示資料在 ASP.NET Web Pages (Razor) 的網站中，使用`Chart`協助程式。
> 
> **您將學習**:
> 
> - 如何在圖表中顯示資料。
> - 如何使用內建的佈景主題樣式圖表。
> - 如何儲存圖表以及如何快取它們，以提升效能。
> 
> 這些是 ASP.NET 程式設計文件中所引進的功能：
> 
> - `Chart`協助程式。
> 
> > [!NOTE]
> > 本主題資訊適用於 ASP.NET Web Pages 1.0 和 Web Pages 2。


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>圖表協助程式

當您想要的資料顯示在圖形表單中時，您可以使用`Chart`協助程式。 `Chart` Helper 可以呈現影像中各種不同的圖表類型顯示資料。 格式化及標示支援許多選項。 `Chart` Helper 可以轉譯 30 個以上類型的圖表，包括所有類型的圖表，您可能已經熟悉 Microsoft Excel 或其他工具中&#8212;區域圖、 橫條圖、 直條圖、 折線圖、 和圓餅圖，以及多個特殊的圖表，例如股票圖。

| **區域圖**![描述： 區域圖類型的圖片](7-displaying-data-in-a-chart/_static/image1.jpg) | **橫條圖**![描述： 橫條圖類型的圖片](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **直條圖**![描述： 直條圖類型的圖片](7-displaying-data-in-a-chart/_static/image3.jpg) | **折線圖**![描述： 折線圖類型的圖片](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **圓形圖**![描述： 圓形圖類型的圖片](7-displaying-data-in-a-chart/_static/image5.jpg) | **股票圖**![描述： 股票圖類型的圖片](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>圖表項目

圖表會顯示資料和其他元素，例如圖例、 軸、 數列和等等。 下圖顯示的圖表項目，當您使用時，您可以自訂許多`Chart`協助程式。 本文將說明如何設定某些 （並非全部） 的這些項目。

![描述： 圖片顯示圖表項目](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>從資料建立圖表

您在圖表中顯示的資料可以是陣列，從資料庫中，從傳回的結果或 XML 檔案中的資料。

### <a name="using-an-array"></a>使用陣列

中所述[ASP.NET Web Pages 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890)，陣列可讓您在單一的變數中儲存的類似項目集合。 您可以使用陣列，包含您想要包含在圖表中的資料。

此程序示範如何建立圖表資料在陣列中，從使用預設的圖表類型。 它也會示範如何顯示在頁面內的圖表。

1. 建立新的檔案命名為*ChartArrayBasic.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    程式碼會先建立新的圖表，並設定其寬度和高度。 使用指定的圖表標題`AddTitle`方法。 若要新增的資料，您使用`AddSeries`方法。 在此範例中，您會使用`name`， `xValue`，和`yValues`參數`AddSeries`方法。 `name`參數會顯示在圖表圖例中。 `xValue`參數會包含沿著圖表的水平軸顯示的資料陣列。 `yValues`參數包含用來繪製圖表垂直點的資料陣列。

    `Write`方法實際上會呈現的圖表。 在此情況下，因為您沒有指定圖表類型， `Chart` helper 呈現其預設的圖表，這是直條圖。
3. 執行網頁瀏覽器中。 瀏覽器會顯示該圖表。 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>使用圖表資料的資料庫查詢

如果您想要繪製的資訊在資料庫中，您可以執行資料庫查詢，然後使用 建立該圖表的 結果的資料。 此程序會示範如何讀取和顯示文件中建立的資料庫資料[簡介使用 ASP.NET Web Pages 站台中的資料庫](https://go.microsoft.com/fwlink/?LinkId=202893)。

1. 新增*應用程式\_資料*如果資料夾不存在該網站的根資料夾。
2. 在*應用程式\_資料*資料夾中，加入名為資料庫檔案*SmallBakery.sdf*所述[簡介使用 ASP.NET Web Pages 站台中中的資料庫](https://go.microsoft.com/fwlink/?LinkId=202893).
3. 建立新的檔案命名為*ChartDataQuery.cshtml*。
4. 以下列內容取代現有的內容：   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    此程式碼第一次開啟 SmallBakery 資料庫，並將它指派給變數，名為`db`。 此變數代表`Database`可用來讀取和寫入至資料庫的物件。 接下來，程式碼會執行 SQL 查詢，以取得每個產品價格與名稱。 程式碼建立新的圖表，並將資料庫查詢傳遞給它，藉由呼叫圖表的`DataBindTable`方法。 這個方法會採用兩個參數：`dataSource`參數是用來從查詢中，資料和`xField`參數可讓您設定資料行用於圖表的 x 軸。

    做為使用替代`DataBindTable`方法，您可以使用`AddSeries`方法`Chart`協助程式。 `AddSeries`方法可讓您設定`xValue`和`yValues`參數。 例如，而不是使用`DataBindTable`方法如下：

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    您可以使用`AddSeries`方法如下：

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    兩者會呈現相同的結果。 `AddSeries`方法是更有彈性，因為您可以更明確地指定的圖表類型和資料，但`DataBindTable`方法比較容易使用，如果您不需要額外的彈性。
5. 執行網頁瀏覽器中。 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>使用 XML 資料

圖表的第三個選項是使用 XML 檔案做為資料圖表。 這需要 XML 檔案也具有結構描述檔案 (*.xsd*檔案) 的 XML 結構描述。 此程序會示範如何從 XML 檔案讀取資料。

1. 在*應用程式\_資料*資料夾中，建立新的 XML 檔案，名為*data.xml*。
2. 下列程式碼，也就是一些有關虛構公司中員工的 XML 資料取代現有的 XML。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. 在*應用程式\_資料*資料夾中，建立新的 XML 檔案，名為*data.xsd*。 (請注意，擴充功能目前是*.xsd*。)
4. 以下列內容取代現有的 XML: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. 在網站的根目錄，在建立新的檔案，名為*ChartDataXML.cshtml*。
6. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    程式碼會先建立`DataSet`物件。 此物件用來管理的資料會從 XML 檔案讀取，並組織根據結構描述檔案中的資訊。 (請注意程式碼的頂端包含陳述式`using SystemData`。 這為了能夠使用必要`DataSet`物件。 如需詳細資訊，請參閱[&quot;使用&quot;陳述式和完整名稱](#SB_UsingStatements)本文稍後。)

    接下來，程式碼建立`DataView`資料集為基礎的物件。 資料檢視提供的物件，圖表可以繫結至&#8212;亦即，讀取和繪製。 圖表會繫結至資料使用`AddSeries`方法，為您稍早時看見圖表陣列資料，但這次`xValue`和`yValues`參數已設為`DataView`物件。

    此範例也示範如何指定特定圖表類型。 在 加入資料時`AddSeries`方法，`chartType`參數也會設為顯示圓形圖。
7. 執行網頁瀏覽器中。 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP] 
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"陳述式和完整限定的名稱
> 
> 含有 Razor 語法的 ASP.NET Web Pages 根據.NET Framework 包含數千個元件 （類別）。 若要使用所有這些類別可管理，它們組織成*命名空間*，其中會有些許類似文件庫。 例如，`System.Web`命名空間包含類別，可支援瀏覽器/伺服器通訊，`System.Xml`命名空間包含類別，可用來建立和讀取 XML 檔案和`System.Data`命名空間包含類別，可讓您處理與資料。
> 
> 若要存取.NET Framework 中的任何特定的類別，程式碼需要知道不只是類別名稱，但也表示類別仍在命名空間。 例如，若要使用`Chart`helper，若要尋找的程式碼需要`System.Web.Helpers.Chart`類別，其結合了命名空間 (`System.Web.Helpers`) 與類別名稱 (`Chart`)。 這就所謂的類別*完整*名稱&#8212;其完整的明確位置 vastness 內的.NET framework。 在程式碼，這看起來會如下所示：
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> 不過，它很麻煩 （又容易出錯） 需要使用這些長、 完整的名稱，每次您想要參考類別或協助程式。 因此，若要更輕鬆地使用類別名稱，您可以*匯入*的命名空間您感興趣，通常是只從.NET Framework 中的多個命名空間中的少數。 如果您已匯入命名空間，您可以使用只有類別名稱 (`Chart`) 而不是完整限定名稱 (`System.Web.Helpers.Chart`)。 當您的程式碼執行，且遇到類別名稱時，它看起來可能只匯入到該類別的命名空間中。
> 
> 當您使用含有 Razor 語法的 ASP.NET Web Pages 建立網頁時，您通常使用相同的一組類別，每次包括`WebPage`類別、 各種 helper，等等。 若要儲存匯入相關的命名空間，每當您建立網站的工作，會將 ASP.NET 設定讓它自動匯入的每個網站的核心命名空間的一組。 這就是為什麼您沒處理命名空間或匯入到現在;您已使用過的所有類別都都會為您已經匯入的命名空間中。
> 
> 不過，有時候您需要使用不會自動為您匯入的命名空間中的類別。 在此情況下，您可以使用該類別的完整名稱，或您手動匯入的命名空間包含的類別。 若要匯入命名空間，您使用`using`陳述式 (`import`在 Visual Basic 中)，如您之前在範例中看到文件。
> 
> 例如，`DataSet`類別位於`System.Data`命名空間。 `System.Data`命名空間不會自動提供給 ASP.NET Razor 頁面。 因此，若要使用`DataSet`類別使用的完整限定的名稱，您可以使用類似的程式碼：
> 
> `var dataSet = new System.Data.DataSet();`
> 
> 如果您必須使用`DataSet`重複類別您可以匯入像這樣的命名空間，然後使用程式碼中的 類別名稱：
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> 您可以加入`using`陳述式，針對您想要參考的任何其他.NET Framework 命名空間。 不過，如所述，您不需要這種做法，因為會自動匯入 asp.net 中使用的命名空間中大部分的類別，您將使用*.cshtml*和*.vbhtml*頁面。


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>顯示在網頁內的圖表

在範例中您看到了到目前為止，您建立圖表，然後圖表直接瀏覽器中呈現為圖形。 在許多情況下，不過，您想要顯示圖表 頁面上，不只是單獨使用瀏覽器中。 若要這樣做，需要兩步驟程序。 第一個步驟是建立一個頁面，會產生圖表，您已經看到。

第二個步驟是在另一個網頁中顯示所產生的影像。 若要顯示的映像，您可以使用 HTML`<img>`中相同的項目，您通常用來顯示任何映像的方式。 不過，而不是參考*.jpg*或*.png*檔案，`<img>`項目參考*.cshtml*檔案，其中包含`Chart`helper，建立的圖表。 顯示頁面的執行時，則`<img>`元素取得的輸出`Chart`協助程式，並轉譯圖表。

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. 建立名為*ShowChart.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    程式碼會使用`<img>`項目以顯示您稍早建立的圖表*ChartArrayBasic.cshtml*檔案。
3. 執行網頁瀏覽器中。 *ShowChart.cshtml*檔案會顯示包含在程式碼為基礎的圖表影像*ChartArrayBasic.cshtml*檔案。

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>設定圖表的樣式

`Chart`協助程式支援大量的選項可讓您自訂圖表的外觀。 您可以設定色彩、 字型、 框線和等等。 若要自訂圖表外觀的簡單方法是使用*佈景主題*。 佈景主題是一個資訊集合，指定如何呈現圖表使用的字型、 色彩、 標籤、 調色盤、 框線和效果。 （請注意圖表樣式的圖表類型不表示）。

下表列出內建的佈景主題。

| 主題 | 描述 |
| --- | --- |
| `Vanilla` | 白色背景上顯示紅色的資料行。 |
| `Blue` | 顯示藍色藍色漸層背景上的資料行。 |
| `Green` | 顯示藍色綠色漸層背景上的資料行。 |
| `Yellow` | 黃色漸層背景上顯示橙色的資料行。 |
| `Vanilla3D` | 白色背景上顯示 3d 紅色的資料行。 |

您可以指定要使用當您建立新圖表的佈景主題。

1. 建立新的檔案命名為*ChartStyleGreen.cshtml*。
2. 以下列內容取代現有的內容頁面中：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    此程式碼相當於先前的範例會使用資料庫的資料，但將`theme`參數會在建立時`Chart`物件。 下圖顯示變更的程式碼：

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. 執行網頁瀏覽器中。 您會看到相同的資料之前，但圖表看起來更精美： 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>儲存圖表

當您使用`Chart`協助程式為您所知本文中，協助重新建立的圖表從頭每次叫用時。 必要時，圖表的程式碼也重新查詢資料庫，或重新讀取 XML 檔案，以取得資料。 在某些情況下，這個動作可能複雜的作業，例如，如果您要查詢的資料庫很大，或 XML 檔案包含大量資料。 即使圖表不會包含大量資料，以動態方式建立映像的程序佔用伺服器資源，若有許多人要求的頁面顯示圖表，則可能會影響您的網站的效能。

為了協助您降低建立圖表的潛在效能影響，您可以建立圖表第一次需要它，並將其儲存。 當圖表所需一次，而不是重新產生，您也可以擷取儲存的版本，並呈現該。

您可以以下列方式來儲存圖表：

- 快取 （伺服器） 上的電腦記憶體中的圖表。
- 將圖表儲存為影像檔。
- 將圖表儲存為 XML 檔案。 此選項可讓您儲存之前，修改圖表。

### <a name="caching-a-chart"></a>快取圖表

在您建立圖表後，您可以快取。 快取圖表，以表示它並未在重新建立，需要再次顯示。 當您將圖表儲存快取中時，您指定不得重複用於該圖表的索引鍵。

如果在伺服器記憶體不足，可能會移除儲存在快取的圖表。 此外，如果因為任何原因而重新啟動您的應用程式，會清除快取。 因此，標準的方式來處理快取的圖表是以一律先查看是否有快取中，以及如果不是，則建立或重新建立它。

1. 在您的網站根目錄中，建立名為*ShowCachedChart.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>`標記包含`src`屬性指向*ChartSaveToCache.cshtml*檔案，並將金鑰傳遞給查詢字串為頁面。 此金鑰包含值&quot;myChartKey&quot;。 *ChartSaveToCache.cshtml*檔案包含`Chart`helper 所建立的圖表。 您要立即建立這個頁面。

    在頁面的結尾沒有名為頁面的連結*ClearCache.cshtml*。 這是您也會短時間內建立的頁面。 您需要*ClearCache.cshtml*僅要測試此範例中的快取，它不是連結或使用快取的圖表時，通常會包含的頁面。
3. 在您的網站根目錄中，建立新的檔案命名為*ChartSaveToCache.cshtml*。
4. 以下列內容取代現有的內容：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    程式碼首先檢查任何項目是否已傳遞做為查詢字串中的索引鍵值。 如果這個程式碼，會嘗試藉由呼叫讀取快取圖表`GetFromCache`方法，並將其傳遞索引鍵。 如果證明，沒有在該索引鍵 （這會發生第一次要求圖表） 快取中的項目，程式碼會如往常般建立圖表。 圖表完成時，程式碼將它儲存至快取藉由呼叫`SaveToCache`。 該方法需要索引鍵 （因此稍後可以要求圖表） 和圖表應儲存在快取中的時間量。 （您會快取圖表的確切時間會取決於您認為它所代表的資料可能變更的頻率）。`SaveToCache`方法也需要`slidingExpiration`參數&#8212;如果設定為 true，在逾時計數器會重設每次存取時的圖表。 在此情況下，其作用中表示圖表的快取項目過期有人存取圖表上一次之後的 2 分鐘。 （滑動期限的替代方式是絕對期限，表示快取項目會到期完全 2 分鐘後它已放入快取中，不論其頻率必須存取）。

    最後，程式碼會使用`WriteFromCache`方法來擷取和轉譯來自快取的圖表。 請注意，這個方法是外部`if`檢查快取，因為圖表已有一開始還是必須產生並儲存在快取，它會從快取取得圖表的區塊。

    請注意，在範例中，`AddTitle`方法包含時間戳記。 (它會將目前的日期和時間&#8212; `DateTime.Now` &#8212;標題。)
5. 建立名為的新頁面*ClearCache.cshtml*和其內容取代為下列：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    此頁面會使用`WebCache`移除圖表中快取*ChartSaveToCache.cshtml*。 如前文所述，您通常不必有一個像這樣的頁面。 您要建立，請參閱這裡只以更輕鬆地測試快取。
6. 執行*ShowCachedChart.cshtml*網頁瀏覽器中。 此頁面會顯示包含在程式碼為基礎的圖表影像*ChartSaveToCache.cshtml*檔案。 請注意圖表標題中顯示的時間戳記。 

    ![描述： 時間戳記在圖表標題的基本圖圖片](7-displaying-data-in-a-chart/_static/image13.jpg)
7. 關閉瀏覽器。
8. 執行*ShowCachedChart.cshtml*一次。 請注意，時間戳記相同如往常一般，表示圖表未重新產生，但改為從快取中讀取。
9. 在*ShowCachedChart.cshtml*，按一下 **清除快取**連結。 這會帶您前往*ClearCache.cshtml*，哪些報告已清除快取。
10. 按一下**返回 ShowCachedChart.cshtml**連結，或重新執行*ShowCachedChart.cshtml*從 WebMatrix。 請注意，此時間戳記已變更，因為在清除快取。 因此，程式碼必須重新產生圖表，並放回快取。

### <a name="saving-a-chart-as-an-image-file"></a>將圖表儲存為影像檔

您也可以將圖表儲存為影像檔 (例如，做為*.jpg*檔案) 的伺服器上。 就像任何映像的方式時，可以使用映像檔。 優點是，檔案會儲存，而不是儲存在暫時快取。 您可以將新的圖表影像儲存在不同的時間 （例如，每小時），並保持永久記錄一段時間的變更。 請注意，您必須先確定您的 web 應用程式已將檔案儲存到資料夾中，您要將影像檔放在伺服器上的權限。

1. 在您的網站根目錄中，建立名為 *\_ChartFiles*如果不存在。
2. 在您的網站根目錄中，建立新的檔案命名為*ChartSave.cshtml*。
3. 以下列內容取代現有的內容：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    程式碼會先檢查以查看是否*.jpg*檔案存在，藉由呼叫`File.Exists`方法。 如果檔案不存在，程式碼會建立新`Chart`從陣列。 此時，程式碼會呼叫`Save`方法，並傳遞`path`參數來指定的檔案路徑和檔案名稱儲存圖表的位置。 本文的頁面上，`<img>`項目使用的路徑指向*.jpg*檔案，即可顯示。
4. 執行*ChartSave.cshtml*檔案。
5. 傳回至 WebMatrix。 請注意，映像檔名為*chart01.jpg*就已經儲存在 *\_ChartFiles*資料夾。

### <a name="saving-a-chart-as-an-xml-file"></a>將圖表儲存為 XML 檔案

最後，您可以將圖表儲存為伺服器上的 XML 檔案。 使用此方法優於快取圖表或將圖表儲存至檔案的優點是您無法再顯示圖表，如果您想要修改的 XML。 您的應用程式必須要有您要將影像檔放在伺服器上資料夾的讀取/寫入權限。

1. 在您的網站根目錄中，建立新的檔案命名為*ChartSaveXml.cshtml*。
2. 以下列內容取代現有的內容：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    此程式碼很類似之前看到將圖表儲存在快取中，不同之處在於它會使用 XML 檔案的程式碼。 程式碼會先檢查以查看 XML 檔案是否存在藉由呼叫`File.Exists`方法。 如果檔案不存在，程式碼會建立新`Chart`物件，並傳遞的檔案名稱為`themePath`參數。 這會建立基礎無論是 XML 檔案中的圖表。 如果 XML 檔案不存在，程式碼建立的圖表，以正常的方式，然後呼叫`SaveXml`儲存它。 使用轉譯圖表`Write`方法，為您所見之前。

    如同的快取的頁面上，這段程式碼會在圖表標題中包含時間戳記。
3. 建立名為的新頁面*ChartDisplayXMLChart.cshtml*並加入下列標記： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. 執行*ChartDisplayXMLChart.cshtml*頁面。 圖表會顯示。 記下的時間戳記在圖表的標題。
5. 關閉瀏覽器。
6. 在 WebMatrix 中，以滑鼠右鍵按一下 *\_ChartFiles*資料夾中，按一下 **重新整理**，，然後開啟資料夾。 *XMLChart.xml*此資料夾中的檔案由`Chart`協助程式。 

    ![描述： _ChartFiles 資料夾顯示圖表協助程式 」 所建立的 XMLChart.xml 檔案。](7-displaying-data-in-a-chart/_static/image14.jpg)
7. 執行*ChartDisplayXMLChart.cshtml*頁面上一次。 圖表可顯示相同的時間戳記，當做第一次執行的頁面。 這是因為正在從您稍早儲存的 XML 產生圖表。
8. 在 WebMatrix 中開啟 *\_ChartFiles*資料夾，然後刪除*XMLChart.xml*檔案。
9. 執行*ChartDisplayXMLChart.cshtml*頁面上一次。 這次更新時間戳記，因為`Chart`協助程式必須重新建立 XML 檔案。 如果您想要請檢查 *\_ChartFiles*資料夾，然後注意上一步是 XML 檔案。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

- [簡介使用的資料庫中 ASP.NET Web Pages 站台](https://go.microsoft.com/fwlink/?LinkId=202893)
- [使用快取中 ASP.NET Web Pages 站台，以改善效能](https://go.microsoft.com/fwlink/?LinkId=202903)
- [圖表類別](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99))（MSDN 上的 ASP.NET Web Pages 應用程式開發介面參考）
