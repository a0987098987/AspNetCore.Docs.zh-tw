---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: "設定 Web 伺服器的 Web Deploy 發行 (Web Deploy 處理常式) |Microsoft 文件"
author: jrjlee
description: "本主題描述如何設定 Internet Information Services (IIS) web 伺服器，以支援網頁發佈和部署使用 IIS Web 部署韓文..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 81848c683fb9ddaa8942f030a520847a3c89fde0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a><span data-ttu-id="6e257-103">設定 Web 伺服器的 Web Deploy 發行 (Web Deploy 處理常式)</span><span class="sxs-lookup"><span data-stu-id="6e257-103">Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)</span></span>
====================
<span data-ttu-id="6e257-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="6e257-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="6e257-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="6e257-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="6e257-106">本主題描述如何設定 Internet Information Services (IIS) web 伺服器以支援網頁發佈和部署使用 IIS Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e257-106">This topic describes how to configure an Internet Information Services (IIS) web server to support web publishing and deployment using the IIS Web Deploy Handler.</span></span>
> 
> <span data-ttu-id="6e257-107">當您使用 Web Deploy 2.0 或更新版本時，有三個主要的方式，可用來取得您的應用程式或 web 伺服器上的站台。</span><span class="sxs-lookup"><span data-stu-id="6e257-107">When you work with Web Deploy 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="6e257-108">您可以：</span><span class="sxs-lookup"><span data-stu-id="6e257-108">You can:</span></span>
> 
> - <span data-ttu-id="6e257-109">使用*Web Deploy 遠端代理程式服務*。</span><span class="sxs-lookup"><span data-stu-id="6e257-109">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="6e257-110">這個方法需要較少的 web 伺服器設定，但您必須提供本機伺服器系統管理員的認證，以將任何項目部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="6e257-110">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="6e257-111">使用*Web Deploy 處理常式*。</span><span class="sxs-lookup"><span data-stu-id="6e257-111">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="6e257-112">此方法很複雜，而且需要較多的初始工作，以便設定 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="6e257-112">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="6e257-113">不過，當您使用這個方法時，您可以設定 IIS 以允許非系統管理員使用者能夠執行部署。</span><span class="sxs-lookup"><span data-stu-id="6e257-113">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="6e257-114">只有 IIS 7 或更新版本中提供 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e257-114">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="6e257-115">使用*離線部署*。</span><span class="sxs-lookup"><span data-stu-id="6e257-115">Use *offline deployment*.</span></span> <span data-ttu-id="6e257-116">這個方法會要求最低的網頁伺服器的設定，但伺服器系統管理員必須手動複製到伺服器的 web 套件並匯入它透過 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="6e257-116">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="6e257-117">如需有關主要功能、 優點，與這些方法的缺點的詳細資訊，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="6e257-117">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


<span data-ttu-id="6e257-118">是，如果您想要允許非管理員使用者將內容部署至特定的 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="6e257-118">Yes, if you want to allow non-administrator users to deploy content to specific IIS websites.</span></span> <span data-ttu-id="6e257-119">這種方法通常會希望出現在這種情況下：</span><span class="sxs-lookup"><span data-stu-id="6e257-119">This approach is often desirable in these types of scenarios:</span></span>

- <span data-ttu-id="6e257-120">預備或生產環境，其中的觸發程序遠端部署的人員或服務帳戶是不太可能得以存取伺服器系統管理員的認證。</span><span class="sxs-lookup"><span data-stu-id="6e257-120">Staging or production environments, where the person or service account that triggers the remote deployment is unlikely to have access to the credentials of a server administrator.</span></span>
- <span data-ttu-id="6e257-121">裝載的環境中，您要讓遠端使用者更新他們的網站，而不需給予其您網頁伺服器 （或其他人的網站存取） 的 完全控制的能力。</span><span class="sxs-lookup"><span data-stu-id="6e257-121">Hosted environments, where you want to give remote users the ability to update their websites without giving them full control of your web servers (or access to anyone else's websites).</span></span>

<span data-ttu-id="6e257-122">在開發或測試案例或較小的組織中，使用伺服器管理員認證的部署內容通常是小於競爭。</span><span class="sxs-lookup"><span data-stu-id="6e257-122">In development or test scenarios, or in smaller organizations, deploying content using server administrator credentials is often less contentious.</span></span> <span data-ttu-id="6e257-123">在這些情況下，設定您的 web 伺服器，以支援使用部署[Web 部署遠端代理程式服務](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)提供更直接的方法。</span><span class="sxs-lookup"><span data-stu-id="6e257-123">In these scenarios, configuring your web servers to support deployment using the [Web Deploy Remote Agent Service](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offers a more straightforward approach.</span></span>

## <a name="task-overview"></a><span data-ttu-id="6e257-124">工作概觀</span><span class="sxs-lookup"><span data-stu-id="6e257-124">Task Overview</span></span>

<span data-ttu-id="6e257-125">若要設定 web 伺服器，以接受並部署 web 封裝，從遠端電腦使用的 Web 部署的處理常式方法，您將需要：</span><span class="sxs-lookup"><span data-stu-id="6e257-125">To configure the web server to accept and deploy web packages from a remote computer using the Web Deploy Handler approach, you'll need to:</span></span>

- <span data-ttu-id="6e257-126">建立或選擇，網域使用者帳戶 （「 非系統管理員使用者 」） 來執行部署，您將使用其認證。</span><span class="sxs-lookup"><span data-stu-id="6e257-126">Create, or choose, a domain user account (the "non-administrator user") whose credentials you'll use to perform deployments.</span></span>
- <span data-ttu-id="6e257-127">安裝 IIS 7.5，包括 Web 管理服務和基本驗證模組。</span><span class="sxs-lookup"><span data-stu-id="6e257-127">Install IIS 7.5, including the Web Management Service and the Basic Authentication module.</span></span>
- <span data-ttu-id="6e257-128">安裝 Web Deploy 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="6e257-128">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="6e257-129">Web 管理服務設定為允許遠端連接，並啟動服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-129">Configure the Web Management Service to allow remote connections, and start the service.</span></span>
- <span data-ttu-id="6e257-130">建立已部署的內容裝載在 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="6e257-130">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="6e257-131">授與您在您的網站在 IIS 管理員中的非系統管理員使用者權限。</span><span class="sxs-lookup"><span data-stu-id="6e257-131">Grant your non-administrator user permissions on your website in IIS Manager.</span></span>
- <span data-ttu-id="6e257-132">請確定委派規則允許加入及變更使用您的非系統管理員使用者帳戶的網站內容服務的 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-132">Ensure that the Web Management Service delegation rules permit the service to add and change website content using your non-administrator user account.</span></span>
- <span data-ttu-id="6e257-133">設定任何防火牆以允許連接埠 8172 上的連入連線。</span><span class="sxs-lookup"><span data-stu-id="6e257-133">Configure any firewalls to allow incoming connections on port 8172.</span></span>

<span data-ttu-id="6e257-134">若要特別裝載 ContactManager 範例方案，您還需要：</span><span class="sxs-lookup"><span data-stu-id="6e257-134">To host the ContactManager sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="6e257-135">安裝.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="6e257-135">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="6e257-136">安裝 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="6e257-136">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="6e257-137">本主題將說明如何執行這種程序。</span><span class="sxs-lookup"><span data-stu-id="6e257-137">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="6e257-138">工作與本主題中的逐步解說假設您要開始使用執行 Windows Server 2008 R2 的全新的伺服器組建。</span><span class="sxs-lookup"><span data-stu-id="6e257-138">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="6e257-139">在繼續之前，請確認：</span><span class="sxs-lookup"><span data-stu-id="6e257-139">Before you continue, ensure that:</span></span>

- <span data-ttu-id="6e257-140">已安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。</span><span class="sxs-lookup"><span data-stu-id="6e257-140">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="6e257-141">伺服器已加入網域。</span><span class="sxs-lookup"><span data-stu-id="6e257-141">The server is domain-joined.</span></span>
- <span data-ttu-id="6e257-142">伺服器具有靜態 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6e257-142">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="6e257-143">如需有關如何將電腦加入網域的詳細資訊，請參閱[將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="6e257-143">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="6e257-144">如需有關如何設定靜態 IP 位址的詳細資訊，請參閱[設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="6e257-144">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="6e257-145">安裝的產品和元件</span><span class="sxs-lookup"><span data-stu-id="6e257-145">Install Products and Components</span></span>

<span data-ttu-id="6e257-146">本節將引導您在 web 伺服器上安裝必要的產品和元件。</span><span class="sxs-lookup"><span data-stu-id="6e257-146">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="6e257-147">在開始之前，最好是執行 Windows Update，以確保您的伺服器是完全處於最新狀態。</span><span class="sxs-lookup"><span data-stu-id="6e257-147">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="6e257-148">在此情況下，您需要安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="6e257-148">In this case, you need to install these things:</span></span>

- <span data-ttu-id="6e257-149">**IIS 7 建議的組態**。</span><span class="sxs-lookup"><span data-stu-id="6e257-149">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="6e257-150">這可讓**網頁伺服器 (IIS)** web 伺服器上的角色，並安裝 IIS 模組和元件，您需要以裝載 ASP.NET 應用程式集。</span><span class="sxs-lookup"><span data-stu-id="6e257-150">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="6e257-151">**IIS： 管理服務**。</span><span class="sxs-lookup"><span data-stu-id="6e257-151">**IIS: Management Service**.</span></span> <span data-ttu-id="6e257-152">這是在 IIS 中安裝 Web 管理服務 (WMSvc)。</span><span class="sxs-lookup"><span data-stu-id="6e257-152">This installs the Web Management Service (WMSvc) in IIS.</span></span> <span data-ttu-id="6e257-153">這項服務可讓您的 IIS 網站的遠端管理，並公開 （expose） 給用戶端的 Web 部署的處理常式端點。</span><span class="sxs-lookup"><span data-stu-id="6e257-153">This service enables remote management of IIS websites and exposes the Web Deploy Handler endpoint to clients.</span></span>
- <span data-ttu-id="6e257-154">**IIS： 基本驗證**。</span><span class="sxs-lookup"><span data-stu-id="6e257-154">**IIS: Basic Authentication**.</span></span> <span data-ttu-id="6e257-155">這會安裝 IIS 基本驗證模組。</span><span class="sxs-lookup"><span data-stu-id="6e257-155">This installs the IIS Basic Authentication module.</span></span> <span data-ttu-id="6e257-156">這可讓 Web 管理服務 (WMSvc) 驗證您所提供的認證。</span><span class="sxs-lookup"><span data-stu-id="6e257-156">This lets the Web Management Service (WMSvc) authenticate the credentials you provide.</span></span>
- <span data-ttu-id="6e257-157">**Web Deployment Tool 2.1 或更新版本**。</span><span class="sxs-lookup"><span data-stu-id="6e257-157">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="6e257-158">這會在您的伺服器上安裝 Web Deploy （和其基礎可執行檔，MSDeploy.exe）。</span><span class="sxs-lookup"><span data-stu-id="6e257-158">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="6e257-159">此程序的一部分，它會安裝 Web 部署處理常式，並整合 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-159">As part of this process, it installs the Web Deploy Handler and integrates it with the Web Management Service.</span></span>
- <span data-ttu-id="6e257-160">**.NET framework 4.0**。</span><span class="sxs-lookup"><span data-stu-id="6e257-160">**.NET Framework 4.0**.</span></span> <span data-ttu-id="6e257-161">如此才能執行此版本的.NET Framework 所建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6e257-161">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="6e257-162">**ASP.NET MVC 3**。</span><span class="sxs-lookup"><span data-stu-id="6e257-162">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="6e257-163">這會安裝您要執行 MVC 3 應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="6e257-163">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="6e257-164">本逐步解說說明如何使用 Web Platform Installer 安裝及設定各種元件。</span><span class="sxs-lookup"><span data-stu-id="6e257-164">This walkthrough describes the use of the Web Platform Installer to install and configure various components.</span></span> <span data-ttu-id="6e257-165">雖然您不一定要使用 Web Platform Installer，它可簡化安裝程序自動偵測相依性，並確保您一律取得最新的產品版本。</span><span class="sxs-lookup"><span data-stu-id="6e257-165">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="6e257-166">如需詳細資訊，請參閱[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="6e257-166">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="6e257-167">**若要安裝必要的產品和元件**</span><span class="sxs-lookup"><span data-stu-id="6e257-167">**To install the required products and components**</span></span>

1. <span data-ttu-id="6e257-168">下載並安裝[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="6e257-168">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="6e257-169">當安裝完成時，就會自動啟動 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="6e257-169">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e257-170">您現在可以啟動 Web Platform Installer 的任何時候**啟動**功能表。</span><span class="sxs-lookup"><span data-stu-id="6e257-170">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="6e257-171">若要這樣做，請在**啟動**功能表上，按一下 **所有程式**，然後按一下  **Microsoft Web Platform Installer**。</span><span class="sxs-lookup"><span data-stu-id="6e257-171">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="6e257-172">在頂端**Web Platform Installer 3.0**視窗中，按一下 **產品**。</span><span class="sxs-lookup"><span data-stu-id="6e257-172">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="6e257-173">在左邊視窗中，瀏覽窗格中按一下 **架構**。</span><span class="sxs-lookup"><span data-stu-id="6e257-173">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="6e257-174">在**Microsoft.NET Framework 4**資料列，如果尚未安裝.NET Framework，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-174">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e257-175">您可能已安裝.NET Framework 4.0，透過 Windows Update。</span><span class="sxs-lookup"><span data-stu-id="6e257-175">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="6e257-176">如果已安裝的產品或元件，Web Platform Installer 將會指示這取代**新增**按鈕的文字**已安裝**。</span><span class="sxs-lookup"><span data-stu-id="6e257-176">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. <span data-ttu-id="6e257-177">在**ASP.NET MVC 3 (Visual Studio 2010)**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-177">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="6e257-178">在導覽窗格中，按一下**伺服器**。</span><span class="sxs-lookup"><span data-stu-id="6e257-178">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="6e257-179">在**IIS 7 建議組態**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-179">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="6e257-180">在**Web 部署工具 2.1**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-180">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="6e257-181">在**IIS： 基本驗證**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-181">In the **IIS: Basic Authentication** row, click **Add**.</span></span>
11. <span data-ttu-id="6e257-182">在**IIS： 管理服務**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-182">In the **IIS: Management Service** row, click **Add**.</span></span>
12. <span data-ttu-id="6e257-183">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="6e257-183">Click **Install**.</span></span> <span data-ttu-id="6e257-184">Web Platform Installer 會為您顯示; 以及任何相關聯的相依性 & #x 2014;，也要安裝的產品 & #x 2014年的清單，並將會提示您接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="6e257-184">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. <span data-ttu-id="6e257-185">檢閱授權條款，然後如果您同意條款，按一下**我接受**。</span><span class="sxs-lookup"><span data-stu-id="6e257-185">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
14. <span data-ttu-id="6e257-186">當安裝完成時，按一下 **完成**，然後關閉**Web Platform Installer 3.0**視窗。</span><span class="sxs-lookup"><span data-stu-id="6e257-186">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="6e257-187">如果您安裝 IIS 之前，您會安裝.NET Framework 4.0，您需要執行[ASP.NET IIS 註冊工具](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) 向 IIS 註冊 ASP.NET 的最新版本。</span><span class="sxs-lookup"><span data-stu-id="6e257-187">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="6e257-188">如果沒有這樣做，您會發現，IIS 會提供靜態內容 （例如 HTML 檔） 不會有任何問題，但它會傳回**HTTP 錯誤 404.0 – 找不到**當您嘗試瀏覽至 ASP.NET 內容。</span><span class="sxs-lookup"><span data-stu-id="6e257-188">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="6e257-189">若要確定已註冊 ASP.NET 4.0，您可以使用下一個程序。</span><span class="sxs-lookup"><span data-stu-id="6e257-189">You can use the next procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="6e257-190">**若要向 IIS 註冊 ASP.NET 4.0**</span><span class="sxs-lookup"><span data-stu-id="6e257-190">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="6e257-191">按一下**啟動**，然後輸入**命令提示字元**。</span><span class="sxs-lookup"><span data-stu-id="6e257-191">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="6e257-192">在搜尋結果中，以滑鼠右鍵按一下**命令提示字元**，然後按一下 **系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="6e257-192">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="6e257-193">在 [命令提示字元] 視窗中，瀏覽至**%WINDIR%\Microsoft.NET\Framework\v4.0.30319**目錄。</span><span class="sxs-lookup"><span data-stu-id="6e257-193">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="6e257-194">輸入下列命令，並按 Enter:</span><span class="sxs-lookup"><span data-stu-id="6e257-194">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. <span data-ttu-id="6e257-195">如果您打算裝載在任何時間點的 64 位元 web 應用程式時，則您也應該向 IIS 註冊 64 位元版本的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="6e257-195">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="6e257-196">若要這樣做，請在命令提示字元 視窗中，導覽至**%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**目錄。</span><span class="sxs-lookup"><span data-stu-id="6e257-196">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="6e257-197">輸入下列命令，並按 Enter:</span><span class="sxs-lookup"><span data-stu-id="6e257-197">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

<span data-ttu-id="6e257-198">很好的作法是使用 Windows Update 再次此時若要下載並安裝新的產品和元件已安裝任何可用的更新。</span><span class="sxs-lookup"><span data-stu-id="6e257-198">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-web-management-service"></a><span data-ttu-id="6e257-199">設定 Web 管理服務</span><span class="sxs-lookup"><span data-stu-id="6e257-199">Configure the Web Management Service</span></span>

<span data-ttu-id="6e257-200">現在您已安裝所需的一切下, 一個步驟就是在 IIS 中設定 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-200">Now that you've installed everything you need, the next step is to configure the Web Management Service in IIS.</span></span> <span data-ttu-id="6e257-201">在高層級，您必須完成這些工作：</span><span class="sxs-lookup"><span data-stu-id="6e257-201">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="6e257-202">啟用基本驗證，伺服器層級。</span><span class="sxs-lookup"><span data-stu-id="6e257-202">Enable basic authentication at the server level.</span></span>
- <span data-ttu-id="6e257-203">Web 管理服務設定為接受遠端連接。</span><span class="sxs-lookup"><span data-stu-id="6e257-203">Configure the Web Management Service to accept remote connections.</span></span>
- <span data-ttu-id="6e257-204">啟動 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-204">Start the Web Management Service.</span></span>
- <span data-ttu-id="6e257-205">檢查必要的 Web 管理服務委派規則已就緒。</span><span class="sxs-lookup"><span data-stu-id="6e257-205">Check that the required Web Management Service delegation rules are in place.</span></span>

<span data-ttu-id="6e257-206">**若要設定 Web 管理服務**</span><span class="sxs-lookup"><span data-stu-id="6e257-206">**To configure the Web Management Service**</span></span>

1. <span data-ttu-id="6e257-207">在**啟動**功能表上，指向**系統管理工具**，然後按一下 **網際網路資訊服務 (IIS) 管理員**。</span><span class="sxs-lookup"><span data-stu-id="6e257-207">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
2. <span data-ttu-id="6e257-208">在 IIS 管理員 中，在**連線** 窗格中，按一下伺服器節點 (例如， **STAGEWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-208">In IIS Manager, in the **Connections** pane, click the server node (for example, **STAGEWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. <span data-ttu-id="6e257-209">在中央窗格中，在**IIS**，連按兩下**驗證**。</span><span class="sxs-lookup"><span data-stu-id="6e257-209">In the center pane, under **IIS**, double-click **Authentication**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. <span data-ttu-id="6e257-210">以滑鼠右鍵按一下**基本驗證**，然後按一下 **啟用**。</span><span class="sxs-lookup"><span data-stu-id="6e257-210">Right-click **Basic Authentication**, and then click **Enable**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. <span data-ttu-id="6e257-211">在**連線** 窗格中，按一下伺服器節點，以返回最上層的設定。</span><span class="sxs-lookup"><span data-stu-id="6e257-211">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>
6. <span data-ttu-id="6e257-212">在中央窗格中，在**管理**，連按兩下**管理服務**。</span><span class="sxs-lookup"><span data-stu-id="6e257-212">In the center pane, under **Management**, double-click **Management Service**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. <span data-ttu-id="6e257-213">在中央窗格中，選取**啟用遠端連接**。</span><span class="sxs-lookup"><span data-stu-id="6e257-213">In the center pane, select **Enable remote connections**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e257-214">如果 Web 管理服務已在執行中，您必須先將它停止。</span><span class="sxs-lookup"><span data-stu-id="6e257-214">If the Web Management Service is already running, you'll need to stop it first.</span></span>
8. <span data-ttu-id="6e257-215">在**動作**] 窗格中，按一下 [**啟動**啟動 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-215">In the **Actions** pane, click **Start** to start the Web Management Service.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. <span data-ttu-id="6e257-216">如果系統提示您儲存您的設定，請按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="6e257-216">If you're prompted to save your settings, click **Yes**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e257-217">您也可以設定為自動啟動服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-217">You may also want to configure the service to start automatically.</span></span> <span data-ttu-id="6e257-218">若要這樣做，請開啟 服務 主控台，以滑鼠右鍵按一下**Web 管理服務**，然後按一下 **屬性**。</span><span class="sxs-lookup"><span data-stu-id="6e257-218">To do this, open the Services console, right-click **Web Management Service**, and then click **Properties**.</span></span> <span data-ttu-id="6e257-219">在**啟動類型**下拉式清單中選取**自動**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-219">In the **Startup type** dropdown list, select **Automatic**, and then click **OK**.</span></span>
10. <span data-ttu-id="6e257-220">在**連線** 窗格中，按一下伺服器節點，以返回最上層的設定。</span><span class="sxs-lookup"><span data-stu-id="6e257-220">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>
11. <span data-ttu-id="6e257-221">在中央窗格中，在**管理**，連按兩下**管理服務委派**。</span><span class="sxs-lookup"><span data-stu-id="6e257-221">In the center pane, under **Management**, double-click **Management Service Delegation**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. <span data-ttu-id="6e257-222">請確認中間窗格包含一組規則。</span><span class="sxs-lookup"><span data-stu-id="6e257-222">Verify that the center pane contains a set of rules.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    <span data-ttu-id="6e257-223">這些規則允許使用各種 Web Deploy 提供者授權的 Web 管理服務的使用者。</span><span class="sxs-lookup"><span data-stu-id="6e257-223">These rules allow authorized Web Management Service users to use various Web Deploy providers.</span></span> <span data-ttu-id="6e257-224">例如，若要透過 Web 部署處理常式的 iis 部署 web 應用程式和內容，必須有可讓所有已驗證的 Web 管理服務使用的使用者委派規則**contentPath**和**iisApp**提供者 （上一個規則，您可以在螢幕擷取畫面）。</span><span class="sxs-lookup"><span data-stu-id="6e257-224">For example, to deploy web applications and content to IIS through the Web Deploy Handler, there must be a delegation rule that allows all authenticated Web Management Service users to use the **contentPath** and **iisApp** providers (the last rule that you can see in the screenshot).</span></span>

    <span data-ttu-id="6e257-225">如果您安裝的產品和元件，本主題中所述的順序，最新版的 Web Deploy 應該自動將所有必要的委派規則加入 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="6e257-225">If you installed products and components in the order described in this topic, the latest version of Web Deploy should automatically add all the required delegation rules to the Web Management Service.</span></span> <span data-ttu-id="6e257-226">如果管理服務委派頁面沒有顯示任何規則，您必須自行建立。</span><span class="sxs-lookup"><span data-stu-id="6e257-226">If the Management Service Delegation page does not show any rules, you'll need to create them yourself.</span></span> <span data-ttu-id="6e257-227">如需如何執行這項操作的指示，請參閱[設定的 Web 部署處理常式](https://go.microsoft.com/?linkid=9805124)。</span><span class="sxs-lookup"><span data-stu-id="6e257-227">For instructions on how to do this, see [Configure the Web Deployment Handler](https://go.microsoft.com/?linkid=9805124).</span></span>
13. <span data-ttu-id="6e257-228">在**連線** 窗格中，按一下伺服器節點，以返回最上層的設定。</span><span class="sxs-lookup"><span data-stu-id="6e257-228">In the **Connections** pane, click the server node again to return to the top-level settings.</span></span>

## <a name="create-and-configure-an-iis-website"></a><span data-ttu-id="6e257-229">建立和設定 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="6e257-229">Create and Configure an IIS Website</span></span>

<span data-ttu-id="6e257-230">您可以將 web 內容部署至您的伺服器之前，您需要建立和設定內容裝載在 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="6e257-230">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="6e257-231">Web Deploy 只可以將 web 套件部署到現有的 IIS 網站。它不能為您建立網站。</span><span class="sxs-lookup"><span data-stu-id="6e257-231">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="6e257-232">您也必須進行一些額外的設定，以允許您從遠端部署內容的非系統管理員帳戶。</span><span class="sxs-lookup"><span data-stu-id="6e257-232">You also need to do a little extra configuration to allow your non-administrator account to deploy content remotely.</span></span> <span data-ttu-id="6e257-233">在高層級，您必須完成這些工作：</span><span class="sxs-lookup"><span data-stu-id="6e257-233">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="6e257-234">若要裝載您的內容在檔案系統上建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="6e257-234">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="6e257-235">建立的 IIS 網站來提供內容，並且讓它與本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="6e257-235">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="6e257-236">授與讀取權限的本機資料夾的應用程式集區身分識別。</span><span class="sxs-lookup"><span data-stu-id="6e257-236">Grant read permissions to the application pool identity on the local folder.</span></span>
- <span data-ttu-id="6e257-237">授與必要的 IIS 權限，會將您的 web 應用程式部署的網域帳戶。</span><span class="sxs-lookup"><span data-stu-id="6e257-237">Grant the necessary IIS permissions to the domain account that will deploy your web application.</span></span>

<span data-ttu-id="6e257-238">雖然沒有停止將內容部署到預設的網站在 IIS 中的項目，這個方法不會建議測試或示範案例以外的任何項目。</span><span class="sxs-lookup"><span data-stu-id="6e257-238">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="6e257-239">若要模擬實際執行環境，您應該建立新的 IIS 網站應用程式的需求所特定的設定。</span><span class="sxs-lookup"><span data-stu-id="6e257-239">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="6e257-240">**若要建立 IIS 網站**</span><span class="sxs-lookup"><span data-stu-id="6e257-240">**To create an IIS website**</span></span>

1. <span data-ttu-id="6e257-241">在本機檔案系統上，建立資料夾來儲存您的內容 (例如， **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-241">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="6e257-242">在**啟動**功能表上，指向**系統管理工具**，然後按一下 **網際網路資訊服務 (IIS) 管理員**。</span><span class="sxs-lookup"><span data-stu-id="6e257-242">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="6e257-243">在 IIS 管理員 中，在**連線** 窗格中，展開伺服器節點 (例如， **STAGEWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-243">In IIS Manager, in the **Connections** pane, expand the server node (for example, **STAGEWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. <span data-ttu-id="6e257-244">以滑鼠右鍵按一下**網站**節點，然後再按一下**新增網站**。</span><span class="sxs-lookup"><span data-stu-id="6e257-244">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="6e257-245">在**站台名稱**方塊中，輸入 IIS 網站的名稱 (例如， **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-245">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="6e257-246">在**實體路徑**方塊中，輸入 （或瀏覽至） 您的本機資料夾的路徑 (例如， **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-246">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="6e257-247">在**連接埠**方塊中，輸入您要裝載網站的連接埠號碼 (例如， **85**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-247">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e257-248">標準連接埠號碼為 80 的 HTTP 和 HTTPS 為 443。</span><span class="sxs-lookup"><span data-stu-id="6e257-248">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="6e257-249">不過，如果您主控此網站連接埠 80 上的，您必須停止預設網站，才能存取您的網站。</span><span class="sxs-lookup"><span data-stu-id="6e257-249">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="6e257-250">保留**主機名稱**方塊保留空白，除非您想要設定網站的網域名稱系統 (DNS) 記錄，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-250">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > <span data-ttu-id="6e257-251">在實際執行環境中，您可能會想將網站連接埠 80 上的主控，並設定主機標頭，以及比對 DNS 記錄。</span><span class="sxs-lookup"><span data-stu-id="6e257-251">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="6e257-252">如需有關如何在 IIS 7 中設定主機標頭的詳細資訊，請參閱[設定網站 (IIS 7) 的主機標頭](https://technet.microsoft.com/library/cc753195(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="6e257-252">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="6e257-253">如需有關 Windows Server 2008 R2 中 DNS 伺服器角色的詳細資訊，請參閱[DNS 伺服器概觀](https://technet.microsoft.com/en-gb/library/cc770392.aspx)和[DNS 伺服器](https://technet.microsoft.com/windowsserver/dd448607)。</span><span class="sxs-lookup"><span data-stu-id="6e257-253">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="6e257-254">在 [ **動作** ] 窗格的 [ **編輯站台**] 下方，按一下 [ **繫結**]。</span><span class="sxs-lookup"><span data-stu-id="6e257-254">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="6e257-255">在**站台繫結**對話方塊中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-255">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. <span data-ttu-id="6e257-256">在**新增網站繫結** 對話方塊中，將**IP 位址**和**連接埠**以符合您現有的站台設定。</span><span class="sxs-lookup"><span data-stu-id="6e257-256">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="6e257-257">在**主機名稱**方塊中，輸入您的網頁伺服器的名稱 (例如， **STAGEWEB1**)，然後按一下  **確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-257">In the **Host name** box, type the name of your web server (for example, **STAGEWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > <span data-ttu-id="6e257-258">第一個站台繫結可讓您存取在本機使用的 IP 位址和連接埠的站台或`http://localhost:85`。</span><span class="sxs-lookup"><span data-stu-id="6e257-258">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="6e257-259">第二個網站繫結可讓您從使用電腦名稱 (例如，http://stageweb1:85) 的網域上的其他電腦存取這個網站。</span><span class="sxs-lookup"><span data-stu-id="6e257-259">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://stageweb1:85).</span></span>
13. <span data-ttu-id="6e257-260">在**站台繫結**對話方塊中，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="6e257-260">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="6e257-261">在**連線**] 窗格中，按一下 [**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="6e257-261">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="6e257-262">在**應用程式集區**] 窗格中，以滑鼠右鍵按一下您的應用程式集區的名稱，然後按一下 [**基本設定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-262">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="6e257-263">根據預設，應用程式集區的名稱會符合網站的名稱 (例如， **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-263">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="6e257-264">在**.NET Framework 版本**清單中，選取**.NET Framework v4.0.30319**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-264">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > <span data-ttu-id="6e257-265">範例解決方案需要.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="6e257-265">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="6e257-266">這不是需要 Web Deploy 一般。</span><span class="sxs-lookup"><span data-stu-id="6e257-266">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="6e257-267">為了讓您的網站提供內容，應用程式集區識別必須擁有讀取權限將內容儲存的本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="6e257-267">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="6e257-268">在 IIS 7.5 中應用程式集區具有唯一的應用程式集區身分識別執行預設 （相較於舊版的 IIS，其中應用程式集區通常會使用 Network Service 帳戶）。</span><span class="sxs-lookup"><span data-stu-id="6e257-268">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="6e257-269">應用程式集區識別不是真正的使用者帳戶，並不會顯示任何所列之使用者或群組 & #x 2014年; 相反地，它會動態建立啟動應用程式集區時。</span><span class="sxs-lookup"><span data-stu-id="6e257-269">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="6e257-270">每個應用程式集區識別加入至本機**IIS\_IUSRS**安全性群組做為隱藏的項目。</span><span class="sxs-lookup"><span data-stu-id="6e257-270">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="6e257-271">若要授與應用程式集區識別的檔案或資料夾上的權限，您有兩個選項：</span><span class="sxs-lookup"><span data-stu-id="6e257-271">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="6e257-272">指派的權限的應用程式集區識別直接使用格式**IIS AppPool\***[應用程式集區名稱] * (例如， **IIS AppPool\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6e257-272">Assign permissions to the application pool identity directly, using the format **IIS AppPool\***[application pool name]*(for example, **IIS AppPool\DemoSite**).</span></span>
- <span data-ttu-id="6e257-273">指派的權限**IIS\_IUSRS**群組。</span><span class="sxs-lookup"><span data-stu-id="6e257-273">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="6e257-274">最常見的作法是將權限指派給本機**IIS\_IUSRS**群組，因為這種方法可讓您變更應用程式集區不需要重新設定檔案系統權限。</span><span class="sxs-lookup"><span data-stu-id="6e257-274">The most common approach is to assign permissions to the local **IIS\_IUSRS** group, because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="6e257-275">下一個程序會使用此群組為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="6e257-275">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="6e257-276">如需在 IIS 7.5 中的應用程式集區識別的詳細資訊，請參閱[應用程式集區識別](https://go.microsoft.com/?linkid=9805123)。</span><span class="sxs-lookup"><span data-stu-id="6e257-276">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="6e257-277">**若要設定 IIS 網站的資料夾權限**</span><span class="sxs-lookup"><span data-stu-id="6e257-277">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="6e257-278">在 Windows 檔案總管，瀏覽至您的本機資料夾的位置。</span><span class="sxs-lookup"><span data-stu-id="6e257-278">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="6e257-279">以滑鼠右鍵按一下資料夾，然後按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="6e257-279">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="6e257-280">在**安全性**索引標籤上，按一下 **編輯**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="6e257-280">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="6e257-281">按一下**位置**。</span><span class="sxs-lookup"><span data-stu-id="6e257-281">Click **Locations**.</span></span> <span data-ttu-id="6e257-282">在**位置**對話方塊中，選取 本機伺服器，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-282">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. <span data-ttu-id="6e257-283">在**選取使用者或群組**] 對話方塊中，輸入**IIS\_IUSRS**，按一下**檢查名稱**，然後按一下 [**確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-283">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="6e257-284">在 **權限 * * * [資料夾名稱]*對話方塊中，請注意，新的群組已經被委派**讀取&amp;執行**，**列出資料夾內容**，和**讀取**預設權限。</span><span class="sxs-lookup"><span data-stu-id="6e257-284">In the **Permissions for***[folder name]* dialog box, notice that the new group has been assigned the **Read &amp; execute**, **List folder contents**, and **Read** permissions by default.</span></span> <span data-ttu-id="6e257-285">保留不變，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-285">Leave this unchanged and click **OK**.</span></span>
7. <span data-ttu-id="6e257-286">按一下**確定**關閉*[資料夾名稱] * * * 屬性** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="6e257-286">Click **OK** to close the *[folder name]***Properties** dialog box.</span></span>

<span data-ttu-id="6e257-287">做為最後的工作，您必須非管理員的使用者將內容部署，您將使用其認證授與適當的權限。</span><span class="sxs-lookup"><span data-stu-id="6e257-287">As a final task, you must grant the appropriate permissions to the non-administrator user whose credentials you'll use to deploy content.</span></span> <span data-ttu-id="6e257-288">這位使用者需要從遠端將內容部署至您的網站的權限。</span><span class="sxs-lookup"><span data-stu-id="6e257-288">This user requires the permissions to deploy content remotely to your website.</span></span>

<span data-ttu-id="6e257-289">**若要設定 IIS 網站的權限的非系統管理員的網域使用者**</span><span class="sxs-lookup"><span data-stu-id="6e257-289">**To configure IIS website permissions for a non-administrator domain user**</span></span>

1. <span data-ttu-id="6e257-290">在 IIS 管理員中，在**連線** 窗格中，以滑鼠右鍵按一下 網站 節點 (比方說， **DemoSite**)，指向**部署**，然後按一下**設定 Web部署發行**。</span><span class="sxs-lookup"><span data-stu-id="6e257-290">In IIS Manager, in the **Connections** pane, right-click your website node (for example, **DemoSite**), point to **Deploy**, and then click **Configure Web Deploy Publishing**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. <span data-ttu-id="6e257-291">在**設定 Web 部署發行**對話方塊中的，右邊**選取要發佈的權限授與使用者**清單中，按一下省略符號按鈕。</span><span class="sxs-lookup"><span data-stu-id="6e257-291">In the **Configure Web Deploy Publishing** dialog box, to the right of the **Select a user to give publishing permissions** list, click the ellipsis button.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. <span data-ttu-id="6e257-292">在**允許使用者**對話方塊方塊中，輸入您想要部署的內容，然後按一下使用的帳戶的網域和使用者名稱**確定**。</span><span class="sxs-lookup"><span data-stu-id="6e257-292">In the **Allow User** dialog box, type the domain and user name of the account you want to use to deploy content, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. <span data-ttu-id="6e257-293">在**設定 Web 部署發行**對話方塊中，按一下 **安裝**。</span><span class="sxs-lookup"><span data-stu-id="6e257-293">In the **Configure Web Deploy Publishing** dialog box, click **Setup**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > <span data-ttu-id="6e257-294">這項作業在一個步驟中，執行兩個索引鍵的函式。</span><span class="sxs-lookup"><span data-stu-id="6e257-294">This operation performs two key functions in one step.</span></span> <span data-ttu-id="6e257-295">首先，它會授與使用者的權限修改 Web 管理服務，透過遠端網站根據您檢查上一節中的委派規則。</span><span class="sxs-lookup"><span data-stu-id="6e257-295">First, it grants the user permission to modify the website remotely through the Web Management Service, according to the delegation rules you examined in the previous section.</span></span> <span data-ttu-id="6e257-296">第二，它會授與使用者的網站，可讓使用者加入、 修改及設定網站內容的權限的來源資料夾的完整控制。</span><span class="sxs-lookup"><span data-stu-id="6e257-296">Second, it grants the user full control of the source folder for the website, which allows the user to add, modify, and set permissions on the website content.</span></span>
5. <span data-ttu-id="6e257-297">在**設定 Web 部署發行**對話方塊中，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="6e257-297">In the **Configure Web Deploy Publishing** dialog box, click **Close**.</span></span>

## <a name="configure-firewall-exceptions"></a><span data-ttu-id="6e257-298">設定防火牆例外</span><span class="sxs-lookup"><span data-stu-id="6e257-298">Configure Firewall Exceptions</span></span>

<span data-ttu-id="6e257-299">根據預設，IIS Web 管理服務會接聽 TCP 連接埠 8172 上。</span><span class="sxs-lookup"><span data-stu-id="6e257-299">By default, the IIS Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="6e257-300">如果您的網頁伺服器上已啟用 Windows 防火牆，您必須建立新的輸入的規則以允許連接埠 8172 上的 TCP 流量 （根據預設，在 Windows 防火牆允許所有輸出流量）。</span><span class="sxs-lookup"><span data-stu-id="6e257-300">If Windows Firewall is enabled on your web server, you'll need to create a new inbound rule to allow TCP traffic on port 8172 (all outbound traffic is permitted by default in Windows Firewall).</span></span> <span data-ttu-id="6e257-301">如果您使用協力廠商防火牆，您必須建立規則以允許流量。</span><span class="sxs-lookup"><span data-stu-id="6e257-301">If you use a third-party firewall, you'll need to create rules to allow traffic.</span></span>

| <span data-ttu-id="6e257-302">Direction</span><span class="sxs-lookup"><span data-stu-id="6e257-302">Direction</span></span> | <span data-ttu-id="6e257-303">從連接埠</span><span class="sxs-lookup"><span data-stu-id="6e257-303">From Port</span></span> | <span data-ttu-id="6e257-304">連接埠</span><span class="sxs-lookup"><span data-stu-id="6e257-304">To Port</span></span> | <span data-ttu-id="6e257-305">連接埠類型</span><span class="sxs-lookup"><span data-stu-id="6e257-305">Port Type</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6e257-306">輸入</span><span class="sxs-lookup"><span data-stu-id="6e257-306">Inbound</span></span> | <span data-ttu-id="6e257-307">任何</span><span class="sxs-lookup"><span data-stu-id="6e257-307">Any</span></span> | <span data-ttu-id="6e257-308">8172</span><span class="sxs-lookup"><span data-stu-id="6e257-308">8172</span></span> | <span data-ttu-id="6e257-309">TCP</span><span class="sxs-lookup"><span data-stu-id="6e257-309">TCP</span></span> |
| <span data-ttu-id="6e257-310">輸出</span><span class="sxs-lookup"><span data-stu-id="6e257-310">Outbound</span></span> | <span data-ttu-id="6e257-311">8172</span><span class="sxs-lookup"><span data-stu-id="6e257-311">8172</span></span> | <span data-ttu-id="6e257-312">任何</span><span class="sxs-lookup"><span data-stu-id="6e257-312">Any</span></span> | <span data-ttu-id="6e257-313">TCP</span><span class="sxs-lookup"><span data-stu-id="6e257-313">TCP</span></span> |
  

<span data-ttu-id="6e257-314">如需有關如何在 Windows 防火牆中設定規則的詳細資訊，請參閱[設定防火牆規則](https://technet.microsoft.com/library/dd448559(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="6e257-314">For more information on configuring rules in Windows Firewall, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="6e257-315">針對協力廠商防火牆，請參閱產品文件。</span><span class="sxs-lookup"><span data-stu-id="6e257-315">For third-party firewalls, please consult your product documentation.</span></span>

## <a name="conclusion"></a><span data-ttu-id="6e257-316">結論</span><span class="sxs-lookup"><span data-stu-id="6e257-316">Conclusion</span></span>

<span data-ttu-id="6e257-317">您的網頁伺服器現在應該準備好接受遠端部署到透過 Web 管理服務的 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e257-317">Your web server should now be ready to accept remote deployments to the Web Deploy Handler through the Web Management Service.</span></span> <span data-ttu-id="6e257-318">您嘗試部署至伺服器的 web 應用程式之前，您可能想要檢查這些關鍵點：</span><span class="sxs-lookup"><span data-stu-id="6e257-318">Before you attempt to deploy a web application to the server, you may want to check these key points:</span></span>

- <span data-ttu-id="6e257-319">您已啟用的伺服器層級在 IIS 中的基本驗證？</span><span class="sxs-lookup"><span data-stu-id="6e257-319">Have you enabled basic authentication at the server level in IIS?</span></span>
- <span data-ttu-id="6e257-320">您已啟用遠端連線到 Web 管理服務嗎？</span><span class="sxs-lookup"><span data-stu-id="6e257-320">Have you enabled remote connections to the Web Management Service?</span></span>
- <span data-ttu-id="6e257-321">您已啟動 Web 管理服務嗎？</span><span class="sxs-lookup"><span data-stu-id="6e257-321">Have you started the Web Management Service?</span></span>
- <span data-ttu-id="6e257-322">有管理服務委派規則就地嗎？</span><span class="sxs-lookup"><span data-stu-id="6e257-322">Are there management service delegation rules in place?</span></span>
- <span data-ttu-id="6e257-323">應用程式集區身分識別是否有讀取權限為您的網站的 [來源] 資料夾？</span><span class="sxs-lookup"><span data-stu-id="6e257-323">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="6e257-324">非系統管理員使用者帳戶沒有站台層級權限，在 IIS 中？</span><span class="sxs-lookup"><span data-stu-id="6e257-324">Does the non-administrator user account have site-level permissions in IIS?</span></span>
- <span data-ttu-id="6e257-325">您的防火牆允許 TCP 連接埠 8172 上伺服器的連入連線嗎？</span><span class="sxs-lookup"><span data-stu-id="6e257-325">Does your firewall allow incoming connections to the server on TCP port 8172?</span></span>

## <a name="further-reading"></a><span data-ttu-id="6e257-326">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="6e257-326">Further Reading</span></span>

<span data-ttu-id="6e257-327">如需如何設定自訂的 Microsoft Build Engine (MSBuild) 專案檔，以將 web 套件部署到 Web 部署處理常式的指引，請參閱[目標環境設定部署屬性](configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="6e257-327">For guidance on how to configure custom Microsoft Build Engine (MSBuild) project files to deploy web packages to the Web Deploy Handler, see [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6e257-328">[上一頁](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[下一頁](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="6e257-328">[Previous](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)</span></span>
