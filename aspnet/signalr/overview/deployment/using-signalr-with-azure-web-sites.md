---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: 使用 Azure App Service 中的 Web 應用程式中的 SignalR |Microsoft 文件
author: pfletcher
description: 本文件說明如何設定 Microsoft Azure 執行的 SignalR 應用程式。 教學課程中的軟體版本可用，Visual Studio 2013 或 Vis....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043203"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="d8865-104">使用 Azure App Service 中的 Web 應用程式中的 SignalR</span><span class="sxs-lookup"><span data-stu-id="d8865-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="d8865-105">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d8865-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="d8865-106">本文件說明如何設定 Microsoft Azure 執行的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d8865-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d8865-107">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="d8865-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d8865-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)或 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d8865-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="d8865-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d8865-109">.NET 4.5</span></span>
> - <span data-ttu-id="d8865-110">SignalR 第 2 版</span><span class="sxs-lookup"><span data-stu-id="d8865-110">SignalR version 2</span></span>
> - <span data-ttu-id="d8865-111">適用於 Visual Studio 2013 或 2012年的 azure SDK 2.3</span><span class="sxs-lookup"><span data-stu-id="d8865-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d8865-112">問題和註解</span><span class="sxs-lookup"><span data-stu-id="d8865-112">Questions and comments</span></span>
> 
> <span data-ttu-id="d8865-113">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="d8865-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d8865-114">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)， [StackOverflow.com](http://stackoverflow.com/)，或[Microsoft Azure 論壇](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="d8865-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="d8865-115">目錄</span><span class="sxs-lookup"><span data-stu-id="d8865-115">Table of Contents</span></span>

- [<span data-ttu-id="d8865-116">簡介</span><span class="sxs-lookup"><span data-stu-id="d8865-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="d8865-117">將 SignalR Web 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d8865-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="d8865-118">Azure App Service 上啟用 WebSockets</span><span class="sxs-lookup"><span data-stu-id="d8865-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="d8865-119">使用 Azure Redis 快取後擋板</span><span class="sxs-lookup"><span data-stu-id="d8865-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="d8865-120">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d8865-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="d8865-121">簡介</span><span class="sxs-lookup"><span data-stu-id="d8865-121">Introduction</span></span>

<span data-ttu-id="d8865-122">ASP.NET SignalR 可用來將新的層級的伺服器和 web 或.NET 用戶端之間的互動性。</span><span class="sxs-lookup"><span data-stu-id="d8865-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="d8865-123">時在 Azure 中裝載，SignalR 應用程式可以充分利用高可用性的可擴充、 高效能環境，以及在雲端中執行提供。</span><span class="sxs-lookup"><span data-stu-id="d8865-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="d8865-124">將 SignalR Web 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d8865-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="d8865-125">SignalR 不加入任何特定的複雜應用程式部署到 Azure 與部署至內部部署伺服器。</span><span class="sxs-lookup"><span data-stu-id="d8865-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="d8865-126">使用 SignalR 的應用程式可以裝載在 Azure 中沒有任何設定或其他設定變更 (透過 WebSockets 的支援，請參閱[啟用 WebSockets Azure App Service 上](#websocket)下方。)本教學課程中，您會將部署應用程式中建立[入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)至 Azure。</span><span class="sxs-lookup"><span data-stu-id="d8865-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="d8865-127">**必要條件**</span><span class="sxs-lookup"><span data-stu-id="d8865-127">**Prerequisites**</span></span>

- <span data-ttu-id="d8865-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d8865-128">Visual Studio 2013.</span></span> <span data-ttu-id="d8865-129">如果您沒有 Visual Studio，Visual Studio 2013 Express for Web 包含在 Azure sdk 安裝。</span><span class="sxs-lookup"><span data-stu-id="d8865-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="d8865-130">[Visual Studio 2013 的 azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)或[for Visual Studio 2012 的 Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)。</span><span class="sxs-lookup"><span data-stu-id="d8865-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="d8865-131">若要完成本教學課程，您需要 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="d8865-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="d8865-132">您可以[啟動您的 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，或[註冊試用訂用帳戶](https://azure.microsoft.com/pricing/free-trial/)。</span><span class="sxs-lookup"><span data-stu-id="d8865-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="d8865-133">將 SignalR web 應用程式部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="d8865-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="d8865-134">完成[入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載已完成的專案，從[Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。</span><span class="sxs-lookup"><span data-stu-id="d8865-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="d8865-135">在 Visual Studio 中，選取**建置**，**發行 SignalR 聊天**。</span><span class="sxs-lookup"><span data-stu-id="d8865-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="d8865-136">在 [發行網站] 對話方塊中，選取 「 Windows Azure 網站 」。</span><span class="sxs-lookup"><span data-stu-id="d8865-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![選取 Azure Web Sites](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="d8865-138">如果您未登入您的 Microsoft 帳戶，按一下**登入...** "選取現有的網站 」 對話方塊中，然後在登入。</span><span class="sxs-lookup"><span data-stu-id="d8865-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![選取現有的網站](using-signalr-with-azure-web-sites/_static/image2.png)    ![登入 Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="d8865-141">在 選取現有的網站 」 對話方塊中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="d8865-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![新網站](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="d8865-143">在 「 Windows Azure 上的建立網站 」 對話方塊中，輸入唯一的應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="d8865-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="d8865-144">在區域下拉式清單中選取最接近您的區域。</span><span class="sxs-lookup"><span data-stu-id="d8865-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="d8865-145">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="d8865-145">Click **Create**.</span></span>

    ![在 Azure 上建立站台](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="d8865-147">在 發行網站 對話方塊中，按一下 **發行**。</span><span class="sxs-lookup"><span data-stu-id="d8865-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![發佈站台](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="d8865-149">應用程式已完成發行，就會在瀏覽器中開啟 SignalR 交談應用程式裝載於 Azure App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d8865-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![在瀏覽器中開啟站台](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="d8865-151">在 Azure App Service Web 應用程式上啟用 WebSockets</span><span class="sxs-lookup"><span data-stu-id="d8865-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="d8865-152">WebSockets 必須明確啟用 web 應用程式中使用中的 SignalR 應用程式中;否則，會使用其他通訊協定 (請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="d8865-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="d8865-153">若要在 Azure App Service Web 應用程式上使用 WebSockets，web 應用程式的組態區段中啟用它。</span><span class="sxs-lookup"><span data-stu-id="d8865-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="d8865-154">若要這樣做，請開啟 在 web 應用程式[Azure 管理入口網站](https://manage.windowsazure.com/)，並選取 [設定]。</span><span class="sxs-lookup"><span data-stu-id="d8865-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Configure (設定) 索引標籤](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="d8865-156">在 [組態] 頁面頂端，確定.NET 4.5，用於您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d8865-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework 4.5 版設定](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="d8865-158">在 [組態] 頁面中**WebSockets**設定中，選取**上**。</span><span class="sxs-lookup"><span data-stu-id="d8865-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![WebSockets 設定： 上](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="d8865-160">在 [組態] 頁面底部，選取**儲存**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="d8865-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![儲存設定](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="d8865-162">使用 Azure Redis 快取後擋板</span><span class="sxs-lookup"><span data-stu-id="d8865-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="d8865-163">如果您使用 web 應用程式的多個執行個體，而這些執行個體的使用者需要彼此互動 （如此，比方說，在一個執行個體中建立的交談訊息可達到使用者連接到其他執行個體），則[Azure Redis 快取後擋板](../performance/scaleout-with-redis.md)必須在您的應用程式中實作。</span><span class="sxs-lookup"><span data-stu-id="d8865-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="d8865-164">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d8865-164">Next Steps</span></span>

<span data-ttu-id="d8865-165">如需有關 Azure App Service 中的 Web 應用程式的詳細資訊，請參閱[Web 應用程式的概觀](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)。</span><span class="sxs-lookup"><span data-stu-id="d8865-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
