---
title: "在使用 IIS 的 Windows 上裝載 ASP.NET Core"
author: guardrex
description: "ASP.NET Core 應用程式的 Windows Server Internet Information Services (IIS) 組態及部署。"
keywords: "ASP.NET Core, internet information services, iis, windows server, 裝載套件組合, asp.net core 模組, web deploy"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: f506fc880e50aa2fdc772fd29b905dfad02cda2b
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-windows-with-iis-and-deploy-to-it"></a>在 Windows 上使用 IIS 設定並部署到 ASP.NET Core 裝載環境

作者：[Luke Latham](https://github.com/guardrex) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>支援的作業系統

支援下列作業系統：

* Windows 7 和更新版本

* Windows Server 2008 R2 和更新版本&#8224;

&#8224;就概念而言，本文件中描述的 IIS 組態也適用於在 Nano Server IIS 上裝載 ASP.NET Core 應用程式，但特定指示需參考 [Nano Server 上的 IIS 與 ASP.NET Core](xref:tutorials/nano-server)。

[WebListener 伺服器](xref:fundamentals/servers/weblistener)在使用 IIS 的反向 Proxy 組態中無法運作。 您必須使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。

## <a name="iis-configuration"></a>IIS 組態

啟用**網頁伺服器 (IIS)**角色，並建立角色服務。

### <a name="windows-desktop-operating-systems"></a>Windows 桌面作業系統

巡覽至 [控制台] > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (螢幕左側)。 開啟 [Internet Information Services] 和 [Web 管理工具] 群組。 核取 [IIS 管理主控台] 方塊。 [World Wide Web Services] (全球資訊網服務) 核取方塊。 接受**全球資訊網服務**的預設功能，或自訂符合需求的 IIS 功能。

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Windows Server 作業系統

伺服器作業系統，請透過 [伺服器管理員] 中的 [管理] 功能表或連結，使用 [新增角色及功能精靈]。 在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。

![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](iis/_static/server-roles-ws2016.png)

在**角色服務**步驟中，選取您想要的 IIS 角色服務或接受提供的預設角色服務。

![在選取角色服務步驟中，選取預設的角色服務。](iis/_static/role-services-ws2016.png)

透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。 安裝網頁伺服器 (IIS) 角色之後，不需要重新啟動伺服器/IIS。

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>安裝 .NET Core Windows Server 裝載套件組合

1. 在主控系統上安裝 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore.2.0.0-windowshosting)。 套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。 此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。 注意：如果系統沒有網際網路連線，請先取得並安裝 *[Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)*，再安裝 .NET Core Windows Server 裝載套件組合。

2. 重新啟動系統或從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，挑選系統 PATH 的變更。

> [!NOTE]
> 如果使用 IIS 共用組態，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>使用 Visual Studio 發佈時安裝 Web Deploy

如果您想要在 Visual Studio 中使用 Web Deploy 部署應用程式，請在主控系統上安裝最新版的 Web Deploy。 若要安裝 Web Deploy，您可以使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從[Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。 慣用的方法是使用 WebPI。 WebPI 提供獨立的安裝程式和組態以裝載提供者。

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

在應用程式相依性的 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件中包含相依性。 在 *WebHostBuilder* 新增 *UseIISIntegration* 擴充方法，將 IIS 整合中介程式併入應用程式中：

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

`UseKestrel` 與 `UseIISIntegration` 都是必要項目。 呼叫 *UseIISIntegration* 的程式碼並不會影響程式碼的可攜性。 若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會生效。

---

如需裝載的詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。

### <a name="iis-options"></a>IIS 選項

若要設定 *IISIntegration* 服務選項，*ConfigureServices* 中請包括 *IISOptions* 的服務組態：

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

*web.config* 檔案會設定 ASP.NET Core 模組，並提供其他 IIS 組態。 `Microsoft.NET.Sdk.Web` 處理 *web.config* 的建立、轉換及發佈，這都包含在當您將專案的 SDK 設定在 *.csproj* 檔案 `<Project Sdk="Microsoft.NET.Sdk.Web">` 的上方時。 為防止 MSBuild 目標轉換您的 *web.config* 檔案，請將 **\<IsTransformWebConfigDisabled>** 屬性新增至設定為 `true` 的專案檔：

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

如果專案中沒有 *web.config* 檔案，當您使用 *dotnet publish* 或 Visual Studio publish 發佈時，就會為您在發佈的輸出中建立檔案。 如果專案中有該檔案，它會使用正確的 *processPath* 和「引數」轉換以設定 ASP.NET Core 模組，並移至已發佈的輸出。 轉換不會觸碰檔案包含的 IIS 組態設定。

## <a name="create-the-iis-website"></a>建立 IIS 網站

1. 在目標 IIS 系統上建立資料夾，以包含應用程式的已發佈資料夾和檔案，說明請參閱[目錄結構](xref:hosting/directory-structure)。

2. 在您建立的資料夾內，建立 *logs* 資料夾來保存應用程式記錄檔 (如果您打算啟用記錄)。 如果您打算以將 *logs* 資料夾放在承載中的方式來部署您的應用程式，您可以略過此步驟。

3. 在 **IIS 管理員**中建立新的網站。 提供**網站名稱**並將**實體路徑**設定為您建立的應用程式部署資料夾。 提供**繫結**組態並建立網站。

4. 將應用程式集區設為 [沒有 Managed 程式碼]。 ASP.NET Core 會在不同的處理序中執行，並管理執行階段。

5. 開啟 [新增網站]視窗。

   ![按一下 [網站] 操作功能表的 [新增網站]。](iis/_static/add-website-context-menu-ws2016.png)

6. 設定網站。

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](iis/_static/add-website-ws2016.png)

7. 在 [應用程式集區] 面板中，以滑鼠右鍵按一下網站的應用程式集區，並從快顯功能表中選取 [基本設定...]，開啟 [編輯應用程式集區] 視窗。

   ![選取 [應用程式集區] 操作功能表的 [基本設定]。](iis/_static/apppools-basic-settings-ws2016.png)

8. 將 **.NET CLR 版本**設為 [沒有 Managed 程式碼]。

   ![將 .NET CLR 版本設為 [沒有 Managed 程式碼]。](iis/_static/edit-apppool-ws2016.png)
     
    注意：將 **.NET CLR 版本**設為 [沒有 Managed 程式碼] 是選擇性的。 ASP.NET Core 不依賴載入桌面 CLR。

9. 確認處理序模型身分識別具有適當的權限。

    如果您將應用程式集區的預設身分識別從 **ApplicationPoolIdentity** 變更為其他身分識別 ([處理序模型] > [身分識別])，確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。
   
## <a name="deploy-the-application"></a>部署應用程式
將應用程式部署至您在目標 IIS 系統上建立的資料夾。 Web Deploy 是建議的部署機制。 Web Deploy 的替代項目列示如下。

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 因為無法複製鎖定的檔案，所以無法執行部署。

### <a name="web-deploy-with-visual-studio"></a>使用 Visual Studio 的 Web Deploy
請參閱[建立 Visual Studio 和 MSBuild 的發行設定檔，以部署 ASP.NET Core 應用程式](xref:publishing/web-publishing-vs#publish-profiles)主題，了解如何建立發行設定檔供 Web Deploy 使用。 如果您的裝載提供者提供或支援建立發行設定檔，請下載並使用 Visual Studio 的 [發佈] 對話方塊匯入其設定檔。

![[發佈] 對話方塊頁](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Visual Studio 外部的 Web Deploy
您也可以從命令列使用 Visual Studio 外部的 Web Deploy。 如需詳細資訊，請參閱 [Web 部署工具](https://docs.microsoft.com/iis/publish/using-web-deploy/use-the-web-deployment-tool)。

### <a name="alternatives-to-web-deploy"></a>Web Deploy 的替代項目
如果您不想使用 Web Deploy 或不是使用 Visual Studio，您可以使用 Xcopy、Robocopy 或 PowerShell 等任何一種方法，將應用程式移至主控系統。 Visual Studio 使用者可以使用[發佈範例](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)。

## <a name="browse-the-website"></a>瀏覽網站
![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](iis/_static/browsewebsite.png)
   
>[!WARNING]
> .NET Core 應用程式是透過 IIS 和 Kestrel 伺服器之間的反向 Proxy 裝載。 為了建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)，這是提供給 IIS 的網站實體路徑。 機密檔案位於應用程式的實體路徑，包括 *my_application.runtimeconfig.json*、*my_application.xml* (XML 文件註解) 和 *my_application.deps.json* 等子資料夾。 需要有 *web.config* 檔案才能建立 Kestrel 的反向 Proxy，防止 IIS 提供這些和其他機密檔案。 **因此，絕對不要意外重新命名或從部署移除 *web.config* 檔案，這一點十分重要。**

## <a name="data-protection"></a>資料保護

下列情況下，ASP.NET Core 應用程式會將 Keyring 儲存在記憶體中：

* 網站裝載在 IIS 後端。
* 尚未設定資料保護堆疊將 Keyring 儲存在持續性存放區中。

如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：

* 所有表單驗證權杖都將無效。 
* 下一次要求時使用者需要再次登入。 
* 使用 Keyring 保護的所有資料都會受到保護。

> [!WARNING]
> 有數個 ASP.NET 中介軟體使用資料保護，包括在驗證中使用的中介軟體。 即使不特別從自己的程式碼呼叫任何資料保護 API，您也應該使用部署指令碼或在您自己的程式碼中設定資料保護。 如果您不設定資料保護，索引鍵預設會保留在記憶體中，並於應用程式重新啟動時捨棄。 重新啟動會讓 Cookie 驗證寫入的所有 Cookie 失效，使用者必須再次登入。

若要在 IIS 下設定資料保護，您必須使用下列方法之一：

* 執行 [powershell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) 建立適當的登錄項目 (例如，`.\Provision-AutoGenKeys.ps1 DefaultAppPool`)。 這會將金鑰存放在登錄中，使用具有全電腦金鑰的 DPAPI 保護。
* 設定 IIS 應用程式集區載入使用者設定檔。 此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。 將 [載入使用者設定檔] 設為 `True`。 這會將索引鍵儲存在使用者設定檔目錄下，使用具有應用程式集區使用的使用者帳戶特定金鑰的 DPAPI 保護。
* 調整應用程式程式碼[將檔案系統用為 Keyring 存放區](xref:security/data-protection/configuration/overview)。 使用 X509 憑證保護 Keyring，確保它是受信任的憑證。 例如，如果它是自我簽署的憑證，就必須放在受信任的根存放區。

在 Web 伺服陣列中使用 IIS 時：

* 使用所有電腦都可以存取的檔案共用。
* 將 X509 憑證部署到每一部電腦。  設定[程式碼中的資料保護](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview)。

### <a name="1-create-a-data-protection-registry-hive"></a>1.建立資料保護登錄 Hive

ASP.NET 應用程式使用的資料保護金鑰會儲存在應用程式外部的登錄 Hive 中。 若要保存指定應用程式的金鑰，您必須建立應用程式的應用程式集區登錄 Hive。

若為獨立的 IIS 安裝，您可以針對和 ASP.NET Core 應用程式一起使用的每個應用程式集區使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。 此指令碼會在 HKLM 登錄中建立特殊的登錄機碼，HKLM 登錄只有碰到背景工作處理序帳戶才會是 ACL。 在待用期間使用 DPAPI 加密金鑰。

在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。 根據預設，資料保護金鑰不予加密。 您應該確保這類共用的檔案權限僅限於執行應用程式的 Windows 帳戶身分。 此外，您可以選擇使用 X509 憑證保護待用的金鑰。 您可能要考慮這樣的機制：讓使用者上傳憑證、將它們放入使用者的受信任的憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用它們。 詳細資訊請參閱[設定資料保護](xref:security/data-protection/configuration/overview#data-protection-configuring)。

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2.設定 IIS 應用程式集區載入使用者設定檔
此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。 將 [載入使用者設定檔] 設為 True。 這會將索引鍵儲存在使用者設定檔目錄下，使用具有應用程式集區使用的使用者帳戶特定金鑰的 DPAPI 保護。

### <a name="3-machine-wide-policy-for-data-protection"></a>3.資料保護的全電腦原則

資料保護系統對針對取用資料保護 API 的所有應用程式設定預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy)的支援有限。 如需詳細資訊，請參閱[資料保護](xref:security/data-protection/index)文件。

## <a name="configuration-of-sub-applications"></a>子應用程式的組態

根應用程式下新增的子應用程式不應包含 ASP.NET Core 模組當做處理常式。 如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，當您嘗試瀏覽子應用程式時，您會收到參考錯誤組態檔的 500.19 (內部伺服器錯誤)。 下例顯示已發佈之 ASP.NET Core 子應用程式的 *web.config* 檔案內容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

如果您打算在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式，您必須明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

如需使用 *web.config* 檔案設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)。

## <a name="configuration-of-iis-with-webconfig"></a>使用 web.config 的 IIS 組態

IIS 組態仍然會受到這些 IIS 功能 *web.config* 的 `<system.webServer>` 區段所影響，這些 IIS 功能適用於反向 Proxy 組態。 例如，您可能在系統層級設定 IIS 使用動態壓縮，但可使用應用程式之 *web.config* 檔案中的 `<urlCompression>` 元素，停用應用程式的該項設定。 如需詳細資訊，請參閱 [`<system.webServer>` 的組態參考](https://docs.microsoft.com/iis/configuration/system.webServer/)、[ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)，和[使用 IIS 模組與 ASP.NET Core](xref:hosting/iis-modules)。 如果您需要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (IIS 10.0+ 支援)，請參閱 IIS 參考文件之[環境變數\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。

## <a name="configuration-sections-of-webconfig"></a>web.config 的組態區段

有別於使用 *web.config* 中的 `<system.web>`、`<appSettings>`、`<connectionStrings>` 和 `<location>` 元素所設定的 .NET Framework 應用程式，ASP.NET Core 應用程式設定使用其他組態提供者。 如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。

## <a name="application-pools"></a>應用程式集區

當多個網站裝載在單一系統上時，您應該在其各自的應用程式集區中執行每個應用程式，以隔開所有的應用程式。 IIS [新增網站] 對話方塊預設執行此行為。 當您提供**站台名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。 當您新增網站時，即使用該站台名稱建立新的應用程式集區。

## <a name="application-pool-identity"></a>應用程式集區身分識別

應用程式集區身分識別帳戶可讓您在唯一的帳戶下執行應用程式，不必建立及管理網域或本機帳戶。 在 IIS 8.0+ 上，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，預設在此帳戶下執行應用程式集區的背景工作處理序。 在 IIS 管理主控台中，於您的應用程式集區 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**，如下圖所示。

![應用程式集區進階設定對話方塊](iis/_static/apppool-identity.png)

IIS 管理程序會在 Windows 安全系統中以應用程式集區的名稱建立安全識別碼。 使用這個身分識別可以保護資源，但此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。

如果您需要將提升的 IIS 背景工作處理序存取權授與您的應用程式，就需要修改包含您應用程式的目錄存取控制清單 (ACL)。

1. 開啟 Windows 檔案總管，巡覽至目錄。

2. 以滑鼠右鍵按一下目錄，然後按一下 [內容]。

3. 依序按一下 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。

4. 按一下 [位置] 按鈕，並確定選取您的系統。

5. 在 [輸入要選取的物件名稱] 文字方塊中輸入 **IIS AppPool\DefaultAppPool**。

  ![選取應用程式資料夾的 [使用者或群組] 對話方塊](iis/_static/select-users-or-groups-1.png)

6. 按一下 [檢查名稱] 按鈕，再按一下 [確定]。

  ![選取應用程式資料夾的 [使用者或群組] 對話方塊](iis/_static/select-users-or-groups-2.png)

您也可以透過命令提示字元使用 **ICACLS** 工具執行此作業：

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>疑難排解提示

若要診斷 IIS 部署的問題，請研究瀏覽器輸出、透過 [事件檢視器] 檢查系統的**應用程式**記錄檔，並啟用 `stdout` 記錄。 在 *web.config* 的 `<aspNetCore>` 元素的 *stdoutLogFile* 屬性提供的路徑上，會找到 **ASP.NET Core 模組**記錄。部署中必須有屬性值提供之路徑上的所有資料夾。 您也必須設定 *stdoutLogEnabled ="true"*。 使用 `Microsoft.NET.Sdk.Web` SDK 建立 *web.config* 檔案的應用程式，其 *stdoutLogEnabled* 設定預設值為 *false*，因此您必須手動提供 *web.config* 檔案或修改檔案，以啟用 `stdout` 記錄。

要等到模組 *startupTimeLimit* (預設值：120 秒) 和 *startupRetryCount* (預設值：2) 通過後，數個常見錯誤才會出現在瀏覽器、應用程式記錄檔和 ASP.NET Core 模組記錄檔中。 因此，要等候整整六分鐘，才能推算出模組無法啟動應用程式的程序。

有一個快速方法可判斷應用程式是否正常運作，即直接在 Kestrel 上執行應用程式。 應用程式如已發佈為與 Framework 相依的部署，請執行部署資料夾中的 **dotnet my_application.dll**，這是應用程式的 IIS 實體路徑。 如果應用程式已發佈為獨立部署，請直接從命令提示字元執行部署資料夾中的應用程式可執行檔 **my_application.exe**。 如果 Kestrel 接聽預設通訊埠 5000，您應該能夠瀏覽在 `http://localhost:5000/` 的應用程式。 如果應用程式通常在 Kestrel 端點位址回應，IIS-ASP.NET Core 模組-Kestrel 組態出問題的機率比較大，應用程式本身出問題的機率比較小。

判定 Kestrel 伺服器的 IIS 反向 Proxy 是否正常運作的一種方式，是使用[靜態檔案中介軟體](xref:fundamentals/static-files)針對樣式表、指令碼或來自 *wwwroot* 應用程式靜態檔案的影像，執行簡單的靜態檔案要求。 如果應用程式可以提供靜態檔案，但 MVC 檢視和其他端點卻失敗，就不太可能是 IIS-ASP.NET Core 模組-Kestrel 組態出問題，更可能是應用程式本身出問題 (例如，MVC 路由或 500 內部伺服器錯誤)。

當 Kestrel 在 IIS 後端正常啟動，但應用程式在本機上成功執行之後卻不能在系統上執行時，您可以暫時將環境變數新增至 *web.config*，將 `ASPNETCORE_ENVIRONMENT` 設成 `Development`。 只要您不覆寫應用程式啟動的環境，應用程式在系統上執行時，[開發人員例外狀況頁面](xref:fundamentals/error-handling)就會出現。 只有不公開到網際網路的臨時/測試系統，才建議以這種方式設定 `ASPNETCORE_ENVIRONMENT` 的環境變數。 完成後，請務必從 *web.config* 檔案移除環境變數。 如需透過反向 Proxy 的 *web.config* 設定環境變數的相關資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:hosting/aspnet-core-module#setting-environment-variables)。

在大部分情況下，啟用應用程式記錄會協助疑難排解應用程式或反向 Proxy 的問題。 如需詳細資訊，請參閱[記錄](xref:fundamentals/logging)。

升級開發電腦的 .NET Core SDK 或應用程式內的套件版本後，最後有關應用程式的疑難排解提示無法執行。 在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。 這些問題大部分是可以修正的，方法是刪除專案中的 `bin` 和 `obj` 資料夾、清除 `%UserProfile%\.nuget\packages\` 和 `%LocalAppData%\Nuget\v3-cache` 的套件快取、還原專案，然後確認已完全刪除之前在系統上的部署，再重新部署應用程式。

>[!TIP]
> 有一個便利的方式可以清除套件快取，即從 [NuGet.org](https://www.nuget.org/) 取得 `NuGet.exe` 工具，將它新增系統的 PATH，再從命令提示字元執行 `nuget locals all -clear`。

## <a name="common-errors"></a>常見的錯誤

以下是不完整的錯誤清單。 假如遇到此處未列出的錯誤，請在下方的 [註解] 區段中留下詳細的錯誤訊息。

### <a name="installer-unable-to-obtain-vc-redistributable"></a>安裝程式無法取得 VC++ 可轉散發套件

* **安裝程式的例外狀況：**0x80072efd 或 0x80072f76 - 未指定的錯誤

* **安裝程式記錄例外狀況&#8224;：**錯誤 0x80072efd 或 0x80072f76：無法執行 EXE 套件

  &#8224;記錄位於 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。

疑難排解：

* 如果安裝伺服器裝載套件組合時，系統不能存取網際網路，當安裝程式受阻無法取得 *Microsoft Visual C++ 2015 可轉散發套件*時，就會發生這個例外狀況。 您可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。 如果安裝程式失敗，您可能不會收到裝載與 Framework 相依部署的必要 .NET Core 執行階段。 如果您打算裝載與 Framework 相依的部署，請確認已在 [程式和功能] 中安裝執行階段。 您可從 [.NET 下載](https://www.microsoft.com/net/download/core)取得執行階段安裝程式。 安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>作業系統升級已移除 32 位元的 ASP.NET Core 模組

* **應用程式記錄檔：**無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。 資料即錯誤。

疑難排解：

* 作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。 如果先安裝 ASP.NET Core 模組再升級作業系統，然後嘗試在作業系統升級之後在 32 位元模式中執行任何 AppPool，就會發生這個問題。 作業系統升級之後，請修復 ASP.NET Core 模組。 請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。 執行安裝程式時，請選取 [修復]。

### <a name="platform-conflicts-with-rid"></a>平台發生 RID 衝突

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法使用命令行 '"C:\\{PATH}\my_application.{exe|dll}" ' 啟動程序，ErrorCode = '0x80004005 : ff。

* **ASP.NET Core 模組記錄檔：**未處理的例外狀況：System.BadImageFormatException：無法載入檔案或組件 'my_application.dll'。 嘗試載入了格式不正確的程式。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。

* 確認您並未在 *.csproj* 中設定與 RID 發生衝突的 `<PlatformTarget>`。 例如，藉由使用 *dotnet publish -c Release -r win10-x64*，或將 *.csproj* 的 `<RuntimeIdentifiers>` 設定為 `win10-x64`，不指定 `x86` 的 `<PlatformTarget>` 且以 `win10-x64` 的 RID 發佈。 專案發佈且沒有警告或錯誤，但會失敗，因為上述記錄在系統上的例外狀況。

* 如果在升級應用程式和部署新的組件時， Azure 應用程式部署發生這個例外狀況，請手動刪除先前部署的所有檔案。 部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。

### <a name="uri-endpoint-wrong-or-stopped-website"></a>URI 端點錯誤或停止了網站

* **瀏覽器：**ERR_CONNECTION_REFUSED

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解：

* 確認您為應用程式使用的是正確的 URI 端點。 請檢查您的繫結。

* 確認 IIS 網站不是「已停止」狀態。

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>已停用 CoreWebEngine 或 W3SVC 伺服器功能

* **作業系統例外狀況：**必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。

疑難排解：

* 確認您已啟用適當的角色和功能。 請參閱 [IIS 組態](#iis-configuration)。

### <a name="incorrect-website-physical-path-or-application-missing"></a>不正確的網站實體路徑或遺失應用程式

* **瀏覽器：** 403 禁止 - 存取被拒**--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解：

* 請檢查 IIS 網站的 [基本設定] 和實體的應用程式資料夾。 確認應用程式位於 IIS 網站**實體路徑**的資料夾中。

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>角色不正確、模組未安裝，或權限不正確。

* **瀏覽器：**500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解：

* 確認您已啟用適當的角色。 請參閱 [IIS 組態](#iis-configuration)。

* 請檢查 [程式和功能] 並確認 **Microsoft ASP.NET Core 模組** 已安裝。 如果已安裝的程式清單中沒有 **Microsoft ASP.NET Core 模組**，請安裝此模組。 請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。

* 請確定 [應用程式集區] > [處理序模型] > [身分識別] 設為 [ApplicationPoolIdentity]，或您的自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>不正確的 processPath, 遺失 PATH 變數, 未安裝裝載套件組合, 未重新啟動系統/IIS, 未安裝 VC++ 可轉散發套件, 或 dotnet.exe 存取違規

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '".\my_application.exe" ' 啟動程序，ErrorCode = '0x80070002 : 0。

* **ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。

* 請檢查 *web.config* 中 `<aspNetCore>` 元素的 *processPath* 屬性，以確認它是與 Framework 相依部署的 *dotnet* 或獨立部署的 *.\my_application.exe*。

* 若為與 Framework 相依的部署，可能無法透過 PATH 設定存取 *dotnet.exe*。 確認系統 PATH 設定中有 *C:\Program Files\dotnet\*。

* 若為與 Framework 相依的部署，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。 確認 AppPool 使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。 確認 *C:\Program Files\dotnet* 和應用程式目錄上的 AppPool 使用者身分識別未設定任何拒絕規則。

* 您可能已部署與 Framework 相依的部署，並在不重新啟動 IIS 的狀況下安裝了 .NET Core。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。

* 您可能已部署與 Framework 相依的部署，但未在主控系統上安裝 .NET Core 執行階段。 如果您要嘗試部署與 Framework 相依的部署，但尚未安裝 .NET Core 執行階段，請在系統上執行 **.NET Core Windows Server 裝載套件組合安裝程式**。 請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。 如果您要嘗試在沒有網際網路連線的系統上安裝 .NET Core 執行階段，請從 [.NET 下載](https://www.microsoft.com/net/download/core)取得執行階段，執行裝載套件組合安裝程式以安裝 ASP.NET Core 模組。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。

* 您可能已部署與 Framework 相依的部署，並在不重新啟動系統/IIS 的情況下安裝 .NET Core。 從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。

* 您可能已部署與 Framework 相依的部署，但系統上未安裝 *Microsoft Visual C++ 2015 可轉散發套件 (x64)*。 您可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。

### <a name="incorrect-arguments-of-aspnetcore-element"></a>不正確的 \<aspNetCore\> 元素引數

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '"dotnet" .\my_application.dll' 啟動程序，ErrorCode = '0x80004005 : 80008081。

* **ASP.NET Core 模組記錄檔：**要執行的應用程式不存在：'PATH\my_application.dll'

疑難排解：

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。

* 檢查 *web.config* 中 `<aspNetCore>` 元素的 *arguments* 屬性，以確認它是 (a) 與 Framework 相依部署的 *.\my_application.dll*；或 (b) 不存在的空字串 (*arguments=""*)，或獨立部署的應用程式引數清單 (*arguments="arg1, arg2, ..."*)。

### <a name="missing-net-framework-version"></a>遺失 .NET Framework 版本

* **瀏覽器：** 502.3 不正確的閘道 - 嘗試路由要求時發生連線錯誤。

* **應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '"dotnet" .\my_application.dll' 啟動程序，ErrorCode = '0x80004005 : 80008081。

* **ASP.NET Core 模組記錄檔：**遺失方法、檔案或組件例外狀況。 例外狀況中指定的方法、檔案或組件是 .NET Framework 方法、檔案或組件。

疑難排解：

* 安裝系統中遺失的 .NET Framework 版本。

* 若為與 Framework 相依的部署，請確認已在系統上正確安裝執行階段。 例如，如果您將專案從 1.0 升級為 1.1、部署到主控系統，然後收到這個例外狀況，請確定主控系統上安裝了 1.1 架構。

### <a name="stopped-application-pool"></a>已停止應用程式集區

* **瀏覽器：**503 服務無法使用

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**未建立記錄檔

疑難排解

* 確認應用程式集區不是「已停止」狀態。

### <a name="iis-integration-middleware-not-implemented"></a>未實作 IIS Integration 中介軟體

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION'，使用命令列 '"C:\\{PATH}\my_application.{exe|dll}" ' 建立了程序，但可能當機、或沒有回應、或未接聽指定的連接埠 '{PORT}'，ErrorCode = '0x800705b4'。

* **ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示一般作業。

疑難排解

* 確認應用程式在 Kestrel 本機上執行。 處理序失敗，可能是因為應用程式發生問題。 如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。

* 在應用程式的 *WebHostBuilder()* 呼叫 *.UseIISIntegration()* 方法，確認您已正確參考 IIS Integration 中介軟體。

### <a name="sub-application-includes-a-handlers-section"></a>子應用程式包含\<處理常式\>區段

* **瀏覽器：**HTTP 錯誤 500.19 - 內部伺服器錯誤

* **應用程式記錄檔：**無項目

* **ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示根應用程式的一般作業。 未建立子應用程式的記錄檔。

疑難排解

* 請確認子應用程式的 *web.config* 檔案不包含 `<handlers>`區段。

### <a name="application-configuration-general-issue"></a>應用程式組態一般問題

* **瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗

* **應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION'，使用命令列 '"C:\\{PATH}\my_application.{exe|dll}" ' 建立了程序，但可能當機、或沒有回應、或未接聽指定的連接埠 '{PORT}'，ErrorCode = '0x800705b4'。

* **ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。

疑難排解

* 這個一般的例外狀況指出程序無法啟動，最可能的原因是應用程式組態問題。 參考[目錄結構](xref:hosting/directory-structure)，確認應用程式部署的檔案和資料夾都是合適的，且應用程式組態檔都存在，並包含應用程式和環境的正確設定。 如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。

## <a name="resources"></a>資源

* [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)

* [ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)

* [使用 IIS 模組與 ASP.NET Core](xref:hosting/iis-modules)

* [ASP.NET Core 簡介](../index.md)

* [Microsoft IIS 官方網站](https://www.iis.net/)

* [Microsoft TechNet Library：Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
