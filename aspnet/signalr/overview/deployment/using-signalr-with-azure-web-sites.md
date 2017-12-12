---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "使用 Azure App Service 中的 Web 應用程式中的 SignalR |Microsoft 文件"
author: pfletcher
description: "本文件說明如何設定 Microsoft Azure 執行的 SignalR 應用程式。 教學課程中的軟體版本可用，Visual Studio 2013 或 Vis...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 414701159b4e1fa3da9597503b14281a1e9991de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>使用 Azure App Service 中的 Web 應用程式中的 SignalR
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本文件說明如何設定 Microsoft Azure 執行的 SignalR 應用程式。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)或 Visual Studio 2012
> - .NET 4.5
> - SignalR 第 2 版
> - 適用於 Visual Studio 2013 或 2012年的 azure SDK 2.3
>   
> 
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)， [StackOverflow.com](http://stackoverflow.com/)，或[Microsoft Azure 論壇](https://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>目錄

- [簡介](#introduction)
- [將 SignalR Web 應用程式部署至 Azure App Service](#deploying)
- [Azure App Service 上啟用 WebSockets](#websocket)
- [使用 Azure Redis 快取後擋板](#backplane)
- [接下來的步驟](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>簡介

ASP.NET SignalR 可用來將新的層級的伺服器和 web 或.NET 用戶端之間的互動性。 時在 Azure 中裝載，SignalR 應用程式可以充分利用高可用性的可擴充、 高效能環境，以及在雲端中執行提供。

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>將 SignalR Web 應用程式部署至 Azure App Service

SignalR 不加入任何特定的複雜應用程式部署到 Azure 與部署至內部部署伺服器。 使用 SignalR 的應用程式可以裝載在 Azure 中沒有任何設定或其他設定變更 (透過 WebSockets 的支援，請參閱[啟用 WebSockets Azure App Service 上](#websocket)下方。)本教學課程中，您會將部署應用程式中建立[入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)至 Azure。

**必要條件**

- Visual Studio 2013。 如果您沒有 Visual Studio，Visual Studio 2013 Express for Web 包含在 Azure sdk 安裝。
- [Visual Studio 2013 的 azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)或[for Visual Studio 2012 的 Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)。
- 若要完成本教學課程，您需要 Azure 訂用帳戶。 您可以[啟動您的 MSDN 訂閱者權益](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/)，或[註冊試用訂用帳戶](https://azure.microsoft.com/en-us/pricing/free-trial/)。

### <a name="deploying-a-signalr-web-app-to-azure"></a>將 SignalR web 應用程式部署至 Azure

1. 完成[入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載已完成的專案，從[Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。
2. 在 Visual Studio 中，選取**建置**，**發行 SignalR 聊天**。
3. 在 [發行網站] 對話方塊中，選取 「 Windows Azure 網站 」。

    ![選取 Azure Web Sites](using-signalr-with-azure-web-sites/_static/image1.png)
4. 如果您未登入您的 Microsoft 帳戶，按一下**登入...** "選取現有的網站 」 對話方塊中，然後在登入。

    ![選取現有的網站](using-signalr-with-azure-web-sites/_static/image2.png)    ![登入 Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. 在 選取現有的網站 」 對話方塊中，按一下 **新增**。

    ![新網站](using-signalr-with-azure-web-sites/_static/image4.png)
6. 在 「 Windows Azure 上的建立網站 」 對話方塊中，輸入唯一的應用程式名稱。 在區域下拉式清單中選取最接近您的區域。 按一下 [建立] 。

    ![在 Azure 上建立站台](using-signalr-with-azure-web-sites/_static/image5.png)
7. 在 發行網站 對話方塊中，按一下 **發行**。

    ![發佈站台](using-signalr-with-azure-web-sites/_static/image6.png)
8. 應用程式已完成發行，就會在瀏覽器中開啟 SignalR 交談應用程式裝載於 Azure App Service Web 應用程式。

    ![在瀏覽器中開啟站台](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>在 Azure App Service Web 應用程式上啟用 WebSockets

WebSockets 必須明確啟用 web 應用程式中使用中的 SignalR 應用程式中;否則，會使用其他通訊協定 (請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)如需詳細資訊)。

若要在 Azure App Service Web 應用程式上使用 WebSockets，web 應用程式的組態區段中啟用它。 若要這樣做，請開啟 在 web 應用程式[Azure 管理入口網站](https://manage.windowsazure.com/)，並選取 [設定]。

![Configure (設定) 索引標籤](using-signalr-with-azure-web-sites/_static/image8.png)

在 [組態] 頁面頂端，確定.NET 4.5，用於您的 web 應用程式。

![.NET framework 4.5 版設定](using-signalr-with-azure-web-sites/_static/image9.png)

在 [組態] 頁面中**WebSockets**設定中，選取**上**。

![WebSockets 設定： 上](using-signalr-with-azure-web-sites/_static/image10.png)

在 [組態] 頁面底部，選取**儲存**以儲存變更。

![儲存設定](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>使用 Azure Redis 快取後擋板

如果您使用 web 應用程式的多個執行個體，而這些執行個體的使用者需要彼此互動 （如此，比方說，在一個執行個體中建立的交談訊息可達到使用者連接到其他執行個體），則[Azure Redis 快取後擋板](../performance/scaleout-with-redis.md)必須在您的應用程式中實作。

<a id="nextsteps"></a>
## <a name="next-steps"></a>後續步驟

如需有關 Azure App Service 中的 Web 應用程式的詳細資訊，請參閱[Web 應用程式的概觀](https://azure.microsoft.com/en-us/documentation/articles/app-service-web-overview/)。
