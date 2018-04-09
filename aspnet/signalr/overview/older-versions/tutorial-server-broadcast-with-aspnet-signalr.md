---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 教學課程： 透過 ASP.NET SignalR 的伺服器廣播 1.x |Microsoft 文件
author: pfletcher
description: 本教學課程會示範如何建立使用 ASP.NET SignalR 提供廣播的伺服器功能的 web 應用程式。 伺服器廣播的方法亦 communic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/10/2013
ms.topic: article
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 85d40e411a7ff974da5cc4fa7fbd789b83d92201
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>教學課程： 透過 ASP.NET SignalR 的伺服器廣播 1.x
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本教學課程會示範如何建立使用 ASP.NET SignalR 提供廣播的伺服器功能的 web 應用程式。 伺服器廣播表示傳送至用戶端的通訊由伺服器起始。 此案例會需要不同的程式設計方法，與對等案例，例如交談應用程式，在其中傳送至用戶端通訊所起始的一或多個用戶端。
> 
> 您會在本教學課程中建立的應用程式會模擬股票行情指示器伺服器廣播功能的常見情況。
> 
> 在本教學課程的註解是歡迎畫面。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com)。


## <a name="overview"></a>總覽

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 封裝會安裝 Visual Studio 專案中的範例模擬股票行情指示器應用程式。 在本教學課程的第一個部分中，您會從頭開始建立該應用程式的簡化的版本。 本教學課程的其餘部分，將安裝 NuGet 套件，並檢閱程式碼所建立的其他功能。

股票行情指示器應用程式是一種要在其中您定期 「 推入 」 或廣播，通知所有連線的用戶端與伺服器的即時應用程式是代表值。

在本教學課程的第一個部分中，您將建置的應用程式會顯示股票資料的方格。

![StockTicker 初始版本](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

定期伺服器隨機更新股價，並將更新推送至所有連線的用戶端。 在瀏覽器的數字和符號**變更**和**%**動態變更通知來回應來自伺服器的資料行。 如果您開啟其他瀏覽器中，相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。

本教學課程包含下列各節：

- [必要條件](#prerequisites)
- [建立專案](#createproject)
- [將 SignalR NuGet 封裝](#nugetpackages)
- [設定伺服器程式碼](#server)
- [設定用戶端程式碼](#client)
- [測試應用程式](#test)
- [啟用記錄](#enablelogging)
- [安裝並檢閱完整 StockTicker 範例](#fullsample)
- [後續步驟](#nextsteps)

> [!NOTE]
> 如果您不想要逐步完成建立應用程式的步驟，您可以在新安裝 SignalR.Sample 封裝**空的 ASP.NET Web 應用程式**專案，然後執行下列步驟以取得說明程式碼的讀取。 本教學課程的第一個部分涵蓋 SignalR.Sample 程式碼的子集，而且第二個部分說明 SignalR.Sample 封裝中的其他功能的主要功能。


<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

開始之前，請確定您有 Visual Studio 2012 或 2010 SP1 安裝在電腦上。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express for Web。

如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。

<a id="createproject"></a>

## <a name="create-the-project"></a>建立專案

1. 從**檔案**功能表上，按一下**新專案**。
2. 在**新專案**對話方塊方塊中，展開  **C#**下**範本**選取**Web**。
3. 選取**ASP.NET 空 Web 應用程式**範本，將專案*SignalR.StockTicker*，然後按一下**確定**。

    ![新增專案對話方塊](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>將 SignalR NuGet 封裝

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>將 SignalR 和 JQuery NuGet 封裝

您可以將加入專案的 SignalR 功能安裝 NuGet 套件。

1. 按一下**工具 |程式庫套件管理員 |Package Manager Console**。
2. 封裝管理員中，輸入下列命令。

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    SignalR 套件會安裝其他的 NuGet 套件的數字，做為相依性。 安裝完成時可能會有所有 SignalR 使用 ASP.NET 應用程式所需的伺服器和用戶端元件。

<a id="server"></a>

## <a name="set-up-the-server-code"></a>設定伺服器程式碼

本節中您設定在伺服器執行的程式碼。

### <a name="create-the-stock-class"></a>建立庫存類別

您開始建立您要用以儲存及傳輸內建的相關資訊的內建模型類別。

1. 在專案資料夾中建立新的類別檔案，其命名*Stock.cs*，然後將範本程式碼取代下列程式碼：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    您將設定時建立的股票的兩個屬性為符號 (例如，microsoft MSFT) 和價格。 其他屬性取決於您如何及何時將價格。 第一次設定價格的值取得傳播到 DayOpen。 後續的階段，當您設定的價格變更，且會計算 PercentChange 屬性值會根據價格和 DayOpen 之間的差異。

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>建立 StockTicker 和 StockTickerHub 類別

您將使用 SignalR 中樞應用程式開發介面來處理伺服器到用戶端互動。 StockTickerHub 衍生的類別，從 SignalR 中樞類別將會接收來自用戶端連線和方法呼叫來處理。 您也需要維護的內建資料及執行以定期觸發獨立用戶端連線的價格更新計時器物件。 因為暫時性中樞執行個體，您無法暫停在中樞類別中，這些函式。 在中樞內，例如連線和伺服器從用戶端呼叫每個作業會建立的 Hub 類別的執行個體。 因此，會將內建的資料保存、 更新價格，接著在廣播價格更新的機制，都必須在個別的類別，您會命名 StockTicker 執行。

![從 StockTicker 廣播](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

您只想要執行的伺服器上，因此您必須設定每個 StockTickerHub 執行個體從單一 StockTicker 執行個體參考的 StockTicker 類別的一個執行個體。 StockTicker 類別必須廣播至用戶端，因為它已內建的資料，並觸發更新，但 StockTicker 不是中樞類別。 因此，StockTicker 類別必須取得 SignalR 中樞的連線內容物件的參考。 它然後可以使用廣播至用戶端的 SignalR 連線的內容物件。

1. 在**方案總管 中**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。
2. 如果您有 Visual Studio 2012 [ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=279941)，按一下  **Web**下**Visual C#**選取**SignalR 中樞類別**項目範本。 否則，請選取**類別**範本。
3. 將新類別*StockTickerHub.cs*，然後按一下 **新增**。

    ![新增 StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. 取代為下列程式碼中的範本程式碼：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    [中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別用來定義用戶端可以呼叫在伺服器的方法。 您要定義一種方法： `GetAllStocks()`。 用戶端一開始會連接到伺服器，它會呼叫這個方法，取得所有其目前的價格股票的清單。 方法可以以同步方式執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。 如果方法具有以取得資料所做的事，牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，若要啟用非同步處理。 如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-時以非同步方式執行](index.md)。

    HubName 屬性指定中樞的用戶端上的 JavaScript 程式碼中的參考方式。 如果您不使用這個屬性在用戶端上的預設名稱是依照 camel 命名法的大小寫慣例版本的類別名稱，因此在此情況下會 stockTickerHub。

    當您建立 StockTicker 類別時，您會看到更新版本，該類別的單一執行個體被建立靜態執行個體屬性中。 不論多少用戶端連接或中斷連線、 的記憶體中保留 StockTicker 單一執行個體，該執行個體供 GetAllStocks 方法用來傳回目前的股票資訊。
5. 在專案資料夾中建立新的類別檔案，其命名*StockTicker.cs*，然後將範本程式碼取代下列程式碼：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    由於多個執行緒會執行相同的執行個體的 StockTicker 程式碼，StockTicker 類別必須是 threadsafe。

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>將單一執行個體儲存在靜態欄位

    程式碼會初始化靜態\_支援類別，而這樣的執行個體的執行個體內容的執行個體欄位是唯一的執行個體，可以建立的類別，因為標示為私用建構函式。 [延遲初始設定](https://msdn.microsoft.com/library/dd997286.aspx)用於\_不基於效能考量，但若要確定執行個體建立 threadsafe 的執行個體欄位。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    每次用戶端連接到伺服器，執行不同的執行緒中 StockTickerHub 類別的新執行個體取得 StockTicker 單一執行個體從 StockTicker.Instance 靜態屬性，如同稍早在 StockTickerHub 類別中。

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>在 ConcurrentDictionary 中儲存庫存資料

    建構函式初始化\_股票集合會以一些內建資料範例和 GetAllStocks 傳回股票。 如稍早所見，StockTickerHub.GetAllStocks 也就是在用戶端可以呼叫的中樞類別中的伺服器方法接著就會傳回這個集合的股票。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    股票集合定義為[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的類型。 或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。

    此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料並處置 StockTicker 執行個體時，遺失的資料。 在實際應用您可能會使用後端資料存放區，例如資料庫。

    ### <a name="periodically-updating-stock-prices"></a>定期更新股票價格

    建構函式會啟動計時器物件定期呼叫更新股價隨機為基礎的方法。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices 計時器，state 參數中的 null 會傳入所呼叫。 更新之前的價格，採用的鎖定\_updateStockPricesLock 物件。 如果另一個執行緒已經正在更新價格，然後在清單中的每種股票呼叫 TryUpdateStockPrice，會檢查程式碼。 TryUpdateStockPrice 方法會決定是否要變更的股價，以及有多少變更它。 如果變更的股價，BroadcastStockPrice 稱為股票價格變更廣播給所有連接的用戶端。

    \_UpdatingStockPrices 旗標標示為[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)以確保其存取權是 threadsafe。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    在實際的應用程式，TryUpdateStockPrice 方法會呼叫 web 服務以查閱價格;在此程式碼中，它會使用隨機號碼產生器隨機進行變更。

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>取得 SignalR 內容，以便 StockTicker 類別可以廣播至用戶端

    因為價格變更以下來自 StockTicker 物件中，這是需要 updateStockPrice 方法呼叫所有已連線的用戶端上的物件。 中樞類別中您的應用程式開發介面呼叫用戶端方法，但不是衍生自中樞類別 StockTicker，而且沒有任何中樞物件的參考。 因此，若要廣播至連線的用戶端，StockTicker 類別必須 StockTickerHub 類別取得 SignalR 內容執行個體，然後使用它來呼叫用戶端上的方法。

    建立單一類別執行個體，建構函式，將該參考時，程式碼會取得 SignalR 內容的參考，建構函式將它放在用戶端屬性。

    有兩個您想要一次取得內容的原因： 取得內容是昂貴的作業，並取得它一次可確保預期的順序傳送至用戶端的訊息會保留。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    取得內容的用戶端屬性，並將它放在 StockTickerClient 屬性可讓您撰寫程式碼來呼叫用戶端方法看起來相同一樣中樞類別中。 比方說，若要廣播到所有用戶端，您可以撰寫 Clients.All.updateStockPrice(stock)。

    您要呼叫中 BroadcastStockPrice updateStockPrice 方法不存在。稍後當您撰寫的用戶端執行的程式碼時，會將其新增。 您可以參考以下 updateStockPrice 因為 Clients.All 是動態，這表示會在執行階段評估的運算式。 SignalR 呼叫此方法執行時，會傳送的方法名稱和參數值給用戶端，以及如果用戶端具有名為 updateStockPrice 的方法，該方法會呼叫和參數值會傳遞給它。

    Clients.All 表示傳送給所有用戶端。 SignalR 提供其他選項來指定哪些用戶端或用戶端傳送到群組。 如需詳細資訊，請參閱[HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。

### <a name="register-the-signalr-route"></a>註冊的 SignalR 的路由

伺服器必須知道要攔截並導向 SignalR 的 URL。 若要如何將一些程式碼來*Global.asax*檔案。

1. 在**方案總管 中**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。
2. 選取**全域應用程式類別**項目範本，然後再按一下**新增**。

    ![新增 global.asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. 將 SignalR 的路由登錄程式碼加入至應用程式\_Start 方法：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    根據預設，所有 SignalR 流量的基底 URL 是:"/ signalr 」，和 「 / signalr/中心 」 用來擷取動態產生的 JavaScript 檔案，定義您在您的應用程式的所有主機的 proxy。 MapHubs 方法包含可讓您指定不同的基底 URL 和 SignalR 的特定選項的執行個體中的多載[HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx)類別。
4. 加入 using 陳述式在檔案頂端：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. 儲存並關閉*Global.asax*檔案，然後建置專案。

您現在已完成設定伺服器程式碼。 下一節中您會設定用戶端。

<a id="client"></a>

## <a name="set-up-the-client-code"></a>設定用戶端程式碼

1. 新的 HTML 檔案的資料夾中建立專案，並將其命名*StockTicker.html*。
2. 取代為下列程式碼中的範本程式碼：

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    HTML 5 個資料行、 標頭資料列與資料列跨越所有 5 個資料行的單一資料格與建立資料表。 資料列會顯示 「 載入...」 和，才會顯示在應用程式啟動時，立刻顯示。 JavaScript 程式碼將會移除該資料列，並從伺服器擷取的內建資料進行資料列中加入。

    指令碼標記指定 jQuery 指令碼檔案、 SignalR core 指令碼檔案、 SignalR proxy 指令碼檔案及更新版本，您將建立 StockTicker 指令碼檔案。 SignalR proxy 指令碼檔案，指定 「 signalr/中樞 」 URL，以動態方式產生，並在中樞類別、 方法的 proxy 方法定義在此情況下 StockTickerHub.GetAllStocks。 如果您想要的話，您可以產生這個 JavaScript 檔案手動使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)和停用動態檔案建立 MapHubs 方法呼叫中的。
3. > [!IMPORTANT]
   > 請確定 JavaScript 檔案參考中*StockTicker.html*正確無誤。 也就是確定您的指令碼標記 (在範例 1.8.2) 中的 jQuery 版本是在您的專案中的 jQuery 版本相同*指令碼*資料夾，並確定指令碼標記中的 SignalR 版本是 SignalR 相同在您的專案版本*指令碼*資料夾。 如有必要，請變更指令碼標記中的檔案名稱。
4. 在**方案總管] 中**，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 [**設定為起始頁**。
5. 在專案資料夾中建立新的 JavaScript 檔案並將其命名*StockTicker.js*...
6. 取代為下列程式碼中的範本程式碼：

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $.connection 指 SignalR proxy。 程式碼 StockTickerHub 類別取得 proxy 的參考，並將它放在股票行情指示器變數中。 Proxy 名稱是由 [HubName] 屬性已設定的名稱：

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    定義所有變數和函式後，檔案中的程式碼的最後一行會初始化 SignalR 連線，藉由呼叫 SignalR 啟始函式。 啟始函式會以非同步方式執行，並傳回[jQuery 延期物件](http://api.jquery.com/category/deferred-object/)，這表示您可以呼叫完成的函式，以指定要在完成非同步作業時呼叫的函式...

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    Init 函式呼叫 getAllStocks 函式在伺服器上，並使用伺服器來更新資料表內建傳回的資訊。 請注意，根據預設，需要使用 camel 命名法的大小寫的用戶端上雖然方法名稱是依照 pascal 命名法的大小寫慣例在伺服器上。 依照 camel 命名法的大小寫規則只適用於不是物件的方法。 例如，您參考內建。符號 」 與 「 存貨。價格、 不 stock.symbol 或 stock.price。

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    如果您想要在用戶端，使用 pascal 大小寫，或如果您想要使用完全不同的方法名稱，您無法裝飾中樞方法 HubMethodName 屬性具有相同的方式會裝飾中樞類別具有 HubName 屬性。

    Init 方法中，在 HTML 資料表資料列會建立每個內建物件從伺服器收到的呼叫 formatStock 格式內容的內建物件，並藉由呼叫取代 (定義在頂端*StockTicker.js*) 來取代內建物件屬性值的預留位置 rowTemplate 變數中的。 產生的 HTML 則會附加至內建的資料表中。

    您可以在傳遞做為非同步啟始函式完成之後執行的回呼函式呼叫 init。 如果您呼叫初始化為個別的 JavaScript 陳述式之後呼叫開始時，函式會失敗，因為它會立即執行而不需等待啟始函式，以完成建立連線。 在此情況下，init 函式會嘗試建立伺服器連接之前呼叫 getAllStocks 函式。

    當伺服器變更股價時，它會呼叫 updateStockPrice 連線的用戶端上。 此函式加入 stockTicker proxy 的用戶端屬性，以便將它提供給呼叫從伺服器。

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    UpdateStockPrice 函式會將收到來自伺服器的資料表資料列到 init 函式的相同方式將內建物件格式化。 不過，而不是將資料列附加至資料表，資料表中尋找股票的目前資料列，以新的取代該資料列。

<a id="test"></a>

## <a name="test-the-application"></a>測試應用程式

1. 按 F5 以偵錯模式執行應用程式。

    內建資料表一開始會顯示 「 載入...」 列，然後顯示初始的內建資料，在短暫延遲之後，再股票價格開始變更。

    ![正在載入](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![初始的內建資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![從伺服器接收變更內建的資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. 從瀏覽器網址列複製 URL，並將它貼到一或多個新的瀏覽器視窗。

    初始的內建顯示等同於第一個瀏覽器，並同時進行變更。
3. 關閉所有瀏覽器並開啟新的瀏覽器，然後移至相同的 URL。

    在伺服器中執行，因此則內建的資料表會顯示股票仍繼續變更仍持續 StockTicker 單一物件。 （您未看到變更數據的初始資料表有零）。
4. 關閉瀏覽器。

<a id="enablelogging"></a>

## <a name="enable-logging"></a>啟用記錄

SignalR 有內建的記錄功能，您可以啟用用戶端上，以協助疑難排解。 本節中您要啟用記錄，並查看範例，說明如何記錄告訴您正在使用 SignalR 的下列傳輸方法：

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket)、 IIS 8 及目前的瀏覽器所支援。
- [伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。
- [永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet Explorer 受支援。
- [Ajax 長期輪詢](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。

任何指定的連接，SignalR 選擇最佳伺服器和用戶端支援的傳輸方法。

1. 開啟*StockTicker.js*並加入一行程式碼可以立即初始化檔案結尾處的連接的程式碼之前的記錄：

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. 按 F5 執行專案。
3. 開啟瀏覽器的開發人員工具視窗，然後選取主控台，以查看記錄檔。 您可能必須重新整理頁面，請參閱記錄檔的 Signalr 交涉新連線的傳輸方法。

    如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，傳輸方法是 WebSockets。

    ![IE 10 IIS 8 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，傳輸方法是 iframe。

    ![IE 10 個主控台中，IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    在 Firefox 安裝 firebug 這類增益集以取得主控台視窗。 如果您在 Windows 8 (IIS 8) 執行 Firefox 19，傳輸方法是 WebSockets。

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    如果您在 Windows 7 (IIS 7.5) 上執行 Firefox 19，伺服器傳送的事件所傳輸方法。

    ![Firefox 19 IIS 7.5 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>安裝並檢閱完整 StockTicker 範例

StockTicker 應用程式所安裝[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件會包含更多的功能，比您剛從頭建立簡化版本。 在本章節的教學課程中，您可以安裝 NuGet 套件，並檢閱的新功能以及實作它們的程式碼。

### <a name="install-the-signalrsample-nuget-package"></a>安裝 SignalR.Sample NuGet 套件

1. 在**方案總管 中**，以滑鼠右鍵按一下專案，然後按一下**管理 NuGet 封裝**。
2. 在**管理 NuGet 封裝**對話方塊中，按一下 **線上**，輸入*SignalR.Sample*中**線上搜尋**方塊，然後再按一下  **安裝**中**SignalR.Sample**封裝。

    ![安裝 SignalR.Sample 套件](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. 在*Global.asax*檔案，標記為註解 RouteTable.Routes.MapHubs(); 行您稍早在應用程式中加入\_Start 方法。

    中的程式碼*Global.asax* SignalR.Sample 封裝暫存器中的 SignalR 路由因為不再需要*應用程式\_Start/RegisterHubs.cs*檔案：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    組件屬性所參考的 WebActivator 類別併入 WebActivatorEx NuGet 封裝，它會安裝為 SignalR.Sample 封裝的相依性。
4. 在**方案總管 中**，依序展開*SignalR.Sample*安裝 SignalR.Sample 封裝所建立的資料夾。
5. 在*SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。

    > [!NOTE]
    > 安裝 SignalR.Sample NuGet 封裝可能會變更您在的 jQuery 新版您*指令碼*資料夾。 新*StockTicker.html*檔案，此封裝會安裝在*SignalR.Sample*資料夾將會是與此封裝會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記的 jQuery 參照。

### <a name="run-the-application"></a>執行應用程式

1. 按 F5 執行應用程式。

    方格中稍早所見，除了完整股票行情指示器應用程式會顯示水平捲動視窗，以顯示相同的內建資料。 當您執行第一次應用程式時，「 市集 」 「 關閉 」，您看到靜態格線和不捲動的股票行情指示器視窗。

    ![StockTicker 螢幕開始](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    當您按一下**開啟市場**、 **Live 股票行情即時看板**方塊開始的水平捲動，並在伺服器啟動定期廣播股票價格變更隨機的方式。 每次股票價格變更，同時**Live 存貨資料表**格線和**Live 股票行情即時看板**方塊就會更新。 具有綠色背景，正股票價格變動時，會顯示股票和股票時變更為負數，會顯示具有紅色背景。

    ![StockTicker 應用程式中，開啟 市場](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    **關閉市場**按鈕就會停止所做的變更，並停止股票行情指示器捲動和**重設**按鈕會重設所有內建的資料還原為初始狀態啟動價格變更之前。 如果您開啟多個瀏覽器視窗，並移至相同的 URL，您會看到相同的資料，動態更新在每個瀏覽器中相同的時間。 當您按一下其中一個按鈕時，所有瀏覽器會回應相同的方式，在相同的時間。

### <a name="live-stock-ticker-display"></a>即時股票行情指示器顯示

**Live 股票行情即時看板**顯示是 div 項目的 CSS 樣式格式化成單一行中的未排序的清單。 股票行情指示器會初始化並更新資料表的相同方式來： 取代中的預留位置&lt;li&gt;範本字串，並以動態方式加入&lt;li&gt;項目&lt;ul&gt;項目。 使用 jQuery 動畫函式來改變的未排序的清單內 div.margin-left 執行捲動

股票行情即時看板 HTML:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

股票行情即時看板 CSS:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

JQuery 程式碼，可捲動：

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>用戶端可以呼叫在伺服器上的其他方法

StockTickerHub 類別會定義四種用戶端可以呼叫的其他方法：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket、 CloseMarket 和重設會呼叫以回應位於頁面頂端的按鈕。 它們也示範一部用戶端觸發立即傳播到所有用戶端的狀態變更的模式。 每一種方法呼叫中的方法 StockTicker 類別會影響市場狀態變更，然後再廣播的新狀態。

在 StockTicker 類別中，傳回 MarketState 列舉值的 MarketState 屬性來維護市場的狀態：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

每個市場狀態變更的方法執行這項操作的鎖定區塊內因為 StockTicker 類別必須 threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

若要確保此程式碼是 threadsafe， \_marketState 欄位支援 MarketState 屬性標記為 volatile，

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

BroadcastMarketStateChange 和 BroadcastMarketReset 方法是已經看到的 BroadcastStockPrice 方法類似，只不過它們會呼叫不同的方法在用戶端定義：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>伺服器可以呼叫的用戶端上的其他函式

UpdateStockPrice 函式現在會處理格線和股票行情指示器顯示，並且會使用 jQuery.Color 閃爍紅色和綠色的色彩。

中的新函式*SignalR.StockTicker.js*啟用和停用按鈕根據市場的狀態，並在停止或啟動股票行情指示器視窗水平捲軸。 因為多個函式加入至 ticker.client， [jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)用來將其加入。

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>建立連接後的其他用戶端安裝

在用戶端建立連接之後，它有一些額外的工作，以執行： 找出市場是否開啟或關閉才能呼叫適當的 marketOpened 或 marketClosed 函式，並附加至按鈕的伺服器方法呼叫。

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

伺服器方法都不連線到直到按鈕之後建立的連線時，使程式碼無法嘗試呼叫伺服器方法，才可供。

<a id="nextsteps"></a>

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已學會如何程式從伺服器廣播訊息，所有已連線的用戶端，定期執行並以回應通知來自任何用戶端的 SignalR 應用程式。 使用多執行緒的單一執行個體維護伺服器狀態的模式也也可用在多人線上遊戲案例中。 如需範例，請參閱[ShootR 遊戲，取決於 SignalR](https://github.com/NTaylorMullen/ShootR)。

如需顯示對等通訊案例的教學課程，請參閱[入門 SignalR](index.md)和[即時更新與 SignalR](index.md)。

深入了解更多進階的 SignalR 開發概念，請造訪下列網站 SignalR 原始碼和資源：

- [ASP.NET SignalR](https://asp.net/signalr/)
- [SignalR 專案](http://signalr.net/)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
