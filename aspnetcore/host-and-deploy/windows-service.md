---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758189"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服務上裝載 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 應用程式可以裝載在 Windows 上，不需要使用 IIS 作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)。 當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>將專案轉換成 Windows 服務

至少需要變更下列內容，才能設定現有的 ASP.NET Core 專案作為服務執行：

1. 在專案檔中：

   * 確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將其新增至包含目標 Framework 的 `<PropertyGroup>`：

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      發行多個 RID：

      * 以分號分隔的清單提供 RID。
      * 使用屬性名稱 `<RuntimeIdentifiers>` (複數)。

      如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。

   * 新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。

1. 在 `Program.Main` 中進行下列變更：

   * 呼叫 [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)，而非 `host.Run`。

   * 呼叫 [UseContentRoot](xref:fundamentals/host/web-host#content-root) 並使用應用程式發佈位置的路徑，不要使用 `Directory.GetCurrentDirectory()`。

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. 使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。 使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。

   若要使用命令列介面 (CLI) 工具發行範例應用程式，請從專案資料夾的命令提示字元執行 [dotnet publish](/dotnet/core/tools/dotnet-publish)命令。 必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。 在下列範例中，應用程式在 `win7-x64` 執行階段「發行設定」中發行到在 *c:\\svc* 建立的資料夾：

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
   * `{DOMAIN}` (或若電腦未加入網域，則為本機電腦名稱) 與 `{USER ACCOUNT}` &ndash; 服務執行所在的網域 (或本機電腦名稱) 與使用者帳戶。 請**勿**省略 `obj` 參數。 `obj` 的預設值是 [LocalSystem 帳戶](/windows/desktop/services/localsystem-account)帳戶。 以 `LocalSystem` 帳戶執行服務會帶來重大安全性風險。 一律與具有伺服器上受限權限的使用者帳戶來執行服務。
   * `{PASSWORD}` &ndash; 使用者帳戶密碼。

   在下列範例中：

   * 服務的名稱是 **MyService**。
   * 已發行的服務位於 *c:\\svc* 資料夾。 應用程式可執行檔的名稱是 *AspNetCoreService.exe*。 `binPath` 值以一般引號 (") 括住。
   * 服務是以 `ServiceUser` 帳戶執行。 將 `{DOMAIN}` 取代為使用者帳戶的網域或本機電腦名稱。 將 `obj` 值以一般引號 (") 括住。 範例：若裝載系統是名為 `MairaPC` 的本機電腦，請將 `obj` 設定為 `"MairaPC\ServiceUser"`。
   * 將 `{PASSWORD}` 取代為使用者帳戶的密碼。 `password` 值以一般引號 (") 括住。

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
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

## <a name="run-the-app-outside-of-a-service"></a>在服務外執行應用程式

在服務外執行時較容易進行測試和偵錯，因此習慣上只有在特定情況下，才會新增呼叫 `RunAsService` 的程式碼。 例如，使用 `--console` 命令列引數或連結偵錯工具，即可讓應用程式以主控台應用程式的形式執行：

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

因為 ASP.NET Core 組態需要命令列引數成對的名稱和數值，所以會先移除 `--console` 參數，再將引數傳遞至 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)。

> [!NOTE]
> 因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。

## <a name="handle-stopping-and-starting-events"></a>處理停止和啟動事件

請執行下列額外變更，以處理 [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) 和 [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) 事件：

1. 建立衍生自 [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) 的類別：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. 為將自訂的 `WebHostService` 傳遞給 [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) 的 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 建立擴充方法：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. 在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > 因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。

如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) 屬性取得該服務：

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。 如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。

## <a name="configure-https"></a>設定 HTTPS

使用安全端點來設定服務：

1. 使用您平台的憑證取得與部署機制來建立將用於裝載系統的 X.509 憑證。
1. 指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)以使用憑證。

不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。

## <a name="current-directory-and-content-root"></a>目前目錄和內容根目錄

針對 Windows 服務呼叫 `Directory.GetCurrentDirectory()` 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。 *System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。 請使用下列其中一種方法，於使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 時，利用 [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 來維護及存取服務的資產和設定檔：

* 使用內容根路徑。 `IHostingEnvironment.ContentRootPath` 與建立服務時提供給 `binPath` 引數的路徑相同。 請使用內容根路徑並維護應用程式內容根目錄中的檔案，而不是使用 `Directory.GetCurrentDirectory()` 來建立設定檔的路徑。
* 將檔案儲存在磁碟上的適當位置。 請使用 `SetBasePath` 將絕對路徑指定為包含檔案的資料夾。

## <a name="additional-resources"></a>其他資源

* [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)
* <xref:fundamentals/host/web-host>
