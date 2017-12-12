---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: "案例： 設定的生產環境 Web 部署 |Microsoft 文件"
author: jrjlee
description: "本主題說明在實際執行環境的一般 web 部署案例，並說明您必須完成才能設定類似的工作..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d5574ee353ff41205e9029e4aa5d139a5aa0e959
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a><span data-ttu-id="84663-103">案例： 設定用於 Web 部署的實際執行環境</span><span class="sxs-lookup"><span data-stu-id="84663-103">Scenario: Configuring a Production Environment for Web Deployment</span></span>
====================
<span data-ttu-id="84663-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="84663-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="84663-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="84663-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="84663-106">本主題說明在實際執行環境的一般 web 部署案例，並說明您必須完成才能相似的環境所設定的工作。</span><span class="sxs-lookup"><span data-stu-id="84663-106">This topic describes a typical web deployment scenario for a production environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="84663-107">在實際執行環境是針對 web 應用程式或網站的最終目的地。</span><span class="sxs-lookup"><span data-stu-id="84663-107">The production environment is the final destination for a web application or a website.</span></span> <span data-ttu-id="84663-108">此時，您的應用程式已透過測試、 已部署至預備環境，並準備好"上線。 」</span><span class="sxs-lookup"><span data-stu-id="84663-108">By this point, your application has been through testing, has been deployed to a staging environment, and is ready to "go live."</span></span> <span data-ttu-id="84663-109">在實際執行環境的特性可能會依廣泛的本質和 web 內容的目的，您的組織，您的目標對象和許多其他因素的大小。</span><span class="sxs-lookup"><span data-stu-id="84663-109">The characteristics of a production environment can vary widely according to the nature and purpose of your web content, the size of your organization, your target audience, and lots of other factors.</span></span> <span data-ttu-id="84663-110">在企業規模案例中，實際執行環境可能具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="84663-110">In an enterprise-scale scenario, the production environment may have these characteristics:</span></span>

- <span data-ttu-id="84663-111">環境是由多個負載平衡 web 伺服器和一或多個資料庫伺服器，通常搭配容錯移轉叢集和鏡像資料庫所組成。</span><span class="sxs-lookup"><span data-stu-id="84663-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="84663-112">如果環境是網際網路對向，很可能會從您的內部網路隔離。</span><span class="sxs-lookup"><span data-stu-id="84663-112">If the environment is Internet-facing, it's likely to be segregated from your internal network.</span></span> <span data-ttu-id="84663-113">也可能位於周邊網路中不同的子網路上，可能在不同網域中，並可能完全不同的網路基礎結構上。</span><span class="sxs-lookup"><span data-stu-id="84663-113">It may be on a different subnet in a perimeter network, it may be on a different domain, and it may be on an entirely different network infrastructure.</span></span>
- <span data-ttu-id="84663-114">開發人員和組建伺服器處理序帳戶是不太可能會在實際執行伺服器上具有系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="84663-114">Developers and build server process accounts are highly unlikely to have administrator privileges on the production servers.</span></span>
- <span data-ttu-id="84663-115">對應用程式會部署較不頻繁地比測試或預備環境部署。</span><span class="sxs-lookup"><span data-stu-id="84663-115">Changes to applications are deployed on a less frequent basis than test or staging deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="84663-116">擴充資料庫部署到多部伺服器已超出本教學課程的範圍。</span><span class="sxs-lookup"><span data-stu-id="84663-116">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="84663-117">如需有關此區域的詳細資訊，請參閱[SQL Server 線上叢書 》](https://technet.microsoft.com/en-us/library/ms130214.aspx)。</span><span class="sxs-lookup"><span data-stu-id="84663-117">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/en-us/library/ms130214.aspx).</span></span>


<span data-ttu-id="84663-118">例如，在我們[教學課程案例](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)，Team Build server 包含讓使用者建立連絡人管理員解決方案，並將它部署到預備環境中單一步驟的組建定義。</span><span class="sxs-lookup"><span data-stu-id="84663-118">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), a Team Build server includes build definitions that let users build the Contact Manager solution and deploy it to a staging environment in a single step.</span></span> <span data-ttu-id="84663-119">當應用程式隨時可供部署至生產環境，因為安全性需求和網路基礎結構中，所加諸的條件約束的實際執行環境的系統管理員必須手動複製到實際執行 web 伺服器上的 web 套件並匯入它透過網際網路資訊服務 (IIS) 管理員。</span><span class="sxs-lookup"><span data-stu-id="84663-119">When the application is ready to be deployed to production, due to the constraints imposed by security requirements and the network infrastructure, the production environment administrator must manually copy the web package onto a production web server and import it through Internet Information Services (IIS) Manager.</span></span>

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a><span data-ttu-id="84663-120">解決方案概觀</span><span class="sxs-lookup"><span data-stu-id="84663-120">Solution Overview</span></span>

<span data-ttu-id="84663-121">在此案例中，您可以減少從部署需求的分析這些事實：</span><span class="sxs-lookup"><span data-stu-id="84663-121">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="84663-122">由於安全性限制和網路設定，您無法設定以支援單鍵或自動部署實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="84663-122">Due to security restrictions and the network configuration, you can't configure the production environment to support one-click or automated deployment.</span></span> <span data-ttu-id="84663-123">離線的部署是唯一可行的方法，在此案例中。</span><span class="sxs-lookup"><span data-stu-id="84663-123">Offline deployment is the only viable approach in this scenario.</span></span>
- <span data-ttu-id="84663-124">實際執行環境含有多部網頁伺服器，因此您可以使用 Web 伺服陣列架構 (WFF) 來建立伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="84663-124">The production environment includes multiple web servers, so you can use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="84663-125">使用這個方法，系統管理員只需要一個 web 伺服器 （主要伺服器），應用程式匯入和 WFF 會複寫所有其他 web 伺服器上實際執行環境中部署。</span><span class="sxs-lookup"><span data-stu-id="84663-125">Using this approach, the administrator only needs to import the application onto one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the production environment.</span></span>

<span data-ttu-id="84663-126">這些主題提供您需要才能完成這些工作的所有資訊：</span><span class="sxs-lookup"><span data-stu-id="84663-126">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="84663-127">[建立伺服器陣列與 Web 伺服陣列架構](configuring-a-database-server-for-web-deploy-publishing.md)。</span><span class="sxs-lookup"><span data-stu-id="84663-127">[Create a Server Farm with the Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="84663-128">本主題描述如何建立及設定伺服器陣列使用 WFF，以便跨多個負載平衡 web 伺服器複寫的 web platform 產品和元件、 組態設定和網站和應用程式。</span><span class="sxs-lookup"><span data-stu-id="84663-128">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="84663-129">[設定 Web 伺服器的 Web Deploy 發行 （離線部署）](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="84663-129">[Configure a Web Server for Web Deploy Publishing (Offline Deployment)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="84663-130">本主題描述如何建置網頁伺服器，可讓系統管理員匯入和部署 web 封裝，手動從乾淨的 Windows Server 2008 R2 組建開始。</span><span class="sxs-lookup"><span data-stu-id="84663-130">This topic describes how to build a web server that lets administrators import and deploy web packages manually, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="84663-131">[設定資料庫伺服器的 Web Deploy 發行變更](configuring-a-database-server-for-web-deploy-publishing.md)。</span><span class="sxs-lookup"><span data-stu-id="84663-131">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="84663-132">本主題描述如何設定資料庫伺服器以支援遠端存取，以及部署中，從預設安裝的 SQL Server 2008 R2 開始。</span><span class="sxs-lookup"><span data-stu-id="84663-132">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="84663-133">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="84663-133">Further Reading</span></span>

<span data-ttu-id="84663-134">如需設定的一般開發人員測試環境的指引，請參閱[案例： 在測試環境設定用於 Web 部署](scenario-configuring-a-test-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="84663-134">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="84663-135">如需設定一般的預備環境的指引，請參閱[案例： 設定用於 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="84663-135">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="84663-136">[上一頁](scenario-configuring-a-staging-environment-for-web-deployment.md)
[下一頁](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span><span class="sxs-lookup"><span data-stu-id="84663-136">[Previous](scenario-configuring-a-staging-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span></span>
