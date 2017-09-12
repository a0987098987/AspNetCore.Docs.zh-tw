---
title: "在 ASP.NET Core 中設定 Windows 驗證"
author: ardalis
description: "如何在 ASP.NET Core 中設定 Windows 驗證"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定 Windows 驗證

由[Steve Smith](https://ardalis.com)

裝載於 IIS 或 WebListener ASP.NET Core 應用程式可以設定 Windows 驗證。

## <a name="what-is-windows-authentication"></a>什麼是 Windows 驗證

Windows 驗證會仰賴來驗證使用者的 ASP.NET Core 應用程式的作業系統。 使用 Active Directory 網域身分識別或其他 Windows 帳戶來識別使用者在公司網路上執行您的伺服器時，您可以使用 Windows 驗證。 Windows 驗證是驗證的安全形式最佳適合內部網路的環境，其中使用者、 用戶端應用程式，以及網頁伺服器屬於相同的 Windows 網域。

[深入了解 Windows 驗證並將它安裝為 IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>啟用 ASP.NET Core 應用程式中的 Windows 驗證

Visual Studio Web 應用程式範本可以設定為支援 Windows 驗證。

### <a name="using-the-windows-authentication-app-template"></a>使用 Windows 驗證應用程式範本

在 Visual Studio 中：
* 建立新的 ASP.NET Core Web 應用程式。 
* 從範本清單中選取 Web 應用程式。
* 選取 [變更驗證] 按鈕，然後選取**Windows 驗證**。 

執行應用程式。 使用者名稱會出現在最上層應用程式的權限。

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/browser-screenshot.png)

針對使用 IIS Express 的開發工作，範本會提供所有必要的組態，以使用 Windows 驗證。 下一節顯示如何設定 ASP.NET Core 應用程式以手動方式提供 Windows 驗證。

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Visual Studio 設定 Windows 和匿名驗證

[Visual Studio 屬性] 頁面中，偵錯索引標籤提供核取方塊進行 Windows 驗證和匿名驗證。

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/vs-auth-property-menu.png)

您也可以設定這些屬性在`launchSettings.json`檔案：

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>啟用與 IIS Windows 驗證

IIS 會使用[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)(ANCM) 來裝載 ASP.NET Core 應用程式。 ANCM 流程 Windows 驗證來 IIS 預設。 於 IIS 時，應用程式專案，會完成設定 Windows 驗證。 下列各節顯示如何使用 IIS 管理員來設定 ASP.NET Core 應用程式使用 Windows 驗證：

### <a name="create-a-new-iis-site"></a>建立新的 IIS 站台

指定的名稱和資料夾，並允許其建立新的應用程式集區。

### <a name="customize-authentication"></a>自訂驗證

開啟網站的 [驗證] 功能表。

![IIS 驗證 功能表](windowsauth/_static/iis-authentication-menu.png)

停用匿名驗證，並啟用 Windows 驗證。

![IIS 驗證設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>將專案發行到 IIS 站台資料夾

使用 Visual Studio 或.NET 核心 CLI*發行*應用程式的目的資料夾。

![Visual Studio 發行對話方塊](windowsauth/_static/vs-publish-app.png)

深入了解[發行至 IIS](xref:publishing/iis)。

啟動應用程式，以確認 Windows 驗證運作。

## <a name="enabling-windows-authentication-with-weblistener"></a>啟用 Windows 驗證與 WebListener

雖然 Kestrel 不支援 Windows 驗證，您可以使用[WebListener](xref:fundamentals/servers/weblistener)以支援在 Windows 上的自我裝載的案例。 下列範例會設定應用程式的 web 主機，以搭配 Windows 驗證使用 WebListener:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>使用 Windows 驗證

如果您的應用程式使用 Windows 驗證和匿名存取，您可以使用``[Authorize]``和``[AllowAnonymous]``屬性。 沒有不需要匿名啟用的執行的應用程式``[Authorize]``; 應用程式會被視為要求驗證，會拒絕匿名要求。 請注意，如果已設定的 IIS 站台**不**允許匿名存取，``[AllowAnonymous]``屬性會執行**不**允許匿名要求。 ``[AllowAnonymous]``屬性覆寫``[Authorize]``屬性允許匿名存取的應用程式內的用法。

### <a name="impersonation"></a>模擬

ASP.NET Core 未實作模擬。 應用程式執行與使用應用程式集區或處理序身分識別的所有要求的應用程式識別。 如果您需要明確地執行代表使用者的動作，使用``WindowsIdentity.RunImpersonated``。 在此內容中執行單一動作，然後關閉 內容。 請注意，``RunImpersonated``不支援非同步和不應該用於複雜的案例。 例如，包裝整個要求或中介軟體鏈結不支援或建議。
