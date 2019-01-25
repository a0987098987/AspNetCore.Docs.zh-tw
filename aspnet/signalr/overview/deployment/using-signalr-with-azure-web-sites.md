---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: 使用 SignalR 和 Web 應用程式，Azure App Service 中 |Microsoft Docs
author: bradygaster
description: 本文件說明如何設定 Microsoft Azure 執行的 SignalR 應用程式。 軟體版本會用於本教學課程，Visual Studio 2013 或 vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 13eb5d29a2c40f52aed4b569ec8695f014a05f03
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837698"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>使用 SignalR 和 Azure App Service 中的 Web 應用程式
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本文件說明如何設定 Microsoft Azure 執行的 SignalR 應用程式。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)或 Visual Studio 2012
> - .NET 4.5
> - SignalR 第 2 版
> - Azure SDK 2.3，適用於 Visual Studio 2013 或 2012
>
>
>
> ## <a name="questions-and-comments"></a>提出問題或意見
>
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)， [StackOverflow.com](http://stackoverflow.com/)，或[Microsoft Azure 論壇](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>目錄

- [簡介](#introduction)
- [SignalR Web 應用程式部署至 Azure App Service](#deploying)
- [Azure App Service 上啟用 WebSockets](#websocket)
- [使用 Azure Redis 快取後擋板](#backplane)
- [後續步驟](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>簡介

ASP.NET SignalR 可用來將新的層級的伺服器和 web 或.NET 用戶端之間的互動性。 當裝載在 Azure 中，SignalR 應用程式可以利用高可用性、 可調整的並在雲端中執行提供的高效能環境。

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>SignalR Web 應用程式部署至 Azure App Service

SignalR 不加入任何特定的複雜應用程式部署到 Azure 與部署至內部部署伺服器。 使用 SignalR 的應用程式可以裝載在 Azure 中，而不用變更任何組態或其他設定 (透過 WebSockets 的支援，請參閱[Azure App Service 上的 啟用 Websocket](#websocket)下方。)本教學課程中，您會將建立的應用程式部署[入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)至 Azure。

**必要條件**

- Visual Studio 2013. 如果您沒有 Visual Studio，Visual Studio 2013 Express for Web 隨附於 Azure SDK 安裝中。
- [Visual Studio 2013 的 azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)或是[適用於 Visual Studio 2012 的 Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)。
- 若要完成本教學課程中，您必須有 Azure 訂用帳戶。 您可以[啟用您的 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，或[註冊試用版訂用帳戶](https://azure.microsoft.com/pricing/free-trial/)。

### <a name="deploying-a-signalr-web-app-to-azure"></a>將 SignalR web 應用程式部署至 Azure

1. 完成[入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載已完成的專案，從[程式碼庫](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。
2. 在 Visual Studio 中，選取**建置**，**發佈 SignalR 聊天**。
3. 在 [發行網站] 對話方塊中，選取 「 Windows Azure 網站 」。

    ![選取 Azure 網站](using-signalr-with-azure-web-sites/_static/image1.png)
4. 如果您未登入您的 Microsoft 帳戶，按一下 **登入...** 中的 選取現有網站 對話方塊中和登入。

    ![選取現有的網站](using-signalr-with-azure-web-sites/_static/image2.png)    ![登入 Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. 在 [選取現有網站] 對話方塊中，按一下**新增**。

    ![新網站](using-signalr-with-azure-web-sites/_static/image4.png)
6. 在 [Windows Azure 上的建立站台] 對話方塊中，輸入唯一的應用程式名稱。 在區域下拉式清單中選取最接近您的區域。 按一下 [建立] 。

    ![在 Azure 上建立站台](using-signalr-with-azure-web-sites/_static/image5.png)
7. 在 [發行網站] 對話方塊中，按一下**發佈**。

    ![發佈站台](using-signalr-with-azure-web-sites/_static/image6.png)
8. 當應用程式完成發佈時，裝載於 Azure App Service Web Apps 中的 SignalR 聊天應用程式會開啟瀏覽器中。

    ![在瀏覽器中開啟的站台](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>在 Azure App Service Web Apps 上啟用 WebSockets

Websocket 必須明確啟用 web 應用程式中使用中的 SignalR 應用程式中;否則，將會使用其他通訊協定 (請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)如需詳細資訊)。

若要在 Azure App Service Web Apps 上使用 WebSockets，web 應用程式的組態中啟用它。 若要這樣做，請開啟 在 web 應用程式[Azure 管理入口網站](https://manage.windowsazure.com/)，並選取 [設定]。

![Configure (設定) 索引標籤](using-signalr-with-azure-web-sites/_static/image8.png)

在 [組態] 頁面頂端，確定.NET 4.5 用於 web 應用程式。

![.NET framework 4.5 版設定](using-signalr-with-azure-web-sites/_static/image9.png)

在 [設定] 頁面中**WebSockets**設定中，選取**上**。

![Websocket 的設定：開啟](using-signalr-with-azure-web-sites/_static/image10.png)

在 [組態] 頁面底部，選取**儲存**以儲存變更。

![儲存設定](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>使用 Azure Redis 快取後擋板

如果您使用 web 應用程式的多個執行個體，而這些執行個體的使用者需要 （因此，比方說，在一個執行個體中建立的交談訊息可以觸達使用者連接到其他執行個體） 與對方進行互動， [Azure Redis 快取後擋板](../performance/scaleout-with-redis.md)必須在您的應用程式中實作。

<a id="nextsteps"></a>
## <a name="next-steps"></a>後續步驟

如需有關 Azure App Service 中的 Web 應用程式的詳細資訊，請參閱 < [Web Apps 概觀](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)。
