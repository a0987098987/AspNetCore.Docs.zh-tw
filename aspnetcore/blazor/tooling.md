---
title: 適用于 ASP.NET Core 的工具Blazor
author: guardrex
description: 瞭解可用於建立 Blazor 應用程式的工具。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/tooling
ms.openlocfilehash: 3ff610225c798624b7ead7d365924ae3d193829d
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944941"
---
<!-- zone_pivot_groups: operating-systems -->

# <a name="tooling-for-aspnet-core-blazor"></a>適用于 ASP.NET Core 的工具Blazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

選取適用于您平臺的工具：

* Windows：選取 [ **Visual Studio** ] 索引標籤。
* Linux：選取 [ **Visual Studio Code** ] 索引標籤。
* macOS：選取 [ **Visual Studio for Mac** ] 索引標籤。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 使用**ASP.NET 和 網頁程式開發**工作負載安裝最新版的[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 。

1. 建立新專案。

1. 選取 [ ** Blazor 應用程式**]。 選取 [下一步]。

1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 確認 [**位置**] 專案正確，或提供專案的 [位置]。 選取 [建立]。

1. 如需 Blazor WebAssembly 體驗，請選擇** Blazor WebAssembly 應用程式**範本。 如需 Blazor Server 體驗，請選擇** Blazor Server 應用程式**範本。 選取 [建立]。

   如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。

1. 按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 安裝最新版的[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)。 如果您先前已安裝 SDK，您可以在命令 shell 中執行下列命令，以判斷已安裝的版本：

   ```dotnetcli
   dotnet --version
   ```

1. 安裝最新版的[Visual Studio Code](https://code.visualstudio.com/)。

1. 安裝適用于[Visual Studio Code 擴充](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)功能的最新 c # 和[JavaScript 偵錯工具（夜間）](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)延伸模組，並 `debug.javascript.usePreview` 將設定為 `true` 。

   若要將設定 `debug.javascript.usePreview` 為 `true` 使用 VS Code UI， **File**請開啟 [檔案  >  **喜好**  >  **設定**]，然後搜尋 `debug javascript use preview` 。 選取 [**使用適用于 Node.js 和 Chrome 的新的預覽 JavaScript 偵錯工具**] 核取方塊。

1. 如需 Blazor WebAssembly 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   如需 Blazor Server 體驗，請在命令 shell 中執行下列命令：

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。

1. 在 Visual Studio Code 中開啟 `WebApplication1` 資料夾。

1. IDE 會要求您新增資產，以建立和對專案進行偵錯工具。 選取 [是]。

1. 按<kbd>Ctrl</kbd> + <kbd>F5</kbd>執行應用程式。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 安裝[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)。

1. **File**  >  在 [**開始] 視窗**中選取 [檔案] [**新增方案**] 或 [建立**新**專案]。

1. 在提要欄位中，選取 [ **Web 和主控台**  >  **應用程式**]。

   如需 Blazor WebAssembly 體驗，請選擇** Blazor WebAssembly 應用程式**範本。 如需 Blazor Server 體驗，請選擇** Blazor Server 應用程式**範本。 選取 [下一步]。

   如需這兩個 Blazor 裝載模型的詳細資訊，請 *Blazor WebAssembly* *Blazor Server* 參閱 <xref:blazor/hosting-models> 。

1. 確認下列設定：

   * 設定為 **.Net Core 3.1**的**目標 Framework** 。
   * **驗證**設為 [**無驗證**]。
   
   選取 [下一步]。

1. 在 [**專案名稱**] 欄位中，將應用程式命名為 `WebApplication1` 。 選取 [建立]。

1. 選取 [**執行**  >  **而不進行調試**程式]，以*在沒有偵錯工具的情況下*執行 app。 使用 [**執行**  >  **開始調試**程式] 或 [執行] （&#9654;）按鈕執行應用程式，以*使用調試*程式執行應用程式。

如果出現會信任開發憑證的提示，請信任憑證並繼續。 需要使用者和 keychain 密碼，才能信任憑證。

---

<!--

::: zone pivot="os-windows"

1. Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.

1. Create a new project.

1. Select **Blazor App**. Select **Next**.

1. Provide a project name in the **Project name** field or accept the default project name. Confirm the **Location** entry is correct or provide a location for the project. Select **Create**.

1. For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. Select **Create**.

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.

::: zone-end

::: zone pivot="os-linux"

1. Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1). If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:

   ```dotnetcli
   dotnet --version
   ```

1. Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).

1. Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.

   To set `debug.javascript.usePreview` to `true` using the VS Code UI, open **File** > **Preferences** > **Settings** and search for `debug javascript use preview`. Select the check box for **Use the new in-preview JavaScript debugger for Node.js and Chrome**.

1. For a Blazor WebAssembly experience, execute the following command in a command shell:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   For a Blazor Server experience, execute the following command in a command shell:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Open the `WebApplication1` folder in Visual Studio Code.

1. The IDE requests that you add assets to build and debug the project. Select **Yes**.

1. Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.

::: zone-end

::: zone pivot="os-macos"

1. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).

1. Select **File** > **New Solution** or create a **New** project from the **Start Window**.

1. In the sidebar, select **Web and Console** > **App**.

   For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. Select **Next**.

   For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.

1. Confirm the following configurations:

   * **Target Framework** set to **.NET Core 3.1**.
   * **Authentication** set to **No Authentication**.
   
   Select **Next**.

1. In the **Project Name** field, name the app `WebApplication1`. Select **Create**.

1. Select **Run** > **Start Without Debugging** to run the app *without the debugger*. Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.

If a prompt appears to trust the development certificate, trust the certificate and continue. The user and keychain passwords are required to trust the certificate.

::: zone-end

-->
