---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 瞭解如何在 IIS 和 HTTP.sys 的 ASP.NET Core 中設定 Windows 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/26/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/windowsauth
ms.openlocfilehash: 8f6dc8620df04bcebe996119869ca2e498cffccc
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "87330676"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定 Windows 驗證

作者：[Scott Addie](https://twitter.com/Scott_Addie)

::: moniker range=">= aspnetcore-3.0"

Windows 驗證（也稱為 Negotiate、Kerberos 或 NTLM 驗證）可以針對以[IIS](xref:host-and-deploy/iis/index)、 [Kestrel](xref:fundamentals/servers/kestrel)或[HTTP.sys](xref:fundamentals/servers/httpsys)裝載的 ASP.NET Core 應用程式進行設定。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Windows 驗證（也稱為 Negotiate、Kerberos 或 NTLM 驗證）可以針對使用[IIS](xref:host-and-deploy/iis/index)或[HTTP.sys](xref:fundamentals/servers/httpsys)的 ASP.NET Core 應用程式進行設定。

::: moniker-end

Windows 驗證依賴作業系統來驗證 ASP.NET Core 應用程式的使用者。 當您的伺服器使用 Active Directory 網域身分識別或 Windows 帳戶在公司網路上執行時，您可以使用 Windows 驗證來識別使用者。 Windows 驗證最適合內部網路環境，其中使用者、用戶端應用程式和網頁伺服器都屬於相同的 Windows 網域。

> [!NOTE]
> HTTP/2 不支援 Windows 驗證。 驗證挑戰可以透過 HTTP/2 回應來傳送，但是用戶端必須先降級為 HTTP/1.1，才能進行驗證。

## <a name="proxy-and-load-balancer-scenarios"></a>Proxy 和負載平衡器案例

Windows 驗證是主要用於內部網路的具狀態案例，proxy 或負載平衡器通常不會處理用戶端與伺服器之間的流量。 如果使用 proxy 或負載平衡器，Windows 驗證僅適用于 proxy 或負載平衡器：

* 處理驗證。
* 將使用者驗證資訊傳遞給應用程式（例如，在要求標頭中），其作用於驗證資訊。

在使用 proxy 和負載平衡器的環境中，Windows 驗證的替代方法是使用 OpenID Connect （OIDC） Active Directory 同盟服務（ADFS）。

## <a name="iisiis-express"></a>IIS/IIS Express

藉由 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> <xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> 在中叫用（命名空間）來新增驗證服務 `Startup.ConfigureServices` ：

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a>啟動設定（偵錯工具）

啟動設定的設定只會影響 IIS Express 的檔案*屬性/launchSettings.js* ，而且不會針對 Windows 驗證設定 IIS。 伺服器設定會在[IIS](#iis)一節中說明。

透過 Visual Studio 或 .NET Core CLI 提供的**Web 應用程式**範本可以設定為支援 Windows 驗證，這會自動更新檔案的*屬性/launchSettings.js* 。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

**新增專案**

1. 建立新專案。
1. 選取 **ASP.NET Core Web 應用程式**。 選取 [下一步]  。
1. 在 [**專案名稱**] 欄位中提供名稱。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]。
1. 選取 [**驗證**] 底下的 [**變更**]。
1. 在 [**變更驗證**] 視窗中，選取 [ **Windows 驗證**]。 選取 [確定]。
1. 選取 [ **Web 應用程式**]。
1. 選取 [建立]。

執行應用程式。 使用者名稱會出現在轉譯的應用程式的使用者介面中。

**現有專案**

專案的屬性會啟用 Windows 驗證並停用匿名驗證：

1. 以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [**屬性**]。
1. 選取 [偵錯]**** 索引標籤。
1. 清除 [**啟用匿名驗證**] 的核取方塊。
1. 選取 [**啟用 Windows 驗證**] 的核取方塊。
1. 儲存並關閉屬性頁。

或者，您也可以在檔案 `iisSettings` *上launchSettings.js*的節點中設定屬性：

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

**新增專案**

使用[dotnet new](/dotnet/core/tools/dotnet-new) `webapp` 引數（ASP.NET Core Web 應用程式）和交換器來執行 dotnet new 命令 `--auth Windows` ：

```dotnetcli
dotnet new webapp --auth Windows
```

**現有專案**

更新檔案 `iisSettings` *上launchSettings.js*的節點：

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

修改現有的專案時，請確認專案檔包含[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)**或** [AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet 套件的套件參考。

### <a name="iis"></a>IIS

IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)來裝載 ASP.NET Core 應用程式。 Windows 驗證是透過*web.config*檔案來設定 IIS。 下列各節將示範如何：

* 提供在部署應用程式時，在伺服器上啟動 Windows 驗證的本機*web.config*檔案。
* 使用 IIS 管理員來設定已部署至伺服器之 ASP.NET Core 應用程式的*web.config*檔案。

如果您尚未這麼做，請啟用 IIS 來裝載 ASP.NET Core 應用程式。 如需詳細資訊，請參閱 <xref:host-and-deploy/iis/index>。

啟用 IIS 角色服務以進行 Windows 驗證。 如需詳細資訊，請參閱[在 IIS 角色服務中啟用 Windows 驗證（請參閱步驟2）](xref:host-and-deploy/iis/index#iis-configuration)。

[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)會設定為預設自動驗證要求。 如需詳細資訊，請參閱[在 Windows 上使用 iis 的主機 ASP.NET Core： iis 選項（AutomaticAuthentication）](xref:host-and-deploy/iis/index#iis-options)。

預設會將 ASP.NET Core 模組設定為將 Windows 驗證權杖轉送至應用程式。 如需詳細資訊，請參閱[ASP.NET Core 模組設定參考： aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

請使用下列**其中一**種方法：

* **發行和部署專案之前，請**將下列*web.config*檔案加入至專案根目錄：

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  當 .NET Core SDK 發行專案時（ `<IsTransformWebConfigDisabled>` 在專案檔中未將屬性設定為 `true` ），已發行的*web.config*檔案會包含 `<location><system.webServer><security><authentication>` 區段。 如需屬性的詳細資訊 `<IsTransformWebConfigDisabled>` ，請參閱 <xref:host-and-deploy/iis/index#webconfig-file> 。

* **發行和部署專案之後，請**使用 IIS 管理員來執行伺服器端設定：

  1. 在 [IIS 管理員] 中，**選取 [連線] 提要欄位**的 [**網站**] 節點底下的 IIS 網站。
  1. 按兩下 [ **IIS** ] 區域中的 [**驗證**]。
  1. 選取 [**匿名驗證**]。 在 [**動作**] 提要欄位中選取 [**停**用]。
  1. 選取 **[Windows 驗證]**。 選取 [**動作**] 提要欄位中的 [**啟用**]。

  當採取這些動作時，IIS 管理員會修改應用程式的*web.config*檔案。 `<system.webServer><security><authentication>`加入的節點具有和的更新設定 `anonymousAuthentication` `windowsAuthentication` ：

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  `<system.webServer>`IIS 管理員新增至*web.config*檔案的區段，是在 `<location>` 發佈應用程式時，由 .NET Core SDK 新增的應用程式區段外。 因為區段會新增 `<location>` 至節點外部，所以所有[子應用](xref:host-and-deploy/iis/index#sub-applications)程式都會將這些設定繼承至目前的應用程式。 若要防止繼承，請將新增的區段移至 `<security>` `<location><system.webServer>` .NET Core SDK 提供的區段內。

  當使用 IIS 管理員來新增 IIS 設定時，它只會影響應用程式在伺服器上的*web.config*檔案。 如果伺服器的*web.config*複本已由專案的*web.config*檔取代，應用程式的後續部署可能會覆寫伺服器上的設定。 使用下列**其中一**種方法來管理設定：

  * 在部署之後覆寫檔案之後，請使用 IIS 管理員來重設*web.config*檔案中的設定。
  * 使用設定在本機將*web.config*檔案新增至應用程式。

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a>Kestrel

[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)可以與[Kestrel](xref:fundamentals/servers/kestrel)搭配使用，以支援在 Windows、Linux 和 macOS 上使用 Negotiate 和 Kerberos 來進行 Windows 驗證。

> [!WARNING]
> 認證可以跨連接的要求保存。 *除非 proxy 使用 Kestrel 維護1:1 連線親和性（持續連線），否則不能將 Negotiate 驗證與 proxy 搭配使用。*

> [!NOTE]
> Negotiate 處理常式會偵測基礎伺服器是否以原生方式支援 Windows 驗證，以及是否已啟用。 如果伺服器支援 Windows 驗證但已停用，則會擲回錯誤，要求您啟用伺服器執行。 在伺服器中啟用 Windows 驗證時，Negotiate 處理常式會以透明方式轉寄給它。

藉由在中叫用和來新增驗證服務 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> <xref:Microsoft.Extensions.DependencyInjection.NegotiateExtensions.AddNegotiate*> `Startup.ConfigureServices` ：

 ```csharp
// using Microsoft.AspNetCore.Authentication.Negotiate;
// using Microsoft.Extensions.DependencyInjection;

services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

藉由呼叫中的來新增驗證中介軟體 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> `Startup.Configure` ：

 ```csharp
app.UseAuthentication();
```

如需中介軟體的詳細資訊，請參閱 <xref:fundamentals/middleware/index> 。

允許匿名要求。 使用[ASP.NET Core 授權](xref:security/authorization/introduction)來挑戰匿名要求以進行驗證。

### <a name="windows-environment-configuration"></a>Windows 環境設定

[AspNetCore. Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)元件會執行使用者模式驗證。 必須將服務主體名稱（Spn）新增至執行服務的使用者帳戶，而不是電腦帳戶。 `setspn -S HTTP/myservername.mydomain.com myuser`在系統管理命令 shell 中執行。

### <a name="linux-and-macos-environment-configuration"></a>Linux 和 macOS 環境設定

將 Linux 或 macOS 機器加入 Windows 網域的指示，請參閱[使用 Windows 驗證將 Azure Data Studio 連接到您的 SQL Server-Kerberos 一](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller)文。 指示會在網域上建立 Linux 機器的電腦帳戶。 Spn 必須新增至該機器帳戶。

> [!NOTE]
> 遵循[使用 Windows 驗證將 Azure Data Studio 連接到您的 SQL Server-Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller)文章中的指導方針時，如有需要，請將取代 `python-software-properties` 為 `python3-software-properties` 。

一旦 Linux 或 macOS 機器加入網域之後，需要額外的步驟，才能提供具有 Spn 的[keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)檔案：

* 在網域控制站上，將新的 web 服務 Spn 新增至電腦帳戶：
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* 使用[ktpass](/windows-server/administration/windows-commands/ktpass)來產生 keytab 檔案：
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * 某些欄位必須以大寫指定，如所示。
* 將 keytab 檔案複製到 Linux 或 macOS 機器。
* 透過環境變數選取 keytab 檔案：`export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`
* 叫 `klist` 用以顯示目前可供使用的 spn。

> [!NOTE]
> Keytab 檔案包含網域存取認證，必須據以進行保護。

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys)支援使用 NEGOTIATE、NTLM 或基本驗證的核心模式 Windows 驗證。

藉由 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> 在中叫用（命名空間）來新增驗證服務 `Startup.ConfigureServices` ：

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

將應用程式的 web 主機設定為使用 HTTP.sys 搭配 Windows 驗證（*Program.cs*）。 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*>位於 <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> 命名空間中。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。 Kerberos 和 HTTP.sys 不支援使用者模式驗證。 必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。 請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。

> [!NOTE]
> Nano Server 1709 版或更新版本不支援 HTTP.sys。 若要搭配 Nano Server 使用 Windows 驗證和 HTTP.sys，請使用[Server Core （microsoft/windowsservercore）容器](https://hub.docker.com/r/microsoft/windowsservercore/)。 如需 Server Core 的詳細資訊，請參閱[Windows server 中的 Server core 安裝選項是什麼？](/windows-server/administration/server-core/what-is-server-core)。

## <a name="authorize-users"></a>授權使用者

[匿名存取] 的設定狀態會決定在 `[Authorize]` `[AllowAnonymous]` 應用程式中使用和屬性的方式。 下列兩節說明如何處理匿名存取不允許和允許的設定狀態。

### <a name="disallow-anonymous-access"></a>不允許匿名存取

啟用 Windows 驗證並停用匿名存取時， `[Authorize]` 和 `[AllowAnonymous]` 屬性不會有任何作用。 如果 IIS 網站設定為不允許匿名存取，則要求永遠不會到達應用程式。 基於這個理由，此 `[AllowAnonymous]` 屬性不適用。

### <a name="allow-anonymous-access"></a>允許匿名存取

同時啟用 Windows 驗證和匿名存取時，請使用 `[Authorize]` 和 `[AllowAnonymous]` 屬性。 `[Authorize]`屬性可讓您保護需要驗證之應用程式的端點。 `[AllowAnonymous]`屬性會覆寫 `[Authorize]` 應用程式中允許匿名存取的屬性。 如需屬性使用方式的詳細資訊，請參閱 <xref:security/authorization/simple> 。

> [!NOTE]
> 根據預設，缺少存取頁面授權的使用者會看到空白的 HTTP 403 回應。 您可以設定[StatusCodePages 中介軟體](xref:fundamentals/error-handling#usestatuscodepages)，為使用者提供更好的「拒絕存取」體驗。

## <a name="impersonation"></a>模擬

ASP.NET Core 不會執行模擬。 應用程式會使用應用程式集區或進程身分識別，針對所有要求以應用程式的身分識別執行。 如果應用程式應該代表使用者執行動作，請在的[終端機內嵌中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)中使用[WindowsIdentity. RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) 。 `Startup.Configure` 在此內容中執行單一動作，然後關閉內容。

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated`不支援非同步作業，且不應用於複雜的案例。 例如，不支援或不建議將整個要求或中介軟體鏈換行。

::: moniker range=">= aspnetcore-3.0"

雖然[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)會在 Windows、Linux 和 macOS 上啟用驗證，但只有在 windows 上才支援模擬。

::: moniker-end

## <a name="claims-transformations"></a>宣告轉換

::: moniker range=">= aspnetcore-3.0"

使用 IIS 裝載時， <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 不會在內部呼叫來初始化使用者。 因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。 如需啟用宣告轉換的詳細資訊和程式碼範例，請參閱 <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> 。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

使用 IIS 同進程模式裝載時， <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 不會在內部呼叫來初始化使用者。 因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。 如需在裝載同進程時啟動宣告轉換的詳細資訊和程式碼範例，請參閱 <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> 。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
