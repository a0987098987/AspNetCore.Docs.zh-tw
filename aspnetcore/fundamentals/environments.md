---
title: "使用多個環境中 ASP.NET Core"
author: rick-anderson
description: "了解 ASP.NET Core 應用程式行為控制跨多個環境所提供的支援。"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a>使用多個環境

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 提供支援使用環境變數在執行階段設定應用程式行為。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>環境

ASP.NET Core 讀取環境變數`ASPNETCORE_ENVIRONMENT`應用程式啟動和存放區中的值在[IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)。 `ASPNETCORE_ENVIRONMENT`可以設定為任何值，但[三個值](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)framework 所支援：[開發](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)，[臨時](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)，和[生產](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)。 如果`ASPNETCORE_ENVIRONMENT`未設定，它會預設為`Production`。

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

上述程式碼：

* 呼叫[UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)時`ASPNETCORE_ENVIRONMENT`設`Development`。
* 呼叫[UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)時的值`ASPNETCORE_ENVIRONMENT`設定下列其中之一：

    * `Staging`
    * `Production`
    * `Staging_2`

[環境標記協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用值`IHostingEnvironment.EnvironmentName`来包含或排除的項目中的標記：

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

注意： 在 Windows 及 macOS，環境變數和值不區分大小寫。 Linux 環境變數和值都是**區分大小寫**預設。

### <a name="development"></a>開發

在開發環境可啟用不應該在生產環境中公開的功能。 例如，ASP.NET Core 範本啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)開發環境中。

適用於本機開發環境可以設定*Properties\launchSettings.json*專案檔案。 設定環境值*launchSettings.json*覆寫系統環境中設定的值。

下列 XML 顯示三個設定檔從*launchSettings.json*檔案：

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

當應用程式會使用啟動`dotnet run`，第一個設定檔與`"commandName": "Project"`將使用。 值`commandName`指定要啟動的 web 伺服器。 `commandName`可以是下列之一：

* IIS Express
* IIS
* （這樣會啟動 Kestrel） 專案

當應用程式會使用啟動`dotnet run`:

* *launchSettings.json*讀取如果有的話。 `environmentVariables`中的設定*launchSettings.json*覆寫環境變數。
* 裝載環境隨即顯示。


下列的輸出會顯示應用程式入門`dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio**偵錯** 索引標籤提供的 GUI，若要編輯*launchSettings.json*檔案：

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

專案設定檔所做的變更可能不會生效，直到重新啟動 web 伺服器。 Kestrel 必須重新啟動它就會偵測到它的環境所做的變更。

>[!WARNING]
> *launchSettings.json*不應儲存機密資料。 [密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。

### <a name="production"></a>生產環境

在實際執行環境應該設定為最大化安全性、 效能及應用程式的健全性。 某些常見設定生產環境中可能會不同於開發包括：

* 快取。
* 用戶端的資源配套、 縮短，以及可能從 CDN 服務。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 實際執行記錄，且啟用監視。 例如， [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/)。

## <a name="setting-the-environment"></a>設定環境

通常會很有用來設定特定的環境進行測試。 如果您的環境未設定，它會預設為`Production`表示停用大部分的偵錯功能。

設定環境的方法取決於作業系統。

### <a name="azure"></a>Azure

Azure 應用程式服務：

* 選取**應用程式設定**刀鋒視窗。
* 將索引鍵和值**應用程式設定**。


### <a name="windows"></a>Windows
若要設定`ASPNETCORE_ENVIRONMENT`目前工作階段，如果應用程式會使用啟動`dotnet run`，使用下列命令

**命令列**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

這些命令才會生效，僅針對目前的視窗。 視窗關閉時，ASPNETCORE_ENVIRONMENT 設定會還原為預設值或 machine 值中。 若要開啟 Windows 全域設定的值**控制台** > **系統** > **進階系統設定**以及新增或編輯`ASPNETCORE_ENVIRONMENT`值。

![進階屬性的系統](environments/_static/systemsetting_environment.png)

![ASPNET 核心環境變數](environments/_static/windows_aspnetcore_environment.png)


**web.config**

請參閱*設定環境變數*區段[ASP.NET 核心模組的組態參考](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)主題。

**每個 IIS 應用程式集區**

若要設定個別執行的應用程式 （支援 IIS 10.0 +） 隔離的應用程式集區中的環境變數，請參閱*AppCmd.exe 命令*區段[環境變數\<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)主題。

### <a name="macos"></a>macOS
設定 macOS 的目前環境可以是內建作業時完成執行應用程式。

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
或使用`export`設定之前執行應用程式。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
電腦層級環境變數中設定*.bashrc*或*.bash_profile*檔案。 編輯檔案使用任何文字編輯器，並加入下列陳述式。

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
針對 Linux 散發版本，請使用`export`工作階段以變數設定為基礎的命令在命令列和*bash_profile*機器層級的環境設定的檔案。

### <a name="configuration-by-environment"></a>取決於環境的組態

請參閱[環境組態](xref:fundamentals/configuration/index#configuration-by-environment)如需詳細資訊。

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>環境基礎啟動類別和方法

ASP.NET Core 應用程式啟動時，[啟動類別](xref:fundamentals/startup)bootstraps 應用程式。 如果類別`Startup{EnvironmentName}`存在，將該呼叫類別`EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

注意： 呼叫[WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)覆寫組態區段。

[設定](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_)和[ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0)支援環境特定版本的表單`Configure{EnvironmentName}`和`Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>其他資源

* [應用程式啟動](xref:fundamentals/startup)
* [組態](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
