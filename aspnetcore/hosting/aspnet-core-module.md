---
title: "ASP.NET 核心模組的組態參考"
author: guardrex
description: "如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組。"
keywords: "ASP.NET Core ancm、 核心模組、 iis、 stdout 記錄、 環境變數，env var、 subapplication、 subapp、 appoffline、 app_offline，502，結構描述"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: ac52b791e02ce52da35fe8d599465076d251b4da
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2017
---
# <a name="aspnet-core-module-configuration-reference"></a>ASP.NET 核心模組的組態參考

由[Luke Latham](https://github.com/guardrex)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本文件提供有關如何設定適用於主控 ASP.NET Core 應用程式的 ASP.NET 核心模組的詳細資料。 如需 ASP.NET Core 模組和安裝指示的簡介，請參閱[ASP.NET Core 模組概觀](xref:fundamentals/servers/aspnet-core-module)。

## <a name="configuration-via-webconfig"></a>透過 web.config 設定

ASP.NET 核心模組已透過站台或應用程式設定*web.config*檔案，並有它自己`aspNetCore`組態區段內`system.webServer`。 以下是範例*web.config*檔`Microsoft.NET.Sdk.Web`SDK 會提供當專案發行時[framework 相依的部署](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)使用預留位置`processPath`和`arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*Web.config*下列範例是針對[獨立的部署](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd)至[Azure App Service](https://azure.microsoft.com/services/app-service/)。 如需詳細資訊，請參閱[發行至 IIS](xref:publishing/iis)。 請參閱[子應用程式的組態](xref:publishing/iis#configuration-of-sub-applications)的組態相關的重要注意事項*web.config*子應用程式中的檔案。

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>AspNetCore 元素的屬性

| 屬性 | 說明 |
| --- | --- |
| processPath | <p>必要的字串屬性。</p><p>將會啟動接聽 HTTP 要求的處理序的可執行檔的路徑。 支援相對路徑。 如果路徑是以開頭 '。 '，路徑會被視為相對於網站根目錄。</p><p>它沒有預設值。</p> |
| 引數 | <p>選擇性字串屬性。</p><p>可執行檔中指定的引數**processPath**。</p><p>預設值為空字串。</p> |
| startupTimeLimit | <p>選擇性整數屬性。</p><p>此模組，可開始接聽連接埠之處理序的等候秒持續時間。 如果超過此時間限制，此模組會終止處理序。 此模組會嘗試啟動處理序，當它收到新的要求，並將繼續嘗試重新啟動的程序，在後續的內送要求，除非應用程式無法啟動**rapidFailsPerMinute**數目最後一個循環分鐘的時間。</p><p>預設值為 120。</p> |
| shutdownTimeLimit | <p>選擇性整數屬性。</p><p>持續時間 （秒） 模組將會等候到正常關機狀態的可執行檔時*app_offline.htm*偵測到的檔案。</p><p>預設值為 10。</p> |
| rapidFailsPerMinute | <p>選擇性整數屬性。</p><p>指定處理程序中指定的次數**processPath**允許每分鐘的損毀。 如果超過此限制，此模組會停止啟動該分鐘的剩餘的處理程序。</p><p>預設值為 10。</p> |
| requestTimeout | <p>選擇性 timespan 屬性。</p><p>指定 ASP.NET 核心模組將等候來自接聽 %aspnetcore_port%之處理序的回應持續期間。</p><p>預設值為 "00:02:00"。</p> |
| stdoutLogEnabled | <p>選擇性的 Boolean 屬性。</p><p>如果為 true， **stdout**和**stderr**中指定的處理序**processPath**會重新導向至指定的檔案**stdoutLogFile**.</p><p>預設值為 false。</p> |
| stdoutLogFile | <p>選擇性字串屬性。</p><p>指定的相對或絕對檔案路徑的**stdout**和**stderr**中指定的處理序從**processPath**將記錄。 相對路徑會相對於網站根目錄。 從任何路徑 '。 ' 將會相對於網站根目錄和所有其他路徑將會被視為絕對路徑。 提供的路徑中任何資料夾必須存在於要建立的記錄檔之模組的順序。 處理序識別碼，時間戳記 (*yyyyMdhms*)，和副檔名 (*.log*) 加上底線分隔符號會加入至最後一個區段**stdoutLogFile**提供。</p><p>預設值是 `aspnetcore-stdout`。</p> |
| forwardWindowsAuthToken | true 或 false。</p><p>如果為 true，權杖會轉送子處理序做為每個要求的標頭 ' MS-ASPNETCORE-WINAUTHTOKEN' 接聽 %aspnetcore_port%。 它負責這個語彙基元，每個要求上呼叫 CloseHandle 該程序。</p><p>預設值為 true。</p> |
| disableStartUpErrorPage | true 或 false。</p><p>如果為 true， **502.5-處理序失敗**頁面將會隱藏，而且 502 狀態字碼頁設定在您*web.config*更高的優先順序。</p><p>預設值為 false。</p> |

### <a name="setting-environment-variables"></a>設定環境變數

ASP.NET 核心模組可以讓您指定環境變數中指定的處理序`processPath`中一或多個指定這些屬性`environmentVariable`子項目的`environmentVariables`集合項目底下`aspNetCore`項目。 此區段中設定的環境變數優先於系統處理序的環境變數。

下列範例會設定兩個環境變數。 `ASPNETCORE_ENVIRONMENT`將應用程式的環境設定成`Development`。 開發人員可能會暫時將此值設定*web.config*檔案，以便強制[開發人員例外狀況頁面](xref:fundamentals/error-handling)載入偵錯應用程式例外狀況時。 `CONFIG_DIR`是使用者定義的環境變數的範例，開發人員已撰寫的程式碼會讀取啟動，以形成才能載入的應用程式組態檔的路徑上的值。

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

## <a name="appofflinehtm"></a>app_offline.htm

如果您將檔案名稱*app_offline.htm*根目錄中的 web 應用程式目錄，ASP.NET 核心模組將會嘗試依正常程序關閉應用程式，並停止處理內送要求。 如果應用程式仍在執行`shutdownTimeLimit`數秒的 ASP.NET 核心模組會終止執行的處理序。

雖然*app_offline.htm*檔案是否存在，ASP.NET 核心模組會回應要求傳送回內容*app_offline.htm*檔案。 一次*app_offline.htm*會移除檔案，在下一個要求載入應用程式，然後回應要求。

## <a name="start-up-error-page"></a>啟動錯誤頁面

如果 ASP.NET 核心模組無法啟動後端程序或後端程序會啟動但無法在設定的連接埠上接聽，您會看到 HTTP 502.5 狀態字碼頁。 若要隱藏這個頁面，並還原至預設 IIS 502 狀態字碼頁，請使用`disableStartUpErrorPage`屬性。 如需有關如何設定自訂錯誤訊息的詳細資訊，請參閱[HTTP 錯誤`<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/)。

![502 狀態 頁面](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>記錄檔的建立和重新導向

ASP.NET 核心模組重新導向`stdout`和`stderr`記錄檔磁碟，如果您設定`stdoutLogEnabled`和`stdoutLogFile`屬性`aspNetCore`項目。 中的任何資料夾`stdoutLogFile`路徑必須存在於要建立的記錄檔之模組的順序。 建立記錄檔時，會自動加入時間戳記與檔案的副檔名。 記錄檔不會進行旋轉，除非處理序回收/重新啟動，就會發生。 它是主機服務提供者來限制記錄檔使用的磁碟空間的責任。 使用`stdout`疑難排解應用程式啟動問題而非一般應用程式記錄用途只建議記錄檔。

記錄檔名稱由附加的處理序識別碼 (PID)，時間戳記 (*yyyyMdhms*)，和副檔名 (*.log*) 的最後一個區段以`stdoutLogFile`路徑 (通常*stdout*) 底線字元分隔。 例如如果`stdoutLogFile`路徑的結尾*stdout*，記錄檔的應用程式，但在 8/10/2017年端建立 12:05:02 10652 PID 有檔案名稱*stdout_10652_20178101252.log*。

以下是範例`aspNetCore`設定項目`stdout`記錄。 `stdoutLogFile`範例所示的路徑是否適用於 Azure 應用程式服務。 本機路徑或網路共用路徑是可接受的本機記錄。 確認應用程式集區使用者識別必須提供路徑的寫入權限。

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>IIS 與 ASP.NET Core 模組共用設定

ASP.NET 核心模組安裝程式會執行與的權限**系統**帳戶。 因為本機系統帳戶沒有修改 IIS 共用設定所使用的共用路徑的權限，安裝程式會叫用拒絕存取錯誤時嘗試設定中的模組設定*applicationHost.config*共用上。

不支援的因應措施是停用 IIS 共用設定，執行安裝程式、 匯出更新*applicationHost.config*檔案共用，並重新啟用 IIS 共用設定。

## <a name="module-schema-and-configuration-file-locations"></a>模組、 結構描述和組態檔的位置

### <a name="module"></a>模組

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %Programfiles (x86) %\IIS Express\aspnetcore.dll

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

您可以搜尋*aspnetcore.dll*中*applicationHost.config*檔案。 IIS express， *applicationHost.config*根據預設，檔案不存在。 檔案建立在*{應用程式根目錄}\.vs\config*當您啟動任何 web 應用程式專案中的 Visual Studio 方案。
