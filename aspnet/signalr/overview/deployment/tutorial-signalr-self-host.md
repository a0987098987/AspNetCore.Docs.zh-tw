---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: "教學課程： 自我裝載 SignalR |Microsoft 文件"
author: pfletcher
description: "本教學課程會示範如何建立自我裝載的 SignalR 2 伺服器，以及如何使用 JavaScript 用戶端連線到它。 教學課程 V 中使用的軟體版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 997756ff8d48e41da981491d6154f3107ec7a051
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="8258f-104">自我裝載的教學課程： SignalR</span><span class="sxs-lookup"><span data-stu-id="8258f-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="8258f-105">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8258f-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="8258f-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="8258f-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="8258f-107">本教學課程會示範如何建立自我裝載的 SignalR 2 伺服器，以及如何使用 JavaScript 用戶端連線到它。</span><span class="sxs-lookup"><span data-stu-id="8258f-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8258f-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="8258f-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="8258f-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8258f-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8258f-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8258f-110">.NET 4.5</span></span>
> - <span data-ttu-id="8258f-111">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="8258f-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="8258f-112">使用 Visual Studio 2012 進行這個教學課程</span><span class="sxs-lookup"><span data-stu-id="8258f-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="8258f-113">透過本教學課程中使用 Visual Studio 2012，請執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="8258f-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="8258f-114">更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。</span><span class="sxs-lookup"><span data-stu-id="8258f-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="8258f-115">安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8258f-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="8258f-116">在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。</span><span class="sxs-lookup"><span data-stu-id="8258f-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="8258f-117">這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。</span><span class="sxs-lookup"><span data-stu-id="8258f-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="8258f-118">某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。</span><span class="sxs-lookup"><span data-stu-id="8258f-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="8258f-119">問題和註解</span><span class="sxs-lookup"><span data-stu-id="8258f-119">Questions and comments</span></span>
> 
> <span data-ttu-id="8258f-120">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="8258f-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8258f-121">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="8258f-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="8258f-122">概觀</span><span class="sxs-lookup"><span data-stu-id="8258f-122">Overview</span></span>

<span data-ttu-id="8258f-123">SignalR 伺服器通常裝載於 IIS，ASP.NET 應用程式中，但它也可以是自我裝載 （例如，在主控台應用程式或 Windows 服務） 使用的自我裝載的程式庫。</span><span class="sxs-lookup"><span data-stu-id="8258f-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="8258f-124">此程式庫，如同所有 SignalR 2 根據 OWIN ([開啟適用於.NET 的 Web 介面](http://owin.org))。</span><span class="sxs-lookup"><span data-stu-id="8258f-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="8258f-125">OWIN 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="8258f-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8258f-126">OWIN 以減少從伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8258f-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="8258f-127">未在 IIS 中裝載的原因包括：</span><span class="sxs-lookup"><span data-stu-id="8258f-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="8258f-128">其中 IIS 未提供或需要這樣做，例如 IIS 沒有現有的伺服器陣列環境。</span><span class="sxs-lookup"><span data-stu-id="8258f-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="8258f-129">若要避免需要 IIS 的效能負荷。</span><span class="sxs-lookup"><span data-stu-id="8258f-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="8258f-130">SignalR 的功能是要加入至執行 Windows 服務、 Azure 背景工作角色或其他處理程序中的 exising 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8258f-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="8258f-131">如果您正在開發方案為自我裝載，基於效能考量，建議您也測試應用程式裝載於 IIS 來判斷效能優勢。</span><span class="sxs-lookup"><span data-stu-id="8258f-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="8258f-132">本教學課程包含下列各節：</span><span class="sxs-lookup"><span data-stu-id="8258f-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="8258f-133">建立伺服器</span><span class="sxs-lookup"><span data-stu-id="8258f-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="8258f-134">存取 JavaScript 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="8258f-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="8258f-135">建立伺服器</span><span class="sxs-lookup"><span data-stu-id="8258f-135">Creating the server</span></span>

<span data-ttu-id="8258f-136">在本教學課程中，您會在主控台應用程式，建立裝載的伺服器，但是伺服器可以裝載於任何類型的處理程序，例如 Windows 服務或 Azure 背景工作角色。</span><span class="sxs-lookup"><span data-stu-id="8258f-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="8258f-137">裝載 Windows 服務中的 SignalR 伺服器的範例程式碼，請參閱[Self-Hosting Windows 服務中的 SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)。</span><span class="sxs-lookup"><span data-stu-id="8258f-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="8258f-138">系統管理員權限開啟 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="8258f-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="8258f-139">選取**檔案**，**新專案**。</span><span class="sxs-lookup"><span data-stu-id="8258f-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="8258f-140">選取**Windows**下**Visual C#**節點**範本**窗格，然後選取**主控台應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="8258f-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="8258f-141">將新專案命名為"SignalRSelfHost 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="8258f-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="8258f-142">選取以開啟 [程式庫封裝管理員] 主控台**工具**，**程式庫套件管理員**， **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="8258f-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="8258f-143">在 [封裝管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="8258f-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="8258f-144">此命令會將 SignalR 2 Self-Host 程式庫加入至專案。</span><span class="sxs-lookup"><span data-stu-id="8258f-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="8258f-145">在 [封裝管理員] 主控台中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="8258f-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="8258f-146">此命令會將 Microsoft.Owin.Cors 程式庫加入至專案。</span><span class="sxs-lookup"><span data-stu-id="8258f-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="8258f-147">此程式庫將用於裝載 SignalR 和不同網域中的網頁用戶端應用程式需要的跨網域支援。</span><span class="sxs-lookup"><span data-stu-id="8258f-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="8258f-148">因為您將會裝載 SignalR 伺服器和 web 上的用戶端不同的通訊埠，這表示該跨網域必須啟用這些元件間的通訊。</span><span class="sxs-lookup"><span data-stu-id="8258f-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="8258f-149">以下列程式碼取代 Program.cs 的內容。</span><span class="sxs-lookup"><span data-stu-id="8258f-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="8258f-150">上述程式碼包含三個類別：</span><span class="sxs-lookup"><span data-stu-id="8258f-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="8258f-151">**程式**，包括**Main**方法定義執行主要路徑。</span><span class="sxs-lookup"><span data-stu-id="8258f-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="8258f-152">在此方法中，類型的 web 應用程式**啟動**開始於指定的 URL (`http://localhost:8080`)。</span><span class="sxs-lookup"><span data-stu-id="8258f-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="8258f-153">如果端點需要安全性，您可以實作 SSL。</span><span class="sxs-lookup"><span data-stu-id="8258f-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="8258f-154">請參閱[How to： 使用 SSL 憑證設定連接埠](https://msdn.microsoft.com/en-us/library/ms733791.aspx)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8258f-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/en-us/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="8258f-155">**啟動**、 包含 SignalR 伺服器設定的類別 (本教學課程使用的唯一設定是呼叫`UseCors`)，且呼叫`MapSignalR`，其中的專案中建立中樞的任何物件的路由。</span><span class="sxs-lookup"><span data-stu-id="8258f-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="8258f-156">**MyHub**，應用程式將提供給用戶端的 SignalR 中樞類別。</span><span class="sxs-lookup"><span data-stu-id="8258f-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="8258f-157">這個類別具有單一方法**傳送**，用戶端將呼叫廣播給所有其他連接的用戶端的訊息。</span><span class="sxs-lookup"><span data-stu-id="8258f-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="8258f-158">編譯並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8258f-158">Compile and run the application.</span></span> <span data-ttu-id="8258f-159">主控台視窗中應該會顯示該伺服器正在執行的位址。</span><span class="sxs-lookup"><span data-stu-id="8258f-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="8258f-160">如果執行作業失敗，發生例外狀況`System.Reflection.TargetInvocationException was unhandled`，您必須重新啟動 Visual Studio 系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="8258f-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="8258f-161">停止應用程式後再繼續下一節。</span><span class="sxs-lookup"><span data-stu-id="8258f-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="8258f-162">存取 JavaScript 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="8258f-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="8258f-163">在本節中，您將使用相同的 JavaScript 用戶端從[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="8258f-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="8258f-164">我們只會提供給用戶端，也就是明確地定義中樞 URL 的一項修改。</span><span class="sxs-lookup"><span data-stu-id="8258f-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="8258f-165">自我裝載的應用程式中，伺服器可能一定無法在相同的位址做為連接 URL （由於反向 proxy 和負載平衡器），因此 URL 必須以明確定義。</span><span class="sxs-lookup"><span data-stu-id="8258f-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="8258f-166">在**方案總管 中**，以滑鼠右鍵按一下方案，然後選取**新增**，**新專案**。</span><span class="sxs-lookup"><span data-stu-id="8258f-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="8258f-167">選取**Web**節點，然後選取**ASP.NET Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="8258f-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="8258f-168">將專案命名為"JavascriptClient 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="8258f-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="8258f-169">選取**空**範本，並保持未選取其餘的選項。</span><span class="sxs-lookup"><span data-stu-id="8258f-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="8258f-170">選取**建立專案**。</span><span class="sxs-lookup"><span data-stu-id="8258f-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="8258f-171">在 [封裝管理員] 主控台中，選取"JavascriptClient 」 專案中的**預設專案**下拉式清單中，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8258f-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="8258f-172">此命令會在用戶端安裝您將需要的 SignalR 和 JQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="8258f-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="8258f-173">以滑鼠右鍵按一下您的專案並選取**新增**，**新項目**。</span><span class="sxs-lookup"><span data-stu-id="8258f-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="8258f-174">選取**Web**節點，然後選取 HTML 頁面。</span><span class="sxs-lookup"><span data-stu-id="8258f-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="8258f-175">將頁面命名**Default.html**。</span><span class="sxs-lookup"><span data-stu-id="8258f-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="8258f-176">新的 HTML 網頁的內容取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="8258f-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="8258f-177">請確認指令碼參考符合專案的 [指令碼] 資料夾中的指令碼。</span><span class="sxs-lookup"><span data-stu-id="8258f-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="8258f-178">下列程式碼 （上述的範例程式碼中反白顯示） 是新增您已對 Stared 快速入門教學課程 （除了程式碼升級至 SignalR 第 2 版 beta） 中使用的用戶端。</span><span class="sxs-lookup"><span data-stu-id="8258f-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="8258f-179">這行程式碼明確地設定基底連接 URL SignalR 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="8258f-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="8258f-180">以滑鼠右鍵按一下方案，然後選取**設定啟始專案...**.選取**多個啟始專案**選項按鈕，然後設定兩個專案的**動作**至**啟動**。</span><span class="sxs-lookup"><span data-stu-id="8258f-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="8258f-181">以滑鼠右鍵按一下 「 Default.html"，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="8258f-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="8258f-182">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="8258f-182">Run the application.</span></span> <span data-ttu-id="8258f-183">將會啟動伺服器和頁面。</span><span class="sxs-lookup"><span data-stu-id="8258f-183">The server and page will launch.</span></span> <span data-ttu-id="8258f-184">您可能需要重新載入網頁 (或選取**繼續**偵錯工具中) 如果啟動伺服器之前，載入頁面。</span><span class="sxs-lookup"><span data-stu-id="8258f-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="8258f-185">在瀏覽器中提供的使用者名稱出現提示時。</span><span class="sxs-lookup"><span data-stu-id="8258f-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="8258f-186">網頁的 URL 複製到另一個瀏覽器索引標籤或視窗，並提供不同的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="8258f-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="8258f-187">您可以從一種瀏覽器窗格中傳送訊息至另一個，如同入門 」 教學課程。</span><span class="sxs-lookup"><span data-stu-id="8258f-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
