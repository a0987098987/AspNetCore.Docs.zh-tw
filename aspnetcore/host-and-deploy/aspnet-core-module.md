---
title: ASP.NET 核心模組的組態參考
author: guardrex
description: 了解如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET 核心模組的組態參考

由[Luke Latham](https://github.com/guardrex)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本文件提供有關如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組的指示。 如需 ASP.NET Core 模組和安裝指示的簡介，請參閱[ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。

## <a name="configuration-with-webconfig"></a>Web.config 組態

設定 ASP.NET 核心模組`aspNetCore`區段`system.webServer`中站台的節點*web.config*檔案。

下列*web.config*發行檔案以進行[framework 相依的部署](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)和設定 ASP.NET 核心模組，以處理站台的要求：

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

下列*web.config*發行以供[獨立的部署](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

當應用程式部署至[Azure App Service](https://azure.microsoft.com/services/app-service/)、`stdoutLogFile`路徑會設定為`\\?\%home%\LogFiles\stdout`。 路徑會將 stdout 記錄檔，以儲存*LogFiles*資料夾中，這是放置位置自動服務所建立的。

請參閱[子應用程式組態](xref:host-and-deploy/iis/index#sub-application-configuration)的組態相關的重要注意事項*web.config*子應用程式中的檔案。

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore 元素的屬性

::: moniker range="<= aspnetcore-2.0"
| 屬性 | 描述 | 預設 |
| --------- | ----------- | :-----: |
| `arguments` | <p>選擇性字串屬性。</p><p>可執行檔中指定的引數**processPath**。</p>| |
| `disableStartUpErrorPage` | true 或 false。</p><p>如果為 true， **502.5-處理序失敗**隱藏頁面時，且狀態 502 字碼頁設定*web.config*優先。</p> | `false` |
| `forwardWindowsAuthToken` | true 或 false。</p><p>如果為 true，則會將權杖轉送到接聽 %aspnetcore_port%做為每個要求的標頭 ' MS-ASPNETCORE-WINAUTHTOKEN' 的子處理序。 它負責這個語彙基元，每個要求上呼叫 CloseHandle 該程序。</p> | `true` |
| `processPath` | <p>必要的字串屬性。</p><p>啟動接聽 HTTP 要求的處理序的可執行檔的路徑。 支援相對路徑。 如果路徑是以開頭`.`，路徑會被視為相對於網站根目錄。</p> | |
| `rapidFailsPerMinute` | <p>選擇性整數屬性。</p><p>指定處理程序中指定的次數**processPath**允許每分鐘的損毀。 如果超過此限制時，模組會停止啟動該分鐘的剩餘的處理程序。</p> | `10` |
| `requestTimeout` | <p>選擇性 timespan 屬性。</p><p>指定 ASP.NET 核心模組等候來自接聽 %aspnetcore_port%之處理序的回應持續期間。</p><p>在版本的 ASP.NET 核心模組版本的 ASP.NET Core 2.0 或更早版本，隨附`requestTimeout`必須指定以分鐘為單位，否則它會預設為 2 分鐘。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>選擇性整數屬性。</p><p>持續時間，以秒為單位的模組，可依正常程序關閉時*app_offline.htm*偵測到的檔案。</p> | `10` |
| `startupTimeLimit` | <p>選擇性整數屬性。</p><p>以秒為單位的模組，可開始接聽連接埠之處理序的持續時間。 如果超過此時間限制，此模組會清除程序。 模組會嘗試重新啟動處理程序，當它接收新要求，並會繼續嘗試重新啟動的程序，在後續的內送要求，除非應用程式無法啟動**rapidFailsPerMinute**在上一次循環的分鐘。</p> | `120` |
| `stdoutLogEnabled` | <p>選擇性的 Boolean 屬性。</p><p>如果為 true， **stdout**和**stderr**中指定的處理序**processPath**會重新導向至指定的檔案**stdoutLogFile**。</p> | `false` |
| `stdoutLogFile` | <p>選擇性字串屬性。</p><p>指定的相對或絕對檔案路徑的**stdout**和**stderr**中指定的處理序從**processPath**記錄。 相對路徑會相對於網站根目錄。 從任何路徑`.`是相對於網站根目錄和所有其他路徑會被視為絕對路徑。 提供的路徑中任何資料夾必須存在於要建立的記錄檔之模組的順序。 使用底線分隔符號、 時間戳記、 處理序識別碼和副檔名 (*.log*) 會加入至最後一個區段**stdoutLogFile**路徑。 如果`.\logs\stdout`提供為數值，而範例 stdout 記錄會儲存為*stdout_20180205194132_1934.log*中*記錄*資料夾時儲存在 2/5/2018年 19:41:32 與 1934年的處理序識別碼。</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| 屬性 | 描述 | 預設 |
| --------- | ----------- | :-----: |
| `arguments` | <p>選擇性字串屬性。</p><p>可執行檔中指定的引數**processPath**。</p>| |
| `disableStartUpErrorPage` | true 或 false。</p><p>如果為 true， **502.5-處理序失敗**隱藏頁面時，且狀態 502 字碼頁設定*web.config*優先。</p> | `false` |
| `forwardWindowsAuthToken` | true 或 false。</p><p>如果為 true，則會將權杖轉送到接聽 %aspnetcore_port%做為每個要求的標頭 ' MS-ASPNETCORE-WINAUTHTOKEN' 的子處理序。 它負責這個語彙基元，每個要求上呼叫 CloseHandle 該程序。</p> | `true` |
| `processPath` | <p>必要的字串屬性。</p><p>啟動接聽 HTTP 要求的處理序的可執行檔的路徑。 支援相對路徑。 如果路徑是以開頭`.`，路徑會被視為相對於網站根目錄。</p> | |
| `rapidFailsPerMinute` | <p>選擇性整數屬性。</p><p>指定處理程序中指定的次數**processPath**允許每分鐘的損毀。 如果超過此限制時，模組會停止啟動該分鐘的剩餘的處理程序。</p> | `10` |
| `requestTimeout` | <p>選擇性 timespan 屬性。</p><p>指定 ASP.NET 核心模組等候來自接聽 %aspnetcore_port%之處理序的回應持續期間。</p><p>在版本的 ASP.NET 核心模組版本的 ASP.NET Core 2.1 或更新版本中，隨附`requestTimeout`中小時、 分鐘和秒為單位指定。</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>選擇性整數屬性。</p><p>持續時間，以秒為單位的模組，可依正常程序關閉時*app_offline.htm*偵測到的檔案。</p> | `10` |
| `startupTimeLimit` | <p>選擇性整數屬性。</p><p>以秒為單位的模組，可開始接聽連接埠之處理序的持續時間。 如果超過此時間限制，此模組會清除程序。 模組會嘗試重新啟動處理程序，當它接收新要求，並會繼續嘗試重新啟動的程序，在後續的內送要求，除非應用程式無法啟動**rapidFailsPerMinute**在上一次循環的分鐘。</p> | `120` |
| `stdoutLogEnabled` | <p>選擇性的 Boolean 屬性。</p><p>如果為 true， **stdout**和**stderr**中指定的處理序**processPath**會重新導向至指定的檔案**stdoutLogFile**。</p> | `false` |
| `stdoutLogFile` | <p>選擇性字串屬性。</p><p>指定的相對或絕對檔案路徑的**stdout**和**stderr**中指定的處理序從**processPath**記錄。 相對路徑會相對於網站根目錄。 從任何路徑`.`是相對於網站根目錄和所有其他路徑會被視為絕對路徑。 提供的路徑中任何資料夾必須存在於要建立的記錄檔之模組的順序。 使用底線分隔符號、 時間戳記、 處理序識別碼和副檔名 (*.log*) 會加入至最後一個區段**stdoutLogFile**路徑。 如果`.\logs\stdout`提供為數值，而範例 stdout 記錄會儲存為*stdout_20180205194132_1934.log*中*記錄*資料夾時儲存在 2/5/2018年 19:41:32 與 1934年的處理序識別碼。</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>設定環境變數

中的程序可以指定環境變數`processPath`屬性。 指定的環境變數`environmentVariable`子項目`environmentVariables`集合項目。 此區段中設定的環境變數優先於系統環境變數。

下列範例會設定兩個環境變數。 `ASPNETCORE_ENVIRONMENT` 設定應用程式的環境`Development`。 開發人員可能會暫時將此值設定*web.config*檔案，以便強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入偵錯應用程式例外狀況時。 `CONFIG_DIR` 是使用者定義的環境變數的範例，開發人員已寫入讀取的值，在啟動時形成一個載入的應用程式組態檔路徑的程式碼的位置。

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
> 只能設定`ASPNETCORE_ENVIRONMENT`envirnonment 變數`Development`籌畫與測試無法存取不受信任的網路，例如網際網路的伺服器上。

## <a name="appofflinehtm"></a>app_offline.htm

如果同名的檔案*app_offline.htm*偵測到應用程式的根目錄中 ASP.NET 核心模組嘗試依正常程序關閉應用程式，然後停止處理內送要求。 如果應用程式仍在執行中定義的秒數後`shutdownTimeLimit`，ASP.NET 核心模組會清除執行的處理序。

雖然*app_offline.htm*檔案是否存在，ASP.NET 核心模組回應要求傳送回內容*app_offline.htm*檔案。 當*app_offline.htm*會移除檔案，在下一個要求會啟動應用程式。

## <a name="start-up-error-page"></a>啟動錯誤頁面

如果 ASP.NET 核心模組無法啟動後端程序或後端程序會啟動但無法在設定的連接埠上接聽*502.5 的處理序失敗*狀態程式碼 頁面隨即出現。 若要隱藏這個頁面，並還原至預設 IIS 502 狀態字碼頁，請使用`disableStartUpErrorPage`屬性。 如需有關如何設定自訂錯誤訊息的詳細資訊，請參閱[HTTP 錯誤`<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/)。

![502.5 程序失敗狀態的字碼頁](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>記錄檔的建立和重新導向

ASP.NET 核心模組重新導向磁碟 stdout 及 stderr 記錄`stdoutLogEnabled`和`stdoutLogFile`屬性`aspNetCore`設定項目。 中的任何資料夾`stdoutLogFile`路徑必須存在於要建立的記錄檔之模組的順序。 應用程式集區必須具有寫入存取權來寫入記錄檔的位置 (使用`IIS AppPool\<app_pool_name>`提供寫入權限)。

不旋轉記錄檔，除非處理序回收/重新啟動，就會發生。 它是主機服務提供者來限制記錄檔使用的磁碟空間的責任。

使用 stdout 記錄只建議用於疑難排解應用程式啟動問題。 一般應用程式記錄檔，請勿使用 stdout 記錄檔。 例行記錄中的 ASP.NET Core 應用程式、 使用限制記錄檔大小，並且會旋轉記錄檔的記錄程式庫。 如需詳細資訊，請參閱[協力廠商記錄提供者](xref:fundamentals/logging/index#third-party-logging-providers)。

建立記錄檔時，會自動加入時間戳記與檔案的副檔名。 記錄檔名稱由附加的時間戳記、 處理序識別碼和副檔名 (*.log*) 的最後一個區段以`stdoutLogFile`路徑 (通常*stdout*) 底線字元分隔。 如果`stdoutLogFile`路徑的結尾*stdout*，記錄檔的應用程式，但在 2/5/2018年端建立 19:42:32 1934 PID 有檔案名稱*stdout_20180205194132_1934.log*。

下列範例`aspNetCore`項目會設定 Azure App Service 中裝載的應用程式的 stdout 記錄。 本機路徑或網路共用路徑是可接受的本機記錄。 確認應用程式集區使用者識別必須提供路徑的寫入權限。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

請參閱[web.config 組態](#configuration-with-webconfig)的範例，`aspNetCore`中的項目*web.config*檔案。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Proxy 組態使用 HTTP 通訊協定和配對權杖

在 ASP.NET 核心模組和 Kestrel 之間建立 proxy 會使用 HTTP 通訊協定。 使用 HTTP 是效能最佳化，其中模組和 Kestrel 之間的流量會在回送位址從網路介面。 沒有任何風險竊聽模組和 Kestrel 從位置不在伺服器之間的流量。

配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。 建立並設定環境變數配對的語彙基元 (`ASPNETCORE_TOKEN`) 模組。 配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。 IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。 如果權杖值不相符，將記錄並拒絕要求。 配對的語彙基元的環境變數和模組和 Kestrel 之間的流量無法存取從出伺服器的位置。 在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>IIS 與 ASP.NET Core 模組共用設定

ASP.NET 核心模組安裝程式會執行與的權限**系統**帳戶。 安裝程式的本機系統帳戶不會有修改 IIS 共用設定所使用的共用路徑的權限，因為遇到拒絕存取錯誤時嘗試設定中的模組設定*applicationHost.config*共用上。 當使用 IIS 共用設定，請遵循下列步驟：

1. 停用 IIS 共用的設定。
1. 執行安裝程式。
1. 匯出已更新*applicationHost.config*共用的檔案。
1. 重新啟用 IIS 共用的設定。

## <a name="module-version-and-hosting-bundle-installer-logs"></a>模組版本和裝載配套安裝程式記錄檔

若要判斷已安裝的 ASP.NET 核心模組版本：

1. 在主機系統上，瀏覽至 *%windir%\System32\inetsrv*。
1. 找出*aspnetcore.dll*檔案。
1. 以滑鼠右鍵按一下檔案，然後選取**屬性**從內容功能表。
1. 選取**詳細資料** 索引標籤。**檔案版本**和**產品版本**代表已安裝之模組的版本。

在找到裝載配套安裝程式記錄檔模組*c:\\使用者\\%username%\\AppData\\本機\\Temp*。檔案命名為*dd_DotNetCoreWinSvrHosting__\<時間戳記 > _000_AspNetCoreModule_x64.log*。

## <a name="module-schema-and-configuration-file-locations"></a>模組、 結構描述和組態檔的位置

### <a name="module"></a>Module

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

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

可以找到檔案，藉由搜尋*aspnetcore.dll*中*applicationHost.config*檔案。 IIS express， *applicationHost.config*根據預設，檔案不存在。 檔案建立在 *\<application_root >\\.vs\\config*時啟動 Visual Studio 方案中的任何 web 應用程式專案。
