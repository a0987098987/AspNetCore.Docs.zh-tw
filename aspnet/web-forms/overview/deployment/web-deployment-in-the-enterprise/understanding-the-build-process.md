---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: 了解建置程序 |Microsoft Docs
author: jrjlee
description: 本主題提供企業規模建置和部署程序的逐步解說。 本主題中所述的方法會使用自訂的 Microsoft 建置 Engin...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 9df145b281b086f546c55d0b26a8b0e44e896bb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827216"
---
<a name="understanding-the-build-process"></a><span data-ttu-id="b406c-104">了解建置程序</span><span class="sxs-lookup"><span data-stu-id="b406c-104">Understanding the Build Process</span></span>
====================
<span data-ttu-id="b406c-105">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b406c-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b406c-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="b406c-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b406c-107">本主題提供企業規模建置和部署程序的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="b406c-107">This topic provides a walkthrough of an enterprise-scale build and deployment process.</span></span> <span data-ttu-id="b406c-108">本主題中所述的方法會使用自訂的 Microsoft Build Engine (MSBuild) 專案檔，來提供更細微的控制程序的各個層面。</span><span class="sxs-lookup"><span data-stu-id="b406c-108">The approach described in this topic uses custom Microsoft Build Engine (MSBuild) project files to provide fine-grained control over every aspect of the process.</span></span> <span data-ttu-id="b406c-109">在專案檔中，自訂的 MSBuild 目標來執行部署公用程式，如 Internet Information Services (IIS) Web 部署工具 (MSDeploy.exe) 和資料庫部署公用程式 VSDBCMD.exe。</span><span class="sxs-lookup"><span data-stu-id="b406c-109">Within the project files, custom MSBuild targets are used to run deployment utilities like the Internet Information Services (IIS) Web Deployment Tool (MSDeploy.exe) and the database deployment utility VSDBCMD.exe.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="b406c-110">先前的主題[了解專案檔](understanding-the-project-file.md)描述 MSBuild 專案檔的重要元件，引進了分割專案檔案，以支援部署至多個目標環境的概念。</span><span class="sxs-lookup"><span data-stu-id="b406c-110">The previous topic, [Understanding the Project File](understanding-the-project-file.md), described the key components of an MSBuild project file and introduced the concept of split project files to support deployment to multiple target environments.</span></span> <span data-ttu-id="b406c-111">如果您不熟悉這些概念，您應該檢閱[了解專案檔](understanding-the-project-file.md)您逐步完成本主題之前。</span><span class="sxs-lookup"><span data-stu-id="b406c-111">If you're not already familiar with these concepts, you should review [Understanding the Project File](understanding-the-project-file.md) before you work through this topic.</span></span>


<span data-ttu-id="b406c-112">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="b406c-112">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="b406c-113">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="b406c-113">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="b406c-114">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="b406c-114">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="build-and-deployment-overview"></a><span data-ttu-id="b406c-115">建置和部署概觀</span><span class="sxs-lookup"><span data-stu-id="b406c-115">Build and Deployment Overview</span></span>

<span data-ttu-id="b406c-116">在 [連絡管理員解決方案](the-contact-manager-solution.md)，三個檔案來控制組建和部署程序：</span><span class="sxs-lookup"><span data-stu-id="b406c-116">In the [Contact Manager solution](the-contact-manager-solution.md), three files control the build and deployment process:</span></span>

- <span data-ttu-id="b406c-117">A*通用專案檔*(*Publish.proj*)。</span><span class="sxs-lookup"><span data-stu-id="b406c-117">A *universal project file* (*Publish.proj*).</span></span> <span data-ttu-id="b406c-118">這包含不會變更目的地環境之間的組建和部署指示。</span><span class="sxs-lookup"><span data-stu-id="b406c-118">This contains build and deployment instructions that do not change between destination environments.</span></span>
- <span data-ttu-id="b406c-119">*環境特定專案檔*(*Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="b406c-119">An *environment-specific project file* (*Env-Dev.proj*).</span></span> <span data-ttu-id="b406c-120">這包含專屬於特定的目的地環境的建置和部署設定。</span><span class="sxs-lookup"><span data-stu-id="b406c-120">This contains build and deployment settings that are specific to a particular destination environment.</span></span> <span data-ttu-id="b406c-121">例如，您可以使用*Env Dev.proj*檔案，以提供開發人員或測試環境的設定，並建立名為的替代檔案*Env Stage.proj*提供預備環境的設定環境。</span><span class="sxs-lookup"><span data-stu-id="b406c-121">For example, you could use the *Env-Dev.proj* file to provide settings for a developer or test environment and create an alternative file named *Env-Stage.proj* to provide settings for a staging environment.</span></span>
- <span data-ttu-id="b406c-122">A*命令檔*(*發佈 Dev.cmd*)。</span><span class="sxs-lookup"><span data-stu-id="b406c-122">A *command file* (*Publish-Dev.cmd*).</span></span> <span data-ttu-id="b406c-123">這包含指定的專案檔案的命令要執行的 MSBuild.exe。</span><span class="sxs-lookup"><span data-stu-id="b406c-123">This contains an MSBuild.exe command that specifies which project files you want to execute.</span></span> <span data-ttu-id="b406c-124">您可以建立命令檔的每個目的地環境中，其中每個檔案包含 MSBuild.exe 命令指定不同的環境特定專案檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-124">You can create a command file for every destination environment, where each file contains an MSBuild.exe command that specifies a different environment-specific project file.</span></span> <span data-ttu-id="b406c-125">這可讓開發人員部署至不同的環境，只要執行適當的命令檔。</span><span class="sxs-lookup"><span data-stu-id="b406c-125">This lets the developer deploy to different environments simply by running the appropriate command file.</span></span>

<span data-ttu-id="b406c-126">在範例解決方案中，您可以找到這些發行方案資料夾中的三個檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-126">In the sample solution, you can find these three files in the Publish solution folder.</span></span>

![](understanding-the-build-process/_static/image1.png)

<span data-ttu-id="b406c-127">您可以查看更多詳細資料中的這些檔案之前，讓我們看看當您使用這種方法時，整體的建置程序的運作方式。</span><span class="sxs-lookup"><span data-stu-id="b406c-127">Before you look at these files in more detail, let's take a look at how the overall build process works when you use this approach.</span></span> <span data-ttu-id="b406c-128">概括而言，建置和部署程序看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="b406c-128">At a high level, the build and deployment process looks like this:</span></span>

![](understanding-the-build-process/_static/image2.png)

<span data-ttu-id="b406c-129">發生第一件事是兩個專案檔&#x2014;一個包含通用的組建和部署指示，其中包含環境特定設定的另一個&#x2014;會合併成單一專案檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-129">The first thing that happens is that the two project files&#x2014;one containing universal build and deployment instructions, and one containing environment-specific settings&#x2014;are merged into a single project file.</span></span> <span data-ttu-id="b406c-130">MSBuild 接著適用於透過專案檔中的指示。</span><span class="sxs-lookup"><span data-stu-id="b406c-130">MSBuild then works through the instructions in the project file.</span></span> <span data-ttu-id="b406c-131">它會建置每個方案，每個專案中使用專案檔中的專案。</span><span class="sxs-lookup"><span data-stu-id="b406c-131">It builds each of the projects in the solution, using the project file for each project.</span></span> <span data-ttu-id="b406c-132">然後它會呼叫其他工具，如 Web Deploy (MSDeploy.exe) 和 VSDBCMD 公用程式，以將您的 web 內容和資料庫部署到目標環境。</span><span class="sxs-lookup"><span data-stu-id="b406c-132">It then calls out to other tools, like Web Deploy (MSDeploy.exe) and the VSDBCMD utility to deploy your web content and databases to the target environment.</span></span>

<span data-ttu-id="b406c-133">從開始到完成，建置和部署程序會執行下列工作：</span><span class="sxs-lookup"><span data-stu-id="b406c-133">From start to finish, the build and deployment process performs these tasks:</span></span>

1. <span data-ttu-id="b406c-134">它會刪除輸出目錄中，以準備進行全新的組建的內容。</span><span class="sxs-lookup"><span data-stu-id="b406c-134">It deletes the contents of the output directory, in preparation for a fresh build.</span></span>
2. <span data-ttu-id="b406c-135">它會建置方案中的每個專案：</span><span class="sxs-lookup"><span data-stu-id="b406c-135">It builds each project in the solution:</span></span>

    1. <span data-ttu-id="b406c-136">Web 專案的&#x2014;在此情況下，ASP.NET MVC web 應用程式和 WCF web 服務&#x2014;建置程序會建立每個專案的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="b406c-136">For web projects&#x2014;in this case, an ASP.NET MVC web application and a WCF web service&#x2014;the build process creates a web deployment package for each project.</span></span>
    2. <span data-ttu-id="b406c-137">資料庫專案的建置程序會建立每個專案的部署資訊清單 （.deploymanifest 檔案）。</span><span class="sxs-lookup"><span data-stu-id="b406c-137">For database projects, the build process creates a deployment manifest (.deploymanifest file) for each project.</span></span>
3. <span data-ttu-id="b406c-138">它使用 VSDBCMD.exe 公用程式來部署每個資料庫專案，在解決方案中，從專案檔中使用各種屬性&#x2014;目標連接字串和資料庫名稱&#x2014;以及.deploymanifest 檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-138">It uses the VSDBCMD.exe utility to deploy each database project in the solution, using various properties from the project files&#x2014;a target connection string and a database name&#x2014;together with the .deploymanifest file.</span></span>
4. <span data-ttu-id="b406c-139">它會使用 MSDeploy.exe 公用程式將在解決方案中，從專案檔中使用各種屬性，來控制部署程序的每個 web 專案部署。</span><span class="sxs-lookup"><span data-stu-id="b406c-139">It uses the MSDeploy.exe utility to deploy each web project in the solution, using various properties from the project files to control the deployment process.</span></span>

<span data-ttu-id="b406c-140">若要追蹤此程序的更多詳細資料，您可以使用範例方案。</span><span class="sxs-lookup"><span data-stu-id="b406c-140">You can use the sample solution to trace this process in more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="b406c-141">如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="b406c-141">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


## <a name="invoking-the-build-and-deployment-process"></a><span data-ttu-id="b406c-142">叫用的組建和部署程序</span><span class="sxs-lookup"><span data-stu-id="b406c-142">Invoking the Build and Deployment Process</span></span>

<span data-ttu-id="b406c-143">若要將連絡管理員解決方案部署到開發人員測試環境中，執行開發人員*發佈 Dev.cmd*命令檔。</span><span class="sxs-lookup"><span data-stu-id="b406c-143">To deploy the Contact Manager solution to a developer test environment, the developer runs the *Publish-Dev.cmd* command file.</span></span> <span data-ttu-id="b406c-144">這樣會叫用 MSBuild.exe，指定*Publish.proj*作為要執行的專案檔案並*Env Dev.proj*做為參數值。</span><span class="sxs-lookup"><span data-stu-id="b406c-144">This invokes MSBuild.exe, specifying *Publish.proj* as the project file to execute and *Env-Dev.proj* as a parameter value.</span></span>


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="b406c-145">**/Fl**切換 (簡稱 **/fileLogger**) 的組建輸出記錄到名為的檔案*msbuild.log*目前目錄中。</span><span class="sxs-lookup"><span data-stu-id="b406c-145">The **/fl** switch (short for **/fileLogger**) logs the build output to a file named *msbuild.log* in the current directory.</span></span> <span data-ttu-id="b406c-146">如需詳細資訊，請參閱 < [MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b406c-146">For more information, see the [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>


<span data-ttu-id="b406c-147">此時，MSBuild 會開始執行，載入*Publish.proj*檔案，並開始處理中的指示。</span><span class="sxs-lookup"><span data-stu-id="b406c-147">At this point, MSBuild starts running, loads the *Publish.proj* file, and starts processing the instructions within it.</span></span> <span data-ttu-id="b406c-148">第一個指令會告知 MSBuild，以匯入專案檔**TargetEnvPropsFile**參數指定。</span><span class="sxs-lookup"><span data-stu-id="b406c-148">The first instruction tells MSBuild to import the project file that the **TargetEnvPropsFile** parameter specifies.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


<span data-ttu-id="b406c-149">**TargetEnvPropsFile**參數指定*Env Dev.proj*檔案，因此 MSBuild 會將合併的內容*Env Dev.proj*檔案載入*Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-149">The **TargetEnvPropsFile** parameter specifies the *Env-Dev.proj* file, so MSBuild merges the contents of the *Env-Dev.proj* file into the *Publish.proj* file.</span></span>

<span data-ttu-id="b406c-150">MSBuild 遇到合併的專案檔中的下一個項目是屬性群組。</span><span class="sxs-lookup"><span data-stu-id="b406c-150">The next elements that MSBuild encounters in the merged project file are property groups.</span></span> <span data-ttu-id="b406c-151">屬性會以其出現在檔案中的順序處理。</span><span class="sxs-lookup"><span data-stu-id="b406c-151">Properties are processed in the order in which they appear in the file.</span></span> <span data-ttu-id="b406c-152">MSBuild 會建立的索引鍵 / 值對每個屬性，提供，符合任何指定的條件。</span><span class="sxs-lookup"><span data-stu-id="b406c-152">MSBuild creates a key-value pair for each property, providing that any specified conditions are met.</span></span> <span data-ttu-id="b406c-153">檔案中稍後定義的屬性會以稍早定義檔案中的相同名稱覆寫任何屬性。</span><span class="sxs-lookup"><span data-stu-id="b406c-153">Properties defined later in the file will overwrite any properties with the same name defined earlier in the file.</span></span> <span data-ttu-id="b406c-154">例如，請考慮**OutputRoot**屬性。</span><span class="sxs-lookup"><span data-stu-id="b406c-154">For example, consider the **OutputRoot** properties.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


<span data-ttu-id="b406c-155">當 MSBuild 處理第一個**OutputRoot**項目，提供類似名稱的參數未提供，它會設定的值**OutputRoot**屬性設 **...\Publish\Out**。當它遇到第二個**OutputRoot**項目，如果條件評估為 **，則為 true**，它會覆寫的值**OutputRoot**屬性的值**OutDir**參數。</span><span class="sxs-lookup"><span data-stu-id="b406c-155">When MSBuild processes the first **OutputRoot** element, providing a similarly named parameter has not been provided, it sets the value of the **OutputRoot** property to **..\Publish\Out**. When it encounters the second **OutputRoot** element, if the condition evaluates to **true**, it will overwrite the value of the **OutputRoot** property with the value of the **OutDir** parameter.</span></span>

<span data-ttu-id="b406c-156">MSBuild 遇到下一個項目是單一項目群組，其中包含具名項目**ProjectsToBuild**。</span><span class="sxs-lookup"><span data-stu-id="b406c-156">The next element that MSBuild encounters is a single item group, containing an item named **ProjectsToBuild**.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


<span data-ttu-id="b406c-157">MSBuild 建置名為的項目清單，處理這個指示**ProjectsToBuild**。</span><span class="sxs-lookup"><span data-stu-id="b406c-157">MSBuild processes this instruction by building an item list named **ProjectsToBuild**.</span></span> <span data-ttu-id="b406c-158">在此情況下，項目清單包含單一值&#x2014;的路徑和方案檔的檔名。</span><span class="sxs-lookup"><span data-stu-id="b406c-158">In this case, the item list contains a single value&#x2014;the path and filename of the solution file.</span></span>

<span data-ttu-id="b406c-159">此時，其餘的項目是目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-159">At this point, the remaining elements are targets.</span></span> <span data-ttu-id="b406c-160">目標會以不同方式處理屬性和項目從&#x2014;基本上，目標不會處理除非它們是明確地指定使用者或叫用的專案檔內的另一個建構。</span><span class="sxs-lookup"><span data-stu-id="b406c-160">Targets are processed differently from properties and items&#x2014;essentially, targets are not processed unless they are either explicitly specified by the user or invoked by another construct within the project file.</span></span> <span data-ttu-id="b406c-161">請記得，開頭**專案**標記會包括**DefaultTargets**屬性。</span><span class="sxs-lookup"><span data-stu-id="b406c-161">Recall that the opening **Project** tag includes a **DefaultTargets** attribute.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


<span data-ttu-id="b406c-162">這會指示 MSBuild 叫用**FullPublish**叫用 MSBuild.exe 時指定的目標，如果目標不是。</span><span class="sxs-lookup"><span data-stu-id="b406c-162">This instructs MSBuild to invoke the **FullPublish** target, if targets are not specified when MSBuild.exe is invoked.</span></span> <span data-ttu-id="b406c-163">**FullPublish**目標不包含任何工作，而是它只是指定相依性清單。</span><span class="sxs-lookup"><span data-stu-id="b406c-163">The **FullPublish** target doesn't contain any tasks; instead it simply specifies a list of dependencies.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


<span data-ttu-id="b406c-164">此相依性會告知 MSBuild，以執行為了**FullPublish**目標，它需要叫用此清單所提供的順序中的目標：</span><span class="sxs-lookup"><span data-stu-id="b406c-164">This dependency tells MSBuild that in order to execute the **FullPublish** target, it needs to invoke this list of targets in the order provided:</span></span>

1. <span data-ttu-id="b406c-165">它必須叫用**清除**目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-165">It must invoke the **Clean** target.</span></span>
2. <span data-ttu-id="b406c-166">它必須叫用**BuildProjects**目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-166">It must invoke the **BuildProjects** target.</span></span>
3. <span data-ttu-id="b406c-167">它必須叫用**GatherPackagesForPublishing**目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-167">It must invoke the **GatherPackagesForPublishing** target.</span></span>
4. <span data-ttu-id="b406c-168">它必須叫用**PublishDbPackages**目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-168">It must invoke the **PublishDbPackages** target.</span></span>
5. <span data-ttu-id="b406c-169">它必須叫用**PublishWebPackages**目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-169">It must invoke the **PublishWebPackages** target.</span></span>

### <a name="the-clean-target"></a><span data-ttu-id="b406c-170">「 清除 」 目標</span><span class="sxs-lookup"><span data-stu-id="b406c-170">The Clean Target</span></span>

<span data-ttu-id="b406c-171">**清除**目標基本上會刪除輸出目錄和其所有內容，以準備將全新的組建。</span><span class="sxs-lookup"><span data-stu-id="b406c-171">The **Clean** target basically deletes the output directory and all its contents, as preparation for a fresh build.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


<span data-ttu-id="b406c-172">請注意，目標包含**ItemGroup**項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-172">Notice that the target includes an **ItemGroup** element.</span></span> <span data-ttu-id="b406c-173">當您定義屬性或項目內**目標**項目，您要建立*動態*屬性和項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-173">When you define properties or items within a **Target** element, you're creating *dynamic* properties and items.</span></span> <span data-ttu-id="b406c-174">換句話說，屬性或項目不被處理在執行目標之前。</span><span class="sxs-lookup"><span data-stu-id="b406c-174">In other words, the properties or items aren't processed until the target is executed.</span></span> <span data-ttu-id="b406c-175">輸出目錄可能不存在或包含的任何檔案，直到建置程序開始，因此您無法建置 **\_FilesToDelete**清單做為靜態項目; 您不必等到執行正在進行中。</span><span class="sxs-lookup"><span data-stu-id="b406c-175">The output directory might not exist or contain any files until the build process begins, so you can't build the **\_FilesToDelete** list as a static item; you have to wait until execution is underway.</span></span> <span data-ttu-id="b406c-176">此情況下，您可以建置清單做為目標內的動態項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-176">As such, you build the list as a dynamic item within the target.</span></span>

> [!NOTE]
> <span data-ttu-id="b406c-177">在此情況下，因為**清除**目標是要執行的第一，實際的無須使用動態項目群組。</span><span class="sxs-lookup"><span data-stu-id="b406c-177">In this case, because the **Clean** target is the first to be executed, there's no real need to use a dynamic item group.</span></span> <span data-ttu-id="b406c-178">不過，最好使用動態屬性和項目，在這種案例中，您可能想要不同的順序，在某個時間點執行的目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-178">However, it's good practice to use dynamic properties and items in this type of scenario, as you might want to execute targets in a different order at some point.</span></span>  
> <span data-ttu-id="b406c-179">您應該也設法避免宣告絕不會使用的項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-179">You should also aim to avoid declaring items that will never be used.</span></span> <span data-ttu-id="b406c-180">如果您有只用於特定目標的項目，請考慮將它們放置在要移除任何不必要的額外負荷，在建置程序的目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-180">If you have items that will only be used by a specific target, consider placing them inside the target to remove any unnecessary overhead on the build process.</span></span>


<span data-ttu-id="b406c-181">擱置在一旁，動態項目**Clean**目標就相當簡單，並利用內建**訊息**，**刪除**，和**RemoveDir**工作：</span><span class="sxs-lookup"><span data-stu-id="b406c-181">Dynamic items aside, the **Clean** target is fairly straightforward and makes use of the built-in **Message**, **Delete**, and **RemoveDir** tasks to:</span></span>

1. <span data-ttu-id="b406c-182">將訊息傳送至記錄器。</span><span class="sxs-lookup"><span data-stu-id="b406c-182">Send a message to the logger.</span></span>
2. <span data-ttu-id="b406c-183">建置要刪除檔案的清單。</span><span class="sxs-lookup"><span data-stu-id="b406c-183">Build a list of files to delete.</span></span>
3. <span data-ttu-id="b406c-184">刪除檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-184">Delete the files.</span></span>
4. <span data-ttu-id="b406c-185">移除輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="b406c-185">Remove the output directory.</span></span>

### <a name="the-buildprojects-target"></a><span data-ttu-id="b406c-186">BuildProjects 目標</span><span class="sxs-lookup"><span data-stu-id="b406c-186">The BuildProjects Target</span></span>

<span data-ttu-id="b406c-187">**BuildProjects**目標基本上會建置範例方案中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="b406c-187">The **BuildProjects** target basically builds all the projects in the sample solution.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


<span data-ttu-id="b406c-188">此目標，在上一個主題中，詳細說明[了解專案檔](understanding-the-project-file.md)，以說明如何工作和目標參考屬性和項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-188">This target was described in some detail in the previous topic, [Understanding the Project File](understanding-the-project-file.md), to illustrate how tasks and targets reference properties and items.</span></span> <span data-ttu-id="b406c-189">此時，您有興趣主要**MSBuild**工作。</span><span class="sxs-lookup"><span data-stu-id="b406c-189">At this point, you're mainly interested in the **MSBuild** task.</span></span> <span data-ttu-id="b406c-190">您可以使用這項工作來建置多個專案。</span><span class="sxs-lookup"><span data-stu-id="b406c-190">You can use this task to build multiple projects.</span></span> <span data-ttu-id="b406c-191">工作不會建立 MSBuild.exe; 的新執行個體它會使用目前的執行個體建置每個專案。</span><span class="sxs-lookup"><span data-stu-id="b406c-191">The task does not create a new instance of MSBuild.exe; it uses the current running instance to build each project.</span></span> <span data-ttu-id="b406c-192">此範例中值得注意的重點是部署屬性：</span><span class="sxs-lookup"><span data-stu-id="b406c-192">The key points of interest in this example are the deployment properties:</span></span>

- <span data-ttu-id="b406c-193">**DeployOnBuild**屬性會指示 MSBuild，以每個專案的組建完成時，將專案設定中執行的任何部署指示。</span><span class="sxs-lookup"><span data-stu-id="b406c-193">The **DeployOnBuild** property instructs MSBuild to run any deployment instructions in the project settings when the build of each project is complete.</span></span>
- <span data-ttu-id="b406c-194">**DeployTarget**屬性會識別您想要建置專案之後叫用的目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-194">The **DeployTarget** property identifies the target that you want to invoke after the project is built.</span></span> <span data-ttu-id="b406c-195">在此情況下，**封裝**目標建置成可部署 web 封裝的專案輸出。</span><span class="sxs-lookup"><span data-stu-id="b406c-195">In this case, the **Package** target builds the project output into a deployable web package.</span></span>

> [!NOTE]
> <span data-ttu-id="b406c-196">**封裝**目標會叫用 Web 發行管線 (WPP)，可提供 MSBuild 和 Web Deploy 之間的整合。</span><span class="sxs-lookup"><span data-stu-id="b406c-196">The **Package** target invokes the Web Publishing Pipeline (WPP), which provides integration between MSBuild and Web Deploy.</span></span> <span data-ttu-id="b406c-197">如果您想要的內建目標 WPP 提供，檢閱一下*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-197">If you want to take a look at the built-in targets that the WPP provides, review the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


### <a name="the-gatherpackagesforpublishing-target"></a><span data-ttu-id="b406c-198">GatherPackagesForPublishing 目標</span><span class="sxs-lookup"><span data-stu-id="b406c-198">The GatherPackagesForPublishing Target</span></span>

<span data-ttu-id="b406c-199">如果您先研究**GatherPackagesForPublishing**目標，您會注意到，它實際上不包含任何工作。</span><span class="sxs-lookup"><span data-stu-id="b406c-199">If you study the **GatherPackagesForPublishing** target, you'll notice that it doesn't actually contain any tasks.</span></span> <span data-ttu-id="b406c-200">相反地，它包含三個動態項目會定義單一項目群組。</span><span class="sxs-lookup"><span data-stu-id="b406c-200">Instead, it contains a single item group that defines three dynamic items.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


<span data-ttu-id="b406c-201">這些項目來參考時所建立的部署套件**BuildProjects**執行目標時。</span><span class="sxs-lookup"><span data-stu-id="b406c-201">These items refer to the deployment packages that were created when the **BuildProjects** target was executed.</span></span> <span data-ttu-id="b406c-202">您無法定義這些項目以靜態方式在專案檔中，因為項目所參考的檔案不存在直到**BuildProjects**執行目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-202">You couldn't define these items statically in the project file, because the files to which the items refer don't exist until the **BuildProjects** target is executed.</span></span> <span data-ttu-id="b406c-203">相反地，項目必須是以動態方式定義內不會叫用之前的目標後**BuildProjects**執行目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-203">Instead, the items must be defined dynamically within a target that is not invoked until after the **BuildProjects** target is executed.</span></span>

<span data-ttu-id="b406c-204">項目不會使用這個目標內&#x2014;的項目和每個項目值相關聯的中繼資料，只會建置這個目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-204">The items are not used within this target&#x2014;this target simply builds the items and the metadata associated with each item value.</span></span> <span data-ttu-id="b406c-205">處理這些項目，一旦**PublishPackages**項目將包含兩個值的路徑*ContactManager.Mvc.deploy.cmd*檔案和路徑*ContactManager.Service.deploy.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-205">Once these elements are processed, the **PublishPackages** item will contain two values, the path to the *ContactManager.Mvc.deploy.cmd* file and the path to the *ContactManager.Service.deploy.cmd* file.</span></span> <span data-ttu-id="b406c-206">Web Deploy 建立這些檔案，以針對每個專案中，web 封裝的一部分，這些是您必須叫用的檔案才能部署套件在目的地伺服器上。</span><span class="sxs-lookup"><span data-stu-id="b406c-206">Web Deploy creates these files as part of the web package for each project, and these are the files that you must invoke on the destination server in order to deploy the packages.</span></span> <span data-ttu-id="b406c-207">如果您開啟其中一個檔案，您基本上會看到各種建置特定的參數值 MSDeploy.exe 命令。</span><span class="sxs-lookup"><span data-stu-id="b406c-207">If you open up one of these files, you'll basically see an MSDeploy.exe command with various build-specific parameter values.</span></span>

<span data-ttu-id="b406c-208">**DbPublishPackages**項目將包含單一值的路徑*ContactManager.Database.deploymanifest*檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-208">The **DbPublishPackages** item will contain a single value, the path to the *ContactManager.Database.deploymanifest* file.</span></span>

> [!NOTE]
> <span data-ttu-id="b406c-209">當您建置資料庫專案，並為 MSBuild 專案檔使用相同的結構描述時，會產生.deploymanifest 檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-209">A .deploymanifest file is generated when you build a database project, and it uses the same schema as an MSBuild project file.</span></span> <span data-ttu-id="b406c-210">它包含部署資料庫，包括資料庫結構描述 (.dbschema) 的位置，以及任何部署前和部署後指令碼的詳細資料所需的所有資訊。</span><span class="sxs-lookup"><span data-stu-id="b406c-210">It contains all the information required to deploy a database, including the location of the database schema (.dbschema) and details of any pre-deployment and post-deployment scripts.</span></span> <span data-ttu-id="b406c-211">如需詳細資訊，請參閱 <<c0> [ 概觀的資料庫建置和部署](https://msdn.microsoft.com/library/aa833165.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b406c-211">For more information, see [An Overview of Database Build and Deployment](https://msdn.microsoft.com/library/aa833165.aspx).</span></span>


<span data-ttu-id="b406c-212">您將進一步了解如何部署封裝和資料庫部署資訊清單建立和使用中[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)並[部署資料庫專案](deploying-database-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="b406c-212">You'll learn more about how deployment packages and database deployment manifests are created and used in [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md) and [Deploying Database Projects](deploying-database-projects.md).</span></span>

### <a name="the-publishdbpackages-target"></a><span data-ttu-id="b406c-213">PublishDbPackages 目標</span><span class="sxs-lookup"><span data-stu-id="b406c-213">The PublishDbPackages Target</span></span>

<span data-ttu-id="b406c-214">簡要來說， **PublishDbPackages**目標會叫用 VSDBCMD 公用程式來部署**ContactManager**至目標環境的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b406c-214">Briefly speaking, the **PublishDbPackages** target invokes the VSDBCMD utility to deploy the **ContactManager** database to a target environment.</span></span> <span data-ttu-id="b406c-215">設定資料庫部署牽涉到許多決策和細微差異，以及您將進一步了解這[部署資料庫專案](deploying-database-projects.md)和[自訂資料庫部署多個環境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="b406c-215">Configuring database deployment involves lots of decisions and nuances, and you'll learn more about this in [Deploying Database Projects](deploying-database-projects.md) and [Customizing Database Deployments for Multiple Environments](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="b406c-216">在本主題中，我們將焦點放在此目標，實際的運作。</span><span class="sxs-lookup"><span data-stu-id="b406c-216">In this topic, we'll focus on how this target actually functions.</span></span>

<span data-ttu-id="b406c-217">首先，請注意，要包含項目的開頭標記**輸出**屬性。</span><span class="sxs-lookup"><span data-stu-id="b406c-217">First, notice that the opening tag includes an **Outputs** attribute.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


<span data-ttu-id="b406c-218">這是範例*目標批次處理*。</span><span class="sxs-lookup"><span data-stu-id="b406c-218">This is an example of *target batching*.</span></span> <span data-ttu-id="b406c-219">在 MSBuild 專案檔中，批次處理是一種技術，用於逐一查看集合。</span><span class="sxs-lookup"><span data-stu-id="b406c-219">In MSBuild project files, batching is a technique for iterating over collections.</span></span> <span data-ttu-id="b406c-220">值**輸出**屬性， **"%(DbPublishPackages.Identity)"**，是指**識別**中繼資料屬性**DbPublishPackages**項目清單。</span><span class="sxs-lookup"><span data-stu-id="b406c-220">The value of the **Outputs** attribute, **"%(DbPublishPackages.Identity)"**, refers to the **Identity** metadata property of the **DbPublishPackages** item list.</span></span> <span data-ttu-id="b406c-221">此表示法，**Outputs=%***(ItemList.ItemMetadataName)*，會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="b406c-221">This notation, **Outputs=%***(ItemList.ItemMetadataName)*, is translated as:</span></span>

- <span data-ttu-id="b406c-222">分割中的項目**DbPublishPackages**到包含相同的項目批次**識別**中繼資料值。</span><span class="sxs-lookup"><span data-stu-id="b406c-222">Split the items in **DbPublishPackages** into batches of items that contain the same **Identity** metadata value.</span></span>
- <span data-ttu-id="b406c-223">執行一次每個批次的目標。</span><span class="sxs-lookup"><span data-stu-id="b406c-223">Execute the target once per batch.</span></span>

> [!NOTE]
> <span data-ttu-id="b406c-224">**身分識別**是其中一個[內建的中繼資料值](https://msdn.microsoft.com/library/ms164313.aspx)指派給每個項目上建立。</span><span class="sxs-lookup"><span data-stu-id="b406c-224">**Identity** is one of the [built-in metadata values](https://msdn.microsoft.com/library/ms164313.aspx) that is assigned to every item on creation.</span></span> <span data-ttu-id="b406c-225">它的值是指**Include**屬性中**項目**項目的&#x2014;換句話說，路徑和檔案名稱的項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-225">It refers to the value of the **Include** attribute in the **Item** element&#x2014;in other words, the path and filename of the item.</span></span>


<span data-ttu-id="b406c-226">在此情況下，因為應該永遠不會有一個以上的項目具有相同的路徑和檔名，我們基本上正在使用的其中一個批次大小。</span><span class="sxs-lookup"><span data-stu-id="b406c-226">In this case, because there should never be more than one item with the same path and filename, we're essentially working with batch sizes of one.</span></span> <span data-ttu-id="b406c-227">對於每個資料庫的套件，目標被執行一次。</span><span class="sxs-lookup"><span data-stu-id="b406c-227">The target is executed once for every database package.</span></span>

<span data-ttu-id="b406c-228">您可以看到類似的標記法 **\_Cmd**屬性，建置 VSDBCMD 命令與適當的參數。</span><span class="sxs-lookup"><span data-stu-id="b406c-228">You can see a similar notation in the **\_Cmd** property, which builds a VSDBCMD command with the appropriate switches.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


<span data-ttu-id="b406c-229">在此情況下， **%(DbPublishPackages.DatabaseConnectionString)**， **%(DbPublishPackages.TargetDatabase)**，並 **%(DbPublishPackages.FullPath)** 所有參考中繼資料值**DbPublishPackages**項目集合。</span><span class="sxs-lookup"><span data-stu-id="b406c-229">In this case, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, and **%(DbPublishPackages.FullPath)** all refer to metadata values of the **DbPublishPackages** item collection.</span></span> <span data-ttu-id="b406c-230">**\_Cmd**屬性由**Exec**工作中，叫用命令。</span><span class="sxs-lookup"><span data-stu-id="b406c-230">The **\_Cmd** property is used by the **Exec** task, which invokes the command.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


<span data-ttu-id="b406c-231">因為此表示法，而**Exec**工作會建立唯一的組合為基礎的批次**DatabaseConnectionString**， **TargetDatabase**，以及**FullPath**中繼資料值，以及工作會執行一次的每個批次。</span><span class="sxs-lookup"><span data-stu-id="b406c-231">As a result of this notation, the **Exec** task will create batches based on unique combinations of the **DatabaseConnectionString**, **TargetDatabase**, and **FullPath** metadata values, and the task will execute once for each batch.</span></span> <span data-ttu-id="b406c-232">這是範例*工作批次處理*。</span><span class="sxs-lookup"><span data-stu-id="b406c-232">This is an example of *task batching*.</span></span> <span data-ttu-id="b406c-233">不過，因為目標層級的批次已經分割成單一項目的批次中，我們項目集合**Exec**工作將會執行一次，一次針對目標的每個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="b406c-233">However, because the target-level batching has already divided our item collection into single-item batches, the **Exec** task will run once and only once for each iteration of the target.</span></span> <span data-ttu-id="b406c-234">換句話說，這項工作會叫用 VSDBCMD 公用程式，針對方案中每個資料庫封裝一次。</span><span class="sxs-lookup"><span data-stu-id="b406c-234">In other words, this task invokes the VSDBCMD utility once for each database package in the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="b406c-235">如需有關目標和工作批次處理的詳細資訊，請參閱 MSBuild[批次處理](https://msdn.microsoft.com/library/ms171473.aspx)，[目標批次處理中的項目中繼資料](https://msdn.microsoft.com/library/ms228229.aspx)，並[工作批次處理中的項目中繼資料](https://msdn.microsoft.com/library/ms171474.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b406c-235">For more information on target and task batching, see MSBuild [Batching](https://msdn.microsoft.com/library/ms171473.aspx), [Item Metadata in Target Batching](https://msdn.microsoft.com/library/ms228229.aspx), and [Item Metadata in Task Batching](https://msdn.microsoft.com/library/ms171474.aspx).</span></span>


### <a name="the-publishwebpackages-target"></a><span data-ttu-id="b406c-236">PublishWebPackages 目標</span><span class="sxs-lookup"><span data-stu-id="b406c-236">The PublishWebPackages Target</span></span>

<span data-ttu-id="b406c-237">此時，您已叫用**BuildProjects**目標，這會產生範例方案中的每個專案的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="b406c-237">By this point, you've invoked the **BuildProjects** target, which generates a web deployment package for each project in the sample solution.</span></span> <span data-ttu-id="b406c-238">隨附的每個套件是*deploy.cmd*檔案，其中包含將封裝部署至目標環境所需的 MSDeploy.exe 命令，以及*之 SetParameters.xml*指定的檔案必要的目標環境的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b406c-238">Accompanying each package is a *deploy.cmd* file, which contains the MSDeploy.exe commands required to deploy the package to the target environment, and a *SetParameters.xml* file, which specifies the necessary details of the target environment.</span></span> <span data-ttu-id="b406c-239">您也叫用**GatherPackagesForPublishing**目標，這會產生包含您建立的項目集合*deploy.cmd*您感興趣的檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-239">You've also invoked the **GatherPackagesForPublishing** target, which generates an item collection containing the *deploy.cmd* files you're interested in.</span></span> <span data-ttu-id="b406c-240">基本上， **PublishWebPackages**目標會執行這些函式：</span><span class="sxs-lookup"><span data-stu-id="b406c-240">Essentially, the **PublishWebPackages** target performs these functions:</span></span>

- <span data-ttu-id="b406c-241">其中操作*之 SetParameters.xml*檔案以包含正確的詳細資訊，目標環境中，每個套件使用**XmlPoke**工作。</span><span class="sxs-lookup"><span data-stu-id="b406c-241">It manipulates the *SetParameters.xml* file for each package to include the correct details for the target environment, using the **XmlPoke** task.</span></span>
- <span data-ttu-id="b406c-242">它會叫用*deploy.cmd*每一個封裝，使用適當的參數檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-242">It invokes the *deploy.cmd* file for each package, using the appropriate switches.</span></span>

<span data-ttu-id="b406c-243">就如同**PublishDbPackages**目標**PublishWebPackages**目標會使用目標批次處理以確保每個 web 封裝的執行目標時一次。</span><span class="sxs-lookup"><span data-stu-id="b406c-243">Just like the **PublishDbPackages** target, the **PublishWebPackages** target uses target batching to ensure that the target is executed once for each web package.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


<span data-ttu-id="b406c-244">目標，內**Exec**工作來執行*deploy.cmd*針對每個 web 封裝的檔案。</span><span class="sxs-lookup"><span data-stu-id="b406c-244">Within the target, the **Exec** task is used to run the *deploy.cmd* file for each web package.</span></span>


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


<span data-ttu-id="b406c-245">如需有關如何設定網頁套件部署的詳細資訊，請參閱[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="b406c-245">For more information on configuring the deployment of web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>

## <a name="conclusion"></a><span data-ttu-id="b406c-246">結論</span><span class="sxs-lookup"><span data-stu-id="b406c-246">Conclusion</span></span>

<span data-ttu-id="b406c-247">本主題提供如何分割專案檔用來控制建置和部署程序從開始到結束 「 連絡人管理員 」 範例方案的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="b406c-247">This topic provided a walkthrough of how split project files are used to control the build and deployment process from start to finish for the Contact Manager sample solution.</span></span> <span data-ttu-id="b406c-248">使用這種方法讓您執行複雜的企業級部署在單一、 高再現性的步驟中，只要執行環境特有的命令檔。</span><span class="sxs-lookup"><span data-stu-id="b406c-248">Using this approach lets you run complex, enterprise-scale deployments in a single, repeatable step, simply by running an environment-specific command file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="b406c-249">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="b406c-249">Further Reading</span></span>

<span data-ttu-id="b406c-250">專案檔和 WPP 更深入的簡介，請參閱[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William bartholomew< /，ISBN: 978-0-7356-4524-0。</span><span class="sxs-lookup"><span data-stu-id="b406c-250">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b406c-251">[上一頁](understanding-the-project-file.md)
> [下一頁](building-and-packaging-web-application-projects.md)</span><span class="sxs-lookup"><span data-stu-id="b406c-251">[Previous](understanding-the-project-file.md)
[Next](building-and-packaging-web-application-projects.md)</span></span>
