---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: SignalR 向外延展與 Azure 服務匯流排 (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: bcaff2f371c757bc819bc44b9b3bd296b085a688
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380029"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="d93fd-102">SignalR 向外延展與 Azure 服務匯流排 (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d93fd-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d93fd-103">藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d93fd-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="d93fd-104">在本教學課程中，您將部署至 Windows Azure Web 角色，使用服務匯流排後擋板以將訊息分散至每個角色執行個體的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d93fd-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="d93fd-105">必要條件：</span><span class="sxs-lookup"><span data-stu-id="d93fd-105">Prerequisites:</span></span>

- <span data-ttu-id="d93fd-106">Windows Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="d93fd-106">A Windows Azure account.</span></span>
- <span data-ttu-id="d93fd-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="d93fd-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="d93fd-108">Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="d93fd-108">Visual Studio 2012.</span></span>

<span data-ttu-id="d93fd-109">服務匯流排後擋板還有相容[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)，1.1 版。</span><span class="sxs-lookup"><span data-stu-id="d93fd-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="d93fd-110">不過，不相容 1.0 版的 Service Bus for Windows Server。</span><span class="sxs-lookup"><span data-stu-id="d93fd-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="d93fd-111">Pricing</span><span class="sxs-lookup"><span data-stu-id="d93fd-111">Pricing</span></span>

<span data-ttu-id="d93fd-112">服務匯流排後擋板會用來傳送郵件主題。</span><span class="sxs-lookup"><span data-stu-id="d93fd-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="d93fd-113">最新價格資訊，請參閱[服務匯流排](https://azure.microsoft.com/pricing/details/service-bus/)。</span><span class="sxs-lookup"><span data-stu-id="d93fd-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="d93fd-114">在撰寫本文時，您可以傳送每個月的 1,000,000 訊息小於 $ 1。</span><span class="sxs-lookup"><span data-stu-id="d93fd-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="d93fd-115">後擋板傳送 SignalR 中樞方法的每次叫用服務匯流排訊息。</span><span class="sxs-lookup"><span data-stu-id="d93fd-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="d93fd-116">另外還有一些控制項的訊息連線、 中斷連接、 加入或離開群組，等等。</span><span class="sxs-lookup"><span data-stu-id="d93fd-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="d93fd-117">在大部分的應用程式，大部分的訊息流量會中樞方法叫用。</span><span class="sxs-lookup"><span data-stu-id="d93fd-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="d93fd-118">總覽</span><span class="sxs-lookup"><span data-stu-id="d93fd-118">Overview</span></span>

<span data-ttu-id="d93fd-119">我們會詳細的教學課程之前，以下是您將進行的快速概觀。</span><span class="sxs-lookup"><span data-stu-id="d93fd-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d93fd-120">您可以使用 Windows Azure 入口網站來建立新的服務匯流排命名空間。</span><span class="sxs-lookup"><span data-stu-id="d93fd-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="d93fd-121">將這些 NuGet 套件新增至您的應用程式中：</span><span class="sxs-lookup"><span data-stu-id="d93fd-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d93fd-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="d93fd-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d93fd-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="d93fd-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="d93fd-124">建立 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d93fd-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="d93fd-125">若要設定的後擋板的 Global.asax 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d93fd-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="d93fd-126">每個應用程式，「 YourAppName"挑選不同的值。</span><span class="sxs-lookup"><span data-stu-id="d93fd-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="d93fd-127">請勿在多個應用程式使用相同的值。</span><span class="sxs-lookup"><span data-stu-id="d93fd-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="d93fd-128">建立 Azure 服務</span><span class="sxs-lookup"><span data-stu-id="d93fd-128">Create the Azure Services</span></span>

<span data-ttu-id="d93fd-129">建立雲端服務中所述[如何建立和部署雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)。</span><span class="sxs-lookup"><span data-stu-id="d93fd-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="d93fd-130">請依照下列章節中的步驟 「 如何： 建立雲端服務，使用 快速建立 」。</span><span class="sxs-lookup"><span data-stu-id="d93fd-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="d93fd-131">本教學課程中，您不需要上傳憑證。</span><span class="sxs-lookup"><span data-stu-id="d93fd-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="d93fd-132">建立新的服務匯流排命名空間中所述[如何使用服務匯流排主題/訂閱到](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。</span><span class="sxs-lookup"><span data-stu-id="d93fd-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="d93fd-133">請遵循 < 建立服務命名空間 > 一節的步驟。</span><span class="sxs-lookup"><span data-stu-id="d93fd-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="d93fd-134">請確定選取的雲端服務和服務匯流排命名空間相同的區域。</span><span class="sxs-lookup"><span data-stu-id="d93fd-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="d93fd-135">建立 Visual Studio 專案</span><span class="sxs-lookup"><span data-stu-id="d93fd-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="d93fd-136">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d93fd-136">Start Visual Studio.</span></span> <span data-ttu-id="d93fd-137">從**檔案**功能表上，按一下**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="d93fd-138">在 **新的專案**對話方塊方塊中，展開**Visual C#**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="d93fd-139">底下**已安裝的範本**，選取**雲端**，然後選取**Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="d93fd-140">保留預設.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="d93fd-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="d93fd-141">應用程式 ChatService 命名，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="d93fd-142">在 [**新的 Windows Azure 雲端服務**] 對話方塊中，選取 ASP.NET MVC 4 Web 角色。</span><span class="sxs-lookup"><span data-stu-id="d93fd-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="d93fd-143">按一下向右箭號按鈕 (**&gt;**) 若要將角色新增至您的方案。</span><span class="sxs-lookup"><span data-stu-id="d93fd-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="d93fd-144">滑鼠停留在新的角色，因此顯示的鉛筆圖示。</span><span class="sxs-lookup"><span data-stu-id="d93fd-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="d93fd-145">按一下此圖示以重新命名角色。</span><span class="sxs-lookup"><span data-stu-id="d93fd-145">Click this icon to rename the role.</span></span> <span data-ttu-id="d93fd-146">角色命名為"SignalRChat 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="d93fd-147">在 **新的 ASP.NET MVC 4 專案**精靈中，選取**網際網路應用程式**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="d93fd-148">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="d93fd-148">Click **OK**.</span></span> <span data-ttu-id="d93fd-149">[專案] 精靈會建立兩個專案：</span><span class="sxs-lookup"><span data-stu-id="d93fd-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="d93fd-150">ChatService： 此專案是 Windows Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d93fd-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="d93fd-151">它會定義 Azure 角色和其他組態選項。</span><span class="sxs-lookup"><span data-stu-id="d93fd-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="d93fd-152">SignalRChat： 此專案是您的 ASP.NET MVC 4 專案。</span><span class="sxs-lookup"><span data-stu-id="d93fd-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="d93fd-153">建立 SignalR 聊天應用程式</span><span class="sxs-lookup"><span data-stu-id="d93fd-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="d93fd-154">若要建立交談應用程式，請遵循本教學課程[開始使用 SignalR 和 MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="d93fd-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="d93fd-155">使用 NuGet 來安裝必要的程式庫。</span><span class="sxs-lookup"><span data-stu-id="d93fd-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="d93fd-156">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d93fd-157">在 [ **Package Manager Console** ] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="d93fd-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="d93fd-158">使用`-ProjectName`將套件安裝至 ASP.NET MVC 專案中，而不是 Windows Azure 專案的選項。</span><span class="sxs-lookup"><span data-stu-id="d93fd-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="d93fd-159">設定後擋板</span><span class="sxs-lookup"><span data-stu-id="d93fd-159">Configure the Backplane</span></span>

<span data-ttu-id="d93fd-160">在您的應用程式 Global.asax 檔案中新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d93fd-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="d93fd-161">現在，您需要取得服務匯流排連接字串。</span><span class="sxs-lookup"><span data-stu-id="d93fd-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="d93fd-162">在 Azure 入口網站中，選取您建立的服務匯流排命名空間，並按一下 [存取金鑰] 圖示。</span><span class="sxs-lookup"><span data-stu-id="d93fd-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="d93fd-163">將連接字串複製到剪貼簿，然後將它貼至*connectionString*變數。</span><span class="sxs-lookup"><span data-stu-id="d93fd-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="d93fd-164">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="d93fd-164">Deploy to Azure</span></span>

<span data-ttu-id="d93fd-165">在 [方案總管] 中，展開**角色**ChatService 專案內的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d93fd-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="d93fd-166">以滑鼠右鍵按一下 SignalRChat 角色，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="d93fd-167">選取 [**組態**] 索引標籤。底下**執行個體**選取 [2]。</span><span class="sxs-lookup"><span data-stu-id="d93fd-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="d93fd-168">您也可以在設定的 VM 大小**超小型**。</span><span class="sxs-lookup"><span data-stu-id="d93fd-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="d93fd-169">儲存變更。</span><span class="sxs-lookup"><span data-stu-id="d93fd-169">Save the changes.</span></span>

<span data-ttu-id="d93fd-170">在 [方案總管] 中，以滑鼠右鍵按一下 ChatService 專案。</span><span class="sxs-lookup"><span data-stu-id="d93fd-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="d93fd-171">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="d93fd-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="d93fd-172">如果這是您第一次的時間發行至 Windows Azure 時，您必須下載您的認證。</span><span class="sxs-lookup"><span data-stu-id="d93fd-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="d93fd-173">在 [**發佈**精靈] 中，按一下 [登入以下載認證]。</span><span class="sxs-lookup"><span data-stu-id="d93fd-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="d93fd-174">這會提示您登入 Windows Azure 入口網站並下載發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="d93fd-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="d93fd-175">按一下 **匯入**，然後選取您所下載發佈設定檔。</span><span class="sxs-lookup"><span data-stu-id="d93fd-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="d93fd-176">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="d93fd-176">Click **Next**.</span></span> <span data-ttu-id="d93fd-177">在 [**發佈設定**] 對話方塊底下**雲端服務**，選取您稍早建立的雲端服務。</span><span class="sxs-lookup"><span data-stu-id="d93fd-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="d93fd-178">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="d93fd-178">Click **Publish**.</span></span> <span data-ttu-id="d93fd-179">可能需要幾分鐘的時間來部署應用程式，並啟動 Vm。</span><span class="sxs-lookup"><span data-stu-id="d93fd-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="d93fd-180">現在當您執行交談應用程式時，角色執行個體透過使用服務匯流排主題的 Azure 服務匯流排通訊。</span><span class="sxs-lookup"><span data-stu-id="d93fd-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="d93fd-181">主題是訊息佇列，可讓多個訂閱者。</span><span class="sxs-lookup"><span data-stu-id="d93fd-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="d93fd-182">後擋板會自動建立主題和訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="d93fd-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="d93fd-183">若要查看訂用帳戶和訊息活動，開啟 Azure 入口網站、 選取的服務匯流排命名空間，然後按一下 「 主題 」。</span><span class="sxs-lookup"><span data-stu-id="d93fd-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="d93fd-184">它可能需費時幾分鐘的時間才會出現在儀表板中的訊息 」 活動。</span><span class="sxs-lookup"><span data-stu-id="d93fd-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="d93fd-185">SignalR 管理主題的存留期。</span><span class="sxs-lookup"><span data-stu-id="d93fd-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="d93fd-186">只要您的應用程式部署時，請勿嘗試以手動方式刪除主題或主題上變更設定。</span><span class="sxs-lookup"><span data-stu-id="d93fd-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
