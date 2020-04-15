---
title: ASP.NET Core 基本概念
author: rick-anderson
description: 了解建置 ASP.NET Core 應用程式的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2020
uid: fundamentals/index
ms.openlocfilehash: c675644d8480ef7a5290045067e6cec2ea6f4764
ms.sourcegitcommit: f29a12486313e38e0163a643d8a97c8cecc7e871
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81384054"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core 基本概念

::: moniker range=">= aspnetcore-3.0"

本文概述了如何開發ASP.NET核心應用的關鍵主題。

## <a name="the-startup-class"></a>Startup 類別

`Startup` 類別是：

* 已設定應用程式所需的服務。
* 應用的請求處理管道定義為一系列中間件元件。

以下是 `Startup` 類別範例：

[!code-csharp[](index/samples_snapshot/3.x/Startup.cs?highlight=3,12)]

如需詳細資訊，請參閱 <xref:fundamentals/startup>。

## <a name="dependency-injection-services"></a>相依性插入 (服務)

ASP.NET核心包括一個內置的依賴項注入 (DI) 框架,該框架使配置的服務在整個應用中可用。 例如，記錄元件即為一項服務。

設定 (或「註冊」**) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。 例如：

[!code-csharp[](index/samples_snapshot/3.x/ConfigureServices.cs)]

服務通常使用建構函數注入從 DI 解析。 使用建構函數注入時,類聲明所需類型的構造函數參數或介面。 DI 框架在運行時提供此服務的實例。

下面的範例使用建構函數注入來解決`RazorPagesMovieContext`DI 中的 a:

[!code-csharp[](index/samples_snapshot/3.x/Index.cshtml.cs?highlight=5)]

如果內置的 Control 反轉 (IoC) 容器無法滿足應用的所有需求,則可以改用第三方 IoC 容器。

如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。

## <a name="middleware"></a>中介軟體

要求處理管線是以一系列中介軟體元件組成。 每個元件對 執行操作`HttpContext`,並調用管道中的下一個中間件或終止請求。

按照慣例,通過調用`Use...``Startup.Configure`方法中的擴展方法,將中間件元件添加到管道中。 例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。

以下範例設定要求處理導管:

[!code-csharp[](index/samples_snapshot/3.x/Configure.cs)]

ASP.NET核心包括一組豐富的內置中間件。 也可以編寫自定義中間件元件。

如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。

## <a name="host"></a>Host

啟動時,ASP.NET核心應用將產生*主機*。 主機封裝應用的所有資源,例如:

* HTTP 伺服器實作
* 中介軟體元件
* 記錄
* 相依項(DI) 服務
* 組態

有兩個不同的主機: 

* .NET 泛型主機
* ASP.NET Core Web 主機

建議使用 .NET 通用主機。 ASP.NET核心 Web 主機僅適用於向後相容性。

下面的範例建立一個 .NET 通用主機:

[!code-csharp[](index/samples_snapshot/3.x/Program.cs)]

`CreateDefaultBuilder`與`ConfigureWebHostDefaults`方法使用一組預設選項設定主機,例如:

* 使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。
* 從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。
* 將記錄輸出傳送到主控台及偵錯提供者。

如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。

### <a name="non-web-scenarios"></a>非 Web 案例

一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 和 <xref:fundamentals/host/hosted-services>。

## <a name="servers"></a>伺服器

ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。 伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。

# <a name="windows"></a>[Windows](#tab/windows)

ASP.NET Core 隨附下列伺服器實作：

* *Kestrel* 是跨平台的網頁伺服器。 Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。 在 ASP.NET Core 2.0 或更新版本中，Kestrel 可以作為直接向網際網路公開的公眾 Edge Server 執行。
* *IIS HTTP 伺服器*是使用 IIS 的 Windows 伺服器。 透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。
* *HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。

# <a name="macos"></a>[macOS](#tab/macos)

ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。 在ASP.NET Core 2.0 或更高版本中,Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。 Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。

# <a name="linux"></a>[Linux](#tab/linux)

ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。 在ASP.NET Core 2.0 或更高版本中,Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。 Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。

---

如需詳細資訊，請參閱 <xref:fundamentals/servers/index>。

## <a name="configuration"></a>組態

ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。 內建配置提供者可用於各種源,如 *.json*檔 *、.xml*檔、環境變數和命令列參數。 編寫自定義配置提供程式以支援其他源。

[默認情況下](xref:fundamentals/configuration/index#default),ASP.NET核心應用配置為從*appsettings.json、* 環境變數、命令列等讀取。 載入應用的配置時,環境變數的值將覆蓋*appsettings.json*中的值。

讀取相關設定值的偏好方法是使用[選項模式](xref:fundamentals/configuration/options)。 關於詳細資訊,請參考[使用選項模式繫結分層設定資料](xref:fundamentals/configuration/index#optpat)。

對管理機密設定資料(如密碼),ASP.NET核心提供[機密管理員](xref:security/app-secrets#secret-manager)。 針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。

如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。

## <a name="environments"></a>環境

執行環境(如`Development`、`Staging``Production`和) 是 ASP.NET Core 中的一流概念。 通過設置`ASPNETCORE_ENVIRONMENT`環境變數指定應用正在運行的環境。 ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IWebHostEnvironment` 實作中。 此實現可通過依賴項注入 (DI) 在應用中的任意位置可用。

以下範例將應用設定為在`Development`環境中執行時提供詳細的錯誤資訊:

[!code-csharp[](index/samples_snapshot/3.x/StartupConfigure.cs?highlight=3-6)]

如需詳細資訊，請參閱 <xref:fundamentals/environments>。

## <a name="logging"></a>記錄

ASP.NET Core 支援適用於各種內建和協力廠商記錄提供者的記錄 API。 可用的供應商包括:

* 主控台
* 偵錯
* Windows 上的事件追蹤
* Windows 事件記錄檔
* TraceSource
* Azure App Service
* Azure Application Insights

要建立紀錄,請從<xref:Microsoft.Extensions.Logging.ILogger%601>相依相依支援項 (DI) 解析服務<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>,並呼叫紀錄紀錄方法(如 。 例如：

[!code-csharp[](index/samples_snapshot/3.x/TodoController.cs?highlight=5,13,19)]

日誌記錄方法,如`LogInformation`支援任意數量的欄位。 這些欄位通常用於建構消息`string`,但某些日誌記錄提供程式將這些欄位作為單獨的欄位發送到資料存儲。 這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。

如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。

## <a name="routing"></a>路由

「路由」** 是一種對應到處理常式的 URL 模式。 處理常式通常是 Razor 頁面、MVC 控制器中的動作方法，或是中介軟體。 ASP.NET Core 路由可讓您控制您應用程式使用的 URL。

如需詳細資訊，請參閱 <xref:fundamentals/routing>。

## <a name="error-handling"></a>錯誤處理

ASP.NET Core 具有處理錯誤的內建功能，例如：

* 開發人員例外狀況頁面
* 自訂錯誤頁面
* 靜態狀態碼頁面
* 啟動例外狀況處理

如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。

## <a name="make-http-requests"></a>發出 HTTP 要求

`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。 Factory：

* 提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。 例如,註冊和配置*github*用戶端以訪問 GitHub。 註冊和配置默認用戶端以用於其他目的。
* 支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。 此模式類似於ASP.NET Core 的入站中間件管道。 該模式提供了一種機制來管理 HTTP 請求的跨領域問題,包括緩存、錯誤處理、序列化和日誌記錄。
* 與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。
* 管理基礎`HttpClientHandler`實例的池和存留期,以避免手`HttpClient`動管理 存留期時出現常見的 DNS 問題。
* 通過<xref:Microsoft.Extensions.Logging.ILogger>工廠創建的所有用戶端發送的所有請求添加可配置的日誌記錄體驗。

如需詳細資訊，請參閱 <xref:fundamentals/http-requests>。

## <a name="content-root"></a>內容根目錄

內容根是以下基本路徑:

* 託管應用程式的可執行檔 *(.exe*)。
* 組成應用程式的編譯程式集(*.dll*)。
* 套用內容檔案,例如:
  * 剃刀檔 *(.cshtml*, *.razor*)
  * 設定檔 *(.json*, *.xml*)
  * 資料檔案(*.db*)
* [Web 根](#web-root),通常是*wwwroot*資料夾。

在開發過程中,內容根預設為專案的根目錄。 此目錄也是應用的內容檔和[Web 根](#web-root)目錄的基本路徑。 通過在[構建主機](#host)時設置其路徑來指定其他內容根。 如需詳細資訊，請參閱[內容根](xref:fundamentals/host/generic-host#contentroot)。

## <a name="web-root"></a>Web 根目錄

Web 根是公共靜態資源檔的基本路徑,例如:

* 樣式表 (*.css*)
* JavaScript (*.js*)
* 圖片 (*.png*, *.jpg*)

預設情況下,靜態檔僅從 Web 根目錄及其子目錄提供。 Web 根路徑預設為 *[內容根]/wwwroot*。 通過在[構建主機](#host)時設置其路徑來指定其他 Web 根。 如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/generic-host#webroot)。

防止在*wwwroot*中使用專案檔中[\<的內容>專案項](/visualstudio/msbuild/common-msbuild-project-items#content)發佈檔。 以下範例防止在*wwwroot/local*及其子目錄中發表內容:

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

在 Razor *.cshtml*檔中,`~/`波浪斜 槓 ( ) 指向 Web 根。 以 開`~/`頭的路徑稱為*虛擬路徑*。

如需詳細資訊，請參閱 <xref:fundamentals/static-files>。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文是了解如何開發 ASP.NET Core 應用程式的關鍵主題概觀。

## <a name="the-startup-class"></a>Startup 類別

`Startup` 類別是：

* 已設定應用程式所需的服務。
* 定義要求處理管線的位置。

「服務」** 是應用程式所使用的元件。 例如，記錄元件即為一項服務。 設定 (或「註冊」**) 服務的程式碼會新增至 `Startup.ConfigureServices` 方法。

請求處理管道由一系列*中間件*元件組成。 例如，中介軟體可能會處理靜態檔案的要求，或是將 HTTP 要求重新導向至 HTTPS。 每個中介軟體會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。 設定要求處理管線的程式碼會新增至 `Startup.Configure` 方法。

以下是 `Startup` 類別範例：

[!code-csharp[](index/samples_snapshot/2.x/Startup.cs?highlight=3,12)]

如需詳細資訊，請參閱 <xref:fundamentals/startup>。

## <a name="dependency-injection-services"></a>相依性插入 (服務)

ASP.NET Core 具有內建的相依性插入 (DI) 架構，可讓應用程式的類別使用所設定服務。 取得類別中一項服務執行個體的其中一種方式，便是使用所需類型的參數來建立建構函式。 參數可以是服務類型或是介面。 DI 系統會在執行階段提供服務。

以下是使用 DI 取得 Entity Framework Core 內容物件的類別。 醒目提示的那一行便是建構函式插入的範例：

[!code-csharp[](index/samples_snapshot/2.x/Index.cshtml.cs?highlight=5)]

雖然 DI 為內建，但其設計用於讓您插入協力廠商的控制反轉 (IoC) 容器 (若您想要的話)。

如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。

## <a name="middleware"></a>中介軟體

要求處理管線是以一系列中介軟體元件組成。 每個元件會在 `HttpContext` 上執行非同步操作，然後叫用管線中下一個中介軟體或終止要求。

依照慣例，中介軟體元件會透過叫用其 `Startup.Configure` 方法中的 `Use...` 延伸模組方法來新增至管線。 例如，若要啟用靜態檔案轉譯，請呼叫 `UseStaticFiles`。

下列範例中醒目提示的程式碼會設定要求處理管線：

[!code-csharp[](index/samples_snapshot/2.x/Startup.cs?highlight=14-16)]

ASP.NET Core 包含一組豐富的內建中介軟體，您也可以撰寫自訂中介軟體。

如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。

## <a name="host"></a>Host

ASP.NET Core 應用程式會在啟動時建置一個「主機」**。 主機是封裝所有應用程式資源的物件，例如：

* HTTP 伺服器實作
* 中介軟體元件
* 記錄
* DI
* 組態

在單一物件中包含所有應用程式相互依存資源的主要理由便是生命週期管理：控制應用程式的啟動及順利關機。

可以使用兩種主機：Web 主機與一般主機。 在 ASP.NET Core 2.x 中，一般主機僅適用於非 Web 案例。

建立主機的程式碼位於 `Program.Main` 中：

[!code-csharp[](index/samples_snapshot/2.x/Program.cs)]

`CreateDefaultBuilder` 方法會設定具備一般常用選項的主機，如下所示：

* 使用 [Kestrel](#servers) 作為網頁伺服器，並啟用 IIS 整合。
* 從 *appsettings.json*、*appsettings.{Environment Name}.json*、環境變數與命令列引數載入設定。
* 將記錄輸出傳送到主控台及偵錯提供者。

如需詳細資訊，請參閱 <xref:fundamentals/host/web-host>。

### <a name="non-web-scenarios"></a>非 Web 案例

一般主機允許其他類型的應用程式，使用交叉剪輯架構延伸模組，例如記錄、相依性插入 (DI)、設定與應用程式存留期管理。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 和 <xref:fundamentals/host/hosted-services>。

## <a name="servers"></a>伺服器

ASP.NET Core 應用程式使用 HTTP 伺服器實作來接聽 HTTP 要求。 伺服器會把向應用程式發出的要求作為一組[要求功能](xref:fundamentals/request-features)，合併成一個 `HttpContext`。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[Windows](#tab/windows)

ASP.NET Core 隨附下列伺服器實作：

* *Kestrel* 是跨平台的網頁伺服器。 Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。 Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。
* *IIS HTTP 伺服器*則是適用於使用 IIS Windows 的伺服器。 透過此伺服器，ASP.NET Core 應用程式及 IIS 便可以在相同處理序中執行。
* *HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。

# <a name="macos"></a>[macOS](#tab/macos)

ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。 Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。 Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。

# <a name="linux"></a>[Linux](#tab/linux)

ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。 Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。 Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[Windows](#tab/windows)

ASP.NET Core 隨附下列伺服器實作：

* *Kestrel* 是跨平台的網頁伺服器。 Kestrel 通常會使用 [IIS](https://www.iis.net/)在反向 Proxy 設定中執行。 Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。
* *HTTP.sys* 則是適用於並未搭配 IIS 使用 Windows 的伺服器。

# <a name="macos"></a>[macOS](#tab/macos)

ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。 Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。 Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。

# <a name="linux"></a>[Linux](#tab/linux)

ASP.NET Core 提供 *Kestrel* 跨平台伺服器實作。 Kestrel 可以作為直接暴露到 Internet 的面向公共的邊緣伺服器運行。 Kestrel 通常會使用 [Nginx](https://nginx.org) 或 [Apache](https://httpd.apache.org/)在反向 Proxy 設定中執行。

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

如需詳細資訊，請參閱 <xref:fundamentals/servers/index>。

## <a name="configuration"></a>組態

ASP.NET Core 提供組態架構，可從組態提供者的已排序集合中，以成對名稱和數值的形式取得設定。 您可以使用各種來源的內建組態提供者，例如 *.json* 檔案、*.xml* 檔案、環境變數及命令列引數。 您也可以撰寫自訂組態提供者。

例如，您可以指定組態來自 *appsettings.json* 和環境變數。 然後當要求 *ConnectionString* 的值時，架構便會先在 *appsettings.json* 檔案中尋找。 若有找到值，但在環境變數中也存在該值，則會優先使用來自環境變數的值。

針對管理保密組態資料 (例如密碼)，ASP.NET Core 提供[祕密管理員工具](xref:security/app-secrets)。 針對生產祕密，我們建議使用 [Azure Key Vault](xref:security/key-vault-configuration)。

如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。

## <a name="options"></a>選項。

在可能的情況下，ASP.NET Core 會遵循「選項模式」** 來儲存及擷取組態值。 選項模式使用類別來代表一組相關的設定。

例如，下列程式碼會設定 WebSocket 選項：

[!code-csharp[](index/samples_snapshot/2.x/UseWebSockets.cs)]

如需詳細資訊，請參閱 <xref:fundamentals/configuration/options>。

## <a name="environments"></a>環境

執行環境 (例如「開發」**、「預備」** 及「生產」**) 是 ASP.NET Core 中的第一級概念。 您可以透過設定 `ASPNETCORE_ENVIRONMENT` 環境變數，指定應用程式執行的環境。 ASP.NET Core 會在應用程式啟動時讀取環境變數，然後將值儲存在 `IHostingEnvironment` 實作中。 環境物件可在應用程式中的任何位置，透過 DI 使用。

下列 `Startup` 類別的範例程式碼會設定應用程式，只在於「開發」環境中執行時提供詳細的錯誤資訊：

[!code-csharp[](index/samples_snapshot/2.x/StartupConfigure.cs?highlight=3-6)]

如需詳細資訊，請參閱 <xref:fundamentals/environments>。

## <a name="logging"></a>記錄

ASP.NET Core 支援適用於各種內建和協力廠商記錄提供者的記錄 API。 可用的提供者包括如下：

* 主控台
* 偵錯
* Windows 上的事件追蹤
* Windows 事件記錄檔
* TraceSource
* Azure App Service
* Azure Application Insights

透過從 DI 取得 `ILogger` 物件並呼叫記錄方法，從應用程式程式碼中的任何位置寫入記錄。

以下是使用 `ILogger` 物件的範例程式碼，其中建構函式插入及記錄方法呼叫都已醒目提示。

[!code-csharp[](index/samples_snapshot/2.x/TodoController.cs?highlight=5,13,17)]

`ILogger` 介面可讓您將任何數量的欄位傳遞給記錄提供者。 欄位常用於建構訊息字串，但提供者也可以將它們作為個別欄位，傳送至資料存放區。 這項功能可讓記錄提供者實作 [semantic logging (語意記錄)，又稱為 structured logging (結構化記錄)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。

如需詳細資訊，請參閱 <xref:fundamentals/logging/index>。

## <a name="routing"></a>路由

「路由」** 是一種對應到處理常式的 URL 模式。 處理常式通常是 Razor 頁面、MVC 控制器中的動作方法，或是中介軟體。 ASP.NET Core 路由可讓您控制您應用程式使用的 URL。

如需詳細資訊，請參閱 <xref:fundamentals/routing>。

## <a name="error-handling"></a>錯誤處理

ASP.NET Core 具有處理錯誤的內建功能，例如：

* 開發人員例外狀況頁面
* 自訂錯誤頁面
* 靜態狀態碼頁面
* 啟動例外狀況處理

如需詳細資訊，請參閱 <xref:fundamentals/error-handling>。

## <a name="make-http-requests"></a>發出 HTTP 要求

`IHttpClientFactory` 的實作可用於建立 `HttpClient` 執行個體。 Factory：

* 提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。 例如,可以註冊和配置*github*用戶端以訪問 GitHub。 預設用戶端可以註冊用於其他用途。
* 支援註冊及多個委派處理常式的鏈結，以用於建置傳出要求中介軟體管線。 此模式與 ASP.NET Core 中的輸入中介軟體管線相似。 模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。
* 與 *Polly* 整合，Polly 是一種熱門的協力廠商程式庫，用於進行暫時性的錯誤處理。
* 管理基礎 `HttpClientHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。
* 針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。

如需詳細資訊，請參閱 <xref:fundamentals/http-requests>。

## <a name="content-root"></a>內容根目錄

內容根是到:

* 可執行主機應用程式 *(.exe*)。
* 組成應用程式的編譯程式集(*.dll*)。
* 套用使用的非程式碼內容檔,例如:
  * 剃刀檔 *(.cshtml*, *.razor*)
  * 設定檔 *(.json*, *.xml*)
  * 資料檔案(*.db*)
* [Web 根](#web-root),通常是已發布*的 wwwroot*資料夾。

在開發過程中:

* 內容根預設為專案的根目錄。
* 專案的根目錄用於建立:
  * 專案根目錄中應用的非代碼內容檔的路徑。
  * [Web 根](#web-root),通常是專案根目錄中的*wwwroot*資料夾。

[在構建主機](#host)時,可以指定替代內容根路徑。 如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#content-root>。

## <a name="web-root"></a>Web 根目錄

Web 根是公共、非代碼、靜態資源檔的基本路徑,例如:

* 樣式表 (*.css*)
* JavaScript (*.js*)
* 圖片 (*.png*, *.jpg*)

靜態檔僅預設從 Web 根目錄(和子目錄)提供。

Web 根路徑預設為 *[內容根]/wwwroot,* 但在[構建主機](#host)時可以指定不同的 Web 根。 如需詳細資訊，請參閱 [Web 根目錄](xref:fundamentals/host/web-host#web-root)。

防止在*wwwroot*中使用專案檔中[\<的內容>專案項](/visualstudio/msbuild/common-msbuild-project-items#content)發佈檔。 以下範例防止在*wwwroot/local*目錄和子目錄中發表內容:

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

在 Razor(*.cshtml*)`~/`檔中, 波浪斜線 ( ) 指向 Web 根。 以 開`~/`頭的路徑稱為*虛擬路徑*。

如需詳細資訊，請參閱 <xref:fundamentals/static-files>。

::: moniker-end
