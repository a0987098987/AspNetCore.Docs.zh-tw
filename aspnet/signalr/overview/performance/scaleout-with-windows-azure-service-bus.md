---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR 範圍外使用 Azure 服務匯流排 |Microsoft 文件
author: MikeWasson
description: 此主題的 Visual Studio 2013.NET 4.5 SignalR 版本 2 舊版的此主題的 SignalR 1.x 本主題的版本，用於軟體版本...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Azure 服務匯流排與 SignalR 範圍外
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

在本教學課程中，您將部署到 Windows Azure Web 角色，使用服務匯流排後擋板散發每個角色執行個體訊息的 SignalR 應用程式。 (您也可以使用服務匯流排背板[web 應用程式在 Azure App Service 中的](https://docs.microsoft.com/azure/app-service-web/)。)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

必要條件：

- Windows Azure 帳戶。
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)。
- Visual Studio 2012 或 2013年。

服務匯流排後擋板也是與相容[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)，1.1 版。 不過，不相容 1.0 版的 Service Bus for Windows Server。

## <a name="pricing"></a>Pricing

服務匯流排後擋板用來傳送訊息的主題。 最新定價的資訊，請參閱[Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)。 在撰寫本文時，您可以傳送每個月 1000000 訊息小於 $ 1。 後擋板傳送 SignalR 中樞方法的每次叫用的服務匯流排訊息。 也有某些控制項的訊息連線、 中斷連接、 聯結或讓群組和其他等等。 大多數的應用程式中大部分的訊息流量將中樞方法叫用。

## <a name="overview"></a>總覽

我們可以詳細的教學課程之前，以下是您將執行的快速概觀。

1. 使用 Windows Azure 入口網站建立新的服務匯流排命名空間。
2. 將這些 NuGet 封裝加入至您的應用程式： 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)或[Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. 建立 SignalR 應用程式。
4. 設定後擋板 Startup.cs 中加入下列程式碼： 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

此程式碼使用的預設值，來設定後擋板[TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx)和[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)。 如需變更這些值的資訊，請參閱[SignalR 效能： 向外延展度量](signalr-performance.md#scaleout_metrics)。

每個應用程式，挑選不同值的"YourAppName"。 請勿在多個應用程式使用相同的值。

## <a name="create-the-azure-services"></a>建立 Azure 服務

中所述，建立雲端服務，[如何建立及部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。 請依照下列章節中的步驟 「 如何： 建立雲端服務，使用 快速建立 」。 此教學課程中，您不需要上傳憑證。

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

建立新的服務匯流排命名空間中所述[如何使用服務匯流排主題/訂閱以](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。 遵循 「 建立服務命名空間 」 一節的步驟。

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> 請確定選取的雲端服務和服務匯流排命名空間的相同區域。


## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

啟動 Visual Studio。 從**檔案**功能表上，按一下 **新專案**。

在**新專案**對話方塊方塊中，展開  **Visual C#**。 在下**已安裝的範本**，選取**雲端**，然後選取  **Windows Azure 雲端服務**。 保留預設的.NET Framework 4.5。 將應用程式 ChatService，然後按一下**確定**。

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

在**新的 Windows Azure 雲端服務** 對話方塊中，選取 ASP.NET Web 角色。 按一下向右箭號按鈕 (**&gt;**) 若要將角色加入至方案。

將滑鼠放在新的角色，因此可見的鉛筆圖示。 按一下此圖示以將角色重新命名。 將"SignalRChat 」 角色，然後按一下**確定**。

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

在**新增 ASP.NET 專案**對話方塊中，選取**MVC**，按一下 [確定]。

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

專案精靈會建立兩個專案：

- ChatService： 此專案是 Windows Azure 應用程式。 它會定義 Azure 角色和其他組態選項。
- SignalRChat： 此專案是您的 ASP.NET MVC 5 專案。

## <a name="create-the-signalr-chat-application"></a>建立 SignalR 交談應用程式

若要建立交談應用程式，請遵循教學課程中[SignalR 和 MVC 5 入門](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。

您可以使用 NuGet 來安裝必要的程式庫。 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在**Package Manager Console**視窗中，輸入下列命令：

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

使用`-ProjectName`選項來安裝 ASP.NET MVC 專案中，而不是 Windows Azure 專案的封裝。

## <a name="configure-the-backplane"></a>設定後擋板

在您的應用程式 Startup.cs 檔案中，加入下列程式碼：

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

現在您需要取得您的服務匯流排連接字串。 在 Azure 入口網站中，選取您建立服務匯流排命名空間，然後按一下 [存取金鑰] 圖示。

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

將連接字串複製到剪貼簿，然後將它貼入*connectionString*變數。

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>部署至 Azure

在 [方案總管] 中，展開**角色**ChatService 專案內的資料夾。

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

以滑鼠右鍵按一下 SignalRChat 角色，然後選取**屬性**。 選取**組態** 索引標籤。在下**執行個體**選取 2。 您也可以設定 VM 大小**超小型**。

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

儲存變更。

在 [方案總管] 中，以滑鼠右鍵按一下 ChatService 專案。 選取 [發行]。

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

如果這是您第一次發行至 Windows Azure，您必須下載您的認證。 在**發行**精靈 中，按一下 「 登入以下載認證 」。 這會提示您登入 Windows Azure 入口網站並下載發行設定檔。

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

按一下**匯入**並選取您下載的發行設定檔案。

按 [ **下一步**]。 在**發行設定**對話方塊下方**雲端服務**，選取您稍早建立的雲端服務。

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

按一下 [發行] 。 可能需要幾分鐘才能部署應用程式，並啟動 Vm。

現在當您執行交談應用程式，透過 Azure 服務匯流排，使用服務匯流排主題進行通訊的角色執行個體。 主題是訊息佇列，可讓多個訂閱者。

後擋板會自動建立主題和訂用帳戶。 若要查看的訂閱和訊息活動，開啟 Azure 入口網站、 選取的服務匯流排命名空間，然後按一下 「 主題 」。

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

它讓需要幾分鐘才會出現在儀表板中的訊息 」 活動。

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR 管理主題存留期。 只要您的應用程式部署時，不要嘗試手動刪除的主題或主題上變更設定。

## <a name="troubleshooting"></a>疑難排解

**System.InvalidOperationException 「 唯一支援的 IsolationLevel 」 'IsolationLevel.Serializable'。**

如果以外作業的交易層級設定為其他項目，會發生此錯誤`Serializable`。 請確認與其他交易層級正在執行任何作業。
