---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: 使用 Azure Web 角色中的 SignalR 效能計數器 |Microsoft Docs
author: guardrex
description: 如何安裝和使用 Azure Web 角色中的 SignalR 效能計數器。
keywords: ASP.NET,signalr,performance 計數器，azure web 角色
ms.author: riande
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: acc9836535466a801f1f46ec18d05937e2e42af2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835100"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>使用 Azure Web 角色中的 SignalR 效能計數器

作者：[Luke Latham](https://github.com/guardrex)

SignalR 效能計數器用來監視 Azure Web 角色中的應用程式的效能。 計數器是由 Microsoft Azure 診斷擷取。 您在 Azure 上使用安裝 SignalR 效能計數器*signalr.exe*，獨立或內部部署應用程式所用的相同工具。 Azure 角色是暫時性的因為您會設定應用程式安裝並註冊在啟動時的 SignalR 效能計數器。

## <a name="prerequisites"></a>必要條件

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **附註： 安裝 SDK 之後，重新啟動您的電腦。**
* Microsoft Azure 訂用帳戶： 若要申請免費 Azure 試用帳戶，請參閱[Azure 免費試用](https://azure.microsoft.com/free/)。

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>建立 Azure Web 角色應用程式公開 （expose) 的 SignalR 效能計數器

1. 開啟 Visual Studio 2015。

2. 在 Visual Studio 2015 中，選取**檔案** > **新增** > **專案**。

3. 在**範本**窗格**新增專案**下方視窗**Visual C#** 節點中，選取**雲端**節點，然後選取**Azure 雲端服務**範本。 將應用程式命名**SignalRPerfCounters** ，然後選取**確定**。

   ![新的雲端應用程式](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. 在 [**新的 Microsoft Azure 雲端服務**對話方塊中，選取**ASP.NET Web 角色**，然後選取 >] 按鈕，將角色新增至專案。 選取 [確定]。

   ![新增 ASP.NET Web 角色](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. 在 **新增 ASP.NET Web 應用程式-WebRole1**對話方塊中，選取**MVC**範本，，然後選取**確定**。

   ![新增 MVC 和 Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. 在 [**方案總管] 中**，開啟*diagnostics.wadcfgx*檔案下**WebRole1**。

   ![方案總管 diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. 使用下列組態取代檔案的內容，然後儲存檔案：

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. 開啟**Package Manager Console**從**工具** > **NuGet 套件管理員**。 輸入下列命令來安裝最新版的 SignalR 和 SignalR 的公用程式封裝：

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. SignalR 效能計數器安裝到角色執行個體，它在啟動或回收時，應用程式設定。 在 [**方案總管] 中**，以滑鼠右鍵按一下**WebRole1**專案，然後選取**新增** > **新資料夾**。 將新資料夾命名*啟動*。

   ![加入啟動資料夾](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. 複製*signalr.exe*檔案 (加上**Microsoft.AspNet.SignalR.Utils**套件) 從\<專案資料夾 > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils。\<版本 > / 工具來*啟動*您在上一個步驟中建立的資料夾。

11. 在 **方案總管**，以滑鼠右鍵按一下*啟動*資料夾，然後選取**新增** > **現有項目**。 在出現的對話方塊中，選取*signalr.exe* ，然後選取**新增**。

    ![將 signalr.exe 新增至專案](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. 以滑鼠右鍵按一下*啟動*您所建立的資料夾。 選取 [新增] > [新增項目]。 選取 **一般**節點中，選取**文字檔**，並命名新的項目*SignalRPerfCounterInstall.cmd*。 此命令檔會安裝成 web 角色的 SignalR 效能計數器。

    ![建立 SignalR 效能計數器安裝批次檔](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. 當 Visual Studio 會建立*SignalRPerfCounterInstall.cmd*檔案，它會自動開啟主視窗中。 檔案的內容取代為下列的指令碼，然後儲存並關閉檔案。 此指令碼執行*signalr.exe*，SignalR 效能計數器加入至角色執行個體。

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. 選取 [ *signalr.exe*中的檔案**方案總管] 中**。 中的檔案**屬性**，將**複製到輸出目錄**來**永遠複製**。

    ![設定複製到輸出目錄，將一律複製](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. 重複上述步驟，如*SignalRPerfCounterInstall.cmd*檔案。

    
16. 以滑鼠右鍵按一下*SignalRPerfCounterInstall.cmd*檔案，然後選取**開啟**。 在出現的對話方塊中，選取**二進位編輯器**，然後選取**確定**。

    ![使用二進位編輯器中開啟](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. 在二進位編輯器中，選取 檔案中的任何前導位元組，然後予以刪除。 儲存並關閉檔案。

    ![刪除開頭的位元組](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. 開啟*ServiceDefinition.csdef*並將執行啟動工作加入*SignalrPerfCounterInstall.cmd*檔案服務啟動時：

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. 開啟`Views/Shared/_Layout.cshtml`和 jQuery 的套件組合指令碼移除檔案的結尾。

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. 新增連續呼叫的 JavaScript 用戶端`increment`伺服器上的方法。 開啟`Views/Home/Index.cshtml`並以下列程式碼取代內容：

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. 建立新的資料夾中**WebRole1**專案，命名為*中樞*。 以滑鼠右鍵按一下*集線器*資料夾中的**方案總管**，選取**Web** > **SignalR**，然後選取**SignalR Hub 類別 (v2)**。 命名新的中樞*MyHub.cs* ，然後選取**新增**。

    ![將 SignalR Hub 類別新增至 [中樞] 資料夾，在 [加入新項目] 對話方塊](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs*主視窗中會自動開啟。 內容取代為下列程式碼，然後儲存並關閉檔案：

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)* 是連線密度測試與 SignalR 程式碼基底所提供的工具。 由於即使需要持續連線，新增至您的網站，用於測試時。 新增新的資料夾，以**WebRole1**專案，稱為*PersistentConnections*。 以滑鼠右鍵按一下此資料夾，然後選取**新增** > **類別**。 將新的類別檔案命名*MyPersistentConnections.cs* ，然後選取**新增**。

24. Visual Studio 會開啟*MyPersistentConnections.cs*主視窗中的檔案。 內容取代為下列程式碼，然後儲存並關閉檔案：

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. 使用`Startup`類別，SignalR 物件一開始 OWIN 啟動時。 開啟或建立*Startup.cs*並以下列程式碼取代內容：

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    在上述程式碼中`OwinStartup`屬性會標記此類別來啟動 OWIN。 `Configuration`方法會啟動 SignalR。
    
26. 在 Microsoft Azure 模擬器中測試您的應用程式，藉由按下**F5**。

    > [!NOTE]
    > 如果您遇到**FileLoadException**在**MapSignalR**，變更在繫結重新導向*web.config*如下：

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. 等候約一分鐘。 在 Visual Studio 中開啟 Cloud Explorer 工具視窗 (**檢視** > **Cloud Explorer**) 和展開路徑`(Local)/Storage Accounts/(Development)/Tables`。 按兩下**WADPerformanceCountersTable**。 您應該會看到的資料表資料中的 SignalR 計數器。 如果您沒有看到資料表，您可能需要重新輸入您的 Azure 儲存體認證。 您可能需要選取**重新整理** 按鈕，請參閱表格**Cloud Explorer**或選取**重新整理**在開啟的資料表視窗中，若要查看資料表中的資料 按鈕。

    ![在 Visual Studio 雲端總管 中選取 WAD 效能計數器資料表](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD 效能計數器資料表中顯示的計數器收集](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. 若要測試您的應用程式在雲端中，更新**ServiceConfiguration.Cloud.cscfg**檔案，並設定`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`為有效的 Azure 儲存體帳戶連接字串。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. 部署您的 Azure 訂用帳戶。 如需如何部署至 Azure 應用程式的詳細資訊，請參閱 <<c0> [ 如何建立和部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。

30. 等候幾分鐘的時間。 在 [**雲端總管]**，找出您在上面設定的儲存體帳戶，並尋找`WADPerformanceCountersTable`裡面的資料表。 您應該會看到的資料表資料中的 SignalR 計數器。 如果您沒有看到資料表，您可能需要重新輸入您的 Azure 儲存體認證。 您可能需要選取**重新整理** 按鈕，請參閱表格**Cloud Explorer**或選取**重新整理**在開啟的資料表視窗中，若要查看資料表中的資料 按鈕。

特別感謝[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)本教學課程使用的原始內容。
