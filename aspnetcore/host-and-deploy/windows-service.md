---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/09/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: c2a2941f2a4e27218c90cf47453c69149da8e766
ms.sourcegitcommit: 2d4c1732c4866ed26b83da35f7bc2ad021a9c701
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2019
ms.locfileid: "70815708"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服務上裝載 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。 當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

* [ASP.NET Core SDK 2.1 或更新版本](https://dotnet.microsoft.com/download)
* [PowerShell 6.2 或更新版本](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>背景工作服務範本

ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。 使用範本作為 Windows 服務應用程式的基礎：

1. 從 .NET Core 範本建立背景工作服務應用程式。
1. 請遵循[應用程式組態](#app-configuration)一節中的指導方針，更新背景工作服務應用程式，以便其執行為 Windows 服務。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新的專案。
1. 選取 [ASP.NET Core Web 應用程式]。 選取 [下一步]。
1. 在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。 選取 [建立]。
1. 在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。
1. 選取 [背景工作服務] 範本。 選取 [建立]。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令殼層以 [dotnet new](/dotnet/core/tools/dotnet-new) 命令使用背景工作服務 (`worker`) 範本。 在下列範例中，已建立名為 `ContosoWorkerService` 的背景工作服務應用程式。 當命令執行時，會自動建立 `ContosoWorkerService` 應用程式的資料夾。

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a>應用程式設定

::: moniker range=">= aspnetcore-3.0"

在建置主機時，將呼叫由 [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) 套件提供的 `IHostBuilder.UseWindowsService`。 如果應用程式以 Windows 服務形式執行，則方法會：

* 將主機存留期設定為 `WindowsServiceLifetime`。
* 設定內容根目錄。
* 啟用使用應用程式名稱作為預設來源名稱記錄至事件記錄檔。
  * 您可以使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼來設定記錄層級。
  * 只有系統管理員才能建立新的事件來源。 如果無法使用應用程式名稱建立事件來源，則會向「應用程式」來源記錄警告，並停用事件記錄檔。

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

應用程式需要 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 和 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。

若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。 檢查偵錯工具是否已附加或 `--console` 切換參數是否存在。 若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。 若條件為偽 (應用程式是以服務形式執行)：

* 呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。 請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。 如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。 在 `CreateWebHostBuilder` 中設定應用程式之前執行此步驟。
* 呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。

因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider)需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數中移除。

若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。 使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。

在範例應用程式的下列範例中，為了處理應用程式內的存留期事件，會呼叫 `RunAsCustomService` 而不是 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>。 如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>部署類型

如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。

### <a name="framework-dependent-deployment-fdd"></a>架構相依部署 (FDD)

架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。 依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 ( *.exe*)，稱為「架構相依可執行檔」。

::: moniker range=">= aspnetcore-3.0"

將下列屬性元素新增至專案檔：

* `<OutputType>` &ndash; 應用程式的輸出類型 (`Exe` 表示可執行檔)。
* `<LangVersion>` &ndash; C# 語言版本 (`latest` 或 `preview`)。

針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。 若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) 包含目標 Framework。 在下列範例中，RID 已設定為 `win7-x64`。 `<SelfContained>` 屬性設定為 `false`。 這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 ( *.exe*)。

針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。 若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) 包含目標 Framework。 在下列範例中，RID 已設定為 `win7-x64`。 `<SelfContained>` 屬性設定為 `false`。 這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 ( *.exe*)。

`<UseAppHost>` 屬性設定為 `true`。 此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。

針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。 若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a>自封式部署 (SCD)

自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。 執行階段與應用程式的相依性會隨著應用程式進行部署。

Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

發行多個 RID：

* 以分號分隔的清單提供 RID。
* 使用屬性名稱 [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (複數)。

如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。

::: moniker range="< aspnetcore-3.0"

`<SelfContained>` 屬性設定為 `true`：

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>服務使用者帳戶

若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。

Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：

```PowerShell
New-LocalUser -Name {NAME}
```

在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。

除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。

如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。

使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。 如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。

## <a name="log-on-as-a-service-rights"></a>以服務方式登入權限

若要為服務使用者帳戶建立「以服務方式登入」權限：

1. 執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。
1. 展開 [本機原則] 節點，然後選取 [使用者權限指派]。
1. 開啟 [以服務方式登入] 原則。
1. 選取 [新增使用者或群組]。
1. 使用下列其中一種方法提供物件名稱 (使用者帳戶)：
   1. 在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定] 將使用者新增至原則。
   1. 選取 [進階]。 選取 [立即尋找]。 從清單中選取使用者帳戶。 選取 [確定]。 再次選取 [確定] 將使用者新增至原則。
1. 選取 [確定] 或 [套用] 以接受變更。

## <a name="create-and-manage-the-windows-service"></a>建立及管理 Windows 服務

### <a name="create-a-service"></a>建立服務

使用 PowerShell 命令來註冊服務。 透過系統管理 PowerShell 6 命令殼層，執行下列命令：

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}` &ndash; 主機上應用程式的資料夾路徑 (例如 `d:\myservice`)。 請勿包含路徑中應用程式的可執行檔。 不需要結尾的斜線。
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash; 服務使用者帳戶 (例如 `Contoso\ServiceUser`)。
* `{NAME}` &ndash; 服務名稱 (例如 `MyService`)。
* `{EXE FILE PATH}` &ndash; 應用程式的可執行檔路徑 (例如 `d:\myservice\myservice.exe`)。 包含可執行檔的檔案名稱 (包含副檔名)。
* `{DESCRIPTION}` &ndash; 服務描述 (例如 `My sample service`)。
* `{DISPLAY NAME}` &ndash; 服務顯示名稱 (例如 `My Service`)。

### <a name="start-a-service"></a>啟動服務

以下列 PowerShell 6 命令啟動服務：

```powershell
Start-Service -Name {NAME}
```

此命令需要幾秒鐘啓動服務。

### <a name="determine-a-services-status"></a>判斷服務的狀態

若要檢查服務狀態，請使用下列 PowerShell 6 命令：

```powershell
Get-Service -Name {NAME}
```

狀態會回報為下列值之一：

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>停止服務

使用下列 Powershell 6 命令停止服務：

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>移除服務

在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>處理開始與停止事件

若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件：

1. 使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. 為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. 在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：

   ```csharp
   host.RunAsCustomService();
   ```

   若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[部署類型](#deployment-type)一節中的範例程式碼。

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。 如需詳細資訊，請參閱 <xref:host-and-deploy/proxy-load-balancer>。

## <a name="configure-endpoints"></a>設定端點

ASP.NET Core 預設會繫結至 `http://localhost:5000`。 設定`ASPNETCORE_URLS`環境變數，以設定 URL 和埠。

如需其他 URL 和埠設定方法，包括 HTTPS 端點的支援，請參閱下列主題：

* <xref:fundamentals/servers/kestrel#endpoint-configuration>Kestrel
* <xref:fundamentals/servers/httpsys#configure-windows-server>（Http.sys）

> [!NOTE]
> 不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。

## <a name="current-directory-and-content-root"></a>目前目錄和內容根目錄

針對 Windows 服務呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。 *System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。 使用下列其中一個方式來維護及存取服務的資產與設定檔。

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>使用 ContentRootPath 或 ContentRootFileProvider

使用 [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) 或 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> 來找出應用程式的資源。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>將內容根目錄路徑設定到應用程式的資料夾

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。 請搭配應用程式內容根目錄的路徑呼叫 <xref:System.IO.Directory.SetCurrentDirectory*>，而不要呼叫 `GetCurrentDirectory` 來建立設定檔的路徑。

在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>將服務的檔案儲存在磁碟上的適當位置

使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。

## <a name="additional-resources"></a>其他資源

::: moniker range=">= aspnetcore-3.0"

* [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
