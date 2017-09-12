---
title: "裝載和部署概觀 - ASP.NET Core"
author: tdykstra
description: "如何設定裝載環境，並將 ASP.NET Core 應用程式部署到這些環境的概觀。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: df3c1f0c2768b89c3ea5dc901782170c530a542e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a><span data-ttu-id="a8044-104">ASP.NET Core 應用程式的裝載和部署概觀</span><span class="sxs-lookup"><span data-stu-id="a8044-104">Hosting and deployment overview for ASP.NET Core apps</span></span>

<span data-ttu-id="a8044-105">以下是您將 ASP.NET Core 應用程式部署到裝載環境時執行的主要步驟：</span><span class="sxs-lookup"><span data-stu-id="a8044-105">Here are the main steps you perform to deploy an ASP.NET Core app to a hosting environment:</span></span>

* <span data-ttu-id="a8044-106">將應用程式發行至裝載伺服器上的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8044-106">Publish the app to a folder on the hosting server.</span></span>
* <span data-ttu-id="a8044-107">設定處理序管理員，以在要求傳入時啟動應用程式，並在其損毀或伺服器重新開機後重新啟動。</span><span class="sxs-lookup"><span data-stu-id="a8044-107">Set up a process manager that starts the app when requests come in and restarts it after it crashes or the server reboots.</span></span>
* <span data-ttu-id="a8044-108">設定反向 Proxy，以將要求轉送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8044-108">Set up a reverse proxy that forwards requests to the app.</span></span>

## <a name="publish-to-a-folder"></a><span data-ttu-id="a8044-109">發行至資料夾</span><span class="sxs-lookup"><span data-stu-id="a8044-109">Publish to a folder</span></span> 

<span data-ttu-id="a8044-110">[dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI 命令會編譯應用程式程式碼，並將執行應用程式所需的檔案複製到 *publish* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a8044-110">The [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI command compiles application code and copies the files needed to run the application into a *publish* folder.</span></span> <span data-ttu-id="a8044-111">當您從 Visual Studio 部署時，會在檔案複製到部署目的地之前，為您自動完成 `dotnet publish` 步驟。</span><span class="sxs-lookup"><span data-stu-id="a8044-111">When you deploy from Visual Studio the `dotnet publish` step is done for you automatically before files are copied to the deployment destination.</span></span>

### <a name="folder-contents"></a><span data-ttu-id="a8044-112">資料夾內容</span><span class="sxs-lookup"><span data-stu-id="a8044-112">Folder contents</span></span>

<span data-ttu-id="a8044-113">*publish* 資料夾包含應用程式、其相依性和選擇性 .NET 執行階段的 *.exe* 和 *.dll* 檔案。</span><span class="sxs-lookup"><span data-stu-id="a8044-113">The *publish* folder contains *.exe* and *.dll* files for the application, its dependencies, and optionally the .NET runtime.</span></span>

<span data-ttu-id="a8044-114">.NET Core 應用程式可以發行為*獨立*或 *與 Framework 相依的*應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8044-114">A .NET Core app can be published as *self-contained* or *framework-dependent*.</span></span> <span data-ttu-id="a8044-115">如果應用程式是獨立應用程式，包含 .NET 執行階段的 *.dll* 檔案將包含在 *publish* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a8044-115">If the app is self-contained, the *.dll* files that contain the .NET runtime are included in the *publish* folder.</span></span>  <span data-ttu-id="a8044-116">如果應用程式是與 Framework 相依的應用程式，則不會包含 .NET 執行階段檔案，因為應用程式具有對電腦上已安裝之 .NET 版本的參考。</span><span class="sxs-lookup"><span data-stu-id="a8044-116">If the app is framework-dependent, the .NET runtime files are not included because the app has a reference to a version of .NET that is installed on the computer.</span></span> <span data-ttu-id="a8044-117">預設部署模式是與 Framework 相依。</span><span class="sxs-lookup"><span data-stu-id="a8044-117">The default deployment model is framework-dependent.</span></span> <span data-ttu-id="a8044-118">如需詳細資訊，請參閱 [.NET Core 應用程式部署](https://docs.microsoft.com/dotnet/articles/core/deploying/index)。</span><span class="sxs-lookup"><span data-stu-id="a8044-118">For more information, see [.NET Core application deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/index).</span></span>

<span data-ttu-id="a8044-119">除了 *.exe* 和 *.dll* 檔案之外，ASP.NET Core 應用程式的 *publish* 資料夾通常還包含組態檔、靜態資產和 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="a8044-119">In addition to *.exe* and *.dll* files, the *publish* folder for an ASP.NET Core app typically contains configuration files, static assets, and MVC views.</span></span>  <span data-ttu-id="a8044-120">如需詳細資訊，請參閱[目錄結構](xref:hosting/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="a8044-120">For more information, see [Directory structure](xref:hosting/directory-structure).</span></span>

## <a name="set-up-a-process-manager"></a><span data-ttu-id="a8044-121">設定處理序管理員</span><span class="sxs-lookup"><span data-stu-id="a8044-121">Set up a process manager</span></span>

<span data-ttu-id="a8044-122">ASP.NET Core 應用程式是一種主控台應用程式，必須在伺服器開機和損毀後重新啟動時啟動此應用程式。</span><span class="sxs-lookup"><span data-stu-id="a8044-122">An ASP.NET Core app is a console app that has to be started when a server boots and restarted after crashes.</span></span> <span data-ttu-id="a8044-123">若要自動啟動和重新啟動，您需要有處理序管理員。</span><span class="sxs-lookup"><span data-stu-id="a8044-123">To automate starts and restarts you need a process manager.</span></span> <span data-ttu-id="a8044-124">ASP.NET Core 最常見的處理序管理員是 Linux 上的 [Nginx](xref:publishing/linuxproduction) 和 [Apache](xref:publishing/apache-proxy)，以及 Windows 上的 [IIS](xref:publishing/iis) 和 [Windows 服務](xref:hosting/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="a8044-124">The most common process managers for ASP.NET Core are [Nginx](xref:publishing/linuxproduction) and [Apache](xref:publishing/apache-proxy) on Linux, and [IIS](xref:publishing/iis) and [Windows Service](xref:hosting/windows-service) on Windows.</span></span>

## <a name="set-up-a-reverse-proxy"></a><span data-ttu-id="a8044-125">設定反向 Proxy</span><span class="sxs-lookup"><span data-stu-id="a8044-125">Set up a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a8044-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a8044-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a8044-127">如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器，您可以使用 [Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy) 或 [IIS](xref:publishing/iis) 作為反向 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8044-127">If your app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server, you can use [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), or [IIS](xref:publishing/iis) as a reverse proxy server.</span></span> <span data-ttu-id="a8044-128">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8044-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="a8044-129">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="a8044-129">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a8044-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a8044-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a8044-131">如果應用程式使用 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器，而且將公開至網際網路，您必須使用 [Nginx](xref:publishing/linuxproduction)、 [Apache](xref:publishing/apache-proxy) 或 [IIS](xref:publishing/iis) 作為反向 Proxy 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a8044-131">If your app uses the [Kestrel](xref:fundamentals/servers/kestrel) web server and will be exposed to the Internet, you must use [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), or [IIS](xref:publishing/iis) as a reverse proxy server.</span></span> <span data-ttu-id="a8044-132">反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="a8044-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span> <span data-ttu-id="a8044-133">使用反向 Proxy 的主要原因是安全性。</span><span class="sxs-lookup"><span data-stu-id="a8044-133">The main reason for using a reverse proxy is security.</span></span> <span data-ttu-id="a8044-134">如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)。</span><span class="sxs-lookup"><span data-stu-id="a8044-134">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a><span data-ttu-id="a8044-135">使用 Visual Studio 和 MSBuild 來自動化部署</span><span class="sxs-lookup"><span data-stu-id="a8044-135">Using Visual Studio and MSBuild to automate deployment</span></span>

<span data-ttu-id="a8044-136">除了將輸出從 `dotnet publish` 複製到伺服器之外，部署通常還需要額外的工作。</span><span class="sxs-lookup"><span data-stu-id="a8044-136">Deployment often requires additional tasks besides copying the output from `dotnet publish` to a server.</span></span> <span data-ttu-id="a8044-137">例如，您可以將額外檔案包含在 *publish* 資料夾，也可以從中排除檔案。</span><span class="sxs-lookup"><span data-stu-id="a8044-137">For example, you might want to include extra files in the *publish* folder, or exclude files from it.</span></span> <span data-ttu-id="a8044-138">Visual Studio 會將 MSBuild 用於 Web 部署，而且您可以自訂 MSBuild，以便在部署期間執行許多其他工作。</span><span class="sxs-lookup"><span data-stu-id="a8044-138">Visual Studio uses MSBuild for web deployment, and you can customize MSBuild to do many other tasks during deployment.</span></span> <span data-ttu-id="a8044-139">如需詳細資訊，請參閱[在 Visual Studio 中發行設定檔](xref:publishing/web-publishing-vs)和[使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com/) 書籍。</span><span class="sxs-lookup"><span data-stu-id="a8044-139">For more information, see [Publish profiles in Visual Studio](xref:publishing/web-publishing-vs) and the [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) book.</span></span>

<span data-ttu-id="a8044-140">您可以使用[發行 Web 功能](xref:tutorials/publish-to-azure-webapp-using-vs)或使用[內建的 Git 支援](xref:publishing/azure-continuous-deployment)，直接從 Visual Studio 部署至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="a8044-140">You can deploy directly from Visual Studio to Azure App Service by using [the Publish Web feature](xref:tutorials/publish-to-azure-webapp-using-vs) or by using [built-in Git support](xref:publishing/azure-continuous-deployment).</span></span> <span data-ttu-id="a8044-141">Visual Studio Team Services 支援[持續部署至 Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)。</span><span class="sxs-lookup"><span data-stu-id="a8044-141">Visual Studio Team Services supports [continuous deployment to Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8044-142">其他資源</span><span class="sxs-lookup"><span data-stu-id="a8044-142">Additional resources</span></span>

<span data-ttu-id="a8044-143">如需使用 Docker 作為裝載環境的詳細資訊，請參閱[在 Docker 中裝載 ASP.NET Core 應用程式](xref:publishing/docker)。</span><span class="sxs-lookup"><span data-stu-id="a8044-143">For information about using Docker as a hosting environment, see [Host ASP.NET Core apps in Docker](xref:publishing/docker).</span></span>
