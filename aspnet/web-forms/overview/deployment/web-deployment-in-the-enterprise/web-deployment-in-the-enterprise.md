---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: "Web 企業中的部署 |Microsoft 文件"
author: jrjlee
description: "本教學課程說明如何符合許多管理對內企業規模的 web 應用程式的部署時，就會發生的驗證題目..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 6210d01f65bcadf8ae4209e372d5aac68861bd7a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="web-deployment-in-the-enterprise"></a><span data-ttu-id="89b94-103">在企業中的 web 部署</span><span class="sxs-lookup"><span data-stu-id="89b94-103">Web Deployment in the Enterprise</span></span>
====================
<span data-ttu-id="89b94-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="89b94-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="89b94-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="89b94-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="89b94-106">本教學課程說明如何符合許多管理企業規模 web 應用程式部署至開發、 測試、 預備及生產環境時，您會遇到的挑戰。</span><span class="sxs-lookup"><span data-stu-id="89b94-106">This tutorial describes how to meet lots of the challenges you'll encounter when you manage the deployment of enterprise-scale web applications to development, test, staging, and production environments.</span></span> <span data-ttu-id="89b94-107">本教學課程包含參考方案以及概念和工作導向的內容，引導您完成各種常見的工作和程序的混合。</span><span class="sxs-lookup"><span data-stu-id="89b94-107">The tutorial includes a reference solution together with a mixture of conceptual and task-oriented content to guide you through various common tasks and procedures.</span></span>
> 
> <span data-ttu-id="89b94-108">這些教學課程的義大利文的翻譯，請瀏覽[http://www.lucamorelli.it](http://www.lucamorelli.it)。</span><span class="sxs-lookup"><span data-stu-id="89b94-108">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


## <a name="enterprise-deployment-challenges"></a><span data-ttu-id="89b94-109">企業部署挑戰</span><span class="sxs-lookup"><span data-stu-id="89b94-109">Enterprise Deployment Challenges</span></span>

<span data-ttu-id="89b94-110">它們看起來來管理複雜，企業級解決方案的部署時，組織通常會遇到這些挑戰：</span><span class="sxs-lookup"><span data-stu-id="89b94-110">Organizations often encounter these challenges when they look to manage the deployment of complex, enterprise-scale solutions:</span></span>

- <span data-ttu-id="89b94-111">您必須要能將專案部署到多個環境，例如開發人員或測試環境中，暫存平台和實際執行伺服器。</span><span class="sxs-lookup"><span data-stu-id="89b94-111">You need to be able to deploy projects to multiple environments, like developer or test environments, staging platforms, and production servers.</span></span> <span data-ttu-id="89b94-112">方案必須先部署每個環境的不同組態設定。</span><span class="sxs-lookup"><span data-stu-id="89b94-112">The solution needs to be deployed with different configuration settings for each environment.</span></span>
- <span data-ttu-id="89b94-113">您必須同時在單一步驟或自動化建置和部署程序的一部分部署多個相依專案。</span><span class="sxs-lookup"><span data-stu-id="89b94-113">You need to deploy multiple dependent projects simultaneously as part of a single-step or automated build and deployment process.</span></span>
- <span data-ttu-id="89b94-114">您需要能夠的磁碟機部署從自動化的程序。</span><span class="sxs-lookup"><span data-stu-id="89b94-114">You need to be able to drive deployment from an automated process.</span></span> <span data-ttu-id="89b94-115">例如，您要使用持續整合 (CI) 處理程序部署至測試環境的 web 應用程式，當新的程式碼簽入。</span><span class="sxs-lookup"><span data-stu-id="89b94-115">For example, you want to use a continuous integration (CI) process to deploy web applications to a test environment when new code is checked in.</span></span>
- <span data-ttu-id="89b94-116">您需要能夠控制部署程序以及設定部署變數，從 Visual Studio 外部，為開發人員不太可能會有正確的組態設定或針對每個目標環境所需的認證。</span><span class="sxs-lookup"><span data-stu-id="89b94-116">You need to be able to control the deployment process and set deployment variables from outside Visual Studio, as developers are unlikely to have the correct configuration settings or the necessary credentials for every target environment.</span></span>
- <span data-ttu-id="89b94-117">您需要部署結構描述為基礎的資料庫專案，並保留現有的資料，後續部署。</span><span class="sxs-lookup"><span data-stu-id="89b94-117">You need to deploy schema-based database projects and preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="89b94-118">您需要部署臨機操作為基礎的成員資格資料庫，而不需部署使用者帳戶資料。</span><span class="sxs-lookup"><span data-stu-id="89b94-118">You need to deploy membership databases on an ad hoc basis without deploying user account data.</span></span> <span data-ttu-id="89b94-119">此外，您可能也需要更新的已部署的成員資格資料庫結構描述，而不會遺失現有的使用者帳戶資料。</span><span class="sxs-lookup"><span data-stu-id="89b94-119">You may also need to update the schema of deployed membership databases without losing existing user account data.</span></span>
- <span data-ttu-id="89b94-120">您要排除特定檔案或資料夾，當您將內容部署至各種目標環境。</span><span class="sxs-lookup"><span data-stu-id="89b94-120">You need to exclude certain files or folders when you deploy content to various target environments.</span></span>

## <a name="overview-of-approach"></a><span data-ttu-id="89b94-121">方法的概觀</span><span class="sxs-lookup"><span data-stu-id="89b94-121">Overview of Approach</span></span>

<span data-ttu-id="89b94-122">此教學課程中，以及在這一系列，其他的教學課程會使用此高層級的方式，以符合上面所述的挑戰。</span><span class="sxs-lookup"><span data-stu-id="89b94-122">This tutorial, together with the other tutorials in this series, uses this high-level approach to meet the challenges described above.</span></span>

- <span data-ttu-id="89b94-123">**使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來控制整個建置和部署程序。**</span><span class="sxs-lookup"><span data-stu-id="89b94-123">**Use custom Microsoft Build Engine (MSBuild) project files to control the overall build and deployment process.**</span></span>
- <span data-ttu-id="89b94-124">這可讓您建置和部署方案中的每個專案做為單一、 可編寫指令碼作業的一部分。</span><span class="sxs-lookup"><span data-stu-id="89b94-124">This lets you build and deploy every project in the solution as part of a single, scriptable operation.</span></span>
- <span data-ttu-id="89b94-125">環境特定設定是使用簡單的特定環境的專案檔。</span><span class="sxs-lookup"><span data-stu-id="89b94-125">Environment-specific settings are configured using simple environment-specific project files.</span></span> <span data-ttu-id="89b94-126">相較於使用方案組態 Visual Studio – 以中心方法並將發行設定檔來設定不同的環境的部署，這種方法可讓您設定及管理從 Visual Studio 外部的部署程序。</span><span class="sxs-lookup"><span data-stu-id="89b94-126">In contrast to the Visual Studio–centric approach of using solution configurations and publish profiles to configure deployments for different environments, this approach lets you configure and manage the deployment process from outside Visual Studio.</span></span> <span data-ttu-id="89b94-127">這表示開發人員不需要進階的連接字串、 服務端點、 伺服器認證和其他的部署變數目的地環境的知識。</span><span class="sxs-lookup"><span data-stu-id="89b94-127">This means that developers don't need advance knowledge of connection strings, service endpoints, server credentials, and other deployment variables for destination environments.</span></span>
- <span data-ttu-id="89b94-128">Team Build Team Foundation Server (TFS) 工作流程可以叫用自訂的專案檔。</span><span class="sxs-lookup"><span data-stu-id="89b94-128">The custom project files can be invoked by Team Build as part of a Team Foundation Server (TFS) workflow.</span></span> <span data-ttu-id="89b94-129">這可讓您設定 CI 案例的自動化的部署。</span><span class="sxs-lookup"><span data-stu-id="89b94-129">This lets you configure automated deployment for CI scenarios.</span></span>

<span data-ttu-id="89b94-130">**使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 來封裝及部署 web 應用程式專案。**</span><span class="sxs-lookup"><span data-stu-id="89b94-130">**Use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to package and deploy web application projects.**</span></span>

- <span data-ttu-id="89b94-131">Web Deploy 提供一種架構，可讓您封裝及部署您的 web 應用程式內容到目的地 IIS 網頁伺服器，以及相依性、 組態設定、 安全性設定，以及任何其他需求。</span><span class="sxs-lookup"><span data-stu-id="89b94-131">Web Deploy provides a framework that lets you package and deploy your web application content to a destination IIS web server, together with dependencies, configuration settings, security settings, and any other requirements.</span></span>
- <span data-ttu-id="89b94-132">您可以控制整個的封裝和部署程序，從您自訂的 MSBuild 專案檔內。</span><span class="sxs-lookup"><span data-stu-id="89b94-132">You can control the entire packaging and deployment process from within your custom MSBuild project files.</span></span> <span data-ttu-id="89b94-133">您也可以使用隨附於您的 web 部署套件，例如連接字串、 服務端點和 IIS 目的詳細資料的組態設定。</span><span class="sxs-lookup"><span data-stu-id="89b94-133">You can also manipulate the configuration settings that accompany your web deployment package, like connection strings, service endpoints, and IIS destination details.</span></span>
- <span data-ttu-id="89b94-134">Web Deploy，Web 發行管線，以及提供許多可讓您自訂您的部署的擴充點。</span><span class="sxs-lookup"><span data-stu-id="89b94-134">Web Deploy, together with the Web Publishing Pipeline, offers lots of extensibility points that let you customize your deployments.</span></span> <span data-ttu-id="89b94-135">例如，很容易就能從 web 部署套件中排除不必要的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="89b94-135">For example, it's easy to exclude unwanted files and folders from your web deployment packages.</span></span>

<span data-ttu-id="89b94-136">**使用 VSDBCMD.exe 公用程式來部署和更新資料庫結構描述。**</span><span class="sxs-lookup"><span data-stu-id="89b94-136">**Use the VSDBCMD.exe utility to deploy and update database schemas.**</span></span>

- <span data-ttu-id="89b94-137">VSDBCMD 可讓您部署資料庫結構描述檔案 (.dbschema)，其中建置 Visual Studio 資料庫專案時所產生的資料庫。</span><span class="sxs-lookup"><span data-stu-id="89b94-137">VSDBCMD allows you to deploy databases from a database schema file (.dbschema), which is generated when you build a Visual Studio database project.</span></span> <span data-ttu-id="89b94-138">相反地，Web Deploy 所含的資料庫部署功能是更適合部署現有的資料庫從本機 SQL Server 執行個體。</span><span class="sxs-lookup"><span data-stu-id="89b94-138">In contrast, the database deployment functionality included in Web Deploy is more suited to deploying existing databases from a local SQL Server instance.</span></span>
- <span data-ttu-id="89b94-139">不同於部署資料庫專案的 Visual Studio 功能，VSDBCMD 可讓您將差異更新部署至現有的目標資料庫。</span><span class="sxs-lookup"><span data-stu-id="89b94-139">Unlike Visual Studio's functionality for deploying database projects, VSDBCMD lets you deploy differential updates to an existing target database.</span></span> <span data-ttu-id="89b94-140">這可讓您保留任何現有的資料，當您升級資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="89b94-140">This allows you to preserve any existing data while you upgrade the database schema.</span></span>
- <span data-ttu-id="89b94-141">您可以執行 VSDBCMD 命令，從您自訂的 MSBuild 專案檔內。</span><span class="sxs-lookup"><span data-stu-id="89b94-141">You can execute VSDBCMD commands from within your custom MSBuild project files.</span></span>

## <a name="content-map"></a><span data-ttu-id="89b94-142">內容對應</span><span class="sxs-lookup"><span data-stu-id="89b94-142">Content Map</span></span>

<span data-ttu-id="89b94-143">本教學課程涵蓋的主題可分為四個主要區域。</span><span class="sxs-lookup"><span data-stu-id="89b94-143">This tutorial includes topics that fall into four main areas.</span></span>

<span data-ttu-id="89b94-144">這些主題會介紹參考方案 & #x 2014年; 請連絡管理員解決方案 & #x 2014;，並說明如何下載並設定在本機電腦上：</span><span class="sxs-lookup"><span data-stu-id="89b94-144">These topics introduce the reference solution&#x2014;the Contact Manager solution&#x2014;and describe how to download it and configure it on your local machine:</span></span>

- [<span data-ttu-id="89b94-145">請連絡管理員解決方案</span><span class="sxs-lookup"><span data-stu-id="89b94-145">The Contact Manager Solution</span></span>](the-contact-manager-solution.md)
- [<span data-ttu-id="89b94-146">設定連絡人管理員解決方案</span><span class="sxs-lookup"><span data-stu-id="89b94-146">Setting Up the Contact Manager Solution</span></span>](setting-up-the-contact-manager-solution.md)

<span data-ttu-id="89b94-147">這些主題會介紹 MSBuild 專案檔，說明如何建立和使用自訂的專案檔，並逐步解說，請連絡管理員方案的部署程序：</span><span class="sxs-lookup"><span data-stu-id="89b94-147">These topics introduce MSBuild project files, describe how you can create and use custom project files, and walk through the deployment process for the Contact Manager solution:</span></span>

- [<span data-ttu-id="89b94-148">了解專案檔</span><span class="sxs-lookup"><span data-stu-id="89b94-148">Understanding the Project File</span></span>](understanding-the-project-file.md)
- [<span data-ttu-id="89b94-149">了解建置程序</span><span class="sxs-lookup"><span data-stu-id="89b94-149">Understanding the Build Process</span></span>](understanding-the-build-process.md)

<span data-ttu-id="89b94-150">這些主題會描述 web 應用程式部署，包括如何建置和封裝程序運作、 如何與 Web 發行管線整合建置流程、 如何修改部署的參數，以及如何將 web 套件部署到目的地環境：</span><span class="sxs-lookup"><span data-stu-id="89b94-150">These topics describe web application deployment, including how the build and packaging process works, how the build process integrates with the Web Publishing Pipeline, how to modify deployment parameters, and how to deploy web packages to destination environments:</span></span>

- [<span data-ttu-id="89b94-151">建立和封裝 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="89b94-151">Building and Packaging Web Application Projects</span></span>](building-and-packaging-web-application-projects.md)
- [<span data-ttu-id="89b94-152">用於 Web 套件部署中設定參數</span><span class="sxs-lookup"><span data-stu-id="89b94-152">Configuring Parameters for Web Package Deployment</span></span>](configuring-parameters-for-web-package-deployment.md)
- [<span data-ttu-id="89b94-153">部署 Web 封裝</span><span class="sxs-lookup"><span data-stu-id="89b94-153">Deploying Web Packages</span></span>](deploying-web-packages.md)

- <span data-ttu-id="89b94-154">[部署資料庫專案](deploying-database-projects.md)描述不同的技術，可用來部署 Visual Studio 資料庫專案，以及每一種方法的優缺點。</span><span class="sxs-lookup"><span data-stu-id="89b94-154">[Deploying Database Projects](deploying-database-projects.md) describes the different techniques you can use to deploy Visual Studio database projects, together with the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="89b94-155">[建立和執行部署指令檔](creating-and-running-a-deployment-command-file.md)說明如何建立簡單的命令檔封裝部署邏輯，可讓您將複雜的方案部署為單一步驟的程序。</span><span class="sxs-lookup"><span data-stu-id="89b94-155">[Creating and Running a Deployment Command File](creating-and-running-a-deployment-command-file.md) describes how to create a simple command file that encapsulates your deployment logic and lets you deploy complex solutions as a single-step process.</span></span>
- <span data-ttu-id="89b94-156">最後，[手動安裝 Web 封裝](manually-installing-web-packages.md)總結教學課程，以顯示您將 IIS web 封裝匯入。</span><span class="sxs-lookup"><span data-stu-id="89b94-156">Finally, [Manually Installing Web Packages](manually-installing-web-packages.md) concludes the tutorial by showing you to import web packages into IIS.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="89b94-157">關鍵技術</span><span class="sxs-lookup"><span data-stu-id="89b94-157">Key Technologies</span></span>

<span data-ttu-id="89b94-158">本教學課程中的主題主要會使用這些技術來管理組建與部署：</span><span class="sxs-lookup"><span data-stu-id="89b94-158">The topics in this tutorial primarily use these technologies to manage build and deployment:</span></span>

- <span data-ttu-id="89b94-159">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="89b94-159">Visual Studio 2010</span></span>
- <span data-ttu-id="89b94-160">MSBuild</span><span class="sxs-lookup"><span data-stu-id="89b94-160">MSBuild</span></span>
- <span data-ttu-id="89b94-161">IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="89b94-161">IIS 7.5</span></span>
- <span data-ttu-id="89b94-162">Web Deploy 2.0</span><span class="sxs-lookup"><span data-stu-id="89b94-162">Web Deploy 2.0</span></span>
- <span data-ttu-id="89b94-163">VSDBCMD.exe 資料庫部署公用程式</span><span class="sxs-lookup"><span data-stu-id="89b94-163">The VSDBCMD.exe database deployment utility</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="89b94-164">在這一系列其他教學課程</span><span class="sxs-lookup"><span data-stu-id="89b94-164">Other Tutorials in This Series</span></span>

<span data-ttu-id="89b94-165">這會形成企業規模的 web 部署的一系列的五個教學課程的一部分。</span><span class="sxs-lookup"><span data-stu-id="89b94-165">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="89b94-166">這些是數列中的其他教學課程：</span><span class="sxs-lookup"><span data-stu-id="89b94-166">These are the other tutorials in the series:</span></span>

- <span data-ttu-id="89b94-167">[部署企業案例中的 Web 應用程式](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。</span><span class="sxs-lookup"><span data-stu-id="89b94-167">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="89b94-168">此入門內容提供教學課程系列內容的背景。</span><span class="sxs-lookup"><span data-stu-id="89b94-168">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="89b94-169">它描述教學課程案例中，並將說明如何工作與數列中所描述的逐步解說納入更廣泛的應用程式生命週期管理 (ALM) 程序。</span><span class="sxs-lookup"><span data-stu-id="89b94-169">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="89b94-170">[用於 Web 部署設定伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="89b94-170">[Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span> <span data-ttu-id="89b94-171">本教學課程說明如何設定 Windows servers 以支援各種部署案例，包括使用 Web Deployment Agent Service （遠端代理程式） 或 Web 部署的處理常式和遠端資料庫部署的遠端 web 封裝部署。</span><span class="sxs-lookup"><span data-stu-id="89b94-171">This tutorial describes how to configure Windows servers to support various deployment scenarios, including remote web package deployment using the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler and remote database deployment.</span></span> <span data-ttu-id="89b94-172">它提供有關選擇您自己的環境中，適當的部署方法的指引，並說明如何使用伺服器陣列中的所有 web 伺服器之間複寫部署的 web 應用程式的 Web 伺服陣列架構 (WFF)。</span><span class="sxs-lookup"><span data-stu-id="89b94-172">It provides guidance on choosing the appropriate deployment method for your own environment, and it describes how to use the Web Farm Framework (WFF) to replicate deployed web applications across all the web servers in a server farm.</span></span>
- <span data-ttu-id="89b94-173">[設定用於 Web 部署的 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="89b94-173">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="89b94-174">本教學課程說明如何設定 TFS 以支援各種部署案例，包括自動化的部署 CI 程序的一部分，以及手動觸發部署的特定組建。</span><span class="sxs-lookup"><span data-stu-id="89b94-174">This tutorial describes how to configure TFS to support various deployment scenarios, including automated deployment as part of a CI process and manually triggered deployments of specific builds.</span></span>
- <span data-ttu-id="89b94-175">[進階企業 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="89b94-175">[Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md).</span></span> <span data-ttu-id="89b94-176">本教學課程說明如何完成各種更進階的部署工作，例如自訂的多個環境的資料庫部署、 檔案和資料夾排除在部署中，和部署程序期間取得 web 應用程式離線.</span><span class="sxs-lookup"><span data-stu-id="89b94-176">This tutorial describes how to accomplish various more advanced deployment tasks, like customizing database deployments for multiple environments, excluding files and folders from deployment, and taking web applications offline during the deployment process.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="89b94-177">下一步</span><span class="sxs-lookup"><span data-stu-id="89b94-177">Next</span></span>](the-contact-manager-solution.md)
