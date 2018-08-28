---
title: 將 ASP.NET Core 裝載到 Azure App Service
author: guardrex
description: 探索如何在 Azure App Service 中裝載 ASP.NET Core 應用程式，及實用資源的連結。
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 42775bf4d3e88893260a5973f6f7bc9d3a006b5a
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927824"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="11c0a-103">將 ASP.NET Core 裝載到 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="11c0a-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="11c0a-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="11c0a-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="11c0a-105">實用資源</span><span class="sxs-lookup"><span data-stu-id="11c0a-105">Useful resources</span></span>

<span data-ttu-id="11c0a-106">Azure [Web Apps 文件](/azure/app-service/)是 Azure 應用程式文件、教學課程、範例、使用說明指南及其他資源的首頁。</span><span class="sxs-lookup"><span data-stu-id="11c0a-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="11c0a-107">關於裝載 ASP.NET Core 應用程式，有兩個值得參考的教學課程：</span><span class="sxs-lookup"><span data-stu-id="11c0a-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="11c0a-108">快速入門：在 Azure 中建立 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="11c0a-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="11c0a-109">在 Windows 上使用 Visual Studio 建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="11c0a-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="11c0a-110">快速入門：在 Linux 上於 App Service 中建立 .NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="11c0a-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="11c0a-111">在 Linux 上使用命令列建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="11c0a-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="11c0a-112">若要閱讀下列文章，請參閱 ASP.NET Core 文件：</span><span class="sxs-lookup"><span data-stu-id="11c0a-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="11c0a-113">使用 Visual Studio 發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="11c0a-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="11c0a-114">了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="11c0a-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="11c0a-115">使用 CLI 工具發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="11c0a-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="11c0a-116">了解如何使用 Git 命令列用戶端將 ASP.NET Core 應用程式發佈到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="11c0a-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="11c0a-117">使用 Visual Studio 與 Git 持續部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="11c0a-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="11c0a-118">了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="11c0a-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="11c0a-119">使用 VSTS 持續部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="11c0a-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="11c0a-120">設定 ASP.NET Core 應用程式的 CI 組建，然後建立連續部署發行至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="11c0a-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="11c0a-121">Azure Web 應用程式沙箱</span><span class="sxs-lookup"><span data-stu-id="11c0a-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="11c0a-122">探索 Azure 應用程式平台強制實施的 Azure App Service 執行階段執行限制。</span><span class="sxs-lookup"><span data-stu-id="11c0a-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="11c0a-123">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="11c0a-123">Application configuration</span></span>

<span data-ttu-id="11c0a-124">在 ASP.NET 2.0 或更新版本中，以下 NuGet 套件會為部署至 Azure App Service 的應用程式提供自動記錄功能：</span><span class="sxs-lookup"><span data-stu-id="11c0a-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="11c0a-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) 提供 ASP.NET Core 與 Azure App Service 整合的啟動。</span><span class="sxs-lookup"><span data-stu-id="11c0a-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="11c0a-126">新增的記錄功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 套件提供。</span><span class="sxs-lookup"><span data-stu-id="11c0a-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="11c0a-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 執行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，以在 `Microsoft.Extensions.Logging.AzureAppServices` 套件中新增 Azure App Service 診斷記錄提供者。</span><span class="sxs-lookup"><span data-stu-id="11c0a-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="11c0a-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供記錄器實作以支援 Azure App Service 診斷記錄和記錄串流功能。</span><span class="sxs-lookup"><span data-stu-id="11c0a-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="11c0a-129">若以 .NET Core 為目標且參考 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)，則會已經包含套件。</span><span class="sxs-lookup"><span data-stu-id="11c0a-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="11c0a-130">較新的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中不存在套件。</span><span class="sxs-lookup"><span data-stu-id="11c0a-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="11c0a-131">若以 .NET Framework 為目標或參考 `Microsoft.AspNetCore.App` 中繼套件，則會參考個別記錄套件。</span><span class="sxs-lookup"><span data-stu-id="11c0a-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="11c0a-132">使用 Azure 入口網站覆寫應用程式設定</span><span class="sxs-lookup"><span data-stu-id="11c0a-132">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="11c0a-133">[應用程式設定] 刀鋒視窗的 [應用程式設定] 區域允許您為應用程式設定環境變數。</span><span class="sxs-lookup"><span data-stu-id="11c0a-133">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="11c0a-134">環境變數可由[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)取用。</span><span class="sxs-lookup"><span data-stu-id="11c0a-134">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="11c0a-135">當應用程式使用 [Web 主機](xref:fundamentals/host/web-host)並使用會將主機設定為使用 `ASPNETCORE_` 前置詞的 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 環境變數來建置主機。</span><span class="sxs-lookup"><span data-stu-id="11c0a-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="11c0a-136">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="11c0a-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="11c0a-137">當應用程式使用[一般主機](xref:fundamentals/host/generic-host)時，環境變數預設不會被載入到應用程式的設定中，而且必須由開發人員新增設定提供者。</span><span class="sxs-lookup"><span data-stu-id="11c0a-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="11c0a-138">新增設定提供者時，開發人員必須判斷環境變數前置詞。</span><span class="sxs-lookup"><span data-stu-id="11c0a-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="11c0a-139">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="11c0a-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="11c0a-140">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="11c0a-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="11c0a-141">用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 IIS Integration 中介軟體會設定為轉送配置 (HTTP/HTTPS) 及發出要求的遠端 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="11c0a-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="11c0a-142">其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。</span><span class="sxs-lookup"><span data-stu-id="11c0a-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="11c0a-143">如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。</span><span class="sxs-lookup"><span data-stu-id="11c0a-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="11c0a-144">監視與記錄</span><span class="sxs-lookup"><span data-stu-id="11c0a-144">Monitoring and logging</span></span>

<span data-ttu-id="11c0a-145">如需監視、記錄及疑難排解的資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="11c0a-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="11c0a-146">如何：監視 Azure App Service 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="11c0a-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="11c0a-147">了解如何檢閱應用程式和 App Service 方案的配額和計量。</span><span class="sxs-lookup"><span data-stu-id="11c0a-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="11c0a-148">為 Azure App Service 中的 Web 應用程式啟用診斷記錄</span><span class="sxs-lookup"><span data-stu-id="11c0a-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="11c0a-149">探索如何啟用及存取 HTTP 狀態碼、失敗要求和網頁伺服器活動的診斷記錄。</span><span class="sxs-lookup"><span data-stu-id="11c0a-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="11c0a-150">ASP.NET Core 中的錯誤處理簡介</span><span class="sxs-lookup"><span data-stu-id="11c0a-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="11c0a-151">了解處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="11c0a-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="11c0a-152">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="11c0a-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="11c0a-153">了解如何診斷使用 ASP.NET Core 應用程式部署 Azure App Service 的問題。</span><span class="sxs-lookup"><span data-stu-id="11c0a-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="11c0a-154">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="11c0a-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="11c0a-155">了解託管於 Azure App Service/IIS 之應用程式的常見部署設定錯誤，及疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="11c0a-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="11c0a-156">資料保護金鑰環及部署位置</span><span class="sxs-lookup"><span data-stu-id="11c0a-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="11c0a-157">[資料保護金鑰](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存至 *%HOME%\ASP.NET\DataProtection-Keys* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="11c0a-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="11c0a-158">此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。</span><span class="sxs-lookup"><span data-stu-id="11c0a-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="11c0a-159">金鑰待用時不受保護。</span><span class="sxs-lookup"><span data-stu-id="11c0a-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="11c0a-160">此資料夾對單一部署位置中應用程式的所有執行個體皆提供金鑰環。</span><span class="sxs-lookup"><span data-stu-id="11c0a-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="11c0a-161">各部署位置，例如預備和生產位置，不會共用金鑰環。</span><span class="sxs-lookup"><span data-stu-id="11c0a-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="11c0a-162">當在部署位置間交換時，使用資料保護的任何系統都將無法使用前一位置內的金鑰環，來解密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="11c0a-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="11c0a-163">ASP.NET Cookie 中介軟體使用資料保護來保護其 Cookie。</span><span class="sxs-lookup"><span data-stu-id="11c0a-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="11c0a-164">這會導致使用標準 ASP.NET Cookie 中介軟體的應用程式將使用者登出。</span><span class="sxs-lookup"><span data-stu-id="11c0a-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="11c0a-165">至於非相依於位置的金鑰環解決方案，請使用外部金鑰環提供者，例如：</span><span class="sxs-lookup"><span data-stu-id="11c0a-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="11c0a-166">Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="11c0a-166">Azure Blob Storage</span></span>
* <span data-ttu-id="11c0a-167">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="11c0a-167">Azure Key Vault</span></span>
* <span data-ttu-id="11c0a-168">SQL 存放區</span><span class="sxs-lookup"><span data-stu-id="11c0a-168">SQL store</span></span>
* <span data-ttu-id="11c0a-169">Redis 快取</span><span class="sxs-lookup"><span data-stu-id="11c0a-169">Redis cache</span></span>

<span data-ttu-id="11c0a-170">如需詳細資訊，請參閱[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。</span><span class="sxs-lookup"><span data-stu-id="11c0a-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="11c0a-171">將 ASP.NET Core 預覽版本部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="11c0a-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="11c0a-172">您可以使用下列方法，將 ASP.NET Core 預覽應用程式部署到 Azure App Service：</span><span class="sxs-lookup"><span data-stu-id="11c0a-172">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="11c0a-173">[安裝預覽網站延伸模組](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="11c0a-173">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="11c0a-174">將包含 Web 應用程式的 Docker 用於容器</span><span class="sxs-lookup"><span data-stu-id="11c0a-174">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="11c0a-175">如果您在使用預覽網站延伸模組時發生任何問題，請在 [GitHub](https://github.com/aspnet/azureintegration/issues/new) 上提出問題。</span><span class="sxs-lookup"><span data-stu-id="11c0a-175">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="11c0a-176">安裝預覽網站延伸模組</span><span class="sxs-lookup"><span data-stu-id="11c0a-176">Install the preview site extension</span></span>

1. <span data-ttu-id="11c0a-177">從 Azure 入口網站中，巡覽至 [App Service] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="11c0a-177">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="11c0a-178">選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11c0a-178">Select the web app.</span></span>
1. <span data-ttu-id="11c0a-179">在 [搜尋] 方塊中輸入"ex"，或向下捲動 [管理] 窗格清單至 [開發工具]。</span><span class="sxs-lookup"><span data-stu-id="11c0a-179">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="11c0a-180">選取 [開發工具] > [延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="11c0a-180">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="11c0a-181">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="11c0a-181">Select **Add**.</span></span>

   ![使用先前步驟的 [Azure App] 刀鋒視窗](index/_static/x1.png)

1. <span data-ttu-id="11c0a-183">選取 [ASP.NET Core 延伸模組]。</span><span class="sxs-lookup"><span data-stu-id="11c0a-183">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="11c0a-184">選取 [確定] 以接受法律條款。</span><span class="sxs-lookup"><span data-stu-id="11c0a-184">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="11c0a-185">選取 [確定] 安裝延伸模組。</span><span class="sxs-lookup"><span data-stu-id="11c0a-185">Select **OK** to install the extension.</span></span>

<span data-ttu-id="11c0a-186">新增作業完成後，即會安裝最新版的 .NET Core 預覽。</span><span class="sxs-lookup"><span data-stu-id="11c0a-186">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="11c0a-187">在主控台中執行 `dotnet --info` 以驗證安裝。</span><span class="sxs-lookup"><span data-stu-id="11c0a-187">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="11c0a-188">從 [App Service] 刀鋒視窗中：</span><span class="sxs-lookup"><span data-stu-id="11c0a-188">From the **App Service** blade:</span></span>

1. <span data-ttu-id="11c0a-189">在 [搜尋] 方塊中輸入"con"，或向下捲動 [管理] 窗格清單至 [開發工具]。</span><span class="sxs-lookup"><span data-stu-id="11c0a-189">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="11c0a-190">選取 [開發工具] > [主控台]。</span><span class="sxs-lookup"><span data-stu-id="11c0a-190">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="11c0a-191">在主控台中輸入 `dotnet --info`。</span><span class="sxs-lookup"><span data-stu-id="11c0a-191">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="11c0a-192">如果版本 `2.1.300-preview1-008174` 是最新的預覽版本，在命令提示字元執行 `dotnet --info` 取得下列輸出：</span><span class="sxs-lookup"><span data-stu-id="11c0a-192">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![使用先前步驟的 [Azure App] 刀鋒視窗](index/_static/cons.png)

<span data-ttu-id="11c0a-194">上圖中顯示的 ASP.NET Core 版本 `2.1.300-preview1-008174` 是範例。</span><span class="sxs-lookup"><span data-stu-id="11c0a-194">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="11c0a-195">設定網站延伸模組時最新的 ASP.NET Core 預覽版本於您執行 `dotnet --info` 時出現。</span><span class="sxs-lookup"><span data-stu-id="11c0a-195">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="11c0a-196">`dotnet --info` 會顯示已安裝 [預覽] 之網站延伸模組的路徑。</span><span class="sxs-lookup"><span data-stu-id="11c0a-196">The `dotnet --info` displays the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="11c0a-197">它會顯示應用程式是從網站擴充功能執行，而不是從預設的 ProgramFiles 位置執行。</span><span class="sxs-lookup"><span data-stu-id="11c0a-197">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="11c0a-198">如果您看到 ProgramFiles，請重新啟動網站，然後執行 `dotnet --info`。</span><span class="sxs-lookup"><span data-stu-id="11c0a-198">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="11c0a-199">**搭配使用預覽網站延伸模組與 ARM 範本**</span><span class="sxs-lookup"><span data-stu-id="11c0a-199">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="11c0a-200">如果您使用 ARM 範本來建立及部署應用程式，可以使用 `siteextensions` 資源類型將網站延伸模組新增至 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11c0a-200">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="11c0a-201">例如：</span><span class="sxs-lookup"><span data-stu-id="11c0a-201">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="11c0a-202">將包含 Web 應用程式的 Docker 用於容器</span><span class="sxs-lookup"><span data-stu-id="11c0a-202">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="11c0a-203">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) 包含最新的預覽 Docker 映像。</span><span class="sxs-lookup"><span data-stu-id="11c0a-203">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="11c0a-204">這些映像可用作為基底映像。</span><span class="sxs-lookup"><span data-stu-id="11c0a-204">The images can be used as a base image.</span></span> <span data-ttu-id="11c0a-205">請使用映像，並以一般的方式將其部署至容器的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11c0a-205">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11c0a-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="11c0a-206">Additional resources</span></span>

* [<span data-ttu-id="11c0a-207">Web Apps 概觀 (5 分鐘的概觀影片)</span><span class="sxs-lookup"><span data-stu-id="11c0a-207">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="11c0a-208">Azure App Service：裝載 .NET 應用程式的最佳平台 (55 分鐘的概觀影片)</span><span class="sxs-lookup"><span data-stu-id="11c0a-208">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="11c0a-209">Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)</span><span class="sxs-lookup"><span data-stu-id="11c0a-209">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="11c0a-210">Azure App Service 診斷概觀</span><span class="sxs-lookup"><span data-stu-id="11c0a-210">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="11c0a-211">Windows Server 上的 Azure App Service 使用 [Internet Information Services (IIS)](https://www.iis.net/)。</span><span class="sxs-lookup"><span data-stu-id="11c0a-211">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="11c0a-212">有關基礎 IIS 技術的主題如下：</span><span class="sxs-lookup"><span data-stu-id="11c0a-212">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="11c0a-213">Microsoft TechNet Library：Windows Server</span><span class="sxs-lookup"><span data-stu-id="11c0a-213">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
