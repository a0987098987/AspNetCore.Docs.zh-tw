---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: 設定權限的小組組建部署 |Microsoft Docs
author: jrjlee
description: 本主題描述如何設定以啟用您的組建伺服器，做為自動化的 b 的一部分時，將內容部署至 web 伺服器和資料庫伺服器的權限...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0142be694a4e7d601625022f6fbfe39971823d03
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830512"
---
<a name="configuring-permissions-for-team-build-deployment"></a><span data-ttu-id="f60e0-103">組建部署小組設定權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-103">Configuring Permissions for Team Build Deployment</span></span>
====================
<span data-ttu-id="f60e0-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="f60e0-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="f60e0-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="f60e0-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="f60e0-106">本主題描述如何設定以啟用您的自動化的建置程序的一部分時，將內容部署至 web 伺服器和資料庫伺服器的組建伺服器的權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-106">This topic describes how to configure permissions to enable your build server to deploy content to web servers and database servers as part of an automated build process.</span></span>


<span data-ttu-id="f60e0-107">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="f60e0-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="f60e0-108">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="f60e0-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="f60e0-109">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="f60e0-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="f60e0-110">工作概觀</span><span class="sxs-lookup"><span data-stu-id="f60e0-110">Task Overview</span></span>

<span data-ttu-id="f60e0-111">當您安裝 Team Foundation Server (TFS) 2010年組建服務時，您會指定要用來執行服務的身分識別。</span><span class="sxs-lookup"><span data-stu-id="f60e0-111">When you install the Team Foundation Server (TFS) 2010 build service, you specify the identity with which you want the service to run.</span></span> <span data-ttu-id="f60e0-112">根據預設，這是在 Network Service 帳戶。</span><span class="sxs-lookup"><span data-stu-id="f60e0-112">By default, this is the Network Service account.</span></span> <span data-ttu-id="f60e0-113">或者，您可以設定為使用網域帳戶來執行組建服務。</span><span class="sxs-lookup"><span data-stu-id="f60e0-113">Alternatively, you can configure the build service to run using a domain account.</span></span>

<span data-ttu-id="f60e0-114">任何需要 Windows 驗證，並想要自動化 Team Build，所使用的部署工作會使用組建服務身分識別來執行。</span><span class="sxs-lookup"><span data-stu-id="f60e0-114">Any deployment tasks that require Windows authentication, and that you plan to automate using Team Build, will run using the build service identity.</span></span> <span data-ttu-id="f60e0-115">因此，您必須在您的 web 伺服器和資料庫伺服器上任何必要的權限授與組建服務身分識別。</span><span class="sxs-lookup"><span data-stu-id="f60e0-115">As such, you'll need to grant the build service identity any required permissions on your web servers and your database servers.</span></span>

> [!NOTE]
> <span data-ttu-id="f60e0-116">網路服務帳戶使用電腦帳戶來向其他電腦。</span><span class="sxs-lookup"><span data-stu-id="f60e0-116">The Network Service account uses the machine account to authenticate to other computers.</span></span> <span data-ttu-id="f60e0-117">電腦帳戶的形式 * [網域名稱]\[電腦名稱] ***$**&#x2014;，例如**FABRIKAM\TFSBUILD$**。</span><span class="sxs-lookup"><span data-stu-id="f60e0-117">Machine accounts take the form *[domain name]\[machine name]***$**&#x2014;for example, **FABRIKAM\TFSBUILD$**.</span></span> <span data-ttu-id="f60e0-118">同樣地，如果您的組建服務執行使用 Network Service 身分識別，您應該授與的電腦帳戶識別任何必要權限為您的組建伺服器。</span><span class="sxs-lookup"><span data-stu-id="f60e0-118">As such, if your build service runs using the Network Service identity, you should grant any required permissions to the machine account identity for your build server.</span></span>


## <a name="configuring-web-server-permissions"></a><span data-ttu-id="f60e0-119">設定網頁伺服器權限</span><span class="sxs-lookup"><span data-stu-id="f60e0-119">Configuring Web Server Permissions</span></span>

<span data-ttu-id="f60e0-120">中所述[選擇 Web 部署的權限方法](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)，有兩種主要的方法，如果您想要將 web 套件部署至遠端網頁伺服器，您可以使用：</span><span class="sxs-lookup"><span data-stu-id="f60e0-120">As described in [Choosing the Right Approach to Web Deployment](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), there are two main approaches you can use if you want to deploy web packages to a remote web server:</span></span>

- <span data-ttu-id="f60e0-121">將從遠端位置的應用程式部署將目標設為*Web Deployment Agent Service* （也稱為遠端代理程式） 在目的地伺服器上。</span><span class="sxs-lookup"><span data-stu-id="f60e0-121">Deploy the application from a remote location by targeting the *Web Deployment Agent Service* (also known as the remote agent) on the destination server.</span></span>
- <span data-ttu-id="f60e0-122">將從遠端位置的應用程式部署將目標設為*Internet Information Services* (*IIS) Web 部署的處理常式*目的地伺服器上。</span><span class="sxs-lookup"><span data-stu-id="f60e0-122">Deploy the application from a remote location by targeting the *Internet Information Services* (*IIS) Web Deploy Handler* on the destination server.</span></span>

<span data-ttu-id="f60e0-123">遠端代理程式在此情況下有兩個主要限制：</span><span class="sxs-lookup"><span data-stu-id="f60e0-123">The remote agent has two key limitations in this case:</span></span>

- <span data-ttu-id="f60e0-124">遠端代理程式只支援 NTLM 驗證。</span><span class="sxs-lookup"><span data-stu-id="f60e0-124">The remote agent supports only NTLM authentication.</span></span> <span data-ttu-id="f60e0-125">換句話說，此部署必須使用組建服務身分識別&#x2014;您無法模擬另一個帳戶。</span><span class="sxs-lookup"><span data-stu-id="f60e0-125">In other words, the deployment must use the build service identity&#x2014;you can't impersonate another account.</span></span>
- <span data-ttu-id="f60e0-126">若要使用遠端代理程式，請執行部署的帳戶必須是目標伺服器的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="f60e0-126">To use the remote agent, the account that performs the deployment must be an administrator on the target server.</span></span>

<span data-ttu-id="f60e0-127">放在一起，這些兩個的限制進行遠端代理程式的方法不想要自動化 Team Build 部署。</span><span class="sxs-lookup"><span data-stu-id="f60e0-127">Together, these two limitations make the remote agent approach undesirable for an automated Team Build deployment.</span></span> <span data-ttu-id="f60e0-128">若要使用這種方法，您必須進行組建服務帳戶在任何目標 web 伺服器上的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="f60e0-128">To use this approach, you'd need to make the build service account an administrator on any target web servers.</span></span>

<span data-ttu-id="f60e0-129">相反地，Web 部署的處理常式方法會提供各種不同的優點：</span><span class="sxs-lookup"><span data-stu-id="f60e0-129">In contrast, the Web Deploy Handler approach offers various advantages:</span></span>

- <span data-ttu-id="f60e0-130">Web 部署處理常式支援基本驗證，透過 HTTPS，可讓您將替代帳戶的認證傳遞至 IIS Web Deployment Tool (Web Deploy)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-130">The Web Deploy Handler supports basic authentication over HTTPS, which allows you to pass the credentials of an alternative account to the IIS Web Deployment Tool (Web Deploy).</span></span>
- <span data-ttu-id="f60e0-131">您可以設定目標 web 伺服器，以允許非系統管理員的使用者，將內容部署至特定的 IIS 網站，使用 Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="f60e0-131">You can configure target web servers to allow non-administrator users to deploy content to specific IIS websites using the Web Deploy Handler.</span></span>

<span data-ttu-id="f60e0-132">如此一來，最好是清楚地將目標設 Web 部署處理常式，當您從 Team Build web 套件部署自動化。</span><span class="sxs-lookup"><span data-stu-id="f60e0-132">As a result, it's clearly preferable to target the Web Deploy Handler when you automate web package deployment from Team Build.</span></span> <span data-ttu-id="f60e0-133">這是建議的程序：</span><span class="sxs-lookup"><span data-stu-id="f60e0-133">This is the recommended process:</span></span>

1. <span data-ttu-id="f60e0-134">建立要用於部署的低權限的網域帳戶。</span><span class="sxs-lookup"><span data-stu-id="f60e0-134">Create a low-privileged domain account to use for the deployment.</span></span>
2. <span data-ttu-id="f60e0-135">設定 Web 部署處理常式，並授與帳戶必要的權限，將內容部署至特定的 IIS 網站，如中所述[設定 Web 伺服器的 Web 部署發行 （Web 部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-135">Configure the Web Deploy Handler and grant the account the required permissions to deploy content to a specific IIS website, as described in [Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span>
3. <span data-ttu-id="f60e0-136">叫用 Web Deploy 和目標 Web 部署處理常式時，使用基本驗證，並提供網域帳戶的認證建立，若要執行部署。</span><span class="sxs-lookup"><span data-stu-id="f60e0-136">Invoke Web Deploy and target the Web Deploy Handler, using basic authentication and supplying the credentials of the domain account you created, to perform the deployment.</span></span>

<span data-ttu-id="f60e0-137">在  [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案，指定驗證類型 (基本或 NTLM)，Web Deploy 認證，以及環境特定專案檔中的端點位址 （遠端代理程式或 Web 部署的處理常式）。</span><span class="sxs-lookup"><span data-stu-id="f60e0-137">In the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, you specify the authentication type (basic or NTLM), the Web Deploy credentials, and the endpoint address (remote agent or Web Deploy Handler) in the environment-specific project file.</span></span> <span data-ttu-id="f60e0-138">這些值用來編寫和執行專案檔時，執行 Web Deploy 命令。</span><span class="sxs-lookup"><span data-stu-id="f60e0-138">These values are used to formulate and run a Web Deploy command when the project file is executed.</span></span> <span data-ttu-id="f60e0-139">如需詳細資訊，請參閱 <<c0> [ 部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-139">For more information, see [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

<span data-ttu-id="f60e0-140">如需有關設定 Web 部署處理常式，包括如何設定權限，請參閱[設定 Web 伺服器的 Web 部署發行 （Web 部署的處理常式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-140">For more information on configuring the Web Deploy Handler, including how to configure permissions, see [Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span> <span data-ttu-id="f60e0-141">如需有關設定遠端代理程式的詳細資訊，請參閱 <<c0> [ 設定 Web 伺服器的 Web 部署發行 （遠端代理程式）](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-141">For more information on configuring the remote agent, see [Configuring a Web Server for Web Deploy Publishing (Remote Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span>

## <a name="configuring-database-server-permissions"></a><span data-ttu-id="f60e0-142">設定資料庫伺服器的權限</span><span class="sxs-lookup"><span data-stu-id="f60e0-142">Configuring Database Server Permissions</span></span>

<span data-ttu-id="f60e0-143">若要將資料庫部署到 SQL Server 中，您必須：</span><span class="sxs-lookup"><span data-stu-id="f60e0-143">To deploy a database to SQL Server, you must:</span></span>

- <span data-ttu-id="f60e0-144">SQL Server 執行個體上建立之部署的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="f60e0-144">Create a login for the deploying account on the SQL Server instance.</span></span>
- <span data-ttu-id="f60e0-145">授與登入**DBCreator** SQL Server 執行個體上的權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-145">Grant the login **DBCreator** permissions on the SQL Server instance.</span></span>
- <span data-ttu-id="f60e0-146">初始部署之後，新增 登入**db\_擁有者**目標資料庫上的角色。</span><span class="sxs-lookup"><span data-stu-id="f60e0-146">After the initial deployment, add the login to the **db\_owner** role on the target database.</span></span> <span data-ttu-id="f60e0-147">這是必要的因為在後續的部署中，您要修改現有的資料庫而建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f60e0-147">This is required because on subsequent deployments, you're modifying an existing database rather than creating a new database.</span></span>

<span data-ttu-id="f60e0-148">您可以驗證使用 NTLM 驗證或 SQL Server 驗證的 SQL Server 執行個體：</span><span class="sxs-lookup"><span data-stu-id="f60e0-148">You can authenticate to a SQL Server instance using either NTLM authentication or SQL Server authentication:</span></span>

- <span data-ttu-id="f60e0-149">如果您使用 NTLM 驗證時，您需要授與上述組建服務帳戶的權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-149">If you use NTLM authentication, you need to grant the permissions described above to the build service account.</span></span>
- <span data-ttu-id="f60e0-150">如果您使用 SQL Server 驗證時，您需要授與上面所述，SQL Server 帳戶的權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-150">If you use SQL Server authentication, you need to grant the permissions described above to the SQL Server account.</span></span> <span data-ttu-id="f60e0-151">您也需要您使用部署資料庫的連接字串中包含的 SQL Server 使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="f60e0-151">You also need to include the SQL Server user name and password in the connection string you use to deploy the database.</span></span>

<span data-ttu-id="f60e0-152">如需如何設定資料庫部署的權限的逐步詳細資訊，請參閱[設定資料庫伺服器的 Web 部署發佈](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-152">For step-by-step details on how to configure permissions for database deployment, see [Configuring a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

## <a name="conclusion"></a><span data-ttu-id="f60e0-153">結論</span><span class="sxs-lookup"><span data-stu-id="f60e0-153">Conclusion</span></span>

<span data-ttu-id="f60e0-154">此時，您應該了解時所需，驗證選項，開啟您 web 應用程式與資料庫從將部署自動化 Team Build 的權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-154">At this point, you should understand the permissions required, together with the authentication options open to you, when you automate web application and database deployments from Team Build.</span></span> <span data-ttu-id="f60e0-155">您也應該能夠在 IIS web 伺服器和 SQL Server 資料庫伺服器上實作必要的權限。</span><span class="sxs-lookup"><span data-stu-id="f60e0-155">You should also be able to implement the necessary permissions on IIS web servers and SQL Server database servers.</span></span>

## <a name="further-reading"></a><span data-ttu-id="f60e0-156">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="f60e0-156">Further Reading</span></span>

<span data-ttu-id="f60e0-157">如需有關如何設定 Windows server 環境，以支援遠端部署的詳細資訊，請參閱 <<c0> [ 設定 Web 部署的伺服器環境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="f60e0-157">For more information on configuring Windows server environments to support remote deployment, see [Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f60e0-158">上一步</span><span class="sxs-lookup"><span data-stu-id="f60e0-158">Previous</span></span>](deploying-a-specific-build.md)
