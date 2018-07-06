---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 連絡管理員解決方案 |Microsoft Docs
author: jrjlee
description: 這一系列的教學課程使用範例解決方案&#x2014;連絡管理員解決方案&#x2014;來代表實際的層級的企業級應用程式...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: c8044c37738e9d23ca83648a6b571059338dc1a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835155"
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="82ab7-103">連絡管理員解決方案</span><span class="sxs-lookup"><span data-stu-id="82ab7-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="82ab7-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="82ab7-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="82ab7-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="82ab7-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="82ab7-106">這[系列的教學課程](web-deployment-in-the-enterprise.md)範例解決方案會使用&#x2014;連絡管理員解決方案&#x2014;來代表實際的複雜程度的企業級應用程式。</span><span class="sxs-lookup"><span data-stu-id="82ab7-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="82ab7-107">本主題介紹連絡管理員解決方案、 將告訴您方案的重要元件，並識別部署這類企業環境中的各種目標平台的應用程式的挑戰。</span><span class="sxs-lookup"><span data-stu-id="82ab7-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="82ab7-108">當您完成主題在這些教學課程中，您可以使用連絡管理員解決方案做為示範如何滿足特定的挑戰，在企業部署案例中的參考實作。</span><span class="sxs-lookup"><span data-stu-id="82ab7-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="82ab7-109">下一個主題中，[設定註冊連絡管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在您的開發人員工作站上執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="82ab7-110">解決方案概觀</span><span class="sxs-lookup"><span data-stu-id="82ab7-110">Solution Overview</span></span>

<span data-ttu-id="82ab7-111">連絡管理員解決方案包含四個個別專案：</span><span class="sxs-lookup"><span data-stu-id="82ab7-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="82ab7-112">**ContactManager.Mvc**。</span><span class="sxs-lookup"><span data-stu-id="82ab7-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="82ab7-113">這是 ASP.NET MVC 3 web 應用程式專案，表示方案的進入點。</span><span class="sxs-lookup"><span data-stu-id="82ab7-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="82ab7-114">它提供一些基本的 web 應用程式功能，例如讓使用者能夠建立及檢視連絡人詳細資料。</span><span class="sxs-lookup"><span data-stu-id="82ab7-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="82ab7-115">應用程式依賴 Windows Communication Foundation (WCF) 服務來管理連絡人和 ASP.NET 應用程式服務資料庫來管理驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="82ab7-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="82ab7-116">**ContactManager.Database**。</span><span class="sxs-lookup"><span data-stu-id="82ab7-116">**ContactManager.Database**.</span></span> <span data-ttu-id="82ab7-117">這是 Visual Studio 資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="82ab7-118">專案定義商店連絡人詳細資料的資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="82ab7-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="82ab7-119">**ContactManager.Service**。</span><span class="sxs-lookup"><span data-stu-id="82ab7-119">**ContactManager.Service**.</span></span> <span data-ttu-id="82ab7-120">這是 WCF web 服務專案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-120">This is a WCF web service project.</span></span> <span data-ttu-id="82ab7-121">WCF 服務所公開的端點，可讓呼叫端執行建立、 擷取、 更新和刪除 (CRUD) 作業上**ContactManager**資料庫。</span><span class="sxs-lookup"><span data-stu-id="82ab7-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="82ab7-122">服務會仰賴**ContactManager**資料庫並**ContactManager.Common.dll**組件。</span><span class="sxs-lookup"><span data-stu-id="82ab7-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="82ab7-123">**ContactManager.Common**。</span><span class="sxs-lookup"><span data-stu-id="82ab7-123">**ContactManager.Common**.</span></span> <span data-ttu-id="82ab7-124">這是類別庫專案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-124">This is a class library project.</span></span> <span data-ttu-id="82ab7-125">WCF 服務依賴這個組件中定義的型別。</span><span class="sxs-lookup"><span data-stu-id="82ab7-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="82ab7-126">解決方案也會包含名為發行的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="82ab7-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="82ab7-127">這包含各種自訂的專案檔和示範如何控制和管理組建和部署處理序的命令檔。</span><span class="sxs-lookup"><span data-stu-id="82ab7-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="82ab7-128">這些是稍後在本教學課程涵蓋更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="82ab7-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="82ab7-129">就概念而言，解決方案的元件彼此搭配運作就像這樣：</span><span class="sxs-lookup"><span data-stu-id="82ab7-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="82ab7-130">雖然 ASP.NET MVC 3 web 應用程式使用 ASP.NET 成員資格提供者，但在 web 應用程式中的所有頁面都允許匿名存取。</span><span class="sxs-lookup"><span data-stu-id="82ab7-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="82ab7-131">這顯然是不實際的組態。</span><span class="sxs-lookup"><span data-stu-id="82ab7-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="82ab7-132">不過，解決方案是設定在這種方式，讓您更輕鬆地部署和測試解決方案，但未設定使用者帳戶和角色。</span><span class="sxs-lookup"><span data-stu-id="82ab7-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="82ab7-133">部署挑戰</span><span class="sxs-lookup"><span data-stu-id="82ab7-133">Deployment Challenges</span></span>

<span data-ttu-id="82ab7-134">連絡管理員解決方案說明通用於許多企業部署案例的幾項部署挑戰：</span><span class="sxs-lookup"><span data-stu-id="82ab7-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="82ab7-135">解決方案包含多個相依專案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="82ab7-136">您要同時部署這些專案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="82ab7-137">連接字串和服務端點需要更新每個環境，並在許多情況下這項資訊將無法供開發人員。</span><span class="sxs-lookup"><span data-stu-id="82ab7-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="82ab7-138">當您部署**ContactManager**資料庫來預備和生產環境中，您需要保留現有的資料，在後續的部署。</span><span class="sxs-lookup"><span data-stu-id="82ab7-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="82ab7-139">當您部署 ASP.NET 應用程式服務資料庫時，您需要部署某些組態資料，但省略任何使用者帳戶資料。</span><span class="sxs-lookup"><span data-stu-id="82ab7-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="82ab7-140">專案會包含某些檔案和不應部署的資料夾。</span><span class="sxs-lookup"><span data-stu-id="82ab7-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="82ab7-141">您需要在部署程序中排除這些檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="82ab7-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="82ab7-142">解決方案必須支援從 Team Foundation Server (TFS) 組建伺服器自動化的部署。</span><span class="sxs-lookup"><span data-stu-id="82ab7-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="82ab7-143">結論</span><span class="sxs-lookup"><span data-stu-id="82ab7-143">Conclusion</span></span>

<span data-ttu-id="82ab7-144">本主題提供連絡管理員解決方案的高階概觀，並識別一些通用於許多企業部署案例的固有的部署挑戰。</span><span class="sxs-lookup"><span data-stu-id="82ab7-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="82ab7-145">在本教學課程的其餘主題將描述的一些技術可用來克服這些挑戰。</span><span class="sxs-lookup"><span data-stu-id="82ab7-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="82ab7-146">下一個主題中，[設定註冊連絡管理員解決方案](setting-up-the-contact-manager-solution.md)，說明如何下載並在您的開發人員工作站上執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="82ab7-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="82ab7-147">[上一頁](web-deployment-in-the-enterprise.md)
> [下一頁](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="82ab7-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
