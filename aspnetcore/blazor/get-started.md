---
title: 開始使用 ASP.NET Core Blazor
author: guardrex
description: 藉由使用您選擇的工具來建立 Blazor 應用程式，開始使用 Blazor。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306448"
---
# <a name="get-started-with-aspnet-core-blazor"></a>開始使用 ASP.NET Core Blazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

若要開始使用 Blazor，請遵循您所選工具的指導方針：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 若要建立 Blazor 伺服器應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝[Visual Studio 2019 16.4 版或更新](https://visualstudio.microsoft.com/vs/preview/)版本。

   若要建立 Blazor 伺服器和 Blazor WebAssembly 應用程式，請使用**ASP.NET 和 網頁程式開發**工作負載安裝 Visual Studio 2019 16.6 Preview 2 或更新版本。

   如需*Blazor WebAssembly*和*Blazor Server*這兩個 Blazor 裝載模型的相關資訊，請參閱 <xref:blazor/hosting-models>。

1. 建立新專案。

1. 選取 [ **Blazor 應用程式**]。 選取 [下一步]。

1. 在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]。

1. 如需 Blazor WebAssembly 體驗（Visual Studio 16.6 Preview 2 或更新版本），請選擇 [ **Blazor WebAssembly 應用程式**] 範本。 如需 Blazor 伺服器體驗（Visual Studio 16.4 或更新版本），請選擇 [ **Blazor 伺服器應用程式**] 範本。 選取 [建立]。

1. 按下 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 即可執行應用程式。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。

1. 藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > 使用 3.2 Preview 3 Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 藉由在命令 shell 中執行 `dotnet --version`，確認已安裝的 .NET Core SDK 版本。

1. 安裝 [Visual Studio Code](https://code.visualstudio.com/)。

1. 安裝最新[ C#的 for Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並將 `debug.javascript.usePreview` 設定為 `true`。

1. 如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。

1. 在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。

1. IDE 會要求您新增資產，以建立和對專案進行偵錯工具。 選取 [是]。

1. 使用 Visual Studio Code 偵錯工具執行應用程式。

1. 在瀏覽器中，巡覽至 `https://localhost:5001`。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Visual Studio for Mac 支援 Blazor 伺服器。 目前不支援 Blazor WebAssembly。 若要在 macOS 上建立 Blazor WebAssembly 應用程式，請遵循 [ **.NET Core CLI** ] 索引標籤上的指導方針。

1. 安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。

1. 選取 **[** 檔案] > [**新增方案**] 或 [建立**新專案**]。

1. 在側邊欄中，選取 [ **.Net Core** > **應用程式**]。

1. 選取 [ **Blazor 伺服器應用程式**] 範本。 選取 [建立]。

   如需 Blazor 伺服器裝載模型的詳細資訊，請參閱 <xref:blazor/hosting-models>。

1. 將**目標 Framework**設定為 **.net Core 3.1** ，然後選取 **[下一步]** 。

1. 在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1`。 選取 [建立]。

1. 選取 [**執行**] > **執行而不進行調試**程式，以在不進行偵錯工具的*情況下*執行 使用 [**開始調試**程式] 執行應用程式，以*使用調試*程式執行應用程式。

如果出現會信任開發憑證的提示，請信任憑證並繼續。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 安裝[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。

1. 藉由執行下列命令，選擇性地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview 範本：

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > 使用 3.2 Preview 3 Blazor WebAssembly 範本時，**需要** [.NET Core SDK 版本3.1.201 或更新版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 藉由在命令 shell 中執行 `dotnet --version`，確認已安裝的 .NET Core SDK 版本。

1. 如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   如需 Blazor 的 WebAssembly 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   如需這兩個 Blazor 裝載模型、 *Blazor 伺服器*和*Blazor WebAssembly*的相關資訊，請參閱 <xref:blazor/hosting-models>。

1. 在瀏覽器中，巡覽至 `https://localhost:5001`。

---

提要欄位中的索引標籤可使用多個頁面：

* Home
* 計數器
* 提取資料

在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。 將網頁中的計數器遞增通常需要撰寫 JavaScript，但您可以使用C#Blazor。

*Pages/Counter.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

在瀏覽器中 `/counter` 的要求，如同頂端的 `@page` 指示詞所指定，會導致 `Counter` 元件轉譯其內容。 元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。

每次選取 [**按我**] 按鈕時：

* `onclick` 事件會引發。
* 呼叫 `IncrementCount` 方法。
* `currentCount` 會遞增。
* 元件會再次轉譯。

執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。

使用 HTML 語法將元件新增至另一個元件。 例如，藉由將 `<Counter />` 元素新增至 `Index` 元件，將 `Counter` 元件新增至應用程式的首頁。

*Pages/Index.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

執行應用程式。 首頁有自己的計數器，由 `Counter` 元件提供。

元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。 若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：

* 加入具有 `[Parameter]` 屬性之 `IncrementAmount` 的公用屬性。
* 將 `IncrementCount` 方法變更為在增加 `IncrementAmount`的值時使用 `currentCount`。

*Pages/Counter.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定 `IncrementAmount`。

*Pages/Index.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

執行應用程式。 `Index` 元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。 `/counter` 的 `Counter` 元件（*razor*）會繼續遞增一。

## <a name="next-steps"></a>後續步驟

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>其他資源

* <xref:blazor/templates>
* <xref:signalr/introduction>
