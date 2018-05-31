---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: shirhatti
description: 了解當 ASP.NET Core 應用程式在 Windows Server 上的 IIS 後方執行時，對其提供的偵錯支援。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233075"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core 中的開發階段 IIS 支援

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti) 和 [Luke Latham](https://github.com/guardrex)

本文針對在 Windows Server 上 IIS 後方執行的 ASP.NET Core 應用程式，說明 [Visual Studio](https://www.visualstudio.com/vs/) 的偵錯支援。 本主題會逐步解說如何啟用這項功能及設定專案。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>啟用 IIS

1. 瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。
1. 選取 [Internet Information Services] 核取方塊。

![[Windows 功能] 會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

安裝 IIS 可能需要重新啟動系統。

## <a name="configure-iis"></a>設定 IIS

IIS 的網站必須含有下列設定：

* 與應用程式啟動設定檔 URL 主機名稱相符的主機名稱。
* 搭配所指派憑證的連接埠 443 繫結。

例如，所新增網站的 [主機名稱] 會設定為 "localhost" (啟動設定檔稍後在本主題中也會使用 "localhost")。 連接埠會設定為 "443" (HTTPS)。 已將 [IIS Express 開發憑證] 指派給網站，但可使用任何有效的憑證：

![IIS 中的 [新增網站] 視窗，其中顯示具有已指派的憑證並在連接埠 443 上 localhost 的繫結集合。](development-time-iis-support/_static/add-website-window.png)

如果 IIS 安裝已經有主機名稱與應用程式啟動設定檔 URL 主機名稱相符的「預設網站」：

* 為連接埠 443 (HTTPS) 新增連接埠繫結。
* 指派一個有效的憑證給網站。

## <a name="enable-development-time-iis-support-in-visual-studio"></a>在 Visual Studio 中啟用開發階段 IIS 支援

1. 啟動 Visual Studio 安裝程式。
1. 選取 [開發階段 IIS 支援] 元件。 此元件在 [ASP.NET 與網頁程式開發] 工作負載的 [摘要] 面板中，會列為選擇性元件。 此元件會安裝 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，這是在反向 Proxy 設定中，於 IIS 後方執行 ASP.NET Core 應用程式所需的原生 IIS 模組。

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。 在 [Web and Cloud]\(Web 與雲端\) 區段中，選取 [ASP.NET 與網頁程式開發] 面板。 在右側 [摘要] 面板的 [選擇性] 區域中，有 [開發階段 IIS 支援] 的核取方塊。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>設定專案

### <a name="https-redirection"></a>HTTPS 重新導向

針對新的專案，請在 [新增 ASP.NET Core Web 應用程式] 視窗中，選取 [針對 HTTPS 進行設定] 核取方塊：

![已選取 [針對 HTTPS 進行設定] 核取方塊的 [新增 ASP.NET Core Web 應用程式] 視窗。](development-time-iis-support/_static/new-app.png)

在現有的專案中，請在 `Startup.Configure` 中透過呼叫 [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) 擴充方法來使用「HTTPS 重新導向中介軟體」：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>IIS 啟動設定檔

建立新的啟動設定檔，以新增開發階段 IIS 支援：

1. 針對 [設定檔]，選取 [新增] 按鈕。 在快顯示窗中，將設定檔命名為 "IIS"。 選取 [確定] 以建立設定檔。
1. 針對 [啟動] 設定，從清單中選取 [IIS]。
1. 選取 [啟動瀏覽器] 的核取方塊並提供端點 URL。 使用 HTTPS 通訊協定。 這個範例會使用 `https://localhost/WebApplication1`。
1. 在 [環境變數] 區段中，選取 [新增] 按鈕。 提供一個索引鍵為 `ASPNETCORE_ENVIRONMENT` 且值為 `Development` 的環境變數。
1. 在 [Web 伺服器設定] 區域中，設定 [應用程式 URL]。 這個範例會使用 `https://localhost/WebApplication1`。
1. 儲存設定檔。

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。 [設定檔] 與 [啟動] 的設定皆設為 [IIS]。 [啟動瀏覽器] 功能已啟用且位址為 https://localhost/WebApplication1。 在 [Web 伺服器設定] 區域的 [應用程式 URL] 欄位中也提供了相同的位址。](development-time-iis-support/_static/project_properties.png)

或者，您也可以手動將啟動設定檔新增至應用程式中的 [launchSettings.json](http://json.schemastore.org/launchsettings) 檔案：

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>執行專案

在 VS UI 中，將 [執行] 按鈕設定為 **IIS** 設定檔，然後選取該按鈕來啟動應用程式：

![已設定為 [IIS] 設定檔的 VS 工具列 [執行] 按鈕。](development-time-iis-support/_static/toolbar.png)

如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。 若收到提示，請重新啟動 Visual Studio。

如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。

## <a name="additional-resources"></a>其他資源

* [使用 IIS 在 Windows 上裝載 ASP.NET](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
* [強制使用 HTTPS](xref:security/enforcing-ssl)
