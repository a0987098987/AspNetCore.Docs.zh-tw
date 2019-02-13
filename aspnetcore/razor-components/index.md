---
title: Razor 元件簡介
author: guardrex
description: 瀏覽 Blazor，這是使用 C#/Razor 和 HTML 的 .NET Web 架構，在具有 WebAssembly 的瀏覽器中執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667910"
---
# <a name="introduction-to-razor-components"></a>Razor 元件簡介

作者：[Steve Sanderson](http://blog.stevensanderson.com)、[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

「ASP.NET Core Razor 元件」會在 ASP.NET Core 的伺服器端執行，而 *Blazor* (在用戶端執行的 Razor 元件) 是使用 C#/Razor 和 HTML 的實驗性 .NET Web 架構，在具有 WebAssembly 的瀏覽器中執行。 Blazor 提供在用戶端上使用 .NET 的所有用戶端 Web UI 架構優點。

多年來，Web 開發已在許多方面改善，但建置現代化 Web 應用程式仍然造成一些挑戰。 在瀏覽器中使用 .NET 提供了許多優點，可協助您更輕鬆且更具生產力地進行 Web 開發：

* **穩定性與一致性**：.NET 提供跨平台的標準化程式設計架構，不僅穩定、功能豐富，且容易使用。
* **現代化的創新語言**：.NET 語言會持續以創新的新語言功能來改善。
* **領先業界的工具**：Visual Studio 產品系列在 Windows、Linux 和 macOS 上提供跨平台的卓越 .NET 開發體驗。
* **速度和延展性**：.NET 有堅強的應用程式開發效能、可靠性和安全性歷程記錄。 使用 .NET 作為完整堆疊解決方案，可讓您更輕鬆地建置快速、可靠且安全的應用程式。
* **運用現有技能的完整堆疊開發**：C#/Razor 開發人員使用其現有的 C#/Razor 技能，撰寫用戶端程式碼，並在應用程式之間共用伺服器和用戶端的邏輯。
* **廣泛的瀏覽器支援**：Razor 元件將 UI 轉譯為一般的標記和 JavaScript。 Blazor 會在使用開放式 Web 標準，且不含任何外掛程式、任何程式碼轉譯的瀏覽器中，在 .NET 上執行。 Blazor 適用於所有現代化網頁瀏覽器，包括行動瀏覽器。

## <a name="hosting-models"></a>裝載模型

### <a name="server-side-hosting-model"></a>伺服器端裝載模型

因為 Razor 元件解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Razor 元件的方式具有彈性。 .NET Core 3.0 中的 ASP.NET Core Razor 元件在 ASP.NET Core 應用程式裡新增支援以將 Razor 元件裝載在伺服器上，使得透過 SignalR 連線處理所有的 UI 更新。 執行階段會處理從瀏覽器將 UI 事件傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。 相同的連線也用來處理 JavaScript Interop 呼叫。

![Razor 元件在伺服器上執行 .NET 程式碼，並透過 SignalR 連線與用戶端上的文件物件模型互動](index/_static/aspnet-core-razor-components.png)

如需詳細資訊，請參閱<xref:razor-components/hosting-models#server-side-hosting-model>。

### <a name="client-side-hosting-model"></a>用戶端裝載模型

在網頁瀏覽器內執行 .NET 程式碼，已經藉由一項相當新的技術達成：[WebAssembly](http://webassembly.org) (縮寫為 *wasm*)。 WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。 WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。

WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。 在此同時，WebAssembly 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。

![Blazor 在具有 WebAssembly 的瀏覽器中執行 .NET 程式碼。](index/_static/blazor.png)

當建置 Blazor 應用程式並在瀏覽器中執行時：

1. C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。
1. 組件和 .NET 執行階段會下載至瀏覽器中。
1. Blazor 使用 JavaScript 來啟動 .NET 執行階段，並設定執行階段以載入必要的組件參考。 Blazor 執行階段會透過 JavaScript 互通性，處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。

若要支援那些不支援 WebAssembly 的較舊瀏覽器，您可以使用 ASP.NET Core Razor 元件[伺服器端裝載模型](#server-side-hosting-model)。

如需詳細資訊，請參閱<xref:razor-components/hosting-models#client-side-hosting-model>。

## <a name="components"></a>元件

應用程式是使用「元件」建置。 元件是一種 UI，例如頁面、對話方塊或資料輸入表單。 元件可以為巢狀結構、可重複使用，且在專案之間共用。

「元件」是一種 .NET 類別。 其可以直接撰寫為 C# 類別 (*\*.cs*)，或更常見的 Razor 標記頁面形式 (*\*.cshtml*)。

[Razor](/aspnet/core/mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。 Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。 下列標記是 Razor 檔案中的基本自訂對話方塊元件範例 (*DialogComponent.cshtml*)：

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

在應用程式的其他地方使用此元件時，IntelliSense 會藉由語法和參數完成而加快開發速度。

元件可以是：

* 巢狀結構。
* 使用 Razor (*\*.cshtml*) 或 C# (*\*.cs*) 程式碼建立。
* 透過類別庫而共用。
* 進行單元測試而不需要瀏覽器 DOM。

## <a name="infrastructure"></a>基礎結構

Razor 元件提供大部分應用程式需要的核心功能，包括：

* 版面配置
* 路由
* 相依性插入

所有這些功能皆為選擇性。 當這些功能的其中一個未在應用程式中使用時，且由 [Intermediate Language (IL) 連結器](xref:host-and-deploy/razor-components/configure-linker) 發佈時會從應用程式移除實作。

## <a name="code-sharing-and-net-standard"></a>程式碼共用和 .NET Standard

應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。 .NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。 支援 .NET Standard 2.0 或更新版本。 網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception)。 .NET Standard 類別庫可以跨伺服端程式碼共用，以及在以瀏覽器為基礎的應用程式中共用。

## <a name="javascript-interop"></a>JavaScript Interop

對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，WebAssembly 設計為與 JavaScript 交互操作。 Razor 元件可以使用 JavaScript 可以使用的任何程式庫或 API。 C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。 如需詳細資訊，請參閱 [JavaScript Interop](xref:razor-components/javascript-interop)。

## <a name="optimization"></a>最佳化

對於用戶端應用程式而言，承載大小很重要。 Blazor 對承載大小進行最佳化，以縮短下載時間。 比方說，在建置程序期間會移除未使用的 .NET 組件部分、HTTP 回應會壓縮，而 .NET 執行階段和組件會在瀏覽器中快取。

Razor 元件藉由在伺服器端維護 .NET 組件、應用程式的組件和執行階段，提供比 Blazor 更小的承載大小。 Razor 元件應用程式只會提供標記、指令碼和樣式表給用戶端。

## <a name="deployment"></a>部署

使用 Blazor 建置一個單純的獨立用戶端應用程式，或同時包含伺服器和用戶端應用程式的完整堆疊 ASP.NET Core 應用程式：

* 在**獨立用戶端應用程式**中，Blazor 應用程式會編譯成僅包含靜態檔案的 *dist* 資料夾。 檔案可以裝載於 Azure App Service、GitHub Pages、IIS (設定為靜態檔案伺服器)、Node.js 伺服器和其他許多伺服器和服務上。 在生產的伺服器上，不需要 .NET。
* 在**完整堆疊 ASP.NET Core 應用程式**中，可以在伺服器和用戶端應用程式之間共用程式碼。 產生的 ASP.NET Core Razor 元件應用程式，會提供用戶端 UI 和其他伺服器端 API 端點，其可被建置並部署到 ASP.NET Core 支援的任何雲端或內部部署主機。

## <a name="suggest-a-feature-or-file-a-bug-report"></a>建議功能或提出 Bug 報告

若要提出建議和提出 Bug 報告，請[開立問題](https://github.com/aspnet/AspNetCore/issues/new)。 如需一般說明並從社群取得答案，請加入 [Gitter](https://gitter.im/aspnet/Blazor) 上的交談。

## <a name="additional-resources"></a>其他資源

* [WebAssembly](http://webassembly.org/)
* [C# 指南](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
