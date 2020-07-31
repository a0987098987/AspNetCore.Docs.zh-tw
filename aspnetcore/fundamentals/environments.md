---
title: 在 ASP.NET Core 中使用多個環境
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中如何跨多個環境控制應用程式的行為。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 7/1/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/environments
ms.openlocfilehash: 977d222ed61fa914bd4ffb70e192ef19d4da5c33
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "87330610"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>在 ASP.NET Core 中使用多個環境

::: moniker range=">= aspnetcore-3.0"

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)

ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="environments"></a>環境

若要判斷執行時間環境，ASP.NET Core 從下列環境變數讀取：

1. [DOTNET_ENVIRONMENT](xref:fundamentals/configuration/index#default-host-configuration)
1. `ASPNETCORE_ENVIRONMENT`當 <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> 呼叫時。 預設 ASP.NET Core web 應用程式範本呼叫 `ConfigureWebHostDefaults` 。 `ASPNETCORE_ENVIRONMENT`值會覆寫 `DOTNET_ENVIRONMENT` 。

`IHostEnvironment.EnvironmentName`可以設定為任何值，但下列值是由架構所提供：

* <xref:Microsoft.Extensions.Hosting.Environments.Development>：在本機電腦上，將檔案[上的launchSettings.js](#lsj)設定 `ASPNETCORE_ENVIRONMENT` 為 `Development` 。
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <xref:Microsoft.Extensions.Hosting.Environments.Production>：如果 `DOTNET_ENVIRONMENT` 未設定和，則為預設值 `ASPNETCORE_ENVIRONMENT` 。

下列程式碼：

* 當 `ASPNETCORE_ENVIRONMENT` 設為 `Development` 時，會呼叫 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage)。
* 當[UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler)的值 `ASPNETCORE_ENVIRONMENT` 設定為、或時，會呼叫 UseExceptionHandler `Staging` `Production` `Staging_2` 。
* 將插入 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 中 `Startup.Configure` 。 當應用程式只需要調整 `Startup.Configure` 少數環境，且每個環境的程式碼差異最低時，這個方法就很有用。
* 類似于 ASP.NET Core 範本所產生的程式碼。

[!code-csharp[](environments/3.1sample/EnvironmentsSample/Startup.cs?name=snippet)]

[環境標記](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)協助程式會使用[IHostEnvironment](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)的值來包含或排除元素中的標記：

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)中的 [[關於] 頁面](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml)包含前述標記，並顯示的值 `IWebHostEnvironment.EnvironmentName` 。

在 Windows 和 macOS 上，環境變數和值不區分大小寫。 根據預設，Linux 環境變數和值會區分**大小寫**。

### <a name="create-environmentssample"></a>建立 EnvironmentsSample

本檔中使用的[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)是以 Razor 名為*EnvironmentsSample*的頁面專案為基礎。

下列程式碼會建立並執行名為*EnvironmentsSample*的 web 應用程式：

```bash
dotnet new webapp -o EnvironmentsSample
cd EnvironmentsSample
dotnet run --verbosity normal
```

當應用程式執行時，它會顯示下列部分輸出：

```bash
Using launch settings from c:\tmp\EnvironmentsSample\Properties\launchSettings.json
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: c:\tmp\EnvironmentsSample
```

<a name="lsj"></a>

### <a name="development-and-launchsettingsjson"></a>開發與 launchSettings.js

您可以在開發環境中啟用生產環境不應該公開的功能。 例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。

您可以在專案的 *Properties\launchSettings.json* 檔案設定適用於本機電腦開發的環境。 在 *launchSettings.json* 中設定的環境值會覆寫系統環境的設定值。

檔案*上的launchSettings.js* ：

* 僅用於本機開發電腦。
* 未部署。
* 包含設定檔設定。

下列 JSON 會針對以 Visual Studio 或建立的 ASP.NET Core 網路計畫，顯示名為*EnvironmentsSample*的檔案*launchSettings.js*檔案 `dotnet new` ：

[!code-json[](environments/3.1sample/EnvironmentsSample/Properties/launchSettingsCopy.json)]

上述標記包含兩個設定檔：

* `IIS Express`：從 Visual Studio 啟動應用程式時所使用的預設設定檔。 此索引 `"commandName"` 鍵的值 `"IISExpress"` 為，因此， [IISExpress](/iis/extensions/introduction-to-iis-express/iis-express-overview)是 web 伺服器。 您可以將啟動設定檔設定為專案或包含的任何其他設定檔。 例如，在下圖中，選取專案名稱會啟動[Kestrel web 伺服器](xref:fundamentals/servers/kestrel)。

  ![IIS Express 在功能表上啟動](environments/_static/iisx2.png)
* `EnvironmentsSample`：設定檔名稱是專案名稱。 當使用啟動應用程式時，預設會使用此設定檔 `dotnet run` 。  `"commandName"`金鑰具有值 `"Project"` ，因此會啟動[Kestrel web 伺服器](xref:fundamentals/servers/kestrel)。

的值 `commandName` 可指定要啟動的 web 伺服器。 `commandName` 可以是下列任何一個項目：

* `IISExpress`：啟動 IIS Express。
* `IIS`：未啟動任何 web 伺服器。 IIS 應該可以使用。
* `Project`：啟動 Kestrel。

[Visual Studio 專案屬性] [**調試**] 索引標籤會提供 GUI 來編輯檔案*上的launchSettings.js* 。 您對專案設定檔所做的變更可能要等重新啟動網頁伺服器後才會生效。 您必須重新啟動 Kestrel，它才會偵測到環境已進行的變更。

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

下列*launchSettings.js*檔案包含多個設定檔：

[!code-json[](environments/3.1sample/EnvironmentsSample/Properties/launchSettings.json)]

可以選取設定檔：

* 從 [Visual Studio] UI。
* [`dotnet run`](/dotnet/core/tools/dotnet-run)在命令 shell 中使用命令，並將 `--launch-profile` 選項設定為設定檔的名稱。 *這個方法只支援 Kestrel 設定檔。*

  ```dotnetcli
  dotnet run --launch-profile "SampleApp"
  ```

> [!WARNING]
> *launchSettings.json* 不應儲存密碼。 [密碼管理員工具](xref:security/app-secrets)可以用來儲存本機開發的密碼。

使用 [Visual Studio Code](https://code.visualstudio.com/) 時，可以會在 *.vscode/launch.json* 檔案設定環境變數。 下列範例會設定數個[主機配置值環境變數](xref:fundamentals/host/web-host#host-configuration-values)：

[!code-json[](environments/3.1sample/EnvironmentsSample/.vscode/launch.json?range=4-10,32-38)]

檔案的*vscode/launch.js*僅供 Visual Studio Code 使用。

### <a name="production"></a>生產

應設定生產環境，以最大化安全性、[效能](xref:performance/performance-best-practices)和應用程式的穩定性。 不同於開發的某些一般設定包括：

* 快[取。](xref:performance/caching/memory)
* 用戶端的資源會經過配套、縮減，且可能由 CDN 提供。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 已啟用生產[記錄](xref:fundamentals/logging/index)和監視。 例如，使用[Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="set-the-environment"></a>設定環境

設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。 如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。 環境的設定方法取決於作業系統而定。

建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。 應用程式正在執行時，無法變更應用程式的環境。

[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample)中的 [[關於] 頁面](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/3.1sample/EnvironmentsSample/Pages/About.cshtml)會顯示的值 `IWebHostEnvironment.EnvironmentName` 。

### <a name="azure-app-service"></a>Azure App Service

若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：

1. 從 [應用程式服務]**** 刀鋒視窗選取應用程式。
1. 在 [**設定**] 群組中，**選取 [** 設定] 分頁。
1. 在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。
1. 在 [**新增/編輯應用程式設定**] 視窗中，提供 `ASPNETCORE_ENVIRONMENT` **名稱**的。 針對 [**值**]，提供環境（例如 `Staging` ）。
1. 如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。 如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。
1. 選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。
1. 選取 [設定] 分頁頂端的 **[** **儲存**]。

Azure App Service 在 Azure 入口網站中新增、變更或刪除應用程式設定後，自動重新開機應用程式。

### <a name="windows"></a>Windows

*launchSettings.js*中的環境值會覆寫系統內容中所設定的覆寫值。

如果應用程式是使用 [dotnet run](/dotnet/core/tools/dotnet-run) 啟動，請使用下列命令來設定目前工作階段的 `ASPNETCORE_ENVIRONMENT`：

**命令提示字元**

```console
set ASPNETCORE_ENVIRONMENT=Staging
dotnet run --no-launch-profile
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Staging"
dotnet run --no-launch-profile
```

上述命令 `ASPNETCORE_ENVIRONMENT` 只會設定從該命令視窗啟動的進程。

若要在 Windows 中以全域的方式設定值，請使用下列其中一個方法：

* 開啟 [控制台]**[系統]** > **[進階系統設定]** > ****，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：

  ![系統進階屬性](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境變數](environments/_static/windows_aspnetcore_environment.png)

* 開啟系統管理命令提示字元，然後使用 `setx` 命令，或開啟系統管理 PowerShell 命令提示字元，然後使用 `[Environment]::SetEnvironmentVariable`：

  **命令提示字元**

  ```console
  setx ASPNETCORE_ENVIRONMENT Staging /M
  ```

  `/M` 參數表示將環境變數設定在系統層級。 若未使用 `/M` 參數，則將環境變數設定為使用者帳戶。

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Staging", "Machine")
  ```

  `Machine` 選項值表示將環境變數設定在系統層級。 若選項值變更為 `User`，則將環境變數設定為使用者帳戶。

當以全域的方式設定 `ASPNETCORE_ENVIRONMENT` 環境變數時，則在設定該值後開啟的任何命令視窗中，對 `dotnet run` 均有效。 *launchSettings.js*中的環境值會覆寫系統內容中所設定的覆寫值。

**web.config**

若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞** 一節。

**專案檔或發行設定檔**

**針對 WINDOWS IIS 部署：**`<EnvironmentName>`將屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。 此方法會在專案發行時於 *web.config* 中設定環境：

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

**每個 IIS 應用程式集區**

若要為執行於隔離應用程式集區 (受 IIS 10.0 或更新版本支援) 的應用程式設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱[環境變數 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的＜AppCmd.exe 命令＞** 一節。 當 `ASPNETCORE_ENVIRONMENT` 環境變數設定為應用程式集區時，其值會覆寫系統層級的設定。

當在 IIS 中裝載應用程式，並新增或變更 `ASPNETCORE_ENVIRONMENT` 環境變數時，請使用下列任一種方法，讓應用程式挑選新的值：

* 從命令提示字元執行後面接著 `net start w3svc` 的 `net stop was /y`。
* 重新啟動伺服器。

#### <a name="macos"></a>macOS

您可以在執行應用程式時，以內嵌方式設定 macOS 目前的環境：

```bash
ASPNETCORE_ENVIRONMENT=Staging dotnet run
```

或者，在執行應用程式之前，使用 `export` 設定環境：

```bash
export ASPNETCORE_ENVIRONMENT=Staging
```

您可以在 *.bashrc* 或 *.bash_profile* 檔案中設定電腦層級的環境變數。 使用任何文字編輯器編輯檔案。 新增下列陳述式：

```bash
export ASPNETCORE_ENVIRONMENT=Staging
```

#### <a name="linux"></a>Linux

針對 Linux 散發套件，請 `export` 在命令提示字元中使用命令來進行會話型變數設定，並針對電腦層級的環境設定*bash_profile*檔案。

### <a name="set-the-environment-in-code"></a>在程式碼中設定環境

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*>在建立主機時呼叫。 請參閱＜ <xref:fundamentals/host/generic-host#environmentname> ＞。

### <a name="configuration-by-environment"></a>取決於環境的組態

若要依環境載入設定，請參閱 <xref:fundamentals/configuration/index#json-configuration-provider> 。

## <a name="environment-based-startup-class-and-methods"></a>以環境為基礎的 Startup 類別和方法

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a>將 IWebHostEnvironment 插入 Startup 類別

插入 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 至此函式 `Startup` 。 當應用程式 `Startup` 只需要針對少數環境進行設定，而且每個環境的程式碼差異最低時，這個方法就很有用。

在下例中︰

* 環境會保留在欄位中 `_env` 。
* `_env`會在和中使用 `ConfigureServices` ， `Configure` 以根據應用程式的環境來套用啟動設定。

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupInject.cs?name=snippet&highlight=3-11)]

### <a name="startup-class-conventions"></a>Startup 類別慣例

ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。 應用程式可以 `Startup` 針對不同的環境定義多個類別。 在 `Startup` 執行時間選取適當的類別。 將優先使用其名稱尾碼符合目前環境的類別。 如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。 當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。 一般的應用程式不需要這種方法。

若要執行以環境為基礎的 `Startup` 類別，請建立 `Startup{EnvironmentName}` 類別和 fallback `Startup` 類別：

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupClassConventions.cs?name=snippet)]

使用接受組件名稱的 [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) 多載：

[!code-csharp[](environments/3.1sample/EnvironmentsSample/Program.cs?name=snippet)]

### <a name="startup-method-conventions"></a>Startup 方法慣例

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單和的環境特定版本 `Configure<EnvironmentName>` `Configure<EnvironmentName>Services` 。 當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，這個方法會很有用：

[!code-csharp[](environments/3.1sample/EnvironmentsSample/StartupMethodConventions.cs?name=snippet)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* <xref:blazor/fundamentals/environments>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 會使用環境變數根據執行階段環境來設定應用程式行為。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="environments"></a>環境

ASP.NET Core 會在應用程式啟動時讀取 `ASPNETCORE_ENVIRONMENT` 環境變數，並將該值儲存在 [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName) 中。 `ASPNETCORE_ENVIRONMENT`可以設定為任何值，但架構會提供三個值：

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

[環境標記](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)協助程式會使用的值 `IHostingEnvironment.EnvironmentName` 來包含或排除元素中的標記：

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

在 Windows 和 macOS 上，環境變數和值不區分大小寫。 根據預設，Linux 環境變數和值會區分大小寫。

### <a name="development"></a>部署

您可以在開發環境中啟用生產環境不應該公開的功能。 例如，ASP.NET Core 範本會在開發環境中啟用[開發人員例外狀況頁面](xref:fundamentals/error-handling#developer-exception-page)。

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

### <a name="production"></a>生產

若要將安全性、效能及應用程式加強性最大化，您應該設定生產環境。 不同於開發的某些一般設定包括：

* 快取。
* 用戶端的資源會經過配套、縮減，且可能由 CDN 提供。
* 停用診斷錯誤頁面。
* 啟用易懂的錯誤頁面。
* 啟用生產記錄與監視。 例如， [Application Insights](/azure/application-insights/app-insights-asp-net-core)。

## <a name="set-the-environment"></a>設定環境

設定特定環境以使用環境變數或平臺設定進行測試時，通常會很有用。 如果您未設定環境，它會預設為 `Production` 並停用大部分的偵錯功能。 環境的設定方法取決於作業系統而定。

建立主機時，應用程式所讀取的最後一個環境設定會決定應用程式的環境。 應用程式正在執行時，無法變更應用程式的環境。

### <a name="environment-variable-or-platform-setting"></a>環境變數或平臺設定

#### <a name="azure-app-service"></a>Azure App Service

若要在 [Azure App Service](https://azure.microsoft.com/services/app-service/) 設定環境，請執行下列步驟：

1. 從 [應用程式服務]**** 刀鋒視窗選取應用程式。
1. 在 [**設定**] 群組中，**選取 [** 設定] 分頁。
1. 在 [**應用程式設定**] 索引標籤中，選取 [**新增應用程式設定**]。
1. 在 [**新增/編輯應用程式設定**] 視窗中，提供 `ASPNETCORE_ENVIRONMENT` **名稱**的。 針對 [**值**]，提供環境（例如 `Staging` ）。
1. 如果您想要在交換部署位置時，將環境設定維持在目前的位置，請選取 [**部署位置設定**] 核取方塊。 如需詳細資訊，請參閱 Azure 檔[中的在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)。
1. 選取 **[確定]** 以關閉 [**新增/編輯應用程式設定**] 視窗。
1. 選取 [設定] 分頁頂端的 **[** **儲存**]。

Azure App Service 會在新增、變更或刪除 Azure 入口網站的應用程式設定 (環境變數) 之後自動重新啟動應用程式。

#### <a name="windows"></a>Windows

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

* 開啟 [控制台]**[系統]** > **[進階系統設定]** > ****，然後新增或編輯 `ASPNETCORE_ENVIRONMENT` 值：

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

若要使用 *web.config* 設定 `ASPNETCORE_ENVIRONMENT` 環境變數，請參閱 <xref:host-and-deploy/aspnet-core-module#setting-environment-variables> 的＜設定環境變數＞** 一節。

**專案檔或發行設定檔**

**針對 WINDOWS IIS 部署：**`<EnvironmentName>`將屬性包含在[發行設定檔（. .pubxml）](xref:host-and-deploy/visual-studio-publish-profiles)或專案檔中。 此方法會在專案發行時於 *web.config* 中設定環境：

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

針對 Linux 散發套件，請 `export` 在命令提示字元中使用命令來進行會話型變數設定，並針對電腦層級的環境設定*bash_profile*檔案。

### <a name="set-the-environment-in-code"></a>在程式碼中設定環境

<xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*>在建立主機時呼叫。 請參閱＜ <xref:fundamentals/host/web-host#environment> ＞。

### <a name="configuration-by-environment"></a>取決於環境的組態

若要依環境載入組態，建議使用：

* *appsettings* files （*appsettings. {環境}. json*）。 請參閱＜ <xref:fundamentals/configuration/index#json-configuration-provider> ＞。
* 環境變數（在裝載應用程式的每個系統上設定）。 請參閱 <xref:fundamentals/host/web-host#environment> 和 <xref:security/app-secrets#environment-variables>。
* 祕密管理員 (僅限開發環境)。 請參閱＜ <xref:security/app-secrets> ＞。

## <a name="environment-based-startup-class-and-methods"></a>以環境為基礎的 Startup 類別和方法

### <a name="inject-ihostingenvironment-into-startupconfigure"></a>將 IHostingEnvironment 插入 Startup.Configu

插入 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> `Startup.Configure` 。 當應用程式只需要 `Startup.Configure` 針對每個環境具有最低程式碼差異的幾個環境進行設定時，這個方法就很有用。

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a>將 IHostingEnvironment 插入 Startup 類別

將插入 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> 至此函式 `Startup` ，並將服務指派給欄位，以便在整個類別中使用 `Startup` 。 當應用程式需要針對少數環境設定啟動，但每個環境的程式碼差異最低時，這個方法會很有用。

在下例中︰

* 環境會保留在欄位中 `_env` 。
* `_env`會在和中使用 `ConfigureServices` ， `Configure` 以根據應用程式的環境來套用啟動設定。

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

ASP.NET Core 應用程式啟動時，[Startup 類別](xref:fundamentals/startup)會啟動應用程式。 應用程式可以 `Startup` 針對不同的環境定義個別的類別（例如， `StartupDevelopment` ）。 在 `Startup` 執行時間選取適當的類別。 將優先使用其名稱尾碼符合目前環境的類別。 如果找不到相符的 `Startup{EnvironmentName}` 類別，會使用 `Startup` 類別。 當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。

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

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)支援表單和的環境特定版本 `Configure<EnvironmentName>` `Configure<EnvironmentName>Services` 。 當應用程式需要針對數個環境設定啟動，且每個環境有許多程式碼差異時，此方法很有用。

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
