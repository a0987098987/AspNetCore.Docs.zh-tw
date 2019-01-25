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
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="7188b-103">教學課程：伺服器廣播與 SignalR 2</span><span class="sxs-lookup"><span data-stu-id="7188b-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="7188b-104">本教學課程會示範如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="7188b-105">伺服器廣播表示，伺服器就會傳送至用戶端的通訊。</span><span class="sxs-lookup"><span data-stu-id="7188b-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="7188b-106">您會在本教學課程中建立的應用程式會模擬股票行情即時看板，伺服器廣播功能的一般案例。</span><span class="sxs-lookup"><span data-stu-id="7188b-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="7188b-107">請定期伺服器隨機更新股價，並更新廣播到所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="7188b-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="7188b-108">在瀏覽器、 數字和符號**變更**並**%** 動態變更通知來回應來自伺服器的資料行。</span><span class="sxs-lookup"><span data-stu-id="7188b-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="7188b-109">如果您開啟其他瀏覽器相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。</span><span class="sxs-lookup"><span data-stu-id="7188b-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![建立 web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="7188b-111">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="7188b-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7188b-112">建立專案</span><span class="sxs-lookup"><span data-stu-id="7188b-112">Create the project</span></span>
> * <span data-ttu-id="7188b-113">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-113">Set up the server code</span></span>
> * <span data-ttu-id="7188b-114">檢查伺服端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-114">Examine the server code</span></span>
> * <span data-ttu-id="7188b-115">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-115">Set up the client code</span></span>
> * <span data-ttu-id="7188b-116">檢查用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-116">Examine the client code</span></span>
> * <span data-ttu-id="7188b-117">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="7188b-117">Test the application</span></span>
> * <span data-ttu-id="7188b-118">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="7188b-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7188b-119">如果您不想要逐步建置應用程式的步驟，您可以安裝 SignalR.Sample 封裝在新的空白的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="7188b-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="7188b-120">如果您沒有執行本教學課程中的步驟安裝 NuGet 套件，您必須依照*readme.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="7188b-121">若要執行封裝，您需要新增 OWIN 啟動類別呼叫`ConfigureSignalR`已安裝的封裝中的方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="7188b-122">如果您需要新增 OWIN 啟動類別，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="7188b-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="7188b-123">請參閱[Stockservices.asmx 範例安裝](#install-the-stockticker-sample)一節。</span><span class="sxs-lookup"><span data-stu-id="7188b-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7188b-124">必要條件</span><span class="sxs-lookup"><span data-stu-id="7188b-124">Prerequisites</span></span>

 * <span data-ttu-id="7188b-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="7188b-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7188b-126">建立專案</span><span class="sxs-lookup"><span data-stu-id="7188b-126">Create the project</span></span>

<span data-ttu-id="7188b-127">本節說明如何使用 Visual Studio 2017 來建立空白的 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="7188b-128">在 Visual Studio 中建立 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![建立 web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="7188b-130">在 [**新增 ASP.NET Web 應用程式-SignalR.StockTicker** ] 視窗中，保持**空白**，並選取 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="7188b-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="7188b-131">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-131">Set up the server code</span></span>

<span data-ttu-id="7188b-132">在本節中，您會將設定伺服器執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7188b-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="7188b-133">建立庫存類別</span><span class="sxs-lookup"><span data-stu-id="7188b-133">Create the Stock class</span></span>

<span data-ttu-id="7188b-134">請先建立*股票*模型類別，您將用來儲存和傳輸股票的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="7188b-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="7188b-135">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **類別**。</span><span class="sxs-lookup"><span data-stu-id="7188b-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7188b-136">將類別命名為*股票*並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="7188b-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="7188b-137">中的程式碼取代*Stock.cs*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="7188b-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="7188b-138">當您建立的股票時，您會將兩個屬性是`Symbol`(例如，microsoft 的 MSFT) 和`Price`。</span><span class="sxs-lookup"><span data-stu-id="7188b-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="7188b-139">其他屬性，取決於您如何及何時設定`Price`。</span><span class="sxs-lookup"><span data-stu-id="7188b-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="7188b-140">您所設定的第一次`Price`，值取得傳播到`DayOpen`。</span><span class="sxs-lookup"><span data-stu-id="7188b-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="7188b-141">在那之後，當您設定`Price`，應用程式會計算`Change`並`PercentChange`屬性值根據之間的差異`Price`和`DayOpen`。</span><span class="sxs-lookup"><span data-stu-id="7188b-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="7188b-142">建立 StockTickerHub 和 Stockservices.asmx 類別</span><span class="sxs-lookup"><span data-stu-id="7188b-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="7188b-143">您將使用 SignalR 中樞 API 來處理伺服器到用戶端互動。</span><span class="sxs-lookup"><span data-stu-id="7188b-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="7188b-144">A`StockTickerHub`類別衍生自`SignalRHub`類別會處理來自用戶端接收連線和方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="7188b-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="7188b-145">您也需要維護的內建資料並執行`Timer`物件。</span><span class="sxs-lookup"><span data-stu-id="7188b-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="7188b-146">`Timer`物件將會定期觸發的用戶端連接獨立的價格更新。</span><span class="sxs-lookup"><span data-stu-id="7188b-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="7188b-147">您無法將這些函式放在`Hub`類別，因為中樞是暫時性。</span><span class="sxs-lookup"><span data-stu-id="7188b-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="7188b-148">應用程式建立`Hub`類別執行個體，每個工作中樞，連線等呼叫從用戶端到伺服器上。</span><span class="sxs-lookup"><span data-stu-id="7188b-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="7188b-149">所以，保持股票資料、 更新價格，並會廣播價格更新的機制都在個別的類別中執行。</span><span class="sxs-lookup"><span data-stu-id="7188b-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="7188b-150">您將類別命名為`StockTicker`。</span><span class="sxs-lookup"><span data-stu-id="7188b-150">You'll name the class `StockTicker`.</span></span>

![從 Stockservices.asmx 的廣播](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="7188b-152">您只想的一個執行個體`StockTicker`類別，以在伺服器上執行，因此您必須設定每個參考`StockTickerHub`執行個體與單一`StockTicker`執行個體。</span><span class="sxs-lookup"><span data-stu-id="7188b-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="7188b-153">`StockTicker`的類別必須將廣播給用戶端，因為它具有內建的資料，以及觸發更新，但`StockTicker`不是`Hub`類別。</span><span class="sxs-lookup"><span data-stu-id="7188b-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="7188b-154">`StockTicker`類別必須取得 SignalR 中樞的連接內容物件的參考。</span><span class="sxs-lookup"><span data-stu-id="7188b-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="7188b-155">它可以接著使用 SignalR 連線內容物件廣播給用戶端。</span><span class="sxs-lookup"><span data-stu-id="7188b-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="7188b-156">Create StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7188b-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="7188b-157">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="7188b-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7188b-158">在 **加入新項目-SignalR.StockTicker**，選取**已安裝** > **Visual C#**   >  **Web** >  **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="7188b-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="7188b-159">將類別命名為*StockTickerHub*並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="7188b-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="7188b-160">這個步驟會建立*StockTickerHub.cs*類別檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="7188b-161">同時，它會新增一組指令碼檔案和組件參考加入專案中支援 SignalR。</span><span class="sxs-lookup"><span data-stu-id="7188b-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="7188b-162">中的程式碼取代*StockTickerHub.cs*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="7188b-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="7188b-163">儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-163">Save the file.</span></span>

<span data-ttu-id="7188b-164">應用程式會使用[中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別來定義用戶端可以呼叫在伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="7188b-165">您定義一種方法： `GetAllStocks()`。</span><span class="sxs-lookup"><span data-stu-id="7188b-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="7188b-166">用戶端一開始會連接到伺服器，它會呼叫這個方法，以取得所有與目前的價格股票的清單。</span><span class="sxs-lookup"><span data-stu-id="7188b-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="7188b-167">此方法可以同步執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。</span><span class="sxs-lookup"><span data-stu-id="7188b-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="7188b-168">如果方法必須取得資料所做的事會牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，以啟用非同步處理。</span><span class="sxs-lookup"><span data-stu-id="7188b-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="7188b-169">如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-以非同步方式執行的時機](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)。</span><span class="sxs-lookup"><span data-stu-id="7188b-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="7188b-170">`HubName`屬性會指定應用程式會列印文件的參考，請在用戶端的 JavaScript 程式碼中的中樞。</span><span class="sxs-lookup"><span data-stu-id="7188b-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="7188b-171">在用戶端上的預設名稱，如果您未使用這個屬性，是 camelCase 版本的類別名稱，在此情況下會`stockTickerHub`。</span><span class="sxs-lookup"><span data-stu-id="7188b-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="7188b-172">因為您稍後所見，當您建立`StockTicker`類別，應用程式建立該類別的單一執行個體在它的靜態`Instance`屬性。</span><span class="sxs-lookup"><span data-stu-id="7188b-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="7188b-173">該單一執行個體`StockTicker`無論多少個用戶端連線或中斷連線的記憶體中。</span><span class="sxs-lookup"><span data-stu-id="7188b-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="7188b-174">該執行個體是什麼`GetAllStocks()`方法會使用來傳回目前的股票資訊。</span><span class="sxs-lookup"><span data-stu-id="7188b-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="7188b-175">建立 StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="7188b-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="7188b-176">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **類別**。</span><span class="sxs-lookup"><span data-stu-id="7188b-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7188b-177">將類別命名為*Stockservices.asmx*並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="7188b-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="7188b-178">中的程式碼取代*StockTicker.cs*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="7188b-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="7188b-179">由於所有執行緒將都執行相同的執行個體的 Stockservices.asmx 的程式碼，Stockservices.asmx 類別必須是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="7188b-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="7188b-180">檢查伺服端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-180">Examine the server code</span></span>

<span data-ttu-id="7188b-181">如果您檢查伺服端程式碼時，它會協助您了解應用程式的運作方式。</span><span class="sxs-lookup"><span data-stu-id="7188b-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="7188b-182">將單一執行個體儲存在靜態欄位</span><span class="sxs-lookup"><span data-stu-id="7188b-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="7188b-183">程式碼會初始化靜態`_instance`支援欄位`Instance`屬性類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7188b-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="7188b-184">建構函式為私用，因為它是唯一的執行個體之類別的應用程式可以建立。</span><span class="sxs-lookup"><span data-stu-id="7188b-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="7188b-185">應用程式會使用[延遲初始設定](/dotnet/framework/performance/lazy-initialization)如`_instance`欄位。</span><span class="sxs-lookup"><span data-stu-id="7188b-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="7188b-186">它不是基於效能考量。</span><span class="sxs-lookup"><span data-stu-id="7188b-186">It's not for performance reasons.</span></span> <span data-ttu-id="7188b-187">它是確保執行個體的建立是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="7188b-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="7188b-188">用戶端連線到伺服器，每次在個別的執行緒中執行的 StockTickerHub 類別的新執行個體取得 Stockservices.asmx 單一執行個體，從`StockTicker.Instance`靜態屬性，為您所見稍早在`StockTickerHub`類別。</span><span class="sxs-lookup"><span data-stu-id="7188b-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="7188b-189">在 ConcurrentDictionary 中儲存內建的資料</span><span class="sxs-lookup"><span data-stu-id="7188b-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="7188b-190">建構函式初始化`_stocks`有些範例股票資料集合和`GetAllStocks`傳回股票。</span><span class="sxs-lookup"><span data-stu-id="7188b-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="7188b-191">如您稍早所見，傳回這個集合的股票`StockTickerHub.GetAllStocks`，這是中的伺服器方法`Hub`用戶端可以呼叫的類別。</span><span class="sxs-lookup"><span data-stu-id="7188b-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="7188b-192">股票集合指[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的型別。</span><span class="sxs-lookup"><span data-stu-id="7188b-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="7188b-193">或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。</span><span class="sxs-lookup"><span data-stu-id="7188b-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="7188b-194">此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料，並遺失的資料，當應用程式處置`StockTicker`執行個體。</span><span class="sxs-lookup"><span data-stu-id="7188b-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="7188b-195">在實際的應用程式中，您會使用後端等資料存放區資料庫。</span><span class="sxs-lookup"><span data-stu-id="7188b-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="7188b-196">定期更新股票價格</span><span class="sxs-lookup"><span data-stu-id="7188b-196">Periodically updating stock prices</span></span>

<span data-ttu-id="7188b-197">建構函式啟動`Timer`定期呼叫方法，以更新股價隨機為基礎的物件。</span><span class="sxs-lookup"><span data-stu-id="7188b-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="7188b-198">`Timer` 呼叫`UpdateStockPrices`，哪一個階段中為 state 參數中的 null。</span><span class="sxs-lookup"><span data-stu-id="7188b-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="7188b-199">應用程式更新之前的價格，請上取得鎖定`_updateStockPricesLock`物件。</span><span class="sxs-lookup"><span data-stu-id="7188b-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="7188b-200">如果另一個執行緒正在更新價格，然後呼叫程式碼會檢查`TryUpdateStockPrice`在清單中的每個股票。</span><span class="sxs-lookup"><span data-stu-id="7188b-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="7188b-201">`TryUpdateStockPrice`方法會決定是否要變更的股票的價格，以及有多少變更它。</span><span class="sxs-lookup"><span data-stu-id="7188b-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="7188b-202">如果股票的價格變更時，應用程式會呼叫`BroadcastStockPrice`廣播到所有的股票價格變更連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="7188b-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="7188b-203">`_updatingStockPrices`指定的旗標[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)並確定它是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="7188b-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="7188b-204">在實際的應用程式，`TryUpdateStockPrice`方法會呼叫 web 服務，以查閱價格。</span><span class="sxs-lookup"><span data-stu-id="7188b-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="7188b-205">在此程式碼中，應用程式會使用隨機的數字產生器隨機進行變更。</span><span class="sxs-lookup"><span data-stu-id="7188b-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="7188b-206">取得 SignalR 內容，以便 Stockservices.asmx 類別可以廣播給用戶端</span><span class="sxs-lookup"><span data-stu-id="7188b-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="7188b-207">因為價格變更都是來自以下`StockTicker`物件，它是需要呼叫物件`updateStockPrice`所有已連線的用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="7188b-208">在 `Hub`類別，您有一個 API 呼叫的用戶端方法，但`StockTicker`不是衍生自`Hub`類別，並沒有任何參考`Hub`物件。</span><span class="sxs-lookup"><span data-stu-id="7188b-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="7188b-209">連線的用戶端，向廣播`StockTicker`類別具有取得 SignalR 內容執行個體的`StockTickerHub`類別，並使用它來呼叫用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="7188b-210">建立單一類別執行個體，建構函式，將該參考時，此程式碼會取得 SignalR 內容的參考，並建構函式放在`Clients`屬性。</span><span class="sxs-lookup"><span data-stu-id="7188b-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="7188b-211">有兩個您想要一次取得內容的原因： 取得內容是昂貴的工作中，並取得它之後可確保應用程式保留的訊息傳送至用戶端預期的順序。</span><span class="sxs-lookup"><span data-stu-id="7188b-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="7188b-212">取得`Clients`屬性的內容，並將它放`StockTickerClient`屬性可讓您撰寫程式碼來呼叫用戶端的方法，會看起來相同`Hub`類別。</span><span class="sxs-lookup"><span data-stu-id="7188b-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="7188b-213">若要廣播到所有用戶端可以撰寫比方說， `Clients.All.updateStockPrice(stock)`。</span><span class="sxs-lookup"><span data-stu-id="7188b-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="7188b-214">`updateStockPrice`方法，您會在呼叫`BroadcastStockPrice`尚不存在。</span><span class="sxs-lookup"><span data-stu-id="7188b-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="7188b-215">稍後當您撰寫的用戶端執行的程式碼時，會將其新增。</span><span class="sxs-lookup"><span data-stu-id="7188b-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="7188b-216">您可以參考`updateStockPrice`這裡因為`Clients.All`是動態，這表示應用程式將會評估運算式在執行階段。</span><span class="sxs-lookup"><span data-stu-id="7188b-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="7188b-217">SignalR 當這個方法呼叫執行時，會傳送的方法名稱和參數值，用戶端，而且如果用戶端有一個名為方法`updateStockPrice`，應用程式會呼叫該方法，並將參數值傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="7188b-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="7188b-218">`Clients.All` 表示將傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="7188b-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="7188b-219">SignalR 提供其他選項，以指定哪些用戶端或用戶端傳送至群組。</span><span class="sxs-lookup"><span data-stu-id="7188b-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="7188b-220">如需詳細資訊，請參閱 < [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="7188b-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="7188b-221">註冊 SignalR 的路由</span><span class="sxs-lookup"><span data-stu-id="7188b-221">Register the SignalR route</span></span>

<span data-ttu-id="7188b-222">伺服器必須知道要攔截，並將導向至 SignalR 的 URL。</span><span class="sxs-lookup"><span data-stu-id="7188b-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="7188b-223">若要這樣做，請新增 OWIN 啟動類別：</span><span class="sxs-lookup"><span data-stu-id="7188b-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="7188b-224">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="7188b-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7188b-225">在 **加入新項目-SignalR.StockTicker**選取**已安裝** > **Visual C#**   >  **Web**和然後選取**OWIN 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="7188b-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="7188b-226">將類別命名為*啟始*，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="7188b-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="7188b-227">取代預設的程式碼中*Startup.cs*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="7188b-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="7188b-228">您現在已完成設定伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="7188b-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="7188b-229">在下一步 區段中，您會設定用戶端。</span><span class="sxs-lookup"><span data-stu-id="7188b-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="7188b-230">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-230">Set up the client code</span></span>

<span data-ttu-id="7188b-231">在本節中，您設定用戶端執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7188b-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="7188b-232">建立 HTML 網頁和 JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="7188b-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="7188b-233">HTML 網頁會顯示的資料和 JavaScript 檔案將組織的資料。</span><span class="sxs-lookup"><span data-stu-id="7188b-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="7188b-234">建立 StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="7188b-234">Create StockTicker.html</span></span>

<span data-ttu-id="7188b-235">首先，您要加入 HTML 用戶端。</span><span class="sxs-lookup"><span data-stu-id="7188b-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="7188b-236">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **HTML 網頁**。</span><span class="sxs-lookup"><span data-stu-id="7188b-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="7188b-237">將檔案命名*Stockservices.asmx* ，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="7188b-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7188b-238">取代預設的程式碼中*StockTicker.html*這段程式碼檔案：</span><span class="sxs-lookup"><span data-stu-id="7188b-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="7188b-239">HTML 會建立具有五個資料行、 標頭資料列和資料列與單一儲存格跨越所有五個資料行的資料表。</span><span class="sxs-lookup"><span data-stu-id="7188b-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="7188b-240">資料列會顯示 「 正在載入...」 應用程式啟動時，立刻顯示。</span><span class="sxs-lookup"><span data-stu-id="7188b-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="7188b-241">JavaScript 程式碼將會移除該資料列，並在其位置的資料列中新增，並從伺服器擷取的內建資料。</span><span class="sxs-lookup"><span data-stu-id="7188b-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="7188b-242">指定指令碼標記：</span><span class="sxs-lookup"><span data-stu-id="7188b-242">The script tags specify:</span></span>

    * <span data-ttu-id="7188b-243">JQuery 指令碼檔案中。</span><span class="sxs-lookup"><span data-stu-id="7188b-243">The jQuery script file.</span></span>

    * <span data-ttu-id="7188b-244">SignalR core 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="7188b-245">SignalR proxy 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="7188b-246">您稍後將建立 Stockservices.asmx 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="7188b-247">應用程式以動態方式產生 SignalR proxy 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="7188b-248">它指定 「 signalr/中樞 」 的 URL，並定義中樞類別，在此情況下，方法的 proxy 方法`StockTickerHub.GetAllStocks`。</span><span class="sxs-lookup"><span data-stu-id="7188b-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="7188b-249">如果您想，您可以產生這個 JavaScript 檔案以手動方式使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)。</span><span class="sxs-lookup"><span data-stu-id="7188b-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="7188b-250">別忘了停用動態檔案建立在`MapHubs`方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="7188b-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="7188b-251">在 **方案總管**，展開**指令碼**。</span><span class="sxs-lookup"><span data-stu-id="7188b-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="7188b-252">JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。</span><span class="sxs-lookup"><span data-stu-id="7188b-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7188b-253">封裝管理員會安裝新版 SignalR 指令碼。</span><span class="sxs-lookup"><span data-stu-id="7188b-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="7188b-254">更新程式碼區塊，以對應至專案中的指令碼檔案的版本中的指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="7188b-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="7188b-255">在 [**方案總管] 中**，以滑鼠右鍵按一下*StockTicker.html*，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="7188b-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="7188b-256">建立 StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7188b-256">Create StockTicker.js</span></span>

<span data-ttu-id="7188b-257">現在建立 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="7188b-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="7188b-258">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **JavaScript 檔案**。</span><span class="sxs-lookup"><span data-stu-id="7188b-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="7188b-259">將檔案命名*Stockservices.asmx* ，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="7188b-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7188b-260">新增下列程式碼*StockTicker.js*檔案：</span><span class="sxs-lookup"><span data-stu-id="7188b-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="7188b-261">檢查用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-261">Examine the client code</span></span>

<span data-ttu-id="7188b-262">如果您檢查用戶端程式碼時，它將協助您了解用戶端程式碼互動的伺服器程式碼，讓應用程式運作的方式。</span><span class="sxs-lookup"><span data-stu-id="7188b-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="7188b-263">正在開始連線</span><span class="sxs-lookup"><span data-stu-id="7188b-263">Starting the connection</span></span>

<span data-ttu-id="7188b-264">`$.connection` 是指 SignalR proxy。</span><span class="sxs-lookup"><span data-stu-id="7188b-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="7188b-265">此程式碼取得參考的 proxy`StockTickerHub`類別，並將它放在`ticker`變數。</span><span class="sxs-lookup"><span data-stu-id="7188b-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="7188b-266">Proxy 名稱時所設定的名稱`HubName`屬性：</span><span class="sxs-lookup"><span data-stu-id="7188b-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="7188b-267">在檔案中的程式碼的最後一行定義所有變數和函式之後，會初始化 SignalR 連線，藉由呼叫 SignalR`start`函式。</span><span class="sxs-lookup"><span data-stu-id="7188b-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="7188b-268">`start`函式以非同步方式執行，並傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)。</span><span class="sxs-lookup"><span data-stu-id="7188b-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="7188b-269">您可以呼叫完成的函式，以指定當應用程式完成的非同步動作時要呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="7188b-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="7188b-270">取得所有股票</span><span class="sxs-lookup"><span data-stu-id="7188b-270">Getting all the stocks</span></span>

<span data-ttu-id="7188b-271">`init`函式呼叫`getAllStocks`伺服器上的函式，並使用伺服器來更新內建的資料表所傳回的資訊。</span><span class="sxs-lookup"><span data-stu-id="7188b-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="7188b-272">請注意，根據預設，您必須在用戶端上將 camelCasing，即使方法名稱是依照 pascal 命名法大小寫，在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="7188b-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="7188b-273">CamelCasing 規則僅適用於不是物件的方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="7188b-274">例如，您參照`stock.Symbol`並`stock.Price`，而非`stock.symbol`或`stock.price`。</span><span class="sxs-lookup"><span data-stu-id="7188b-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="7188b-275">在`init`方法，應用程式會建立 HTML 資料表資料列的每個內建物件從伺服器收到藉由呼叫`formatStock`的格式屬性`stock`物件，並藉由呼叫`supplant`來取代中的預留位置`rowTemplate`變數`stock`物件屬性值。</span><span class="sxs-lookup"><span data-stu-id="7188b-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="7188b-276">產生的 HTML 則會附加至內建的資料表中。</span><span class="sxs-lookup"><span data-stu-id="7188b-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="7188b-277">您呼叫`init`傳遞，作為`callback`之後的非同步執行的函式`start`函式完成。</span><span class="sxs-lookup"><span data-stu-id="7188b-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="7188b-278">如果您呼叫`init`做為個別的 JavaScript 陳述式之後呼叫`start`，函式會失敗，因為它會立即執行而不需等待開始函式，來完成 建立連線。</span><span class="sxs-lookup"><span data-stu-id="7188b-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="7188b-279">在此情況下，`init`函式會嘗試呼叫`getAllStocks`函式應用程式會在建立伺服器連線之前。</span><span class="sxs-lookup"><span data-stu-id="7188b-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="7188b-280">取得已更新的股票價格</span><span class="sxs-lookup"><span data-stu-id="7188b-280">Getting updated stock prices</span></span>

<span data-ttu-id="7188b-281">當伺服器變更股票的價格時，它會呼叫`updateStockPrice`連線的用戶端上。</span><span class="sxs-lookup"><span data-stu-id="7188b-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="7188b-282">應用程式會將函式加入至用戶端屬性`stockTicker`以供呼叫至來自伺服器的 proxy。</span><span class="sxs-lookup"><span data-stu-id="7188b-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="7188b-283">`updateStockPrice`函式格式的內建的物件，從伺服器接收到資料表資料列中的相同方式`init`函式。</span><span class="sxs-lookup"><span data-stu-id="7188b-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="7188b-284">而不是將資料列附加至資料表，它會尋找資料表中的股票的目前資料列，並取代該資料列，以新的。</span><span class="sxs-lookup"><span data-stu-id="7188b-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="7188b-285">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="7188b-285">Test the application</span></span>

<span data-ttu-id="7188b-286">您可以測試應用程式，以確定它運作。</span><span class="sxs-lookup"><span data-stu-id="7188b-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="7188b-287">您會看到顯示股票的價格變動的即時的內建資料表的所有瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="7188b-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="7188b-288">在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行應用程式的 播放 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7188b-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![使用者開啟偵錯模式，然後選取 [播放] 的螢幕擷取畫面。](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="7188b-290">瀏覽器視窗會開啟，顯示**即時股票表格**。</span><span class="sxs-lookup"><span data-stu-id="7188b-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="7188b-291">內建的資料表一開始會顯示 「 正在載入...」 列，然後一小段時間之後, 應用程式會顯示初始的內建資料，以及股票的價格再開始變更。</span><span class="sxs-lookup"><span data-stu-id="7188b-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="7188b-292">從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。</span><span class="sxs-lookup"><span data-stu-id="7188b-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="7188b-293">初始的內建顯示等同於第一個瀏覽器，並同時進行變更。</span><span class="sxs-lookup"><span data-stu-id="7188b-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="7188b-294">關閉所有瀏覽器，開啟新的瀏覽器，並移至相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="7188b-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="7188b-295">在伺服器中執行，繼續 Stockservices.asmx 單一物件。</span><span class="sxs-lookup"><span data-stu-id="7188b-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="7188b-296">**即時股票表格**股票持續變更的顯示。</span><span class="sxs-lookup"><span data-stu-id="7188b-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="7188b-297">看不到初始資料表具有零變更數據。</span><span class="sxs-lookup"><span data-stu-id="7188b-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="7188b-298">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7188b-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="7188b-299">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="7188b-299">Enable logging</span></span>

<span data-ttu-id="7188b-300">SignalR 有內建記錄功能，您可以讓用戶端上，以協助疑難排解。</span><span class="sxs-lookup"><span data-stu-id="7188b-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="7188b-301">在本節中，您可以啟用記錄，並查看範例，說明如何記錄告訴您哪一種下列傳輸方法使用 SignalR:</span><span class="sxs-lookup"><span data-stu-id="7188b-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="7188b-302">[Websocket](http://en.wikipedia.org/wiki/WebSocket)、 支援的 IIS 8 和目前的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7188b-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="7188b-303">[伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。</span><span class="sxs-lookup"><span data-stu-id="7188b-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="7188b-304">[永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet explorer 的支援。</span><span class="sxs-lookup"><span data-stu-id="7188b-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="7188b-305">[長時間輪詢的 Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7188b-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="7188b-306">針對任何指定的連接，SignalR 會選擇最佳的伺服器和用戶端支援的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="7188b-307">開啟*StockTicker.js*。</span><span class="sxs-lookup"><span data-stu-id="7188b-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="7188b-308">新增此反白顯示的行程式碼來啟用記錄之前初始化的連線，在檔案結尾處的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7188b-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="7188b-309">按 **F5** 執行專案。</span><span class="sxs-lookup"><span data-stu-id="7188b-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="7188b-310">開啟瀏覽器的開發人員工具 視窗，然後選取主控台，以查看記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7188b-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="7188b-311">您可能必須重新整理頁面，以查看 SignalR 交涉新連線的傳輸方法的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7188b-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="7188b-312">如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，則傳輸方法就**WebSockets**。</span><span class="sxs-lookup"><span data-stu-id="7188b-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="7188b-313">如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，則傳輸方法就**iframe**。</span><span class="sxs-lookup"><span data-stu-id="7188b-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="7188b-314">如果您在 Windows 8 (IIS 8) 來執行 Firefox 19，則傳輸方法就**WebSockets**。</span><span class="sxs-lookup"><span data-stu-id="7188b-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="7188b-315">在 Firefox 中安裝 Firebug 增益集以取得主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="7188b-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="7188b-316">如果您在 Windows 7 (IIS 7.5) 來執行 Firefox 19，則傳輸方法就**伺服器傳送**事件。</span><span class="sxs-lookup"><span data-stu-id="7188b-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="7188b-317">安裝 Stockservices.asmx 範例</span><span class="sxs-lookup"><span data-stu-id="7188b-317">Install the StockTicker sample</span></span>

<span data-ttu-id="7188b-318">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample)安裝 Stockservices.asmx 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="7188b-319">NuGet 套件會包含更多的功能比您從頭開始建立簡化版本。</span><span class="sxs-lookup"><span data-stu-id="7188b-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="7188b-320">在本節的教學課程中，您要安裝 NuGet 套件，並檢閱新功能，以及實作它們的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7188b-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7188b-321">如果您不需要執行本教學課程的先前步驟中安裝套件，您必須新增 OWIN 啟動類別至您的專案。</span><span class="sxs-lookup"><span data-stu-id="7188b-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="7188b-322">這個 NuGet 套件的 readme.txt 檔案會說明此步驟。</span><span class="sxs-lookup"><span data-stu-id="7188b-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="7188b-323">安裝 SignalR.Sample NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="7188b-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="7188b-324">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取 [管理 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="7188b-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="7188b-325">在  **NuGet 套件管理員：SignalR.StockTicker**，選取**瀏覽**。</span><span class="sxs-lookup"><span data-stu-id="7188b-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="7188b-326">從**套件來源**，選取**nuget.org**。</span><span class="sxs-lookup"><span data-stu-id="7188b-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="7188b-327">請輸入*SignalR.Sample*中的搜尋方塊，然後選取**Microsoft.AspNet.SignalR.Sample** > **安裝**。</span><span class="sxs-lookup"><span data-stu-id="7188b-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="7188b-328">在 **方案總管**，展開*SignalR.Sample*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7188b-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="7188b-329">安裝 SignalR.Sample 封裝建立的資料夾和其內容。</span><span class="sxs-lookup"><span data-stu-id="7188b-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="7188b-330">在  *SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="7188b-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7188b-331">安裝 SignalR.Sample NuGet 套件可能會變更您在的 jQuery 版本您*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7188b-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="7188b-332">新*StockTicker.html*套件會安裝中的檔案*SignalR.Sample*資料夾將會是套件會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記中的 jQuery 參考。</span><span class="sxs-lookup"><span data-stu-id="7188b-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="7188b-333">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="7188b-333">Run the application</span></span>

 <span data-ttu-id="7188b-334">您在第一個應用程式中看到的資料表有實用的功能。</span><span class="sxs-lookup"><span data-stu-id="7188b-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="7188b-335">完整股票行情指示器應用程式會顯示新功能： 水平捲動中視窗，顯示的股票資料和變更色彩，如增加，而切換的股票。</span><span class="sxs-lookup"><span data-stu-id="7188b-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="7188b-336">按下 **F5** 即可執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="7188b-337">當您第一次執行應用程式時，「 市場 」 的動作 「 關閉 」，您會看到靜態表格和不捲動的股票行情指示器視窗。</span><span class="sxs-lookup"><span data-stu-id="7188b-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="7188b-338">選取  **Open Market**。</span><span class="sxs-lookup"><span data-stu-id="7188b-338">Select **Open Market**.</span></span>

    ![即時股票行情指示器螢幕擷取畫面。](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="7188b-340">**即時股票行情即時看板**開始的水平捲動方塊，且伺服器會開始定期廣播隨機的方式上的股票價格變更。</span><span class="sxs-lookup"><span data-stu-id="7188b-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="7188b-341">每次股票的價格變更時，應用程式更新這兩個**即時股票表格**並**即時股票行情即時看板**。</span><span class="sxs-lookup"><span data-stu-id="7188b-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="7188b-342">正數股票的價格變動時，應用程式會顯示具有綠色背景股票。</span><span class="sxs-lookup"><span data-stu-id="7188b-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="7188b-343">當變更為負數時，應用程式會顯示具有紅色背景股票。</span><span class="sxs-lookup"><span data-stu-id="7188b-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="7188b-344">選取 **關閉市場**。</span><span class="sxs-lookup"><span data-stu-id="7188b-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="7188b-345">資料表更新停止。</span><span class="sxs-lookup"><span data-stu-id="7188b-345">The table updates stop.</span></span>

    * <span data-ttu-id="7188b-346">股票行情指示器會停止捲動。</span><span class="sxs-lookup"><span data-stu-id="7188b-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="7188b-347">選取 **重設**。</span><span class="sxs-lookup"><span data-stu-id="7188b-347">Select **Reset**.</span></span>

    * <span data-ttu-id="7188b-348">所有內建的資料會重設。</span><span class="sxs-lookup"><span data-stu-id="7188b-348">All stock data is reset.</span></span>

    * <span data-ttu-id="7188b-349">應用程式還原之前啟動的價格變動的初始狀態。</span><span class="sxs-lookup"><span data-stu-id="7188b-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="7188b-350">從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。</span><span class="sxs-lookup"><span data-stu-id="7188b-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="7188b-351">您會看到相同的資料，動態更新在此同時在每個瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="7188b-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="7188b-352">當您選取的任何控制項時，則所有瀏覽器會回應相同的方式在相同的時間。</span><span class="sxs-lookup"><span data-stu-id="7188b-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="7188b-353">即時股票行情指示器顯示</span><span class="sxs-lookup"><span data-stu-id="7188b-353">Live Stock Ticker display</span></span>

<span data-ttu-id="7188b-354">**即時股票行情即時看板**顯示為中的未排序的清單`<div>`項目格式化成單一行的 CSS 樣式。</span><span class="sxs-lookup"><span data-stu-id="7188b-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="7188b-355">應用程式初始化，並更新股票行情指示器與資料表相同的方式： 藉由取代中的預留位置`<li>`範本字串，並以動態方式新增`<li>`項目`<ul>`項目。</span><span class="sxs-lookup"><span data-stu-id="7188b-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="7188b-356">應用程式包含使用 jQuery 捲動`animate`函式來變更邊界內的未排序清單左邊`<div>`。</span><span class="sxs-lookup"><span data-stu-id="7188b-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="7188b-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="7188b-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="7188b-358">股票行情即時看板 HTML 程式碼：</span><span class="sxs-lookup"><span data-stu-id="7188b-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="7188b-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="7188b-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="7188b-360">股票行情即時看板 CSS 程式碼：</span><span class="sxs-lookup"><span data-stu-id="7188b-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="7188b-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7188b-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="7188b-362">JQuery 程式碼，可讓捲動：</span><span class="sxs-lookup"><span data-stu-id="7188b-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="7188b-363">用戶端可以呼叫的伺服器上的其他方法</span><span class="sxs-lookup"><span data-stu-id="7188b-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="7188b-364">若要新增至應用程式的彈性，還有一些應用程式可以呼叫的其他方法。</span><span class="sxs-lookup"><span data-stu-id="7188b-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="7188b-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7188b-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="7188b-366">`StockTickerHub`類別會定義四個用戶端可以呼叫其他方法：</span><span class="sxs-lookup"><span data-stu-id="7188b-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="7188b-367">應用程式呼叫`OpenMarket`， `CloseMarket`，和`Reset`回應位於頁面頂端的按鈕。</span><span class="sxs-lookup"><span data-stu-id="7188b-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="7188b-368">它們示範觸發立即傳播到所有用戶端的狀態變更的一部用戶端的模式。</span><span class="sxs-lookup"><span data-stu-id="7188b-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="7188b-369">每個方法呼叫中的方法`StockTicker`類別，會使市場狀態變更，然後再廣播的新狀態。</span><span class="sxs-lookup"><span data-stu-id="7188b-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="7188b-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="7188b-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="7188b-371">在 `StockTicker`類別，應用程式會維護狀態的市場`MarketState`屬性，傳回`MarketState`列舉值：</span><span class="sxs-lookup"><span data-stu-id="7188b-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="7188b-372">每個市場狀態變更的方法執行這項操作的鎖定區塊內因為`StockTicker`類別必須是安全執行緒：</span><span class="sxs-lookup"><span data-stu-id="7188b-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="7188b-373">若要確定此程式碼是安全執行緒`_marketState`支援欄位`MarketState`屬性指定`volatile`:</span><span class="sxs-lookup"><span data-stu-id="7188b-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="7188b-374">`BroadcastMarketStateChange`和`BroadcastMarketReset`方法很類似您已經看到 BroadcastStockPrice 方法，但在呼叫不同的方法，在用戶端定義：</span><span class="sxs-lookup"><span data-stu-id="7188b-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="7188b-375">用戶端可以呼叫伺服器上的其他函式</span><span class="sxs-lookup"><span data-stu-id="7188b-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="7188b-376">`updateStockPrice`函式現在會處理資料表和股票行情指示器顯示，並使用`jQuery.Color`閃爍紅色和綠色的色彩。</span><span class="sxs-lookup"><span data-stu-id="7188b-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="7188b-377">中的新函式*SignalR.StockTicker.js*啟用和停用根據市場狀態的按鈕。</span><span class="sxs-lookup"><span data-stu-id="7188b-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="7188b-378">它們也停止或啟動**即時股票行情即時看板**水平捲動。</span><span class="sxs-lookup"><span data-stu-id="7188b-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="7188b-379">因為許多函式加入至`ticker.client`，應用程式會使用[jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)將它們加入。</span><span class="sxs-lookup"><span data-stu-id="7188b-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="7188b-380">建立連線後的其他用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="7188b-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="7188b-381">用戶端建立連接之後，它會有一些額外的工作，執行：</span><span class="sxs-lookup"><span data-stu-id="7188b-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="7188b-382">了解市場上是否已開啟或關閉呼叫適當`marketOpened`或`marketClosed`函式。</span><span class="sxs-lookup"><span data-stu-id="7188b-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="7188b-383">附加至按鈕的伺服器方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="7188b-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="7188b-384">伺服器方法未連線到之前的按鈕中之後應用程式建立連接。</span><span class="sxs-lookup"><span data-stu-id="7188b-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="7188b-385">它是讓程式碼無法呼叫伺服器方法，才能提供。</span><span class="sxs-lookup"><span data-stu-id="7188b-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7188b-386">其他資源</span><span class="sxs-lookup"><span data-stu-id="7188b-386">Additional resources</span></span>

<span data-ttu-id="7188b-387">在本教學課程中，您已了解如何撰寫程式將訊息從伺服器廣播到所有已連線的用戶端的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="7188b-388">現在您可以廣播定期執行，以回應通知從任何用戶端的訊息。</span><span class="sxs-lookup"><span data-stu-id="7188b-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="7188b-389">您可以使用多執行緒的單一執行個體的概念來維護多玩家線上遊戲案例中的伺服器狀態。</span><span class="sxs-lookup"><span data-stu-id="7188b-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="7188b-390">如需範例，請參閱[ShootR 遊戲根據 SignalR](https://github.com/NTaylorMullen/ShootR)。</span><span class="sxs-lookup"><span data-stu-id="7188b-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="7188b-391">顯示對等通訊案例的教學課程，請參閱[開始使用 SignalR](introduction-to-signalr.md)並[即時更新與 SignalR](tutorial-high-frequency-realtime-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="7188b-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="7188b-392">如需 SignalR 相關資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="7188b-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="7188b-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="7188b-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="7188b-394">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="7188b-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="7188b-395">SignalR GitHub 和範例</span><span class="sxs-lookup"><span data-stu-id="7188b-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="7188b-396">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7188b-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="7188b-397">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7188b-397">Next steps</span></span>

<span data-ttu-id="7188b-398">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="7188b-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7188b-399">建立專案</span><span class="sxs-lookup"><span data-stu-id="7188b-399">Created the project</span></span>
> * <span data-ttu-id="7188b-400">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-400">Set up the server code</span></span>
> * <span data-ttu-id="7188b-401">檢查伺服端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-401">Examined the server code</span></span>
> * <span data-ttu-id="7188b-402">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-402">Set up the client code</span></span>
> * <span data-ttu-id="7188b-403">檢查用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="7188b-403">Examined the client code</span></span>
> * <span data-ttu-id="7188b-404">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="7188b-404">Tested the application</span></span>
> * <span data-ttu-id="7188b-405">啟用的記錄</span><span class="sxs-lookup"><span data-stu-id="7188b-405">Enabled logging</span></span>

<span data-ttu-id="7188b-406">請前往下一篇文章，以了解如何建立使用 ASP.NET SignalR 2 的即時 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7188b-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7188b-407">使用 SignalR 建立即時 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7188b-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)