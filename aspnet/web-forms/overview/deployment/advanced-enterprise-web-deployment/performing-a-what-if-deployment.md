---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 如果執行模擬部署 |Microsoft Docs
author: jrjlee
description: 本主題描述如何執行 '如果' （或模擬） 使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 和 V 部署...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833168"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="22761-103">執行"What If"部署</span><span class="sxs-lookup"><span data-stu-id="22761-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="22761-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="22761-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="22761-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="22761-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="22761-106">本主題描述如何執行 「 假設 」 （或模擬） 使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 與 VSDBCMD 部署。</span><span class="sxs-lookup"><span data-stu-id="22761-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="22761-107">這可讓您實際部署您的應用程式之前，判斷特定的目標環境上部署邏輯的影響。</span><span class="sxs-lookup"><span data-stu-id="22761-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="22761-108">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="22761-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="22761-109">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置和部署流程控制的兩個專案檔&#x2014;其中一個包含套用至每個目的地環境，以及包含環境特定建置和部署設定的組建指示。</span><span class="sxs-lookup"><span data-stu-id="22761-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="22761-110">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="22761-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="22761-111">執行"What If"部署 Web 封裝</span><span class="sxs-lookup"><span data-stu-id="22761-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="22761-112">Web Deploy 包含可讓您執行中的部署 「 假設 」 的功能 （或試用版） 模式。</span><span class="sxs-lookup"><span data-stu-id="22761-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="22761-113">當您部署 「 假設 」 模式中的成品時，Web Deploy 產生記錄檔，如同執行部署，但它不會實際變更目的地伺服器上的任何項目。</span><span class="sxs-lookup"><span data-stu-id="22761-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="22761-114">檢閱記錄檔，可協助您了解您的部署必須在目的地伺服器上，特別是在何種影響：</span><span class="sxs-lookup"><span data-stu-id="22761-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="22761-115">項目就會被新增。</span><span class="sxs-lookup"><span data-stu-id="22761-115">What will get added.</span></span>
- <span data-ttu-id="22761-116">將更新項目。</span><span class="sxs-lookup"><span data-stu-id="22761-116">What will get updated.</span></span>
- <span data-ttu-id="22761-117">項目將會刪除。</span><span class="sxs-lookup"><span data-stu-id="22761-117">What will get deleted.</span></span>

<span data-ttu-id="22761-118">因為"what if"部署不會實際變更任何項目在目的地伺服器上，它不能永遠做什麼是預測是否會成功部署。</span><span class="sxs-lookup"><span data-stu-id="22761-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="22761-119">中所述[部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)，您可以將使用兩種方式中的 Web Deploy 的 web 套件部署&#x2014;使用 MSDeploy.exe 命令列公用程式，直接或透過執行 *。 deploy.cmd*檔案會產生建置程序。</span><span class="sxs-lookup"><span data-stu-id="22761-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="22761-120">如果您直接使用 MSDeploy.exe，您可以執行"what if"部署加上 **– whatif**旗標，以您的命令。</span><span class="sxs-lookup"><span data-stu-id="22761-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="22761-121">比方說，若要評估您將 ContactManager.Mvc.zip 套件部署至預備環境會發生什麼事，MSDeploy 命令看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="22761-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="22761-122">當您滿意您 「 假設 」 部署的結果時，您可以移除 **– whatif**旗標，以執行即時的部署。</span><span class="sxs-lookup"><span data-stu-id="22761-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="22761-123">如需有關 MSDeploy.exe 命令列選項的詳細資訊，請參閱[Web Deploy 作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="22761-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="22761-124">如果您使用 *。 deploy.cmd*檔案中，您可以執行"what if"部署包括 **/t**旗標 （試用模式） 旗標，而非 **/y**旗標 （"yes"或更新模式）您的命令。</span><span class="sxs-lookup"><span data-stu-id="22761-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="22761-125">例如，若要評估您部署 ContactManager.Mvc.zip 封裝執行會發生什麼事 *。 deploy.cmd*檔案中，您的命令應該看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="22761-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="22761-126">當您滿意您的 「 試用模式 」 部署的結果時，您可以取代 **/t**旗標進行 **/y**旗標，以執行即時的部署：</span><span class="sxs-lookup"><span data-stu-id="22761-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="22761-127">如需有關命令列選項 *。 deploy.cmd*檔，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。</span><span class="sxs-lookup"><span data-stu-id="22761-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="22761-128">如果您執行 *。 deploy.cmd*檔案而不指定任何旗標，命令提示字元會顯示可用的旗標的清單。</span><span class="sxs-lookup"><span data-stu-id="22761-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="22761-129">執行"What If"部署資料庫</span><span class="sxs-lookup"><span data-stu-id="22761-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="22761-130">本節假設您使用 VSDBCMD 公用程式來執行累加式、 結構描述為基礎的資料庫部署。</span><span class="sxs-lookup"><span data-stu-id="22761-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="22761-131">這種方法更詳細地說明[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="22761-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="22761-132">我們建議，您自己熟悉本主題之前套用以下所述的概念。</span><span class="sxs-lookup"><span data-stu-id="22761-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="22761-133">當您使用在 VSDBCMD**部署**模式中，您可以使用 **/dd** (或 **/DeployToDatabase**) 是否 VSDBCMD 實際部署資料庫，或只產生要控制旗標部署指令碼。</span><span class="sxs-lookup"><span data-stu-id="22761-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="22761-134">如果您要部署.dbschema 檔案，這是行為：</span><span class="sxs-lookup"><span data-stu-id="22761-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="22761-135">如果您指定 **/dd+** 或是 **/dd**，VSDBCMD 會產生部署指令碼，並將資料庫部署。</span><span class="sxs-lookup"><span data-stu-id="22761-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="22761-136">如果您指定 **/dd-** 或省略此參數，VSDBCMD 會產生只部署指令碼。</span><span class="sxs-lookup"><span data-stu-id="22761-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="22761-137">如果您要部署.deploymanifest 檔案，而不是.dbschema 檔案的行為 **/dd**參數是複雜多了。</span><span class="sxs-lookup"><span data-stu-id="22761-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="22761-138">基本上，VSDBCMD 會略過的值 **/dd**如果.deploymanifest 檔案包含切換**DeployToDatabase**值的項目**True**。</span><span class="sxs-lookup"><span data-stu-id="22761-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="22761-139">[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)描述完整的這個行為。</span><span class="sxs-lookup"><span data-stu-id="22761-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="22761-140">例如，若要產生的部署指令碼**ContactManager**資料庫而不實際部署的資料庫，VSDBCMD 命令應該看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="22761-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="22761-141">VSDBCMD 是差異資料庫部署工具，並因此產生部署指令碼是以動態方式以包含所有的 SQL 命令需要更新目前資料庫中，如果存在的話，指定的結構描述。</span><span class="sxs-lookup"><span data-stu-id="22761-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="22761-142">檢閱部署指令碼是一個實用的方式，來判斷項目會影響您的部署會對目前的資料庫和它所包含的資料。</span><span class="sxs-lookup"><span data-stu-id="22761-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="22761-143">例如，您可以判斷：</span><span class="sxs-lookup"><span data-stu-id="22761-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="22761-144">是否將會移除任何現有的資料表，以及是否會導致資料遺失。</span><span class="sxs-lookup"><span data-stu-id="22761-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="22761-145">是否作業的順序會帶來風險的資料遺失，比方說，如果您要分割或合併的資料表。</span><span class="sxs-lookup"><span data-stu-id="22761-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="22761-146">如果您是部署指令碼感到滿意，您可以重複使用 VSDBCMD **/dd+** 旗標，以進行變更。</span><span class="sxs-lookup"><span data-stu-id="22761-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="22761-147">或者，您可以編輯的部署指令碼，以符合您的需求，然後在資料庫伺服器上手動執行它。</span><span class="sxs-lookup"><span data-stu-id="22761-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="22761-148">將"What If"功能整合到自訂的專案檔</span><span class="sxs-lookup"><span data-stu-id="22761-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="22761-149">在更複雜的部署案例中，您會想要來封裝您的組建和部署邏輯，使用自訂的 Microsoft Build Engine (MSBuild) 專案檔，如中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。</span><span class="sxs-lookup"><span data-stu-id="22761-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="22761-150">例如，在[Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案， *Publish.proj*檔案：</span><span class="sxs-lookup"><span data-stu-id="22761-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="22761-151">建置方案。</span><span class="sxs-lookup"><span data-stu-id="22761-151">Builds the solution.</span></span>
- <span data-ttu-id="22761-152">使用 Web Deploy 封裝和部署 ContactManager.Mvc 應用程式。</span><span class="sxs-lookup"><span data-stu-id="22761-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="22761-153">使用 Web Deploy 封裝和部署 ContactManager.Service 應用程式。</span><span class="sxs-lookup"><span data-stu-id="22761-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="22761-154">部署**ContactManager**資料庫。</span><span class="sxs-lookup"><span data-stu-id="22761-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="22761-155">當您將多個 web 封裝及/或資料庫的部署整合的單一步驟程序，如此一來時，您也可以選擇在 「 假設 」 模式中執行整個部署。</span><span class="sxs-lookup"><span data-stu-id="22761-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="22761-156">*Publish.proj*檔會示範如何可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="22761-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="22761-157">首先，您必須建立要儲存 「 假設 」 值的屬性：</span><span class="sxs-lookup"><span data-stu-id="22761-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="22761-158">在此情況下，您已建立名為的屬性**WhatIf**預設值是**false**。</span><span class="sxs-lookup"><span data-stu-id="22761-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="22761-159">使用者可以覆寫此值的屬性設定為 **，則為 true**在命令列參數中，您很快會看到。</span><span class="sxs-lookup"><span data-stu-id="22761-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="22761-160">下一個階段是參數化 Web Deploy 和 VSDBCMD 命令，讓旗標會反映**WhatIf**屬性值。</span><span class="sxs-lookup"><span data-stu-id="22761-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="22761-161">例如下, 一個目標 (取自*Publish.proj*檔案，並簡化) 執行 *。 deploy.cmd*將 web 套件部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="22761-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="22761-162">根據預設，此命令包含 **/Y**參數 （"yes"或更新模式）。</span><span class="sxs-lookup"><span data-stu-id="22761-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="22761-163">如果**WhatIf**設為 **，則為 true**，將會取代運算式 **/T** （試用版或 「 假設 」 模式） 的參數。</span><span class="sxs-lookup"><span data-stu-id="22761-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="22761-164">同樣地下, 一個目標會將資料庫部署中，使用 VSDBCMD 公用程式。</span><span class="sxs-lookup"><span data-stu-id="22761-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="22761-165">根據預設， **/dd**未包含參數。</span><span class="sxs-lookup"><span data-stu-id="22761-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="22761-166">這表示 VSDBCMD 會產生部署指令碼，但並不會部署資料庫&#x2014;也就是說，「 假設 」 案例。</span><span class="sxs-lookup"><span data-stu-id="22761-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="22761-167">如果**WhatIf**屬性未設定為 **，則為 true**，則 **/dd**會加入參數，而且 VSDBCMD 會將資料庫部署。</span><span class="sxs-lookup"><span data-stu-id="22761-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="22761-168">您可以使用相同的方法，專案檔中的所有相關的命令參數化。</span><span class="sxs-lookup"><span data-stu-id="22761-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="22761-169">當您想要執行 「 假設 」 部署時，您可以接著只需要提供**WhatIf**屬性值，從命令列：</span><span class="sxs-lookup"><span data-stu-id="22761-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="22761-170">如此一來，您可以執行"what if"部署在單一步驟中所有專案元件。</span><span class="sxs-lookup"><span data-stu-id="22761-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="22761-171">結論</span><span class="sxs-lookup"><span data-stu-id="22761-171">Conclusion</span></span>

<span data-ttu-id="22761-172">本主題說明如何執行 「 假設 」 使用 Web Deploy、 VSDBCMD 和 MSBuild 的部署。</span><span class="sxs-lookup"><span data-stu-id="22761-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="22761-173">"What if"部署可讓您評估建議的部署的影響，實際對目的地環境的任何變更之前。</span><span class="sxs-lookup"><span data-stu-id="22761-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="22761-174">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="22761-174">Further Reading</span></span>

<span data-ttu-id="22761-175">如需有關 Web Deploy 命令列語法的詳細資訊，請參閱 < [Web Deploy 作業設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="22761-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="22761-176">如需命令列選項，當您使用的指引 *。 deploy.cmd*檔案，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。</span><span class="sxs-lookup"><span data-stu-id="22761-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="22761-177">如需 VSDBCMD 命令列語法的指引，請參閱[VSDBCMD 的命令列參考。（「 部署 」 和 「 結構描述匯入 」） 的 EXE](https://msdn.microsoft.com/library/dd193283.aspx)。</span><span class="sxs-lookup"><span data-stu-id="22761-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="22761-178">[上一頁](advanced-enterprise-web-deployment.md)
> [下一頁](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="22761-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
