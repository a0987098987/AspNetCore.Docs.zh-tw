---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 教學課程： SignalR 自我裝載 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立自我裝載的 SignalR 2 伺服器，以及如何使用 JavaScript 用戶端連線到它。 教學課程 V 中使用的軟體版本...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 71fb121377a49bb741ebff098ff20ec82e85c82a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821780"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="05926-104">教學課程： SignalR 自我裝載</span><span class="sxs-lookup"><span data-stu-id="05926-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="05926-105">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="05926-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="05926-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="05926-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="05926-107">本教學課程會示範如何建立自我裝載的 SignalR 2 伺服器，以及如何使用 JavaScript 用戶端連線到它。</span><span class="sxs-lookup"><span data-stu-id="05926-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="05926-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="05926-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="05926-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="05926-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="05926-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="05926-110">.NET 4.5</span></span>
> - <span data-ttu-id="05926-111">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="05926-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="05926-112">本教學課程中使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="05926-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="05926-113">若要使用 Visual Studio 2012，本教學課程中，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="05926-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="05926-114">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。</span><span class="sxs-lookup"><span data-stu-id="05926-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="05926-115">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="05926-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="05926-116">在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="05926-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="05926-117">這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="05926-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="05926-118">有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="05926-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="05926-119">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="05926-119">Questions and comments</span></span>
> 
> <span data-ttu-id="05926-120">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="05926-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="05926-121">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="05926-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="05926-122">總覽</span><span class="sxs-lookup"><span data-stu-id="05926-122">Overview</span></span>

<span data-ttu-id="05926-123">SignalR 伺服器通常裝載在 IIS 中，ASP.NET 應用程式中，但它也可以是自我裝載 （例如主控台應用程式或 Windows 服務） 使用的自我裝載的程式庫。</span><span class="sxs-lookup"><span data-stu-id="05926-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="05926-124">此程式庫，例如 SignalR 2 的所有以 OWIN 為基礎 ([Open Web Interface for.NET](http://owin.org))。</span><span class="sxs-lookup"><span data-stu-id="05926-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="05926-125">OWIN 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="05926-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="05926-126">OWIN 可分隔的伺服器，讓 OWIN 很適合用於自我裝載的 web 應用程式，在您自己的程序，IIS 外部的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="05926-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="05926-127">不在 IIS 中裝載的原因包括：</span><span class="sxs-lookup"><span data-stu-id="05926-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="05926-128">其中 IIS 不提供或需要的資源，例如現有的伺服器陣列，而不使用 IIS 的環境。</span><span class="sxs-lookup"><span data-stu-id="05926-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="05926-129">若要避免需要 IIS 的效能負荷。</span><span class="sxs-lookup"><span data-stu-id="05926-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="05926-130">SignalR 功能是可以加入至執行 Windows 服務、 Azure 背景工作角色或其他處理序中的現有應用程式。</span><span class="sxs-lookup"><span data-stu-id="05926-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="05926-131">如果正在開發的解決方案中做為自我裝載基於效能考量，建議也是測試應用程式裝載在 IIS 中決定的效能優勢。</span><span class="sxs-lookup"><span data-stu-id="05926-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="05926-132">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="05926-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="05926-133">建立伺服器</span><span class="sxs-lookup"><span data-stu-id="05926-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="05926-134">存取 JavaScript 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="05926-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="05926-135">建立伺服器</span><span class="sxs-lookup"><span data-stu-id="05926-135">Creating the server</span></span>

<span data-ttu-id="05926-136">在本教學課程中，您會在主控台應用程式，來建立裝載的伺服器，但是伺服器可以裝載在任何一種處理程序，例如 Windows 服務或 Azure 背景工作角色。</span><span class="sxs-lookup"><span data-stu-id="05926-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="05926-137">裝載 Windows 服務中的 SignalR 伺服器的範例程式碼，請參閱[Self-Hosting Windows 服務中的 SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)。</span><span class="sxs-lookup"><span data-stu-id="05926-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="05926-138">系統管理員權限開啟 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="05926-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="05926-139">選取 **檔案**，**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="05926-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="05926-140">選取  **Windows**下方**Visual C#** 節點中的**範本**窗格，然後選取**主控台應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="05926-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="05926-141">將新專案命名為"SignalRSelfHost 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="05926-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="05926-142">開啟選取的程式庫套件管理員主控台**工具**，**程式庫套件管理員**， **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="05926-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="05926-143">在 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="05926-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="05926-144">此命令會將專案中的 SignalR 2 自我裝載的程式庫。</span><span class="sxs-lookup"><span data-stu-id="05926-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="05926-145">在 [套件管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="05926-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="05926-146">此命令會將 Microsoft.Owin.Cors 程式庫加入至專案。</span><span class="sxs-lookup"><span data-stu-id="05926-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="05926-147">此程式庫會用於跨網域支援，所需的裝載 SignalR 和不同網域中的網頁用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="05926-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="05926-148">由於 SignalR 伺服器和 web 用戶端上不同的連接埠，您會在裝載，這表示該跨網域必須啟用這些元件之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="05926-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="05926-149">以下列程式碼取代 Program.cs 的內容。</span><span class="sxs-lookup"><span data-stu-id="05926-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="05926-150">上述程式碼包含三個類別：</span><span class="sxs-lookup"><span data-stu-id="05926-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="05926-151">**計劃**，包括**Main**定義主要執行路徑的方法。</span><span class="sxs-lookup"><span data-stu-id="05926-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="05926-152">在此方法中，類型的 web 應用程式**啟始**會在指定的 URL 啟動 (`http://localhost:8080`)。</span><span class="sxs-lookup"><span data-stu-id="05926-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="05926-153">如果端點需要安全性，就可以實作 SSL。</span><span class="sxs-lookup"><span data-stu-id="05926-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="05926-154">請參閱[如何： 使用 SSL 憑證設定連接埠](https://msdn.microsoft.com/library/ms733791.aspx)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="05926-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="05926-155">**啟始**、 包含 SignalR 伺服器組態的類別 (本教學課程使用的唯一設定是在呼叫`UseCors`)，和呼叫`MapSignalR`，專案中建立路由的中樞中的任何物件。</span><span class="sxs-lookup"><span data-stu-id="05926-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="05926-156">**MyHub**，應用程式會提供給用戶端的 SignalR Hub 類別。</span><span class="sxs-lookup"><span data-stu-id="05926-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="05926-157">這個類別具有單一方法**傳送**，用戶端會呼叫以將訊息廣播到所有其他連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="05926-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="05926-158">編譯並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="05926-158">Compile and run the application.</span></span> <span data-ttu-id="05926-159">伺服器正在執行的地址應該會顯示在主控台視窗中。</span><span class="sxs-lookup"><span data-stu-id="05926-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="05926-160">如果執行失敗，發生例外狀況`System.Reflection.TargetInvocationException was unhandled`，您必須重新啟動 Visual Studio 系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="05926-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="05926-161">將應用程式停止後再繼續下一節。</span><span class="sxs-lookup"><span data-stu-id="05926-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="05926-162">存取 JavaScript 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="05926-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="05926-163">在本節中，您將使用相同的 JavaScript 用戶端，從[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="05926-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="05926-164">我們只進行一項修改用戶端，也就是明確地定義中樞的 URL。</span><span class="sxs-lookup"><span data-stu-id="05926-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="05926-165">自我裝載的應用程式中，伺服器不一定可能在相同的位址，做為連接 URL （因為反向 proxy 和負載平衡器），因此 URL 必須明確定義。</span><span class="sxs-lookup"><span data-stu-id="05926-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="05926-166">在 [**方案總管] 中**，以滑鼠右鍵按一下方案，然後選取**新增**，**新專案**。</span><span class="sxs-lookup"><span data-stu-id="05926-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="05926-167">選取  **Web**節點，然後選取**ASP.NET Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="05926-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="05926-168">將專案命名為"JavascriptClient 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="05926-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="05926-169">選取 **空**範本，並保留其餘選項取消選取。</span><span class="sxs-lookup"><span data-stu-id="05926-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="05926-170">選取 **建立專案**。</span><span class="sxs-lookup"><span data-stu-id="05926-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="05926-171">在 套件管理員 主控台中，選取 中的"JavascriptClient 」 專案**預設專案**下拉式清單中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="05926-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="05926-172">此命令會在用戶端安裝所需的 SignalR 和 JQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="05926-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="05926-173">以滑鼠右鍵按一下專案，然後選取**新增**，**新項目**。</span><span class="sxs-lookup"><span data-stu-id="05926-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="05926-174">選取  **Web**節點，然後選取 HTML 頁面。</span><span class="sxs-lookup"><span data-stu-id="05926-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="05926-175">將頁面命名**Default.html**。</span><span class="sxs-lookup"><span data-stu-id="05926-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="05926-176">新的 HTML 網頁的內容取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="05926-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="05926-177">請確認指令碼參考符合專案的 [指令碼] 資料夾中的指令碼。</span><span class="sxs-lookup"><span data-stu-id="05926-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="05926-178">下列程式碼 （在上述程式碼範例中反白顯示） 是您已對用戶端 （除了 SignalR 第 2 版 beta 升級程式碼） 取得厭煩教學課程中使用。</span><span class="sxs-lookup"><span data-stu-id="05926-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="05926-179">這行程式碼明確地設定基底連接 URL SignalR 的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="05926-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="05926-180">以滑鼠右鍵按一下方案，然後選取**設定啟始專案...**.選取 **多個啟始專案**選項按鈕，然後設定兩個專案**動作**來**啟動**。</span><span class="sxs-lookup"><span data-stu-id="05926-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="05926-181">以滑鼠右鍵按一下 「 Default.html"，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="05926-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="05926-182">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="05926-182">Run the application.</span></span> <span data-ttu-id="05926-183">將會啟動伺服器和頁面。</span><span class="sxs-lookup"><span data-stu-id="05926-183">The server and page will launch.</span></span> <span data-ttu-id="05926-184">您可能需要重新載入網頁 (或選取**繼續**偵錯工具中) 如果頁面載入之後才啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="05926-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="05926-185">在瀏覽器中，提供使用者名稱出現提示時。</span><span class="sxs-lookup"><span data-stu-id="05926-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="05926-186">在頁面的 URL 複製到另一個瀏覽器索引標籤或視窗，並提供不同的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="05926-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="05926-187">您將能夠從一種瀏覽器窗格中將訊息傳送到另一個，如本快速入門教學課程所示。</span><span class="sxs-lookup"><span data-stu-id="05926-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
