---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: 資料顯示在圖表中的 ASP.NET Web Pages (Razor) |Microsoft Docs
author: microsoft
description: 本章說明如何在圖表中顯示資料。 在先前章節中，您已了解如何以手動方式和在方格中顯示資料。 本章說明...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 41af8ac4dba0ba5ad478df7075628186a96f48c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393752"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>在與 ASP.NET Web Pages (Razor) 圖表中顯示資料
====================
by [Microsoft](https://github.com/microsoft)

> 這篇文章說明如何使用圖表來顯示資料在 ASP.NET Web Pages (Razor) 網站中，使用`Chart`協助程式。
> 
> **您將學到什麼**:
> 
> - 如何在圖表中顯示資料。
> - 如何使用內建的佈景主題樣式圖表。
> - 如何儲存圖表以及如何快取它們，以提升效能。
> 
> 這些是 ASP.NET 程式設計文章中所引進的功能：
> 
> - `Chart`協助程式。
> 
> > [!NOTE]
> > 這篇文章中的資訊適用於 ASP.NET Web Pages 1.0 和 Web Pages 2。


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>圖表協助程式

當您想要的資料顯示在圖形表單中時，您可以使用`Chart`協助程式。 `Chart`協助程式可以呈現影像顯示各種不同的圖表類型的資料。 它支援許多選項，用於格式化及標示。 `Chart`協助程式可以轉譯超過 30 種圖表，包括所有類型的圖表，您可能熟悉的 Microsoft Excel 或其他工具&#8212;區域圖、 橫條圖、 直條圖、 折線圖和圓餅圖，以及更多特製化的圖表，例如股票圖。

| **區域圖**![描述： 區域圖類型的圖片](7-displaying-data-in-a-chart/_static/image1.jpg) | **橫條圖**![描述： 橫條圖類型的圖片](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **直條圖**![描述： 直條圖類型的圖片](7-displaying-data-in-a-chart/_static/image3.jpg) | **折線圖**![描述： 折線圖類型的圖片](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **圓形圖**![描述： 圓形圖類型的圖片](7-displaying-data-in-a-chart/_static/image5.jpg) | **股票圖**![描述： 股票圖類型的圖片](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>圖表項目

圖表會顯示資料和其他元素，例如圖例、 軸、 數列和等等。 下圖顯示的圖表元素，當您使用時，您可以自訂許多`Chart`協助程式。 本文說明如何設定一些 （並非全部） 這些項目。

![描述： 圖片顯示圖表項目](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>從資料建立圖表

您在圖表中顯示的資料可以是陣列，從結果中傳回，從資料庫或 XML 檔案中的資料。

### <a name="using-an-array"></a>使用陣列

中所述[ASP.NET Web Pages 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890)，陣列可讓您將一組類似的項目儲存在單一變數。 您可以使用陣列來包含您想要併入圖表中的資料。

此程序示範如何建立圖表資料在陣列中，從使用預設的圖表類型。 它也會示範如何顯示在頁面中的圖表。

1. 建立新的檔案，名為*ChartArrayBasic.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    程式碼第一次建立新的圖表，並設定其寬度和高度。 使用指定圖表標題`AddTitle`方法。 若要新增的資料，您使用`AddSeries`方法。 在此範例中，您可以使用`name`， `xValue`，並`yValues`參數`AddSeries`方法。 `name`參數會顯示在圖表圖例。 `xValue`參數包含圖表的水平軸顯示之資料的陣列。 `yValues`參數會包含用來繪製圖表垂直點的資料陣列。

    `Write`方法實際呈現的圖表。 在此情況下，因為您未指定圖表類型， `Chart` helper 會呈現其預設的圖表，也就是直條圖。
3. 執行網頁瀏覽器中。 瀏覽器會顯示該圖表。 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>使用圖表資料的資料庫查詢

如果您想要繪製的資訊是在資料庫中，您可以執行資料庫查詢，然後使用結果的資料建立圖表。 此程序會示範如何讀取和顯示文件中所建立的資料庫中的資料[使用 ASP.NET Web Pages 網站中的資料庫簡介](https://go.microsoft.com/fwlink/?LinkId=202893)。

1. 新增*應用程式\_資料*如果資料夾不存在的網站根目錄的資料夾。
2. 在*應用程式\_資料*資料夾中，加入名為資料庫檔案*SmallBakery.sdf*中所述的[簡介使用 ASP.NET Web Pages 站台中中的資料庫](https://go.microsoft.com/fwlink/?LinkId=202893).
3. 建立新的檔案，名為*ChartDataQuery.cshtml*。
4. 以下列內容取代現有的內容：   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    程式碼首先會開啟 SmallBakery 資料庫，並將它指派給變數，名為`db`。 此變數代表`Database`可用來讀取和寫入至資料庫的物件。 接下來，程式碼會執行 SQL 查詢，以取得每個產品價格與名稱。 程式碼會建立新的圖表，並傳遞給它的資料庫查詢，藉由呼叫圖表的`DataBindTable`方法。 這個方法會採用兩個參數：`dataSource`參數是用來查詢資料和`xField`參數可讓您設定資料行用於圖表的 x 軸。

    若要使用替代`DataBindTable`方法，您可以使用`AddSeries`方法`Chart`協助程式。 `AddSeries`方法可讓您設定`xValue`和`yValues`參數。 比方說，而不是使用`DataBindTable`方法如下：

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    您可以使用`AddSeries`方法如下：

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    轉譯相同的結果。 `AddSeries`方法會更有彈性，因為您可以更明確地指定的圖表類型和資料但`DataBindTable`方法較為容易使用，如果您不需要額外的彈性。
5. 執行網頁瀏覽器中。 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>使用 XML 資料

圖表的第三個選項是使用圖表做為資料的 XML 檔案。 這需要的 XML 檔案也具有結構描述檔案 (*.xsd*檔案) 的 XML 結構描述。 此程序會示範如何從 XML 檔案讀取資料。

1. 在 *應用程式\_資料*資料夾中，建立新的 XML 檔案，名為*data.xml*。
2. 下列程式碼，也就是一家虛構公司員工相關的某些 XML 資料取代現有的 XML。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. 在 *應用程式\_資料*資料夾中，建立新的 XML 檔案，名為*data.xsd*。 (請注意，擴充功能目前是 *.xsd*。)
4. 以下列內容取代現有的 XML: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. 在網站根目錄中，建立新的檔案，名為*ChartDataXML.cshtml*。
6. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    程式碼會先建立`DataSet`物件。 此物件用來管理的資料會從 XML 檔案讀取，並組織根據結構描述檔案中的資訊。 (請注意頂端的程式碼包含陳述式`using SystemData`。 這為了能夠使用必要`DataSet`物件。 如需詳細資訊，請參閱 < [ &quot;Using&quot;陳述式和完整名稱](#SB_UsingStatements)本文稍後。)

    接下來，程式碼會建立`DataView`資料集為基礎的物件。 資料檢視會提供圖表可以繫結至物件&#8212;也就是讀取，並繪製。 圖表繫結至資料`AddSeries`方法，為您稍早時看到這次圖表陣列資料，不同之處在於`xValue`並`yValues`參數會設定為`DataView`物件。

    此範例也示範如何指定特定圖表類型。 在 新增資料時`AddSeries`方法，`chartType`參數也會設定為顯示圓形圖。
7. 執行網頁瀏覽器中。 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"陳述式和完整格式的名稱
> 
> 含有 Razor 語法的 ASP.NET Web Pages 根據.NET Framework 是由數千個元件 （類別） 所組成。 若要使用所有這些類別可管理，它們會組織成*命名空間*，這是有點像程式庫。 例如，`System.Web`命名空間包含類別，可支援瀏覽器/伺服器通訊`System.Xml`命名空間包含類別，用來建立和讀取 XML 檔案，和`System.Data`命名空間包含類別，可讓您處理使用資料。
> 
> 若要存取.NET Framework 中的任何特定的類別，程式碼需要知道不只是類別名稱，但也表示類別仍在命名空間。 例如，若要使用`Chart`協助程式，必須先找到的程式碼`System.Web.Helpers.Chart`類別，其結合了命名空間 (`System.Web.Helpers`) 類別名稱 (`Chart`)。 這就所謂的類別*完整*名稱&#8212;其完整的模稜兩可的位置內 vastness 的.NET framework。 在程式碼，這看起來如下所示：
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> 不過，很麻煩 （又容易出錯） 能夠使用這些長、 完整的名稱，每次您想要參考類別或協助程式。 因此，若要讓您更輕鬆地使用類別名稱，您可以*匯入*的命名空間您感興趣，這通常是您只在從.NET Framework 中的許多命名空間中。 如果您已匯入命名空間，您可以使用的類別名稱 (`Chart`) 而不是完整限定名稱 (`System.Web.Helpers.Chart`)。 當您的程式碼執行，且遇到類別名稱時，它看起來可能只是您已匯入到該類別的命名空間中。
> 
> 當您使用含有 Razor 語法的 ASP.NET Web Pages 建立 web 網頁上時，您通常使用相同的類別集，每次包括`WebPage`類別、 各種不同的協助程式等等。 若要儲存匯入相關的命名空間，每次您建立網站的工作，因此它會自動匯入一組核心命名空間，每個網站設定 ASP.NET。 這就是為什麼您還沒有必須處理命名空間，或匯入為止;您曾經使用過的所有類別都都會在已為您匯入的命名空間中。
> 
> 不過，有時候您會需要使用不會自動為您匯入的命名空間中的類別。 在此情況下，您可以使用該類別的完整名稱，或您手動匯入的命名空間包含的類別。 若要匯入命名空間，您使用`using`陳述式 (`import` Visual Basic 中)，如您稍早在範例中看到一文。
> 
> 例如，`DataSet`類別位於`System.Data`命名空間。 `System.Data`命名空間不會自動提供給 ASP.NET Razor 頁面。 因此，若要使用`DataSet`類別使用其完整的名稱，您可以使用如下的程式碼：
> 
> `var dataSet = new System.Data.DataSet();`
> 
> 如果您必須使用`DataSet`類別重複您可以匯入像這樣的命名空間，然後使用程式碼中的 類別名稱：
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> 您可以新增`using`陳述式，針對您想要參考的任何其他.NET Framework 命名空間。 不過，如前所述，您不需要這種做法，因為大部分的類別，您將使用的是會自動匯入 asp.net 中使用的命名空間中 *.cshtml*並 *.vbhtml*頁面。


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>顯示在 Web 網頁內的圖表

在範例中您已經看到截至目前為止，您建立的圖表，則圖表會呈現直接在瀏覽器為圖形。 在許多情況下，不過，您想要顯示圖表 頁面上，不只是依本身在瀏覽器中。 若要這樣做需要雙步驟程序。 第一個步驟是建立會產生圖表，頁面，如您所見。

第二個步驟是在另一個網頁中顯示產生的映像。 若要顯示的映像，您可以使用 HTML`<img>`中相同的項目，您通常用來顯示任何映像的方式。 不過，而不是參考 *.jpg*或是 *.png*檔案中，`<img>`項目參考 *.cshtml*檔案，其中包含`Chart`協助程式，建立的圖表。 顯示頁面時執行，則`<img>`項目取得的輸出`Chart`協助程式，並轉譯圖表。

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. 建立名為*ShowChart.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    程式碼會使用`<img>`項目，以顯示您稍早建立的圖表*ChartArrayBasic.cshtml*檔案。
3. 在瀏覽器中執行之 web 網頁。 *ShowChart.cshtml*檔案會顯示包含在程式碼為基礎的圖表影像*ChartArrayBasic.cshtml*檔案。

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>設定圖表的樣式

`Chart`協助程式支援大量的選項可讓您自訂圖表的外觀。 您可以設定色彩、 字型、 框線和等等。 自訂圖表的外觀的簡單方法是使用*佈景主題*。 佈景主題是資訊的指定方式來呈現圖表使用的字型、 色彩、 標籤、 調色盤、 框線和效果的集合。 （請注意圖表的樣式不會指出類型的圖表）。

下表列出內建的佈景主題。

| 主題 | 描述 |
| --- | --- |
| `Vanilla` | 白色背景上顯示紅色的資料行。 |
| `Blue` | 顯示藍色上藍色漸層背景的資料行。 |
| `Green` | 顯示藍色上綠色漸層背景的資料行。 |
| `Yellow` | 黃色漸層背景上顯示橘色的資料行。 |
| `Vanilla3D` | 白色背景上顯示 3d 紅色的資料行。 |

您可以指定要建立新的圖表時所使用之佈景主題。

1. 建立新的檔案，名為*ChartStyleGreen.cshtml*。
2. 以下列內容取代現有的內容頁面中：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    此程式碼等同於先前的範例，會使用資料庫的資料，但會增加`theme`參數時，它會建立`Chart`物件。 下列範例顯示已變更的程式碼：

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. 執行網頁瀏覽器中。 您會看到與之前相同的資料，但圖表看起來更精良： 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>儲存圖表

當您使用`Chart`為您的協助程式為止所看到本文章會協助程式會重新建立從頭圖表每次叫用時。 如有必要，圖表的程式碼也會重新查詢資料庫，或重新讀取 XML 檔案，以取得資料。 在某些情況下，執行此動作可以是複雜的作業，例如您想要查詢的資料庫很大，則 XML 檔案包含大量資料。 即使圖表不需要用到大量資料，以動態方式建立映像的程序佔用伺服器資源，以及如果許多人要求的頁面或頁面會顯示圖表，有可能會影響您的網站的效能。

為了協助您降低建立圖表的潛在效能影響，您可以建立圖表第一次您需要它，然後儲存它。 當圖表需要一次，而不是重新產生它，您也可以擷取已儲存的版本，並呈現的。

您可以透過下列方式來儲存圖表：

- 快取 （伺服器） 上的電腦記憶體中的圖表。
- 將圖表儲存為影像檔。
- 將圖表儲存為 XML 檔案。 此選項可讓您修改圖表，再加以儲存。

### <a name="caching-a-chart"></a>快取的圖表

建立圖表之後，您就可以將它快取。 快取的圖表，以表示它不一定會再次顯示，需要要重新建立。 當您將圖表儲存在快取時，您指定對該圖表必須是唯一的索引鍵。

如果在伺服器記憶體不足，可能會移除儲存至快取的圖表。 此外，如果因為任何原因而重新啟動您的應用程式，會清除快取。 因此，標準的方式來使用快取的圖表是一律先檢查其是否可快取中，和如果沒有，則建立或重建它。

1. 在您的網站根目錄中，建立名為*ShowCachedChart.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>`標記會包括`src`屬性，指向*ChartSaveToCache.cshtml*檔案，並將金鑰傳遞至查詢字串的頁面。 索引鍵包含值&quot;myChartKey&quot;。 *ChartSaveToCache.cshtml*檔案包含`Chart`協助程式建立的圖表。 稍後，您將建立此頁面。

    結尾的頁面中，沒有名為頁面的連結*ClearCache.cshtml*。 這是您也會短時間內建立的頁面。 您需要*ClearCache.cshtml*來測試此範例中的快取，它不是連結或使用快取的圖表時，通常會包含的頁面。
3. 在您的網站根目錄中，建立新的檔案，名為*ChartSaveToCache.cshtml*。
4. 以下列內容取代現有的內容：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    程式碼會先檢查是否任何項目傳遞作為查詢字串中的索引鍵值。 因此，此程式碼嘗試讀取從快取的圖表，藉由呼叫如果`GetFromCache`方法，並將金鑰傳遞給它。 事實上，沒有任何快取下該索引鍵 （這會發生第一次要求圖表） 中，如果程式碼會如往常般建立圖表。 圖表完成後，程式碼將它儲存至快取藉由呼叫`SaveToCache`。 該方法需要索引鍵，因此可以在稍後要求圖表） 和圖表應該儲存在快取的時間量。 （您會快取圖表的確切時間會取決於您認為它所代表的資料可能會變更的頻率）。`SaveToCache`方法也需要`slidingExpiration`參數&#8212;如果此值設為 true，在逾時計數器會重設每次存取時的圖表。 在此情況下，它實際上表示圖表的快取項目到期之後有人存取圖表的最後一個時間的 2 分鐘。 （滑動期限的替代方案是絕對期限，這表示快取項目會到期剛好 2 分鐘後已把它放入快取中，不論它有已存取頻率）。

    最後，程式碼會使用`WriteFromCache`方法來擷取和轉譯來自快取的圖表。 請注意，這個方法是外部`if`檢查快取，因為它會從快取取得圖表，圖表是否有一開始，或必須產生並儲存在快取的區塊。

    請注意，在此範例中，`AddTitle`方法包含時間戳記。 (目前的日期和時間，它會新增&#8212; `DateTime.Now` &#8212;標題。)
5. 建立名為的新頁面*ClearCache.cshtml*和其內容取代為下列：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    此頁面會使用`WebCache`若要移除圖表中快取的 helper *ChartSaveToCache.cshtml*。 如先前所述，您通常不必像這樣的頁面。 您要建立這裡只是為了讓您更輕鬆地測試快取。
6. 執行*ShowCachedChart.cshtml*網頁瀏覽器中。 此頁面會顯示圖表影像中所包含的程式碼為基礎*ChartSaveToCache.cshtml*檔案。 記下的圖表標題中顯示的時間戳記。 

    ![圖表標題中具有時間戳記的基本圖描述： 圖片](7-displaying-data-in-a-chart/_static/image13.jpg)
7. 關閉瀏覽器。
8. 執行*ShowCachedChart.cshtml*一次。 請注意，時間戳記相同，表示圖表未重新產生，但改為從快取中讀取。
9. 在  *ShowCachedChart.cshtml*，按一下**清除快取**連結。 這會帶您前往*ClearCache.cshtml*，這會報告已清除快取。
10. 按一下 **返回 ShowCachedChart.cshtml**連結，或重新執行*ShowCachedChart.cshtml*從 WebMatrix。 請注意，這次的時間戳記已變更，因為在清除快取也一樣。 因此，程式碼，就必須重新產生圖表，並把它放回快取。

### <a name="saving-a-chart-as-an-image-file"></a>將圖表儲存為影像檔

您也可以儲存圖表為影像檔 (例如 *.jpg*檔案) 的伺服器上。 任何映像的方式時，可以使用映像檔。 優點是，檔案會儲存，而不是儲存至暫時快取。 您可以將新的圖表影像儲存在不同的時間 （例如，每隔一小時），並再記錄永久的一段時間的變更。 請注意，您必須先確定您的 web 應用程式已將檔案儲存至資料夾中，您要將映像檔案放在伺服器上的權限。

1. 在您的網站根目錄中，建立名為 *\_ChartFiles*如果不存在。
2. 在您的網站根目錄中，建立新的檔案，名為*ChartSave.cshtml*。
3. 以下列內容取代現有的內容：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    程式碼會先檢查以查看是否 *.jpg*檔案存在，藉由呼叫`File.Exists`方法。 如果檔案不存在，程式碼會建立新`Chart`從陣列。 此時，程式碼會呼叫`Save`方法，並傳遞`path`參數指定的檔案路徑和檔案名稱儲存圖表的位置。 在頁面中，主體`<img>`項目會使用路徑來指向 *.jpg*檔案來顯示。
4. 執行*ChartSave.cshtml*檔案。
5. 傳回至 WebMatrix。 請注意，影像檔名為*chart01.jpg*就已經儲存在 *\_ChartFiles*資料夾。

### <a name="saving-a-chart-as-an-xml-file"></a>將圖表儲存為 XML 檔案

最後，您可以將圖表儲存為 XML 檔案在伺服器上。 透過快取圖表或將圖表儲存至檔案中使用此方法的優點是您無法再顯示圖表，如果您想要修改的 XML。 您的應用程式必須要有您要將映像檔案放在伺服器上資料夾的讀取/寫入權限。

1. 在您的網站根目錄中，建立新的檔案，名為*ChartSaveXml.cshtml*。
2. 以下列內容取代現有的內容：

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    此程式碼很類似您稍早看到將圖表儲存在快取中，不同之處在於它會使用 XML 檔案的程式碼。 程式碼會先檢查 XML 檔案是否存在藉由呼叫`File.Exists`方法。 如果檔案不存在，程式碼會建立新`Chart`物件，並傳遞的檔案名稱為`themePath`參數。 這會建立基礎 XML 檔案中的圖表。 如果 XML 檔案不存在，程式碼會建立圖表，以正常的方式，並接著呼叫`SaveXml`將它儲存。 圖表會呈現使用`Write`方法，為您曾看過。

    如同顯示快取的頁面上，此程式碼會在圖表標題中包含時間戳記。
3. 建立名為的新頁面*ChartDisplayXMLChart.cshtml*並加入下列標記： 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. 執行*ChartDisplayXMLChart.cshtml*頁面。 圖表會顯示。 記下的時間戳記在圖表的標題。
5. 關閉瀏覽器。
6. 在 WebMatrix 中，以滑鼠右鍵按一下 *\_ChartFiles*資料夾中，按一下 **重新整理**，然後再開啟資料夾。 *XMLChart.xml*此資料夾中的檔案由`Chart`協助程式。 

    ![描述： _ChartFiles 資料夾顯示圖表協助程式 」 所建立的 XMLChart.xml 檔案。](7-displaying-data-in-a-chart/_static/image14.jpg)
7. 執行*ChartDisplayXMLChart.cshtml*頁面上一次。 圖表會顯示相同的時間戳記，當做第一次執行的頁面。 這是因為從您稍早儲存的 XML 產生的圖表。
8. 在 WebMatrix 中開啟 *\_ChartFiles*資料夾，然後刪除*XMLChart.xml*檔案。
9. 執行*ChartDisplayXMLChart.cshtml*頁面上一次。 這次更新時間戳記，因為`Chart`helper 必須重新建立 XML 檔案。 如果您想，檢查 *\_ChartFiles*資料夾，請注意，XML 檔案已恢復。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

- [簡介使用的資料庫，在 ASP.NET Web Pages 網站](https://go.microsoft.com/fwlink/?LinkId=202893)
- [使用快取的 ASP.NET Web Pages 站台，以改善效能](https://go.microsoft.com/fwlink/?LinkId=202903)
- [圖表類別](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99))（MSDN 上的 ASP.NET 網頁 API 參考）
