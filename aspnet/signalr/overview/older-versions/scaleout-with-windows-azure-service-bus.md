---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Azure 服務匯流排與 SignalR 範圍外 (SignalR 1.x) |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036436"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="51b33-102">Azure 服務匯流排與 SignalR 範圍外 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="51b33-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="51b33-103">由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="51b33-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="51b33-104">在本教學課程中，您將部署到 Windows Azure Web 角色，使用服務匯流排後擋板散發每個角色執行個體訊息的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="51b33-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="51b33-105">必要條件：</span><span class="sxs-lookup"><span data-stu-id="51b33-105">Prerequisites:</span></span>

- <span data-ttu-id="51b33-106">Windows Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="51b33-106">A Windows Azure account.</span></span>
- <span data-ttu-id="51b33-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="51b33-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="51b33-108">Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="51b33-108">Visual Studio 2012.</span></span>

<span data-ttu-id="51b33-109">服務匯流排後擋板也是與相容[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)，1.1 版。</span><span class="sxs-lookup"><span data-stu-id="51b33-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="51b33-110">不過，不相容 1.0 版的 Service Bus for Windows Server。</span><span class="sxs-lookup"><span data-stu-id="51b33-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="51b33-111">Pricing</span><span class="sxs-lookup"><span data-stu-id="51b33-111">Pricing</span></span>

<span data-ttu-id="51b33-112">服務匯流排後擋板用來傳送訊息的主題。</span><span class="sxs-lookup"><span data-stu-id="51b33-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="51b33-113">最新定價的資訊，請參閱[Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)。</span><span class="sxs-lookup"><span data-stu-id="51b33-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="51b33-114">在撰寫本文時，您可以傳送每個月 1000000 訊息小於 $ 1。</span><span class="sxs-lookup"><span data-stu-id="51b33-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="51b33-115">後擋板傳送 SignalR 中樞方法的每次叫用的服務匯流排訊息。</span><span class="sxs-lookup"><span data-stu-id="51b33-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="51b33-116">也有某些控制項的訊息連線、 中斷連接、 聯結或讓群組和其他等等。</span><span class="sxs-lookup"><span data-stu-id="51b33-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="51b33-117">大多數的應用程式中大部分的訊息流量將中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="51b33-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="51b33-118">總覽</span><span class="sxs-lookup"><span data-stu-id="51b33-118">Overview</span></span>

<span data-ttu-id="51b33-119">我們可以詳細的教學課程之前，以下是您將執行的快速概觀。</span><span class="sxs-lookup"><span data-stu-id="51b33-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="51b33-120">使用 Windows Azure 入口網站建立新的服務匯流排命名空間。</span><span class="sxs-lookup"><span data-stu-id="51b33-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="51b33-121">將這些 NuGet 封裝加入至您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="51b33-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="51b33-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="51b33-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="51b33-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="51b33-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="51b33-124">建立 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="51b33-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="51b33-125">將下列程式碼加入至 Global.asax 設定後擋板：</span><span class="sxs-lookup"><span data-stu-id="51b33-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="51b33-126">每個應用程式，挑選不同值的"YourAppName"。</span><span class="sxs-lookup"><span data-stu-id="51b33-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="51b33-127">請勿在多個應用程式使用相同的值。</span><span class="sxs-lookup"><span data-stu-id="51b33-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="51b33-128">建立 Azure 服務</span><span class="sxs-lookup"><span data-stu-id="51b33-128">Create the Azure Services</span></span>

<span data-ttu-id="51b33-129">中所述，建立雲端服務，[如何建立及部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。</span><span class="sxs-lookup"><span data-stu-id="51b33-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="51b33-130">請依照下列章節中的步驟 「 如何： 建立雲端服務，使用 快速建立 」。</span><span class="sxs-lookup"><span data-stu-id="51b33-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="51b33-131">此教學課程中，您不需要上傳憑證。</span><span class="sxs-lookup"><span data-stu-id="51b33-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="51b33-132">建立新的服務匯流排命名空間中所述[如何使用服務匯流排主題/訂閱以](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。</span><span class="sxs-lookup"><span data-stu-id="51b33-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="51b33-133">遵循 「 建立服務命名空間 」 一節的步驟。</span><span class="sxs-lookup"><span data-stu-id="51b33-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="51b33-134">請確定選取的雲端服務和服務匯流排命名空間的相同區域。</span><span class="sxs-lookup"><span data-stu-id="51b33-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="51b33-135">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="51b33-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="51b33-136">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="51b33-136">Start Visual Studio.</span></span> <span data-ttu-id="51b33-137">從**檔案**功能表上，按一下 **新專案**。</span><span class="sxs-lookup"><span data-stu-id="51b33-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="51b33-138">在**新專案**對話方塊方塊中，展開  **Visual C#**。</span><span class="sxs-lookup"><span data-stu-id="51b33-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="51b33-139">在下**已安裝的範本**，選取**雲端**，然後選取  **Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="51b33-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="51b33-140">保留預設的.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="51b33-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="51b33-141">將應用程式 ChatService，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="51b33-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="51b33-142">在**新的 Windows Azure 雲端服務** 對話方塊中，選取 ASP.NET MVC 4 Web 角色。</span><span class="sxs-lookup"><span data-stu-id="51b33-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="51b33-143">按一下向右箭號按鈕 (**&gt;**) 若要將角色加入至方案。</span><span class="sxs-lookup"><span data-stu-id="51b33-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="51b33-144">將滑鼠放在新的角色，因此可見的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="51b33-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="51b33-145">按一下此圖示以將角色重新命名。</span><span class="sxs-lookup"><span data-stu-id="51b33-145">Click this icon to rename the role.</span></span> <span data-ttu-id="51b33-146">將"SignalRChat 」 角色，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="51b33-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="51b33-147">在**新增 ASP.NET MVC 4 專案**精靈中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="51b33-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="51b33-148">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="51b33-148">Click **OK**.</span></span> <span data-ttu-id="51b33-149">專案精靈會建立兩個專案：</span><span class="sxs-lookup"><span data-stu-id="51b33-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="51b33-150">ChatService： 此專案是 Windows Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="51b33-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="51b33-151">它會定義 Azure 角色和其他組態選項。</span><span class="sxs-lookup"><span data-stu-id="51b33-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="51b33-152">SignalRChat： 此專案是您的 ASP.NET MVC 4 專案。</span><span class="sxs-lookup"><span data-stu-id="51b33-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="51b33-153">建立 SignalR 交談應用程式</span><span class="sxs-lookup"><span data-stu-id="51b33-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="51b33-154">若要建立交談應用程式，請遵循教學課程中[SignalR 和 MVC 4 入門](tutorial-getting-started-with-signalr-and-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="51b33-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="51b33-155">您可以使用 NuGet 來安裝必要的程式庫。</span><span class="sxs-lookup"><span data-stu-id="51b33-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="51b33-156">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="51b33-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="51b33-157">在**Package Manager Console**視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="51b33-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="51b33-158">使用`-ProjectName`選項來安裝 ASP.NET MVC 專案中，而不是 Windows Azure 專案的封裝。</span><span class="sxs-lookup"><span data-stu-id="51b33-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="51b33-159">設定後擋板</span><span class="sxs-lookup"><span data-stu-id="51b33-159">Configure the Backplane</span></span>

<span data-ttu-id="51b33-160">在您的應用程式 Global.asax 檔案中，加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="51b33-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="51b33-161">現在您需要取得您的服務匯流排連接字串。</span><span class="sxs-lookup"><span data-stu-id="51b33-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="51b33-162">在 Azure 入口網站中，選取您建立服務匯流排命名空間，然後按一下 [存取金鑰] 圖示。</span><span class="sxs-lookup"><span data-stu-id="51b33-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="51b33-163">將連接字串複製到剪貼簿，然後將它貼入*connectionString*變數。</span><span class="sxs-lookup"><span data-stu-id="51b33-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="51b33-164">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="51b33-164">Deploy to Azure</span></span>

<span data-ttu-id="51b33-165">在 [方案總管] 中，展開**角色**ChatService 專案內的資料夾。</span><span class="sxs-lookup"><span data-stu-id="51b33-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="51b33-166">以滑鼠右鍵按一下 SignalRChat 角色，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="51b33-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="51b33-167">選取**組態** 索引標籤。在下**執行個體**選取 2。</span><span class="sxs-lookup"><span data-stu-id="51b33-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="51b33-168">您也可以設定 VM 大小**超小型**。</span><span class="sxs-lookup"><span data-stu-id="51b33-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="51b33-169">儲存變更。</span><span class="sxs-lookup"><span data-stu-id="51b33-169">Save the changes.</span></span>

<span data-ttu-id="51b33-170">在 [方案總管] 中，以滑鼠右鍵按一下 ChatService 專案。</span><span class="sxs-lookup"><span data-stu-id="51b33-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="51b33-171">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="51b33-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="51b33-172">如果這是您第一次發行至 Windows Azure，您必須下載您的認證。</span><span class="sxs-lookup"><span data-stu-id="51b33-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="51b33-173">在**發行**精靈 中，按一下 「 登入以下載認證 」。</span><span class="sxs-lookup"><span data-stu-id="51b33-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="51b33-174">這會提示您登入 Windows Azure 入口網站並下載發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="51b33-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="51b33-175">按一下**匯入**並選取您下載的發行設定檔案。</span><span class="sxs-lookup"><span data-stu-id="51b33-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="51b33-176">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="51b33-176">Click **Next**.</span></span> <span data-ttu-id="51b33-177">在**發行設定**對話方塊下方**雲端服務**，選取您稍早建立的雲端服務。</span><span class="sxs-lookup"><span data-stu-id="51b33-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="51b33-178">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="51b33-178">Click **Publish**.</span></span> <span data-ttu-id="51b33-179">可能需要幾分鐘才能部署應用程式，並啟動 Vm。</span><span class="sxs-lookup"><span data-stu-id="51b33-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="51b33-180">現在當您執行交談應用程式，透過 Azure 服務匯流排，使用服務匯流排主題進行通訊的角色執行個體。</span><span class="sxs-lookup"><span data-stu-id="51b33-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="51b33-181">主題是訊息佇列，可讓多個訂閱者。</span><span class="sxs-lookup"><span data-stu-id="51b33-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="51b33-182">後擋板會自動建立主題和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="51b33-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="51b33-183">若要查看的訂閱和訊息活動，開啟 Azure 入口網站、 選取的服務匯流排命名空間，然後按一下 「 主題 」。</span><span class="sxs-lookup"><span data-stu-id="51b33-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="51b33-184">它讓需要幾分鐘才會出現在儀表板中的訊息 」 活動。</span><span class="sxs-lookup"><span data-stu-id="51b33-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="51b33-185">SignalR 管理主題存留期。</span><span class="sxs-lookup"><span data-stu-id="51b33-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="51b33-186">只要您的應用程式部署時，不要嘗試手動刪除的主題或主題上變更設定。</span><span class="sxs-lookup"><span data-stu-id="51b33-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
