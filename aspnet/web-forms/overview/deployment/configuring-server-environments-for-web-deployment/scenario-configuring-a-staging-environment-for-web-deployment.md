---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 案例： 設定預備環境，用於 Web 部署 |Microsoft 文件
author: jrjlee
description: 本主題描述預備環境的一般 web 部署案例，並說明您需要完成，才能設定類似 env 工作...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3864559b0599091beeacb87e90e80a51285039df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a><span data-ttu-id="e4ecf-103">案例： 設定用於 Web 部署的預備環境</span><span class="sxs-lookup"><span data-stu-id="e4ecf-103">Scenario: Configuring a Staging Environment for Web Deployment</span></span>
====================
<span data-ttu-id="e4ecf-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e4ecf-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e4ecf-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="e4ecf-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e4ecf-106">本主題描述預備環境的一般 web 部署案例，並說明您必須完成才能相似的環境所設定的工作。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-106">This topic describes a typical web deployment scenario for a staging environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="e4ecf-107">許多組織會使用預備環境來預覽更新 web 應用程式或網站。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-107">Lots of organizations use staging environments to preview updates to web applications or websites.</span></span> <span data-ttu-id="e4ecf-108">這可讓組織內的人員有機會瀏覽和站台 」 變成作用中 」 或換句話說部署到生產環境之前，檢閱新功能或內容。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-108">This gives people within the organization a chance to explore and review new functionality or content before the site "goes live," or in other words is deployed to a production environment.</span></span> <span data-ttu-id="e4ecf-109">預備環境被為了盡可能，以便提供實際的預覽複寫實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-109">The staging environment is designed to replicate the production environment as closely as possible, in order to provide a realistic preview.</span></span> <span data-ttu-id="e4ecf-110">這種執行環境通常具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="e4ecf-110">This kind of staging environment typically has these characteristics:</span></span>

- <span data-ttu-id="e4ecf-111">環境是由多個負載平衡 web 伺服器和一或多個資料庫伺服器，通常搭配容錯移轉叢集和鏡像資料庫所組成。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="e4ecf-112">由開發小組或自動由 Team Build 的伺服器應用程式可能以手動方式部署。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-112">Applications may be deployed manually by a development team or automatically by a Team Build server.</span></span>
- <span data-ttu-id="e4ecf-113">部署應用程式的處理序帳戶的使用者不太可能在臨時伺服器上具有系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-113">The users or process accounts that deploy applications are unlikely to have administrator privileges on the staging servers.</span></span>
- <span data-ttu-id="e4ecf-114">對應用程式部署頻繁地，所以環境必須支援單一步驟或自動部署。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-114">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ecf-115">擴充資料庫部署到多部伺服器已超出本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-115">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="e4ecf-116">如需有關此區域的詳細資訊，請參閱[SQL Server 線上叢書 》](https://technet.microsoft.com/library/ms130214.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-116">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="e4ecf-117">例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Foundation Server (TFS) 管理連絡人管理員解決方案。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-117">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) manages the Contact Manager solution.</span></span> <span data-ttu-id="e4ecf-118">TFS 系統管理員，Rob 郭已經建立可讓開發人員觸發部署到預備環境所需的組建定義。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-118">The TFS administrator, Rob Walters, has created a build definition that lets developers trigger a deployment to the staging environment as required.</span></span>

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="e4ecf-119">請注意，在大部分情況下，您不一定要將最新組建部署到預備環境。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-119">Note that in most cases, you won't necessarily want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="e4ecf-120">相反地，您有更多可能想要部署特定的組建已經經過驗證和測試環境中的驗證。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-120">Instead, you're a lot more likely to want to deploy a specific build that has already undergone validation and verification in the test environment.</span></span>

## <a name="solution-overview"></a><span data-ttu-id="e4ecf-121">解決方案概觀</span><span class="sxs-lookup"><span data-stu-id="e4ecf-121">Solution Overview</span></span>

<span data-ttu-id="e4ecf-122">在此案例中，您可以減少從部署需求的分析這些事實：</span><span class="sxs-lookup"><span data-stu-id="e4ecf-122">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="e4ecf-123">執行部署的使用者或處理序帳戶將不會有管理員權限臨時伺服器，因此臨時 web 伺服器必須支援非系統管理員部署。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-123">The user or process account that performs the deployment won't have administrator privileges on the staging servers, so the staging web servers must support non-administrator deployment.</span></span> <span data-ttu-id="e4ecf-124">因此，您必須設定臨時 web 伺服器來使用 Web 部署處理常式，而不是遠端代理程式。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-124">As such, you'll need to configure the staging web servers to use the Web Deploy Handler rather than the remote agent.</span></span>
- <span data-ttu-id="e4ecf-125">預備環境包含多個 web 伺服器，但必須支援一種單鍵或自動化部署，因此您必須要用來建立伺服器陣列的 Web 伺服陣列架構 (WFF)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-125">The staging environment includes multiple web servers, but it needs to support one-click or automated deployment, so you'll need to use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="e4ecf-126">使用這個方法，您可以部署一個網頁伺服器 （主要伺服器），應用程式，而且 WFF 會複寫所有其他 web 伺服器上的預備環境中部署。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-126">Using this approach, you can deploy an application to one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the staging environment.</span></span>
- <span data-ttu-id="e4ecf-127">使用者或執行部署的處理序帳戶必須具有建立資料庫的權限。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-127">The user or process account that performs the deployment must have permissions to create databases.</span></span> <span data-ttu-id="e4ecf-128">因此，您必須將帳戶加入**dbcreator**資料庫伺服器，除了設定支援遠端存取和部署資料庫伺服器上的伺服器角色。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-128">As such, you'll need to add the account to the **dbcreator** server role on the database server, in addition to configuring the database server to support remote access and deployment.</span></span>

<span data-ttu-id="e4ecf-129">這些主題提供您需要才能完成這些工作的所有資訊：</span><span class="sxs-lookup"><span data-stu-id="e4ecf-129">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="e4ecf-130">[建立伺服器陣列與 Web 伺服陣列架構](creating-a-server-farm-with-the-web-farm-framework.md)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-130">[Create a Server Farm with the Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md).</span></span> <span data-ttu-id="e4ecf-131">本主題描述如何建立及設定伺服器陣列使用 WFF，以便跨多個負載平衡 web 伺服器複寫的 web platform 產品和元件、 組態設定和網站和應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-131">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="e4ecf-132">[設定 Web Deploy 發行變更網頁伺服器 (Web Deploy 處理常式)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-132">[Configure a Web Server for Web Deploy Publishing (Web Deploy Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span> <span data-ttu-id="e4ecf-133">本主題描述如何建立支援 Web Deploy 發行，使用遠端代理程式的方法，從乾淨的 Windows Server 2008 R2 組建開始的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-133">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="e4ecf-134">[設定資料庫伺服器的 Web Deploy 發行變更](configuring-a-database-server-for-web-deploy-publishing.md)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-134">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="e4ecf-135">本主題描述如何設定資料庫伺服器以支援遠端存取，以及部署中，從預設安裝的 SQL Server 2008 R2 開始。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-135">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="e4ecf-136">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="e4ecf-136">Further Reading</span></span>

<span data-ttu-id="e4ecf-137">如需設定的一般開發人員測試環境的指引，請參閱[案例： 在測試環境設定用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-137">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="e4ecf-138">如需設定的標準生產環境的指引，請參閱[案例： 實際執行環境中設定用於 Web 部署](scenario-configuring-a-production-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="e4ecf-138">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4ecf-139">[上一頁](scenario-configuring-a-test-environment-for-web-deployment.md)
> [下一頁](scenario-configuring-a-production-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="e4ecf-139">[Previous](scenario-configuring-a-test-environment-for-web-deployment.md)
[Next](scenario-configuring-a-production-environment-for-web-deployment.md)</span></span>
