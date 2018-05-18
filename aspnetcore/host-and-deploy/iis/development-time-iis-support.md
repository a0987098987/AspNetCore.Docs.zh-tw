---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: shirhatti
description: 探索 Windows 伺服器上執行 IIS 後方時，偵錯 ASP.NET Core 應用程式的支援。
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
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core 中的開發階段 IIS 支援

由[Sourabh Shirhatti](https://twitter.com/sshirhatti)和[Luke Latham](https://github.com/guardrex)

本文說明[Visual Studio](https://www.visualstudio.com/vs/)支援 Windows Server 上執行後 IIS 的 ASP.NET Core 應用程式偵錯。 本主題引導啟用此功能，以及設定專案。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>啟用 IIS

1. 瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。
1. 選取**Internet Information Services**核取方塊。

![檢查變成黑色方塊 （未勾選記號） 表示的部分 IIS 功能已啟用 Windows 功能顯示 Internet Information Services 核取方塊](development-time-iis-support/_static/enable_iis.png)

IIS 安裝可能需要重新啟動系統。

## <a name="configure-iis"></a>設定 IIS

IIS 必須設定下列網站：

* 主機名稱符合應用程式的啟動設定檔 URL 主機名稱。
* 指派的憑證與連接埠 443 繫結。

例如，**主機名稱**新增的網站設定為"localhost"（啟動設定檔也會使用"localhost"本主題稍後的）。 連接埠會設定為"443"(HTTPS)。 **IIS Express 開發憑證**指派給網站，但任何有效的憑證適用於：

![在 IIS 連接埠 443 上顯示為本機主機設定的繫結，以指派憑證中加入網站視窗。](development-time-iis-support/_static/add-website-window.png)

如果 IIS 已經安裝具有**Default Web Site**符合應用程式的啟動設定檔 URL 主機名稱的主機名稱：

* 加入連接埠 443 (HTTPS) 連接埠繫結。
* 為網站指派有效的憑證。

## <a name="enable-development-time-iis-support-in-visual-studio"></a>啟用 Visual Studio 中的開發時間 IIS 支援

1. 啟動 Visual Studio 安裝程式。
1. 選取**支援 IIS 的開發時間**元件。 元件會列在為選擇性**摘要** 面板的**ASP.NET 及 web 開發**工作負載。 元件會安裝[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 背後 IIS 應用程式中的反向 proxy 設定所需的原生 IIS 模組。

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。 在 [Web and Cloud]\(Web 與雲端\) 區段中，選取 [ASP.NET 與網頁程式開發] 面板。 在右側的選擇性摘要面板區域中，沒有核取方塊適用於 IIS 支援開發時間。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>設定專案

### <a name="https-redirection"></a>HTTPS 的重新導向

對於新的專案中，選取核取方塊，以**設定以進行 HTTPS**中**新 ASP.NET Core Web 應用程式**視窗：

![具有 HTTPS 核取方塊，選取 設定的新 ASP.NET Core Web 應用程式視窗。](development-time-iis-support/_static/new-app.png)

在現有的專案中，使用 HTTPS 重新導向中介軟體中`Startup.Configure`藉由呼叫[UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)擴充方法：

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

建立新的啟動設定檔，以加入開發時間 IIS 支援：

1. 如**設定檔**，選取**新增** 按鈕。 名稱"IIS"設定檔快顯視窗中。 選取**確定**建立設定檔。
1. 如**啟動**設定中，選取**IIS**從清單中。
1. 選取核取方塊**啟動瀏覽器**和提供的端點 URL。 使用 HTTPS 通訊協定。 這個範例會使用 `https://localhost/WebApplication1`。
1. 在**環境變數**區段中，選取**新增** 按鈕。 提供索引鍵為環境變數`ASPNETCORE_ENVIRONMENT`以及值`Development`。
1. 在**網頁伺服器設定**區域中，設定**應用程式 URL**。 這個範例會使用 `https://localhost/WebApplication1`。
1. 儲存設定檔。

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。 [設定檔] 與 [啟動] 的設定皆設為 [IIS]。 啟動瀏覽器功能已啟用的位址https://localhost/WebApplication1。 [Web 伺服器設定] 區域的 [應用程式 URL] 欄位中，也會提供相同的位址。](development-time-iis-support/_static/project_properties.png)

或者，手動新增至啟動設定檔[launchSettings.json](http://json.schemastore.org/launchsettings)應用程式中的檔案：

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

在 VS UI 中，設定為 [執行] 按鈕**IIS**剖析，並選取 [啟動應用程式] 按鈕：

![執行與工具列設定為"IIS"設定檔中的按鈕。](development-time-iis-support/_static/toolbar.png)

如果未以系統管理員身分執行 visual Studio 可能會提示重新啟動。 若收到提示，請重新啟動 Visual Studio。

如果使用不受信任的程式開發憑證瀏覽器可能會需要您建立受信任的憑證的例外狀況。

## <a name="additional-resources"></a>其他資源

* [使用 IIS 在 Windows 上裝載 ASP.NET](xref:host-and-deploy/iis/index)
* [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)
* [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)
* [強制使用 HTTPS](xref:security/enforcing-ssl)
