---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: de3c3fd5a2f0e49366d9d5b4e992d0247bcab0e5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577518"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>在 ASP.NET Core 中使用多個環境

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>環境

ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) 中。 您可將 `ASPNETCORE_ENVIRONMENT` 設定為任何值，但架構支援下列[三個值](/dotnet/api/microsoft.aspnetcore.hosting.environmentname)：[Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development)、[Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) 和 [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production)。 如果未設定 `ASPNETCORE_ENVIRONMENT`，則預設為 `Production`。

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

上述程式碼：

* 當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。
* 當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：

    * `Staging`
    * `Production`
    * `Staging_2`

[環境標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)會使用 `IHostingEnvironment.EnvironmentName` 的值來包含或排除項目中的標籤：

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

在 Windows 及 macOS 中，環境變數和值不區分大小寫。 Linux 的環境變數和值則預設為**區分大小寫**。

### <a name="development"></a>開發

您可以在開發環境中啟用生產環境不應該公開的功能。 例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)。

您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。 在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。

下列 JSON 顯示來自 *launchSettings.json* 檔案的三個設定檔：

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> *launchSettings.json* 中的 `applicationUrl` 屬性可以指定伺服器 URL 的清單。 請在清單的 URL 之間使用分號：
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。 `commandName` 的值可指定要啟動的網頁伺服器。 `commandName` 可以是下列任何一個項目：

* IIS Express
* IIS
* 啟動 Kestrel 的專案

以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時：

* 會讀取 *launchSettings.json* (如果有的話)。 *launchSettings.json* 中的 `environmentVariables` 設定會覆寫環境變數。
* 主控環境隨即顯示。

下列輸出顯示應用程式是以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動：

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio 專案屬性的 [偵錯] 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。 您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。

> [!WARNING]
> *launchSettings.json* 不應儲存密碼。 [密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。

使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。 下列範例會將環境設定為 `Development`：

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

使用 `dotnet run` 啟動應用程式時，不會如同 *Properties/launchSettings.json* 一樣讀取專案中的 *.vscode/launch.json* 檔。 開發時若啟動的應用程式沒有 *launchSettings.json* 檔，請使用環境變數設定環境，或是使用 `dotnet run` 命令的命令列引數。

### <a name="production"></a>生產環境

若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。 不同於開發的某些一般設定包括：

* 快取。
* 用戶端的資源會經過配套、縮減，且可能由 CDN 提供。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 啟用生產記錄與監視。 例如 [Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="set-the-environment"></a>設定環境

一般來說，設定特定環境以進行測試，是很實用的方法。 如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。 環境的設定方法取決於作業系統而定。

### <a name="azure-app-service"></a>Azure App Service

若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：

1. 從 [應用程式服務] 刀鋒視窗選取應用程式。
1. 在 [設定] 群組中，選取 [應用程式設定] 刀鋒視窗。
1. 在 [應用程式設定] 區域中，選取 [新增設定]。
1. 針對 [輸入名稱]，提供 `ASPNETCORE_ENVIRONMENT`。 針對 [輸入值]，提供環境 (例如 `Staging`)。
1. 如果您想要在交換部署位置時，環境設定保留目前的位置，請選取 [位置設定] 核取方塊。 如需詳細資訊，請參閱 [Azure 文件：交換哪些設定？](/azure/app-service/web-sites-staged-publishing)。
1. 選取刀鋒視窗頂端的 [儲存]。

Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。

### <a name="windows"></a>Windows

如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：

**命令提示字元**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

這些命令僅針對目前的視窗才會生效。 視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。

若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：

* 開啟 [控制台] > [系統] > [進階系統設定]，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* 開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：

  **命令提示字元**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  `/M` 參數表示將環境變數設定在系統層級。 若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  `Machine` 選項值表示將環境變數設定在系統層級。 若選項值變更為 `User`，則將環境變數設定為使用者帳戶。

當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。

**web.config**

若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞一節。 當使用 *web.config*設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，其值會覆寫系統層級的設定。

**每個 IIS 應用程式集區**

若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞一節。 當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。

> [!IMPORTANT]
> 當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：
>
> * 從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。
> * 重新啟動伺服器。

### <a name="macos"></a>macOS

您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

或者，在執行應用程式之前，使用 `export` 設定環境：

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。 使用任何文字編輯器編輯檔案。 新增下列陳述式：

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。

### <a name="configuration-by-environment"></a>取決於環境的組態

若要依環境載入組態，建議使用：

* *appsettings* 檔案 (*appsettings.&lt;<Environment>&gt;.json)。 請參閱[組態：檔案組態提供者](xref:fundamentals/configuration/index#file-configuration-provider)。
* 環境變數 (在裝載應用程式的每部系統上設定)。 請參閱[組態：檔案組態提供者](xref:fundamentals/configuration/index#file-configuration-provider)和[在開發期間安全儲存應用程式祕密：環境變數](xref:security/app-secrets#environment-variables)。
* 祕密管理員 (僅限開發環境)。 請參閱 <xref:security/app-secrets>。

## <a name="environment-based-startup-class-and-methods"></a>以環境為基礎的 Startup 類別和方法

### <a name="startup-class-conventions"></a>Startup 類別慣例

ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。 應用程式可以針對不同的環境定義個別的 `Startup` 類別 (例如 `StartupDevelopment`)，並在執行階段選取適當的 `Startup` 類別。 將優先使用其名稱尾碼符合目前環境的類別。 如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。

若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：

::: moniker range=">= aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a>Startup 方法慣例

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) 支援表單 `Configure<EnvironmentName>` 和 `Configure<EnvironmentName>Services` 的環境特定版本：

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
