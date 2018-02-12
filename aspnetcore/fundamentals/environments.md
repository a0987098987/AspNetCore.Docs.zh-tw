---
title: "在 ASP.NET Core 中使用多個環境"
author: rick-anderson
description: "了解 ASP.NET Core 如何跨多個環境支援應用程式的行為控制。"
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a>使用多個環境

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 可支援使用環境變數，以在執行階段設定應用程式的行為。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>環境

ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName) 中。 您可將 `ASPNETCORE_ENVIRONMENT` 設定為任何值，但架構支援下列[三個值](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)：[Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) 和 [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)。 如果未設定 `ASPNETCORE_ENVIRONMENT`，則會預設為 `Production`。

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

上述程式碼：

* 當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)。
* 當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：

    * `Staging`
    * `Production`
    * `Staging_2`

[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

注意：在 Windows 及 macOS 中，環境變數和值不區分大小寫。 Linux 的環境變數和值則預設為**區分大小寫**。

### <a name="development"></a>開發

您可以在開發環境中啟用生產環境不應該公開的功能。 例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)。

您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。 在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。

下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

以 `dotnet run` 啟動應用程式時，會使用含有 `"commandName": "Project"` 的第一個設定檔。 `commandName` 的值可指定要啟動的網頁伺服器。 `commandName` 可以是下列之一：

* IIS Express
* IIS
* 啟動 Kestrel 的專案

以 `dotnet run` 啟動應用程式時：

* 會讀取 *launchSettings.json* (如果有的話)。 *launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。
* 主控環境隨即顯示。


下列輸出顯示應用程式是以 `dotnet run` 啟動：
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio 的 [偵錯] 索引標籤提供的 GUI 可用來編輯 *launchSettings.json* 檔案：

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。 您必須重新啟動 Kestrel，它才會偵測到環境已有的變更。

>[!WARNING]
> *launchSettings.json* 不應儲存密碼。 [密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。

### <a name="production"></a>生產環境

若要將安全性、效能及應用程式的健全性最大化，您應該設定生產環境。 有些生產環境中的常見設定可能與開發環境不同，包括：

* 快取。
* 用戶端的資源會經過配套、縮減，且可能由 CDN 提供。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 啟用生產記錄與監視。 例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="setting-the-environment"></a>設定環境

一般來說，設定特定環境以進行測試，是很實用的方法。 如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。

環境的設定方法取決於作業系統而定。

### <a name="azure"></a>Azure

若是 Azure App Service：

* 選取 [應用程式設定] 刀鋒視窗。
* 將索引鍵和值新增至 [應用程式設定]。


### <a name="windows"></a>Windows
如果應用程式是使用 `dotnet run` 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：

**命令列**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

這些命令僅針對目前的視窗才會生效。 視窗關閉時，ASPNETCORE_ENVIRONMENT 設定會還原為預設值或電腦值。 若要全域設定 Windows 的值，請開啟 [控制台] > [系統] > [進階系統設定]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 的值。

![系統進階屬性](environments/_static/systemsetting_environment.png)

![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)


**web.config**

請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主題的＜設定環境變數＞一節。

**每個 IIS 應用程式集區**

若要為執行於隔離應用程式集區 (IIS 10.0+ 支援)　的個別應用程式設定環境變數，請參閱[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。

### <a name="macos"></a>macOS
您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境，

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
或在執行應用程式之前，使用 `export` 來設定。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。 使用任何文字編輯器編輯檔案，並新增下列陳述式。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
若是 Linux 散發版本，請在命令列使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。

### <a name="configuration-by-environment"></a>取決於環境的組態

如需詳細資訊，請參閱[取決於環境的組態](xref:fundamentals/configuration/index#configuration-by-environment)。

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>以環境為基礎的 Startup 類別和方法

ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。 如果 `Startup{EnvironmentName}` 類別存在，就會針對該 `EnvironmentName` 呼叫此類別：

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

注意：呼叫 [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 時會覆寫組態區段。

[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) 支援表單 `Configure{EnvironmentName}` 和 `Configure{EnvironmentName}Services` 的環境特定版本：

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>其他資源

* [應用程式啟動](xref:fundamentals/startup)
* [組態](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
