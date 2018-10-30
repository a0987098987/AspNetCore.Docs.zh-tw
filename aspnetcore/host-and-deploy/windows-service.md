---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f9b1c3fbfafa839c116688e0ac63804afcd5dbe0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206669"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>在 Windows 服務上裝載 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 應用程式可以裝載在 Windows 上，不需要使用 IIS 作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)。 當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>將專案轉換成 Windows 服務

至少需要變更下列內容，才能設定現有的 ASP.NET Core 專案作為服務執行：

1. 在專案檔中：

   * 確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將其新增至包含目標 Framework 的 `<PropertyGroup>`：

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      發行多個 RID：

      * 以分號分隔的清單提供 RID。
      * 使用屬性名稱 `<RuntimeIdentifiers>` (複數)。

      如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。

   * 新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。

1. 在 `Program.Main` 中進行下列變更：

   * 呼叫 [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)，而非 `host.Run`。

   * 呼叫 [UseContentRoot](xref:fundamentals/host/web-host#content-root) 並使用應用程式發佈位置的路徑，不要使用 `Directory.GetCurrentDirectory()`。

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. 發行應用程式。 使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) 或 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)。 使用 Visual Studio 時，請選取 [FolderProfile]。

   若要使用命令列介面 (CLI) 工具發行範例應用程式，請從專案資料夾的命令提示字元執行 [dotnet publish](/dotnet/core/tools/dotnet-publish)命令。 必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。 在下列範例中，應用程式在 `win7-x64` 執行階段發行設定中發行：

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. 使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具建立服務。 `binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。 **等號和路徑開頭的引號字元之間需要有間距。**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   對於在專案資料夾中發行的服務，請使用 *publish* 資料夾的路徑來建立服務。 在以下範例中：

   * 專案位於 *c:\\my_services\\AspNetCoreService* 資料夾。
   * 專案是以 `Release` 設定所發行。
   * 目標 Framework Moniker (TFM) 是 `netcoreapp2.1`。
   * 執行階段識別碼 (RID) 是 `win7-x64`。
   * 應用程式可執行檔的名稱是 *AspNetCoreService.exe*。
   * 服務的名稱是 **MyService**。

   範例：

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > 確定 `binPath=` 引數與其值之間具有間距。

   若要從不同的資料夾發行並啟動服務：

      * 在 `dotnet publish` 命令上使用 [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) 選項。 若使用 Visual Studio，選取 [發行] 按鈕之前，請先選取 [FolderProfile] 發行屬性頁面中的 [目標位置]。
      * 使用 `sc.exe` 命令搭配輸出資料夾路徑來建立服務。 在提供給 `binPath` 的路徑中包含服務的可執行檔名稱。

1. 以 `sc start <SERVICE_NAME>` 命令啟動服務。

   請使用下列命令啟動範例應用程式服務：

   ```console
   sc start MyService
   ```

   此命令需要幾秒鐘啓動服務。

1. 若要檢查服務的狀態，請使用 `sc query <SERVICE_NAME>` 命令。 狀態會回報為下列值之一：

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

1. 以 `sc stop <SERVICE_NAME>` 命令停止服務。

   下列命令會停止範例應用程式服務：

   ```console
   sc stop MyService
   ```

1. 在停止服務的短暫延遲之後，請以 `sc delete <SERVICE_NAME>` 命令解除安裝服務。

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

因為 ASP.NET Core 組態需要命令列引數成對的名稱和數值，所以會先移除 `--console` 參數，再將引數傳遞至 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)。

> [!NOTE]
> 因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>處理停止和啟動事件

請執行下列額外變更，以處理 [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) 和 [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) 事件：

1. 建立衍生自 [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) 的類別：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. 為將自訂的 `WebHostService` 傳遞給 [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) 的 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 建立擴充方法：

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. 在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)：

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > 因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) 屬性取得該服務：

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。 如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。

## <a name="configure-https"></a>設定 HTTPS

指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)。

## <a name="current-directory-and-content-root"></a>目前目錄和內容根目錄

針對 Windows 服務呼叫 `Directory.GetCurrentDirectory()` 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。 *System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。 請使用下列其中一種方法，於使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 時，利用 [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 來維護及存取服務的資產和設定檔：

* 使用內容根路徑。 `IHostingEnvironment.ContentRootPath` 與建立服務時提供給 `binPath` 引數的路徑相同。 請使用內容根路徑並維護應用程式內容根目錄中的檔案，而不是使用 `Directory.GetCurrentDirectory()` 來建立設定檔的路徑。
* 將檔案儲存在磁碟上的適當位置。 請使用 `SetBasePath` 將絕對路徑指定為包含檔案的資料夾。

## <a name="additional-resources"></a>其他資源

* [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)
* <xref:fundamentals/host/web-host>
