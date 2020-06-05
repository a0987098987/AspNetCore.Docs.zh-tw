---
title: 開始使用 ASP.NET CoreBlazor
author: guardrex
description: 開始使用， Blazor 方法是 Blazor 使用您選擇的工具來建立應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 08229283882928c4cc733de19840d25872846c97
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84452027"
---
# <a name="get-started-with-aspnet-core-blazor"></a>開始使用 ASP.NET CoreBlazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

若要開始使用 Blazor ，請遵循您所選工具的指導方針：

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。

1. 建立新專案。

1. 選取 [ ** Blazor 應用程式**]。 選取 [下一步]。

1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]。

1. 如需 Blazor WebAssembly 體驗，請選擇 [ ** Blazor WebAssembly 應用程式**] 範本。 如需 Blazor 伺服器體驗，請選擇 [ ** Blazor 伺服器應用程式**] 範本。 選取 [建立]。

   如需這兩個 Blazor 裝載模型（ * Blazor WebAssembly*和* Blazor 伺服器*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。

1. 按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：

   ```dotnetcli
   dotnet --version
   ```

1. 安裝最新版的[Visual Studio Code](https://code.visualstudio.com/)。

1. 安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並 `debug.javascript.usePreview` 將設定為 `true` 。

  若要將設定 `debug.javascript.usePreview` 為 `true` 使用 VS Code UI， **File**請開啟 [檔案  >  **喜好**  >  **設定**]，然後搜尋 `debug javascript use preview` 。 選取 [**使用適用于 node.js 的新預覽版 JavaScript 偵錯工具] 和 [Chrome**] 的核取方塊。

1. 如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   如需這兩個 Blazor 裝載模型（ * Blazor WebAssembly*和* Blazor 伺服器*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。

1. 在 Visual Studio Code 中開啟 [ *WebApplication1* ] 資料夾。

1. IDE 會要求您新增資產，以建立和對專案進行偵錯工具。 選取 [是]。

1. 按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。

1. **File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。

1. 在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。

   如需 Blazor WebAssembly 體驗，請選擇 [ ** Blazor WebAssembly 應用程式**] 範本。 如需 Blazor 伺服器體驗，請選擇 [ ** Blazor 伺服器應用程式**] 範本。 選取 [下一步]。

   如需這兩個 Blazor 裝載模型（ * Blazor WebAssembly*和* Blazor 伺服器*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。

1. 確認下列設定：

   * 設定為 **.Net Core 3.1**的**目標 Framework** 。
   * **驗證**設為 [**無驗證**]。
   
   選取 [下一步]。

1. 在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1` 。 選取 [建立]。

1. 選取 [**執行**  >  **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。 使用 [**執行**  >  **開始調試**程式] 或 [執行] （&#9654;）按鈕執行應用程式，以*使用調試*程式執行應用程式。

如果出現會信任開發憑證的提示，請信任憑證並繼續。 需要使用者和 keychain 密碼，才能信任憑證。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：

   ```dotnetcli
   dotnet --version
   ```

1. 如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   如需 Blazor 伺服器體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   如需這兩個 Blazor 裝載模型（ * Blazor WebAssembly*和* Blazor 伺服器*）的詳細資訊，請參閱 <xref:blazor/hosting-models> 。

1. 在瀏覽器中，巡覽至 `https://localhost:5001`。

---

提要欄位中的索引標籤可使用多個頁面：

* 家庭
* 計數器
* 提取資料

在 [計數器] 頁面上，選取 [按我]**** 按鈕以在不重新整理頁面的情況下讓計數器遞增。 將網頁中的計數器遞增通常需要撰寫 JavaScript，但 Blazor 您可以使用 c #。

*Pages/Counter.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

`/counter`在瀏覽器中的要求（如頂端的指示詞所指定） `@page` 會導致 `Counter` 元件轉譯其內容。 元件會轉譯成轉譯樹狀結構的記憶體中標記法，然後用來以彈性且有效率的方式更新 UI。

每次選取 [**按我**] 按鈕時：

* `onclick`引發事件。
* 已呼叫 `IncrementCount` 方法。
* `currentCount`會遞增。
* 元件會再次轉譯。

執行時間會比較新的內容與先前的內容，而且只會將已變更的內容套用至檔物件模型（DOM）。

使用 HTML 語法將元件新增至另一個元件。 例如，藉 `Counter` 由將元素新增至元件，將元件新增至應用程式的首頁 `<Counter />` `Index` 。

*Pages/Index.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

執行應用程式。 首頁有自己的計數器，由元件提供 `Counter` 。

元件參數是使用屬性或[子內容](xref:blazor/components#child-content)所指定，可讓您設定子元件上的屬性。 若要將參數新增至 `Counter` 元件，請更新元件的 `@code` 區塊：

* 使用屬性加入的公用屬性 `IncrementAmount` [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。
* 將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。

*Pages/Counter.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

`IncrementAmount`使用屬性，在 `Index` 元件的 `<Counter>` 元素中指定。

*Pages/Index.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

執行應用程式。 `Index`元件有自己的計數器，每次選取 [按**我**] 按鈕時，就會遞增10。 `Counter`中的元件（*razor*）會 `/counter` 繼續遞增一。

## <a name="next-steps"></a>後續步驟

逐步建立 Blazor 應用程式：

> [!div class="nextstepaction"]
> <xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>其他資源

* <xref:blazor/templates>
* <xref:signalr/introduction>
