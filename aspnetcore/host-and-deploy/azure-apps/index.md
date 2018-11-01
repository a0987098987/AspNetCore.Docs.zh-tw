---
title: 將 ASP.NET Core 應用程式部署至 Azure App Service
author: guardrex
description: 本文包含 Azure 主機和部署資源的連結。
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 315261c4d20970fc399cc2a879dd452bdf3be93f
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326052"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="0bd1d-103">將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0bd1d-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="0bd1d-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="0bd1d-105">實用資源</span><span class="sxs-lookup"><span data-stu-id="0bd1d-105">Useful resources</span></span>

<span data-ttu-id="0bd1d-106">Azure [Web Apps 文件](/azure/app-service/)是 Azure 應用程式文件、教學課程、範例、使用說明指南及其他資源的首頁。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="0bd1d-107">關於裝載 ASP.NET Core 應用程式，有兩個值得參考的教學課程：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="0bd1d-108">快速入門：在 Azure 中建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0bd1d-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="0bd1d-109">在 Windows 上使用 Visual Studio 建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="0bd1d-110">快速入門：在 Linux 上於 App Service 中建立 .NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="0bd1d-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="0bd1d-111">在 Linux 上使用命令列建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="0bd1d-112">若要閱讀下列文章，請參閱 ASP.NET Core 文件：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="0bd1d-113">使用 Visual Studio 發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="0bd1d-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="0bd1d-114">了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="0bd1d-115">使用 Visual Studio 與 Git 持續部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="0bd1d-115">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="0bd1d-116">了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-116">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="0bd1d-117">使用 Azure Pipelines 建立您的第一個管線</span><span class="sxs-lookup"><span data-stu-id="0bd1d-117">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="0bd1d-118">設定 ASP.NET Core 應用程式的 CI 組建，然後建立連續部署發行至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-118">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="0bd1d-119">Azure Web 應用程式沙箱</span><span class="sxs-lookup"><span data-stu-id="0bd1d-119">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="0bd1d-120">探索 Azure 應用程式平台強制實施的 Azure App Service 執行階段執行限制。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-120">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="0bd1d-121">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="0bd1d-121">Application configuration</span></span>

<span data-ttu-id="0bd1d-122">在 ASP.NET 2.0 或更新版本中，以下 NuGet 套件會為部署至 Azure App Service 的應用程式提供自動記錄功能：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-122">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="0bd1d-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) 提供 ASP.NET Core 與 Azure App Service 整合的啟動。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="0bd1d-124">新增的記錄功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 套件提供。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="0bd1d-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 執行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，以在 `Microsoft.Extensions.Logging.AzureAppServices` 套件中新增 Azure App Service 診斷記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="0bd1d-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供記錄器實作以支援 Azure App Service 診斷記錄和記錄串流功能。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="0bd1d-127">若以 .NET Core 為目標且參考 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)，則會已經包含套件。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-127">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="0bd1d-128">較新的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中不存在套件。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-128">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="0bd1d-129">若以 .NET Framework 為目標或參考 `Microsoft.AspNetCore.App` 中繼套件，則會參考個別記錄套件。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-129">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="0bd1d-130">使用 Azure 入口網站覆寫應用程式設定</span><span class="sxs-lookup"><span data-stu-id="0bd1d-130">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="0bd1d-131">[應用程式設定] 刀鋒視窗的 [應用程式設定] 區域允許您為應用程式設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-131">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="0bd1d-132">環境變數可由[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)取用。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-132">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="0bd1d-133">在 Azure 入口網站中建立或修改應用程式設定並選取 [儲存] 按鈕後，即會重新啟動 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-133">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="0bd1d-134">當服務重新啟動之後，環境變數便可供應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-134">The environment variable is available to the app after the service restarts.</span></span>

<span data-ttu-id="0bd1d-135">當應用程式使用 [Web 主機](xref:fundamentals/host/web-host)並使用會將主機設定為使用 `ASPNETCORE_` 前置詞的 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 環境變數來建置主機。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="0bd1d-136">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="0bd1d-137">當應用程式使用[一般主機](xref:fundamentals/host/generic-host)時，環境變數預設不會被載入到應用程式的設定中，而且必須由開發人員新增設定提供者。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="0bd1d-138">新增設定提供者時，開發人員必須判斷環境變數前置詞。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="0bd1d-139">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0bd1d-140">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="0bd1d-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0bd1d-141">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 IIS Integration 中介軟體會設定為轉送配置 (HTTP/HTTPS) 及發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="0bd1d-142">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="0bd1d-143">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="0bd1d-144">監視與記錄</span><span class="sxs-lookup"><span data-stu-id="0bd1d-144">Monitoring and logging</span></span>

<span data-ttu-id="0bd1d-145">如需監視、記錄及疑難排解的資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="0bd1d-146">如何：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="0bd1d-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="0bd1d-147">了解如何檢閱應用程式和 App Service 方案的配額和計量。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="0bd1d-148">為 Azure App Service 中的 Web 應用程式啟用診斷記錄</span><span class="sxs-lookup"><span data-stu-id="0bd1d-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="0bd1d-149">探索如何啟用及存取 HTTP 狀態碼、失敗要求和網頁伺服器活動的診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="0bd1d-150">ASP.NET Core 中的錯誤處理簡介</span><span class="sxs-lookup"><span data-stu-id="0bd1d-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="0bd1d-151">了解處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="0bd1d-152">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="0bd1d-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="0bd1d-153">了解如何診斷使用 ASP.NET Core 應用程式部署 Azure App Service 的問題。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="0bd1d-154">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="0bd1d-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="0bd1d-155">了解託管於 Azure App Service/IIS 之應用程式的常見部署組態錯誤，及疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="0bd1d-156">資料保護金鑰環及部署位置</span><span class="sxs-lookup"><span data-stu-id="0bd1d-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="0bd1d-157">[資料保護金鑰](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存至 *%HOME%\ASP.NET\DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="0bd1d-158">此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="0bd1d-159">金鑰待用時不受保護。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="0bd1d-160">此資料夾對單一部署位置中應用程式的所有執行個體皆提供金鑰環。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="0bd1d-161">各部署位置，例如預備和生產位置，不會共用金鑰環。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="0bd1d-162">當在部署位置間交換時，使用資料保護的任何系統都將無法使用前一位置內的金鑰環，來解密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="0bd1d-163">ASP.NET Cookie 中介軟體使用資料保護來保護其 Cookie。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="0bd1d-164">這會導致使用標準 ASP.NET Cookie 中介軟體的應用程式將使用者登出。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="0bd1d-165">至於非相依於位置的金鑰環解決方案，請使用外部金鑰環提供者，例如：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="0bd1d-166">Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="0bd1d-166">Azure Blob Storage</span></span>
* <span data-ttu-id="0bd1d-167">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0bd1d-167">Azure Key Vault</span></span>
* <span data-ttu-id="0bd1d-168">SQL 存放區</span><span class="sxs-lookup"><span data-stu-id="0bd1d-168">SQL store</span></span>
* <span data-ttu-id="0bd1d-169">Redis 快取</span><span class="sxs-lookup"><span data-stu-id="0bd1d-169">Redis cache</span></span>

<span data-ttu-id="0bd1d-170">如需詳細資訊，請參閱[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="0bd1d-171">將 ASP.NET Core 預覽版本部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0bd1d-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="0bd1d-172">請使用下列的其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-172">Use one of the following approaches:</span></span>

* <span data-ttu-id="0bd1d-173">[安裝預覽網站延伸模組](#install-the-preview-site-extension)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-173">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="0bd1d-174">[部署獨立式應用程式](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="0bd1d-174">[Deploy the app self-contained](#deploy-the-app-self-contained).</span></span>
* <span data-ttu-id="0bd1d-175">[將具有 Web Apps 的 Docker 用於容器](#use-docker-with-web-apps-for-containers)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-175">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="0bd1d-176">安裝預覽網站延伸模組</span><span class="sxs-lookup"><span data-stu-id="0bd1d-176">Install the preview site extension</span></span>

<span data-ttu-id="0bd1d-177">如果您在使用預覽網站延伸模組時發生任何問題，請在 [GitHub](https://github.com/aspnet/azureintegration/issues/new) 上提出問題。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-177">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="0bd1d-178">從 Azure 入口網站中，巡覽至 [App Service] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-178">From the Azure Portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="0bd1d-179">選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-179">Select the web app.</span></span>
1. <span data-ttu-id="0bd1d-180">在搜尋方塊中鍵入"ex"，或將管理區段的清單向下捲動至 [開發工具]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-180">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="0bd1d-181">選取 [開發工具] > [延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-181">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="0bd1d-182">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-182">Select **Add**.</span></span>
1. <span data-ttu-id="0bd1d-183">從清單選取 [ASP.NET Core &lt;x.y&gt; (x86) 執行階段] 延伸模組，其中 `<x.y>` 是 ASP.NET Core 預覽版本 (例如 **ASP.NET Core 2.2 (x86) 執行階段**)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-183">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="0bd1d-184">x86 執行階段適用於[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，其依賴由 ASP.NET Core 模組裝載的跨處理序。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-184">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="0bd1d-185">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-185">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="0bd1d-186">選取 [確定] 安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-186">Select **OK** to install the extension.</span></span>

<span data-ttu-id="0bd1d-187">當作業完成後，會安裝最新的 .NET Core 預覽。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-187">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="0bd1d-188">確認安裝：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-188">Verify the installation:</span></span>

1. <span data-ttu-id="0bd1d-189">選取 [開發工具] 下的 [進階工具]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-189">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="0bd1d-190">選取 [進階工具] 刀鋒視窗上的 [開始]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-190">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="0bd1d-191">選取 [偵錯主控台] > [PowerShell] 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-191">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="0bd1d-192">在 PowerShell 提示執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-192">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="0bd1d-193">在命令中使用 ASP.NET Core 執行階段版本取代 `<x.y>`：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-193">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="0bd1d-194">如果安裝的預覽執行階段適用於 ASP.NET Core 2.2，命令就會是：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-194">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="0bd1d-195">當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-195">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="0bd1d-196">對於裝載於 A 系列計算或更高裝載層的應用程式，應用程式服務應用程式的平台架構 (x86 x64) 設定在 [一般設定] 下的 [應用程式設定] 刀鋒視窗中。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-196">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="0bd1d-197">如果在同處理序模式中執行應用程式，且平台架構設定為適用於 64 位元 (x64)，ASP.NET Core 模組會使用 64 位元預覽執行階段 (如果有)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-197">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="0bd1d-198">安裝 **ASP.NET Core &lt;x.y&gt; (x64) 執行階段**延伸模組 (例如 **ASP.NET Core 2.2 (x64) 執行階段**)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-198">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="0bd1d-199">在安裝 x64 預覽執行階段後，請在 Kudu PowerShell 命令視窗中執行下列命令，以確認安裝。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-199">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="0bd1d-200">在命令中使用 ASP.NET Core 執行階段版本取代 `<x.y>`：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-200">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="0bd1d-201">如果安裝的預覽執行階段適用於 ASP.NET Core 2.2，命令就會是：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-201">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="0bd1d-202">當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-202">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0bd1d-203">**ASP.NET Core 延伸模組**可在 Azure 應用程式服務上提供適用於 ASP.NET Core 的其他功能，例如：提供 Azure 記錄。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-203">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="0bd1d-204">若從 Visual Studio 部署，會自動安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-204">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="0bd1d-205">若未安裝延伸模組，請為應用程式安裝。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-205">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="0bd1d-206">**搭配使用預覽網站延伸模組與 ARM 範本**</span><span class="sxs-lookup"><span data-stu-id="0bd1d-206">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="0bd1d-207">如果您使用 ARM 範本來建立及部署應用程式，可以使用 `siteextensions` 資源類型將網站延伸模組新增至 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-207">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="0bd1d-208">例如: </span><span class="sxs-lookup"><span data-stu-id="0bd1d-208">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="0bd1d-209">部署獨立式應用程式</span><span class="sxs-lookup"><span data-stu-id="0bd1d-209">Deploy the app self-contained</span></span>

<span data-ttu-id="0bd1d-210">以預覽執行階段為目標的[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 會在部署中包含預覽執行階段。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-210">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="0bd1d-211">部署獨立式應用程式時：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-211">When deploying a self-contained app:</span></span>

* <span data-ttu-id="0bd1d-212">Azure App Service 中的網站不需要[預覽網站延伸模組](#install-the-preview-site-extension)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-212">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="0bd1d-213">應用程式發佈必須遵循不同於針對 [Framework 相依部署 (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd) 發佈時使用的方法。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-213">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

#### <a name="publish-from-visual-studio"></a><span data-ttu-id="0bd1d-214">從 Visual Studio 發佈</span><span class="sxs-lookup"><span data-stu-id="0bd1d-214">Publish from Visual Studio</span></span>

1. <span data-ttu-id="0bd1d-215">從 Visual Studio 工具列中選取 [建置] > [發佈 {應用程式名稱}]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-215">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar.</span></span>
1. <span data-ttu-id="0bd1d-216">在 [挑選發佈目標] 對話方塊中，確認已選取 [App Service]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-216">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="0bd1d-217">選取 [進階]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-217">Select **Advanced**.</span></span> <span data-ttu-id="0bd1d-218">[發佈] 對話方塊隨即開啟。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-218">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="0bd1d-219">在 [發行] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-219">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="0bd1d-220">確認已選取 [發行] 設定。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-220">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="0bd1d-221">開啟 [部署模式] 下拉式清單，然後選取 [獨立式]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-221">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="0bd1d-222">從 [目標執行階段] 下拉式清單中選取目標執行階段。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-222">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="0bd1d-223">預設為 `win-x86`。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-223">The default is `win-x86`.</span></span>
   * <span data-ttu-id="0bd1d-224">如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項] 並選取核取方塊，以移除目的地的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-224">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="0bd1d-225">選取 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-225">Select **Save**.</span></span>
1. <span data-ttu-id="0bd1d-226">遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-226">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

#### <a name="publish-using-command-line-interface-cli-tools"></a><span data-ttu-id="0bd1d-227">使用命令列介面 (CLI) 工具發佈</span><span class="sxs-lookup"><span data-stu-id="0bd1d-227">Publish using command-line interface (CLI) tools</span></span>

1. <span data-ttu-id="0bd1d-228">在專案檔中，指定一或多個[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-228">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="0bd1d-229">針對單一 RID 使用 `<RuntimeIdentifier>` (單數)，或者使用 `<RuntimeIdentifiers>` (複數) 來提供以分號分隔的 RID 清單。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-229">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="0bd1d-230">在下列範例中已指定 `win-x86`：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-230">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```
1. <span data-ttu-id="0bd1d-231">從命令殼層中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，針對主機執行階段以 [發行] 設定來發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-231">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="0bd1d-232">在下列範例中，將針對 `win-x86` RID發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-232">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="0bd1d-233">提供給 `--runtime` 選項的 RID 必須在專案檔的 `<RuntimeIdentifier>` (或 `<RuntimeIdentifiers>`) 屬性中提供。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-233">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```
1. <span data-ttu-id="0bd1d-234">將 bin/Release/{目標 FRAMEWORK}/{執行階段識別碼}/publish 目錄的內容移至 App Service 中的網站。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-234">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="0bd1d-235">將包含 Web 應用程式的 Docker 用於容器</span><span class="sxs-lookup"><span data-stu-id="0bd1d-235">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="0bd1d-236">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) 包含最新的預覽 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-236">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="0bd1d-237">這些映像可用作為基底映像。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-237">The images can be used as a base image.</span></span> <span data-ttu-id="0bd1d-238">請使用映像，並以一般的方式將其部署至容器的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-238">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0bd1d-239">其他資源</span><span class="sxs-lookup"><span data-stu-id="0bd1d-239">Additional resources</span></span>

* [<span data-ttu-id="0bd1d-240">Web Apps 概觀 (5 分鐘的概觀影片)</span><span class="sxs-lookup"><span data-stu-id="0bd1d-240">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="0bd1d-241">Azure App Service：裝載 .NET 應用程式的最佳平台 (55 分鐘的概觀影片)</span><span class="sxs-lookup"><span data-stu-id="0bd1d-241">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="0bd1d-242">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="0bd1d-242">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="0bd1d-243">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="0bd1d-243">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="0bd1d-244">Windows Server 上的 Azure App Service 使用 [Internet Information Services (IIS)](https://www.iis.net/)。</span><span class="sxs-lookup"><span data-stu-id="0bd1d-244">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="0bd1d-245">有關基礎 IIS 技術的主題如下：</span><span class="sxs-lookup"><span data-stu-id="0bd1d-245">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="0bd1d-246">Microsoft TechNet Library：Windows Server</span><span class="sxs-lookup"><span data-stu-id="0bd1d-246">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
