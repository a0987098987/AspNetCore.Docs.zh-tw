---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: "SignalR 範圍外使用 Azure 服務匯流排 |Microsoft 文件"
author: MikeWasson
description: "此主題的 Visual Studio 2013.NET 4.5 SignalR 版本 2 舊版的此主題的 SignalR 1.x 本主題的版本，用於軟體版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 857fc8baa61549e2fabbb8da012b1fa23950237d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="3433d-103">Azure 服務匯流排與 SignalR 範圍外</span><span class="sxs-lookup"><span data-stu-id="3433d-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="3433d-104">由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3433d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="3433d-105">在本教學課程中，您將部署到 Windows Azure Web 角色，使用服務匯流排後擋板散發每個角色執行個體訊息的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3433d-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="3433d-106">(您也可以使用服務匯流排背板[web 應用程式在 Azure App Service 中的](https://docs.microsoft.com/azure/app-service-web/)。)</span><span class="sxs-lookup"><span data-stu-id="3433d-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="3433d-107">必要條件：</span><span class="sxs-lookup"><span data-stu-id="3433d-107">Prerequisites:</span></span>

- <span data-ttu-id="3433d-108">Windows Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3433d-108">A Windows Azure account.</span></span>
- <span data-ttu-id="3433d-109">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="3433d-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="3433d-110">Visual Studio 2012 或 2013年。</span><span class="sxs-lookup"><span data-stu-id="3433d-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="3433d-111">服務匯流排後擋板也是與相容[Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx)，1.1 版。</span><span class="sxs-lookup"><span data-stu-id="3433d-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="3433d-112">不過，不相容 1.0 版的 Service Bus for Windows Server。</span><span class="sxs-lookup"><span data-stu-id="3433d-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="3433d-113">Pricing</span><span class="sxs-lookup"><span data-stu-id="3433d-113">Pricing</span></span>

<span data-ttu-id="3433d-114">服務匯流排後擋板用來傳送訊息的主題。</span><span class="sxs-lookup"><span data-stu-id="3433d-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="3433d-115">最新定價的資訊，請參閱[Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)。</span><span class="sxs-lookup"><span data-stu-id="3433d-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="3433d-116">在撰寫本文時，您可以傳送每個月 1000000 訊息小於 $ 1。</span><span class="sxs-lookup"><span data-stu-id="3433d-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="3433d-117">後擋板傳送 SignalR 中樞方法的每次叫用的服務匯流排訊息。</span><span class="sxs-lookup"><span data-stu-id="3433d-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="3433d-118">也有某些控制項的訊息連線、 中斷連接、 聯結或讓群組和其他等等。</span><span class="sxs-lookup"><span data-stu-id="3433d-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="3433d-119">大多數的應用程式中大部分的訊息流量將中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="3433d-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="3433d-120">概觀</span><span class="sxs-lookup"><span data-stu-id="3433d-120">Overview</span></span>

<span data-ttu-id="3433d-121">我們可以詳細的教學課程之前，以下是您將執行的快速概觀。</span><span class="sxs-lookup"><span data-stu-id="3433d-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="3433d-122">使用 Windows Azure 入口網站建立新的服務匯流排命名空間。</span><span class="sxs-lookup"><span data-stu-id="3433d-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="3433d-123">將這些 NuGet 封裝加入至您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="3433d-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="3433d-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="3433d-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="3433d-125">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="3433d-125">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="3433d-126">建立 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3433d-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="3433d-127">設定後擋板 Startup.cs 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="3433d-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="3433d-128">此程式碼使用的預設值，來設定後擋板[TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx)和[MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)。</span><span class="sxs-lookup"><span data-stu-id="3433d-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="3433d-129">如需變更這些值的資訊，請參閱[SignalR 效能： 向外延展度量](signalr-performance.md#scaleout_metrics)。</span><span class="sxs-lookup"><span data-stu-id="3433d-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="3433d-130">每個應用程式，挑選不同值的"YourAppName"。</span><span class="sxs-lookup"><span data-stu-id="3433d-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="3433d-131">請勿在多個應用程式使用相同的值。</span><span class="sxs-lookup"><span data-stu-id="3433d-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="3433d-132">建立 Azure 服務</span><span class="sxs-lookup"><span data-stu-id="3433d-132">Create the Azure Services</span></span>

<span data-ttu-id="3433d-133">中所述，建立雲端服務，[如何建立及部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。</span><span class="sxs-lookup"><span data-stu-id="3433d-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="3433d-134">請依照下列章節中的步驟 「 如何： 建立雲端服務，使用 快速建立 」。</span><span class="sxs-lookup"><span data-stu-id="3433d-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="3433d-135">此教學課程中，您不需要上傳憑證。</span><span class="sxs-lookup"><span data-stu-id="3433d-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="3433d-136">建立新的服務匯流排命名空間中所述[如何使用服務匯流排主題/訂閱以](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。</span><span class="sxs-lookup"><span data-stu-id="3433d-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="3433d-137">遵循 「 建立服務命名空間 」 一節的步驟。</span><span class="sxs-lookup"><span data-stu-id="3433d-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="3433d-138">請確定選取的雲端服務和服務匯流排命名空間的相同區域。</span><span class="sxs-lookup"><span data-stu-id="3433d-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="3433d-139">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="3433d-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="3433d-140">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="3433d-140">Start Visual Studio.</span></span> <span data-ttu-id="3433d-141">從**檔案**功能表上，按一下 **新專案**。</span><span class="sxs-lookup"><span data-stu-id="3433d-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="3433d-142">在**新專案**對話方塊方塊中，展開  **Visual C#**。</span><span class="sxs-lookup"><span data-stu-id="3433d-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="3433d-143">在下**已安裝的範本**，選取**雲端**，然後選取  **Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="3433d-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="3433d-144">保留預設的.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="3433d-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="3433d-145">將應用程式 ChatService，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="3433d-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="3433d-146">在**新的 Windows Azure 雲端服務** 對話方塊中，選取 ASP.NET Web 角色。</span><span class="sxs-lookup"><span data-stu-id="3433d-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="3433d-147">按一下向右箭號按鈕 (**&gt;**) 若要將角色加入至方案。</span><span class="sxs-lookup"><span data-stu-id="3433d-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="3433d-148">將滑鼠放在新的角色，因此可見的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="3433d-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="3433d-149">按一下此圖示以將角色重新命名。</span><span class="sxs-lookup"><span data-stu-id="3433d-149">Click this icon to rename the role.</span></span> <span data-ttu-id="3433d-150">將"SignalRChat 」 角色，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="3433d-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="3433d-151">在**新增 ASP.NET 專案**對話方塊中，選取**MVC**，按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="3433d-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="3433d-152">專案精靈會建立兩個專案：</span><span class="sxs-lookup"><span data-stu-id="3433d-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="3433d-153">ChatService： 此專案是 Windows Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3433d-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="3433d-154">它會定義 Azure 角色和其他組態選項。</span><span class="sxs-lookup"><span data-stu-id="3433d-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="3433d-155">SignalRChat： 此專案是您的 ASP.NET MVC 5 專案。</span><span class="sxs-lookup"><span data-stu-id="3433d-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="3433d-156">建立 SignalR 交談應用程式</span><span class="sxs-lookup"><span data-stu-id="3433d-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="3433d-157">若要建立交談應用程式，請遵循教學課程中[SignalR 和 MVC 5 入門](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="3433d-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="3433d-158">您可以使用 NuGet 來安裝必要的程式庫。</span><span class="sxs-lookup"><span data-stu-id="3433d-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="3433d-159">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="3433d-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3433d-160">在**Package Manager Console**視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="3433d-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="3433d-161">使用`-ProjectName`選項來安裝 ASP.NET MVC 專案中，而不是 Windows Azure 專案的封裝。</span><span class="sxs-lookup"><span data-stu-id="3433d-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="3433d-162">設定後擋板</span><span class="sxs-lookup"><span data-stu-id="3433d-162">Configure the Backplane</span></span>

<span data-ttu-id="3433d-163">在您的應用程式 Startup.cs 檔案中，加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="3433d-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="3433d-164">現在您需要取得您的服務匯流排連接字串。</span><span class="sxs-lookup"><span data-stu-id="3433d-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="3433d-165">在 Azure 入口網站中，選取您建立服務匯流排命名空間，然後按一下 [存取金鑰] 圖示。</span><span class="sxs-lookup"><span data-stu-id="3433d-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="3433d-166">將連接字串複製到剪貼簿，然後將它貼入*connectionString*變數。</span><span class="sxs-lookup"><span data-stu-id="3433d-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="3433d-167">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="3433d-167">Deploy to Azure</span></span>

<span data-ttu-id="3433d-168">在 [方案總管] 中，展開**角色**ChatService 專案內的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3433d-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="3433d-169">以滑鼠右鍵按一下 SignalRChat 角色，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="3433d-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="3433d-170">選取**組態** 索引標籤。在下**執行個體**選取 2。</span><span class="sxs-lookup"><span data-stu-id="3433d-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="3433d-171">您也可以設定 VM 大小**超小型**。</span><span class="sxs-lookup"><span data-stu-id="3433d-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="3433d-172">儲存變更。</span><span class="sxs-lookup"><span data-stu-id="3433d-172">Save the changes.</span></span>

<span data-ttu-id="3433d-173">在 [方案總管] 中，以滑鼠右鍵按一下 ChatService 專案。</span><span class="sxs-lookup"><span data-stu-id="3433d-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="3433d-174">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="3433d-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="3433d-175">如果這是您第一次發行至 Windows Azure，您必須下載您的認證。</span><span class="sxs-lookup"><span data-stu-id="3433d-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="3433d-176">在**發行**精靈 中，按一下 「 登入以下載認證 」。</span><span class="sxs-lookup"><span data-stu-id="3433d-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="3433d-177">這會提示您登入 Windows Azure 入口網站並下載發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="3433d-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="3433d-178">按一下**匯入**並選取您下載的發行設定檔案。</span><span class="sxs-lookup"><span data-stu-id="3433d-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="3433d-179">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="3433d-179">Click **Next**.</span></span> <span data-ttu-id="3433d-180">在**發行設定**對話方塊下方**雲端服務**，選取您稍早建立的雲端服務。</span><span class="sxs-lookup"><span data-stu-id="3433d-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="3433d-181">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="3433d-181">Click **Publish**.</span></span> <span data-ttu-id="3433d-182">可能需要幾分鐘才能部署應用程式，並啟動 Vm。</span><span class="sxs-lookup"><span data-stu-id="3433d-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="3433d-183">現在當您執行交談應用程式，透過 Azure 服務匯流排，使用服務匯流排主題進行通訊的角色執行個體。</span><span class="sxs-lookup"><span data-stu-id="3433d-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="3433d-184">主題是訊息佇列，可讓多個訂閱者。</span><span class="sxs-lookup"><span data-stu-id="3433d-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="3433d-185">後擋板會自動建立主題和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="3433d-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="3433d-186">若要查看的訂閱和訊息活動，開啟 Azure 入口網站、 選取的服務匯流排命名空間，然後按一下 「 主題 」。</span><span class="sxs-lookup"><span data-stu-id="3433d-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="3433d-187">它讓需要幾分鐘才會出現在儀表板中的訊息 」 活動。</span><span class="sxs-lookup"><span data-stu-id="3433d-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="3433d-188">SignalR 管理主題存留期。</span><span class="sxs-lookup"><span data-stu-id="3433d-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="3433d-189">只要您的應用程式部署時，不要嘗試手動刪除的主題或主題上變更設定。</span><span class="sxs-lookup"><span data-stu-id="3433d-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3433d-190">疑難排解</span><span class="sxs-lookup"><span data-stu-id="3433d-190">Troubleshooting</span></span>

<span data-ttu-id="3433d-191">**System.InvalidOperationException 「 唯一支援的 IsolationLevel 」 'IsolationLevel.Serializable'。**</span><span class="sxs-lookup"><span data-stu-id="3433d-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="3433d-192">如果以外作業的交易層級設定為其他項目，會發生此錯誤`Serializable`。</span><span class="sxs-lookup"><span data-stu-id="3433d-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="3433d-193">請確認與其他交易層級正在執行任何作業。</span><span class="sxs-lookup"><span data-stu-id="3433d-193">Verify that no operations are being performed with other transaction levels.</span></span>
