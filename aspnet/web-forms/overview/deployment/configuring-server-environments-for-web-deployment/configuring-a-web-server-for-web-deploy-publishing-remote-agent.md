---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: "設定 Web 伺服器的 Web Deploy 發行 （遠端代理程式） |Microsoft 文件"
author: jrjlee
description: "本主題描述如何設定 Internet Information Services (IIS) web 伺服器，以支援網頁發佈和使用 IIS Web 部署的部署..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 23195949121cd13ca4b1809b8db91a7320d1fdd2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a><span data-ttu-id="a36f6-103">設定 Web 伺服器的 Web Deploy 發行 （遠端代理程式）</span><span class="sxs-lookup"><span data-stu-id="a36f6-103">Configuring a Web Server for Web Deploy Publishing (Remote Agent)</span></span>
====================
<span data-ttu-id="a36f6-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a36f6-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a36f6-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="a36f6-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a36f6-106">本主題描述如何設定 Internet Information Services (IIS) web 伺服器，以支援網頁發佈和使用 IIS Web Deployment Tool (Web Deploy) 的遠端代理程式服務的部署。</span><span class="sxs-lookup"><span data-stu-id="a36f6-106">This topic describes how to configure an Internet Information Services (IIS) web server to support web publishing and deployment using the IIS Web Deployment Tool (Web Deploy) Remote Agent Service.</span></span>
> 
> <span data-ttu-id="a36f6-107">當您使用 Web Deploy 2.0 或更新版本時，有三個主要的方式，可用來取得您的應用程式或 web 伺服器上的站台。</span><span class="sxs-lookup"><span data-stu-id="a36f6-107">When you work with Web Deploy 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="a36f6-108">您可以：</span><span class="sxs-lookup"><span data-stu-id="a36f6-108">You can:</span></span>
> 
> - <span data-ttu-id="a36f6-109">使用*Web Deploy 遠端代理程式服務*。</span><span class="sxs-lookup"><span data-stu-id="a36f6-109">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="a36f6-110">這個方法需要較少的 web 伺服器設定，但您必須提供本機伺服器系統管理員的認證，以將任何項目部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="a36f6-110">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="a36f6-111">使用*Web Deploy 處理常式*。</span><span class="sxs-lookup"><span data-stu-id="a36f6-111">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="a36f6-112">此方法很複雜，而且需要較多的初始工作，以便設定 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a36f6-112">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="a36f6-113">不過，當您使用這個方法時，您可以設定 IIS 以允許非系統管理員使用者能夠執行部署。</span><span class="sxs-lookup"><span data-stu-id="a36f6-113">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="a36f6-114">只有 IIS 7 或更新版本中提供 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="a36f6-114">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="a36f6-115">使用*離線部署*。</span><span class="sxs-lookup"><span data-stu-id="a36f6-115">Use *offline deployment*.</span></span> <span data-ttu-id="a36f6-116">這個方法會要求最低的網頁伺服器的設定，但伺服器系統管理員必須手動複製到伺服器的 web 套件並匯入它透過 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="a36f6-116">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="a36f6-117">如需有關主要功能、 優點，與這些方法的缺點的詳細資訊，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-117">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a><span data-ttu-id="a36f6-118">是 Web Deploy 為您的遠端代理程式正確的方法？</span><span class="sxs-lookup"><span data-stu-id="a36f6-118">Is the Web Deploy Remote Agent the Right Approach for You?</span></span>

<span data-ttu-id="a36f6-119">是，如果將內容部署的使用者可以提供的目的地伺服器上的系統管理員認證。</span><span class="sxs-lookup"><span data-stu-id="a36f6-119">Yes, if the user who will deploy the content can supply the credentials of an administrator on the destination server.</span></span> <span data-ttu-id="a36f6-120">這種方法通常會希望出現在這種情況下：</span><span class="sxs-lookup"><span data-stu-id="a36f6-120">This approach is often desirable in these types of scenarios:</span></span>

- <span data-ttu-id="a36f6-121">開發或測試環境，其中的開發人員可以在目的地 web 伺服器和資料庫伺服器的完整控制權。</span><span class="sxs-lookup"><span data-stu-id="a36f6-121">Development or test environments, where the developer has full control over the destination web server and database server.</span></span>
- <span data-ttu-id="a36f6-122">單一使用者或一小群使用者具有整個應用程式生命週期的控制權較小的組織。</span><span class="sxs-lookup"><span data-stu-id="a36f6-122">Smaller organizations in which a single user or a small group of users has control over the entire application lifecycle.</span></span>

<span data-ttu-id="a36f6-123">中大型組織，以及特別是針對預備環境或生產環境很多，它通常是不實際，讓使用者在 web 伺服器上的系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="a36f6-123">In lots of larger organizations, and particularly for staging or production environments, it's often not realistic to give users administrator rights on web servers.</span></span> <span data-ttu-id="a36f6-124">若為託管的 web 伺服器，這是特別不太可能會發生這個情況。</span><span class="sxs-lookup"><span data-stu-id="a36f6-124">In the case of hosted web servers, this is especially unlikely to be the case.</span></span> <span data-ttu-id="a36f6-125">此外，如果您計劃要從組建伺服器的部署作業自動化，您可能不想要用於部署程序的系統管理員認證。</span><span class="sxs-lookup"><span data-stu-id="a36f6-125">In addition, if you're planning to automate deployment from a build server, you may not want to use administrator credentials for the deployment process.</span></span> <span data-ttu-id="a36f6-126">在這些情況下，設定您的 web 伺服器，以支援使用部署[Web 部署的處理常式](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)可能會提供更令人滿意的選擇。</span><span class="sxs-lookup"><span data-stu-id="a36f6-126">In these scenarios, configuring your web servers to support deployment using the [Web Deploy Handler](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) may provide a more satisfactory choice.</span></span>

## <a name="task-overview"></a><span data-ttu-id="a36f6-127">工作概觀</span><span class="sxs-lookup"><span data-stu-id="a36f6-127">Task Overview</span></span>

<span data-ttu-id="a36f6-128">本主題描述如何在網際網路資訊服務 (IIS) 7.5 網頁伺服器設定成接受及部署 web 封裝，從遠端電腦使用網路部署遠端代理程式的方法。</span><span class="sxs-lookup"><span data-stu-id="a36f6-128">This topic describes how to configure an Internet Information Services (IIS) 7.5 web server to accept and deploy web packages from a remote computer using the Web Deploy Remote Agent approach.</span></span> <span data-ttu-id="a36f6-129">您將需要：</span><span class="sxs-lookup"><span data-stu-id="a36f6-129">You'll need to:</span></span>

- <span data-ttu-id="a36f6-130">安裝 IIS 7.5 和 IIS 7 建議的組態。</span><span class="sxs-lookup"><span data-stu-id="a36f6-130">Install IIS 7.5 and the IIS 7 recommended configuration.</span></span>
- <span data-ttu-id="a36f6-131">安裝 Web Deploy 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="a36f6-131">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="a36f6-132">建立已部署的內容裝載在 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="a36f6-132">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="a36f6-133">確定 Web Deployment Agent Service 正在執行。</span><span class="sxs-lookup"><span data-stu-id="a36f6-133">Ensure that the Web Deployment Agent Service is running.</span></span>

<span data-ttu-id="a36f6-134">若要特別裝載範例方案，您還需要：</span><span class="sxs-lookup"><span data-stu-id="a36f6-134">To host the sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="a36f6-135">安裝.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="a36f6-135">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="a36f6-136">安裝 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="a36f6-136">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="a36f6-137">本主題將說明如何執行這種程序。</span><span class="sxs-lookup"><span data-stu-id="a36f6-137">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="a36f6-138">工作與本主題中的逐步解說假設您要開始使用執行 Windows Server 2008 R2 的全新的伺服器組建。</span><span class="sxs-lookup"><span data-stu-id="a36f6-138">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="a36f6-139">在繼續之前，請確認：</span><span class="sxs-lookup"><span data-stu-id="a36f6-139">Before you continue, ensure that:</span></span>

- <span data-ttu-id="a36f6-140">已安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。</span><span class="sxs-lookup"><span data-stu-id="a36f6-140">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="a36f6-141">伺服器已加入網域。</span><span class="sxs-lookup"><span data-stu-id="a36f6-141">The server is domain-joined.</span></span>
- <span data-ttu-id="a36f6-142">伺服器具有靜態 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="a36f6-142">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="a36f6-143">如需有關如何將電腦加入網域的詳細資訊，請參閱[將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-143">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="a36f6-144">如需有關如何設定靜態 IP 位址的詳細資訊，請參閱[設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-144">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span> <span data-ttu-id="a36f6-145">遠端代理程式服務的 IIS 6 及更新版本支援，並不需要加入網域。</span><span class="sxs-lookup"><span data-stu-id="a36f6-145">The Remote Agent service is supported by IIS 6 onwards and does not require you to be joined to a domain.</span></span> <span data-ttu-id="a36f6-146">不過，在本教學課程步驟開發及測試在 IIS 7.5 上，為其他版本的程序可能會不同。</span><span class="sxs-lookup"><span data-stu-id="a36f6-146">However, the steps in this tutorial were developed and tested on IIS 7.5 and procedures for other versions may vary.</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="a36f6-147">安裝的產品和元件</span><span class="sxs-lookup"><span data-stu-id="a36f6-147">Install Products and Components</span></span>

<span data-ttu-id="a36f6-148">本節將引導您在 web 伺服器上安裝必要的產品和元件。</span><span class="sxs-lookup"><span data-stu-id="a36f6-148">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="a36f6-149">在開始之前，最好是執行 Windows Update，以確保您的伺服器是完全處於最新狀態。</span><span class="sxs-lookup"><span data-stu-id="a36f6-149">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="a36f6-150">在此情況下，您需要安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="a36f6-150">In this case, you need to install these things:</span></span>

- <span data-ttu-id="a36f6-151">**IIS 7 建議的組態**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-151">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="a36f6-152">這可讓**網頁伺服器 (IIS)** web 伺服器上的角色，並安裝 IIS 模組和元件，您需要以裝載 ASP.NET 應用程式集。</span><span class="sxs-lookup"><span data-stu-id="a36f6-152">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="a36f6-153">**.NET framework 4.0**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-153">**.NET Framework 4.0**.</span></span> <span data-ttu-id="a36f6-154">如此才能執行此版本的.NET Framework 所建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a36f6-154">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="a36f6-155">**Web Deployment Tool 2.1 或更新版本**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-155">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="a36f6-156">這會在您的伺服器上安裝 Web Deploy （和其基礎可執行檔，MSDeploy.exe）。</span><span class="sxs-lookup"><span data-stu-id="a36f6-156">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="a36f6-157">此程序的一部分，它將會安裝並啟動 Web 部署代理程式服務。</span><span class="sxs-lookup"><span data-stu-id="a36f6-157">As part of this process, it installs and starts the Web Deployment Agent Service.</span></span> <span data-ttu-id="a36f6-158">此服務可讓您部署 web 封裝，從遠端電腦。</span><span class="sxs-lookup"><span data-stu-id="a36f6-158">This service lets you deploy web packages from a remote computer.</span></span>
- <span data-ttu-id="a36f6-159">**ASP.NET MVC 3**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-159">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="a36f6-160">這會安裝您要執行 MVC 3 應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="a36f6-160">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="a36f6-161">本逐步解說說明如何使用 Web Platform Installer 安裝及設定的必要的元件。</span><span class="sxs-lookup"><span data-stu-id="a36f6-161">This walkthrough describes the use of the Web Platform Installer to install and configure the required components.</span></span> <span data-ttu-id="a36f6-162">雖然您不一定要使用 Web Platform Installer，它可簡化安裝程序自動偵測相依性，並確保您一律取得最新的產品版本。</span><span class="sxs-lookup"><span data-stu-id="a36f6-162">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="a36f6-163">如需詳細資訊，請參閱[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-163">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="a36f6-164">**若要安裝必要的產品和元件**</span><span class="sxs-lookup"><span data-stu-id="a36f6-164">**To install the required products and components**</span></span>

1. <span data-ttu-id="a36f6-165">下載並安裝[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-165">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="a36f6-166">當安裝完成時，就會自動啟動 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="a36f6-166">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a36f6-167">您現在可以啟動 Web Platform Installer 的任何時候**啟動**功能表。</span><span class="sxs-lookup"><span data-stu-id="a36f6-167">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="a36f6-168">若要這樣做，請在**啟動**功能表上，按一下 **所有程式**，然後按一下  **Microsoft Web Platform Installer**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-168">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="a36f6-169">在頂端**Web Platform Installer 3.0**視窗中，按一下 **產品**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-169">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="a36f6-170">在左邊視窗中，瀏覽窗格中按一下 **架構**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-170">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="a36f6-171">在**Microsoft.NET Framework 4**資料列，如果尚未安裝.NET Framework，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-171">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a36f6-172">您可能已安裝.NET Framework 4.0，透過 Windows Update。</span><span class="sxs-lookup"><span data-stu-id="a36f6-172">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="a36f6-173">如果已安裝的產品或元件，Web Platform Installer 將會指示這取代**新增**按鈕的文字**已安裝**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-173">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. <span data-ttu-id="a36f6-174">在**ASP.NET MVC 3 (Visual Studio 2010)**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-174">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="a36f6-175">在導覽窗格中，按一下**伺服器**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-175">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="a36f6-176">在**IIS 7 建議組態**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-176">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="a36f6-177">在**Web 部署工具 2.1**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-177">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="a36f6-178">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="a36f6-178">Click **Install**.</span></span> <span data-ttu-id="a36f6-179">Web Platform Installer 會為您顯示; 以及任何相關聯的相依性 & #x 2014;，也要安裝的產品 & #x 2014年的清單，並將會提示您接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="a36f6-179">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. <span data-ttu-id="a36f6-180">檢閱授權條款，然後如果您同意條款，按一下**我接受**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-180">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="a36f6-181">當安裝完成時，按一下 **完成**，然後關閉**Web Platform Installer 3.0**視窗。</span><span class="sxs-lookup"><span data-stu-id="a36f6-181">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="a36f6-182">如果您安裝 IIS 之前，您會安裝.NET Framework 4.0，您需要執行[ASP.NET IIS 註冊工具](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) 向 IIS 註冊 ASP.NET 的最新版本。</span><span class="sxs-lookup"><span data-stu-id="a36f6-182">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="a36f6-183">如果沒有這樣做，您會發現，IIS 會提供靜態內容 （例如 HTML 檔） 不會有任何問題，但它會傳回**HTTP 錯誤 404.0 – 找不到**當您嘗試瀏覽至 ASP.NET 內容。</span><span class="sxs-lookup"><span data-stu-id="a36f6-183">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="a36f6-184">您可以使用此程序，確定已註冊 ASP.NET 4.0。</span><span class="sxs-lookup"><span data-stu-id="a36f6-184">You can use this procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="a36f6-185">**若要向 IIS 註冊 ASP.NET 4.0**</span><span class="sxs-lookup"><span data-stu-id="a36f6-185">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="a36f6-186">按一下**啟動**，然後輸入**命令提示字元**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-186">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="a36f6-187">在搜尋結果中，以滑鼠右鍵按一下**命令提示字元**，然後按一下 **系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-187">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="a36f6-188">在 [命令提示字元] 視窗中，瀏覽至**%WINDIR%\Microsoft.NET\Framework\v4.0.30319**目錄。</span><span class="sxs-lookup"><span data-stu-id="a36f6-188">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="a36f6-189">輸入下列命令，並按 Enter:</span><span class="sxs-lookup"><span data-stu-id="a36f6-189">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. <span data-ttu-id="a36f6-190">如果您打算裝載在任何時間點的 64 位元 web 應用程式時，則您也應該向 IIS 註冊 64 位元版本的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="a36f6-190">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="a36f6-191">若要這樣做，請在命令提示字元 視窗中，導覽至**%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**目錄。</span><span class="sxs-lookup"><span data-stu-id="a36f6-191">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="a36f6-192">輸入下列命令，並按 Enter:</span><span class="sxs-lookup"><span data-stu-id="a36f6-192">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

<span data-ttu-id="a36f6-193">很好的作法是使用 Windows Update 再次此時若要下載並安裝新的產品和元件已安裝任何可用的更新。</span><span class="sxs-lookup"><span data-stu-id="a36f6-193">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-iis-website"></a><span data-ttu-id="a36f6-194">設定 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="a36f6-194">Configure the IIS Website</span></span>

<span data-ttu-id="a36f6-195">您可以將 web 內容部署至您的伺服器之前，您需要建立和設定內容裝載在 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="a36f6-195">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="a36f6-196">Web Deploy 只可以將 web 套件部署到現有的 IIS 網站。它不能為您建立網站。</span><span class="sxs-lookup"><span data-stu-id="a36f6-196">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="a36f6-197">在高層級，您必須完成這些工作：</span><span class="sxs-lookup"><span data-stu-id="a36f6-197">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="a36f6-198">若要裝載您的內容在檔案系統上建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="a36f6-198">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="a36f6-199">建立的 IIS 網站來提供內容，並且讓它與本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="a36f6-199">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="a36f6-200">授與讀取權限的本機資料夾的應用程式集區身分識別。</span><span class="sxs-lookup"><span data-stu-id="a36f6-200">Grant read permissions to the application pool identity on the local folder.</span></span>

<span data-ttu-id="a36f6-201">雖然沒有停止將內容部署到預設的網站在 IIS 中的項目，這個方法不會建議測試或示範案例以外的任何項目。</span><span class="sxs-lookup"><span data-stu-id="a36f6-201">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="a36f6-202">若要模擬實際執行環境，您應該建立新的 IIS 網站應用程式的需求所特定的設定。</span><span class="sxs-lookup"><span data-stu-id="a36f6-202">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="a36f6-203">**建立和設定 IIS 網站**</span><span class="sxs-lookup"><span data-stu-id="a36f6-203">**To create and configure an IIS website**</span></span>

1. <span data-ttu-id="a36f6-204">在本機檔案系統上，建立資料夾來儲存您的內容 (例如， **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-204">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="a36f6-205">在**啟動**功能表上，指向**系統管理工具**，然後按一下 **網際網路資訊服務 (IIS) 管理員**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-205">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="a36f6-206">在 IIS 管理員 中，在**連線** 窗格中，展開伺服器節點 (例如， **TESTWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-206">In IIS Manager, in the **Connections** pane, expand the server node (for example, **TESTWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. <span data-ttu-id="a36f6-207">以滑鼠右鍵按一下**網站**節點，然後再按一下**新增網站**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-207">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="a36f6-208">在**站台名稱**方塊中，輸入 IIS 網站的名稱 (例如， **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-208">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="a36f6-209">在**實體路徑**方塊中，輸入 （或瀏覽至） 您的本機資料夾的路徑 (例如， **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-209">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="a36f6-210">在**連接埠**方塊中，輸入您要裝載網站的連接埠號碼 (例如， **85**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-210">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="a36f6-211">標準連接埠號碼為 80 的 HTTP 和 HTTPS 為 443。</span><span class="sxs-lookup"><span data-stu-id="a36f6-211">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="a36f6-212">不過，如果您主控此網站連接埠 80 上的，您必須停止預設網站，才能存取您的網站。</span><span class="sxs-lookup"><span data-stu-id="a36f6-212">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="a36f6-213">保留**主機名稱**方塊保留空白，除非您想要設定網站的網域名稱系統 (DNS) 記錄，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-213">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="a36f6-214">在實際執行環境中，您可能會想將網站連接埠 80 上的主控，並設定主機標頭，以及比對 DNS 記錄。</span><span class="sxs-lookup"><span data-stu-id="a36f6-214">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="a36f6-215">如需有關如何在 IIS 7 中設定主機標頭的詳細資訊，請參閱[設定網站 (IIS 7) 的主機標頭](https://technet.microsoft.com/library/cc753195(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-215">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="a36f6-216">如需有關 Windows Server 2008 R2 中 DNS 伺服器角色的詳細資訊，請參閱[DNS 伺服器概觀](https://technet.microsoft.com/en-gb/library/cc770392.aspx)和[DNS 伺服器](https://technet.microsoft.com/windowsserver/dd448607)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-216">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="a36f6-217">在 [ **動作** ] 窗格的 [ **編輯站台**] 下方，按一下 [ **繫結**]。</span><span class="sxs-lookup"><span data-stu-id="a36f6-217">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="a36f6-218">在**站台繫結**對話方塊中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-218">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. <span data-ttu-id="a36f6-219">在**新增網站繫結** 對話方塊中，將**IP 位址**和**連接埠**以符合您現有的站台設定。</span><span class="sxs-lookup"><span data-stu-id="a36f6-219">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="a36f6-220">在**主機名稱**方塊中，輸入您的網頁伺服器的名稱 (例如， **TESTWEB1**)，然後按一下  **確定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-220">In the **Host name** box, type the name of your web server (for example, **TESTWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="a36f6-221">第一個站台繫結可讓您存取在本機使用的 IP 位址和連接埠的站台或`http://localhost:85`。</span><span class="sxs-lookup"><span data-stu-id="a36f6-221">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="a36f6-222">第二個網站繫結可讓您從使用電腦名稱 (例如，http://testweb1:85) 的網域上的其他電腦存取這個網站。</span><span class="sxs-lookup"><span data-stu-id="a36f6-222">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://testweb1:85).</span></span>
13. <span data-ttu-id="a36f6-223">在**站台繫結**對話方塊中，按一下 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-223">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="a36f6-224">在**連線**] 窗格中，按一下 [**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-224">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="a36f6-225">在**應用程式集區**] 窗格中，以滑鼠右鍵按一下您的應用程式集區的名稱，然後按一下 [**基本設定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-225">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="a36f6-226">根據預設，應用程式集區的名稱會符合網站的名稱 (例如， **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-226">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="a36f6-227">在**.NET Framework 版本**清單中，選取**.NET Framework v4.0.30319**，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-227">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="a36f6-228">範例解決方案需要.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="a36f6-228">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="a36f6-229">這不是需要 Web Deploy 一般。</span><span class="sxs-lookup"><span data-stu-id="a36f6-229">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="a36f6-230">為了讓您的網站提供內容，應用程式集區識別必須擁有讀取權限將內容儲存的本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="a36f6-230">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="a36f6-231">在 IIS 7.5 中應用程式集區具有唯一的應用程式集區身分識別執行預設 （相較於舊版的 IIS，其中應用程式集區通常會使用 Network Service 帳戶）。</span><span class="sxs-lookup"><span data-stu-id="a36f6-231">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="a36f6-232">應用程式集區識別不是真正的使用者帳戶，並不會顯示任何所列之使用者或群組 & #x 2014年; 相反地，它會動態建立啟動應用程式集區時。</span><span class="sxs-lookup"><span data-stu-id="a36f6-232">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="a36f6-233">每個應用程式集區識別加入至本機**IIS\_IUSRS**安全性群組做為隱藏的項目。</span><span class="sxs-lookup"><span data-stu-id="a36f6-233">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="a36f6-234">若要授與應用程式集區識別的檔案或資料夾上的權限，您有兩個選項：</span><span class="sxs-lookup"><span data-stu-id="a36f6-234">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="a36f6-235">指派的權限的應用程式集區識別直接使用格式**IIS AppPool\***[應用程式集區名稱] * (例如， **IIS AppPool\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-235">Assign permissions to the application pool identity directly, using the format **IIS AppPool\***[application pool name]*(for example, **IIS AppPool\DemoSite**).</span></span>
- <span data-ttu-id="a36f6-236">指派的權限**IIS\_IUSRS**群組。</span><span class="sxs-lookup"><span data-stu-id="a36f6-236">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="a36f6-237">最常見的作法是將權限指派給本機**IIS\_IUSRS**群組，因為這種方法可讓您變更應用程式集區不需要重新設定檔案系統權限。</span><span class="sxs-lookup"><span data-stu-id="a36f6-237">The most common approach is to assign permissions to the local **IIS\_IUSRS** group because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="a36f6-238">下一個程序會使用此群組為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="a36f6-238">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="a36f6-239">如需在 IIS 7.5 中的應用程式集區識別的詳細資訊，請參閱[應用程式集區識別](https://go.microsoft.com/?linkid=9805123)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-239">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="a36f6-240">**若要設定 IIS 網站的資料夾權限**</span><span class="sxs-lookup"><span data-stu-id="a36f6-240">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="a36f6-241">在 Windows 檔案總管，瀏覽至您的本機資料夾的位置。</span><span class="sxs-lookup"><span data-stu-id="a36f6-241">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="a36f6-242">以滑鼠右鍵按一下資料夾，然後按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-242">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="a36f6-243">在**安全性**索引標籤上，按一下 **編輯**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-243">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="a36f6-244">按一下**位置**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-244">Click **Locations**.</span></span> <span data-ttu-id="a36f6-245">在**位置**對話方塊中，選取 本機伺服器，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-245">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. <span data-ttu-id="a36f6-246">在**選取使用者或群組**] 對話方塊中，輸入**IIS\_IUSRS**，按一下**檢查名稱**，然後按一下 [**確定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-246">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="a36f6-247">在 **權限 * * * [資料夾名稱]*對話方塊中，請注意，新的群組已經被委派**讀取&amp;執行**，**列出資料夾內容**，和**讀取**預設權限。</span><span class="sxs-lookup"><span data-stu-id="a36f6-247">In the **Permissions for***[folder name]*dialog box, notice that the new group has been assigned the **Read &amp; execute**, **List folder contents**, and **Read** permissions by default.</span></span> <span data-ttu-id="a36f6-248">保留不變，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-248">Leave this unchanged and click **OK**.</span></span>
7. <span data-ttu-id="a36f6-249">按一下**確定**關閉*[資料夾名稱] * * * 屬性** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a36f6-249">Click **OK** to close the *[folder name]***Properties** dialog box.</span></span>

<span data-ttu-id="a36f6-250">做為最終工作之前您嘗試將任何 web 套件部署到您的伺服器，您應該確認 Web Deployment Agent Service 正在執行。</span><span class="sxs-lookup"><span data-stu-id="a36f6-250">As a final task before you attempt to deploy any web packages to your server, you should ensure that the Web Deployment Agent Service is running.</span></span> <span data-ttu-id="a36f6-251">當您部署封裝，以從遠端電腦時，Web Deployment Agent Service 負責擷取及安裝套件的內容。</span><span class="sxs-lookup"><span data-stu-id="a36f6-251">When you deploy a package from a remote computer, the Web Deployment Agent Service is responsible for extracting and installing the contents of the package.</span></span> <span data-ttu-id="a36f6-252">當您安裝 Web Deployment Tool，依預設並啟動服務，並會在 Network Service 識別下執行。</span><span class="sxs-lookup"><span data-stu-id="a36f6-252">The service is started by default when you install the Web Deployment Tool and runs under the Network Service identity.</span></span>

<span data-ttu-id="a36f6-253">您可以檢查服務是否正執行於多個不同的方式，使用各種命令列公用程式或 Windows PowerShell cmdlet。</span><span class="sxs-lookup"><span data-stu-id="a36f6-253">You can check whether a service is running in multiple different ways, using various command-line utilities or Windows PowerShell cmdlets.</span></span> <span data-ttu-id="a36f6-254">此程序描述的簡單 UI 為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="a36f6-254">This procedure describes a straightforward UI-based approach.</span></span>

<span data-ttu-id="a36f6-255">**若要檢查 Web Deployment Agent Service 正在執行**</span><span class="sxs-lookup"><span data-stu-id="a36f6-255">**To check that the Web Deployment Agent Service is running**</span></span>

1. <span data-ttu-id="a36f6-256">在 [開始]  功能表上，指向 [系統管理工具] ，然後按一下 [服務] 。</span><span class="sxs-lookup"><span data-stu-id="a36f6-256">On the **Start** menu, point to **Administrative Tools**, and then click **Services**.</span></span>
2. <span data-ttu-id="a36f6-257">找出**Web Deployment Agent Service**資料列，並確認**狀態**設**已啟動**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-257">Locate the **Web Deployment Agent Service** row, and verify that the **Status** is set to **Started**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. <span data-ttu-id="a36f6-258">如果服務尚未啟動，請按一下**啟動**。</span><span class="sxs-lookup"><span data-stu-id="a36f6-258">If the service is not already started, click **Start**.</span></span>

## <a name="configure-firewall-exceptions"></a><span data-ttu-id="a36f6-259">設定防火牆例外</span><span class="sxs-lookup"><span data-stu-id="a36f6-259">Configure Firewall Exceptions</span></span>

<span data-ttu-id="a36f6-260">根據預設，遠端代理程式服務會接聽 TCP 連接埠 80，這個 url:</span><span class="sxs-lookup"><span data-stu-id="a36f6-260">By default, the Remote Agent Service listens on TCP port 80, at this URL:</span></span>

<span data-ttu-id="a36f6-261">http://[*server name*]/MSDEPLOYAGENTSERVICE</span><span class="sxs-lookup"><span data-stu-id="a36f6-261">http://[*server name*]/MSDEPLOYAGENTSERVICE</span></span>

<span data-ttu-id="a36f6-262">在大部分情況下，您不需要任何額外的防火牆規則設定為遠端代理程式服務，因為網頁伺服器通常會接聽通訊埠 80 上的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="a36f6-262">In most cases, you won't need to configure any additional firewall rules for the Remote Agent Service because web servers typically listen for HTTP requests on port 80.</span></span> <span data-ttu-id="a36f6-263">如果您自訂您的安裝以非標準連接埠上接聽，您必須視需要設定防火牆例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a36f6-263">If you customized your installation to listen on a nonstandard port, you'll need to configure firewall exceptions as required.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a36f6-264">結論</span><span class="sxs-lookup"><span data-stu-id="a36f6-264">Conclusion</span></span>

<span data-ttu-id="a36f6-265">此時，您的網頁伺服器已準備好接受與安裝 web 封裝，從遠端電腦。</span><span class="sxs-lookup"><span data-stu-id="a36f6-265">At this point, your web server is ready to accept and install web packages from a remote computer.</span></span> <span data-ttu-id="a36f6-266">您嘗試部署至伺服器的 web 應用程式之前，您可能想要檢查這些關鍵點：</span><span class="sxs-lookup"><span data-stu-id="a36f6-266">Before you attempt to deploy a web application to the server, you may want to check these key points:</span></span>

- <span data-ttu-id="a36f6-267">您已向 IIS 註冊 ASP.NET 4.0？</span><span class="sxs-lookup"><span data-stu-id="a36f6-267">Have you registered ASP.NET 4.0 with IIS?</span></span>
- <span data-ttu-id="a36f6-268">應用程式集區身分識別是否有讀取權限為您的網站的 [來源] 資料夾？</span><span class="sxs-lookup"><span data-stu-id="a36f6-268">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="a36f6-269">執行 Web Deployment Agent Service</span><span class="sxs-lookup"><span data-stu-id="a36f6-269">Is the Web Deployment Agent Service running?</span></span>

## <a name="further-reading"></a><span data-ttu-id="a36f6-270">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="a36f6-270">Further Reading</span></span>

<span data-ttu-id="a36f6-271">如需如何設定自訂的 Microsoft Build Engine (MSBuild) 專案檔，以將 web 套件部署到遠端代理程式服務的指引，請參閱[目標環境設定部署屬性](configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="a36f6-271">For guidance on how to configure custom Microsoft Build Engine (MSBuild) project files to deploy web packages to the Remote Agent Service, see [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a36f6-272">[上一頁](scenario-configuring-a-production-environment-for-web-deployment.md)
[下一頁](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)</span><span class="sxs-lookup"><span data-stu-id="a36f6-272">[Previous](scenario-configuring-a-production-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)</span></span>
