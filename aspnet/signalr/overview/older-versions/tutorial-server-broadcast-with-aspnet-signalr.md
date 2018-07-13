---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 教學課程： 伺服器廣播與 ASP.NET SignalR 1.x |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立使用 ASP.NET SignalR 來提供伺服器廣播的功能的 web 應用程式。 伺服器廣播方法，communic...
ms.author: aspnetcontent
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 1f98b35236812aac1362f1e36e60971ff8d896bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816190"
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>教學課程： 伺服器廣播與 ASP.NET SignalR 1.x
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)

> 本教學課程會示範如何建立使用 ASP.NET SignalR 來提供伺服器廣播的功能的 web 應用程式。 伺服器廣播表示傳送至用戶端的通訊由伺服器起始。 這種情況下需要不同的程式設計方法，比對等案例，例如交談應用程式，在其中傳送到用戶端的通訊所起始的一或多個用戶端。
> 
> 您會在本教學課程中建立的應用程式會模擬股票行情即時看板，伺服器廣播功能的一般案例。
> 
> 在本教學課程的註解是歡迎畫面。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com)。


## <a name="overview"></a>總覽

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件會安裝 Visual Studio 專案中的模擬的範例股票行情指示器應用程式。 在本教學課程的第一個部分中，您會從頭開始建立該應用程式的簡化的版本。 在本教學課程的其餘部分，您將安裝 NuGet 套件，並檢閱它所建立的程式碼的其他功能。

股票行情指示器應用程式是一種您想要在其中定期 「 推入 」 或廣播，通知所有連線的用戶端與伺服器的即時應用程式的代表值。

在本教學課程的第一個部分中，您將建置的應用程式會顯示方格，以使用內建的資料。

![Stockservices.asmx 的初始版本](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

定期伺服器隨機更新股價，並將更新推送至所有連線的用戶端。 在瀏覽器的數字和符號**變更**並**%** 動態變更通知來回應來自伺服器的資料行。 如果您開啟其他瀏覽器相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。

本教學課程包含下列各節：

- [必要條件](#prerequisites)
- [建立專案](#createproject)
- [將 SignalR NuGet 封裝](#nugetpackages)
- [設定伺服器程式碼](#server)
- [設定用戶端程式碼](#client)
- [測試應用程式](#test)
- [啟用記錄](#enablelogging)
- [安裝並檢閱完整的 Stockservices.asmx 範例](#fullsample)
- [後續步驟](#nextsteps)

> [!NOTE]
> 如果您不想要逐步建置應用程式的步驟，您可以在新安裝 SignalR.Sample 封裝**空的 ASP.NET Web 應用程式**專案，然後執行下列步驟以取得說明程式碼的讀取。 本教學課程的第一個部分涵蓋 SignalR.Sample 程式碼的子集，並第二個部分說明 SignalR.Sample 封裝中的其他功能的主要功能。


<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

在開始之前，請確定您有 Visual Studio 2012 或 2010 SP1 安裝在您的電腦上。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得的免費 Visual Studio 2012 Express for Web。

如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。

<a id="createproject"></a>

## <a name="create-the-project"></a>建立專案

1. 從**檔案**功能表上，按一下**新的專案**。
2. 在**新的專案**對話方塊方塊中，展開**C#** 之下**範本**，然後選取**Web**。
3. 選取  **ASP.NET 空白 Web 應用程式**範本，將專案命名為*SignalR.StockTicker*，然後按一下**確定**。

    ![新增專案對話方塊](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>將 SignalR NuGet 封裝

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>將 SignalR 和 JQuery 的 NuGet 套件

您可以加入至專案的 SignalR 功能，藉由安裝 NuGet 套件。

1. 按一下 **工具 |程式庫套件管理員 |套件管理員主控台**。
2. 在 套件管理員中，輸入下列命令。

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    SignalR 套件會安裝一些其他 NuGet 套件作為相依項目。 安裝完成時可能會有所有的伺服器和用戶端所需的元件使用 SignalR 的 ASP.NET 應用程式中。

<a id="server"></a>

## <a name="set-up-the-server-code"></a>設定伺服器程式碼

在本節中您設定在伺服器執行的程式碼。

### <a name="create-the-stock-class"></a>建立庫存類別

請先建立您將使用它來儲存和傳輸股票的相關資訊的內建模型類別。

1. 在專案資料夾中建立新的類別檔案，將其命名*Stock.cs*，並以下列程式碼取代範本程式碼：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    當您建立的股票時，您會將兩個屬性是符號 (例如，microsoft 的 MSFT) 和價格。 其他屬性取決於您如何及何時設定價格。 第一次設定價格值取得傳播到 DayOpen。 後續的階段，當您設定價格變更，且 PercentChange 屬性值的計算方式為基礎的價格和 DayOpen 之間的差異。

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>建立 Stockservices.asmx 和 StockTickerHub 類別

您將使用 SignalR 中樞 API 來處理伺服器到用戶端互動。 StockTickerHub 類別衍生自 SignalR Hub 類別會處理來自用戶端接收連線和方法呼叫。 您也需要維護的內建資料並執行的計時器物件，以定期觸發價格更新，不需依賴用戶端連接。 因為暫時性中樞執行個體，您無法暫停在中樞類別中，這些函式。 在中樞內，例如連線和伺服器從用戶端呼叫的每個作業建立中樞類別執行個體。 因此，必須執行在不同的類別，您將會命名為 Stockservices.asmx 的機制，可讓內建的資料、 更新價格，接著在廣播價格更新。

![從 Stockservices.asmx 的廣播](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

您只想 Stockservices.asmx 類別，因此您必須設定為參考從每個 StockTickerHub 執行個體的單一 Stockservices.asmx 執行個體的伺服器上，執行一個執行個體。 Stockservices.asmx 類別必須能夠向用戶端廣播，因為它具有內建的資料，以及觸發更新，但 Stockservices.asmx 不 Hub 類別。 因此，Stockservices.asmx 類別必須取得 SignalR 中樞的連接內容物件的參考。 它可以接著使用 SignalR 連線內容物件廣播給用戶端。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。
2. 如果您有 Visual Studio 2012 [ASP.NET 和 Web 工具 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=279941)，按一下**Web**之下**Visual C#** ，然後選取**SignalR Hub 類別**項目範本。 否則，請選取**類別**範本。
3. 新類別命名*StockTickerHub.cs*，然後按一下**新增**。

    ![Add StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. 下列程式碼取代範本程式碼：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    [中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別用來定義用戶端可以呼叫在伺服器的方法。 您要定義一種方法： `GetAllStocks()`。 用戶端一開始會連接到伺服器，它會呼叫這個方法，以取得所有與目前的價格股票的清單。 此方法可以同步執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。 如果方法必須取得資料所做的事會牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，以啟用非同步處理。 如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-以非同步方式執行的時機](index.md)。

    HubName 屬性會指定中樞上的用戶端的 JavaScript 程式碼的參考方式。 如果您未使用這個屬性在用戶端上的預設名稱是類別名稱，在此情況下會 stockTickerHub 的 camel 案例版本。

    因為您稍後就會看到當您建立 Stockservices.asmx 類別，該類別的單一執行個體被建立在其靜態執行個體屬性中。 Stockservices.asmx 的單一執行個體保留多少個用戶端連線或中斷連線，無論記憶體中，而且該執行個體是 GetAllStocks 方法用來傳回目前的庫存資訊。
5. 在專案資料夾中建立新的類別檔案，將其命名*StockTicker.cs*，並以下列程式碼取代範本程式碼：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    因為多個執行緒會執行為 Stockservices.asmx 的程式碼的相同執行個體，Stockservices.asmx 類別必須是 threadsafe。

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>將單一執行個體儲存在靜態欄位

    程式碼會初始化靜態\_支援的類別和此執行個體的執行個體屬性的執行個體欄位是唯一的執行個體，可以建立的類別，因為建構函式已標示為私用。 [延遲初始設定](https://msdn.microsoft.com/library/dd997286.aspx)使用於\_執行個體欄位，不會基於效能的考量，但若要確定執行個體的建立是 threadsafe。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    用戶端連線到伺服器，每次在個別的執行緒中執行的 StockTickerHub 類別的新執行個體取得 Stockservices.asmx 的單一執行個體從 StockTicker.Instance 靜態屬性，如您稍早在 StockTickerHub 類別中所見。

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>在 ConcurrentDictionary 中儲存內建的資料

    建構函式初始化\_具備一些內建資料範例及 GetAllStocks 股票集合會傳回股票。 如稍早所見，StockTickerHub.GetAllStocks 也就是在用戶端可以呼叫 Hub 類別中的伺服器方法接著會傳回這個集合的股票。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    股票集合指[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的型別。 或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。

    此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料，並處置 Stockservices.asmx 的執行個體時，會遺失資料。 在實際的應用程式中您會使用後端資料存放區，例如資料庫。

    ### <a name="periodically-updating-stock-prices"></a>定期更新股票價格

    建構函式會啟動計時器物件的定期呼叫更新股價隨機為基礎的方法。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices 是由計時器，state 參數中的 null 會傳入呼叫。 更新價格之前, 取得的鎖定\_updateStockPricesLock 物件。 如果另一個執行緒正在更新價格，然後在清單中的每個股票呼叫 TryUpdateStockPrice，則會檢查程式碼。 TryUpdateStockPrice 方法會決定是否要變更的股票的價格，以及有多少變更它。 如果變更的股價，BroadcastStockPrice 稱為廣播到所有已連線的用戶端的股票價格變更。

    \_UpdatingStockPrices 旗標標示[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)以確保其存取權是 threadsafe。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    在實際的應用程式，TryUpdateStockPrice 方法會呼叫 web 服務，以查閱價格;在此程式碼中，它會使用隨機的數字產生器隨機進行變更。

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>取得 SignalR 內容，以便 Stockservices.asmx 類別可以廣播給用戶端

    因為價格變更以下來自 Stockservices.asmx 物件中，這會是需要在所有連線的用戶端上呼叫 updateStockPrice 方法的物件。 中樞類別中您的 API 呼叫的用戶端方法，但 Stockservices.asmx 並非衍生自 Hub 類別，而且沒有任何中樞物件的參考。 因此，若要廣播至連線的用戶端，Stockservices.asmx 類別必須 StockTickerHub 類別取得 SignalR 的 context 執行個體，並使用它來呼叫用戶端上的方法。

    建立單一類別執行個體，建構函式，將該參考時，此程式碼會取得 SignalR 內容的參考，建構函式將它放在用戶端屬性。

    有兩個您想要一次取得內容的原因： 取得內容是昂貴的作業，並取得它一次可確保預期的順序傳送至用戶端的訊息會保留。

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    取得內容的用戶端屬性，並將它放在 StockTickerClient 屬性可讓您撰寫來呼叫用戶端方法看起來一樣如同中樞類別中的程式碼。 比方說，廣播到所有用戶端，您可以撰寫 Clients.All.updateStockPrice(stock)。

    您呼叫在 BroadcastStockPrice updateStockPrice 方法還; 不存在稍後當您撰寫的用戶端執行的程式碼時，會將其新增。 您可以參考以下 updateStockPrice 因為 Clients.All 是動態，這表示將會在執行階段評估的運算式。 SignalR 當這個方法呼叫執行時，會傳送的方法名稱和參數值，用戶端，而且如果用戶端有一個名為 updateStockPrice 方法，就會呼叫該方法的參數值會傳遞給它。

    Clients.All 表示傳送至所有用戶端。 SignalR 提供其他選項，以指定哪些用戶端或用戶端傳送至群組。 如需詳細資訊，請參閱 < [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。

### <a name="register-the-signalr-route"></a>註冊 SignalR 的路由

伺服器必須知道要攔截，並將導向至 SignalR 的 URL。 若要如何將一些程式碼來*Global.asax*檔案。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。
2. 選取 **全域應用程式類別**項目範本，然後按一下**新增**。

    ![加入 global.asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. 將 SignalR 的路由註冊程式碼新增至應用程式\_Start 方法：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    根據預設，所有的 SignalR 流量的基底 URL 是"/ signalr"，而 「 signalr/中樞 」 用來擷取動態產生的 JavaScript 檔案，定義您在您的應用程式的所有主機的 proxy。 MapHubs 方法包含可讓您指定不同的基底 URL 和特定 SignalR 選項的執行個體中的多載[HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx)類別。
4. 加入 using 陳述式在檔案頂端：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. 儲存並關閉*Global.asax*檔案，並建置專案。

您現在已完成設定伺服器程式碼。 在下一節中您會設定用戶端。

<a id="client"></a>

## <a name="set-up-the-client-code"></a>設定用戶端程式碼

1. 新的 HTML 檔案的資料夾中建立專案，並將它命名*StockTicker.html*。
2. 下列程式碼取代範本程式碼：

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    HTML 5 個資料行、 標頭資料列與資料列與單一儲存格跨越所有的 5 個資料行建立的資料表。 資料列會顯示 「 正在載入...」，才會暫時在應用程式啟動時，才會顯示。 JavaScript 程式碼將會移除該資料列，並在其位置的資料列中新增，並從伺服器擷取的內建資料。

    指令碼標記指定 jQuery 指令碼檔案，SignalR core 指令碼檔案、 SignalR proxy 指令碼檔案中，您稍後將建立 Stockservices.asmx 指令碼檔案。 SignalR proxy 指令碼檔案，指定 「 signalr/中樞 」 的 URL，以動態方式產生，並在中樞類別、 方法的 proxy 方法定義在此情況下 StockTickerHub.GetAllStocks。 如果您想，您可以產生這個 JavaScript 檔案以手動方式使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)和停用動態檔案建立 MapHubs 方法呼叫中的。
3. > [!IMPORTANT]
   > 請確定 JavaScript 檔案參考中*StockTicker.html*正確無誤。 也就是確定您的指令碼標記 (在範例中的 1.8.2) 中的 jQuery 版本是 jQuery 版本在專案的相同*指令碼*資料夾，並確定您的指令碼標記中的 SignalR 版本是 SignalR 相同在您的專案中的版本*指令碼*資料夾。 如有必要，請變更指令碼標記中的檔案名稱。
4. 在 **方案總管 中**，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。
5. 在專案資料夾中建立新的 JavaScript 檔案並將它命名*StockTicker.js*...
6. 下列程式碼取代範本程式碼：

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $.connection 指 SignalR proxy。 程式碼取得 StockTickerHub 類別將 proxy 的參考，並將它放在股票行情指示器變數中。 Proxy 名稱會是 [HubName] 屬性所設定的名稱：

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    定義所有變數和函式之後，檔案中的程式碼的最後一行會初始化 SignalR 連線，藉由呼叫 SignalR 開始函式。 開始函式以非同步方式執行，並傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)，這表示您可以呼叫完成的函式，以指定的非同步作業完成時要呼叫的函式...

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    Init 函式 getAllStocks 函式會呼叫伺服器上，並使用伺服器來更新內建的資料表所傳回的資訊。 請注意，根據預設，您必須使用駝峰式命名法大小寫用戶端上雖然方法名稱是依照 pascal 命名法大小寫，在伺服器上。 依照 camel 命名法大小寫規則只適用於不是物件的方法。 例如，您參考股票。符號 」 與 「 存貨。價格、 未 stock.symbol 或 stock.price。

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    如果您想要在用戶端，使用 pascal 命名法大小寫，或如果您想要使用完全不同的方法名稱，您可以裝飾具有 HubMethodName 屬性之中樞方法相同的方式會裝飾 Hub 類別具有 HubName 屬性。

    在 init 方法中，HTML 資料表資料列會建立從伺服器收到的呼叫 formatStock 格式內容的內建的物件，每一個股票物件，並藉由呼叫取代式裝置 (定義在頂端*StockTicker.js*) 取代內建的物件屬性值的 rowTemplate 變數中的預留位置。 產生的 HTML 則會附加至內建的資料表中。

    您可以傳遞，以執行非同步的啟動函式完成後的回呼函式呼叫 init。 如果您呼叫 init 做為個別的 JavaScript 陳述式之後呼叫開始時，函式會失敗，因為它會立即執行而不需等待開始函式，來完成 建立連線。 在此情況下，init 函式會嘗試建立伺服器連接之前呼叫 getAllStocks 函式。

    當伺服器變成股票的價格時，它會呼叫 updateStockPrice 連線的用戶端上。 此函式會新增至 Stockservices.asmx proxy 的用戶端屬性，以便將它提供給呼叫從伺服器。

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    UpdateStockPrice 函式會將接收自伺服器的資料表資料列到 init 函式的相同方式將內建物件格式化。 不過，而不是將資料列附加至資料表，它在資料表中尋找股票的目前資料列，並以新的取代該資料列。

<a id="test"></a>

## <a name="test-the-application"></a>測試應用程式

1. 按 F5 以偵錯模式執行應用程式。

    內建資料表一開始會顯示 「 正在載入...」 列，然後顯示初始的內建資料，在短暫延遲之後，再股票價格開始變更。

    ![正在載入](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![初始的內建資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![從伺服器接收變更內建的資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. 從瀏覽器網址列複製 URL，並將它貼到一或多個新的瀏覽器視窗。

    初始的內建顯示等同於第一個瀏覽器，並同時進行變更。
3. 關閉所有瀏覽器並開啟新的瀏覽器，然後移至相同的 URL。

    若要在伺服器中執行，因此內建的資料表會顯示股票，若要變更持續一直在 Stockservices.asmx 單一物件。 （您沒有看到變更數據的初始資料表為零）。
4. 關閉瀏覽器。

<a id="enablelogging"></a>

## <a name="enable-logging"></a>啟用記錄

SignalR 有內建記錄功能，您可以讓用戶端上，以協助疑難排解。 在本節中您要啟用記錄，並查看範例，說明如何記錄告訴您哪一種下列傳輸方法使用 SignalR:

- [Websocket](http://en.wikipedia.org/wiki/WebSocket)、 支援的 IIS 8 和目前的瀏覽器。
- [伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。
- [永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet explorer 的支援。
- [長時間輪詢的 Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。

針對任何指定的連接，SignalR 會選擇最佳的伺服器和用戶端支援的傳輸方法。

1. 開啟*StockTicker.js*並新增一行程式碼，若要啟用記錄之前初始化的連線，在檔案結尾處的程式碼：

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. 按 F5 執行專案。
3. 開啟瀏覽器的開發人員工具 視窗，然後選取主控台，以查看記錄檔。 您可能必須重新整理頁面，以查看 Signalr 交涉新連線的傳輸方法的記錄檔。

    如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，傳輸方法是 WebSockets。

    ![IE 10 IIS 8 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，傳輸方法是 iframe。

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    在 Firefox 中安裝 Firebug 增益集以取得主控台視窗。 如果您在 Windows 8 (IIS 8) 來執行 Firefox 19，傳輸方法是 WebSockets。

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    如果您在 Windows 7 (IIS 7.5) 上執行 Firefox 19，傳輸方法就會是伺服器傳送事件。

    ![Firefox 19 IIS 7.5 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>安裝並檢閱完整的 Stockservices.asmx 範例

Stockservices.asmx 應用程式會安裝[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件包含您剛從頭開始建立簡化版本較多的功能。 在本節的教學課程中，您要安裝 NuGet 套件，並檢閱新功能，以及實作它們的程式碼。

### <a name="install-the-signalrsample-nuget-package"></a>安裝 SignalR.Sample NuGet 套件

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**管理 NuGet 套件**。
2. 在 **管理 NuGet 套件** 對話方塊中，按一下**線上**，輸入*SignalR.Sample*中**線上搜尋**方塊，然後再按一下  **安裝**中**SignalR.Sample**封裝。

    ![安裝 SignalR.Sample 套件](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. 在  *Global.asax*檔案，標記為註解 RouteTable.Routes.MapHubs(); 行您稍早在應用程式中新增\_Start 方法。

    中的程式碼*Global.asax*不再需要因為 SignalR.Sample 封裝註冊中的 SignalR 路由*應用程式\_Start/RegisterHubs.cs*檔案：

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    組件屬性所參考的 WebActivator 類別一併併入 WebActivatorEx NuGet 封裝，它會安裝為 SignalR.Sample 封裝相依性。
4. 在 [**方案總管] 中**，展開*SignalR.Sample*安裝 SignalR.Sample 封裝所建立的資料夾。
5. 在  *SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。

    > [!NOTE]
    > 安裝 SignalR.Sample NuGet 套件可能會變更您在的 jQuery 版本您*指令碼*資料夾。 新*StockTicker.html*套件會安裝中的檔案*SignalR.Sample*資料夾將會是套件會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記中的 jQuery 參考。

### <a name="run-the-application"></a>執行應用程式

1. 按 F5 執行應用程式。

    方格中稍早所見，除了完整股票行情指示器應用程式會顯示可顯示相同的股票資料的水平捲動視窗。 當您執行第一次應用程式時，「 市場 」 的動作 「 關閉 」，您會看到靜態方格和不捲動的股票行情指示器視窗。

    ![Stockservices.asmx 畫面開始](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    當您按一下 **開放性市場**，則**即時股票行情即時看板**開始的水平捲動方塊，且伺服器會開始定期廣播隨機的方式上的股票價格變更。 每次股票的價格變更，同時**即時股票表格**格線和**即時股票行情即時看板**方塊會更新。 具有綠色背景，正股票的價格變動時，會顯示股票和股票時變更為負數，會顯示具有紅色背景。

    ![開啟 Stockservices.asmx 的應用程式市集](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    **關閉市場** 按鈕會停止所做的變更，並停止股票行情指示器捲動，而**重設**按鈕會重設內建的所有資料的初始狀態價格變動啟動之前。 如果您開啟多個瀏覽器視窗，並移至相同的 URL，您會看到相同的資料，動態更新在此同時在每個瀏覽器中。 當您按一下其中一個按鈕時，則所有瀏覽器會回應相同的方式在相同的時間。

### <a name="live-stock-ticker-display"></a>即時股票行情指示器顯示

**即時股票行情即時看板**顯示會格式化為單一行的 CSS 樣式的 div 元素中的未排序的清單。 股票行情指示器會初始化並更新資料表的相同方式來： 取代中的預留位置&lt;li&gt;範本字串，並以動態方式新增&lt;li&gt;項目&lt;ul&gt;項目。 使用 jQuery 動畫函式來改變的未排序的清單內的 div。 margin-left 執行捲動

股票行情即時看板 HTML:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

股票行情即時看板 CSS:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

JQuery 程式碼，可讓捲動：

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>用戶端可以呼叫的伺服器上的其他方法

StockTickerHub 類別會定義四個用戶端可以呼叫其他方法：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

OpenMarket、 CloseMarket 和重設稱為回應位於頁面頂端的按鈕。 它們會示範一個觸發會立即傳播到所有用戶端的狀態變更的用戶端的模式。 每個方法呼叫的方法 Stockservices.asmx 類別中會影響市場狀態變更，然後再廣播的新狀態。

在 Stockservices.asmx 類別中，由傳回 MarketState 列舉值的 MarketState 屬性維護的市場狀況：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

每個市場狀態變更的方法在內進行的鎖定區塊 Stockservices.asmx 類別必須為 threadsafe 因為：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

若要確保此程式碼 threadsafe， \_marketState 欄位支援 MarketState 屬性會標示為 volatile，

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

BroadcastMarketStateChange 和 BroadcastMarketReset 方法都是類似於 BroadcastStockPrice 方法，您看到的但在呼叫不同的方法，在用戶端定義：

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>用戶端可以呼叫伺服器上的其他函式

方格與顯示的 ticker，現在會處理 updateStockPrice 函式，並使用 jQuery.Color 閃爍紅色和綠色的色彩。

中的新函式*SignalR.StockTicker.js*啟用和停用按鈕根據市場的狀態，和它們停止或啟動股票行情指示器視窗水平捲動。 因為 ticker.client，將會新增多個函式[jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)用來新增它們。

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>建立連線後的其他用戶端安裝程式

用戶端建立連接之後，它會有一些額外的工作，執行： 找出市場上是否已開啟或關閉才能呼叫適當的 marketOpened 或 marketClosed 函式，並附加至按鈕的伺服器方法呼叫。

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

伺服器方法不是連線到之前的按鈕之後建立的連線，這樣的程式碼無法嘗試呼叫伺服器方法，才能使用。

<a id="nextsteps"></a>

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將訊息從伺服器廣播給所有已連線的用戶端，定期執行並以通知從任何用戶端的回應方式。 使用多執行緒的單一執行個體維護伺服器狀態的模式也也可用在多玩家線上遊戲案例中。 如需範例，請參閱[ShootR 遊戲為基礎的 SignalR](https://github.com/NTaylorMullen/ShootR)。

顯示對等通訊案例的教學課程，請參閱[開始使用 SignalR](index.md)並[即時更新與 SignalR](index.md)。

若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：

- [ASP.NET SignalR](https://asp.net/signalr/)
- [SignalR 專案](http://signalr.net/)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
