---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: 使用 Azure Web 角色中的 SignalR 效能計數器 |Microsoft Docs
author: guardrex
description: 如何安裝和使用 Azure Web 角色中的 SignalR 效能計數器。
keywords: ASP.NET,signalr,performance 計數器，azure web 角色
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 8e17e945bc144731dd149bd7ddfc9e29160eaf0b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836477"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="f2a85-104">使用 Azure Web 角色中的 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="f2a85-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="f2a85-105">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f2a85-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="f2a85-106">SignalR 效能計數器用來監視 Azure Web 角色中的應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="f2a85-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="f2a85-107">計數器是由 Microsoft Azure 診斷擷取。</span><span class="sxs-lookup"><span data-stu-id="f2a85-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="f2a85-108">您在 Azure 上使用安裝 SignalR 效能計數器*signalr.exe*，獨立或內部部署應用程式所用的相同工具。</span><span class="sxs-lookup"><span data-stu-id="f2a85-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="f2a85-109">Azure 角色是暫時性的因為您會設定應用程式安裝並註冊在啟動時的 SignalR 效能計數器。</span><span class="sxs-lookup"><span data-stu-id="f2a85-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2a85-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="f2a85-110">Prerequisites</span></span>

* <span data-ttu-id="f2a85-111">Visual Studio 2015 或[2017年](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="f2a85-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="f2a85-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **附註：安裝 SDK 之後，重新啟動您的電腦。**</span><span class="sxs-lookup"><span data-stu-id="f2a85-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="f2a85-113">Microsoft Azure 訂用帳戶：若要申請免費 Azure 試用帳戶，請參閱[Azure 免費試用](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="f2a85-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="f2a85-114">建立 Azure Web 角色應用程式公開 （expose) 的 SignalR 效能計數器</span><span class="sxs-lookup"><span data-stu-id="f2a85-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="f2a85-115">開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f2a85-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="f2a85-116">在 Visual Studio 中，選取 [檔案] > [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f2a85-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="f2a85-117">在**新的專案**對話方塊中，選取**Visual C#** > **雲端**在左側的類別目錄，然後選取**Azure 雲端服務**範本。</span><span class="sxs-lookup"><span data-stu-id="f2a85-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="f2a85-118">將應用程式命名**SignalRPerfCounters** ，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![新的雲端應用程式](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="f2a85-120">如果您沒有看到**雲端**範本類別目錄或**Azure 雲端服務** 範本，您需要安裝**Azure 開發**for Visual Studio 2017 工作負載。</span><span class="sxs-lookup"><span data-stu-id="f2a85-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="f2a85-121">選擇**開啟 Visual Studio 安裝程式**左下方的連結**新的專案**對話方塊來開啟 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f2a85-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="f2a85-122">選取  **Azure 開發**工作負載，然後選擇**修改**開始安裝工作負載。</span><span class="sxs-lookup"><span data-stu-id="f2a85-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![在 Visual Studio 安裝程式中的 azure 開發工作負載](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="f2a85-124">在 [**新的 Microsoft Azure 雲端服務**對話方塊中，選取**ASP.NET Web 角色**，然後選取 >] 按鈕，將角色新增至專案。</span><span class="sxs-lookup"><span data-stu-id="f2a85-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="f2a85-125">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="f2a85-125">Select **OK**.</span></span>

   ![新增 ASP.NET Web 角色](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="f2a85-127">在 **新增 ASP.NET Web 應用程式-WebRole1**對話方塊中，選取**MVC**範本，，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![新增 MVC 和 Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="f2a85-129">在 [**方案總管] 中**，開啟*diagnostics.wadcfgx*檔案下**WebRole1**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![方案總管 diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="f2a85-131">使用下列組態取代檔案的內容，然後儲存檔案：</span><span class="sxs-lookup"><span data-stu-id="f2a85-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="f2a85-132">開啟**Package Manager Console**從**工具** > **NuGet 套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="f2a85-133">輸入下列命令來安裝最新版的 SignalR 和 SignalR 的公用程式封裝：</span><span class="sxs-lookup"><span data-stu-id="f2a85-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="f2a85-134">SignalR 效能計數器安裝到角色執行個體，它在啟動或回收時，應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="f2a85-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="f2a85-135">在 [**方案總管] 中**，以滑鼠右鍵按一下**WebRole1**專案，然後選取**新增** > **新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="f2a85-136">將新資料夾命名*啟動*。</span><span class="sxs-lookup"><span data-stu-id="f2a85-136">Name the new folder *Startup*.</span></span>

   ![加入啟動資料夾](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="f2a85-138">複製*signalr.exe*檔案 (加上**Microsoft.AspNet.SignalR.Utils**套件) 從\<專案資料夾 > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils。\<版本 > / 工具來*啟動*您在上一個步驟中建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2a85-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="f2a85-139">在 **方案總管**，以滑鼠右鍵按一下*啟動*資料夾，然後選取**新增** > **現有項目**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="f2a85-140">在出現的對話方塊中，選取*signalr.exe* ，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![將 signalr.exe 新增至專案](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="f2a85-142">以滑鼠右鍵按一下*啟動*您所建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2a85-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="f2a85-143">選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="f2a85-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="f2a85-144">選取 **一般**節點中，選取**文字檔**，並命名新的項目*SignalRPerfCounterInstall.cmd*。</span><span class="sxs-lookup"><span data-stu-id="f2a85-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="f2a85-145">此命令檔會安裝成 web 角色的 SignalR 效能計數器。</span><span class="sxs-lookup"><span data-stu-id="f2a85-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![建立 SignalR 效能計數器安裝批次檔](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="f2a85-147">當 Visual Studio 會建立*SignalRPerfCounterInstall.cmd*檔案，它會自動開啟主視窗中。</span><span class="sxs-lookup"><span data-stu-id="f2a85-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="f2a85-148">檔案的內容取代為下列的指令碼，然後儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="f2a85-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="f2a85-149">此指令碼執行*signalr.exe*，SignalR 效能計數器加入至角色執行個體。</span><span class="sxs-lookup"><span data-stu-id="f2a85-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="f2a85-150">選取 [ *signalr.exe*中的檔案**方案總管] 中**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="f2a85-151">中的檔案**屬性**，將**複製到輸出目錄**來**永遠複製**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![設定複製到輸出目錄，將一律複製](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="f2a85-153">重複上述步驟，如*SignalRPerfCounterInstall.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="f2a85-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>


16. <span data-ttu-id="f2a85-154">以滑鼠右鍵按一下*SignalRPerfCounterInstall.cmd*檔案，然後選取**開啟**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="f2a85-155">在出現的對話方塊中，選取**二進位編輯器**，然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![使用二進位編輯器中開啟](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="f2a85-157">在二進位編輯器中，選取 檔案中的任何前導位元組，然後予以刪除。</span><span class="sxs-lookup"><span data-stu-id="f2a85-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="f2a85-158">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="f2a85-158">Save and close the file.</span></span>

    ![刪除開頭的位元組](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="f2a85-160">開啟*ServiceDefinition.csdef*並將執行啟動工作加入*SignalrPerfCounterInstall.cmd*檔案服務啟動時：</span><span class="sxs-lookup"><span data-stu-id="f2a85-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="f2a85-161">開啟`Views/Shared/_Layout.cshtml`和 jQuery 的套件組合指令碼移除檔案的結尾。</span><span class="sxs-lookup"><span data-stu-id="f2a85-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="f2a85-162">新增連續呼叫的 JavaScript 用戶端`increment`伺服器上的方法。</span><span class="sxs-lookup"><span data-stu-id="f2a85-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="f2a85-163">開啟`Views/Home/Index.cshtml`並以下列程式碼取代內容：</span><span class="sxs-lookup"><span data-stu-id="f2a85-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="f2a85-164">建立新的資料夾中**WebRole1**專案，命名為*中樞*。</span><span class="sxs-lookup"><span data-stu-id="f2a85-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="f2a85-165">以滑鼠右鍵按一下*集線器*資料夾中的**方案總管**，然後選取**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="f2a85-166">在 **加入新項目**對話方塊中，選取**Web** > **SignalR**類別，然後再選取**SignalR Hub 類別 (v2)** 項目範本。</span><span class="sxs-lookup"><span data-stu-id="f2a85-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="f2a85-167">命名新的中樞*MyHub.cs* ，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![將 SignalR Hub 類別新增至 [中樞] 資料夾，在 [加入新項目] 對話方塊](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="f2a85-169">*MyHub.cs*主視窗中會自動開啟。</span><span class="sxs-lookup"><span data-stu-id="f2a85-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="f2a85-170">內容取代為下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="f2a85-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="f2a85-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* 是連線密度測試與 SignalR 程式碼基底所提供的工具。</span><span class="sxs-lookup"><span data-stu-id="f2a85-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="f2a85-172">由於即使需要持續連線，新增至您的網站，用於測試時。</span><span class="sxs-lookup"><span data-stu-id="f2a85-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="f2a85-173">新增新的資料夾，以**WebRole1**專案，稱為*PersistentConnections*。</span><span class="sxs-lookup"><span data-stu-id="f2a85-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="f2a85-174">以滑鼠右鍵按一下此資料夾，然後選取**新增** > **類別**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f2a85-175">將新的類別檔案命名*MyPersistentConnections.cs* ，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="f2a85-176">Visual Studio 會開啟*MyPersistentConnections.cs*主視窗中的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2a85-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="f2a85-177">內容取代為下列程式碼，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="f2a85-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="f2a85-178">使用`Startup`類別，SignalR 物件一開始 OWIN 啟動時。</span><span class="sxs-lookup"><span data-stu-id="f2a85-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="f2a85-179">開啟或建立*Startup.cs*並以下列程式碼取代內容：</span><span class="sxs-lookup"><span data-stu-id="f2a85-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="f2a85-180">在上述程式碼中`OwinStartup`屬性會標記此類別來啟動 OWIN。</span><span class="sxs-lookup"><span data-stu-id="f2a85-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="f2a85-181">`Configuration`方法會啟動 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f2a85-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="f2a85-182">在 Microsoft Azure 模擬器中測試您的應用程式，藉由按下**F5**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2a85-183">如果您遇到**FileLoadException**在**MapSignalR**，變更在繫結重新導向*web.config*如下：</span><span class="sxs-lookup"><span data-stu-id="f2a85-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="f2a85-184">等候約一分鐘。</span><span class="sxs-lookup"><span data-stu-id="f2a85-184">Wait about one minute.</span></span> <span data-ttu-id="f2a85-185">在 Visual Studio 中開啟 Cloud Explorer 工具視窗 (**檢視** > **Cloud Explorer**) 和展開路徑`(Local)/Storage Accounts/(Development)/Tables`。</span><span class="sxs-lookup"><span data-stu-id="f2a85-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="f2a85-186">按兩下**WADPerformanceCountersTable**。</span><span class="sxs-lookup"><span data-stu-id="f2a85-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="f2a85-187">您應該會看到的資料表資料中的 SignalR 計數器。</span><span class="sxs-lookup"><span data-stu-id="f2a85-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="f2a85-188">如果您沒有看到資料表，您可能需要重新輸入您的 Azure 儲存體認證。</span><span class="sxs-lookup"><span data-stu-id="f2a85-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="f2a85-189">您可能需要選取**重新整理** 按鈕，請參閱表格**Cloud Explorer**或選取**重新整理**在開啟的資料表視窗中，若要查看資料表中的資料 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f2a85-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![在 Visual Studio 雲端總管 中選取 WAD 效能計數器資料表](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD 效能計數器資料表中顯示的計數器收集](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="f2a85-192">若要測試您的應用程式在雲端中，更新**ServiceConfiguration.Cloud.cscfg**檔案，並設定`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`為有效的 Azure 儲存體帳戶連接字串。</span><span class="sxs-lookup"><span data-stu-id="f2a85-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="f2a85-193">部署您的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="f2a85-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="f2a85-194">如需如何部署至 Azure 應用程式的詳細資訊，請參閱 <<c0> [ 如何建立和部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。</span><span class="sxs-lookup"><span data-stu-id="f2a85-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="f2a85-195">等候幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="f2a85-195">Wait a few minutes.</span></span> <span data-ttu-id="f2a85-196">在 [**雲端總管]**，找出您在上面設定的儲存體帳戶，並尋找`WADPerformanceCountersTable`裡面的資料表。</span><span class="sxs-lookup"><span data-stu-id="f2a85-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="f2a85-197">您應該會看到的資料表資料中的 SignalR 計數器。</span><span class="sxs-lookup"><span data-stu-id="f2a85-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="f2a85-198">如果您沒有看到資料表，您可能需要重新輸入您的 Azure 儲存體認證。</span><span class="sxs-lookup"><span data-stu-id="f2a85-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="f2a85-199">您可能需要選取**重新整理** 按鈕，請參閱表格**Cloud Explorer**或選取**重新整理**在開啟的資料表視窗中，若要查看資料表中的資料 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f2a85-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="f2a85-200">特別感謝[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)本教學課程使用的原始內容。</span><span class="sxs-lookup"><span data-stu-id="f2a85-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
