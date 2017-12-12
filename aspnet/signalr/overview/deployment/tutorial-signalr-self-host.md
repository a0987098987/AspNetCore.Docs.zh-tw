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
<a name="tutorial-signalr-self-host"></a>自我裝載的教學課程： SignalR
====================
由[Patrick Fletcher](https://github.com/pfletcher)

[下載完成的專案](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> 本教學課程會示範如何建立自我裝載的 SignalR 2 伺服器，以及如何使用 JavaScript 用戶端連線到它。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>使用 Visual Studio 2012 進行這個教學課程
> 
> 
> 透過本教學課程中使用 Visual Studio 2012，請執行下列作業：
> 
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。
> - 某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。
> 
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>概觀

SignalR 伺服器通常裝載於 IIS，ASP.NET 應用程式中，但它也可以是自我裝載 （例如，在主控台應用程式或 Windows 服務） 使用的自我裝載的程式庫。 此程式庫，如同所有 SignalR 2 根據 OWIN ([開啟適用於.NET 的 Web 介面](http://owin.org))。 OWIN 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 以減少從伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部的 web 應用程式。

未在 IIS 中裝載的原因包括：

- 其中 IIS 未提供或需要這樣做，例如 IIS 沒有現有的伺服器陣列環境。
- 若要避免需要 IIS 的效能負荷。
- SignalR 的功能是要加入至執行 Windows 服務、 Azure 背景工作角色或其他處理程序中的 exising 應用程式。

如果您正在開發方案為自我裝載，基於效能考量，建議您也測試應用程式裝載於 IIS 來判斷效能優勢。

本教學課程包含下列各節：

- [建立伺服器](#server)
- [存取 JavaScript 用戶端與伺服器](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>建立伺服器

在本教學課程中，您會在主控台應用程式，建立裝載的伺服器，但是伺服器可以裝載於任何類型的處理程序，例如 Windows 服務或 Azure 背景工作角色。 裝載 Windows 服務中的 SignalR 伺服器的範例程式碼，請參閱[Self-Hosting Windows 服務中的 SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)。

1. 系統管理員權限開啟 Visual Studio 2013。 選取**檔案**，**新專案**。 選取**Windows**下**Visual C#**節點**範本**窗格，然後選取**主控台應用程式**範本。 將新專案命名為"SignalRSelfHost 」，然後按一下**確定**。

    ![](tutorial-signalr-self-host/_static/image1.png)
2. 選取以開啟 [程式庫封裝管理員] 主控台**工具**，**程式庫套件管理員**， **Package Manager Console**。
3. 在 [封裝管理員] 主控台中，輸入下列命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    此命令會將 SignalR 2 Self-Host 程式庫加入至專案。
4. 在 [封裝管理員] 主控台中，輸入下列命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    此命令會將 Microsoft.Owin.Cors 程式庫加入至專案。 此程式庫將用於裝載 SignalR 和不同網域中的網頁用戶端應用程式需要的跨網域支援。 因為您將會裝載 SignalR 伺服器和 web 上的用戶端不同的通訊埠，這表示該跨網域必須啟用這些元件間的通訊。
5. 以下列程式碼取代 Program.cs 的內容。

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    上述程式碼包含三個類別：

    - **程式**，包括**Main**方法定義執行主要路徑。 在此方法中，類型的 web 應用程式**啟動**開始於指定的 URL (`http://localhost:8080`)。 如果端點需要安全性，您可以實作 SSL。 請參閱[How to： 使用 SSL 憑證設定連接埠](https://msdn.microsoft.com/en-us/library/ms733791.aspx)如需詳細資訊。
    - **啟動**、 包含 SignalR 伺服器設定的類別 (本教學課程使用的唯一設定是呼叫`UseCors`)，且呼叫`MapSignalR`，其中的專案中建立中樞的任何物件的路由。
    - **MyHub**，應用程式將提供給用戶端的 SignalR 中樞類別。 這個類別具有單一方法**傳送**，用戶端將呼叫廣播給所有其他連接的用戶端的訊息。
6. 編譯並執行應用程式。 主控台視窗中應該會顯示該伺服器正在執行的位址。

    ![](tutorial-signalr-self-host/_static/image2.png)
7. 如果執行作業失敗，發生例外狀況`System.Reflection.TargetInvocationException was unhandled`，您必須重新啟動 Visual Studio 系統管理員權限。
8. 停止應用程式後再繼續下一節。

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>存取 JavaScript 用戶端與伺服器

在本節中，您將使用相同的 JavaScript 用戶端從[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)。 我們只會提供給用戶端，也就是明確地定義中樞 URL 的一項修改。 自我裝載的應用程式中，伺服器可能一定無法在相同的位址做為連接 URL （由於反向 proxy 和負載平衡器），因此 URL 必須以明確定義。

1. 在**方案總管 中**，以滑鼠右鍵按一下方案，然後選取**新增**，**新專案**。 選取**Web**節點，然後選取**ASP.NET Web 應用程式**範本。 將專案命名為"JavascriptClient 」，然後按一下**確定**。

    ![](tutorial-signalr-self-host/_static/image3.png)
2. 選取**空**範本，並保持未選取其餘的選項。 選取**建立專案**。

    ![](tutorial-signalr-self-host/_static/image4.png)
3. 在 [封裝管理員] 主控台中，選取"JavascriptClient 」 專案中的**預設專案**下拉式清單中，然後執行下列命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    此命令會在用戶端安裝您將需要的 SignalR 和 JQuery 程式庫。
4. 以滑鼠右鍵按一下您的專案並選取**新增**，**新項目**。 選取**Web**節點，然後選取 HTML 頁面。 將頁面命名**Default.html**。

    ![](tutorial-signalr-self-host/_static/image5.png)
5. 新的 HTML 網頁的內容取代為下列程式碼。 請確認指令碼參考符合專案的 [指令碼] 資料夾中的指令碼。

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    下列程式碼 （上述的範例程式碼中反白顯示） 是新增您已對 Stared 快速入門教學課程 （除了程式碼升級至 SignalR 第 2 版 beta） 中使用的用戶端。 這行程式碼明確地設定基底連接 URL SignalR 的伺服器。

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. 以滑鼠右鍵按一下方案，然後選取**設定啟始專案...**.選取**多個啟始專案**選項按鈕，然後設定兩個專案的**動作**至**啟動**。

    ![](tutorial-signalr-self-host/_static/image6.png)
7. 以滑鼠右鍵按一下 「 Default.html"，然後選取**設定為起始頁**。
8. 執行應用程式。 將會啟動伺服器和頁面。 您可能需要重新載入網頁 (或選取**繼續**偵錯工具中) 如果啟動伺服器之前，載入頁面。
9. 在瀏覽器中提供的使用者名稱出現提示時。 網頁的 URL 複製到另一個瀏覽器索引標籤或視窗，並提供不同的使用者名稱。 您可以從一種瀏覽器窗格中傳送訊息至另一個，如同入門 」 教學課程。
