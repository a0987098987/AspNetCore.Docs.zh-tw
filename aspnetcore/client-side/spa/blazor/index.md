---
title: Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是使用 .NET 建置互動式用戶端應用程式的全新方式，透過此方式建置的應用程式可在具有 WebAssembly 的瀏覽器中執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: spa/blazor/index
---
# <a name="introduction-to-blazor"></a>Blazor 簡介

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor 是單一頁面應用程式架構，適用於建置使用 .NET 的互動式用戶端 Web 應用程式。 Blazor 使用開放式 Web 標準，而不需要讓和外掛程式或程式碼轉譯。 Blazor 適用於所有現代化網頁瀏覽器，包括行動瀏覽器。

在瀏覽器中使用 .NET 進行用戶端 Web 開發提供許多優點：

* **C# 語言**：以 C# 撰寫而不是 JavaScript。
* **.NET 生態系統**：利用現有的 .NET 程式庫生態系統。
* **完整堆疊開發**：共用伺服器和用戶端邏輯。
* **速度與延展性**：.NET 專為效能、可靠性和安全性設計。
* **領先業界的工具**：使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。
* **穩定性與一致性**：以平易近人的語言、架構和工具建置，穩定、功能豐富且容易使用。

在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](http://webassembly.org) (縮寫為 *wasm*) 達成。 WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。 WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。

WebAssembly 程式碼可以透過 JavaScript Interop 存取瀏覽器的完整功能。 在此同時，透過 WebAssembly 執行的 .NET 程式碼會在與 JavaScript 相同的受信任沙箱中執行，以防止對用戶端電腦執行惡意的動作。

![Blazor 在具有 WebAssembly 的瀏覽器中執行 .NET 程式碼。](index/_static/blazor.png)

當建置 Blazor 應用程式並在瀏覽器中執行時：

* C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。
* 組件和 .NET 執行階段會下載至瀏覽器中。
* Blazor 啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。 Blazor 執行階段會透過 JavaScript 互通，處理文件物件模型 (DOM) 操作和瀏覽器 API 呼叫。

Blazor 支援大部分應用程式所需的核心功能，包括：

* 參數
* 事件處理
* 資料繫結
* 路由
* 相依性插入
* 版面配置
* 範本化
* 階層式值

透過[中繼語言 (IL) 連接器](xref:host-and-deploy/razor-components/configure-linker)發行時刪除應用程式未使用的程式碼，減少下載之應用程式的大小。

Blazor 是 Razor 元件的用戶端裝載模型。 因為 Razor 元件解除了元件轉譯邏輯與 UI 更新套用方式的聯結，所以裝載 Razor 元件的方式具有彈性。 使用 ASP.NET Core Razor 元件，在 ASP.NET Core 應用程式裡將 Razor 元件裝載在伺服器上，使得透過 SignalR 連線處理所有的 UI 更新。 如需詳細資訊，請參閱<xref:razor-components/hosting-models#server-side-hosting-model>。 

## <a name="components"></a>元件

*Razor 元件*是一種 UI，例如頁面、對話方塊或資料輸入表單。 元件會處理使用者事件，並定義彈性的 UI 轉譯邏輯。 元件可以為巢狀，且可重複使用。

元件是內建在 .NET 組建內的 .NET 類別，能夠以 NuGet 套件的方式共用及散發。 此類別可使用 Razor 標記頁面 (*.cshtml*) 或 C# 類別 (*.cs*) 的形式撰寫。

[Razor](xref:mvc/views/razor) 是結合 HTML 標記與 C# 程式碼的語法。 Razor 專為開發人員生產力而設計，讓開發人員能在同一個檔案中，使用 [IntelliSense](/visualstudio/ide/using-intellisense) 支援切換標記和 C#。 Razor Pages 和 MVC 檢視也會使用 Razor。 不同於 Razor Pages 和 MVC 檢視，它們是圍繞在要求/回應模型而建置，元件則是專門用來處理 UI 組合。 Razor 元件可專門用來處理用戶端 UI 邏輯和組合。

下列標記是 Razor 檔案 (*DialogComponent.cshtml*) 中自訂對話方塊元件的範例：

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

元件會轉譯至瀏覽器 DOM 的記憶體內表示法，稱為「轉譯樹」，接著能夠以彈性有效率的方式更新 UI。

## <a name="javascript-interop"></a>JavaScript Interop

對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，Blazor 能夠和 JavaScript 交互操作。 元件可以使用 JavaScript 可以使用的任何程式庫或 API。 C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。 如需詳細資訊，請參閱 [JavaScript Interop](xref:razor-components/javascript-interop)。

## <a name="code-sharing-and-net-standard"></a>程式碼共用和 .NET Standard

應用程式可以參考並使用現有的 [.NET Standard](/dotnet/standard/net-standard) 程式庫。 .NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。 支援 .NET Standard 2.0 或更新版本。 網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端、執行緒作業和其他功能) 會擲回 <xref:System.PlatformNotSupportedException>。 .NET Standard 類別庫可以跨伺服端程式碼共用，以及在以瀏覽器為基礎的應用程式中共用。

## <a name="optimization"></a>最佳化

承載大小是應用程式使用性的重要效能因素。 Blazor 對承載大小進行最佳化，以縮短下載時間：

* 未使用的 .NET 組件部分會在建置程序期間移除。
* HTTP 回應會進行壓縮。
* .NET 執行階段與組件會在瀏覽器中進行快取。

[伺服器端 Razor 元件](xref:razor-components/index)藉由在伺服器端維護 .NET 組件、應用程式的組件和執行階段，提供比 Blazor 更小的承載大小。 Razor 元件應用程式只會提供標記檔案和靜態資產給用戶端。

## <a name="additional-resources"></a>其他資源

* <xref:razor-components/index>
* [WebAssembly](http://webassembly.org/)
* [C# 指南](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
