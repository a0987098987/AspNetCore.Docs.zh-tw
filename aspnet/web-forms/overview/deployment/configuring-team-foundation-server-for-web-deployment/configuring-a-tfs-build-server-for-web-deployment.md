---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: 設定 TFS 組建伺服器，Web 部署 |Microsoft Docs
author: jrjlee
description: 本主題說明如何準備 Team Foundation Server (TFS) 組建伺服器，來建置及部署您的解決方案使用 Team Build 和網際網路資訊...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7f57ad0392a068964bb910fbbaafea105fdbb3d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841337"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a><span data-ttu-id="6ea9a-103">設定 Web 部署的 TFS 組建伺服器</span><span class="sxs-lookup"><span data-stu-id="6ea9a-103">Configuring a TFS Build Server for Web Deployment</span></span>
====================
<span data-ttu-id="6ea9a-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="6ea9a-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="6ea9a-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="6ea9a-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="6ea9a-106">本主題說明如何準備要建置及部署您的解決方案使用 Team Build 和 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 的 Team Foundation Server (TFS) 組建伺服器。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-106">This topic describes how to prepare a Team Foundation Server (TFS) build server to build and deploy your solutions using Team Build and the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span>


<span data-ttu-id="6ea9a-107">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="6ea9a-108">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="6ea9a-109">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="6ea9a-110">工作概觀</span><span class="sxs-lookup"><span data-stu-id="6ea9a-110">Task Overview</span></span>

<span data-ttu-id="6ea9a-111">若要準備組建伺服器，來建置及部署您的解決方案，您將需要：</span><span class="sxs-lookup"><span data-stu-id="6ea9a-111">To prepare a build server to build and deploy your solutions, you'll need to:</span></span>

- <span data-ttu-id="6ea9a-112">安裝和設定 TFS 組建服務。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-112">Install and configure the TFS build service.</span></span>
- <span data-ttu-id="6ea9a-113">安裝 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-113">Install Visual Studio 2010.</span></span>
- <span data-ttu-id="6ea9a-114">安裝任何產品或建置您的解決方案，如同舊版的.NET Framework 或 ASP.NET MVC 所需的元件。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-114">Install any products or components that are required to build your solution, like versions of the .NET Framework or ASP.NET MVC.</span></span>
- <span data-ttu-id="6ea9a-115">安裝 Web Deploy 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-115">Install Web Deploy 2.0 or later.</span></span>

<span data-ttu-id="6ea9a-116">本主題將說明如何執行這些程序，或指向其所在的其他資源。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-116">This topic will show you how to perform these procedures or point to other resources where they exist.</span></span> <span data-ttu-id="6ea9a-117">工作與本主題中的逐步解說假設：</span><span class="sxs-lookup"><span data-stu-id="6ea9a-117">The tasks and walkthroughs in this topic assume that:</span></span>

- <span data-ttu-id="6ea9a-118">您開始使用執行 Windows Server 2008 R2 Service Pack 1 的乾淨的伺服器組建。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-118">You're starting with a clean server build running Windows Server 2008 R2 Service Pack 1.</span></span>
- <span data-ttu-id="6ea9a-119">伺服器已加入網域，使用靜態 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-119">The server is domain-joined with a static IP address.</span></span>
- <span data-ttu-id="6ea9a-120">中所述，您已在不同的伺服器上安裝 TFS 應用程式層[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-120">You've installed the TFS application tier on a separate server, as described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="6ea9a-121">對於執行這些程序？</span><span class="sxs-lookup"><span data-stu-id="6ea9a-121">Who Performs These Procedures?</span></span>

<span data-ttu-id="6ea9a-122">在大部分情況下，TFS 系統管理員會負責設定組建伺服器。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-122">In most cases, a TFS administrator will be responsible for configuring build servers.</span></span> <span data-ttu-id="6ea9a-123">在某些情況下，開發人員小組可能需要特定的組建伺服器的擁有權。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-123">In some cases, the developer team may take ownership of specific build servers.</span></span>

## <a name="install-and-configure-the-tfs-build-service"></a><span data-ttu-id="6ea9a-124">安裝和設定 TFS 組建服務</span><span class="sxs-lookup"><span data-stu-id="6ea9a-124">Install and Configure the TFS Build Service</span></span>

<span data-ttu-id="6ea9a-125">當您設定組建伺服器時，您的第一個工作是安裝和設定 TFS 組建服務。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-125">When you configure a build server, your first task is to install and configure the TFS build service.</span></span> <span data-ttu-id="6ea9a-126">此程序的一部分，您必須：</span><span class="sxs-lookup"><span data-stu-id="6ea9a-126">As part of this process, you'll need to:</span></span>

- <span data-ttu-id="6ea9a-127">安裝 TFS 組建服務，並設定服務帳戶。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-127">Install the TFS build service and configure a service account.</span></span> <span data-ttu-id="6ea9a-128">任何建置工作，包括部署，會使用組建服務帳戶的身分識別來執行。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-128">Any build tasks, including deployment, will run using the identity of the build service account.</span></span>
- <span data-ttu-id="6ea9a-129">建立*組建控制器*和一或多個*組建代理程式*。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-129">Create a *build controller* and one or more *build agents*.</span></span> <span data-ttu-id="6ea9a-130">每個組建控制器會管理一組組建代理程式。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-130">Each build controller manages a set of build agents.</span></span> <span data-ttu-id="6ea9a-131">您的組建排入佇列，組建控制器會建置工作指派給可用的組建代理程式。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-131">When you queue a build, the build controller assigns the build task to an available build agent.</span></span> <span data-ttu-id="6ea9a-132">在 TFS 中的每個 team 專案集合會對應至單一組建控制器。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-132">Each team project collection in TFS is mapped to a single build controller.</span></span>
- <span data-ttu-id="6ea9a-133">設定您的組建輸出置放資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-133">Configure a drop folder for your build outputs.</span></span> <span data-ttu-id="6ea9a-134">這是網路共用。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-134">This is a network share.</span></span> <span data-ttu-id="6ea9a-135">任何組建輸出，例如 web 部署套件，會傳送至放置資料夾。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-135">Any build outputs, like web deployment packages, are sent to the drop folder.</span></span>

<span data-ttu-id="6ea9a-136">[管理 Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) MSDN 上的一章包含您需要才能執行這些工作的所有資源：</span><span class="sxs-lookup"><span data-stu-id="6ea9a-136">The [Administering Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) chapter on MSDN contains all the resources you need in order to perform these tasks:</span></span>

- <span data-ttu-id="6ea9a-137">Team Foundation Build 的概念性概觀，包括組建服務、 組建控制器和組建代理程式，請參閱[了解 Team Foundation Build 系統](https://msdn.microsoft.com/library/dd793166.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-137">For a conceptual overview of Team Foundation Build, including the build service, build controllers, and build agents, see [Understanding a Team Foundation Build System](https://msdn.microsoft.com/library/dd793166.aspx).</span></span>
- <span data-ttu-id="6ea9a-138">如需安裝和設定組建服務的詳細資訊，請參閱[設定組建電腦](https://msdn.microsoft.com/library/ms181712.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-138">For information on installing and configuring the build service, see [Configure a Build Machine](https://msdn.microsoft.com/library/ms181712.aspx).</span></span>
- <span data-ttu-id="6ea9a-139">如需建立組建控制器的詳細資訊，請參閱[建立和使用組建的控制器工作](https://msdn.microsoft.com/library/ee330987.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-139">For information on creating build controllers, see [Create and Work with a Build Controller](https://msdn.microsoft.com/library/ee330987.aspx).</span></span>
- <span data-ttu-id="6ea9a-140">如需建立組建代理程式的資訊，請參閱[建立和使用組建代理程式工作](https://msdn.microsoft.com/library/bb399135.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-140">For information on creating build agents, see [Create and Work with Build Agents](https://msdn.microsoft.com/library/bb399135.aspx).</span></span>
- <span data-ttu-id="6ea9a-141">如需建立和設定置放資料夾的詳細資訊，請參閱[設定置放資料夾](https://msdn.microsoft.com/library/bb778394.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-141">For information on creating and configuring drop folders, see [Set Up Drop Folders](https://msdn.microsoft.com/library/bb778394.aspx).</span></span>

## <a name="install-required-products-and-components"></a><span data-ttu-id="6ea9a-142">安裝必要的產品和元件</span><span class="sxs-lookup"><span data-stu-id="6ea9a-142">Install Required Products and Components</span></span>

<span data-ttu-id="6ea9a-143">若要讓組建伺服器，來建置您的解決方案，您必須安裝任何產品、 元件或您的解決方案需要的組件。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-143">To enable the build server to build your solutions, you must install any products, components, or assemblies that your solution requires.</span></span> <span data-ttu-id="6ea9a-144">在安裝任何 web 平台元件之前，您應該在組建伺服器上安裝 Visual Studio 2010 （任何版本）。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-144">Before you install any web platform components, you should install Visual Studio 2010 (any version) on the build server.</span></span> <span data-ttu-id="6ea9a-145">這可確保將核心 Microsoft Build Engine (MSBuild) 目標檔案及 Web 發行管線 (WPP) 目標檔案會提供給組建服務。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-145">This ensures that the core Microsoft Build Engine (MSBuild) target files and the Web Publishing Pipeline (WPP) target files are available to the build service.</span></span> <span data-ttu-id="6ea9a-146">Visual Studio 安裝程式也應該安裝 Web Deploy，如果您打算將 web 套件部署您的建置程序的一部分，您將需要它。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-146">The Visual Studio installer should also install Web Deploy, which you'll need if you plan to deploy web packages as part of your build process.</span></span>

<span data-ttu-id="6ea9a-147">若要安裝通用的 web 平台元件的最佳方式是使用[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-147">The best way to install common web platform components is to use the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span> <span data-ttu-id="6ea9a-148">這可確保您要安裝最新版的每個產品，而且它也會自動偵測並安裝任何必要條件，針對每個產品。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-148">This ensures that you're installing the latest version of each product, and it also automatically detects and installs any prerequisites for each product.</span></span> <span data-ttu-id="6ea9a-149">若是[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)解決方案，您應該使用 Web Platform Installer 來安裝這些產品和元件：</span><span class="sxs-lookup"><span data-stu-id="6ea9a-149">In the case of the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution, you should use the Web Platform Installer to install these products and components:</span></span>

- <span data-ttu-id="6ea9a-150">**.NET framework 4.0**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-150">**.NET Framework 4.0**.</span></span> <span data-ttu-id="6ea9a-151">如此才能執行這個版本的.NET Framework 所建置的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-151">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="6ea9a-152">**Web Deployment Tool 2.1 或更新版本**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-152">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="6ea9a-153">這會在您的伺服器上安裝 Web Deploy （和其基礎可執行檔，MSDeploy.exe）。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-153">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="6ea9a-154">此程序的一部分，它將會安裝並啟動 Web 部署代理程式服務。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-154">As part of this process, it installs and starts the Web Deployment Agent Service.</span></span> <span data-ttu-id="6ea9a-155">這項服務可讓您從遠端電腦的 web 套件部署。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-155">This service lets you deploy web packages from a remote computer.</span></span>
- <span data-ttu-id="6ea9a-156">**ASP.NET MVC 3**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-156">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="6ea9a-157">這會安裝您需要執行 ASP.NET MVC 3 應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-157">This installs the assemblies you need to run ASP.NET MVC 3 applications.</span></span>

<span data-ttu-id="6ea9a-158">**若要安裝必要的產品和元件**</span><span class="sxs-lookup"><span data-stu-id="6ea9a-158">**To install the required products and components**</span></span>

1. <span data-ttu-id="6ea9a-159">安裝 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-159">Install Visual Studio 2010.</span></span> <span data-ttu-id="6ea9a-160">當系統提示您選取要安裝的功能，您應該包含：</span><span class="sxs-lookup"><span data-stu-id="6ea9a-160">When prompted to select features to install, you should include:</span></span>

    1. <span data-ttu-id="6ea9a-161">您需要編譯任何程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-161">Any programming languages that you need to compile.</span></span>
    2. <span data-ttu-id="6ea9a-162">Visual Web Developer。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-162">Visual Web Developer.</span></span> <span data-ttu-id="6ea9a-163">這可確保 WPP 目標會加入您的組建伺服器。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-163">This ensures that the WPP targets are added to your build server.</span></span>

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. <span data-ttu-id="6ea9a-164">Visual Studio 2010 安裝完成時，下載並安裝[Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) （如果它尚未包含在您的安裝媒體）。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-164">When the installation of Visual Studio 2010 is complete, download and install [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (if it's not already included in your installation media).</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ea9a-165">Visual Studio 2010 Service Pack 1 可解決的 bug，可以防止 MSBuild 尋找 MSDeploy 可執行檔。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-165">Visual Studio 2010 Service Pack 1 resolves a bug that can prevent MSBuild from locating the MSDeploy executable.</span></span>
3. <span data-ttu-id="6ea9a-166">下載並啟動[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-166">Download and launch the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
4. <span data-ttu-id="6ea9a-167">在頂端**Web Platform Installer 3.0**  視窗中，按一下**產品**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-167">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
5. <span data-ttu-id="6ea9a-168">在左邊視窗中，在導覽窗格中，按一下 **架構**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-168">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
6. <span data-ttu-id="6ea9a-169">在  **Microsoft.NET Framework 4**資料列，如果尚未安裝.NET Framework，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-169">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6ea9a-170">您可能已經安裝.NET Framework 4.0，透過 Windows Update。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-170">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="6ea9a-171">如果已安裝的產品或元件，Web Platform Installer 會指出這藉由取代**新增**按鈕，但文字**已安裝**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-171">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. <span data-ttu-id="6ea9a-172">在  **ASP.NET MVC 3 (Visual Studio 2010)** 資料列中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-172">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
8. <span data-ttu-id="6ea9a-173">在 [導覽] 窗格中，按一下**Server**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-173">In the navigation pane, click **Server**.</span></span>
9. <span data-ttu-id="6ea9a-174">在  **Web 部署工具 2.1**資料列中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-174">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="6ea9a-175">按一下 [安裝] 。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-175">Click **Install**.</span></span> <span data-ttu-id="6ea9a-176">Web Platform Installer 將會顯示您的產品清單&#x2014;以及任何相關聯相依性&#x2014;安裝就會提示您接受授權條款。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-176">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>
11. <span data-ttu-id="6ea9a-177">檢閱授權條款，然後如果您同意這些條款，按一下**我接受**。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-177">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="6ea9a-178">安裝完成時，按一下**完成**，然後關閉**Web Platform Installer 3.0**視窗。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-178">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

> [!NOTE]
> <span data-ttu-id="6ea9a-179">如果您的部署程序包含使用 VSDBCMD.exe 或 SQLCMD.exe 等工具，您必須確保這些都會安裝在您的組建伺服器上。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-179">If your deployment process includes the use of tools like VSDBCMD.exe or SQLCMD.exe, you'll need to ensure that these are installed on your build server.</span></span> <span data-ttu-id="6ea9a-180">VSDBCMD.exe 是 Visual Studio 工具，而且通常會加入至伺服器時安裝 Team Foundation Build。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-180">VSDBCMD.exe is a Visual Studio tool and is typically added to the server when you install Team Foundation Build.</span></span> <span data-ttu-id="6ea9a-181">SQLCMD.exe 是 SQL Server 工具。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-181">SQLCMD.exe is a SQL Server tool.</span></span> <span data-ttu-id="6ea9a-182">您可以下載獨立版本從 SQLCMD.exe [Microsoft SQL Server 2008 R2 功能套件](https://go.microsoft.com/?linkid=9805134)頁面。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-182">You can download a stand-alone version of SQLCMD.exe from the [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) page.</span></span>


## <a name="conclusion"></a><span data-ttu-id="6ea9a-183">結論</span><span class="sxs-lookup"><span data-stu-id="6ea9a-183">Conclusion</span></span>

<span data-ttu-id="6ea9a-184">此時，您的組建伺服器已準備好開始建置和部署您的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-184">At this point, your build server is ready to start building and deploying your web application projects.</span></span> <span data-ttu-id="6ea9a-185">下一個主題中，[建立組建定義，支援部署](creating-a-build-definition-that-supports-deployment.md)，說明如何建立和設定組建定義，以控制何時以及如何建置及部署您的專案。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-185">The next topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md), describes how to create and configure a build definition to control when and how your projects are built and deployed.</span></span>

## <a name="further-reading"></a><span data-ttu-id="6ea9a-186">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="6ea9a-186">Further Reading</span></span>

<span data-ttu-id="6ea9a-187">更多一般指引與 Team Build 工作的詳細資訊，請參閱[管理 Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6ea9a-187">For more general guidance on working with Team Build, see [Administering Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6ea9a-188">[上一頁](adding-content-to-source-control.md)
> [下一頁](creating-a-build-definition-that-supports-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="6ea9a-188">[Previous](adding-content-to-source-control.md)
[Next](creating-a-build-definition-that-supports-deployment.md)</span></span>
