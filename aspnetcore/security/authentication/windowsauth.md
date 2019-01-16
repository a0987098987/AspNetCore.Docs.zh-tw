---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 了解如何在 ASP.NET Core，使用 IIS Express、 IIS 和 HTTP.sys 中設定 Windows 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 01/15/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: c98bdedcf943a9057c96a8e5d62615e400074899
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341650"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定 Windows 驗證

藉由[Scott Addie](https://twitter.com/Scott_Addie)和[Luke Latham](https://github.com/guardrex)

[Windows 驗證](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)可以設定與裝載的 ASP.NET Core 應用程式[IIS](xref:host-and-deploy/iis/index)或是[HTTP.sys](xref:fundamentals/servers/httpsys)。

Windows 驗證會仰賴作業系統來驗證的 ASP.NET Core 應用程式的使用者。 使用 Active Directory 網域身分識別或 Windows 帳戶，來識別使用者在公司網路中執行您的伺服器時，您可以使用 Windows 驗證。 Windows 驗證最適合內部網路的環境，其中使用者、 用戶端應用程式，以及網頁伺服器都屬於相同 Windows 網域。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>啟用 ASP.NET Core 應用程式中的 Windows 驗證

**Web 應用程式**可透過 Visual Studio 或.NET Core CLI 的範本可以設定為支援 Windows 驗證。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>新的專案中使用 Windows 驗證應用程式範本

在 Visual Studio 中：

1. 建立新**ASP.NET Core Web 應用程式**。
1. 選取  **Web 應用程式**從範本清單。
1. 選取 **變更驗證**按鈕，然後選取**Windows 驗證**。

執行應用程式。 使用者名稱會出現在呈現的應用程式使用者介面。

### <a name="manual-configuration-for-an-existing-project"></a>手動設定現有的專案

專案的屬性可讓您以啟用 Windows 驗證並停用匿名驗證：

1. 在 Visual Studio 的專案上按一下滑鼠右鍵**方案總管**，然後選取**屬性**。
1. 選取 [偵錯] 索引標籤。
1. 清除核取方塊**啟用匿名驗證**。
1. 選取核取方塊**啟用 Windows 驗證**。

或者，設定屬性，在`iisSettings`的節點*launchSettings.json*檔案：

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用**Windows 驗證**應用程式範本。

執行[dotnet 新](/dotnet/core/tools/dotnet-new)命令搭配`webapp`引數 （ASP.NET Core Web 應用程式） 和`--auth Windows`切換：

```console
dotnet new webapp --auth Windows
```

---

當修改現有的專案，請確認專案檔包含的套件參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)**或是** [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet 套件。

## <a name="enable-windows-authentication-with-iis"></a>啟用 iis 的 Windows 驗證

IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主機 ASP.NET Core 應用程式。 Windows 驗證針對透過 IIS *web.config*檔案。 下列各節將示範如何：

* 提供本機*web.config*部署應用程式時，請在伺服器啟動 Windows 驗證的檔案。
* 使用 IIS 管理員設定*web.config*已經部署到伺服器的 ASP.NET Core 應用程式的檔案。

### <a name="iis-configuration"></a>IIS 組態

如果您尚未這麼做，請啟用 IIS 可裝載 ASP.NET Core 應用程式。 如需詳細資訊，請參閱<xref:host-and-deploy/iis/index>。

啟用 Windows 驗證的 IIS 角色服務。 如需詳細資訊，請參閱 <<c0> [ 啟用 IIS 角色服務 （請參閱步驟 2） 中的 Windows 驗證](xref:host-and-deploy/iis/index#iis-configuration)。

根據預設，IIS Integration 中介軟體會設定來自動驗證要求。 如需詳細資訊，請參閱[裝載 ASP.NET Core 與 IIS 的 Windows 上：IIS 選項 (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)。

ASP.NET Core 模組預設設定為轉送至應用程式的 Windows 驗證語彙基元。 如需詳細資訊，請參閱[ASP.NET Core 模組組態參考：AspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

### <a name="create-a-new-iis-site"></a>建立新的 IIS 站台

指定的名稱和資料夾，並允許它建立新的應用程式集區。

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>啟用 IIS 中的應用程式的 Windows 驗證

使用**任一**下列其中一個方法：

* [在之前發佈的應用程式的開發後端設定](#development-side-configuration-with-a-local-webconfig-file)(*建議*)
* [之後發佈的應用程式的伺服器端設定](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>使用本機 web.config 檔案的開發後端設定

執行下列步驟**之前**您[發佈和部署您的專案](#publish-and-deploy-your-project-to-the-iis-site-folder)。

新增下列*web.config*至專案根目錄的檔案：

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

專案 sdk 的發行時 (不含`<IsTransformWebConfigDisabled>`屬性設定為`true`專案檔中)，發行*web.config*檔案包含`<location><system.webServer><security><authentication>`一節。 如需詳細資訊`<IsTransformWebConfigDisabled>`屬性，請參閱<xref:host-and-deploy/iis/index#webconfig-file>。

#### <a name="server-side-configuration-with-the-iis-manager"></a>伺服器端設定使用 IIS 管理員

執行下列步驟**之後**您[發佈和部署您的專案](#publish-and-deploy-your-project-to-the-iis-site-folder)。

1. 在 [IIS 管理員] 中，選取 IIS 站台之下**站台**節點**連線**資訊看板。
1. 按兩下**驗證**中**IIS**區域。
1. 選取 **匿名驗證**。 選取 **停用**中**動作**資訊看板。
1. 選取  **Windows 驗證**。 選取 **啟用**中**動作**資訊看板。

IIS 管理員在採取這些動作，會修改應用程式的*web.config*檔案。 A`<system.webServer><security><authentication>`節點新增與更新的設定，如`anonymousAuthentication`和`windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

`<system.webServer>`區段新增至*web.config*由 IIS 管理員中的檔案超出應用程式的`<location>`發佈應用程式時，由.NET Core SDK 加入的區段。 因為區段會新增外部`<location>`節點，設定會由任何繼承[子應用程式](xref:host-and-deploy/iis/index#sub-applications)目前的應用程式。 若要防止繼承，移動加入`<security>`區段內的`<location><system.webServer>`SDK 所提供的一節。

將 IIS 設定使用 IIS 管理員時，它只會影響應用程式的*web.config*伺服器上的檔案。 後續部署應用程式可能會覆寫伺服器上的設定，如果伺服器的複本*web.config*專案的取代*web.config*檔案。 使用**任一**下列其中一個方法來管理設定：

* 使用 IIS 管理員中的設定重設*web.config*檔案之後部署上覆寫該檔案。
* 新增*web.config 檔案*應用程式在本機使用的設定。 如需詳細資訊，請參閱 <<c0> [ 開發後端設定](#development-side-configuration-with-a-local-webconfig-file)一節。

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>發行，並將專案部署至 IIS 的站台資料夾

使用 Visual Studio 或.NET Core CLI，發行應用程式並部署到目的資料夾。

如需有關如何使用 IIS 裝載的詳細資訊，發行和部署，請參閱下列主題：

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

啟動應用程式以確認 Windows 驗證正常運作。

## <a name="enable-windows-authentication-with-httpsys"></a>啟用 Windows 驗證，http.sys

雖然 Kestrel 不支援 Windows 驗證，您可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)支援在 Windows 上的自我裝載的案例。 下列範例會設定要搭配 Windows 驗證使用 HTTP.sys 的應用程式的 web 主機：

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。 Kerberos 和 HTTP.sys 不支援使用者模式驗證。 必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。 請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。

> [!NOTE]
> HTTP.sys 不支援 Nano Server 1709 版或更新版本上。 若要使用 Windows 驗證和 HTTP.sys 使用 Nano Server，請使用[Server Core (microsoft/windowsservercore) 容器](https://hub.docker.com/r/microsoft/windowsservercore/)。 如需有關 Server Core 的詳細資訊，請參閱[什麼是 Windows Server 中的 Server Core 安裝選項？](/windows-server/administration/server-core/what-is-server-core)。

## <a name="work-with-windows-authentication"></a>使用 Windows 驗證

匿名存取的設定狀態決定的方式`[Authorize]`和`[AllowAnonymous]`應用程式中使用屬性。 下列兩節會說明如何處理不允許和允許設定狀態的匿名存取。

### <a name="disallow-anonymous-access"></a>不允許匿名存取

當您啟用 Windows 驗證，並已停用匿名存取，`[Authorize]`和`[AllowAnonymous]`屬性沒有任何作用。 如果 IIS 網站 （或 HTTP.sys） 設定為不允許匿名存取，要求永遠不會到達您的應用程式。 基於這個理由，`[AllowAnonymous]`屬性不適用。

### <a name="allow-anonymous-access"></a>允許匿名存取

當啟用 Windows 驗證和匿名存取時，使用`[Authorize]`和`[AllowAnonymous]`屬性。 `[Authorize]`屬性可讓您安全的應用程式真正需要 Windows 驗證的項目。 `[AllowAnonymous]`屬性覆寫`[Authorize]`屬性允許匿名存取的應用程式內的使用方式。 請參閱[簡單授權](xref:security/authorization/simple)屬性使用方式詳細資料。

在 ASP.NET Core 2.x 中，`[Authorize]`屬性需要額外的設定，在*Startup.cs*挑戰進行 Windows 驗證的匿名要求。 建議的設定會有些許出入所使用的 web 伺服器而有所不同。

> [!NOTE]
> 根據預設，缺少授權，才能存取頁面的使用者會看到空的 HTTP 403 回應。 [StatusCodePages 中介軟體](xref:fundamentals/error-handling#configure-status-code-pages)可以設定為使用者提供更好的 「 拒絕存取 」 體驗。

#### <a name="iis"></a>IIS

如果使用 IIS，請將下列內容加入`ConfigureServices`方法：

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

如果使用 HTTP.sys，將下列內容加入`ConfigureServices`方法：

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>模擬

ASP.NET Core 不會實作模擬。 應用程式執行的所有要求，使用應用程式集區或處理序身分識別的應用程式的身分識別。 如果您需要明確地執行動作的使用者身分，使用[WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)中[終端機內嵌中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)在`Startup.Configure`。 在此內容中執行單一動作，然後關閉 內容。

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` 不支援非同步作業，而不應該用於複雜的案例。 比方說，包裝整個要求或中介軟體鏈結不支援或建議。
