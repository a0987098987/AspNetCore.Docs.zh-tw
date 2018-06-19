---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 請連絡管理員解決方案 |Microsoft 文件
author: jrjlee
description: 這一系列的教學課程使用範例方案&#x2014;連絡人管理員解決方案&#x2014;來表示實際 leve 的企業規模應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883696"
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="5002f-103">請連絡管理員解決方案</span><span class="sxs-lookup"><span data-stu-id="5002f-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="5002f-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="5002f-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="5002f-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="5002f-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="5002f-106">這[系列的教學課程](web-deployment-in-the-enterprise.md)會使用範例方案&#x2014;連絡人管理員解決方案&#x2014;來代表企業規模應用程式與實際的層級的複雜性。</span><span class="sxs-lookup"><span data-stu-id="5002f-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="5002f-107">本主題導入了連絡人管理員解決方案、 描述方案的重要元件，並識別部署這類企業環境中的各種目標平台的應用程式的挑戰。</span><span class="sxs-lookup"><span data-stu-id="5002f-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="5002f-108">當您完成主題這些教學課程中，您可以使用連絡人管理員解決方案做為示範如何滿足特定的挑戰，在企業部署案例中參考實作。</span><span class="sxs-lookup"><span data-stu-id="5002f-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="5002f-109">下一個主題[設定總連絡人管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在開發人員工作站上執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="5002f-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="5002f-110">解決方案概觀</span><span class="sxs-lookup"><span data-stu-id="5002f-110">Solution Overview</span></span>

<span data-ttu-id="5002f-111">連絡管理員方案包含四個個別專案：</span><span class="sxs-lookup"><span data-stu-id="5002f-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="5002f-112">**ContactManager.Mvc**.</span><span class="sxs-lookup"><span data-stu-id="5002f-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="5002f-113">這是 ASP.NET MVC 3 web 應用程式專案，表示方案的進入點。</span><span class="sxs-lookup"><span data-stu-id="5002f-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="5002f-114">它提供一些基本 web 應用程式功能，例如使用者提供的功能，以建立及檢視連絡人詳細資料。</span><span class="sxs-lookup"><span data-stu-id="5002f-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="5002f-115">應用程式依賴 Windows Communication Foundation (WCF) 服務來管理連絡人及管理驗證和授權的 ASP.NET 應用程式服務資料庫。</span><span class="sxs-lookup"><span data-stu-id="5002f-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="5002f-116">**ContactManager.Database**.</span><span class="sxs-lookup"><span data-stu-id="5002f-116">**ContactManager.Database**.</span></span> <span data-ttu-id="5002f-117">這是 Visual Studio 資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="5002f-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="5002f-118">專案定義的商店連絡人詳細資料的資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="5002f-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="5002f-119">**ContactManager.Service**.</span><span class="sxs-lookup"><span data-stu-id="5002f-119">**ContactManager.Service**.</span></span> <span data-ttu-id="5002f-120">這是 WCF web 服務專案。</span><span class="sxs-lookup"><span data-stu-id="5002f-120">This is a WCF web service project.</span></span> <span data-ttu-id="5002f-121">WCF 服務會公開端點，可讓呼叫端執行建立、 擷取、 更新和刪除 (CRUD) 作業上**ContactManager**資料庫。</span><span class="sxs-lookup"><span data-stu-id="5002f-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="5002f-122">服務會仰賴**ContactManager**資料庫和**ContactManager.Common.dll**組件。</span><span class="sxs-lookup"><span data-stu-id="5002f-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="5002f-123">**ContactManager.Common**.</span><span class="sxs-lookup"><span data-stu-id="5002f-123">**ContactManager.Common**.</span></span> <span data-ttu-id="5002f-124">這是類別庫專案。</span><span class="sxs-lookup"><span data-stu-id="5002f-124">This is a class library project.</span></span> <span data-ttu-id="5002f-125">WCF 服務依賴這個組件中定義的型別。</span><span class="sxs-lookup"><span data-stu-id="5002f-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="5002f-126">解決方案也會包含名為發行的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="5002f-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="5002f-127">這包含各種自訂專案檔和示範如何控制和管理組建與部署處理序的命令檔。</span><span class="sxs-lookup"><span data-stu-id="5002f-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="5002f-128">這些是稍後在本教學課程涵蓋更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="5002f-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="5002f-129">在概念層級，解決方案的元件搭配像這樣：</span><span class="sxs-lookup"><span data-stu-id="5002f-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="5002f-130">ASP.NET MVC 3 web 應用程式使用 ASP.NET 成員資格提供者，而 web 應用程式內的所有頁面就會都允許匿名存取。</span><span class="sxs-lookup"><span data-stu-id="5002f-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="5002f-131">這顯然不是實際的組態。</span><span class="sxs-lookup"><span data-stu-id="5002f-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="5002f-132">但是，方案是設定在這種方式，讓您更輕鬆地部署和測試解決方案，而不需設定使用者帳戶和角色。</span><span class="sxs-lookup"><span data-stu-id="5002f-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="5002f-133">部署的挑戰</span><span class="sxs-lookup"><span data-stu-id="5002f-133">Deployment Challenges</span></span>

<span data-ttu-id="5002f-134">請連絡管理員解決方案說明企業部署案例的許多常見的幾個部署挑戰：</span><span class="sxs-lookup"><span data-stu-id="5002f-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="5002f-135">方案包含多個相依專案。</span><span class="sxs-lookup"><span data-stu-id="5002f-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="5002f-136">您必須同時部署這些專案。</span><span class="sxs-lookup"><span data-stu-id="5002f-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="5002f-137">連接字串和服務端點需要更新每個環境中，而且在許多情況下這項資訊將無法供開發人員。</span><span class="sxs-lookup"><span data-stu-id="5002f-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="5002f-138">當您部署**ContactManager**資料庫預備和生產環境，您需要保留現有的資料，後續部署。</span><span class="sxs-lookup"><span data-stu-id="5002f-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="5002f-139">當您部署 ASP.NET 應用程式服務資料庫時，您需要部署某些組態資料，但略過任何使用者帳戶的資料。</span><span class="sxs-lookup"><span data-stu-id="5002f-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="5002f-140">專案包含某些檔案和不應部署的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5002f-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="5002f-141">您需要這些檔案和資料夾排除部署程序。</span><span class="sxs-lookup"><span data-stu-id="5002f-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="5002f-142">解決方案必須支援從 Team Foundation Server (TFS) 組建伺服器的自動的部署。</span><span class="sxs-lookup"><span data-stu-id="5002f-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5002f-143">結論</span><span class="sxs-lookup"><span data-stu-id="5002f-143">Conclusion</span></span>

<span data-ttu-id="5002f-144">本主題提供連絡人管理員解決方案的高階概觀，並識別通用於許多企業部署案例的固有部署挑戰。</span><span class="sxs-lookup"><span data-stu-id="5002f-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="5002f-145">本教學課程中的其他主題會說明一些您可以使用為了克服這些挑戰的技術。</span><span class="sxs-lookup"><span data-stu-id="5002f-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="5002f-146">下一個主題[設定總連絡人管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在開發人員工作站上執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="5002f-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5002f-147">[上一頁](web-deployment-in-the-enterprise.md)
> [下一頁](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="5002f-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
