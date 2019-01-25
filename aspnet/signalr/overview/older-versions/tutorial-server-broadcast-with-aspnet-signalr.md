---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 教學課程：伺服器廣播與 ASP.NET SignalR 1.x |Microsoft Docs
author: bradygaster
description: 本教學課程會示範如何建立使用 ASP.NET SignalR 來提供伺服器廣播的功能的 web 應用程式。 伺服器廣播方法，communic...
ms.author: bradyg
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 258a55bf72c4b3425d001f478620fa9651952b3f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837464"
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a><span data-ttu-id="cd285-104">教學課程：伺服器廣播與 ASP.NET SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="cd285-104">Tutorial: Server Broadcast with ASP.NET SignalR 1.x</span></span>
====================
<span data-ttu-id="cd285-105">藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cd285-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="cd285-106">本教學課程會示範如何建立使用 ASP.NET SignalR 來提供伺服器廣播的功能的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cd285-106">This tutorial shows how to create a web application that uses ASP.NET SignalR to provide server broadcast functionality.</span></span> <span data-ttu-id="cd285-107">伺服器廣播表示傳送至用戶端的通訊由伺服器起始。</span><span class="sxs-lookup"><span data-stu-id="cd285-107">Server broadcast means that communications sent to clients are initiated by the server.</span></span> <span data-ttu-id="cd285-108">這種情況下需要不同的程式設計方法，比對等案例，例如交談應用程式，在其中傳送到用戶端的通訊所起始的一或多個用戶端。</span><span class="sxs-lookup"><span data-stu-id="cd285-108">This scenario requires a different programming approach than peer-to-peer scenarios such as chat applications, in which communications sent to clients are initiated by one or more of the clients.</span></span>
> 
> <span data-ttu-id="cd285-109">您會在本教學課程中建立的應用程式會模擬股票行情即時看板，伺服器廣播功能的一般案例。</span><span class="sxs-lookup"><span data-stu-id="cd285-109">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span>
> 
> <span data-ttu-id="cd285-110">在本教學課程的註解是歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="cd285-110">Comments on the tutorial are welcome.</span></span> <span data-ttu-id="cd285-111">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com)。</span><span class="sxs-lookup"><span data-stu-id="cd285-111">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com).</span></span>


## <a name="overview"></a><span data-ttu-id="cd285-112">總覽</span><span class="sxs-lookup"><span data-stu-id="cd285-112">Overview</span></span>

<span data-ttu-id="cd285-113">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件會安裝 Visual Studio 專案中的模擬的範例股票行情指示器應用程式。</span><span class="sxs-lookup"><span data-stu-id="cd285-113">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package installs a sample simulated stock ticker application in a Visual Studio project.</span></span> <span data-ttu-id="cd285-114">在本教學課程的第一個部分中，您會從頭開始建立該應用程式的簡化的版本。</span><span class="sxs-lookup"><span data-stu-id="cd285-114">In the first part of this tutorial, you'll create a simplified version of that application from scratch.</span></span> <span data-ttu-id="cd285-115">在本教學課程的其餘部分，您將安裝 NuGet 套件，並檢閱它所建立的程式碼的其他功能。</span><span class="sxs-lookup"><span data-stu-id="cd285-115">In the remainder of the tutorial, you'll install the NuGet package and review the additional features and code that it creates.</span></span>

<span data-ttu-id="cd285-116">股票行情指示器應用程式是一種您想要在其中定期 「 推入 」 或廣播，通知所有連線的用戶端與伺服器的即時應用程式的代表值。</span><span class="sxs-lookup"><span data-stu-id="cd285-116">The stock ticker application is a representative of a kind of real-time application in which you want to periodically "push," or broadcast, notifications from the server to all connected clients.</span></span>

<span data-ttu-id="cd285-117">在本教學課程的第一個部分中，您將建置的應用程式會顯示方格，以使用內建的資料。</span><span class="sxs-lookup"><span data-stu-id="cd285-117">The application that you'll build in the first part of this tutorial displays a grid with stock data.</span></span>

![Stockservices.asmx 的初始版本](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

<span data-ttu-id="cd285-119">定期伺服器隨機更新股價，並將更新推送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="cd285-119">Periodically the server randomly updates stock prices and pushes the updates to all connected clients.</span></span> <span data-ttu-id="cd285-120">在瀏覽器的數字和符號**變更**並**%** 動態變更通知來回應來自伺服器的資料行。</span><span class="sxs-lookup"><span data-stu-id="cd285-120">In the browser the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="cd285-121">如果您開啟其他瀏覽器相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。</span><span class="sxs-lookup"><span data-stu-id="cd285-121">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

<span data-ttu-id="cd285-122">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="cd285-122">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="cd285-123">必要條件</span><span class="sxs-lookup"><span data-stu-id="cd285-123">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="cd285-124">建立專案</span><span class="sxs-lookup"><span data-stu-id="cd285-124">Create the project</span></span>](#createproject)
- [<span data-ttu-id="cd285-125">將 SignalR NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="cd285-125">Add the SignalR NuGet packages</span></span>](#nugetpackages)
- [<span data-ttu-id="cd285-126">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="cd285-126">Set up the server code</span></span>](#server)
- [<span data-ttu-id="cd285-127">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="cd285-127">Set up the client code</span></span>](#client)
- [<span data-ttu-id="cd285-128">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="cd285-128">Test the application</span></span>](#test)
- [<span data-ttu-id="cd285-129">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="cd285-129">Enable logging</span></span>](#enablelogging)
- [<span data-ttu-id="cd285-130">安裝並檢閱完整的 Stockservices.asmx 範例</span><span class="sxs-lookup"><span data-stu-id="cd285-130">Install and review the full StockTicker sample</span></span>](#fullsample)
- [<span data-ttu-id="cd285-131">後續步驟</span><span class="sxs-lookup"><span data-stu-id="cd285-131">Next steps</span></span>](#nextsteps)

> [!NOTE]
> <span data-ttu-id="cd285-132">如果您不想要逐步建置應用程式的步驟，您可以在新安裝 SignalR.Sample 封裝**空的 ASP.NET Web 應用程式**專案，然後執行下列步驟以取得說明程式碼的讀取。</span><span class="sxs-lookup"><span data-stu-id="cd285-132">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new **Empty ASP.NET Web Application** project, and read through these steps to get explanations of the code.</span></span> <span data-ttu-id="cd285-133">本教學課程的第一個部分涵蓋 SignalR.Sample 程式碼的子集，並第二個部分說明 SignalR.Sample 封裝中的其他功能的主要功能。</span><span class="sxs-lookup"><span data-stu-id="cd285-133">The first part of the tutorial covers a subset of the SignalR.Sample code, and the second part explains key features of the additional functionality in the SignalR.Sample package.</span></span>


<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="cd285-134">必要條件</span><span class="sxs-lookup"><span data-stu-id="cd285-134">Prerequisites</span></span>

<span data-ttu-id="cd285-135">在開始之前，請確定您有 Visual Studio 2012 或 2010 SP1 安裝在您的電腦上。</span><span class="sxs-lookup"><span data-stu-id="cd285-135">Before you start, make sure that you have Visual Studio 2012 or 2010 SP1 installed on your computer.</span></span> <span data-ttu-id="cd285-136">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得的免費 Visual Studio 2012 Express for Web。</span><span class="sxs-lookup"><span data-stu-id="cd285-136">If you don't have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express for Web.</span></span>

<span data-ttu-id="cd285-137">如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。</span><span class="sxs-lookup"><span data-stu-id="cd285-137">If you have Visual Studio 2010, make sure that [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) is installed.</span></span>

<a id="createproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="cd285-138">建立專案</span><span class="sxs-lookup"><span data-stu-id="cd285-138">Create the project</span></span>

1. <span data-ttu-id="cd285-139">從**檔案**功能表上，按一下**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="cd285-139">From the **File** menu click **New Project**.</span></span>
2. <span data-ttu-id="cd285-140">在**新的專案**對話方塊方塊中，展開**C#** 之下**範本**，然後選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="cd285-140">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="cd285-141">選取  **ASP.NET 空白 Web 應用程式**範本，將專案命名為*SignalR.StockTicker*，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="cd285-141">Select the **ASP.NET Empty Web Application** template, name the project *SignalR.StockTicker*, and click **OK**.</span></span>

    ![新增專案對話方塊](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a><span data-ttu-id="cd285-143">將 SignalR NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="cd285-143">Add the SignalR NuGet Packages</span></span>

### <a name="add-the-signalr-and-jquery-nuget-packages"></a><span data-ttu-id="cd285-144">將 SignalR 和 JQuery 的 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="cd285-144">Add the SignalR and JQuery NuGet Packages</span></span>

<span data-ttu-id="cd285-145">您可以加入至專案的 SignalR 功能，藉由安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="cd285-145">You can add SignalR functionality to a project by installing a NuGet package.</span></span>

1. <span data-ttu-id="cd285-146">按一下 **工具 |NuGet 套件管理員 |套件管理員主控台**。</span><span class="sxs-lookup"><span data-stu-id="cd285-146">Click **Tools | NuGet Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="cd285-147">在 套件管理員中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="cd285-147">Enter the following command in the package manager.</span></span>

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    <span data-ttu-id="cd285-148">SignalR 套件會安裝一些其他 NuGet 套件作為相依項目。</span><span class="sxs-lookup"><span data-stu-id="cd285-148">The SignalR package installs a number of other NuGet packages as dependencies.</span></span> <span data-ttu-id="cd285-149">安裝完成時可能會有所有的伺服器和用戶端所需的元件使用 SignalR 的 ASP.NET 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="cd285-149">When the installation is finished you have all of the server and client components required to use SignalR in an ASP.NET application.</span></span>

<a id="server"></a>

## <a name="set-up-the-server-code"></a><span data-ttu-id="cd285-150">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="cd285-150">Set up the server code</span></span>

<span data-ttu-id="cd285-151">在本節中您設定在伺服器執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cd285-151">In this section you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="cd285-152">建立庫存類別</span><span class="sxs-lookup"><span data-stu-id="cd285-152">Create the Stock class</span></span>

<span data-ttu-id="cd285-153">請先建立您將使用它來儲存和傳輸股票的相關資訊的內建模型類別。</span><span class="sxs-lookup"><span data-stu-id="cd285-153">You begin by creating the Stock model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="cd285-154">在專案資料夾中建立新的類別檔案，將其命名*Stock.cs*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="cd285-154">Create a new class file in the project folder, name it *Stock.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    <span data-ttu-id="cd285-155">當您建立的股票時，您會將兩個屬性是符號 (例如，microsoft 的 MSFT) 和價格。</span><span class="sxs-lookup"><span data-stu-id="cd285-155">The two properties that you'll set when you create stocks are the Symbol (for example, MSFT for Microsoft) and the Price.</span></span> <span data-ttu-id="cd285-156">其他屬性取決於您如何及何時設定價格。</span><span class="sxs-lookup"><span data-stu-id="cd285-156">The other properties depend on how and when you set Price.</span></span> <span data-ttu-id="cd285-157">第一次設定價格值取得傳播到 DayOpen。</span><span class="sxs-lookup"><span data-stu-id="cd285-157">The first time you set Price, the value gets propagated to DayOpen.</span></span> <span data-ttu-id="cd285-158">後續的階段，當您設定價格變更，且 PercentChange 屬性值的計算方式為基礎的價格和 DayOpen 之間的差異。</span><span class="sxs-lookup"><span data-stu-id="cd285-158">Subsequent times when you set Price, the Change and PercentChange property values are calculated based on the difference between Price and DayOpen.</span></span>

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a><span data-ttu-id="cd285-159">建立 Stockservices.asmx 和 StockTickerHub 類別</span><span class="sxs-lookup"><span data-stu-id="cd285-159">Create the StockTicker and StockTickerHub classes</span></span>

<span data-ttu-id="cd285-160">您將使用 SignalR 中樞 API 來處理伺服器到用戶端互動。</span><span class="sxs-lookup"><span data-stu-id="cd285-160">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="cd285-161">StockTickerHub 類別衍生自 SignalR Hub 類別會處理來自用戶端接收連線和方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="cd285-161">A StockTickerHub class that derives from the SignalR Hub class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="cd285-162">您也需要維護的內建資料並執行的計時器物件，以定期觸發價格更新，不需依賴用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="cd285-162">You also need to maintain stock data and run a Timer object to periodically trigger price updates, independently of client connections.</span></span> <span data-ttu-id="cd285-163">因為暫時性中樞執行個體，您無法暫停在中樞類別中，這些函式。</span><span class="sxs-lookup"><span data-stu-id="cd285-163">You can't put these functions in a Hub class, because Hub instances are transient.</span></span> <span data-ttu-id="cd285-164">在中樞內，例如連線和伺服器從用戶端呼叫的每個作業建立中樞類別執行個體。</span><span class="sxs-lookup"><span data-stu-id="cd285-164">A Hub class instance is created for each operation on the hub, such as connections and calls from the client to the server.</span></span> <span data-ttu-id="cd285-165">因此，必須執行在不同的類別，您將會命名為 Stockservices.asmx 的機制，可讓內建的資料、 更新價格，接著在廣播價格更新。</span><span class="sxs-lookup"><span data-stu-id="cd285-165">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class, which you'll name StockTicker.</span></span>

![從 Stockservices.asmx 的廣播](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

<span data-ttu-id="cd285-167">您只想 Stockservices.asmx 類別，因此您必須設定為參考從每個 StockTickerHub 執行個體的單一 Stockservices.asmx 執行個體的伺服器上，執行一個執行個體。</span><span class="sxs-lookup"><span data-stu-id="cd285-167">You only want one instance of the StockTicker class to run on the server, so you'll need to set up a reference from each StockTickerHub instance to the singleton StockTicker instance.</span></span> <span data-ttu-id="cd285-168">Stockservices.asmx 類別必須能夠向用戶端廣播，因為它具有內建的資料，以及觸發更新，但 Stockservices.asmx 不 Hub 類別。</span><span class="sxs-lookup"><span data-stu-id="cd285-168">The StockTicker class has to be able to broadcast to clients because it has the stock data and triggers updates, but StockTicker is not a Hub class.</span></span> <span data-ttu-id="cd285-169">因此，Stockservices.asmx 類別必須取得 SignalR 中樞的連接內容物件的參考。</span><span class="sxs-lookup"><span data-stu-id="cd285-169">Therefore, the StockTicker class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="cd285-170">它可以接著使用 SignalR 連線內容物件廣播給用戶端。</span><span class="sxs-lookup"><span data-stu-id="cd285-170">It can then use the SignalR connection context object to broadcast to clients.</span></span>

1. <span data-ttu-id="cd285-171">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="cd285-171">In **Solution Explorer**, right-click the project and click **Add New Item**.</span></span>
2. <span data-ttu-id="cd285-172">如果您有 Visual Studio 2012 [ASP.NET 和 Web 工具 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=279941)，按一下**Web**之下**Visual C#** ，然後選取**SignalR Hub 類別**項目範本。</span><span class="sxs-lookup"><span data-stu-id="cd285-172">If you have Visual Studio 2012 with the [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=279941), click **Web** under **Visual C#** and select the **SignalR Hub Class** item template.</span></span> <span data-ttu-id="cd285-173">否則，請選取**類別**範本。</span><span class="sxs-lookup"><span data-stu-id="cd285-173">Otherwise, select the **Class** template.</span></span>
3. <span data-ttu-id="cd285-174">新類別命名*StockTickerHub.cs*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="cd285-174">Name the new class *StockTickerHub.cs*, and then click **Add**.</span></span>

    ![Add StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. <span data-ttu-id="cd285-176">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="cd285-176">Replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    <span data-ttu-id="cd285-177">[中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別用來定義用戶端可以呼叫在伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="cd285-177">The [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class is used to define methods the clients can call on the server.</span></span> <span data-ttu-id="cd285-178">您要定義一種方法： `GetAllStocks()`。</span><span class="sxs-lookup"><span data-stu-id="cd285-178">You are defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="cd285-179">用戶端一開始會連接到伺服器，它會呼叫這個方法，以取得所有與目前的價格股票的清單。</span><span class="sxs-lookup"><span data-stu-id="cd285-179">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="cd285-180">此方法可以同步執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。</span><span class="sxs-lookup"><span data-stu-id="cd285-180">The method can execute synchronously and return `IEnumerable<Stock>` because it is returning data from memory.</span></span> <span data-ttu-id="cd285-181">如果方法必須取得資料所做的事會牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，以啟用非同步處理。</span><span class="sxs-lookup"><span data-stu-id="cd285-181">If the method had to get the data by doing something that would involve waiting, such as a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="cd285-182">如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-以非同步方式執行的時機](index.md)。</span><span class="sxs-lookup"><span data-stu-id="cd285-182">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](index.md).</span></span>

    <span data-ttu-id="cd285-183">HubName 屬性會指定中樞上的用戶端的 JavaScript 程式碼的參考方式。</span><span class="sxs-lookup"><span data-stu-id="cd285-183">The HubName attribute specifies how the Hub will be referenced in JavaScript code on the client.</span></span> <span data-ttu-id="cd285-184">如果您未使用這個屬性在用戶端上的預設名稱是類別名稱，在此情況下會 stockTickerHub 的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="cd285-184">The default name on the client if you don't use this attribute is a camel-cased version of the class name, which in this case would be stockTickerHub.</span></span>

    <span data-ttu-id="cd285-185">因為您稍後就會看到當您建立 Stockservices.asmx 類別，該類別的單一執行個體被建立在其靜態執行個體屬性中。</span><span class="sxs-lookup"><span data-stu-id="cd285-185">As you'll see later when you create the StockTicker class, a singleton instance of that class is created in its static Instance property.</span></span> <span data-ttu-id="cd285-186">Stockservices.asmx 的單一執行個體保留多少個用戶端連線或中斷連線，無論記憶體中，而且該執行個體是 GetAllStocks 方法用來傳回目前的庫存資訊。</span><span class="sxs-lookup"><span data-stu-id="cd285-186">That singleton instance of StockTicker remains in memory no matter how many clients connect or disconnect, and that instance is what the GetAllStocks method uses to return current stock information.</span></span>
5. <span data-ttu-id="cd285-187">在專案資料夾中建立新的類別檔案，將其命名*StockTicker.cs*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="cd285-187">Create a new class file in the project folder, name it *StockTicker.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    <span data-ttu-id="cd285-188">因為多個執行緒會執行為 Stockservices.asmx 的程式碼的相同執行個體，Stockservices.asmx 類別必須是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="cd285-188">Since multiple threads will be running the same instance of StockTicker code, the StockTicker class has to be threadsafe.</span></span>

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="cd285-189">將單一執行個體儲存在靜態欄位</span><span class="sxs-lookup"><span data-stu-id="cd285-189">Storing the singleton instance in a static field</span></span>

    <span data-ttu-id="cd285-190">程式碼會初始化靜態\_支援的類別和此執行個體的執行個體屬性的執行個體欄位是唯一的執行個體，可以建立的類別，因為建構函式已標示為私用。</span><span class="sxs-lookup"><span data-stu-id="cd285-190">The code initializes the static \_instance field that backs the Instance property with an instance of the class, and this is the only instance of the class that can be created, because the constructor is marked as private.</span></span> <span data-ttu-id="cd285-191">[延遲初始設定](https://msdn.microsoft.com/library/dd997286.aspx)使用於\_執行個體欄位，不會基於效能的考量，但若要確定執行個體的建立是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="cd285-191">[Lazy initialization](https://msdn.microsoft.com/library/dd997286.aspx) is used for the \_instance field, not for performance reasons but to ensure that the instance creation is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    <span data-ttu-id="cd285-192">用戶端連線到伺服器，每次在個別的執行緒中執行的 StockTickerHub 類別的新執行個體取得 Stockservices.asmx 的單一執行個體從 StockTicker.Instance 靜態屬性，如您稍早在 StockTickerHub 類別中所見。</span><span class="sxs-lookup"><span data-stu-id="cd285-192">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the StockTicker.Instance static property, as you saw earlier in the StockTickerHub class.</span></span>

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="cd285-193">在 ConcurrentDictionary 中儲存內建的資料</span><span class="sxs-lookup"><span data-stu-id="cd285-193">Storing stock data in a ConcurrentDictionary</span></span>

    <span data-ttu-id="cd285-194">建構函式初始化\_具備一些內建資料範例及 GetAllStocks 股票集合會傳回股票。</span><span class="sxs-lookup"><span data-stu-id="cd285-194">The constructor initializes the \_stocks collection with some sample stock data, and GetAllStocks returns the stocks.</span></span> <span data-ttu-id="cd285-195">如稍早所見，StockTickerHub.GetAllStocks 也就是在用戶端可以呼叫 Hub 類別中的伺服器方法接著會傳回這個集合的股票。</span><span class="sxs-lookup"><span data-stu-id="cd285-195">As you saw earlier, this collection of stocks is in turn returned by StockTickerHub.GetAllStocks which is a server method in the Hub class that clients can call.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    <span data-ttu-id="cd285-196">股票集合指[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的型別。</span><span class="sxs-lookup"><span data-stu-id="cd285-196">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="cd285-197">或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。</span><span class="sxs-lookup"><span data-stu-id="cd285-197">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

    <span data-ttu-id="cd285-198">此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料，並處置 Stockservices.asmx 的執行個體時，會遺失資料。</span><span class="sxs-lookup"><span data-stu-id="cd285-198">For this sample application, it's OK to store application data in memory and to lose the data when the StockTicker instance is disposed.</span></span> <span data-ttu-id="cd285-199">在實際的應用程式中您會使用後端資料存放區，例如資料庫。</span><span class="sxs-lookup"><span data-stu-id="cd285-199">In a real application you would work with a back-end data store such as a database.</span></span>

    ### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="cd285-200">定期更新股票價格</span><span class="sxs-lookup"><span data-stu-id="cd285-200">Periodically updating stock prices</span></span>

    <span data-ttu-id="cd285-201">建構函式會啟動計時器物件的定期呼叫更新股價隨機為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="cd285-201">The constructor starts up a Timer object that periodically calls methods that update stock prices on a random basis.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    <span data-ttu-id="cd285-202">UpdateStockPrices 是由計時器，state 參數中的 null 會傳入呼叫。</span><span class="sxs-lookup"><span data-stu-id="cd285-202">UpdateStockPrices is called by the Timer, which passes in null in the state parameter.</span></span> <span data-ttu-id="cd285-203">更新價格之前, 取得的鎖定\_updateStockPricesLock 物件。</span><span class="sxs-lookup"><span data-stu-id="cd285-203">Before updating prices, a lock is taken on the \_updateStockPricesLock object.</span></span> <span data-ttu-id="cd285-204">如果另一個執行緒正在更新價格，然後在清單中的每個股票呼叫 TryUpdateStockPrice，則會檢查程式碼。</span><span class="sxs-lookup"><span data-stu-id="cd285-204">The code checks if another thread is already updating prices, and then it calls TryUpdateStockPrice on each stock in the list.</span></span> <span data-ttu-id="cd285-205">TryUpdateStockPrice 方法會決定是否要變更的股票的價格，以及有多少變更它。</span><span class="sxs-lookup"><span data-stu-id="cd285-205">The TryUpdateStockPrice method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="cd285-206">如果變更的股價，BroadcastStockPrice 稱為廣播到所有已連線的用戶端的股票價格變更。</span><span class="sxs-lookup"><span data-stu-id="cd285-206">If the stock price is changed, BroadcastStockPrice is called to broadcast the stock price change to all connected clients.</span></span>

    <span data-ttu-id="cd285-207">\_UpdatingStockPrices 旗標標示[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)以確保其存取權是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="cd285-207">The \_updatingStockPrices flag is marked as [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to ensure that access to it is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    <span data-ttu-id="cd285-208">在實際的應用程式，TryUpdateStockPrice 方法會呼叫 web 服務，以查閱價格;在此程式碼中，它會使用隨機的數字產生器隨機進行變更。</span><span class="sxs-lookup"><span data-stu-id="cd285-208">In a real application, the TryUpdateStockPrice method would call a web service to look up the price; in this code it uses a random number generator to make changes randomly.</span></span>

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="cd285-209">取得 SignalR 內容，以便 Stockservices.asmx 類別可以廣播給用戶端</span><span class="sxs-lookup"><span data-stu-id="cd285-209">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

    <span data-ttu-id="cd285-210">因為價格變更以下來自 Stockservices.asmx 物件中，這會是需要在所有連線的用戶端上呼叫 updateStockPrice 方法的物件。</span><span class="sxs-lookup"><span data-stu-id="cd285-210">Because the price changes originate here in the StockTicker object, this is the object that needs to call an updateStockPrice method on all connected clients.</span></span> <span data-ttu-id="cd285-211">中樞類別中您的 API 呼叫的用戶端方法，但 Stockservices.asmx 並非衍生自 Hub 類別，而且沒有任何中樞物件的參考。</span><span class="sxs-lookup"><span data-stu-id="cd285-211">In a Hub class you have an API for calling client methods, but StockTicker does not derive from the Hub class and does not have a reference to any Hub object.</span></span> <span data-ttu-id="cd285-212">因此，若要廣播至連線的用戶端，Stockservices.asmx 類別必須 StockTickerHub 類別取得 SignalR 的 context 執行個體，並使用它來呼叫用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="cd285-212">Therefore, in order to broadcast to connected clients, the StockTicker class has to get the SignalR context instance for the StockTickerHub class and use that to call methods on clients.</span></span>

    <span data-ttu-id="cd285-213">建立單一類別執行個體，建構函式，將該參考時，此程式碼會取得 SignalR 內容的參考，建構函式將它放在用戶端屬性。</span><span class="sxs-lookup"><span data-stu-id="cd285-213">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the Clients property.</span></span>

    <span data-ttu-id="cd285-214">有兩個您想要一次取得內容的原因： 取得內容是昂貴的作業，並取得它一次可確保預期的順序傳送至用戶端的訊息會保留。</span><span class="sxs-lookup"><span data-stu-id="cd285-214">There are two reasons why you want to get the context just once: getting the context is an expensive operation, and getting it once ensures that the intended order of messages sent to clients is preserved.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    <span data-ttu-id="cd285-215">取得內容的用戶端屬性，並將它放在 StockTickerClient 屬性可讓您撰寫來呼叫用戶端方法看起來一樣如同中樞類別中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cd285-215">Getting the Clients property of the context and putting it in the StockTickerClient property lets you write code to call client methods that looks the same as it would in a Hub class.</span></span> <span data-ttu-id="cd285-216">比方說，廣播到所有用戶端，您可以撰寫 Clients.All.updateStockPrice(stock)。</span><span class="sxs-lookup"><span data-stu-id="cd285-216">For instance, to broadcast to all clients you can write Clients.All.updateStockPrice(stock).</span></span>

    <span data-ttu-id="cd285-217">您呼叫在 BroadcastStockPrice updateStockPrice 方法還; 不存在稍後當您撰寫的用戶端執行的程式碼時，會將其新增。</span><span class="sxs-lookup"><span data-stu-id="cd285-217">The updateStockPrice method that you are calling in BroadcastStockPrice doesn't exist yet; you'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="cd285-218">您可以參考以下 updateStockPrice 因為 Clients.All 是動態，這表示將會在執行階段評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="cd285-218">You can refer to updateStockPrice here because Clients.All is dynamic, which means the expression will be evaluated at runtime.</span></span> <span data-ttu-id="cd285-219">SignalR 當這個方法呼叫執行時，會傳送的方法名稱和參數值，用戶端，而且如果用戶端有一個名為 updateStockPrice 方法，就會呼叫該方法的參數值會傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="cd285-219">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named updateStockPrice, that method will be called and the parameter value will be passed to it.</span></span>

    <span data-ttu-id="cd285-220">Clients.All 表示傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="cd285-220">Clients.All means send to all clients.</span></span> <span data-ttu-id="cd285-221">SignalR 提供其他選項，以指定哪些用戶端或用戶端傳送至群組。</span><span class="sxs-lookup"><span data-stu-id="cd285-221">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="cd285-222">如需詳細資訊，請參閱 < [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="cd285-222">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="cd285-223">註冊 SignalR 的路由</span><span class="sxs-lookup"><span data-stu-id="cd285-223">Register the SignalR route</span></span>

<span data-ttu-id="cd285-224">伺服器必須知道要攔截，並將導向至 SignalR 的 URL。</span><span class="sxs-lookup"><span data-stu-id="cd285-224">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="cd285-225">若要如何將一些程式碼來*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="cd285-225">To do that you'll add some code to the *Global.asax* file.</span></span>

1. <span data-ttu-id="cd285-226">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="cd285-226">In **Solution Explorer**, right-click the project, and then click **Add New Item**.</span></span>
2. <span data-ttu-id="cd285-227">選取 **全域應用程式類別**項目範本，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="cd285-227">Select the **Global Application Class** item template, and then click **Add**.</span></span>

    ![加入 global.asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. <span data-ttu-id="cd285-229">將 SignalR 的路由註冊程式碼新增至應用程式\_Start 方法：</span><span class="sxs-lookup"><span data-stu-id="cd285-229">Add the SignalR route registration code to the Application\_Start method:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    <span data-ttu-id="cd285-230">根據預設，所有的 SignalR 流量的基底 URL 是"/ signalr"，而 「 signalr/中樞 」 用來擷取動態產生的 JavaScript 檔案，定義您在您的應用程式的所有主機的 proxy。</span><span class="sxs-lookup"><span data-stu-id="cd285-230">By default, the base URL for all SignalR traffic is "/signalr", and "/signalr/hubs" is used to retrieve a dynamically generated JavaScript file that defines proxies for all the Hubs you have in your application.</span></span> <span data-ttu-id="cd285-231">MapHubs 方法包含可讓您指定不同的基底 URL 和特定 SignalR 選項的執行個體中的多載[HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="cd285-231">The MapHubs method includes overloads that let you specify a different base URL and certain SignalR options in an instance of the [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) class.</span></span>
4. <span data-ttu-id="cd285-232">加入 using 陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="cd285-232">Add a using statement at the top of the file:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. <span data-ttu-id="cd285-233">儲存並關閉*Global.asax*檔案，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="cd285-233">Save and close the *Global.asax* file, and build the project.</span></span>

<span data-ttu-id="cd285-234">您現在已完成設定伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="cd285-234">You have now completed setting up the server code.</span></span> <span data-ttu-id="cd285-235">在下一節中您會設定用戶端。</span><span class="sxs-lookup"><span data-stu-id="cd285-235">In the next section you'll set up the client.</span></span>

<a id="client"></a>

## <a name="set-up-the-client-code"></a><span data-ttu-id="cd285-236">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="cd285-236">Set up the client code</span></span>

1. <span data-ttu-id="cd285-237">新的 HTML 檔案的資料夾中建立專案，並將它命名*StockTicker.html*。</span><span class="sxs-lookup"><span data-stu-id="cd285-237">Create a new HTML file in the project folder, and name it *StockTicker.html*.</span></span>
2. <span data-ttu-id="cd285-238">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="cd285-238">Replace the template code with the following code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    <span data-ttu-id="cd285-239">HTML 5 個資料行、 標頭資料列與資料列與單一儲存格跨越所有的 5 個資料行建立的資料表。</span><span class="sxs-lookup"><span data-stu-id="cd285-239">The HTML creates a table with 5 columns, a header row, and a data row with a single cell that spans all 5 columns.</span></span> <span data-ttu-id="cd285-240">資料列會顯示 「 正在載入...」，才會暫時在應用程式啟動時，才會顯示。</span><span class="sxs-lookup"><span data-stu-id="cd285-240">The data row displays "loading..." and will only be shown momentarily when the application starts.</span></span> <span data-ttu-id="cd285-241">JavaScript 程式碼將會移除該資料列，並在其位置的資料列中新增，並從伺服器擷取的內建資料。</span><span class="sxs-lookup"><span data-stu-id="cd285-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="cd285-242">指令碼標記指定 jQuery 指令碼檔案，SignalR core 指令碼檔案、 SignalR proxy 指令碼檔案中，您稍後將建立 Stockservices.asmx 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="cd285-242">The script tags specify the jQuery script file, the SignalR core script file, the SignalR proxies script file, and a StockTicker script file that you'll create later.</span></span> <span data-ttu-id="cd285-243">SignalR proxy 指令碼檔案，指定 「 signalr/中樞 」 的 URL，以動態方式產生，並在中樞類別、 方法的 proxy 方法定義在此情況下 StockTickerHub.GetAllStocks。</span><span class="sxs-lookup"><span data-stu-id="cd285-243">The SignalR proxies script file, which specifies the "/signalr/hubs" URL, is dynamically generated and defines proxy methods for the methods on the Hub class, in this case for StockTickerHub.GetAllStocks.</span></span> <span data-ttu-id="cd285-244">如果您想，您可以產生這個 JavaScript 檔案以手動方式使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)和停用動態檔案建立 MapHubs 方法呼叫中的。</span><span class="sxs-lookup"><span data-stu-id="cd285-244">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) and disable dynamic file creation in the MapHubs method call.</span></span>
3. > [!IMPORTANT]
   > <span data-ttu-id="cd285-245">請確定 JavaScript 檔案參考中*StockTicker.html*正確無誤。</span><span class="sxs-lookup"><span data-stu-id="cd285-245">Make sure that the JavaScript file references in *StockTicker.html* are correct.</span></span> <span data-ttu-id="cd285-246">也就是確定您的指令碼標記 (在範例中的 1.8.2) 中的 jQuery 版本是 jQuery 版本在專案的相同*指令碼*資料夾，並確定您的指令碼標記中的 SignalR 版本是 SignalR 相同在您的專案中的版本*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="cd285-246">That is, make sure that the jQuery version in your script tag (1.8.2 in the example) is the same as the jQuery version in your project's *Scripts* folder, and make sure that the SignalR version in your script tag is the same as the SignalR version in your project's *Scripts* folder.</span></span> <span data-ttu-id="cd285-247">如有必要，請變更指令碼標記中的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="cd285-247">Change the file names in the script tags if necessary.</span></span>
4. <span data-ttu-id="cd285-248">在 **方案總管 中**，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="cd285-248">In **Solution Explorer**, right-click *StockTicker.html*, and then click **Set as Start Page**.</span></span>
5. <span data-ttu-id="cd285-249">在專案資料夾中建立新的 JavaScript 檔案並將它命名*StockTicker.js*...</span><span class="sxs-lookup"><span data-stu-id="cd285-249">Create a new JavaScript file in the project folder and name it *StockTicker.js*..</span></span>
6. <span data-ttu-id="cd285-250">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="cd285-250">Replace the template code with the following code:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    <span data-ttu-id="cd285-251">$.connection 指 SignalR proxy。</span><span class="sxs-lookup"><span data-stu-id="cd285-251">$.connection refers to the SignalR proxies.</span></span> <span data-ttu-id="cd285-252">程式碼取得 StockTickerHub 類別將 proxy 的參考，並將它放在股票行情指示器變數中。</span><span class="sxs-lookup"><span data-stu-id="cd285-252">The code gets a reference to the proxy for the StockTickerHub class and puts it in the ticker variable.</span></span> <span data-ttu-id="cd285-253">Proxy 名稱會是 [HubName] 屬性所設定的名稱：</span><span class="sxs-lookup"><span data-stu-id="cd285-253">The proxy name is the name that was set by the [HubName] attribute:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    <span data-ttu-id="cd285-254">定義所有變數和函式之後，檔案中的程式碼的最後一行會初始化 SignalR 連線，藉由呼叫 SignalR 開始函式。</span><span class="sxs-lookup"><span data-stu-id="cd285-254">After all the variables and functions are defined, the last line of code in the file initializes the SignalR connection by calling the SignalR start function.</span></span> <span data-ttu-id="cd285-255">開始函式以非同步方式執行，並傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)，這表示您可以呼叫完成的函式，以指定的非同步作業完成時要呼叫的函式...</span><span class="sxs-lookup"><span data-stu-id="cd285-255">The start function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means you can call the done function to specify the function to call when the asynchronous operation is completed..</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    <span data-ttu-id="cd285-256">Init 函式 getAllStocks 函式會呼叫伺服器上，並使用伺服器來更新內建的資料表所傳回的資訊。</span><span class="sxs-lookup"><span data-stu-id="cd285-256">The init function calls the getAllStocks function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="cd285-257">請注意，根據預設，您必須使用駝峰式命名法大小寫用戶端上雖然方法名稱是依照 pascal 命名法大小寫，在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="cd285-257">Notice that by default, you have to use camel casing on the client although the method name is pascal-cased on the server.</span></span> <span data-ttu-id="cd285-258">依照 camel 命名法大小寫規則只適用於不是物件的方法。</span><span class="sxs-lookup"><span data-stu-id="cd285-258">The camel-casing rule only applies to methods, not objects.</span></span> <span data-ttu-id="cd285-259">例如，您參考股票。符號 」 與 「 存貨。價格、 未 stock.symbol 或 stock.price。</span><span class="sxs-lookup"><span data-stu-id="cd285-259">For example, you refer to stock.Symbol and stock.Price, not stock.symbol or stock.price.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    <span data-ttu-id="cd285-260">如果您想要在用戶端，使用 pascal 命名法大小寫，或如果您想要使用完全不同的方法名稱，您可以裝飾具有 HubMethodName 屬性之中樞方法相同的方式會裝飾 Hub 類別具有 HubName 屬性。</span><span class="sxs-lookup"><span data-stu-id="cd285-260">If you wanted to use pascal casing on the client, or if you wanted to use a completely different method name, you could decorate the Hub method with the HubMethodName attribute the same way you decorated the Hub class itself with the HubName attribute.</span></span>

    <span data-ttu-id="cd285-261">在 init 方法中，HTML 資料表資料列會建立從伺服器收到的呼叫 formatStock 格式內容的內建的物件，每一個股票物件，並藉由呼叫取代式裝置 (定義在頂端*StockTicker.js*) 取代內建的物件屬性值的 rowTemplate 變數中的預留位置。</span><span class="sxs-lookup"><span data-stu-id="cd285-261">In the init method, HTML for a table row is created for each stock object received from the server by calling formatStock to format properties of the stock object, and then by calling supplant (which is defined at the top of *StockTicker.js*) to replace placeholders in the rowTemplate variable with the stock object property values.</span></span> <span data-ttu-id="cd285-262">產生的 HTML 則會附加至內建的資料表中。</span><span class="sxs-lookup"><span data-stu-id="cd285-262">The resulting HTML is then appended to the stock table.</span></span>

    <span data-ttu-id="cd285-263">您可以傳遞，以執行非同步的啟動函式完成後的回呼函式呼叫 init。</span><span class="sxs-lookup"><span data-stu-id="cd285-263">You call init by passing it in as a callback function that executes after the asynchronous start function completes.</span></span> <span data-ttu-id="cd285-264">如果您呼叫 init 做為個別的 JavaScript 陳述式之後呼叫開始時，函式會失敗，因為它會立即執行而不需等待開始函式，來完成 建立連線。</span><span class="sxs-lookup"><span data-stu-id="cd285-264">If you called init as a separate JavaScript statement after calling start, the function would fail because it would execute immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="cd285-265">在此情況下，init 函式會嘗試建立伺服器連接之前呼叫 getAllStocks 函式。</span><span class="sxs-lookup"><span data-stu-id="cd285-265">In that case, the init function would try to call the getAllStocks function before the server connection is established.</span></span>

    <span data-ttu-id="cd285-266">當伺服器變成股票的價格時，它會呼叫 updateStockPrice 連線的用戶端上。</span><span class="sxs-lookup"><span data-stu-id="cd285-266">When the server changes a stock's price, it calls the updateStockPrice on connected clients.</span></span> <span data-ttu-id="cd285-267">此函式會新增至 Stockservices.asmx proxy 的用戶端屬性，以便將它提供給呼叫從伺服器。</span><span class="sxs-lookup"><span data-stu-id="cd285-267">The function is added to the client property of the stockTicker proxy in order to make it available to calls from the server.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    <span data-ttu-id="cd285-268">UpdateStockPrice 函式會將接收自伺服器的資料表資料列到 init 函式的相同方式將內建物件格式化。</span><span class="sxs-lookup"><span data-stu-id="cd285-268">The updateStockPrice function formats a stock object received from the server into a table row the same way as in the init function.</span></span> <span data-ttu-id="cd285-269">不過，而不是將資料列附加至資料表，它在資料表中尋找股票的目前資料列，並以新的取代該資料列。</span><span class="sxs-lookup"><span data-stu-id="cd285-269">However, instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

<a id="test"></a>

## <a name="test-the-application"></a><span data-ttu-id="cd285-270">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="cd285-270">Test the application</span></span>

1. <span data-ttu-id="cd285-271">按 F5 以偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cd285-271">Press F5 to run the application in debug mode.</span></span>

    <span data-ttu-id="cd285-272">內建資料表一開始會顯示 「 正在載入...」 列，然後顯示初始的內建資料，在短暫延遲之後，再股票價格開始變更。</span><span class="sxs-lookup"><span data-stu-id="cd285-272">The stock table initially displays the "loading..." line, then after a short delay the initial stock data is displayed, and then the stock prices start to change.</span></span>

    ![正在載入](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![初始的內建資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![從伺服器接收變更內建的資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. <span data-ttu-id="cd285-276">從瀏覽器網址列複製 URL，並將它貼到一或多個新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="cd285-276">Copy the URL from the browser address bar and paste it into one or more new browser window(s).</span></span>

    <span data-ttu-id="cd285-277">初始的內建顯示等同於第一個瀏覽器，並同時進行變更。</span><span class="sxs-lookup"><span data-stu-id="cd285-277">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>
3. <span data-ttu-id="cd285-278">關閉所有瀏覽器並開啟新的瀏覽器，然後移至相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="cd285-278">Close all browsers and open a new browser, then go to the same URL.</span></span>

    <span data-ttu-id="cd285-279">若要在伺服器中執行，因此內建的資料表會顯示股票，若要變更持續一直在 Stockservices.asmx 單一物件。</span><span class="sxs-lookup"><span data-stu-id="cd285-279">The StockTicker singleton object has continued to run in the server, so the stock table display shows that the stocks have continued to change.</span></span> <span data-ttu-id="cd285-280">（您沒有看到變更數據的初始資料表為零）。</span><span class="sxs-lookup"><span data-stu-id="cd285-280">(You don't see the initial table with zero change figures.)</span></span>
4. <span data-ttu-id="cd285-281">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cd285-281">Close the browser.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="cd285-282">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="cd285-282">Enable logging</span></span>

<span data-ttu-id="cd285-283">SignalR 有內建記錄功能，您可以讓用戶端上，以協助疑難排解。</span><span class="sxs-lookup"><span data-stu-id="cd285-283">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="cd285-284">在本節中您要啟用記錄，並查看範例，說明如何記錄告訴您哪一種下列傳輸方法使用 SignalR:</span><span class="sxs-lookup"><span data-stu-id="cd285-284">In this section you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

- <span data-ttu-id="cd285-285">[Websocket](http://en.wikipedia.org/wiki/WebSocket)、 支援的 IIS 8 和目前的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cd285-285">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>
- <span data-ttu-id="cd285-286">[伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。</span><span class="sxs-lookup"><span data-stu-id="cd285-286">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>
- <span data-ttu-id="cd285-287">[永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet explorer 的支援。</span><span class="sxs-lookup"><span data-stu-id="cd285-287">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>
- <span data-ttu-id="cd285-288">[長時間輪詢的 Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cd285-288">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="cd285-289">針對任何指定的連接，SignalR 會選擇最佳的伺服器和用戶端支援的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="cd285-289">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="cd285-290">開啟*StockTicker.js*並新增一行程式碼，若要啟用記錄之前初始化的連線，在檔案結尾處的程式碼：</span><span class="sxs-lookup"><span data-stu-id="cd285-290">Open *StockTicker.js* and add a line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. <span data-ttu-id="cd285-291">按 F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="cd285-291">Press F5 to run the project.</span></span>
3. <span data-ttu-id="cd285-292">開啟瀏覽器的開發人員工具 視窗，然後選取主控台，以查看記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cd285-292">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="cd285-293">您可能必須重新整理頁面，以查看 Signalr 交涉新連線的傳輸方法的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="cd285-293">You might have to refresh the page to see the logs of Signalr negotiating the transport method for a new connection.</span></span>

    <span data-ttu-id="cd285-294">如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，傳輸方法是 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="cd285-294">If you are running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![IE 10 IIS 8 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    <span data-ttu-id="cd285-296">如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，傳輸方法是 iframe。</span><span class="sxs-lookup"><span data-stu-id="cd285-296">If you are running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is iframe.</span></span>

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    <span data-ttu-id="cd285-298">在 Firefox 中安裝 Firebug 增益集以取得主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="cd285-298">In Firefox, install the Firebug add-in to get a Console window.</span></span> <span data-ttu-id="cd285-299">如果您在 Windows 8 (IIS 8) 來執行 Firefox 19，傳輸方法是 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="cd285-299">If you are running Firefox 19 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    <span data-ttu-id="cd285-301">如果您在 Windows 7 (IIS 7.5) 上執行 Firefox 19，傳輸方法就會是伺服器傳送事件。</span><span class="sxs-lookup"><span data-stu-id="cd285-301">If you are running Firefox 19 on Windows 7 (IIS 7.5), the transport method is server-sent events.</span></span>

    ![Firefox 19 IIS 7.5 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a><span data-ttu-id="cd285-303">安裝並檢閱完整的 Stockservices.asmx 範例</span><span class="sxs-lookup"><span data-stu-id="cd285-303">Install and review the full StockTicker sample</span></span>

<span data-ttu-id="cd285-304">Stockservices.asmx 應用程式會安裝[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件包含您剛從頭開始建立簡化版本較多的功能。</span><span class="sxs-lookup"><span data-stu-id="cd285-304">The StockTicker application that is installed by the [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package includes more features than the simplified version that you just created from scratch.</span></span> <span data-ttu-id="cd285-305">在本節的教學課程中，您要安裝 NuGet 套件，並檢閱新功能，以及實作它們的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cd285-305">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="cd285-306">安裝 SignalR.Sample NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="cd285-306">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="cd285-307">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="cd285-307">In **Solution Explorer**, right-click the project and click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="cd285-308">在 **管理 NuGet 套件** 對話方塊中，按一下**線上**，輸入*SignalR.Sample*中**線上搜尋**方塊，然後再按一下  **安裝**中**SignalR.Sample**封裝。</span><span class="sxs-lookup"><span data-stu-id="cd285-308">In the **Manage NuGet Packages** dialog box, click **Online**, enter *SignalR.Sample* in the **Search Online** box, and then click **Install** in the **SignalR.Sample** package.</span></span>

    ![安裝 SignalR.Sample 套件](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. <span data-ttu-id="cd285-310">在  *Global.asax*檔案，標記為註解 RouteTable.Routes.MapHubs(); 行您稍早在應用程式中新增\_Start 方法。</span><span class="sxs-lookup"><span data-stu-id="cd285-310">In the *Global.asax* file, comment out the RouteTable.Routes.MapHubs(); line that you added earlier in the Application\_Start method.</span></span>

    <span data-ttu-id="cd285-311">中的程式碼*Global.asax*不再需要因為 SignalR.Sample 封裝註冊中的 SignalR 路由*應用程式\_Start/RegisterHubs.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="cd285-311">The code in *Global.asax* is no longer needed because the SignalR.Sample package registers the SignalR route in the *App\_Start/RegisterHubs.cs* file:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    <span data-ttu-id="cd285-312">組件屬性所參考的 WebActivator 類別一併併入 WebActivatorEx NuGet 封裝，它會安裝為 SignalR.Sample 封裝相依性。</span><span class="sxs-lookup"><span data-stu-id="cd285-312">The WebActivator class that is referenced by the assembly attribute is included in the WebActivatorEx NuGet package, which is installed as a dependency of the SignalR.Sample package.</span></span>
4. <span data-ttu-id="cd285-313">在 [**方案總管] 中**，展開*SignalR.Sample*安裝 SignalR.Sample 封裝所建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="cd285-313">In **Solution Explorer**, expand the *SignalR.Sample* folder which was created by installing the SignalR.Sample package.</span></span>
5. <span data-ttu-id="cd285-314">在  *SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="cd285-314">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then click **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd285-315">安裝 SignalR.Sample NuGet 套件可能會變更您在的 jQuery 版本您*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="cd285-315">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="cd285-316">新*StockTicker.html*套件會安裝中的檔案*SignalR.Sample*資料夾將會是套件會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記中的 jQuery 參考。</span><span class="sxs-lookup"><span data-stu-id="cd285-316">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="cd285-317">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="cd285-317">Run the application</span></span>

1. <span data-ttu-id="cd285-318">按 F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cd285-318">Press F5 to run the application.</span></span>

    <span data-ttu-id="cd285-319">方格中稍早所見，除了完整股票行情指示器應用程式會顯示可顯示相同的股票資料的水平捲動視窗。</span><span class="sxs-lookup"><span data-stu-id="cd285-319">In addition to the grid that you saw earlier, the full stock ticker application shows a horizontally scrolling window that displays the same stock data.</span></span> <span data-ttu-id="cd285-320">當您執行第一次應用程式時，「 市場 」 的動作 「 關閉 」，您會看到靜態方格和不捲動的股票行情指示器視窗。</span><span class="sxs-lookup"><span data-stu-id="cd285-320">When you run the application for the first time, the "market" is "closed" and you see a static grid and a ticker window that isn't scrolling.</span></span>

    ![Stockservices.asmx 畫面開始](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    <span data-ttu-id="cd285-322">當您按一下 **開放性市場**，則**即時股票行情即時看板**開始的水平捲動方塊，且伺服器會開始定期廣播隨機的方式上的股票價格變更。</span><span class="sxs-lookup"><span data-stu-id="cd285-322">When you click **Open Market**, the **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span> <span data-ttu-id="cd285-323">每次股票的價格變更，同時**即時股票表格**格線和**即時股票行情即時看板**方塊會更新。</span><span class="sxs-lookup"><span data-stu-id="cd285-323">Each time a stock price changes, both the **Live Stock Table** grid and the **Live Stock Ticker** box are updated.</span></span> <span data-ttu-id="cd285-324">具有綠色背景，正股票的價格變動時，會顯示股票和股票時變更為負數，會顯示具有紅色背景。</span><span class="sxs-lookup"><span data-stu-id="cd285-324">When a stock's price change is positive, the stock is shown with a green background, and when the change is negative, the stock is shown with a red background.</span></span>

    ![開啟 Stockservices.asmx 的應用程式市集](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    <span data-ttu-id="cd285-326">**關閉市場** 按鈕會停止所做的變更，並停止股票行情指示器捲動，而**重設**按鈕會重設內建的所有資料的初始狀態價格變動啟動之前。</span><span class="sxs-lookup"><span data-stu-id="cd285-326">The **Close Market** button stops the changes and stops the ticker scrolling, and the **Reset** button resets all stock data to the initial state before price changes started.</span></span> <span data-ttu-id="cd285-327">如果您開啟多個瀏覽器視窗，並移至相同的 URL，您會看到相同的資料，動態更新在此同時在每個瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="cd285-327">If you open more browser windows and go to the same URL, you see the same data dynamically updated at the same time in each browser.</span></span> <span data-ttu-id="cd285-328">當您按一下其中一個按鈕時，則所有瀏覽器會回應相同的方式在相同的時間。</span><span class="sxs-lookup"><span data-stu-id="cd285-328">When you click one of the buttons, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="cd285-329">即時股票行情指示器顯示</span><span class="sxs-lookup"><span data-stu-id="cd285-329">Live Stock Ticker display</span></span>

<span data-ttu-id="cd285-330">**即時股票行情即時看板**顯示會格式化為單一行的 CSS 樣式的 div 元素中的未排序的清單。</span><span class="sxs-lookup"><span data-stu-id="cd285-330">The **Live Stock Ticker** display is an unordered list in a div element that is formatted into a single line by CSS styles.</span></span> <span data-ttu-id="cd285-331">股票行情指示器會初始化並更新資料表的相同方式來： 取代中的預留位置&lt;li&gt;範本字串，並以動態方式新增&lt;li&gt;項目&lt;ul&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="cd285-331">The ticker is initialized and updated the same way as the table: by replacing placeholders in a &lt;li&gt; template string and dynamically adding the &lt;li&gt; elements to the &lt;ul&gt; element.</span></span> <span data-ttu-id="cd285-332">使用 jQuery 動畫函式來改變的未排序的清單內的 div。 margin-left 執行捲動</span><span class="sxs-lookup"><span data-stu-id="cd285-332">The scrolling is performed by using the jQuery animate function to vary the margin-left of the unordered list within the div.</span></span>

<span data-ttu-id="cd285-333">股票行情即時看板 HTML:</span><span class="sxs-lookup"><span data-stu-id="cd285-333">The stock ticker HTML:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

<span data-ttu-id="cd285-334">股票行情即時看板 CSS:</span><span class="sxs-lookup"><span data-stu-id="cd285-334">The stock ticker CSS:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

<span data-ttu-id="cd285-335">JQuery 程式碼，可讓捲動：</span><span class="sxs-lookup"><span data-stu-id="cd285-335">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="cd285-336">用戶端可以呼叫的伺服器上的其他方法</span><span class="sxs-lookup"><span data-stu-id="cd285-336">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="cd285-337">StockTickerHub 類別會定義四個用戶端可以呼叫其他方法：</span><span class="sxs-lookup"><span data-stu-id="cd285-337">The StockTickerHub class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

<span data-ttu-id="cd285-338">OpenMarket、 CloseMarket 和重設稱為回應位於頁面頂端的按鈕。</span><span class="sxs-lookup"><span data-stu-id="cd285-338">OpenMarket, CloseMarket, and Reset are called in response to the buttons at the top of the page.</span></span> <span data-ttu-id="cd285-339">它們會示範一個觸發會立即傳播到所有用戶端的狀態變更的用戶端的模式。</span><span class="sxs-lookup"><span data-stu-id="cd285-339">They demonstrate the pattern of one client triggering a change in state that is immediately propagated to all clients.</span></span> <span data-ttu-id="cd285-340">每個方法呼叫的方法 Stockservices.asmx 類別中會影響市場狀態變更，然後再廣播的新狀態。</span><span class="sxs-lookup"><span data-stu-id="cd285-340">Each of these methods calls a method in the StockTicker class that effects the market state change and then broadcasts the new state.</span></span>

<span data-ttu-id="cd285-341">在 Stockservices.asmx 類別中，由傳回 MarketState 列舉值的 MarketState 屬性維護的市場狀況：</span><span class="sxs-lookup"><span data-stu-id="cd285-341">In the StockTicker class, the state of the market is maintained by a MarketState property that returns a MarketState enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

<span data-ttu-id="cd285-342">每個市場狀態變更的方法在內進行的鎖定區塊 Stockservices.asmx 類別必須為 threadsafe 因為：</span><span class="sxs-lookup"><span data-stu-id="cd285-342">Each of the methods that change the market state do so inside a lock block because the StockTicker class has to be threadsafe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

<span data-ttu-id="cd285-343">若要確保此程式碼 threadsafe， \_marketState 欄位支援 MarketState 屬性會標示為 volatile，</span><span class="sxs-lookup"><span data-stu-id="cd285-343">To ensure that this code is threadsafe, the \_marketState field that backs the MarketState property is marked as volatile,</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

<span data-ttu-id="cd285-344">BroadcastMarketStateChange 和 BroadcastMarketReset 方法都是類似於 BroadcastStockPrice 方法，您看到的但在呼叫不同的方法，在用戶端定義：</span><span class="sxs-lookup"><span data-stu-id="cd285-344">The BroadcastMarketStateChange and BroadcastMarketReset methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="cd285-345">用戶端可以呼叫伺服器上的其他函式</span><span class="sxs-lookup"><span data-stu-id="cd285-345">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="cd285-346">方格與顯示的 ticker，現在會處理 updateStockPrice 函式，並使用 jQuery.Color 閃爍紅色和綠色的色彩。</span><span class="sxs-lookup"><span data-stu-id="cd285-346">The updateStockPrice function now handles both the grid and the ticker display, and it uses jQuery.Color to flash red and green colors.</span></span>

<span data-ttu-id="cd285-347">中的新函式*SignalR.StockTicker.js*啟用和停用按鈕根據市場的狀態，和它們停止或啟動股票行情指示器視窗水平捲動。</span><span class="sxs-lookup"><span data-stu-id="cd285-347">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state, and they stop or start the ticker window horizontal scrolling.</span></span> <span data-ttu-id="cd285-348">因為 ticker.client，將會新增多個函式[jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)用來新增它們。</span><span class="sxs-lookup"><span data-stu-id="cd285-348">Since multiple functions are being added to ticker.client, the [jQuery extend function](http://api.jquery.com/jQuery.extend/) is used to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="cd285-349">建立連線後的其他用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="cd285-349">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="cd285-350">用戶端建立連接之後，它會有一些額外的工作，執行： 找出市場上是否已開啟或關閉才能呼叫適當的 marketOpened 或 marketClosed 函式，並附加至按鈕的伺服器方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="cd285-350">After the client establishes the connection, it has some additional work to do: find out if the market is open or closed in order to call the appropriate marketOpened or marketClosed function, and attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

<span data-ttu-id="cd285-351">伺服器方法不是連線到之前的按鈕之後建立的連線，這樣的程式碼無法嘗試呼叫伺服器方法，才能使用。</span><span class="sxs-lookup"><span data-stu-id="cd285-351">The server methods are not wired up to the buttons until after the connection is established, so that the code can't try to call the server methods before they are available.</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="cd285-352">後續步驟</span><span class="sxs-lookup"><span data-stu-id="cd285-352">Next steps</span></span>

<span data-ttu-id="cd285-353">在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將訊息從伺服器廣播給所有已連線的用戶端，定期執行並以通知從任何用戶端的回應方式。</span><span class="sxs-lookup"><span data-stu-id="cd285-353">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients, both on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="cd285-354">使用多執行緒的單一執行個體維護伺服器狀態的模式也也可用在多玩家線上遊戲案例中。</span><span class="sxs-lookup"><span data-stu-id="cd285-354">The pattern of using a multi-threaded singleton instance to maintain server state can also be also used in multi-player online game scenarios.</span></span> <span data-ttu-id="cd285-355">如需範例，請參閱[ShootR 遊戲為基礎的 SignalR](https://github.com/NTaylorMullen/ShootR)。</span><span class="sxs-lookup"><span data-stu-id="cd285-355">For an example, see [the ShootR game that is based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="cd285-356">顯示對等通訊案例的教學課程，請參閱[開始使用 SignalR](index.md)並[即時更新與 SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="cd285-356">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](index.md) and [Real-Time Updating with SignalR](index.md).</span></span>

<span data-ttu-id="cd285-357">若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="cd285-357">To learn more advanced SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="cd285-358">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="cd285-358">ASP.NET SignalR</span></span>](https://asp.net/signalr/)
- [<span data-ttu-id="cd285-359">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="cd285-359">SignalR Project</span></span>](http://signalr.net/)
- [<span data-ttu-id="cd285-360">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="cd285-360">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="cd285-361">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="cd285-361">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
