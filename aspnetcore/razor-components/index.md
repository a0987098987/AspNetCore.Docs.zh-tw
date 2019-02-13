---
title: Razor 元件簡介
author: guardrex
description: 探索 ASP.NET Core Razor 元件，這是在 ASP.NET Core 應用程式中使用 .NET 建置互動式用戶端 Web UI 的方式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: razor-components/index
ms.openlocfilehash: 04a73d33cee0deedaf3dc97395836a936b580fbd
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159522"
---
# <a name="introduction-to-razor-components"></a>Razor 元件簡介

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

*Razor 元件*是使用 .NET 建置互動式用戶端 Web UI 的一種方式：

* 使用 C# 而不是 JavaScript 建置豐富的互動式 UI。
* 共用伺服器端與用戶端應用程式邏輯都是以 .NET 撰寫。
* 將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。

Razor 元件支援大部分應用程式所需的核心功能，包括：

* 參數
* 事件處理
* 資料繫結
* 路由
* 相依性插入
* 版面配置
* 範本化
* 階層式值

Razor 元件將元件轉譯邏輯從 UI 套用更新的方式分隔。 .NET Core 3.0 中的 ASP.NET Core Razor 元件在 ASP.NET Core 應用程式裡新增支援以將 Razor 元件裝載在伺服器上。 UI 更新透過 SignalR 連線處理。

執行階段：

* 處理從瀏覽器傳送到伺服器的 UI 事件。
* 在執行元件後，伺服器會將套用 UI 更新傳回瀏覽器。

Razor 元件與瀏覽器通訊所使用的連線也會用來處理 JavaScript interop 呼叫。

![Razor 元件在伺服器上執行 .NET 程式碼，並透過 SignalR 連線與用戶端上的文件物件模型互動](index/_static/aspnet-core-razor-components.png)

如需詳細資訊，請參閱<xref:razor-components/hosting-models#server-side-hosting-model>。

*Blazor* 是實驗性的 Razor 元件用戶端裝載模型。 Blazor 會在使用開放式 Web 標準，且不含任何外掛程式、任何程式碼轉譯的瀏覽器中，在 .NET 上執行。 如需詳細資訊，請參閱<xref:razor-components/hosting-models#client-side-hosting-model>。

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

對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 的應用程式，元件能夠和 JavaScript 交互操作。 元件可以使用 JavaScript 可以使用的任何程式庫或 API。 C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。 如需詳細資訊，請參閱 [JavaScript Interop](xref:razor-components/javascript-interop)。

## <a name="additional-resources"></a>其他資源

* <xref:spa/blazor/index>
* [WebAssembly](http://webassembly.org/)
* [C# 指南](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
