---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: ardalis
description: 本文說明如何在 ASP.NET Core，使用 IIS Express、 IIS、 HTTP.sys 和 WebListener 中設定 Windows 驗證。
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: dbcef095561fe656bdd28c4fa6560c6b269a2db0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689005"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定 Windows 驗證

作者：[Steve Smith](https://ardalis.com) 和 [Scott Addie](https://twitter.com/Scott_Addie)

可以針對以 IIS 裝載的 ASP.NET Core 應用程式設定 Windows 驗證[HTTP.sys](xref:fundamentals/servers/httpsys)，或[WebListener](xref:fundamentals/servers/weblistener)。

## <a name="what-is-windows-authentication"></a>什麼是 Windows 驗證？

Windows 驗證會仰賴來驗證使用者的 ASP.NET Core 應用程式的作業系統。 使用 Active Directory 網域身分識別或其他 Windows 帳戶來識別使用者在公司網路上執行您的伺服器時，您可以使用 Windows 驗證。 Windows 驗證最適合內部網路環境中的使用者、 用戶端應用程式，以及網頁伺服器屬於相同的 Windows 網域。

[深入了解 Windows 驗證並將它安裝為 IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>啟用 ASP.NET Core 應用程式中的 Windows 驗證

Visual Studio Web 應用程式範本可以設定為支援 Windows 驗證。

### <a name="use-the-windows-authentication-app-template"></a>使用 Windows 驗證應用程式範本

在 Visual Studio 中：
1. 建立新的 ASP.NET Core Web 應用程式。 
1. 從範本清單中選取 Web 應用程式。
1. 選取**變更驗證**按鈕，然後選取**Windows 驗證**。 

執行應用程式。 使用者名稱會出現在最上層應用程式的權限。

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/browser-screenshot.png)

針對使用 IIS Express 的開發工作，範本會提供所有必要的組態，以使用 Windows 驗證。 下一節顯示如何手動設定 Windows 驗證的 ASP.NET Core 應用程式。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Visual Studio 設定 Windows 和匿名驗證

Visual Studio 專案**屬性**網頁的**偵錯** 索引標籤提供 Windows 驗證和匿名驗證 核取方塊。

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/vs-auth-property-menu.png)

或者，可以設定這兩個屬性在*launchSettings.json*檔案：

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>啟用與 IIS Windows 驗證

IIS 會使用[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)主機 ASP.NET Core 應用程式。 模組可以根據預設，流向 IIS Windows 驗證。 在 IIS 中，不是應用程式設定 Windows 驗證。 下列各節將示範如何使用 IIS 管理員來設定 ASP.NET Core 應用程式使用 Windows 驗證。

### <a name="create-a-new-iis-site"></a>建立新的 IIS 站台

指定的名稱和資料夾，並允許其建立新的應用程式集區。

### <a name="customize-authentication"></a>自訂驗證

開啟網站的 [驗證] 功能表。

![IIS 驗證 功能表](windowsauth/_static/iis-authentication-menu.png)

停用匿名驗證，並啟用 Windows 驗證。

![IIS 驗證設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>將專案發行到 IIS 站台資料夾

使用 Visual Studio 或.NET 核心 CLI，將應用程式發行至目的資料夾。

![Visual Studio 發行對話方塊](windowsauth/_static/vs-publish-app.png)

深入了解[發行至 IIS](xref:host-and-deploy/iis/index)。

啟動應用程式，以確認 Windows 驗證運作。

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>啟用 Windows 驗證與 HTTP.sys 或 WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

雖然 Kestrel 不支援 Windows 驗證，您可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)以支援在 Windows 上的自我裝載的案例。 下列範例會設定 HTTP.sys 使用 Windows 驗證的應用程式的 web 主機：

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

雖然 Kestrel 不支援 Windows 驗證，您可以使用[WebListener](xref:fundamentals/servers/weblistener)以支援在 Windows 上的自我裝載的案例。 下列範例會設定應用程式的 web 主機，以搭配 Windows 驗證使用 WebListener:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>使用 Windows 驗證

匿名存取的設定狀態的方式會決定`[Authorize]`和`[AllowAnonymous]`應用程式中使用屬性。 下列兩節會說明如何處理不允許和允許設定狀態的匿名存取。

### <a name="disallow-anonymous-access"></a>不允許匿名存取

啟用 Windows 驗證，且已停用匿名存取，`[Authorize]`和`[AllowAnonymous]`沒有作用。 如果 IIS 網站 （或 HTTP.sys 或 WebListener 伺服器） 設定為不允許匿名存取，要求永遠不會到達您的應用程式。 基於這個理由，`[AllowAnonymous]`屬性不適用。

### <a name="allow-anonymous-access"></a>允許匿名存取

啟用 Windows 驗證和匿名存取，當使用`[Authorize]`和`[AllowAnonymous]`屬性。 `[Authorize]`屬性可讓您保護應用程式確實需要 Windows 驗證的片段。 `[AllowAnonymous]`屬性覆寫`[Authorize]`屬性允許匿名存取的應用程式內的用法。 請參閱[簡單授權](xref:security/authorization/simple)的屬性使用方式詳細資料。

在 ASP.NET Core 2.x，`[Authorize]`屬性需要額外的設定中*Startup.cs*挑戰 Windows 驗證的匿名要求。 建議的組態設定稍微依據所使用的 web 伺服器而異。

> [!NOTE]
> 根據預設，缺少授權存取的網頁的使用者會看到空白 HTTP 403 回應。 [StatusCodePages 中介軟體](xref:fundamentals/error-handling#configuring-status-code-pages)可以設定為使用者提供更好的 「 拒絕存取 」 體驗。

#### <a name="iis"></a>IIS

如果使用 IIS，將下列內容加入`ConfigureServices`方法： 

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

ASP.NET Core 不會實作模擬。 應用程式執行與使用應用程式集區或處理序身分識別的所有要求的應用程式識別。 如果您需要明確地執行代表使用者的動作，使用`WindowsIdentity.RunImpersonated`。 在此內容中執行單一動作，然後關閉 內容。

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

請注意，`RunImpersonated`不支援非同步作業，而且不應該用於複雜的案例。 例如，包裝整個要求或中介軟體鏈結不支援或建議。

---
