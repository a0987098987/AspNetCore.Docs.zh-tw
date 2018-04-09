---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: 裝載 ASP.NET Web API 2 中的 Azure 背景工作角色 |Microsoft 文件
author: MikeWasson
description: 本教學課程會示範如何裝載 Azure 背景工作角色中的 ASP.NET Web API 使用 OWIN 自我裝載的 Web API framework。 開啟 Web 介面的.NET (OWIN) de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="a7136-104">裝載 ASP.NET Web API 2 中的 Azure 背景工作角色</span><span class="sxs-lookup"><span data-stu-id="a7136-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="a7136-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a7136-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="a7136-106">本教學課程會示範如何裝載 Azure 背景工作角色中的 ASP.NET Web API 使用 OWIN 自我裝載的 Web API framework。</span><span class="sxs-lookup"><span data-stu-id="a7136-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="a7136-107">[開啟適用於.NET 的 Web 介面](http://owin.org/)(OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="a7136-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="a7136-108">OWIN 以減少從 web 應用程式伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部 – 例如，在 Azure 工作者角色。</span><span class="sxs-lookup"><span data-stu-id="a7136-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="a7136-109">在此教學課程中，您將使用 Microsoft.Owin.Host.HttpListener 封裝，這樣會提供 HTTP 伺服器的自我裝載 OWIN 應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="a7136-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a7136-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="a7136-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="a7136-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a7136-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a7136-112">Web API 2</span><span class="sxs-lookup"><span data-stu-id="a7136-112">Web API 2</span></span>
> - [<span data-ttu-id="a7136-113">Azure SDK for .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="a7136-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="a7136-114">建立 Microsoft Azure 專案</span><span class="sxs-lookup"><span data-stu-id="a7136-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="a7136-115">啟動 Visual Studio 系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="a7136-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="a7136-116">若要偵錯應用程式在本機，使用 Azure 計算模擬器需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="a7136-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="a7136-117">在**檔案**功能表上，按一下 **新增**，然後按一下 **專案**。</span><span class="sxs-lookup"><span data-stu-id="a7136-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="a7136-118">從**已安裝的範本**，在 Visual C# 中，按一下**雲端**，然後按一下  **Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="a7136-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="a7136-119">將專案命名為"AzureApp 」，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="a7136-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="a7136-120">在**新的 Windows Azure 雲端服務** 對話方塊中，按兩下**背景工作角色**。</span><span class="sxs-lookup"><span data-stu-id="a7136-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="a7136-121">保留預設名稱 ("WorkerRole1")。</span><span class="sxs-lookup"><span data-stu-id="a7136-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="a7136-122">此步驟會將背景工作角色加入至方案。</span><span class="sxs-lookup"><span data-stu-id="a7136-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="a7136-123">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="a7136-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="a7136-124">建立 Visual Studio 方案包含兩個專案：</span><span class="sxs-lookup"><span data-stu-id="a7136-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="a7136-125">&quot;AzureApp&quot;定義的角色和 Azure 應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="a7136-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="a7136-126">&quot;WorkerRole1&quot;包含背景工作角色的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a7136-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="a7136-127">一般情況下，Azure 應用程式都可以包含多個角色，但本教學課程使用的單一角色。</span><span class="sxs-lookup"><span data-stu-id="a7136-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="a7136-128">加入 Web 應用程式開發介面和 OWIN 封裝</span><span class="sxs-lookup"><span data-stu-id="a7136-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="a7136-129">從**工具**功能表上，按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="a7136-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="a7136-130">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="a7136-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="a7136-131">加入的 HTTP 端點</span><span class="sxs-lookup"><span data-stu-id="a7136-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="a7136-132">在 [方案總管] 中，展開 AzureApp 專案。</span><span class="sxs-lookup"><span data-stu-id="a7136-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="a7136-133">展開 角色 節點，以滑鼠右鍵按一下 WorkerRole1，並選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="a7136-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="a7136-134">按一下**端點**，然後按一下 **加入端點**。</span><span class="sxs-lookup"><span data-stu-id="a7136-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="a7136-135">在**通訊協定**下拉式清單中，選取 「 http 」。</span><span class="sxs-lookup"><span data-stu-id="a7136-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="a7136-136">在**公用連接埠**和**私用連接埠**，輸入 80。</span><span class="sxs-lookup"><span data-stu-id="a7136-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="a7136-137">這些連接埠號碼可能會不同。</span><span class="sxs-lookup"><span data-stu-id="a7136-137">These port numbers can be different.</span></span> <span data-ttu-id="a7136-138">公用連接埠是哪些用戶端時使用它們將要求傳送至角色。</span><span class="sxs-lookup"><span data-stu-id="a7136-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="a7136-139">設定 Web API 的自我裝載</span><span class="sxs-lookup"><span data-stu-id="a7136-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="a7136-140">在 方案總管 中，以滑鼠右鍵按一下 WorkerRole1 專案並選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="a7136-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="a7136-141">將類別命名為 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="a7136-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="a7136-142">以下列內容取代所有此檔案中的未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="a7136-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="a7136-143">加入 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="a7136-143">Add a Web API Controller</span></span>

<span data-ttu-id="a7136-144">接下來，加入 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="a7136-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="a7136-145">以滑鼠右鍵按一下 WorkerRole1 專案並選取**新增** / **類別**。</span><span class="sxs-lookup"><span data-stu-id="a7136-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="a7136-146">TestController 的類別命名。</span><span class="sxs-lookup"><span data-stu-id="a7136-146">Name the class TestController.</span></span> <span data-ttu-id="a7136-147">以下列內容取代所有此檔案中的未定案程式碼：</span><span class="sxs-lookup"><span data-stu-id="a7136-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="a7136-148">為了簡單起見，此控制站只會定義兩個 GET 方法會傳回純文字。</span><span class="sxs-lookup"><span data-stu-id="a7136-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="a7136-149">啟動 OWIN 主機</span><span class="sxs-lookup"><span data-stu-id="a7136-149">Start the OWIN Host</span></span>

<span data-ttu-id="a7136-150">開啟 WorkerRole.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="a7136-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="a7136-151">這個類別會定義啟動及停止背景工作角色時執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a7136-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="a7136-152">加入下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="a7136-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="a7136-153">新增**IDisposable**成員`WorkerRole`類別：</span><span class="sxs-lookup"><span data-stu-id="a7136-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="a7136-154">在`OnStart`方法，加入下列的程式碼，以啟動主機時：</span><span class="sxs-lookup"><span data-stu-id="a7136-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="a7136-155">**WebApp.Start**方法會啟動 OWIN 主機。</span><span class="sxs-lookup"><span data-stu-id="a7136-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="a7136-156">名稱`Startup`類別是方法的型別參數。</span><span class="sxs-lookup"><span data-stu-id="a7136-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="a7136-157">依照慣例，主機會呼叫`Configure`這個類別的方法。</span><span class="sxs-lookup"><span data-stu-id="a7136-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="a7136-158">覆寫`OnStop`來處置*\_應用程式*執行個體：</span><span class="sxs-lookup"><span data-stu-id="a7136-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="a7136-159">以下是 WorkerRole.cs 的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="a7136-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="a7136-160">建置方案，並按下 F5，在 Azure 計算模擬器在本機執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a7136-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="a7136-161">根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。</span><span class="sxs-lookup"><span data-stu-id="a7136-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="a7136-162">如果您收到類似下面的例外狀況，請參閱[此部落格文章](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)的因應措施。</span><span class="sxs-lookup"><span data-stu-id="a7136-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="a7136-163">「 無法載入檔案或組件 ' Microsoft.Owin，Version = 2.0.2.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。</span><span class="sxs-lookup"><span data-stu-id="a7136-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="a7136-164">找到的組件資訊清單的定義不符合組件參考。</span><span class="sxs-lookup"><span data-stu-id="a7136-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="a7136-165">(來自 HRESULT 的例外狀況： 0x80131040) 」</span><span class="sxs-lookup"><span data-stu-id="a7136-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="a7136-166">計算模擬器會將本機 IP 位址指派給端點。</span><span class="sxs-lookup"><span data-stu-id="a7136-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="a7136-167">您可以藉由檢視計算模擬器 UI 中找到的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="a7136-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="a7136-168">在工作列通知區域中的模擬器圖示上按一下滑鼠右鍵，然後選取**顯示計算模擬器 UI**。</span><span class="sxs-lookup"><span data-stu-id="a7136-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="a7136-169">尋找 IP 位址在服務部署、 部署 [識別碼]，服務詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a7136-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="a7136-170">開啟網頁瀏覽器並瀏覽至 http://<em>位址</em>/測試/1，其中<em>位址</em>是由計算模擬器; 指派的 IP 位址，例如`http://127.0.0.1:80/test/1`。</span><span class="sxs-lookup"><span data-stu-id="a7136-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="a7136-171">您應該會看到來自 Web API 控制器的回應：</span><span class="sxs-lookup"><span data-stu-id="a7136-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="a7136-172">部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="a7136-172">Deploy to Azure</span></span>

<span data-ttu-id="a7136-173">此步驟中，您必須有 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="a7136-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="a7136-174">如果您還沒有其中一個，您可以建立免費的試用帳戶只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="a7136-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a7136-175">如需詳細資訊，請參閱[Microsoft Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="a7136-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="a7136-176">在 [方案總管] 中，以滑鼠右鍵按一下 AzureApp 專案。</span><span class="sxs-lookup"><span data-stu-id="a7136-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="a7136-177">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="a7136-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="a7136-178">如果您未登入您的 Azure 帳戶，按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="a7136-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="a7136-179">您已登入後，選擇訂用帳戶，並按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="a7136-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="a7136-180">輸入雲端服務的名稱，然後選擇一個區域。</span><span class="sxs-lookup"><span data-stu-id="a7136-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="a7136-181">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="a7136-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="a7136-182">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="a7136-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="a7136-183">Azure 活動記錄 視窗會顯示部署的進度。</span><span class="sxs-lookup"><span data-stu-id="a7136-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="a7136-184">部署應用程式時，瀏覽至http://appname.cloudapp.net/test/1。</span><span class="sxs-lookup"><span data-stu-id="a7136-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="a7136-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="a7136-185">Additional Resources</span></span>

- [<span data-ttu-id="a7136-186">Katana 專案概觀</span><span class="sxs-lookup"><span data-stu-id="a7136-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="a7136-187">Katana GitHub 上的專案</span><span class="sxs-lookup"><span data-stu-id="a7136-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
