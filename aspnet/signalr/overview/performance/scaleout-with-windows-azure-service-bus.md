---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR 向外延展與 Azure 服務匯流排 |Microsoft Docs
author: MikeWasson
description: 此主題的 Visual Studio 2013.NET 4.5 SignalR 版本中使用的軟體版本 2 本主題中，此主題的 SignalR 1.x 版的舊版...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b87eb9f2df82d92c07ea0c86873849a44660e5c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830531"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="18399-103">SignalR 向外延展與 Azure 服務匯流排</span><span class="sxs-lookup"><span data-stu-id="18399-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="18399-104">藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="18399-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="18399-105">在本教學課程中，您將部署至 Windows Azure Web 角色，使用服務匯流排後擋板以將訊息分散至每個角色執行個體的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18399-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="18399-106">(您也可以使用服務匯流排後的擋板[web 應用程式在 Azure App Service 中的](https://docs.microsoft.com/azure/app-service-web/)。)</span><span class="sxs-lookup"><span data-stu-id="18399-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="18399-107">必要條件：</span><span class="sxs-lookup"><span data-stu-id="18399-107">Prerequisites:</span></span>

- <span data-ttu-id="18399-108">Windows Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="18399-108">A Windows Azure account.</span></span>
- <span data-ttu-id="18399-109">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="18399-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="18399-110">Visual Studio 2012 或 2013年。</span><span class="sxs-lookup"><span data-stu-id="18399-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="18399-111">服務匯流排後擋板還有相容[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)，1.1 版。</span><span class="sxs-lookup"><span data-stu-id="18399-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="18399-112">不過，不相容 1.0 版的 Service Bus for Windows Server。</span><span class="sxs-lookup"><span data-stu-id="18399-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="18399-113">Pricing</span><span class="sxs-lookup"><span data-stu-id="18399-113">Pricing</span></span>

<span data-ttu-id="18399-114">服務匯流排後擋板會用來傳送郵件主題。</span><span class="sxs-lookup"><span data-stu-id="18399-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="18399-115">最新價格資訊，請參閱[服務匯流排](https://azure.microsoft.com/pricing/details/service-bus/)。</span><span class="sxs-lookup"><span data-stu-id="18399-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="18399-116">在撰寫本文時，您可以傳送每個月的 1,000,000 訊息小於 $ 1。</span><span class="sxs-lookup"><span data-stu-id="18399-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="18399-117">後擋板傳送 SignalR 中樞方法的每次叫用服務匯流排訊息。</span><span class="sxs-lookup"><span data-stu-id="18399-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="18399-118">另外還有一些控制項的訊息連線、 中斷連接、 加入或離開群組，等等。</span><span class="sxs-lookup"><span data-stu-id="18399-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="18399-119">在大部分的應用程式，大部分的訊息流量會中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="18399-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="18399-120">總覽</span><span class="sxs-lookup"><span data-stu-id="18399-120">Overview</span></span>

<span data-ttu-id="18399-121">我們會詳細的教學課程之前，以下是您將進行的快速概觀。</span><span class="sxs-lookup"><span data-stu-id="18399-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="18399-122">您可以使用 Windows Azure 入口網站來建立新的服務匯流排命名空間。</span><span class="sxs-lookup"><span data-stu-id="18399-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="18399-123">將這些 NuGet 套件新增至您的應用程式中：</span><span class="sxs-lookup"><span data-stu-id="18399-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="18399-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="18399-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="18399-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)或[Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="18399-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="18399-126">建立 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18399-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="18399-127">若要設定的後擋板的 Startup.cs 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="18399-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="18399-128">此程式碼會使用的預設值來設定的後擋板[TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx)並[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)。</span><span class="sxs-lookup"><span data-stu-id="18399-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="18399-129">如需變更這些值的詳細資訊，請參閱[SignalR 效能： 向外延展計量](signalr-performance.md#scaleout_metrics)。</span><span class="sxs-lookup"><span data-stu-id="18399-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="18399-130">每個應用程式，「 YourAppName"挑選不同的值。</span><span class="sxs-lookup"><span data-stu-id="18399-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="18399-131">請勿在多個應用程式使用相同的值。</span><span class="sxs-lookup"><span data-stu-id="18399-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="18399-132">建立 Azure 服務</span><span class="sxs-lookup"><span data-stu-id="18399-132">Create the Azure Services</span></span>

<span data-ttu-id="18399-133">建立雲端服務中所述[如何建立和部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。</span><span class="sxs-lookup"><span data-stu-id="18399-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="18399-134">請依照下列章節中的步驟 「 如何： 建立雲端服務，使用 快速建立 」。</span><span class="sxs-lookup"><span data-stu-id="18399-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="18399-135">本教學課程中，您不需要上傳憑證。</span><span class="sxs-lookup"><span data-stu-id="18399-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="18399-136">建立新的服務匯流排命名空間中所述[如何使用服務匯流排主題/訂閱到](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。</span><span class="sxs-lookup"><span data-stu-id="18399-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="18399-137">請遵循 < 建立服務命名空間 > 一節的步驟。</span><span class="sxs-lookup"><span data-stu-id="18399-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="18399-138">請確定選取的雲端服務和服務匯流排命名空間相同的區域。</span><span class="sxs-lookup"><span data-stu-id="18399-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="18399-139">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="18399-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="18399-140">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="18399-140">Start Visual Studio.</span></span> <span data-ttu-id="18399-141">從**檔案**功能表上，按一下**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="18399-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="18399-142">在 **新的專案**對話方塊方塊中，展開**Visual C#**。</span><span class="sxs-lookup"><span data-stu-id="18399-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="18399-143">底下**已安裝的範本**，選取**雲端**，然後選取**Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="18399-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="18399-144">保留預設.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="18399-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="18399-145">應用程式 ChatService 命名，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="18399-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="18399-146">在 [**新的 Windows Azure 雲端服務**] 對話方塊中，選取 ASP.NET Web 角色。</span><span class="sxs-lookup"><span data-stu-id="18399-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="18399-147">按一下向右箭號按鈕 (**&gt;**) 若要將角色新增至您的方案。</span><span class="sxs-lookup"><span data-stu-id="18399-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="18399-148">滑鼠停留在新的角色，因此顯示的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="18399-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="18399-149">按一下此圖示以重新命名角色。</span><span class="sxs-lookup"><span data-stu-id="18399-149">Click this icon to rename the role.</span></span> <span data-ttu-id="18399-150">角色命名為"SignalRChat 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="18399-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="18399-151">在 **新的 ASP.NET 專案**對話方塊中，選取**MVC**，按一下 確定。</span><span class="sxs-lookup"><span data-stu-id="18399-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="18399-152">[專案] 精靈會建立兩個專案：</span><span class="sxs-lookup"><span data-stu-id="18399-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="18399-153">ChatService： 此專案是 Windows Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="18399-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="18399-154">它會定義 Azure 角色和其他組態選項。</span><span class="sxs-lookup"><span data-stu-id="18399-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="18399-155">SignalRChat： 此專案是您的 ASP.NET MVC 5 專案。</span><span class="sxs-lookup"><span data-stu-id="18399-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="18399-156">建立 SignalR 聊天應用程式</span><span class="sxs-lookup"><span data-stu-id="18399-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="18399-157">若要建立交談應用程式，請遵循本教學課程[開始使用 SignalR 和 MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="18399-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="18399-158">使用 NuGet 來安裝必要的程式庫。</span><span class="sxs-lookup"><span data-stu-id="18399-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="18399-159">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="18399-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="18399-160">在 [ **Package Manager Console** ] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="18399-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="18399-161">使用`-ProjectName`將套件安裝至 ASP.NET MVC 專案中，而不是 Windows Azure 專案的選項。</span><span class="sxs-lookup"><span data-stu-id="18399-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="18399-162">設定後擋板</span><span class="sxs-lookup"><span data-stu-id="18399-162">Configure the Backplane</span></span>

<span data-ttu-id="18399-163">在您的應用程式的 Startup.cs 檔案中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="18399-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="18399-164">現在，您需要取得服務匯流排連接字串。</span><span class="sxs-lookup"><span data-stu-id="18399-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="18399-165">在 Azure 入口網站中，選取您建立的服務匯流排命名空間，並按一下 [存取金鑰] 圖示。</span><span class="sxs-lookup"><span data-stu-id="18399-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="18399-166">將連接字串複製到剪貼簿，然後將它貼至*connectionString*變數。</span><span class="sxs-lookup"><span data-stu-id="18399-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="18399-167">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="18399-167">Deploy to Azure</span></span>

<span data-ttu-id="18399-168">在 [方案總管] 中，展開**角色**ChatService 專案內的資料夾。</span><span class="sxs-lookup"><span data-stu-id="18399-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="18399-169">以滑鼠右鍵按一下 SignalRChat 角色，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="18399-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="18399-170">選取 [**組態**] 索引標籤。底下**執行個體**選取 [2]。</span><span class="sxs-lookup"><span data-stu-id="18399-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="18399-171">您也可以在設定的 VM 大小**超小型**。</span><span class="sxs-lookup"><span data-stu-id="18399-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="18399-172">儲存變更。</span><span class="sxs-lookup"><span data-stu-id="18399-172">Save the changes.</span></span>

<span data-ttu-id="18399-173">在 [方案總管] 中，以滑鼠右鍵按一下 ChatService 專案。</span><span class="sxs-lookup"><span data-stu-id="18399-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="18399-174">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="18399-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="18399-175">如果這是您第一次的時間發行至 Windows Azure 時，您必須下載您的認證。</span><span class="sxs-lookup"><span data-stu-id="18399-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="18399-176">在 [**發佈**精靈] 中，按一下 [登入以下載認證]。</span><span class="sxs-lookup"><span data-stu-id="18399-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="18399-177">這會提示您登入 Windows Azure 入口網站並下載發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="18399-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="18399-178">按一下 **匯入**，然後選取您所下載發佈設定檔。</span><span class="sxs-lookup"><span data-stu-id="18399-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="18399-179">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="18399-179">Click **Next**.</span></span> <span data-ttu-id="18399-180">在 [**發佈設定**] 對話方塊底下**雲端服務**，選取您稍早建立的雲端服務。</span><span class="sxs-lookup"><span data-stu-id="18399-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="18399-181">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="18399-181">Click **Publish**.</span></span> <span data-ttu-id="18399-182">可能需要幾分鐘的時間來部署應用程式，並啟動 Vm。</span><span class="sxs-lookup"><span data-stu-id="18399-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="18399-183">現在當您執行交談應用程式時，角色執行個體透過使用服務匯流排主題的 Azure 服務匯流排通訊。</span><span class="sxs-lookup"><span data-stu-id="18399-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="18399-184">主題是訊息佇列，可讓多個訂閱者。</span><span class="sxs-lookup"><span data-stu-id="18399-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="18399-185">後擋板會自動建立主題和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="18399-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="18399-186">若要查看訂用帳戶和訊息活動，開啟 Azure 入口網站、 選取的服務匯流排命名空間，然後按一下 「 主題 」。</span><span class="sxs-lookup"><span data-stu-id="18399-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="18399-187">它可能需費時幾分鐘的時間才會出現在儀表板中的訊息 」 活動。</span><span class="sxs-lookup"><span data-stu-id="18399-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="18399-188">SignalR 管理主題的存留期。</span><span class="sxs-lookup"><span data-stu-id="18399-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="18399-189">只要您的應用程式部署時，請勿嘗試以手動方式刪除主題或主題上變更設定。</span><span class="sxs-lookup"><span data-stu-id="18399-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="18399-190">疑難排解</span><span class="sxs-lookup"><span data-stu-id="18399-190">Troubleshooting</span></span>

<span data-ttu-id="18399-191">**System.InvalidOperationException 「 唯一支援的 IsolationLevel 是 'IsolationLevel.Serializable' 」。**</span><span class="sxs-lookup"><span data-stu-id="18399-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="18399-192">如果以外的其他作業的交易層級設定為項目，會發生此錯誤`Serializable`。</span><span class="sxs-lookup"><span data-stu-id="18399-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="18399-193">請確認與其他交易層級正在執行任何作業。</span><span class="sxs-lookup"><span data-stu-id="18399-193">Verify that no operations are being performed with other transaction levels.</span></span>
