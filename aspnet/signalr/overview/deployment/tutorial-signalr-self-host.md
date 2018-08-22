---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 教學課程： SignalR 自我裝載 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立自我裝載的 SignalR 2 伺服器，以及如何使用 JavaScript 用戶端連線到它。 教學課程 V 中使用的軟體版本...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 5d7d485357a6c820f11e0135e2ff9479c1965d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826845"
---
<a name="tutorial-signalr-self-host"></a>教學課程： SignalR 自我裝載
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[下載已完成的專案](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

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
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>本教學課程中使用 Visual Studio 2012
> 
> 
> 若要使用 Visual Studio 2012，本教學課程中，執行下列作業：
> 
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。
> - 有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。
> 
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

SignalR 伺服器通常裝載在 IIS 中，ASP.NET 應用程式中，但它也可以是自我裝載 （例如主控台應用程式或 Windows 服務） 使用的自我裝載的程式庫。 此程式庫，例如 SignalR 2 的所有以 OWIN 為基礎 ([Open Web Interface for.NET](http://owin.org))。 OWIN 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 可分隔的伺服器，讓 OWIN 很適合用於自我裝載的 web 應用程式，在您自己的程序，IIS 外部的 web 應用程式。

不在 IIS 中裝載的原因包括：

- 其中 IIS 不提供或需要的資源，例如現有的伺服器陣列，而不使用 IIS 的環境。
- 若要避免需要 IIS 的效能負荷。
- SignalR 功能是可以加入至執行 Windows 服務、 Azure 背景工作角色或其他處理序中的現有應用程式。

如果正在開發的解決方案中做為自我裝載基於效能考量，建議也是測試應用程式裝載在 IIS 中決定的效能優勢。

本教學課程包含下列各節：

- [建立伺服器](#server)
- [存取 JavaScript 用戶端與伺服器](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>建立伺服器

在本教學課程中，您會在主控台應用程式，來建立裝載的伺服器，但是伺服器可以裝載在任何一種處理程序，例如 Windows 服務或 Azure 背景工作角色。 裝載 Windows 服務中的 SignalR 伺服器的範例程式碼，請參閱[Self-Hosting Windows 服務中的 SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)。

1. 系統管理員權限開啟 Visual Studio 2013。 選取 **檔案**，**新的專案**。 選取  **Windows**下方**Visual C#** 節點中的**範本**窗格，然後選取**主控台應用程式**範本。 將新專案命名為"SignalRSelfHost 」，然後按一下**確定**。

    ![](tutorial-signalr-self-host/_static/image1.png)
2. 開啟選取的程式庫套件管理員主控台**工具**，**程式庫套件管理員**， **Package Manager Console**。
3. 在 [套件管理員] 主控台中，輸入下列命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    此命令會將專案中的 SignalR 2 自我裝載的程式庫。
4. 在 [套件管理員] 主控台中，輸入下列命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    此命令會將 Microsoft.Owin.Cors 程式庫加入至專案。 此程式庫會用於跨網域支援，所需的裝載 SignalR 和不同網域中的網頁用戶端應用程式。 由於 SignalR 伺服器和 web 用戶端上不同的連接埠，您會在裝載，這表示該跨網域必須啟用這些元件之間的通訊。
5. 以下列程式碼取代 Program.cs 的內容。

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    上述程式碼包含三個類別：

    - **計劃**，包括**Main**定義主要執行路徑的方法。 在此方法中，類型的 web 應用程式**啟始**會在指定的 URL 啟動 (`http://localhost:8080`)。 如果端點需要安全性，就可以實作 SSL。 請參閱[如何： 使用 SSL 憑證設定連接埠](https://msdn.microsoft.com/library/ms733791.aspx)如需詳細資訊。
    - **啟始**、 包含 SignalR 伺服器組態的類別 (本教學課程使用的唯一設定是在呼叫`UseCors`)，和呼叫`MapSignalR`，專案中建立路由的中樞中的任何物件。
    - **MyHub**，應用程式會提供給用戶端的 SignalR Hub 類別。 這個類別具有單一方法**傳送**，用戶端會呼叫以將訊息廣播到所有其他連線的用戶端。
6. 編譯並執行應用程式。 伺服器正在執行的地址應該會顯示在主控台視窗中。

    ![](tutorial-signalr-self-host/_static/image2.png)
7. 如果執行失敗，發生例外狀況`System.Reflection.TargetInvocationException was unhandled`，您必須重新啟動 Visual Studio 系統管理員權限。
8. 將應用程式停止後再繼續下一節。

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>存取 JavaScript 用戶端與伺服器

在本節中，您將使用相同的 JavaScript 用戶端，從[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)。 我們只進行一項修改用戶端，也就是明確地定義中樞的 URL。 自我裝載的應用程式中，伺服器不一定可能在相同的位址，做為連接 URL （因為反向 proxy 和負載平衡器），因此 URL 必須明確定義。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下方案，然後選取**新增**，**新專案**。 選取  **Web**節點，然後選取**ASP.NET Web 應用程式**範本。 將專案命名為"JavascriptClient 」，然後按一下**確定**。

    ![](tutorial-signalr-self-host/_static/image3.png)
2. 選取 **空**範本，並保留其餘選項取消選取。 選取 **建立專案**。

    ![](tutorial-signalr-self-host/_static/image4.png)
3. 在 套件管理員 主控台中，選取 中的"JavascriptClient 」 專案**預設專案**下拉式清單中，然後執行下列命令：

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    此命令會在用戶端安裝所需的 SignalR 和 JQuery 程式庫。
4. 以滑鼠右鍵按一下專案，然後選取**新增**，**新項目**。 選取  **Web**節點，然後選取 HTML 頁面。 將頁面命名**Default.html**。

    ![](tutorial-signalr-self-host/_static/image5.png)
5. 新的 HTML 網頁的內容取代為下列程式碼。 請確認指令碼參考符合專案的 [指令碼] 資料夾中的指令碼。

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    下列程式碼 （在上述程式碼範例中反白顯示） 是您已對用戶端 （除了 SignalR 第 2 版 beta 升級程式碼） 取得厭煩教學課程中使用。 這行程式碼明確地設定基底連接 URL SignalR 的伺服器上。

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. 以滑鼠右鍵按一下方案，然後選取**設定啟始專案...**.選取 **多個啟始專案**選項按鈕，然後設定兩個專案**動作**來**啟動**。

    ![](tutorial-signalr-self-host/_static/image6.png)
7. 以滑鼠右鍵按一下 「 Default.html"，然後選取**設定為起始頁**。
8. 執行應用程式。 將會啟動伺服器和頁面。 您可能需要重新載入網頁 (或選取**繼續**偵錯工具中) 如果頁面載入之後才啟動伺服器。
9. 在瀏覽器中，提供使用者名稱出現提示時。 在頁面的 URL 複製到另一個瀏覽器索引標籤或視窗，並提供不同的使用者名稱。 您將能夠從一種瀏覽器窗格中將訊息傳送到另一個，如本快速入門教學課程所示。
