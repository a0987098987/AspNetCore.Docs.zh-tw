---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: "手動安裝 Web 封裝 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何手動匯入網際網路資訊服務 (IIS) web 部署套件。 主題建置並封裝 Web 某個應用程式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 0ab0b4c24c1771a21c45bac011b5f156cb15d28a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="manually-installing-web-packages"></a><span data-ttu-id="db950-104">手動安裝 Web 封裝</span><span class="sxs-lookup"><span data-stu-id="db950-104">Manually Installing Web Packages</span></span>
====================
<span data-ttu-id="db950-105">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="db950-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="db950-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="db950-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="db950-107">本主題描述如何手動匯入網際網路資訊服務 (IIS) web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="db950-107">This topic describes how to manually import a web deployment package into Internet Information Services (IIS).</span></span>
> 
> <span data-ttu-id="db950-108">本主題[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)描述如何在 IIS Web Deployment Tool (Web Deploy)，搭配 Microsoft Build Engine (MSBuild) 及發行 Web 管線 (WPP)，可讓您封裝程式將單一 zip 檔案的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="db950-108">The topic [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md) described how the IIS Web Deployment Tool (Web Deploy), in conjunction with the Microsoft Build Engine (MSBuild) and the Web Publishing Pipeline (WPP), lets you package your web application projects into a single zip file.</span></span> <span data-ttu-id="db950-109">這個檔案，通常稱為 web 部署套件 （或 只部署套件），包含所有的內容與組態資訊才能重新建立您的 web 應用程式，網頁伺服器上需要 IIS。</span><span class="sxs-lookup"><span data-stu-id="db950-109">This file, commonly known as a web deployment package (or simply a deployment package), contains all the content and configuration information that IIS needs in order to re-create your web application on a web server.</span></span>
> 
> <span data-ttu-id="db950-110">一旦您已建立 web 部署套件，您可以將它發行至 IIS 伺服器上以各種方式。</span><span class="sxs-lookup"><span data-stu-id="db950-110">Once you've created a web deployment package, you can publish it to an IIS server in various ways.</span></span> <span data-ttu-id="db950-111">在許多案例中，您會想要利用 MSBuild、 WPP，和 Web Deploy 來建立和自動化或單一步驟建置和部署程序的一部分，從遠端安裝 web 封裝之間的整合點。</span><span class="sxs-lookup"><span data-stu-id="db950-111">In a lot of scenarios, you'll want to take advantage of the integration points between MSBuild, the WPP, and Web Deploy to create and install web packages remotely as part of an automated or single-step build and deployment process.</span></span> <span data-ttu-id="db950-112">此程序所述[部署 Web 封裝](deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-112">This process is described in [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="db950-113">不過，這不可能。</span><span class="sxs-lookup"><span data-stu-id="db950-113">However, this isn't always possible.</span></span> <span data-ttu-id="db950-114">假設您想要部署到網際網路對向的生產環境 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="db950-114">Suppose you want to deploy a web application to an Internet-facing production environment.</span></span> <span data-ttu-id="db950-115">基於安全性理由，實際執行環境是在非常大可能位於周邊網路 （也稱為 DMZ、 非軍事區域和遮蔽式子網路） 中的組建伺服器，不同的子網路防火牆後方。</span><span class="sxs-lookup"><span data-stu-id="db950-115">For security reasons, such a production environment is at the very least likely to be behind a firewall on a subnet that is separate from the build server, in a perimeter network (also known as DMZ, demilitarized zone, and screened subnet).</span></span> <span data-ttu-id="db950-116">在許多情況下，實際執行環境會在另一個網域或實際隔離的網路上。</span><span class="sxs-lookup"><span data-stu-id="db950-116">In lots of cases, the production environment will be on a separate domain or on a physically isolated network.</span></span>
> 
> <span data-ttu-id="db950-117">在這些情況下，可能是唯一的選擇移植到目的地伺服器上的 web 套件，且以手動方式將它匯入至 IIS。</span><span class="sxs-lookup"><span data-stu-id="db950-117">In these scenarios, your only option may be to port the web package onto the destination server and manually import it into IIS.</span></span> <span data-ttu-id="db950-118">雖然這種方法，即無需自動化的部署，但它仍然是發行 web 應用程式 & #x 2014年的高效率技術，只要將單一 zip 檔案複製到您的 web 伺服器和使用精靈將引導您完成匯入程序。</span><span class="sxs-lookup"><span data-stu-id="db950-118">Although this approach precludes automated deployment, it's still a highly effective technique for publishing a web application&#x2014;you simply copy a single zip file to your web server and use a wizard to guide you through the import process.</span></span>


<span data-ttu-id="db950-119">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="db950-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="db950-120">工作概觀</span><span class="sxs-lookup"><span data-stu-id="db950-120">Task Overview</span></span>

<span data-ttu-id="db950-121">您必須完成這些高層級的工作，才能匯入至 IIS 的 web 部署套件：</span><span class="sxs-lookup"><span data-stu-id="db950-121">You'll need to complete these high-level tasks to import a web deployment package into IIS:</span></span>

- <span data-ttu-id="db950-122">建立 web 部署套件使用 MSBuild 命令列、 Team Build 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="db950-122">Create a web deployment package using the MSBuild command line, Team Build, or Visual Studio 2010.</span></span>
- <span data-ttu-id="db950-123">將 web 套件複製到目的地 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="db950-123">Copy the web package to the destination web server.</span></span>
- <span data-ttu-id="db950-124">使用 IIS 管理員中匯入應用程式套件精靈，安裝 web 封裝，並提供值給變數，例如連接字串和服務端點。</span><span class="sxs-lookup"><span data-stu-id="db950-124">Use the Import Application Package Wizard in IIS Manager to install the web package and provide values for variables like connection strings and service endpoints.</span></span>

<span data-ttu-id="db950-125">本主題將說明如何執行這些程序。</span><span class="sxs-lookup"><span data-stu-id="db950-125">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="db950-126">工作與本主題中的逐步解說假設您已經熟悉 web 封裝、 Web Deploy 和 WPP 背後的概念。</span><span class="sxs-lookup"><span data-stu-id="db950-126">The tasks and walkthroughs in this topic assume that you're already familiar with the concepts behind web packages, Web Deploy, and the WPP.</span></span> <span data-ttu-id="db950-127">如需詳細資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-127">For more information, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

> [!NOTE]
> <span data-ttu-id="db950-128">本主題最適合用於搭配[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)，其中說明如何安裝必要的元件，並準備封裝匯入的 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="db950-128">This topic is best used in conjunction with [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), which explains how to install the required components and prepare an IIS website for package import.</span></span>


## <a name="create-a-web-deployment-package"></a><span data-ttu-id="db950-129">建立 Web 部署套件</span><span class="sxs-lookup"><span data-stu-id="db950-129">Create a Web Deployment Package</span></span>

<span data-ttu-id="db950-130">第一個工作是建立您想要部署 web 應用程式專案的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="db950-130">The first task is to create a web deployment package for the web application project you want to deploy.</span></span> <span data-ttu-id="db950-131">您可以建立 web 封裝各種不同的方式。</span><span class="sxs-lookup"><span data-stu-id="db950-131">You can create web packages in a variety of ways.</span></span>

<span data-ttu-id="db950-132">**方法 1： 建立 Visual Studio 中的封裝，以做為建置流程的一部分**</span><span class="sxs-lookup"><span data-stu-id="db950-132">**Approach 1: Create a package as part of the build process with Visual Studio**</span></span>

<span data-ttu-id="db950-133">您可以設定您的 web 應用程式專案來建立每次建置後透過 web 部署套件**封裝/發行 Web**專案屬性頁 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="db950-133">You can configure your web application project to create a web deployment package after every build through the **Package/Publish Web** tab on the project property pages.</span></span> <span data-ttu-id="db950-134">此程序所述[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-134">This process is described in [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="db950-135">**方法 2： 使用 MSBuild 建立封裝，以做為建置流程的一部分**</span><span class="sxs-lookup"><span data-stu-id="db950-135">**Approach 2: Create a package as part of the build process with MSBuild**</span></span>

<span data-ttu-id="db950-136">如果您的 web 應用程式專案來建置使用 MSBuild 直接透過自訂的 MSBuild 專案檔，或從命令列中，您可以藉由包含做為建置流程的一部分建立 web 部署套件**DeployOnBuild = true**和**DeployTarget = 封裝**您在命令中的屬性。</span><span class="sxs-lookup"><span data-stu-id="db950-136">If you build your web application project by using MSBuild directly, either through a custom MSBuild project file or from the command line, you can create a web deployment package as part of the build process by including the **DeployOnBuild=true** and **DeployTarget=Package** properties in your command.</span></span> <span data-ttu-id="db950-137">此程序所述[瞭解建置程序](understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-137">This process is described in [Understanding the Build Process](understanding-the-build-process.md).</span></span>

<span data-ttu-id="db950-138">**方法 3： 在 Visual Studio 中視需要建立封裝**</span><span class="sxs-lookup"><span data-stu-id="db950-138">**Approach 3: Create a package on demand in Visual Studio**</span></span>

<span data-ttu-id="db950-139">您可以隨時在 Visual Studio 2010 建立 web 應用程式專案的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="db950-139">You can create a web deployment package for a web application project at any time in Visual Studio 2010.</span></span> <span data-ttu-id="db950-140">若要這樣做，請在**方案總管 中**視窗中，以滑鼠右鍵按一下您的 web 應用程式專案，然後**建置部署封裝**。</span><span class="sxs-lookup"><span data-stu-id="db950-140">To do this, in the **Solution Explorer** window, right-click your web application project, and then click **Build Deployment Package**.</span></span>

![](manually-installing-web-packages/_static/image1.png)

<span data-ttu-id="db950-141">**方法 4： 從命令列中，視需要建立封裝**</span><span class="sxs-lookup"><span data-stu-id="db950-141">**Approach 4: Create a package on demand from the command line**</span></span>

<span data-ttu-id="db950-142">您可以從命令列叫用來建立 web 部署套件**封裝**上 web 應用程式專案使用 MSBuild 目標。</span><span class="sxs-lookup"><span data-stu-id="db950-142">You can create a web deployment package from the command line by invoking the **Package** target on your web application project using MSBuild.</span></span> <span data-ttu-id="db950-143">此命令應該與以下相似：</span><span class="sxs-lookup"><span data-stu-id="db950-143">The command should resemble this:</span></span>


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


<span data-ttu-id="db950-144">較接近您使用，最終結果相同。</span><span class="sxs-lookup"><span data-stu-id="db950-144">Whichever approach you use, the end result is the same.</span></span> <span data-ttu-id="db950-145">WPP 為 zip 檔案，以及各種支援的資源，您的 web 應用程式專案的輸出資料夾中建立 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="db950-145">The WPP creates a web deployment package as a zip file, together with various supporting resources, in the output folder for your web application project.</span></span>

![](manually-installing-web-packages/_static/image2.png)

<span data-ttu-id="db950-146">當您計劃手動匯入 web 封裝時，您需要 zip 檔案。</span><span class="sxs-lookup"><span data-stu-id="db950-146">When you're planning to import the web package manually, you require only the zip file.</span></span> <span data-ttu-id="db950-147">將這個檔案複製到目標 web 伺服器，您可以開始匯入程序。</span><span class="sxs-lookup"><span data-stu-id="db950-147">Copy this file to your target web server and you can begin the import process.</span></span>

## <a name="import-a-web-package-into-iis"></a><span data-ttu-id="db950-148">匯入至 IIS 的 Web 套件</span><span class="sxs-lookup"><span data-stu-id="db950-148">Import a Web Package into IIS</span></span>

<span data-ttu-id="db950-149">您可以使用下一個程序，從本機檔案系統的 web 部署套件匯入至 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="db950-149">You can use the next procedure to import a web deployment package from the local file system into an IIS website.</span></span> <span data-ttu-id="db950-150">執行此程序之前，請確認您已經：</span><span class="sxs-lookup"><span data-stu-id="db950-150">Before you perform this procedure, ensure that you have:</span></span>

- <span data-ttu-id="db950-151">複製到 web 伺服器上的 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="db950-151">Copied the web deployment package to the web server.</span></span>
- <span data-ttu-id="db950-152">設定 IIS web 伺服器來裝載您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="db950-152">Configured an IIS web server to host your application.</span></span>

<span data-ttu-id="db950-153">如需有關如何設定 IIS web 伺服器以支援 web 部署套件的詳細資訊，請參閱[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-153">For more information on configuring an IIS web server to support web deployment packages, see [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span>

<span data-ttu-id="db950-154">**若要匯入使用 IIS 管理員的 web 部署封裝**</span><span class="sxs-lookup"><span data-stu-id="db950-154">**To import a web deployment package using IIS Manager**</span></span>

1. <span data-ttu-id="db950-155">在 [IIS 管理員] 中，在**連線**] 窗格中，您的 IIS 網站上按一下滑鼠右鍵，指向**部署**，然後按一下 [**匯入應用程式**。</span><span class="sxs-lookup"><span data-stu-id="db950-155">In IIS Manager, in the **Connections** pane, right-click your IIS website, point to **Deploy**, and then click **Import Application**.</span></span>

    ![](manually-installing-web-packages/_static/image3.png)
2. <span data-ttu-id="db950-156">在匯入應用程式套件精靈，在**選取的套件**頁面上，瀏覽至您的 web 部署封裝的位置，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="db950-156">In the Import Application Package Wizard, on the **Select the Package** page, browse to the location of your web deployment package, and then click **Next**.</span></span>
3. <span data-ttu-id="db950-157">在**選取封裝的內容**頁面上，清除您不需要此項目，然後再按一下的任何內容**下一步**。</span><span class="sxs-lookup"><span data-stu-id="db950-157">On the **Select the Contents of the Package** page, clear any content that you don't require, and then click **Next**.</span></span>

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="db950-158">在許多情況下，您可能不想匯入 web 部署套件隨附的所有項目。</span><span class="sxs-lookup"><span data-stu-id="db950-158">In a lot of cases, you may not want to import everything that comes with a web deployment package.</span></span> <span data-ttu-id="db950-159">例如，您可能不想讓 Web Deploy 來取代相關聯的資料庫。</span><span class="sxs-lookup"><span data-stu-id="db950-159">For example, you may not want to allow Web Deploy to replace the associated database.</span></span>  
    > <span data-ttu-id="db950-160">**授與權限**項目設定目的地檔案系統，以確保應用程式集區身分識別可以存取儲存網站內容的實體資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="db950-160">The **Grant permissions** entries set permissions on the destination file system to ensure that the application pool identity can access the physical folder that stores the website content.</span></span> <span data-ttu-id="db950-161">此外，匿名驗證使用者授與讀取資料夾權限，讓應用程式支援多用途網際網路郵件延伸標準 (MIME) 類型的檔案。</span><span class="sxs-lookup"><span data-stu-id="db950-161">In addition, the anonymous authentication user is granted read permission to the folder to let the application serve Multipurpose Internet Mail Extensions (MIME) type files.</span></span> <span data-ttu-id="db950-162">如果您想要的話，您可以移除這些項目，並手動設定權限。</span><span class="sxs-lookup"><span data-stu-id="db950-162">If you prefer, you can remove these entries and configure permissions manually.</span></span>
4. <span data-ttu-id="db950-163">在**輸入應用程式封裝資訊**頁面上，提供要求的資訊。</span><span class="sxs-lookup"><span data-stu-id="db950-163">On the **Enter Application Package Information** page, provide the requested information.</span></span>

    ![](manually-installing-web-packages/_static/image5.png)
5. <span data-ttu-id="db950-164">當您建立 web 封裝時，WPP 分析您的應用程式的組態檔，並偵測到任何變數，例如連接字串和服務端點。</span><span class="sxs-lookup"><span data-stu-id="db950-164">When you create a web package, the WPP analyzes the configuration file for your application and detects any variables, like connection strings and service endpoints.</span></span> <span data-ttu-id="db950-165">在此情況下：</span><span class="sxs-lookup"><span data-stu-id="db950-165">In this case:</span></span>

    1. <span data-ttu-id="db950-166">**應用程式路徑**是您要安裝應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="db950-166">**Application Path** is the IIS path where you want to install your application.</span></span> <span data-ttu-id="db950-167">此設定是通用於所有 WPP 建立的部署封裝。</span><span class="sxs-lookup"><span data-stu-id="db950-167">This setting is common to all deployment packages that the WPP creates.</span></span>
    2. <span data-ttu-id="db950-168">**ContactService 服務端點位址**是應用程式應該使用與已部署的 WCF 服務進行通訊的位址。</span><span class="sxs-lookup"><span data-stu-id="db950-168">**ContactService Service Endpoint Address** is the address that the application should use to communicate with the deployed WCF service.</span></span> <span data-ttu-id="db950-169">此設定會對應中的項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="db950-169">This setting corresponds to an entry in the *web.config* file.</span></span>
    3. <span data-ttu-id="db950-170">第一個**連接字串**設定是 Web Deploy 來部署與應用程式 （在此案例的 ASP.NET 成員資格資料庫） 中相關聯的資料庫應該使用的連接字串。</span><span class="sxs-lookup"><span data-stu-id="db950-170">The first **Connection String** setting is the connection string that Web Deploy should use to deploy the database associated with the application (in this case an ASP.NET membership database).</span></span> <span data-ttu-id="db950-171">此設定會對應到上設定**封裝/發行 SQL** Visual Studio 中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="db950-171">This setting corresponds to the setting on the **Package/Publish SQL** tab in Visual Studio.</span></span>
    4. <span data-ttu-id="db950-172">第二個**連接字串**設定是您的應用程式實際上將用來啟動並執行時，與資料庫通訊的連接字串。</span><span class="sxs-lookup"><span data-stu-id="db950-172">The second **Connection String** setting is the connection string that your application will actually use to communicate with the database when it's up and running.</span></span> <span data-ttu-id="db950-173">這會對應至連接字串的項目中*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="db950-173">This corresponds to a connection string entry in the *web.config* file.</span></span>

        > [!NOTE]
        > <span data-ttu-id="db950-174">如需這些參數來自何處的詳細資訊，請參閱[Web 封裝部署的設定參數](configuring-parameters-for-web-package-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-174">For more information on where these parameters come from, see [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md).</span></span>
6. <span data-ttu-id="db950-175">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="db950-175">Click **Next**.</span></span>
7. <span data-ttu-id="db950-176">如果這不被您部署了應用程式瀏覽此網站的第一次，將提示您指定是否要刪除所有現有的內容，再進行安裝。</span><span class="sxs-lookup"><span data-stu-id="db950-176">If this is not the first time you've deployed the application to this website, you'll be prompted to specify whether you want to delete all existing content prior to installation.</span></span> <span data-ttu-id="db950-177">選擇適合您需求的選項，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="db950-177">Choose the option that's appropriate for your requirements, and then click **Next**.</span></span>

    ![](manually-installing-web-packages/_static/image6.png)
8. <span data-ttu-id="db950-178">完成 IIS 已安裝封裝，請按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="db950-178">When IIS has finished installing the package, click **Finish**.</span></span>

    ![](manually-installing-web-packages/_static/image7.png)

<span data-ttu-id="db950-179">此時，您已成功發行至 IIS web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="db950-179">At this point, you've successfully published your web application to IIS.</span></span>

## <a name="conclusion"></a><span data-ttu-id="db950-180">結論</span><span class="sxs-lookup"><span data-stu-id="db950-180">Conclusion</span></span>

<span data-ttu-id="db950-181">本主題描述如何匯入至 IIS 網站使用 IIS 管理員的 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="db950-181">This topic described how to import a web deployment package into an IIS website using IIS Manager.</span></span> <span data-ttu-id="db950-182">當安全性或基礎結構的條件約束進行遠端部署，無法執行或不想要適合 web 應用程式發行至這個方法。</span><span class="sxs-lookup"><span data-stu-id="db950-182">This approach to web application publishing is appropriate when security or infrastructure constraints make remote deployment impossible or undesirable.</span></span>

## <a name="further-reading"></a><span data-ttu-id="db950-183">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="db950-183">Further Reading</span></span>

<span data-ttu-id="db950-184">如需如何設定 IIS 網頁伺服器支援手動匯入 web 封裝的指引，請參閱[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="db950-184">For guidance on how to configure an IIS web server to support manually importing a web package, see [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="db950-185">多個一般指引部署 web 封裝的詳細資訊，請參閱[逐步解說： 部署 Web 應用程式的專案使用的 Web 部署套件 (第 1 部分為 4)](https://msdn.microsoft.com/en-us/library/dd483479.aspx)。</span><span class="sxs-lookup"><span data-stu-id="db950-185">For more general guidance on deploying web packages, see [Walkthrough: Deploying a Web Application Project Using a Web Deployment Package (Part 1 of 4)](https://msdn.microsoft.com/en-us/library/dd483479.aspx).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="db950-186">上一步</span><span class="sxs-lookup"><span data-stu-id="db950-186">Previous</span></span>](creating-and-running-a-deployment-command-file.md)
