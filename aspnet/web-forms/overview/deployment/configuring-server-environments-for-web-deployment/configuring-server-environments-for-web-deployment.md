---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: 設定伺服器環境，用於 Web 部署 |Microsoft 文件
author: jrjlee
description: 本教學課程顯示如何將伺服器環境以支援一種單鍵或自動化、 部署網站和各種不同的畫面中的發行設定...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892292"
---
<a name="configuring-server-environments-for-web-deployment"></a><span data-ttu-id="86381-103">用於 Web 部署設定伺服器環境</span><span class="sxs-lookup"><span data-stu-id="86381-103">Configuring Server Environments for Web Deployment</span></span>
====================
<span data-ttu-id="86381-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="86381-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="86381-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="86381-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="86381-106">本教學課程將說明如何設定伺服器環境以支援一種單鍵或自動化、 部署網站和各種不同案例中的發行。</span><span class="sxs-lookup"><span data-stu-id="86381-106">This tutorial will show you how to set up server environments to support one-click, or automated, website deployment and publishing in various different scenarios.</span></span> <span data-ttu-id="86381-107">本教學課程包含的主題，以引導您完成各種工作，例如設定 web 伺服器，以支援特定的方法，來部署和設定 Web 伺服陣列架構 (WFF) 伺服器的伺服器陣列，以及提供以案例為基礎的概觀較高層級的端對端指引。</span><span class="sxs-lookup"><span data-stu-id="86381-107">The tutorial includes topics to walk you through completing various tasks, like configuring a web server to support specific approaches to deployment and setting up a Web Farm Framework (WFF) server farm, together with scenario-based overviews that provide higher-level end-to-end guidance.</span></span>
> 
> <span data-ttu-id="86381-108">本教學課程會使用 Fabrikam，Inc.的部署案例中所述[企業 Web 部署： 案例概觀](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)作為範例和網路基礎結構的參考點。</span><span class="sxs-lookup"><span data-stu-id="86381-108">The tutorial uses the Fabrikam, Inc. deployment scenario described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) as a reference point for examples and network infrastructure.</span></span>
> 
> <span data-ttu-id="86381-109">這些教學課程的義大利文的翻譯，請瀏覽[ http://www.lucamorelli.it ](http://www.lucamorelli.it)。</span><span class="sxs-lookup"><span data-stu-id="86381-109">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="86381-110">本教學課程包含下列主題：</span><span class="sxs-lookup"><span data-stu-id="86381-110">This tutorial includes these topics:</span></span>

- [<span data-ttu-id="86381-111">選擇 Web 部署的正確方法</span><span class="sxs-lookup"><span data-stu-id="86381-111">Choosing the Right Approach to Web Deployment</span></span>](choosing-the-right-approach-to-web-deployment.md)
- [<span data-ttu-id="86381-112">案例：設定 Web 部署的測試環境</span><span class="sxs-lookup"><span data-stu-id="86381-112">Scenario: Configuring a Test Environment for Web Deployment</span></span>](scenario-configuring-a-test-environment-for-web-deployment.md)
- [<span data-ttu-id="86381-113">案例：設定 Web 部署的預備環境</span><span class="sxs-lookup"><span data-stu-id="86381-113">Scenario: Configuring a Staging Environment for Web Deployment</span></span>](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [<span data-ttu-id="86381-114">案例：設定 Web 部署的生產環境</span><span class="sxs-lookup"><span data-stu-id="86381-114">Scenario: Configuring a Production Environment for Web Deployment</span></span>](scenario-configuring-a-production-environment-for-web-deployment.md)
- [<span data-ttu-id="86381-115">設定 Web Deploy 發行的網頁伺服器 (遠端代理程式)</span><span class="sxs-lookup"><span data-stu-id="86381-115">Configuring a Web Server for Web Deploy Publishing (Remote Agent)</span></span>](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [<span data-ttu-id="86381-116">設定 Web Deploy 發行的網頁伺服器 (Web Deploy 處理常式)</span><span class="sxs-lookup"><span data-stu-id="86381-116">Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)</span></span>](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [<span data-ttu-id="86381-117">設定 Web Deploy 發行的網頁伺服器 (離線部署)</span><span class="sxs-lookup"><span data-stu-id="86381-117">Configuring a Web Server for Web Deploy Publishing (Offline Deployment)</span></span>](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [<span data-ttu-id="86381-118">設定 Web Deploy 發行的資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="86381-118">Configuring a Database Server for Web Deploy Publishing</span></span>](configuring-a-database-server-for-web-deploy-publishing.md)
- [<span data-ttu-id="86381-119">使用 Web 伺服陣列架構建立伺服器陣列</span><span class="sxs-lookup"><span data-stu-id="86381-119">Creating a Server Farm with the Web Farm Framework</span></span>](creating-a-server-farm-with-the-web-farm-framework.md)
- [<span data-ttu-id="86381-120">設定目標環境的部署屬性</span><span class="sxs-lookup"><span data-stu-id="86381-120">Configuring Deployment Properties for a Target Environment</span></span>](configuring-deployment-properties-for-a-target-environment.md)

<span data-ttu-id="86381-121">第一個主題，[選擇 Web 部署的權限方法](choosing-the-right-approach-to-web-deployment.md)，描述主要的方法可用於發行 web 應用程式使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 2.0。</span><span class="sxs-lookup"><span data-stu-id="86381-121">The first topic, [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md), describes the main approaches you can use to publish web applications by using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0.</span></span> <span data-ttu-id="86381-122">它也可以識別對應至每一種方法的案例。</span><span class="sxs-lookup"><span data-stu-id="86381-122">It also identifies the scenarios that map to each approach.</span></span> <span data-ttu-id="86381-123">從這裡開始，每個案例主題提供您需要完成的工作的高階概觀，並識別您將需要透過運作來協助您完成這些工作的主題。</span><span class="sxs-lookup"><span data-stu-id="86381-123">From here, each scenario topic provides a high-level overview of the tasks you need to complete and identifies the topics you'll need to work through to help you complete these tasks.</span></span>

<span data-ttu-id="86381-124">如果您使用分割專案檔案描述的方法中[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)建置和部署方案時，最後一個主題，[設定部署屬性的目標環境](configuring-deployment-properties-for-a-target-environment.md)，說明如何設定以部署至不同目的地環境的特定環境的專案檔。</span><span class="sxs-lookup"><span data-stu-id="86381-124">If you're using the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md) to build and deploy your solution, the final topic, [Configuring Deployment Properties for a Target Environment](configuring-deployment-properties-for-a-target-environment.md), describes how to configure environment-specific project files for deployment to different destination environments.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="86381-125">關鍵技術</span><span class="sxs-lookup"><span data-stu-id="86381-125">Key Technologies</span></span>

<span data-ttu-id="86381-126">本教學課程著重於如何使用這些產品和技術，支援 web 部署：</span><span class="sxs-lookup"><span data-stu-id="86381-126">This tutorial focuses on how to use these products and technologies to support web deployment:</span></span>

- <span data-ttu-id="86381-127">IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="86381-127">IIS 7.5</span></span>
- <span data-ttu-id="86381-128">Web Deploy 2.x</span><span class="sxs-lookup"><span data-stu-id="86381-128">Web Deploy 2.x</span></span>
- <span data-ttu-id="86381-129">WFF 2.x</span><span class="sxs-lookup"><span data-stu-id="86381-129">WFF 2.x</span></span>
- <span data-ttu-id="86381-130">IIS Web 管理服務 (WMSvc)</span><span class="sxs-lookup"><span data-stu-id="86381-130">IIS Web Management Service (WMSvc)</span></span>

<span data-ttu-id="86381-131">本教學課程也會牽涉到使用 Windows Server 2008 R2、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="86381-131">The tutorial also touches on the use of Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0, and ASP.NET MVC 3.</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="86381-132">在這一系列其他教學課程</span><span class="sxs-lookup"><span data-stu-id="86381-132">Other Tutorials in This Series</span></span>

<span data-ttu-id="86381-133">這會形成企業規模的 web 部署的一系列的五個教學課程的一部分。</span><span class="sxs-lookup"><span data-stu-id="86381-133">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="86381-134">這些是數列中的其他教學課程：</span><span class="sxs-lookup"><span data-stu-id="86381-134">These are the other tutorials in the series:</span></span>

- <span data-ttu-id="86381-135">[部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。</span><span class="sxs-lookup"><span data-stu-id="86381-135">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="86381-136">此入門內容提供教學課程系列內容的背景。</span><span class="sxs-lookup"><span data-stu-id="86381-136">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="86381-137">它描述教學課程案例中，並將說明如何工作與數列中所描述的逐步解說納入更廣泛的應用程式生命週期管理 (ALM) 程序。</span><span class="sxs-lookup"><span data-stu-id="86381-137">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="86381-138">[Web 部署，在企業中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。</span><span class="sxs-lookup"><span data-stu-id="86381-138">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="86381-139">本教學課程提供 Microsoft Build Engine (MSBuild) 專案檔的概念性簡介、 Web 發行管線、 Web Deploy，和其他相關的技術。</span><span class="sxs-lookup"><span data-stu-id="86381-139">This tutorial provides a conceptual introduction to Microsoft Build Engine (MSBuild) project files, the Web Publishing Pipeline, Web Deploy, and other related technologies.</span></span> <span data-ttu-id="86381-140">它說明如何使用這些工具一起管理複雜的部署處理程序。</span><span class="sxs-lookup"><span data-stu-id="86381-140">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="86381-141">[設定用於 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="86381-141">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="86381-142">本教學課程說明如何設定 Team Foundation Server (TFS) 支援各種部署案例，包括持續整合 (CI) 程序的一部分的自動的部署，以及手動觸發部署的特定的組建。</span><span class="sxs-lookup"><span data-stu-id="86381-142">This tutorial describes how to configure Team Foundation Server (TFS) to support various deployment scenarios, including automated deployment as part of a continuous integration (CI) process and manually triggered deployments of specific builds.</span></span>
- <span data-ttu-id="86381-143">[進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="86381-143">[Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md).</span></span> <span data-ttu-id="86381-144">本教學課程說明如何完成各種更進階的部署工作，例如自訂的多個環境的資料庫部署、 檔案和資料夾排除在部署中，和部署程序期間取得 web 應用程式離線.</span><span class="sxs-lookup"><span data-stu-id="86381-144">This tutorial describes how to accomplish various more advanced deployment tasks, like customizing database deployments for multiple environments, excluding files and folders from deployment, and taking web applications offline during the deployment process.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86381-145">下一步</span><span class="sxs-lookup"><span data-stu-id="86381-145">Next</span></span>](choosing-the-right-approach-to-web-deployment.md)
