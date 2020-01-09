---
title: 將 ASP.NET Core 應用程式部署至 Azure App Service
author: bradygaster
description: 本文包含 Azure 主機和部署資源的連結。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/16/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 51d82d1deadb3d2adbdccd39c8d949e3f9f812fd
ms.sourcegitcommit: 79850db9e79b1705b89f466c6f2c961ff15485de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2020
ms.locfileid: "75693839"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>將 ASP.NET Core 應用程式部署至 Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。

## <a name="useful-resources"></a>實用資源

[App Service 文件](/azure/app-service/)是 Azure 應用程式文件、教學課程、範例、使用說明指南與其他資源的首頁。 關於裝載 ASP.NET Core 應用程式，有兩個值得參考的教學課程：

[在 Azure 中建立 ASP.NET Core Web 應用程式](/azure/app-service/app-service-web-get-started-dotnet)  
在 Windows 上使用 Visual Studio 建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。

[在 Linux 上的 App Service 中建立 ASP.NET Core 應用程式](/azure/app-service/containers/quickstart-dotnetcore)  
在 Linux 上使用命令列建立 ASP.NET Core Web 應用程式並將其部署到 Azure App Service。

如需 Azure App 服務上可用的 ASP.NET Core 版本，請參閱[App Service 儀表板上的 ASP.NET Core](https://aspnetcoreon.azurewebsites.net/) 。

訂閱[App Service 公告](https://github.com/Azure/app-service-announcements/)存放庫並監視問題。 App Service 小組會定期張貼傳入 App Service 的公告和案例。

若要閱讀下列文章，請參閱 ASP.NET Core 文件：

<xref:tutorials/publish-to-azure-webapp-using-vs>  
了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。

[建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)  
設定 ASP.NET Core 應用程式的 CI 組建，然後建立連續部署發行至 Azure App Service。

[Azure Web 應用程式沙箱](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
探索 Azure 應用程式平台強制實施的 Azure App Service 執行階段執行限制。

<xref:test/troubleshoot>  
了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。

## <a name="application-configuration"></a>應用程式組態

### <a name="platform"></a>Platform

應用程式服務應用程式的平臺架構（x86/x64）會在 Azure 入口網站的應用程式設定中，針對裝載于 A 系列計算（基本）或更高主機層上的應用程式進行設定。 確認應用程式的發佈設定（例如，在 Visual Studio[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)）中，是否符合應用程式在 Azure 入口網站中的服務設定。

::: moniker range=">= aspnetcore-2.2"

Azure App Service 具有 64 位元 (x64) 及 32 位元 (x86) 應用程式的執行階段。 App Service 提供的 [.NET Core SDK](/dotnet/core/sdk) 為 32 位元，但您可以使用 [Kudu](https://github.com/projectkudu/kudu/wiki) 主控台或 Visual Studio 中的發佈處理序，部署在本機建置的 64 位元應用程式。 如需詳細資訊，請參閱[發佈與部署應用程式](#publish-and-deploy-the-app)一節。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

對於具有原生相依性的應用程式而言，Azure App Service 具有 32 位元 (x86) 應用程式的執行階段。 App Service 可使用的 [.NET Core SDK](/dotnet/core/sdk) 為 32 位元。

::: moniker-end

如需 .NET Core framework 元件和散發方法的詳細資訊，例如 .NET Core 執行時間和 .NET Core SDK 的相關資訊，請參閱[關於 .Net core：組合](/dotnet/core/about#composition)。

### <a name="packages"></a>package

包含下列 NuGet 套件，為部署至 Azure App Service 的應用程式提供自動記錄功能：

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) 使用 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) 提供 ASP.NET Core 與 Azure App Service 整合的啟動。 新增的記錄功能由 `Microsoft.AspNetCore.AzureAppServicesIntegration` 套件提供。
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) 執行 [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)，以在 `Microsoft.Extensions.Logging.AzureAppServices` 套件中新增 Azure App Service 診斷記錄提供者。
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) 提供記錄器實作以支援 Azure App Service 診斷記錄和記錄串流功能。

上述套件無法從 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)取得。 以 .NET Framework 為目標或參考 `Microsoft.AspNetCore.App` 中繼套件的應用程式必須明確參考應用程式之專案檔中的個別套件。

## <a name="override-app-configuration-using-the-azure-portal"></a>使用 Azure 入口網站覆寫應用程式設定

Azure 入口網站中的應用程式設定允許您為應用程式設定環境變數。 環境變數可由[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)取用。

在 Azure 入口網站中建立或修改應用程式設定並選取 [儲存] 按鈕後，即會重新啟動 Azure 應用程式。 當服務重新啟動之後，環境變數便可供應用程式使用。

::: moniker range=">= aspnetcore-3.0"

當應用程式使用[泛型主機](xref:fundamentals/host/generic-host)時，會在呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> 來建立主機時，將環境變數載入應用程式的設定中。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

當應用程式使用[Web 主機](xref:fundamentals/host/web-host)時，系統會在呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 來建立主機時，將環境變數載入應用程式的設定中。 如需詳細資訊，請參閱 <xref:fundamentals/host/web-host> 與[環境變數設定提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)。

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) (當裝載[處理序外](xref:host-and-deploy/iis/index#out-of-process-hosting-model)時會設定轉送標頭中介軟體) 與 ASP.NET Core 模組會設定為轉送配置 (HTTP/HTTPS) 與發出要求的遠端 IP 位址。 其他 Proxy 伺服器和負載平衡器後方託管的應用程式可能需要其他設定。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="monitoring-and-logging"></a>監視與記錄

::: moniker range=">= aspnetcore-3.0"

部署到 App Service 的 ASP.NET Core 應用程式會自動接收 App Service 延伸模組：**ASP.NET Core 記錄整合**。 延伸模組讓 Azure App Service 上的 ASP.NET Core 應用程式得以進行記錄整合。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

部署到 App Service 的 ASP.NET Core 應用程式會自動接收 App Service 延伸模組 **ASP.NET Core 記錄延伸模組**。 延伸模組讓 Azure App Service 上的 ASP.NET Core 應用程式得以進行記錄整合。

::: moniker-end

如需監視、記錄及疑難排解的資訊，請參閱下列文章：

[監視 Azure App Service 中的應用程式](/azure/app-service/web-sites-monitor)  
了解如何檢閱應用程式和 App Service 方案的配額和計量。

[為 Azure App Service 中的應用程式啟用診斷記錄](/azure/app-service/web-sites-enable-diagnostic-log)  
探索如何啟用及存取 HTTP 狀態碼、失敗要求和網頁伺服器活動的診斷記錄。

<xref:fundamentals/error-handling>  
了解處理 ASP.NET Core 應用程式錯誤的常見方法。

<xref:test/troubleshoot-azure-iis>  
了解如何診斷使用 ASP.NET Core 應用程式部署 Azure App Service 的問題。

<xref:host-and-deploy/azure-iis-errors-reference>  
了解託管於 Azure App Service/IIS 之應用程式的常見部署組態錯誤，及疑難排解建議。

## <a name="data-protection-key-ring-and-deployment-slots"></a>資料保護金鑰環及部署位置

[資料保護金鑰](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)保存至 *%HOME%\ASP.NET\DataProtection-Keys* 資料夾。 此資料夾使用網路儲存體進行保存，並會在裝載應用程式的所有電腦上同步。 金鑰待用時不受保護。 此資料夾對單一部署位置中應用程式的所有執行個體皆提供金鑰環。 各部署位置，例如預備和生產位置，不會共用金鑰環。

當在部署位置間交換時，使用資料保護的任何系統都將無法使用前一位置內的金鑰環，來解密儲存的資料。 ASP.NET Cookie 中介軟體使用資料保護來保護其 Cookie。 這會導致使用標準 ASP.NET Cookie 中介軟體的應用程式將使用者登出。 至於非相依於位置的金鑰環解決方案，請使用外部金鑰環提供者，例如：

* Azure Blob 儲存體
* Azure Key Vault
* SQL 存放區
* Redis 快取

如需詳細資訊，請參閱<xref:security/data-protection/implementation/key-storage-providers>。
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>

## <a name="deploy-an-aspnet-core-app-that-uses-a-net-core-preview"></a>部署使用 .NET Core 預覽的 ASP.NET Core 應用程式

若要部署使用 .NET Core 預覽版本的應用程式，請參閱下列資源。 當執行時間可用但 SDK 尚未安裝在 Azure App Service 上時，也會使用這些方法。

* [使用 Azure Pipelines 指定 .NET Core SDK 版本](#specify-the-net-core-sdk-version-using-azure-pipelines)
* [部署獨立的預覽應用程式](#deploy-a-self-contained-preview-app)
* [將包含 Web 應用程式的 Docker 用於容器](#use-docker-with-web-apps-for-containers)
* [安裝預覽網站延伸模組](#install-the-preview-site-extension)

如需 Azure App 服務上可用的 ASP.NET Core 版本，請參閱[App Service 儀表板上的 ASP.NET Core](https://aspnetcoreon.azurewebsites.net/) 。

### <a name="specify-the-net-core-sdk-version-using-azure-pipelines"></a>使用 Azure Pipelines 指定 .NET Core SDK 版本

使用[AZURE APP SERVICE CI/CD 案例](/azure/app-service/deploy-continuous-deployment)來設定具有 Azure DevOps 的持續整合組建。 建立 Azure DevOps 組建之後，選擇性地將組建設定為使用特定的 SDK 版本。 

#### <a name="specify-the-net-core-sdk-version"></a>指定 .NET Core SDK 版本

使用 App Service 部署中心建立 Azure DevOps 組建時，預設的組建管線會包含 `Restore`、`Build`、`Test`和 `Publish`的步驟。 若要指定 SDK 版本，請選取 [代理程式作業] 清單中的 [新增] **（+）** 按鈕，以新增新的步驟。 在搜尋列中搜尋 **.NET Core SDK** 。 

![新增 .NET Core SDK 步驟](index/add-sdk-step.png)

將步驟移至組建中的第一個位置，使其後面的步驟使用指定的 .NET Core SDK 版本。 指定 .NET Core SDK 的版本。 在此範例中，SDK 設定為 `3.0.100`。

![完成的 SDK 步驟](index/sdk-step-first-place.png)

若要發佈[獨立部署（SCD）](/dotnet/core/deploying/#self-contained-deployments-scd)，請在 `Publish` 步驟中設定 SCD，並提供[執行時間識別碼（RID）](/dotnet/core/rid-catalog)。

![獨立發行](index/self-contained.png)

### <a name="deploy-a-self-contained-preview-app"></a>部署獨立式預覽應用程式

以預覽執行階段為目標的[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 會在部署中包含預覽執行階段。

部署獨立式應用程式時：

* Azure App Service 中的網站不需要[預覽網站延伸模組](#install-the-preview-site-extension)。
* 應用程式發佈必須遵循不同於針對 [Framework 相依部署 (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd) 發佈時使用的方法。

請遵循[部署獨立式應用程式](#deploy-the-app-self-contained)一節。

### <a name="use-docker-with-web-apps-for-containers"></a>將包含 Web 應用程式的 Docker 用於容器

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) 包含最新的預覽 Docker 映像。 這些映像可用作為基底映像。 請使用映像，並以一般的方式將其部署至容器的 Web 應用程式。

### <a name="install-the-preview-site-extension"></a>安裝預覽網站延伸模組

如果您在使用預覽網站延伸模組時發生任何問題，請建立 [aspnet/AspNetCore 問題](https://github.com/aspnet/AspNetCore/issues)。

1. 從 Azure 入口網站瀏覽至 App Service。
1. 選取 Web 應用程式。
1. 在搜尋方塊中鍵入 "ex" 來篩選 "Extensions"，也可往下捲動管理工具的清單。
1. 選取 [擴充功能]。
1. 選取 [新增]。
1. 從清單選取 [ASP.NET Core {X.Y} ({x64|x86}) 執行階段] 延伸模組，其中 `{X.Y}` 是 ASP.NET Core 預覽版本，而 `{x64|x86}` 則指定平台。
1. 選取 [確定] 以接受法律條款。
1. 選取 [確定] 安裝延伸模組。

當作業完成後，會安裝最新的 .NET Core 預覽。 確認安裝：

1. 選取 [進階工具]。
1. 在 [進階工具] 中選取 [移至]。
1. 選取 [偵錯主控台] > [PowerShell] 功能表項目。
1. 在 PowerShell 提示執行下列命令。 在命令中使用 ASP.NET Core 執行階段版本取代 `{X.Y}`，並以平台取代 `{PLATFORM}`：

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。

> [!NOTE]
> 應用程式服務應用程式的平臺架構（x86/x64）會在 Azure 入口網站的應用程式設定中，針對裝載于 A 系列計算（基本）或更高主機層上的應用程式進行設定。 確認應用程式的發佈設定（例如，在 Visual Studio[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)）是否符合 Azure 入口網站中應用程式的服務設定中的設定。
>
> 如果在同處理序模式中執行應用程式，且平台架構設定為適用於 64 位元 (x64)，ASP.NET Core 模組會使用 64 位元預覽執行階段 (如果有)。 使用 Azure 入口網站安裝**ASP.NET Core {X. Y} （x64）運行**時間擴充功能。
>
> 安裝 x64 preview 執行時間之後，請在 Azure Kudu PowerShell 命令視窗中執行下列命令，以確認安裝。 在下列命令中，以 `{X.Y}` 的 ASP.NET Core 執行階段版本取代：
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> 當已安裝 x64 預覽執行階段時，此命令會傳回 `True`。

> [!NOTE]
> **ASP.NET Core 延伸模組**可在 Azure 應用程式服務上提供適用於 ASP.NET Core 的其他功能，例如：提供 Azure 記錄。 若從 Visual Studio 部署，會自動安裝延伸模組。 若未安裝延伸模組，請為應用程式安裝。

**搭配使用預覽網站延伸模組與 ARM 範本**

如果您使用 ARM 範本來建立及部署應用程式，可以使用 `siteextensions` 資源類型將網站延伸模組新增至 Web 應用程式。 例如：

[!code-json[](index/sample/arm.json?highlight=2)]

## <a name="publish-and-deploy-the-app"></a>發佈及部署應用程式

::: moniker range=">= aspnetcore-2.2"

若為64位部署：

* 請使用 64 位元 .NET Core SDK 來建置 64 位元應用程式。
* 在 App Service 的 [組態] > [一般設定] 中，將 [平台] 設為 [64 位元]。 應用程式必須使用基本或更高的服務方案，才能選擇平台位元。

::: moniker-end

### <a name="deploy-the-app-framework-dependent"></a>部署依架構不同的應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從 Visual Studio 工具列選取 [建置] > [發佈 {應用程式名稱}]，或在 [方案總管] 中以滑鼠右鍵按一下專案，然後選取 [發佈]。
1. 在 [挑選發佈目標] 對話方塊中，確認已選取 [App Service]。
1. 選取 [進階]。 [發佈] 對話方塊隨即開啟。
1. 在 [發行] 對話方塊中：
   * 確認已選取 [發行] 設定。
   * 開啟 [部署模式] 下拉式清單，然後選取 [依架構不同]。
   * 將 [目標執行階段] 選取為 [可攜式]。
   * 如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項] 並選取核取方塊，以移除目的地的其他檔案。
   * 選取 [儲存]。
1. 遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 在專案檔中，請勿指定[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。

1. 從命令殼層使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，以發行組態來發佈應用程式。 在下列範例中，應用程式會發佈為依架構不同的應用程式：

   ```console
   dotnet publish --configuration Release
   ```

1. 將 bin/Release/{目標 FRAMEWORK}/publish 目錄的內容移到 App Service 中的網站。 若要在 [Kudu](https://github.com/projectkudu/kudu/wiki) 主控台將 *publish* 資料夾內容從本機硬碟或網路共用直接拖曳到 App Service，請將檔案拖曳到 Kudu 主控台中的 `D:\home\site\wwwroot` 資料夾。

---

### <a name="deploy-the-app-self-contained"></a>部署獨立式應用程式

若是[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)，請使用 Visual Studio 或命令列介面 (CLI) 工具。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從 Visual Studio 工具列選取 [建置] > [發佈 {應用程式名稱}]，或在 [方案總管] 中以滑鼠右鍵按一下專案，然後選取 [發佈]。
1. 在 [挑選發佈目標] 對話方塊中，確認已選取 [App Service]。
1. 選取 [進階]。 [發佈] 對話方塊隨即開啟。
1. 在 [發行] 對話方塊中：
   * 確認已選取 [發行] 設定。
   * 開啟 [部署模式] 下拉式清單，然後選取 [獨立式]。
   * 從 [目標執行階段] 下拉式清單中選取目標執行階段。 預設為 `win-x86`。
   * 如果您需要在部署時移除其他檔案，請開啟 [檔案發佈選項] 並選取核取方塊，以移除目的地的其他檔案。
   * 選取 [儲存]。
1. 遵循 [發佈精靈] 的其餘提示來建立新網站，或更新現有網站。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 在專案檔中，指定一或多個[執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。 針對單一 RID 使用 `<RuntimeIdentifier>` (單數)，或者使用 `<RuntimeIdentifiers>` (複數) 來提供以分號分隔的 RID 清單。 在下列範例中已指定 `win-x86`：

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. 從命令殼層中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，針對主機執行階段以 [發行] 設定來發佈應用程式。 在下列範例中，將針對 `win-x86` RID發佈應用程式。 提供給 `--runtime` 選項的 RID 必須在專案檔的 `<RuntimeIdentifier>` (或 `<RuntimeIdentifiers>`) 屬性中提供。

   ```console
   dotnet publish --configuration Release --runtime win-x86 --self-contained
   ```

1. 將 bin/Release/{目標 FRAMEWORK}/{執行階段識別碼}/publish 目錄的內容移至 App Service 中的網站。 若要在 Kudu 主控台將 *publish* 資料夾內容從本機硬碟或網路共用直接拖曳到 App Service，請將檔案拖曳到 Kudu 主控台中的 `D:\home\site\wwwroot` 資料夾。

---

## <a name="protocol-settings-https"></a>通訊協定設定 (HTTPS)

安全通訊協定繫結可讓您指定透過 HTTPS 回應要求時要使用的憑證。 繫結需要針對特定主機名稱簽發的有效私密憑證 ( *.pfx*)。 如需詳細資訊，請參閱[教學課程：將現有的自訂 SSL 憑證系結至 Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl)。

## <a name="transform-webconfig"></a>轉換 web.config

如需在發佈時轉換 *web.config* (例如依據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。

## <a name="additional-resources"></a>其他資源

* [App Service 概觀](/azure/app-service/app-service-web-overview)
* [Azure App Service：裝載 .NET 應用程式的最佳平台 (55 分鐘的概觀影片)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday：Azure App Service 診斷和疑難排解體驗 (12 分鐘的影片)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Azure App Service 診斷概觀](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Windows Server 上的 Azure App Service 使用 [Internet Information Services (IIS)](https://www.iis.net/)。 有關基礎 IIS 技術的主題如下：

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Windows Server - 適用於目前版本與先前版本的 IT 系統管理員內容](/windows-server/windows-server-versions)
