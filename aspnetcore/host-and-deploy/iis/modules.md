---
title: 與 ASP.NET Core 搭配運作的 IIS 模組
author: rick-anderson
description: 探索 ASP.NET Core 應用程式的使用中和非使用中 IIS 模組，管理 IIS 模組的方式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 7262b9ea18e4cf6acd278d087fcc44262f8f9c80
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775943"
---
# <a name="iis-modules-with-aspnet-core"></a>與 ASP.NET Core 搭配運作的 IIS 模組

您無法使用部分原生 IIS 模組與所有 IIS 受控模組來處理 ASP.NET Core 應用程式的要求。 在許多情況下，ASP.NET Core 都提供由 IIS 原生與受控模組處理之情節的替代方案。

## <a name="native-modules"></a>原生模組

下表指出可搭配 ASP.NET Core 應用程式和 ASP.NET Core 模組運作的原生 IIS 模組。

| Module | 對 ASP.NET Core 應用程式有作用 | ASP.NET Core 選項 |
| --- | :---: | --- |
| **匿名驗證**<br>`AnonymousAuthenticationModule`                                  | 是 | |
| **基本驗證**<br>`BasicAuthenticationModule`                                          | 是 | |
| **用戶端憑證對應驗證**<br>`CertificateMappingAuthenticationModule`      | 是 | |
| **CGI**<br>`CgiModule`                                                                           | 否  | |
| **設定驗證**<br>`ConfigurationValidationModule`                                  | 是 | |
| **HTTP 錯誤**<br>`CustomErrorModule`                                                           | 否  | [狀態碼頁面中介軟體](xref:fundamentals/error-handling#usestatuscodepages) |
| **自訂記錄**<br>`CustomLoggingModule`                                                      | 是 | |
| **預設檔**<br>`DefaultDocumentModule`                                                  | 否  | [預設檔案中介軟體](xref:fundamentals/static-files#serve-a-default-document) |
| **摘要式驗證**<br>`DigestAuthenticationModule`                                        | 是 | |
| **流覽目錄**<br>`DirectoryListingModule`                                               | 否  | [目錄瀏覽中介軟體](xref:fundamentals/static-files#enable-directory-browsing) |
| **動態壓縮**<br>`DynamicCompressionModule`                                            | 是 | [回應壓縮中介軟體](xref:performance/response-compression) |
| **失敗的要求追蹤**<br>`FailedRequestsTracingModule`                                     | 是 | [ASP.NET Core 記錄](xref:fundamentals/logging/index#tracesource-provider) |
| **檔案快取**<br>`FileCacheModule`                                                            | 否  | [回應快取中介軟體](xref:performance/caching/middleware) |
| **HTTP 快取**<br>`HttpCacheModule`                                                            | 否  | [回應快取中介軟體](xref:performance/caching/middleware) |
| **HTTP 記錄**<br>`HttpLoggingModule`                                                          | 是 | [ASP.NET Core 記錄](xref:fundamentals/logging/index) |
| **HTTP 重新導向**<br>`HttpRedirectionModule`                                                  | 是 | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| **HTTP 追蹤**<br>`TracingModule`                                                              | 是 | |
| **IIS 用戶端憑證對應驗證**<br>`IISCertificateMappingAuthenticationModule` | 是 | |
| **IP 和網域限制**<br>`IpRestrictionModule`                                          | 是 | |
| **ISAPI 篩選器**<br>`IsapiFilterModule`                                                         | 是 | [中介軟體](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule`                                                                       | 是 | [中介軟體](xref:fundamentals/middleware/index) |
| **通訊協定支援**<br>`ProtocolSupportModule`                                                  | 是 | |
| **要求篩選**<br>`RequestFilteringModule`                                                | 是 | [URL 重寫中介軟體`IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **要求監視器**<br>`RequestMonitorModule`                                                    | 是 | |
| **URL 重新寫入**&#8224;<br>`RewriteModule`                                                      | 是 | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| **伺服器端包含**<br>`ServerSideIncludeModule`                                            | 否  | |
| **靜態壓縮**<br>`StaticCompressionModule`                                              | 否  | [回應壓縮中介軟體](xref:performance/response-compression) |
| **靜態內容**<br>`StaticFileModule`                                                         | 否  | [靜態檔案中介軟體](xref:fundamentals/static-files) |
| **權杖快取**<br>`TokenCacheModule`                                                          | 是 | |
| **URI 快取**<br>`UriCacheModule`                                                              | 是 | |
| **URL 授權**<br>`UrlAuthorizationModule`                                                | 是 | [ASP.NET Core 身分識別](xref:security/authentication/identity) |
| **Windows 驗證**<br>`WindowsAuthenticationModule`                                      | 是 | |

&#8224;由於[目錄結構](xref:host-and-deploy/directory-structure)變更的緣故，因此「URL 重寫模組」的 `isFile` 和 `isDirectory` 比對類型對 ASP.NET Core 應用程式沒有作用。

## <a name="managed-modules"></a>受控模組

當應用程式集區的 .NET CLR 版本已設定為 [沒有 Managed 程式碼]**** 時，受控模組對所裝載的 ASP.NET Core 應用程式「沒有」** 作用。 ASP.NET Core 在數種案例中都有提供中介軟體替代方案。

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
| UrlRoutingModule-4.0    | [ASP.NET CoreIdentity](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS 管理員應用程式變更

使用「IIS 管理員」來進行設定時，會變更應用程式的 *web.config* 檔案。 如果部署應用程式並包含 *web.config*，則所部署的 *web.config* 檔案會覆寫使用「IIS 管理員」來進行的所有變更。 對伺服器的 *web.config* 檔案進行變更後，請立即將伺服器上已更新的 *web.config* 檔案複製到本機專案。

## <a name="disabling-iis-modules"></a>停用 IIS 模組

如果在必須針對應用程式停用的伺服器層級設定了 IIS 模組，只要在應用程式的 *web.config* 檔案中新增設定，即可停用該模組。 請將模組留在原處，然後使用組態設定 (如果有的話) 來停用它，或是從應用程式移除模組。

### <a name="module-deactivation"></a>模組停用

許多模組都有提供可將模組停用而無須從應用程式中移除的組態設定。 這是停用模組的最簡便快速方式。 例如，使用 *web.config* 中的 `<httpRedirect>` 元素，即可停用「HTTP 重新導向模組」：

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

如需有關使用組態設定來停用模組的詳細資訊，請參考 [IIS \<system.webServer>](/iis/configuration/system.webServer/) ＜子元素＞** 一節中的連結。

### <a name="module-removal"></a>模組移除

如果選擇透過 *web.config* 中的設定來移除模組，請先將模組解除鎖定，以及將 *web.config* 的 `<modules>` 區段解除鎖定：

1. 將伺服器層級的模組解除鎖定。 選取「IIS 管理員」[連線]**** 資訊看板中的 IIS 伺服器。 開啟 [IIS]**** 區域中的 [模組]****。 選取清單中的模組。 在右邊的 [動作]**** 資訊看板上，選取 [解除鎖定]****。 若模組的動作項目顯示為**鎖定**，就代表該模組已經解除鎖定，且不需要任何動作。 將您打算稍後從 *web.config* 移除的模組都解除鎖定。

2. 部署應用程式，而`<modules>`不使用*web.config 中的區段。* 如果應用程式是以包含`<modules>`區段的*web.config*部署，但未在 IIS 管理員中先解除鎖定區段，則當嘗試解除鎖定區段時，Configuration Manager 會擲回例外狀況。 因此，請在沒有 `<modules>` 區段的情況下部署應用程式。

3. 解除鎖定`<modules>` *web.config*的區段。在 [**連接**] 提要欄位中，選取 [**網站**] 中的網站。 在 [管理]**** 區域中，開啟 [設定編輯器]****。 使用導覽控制項來選取 `system.webServer/modules` 區段。 在右邊的 [動作]**** 資訊看板上，選取將區段 [解除鎖定]****。 若模組區段的動作項目顯示為**鎖定區段**，就代表該模組區段已經解除鎖定，且不需要任何動作。

4. 將 `<modules>` 區段新增至具有 `<remove>` 元素的應用程式本機 *web.config* 檔案，以從應用程式移除該模組。 新增多個 `<remove>` 元素以移除多個模組。 如果已在伺服器上進行 *web.config* 變更，請立即在本機對專案的 *web.config* 檔案進行相同的變更。 使用此方法移除模組不會影響模組與伺服器上其他應用程式的搭配使用。

   ```xml
   <configuration>
    <system.webServer>
      <modules>
        <remove name="MODULE_NAME" />
      </modules>
    </system.webServer>
   </configuration>
   ```

若要使用 *web.config* 對 IIS Express 新增或移除模組，請修改 *applicationHost.config* 以解除鎖定 `<modules>` 區段：

1. 開啟 *{APPLICATION ROOT}\\.vs\config\applicationhost.config*。

1. 找出 IIS 模組的 `<section>` 元素，並將 `overrideModeDefault` 從 `Deny` 變更為 `Allow`：

   ```xml
   <section name="modules"
            allowDefinition="MachineToApplication"
            overrideModeDefault="Allow" />
   ```

1. 找出 `<location path="" overrideMode="Allow"><system.webServer><modules>` 區段。 對於您要移除的任何模組，請將 `lockItem` 從 `true` 變更為 `false`。 以下為將 CGI 模組解除鎖定的範例：

   ```xml
   <add name="CgiModule" lockItem="false" />
   ```

1. 在將 `<modules>` 區段及個別模組解除鎖定後，您可任意使用應用程式的 *web.config* 檔案新增或移除 IIS 模組，以在 IIS Express 上執行應用程式。

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

「URI 快取模組」(`UriCacheModule`) 可讓 IIS 快取 URL 層級的網站設定。 如果沒有此模組，IIS 就必須針對每個要求都讀取並剖析設定，即使是重複要求相同的 URL 時也一樣。 針對每個要求都剖析設定會導致效能大幅降低。 *雖然不一定要有「URI 快取模組」，所裝載的 ASP.NET Core 應用程式就能執行，但建議您為所有 ASP.NET Core 部署都啟用「URI 快取模組」。*

「HTTP 快取模組」(`HttpCacheModule`) 會實作 IIS 輸出快取，也會實作用來將項目快取至 HTTP.sys 快取中的邏輯。 如果沒有此模組，就不會再以核心模式快取內容，而且會忽略快取設定檔。 移除「HTTP 快取模組」通常會對效能和資源使用情況造成負面影響。 *雖然不一定要有「HTTP 快取模組」，所裝載的 ASP.NET Core 應用程式就能執行，但建議您為所有 ASP.NET Core 部署都啟用「HTTP 快取模組」。*

## <a name="additional-resources"></a>其他資源

* [IIS 架構簡介：IIS 中的模組](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS 模組概觀](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [自訂 IIS 7.0 角色和模組](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS \<system.webserver>](/iis/configuration/system.webServer/)
