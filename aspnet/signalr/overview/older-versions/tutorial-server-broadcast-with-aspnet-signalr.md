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
ms.locfileid: "30879513"
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a><span data-ttu-id="e8704-104">教學課程： 透過 ASP.NET SignalR 的伺服器廣播 1.x</span><span class="sxs-lookup"><span data-stu-id="e8704-104">Tutorial: Server Broadcast with ASP.NET SignalR 1.x</span></span>
====================
<span data-ttu-id="e8704-105">由[Patrick Fletcher](https://github.com/pfletcher)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e8704-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="e8704-106">本教學課程會示範如何建立使用 ASP.NET SignalR 提供廣播的伺服器功能的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e8704-106">This tutorial shows how to create a web application that uses ASP.NET SignalR to provide server broadcast functionality.</span></span> <span data-ttu-id="e8704-107">伺服器廣播表示傳送至用戶端的通訊由伺服器起始。</span><span class="sxs-lookup"><span data-stu-id="e8704-107">Server broadcast means that communications sent to clients are initiated by the server.</span></span> <span data-ttu-id="e8704-108">此案例會需要不同的程式設計方法，與對等案例，例如交談應用程式，在其中傳送至用戶端通訊所起始的一或多個用戶端。</span><span class="sxs-lookup"><span data-stu-id="e8704-108">This scenario requires a different programming approach than peer-to-peer scenarios such as chat applications, in which communications sent to clients are initiated by one or more of the clients.</span></span>
> 
> <span data-ttu-id="e8704-109">您會在本教學課程中建立的應用程式會模擬股票行情指示器伺服器廣播功能的常見情況。</span><span class="sxs-lookup"><span data-stu-id="e8704-109">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span>
> 
> <span data-ttu-id="e8704-110">在本教學課程的註解是歡迎畫面。</span><span class="sxs-lookup"><span data-stu-id="e8704-110">Comments on the tutorial are welcome.</span></span> <span data-ttu-id="e8704-111">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com)。</span><span class="sxs-lookup"><span data-stu-id="e8704-111">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com).</span></span>


## <a name="overview"></a><span data-ttu-id="e8704-112">總覽</span><span class="sxs-lookup"><span data-stu-id="e8704-112">Overview</span></span>

<span data-ttu-id="e8704-113">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 封裝會安裝 Visual Studio 專案中的範例模擬股票行情指示器應用程式。</span><span class="sxs-lookup"><span data-stu-id="e8704-113">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package installs a sample simulated stock ticker application in a Visual Studio project.</span></span> <span data-ttu-id="e8704-114">在本教學課程的第一個部分中，您會從頭開始建立該應用程式的簡化的版本。</span><span class="sxs-lookup"><span data-stu-id="e8704-114">In the first part of this tutorial, you'll create a simplified version of that application from scratch.</span></span> <span data-ttu-id="e8704-115">本教學課程的其餘部分，將安裝 NuGet 套件，並檢閱程式碼所建立的其他功能。</span><span class="sxs-lookup"><span data-stu-id="e8704-115">In the remainder of the tutorial, you'll install the NuGet package and review the additional features and code that it creates.</span></span>

<span data-ttu-id="e8704-116">股票行情指示器應用程式是一種要在其中您定期 「 推入 」 或廣播，通知所有連線的用戶端與伺服器的即時應用程式是代表值。</span><span class="sxs-lookup"><span data-stu-id="e8704-116">The stock ticker application is a representative of a kind of real-time application in which you want to periodically "push," or broadcast, notifications from the server to all connected clients.</span></span>

<span data-ttu-id="e8704-117">在本教學課程的第一個部分中，您將建置的應用程式會顯示股票資料的方格。</span><span class="sxs-lookup"><span data-stu-id="e8704-117">The application that you'll build in the first part of this tutorial displays a grid with stock data.</span></span>

![StockTicker 初始版本](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

<span data-ttu-id="e8704-119">定期伺服器隨機更新股價，並將更新推送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e8704-119">Periodically the server randomly updates stock prices and pushes the updates to all connected clients.</span></span> <span data-ttu-id="e8704-120">在瀏覽器的數字和符號**變更**和**%** 動態變更通知來回應來自伺服器的資料行。</span><span class="sxs-lookup"><span data-stu-id="e8704-120">In the browser the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="e8704-121">如果您開啟其他瀏覽器中，相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。</span><span class="sxs-lookup"><span data-stu-id="e8704-121">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

<span data-ttu-id="e8704-122">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="e8704-122">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="e8704-123">必要條件</span><span class="sxs-lookup"><span data-stu-id="e8704-123">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="e8704-124">建立專案</span><span class="sxs-lookup"><span data-stu-id="e8704-124">Create the project</span></span>](#createproject)
- [<span data-ttu-id="e8704-125">將 SignalR NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="e8704-125">Add the SignalR NuGet packages</span></span>](#nugetpackages)
- [<span data-ttu-id="e8704-126">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="e8704-126">Set up the server code</span></span>](#server)
- [<span data-ttu-id="e8704-127">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="e8704-127">Set up the client code</span></span>](#client)
- [<span data-ttu-id="e8704-128">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e8704-128">Test the application</span></span>](#test)
- [<span data-ttu-id="e8704-129">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="e8704-129">Enable logging</span></span>](#enablelogging)
- [<span data-ttu-id="e8704-130">安裝並檢閱完整 StockTicker 範例</span><span class="sxs-lookup"><span data-stu-id="e8704-130">Install and review the full StockTicker sample</span></span>](#fullsample)
- [<span data-ttu-id="e8704-131">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e8704-131">Next steps</span></span>](#nextsteps)

> [!NOTE]
> <span data-ttu-id="e8704-132">如果您不想要逐步完成建立應用程式的步驟，您可以在新安裝 SignalR.Sample 封裝**空的 ASP.NET Web 應用程式**專案，然後執行下列步驟以取得說明程式碼的讀取。</span><span class="sxs-lookup"><span data-stu-id="e8704-132">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new **Empty ASP.NET Web Application** project, and read through these steps to get explanations of the code.</span></span> <span data-ttu-id="e8704-133">本教學課程的第一個部分涵蓋 SignalR.Sample 程式碼的子集，而且第二個部分說明 SignalR.Sample 封裝中的其他功能的主要功能。</span><span class="sxs-lookup"><span data-stu-id="e8704-133">The first part of the tutorial covers a subset of the SignalR.Sample code, and the second part explains key features of the additional functionality in the SignalR.Sample package.</span></span>


<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="e8704-134">必要條件</span><span class="sxs-lookup"><span data-stu-id="e8704-134">Prerequisites</span></span>

<span data-ttu-id="e8704-135">開始之前，請確定您有 Visual Studio 2012 或 2010 SP1 安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="e8704-135">Before you start, make sure that you have Visual Studio 2012 or 2010 SP1 installed on your computer.</span></span> <span data-ttu-id="e8704-136">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express for Web。</span><span class="sxs-lookup"><span data-stu-id="e8704-136">If you don't have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express for Web.</span></span>

<span data-ttu-id="e8704-137">如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。</span><span class="sxs-lookup"><span data-stu-id="e8704-137">If you have Visual Studio 2010, make sure that [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) is installed.</span></span>

<a id="createproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="e8704-138">建立專案</span><span class="sxs-lookup"><span data-stu-id="e8704-138">Create the project</span></span>

1. <span data-ttu-id="e8704-139">從**檔案**功能表上，按一下**新專案**。</span><span class="sxs-lookup"><span data-stu-id="e8704-139">From the **File** menu click **New Project**.</span></span>
2. <span data-ttu-id="e8704-140">在**新專案**對話方塊方塊中，展開  **C#** 下**範本**選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="e8704-140">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="e8704-141">選取**ASP.NET 空 Web 應用程式**範本，將專案*SignalR.StockTicker*，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="e8704-141">Select the **ASP.NET Empty Web Application** template, name the project *SignalR.StockTicker*, and click **OK**.</span></span>

    ![新增專案對話方塊](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a><span data-ttu-id="e8704-143">將 SignalR NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="e8704-143">Add the SignalR NuGet Packages</span></span>

### <a name="add-the-signalr-and-jquery-nuget-packages"></a><span data-ttu-id="e8704-144">將 SignalR 和 JQuery NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="e8704-144">Add the SignalR and JQuery NuGet Packages</span></span>

<span data-ttu-id="e8704-145">您可以將加入專案的 SignalR 功能安裝 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e8704-145">You can add SignalR functionality to a project by installing a NuGet package.</span></span>

1. <span data-ttu-id="e8704-146">按一下**工具 |程式庫套件管理員 |Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="e8704-146">Click **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="e8704-147">封裝管理員中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="e8704-147">Enter the following command in the package manager.</span></span>

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    <span data-ttu-id="e8704-148">SignalR 套件會安裝其他的 NuGet 套件的數字，做為相依性。</span><span class="sxs-lookup"><span data-stu-id="e8704-148">The SignalR package installs a number of other NuGet packages as dependencies.</span></span> <span data-ttu-id="e8704-149">安裝完成時可能會有所有 SignalR 使用 ASP.NET 應用程式所需的伺服器和用戶端元件。</span><span class="sxs-lookup"><span data-stu-id="e8704-149">When the installation is finished you have all of the server and client components required to use SignalR in an ASP.NET application.</span></span>

<a id="server"></a>

## <a name="set-up-the-server-code"></a><span data-ttu-id="e8704-150">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="e8704-150">Set up the server code</span></span>

<span data-ttu-id="e8704-151">本節中您設定在伺服器執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e8704-151">In this section you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="e8704-152">建立庫存類別</span><span class="sxs-lookup"><span data-stu-id="e8704-152">Create the Stock class</span></span>

<span data-ttu-id="e8704-153">您開始建立您要用以儲存及傳輸內建的相關資訊的內建模型類別。</span><span class="sxs-lookup"><span data-stu-id="e8704-153">You begin by creating the Stock model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="e8704-154">在專案資料夾中建立新的類別檔案，其命名*Stock.cs*，然後將範本程式碼取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e8704-154">Create a new class file in the project folder, name it *Stock.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    <span data-ttu-id="e8704-155">您將設定時建立的股票的兩個屬性為符號 (例如，microsoft MSFT) 和價格。</span><span class="sxs-lookup"><span data-stu-id="e8704-155">The two properties that you'll set when you create stocks are the Symbol (for example, MSFT for Microsoft) and the Price.</span></span> <span data-ttu-id="e8704-156">其他屬性取決於您如何及何時將價格。</span><span class="sxs-lookup"><span data-stu-id="e8704-156">The other properties depend on how and when you set Price.</span></span> <span data-ttu-id="e8704-157">第一次設定價格的值取得傳播到 DayOpen。</span><span class="sxs-lookup"><span data-stu-id="e8704-157">The first time you set Price, the value gets propagated to DayOpen.</span></span> <span data-ttu-id="e8704-158">後續的階段，當您設定的價格變更，且會計算 PercentChange 屬性值會根據價格和 DayOpen 之間的差異。</span><span class="sxs-lookup"><span data-stu-id="e8704-158">Subsequent times when you set Price, the Change and PercentChange property values are calculated based on the difference between Price and DayOpen.</span></span>

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a><span data-ttu-id="e8704-159">建立 StockTicker 和 StockTickerHub 類別</span><span class="sxs-lookup"><span data-stu-id="e8704-159">Create the StockTicker and StockTickerHub classes</span></span>

<span data-ttu-id="e8704-160">您將使用 SignalR 中樞應用程式開發介面來處理伺服器到用戶端互動。</span><span class="sxs-lookup"><span data-stu-id="e8704-160">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="e8704-161">StockTickerHub 衍生的類別，從 SignalR 中樞類別將會接收來自用戶端連線和方法呼叫來處理。</span><span class="sxs-lookup"><span data-stu-id="e8704-161">A StockTickerHub class that derives from the SignalR Hub class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="e8704-162">您也需要維護的內建資料及執行以定期觸發獨立用戶端連線的價格更新計時器物件。</span><span class="sxs-lookup"><span data-stu-id="e8704-162">You also need to maintain stock data and run a Timer object to periodically trigger price updates, independently of client connections.</span></span> <span data-ttu-id="e8704-163">因為暫時性中樞執行個體，您無法暫停在中樞類別中，這些函式。</span><span class="sxs-lookup"><span data-stu-id="e8704-163">You can't put these functions in a Hub class, because Hub instances are transient.</span></span> <span data-ttu-id="e8704-164">在中樞內，例如連線和伺服器從用戶端呼叫每個作業會建立的 Hub 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e8704-164">A Hub class instance is created for each operation on the hub, such as connections and calls from the client to the server.</span></span> <span data-ttu-id="e8704-165">因此，會將內建的資料保存、 更新價格，接著在廣播價格更新的機制，都必須在個別的類別，您會命名 StockTicker 執行。</span><span class="sxs-lookup"><span data-stu-id="e8704-165">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class, which you'll name StockTicker.</span></span>

![從 StockTicker 廣播](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

<span data-ttu-id="e8704-167">您只想要執行的伺服器上，因此您必須設定每個 StockTickerHub 執行個體從單一 StockTicker 執行個體參考的 StockTicker 類別的一個執行個體。</span><span class="sxs-lookup"><span data-stu-id="e8704-167">You only want one instance of the StockTicker class to run on the server, so you'll need to set up a reference from each StockTickerHub instance to the singleton StockTicker instance.</span></span> <span data-ttu-id="e8704-168">StockTicker 類別必須廣播至用戶端，因為它已內建的資料，並觸發更新，但 StockTicker 不是中樞類別。</span><span class="sxs-lookup"><span data-stu-id="e8704-168">The StockTicker class has to be able to broadcast to clients because it has the stock data and triggers updates, but StockTicker is not a Hub class.</span></span> <span data-ttu-id="e8704-169">因此，StockTicker 類別必須取得 SignalR 中樞的連線內容物件的參考。</span><span class="sxs-lookup"><span data-stu-id="e8704-169">Therefore, the StockTicker class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="e8704-170">它然後可以使用廣播至用戶端的 SignalR 連線的內容物件。</span><span class="sxs-lookup"><span data-stu-id="e8704-170">It can then use the SignalR connection context object to broadcast to clients.</span></span>

1. <span data-ttu-id="e8704-171">在**方案總管 中**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="e8704-171">In **Solution Explorer**, right-click the project and click **Add New Item**.</span></span>
2. <span data-ttu-id="e8704-172">如果您有 Visual Studio 2012 [ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=279941)，按一下  **Web**下**Visual C#** 選取**SignalR 中樞類別**項目範本。</span><span class="sxs-lookup"><span data-stu-id="e8704-172">If you have Visual Studio 2012 with the [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=279941), click **Web** under **Visual C#** and select the **SignalR Hub Class** item template.</span></span> <span data-ttu-id="e8704-173">否則，請選取**類別**範本。</span><span class="sxs-lookup"><span data-stu-id="e8704-173">Otherwise, select the **Class** template.</span></span>
3. <span data-ttu-id="e8704-174">將新類別*StockTickerHub.cs*，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e8704-174">Name the new class *StockTickerHub.cs*, and then click **Add**.</span></span>

    ![新增 StockTickerHub.cs](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. <span data-ttu-id="e8704-176">取代為下列程式碼中的範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e8704-176">Replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    <span data-ttu-id="e8704-177">[中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別用來定義用戶端可以呼叫在伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-177">The [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class is used to define methods the clients can call on the server.</span></span> <span data-ttu-id="e8704-178">您要定義一種方法： `GetAllStocks()`。</span><span class="sxs-lookup"><span data-stu-id="e8704-178">You are defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="e8704-179">用戶端一開始會連接到伺服器，它會呼叫這個方法，取得所有其目前的價格股票的清單。</span><span class="sxs-lookup"><span data-stu-id="e8704-179">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="e8704-180">方法可以以同步方式執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。</span><span class="sxs-lookup"><span data-stu-id="e8704-180">The method can execute synchronously and return `IEnumerable<Stock>` because it is returning data from memory.</span></span> <span data-ttu-id="e8704-181">如果方法具有以取得資料所做的事，牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，若要啟用非同步處理。</span><span class="sxs-lookup"><span data-stu-id="e8704-181">If the method had to get the data by doing something that would involve waiting, such as a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="e8704-182">如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南-伺服器-時以非同步方式執行](index.md)。</span><span class="sxs-lookup"><span data-stu-id="e8704-182">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](index.md).</span></span>

    <span data-ttu-id="e8704-183">HubName 屬性指定中樞的用戶端上的 JavaScript 程式碼中的參考方式。</span><span class="sxs-lookup"><span data-stu-id="e8704-183">The HubName attribute specifies how the Hub will be referenced in JavaScript code on the client.</span></span> <span data-ttu-id="e8704-184">如果您不使用這個屬性在用戶端上的預設名稱是依照 camel 命名法的大小寫慣例版本的類別名稱，因此在此情況下會 stockTickerHub。</span><span class="sxs-lookup"><span data-stu-id="e8704-184">The default name on the client if you don't use this attribute is a camel-cased version of the class name, which in this case would be stockTickerHub.</span></span>

    <span data-ttu-id="e8704-185">當您建立 StockTicker 類別時，您會看到更新版本，該類別的單一執行個體被建立靜態執行個體屬性中。</span><span class="sxs-lookup"><span data-stu-id="e8704-185">As you'll see later when you create the StockTicker class, a singleton instance of that class is created in its static Instance property.</span></span> <span data-ttu-id="e8704-186">不論多少用戶端連接或中斷連線、 的記憶體中保留 StockTicker 單一執行個體，該執行個體供 GetAllStocks 方法用來傳回目前的股票資訊。</span><span class="sxs-lookup"><span data-stu-id="e8704-186">That singleton instance of StockTicker remains in memory no matter how many clients connect or disconnect, and that instance is what the GetAllStocks method uses to return current stock information.</span></span>
5. <span data-ttu-id="e8704-187">在專案資料夾中建立新的類別檔案，其命名*StockTicker.cs*，然後將範本程式碼取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e8704-187">Create a new class file in the project folder, name it *StockTicker.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    <span data-ttu-id="e8704-188">由於多個執行緒會執行相同的執行個體的 StockTicker 程式碼，StockTicker 類別必須是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="e8704-188">Since multiple threads will be running the same instance of StockTicker code, the StockTicker class has to be threadsafe.</span></span>

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="e8704-189">將單一執行個體儲存在靜態欄位</span><span class="sxs-lookup"><span data-stu-id="e8704-189">Storing the singleton instance in a static field</span></span>

    <span data-ttu-id="e8704-190">程式碼會初始化靜態\_支援類別，而這樣的執行個體的執行個體內容的執行個體欄位是唯一的執行個體，可以建立的類別，因為標示為私用建構函式。</span><span class="sxs-lookup"><span data-stu-id="e8704-190">The code initializes the static \_instance field that backs the Instance property with an instance of the class, and this is the only instance of the class that can be created, because the constructor is marked as private.</span></span> <span data-ttu-id="e8704-191">[延遲初始設定](https://msdn.microsoft.com/library/dd997286.aspx)用於\_不基於效能考量，但若要確定執行個體建立 threadsafe 的執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="e8704-191">[Lazy initialization](https://msdn.microsoft.com/library/dd997286.aspx) is used for the \_instance field, not for performance reasons but to ensure that the instance creation is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    <span data-ttu-id="e8704-192">每次用戶端連接到伺服器，執行不同的執行緒中 StockTickerHub 類別的新執行個體取得 StockTicker 單一執行個體從 StockTicker.Instance 靜態屬性，如同稍早在 StockTickerHub 類別中。</span><span class="sxs-lookup"><span data-stu-id="e8704-192">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the StockTicker.Instance static property, as you saw earlier in the StockTickerHub class.</span></span>

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="e8704-193">在 ConcurrentDictionary 中儲存庫存資料</span><span class="sxs-lookup"><span data-stu-id="e8704-193">Storing stock data in a ConcurrentDictionary</span></span>

    <span data-ttu-id="e8704-194">建構函式初始化\_股票集合會以一些內建資料範例和 GetAllStocks 傳回股票。</span><span class="sxs-lookup"><span data-stu-id="e8704-194">The constructor initializes the \_stocks collection with some sample stock data, and GetAllStocks returns the stocks.</span></span> <span data-ttu-id="e8704-195">如稍早所見，StockTickerHub.GetAllStocks 也就是在用戶端可以呼叫的中樞類別中的伺服器方法接著就會傳回這個集合的股票。</span><span class="sxs-lookup"><span data-stu-id="e8704-195">As you saw earlier, this collection of stocks is in turn returned by StockTickerHub.GetAllStocks which is a server method in the Hub class that clients can call.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    <span data-ttu-id="e8704-196">股票集合定義為[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的類型。</span><span class="sxs-lookup"><span data-stu-id="e8704-196">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="e8704-197">或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。</span><span class="sxs-lookup"><span data-stu-id="e8704-197">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

    <span data-ttu-id="e8704-198">此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料並處置 StockTicker 執行個體時，遺失的資料。</span><span class="sxs-lookup"><span data-stu-id="e8704-198">For this sample application, it's OK to store application data in memory and to lose the data when the StockTicker instance is disposed.</span></span> <span data-ttu-id="e8704-199">在實際應用您可能會使用後端資料存放區，例如資料庫。</span><span class="sxs-lookup"><span data-stu-id="e8704-199">In a real application you would work with a back-end data store such as a database.</span></span>

    ### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="e8704-200">定期更新股票價格</span><span class="sxs-lookup"><span data-stu-id="e8704-200">Periodically updating stock prices</span></span>

    <span data-ttu-id="e8704-201">建構函式會啟動計時器物件定期呼叫更新股價隨機為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-201">The constructor starts up a Timer object that periodically calls methods that update stock prices on a random basis.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    <span data-ttu-id="e8704-202">UpdateStockPrices 計時器，state 參數中的 null 會傳入所呼叫。</span><span class="sxs-lookup"><span data-stu-id="e8704-202">UpdateStockPrices is called by the Timer, which passes in null in the state parameter.</span></span> <span data-ttu-id="e8704-203">更新之前的價格，採用的鎖定\_updateStockPricesLock 物件。</span><span class="sxs-lookup"><span data-stu-id="e8704-203">Before updating prices, a lock is taken on the \_updateStockPricesLock object.</span></span> <span data-ttu-id="e8704-204">如果另一個執行緒已經正在更新價格，然後在清單中的每種股票呼叫 TryUpdateStockPrice，會檢查程式碼。</span><span class="sxs-lookup"><span data-stu-id="e8704-204">The code checks if another thread is already updating prices, and then it calls TryUpdateStockPrice on each stock in the list.</span></span> <span data-ttu-id="e8704-205">TryUpdateStockPrice 方法會決定是否要變更的股價，以及有多少變更它。</span><span class="sxs-lookup"><span data-stu-id="e8704-205">The TryUpdateStockPrice method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="e8704-206">如果變更的股價，BroadcastStockPrice 稱為股票價格變更廣播給所有連接的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e8704-206">If the stock price is changed, BroadcastStockPrice is called to broadcast the stock price change to all connected clients.</span></span>

    <span data-ttu-id="e8704-207">\_UpdatingStockPrices 旗標標示為[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)以確保其存取權是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="e8704-207">The \_updatingStockPrices flag is marked as [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to ensure that access to it is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    <span data-ttu-id="e8704-208">在實際的應用程式，TryUpdateStockPrice 方法會呼叫 web 服務以查閱價格;在此程式碼中，它會使用隨機號碼產生器隨機進行變更。</span><span class="sxs-lookup"><span data-stu-id="e8704-208">In a real application, the TryUpdateStockPrice method would call a web service to look up the price; in this code it uses a random number generator to make changes randomly.</span></span>

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="e8704-209">取得 SignalR 內容，以便 StockTicker 類別可以廣播至用戶端</span><span class="sxs-lookup"><span data-stu-id="e8704-209">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

    <span data-ttu-id="e8704-210">因為價格變更以下來自 StockTicker 物件中，這是需要 updateStockPrice 方法呼叫所有已連線的用戶端上的物件。</span><span class="sxs-lookup"><span data-stu-id="e8704-210">Because the price changes originate here in the StockTicker object, this is the object that needs to call an updateStockPrice method on all connected clients.</span></span> <span data-ttu-id="e8704-211">中樞類別中您的應用程式開發介面呼叫用戶端方法，但不是衍生自中樞類別 StockTicker，而且沒有任何中樞物件的參考。</span><span class="sxs-lookup"><span data-stu-id="e8704-211">In a Hub class you have an API for calling client methods, but StockTicker does not derive from the Hub class and does not have a reference to any Hub object.</span></span> <span data-ttu-id="e8704-212">因此，若要廣播至連線的用戶端，StockTicker 類別必須 StockTickerHub 類別取得 SignalR 內容執行個體，然後使用它來呼叫用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-212">Therefore, in order to broadcast to connected clients, the StockTicker class has to get the SignalR context instance for the StockTickerHub class and use that to call methods on clients.</span></span>

    <span data-ttu-id="e8704-213">建立單一類別執行個體，建構函式，將該參考時，程式碼會取得 SignalR 內容的參考，建構函式將它放在用戶端屬性。</span><span class="sxs-lookup"><span data-stu-id="e8704-213">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the Clients property.</span></span>

    <span data-ttu-id="e8704-214">有兩個您想要一次取得內容的原因： 取得內容是昂貴的作業，並取得它一次可確保預期的順序傳送至用戶端的訊息會保留。</span><span class="sxs-lookup"><span data-stu-id="e8704-214">There are two reasons why you want to get the context just once: getting the context is an expensive operation, and getting it once ensures that the intended order of messages sent to clients is preserved.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    <span data-ttu-id="e8704-215">取得內容的用戶端屬性，並將它放在 StockTickerClient 屬性可讓您撰寫程式碼來呼叫用戶端方法看起來相同一樣中樞類別中。</span><span class="sxs-lookup"><span data-stu-id="e8704-215">Getting the Clients property of the context and putting it in the StockTickerClient property lets you write code to call client methods that looks the same as it would in a Hub class.</span></span> <span data-ttu-id="e8704-216">比方說，若要廣播到所有用戶端，您可以撰寫 Clients.All.updateStockPrice(stock)。</span><span class="sxs-lookup"><span data-stu-id="e8704-216">For instance, to broadcast to all clients you can write Clients.All.updateStockPrice(stock).</span></span>

    <span data-ttu-id="e8704-217">您要呼叫中 BroadcastStockPrice updateStockPrice 方法不存在。稍後當您撰寫的用戶端執行的程式碼時，會將其新增。</span><span class="sxs-lookup"><span data-stu-id="e8704-217">The updateStockPrice method that you are calling in BroadcastStockPrice doesn't exist yet; you'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="e8704-218">您可以參考以下 updateStockPrice 因為 Clients.All 是動態，這表示會在執行階段評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="e8704-218">You can refer to updateStockPrice here because Clients.All is dynamic, which means the expression will be evaluated at runtime.</span></span> <span data-ttu-id="e8704-219">SignalR 呼叫此方法執行時，會傳送的方法名稱和參數值給用戶端，以及如果用戶端具有名為 updateStockPrice 的方法，該方法會呼叫和參數值會傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="e8704-219">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named updateStockPrice, that method will be called and the parameter value will be passed to it.</span></span>

    <span data-ttu-id="e8704-220">Clients.All 表示傳送給所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="e8704-220">Clients.All means send to all clients.</span></span> <span data-ttu-id="e8704-221">SignalR 提供其他選項來指定哪些用戶端或用戶端傳送到群組。</span><span class="sxs-lookup"><span data-stu-id="e8704-221">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="e8704-222">如需詳細資訊，請參閱[HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="e8704-222">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="e8704-223">註冊的 SignalR 的路由</span><span class="sxs-lookup"><span data-stu-id="e8704-223">Register the SignalR route</span></span>

<span data-ttu-id="e8704-224">伺服器必須知道要攔截並導向 SignalR 的 URL。</span><span class="sxs-lookup"><span data-stu-id="e8704-224">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="e8704-225">若要如何將一些程式碼來*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="e8704-225">To do that you'll add some code to the *Global.asax* file.</span></span>

1. <span data-ttu-id="e8704-226">在**方案總管 中**，以滑鼠右鍵按一下專案，然後按一下**加入新項目**。</span><span class="sxs-lookup"><span data-stu-id="e8704-226">In **Solution Explorer**, right-click the project, and then click **Add New Item**.</span></span>
2. <span data-ttu-id="e8704-227">選取**全域應用程式類別**項目範本，然後再按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="e8704-227">Select the **Global Application Class** item template, and then click **Add**.</span></span>

    ![新增 global.asax](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. <span data-ttu-id="e8704-229">將 SignalR 的路由登錄程式碼加入至應用程式\_Start 方法：</span><span class="sxs-lookup"><span data-stu-id="e8704-229">Add the SignalR route registration code to the Application\_Start method:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    <span data-ttu-id="e8704-230">根據預設，所有 SignalR 流量的基底 URL 是:"/ signalr 」，和 「 / signalr/中心 」 用來擷取動態產生的 JavaScript 檔案，定義您在您的應用程式的所有主機的 proxy。</span><span class="sxs-lookup"><span data-stu-id="e8704-230">By default, the base URL for all SignalR traffic is "/signalr", and "/signalr/hubs" is used to retrieve a dynamically generated JavaScript file that defines proxies for all the Hubs you have in your application.</span></span> <span data-ttu-id="e8704-231">MapHubs 方法包含可讓您指定不同的基底 URL 和 SignalR 的特定選項的執行個體中的多載[HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="e8704-231">The MapHubs method includes overloads that let you specify a different base URL and certain SignalR options in an instance of the [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) class.</span></span>
4. <span data-ttu-id="e8704-232">加入 using 陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="e8704-232">Add a using statement at the top of the file:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. <span data-ttu-id="e8704-233">儲存並關閉*Global.asax*檔案，然後建置專案。</span><span class="sxs-lookup"><span data-stu-id="e8704-233">Save and close the *Global.asax* file, and build the project.</span></span>

<span data-ttu-id="e8704-234">您現在已完成設定伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="e8704-234">You have now completed setting up the server code.</span></span> <span data-ttu-id="e8704-235">下一節中您會設定用戶端。</span><span class="sxs-lookup"><span data-stu-id="e8704-235">In the next section you'll set up the client.</span></span>

<a id="client"></a>

## <a name="set-up-the-client-code"></a><span data-ttu-id="e8704-236">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="e8704-236">Set up the client code</span></span>

1. <span data-ttu-id="e8704-237">新的 HTML 檔案的資料夾中建立專案，並將其命名*StockTicker.html*。</span><span class="sxs-lookup"><span data-stu-id="e8704-237">Create a new HTML file in the project folder, and name it *StockTicker.html*.</span></span>
2. <span data-ttu-id="e8704-238">取代為下列程式碼中的範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e8704-238">Replace the template code with the following code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    <span data-ttu-id="e8704-239">HTML 5 個資料行、 標頭資料列與資料列跨越所有 5 個資料行的單一資料格與建立資料表。</span><span class="sxs-lookup"><span data-stu-id="e8704-239">The HTML creates a table with 5 columns, a header row, and a data row with a single cell that spans all 5 columns.</span></span> <span data-ttu-id="e8704-240">資料列會顯示 「 載入...」 和，才會顯示在應用程式啟動時，立刻顯示。</span><span class="sxs-lookup"><span data-stu-id="e8704-240">The data row displays "loading..." and will only be shown momentarily when the application starts.</span></span> <span data-ttu-id="e8704-241">JavaScript 程式碼將會移除該資料列，並從伺服器擷取的內建資料進行資料列中加入。</span><span class="sxs-lookup"><span data-stu-id="e8704-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="e8704-242">指令碼標記指定 jQuery 指令碼檔案、 SignalR core 指令碼檔案、 SignalR proxy 指令碼檔案及更新版本，您將建立 StockTicker 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="e8704-242">The script tags specify the jQuery script file, the SignalR core script file, the SignalR proxies script file, and a StockTicker script file that you'll create later.</span></span> <span data-ttu-id="e8704-243">SignalR proxy 指令碼檔案，指定 「 signalr/中樞 」 URL，以動態方式產生，並在中樞類別、 方法的 proxy 方法定義在此情況下 StockTickerHub.GetAllStocks。</span><span class="sxs-lookup"><span data-stu-id="e8704-243">The SignalR proxies script file, which specifies the "/signalr/hubs" URL, is dynamically generated and defines proxy methods for the methods on the Hub class, in this case for StockTickerHub.GetAllStocks.</span></span> <span data-ttu-id="e8704-244">如果您想要的話，您可以產生這個 JavaScript 檔案手動使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)和停用動態檔案建立 MapHubs 方法呼叫中的。</span><span class="sxs-lookup"><span data-stu-id="e8704-244">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) and disable dynamic file creation in the MapHubs method call.</span></span>
3. > [!IMPORTANT]
   > <span data-ttu-id="e8704-245">請確定 JavaScript 檔案參考中*StockTicker.html*正確無誤。</span><span class="sxs-lookup"><span data-stu-id="e8704-245">Make sure that the JavaScript file references in *StockTicker.html* are correct.</span></span> <span data-ttu-id="e8704-246">也就是確定您的指令碼標記 (在範例 1.8.2) 中的 jQuery 版本是在您的專案中的 jQuery 版本相同*指令碼*資料夾，並確定指令碼標記中的 SignalR 版本是 SignalR 相同在您的專案版本*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e8704-246">That is, make sure that the jQuery version in your script tag (1.8.2 in the example) is the same as the jQuery version in your project's *Scripts* folder, and make sure that the SignalR version in your script tag is the same as the SignalR version in your project's *Scripts* folder.</span></span> <span data-ttu-id="e8704-247">如有必要，請變更指令碼標記中的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="e8704-247">Change the file names in the script tags if necessary.</span></span>
4. <span data-ttu-id="e8704-248">在**方案總管] 中**，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 [**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="e8704-248">In **Solution Explorer**, right-click *StockTicker.html*, and then click **Set as Start Page**.</span></span>
5. <span data-ttu-id="e8704-249">在專案資料夾中建立新的 JavaScript 檔案並將其命名*StockTicker.js*...</span><span class="sxs-lookup"><span data-stu-id="e8704-249">Create a new JavaScript file in the project folder and name it *StockTicker.js*..</span></span>
6. <span data-ttu-id="e8704-250">取代為下列程式碼中的範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e8704-250">Replace the template code with the following code:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    <span data-ttu-id="e8704-251">$.connection 指 SignalR proxy。</span><span class="sxs-lookup"><span data-stu-id="e8704-251">$.connection refers to the SignalR proxies.</span></span> <span data-ttu-id="e8704-252">程式碼 StockTickerHub 類別取得 proxy 的參考，並將它放在股票行情指示器變數中。</span><span class="sxs-lookup"><span data-stu-id="e8704-252">The code gets a reference to the proxy for the StockTickerHub class and puts it in the ticker variable.</span></span> <span data-ttu-id="e8704-253">Proxy 名稱是由 [HubName] 屬性已設定的名稱：</span><span class="sxs-lookup"><span data-stu-id="e8704-253">The proxy name is the name that was set by the [HubName] attribute:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    <span data-ttu-id="e8704-254">定義所有變數和函式後，檔案中的程式碼的最後一行會初始化 SignalR 連線，藉由呼叫 SignalR 啟始函式。</span><span class="sxs-lookup"><span data-stu-id="e8704-254">After all the variables and functions are defined, the last line of code in the file initializes the SignalR connection by calling the SignalR start function.</span></span> <span data-ttu-id="e8704-255">啟始函式會以非同步方式執行，並傳回[jQuery 延期物件](http://api.jquery.com/category/deferred-object/)，這表示您可以呼叫完成的函式，以指定要在完成非同步作業時呼叫的函式...</span><span class="sxs-lookup"><span data-stu-id="e8704-255">The start function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means you can call the done function to specify the function to call when the asynchronous operation is completed..</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    <span data-ttu-id="e8704-256">Init 函式呼叫 getAllStocks 函式在伺服器上，並使用伺服器來更新資料表內建傳回的資訊。</span><span class="sxs-lookup"><span data-stu-id="e8704-256">The init function calls the getAllStocks function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="e8704-257">請注意，根據預設，需要使用 camel 命名法的大小寫的用戶端上雖然方法名稱是依照 pascal 命名法的大小寫慣例在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e8704-257">Notice that by default, you have to use camel casing on the client although the method name is pascal-cased on the server.</span></span> <span data-ttu-id="e8704-258">依照 camel 命名法的大小寫規則只適用於不是物件的方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-258">The camel-casing rule only applies to methods, not objects.</span></span> <span data-ttu-id="e8704-259">例如，您參考內建。符號 」 與 「 存貨。價格、 不 stock.symbol 或 stock.price。</span><span class="sxs-lookup"><span data-stu-id="e8704-259">For example, you refer to stock.Symbol and stock.Price, not stock.symbol or stock.price.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    <span data-ttu-id="e8704-260">如果您想要在用戶端，使用 pascal 大小寫，或如果您想要使用完全不同的方法名稱，您無法裝飾中樞方法 HubMethodName 屬性具有相同的方式會裝飾中樞類別具有 HubName 屬性。</span><span class="sxs-lookup"><span data-stu-id="e8704-260">If you wanted to use pascal casing on the client, or if you wanted to use a completely different method name, you could decorate the Hub method with the HubMethodName attribute the same way you decorated the Hub class itself with the HubName attribute.</span></span>

    <span data-ttu-id="e8704-261">Init 方法中，在 HTML 資料表資料列會建立每個內建物件從伺服器收到的呼叫 formatStock 格式內容的內建物件，並藉由呼叫取代 (定義在頂端*StockTicker.js*) 來取代內建物件屬性值的預留位置 rowTemplate 變數中的。</span><span class="sxs-lookup"><span data-stu-id="e8704-261">In the init method, HTML for a table row is created for each stock object received from the server by calling formatStock to format properties of the stock object, and then by calling supplant (which is defined at the top of *StockTicker.js*) to replace placeholders in the rowTemplate variable with the stock object property values.</span></span> <span data-ttu-id="e8704-262">產生的 HTML 則會附加至內建的資料表中。</span><span class="sxs-lookup"><span data-stu-id="e8704-262">The resulting HTML is then appended to the stock table.</span></span>

    <span data-ttu-id="e8704-263">您可以在傳遞做為非同步啟始函式完成之後執行的回呼函式呼叫 init。</span><span class="sxs-lookup"><span data-stu-id="e8704-263">You call init by passing it in as a callback function that executes after the asynchronous start function completes.</span></span> <span data-ttu-id="e8704-264">如果您呼叫初始化為個別的 JavaScript 陳述式之後呼叫開始時，函式會失敗，因為它會立即執行而不需等待啟始函式，以完成建立連線。</span><span class="sxs-lookup"><span data-stu-id="e8704-264">If you called init as a separate JavaScript statement after calling start, the function would fail because it would execute immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="e8704-265">在此情況下，init 函式會嘗試建立伺服器連接之前呼叫 getAllStocks 函式。</span><span class="sxs-lookup"><span data-stu-id="e8704-265">In that case, the init function would try to call the getAllStocks function before the server connection is established.</span></span>

    <span data-ttu-id="e8704-266">當伺服器變更股價時，它會呼叫 updateStockPrice 連線的用戶端上。</span><span class="sxs-lookup"><span data-stu-id="e8704-266">When the server changes a stock's price, it calls the updateStockPrice on connected clients.</span></span> <span data-ttu-id="e8704-267">此函式加入 stockTicker proxy 的用戶端屬性，以便將它提供給呼叫從伺服器。</span><span class="sxs-lookup"><span data-stu-id="e8704-267">The function is added to the client property of the stockTicker proxy in order to make it available to calls from the server.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    <span data-ttu-id="e8704-268">UpdateStockPrice 函式會將收到來自伺服器的資料表資料列到 init 函式的相同方式將內建物件格式化。</span><span class="sxs-lookup"><span data-stu-id="e8704-268">The updateStockPrice function formats a stock object received from the server into a table row the same way as in the init function.</span></span> <span data-ttu-id="e8704-269">不過，而不是將資料列附加至資料表，資料表中尋找股票的目前資料列，以新的取代該資料列。</span><span class="sxs-lookup"><span data-stu-id="e8704-269">However, instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

<a id="test"></a>

## <a name="test-the-application"></a><span data-ttu-id="e8704-270">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e8704-270">Test the application</span></span>

1. <span data-ttu-id="e8704-271">按 F5 以偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e8704-271">Press F5 to run the application in debug mode.</span></span>

    <span data-ttu-id="e8704-272">內建資料表一開始會顯示 「 載入...」 列，然後顯示初始的內建資料，在短暫延遲之後，再股票價格開始變更。</span><span class="sxs-lookup"><span data-stu-id="e8704-272">The stock table initially displays the "loading..." line, then after a short delay the initial stock data is displayed, and then the stock prices start to change.</span></span>

    ![正在載入](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![初始的內建資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![從伺服器接收變更內建的資料表](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. <span data-ttu-id="e8704-276">從瀏覽器網址列複製 URL，並將它貼到一或多個新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="e8704-276">Copy the URL from the browser address bar and paste it into one or more new browser window(s).</span></span>

    <span data-ttu-id="e8704-277">初始的內建顯示等同於第一個瀏覽器，並同時進行變更。</span><span class="sxs-lookup"><span data-stu-id="e8704-277">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>
3. <span data-ttu-id="e8704-278">關閉所有瀏覽器並開啟新的瀏覽器，然後移至相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="e8704-278">Close all browsers and open a new browser, then go to the same URL.</span></span>

    <span data-ttu-id="e8704-279">在伺服器中執行，因此則內建的資料表會顯示股票仍繼續變更仍持續 StockTicker 單一物件。</span><span class="sxs-lookup"><span data-stu-id="e8704-279">The StockTicker singleton object has continued to run in the server, so the stock table display shows that the stocks have continued to change.</span></span> <span data-ttu-id="e8704-280">（您未看到變更數據的初始資料表有零）。</span><span class="sxs-lookup"><span data-stu-id="e8704-280">(You don't see the initial table with zero change figures.)</span></span>
4. <span data-ttu-id="e8704-281">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e8704-281">Close the browser.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="e8704-282">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="e8704-282">Enable logging</span></span>

<span data-ttu-id="e8704-283">SignalR 有內建的記錄功能，您可以啟用用戶端上，以協助疑難排解。</span><span class="sxs-lookup"><span data-stu-id="e8704-283">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="e8704-284">本節中您要啟用記錄，並查看範例，說明如何記錄告訴您正在使用 SignalR 的下列傳輸方法：</span><span class="sxs-lookup"><span data-stu-id="e8704-284">In this section you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

- <span data-ttu-id="e8704-285">[WebSockets](http://en.wikipedia.org/wiki/WebSocket)、 IIS 8 及目前的瀏覽器所支援。</span><span class="sxs-lookup"><span data-stu-id="e8704-285">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>
- <span data-ttu-id="e8704-286">[伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。</span><span class="sxs-lookup"><span data-stu-id="e8704-286">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>
- <span data-ttu-id="e8704-287">[永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet Explorer 受支援。</span><span class="sxs-lookup"><span data-stu-id="e8704-287">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>
- <span data-ttu-id="e8704-288">[Ajax 長期輪詢](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e8704-288">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="e8704-289">任何指定的連接，SignalR 選擇最佳伺服器和用戶端支援的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-289">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="e8704-290">開啟*StockTicker.js*並加入一行程式碼可以立即初始化檔案結尾處的連接的程式碼之前的記錄：</span><span class="sxs-lookup"><span data-stu-id="e8704-290">Open *StockTicker.js* and add a line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. <span data-ttu-id="e8704-291">按 F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="e8704-291">Press F5 to run the project.</span></span>
3. <span data-ttu-id="e8704-292">開啟瀏覽器的開發人員工具視窗，然後選取主控台，以查看記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e8704-292">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="e8704-293">您可能必須重新整理頁面，請參閱記錄檔的 Signalr 交涉新連線的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-293">You might have to refresh the page to see the logs of Signalr negotiating the transport method for a new connection.</span></span>

    <span data-ttu-id="e8704-294">如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，傳輸方法是 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="e8704-294">If you are running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![IE 10 IIS 8 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    <span data-ttu-id="e8704-296">如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，傳輸方法是 iframe。</span><span class="sxs-lookup"><span data-stu-id="e8704-296">If you are running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is iframe.</span></span>

    ![IE 10 個主控台中，IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    <span data-ttu-id="e8704-298">在 Firefox 安裝 firebug 這類增益集以取得主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="e8704-298">In Firefox, install the Firebug add-in to get a Console window.</span></span> <span data-ttu-id="e8704-299">如果您在 Windows 8 (IIS 8) 執行 Firefox 19，傳輸方法是 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="e8704-299">If you are running Firefox 19 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    <span data-ttu-id="e8704-301">如果您在 Windows 7 (IIS 7.5) 上執行 Firefox 19，伺服器傳送的事件所傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-301">If you are running Firefox 19 on Windows 7 (IIS 7.5), the transport method is server-sent events.</span></span>

    ![Firefox 19 IIS 7.5 主控台](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a><span data-ttu-id="e8704-303">安裝並檢閱完整 StockTicker 範例</span><span class="sxs-lookup"><span data-stu-id="e8704-303">Install and review the full StockTicker sample</span></span>

<span data-ttu-id="e8704-304">StockTicker 應用程式所安裝[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件會包含更多的功能，比您剛從頭建立簡化版本。</span><span class="sxs-lookup"><span data-stu-id="e8704-304">The StockTicker application that is installed by the [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package includes more features than the simplified version that you just created from scratch.</span></span> <span data-ttu-id="e8704-305">在本章節的教學課程中，您可以安裝 NuGet 套件，並檢閱的新功能以及實作它們的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e8704-305">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="e8704-306">安裝 SignalR.Sample NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="e8704-306">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="e8704-307">在**方案總管 中**，以滑鼠右鍵按一下專案，然後按一下**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="e8704-307">In **Solution Explorer**, right-click the project and click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="e8704-308">在**管理 NuGet 封裝**對話方塊中，按一下 **線上**，輸入*SignalR.Sample*中**線上搜尋**方塊，然後再按一下  **安裝**中**SignalR.Sample**封裝。</span><span class="sxs-lookup"><span data-stu-id="e8704-308">In the **Manage NuGet Packages** dialog box, click **Online**, enter *SignalR.Sample* in the **Search Online** box, and then click **Install** in the **SignalR.Sample** package.</span></span>

    ![安裝 SignalR.Sample 套件](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. <span data-ttu-id="e8704-310">在*Global.asax*檔案，標記為註解 RouteTable.Routes.MapHubs(); 行您稍早在應用程式中加入\_Start 方法。</span><span class="sxs-lookup"><span data-stu-id="e8704-310">In the *Global.asax* file, comment out the RouteTable.Routes.MapHubs(); line that you added earlier in the Application\_Start method.</span></span>

    <span data-ttu-id="e8704-311">中的程式碼*Global.asax* SignalR.Sample 封裝暫存器中的 SignalR 路由因為不再需要*應用程式\_Start/RegisterHubs.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="e8704-311">The code in *Global.asax* is no longer needed because the SignalR.Sample package registers the SignalR route in the *App\_Start/RegisterHubs.cs* file:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    <span data-ttu-id="e8704-312">組件屬性所參考的 WebActivator 類別併入 WebActivatorEx NuGet 封裝，它會安裝為 SignalR.Sample 封裝的相依性。</span><span class="sxs-lookup"><span data-stu-id="e8704-312">The WebActivator class that is referenced by the assembly attribute is included in the WebActivatorEx NuGet package, which is installed as a dependency of the SignalR.Sample package.</span></span>
4. <span data-ttu-id="e8704-313">在**方案總管 中**，依序展開*SignalR.Sample*安裝 SignalR.Sample 封裝所建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e8704-313">In **Solution Explorer**, expand the *SignalR.Sample* folder which was created by installing the SignalR.Sample package.</span></span>
5. <span data-ttu-id="e8704-314">在*SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="e8704-314">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then click **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8704-315">安裝 SignalR.Sample NuGet 封裝可能會變更您在的 jQuery 新版您*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e8704-315">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="e8704-316">新*StockTicker.html*檔案，此封裝會安裝在*SignalR.Sample*資料夾將會是與此封裝會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記的 jQuery 參照。</span><span class="sxs-lookup"><span data-stu-id="e8704-316">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="e8704-317">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="e8704-317">Run the application</span></span>

1. <span data-ttu-id="e8704-318">按 F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e8704-318">Press F5 to run the application.</span></span>

    <span data-ttu-id="e8704-319">方格中稍早所見，除了完整股票行情指示器應用程式會顯示水平捲動視窗，以顯示相同的內建資料。</span><span class="sxs-lookup"><span data-stu-id="e8704-319">In addition to the grid that you saw earlier, the full stock ticker application shows a horizontally scrolling window that displays the same stock data.</span></span> <span data-ttu-id="e8704-320">當您執行第一次應用程式時，「 市集 」 「 關閉 」，您看到靜態格線和不捲動的股票行情指示器視窗。</span><span class="sxs-lookup"><span data-stu-id="e8704-320">When you run the application for the first time, the "market" is "closed" and you see a static grid and a ticker window that isn't scrolling.</span></span>

    ![StockTicker 螢幕開始](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    <span data-ttu-id="e8704-322">當您按一下**開啟市場**、 **Live 股票行情即時看板**方塊開始的水平捲動，並在伺服器啟動定期廣播股票價格變更隨機的方式。</span><span class="sxs-lookup"><span data-stu-id="e8704-322">When you click **Open Market**, the **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span> <span data-ttu-id="e8704-323">每次股票價格變更，同時**Live 存貨資料表**格線和**Live 股票行情即時看板**方塊就會更新。</span><span class="sxs-lookup"><span data-stu-id="e8704-323">Each time a stock price changes, both the **Live Stock Table** grid and the **Live Stock Ticker** box are updated.</span></span> <span data-ttu-id="e8704-324">具有綠色背景，正股票價格變動時，會顯示股票和股票時變更為負數，會顯示具有紅色背景。</span><span class="sxs-lookup"><span data-stu-id="e8704-324">When a stock's price change is positive, the stock is shown with a green background, and when the change is negative, the stock is shown with a red background.</span></span>

    ![StockTicker 應用程式中，開啟 市場](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    <span data-ttu-id="e8704-326">**關閉市場**按鈕就會停止所做的變更，並停止股票行情指示器捲動和**重設**按鈕會重設所有內建的資料還原為初始狀態啟動價格變更之前。</span><span class="sxs-lookup"><span data-stu-id="e8704-326">The **Close Market** button stops the changes and stops the ticker scrolling, and the **Reset** button resets all stock data to the initial state before price changes started.</span></span> <span data-ttu-id="e8704-327">如果您開啟多個瀏覽器視窗，並移至相同的 URL，您會看到相同的資料，動態更新在每個瀏覽器中相同的時間。</span><span class="sxs-lookup"><span data-stu-id="e8704-327">If you open more browser windows and go to the same URL, you see the same data dynamically updated at the same time in each browser.</span></span> <span data-ttu-id="e8704-328">當您按一下其中一個按鈕時，所有瀏覽器會回應相同的方式，在相同的時間。</span><span class="sxs-lookup"><span data-stu-id="e8704-328">When you click one of the buttons, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="e8704-329">即時股票行情指示器顯示</span><span class="sxs-lookup"><span data-stu-id="e8704-329">Live Stock Ticker display</span></span>

<span data-ttu-id="e8704-330">**Live 股票行情即時看板**顯示是 div 項目的 CSS 樣式格式化成單一行中的未排序的清單。</span><span class="sxs-lookup"><span data-stu-id="e8704-330">The **Live Stock Ticker** display is an unordered list in a div element that is formatted into a single line by CSS styles.</span></span> <span data-ttu-id="e8704-331">股票行情指示器會初始化並更新資料表的相同方式來： 取代中的預留位置&lt;li&gt;範本字串，並以動態方式加入&lt;li&gt;項目&lt;ul&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="e8704-331">The ticker is initialized and updated the same way as the table: by replacing placeholders in a &lt;li&gt; template string and dynamically adding the &lt;li&gt; elements to the &lt;ul&gt; element.</span></span> <span data-ttu-id="e8704-332">使用 jQuery 動畫函式來改變的未排序的清單內 div.margin-left 執行捲動</span><span class="sxs-lookup"><span data-stu-id="e8704-332">The scrolling is performed by using the jQuery animate function to vary the margin-left of the unordered list within the div.</span></span>

<span data-ttu-id="e8704-333">股票行情即時看板 HTML:</span><span class="sxs-lookup"><span data-stu-id="e8704-333">The stock ticker HTML:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

<span data-ttu-id="e8704-334">股票行情即時看板 CSS:</span><span class="sxs-lookup"><span data-stu-id="e8704-334">The stock ticker CSS:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

<span data-ttu-id="e8704-335">JQuery 程式碼，可捲動：</span><span class="sxs-lookup"><span data-stu-id="e8704-335">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="e8704-336">用戶端可以呼叫在伺服器上的其他方法</span><span class="sxs-lookup"><span data-stu-id="e8704-336">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="e8704-337">StockTickerHub 類別會定義四種用戶端可以呼叫的其他方法：</span><span class="sxs-lookup"><span data-stu-id="e8704-337">The StockTickerHub class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

<span data-ttu-id="e8704-338">OpenMarket、 CloseMarket 和重設會呼叫以回應位於頁面頂端的按鈕。</span><span class="sxs-lookup"><span data-stu-id="e8704-338">OpenMarket, CloseMarket, and Reset are called in response to the buttons at the top of the page.</span></span> <span data-ttu-id="e8704-339">它們也示範一部用戶端觸發立即傳播到所有用戶端的狀態變更的模式。</span><span class="sxs-lookup"><span data-stu-id="e8704-339">They demonstrate the pattern of one client triggering a change in state that is immediately propagated to all clients.</span></span> <span data-ttu-id="e8704-340">每一種方法呼叫中的方法 StockTicker 類別會影響市場狀態變更，然後再廣播的新狀態。</span><span class="sxs-lookup"><span data-stu-id="e8704-340">Each of these methods calls a method in the StockTicker class that effects the market state change and then broadcasts the new state.</span></span>

<span data-ttu-id="e8704-341">在 StockTicker 類別中，傳回 MarketState 列舉值的 MarketState 屬性來維護市場的狀態：</span><span class="sxs-lookup"><span data-stu-id="e8704-341">In the StockTicker class, the state of the market is maintained by a MarketState property that returns a MarketState enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

<span data-ttu-id="e8704-342">每個市場狀態變更的方法執行這項操作的鎖定區塊內因為 StockTicker 類別必須 threadsafe:</span><span class="sxs-lookup"><span data-stu-id="e8704-342">Each of the methods that change the market state do so inside a lock block because the StockTicker class has to be threadsafe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

<span data-ttu-id="e8704-343">若要確保此程式碼是 threadsafe， \_marketState 欄位支援 MarketState 屬性標記為 volatile，</span><span class="sxs-lookup"><span data-stu-id="e8704-343">To ensure that this code is threadsafe, the \_marketState field that backs the MarketState property is marked as volatile,</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

<span data-ttu-id="e8704-344">BroadcastMarketStateChange 和 BroadcastMarketReset 方法是已經看到的 BroadcastStockPrice 方法類似，只不過它們會呼叫不同的方法在用戶端定義：</span><span class="sxs-lookup"><span data-stu-id="e8704-344">The BroadcastMarketStateChange and BroadcastMarketReset methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="e8704-345">伺服器可以呼叫的用戶端上的其他函式</span><span class="sxs-lookup"><span data-stu-id="e8704-345">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="e8704-346">UpdateStockPrice 函式現在會處理格線和股票行情指示器顯示，並且會使用 jQuery.Color 閃爍紅色和綠色的色彩。</span><span class="sxs-lookup"><span data-stu-id="e8704-346">The updateStockPrice function now handles both the grid and the ticker display, and it uses jQuery.Color to flash red and green colors.</span></span>

<span data-ttu-id="e8704-347">中的新函式*SignalR.StockTicker.js*啟用和停用按鈕根據市場的狀態，並在停止或啟動股票行情指示器視窗水平捲軸。</span><span class="sxs-lookup"><span data-stu-id="e8704-347">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state, and they stop or start the ticker window horizontal scrolling.</span></span> <span data-ttu-id="e8704-348">因為多個函式加入至 ticker.client， [jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)用來將其加入。</span><span class="sxs-lookup"><span data-stu-id="e8704-348">Since multiple functions are being added to ticker.client, the [jQuery extend function](http://api.jquery.com/jQuery.extend/) is used to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="e8704-349">建立連接後的其他用戶端安裝</span><span class="sxs-lookup"><span data-stu-id="e8704-349">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="e8704-350">在用戶端建立連接之後，它有一些額外的工作，以執行： 找出市場是否開啟或關閉才能呼叫適當的 marketOpened 或 marketClosed 函式，並附加至按鈕的伺服器方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="e8704-350">After the client establishes the connection, it has some additional work to do: find out if the market is open or closed in order to call the appropriate marketOpened or marketClosed function, and attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

<span data-ttu-id="e8704-351">伺服器方法都不連線到直到按鈕之後建立的連線時，使程式碼無法嘗試呼叫伺服器方法，才可供。</span><span class="sxs-lookup"><span data-stu-id="e8704-351">The server methods are not wired up to the buttons until after the connection is established, so that the code can't try to call the server methods before they are available.</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="e8704-352">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e8704-352">Next steps</span></span>

<span data-ttu-id="e8704-353">在本教學課程中，您已學會如何程式從伺服器廣播訊息，所有已連線的用戶端，定期執行並以回應通知來自任何用戶端的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e8704-353">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients, both on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="e8704-354">使用多執行緒的單一執行個體維護伺服器狀態的模式也也可用在多人線上遊戲案例中。</span><span class="sxs-lookup"><span data-stu-id="e8704-354">The pattern of using a multi-threaded singleton instance to maintain server state can also be also used in multi-player online game scenarios.</span></span> <span data-ttu-id="e8704-355">如需範例，請參閱[ShootR 遊戲，取決於 SignalR](https://github.com/NTaylorMullen/ShootR)。</span><span class="sxs-lookup"><span data-stu-id="e8704-355">For an example, see [the ShootR game that is based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="e8704-356">如需顯示對等通訊案例的教學課程，請參閱[入門 SignalR](index.md)和[即時更新與 SignalR](index.md)。</span><span class="sxs-lookup"><span data-stu-id="e8704-356">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](index.md) and [Real-Time Updating with SignalR](index.md).</span></span>

<span data-ttu-id="e8704-357">深入了解更多進階的 SignalR 開發概念，請造訪下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="e8704-357">To learn more advanced SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="e8704-358">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="e8704-358">ASP.NET SignalR</span></span>](https://asp.net/signalr/)
- [<span data-ttu-id="e8704-359">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="e8704-359">SignalR Project</span></span>](http://signalr.net/)
- [<span data-ttu-id="e8704-360">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="e8704-360">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="e8704-361">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="e8704-361">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
