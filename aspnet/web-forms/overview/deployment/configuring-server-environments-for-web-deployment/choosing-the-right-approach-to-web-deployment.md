---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: 選擇 Web 部署的正確方法 |Microsoft Docs
author: jrjlee
description: 當您使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三種主要的方法，可用來取得...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367741"
---
<a name="choosing-the-right-approach-to-web-deployment"></a><span data-ttu-id="8855a-103">選擇 Web 部署的正確方法</span><span class="sxs-lookup"><span data-stu-id="8855a-103">Choosing the Right Approach to Web Deployment</span></span>
====================
<span data-ttu-id="8855a-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="8855a-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="8855a-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="8855a-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="8855a-106">當您使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 或更新版本時，有三種主要的方法，可用來取得封裝的 web 應用程式到 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="8855a-106">When you work with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your packaged web applications onto a web server.</span></span> <span data-ttu-id="8855a-107">您可以：</span><span class="sxs-lookup"><span data-stu-id="8855a-107">You can either:</span></span>
> 
> - <span data-ttu-id="8855a-108">將從遠端位置的應用程式部署將目標設為*Web Deployment Agent Service* （也稱為 「 遠端代理程式 」） 在目的地伺服器上。</span><span class="sxs-lookup"><span data-stu-id="8855a-108">Deploy the application from a remote location by targeting the *Web Deployment Agent Service* (also known as the "remote agent") on the destination server.</span></span>
> - <span data-ttu-id="8855a-109">部署應用程式從遠端位置使用 Web 部署 On Demand （也稱為 「 暫存代理程式 」）。</span><span class="sxs-lookup"><span data-stu-id="8855a-109">Deploy the application from a remote location using Web Deploy On Demand (also known as the "temp agent").</span></span>
> - <span data-ttu-id="8855a-110">將從遠端位置的應用程式部署將目標設為*IIS Web 部署處理常式*目的地伺服器上。</span><span class="sxs-lookup"><span data-stu-id="8855a-110">Deploy the application from a remote location by targeting the *IIS Web Deploy Handler* on the destination server.</span></span>
> - <span data-ttu-id="8855a-111">部署應用程式，以手動方式將 web 套件複製到目的地伺服器，並透過 IIS 管理員匯入。</span><span class="sxs-lookup"><span data-stu-id="8855a-111">Deploy the application by manually copying the web package to the destination server and importing it through IIS Manager.</span></span>
> 
> <span data-ttu-id="8855a-112">設定您的目的地 web 伺服器的方式將取決於您想要使用哪種方法來部署。</span><span class="sxs-lookup"><span data-stu-id="8855a-112">How you configure your destination web servers will depend on which approach to deployment you want to use.</span></span> <span data-ttu-id="8855a-113">本主題將協助您決定要部署哪種方法最適合您。</span><span class="sxs-lookup"><span data-stu-id="8855a-113">This topic will help you decide which approach to deployment is right for you.</span></span>


<span data-ttu-id="8855a-114">下表顯示主要的優點和缺點，每個部署方法，以及最常配合每一種方法的案例。</span><span class="sxs-lookup"><span data-stu-id="8855a-114">This table shows the main advantages and disadvantages of each deployment approach, together with the scenarios that most typically suit each approach.</span></span>

| <span data-ttu-id="8855a-115">方法</span><span class="sxs-lookup"><span data-stu-id="8855a-115">Approach</span></span> | <span data-ttu-id="8855a-116">優點</span><span class="sxs-lookup"><span data-stu-id="8855a-116">Advantages</span></span> | <span data-ttu-id="8855a-117">缺點</span><span class="sxs-lookup"><span data-stu-id="8855a-117">Disadvantages</span></span> | <span data-ttu-id="8855a-118">典型的案例</span><span class="sxs-lookup"><span data-stu-id="8855a-118">Typical Scenarios</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8855a-119">遠端代理程式</span><span class="sxs-lookup"><span data-stu-id="8855a-119">Remote Agent</span></span> | <span data-ttu-id="8855a-120">它很容易設定。</span><span class="sxs-lookup"><span data-stu-id="8855a-120">It is easy to set up.</span></span> <span data-ttu-id="8855a-121">它是適合 web 應用程式和內容的定期更新。</span><span class="sxs-lookup"><span data-stu-id="8855a-121">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="8855a-122">使用者必須是目標伺服器的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="8855a-122">The user must be an administrator on the target server.</span></span> <span data-ttu-id="8855a-123">使用者無法提供替代的認證。</span><span class="sxs-lookup"><span data-stu-id="8855a-123">the user can't supply alternative credentials.</span></span> | <span data-ttu-id="8855a-124">開發環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-124">Development environments.</span></span> <span data-ttu-id="8855a-125">測試環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-125">Test environments.</span></span> |
| <span data-ttu-id="8855a-126">暫存的代理程式</span><span class="sxs-lookup"><span data-stu-id="8855a-126">Temp Agent</span></span> | <span data-ttu-id="8855a-127">沒有需要在目標電腦上安裝 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="8855a-127">There is no need to install Web Deploy on the target computer.</span></span> <span data-ttu-id="8855a-128">會自動使用最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="8855a-128">The latest version of Web Deploy is automatically used.</span></span> | <span data-ttu-id="8855a-129">使用者必須是目標伺服器的系統管理員。</span><span class="sxs-lookup"><span data-stu-id="8855a-129">The user must be an administrator on the target server.</span></span> <span data-ttu-id="8855a-130">使用者無法提供替代的認證。</span><span class="sxs-lookup"><span data-stu-id="8855a-130">The user can't supply alternative credentials.</span></span> | <span data-ttu-id="8855a-131">開發環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-131">Development environments.</span></span> <span data-ttu-id="8855a-132">測試環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-132">Test environments.</span></span> |
| <span data-ttu-id="8855a-133">Web Deploy 處理常式</span><span class="sxs-lookup"><span data-stu-id="8855a-133">Web Deploy Handler</span></span> | <span data-ttu-id="8855a-134">非系統管理員使用者可以將內容部署。</span><span class="sxs-lookup"><span data-stu-id="8855a-134">Non-administrator users can deploy content.</span></span> <span data-ttu-id="8855a-135">它是適合 web 應用程式和內容的定期更新。</span><span class="sxs-lookup"><span data-stu-id="8855a-135">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="8855a-136">它是設為更加複雜。</span><span class="sxs-lookup"><span data-stu-id="8855a-136">It is a lot more complex to set up.</span></span> | <span data-ttu-id="8855a-137">預備環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-137">Staging environments.</span></span> <span data-ttu-id="8855a-138">內部網路的實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-138">Intranet production environments.</span></span> <span data-ttu-id="8855a-139">主控的環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-139">Hosted environments.</span></span> |
| <span data-ttu-id="8855a-140">離線部署</span><span class="sxs-lookup"><span data-stu-id="8855a-140">Offline Deployment</span></span> | <span data-ttu-id="8855a-141">它是非常容易設定。</span><span class="sxs-lookup"><span data-stu-id="8855a-141">It is very easy to set up.</span></span> <span data-ttu-id="8855a-142">它適合隔離的環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-142">It is suitable for isolated environments.</span></span> | <span data-ttu-id="8855a-143">伺服器系統管理員必須手動複製並匯入 web 封裝每次。</span><span class="sxs-lookup"><span data-stu-id="8855a-143">The server administrator must manually copy and import the web package every time.</span></span> | <span data-ttu-id="8855a-144">網際網路對向的生產環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-144">Internet-facing production environments.</span></span> <span data-ttu-id="8855a-145">隔離的網路環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-145">Isolated network environments.</span></span> |
  

## <a name="using-the-remote-agent"></a><span data-ttu-id="8855a-146">使用遠端代理程式</span><span class="sxs-lookup"><span data-stu-id="8855a-146">Using the Remote Agent</span></span>

<span data-ttu-id="8855a-147">當您安裝 Web Deploy 使用目的地伺服器上的預設設定時，Web Deployment Agent Service （「 遠端代理程式 」） 會自動安裝並啟動。</span><span class="sxs-lookup"><span data-stu-id="8855a-147">When you install Web Deploy using the default settings on a destination server, the Web Deployment Agent Service (the "remote agent") is automatically installed and started.</span></span> <span data-ttu-id="8855a-148">根據預設，遠端代理程式會公開 HTTP 端點，在此位址：</span><span class="sxs-lookup"><span data-stu-id="8855a-148">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="8855a-149">您可以取代 [*server*] 與您的 web 伺服器的電腦名稱，您的 web 伺服器或主機名稱的 IP 位址解析為您的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="8855a-149">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="8855a-150">伺服器系統管理員可以部署 web 封裝，從遠端位置，例如開發人員電腦或組建伺服器，藉由指定這個端點位址。</span><span class="sxs-lookup"><span data-stu-id="8855a-150">Server administrators can deploy web packages from a remote location, like a developer machine or a build server, by specifying this endpoint address.</span></span> <span data-ttu-id="8855a-151">例如，假設 Fabrikam，inc.的 Matt 世昕他的開發人員機器上建置 ContactManager.Mvc web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="8855a-151">For example, suppose Matt Hink at Fabrikam, Inc. has built the ContactManager.Mvc web application project on his developer machine.</span></span> <span data-ttu-id="8855a-152">建置程序會產生 web 封裝，並搭配 *。 deploy.cmd*安裝套件所需的檔案，其中包含的 Web Deploy 命令。</span><span class="sxs-lookup"><span data-stu-id="8855a-152">The build process generates a web package, together with a *.deploy.cmd* file that contains the Web Deploy commands required to install the package.</span></span> <span data-ttu-id="8855a-153">如果 Matt 是常青州立大學 TESTWEB1 伺服器上的伺服器管理員，他可以部署來測試 web 伺服器的 web 應用程式開發人員機器上執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8855a-153">If Matt is a server administrator on the TESTWEB1 server, he can deploy the web application to the test web server by running this command on his developer machine:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


<span data-ttu-id="8855a-154">實際事實上，如果您提供的電腦名稱，因此 Matt 只需要鍵入此 Web Deploy 的可執行檔時，可以推斷遠端代理程式的端點位址：</span><span class="sxs-lookup"><span data-stu-id="8855a-154">In actual fact, the Web Deploy executable can infer the endpoint address of the remote agent if you provide the machine name, so Matt only needs to type this:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="8855a-155">如需有關 Web Deploy 命令列語法和 *。 deploy.cmd*檔，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-155">For more information on Web Deploy command-line syntax and *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="8855a-156">遠端代理程式提供直接的方法，從遠端位置，部署內容，這種方法也可以使用一種單鍵或自動部署。</span><span class="sxs-lookup"><span data-stu-id="8855a-156">The remote agent offers a straightforward way to deploy content from a remote location, and this approach can work well with one-click or automated deployment.</span></span> <span data-ttu-id="8855a-157">不過，執行部署命令的使用者也必須是網域系統管理員或目的地伺服器上的本機 administrators 群組的成員。</span><span class="sxs-lookup"><span data-stu-id="8855a-157">However, the user who runs the deployment command must also be either a domain administrator or a member of the local administrators group on the destination server.</span></span> <span data-ttu-id="8855a-158">此外，遠端代理程式不支援基本驗證，因此您無法傳遞命令列上的替代認證。</span><span class="sxs-lookup"><span data-stu-id="8855a-158">In addition, the remote agent doesn't support basic authentication, so you can't pass alternative credentials on the command line.</span></span>

<span data-ttu-id="8855a-159">遠端代理程式提供部署在開發或測試案例，其中並不是罕見狀況測試伺服器環境的完整系統管理員控制的開發人員和應用程式通常會重建和重新部署非常有用的方式常見問題。</span><span class="sxs-lookup"><span data-stu-id="8855a-159">The remote agent provides a useful approach to deployment in development or test scenarios, where it's not uncommon for developers to have full administrator control over a test server environment, and applications are typically rebuilt and redeployed very frequently.</span></span> <span data-ttu-id="8855a-160">不過，這種方法是通常較低可接受的預備環境或生產環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-160">However, this approach is usually less acceptable for staging or production environments.</span></span>

<span data-ttu-id="8855a-161">如需端對端的範例會使用遠端代理程式方法的案例，請參閱[案例： 設定 Web 部署的測試環境](scenario-configuring-a-test-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="8855a-161">For an end-to-end example of a scenario that uses the remote agent approach, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span>

## <a name="using-the-temp-agent"></a><span data-ttu-id="8855a-162">使用暫存的代理程式</span><span class="sxs-lookup"><span data-stu-id="8855a-162">Using the Temp Agent</span></span>

<span data-ttu-id="8855a-163">部署暫存的代理程式方法是類似於遠端代理程式的方法。</span><span class="sxs-lookup"><span data-stu-id="8855a-163">The temp agent approach to deployment is similar to the remote agent approach.</span></span> <span data-ttu-id="8855a-164">不過，相較於遠端代理程式的方法，您不需要在目的地 web 伺服器上安裝 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="8855a-164">However, in contrast to the remote agent approach, you don't need to install Web Deploy on the destination web server.</span></span> <span data-ttu-id="8855a-165">相反地，當您執行部署時，Web Deploy 會在目的地伺服器上安裝 web deployment agent service 暫存版本並將使用此 url 將內容部署至 IIS。</span><span class="sxs-lookup"><span data-stu-id="8855a-165">Instead, when you perform the deployment, Web Deploy will install a temporary version of the web deployment agent service on the destination server and will use this to deploy your content to IIS.</span></span> <span data-ttu-id="8855a-166">部署完成時，會移除所有的暫存檔案。</span><span class="sxs-lookup"><span data-stu-id="8855a-166">When the deployment is complete, all temporary files are removed.</span></span>

<span data-ttu-id="8855a-167">如果您想要使用暫存的代理程式提供者設定，請將 **/g**旗標，以您的部署命令：</span><span class="sxs-lookup"><span data-stu-id="8855a-167">If you want to use the temp agent provider setting, add the **/g** flag to your deployment command:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="8855a-168">您無法使用暫存的代理程式如果 web 部署代理程式服務已安裝的目的地電腦上，即使服務並未執行。</span><span class="sxs-lookup"><span data-stu-id="8855a-168">You can't use the temp agent if the web deployment agent service is installed on the destination computer, even if the service is not running.</span></span>


<span data-ttu-id="8855a-169">此方法的優點是，您不需要維護您的目的地伺服器上的 Web Deploy 的安裝。</span><span class="sxs-lookup"><span data-stu-id="8855a-169">The advantage of this approach is that you don't need to maintain installations of Web Deploy on your destination servers.</span></span> <span data-ttu-id="8855a-170">此外，您不需要確定來源和目的地電腦執行相同版本的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="8855a-170">Furthermore, you don't need to ensure that the source and destination computers are running the same version of Web Deploy.</span></span> <span data-ttu-id="8855a-171">不過，這種方法有相同的主要限制遠端代理程式的方法，也就是，您必須是目的地伺服器上的本機系統管理員，才能部署內容，而且支援只 NTLM 驗證。</span><span class="sxs-lookup"><span data-stu-id="8855a-171">However, this approach suffers from the same principal limitations as the remote agent approach, namely that you must be a local administrator on the destination server in order to deploy content, and only NTLM authentication is supported.</span></span> <span data-ttu-id="8855a-172">暫存的代理程式的方法也需要更多初始設定目的地環境。</span><span class="sxs-lookup"><span data-stu-id="8855a-172">The temp agent approach also requires a lot more initial configuration of the destination environment.</span></span>

<span data-ttu-id="8855a-173">如需有關如何使用暫存的代理程式的詳細資訊，請參閱 <<c0> [ 如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)並[On Demand Web 部署](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-173">For more information on using the temp agent, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx) and [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

## <a name="using-the-web-deploy-handler"></a><span data-ttu-id="8855a-174">使用 Web Deploy 處理常式</span><span class="sxs-lookup"><span data-stu-id="8855a-174">Using the Web Deploy Handler</span></span>

<span data-ttu-id="8855a-175">適用於 IIS 7 及更新版本，Web Deploy 提供其他部署方法，透過 IIS Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="8855a-175">For IIS 7 onwards, Web Deploy offers an alternative deployment approach through the IIS Web Deploy Handler.</span></span> <span data-ttu-id="8855a-176">與 IIS Web 管理服務 (WMSvc)，其設計目的是要讓使用者從遠端位置管理 IIS 網站緊密整合，Web 部署處理常式。</span><span class="sxs-lookup"><span data-stu-id="8855a-176">The Web Deploy Handler is closely integrated with the IIS Web Management Service (WMSvc), which is designed to allow users to manage IIS websites from remote locations.</span></span>

<span data-ttu-id="8855a-177">根據預設，遠端代理程式會公開 HTTP 端點，在此位址：</span><span class="sxs-lookup"><span data-stu-id="8855a-177">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="8855a-178">您可以取代 [*server*] 與您的 web 伺服器的電腦名稱，您的 web 伺服器或主機名稱的 IP 位址解析為您的 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="8855a-178">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="8855a-179">遠端代理程式，以及暫存的代理程式，Web 部署處理常式的最大好處是，您可以設定 IIS 以允許非系統管理員將應用程式和內容部署到特定的 IIS 網站的使用者。</span><span class="sxs-lookup"><span data-stu-id="8855a-179">The big advantage of the Web Deploy Handler over the remote agent, and the temp agent, is that you can configure IIS to allow non-administrator users to deploy applications and content to specific IIS websites.</span></span> <span data-ttu-id="8855a-180">Web 部署處理常式也會支援基本驗證，因此您可以做為參數提供替代的認證，在您的 Web Deploy 命令。</span><span class="sxs-lookup"><span data-stu-id="8855a-180">The Web Deploy Handler also supports basic authentication, so you can provide alternative credentials as parameters in your Web Deploy commands.</span></span> <span data-ttu-id="8855a-181">主要的缺點是，Web 部署處理常式一開始更多複雜而無法安裝和設定。</span><span class="sxs-lookup"><span data-stu-id="8855a-181">The major drawback is that the Web Deploy Handler is initially a lot more complicated to set up and configure.</span></span>

<span data-ttu-id="8855a-182">在非系統管理員使用者的情況下 Web 管理服務 (WMSvc) 只允許使用者連接到 IIS 使用站台層級的連線，而不是伺服器層級的連線。</span><span class="sxs-lookup"><span data-stu-id="8855a-182">In the case of non-administrator users, the Web Management Service (WMSvc) will only allow the user to connect to IIS using a site-level connection, rather than a server-level connection.</span></span> <span data-ttu-id="8855a-183">若要存取特定的網站，您可以包含站台特有的查詢字串中的端點位址：</span><span class="sxs-lookup"><span data-stu-id="8855a-183">To access a particular site, you can include a site-specific query string in the endpoint address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


<span data-ttu-id="8855a-184">例如，假設您的建置流程設定為自動部署至預備環境之後每次成功建置 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8855a-184">For example, suppose a build process is configured to automatically deploy a web application to a staging environment after every successful build.</span></span> <span data-ttu-id="8855a-185">如果您使用遠端代理程式的方法，您必須在目的地伺服器上進行建置處理序身分識別系統管理員。</span><span class="sxs-lookup"><span data-stu-id="8855a-185">If you used the remote agent approach, you'd need to make the build process identity an administrator on your destination servers.</span></span> <span data-ttu-id="8855a-186">相反地，使用 Web 部署的處理常式方法您可以授與非系統管理員使用者&#x2014;**FABRIKAM\stagingdeployer**在此情況下&#x2014;的特定 IIS 網站和建置程序的權限可以提供這些若要部署網頁套件的認證。</span><span class="sxs-lookup"><span data-stu-id="8855a-186">In contrast, using the Web Deploy Handler approach you can give a non-administrator user&#x2014;**FABRIKAM\stagingdeployer** in this case&#x2014;permission to a specific IIS website only, and the build process can provide these credentials to deploy the web package.</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> <span data-ttu-id="8855a-187">如需有關 Web Deploy 命令列的作業和語法的詳細資訊，請參閱 < [Web 部署的命令列參考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-187">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="8855a-188">如需有關使用 *。 deploy.cmd*檔案，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-188">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="8855a-189">Web 部署處理常式提供部署在預備環境、 託管的環境和內部網路為基礎的生產環境中，遠端存取伺服器可用，但系統管理員認證不是很實用的方法。</span><span class="sxs-lookup"><span data-stu-id="8855a-189">The Web Deploy Handler provides a useful approach to deployment in staging environments, hosted environments, and intranet-based production environments, where remote access to the server is available but administrator credentials are not.</span></span>

<span data-ttu-id="8855a-190">如需端對端的範例會使用 Web 部署的處理常式方法的案例，請參閱 <<c0> [ 案例： 設定 Web 部署的預備環境](scenario-configuring-a-staging-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="8855a-190">For an end-to-end example of a scenario that uses the Web Deploy Handler approach, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

## <a name="using-offline-deployment"></a><span data-ttu-id="8855a-191">使用離線部署</span><span class="sxs-lookup"><span data-stu-id="8855a-191">Using Offline Deployment</span></span>

<span data-ttu-id="8855a-192">在某些情況下，它是不可行或不合實際從遠端位置中將應用程式和內容部署至 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="8855a-192">In some cases, it's not possible or practical to deploy applications and content to an IIS website from a remote location.</span></span> <span data-ttu-id="8855a-193">比方說，在來源與目的地電腦可能會在隔離的網路或網路區段或防火牆原則可能不會允許遠端存取。</span><span class="sxs-lookup"><span data-stu-id="8855a-193">For example, the source and destination computers may be in isolated networks or network segments, or firewall policy may not permit remote access.</span></span>

<span data-ttu-id="8855a-194">在這類的情況下，您仍然可以使用封裝和發佈的 Web Deploy; 的功能您只是無法使用它們從遠端位置。</span><span class="sxs-lookup"><span data-stu-id="8855a-194">In scenarios like these, you can still use the packaging and publishing capabilities of Web Deploy; you just can't use them from a remote location.</span></span> <span data-ttu-id="8855a-195">相反地，在目的地伺服器上的系統管理員必須複製到伺服器上的 web 套件，並匯入它透過 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="8855a-195">Instead, an administrator on the destination server must copy the web package onto the server and import it through IIS Manager.</span></span>

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

<span data-ttu-id="8855a-196">離線部署方法是在網際網路對向生產環境中，周邊網路中的伺服器可能有限制的連線與內部網路中的電腦通常很有用。</span><span class="sxs-lookup"><span data-stu-id="8855a-196">The offline deployment approach is typically useful in Internet-facing production environments, where servers in a perimeter network may have restricted connectivity with computers in the internal network.</span></span>

<span data-ttu-id="8855a-197">如需端對端的範例會使用離線部署方法的案例，請參閱 <<c0> [ 案例： 設定 Web 部署的生產環境](scenario-configuring-a-production-environment-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="8855a-197">For an end-to-end example of a scenario that uses the offline deployment approach, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

## <a name="further-reading"></a><span data-ttu-id="8855a-198">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="8855a-198">Further Reading</span></span>

<span data-ttu-id="8855a-199">如需有關 Web Deploy 命令列的作業和語法的詳細資訊，請參閱 < [Web 部署的命令列參考](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-199">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="8855a-200">如需有關使用 *。 deploy.cmd*檔案，請參閱[如何： 部署封裝使用 deploy.cmd 檔案安裝](https://msdn.microsoft.com/library/ff356104.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-200">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>

<span data-ttu-id="8855a-201">更多一般指引，您可以在其中部署 web 封裝，從遠端電腦的不同方式的詳細資訊，請參閱 <<c0> [ 使用 Web 部署遠端](https://technet.microsoft.com/library/ee461175(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-201">For more general guidance on the different ways in which you can deploy web packages from a remote computer, see [Using Web Deploy Remotely](https://technet.microsoft.com/library/ee461175(WS.10).aspx).</span></span> <span data-ttu-id="8855a-202">如需有關使用 On Demand Web 部署的詳細資訊，請參閱[On Demand Web 部署](https://technet.microsoft.com/library/ee517345(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8855a-202">For more information on using Web Deploy On Demand, see [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8855a-203">[上一頁](configuring-server-environments-for-web-deployment.md)
> [下一頁](scenario-configuring-a-test-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="8855a-203">[Previous](configuring-server-environments-for-web-deployment.md)
[Next](scenario-configuring-a-test-environment-for-web-deployment.md)</span></span>
