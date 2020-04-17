---
title: 開始使用ASP.NET核心Blazor
author: guardrex
description: Blazor使用您選擇的工具建Blazor構應用,開始入門。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 7fe4fbb082f08d4f71684c836a826d8b6dd888f6
ms.sourcegitcommit: 77c046331f3d633d7cc247ba77e58b89e254f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488724"
---
# <a name="get-started-with-aspnet-core-blazor"></a>開始ASP.NET核心布拉佐爾

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

要開始使用 Blazor,請按照您選擇的工具指南操作:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 要創建 Blazor Server 應用,請使用**ASP.NET 和 Web 開發**工作負載安裝最新版本的[Visual Studio 2019。](https://visualstudio.microsoft.com/downloads/)

   要創建 Blazor Server 和 Blazor WebAssembly 應用,請使用**ASP.NET 和網路開發**工作負載安裝[Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)的最新預覽版。

   有關兩個布拉佐託管模型,*布拉佐網路大會*和*布拉佐伺服器*的資訊<xref:blazor/hosting-models>,請參閱 。

1. 透過執行以下指令安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)預覽樣本:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview4.20210.8
   ```

1. 建立新專案。

1. 選擇**布拉佐爾應用程式**。 選取 [下一步]  。

1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 確認 **「位置**」條目正確或為專案提供位置。 選取 [建立]  。

1. 有關 Blazor Web 組裝體驗(可視化工作室 16.6 預覽版 2 或更高版本),請選擇**Blazor Web 組裝應用**範本。 有關 Blazor 伺服器體驗(可視化工作室 16.4 或更高版本),請選擇**Blazor 伺服器應用**範本。 選取 [建立]  。

1. 按<kbd>Ctrl</kbd>+<kbd>F5</kbd>運行應用程式。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 安裝[.NET 核心 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。

1. 通過運行以下命令,可選地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)預覽樣本:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview4.20210.8
   ```

   > [!NOTE]
   > [.NET 核心 SDK 版本 3.1.201 或更高版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)**需要**使用 3.2 預覽 4 Blazor WebAssembly 範本。 通過在命令外殼中運行`dotnet --version`來確認已安裝的 .NET Core SDK 版本。

1. 安裝[視覺化工作室代碼](https://code.visualstudio.com/)。

1. 安裝最新的[C# 視覺化工作室碼延伸](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)與[JavaScript 除錯器(夜間)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸,`debug.javascript.usePreview`設定為`true`。

1. 要獲得 Blazor Server 體驗,請在命令外殼中執行以下命令:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   要獲得 Blazor WebAssembly 體驗,請在命令外殼中執行以下命令:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   有關兩個布拉佐託管模型,*布拉佐伺服器*和*布拉佐網路大會*的資訊<xref:blazor/hosting-models>,請參閱 。

1. 打開視覺化工作室代碼中的*Web 應用程式 1*資料夾。

1. IDE 請求添加資源以生成和調試專案。 選取 [是]  。

1. 使用 Blazor 伺服器,使用可視化工作室代碼調試器運行應用程式。

   使用 Blazor WebAssembly,使用 **.NET 核心啟動 (Blazor 獨立)** 啟動配置啟動應用程式,然後在 Chrome 啟動配置中使用 **.NET 核心調試 Blazor 網路程式集**啟動瀏覽器(需要 Chrome)。 如需詳細資訊，請參閱 <xref:blazor/debug#visual-studio-code>。

1. 在瀏覽器中，巡覽至 `https://localhost:5001`。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Mac 視覺工作室支援 Blazor 伺服器。 布拉佐網路組裝目前不受支援。 要在 macOS 上建構 Blazor WebAssembly 應用,請按照 **.NET 核心 CLI**選項卡上的指南操作。

1. [安裝 Mac 的視覺工作室](https://visualstudio.microsoft.com/vs/mac/)。

1. 選擇**檔案** > **新解決方案**或建立新**專案**。

1. 在側邊列中,選擇 **.NET 核心** > **應用**。

1. 選擇**Blazor 伺服器應用**範本。 選取 [建立]  。

   有關 Blazor 伺服器託管模型的資訊,請<xref:blazor/hosting-models>參閱 。

1. 將**目標框架**設置為 **.NET 核心 3.1,** 然後選擇 **「下一步**」 。

1. 在「**項目名稱」** 欄位中,`WebApplication1`為套用命名 。 選取 [建立]  。

1. 選擇 **「** > **執行而不除錯執行**」 以在沒有*除錯器的情況下*執行應用程式。 使用 **「開始除錯」** 執行應用,*以便使用除錯器*執行應用。

如果出現信任開發證書的提示,請信任證書並繼續。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. 安裝[.NET 核心 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。

1. 通過運行以下命令,可選地安裝[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly)預覽樣本:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview4.20210.8
   ```

   > [!NOTE]
   > [.NET 核心 SDK 版本 3.1.201 或更高版本](https://dotnet.microsoft.com/download/dotnet-core/3.1)**需要**使用 3.2 預覽 4 Blazor WebAssembly 範本。 通過在命令外殼中運行`dotnet --version`來確認已安裝的 .NET Core SDK 版本。

1. 要獲得 Blazor Server 體驗,請在命令外殼中執行以下命令:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   要獲得 Blazor WebAssembly 體驗,請在命令外殼中執行以下命令:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   有關兩個布拉佐託管模型,*布拉佐伺服器*和*布拉佐網路大會*的資訊<xref:blazor/hosting-models>,請參閱 。

1. 在瀏覽器中，巡覽至 `https://localhost:5001`。

---

側邊列中的選項卡提供多個頁面:

* Home
* 計數器
* 取得資料

在 [計數器] 頁面上，選取 [按我]**** 按鈕以在不重新整理頁面的情況下讓計數器遞增。 在網頁中增加計數器通常需要編寫 JavaScript,但Blazor使用 C# 時可以使用 C#。

*Pages/Counter.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

瀏覽器中的請求`/counter`(如頂部`@page`的 指令所指定的`Counter`)會導致 元件呈現其內容。 元件呈現為呈現樹的記憶體中表示形式,然後可用於以靈活、高效的方式更新 UI。

每次選擇 **「按下我」** 按鈕時:

* 事件`onclick`已觸發。
* 已呼叫 `IncrementCount` 方法。
* 遞`currentCount`增。
* 元件將再次呈現。

運行時將新內容與以前的內容進行比較,並且僅將更改的內容應用於文檔物件模型 (DOM)。

使用 HTML 語法將元件新增到另一個元件。 例如,通過將`Counter``<Counter />`元素添加到`Index`元件,將元件添加到應用的主頁。

*Pages/Index.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

執行應用程式。 主頁有其自己的計數器由`Counter`元件提供。

元件參數使用屬性或[子內容](xref:blazor/components#child-content)指定,允許您在子元件上設置屬性。 要加入`Counter`元件加入參數,請更新元件的`@code`區塊:

* 使用`[Parameter]`屬性添加`IncrementAmount`公共屬性。
* 將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。

*Pages/Counter.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

使用屬性`IncrementAmount``Index`在元件`<Counter>`的元素中指定 。

*Pages/Index.razor*：

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

執行應用程式。 元件`Index`有自己的計數器,每次選擇 **「按下我」** 按鈕時,該計數器都會增加 10。 在`Counter``/counter`元件 (*反.razor*) 繼續增加一個.

## <a name="next-steps"></a>後續步驟

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>其他資源

* <xref:blazor/templates>
* <xref:signalr/introduction>
