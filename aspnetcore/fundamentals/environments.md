---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: b0218b2c77c283c0849dca9491046534b88c5a77
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78656216"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>在 ASP.NET Core 中使用多個環境

::: moniker range=">= aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample)([如何下載](xref:index#how-to-download-a-sample))

## <a name="environments"></a>環境

ASP.NET Core`ASPNETCORE_ENVIRONMENT`在應用 啟動時讀取環境變數,並將該值存儲在[IWebHost 環境.環境名稱](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)中。 `ASPNETCORE_ENVIRONMENT`可以設置為任何值,但框架提供了三個值:

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <xref:Microsoft.Extensions.Hosting.Environments.Production> (預設值)

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

上述程式碼：

* 當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。
* 當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：

  * `Staging`
  * `Production`
  * `Staging_2`

[環境標記説明程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)使用`IHostingEnvironment.EnvironmentName`的值 在 元素中包括或排除標記:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

在 Windows 及 macOS 中，環境變數和值不區分大小寫。 Linux 的環境變數和值則預設為**區分大小寫**。

### <a name="development"></a>部署

您可以在開發環境中啟用生產環境不應該公開的功能。 例如,ASP.NET核心範本在開發環境中啟用[開發人員異常頁](xref:fundamentals/error-handling#developer-exception-page)。

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

以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。 `commandName` 的值可指定要啟動的網頁伺服器。 `commandName` 可以是下列任何一個項目：

* `IISExpress`
* `IIS`
* `Project` (這會啟動 Kestrel)

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

Visual Studio 專案屬性的 [偵錯]**** 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：

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

### <a name="production"></a>Production

若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。 不同於開發的某些一般設定包括：

* 快取。
* 用戶端的資源會經過配套、縮減，且可能由 CDN 提供。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 啟用生產記錄與監視。 例如,[應用程式見解](/azure/application-insights/app-insights-asp-net-core)。

## <a name="set-the-environment"></a>設定環境

使用環境變數或平臺設置設置用於測試的特定環境通常很有用。 如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。 環境的設定方法取決於作業系統而定。

生成主機時,應用讀取的最後一個環境設置將確定應用的環境。 應用在運行時無法更改應用的環境。

### <a name="environment-variable-or-platform-setting"></a>環境變數或平台設定

#### <a name="azure-app-service"></a>Azure App Service

若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：

1. 從 [應用程式服務]**** 刀鋒視窗選取應用程式。
1. 在 **「設定」** 群組中,選擇 **「設定」** 邊欄選項卡。
1. 在應用程式**設定「** 選擇」 中, 選擇 **「新增應用程式」 設定**。
1. 在 **'新增/編輯應用程式' 設定**視窗`ASPNETCORE_ENVIRONMENT`中, 提供**名稱**。 對於**值**,提供環境(例如`Staging`, 。
1. 如果您希望環境設置在交換部署槽時保留與當前插槽一起,請選擇 **「部署槽設置**」複選框。 有關詳細資訊,請參閱在[Azure 文件中設定 Azure 應用服務中的暫存環境](/azure/app-service/web-sites-staged-publishing)。
1. 選擇 **「確定」** 以關閉 **「新增/編輯應用程式」設定**視窗。
1. 選擇 **「在****設定**」邊欄選項卡頂部保存。

Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。

#### <a name="windows"></a>Windows

如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：

**指令提示符**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

這些命令僅針對目前的視窗才會生效。 視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。

若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：

* 開啟 [控制台]**[系統]** > **[進階系統設定]** > ****，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* 開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：

  **指令提示符**

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

若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞** 一節。

**專案檔或發行設定檔**

**對於 Windows IIS 部署:** 在`<EnvironmentName>`[發佈設定檔 (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中包括該屬性。 此方法會在專案發行時於 *web.config* 中設定環境：

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

**每個 IIS 應用程式集區**

若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞** 一節。 當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。

> [!IMPORTANT]
> 當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：
>
> * 從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。
> * 重新啟動伺服器。

#### <a name="macos"></a>macOS

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

#### <a name="linux"></a>Linux

針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。

### <a name="set-the-environment-in-code"></a>在代碼中設定環境

生成<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*>主機時調用。 請參閱＜<xref:fundamentals/host/generic-host#environmentname>＞。


### <a name="configuration-by-environment"></a>取決於環境的組態

若要依環境載入組態，建議使用：

* *套用設定*檔案(*應用程式設定。環境\.json*。 請參閱＜<xref:fundamentals/configuration/index#json-configuration-provider>＞。
* 環境變數(在託管應用的每個系統上設置)。 請參閱 <xref:fundamentals/host/generic-host#environmentname> 和 <xref:security/app-secrets#environment-variables>。
* 祕密管理員 (僅限開發環境)。 請參閱＜<xref:security/app-secrets>＞。

## <a name="environment-based-startup-class-and-methods"></a>以環境為基礎的 Startup 類別和方法

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a>將 IWebHost 環境注入啟動。

註<xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>`Startup.Configure`入 。 當應用僅需要調整`Startup.Configure`每個環境代碼差異最小的幾個環境時,此方法非常有用。

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a>將 IWebHost 環境注入開機類別

注入<xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>建構`Startup`函數。 當應用只需要為每個環境的代碼差異最小的幾個`Startup`環境進行配置時,此方法非常有用。

在下例中︰

* 環境在`_env`野外舉行。
* `_env`用於`ConfigureServices`和應用`Configure`基於應用環境的啟動配置。

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```
### <a name="startup-class-conventions"></a>Startup 類別慣例

ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。 應用可以為不同的環境定義`Startup`單獨的類(例如`StartupDevelopment`, 在運行時`Startup`選擇相應的類。 將優先使用其名稱尾碼符合目前環境的類別。 如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。 當應用需要為每個環境具有許多代碼差異的多個環境配置啟動時,此方法非常有用。

若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：

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

### <a name="startup-method-conventions"></a>Startup 方法慣例

[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)與[設定服務](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援特定於環境的`Configure<EnvironmentName>`表`Configure<EnvironmentName>Services`單與 。 當應用需要為每個環境具有許多代碼差異的多個環境配置啟動時,此方法非常有用。

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample)([如何下載](xref:index#how-to-download-a-sample))

## <a name="environments"></a>環境

ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName) 中。 `ASPNETCORE_ENVIRONMENT`可以設置為任何值,但框架提供了三個值:

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (預設值)

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

上述程式碼：

* 當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。
* 當 `ASPNETCORE_ENVIRONMENT` 的值設為下列其中之一時，會呼叫 [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)：

  * `Staging`
  * `Production`
  * `Staging_2`

[環境標記説明程式](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)使用`IHostingEnvironment.EnvironmentName`的值 在 元素中包括或排除標記:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

在 Windows 及 macOS 中，環境變數和值不區分大小寫。 Linux 的環境變數和值則預設為**區分大小寫**。

### <a name="development"></a>部署

您可以在開發環境中啟用生產環境不應該公開的功能。 例如,ASP.NET核心範本在開發環境中啟用[開發人員異常頁](xref:fundamentals/error-handling#developer-exception-page)。

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

以 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動應用程式時，會使用具有 `"commandName": "Project"` 的第一個設定檔。 `commandName` 的值可指定要啟動的網頁伺服器。 `commandName` 可以是下列任何一個項目：

* `IISExpress`
* `IIS`
* `Project` (這會啟動 Kestrel)

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

Visual Studio 專案屬性的 [偵錯]**** 索引標籤提供 GUI，可編輯 *launchSettings.json* 檔案：

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

### <a name="production"></a>Production

若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。 不同於開發的某些一般設定包括：

* 快取。
* 用戶端的資源會經過配套、縮減，且可能由 CDN 提供。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 啟用生產記錄與監視。 例如,[應用程式見解](/azure/application-insights/app-insights-asp-net-core)。

## <a name="set-the-environment"></a>設定環境

使用環境變數或平臺設置設置用於測試的特定環境通常很有用。 如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。 環境的設定方法取決於作業系統而定。

生成主機時,應用讀取的最後一個環境設置將確定應用的環境。 應用在運行時無法更改應用的環境。

### <a name="environment-variable-or-platform-setting"></a>環境變數或平台設定

#### <a name="azure-app-service"></a>Azure App Service

若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：

1. 從 [應用程式服務]**** 刀鋒視窗選取應用程式。
1. 在 **「設定」** 群組中,選擇 **「設定」** 邊欄選項卡。
1. 在應用程式**設定「** 選擇」 中, 選擇 **「新增應用程式」 設定**。
1. 在 **'新增/編輯應用程式' 設定**視窗`ASPNETCORE_ENVIRONMENT`中, 提供**名稱**。 對於**值**,提供環境(例如`Staging`, 。
1. 如果您希望環境設置在交換部署槽時保留與當前插槽一起,請選擇 **「部署槽設置**」複選框。 有關詳細資訊,請參閱在[Azure 文件中設定 Azure 應用服務中的暫存環境](/azure/app-service/web-sites-staged-publishing)。
1. 選擇 **「確定」** 以關閉 **「新增/編輯應用程式」設定**視窗。
1. 選擇 **「在****設定**」邊欄選項卡頂部保存。

Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。

#### <a name="windows"></a>Windows

如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：

**指令提示符**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

這些命令僅針對目前的視窗才會生效。 視窗關閉時，`ASPNETCORE_ENVIRONMENT` 設定會還原為預設值或電腦值。

若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：

* 開啟 [控制台]**[系統]** > **[進階系統設定]** > ****，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* 開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：

  **指令提示符**

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

若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞** 一節。

**專案檔或發行設定檔**

**對於 Windows IIS 部署:** 在`<EnvironmentName>`[發佈設定檔 (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中包括該屬性。 此方法會在專案發行時於 *web.config* 中設定環境：

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

**每個 IIS 應用程式集區**

若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞** 一節。 當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。

> [!IMPORTANT]
> 當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：
>
> * 從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。
> * 重新啟動伺服器。

#### <a name="macos"></a>macOS

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

#### <a name="linux"></a>Linux

針對 Linux 散發版本，請在命令提示字元使用 `export` 命令，進行以工作階段為基礎的變數設定，並使用 *bash_profile* 檔案，進行電腦層級的環境設定。

### <a name="set-the-environment-in-code"></a>在代碼中設定環境

生成<xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*>主機時調用。 請參閱＜<xref:fundamentals/host/web-host#environment>＞。

### <a name="configuration-by-environment"></a>取決於環境的組態

若要依環境載入組態，建議使用：

* *套用設定*檔案(*應用程式設定。環境\.json*。 請參閱＜<xref:fundamentals/configuration/index#json-configuration-provider>＞。
* 環境變數(在託管應用的每個系統上設置)。 請參閱 <xref:fundamentals/host/web-host#environment> 和 <xref:security/app-secrets#environment-variables>。
* 祕密管理員 (僅限開發環境)。 請參閱＜<xref:security/app-secrets>＞。

## <a name="environment-based-startup-class-and-methods"></a>以環境為基礎的 Startup 類別和方法

### <a name="inject-ihostingenvironment-into-startupconfigure"></a>將 I託管環境注入啟動。

註<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>`Startup.Configure`入 。 當應用僅需要為每個環境代碼差異最小的少數環境`Startup.Configure`配置時,此方法非常有用。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a>將 IHosting 環境注入啟動類別

注入<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>建構函`Startup`數 並將服務分配給一個字段,以便在整個類`Startup`中使用 。 當應用僅要求對每個環境代碼差異最小的少數環境配置啟動時,此方法非常有用。

在下例中︰

* 環境在`_env`野外舉行。
* `_env`用於`ConfigureServices`和應用`Configure`基於應用環境的啟動配置。

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a>Startup 類別慣例

ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。 應用可以為不同的環境定義`Startup`單獨的類(例如`StartupDevelopment`, 在運行時`Startup`選擇相應的類。 將優先使用其名稱尾碼符合目前環境的類別。 如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。 當應用需要為每個環境具有許多代碼差異的多個環境配置啟動時,此方法非常有用。

若要實作以環境為基礎的 `Startup` 類別，請為每個使用中的環境建立 `Startup{EnvironmentName}` 類別和後援 `Startup` 類別：

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：

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

### <a name="startup-method-conventions"></a>Startup 方法慣例

[設定](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)與[設定服務](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援特定於環境的`Configure<EnvironmentName>`表`Configure<EnvironmentName>Services`單與 。 當應用需要為每個環境具有許多代碼差異的多個環境配置啟動時,此方法非常有用。

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
