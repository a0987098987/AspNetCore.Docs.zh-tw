---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: "建立伺服器陣列與 Web 伺服陣列架構 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何使用 Web 伺服陣列架構 (WFF) 2.0 來建立及設定 web 伺服器陣列中伺服器的集合。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 2458abc863a83364f90fc9d6edaace897c23b4c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a><span data-ttu-id="e6234-103">建立伺服器陣列與 Web 伺服陣列架構</span><span class="sxs-lookup"><span data-stu-id="e6234-103">Creating a Server Farm with the Web Farm Framework</span></span>
====================
<span data-ttu-id="e6234-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e6234-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e6234-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="e6234-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e6234-106">本主題描述如何使用 Web 伺服陣列架構 (WFF) 2.0 來建立及設定 web 伺服器陣列中伺服器的集合。</span><span class="sxs-lookup"><span data-stu-id="e6234-106">This topic describes how to use the Web Farm Framework (WFF) 2.0 to create and configure a web server farm from a collection of servers.</span></span>


<span data-ttu-id="e6234-107">WFF 可讓您跨多個負載平衡 web 伺服器同步處理的 web platform 產品和元件、 web 應用程式、 網站和組態設定。</span><span class="sxs-lookup"><span data-stu-id="e6234-107">WFF lets you synchronize web platform products and components, web applications, websites, and configuration settings across multiple load-balanced web servers.</span></span> <span data-ttu-id="e6234-108">在案例中您需要一個以上的 web 伺服器，例如預備與生產環境中，這可大幅簡化您的部署和設定程序。</span><span class="sxs-lookup"><span data-stu-id="e6234-108">In scenarios where you need more than one web server, like staging and production environments, this can vastly simplify your deployment and configuration process.</span></span> <span data-ttu-id="e6234-109">您可以部署 web 應用程式的單一伺服器 & #x 2014;*主要伺服器*（& c) 2014; #x 和 WFF 會自動複寫所有其他 web 伺服器上的伺服器陣列中的該 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6234-109">You can deploy a web application to a single server&#x2014;the *primary server*&#x2014;and WFF will automatically replicate that web application on all the other web servers in the server farm.</span></span>

## <a name="understanding-the-web-farm-framework"></a><span data-ttu-id="e6234-110">了解 Web 伺服陣列架構</span><span class="sxs-lookup"><span data-stu-id="e6234-110">Understanding the Web Farm Framework</span></span>

<span data-ttu-id="e6234-111">您可以使用 WFF 2.0 來佈建、 管理和內容部署至 web 伺服器群組。</span><span class="sxs-lookup"><span data-stu-id="e6234-111">You can use WFF 2.0 to provision, manage, and deploy content to a group of web servers.</span></span> <span data-ttu-id="e6234-112">WFF 部署是由三個關鍵伺服器角色所組成：</span><span class="sxs-lookup"><span data-stu-id="e6234-112">A WFF deployment consists of three key server roles:</span></span>

- <span data-ttu-id="e6234-113">*控制站伺服器*。</span><span class="sxs-lookup"><span data-stu-id="e6234-113">The *controller server*.</span></span> <span data-ttu-id="e6234-114">您可以使用此伺服器來建立及設定 WFF 伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-114">You use this server to create and configure WFF server farms.</span></span> <span data-ttu-id="e6234-115">控制站伺服器管理 web 平台的元件、 組態設定和應用程式在伺服器陣列中的 web 伺服器之間的同步處理。</span><span class="sxs-lookup"><span data-stu-id="e6234-115">The controller server manages the synchronization of web platform components, configuration settings, and applications between the web servers in a server farm.</span></span> <span data-ttu-id="e6234-116">WFF 2.0 伺服器上安裝控制站，並控制站伺服器接著將每一個伺服器陣列中伺服器上安裝 WFF 代理程式。</span><span class="sxs-lookup"><span data-stu-id="e6234-116">You install WFF 2.0 on the controller server, and the controller server will in turn install the WFF agent on each of the servers in a server farm.</span></span> <span data-ttu-id="e6234-117">控制站伺服器不在概念上屬於任何 WFF server 伺服器陣列中，與單一控制器伺服器可以管理多個伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-117">The controller server does not conceptually belong to any WFF server farm, and a single controller server can manage multiple server farms.</span></span> <span data-ttu-id="e6234-118">在此案例中，您可以使用單一 WFF 控制站伺服器來建立和管理臨時伺服器陣列與實際執行伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-118">In this scenario, you use a single WFF controller server to create and manage the staging server farm and the production server farm.</span></span>
- <span data-ttu-id="e6234-119">*主要伺服器*。</span><span class="sxs-lookup"><span data-stu-id="e6234-119">The *primary server*.</span></span> <span data-ttu-id="e6234-120">每個 WFF 伺服器陣列包含一部主要伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-120">Each WFF server farm includes a single primary server.</span></span> <span data-ttu-id="e6234-121">當您安裝 web 平台的元件或應用程式部署到主要伺服器時，WFF 會同步處理您的變更至伺服器陣列中的所有其他伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-121">When you install web platform components or deploy applications to the primary server, the WFF synchronizes your changes to all the other servers in the server farm.</span></span>
- <span data-ttu-id="e6234-122">*次要伺服器*。</span><span class="sxs-lookup"><span data-stu-id="e6234-122">The *secondary server*.</span></span> <span data-ttu-id="e6234-123">每個 WFF 伺服器陣列包含一或多個次要伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-123">Each WFF server farm includes one or more secondary servers.</span></span> <span data-ttu-id="e6234-124">主要伺服器進行任何變更會複寫到伺服器陣列中每個次要伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-124">Any changes you make to the primary server are replicated to every secondary server within the server farm.</span></span>

<span data-ttu-id="e6234-125">這會顯示這些伺服器角色與 Fabrikam，Inc.預備與生產環境的相關聯：</span><span class="sxs-lookup"><span data-stu-id="e6234-125">This shows how these server roles relate to the Fabrikam, Inc. staging and production environments:</span></span>

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

<span data-ttu-id="e6234-126">在此案例中，預備環境和生產環境會同時設定為 WFF 伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-126">In this scenario, the staging environment and the production environment are both configured as WFF server farms.</span></span> <span data-ttu-id="e6234-127">單一 WFF 控制站伺服器管理兩個伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-127">A single WFF controller server manages both farms.</span></span> <span data-ttu-id="e6234-128">內每個伺服器陣列中，主要伺服器的任何變更會複寫到每個次要伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-128">Within each server farm, any changes to the primary server are replicated to every secondary server.</span></span>

<span data-ttu-id="e6234-129">在開始設定您的預備與生產環境之前，我們建議您先閱讀下列文章來熟悉 WFF 2.0 的重要概念：</span><span class="sxs-lookup"><span data-stu-id="e6234-129">Before you start to configure your staging and production environments, we recommend that you read these articles to familiarize yourself with the key concepts of WFF 2.0:</span></span>

- [<span data-ttu-id="e6234-130">Web Farm Framework 2.0 for IIS 7 的概觀</span><span class="sxs-lookup"><span data-stu-id="e6234-130">Overview of the Web Farm Framework 2.0 for IIS 7</span></span>](https://go.microsoft.com/?linkid=9805126)
- [<span data-ttu-id="e6234-131">具有 Web Farm Framework 2.0 的伺服器陣列設定適用於 IIS 7</span><span class="sxs-lookup"><span data-stu-id="e6234-131">Setting up a Server Farm with the Web Farm Framework 2.0 for IIS 7</span></span>](https://go.microsoft.com/?linkid=9805127)
- [<span data-ttu-id="e6234-132">系統和 Web Farm Framework 2.0 for IIS 7 的平台需求</span><span class="sxs-lookup"><span data-stu-id="e6234-132">System and Platform Requirements for the Web Farm Framework 2.0 for IIS 7</span></span>](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a><span data-ttu-id="e6234-133">工作概觀</span><span class="sxs-lookup"><span data-stu-id="e6234-133">Task Overview</span></span>

<span data-ttu-id="e6234-134">若要完成的工作和本主題中的逐步解說，您將需要至少三個伺服器 & #x 2014; 一個 WFF 控制站、 伺服器陣列的一個主要 web 伺服器和伺服器陣列的一個或多個次要的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-134">To complete the tasks and walkthroughs in this topic, you'll need at least three servers&#x2014;one WFF controller, one primary web server for the server farm, and one or more secondary web servers for the server farm.</span></span> <span data-ttu-id="e6234-135">您可以加入更多的次要伺服器 WFF 伺服器陣列在任何時間。</span><span class="sxs-lookup"><span data-stu-id="e6234-135">You can add more secondary servers to a WFF server farm at any time.</span></span> <span data-ttu-id="e6234-136">在高層級，來建立及設定 WFF 伺服器陣列，您將需要您預備或生產環境：</span><span class="sxs-lookup"><span data-stu-id="e6234-136">At a high level, to create and configure a WFF server farm for your staging or production environment you'll need to:</span></span>

- <span data-ttu-id="e6234-137">藉由安裝網際網路資訊服務 (IIS) 7.5 和 WFF 2.0 中建立控制站伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-137">Create a controller server by installing Internet Information Services (IIS) 7.5 and WFF 2.0.</span></span>
- <span data-ttu-id="e6234-138">建立一般的系統管理員帳戶和設定防火牆例外，以準備主要和次要伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-138">Prepare primary and secondary servers by creating a common administrator account and configuring firewall exceptions.</span></span>
- <span data-ttu-id="e6234-139">設定伺服器陣列的控制站伺服器上使用 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="e6234-139">Configure the server farm by using IIS Manager on the controller server.</span></span>
- <span data-ttu-id="e6234-140">設定負載平衡使用 IIS 應用程式要求路由 (ARR) 或替代的負載平衡技術。</span><span class="sxs-lookup"><span data-stu-id="e6234-140">Configure load balancing using IIS Application Request Routing (ARR) or an alternative load-balancing technology.</span></span>

<span data-ttu-id="e6234-141">工作與本主題中的逐步解說假設您要開始使用執行 Windows Server 2008 R2 的全新的伺服器組建。</span><span class="sxs-lookup"><span data-stu-id="e6234-141">The tasks and walkthroughs in this topic assume that you're starting with clean server builds running Windows Server 2008 R2.</span></span> <span data-ttu-id="e6234-142">您一開始，每個伺服器，才能確保：</span><span class="sxs-lookup"><span data-stu-id="e6234-142">Before you begin, for each server, ensure that:</span></span>

- <span data-ttu-id="e6234-143">已安裝 Windows Server 2008 R2 Service Pack 1 和所有可用的更新。</span><span class="sxs-lookup"><span data-stu-id="e6234-143">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="e6234-144">伺服器已加入網域。</span><span class="sxs-lookup"><span data-stu-id="e6234-144">The server is domain-joined.</span></span>
- <span data-ttu-id="e6234-145">伺服器具有靜態 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="e6234-145">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="e6234-146">如需有關如何將電腦加入網域的詳細資訊，請參閱[將電腦加入網域並登入](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="e6234-146">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="e6234-147">如需有關如何設定靜態 IP 位址的詳細資訊，請參閱[設定靜態 IP 位址](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="e6234-147">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="create-the-wff-controller-server"></a><span data-ttu-id="e6234-148">建立 WFF 控制站伺服器</span><span class="sxs-lookup"><span data-stu-id="e6234-148">Create the WFF Controller Server</span></span>

<span data-ttu-id="e6234-149">若要建立 WFF 控制器伺服器，您將需要安裝 IIS 7 或更新版本和 WFF 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6234-149">To create a WFF controller server, you'll need to install both IIS 7 or later and WFF 2.0 or later.</span></span> <span data-ttu-id="e6234-150">實際上，WFF 使用 IIS Web Deployment Tool (Web Deploy) 2.x 來同步處理您的伺服陣列中的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-150">Under the covers, WFF uses the IIS Web Deployment Tool (Web Deploy) 2.x to synchronize the servers in your farm.</span></span> <span data-ttu-id="e6234-151">如果您使用 Web Platform Installer 安裝 WFF，安裝程式將會自動下載並安裝您的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="e6234-151">If you use the Web Platform Installer to install WFF, the installer will automatically download and install Web Deploy for you.</span></span>

<span data-ttu-id="e6234-152">**若要建立 WFF 控制站伺服器**</span><span class="sxs-lookup"><span data-stu-id="e6234-152">**To create the WFF controller server**</span></span>

1. <span data-ttu-id="e6234-153">下載並安裝[Web Platform Installer](https://go.microsoft.com/?linkid=9739157)。</span><span class="sxs-lookup"><span data-stu-id="e6234-153">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9739157).</span></span>
2. <span data-ttu-id="e6234-154">在頂端**Web Platform Installer 3.0**視窗中，按一下 **產品**。</span><span class="sxs-lookup"><span data-stu-id="e6234-154">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
3. <span data-ttu-id="e6234-155">在左邊視窗中，瀏覽窗格中按一下 **伺服器**。</span><span class="sxs-lookup"><span data-stu-id="e6234-155">On the left side of the window, in the navigation pane, click **Server**.</span></span>
4. <span data-ttu-id="e6234-156">在**IIS 7 建議組態**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e6234-156">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
5. <span data-ttu-id="e6234-157">在**Web 伺服陣列架構 2。***x*資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e6234-157">In the **Web Farm Framework 2.***x* row, click **Add**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. <span data-ttu-id="e6234-158">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="e6234-158">Click **Install**.</span></span> <span data-ttu-id="e6234-159">請注意，Web Platform Installer 已加入 Web Deployment Tool，各種其他相依性，以及安裝清單中。</span><span class="sxs-lookup"><span data-stu-id="e6234-159">Notice that the Web Platform Installer has added the Web Deployment Tool, along with various other dependencies, to the installation list.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. <span data-ttu-id="e6234-160">檢閱授權條款，然後如果您同意條款，按一下**我接受**。</span><span class="sxs-lookup"><span data-stu-id="e6234-160">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
8. <span data-ttu-id="e6234-161">當安裝完成時，按一下 **完成**，然後關閉**Web Platform Installer 3.0**視窗。</span><span class="sxs-lookup"><span data-stu-id="e6234-161">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

## <a name="configure-the-primary-and-secondary-servers"></a><span data-ttu-id="e6234-162">設定主要和次要伺服器</span><span class="sxs-lookup"><span data-stu-id="e6234-162">Configure the Primary and Secondary Servers</span></span>

<span data-ttu-id="e6234-163">建立 WFF 伺服器陣列之前，應該先完成一些準備工作，會組成伺服器陣列的 web 伺服器上：</span><span class="sxs-lookup"><span data-stu-id="e6234-163">Before you create a WFF server farm, you should complete some preparation tasks on the web servers that will make up the farm:</span></span>

- <span data-ttu-id="e6234-164">新增防火牆例外以允許**核心網路功能**，**遠端系統管理**，和**檔案及印表機共用**與 WFF 控制站伺服器通訊的功能.</span><span class="sxs-lookup"><span data-stu-id="e6234-164">Add firewall exceptions to allow the **Core Networking**, **Remote Administration**, and **File and Printer Sharing** features to communicate with the WFF controller server.</span></span>
- <span data-ttu-id="e6234-165">建立網域帳戶 (例如， **FABRIKAM\stagingfarm**) 在 Active Directory 中將它加入至每一部伺服器上的本機 administrators 群組。</span><span class="sxs-lookup"><span data-stu-id="e6234-165">Create a domain account (for example, **FABRIKAM\stagingfarm**) in Active Directory and add it to the local administrators group on each server.</span></span> <span data-ttu-id="e6234-166">當您建立伺服器陣列時，您將使用此帳戶為伺服器陣列系統管理員帳戶。</span><span class="sxs-lookup"><span data-stu-id="e6234-166">You'll use this account as the server farm administrator account when you create the server farm.</span></span>

<span data-ttu-id="e6234-167">如需有關如何設定 Windows 防火牆中的這些防火牆例外狀況的詳細資訊，請參閱[系統和 Web Farm Framework 2.0，適用於 IIS 7 的平台需求](https://go.microsoft.com/?linkid=9805128)。</span><span class="sxs-lookup"><span data-stu-id="e6234-167">For more information on how to configure these firewall exceptions in Windows Firewall, see [System and Platform Requirements for the Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805128).</span></span> <span data-ttu-id="e6234-168">適用於其他防火牆系統，請參閱產品文件。</span><span class="sxs-lookup"><span data-stu-id="e6234-168">For other firewall systems, consult your product documentation.</span></span>

<span data-ttu-id="e6234-169">您可以使用下一個程序，將網域帳戶新增至 Windows Server 2008 R2 中的本機 administrators 群組。</span><span class="sxs-lookup"><span data-stu-id="e6234-169">You can use the next procedure to add a domain account to the local administrators group in Windows Server 2008 R2.</span></span> <span data-ttu-id="e6234-170">您應該在您想要新增至伺服器陣列 & #x 2014; 每一部伺服器上執行此程序亦即，將相同的網域帳戶加入至主要伺服器和每個次要伺服器上的本機 administrators 群組。</span><span class="sxs-lookup"><span data-stu-id="e6234-170">You should perform this procedure on every server that you want to add to the server farm&#x2014;in other words, add the same domain account to the local administrators group on the primary server and on each secondary server.</span></span>

<span data-ttu-id="e6234-171">**將網域帳戶新增至本機 administrators 群組**</span><span class="sxs-lookup"><span data-stu-id="e6234-171">**To add a domain account to the local administrators group**</span></span>

1. <span data-ttu-id="e6234-172">在**啟動**功能表上，指向**系統管理工具**，然後按一下 **伺服器管理員**。</span><span class="sxs-lookup"><span data-stu-id="e6234-172">On the **Start** menu, point to **Administrative Tools**, and then click **Server Manager**.</span></span>
2. <span data-ttu-id="e6234-173">在**伺服器管理員**視窗中的，於樹狀檢視窗格中，展開**組態**，依序展開**本機使用者和群組**，然後按一下 **群組**.</span><span class="sxs-lookup"><span data-stu-id="e6234-173">In the **Server Manager** window, in the tree view pane, expand **Configuration**, expand **Local Users and Groups**, and then click **Groups**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. <span data-ttu-id="e6234-174">在**群組** 窗格中，按兩下**管理員**。</span><span class="sxs-lookup"><span data-stu-id="e6234-174">In the **Groups** pane, double-click **Administrators**.</span></span>
4. <span data-ttu-id="e6234-175">在**系統管理員屬性**對話方塊中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e6234-175">In the **Administrators Properties** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="e6234-176">在**選取使用者、 電腦、 服務帳戶或群組**對話方塊、 型別 （或瀏覽） 到您的網域帳戶 (例如， **FABRIKAM\stagingfarm**)，然後按一下 **確定**.</span><span class="sxs-lookup"><span data-stu-id="e6234-176">In the **Select Users, Computers, Service Accounts, or Groups** dialog box, type (or browse) to your domain account (for example, **FABRIKAM\stagingfarm**), and then click **OK**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. <span data-ttu-id="e6234-177">在**系統管理員屬性**對話方塊中，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="e6234-177">In the **Administrators Properties** dialog box, click **OK**.</span></span>

<span data-ttu-id="e6234-178">您的伺服器現在已準備好新增至伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-178">Your servers are now ready to be added to a server farm.</span></span> <span data-ttu-id="e6234-179">如果主要伺服器中，您可以設定伺服器以符合之前或之後建立的伺服器陣列 & #x 2014年的應用程式的需求; 在這兩種情況下，WFF 會同步處理伺服器所部署的相同產品的元件，或次要伺服器的組態。</span><span class="sxs-lookup"><span data-stu-id="e6234-179">In the case of the primary server, you can configure the server to meet your application requirements before or after you create the server farm&#x2014;in both cases, the WFF will synchronize the servers by deploying the same products, components, or configuration to your secondary servers.</span></span> <span data-ttu-id="e6234-180">為了簡單起見，本教學課程假設您會將主要伺服器設定完成後建立伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-180">For the sake of simplicity, this tutorial assumes that you'll configure the primary server when you've finished creating the server farm.</span></span>

## <a name="create-the-wff-server-farm"></a><span data-ttu-id="e6234-181">建立 WFF 伺服器陣列</span><span class="sxs-lookup"><span data-stu-id="e6234-181">Create the WFF Server Farm</span></span>

<span data-ttu-id="e6234-182">此時，您的伺服器已經準備要加入至 WFF 伺服器陣列：</span><span class="sxs-lookup"><span data-stu-id="e6234-182">At this point, all your servers are ready to be added to a WFF server farm:</span></span>

- <span data-ttu-id="e6234-183">您已安裝 WFF controller 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-183">You've installed WFF on the controller server.</span></span>
- <span data-ttu-id="e6234-184">您已在主要和次要網站伺服器上設定防火牆例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e6234-184">You've configured firewall exceptions on your primary and secondary web servers.</span></span>
- <span data-ttu-id="e6234-185">您已加入至本機 administrators 群組的網域帳戶，主要和次要網站伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-185">You've added a domain account to the local administrators group on your primary and secondary web servers.</span></span>

<span data-ttu-id="e6234-186">下一個步驟是建立伺服器陣列中 WFF。</span><span class="sxs-lookup"><span data-stu-id="e6234-186">The next step is to create the server farm in WFF.</span></span> <span data-ttu-id="e6234-187">您可以從 IIS 管理員 WFF 控制站伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-187">You can do this from IIS Manager on the WFF controller server.</span></span>

<span data-ttu-id="e6234-188">**若要建立 WFF 伺服器陣列**</span><span class="sxs-lookup"><span data-stu-id="e6234-188">**To create a WFF server farm**</span></span>

1. <span data-ttu-id="e6234-189">WFF 控制器伺服器上**啟動**功能表上，指向**系統管理工具**，然後按一下 **網際網路資訊服務 (IIS) 管理員**。</span><span class="sxs-lookup"><span data-stu-id="e6234-189">On the WFF controller server, on the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
2. <span data-ttu-id="e6234-190">在**連線**] 窗格中，展開本機伺服器節點，以滑鼠右鍵按一下**伺服器陣列**，然後按一下 [**建立伺服器陣列**。</span><span class="sxs-lookup"><span data-stu-id="e6234-190">In the **Connections** pane, expand the local server node, right-click **Server Farms**, and then click **Create Server Farm**.</span></span>
3. <span data-ttu-id="e6234-191">中**建立伺服器陣列**對話方塊方塊中，輸入伺服器陣列有意義的名稱 (例如，**臨時伺服器陣列**)，然後選取**佈建伺服器陣列**。</span><span class="sxs-lookup"><span data-stu-id="e6234-191">In the **Create Server Farm** dialog box, type a meaningful name for the server farm (for example, **Staging Farm**), and then select **Provision server farm**.</span></span>
4. <span data-ttu-id="e6234-192">輸入使用者名稱和您加入至每一部伺服器上的本機 administrators 群組之網域帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="e6234-192">Type the user name and password of the domain account that you added to the local administrators group on each server.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. <span data-ttu-id="e6234-193">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="e6234-193">Click **Next**.</span></span>
6. <span data-ttu-id="e6234-194">在**新增伺服器**頁面上，輸入完整的網域名稱 (FQDN) 的主要伺服器，選取**主要伺服器**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e6234-194">On the **Add Servers** page, type the fully qualified domain name (FQDN) of the primary server, select **Primary Server**, and then click **Add**.</span></span>
7. <span data-ttu-id="e6234-195">此時，WFF 會嘗試連絡主要伺服器使用您提供的認證。</span><span class="sxs-lookup"><span data-stu-id="e6234-195">At this point, WFF will attempt to contact the primary server using the credentials you provided.</span></span> <span data-ttu-id="e6234-196">如果連線成功，主要伺服器將加入至資料表**新增伺服器**頁面。</span><span class="sxs-lookup"><span data-stu-id="e6234-196">If the connection succeeds, the primary server will be added to the table on the **Add Servers** page.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="e6234-197">您可能已注意到，**伺服器是可用於負載平衡**預設會選取。</span><span class="sxs-lookup"><span data-stu-id="e6234-197">You might have noticed that **Server is available for Load Balancing** is selected by default.</span></span> <span data-ttu-id="e6234-198">WFF 實作負載平衡，並藉此將要求到您的伺服器陣列中的 web 伺服器使用 IIS ARR 模組。</span><span class="sxs-lookup"><span data-stu-id="e6234-198">WFF uses the IIS ARR module to implement load balancing and thereby distribute requests across the web servers in your server farm.</span></span> <span data-ttu-id="e6234-199">在大部分情況下，您可以只清除**伺服器可以使用負載平衡**選項，如果您想要使用協力廠商負載平衡解決方案，而。</span><span class="sxs-lookup"><span data-stu-id="e6234-199">In most scenarios, you'd only clear the **Server is available for Load Balancing** option if you wanted to use a third-party load balancing solution instead.</span></span>
8. <span data-ttu-id="e6234-200">在**新增伺服器**頁面上，輸入第一部次要伺服器的 FQDN，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="e6234-200">On the **Add Servers** page, type the FQDN of your first secondary server, and then click **Add**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. <span data-ttu-id="e6234-201">針對您的伺服陣列中的任何其他次要伺服器重複步驟 7，然後按一下 **完成**。</span><span class="sxs-lookup"><span data-stu-id="e6234-201">Repeat step 7 for any additional secondary servers in your farm, and then click **Finish**.</span></span>

<span data-ttu-id="e6234-202">WFF 伺服器陣列現在會啟動並執行。</span><span class="sxs-lookup"><span data-stu-id="e6234-202">Your WFF server farm is now up and running.</span></span> <span data-ttu-id="e6234-203">任何 web 平台產品或您在主要伺服器，任何 web 應用程式或內容部署到主要伺服器安裝的元件會自動佈建您所有的次要伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-203">Any web platform products or components that you install on the primary server, and any web applications or content that you deploy to the primary server, will be automatically provisioned on all your secondary servers.</span></span>

<span data-ttu-id="e6234-204">WFF 是廣泛且複雜的主題，以及您可以深入了解上[Microsoft Web Farm Framework 2.0 適用於 IIS 7](https://go.microsoft.com/?linkid=9805129)網站。</span><span class="sxs-lookup"><span data-stu-id="e6234-204">WFF is a broad and complex topic, and you can learn more about it on the [Microsoft Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805129) website.</span></span> <span data-ttu-id="e6234-205">目前 nano，不過，有，您必須要注意的兩個功能區域：</span><span class="sxs-lookup"><span data-stu-id="e6234-205">For the time being, however, there are two features areas that you need to be aware of:</span></span>

- <span data-ttu-id="e6234-206">*應用程式佈建*是可將內容從主要伺服器，例如 web 應用程式和組態設定複寫到伺服器陣列中的所有次要伺服器的程序。</span><span class="sxs-lookup"><span data-stu-id="e6234-206">*Application provisioning* is the process that replicates content from the primary server, like web applications and configuration settings, across all the secondary servers in the server farm.</span></span> <span data-ttu-id="e6234-207">例如，如果您將連絡人管理員範例方案部署到預備環境，在主要伺服器時，WFF 應用程式佈建程序將這個方案部署至所有次要臨時伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-207">For example, if you deploy the Contact Manager sample solution to your primary staging server, the WFF application provisioning process will deploy this solution to all your secondary staging servers.</span></span> <span data-ttu-id="e6234-208">根據預設，應用程式佈建程序執行每隔 30 秒。</span><span class="sxs-lookup"><span data-stu-id="e6234-208">By default, the application provisioning process runs every 30 seconds.</span></span>
- <span data-ttu-id="e6234-209">*平台佈建*是同步處理的 web platform 產品和元件透過從主要伺服器的伺服器陣列中的所有次要伺服器的程序。</span><span class="sxs-lookup"><span data-stu-id="e6234-209">*Platform provisioning* is the process that synchronizes web platform products and components from the primary server to all the secondary servers in the server farm.</span></span> <span data-ttu-id="e6234-210">例如，如果您主要的臨時伺服器上安裝 ASP.NET MVC 3，平台佈建程序會在所有次要臨時伺服器上安裝 ASP.NET MVC 3 使用 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="e6234-210">For example, if you install ASP.NET MVC 3 on your primary staging server, the platform provisioning process will use the Web Platform Installer to install ASP.NET MVC 3 on all your secondary staging servers.</span></span> <span data-ttu-id="e6234-211">根據預設，平台佈建程序執行每隔五分鐘。</span><span class="sxs-lookup"><span data-stu-id="e6234-211">By default, the platform provisioning process runs every five minutes.</span></span>

<span data-ttu-id="e6234-212">您可以管理基本的應用程式與平台佈建的設定從 IIS 管理員 WFF 控制站伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-212">You can manage basic application and platform provisioning settings from IIS Manager on your WFF controller server.</span></span>

<span data-ttu-id="e6234-213">**瀏覽應用程式和佈建設定的平台**</span><span class="sxs-lookup"><span data-stu-id="e6234-213">**Explore application and platform provisioning settings**</span></span>

1. <span data-ttu-id="e6234-214">在 IIS 管理員 中，在**連線** 窗格中，選取您的伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="e6234-214">In IIS Manager, in the **Connections** pane, select your server farm.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. <span data-ttu-id="e6234-215">在**伺服器陣列** 窗格中，按兩下**應用程式佈建**。</span><span class="sxs-lookup"><span data-stu-id="e6234-215">In the **Server Farm** pane, double-click **Application Provisioning**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. <span data-ttu-id="e6234-216">如您所見，伺服器陣列目前設定同步處理之間的主要伺服器和次要伺服器的 web 內容和組態設定每隔 30 秒。</span><span class="sxs-lookup"><span data-stu-id="e6234-216">As you can see, the server farm is currently configured to synchronize web content and configuration settings between the primary server and the secondary servers every 30 seconds.</span></span>
4. <span data-ttu-id="e6234-217">按一下**回**，然後按兩下**平台佈建**。</span><span class="sxs-lookup"><span data-stu-id="e6234-217">Click **Back**, and then double-click **Platform Provisioning**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. <span data-ttu-id="e6234-218">如您所見，伺服器陣列目前設定要同步處理的 web platform 產品和元件之間的主要伺服器和次要伺服器每隔五分鐘。</span><span class="sxs-lookup"><span data-stu-id="e6234-218">As you can see, the server farm is currently configured to synchronize web platform products and components between the primary server and the secondary servers every five minutes.</span></span>
6. <span data-ttu-id="e6234-219">按一下**回**。</span><span class="sxs-lookup"><span data-stu-id="e6234-219">Click **Back**.</span></span>
7. <span data-ttu-id="e6234-220">若要強制立即同步處理的 web platform 產品的伺服器陣列中**動作**] 窗格中，按一下 [**佈建的平台**。</span><span class="sxs-lookup"><span data-stu-id="e6234-220">To force the server farm to synchronize web platform products immediately, in the **Actions** pane, click **Provision Platform**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > <span data-ttu-id="e6234-221">平台佈建，可能需要一些時間。</span><span class="sxs-lookup"><span data-stu-id="e6234-221">Platform provisioning may take some time.</span></span> <span data-ttu-id="e6234-222">在您的伺服器陣列中的次要伺服器上，在背景中執行安裝程式程序。</span><span class="sxs-lookup"><span data-stu-id="e6234-222">The installer process runs in the background on the secondary servers in your server farm.</span></span>
8. <span data-ttu-id="e6234-223">一旦您已允許有足夠的時間才能完成佈建程序，您可以確認產品和您加入至主要伺服器的元件已現在已複寫的次要伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-223">Once you've allowed sufficient time for the provisioning process to complete, you can verify that the products and components that you added to the primary server have now been replicated on the secondary servers.</span></span> <span data-ttu-id="e6234-224">例如，您可以在 登入到次要伺服器，並使用**伺服器管理員**視窗，以確認是否已安裝網頁伺服器角色。</span><span class="sxs-lookup"><span data-stu-id="e6234-224">For example, you can log on to a secondary server and use the **Server Manager** window to verify that the web server role has been installed.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. <span data-ttu-id="e6234-225">您也可以檢查以確認已新增各種 web 平台元件的安裝的程式清單。</span><span class="sxs-lookup"><span data-stu-id="e6234-225">You can also check the installed programs list to verify that various web platform components have been added.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a><span data-ttu-id="e6234-226">設定負載平衡</span><span class="sxs-lookup"><span data-stu-id="e6234-226">Configure Load Balancing</span></span>

<span data-ttu-id="e6234-227">當您建立的 web 伺服陣列時，您需要設定某種形式的負載平衡機制來發佈您的 web 伺服器之間的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="e6234-227">When you create a web farm, you need to set up some form of load balancing to distribute HTTP requests between your web servers.</span></span> <span data-ttu-id="e6234-228">這可能是 Windows Server 2008 網路負載平衡，IIS ARR 或協力廠商軟體或硬體負載平衡解決方案。</span><span class="sxs-lookup"><span data-stu-id="e6234-228">This could be Windows Server 2008 network load balancing, IIS ARR, or a third-party software-based or hardware-based load balancing solution.</span></span>

<span data-ttu-id="e6234-229">WFF 被設計來與 IIS ARR 緊密整合</span><span class="sxs-lookup"><span data-stu-id="e6234-229">WFF is designed to integrate closely with IIS ARR.</span></span> <span data-ttu-id="e6234-230">若要利用這項整合，您需要安裝 ARR 模組 WFF 控制站伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-230">To take advantage of this integration, you need to install the ARR module on the WFF controller server.</span></span> <span data-ttu-id="e6234-231">您再直接控制站伺服器，您所有的 web 流量通常藉由設定網域名稱系統 (DNS) 記錄。</span><span class="sxs-lookup"><span data-stu-id="e6234-231">You then direct all your web traffic to the controller server, typically by configuring Domain Name System (DNS) records.</span></span> <span data-ttu-id="e6234-232">控制站伺服器接著會發佈在陣列中，根據伺服器的可用性和各種其他條件在伺服器之間的連入要求。</span><span class="sxs-lookup"><span data-stu-id="e6234-232">The controller server will then distribute incoming requests among the servers in your farm, based on server availability and various other criteria.</span></span>

> [!NOTE]
> <span data-ttu-id="e6234-233">您不必使用 ARR 與 WFF;您可以設定 WFF 使用協力廠商負載平衡解決方案。</span><span class="sxs-lookup"><span data-stu-id="e6234-233">You don't have to use ARR with WFF; you can configure WFF to work with third-party load balancing solutions.</span></span> <span data-ttu-id="e6234-234">如需詳細資訊，請參閱[概觀 Web Farm Framework 2.0，適用於 IIS 7](https://go.microsoft.com/?linkid=9805126)。</span><span class="sxs-lookup"><span data-stu-id="e6234-234">For more information, see [Overview of the Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805126).</span></span>


<span data-ttu-id="e6234-235">負載平衡使用 ARR 是一個複雜的主題，最其中已超出本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="e6234-235">Load balancing using ARR is a complex topic, most of which is beyond the scope of this tutorial.</span></span> <span data-ttu-id="e6234-236">不過，您可以使用下一個程序來安裝 ARR 模組，並開始使用負載平衡。</span><span class="sxs-lookup"><span data-stu-id="e6234-236">However, you can use the next procedure to install the ARR module and get started with load balancing.</span></span>

<span data-ttu-id="e6234-237">**若要設定 WFF 控制站伺服器上的負載平衡**</span><span class="sxs-lookup"><span data-stu-id="e6234-237">**To set up load balancing on the WFF controller server**</span></span>

1. <span data-ttu-id="e6234-238">在 WFF 控制站伺服器上，啟動 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="e6234-238">On the WFF controller server, launch the Web Platform Installer.</span></span>
2. <span data-ttu-id="e6234-239">在頂端**Web Platform Installer 3.0**視窗中，按一下 **產品**。</span><span class="sxs-lookup"><span data-stu-id="e6234-239">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
3. <span data-ttu-id="e6234-240">在左邊視窗中，瀏覽窗格中按一下 **伺服器**。</span><span class="sxs-lookup"><span data-stu-id="e6234-240">On the left side of the window, in the navigation pane, click **Server**.</span></span>
4. <span data-ttu-id="e6234-241">在**應用程式要求路由 2.5**資料列中，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e6234-241">In the **Application Request Routing 2.5** row, click **Add**.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. <span data-ttu-id="e6234-242">按一下**安裝**，然後依照中的指示**Web 平台安裝**視窗。</span><span class="sxs-lookup"><span data-stu-id="e6234-242">Click **Install**, and then follow the instructions in the **Web Platform Installation** window.</span></span>
6. <span data-ttu-id="e6234-243">當安裝完成時，啟動 [IIS 管理員中，然後在**連線**] 窗格中，按一下您伺服器的伺服器陣列節點。</span><span class="sxs-lookup"><span data-stu-id="e6234-243">When the installation is complete, launch IIS Manager, and in the **Connections** pane, click your server farm node.</span></span> <span data-ttu-id="e6234-244">請注意，已加入數個新的圖示**伺服器陣列**窗格。</span><span class="sxs-lookup"><span data-stu-id="e6234-244">Notice that several new icons have been added to the **Server Farm** pane.</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. <span data-ttu-id="e6234-245">在**伺服器陣列** 窗格中，按兩下**負載平衡**。</span><span class="sxs-lookup"><span data-stu-id="e6234-245">In the **Server Farm** pane, double-click **Load Balance**.</span></span>
8. <span data-ttu-id="e6234-246">在**負載平衡** 窗格中，選取的負載平衡演算法 (例如，**至少目前要求**)。</span><span class="sxs-lookup"><span data-stu-id="e6234-246">In the **Load Balance** pane, select a load balance algorithm (for example, **Least current request**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6234-247">如需有關負載平衡演算法和其他組態設定的詳細資訊，請參閱[應用程式要求路由模組](https://go.microsoft.com/?linkid=9805130)。</span><span class="sxs-lookup"><span data-stu-id="e6234-247">For more information on load balancing algorithms and other configuration settings, see [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).</span></span>

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. <span data-ttu-id="e6234-248">在**動作**] 窗格中，按一下 [**套用**。</span><span class="sxs-lookup"><span data-stu-id="e6234-248">In the **Actions** pane, click **Apply**.</span></span>

<span data-ttu-id="e6234-249">您現在已設定基本的負載平衡的伺服器陣列中的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6234-249">You have now configured basic load balancing for the servers in your server farm.</span></span> <span data-ttu-id="e6234-250">如果您您 web 伺服器陣列將所有流量都導向的控制站伺服器，將您根據可用性的伺服器陣列中的伺服器與您選取的負載平衡演算法之間分散要求。</span><span class="sxs-lookup"><span data-stu-id="e6234-250">If you direct all your web farm traffic to the controller server, the requests will be distributed between the servers in your farm according to availability and the load balancing algorithm you selected.</span></span>

<span data-ttu-id="e6234-251">如需有關如何設定負載平衡使用 ARR 的詳細資訊，請參閱[應用程式要求路由模組](https://go.microsoft.com/?linkid=9805130)。</span><span class="sxs-lookup"><span data-stu-id="e6234-251">For more information on how to configure load balancing with ARR, see [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).</span></span>

## <a name="monitor-the-server-farm"></a><span data-ttu-id="e6234-252">監視伺服器陣列</span><span class="sxs-lookup"><span data-stu-id="e6234-252">Monitor the Server Farm</span></span>

<span data-ttu-id="e6234-253">您可以隨時透過 IIS 管理員的控制站伺服器監視您的伺服器陣列的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="e6234-253">You can monitor the health of your server farm at any time through IIS Manager on the controller server.</span></span> <span data-ttu-id="e6234-254">在**連線** 窗格中，展開您的伺服器陣列，然後按一下**伺服器**。</span><span class="sxs-lookup"><span data-stu-id="e6234-254">In the **Connections** pane, expand your server farm, and then click **Servers**.</span></span> <span data-ttu-id="e6234-255">中央窗格會顯示最近活動的追蹤記錄檔以及伺服器陣列中的每一部伺服器的摘要。</span><span class="sxs-lookup"><span data-stu-id="e6234-255">The center pane will show a summary of each server in the farm together with a trace log of recent activity.</span></span>

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a><span data-ttu-id="e6234-256">結論</span><span class="sxs-lookup"><span data-stu-id="e6234-256">Conclusion</span></span>

<span data-ttu-id="e6234-257">WFF 伺服器陣列時，現在應該可以啟動並執行。</span><span class="sxs-lookup"><span data-stu-id="e6234-257">Your WFF server farm should now be up and running.</span></span> <span data-ttu-id="e6234-258">您可以設定主要伺服器，以支援任何您偏好的部署方法 & #x 2014年，請參閱 「 進一步閱讀區段以瞭解詳細資料 （& s） #x 2014; 和您的設定會複寫伺服器陣列中每個次要伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e6234-258">You can configure the primary server to support whichever deployment approach you prefer&#x2014;see the Further Reading section for details&#x2014;and your configuration will be replicated on each secondary server in the server farm.</span></span>

## <a name="further-reading"></a><span data-ttu-id="e6234-259">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="e6234-259">Further Reading</span></span>

<span data-ttu-id="e6234-260">如需詳細的設定和使用 WFF 所有層面的詳細指引，請參閱[Microsoft Web Farm Framework 2.0 適用於 IIS 7](https://go.microsoft.com/?linkid=9805129)網站。</span><span class="sxs-lookup"><span data-stu-id="e6234-260">For more guidance on all aspects of configuring and using the WFF, see the [Microsoft Web Farm Framework 2.0 for IIS 7](https://go.microsoft.com/?linkid=9805129) website.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e6234-261">[上一頁](configuring-a-database-server-for-web-deploy-publishing.md)
[下一頁](configuring-deployment-properties-for-a-target-environment.md)</span><span class="sxs-lookup"><span data-stu-id="e6234-261">[Previous](configuring-a-database-server-for-web-deploy-publishing.md)
[Next](configuring-deployment-properties-for-a-target-environment.md)</span></span>
