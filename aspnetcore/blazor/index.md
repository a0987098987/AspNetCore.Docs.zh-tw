---
title: ASP.NET Core 中的 Blazor 簡介
author: guardrex
description: 探索 ASP.NET Core Blazor，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 07/01/2019
uid: blazor/index
ms.openlocfilehash: 69a82bebdb787003e36568ca03e1104b9f2edf15
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412396"
---
# <a name="introduction-to-blazor"></a>Blazor 簡介

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

*歡迎使用 Blazor！*

Blazor 是一個可使用 .NET 來建置互動式用戶端 Web UI 的架構：

* 使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。
* 共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。
* 將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。

使用 .NET 進行用戶端網頁程式開發可提供下列優點：

* 以 C# 撰寫而不是 JavaScript。
* 利用 .NET 程式庫的現有 .NET 生態系統。
* 跨伺服器和用戶端共用應用程式邏輯。
* 從 .NET 的效能、可靠性和安全性中獲益。
* 使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。
* 以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。

## <a name="components"></a>元件

Blazor 應用程式的基礎為「元件」  。 Blazor 中的元件為 UI 的元素，例如頁面、對話方塊或資料輸入表單。

元件是內建在 .NET 組件中且具有下列功能的 .NET 類別：

* 定義彈性 UI 轉譯邏輯。
* 處理使用者動作。
* 可以為巢狀結構，且可重複使用。
* 能以 [Razor 類別庫](xref:razor-pages/ui-class)或 [NuGet 套件](/nuget/what-is-nuget)方式共用及散發。

該元件類別通常使用副檔名為 *.razor* 的 [Razor](xref:mvc/views/razor) 標記頁面形式撰寫而成。 Blazor 中的元件正式稱為「Razor 元件」  。 Razor 是結合了 HTML 標記與 C# 程式碼的語法，專為開發人員的生產力而設計。 Razor 可讓您在同一個具有 [IntelliSense](/visualstudio/ide/using-intellisense) 支援的檔案中，於 HTML 標記和 C# 之間切換。 Razor Pages 和 MVC 也會使用 Razor。 不同於 Razor Pages 和 MVC，它們是圍繞著要求/回應模型而建置的，元件則是專門用來處理用戶端 UI 邏輯與組合。

下列 Razor 標記示範一個元件 (*Dialog.razor*)，此元件可巢狀於另一個元件內：

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

對話方塊的內文內容 (`ChildContent`) 和標題 (`Title`) 均由要在 UI 中使用此元件的元件所提供。 `OnYes` 是按鈕的 `onclick` 事件所觸發的 C# 方法。

Blazor 會針對 UI 組合使用自然的 HTML 標記。 HTML 元素會指定元件，而標記的屬性 (Attribute) 會將值傳遞至元件的屬性 (Property)。

在下列範例中， `Index` 元件使用 `Dialog` 元件。 `ChildContent` 與 `Title` 是由 `<Dialog>` 元素的屬性與內容設定的。

*Index.razor*：

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

當您於瀏覽器中存取父代 (*Index.razor*) 時，即會轉譯此對話方塊：

![瀏覽器中轉譯的對話方塊元件](index/_static/dialog.png)

在應用程式中使用此元件時，[Visual Studio](/visualstudio/ide/using-intellisense) 和 [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) \(英文\) 中的 IntelliSense 會藉由完成語法和參數來加快開發速度。

元件會轉譯為瀏覽器文件物件模型 (DOM) 的記憶體內部表示法 (稱為「轉譯樹」  )，可用來以彈性且有效率的方式更新 UI。

## <a name="blazor-client-side"></a>Blazor 用戶端

Blazor 用戶端是單一頁面應用程式架構，可使用 .NET 來建置互動式用戶端 Web 應用程式。 Blazor 用戶端會使用開放式 Web 標準，而不需外掛程式或程式碼轉譯，且適用於包括行動瀏覽器在內的所有新式網頁瀏覽器。

在網頁瀏覽器內執行 .NET 程式碼已可藉由 [WebAssembly](https://webassembly.org) (縮寫為 *wasm*) 達成。 WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。 WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。

WebAssembly 程式碼可以透過 JavaScript 存取瀏覽器的完整功能，稱為 「JavaScript 互通性」  (或 *JavaScript Interop*)。 在瀏覽器中透過 WebAssembly 執行的 .NET 程式碼會在瀏覽器的 JavaScript 沙箱執行，且受沙箱所提供對用戶端電腦之惡意動作的保護。

![Blazor 用戶端會透過 WebAssembly 在瀏覽器中執行 .NET 程式碼。](index/_static/blazor-client-side.png)

當 Blazor 用戶端應用程式建置好並在瀏覽器中執行時：

* C# 程式碼檔案和 Razor 檔案會編譯成 .NET 組件。
* 組件和 .NET 執行階段會下載至瀏覽器中。
* Blazor 用戶端會啟動 .NET 執行階段，並設定執行階段以載入應用程式的組件。 Blazor 用戶端執行階段會使用 JavaScript Interop 來處理 DOM 操作和瀏覽器 API 呼叫。

發佈的應用程式大小 (它的「承載大小」  ) 是應用程式使用性的重要效能因素。 大型應用程式需要相對較長的時間才能下載至瀏覽器，這點會對使用者體驗造成傷害。 Blazor 會將承載調整成最佳大小，以縮短下載時間：

* 若應用程式是透過[中繼語言 (IL) 連接器](xref:host-and-deploy/blazor/configure-linker)所發佈的，則會移除未使用的程式碼。
* HTTP 回應會進行壓縮。
* .NET 執行階段與組件會在瀏覽器中進行快取。

## <a name="blazor-server-side"></a>Blazor 伺服器端

Blazor 會將元件轉譯邏輯與套用 UI 更新的方式分隔。 Blazor 伺服器端提供從 ASP.NET Core 應用程式於伺服器上裝載 Razor 元件的支援。 UI 更新會透過 [SignalR](xref:signalr/introduction) 連線來處理。

執行階段會處理將 UI 事件從瀏覽器傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。

Blazor 伺服器端用於與瀏覽器通訊的連線也會用於處理 JavaScript Interop 呼叫。

![Blazor 伺服器端在伺服器上執行 .NET 程式碼，並經由 SignalR 連線與用戶端上的文件物件模型互動](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a>JavaScript Interop

對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 存取的應用程式，元件能夠和 JavaScript 交互操作。 元件可以使用 JavaScript 可以使用的任何程式庫或 API。 C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。 如需詳細資訊，請參閱 <xref:blazor/javascript-interop>。

## <a name="code-sharing-and-net-standard"></a>程式碼共用和 .NET Standard

Blazor 會實作 [.NET Standard 2.0](/dotnet/standard/net-standard)。 .NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。 .NET Standard 類別庫可與不同的 .NET 平台共用，例如 Blazor、.NET Framework、.NET Core、Xamarin、Mono 和 Unity。

網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端與執行緒) 均會擲回 <xref:System.PlatformNotSupportedException>。

## <a name="additional-resources"></a>其他資源

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [C# 指南](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
