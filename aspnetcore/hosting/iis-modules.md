---
title: "使用 IIS 模組與 ASP.NET Core"
author: guardrex
description: "說明 ASP.NET Core 應用程式的作用中和非使用中的 IIS 模組的參考文件。"
keywords: "ASP.NET Core iis、 模組、 反向 proxy"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: afad266874d3ac059d9f3a6d26a5330a0006320b
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>使用 IIS 模組與 ASP.NET Core

由[Luke Latham](https://github.com/guardrex)

由 IIS 所裝載 ASP.NET Core 應用程式中的反向 proxy 設定。 原生 IIS 模組的某些和所有 IIS 管理模組並非可用來處理要求的 ASP.NET Core 應用程式。 在許多情況下，ASP.NET Core 可提供替代性 IIS 原生和 managed 模組的功能。

## <a name="native-modules"></a>原生模組
模組 | .NET core 作用中 | ASP.NET Core 選項
--- | :---: | ---
**匿名驗證**<br>`AnonymousAuthenticationModule` | 是 | 
**基本驗證**<br>`BasicAuthenticationModule` | 是 | 
**用戶端憑證對應驗證**<br>`CertificateMappingAuthenticationModule` | 是 | 
**CGI**<br>`CgiModule` | 否 | 
**組態驗證**<br>`ConfigurationValidationModule` | 是 | 
**HTTP 錯誤**<br>`CustomErrorModule` | 否 | [狀態碼頁中介軟體](xref:fundamentals/error-handling#configuring-status-code-pages)
**自訂記錄**<br>`CustomLoggingModule` | 是 | 
**預設文件**<br>`DefaultDocumentModule` | 否 | [預設的檔案中介軟體](xref:fundamentals/static-files#serving-a-default-document)
**摘要式驗證**<br>`DigestAuthenticationModule` | 是 | 
**目錄瀏覽**<br>`DirectoryListingModule` | 否 | [目錄瀏覽中介軟體](xref:fundamentals/static-files#enabling-directory-browsing)
**動態壓縮**<br>`DynamicCompressionModule` | 是 | [回應壓縮中介軟體](xref:performance/response-compression)
**追蹤**<br>`FailedRequestsTracingModule` | 是 | [ASP.NET Core 記錄](xref:fundamentals/logging#the-tracesource-provider)
**檔案快取**<br>`FileCacheModule` | 否 | [回應快取中介軟體](xref:performance/caching/middleware)
**HTTP 快取**<br>`HttpCacheModule` | 否 | [回應快取中介軟體](xref:performance/caching/middleware)
**HTTP 記錄**<br>`HttpLoggingModule` | 是 | [ASP.NET Core 記錄](xref:fundamentals/logging)<br>實作： [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging)， [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)， [NLog](https://github.com/NLog/NLog.Extensions.Logging)， [Serilog](https://github.com/serilog/serilog-extensions-logging)
**HTTP 重新導向**<br>`HttpRedirectionModule` | 是 | [URL 重寫中介軟體](xref:fundamentals/url-rewriting)
**IIS 用戶端憑證對應驗證**<br>`IISCertificateMappingAuthenticationModule` | 是 | 
**IP 及網域限制**<br>`IpRestrictionModule` | 是 | 
**ISAPI 篩選器**<br>`IsapiFilterModule` | 是 | [中介軟體](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | 是 | [中介軟體](xref:fundamentals/middleware)
**通訊協定支援**<br>`ProtocolSupportModule` | 是 | 
**要求篩選**<br>`RequestFilteringModule` | 是 | [URL 重寫中介軟體`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**要求監視器**<br>`RequestMonitorModule` | 是 | 
**URL 重寫**<br>`RewriteModule` | Yes† | [URL 重寫中介軟體](xref:fundamentals/url-rewriting)
**伺服器端包含**<br>`ServerSideIncludeModule` | 否 | 
**靜態壓縮**<br>`StaticCompressionModule` | 否 | [回應壓縮中介軟體](xref:performance/response-compression)
**靜態內容**<br>`StaticFileModule` | 否 | [靜態檔案中介軟體](xref:fundamentals/static-files)
**權杖快取**<br>`TokenCacheModule` | 是 | 
**URI 快取**<br>`UriCacheModule` | 是 | 
**URL 授權**<br>`UrlAuthorizationModule` | 是 | [ASP.NET Core 身分識別](xref:security/authentication/identity)
**Windows 驗證**<br>`WindowsAuthenticationModule` | 是 | 

†The URL Rewrite Module 的`isFile`和`isDirectory`無法搭配 ASP.NET Core 應用程式，因為變更[目錄結構](xref:hosting/directory-structure)。

## <a name="managed-modules"></a>受管理的模組
模組 | .NET core 作用中 | ASP.NET Core 選項
--- | :---: | ---
AnonymousIdentification | 否 | 
DefaultAuthentication | 否 | 
FileAuthorization | 否 | 
FormsAuthentication | 否 | [Cookie 驗證中介軟體](xref:security/authentication/cookie)
OutputCache | 否 | [回應快取中介軟體](xref:performance/caching/middleware)
設定檔 | 否 | 
RoleManager | 否 | 
ScriptModule 4.0 | 否 | 
工作階段 | 否 | [工作階段中介軟體](xref:fundamentals/app-state)
UrlAuthorization | 否 | 
UrlMappingsModule | 否 | [URL 重寫中介軟體](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | 否 | [ASP.NET Core 身分識別](xref:security/authentication/identity)
WindowsAuthentication | 否 | 

## <a name="iis-manager-application-changes"></a>IIS 管理員變更應用程式
當您使用 IIS 管理員進行設定時，要直接變更*web.config*應用程式的檔案。 如果您部署您的應用程式，包括*web.config*，您使用 IIS 管理員所做的變更將會覆寫的已部署*web.config 檔案*。 因此如果您變更伺服器的*web.config*檔案中，複製已更新*web.config*立即本機專案的檔案。

## <a name="disabling-iis-modules"></a>停用 IIS 模組
如果您有想要停用應用程式的伺服器層級設定的 IIS 模組，您可以新增至與您*web.config*檔案。 請讓模組留在原處和停用它使用組態設定 （如果有的話），或將模組從應用程式中移除。

### <a name="module-deactivation"></a>模組停用
許多模組提供可讓您停用它們，而不需移除應用程式的組態設定。 這是用來停用模組的最簡單且最快方式。 例如，如果您想要停用 IIS URL Rewrite 模組，請使用`<httpRedirect>`如下所示的項目。 如需有關如何停用模組組態設定的詳細資訊，請依照下列中的連結*子項目*區段[IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/)。

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>模組移除
如果您選擇的設定中移除模組*web.config*，您必須解除鎖定該模組，並解除鎖定`<modules>`區段*web.config*第一次。 步驟如下所述：

1. 解除鎖定伺服器層級的模組。 在 IIS 管理員中的 IIS 伺服器上按一下**連線**[資訊看板]。 開啟**模組**中**IIS**區域。 按一下清單中的模組。 在**動作**提要欄位的右邊按一下**Unlock**。 解除鎖定數量的模組，當您規劃要移除與*web.config*更新版本。

2. 部署應用程式而沒有`<modules>`一節中*web.config*。如果您部署應用程式，但*web.config*包含`<modules>`區段，而不需要解除鎖定區段第一次在 IIS 管理員中，Configuration Manager 將會擲回例外狀況，當您嘗試解除鎖定的區段。 因此，部署應用程式而沒有`<modules>`> 一節。

3. 解除鎖定`<modules>`區段*web.config*。在**連線**提要欄位中，按一下中的網站**網站**。 在**管理**區域中，開啟**組態編輯器**。 使用瀏覽控制項，來選取`system.webServer/modules`> 一節。 在**動作**[資訊看板]，在右側，按一下以**Unlock** > 一節。

4. 此時，您可以在此處新增`<modules>`區段您*web.config*檔案搭配`<remove>`模組從應用程式中移除的項目。 您可以加入多個`<remove>`移除多個模組的項目。 請記住，如果您進行*web.config*以立即進行中的專案在本機伺服器上的變更。 這種方式移除模組不會影響您使用與其他應用程式伺服器上的模組。

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

IIS 安裝使用預設安裝的模組中，您可以使用下列`<module>`區段，以移除預設的模組。

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

您也可以移除的 IIS 模組*Appcmd.exe*。 提供`MODULE_NAME`和`APPLICATION_NAME`命令如下所示：

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

以下是如何移除`DynamicCompressionModule`從預設的網站：

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>最小模組設定
只有執行 ASP.NET Core 應用程式所需的模組是匿名驗證模組和 ASP.NET 核心模組。

![IIS 管理員 中開啟模組顯示的最小模組設定](iis-modules/_static/modules.png)

## <a name="resources"></a>資源
* [發行至 IIS](xref:publishing/iis)
* [IIS 模組概觀](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [自訂的 IIS 7.0 角色和模組](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
