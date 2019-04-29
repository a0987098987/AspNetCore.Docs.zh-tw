---
title: ASP.NET Core 中的 Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982993"
---
# <a name="introduction-to-blazor"></a>Blazor 簡介

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

歡迎使用 Blazor！

使用.NET 建立互動式用戶端 Web UI：

* 使用 C# 而不是 JavaScript 建置豐富的互動式 UI。
* 共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。
* 將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。

Blazor 支援大部分應用程式所需的核心案例：

* 參數
* 事件處理
* 資料繫結
* 路由
* 相依性插入
* 版面配置
* 範本
* 階層式值

## <a name="components"></a>元件

Blazor 中的「元件」是一種 UI 元素，例如頁面、對話方塊或資料輸入表單。 元件會處理使用者事件，並定義彈性的 UI 轉譯邏輯。 元件可以為巢狀，且可重複使用。

元件是內建在 .NET 組建內的 .NET 類別，能夠以 NuGet 套件的方式共用及散發。 該元件類別通常使用副檔名為 *.razor* 的 Razor 標記頁面形式撰寫而成。 Blazor 中的元件有時也稱為 Razor 元件。

[Razor](xref:mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。 Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。 Razor Pages 和 MVC 檢視也會使用 Razor。 不同於 Razor Pages 和 MVC 檢視，它們是圍繞在要求/回應模型而建置，元件則是專門用來處理 UI 組合。 Razor 元件可專門用來處理用戶端 UI 邏輯和組合。

下列標記是自訂對話方塊元件的範例：

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

在應用程式的其他地方使用此元件時，[Visual Studio](https://visualstudio.microsoft.com/vs/) 中的 IntelliSense 會藉由語法和參數完成加快開發速度。

元件會轉譯至瀏覽器 DOM 的記憶體內表示法，稱為「轉譯樹」，接著能夠以彈性有效率的方式更新 UI。

## <a name="blazor-server-side"></a>Blazor 伺服器端

Blazor 會將元件轉譯邏輯與套用 UI 更新的方式分隔。 Blazor 伺服器端提供從 ASP.NET Core 應用程式於伺服器上裝載 Razor 元件的支援。 UI 更新透過 SignalR 連線處理。

執行階段：

* 處理從瀏覽器傳送到伺服器的 UI 事件。
* 在執行元件後，伺服器會將套用 UI 更新傳回瀏覽器。

Blazor 伺服器端用於與瀏覽器通訊的連線也會用於處理 JavaScript Interop 呼叫。

![Blazor 伺服器端在伺服器上執行 .NET 程式碼，並經由 SignalR 連線與用戶端上的文件物件模型互動](index/_static/blazor-server-side.png)

如需詳細資訊，請參閱<xref:blazor/hosting-models#server-side>。

## <a name="blazor-client-side"></a>Blazor 用戶端

Blazor 用戶端是單一頁面應用程式架構，用於使用 .NET 建置互動式用戶端 Web 應用程式。 Blazor 用戶端使用開放式 Web 標準，不需要轉譯外掛程式或程式碼。 Blazor 用戶端適用於所有新式網頁瀏覽器，包括行動瀏覽器。

在瀏覽器中使用 .NET 進行用戶端 Web 開發提供許多優點：

* **C# 語言**：以 C# 撰寫而不是 JavaScript。
* **.NET 生態系統**：利用現有的 .NET 程式庫生態系統。
* **完整堆疊開發**：共用伺服器和用戶端邏輯。
* **速度與延展性**：.NET 專為效能、可靠性和安全性設計。
* **領先業界的工具**：使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。
* **穩定性與一致性**：以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。

在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](http://webassembly.org) (縮寫為 *wasm*) 達成。 WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。 WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。

WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。 在此同時，透過 WebAssembly 執行的 .NET 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。

![Blazor 用戶端會透過 WebAssembly 在瀏覽器中執行 .NET 程式碼。](index/_static/blazor-client-side.png)

當 Blazor 用戶端應用程式建置好並在瀏覽器中執行時：

* C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。
* 組件和 .NET 執行階段會下載至瀏覽器中。
* Blazor 用戶端會啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。 Blazor 執行階段會透過 JavaScript Interop 處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。

為了減少下載之應用程式的大小，透過[中繼語言 (IL) 連接器](xref:host-and-deploy/blazor/configure-linker)發行時會刪除應用程式未使用的程式碼。

Blazor 用戶端是用戶端裝載模型。 因為 Blazor 解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Blazor 的方式具有彈性。 使用 [Blazor 伺服器端](#blazor-server-side)可從 ASP.NET Core 應用程式在伺服器上裝載 Blazor，以透過 SignalR 連線處理 UI 更新。 如需詳細資訊，請參閱<xref:blazor/hosting-models#server-side>。 

承載大小是應用程式使用性的重要效能因素。 Blazor 會將承載調整成最佳大小，以縮短下載時間：

* 未使用的 .NET 組件部分會在建置程序期間移除。
* HTTP 回應會進行壓縮。
* .NET 執行階段與組件會在瀏覽器中進行快取。

[Blazor 伺服器端](#blazor-server-side)藉由維護 .NET 組件、應用程式的組件以及執行階段伺服器端，可提供較 Blazor 用戶端小的承載大小。 Blazor 伺服器端應用程式只提供標記檔案和靜態資產給用戶端。

## <a name="javascript-interop"></a>JavaScript Interop

對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，元件能夠和 JavaScript 交互操作。 元件可以使用 JavaScript 可以使用的任何程式庫或 API。 C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。 如需詳細資訊，請參閱 [JavaScript Interop](xref:blazor/javascript-interop)。

## <a name="code-sharing-and-net-standard"></a>程式碼共用和 .NET Standard

應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。 .NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。 Blazor 實作 .NET Standard 2.0。 網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 <xref:System.PlatformNotSupportedException>。 .NET Standard 類別庫可與不同的 .NET 平台共用，例如 Blazor、.NET Framework、.NET Core、Xamarin、Mono 和 Unity。

## <a name="additional-resources"></a>其他資源

* [WebAssembly](http://webassembly.org/)
* [C# 指南](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
