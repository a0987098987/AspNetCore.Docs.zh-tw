---
uid: signalr/overview/older-versions/scaleout-with-redis
title: SignalR 向外延展與 Redis (SignalR 1.x) |Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 74294bd04d5649f2ec54e58adb744f5e30525162
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836411"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>SignalR 向外延展與 Redis (SignalR 1.x)
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

在本教學課程中，您將使用[Redis](http://redis.io/)將訊息散發到部署兩個個別的 IIS 執行個體的 SignalR 應用程式。

Redis 是記憶體中索引鍵-值存放區。 它也支援發行/訂閱模型的傳訊系統。 Redis 的 SignalR 後擋板會使用發佈/訂閱功能，將訊息轉送到其他伺服器。

![](scaleout-with-redis/_static/image1.png)

本教學課程中，您會使用三部伺服器：

- 執行 Windows，您將使用的 SignalR 應用程式部署的兩部伺服器。
- 執行 Linux，您將用來執行 Redis 的一部伺服器。 在本教學課程的螢幕擷取畫面，我會使用 Ubuntu 12.04 TLS。

如果您沒有要使用的三個實體伺服器，您可以建立在 HYPER-V 上的 Vm。 另一個選項是在 Azure 上建立 Vm。

雖然本教學課程使用官方 Redis 實作，另外還有[Windows 連接埠的 Redis](https://github.com/MSOpenTech/redis)從 MSOpenTech。 安裝和設定不同，但除此以外步驟都相同。

> [!NOTE] 
> 
> SignalR 向外的延展與 Redis 不支援 Redis 叢集。


## <a name="overview"></a>總覽

我們會詳細的教學課程之前，以下是您將進行的快速概觀。

1. 安裝 Redis 並啟動 Redis 伺服器。
2. 將這些 NuGet 套件新增至您的應用程式中： 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. 建立 SignalR 應用程式。
4. 若要設定的後擋板的 Global.asax 中加入下列程式碼： 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>HYPER-V 上的 Ubuntu

使用 Windows HYPER-V，可以在 Windows Server 上輕鬆建立 Ubuntu VM。

下載從 Ubuntu ISO [ http://www.ubuntu.com ](http://www.ubuntu.com/)。

在 HYPER-V 中新增新的 VM。 在 **連接虛擬硬碟**步驟中，選取**建立虛擬硬碟**。

![](scaleout-with-redis/_static/image2.png)

在 **安裝選項**步驟中，選取**映像檔 (.iso)**，按一下 **瀏覽**，並瀏覽至 Ubuntu 安裝 ISO。

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>安裝 Redis

請遵循的步驟[ http://redis.io/download ](http://redis.io/download)來下載並建置 Redis。

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

這會建置 Redis 二進位檔`src`目錄。

根據預設，Redis 不需要密碼。 若要設定密碼，請編輯`redis.conf`位於原始碼的根目錄中的檔案。 （請將檔案的備份副本才能進行編輯時） ！新增下列指示詞到`redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

現在開始 Redis 伺服器：

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

開啟連接埠 6379，也就是 Redis 的預設通訊埠接聽時。 （您可以變更組態檔中的連接埠號碼）。

## <a name="create-the-signalr-application"></a>建立 SignalR 應用程式

依照下列其中一個這些教學課程中建立的 SignalR 應用程式：

- [開始使用 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [開始使用 SignalR 和 MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

接下來，我們將修改的聊天應用程式，以支援向外的延展與 Redis。 首先，將 SignalR.Redis NuGet 封裝加入您的專案。 在 Visual Studio 中，從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

接下來，開啟 Global.asax 檔案。 將下列程式碼加入**應用程式\_啟動**方法：

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- 「 伺服器 」 是正在執行的 Redis 伺服器的名稱。
- *連接埠*的通訊埠編號
- 「 密碼 」 是指您在 redis.conf 檔案中定義的密碼。
- :"AppName"是任何字串。 SignalR 使用此名稱建立 Redis pub/sub 通道。

例如: 

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>部署和執行應用程式

準備您的 Windows Server 執行個體，若要部署的 SignalR 應用程式。

新增 IIS 角色。 包含 「 應用程式開發 」 功能，包括 WebSocket 通訊協定。

![](scaleout-with-redis/_static/image5.png)

也包含 「 管理 」 服務 （列在 [管理工具]）。

![](scaleout-with-redis/_static/image6.png)

**安裝 Web Deploy 3.0。** 當您執行 IIS 管理員，它會提示您安裝 Microsoft Web Platform，或者您可以[下載 intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)。 在 平台安裝程式中，搜尋 Web Deploy，並安裝 Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

檢查 Web 管理服務正在執行。 否則，請啟動服務。 （如果您沒有看到 Web 管理服務中的 Windows 服務清單，請確定您新增 IIS 角色時，安裝 「 管理服務。）

根據預設，Web 管理服務會接聽 TCP 連接埠 8172。 在 [Windows 防火牆] 中，建立新的輸入的規則，以允許連接埠 8172 上的 TCP 流量。 如需詳細資訊，請參閱 <<c0> [ 設定防火牆規則](https://technet.microsoft.com/library/dd448559(WS.10).aspx)。 （如果您裝載在 Azure 上的 Vm，您可以直接在 Azure 入口網站中。 請參閱[如何設定虛擬機器的端點](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)。)

現在，您已準備好要部署到伺服器的 從您的開發電腦的 Visual Studio 專案。 在 方案總管 中，以滑鼠右鍵按一下方案，然後按一下 **發佈**。

如需詳細的 web 部署的相關文件，請參閱[Visual Studio 及 ASP.NET 的 Web 部署內容對應](../../../whitepapers/aspnet-web-deployment-content-map.md)。

如果您部署兩部伺服器應用程式時，您可以在另一個瀏覽器視窗開啟每個執行個體，並查看他們都收到來自其他使用 SignalR 訊息。 （當然，在生產環境中，兩部伺服器會位於負載平衡器後方。）

![](scaleout-with-redis/_static/image8.png)

如果您是想要查看傳送訊息至 Redis，您可以使用**redis cli**用戶端，會使用 Redis 進行安裝。

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
