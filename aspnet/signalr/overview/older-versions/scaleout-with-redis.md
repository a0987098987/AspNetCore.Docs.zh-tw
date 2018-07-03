---
uid: signalr/overview/older-versions/scaleout-with-redis
title: SignalR 向外延展與 Redis (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2214ce5b4e064b60fa3230c3ae7351ef2654706a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362073"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="b7d4d-102">SignalR 向外延展與 Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b7d4d-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b7d4d-103">藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b7d4d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="b7d4d-104">在本教學課程中，您將使用[Redis](http://redis.io/)將訊息散發到部署兩個個別的 IIS 執行個體的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="b7d4d-105">Redis 是記憶體中索引鍵-值存放區。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="b7d4d-106">它也支援發行/訂閱模型的傳訊系統。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="b7d4d-107">Redis 的 SignalR 後擋板會使用發佈/訂閱功能，將訊息轉送到其他伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="b7d4d-108">本教學課程中，您會使用三部伺服器：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="b7d4d-109">執行 Windows，您將使用的 SignalR 應用程式部署的兩部伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="b7d4d-110">執行 Linux，您將用來執行 Redis 的一部伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="b7d4d-111">在本教學課程的螢幕擷取畫面，我會使用 Ubuntu 12.04 TLS。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="b7d4d-112">如果您沒有要使用的三個實體伺服器，您可以建立在 HYPER-V 上的 Vm。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="b7d4d-113">另一個選項是在 Azure 上建立 Vm。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="b7d4d-114">雖然本教學課程使用官方 Redis 實作，另外還有[Windows 連接埠的 Redis](https://github.com/MSOpenTech/redis)從 MSOpenTech。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="b7d4d-115">安裝和設定不同，但除此以外步驟都相同。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b7d4d-116">SignalR 向外的延展與 Redis 不支援 Redis 叢集。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="b7d4d-117">總覽</span><span class="sxs-lookup"><span data-stu-id="b7d4d-117">Overview</span></span>

<span data-ttu-id="b7d4d-118">我們會詳細的教學課程之前，以下是您將進行的快速概觀。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="b7d4d-119">安裝 Redis 並啟動 Redis 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="b7d4d-120">將這些 NuGet 套件新增至您的應用程式中：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="b7d4d-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="b7d4d-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="b7d4d-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="b7d4d-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="b7d4d-123">建立 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="b7d4d-124">若要設定的後擋板的 Global.asax 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="b7d4d-125">HYPER-V 上的 Ubuntu</span><span class="sxs-lookup"><span data-stu-id="b7d4d-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="b7d4d-126">使用 Windows HYPER-V，可以在 Windows Server 上輕鬆建立 Ubuntu VM。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="b7d4d-127">下載從 Ubuntu ISO [ http://www.ubuntu.com ](http://www.ubuntu.com/)。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="b7d4d-128">在 HYPER-V 中新增新的 VM。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="b7d4d-129">在 **連接虛擬硬碟**步驟中，選取**建立虛擬硬碟**。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="b7d4d-130">在 **安裝選項**步驟中，選取**映像檔 (.iso)**，按一下 **瀏覽**，並瀏覽至 Ubuntu 安裝 ISO。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="b7d4d-131">安裝 Redis</span><span class="sxs-lookup"><span data-stu-id="b7d4d-131">Install Redis</span></span>

<span data-ttu-id="b7d4d-132">請遵循的步驟[ http://redis.io/download ](http://redis.io/download)來下載並建置 Redis。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="b7d4d-133">這會建置 Redis 二進位檔`src`目錄。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="b7d4d-134">根據預設，Redis 不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="b7d4d-135">若要設定密碼，請編輯`redis.conf`位於原始碼的根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="b7d4d-136">（請將檔案的備份副本才能進行編輯時） ！新增下列指示詞到`redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="b7d4d-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="b7d4d-137">現在開始 Redis 伺服器：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="b7d4d-138">開啟連接埠 6379，也就是 Redis 的預設通訊埠接聽時。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="b7d4d-139">（您可以變更組態檔中的連接埠號碼）。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="b7d4d-140">建立 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="b7d4d-140">Create the SignalR Application</span></span>

<span data-ttu-id="b7d4d-141">依照下列其中一個這些教學課程中建立的 SignalR 應用程式：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="b7d4d-142">開始使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="b7d4d-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="b7d4d-143">開始使用 SignalR 和 MVC 4</span><span class="sxs-lookup"><span data-stu-id="b7d4d-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="b7d4d-144">接下來，我們將修改的聊天應用程式，以支援向外的延展與 Redis。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="b7d4d-145">首先，將 SignalR.Redis NuGet 封裝加入您的專案。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="b7d4d-146">在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b7d4d-147">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="b7d4d-148">接下來，開啟 Global.asax 檔案。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="b7d4d-149">將下列程式碼加入**應用程式\_啟動**方法：</span><span class="sxs-lookup"><span data-stu-id="b7d4d-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="b7d4d-150">「 伺服器 」 是正在執行的 Redis 伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="b7d4d-151">*連接埠*的通訊埠編號</span><span class="sxs-lookup"><span data-stu-id="b7d4d-151">*port* is the port number</span></span>
- <span data-ttu-id="b7d4d-152">「 密碼 」 是指您在 redis.conf 檔案中定義的密碼。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="b7d4d-153">:"AppName"是任何字串。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-153">"AppName" is any string.</span></span> <span data-ttu-id="b7d4d-154">SignalR 使用此名稱建立 Redis pub/sub 通道。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="b7d4d-155">例如: </span><span class="sxs-lookup"><span data-stu-id="b7d4d-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="b7d4d-156">部署和執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b7d4d-156">Deploy and Run the Application</span></span>

<span data-ttu-id="b7d4d-157">準備您的 Windows Server 執行個體，若要部署的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="b7d4d-158">新增 IIS 角色。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-158">Add the IIS role.</span></span> <span data-ttu-id="b7d4d-159">包含 「 應用程式開發 」 功能，包括 WebSocket 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="b7d4d-160">也包含 「 管理 」 服務 （列在 [管理工具]）。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="b7d4d-161">**安裝 Web Deploy 3.0。**</span><span class="sxs-lookup"><span data-stu-id="b7d4d-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="b7d4d-162">當您執行 IIS 管理員，它會提示您安裝 Microsoft Web Platform，或者您可以[下載 intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="b7d4d-163">在 平台安裝程式中，搜尋 Web Deploy，並安裝 Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="b7d4d-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="b7d4d-164">檢查 Web 管理服務正在執行。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="b7d4d-165">否則，請啟動服務。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-165">If not, start the service.</span></span> <span data-ttu-id="b7d4d-166">（如果您沒有看到 Web 管理服務中的 Windows 服務清單，請確定您新增 IIS 角色時，安裝 「 管理服務。）</span><span class="sxs-lookup"><span data-stu-id="b7d4d-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="b7d4d-167">根據預設，Web 管理服務會接聽 TCP 連接埠 8172。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="b7d4d-168">在 [Windows 防火牆] 中，建立新的輸入的規則，以允許連接埠 8172 上的 TCP 流量。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="b7d4d-169">如需詳細資訊，請參閱 <<c0> [ 設定防火牆規則](https://technet.microsoft.com/library/dd448559(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="b7d4d-170">（如果您裝載在 Azure 上的 Vm，您可以直接在 Azure 入口網站中。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="b7d4d-171">請參閱[如何設定虛擬機器的端點](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)。)</span><span class="sxs-lookup"><span data-stu-id="b7d4d-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="b7d4d-172">現在，您已準備好要部署到伺服器的 從您的開發電腦的 Visual Studio 專案。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="b7d4d-173">在 方案總管 中，以滑鼠右鍵按一下方案，然後按一下 **發佈**。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="b7d4d-174">如需詳細的 web 部署的相關文件，請參閱[Visual Studio 及 ASP.NET 的 Web 部署內容對應](../../../whitepapers/aspnet-web-deployment-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="b7d4d-175">如果您部署兩部伺服器應用程式時，您可以在另一個瀏覽器視窗開啟每個執行個體，並查看他們都收到來自其他使用 SignalR 訊息。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="b7d4d-176">（當然，在生產環境中，兩部伺服器會位於負載平衡器後方。）</span><span class="sxs-lookup"><span data-stu-id="b7d4d-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="b7d4d-177">如果您是想要查看傳送訊息至 Redis，您可以使用**redis cli**用戶端，會使用 Redis 進行安裝。</span><span class="sxs-lookup"><span data-stu-id="b7d4d-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
