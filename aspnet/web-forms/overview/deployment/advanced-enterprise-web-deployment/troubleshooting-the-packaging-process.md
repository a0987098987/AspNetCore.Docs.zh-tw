---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: 疑難排解封裝程序 |Microsoft 文件
author: jrjlee
description: 本主題描述如何使用 EnablePackageProcessLoggingAndAssert 屬性 M 中可以收集在封裝程序的詳細的資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892682"
---
<a name="troubleshooting-the-packaging-process"></a><span data-ttu-id="fbbb8-103">疑難排解封裝程序</span><span class="sxs-lookup"><span data-stu-id="fbbb8-103">Troubleshooting the Packaging Process</span></span>
====================
<span data-ttu-id="fbbb8-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="fbbb8-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="fbbb8-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="fbbb8-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="fbbb8-106">本主題描述如何收集封裝程序的詳細的資訊，以及在使用**EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 中的屬性。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-106">This topic describes how you can collect detailed information about the packaging process by using the **EnablePackageProcessLoggingAndAssert** property in the Microsoft Build Engine (MSBuild).</span></span>
> 
> <span data-ttu-id="fbbb8-107">當您將**EnablePackageProcessLoggingAndAssert**屬性**true**，MSBuild 將會：</span><span class="sxs-lookup"><span data-stu-id="fbbb8-107">When you set the **EnablePackageProcessLoggingAndAssert** property to **true**, MSBuild will:</span></span>
> 
> - <span data-ttu-id="fbbb8-108">將封裝程序的其他資訊加入至組建記錄檔。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-108">Add additional information about the packaging process to the build logs.</span></span>
> - <span data-ttu-id="fbbb8-109">記錄錯誤，某些狀況下，比方說，如果封裝清單中找到重複的檔案。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-109">Log errors under certain conditions, for example, if duplicate files are found in the packaging list.</span></span>
> - <span data-ttu-id="fbbb8-110">建立的記錄檔目錄*ProjectName*\_封裝的資料夾，並使用它來記錄您要封裝之檔案的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-110">Create a Log directory in the *ProjectName*\_Package folder and use it to record information about the files you're packaging.</span></span>
> 
> <span data-ttu-id="fbbb8-111">如果無法在封裝程序，或您的 web 部署封裝不包含您預期的檔案，您可以使用此資訊來疑難排解程序，而且 pinpoint 將要錯誤項目。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-111">If the packaging process is failing, or your web deployment packages don't contain the files that you expect, you can use this information to troubleshoot the process and pinpoint where things are going wrong.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="fbbb8-112">**EnablePackageProcessLoggingAndAssert**屬性只有在您建置的專案使用**偵錯**組態。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-112">The **EnablePackageProcessLoggingAndAssert** property only works if you build your project using the **Debug** configuration.</span></span> <span data-ttu-id="fbbb8-113">在其他組態中會忽略此屬性。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-113">The property is ignored in other configurations.</span></span>


<span data-ttu-id="fbbb8-114">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-114">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="fbbb8-115">這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-115">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="fbbb8-116">在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-116">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a><span data-ttu-id="fbbb8-117">了解 EnablePackageProcessLoggingAndAssert 屬性</span><span class="sxs-lookup"><span data-stu-id="fbbb8-117">Understanding the EnablePackageProcessLoggingAndAssert Property</span></span>

<span data-ttu-id="fbbb8-118">[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)描述 Web 發行管線 (WPP) 如何提供一組擴充功能的 MSBuild 並啟用整合與網際網路資訊服務 (IIS) Web MSBuild 目標部署工具 (Web Deploy)。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-118">[Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) described how the Web Publishing Pipeline (WPP) provides a set of MSBuild targets that extend the functionality of MSBuild and enable it to integrate with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="fbbb8-119">當您封裝的 web 應用程式專案時，您要叫用 WPP 目標。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-119">When you package a web application project, you're invoking WPP targets.</span></span>

<span data-ttu-id="fbbb8-120">許多這些 WPP 目標加入額外的資訊記錄的條件式邏輯時**EnablePackageProcessLoggingAndAssert**屬性設定為**true**。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-120">Lots of these WPP targets include conditional logic that logs additional information when the **EnablePackageProcessLoggingAndAssert** property is set to **true**.</span></span> <span data-ttu-id="fbbb8-121">例如，如果您檢閱**封裝**目標，您可以看到它會建立額外的記錄檔目錄，以及如果寫入文字檔案的檔案清單**EnablePackageProcessLoggingAndAssert**等於**true**。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-121">For example, if you review the **Package** target, you can see that it creates an additional log directory and writes a list of files to a text file if **EnablePackageProcessLoggingAndAssert** is equal to **true**.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> <span data-ttu-id="fbbb8-122">中所定義的 WPP 目標*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-122">The WPP targets are defined in the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span> <span data-ttu-id="fbbb8-123">您可以開啟這個檔案，並檢閱 Visual Studio 2010 或任何 XML 編輯器中的目標。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-123">You can open this file and review the targets in Visual Studio 2010 or any XML editor.</span></span> <span data-ttu-id="fbbb8-124">請注意不要修改檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-124">Take care not to modify the contents of the file.</span></span>


## <a name="enabling-the-additional-logging"></a><span data-ttu-id="fbbb8-125">啟用額外的記錄功能</span><span class="sxs-lookup"><span data-stu-id="fbbb8-125">Enabling the Additional Logging</span></span>

<span data-ttu-id="fbbb8-126">您可以提供的值**EnablePackageProcessLoggingAndAssert**以各種方式，根據您如何建置您的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-126">You can supply a value for the **EnablePackageProcessLoggingAndAssert** property in various ways, depending on how you build your project.</span></span>

<span data-ttu-id="fbbb8-127">如果您建置您的專案，從命令列，您可以提供的值**EnablePackageProcessLoggingAndAssert**做為命令列引數的屬性：</span><span class="sxs-lookup"><span data-stu-id="fbbb8-127">If you build your project from the command line, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property as a command-line argument:</span></span>


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


<span data-ttu-id="fbbb8-128">如果您使用自訂的專案檔案以建置專案，您可以包含**EnablePackageProcessLoggingAndAssert**值**屬性**屬性**MSBuild**工作：</span><span class="sxs-lookup"><span data-stu-id="fbbb8-128">If you're using a custom project file to build your projects, you can include the **EnablePackageProcessLoggingAndAssert** value in the **Properties** attribute of the **MSBuild** task:</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


<span data-ttu-id="fbbb8-129">如果您使用 Team Foundation Server (TFS) 組建定義來建置您的專案，您可以提供的值**EnablePackageProcessLoggingAndAssert**屬性**MSBuild 引數**資料列：![](troubleshooting-the-packaging-process/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fbbb8-129">If you're using a Team Foundation Server (TFS) build definition to build your projects, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property in the **MSBuild Arguments** row:![](troubleshooting-the-packaging-process/_static/image1.png)</span></span>

> [!NOTE]
> <span data-ttu-id="fbbb8-130">如需有關建立及設定組建定義的詳細資訊，請參閱[建立組建定義，支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-130">For more information on creating and configuring build definitions, see [Creating a Build Definition That Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>


<span data-ttu-id="fbbb8-131">或者，如果您想要在每次建置中包含的封裝，您可以修改專案檔來設定 web 應用程式專案**EnablePackageProcessLoggingAndAssert**屬性**true**。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-131">Alternatively, if you want to include the package in every build, you can modify the project file for your web application project to set the **EnablePackageProcessLoggingAndAssert** property to **true**.</span></span> <span data-ttu-id="fbbb8-132">您應該將屬性加入至第一個**PropertyGroup**在您的.csproj 或.vbproj 檔案中的項目。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-132">You should add the property to the first **PropertyGroup** element within your .csproj or .vbproj file.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a><span data-ttu-id="fbbb8-133">檢閱記錄檔</span><span class="sxs-lookup"><span data-stu-id="fbbb8-133">Reviewing the Log Files</span></span>

<span data-ttu-id="fbbb8-134">當您建置並封裝使用的 web 應用程式專案**EnablePackageProcessLoggingAndAssert**設**true**，MSBuild 會建立名為記錄檔中的其他資料夾*ProjectName*\_套件資料夾。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-134">When you build and package a web application project with **EnablePackageProcessLoggingAndAssert** set to **true**, MSBuild creates an additional folder named Log in the *ProjectName*\_Package folder.</span></span> <span data-ttu-id="fbbb8-135">記錄檔資料夾包含各種不同的檔案：</span><span class="sxs-lookup"><span data-stu-id="fbbb8-135">The Log folder contains various files:</span></span>

![](troubleshooting-the-packaging-process/_static/image2.png)

<span data-ttu-id="fbbb8-136">您看到的檔案清單會根據您的專案和您的建置流程中的項目而有所不同。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-136">The list of files that you see will vary according to the things in your project and your build process.</span></span> <span data-ttu-id="fbbb8-137">不過，這些檔案通常用來記錄的封裝，在程序的各個階段 WPP 正在收集的檔案清單：</span><span class="sxs-lookup"><span data-stu-id="fbbb8-137">However, these files are typically used to record the list of files that the WPP is collecting for packaging, at various stages of the process:</span></span>

- <span data-ttu-id="fbbb8-138">*PreExcludePipelineCollectFilesPhaseFileList.txt*檔案列出檔，MSBuild 會收集封裝之前指定排除的任何檔案會遭到移除。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-138">The *PreExcludePipelineCollectFilesPhaseFileList.txt* file lists the files that MSBuild collects for packaging before any files that are specified for exclusion are removed.</span></span>
- <span data-ttu-id="fbbb8-139">*AfterExcludeFilesFilesList.txt*檔案包含已修改的檔案清單之後指定排除的任何檔案會遭到移除。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-139">The *AfterExcludeFilesFilesList.txt* file contains the modified file list after any files that are specified for exclusion are removed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fbbb8-140">如需有關檔案和資料夾排除在封裝程序的詳細資訊，請參閱[排除檔案和資料夾從部署](excluding-files-and-folders-from-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-140">For more information on excluding files and folders from the packaging process, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>
- <span data-ttu-id="fbbb8-141">*AfterTransformWebConfig.txt*檔會列出針對封裝之後收集的檔案*Web.config*已執行轉換。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-141">The *AfterTransformWebConfig.txt* file lists the files collected for packaging after any *Web.config* transforms have been performed.</span></span> <span data-ttu-id="fbbb8-142">在此清單中，任何特定組態*Web.config*轉換檔案，例如*Web.Debug.config*和*Web.Release.config*，會排除的檔案清單封裝。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-142">In this list, any configuration-specific *Web.config* transform files, like *Web.Debug.config* and *Web.Release.config*, are excluded from the list of files for packaging.</span></span> <span data-ttu-id="fbbb8-143">轉換單一*Web.config*隨附於其位置。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-143">A single transformed *Web.config* is included in their place.</span></span>
- <span data-ttu-id="fbbb8-144">*PostAutoParameterizationWebConfigConnectionStrings.txt*檔案包含的檔案清單中的連接字串之後*Web.config*已參數化檔案。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-144">The *PostAutoParameterizationWebConfigConnectionStrings.txt* file contains the list of files after the connection strings in the *Web.config* file have been parameterized.</span></span> <span data-ttu-id="fbbb8-145">這是程序，可讓您部署封裝時將連接字串取代您的目標環境的正確設定。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-145">This is the process that lets you replace your connection strings with the right settings for your target environment when you deploy the package.</span></span>
- <span data-ttu-id="fbbb8-146">*Prepackage.txt*檔案包含最終的建置前要包含在封裝中的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-146">The *Prepackage.txt* file contains the finalized pre-build list of files to be included in the package.</span></span>

> [!NOTE]
> <span data-ttu-id="fbbb8-147">額外的記錄檔的名稱通常會對應到 WPP 目標。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-147">The names of the additional log files typically correspond to WPP targets.</span></span> <span data-ttu-id="fbbb8-148">您可以檢閱這些目標，藉由檢查*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-148">You can review these targets by examining the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


<span data-ttu-id="fbbb8-149">如果 web 封裝的內容不是您所預期，請檢閱這些檔案可以是實用的方式，來識別在何時處理程序中的發生錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-149">If the contents of your web package aren't what you expected, reviewing these files can be a useful way to identify at what point in the process things went wrong.</span></span>

## <a name="conclusion"></a><span data-ttu-id="fbbb8-150">結論</span><span class="sxs-lookup"><span data-stu-id="fbbb8-150">Conclusion</span></span>

<span data-ttu-id="fbbb8-151">本主題描述如何使用**EnablePackageProcessLoggingAndAssert** MSBuild 疑難排解封裝程序中的屬性。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-151">This topic described how you can use the **EnablePackageProcessLoggingAndAssert** property in MSBuild to troubleshoot the packaging process.</span></span> <span data-ttu-id="fbbb8-152">它說明不同的方式，您可以在其中提供要建置處理序的屬性值和它所述的其他資訊，您將屬性設定為時，會記錄**true**。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-152">It explained the different ways in which you can supply the property value to the build process, and it described the additional information that is recorded when you set the property to **true**.</span></span>

## <a name="further-reading"></a><span data-ttu-id="fbbb8-153">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="fbbb8-153">Further Reading</span></span>

<span data-ttu-id="fbbb8-154">如需使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-154">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="fbbb8-155">如需有關 WPP 及如何管理封裝程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-155">For more information on the WPP and how it manages the packaging process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="fbbb8-156">如需如何從 web 部署套件中排除特定檔案和資料夾的指引，請參閱[排除檔案和資料夾從部署](excluding-files-and-folders-from-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="fbbb8-156">For guidance on how to exclude specific files and folders from web deployment packages, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fbbb8-157">上一步</span><span class="sxs-lookup"><span data-stu-id="fbbb8-157">Previous</span></span>](running-windows-powershell-scripts-from-msbuild-project-files.md)
