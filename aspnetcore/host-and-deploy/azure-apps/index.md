---
title: 將 ASP.NET Core 應用程式部署至 Azure App Service
author: bradygaster
description: 本文包含 Azure 主機和部署資源的連結。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/16/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: ba9671f68a0faf99ff5232a6d5dd132d0a1d5ac5
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928422"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="25af4-103">將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="25af4-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="25af4-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="25af4-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="25af4-105">實用資源</span><span class="sxs-lookup"><span data-stu-id="25af4-105">Useful resources</span></span>

<span data-ttu-id="25af4-106">[App Service 文件](/azure/app-service/)是 Azure 應用程式文件、教學課程、範例、使用說明指南與其他資源的首頁。</span><span class="sxs-lookup"><span data-stu-id="25af4-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="25af4-107">關於裝載 ASP.NET Core 應用程式，有兩個值得參考的教學課程：</span><span class="sxs-lookup"><span data-stu-id="25af4-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="25af4-108">在 Azure 中建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="25af4-109">在 Windows 上使用 Visual Studio 建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="25af4-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="25af4-110">在 Linux 上的 App Service 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="25af4-111">在 Linux 上使用命令列建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="25af4-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="25af4-112">如需 Azure App 服務上可用的 ASP.NET Core 版本，請參閱[App Service 儀表板上的 ASP.NET Core](https://aspnetcoreon.azurewebsites.net/) 。</span><span class="sxs-lookup"><span data-stu-id="25af4-112">See the [ASP.NET Core on App Service Dashboard](https://aspnetcoreon.azurewebsites.net/) for the version of ASP.NET Core available on Azure App service.</span></span>

<span data-ttu-id="25af4-113">訂閱[App Service 公告](https://github.com/Azure/app-service-announcements/)存放庫並監視問題。</span><span class="sxs-lookup"><span data-stu-id="25af4-113">Subscribe to the [App Service Announcements](https://github.com/Azure/app-service-announcements/) repository and monitor the issues.</span></span> <span data-ttu-id="25af4-114">App Service 小組會定期張貼傳入 App Service 的公告和案例。</span><span class="sxs-lookup"><span data-stu-id="25af4-114">The App Service team regularly posts announcements and scenarios arriving in App Service.</span></span>

<span data-ttu-id="25af4-115">若要閱讀下列文章，請參閱 ASP.NET Core 文件：</span><span class="sxs-lookup"><span data-stu-id="25af4-115">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="25af4-116">了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="25af4-116">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="25af4-117">了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="25af4-117">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="25af4-118">建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="25af4-118">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="25af4-119">設定 ASP.NET Core 應用程式的 CI 組建，然後建立連續部署發行至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="25af4-119">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="25af4-120">Azure Web 應用程式沙箱</span><span class="sxs-lookup"><span data-stu-id="25af4-120">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="25af4-121">探索 Azure 應用程式平台強制實施的 Azure App Service 執行階段執行限制。</span><span class="sxs-lookup"><span data-stu-id="25af4-121">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="25af4-122">了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="25af4-122">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="25af4-123">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="25af4-123">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="25af4-124">Platform</span><span class="sxs-lookup"><span data-stu-id="25af4-124">Platform</span></span>

<span data-ttu-id="25af4-125">應用程式服務應用程式的平臺架構（x86/x64）會在 Azure 入口網站的應用程式設定中，針對裝載于 A 系列計算（基本）或更高主機層上的應用程式進行設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-125">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute (Basic) or higher hosting tier.</span></span> <span data-ttu-id="25af4-126">確認應用程式的發佈設定（例如，在 Visual Studio[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)）中，是否符合應用程式在 Azure 入口網站中的服務設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-126">Confirm that the app's publish settings (for example, in the Visual Studio [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)) match the setting in the app's service configuration in the Azure Portal.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="25af4-127">Azure App Service 具有 64 位元 (x64) 及 32 位元 (x86) 應用程式的執行階段。</span><span class="sxs-lookup"><span data-stu-id="25af4-127">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="25af4-128">App Service 提供的 [.NET Core SDK](/dotnet/core/sdk) 為 32 位元，但您可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 主控台或 Visual Studio 中的發佈處理序，部署在本機建置的 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-128">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="25af4-129">如需詳細資訊，請參閱[發佈與部署應用程式](#publish-and-deploy-the-app)一節。</span><span class="sxs-lookup"><span data-stu-id="25af4-129">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="25af4-130">對於具有原生相依性的應用程式而言，Azure App Service 具有 32 位元 (x86) 應用程式的執行階段。</span><span class="sxs-lookup"><span data-stu-id="25af4-130">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="25af4-131">App Service 可使用的 [.NET Core SDK](/dotnet/core/sdk) 為 32 位元。</span><span class="sxs-lookup"><span data-stu-id="25af4-131">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="25af4-132">如需 .NET Core framework 元件和散發方法的詳細資訊，例如 .NET Core 執行時間和 .NET Core SDK 的相關資訊，請參閱[關於 .Net core：組合](/dotnet/core/about#composition)。</span><span class="sxs-lookup"><span data-stu-id="25af4-132">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="25af4-133">package</span><span class="sxs-lookup"><span data-stu-id="25af4-133">Packages</span></span>

<span data-ttu-id="25af4-134">包含下列 NuGet 套件，為部署至 Azure App Service 的應用程式提供自動記錄功能：</span><span class="sxs-lookup"><span data-stu-id="25af4-134">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="25af4-135">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) 提供 ASP.NET Core 與 Azure App Service 整合的啟動。</span><span class="sxs-lookup"><span data-stu-id="25af4-135">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="25af4-136">新增的記錄功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 套件提供。</span><span class="sxs-lookup"><span data-stu-id="25af4-136">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="25af4-137">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 執行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，以在 `Microsoft.Extensions.Logging.AzureAppServices` 套件中新增 Azure App Service 診斷記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="25af4-137">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="25af4-138">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供記錄器實作以支援 Azure App Service 診斷記錄和記錄串流功能。</span><span class="sxs-lookup"><span data-stu-id="25af4-138">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="25af4-139">上述套件無法從 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)取得。</span><span class="sxs-lookup"><span data-stu-id="25af4-139">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="25af4-140">以 .NET Framework 為目標或參考 `Microsoft.AspNetCore.App` 中繼套件的應用程式必須明確參考應用程式之專案檔中的個別套件。</span><span class="sxs-lookup"><span data-stu-id="25af4-140">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="25af4-141">使用 Azure 入口網站覆寫應用程式設定</span><span class="sxs-lookup"><span data-stu-id="25af4-141">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="25af4-142">Azure 入口網站中的應用程式設定允許您為應用程式設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="25af4-142">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="25af4-143">環境變數可由[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)取用。</span><span class="sxs-lookup"><span data-stu-id="25af4-143">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="25af4-144">在 Azure 入口網站中建立或修改應用程式設定並選取 [儲存] 按鈕後，即會重新啟動 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-144">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="25af4-145">當服務重新啟動之後，環境變數便可供應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="25af4-145">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="25af4-146">當應用程式使用[泛型主機](xref:fundamentals/host/generic-host)時，會在呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 來建立主機時，將環境變數載入應用程式的設定中。</span><span class="sxs-lookup"><span data-stu-id="25af4-146">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables are loaded into the app's configuration when <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> is called to build the host.</span></span> <span data-ttu-id="25af4-147">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="25af4-147">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="25af4-148">當應用程式使用[Web 主機](xref:fundamentals/host/web-host)時，系統會在呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 來建立主機時，將環境變數載入應用程式的設定中。</span><span class="sxs-lookup"><span data-stu-id="25af4-148">When an app uses the [Web Host](xref:fundamentals/host/web-host), environment variables are loaded into the app's configuration when <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> is called to build the host.</span></span> <span data-ttu-id="25af4-149">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="25af4-149">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="25af4-150">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="25af4-150">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="25af4-151">[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) (當裝載[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)時會設定轉送標頭中介軟體) 與 ASP.NET Core 模組會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="25af4-151">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="25af4-152">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-152">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="25af4-153">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="25af4-153">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="25af4-154">監視與記錄</span><span class="sxs-lookup"><span data-stu-id="25af4-154">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="25af4-155">部署到 App Service 的 ASP.NET Core 應用程式會自動接收 App Service 延伸模組：**ASP.NET Core 記錄整合**。</span><span class="sxs-lookup"><span data-stu-id="25af4-155">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="25af4-156">延伸模組讓 Azure App Service 上的 ASP.NET Core 應用程式得以進行記錄整合。</span><span class="sxs-lookup"><span data-stu-id="25af4-156">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="25af4-157">部署到 App Service 的 ASP.NET Core 應用程式會自動接收 App Service 延伸模組 **ASP.NET Core 記錄延伸模組**。</span><span class="sxs-lookup"><span data-stu-id="25af4-157">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="25af4-158">延伸模組讓 Azure App Service 上的 ASP.NET Core 應用程式得以進行記錄整合。</span><span class="sxs-lookup"><span data-stu-id="25af4-158">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="25af4-159">如需監視、記錄及疑難排解的資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="25af4-159">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="25af4-160">監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-160">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="25af4-161">了解如何檢閱應用程式和 App Service 方案的配額和計量。</span><span class="sxs-lookup"><span data-stu-id="25af4-161">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="25af4-162">為 Azure App Service 中的應用程式啟用診斷記錄</span><span class="sxs-lookup"><span data-stu-id="25af4-162">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="25af4-163">探索如何啟用及存取 HTTP 狀態碼、失敗要求和網頁伺服器活動的診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="25af4-163">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="25af4-164">了解處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="25af4-164">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="25af4-165">了解如何診斷使用 ASP.NET Core 應用程式部署 Azure App Service 的問題。</span><span class="sxs-lookup"><span data-stu-id="25af4-165">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="25af4-166">了解託管於 Azure App Service/IIS 之應用程式的常見部署組態錯誤，及疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="25af4-166">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="25af4-167">資料保護金鑰環及部署位置</span><span class="sxs-lookup"><span data-stu-id="25af4-167">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="25af4-168">[資料保護金鑰](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存至 *%HOME%\ASP.NET\DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="25af4-168">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="25af4-169">此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。</span><span class="sxs-lookup"><span data-stu-id="25af4-169">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="25af4-170">金鑰待用時不受保護。</span><span class="sxs-lookup"><span data-stu-id="25af4-170">Keys aren't protected at rest.</span></span> <span data-ttu-id="25af4-171">此資料夾對單一部署位置中應用程式的所有執行個體皆提供金鑰環。</span><span class="sxs-lookup"><span data-stu-id="25af4-171">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="25af4-172">各部署位置，例如預備和生產位置，不會共用金鑰環。</span><span class="sxs-lookup"><span data-stu-id="25af4-172">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="25af4-173">當在部署位置間交換時，使用資料保護的任何系統都將無法使用前一位置內的金鑰環，來解密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="25af4-173">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="25af4-174">ASP.NET Cookie 中介軟體使用資料保護來保護其 Cookie。</span><span class="sxs-lookup"><span data-stu-id="25af4-174">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="25af4-175">這會導致使用標準 ASP.NET Cookie 中介軟體的應用程式將使用者登出。</span><span class="sxs-lookup"><span data-stu-id="25af4-175">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="25af4-176">至於非相依於位置的金鑰環解決方案，請使用外部金鑰環提供者，例如：</span><span class="sxs-lookup"><span data-stu-id="25af4-176">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="25af4-177">Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="25af4-177">Azure Blob Storage</span></span>
* <span data-ttu-id="25af4-178">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="25af4-178">Azure Key Vault</span></span>
* <span data-ttu-id="25af4-179">SQL 存放區</span><span class="sxs-lookup"><span data-stu-id="25af4-179">SQL store</span></span>
* <span data-ttu-id="25af4-180">Redis 快取</span><span class="sxs-lookup"><span data-stu-id="25af4-180">Redis cache</span></span>

<span data-ttu-id="25af4-181">如需詳細資訊，請參閱<xref:security/data-protection/implementation/key-storage-providers>。</span><span class="sxs-lookup"><span data-stu-id="25af4-181">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>

## <a name="deploy-an-aspnet-core-app-that-uses-a-net-core-preview"></a><span data-ttu-id="25af4-182">部署使用 .NET Core 預覽的 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-182">Deploy an ASP.NET Core app that uses a .NET Core preview</span></span>

<span data-ttu-id="25af4-183">若要部署使用 .NET Core 預覽版本的應用程式，請參閱下列資源。</span><span class="sxs-lookup"><span data-stu-id="25af4-183">To deploy an app that uses a preview release of .NET Core, see the following resources.</span></span> <span data-ttu-id="25af4-184">當執行時間可用但 SDK 尚未安裝在 Azure App Service 上時，也會使用這些方法。</span><span class="sxs-lookup"><span data-stu-id="25af4-184">These approaches are also used when the runtime is available but the SDK hasn't been installed on Azure App Service.</span></span>

* [<span data-ttu-id="25af4-185">使用 Azure Pipelines 指定 .NET Core SDK 版本</span><span class="sxs-lookup"><span data-stu-id="25af4-185">Specify the .NET Core SDK Version using Azure Pipelines</span></span>](#specify-the-net-core-sdk-version-using-azure-pipelines)
* [<span data-ttu-id="25af4-186">部署獨立的預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-186">Deploy a self-contained preview app</span></span>](#deploy-a-self-contained-preview-app)
* [<span data-ttu-id="25af4-187">將包含 Web 應用程式的 Docker 用於容器</span><span class="sxs-lookup"><span data-stu-id="25af4-187">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)
* [<span data-ttu-id="25af4-188">安裝預覽網站延伸模組</span><span class="sxs-lookup"><span data-stu-id="25af4-188">Install the preview site extension</span></span>](#install-the-preview-site-extension)

<span data-ttu-id="25af4-189">如需 Azure App 服務上可用的 ASP.NET Core 版本，請參閱[App Service 儀表板上的 ASP.NET Core](https://aspnetcoreon.azurewebsites.net/) 。</span><span class="sxs-lookup"><span data-stu-id="25af4-189">See the [ASP.NET Core on App Service Dashboard](https://aspnetcoreon.azurewebsites.net/) for the version of ASP.NET Core available on Azure App service.</span></span>

### <a name="specify-the-net-core-sdk-version-using-azure-pipelines"></a><span data-ttu-id="25af4-190">使用 Azure Pipelines 指定 .NET Core SDK 版本</span><span class="sxs-lookup"><span data-stu-id="25af4-190">Specify the .NET Core SDK Version using Azure Pipelines</span></span>

<span data-ttu-id="25af4-191">使用[AZURE APP SERVICE CI/CD 案例](/azure/app-service/deploy-continuous-deployment)來設定具有 Azure DevOps 的持續整合組建。</span><span class="sxs-lookup"><span data-stu-id="25af4-191">Use [Azure App Service CI/CD scenarios](/azure/app-service/deploy-continuous-deployment) to set up a continuous integration build with Azure DevOps.</span></span> <span data-ttu-id="25af4-192">建立 Azure DevOps 組建之後，選擇性地將組建設定為使用特定的 SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="25af4-192">After the Azure DevOps build is created, optionally configure the build to use a specific SDK version.</span></span> 

#### <a name="specify-the-net-core-sdk-version"></a><span data-ttu-id="25af4-193">指定 .NET Core SDK 版本</span><span class="sxs-lookup"><span data-stu-id="25af4-193">Specify the .NET Core SDK version</span></span>

<span data-ttu-id="25af4-194">使用 App Service 部署中心建立 Azure DevOps 組建時，預設的組建管線會包含 `Restore`、`Build`、`Test`和 `Publish`的步驟。</span><span class="sxs-lookup"><span data-stu-id="25af4-194">When using the App Service deployment center to create an Azure DevOps build, the default build pipeline includes steps for `Restore`, `Build`, `Test`, and `Publish`.</span></span> <span data-ttu-id="25af4-195">若要指定 SDK 版本，請選取 [代理程式作業] 清單中的 [新增] **（+）** 按鈕，以新增新的步驟。</span><span class="sxs-lookup"><span data-stu-id="25af4-195">To specify the SDK version, select the **Add (+)** button in the Agent job list to add a new step.</span></span> <span data-ttu-id="25af4-196">在搜尋列中搜尋 **.NET Core SDK** 。</span><span class="sxs-lookup"><span data-stu-id="25af4-196">Search for **.NET Core SDK** in the search bar.</span></span> 

![新增 .NET Core SDK 步驟](index/add-sdk-step.png)

<span data-ttu-id="25af4-198">將步驟移至組建中的第一個位置，使其後面的步驟使用指定的 .NET Core SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="25af4-198">Move the step into the first position in the build so that the steps following it use the specified version of the .NET Core SDK.</span></span> <span data-ttu-id="25af4-199">指定 .NET Core SDK 的版本。</span><span class="sxs-lookup"><span data-stu-id="25af4-199">Specify the version of the .NET Core SDK.</span></span> <span data-ttu-id="25af4-200">在此範例中，SDK 設定為 `3.0.100`。</span><span class="sxs-lookup"><span data-stu-id="25af4-200">In this example, the SDK is set to `3.0.100`.</span></span>

![完成的 SDK 步驟](index/sdk-step-first-place.png)

<span data-ttu-id="25af4-202">若要發佈[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)，請在 `Publish` 步驟中設定 SCD，並提供[執行時間識別碼（RID）](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="25af4-202">To publish a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd), configure SCD in the `Publish` step and provide the [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

![獨立發行](index/self-contained.png)

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="25af4-204">部署獨立式預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-204">Deploy a self-contained preview app</span></span>

<span data-ttu-id="25af4-205">以預覽執行階段為目標的[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 會在部署中包含預覽執行階段。</span><span class="sxs-lookup"><span data-stu-id="25af4-205">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="25af4-206">部署獨立式應用程式時：</span><span class="sxs-lookup"><span data-stu-id="25af4-206">When deploying a self-contained app:</span></span>

* <span data-ttu-id="25af4-207">Azure App Service 中的網站不需要[預覽網站延伸模組](#install-the-preview-site-extension)。</span><span class="sxs-lookup"><span data-stu-id="25af4-207">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="25af4-208">應用程式發佈必須遵循不同於針對 [Framework 相依部署 (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd) 發佈時使用的方法。</span><span class="sxs-lookup"><span data-stu-id="25af4-208">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="25af4-209">請遵循[部署獨立式應用程式](#deploy-the-app-self-contained)一節。</span><span class="sxs-lookup"><span data-stu-id="25af4-209">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="25af4-210">將包含 Web 應用程式的 Docker 用於容器</span><span class="sxs-lookup"><span data-stu-id="25af4-210">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="25af4-211">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) 包含最新的預覽 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="25af4-211">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="25af4-212">這些映像可用作為基底映像。</span><span class="sxs-lookup"><span data-stu-id="25af4-212">The images can be used as a base image.</span></span> <span data-ttu-id="25af4-213">請使用映像，並以一般的方式將其部署至容器的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-213">Use the image and deploy to Web Apps for Containers normally.</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="25af4-214">安裝預覽網站延伸模組</span><span class="sxs-lookup"><span data-stu-id="25af4-214">Install the preview site extension</span></span>

<span data-ttu-id="25af4-215">如果使用預覽網站延伸模組發生問題，請開啟[dotnet/AspNetCore 問題](https://github.com/dotnet/AspNetCore/issues)。</span><span class="sxs-lookup"><span data-stu-id="25af4-215">If a problem occurs using the preview site extension, open an [dotnet/AspNetCore issue](https://github.com/dotnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="25af4-216">從 Azure 入口網站瀏覽至 App Service。</span><span class="sxs-lookup"><span data-stu-id="25af4-216">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="25af4-217">選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-217">Select the web app.</span></span>
1. <span data-ttu-id="25af4-218">在搜尋方塊中鍵入 "ex" 來篩選 "Extensions"，也可往下捲動管理工具的清單。</span><span class="sxs-lookup"><span data-stu-id="25af4-218">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="25af4-219">選取 [擴充功能]。</span><span class="sxs-lookup"><span data-stu-id="25af4-219">Select **Extensions**.</span></span>
1. <span data-ttu-id="25af4-220">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="25af4-220">Select **Add**.</span></span>
1. <span data-ttu-id="25af4-221">從清單選取 [ASP.NET Core {X.Y} ({x64|x86}) 執行階段] 延伸模組，其中 `{X.Y}` 是 ASP.NET Core 預覽版本，而 `{x64|x86}` 則指定平台。</span><span class="sxs-lookup"><span data-stu-id="25af4-221">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="25af4-222">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="25af4-222">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="25af4-223">選取 [確定] 安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="25af4-223">Select **OK** to install the extension.</span></span>

<span data-ttu-id="25af4-224">當作業完成後，會安裝最新的 .NET Core 預覽。</span><span class="sxs-lookup"><span data-stu-id="25af4-224">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="25af4-225">確認安裝：</span><span class="sxs-lookup"><span data-stu-id="25af4-225">Verify the installation:</span></span>

1. <span data-ttu-id="25af4-226">選取 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="25af4-226">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="25af4-227">在 [進階工具] 中選取 [移至]。</span><span class="sxs-lookup"><span data-stu-id="25af4-227">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="25af4-228">選取 [偵錯主控台] > [PowerShell] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="25af4-228">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="25af4-229">在 PowerShell 提示執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="25af4-229">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="25af4-230">在命令中使用 ASP.NET Core 執行階段版本取代 `{X.Y}`，並以平台取代 `{PLATFORM}`：</span><span class="sxs-lookup"><span data-stu-id="25af4-230">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="25af4-231">當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。</span><span class="sxs-lookup"><span data-stu-id="25af4-231">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="25af4-232">應用程式服務應用程式的平臺架構（x86/x64）會在 Azure 入口網站的應用程式設定中，針對裝載于 A 系列計算（基本）或更高主機層上的應用程式進行設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-232">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute (Basic) or higher hosting tier.</span></span> <span data-ttu-id="25af4-233">確認應用程式的發佈設定（例如，在 Visual Studio[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)）是否符合 Azure 入口網站中應用程式的服務設定中的設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-233">Confirm that the app's publish settings (for example, in the Visual Studio [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)) match the setting in the app's service configuration in the Azure portal.</span></span>
>
> <span data-ttu-id="25af4-234">如果在同處理序模式中執行應用程式，且平台架構設定為適用於 64 位元 (x64)，ASP.NET Core 模組會使用 64 位元預覽執行階段 (如果有)。</span><span class="sxs-lookup"><span data-stu-id="25af4-234">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="25af4-235">使用 Azure 入口網站安裝**ASP.NET Core {X. Y} （x64）運行**時間擴充功能。</span><span class="sxs-lookup"><span data-stu-id="25af4-235">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension using the Azure Portal.</span></span>
>
> <span data-ttu-id="25af4-236">安裝 x64 preview 執行時間之後，請在 Azure Kudu PowerShell 命令視窗中執行下列命令，以確認安裝。</span><span class="sxs-lookup"><span data-stu-id="25af4-236">After installing the x64 preview runtime, run the following command in the Azure Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="25af4-237">在下列命令中，以 `{X.Y}` 的 ASP.NET Core 執行階段版本取代：</span><span class="sxs-lookup"><span data-stu-id="25af4-237">Substitute the ASP.NET Core runtime version for `{X.Y}` in the following command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="25af4-238">當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。</span><span class="sxs-lookup"><span data-stu-id="25af4-238">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="25af4-239">**ASP.NET Core 延伸模組**可在 Azure 應用程式服務上提供適用於 ASP.NET Core 的其他功能，例如：提供 Azure 記錄。</span><span class="sxs-lookup"><span data-stu-id="25af4-239">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="25af4-240">若從 Visual Studio 部署，會自動安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="25af4-240">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="25af4-241">若未安裝延伸模組，請為應用程式安裝。</span><span class="sxs-lookup"><span data-stu-id="25af4-241">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="25af4-242">**搭配使用預覽網站延伸模組與 ARM 範本**</span><span class="sxs-lookup"><span data-stu-id="25af4-242">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="25af4-243">如果您使用 ARM 範本來建立及部署應用程式，可以使用 `siteextensions` 資源類型將網站延伸模組新增至 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-243">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="25af4-244">例如：</span><span class="sxs-lookup"><span data-stu-id="25af4-244">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="25af4-245">發佈及部署應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-245">Publish and deploy the app</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="25af4-246">若為64位部署：</span><span class="sxs-lookup"><span data-stu-id="25af4-246">For a 64-bit deployment:</span></span>

* <span data-ttu-id="25af4-247">請使用 64 位元 .NET Core SDK 來建置 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-247">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="25af4-248">在 App Service 的 [組態] > [一般設定] 中，將 [平台] 設為 [64 位元]。</span><span class="sxs-lookup"><span data-stu-id="25af4-248">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="25af4-249">應用程式必須使用基本或更高的服務方案，才能選擇平台位元。</span><span class="sxs-lookup"><span data-stu-id="25af4-249">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="25af4-250">部署依架構不同的應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-250">Deploy the app framework-dependent</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="25af4-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25af4-251">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="25af4-252">從 Visual Studio 工具列選取 [建置] > [發佈 {應用程式名稱}]，或在 [方案總管] 中以滑鼠右鍵按一下專案，然後選取 [發佈]。</span><span class="sxs-lookup"><span data-stu-id="25af4-252">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="25af4-253">在 [挑選發佈目標] 對話方塊中，確認已選取 [App Service]。</span><span class="sxs-lookup"><span data-stu-id="25af4-253">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="25af4-254">選取 [進階]。</span><span class="sxs-lookup"><span data-stu-id="25af4-254">Select **Advanced**.</span></span> <span data-ttu-id="25af4-255">[發佈] 對話方塊隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="25af4-255">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="25af4-256">在 [發行] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="25af4-256">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="25af4-257">確認已選取 [發行] 設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-257">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="25af4-258">開啟 [部署模式] 下拉式清單，然後選取 [依架構不同]。</span><span class="sxs-lookup"><span data-stu-id="25af4-258">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="25af4-259">將 [目標執行階段] 選取為 [可攜式]。</span><span class="sxs-lookup"><span data-stu-id="25af4-259">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="25af4-260">如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項] 並選取核取方塊，以移除目的地的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="25af4-260">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="25af4-261">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="25af4-261">Select **Save**.</span></span>
1. <span data-ttu-id="25af4-262">遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。</span><span class="sxs-lookup"><span data-stu-id="25af4-262">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="25af4-263">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="25af4-263">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="25af4-264">在專案檔中，請勿指定[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="25af4-264">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="25af4-265">從命令殼層使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，以發行組態來發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-265">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="25af4-266">在下列範例中，應用程式會發佈為依架構不同的應用程式：</span><span class="sxs-lookup"><span data-stu-id="25af4-266">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="25af4-267">將 bin/Release/{目標 FRAMEWORK}/publish 目錄的內容移到 App Service 中的網站。</span><span class="sxs-lookup"><span data-stu-id="25af4-267">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="25af4-268">若要在 [Kudu](https://github.com/projectkudu/kudu/wiki) 主控台將 *publish* 資料夾內容從本機硬碟或網路共用直接拖曳到 App Service，請將檔案拖曳到 Kudu 主控台中的 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="25af4-268">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="25af4-269">部署獨立式應用程式</span><span class="sxs-lookup"><span data-stu-id="25af4-269">Deploy the app self-contained</span></span>

<span data-ttu-id="25af4-270">使用 Visual Studio 或[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)的 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="25af4-270">Use Visual Studio or the .NET Core CLI for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="25af4-271">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25af4-271">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="25af4-272">從 Visual Studio 工具列選取 [建置] > [發佈 {應用程式名稱}]，或在 [方案總管] 中以滑鼠右鍵按一下專案，然後選取 [發佈]。</span><span class="sxs-lookup"><span data-stu-id="25af4-272">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="25af4-273">在 [挑選發佈目標] 對話方塊中，確認已選取 [App Service]。</span><span class="sxs-lookup"><span data-stu-id="25af4-273">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="25af4-274">選取 [進階]。</span><span class="sxs-lookup"><span data-stu-id="25af4-274">Select **Advanced**.</span></span> <span data-ttu-id="25af4-275">[發佈] 對話方塊隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="25af4-275">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="25af4-276">在 [發行] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="25af4-276">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="25af4-277">確認已選取 [發行] 設定。</span><span class="sxs-lookup"><span data-stu-id="25af4-277">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="25af4-278">開啟 [部署模式] 下拉式清單，然後選取 [獨立式]。</span><span class="sxs-lookup"><span data-stu-id="25af4-278">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="25af4-279">從 [目標執行階段] 下拉式清單中選取目標執行階段。</span><span class="sxs-lookup"><span data-stu-id="25af4-279">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="25af4-280">預設為 `win-x86`。</span><span class="sxs-lookup"><span data-stu-id="25af4-280">The default is `win-x86`.</span></span>
   * <span data-ttu-id="25af4-281">如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項] 並選取核取方塊，以移除目的地的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="25af4-281">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="25af4-282">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="25af4-282">Select **Save**.</span></span>
1. <span data-ttu-id="25af4-283">遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。</span><span class="sxs-lookup"><span data-stu-id="25af4-283">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="25af4-284">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="25af4-284">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="25af4-285">在專案檔中，指定一或多個[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="25af4-285">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="25af4-286">針對單一 RID 使用 `<RuntimeIdentifier>` (單數)，或者使用 `<RuntimeIdentifiers>` (複數) 來提供以分號分隔的 RID 清單。</span><span class="sxs-lookup"><span data-stu-id="25af4-286">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="25af4-287">在下列範例中已指定 `win-x86`：</span><span class="sxs-lookup"><span data-stu-id="25af4-287">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="25af4-288">從命令殼層中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，針對主機執行階段以 [發行] 設定來發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-288">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="25af4-289">在下列範例中，將針對 `win-x86` RID發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="25af4-289">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="25af4-290">提供給 `--runtime` 選項的 RID 必須在專案檔的 `<RuntimeIdentifier>` (或 `<RuntimeIdentifiers>`) 屬性中提供。</span><span class="sxs-lookup"><span data-stu-id="25af4-290">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86 --self-contained
   ```

1. <span data-ttu-id="25af4-291">將 bin/Release/{目標 FRAMEWORK}/{執行階段識別碼}/publish 目錄的內容移至 App Service 中的網站。</span><span class="sxs-lookup"><span data-stu-id="25af4-291">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="25af4-292">若要在 Kudu 主控台將 *publish* 資料夾內容從本機硬碟或網路共用直接拖曳到 App Service，請將檔案拖曳到 Kudu 主控台中的 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="25af4-292">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="25af4-293">通訊協定設定 (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="25af4-293">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="25af4-294">安全通訊協定繫結可讓您指定透過 HTTPS 回應要求時要使用的憑證。</span><span class="sxs-lookup"><span data-stu-id="25af4-294">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="25af4-295">繫結需要針對特定主機名稱簽發的有效私密憑證 ( *.pfx*)。</span><span class="sxs-lookup"><span data-stu-id="25af4-295">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="25af4-296">如需詳細資訊，請參閱[教學課程：將現有的自訂 SSL 憑證系結至 Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl)。</span><span class="sxs-lookup"><span data-stu-id="25af4-296">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="25af4-297">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="25af4-297">Transform web.config</span></span>

<span data-ttu-id="25af4-298">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="25af4-298">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25af4-299">其他資源</span><span class="sxs-lookup"><span data-stu-id="25af4-299">Additional resources</span></span>

* [<span data-ttu-id="25af4-300">App Service 概觀</span><span class="sxs-lookup"><span data-stu-id="25af4-300">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="25af4-301">Azure App Service：裝載 .NET 應用程式的最佳平台 (55 分鐘的概觀影片)</span><span class="sxs-lookup"><span data-stu-id="25af4-301">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="25af4-302">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="25af4-302">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="25af4-303">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="25af4-303">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="25af4-304">Windows Server 上的 Azure App Service 使用 [Internet Information Services (IIS)](https://www.iis.net/)。</span><span class="sxs-lookup"><span data-stu-id="25af4-304">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="25af4-305">有關基礎 IIS 技術的主題如下：</span><span class="sxs-lookup"><span data-stu-id="25af4-305">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="25af4-306">Windows Server - 適用於目前版本與先前版本的 IT 系統管理員內容</span><span class="sxs-lookup"><span data-stu-id="25af4-306">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
