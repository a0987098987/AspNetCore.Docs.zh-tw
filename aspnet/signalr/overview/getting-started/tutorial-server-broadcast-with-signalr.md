---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 教學課程： 伺服器廣播與 SignalR 2 |Microsoft Docs
author: tdykstra
description: 本教學課程會示範如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。 伺服器廣播表示該 commun...
ms.author: aspnetcontent
ms.date: 10/13/2014
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0e86fbea9c5668e20fce7a494c76c52f9c089c09
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820693"
---
<a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="14ba4-104">教學課程： 伺服器廣播與 SignalR 2</span><span class="sxs-lookup"><span data-stu-id="14ba4-104">Tutorial: Server Broadcast with SignalR 2</span></span>
====================
<span data-ttu-id="14ba4-105">藉由[Tom Dykstra](https://github.com/tdykstra)， [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="14ba4-105">by [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="14ba4-106">本教學課程會示範如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-106">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="14ba4-107">伺服器廣播表示傳送至用戶端的通訊由伺服器起始。</span><span class="sxs-lookup"><span data-stu-id="14ba4-107">Server broadcast means that communications sent to clients are initiated by the server.</span></span> <span data-ttu-id="14ba4-108">這種情況下需要不同的程式設計方法，比對等案例，例如交談應用程式，在其中傳送到用戶端的通訊所起始的一或多個用戶端。</span><span class="sxs-lookup"><span data-stu-id="14ba4-108">This scenario requires a different programming approach than peer-to-peer scenarios such as chat applications, in which communications sent to clients are initiated by one or more of the clients.</span></span>
> 
> <span data-ttu-id="14ba4-109">您會在本教學課程中建立的應用程式會模擬股票行情即時看板，伺服器廣播功能的一般案例。</span><span class="sxs-lookup"><span data-stu-id="14ba4-109">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span>
> 
> <span data-ttu-id="14ba4-110">本主題最初是由 Patrick Fletcher 寫入。</span><span class="sxs-lookup"><span data-stu-id="14ba4-110">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="14ba4-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="14ba4-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="14ba4-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="14ba4-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="14ba4-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="14ba4-113">.NET 4.5</span></span>
> - <span data-ttu-id="14ba4-114">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="14ba4-114">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="14ba4-115">本教學課程中使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="14ba4-115">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="14ba4-116">若要使用 Visual Studio 2012，本教學課程中，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="14ba4-116">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="14ba4-117">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。</span><span class="sxs-lookup"><span data-stu-id="14ba4-117">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="14ba4-118">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-118">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="14ba4-119">在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-119">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="14ba4-120">這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-120">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="14ba4-121">有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="14ba4-121">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="14ba4-122">教學課程的版本</span><span class="sxs-lookup"><span data-stu-id="14ba4-122">Tutorial Versions</span></span>
> 
> <span data-ttu-id="14ba4-123">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-123">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="14ba4-124">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="14ba4-124">Questions and comments</span></span>
> 
> <span data-ttu-id="14ba4-125">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="14ba4-125">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="14ba4-126">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-126">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="14ba4-127">總覽</span><span class="sxs-lookup"><span data-stu-id="14ba4-127">Overview</span></span>

<span data-ttu-id="14ba4-128">在本教學課程中，您將建立股票行情指示器應用程式代表您要定期 「 發送 」 的即時應用程式，或廣播，從伺服器到所有已連線的用戶端的通知。</span><span class="sxs-lookup"><span data-stu-id="14ba4-128">In this tutorial, you'll create a stock ticker application that is representative of real-time applications in which you want to periodically "push," or broadcast, notifications from the server to all connected clients.</span></span> <span data-ttu-id="14ba4-129">在本教學課程的第一個部分中，您會從頭開始建立該應用程式的簡化的版本。</span><span class="sxs-lookup"><span data-stu-id="14ba4-129">In the first part of this tutorial, you'll create a simplified version of that application from scratch.</span></span> <span data-ttu-id="14ba4-130">在本教學課程的其餘部分，您將安裝 NuGet 套件，其中包含其他功能，並檢閱這些功能的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-130">In the remainder of the tutorial, you'll install a NuGet package that contains additional features, and review the code for those features.</span></span>

<span data-ttu-id="14ba4-131">在本教學課程的第一個部分中，您將建置的應用程式會顯示方格，以使用內建的資料。</span><span class="sxs-lookup"><span data-stu-id="14ba4-131">The application that you'll build in the first part of this tutorial displays a grid with stock data.</span></span>

![Stockservices.asmx 的初始版本](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="14ba4-133">定期伺服器隨機更新股價，並將更新推送至所有連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="14ba4-133">Periodically the server randomly updates stock prices and pushes the updates to all connected clients.</span></span> <span data-ttu-id="14ba4-134">在瀏覽器的數字和符號**變更**並**%** 動態變更通知來回應來自伺服器的資料行。</span><span class="sxs-lookup"><span data-stu-id="14ba4-134">In the browser the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="14ba4-135">如果您開啟其他瀏覽器相同的 URL，它們全都同時顯示相同的資料和資料的相同變更。</span><span class="sxs-lookup"><span data-stu-id="14ba4-135">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

<span data-ttu-id="14ba4-136">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="14ba4-136">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="14ba4-137">必要條件</span><span class="sxs-lookup"><span data-stu-id="14ba4-137">Prerequisites</span></span>](#prerequisites)
- [<span data-ttu-id="14ba4-138">建立專案</span><span class="sxs-lookup"><span data-stu-id="14ba4-138">Create the project</span></span>](#createproject)
- [<span data-ttu-id="14ba4-139">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="14ba4-139">Set up the server code</span></span>](#server)
- [<span data-ttu-id="14ba4-140">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="14ba4-140">Set up the client code</span></span>](#client)
- [<span data-ttu-id="14ba4-141">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="14ba4-141">Test the application</span></span>](#test)
- [<span data-ttu-id="14ba4-142">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="14ba4-142">Enable logging</span></span>](#enablelogging)
- [<span data-ttu-id="14ba4-143">安裝並檢閱完整的 Stockservices.asmx 範例</span><span class="sxs-lookup"><span data-stu-id="14ba4-143">Install and review the full StockTicker sample</span></span>](#fullsample)
- [<span data-ttu-id="14ba4-144">後續步驟</span><span class="sxs-lookup"><span data-stu-id="14ba4-144">Next steps</span></span>](#nextsteps)

<span data-ttu-id="14ba4-145">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件會安裝 Visual Studio 專案中的模擬的範例股票行情指示器應用程式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-145">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package installs a sample simulated stock ticker application in a Visual Studio project.</span></span>

> [!NOTE]
> <span data-ttu-id="14ba4-146">如果您不想要逐步建置應用程式的步驟，您可以安裝 SignalR.Sample 封裝在新的空白的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="14ba4-146">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="14ba4-147">如果您沒有執行本教學課程中的步驟安裝 NuGet 套件**您必須依照 readme.txt 檔案中的指示**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-147">If you install the NuGet package without performing the steps in this tutorial, **you must follow the instructions in the readme.txt file**.</span></span> <span data-ttu-id="14ba4-148">若要執行封裝，您需要新增 OWIN 啟動類別中已安裝的封裝呼叫 ConfigureSignalR 方法。</span><span class="sxs-lookup"><span data-stu-id="14ba4-148">To run the package you need to add an OWIN startup class which calls the ConfigureSignalR method in the installed package.</span></span> <span data-ttu-id="14ba4-149">如果您需要新增 OWIN 啟動類別，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="14ba4-149">You will receive an error if you do not add the OWIN startup class.</span></span>


<a id="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="14ba4-150">必要條件</span><span class="sxs-lookup"><span data-stu-id="14ba4-150">Prerequisites</span></span>

<span data-ttu-id="14ba4-151">在開始之前，請確定您已在電腦上安裝的 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="14ba4-151">Before you start, make sure that you have Visual Studio 2013 installed on your computer.</span></span> <span data-ttu-id="14ba4-152">如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得的免費 Visual Studio 2013 Express。</span><span class="sxs-lookup"><span data-stu-id="14ba4-152">If you don't have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express.</span></span>

<a id="createproject"></a>

## <a name="create-the-project"></a><span data-ttu-id="14ba4-153">建立專案</span><span class="sxs-lookup"><span data-stu-id="14ba4-153">Create the project</span></span>

1. <span data-ttu-id="14ba4-154">從**檔案**功能表上，按一下**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-154">From the **File** menu, click **New Project**.</span></span>
2. <span data-ttu-id="14ba4-155">在**新的專案**對話方塊方塊中，展開**C#** 之下**範本**，然後選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-155">In the **New Project** dialog box, expand **C#** under **Templates** and select **Web**.</span></span>
3. <span data-ttu-id="14ba4-156">選取  **ASP.NET 空白 Web 應用程式**範本，將專案命名為*SignalR.StockTicker*，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-156">Select the **ASP.NET Empty Web Application** template, name the project *SignalR.StockTicker*, and click **OK**.</span></span>

    ![新增專案對話方塊](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. <span data-ttu-id="14ba4-158">在 **新的 ASP.NET**專案 視窗中，保持**空白**選取，然後按一下 **建立專案**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-158">In the **New ASP.NET** Project window, leave **Empty** selected and click **Create Project**.</span></span>

    ![新增 ASP 專案對話方塊](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a><span data-ttu-id="14ba4-160">設定伺服器程式碼</span><span class="sxs-lookup"><span data-stu-id="14ba4-160">Set up the server code</span></span>

<span data-ttu-id="14ba4-161">在本節中您設定在伺服器執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-161">In this section you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="14ba4-162">建立庫存類別</span><span class="sxs-lookup"><span data-stu-id="14ba4-162">Create the Stock class</span></span>

<span data-ttu-id="14ba4-163">請先建立您將使用它來儲存和傳輸股票的相關資訊的內建模型類別。</span><span class="sxs-lookup"><span data-stu-id="14ba4-163">You begin by creating the Stock model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="14ba4-164">在專案資料夾中建立新的類別檔案，將其命名*Stock.cs*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="14ba4-164">Create a new class file in the project folder, name it *Stock.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="14ba4-165">當您建立的股票時，您會將兩個屬性是符號 (例如，microsoft 的 MSFT) 和價格。</span><span class="sxs-lookup"><span data-stu-id="14ba4-165">The two properties that you'll set when you create stocks are the Symbol (for example, MSFT for Microsoft) and the Price.</span></span> <span data-ttu-id="14ba4-166">其他屬性取決於您如何及何時設定價格。</span><span class="sxs-lookup"><span data-stu-id="14ba4-166">The other properties depend on how and when you set Price.</span></span> <span data-ttu-id="14ba4-167">第一次設定價格值取得傳播到 DayOpen。</span><span class="sxs-lookup"><span data-stu-id="14ba4-167">The first time you set Price, the value gets propagated to DayOpen.</span></span> <span data-ttu-id="14ba4-168">後續的階段，當您設定價格變更，且 PercentChange 屬性值的計算方式為基礎的價格和 DayOpen 之間的差異。</span><span class="sxs-lookup"><span data-stu-id="14ba4-168">Subsequent times when you set Price, the Change and PercentChange property values are calculated based on the difference between Price and DayOpen.</span></span>

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a><span data-ttu-id="14ba4-169">建立 Stockservices.asmx 和 StockTickerHub 類別</span><span class="sxs-lookup"><span data-stu-id="14ba4-169">Create the StockTicker and StockTickerHub classes</span></span>

<span data-ttu-id="14ba4-170">您將使用 SignalR 中樞 API 來處理伺服器到用戶端互動。</span><span class="sxs-lookup"><span data-stu-id="14ba4-170">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="14ba4-171">StockTickerHub 類別衍生自 SignalR Hub 類別會處理來自用戶端接收連線和方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="14ba4-171">A StockTickerHub class that derives from the SignalR Hub class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="14ba4-172">您也需要維護的內建資料並執行的計時器物件，以定期觸發價格更新，不需依賴用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="14ba4-172">You also need to maintain stock data and run a Timer object to periodically trigger price updates, independently of client connections.</span></span> <span data-ttu-id="14ba4-173">因為暫時性中樞執行個體，您無法暫停在中樞類別中，這些函式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-173">You can't put these functions in a Hub class, because Hub instances are transient.</span></span> <span data-ttu-id="14ba4-174">在中樞內，例如連線和伺服器從用戶端呼叫的每個作業建立中樞類別執行個體。</span><span class="sxs-lookup"><span data-stu-id="14ba4-174">A Hub class instance is created for each operation on the hub, such as connections and calls from the client to the server.</span></span> <span data-ttu-id="14ba4-175">因此，必須執行在不同的類別，您將會命名為 Stockservices.asmx 的機制，可讓內建的資料、 更新價格，接著在廣播價格更新。</span><span class="sxs-lookup"><span data-stu-id="14ba4-175">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class, which you'll name StockTicker.</span></span>

![從 Stockservices.asmx 的廣播](tutorial-server-broadcast-with-signalr/_static/image5.png)

<span data-ttu-id="14ba4-177">您只想 Stockservices.asmx 類別，因此您必須設定為參考從每個 StockTickerHub 執行個體的單一 Stockservices.asmx 執行個體的伺服器上，執行一個執行個體。</span><span class="sxs-lookup"><span data-stu-id="14ba4-177">You only want one instance of the StockTicker class to run on the server, so you'll need to set up a reference from each StockTickerHub instance to the singleton StockTicker instance.</span></span> <span data-ttu-id="14ba4-178">Stockservices.asmx 類別必須能夠向用戶端廣播，因為它具有內建的資料，以及觸發更新，但 Stockservices.asmx 不 Hub 類別。</span><span class="sxs-lookup"><span data-stu-id="14ba4-178">The StockTicker class has to be able to broadcast to clients because it has the stock data and triggers updates, but StockTicker is not a Hub class.</span></span> <span data-ttu-id="14ba4-179">因此，Stockservices.asmx 類別必須取得 SignalR 中樞的連接內容物件的參考。</span><span class="sxs-lookup"><span data-stu-id="14ba4-179">Therefore, the StockTicker class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="14ba4-180">它可以接著使用 SignalR 連線內容物件廣播給用戶端。</span><span class="sxs-lookup"><span data-stu-id="14ba4-180">It can then use the SignalR connection context object to broadcast to clients.</span></span>

1. <span data-ttu-id="14ba4-181">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**新增 |SignalR Hub 類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-181">In **Solution Explorer**, right-click the project and click **Add | SignalR Hub Class (v2)**.</span></span>
2. <span data-ttu-id="14ba4-182">命名新的中樞*StockTickerHub.cs*，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-182">Name the new hub *StockTickerHub.cs*, and then click **Add**.</span></span> <span data-ttu-id="14ba4-183">SignalR NuGet 封裝會新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="14ba4-183">SignalR NuGet packages will be added to your project.</span></span>
3. <span data-ttu-id="14ba4-184">下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="14ba4-184">Replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    <span data-ttu-id="14ba4-185">[中樞](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)類別用來定義用戶端可以呼叫在伺服器的方法。</span><span class="sxs-lookup"><span data-stu-id="14ba4-185">The [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class is used to define methods the clients can call on the server.</span></span> <span data-ttu-id="14ba4-186">您要定義一種方法： `GetAllStocks()`。</span><span class="sxs-lookup"><span data-stu-id="14ba4-186">You are defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="14ba4-187">用戶端一開始會連接到伺服器，它會呼叫這個方法，以取得所有與目前的價格股票的清單。</span><span class="sxs-lookup"><span data-stu-id="14ba4-187">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="14ba4-188">此方法可以同步執行，並傳回`IEnumerable<Stock>`因為它會從記憶體中傳回資料。</span><span class="sxs-lookup"><span data-stu-id="14ba4-188">The method can execute synchronously and return `IEnumerable<Stock>` because it is returning data from memory.</span></span> <span data-ttu-id="14ba4-189">如果方法必須取得資料所做的事會牽涉到等待，例如資料庫尋查 」 或 「 web 服務呼叫，您會指定`Task<IEnumerable<Stock>>`當做傳回值，以啟用非同步處理。</span><span class="sxs-lookup"><span data-stu-id="14ba4-189">If the method had to get the data by doing something that would involve waiting, such as a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="14ba4-190">如需詳細資訊，請參閱 < [ASP.NET SignalR 中樞 API 指南-Server-以非同步方式執行的時機](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-190">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

    <span data-ttu-id="14ba4-191">HubName 屬性會指定中樞上的用戶端的 JavaScript 程式碼的參考方式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-191">The HubName attribute specifies how the Hub will be referenced in JavaScript code on the client.</span></span> <span data-ttu-id="14ba4-192">如果您未使用這個屬性在用戶端上的預設名稱是類別名稱，在此情況下會 stockTickerHub 的 camel 案例版本。</span><span class="sxs-lookup"><span data-stu-id="14ba4-192">The default name on the client if you don't use this attribute is a camel-cased version of the class name, which in this case would be stockTickerHub.</span></span>

    <span data-ttu-id="14ba4-193">因為您稍後就會看到當您建立 Stockservices.asmx 類別，該類別的單一執行個體被建立在其靜態執行個體屬性中。</span><span class="sxs-lookup"><span data-stu-id="14ba4-193">As you'll see later when you create the StockTicker class, a singleton instance of that class is created in its static Instance property.</span></span> <span data-ttu-id="14ba4-194">Stockservices.asmx 的單一執行個體保留多少個用戶端連線或中斷連線，無論記憶體中，而且該執行個體是 GetAllStocks 方法用來傳回目前的庫存資訊。</span><span class="sxs-lookup"><span data-stu-id="14ba4-194">That singleton instance of StockTicker remains in memory no matter how many clients connect or disconnect, and that instance is what the GetAllStocks method uses to return current stock information.</span></span>
4. <span data-ttu-id="14ba4-195">在專案資料夾中建立新的類別檔案，將其命名*StockTicker.cs*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="14ba4-195">Create a new class file in the project folder, name it *StockTicker.cs*, and then replace the template code with the following code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    <span data-ttu-id="14ba4-196">因為多個執行緒會執行為 Stockservices.asmx 的程式碼的相同執行個體，Stockservices.asmx 類別必須是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="14ba4-196">Since multiple threads will be running the same instance of StockTicker code, the StockTicker class has to be threadsafe.</span></span>

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="14ba4-197">將單一執行個體儲存在靜態欄位</span><span class="sxs-lookup"><span data-stu-id="14ba4-197">Storing the singleton instance in a static field</span></span>

    <span data-ttu-id="14ba4-198">程式碼會初始化靜態\_支援的類別和此執行個體的執行個體屬性的執行個體欄位是唯一的執行個體，可以建立的類別，因為建構函式已標示為私用。</span><span class="sxs-lookup"><span data-stu-id="14ba4-198">The code initializes the static \_instance field that backs the Instance property with an instance of the class, and this is the only instance of the class that can be created, because the constructor is marked as private.</span></span> <span data-ttu-id="14ba4-199">[延遲初始設定](https://msdn.microsoft.com/library/dd997286.aspx)使用於\_執行個體欄位，不會基於效能的考量，但若要確定執行個體的建立是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="14ba4-199">[Lazy initialization](https://msdn.microsoft.com/library/dd997286.aspx) is used for the \_instance field, not for performance reasons but to ensure that the instance creation is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    <span data-ttu-id="14ba4-200">用戶端連線到伺服器，每次在個別的執行緒中執行的 StockTickerHub 類別的新執行個體取得 Stockservices.asmx 的單一執行個體從 StockTicker.Instance 靜態屬性，如您稍早在 StockTickerHub 類別中所見。</span><span class="sxs-lookup"><span data-stu-id="14ba4-200">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the StockTicker.Instance static property, as you saw earlier in the StockTickerHub class.</span></span>

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="14ba4-201">在 ConcurrentDictionary 中儲存內建的資料</span><span class="sxs-lookup"><span data-stu-id="14ba4-201">Storing stock data in a ConcurrentDictionary</span></span>

    <span data-ttu-id="14ba4-202">建構函式初始化\_具備一些內建資料範例及 GetAllStocks 股票集合會傳回股票。</span><span class="sxs-lookup"><span data-stu-id="14ba4-202">The constructor initializes the \_stocks collection with some sample stock data, and GetAllStocks returns the stocks.</span></span> <span data-ttu-id="14ba4-203">如稍早所見，StockTickerHub.GetAllStocks 也就是在用戶端可以呼叫 Hub 類別中的伺服器方法接著會傳回這個集合的股票。</span><span class="sxs-lookup"><span data-stu-id="14ba4-203">As you saw earlier, this collection of stocks is in turn returned by StockTickerHub.GetAllStocks which is a server method in the Hub class that clients can call.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    <span data-ttu-id="14ba4-204">股票集合指[ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)執行緒安全的型別。</span><span class="sxs-lookup"><span data-stu-id="14ba4-204">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="14ba4-205">或者，您可以使用[字典](https://msdn.microsoft.com/library/xfhwa508.aspx)物件，並明確地鎖定字典，對它進行變更時。</span><span class="sxs-lookup"><span data-stu-id="14ba4-205">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

    <span data-ttu-id="14ba4-206">此範例應用程式，它是 [確定] 儲存在記憶體中的應用程式資料，並處置 Stockservices.asmx 的執行個體時，會遺失資料。</span><span class="sxs-lookup"><span data-stu-id="14ba4-206">For this sample application, it's OK to store application data in memory and to lose the data when the StockTicker instance is disposed.</span></span> <span data-ttu-id="14ba4-207">在實際的應用程式中您會使用後端資料存放區，例如資料庫。</span><span class="sxs-lookup"><span data-stu-id="14ba4-207">In a real application you would work with a back-end data store such as a database.</span></span>

    ### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="14ba4-208">定期更新股票價格</span><span class="sxs-lookup"><span data-stu-id="14ba4-208">Periodically updating stock prices</span></span>

    <span data-ttu-id="14ba4-209">建構函式會啟動計時器物件的定期呼叫更新股價隨機為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="14ba4-209">The constructor starts up a Timer object that periodically calls methods that update stock prices on a random basis.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    <span data-ttu-id="14ba4-210">UpdateStockPrices 是由計時器，state 參數中的 null 會傳入呼叫。</span><span class="sxs-lookup"><span data-stu-id="14ba4-210">UpdateStockPrices is called by the Timer, which passes in null in the state parameter.</span></span> <span data-ttu-id="14ba4-211">更新價格之前, 取得的鎖定\_updateStockPricesLock 物件。</span><span class="sxs-lookup"><span data-stu-id="14ba4-211">Before updating prices, a lock is taken on the \_updateStockPricesLock object.</span></span> <span data-ttu-id="14ba4-212">如果另一個執行緒正在更新價格，然後在清單中的每個股票呼叫 TryUpdateStockPrice，則會檢查程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-212">The code checks if another thread is already updating prices, and then it calls TryUpdateStockPrice on each stock in the list.</span></span> <span data-ttu-id="14ba4-213">TryUpdateStockPrice 方法會決定是否要變更的股票的價格，以及有多少變更它。</span><span class="sxs-lookup"><span data-stu-id="14ba4-213">The TryUpdateStockPrice method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="14ba4-214">如果變更的股價，BroadcastStockPrice 稱為廣播到所有已連線的用戶端的股票價格變更。</span><span class="sxs-lookup"><span data-stu-id="14ba4-214">If the stock price is changed, BroadcastStockPrice is called to broadcast the stock price change to all connected clients.</span></span>

    <span data-ttu-id="14ba4-215">\_UpdatingStockPrices 旗標標示[volatile](https://msdn.microsoft.com/library/x13ttww7.aspx)以確保其存取權是 threadsafe。</span><span class="sxs-lookup"><span data-stu-id="14ba4-215">The \_updatingStockPrices flag is marked as [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to ensure that access to it is threadsafe.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    <span data-ttu-id="14ba4-216">在實際的應用程式，TryUpdateStockPrice 方法會呼叫 web 服務，以查閱價格;在此程式碼中，它會使用隨機的數字產生器隨機進行變更。</span><span class="sxs-lookup"><span data-stu-id="14ba4-216">In a real application, the TryUpdateStockPrice method would call a web service to look up the price; in this code it uses a random number generator to make changes randomly.</span></span>

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="14ba4-217">取得 SignalR 內容，以便 Stockservices.asmx 類別可以廣播給用戶端</span><span class="sxs-lookup"><span data-stu-id="14ba4-217">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

    <span data-ttu-id="14ba4-218">因為價格變更以下來自 Stockservices.asmx 物件中，這會是需要在所有連線的用戶端上呼叫 updateStockPrice 方法的物件。</span><span class="sxs-lookup"><span data-stu-id="14ba4-218">Because the price changes originate here in the StockTicker object, this is the object that needs to call an updateStockPrice method on all connected clients.</span></span> <span data-ttu-id="14ba4-219">中樞類別中您的 API 呼叫的用戶端方法，但 Stockservices.asmx 並非衍生自 Hub 類別，而且沒有任何中樞物件的參考。</span><span class="sxs-lookup"><span data-stu-id="14ba4-219">In a Hub class you have an API for calling client methods, but StockTicker does not derive from the Hub class and does not have a reference to any Hub object.</span></span> <span data-ttu-id="14ba4-220">因此，若要廣播至連線的用戶端，Stockservices.asmx 類別必須 StockTickerHub 類別取得 SignalR 的 context 執行個體，並使用它來呼叫用戶端上的方法。</span><span class="sxs-lookup"><span data-stu-id="14ba4-220">Therefore, in order to broadcast to connected clients, the StockTicker class has to get the SignalR context instance for the StockTickerHub class and use that to call methods on clients.</span></span>

    <span data-ttu-id="14ba4-221">建立單一類別執行個體，建構函式，將該參考時，此程式碼會取得 SignalR 內容的參考，建構函式將它放在用戶端屬性。</span><span class="sxs-lookup"><span data-stu-id="14ba4-221">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the Clients property.</span></span>

    <span data-ttu-id="14ba4-222">有兩個您想要一次取得內容的原因： 取得內容是昂貴的作業，並取得它一次可確保預期的順序傳送至用戶端的訊息會保留。</span><span class="sxs-lookup"><span data-stu-id="14ba4-222">There are two reasons why you want to get the context just once: getting the context is an expensive operation, and getting it once ensures that the intended order of messages sent to clients is preserved.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    <span data-ttu-id="14ba4-223">取得內容的用戶端屬性，並將它放在 StockTickerClient 屬性可讓您撰寫來呼叫用戶端方法看起來一樣如同中樞類別中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-223">Getting the Clients property of the context and putting it in the StockTickerClient property lets you write code to call client methods that looks the same as it would in a Hub class.</span></span> <span data-ttu-id="14ba4-224">比方說，廣播到所有用戶端，您可以撰寫 Clients.All.updateStockPrice(stock)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-224">For instance, to broadcast to all clients you can write Clients.All.updateStockPrice(stock).</span></span>

    <span data-ttu-id="14ba4-225">您呼叫在 BroadcastStockPrice updateStockPrice 方法還; 不存在稍後當您撰寫的用戶端執行的程式碼時，會將其新增。</span><span class="sxs-lookup"><span data-stu-id="14ba4-225">The updateStockPrice method that you are calling in BroadcastStockPrice doesn't exist yet; you'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="14ba4-226">您可以參考以下 updateStockPrice 因為 Clients.All 是動態，這表示將會在執行階段評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-226">You can refer to updateStockPrice here because Clients.All is dynamic, which means the expression will be evaluated at runtime.</span></span> <span data-ttu-id="14ba4-227">SignalR 當這個方法呼叫執行時，會傳送的方法名稱和參數值，用戶端，而且如果用戶端有一個名為 updateStockPrice 方法，就會呼叫該方法的參數值會傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="14ba4-227">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named updateStockPrice, that method will be called and the parameter value will be passed to it.</span></span>

    <span data-ttu-id="14ba4-228">Clients.All 表示傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="14ba4-228">Clients.All means send to all clients.</span></span> <span data-ttu-id="14ba4-229">SignalR 提供其他選項，以指定哪些用戶端或用戶端傳送至群組。</span><span class="sxs-lookup"><span data-stu-id="14ba4-229">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="14ba4-230">如需詳細資訊，請參閱 < [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-230">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="14ba4-231">註冊 SignalR 的路由</span><span class="sxs-lookup"><span data-stu-id="14ba4-231">Register the SignalR route</span></span>

<span data-ttu-id="14ba4-232">伺服器必須知道要攔截，並將導向至 SignalR 的 URL。</span><span class="sxs-lookup"><span data-stu-id="14ba4-232">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="14ba4-233">若要這樣做，請新增 OWIN 啟動類別：</span><span class="sxs-lookup"><span data-stu-id="14ba4-233">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="14ba4-234">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |OWIN 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-234">In **Solution Explorer**, right-click the project, and then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="14ba4-235">將類別命名為**Startup.cs**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-235">Name the class **Startup.cs**.</span></span>
2. <span data-ttu-id="14ba4-236">中的程式碼取代**Startup.cs**取代下列項目。</span><span class="sxs-lookup"><span data-stu-id="14ba4-236">Replace the code in **Startup.cs** with the following.</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="14ba4-237">您現在已完成設定伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-237">You have now completed setting up the server code.</span></span> <span data-ttu-id="14ba4-238">在下一節中您會設定用戶端。</span><span class="sxs-lookup"><span data-stu-id="14ba4-238">In the next section you'll set up the client.</span></span>

<a id="client"></a>

## <a name="set-up-the-client-code"></a><span data-ttu-id="14ba4-239">設定用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="14ba4-239">Set up the client code</span></span>

1. <span data-ttu-id="14ba4-240">新的 HTML 檔案的資料夾中建立專案，並將它命名*StockTicker.html*。</span><span class="sxs-lookup"><span data-stu-id="14ba4-240">Create a new HTML file in the project folder, and name it *StockTicker.html*.</span></span>
2. <span data-ttu-id="14ba4-241">下列程式碼取代範本程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-241">Replace the template code with the following code.</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    <span data-ttu-id="14ba4-242">HTML 5 個資料行、 標頭資料列與資料列與單一儲存格跨越所有的 5 個資料行建立的資料表。</span><span class="sxs-lookup"><span data-stu-id="14ba4-242">The HTML creates a table with 5 columns, a header row, and a data row with a single cell that spans all 5 columns.</span></span> <span data-ttu-id="14ba4-243">資料列會顯示 「 正在載入...」，才會暫時在應用程式啟動時，才會顯示。</span><span class="sxs-lookup"><span data-stu-id="14ba4-243">The data row displays "loading..." and will only be shown momentarily when the application starts.</span></span> <span data-ttu-id="14ba4-244">JavaScript 程式碼將會移除該資料列，並在其位置的資料列中新增，並從伺服器擷取的內建資料。</span><span class="sxs-lookup"><span data-stu-id="14ba4-244">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="14ba4-245">指令碼標記指定 jQuery 指令碼檔案，SignalR core 指令碼檔案、 SignalR proxy 指令碼檔案中，您稍後將建立 Stockservices.asmx 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="14ba4-245">The script tags specify the jQuery script file, the SignalR core script file, the SignalR proxies script file, and a StockTicker script file that you'll create later.</span></span> <span data-ttu-id="14ba4-246">SignalR proxy 指令碼檔案，指定 「 signalr/中樞 」 的 URL，以動態方式產生，並在中樞類別、 方法的 proxy 方法定義在此情況下 StockTickerHub.GetAllStocks。</span><span class="sxs-lookup"><span data-stu-id="14ba4-246">The SignalR proxies script file, which specifies the "/signalr/hubs" URL, is dynamically generated and defines proxy methods for the methods on the Hub class, in this case for StockTickerHub.GetAllStocks.</span></span> <span data-ttu-id="14ba4-247">如果您想，您可以產生這個 JavaScript 檔案以手動方式使用[SignalR 的公用程式](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)和停用動態檔案建立 MapHubs 方法呼叫中的。</span><span class="sxs-lookup"><span data-stu-id="14ba4-247">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) and disable dynamic file creation in the MapHubs method call.</span></span>
3. > [!IMPORTANT]
   > <span data-ttu-id="14ba4-248">請確定 JavaScript 檔案參考中*StockTicker.html*正確無誤。</span><span class="sxs-lookup"><span data-stu-id="14ba4-248">Make sure that the JavaScript file references in *StockTicker.html* are correct.</span></span> <span data-ttu-id="14ba4-249">也就是確定您的指令碼標記 (在範例中的 1.10.2) 中的 jQuery 版本是 jQuery 版本在專案的相同*指令碼*資料夾，並確定您的指令碼標記中的 SignalR 版本是 SignalR 相同在您的專案中的版本*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="14ba4-249">That is, make sure that the jQuery version in your script tag (1.10.2 in the example) is the same as the jQuery version in your project's *Scripts* folder, and make sure that the SignalR version in your script tag is the same as the SignalR version in your project's *Scripts* folder.</span></span> <span data-ttu-id="14ba4-250">如有必要，請變更指令碼標記中的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="14ba4-250">Change the file names in the script tags if necessary.</span></span>
4. <span data-ttu-id="14ba4-251">在 **方案總管 中**，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-251">In **Solution Explorer**, right-click *StockTicker.html*, and then click **Set as Start Page**.</span></span>
5. <span data-ttu-id="14ba4-252">在專案資料夾中建立新的 JavaScript 檔案並將它命名*StockTicker.js*...</span><span class="sxs-lookup"><span data-stu-id="14ba4-252">Create a new JavaScript file in the project folder and name it *StockTicker.js*..</span></span>
6. <span data-ttu-id="14ba4-253">下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="14ba4-253">Replace the template code with the following code:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    <span data-ttu-id="14ba4-254">$.connection 指 SignalR proxy。</span><span class="sxs-lookup"><span data-stu-id="14ba4-254">$.connection refers to the SignalR proxies.</span></span> <span data-ttu-id="14ba4-255">程式碼取得 StockTickerHub 類別將 proxy 的參考，並將它放在股票行情指示器變數中。</span><span class="sxs-lookup"><span data-stu-id="14ba4-255">The code gets a reference to the proxy for the StockTickerHub class and puts it in the ticker variable.</span></span> <span data-ttu-id="14ba4-256">Proxy 名稱會是 [HubName] 屬性所設定的名稱：</span><span class="sxs-lookup"><span data-stu-id="14ba4-256">The proxy name is the name that was set by the [HubName] attribute:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    <span data-ttu-id="14ba4-257">定義所有變數和函式之後，檔案中的程式碼的最後一行會初始化 SignalR 連線，藉由呼叫 SignalR 開始函式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-257">After all the variables and functions are defined, the last line of code in the file initializes the SignalR connection by calling the SignalR start function.</span></span> <span data-ttu-id="14ba4-258">開始函式以非同步方式執行，並傳回[jQuery 延後物件](http://api.jquery.com/category/deferred-object/)，這表示您可以呼叫完成的函式，以指定的非同步作業完成時要呼叫的函式...</span><span class="sxs-lookup"><span data-stu-id="14ba4-258">The start function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means you can call the done function to specify the function to call when the asynchronous operation is completed..</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    <span data-ttu-id="14ba4-259">Init 函式 getAllStocks 函式會呼叫伺服器上，並使用伺服器來更新內建的資料表所傳回的資訊。</span><span class="sxs-lookup"><span data-stu-id="14ba4-259">The init function calls the getAllStocks function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="14ba4-260">請注意，根據預設，您必須使用駝峰式命名法大小寫用戶端上雖然方法名稱是依照 pascal 命名法大小寫，在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="14ba4-260">Notice that by default, you have to use camel casing on the client although the method name is pascal-cased on the server.</span></span> <span data-ttu-id="14ba4-261">依照 camel 命名法大小寫規則只適用於不是物件的方法。</span><span class="sxs-lookup"><span data-stu-id="14ba4-261">The camel-casing rule only applies to methods, not objects.</span></span> <span data-ttu-id="14ba4-262">例如，您參考股票。符號 」 與 「 存貨。價格、 未 stock.symbol 或 stock.price。</span><span class="sxs-lookup"><span data-stu-id="14ba4-262">For example, you refer to stock.Symbol and stock.Price, not stock.symbol or stock.price.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    <span data-ttu-id="14ba4-263">如果您想要在用戶端，使用 pascal 命名法大小寫，或如果您想要使用完全不同的方法名稱，您可以裝飾具有 HubMethodName 屬性之中樞方法相同的方式會裝飾 Hub 類別具有 HubName 屬性。</span><span class="sxs-lookup"><span data-stu-id="14ba4-263">If you wanted to use pascal casing on the client, or if you wanted to use a completely different method name, you could decorate the Hub method with the HubMethodName attribute the same way you decorated the Hub class itself with the HubName attribute.</span></span>

    <span data-ttu-id="14ba4-264">在 init 方法中，HTML 資料表資料列會建立從伺服器收到的呼叫 formatStock 格式內容的內建的物件，每一個股票物件，並藉由呼叫取代式裝置 (定義在頂端*StockTicker.js*) 取代內建的物件屬性值的 rowTemplate 變數中的預留位置。</span><span class="sxs-lookup"><span data-stu-id="14ba4-264">In the init method, HTML for a table row is created for each stock object received from the server by calling formatStock to format properties of the stock object, and then by calling supplant (which is defined at the top of *StockTicker.js*) to replace placeholders in the rowTemplate variable with the stock object property values.</span></span> <span data-ttu-id="14ba4-265">產生的 HTML 則會附加至內建的資料表中。</span><span class="sxs-lookup"><span data-stu-id="14ba4-265">The resulting HTML is then appended to the stock table.</span></span>

    <span data-ttu-id="14ba4-266">您可以傳遞，以執行非同步的啟動函式完成後的回呼函式呼叫 init。</span><span class="sxs-lookup"><span data-stu-id="14ba4-266">You call init by passing it in as a callback function that executes after the asynchronous start function completes.</span></span> <span data-ttu-id="14ba4-267">如果您呼叫 init 做為個別的 JavaScript 陳述式之後呼叫開始時，函式會失敗，因為它會立即執行而不需等待開始函式，來完成 建立連線。</span><span class="sxs-lookup"><span data-stu-id="14ba4-267">If you called init as a separate JavaScript statement after calling start, the function would fail because it would execute immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="14ba4-268">在此情況下，init 函式會嘗試建立伺服器連接之前呼叫 getAllStocks 函式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-268">In that case, the init function would try to call the getAllStocks function before the server connection is established.</span></span>

    <span data-ttu-id="14ba4-269">當伺服器變成股票的價格時，它會呼叫 updateStockPrice 連線的用戶端上。</span><span class="sxs-lookup"><span data-stu-id="14ba4-269">When the server changes a stock's price, it calls the updateStockPrice on connected clients.</span></span> <span data-ttu-id="14ba4-270">此函式會新增至 Stockservices.asmx proxy 的用戶端屬性，以便將它提供給呼叫從伺服器。</span><span class="sxs-lookup"><span data-stu-id="14ba4-270">The function is added to the client property of the stockTicker proxy in order to make it available to calls from the server.</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    <span data-ttu-id="14ba4-271">UpdateStockPrice 函式會將接收自伺服器的資料表資料列到 init 函式的相同方式將內建物件格式化。</span><span class="sxs-lookup"><span data-stu-id="14ba4-271">The updateStockPrice function formats a stock object received from the server into a table row the same way as in the init function.</span></span> <span data-ttu-id="14ba4-272">不過，而不是將資料列附加至資料表，它在資料表中尋找股票的目前資料列，並以新的取代該資料列。</span><span class="sxs-lookup"><span data-stu-id="14ba4-272">However, instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

<a id="test"></a>

## <a name="test-the-application"></a><span data-ttu-id="14ba4-273">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="14ba4-273">Test the application</span></span>

1. <span data-ttu-id="14ba4-274">按 F5 以偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-274">Press F5 to run the application in debug mode.</span></span>

    <span data-ttu-id="14ba4-275">內建資料表一開始會顯示 「 正在載入...」 列，然後顯示初始的內建資料，在短暫延遲之後，再股票價格開始變更。</span><span class="sxs-lookup"><span data-stu-id="14ba4-275">The stock table initially displays the "loading..." line, then after a short delay the initial stock data is displayed, and then the stock prices start to change.</span></span>

    ![正在載入](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![初始的內建資料表](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![從伺服器接收變更內建的資料表](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. <span data-ttu-id="14ba4-279">從瀏覽器網址列複製 URL，並將它貼到一或多個新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="14ba4-279">Copy the URL from the browser address bar and paste it into one or more new browser window(s).</span></span>

    <span data-ttu-id="14ba4-280">初始的內建顯示等同於第一個瀏覽器，並同時進行變更。</span><span class="sxs-lookup"><span data-stu-id="14ba4-280">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>
3. <span data-ttu-id="14ba4-281">關閉所有瀏覽器並開啟新的瀏覽器，然後移至相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="14ba4-281">Close all browsers and open a new browser, then go to the same URL.</span></span>

    <span data-ttu-id="14ba4-282">若要在伺服器中執行，因此內建的資料表會顯示股票，若要變更持續一直在 Stockservices.asmx 單一物件。</span><span class="sxs-lookup"><span data-stu-id="14ba4-282">The StockTicker singleton object has continued to run in the server, so the stock table display shows that the stocks have continued to change.</span></span> <span data-ttu-id="14ba4-283">（您沒有看到變更數據的初始資料表為零）。</span><span class="sxs-lookup"><span data-stu-id="14ba4-283">(You don't see the initial table with zero change figures.)</span></span>
4. <span data-ttu-id="14ba4-284">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="14ba4-284">Close the browser.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="14ba4-285">啟用記錄</span><span class="sxs-lookup"><span data-stu-id="14ba4-285">Enable logging</span></span>

<span data-ttu-id="14ba4-286">SignalR 有內建記錄功能，您可以讓用戶端上，以協助疑難排解。</span><span class="sxs-lookup"><span data-stu-id="14ba4-286">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="14ba4-287">在本節中您要啟用記錄，並查看範例，說明如何記錄告訴您哪一種下列傳輸方法使用 SignalR:</span><span class="sxs-lookup"><span data-stu-id="14ba4-287">In this section you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

- <span data-ttu-id="14ba4-288">[Websocket](http://en.wikipedia.org/wiki/WebSocket)、 支援的 IIS 8 和目前的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="14ba4-288">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>
- <span data-ttu-id="14ba4-289">[伺服器傳送事件](http://en.wikipedia.org/wiki/Server-sent_events)、 非 Internet Explorer 的瀏覽器所支援。</span><span class="sxs-lookup"><span data-stu-id="14ba4-289">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>
- <span data-ttu-id="14ba4-290">[永久框架](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、 Internet explorer 的支援。</span><span class="sxs-lookup"><span data-stu-id="14ba4-290">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>
- <span data-ttu-id="14ba4-291">[長時間輪詢的 Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、 支援所有的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="14ba4-291">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="14ba4-292">針對任何指定的連接，SignalR 會選擇最佳的伺服器和用戶端支援的傳輸方法。</span><span class="sxs-lookup"><span data-stu-id="14ba4-292">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="14ba4-293">開啟*StockTicker.js*並新增一行程式碼，若要啟用記錄之前初始化的連線，在檔案結尾處的程式碼：</span><span class="sxs-lookup"><span data-stu-id="14ba4-293">Open *StockTicker.js* and add a line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. <span data-ttu-id="14ba4-294">按 F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="14ba4-294">Press F5 to run the project.</span></span>
3. <span data-ttu-id="14ba4-295">開啟瀏覽器的開發人員工具 視窗，然後選取主控台，以查看記錄檔。</span><span class="sxs-lookup"><span data-stu-id="14ba4-295">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="14ba4-296">您可能必須重新整理頁面，以查看 Signalr 交涉新連線的傳輸方法的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="14ba4-296">You might have to refresh the page to see the logs of Signalr negotiating the transport method for a new connection.</span></span>

    <span data-ttu-id="14ba4-297">如果您在 Windows 8 (IIS 8) 上執行 Internet Explorer 10，傳輸方法是 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="14ba4-297">If you are running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![IE 10 IIS 8 主控台](tutorial-server-broadcast-with-signalr/_static/image9.png)

    <span data-ttu-id="14ba4-299">如果您在 Windows 7 (IIS 7.5) 上執行 Internet Explorer 10，傳輸方法是 iframe。</span><span class="sxs-lookup"><span data-stu-id="14ba4-299">If you are running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is iframe.</span></span>

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    <span data-ttu-id="14ba4-301">在 Firefox 中安裝 Firebug 增益集以取得主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="14ba4-301">In Firefox, install the Firebug add-in to get a Console window.</span></span> <span data-ttu-id="14ba4-302">如果您在 Windows 8 (IIS 8) 來執行 Firefox 19，傳輸方法是 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="14ba4-302">If you are running Firefox 19 on Windows 8 (IIS 8), the transport method is WebSockets.</span></span>

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    <span data-ttu-id="14ba4-304">如果您在 Windows 7 (IIS 7.5) 上執行 Firefox 19，傳輸方法就會是伺服器傳送事件。</span><span class="sxs-lookup"><span data-stu-id="14ba4-304">If you are running Firefox 19 on Windows 7 (IIS 7.5), the transport method is server-sent events.</span></span>

    ![Firefox 19 IIS 7.5 主控台](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a><span data-ttu-id="14ba4-306">安裝並檢閱完整的 Stockservices.asmx 範例</span><span class="sxs-lookup"><span data-stu-id="14ba4-306">Install and review the full StockTicker sample</span></span>

<span data-ttu-id="14ba4-307">Stockservices.asmx 應用程式會安裝[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet 套件包含您剛從頭開始建立簡化版本較多的功能。</span><span class="sxs-lookup"><span data-stu-id="14ba4-307">The StockTicker application that is installed by the [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet package includes more features than the simplified version that you just created from scratch.</span></span> <span data-ttu-id="14ba4-308">在本節的教學課程中，您要安裝 NuGet 套件，並檢閱新功能，以及實作它們的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14ba4-308">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span> <span data-ttu-id="14ba4-309">如果您不需要執行本教學課程的先前步驟中安裝套件，您必須新增 OWIN 啟動類別至您的專案。</span><span class="sxs-lookup"><span data-stu-id="14ba4-309">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="14ba4-310">此步驟會說明適用於 NuGet 套件的 readme.txt 檔案中。</span><span class="sxs-lookup"><span data-stu-id="14ba4-310">This step is explained in the readme.txt file for the NuGet package.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="14ba4-311">安裝 SignalR.Sample NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="14ba4-311">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="14ba4-312">在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-312">In **Solution Explorer**, right-click the project and click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="14ba4-313">在 **管理 NuGet 套件** 對話方塊中，按一下**線上**，輸入*SignalR.Sample*中**線上搜尋**方塊，然後再按一下  **安裝**中**SignalR.Sample**封裝。</span><span class="sxs-lookup"><span data-stu-id="14ba4-313">In the **Manage NuGet Packages** dialog box, click **Online**, enter *SignalR.Sample* in the **Search Online** box, and then click **Install** in the **SignalR.Sample** package.</span></span>

    ![安裝 SignalR.Sample 套件](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. <span data-ttu-id="14ba4-315">在 [**方案總管] 中**，展開*SignalR.Sample*安裝 SignalR.Sample 封裝所建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="14ba4-315">In **Solution Explorer**, expand the *SignalR.Sample* folder which was created by installing the SignalR.Sample package.</span></span>
4. <span data-ttu-id="14ba4-316">在  *SignalR.Sample*資料夾中，以滑鼠右鍵按一下*StockTicker.html*，然後按一下 **設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="14ba4-316">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then click **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="14ba4-317">安裝 SignalR.Sample NuGet 套件可能會變更您在的 jQuery 版本您*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="14ba4-317">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="14ba4-318">新*StockTicker.html*套件會安裝中的檔案*SignalR.Sample*資料夾將會是套件會安裝，但如果您想要執行您的原始的jQuery版本同步*StockTicker.html*一次檔案中，您可能必須先更新指令碼標記中的 jQuery 參考。</span><span class="sxs-lookup"><span data-stu-id="14ba4-318">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="14ba4-319">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="14ba4-319">Run the application</span></span>

1. <span data-ttu-id="14ba4-320">按 F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-320">Press F5 to run the application.</span></span>

    <span data-ttu-id="14ba4-321">方格中稍早所見，除了完整股票行情指示器應用程式會顯示可顯示相同的股票資料的水平捲動視窗。</span><span class="sxs-lookup"><span data-stu-id="14ba4-321">In addition to the grid that you saw earlier, the full stock ticker application shows a horizontally scrolling window that displays the same stock data.</span></span> <span data-ttu-id="14ba4-322">當您執行第一次應用程式時，「 市場 」 的動作 「 關閉 」，您會看到靜態方格和不捲動的股票行情指示器視窗。</span><span class="sxs-lookup"><span data-stu-id="14ba4-322">When you run the application for the first time, the "market" is "closed" and you see a static grid and a ticker window that isn't scrolling.</span></span>

    ![Stockservices.asmx 畫面開始](tutorial-server-broadcast-with-signalr/_static/image14.png)

    <span data-ttu-id="14ba4-324">當您按一下 **開放性市場**，則**即時股票行情即時看板**開始的水平捲動方塊，且伺服器會開始定期廣播隨機的方式上的股票價格變更。</span><span class="sxs-lookup"><span data-stu-id="14ba4-324">When you click **Open Market**, the **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span> <span data-ttu-id="14ba4-325">每次股票的價格變更，同時**即時股票表格**格線和**即時股票行情即時看板**方塊會更新。</span><span class="sxs-lookup"><span data-stu-id="14ba4-325">Each time a stock price changes, both the **Live Stock Table** grid and the **Live Stock Ticker** box are updated.</span></span> <span data-ttu-id="14ba4-326">具有綠色背景，正股票的價格變動時，會顯示股票和股票時變更為負數，會顯示具有紅色背景。</span><span class="sxs-lookup"><span data-stu-id="14ba4-326">When a stock's price change is positive, the stock is shown with a green background, and when the change is negative, the stock is shown with a red background.</span></span>

    ![開啟 Stockservices.asmx 的應用程式市集](tutorial-server-broadcast-with-signalr/_static/image15.png)

    <span data-ttu-id="14ba4-328">**關閉市場** 按鈕會停止所做的變更，並停止股票行情指示器捲動，而**重設**按鈕會重設內建的所有資料的初始狀態價格變動啟動之前。</span><span class="sxs-lookup"><span data-stu-id="14ba4-328">The **Close Market** button stops the changes and stops the ticker scrolling, and the **Reset** button resets all stock data to the initial state before price changes started.</span></span> <span data-ttu-id="14ba4-329">如果您開啟多個瀏覽器視窗，並移至相同的 URL，您會看到相同的資料，動態更新在此同時在每個瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="14ba4-329">If you open more browser windows and go to the same URL, you see the same data dynamically updated at the same time in each browser.</span></span> <span data-ttu-id="14ba4-330">當您按一下其中一個按鈕時，則所有瀏覽器會回應相同的方式在相同的時間。</span><span class="sxs-lookup"><span data-stu-id="14ba4-330">When you click one of the buttons, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="14ba4-331">即時股票行情指示器顯示</span><span class="sxs-lookup"><span data-stu-id="14ba4-331">Live Stock Ticker display</span></span>

<span data-ttu-id="14ba4-332">**即時股票行情即時看板**顯示會格式化為單一行的 CSS 樣式的 div 元素中的未排序的清單。</span><span class="sxs-lookup"><span data-stu-id="14ba4-332">The **Live Stock Ticker** display is an unordered list in a div element that is formatted into a single line by CSS styles.</span></span> <span data-ttu-id="14ba4-333">股票行情指示器會初始化並更新資料表的相同方式來： 取代中的預留位置&lt;li&gt;範本字串，並以動態方式新增&lt;li&gt;項目&lt;ul&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="14ba4-333">The ticker is initialized and updated the same way as the table: by replacing placeholders in a &lt;li&gt; template string and dynamically adding the &lt;li&gt; elements to the &lt;ul&gt; element.</span></span> <span data-ttu-id="14ba4-334">使用 jQuery 動畫函式來改變的未排序的清單內的 div。 margin-left 執行捲動</span><span class="sxs-lookup"><span data-stu-id="14ba4-334">The scrolling is performed by using the jQuery animate function to vary the margin-left of the unordered list within the div.</span></span>

<span data-ttu-id="14ba4-335">股票行情即時看板 HTML:</span><span class="sxs-lookup"><span data-stu-id="14ba4-335">The stock ticker HTML:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

<span data-ttu-id="14ba4-336">股票行情即時看板 CSS:</span><span class="sxs-lookup"><span data-stu-id="14ba4-336">The stock ticker CSS:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

<span data-ttu-id="14ba4-337">JQuery 程式碼，可讓捲動：</span><span class="sxs-lookup"><span data-stu-id="14ba4-337">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="14ba4-338">用戶端可以呼叫的伺服器上的其他方法</span><span class="sxs-lookup"><span data-stu-id="14ba4-338">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="14ba4-339">StockTickerHub 類別會定義四個用戶端可以呼叫其他方法：</span><span class="sxs-lookup"><span data-stu-id="14ba4-339">The StockTickerHub class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="14ba4-340">OpenMarket、 CloseMarket 和重設稱為回應位於頁面頂端的按鈕。</span><span class="sxs-lookup"><span data-stu-id="14ba4-340">OpenMarket, CloseMarket, and Reset are called in response to the buttons at the top of the page.</span></span> <span data-ttu-id="14ba4-341">它們會示範一個觸發會立即傳播到所有用戶端的狀態變更的用戶端的模式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-341">They demonstrate the pattern of one client triggering a change in state that is immediately propagated to all clients.</span></span> <span data-ttu-id="14ba4-342">每個方法呼叫的方法 Stockservices.asmx 類別中會影響市場狀態變更，然後再廣播的新狀態。</span><span class="sxs-lookup"><span data-stu-id="14ba4-342">Each of these methods calls a method in the StockTicker class that effects the market state change and then broadcasts the new state.</span></span>

<span data-ttu-id="14ba4-343">在 Stockservices.asmx 類別中，由傳回 MarketState 列舉值的 MarketState 屬性維護的市場狀況：</span><span class="sxs-lookup"><span data-stu-id="14ba4-343">In the StockTicker class, the state of the market is maintained by a MarketState property that returns a MarketState enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="14ba4-344">每個市場狀態變更的方法在內進行的鎖定區塊 Stockservices.asmx 類別必須為 threadsafe 因為：</span><span class="sxs-lookup"><span data-stu-id="14ba4-344">Each of the methods that change the market state do so inside a lock block because the StockTicker class has to be threadsafe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="14ba4-345">若要確保此程式碼 threadsafe， \_marketState 欄位支援 MarketState 屬性會標示為 volatile，</span><span class="sxs-lookup"><span data-stu-id="14ba4-345">To ensure that this code is threadsafe, the \_marketState field that backs the MarketState property is marked as volatile,</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="14ba4-346">BroadcastMarketStateChange 和 BroadcastMarketReset 方法都是類似於 BroadcastStockPrice 方法，您看到的但在呼叫不同的方法，在用戶端定義：</span><span class="sxs-lookup"><span data-stu-id="14ba4-346">The BroadcastMarketStateChange and BroadcastMarketReset methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="14ba4-347">用戶端可以呼叫伺服器上的其他函式</span><span class="sxs-lookup"><span data-stu-id="14ba4-347">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="14ba4-348">方格與顯示的 ticker，現在會處理 updateStockPrice 函式，並使用 jQuery.Color 閃爍紅色和綠色的色彩。</span><span class="sxs-lookup"><span data-stu-id="14ba4-348">The updateStockPrice function now handles both the grid and the ticker display, and it uses jQuery.Color to flash red and green colors.</span></span>

<span data-ttu-id="14ba4-349">中的新函式*SignalR.StockTicker.js*啟用和停用按鈕根據市場的狀態，和它們停止或啟動股票行情指示器視窗水平捲動。</span><span class="sxs-lookup"><span data-stu-id="14ba4-349">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state, and they stop or start the ticker window horizontal scrolling.</span></span> <span data-ttu-id="14ba4-350">因為 ticker.client，將會新增多個函式[jQuery 擴充函式](http://api.jquery.com/jQuery.extend/)用來新增它們。</span><span class="sxs-lookup"><span data-stu-id="14ba4-350">Since multiple functions are being added to ticker.client, the [jQuery extend function](http://api.jquery.com/jQuery.extend/) is used to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="14ba4-351">建立連線後的其他用戶端安裝程式</span><span class="sxs-lookup"><span data-stu-id="14ba4-351">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="14ba4-352">用戶端建立連接之後，它會有一些額外的工作，執行： 找出市場上是否已開啟或關閉才能呼叫適當的 marketOpened 或 marketClosed 函式，並附加至按鈕的伺服器方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="14ba4-352">After the client establishes the connection, it has some additional work to do: find out if the market is open or closed in order to call the appropriate marketOpened or marketClosed function, and attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="14ba4-353">伺服器方法不是連線到之前的按鈕之後建立的連線，這樣的程式碼無法嘗試呼叫伺服器方法，才能使用。</span><span class="sxs-lookup"><span data-stu-id="14ba4-353">The server methods are not wired up to the buttons until after the connection is established, so that the code can't try to call the server methods before they are available.</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="14ba4-354">後續步驟</span><span class="sxs-lookup"><span data-stu-id="14ba4-354">Next steps</span></span>

<span data-ttu-id="14ba4-355">在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將訊息從伺服器廣播給所有已連線的用戶端，定期執行並以通知從任何用戶端的回應方式。</span><span class="sxs-lookup"><span data-stu-id="14ba4-355">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients, both on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="14ba4-356">使用多執行緒的單一執行個體維護伺服器狀態的模式也也可用在多玩家線上遊戲案例中。</span><span class="sxs-lookup"><span data-stu-id="14ba4-356">The pattern of using a multi-threaded singleton instance to maintain server state can also be also used in multi-player online game scenarios.</span></span> <span data-ttu-id="14ba4-357">如需範例，請參閱[ShootR 遊戲為基礎的 SignalR](https://github.com/NTaylorMullen/ShootR)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-357">For an example, see [the ShootR game that is based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="14ba4-358">顯示對等通訊案例的教學課程，請參閱[開始使用 SignalR](introduction-to-signalr.md)並[即時更新與 SignalR](tutorial-high-frequency-realtime-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-358">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="14ba4-359">若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：</span><span class="sxs-lookup"><span data-stu-id="14ba4-359">To learn more advanced SignalR development concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="14ba4-360">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="14ba4-360">ASP.NET SignalR</span></span>](../../index.md)
- [<span data-ttu-id="14ba4-361">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="14ba4-361">SignalR Project</span></span>](http://signalr.net/)
- [<span data-ttu-id="14ba4-362">SignalR Github 和範例</span><span class="sxs-lookup"><span data-stu-id="14ba4-362">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="14ba4-363">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="14ba4-363">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

<span data-ttu-id="14ba4-364">如需如何部署至 Azure 的 SignalR 應用程式的逐步解說，請參閱 <<c0> [ 使用 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-364">For a walkthrough on how to deploy a SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="14ba4-365">如需如何將 Visual Studio web 專案部署至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。</span><span class="sxs-lookup"><span data-stu-id="14ba4-365">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
