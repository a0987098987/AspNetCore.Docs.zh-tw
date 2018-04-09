---
title: 使用 ASP.NET Core 的 IIS 模組
author: guardrex
description: 探索使用中和非使用中的 IIS 模組 ASP.NET Core 應用程式，以及如何管理 IIS 模組。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: d9b3de915df333153255f91649f9169f76ba2fe0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="iis-modules-with-aspnet-core"></a>使用 ASP.NET Core 的 IIS 模組

作者：[Luke Latham](https://github.com/guardrex)

IIS 裝載 ASP.NET Core 應用程式中的反向 proxy 設定。 某些原生 IIS 模組及其所有的受管理的 IIS 模組不是可用來處理要求的 ASP.NET Core 應用程式。 在許多情況下，ASP.NET Core 可提供替代性 IIS 原生和 managed 模組的功能。

## <a name="native-modules"></a>原生模組

| Module | .NET core 作用中 | ASP.NET Core Option |
| ------ | :--------------: | ------------------- |
| **匿名驗證**<br>`AnonymousAuthenticationModule` | [是] | |
| **基本驗證**<br>`BasicAuthenticationModule` | [是] | |
| **用戶端憑證對應驗證**<br>`CertificateMappingAuthenticationModule` | [是] | |
| **CGI**<br>`CgiModule` | 否 | |
| **組態驗證**<br>`ConfigurationValidationModule` | [是] | |
| **HTTP 錯誤**<br>`CustomErrorModule` | 否 | [狀態碼頁中介軟體](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **自訂記錄**<br>`CustomLoggingModule` | [是] | |
| **預設文件**<br>`DefaultDocumentModule` | 否 | [預設的檔案中介軟體](xref:fundamentals/static-files#serve-a-default-document) |
| **摘要式驗證**<br>`DigestAuthenticationModule` | [是] | |
| **目錄瀏覽**<br>`DirectoryListingModule` | 否 | [目錄瀏覽中介軟體](xref:fundamentals/static-files#enable-directory-browsing) |
| **動態壓縮**<br>`DynamicCompressionModule` | [是] | [回應壓縮中介軟體](xref:performance/response-compression) |
| **追蹤**<br>`FailedRequestsTracingModule` | [是] | [ASP.NET Core 記錄](xref:fundamentals/logging/index#the-tracesource-provider) |
| **檔案快取**<br>`FileCacheModule` | 否 | [回應快取中介軟體](xref:performance/caching/middleware) |
| **HTTP 快取**<br>`HttpCacheModule` | 否 | [回應快取中介軟體](xref:performance/caching/middleware) |
| **HTTP 記錄**<br>`HttpLoggingModule` | [是] | [ASP.NET Core 記錄](xref:fundamentals/logging/index)<br>實作： [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)， [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)， [NLog](https://github.com/NLog/NLog.Extensions.Logging)， [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP 重新導向**<br>`HttpRedirectionModule` | [是] | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| **IIS 用戶端憑證對應驗證**<br>`IISCertificateMappingAuthenticationModule` | [是] | |
| **IP 及網域限制**<br>`IpRestrictionModule` | [是] | |
| **ISAPI 篩選器**<br>`IsapiFilterModule` | [是] | [中介軟體](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | [是] | [中介軟體](xref:fundamentals/middleware/index) |
| **通訊協定支援**<br>`ProtocolSupportModule` | [是] | |
| **要求篩選**<br>`RequestFilteringModule` | [是] | [URL 重寫中介軟體 `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **要求監視器**<br>`RequestMonitorModule` | [是] | |
| **URL 重寫**<br>`RewriteModule` | Yes&#8224; | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| **伺服器端包含**<br>`ServerSideIncludeModule` | 否 | |
| **靜態壓縮**<br>`StaticCompressionModule` | 否 | [回應壓縮中介軟體](xref:performance/response-compression) |
| **靜態內容**<br>`StaticFileModule` | 否 | [靜態檔案中介軟體](xref:fundamentals/static-files) |
| **權杖快取**<br>`TokenCacheModule` | [是] | |
| **URI 快取**<br>`UriCacheModule` | [是] | |
| **URL 授權**<br>`UrlAuthorizationModule` | [是] | [ASP.NET Core 身分識別](xref:security/authentication/identity) |
| **Windows 驗證**<br>`WindowsAuthenticationModule` | [是] | |

&#8224;URL Rewrite Module 的`isFile`和`isDirectory`相符項目類型不適用於 ASP.NET Core 應用程式，因為已變更的[目錄結構](xref:host-and-deploy/directory-structure)。

## <a name="managed-modules"></a>受管理的模組

| Module                  | .NET core 作用中 | ASP.NET Core Option |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | 否               | |
| DefaultAuthentication   | 否               | |
| FileAuthorization       | 否               | |
| FormsAuthentication     | 否               | [Cookie 驗證中介軟體](xref:security/authentication/cookie) |
| OutputCache             | 否               | [回應快取中介軟體](xref:performance/caching/middleware) |
| 設定檔                 | 否               | |
| RoleManager             | 否               | |
| ScriptModule-4.0        | 否               | |
| 工作階段                 | 否               | [工作階段中介軟體](xref:fundamentals/app-state) |
| UrlAuthorization        | 否               | |
| UrlMappingsModule       | 否               | [URL 重寫中介軟體](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | 否               | [ASP.NET Core 身分識別](xref:security/authentication/identity) |
| WindowsAuthentication   | 否               | |

## <a name="iis-manager-application-changes"></a>IIS 管理員變更應用程式

當使用 IIS 管理員來設定設定值， *web.config*應用程式的檔案變更時。 如果部署應用程式，包括*web.config*，使用 IIS 管理員進行任何變更會覆寫的已部署*web.config*檔案。 如果變更伺服器的*web.config*檔案中，複製已更新*web.config*立即的本機專案在伺服器上的檔案。

## <a name="disabling-iis-modules"></a>停用 IIS 模組

如果必須停用應用程式中，新增至應用程式的伺服器層級設定的 IIS 模組*web.config*檔案可以停用模組。 請讓模組留在原處和停用它使用組態設定 （如果有的話），或將模組從應用程式中移除。

### <a name="module-deactivation"></a>模組停用

許多模組提供可讓它們不會從應用程式移除此模組會停用的組態設定。 這是用來停用模組的最簡單且最快方式。 例如，IIS URL Rewrite Module 可以停用與 **\<httpRedirect >**中的項目*web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

如需有關如何停用模組組態設定的詳細資訊，請依照下列中的連結*子項目*區段[IIS \<system.webServer >](/iis/configuration/system.webServer/)。

### <a name="module-removal"></a>模組移除

如果選擇移除模組中的設定與*web.config*、 模組解除鎖定和解除鎖定**\<模組 >**區段*web.config*第一次：

1. 解除鎖定伺服器層級的模組。 選取 IIS 伺服器上 IIS Manager 中**連線**[資訊看板]。 開啟**模組**中**IIS**區域。 在清單中選取的模組。 在**動作**在右側，資訊看板選取**Unlock**。 解除鎖定數量的模組，當您規劃要移除*web.config*更新版本。

2. 部署應用程式不含**\<模組 >**一節中*web.config*。如果應用程式部署與*web.config*包含**\<模組 >**區段，而不需要解除鎖定區段第一次在 IIS 管理員中，Configuration Manager 就會擲回例外狀況當嘗試解除鎖定的區段。 因此，部署應用程式不含**\<模組 >** > 一節。

3. 解除鎖定**\<模組 >**區段*web.config*。在**連線**提要欄位中，選取的網站**網站**。 在**管理**區域中，開啟**組態編輯器**。 使用瀏覽控制項，來選取`system.webServer/modules`> 一節。 在**動作**來選取在右側，資訊看板**Unlock** > 一節。

4. 此時， **\<模組 >**區段可以新增至*web.config*檔案搭配**\<移除 >**從模組移除的項目應用程式。 多個**\<移除 >**項目可以加入要移除多個模組。 如果*web.config*伺服器上進行變更，立即進行相同的變更，在專案的*web.config*在本機檔案。 這種方式移除模組不會影響其他應用程式伺服器上之模組的使用。

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

IIS 安裝使用預設安裝的模組，請使用下列**\<模組 >**區段，以移除預設的模組。

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

IIS 模組也可以移除與*Appcmd.exe*。 提供`MODULE_NAME`和`APPLICATION_NAME`命令中：

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

例如，移除`DynamicCompressionModule`從預設的網站：

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>最小模組設定

只有執行 ASP.NET Core 應用程式所需的模組是匿名驗證模組和 ASP.NET 核心模組。

![IIS 管理員 中開啟模組顯示的最小模組設定](modules/_static/modules.png)

## <a name="additional-resources"></a>其他資源

* [ Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)
* [IIS 架構簡介： 在 IIS 中的模組](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS 模組概觀](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [自訂的 IIS 7.0 角色和模組](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
