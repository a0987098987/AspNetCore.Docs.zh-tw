---
title: "ASP.NET Core 模組"
author: tdykstra
description: "介紹 ASP.NET Core 模組 (ANCM)，這是可讓 Kestrel 網頁伺服器使用 IIS 或 IIS Express 作為反向 Proxy 伺服器的 IIS 模組。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>ASP.NET Core 模組簡介

作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl) 和 [Chris Ross](https://github.com/Tratcher) 

ASP.NET Core 模組 (ANCM) 可讓您在 IIS 背後執行 ASP.NET Core 應用程式，將 IIS 用於其擅長領域 (安全性、易管理性及許多其他項目) 並將 [Kestrel](kestrel.md) 用於其擅長領域 (速度真得很快)，同時從這兩種技術獲得好處。 **ANCM 只適用於 Kestrel；它與 WebListener (在 ASP.NET Core 1.x 中) 或 HTTP.sys (在 2.x 中) 不相容。** 

支援的 Windows 版本：

* Windows 7 與 Windows Server 2008 R2 和更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>ASP.NET Core 模組的功能

ANCM 是連結到 IIS 管線，並將流量重新導向至後端 ASP.NET Core 應用程式的原生 IIS 模組。 大部分的其他模組 (例如 Windows 驗證) 仍有機會執行。 唯有當已針對要求選取處理常式，且處理常式對應定義於應用程式的 *web.config* 檔案中時，ANCM 才會取得控制權。

因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以 ANCM 也會執行處理序管理。 ANCM 會在第一個要求送入時啟動 ASP.NET Core 應用程式的處理序，並在其損毀時將它重新啟動。 此行為基本上與在 IIS 中執行同處理序，並由 WAS (Windows 啟用服務) 所管理的傳統 ASP.NET 應用程式相同。

以下圖表說明 IIS、ANCM 和 ASP.NET Core 應用程式之間的關聯性。

![ASP.NET Core 模組](aspnet-core-module/_static/ancm.png)

要求自網路傳入並叫用核心模式 Http.Sys 驅動程式，該驅動程式會在主要連接埠 (80) 或 SSL 連接埠 (443) 上將要求路由至 IIS。 ANCM 則會在針對應用程式設定的 HTTP 連接埠 (不是連接埠 80/443) 上，將要求轉送至 ASP.NET Core 應用程式。

Kestrel 會接聽來自 ANCM 的流量。  ANCM 在啟動時透過環境變數指定連接埠，而 [UseIISIntegration](#call-useiisintegration) 方法則會設定伺服器來接聽 `http://localhost:{port}`。 有額外的檢查可拒絕不是來自 ANCM 的要求。 (ANCM 不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。)

Kestrel 會收取來自 ANCM 的要求，並將它們推送到 ASP.NET Core 中介軟體管線，該管線接著會處理這些要求，並將它們作為 `HttpContext` 執行個體傳遞至應用程式邏輯。 然後，應用程式的回應會傳回 IIS，而 IIS 會將它們推送回起始要求的 HTTP 用戶端。

ANCM 也有幾個其他函式：

* 設定環境變數。
* 將 `stdout` 輸出記錄到檔案儲存體。
* 轉送 Windows 驗證權杖。

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>如何在 ASP.NET Core 應用程式中使用 ANCM

本節提供設定 IIS 伺服器和 ASP.NET Core 應用程式的處理序概觀。 如需詳細指示，請參閱[使用 IIS 裝載在 Windows 上](xref:host-and-deploy/iis/index)。

### <a name="install-ancm"></a>安裝 ANCM


ASP.NET Core 模組必須安裝在伺服器上的 IIS 中，以及在安裝在開發電腦上的 IIS Express 中。 如果是伺服器，ANCM 隨附於 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore-2-windowshosting)。 如果是開發電腦，Visual Studio 會自動在 IIS Express 以及 IIS (如果已安裝在電腦上) 中安裝 ANCM。

### <a name="install-the-iisintegration-nuget-package"></a>安裝 IISIntegration NuGet 套件

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件包含在 ASP.NET Core 中繼套件 ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) 和 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)) 中。 如果您未使用其中一個中繼套件，請個別安裝 `Microsoft.AspNetCore.Server.IISIntegration`。 `IISIntegration` 套件是一種互通性組件，可由 ANCM 讀取環境變數廣播來設定應用程式。 環境變數提供組態資訊，例如要接聽的連接埠。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在您的應用程式中，安裝 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)。 `IISIntegration` 套件是一種互通性組件，可由 ANCM 讀取環境變數廣播來設定應用程式。 環境變數提供組態資訊，例如要接聽的連接埠。 

---

### <a name="call-useiisintegration"></a>呼叫 UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

當您搭配 IIS 執行時，會自動在 [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 上呼叫 `UseIISIntegration` 擴充方法。

如果您未使用其中一個 ASP.NET Core 中繼套件，而且並未安裝 `Microsoft.AspNetCore.Server.IISIntegration` 套件時，將會收到執行階段錯誤。 如果您明確地呼叫 `UseIISIntegration`，則會在未安裝套件時，收到編譯時間錯誤。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在應用程式的 `Main` 方法中，於 [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) 上呼叫 `UseIISIntegration` 擴充方法。 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` 方法會尋找 ANCM 設定的環境變數，如果找不到它們，就不會生效。 此行為有利於在 macOS 或 Linux 上　開發和測試，以及部署到執行 IIS 的伺服器等情況。 在 macOS 或 Linux 上執行時，Kestrel 會作為網頁伺服器；但是，當應用程式部署到 IIS 環境時，它會自動使用 ANCM 和 IIS。

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>ANCM 連接埠繫結覆寫其他連接埠繫結

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM 會產生要指派給後端處理序的動態連接埠。 `UseIISIntegration` 方法採用此動態連接埠，並設定 Kestrel 來接聽 `http://locahost:{dynamicPort}/`。 這會覆寫其他 URL 設定，例如呼叫 `UseUrls` 或 [Kestrel 的 Listen API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。 因此，當您使用 ANCM 時，不需要呼叫 `UseUrls` 或 Kestrel 的 `Listen` API。 如果確實呼叫 `UseUrls` 或 `Listen`，當您執行應用程式但未使用 IIS 時，Kestrel 會接聽您指定的連接埠。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM 會產生要指派給後端處理序的動態連接埠。 `UseIISIntegration` 方法採用此動態連接埠，並設定 Kestrel 來接聽 `http://locahost:{dynamicPort}/`。 這會覆寫其他 URL 設定，例如呼叫 `UseUrls`。 因此，當您使用 ANCM 時，不需要呼叫 `UseUrls`。 如果確實呼叫 `UseUrls`，當您執行應用程式但未使用 IIS 時，Kestrel 會接聽您指定的連接埠。

在 ASP.NET Core 1.0 中，如果您呼叫 `UseUrls`，請在呼叫 `UseIISIntegration` **之前**呼叫它，這樣 ANCM 設定的連接埠才不會被覆寫。 在 ASP.NET Core 1.1 中不需要遵循此呼叫順序，因為 ANCM 設定會覆寫 `UseUrls`。

---

### <a name="configure-ancm-options-in-webconfig"></a>在 Web.config 中設定 ANCM 選項

ASP.NET Core 模組的組態會儲存在位於應用程式根資料夾的 *web.config* 檔案中。 此檔案中的設定會指向啟動 ASP.NET Core 應用程式的啟動命令和引數。 如需 *web.config* 程式碼範例和組態選項的指引，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。

### <a name="run-with-iis-express-in-development"></a>在開發中搭配 IIS Express 執行

Visual Studio 可以使用 ASP.NET Core 範本所定義的預設設定檔來啟動 IIS Express。

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Proxy 組態使用 HTTP 通訊協定和配對權杖

ANCM 和 Kestrel 之間建立的 Proxy 會使用 HTTP 通訊協定。 使用 HTTP 是一項效能最佳化作業，其中 ANCM 與 Kestrel 之間的流量會在網路介面外的回送位址進行。 沒有從伺服器外的位置竊聽 ANCM 與 Kestrel 之間流量的風險。

配對權杖用來保證 Kestrel 所接收的要求已由 IIS 代理，而且不是來自其他來源。 ANCM 會建立配對權杖，並將其設定成環境變數 (`ASPNETCORE_TOKEN`)。 配對權杖也會設定成每個代理要求的標頭 (`MSAspNetCoreToken`)。 IIS 中介軟體會檢查其收到的每個要求，以確認配對權杖的標頭值符合環境變數值。 如果權杖值不相符，將記錄並拒絕要求。 配對權杖的環境變數和 ANCM 與 Kestrel 之間的流量無法從伺服器外的位置進行存取。 在不知道配對權杖值的情況下，攻擊者無法略過 IIS 中介軟體的檢查送出要求。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [本文的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [ASP.NET Core 模組的原始程式碼](https://github.com/aspnet/AspNetCoreModule)
* [ASP.NET Core 模組的組態參考](xref:host-and-deploy/aspnet-core-module)
* [ Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)
