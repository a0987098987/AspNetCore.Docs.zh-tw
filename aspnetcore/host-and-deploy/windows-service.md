---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068332"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服務上裝載 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。 當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

* [PowerShell 6.2 或更新版本](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> 針對早於 Windows 10 2018 年 10 月更新 (1809 版/組建 10.0.17763) 的 Windows OS，必須使用 [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) \(英文\) 來匯入 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) 模組，以存取用於[建立使用者帳戶](#create-a-user-account)一節中的 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet：
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a>部署類型

您可以建立架構相依或自封式 Windows 服務部署。 如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。

### <a name="framework-dependent-deployment"></a>與 Framework 相依的部署

架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。 搭配 ASP.NET Core Windows 服務應用程式使用 FDD 案例時，SDK 會產生可執行檔 (*\*.exe*)，稱為架構相依可執行檔。

### <a name="self-contained-deployment"></a>自封式部署

自封式部署 (SCD) 不仰賴任何存在於目標系統上的共用元件。 執行階段與應用程式的相依性會隨著應用程式部署到裝載系統。

## <a name="convert-a-project-into-a-windows-service"></a>將專案轉換成 Windows 服務

對現有的 ASP.NET Core 專進行下列變更，以服務形式執行應用程式：

### <a name="project-file-updates"></a>專案檔更新

視您選擇的[部署類型](#deployment-type)而定，更新專案檔：

#### <a name="framework-dependent-deployment-fdd"></a>架構相依部署 (FDD)

將 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 新增至包含目標 Framework 的 `<PropertyGroup>`。 在下列範例中，RID 已設定為 `win7-x64`。 新增 `<SelfContained>` 屬性集到 `false`。 這些屬性會指示 SDK 產生適用於 Windows 的可執行檔 (*.exe*)。

針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。 若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。

::: moniker range=">= aspnetcore-2.2"

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

新增 `<UseAppHost>` 屬性集到 `true`。 此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。

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

#### <a name="self-contained-deployment-scd"></a>自封式部署 (SCD)

確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將 RID 新增至包含目標 Framework 的 `<PropertyGroup>`。 透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

發行多個 RID：

* 以分號分隔的清單提供 RID。
* 使用屬性名稱 `<RuntimeIdentifiers>` (複數)。

  如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。

新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。

若要啟用 Windows 事件記錄的記錄功能，請加入對 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。

如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。

### <a name="programmain-updates"></a>Program.Main 更新

在 `Program.Main` 中進行下列變更：

* 若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。 檢查偵錯工具是否已附加或 `--console` 命令列引數是否存在。

  若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 Web 主機上的 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。

  若條件為偽 (應用程式是以服務形式執行)：

  * 呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。 請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。 如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。
  * 呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。

  因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider) 需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數移除。

* 若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。 使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。 針對示範與測試用途，範例應用程式的生產設定檔案會將記錄層級設定為 `Information`。 在生產環境中，該值一般會設定為 `Error`。 如需詳細資訊，請參閱<xref:fundamentals/logging/index#windows-eventlog-provider>。

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a>發行應用程式

使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。 使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。

若要使用命令列介面 (CLI) 工具來發佈範例應用程式，請從專案資料夾在 Windows 命令殼層中執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，同時搭配將發行設定傳遞至 [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) 選項。 搭配路徑使用 [-o|--output](/dotnet/core/tools/dotnet-publish#options) 選項以發行到應用程式以外的資料夾。

### <a name="publish-a-framework-dependent-deployment-fdd"></a>發行架構相依部署 (FDD)

在下列範例中，應用程式是發行到 *c:\\svc* 資料夾：

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a>發行自封式部署 (SCD)

必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。 提供執行階段給 `dotnet publish` 命令的 [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) 選項。

在下列範例中，應用程式是針對 `win7-x64` 執行階段發行到 *c:\\svc* 資料夾：

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a>建立使用者帳戶

從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，為服務建立使用者帳戶：

```powershell
New-LocalUser -Name {NAME}
```

在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。

針對範例應用程式，建立名為 `ServiceUser` 的使用者帳戶。

```powershell
New-LocalUser -Name ServiceUser
```

除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。

如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。

使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。 如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。

## <a name="set-permission-log-on-as-a-service"></a>設定權限：以服務方式登入

在系統管理 PowerShell 6 命令殼層中使用 [icacls](/windows-server/administration/windows-commands/icacls) 命令，授與對應用程式資料夾的寫入/讀取/執行存取權。

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* `{PATH}` &ndash; 應用程式資料夾的路徑。
* `{USER ACCOUNT}` &ndash; 使用者帳戶 (SID)。
* `(OI)` &ndash; 物件繼承旗標會將權限傳播到次級檔案。
* `(CI)` &ndash; 容器繼承旗標會將權限傳播到次級資料夾。
* `{PERMISSION FLAGS}` &ndash; 設定應用程式的存取權限。
  * 寫入 (`W`)
  * 讀取 (`R`)
  * 執行 (`X`)
  * 完整 (`F`)
  * 修改 (`M`)
* `/t` &ndash; 套用遞迴到現有的次級資料夾與檔案。

針對發行到 *c:\\svc* 資料夾的範例應用程式，以及具有寫入/讀取/執行權限的 `ServiceUser` 帳戶，請在系統管理 PowerShell 6 命令殼層中使用下列命令。

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls)。

## <a name="create-the-service"></a>建立服務

使用 [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) 的 PowerShell 指令碼註冊服務。 從系統管理 PowerShell 6 命令殼層，搭配下列命令執行指令碼：

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

在範例應用程式的下列範例中：

* 服務的名稱是 **MyService**。
* 已發行的服務位於 *c:\\svc* 資料夾。 應用程式可執行檔名稱是 *SampleApp.exe*。
* 服務是以 `ServiceUser` 帳戶執行。 下列範例命令中，本機電腦名稱為 `Desktop-PC`。 將 `Desktop-PC` 取代為您系統的電腦名稱或網域。

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a>管理服務

### <a name="start-the-service"></a>啟動服務

以 `Start-Service -Name {NAME}` 的 PowerShell 6 命令啟動服務。

請使用下列命令啟動範例應用程式服務：

```powershell
Start-Service -Name MyService
```

此命令需要幾秒鐘啓動服務。

### <a name="determine-the-service-status"></a>判斷服務狀態

若要檢查服務狀態，請使用 `Get-Service -Name {NAME}` 的 PowerShell 6 命令。 狀態會回報為下列值之一：

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

使用下列命令來檢查範例應用程式服務的狀態：

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a>瀏覽 Web 應用程式服務

當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。

範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。

### <a name="stop-the-service"></a>停止服務

以 `Stop-Service -Name {NAME}` 的 PowerShell 6 命令停止服務。

下列命令會停止範例應用程式服務：

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a>移除服務

在停止服務的短暫延遲之後，請以 `Remove-Service -Name {NAME}` 的 Powershell 6 命令移除服務。

檢查範例應用程式服務的狀態：

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a>處理開始與停止事件

若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件，請進行下列額外變更：

1. 使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. 為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. 在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：

   ```csharp
   host.RunAsCustomService();
   ```

   若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[將專案轉換為 Windows 服務](#convert-a-project-into-a-windows-service)一節中的範例程式碼。

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。 如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。

## <a name="configure-https"></a>設定 HTTPS

使用安全端點來設定服務：

1. 使用您平台的憑證取得與部署機制來建立將用於裝載系統的 X.509 憑證。

1. 指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)以使用憑證。

不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。

## <a name="current-directory-and-content-root"></a>目前目錄和內容根目錄

針對 Windows 服務呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。 *System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。 使用下列其中一個方式來維護及存取服務的資產與設定檔。

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

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>將服務的檔案儲存在磁碟上的適當位置

使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。

## <a name="additional-resources"></a>其他資源

* [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
