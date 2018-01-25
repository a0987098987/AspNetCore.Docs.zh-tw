---
uid: signalr/overview/performance/scaleout-with-redis
title: "SignalR 範圍外使用 Redis |Microsoft 文件"
author: MikeWasson
description: "軟體版本本主題中使用 Visual Studio 2013.NET 4.5 SignalR 第 2 版舊版的此主題的較早版本的相關資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-redis"></a>使用 Redis SignalR 範圍外
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


在此教學課程中，您將使用[Redis](http://redis.io/)可以分散部署兩個個別的 IIS 執行個體的 SignalR 應用程式中的訊息。

Redis 是記憶體中索引鍵-值存放區。 它也支援發行/訂閱模型的傳訊系統。 SignalR Redis 的後擋板使用 pub/sub 功能，將訊息轉送到其他伺服器。

![](scaleout-with-redis/_static/image1.png)

此教學課程中，您將使用三部伺服器：

- 兩個伺服器執行 Windows，您將用來部署 SignalR 應用程式。
- 執行 Linux，您將用來執行 Redis 的一部伺服器。 在本教學課程螢幕擷取畫面，我使用 Ubuntu 12.04 TLS。

如果您沒有使用三部實體伺服器，您可以在 HYPER-V 上建立 Vm。 另一個選項是在 Azure 上建立 Vm。

雖然本教學課程使用的官方 Redis 實作，另外還有[Windows 連接埠的 Redis](https://github.com/MSOpenTech/redis)從 MSOpenTech。 安裝和設定不同，但在其他方面的步驟都相同。

> [!NOTE] 
> 
> SignalR 範圍外 Redis 與不支援 Redis 的叢集。


## <a name="overview"></a>總覽

我們可以詳細的教學課程之前，以下是您將執行的快速概觀。

1. 安裝 Redis 並啟動 Redis 伺服器。
2. 將這些 NuGet 封裝加入至您的應用程式： 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. 建立 SignalR 應用程式。
4. 設定後擋板 Startup.cs 中加入下列程式碼： 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>HYPER-V 上的 Ubuntu

使用 Windows HYPER-V，您可以輕鬆地建立 Ubuntu VM Windows 伺服器上。

下載 Ubuntu ISO 從[http://www.ubuntu.com](http://www.ubuntu.com/)。

在 HYPER-V 中加入新的 VM。 在**連接虛擬硬碟**步驟中，選取**建立虛擬硬碟**。

![](scaleout-with-redis/_static/image2.png)

在**安裝選項**步驟中，選取**映像檔 (.iso)**，按一下 **瀏覽**，並瀏覽至 Ubuntu 安裝 ISO。

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>安裝 Redis

請依照下列步驟[http://redis.io/download](http://redis.io/download)下載，並建置 Redis。

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

這會建置 Redis 二進位檔`src`目錄。

根據預設，Redis 不需要密碼。 若要設定密碼，編輯`redis.conf`位於原始碼的根目錄中的檔案。 （請在檔案的備份副本才能進行編輯時） ！加入下列指示詞，以`redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

現在開始 Redis 伺服器：

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

開啟連接埠 6379，這種 Redis 的預設連接埠上接聽。 （您可以變更組態檔中的連接埠號碼）。

## <a name="create-the-signalr-application"></a>建立 SignalR 應用程式

建立 SignalR 應用程式的其中一個這些教學課程：

- [SignalR 2.0 使用者入門](../getting-started/tutorial-getting-started-with-signalr.md)
- [開始使用 SignalR 2.0 和 MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

接下來，我們將修改交談應用程式支援使用 Redis 範圍外。 首先，SignalR.Redis NuGet 封裝加入您的專案。 在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

接下來，開啟 Startup.cs 檔案。 將下列程式碼加入**組態**方法：

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- 「 伺服器 」 是正在執行 Redis 伺服器的名稱。
- *連接埠*通訊埠編號
- 「 密碼 」 是您在 redis.conf 檔案中定義的密碼。
- :"AppName"是任何字串。 SignalR 建立 Redis pub/sub 通道具有此名稱。

例如: 

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>部署和執行應用程式

準備部署 SignalR 應用程式的 Windows 伺服器執行個體。

新增 IIS 角色。 包含 「 應用程式開發 」 功能，包括 WebSocket 通訊協定。

![](scaleout-with-redis/_static/image5.png)

也包含管理服務 （列在 [管理工具]）。

![](scaleout-with-redis/_static/image6.png)

**安裝 Web Deploy 3.0。** 當執行 IIS 管理員，就會提示您安裝 Microsoft Web Platform，或是您可以[下載 intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)。 在 Platform Installer 中搜尋 Web deploy 及安裝 Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

檢查 Web 管理服務正在執行。 否則，請啟動服務。 （如果您沒有看到 Web 管理服務中 Windows 服務的清單，請確定您加入的 IIS 角色時，已安裝管理服務。）

根據預設，Web 管理服務會接聽 TCP 連接埠 8172 上。 在 Windows 防火牆中建立新的輸入的規則以允許連接埠 8172 上的 TCP 流量。 如需詳細資訊，請參閱[設定防火牆規則](https://technet.microsoft.com/library/dd448559(WS.10).aspx)。 （如果您裝載在 Azure 上的 Vm，您可以直接在 Azure 入口網站中。 請參閱[如何設定虛擬機器的端點](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)。)

現在您已準備好部署從開發電腦的 Visual Studio 專案到伺服器。 在 [方案總管] 中，以滑鼠右鍵按一下方案，然後按一下**發行**。

如需詳細文件中的有關 web 部署，請參閱[Visual Studio 和 ASP.NET 的 Web 部署內容地圖](../../../whitepapers/aspnet-web-deployment-content-map.md)。

如果您部署至兩個伺服器應用程式，您可以在另一個瀏覽器視窗開啟每個執行個體，並查看每個不同的方式接收 SignalR 訊息。 （當然，在實際執行環境中，兩部伺服器會位在負載平衡器後方。）

![](scaleout-with-redis/_static/image8.png)

如果您想知道即可查看訊息傳送至 Redis，您可以使用**redis cli** Redis 會安裝用戶端。

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
