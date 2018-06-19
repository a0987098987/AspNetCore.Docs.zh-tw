---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: 設定資料庫伺服器的 Web Deploy 發行 |Microsoft 文件
author: jrjlee
description: 本主題描述如何設定 SQL Server 2008 R2 資料庫伺服器以支援 web 部署和發行。 本主題中所述的工作會共同...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: a2340c0d561ed274e281b5f6d942af0a2027315a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885568"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a><span data-ttu-id="e12f6-104">設定 Web Deploy 發行變更資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="e12f6-104">Configuring a Database Server for Web Deploy Publishing</span></span>
====================
<span data-ttu-id="e12f6-105">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e12f6-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e12f6-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="e12f6-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e12f6-107">本主題描述如何設定 SQL Server 2008 R2 資料庫伺服器以支援 web 部署和發行。</span><span class="sxs-lookup"><span data-stu-id="e12f6-107">This topic describes how to configure a SQL Server 2008 R2 database server to support web deployment and publishing.</span></span>
> 
> <span data-ttu-id="e12f6-108">本主題中所述的工作通用於每個部署情況&#x2014;不論您的 web 伺服器設定為使用 IIS Web Deployment Tool (Web Deploy) 的遠端代理程式服務、 Web 部署的處理常式或離線部署或在單一網頁伺服器或伺服器陣列上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e12f6-108">The tasks described in this topic are common to every deployment scenario&#x2014;it doesn't matter whether your web servers are configured to use the IIS Web Deployment Tool (Web Deploy) Remote Agent Service, the Web Deploy Handler, or offline deployment or your application is running on a single web server or a server farm.</span></span> <span data-ttu-id="e12f6-109">部署資料庫的方式會根據安全性需求和其他考量可能會變更。</span><span class="sxs-lookup"><span data-stu-id="e12f6-109">The way you deploy the database may change according to security requirements and other considerations.</span></span> <span data-ttu-id="e12f6-110">比方說，您可以部署資料庫，不論範例資料，而您可能會部署角色的使用者對應，或部署後手動設定。</span><span class="sxs-lookup"><span data-stu-id="e12f6-110">For example, you might deploy the database with or without sample data, and you might deploy user role mappings or configure them manually after deployment.</span></span> <span data-ttu-id="e12f6-111">不過，您將資料庫伺服器設定的方式也維持不變。</span><span class="sxs-lookup"><span data-stu-id="e12f6-111">However, the way you configure the database server remains the same.</span></span>


<span data-ttu-id="e12f6-112">您沒有安裝任何其他產品或工具，以設定要支援 web 部署的資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="e12f6-112">You don't have to install any additional products or tools to configuring a database server to support web deployment.</span></span> <span data-ttu-id="e12f6-113">假設您的資料庫伺服器和網頁伺服器執行於不同電腦，您只需要：</span><span class="sxs-lookup"><span data-stu-id="e12f6-113">Assuming that your database server and your web server run on different machines, you simply need to:</span></span>

- <span data-ttu-id="e12f6-114">允許 SQL Server 使用 TCP/IP 進行通訊。</span><span class="sxs-lookup"><span data-stu-id="e12f6-114">Permit SQL Server to communicate using TCP/IP.</span></span>
- <span data-ttu-id="e12f6-115">允許 SQL Server 流量通過防火牆。</span><span class="sxs-lookup"><span data-stu-id="e12f6-115">Allow SQL Server traffic through any firewalls.</span></span>
- <span data-ttu-id="e12f6-116">授與 web 伺服器機器帳戶的 SQL Server 登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-116">Give the web server machine account a SQL Server login.</span></span>
- <span data-ttu-id="e12f6-117">對應至任何必要的資料庫角色的電腦帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-117">Map the machine account login to any required database roles.</span></span>
- <span data-ttu-id="e12f6-118">將部署執行的 SQL Server 登入和資料庫建立者權限的帳戶授與。</span><span class="sxs-lookup"><span data-stu-id="e12f6-118">Give the account that will run the deployment a SQL Server login and database creator permissions.</span></span>
- <span data-ttu-id="e12f6-119">若要支援重複的部署，將對應的部署的帳戶登入**db\_擁有者**資料庫角色。</span><span class="sxs-lookup"><span data-stu-id="e12f6-119">To support repeat deployments, map the deployment account login to the **db\_owner** database role.</span></span>

<span data-ttu-id="e12f6-120">本主題將說明如何執行這種程序。</span><span class="sxs-lookup"><span data-stu-id="e12f6-120">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="e12f6-121">工作與本主題中的逐步解說假設您要開始使用 Windows Server 2008 R2 上執行的 SQL Server 2008 R2 的預設執行個體。</span><span class="sxs-lookup"><span data-stu-id="e12f6-121">The tasks and walkthroughs in this topic assume that you're starting with a default instance of SQL Server 2008 R2 running on Windows Server 2008 R2.</span></span> <span data-ttu-id="e12f6-122">在繼續之前，請確認：</span><span class="sxs-lookup"><span data-stu-id="e12f6-122">Before you continue, ensure that:</span></span>

- <span data-ttu-id="e12f6-123">已安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。</span><span class="sxs-lookup"><span data-stu-id="e12f6-123">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="e12f6-124">伺服器已加入網域。</span><span class="sxs-lookup"><span data-stu-id="e12f6-124">The server is domain-joined.</span></span>
- <span data-ttu-id="e12f6-125">伺服器具有靜態 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="e12f6-125">The server has a static IP address.</span></span>
- <span data-ttu-id="e12f6-126">已安裝 SQL Server 2008 R2 Service Pack 1 和所有可用的更新。</span><span class="sxs-lookup"><span data-stu-id="e12f6-126">SQL Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>

<span data-ttu-id="e12f6-127">SQL Server 執行個體只需要包含**Database Engine Services**角色，會自動包含在任何 SQL Server 安裝。</span><span class="sxs-lookup"><span data-stu-id="e12f6-127">The SQL Server instance only needs to include the **Database Engine Services** role, which is included automatically in any SQL Server installation.</span></span> <span data-ttu-id="e12f6-128">不過，為了簡化組態和維護，我們建議您包含**管理工具 – 基本**和**管理工具-完整**伺服器角色。</span><span class="sxs-lookup"><span data-stu-id="e12f6-128">However, for ease of configuration and maintenance, we recommend that you include the **Management Tools – Basic** and **Management Tools – Complete** server roles.</span></span>

> [!NOTE]
> <span data-ttu-id="e12f6-129">如需有關如何將電腦加入網域的詳細資訊，請參閱[將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-129">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="e12f6-130">如需有關如何設定靜態 IP 位址的詳細資訊，請參閱[設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-130">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span> <span data-ttu-id="e12f6-131">如需安裝 SQL Server 的詳細資訊，請參閱[安裝 SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-131">For more information on installing SQL Server, see [Installing SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).</span></span>


## <a name="enable-remote-access-to-sql-server"></a><span data-ttu-id="e12f6-132">啟用 SQL Server 的遠端存取</span><span class="sxs-lookup"><span data-stu-id="e12f6-132">Enable Remote Access to SQL Server</span></span>

<span data-ttu-id="e12f6-133">SQL Server 會使用 TCP/IP 的遠端電腦進行通訊。</span><span class="sxs-lookup"><span data-stu-id="e12f6-133">SQL Server uses TCP/IP to communicate with remote computers.</span></span> <span data-ttu-id="e12f6-134">如果您的資料庫伺服器和網頁伺服器位於不同的電腦，您需要：</span><span class="sxs-lookup"><span data-stu-id="e12f6-134">If your database server and your web server are on different machines, you need to:</span></span>

- <span data-ttu-id="e12f6-135">設定 SQL Server 以允許透過 TCP/IP 通訊的網路設定。</span><span class="sxs-lookup"><span data-stu-id="e12f6-135">Configure SQL Server networking settings to allow communication over TCP/IP.</span></span>
- <span data-ttu-id="e12f6-136">設定 SQL Server 執行個體所使用的連接埠上允許 TCP 流量 （而且在某些情況下使用者資料包通訊協定 (UDP) 流量） 的任何硬體或軟體防火牆。</span><span class="sxs-lookup"><span data-stu-id="e12f6-136">Configure any hardware or software firewalls to allow TCP traffic (and in some cases User Datagram Protocol (UDP) traffic) on the ports that the SQL Server instance uses.</span></span>

<span data-ttu-id="e12f6-137">若要啟用 SQL Server over TCP/IP 通訊，請使用 SQL Server 組態管理員來變更您的 SQL Server 執行個體的網路設定。</span><span class="sxs-lookup"><span data-stu-id="e12f6-137">To enable SQL Server to communicate over TCP/IP, use SQL Server Configuration Manager to change the network configuration for your SQL Server instance.</span></span>

<span data-ttu-id="e12f6-138">**若要啟用 SQL Server 使用 TCP/IP 通訊**</span><span class="sxs-lookup"><span data-stu-id="e12f6-138">**To enable SQL Server to communicate using TCP/IP**</span></span>

1. <span data-ttu-id="e12f6-139">上**啟動**功能表上，指向**所有程式**，按一下  **Microsoft SQL Server 2008 R2**，按一下 **組態工具**，然後按一下**SQL Server 組態管理員**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-139">On the **Start** menu, point to **All Programs**, click **Microsoft SQL Server 2008 R2**, click **Configuration Tools**, and then click **SQL Server Configuration Manager**.</span></span>
2. <span data-ttu-id="e12f6-140">在樹狀檢視窗格中，依序展開**SQL Server 網路組態**，然後按一下  **MSSQLSERVER 的通訊協定**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-140">In the tree view pane, expand **SQL Server Network Configuration**, and then click **Protocols for MSSQLSERVER**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e12f6-141">如果您已安裝多個 SQL Server 執行個體，您會看到<strong>通訊協定</strong><em>[執行個體名稱]</em>每個執行個體的項目。</span><span class="sxs-lookup"><span data-stu-id="e12f6-141">If you have installed multiple instances of SQL Server, you'll see a <strong>Protocols for</strong><em>[instance name]</em> item for each instance.</span></span> <span data-ttu-id="e12f6-142">您要設定執行個體的執行個體為基礎的網路設定。</span><span class="sxs-lookup"><span data-stu-id="e12f6-142">You need to configure network settings on an instance-by-instance basis.</span></span>
3. <span data-ttu-id="e12f6-143">在詳細資料窗格中，以滑鼠右鍵按一下**TCP/IP**資料列，然後再按一下**啟用**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-143">In the details pane, right-click the **TCP/IP** row, and then click **Enable**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. <span data-ttu-id="e12f6-144">在**警告**對話方塊中，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-144">In the **Warning** dialog box, click **OK**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. <span data-ttu-id="e12f6-145">您需要重新啟動 MSSQLSERVER 服務，新的網路組態才會生效。</span><span class="sxs-lookup"><span data-stu-id="e12f6-145">You need to restart the MSSQLSERVER service before your new network configuration will take effect.</span></span> <span data-ttu-id="e12f6-146">您可以執行的命令提示字元中，從 [服務] 主控台中，或從 SQL Server Management Studio。</span><span class="sxs-lookup"><span data-stu-id="e12f6-146">You can do that at a command prompt, from the Services console, or from SQL Server Management Studio.</span></span> <span data-ttu-id="e12f6-147">在此程序，您將使用 SQL Server Management Studio。</span><span class="sxs-lookup"><span data-stu-id="e12f6-147">In this procedure, you'll use SQL Server Management Studio.</span></span>
6. <span data-ttu-id="e12f6-148">關閉 SQL Server 組態管理員。</span><span class="sxs-lookup"><span data-stu-id="e12f6-148">Close SQL Server Configuration Manager.</span></span>
7. <span data-ttu-id="e12f6-149">在**啟動**功能表上，指向**所有程式**，按一下  **Microsoft SQL Server 2008 R2**，然後按一下  **SQL Server Management Studio**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-149">On the **Start** menu, point to **All Programs**, click **Microsoft SQL Server 2008 R2**, and then click **SQL Server Management Studio**.</span></span>
8. <span data-ttu-id="e12f6-150">在**連接到伺服器**對話方塊中，於**伺服器名稱**方塊中，輸入資料庫伺服器的名稱，然後按一下**連接**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-150">In the **Connect to Server** dialog box, in the **Server name** box, type the name of the database server, and then click **Connect**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. <span data-ttu-id="e12f6-151">在**物件總管 中** 窗格中，父伺服器節點上按一下滑鼠右鍵 (例如， **TESTDB1**)，然後按一下**重新啟動**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-151">In the **Object Explorer** pane, right-click the parent server node (for example, **TESTDB1**), and then click **Restart**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. <span data-ttu-id="e12f6-152">在**Microsoft SQL Server Management Studio**對話方塊中，按一下 **是**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-152">In the **Microsoft SQL Server Management Studio** dialog box, click **Yes**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. <span data-ttu-id="e12f6-153">當重新啟動服務後時，關閉 SQL Server Management Studio。</span><span class="sxs-lookup"><span data-stu-id="e12f6-153">When the service has restarted, close SQL Server Management Studio.</span></span>

<span data-ttu-id="e12f6-154">若要讓 SQL Server 流量通過防火牆，您首先要知道您的 SQL Server 執行個體所使用的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="e12f6-154">To allow SQL Server traffic through a firewall, you first need to know which ports your SQL Server instance is using.</span></span> <span data-ttu-id="e12f6-155">這會取決於如何建立及設定 SQL Server 執行個體已：</span><span class="sxs-lookup"><span data-stu-id="e12f6-155">This will depend on how the SQL Server instance was created and configured:</span></span>

- <span data-ttu-id="e12f6-156">A*預設執行個體*的 SQL Server 會接聽 （和回應） TCP 通訊埠 1433年上的要求。</span><span class="sxs-lookup"><span data-stu-id="e12f6-156">A *default instance* of SQL Server listens for (and responds to) requests on TCP port 1433.</span></span>
- <span data-ttu-id="e12f6-157">A*具名執行個體*的 SQL Server 會接聽 （和回應） 動態指派的 TCP 連接埠上的要求。</span><span class="sxs-lookup"><span data-stu-id="e12f6-157">A *named instance* of SQL Server listens for (and responds to) requests on a dynamically assigned TCP port.</span></span>
- <span data-ttu-id="e12f6-158">如果已啟用 SQL Server Browser 服務，用戶端可以查詢 UDP 通訊埠 1434，若要找出要使用特定的 SQL Server 執行個體的 TCP 通訊埠上的服務。</span><span class="sxs-lookup"><span data-stu-id="e12f6-158">If the SQL Server Browser service is enabled, clients can query the service on UDP port 1434 to find out which TCP port to use for a particular SQL Server instance.</span></span> <span data-ttu-id="e12f6-159">不過，此服務是通常停用基於安全性考量。</span><span class="sxs-lookup"><span data-stu-id="e12f6-159">However, this service is often disabled for security reasons.</span></span>

<span data-ttu-id="e12f6-160">假設您使用 SQL Server 的預設執行個體，您需要設定防火牆允許流量。</span><span class="sxs-lookup"><span data-stu-id="e12f6-160">Assuming that you're using a default instance of SQL Server, you need to configure your firewall to allow traffic.</span></span>

| <span data-ttu-id="e12f6-161">Direction</span><span class="sxs-lookup"><span data-stu-id="e12f6-161">Direction</span></span> | <span data-ttu-id="e12f6-162">從連接埠</span><span class="sxs-lookup"><span data-stu-id="e12f6-162">From Port</span></span> | <span data-ttu-id="e12f6-163">連接埠</span><span class="sxs-lookup"><span data-stu-id="e12f6-163">To Port</span></span> | <span data-ttu-id="e12f6-164">連接埠類型</span><span class="sxs-lookup"><span data-stu-id="e12f6-164">Port Type</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e12f6-165">輸入</span><span class="sxs-lookup"><span data-stu-id="e12f6-165">Inbound</span></span> | <span data-ttu-id="e12f6-166">任何</span><span class="sxs-lookup"><span data-stu-id="e12f6-166">Any</span></span> | <span data-ttu-id="e12f6-167">1433</span><span class="sxs-lookup"><span data-stu-id="e12f6-167">1433</span></span> | <span data-ttu-id="e12f6-168">TCP</span><span class="sxs-lookup"><span data-stu-id="e12f6-168">TCP</span></span> |
| <span data-ttu-id="e12f6-169">輸出</span><span class="sxs-lookup"><span data-stu-id="e12f6-169">Outbound</span></span> | <span data-ttu-id="e12f6-170">1433</span><span class="sxs-lookup"><span data-stu-id="e12f6-170">1433</span></span> | <span data-ttu-id="e12f6-171">任何</span><span class="sxs-lookup"><span data-stu-id="e12f6-171">Any</span></span> | <span data-ttu-id="e12f6-172">TCP</span><span class="sxs-lookup"><span data-stu-id="e12f6-172">TCP</span></span> |
  

> [!NOTE]
> <span data-ttu-id="e12f6-173">技術上來說，用戶端電腦會使用介於 1024年到 5000 之間隨機指派的 TCP 通訊埠與 SQL Server 通訊，因此您可以據以限制您的防火牆規則。</span><span class="sxs-lookup"><span data-stu-id="e12f6-173">Technically, a client computer will use a randomly assigned TCP port between 1024 and 5000 to communicate with SQL Server, and you can restrict your firewall rules accordingly.</span></span> <span data-ttu-id="e12f6-174">如需有關 SQL Server 連接埠和防火牆的詳細資訊，請參閱[TCP/IP 通訊埠編號，才能透過防火牆進行通訊 to SQL](https://go.microsoft.com/?linkid=9805125)和[How to： 設定伺服器接聽特定 TCP 通訊埠 （SQL Server 組態管理員）](https://msdn.microsoft.com/library/ms177440.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-174">For more information on SQL Server ports and firewalls, see [TCP/IP port numbers required to communicate to SQL over a firewall](https://go.microsoft.com/?linkid=9805125) and [How to: Configure a Server to Listen on a Specific TCP Port (SQL Server Configuration Manager)](https://msdn.microsoft.com/library/ms177440.aspx).</span></span>


<span data-ttu-id="e12f6-175">在大部分的 Windows Server 環境中，您可能必須在資料庫伺服器上設定 Windows 防火牆。</span><span class="sxs-lookup"><span data-stu-id="e12f6-175">In most Windows Server environments, you'll likely have to configure Windows Firewall on the database server.</span></span> <span data-ttu-id="e12f6-176">根據預設，Windows 防火牆會允許所有輸出流量，除非規則明確禁止。</span><span class="sxs-lookup"><span data-stu-id="e12f6-176">By default, Windows Firewall allows all outbound traffic unless a rule specifically prohibits it.</span></span> <span data-ttu-id="e12f6-177">若要啟用您的 web 伺服器連線到您的資料庫，您需要設定 SQL Server 執行個體會使用通訊埠編號允許 TCP 流量的輸入的規則。</span><span class="sxs-lookup"><span data-stu-id="e12f6-177">To enable your web server to reach your database, you need to configure an inbound rule that allows TCP traffic on the port number that the SQL Server instance uses.</span></span> <span data-ttu-id="e12f6-178">如果您使用 SQL Server 的預設執行個體，您可以使用下一個程序來設定此規則。</span><span class="sxs-lookup"><span data-stu-id="e12f6-178">If you're using a default instance of SQL Server, you can use the next procedure to configure this rule.</span></span>

<span data-ttu-id="e12f6-179">**若要設定 Windows 防火牆以允許使用預設 SQL Server 執行個體的通訊**</span><span class="sxs-lookup"><span data-stu-id="e12f6-179">**To configure Windows Firewall to allow communication with a default SQL Server instance**</span></span>

1. <span data-ttu-id="e12f6-180">在資料庫伺服器上，在**啟動**功能表上，指向**系統管理工具**，然後按一下 **具有進階安全性的 Windows 防火牆**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-180">On the database server, on the **Start** menu, point to **Administrative Tools**, and then click **Windows Firewall with Advanced Security**.</span></span>
2. <span data-ttu-id="e12f6-181">在 樹狀檢視窗格中，按一下 **輸入規則**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-181">In the tree view pane, click **Inbound Rules**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. <span data-ttu-id="e12f6-182">在**動作**窗格下**輸入規則**，按一下 **新規則**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-182">In the **Actions** pane, under **Inbound Rules**, click **New Rule**.</span></span>
4. <span data-ttu-id="e12f6-183">在 新增輸入規則精靈，在**規則類型**頁面上，選取**連接埠**，然後按一下 **下一步**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-183">In the New Inbound Rule Wizard, on the **Rule Type** page, select **Port**, and then click **Next**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. <span data-ttu-id="e12f6-184">在**通訊協定和連接埠**頁面上，確認**TCP**已選取，然後在**特定本機連接埠**方塊中，輸入**1433年**，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-184">On the **Protocol and Ports** page, ensure that **TCP** is selected, and in the **Specific local ports** box, type **1433**, and then click **Next**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. <span data-ttu-id="e12f6-185">在**動作**頁面上，保留**允許連線**選取並按**下一步**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-185">On the **Action** page, leave **Allow the connection** selected and click **Next**.</span></span>
7. <span data-ttu-id="e12f6-186">在**設定檔**頁面上，保留**網域**選取，清除**私人**和**公用**核取方塊，，然後按一下  **下一步**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-186">On the **Profile** page, leave **Domain** selected, clear the **Private** and **Public** check boxes, and then click **Next**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. <span data-ttu-id="e12f6-187">上**名稱**頁面上，為規則提供適當的描述性名稱 (例如， **SQL Server 預設執行個體 – 網路存取**)，然後按一下 **完成**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-187">On the **Name** page, give the rule a suitably descriptive name (for example, **SQL Server default instance – network access**), and then click **Finish**.</span></span>

<span data-ttu-id="e12f6-188">如需有關設定用於 SQL Server 的 Windows 防火牆，特別是如果您需要透過非標準或動態連接埠與 SQL Server 通訊，請參閱[How to： 設定用於 Database Engine 存取的 Windows 防火牆](https://technet.microsoft.com/library/ms175043.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-188">For more information on configuring Windows Firewall for SQL Server, particularly if you need to communicate with SQL Server over non-standard or dynamic ports, see [How to: Configure a Windows Firewall for Database Engine Access](https://technet.microsoft.com/library/ms175043.aspx).</span></span>

## <a name="configure-logins-and-database-permissions"></a><span data-ttu-id="e12f6-189">設定登入和資料庫權限</span><span class="sxs-lookup"><span data-stu-id="e12f6-189">Configure Logins and Database Permissions</span></span>

<span data-ttu-id="e12f6-190">當您部署 web 應用程式到網際網路資訊服務 (IIS) 時，應用程式會使用執行的應用程式集區身分識別。</span><span class="sxs-lookup"><span data-stu-id="e12f6-190">When you deploy a web application to Internet Information Services (IIS), the application runs using the identity of the application pool.</span></span> <span data-ttu-id="e12f6-191">在網域環境中，應用程式集區識別使用其執行存取網路資源所在伺服器的電腦帳戶。</span><span class="sxs-lookup"><span data-stu-id="e12f6-191">In a domain environment, application pool identities use the machine account of the server on which they run to access network resources.</span></span> <span data-ttu-id="e12f6-192">電腦帳戶會採用<em>[網域名稱]</em><strong>\<n g ><em>[電腦名稱]</em><strong>$</strong>&#x2014;，例如<strong>FABRIKAM\TESTWEB1$</strong>。</span><span class="sxs-lookup"><span data-stu-id="e12f6-192">Machine accounts take the form <em>[domain name]</em><strong>\</strong><em>[machine name]</em><strong>$</strong>&#x2014;for example, <strong>FABRIKAM\TESTWEB1$</strong>.</span></span> <span data-ttu-id="e12f6-193">若要讓 web 應用程式透過網路存取的資料庫，您需要：</span><span class="sxs-lookup"><span data-stu-id="e12f6-193">To allow your web application to access a database across the network, you need to:</span></span>

- <span data-ttu-id="e12f6-194">將 web 伺服器電腦帳戶的登入加入至 SQL Server 執行個體。</span><span class="sxs-lookup"><span data-stu-id="e12f6-194">Add a login for the web server machine account to the SQL Server instance.</span></span>
- <span data-ttu-id="e12f6-195">將電腦帳戶登入對應到任何必要的資料庫角色 (通常**db\_datareader**和**db\_datawriter**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-195">Map the machine account login to any required database roles (typically **db\_datareader** and **db\_datawriter**).</span></span>

<span data-ttu-id="e12f6-196">如果伺服器陣列中，而不是單一伺服器上執行您 web 應用程式，您必須針對伺服器陣列中每個網頁伺服器重複這些程序。</span><span class="sxs-lookup"><span data-stu-id="e12f6-196">If your web application is running on a server farm, rather than a single server, you'll need to repeat these procedures for every web server in the server farm.</span></span>

> [!NOTE]
> <span data-ttu-id="e12f6-197">如需有關應用程式集區身分識別和存取網路資源的詳細資訊，請參閱[應用程式集區識別](https://go.microsoft.com/?linkid=9805123)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-197">For more information on application pool identities and accessing network resources, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="e12f6-198">您可以讓這些工作，以各種方式。</span><span class="sxs-lookup"><span data-stu-id="e12f6-198">You can approach these tasks in various ways.</span></span> <span data-ttu-id="e12f6-199">若要建立登入，您可以：</span><span class="sxs-lookup"><span data-stu-id="e12f6-199">To create the login, you can either:</span></span>

- <span data-ttu-id="e12f6-200">使用 TRANSACT-SQL 或 SQL Server Management Studio，在資料庫伺服器上手動建立登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-200">Create the login manually on the database server, using Transact-SQL or SQL Server Management Studio.</span></span>
- <span data-ttu-id="e12f6-201">使用 Visual Studio 中的 SQL Server 2008 伺服器專案來建立及部署的登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-201">Use a SQL Server 2008 Server Project in Visual Studio to create and deploy the login.</span></span>

<span data-ttu-id="e12f6-202">SQL Server 登入是伺服器層級物件，而不是資料庫層級物件，而不相依於您想要部署的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e12f6-202">A SQL Server login is a server-level object, rather than a database-level object, so it's not dependent on the database you want to deploy.</span></span> <span data-ttu-id="e12f6-203">因此，您可以在任何時間點，來建立登入，而最簡單的方法通常是登入先手動建立資料庫伺服器上啟動 部署資料庫。</span><span class="sxs-lookup"><span data-stu-id="e12f6-203">As such, you can create the login at any point, and the easiest approach is often to create the login manually on the database server before you start deploying databases.</span></span> <span data-ttu-id="e12f6-204">您可以使用下一個程序建立 SQL Server Management Studio 中的登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-204">You can use the next procedure to create a login in SQL Server Management Studio.</span></span>

<span data-ttu-id="e12f6-205">**若要建立 web 伺服器電腦帳戶的 SQL Server 登入**</span><span class="sxs-lookup"><span data-stu-id="e12f6-205">**To create a SQL Server login for the web server machine account**</span></span>

1. <span data-ttu-id="e12f6-206">在資料庫伺服器上，在**啟動**功能表上，指向**所有程式**，按一下**Microsoft SQL Server 2008 R2**，然後按一下  **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="e12f6-206">On the database server, on the **Start** menu, point to **All Programs**, click **Microsoft SQL Server 2008 R2**, and then click **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="e12f6-207">在**連接到伺服器**對話方塊中，於**伺服器名稱**方塊中，輸入資料庫伺服器的名稱，然後按一下**連接**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-207">In the **Connect to Server** dialog box, in the **Server name** box, type the name of the database server, and then click **Connect**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. <span data-ttu-id="e12f6-208">在**物件總管] 中**] 窗格中，以滑鼠右鍵按一下**安全性**，指向 [**新增**，然後按一下 [**登入**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-208">In the **Object Explorer** pane, right-click **Security**, point to **New**, and then click **Login**.</span></span>
4. <span data-ttu-id="e12f6-209">在**登入-新增**對話方塊中，於**登入名稱**方塊中，輸入您的 web 伺服器電腦帳戶的名稱 (例如， **FABRIKAM\TESTWEB1$**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-209">In the **Login – New** dialog box, in the **Login name** box, type the name of your web server machine account (for example, **FABRIKAM\TESTWEB1$**).</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. <span data-ttu-id="e12f6-210">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="e12f6-210">Click **OK**.</span></span>

<span data-ttu-id="e12f6-211">此時，您的資料庫伺服器可供 Web Deploy 發行。</span><span class="sxs-lookup"><span data-stu-id="e12f6-211">At this point, your database server is ready for Web Deploy publishing.</span></span> <span data-ttu-id="e12f6-212">不過，任何您所部署的解決方案也將無法運作，直到您將電腦帳戶登入對應至所需的資料庫角色。</span><span class="sxs-lookup"><span data-stu-id="e12f6-212">However, any solutions you deploy won't work until you map the machine account login to the required database roles.</span></span> <span data-ttu-id="e12f6-213">登入對應到資料庫角色需要更以為大量，您無法之後，直到對應角色已部署資料庫。</span><span class="sxs-lookup"><span data-stu-id="e12f6-213">Mapping the login to database roles requires a lot more thought, as you can't map roles until after you've deployed the database.</span></span> <span data-ttu-id="e12f6-214">若要對應到必要的資料庫角色的電腦帳戶登入，您可以：</span><span class="sxs-lookup"><span data-stu-id="e12f6-214">To map the machine account login to the required database roles, you can either:</span></span>

- <span data-ttu-id="e12f6-215">資料庫角色後指派給登入以手動方式，您已部署資料庫第一次。</span><span class="sxs-lookup"><span data-stu-id="e12f6-215">Assign the database roles to the login manually, after you've deployed the database for the first time.</span></span>
- <span data-ttu-id="e12f6-216">要指派給登入的資料庫角色中使用的部署後指令碼。</span><span class="sxs-lookup"><span data-stu-id="e12f6-216">Use a post-deployment script to assign the database roles to the login.</span></span>

<span data-ttu-id="e12f6-217">如需有關自動建立登入和資料庫角色對應的詳細資訊，請參閱[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-217">For more information on automating the creation of logins and database role mappings, see [Deploying Database Role Memberships to Test Environments](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).</span></span> <span data-ttu-id="e12f6-218">或者，您可以使用下一個程序來手動對應到必要的資料庫角色的電腦帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-218">Alternatively, you can use the next procedure to map the machine account login to the required database roles manually.</span></span> <span data-ttu-id="e12f6-219">請記住，您無法執行此程序，直到*之後*已部署資料庫。</span><span class="sxs-lookup"><span data-stu-id="e12f6-219">Remember that you can't perform this procedure until *after* you've deployed the database.</span></span>

<span data-ttu-id="e12f6-220">**將資料庫角色對應至 web 伺服器電腦帳戶登入**</span><span class="sxs-lookup"><span data-stu-id="e12f6-220">**To map database roles to the web server machine account login**</span></span>

1. <span data-ttu-id="e12f6-221">如常開啟 SQL Server Management Studio。</span><span class="sxs-lookup"><span data-stu-id="e12f6-221">Open SQL Server Management Studio as before.</span></span>
2. <span data-ttu-id="e12f6-222">中**物件總管 中** 窗格中，展開**安全性** 節點，展開**登入** 節點，然後按兩下 電腦帳戶登入 (例如， **FABRIKAM\TESTWEB1$**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-222">In the **Object Explorer** pane, expand the **Security** node, expand the **Logins** node, and then double-click the machine account login (for example, **FABRIKAM\TESTWEB1$**).</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. <span data-ttu-id="e12f6-223">在**登入屬性**對話方塊中，按一下 **使用者對應**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-223">In the **Login Properties** dialog box, click **User Mapping**.</span></span>
4. <span data-ttu-id="e12f6-224">在**對應到此登入的使用者**資料表中，選取您的資料庫名稱 (例如， **ContactManager**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-224">In the **Users mapped to this login** table, select the name of your database (for example, **ContactManager**).</span></span>
5. <span data-ttu-id="e12f6-225">在**資料庫角色成員資格對象：** *[資料庫名稱]* 清單中，選取所需的權限。</span><span class="sxs-lookup"><span data-stu-id="e12f6-225">In the **Database role membership for:** *[database name]* list, select the permissions required.</span></span> <span data-ttu-id="e12f6-226">範例解決方案的連絡管理員，您必須選取**db\_datareader**和**db\_datawriter**角色。</span><span class="sxs-lookup"><span data-stu-id="e12f6-226">In the case of the Contact Manager sample solution, you must select the **db\_datareader** and **db\_datawriter** roles.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. <span data-ttu-id="e12f6-227">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="e12f6-227">Click **OK**.</span></span>

<span data-ttu-id="e12f6-228">雖然手動對應資料庫角色的方法，通常更適合用於測試環境，它是較不適合自動或單鍵部署到預備環境或生產環境。</span><span class="sxs-lookup"><span data-stu-id="e12f6-228">While manually mapping database roles is often more than adequate for test environments, it's less desirable for automated or one-click deployments to staging or production environments.</span></span> <span data-ttu-id="e12f6-229">您可以找到更多有關自動化這種使用中的部署後指令碼工作[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-229">You can find more information on automating this kind of task using post-deployment scripts in [Deploying Database Role Memberships to Test Environments](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e12f6-230">如需有關伺服器專案及資料庫專案的詳細資訊，請參閱[Visual Studio 2010 SQL Server 資料庫專案](https://msdn.microsoft.com/library/ff678491.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-230">For more information on server projects and database projects, see [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/library/ff678491.aspx).</span></span>


## <a name="configure-permissions-for-the-deployment-account"></a><span data-ttu-id="e12f6-231">設定部署帳戶的權限</span><span class="sxs-lookup"><span data-stu-id="e12f6-231">Configure Permissions for the Deployment Account</span></span>

<span data-ttu-id="e12f6-232">如果您要執行部署將使用的帳戶不是 SQL Server 系統管理員，您還需要建立此帳戶的登入。</span><span class="sxs-lookup"><span data-stu-id="e12f6-232">If the account that you'll use to run the deployment is not a SQL Server administrator, you'll also need to create a login for this account.</span></span> <span data-ttu-id="e12f6-233">若要建立資料庫，帳戶必須隸屬**dbcreator**伺服器角色或具有同等權限。</span><span class="sxs-lookup"><span data-stu-id="e12f6-233">In order to create the database, the account must be a member of the **dbcreator** server role or have equivalent permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="e12f6-234">當您使用 Web Deploy 或 VSDBCMD 部署資料庫時，您可以使用 Windows 認證或 SQL Server 認證 （如果您的 SQL Server 執行個體已設定為支援混合的模式驗證）。</span><span class="sxs-lookup"><span data-stu-id="e12f6-234">When you use Web Deploy or VSDBCMD to deploy a database, you can use Windows credentials or SQL Server credentials (if your SQL Server instance is configured to support mixed mode authentication).</span></span> <span data-ttu-id="e12f6-235">下一個程序假設您想要使用的 Windows 認證，但是沒有停止您從 SQL Server 使用者名稱和密碼指定連接字串中，當您將部署設定的項目。</span><span class="sxs-lookup"><span data-stu-id="e12f6-235">The next procedure assumes that you want to use Windows credentials, but there's nothing stopping you from specifying a SQL Server user name and password in your connection string when you configure the deployment.</span></span>


<span data-ttu-id="e12f6-236">**若要設定部署帳戶的權限**</span><span class="sxs-lookup"><span data-stu-id="e12f6-236">**To set up permissions for the deployment account**</span></span>

1. <span data-ttu-id="e12f6-237">如常開啟 SQL Server Management Studio。</span><span class="sxs-lookup"><span data-stu-id="e12f6-237">Open SQL Server Management Studio as before.</span></span>
2. <span data-ttu-id="e12f6-238">在**物件總管] 中**] 窗格中，以滑鼠右鍵按一下**安全性**，指向 [**新增**，然後按一下 [**登入**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-238">In the **Object Explorer** pane, right-click **Security**, point to **New**, and then click **Login**.</span></span>
3. <span data-ttu-id="e12f6-239">在**登入-新增**對話方塊中，於**登入名稱**方塊中，輸入您部署的帳戶名稱 (例如， **FABRIKAM\matt**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-239">In the **Login – New** dialog box, in the **Login name** box, type the name of your deployment account (for example, **FABRIKAM\matt**).</span></span>
4. <span data-ttu-id="e12f6-240">在**選取頁面**] 窗格中，按一下 [**伺服器角色**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-240">In the **Select a page** pane, click **Server Roles**.</span></span>
5. <span data-ttu-id="e12f6-241">選取**dbcreator**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-241">Select **dbcreator**, and then click **OK**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

<span data-ttu-id="e12f6-242">若要支援後續部署，您還需要將部署的帳戶加入**db\_擁有者**上第一次部署之後的資料庫角色。</span><span class="sxs-lookup"><span data-stu-id="e12f6-242">To support subsequent deployments, you'll also need to add the deploying account to the **db\_owner** role on the database after the first deployment.</span></span> <span data-ttu-id="e12f6-243">這是資料庫的因為後續部署您要修改現有結構描述而非建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e12f6-243">This is because on subsequent deployments you're modifying the schema of an existing database, rather than creating a new database.</span></span> <span data-ttu-id="e12f6-244">上一節所述，您無法將使用者加入資料庫角色之前明顯原因的建立資料庫，也可以。</span><span class="sxs-lookup"><span data-stu-id="e12f6-244">As described in the previous section, you can't add a user to a database role until you've created the database, for obvious reasons.</span></span>

<span data-ttu-id="e12f6-245">**若要部署的帳戶登入對應至 db\_擁有者的資料庫角色**</span><span class="sxs-lookup"><span data-stu-id="e12f6-245">**To map the deployment account login to the db\_owner database role**</span></span>

1. <span data-ttu-id="e12f6-246">如常開啟 SQL Server Management Studio。</span><span class="sxs-lookup"><span data-stu-id="e12f6-246">Open SQL Server Management Studio as before.</span></span>
2. <span data-ttu-id="e12f6-247">在**物件總管 中**視窗中，展開**安全性** 節點，展開**登入** 節點，然後按兩下 電腦帳戶登入 (比方說， **FABRIKAM\matt**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-247">In the **Object Explorer** window, expand the **Security** node, expand the **Logins** node, and then double-click the machine account login (for example, **FABRIKAM\matt**).</span></span>
3. <span data-ttu-id="e12f6-248">在**登入屬性**對話方塊中，按一下 **使用者對應**。</span><span class="sxs-lookup"><span data-stu-id="e12f6-248">In the **Login Properties** dialog box, click **User Mapping**.</span></span>
4. <span data-ttu-id="e12f6-249">在**對應到此登入的使用者**資料表中，選取您的資料庫名稱 (例如， **ContactManager**)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-249">In the **Users mapped to this login** table, select the name of your database (for example, **ContactManager**).</span></span>
5. <span data-ttu-id="e12f6-250">在**資料庫角色成員資格對象：** *[資料庫名稱]* 清單中，選取**db\_擁有者**角色。</span><span class="sxs-lookup"><span data-stu-id="e12f6-250">In the **Database role membership for:** *[database name]* list, select the **db\_owner** role.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. <span data-ttu-id="e12f6-251">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="e12f6-251">Click **OK**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e12f6-252">結論</span><span class="sxs-lookup"><span data-stu-id="e12f6-252">Conclusion</span></span>

<span data-ttu-id="e12f6-253">您的資料庫伺服器現在應該可以接受遠端資料庫部署，並允許遠端 IIS web 伺服器來存取您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e12f6-253">Your database server should now be ready to accept remote database deployments and to allow remote IIS web servers to access your databases.</span></span> <span data-ttu-id="e12f6-254">您嘗試部署與使用資料庫之前，您可能想要檢查這些關鍵點：</span><span class="sxs-lookup"><span data-stu-id="e12f6-254">Before you attempt to deploy and use databases, you may want to check these key points:</span></span>

- <span data-ttu-id="e12f6-255">您已設定成接受遠端 TCP/IP 連接的 SQL Server？</span><span class="sxs-lookup"><span data-stu-id="e12f6-255">Have you configured SQL Server to accept remote TCP/IP connections?</span></span>
- <span data-ttu-id="e12f6-256">您已設定任何防火牆以允許 SQL Server 流量？</span><span class="sxs-lookup"><span data-stu-id="e12f6-256">Have you configured any firewalls to permit SQL Server traffic?</span></span>
- <span data-ttu-id="e12f6-257">您已建立將存取 SQL Server 每個 web 伺服器的機器帳戶登入？</span><span class="sxs-lookup"><span data-stu-id="e12f6-257">Have you created a machine account login for every web server that will access SQL Server?</span></span>
- <span data-ttu-id="e12f6-258">資料庫部署是否包含指令碼來建立使用者角色對應，或您需要以手動方式建立這些之後您將資料庫部署第一次,？</span><span class="sxs-lookup"><span data-stu-id="e12f6-258">Does your database deployment include a script to create user role mappings, or do you need to create these manually after you deploy the database for the first time?</span></span>
- <span data-ttu-id="e12f6-259">您建立部署帳戶的登入並新增到**dbcreator**伺服器角色嗎？</span><span class="sxs-lookup"><span data-stu-id="e12f6-259">Have you created a login for the deployment account and added it to the **dbcreator** server role?</span></span>

## <a name="further-reading"></a><span data-ttu-id="e12f6-260">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="e12f6-260">Further Reading</span></span>

<span data-ttu-id="e12f6-261">如需部署資料庫專案的指引，請參閱[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-261">For guidance on deploying database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="e12f6-262">如需指引，以執行部署後指令碼建立資料庫角色成員資格，請參閱[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-262">For guidance on creating database role memberships by running a post-deployment script, see [Deploying Database Role Memberships to Test Environments](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).</span></span> <span data-ttu-id="e12f6-263">如需有關如何符合成員資格資料庫造成的唯一部署挑戰的指引，請參閱[部署至企業環境的成員資格資料庫](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。</span><span class="sxs-lookup"><span data-stu-id="e12f6-263">For guidance on how to meet the unique deployment challenges that membership databases pose, see [Deploying Membership Databases to Enterprise Environments](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e12f6-264">[上一頁](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [下一頁](creating-a-server-farm-with-the-web-farm-framework.md)</span><span class="sxs-lookup"><span data-stu-id="e12f6-264">[Previous](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[Next](creating-a-server-farm-with-the-web-farm-framework.md)</span></span>
