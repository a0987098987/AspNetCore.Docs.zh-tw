---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 了解如何在 ASP.NET Core，使用 IIS Express、 IIS 和 HTTP.sys 中設定 Windows 驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 94dff2f47b2b076cb15f8d385239179b52786678
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637816"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定 Windows 驗證

作者：[Steve Smith](https://ardalis.com) 和 [Scott Addie](https://twitter.com/Scott_Addie)

可以使用 IIS 裝載的 ASP.NET Core 應用程式設定 Windows 驗證或[HTTP.sys](xref:fundamentals/servers/httpsys)。

## <a name="windows-authentication"></a>Windows 驗證

Windows 驗證會仰賴作業系統來驗證的 ASP.NET Core 應用程式的使用者。 使用 Active Directory 網域身分識別或其他 Windows 帳戶來識別使用者在公司網路中執行您的伺服器時，您可以使用 Windows 驗證。 Windows 驗證最適合內部網路環境中使用者、 用戶端應用程式，以及 web 伺服器屬於相同 Windows 網域。

[深入了解 Windows 驗證並將它安裝為 IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>啟用 ASP.NET Core 應用程式中的 Windows 驗證

若要支援 Windows 驗證，可以設定 Visual Studio Web 應用程式範本。

### <a name="use-the-windows-authentication-app-template"></a>使用 Windows 驗證應用程式範本

在 Visual Studio 中：

1. 建立新的 ASP.NET Core Web 應用程式。
1. 從範本清單中選取 Web 應用程式。
1. 選取 **變更驗證**按鈕，然後選取**Windows 驗證**。

執行應用程式。 使用者名稱會出現在右上方的應用程式。

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/browser-screenshot.png)

針對使用 IIS Express 的開發工作，此範本會提供所有必要的組態，若要使用 Windows 驗證。 下一節示範如何手動設定 Windows 驗證的 ASP.NET Core 應用程式。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Visual Studio 設定 Windows 和匿名驗證

Visual Studio 專案**屬性**頁面的**偵錯** 索引標籤能提供 Windows 驗證和匿名驗證的核取方塊。

![Windows 驗證的瀏覽器螢幕擷取畫面反白顯示的驗證選項](windowsauth/_static/vs-auth-property-menu.png)

或者，設定這兩個屬性，在*launchSettings.json*檔案：

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>啟用 iis 的 Windows 驗證

IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主機 ASP.NET Core 應用程式。 在 IIS 中，應用程式設定 Windows 驗證。 下列各節示範如何使用 IIS 管理員來設定 ASP.NET Core 應用程式以使用 Windows 驗證。

### <a name="iis-configuration"></a>IIS 組態

啟用 Windows 驗證的 IIS 角色服務。 如需詳細資訊，請參閱 <<c0> [ 啟用 IIS 角色服務 （請參閱步驟 2） 中的 Windows 驗證](xref:host-and-deploy/iis/index#iis-configuration)。

根據預設，IIS Integration 中介軟體會設定來自動驗證要求。 如需詳細資訊，請參閱[裝載 ASP.NET Core 與 IIS 的 Windows 上：IIS 選項 (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)。

ASP.NET Core 模組預設設定為轉送至應用程式的 Windows 驗證語彙基元。 如需詳細資訊，請參閱[ASP.NET Core 模組組態參考：AspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。

### <a name="create-a-new-iis-site"></a>建立新的 IIS 站台

指定的名稱和資料夾，並允許它建立新的應用程式集區。

### <a name="customize-authentication"></a>自訂驗證

開啟站台的驗證功能。

![IIS 驗證 功能表](windowsauth/_static/iis-authentication-menu.png)

停用匿名驗證，並啟用 Windows 驗證。

![IIS 驗證設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>將專案發佈至 IIS 的站台資料夾

使用 Visual Studio 或.NET Core CLI，將應用程式發行至目的資料夾。

![Visual Studio 發佈對話方塊](windowsauth/_static/vs-publish-app.png)

深入了解[發行至 IIS](xref:host-and-deploy/iis/index)。

啟動應用程式以確認 Windows 驗證正常運作。

## <a name="enable-windows-authentication-with-httpsys"></a>啟用 Windows 驗證，http.sys

雖然 Kestrel 不支援 Windows 驗證，您可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)支援在 Windows 上的自我裝載的案例。 下列範例會設定要搭配 Windows 驗證使用 HTTP.sys 的應用程式的 web 主機：

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

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

ASP.NET Core 不會實作模擬。 應用程式執行的所有要求，使用應用程式集區或處理序身分識別的應用程式身分識別。 如果您需要明確地執行動作的使用者身分，使用`WindowsIdentity.RunImpersonated`。 在此內容中執行單一動作，然後關閉 內容。

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

請注意，`RunImpersonated`不支援非同步作業，而不應該用於複雜的案例。 比方說，包裝整個要求或中介軟體鏈結不支援或建議。
