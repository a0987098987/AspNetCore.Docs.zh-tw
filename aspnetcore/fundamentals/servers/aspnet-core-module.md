---
title: "ASP.NET 核心模組"
author: tdykstra
description: "導入了 ASP.NET 核心模組 (ANCM)，可讓 Kestrel 網頁伺服器 IIS 或 IIS Express 做為反向 proxy 伺服器的 IIS 模組。"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 153c40f0e825ff5826e916c7ea877a25d81954f1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-aspnet-core-module"></a>ASP.NET Core 模組簡介

由[Tom Dykstra](https://github.com/tdykstra)， [Rick Strahl](https://github.com/RickStrahl)，和[Chris Ross](https://github.com/Tratcher) 

ASP.NET 核心模組 (ANCM) 可讓您執行 ASP.NET Core 應用程式背後 IIS，它很適合 （安全性、 管理及許多更多） 中使用 IIS，並使用[Kestrel](kestrel.md)如它很適合 （要快速學會），以及取得從一次這兩種技術的優點。 **ANCM 只適用於 Kestrel;它與不相容 WebListener (在 ASP.NET Core 1.x) 或 （在 2.x) HTTP.sys。** 

支援的 Windows 版本：

* Windows 7 和 Windows Server 2008 R2 和更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>ASP.NET 核心模組的功能

ANCM 是連結到管線的 IIS，並將流量重新導向到後端 ASP.NET Core 應用程式的原生 IIS 模組。 大部分其他模組，例如 windows 驗證時，仍有機會執行。 ANCM 才會控制當處理常式已選取的要求，且應用程式中定義的處理常式對應*web.config*檔案。

因為處理序中執行的 ASP.NET 核心應用程式是由 IIS 工作者處理序分開，ANCM 也沒有處理序管理。 ANCM 啟動 ASP.NET Core 應用程式的程序，當第一個要求送入，然後損毀時將它重新啟動。 這是傳統的 ASP.NET 應用程式基本上相同的行為執行同處理序在 IIS 和 WAS （Windows 啟用服務） 所管理。

以下是說明 IIS、 ANCM 和 ASP.NET Core 應用程式之間的關聯性圖表。

![ASP.NET 核心模組](aspnet-core-module/_static/ancm.png)

要求來自網站及叫用核心模式 Http.Sys 驅動程式的路由傳送到 IIS 上的主要連接埠 (80) 或 SSL 連接埠 (443)。 ANCM 將要求轉送至 ASP.NET Core 上的應用程式設定應用程式，不是通訊埠 80/443 的 HTTP 連接埠。

Kestrel 會接聽來自 ANCM 流量。  ANCM 指定環境變數在啟動時，透過的連接埠和[UseIISIntegration](#call-useiisintegration)方法會設定伺服器接聽`http://localhost:{port}`。 沒有額外的檢查拒絕不是從 ANCM 的要求。 （ANCM 不支援 HTTPS 轉送，因此即使透過 HTTPS 接收 IIS 要求都會轉送 over HTTP。）

Kestrel 拾取 ANCM 要求，並將 ASP.NET Core 中介軟體管線，然後處理它們，並將它們當做傳遞`HttpContext`應用程式邏輯的執行個體。 應用程式的回應，接著會傳遞至 IIS，它們回起始要求的 HTTP 用戶端的推播通知。

ANCM 有幾個其他函式：

* 設定環境變數。
* 記錄檔`stdout`輸出到檔案存放裝置。
* 將轉送 Windows 驗證權杖。

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>如何在 ASP.NET Core 應用程式中使用 ANCM

本節提供設定 IIS 伺服器和 ASP.NET Core 應用程式的程序概觀。 如需詳細指示，請參閱[與 IIS 的 Windows 上的主機](xref:host-and-deploy/iis/index)。

### <a name="install-ancm"></a>安裝 ANCM


ASP.NET 核心模組必須安裝在 IIS 伺服器上，並在 IIS Express 開發電腦上。 針對伺服器，ANCM 隨附於[.NET 核心 Windows Server 裝載配套](https://aka.ms/dotnetcore-2-windowshosting)。 開發電腦的 Visual Studio 會自動安裝 ANCM 在 IIS Express 中，並在 IIS 中如果已安裝在電腦上。

### <a name="install-the-iisintegration-nuget-package"></a>安裝 IISIntegration NuGet 套件

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)封裝包含在 ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/)和[Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). 如果您不使用其中一個 metapackages，安裝`Microsoft.AspNetCore.Server.IISIntegration`分開。 `IISIntegration`封裝會讀取廣播 ANCM 設定您的應用程式的環境變數的互通性組件。 環境變數提供組態資訊，例如要接聽的通訊埠。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在您的應用程式安裝[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)。 `IISIntegration`封裝會讀取廣播 ANCM 設定您的應用程式的環境變數的互通性組件。 環境變數提供組態資訊，例如要接聽的通訊埠。 

---

### <a name="call-useiisintegration"></a>呼叫 UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`UseIISIntegration`上的擴充方法[ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)使用 IIS 執行時自動呼叫。

如果您不使用其中一種 ASP.NET Core metapackages 並且尚未安裝`Microsoft.AspNetCore.Server.IISIntegration`封裝時，取得執行階段錯誤。 如果您呼叫`UseIISIntegration`明確地便會產生編譯時間錯誤如果封裝未安裝。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在您的應用程式中`Main`方法，請呼叫`UseIISIntegration`上的擴充方法[ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)。 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration`方法會尋找 ANCM 設定時，環境變數，並且它沒有 ops 如果找不到它們。 此行為可加速開發與 macOS 或 Linux 上測試並部署到執行 IIS 的伺服器等案例。 MacOS 或 Linux 上執行，同時 Kestrel 做為網頁伺服器;但是，當應用程式部署至 IIS 環境時，它會自動使用 ANCM 和 IIS。

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>ANCM 連接埠繫結會覆寫其他連接埠繫結

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM 會產生要指派給後端程序的動態連接埠。 `UseIISIntegration`方法會拾取此動態連接埠，並會設定為接聽 Kestrel `http://locahost:{dynamicPort}/`。 這會覆寫其他的 URL 設定，例如呼叫`UseUrls`或[Kestrel 的接聽 API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。 因此，您不需要呼叫`UseUrls`或 Kestrel 的`Listen`API，當您使用 ANCM。 如果您呼叫`UseUrls`或`Listen`，Kestrel 執行未使用 IIS 應用程式時指定通訊埠上接聽。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM 會產生要指派給後端程序的動態連接埠。 `UseIISIntegration`方法會拾取此動態連接埠，並會設定為接聽 Kestrel `http://locahost:{dynamicPort}/`。 這會覆寫其他的 URL 設定，例如呼叫`UseUrls`。 因此，您不需要呼叫`UseUrls`當您使用 ANCM。 如果您呼叫`UseUrls`，Kestrel 執行未使用 IIS 應用程式時指定通訊埠上接聽。

在 ASP.NET Core 1.0 中，如果您呼叫`UseUrls`，稱之為 「**之前**您呼叫`UseIISIntegration`以便 ANCM 設定連接埠不會覆寫。 在 ASP.NET Core 1.1 中，您不需要此呼叫的順序，因為 ANCM 設定會覆寫`UseUrls`。

---

### <a name="configure-ancm-options-in-webconfig"></a>在 Web.config 中設定 ANCM 選項

ASP.NET Core 模組的設定會儲存在*web.config*位於應用程式的根資料夾中的檔案。 此檔案中的設定會指向的啟動命令和啟動 ASP.NET Core 應用程式的引數。 範例*web.config*程式碼和指引組態選項，請參閱[ASP.NET 核心模組的組態參考](xref:host-and-deploy/aspnet-core-module)。

### <a name="run-with-iis-express-in-development"></a>在開發中執行的 IIS Express

IIS Express 可以由 Visual Studio 中使用 ASP.NET Core 範本所定義的預設設定檔啟動。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Proxy 設定使用 HTTP 通訊協定和配對的語彙基元

ANCM 和 Kestrel 之間建立 proxy 會使用 HTTP 通訊協定。 使用 HTTP 是其中 ANCM 和 Kestrel 之間的流量會在回送位址從網路介面效能最佳化。 沒有任何風險竊聽 ANCM 和 Kestrel 從位置不在伺服器之間的流量。

配對的語彙基元用來保證 Kestrel 所接收的要求已由 IIS 代理，且不是來自其他來源。 建立並設定環境變數配對的語彙基元 (`ASPNETCORE_TOKEN`) 由 ANCM。 配對的語彙基元也會設成標頭 (`MSAspNetCoreToken`) 針對每個 proxy 的要求。 IIS 中介軟體檢查每個要求收到確認配對的語彙基元的標頭值符合環境變數值。 如果語彙基元值不相符時，要求將記錄中，並拒絕。 配對的語彙基元的環境變數和 ANCM 和 Kestrel 之間的流量無法存取從出伺服器的位置。 而不需要知道配對的語彙基元值，攻擊者無法送出要求，略過檢查在 IIS 中介軟體。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [這篇文章的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET 核心模組的原始程式碼](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core 模組的組態參考](xref:host-and-deploy/aspnet-core-module)
* [ Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)
