---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: 手動安裝網頁套件 |Microsoft Docs
author: jrjlee
description: 本主題描述如何手動匯入網際網路資訊服務 (IIS) web 部署套件。 主題建置和封裝 Web 應用程式...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 7fd8060104ca4e02919a3fbac135edb3e9396c64
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833001"
---
<a name="manually-installing-web-packages"></a><span data-ttu-id="cbc89-104">手動安裝網頁套件</span><span class="sxs-lookup"><span data-stu-id="cbc89-104">Manually Installing Web Packages</span></span>
====================
<span data-ttu-id="cbc89-105">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="cbc89-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="cbc89-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="cbc89-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="cbc89-107">本主題描述如何手動匯入網際網路資訊服務 (IIS) web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="cbc89-107">This topic describes how to manually import a web deployment package into Internet Information Services (IIS).</span></span>
> 
> <span data-ttu-id="cbc89-108">本主題[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)說明如何在 IIS Web Deployment Tool (Web Deploy)，搭配 Microsoft Build Engine (MSBuild) 及發行 Web 管線 (WPP)，可讓您封裝程式在單一 zip 檔案的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="cbc89-108">The topic [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md) described how the IIS Web Deployment Tool (Web Deploy), in conjunction with the Microsoft Build Engine (MSBuild) and the Web Publishing Pipeline (WPP), lets you package your web application projects into a single zip file.</span></span> <span data-ttu-id="cbc89-109">此檔案中，通常稱為 web 部署套件 （或 只部署套件），包含所有內容和組態資訊才能重新建立您的 web 應用程式，在 web 伺服器上需要 IIS。</span><span class="sxs-lookup"><span data-stu-id="cbc89-109">This file, commonly known as a web deployment package (or simply a deployment package), contains all the content and configuration information that IIS needs in order to re-create your web application on a web server.</span></span>
> 
> <span data-ttu-id="cbc89-110">一旦您已建立 web 部署套件，您可以將它發行至 IIS 伺服器上以各種方式。</span><span class="sxs-lookup"><span data-stu-id="cbc89-110">Once you've created a web deployment package, you can publish it to an IIS server in various ways.</span></span> <span data-ttu-id="cbc89-111">在許多案例中，您會想要利用的 MSBuild、 WPP，和 Web Deploy 建立及自動化或單一步驟建置和部署程序的一部分，從遠端安裝網頁套件之間的整合點。</span><span class="sxs-lookup"><span data-stu-id="cbc89-111">In a lot of scenarios, you'll want to take advantage of the integration points between MSBuild, the WPP, and Web Deploy to create and install web packages remotely as part of an automated or single-step build and deployment process.</span></span> <span data-ttu-id="cbc89-112">此程序所述[部署網頁套件](deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-112">This process is described in [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="cbc89-113">不過，這不一定可行。</span><span class="sxs-lookup"><span data-stu-id="cbc89-113">However, this isn't always possible.</span></span> <span data-ttu-id="cbc89-114">假設您想要部署網際網路對向的生產環境的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbc89-114">Suppose you want to deploy a web application to an Internet-facing production environment.</span></span> <span data-ttu-id="cbc89-115">基於安全性理由，實際執行環境是在非常大可能位於周邊網路 （也稱為 DMZ 和遮蔽式子網路） 中的組建伺服器，不同的子網路防火牆後方。</span><span class="sxs-lookup"><span data-stu-id="cbc89-115">For security reasons, such a production environment is at the very least likely to be behind a firewall on a subnet that is separate from the build server, in a perimeter network (also known as DMZ, demilitarized zone, and screened subnet).</span></span> <span data-ttu-id="cbc89-116">在許多情況下，實際執行環境會在不同的網域或實際隔離的網路上。</span><span class="sxs-lookup"><span data-stu-id="cbc89-116">In lots of cases, the production environment will be on a separate domain or on a physically isolated network.</span></span>
> 
> <span data-ttu-id="cbc89-117">在這些情況下，唯一的選擇可能是移植到目的地伺服器上的 web 套件，並以手動方式將它匯入到 IIS。</span><span class="sxs-lookup"><span data-stu-id="cbc89-117">In these scenarios, your only option may be to port the web package onto the destination server and manually import it into IIS.</span></span> <span data-ttu-id="cbc89-118">雖然這個方法會防止自動的部署，它仍然是非常有效的技術發行 web 應用程式的&#x2014;您只要將單一 zip 檔案複製到您的 web 伺服器，並使用精靈來引導您完成匯入程序。</span><span class="sxs-lookup"><span data-stu-id="cbc89-118">Although this approach precludes automated deployment, it's still a highly effective technique for publishing a web application&#x2014;you simply copy a single zip file to your web server and use a wizard to guide you through the import process.</span></span>


<span data-ttu-id="cbc89-119">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="cbc89-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="cbc89-120">工作概觀</span><span class="sxs-lookup"><span data-stu-id="cbc89-120">Task Overview</span></span>

<span data-ttu-id="cbc89-121">您必須完成這些高層級的工作，以匯入至 IIS 的 web 部署套件：</span><span class="sxs-lookup"><span data-stu-id="cbc89-121">You'll need to complete these high-level tasks to import a web deployment package into IIS:</span></span>

- <span data-ttu-id="cbc89-122">建立使用 MSBuild 命令列、 Team Build 或 Visual Studio 2010 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="cbc89-122">Create a web deployment package using the MSBuild command line, Team Build, or Visual Studio 2010.</span></span>
- <span data-ttu-id="cbc89-123">將 web 套件複製到目的地 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="cbc89-123">Copy the web package to the destination web server.</span></span>
- <span data-ttu-id="cbc89-124">使用匯入應用程式套件精靈 在 IIS 管理員 中安裝 web 套件，並提供值給變數，例如連接字串和服務端點。</span><span class="sxs-lookup"><span data-stu-id="cbc89-124">Use the Import Application Package Wizard in IIS Manager to install the web package and provide values for variables like connection strings and service endpoints.</span></span>

<span data-ttu-id="cbc89-125">本主題將示範如何執行這些程序。</span><span class="sxs-lookup"><span data-stu-id="cbc89-125">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="cbc89-126">工作與本主題中的逐步解說假設您已經熟悉網頁套件、 Web Deploy 和 WPP 背後的概念。</span><span class="sxs-lookup"><span data-stu-id="cbc89-126">The tasks and walkthroughs in this topic assume that you're already familiar with the concepts behind web packages, Web Deploy, and the WPP.</span></span> <span data-ttu-id="cbc89-127">如需詳細資訊，請參閱 <<c0> [ 建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-127">For more information, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cbc89-128">本主題最適合用於搭配[設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)，其中說明如何安裝必要的元件，並準備封裝匯入 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="cbc89-128">This topic is best used in conjunction with [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), which explains how to install the required components and prepare an IIS website for package import.</span></span>


## <a name="create-a-web-deployment-package"></a><span data-ttu-id="cbc89-129">建立 Web 部署套件</span><span class="sxs-lookup"><span data-stu-id="cbc89-129">Create a Web Deployment Package</span></span>

<span data-ttu-id="cbc89-130">第一項工作是建立您想要部署 web 應用程式專案的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="cbc89-130">The first task is to create a web deployment package for the web application project you want to deploy.</span></span> <span data-ttu-id="cbc89-131">您可以將網頁套件在各種不同的方式。</span><span class="sxs-lookup"><span data-stu-id="cbc89-131">You can create web packages in a variety of ways.</span></span>

<span data-ttu-id="cbc89-132">**方法 1： 使用 Visual Studio 中建立的套件建置程序的一部分**</span><span class="sxs-lookup"><span data-stu-id="cbc89-132">**Approach 1: Create a package as part of the build process with Visual Studio**</span></span>

<span data-ttu-id="cbc89-133">您可以設定 web 應用程式專案來建立 web 部署套件，透過每次建置後**封裝/發行 Web**上專案屬性頁 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="cbc89-133">You can configure your web application project to create a web deployment package after every build through the **Package/Publish Web** tab on the project property pages.</span></span> <span data-ttu-id="cbc89-134">此程序所述[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-134">This process is described in [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="cbc89-135">**方法 2： 使用 MSBuild 建立的套件建置程序的一部分**</span><span class="sxs-lookup"><span data-stu-id="cbc89-135">**Approach 2: Create a package as part of the build process with MSBuild**</span></span>

<span data-ttu-id="cbc89-136">如果您 web 應用程式專案使用 MSBuild 來建置，直接透過自訂的 MSBuild 專案檔，或從命令列中，您可以建立 web 部署套件建置程序的一部分包括**DeployOnBuild = true**並**DeployTarget = 封裝**命令中的屬性。</span><span class="sxs-lookup"><span data-stu-id="cbc89-136">If you build your web application project by using MSBuild directly, either through a custom MSBuild project file or from the command line, you can create a web deployment package as part of the build process by including the **DeployOnBuild=true** and **DeployTarget=Package** properties in your command.</span></span> <span data-ttu-id="cbc89-137">此程序所述[了解建置程序](understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-137">This process is described in [Understanding the Build Process](understanding-the-build-process.md).</span></span>

<span data-ttu-id="cbc89-138">**方法 3： 在 Visual Studio 中視需要建立封裝**</span><span class="sxs-lookup"><span data-stu-id="cbc89-138">**Approach 3: Create a package on demand in Visual Studio**</span></span>

<span data-ttu-id="cbc89-139">您可以隨時在 Visual Studio 2010 中建立 web 應用程式專案的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="cbc89-139">You can create a web deployment package for a web application project at any time in Visual Studio 2010.</span></span> <span data-ttu-id="cbc89-140">若要這樣做，請在**方案總管** 視窗中，以滑鼠右鍵按一下您的 web 應用程式專案，然後**建置部署封裝**。</span><span class="sxs-lookup"><span data-stu-id="cbc89-140">To do this, in the **Solution Explorer** window, right-click your web application project, and then click **Build Deployment Package**.</span></span>

![](manually-installing-web-packages/_static/image1.png)

<span data-ttu-id="cbc89-141">**方法 4： 從命令列中，視需要建立封裝**</span><span class="sxs-lookup"><span data-stu-id="cbc89-141">**Approach 4: Create a package on demand from the command line**</span></span>

<span data-ttu-id="cbc89-142">您可以從命令列叫用來建立 web 部署封裝**封裝**上 web 應用程式專案使用 MSBuild 目標。</span><span class="sxs-lookup"><span data-stu-id="cbc89-142">You can create a web deployment package from the command line by invoking the **Package** target on your web application project using MSBuild.</span></span> <span data-ttu-id="cbc89-143">此命令應該與以下相似：</span><span class="sxs-lookup"><span data-stu-id="cbc89-143">The command should resemble this:</span></span>


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


<span data-ttu-id="cbc89-144">較接近您使用，最終結果相同。</span><span class="sxs-lookup"><span data-stu-id="cbc89-144">Whichever approach you use, the end result is the same.</span></span> <span data-ttu-id="cbc89-145">WPP 為 zip 檔案，以及各種支援資源的詳細資訊，在您的 web 應用程式專案的輸出資料夾中建立 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="cbc89-145">The WPP creates a web deployment package as a zip file, together with various supporting resources, in the output folder for your web application project.</span></span>

![](manually-installing-web-packages/_static/image2.png)

<span data-ttu-id="cbc89-146">當您計劃手動匯入的 web 套件時，您會需要 zip 檔案。</span><span class="sxs-lookup"><span data-stu-id="cbc89-146">When you're planning to import the web package manually, you require only the zip file.</span></span> <span data-ttu-id="cbc89-147">這個檔案複製到目標 web 伺服器，您可以開始匯入程序。</span><span class="sxs-lookup"><span data-stu-id="cbc89-147">Copy this file to your target web server and you can begin the import process.</span></span>

## <a name="import-a-web-package-into-iis"></a><span data-ttu-id="cbc89-148">網頁套件匯入 IIS</span><span class="sxs-lookup"><span data-stu-id="cbc89-148">Import a Web Package into IIS</span></span>

<span data-ttu-id="cbc89-149">您可以使用下一個程序，從本機檔案系統的 web 部署套件匯入至 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="cbc89-149">You can use the next procedure to import a web deployment package from the local file system into an IIS website.</span></span> <span data-ttu-id="cbc89-150">執行此程序之前，請確定您有：</span><span class="sxs-lookup"><span data-stu-id="cbc89-150">Before you perform this procedure, ensure that you have:</span></span>

- <span data-ttu-id="cbc89-151">複製到 web 伺服器上的 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="cbc89-151">Copied the web deployment package to the web server.</span></span>
- <span data-ttu-id="cbc89-152">設定 IIS web 伺服器來裝載您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbc89-152">Configured an IIS web server to host your application.</span></span>

<span data-ttu-id="cbc89-153">如需有關如何設定 IIS web 伺服器，以支援 web 部署套件的詳細資訊，請參閱 <<c0> [ 設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-153">For more information on configuring an IIS web server to support web deployment packages, see [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span>

<span data-ttu-id="cbc89-154">**若要匯入 web 部署套件使用 IIS 管理員**</span><span class="sxs-lookup"><span data-stu-id="cbc89-154">**To import a web deployment package using IIS Manager**</span></span>

1. <span data-ttu-id="cbc89-155">在 [IIS 管理員] 中，在**連線**窗格中，您的 IIS 網站上按一下滑鼠右鍵，指向**部署**，然後按一下**匯入應用程式**。</span><span class="sxs-lookup"><span data-stu-id="cbc89-155">In IIS Manager, in the **Connections** pane, right-click your IIS website, point to **Deploy**, and then click **Import Application**.</span></span>

    ![](manually-installing-web-packages/_static/image3.png)
2. <span data-ttu-id="cbc89-156">在匯入應用程式套件精靈中，在**選取的套件**頁面上，瀏覽至您的 web 部署套件的位置，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="cbc89-156">In the Import Application Package Wizard, on the **Select the Package** page, browse to the location of your web deployment package, and then click **Next**.</span></span>
3. <span data-ttu-id="cbc89-157">在 [**選取封裝的內容**頁面上，清除任何內容，您不需要此項目，然後再按一下**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="cbc89-157">On the **Select the Contents of the Package** page, clear any content that you don't require, and then click **Next**.</span></span>

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="cbc89-158">在許多情況下，您可能不想要匯入 web 部署套件隨附的所有項目。</span><span class="sxs-lookup"><span data-stu-id="cbc89-158">In a lot of cases, you may not want to import everything that comes with a web deployment package.</span></span> <span data-ttu-id="cbc89-159">例如，您可能不想讓 Web Deploy 來取代相關聯的資料庫。</span><span class="sxs-lookup"><span data-stu-id="cbc89-159">For example, you may not want to allow Web Deploy to replace the associated database.</span></span>  
    > <span data-ttu-id="cbc89-160">**授與權限**項目設定目的地的檔案系統，以確保應用程式集區身分識別可以存取存放網站內容的實體資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="cbc89-160">The **Grant permissions** entries set permissions on the destination file system to ensure that the application pool identity can access the physical folder that stores the website content.</span></span> <span data-ttu-id="cbc89-161">此外，匿名驗證使用者會被授與讀取到資料夾的權限，讓應用程式提供多用途網際網路郵件延伸標準 (MIME) 類型的檔案。</span><span class="sxs-lookup"><span data-stu-id="cbc89-161">In addition, the anonymous authentication user is granted read permission to the folder to let the application serve Multipurpose Internet Mail Extensions (MIME) type files.</span></span> <span data-ttu-id="cbc89-162">如果您想，您可以移除這些項目，並手動設定權限。</span><span class="sxs-lookup"><span data-stu-id="cbc89-162">If you prefer, you can remove these entries and configure permissions manually.</span></span>
4. <span data-ttu-id="cbc89-163">在 **輸入應用程式封裝資訊**頁面上，提供要求的資訊。</span><span class="sxs-lookup"><span data-stu-id="cbc89-163">On the **Enter Application Package Information** page, provide the requested information.</span></span>

    ![](manually-installing-web-packages/_static/image5.png)
5. <span data-ttu-id="cbc89-164">當您建立 web 封裝時，WPP 會分析您的應用程式的組態檔，並偵測到任何變數，例如連接字串和服務端點。</span><span class="sxs-lookup"><span data-stu-id="cbc89-164">When you create a web package, the WPP analyzes the configuration file for your application and detects any variables, like connection strings and service endpoints.</span></span> <span data-ttu-id="cbc89-165">在此情況下：</span><span class="sxs-lookup"><span data-stu-id="cbc89-165">In this case:</span></span>

    1. <span data-ttu-id="cbc89-166">**應用程式路徑**是您想要用來安裝應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="cbc89-166">**Application Path** is the IIS path where you want to install your application.</span></span> <span data-ttu-id="cbc89-167">此設定是通用於所有 WPP 建立的部署封裝。</span><span class="sxs-lookup"><span data-stu-id="cbc89-167">This setting is common to all deployment packages that the WPP creates.</span></span>
    2. <span data-ttu-id="cbc89-168">**ContactService 服務端點位址**是應用程式應該使用與已部署的 WCF 服務進行通訊的位址。</span><span class="sxs-lookup"><span data-stu-id="cbc89-168">**ContactService Service Endpoint Address** is the address that the application should use to communicate with the deployed WCF service.</span></span> <span data-ttu-id="cbc89-169">此設定對應中的項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="cbc89-169">This setting corresponds to an entry in the *web.config* file.</span></span>
    3. <span data-ttu-id="cbc89-170">第一個**連接字串**設定是 Web Deploy 來部署應用程式 （在此案例中的 ASP.NET 成員資格資料庫） 相關聯的資料庫應該使用的連接字串。</span><span class="sxs-lookup"><span data-stu-id="cbc89-170">The first **Connection String** setting is the connection string that Web Deploy should use to deploy the database associated with the application (in this case an ASP.NET membership database).</span></span> <span data-ttu-id="cbc89-171">此設定會對應到上設定**封裝/發行 SQL** Visual Studio 中的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="cbc89-171">This setting corresponds to the setting on the **Package/Publish SQL** tab in Visual Studio.</span></span>
    4. <span data-ttu-id="cbc89-172">第二個**連接字串**設定是您的應用程式實際上將用來啟動並執行時，與資料庫通訊的連接字串。</span><span class="sxs-lookup"><span data-stu-id="cbc89-172">The second **Connection String** setting is the connection string that your application will actually use to communicate with the database when it's up and running.</span></span> <span data-ttu-id="cbc89-173">這會對應到中的連接字串項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="cbc89-173">This corresponds to a connection string entry in the *web.config* file.</span></span>

        > [!NOTE]
        > <span data-ttu-id="cbc89-174">如需有關這些參數來自何處的詳細資訊，請參閱 <<c0> [ 網頁套件部署的設定參數](configuring-parameters-for-web-package-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-174">For more information on where these parameters come from, see [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md).</span></span>
6. <span data-ttu-id="cbc89-175">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="cbc89-175">Click **Next**.</span></span>
7. <span data-ttu-id="cbc89-176">如果這不是您已部署應用程式瀏覽此網站的第一次，系統會提示您指定您是否要刪除所有現有的內容，再進行安裝。</span><span class="sxs-lookup"><span data-stu-id="cbc89-176">If this is not the first time you've deployed the application to this website, you'll be prompted to specify whether you want to delete all existing content prior to installation.</span></span> <span data-ttu-id="cbc89-177">選擇適合您的需求的選項，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="cbc89-177">Choose the option that's appropriate for your requirements, and then click **Next**.</span></span>

    ![](manually-installing-web-packages/_static/image6.png)
8. <span data-ttu-id="cbc89-178">完成 IIS 已安裝套件，請按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="cbc89-178">When IIS has finished installing the package, click **Finish**.</span></span>

    ![](manually-installing-web-packages/_static/image7.png)

<span data-ttu-id="cbc89-179">到目前為止，您已成功發行至 IIS web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbc89-179">At this point, you've successfully published your web application to IIS.</span></span>

## <a name="conclusion"></a><span data-ttu-id="cbc89-180">結論</span><span class="sxs-lookup"><span data-stu-id="cbc89-180">Conclusion</span></span>

<span data-ttu-id="cbc89-181">本主題說明如何將 IIS 網站使用 IIS 管理員匯入 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="cbc89-181">This topic described how to import a web deployment package into an IIS website using IIS Manager.</span></span> <span data-ttu-id="cbc89-182">當安全性或基礎結構的限制會使得遠端部署，不可能或不想要適合使用這個方法，來發佈 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbc89-182">This approach to web application publishing is appropriate when security or infrastructure constraints make remote deployment impossible or undesirable.</span></span>

## <a name="further-reading"></a><span data-ttu-id="cbc89-183">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="cbc89-183">Further Reading</span></span>

<span data-ttu-id="cbc89-184">如需如何設定 IIS web 伺服器來支援手動匯入 web 封裝的指引，請參閱 <<c0> [ 設定 Web 伺服器進行 Web 部署發行 （離線部署）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-184">For guidance on how to configure an IIS web server to support manually importing a web package, see [Configure a Web Server for Web Deploy Publishing (Offline Deployment)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="cbc89-185">如需更多一般指引部署網頁套件的詳細資訊，請參閱[逐步解說： 部署 Web 應用程式專案使用 Web 部署套件 (第 1 部分，共 4 部分)](https://msdn.microsoft.com/library/dd483479.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cbc89-185">For more general guidance on deploying web packages, see [Walkthrough: Deploying a Web Application Project Using a Web Deployment Package (Part 1 of 4)](https://msdn.microsoft.com/library/dd483479.aspx).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cbc89-186">上一步</span><span class="sxs-lookup"><span data-stu-id="cbc89-186">Previous</span></span>](creating-and-running-a-deployment-command-file.md)
