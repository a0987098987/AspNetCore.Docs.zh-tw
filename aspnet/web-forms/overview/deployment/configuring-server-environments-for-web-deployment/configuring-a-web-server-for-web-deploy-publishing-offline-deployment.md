---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: 設定 Web 伺服器的 Web Deploy 發行 （離線部署） |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定 IIS web 伺服器以支援離線網頁發佈和部署。 當您使用 Internet Information Services (我...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: eab69e93417ddedea9214523de34697b044a871e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834709"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a><span data-ttu-id="79307-104">設定 Web 伺服器，Web deploy 發行 （離線部署）</span><span class="sxs-lookup"><span data-stu-id="79307-104">Configuring a Web Server for Web Deploy Publishing (Offline Deployment)</span></span>
====================
<span data-ttu-id="79307-105">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="79307-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="79307-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="79307-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="79307-107">本主題描述如何設定 IIS web 伺服器以支援離線網頁發佈和部署。</span><span class="sxs-lookup"><span data-stu-id="79307-107">This topic describes how to configure an IIS web server to support offline web publishing and deployment.</span></span>
> 
> <span data-ttu-id="79307-108">當您使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三種主要的方法，可用來取得您的應用程式或 web 伺服器上的站台。</span><span class="sxs-lookup"><span data-stu-id="79307-108">When you work with Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="79307-109">您可以：</span><span class="sxs-lookup"><span data-stu-id="79307-109">You can:</span></span>
> 
> - <span data-ttu-id="79307-110">使用*Web Deploy 遠端代理程式服務*。</span><span class="sxs-lookup"><span data-stu-id="79307-110">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="79307-111">此方法需要較少組態的 web 伺服器，但您必須提供本機伺服器系統管理員的認證，以將任何項目部署至伺服器。</span><span class="sxs-lookup"><span data-stu-id="79307-111">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="79307-112">使用*Web Deploy 處理常式*。</span><span class="sxs-lookup"><span data-stu-id="79307-112">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="79307-113">這種方法更多複雜，而且需要更多的初步設定 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="79307-113">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="79307-114">不過，當您使用這個方法時，您可以設定 IIS 以允許非系統管理員使用者，進行部署。</span><span class="sxs-lookup"><span data-stu-id="79307-114">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="79307-115">只有在 IIS 7 或更新版本中使用 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="79307-115">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="79307-116">使用*離線部署*。</span><span class="sxs-lookup"><span data-stu-id="79307-116">Use *offline deployment*.</span></span> <span data-ttu-id="79307-117">這種方法需要最低的網頁伺服器的設定，但伺服器系統管理員必須手動複製到伺服器上的 web 套件並匯入它透過 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="79307-117">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="79307-118">如需有關的主要功能、 優點和這些方法的缺點的詳細資訊，請參閱[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="79307-118">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


<span data-ttu-id="79307-119">是，如果您的網路基礎結構或安全性限制導致無法遠端部署。</span><span class="sxs-lookup"><span data-stu-id="79307-119">Yes, if your network infrastructure or security restrictions prevent remote deployment.</span></span> <span data-ttu-id="79307-120">這是最可能的情況下，面對網際網路的生產環境中，web 伺服器所在位置，隔離&#x2014;是實體或透過防火牆和子網路&#x2014;伺服器基礎結構的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="79307-120">This is most likely to be the case in Internet-facing production environments, where the web servers are isolated&#x2014;either physically or by firewalls and subnets&#x2014;from the rest of your server infrastructure.</span></span>

<span data-ttu-id="79307-121">很明顯地，此方法會變得較不建議，如果您的 web 應用程式會定期更新。</span><span class="sxs-lookup"><span data-stu-id="79307-121">Obviously, this approach becomes less desirable if your web applications are updated on a regular basis.</span></span> <span data-ttu-id="79307-122">如果您的基礎結構允許，您可能要考慮啟用遠端部署，使用 Web 部署處理常式或 Web 部署遠端代理程式服務。</span><span class="sxs-lookup"><span data-stu-id="79307-122">If your infrastructure allows it, you may want to consider enabling remote deployment, using either the Web Deploy Handler or the Web Deploy Remote Agent Service.</span></span>

## <a name="task-overview"></a><span data-ttu-id="79307-123">工作概觀</span><span class="sxs-lookup"><span data-stu-id="79307-123">Task Overview</span></span>

<span data-ttu-id="79307-124">若要設定 web 伺服器，以支援離線匯入和 web 封裝的部署，您將需要：</span><span class="sxs-lookup"><span data-stu-id="79307-124">To configure the web server to support offline import and deployment of web packages, you'll need to:</span></span>

- <span data-ttu-id="79307-125">安裝 IIS 7.5 和 IIS 7 建議組態。</span><span class="sxs-lookup"><span data-stu-id="79307-125">Install IIS 7.5 and the IIS 7 recommended configuration.</span></span>
- <span data-ttu-id="79307-126">安裝 Web Deploy 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="79307-126">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="79307-127">建立 IIS 網站來裝載部署的內容。</span><span class="sxs-lookup"><span data-stu-id="79307-127">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="79307-128">停用 Web 部署代理程式服務。</span><span class="sxs-lookup"><span data-stu-id="79307-128">Disable the Web Deployment Agent Service.</span></span>

<span data-ttu-id="79307-129">若要特別裝載範例方案，您還需要以：</span><span class="sxs-lookup"><span data-stu-id="79307-129">To host the sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="79307-130">安裝.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="79307-130">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="79307-131">安裝 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="79307-131">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="79307-132">本主題將說明如何執行上述各程序。</span><span class="sxs-lookup"><span data-stu-id="79307-132">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="79307-133">工作與本主題中的逐步解說假設您從執行 Windows Server 2008 R2 的全新的伺服器組建。</span><span class="sxs-lookup"><span data-stu-id="79307-133">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="79307-134">在繼續之前，請確認：</span><span class="sxs-lookup"><span data-stu-id="79307-134">Before you continue, ensure that:</span></span>

- <span data-ttu-id="79307-135">安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。</span><span class="sxs-lookup"><span data-stu-id="79307-135">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="79307-136">伺服器已加入網域。</span><span class="sxs-lookup"><span data-stu-id="79307-136">The server is domain-joined.</span></span>
- <span data-ttu-id="79307-137">伺服器具有靜態 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="79307-137">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="79307-138">如需有關如何將電腦加入網域的詳細資訊，請參閱 <<c0> [ 將電腦加入網域並登入](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="79307-138">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="79307-139">如需有關如何設定靜態 IP 位址的詳細資訊，請參閱 <<c0> [ 設定靜態 IP 位址](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="79307-139">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="79307-140">安裝的產品和元件</span><span class="sxs-lookup"><span data-stu-id="79307-140">Install Products and Components</span></span>

<span data-ttu-id="79307-141">本節將引導您完成 web 伺服器上安裝必要的產品和元件。</span><span class="sxs-lookup"><span data-stu-id="79307-141">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="79307-142">在開始之前，理想的作法就是執行 Windows Update，以確保您的伺服器已完全更新。</span><span class="sxs-lookup"><span data-stu-id="79307-142">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="79307-143">在此情況下，您需要安裝這些項目：</span><span class="sxs-lookup"><span data-stu-id="79307-143">In this case, you need to install these things:</span></span>

- <span data-ttu-id="79307-144">**IIS 7 建議組態**。</span><span class="sxs-lookup"><span data-stu-id="79307-144">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="79307-145">這可讓**網頁伺服器 (IIS)** web 伺服器上的角色，並安裝的一組 IIS 模組和元件，您需要以裝載 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="79307-145">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="79307-146">**.NET framework 4.0**。</span><span class="sxs-lookup"><span data-stu-id="79307-146">**.NET Framework 4.0**.</span></span> <span data-ttu-id="79307-147">如此才能執行這個版本的.NET Framework 所建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="79307-147">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="79307-148">**Web Deployment Tool 2.1 或更新版本**。</span><span class="sxs-lookup"><span data-stu-id="79307-148">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="79307-149">這會在您的伺服器上安裝 Web Deploy （和其基礎可執行檔，MSDeploy.exe）。</span><span class="sxs-lookup"><span data-stu-id="79307-149">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="79307-150">Web Deploy 與 IIS 整合，並讓您匯入和匯出網頁套件。</span><span class="sxs-lookup"><span data-stu-id="79307-150">Web Deploy integrates with IIS and lets you import and export web packages.</span></span>
- <span data-ttu-id="79307-151">**ASP.NET MVC 3**。</span><span class="sxs-lookup"><span data-stu-id="79307-151">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="79307-152">這會安裝您要執行 MVC 3 應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="79307-152">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="79307-153">本逐步解說說明如何使用 Web Platform Installer 來安裝和設定各種元件。</span><span class="sxs-lookup"><span data-stu-id="79307-153">This walkthrough describes the use of the Web Platform Installer to install and configure various components.</span></span> <span data-ttu-id="79307-154">雖然您不需要使用 Web Platform Installer，它可以簡化安裝程序自動偵測相依性，並確保您一律取得最新的產品版本。</span><span class="sxs-lookup"><span data-stu-id="79307-154">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="79307-155">如需詳細資訊，請參閱 < [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="79307-155">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="79307-156">**若要安裝必要的產品和元件**</span><span class="sxs-lookup"><span data-stu-id="79307-156">**To install the required products and components**</span></span>

1. <span data-ttu-id="79307-157">下載並安裝[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="79307-157">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="79307-158">安裝完成時，Web Platform Installer 將會自動啟動。</span><span class="sxs-lookup"><span data-stu-id="79307-158">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79307-159">您現在可以隨時從啟動 Web Platform Installer**啟動**功能表。</span><span class="sxs-lookup"><span data-stu-id="79307-159">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="79307-160">若要這樣做，請在**開始**功能表上，按一下**所有程式**，然後按一下**Microsoft Web Platform Installer**。</span><span class="sxs-lookup"><span data-stu-id="79307-160">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="79307-161">在頂端**Web Platform Installer 3.0**  視窗中，按一下**產品**。</span><span class="sxs-lookup"><span data-stu-id="79307-161">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="79307-162">在左邊視窗中，在導覽窗格中，按一下 **架構**。</span><span class="sxs-lookup"><span data-stu-id="79307-162">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="79307-163">在  **Microsoft.NET Framework 4**資料列，如果尚未安裝.NET Framework，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79307-163">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="79307-164">您可能已經安裝.NET Framework 4.0，透過 Windows Update。</span><span class="sxs-lookup"><span data-stu-id="79307-164">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="79307-165">如果已安裝的產品或元件，Web Platform Installer 會指出這藉由取代**新增**按鈕，但文字**已安裝**。</span><span class="sxs-lookup"><span data-stu-id="79307-165">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. <span data-ttu-id="79307-166">在  **ASP.NET MVC 3 (Visual Studio 2010)** 資料列中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79307-166">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="79307-167">在 [導覽] 窗格中，按一下**Server**。</span><span class="sxs-lookup"><span data-stu-id="79307-167">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="79307-168">在  **IIS 7 建議組態**資料列中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79307-168">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="79307-169">在  **Web 部署工具 2.1**資料列中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79307-169">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="79307-170">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="79307-170">Click **Install**.</span></span> <span data-ttu-id="79307-171">Web Platform Installer 將會顯示您的產品清單&#x2014;以及任何相關聯相依性&#x2014;安裝就會提示您接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="79307-171">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. <span data-ttu-id="79307-172">檢閱授權條款，然後如果您同意這些條款，按一下**我接受**。</span><span class="sxs-lookup"><span data-stu-id="79307-172">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="79307-173">安裝完成時，按一下**完成**，然後關閉**Web Platform Installer 3.0**視窗。</span><span class="sxs-lookup"><span data-stu-id="79307-173">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="79307-174">如果您安裝 IIS 之前，您就會安裝.NET Framework 4.0，您必須執行[ASP.NET IIS 註冊工具](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) 向 IIS 註冊 ASP.NET 的最新版本。</span><span class="sxs-lookup"><span data-stu-id="79307-174">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="79307-175">如果沒有這麼做，您會發現，IIS 會提供靜態內容 （例如 HTML 檔案） 沒有任何問題，但它會傳回**HTTP 錯誤 404.0 – 找不到**當您嘗試瀏覽至 ASP.NET 內容。</span><span class="sxs-lookup"><span data-stu-id="79307-175">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="79307-176">您可以使用下一個程序，以確保已註冊 ASP.NET 4.0。</span><span class="sxs-lookup"><span data-stu-id="79307-176">You can use the next procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="79307-177">**若要向 IIS 註冊 ASP.NET 4.0**</span><span class="sxs-lookup"><span data-stu-id="79307-177">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="79307-178">按一下 **開始**，然後輸入**命令提示字元**。</span><span class="sxs-lookup"><span data-stu-id="79307-178">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="79307-179">在搜尋結果中，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="79307-179">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="79307-180">在 [命令提示字元] 視窗中，瀏覽至 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**目錄。</span><span class="sxs-lookup"><span data-stu-id="79307-180">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="79307-181">輸入下列命令，並再按 Enter 鍵：</span><span class="sxs-lookup"><span data-stu-id="79307-181">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. <span data-ttu-id="79307-182">如果您打算裝載在任何時間點的 64 位元 web 應用程式時，則您也應該向 IIS 註冊 ASP.NET 的 64 位元版本。</span><span class="sxs-lookup"><span data-stu-id="79307-182">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="79307-183">若要這樣做，請在 [命令提示字元] 視窗中，巡覽至 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**目錄。</span><span class="sxs-lookup"><span data-stu-id="79307-183">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="79307-184">輸入下列命令，並再按 Enter 鍵：</span><span class="sxs-lookup"><span data-stu-id="79307-184">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

<span data-ttu-id="79307-185">好的做法是，使用 Windows Update 一次此時下載並安裝任何可用的更新，新的產品及您已安裝的元件。</span><span class="sxs-lookup"><span data-stu-id="79307-185">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-iis-website"></a><span data-ttu-id="79307-186">設定 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="79307-186">Configure the IIS Website</span></span>

<span data-ttu-id="79307-187">您可以將 web 內容部署到您的伺服器之前，您需要建立及設定 IIS 網站來裝載內容。</span><span class="sxs-lookup"><span data-stu-id="79307-187">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="79307-188">Web Deploy 只可以將 web 套件部署至現有的 IIS 網站;它不能為您建立網站。</span><span class="sxs-lookup"><span data-stu-id="79307-188">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="79307-189">概括而言，您將需要完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="79307-189">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="79307-190">若要將內容裝載在檔案系統上建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="79307-190">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="79307-191">建立 IIS 網站提供內容，並將它關聯的本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="79307-191">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="79307-192">授與讀取權限的本機資料夾上的應用程式集區識別。</span><span class="sxs-lookup"><span data-stu-id="79307-192">Grant read permissions to the application pool identity on the local folder.</span></span>

<span data-ttu-id="79307-193">雖然沒有任何阻礙您將內容部署到預設的網站在 IIS 中，這種方法不會建議針對測試或示範的案例以外的任何項目。</span><span class="sxs-lookup"><span data-stu-id="79307-193">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="79307-194">若要模擬生產環境中，您應該建立新的 IIS 網站設定專屬於您的應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="79307-194">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="79307-195">**若要建立及設定 IIS 網站**</span><span class="sxs-lookup"><span data-stu-id="79307-195">**To create and configure an IIS website**</span></span>

1. <span data-ttu-id="79307-196">在本機檔案系統上，建立資料夾來儲存您的內容 (例如**C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="79307-196">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="79307-197">在上**開始**功能表上，指向**系統管理工具**，然後按一下**Internet Information Services (IIS) 管理員**。</span><span class="sxs-lookup"><span data-stu-id="79307-197">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="79307-198">在 [IIS 管理員] 中，在**連線**窗格中，展開伺服器節點 (例如**PROWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="79307-198">In IIS Manager, in the **Connections** pane, expand the server node (for example, **PROWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. <span data-ttu-id="79307-199">以滑鼠右鍵按一下**站台**節點，然後再按一下**新增網站**。</span><span class="sxs-lookup"><span data-stu-id="79307-199">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="79307-200">在 **站台名稱**方塊中，輸入 IIS 網站的名稱 (例如**DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="79307-200">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="79307-201">在**實體路徑**方塊中，輸入 （或瀏覽至） 您的本機資料夾的路徑 (例如**C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="79307-201">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="79307-202">在 **連接埠**方塊中，輸入您要裝載網站的連接埠號碼 (例如**85**)。</span><span class="sxs-lookup"><span data-stu-id="79307-202">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="79307-203">標準連接埠號碼是 80 用於 HTTP 和 HTTPS 為 443。</span><span class="sxs-lookup"><span data-stu-id="79307-203">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="79307-204">不過，如果您主控此連接埠 80 上的網站，您必須停止預設網站，才能存取您的網站。</span><span class="sxs-lookup"><span data-stu-id="79307-204">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="79307-205">離開**主機名稱**空白，除非您想要設定之網站中，網域名稱系統 (DNS) 記錄，然後按一下方塊**確定**。</span><span class="sxs-lookup"><span data-stu-id="79307-205">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="79307-206">在生產環境中，您可能會想要裝載您的網站連接埠 80 上，並設定主機標頭，以及比對 DNS 記錄。</span><span class="sxs-lookup"><span data-stu-id="79307-206">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="79307-207">如需有關如何在 IIS 7 中設定主機標頭的詳細資訊，請參閱 <<c0> [ 網站 (IIS 7) 中設定主機標頭](https://technet.microsoft.com/library/cc753195(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="79307-207">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="79307-208">如需有關 Windows Server 2008 R2 中 DNS 伺服器角色的詳細資訊，請參閱 < [DNS 伺服器概觀](https://technet.microsoft.com/en-gb/library/cc770392.aspx)並[DNS 伺服器](https://technet.microsoft.com/windowsserver/dd448607)。</span><span class="sxs-lookup"><span data-stu-id="79307-208">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="79307-209">在 [ **動作** ] 窗格的 [ **編輯站台**] 下方，按一下 [ **繫結**]。</span><span class="sxs-lookup"><span data-stu-id="79307-209">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="79307-210">在 [**站台繫結**] 對話方塊中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79307-210">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. <span data-ttu-id="79307-211">在**新增網站繫結** 對話方塊中，將**IP 位址**並**連接埠**以符合您現有的站台設定。</span><span class="sxs-lookup"><span data-stu-id="79307-211">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="79307-212">中**主機名稱**方塊中，輸入您的 web 伺服器的名稱 (例如**PROWEB1**)，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="79307-212">In the **Host name** box, type the name of your web server (for example, **PROWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="79307-213">第一個站台繫結可讓您存取使用的 IP 位址和連接埠，在本機站台或 `http://localhost:85` 。</span><span class="sxs-lookup"><span data-stu-id="79307-213">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="79307-214">第二個網站繫結可讓您從使用電腦名稱在網域上其他電腦存取站台 (例如 http://proweb1:85) 。</span><span class="sxs-lookup"><span data-stu-id="79307-214">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://proweb1:85).</span></span>
13. <span data-ttu-id="79307-215">在 [**站台繫結**] 對話方塊中，按一下**關閉**。</span><span class="sxs-lookup"><span data-stu-id="79307-215">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="79307-216">在 **連線**窗格中，按一下**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="79307-216">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="79307-217">在 **應用程式集區**窗格中，以滑鼠右鍵按一下您的應用程式集區的名稱，然後按一下**基本設定**。</span><span class="sxs-lookup"><span data-stu-id="79307-217">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="79307-218">根據預設，應用程式集區的名稱會符合您網站的名稱 (例如**DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="79307-218">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="79307-219">在  **.NET Framework 版本**清單中，選取 **.NET Framework v4.0.30319**，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="79307-219">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="79307-220">範例解決方案需要.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="79307-220">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="79307-221">這不是 Web Deploy 的需求在一般情況下。</span><span class="sxs-lookup"><span data-stu-id="79307-221">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="79307-222">為了讓您的網站提供內容，應用程式集區識別必須擁有讀取權限儲存內容的本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="79307-222">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="79307-223">在 IIS 7.5 中應用程式集區執行具有唯一的應用程式集區身分識別 （相較於舊版的 IIS，其中應用程式集區會通常執行使用 Network Service 帳戶） 的預設值。</span><span class="sxs-lookup"><span data-stu-id="79307-223">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="79307-224">應用程式集區識別不是真正的使用者帳戶，以及未顯示任何使用者或群組的清單上&#x2014;相反地，它會動態建立啟動應用程式集區時。</span><span class="sxs-lookup"><span data-stu-id="79307-224">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="79307-225">每個應用程式集區身分識別加入至本機**IIS\_IUSRS**安全性群組做為隱藏的項目。</span><span class="sxs-lookup"><span data-stu-id="79307-225">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="79307-226">若要授與權限到檔案或資料夾上的應用程式集區身分識別，您會有兩個選項：</span><span class="sxs-lookup"><span data-stu-id="79307-226">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="79307-227">指派的權限的應用程式集區識別，直接使用的格式<strong>IIS AppPool\</ s t ><em>[應用程式集區名稱]</em>(比方說， <strong>IIS AppPool\DemoSite</strong>).</span><span class="sxs-lookup"><span data-stu-id="79307-227">Assign permissions to the application pool identity directly, using the format <strong>IIS AppPool\</strong><em>[application pool name]</em>(for example, <strong>IIS AppPool\DemoSite</strong>).</span></span>
- <span data-ttu-id="79307-228">指派權限**IIS\_IUSRS**群組。</span><span class="sxs-lookup"><span data-stu-id="79307-228">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="79307-229">最常見的方法是將權限指派給本機**IIS\_IUSRS**群組，因為這種方法可讓您變更應用程式集區，不需要重新設定檔案系統權限。</span><span class="sxs-lookup"><span data-stu-id="79307-229">The most common approach is to assign permissions to the local **IIS\_IUSRS** group, because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="79307-230">下一個程序會使用此群組為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="79307-230">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="79307-231">如需有關在 IIS 7.5 中的應用程式集區身分識別的詳細資訊，請參閱[應用程式集區識別](https://go.microsoft.com/?linkid=9805123)。</span><span class="sxs-lookup"><span data-stu-id="79307-231">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="79307-232">**若要設定 IIS 網站的資料夾權限**</span><span class="sxs-lookup"><span data-stu-id="79307-232">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="79307-233">在 Windows 檔案總管中，瀏覽至您本機資料夾的位置。</span><span class="sxs-lookup"><span data-stu-id="79307-233">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="79307-234">以滑鼠右鍵按一下資料夾，然後按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="79307-234">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="79307-235">在上**安全性**索引標籤上，按一下**編輯**，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="79307-235">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="79307-236">按一下 **位置**。</span><span class="sxs-lookup"><span data-stu-id="79307-236">Click **Locations**.</span></span> <span data-ttu-id="79307-237">在 **位置** 對話方塊中，選取 本機伺服器，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="79307-237">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. <span data-ttu-id="79307-238">中**選取使用者或群組** 對話方塊中，輸入**IIS\_IUSRS**，按一下**檢查名稱**，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="79307-238">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="79307-239">在 <strong>權限</strong><em>[資料夾名稱]</em>對話方塊中，請注意，已被指派新的群組<strong>讀取&amp;執行</strong>，<strong>列出資料夾內容</strong>，並<strong>讀取</strong>預設的權限。</span><span class="sxs-lookup"><span data-stu-id="79307-239">In the <strong>Permissions for</strong><em>[folder name]</em> dialog box, notice that the new group has been assigned the <strong>Read &amp; execute</strong>, <strong>List folder contents</strong>, and <strong>Read</strong> permissions by default.</span></span> <span data-ttu-id="79307-240">保留不變，然後按一下 <strong>確定</strong>。</span><span class="sxs-lookup"><span data-stu-id="79307-240">Leave this unchanged and click <strong>OK</strong>.</span></span>
7. <span data-ttu-id="79307-241">按一下 [ <strong>[確定]</strong>以關閉<em>[資料夾名稱]</em><strong>屬性</strong>] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="79307-241">Click <strong>OK</strong> to close the <em>[folder name]</em><strong>Properties</strong> dialog box.</span></span>

## <a name="disable-the-remote-agent-service"></a><span data-ttu-id="79307-242">停用遠端代理程式服務</span><span class="sxs-lookup"><span data-stu-id="79307-242">Disable the Remote Agent Service</span></span>

<span data-ttu-id="79307-243">當您安裝 Web Deploy 時，Web Deployment Agent Service 安裝中，並自動啟動。</span><span class="sxs-lookup"><span data-stu-id="79307-243">When you install Web Deploy, the Web Deployment Agent Service is installed and started automatically.</span></span> <span data-ttu-id="79307-244">這項服務可讓您部署及發行 web 封裝，從遠端位置。</span><span class="sxs-lookup"><span data-stu-id="79307-244">This service allows you to deploy and publish web packages from a remote location.</span></span> <span data-ttu-id="79307-245">您不會使用遠端部署功能在此案例中，因此您應該停止並停用服務。</span><span class="sxs-lookup"><span data-stu-id="79307-245">You won't be using the remote deployment capability in this scenario, so you should stop and disable the service.</span></span>

> [!NOTE]
> <span data-ttu-id="79307-246">您不需要停止遠端代理程式服務，才能匯入，並以手動方式部署網頁套件。</span><span class="sxs-lookup"><span data-stu-id="79307-246">You don't need to stop the remote agent service in order to import and deploy a web package manually.</span></span> <span data-ttu-id="79307-247">不過，它是個不錯的做法停止並停用服務，如果您不打算使用它。</span><span class="sxs-lookup"><span data-stu-id="79307-247">However, it's a good practice to stop and disable the service if you don't plan to use it.</span></span>


<span data-ttu-id="79307-248">您可以停止並停用服務，以多種方式使用各種命令列公用程式或 Windows PowerShell cmdlet。</span><span class="sxs-lookup"><span data-stu-id="79307-248">You can stop and disable a service in multiple ways, using various command-line utilities or Windows PowerShell cmdlets.</span></span> <span data-ttu-id="79307-249">此程序描述直接以 UI 為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="79307-249">This procedure describes a straightforward UI-based approach.</span></span>

<span data-ttu-id="79307-250">**若要停止並停用遠端代理程式服務**</span><span class="sxs-lookup"><span data-stu-id="79307-250">**To stop and disable the remote agent service**</span></span>

1. <span data-ttu-id="79307-251">在 [開始]  功能表上，指向 [系統管理工具] ，然後按一下 [服務] 。</span><span class="sxs-lookup"><span data-stu-id="79307-251">On the **Start** menu, point to **Administrative Tools**, and then click **Services**.</span></span>
2. <span data-ttu-id="79307-252">在 [服務] 主控台中，找出**Web Deployment Agent Service**資料列。</span><span class="sxs-lookup"><span data-stu-id="79307-252">In the Services console, locate the **Web Deployment Agent Service** row.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. <span data-ttu-id="79307-253">以滑鼠右鍵按一下**Web Deployment Agent Service**，然後按一下**屬性**。</span><span class="sxs-lookup"><span data-stu-id="79307-253">Right-click **Web Deployment Agent Service**, and then click **Properties**.</span></span>
4. <span data-ttu-id="79307-254">在 [ **Web 部署代理程式服務屬性**] 對話方塊中，按一下**停止**。</span><span class="sxs-lookup"><span data-stu-id="79307-254">In the **Web Deployment Agent Service Properties** dialog box, click **Stop**.</span></span>
5. <span data-ttu-id="79307-255">在 **啟動類型**清單中，選取**停用**，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="79307-255">In the **Startup type** list, select **Disabled**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a><span data-ttu-id="79307-256">結論</span><span class="sxs-lookup"><span data-stu-id="79307-256">Conclusion</span></span>

<span data-ttu-id="79307-257">此時，您的網頁伺服器可供離線 web 套件部署。</span><span class="sxs-lookup"><span data-stu-id="79307-257">At this point, your web server is ready for offline web package deployment.</span></span> <span data-ttu-id="79307-258">您嘗試匯入至 IIS 網站的 web 封裝之前，您可能想要檢查的這些關鍵點：</span><span class="sxs-lookup"><span data-stu-id="79307-258">Before you attempt to import web packages to an IIS website, you may want to check these key points:</span></span>

- <span data-ttu-id="79307-259">您已向 IIS 註冊 ASP.NET 4.0？</span><span class="sxs-lookup"><span data-stu-id="79307-259">Have you registered ASP.NET 4.0 with IIS?</span></span>
- <span data-ttu-id="79307-260">應用程式集區識別是否有讀取權限為您的網站的 [來源] 資料夾？</span><span class="sxs-lookup"><span data-stu-id="79307-260">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="79307-261">您已停止 Web Deployment Agent Service？</span><span class="sxs-lookup"><span data-stu-id="79307-261">Have you stopped the Web Deployment Agent Service?</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="79307-262">[上一頁](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [下一頁](configuring-a-database-server-for-web-deploy-publishing.md)</span><span class="sxs-lookup"><span data-stu-id="79307-262">[Previous](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[Next](configuring-a-database-server-for-web-deploy-publishing.md)</span></span>
