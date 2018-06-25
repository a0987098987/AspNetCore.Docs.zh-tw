---
title: 與 ASP.NET Core 搭配運作的 IIS 模組
author: guardrex
description: 探索 ASP.NET Core 應用程式的使用中和非使用中 IIS 模組，管理 IIS 模組的方式。
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 1ff0fdcaae066b493eeebf6a061e383f88c81052
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272733"
---
# <a name="iis-modules-with-aspnet-core"></a>與 ASP.NET Core 搭配運作的 IIS 模組

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET Core 應用程式在反向 Proxy 設定中會由 IIS 裝載。 您無法使用部分原生 IIS 模組及所有 IIS 受控模組來處理 ASP.NET Core 應用程式的要求。 在許多情況下，ASP.NET Core 都有提供 IIS 原生和受控模組的替代方案。

## <a name="native-modules"></a>原生模組

下表指出對於向 ASP.NET Core 應用程式發出的反向 Proxy 要求有作用的原生 IIS 模組。

| Module | 對 ASP.NET Core 應用程式有作用 | ASP.NET Core 選項 |
| ------ | :-------------------------------: | ------------------- |
| **匿名驗證**<br>`AnonymousAuthenticationModule` | [是] | |
| **基本驗證**<br>`BasicAuthenticationModule` | [是] | |
| **用戶端憑證對應驗證**<br>`CertificateMappingAuthenticationModule` | [是] | |
| **CGI**<br>`CgiModule` | 否 | |
| **設定驗證**<br>`ConfigurationValidationModule` | [是] | |
| **HTTP 錯誤**<br>`CustomErrorModule` | 否 | [狀態碼頁面中介軟體](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **自訂記錄**<br>`CustomLoggingModule` | [是] | |
| **預設文件**<br>`DefaultDocumentModule` | 否 | [預設檔案中介軟體](xref:fundamentals/static-files#serve-a-default-document) |
| **摘要式驗證**<br>`DigestAuthenticationModule` | [是] | |
| **目錄瀏覽**<br>`DirectoryListingModule` | 否 | [目錄瀏覽中介軟體](xref:fundamentals/static-files#enable-directory-browsing) |
| **動態壓縮**<br>`DynamicCompressionModule` | [是] | [回應壓縮中介軟體](xref:performance/response-compression) |
| **追蹤**<br>`FailedRequestsTracingModule` | [是] | [ASP.NET Core 記錄](xref:fundamentals/logging/index#tracesource-provider) |
| **檔案快取**<br>`FileCacheModule` | 否 | [回應快取中介軟體](xref:performance/caching/middleware) |
| **HTTP 快取**<br>`HttpCacheModule` | 否 | [回應快取中介軟體](xref:performance/caching/middleware) |
| **HTTP 記錄**<br>`HttpLoggingModule` | [是] | [ASP.NET Core 記錄](xref:fundamentals/logging/index)<br>實作：[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)、[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)、[NLog](https://github.com/NLog/NLog.Extensions.Logging)、[Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP 重新導向**<br>`HttpRedirectionModule` | [是] | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| **IIS 用戶端憑證對應驗證**<br>`IISCertificateMappingAuthenticationModule` | [是] | |
| **IP 及網域限制**<br>`IpRestrictionModule` | [是] | |
| **ISAPI 篩選器**<br>`IsapiFilterModule` | [是] | [中介軟體](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | [是] | [中介軟體](xref:fundamentals/middleware/index) |
| **通訊協定支援**<br>`ProtocolSupportModule` | [是] | |
| **要求篩選**<br>`RequestFilteringModule` | [是] | [URL 重寫中介軟體`IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **要求監視器**<br>`RequestMonitorModule` | [是] | |
| **URL 重寫**<br>`RewriteModule` | 是&#8224; | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| **伺服器端包含**<br>`ServerSideIncludeModule` | 否 | |
| **靜態壓縮**<br>`StaticCompressionModule` | 否 | [回應壓縮中介軟體](xref:performance/response-compression) |
| **靜態內容**<br>`StaticFileModule` | 否 | [靜態檔案中介軟體](xref:fundamentals/static-files) |
| **權杖快取**<br>`TokenCacheModule` | [是] | |
| **URI 快取**<br>`UriCacheModule` | [是] | |
| **URL 授權**<br>`UrlAuthorizationModule` | [是] | [ASP.NET Core 身分識別](xref:security/authentication/identity) |
| **Windows 驗證**<br>`WindowsAuthenticationModule` | [是] | |

&#8224;由於[目錄結構](xref:host-and-deploy/directory-structure)變更的緣故，因此「URL 重寫模組」的 `isFile` 和 `isDirectory` 比對類型對 ASP.NET Core 應用程式沒有作用。

## <a name="managed-modules"></a>受控模組

當應用程式集區的 .NET CLR 版本已設定為 [沒有 Managed 程式碼] 時，受控模組對所裝載的 ASP.NET Core 應用程式「沒有」作用。 ASP.NET Core 在數種案例中都有提供中介軟體替代方案。

| Module                  | ASP.NET Core 選項 |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Cookie 驗證中介軟體](xref:security/authentication/cookie) |
| OutputCache             | [回應快取中介軟體](xref:performance/caching/middleware) |
| 設定檔                 | |
| RoleManager             | |
| ScriptModule-4.0        | |
| 工作階段                 | [工作階段中介軟體](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [ASP.NET Core 身分識別](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS 管理員應用程式變更

使用「IIS 管理員」來進行設定時，會變更應用程式的 *web.config* 檔案。 如果部署應用程式並包含 *web.config*，則所部署的 *web.config* 檔案會覆寫使用「IIS 管理員」來進行的所有變更。 對伺服器的 *web.config* 檔案進行變更後，請立即將伺服器上已更新的 *web.config* 檔案複製到本機專案。

## <a name="disabling-iis-modules"></a>停用 IIS 模組

如果在必須針對應用程式停用的伺服器層級設定了 IIS 模組，只要在應用程式的 *web.config* 檔案中新增設定，即可停用該模組。 請將模組留在原處，然後使用組態設定 (如果有的話) 來停用它，或是從應用程式移除模組。

### <a name="module-deactivation"></a>模組停用

許多模組都有提供可將模組停用而無須從應用程式中移除的組態設定。 這是停用模組的最簡便快速方式。 例如，使用 *web.config* 中的 **\<httpRedirect>** 元素，即可停用「HTTP 重新導向模組」：

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

如需有關使用組態設定來停用模組的詳細資訊，請參考 [IIS \<system.webServer>](/iis/configuration/system.webServer/) ＜子元素＞一節中的連結。

### <a name="module-removal"></a>模組移除

如果選擇藉由 *web.config* 中的設定來移除模組，請先將模組解除鎖定，以及將 *web.config* 的 **\<modules>** 區段解除鎖定：

1. 將伺服器層級的模組解除鎖定。 選取「IIS 管理員」[連線] 資訊看板中的 IIS 伺服器。 開啟 [IIS] 區域中的 [模組]。 選取清單中的模組。 在右邊的 [動作] 資訊看板上，選取 [解除鎖定]。 將您打算稍後從 *web.config* 移除的模組都解除鎖定。

2. 在 *web.config* 不含 **\<modules>** 區段的情況下部署應用程式。如果在 *web.config* 包含 **\<modules>** 區段的情況下部署應用程式，但未先在「IIS 管理員」中將該區段解除鎖定，則當「設定管理員」嘗試將該區段解除鎖定時就會擲回例外狀況。 因此，請在沒有 **\<modules>** 區段的情況下部署應用程式。

3. 將 *web.config*的 **\<modules>** 區段解除鎖定。在 [連線] 資訊看板中，選取 [站台]中的網站。 在 [管理] 區域中，開啟 [設定編輯器]。 使用導覽控制項來選取 `system.webServer/modules` 區段。 在右邊的 [動作] 資訊看板上，選取將區段 [解除鎖定]。

4. 此時，可以在 *web.config* 檔案中，新增一個含有 **\<remove>** 元素以從應用程式移除模組的 **\<modules>** 區段。 您可以新增多個 **\<remove>** 元素來移除多個模組。 如果已在伺服器上進行 *web.config* 變更，請立即在本機對專案的 *web.config* 檔案進行相同的變更。 以這種方式移除模組不會影響模組與伺服器上其他應用程式的搭配使用。

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

您也可以使用 *Appcmd.exe* 來移除 IIS 模組。 請在命令中提供 `MODULE_NAME` 和 `APPLICATION_NAME`：

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

例如，從預設網站中移除 `DynamicCompressionModule`：

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>最基本的模組設定

執行 ASP.NET Core 應用程式只需「匿名驗證模組」和「ASP.NET Core 模組」這兩個模組。

![開啟至只顯示最基本模組設定 [模組] 畫面的「IIS 管理員」](modules/_static/modules.png)

「URI 快取模組」(`UriCacheModule`) 可讓 IIS 快取 URL 層級的網站設定。 如果沒有此模組，IIS 就必須針對每個要求都讀取並剖析設定，即使是重複要求相同的 URL 時也一樣。 針對每個要求都剖析設定會導致效能大幅降低。 *雖然不一定要有「URI 快取模組」，所裝載的 ASP.NET Core 應用程式就能執行，但建議您為所有 ASP.NET Core 部署都啟用「URI 快取模組」。*

「HTTP 快取模組」(`HttpCacheModule`) 會實作 IIS 輸出快取，也會實作用來將項目快取至 HTTP.sys 快取中的邏輯。 如果沒有此模組，就不會再以核心模式快取內容，而且會忽略快取設定檔。 移除「HTTP 快取模組」通常會對效能和資源使用情況造成負面影響。 *雖然不一定要有「HTTP 快取模組」，所裝載的 ASP.NET Core 應用程式就能執行，但建議您為所有 ASP.NET Core 部署都啟用「HTTP 快取模組」。*

## <a name="additional-resources"></a>其他資源

* [ Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)
* [IIS 架構簡介：IIS 中的模組](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS 模組概觀](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [自訂 IIS 7.0 角色和模組](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
