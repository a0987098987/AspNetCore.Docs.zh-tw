---
title: 將 ASP.NET Core 應用程式部署至 Azure App Service
author: guardrex
description: 本文包含 Azure 主機和部署資源的連結。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 5035a31526e0290964e0fdee05753aeaf6cb3790
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602435"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="97bf8-103">將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="97bf8-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="97bf8-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="97bf8-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="97bf8-105">實用資源</span><span class="sxs-lookup"><span data-stu-id="97bf8-105">Useful resources</span></span>

<span data-ttu-id="97bf8-106">[App Service 文件](/azure/app-service/)是 Azure 應用程式文件、教學課程、範例、使用說明指南與其他資源的首頁。</span><span class="sxs-lookup"><span data-stu-id="97bf8-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="97bf8-107">關於裝載 ASP.NET Core 應用程式，有兩個值得參考的教學課程：</span><span class="sxs-lookup"><span data-stu-id="97bf8-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="97bf8-108">在 Azure 中建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="97bf8-109">在 Windows 上使用 Visual Studio 建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="97bf8-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="97bf8-110">在 Linux 上的 App Service 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="97bf8-111">在 Linux 上使用命令列建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="97bf8-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="97bf8-112">若要閱讀下列文章，請參閱 ASP.NET Core 文件：</span><span class="sxs-lookup"><span data-stu-id="97bf8-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="97bf8-113">了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="97bf8-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="97bf8-114">了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="97bf8-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="97bf8-115">建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="97bf8-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="97bf8-116">設定 ASP.NET Core 應用程式的 CI 組建，然後建立連續部署發行至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="97bf8-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="97bf8-117">Azure Web 應用程式沙箱</span><span class="sxs-lookup"><span data-stu-id="97bf8-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="97bf8-118">探索 Azure 應用程式平台強制實施的 Azure App Service 執行階段執行限制。</span><span class="sxs-lookup"><span data-stu-id="97bf8-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="97bf8-119">了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="97bf8-119">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="97bf8-120">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="97bf8-120">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="97bf8-121">Platform</span><span class="sxs-lookup"><span data-stu-id="97bf8-121">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="97bf8-122">Azure App Service 具有 64 位元 (x64) 及 32 位元 (x86) 應用程式的執行階段。</span><span class="sxs-lookup"><span data-stu-id="97bf8-122">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="97bf8-123">App Service 提供的 [.NET Core SDK](/dotnet/core/sdk) 為 32 位元，但您可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 主控台或 Visual Studio 中的發佈處理序，部署在本機建置的 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-123">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="97bf8-124">如需詳細資訊，請參閱[發佈與部署應用程式](#publish-and-deploy-the-app)一節。</span><span class="sxs-lookup"><span data-stu-id="97bf8-124">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="97bf8-125">對於具有原生相依性的應用程式而言，Azure App Service 具有 32 位元 (x86) 應用程式的執行階段。</span><span class="sxs-lookup"><span data-stu-id="97bf8-125">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="97bf8-126">App Service 可使用的 [.NET Core SDK](/dotnet/core/sdk) 為 32 位元。</span><span class="sxs-lookup"><span data-stu-id="97bf8-126">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="97bf8-127">如需 .NET Core 架構元件與發佈方法的詳細資訊，例如 .NET Core 執行階段和 .NET Core SDK 的相關資訊，請參閱[關於 .NET Core：組合](/dotnet/core/about#composition)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-127">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="97bf8-128">package</span><span class="sxs-lookup"><span data-stu-id="97bf8-128">Packages</span></span>

<span data-ttu-id="97bf8-129">包含下列 NuGet 套件，為部署至 Azure App Service 的應用程式提供自動記錄功能：</span><span class="sxs-lookup"><span data-stu-id="97bf8-129">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="97bf8-130">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) 提供 ASP.NET Core 與 Azure App Service 整合的啟動。</span><span class="sxs-lookup"><span data-stu-id="97bf8-130">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="97bf8-131">新增的記錄功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 套件提供。</span><span class="sxs-lookup"><span data-stu-id="97bf8-131">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="97bf8-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 執行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，以在 `Microsoft.Extensions.Logging.AzureAppServices` 套件中新增 Azure App Service 診斷記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="97bf8-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="97bf8-133">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供記錄器實作以支援 Azure App Service 診斷記錄和記錄串流功能。</span><span class="sxs-lookup"><span data-stu-id="97bf8-133">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="97bf8-134">上述套件無法從 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)取得。</span><span class="sxs-lookup"><span data-stu-id="97bf8-134">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="97bf8-135">以 .NET Framework 為目標或參考 `Microsoft.AspNetCore.App` 中繼套件的應用程式必須明確參考應用程式之專案檔中的個別套件。</span><span class="sxs-lookup"><span data-stu-id="97bf8-135">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="97bf8-136">使用 Azure 入口網站覆寫應用程式設定</span><span class="sxs-lookup"><span data-stu-id="97bf8-136">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="97bf8-137">Azure 入口網站中的應用程式設定允許您為應用程式設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="97bf8-137">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="97bf8-138">環境變數可由[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)取用。</span><span class="sxs-lookup"><span data-stu-id="97bf8-138">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="97bf8-139">在 Azure 入口網站中建立或修改應用程式設定並選取 [儲存]  按鈕後，即會重新啟動 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-139">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="97bf8-140">當服務重新啟動之後，環境變數便可供應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="97bf8-140">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="97bf8-141">當應用程式使用[一般主機](xref:fundamentals/host/generic-host)時，環境變數預設不會被載入到應用程式的設定中，而且必須由開發人員新增設定提供者。</span><span class="sxs-lookup"><span data-stu-id="97bf8-141">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="97bf8-142">新增設定提供者時，開發人員必須判斷環境變數前置詞。</span><span class="sxs-lookup"><span data-stu-id="97bf8-142">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="97bf8-143">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-143">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="97bf8-144">當應用程式使用 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 建置主機時，環境變數會將主機設定為使用 `ASPNETCORE_`前置詞。</span><span class="sxs-lookup"><span data-stu-id="97bf8-144">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="97bf8-145">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-145">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="97bf8-146">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="97bf8-146">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="97bf8-147">[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) (當裝載[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)時會設定轉送標頭中介軟體) 與 ASP.NET Core 模組會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="97bf8-147">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="97bf8-148">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="97bf8-148">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="97bf8-149">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="97bf8-150">監視與記錄</span><span class="sxs-lookup"><span data-stu-id="97bf8-150">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="97bf8-151">部署到 App Service 的 ASP.NET Core 應用程式會自動接收 App Service 延伸模組：**ASP.NET Core 記錄整合**。</span><span class="sxs-lookup"><span data-stu-id="97bf8-151">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="97bf8-152">延伸模組讓 Azure App Service 上的 ASP.NET Core 應用程式得以進行記錄整合。</span><span class="sxs-lookup"><span data-stu-id="97bf8-152">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="97bf8-153">部署到 App Service 的 ASP.NET Core 應用程式會自動接收 App Service 延伸模組 **ASP.NET Core 記錄延伸模組**。</span><span class="sxs-lookup"><span data-stu-id="97bf8-153">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="97bf8-154">延伸模組讓 Azure App Service 上的 ASP.NET Core 應用程式得以進行記錄整合。</span><span class="sxs-lookup"><span data-stu-id="97bf8-154">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="97bf8-155">如需監視、記錄及疑難排解的資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="97bf8-155">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="97bf8-156">監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-156">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="97bf8-157">了解如何檢閱應用程式和 App Service 方案的配額和計量。</span><span class="sxs-lookup"><span data-stu-id="97bf8-157">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="97bf8-158">為 Azure App Service 中的應用程式啟用診斷記錄</span><span class="sxs-lookup"><span data-stu-id="97bf8-158">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="97bf8-159">探索如何啟用及存取 HTTP 狀態碼、失敗要求和網頁伺服器活動的診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="97bf8-159">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="97bf8-160">了解處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="97bf8-160">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="97bf8-161">了解如何診斷使用 ASP.NET Core 應用程式部署 Azure App Service 的問題。</span><span class="sxs-lookup"><span data-stu-id="97bf8-161">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="97bf8-162">了解託管於 Azure App Service/IIS 之應用程式的常見部署組態錯誤，及疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="97bf8-162">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="97bf8-163">資料保護金鑰環及部署位置</span><span class="sxs-lookup"><span data-stu-id="97bf8-163">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="97bf8-164">[資料保護金鑰](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存至 *%HOME%\ASP.NET\DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="97bf8-164">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="97bf8-165">此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。</span><span class="sxs-lookup"><span data-stu-id="97bf8-165">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="97bf8-166">金鑰待用時不受保護。</span><span class="sxs-lookup"><span data-stu-id="97bf8-166">Keys aren't protected at rest.</span></span> <span data-ttu-id="97bf8-167">此資料夾對單一部署位置中應用程式的所有執行個體皆提供金鑰環。</span><span class="sxs-lookup"><span data-stu-id="97bf8-167">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="97bf8-168">各部署位置，例如預備和生產位置，不會共用金鑰環。</span><span class="sxs-lookup"><span data-stu-id="97bf8-168">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="97bf8-169">當在部署位置間交換時，使用資料保護的任何系統都將無法使用前一位置內的金鑰環，來解密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="97bf8-169">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="97bf8-170">ASP.NET Cookie 中介軟體使用資料保護來保護其 Cookie。</span><span class="sxs-lookup"><span data-stu-id="97bf8-170">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="97bf8-171">這會導致使用標準 ASP.NET Cookie 中介軟體的應用程式將使用者登出。</span><span class="sxs-lookup"><span data-stu-id="97bf8-171">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="97bf8-172">至於非相依於位置的金鑰環解決方案，請使用外部金鑰環提供者，例如：</span><span class="sxs-lookup"><span data-stu-id="97bf8-172">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="97bf8-173">Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="97bf8-173">Azure Blob Storage</span></span>
* <span data-ttu-id="97bf8-174">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="97bf8-174">Azure Key Vault</span></span>
* <span data-ttu-id="97bf8-175">SQL 存放區</span><span class="sxs-lookup"><span data-stu-id="97bf8-175">SQL store</span></span>
* <span data-ttu-id="97bf8-176">Redis 快取</span><span class="sxs-lookup"><span data-stu-id="97bf8-176">Redis cache</span></span>

<span data-ttu-id="97bf8-177">如需詳細資訊，請參閱 <xref:security/data-protection/implementation/key-storage-providers>。</span><span class="sxs-lookup"><span data-stu-id="97bf8-177">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="97bf8-178">將 ASP.NET Core 預覽版本部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="97bf8-178">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="97bf8-179">如果應用程式仰賴 .NET Core 的預覽版本，請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="97bf8-179">Use one of the following approaches if the app relies on a preview release of .NET Core:</span></span>

* <span data-ttu-id="97bf8-180">[安裝預覽網站延伸模組](#install-the-preview-site-extension)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-180">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="97bf8-181">[部署獨立式預覽應用程式](#deploy-a-self-contained-preview-app)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-181">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="97bf8-182">[將具有 Web Apps 的 Docker 用於容器](#use-docker-with-web-apps-for-containers)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-182">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="97bf8-183">安裝預覽網站延伸模組</span><span class="sxs-lookup"><span data-stu-id="97bf8-183">Install the preview site extension</span></span>

<span data-ttu-id="97bf8-184">如果您在使用預覽網站延伸模組時發生任何問題，請建立 [aspnet/AspNetCore 問題](https://github.com/aspnet/AspNetCore/issues)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-184">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="97bf8-185">從 Azure 入口網站瀏覽至 App Service。</span><span class="sxs-lookup"><span data-stu-id="97bf8-185">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="97bf8-186">選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-186">Select the web app.</span></span>
1. <span data-ttu-id="97bf8-187">在搜尋方塊中鍵入 "ex" 來篩選 "Extensions"，也可往下捲動管理工具的清單。</span><span class="sxs-lookup"><span data-stu-id="97bf8-187">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="97bf8-188">選取 [擴充功能]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-188">Select **Extensions**.</span></span>
1. <span data-ttu-id="97bf8-189">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-189">Select **Add**.</span></span>
1. <span data-ttu-id="97bf8-190">從清單選取 [ASP.NET Core {X.Y} ({x64|x86}) 執行階段]  延伸模組，其中 `{X.Y}` 是 ASP.NET Core 預覽版本，而 `{x64|x86}` 則指定平台。</span><span class="sxs-lookup"><span data-stu-id="97bf8-190">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="97bf8-191">選取 [確定]  以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="97bf8-191">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="97bf8-192">選取 [確定]  安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="97bf8-192">Select **OK** to install the extension.</span></span>

<span data-ttu-id="97bf8-193">當作業完成後，會安裝最新的 .NET Core 預覽。</span><span class="sxs-lookup"><span data-stu-id="97bf8-193">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="97bf8-194">確認安裝：</span><span class="sxs-lookup"><span data-stu-id="97bf8-194">Verify the installation:</span></span>

1. <span data-ttu-id="97bf8-195">選取 [進階工具]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-195">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="97bf8-196">在 [進階工具]  中選取 [移至]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-196">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="97bf8-197">選取 [偵錯主控台]   > [PowerShell]  功能表項目。</span><span class="sxs-lookup"><span data-stu-id="97bf8-197">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="97bf8-198">在 PowerShell 提示執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="97bf8-198">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="97bf8-199">在命令中使用 ASP.NET Core 執行階段版本取代 `{X.Y}`，並以平台取代 `{PLATFORM}`：</span><span class="sxs-lookup"><span data-stu-id="97bf8-199">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="97bf8-200">當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。</span><span class="sxs-lookup"><span data-stu-id="97bf8-200">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="97bf8-201">對於裝載於 A 系列計算或更高裝載層的應用程式，應用程式服務應用程式的平台架構 (x86/x64) 會設定在 Azure 入口網站中應用程式的設定內。</span><span class="sxs-lookup"><span data-stu-id="97bf8-201">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="97bf8-202">如果在同處理序模式中執行應用程式，且平台架構設定為適用於 64 位元 (x64)，ASP.NET Core 模組會使用 64 位元預覽執行階段 (如果有)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-202">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="97bf8-203">請安裝 [ASP.NET Core {X.Y} (x64) 執行階段]  延伸模組。</span><span class="sxs-lookup"><span data-stu-id="97bf8-203">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="97bf8-204">在安裝 x64 預覽執行階段後，請在 Kudu PowerShell 命令視窗中執行下列命令，以確認安裝。</span><span class="sxs-lookup"><span data-stu-id="97bf8-204">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="97bf8-205">在命令中使用 ASP.NET Core 執行階段版本取代 `{X.Y}`：</span><span class="sxs-lookup"><span data-stu-id="97bf8-205">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="97bf8-206">當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。</span><span class="sxs-lookup"><span data-stu-id="97bf8-206">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="97bf8-207">**ASP.NET Core 延伸模組**可在 Azure 應用程式服務上提供適用於 ASP.NET Core 的其他功能，例如：提供 Azure 記錄。</span><span class="sxs-lookup"><span data-stu-id="97bf8-207">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="97bf8-208">若從 Visual Studio 部署，會自動安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="97bf8-208">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="97bf8-209">若未安裝延伸模組，請為應用程式安裝。</span><span class="sxs-lookup"><span data-stu-id="97bf8-209">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="97bf8-210">**搭配使用預覽網站延伸模組與 ARM 範本**</span><span class="sxs-lookup"><span data-stu-id="97bf8-210">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="97bf8-211">如果您使用 ARM 範本來建立及部署應用程式，可以使用 `siteextensions` 資源類型將網站延伸模組新增至 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-211">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="97bf8-212">例如：</span><span class="sxs-lookup"><span data-stu-id="97bf8-212">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="97bf8-213">部署獨立式預覽應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-213">Deploy a self-contained preview app</span></span>

<span data-ttu-id="97bf8-214">以預覽執行階段為目標的[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 會在部署中包含預覽執行階段。</span><span class="sxs-lookup"><span data-stu-id="97bf8-214">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="97bf8-215">部署獨立式應用程式時：</span><span class="sxs-lookup"><span data-stu-id="97bf8-215">When deploying a self-contained app:</span></span>

* <span data-ttu-id="97bf8-216">Azure App Service 中的網站不需要[預覽網站延伸模組](#install-the-preview-site-extension)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-216">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="97bf8-217">應用程式發佈必須遵循不同於針對 [Framework 相依部署 (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd) 發佈時使用的方法。</span><span class="sxs-lookup"><span data-stu-id="97bf8-217">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="97bf8-218">請遵循[部署獨立式應用程式](#deploy-the-app-self-contained)一節。</span><span class="sxs-lookup"><span data-stu-id="97bf8-218">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="97bf8-219">將包含 Web 應用程式的 Docker 用於容器</span><span class="sxs-lookup"><span data-stu-id="97bf8-219">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="97bf8-220">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) 包含最新的預覽 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="97bf8-220">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="97bf8-221">這些映像可用作為基底映像。</span><span class="sxs-lookup"><span data-stu-id="97bf8-221">The images can be used as a base image.</span></span> <span data-ttu-id="97bf8-222">請使用映像，並以一般的方式將其部署至容器的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-222">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="97bf8-223">發佈及部署應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-223">Publish and deploy the app</span></span>

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="97bf8-224">部署依架構不同的應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-224">Deploy the app framework-dependent</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="97bf8-225">若是 64 位元[依架構不同的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)：</span><span class="sxs-lookup"><span data-stu-id="97bf8-225">For a 64-bit [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

* <span data-ttu-id="97bf8-226">請使用 64 位元 .NET Core SDK 來建置 64 位元應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-226">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="97bf8-227">在 App Service 的 [組態]   > [一般設定]  中，將 [平台]  設為 [64 位元]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-227">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="97bf8-228">應用程式必須使用基本或更高的服務方案，才能選擇平台位元。</span><span class="sxs-lookup"><span data-stu-id="97bf8-228">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="97bf8-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97bf8-229">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="97bf8-230">從 Visual Studio 工具列選取 [建置]   > [發佈 {應用程式名稱}]  ，或在 [方案總管]  中以滑鼠右鍵按一下專案，然後選取 [發佈]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-230">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="97bf8-231">在 [挑選發佈目標]  對話方塊中，確認已選取 [App Service]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-231">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="97bf8-232">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-232">Select **Advanced**.</span></span> <span data-ttu-id="97bf8-233">[發佈]  對話方塊隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="97bf8-233">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="97bf8-234">在 [發行]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="97bf8-234">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="97bf8-235">確認已選取 [發行]  設定。</span><span class="sxs-lookup"><span data-stu-id="97bf8-235">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="97bf8-236">開啟 [部署模式]  下拉式清單，然後選取 [依架構不同]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-236">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="97bf8-237">將 [目標執行階段]  選取為 [可攜式]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-237">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="97bf8-238">如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項]  並選取核取方塊，以移除目的地的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="97bf8-238">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="97bf8-239">選取 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-239">Select **Save**.</span></span>
1. <span data-ttu-id="97bf8-240">遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。</span><span class="sxs-lookup"><span data-stu-id="97bf8-240">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="97bf8-241">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="97bf8-241">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="97bf8-242">在專案檔中，請勿指定[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-242">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="97bf8-243">從命令殼層使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，以發行組態來發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-243">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="97bf8-244">在下列範例中，應用程式會發佈為依架構不同的應用程式：</span><span class="sxs-lookup"><span data-stu-id="97bf8-244">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="97bf8-245">將 bin/Release/{目標 FRAMEWORK}/publish  目錄的內容移到 App Service 中的網站。</span><span class="sxs-lookup"><span data-stu-id="97bf8-245">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="97bf8-246">若要在 [Kudu](https://github.com/projectkudu/kudu/wiki) 主控台將 *publish* 資料夾內容從本機硬碟或網路共用直接拖曳到 App Service，請將檔案拖曳到 Kudu 主控台中的 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="97bf8-246">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="97bf8-247">部署獨立式應用程式</span><span class="sxs-lookup"><span data-stu-id="97bf8-247">Deploy the app self-contained</span></span>

<span data-ttu-id="97bf8-248">若是[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)，請使用 Visual Studio 或命令列介面 (CLI) 工具。</span><span class="sxs-lookup"><span data-stu-id="97bf8-248">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="97bf8-249">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97bf8-249">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="97bf8-250">從 Visual Studio 工具列選取 [建置]   > [發佈 {應用程式名稱}]  ，或在 [方案總管]  中以滑鼠右鍵按一下專案，然後選取 [發佈]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-250">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="97bf8-251">在 [挑選發佈目標]  對話方塊中，確認已選取 [App Service]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-251">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="97bf8-252">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-252">Select **Advanced**.</span></span> <span data-ttu-id="97bf8-253">[發佈]  對話方塊隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="97bf8-253">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="97bf8-254">在 [發行]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="97bf8-254">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="97bf8-255">確認已選取 [發行]  設定。</span><span class="sxs-lookup"><span data-stu-id="97bf8-255">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="97bf8-256">開啟 [部署模式]  下拉式清單，然後選取 [獨立式]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-256">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="97bf8-257">從 [目標執行階段]  下拉式清單中選取目標執行階段。</span><span class="sxs-lookup"><span data-stu-id="97bf8-257">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="97bf8-258">預設為 `win-x86`。</span><span class="sxs-lookup"><span data-stu-id="97bf8-258">The default is `win-x86`.</span></span>
   * <span data-ttu-id="97bf8-259">如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項]  並選取核取方塊，以移除目的地的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="97bf8-259">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="97bf8-260">選取 [儲存]  。</span><span class="sxs-lookup"><span data-stu-id="97bf8-260">Select **Save**.</span></span>
1. <span data-ttu-id="97bf8-261">遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。</span><span class="sxs-lookup"><span data-stu-id="97bf8-261">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="97bf8-262">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="97bf8-262">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="97bf8-263">在專案檔中，指定一或多個[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-263">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="97bf8-264">針對單一 RID 使用 `<RuntimeIdentifier>` (單數)，或者使用 `<RuntimeIdentifiers>` (複數) 來提供以分號分隔的 RID 清單。</span><span class="sxs-lookup"><span data-stu-id="97bf8-264">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="97bf8-265">在下列範例中已指定 `win-x86`：</span><span class="sxs-lookup"><span data-stu-id="97bf8-265">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="97bf8-266">從命令殼層中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，針對主機執行階段以 [發行] 設定來發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-266">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="97bf8-267">在下列範例中，將針對 `win-x86` RID發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="97bf8-267">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="97bf8-268">提供給 `--runtime` 選項的 RID 必須在專案檔的 `<RuntimeIdentifier>` (或 `<RuntimeIdentifiers>`) 屬性中提供。</span><span class="sxs-lookup"><span data-stu-id="97bf8-268">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="97bf8-269">將 bin/Release/{目標 FRAMEWORK}/{執行階段識別碼}/publish  目錄的內容移至 App Service 中的網站。</span><span class="sxs-lookup"><span data-stu-id="97bf8-269">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="97bf8-270">若要在 Kudu 主控台將 *publish* 資料夾內容從本機硬碟或網路共用直接拖曳到 App Service，請將檔案拖曳到 Kudu 主控台中的 `D:\home\site\wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="97bf8-270">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="97bf8-271">通訊協定設定 (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="97bf8-271">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="97bf8-272">安全通訊協定繫結可讓您指定透過 HTTPS 回應要求時要使用的憑證。</span><span class="sxs-lookup"><span data-stu-id="97bf8-272">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="97bf8-273">繫結需要針對特定主機名稱簽發的有效私密憑證 ( *.pfx*)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-273">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="97bf8-274">如需詳細資訊，請參閱[教學課程：將現有的自訂 SSL 憑證繫結至 Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-274">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="97bf8-275">轉換 web.config</span><span class="sxs-lookup"><span data-stu-id="97bf8-275">Transform web.config</span></span>

<span data-ttu-id="97bf8-276">如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="97bf8-276">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97bf8-277">其他資源</span><span class="sxs-lookup"><span data-stu-id="97bf8-277">Additional resources</span></span>

* [<span data-ttu-id="97bf8-278">App Service 概觀</span><span class="sxs-lookup"><span data-stu-id="97bf8-278">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="97bf8-279">Azure App Service：裝載 .NET 應用程式的最佳平台 (55 分鐘的概觀影片)</span><span class="sxs-lookup"><span data-stu-id="97bf8-279">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="97bf8-280">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="97bf8-280">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="97bf8-281">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="97bf8-281">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="97bf8-282">Windows Server 上的 Azure App Service 使用 [Internet Information Services (IIS)](https://www.iis.net/)。</span><span class="sxs-lookup"><span data-stu-id="97bf8-282">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="97bf8-283">有關基礎 IIS 技術的主題如下：</span><span class="sxs-lookup"><span data-stu-id="97bf8-283">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="97bf8-284">Windows Server - 適用於目前版本與先前版本的 IT 系統管理員內容</span><span class="sxs-lookup"><span data-stu-id="97bf8-284">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
