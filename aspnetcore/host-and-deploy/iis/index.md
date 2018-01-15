---
title: "在使用 IIS 的 Windows 上裝載 ASP.NET Core"
author: guardrex
description: "了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>在使用 IIS 的 Windows 上裝載 ASP.NET Core

作者：[Luke Latham](https://github.com/guardrex) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>支援的作業系統

支援下列作業系統：

* Windows 7 和更新版本
* Windows Server 2008 R2 和更新版本&#8224;

&#8224;就概念而言，本文件中描述的 IIS 組態也適用於在 Nano Server IIS 上裝載 ASP.NET Core 應用程式，但特定指示需參考 [Nano Server 上的 IIS 與 ASP.NET Core](xref:tutorials/nano-server)。

[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不適用 IIS 的反向 Proxy 組態。 請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。

## <a name="iis-configuration"></a>IIS 組態

啟用**網頁伺服器 (IIS)**角色，並建立角色服務。

### <a name="windows-desktop-operating-systems"></a>Windows 桌面作業系統

瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。 開啟 [Internet Information Services] 和 [Web 管理工具] 群組。 核取 [IIS 管理主控台] 方塊。 [World Wide Web Services] (全球資訊網服務) 核取方塊。 接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server 作業系統

伺服器作業系統，請透過 [伺服器管理員] 中的 [管理] 功能表或連結，使用 [新增角色及功能精靈]。 在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。

![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

在**角色服務**步驟中，選取所要的 IIS 角色服務或接受所提供的預設角色服務。

![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。 安裝網頁伺服器 (IIS) 角色之後，不需要重新啟動伺服器/IIS。

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>安裝 .NET Core Windows Server 裝載套件組合

1. 在主控系統上安裝 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore-2-windowshosting)。 套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。 此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。 如果系統沒有網際網路連線，請先取得並安裝 [Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)，再安裝 .NET Core Windows Server 裝載套件組合。

2. 重新啟動系統或從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，挑選系統 PATH 的變更。

> [!NOTE]
> 如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>使用 Visual Studio 發佈時安裝 Web Deploy

將應用程式部署到具有 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。 若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。 慣用的方法是使用 WebPI。 WebPI 提供獨立的安裝程式和組態以裝載提供者。

## <a name="application-configuration"></a>應用程式組態

### <a name="enabling-the-iisintegration-components"></a>啟用 IISIntegration 元件

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機。 `CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在應用程式相依性的 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件中包含相依性。 將 *UseIISIntegration* 擴充方法新增至 *WebHostBuilder*，以在應用程式中併入 IIS 整合中介軟體：

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

`UseKestrel` 與 `UseIISIntegration` 都是必要項目。 呼叫 `UseIISIntegration` 的程式碼並不會影響程式碼的可攜性。 若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會生效。

---

如需裝載的詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。

### <a name="iis-options"></a>IIS 選項

若要設定 IIS 選項，請在 `ConfigureServices` 中包含 `IISOptions` 的服務組態：

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| 選項                         | 預設 | 設定 |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | 如果為 `true`，則驗證中介軟體設為 `HttpContext.User`並回應泛行挑戰。 如果為 `false`，則驗證中介軟體僅提供身分識別 (`HttpContext.User`) 並當 `AuthenticationScheme` 提出明確要求時，回應泛型挑戰。 必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。 |
| `AuthenticationDisplayName`    | `null`  | 設定使用者在登入頁面上看到的顯示名稱。 |
| `ForwardClientCertificate`     | `true`  | 如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。 |

### <a name="webconfig"></a>web.config

*web.config* 檔案的主要目的是設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。 它能選擇性地提供其他 IIS 組態設定。 建立、轉換及發行 *web.config* 是由 .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) 處理。 SDK 設定在專案檔 `<Project Sdk="Microsoft.NET.Sdk.Web">` 的頂端。 為防止 SDK 轉換 *web.config* 檔案，請將 **\<IsTransformWebConfigDisabled>** 屬性新增至專案檔，並將其設定為 `true`：

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

如果 *web.config* 檔案在專案中，則系統會使用正確的 *processPath* 和 *arguments* 轉換它以設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，然後將它移至[已發行的輸出](xref:host-and-deploy/directory-structure)。 轉換不會修改檔案中的 IIS 組態設定。

### <a name="webconfig-location"></a>web.config 位置

.NET Core 應用程式是透過 IIS 和 Kestrel 伺服器之間的反向 Proxy 裝載。 為了建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)，這是提供給 IIS 的網站實體路徑。 應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。

機密檔案位於應用程式的實體路徑，包括 \<組件名稱>.runtimeconfig.json、\<組件名稱>.xm*l* (XML 文件註解) 和 *組件名稱>.deps.json\<* 等子資料夾。 當 *web.config* 檔案存在且設定網站時，IIS 會禁止使用這些機密檔案。 **因此，請注意避免意外重新命名或從部署移除 *web.config* 檔案。**

## <a name="create-the-iis-website"></a>建立 IIS 網站

1. 在目標 IIS 系統上建立資料夾，以包含應用程式的已發佈資料夾和檔案；相關說明請參閱[目錄結構](xref:host-and-deploy/directory-structure)。

2. 在資料夾內建立 *logs* 資料夾，以在啟用 stdout 記錄時保留 stdout 記錄。 如果應用程式已在承載中部署 *logs* 資料夾，請略過此步驟。 如需如何讓 MSBuild 建立 *logs* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。

3. 在 **IIS 管理員**中建立新的網站。 提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。 提供**繫結**組態並建立網站。

4. 將應用程式集區設為 [沒有 Managed 程式碼]。 ASP.NET Core 會在不同的處理序中執行，並管理執行階段。

5. 開啟 [新增網站]視窗。

   ![選取 [網站] 操作功能表的 [新增網站]。](index/_static/add-website-context-menu-ws2016.png)

6. 設定網站。

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

7. 在 [應用程式集區] 面板中，以滑鼠右鍵按一下網站的應用程式集區，並從快顯功能表中選取 [基本設定...]，開啟 [編輯應用程式集區] 窗格。

   ![選取 [應用程式集區] 操作功能表的 [基本設定]。](index/_static/apppools-basic-settings-ws2016.png)

8. 將 **.NET CLR 版本**設為 [沒有 Managed 程式碼]。

   ![將 .NET CLR 版本設為 [沒有 Managed 程式碼]。](index/_static/edit-apppool-ws2016.png)
     
    注意：將 **.NET CLR 版本**設為 [沒有 Managed 程式碼] 是選擇性的。 ASP.NET Core 不依賴載入桌面 CLR。

9. 確認處理序模型身分識別具有適當的權限。

   如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。
   
## <a name="deploy-the-app"></a>部署應用程式

將應用程式部署至您在目標 IIS 系統上建立的資料夾。 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制。

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 無法覆寫鎖定的檔案。 若要釋放部署內的鎖定檔案，請停止應用程式集區：

* 伺服器上的 IIS 管理員手動方式。
* 使用 Web Deploy 和參考專案檔中的 `Microsoft.NET.Sdk.Web`。 *app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。 當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。 如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#appofflinehtm)。
* 使用 PowerShell 停止和重新啟動應用程式集區 (需要 PowerShell 5 或更新版本)：
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>使用 Visual Studio 的 Web Deploy

請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。 如果裝載提供者提供或支援建立發行設定檔，請下載並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔。

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Visual Studio 外部的 Web Deploy

透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。 如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。

### <a name="alternatives-to-web-deploy"></a>Web Deploy 的替代項目

有多種方法可將應用程式移到 Xcopy、Robocopy 或 PowerShell 等主機系統。 Visual Studio 使用者可以使用[發佈範例](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)。

## <a name="browse-the-website"></a>瀏覽網站

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="data-protection"></a>資料保護

有數個 ASP.NET 中介軟體使用資料保護，包括在驗證中使用的中介軟體。 即使不從使用者程式碼呼叫資料保護 API (DPAPI) ，仍應使用部署指令碼設定資料保護，或以使用者程式碼建立持續性的金鑰存放區。 如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。

如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：

* 所有表單驗證權杖都會失效。 
* 當使用者提出下一個要求時，需要再次登入。 
* 所有以 keyring 保護的資料都無法再解密。

若要在 IIS 下設定資料保護，請使用下列方法**之一**：

### <a name="create-a-data-protection-registry-hive"></a>建立資料保護登錄 Hive

ASP.NET 應用程式使用的資料保護金鑰會儲存在應用程式外部的登錄 Hive 中。 若要保存指定應用程式的金鑰，請為應用程式集區建立登錄 Hive。

若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。 此指令碼會在 HKLM 登錄中建立特殊的登錄機碼，其只會將背景工作處理序帳戶列入 ACL。 在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。

在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。 根據預設，資料保護金鑰不予加密。 請確保這類共用的檔案權限僅限於執行應用程式的 Windows 帳戶身分。 此外，可以使用 X509 憑證保護待用的金鑰。 請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。 詳細資訊請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>設定 IIS 應用程式集區載入使用者設定檔

此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。 將 [載入使用者設定檔] 設為 `True`。 這會將金鑰儲存在使用者設定檔目錄下，並使用 DPAPI 與應用程式集區所用的特定使用者帳戶金鑰加以保護。

### <a name="use-the-file-system-as-a-key-ring-store"></a>將檔案系統當作 Keyring 存放區使用

調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。 使用 X509 憑證保護 Keyring，確保憑證是受信任的憑證。 如果它是自我簽署的憑證，請將憑證放在受信任的根存放區。

在 Web 伺服陣列中使用 IIS 時：

* 請使用所有電腦都可以存取的檔案共用。
* 將 X509 憑證部署到每一部電腦。 設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。

### <a name="set-a-machine-wide-policy-for-data-protection"></a>設定資料保護的全電腦原則

針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。 如需詳細資訊，請參閱[資料保護](xref:security/data-protection/index)文件。

## <a name="configuration-of-sub-applications"></a>子應用程式的組態

根應用程式下新增的子應用程式不應包含當作處理常式的 ASP.NET Core 模組。 如果您在子應用程式的 *web.config* 檔案中，將模組新增為處理常式，則當您嘗試瀏覽子應用程式時，會收到參考錯誤組態檔的 500.19 (內部伺服器錯誤)。 下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案內容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

如需設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。

## <a name="configuration-of-iis-with-webconfig"></a>使用 web.config 的 IIS 組態

IIS 組態受到這些適用於反向 Proxy 組態之 IIS 功能 *web.config* 的 **\<system.webServer>** 區段所影響。 如果在系統層級設定 IIS 使用動態壓縮，則在應用程式的 *web.config* 檔案中有 **\<urlCompression>** 項目的應用程式，可停用該項設定。 如需詳細資訊，請參閱 [\<system.webServer> 的組態參考](https://docs.microsoft.com/iis/configuration/system.webServer/)、[ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)以及[使用 IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)。 如果需要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (IIS 10.0+ 支援)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。

## <a name="configuration-sections-of-webconfig"></a>web.config 的組態區段

ASP.NET Core 應用程式的組態不使用 *web.config* 中的 ASP.NET Framework 應用程式組態區段：

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<位置>**

使用其他組態提供者設定的 ASP.NET Core 應用程式。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。

## <a name="application-pools"></a>應用程式集區

在單一系統上裝載多個網站時，請在其各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。 IIS [新增網站] 對話方塊預設成此組態。 當您提供**網站名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。 新增網站時，會使用該網站名稱建立新的應用程式集區。

## <a name="application-pool-identity"></a>應用程式集區身分識別

應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。 在 IIS 8.0+ 中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。 在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。 使用這個身分識別時，即可保護資源；不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。

如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：

1. 開啟 Windows 檔案總管，巡覽至目錄。

2. 以滑鼠右鍵按一下目錄並選取 [屬性]。

3. 依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。

4. 選取 [位置] 按鈕，並確定選取系統。

5. 在 [輸入要選取的物件名稱] 文字方塊中輸入 **IIS AppPool\DefaultAppPool**。

   ![選取應用程式資料夾的 [使用者或群組] 對話方塊](index/_static/select-users-or-groups-1.png)

6. 選取 [檢查名稱] 按鈕。 選取 [確定]。

   ![選取應用程式資料夾的 [使用者或群組] 對話方塊](index/_static/select-users-or-groups-2.png)

也可使用 **ICACLS** 工具透過命令提示字元完成：

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>其他資源

* [針對 IIS 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/iis/troubleshoot)
* [Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
* [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
* [使用 IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)
* [ASP.NET Core 簡介](../index.md)
* [Microsoft IIS 官方網站](https://www.iis.net/)
* [Microsoft TechNet Library：Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
