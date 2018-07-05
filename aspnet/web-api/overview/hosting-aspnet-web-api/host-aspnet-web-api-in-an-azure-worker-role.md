---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: 裝載 ASP.NET Web API 2 中的 Azure 背景工作角色 |Microsoft Docs
author: MikeWasson
description: 本教學課程會示範如何裝載在 Azure 背景工作角色中的 ASP.NET Web API 使用 OWIN 自我裝載 Web API 架構。 Open Web Interface for.NET (OWIN) de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: a370f3bea74332d47e9132206c25d1be4211772c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395567"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="5b1fe-104">裝載 ASP.NET Web API 2 中的 Azure 背景工作角色</span><span class="sxs-lookup"><span data-stu-id="5b1fe-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="5b1fe-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5b1fe-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5b1fe-106">本教學課程會示範如何裝載在 Azure 背景工作角色中的 ASP.NET Web API 使用 OWIN 自我裝載 Web API 架構。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="5b1fe-107">[Open Web Interface for.NET](http://owin.org/) (OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="5b1fe-108">OWIN 可分隔的伺服器，讓 OWIN 很適合用於自我裝載的 web 應用程式，在您自己的程序，IIS 外部的 web 應用程式 – 比方說，在 Azure 背景工作角色。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="5b1fe-109">在本教學課程中，您將使用 Microsoft.Owin.Host.HttpListener 封裝，以提供 HTTP 伺服器，用於自我裝載 OWIN 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5b1fe-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="5b1fe-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="5b1fe-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5b1fe-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5b1fe-112">Web API 2</span><span class="sxs-lookup"><span data-stu-id="5b1fe-112">Web API 2</span></span>
> - [<span data-ttu-id="5b1fe-113">Azure SDK for.NET 2.3</span><span class="sxs-lookup"><span data-stu-id="5b1fe-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="5b1fe-114">建立 Microsoft Azure 專案</span><span class="sxs-lookup"><span data-stu-id="5b1fe-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="5b1fe-115">啟動 Visual Studio 系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="5b1fe-116">偵錯在本機的應用程式使用 Azure 計算模擬器需要系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="5b1fe-117">在上**檔案**功能表上，按一下**新增**，然後按一下**專案**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="5b1fe-118">從**已安裝的範本**，在 Visual C# 中，按一下**雲端**，然後按一下  **Windows Azure 雲端服務**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="5b1fe-119">將專案命名為"AzureApp"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="5b1fe-120">在 [**新的 Windows Azure 雲端服務**] 對話方塊中，按兩下**背景工作角色**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="5b1fe-121">保留預設名稱 ("WorkerRole1")。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="5b1fe-122">此步驟會將背景工作角色加入至方案。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="5b1fe-123">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="5b1fe-124">建立 Visual Studio 方案包含兩個專案：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="5b1fe-125">&quot;AzureApp&quot;定義的角色和設定 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="5b1fe-126">&quot;WorkerRole1&quot;包含背景工作角色的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="5b1fe-127">一般情況下，Azure 應用程式都可以包含多個角色，雖然本教學課程使用單一角色。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="5b1fe-128">新增 Web API 和 OWIN 套件</span><span class="sxs-lookup"><span data-stu-id="5b1fe-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="5b1fe-129">從**工具**功能表上，按一下**程式庫套件管理員**，然後按一下**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="5b1fe-130">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="5b1fe-131">新增 HTTP 端點</span><span class="sxs-lookup"><span data-stu-id="5b1fe-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="5b1fe-132">在 [方案總管] 中，展開 AzureApp 專案。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="5b1fe-133">展開 角色 節點，以滑鼠右鍵按一下 WorkerRole1，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="5b1fe-134">按一下 **端點**，然後按一下**新增端點**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="5b1fe-135">在 **通訊協定**下拉式清單中，選取 「 http 」。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="5b1fe-136">在 **公用連接埠**並**私人連接埠**，輸入 80。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="5b1fe-137">這些連接埠號碼可以不同。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-137">These port numbers can be different.</span></span> <span data-ttu-id="5b1fe-138">公用連接埠是用戶端使用的項目時，它們將要求傳送至角色。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="5b1fe-139">設定 Web API 的自我裝載</span><span class="sxs-lookup"><span data-stu-id="5b1fe-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="5b1fe-140">在 方案總管 中，以滑鼠右鍵按一下 WorkerRole1 專案，然後選取**新增** / **類別**若要加入新的類別。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="5b1fe-141">將類別命名為 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="5b1fe-142">您可以將所有未定案程式碼，此檔案中取代為下列：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="5b1fe-143">新增 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="5b1fe-143">Add a Web API Controller</span></span>

<span data-ttu-id="5b1fe-144">接下來，新增 Web API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="5b1fe-145">以滑鼠右鍵按一下 WorkerRole1 專案，然後選取**新增** / **類別**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="5b1fe-146">將類別命名為 TestController。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-146">Name the class TestController.</span></span> <span data-ttu-id="5b1fe-147">您可以將所有未定案程式碼，此檔案中取代為下列：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="5b1fe-148">為了簡單起見，此控制站只會定義兩個 GET 方法會傳回純文字。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="5b1fe-149">啟動 OWIN 主機</span><span class="sxs-lookup"><span data-stu-id="5b1fe-149">Start the OWIN Host</span></span>

<span data-ttu-id="5b1fe-150">開啟 WorkerRole.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="5b1fe-151">這個類別會定義啟動及停止背景工作角色時所執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="5b1fe-152">新增下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="5b1fe-153">新增**IDisposable**成員，才能`WorkerRole`類別：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="5b1fe-154">在 `OnStart`方法，加入下列的程式碼，以啟動主機：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="5b1fe-155">**WebApp.Start**方法會啟動 OWIN 主機。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="5b1fe-156">名稱`Startup`類別是型別參數的方法。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="5b1fe-157">依照慣例，主機會呼叫`Configure`這個類別的方法。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="5b1fe-158">覆寫`OnStop`處置*\_應用程式*執行個體：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="5b1fe-159">以下是 WorkerRole.cs 的完整程式碼：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="5b1fe-160">建置方案，並按下 F5 以在 Azure 計算模擬器在本機執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="5b1fe-161">根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="5b1fe-162">如果您收到類似下列的例外狀況，請參閱[此部落格文章](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)的因應措施。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="5b1fe-163">「 無法載入檔案或組件 ' Microsoft.Owin，版本 = 2.0.2.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="5b1fe-164">找到的組件資訊清單定義不符合組件參考。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="5b1fe-165">(來自 HRESULT 的例外狀況： 0x80131040) 」</span><span class="sxs-lookup"><span data-stu-id="5b1fe-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="5b1fe-166">計算模擬器會將本機的 IP 位址指派給端點。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="5b1fe-167">您可以藉由檢視計算模擬器 UI 中找到的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="5b1fe-168">在工作列通知區域中的 [模擬器] 圖示上按一下滑鼠右鍵，然後選取**顯示計算模擬器 UI**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="5b1fe-169">尋找 [服務部署，部署 [識別碼]，服務詳細資料] 下的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="5b1fe-170">開啟網頁瀏覽器並瀏覽至 http://<em>地址</em>/測試/1，其中<em>地址</em>是由計算模擬器; 指派的 IP 位址，例如`http://127.0.0.1:80/test/1`。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="5b1fe-171">您應該會看到來自 Web API 控制器的回應：</span><span class="sxs-lookup"><span data-stu-id="5b1fe-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="5b1fe-172">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="5b1fe-172">Deploy to Azure</span></span>

<span data-ttu-id="5b1fe-173">此步驟中，您必須有 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="5b1fe-174">如果您還沒有做，您可以建立免費的試用帳戶，只需要幾分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="5b1fe-175">如需詳細資訊，請參閱 < [Microsoft Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="5b1fe-176">在 [方案總管] 中，以滑鼠右鍵按一下 AzureApp 專案。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="5b1fe-177">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="5b1fe-178">如果您未登入您的 Azure 帳戶，按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="5b1fe-179">您已登入後，選擇訂用帳戶，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="5b1fe-180">輸入雲端服務的名稱，然後選擇的區域。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="5b1fe-181">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="5b1fe-182">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="5b1fe-183">[Azure 活動記錄] 視窗會顯示部署進度。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="5b1fe-184">部署應用程式時，瀏覽至http://appname.cloudapp.net/test/1。</span><span class="sxs-lookup"><span data-stu-id="5b1fe-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="5b1fe-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="5b1fe-185">Additional Resources</span></span>

- [<span data-ttu-id="5b1fe-186">Katana 專案概觀</span><span class="sxs-lookup"><span data-stu-id="5b1fe-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="5b1fe-187">在 GitHub 上的 Katana 專案</span><span class="sxs-lookup"><span data-stu-id="5b1fe-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
