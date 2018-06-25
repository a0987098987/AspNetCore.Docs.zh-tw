---
title: ASP.NET Core 模組設定參考
author: guardrex
description: 了解如何設定 ASP.NET Core 模組以裝載 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 8d4283c61163a586557135fddfb85440251aaf29
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275615"
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET Core 模組設定參考

作者：[Luke Latham](https://github.com/guardrex)[Rick Anderson](https://twitter.com/RickAndMSFT)及 [Sourabh Shirhatti](https://twitter.com/sshirhatti)

本文說明如何設定 ASP.NET Core 模組來裝載 ASP.NET Core 應用程式。 如需 ASP.NET Core 模組簡介及安裝指示，請參閱 [ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。

## <a name="configuration-with-webconfig"></a>使用 web.config 進行設定

設定 ASP.NET Core 模組時，是使用網站 *web.config* 檔案中 `system.webServer` 節點的 `aspNetCore` 區段來設定。

以下 *web.config* 檔案是針對[架構相依部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)發佈的檔案，會設定 ASP.NET Core 模組來處理網站要求：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

以下 *web.config* 是針對[自封式部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd)發佈的檔案：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

將應用程式部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/) 時，`stdoutLogFile` 路徑會設定為 `\\?\%home%\LogFiles\stdout`。 此路徑會將 stdout 記錄檔儲存至 [LogFiles] 資料夾，這是服務自動建立的位置。

如需子應用程式中 *web.config* 檔案設定相關的重要注意事項，請參閱[子應用程式設定](xref:host-and-deploy/iis/index#sub-application-configuration)。

### <a name="attributes-of-the-aspnetcore-element"></a>aspNetCore 元素的屬性

::: moniker range="<= aspnetcore-2.0"
| 屬性 | 描述 | 預設 |
| --------- | ----------- | :-----: |
| `arguments` | <p>選擇性字串屬性。</p><p>**processPath** 中所指定可執行檔的引數。</p>| |
| `disableStartUpErrorPage` | true 或 false。</p><p>如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</p> | `false` |
| `forwardWindowsAuthToken` | true 或 false。</p><p>如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。 該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</p> | `true` |
| `processPath` | <p>必要的字串屬性。</p><p>啟動接聽 HTTP 要求之處理序的可執行檔路徑。 支援相對路徑。 如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</p> | |
| `rapidFailsPerMinute` | <p>選擇性的整數屬性。</p><p>指定允許 **processPath** 中所指定處理序每分鐘當機的次數。 如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</p> | `10` |
| `requestTimeout` | <p>選擇性的時間範圍屬性。</p><p>針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</p><p>在 ASP.NET Core 2.0 或更舊版本隨附的 ASP.NET Core 模組版本中，只能以整數分鐘為單位來指定 `requestTimeout`，否則會預設為 2 分鐘。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>選擇性的整數屬性。</p><p>當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</p> | `10` |
| `startupTimeLimit` | <p>選擇性的整數屬性。</p><p>針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。 如果超出此時間限制，模組就會終止處理序。 模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</p> | `120` |
| `stdoutLogEnabled` | <p>選擇性的 Boolean 屬性。</p><p>如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</p> | `false` |
| `stdoutLogFile` | <p>選擇性字串屬性。</p><p>指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。 相對路徑是相對於網站的根目錄。 所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。 路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。 使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。 如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| 屬性 | 描述 | 預設 |
| --------- | ----------- | :-----: |
| `arguments` | <p>選擇性字串屬性。</p><p>**processPath** 中所指定可執行檔的引數。</p>| |
| `disableStartUpErrorPage` | true 或 false。</p><p>如果為 true，就會抑制 [502.5 - 處理序失敗] 頁面，而優先顯示 *web.config* 中設定的 502 狀態碼頁面。</p> | `false` |
| `forwardWindowsAuthToken` | true 或 false。</p><p>如果為 true，就會依據要求將權杖以標頭 'MS-ASPNETCORE-WINAUTHTOKEN' 形式轉送至在 %ASPNETCORE_PORT% 進行接聽的子處理序。 該處理序需負責依據要求呼叫此權杖上的 CloseHandle。</p> | `true` |
| `processPath` | <p>必要的字串屬性。</p><p>啟動接聽 HTTP 要求之處理序的可執行檔路徑。 支援相對路徑。 如果路徑的開頭為 `.`，該路徑即被視為網站根目錄的相對路徑。</p> | |
| `rapidFailsPerMinute` | <p>選擇性的整數屬性。</p><p>指定允許 **processPath** 中所指定處理序每分鐘當機的次數。 如果超出此限制，模組就會在該分鐘的剩餘時間內停止啟動處理序。</p> | `10` |
| `requestTimeout` | <p>選擇性的時間範圍屬性。</p><p>針對在 %ASPNETCORE_PORT% 進行接聽的處理序，指定 ASP.NET Core 模組等候回應的持續時間。</p><p>在 ASP.NET Core 2.1 或更新版本隨附的 ASP.NET Core 模組版本中，是以小時、分鐘及秒為單位來指定 `requestTimeout`。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>選擇性的整數屬性。</p><p>當偵測到 *app_offline.htm* 檔案時，模組等候可執行檔正常關閉的持續時間 (以秒為單位)。</p> | `10` |
| `startupTimeLimit` | <p>選擇性的整數屬性。</p><p>針對可執行檔啟動在連接埠進行接聽的處理序，模組等候的持續時間 (以秒為單位)。 如果超出此時間限制，模組就會終止處理序。 模組會在收到新要求時，嘗試重新啟動處理序，然後在後續的連入要求上繼續嘗試重新啟動處理序，除非應用程式在上一次循環的分鐘內無法啟動的次數達到 **rapidFailsPerMinute** 所指定的次數。</p> | `120` |
| `stdoutLogEnabled` | <p>選擇性的 Boolean 屬性。</p><p>如果為 true，就會將 **processPath** 中所指定處理序的 **stdout** 和 **stderr** 重新導向到 **stdoutLogFile** 中所指定的檔案。</p> | `false` |
| `stdoutLogFile` | <p>選擇性字串屬性。</p><p>指定記錄來自 **processPath** 中所指定處理序之 **stdout** 和 **stderr** 的相對或絕對檔案路徑。 相對路徑是相對於網站的根目錄。 所有開頭為 `.` 的路徑都是網站根目錄的相對路徑，而所有其他路徑則視為絕對路徑。 路徑中提供的所有資料夾都必須存在，模組才能建立記錄檔。 使用底線分隔符號，時間戳記、處理序識別碼及副檔名 (*.log*) 會新增至 **stdoutLogFile** 路徑的最後一個區段。 如果提供 `.\logs\stdout` 作為值，在 2018 年 2 月 5 日的 19:41:32 以處理序識別碼 1934 進行儲存時，範例 stdout 記錄檔就會以 *stdout_20180205194132_1934.log* 的形式儲存在 [logs] 資料夾中。</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>設定環境變數

您可以在 `processPath` 屬性中為處理序指定環境變數。 請使用 `environmentVariables` 集合元素的 `environmentVariable` 子元素來指定環境變數。 本節中所設定環境變數的優先順序會高於系統環境變數。

下列範例會設定兩個環境變數。 `ASPNETCORE_ENVIRONMENT` 會將應用程式的環境設定為 `Development`。 開發人員可以在 *web.config* 檔案中暫時設定這個值，以在進行應用程式例外狀況偵錯時，強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入。 `CONFIG_DIR` 是一個使用者定義的環境變數範例，其中開發人員已撰寫程式碼，會在啟動時讀取值來構成用以載入應用程式設定檔的路徑。

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> 請只有在未受信任網路 (例如網際網路) 無法存取的暫存和測試伺服器上，才將 `ASPNETCORE_ENVIRONMENT` 環境變數設定為 `Development`。

## <a name="appofflinehtm"></a>app_offline.htm

如果在應用程式根目錄中偵測到名稱為 *app_offline.htm* 的檔案，ASP.NET Core Module 會嘗試正常關閉應用程式並停止處理連入的要求。 如果在經過 `shutdownTimeLimit` 中所定義的秒數之後，應用程式仍然在執行，ASP.NET Core Module 就會終止執行中的處理序。

當 *app_offline.htm* 檔案存在時，ASP.NET Core 模組會藉由傳回 *app_offline.htm* 檔案的內容來回應要求。 已移除 *app_offline.htm* 檔案時，下一個要求則會啟動應用程式。

## <a name="start-up-error-page"></a>啟動錯誤頁面

如果 ASP.NET Core 模組無法啟動後端處理序，或後端處理序啟動但無法在所設定的連接埠上進行接聽，就會顯示 [502.5 - 處理序失敗] 狀態碼頁面。 若要抑制此頁面並還原至預設的 IIS 502 狀態碼頁面，請使用 `disableStartUpErrorPage` 屬性。 如需有關設定自訂錯誤訊息的詳細資訊，請參閱 [HTTP 錯誤`<httpErrors>`](/iis/configuration/system.webServer/httpErrors/)。

![502.5 處理序失敗狀態碼頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>記錄檔建立和重新導向

如果已設定 `aspNetCore` 元素的 `stdoutLogEnabled` 和 `stdoutLogFile` 屬性，ASP.NET Core 模組就會將 stdout 和 stderr 記錄檔重新導向到磁碟。 `stdoutLogFile` 路徑中的所有資料夾都必須存在，模組才能建立記錄檔。 應用程式集區必須具有記錄檔寫入位置的寫入權限 (請使用 `IIS AppPool\<app_pool_name>` 來提供寫入權限)。

除非發生處理序回收/重新啟動，否則不會輪替記錄檔。 主機服務提供者必須負責限制記錄檔所使用的磁碟空間。

建議只有在進行應用程式啟動問題疑難排解時，才使用 stdout 記錄檔。 請勿將 stdout 記錄檔用來進行一般應用程式記錄。 針對 ASP.NET Core 應用程式中的例行性記錄，請使用會限制記錄檔大小並輪替記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

建立記錄檔時，系統會自動新增時間戳記和副檔名。 記錄檔名稱會藉由將時間戳記、處理序識別碼及副檔名 (*.log*) 以底線分隔並附加至 `stdoutLogFile` 路徑的最後一個區段 (通常是 *stdout*) 來組成。 如果 `stdoutLogFile` 路徑的結尾是 *stdout*，則在 2018 年 2 月 5 日 19:42:32 建立且 PID 為 1934 的應用程式記錄檔檔案名稱會是 *stdout_20180205194132_1934.log*。

下列範例 `aspNetCore` 元素會設定 Azure App Service 中所裝載應用程式的 stdout 記錄。 系統可接受使用本機路徑或網路共用路徑來進行本機記錄。 請確認 AppPool 使用者身分識別具備所提供路徑的寫入權限。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

如需 *web.config* 檔案中 `aspNetCore` 元素的範例，請參閱[使用 web.config 進行設定](#configuration-with-webconfig)。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Proxy 組態使用 HTTP 通訊協定和配對權杖

在 ASP.NET Core 模組與 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。 使用 HTTP 是一項效能最佳化作業，其中模組與 Kestrel 之間的流量會在網路介面外的回送位址進行。 沒有從伺服器外的位置竊聽模組與 Kestrel 之間流量的風險。

配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。 模組會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。 配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。 IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。 如果權杖值不相符，將記錄並拒絕要求。 使用者無法從伺服器外的位置存取配對權杖環境變數，以及模組與 Kestrel 之間的流量。 在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>具有 IIS 共用設定的 ASP.NET Core 模組

ASP.NET Core 模組安裝程式會以 **SYSTEM** 帳戶的權限執行。 由於本機系統帳戶並不具備「IIS 共用設定」所使用共用路徑的修改權限，因此安裝程式在嘗試於共用上的 *applicationHost.config* 中進行模組設定時，會發生存取被拒錯誤。 使用「IIS 共用設定」時，請依照下列步驟進行操作：

1. 停用「IIS 共用設定」。
1. 執行安裝程式。
1. 將已更新的 *applicationHost.config* 檔案匯出到共用。
1. 重新啟用「IIS 共用設定」。

## <a name="module-version-and-hosting-bundle-installer-logs"></a>模組版本和裝載套件組合安裝程式記錄檔

判斷已安裝的 ASP.NET Core 模組版本：

1. 在主控系統上，瀏覽至 *%windir%\System32\inetsrv*。
1. 找出 *aspnetcore.dll* 檔案。
1. 在該檔案上按一下滑鼠右鍵，然後從關聯式功能表中選取 [內容]。
1. 選取 [詳細資料] 索引標籤。[檔案版本] 和 [產品版本] 代表已安裝的模組版本。

模組的「裝載套件組合」安裝程式記錄檔位於 *C:\\Users\\%UserName%\\AppData\\Local\\Temp*。檔案的名稱為 *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*。

## <a name="module-schema-and-configuration-file-locations"></a>模組、結構描述及設定檔位置

### <a name="module"></a>Module

**IIS (x86/amd64)：**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64)：**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>結構描述

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>組態

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

在 *applicationHost.config* 檔案中搜尋 *aspnetcore.dll*，即可找到這些檔案。 針對 IIS Express，*applicationHost.config* 檔案預設並不存在。 啟動 Visual Studio 方案中的任何 Web 應用程式專案時，便會在 *\<application_root>\\.vs\\config* 建立該檔案。
