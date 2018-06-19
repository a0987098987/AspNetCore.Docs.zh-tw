---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: 裝載 Azure 背景工作角色中的 OWIN |Microsoft 文件
author: MikeWasson
description: 本教學課程會示範如何在自我裝載 OWIN Microsoft Azure 背景工作角色中。 開啟 Web 介面的.NET (OWIN) 所定義的.NET web 伺服器之間的抽象概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868421"
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="40ef2-104">主機 OWIN 的 Azure 背景工作角色</span><span class="sxs-lookup"><span data-stu-id="40ef2-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="40ef2-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="40ef2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="40ef2-106">本教學課程會示範如何在自我裝載 OWIN Microsoft Azure 背景工作角色中。</span><span class="sxs-lookup"><span data-stu-id="40ef2-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="40ef2-107">[開啟適用於.NET 的 Web 介面](http://owin.org/)(OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="40ef2-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="40ef2-108">OWIN 以減少從 web 應用程式伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部 – 例如，在 Azure 工作者角色。</span><span class="sxs-lookup"><span data-stu-id="40ef2-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="40ef2-109">在此教學課程中，您將學習如何在自我裝載在 Microsoft Azure 背景工作角色內 OWIN 應用程式。</span><span class="sxs-lookup"><span data-stu-id="40ef2-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="40ef2-110">若要了解有關背景工作角色的詳細資訊，請參閱[Azure 執行模型](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices)。</span><span class="sxs-lookup"><span data-stu-id="40ef2-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="40ef2-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="40ef2-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="40ef2-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="40ef2-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="40ef2-113">Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="40ef2-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="40ef2-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="40ef2-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="40ef2-115">建立 Microsoft Azure 專案</span><span class="sxs-lookup"><span data-stu-id="40ef2-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="40ef2-116">啟動 Visual Studio 系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="40ef2-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="40ef2-117">若要偵錯應用程式在本機，使用 Azure 計算模擬器需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="40ef2-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="40ef2-118">在**檔案**功能表上，按一下 **新增**，然後按一下 **專案**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="40ef2-119">從**已安裝的範本**，在 Visual C# 中，按一下**雲端**，然後按一下  **Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="40ef2-120">將專案命名為"AzureApp 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="40ef2-121">在**新的 Windows Azure 雲端服務** 對話方塊中，按兩下**背景工作角色**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="40ef2-122">保留預設名稱 ("WorkerRole1")。</span><span class="sxs-lookup"><span data-stu-id="40ef2-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="40ef2-123">此步驟會將背景工作角色加入至方案。</span><span class="sxs-lookup"><span data-stu-id="40ef2-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="40ef2-124">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="40ef2-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="40ef2-125">建立 Visual Studio 方案包含兩個專案：</span><span class="sxs-lookup"><span data-stu-id="40ef2-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="40ef2-126">&quot;AzureApp&quot;定義的角色和 Azure 應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="40ef2-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="40ef2-127">&quot;WorkerRole1&quot;包含背景工作角色的程式碼。</span><span class="sxs-lookup"><span data-stu-id="40ef2-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="40ef2-128">一般情況下，Azure 應用程式都可以包含多個角色，但本教學課程使用的單一角色。</span><span class="sxs-lookup"><span data-stu-id="40ef2-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="40ef2-129">新增 OWIN 自我裝載套件</span><span class="sxs-lookup"><span data-stu-id="40ef2-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="40ef2-130">從**工具**功能表上，按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="40ef2-131">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="40ef2-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="40ef2-132">加入的 HTTP 端點</span><span class="sxs-lookup"><span data-stu-id="40ef2-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="40ef2-133">在 [方案總管] 中，展開 AzureApp 專案。</span><span class="sxs-lookup"><span data-stu-id="40ef2-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="40ef2-134">展開 角色 節點，以滑鼠右鍵按一下 WorkerRole1，並選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="40ef2-135">按一下**端點**，然後按一下 **加入端點**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="40ef2-136">在**通訊協定**下拉式清單中，選取 「 http 」。</span><span class="sxs-lookup"><span data-stu-id="40ef2-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="40ef2-137">在**公用連接埠**和**私用連接埠**，輸入 80。</span><span class="sxs-lookup"><span data-stu-id="40ef2-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="40ef2-138">這些連接埠號碼可能會不同。</span><span class="sxs-lookup"><span data-stu-id="40ef2-138">These port numbers can be different.</span></span> <span data-ttu-id="40ef2-139">公用連接埠是哪些用戶端時使用它們將要求傳送至角色。</span><span class="sxs-lookup"><span data-stu-id="40ef2-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="40ef2-140">建立 OWIN 啟動類別</span><span class="sxs-lookup"><span data-stu-id="40ef2-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="40ef2-141">在 方案總管 中，以滑鼠右鍵按一下 WorkerRole1 專案並選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="40ef2-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="40ef2-142">將類別命名為 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="40ef2-142">Name the class `Startup`.</span></span>

<span data-ttu-id="40ef2-143">以下列內容取代所有未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="40ef2-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="40ef2-144">`UseWelcomePage`擴充方法會將簡單的 HTML 頁面加入至您的應用程式，以確認站台運作。</span><span class="sxs-lookup"><span data-stu-id="40ef2-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="40ef2-145">啟動 OWIN 主機</span><span class="sxs-lookup"><span data-stu-id="40ef2-145">Start the OWIN Host</span></span>

<span data-ttu-id="40ef2-146">開啟 WorkerRole.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="40ef2-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="40ef2-147">這個類別會定義啟動及停止背景工作角色時執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="40ef2-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="40ef2-148">加入下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="40ef2-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="40ef2-149">新增**IDisposable**成員`WorkerRole`類別：</span><span class="sxs-lookup"><span data-stu-id="40ef2-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="40ef2-150">在`OnStart`方法，加入下列的程式碼，以啟動主機時：</span><span class="sxs-lookup"><span data-stu-id="40ef2-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="40ef2-151">**WebApp.Start**方法會啟動 OWIN 主機。</span><span class="sxs-lookup"><span data-stu-id="40ef2-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="40ef2-152">名稱`Startup`類別是方法的型別參數。</span><span class="sxs-lookup"><span data-stu-id="40ef2-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="40ef2-153">依照慣例，主機會呼叫`Configure`這個類別的方法。</span><span class="sxs-lookup"><span data-stu-id="40ef2-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="40ef2-154">覆寫`OnStop`來處置*\_應用程式*執行個體：</span><span class="sxs-lookup"><span data-stu-id="40ef2-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="40ef2-155">以下是 WorkerRole.cs 的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="40ef2-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="40ef2-156">建置方案，並按下 F5，在 Azure 計算模擬器在本機執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="40ef2-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="40ef2-157">根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。</span><span class="sxs-lookup"><span data-stu-id="40ef2-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="40ef2-158">計算模擬器會將本機 IP 位址指派給端點。</span><span class="sxs-lookup"><span data-stu-id="40ef2-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="40ef2-159">您可以藉由檢視計算模擬器 UI 中找到的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="40ef2-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="40ef2-160">在工作列通知區域中的模擬器圖示上按一下滑鼠右鍵，然後選取**顯示計算模擬器 UI**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="40ef2-161">尋找 IP 位址在服務部署、 部署 [識別碼]，服務詳細資料。</span><span class="sxs-lookup"><span data-stu-id="40ef2-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="40ef2-162">開啟網頁瀏覽器並瀏覽至 http://<em>位址</em>，其中<em>位址</em>是由計算模擬器; 指派的 IP 位址，例如`http://127.0.0.1:80`。</span><span class="sxs-lookup"><span data-stu-id="40ef2-162">Open a web browser and navigate to http://<em>address</em>, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="40ef2-163">您應該會看到 OWIN 歡迎頁面：</span><span class="sxs-lookup"><span data-stu-id="40ef2-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="40ef2-164">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="40ef2-164">Deploy to Azure</span></span>

<span data-ttu-id="40ef2-165">此步驟中，您必須有 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="40ef2-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="40ef2-166">如果您還沒有其中一個，您可以建立免費的試用帳戶只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="40ef2-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="40ef2-167">如需詳細資訊，請參閱[Microsoft Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="40ef2-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="40ef2-168">在 [方案總管] 中，以滑鼠右鍵按一下 AzureApp 專案。</span><span class="sxs-lookup"><span data-stu-id="40ef2-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="40ef2-169">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="40ef2-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="40ef2-170">如果您未登入您的 Azure 帳戶，按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="40ef2-171">您已登入後，選擇訂用帳戶，並按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="40ef2-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="40ef2-172">輸入雲端服務的名稱，然後選擇一個區域。</span><span class="sxs-lookup"><span data-stu-id="40ef2-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="40ef2-173">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="40ef2-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="40ef2-174">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="40ef2-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="40ef2-175">Azure 活動記錄 視窗會顯示部署的進度。</span><span class="sxs-lookup"><span data-stu-id="40ef2-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="40ef2-176">部署應用程式時，瀏覽至`http://appname.cloudapp.net/`，其中*appname*是您的雲端服務的名稱。</span><span class="sxs-lookup"><span data-stu-id="40ef2-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40ef2-177">其他資源</span><span class="sxs-lookup"><span data-stu-id="40ef2-177">Additional Resources</span></span>

- [<span data-ttu-id="40ef2-178">Katana 專案概觀</span><span class="sxs-lookup"><span data-stu-id="40ef2-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="40ef2-179">Katana GitHub 上的專案</span><span class="sxs-lookup"><span data-stu-id="40ef2-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
