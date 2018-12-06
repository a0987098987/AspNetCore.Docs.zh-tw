---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.2'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f857e96108b68bb6ec64a85910bf4d889cdf2822
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458513"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服務上裝載 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。 當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="deployment-type"></a>部署類型

您可以建立架構相依或自封式 Windows 服務部署。 如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。

### <a name="framework-dependent-deployment"></a>架構相的部署

架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。 搭配 ASP.NET Core Windows 服務應用程式使用 FDD 案例時，SDK 會產生可執行檔 (*\*.exe*)，稱為架構相依可執行檔。

### <a name="self-contained-deployment"></a>自封式部署

自封式部署 (SCD) 不仰賴任何存在於目標系統上的共用元件。 執行階段與應用程式的相依性會隨著應用程式部署到裝載系統。

## <a name="convert-a-project-into-a-windows-service"></a>將專案轉換成 Windows 服務

對現有的 ASP.NET Core 專進行下列變更，以服務形式執行應用程式：

1. 視您選擇的[部署類型](#deployment-type)而定，更新專案檔：

   * **架構相依部署 (FDD)** &ndash; 新增 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 到包含目標架構的 `<PropertyGroup>`。 新增 `<SelfContained>` 屬性集到 `false`。 透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <SelfContained>false</SelfContained>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     **自封式部署 (SCD)** &ndash; 確認 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 存在或新增 RID 到包含目標架構的 `<PropertyGroup>`。 透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。

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

   * 新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。

   * 若要啟用 Windows 事件記錄的記錄功能，請加入對 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。

     如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。

1. 在 `Program.Main` 中進行下列變更：

   * 若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。 檢查偵錯工具是否已附加或 `--console` 命令列引數是否存在。

     若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 Web 主機上的 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。

     若條件為偽 (應用程式是以服務形式執行)：

     * 呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> 並使用應用程式發佈位置的路徑。 請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 `GetCurrentDirectory` 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。 如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。
     * 呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。

     因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider) 需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數移除。

   * 若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。 使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。 針對示範與測試用途，範例應用程式的生產設定檔案會將記錄層級設定為 `Information`。 在生產環境中，該值一般會設定為 `Error`。 如需詳細資訊，請參閱<xref:fundamentals/logging/index#windows-eventlog-provider>。

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

1. 使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。 使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。

   若要使用命令列介面 (CLI) 工具發佈應用程式，請在將發行設定傳遞到 [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) 選項的情況下從專案資料夾的命令提示字元中執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令。 搭配路徑使用 [-o|--output](/dotnet/core/tools/dotnet-publish#options) 選項以發行到應用程式以外的資料夾。

   * **架構相依部署 (FDD)**

     在下列範例中，應用程式是發行到 *c:\\svc* 資料夾：

     ```console
     dotnet publish --configuration Release --output c:\svc
     ```

   * **自封式部署 (SCD)** &ndash; 必須在專案檔的 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 屬性中指定 RID。 提供執行階段給 `dotnet publish` 命令的 [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) 選項。

     在下列範例中，應用程式是針對 `win7-x64` 執行階段發行到 *c:\\svc* 資料夾：

     ```console
     dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
     ```

1. 使用 `net user` 命令為服務建立使用者帳戶：

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   針對範例應用程式，建立名為 `ServiceUser` 的使用者帳戶與密碼。 在下列命令中，將 `{PASSWORD}` 取代為[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   若需要將使用者新增到某個群組，請使用 `net localgroup` 命令，其中 `{GROUP}` 是群組的名稱：

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   如需詳細資訊，請參閱[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。

1. 使用 [icacls](/windows-server/administration/windows-commands/icacls) 命令授與對應用程式資料夾的寫入/讀取/執行存取權：

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; 應用程式資料夾的路徑。
   * `{USER ACCOUNT}` &ndash; 使用者帳戶 (SID)。
   * `(OI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。
   * `(CI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。
   * `{PERMISSION FLAGS}` &ndash; 設定應用程式的存取權限。
     * 寫入 (`W`)
     * 讀取 (`R`)
     * 執行 (`X`)
     * 完整 (`F`)
     * 修改 (`M`)
   * `/t` &ndash; 套用遞迴到現有的次級資料夾與檔案。

   針對發行到 *c:\\svc* 資料夾的範例應用程式與具有寫入/讀取/執行權限的 `ServiceUser` 帳戶，請使用下列命令：

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls)。

1. 使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具建立服務。 `binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。 **等號與每個參數與值的引號字元之間必須有空格。**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; 要指派給[服務控制管理員](/windows/desktop/services/service-control-manager)中之服務的名稱。
   * `{PATH}` &ndash; 服務可執行檔的路徑。
   * `{DOMAIN}` &ndash; 已加入網域之機器的網域。 若機器未加入網域，則為本機機器名稱。
   * `{USER ACCOUNT}` &ndash; 用於執行服務的使用者帳戶。
   * `{PASSWORD}` &ndash; 使用者帳戶密碼。

   > [!WARNING]
   > 請**勿**省略 `obj` 參數。 `obj` 的預設值是 [LocalSystem 帳戶](/windows/desktop/services/localsystem-account)帳戶。 以 `LocalSystem` 帳戶執行服務會帶來重大安全性風險。 一律使用具有受限權限的使用者帳戶來執行服務。

   在範例應用程式的下列範例中：

   * 服務的名稱是 **MyService**。
   * 已發行的服務位於 *c:\\svc* 資料夾。 應用程式可執行檔名稱是 *SampleApp.exe*。 以雙引號 (") 括住 `binPath` 值。
   * 服務是以 `ServiceUser` 帳戶執行。 將 `{DOMAIN}` 取代為使用者帳戶的網域或本機電腦名稱。 以雙引號 (") 括住 `obj` 值。 範例：若裝載系統是名為 `MairaPC` 的本機電腦，請將 `obj` 設定為 `"MairaPC\ServiceUser"`。
   * 將 `{PASSWORD}` 取代為使用者帳戶的密碼。 以雙引號 (") 括住 `password` 值。

   ```console
   sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > 確定參數的等號與參數的值之間有空格。

1. 以 `sc start {SERVICE NAME}` 命令啟動服務。

   請使用下列命令啟動範例應用程式服務：

   ```console
   sc start MyService
   ```

   此命令需要幾秒鐘啓動服務。

1. 若要檢查服務的狀態，請使用 `sc query {SERVICE NAME}` 命令。 狀態會回報為下列值之一：

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   使用下列命令來檢查範例應用程式服務的狀態：

   ```console
   sc query MyService
   ```

1. 當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。

   範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。

1. 以 `sc stop {SERVICE NAME}` 命令停止服務。

   下列命令會停止範例應用程式服務：

   ```console
   sc stop MyService
   ```

1. 在停止服務的短暫延遲之後，請以 `sc delete {SERVICE NAME}` 命令解除安裝服務。

   檢查範例應用程式服務的狀態：

   ```console
   sc query MyService
   ```

   當範例應用程式服務處於 `STOPPED` 狀態時，請使用下列命令來解除安裝範例應用程式服務：

   ```console
   sc delete MyService
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

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。 請搭配應用程式內容根目錄的路徑呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>，而不要呼叫 `GetCurrentDirectory` 來建立設定檔的路徑。

在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>將服務的檔案儲存在磁碟上的適當位置

使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。

## <a name="additional-resources"></a>其他資源

* [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
