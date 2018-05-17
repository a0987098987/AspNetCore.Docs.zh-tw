---
title: 裝載及部署 ASP.NET Core
author: rick-anderson
description: 了解如何設定裝載環境及部署 ASP.NET Core 應用程式。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 6364f54ede43b8eec070166a75b4552d896644f3
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
---
# <a name="host-and-deploy-aspnet-core"></a><span data-ttu-id="b8ea2-103">裝載及部署 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8ea2-103">Host and deploy ASP.NET Core</span></span>

<span data-ttu-id="b8ea2-104">一般是將 ASP.NET Core 應用程式部署到裝載環境：</span><span class="sxs-lookup"><span data-stu-id="b8ea2-104">In general, to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="b8ea2-105">將應用程式發行至裝載伺服器上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-105">Publish the app to a folder on the hosting server.</span></span>
* <span data-ttu-id="b8ea2-106">設定處理序管理員，以在要求到達時啟動應用程式，並在其損毀或伺服器重新開機後重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-106">Set up a process manager that starts the app when requests arrive and restarts the app after it crashes or the server reboots.</span></span>
* <span data-ttu-id="b8ea2-107">設定反向 Proxy，以將要求轉送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-107">Set up a reverse proxy that forwards requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="b8ea2-108">發行至資料夾</span><span class="sxs-lookup"><span data-stu-id="b8ea2-108">Publish to a folder</span></span> 

<span data-ttu-id="b8ea2-109">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish) CLI 命令會編譯應用程式程式碼，並將執行應用程式所需的檔案複製到 *publish* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-109">The [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) CLI command compiles app code and copies the files needed to run the app into a *publish* folder.</span></span> <span data-ttu-id="b8ea2-110">從 Visual Studio 部署時，[dotnet publish](/dotnet/core/tools/dotnet-publish) 步驟會在檔案複製到部署目的地之前自動完成。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-110">When deploying from Visual Studio, the [dotnet publish](/dotnet/core/tools/dotnet-publish) step happens automatically before the files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="b8ea2-111">資料夾內容</span><span class="sxs-lookup"><span data-stu-id="b8ea2-111">Folder contents</span></span>

<span data-ttu-id="b8ea2-112">*publish* 資料夾包含應用程式、其相依性和選擇性 .NET 執行階段的 *.exe* 和 *.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-112">The *publish* folder contains *.exe* and *.dll* files for the app, its dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="b8ea2-113">.NET Core 應用程式可以發行為「獨立式」或「與 Framework 相依」的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-113">A .NET Core app can be published as *self-contained* or *framework-dependent* app.</span></span> <span data-ttu-id="b8ea2-114">如果應用程式是獨立應用程式，包含 .NET 執行階段的 *.dll* 檔案將包含在 *publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-114">If the app is self-contained, the *.dll* files that contain the .NET runtime are included in the *publish* folder.</span></span> <span data-ttu-id="b8ea2-115">如果應用程式是與 Framework 相依的應用程式，則不會包含 .NET 執行階段檔案，因為應用程式具有對伺服器上已安裝之 .NET 版本的參考。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-115">If the app is framework-dependent, the .NET runtime files aren't included because the app has a reference to a version of .NET that's installed on the server.</span></span> <span data-ttu-id="b8ea2-116">預設部署模式是與 Framework 相依。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-116">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="b8ea2-117">如需詳細資訊，請參閱 [.NET Core 應用程式部署](/dotnet/articles/core/deploying/index)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-117">For more information, see [.NET Core application deployment](/dotnet/articles/core/deploying/index).</span></span>

<span data-ttu-id="b8ea2-118">除了 *.exe* 和 *.dll* 檔案之外，ASP.NET Core 應用程式的 *publish* 資料夾通常還包含組態檔、靜態資產和 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-118">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span> <span data-ttu-id="b8ea2-119">如需詳細資訊，請參閱[目錄結構](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-119">For more information, see [Directory structure](xref:host-and-deploy/directory-structure).</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="b8ea2-120">設定處理序管理員</span><span class="sxs-lookup"><span data-stu-id="b8ea2-120">Set up a process manager</span></span>

<span data-ttu-id="b8ea2-121">ASP.NET Core 應用程式是一種主控台應用程式，必須在伺服器開機和損毀後重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-121">An ASP.NET Core app is a console app that must be started when a server boots and restarted if it crashes.</span></span> <span data-ttu-id="b8ea2-122">若要自動化啟動和重新啟動，需要有處理序管理員。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-122">To automate starts and restarts, a process manager is required.</span></span> <span data-ttu-id="b8ea2-123">ASP.NET Core 最常見的處理序管理員是：</span><span class="sxs-lookup"><span data-stu-id="b8ea2-123">The most common process managers for ASP.NET Core are:</span></span>

* <span data-ttu-id="b8ea2-124">Linux</span><span class="sxs-lookup"><span data-stu-id="b8ea2-124">Linux</span></span>
  * [<span data-ttu-id="b8ea2-125">Nginx</span><span class="sxs-lookup"><span data-stu-id="b8ea2-125">Nginx</span></span>](xref:host-and-deploy/linux-nginx)
  * [<span data-ttu-id="b8ea2-126">Apache</span><span class="sxs-lookup"><span data-stu-id="b8ea2-126">Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="b8ea2-127">Windows</span><span class="sxs-lookup"><span data-stu-id="b8ea2-127">Windows</span></span>
  * [<span data-ttu-id="b8ea2-128">IIS</span><span class="sxs-lookup"><span data-stu-id="b8ea2-128">IIS</span></span>](xref:host-and-deploy/iis/index)
  * [<span data-ttu-id="b8ea2-129">Windows 服務</span><span class="sxs-lookup"><span data-stu-id="b8ea2-129">Windows Service</span></span>](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="b8ea2-130">設定反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="b8ea2-130">Set up a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b8ea2-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b8ea2-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b8ea2-132">如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器，反向 Proxy 伺服器可用 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache) 或 [IIS](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-132">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server, [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) can be used as a reverse proxy server.</span></span> <span data-ttu-id="b8ea2-133">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-133">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="b8ea2-134">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b8ea2-135">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b8ea2-135">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b8ea2-136">如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器且將公開至網際網路，反向 Proxy 伺服器則用 [Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)或 [IIS](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-136">If the app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server and will be exposed to the Internet, use [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), or [IIS](xref:host-and-deploy/iis/index) as a reverse proxy server.</span></span> <span data-ttu-id="b8ea2-137">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-137">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="b8ea2-138">使用反向 Proxy 的主要原因是安全性。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-138">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="b8ea2-139">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-139">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b8ea2-140">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="b8ea2-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b8ea2-141">Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="b8ea2-142">若沒有其他設定，應用程式可能無法存取配置 (HTTP/HTTPS) 和發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-142">Without additional configuration, an app might not have access to the scheme (HTTP/HTTPS) and the remote IP address where a request originated.</span></span> <span data-ttu-id="b8ea2-143">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a><span data-ttu-id="b8ea2-144">使用 Visual Studio 和 MSBuild 來自動化部署</span><span class="sxs-lookup"><span data-stu-id="b8ea2-144">Using Visual Studio and MSBuild to automate deployment</span></span>

<span data-ttu-id="b8ea2-145">除了從 [dotnet publish](/dotnet/core/tools/dotnet-publish) 將輸出複製到伺服器之外，部署通常還需要額外的工作。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-145">Deployment often requires additional tasks besides copying the output from [dotnet publish](/dotnet/core/tools/dotnet-publish) to a server.</span></span> <span data-ttu-id="b8ea2-146">例如，*publish* 資料夾可能需要或排除額外的檔案。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-146">For example, extra files might be required or excluded from the *publish* folder.</span></span> <span data-ttu-id="b8ea2-147">Visual Studio 會將 MSBuild 用於 Web 部署，而且您可以自訂 MSBuild 在部署期間執行許多其他工作。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-147">Visual Studio uses MSBuild for web deployment, and MSBuild can be customized to do many other tasks during deployment.</span></span> <span data-ttu-id="b8ea2-148">如需詳細資訊，請參閱[在 Visual Studio 中發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 書籍。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-148">For more information, see [Publish profiles in Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="b8ea2-149">使用[發行 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或使用[內建的 Git 支援](xref:host-and-deploy/azure-apps/azure-continuous-deployment)，應用程式可以直接從 Visual Studio 部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-149">By using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or [built-in Git support](xref:host-and-deploy/azure-apps/azure-continuous-deployment), apps can be deployed directly from Visual Studio to the Azure App Service.</span></span> <span data-ttu-id="b8ea2-150">Visual Studio Team Services 支援[持續部署至 Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-150">Visual Studio Team Services supports [continuous deployment to Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).</span></span>

## <a name="publishing-to-azure"></a><span data-ttu-id="b8ea2-151">發行至 Azure</span><span class="sxs-lookup"><span data-stu-id="b8ea2-151">Publishing to Azure</span></span>

<span data-ttu-id="b8ea2-152">如需使用 Visual Studio 將應用程式發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-152">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish an app to Azure using Visual Studio.</span></span> <span data-ttu-id="b8ea2-153">您也可以透過[命令列](xref:tutorials/publish-to-azure-webapp-using-cli)，將應用程式發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-153">The app can also be published to Azure from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8ea2-154">其他資源</span><span class="sxs-lookup"><span data-stu-id="b8ea2-154">Additional resources</span></span>

<span data-ttu-id="b8ea2-155">如需使用 Docker 作為裝載環境的資訊，請參閱[在 Docker 中裝載 ASP.NET Core 應用程式](xref:host-and-deploy/docker/index)。</span><span class="sxs-lookup"><span data-stu-id="b8ea2-155">For information on using Docker as a hosting environment, see [Host ASP.NET Core apps in Docker](xref:host-and-deploy/docker/index).</span></span>
