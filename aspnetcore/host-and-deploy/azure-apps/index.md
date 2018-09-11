---
title: 將 ASP.NET Core 裝載到 Azure App Service
author: guardrex
description: 探索如何在 Azure App Service 中裝載 ASP.NET Core 應用程式，及實用資源的連結。
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 23289823e154d93e4bedd23a1efae0e58c71eae0
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340182"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>將 ASP.NET Core 裝載到 Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。

## <a name="useful-resources"></a>實用資源

Azure [Web Apps 文件](/azure/app-service/)是 Azure 應用程式文件、教學課程、範例、使用說明指南及其他資源的首頁。 關於裝載 ASP.NET Core 應用程式，有兩個值得參考的教學課程：

[快速入門：在 Azure 中建立 ASP.NET Core Web 應用程式](/azure/app-service/app-service-web-get-started-dotnet)  
在 Windows 上使用 Visual Studio 建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。

[快速入門：在 Linux 上於 App Service 中建立 .NET Core Web 應用程式](/azure/app-service/containers/quickstart-dotnetcore)  
在 Linux 上使用命令列建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。

若要閱讀下列文章，請參閱 ASP.NET Core 文件：

[使用 Visual Studio 發佈至 Azure](xref:tutorials/publish-to-azure-webapp-using-vs)  
了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。

[使用 CLI 工具發佈至 Azure](xref:tutorials/publish-to-azure-webapp-using-cli)  
了解如何使用 Git 命令列用戶端將 ASP.NET Core 應用程式發佈到 Azure App Service。

[使用 Visual Studio 與 Git 持續部署到 Azure](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。

[使用 Azure Pipelines 建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)  
設定 ASP.NET Core 應用程式的 CI 組建，然後建立連續部署發行至 Azure App Service。

[Azure Web 應用程式沙箱](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
探索 Azure 應用程式平台強制實施的 Azure App Service 執行階段執行限制。

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>應用程式組態

在 ASP.NET 2.0 或更新版本中，以下 NuGet 套件會為部署至 Azure App Service 的應用程式提供自動記錄功能：

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) 提供 ASP.NET Core 與 Azure App Service 整合的啟動。 新增的記錄功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 套件提供。
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 執行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，以在 `Microsoft.Extensions.Logging.AzureAppServices` 套件中新增 Azure App Service 診斷記錄提供者。
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供記錄器實作以支援 Azure App Service 診斷記錄和記錄串流功能。

若以 .NET Core 為目標且參考 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)，則會已經包含套件。 較新的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中不存在套件。 若以 .NET Framework 為目標或參考 `Microsoft.AspNetCore.App` 中繼套件，則會參考個別記錄套件。

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a>使用 Azure 入口網站覆寫應用程式設定

[應用程式設定] 刀鋒視窗的 [應用程式設定] 區域允許您為應用程式設定環境變數。 環境變數可由[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)取用。

當應用程式使用 [Web 主機](xref:fundamentals/host/web-host)並使用會將主機設定為使用 `ASPNETCORE_` 前置詞的 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 環境變數來建置主機。 如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。

當應用程式使用[一般主機](xref:fundamentals/host/generic-host)時，環境變數預設不會被載入到應用程式的設定中，而且必須由開發人員新增設定提供者。 新增設定提供者時，開發人員必須判斷環境變數前置詞。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

用來設定轉送標頭中介軟體及 ASP.NET Core 模組的 IIS Integration 中介軟體會設定為轉送配置 (HTTP/HTTPS) 及發出要求的遠端 IP 位址。 其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="monitoring-and-logging"></a>監視與記錄

如需監視、記錄及疑難排解的資訊，請參閱下列文章：

[如何：監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)  
了解如何檢閱應用程式和 App Service 方案的配額和計量。

[為 Azure App Service 中的 Web 應用程式啟用診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log)  
探索如何啟用及存取 HTTP 狀態碼、失敗要求和網頁伺服器活動的診斷記錄。

[ASP.NET Core 中的錯誤處理簡介](xref:fundamentals/error-handling)  
了解處理 ASP.NET Core 應用程式錯誤的常見方法。

[針對 Azure App Service 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/azure-apps/troubleshoot)  
了解如何診斷使用 ASP.NET Core 應用程式部署 Azure App Service 的問題。

[Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)  
了解託管於 Azure App Service/IIS 之應用程式的常見部署組態錯誤，及疑難排解建議。

## <a name="data-protection-key-ring-and-deployment-slots"></a>資料保護金鑰環及部署位置

[資料保護金鑰](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存至 *%HOME%\ASP.NET\DataProtection-Keys* 資料夾。 此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。 金鑰待用時不受保護。 此資料夾對單一部署位置中應用程式的所有執行個體皆提供金鑰環。 各部署位置，例如預備和生產位置，不會共用金鑰環。

當在部署位置間交換時，使用資料保護的任何系統都將無法使用前一位置內的金鑰環，來解密儲存的資料。 ASP.NET Cookie 中介軟體使用資料保護來保護其 Cookie。 這會導致使用標準 ASP.NET Cookie 中介軟體的應用程式將使用者登出。 至於非相依於位置的金鑰環解決方案，請使用外部金鑰環提供者，例如：

* Azure Blob 儲存體
* Azure Key Vault
* SQL 存放區
* Redis 快取

如需詳細資訊，請參閱[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>將 ASP.NET Core 預覽版本部署至 Azure App Service

您可以使用下列方法，將 ASP.NET Core 預覽應用程式部署到 Azure App Service：

* [安裝預覽網站延伸模組](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [將包含 Web 應用程式的 Docker 用於容器](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a>安裝預覽網站延伸模組

如果您在使用預覽網站延伸模組時發生任何問題，請在 [GitHub](https://github.com/aspnet/azureintegration/issues/new) 上提出問題。

1. 從 Azure 入口網站中，巡覽至 [App Service] 刀鋒視窗。
1. 選取 Web 應用程式。
1. 在搜尋方塊中鍵入"ex"，或將管理區段的清單向下捲動至 [開發工具]。
1. 選取 [開發工具] > [延伸模組]。
1. 選取 [新增]。
1. 從清單選取 [ASP.NET Core &lt;x.y&gt; (x86) 執行階段] 延伸模組，其中 `<x.y>` 是 ASP.NET Core 預覽版本 (例如 **ASP.NET Core 2.2 (x86) 執行階段**)。 x86 執行階段適用於[架構相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，其依賴由 ASP.NET Core 模組裝載的跨處理序。
1. 選取 [確定] 以接受法律條款。
1. 選取 [確定] 安裝延伸模組。

當作業完成後，會安裝最新的 .NET Core 預覽。 確認安裝：

1. 選取 [開發工具] 下的 [進階工具]。
1. 選取 [進階工具] 刀鋒視窗上的 [開始]。
1. 選取 [偵錯主控台] > [PowerShell] 功能表項目。
1. 在 PowerShell 提示執行下列命令。 在命令中使用 ASP.NET Core 執行階段版本取代 `<x.y>`：

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   如果安裝的預覽執行階段適用於 ASP.NET Core 2.2，命令就會是：
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> 對於裝載於 A 系列計算或更高裝載層的應用程式，應用程式服務應用程式的平台架構 (x86 x64) 設定在 [一般設定] 下的 [應用程式設定] 刀鋒視窗中。 如果在同處理序模式中執行應用程式，且平台架構設定為適用於 64 位元 (x64)，ASP.NET Core 模組會使用 64 位元預覽執行階段 (如果有)。 安裝 **ASP.NET Core &lt;x.y&gt; (x64) 執行階段**延伸模組 (例如 **ASP.NET Core 2.2 (x64) 執行階段**)。
>
> 在安裝 x64 預覽執行階段後，請在 Kudu PowerShell 命令視窗中執行下列命令，以確認安裝。 在命令中使用 ASP.NET Core 執行階段版本取代 `<x.y>`：
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> 如果安裝的預覽執行階段適用於 ASP.NET Core 2.2，命令就會是：
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> 當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。

::: moniker-end

> [!NOTE]
> **ASP.NET Core 延伸模組**可在 Azure 應用程式服務上提供適用於 ASP.NET Core 的其他功能，例如：提供 Azure 記錄。 若從 Visual Studio 部署，會自動安裝延伸模組。 若未安裝延伸模組，請為應用程式安裝。

**搭配使用預覽網站延伸模組與 ARM 範本**

如果您使用 ARM 範本來建立及部署應用程式，可以使用 `siteextensions` 資源類型將網站延伸模組新增至 Web 應用程式。 例如: 

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>將包含 Web 應用程式的 Docker 用於容器

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) 包含最新的預覽 Docker 映像。 這些映像可用作為基底映像。 請使用映像，並以一般的方式將其部署至容器的 Web 應用程式。

## <a name="additional-resources"></a>其他資源

* [Web Apps 概觀 (5 分鐘的概觀影片)](/azure/app-service/app-service-web-overview)
* [Azure App Service：裝載 .NET 應用程式的最佳平台 (55 分鐘的概觀影片)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Windows Server 上的 Azure App Service 使用 [Internet Information Services (IIS)](https://www.iis.net/)。 有關基礎 IIS 技術的主題如下：

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Microsoft TechNet Library：Windows Server](/windows-server/windows-server-versions)
