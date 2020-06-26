---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: rick-anderson
description: 了解在 Windows Server 上搭配 IIS 執行 ASP.NET Core 應用程式時，對該應用程式所提供的偵錯支援。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: d922b2eb4d00a252b3e7da2f00de8b4571359b20
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406429"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Visual Studio for ASP.NET Core 中的開發階段 IIS 支援

由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供

::: moniker range=">= aspnetcore-3.0"

本文說明針對在 Windows Server 上搭配 IIS 執行的 ASP.NET Core 應用程式所提供的 [Visual Studio](https://visualstudio.microsoft.com) 偵錯支援。 本主題會逐步解說如何啟用此案例及設定專案。

## <a name="prerequisites"></a>必要條件

* [適用于 Windows 的 Visual Studio](https://visualstudio.microsoft.com/downloads/)
* **ASP.NET 與網頁程式開發**工作負載
* **.NET Core 跨平台開發**工作負載
* X.509 安全性憑證 (以取得 HTTPS 支援)

## <a name="enable-iis"></a>啟用 IIS

1. 在 Windows 中，瀏覽至 [控制台]**[程式]** > **[程式和功能]** > **[開啟或關閉 Windows 功能]** > **** (畫面左側)。
1. 選取 [Internet Information Services]**** 核取方塊。 選取 [確定]****。

安裝 IIS 可能需要重新啟動系統。

## <a name="configure-iis"></a>設定 IIS

IIS 的網站必須含有下列設定：

* **主機名稱**：通常，**預設的網站**會與**主機名稱**搭配使用 `localhost` 。 不過，任何具有唯一主機名稱的有效 IIS 網站皆適用。
* **網站繫結**
  * 針對需要 HTTPS 的應用程式，請搭配憑證針對連接埠 443 建立繫結。 通常會使用 [IIS Express 開發憑證]****，但可使用任何有效的憑證。
  * 針對使用 HTTP 的應用程式，請確認已存在針對連接埠 80 的繫結，或是為新網站建立針對連接埠 80 的繫結。
  * 需針對 HTTP 或 HTTPS 使用單一繫結。 **不支援同時針對 HTTP 和 HTTPS 連接埠的繫結。**

## <a name="enable-development-time-iis-support-in-visual-studio"></a>在 Visual Studio 中啟用開發階段 IIS 支援

1. 啟動 Visual Studio 安裝程式。
1. 針對您打算用於 IIS 開發階段支援的 Visual Studio 安裝，選取 [修改]****。
1. 針對 [ASP.NET 與網頁程式開發]**** 工作負載，尋找並安裝 [開發時間的 IIS 支援]**** 元件。

   該元件會列於工作負載右側 [安裝詳細資料]**** 中 [開發時間的 IIS 支援]**** 底下的 [選擇性]**** 區段中。 此元件會安裝 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。

## <a name="configure-the-project"></a>設定專案

### <a name="https-redirection"></a>HTTPS 重新導向

針對需要 HTTPS 的新專案，請在 [建立新的 ASP.NET Core Web 應用程式]**** 視窗中，選取 [針對 HTTPS 進行設定]**** 核取方塊。 選取該核取方塊會在建立應用程式時，將 [HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)加入該應用程式。

針對需要 HTTPS 的現有專案，請使用 `Startup.Configure`中的 HTTPS 重新導向和 HSTS 中介軟體。 如需詳細資訊，請參閱 <xref:security/enforcing-ssl> 。

針對使用 HTTP 的專案，[HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)並不會被加入至應用程式。 您不需要進行任何應用程式設定。

### <a name="iis-launch-profile"></a>IIS 啟動設定檔

建立新的啟動設定檔，以新增開發階段 IIS 支援：

1. 以滑鼠右鍵按一下 [方案總管]**** 中的專案。 選取 [屬性] 。 開啟 [**調試**] 索引標籤。
1. 針對 [設定檔]****，選取 [新增]**** 按鈕。 在快顯示窗中，將設定檔命名為 "IIS"。 選取 [確定]**** 以建立設定檔。
1. 針對 [啟動]**** 設定，從清單中選取 [IIS]****。
1. 選取 [啟動瀏覽器]**** 的核取方塊並提供端點 URL。

   當應用程式需要 HTTPS 時，請使用 HTTPS 端點 (`https://`)。 針對 HTTP，請使用 HTTP (`http://`) 端點。

   提供相同的主機名稱和連接埠作為 [IIS 設定所指定的早期用途](#configure-iis)，其通常是 `localhost`。

   在 URL 結尾提供應用程式的名稱。

   例如，`https://localhost/WebApplication1` (HTTPS) 或 `http://localhost/WebApplication1` (HTTP) 皆為有效的端點 URL。
1. 在 [環境變數]**** 區段中，選取 [新增]**** 按鈕。 提供 [名稱]**** 為 `ASPNETCORE_ENVIRONMENT` 且 [值]**** 為 `Development` 的環境變數。
1. 在 [Web 伺服器設定]**** 區域中，將 [應用程式 URL]**** 設定為用於 [啟動瀏覽器]**** 端點 URL 的相同值。
1. 針對 Visual Studio 2019 或更新版本中的 [裝載模型]****，請選取 [預設]**** 以使用專案所使用的裝載模型。 如果專案在其專案檔中已設定 `<AspNetCoreHostingModel>` 屬性，則會使用該屬性的值 (`InProcess` 或 `OutOfProcess`)。 如果該屬性不存在，便會使用應用程式的預設裝載模型，其為內部處理序。 如果應用程式需要與應用程式一般裝載模型不同的明確裝載模型設定，請視需要將 [裝載模型]**** 設定為 `In Process` 或 `Out Of Process`。
1. 儲存設定檔。

不使用 Visual Studio 時，請手動將啟動設定檔新增至 *Properties* 資料夾中的 [launchSettings.json](https://json.schemastore.org/launchsettings) \(英文\) 檔案。 下列範例會將專案設定為使用 HTTPS 通訊協定：

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

請確認 `applicationUrl` 和 `launchUrl` 端點相符，並使用和 IIS 繫結設定相同的通訊協定 (HTTP 或 HTTPS)。

## <a name="run-the-project"></a>執行專案

以系統管理員身分執行 Visual Studio：

* 確認建置設定下拉式清單已設定為 [偵錯]****。
* 將 [[開始調試](/visualstudio/debugger/debugger-feature-tour)程式] 按鈕設定為**IIS**設定檔，然後選取 [] 按鈕來啟動應用程式。

如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。 若收到提示，請重新啟動 Visual Studio。

如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。

> [!NOTE]
> 使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。 例如，不會碰到中斷點。

## <a name="additional-resources"></a>其他資源

* [IIS 中的 IIS 管理員使用者入門](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文說明針對在 Windows Server 上搭配 IIS 執行的 ASP.NET Core 應用程式所提供的 [Visual Studio](https://visualstudio.microsoft.com) 偵錯支援。 本主題會逐步解說如何啟用此案例及設定專案。

## <a name="prerequisites"></a>必要條件

* [適用于 Windows 的 Visual Studio](https://visualstudio.microsoft.com/downloads/)
* **ASP.NET 與網頁程式開發**工作負載
* **.NET Core 跨平台開發**工作負載
* X.509 安全性憑證 (以取得 HTTPS 支援)

## <a name="enable-iis"></a>啟用 IIS

1. 在 Windows 中，瀏覽至 [控制台]**[程式]** > **[程式和功能]** > **[開啟或關閉 Windows 功能]** > **** (畫面左側)。
1. 選取 [Internet Information Services]**** 核取方塊。 選取 [確定]****。

安裝 IIS 可能需要重新啟動系統。

## <a name="configure-iis"></a>設定 IIS

IIS 的網站必須含有下列設定：

* **主機名稱**：通常，**預設的網站**會與**主機名稱**搭配使用 `localhost` 。 不過，任何具有唯一主機名稱的有效 IIS 網站皆適用。
* **網站繫結**
  * 針對需要 HTTPS 的應用程式，請搭配憑證針對連接埠 443 建立繫結。 通常會使用 [IIS Express 開發憑證]****，但可使用任何有效的憑證。
  * 針對使用 HTTP 的應用程式，請確認已存在針對連接埠 80 的繫結，或是為新網站建立針對連接埠 80 的繫結。
  * 需針對 HTTP 或 HTTPS 使用單一繫結。 **不支援同時針對 HTTP 和 HTTPS 連接埠的繫結。**

## <a name="enable-development-time-iis-support-in-visual-studio"></a>在 Visual Studio 中啟用開發階段 IIS 支援

1. 啟動 Visual Studio 安裝程式。
1. 針對您打算用於 IIS 開發階段支援的 Visual Studio 安裝，選取 [修改]****。
1. 針對 [ASP.NET 與網頁程式開發]**** 工作負載，尋找並安裝 [開發時間的 IIS 支援]**** 元件。

   該元件會列於工作負載右側 [安裝詳細資料]**** 中 [開發時間的 IIS 支援]**** 底下的 [選擇性]**** 區段中。 此元件會安裝 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。

## <a name="configure-the-project"></a>設定專案

### <a name="https-redirection"></a>HTTPS 重新導向

針對需要 HTTPS 的新專案，請在 [建立新的 ASP.NET Core Web 應用程式]**** 視窗中，選取 [針對 HTTPS 進行設定]**** 核取方塊。 選取該核取方塊會在建立應用程式時，將 [HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)加入該應用程式。

針對需要 HTTPS 的現有專案，請使用 `Startup.Configure`中的 HTTPS 重新導向和 HSTS 中介軟體。 如需詳細資訊，請參閱 <xref:security/enforcing-ssl> 。

針對使用 HTTP 的專案，[HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)並不會被加入至應用程式。 您不需要進行任何應用程式設定。

### <a name="iis-launch-profile"></a>IIS 啟動設定檔

建立新的啟動設定檔，以新增開發階段 IIS 支援：

1. 以滑鼠右鍵按一下 [方案總管]**** 中的專案。 選取 [屬性] 。 開啟 [**調試**] 索引標籤。
1. 針對 [設定檔]****，選取 [新增]**** 按鈕。 在快顯示窗中，將設定檔命名為 "IIS"。 選取 [確定]**** 以建立設定檔。
1. 針對 [啟動]**** 設定，從清單中選取 [IIS]****。
1. 選取 [啟動瀏覽器]**** 的核取方塊並提供端點 URL。

   當應用程式需要 HTTPS 時，請使用 HTTPS 端點 (`https://`)。 針對 HTTP，請使用 HTTP (`http://`) 端點。

   提供相同的主機名稱和連接埠作為 [IIS 設定所指定的早期用途](#configure-iis)，其通常是 `localhost`。

   在 URL 結尾提供應用程式的名稱。

   例如，`https://localhost/WebApplication1` (HTTPS) 或 `http://localhost/WebApplication1` (HTTP) 皆為有效的端點 URL。
1. 在 [環境變數]**** 區段中，選取 [新增]**** 按鈕。 提供 [名稱]**** 為 `ASPNETCORE_ENVIRONMENT` 且 [值]**** 為 `Development` 的環境變數。
1. 在 [Web 伺服器設定]**** 區域中，將 [應用程式 URL]**** 設定為用於 [啟動瀏覽器]**** 端點 URL 的相同值。
1. 針對 Visual Studio 2019 或更新版本中的 [裝載模型]****，請選取 [預設]**** 以使用專案所使用的裝載模型。 如果專案在其專案檔中已設定 `<AspNetCoreHostingModel>` 屬性，則會使用該屬性的值 (`InProcess` 或 `OutOfProcess`)。 如果該屬性不存在，便會使用應用程式的預設裝載模型，其為外部處理序。 如果應用程式需要與應用程式一般裝載模型不同的明確裝載模型設定，請視需要將 [裝載模型]**** 設定為 `In Process` 或 `Out Of Process`。
1. 儲存設定檔。

不使用 Visual Studio 時，請手動將啟動設定檔新增至 *Properties* 資料夾中的 [launchSettings.json](https://json.schemastore.org/launchsettings) \(英文\) 檔案。 下列範例會將專案設定為使用 HTTPS 通訊協定：

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

請確認 `applicationUrl` 和 `launchUrl` 端點相符，並使用和 IIS 繫結設定相同的通訊協定 (HTTP 或 HTTPS)。

## <a name="run-the-project"></a>執行專案

以系統管理員身分執行 Visual Studio：

* 確認建置設定下拉式清單已設定為 [偵錯]****。
* 將 [[開始調試](/visualstudio/debugger/debugger-feature-tour)程式] 按鈕設定為**IIS**設定檔，然後選取 [] 按鈕來啟動應用程式。

如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。 若收到提示，請重新啟動 Visual Studio。

如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。

> [!NOTE]
> 使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。 例如，不會碰到中斷點。

## <a name="additional-resources"></a>其他資源

* [IIS 中的 IIS 管理員使用者入門](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end
