---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: 用於 Web 套件部署中設定參數 |Microsoft 文件
author: jrjlee
description: 本主題描述如何設定參數值，例如網際網路資訊服務 (IIS) web 應用程式名稱、 連接字串，以及服務端點...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7be08f1a1fb7232911a44cf64e2e784dbb95ff48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880397"
---
<a name="configuring-parameters-for-web-package-deployment"></a><span data-ttu-id="a9683-103">用於 Web 套件部署中設定參數</span><span class="sxs-lookup"><span data-stu-id="a9683-103">Configuring Parameters for Web Package Deployment</span></span>
====================
<span data-ttu-id="a9683-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a9683-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a9683-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="a9683-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a9683-106">本主題描述如何設定參數值，例如網際網路資訊服務 (IIS) web 應用程式名稱、 連接字串，以及服務端點，當您將 web 套件部署到遠端的 IIS 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9683-106">This topic describes how to set parameter values, like Internet Information Services (IIS) web application names, connection strings, and service endpoints, when you deploy a web package to a remote IIS web server.</span></span>


<span data-ttu-id="a9683-107">當您建立 web 應用程式專案，建置和封裝程序會產生三個索引鍵的檔案：</span><span class="sxs-lookup"><span data-stu-id="a9683-107">When you build a web application project, the build and packaging process generates three key files:</span></span>

- <span data-ttu-id="a9683-108">A *[專案名稱].zip*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-108">A *[project name].zip* file.</span></span> <span data-ttu-id="a9683-109">這是 web 應用程式專案的 web 部署套件。</span><span class="sxs-lookup"><span data-stu-id="a9683-109">This is the web deployment package for your web application project.</span></span> <span data-ttu-id="a9683-110">此套件包含所有組件、 檔案、 資料庫指令碼和重新建立您的 web 應用程式，遠端的 IIS 網頁伺服器上所需的資源。</span><span class="sxs-lookup"><span data-stu-id="a9683-110">This package contains all the assemblies, files, database scripts, and resources required to recreate your web application on a remote IIS web server.</span></span>
- <span data-ttu-id="a9683-111">A *[專案名稱].deploy.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-111">A *[project name].deploy.cmd* file.</span></span> <span data-ttu-id="a9683-112">這包含一組的參數化 Web Deploy (MSDeploy.exe) 命令，將您的 web 部署套件發佈到遠端的 IIS 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9683-112">This contains a set of parameterized Web Deploy (MSDeploy.exe) commands that publish your web deployment package to a remote IIS web server.</span></span>
- <span data-ttu-id="a9683-113">A *[專案名稱]。SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-113">A *[project name].SetParameters.xml* file.</span></span> <span data-ttu-id="a9683-114">這會提供一組 MSDeploy.exe 命令的參數值。</span><span class="sxs-lookup"><span data-stu-id="a9683-114">This provides a set of parameter values to the MSDeploy.exe command.</span></span> <span data-ttu-id="a9683-115">您可以更新這個檔案中的值，並將它傳遞至 Web Deploy 做為命令列參數部署 web 封裝時。</span><span class="sxs-lookup"><span data-stu-id="a9683-115">You can update the values in this file and pass it to Web Deploy as a command-line parameter when you deploy your web package.</span></span>

> [!NOTE]
> <span data-ttu-id="a9683-116">如需有關建置和封裝程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="a9683-116">For more information on the build and packaging process, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>


<span data-ttu-id="a9683-117">*SetParameters.xml*從您的 web 應用程式專案檔和任何組態檔，在專案中的動態產生檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-117">The *SetParameters.xml* file is dynamically generated from your web application project file and any configuration files within your project.</span></span> <span data-ttu-id="a9683-118">當您建置並封裝您的專案，Web 發行管線 (WPP) 會自動偵測許多可能會變更部署的環境，例如目的 IIS web 應用程式之間的任何資料庫連接字串的變數。</span><span class="sxs-lookup"><span data-stu-id="a9683-118">When you build and package your project, the Web Publishing Pipeline (WPP) will automatically detect lots of the variables that are likely to change between deployment environments, like the destination IIS web application and any database connection strings.</span></span> <span data-ttu-id="a9683-119">這些值會自動參數化 web 部署套件中，並新增到*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-119">These values are automatically parameterized in the web deployment package and added to the *SetParameters.xml* file.</span></span> <span data-ttu-id="a9683-120">例如，如果您加入的連接字串*web.config*檔案在 web 應用程式專案中，在建置程序會偵測這項變更，並將新增的項目，以*SetParameters.xml*檔案據以。</span><span class="sxs-lookup"><span data-stu-id="a9683-120">For example, if you add a connection string to the *web.config* file in your web application project, the build process will detect this change and will add an entry to the *SetParameters.xml* file accordingly.</span></span>

<span data-ttu-id="a9683-121">在許多情況下，此自動參數化會足夠。</span><span class="sxs-lookup"><span data-stu-id="a9683-121">In a lot of cases, this automatic parameterization will be sufficient.</span></span> <span data-ttu-id="a9683-122">不過，如果您的使用者需要變更其他設定之間部署環境，例如應用程式設定或服務端點 Url，您需要告訴部署套件中的這些值參數化，並將對應的項目加入至WPP*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-122">However, if your users need to vary other settings between deployment environments, like application settings or service endpoint URLs, you need to tell the WPP to parameterize these values in the deployment package and add corresponding entries to the *SetParameters.xml* file.</span></span> <span data-ttu-id="a9683-123">以下各節說明如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="a9683-123">The sections that follow explain how to do this.</span></span>

### <a name="automatic-parameterization"></a><span data-ttu-id="a9683-124">自動參數化</span><span class="sxs-lookup"><span data-stu-id="a9683-124">Automatic Parameterization</span></span>

<span data-ttu-id="a9683-125">當您建置並封裝 web 應用程式時，WPP 將自動參數化這些項目：</span><span class="sxs-lookup"><span data-stu-id="a9683-125">When you build and package a web application, the WPP will automatically parameterize these things:</span></span>

- <span data-ttu-id="a9683-126">目的地 IIS web 應用程式路徑和名稱。</span><span class="sxs-lookup"><span data-stu-id="a9683-126">The destination IIS web application path and name.</span></span>
- <span data-ttu-id="a9683-127">中的任何連接字串您*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-127">Any connection strings in your *web.config* file.</span></span>
- <span data-ttu-id="a9683-128">您將加入至任何資料庫的連接字串**封裝/發行 SQL**在專案屬性頁 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a9683-128">Connection strings for any databases you add to the **Package/Publish SQL** tab in the project property pages.</span></span>

<span data-ttu-id="a9683-129">例如，如果您要建置並封裝[連絡人管理員](the-contact-manager-solution.md)範例方案，但沒有碰觸的參數化程序，以任何方式，WPP 仍會產生這*ContactManager.Mvc.SetParameters.xml*檔案：</span><span class="sxs-lookup"><span data-stu-id="a9683-129">For example, if you were to build and package the [Contact Manager](the-contact-manager-solution.md) sample solution without touching the parameterization process in any way, the WPP would generate this *ContactManager.Mvc.SetParameters.xml* file:</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


<span data-ttu-id="a9683-130">在此情況下：</span><span class="sxs-lookup"><span data-stu-id="a9683-130">In this case:</span></span>

- <span data-ttu-id="a9683-131">**IIS Web 應用程式名稱**參數是您要部署 web 應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="a9683-131">The **IIS Web Application Name** parameter is the IIS path where you want to deploy the web application.</span></span> <span data-ttu-id="a9683-132">預設值取自**封裝/發行 Web**專案屬性頁中的頁面。</span><span class="sxs-lookup"><span data-stu-id="a9683-132">The default value is taken from the **Package/Publish Web** page in the project property pages.</span></span>
- <span data-ttu-id="a9683-133">**ApplicationServices Web.config 連接字串**參數產生自**connectionStrings/加入**中的項目*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-133">The **ApplicationServices-Web.config Connection String** parameter was generated from a **connectionStrings/add** element in the *web.config* file.</span></span> <span data-ttu-id="a9683-134">它代表應用程式應該使用連絡成員資格資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="a9683-134">It represents the connection string that the application should use to contact the membership database.</span></span> <span data-ttu-id="a9683-135">這裡將您所提供的值取代已部署至*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-135">The value you provide here will be substituted into the deployed *web.config* file.</span></span> <span data-ttu-id="a9683-136">預設值取自預先部署*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-136">The default value is taken from the pre-deployment *web.config* file.</span></span>

<span data-ttu-id="a9683-137">WPP 也會參數化這些屬性，它會產生的部署套件中。</span><span class="sxs-lookup"><span data-stu-id="a9683-137">The WPP also parameterizes these properties in the deployment package it generates.</span></span> <span data-ttu-id="a9683-138">當您安裝的部署套件時，您便可以提供這些屬性的值。</span><span class="sxs-lookup"><span data-stu-id="a9683-138">You can provide values for these properties when you install the deployment package.</span></span> <span data-ttu-id="a9683-139">如果您安裝封裝手動透過 IIS 管理員 中所述[手動安裝 Web 封裝](manually-installing-web-packages.md)，安裝精靈會提示您提供任何參數的值。</span><span class="sxs-lookup"><span data-stu-id="a9683-139">If you install the package manually through IIS Manager, as described in [Manually Installing Web Packages](manually-installing-web-packages.md), the installation wizard prompts you to provide values for any parameters.</span></span> <span data-ttu-id="a9683-140">如果您使用安裝套件遠端 *。 deploy.cmd*檔案中所述[部署 Web 封裝](deploying-web-packages.md)，Web Deploy 會尋找此*SetParameters.xml*檔案提供參數值。</span><span class="sxs-lookup"><span data-stu-id="a9683-140">If you install the package remotely using the *.deploy.cmd* file, as described in [Deploying Web Packages](deploying-web-packages.md), Web Deploy will look to this *SetParameters.xml* file to provide the parameter values.</span></span> <span data-ttu-id="a9683-141">您可以編輯中的值*SetParameters.xml*手動檔案，或者您可以自訂檔案做為自動化組建和部署程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="a9683-141">You can edit the values in the *SetParameters.xml* file manually, or you can customize the file as part of an automated build and deployment process.</span></span> <span data-ttu-id="a9683-142">此程序所述在本主題稍後的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a9683-142">This process is described in more detail later in this topic.</span></span>

### <a name="custom-parameterization"></a><span data-ttu-id="a9683-143">自訂參數化</span><span class="sxs-lookup"><span data-stu-id="a9683-143">Custom Parameterization</span></span>

<span data-ttu-id="a9683-144">在更複雜的部署案例中，您通常會想要參數化的其他屬性，才能部署您的專案。</span><span class="sxs-lookup"><span data-stu-id="a9683-144">In more complex deployment scenarios, you'll often want to parameterize additional properties before you deploy your project.</span></span> <span data-ttu-id="a9683-145">一般而言，您應該參數化任何屬性與目的地環境之間而異的設定。</span><span class="sxs-lookup"><span data-stu-id="a9683-145">Generally speaking, you should parameterize any properties and settings that will vary between destination environments.</span></span> <span data-ttu-id="a9683-146">這些可以包括：</span><span class="sxs-lookup"><span data-stu-id="a9683-146">These can include:</span></span>

- <span data-ttu-id="a9683-147">服務端點中的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-147">Service endpoints in the *web.config* file.</span></span>
- <span data-ttu-id="a9683-148">中的應用程式設定*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-148">Application settings in the *web.config* file.</span></span>
- <span data-ttu-id="a9683-149">任何其他宣告式屬性，您想要提示使用者指定。</span><span class="sxs-lookup"><span data-stu-id="a9683-149">Any other declarative properties that you want to prompt users to specify.</span></span>

<span data-ttu-id="a9683-150">參數化這些屬性的最簡單方式是加入*parameters.xml*檔案到您的 web 應用程式專案的根資料夾。</span><span class="sxs-lookup"><span data-stu-id="a9683-150">The easiest way to parameterize these properties is to add a *parameters.xml* file to the root folder of your web application project.</span></span> <span data-ttu-id="a9683-151">例如，在連絡人管理員解決方案中，ContactManager.Mvc 專案包含*parameters.xml*根資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-151">For example, in the Contact Manager solution, the ContactManager.Mvc project includes a *parameters.xml* file in the root folder.</span></span>

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

<span data-ttu-id="a9683-152">如果您開啟這個檔案，您會看到它包含單一**參數**項目。</span><span class="sxs-lookup"><span data-stu-id="a9683-152">If you open this file, you'll see that it contains a single **parameter** entry.</span></span> <span data-ttu-id="a9683-153">項目會使用 XML 路徑語言 (XPath) 查詢來找出並參數化的 ContactService Windows Communication Foundation (WCF) 服務的端點 URL *web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-153">The entry uses an XML Path Language (XPath) query to locate and parameterize the endpoint URL of the ContactService Windows Communication Foundation (WCF) service in the *web.config* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


<span data-ttu-id="a9683-154">除了將部署套件中的端點 URL 參數化，WPP 也會加入至對應的項目*SetParameters.xml*取得部署套件與產生的檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-154">In addition to parameterizing the endpoint URL in the deployment package, the WPP also adds a corresponding entry to the *SetParameters.xml* file that gets generated alongside the deployment package.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


<span data-ttu-id="a9683-155">如果您手動安裝的部署套件時，IIS 管理員會提示您輸入的服務端點位址，連同已自動參數化的屬性。</span><span class="sxs-lookup"><span data-stu-id="a9683-155">If you install the deployment package manually, IIS Manager will prompt you for the service endpoint address alongside the properties that were parameterized automatically.</span></span> <span data-ttu-id="a9683-156">如果您執行安裝的部署套件 *。 deploy.cmd*檔案中，您可以編輯*SetParameters.xml*檔案，以提供的服務端點位址，以及值的值已自動參數化的屬性。</span><span class="sxs-lookup"><span data-stu-id="a9683-156">If you install the deployment package by running the *.deploy.cmd* file, you can edit the *SetParameters.xml* file to provide a value for the service endpoint address together with values for the properties that were parameterized automatically.</span></span>

<span data-ttu-id="a9683-157">如需如何建立完整細節*parameters.xml*檔案，請參閱[How to： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/library/ff398068.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a9683-157">For full details on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="a9683-158">名為程序**部署參數用於 Web.config 檔案設定**提供逐步指示。</span><span class="sxs-lookup"><span data-stu-id="a9683-158">The procedure named **To use deployment parameters for Web.config file settings** provides step-by-step instructions.</span></span>

## <a name="modifying-the-setparametersxml-file"></a><span data-ttu-id="a9683-159">修改 SetParameters.xml 檔案</span><span class="sxs-lookup"><span data-stu-id="a9683-159">Modifying the SetParameters.xml File</span></span>

<span data-ttu-id="a9683-160">如果您打算以手動方式部署 web 應用程式封裝&#x2014;藉由執行 *。 deploy.cmd*檔案或從命令列執行 MSDeploy.exe&#x2014;沒有阻止您以手動方式編輯*SetParameters.xml*之前部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-160">If you plan to deploy the web application package manually&#x2014;either by running the *.deploy.cmd* file or by running MSDeploy.exe from the command line&#x2014;there's nothing to stop you manually editing the *SetParameters.xml* file prior to the deployment.</span></span> <span data-ttu-id="a9683-161">不過，如果您使用企業規模方案，您可能需要較大，自動化組建和部署程序的一部分部署 web 應用程式套件。</span><span class="sxs-lookup"><span data-stu-id="a9683-161">However, if you're working on an enterprise-scale solution, you may need to deploy a web application package as part of a larger, automated build and deployment process.</span></span> <span data-ttu-id="a9683-162">在此案例中，您需要 Microsoft Build Engine (MSBuild) 修改*SetParameters.xml*為您的檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-162">In this scenario, you need the Microsoft Build Engine (MSBuild) to modify the *SetParameters.xml* file for you.</span></span> <span data-ttu-id="a9683-163">您可以使用 MSBuild **XmlPoke**工作。</span><span class="sxs-lookup"><span data-stu-id="a9683-163">You can do this by using the MSBuild **XmlPoke** task.</span></span>

<span data-ttu-id="a9683-164">[連絡人管理員範例方案](the-contact-manager-solution.md)說明此程序。</span><span class="sxs-lookup"><span data-stu-id="a9683-164">The [Contact Manager sample solution](the-contact-manager-solution.md) illustrates this process.</span></span> <span data-ttu-id="a9683-165">以下程式碼範例都已經過編輯，以顯示與此範例的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="a9683-165">The code examples that follow have been edited to show only the details that are relevant to this example.</span></span>

> [!NOTE]
> <span data-ttu-id="a9683-166">如需更廣泛的專案檔案模型中的範例解決方案，以及自訂的專案檔的一般簡介概觀，請參閱[了解專案檔](understanding-the-project-file.md)和[瞭解建置程序](understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="a9683-166">For a broader overview of the project file model in the sample solution, and an introduction to custom project files in general, see [Understanding the Project File](understanding-the-project-file.md) and [Understanding the Build Process](understanding-the-build-process.md).</span></span>


<span data-ttu-id="a9683-167">首先，您感興趣的參數值會定義為特定環境的專案檔中的屬性 (例如， *Env Dev.proj*)。</span><span class="sxs-lookup"><span data-stu-id="a9683-167">First, the parameter values of interest are defined as properties in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> <span data-ttu-id="a9683-168">如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="a9683-168">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="a9683-169">下一步 *Publish.proj*檔匯入這些屬性。</span><span class="sxs-lookup"><span data-stu-id="a9683-169">Next, the *Publish.proj* file imports these properties.</span></span> <span data-ttu-id="a9683-170">因為每個*SetParameters.xml*與檔案關聯 *。 deploy.cmd*檔案，以及我們最終需要在專案檔以便可以叫用每個 *。 deploy.cmd*檔案，專案檔案建立 MSBuild*項目*每個 *。 deploy.cmd*檔案，並定義為感興趣的屬性*項目中繼資料*。</span><span class="sxs-lookup"><span data-stu-id="a9683-170">Because each *SetParameters.xml* file is associated with a *.deploy.cmd* file, and we ultimately want the project file to invoke each *.deploy.cmd* file, the project file creates an MSBuild *item* for each *.deploy.cmd* file and defines the properties of interest as *item metadata*.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


<span data-ttu-id="a9683-171">在此情況下：</span><span class="sxs-lookup"><span data-stu-id="a9683-171">In this case:</span></span>

- <span data-ttu-id="a9683-172">**ParametersXml**中繼資料值表示的位置*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-172">The **ParametersXml** metadata value indicates the location of the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="a9683-173">**IisWebAppName**值是您要部署 web 應用程式的 IIS 路徑。</span><span class="sxs-lookup"><span data-stu-id="a9683-173">The **IisWebAppName** value is the IIS path to which you want to deploy the web application.</span></span>
- <span data-ttu-id="a9683-174">**MembershipDBConnectionString**值是成員資格資料庫中，連接字串和**MembershipDBConnectionName**值是**名稱**屬性中的對應參數*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-174">The **MembershipDBConnectionString** value is the connection string for the membership database, and the **MembershipDBConnectionName** value is the **name** attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="a9683-175">**ServiceEndpointValue**值是目的地伺服器上的 WCF 服務的端點位址和**ServiceEndpointParamName**值是中的對應參數的名稱屬性*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-175">The **ServiceEndpointValue** value is the endpoint address for the WCF service on the destination server, and the **ServiceEndpointParamName** value is the name attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>

<span data-ttu-id="a9683-176">最後，在*Publish.proj*檔案， **PublishWebPackages**目標使用**XmlPoke**工作來修改這些值在*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-176">Finally, in the *Publish.proj* file, the **PublishWebPackages** target uses the **XmlPoke** task to modify these values in the *SetParameters.xml* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


<span data-ttu-id="a9683-177">您將會注意到，每個**XmlPoke**工作指定四個屬性值：</span><span class="sxs-lookup"><span data-stu-id="a9683-177">You'll notice that each **XmlPoke** task specifies four attribute values:</span></span>

- <span data-ttu-id="a9683-178">**XmlInputPath**屬性會告知工作如何尋找您想要修改的檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-178">The **XmlInputPath** attribute tells the task where to find the file you want to modify.</span></span>
- <span data-ttu-id="a9683-179">**查詢**屬性是識別您想要變更的 XML 節點的 XPath 查詢。</span><span class="sxs-lookup"><span data-stu-id="a9683-179">The **Query** attribute is an XPath query that identifies the XML node you want to change.</span></span>
- <span data-ttu-id="a9683-180">**值**屬性是您想要插入選取的 XML 節點的新值。</span><span class="sxs-lookup"><span data-stu-id="a9683-180">The **Value** attribute is the new value you want to insert into the selected XML node.</span></span>
- <span data-ttu-id="a9683-181">**條件**是屬性的準則，工作應該在其執行，或無法執行。</span><span class="sxs-lookup"><span data-stu-id="a9683-181">The **Condition** attribute is the criteria on which the task should run or not run.</span></span> <span data-ttu-id="a9683-182">在這些情況下，條件可確保您不要嘗試 null 或空白值插入*SetParameters.xml*檔案。</span><span class="sxs-lookup"><span data-stu-id="a9683-182">In these cases, the condition ensures that you don't try to insert a null or empty value into the *SetParameters.xml* file.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a9683-183">結論</span><span class="sxs-lookup"><span data-stu-id="a9683-183">Conclusion</span></span>

<span data-ttu-id="a9683-184">本主題所述的角色*SetParameters.xml*檔案，並說明其產生的方法時建立的 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="a9683-184">This topic described the role of the *SetParameters.xml* file and explained how it's generated when you build a web application project.</span></span> <span data-ttu-id="a9683-185">它說明如何將其他設定參數化加入*parameters.xml*檔案加入專案。</span><span class="sxs-lookup"><span data-stu-id="a9683-185">It explained how you can parameterize additional settings by adding a *parameters.xml* file to your project.</span></span> <span data-ttu-id="a9683-186">它也說明如何修改*SetParameters.xml*檔案的較大的自動建置程序，使用為**XmlPoke**專案檔中的工作。</span><span class="sxs-lookup"><span data-stu-id="a9683-186">It also described how you can modify the *SetParameters.xml* file as part of a larger, automated build process, by using the **XmlPoke** task in your project files.</span></span>

<span data-ttu-id="a9683-187">下一個主題[部署 Web 封裝](deploying-web-packages.md)，說明如何部署 web 封裝藉由執行 *。 deploy.cmd*檔案，或直接使用 MSDeploy.exe 命令。</span><span class="sxs-lookup"><span data-stu-id="a9683-187">The next topic, [Deploying Web Packages](deploying-web-packages.md), describes how you can deploy a web package either by running the *.deploy.cmd* file or by using MSDeploy.exe commands directly.</span></span> <span data-ttu-id="a9683-188">在這兩種情況下，您可以指定您*SetParameters.xml*檔案做為部署參數。</span><span class="sxs-lookup"><span data-stu-id="a9683-188">In both cases, you can specify your *SetParameters.xml* file as a deployment parameter.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a9683-189">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="a9683-189">Further Reading</span></span>

<span data-ttu-id="a9683-190">如需如何建立 web 封裝的資訊，請參閱[建置和封裝 Web 應用程式專案](building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="a9683-190">For information on how to create web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="a9683-191">如需如何部署 web 封裝的指引，請參閱[部署 Web 封裝](deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="a9683-191">For guidance on how to actually deploy a web package, see [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="a9683-192">如需如何建立的逐步解說*parameters.xml*檔案，請參閱[How to： 使用參數來設定部署設定時封裝會安裝](https://msdn.microsoft.com/library/ff398068.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a9683-192">For a step-by-step walkthrough on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span>

<span data-ttu-id="a9683-193">一般參數化的 Web Deploy 的詳細資訊，請參閱[動作中的 Web 部署參數化](https://go.microsoft.com/?linkid=9805119)（部落格文章）。</span><span class="sxs-lookup"><span data-stu-id="a9683-193">For more general information on parameterization in Web Deploy, see [Web Deploy Parameterization in Action](https://go.microsoft.com/?linkid=9805119) (blog post).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9683-194">[上一頁](building-and-packaging-web-application-projects.md)
> [下一頁](deploying-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="a9683-194">[Previous](building-and-packaging-web-application-projects.md)
[Next](deploying-web-packages.md)</span></span>
