---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 教學課程：伺服器廣播與 SignalR 2 |Microsoft Docs
author: tdykstra
description: 本教學課程會示範如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837425"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>教學課程：伺服器廣播與 SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

本教學課程會示範如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。 伺服器廣播表示，伺服器就會傳送至用戶端的通訊。

您會在本教學課程中建立的應用程式會模擬股票行情即時看板，伺服器廣播功能的一般案例。 請定期伺服器隨機更新股價，並更新廣播到所有已連線的用戶端。 在瀏覽器、 數字和符號**變更**並**%** 動態變更通知來回應來自伺服器的資料行。 如果您開啟其他瀏覽器相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。

![建立 web](tutorial-server-broadcast-with-signalr/_static/image1.png)

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立專案
> * 設定伺服器程式碼
> * 檢查伺服端程式碼
> * 設定用戶端程式碼
> * 檢查用戶端程式碼
> * 測試應用程式
> * 啟用記錄

> [!IMPORTANT]
> 如果您不想要逐步建置應用程式的步驟，您可以安裝 SignalR.Sample 封裝在新的空白的 ASP.NET Web 應用程式專案。 如果您沒有執行本教學課程中的步驟安裝 NuGet 套件，您必須依照*readme.txt*檔案。 若要執行封裝，您需要新增 OWIN 啟動類別呼叫`ConfigureSignalR`已安裝的封裝中的方法。 如果您需要新增 OWIN 啟動類別，您會收到錯誤。 請參閱[Stockservices.asmx 範例安裝](#install-the-stockticker-sample)一節。


## <a name="prerequisites"></a>必要條件

 * [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。

## <a name="create-the-project"></a>建立專案

本節說明如何使用 Visual Studio 2017 來建立空白的 ASP.NET Web 應用程式。

1. 在 Visual Studio 中建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. 在 [**新增 ASP.NET Web 應用程式-SignalR.StockTicker** ] 視窗中，保持**空白**，並選取 **[確定]**。

## <a name="set-up-the-server-code"></a>設定伺服器程式碼

在本節中，您會將設定伺服器執行的程式碼。

### <a name="create-the-stock-class"></a>建立庫存類別

請先建立*股票*模型類別，您將用來儲存和傳輸股票的相關資訊。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **類別**。

1. 將類別命名為*股票*並將它新增至專案。

1. 中的程式碼取代*Stock.cs*這段程式碼檔案：

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    當您建立的股票時，您會將兩個屬性是`Symbol`(例如，microsoft 的 MSFT) 和`Price`。 其他屬性，取決於您如何及何時設定`Price`。 您所設定的第一次`Price`，值取得傳播到`DayOpen`。 在那之後，當您設定`Price`，應用程式會計算`Change`並`PercentChange`屬性值根據之間的差異`Price`和`DayOpen`。

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>建立 StockTickerHub 和 Stockservices.asmx 類別

您將使用 SignalR 中樞 API 來處理伺服器到用戶端互動。 A`StockTickerHub`類別衍生自`SignalRHub`類別會處理來自用戶端接收連線和方法呼叫。 您也需要維護的內建資料並執行`Timer`物件。 `Timer`物件將會定期觸發的用戶端連接獨立的價格更新。 您無法將這些函式放在`Hub`類別，因為中樞是暫時性。 應用程式建立`Hub`類別執行個體，每個工作中樞，連線等呼叫從用戶端到伺服器上。 所以，保持股票資料、 更新價格，並會廣播價格更新的機制都在個別的類別中執行。 您將類別命名為`StockTicker`。

![從 Stockservices.asmx 的廣播](tutorial-server-broadcast-with-signalr/_static/image3.png)

您只想的一個執行個體`StockTicker`類別，以在伺服器上執行，因此您必須設定每個參考`StockTickerHub`執行個體與單一`StockTicker`執行個體。 `StockTicker`的類別必須將廣播給用戶端，因為它具有內建的資料，以及觸發更新，但`StockTicker`不是`Hub`類別。 `StockTicker`類別必須取得 SignalR 中樞的連接內容物件的參考。 它可以接著使用 SignalR 連線內容物件廣播給用戶端。

#### <a name="create-stocktickerhubcs"></a>Create StockTickerHub.cs

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-SignalR.StockTicker**，選取**已安裝** > **Visual C#**   >  **Web** >  **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。

1. 將類別命名為*StockTickerHub*並將它新增至專案。

    這個步驟會建立*StockTickerHub.cs*類別檔案。 同時，它會新增一組指令碼檔案和組件參考加入專案中支援 SignalR。

1. 中的程式碼取代*StockTickerHub.cs*這段程式碼檔案：

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. 儲存檔案。

應用程式會使用[中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別來定義用戶端可以呼叫在伺服器的方法。 您定義一種方法： `GetAllStocks()`。 用戶端一開始會連接到伺服器，它會呼叫這個方法，以取得所有與目前的價格股票的清單。 此方法可以同步執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。

如果方法必須取得資料所做的事會牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，以啟用非同步處理。 如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-以非同步方式執行的時機](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)。

`HubName`屬性會指定應用程式會列印文件的參考，請在用戶端的 JavaScript 程式碼中的中樞。 在用戶端上的預設名稱，如果您未使用這個屬性，是 camelCase 版本的類別名稱，在此情況下會`stockTickerHub`。

因為您稍後所見，當您建立`StockTicker`類別，應用程式建立該類別的單一執行個體在它的靜態`Instance`屬性。 該單一執行個體`StockTicker`無論多少個用戶端連線或中斷連線的記憶體中。 該執行個體是什麼`GetAllStocks()`方法會使用來傳回目前的股票資訊。

#### <a name="create-stocktickercs"></a>建立 StockTicker.cs

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **類別**。

1. 將類別命名為*Stockservices.asmx*並將它新增至專案。

1. 中的程式碼取代*StockTicker.cs*這段程式碼檔案：

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

由於所有執行緒將都執行相同的執行個體的 Stockservices.asmx 的程式碼，Stockservices.asmx 類別必須是安全執行緒。

### <a name="examine-the-server-code"></a>檢查伺服端程式碼

如果您檢查伺服端程式碼時，它會協助您了解應用程式的運作方式。

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>將單一執行個體儲存在靜態欄位

程式碼會初始化靜態`_instance`支援欄位`Instance`屬性類別的執行個體。 建構函式為私用，因為它是唯一的執行個體之類別的應用程式可以建立。 應用程式會使用[延遲初始設定](/dotnet/framework/performance/lazy-initialization)如`_instance`欄位。 它不是基於效能考量。 它是確保執行個體的建立是安全執行緒。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

用戶端連線到伺服器，每次在個別的執行緒中執行的 StockTickerHub 類別的新執行個體取得 Stockservices.asmx 單一執行個體，從`StockTicker.Instance`靜態屬性，為您所見稍早在`StockTickerHub`類別。

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>在 ConcurrentDictionary 中儲存內建的資料

建構函式初始化`_stocks`有些範例股票資料集合和`GetAllStocks`傳回股票。 如您稍早所見，傳回這個集合的股票`StockTickerHub.GetAllStocks`，這是中的伺服器方法`Hub`用戶端可以呼叫的類別。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

股票集合指[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的型別。 或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。

此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料，並遺失的資料，當應用程式處置`StockTicker`執行個體。 在實際的應用程式中，您會使用後端等資料存放區資料庫。

#### <a name="periodically-updating-stock-prices"></a>定期更新股票價格

建構函式啟動`Timer`定期呼叫方法，以更新股價隨機為基礎的物件。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` 呼叫`UpdateStockPrices`，哪一個階段中為 state 參數中的 null。 應用程式更新之前的價格，請上取得鎖定`_updateStockPricesLock`物件。 如果另一個執行緒正在更新價格，然後呼叫程式碼會檢查`TryUpdateStockPrice`在清單中的每個股票。 `TryUpdateStockPrice`方法會決定是否要變更的股票的價格，以及有多少變更它。 如果股票的價格變更時，應用程式會呼叫`BroadcastStockPrice`廣播到所有的股票價格變更連線的用戶端。

`_updatingStockPrices`指定的旗標[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)並確定它是安全執行緒。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

在實際的應用程式，`TryUpdateStockPrice`方法會呼叫 web 服務，以查閱價格。 在此程式碼中，應用程式會使用隨機的數字產生器隨機進行變更。

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>取得 SignalR 內容，以便 Stockservices.asmx 類別可以廣播給用戶端

因為價格變更都是來自以下`StockTicker`物件，它是需要呼叫物件`updateStockPrice`所有已連線的用戶端上的方法。 在 `Hub`類別，您有一個 API 呼叫的用戶端方法，但`StockTicker`不是衍生自`Hub`類別，並沒有任何參考`Hub`物件。 連線的用戶端，向廣播`StockTicker`類別具有取得 SignalR 內容執行個體的`StockTickerHub`類別，並使用它來呼叫用戶端上的方法。

建立單一類別執行個體，建構函式，將該參考時，此程式碼會取得 SignalR 內容的參考，並建構函式放在`Clients`屬性。

有兩個您想要一次取得內容的原因： 取得內容是昂貴的工作中，並取得它之後可確保應用程式保留的訊息傳送至用戶端預期的順序。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

取得`Clients`屬性的內容，並將它放`StockTickerClient`屬性可讓您撰寫程式碼來呼叫用戶端的方法，會看起來相同`Hub`類別。 若要廣播到所有用戶端可以撰寫比方說， `Clients.All.updateStockPrice(stock)`。

`updateStockPrice`方法，您會在呼叫`BroadcastStockPrice`尚不存在。 稍後當您撰寫的用戶端執行的程式碼時，會將其新增。 您可以參考`updateStockPrice`這裡因為`Clients.All`是動態，這表示應用程式將會評估運算式在執行階段。 SignalR 當這個方法呼叫執行時，會傳送的方法名稱和參數值，用戶端，而且如果用戶端有一個名為方法`updateStockPrice`，應用程式會呼叫該方法，並將參數值傳遞給它。

`Clients.All` 表示將傳送至所有用戶端。 SignalR 提供其他選項，以指定哪些用戶端或用戶端傳送至群組。 如需詳細資訊，請參閱 < [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。

### <a name="register-the-signalr-route"></a>註冊 SignalR 的路由

伺服器必須知道要攔截，並將導向至 SignalR 的 URL。 若要這樣做，請新增 OWIN 啟動類別：

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-SignalR.StockTicker**選取**已安裝** > **Visual C#**   >  **Web**和然後選取**OWIN 啟動類別**。

1. 將類別命名為*啟始*，然後選取**確定**。

1. 取代預設的程式碼中*Startup.cs*這段程式碼檔案：

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

您現在已完成設定伺服器程式碼。 在下一步 區段中，您會設定用戶端。

## <a name="set-up-the-client-code"></a>設定用戶端程式碼

在本節中，您設定用戶端執行的程式碼。

### <a name="create-the-html-page-and-javascript-file"></a>建立 HTML 網頁和 JavaScript 檔案

HTML 網頁會顯示的資料和 JavaScript 檔案將組織的資料。

#### <a name="create-stocktickerhtml"></a>建立 StockTicker.html

首先，您要加入 HTML 用戶端。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **HTML 網頁**。

1. 將檔案命名*Stockservices.asmx* ，然後選取**確定**。

1. 取代預設的程式碼中*StockTicker.html*這段程式碼檔案：

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    HTML 會建立具有五個資料行、 標頭資料列和資料列與單一儲存格跨越所有五個資料行的資料表。 資料列會顯示 「 正在載入...」 應用程式啟動時，立刻顯示。 JavaScript 程式碼將會移除該資料列，並在其位置的資料列中新增，並從伺服器擷取的內建資料。

    指定指令碼標記：

    * JQuery 指令碼檔案中。

    * SignalR core 指令碼檔案。

    * SignalR proxy 指令碼檔案。

    * 您稍後將建立 Stockservices.asmx 指令碼檔案。

    應用程式以動態方式產生 SignalR proxy 指令碼檔案。 它指定 「 signalr/中樞 」 的 URL，並定義中樞類別，在此情況下，方法的 proxy 方法`StockTickerHub.GetAllStocks`。 如果您想，您可以產生這個 JavaScript 檔案以手動方式使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)。 別忘了停用動態檔案建立在`MapHubs`方法呼叫。

1. 在 **方案總管**，展開**指令碼**。

    JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。

    > [!IMPORTANT]
    > 封裝管理員會安裝新版 SignalR 指令碼。

1. 更新程式碼區塊，以對應至專案中的指令碼檔案的版本中的指令碼參考。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下*StockTicker.html*，然後選取**設定為起始頁**。

#### <a name="create-stocktickerjs"></a>建立 StockTicker.js

現在建立 JavaScript 檔案。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **JavaScript 檔案**。

1. 將檔案命名*Stockservices.asmx* ，然後選取**確定**。

1. 新增下列程式碼*StockTicker.js*檔案：

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>檢查用戶端程式碼

如果您檢查用戶端程式碼時，它將協助您了解用戶端程式碼互動的伺服器程式碼，讓應用程式運作的方式。

#### <a name="starting-the-connection"></a>正在開始連線

`$.connection` 是指 SignalR proxy。 此程式碼取得參考的 proxy`StockTickerHub`類別，並將它放在`ticker`變數。 Proxy 名稱時所設定的名稱`HubName`屬性：

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

在檔案中的程式碼的最後一行定義所有變數和函式之後，會初始化 SignalR 連線，藉由呼叫 SignalR`start`函式。 `start`函式以非同步方式執行，並傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)。 您可以呼叫完成的函式，以指定當應用程式完成的非同步動作時要呼叫的函式。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>取得所有股票

`init`函式呼叫`getAllStocks`伺服器上的函式，並使用伺服器來更新內建的資料表所傳回的資訊。 請注意，根據預設，您必須在用戶端上將 camelCasing，即使方法名稱是依照 pascal 命名法大小寫，在伺服器上。 CamelCasing 規則僅適用於不是物件的方法。 例如，您參照`stock.Symbol`並`stock.Price`，而非`stock.symbol`或`stock.price`。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

在`init`方法，應用程式會建立 HTML 資料表資料列的每個內建物件從伺服器收到藉由呼叫`formatStock`的格式屬性`stock`物件，並藉由呼叫`supplant`來取代中的預留位置`rowTemplate`變數`stock`物件屬性值。 產生的 HTML 則會附加至內建的資料表中。

> [!NOTE]
> 您呼叫`init`傳遞，作為`callback`之後的非同步執行的函式`start`函式完成。 如果您呼叫`init`做為個別的 JavaScript 陳述式之後呼叫`start`，函式會失敗，因為它會立即執行而不需等待開始函式，來完成 建立連線。 在此情況下，`init`函式會嘗試呼叫`getAllStocks`函式應用程式會在建立伺服器連線之前。

#### <a name="getting-updated-stock-prices"></a>取得已更新的股票價格

當伺服器變更股票的價格時，它會呼叫`updateStockPrice`連線的用戶端上。 應用程式會將函式加入至用戶端屬性`stockTicker`以供呼叫至來自伺服器的 proxy。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

`updateStockPrice`函式格式的內建的物件，從伺服器接收到資料表資料列中的相同方式`init`函式。 而不是將資料列附加至資料表，它會尋找資料表中的股票的目前資料列，並取代該資料列，以新的。

## <a name="test-the-application"></a>測試應用程式

您可以測試應用程式，以確定它運作。 您會看到顯示股票的價格變動的即時的內建資料表的所有瀏覽器視窗。

1. 在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行應用程式的 播放 按鈕。

    ![使用者開啟偵錯模式，然後選取 [播放] 的螢幕擷取畫面。](tutorial-server-broadcast-with-signalr/_static/image4.png)

    瀏覽器視窗會開啟，顯示**即時股票表格**。 內建的資料表一開始會顯示 「 正在載入...」 列，然後一小段時間之後, 應用程式會顯示初始的內建資料，以及股票的價格再開始變更。

1. 從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。

    初始的內建顯示等同於第一個瀏覽器，並同時進行變更。

1. 關閉所有瀏覽器，開啟新的瀏覽器，並移至相同的 URL。

    在伺服器中執行，繼續 Stockservices.asmx 單一物件。 **即時股票表格**股票持續變更的顯示。 看不到初始資料表具有零變更數據。

1. 關閉瀏覽器。

## <a name="enable-logging"></a>啟用記錄

SignalR 有內建記錄功能，您可以讓用戶端上，以協助疑難排解。 在本節中，您可以啟用記錄，並查看範例，說明如何記錄告訴您哪一種下列傳輸方法使用 SignalR:

* [Websocket](http://en.wikipedia.org/wiki/WebSocket)、 支援的 IIS 8 和目前的瀏覽器。

* [伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。

* [永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet explorer 的支援。

* [長時間輪詢的 Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。

針對任何指定的連接，SignalR 會選擇最佳的伺服器和用戶端支援的傳輸方法。

1. 開啟*StockTicker.js*。

1. 新增此反白顯示的行程式碼來啟用記錄之前初始化的連線，在檔案結尾處的程式碼：

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. 按 **F5** 執行專案。

1. 開啟瀏覽器的開發人員工具 視窗，然後選取主控台，以查看記錄檔。 您可能必須重新整理頁面，以查看 SignalR 交涉新連線的傳輸方法的記錄檔。

    * 如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，則傳輸方法就**WebSockets**。

    * 如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，則傳輸方法就**iframe**。

    * 如果您在 Windows 8 (IIS 8) 來執行 Firefox 19，則傳輸方法就**WebSockets**。

        > [!TIP]
        > 在 Firefox 中安裝 Firebug 增益集以取得主控台視窗。

    * 如果您在 Windows 7 (IIS 7.5) 來執行 Firefox 19，則傳輸方法就**伺服器傳送**事件。

## <a name="install-the-stockticker-sample"></a>安裝 Stockservices.asmx 範例

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample)安裝 Stockservices.asmx 應用程式。 NuGet 套件會包含更多的功能比您從頭開始建立簡化版本。 在本節的教學課程中，您要安裝 NuGet 套件，並檢閱新功能，以及實作它們的程式碼。

> [!IMPORTANT]
> 如果您不需要執行本教學課程的先前步驟中安裝套件，您必須新增 OWIN 啟動類別至您的專案。 這個 NuGet 套件的 readme.txt 檔案會說明此步驟。

### <a name="install-the-signalrsample-nuget-package"></a>安裝 SignalR.Sample NuGet 套件

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [管理 NuGet 套件]。

1. 在  **NuGet 套件管理員：SignalR.StockTicker**，選取**瀏覽**。

1. 從**套件來源**，選取**nuget.org**。

1. 請輸入*SignalR.Sample*中的搜尋方塊，然後選取**Microsoft.AspNet.SignalR.Sample** > **安裝**。

1. 在 **方案總管**，展開*SignalR.Sample*資料夾。

    安裝 SignalR.Sample 封裝建立的資料夾和其內容。

1. 在  *SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後選取**設定為起始頁**。

    > [!NOTE]
    > 安裝 SignalR.Sample NuGet 套件可能會變更您在的 jQuery 版本您*指令碼*資料夾。 新*StockTicker.html*套件會安裝中的檔案*SignalR.Sample*資料夾將會是套件會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記中的 jQuery 參考。

### <a name="run-the-application"></a>執行應用程式

 您在第一個應用程式中看到的資料表有實用的功能。 完整股票行情指示器應用程式會顯示新功能： 水平捲動中視窗，顯示的股票資料和變更色彩，如增加，而切換的股票。

1. 按下 **F5** 即可執行應用程式。

     當您第一次執行應用程式時，「 市場 」 的動作 「 關閉 」，您會看到靜態表格和不捲動的股票行情指示器視窗。

1. 選取  **Open Market**。

    ![即時股票行情指示器螢幕擷取畫面。](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * **即時股票行情即時看板**開始的水平捲動方塊，且伺服器會開始定期廣播隨機的方式上的股票價格變更。

    * 每次股票的價格變更時，應用程式更新這兩個**即時股票表格**並**即時股票行情即時看板**。

    * 正數股票的價格變動時，應用程式會顯示具有綠色背景股票。

    * 當變更為負數時，應用程式會顯示具有紅色背景股票。

1. 選取 **關閉市場**。

    * 資料表更新停止。

    * 股票行情指示器會停止捲動。

1. 選取 **重設**。

    * 所有內建的資料會重設。

    * 應用程式還原之前啟動的價格變動的初始狀態。

1. 從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。

1. 您會看到相同的資料，動態更新在此同時在每個瀏覽器中。

1. 當您選取的任何控制項時，則所有瀏覽器會回應相同的方式在相同的時間。

### <a name="live-stock-ticker-display"></a>即時股票行情指示器顯示

**即時股票行情即時看板**顯示為中的未排序的清單`<div>`項目格式化成單一行的 CSS 樣式。 應用程式初始化，並更新股票行情指示器與資料表相同的方式： 藉由取代中的預留位置`<li>`範本字串，並以動態方式新增`<li>`項目`<ul>`項目。 應用程式包含使用 jQuery 捲動`animate`函式來變更邊界內的未排序清單左邊`<div>`。

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

股票行情即時看板 HTML 程式碼：

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

股票行情即時看板 CSS 程式碼：

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

JQuery 程式碼，可讓捲動：

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>用戶端可以呼叫的伺服器上的其他方法

若要新增至應用程式的彈性，還有一些應用程式可以呼叫的其他方法。

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

`StockTickerHub`類別會定義四個用戶端可以呼叫其他方法：

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

應用程式呼叫`OpenMarket`， `CloseMarket`，和`Reset`回應位於頁面頂端的按鈕。 它們示範觸發立即傳播到所有用戶端的狀態變更的一部用戶端的模式。 每個方法呼叫中的方法`StockTicker`類別，會使市場狀態變更，然後再廣播的新狀態。

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

在 `StockTicker`類別，應用程式會維護狀態的市場`MarketState`屬性，傳回`MarketState`列舉值：

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

每個市場狀態變更的方法執行這項操作的鎖定區塊內因為`StockTicker`類別必須是安全執行緒：

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

若要確定此程式碼是安全執行緒`_marketState`支援欄位`MarketState`屬性指定`volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

`BroadcastMarketStateChange`和`BroadcastMarketReset`方法很類似您已經看到 BroadcastStockPrice 方法，但在呼叫不同的方法，在用戶端定義：

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>用戶端可以呼叫伺服器上的其他函式

`updateStockPrice`函式現在會處理資料表和股票行情指示器顯示，並使用`jQuery.Color`閃爍紅色和綠色的色彩。

中的新函式*SignalR.StockTicker.js*啟用和停用根據市場狀態的按鈕。 它們也停止或啟動**即時股票行情即時看板**水平捲動。 因為許多函式加入至`ticker.client`，應用程式會使用[jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)將它們加入。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>建立連線後的其他用戶端安裝程式

用戶端建立連接之後，它會有一些額外的工作，執行：

* 了解市場上是否已開啟或關閉呼叫適當`marketOpened`或`marketClosed`函式。

* 附加至按鈕的伺服器方法呼叫。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

伺服器方法未連線到之前的按鈕中之後應用程式建立連接。 它是讓程式碼無法呼叫伺服器方法，才能提供。

## <a name="additional-resources"></a>其他資源

在本教學課程中，您已了解如何撰寫程式將訊息從伺服器廣播到所有已連線的用戶端的 SignalR 應用程式。 現在您可以廣播定期執行，以回應通知從任何用戶端的訊息。 您可以使用多執行緒的單一執行個體的概念來維護多玩家線上遊戲案例中的伺服器狀態。 如需範例，請參閱[ShootR 遊戲根據 SignalR](https://github.com/NTaylorMullen/ShootR)。

顯示對等通訊案例的教學課程，請參閱[開始使用 SignalR](introduction-to-signalr.md)並[即時更新與 SignalR](tutorial-high-frequency-realtime-with-signalr.md)。

如需 SignalR 相關資訊，請參閱下列資源：

* [ASP.NET SignalR](../../index.md)
* [SignalR 專案](http://signalr.net/)
* [SignalR GitHub 和範例](https://github.com/SignalR/SignalR)
* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立專案
> * 設定伺服器程式碼
> * 檢查伺服端程式碼
> * 設定用戶端程式碼
> * 檢查用戶端程式碼
> * 測試應用程式
> * 啟用的記錄

請前往下一篇文章，以了解如何建立使用 ASP.NET SignalR 2 的即時 web 應用程式。
> [!div class="nextstepaction"]
> [使用 SignalR 建立即時 web 應用程式](real-time-web-applications-with-signalr.md)