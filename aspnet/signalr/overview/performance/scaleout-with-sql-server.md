---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SignalR 範圍外的 SQL Server |Microsoft 文件
author: MikeWasson
description: 軟體版本本主題中使用 Visual Studio 2013.NET 4.5 SignalR 第 2 版舊版的此主題的較早版本的相關資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874365"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="edf76-103">與 SQL Server 的 SignalR 範圍外</span><span class="sxs-lookup"><span data-stu-id="edf76-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="edf76-104">由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="edf76-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="edf76-105">本主題中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="edf76-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="edf76-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="edf76-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="edf76-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="edf76-107">.NET 4.5</span></span>
> - <span data-ttu-id="edf76-108">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="edf76-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="edf76-109">本主題的先前版本</span><span class="sxs-lookup"><span data-stu-id="edf76-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="edf76-110">如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="edf76-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="edf76-111">問題和註解</span><span class="sxs-lookup"><span data-stu-id="edf76-111">Questions and comments</span></span>
> 
> <span data-ttu-id="edf76-112">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="edf76-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="edf76-113">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="edf76-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="edf76-114">在本教學課程中，您將使用 SQL Server 可以分散部署兩個不同的 IIS 執行個體中的 SignalR 應用程式中的訊息。</span><span class="sxs-lookup"><span data-stu-id="edf76-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="edf76-115">您也可以在單一測試電腦上，執行此教學課程中，但若要取得完整的效果，您需要部署兩個或多部伺服器的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="edf76-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="edf76-116">您也必須在一部伺服器上或不同的專用伺服器上安裝 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="edf76-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="edf76-117">另一個選項是執行本教學課程使用 Azure 上的 Vm。</span><span class="sxs-lookup"><span data-stu-id="edf76-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="edf76-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="edf76-118">Prerequisites</span></span>

<span data-ttu-id="edf76-119">Microsoft SQL Server 2005 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="edf76-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="edf76-120">後擋板支援桌上型電腦和伺服器版本的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="edf76-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="edf76-121">它不支援 SQL Server Compact Edition 或 Azure SQL Database。</span><span class="sxs-lookup"><span data-stu-id="edf76-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="edf76-122">（如果您的應用程式裝載於 Azure 時，請考慮服務匯流排後擋板，已改用。）</span><span class="sxs-lookup"><span data-stu-id="edf76-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="edf76-123">總覽</span><span class="sxs-lookup"><span data-stu-id="edf76-123">Overview</span></span>

<span data-ttu-id="edf76-124">我們可以詳細的教學課程之前，以下是您將執行的快速概觀。</span><span class="sxs-lookup"><span data-stu-id="edf76-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="edf76-125">建立新的空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="edf76-125">Create a new empty database.</span></span> <span data-ttu-id="edf76-126">後擋板，已將此資料庫中建立所需的資料表。</span><span class="sxs-lookup"><span data-stu-id="edf76-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="edf76-127">將這些 NuGet 封裝加入至您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="edf76-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="edf76-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="edf76-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="edf76-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="edf76-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="edf76-130">建立 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="edf76-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="edf76-131">設定後擋板 Startup.cs 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="edf76-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="edf76-132">此程式碼使用的預設值，來設定後擋板[TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx)和[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)。</span><span class="sxs-lookup"><span data-stu-id="edf76-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="edf76-133">如需變更這些值的資訊，請參閱[SignalR 效能： 向外延展度量](signalr-performance.md#scaleout_metrics)。</span><span class="sxs-lookup"><span data-stu-id="edf76-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="edf76-134">設定資料庫</span><span class="sxs-lookup"><span data-stu-id="edf76-134">Configure the Database</span></span>

<span data-ttu-id="edf76-135">決定應用程式會使用 Windows 驗證或 SQL Server 驗證來存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="edf76-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="edf76-136">不論如何，請確定資料庫使用者具有登入，建立結構描述，然後建立資料表的權限。</span><span class="sxs-lookup"><span data-stu-id="edf76-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="edf76-137">建立後擋板，已使用新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="edf76-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="edf76-138">您可以提供該資料庫的任何名稱。</span><span class="sxs-lookup"><span data-stu-id="edf76-138">You can give the database any name.</span></span> <span data-ttu-id="edf76-139">您不需要在資料庫中建立的任何資料表後擋板，已將會建立所需的資料表。</span><span class="sxs-lookup"><span data-stu-id="edf76-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="edf76-140">啟用 Service Broker</span><span class="sxs-lookup"><span data-stu-id="edf76-140">Enable Service Broker</span></span>

<span data-ttu-id="edf76-141">建議您啟用 Service Broker 的後擋板資料庫。</span><span class="sxs-lookup"><span data-stu-id="edf76-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="edf76-142">Service Broker 提供傳訊和佇列在 SQL Server，可讓更有效率地收到更新後擋板的原生支援。</span><span class="sxs-lookup"><span data-stu-id="edf76-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="edf76-143">（不過後, 擋板也適用於沒有 Service Broker。）</span><span class="sxs-lookup"><span data-stu-id="edf76-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="edf76-144">若要檢查是否啟用 Service Broker，查詢**是\_broker\_啟用**中的資料行**sys.databases**目錄檢視。</span><span class="sxs-lookup"><span data-stu-id="edf76-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="edf76-145">若要啟用 Service Broker，請使用下列 SQL 查詢：</span><span class="sxs-lookup"><span data-stu-id="edf76-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="edf76-146">如果此查詢似乎發生死結，請確定沒有應用程式連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="edf76-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="edf76-147">如果您已啟用追蹤，追蹤也會顯示是否已啟用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="edf76-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="edf76-148">建立 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="edf76-148">Create a SignalR Application</span></span>

<span data-ttu-id="edf76-149">建立 SignalR 應用程式的其中一個這些教學課程：</span><span class="sxs-lookup"><span data-stu-id="edf76-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="edf76-150">SignalR 2.0 使用者入門</span><span class="sxs-lookup"><span data-stu-id="edf76-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="edf76-151">開始使用 SignalR 2.0 和 MVC 5</span><span class="sxs-lookup"><span data-stu-id="edf76-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="edf76-152">接下來，我們將修改交談應用程式支援使用 SQL Server 範圍外。</span><span class="sxs-lookup"><span data-stu-id="edf76-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="edf76-153">首先，SignalR.SqlServer NuGet 封裝加入您的專案。</span><span class="sxs-lookup"><span data-stu-id="edf76-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="edf76-154">在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="edf76-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="edf76-155">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="edf76-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="edf76-156">接下來，開啟 Startup.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="edf76-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="edf76-157">將下列程式碼加入**設定**方法：</span><span class="sxs-lookup"><span data-stu-id="edf76-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="edf76-158">部署和執行應用程式</span><span class="sxs-lookup"><span data-stu-id="edf76-158">Deploy and Run the Application</span></span>

<span data-ttu-id="edf76-159">準備部署 SignalR 應用程式的 Windows 伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="edf76-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="edf76-160">新增 IIS 角色。</span><span class="sxs-lookup"><span data-stu-id="edf76-160">Add the IIS role.</span></span> <span data-ttu-id="edf76-161">包含 「 應用程式開發 」 功能，包括 WebSocket 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="edf76-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="edf76-162">也包含管理服務 （列在 [管理工具]）。</span><span class="sxs-lookup"><span data-stu-id="edf76-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="edf76-163">**安裝 Web Deploy 3.0。**</span><span class="sxs-lookup"><span data-stu-id="edf76-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="edf76-164">當執行 IIS 管理員，就會提示您安裝 Microsoft Web Platform，或是您可以[下載 intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)。</span><span class="sxs-lookup"><span data-stu-id="edf76-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="edf76-165">在 Platform Installer 中搜尋 Web deploy 及安裝 Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="edf76-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="edf76-166">檢查 Web 管理服務正在執行。</span><span class="sxs-lookup"><span data-stu-id="edf76-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="edf76-167">否則，請啟動服務。</span><span class="sxs-lookup"><span data-stu-id="edf76-167">If not, start the service.</span></span> <span data-ttu-id="edf76-168">（如果您沒有看到 Web 管理服務中 Windows 服務的清單，請確定您加入的 IIS 角色時，已安裝管理服務。）</span><span class="sxs-lookup"><span data-stu-id="edf76-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="edf76-169">最後，開啟 TCP 連接埠 8172。</span><span class="sxs-lookup"><span data-stu-id="edf76-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="edf76-170">這是 Web Deploy 工具使用的連接埠。</span><span class="sxs-lookup"><span data-stu-id="edf76-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="edf76-171">現在您已準備好部署從開發電腦的 Visual Studio 專案到伺服器。</span><span class="sxs-lookup"><span data-stu-id="edf76-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="edf76-172">在 [方案總管] 中，以滑鼠右鍵按一下方案，然後按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="edf76-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="edf76-173">如需詳細文件中的有關 web 部署，請參閱[Visual Studio 和 ASP.NET 的 Web 部署內容地圖](../../../whitepapers/aspnet-web-deployment-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="edf76-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="edf76-174">如果您部署至兩個伺服器應用程式，您可以在另一個瀏覽器視窗開啟每個執行個體，並查看每個不同的方式接收 SignalR 訊息。</span><span class="sxs-lookup"><span data-stu-id="edf76-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="edf76-175">（當然，在實際執行環境中，兩部伺服器會位在負載平衡器後方。）</span><span class="sxs-lookup"><span data-stu-id="edf76-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="edf76-176">執行應用程式之後，您可以看到 SignalR，已自動建立的資料表在資料庫中：</span><span class="sxs-lookup"><span data-stu-id="edf76-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="edf76-177">SignalR 管理資料表。</span><span class="sxs-lookup"><span data-stu-id="edf76-177">SignalR manages the tables.</span></span> <span data-ttu-id="edf76-178">只要您的應用程式部署時，不刪除資料列、 修改資料表，等等。</span><span class="sxs-lookup"><span data-stu-id="edf76-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
