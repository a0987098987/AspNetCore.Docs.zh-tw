---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: 設定網頁套件部署的參數 |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定參數值，例如 Internet Information Services (IIS) web 應用程式名稱、 連接字串和服務端點...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386545"
---
<a name="configuring-parameters-for-web-package-deployment"></a><span data-ttu-id="d8b31-103">網頁套件部署的設定參數</span><span class="sxs-lookup"><span data-stu-id="d8b31-103">Configuring Parameters for Web Package Deployment</span></span>
====================
<span data-ttu-id="d8b31-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d8b31-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d8b31-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="d8b31-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d8b31-106">本主題描述如何設定參數值，Internet Information Services (IIS) web 應用程式名稱、 連接字串和服務端點，例如，當您將 web 套件部署到遠端 IIS web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d8b31-106">This topic describes how to set parameter values, like Internet Information Services (IIS) web application names, connection strings, and service endpoints, when you deploy a web package to a remote IIS web server.</span></span>


<span data-ttu-id="d8b31-107">當您建置的 web 應用程式專案、 建置和封裝程序會產生三個索引鍵的檔案：</span><span class="sxs-lookup"><span data-stu-id="d8b31-107">When you build a web application project, the build and packaging process generates three key files:</span></span>

- <span data-ttu-id="d8b31-108">A *[專案名稱].zip*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-108">A *[project name].zip* file.</span></span> <span data-ttu-id="d8b31-109">這是 web 應用程式專案的 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="d8b31-109">This is the web deployment package for your web application project.</span></span> <span data-ttu-id="d8b31-110">此套件包含所有組件、 檔案、 資料庫指令碼和重新建立您的 web 應用程式，遠端的 IIS 網頁伺服器上所需的資源。</span><span class="sxs-lookup"><span data-stu-id="d8b31-110">This package contains all the assemblies, files, database scripts, and resources required to recreate your web application on a remote IIS web server.</span></span>
- <span data-ttu-id="d8b31-111">A *[專案名稱].deploy.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-111">A *[project name].deploy.cmd* file.</span></span> <span data-ttu-id="d8b31-112">這包含一組參數化 Web Deploy (MSDeploy.exe) 命令，將您的 web 部署套件發佈至遠端 IIS web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d8b31-112">This contains a set of parameterized Web Deploy (MSDeploy.exe) commands that publish your web deployment package to a remote IIS web server.</span></span>
- <span data-ttu-id="d8b31-113">A *[專案名稱]。之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-113">A *[project name].SetParameters.xml* file.</span></span> <span data-ttu-id="d8b31-114">這提供一組 MSDeploy.exe 命令的參數值。</span><span class="sxs-lookup"><span data-stu-id="d8b31-114">This provides a set of parameter values to the MSDeploy.exe command.</span></span> <span data-ttu-id="d8b31-115">您可以更新此檔案中的值，並將它傳遞至 Web Deploy 命令列參數為當您將 web 套件部署。</span><span class="sxs-lookup"><span data-stu-id="d8b31-115">You can update the values in this file and pass it to Web Deploy as a command-line parameter when you deploy your web package.</span></span>

> [!NOTE]
> <span data-ttu-id="d8b31-116">如需有關建置和封裝程序的詳細資訊，請參閱 <<c0> [ 建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-116">For more information on the build and packaging process, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>


<span data-ttu-id="d8b31-117">*之 SetParameters.xml*從您的 web 應用程式專案檔和您的專案內的任何設定檔，以動態方式產生檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-117">The *SetParameters.xml* file is dynamically generated from your web application project file and any configuration files within your project.</span></span> <span data-ttu-id="d8b31-118">當您建置並封裝您的專案中，Web 發行管線 (WPP) 會自動偵測許多可能會變更部署的環境，例如目的地 IIS web 應用程式之間的任何資料庫連接字串的變數。</span><span class="sxs-lookup"><span data-stu-id="d8b31-118">When you build and package your project, the Web Publishing Pipeline (WPP) will automatically detect lots of the variables that are likely to change between deployment environments, like the destination IIS web application and any database connection strings.</span></span> <span data-ttu-id="d8b31-119">這些值會自動參數化的 web 部署套件中，並加入*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-119">These values are automatically parameterized in the web deployment package and added to the *SetParameters.xml* file.</span></span> <span data-ttu-id="d8b31-120">例如，如果您加入的連接字串*web.config*檔案中您的 web 應用程式專案，建置流程會偵測到這項變更並將項目新增至*之 SetParameters.xml*檔案據此。</span><span class="sxs-lookup"><span data-stu-id="d8b31-120">For example, if you add a connection string to the *web.config* file in your web application project, the build process will detect this change and will add an entry to the *SetParameters.xml* file accordingly.</span></span>

<span data-ttu-id="d8b31-121">在許多情況下，此自動參數化會足夠。</span><span class="sxs-lookup"><span data-stu-id="d8b31-121">In a lot of cases, this automatic parameterization will be sufficient.</span></span> <span data-ttu-id="d8b31-122">不過，如果您的使用者需要變更其他設定之間部署環境，例如應用程式設定] 或 [服務端點 Url，您需要告訴參數化的部署套件中的這些值，並將對應的項目加入至WPP*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-122">However, if your users need to vary other settings between deployment environments, like application settings or service endpoint URLs, you need to tell the WPP to parameterize these values in the deployment package and add corresponding entries to the *SetParameters.xml* file.</span></span> <span data-ttu-id="d8b31-123">下列各節說明如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="d8b31-123">The sections that follow explain how to do this.</span></span>

### <a name="automatic-parameterization"></a><span data-ttu-id="d8b31-124">自動參數化</span><span class="sxs-lookup"><span data-stu-id="d8b31-124">Automatic Parameterization</span></span>

<span data-ttu-id="d8b31-125">當您建置和封裝 web 應用程式時，WPP，將會自動將這些項目：</span><span class="sxs-lookup"><span data-stu-id="d8b31-125">When you build and package a web application, the WPP will automatically parameterize these things:</span></span>

- <span data-ttu-id="d8b31-126">目的地 IIS web 應用程式路徑和名稱。</span><span class="sxs-lookup"><span data-stu-id="d8b31-126">The destination IIS web application path and name.</span></span>
- <span data-ttu-id="d8b31-127">任何連接字串中您*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-127">Any connection strings in your *web.config* file.</span></span>
- <span data-ttu-id="d8b31-128">您將新增至任何資料庫的連接字串**封裝/發行 SQL**在專案屬性頁 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d8b31-128">Connection strings for any databases you add to the **Package/Publish SQL** tab in the project property pages.</span></span>

<span data-ttu-id="d8b31-129">例如，如果您要建置和封裝[Contactmanager](the-contact-manager-solution.md)不必以任何方式，WPP 參數化程序的範例解決方案會產生這個*ContactManager.Mvc.SetParameters.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="d8b31-129">For example, if you were to build and package the [Contact Manager](the-contact-manager-solution.md) sample solution without touching the parameterization process in any way, the WPP would generate this *ContactManager.Mvc.SetParameters.xml* file:</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


<span data-ttu-id="d8b31-130">在此情況下：</span><span class="sxs-lookup"><span data-stu-id="d8b31-130">In this case:</span></span>

- <span data-ttu-id="d8b31-131">**IIS Web 應用程式名稱**參數是您要部署 web 應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="d8b31-131">The **IIS Web Application Name** parameter is the IIS path where you want to deploy the web application.</span></span> <span data-ttu-id="d8b31-132">預設值取自**封裝/發行 Web**專案屬性頁中的頁面。</span><span class="sxs-lookup"><span data-stu-id="d8b31-132">The default value is taken from the **Package/Publish Web** page in the project property pages.</span></span>
- <span data-ttu-id="d8b31-133">**ApplicationServices 連接字串**參數所產生**connectionStrings/新增**中的項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-133">The **ApplicationServices-Web.config Connection String** parameter was generated from a **connectionStrings/add** element in the *web.config* file.</span></span> <span data-ttu-id="d8b31-134">它代表應用程式應該使用連絡成員資格資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="d8b31-134">It represents the connection string that the application should use to contact the membership database.</span></span> <span data-ttu-id="d8b31-135">將會是您所提供的值替代至已部署*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-135">The value you provide here will be substituted into the deployed *web.config* file.</span></span> <span data-ttu-id="d8b31-136">預設值取自部署前*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-136">The default value is taken from the pre-deployment *web.config* file.</span></span>

<span data-ttu-id="d8b31-137">WPP 也會參數化這些屬性，它會產生的部署套件中。</span><span class="sxs-lookup"><span data-stu-id="d8b31-137">The WPP also parameterizes these properties in the deployment package it generates.</span></span> <span data-ttu-id="d8b31-138">當您安裝的部署套件時，您便可以提供這些屬性的值。</span><span class="sxs-lookup"><span data-stu-id="d8b31-138">You can provide values for these properties when you install the deployment package.</span></span> <span data-ttu-id="d8b31-139">如果您安裝套件以手動方式透過 IIS 管理員 中所述[手動安裝網頁套件](manually-installing-web-packages.md)，安裝精靈會提示您提供任何參數的值。</span><span class="sxs-lookup"><span data-stu-id="d8b31-139">If you install the package manually through IIS Manager, as described in [Manually Installing Web Packages](manually-installing-web-packages.md), the installation wizard prompts you to provide values for any parameters.</span></span> <span data-ttu-id="d8b31-140">如果您安裝從遠端使用的套件 *。 deploy.cmd*檔案中所述[部署網頁套件](deploying-web-packages.md)，Web Deploy 起來至此*之 SetParameters.xml*檔案提供參數值。</span><span class="sxs-lookup"><span data-stu-id="d8b31-140">If you install the package remotely using the *.deploy.cmd* file, as described in [Deploying Web Packages](deploying-web-packages.md), Web Deploy will look to this *SetParameters.xml* file to provide the parameter values.</span></span> <span data-ttu-id="d8b31-141">您可以編輯中的值*之 SetParameters.xml*檔案以手動的方式，或您可以自訂檔案自動化建置和部署程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="d8b31-141">You can edit the values in the *SetParameters.xml* file manually, or you can customize the file as part of an automated build and deployment process.</span></span> <span data-ttu-id="d8b31-142">此程序所述本主題稍後的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d8b31-142">This process is described in more detail later in this topic.</span></span>

### <a name="custom-parameterization"></a><span data-ttu-id="d8b31-143">自訂參數化</span><span class="sxs-lookup"><span data-stu-id="d8b31-143">Custom Parameterization</span></span>

<span data-ttu-id="d8b31-144">在更複雜的部署案例中，您通常會想要參數化的其他屬性，然後再部署您的專案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-144">In more complex deployment scenarios, you'll often want to parameterize additional properties before you deploy your project.</span></span> <span data-ttu-id="d8b31-145">一般而言，您應該參數化任何屬性與目的地環境之間而異的設定。</span><span class="sxs-lookup"><span data-stu-id="d8b31-145">Generally speaking, you should parameterize any properties and settings that will vary between destination environments.</span></span> <span data-ttu-id="d8b31-146">這些可能包括：</span><span class="sxs-lookup"><span data-stu-id="d8b31-146">These can include:</span></span>

- <span data-ttu-id="d8b31-147">服務中的端點*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-147">Service endpoints in the *web.config* file.</span></span>
- <span data-ttu-id="d8b31-148">中的應用程式設定*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-148">Application settings in the *web.config* file.</span></span>
- <span data-ttu-id="d8b31-149">任何其他宣告式屬性，您想要提示使用者指定。</span><span class="sxs-lookup"><span data-stu-id="d8b31-149">Any other declarative properties that you want to prompt users to specify.</span></span>

<span data-ttu-id="d8b31-150">參數化這些屬性的最簡單方式是將*無效*web 應用程式專案的根資料夾的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-150">The easiest way to parameterize these properties is to add a *parameters.xml* file to the root folder of your web application project.</span></span> <span data-ttu-id="d8b31-151">比方說，在 連絡管理員解決方案，ContactManager.Mvc 專案包含*無效*根資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-151">For example, in the Contact Manager solution, the ContactManager.Mvc project includes a *parameters.xml* file in the root folder.</span></span>

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

<span data-ttu-id="d8b31-152">如果您開啟此檔案時，您會看到它包含單一**參數**項目。</span><span class="sxs-lookup"><span data-stu-id="d8b31-152">If you open this file, you'll see that it contains a single **parameter** entry.</span></span> <span data-ttu-id="d8b31-153">項目會使用 XML 路徑語言 (XPath) 查詢來找出並參數化中的 ContactService Windows Communication Foundation (WCF) 服務的端點 URL *web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-153">The entry uses an XML Path Language (XPath) query to locate and parameterize the endpoint URL of the ContactService Windows Communication Foundation (WCF) service in the *web.config* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


<span data-ttu-id="d8b31-154">除了參數化部署套件中的端點 URL，WPP 也新增至對應的項目*之 SetParameters.xml*隨部署套件產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-154">In addition to parameterizing the endpoint URL in the deployment package, the WPP also adds a corresponding entry to the *SetParameters.xml* file that gets generated alongside the deployment package.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


<span data-ttu-id="d8b31-155">如果您手動安裝的部署套件時，IIS 管理員 會提示您的服務端點位址，以及已自動參數化的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8b31-155">If you install the deployment package manually, IIS Manager will prompt you for the service endpoint address alongside the properties that were parameterized automatically.</span></span> <span data-ttu-id="d8b31-156">如果您執行安裝的部署套件 *。 deploy.cmd*檔案中，您可以編輯*之 SetParameters.xml*檔案，以提供的服務端點位址，以及值的值已自動參數化的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8b31-156">If you install the deployment package by running the *.deploy.cmd* file, you can edit the *SetParameters.xml* file to provide a value for the service endpoint address together with values for the properties that were parameterized automatically.</span></span>

<span data-ttu-id="d8b31-157">如需如何建立完整的詳細資訊*無效*檔案，請參閱[如何： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/library/ff398068.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-157">For full details on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="d8b31-158">名為程序**要用於 Web.config 檔案設定的部署參數**提供逐步指示。</span><span class="sxs-lookup"><span data-stu-id="d8b31-158">The procedure named **To use deployment parameters for Web.config file settings** provides step-by-step instructions.</span></span>

## <a name="modifying-the-setparametersxml-file"></a><span data-ttu-id="d8b31-159">修改之 SetParameters.xml 檔案</span><span class="sxs-lookup"><span data-stu-id="d8b31-159">Modifying the SetParameters.xml File</span></span>

<span data-ttu-id="d8b31-160">如果您打算以手動方式部署 web 應用程式封裝&#x2014;藉由執行 *。 deploy.cmd*檔案或從命令列執行 MSDeploy.exe&#x2014;沒有要阻止您以手動方式編輯*之 SetParameters.xml*之前部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-160">If you plan to deploy the web application package manually&#x2014;either by running the *.deploy.cmd* file or by running MSDeploy.exe from the command line&#x2014;there's nothing to stop you manually editing the *SetParameters.xml* file prior to the deployment.</span></span> <span data-ttu-id="d8b31-161">不過，如果您正在操作的企業級解決方案，您可能需要將 web 應用程式封裝部署為更大的自動化建置和部署程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="d8b31-161">However, if you're working on an enterprise-scale solution, you may need to deploy a web application package as part of a larger, automated build and deployment process.</span></span> <span data-ttu-id="d8b31-162">在此案例中，您需要 Microsoft Build Engine (MSBuild) 來修改*之 SetParameters.xml*為您的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-162">In this scenario, you need the Microsoft Build Engine (MSBuild) to modify the *SetParameters.xml* file for you.</span></span> <span data-ttu-id="d8b31-163">您可以使用 MSBuild **XmlPoke**工作。</span><span class="sxs-lookup"><span data-stu-id="d8b31-163">You can do this by using the MSBuild **XmlPoke** task.</span></span>

<span data-ttu-id="d8b31-164">[Contact Manager 範例方案](the-contact-manager-solution.md)說明此程序。</span><span class="sxs-lookup"><span data-stu-id="d8b31-164">The [Contact Manager sample solution](the-contact-manager-solution.md) illustrates this process.</span></span> <span data-ttu-id="d8b31-165">請依照下列的程式碼範例有經過編輯，以顯示與此範例的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d8b31-165">The code examples that follow have been edited to show only the details that are relevant to this example.</span></span>

> [!NOTE]
> <span data-ttu-id="d8b31-166">專案檔案模型中的範例解決方案，以及自訂的專案檔的一般簡介的廣泛概觀，請參閱 <<c0> [ 了解專案檔](understanding-the-project-file.md)並[了解建置程序](understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-166">For a broader overview of the project file model in the sample solution, and an introduction to custom project files in general, see [Understanding the Project File](understanding-the-project-file.md) and [Understanding the Build Process](understanding-the-build-process.md).</span></span>


<span data-ttu-id="d8b31-167">首先，感興趣的參數值會定義為環境特有的專案檔中的屬性 (例如*Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-167">First, the parameter values of interest are defined as properties in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> <span data-ttu-id="d8b31-168">如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-168">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="d8b31-169">下一步 *Publish.proj*檔匯入這些屬性。</span><span class="sxs-lookup"><span data-stu-id="d8b31-169">Next, the *Publish.proj* file imports these properties.</span></span> <span data-ttu-id="d8b31-170">因為每個*之 SetParameters.xml*檔案相關聯 *。 deploy.cmd*檔案，以及我們最終想要叫用每個專案檔 *。 deploy.cmd*檔案，專案檔案會建立 MSBuild*項目*每個 *。 deploy.cmd*檔案，並為想要的屬性會定義*項目中繼資料*。</span><span class="sxs-lookup"><span data-stu-id="d8b31-170">Because each *SetParameters.xml* file is associated with a *.deploy.cmd* file, and we ultimately want the project file to invoke each *.deploy.cmd* file, the project file creates an MSBuild *item* for each *.deploy.cmd* file and defines the properties of interest as *item metadata*.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


<span data-ttu-id="d8b31-171">在此情況下：</span><span class="sxs-lookup"><span data-stu-id="d8b31-171">In this case:</span></span>

- <span data-ttu-id="d8b31-172">**ParametersXml**中繼資料值表示的位置*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-172">The **ParametersXml** metadata value indicates the location of the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="d8b31-173">**IisWebAppName**值是您要部署 web 應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="d8b31-173">The **IisWebAppName** value is the IIS path to which you want to deploy the web application.</span></span>
- <span data-ttu-id="d8b31-174">**MembershipDBConnectionString**值為連接字串的成員資格資料庫中，而**MembershipDBConnectionName**值是**名稱**屬性中的對應參數*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-174">The **MembershipDBConnectionString** value is the connection string for the membership database, and the **MembershipDBConnectionName** value is the **name** attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="d8b31-175">**ServiceEndpointValue**值為在目的地伺服器上的 WCF 服務的端點位址和**ServiceEndpointParamName**值是 name 屬性中的對應參數*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-175">The **ServiceEndpointValue** value is the endpoint address for the WCF service on the destination server, and the **ServiceEndpointParamName** value is the name attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>

<span data-ttu-id="d8b31-176">最後，在*Publish.proj*檔案， **PublishWebPackages**目標會使用**XmlPoke**工作來修改這些值在*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-176">Finally, in the *Publish.proj* file, the **PublishWebPackages** target uses the **XmlPoke** task to modify these values in the *SetParameters.xml* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


<span data-ttu-id="d8b31-177">您會在每個發現**XmlPoke**工作可讓您指定四個屬性值：</span><span class="sxs-lookup"><span data-stu-id="d8b31-177">You'll notice that each **XmlPoke** task specifies four attribute values:</span></span>

- <span data-ttu-id="d8b31-178">**XmlInputPath**屬性會告知工作哪裡可以找到您想要修改的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-178">The **XmlInputPath** attribute tells the task where to find the file you want to modify.</span></span>
- <span data-ttu-id="d8b31-179">**查詢**屬性會識別您想要變更的 XML 節點的 XPath 查詢。</span><span class="sxs-lookup"><span data-stu-id="d8b31-179">The **Query** attribute is an XPath query that identifies the XML node you want to change.</span></span>
- <span data-ttu-id="d8b31-180">**值**屬性是您想要插入至所選取的 XML 節點的新值。</span><span class="sxs-lookup"><span data-stu-id="d8b31-180">The **Value** attribute is the new value you want to insert into the selected XML node.</span></span>
- <span data-ttu-id="d8b31-181">**條件**屬性是工作應該在其執行，或無法執行的準則。</span><span class="sxs-lookup"><span data-stu-id="d8b31-181">The **Condition** attribute is the criteria on which the task should run or not run.</span></span> <span data-ttu-id="d8b31-182">在這些情況下，條件可確保您不要嘗試為 null 或空白值插入*之 SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-182">In these cases, the condition ensures that you don't try to insert a null or empty value into the *SetParameters.xml* file.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d8b31-183">結論</span><span class="sxs-lookup"><span data-stu-id="d8b31-183">Conclusion</span></span>

<span data-ttu-id="d8b31-184">本主題所述的角色*之 SetParameters.xml*檔案，並說明如何產生當您建置的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-184">This topic described the role of the *SetParameters.xml* file and explained how it's generated when you build a web application project.</span></span> <span data-ttu-id="d8b31-185">它說明如何將其他設定參數化加*無效*檔案加入專案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-185">It explained how you can parameterize additional settings by adding a *parameters.xml* file to your project.</span></span> <span data-ttu-id="d8b31-186">它也說明如何修改*之 SetParameters.xml*更大的自動化建置程序，利用一部分的檔案**XmlPoke**專案檔中的工作。</span><span class="sxs-lookup"><span data-stu-id="d8b31-186">It also described how you can modify the *SetParameters.xml* file as part of a larger, automated build process, by using the **XmlPoke** task in your project files.</span></span>

<span data-ttu-id="d8b31-187">下一個主題中，[部署網頁套件](deploying-web-packages.md)，說明如何部署網頁套件藉由執行 *。 deploy.cmd*檔案，或直接透過 MSDeploy.exe 命令。</span><span class="sxs-lookup"><span data-stu-id="d8b31-187">The next topic, [Deploying Web Packages](deploying-web-packages.md), describes how you can deploy a web package either by running the *.deploy.cmd* file or by using MSDeploy.exe commands directly.</span></span> <span data-ttu-id="d8b31-188">在這兩種情況下，您可以指定您*之 SetParameters.xml*作為部署參數的檔案。</span><span class="sxs-lookup"><span data-stu-id="d8b31-188">In both cases, you can specify your *SetParameters.xml* file as a deployment parameter.</span></span>

## <a name="further-reading"></a><span data-ttu-id="d8b31-189">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="d8b31-189">Further Reading</span></span>

<span data-ttu-id="d8b31-190">如需如何建立 web 封裝的資訊，請參閱[建置和封裝 Web Application Projects](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-190">For information on how to create web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="d8b31-191">如需如何實際部署網頁套件的指引，請參閱[部署網頁套件](deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-191">For guidance on how to actually deploy a web package, see [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="d8b31-192">如需如何建立的逐步解說*無效*檔案，請參閱[如何： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/library/ff398068.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d8b31-192">For a step-by-step walkthrough on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span>

<span data-ttu-id="d8b31-193">更多一般參數化的 Web Deploy 的詳細資訊，請參閱[動作中的 Web 部署參數化](https://go.microsoft.com/?linkid=9805119)（部落格文章）。</span><span class="sxs-lookup"><span data-stu-id="d8b31-193">For more general information on parameterization in Web Deploy, see [Web Deploy Parameterization in Action](https://go.microsoft.com/?linkid=9805119) (blog post).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8b31-194">[上一頁](building-and-packaging-web-application-projects.md)
> [下一頁](deploying-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="d8b31-194">[Previous](building-and-packaging-web-application-projects.md)
[Next](deploying-web-packages.md)</span></span>
