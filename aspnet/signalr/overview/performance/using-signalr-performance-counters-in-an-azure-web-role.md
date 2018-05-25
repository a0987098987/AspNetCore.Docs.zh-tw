---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: 使用 Azure Web 角色中的 SignalR 效能計數器 |Microsoft 文件
author: guardrex
description: 如何安裝及使用 Azure Web 角色中的 SignalR 效能計數器。
keywords: ASP.NET,signalr,performance 計數器，azure web 角色
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2018
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="f6192-104">使用 Azure Web 角色中的 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="f6192-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="f6192-105">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f6192-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f6192-106">SignalR 效能計數器可用來監視 Azure Web 角色中的應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="f6192-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="f6192-107">計數器是由 Microsoft Azure 診斷擷取。</span><span class="sxs-lookup"><span data-stu-id="f6192-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="f6192-108">與在 Azure 上安裝 SignalR 效能計數器*signalr.exe*，獨立或內部部署應用程式所用的相同工具。</span><span class="sxs-lookup"><span data-stu-id="f6192-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="f6192-109">Azure 角色是暫時性的因為您會設定應用程式安裝並註冊在啟動時的 SignalR 效能計數器。</span><span class="sxs-lookup"><span data-stu-id="f6192-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6192-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="f6192-110">Prerequisites</span></span>

* [<span data-ttu-id="f6192-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="f6192-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="f6192-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **附註： 在安裝 SDK 之後重新啟動電腦。**</span><span class="sxs-lookup"><span data-stu-id="f6192-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="f6192-113">Microsoft Azure 訂用帳戶： 若要申請免費的 Azure 試用帳戶，請參閱[Azure 免費試用](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="f6192-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="f6192-114">建立 Azure Web 角色的應用程式公開 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="f6192-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="f6192-115">開啟 Visual Studio 2015。</span><span class="sxs-lookup"><span data-stu-id="f6192-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="f6192-116">在 Visual Studio 2015 中，選取**檔案** > **新增** > **專案**。</span><span class="sxs-lookup"><span data-stu-id="f6192-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="f6192-117">在**範本**窗格**新專案**視窗下的**Visual C#** 節點中，選取**雲端**節點，然後選取**Azure 雲端服務**範本。</span><span class="sxs-lookup"><span data-stu-id="f6192-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="f6192-118">將應用程式命名**SignalRPerfCounters**選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="f6192-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![新的雲端應用程式](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="f6192-120">在**新 Microsoft Azure 雲端服務**對話方塊中，選取**ASP.NET Web 角色**選取 > 按鈕，將角色加入至專案。</span><span class="sxs-lookup"><span data-stu-id="f6192-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="f6192-121">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f6192-121">Select **OK**.</span></span>

   ![新增 ASP.NET Web 角色](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="f6192-123">在**新 ASP.NET Web 應用程式-WebRole1**對話方塊中，選取**MVC**範本，，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="f6192-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![新增 MVC 和 Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="f6192-125">在**方案總管 中**，開啟*diagnostics.wadcfgx*底下**WebRole1**。</span><span class="sxs-lookup"><span data-stu-id="f6192-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![方案總管 diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="f6192-127">使用下列組態取代檔案的內容，然後儲存檔案：</span><span class="sxs-lookup"><span data-stu-id="f6192-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="f6192-128">開啟**Package Manager Console**從**工具** > **NuGet 套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="f6192-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="f6192-129">輸入下列命令來安裝 SignalR 和 SignalR 的公用程式套件的最新版本：</span><span class="sxs-lookup"><span data-stu-id="f6192-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="f6192-130">設定應用程式啟動或回收時，安裝 SignalR 效能計數器到角色執行個體。</span><span class="sxs-lookup"><span data-stu-id="f6192-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="f6192-131">在**方案總管 中**，以滑鼠右鍵按一下**WebRole1**專案，然後選取**新增** > **新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="f6192-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="f6192-132">將新的資料夾命名*啟動*。</span><span class="sxs-lookup"><span data-stu-id="f6192-132">Name the new folder *Startup*.</span></span>

   ![新增 [啟動] 資料夾](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="f6192-134">複製*signalr.exe*檔案 (使用新增**Microsoft.AspNet.SignalR.Utils**封裝) 從\<專案資料夾 > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils。\<版本 > 工具來 /*啟動*您在上一個步驟中建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6192-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="f6192-135">在**方案總管 中**，以滑鼠右鍵按一下*啟動*資料夾，然後選取**新增** > **現有項目**。</span><span class="sxs-lookup"><span data-stu-id="f6192-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="f6192-136">在出現的對話方塊，選取*signalr.exe*選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6192-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![將 signalr.exe 加入至專案](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="f6192-138">以滑鼠右鍵按一下*啟動*您建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6192-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="f6192-139">選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="f6192-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="f6192-140">選取**一般**節點中，選取**文字檔**，並命名新的項目*SignalRPerfCounterInstall.cmd*。</span><span class="sxs-lookup"><span data-stu-id="f6192-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="f6192-141">此命令檔將 web 角色安裝 SignalR 效能計數器。</span><span class="sxs-lookup"><span data-stu-id="f6192-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![建立 SignalR 效能計數器安裝批次檔](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="f6192-143">當 Visual Studio 建立*SignalRPerfCounterInstall.cmd*檔案，它會自動開啟主視窗中。</span><span class="sxs-lookup"><span data-stu-id="f6192-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="f6192-144">檔案的內容取代為下列指令碼，然後儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="f6192-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="f6192-145">此指令碼執行*signalr.exe*，SignalR 效能計數器加入至角色執行個體。</span><span class="sxs-lookup"><span data-stu-id="f6192-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="f6192-146">選取*signalr.exe*檔案**方案總管 中**。</span><span class="sxs-lookup"><span data-stu-id="f6192-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="f6192-147">在檔案的**屬性**，將**複製到輸出目錄**至**永遠複製**。</span><span class="sxs-lookup"><span data-stu-id="f6192-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![將複本設定為 永遠複製到輸出目錄](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="f6192-149">重複前一個步驟*SignalRPerfCounterInstall.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="f6192-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="f6192-150">以滑鼠右鍵按一下*SignalRPerfCounterInstall.cmd*檔案，然後選取**開啟**。</span><span class="sxs-lookup"><span data-stu-id="f6192-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="f6192-151">在出現的對話方塊，選取**二進位編輯器**選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="f6192-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![使用二進位編輯器中開啟](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="f6192-153">在二進位編輯器中，選取檔案中的任何前導位元組，然後予以刪除。</span><span class="sxs-lookup"><span data-stu-id="f6192-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="f6192-154">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="f6192-154">Save and close the file.</span></span>

    ![刪除前導位元組](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="f6192-156">開啟*ServiceDefinition.csdef*並加入啟動工作執行*SignalrPerfCounterInstall.cmd*檔案服務啟動時：</span><span class="sxs-lookup"><span data-stu-id="f6192-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="f6192-157">開啟`Views/Shared/_Layout.cshtml`和 jQuery 配套指令碼移除檔案的結尾。</span><span class="sxs-lookup"><span data-stu-id="f6192-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="f6192-158">新增連續呼叫的 JavaScript 用戶端`increment`在伺服器上的方法。</span><span class="sxs-lookup"><span data-stu-id="f6192-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="f6192-159">開啟`Views/Home/Index.cshtml`並取代為下列程式碼的內容：</span><span class="sxs-lookup"><span data-stu-id="f6192-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="f6192-160">建立新的資料夾中**WebRole1**專案名為*集線器*。</span><span class="sxs-lookup"><span data-stu-id="f6192-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="f6192-161">以滑鼠右鍵按一下*集線器*資料夾中的**方案總管 中**，選取**Web** > **SignalR**，然後選取**SignalR 中樞類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="f6192-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="f6192-162">命名新的中樞*MyHub.cs*選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6192-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![將 SignalR 中樞類別加入至中樞中的資料夾加入新項目對話方塊](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="f6192-164">*MyHub.cs*會自動在主要視窗中開啟。</span><span class="sxs-lookup"><span data-stu-id="f6192-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="f6192-165">下列程式碼，以取代內容，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="f6192-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="f6192-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* 是測試工具提供與 SignalR 程式碼基底連接密度。</span><span class="sxs-lookup"><span data-stu-id="f6192-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="f6192-167">由於區軸需要持續連線，您加入一個參考至您的網站使用的測試時。</span><span class="sxs-lookup"><span data-stu-id="f6192-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="f6192-168">新增資料夾到**WebRole1**專案，稱為*PersistentConnections*。</span><span class="sxs-lookup"><span data-stu-id="f6192-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="f6192-169">以滑鼠右鍵按一下此資料夾，然後選取**新增** > **類別**。</span><span class="sxs-lookup"><span data-stu-id="f6192-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f6192-170">將新的類別檔案*MyPersistentConnections.cs*選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6192-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="f6192-171">Visual Studio 會開啟*MyPersistentConnections.cs*主視窗中的檔案。</span><span class="sxs-lookup"><span data-stu-id="f6192-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="f6192-172">下列程式碼，以取代內容，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="f6192-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="f6192-173">使用`Startup`類別，SignalR 物件一開始 OWIN 啟動時。</span><span class="sxs-lookup"><span data-stu-id="f6192-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="f6192-174">開啟或建立*Startup.cs*並取代為下列程式碼的內容：</span><span class="sxs-lookup"><span data-stu-id="f6192-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="f6192-175">在上述程式碼中`OwinStartup`屬性標記開始 OWIN 這個類別。</span><span class="sxs-lookup"><span data-stu-id="f6192-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="f6192-176">`Configuration`方法會啟動 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f6192-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="f6192-177">在 Microsoft Azure 模擬器中測試您的應用程式，藉由按下**F5**。</span><span class="sxs-lookup"><span data-stu-id="f6192-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6192-178">如果您遇到**FileLoadException**在**MapSignalR**，變更中的繫結重新導向*web.config*如下：</span><span class="sxs-lookup"><span data-stu-id="f6192-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="f6192-179">等候約 1 分鐘。</span><span class="sxs-lookup"><span data-stu-id="f6192-179">Wait about one minute.</span></span> <span data-ttu-id="f6192-180">在 Visual Studio 中開啟 Cloud Explorer 工具視窗 (**檢視** > **Cloud Explorer**) 並展開路徑`(Local)/Storage Accounts/(Development)/Tables`。</span><span class="sxs-lookup"><span data-stu-id="f6192-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="f6192-181">按兩下**WADPerformanceCountersTable**。</span><span class="sxs-lookup"><span data-stu-id="f6192-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="f6192-182">您應該會看到的資料表資料中的 SignalR 計數器。</span><span class="sxs-lookup"><span data-stu-id="f6192-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="f6192-183">如果您沒有看到資料表，您可能需要重新輸入您的 Azure 儲存體認證。</span><span class="sxs-lookup"><span data-stu-id="f6192-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="f6192-184">您可能需要選取**重新整理** 按鈕，請參閱表格**Cloud Explorer**或選取**重新整理**按鈕的開啟資料表視窗中，以查看資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="f6192-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![在 Visual Studio Cloud Explorer 中選取 WAD 效能計數器資料表](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD 效能計數器資料表中顯示計數器收集](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="f6192-187">若要測試您的應用程式在雲端中，更新**ServiceConfiguration.Cloud.cscfg**檔案並設定`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`為有效的 Azure 儲存體帳戶連接字串。</span><span class="sxs-lookup"><span data-stu-id="f6192-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="f6192-188">部署您的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="f6192-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="f6192-189">如需有關如何在應用程式部署至 Azure 的詳細資訊，請參閱[如何建立及部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。</span><span class="sxs-lookup"><span data-stu-id="f6192-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="f6192-190">請稍候幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="f6192-190">Wait a few minutes.</span></span> <span data-ttu-id="f6192-191">在**Cloud Explorer**，找出您在上列設定的儲存體帳戶，並尋找`WADPerformanceCountersTable`資料表中的。</span><span class="sxs-lookup"><span data-stu-id="f6192-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="f6192-192">您應該會看到的資料表資料中的 SignalR 計數器。</span><span class="sxs-lookup"><span data-stu-id="f6192-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="f6192-193">如果您沒有看到資料表，您可能需要重新輸入您的 Azure 儲存體認證。</span><span class="sxs-lookup"><span data-stu-id="f6192-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="f6192-194">您可能需要選取**重新整理** 按鈕，請參閱表格**Cloud Explorer**或選取**重新整理**按鈕的開啟資料表視窗中，以查看資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="f6192-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="f6192-195">特別感謝[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)本教學課程中使用的原始內容。</span><span class="sxs-lookup"><span data-stu-id="f6192-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
